# そもそもQuineって?
Quineとは自分自身を出力するプログラムのことです。

# Quineの例
## C
```c
#include <stdio.h>
int main(){char*s="#include <stdio.h>%cint main(){char*s=%c%s%c;printf(s, 10, 34, s, 34);}";printf(s, 10, 34, s, 34);}
```
(実際のデモ画像を用意)
## Ruby
```ruby
eval$s="puts('eval$s='+$s.inspect)"
```

# Quineを書いてみよう
## Ruby
### eval
### $
### inspect
## C
### printf

# 楽しい(?)Quineを書こう
## Ruby
### eval内には何を書いても良い
### joinメソッドを使ってAAを書こう
### 出力する文字列をいじろう
## C
### 短いコードを書こう
### 繰り返しを避けよう(マクロで!)

# おわり