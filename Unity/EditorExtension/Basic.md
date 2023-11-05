- [EditorWindowテンプレ](#editorwindowテンプレ)
- [EditorWindowでScriptableObjectを更新する場合](#editorwindowでscriptableobjectを更新する場合)
- [EditorWindowのライフサイクル関数](#editorwindowのライフサイクル関数)
- [CustomPropertyDrawerテンプレ](#custompropertydrawerテンプレ)
- [エディタ起動時・コンパイル時にStatic関数を実行する(InitializeOnLoad属性)](#エディタ起動時コンパイル時にstatic関数を実行するinitializeonload属性)
- [Scope](#scope)
- [Unityでアセンブリリロード時に消えない値(SessionState)](#unityでアセンブリリロード時に消えない値sessionstate)  
- [エディタ拡張で使用できるコールバック一覧](#エディタ拡張で使用できるコールバック一覧)
- [ヘルプボックス](#ヘルプボックス)  
- [参考記事](#参考記事)  

***

## EditorWindowテンプレ
```
public class TestWindow : EditorWindow
{
    [MenuItem("Tools/" + nameof(TestWindow))]
    private static void Open()
    {
        GetWindow<TestWindow>();
    }

    void OnEnable()
    {
    
    }
    
    void OnDisable()
    {
    
    }
    
    void OnDestroy()
    {
    
    }
    
    void Update()
    {
    
    }
    
    void OnGUI()
    {
    
    }
}
```

***

## EditorWindowでScriptableObjectを更新する場合
基本的にはSerializedObjectを通して更新するべき(変更がシリアライズ(保存)され、Undoにも対応)
```
ScriptableObjectX X;
SerializedObject serializedX;

void OnEnable()
{
    X = // なんかXをロードする処理
    serializedX = new SerializedObject(X);
}

void OnGUI()
{
    serializedObject.Update();
    
    // serializedPropertyを通して値を更新
    serializedX.FindProperty("propertyName").intValue = 1;
    
    serializedObject.ApplyModifiedProperties()
}
```
SerializedPropertyを通さずに直接更新する場合はDirtyフラグを立てたり、Undoの対応が必要  

参考  
[【Unity】【エディタ拡張】シリアライズ対象の値を直接編集する際の挙動をちゃんと理解する](https://light11.hatenadiary.com/entry/2022/05/25/193411)  

***

## EditorWindowのライフサイクル関数
|関数|呼ばれるタイミング|
----|----  
|OnEnable|ウィンドウ生成時、コンパイル時、Unityエディタ起動時(ウィンドウが存在すれば)、再生時|  
|OnDisable|ウインドウを閉じる時、Unityエディタを閉じる時、再生時|  
|OnDestroy|ウインドウを閉じる時、Unityエディタを閉じる時|  
|Update|1秒間に200回前後(バックグラウンドだと10fpsくらいらしい)|  
|OnGUI|ウィンドウの描画|  

※ゲーム再生時(正確にはアセンブリのロード時)、OnDisable→OnEnableの順で呼ばれる  

参考  
[【Unity】 EditorWindowのライフサイクルの謎  ](https://www.f-sp.com/entry/2016/09/04/231754)  

***
## CustomPropertyDrawerテンプレ
```
[CustomPropertyDrawer(typeof(SomeClass))]
public class SomeClassDrawer : PropertyDrawer {
    public override void OnGUI(Rect position, SerializedProperty property, GUIContent label)
    {
        label = EditorGUI.BeginProperty(position, label, property);

        EditorGUI.EndProperty();
    }
}
```
***

## エディタ起動時・コンパイル時にStatic関数を実行する(InitializeOnLoad属性)
### InitializeOnLoad属性
クラスに適用すると、エディタ起動時・コンパイル時にstaticコンストラクタが呼ばれる。  
エディタのコールバックを利用する場合に便利。  
 
ex) InitializeOnLoadによって呼ばれるstaticコンストラクタ内でupdateコールバックに関数を登録。  
```
[InitializeOnLoad]
public class ClassA {
    static ClassA () {
        EditorApplication.update += Update;
    }

    static void Update () {
        Debug.Log("Updating");
    }
}
```

### InitializeOnLoadMethod属性
static関数に適用する。staticコンストラクタである必要はないのでこっちの方が使いやすい。  

ex) 上の例と同様の処理  
```
public class ClassA {
    [InitializeOnLoadMethod]
    static void MethodA() {
        EditorApplication.update += Update;
    }

    static void Update () {
        Debug.Log("Updating");
    }
}
```
***

## Scope
GUI.Scope継承型
```
public class VerticalScope : GUI.Scope {
    // コンストラクタでエディタの設定を変更
    public VerticalScope(params GUILayoutOption[] options) {
        BeginVertical(options);
    }
    // CloseScope(Disposeで呼ばれる)で設定を元に戻す
    protected override void CloseScope() {
        EndVertical();
    }
}
```
使い方  
```
void OnGUI() {
    using(new VerticalScope()) {
        // ブロック内で有効
    }

    // OnGUIを抜けてDisposeが呼ばれるまで有効
    using var _ = new VerticalScope();
}
```
理解すれば背景色Scoopeとか色々自作できる  

### Unityで用意されているScope
GUI.ClipScope
GUI.GroupScope  
GUI.ScrollViewScope
GUILayout.AreaScope  
GUILayout.ScrollViewScope  
GUILayout.VerticalScope  
GUILayout.HorizontalScope  
EditorGUI.DisabledScope  
EditorGUI.PropertyScope  
EditorGUI.ChangeCheckScope  
EditorGUI.IndentLevelScope  
EditorGUI.DisabledGroupScope  
EditorGUILayout.ScrollViewScope  
EditorGUILayout.VerticalScope  
EditorGUILayout.FadeGroupScope  
EditorGUILayout.HorizontalScope  
EditorGUILayout.ToggleGroupScope  

***

## Unityでアセンブリリロード時に消えない値(SessionState)
[【Unity】【エディタ】アセンブリリロード時に消えない値を保持するSessionStateの使い方まとめ](https://light11.hatenadiary.com/entry/2022/03/09/203032)  

***

## エディタ拡張で使用できるコールバック一覧
[【Unity】エディタ拡張で使用できるコールバックを40個まとめて紹介](https://baba-s.hatenablog.com/entry/2017/12/04/090000)  

***

## ヘルプボックス
[[Unity]Inspectorにヘルプボックスを表示する](http://koganegames.blog.fc2.com/blog-entry-125.html)  

***

### 参考記事
[エディタ拡張で仕切り線を描く](https://qiita.com/Gok/items/96e8747269bf4a2a9cc5)
