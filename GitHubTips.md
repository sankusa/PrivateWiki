### gitignore
#### 特定フォルダの除外解除
/Assets/*  
!/Assets/Scenes/  

以下の書き方だとダメ  
/Assets/   
!/Assets/Scenes/   

/Assets/*   
!/Assets/Scenes/*  

親デイレクトりが無視されているファイルは!を使っても無視しないようにはできない、らしい。  
よって、「/Assets/*」(AssetsディレクトリではなくAssets以下の全ファイルを無視するという書き方)と書くと「!/Assets/Scenes/」が効くようになる。  
正直原理をちゃんと理解してない。  
