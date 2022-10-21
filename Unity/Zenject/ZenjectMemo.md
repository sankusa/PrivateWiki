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
