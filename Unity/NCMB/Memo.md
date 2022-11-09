[基本的な使い方](https://mbaas.nifcloud.com/doc/current/datastore/basic_usage_unity.html)  
[NCMBのUnity SDKにおけるデータの取得方法](https://blog.mbaas.nifcloud.com/entry/2021/09/17/185329#%E9%85%8D%E5%88%97%E5%9E%8B)  
[mBaaSのデータストアで扱う特殊なオブジェクト形式について](https://blog.mbaas.nifcloud.com/entry/2020/01/24/174832)  
[エラーコード一覧](https://mbaas.nifcloud.com/doc/current/rest/common/error.html)  

NCMBObject.RemoveRange(string key, IEnumearble list)の仕様  
サーバ上に対象の値が無くてもエラーにならない  
多分検知できない  

NCMBObject.AddUniqueToList(string key, IEnumearble list)の仕様  
値がかぶって登録されなくてもエラーにならない  
多分検知できない  

厳密にやるなら配列よりリレーションの方がよさそう？  

NCMBObject.SaveAsync()  
通信後、NCMBObjectからobjectId,CreateDate,UpdateTimeが取得できる  
