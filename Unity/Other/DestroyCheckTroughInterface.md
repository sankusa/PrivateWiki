#### MonoBehaviour継承型をInterface型で保持していた場合のDestroy判定
> 下記の場合、外部でxの実体がDestroyされてもログへは何も出力されない。  
>```
>public class Hoge : MonoBehaviour, InterfaceX {}
>
>public class Fuga : MonoBehaviour
>{
>    privte InterfaceX x;
>    void Start()
>    {
>        x = GetComponent<Hoge>();
>    }
>
>    void Update()
>    {
>        if(x == null) Debug.Log("Destroyed.");
>    }
>}
>```
> Destroyしてもメモリ上はインスタンスが残っており、変数xは依然として残ったインスタンスを参照し続けるためnullと判定されないのが理由。(※x内の変数にアクセスしようとすると例外は発生する。)  
>
> UnityEngine.Objectクラスは==演算子をオーバーロードしており、Destroy後は==nullに引っかかる特殊仕様。  
> そのため、Destroyされているかを判定する場合はUnityEngine.Object型の==演算子を呼ばなければならない。  
> 以下のようにキャストすればDestroyされているかの判定が正しくできる
>```
>public class Hoge : MonoBehaviour, InterfaceX {}
>
>public class Fuga : MonoBehaviour
>{
>    privte InterfaceX x;
>    void Start()
>    {
>        x = GetComponent<Hoge>();
>    }
>
>    void Update()
>    {
>        if(x is UnityEngine.Object o && o == null) Debug.Log("Destroyed.");
>    }
>}
>```
