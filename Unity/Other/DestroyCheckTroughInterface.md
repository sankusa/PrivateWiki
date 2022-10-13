#### MonoBehaviour継承型をInterface型で保持していた場合のDestroy判定
> UnityEngine.Objectは==演算子をオーバーロードしているらしい。  
> 恐らくDestroy済みの場合に==nullで判定できるようにするためと思われる。  
> 
> 下記のようなクラスがあったとする。
>```
>public class Hoge : MonoBehaviour, InterfaceX {}
>```
> 下記の場合、外部でxの実体がDestroyされてもログへは何も出力されない。  
> Destroyしてもメモリ上はインスタンスが残っており、変数xは依然として残ったインスタンスを参照し続けるためnullと判定されないのが理由。  
> ※x内の変数にアクセスしようとすると例外は発生する。  
> Destroyを判定する場合はUnityEngine.Object型の==演算子を呼ばないとダメ。  
>```
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
> 以下のようにInterfaceX型の変数xをUnityEngine.Object型にキャストすればDestroyされているかの判定が正しくできる
>```
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
