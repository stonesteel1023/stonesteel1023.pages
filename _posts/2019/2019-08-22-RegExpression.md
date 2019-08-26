---
layout : post
date : 2019-08-22 23:59
title : "正規表現式"
comment : true
tag : java
---

> link : https://www.sejuku.net/blog/13215

<div class="entry-content">
<p>Javaには文字列から特定のパターンを検索して、一致する文字列があるかをチェックするための正規表現あります。</p>
<p>正規表現を使えば文字列の中から数字だけを抽出したり、メールアドレスを抽出することができます。</p>
<p>この記事では、<br />
<div class="box01"></p>
<ul>
<li><b>Javaの正規表現とは</b></li>
<li><b>正規表現の使い方</b></li>
<li><b>一致するかチェックする方法</b></li>
<li><b>一致する文字を抽出する方法</b></li>
<li><b>特殊文字のエスケープ処理について</b></li>
</ul>
<p></div><br />
などの基本的な内容から、応用的な使い方に関しても解説していきます。</p>
<p>さらに、数字やメールアドレスなどのよく使う正規表現の書き方のサンプル一覧もあるので、忘れてしまったらこちらを確認してください。</p>
<p>今回はこれらの方法を覚えるために、正規表現のさまざまな使い方をわかりやすく解説します！</p>

<div id="toc_container" class="no_bullets"><p class="toc_title">この記事の目次</p><ul class="toc_list"><li><a href="#Java"><span class="toc_number toc_depth_1">1</span> Javaの正規表現とは</a></li><li><a href="#i"><span class="toc_number toc_depth_1">2</span> 正規表現の使い方</a><ul><li><a href="#matches"><span class="toc_number toc_depth_2">2.1</span> matchesメソッドでチェックする方法</a></li><li><a href="#Pattern"><span class="toc_number toc_depth_2">2.2</span> パターンの作り方(Patternクラス)</a></li><li><a href="#Matcher"><span class="toc_number toc_depth_2">2.3</span> 一致するかチェックする方法(Matcherクラス)</a></li><li><a href="#i-2"><span class="toc_number toc_depth_2">2.4</span> 一致する複数の文字をすべて抽出する方法</a></li></ul></li><li><a href="#i-3"><span class="toc_number toc_depth_1">3</span> 特殊文字のエスケープ処理について</a></li><li><a href="#i-4"><span class="toc_number toc_depth_1">4</span> 正規表現の書き方サンプル一覧</a><ul><li><a href="#i-5"><span class="toc_number toc_depth_2">4.1</span> 数字のチェック</a></li><li><a href="#i-6"><span class="toc_number toc_depth_2">4.2</span> 英数字のチェック</a></li><li><a href="#i-7"><span class="toc_number toc_depth_2">4.3</span> 日付のチェック</a></li><li><a href="#i-8"><span class="toc_number toc_depth_2">4.4</span> 電話番号のチェック</a></li><li><a href="#i-9"><span class="toc_number toc_depth_2">4.5</span> 郵便番号のチェック</a></li><li><a href="#IPIPv4_IPv6"><span class="toc_number toc_depth_2">4.6</span> IPアドレス(IPv4, IPv6)のチェック</a></li><li><a href="#i-10"><span class="toc_number toc_depth_2">4.7</span> メールアドレスのチェック</a></li><li><a href="#URL"><span class="toc_number toc_depth_2">4.8</span> URLのチェック</a></li></ul></li><li><a href="#i-11"><span class="toc_number toc_depth_1">5</span> まとめ</a></li></ul></div>
<h2><span id="Java">Javaの正規表現とは</span></h2>
<p><span class="green_strong">正規表現</span>とは<b>文字列のパターンを一つの形式でまとめて表現するために使うもののこと</b>です。</p>
<p>例えば、文字列の中から&#8221;123-4567”のような郵便番号を検索したい場合には次のような正規表現の記述を使います。</p>
<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bc3768885463" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">

<div class="crayon-main" style="">
<table class="crayon-table">
<tr class="crayon-row">
<td class="crayon-nums " data-settings="show">
<div class="crayon-nums-content" style="font-size: 15px !important; line-height: 22px !important;"><div class="crayon-num" data-line="crayon-5d5de958a1bc3768885463-1">1</div></div>
</td>
<td class="crayon-code"><div class="crayon-pre" style="font-size: 15px !important; line-height: 22px !important; -moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4;"><div class="crayon-line" id="crayon-5d5de958a1bc3768885463-1"><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9</span><span class="crayon-sy">]</span><span class="crayon-sy">{</span><span class="crayon-cn">3</span><span class="crayon-sy">}</span>-<span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9</span><span class="crayon-sy">]</span><span class="crayon-sy">{</span><span class="crayon-cn">4</span><span class="crayon-sy">}</span></div></div></td>
</tr>
</table>
</div>
</div>

<p>
この正規表現の記述の意味は、0から9の数字が3回続いた後に”-”(ハイフン)があり、その次に0から9の数字が4回続いたら郵便番号だと判断して一致する判定をします。</p>
<p>主な正規表現の記号と意味については以下の表の通りです。<br />
<table id="tablepress-64" class="tablepress tablepress-id-64">
<thead>
<tr class="row-1 odd">
<th class="column-1">記号</th><th class="column-2">記号の説明</th><th class="column-3">例</th><th class="column-4">例の説明</th>
</tr>
</thead>
<tbody class="row-hover">
<tr class="row-2 even">
<td class="column-1">.</td><td class="column-2">任意の1文字。改行文字は除く。</td><td class="column-3">.+</td><td class="column-4">任意の文字列</td>
</tr>
<tr class="row-3 odd">
<td class="column-1">*</td><td class="column-2">直前の1文字の0回以上の繰り返しと一致</td><td class="column-3">hoge*</td><td class="column-4">hogeもしくはhogee...と一致</td>
</tr>
<tr class="row-4 even">
<td class="column-1">^</td><td class="column-2">行の先頭</td><td class="column-3">^[0-9]</td><td class="column-4">行頭が数字</td>
</tr>
<tr class="row-5 odd">
<td class="column-1">$</td><td class="column-2">行の末尾</td><td class="column-3">^.{10}$</td><td class="column-4">10文字の行</td>
</tr>
<tr class="row-6 even">
<td class="column-1">[ ]</td><td class="column-2">カッコ内の任意の1文字と一致。「-」で範囲指定可。</td><td class="column-3">[a-z]</td><td class="column-4">小文字のアルファベット1文字と一致</td>
</tr>
<tr class="row-7 odd">
<td class="column-1">[^ ]</td><td class="column-2">カッコ内の任意の1文字と不一致。「-」で範囲指定可。</td><td class="column-3">[^A-Z]</td><td class="column-4">大文字のアルファベット以外</td>
</tr>
<tr class="row-8 even">
<td class="column-1">+</td><td class="column-2">直前の文字の1個以上の繰り返しと一致</td><td class="column-3">hoge+</td><td class="column-4">hogee...と一致</td>
</tr>
<tr class="row-9 odd">
<td class="column-1">?</td><td class="column-2">直前の文字が0個または1個の場合に一致</td><td class="column-3">hoge?</td><td class="column-4">hogeもしくはhogと一致</td>
</tr>
<tr class="row-10 even">
<td class="column-1">{ }</td><td class="column-2">カッコ内の数値の繰り返しと一致</td><td class="column-3">{n}</td><td class="column-4">直前の文字のn個の繰り返しと一致</td>
</tr>
<tr class="row-11 odd">
<td class="column-1"></td><td class="column-2">同上</td><td class="column-3">{,n}</td><td class="column-4">直前の文字のn個以下の繰り返しと一致</td>
</tr>
<tr class="row-12 even">
<td class="column-1"></td><td class="column-2">同上</td><td class="column-3">{m,}</td><td class="column-4">直前の文字のm個以上の繰り返しと一致</td>
</tr>
<tr class="row-13 odd">
<td class="column-1"></td><td class="column-2">同上</td><td class="column-3">{m,n}</td><td class="column-4">直前の文字のm個以上、n個以下の繰り返しと一致</td>
</tr>
<tr class="row-14 even">
<td class="column-1">|</td><td class="column-2">直前、直後どちらかのパターンに一致</td><td class="column-3">hoge|piyo</td><td class="column-4">hogeまたはpiyo</td>
</tr>
<tr class="row-15 odd">
<td class="column-1">( )</td><td class="column-2">カッコ内をグループ化。マッチした内容は参照可。</td><td class="column-3">ー</td><td class="column-4">ー</td>
</tr>
</tbody>
</table>
</p>
<p>ここでは正規表現の使い方をサンプルコードを交えて解説していきます。</p>
<h3><span id="matches">matchesメソッドでチェックする方法</span></h3>
<p>初めに正規表現で指定したパターンと一致するかを確認する簡単な方法として、Stringクラスの<span class="green_strong">matchesメソッド</span>の使い方を解説します。</p>
<p>matchesメソッドは指定した文字列と正規表現が完全に一致した場合に”true”を返します。一部が一致しただけでは”false&#8221;を返すので注意してください！</p>
<p>Stringクラスのmatchesメソッドの使い方を次のプログラムで確認してみましょう。</p>

