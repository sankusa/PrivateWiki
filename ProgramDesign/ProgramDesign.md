# プログラム設計(※Unityでの開発を想定)

## 設計の大目的
「プロダクトの開発・保守・運用のコストを最低限にする」こと  

***

## 評価軸
### プログラム全体の見通しやすさ
### 修正・拡張のしやすさ

***

## するべきこと
>### 依存関係の整理
>`変更が少ない部分に依存を集め、変更が多い部分への依存を減らす`  
>#### プログラム構成要素の依存性
>>・引数、他のクラス  
>>・ライブラリ、フレームワーク(便利な反面、小回りが利かない・突然のサポート終了などの危険性(有名どころなら比較的安心))  
>>の他、潜在的に  
>>・使用言語  
>>・デザインパターンやコーディング規約  
>>・特定状況下でのみ起こるバグとそれを回避するハック  
>>・プログラムの要件  
>>・プログラムのユーザー  
## パターン・原則
>### モジュール化  
>モジュール=1つのプログラムファイル  
>モジュール化=大きいソフトウェアを複数のプログラムファイルに分割すること  
>#### モジュール化の２つのアプローチ
>>・型によるモジュール化(オブジェクト指向プログラミングの基本)・・・ボトムアップ  
>>・手続き的なモジュール化(「入力-処理-出力」の単位でソフトウェアを分割する)・・・トップダウン  
>#### 関心の分離  
>#### カプセル化  
>#### 抽象化  
>#### 疎結合  
>#### 高凝集  
>#### 単一定義 
>[参考サイト](https://masuda220.hatenablog.com/entry/2020/06/26/182317#%E7%99%BA%E5%B1%95%E6%80%A7%E3%82%92%E3%81%86%E3%81%BF%E3%81%A0%E3%81%99%EF%BC%97%E3%81%A4%E3%81%AE%E8%A8%AD%E8%A8%88%E5%8E%9F%E5%89%87)
>
>### オブジェクト指向設計原則　GRASP  
>https://qiita.com/Yahagi_pg/items/0bb484f3c25fb9f84be8  

参考  
https://qiita.com/wm3/items/2c90bfd9e973d368ebd8
