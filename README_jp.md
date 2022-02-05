#  Playground Book Xcode Project #

## 概要 ##

Playground BookのXcodeプロジェクトファイルにようこそ！
このXcodeプロジェクトファイルは，以下の二つのものをビルドするためのものです．

- playground book
- playground bookのlive viewをデバッグするためのアプリケーション

これをサポートするため，Xcodeプロジェクトは，５つのターゲットが含まれています．

- **PlaygroundBook**: playground book自体を出力します．
- **BookCore**: live viewや著者のみが使う関数を含む補助モジュール，**BookCore**をコンパイルします．
- **BookAPI**: bookを通してユーザが書くコードにも自動的にインポートされるモジュール，**BookAPI**をコンパイルします．
- **UserModule**: ユーザがコードを書き足せる空のユーザモジュールを，**UserModule**をコンパイルします．
- **LiveViewTestApp**:  `Book_Sources`モジュールを使って，bookのlive viewがSwift Playgroundsでの表示をシミュレートするためのアプリをビルドします．

このプロジェクトは，PlaygroundSupport frameworkとPlaygroundBluetooth frameworkを含んでいます．
これら二つのフレームワークは，`BookCore`, `BookAPI`, `UserModule`，`LiveViewTestApp`ターゲットが，そのフレームワークのAPIを存分に活かせるようにするためのものです．
これらのフレームワークを含む，テンプレートなどのサポートコンテンツをビルドするには，Xcode 13.2が必要です．
他のバージョンのXcodeでこのテンプレートを使おうとしても，エラーが出るだけです．