### matcheメソッドのサンプルコード

<div id="crayon-5d5de958a1bc9755813901" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">

```java
public class Main {
    public static void main(String[] args) {

        // 検索する文字列を用意
        String str = "東京都千代田区 123-4567";

        System.out.println(str.matches(".*[0-9]{3}-[0-9]{4}.*"));
        System.out.println(str.matches("[0-9]{3}-[0-9]{4}"));

        System.out.println(str.matches(".*123-4567.*"));
        System.out.println(str.matches("123-4567"));
    }
}
```

### 実行結果

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bcb432730109" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">

<div class="crayon-main" style="">
<table class="crayon-table">
<tr class="crayon-row">
<td class="crayon-nums " data-settings="hide">
<div class="crayon-nums-content" style="font-size: 15px !important; line-height: 22px !important;"><div class="crayon-num" data-line="crayon-5d5de958a1bcb432730109-1">1</div><div class="crayon-num" data-line="crayon-5d5de958a1bcb432730109-2">2</div><div class="crayon-num" data-line="crayon-5d5de958a1bcb432730109-3">3</div><div class="crayon-num" data-line="crayon-5d5de958a1bcb432730109-4">4</div></div>
</td>
<td class="crayon-code"><div class="crayon-pre" style="font-size: 15px !important; line-height: 22px !important; -moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4;"><div class="crayon-line" id="crayon-5d5de958a1bcb432730109-1"><span class="crayon-t">true</span></div><div class="crayon-line" id="crayon-5d5de958a1bcb432730109-2"><span class="crayon-t">false</span></div><div class="crayon-line" id="crayon-5d5de958a1bcb432730109-3"><span class="crayon-t">true</span></div><div class="crayon-line" id="crayon-5d5de958a1bcb432730109-4"><span class="crayon-t">false</span></div></div></td>
</tr>
</table>
</div>
</div>

このプログラムではmatchesメソッドを使って、正規表現で表現した文字列が含まれているかを判定しています。matchesメソッドは完全一致のときに”true”を返すので、検索したい文字列の前後に他の文字があると”false”を返します。
<p>そのため、正規表現のパターンの前後に「任意の文字がいくつかある」という意味の「.*」を追加して完全一致させることで”true”を返しています。</p>

<h3><span id="Pattern">パターンの作り方(Patternクラス)</span></h3>
<p>ここではPatternクラスを使って正規表現のパターンを作る方法を解説します。</p>
<p>正規表現のパターンオブジェクトを作るには、Patternクラスのcompileメソッドの引数に正規表現のパターンを指定します。</p>
<p>次に、Patternクラスのmatcherメソッドの引数にパターンとマッチさせる文字列を指定してMatcherオブジェクトを作成します。</p>
<p>正規表現のパターンを作る方法を次のプログラムで確認してみましょう。</p>

### 正規表現のパターンの作り方のサンプルコード

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bcd173085216" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">

```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class Main {
    public static void main(String[] args) {

        // 検索する文字列を用意
        String str = "東京都千代田区 123-4567";

        // 正規表現のパターンを作成
        Pattern p = Pattern.compile("[0-9]{3}-[0-9]{4}");
        Matcher m = p.matcher(str);
    }
}
```

このプログラムでは、文字列「東京都千代田区 123-4567」と正規表現のパターン「[0-9]{3}-[0-9]{4}」のMatcherオブジェクトを作成しています。</p>
<p>このMatcherオブジェクトを使ってチェックや抽出する方法をこれから解説していきます！</p>
<h3><span id="Matcher">一致するかチェックする方法(Matcherクラス)</span></h3>
<p>ここではMatcherクラスのfindメソッドを使って、<span class="red_strong">文字列が正規表現のパターンに一致するかをチェックする方法</span>を解説します。</p>
<p>Matcherクラスのfindメソッドは、文字列の中に正規表現のパターンが含まれる場合に”true”を返し、それ以外の場合には”false”を返します。</p>
<p>次のプログラムでfindメソッドの使い方を確認してみましょう。</p>

### findメソッドの使い方のサンプルコード

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bcf238412064" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">

```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class Main {
    public static void main(String[] args) {

        // 検索する文字列を用意
        String str = "東京都千代田区 123-4567";

        // 正規表現のパターンを作成
        Pattern p = Pattern.compile("[0-9]{3}-[0-9]{4}");
        Matcher m = p.matcher(str);

        System.out.println(m.find());
    }
}
```

### 実行結果

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bd0945552279" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">

<div class="crayon-main" style="">
<table class="crayon-table">
<tr class="crayon-row">
<td class="crayon-nums " data-settings="hide">
<div class="crayon-nums-content" style="font-size: 15px !important; line-height: 22px !important;"><div class="crayon-num" data-line="crayon-5d5de958a1bd0945552279-1">1</div></div>
</td>
<td class="crayon-code"><div class="crayon-pre" style="font-size: 15px !important; line-height: 22px !important; -moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4;"><div class="crayon-line" id="crayon-5d5de958a1bd0945552279-1"><span class="crayon-t">true</span></div></div></td>
</tr>
</table>
</div>
</div>

このプログラムでは文字列「東京都千代田区 123-4567」の中に、正規表現の郵便番号のパターン「[0-9]{3}-[0-9]{4}」が含まれているため”true”を返しています。</p>
<h3><span id="i-2">一致する複数の文字をすべて抽出する方法</span></h3>
<p>ここではMatcherクラスのgroupメソッドを使って、<span class="red_strong">正規表現のパターンに一致した文字列を抽出する方法</span>を解説します。</p>
<p>groupメソッドはfindメソッドで一致した文字列を返します。次のプログラムで確認してみましょう。</p>

### groupメソッドのサンプルコード

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bd2500274253" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">

```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class Main {
    public static void main(String[] args) {

        // 検索する文字列を用意
        String str = "東京都千代田区 123-4567, 東京都渋谷区 111-2233";

        // 正規表現のパターンを作成
        Pattern p = Pattern.compile("[0-9]{3}-[0-9]{4}");
        Matcher m = p.matcher(str);

        while (m.find()) {
            System.out.println("一致した部分は : " + m.group());
        }
    }
}
``` 

### 実行結果

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bd4381980823" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">

<div class="crayon-main" style="">
<table class="crayon-table">
<tr class="crayon-row">
<td class="crayon-nums " data-settings="hide">
<div class="crayon-nums-content" style="font-size: 15px !important; line-height: 22px !important;"><div class="crayon-num" data-line="crayon-5d5de958a1bd4381980823-1">1</div><div class="crayon-num" data-line="crayon-5d5de958a1bd4381980823-2">2</div></div>
</td>
<td class="crayon-code"><div class="crayon-pre" style="font-size: 15px !important; line-height: 22px !important; -moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4;"><div class="crayon-line" id="crayon-5d5de958a1bd4381980823-1">一致した部分は<span class="crayon-h"> </span><span class="crayon-sy">:</span><span class="crayon-h"> </span><span class="crayon-cn">123</span>-<span class="crayon-cn">4567</span></div><div class="crayon-line" id="crayon-5d5de958a1bd4381980823-2">一致した部分は<span class="crayon-h"> </span><span class="crayon-sy">:</span><span class="crayon-h"> </span><span class="crayon-cn">111</span>-<span class="crayon-cn">2233</span></div></div></td>
</tr>
</table>
</div>
</div>

このプログラムでは、findメソッドを使って郵便番号に一致した文字列をgroupメソッドで抽出して表示しています。</p>
<p>findメソッドは一致した場合に”true”を返すので、while文のループで一致する文字列がなくなるまで検索と表示を繰り返しています。このプログラムの実行結果から、正規表現を使って一致する複数の文字をすべて抽出できることが確認できました！</p>
<h2><span id="i-3">特殊文字のエスケープ処理について</span></h2>
<p>正規表現で使う特殊な意味を持った記号をただの文字として使う場合には、<span class="green_strong">エスケープ処理</span>をする必要があります。</p>
<p>エスケープ処理をするには記号の前に\(バックスラッシュ)を付けるだけでOKです。ただし、\(バックスラッシュ)を使うには「\\」のように2回続けて記述する必要があるので注意してください！</p>
<p>この\(バックスラッシュ)は環境によっては￥(円記号)になりますがどちらでも問題ありません。エスケープ処理が必要な記号は次のとおりです。</p>

