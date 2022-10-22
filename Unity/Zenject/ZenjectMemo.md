### Zenjectを使う意義
>・依存性注入(DI)ができる  
>　=依存するクラスのインスタンスをnewではなく外部から注入してもらう。  
>
>これにより、  
>・インスタンスの生成方法を意識しなくてよくなる。(コンストラクタの引数が変わる、 Factoryで作るようになる、などの変更の影響を受けなくなる)  
>・インターフェースを噛ませることで、具象クラスを無影響ですげ替えることができる(ミドルウェアによって切り替えたいクラス、テスト用のクラスなど)  
>#### Before  
>```
>public class Hoge
>{
>    private Fuga fuga;
>    
>    public Hoge()
>    {
>        fuga = new Fuga();
>    }
>}
>```
>#### After(依存性注入)  
>```
>public class Hoge
>{
>    private Fuga fuga;
>    
>    public Hoge(IFuga fuga)
>    {
>        this.fuga = fuga;
>    }
>}
>```
>
### どのインジェクションを使うか
>基本的にコンストラクタインジェクションを使うのが安全性的には良い。  
>コンストラクタは1度しか呼ばれないので、後から変更される恐れがない。  
>またコンストラクタインジェクションのみ、循環参照が発生した場合にエラーを吐くらしい。
>
>でもフィールドorプロパティインジェクションの方が書くのが楽・・・
>
>#### ・コンストラクタインジェクション
>```
>private readonly Hoge hoge;
>
>public Constructor(Hoge hoge)
>{
>    this.hoge = hoge;
>}
>```
>#### ・フィールドインジェクション
>```
>[Inject] private Hoge hoge;
>```

参考  
https://adarapata.hatenablog.com/entry/unity-advent-calendar-2019  
