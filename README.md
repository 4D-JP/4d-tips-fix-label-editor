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
