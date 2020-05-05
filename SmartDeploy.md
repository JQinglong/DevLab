#RPGツクール リスト表示

## 初めての公開ゲーム
CSV読み込みのプラグイン
[RPGツクールプラグイン入門](https://qiita.com/JQinglong/items/787c356efd73e7a9933a)
を使って、クイズアプリを作って、公開してみました。
[クイズde三国志](https://game.nicovideo.jp/atsumaru/games/gm14659)
クイズに答えて、敵を倒そう！みたいなもので、クイズは、情報処理試験と、中国語試験となっています。
マニアック！
（問題・解答が公開されている情報って限られますよね・・・）

上記で作ったプラグインをさらに変更し、ゲームの途中で再度別のCSVを読み込めるようにしました。
これに手間取って、現状おそらく正しくない方法で実装していますが、これの考察はまた改めて。

で、元々やりたかったこととしては、オンラインゲーム化？複数ユーザが一緒に参加している感を出したいなと。
例えば、自分の経験値を公開して、ランキング表示するとか。
ところが、この辺は、RPGアツマールに公開する前提で、アツマールのプラグインを使えば、すぐに実現できてしまいました。
なんとなく、アツマールへ（というかニコニコへかな）への警戒感を持っていたのですが、モバイル端末向けのコントローラが用意されていたり、ニコニコ式のコメント機能があったり、いや、これはすごい。
しかも、ゲーム公開すると、新着ゲームとして表示がされることにより、一気に利用者が広がってくれる。
いきなりランキングが埋まるほどの利用者がいてくれて、自分よりやり込んでくれている人がいるというのは感動すら覚えます。

ということで、アツマール機能を活用して、さらに、みんなで協力して強大な敵を倒そうみたいなことをやりたいのですが（こういうのをレイドボスというのですね）、これも サンプルゲームが用意されているので、お手軽にできそうです。
[サンプルゲームのプロジェクトファイル](https://atsumaru.github.io/api-references/download/sample-projects)

ということで、より面白いゲームが作れそうなのですが、果たしてそれでよいのでしょうか？

（？）

面白いゲームを作れれば、みんな喜んでくれるというわけではないですよね。

（面白いゲームができたら嬉しいけど・・・？？）

未来を見据えたアプリ開発を目指したいと思います。

（お好きにどうぞ！）

## 開発プラットフォームとしてのRPGツクール

直近ではVueの勉強をしたりして、面白いなと思ったりしているわけですが、どうしてもアプリ作りにネックになってしまうのが、UIの部分です。
RPGツクールを見つけた時に思ったことですが、
[RPGツクール入門（ちょっと変化球）](https://qiita.com/JQinglong/items/b0f4975bb5f70fac9518)
魅せる画面デザインの作成手段として使えるのではないかという期待をしています。

単に時間をつぶすためのゲームを作りたいわけではないのですが、みんなに愛されるゲームのUI・UXを生かすことで、提供するものの価値をあげられるのではないかと考えています。
そういう開発を行うために、割とごりごりのJavascriptを書けるRPGツクールは、すぐれた開発プラットフォームなのではないかと感じました。

そして、ゲームではないアプリをRPGツクールで作ろうと考えた時に、改めて原点に立ち返ってみようと。
原点とは。
Hello World は「文章の表示」でできるのでよいですよねと。
で、何か作ってみようという時に、よくTodoリストを作りますよね。

**RPGツクールでTodoリストを作ってみよう**

そういうチャレンジをしてみます。

## 環境構築
こちらを大いに参考にさせていただきました。
[RPGMV-SmartDeploy-forWEB](https://github.com/katai5plate/RPGMV-SmartDeploy-forWEB)

ゲーム開発用のフォルダで、git cloneして、そのフォルダをVSCodeで開いて、ターミナルで、yarn setupしたところから開始します。

（作成したフォルダ）/RPGMV-SmartDeploy-forWEB/src
を指定して、ツクールで新規プロジェクト DevLab を作成します。

script.config.json
編集で、

```json:
    "gameDirName": "DevLab",
```

として、

```
yarn build
```

Docにデプロイ成功。
完全に新規のプロジェクトのままで150MB。

いったんローカルで動かしてみます。
macでのapache設定はこちらを参照。
[Macでローカルサーバを立ち上げる方法](https://qiita.com/shuntaro_tamura/items/bdabcb77926dc92617b1)

```
cd /Library/WebServer/Documents/
ls
sudo mkdir games
sudo chmod 777 games
mkdir DevLab
cp -R （作成したフォルダ）/RPGMV-SmartDeploy-forWEB/docs/* /Library/WebServer/Documents/games/DevLab/
```

http://localhost/games/DevLab/
成功しています。

デプロイ先として、GitHub Pages と、Netlify を書いてくださっています。
冒頭の公開したゲームも、アツマールを候補外としていた段階でデプロイ先をどこにするか考えました。
無料で借りているレンタルホスティングもあるのですが、今時のホスティング手段を使ってみたいなと。
Firebase か、GitHub Pages か、Netlify か。
ツクール入門時に早速使ってみた、Firebase が当然候補になりましたが、この３つを比べてみると、Netlify だけ、容量？が大きい。
(みんな容量100GBと書いてるけど、本当かな？？Netlify の公式サイト情報が見つからない・・・転送量が月100GB。)
Firebase か、GitHub Pagesは1GBで、RPGツクールのゲームは結構大きいので、1GBだとちょっと不安な気もして、Netlify としました。

githubのリポジトリDevLabを作ります。
元々、LICENSEファイルも、README.mdも、.gitignoreも準備されているので、特に設定はせずに、作成します。
この後、いったん別フォルダにクローンして、RPGMV-SmartDeploy-forWEBディレクトリの内容をそのままコピーする、という手順になっていますが、これは何故でしょう？？
それすると、git remote add しなくて良かったりしますか？？

```
$ git remote -v
origin  https://github.com/katai5plate/RPGMV-SmartDeploy-forWEB (fetch)
origin  https://github.com/katai5plate/RPGMV-SmartDeploy-forWEB (push)
```

確かに。そんなこともわかっていない・・・
ちなみに

```
$ git remote add origin https://github.com/JQinglong/DevLab.git
fatal: remote origin already exists.
```

へえ。

```
git remote add origin https://github.com/JQinglong/DevLab.git
git remote -v


git push -u origin master
```
としておきます。

















