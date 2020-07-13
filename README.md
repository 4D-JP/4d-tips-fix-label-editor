# 4d-tips-fix-label-editor
ラベルエディターコンポーネントをカスタマイズするには？

#### ソースコードの場所

4Dに組み込まれているエディターの中には，ソースコードが公開されているものがあります。

v17 R6/v18.xまでは，4D Forums/[4D Partners Worldwide](https://forums.4d.com/List_Message/JP:0/0/2/1/1/1/17350595/0/0/1/-1/0/0/0/0/0/0)にログインし，バイナリモードのストラクチャファイルをダウンロードします（ライセンスに合意する必要があります）。v15から18.xまで，バージョン毎にスレッドが建てられていますが，FTPのアドレスは共通です。18.xでは，下記のコンポーネントが入手できます。

* 4D Labels / 16R4以降
* 4D Progress / 16.x以降
* 4D Report / 16R2以降
* 4D SVG / 15R5以降
* 4D SVG Area / 16 R2から17R3まで
* 4D Widgets / 16.x以降
* 4D WritePro Interface / 16R2以降
* 4D Pop / 16R2以降

[18R2以降](https://blog.4d.com/news-flash-4d-components-available-on-github/)，ソースコードのプロジェクトが[GitHub](https://github.com/4d/)で公開されています。

* [4D Labels](https://github.com/4d/4D-Labels)
* [4D Progress](https://github.com/4d/4D-Progress)
* [4D Report](https://github.com/4d/4D-Report)
* [4D SVG](https://github.com/4d/4D-SVG)
* ~~4D SVG Area~~
* [4D Widgets](https://github.com/4d/4D-Widgets)
* [4D WritePro Interface](https://github.com/4d/4D-WritePro-Interface)

下記のコンポーネントは，4Dではなく，Vincent de Lachauxのリポジトリで公開されています。

* [4D Pop](https://github.com/vdelachaux/4DPop)

#### ソースコードの入手

* v18R2以降

GitHubのリポジトリを自分のアカウントにクローンします。

* v18.x以前

コンポーネントのソースコードをダウンロードします。

#### ソースコードの改変

ストラクチャまたはプロジェクトの名称を変更します。

例：

4D Labels → **My** Labels

エントリーポイント（公開メソッド）の名称を変更します。

C_OPEN_EDITOR  → **My**_OPEN_EDITOR  
C_Print_label  → **My**_Print_label  

標準コマンドを使用する代わりに，直接エントリーポイントを呼び出すようにホストのメソッドを書き換えます。

``PRINT LABEL([Table_1])`` → ``My_OPEN_EDITOR(Table(->[Table_1]);False;$Txt_labelDocument)``  
``PRINT LABEL([Table_1];*)`` → ``$Lon_error:=My_Print_label(Table(->[Table_1]);"file:"+$Txt_labelDocument;"*")``  

ストラクチャのComponentsフォルダーにインストールします。アプリケーション内にインストールされているオリジナルを入れ替えるのではない点に留意してください。

#### コンポーネントのアップデート

* v18R2以降

必要に応じてプロジェクトの変更点を``git merge``します。

* v18.x以前

バイナリ版ストラクチャのコピーをv18に変換し，書き出したプロジェクトの差分を比較して，手作業でメソッドの変更点をマージする必要があります。

---

### ケーススタディ

4D Labels（v18.253933）には，下記のような不具合があります。

* フォームを指定してラベル印刷した場合，ラベルのレイアウト（行数・列数・マージン・水平垂直間隔）が反映されない
* ラベルが用紙の端で切れてしまう

ソースコードを調べると，フォームを指定した場合，用紙サイズを元に整数を返す除算で行列の数を決めていることがわかります。

```4d
If ($Boo_form)
	
	  // Calculate columns & rows number according to the form dimensions
	GET PRINTABLE AREA($Lon_height;$Lon_width)
	GET PRINT OPTION(Orientation option;$Lon_orientation)

Else

	DOM GET XML ATTRIBUTE BY NAME($Dom_label;"columns";$Lon_columns)
	DOM GET XML ATTRIBUTE BY NAME($Dom_label;"rows";$Lon_rows)

End if 
```

**問題点**

* フォーム名だけを指定した場合と設定ファイル（4lbp）でフォーム名を指定した場合で処理が分かれていない
* マージンと間隔を計算に含めていない

修正版のコンポーネントは[Releases/0.1.1](https://github.com/4D-JP/4d-tips-fix-label-editor/tree/0.1.1)からダウンロードすることができます。
