# さあ、始めよう

## PHPの基礎

この本では、PHPの基礎的な知識があることを前提としています。その知識には次の項目が含まれます:

 - [関数](http://www.php.net/manual/en/language.functions.php)
 - [配列](http://www.php.net/manual/en/language.types.array.php)
 - [変数](http://www.php.net/manual/en/language.variables.php)
 - [ループと条件](http://www.php.net/manual/en/language.control-structures.php)
 - [クラスとオブジェクト](http://www.php.net/manual/en/language.oop5.php)
 - [クラスの継承](http://www.php.net/manual/en/language.oop5.inheritance.php)
 - [ポリモーフィズム](http://code.tutsplus.com/tutorials/understanding-and-applying-polymorphism-in-php--net-14362)
 - [POST](http://www.php.net/manual/en/reserved.variables.post.php) と [GET](http://www.php.net/manual/en/reserved.variables.get.php)
 - [変数のスコープ](http://www.php.net/manual/en/language.variables.scope.php)

これらの概念の十分な理解がない場合、先に進む前にしっかりと理解しておいたほうがいいでしょう。

また、PHPシンタックスハイライト機能を持つコードエディターを持っていることも前提としています。次も訳に立ちます:

 - 自動インデント
 - 自動補完
 - ブレスマッチング
 - 構文チェック

## ローカル開発環境

ローカルの開発環境を持つことも重要です。PHPファイルを変更し、本番サーバーのそれをアップデートして無事を祈るという昔の日々は去りました。

ローカルの開発環境を使えば、より速く作業でき、ファイルのアップロードやダウンロードが必要無くなり、不安定なインターネット接続に翻弄されることもなく、ウェブページの読み込みを待たされることもなくなります。ローカルのサーバスタックを使えば、Wifiや携帯電話の電波のないトンネルに入った列車の中でも作業できますし、本番サーバにデプロイ前にテストもできます。

ローカルの開発環境の構築にはいくつか方法がありますが、大別すると2つのカテゴリーになります:

 - バーチャルマシーン
 - ネイティブのサーバスタック

1つ目のタイプの環境は、Vagrantなどのプロジェクトを通常は含んでいて、標準化された一貫性のある仮想マシンを利用します。

2つ目のタイプは、自分のオペレーティングシステムにサーバーソフトウェアを直接インストールするタイプです。これを簡単に行うための様々なツールがありますが、その環境は独自のものになってしまうためデバッグが難しくなります。これらはLAMP(Linux Apache MySQL PHPの頭文字)スタックと呼ばれることもあります。

### IIS

Microsoft Internet Information Services はWindowsベースのサーバを動かすサーバーソフトウェアです。Windowsに付属していて、インストールするコンポーネントによって様々な変種があります。WordPressのコミュニティではIISのセットアップに関する知見は多くありません。たいていのリモートサーバーはApacheもしくはNginxで動いていて、開発者の知見もそちらの方に集中しています。

IISの選択は難しいものとなるでしょう。

## バージョン管理

チームでの作業にはバージョン管理は必須です。バージョン管理システムは長期間に渡って変更を追跡し、開発者が共同で作業をしたり、変更を元に戻したりできるようにします。

### Git

Linuxの作者、リーナス・トーバルズによって作られた[Gitは人気のある分散型のシステム](http://git-scm.com/)です。GitHubをお使いであれば、すでにご存知かと思います。

### Subversion

svnとしても知られていて、集中管理型のバージョン管理システムです。WordPress.org上のプラグインとテーマのリポジトリに使われています。
