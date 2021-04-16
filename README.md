# はじめに

これは中学生でもphpのClassがわかるような取り組みです。
そのため、必要最小限の内容に留めている点や、細かく言うとそれちがくね？な点も含まれています。

予めご了承ください。

# 参照

- [わかりやすいYouTube](https://www.youtube.com/watch?v=wi0BAL2Pazg)
**※こちらの動画をもとに、更に簡単にしたのがこのファイルの中身です。**

- [PHP オンラインエディタ](https://3v4l.org/)
  - ↑コードを書いて`eval()`をクリックすればok

- [より詳しく](https://qiita.com/mpyw/items/41230bec5c02142ae691)

# クラスとは？

**変数と関数の集まり**です。

似たもの同士 / まとめておいたほうが便利な変数と関数をまとめたものです。

※よく、_classは設計図_とか言われますが、私は集合体として捉えています。

例えば、PHPにおいてデータベースに接続するときに

- DBに接続する関数
- この関数を利用するときに用意する変数

など、データベース接続には必要な処理がたくさんあります。

こういったものを`DBクラス`にまとめて書いておくと、利用者としてはめちゃくちゃ便利なのである！

# クラスを設計してみよう

ざっくりclassの書き方は以下のとおりです。

```php
Class クラス名
{
    $変数;
    function 関数 () {
        関数の中身;
    }
}
```

**※必ず変数や関数が必要、というわけではありません。**

このとき、classには以下のようなルールがあるあります。

- クラス名の頭は**必ず大文字**に。例えば、testというクラス名ならTestとする。これは約束だよ！
- クラスの中の変数は *プロパティ*と呼ぶ。
- クラスの中の関数は *メソッド*と呼ぶ。
  - (中学校では山田って呼ばれてたやつが高校だと山ちゃんて呼ばれる、みたいな。クラスの中だとプロパティ、メソッドになるのだ！)

よって、もっとちゃんとclassを書くと↓の感じ

```php
// Personというクラスを生成します。頭は大文字に。
Class Person
{
    // ↓プロパティ
    $age = 12;

    // メソッド↓
    function getName () {
        return '太郎';
    }
}
```

更に、classのプロパティには「クラス内だけしか使えない」ものなのか、classの外からも参照できるものなのか？
を決める必要があります。

内容としては、

- **public** : 外から参照可能
- **protected** : 外からアクセスできないが、継承先と呼ばれる、このクラスのコピー先みたいなものではok
- **private** : このクラスだけ。
があります。

ただし、この種類区別は少し難易度高いので一旦`public`を変数につけてあげましう。

*※functionにもpublicなどつけてあげます。なお、省略するとpublic扱いになります。*

```php
Class Person
{
    // ↓プロパティ, メソッドにpublicをつける。
    public $age = 12;
    
    public function getName () {
        return '太郎';
    }
}
```

# ここまでのチェック ※答えは一番下

1-1. <details><summary>クラス名前を書くときの約束事は？</summary>クラス名の頭は大文字にする！</details>
1-2. <details><summary>クラスの中の変数を何と言う？</summary>プロパティ</details>
1-3. <details><summary>クラスの中の関数を何と言う？</summary>メソッド</details>
1-4. <details><summary>geekという名前のクラスを作成してください。プロパティは`$name = taro`, メソッドは`hello`を出力するものを書いてください。</summary>※答えは一番下に書きました。</details>

# Classを利用する その1

ここまではclassの作成方法を確認しました。
Classはfunctionの様に、用意をしたら使う宣言をする必要があります。
この宣言には`New`を利用します。

`New`で作成したら変数に代入してあげます。

この製造された物体(変数に入れてあげたもの)のことを「オブジェクト」や「インスタンス」と呼びます。
**※ここではこれらの用語を区別せずに用いることにします。**

```php
Class Person
{
    public $age = 12;
    
    public function getName () {
        return '太郎';
    }
}

// ↓newして、$personインスタンスができた
$person = new Person;
```

これで、`$person`の中に、クラスの中身が入っているので中身を確認する。

```php
Class Person
{
    public $age = 12;
    public function getName () {
        return '太郎';
    }
}

$person = new Person;

// ↓var_dumpしてみる。
var_dump($person);
```

すると、以下の様に表示がされる。

```php
object(Person)#1 (1) {
  ["age"]=>
  int(12)
}
```

内容としては、
- `object(Person)`というのは型がobject型で有ることを表す。
- その中には`age`があり、その中身は`12`というのも見てわかる。



# Classを利用する その2

この`$person`は現状はまだうまく利用できません。
あくまでもプロパティ（変数）やメソッド（関数）が入ったパッケージみたいなものです。

このパッケージから中身を取り出す方法は、`アロー演算子 (->)`を利用します。
かっこよく言うと、インスタンスのプロパティを参照する演算子です。

```php
Class Person
{
    public $age = 12;
    function getName() {
        return '太郎';
    }
}

$person = new Person;

// アロー演算子で、中身を取得する。
echo $person->age;
echo $person->getName();

// 出力は、12太郎
```

- なお、このアロー演算子を利用するとき(`$person->age`)に、`age`の頭には`$`が付きません。
`$person->$age`は間違えです。

- `getName`はメソッドなので、ちゃんとカッコを付けてあげます。
`$person->getName` は間違えです。

**この様に、`->`はClass(インスタンス)の中身を取得参照する記号ということ理解できます。**

`->`を見たら、クラスの中に入っているプロパティ, メソッドを利用するために呼び出しているんだな〜とイメージしてあげてください。

# 初期動作を利用する

さて、Class作者としてはインスタンスを作成したときに自動で行ってほしい動作を設定したい場合があります。
これは `__construct`というものを利用すると設定が可能です。

※ **__は、アンダーバー２本です。**

実際に記述して、動作を確認してみましょう。

```php
Class Person
{
    // 関数の__construct.Classが生成されたら、この関数が自動で生成。
    public function __construct($test) {
        echo 'classが生成されました。' . PHP_EOL;
        echo $test;
    }

    public $age = 12;
    function getName() {
        return '太郎';
    }
}

// __constructを設定している場合、__constructの引数は↓の通りクラス名に与えればokです。
new Person('引数です');
```

ここでは、`new Person('引数です');` としたときに変数に入れることはしません。

**※`__construct`の動きが見たいので、単純に `new`しただけ ＝ インスタンスの生成をしただけです。**

このコードを実行すると、

```text
classが生成されました。
引数です
```

と表示されると思います。

これは、Classが生成された際に`__construct`が実行されたということです。

# ちなみに。
phpにて、みなさんがDBを利用する際に、`PDO`というものを利用したことがあると思います。

↓こんなやつ。
`$pdo = new PDO('mysql:dbname=gs_db;charset=utf8;host=localhost', 'root', 'root');`

今までのことをまとめると、この１行は、
- PDOのインスタンスを生成して、$pdoの中に入れている
- ()に与えられている引数は、PDOの中の`__construct`に与えられている
ということがわかります。

また↓の１行も見たことがあると思います。

`$pdo->prepare("INSERT INTO table(id, name, email, text, indate)VALUES(NULL, :name, :email, :text, sysdate())");`

これは`$pdo`の中の`prepare`というメソッドを利用しているんだな、ということがわかります。

# 問題の答え

1-4.

```php
// Gは大文字
Class Geek
{
    // publicをつける。
    public $name = 'taro';

    // メソッド名は何でも良いです。
    function hello(){
        // echoでもプリントでも何でも良いです。
        echo 'hello';
    }
}

// 利用する場合は
$geek = new geek;
```