`\ * + . ? { } ( ) [ ] ^ $ &#8211; |`

<p>エスケープ処理の例は以下のとおりです。</p>
<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bd6201799099" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">

<div class="crayon-main" style="">
<table class="crayon-table">
<tr class="crayon-row">
<td class="crayon-nums " data-settings="hide">
<div class="crayon-nums-content" style="font-size: 15px !important; line-height: 22px !important;"><div class="crayon-num" data-line="crayon-5d5de958a1bd6201799099-1">1</div><div class="crayon-num" data-line="crayon-5d5de958a1bd6201799099-2">2</div><div class="crayon-num" data-line="crayon-5d5de958a1bd6201799099-3">3</div></div>
</td>
<td class="crayon-code"><div class="crayon-pre" style="font-size: 15px !important; line-height: 22px !important; -moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4;"><div class="crayon-line" id="crayon-5d5de958a1bd6201799099-1"><span class="crayon-sy">\</span><span class="crayon-h">&nbsp;&nbsp;</span>→<span class="crayon-h">&nbsp;&nbsp;</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span></div><div class="crayon-line" id="crayon-5d5de958a1bd6201799099-2">*<span class="crayon-h">&nbsp;&nbsp;</span>→<span class="crayon-h">&nbsp;&nbsp;</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span>*</div><div class="crayon-line" id="crayon-5d5de958a1bd6201799099-3"><span class="crayon-sy">{</span><span class="crayon-sy">}</span><span class="crayon-h"> </span>→<span class="crayon-h">&nbsp;&nbsp;</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-sy">{</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-sy">}</span></div></div></td>
</tr>
</table>
</div>
</div>

<p>
<h2><span id="i-4">正規表現の書き方サンプル一覧</span></h2>
<p>ここでは正規表現でよく使われるパターンのサンプルを解説します。</p>
<h3><span id="i-5">数字のチェック</span></h3>
<p>数字のチェックをする正規表現のパターンは次のとおりです。</p>
<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bd7335882549" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">

`^[0-9]+$`

<div class="crayon-main" style="">
<table class="crayon-table">
<tr class="crayon-row">
<td class="crayon-nums " data-settings="hide">
<div class="crayon-nums-content" style="font-size: 15px !important; line-height: 22px !important;"><div class="crayon-num" data-line="crayon-5d5de958a1bd7335882549-1">1</div></div>
</td>
<td class="crayon-code"><div class="crayon-pre" style="font-size: 15px !important; line-height: 22px !important; -moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4;"><div class="crayon-line" id="crayon-5d5de958a1bd7335882549-1">^<span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9</span><span class="crayon-sy">]</span>+<span class="crayon-sy">$</span></div></div></td>
</tr>
</table>
</div>
</div>

<p>
次のプログラムで確認してみましょう。</p>

### 数字のチェックをするサンプルコード

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bd9496513020" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">
    
```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;
 
public class Main {
    public static void main(String[] args) {
 
        // 検索する文字列を用意
        String str = "123";
 
        // 正規表現のパターンを作成
        Pattern p = Pattern.compile("^[0-9]+$");
        Matcher m = p.matcher(str);
 
        System.out.println(m.find());
    }
}
``` 

### 実行結果

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bda012944083" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">

<div class="crayon-main" style="">
<table class="crayon-table">
<tr class="crayon-row">
<td class="crayon-nums " data-settings="hide">
<div class="crayon-nums-content" style="font-size: 15px !important; line-height: 22px !important;"><div class="crayon-num" data-line="crayon-5d5de958a1bda012944083-1">1</div></div>
</td>
<td class="crayon-code"><div class="crayon-pre" style="font-size: 15px !important; line-height: 22px !important; -moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4;"><div class="crayon-line" id="crayon-5d5de958a1bda012944083-1"><span class="crayon-t">true</span></div></div></td>
</tr>
</table>
</div>
</div>

<h3><span id="i-6">英数字のチェック</span></h3>
<p>英数字のチェックをする正規表現のパターンは次のとおりです。</p>
<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bdb656745243" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">

<div class="crayon-main" style="">
<table class="crayon-table">
<tr class="crayon-row">
<td class="crayon-nums " data-settings="hide">
<div class="crayon-nums-content" style="font-size: 15px !important; line-height: 22px !important;"><div class="crayon-num" data-line="crayon-5d5de958a1bdb656745243-1">1</div></div>
</td>
<td class="crayon-code"><div class="crayon-pre" style="font-size: 15px !important; line-height: 22px !important; -moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4;"><div class="crayon-line" id="crayon-5d5de958a1bdb656745243-1">^<span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9a</span>-<span class="crayon-i">zA</span>-<span class="crayon-i">Z</span><span class="crayon-sy">]</span>+<span class="crayon-sy">$</span></div></div></td>
</tr>
</table>
</div>
</div>

<p>
次のプログラムで確認してみましょう。</p>

### 英数字のチェックをするサンプルコード

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bdd902497003" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">
    
```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;
 
public class Main {
    public static void main(String[] args) {
 
        // 検索する文字列を用意
        String str = "SAMURAI123";
 
        // 正規表現のパターンを作成
        Pattern p = Pattern.compile("^[0-9a-zA-Z]+$");
        Matcher m = p.matcher(str);
 
        System.out.println(m.find());
    }
}
```

### 実行結果

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bde110765501" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">

<div class="crayon-main" style="">
<table class="crayon-table">
<tr class="crayon-row">
<td class="crayon-nums " data-settings="hide">
<div class="crayon-nums-content" style="font-size: 15px !important; line-height: 22px !important;"><div class="crayon-num" data-line="crayon-5d5de958a1bde110765501-1">1</div></div>
</td>
<td class="crayon-code"><div class="crayon-pre" style="font-size: 15px !important; line-height: 22px !important; -moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4;"><div class="crayon-line" id="crayon-5d5de958a1bde110765501-1"><span class="crayon-t">true</span></div></div></td>
</tr>
</table>
</div>
</div>

<h3><span id="i-7">日付のチェック</span></h3>
<p>日付のチェックをする正規表現のパターンは次のとおりです。</p>
<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bdf012796726" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">

<div class="crayon-main" style="">
<table class="crayon-table">
<tr class="crayon-row">
<td class="crayon-nums " data-settings="hide">
<div class="crayon-nums-content" style="font-size: 15px !important; line-height: 22px !important;"><div class="crayon-num" data-line="crayon-5d5de958a1bdf012796726-1">1</div></div>
</td>
<td class="crayon-code"><div class="crayon-pre" style="font-size: 15px !important; line-height: 22px !important; -moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4;"><div class="crayon-line" id="crayon-5d5de958a1bdf012796726-1">^<span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9</span><span class="crayon-sy">]</span><span class="crayon-sy">{</span><span class="crayon-cn">4</span><span class="crayon-sy">}</span>/<span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9</span><span class="crayon-sy">]</span><span class="crayon-sy">{</span><span class="crayon-cn">2</span><span class="crayon-sy">}</span>/<span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9</span><span class="crayon-sy">]</span><span class="crayon-sy">{</span><span class="crayon-cn">2</span><span class="crayon-sy">}</span><span class="crayon-sy">$</span></div></div></td>
</tr>
</table>
</div>
</div>

<p>
日付を正規表現で厳密にチェックしようとすると、うるう年などがあり大変なので、<b>厳密にチェックする場合は日付型のオブジェクトにしてチェックする方が実用的</b>です。</p>

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1be1187390847" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">
    
```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;
 
public class Main {
    public static void main(String[] args) {
 
        // 検索する文字列を用意
        String str = "2017/06/28";
 
        // 正規表現のパターンを作成
        Pattern p = Pattern.compile("^[0-9]{4}/[0-9]{2}/[0-9]{2}$");
        Matcher m = p.matcher(str);
 
        System.out.println(m.find());
    }
}
```

### 実行結果

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1be2174456946" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">

<div class="crayon-main" style="">
<table class="crayon-table">
<tr class="crayon-row">
<td class="crayon-nums " data-settings="hide">
<div class="crayon-nums-content" style="font-size: 15px !important; line-height: 22px !important;"><div class="crayon-num" data-line="crayon-5d5de958a1be2174456946-1">1</div></div>
</td>
<td class="crayon-code"><div class="crayon-pre" style="font-size: 15px !important; line-height: 22px !important; -moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4;"><div class="crayon-line" id="crayon-5d5de958a1be2174456946-1"><span class="crayon-t">true</span></div></div></td>
</tr>
</table>
</div>
</div>

