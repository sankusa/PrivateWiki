- [EditorWindowテンプレ](#editorwindowテンプレ)
- [EditorWindowでScriptableObjectを更新する場合](#editorwindowでscriptableobjectを更新する場合)
- [EditorWindowのライフサイクル関数](#editorwindowのライフサイクル関数)  

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
直接更新する場合はDirtyフラグを立てたり、Undoの対応が必要  

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
