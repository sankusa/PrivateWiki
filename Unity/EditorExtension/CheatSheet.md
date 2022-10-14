### EditorWindow
#### テンプレ
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
#### ライフサイクル
```
public class TestWindow : EditorWindow
{
    // UnityEditor上部のツールバーから呼び出せるようになる
    [MenuItem("Tools/" + nameof(TestWindow))]
    // 関数名はOpenでなくても何でもいい
    private static void Open()
    {
        // ConvenientWindowを表示する
        GetWindow<TestWindow>();
    }

    // ウィンドウ生成時に呼ばれる
    // コンパイル時にも呼ばれる
    // Unityエディタを開いたときも呼ばれる
    void OnEnable(){}
    
    // ウインドウを閉じる時に呼ばれる
    // Unityエディタを閉じる時に呼ばれる
    void OnDisable(){}
    
    // ウインドウを閉じる時に呼ばれる
    // Unityエディタを閉じる時に呼ばれる
    void OnDestroy(){}
    
    // 1秒間に200回前後呼び出される。(バックグラウンドだと10fpsくらいらしい)
    void Update(){}
    
    // ウィンドウの描画
    void OnGUI(){}
    
    // ※ゲーム再生時(正確にはアセンブリのロード時)、OnDisable→OnEnableの順で呼ばれる
}
```
