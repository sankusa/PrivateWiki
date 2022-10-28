- [EditorWindowテンプレ](#editorwindowテンプレ)
- [EditorWindowのライフサイクル関数](#editorwindowのライフサイクル関数)  
- [Objectがアセットか判定する](#objectがアセットか判定する)  

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

## EditorWindowのライフサイクル関数
|関数|呼ばれるタイミング|
----|----  
|OnEnable|ウィンドウ生成時、コンパイル時、Unityエディタ起動時(ウィンドウが存在すれば)、再生時|  
|OnDisable|ウインドウを閉じる時、Unityエディタを閉じる時、再生時|  
|OnDestroy|ウインドウを閉じる時、Unityエディタを閉じる時|  
|Update|1秒間に200回前後(バックグラウンドだと10fpsくらいらしい)|  
|OnGUI|ウィンドウの描画|  

※ゲーム再生時(正確にはアセンブリのロード時)、OnDisable→OnEnableの順で呼ばれる  

参考サイト  
[【Unity】 EditorWindowのライフサイクルの謎  ](https://www.f-sp.com/entry/2016/09/04/231754)  

***

## Objectがアセットか判定する
```
bool AssetDatabase.Contains(Object obj)
```
シーン上やランタイムならfalse
