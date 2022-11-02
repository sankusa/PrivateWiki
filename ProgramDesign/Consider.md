# 実例で考えるプログラム設計
## Viewにロジックを持たせない
参考  
[UnityでPureC#で書いてみよう](https://nr-dev.hatenablog.com/entry/2021/11/15/040230)  

- 元のコード
```
public ShopItemView : MonoBehaviour
{
  [SerializeField] Text _price;
  [SerializeField] Image _icon;
  [SerializeFIeld] GameObject _soldOutLabel;

  public void Setup(UserData userData, ItemData itemData)
  {
    // 画像のセットアップ
    if (itemData.type == Item.Ticket)
    {
      _icon.sprite = 画像ロード();
    }
    else if (itemData.type == Item.Crystal)
    {
      _icon.sprite = 画像ロード();
    }

    // 売り切れラベルの表示
    _soldOutLabel.SetActive( itemData.shopStock <= 0 );

    // マスタデータを検索して値段を表示
    var masterData = MasterDataManager.FindShopItemMaster( itemData.masterId );
    _price.text = masterData.price;
    _price.color = userData.money < masterData.price ? Color.red : Color.white;
  }
}
```
◇検討  
ItemDataの他にEventItemDataも表示したい、となった場合の修正方法  
1. EventItemData用のViewを作る・・・見た目だけに関する部分を共通化できない。別のPrefabを作らなければならない。
2. EventItemData用の初期化関数を作る・・・ItemData、EventItemDataの両方に依存してしまう。
3. ItemData、EventItemDataに共通のインターフェースを継承させ、インターフェース型で受け取る・・・型に応じた柔軟な対応ができなくなる。無駄な関数の実装を強制してしまうかも。

◇ではどうすべきか  
見た目部分とロジック部分を別クラスにすべき。  
Viewは見た目だけ。  
データによってどう見た目を変えるかはViewの上位にViewSetterクラス(名前は要検討)を作ってそちらで決める。  
ItemDataとEventItemDataのそれぞれ用にViewSetterを作って使い分ける。  
```
// Viewの表示を単純にセットするメソッドのみで判断は持たない。
// それによりデータや状態を持たない。クラスの責任は表示のみ！Viewのテストもしやすいね！
public ShopItemView : MonoBehaviour
{
  [SerializeField] Text _price;
  [SerializeField] Image _icon;
  [SerializeFIeld] GameObject _soldOutLabel;

  public void SetImage(Sprite sprite)
  {
    // SpriteもSerializeFieldしている場合は、その決まった画像の種類をEnumを用意。
    // マスタデータの種類が、そのEnumのどれに対応するのかはPureC#側で判断して、Enumをセットする。など。
    _icon.sprite = sprite;
  }
  public void SetSoldOutLabel(bool active)
  {
    _soldOutLabel.SetActive(active);
  }
  public void SetPriceText(string text, Color color)
  {
    _price.text = text;
    _price.color = color;
  }
}
```
```
public class NormalShopItemViewSetter
{
  readonly ShopItemView _itemView;

  public NormalShopItemViewSetter(ShopItemView itemView)
  {
    _itemView = itemView;
  }

  public void Setup(UserData userData, ItemData item)
  {
    // 画像のセットアップ
    var iconSprite = LoadSprite(itemData.type);
    _itemView.SetImage( iconSprite );

    // 売り切れラベルの表示
    bool hasStock = itemData.shopStock > 0 ;
    _itemView.SetSoldOutLabel( !hasStock );

    // マスタデータを検索して値段を表示
    var masterData = MasterDataManager.FindShopItemMaster( itemData.masterId );
    var priceColor = userData.money < masterData.price ? Color.red : Color.white;
    _itemView.SetPriceText(masterData.price, priceColor);
  }
}

public class EventShopItemViewSetter
{
  (省略)
  pubcli void Setup(UserData userData, EventItemData eventItem)
  {
    EventItem関連の処理
  }
}
```
```
  if (isEvent)
  {
    var setter = new EventShopItemViewSetter(_shopItemView);
    setter.Setup(userData, eventItemData);
  }
  else
  {
    var setter = new NormalShopItemViewSetter(_shopItemView);
    setter.Setup(userData, itemData);
  }
```

◇その他の恩恵  
Viewが状態を持つ場合、表示部分をViewに押し付けることで状態管理がしやすくなる。  
ロジックと表示が絡み合っていると、互いに修正の影響が出る場合もある。分割すれば被害は最小限。  


***
