#### Taskと及びasync/awaitの話
async・・・await、つまり待機を関数内でできるようにする修飾子  
await・・・Taskなど、待機可能な戻り値を持つメソッドを終了するまで待機するという宣言  
Task.Run(() => 処理)・・・別スレッドで処理を行う。

asyncを付けただけでは挙動は変わらない。awaitが使えるようになるだけ。
```
using UnityEngine;
using System.Threading;
using System.Threading.Tasks;

public class Test : MonoBehaviour {

    // Unityから呼ばれる
    // メインスレッドで実行される
    async void Start() {
        // メインスレッドで実行されるのでフリーズする
        await SleepTask();

        // これもメインスレッドで実行されてフリーズする
        var _ = SleepTask();

        // Task.Delayならフリーズせずに待機できる(共にメインスレッド上で実行される)
        await DelayTask();
        _ = DelayTask();

        // 別スレッドで呼ばれる
        // Unityの機能をメインスレッド以外から呼んだので怒られる(UnityException: get_gameObject can only be called from the main thread.)
        await Task.Run(UnityTask);

        // 別スレッドで呼ばれる
        // awaitしていないのでエラーログは出力されない
        _ = Task.Run(UnityTask);
    }

    // スレッドを止める(フリーズする)
    async Task SleepTask() {
        Thread.Sleep(1000);
        await Task.CompletedTask;
    }

    // 停止(フリーズはしない)
    async Task DelayTask() {
        // awaitはメインスレッドに一旦制御を返せる
        await Task.Delay(1000);
    }

    // Unityの機能を使うタスク
    async Task UnityTask() {
        Debug.Log(gameObject.transform.position);
        await Task.CompletedTask;
    }
}
```

#### TaskとUniTaskの違い
できることは大体同じっぽい。  
UniTaskはUnity用に最適化され、機能がないので省メモリ、ゼロアロケーション、TaskTrackerなどの利点がある。  