<h3><span id="i-8">電話番号のチェック</span></h3>
<p>電話番号のチェックをする正規表現のパターンは次のとおりです。</p>
<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1be4997122816" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">

<div class="crayon-main" style="">
<table class="crayon-table">
<tr class="crayon-row">
<td class="crayon-nums " data-settings="hide">
<div class="crayon-nums-content" style="font-size: 15px !important; line-height: 22px !important;"><div class="crayon-num" data-line="crayon-5d5de958a1be4997122816-1">1</div></div>
</td>
<td class="crayon-code"><div class="crayon-pre" style="font-size: 15px !important; line-height: 22px !important; -moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4;"><div class="crayon-line" id="crayon-5d5de958a1be4997122816-1">^<span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9</span><span class="crayon-sy">]</span><span class="crayon-sy">{</span><span class="crayon-cn">3</span><span class="crayon-sy">}</span>-<span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9</span><span class="crayon-sy">]</span><span class="crayon-sy">{</span><span class="crayon-cn">4</span><span class="crayon-sy">}</span>-<span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9</span><span class="crayon-sy">]</span><span class="crayon-sy">{</span><span class="crayon-cn">4</span><span class="crayon-sy">}</span><span class="crayon-sy">$</span></div></div></td>
</tr>
</table>
</div>
</div>

<p>
区切りや桁数を変えることで他のパターンの電話番号にも対応できます。</p>
<p>次のプログラムで確認してみましょう。</p>

### 電話番号のチェックをするサンプルコード

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1be5105843512" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">
    
```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;
 
public class Main {
    public static void main(String[] args) {
 
        // 検索する文字列を用意
        String str = "090-1234-5678";
 
        // 正規表現のパターンを作成
        Pattern p = Pattern.compile("^[0-9]{3}-[0-9]{4}-[0-9]{4}$");
        Matcher m = p.matcher(str);
 
        System.out.println(m.find());
    }
}
```

### 実行結果

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1be6217131078" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">

<div class="crayon-main" style="">
<table class="crayon-table">
<tr class="crayon-row">
<td class="crayon-nums " data-settings="hide">
<div class="crayon-nums-content" style="font-size: 15px !important; line-height: 22px !important;"><div class="crayon-num" data-line="crayon-5d5de958a1be6217131078-1">1</div></div>
</td>
<td class="crayon-code"><div class="crayon-pre" style="font-size: 15px !important; line-height: 22px !important; -moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4;"><div class="crayon-line" id="crayon-5d5de958a1be6217131078-1"><span class="crayon-t">true</span></div></div></td>
</tr>
</table>
</div>
</div>

<h3><span id="i-9">郵便番号のチェック</span></h3>
<p>郵便番号のチェックをする正規表現のパターンは次のとおりです。</p>
<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1be8445941819" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">

<div class="crayon-main" style="">
<table class="crayon-table">
<tr class="crayon-row">
<td class="crayon-nums " data-settings="hide">
<div class="crayon-nums-content" style="font-size: 15px !important; line-height: 22px !important;"><div class="crayon-num" data-line="crayon-5d5de958a1be8445941819-1">1</div></div>
</td>
<td class="crayon-code"><div class="crayon-pre" style="font-size: 15px !important; line-height: 22px !important; -moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4;"><div class="crayon-line" id="crayon-5d5de958a1be8445941819-1">^<span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9</span><span class="crayon-sy">]</span><span class="crayon-sy">{</span><span class="crayon-cn">3</span><span class="crayon-sy">}</span>-<span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9</span><span class="crayon-sy">]</span><span class="crayon-sy">{</span><span class="crayon-cn">4</span><span class="crayon-sy">}</span><span class="crayon-sy">$</span></div></div></td>
</tr>
</table>
</div>
</div>

<p>
次のプログラムで確認してみましょう。</p>

### 郵便番号のチェックをするサンプルコード

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1be9304545844" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">
    
```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;
 
public class Main {
    public static void main(String[] args) {
 
        // 検索する文字列を用意
        String str = "123-4567";
 
        // 正規表現のパターンを作成
        Pattern p = Pattern.compile("^[0-9]{3}-[0-9]{4}$");
        Matcher m = p.matcher(str);
 
        System.out.println(m.find());
    }
}
``` 

### 実行結果

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bea430642836" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">

<div class="crayon-main" style="">
<table class="crayon-table">
<tr class="crayon-row">
<td class="crayon-nums " data-settings="hide">
<div class="crayon-nums-content" style="font-size: 15px !important; line-height: 22px !important;"><div class="crayon-num" data-line="crayon-5d5de958a1bea430642836-1">1</div></div>
</td>
<td class="crayon-code"><div class="crayon-pre" style="font-size: 15px !important; line-height: 22px !important; -moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4;"><div class="crayon-line" id="crayon-5d5de958a1bea430642836-1"><span class="crayon-t">true</span></div></div></td>
</tr>
</table>
</div>
</div>

<h3><span id="IPIPv4_IPv6">IPアドレス(IPv4, IPv6)のチェック</span></h3>
<p>ここでは<span class="red_strong">IPアドレスのチェックの方法</span>をIPv4とIPv6でそれぞれ解説します。</p>
<h4>IPv4のIPアドレスをチェック</h4>
<p>IPv4のIPアドレスのチェックをする正規表現のパターンは次のとおりです。</p>
<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bec081337518" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">

<div class="crayon-main" style="">
<table class="crayon-table">
<tr class="crayon-row">
<td class="crayon-nums " data-settings="hide">
<div class="crayon-nums-content" style="font-size: 15px !important; line-height: 22px !important;"><div class="crayon-num" data-line="crayon-5d5de958a1bec081337518-1">1</div></div>
</td>
<td class="crayon-code"><div class="crayon-pre" style="font-size: 15px !important; line-height: 22px !important; -moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4;"><div class="crayon-line" id="crayon-5d5de958a1bec081337518-1">^<span class="crayon-sy">(</span><span class="crayon-sy">(</span><span class="crayon-sy">[</span><span class="crayon-cn">1</span>-<span class="crayon-cn">9</span><span class="crayon-sy">]</span><span class="crayon-sy">?</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9</span><span class="crayon-sy">]</span><span class="crayon-sy">|</span><span class="crayon-cn">1</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9</span><span class="crayon-sy">]</span><span class="crayon-sy">{</span><span class="crayon-cn">2</span><span class="crayon-sy">}</span><span class="crayon-sy">|</span><span class="crayon-cn">2</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">4</span><span class="crayon-sy">]</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9</span><span class="crayon-sy">]</span><span class="crayon-sy">|</span><span class="crayon-cn">25</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">5</span><span class="crayon-sy">]</span><span class="crayon-sy">)</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-sy">.</span><span class="crayon-sy">)</span><span class="crayon-sy">{</span><span class="crayon-cn">3</span><span class="crayon-sy">}</span><span class="crayon-sy">(</span><span class="crayon-sy">[</span><span class="crayon-cn">1</span>-<span class="crayon-cn">9</span><span class="crayon-sy">]</span><span class="crayon-sy">?</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9</span><span class="crayon-sy">]</span><span class="crayon-sy">|</span><span class="crayon-cn">1</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9</span><span class="crayon-sy">]</span><span class="crayon-sy">{</span><span class="crayon-cn">2</span><span class="crayon-sy">}</span><span class="crayon-sy">|</span><span class="crayon-cn">2</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">4</span><span class="crayon-sy">]</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9</span><span class="crayon-sy">]</span><span class="crayon-sy">|</span><span class="crayon-cn">25</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">5</span><span class="crayon-sy">]</span><span class="crayon-sy">)</span><span class="crayon-sy">$</span></div></div></td>
</tr>
</table>
</div>
</div>

<p>
IPv4のIPアドレスは、.(ドット)で区切られた0～255の4つの値なので、その範囲の値かどうかをチェックしています。</p>
<p>次のプログラムで確認してみましょう。</p>

### IPv4のアドレスのチェックをするサンプルコード

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bed790164709" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">

    
```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;
 
public class Main {
    public static void main(String[] args) {
 
        // 検索する文字列を用意
        String str = "192.168.0.10";
 
        // 正規表現のパターンを作成
        Pattern p = Pattern.compile(
                "^(([1-9]?[0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])\\.){3}([1-9]?[0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])$");
 
        Matcher m = p.matcher(str);
        System.out.println(m.find());
 
    }
}
```

### 実行結果

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bef147717154" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">

