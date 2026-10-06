# jlox（完成版）

講義「言語処理系」で使う、Lox 言語のインタプリタ jlox です。
教科書 [Crafting Interpreters](https://craftinginterpreters.com/) の Java 版（jlox）の完成したコードに、
入力を読む組み込み関数を足してあります。

このリポジトリはテンプレートです。「Use this template」で自分のリポジトリを作ってから使ってください。
使い方の詳しい説明は、授業の資料にあります。

## 動かしかた

Java 17 と Maven が要ります。Codespaces で開いたときにどちらも入るように、`.devcontainer/` で設定してあります。

Lox のプログラムのファイルを実行する：

```
mvn -q compile exec:java -Plox -Dexec.args="examples/fib10.lox"
```

ファイルを指定しないと、1行ずつ入力して実行するモード（REPL）になります。Ctrl-D で終わります。

```
mvn -q compile exec:java -Plox
```

テストを実行する：

```
mvn -q test-compile exec:java -Ploxtest -Dexec.args="test"
```

`-Dexec.args` にはディレクトリかファイルを指定します（`test/input` など）。
最後に失敗したテストの数が表示されます。
`test/scanning/` と `test/expressions/` は、本の途中の段階のインタプリタ向けのテストなので、
完成版では失敗します（8件）。

## 本のコードとの違い

組み込み関数を2つ足してあります。どちらも標準入力から1行を読みます。

- `read_string()`：読んだ行を文字列で返します。
- `read_number()`：読んだ行を数で返します。前後の空白は無視します。
  数として読めない行（`abc`、`1e3` など）のときは `nil` を返します。

入力が終わっていたら、どちらも `nil` を返します。たとえば次のプログラムは、入力された数をすべて足します。

```
var sum = 0;
var num = read_number();
while (num != nil) {
  sum = sum + num;
  num = read_number();
}
print sum;
```

## ファイルの置き場所

- `src/main/java/com/craftinginterpreters/lox/`：jlox のコード（本のコード）
- `src/main/java/be/guldentops/`：Geert Guldentops による、別の jlox の実装
- `test/`：Lox のテスト。`// expect:` の後に、その行で出力されるはずの値が書いてあります。
  `foo.lox` の隣に `foo.in` があれば、その中身が標準入力として渡されます。
- `src/test/java/io/github/yoriyuki/jlox/LoxTestRunner.java`：`test/` のテストを実行するプログラム

## ライセンス

[LICENSE](LICENSE) を見てください。本のコードは Robert Nystrom、`be/guldentops` の下のコードは
Geert Guldentops によるもので、どちらも MIT License です。
