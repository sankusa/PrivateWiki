### OnDestroy中にInstantiateを呼ぶ場合、注意が必要
> 以下のようなエラーを吐く場合がある  
> Some objects were not cleaned up when closing the scene. (Did you spawn new GameObjects from OnDestroy?)  
> Unityはシーン再生終了時にシーンのオブジェクトを全て破棄し、シーン再生時の状態をロードするらしいのだが、
> OnDestroy時にInstantiateするとこのロードのタイミングでゴミが残るためエラーが起きるよう。  
>
> ■回避策
> ```
>{
>     private bool isQuitting = false;
>     
>     void OnApplicationQuit()
>     {
>         isQuitting = true;
>     }
>    
>     void OnDestroy()
>     {
>         if (!isQuitting)  
>         {  
>             GameObject.Instantiate (new GameObject ("gomi"));
>         }  
>     }
>}
> ```
> 参考文献
> https://tsubakit1.hateblo.jp/entry/20140127/1390834375
