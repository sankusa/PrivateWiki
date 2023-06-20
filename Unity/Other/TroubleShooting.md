### トラブル解決メモ

### unity2022導入後、VSCodeの補完が効かない＋コンパイルエラーすら表示されない問題
①導入時、VisualStudioのチェックを外したことでcsproj,slnファイルが生成されないのが原因？→VisualStudio導入→VisualStudio起動中はVSCodeが機能するように。  
②VisualStudio閉じた後、csファイルを新規作成するとそのファイルはVSCode上で補完が効かない。  
③Unity上で[Edit]→[Preference]→[ExternalTools]→[ExternalScriptEditor]→[BrowseからVSCode]としていたが、PackageManagerからVSCodeをインストールしたら直った。  
