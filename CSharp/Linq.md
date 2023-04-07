IEnumerable<T>を返すメソッド(Select,Where,OrderByなど)は遅延評価。  
Sum,Max,ToList,ToArray,foreach(GetEnumeratorが実行される)などが使われると実体化する。  

```
List<int> nums = new List(){0, 1, 2, 3, 4, 5};
IEnumerable<int> query = nums.Select(x => f(x));  ←この時点ではfは実行されないしメモリも確保されない
nums.Select(x => f(x)).ToList(); ←fは実行されてメモリも確保される
foreach(int x in query)  ←fは実行されてメモリも確保される
{
    // なんか関係ない処理
}
```

### 注意すべき使い方  
```
public IEnumerable<int> Results => nums.Select(x => f(x));
```
呼び出し側で評価される度に計算とメモリの確保が行われる。  
クエリの結果がタイミングによって変わるならこれでもいいが、numsとfが固定ならば無駄な計算とメモリの確保が行われてしまう。  
そういった場合はキャッシュすべき
```
private List<int> results = nums.Select(x => f(x)).ToList();
public IEnumerable<int> Results => results;
```
  
### 注意すべき使い方②  
```
private IEnumerable<int> Results => nums.Select(x => f(x));
foreach(int x in Results)
{
    // 何か
}
foreach(int x in Results)
{
    // 何か
}
```
これもResultsが2回実体化されて無駄。キャッシュすべき。