<div class="crayon-main" style="">
<table class="crayon-table">
<tr class="crayon-row">
<td class="crayon-nums " data-settings="hide">
<div class="crayon-nums-content" style="font-size: 15px !important; line-height: 22px !important;"><div class="crayon-num" data-line="crayon-5d5de958a1bef147717154-1">1</div></div>
</td>
<td class="crayon-code"><div class="crayon-pre" style="font-size: 15px !important; line-height: 22px !important; -moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4;"><div class="crayon-line" id="crayon-5d5de958a1bef147717154-1"><span class="crayon-t">true</span></div></div></td>
</tr>
</table>
</div>
</div>

<h4>IPv6のIPアドレスをチェック</h4>
<p>IPv6のIPアドレスのチェックをする正規表現のパターンは次のとおりです。</p>
<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bf0387044596" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">

<div class="crayon-main" style="">
<table class="crayon-table">
<tr class="crayon-row">
<td class="crayon-nums " data-settings="hide">
<div class="crayon-nums-content" style="font-size: 15px !important; line-height: 22px !important;"><div class="crayon-num" data-line="crayon-5d5de958a1bf0387044596-1">1</div></div>
</td>
<td class="crayon-code"><div class="crayon-pre" style="font-size: 15px !important; line-height: 22px !important; -moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4;"><div class="crayon-line" id="crayon-5d5de958a1bf0387044596-1">^<span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-e">s</span>*<span class="crayon-sy">(</span><span class="crayon-sy">(</span><span class="crayon-sy">(</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9A</span>-<span class="crayon-e">Fa</span>-<span class="crayon-e">f</span><span class="crayon-sy">]</span><span class="crayon-sy">{</span><span class="crayon-cn">1</span><span class="crayon-sy">,</span><span class="crayon-cn">4</span><span class="crayon-sy">}</span><span class="crayon-sy">:</span><span class="crayon-sy">)</span><span class="crayon-sy">{</span><span class="crayon-cn">7</span><span class="crayon-sy">}</span><span class="crayon-sy">(</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9A</span>-<span class="crayon-e">Fa</span>-<span class="crayon-e">f</span><span class="crayon-sy">]</span><span class="crayon-sy">{</span><span class="crayon-cn">1</span><span class="crayon-sy">,</span><span class="crayon-cn">4</span><span class="crayon-sy">}</span><span class="crayon-sy">|</span><span class="crayon-sy">:</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">|</span><span class="crayon-sy">(</span><span class="crayon-sy">(</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9A</span>-<span class="crayon-e">Fa</span>-<span class="crayon-e">f</span><span class="crayon-sy">]</span><span class="crayon-sy">{</span><span class="crayon-cn">1</span><span class="crayon-sy">,</span><span class="crayon-cn">4</span><span class="crayon-sy">}</span><span class="crayon-sy">:</span><span class="crayon-sy">)</span><span class="crayon-sy">{</span><span class="crayon-cn">6</span><span class="crayon-sy">}</span><span class="crayon-sy">(</span><span class="crayon-sy">:</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9A</span>-<span class="crayon-e">Fa</span>-<span class="crayon-e">f</span><span class="crayon-sy">]</span><span class="crayon-sy">{</span><span class="crayon-cn">1</span><span class="crayon-sy">,</span><span class="crayon-cn">4</span><span class="crayon-sy">}</span><span class="crayon-sy">|</span><span class="crayon-sy">(</span><span class="crayon-sy">(</span><span class="crayon-cn">25</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">5</span><span class="crayon-sy">]</span><span class="crayon-sy">|</span><span class="crayon-cn">2</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">4</span><span class="crayon-sy">]</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">|</span><span class="crayon-cn">1</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">|</span><span class="crayon-sy">[</span><span class="crayon-cn">1</span>-<span class="crayon-cn">9</span><span class="crayon-sy">]</span><span class="crayon-sy">?</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">)</span><span class="crayon-sy">(</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-sy">.</span><span class="crayon-sy">(</span><span class="crayon-cn">25</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">5</span><span class="crayon-sy">]</span><span class="crayon-sy">|</span><span class="crayon-cn">2</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">4</span><span class="crayon-sy">]</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">|</span><span class="crayon-cn">1</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">|</span><span class="crayon-sy">[</span><span class="crayon-cn">1</span>-<span class="crayon-cn">9</span><span class="crayon-sy">]</span><span class="crayon-sy">?</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">{</span><span class="crayon-cn">3</span><span class="crayon-sy">}</span><span class="crayon-sy">)</span><span class="crayon-sy">|</span><span class="crayon-sy">:</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">|</span><span class="crayon-sy">(</span><span class="crayon-sy">(</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9A</span>-<span class="crayon-e">Fa</span>-<span class="crayon-e">f</span><span class="crayon-sy">]</span><span class="crayon-sy">{</span><span class="crayon-cn">1</span><span class="crayon-sy">,</span><span class="crayon-cn">4</span><span class="crayon-sy">}</span><span class="crayon-sy">:</span><span class="crayon-sy">)</span><span class="crayon-sy">{</span><span class="crayon-cn">5</span><span class="crayon-sy">}</span><span class="crayon-sy">(</span><span class="crayon-sy">(</span><span class="crayon-sy">(</span><span class="crayon-sy">:</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9A</span>-<span class="crayon-e">Fa</span>-<span class="crayon-e">f</span><span class="crayon-sy">]</span><span class="crayon-sy">{</span><span class="crayon-cn">1</span><span class="crayon-sy">,</span><span class="crayon-cn">4</span><span class="crayon-sy">}</span><span class="crayon-sy">)</span><span class="crayon-sy">{</span><span class="crayon-cn">1</span><span class="crayon-sy">,</span><span class="crayon-cn">2</span><span class="crayon-sy">}</span><span class="crayon-sy">)</span><span class="crayon-sy">|</span><span class="crayon-sy">:</span><span class="crayon-sy">(</span><span class="crayon-sy">(</span><span class="crayon-cn">25</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">5</span><span class="crayon-sy">]</span><span class="crayon-sy">|</span><span class="crayon-cn">2</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">4</span><span class="crayon-sy">]</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">|</span><span class="crayon-cn">1</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">|</span><span class="crayon-sy">[</span><span class="crayon-cn">1</span>-<span class="crayon-cn">9</span><span class="crayon-sy">]</span><span class="crayon-sy">?</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">)</span><span class="crayon-sy">(</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-sy">.</span><span class="crayon-sy">(</span><span class="crayon-cn">25</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">5</span><span class="crayon-sy">]</span><span class="crayon-sy">|</span><span class="crayon-cn">2</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">4</span><span class="crayon-sy">]</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">|</span><span class="crayon-cn">1</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">|</span><span class="crayon-sy">[</span><span class="crayon-cn">1</span>-<span class="crayon-cn">9</span><span class="crayon-sy">]</span><span class="crayon-sy">?</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">{</span><span class="crayon-cn">3</span><span class="crayon-sy">}</span><span class="crayon-sy">)</span><span class="crayon-sy">|</span><span class="crayon-sy">:</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">|</span><span class="crayon-sy">(</span><span class="crayon-sy">(</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9A</span>-<span class="crayon-e">Fa</span>-<span class="crayon-e">f</span><span class="crayon-sy">]</span><span class="crayon-sy">{</span><span class="crayon-cn">1</span><span class="crayon-sy">,</span><span class="crayon-cn">4</span><span class="crayon-sy">}</span><span class="crayon-sy">:</span><span class="crayon-sy">)</span><span class="crayon-sy">{</span><span class="crayon-cn">4</span><span class="crayon-sy">}</span><span class="crayon-sy">(</span><span class="crayon-sy">(</span><span class="crayon-sy">(</span><span class="crayon-sy">:</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9A</span>-<span class="crayon-e">Fa</span>-<span class="crayon-e">f</span><span class="crayon-sy">]</span><span class="crayon-sy">{</span><span class="crayon-cn">1</span><span class="crayon-sy">,</span><span class="crayon-cn">4</span><span class="crayon-sy">}</span><span class="crayon-sy">)</span><span class="crayon-sy">{</span><span class="crayon-cn">1</span><span class="crayon-sy">,</span><span class="crayon-cn">3</span><span class="crayon-sy">}</span><span class="crayon-sy">)</span><span class="crayon-sy">|</span><span class="crayon-sy">(</span><span class="crayon-sy">(</span><span class="crayon-sy">:</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9A</span>-<span class="crayon-e">Fa</span>-<span class="crayon-e">f</span><span class="crayon-sy">]</span><span class="crayon-sy">{</span><span class="crayon-cn">1</span><span class="crayon-sy">,</span><span class="crayon-cn">4</span><span class="crayon-sy">}</span><span class="crayon-sy">)</span><span class="crayon-sy">?</span><span class="crayon-sy">:</span><span class="crayon-sy">(</span><span class="crayon-sy">(</span><span class="crayon-cn">25</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">5</span><span class="crayon-sy">]</span><span class="crayon-sy">|</span><span class="crayon-cn">2</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">4</span><span class="crayon-sy">]</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">|</span><span class="crayon-cn">1</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">|</span><span class="crayon-sy">[</span><span class="crayon-cn">1</span>-<span class="crayon-cn">9</span><span class="crayon-sy">]</span><span class="crayon-sy">?</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">)</span><span class="crayon-sy">(</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-sy">.</span><span class="crayon-sy">(</span><span class="crayon-cn">25</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">5</span><span class="crayon-sy">]</span><span class="crayon-sy">|</span><span class="crayon-cn">2</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">4</span><span class="crayon-sy">]</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">|</span><span class="crayon-cn">1</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">|</span><span class="crayon-sy">[</span><span class="crayon-cn">1</span>-<span class="crayon-cn">9</span><span class="crayon-sy">]</span><span class="crayon-sy">?</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">{</span><span class="crayon-cn">3</span><span class="crayon-sy">}</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">|</span><span class="crayon-sy">:</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">|</span><span class="crayon-sy">(</span><span class="crayon-sy">(</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9A</span>-<span class="crayon-e">Fa</span>-<span class="crayon-e">f</span><span class="crayon-sy">]</span><span class="crayon-sy">{</span><span class="crayon-cn">1</span><span class="crayon-sy">,</span><span class="crayon-cn">4</span><span class="crayon-sy">}</span><span class="crayon-sy">:</span><span class="crayon-sy">)</span><span class="crayon-sy">{</span><span class="crayon-cn">3</span><span class="crayon-sy">}</span><span class="crayon-sy">(</span><span class="crayon-sy">(</span><span class="crayon-sy">(</span><span class="crayon-sy">:</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9A</span>-<span class="crayon-e">Fa</span>-<span class="crayon-e">f</span><span class="crayon-sy">]</span><span class="crayon-sy">{</span><span class="crayon-cn">1</span><span class="crayon-sy">,</span><span class="crayon-cn">4</span><span class="crayon-sy">}</span><span class="crayon-sy">)</span><span class="crayon-sy">{</span><span class="crayon-cn">1</span><span class="crayon-sy">,</span><span class="crayon-cn">4</span><span class="crayon-sy">}</span><span class="crayon-sy">)</span><span class="crayon-sy">|</span><span class="crayon-sy">(</span><span class="crayon-sy">(</span><span class="crayon-sy">:</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9A</span>-<span class="crayon-e">Fa</span>-<span class="crayon-e">f</span><span class="crayon-sy">]</span><span class="crayon-sy">{</span><span class="crayon-cn">1</span><span class="crayon-sy">,</span><span class="crayon-cn">4</span><span class="crayon-sy">}</span><span class="crayon-sy">)</span><span class="crayon-sy">{</span><span class="crayon-cn">0</span><span class="crayon-sy">,</span><span class="crayon-cn">2</span><span class="crayon-sy">}</span><span class="crayon-sy">:</span><span class="crayon-sy">(</span><span class="crayon-sy">(</span><span class="crayon-cn">25</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">5</span><span class="crayon-sy">]</span><span class="crayon-sy">|</span><span class="crayon-cn">2</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">4</span><span class="crayon-sy">]</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">|</span><span class="crayon-cn">1</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">|</span><span class="crayon-sy">[</span><span class="crayon-cn">1</span>-<span class="crayon-cn">9</span><span class="crayon-sy">]</span><span class="crayon-sy">?</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">)</span><span class="crayon-sy">(</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-sy">.</span><span class="crayon-sy">(</span><span class="crayon-cn">25</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">5</span><span class="crayon-sy">]</span><span class="crayon-sy">|</span><span class="crayon-cn">2</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">4</span><span class="crayon-sy">]</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">|</span><span class="crayon-cn">1</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">|</span><span class="crayon-sy">[</span><span class="crayon-cn">1</span>-<span class="crayon-cn">9</span><span class="crayon-sy">]</span><span class="crayon-sy">?</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">{</span><span class="crayon-cn">3</span><span class="crayon-sy">}</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">|</span><span class="crayon-sy">:</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">|</span><span class="crayon-sy">(</span><span class="crayon-sy">(</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9A</span>-<span class="crayon-e">Fa</span>-<span class="crayon-e">f</span><span class="crayon-sy">]</span><span class="crayon-sy">{</span><span class="crayon-cn">1</span><span class="crayon-sy">,</span><span class="crayon-cn">4</span><span class="crayon-sy">}</span><span class="crayon-sy">:</span><span class="crayon-sy">)</span><span class="crayon-sy">{</span><span class="crayon-cn">2</span><span class="crayon-sy">}</span><span class="crayon-sy">(</span><span class="crayon-sy">(</span><span class="crayon-sy">(</span><span class="crayon-sy">:</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9A</span>-<span class="crayon-e">Fa</span>-<span class="crayon-e">f</span><span class="crayon-sy">]</span><span class="crayon-sy">{</span><span class="crayon-cn">1</span><span class="crayon-sy">,</span><span class="crayon-cn">4</span><span class="crayon-sy">}</span><span class="crayon-sy">)</span><span class="crayon-sy">{</span><span class="crayon-cn">1</span><span class="crayon-sy">,</span><span class="crayon-cn">5</span><span class="crayon-sy">}</span><span class="crayon-sy">)</span><span class="crayon-sy">|</span><span class="crayon-sy">(</span><span class="crayon-sy">(</span><span class="crayon-sy">:</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9A</span>-<span class="crayon-e">Fa</span>-<span class="crayon-e">f</span><span class="crayon-sy">]</span><span class="crayon-sy">{</span><span class="crayon-cn">1</span><span class="crayon-sy">,</span><span class="crayon-cn">4</span><span class="crayon-sy">}</span><span class="crayon-sy">)</span><span class="crayon-sy">{</span><span class="crayon-cn">0</span><span class="crayon-sy">,</span><span class="crayon-cn">3</span><span class="crayon-sy">}</span><span class="crayon-sy">:</span><span class="crayon-sy">(</span><span class="crayon-sy">(</span><span class="crayon-cn">25</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">5</span><span class="crayon-sy">]</span><span class="crayon-sy">|</span><span class="crayon-cn">2</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">4</span><span class="crayon-sy">]</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">|</span><span class="crayon-cn">1</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">|</span><span class="crayon-sy">[</span><span class="crayon-cn">1</span>-<span class="crayon-cn">9</span><span class="crayon-sy">]</span><span class="crayon-sy">?</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">)</span><span class="crayon-sy">(</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-sy">.</span><span class="crayon-sy">(</span><span class="crayon-cn">25</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">5</span><span class="crayon-sy">]</span><span class="crayon-sy">|</span><span class="crayon-cn">2</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">4</span><span class="crayon-sy">]</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">|</span><span class="crayon-cn">1</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">|</span><span class="crayon-sy">[</span><span class="crayon-cn">1</span>-<span class="crayon-cn">9</span><span class="crayon-sy">]</span><span class="crayon-sy">?</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">{</span><span class="crayon-cn">3</span><span class="crayon-sy">}</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">|</span><span class="crayon-sy">:</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">|</span><span class="crayon-sy">(</span><span class="crayon-sy">(</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9A</span>-<span class="crayon-e">Fa</span>-<span class="crayon-e">f</span><span class="crayon-sy">]</span><span class="crayon-sy">{</span><span class="crayon-cn">1</span><span class="crayon-sy">,</span><span class="crayon-cn">4</span><span class="crayon-sy">}</span><span class="crayon-sy">:</span><span class="crayon-sy">)</span><span class="crayon-sy">{</span><span class="crayon-cn">1</span><span class="crayon-sy">}</span><span class="crayon-sy">(</span><span class="crayon-sy">(</span><span class="crayon-sy">(</span><span class="crayon-sy">:</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9A</span>-<span class="crayon-e">Fa</span>-<span class="crayon-e">f</span><span class="crayon-sy">]</span><span class="crayon-sy">{</span><span class="crayon-cn">1</span><span class="crayon-sy">,</span><span class="crayon-cn">4</span><span class="crayon-sy">}</span><span class="crayon-sy">)</span><span class="crayon-sy">{</span><span class="crayon-cn">1</span><span class="crayon-sy">,</span><span class="crayon-cn">6</span><span class="crayon-sy">}</span><span class="crayon-sy">)</span><span class="crayon-sy">|</span><span class="crayon-sy">(</span><span class="crayon-sy">(</span><span class="crayon-sy">:</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9A</span>-<span class="crayon-e">Fa</span>-<span class="crayon-e">f</span><span class="crayon-sy">]</span><span class="crayon-sy">{</span><span class="crayon-cn">1</span><span class="crayon-sy">,</span><span class="crayon-cn">4</span><span class="crayon-sy">}</span><span class="crayon-sy">)</span><span class="crayon-sy">{</span><span class="crayon-cn">0</span><span class="crayon-sy">,</span><span class="crayon-cn">4</span><span class="crayon-sy">}</span><span class="crayon-sy">:</span><span class="crayon-sy">(</span><span class="crayon-sy">(</span><span class="crayon-cn">25</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">5</span><span class="crayon-sy">]</span><span class="crayon-sy">|</span><span class="crayon-cn">2</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">4</span><span class="crayon-sy">]</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">|</span><span class="crayon-cn">1</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">|</span><span class="crayon-sy">[</span><span class="crayon-cn">1</span>-<span class="crayon-cn">9</span><span class="crayon-sy">]</span><span class="crayon-sy">?</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">)</span><span class="crayon-sy">(</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-sy">.</span><span class="crayon-sy">(</span><span class="crayon-cn">25</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">5</span><span class="crayon-sy">]</span><span class="crayon-sy">|</span><span class="crayon-cn">2</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">4</span><span class="crayon-sy">]</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">|</span><span class="crayon-cn">1</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">|</span><span class="crayon-sy">[</span><span class="crayon-cn">1</span>-<span class="crayon-cn">9</span><span class="crayon-sy">]</span><span class="crayon-sy">?</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">{</span><span class="crayon-cn">3</span><span class="crayon-sy">}</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">|</span><span class="crayon-sy">:</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">|</span><span class="crayon-sy">(</span><span class="crayon-sy">:</span><span class="crayon-sy">(</span><span class="crayon-sy">(</span><span class="crayon-sy">(</span><span class="crayon-sy">:</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9A</span>-<span class="crayon-e">Fa</span>-<span class="crayon-e">f</span><span class="crayon-sy">]</span><span class="crayon-sy">{</span><span class="crayon-cn">1</span><span class="crayon-sy">,</span><span class="crayon-cn">4</span><span class="crayon-sy">}</span><span class="crayon-sy">)</span><span class="crayon-sy">{</span><span class="crayon-cn">1</span><span class="crayon-sy">,</span><span class="crayon-cn">7</span><span class="crayon-sy">}</span><span class="crayon-sy">)</span><span class="crayon-sy">|</span><span class="crayon-sy">(</span><span class="crayon-sy">(</span><span class="crayon-sy">:</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9A</span>-<span class="crayon-e">Fa</span>-<span class="crayon-e">f</span><span class="crayon-sy">]</span><span class="crayon-sy">{</span><span class="crayon-cn">1</span><span class="crayon-sy">,</span><span class="crayon-cn">4</span><span class="crayon-sy">}</span><span class="crayon-sy">)</span><span class="crayon-sy">{</span><span class="crayon-cn">0</span><span class="crayon-sy">,</span><span class="crayon-cn">5</span><span class="crayon-sy">}</span><span class="crayon-sy">:</span><span class="crayon-sy">(</span><span class="crayon-sy">(</span><span class="crayon-cn">25</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">5</span><span class="crayon-sy">]</span><span class="crayon-sy">|</span><span class="crayon-cn">2</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">4</span><span class="crayon-sy">]</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">|</span><span class="crayon-cn">1</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">|</span><span class="crayon-sy">[</span><span class="crayon-cn">1</span>-<span class="crayon-cn">9</span><span class="crayon-sy">]</span><span class="crayon-sy">?</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">)</span><span class="crayon-sy">(</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-sy">.</span><span class="crayon-sy">(</span><span class="crayon-cn">25</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">5</span><span class="crayon-sy">]</span><span class="crayon-sy">|</span><span class="crayon-cn">2</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">4</span><span class="crayon-sy">]</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">|</span><span class="crayon-cn">1</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">|</span><span class="crayon-sy">[</span><span class="crayon-cn">1</span>-<span class="crayon-cn">9</span><span class="crayon-sy">]</span><span class="crayon-sy">?</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-i">d</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">{</span><span class="crayon-cn">3</span><span class="crayon-sy">}</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">|</span><span class="crayon-sy">:</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">(</span><span class="crayon-sy">%</span><span class="crayon-sy">.</span>+<span class="crayon-sy">)</span><span class="crayon-sy">?</span><span class="crayon-sy">\</span><span class="crayon-sy">\</span><span class="crayon-e ">s*</span><span class="crayon-sy">$</span></div></div></td>
</tr>
</table>
</div>
</div>

<p>
この正規表現のパターンは次のリンク先を参考にしたものをJava向けに書き換えて使用しています。<br />
リンク先：<a href="https://community.helpsystems.com/forums/intermapper/miscellaneous-topics/5acc4fcf-fa83-e511-80cf-0050568460e4#aac8bffd-fc83-e511-80d0-005056842064">helpsystems.com</a></p>
<p>IPv6のIPアドレスは省略して書けるためパターンが複雑になります。</p>
<p>次のプログラムで確認してみましょう。</p>

### IPv6のアドレスのチェックをするサンプルコード

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bf3958824701" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">

```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;
 
public class Main {
    public static void main(String[] args) {
 
        // 検索する文字列を用意
        String str1 = "1000:2000:3000:4000:5000:6000:7000:abcd";
        String str2 = "1:2:3:4:5:6:7:a";
        String str3 = "1:2:3::6:7:a";
        String str4 = "1:2::a";
 
        // 正規表現のパターンを作成
        Pattern p = Pattern.compile("^\\s*((([0-9A-Fa-f]{1,4}:){7}([0-9A-Fa-f]{1,4}|:))|"
                + "(([0-9A-Fa-f]{1,4}:){6}(:[0-9A-Fa-f]{1,4}|((25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)(\\.(25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)){3})|:))|"
                + "(([0-9A-Fa-f]{1,4}:){5}(((:[0-9A-Fa-f]{1,4}){1,2})|:((25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)(\\.(25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)){3})|:))|"
                + "(([0-9A-Fa-f]{1,4}:){4}(((:[0-9A-Fa-f]{1,4}){1,3})|((:[0-9A-Fa-f]{1,4})?:((25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)(\\.(25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)){3}))|:))|"
                + "(([0-9A-Fa-f]{1,4}:){3}(((:[0-9A-Fa-f]{1,4}){1,4})|((:[0-9A-Fa-f]{1,4}){0,2}:((25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)(\\.(25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)){3}))|:))|"
                + "(([0-9A-Fa-f]{1,4}:){2}(((:[0-9A-Fa-f]{1,4}){1,5})|((:[0-9A-Fa-f]{1,4}){0,3}:((25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)(\\.(25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)){3}))|:))|"
                + "(([0-9A-Fa-f]{1,4}:){1}(((:[0-9A-Fa-f]{1,4}){1,6})|((:[0-9A-Fa-f]{1,4}){0,4}:((25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)(\\.(25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)){3}))|:))|"
                + "(:(((:[0-9A-Fa-f]{1,4}){1,7})|((:[0-9A-Fa-f]{1,4}){0,5}:((25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)(\\.(25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)){3}))|:)))(%.+)?\\s*$");
 
        Matcher m = p.matcher(str1);
        System.out.println(m.find());
 
        m = p.matcher(str2);
        System.out.println(m.find());
        
        m = p.matcher(str3);
        System.out.println(m.find());
        
        m = p.matcher(str4);
        System.out.println(m.find());
    }
}
```

### 実行結果

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bf6862146072" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">

<div class="crayon-main" style="">
<table class="crayon-table">
<tr class="crayon-row">
<td class="crayon-nums " data-settings="hide">
<div class="crayon-nums-content" style="font-size: 15px !important; line-height: 22px !important;"><div class="crayon-num" data-line="crayon-5d5de958a1bf6862146072-1">1</div><div class="crayon-num" data-line="crayon-5d5de958a1bf6862146072-2">2</div><div class="crayon-num" data-line="crayon-5d5de958a1bf6862146072-3">3</div><div class="crayon-num" data-line="crayon-5d5de958a1bf6862146072-4">4</div></div>
</td>
<td class="crayon-code"><div class="crayon-pre" style="font-size: 15px !important; line-height: 22px !important; -moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4;"><div class="crayon-line" id="crayon-5d5de958a1bf6862146072-1"><span class="crayon-t">true</span></div><div class="crayon-line" id="crayon-5d5de958a1bf6862146072-2"><span class="crayon-t">true</span></div><div class="crayon-line" id="crayon-5d5de958a1bf6862146072-3"><span class="crayon-t">true</span></div><div class="crayon-line" id="crayon-5d5de958a1bf6862146072-4"><span class="crayon-t">true</span></div></div></td>
</tr>
</table>
</div>
</div>

<h3><span id="i-10">メールアドレスのチェック</span></h3>
<p>メールアドレスのチェックをする正規表現のパターンは次のとおりです。</p>
<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bf7932610548" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">

<div class="crayon-main" style="">
<table class="crayon-table">
<tr class="crayon-row">
<td class="crayon-nums " data-settings="hide">
<div class="crayon-nums-content" style="font-size: 15px !important; line-height: 22px !important;"><div class="crayon-num" data-line="crayon-5d5de958a1bf7932610548-1">1</div></div>
</td>
<td class="crayon-code"><div class="crayon-pre" style="font-size: 15px !important; line-height: 22px !important; -moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4;"><div class="crayon-line" id="crayon-5d5de958a1bf7932610548-1">^<span class="crayon-sy">(</span><span class="crayon-sy">(</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span>-<span class="crayon-cn">9a</span>-<span class="crayon-i">zA</span>-<span class="crayon-i">Z</span><span class="crayon-sy">!</span><span class="crayon-p">#\\$%&amp;'\\*\\+\\-/=\\?\\^_`\\{\\}\\|~]+(\\.[0-9a-zA-Z!#\\$%&amp;'\\*\\+\\-/=\\?\\^_`\\{\\}\\|~]+)*)|(\"[^\"]*\"))@[0-9a-zA-Z!#\\$%&amp;'\\*\\+\\-/=\\?\\^_`\\{\\}\\|~]+(\\.[0-9a-zA-Z!#\\$%&amp;'\\*\\+\\-/=\\?\\^_`\\{\\}\\|~]+)*$</span></div></div></td>
</tr>
</table>
</div>
</div>

<p>
メールアドレスのルールは複雑なため、正規表現のパターンも分かりにくいものになりが、順に解説していきます。</p>
<p>まず、メールアドレスには英数字の他にも次の記号「! # $ % &amp; &#8216; * + &#8211; / = ? ^ _ ` { } | ~ .」が使えます。</p>
<p>そのため、英数字の他にも記号を含むパターンを作成しています。エスケープ処理が必要な記号もあるので間違えないように気をつけましょう！</p>
<p>次に、.(ドット)が2つ以上連続してはいけないのでそれもチェックしています。ただし、”(ダブルクォーテーション)で囲んだ場合は.(ドット)が2つ以上連続してもOKなので、それも判定しています。</p>
<p>次のプログラムで確認してみましょう。</p>

### メールアドレスのチェックをするサンプルコード

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bf9973872259" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">
    
```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;
 
public class Main {
    public static void main(String[] args) {
 
        // 検索する文字列を用意
        String str1 = "Samurai+Engineer.123@gmail.com";
        String str2 = "Samurai..Engineer@gmail.com";
        String str3 = "\"Samurai..Engineer\"@gmail.com";
 
        // 正規表現のパターンを作成
        Pattern p = Pattern.compile(
                "^(([0-9a-zA-Z!#\\$%&amp;'\\*\\+\\-/=\\?\\^_`\\{\\}\\|~]+(\\.[0-9a-zA-Z!#\\$%&amp;'\\*\\+\\-/=\\?\\^_`\\{\\}\\|~]+)*)|(\"[^\"]*\"))"
                        + "@[0-9a-zA-Z!#\\$%&amp;'\\*\\+\\-/=\\?\\^_`\\{\\}\\|~]+"
                        + "(\\.[0-9a-zA-Z!#\\$%&amp;'\\*\\+\\-/=\\?\\^_`\\{\\}\\|~]+)*$");
 
        Matcher m = p.matcher(str1);
        System.out.println(str1 + " = " + m.find());

        m = p.matcher(str2);
        System.out.println(str2 + " = " + m.find());

        m = p.matcher(str3);
        System.out.println(str3 + " = " + m.find());
    }
}
```

### 実行結果

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bfb107983660" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">

<div class="crayon-main" style="">
<table class="crayon-table">
<tr class="crayon-row">
<td class="crayon-nums " data-settings="hide">
<div class="crayon-nums-content" style="font-size: 15px !important; line-height: 22px !important;"><div class="crayon-num" data-line="crayon-5d5de958a1bfb107983660-1">1</div><div class="crayon-num" data-line="crayon-5d5de958a1bfb107983660-2">2</div><div class="crayon-num" data-line="crayon-5d5de958a1bfb107983660-3">3</div></div>
</td>
<td class="crayon-code"><div class="crayon-pre" style="font-size: 15px !important; line-height: 22px !important; -moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4;"><div class="crayon-line" id="crayon-5d5de958a1bfb107983660-1"><span class="crayon-i">Samurai</span>+<span class="crayon-i">Engineer</span><span class="crayon-sy">.</span><span class="crayon-cn">123</span><span class="crayon-sy">@</span><span class="crayon-i">gmail</span><span class="crayon-sy">.</span><span class="crayon-i">com</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-t">true</span></div><div class="crayon-line" id="crayon-5d5de958a1bfb107983660-2"><span class="crayon-i">Samurai</span><span class="crayon-sy">.</span><span class="crayon-sy">.</span><span class="crayon-i">Engineer</span><span class="crayon-sy">@</span><span class="crayon-i">gmail</span><span class="crayon-sy">.</span><span class="crayon-i">com</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-t">false</span></div><div class="crayon-line" id="crayon-5d5de958a1bfb107983660-3"><span class="crayon-s">"Samurai..Engineer"</span><span class="crayon-sy">@</span><span class="crayon-i">gmail</span><span class="crayon-sy">.</span><span class="crayon-i">com</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-t">true</span></div></div></td>
</tr>
</table>
</div>
</div>

<h3><span id="URL">URLのチェック</span></h3>
<p>URLのチェックをする正規表現のパターンは次のとおりです。</p>
<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bfc646726037" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">

<div class="crayon-main" style="">
<table class="crayon-table">
<tr class="crayon-row">
<td class="crayon-nums " data-settings="hide">
<div class="crayon-nums-content" style="font-size: 15px !important; line-height: 22px !important;"><div class="crayon-num" data-line="crayon-5d5de958a1bfc646726037-1">1</div></div>
</td>
<td class="crayon-code"><div class="crayon-pre" style="font-size: 15px !important; line-height: 22px !important; -moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4;"><div class="crayon-line" id="crayon-5d5de958a1bfc646726037-1">^<span class="crayon-i">https</span><span class="crayon-sy">?</span><span class="crayon-sy">:</span><span class="crayon-c">//[a-z\\.:/\\+\\-\\#\\?\\=\\&amp;\\;\\%\\~]+$</span></div></div></td>
</tr>
</table>
</div>
</div>

<p>
次のプログラムで確認してみましょう。</p>

### URLのチェックをするサンプルコード

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bfd002406925" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">

```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;
 
public class Main {
    public static void main(String[] args) {
 
        // 検索する文字列を用意
        String str = "http://www.sejuku.net/blog";
 
        // 正規表現のパターンを作成
        Pattern p = Pattern.compile("^https?://[a-z\\.:/\\+\\-\\#\\?\\=\\&amp;\\;\\%\\~]+$");
        Matcher m = p.matcher(str);
 
        System.out.println(m.find());
    }
}
```

### 実行結果

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bff394109116" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">

<div class="crayon-main" style="">
<table class="crayon-table">
<tr class="crayon-row">
<td class="crayon-nums " data-settings="hide">
<div class="crayon-nums-content" style="font-size: 15px !important; line-height: 22px !important;"><div class="crayon-num" data-line="crayon-5d5de958a1bff394109116-1">1</div></div>
</td>
<td class="crayon-code"><div class="crayon-pre" style="font-size: 15px !important; line-height: 22px !important; -moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4;"><div class="crayon-line" id="crayon-5d5de958a1bff394109116-1"><span class="crayon-t">true</span></div></div></td>
</tr>
</table>
</div>
</div>

<h2><span id="i-11">まとめ</span></h2>
<p>いかがでしたか？</p>
<p>今回は<span class="red_strong">正規表現の使い方</span>について解説しました。正規表現のパターンを使いこなせば、任意の文字列の検索ができるのようになるのでぜひ覚えてくださいね。</p>
<p>もし、正規表現を使う方法を忘れてしまったらこの記事を確認してください！</p>