playground bookファイルのフォーマットについては，*[Swift Playgrounds authoring documentation](https://developer.apple.com/go/?id=swift-playgroundbook-authoring)*をご覧ください．

## はじめに ##

このXcodeプロジェクトを始めるにあてって，playground bookをカスタマイズするためにいくつかの修正を加える必要があります．

1. `BuildSettings.xcconfig`(プロジェクトナビゲータの中の"Config Files"の中にある)を開き，以下の修正を加える．

  - `BUNDLE_IDENTIFIER_PREFIX`に，あなたのチームに適切な値をセットする．
  - `PLAYGROUND_BOOK_FILE_NAME`に，playground bookの名前をセットする．

また，bookの`Manifest.plist`にある`ContentVersion`の値を変更するために，`PLAYGROUND_BOOK_CONTENT_VERSION`を修正できます．
bookの`Manifest.plist`の`ContentIdentifier`を変更するために`PLAYGROUND_BOOK_CONTENT_IDENTIFIER`を修正できます。
`ContentIdentifier`のデフォルトの値は、`BUNDLE_IDENTIFIER_PREFIX` と`PLAYGROUND_BOOK_FILE_NAME`から生成されます。

1. `ManifestPlist.strings`を開き(プロジェクトナビゲータの”PlaygroundBook”の”PrivateResources”グループの中にある)、playground bookに関する文字列の値を変更します。
2. プロジェクトエディタを開き、LiveViewTestAppターゲットを選び、署名セクションの中から適切なTeamを選択します。ただし、テストをiOS Simulatorだけで行う場合は、これをする必要はありません。

一度、プロジェクトの設定を完了すれば、プロダクトとして、playground bookを生成するPlaygroundBookターゲットをビルドできるようになります。生成されたbookには、”Product”メニューを開き、“Open Build Folder in Finder”メニューを選ぶことでアクセスできます。
こうすると、Finderで”Products”ディレクトリが開かれるので、“Debug-iphoneos”や“Release-iphoneos”のようなビルド条件とプラットホームにあったディレクトリを選ぶと、生成されたbookがあります。
このbookをAirDropや他の方法で、Swift Playgroundsが動いているiPadへコピーできます。また、ダブルクリックすれば、使っているMacのSwift Playgroundsへインポートすることができます。

## よくあるタスク ##
このXcodeプロジェクトは、Xcode、あるいはディスク上で正しく、特殊なやり方で構成されていないと、正しくplayground bookを構成することができません。
以下に、playground bookを作るにあたって、やらないといけない、よくある作業を解説します。

### 新しくauxiliary(補助)，user(ユーザ)モジュールを追加する場合 ###

補助モジュールは，ソースを公開することなしに，Playground bookにAPIを提供するためのモジュールです．
ユーザが編集可能なソースは，ユーザモジュールに置きます．
 (補助モジュールと，ユーザモジュールに関する詳しい情報は，こちらをご覧下さい．*[Swift Playgrounds authoring documentation](https://developer.apple.com/go/?id=swift-playgroundbook-authoring)*.)

補助モジュールと，ユーザモジュールを作るためには，ディスク上で正しいフォルダ構成にする必要があります．
コード補完やリアルタイムで問題を表示するエディターの機能を取り入れるためには，ソースをビルドするstaticライブラリターゲットを作る必要があります．

1. プロジェクトナビゲータの`PlaygroundBook`グループを開く．
2. 補助モジュールを追加したいなら，`Module`グループを展開する．もし，ユーザモジュールを追加したいなら，`UserModules`グループを展開する．
3. `Module`あるいは`UserModules`を右クリックする．
4. `New Group`をコンテキストメニューから選択する．
5. `ModuleName.playgroundmodule`という名前をその新しいグループにつける．ここで，`ModuleName`は，好きな名前をつければよい．
6. その新しいグループを右クリックする．
7. `New Group`をコンテキストメニューから選択する．
8. `Sources`という名前をその新しいグループにつける．
9. その新しいグループを右クリックする．
10. `New File…`をコンテキストメニューから選択し，アシスタントを使って，新しいSwiftのソースファイルを作成する．
11. 編集メニューから`Add Target…`を選ぶ．
12. アシスタントのトップにあるiOS platformを選び，`Static Library`テンプレートを選択し，`Next`ボタンを押す．
13. `Product Name`のフィールドにStep.5で入力した`ModuleName`を入力する．
14. 他のフィールドも適当に埋めて，`Finish`ボタンを押す．
15. プロジェクトエディタで，プロジェクト自体を選択し，`Info`タブを選ぶ．
16. `Configurations outline view`にある，すべてのビルドconfigurationを展開する．
17. それぞれのビルドconfigurationに対して，新しく追加したターゲットの行で，`None`をクリックし，`“ModuleOverridingBuildSettings”`をポップアップメニューから選ぶ．
18. プロジェクトエディタで，新しく追加したターゲットを選択し，`Build Settings`タブを選ぶ．
19. フィルタバーの`All`をクリックし，すべてのビルド設定を表示する．
20. `build setting`をクリックして選択し，`Edit`メニューを開き，`Select All`をクリックする．
21. ターゲットレベルにあるすべてのbuild settingのオーバーライドを削除するため，キーボードの`Delete`キーを押す．
22. `Build Phases`タブを選ぶ．
23. build phaseの`Compile Sources`を展開する．
24. build phaseの`Compile Sources`にあるリストに入っているソースファイルを削除する．
25. プロジェクトナビゲータから，Step.10で追加したソースファイルをドラッグし，build phaseの`Compile Sources`の中に入れる．
26. プロジェクトナビゲータで，Step.12-15で追加したターゲット名にちなんだ名前のグループを探します．(例えば，`ModuleName`. `playgroundbook`の**拡張子は含まない．**)
27. このグループ上で，右クリックする．
28. コンテキストメニューから，`Delete`を選ぶ．
29. 確認ダイアログが表示されるので，ソースファイルを削除するために，`Move to Trash`を選ぶ．

以上の操作で，補助モジュールとユーザモジュールを追加することができます．
あなたが作ったモジュールを使うときは，以下のことに注意してください．

1. 補助モジュールとユーザモジュールのためのソースファイルは，`Modules`あるいは`UserModules`フォルダの中の`ModuleName.playgroundmodule`フォルダの中の`Sources`フォルダの中に置いておく必要があります．そうしないと，ソースファイルは，最終的にplayground bookにコピーされません．
2. プロジェクトエディタで，モジュールが正しい順番でビルドされるように明示的にターゲットの依存関係を記述しなければいけません．
3. `LiveViewTestApp`から，追加したモジュールを使いたい場合は，`LiveViewTestApp`から，このモジュールへの依存関係を明示的に指定しなければいけません．

### 補助モジュールとユーザモジュールの削除 ###

補助モジュール，ユーザモジュールを削除するには，３つのものを削除する必要があります．
モジュールをビルドするライブラリターゲット，モジュールのためのXcodeプロジェクトの中のグループ，モジュールのためのディスク上のフォルダ．
これは，以下の作業で完結します．

1. Xcodeプロジェクトのトップをプロジェクトナビゲータで選ぶ．
2. 消したいモジュールのためのターゲットを選ぶ．
3. モジュールを右クリックする．
4. コンテキストメニューから`Delete`を選び，確認ダイアログで削除を実行する．
5. プロジェクトナビゲータにある削除したいモジュールのためのグループを右クリックする．（例えば，`ModuleName.playgroundmodule`）
6. コンテキストメニューから`Delete`を選ぶ．
7. 確認ダイアログが出るので，`Move to Trash`を選び，ソースファイルを削除する．
8. プロジェクトナビゲータで，`Supporting Content`グループ(`PlaygroundBook`グループの中にある)を展開する．
9. `Modules`か`UserModules`の中の削除したいモジュールを見つける．
10. 削除したいモジュールのフォルダを右クリックする．
11. コンテキストメニューから`Delete`を選び，削除する．

### 補助モジュールとユーザモジュールのリネーム ###

補助モジュールとユーザモジュールをリネームするには，`ModuleName.playgroundmodule`グループと`ModuleName`ターゲットを新しい名前に変更します(例えば，`NewModuleName.playgroundmodule`と`NewModuleName`)．
bookの`Manifest.plist`の`UserAutoImportedAuxiliaryModules`にある補助モジュールをリネームする場合は，同様にそのリストの名前もリネームする必要がありますが，それ以外の変更は必要ありません．

### 補助モジュールやユーザモジュールにソースコードを追加する ###

このプロジェクトを正しく動作させるために，ソースコードは，Xcodeプロジェクト，モジュールのstaticライブラリ，ディスク上の`ModuleName.playgroundmodule/Sources`フォルダにそれぞれ追加する必要があります．
新しいソースファイルを追加するには，メニューから，`File` > `New` > `File...`を選ぶ，あるいはプロジェクトナビゲータの`ModuleName.playgroundmodule`グループの`Sources`グループを右クリックし，`New File...`を選びます．

アシスタントで，適当なテンプレートを選びます（おそらく`Swift File`か`Cocoa Touch Class`）．
アシスタントは，新しいファイルを保存するためのシートを表示するとき，以下のことが正しいかを確認してください．

1. `ModuleName.playgroundmodule`ディレクトリの`Sources`ディレクトリがファイルの保存先になっていること(`ModuleName`は，ソースファイルを追加しようとしているモジュールの名前)．
2. 新しいファイルは，`ModuleName`ターゲットにのみ追加される．

上で言及した`Sources`グループへのソースコードの追加すると，上の両方が満たされる場所がデフォルトになるはずです．
もし，ディスク上の正しい`Sources`ディレクトリにソースファイルが正しく保存されないと，最終的に出力されるplayground bookの中の正しい場所にソースがコピーされず，Swift Playgrounds上でソースコードが利用できなくなります．
もし，モジュールのstaticライブラリターゲットのメンバーとしてソースコードを追加してない場合，Xcode上でソースコードはコンパイルされません．
これは，シンタックスハイライト，コード補完などの機能がそのソースファイル上で動作しないことを意味します．
そして，そのソースコードが提供するAPIも，プロジェクトの中の他のソースファイルからは使用できません．

** 注意 **
playground bookでは，Swiftのソースコードだけがサポートされます．
このXcodeプロジェクトは，ソースコードの制限を強制しませんが，Swift以外のソースファイルは，最終的に出力されるplayground bookにコピーされますが，Swift Playgroundsは，無視して処理します．

### 本全体から見えるPrivateResources ###

本全体から見える`PrivateResources`ディレクトリへコンテンツを追加するには，Xcodeプロジェクトにリソースファイルを追加し，そのファイルがPlaygroundBookのターゲットの`Copy Bundle Resources` build phaseのメンバーに入っていることを確認してください．
リソースファイルは必要があれば（例えば，xibs, storyboards, asset catalogsなどのリソースファイルであれば）コンパイルされて，playground bookの`PrivateResources`ディレクトリへコピーされます．

### 本全体から見えるPublicResources ###

Xcodeプロジェクトは，デフォルトでは，本全体から見える`PublicResources`をサポートしません．
bookに`PublicResources`ディレクトリを追加するためには，以下のようにします．

1. `PlaygroundBook`ディレクトリの中に，`PublicResources`ディレクトリをFinderで作る．
2. `PublicResources`ディレクトリをXcode中の`PlaygroundBook`グループに追加する．このとき，`group reference`ではなく，`folder reference`として追加する．
3. `PublicResources`ディレクトリを，プロジェクトエディタの中のPlaygroundBookターゲットの`Copy Book Contents` build phaseに追加する．

`PrivateResources`と違って，`PublicResources`のコンテンツは，コピーされるだけで，コンパイルされません．
もし，リソースをコンパイルしたい場合は，`PrivateResources`に入れる必要があります．

### Chapters, Pages, Chapter, Pageごとのコンテンツ ##

このXcodeプロジェクトは，playground bookのchaptersやpagesの編集をあまりサポートしません．
`Chapters`ディレクトリは，Xcodeプロジェクトにフォルダとして表示されます．
`playgroundchapter`パッケージはそこに置くことができ，置かれたパッケージは，最終的にplayground bookにコピーされます．

chapterやpageを追加するとき，新しいchapterやpageへのリファレンスをbookやchapterの`Manifest.plist`ファイルを編集しなければいけません．

このXcodeプロジェクトは，シンタックスハイライト，コード補完，live issueなどのエディタの機能をchapterやpageの中でサポートしません．
なので，なるべくできるだけコードは補助モジュールに書き，chapterやpageには，なるべくコードを書かないようにすることをお勧めします．

### Live Viewをテストする ###

このXcodeプロジェクトは，playground bookのlive viewのテストをサポートします．
このテストサポートの機能は，playground bookのlive viewがBookCoreの補助モジュールに実装されていることを前提としています．
もし，live viewが（例えばpageの`Contents.swift`や`LiveView.swift`などの）他の場所に実装されているときには，このメカニズムを使ってテストするのは難しくなります．

live viewをテストするためには，`LiveViewTestApp`アプリを実行します．
iPad，iOSシミュレータ，Mac Catalystアプリとして動作するこのアプリケーションは，Swift Playgroundsによるのと同じようにlive viewを表示することができます．
特に，`LiveViewTestApp`は，`PlaygroundSupport.framework`によって提示されるlivew view safe area layout guideを正しく設定します．

livew viewを設定するために，`AppDelegate.swift`で，`setUpLiveView()`メソッドを実装してください．
これは，live viewとして使うように用意された`UIView`や`UIViewController`を返すためのものです．

デフォルトでは，`LiveViewTestApp`は，live viewをフルスクリーンで起動します．
`LiveViewTestApp`は，side-by-sideビューでlive viewを起動することができます（live viewがSwift Playgrounds上でソースコードエディタのとなり，あるいは下に表示されるケースがあります）．
これを実行するためには，`AppDelegate.swift`の`liveViewConfiguration`プロパティが，`.fullScreen`の代わりに`.sideBySide`を返すようにしてください．