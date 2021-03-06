# Quineで遊びましょう
LTを始めさせていただきます

## 私について
まず自己紹介です。
名前はkatoと言います。
高専生ではなく普通科の高校生です。
家なので声が出せませんから合成音声での参加とさせてください。

## Quineを見てみよう
Quineを見てみようということで、
QuineとはどのようなものなのかをRubyとCのコードを実際に実行してみてみましょう。

### Ruby
まず初めにRubyで記述されたQuineです。
一見意味のわからない文字列に見えますが、実際に実行してみると…

こちらのようにもとのプログラムと全く同じ出力がされているのがわかります。これがQuineというプログラムです。
パイプを利用して出力を実行してみると正常に動作することがわかります。

### C
ではCのコードでも見てみましょう。
Cはコンパイル言語なのでRubyとは違うアプローチを利用しています。

実際に実行してみた結果が以下の通りです。
文字が小さいですがgccでコンパイルしたものを実行すると全く同じ出力がされていることがわかります。
この画像ではパーセント記号はシェルによって表示されたものです。

## Quineを書いてみよう
ではQuineを見てみたところで、実際にQuineを書いていきましょう。

### Ruby
まずRubyでQuineを書くことを考えてみましょう。
安直に全く同じコードを出力するコードを出力するコードを書くと画像のように永遠にコードを書くことになってしまいます。
コードの中で文字列を再利用する必要がありそうだとわかりました。

そこで利用できるのがevalです。
Rubyのevalは渡された文字列をそのままRubyスクリプトとして実行します。
添付の画像はevalを利用したコードの例です。
文字列内のコードが実行されていることがわかると思います。

次にQuineのコードに出てきたドル記号を見てみましょう。
Rubyでは多くの記号に機能が割り当てられています。
Rubyはドル記号が変数の頭に付くとその変数をグローバル変数として扱います。
このようにすることでeval内でも外のスコープで定義した変数を使えるようになります。

コードの中に引用符などを含めたいこともあるでしょう。
残念ながら、バックスラッシュを使った特殊文字はそのままではうまく表示できません。
実際にうまく動かないコードを見てみましょう。
これではいけませんね。

いいですね。こうすることで文字をいい感じに表示できました。

さて、ここまで覚えたことを利用して先程のQuineを記述してみましょう。
Rubyでは変数に代入した式は代入した値と同じになるので
代入しながらevalに値を渡すことができます。
eval内でinspectした値をputしてみると画像のようになりました。いいですね

では’eval $s=‘という文字列をinspectした$sに繋げてみましょう。
これでRubyでQuineを記述することができました。
やったぜ

### C
C言語にはevalはありませんが、繰り返し部分を文字列として持っておくことでうまく記述することができます。
また、Rubyではinspectを利用することでいい感じに引用符をつけていましたがCにはありませんのでprintfを利用して文字列をフォーマットしましょう。
上記の例をみて%cなどのフォーマット文字が利用できていることを確認してください。
ASCIIコードで34は二重引用符です。

最後に変数内以外のコードを書いて、、、Quineの完成です！
Cでは基本的に改行が無視されるので整えれば自分と同じコードを出力するようになるはずです!

## 楽しいQuineを書いてみよう
このままではあまり見栄えがしませんね…残念です。
ここからは色々と楽しいQuineを描いてみましょう。

### Ruby
Rubyに戻りましょう。
先ほどのコードをよく確認してみてください。
変数内(出力前)ならどんな処理を挟んでも良いことがわかるでしょうか?試しに適当な文字列をコメントとして出力してみましょう。

実際に適当な文字を出力するコードを書いてみました。
うまくいったようです。

今回のLTではおそらく時間が足りないので紹介に留めておきますが、好きな処理を足すことのできる特徴を生かすとこのようなAAを描くことができます。
コードにはAAの形状データやデータを展開するコードなどが含まれています。
Rubyに搭載されている%w記法で配列を作り、できた配列をjoinメソッドで結合することでコード内の空白を無視しています。
このコードは資料として公開してありますので気になる方は確認して見てください。

また、このような動きのあるコードを書くこともできます。なかなか楽しい(?)ので是非手元で実行して遊んでみてください

こちらは動きのあるQuineです。
実行してみると、秒針が動くのが分かります。

### C
さてC言語でも工夫ができないか試してみましょう。
ここからは裏技的なものになるのでそこら辺ダメな方は目を瞑ってください。
先ほどのCで書かれたQuineを思い出して欲しいのですが、同じ記述を何度も書いていて冗長に見えます。
どこか削れるところはないでしょうか……

なんとgccコンパイラでは、上の赤にした部分を削除してもコンパイルが通ってしまうのです！！
main関数の型やprintfの定義がなくなってしまいましたがいい感じにコンパイルしてくれるようです。意味が分かりませんね。
Macに標準搭載されている自分のことをgccだと思っている精神異常Clangではコンパイルが通らないことがあるのでgccを用意してください。

見にくいですがきちんと動作しているようです。

ちょっとした裏技でコードを短くすることはできましたが、文字列の重複は解消していません。
今度はマクロを利用してコードを短くしてみましょう。
C言語のプリプロセッサはこのようなコードを書くことでマクロを展開してくれます。
画像はサンプルです。下の出力を見るとうまく動いていることがわかりますね。

実際にプリプロセスされたコードはどのようになっているのでしょうか。
gccのEオプションを使って確認してみましょう。
実際にマクロが展開されていることがわかります。

マクロではシャープを使うことで渡された文字列を引用符で囲ってもらうことができます。
これを利用すると。次のようなコードを書くことができます。

このアイデアはmameさんが書かれた超絶技巧プログラミングからお借りしました。
自分では残念なことに思い付かなかった…
こちらもマクロを展開した結果を見てみましょう。
プリプロセッサを使うのはちょっと反則感ありますが、これで好きなコードをはさむことができるようになりました。おめでとうございます。

## 終わりに
気の利いた文句をここで流すことはできません
申し訳ございませんでした！！！
Quine楽しいので是非皆さんも作ってみてください。
ご静聴ありがとうございました！

## 参考文献
このスライドは以下のページなどを参考に勉強し、作成しましたありがとうございます
