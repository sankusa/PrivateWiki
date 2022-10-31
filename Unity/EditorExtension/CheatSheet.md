- [Objectのロード(1件、全件)](#objectのロード1件全件)
- [Objectがアセットか判定する](#objectがアセットか判定する)  
- [便利なサイト](#便利なサイト)

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

## 便利なサイト  
[【Unity】【エディタ拡張】エディタ拡張チートシート](https://light11.hatenadiary.com/entry/2018/07/08/134405)  
