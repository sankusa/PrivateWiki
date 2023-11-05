- [Objectのドラッグ&ドロップを取得](#objectのドラッグドロップを取得)
- [Objectのロード(1件、全件)](#objectのロード1件全件)
- [Objectがアセットか判定する](#objectがアセットか判定する)  
- [便利なサイト](#便利なサイト)
- [ScriptableObjectをスクリプトから生成](#scriptableobjectをスクリプトから生成)

***

## Objectのドラッグ&ドロップを取得
```
public static T GetObject<T>(Rect dropRect) where T : Object
{
    return GetObjects(dropRect)?.OfType<T>().FirstOrDefault();
}

public static List<T> GetObjects<T>(Rect dropRect) where T : Object
{
    return GetObjects(dropRect)?.OfType<T>().ToList();
}

private static Object[] GetObjects(Rect dropRect)
{
    // カーソルが範囲外ならスルー
    if(!dropRect.Contains(Event.current.mousePosition)) return null;

    // カーソルの見た目をドラッグ用に変更
    DragAndDrop.visualMode = DragAndDropVisualMode.Generic;

    // ドロップでなければ終了
    if(Event.current.type != EventType.DragPerform) return null;

    // ドロップを受け入れる
    DragAndDrop.AcceptDrag();

    // イベントを使用済みに
    Event.current.Use();

    return DragAndDrop.objectReferences;
}
```

***

## Objectのロード(1件、全件)
```
private static T LoadAsset<T> () where T : Object {
    string[] guids = AssetDatabase.FindAssets("t:" + typeof(T).Name);

    if(guids.Length == 0) return default(T);

    string path = AssetDatabase.GUIDToAssetPath(guids[0]);
    T asset = (T)AssetDatabase.LoadAssetAtPath(path, typeof(T));

    return asset;
}

public static List<T> LoadAllAssets<T> () where T : Object {
    List<T> list = new List<T>(); 
    
    string[] guids = AssetDatabase.FindAssets("t:" + typeof(T).Name);

    foreach(string guid in guids) {
        string path = AssetDatabase.GUIDToAssetPath(guid);
        T asset = (T)AssetDatabase.LoadAssetAtPath(path, typeof(T));
        list.Add(asset);
    }
    return list;
}
```

***

## Objectがアセットか判定する
```
bool AssetDatabase.Contains(Object obj)
```
シーン上やランタイムならfalse

***

## ScriptableObjectをスクリプトから生成
```
private static void CreateScriptableObject() {
    // SaveFilePanel:OSの保存先選択パネルを読ぶ。戻り値はフルパス。キャンセル時は空文字。
    string path = EditorUtility.SaveFilePanel("", "Assets", "MyScriptableObject", "asset");

    if(!string.IsNullOrEmpty(path)) {
        // インスタンス生成
        MyScriptableObject obj = ScriptableObject.CreateInstance<MyScriptableObject>();
        // アセットとして保存
        AssetDatabase.CreateAsset(obj, path);
    }
}
```

***

## 便利なサイト  
[【Unity】【エディタ拡張】エディタ拡張チートシート](https://light11.hatenadiary.com/entry/2018/07/08/134405)  
[エディタ拡張で配列の入れ替えが簡単に出来るReorderableListの使い方と全コールバック【Unity】【エディタ拡張】](https://kan-kikuchi.hatenablog.com/entry/ReorderableList)  
