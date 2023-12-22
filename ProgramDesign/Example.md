### 方針
肥大化した関数は見るのが辛いが、関数を分割しすぎても辛い。  
1000行の関数1つ→辛い  
100行の関数10個
意味を持つ単位で関数を分割

☆外から関数名を見て中身を見なくても何をやっているかわかるなら分割してよい。→読むときに、関数内に押し込んだ行数分だけ読まなくて済む。    

行数が多いだけなら別にいい。  
関数の呼び出し関係(階層)が複雑だと読むのが辛い。  

関数内→インスタンス変数を使った途端、状態を意識しなくてはいけなくなる。変数を介して他の関数と影響しあってしまう。実際には影響がないとしてもソースコードをなめないと確証できないので一気に読む難易度が上がる。    

クラスXがクラスYのインスタンスを持っていて、クラスYの変数Zを参照しているとしたら、クラスXはクラスYに加えてZにも直接依存している。可能ならZはYの中に隠ぺいするべき。  

参照透過性  

クラス内の変数と関数について、参照をグラフにしたものを考えて、グラフが複雑だとダメ。  

インスタンス変数はできるだけReadonly

```
public class ClassX {
    int a;
    int b;
    int c;
    public void Method0() {
        aを使った処理;
        bを使った処理;
        cを使った処理;
    }
    public void Method2() {
        cを使った処理;
    }
}
```

```
public class ClassX {
    int a;
    int b;
    int c;
    public void Method0() {
        Method1();
        cを使った処理;
    }
    private void Method1() {
        aを使った処理;
        bを使った処理;
    }
    public void Method2() {
        cを使った処理;
    }
}
```

```
public class ClassX {
    ClassAB ab;
    int c;
    public void Method0() {
        ab.Method1();
        cを使った処理;
    }
    public void Method2() {
        cを使った処理;
    }
}

public class ClassAB {
    int a;
    int b;
    public void Method1() {
        aを使った処理;
        bを使った処理;
    }
}
```
