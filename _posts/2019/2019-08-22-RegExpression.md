---
---

> link : https://www.sejuku.net/blog/13215

<div class="entry-content">
<p>こんにちは！エンジニアの中沢です。</p>
<p><a href="https://www.sejuku.net/blog/3686" target="_blank" rel="noopener">Java</a>には文字列から特定のパターンを検索して、一致する文字列があるかをチェックするための<span class="green_strong">正規表現</span>があります。</p>
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
<p><div class="box01"><b>イチオシの記事</b></p>
<ul>
<li><b>&gt;&gt;<a href="https://www.sejuku.net/blog/59584/?cid=tecv_13215">フリーランスエンジニアを目指すために知るべき4つの真実</a></b></li>
<li><b>&gt;&gt;<a href="https://www.sejuku.net/blog/60101/?cid=tecv_13215">あなたは大丈夫？プログラミングが上達しない人の共通点</a></b></li>
</ul>
</div>
<div id="toc_container" class="no_bullets"><p class="toc_title">この記事の目次</p><ul class="toc_list"><li><a href="#Java"><span class="toc_number toc_depth_1">1</span> Javaの正規表現とは</a></li><li><a href="#i"><span class="toc_number toc_depth_1">2</span> 正規表現の使い方</a><ul><li><a href="#matches"><span class="toc_number toc_depth_2">2.1</span> matchesメソッドでチェックする方法</a></li><li><a href="#Pattern"><span class="toc_number toc_depth_2">2.2</span> パターンの作り方(Patternクラス)</a></li><li><a href="#Matcher"><span class="toc_number toc_depth_2">2.3</span> 一致するかチェックする方法(Matcherクラス)</a></li><li><a href="#i-2"><span class="toc_number toc_depth_2">2.4</span> 一致する複数の文字をすべて抽出する方法</a></li></ul></li><li><a href="#i-3"><span class="toc_number toc_depth_1">3</span> 特殊文字のエスケープ処理について</a></li><li><a href="#i-4"><span class="toc_number toc_depth_1">4</span> 正規表現の書き方サンプル一覧</a><ul><li><a href="#i-5"><span class="toc_number toc_depth_2">4.1</span> 数字のチェック</a></li><li><a href="#i-6"><span class="toc_number toc_depth_2">4.2</span> 英数字のチェック</a></li><li><a href="#i-7"><span class="toc_number toc_depth_2">4.3</span> 日付のチェック</a></li><li><a href="#i-8"><span class="toc_number toc_depth_2">4.4</span> 電話番号のチェック</a></li><li><a href="#i-9"><span class="toc_number toc_depth_2">4.5</span> 郵便番号のチェック</a></li><li><a href="#IPIPv4_IPv6"><span class="toc_number toc_depth_2">4.6</span> IPアドレス(IPv4, IPv6)のチェック</a></li><li><a href="#i-10"><span class="toc_number toc_depth_2">4.7</span> メールアドレスのチェック</a></li><li><a href="#URL"><span class="toc_number toc_depth_2">4.8</span> URLのチェック</a></li></ul></li><li><a href="#i-11"><span class="toc_number toc_depth_1">5</span> まとめ</a></li></ul></div>
<h2><span id="Java">Javaの正規表現とは</span></h2>
<p><span class="green_strong">正規表現</span>とは<b>文字列のパターンを一つの形式でまとめて表現するために使うもののこと</b>です。</p>
<p>例えば、文字列の中から&#8221;123-4567”のような郵便番号を検索したい場合には次のような正規表現の記述を使います。</p>
<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bc3768885463" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">
<div class="crayon-plain-wrap"><textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 15px !important; line-height: 22px !important;">
[0-9]{3}-[0-9]{4}</textarea></div>
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
[matcheメソッドのサンプルコード]

<div id="crayon-5d5de958a1bc9755813901" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">
<div class="crayon-plain-wrap"><textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 15px !important; line-height: 22px !important;">
public class Main {
    public static void main(String[] args) {

        // 検索する文字列を用意
        String str = "東京都千代田区 123-4567";

        System.out.println(str.matches(".*[0-9]{3}-[0-9]{4}.*"));
        System.out.println(str.matches("[0-9]{3}-[0-9]{4}"));

        System.out.println(str.matches(".*123-4567.*"));
        System.out.println(str.matches("123-4567"));
    }
}</textarea></div>
<div class="crayon-main" style="">
<table class="crayon-table">
<tr class="crayon-row">
<td class="crayon-nums " data-settings="show">
<div class="crayon-nums-content" style="font-size: 15px !important; line-height: 22px !important;"><div class="crayon-num" data-line="crayon-5d5de958a1bc9755813901-1">1</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bc9755813901-2">2</div><div class="crayon-num" data-line="crayon-5d5de958a1bc9755813901-3">3</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bc9755813901-4">4</div><div class="crayon-num" data-line="crayon-5d5de958a1bc9755813901-5">5</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bc9755813901-6">6</div><div class="crayon-num" data-line="crayon-5d5de958a1bc9755813901-7">7</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bc9755813901-8">8</div><div class="crayon-num" data-line="crayon-5d5de958a1bc9755813901-9">9</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bc9755813901-10">10</div><div class="crayon-num" data-line="crayon-5d5de958a1bc9755813901-11">11</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bc9755813901-12">12</div><div class="crayon-num" data-line="crayon-5d5de958a1bc9755813901-13">13</div></div>
</td>
<td class="crayon-code"><div class="crayon-pre" style="font-size: 15px !important; line-height: 22px !important; -moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4;"><div class="crayon-line" id="crayon-5d5de958a1bc9755813901-1"><span class="crayon-m">public</span><span class="crayon-h"> </span><span class="crayon-t">class</span><span class="crayon-h"> </span><span class="crayon-e">Main</span><span class="crayon-h"> </span><span class="crayon-sy">{</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bc9755813901-2"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-m">public</span><span class="crayon-h"> </span><span class="crayon-m">static</span><span class="crayon-h"> </span><span class="crayon-t">void</span><span class="crayon-h"> </span><span class="crayon-e">main</span><span class="crayon-sy">(</span><span class="crayon-t">String</span><span class="crayon-sy">[</span><span class="crayon-sy">]</span><span class="crayon-h"> </span><span class="crayon-i">args</span><span class="crayon-sy">)</span><span class="crayon-h"> </span><span class="crayon-sy">{</span></div><div class="crayon-line" id="crayon-5d5de958a1bc9755813901-3">&nbsp;</div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bc9755813901-4"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-c">// 検索する文字列を用意</span></div><div class="crayon-line" id="crayon-5d5de958a1bc9755813901-5"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-t">String</span><span class="crayon-h"> </span><span class="crayon-i">str</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-s">"東京都千代田区 123-4567"</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bc9755813901-6">&nbsp;</div><div class="crayon-line" id="crayon-5d5de958a1bc9755813901-7"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">System</span><span class="crayon-sy">.</span><span class="crayon-i">out</span><span class="crayon-sy">.</span><span class="crayon-e">println</span><span class="crayon-sy">(</span><span class="crayon-i">str</span><span class="crayon-sy">.</span><span class="crayon-e">matches</span><span class="crayon-sy">(</span><span class="crayon-s">".*[0-9]{3}-[0-9]{4}.*"</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bc9755813901-8"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">System</span><span class="crayon-sy">.</span><span class="crayon-i">out</span><span class="crayon-sy">.</span><span class="crayon-e">println</span><span class="crayon-sy">(</span><span class="crayon-i">str</span><span class="crayon-sy">.</span><span class="crayon-e">matches</span><span class="crayon-sy">(</span><span class="crayon-s">"[0-9]{3}-[0-9]{4}"</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bc9755813901-9">&nbsp;</div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bc9755813901-10"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">System</span><span class="crayon-sy">.</span><span class="crayon-i">out</span><span class="crayon-sy">.</span><span class="crayon-e">println</span><span class="crayon-sy">(</span><span class="crayon-i">str</span><span class="crayon-sy">.</span><span class="crayon-e">matches</span><span class="crayon-sy">(</span><span class="crayon-s">".*123-4567.*"</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bc9755813901-11"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">System</span><span class="crayon-sy">.</span><span class="crayon-i">out</span><span class="crayon-sy">.</span><span class="crayon-e">println</span><span class="crayon-sy">(</span><span class="crayon-i">str</span><span class="crayon-sy">.</span><span class="crayon-e">matches</span><span class="crayon-sy">(</span><span class="crayon-s">"123-4567"</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bc9755813901-12"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-sy">}</span></div><div class="crayon-line" id="crayon-5d5de958a1bc9755813901-13"><span class="crayon-sy">}</span></div></div></td>
</tr>
</table>
</div>
</div>

[実行結果]

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bcb432730109" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">
<div class="crayon-plain-wrap"><textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 15px !important; line-height: 22px !important;">
true
false
true
false</textarea></div>
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

このプログラムではmatchesメソッドを使って、正規表現で表現した文字列が含まれているかを判定しています。matchesメソッドは完全一致のときに”true”を返すので、検索したい文字列の前後に他の文字があると”false”を返します。</p>
<p>そのため、正規表現のパターンの前後に「任意の文字がいくつかある」という意味の「.*」を追加して完全一致させることで”true”を返しています。</p>
</div>
</div>
</a></p>
<h3><span id="Pattern">パターンの作り方(Patternクラス)</span></h3>
<p>ここではPatternクラスを使って正規表現のパターンを作る方法を解説します。</p>
<p>正規表現のパターンオブジェクトを作るには、Patternクラスのcompileメソッドの引数に正規表現のパターンを指定します。</p>
<p>次に、Patternクラスのmatcherメソッドの引数にパターンとマッチさせる文字列を指定してMatcherオブジェクトを作成します。</p>
<p>正規表現のパターンを作る方法を次のプログラムで確認してみましょう。</p>
[正規表現のパターンの作り方のサンプルコード]

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bcd173085216" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">
<div class="crayon-plain-wrap"><textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 15px !important; line-height: 22px !important;">
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
}</textarea></div>
<div class="crayon-main" style="">
<table class="crayon-table">
<tr class="crayon-row">
<td class="crayon-nums " data-settings="show">
<div class="crayon-nums-content" style="font-size: 15px !important; line-height: 22px !important;"><div class="crayon-num" data-line="crayon-5d5de958a1bcd173085216-1">1</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bcd173085216-2">2</div><div class="crayon-num" data-line="crayon-5d5de958a1bcd173085216-3">3</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bcd173085216-4">4</div><div class="crayon-num" data-line="crayon-5d5de958a1bcd173085216-5">5</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bcd173085216-6">6</div><div class="crayon-num" data-line="crayon-5d5de958a1bcd173085216-7">7</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bcd173085216-8">8</div><div class="crayon-num" data-line="crayon-5d5de958a1bcd173085216-9">9</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bcd173085216-10">10</div><div class="crayon-num" data-line="crayon-5d5de958a1bcd173085216-11">11</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bcd173085216-12">12</div><div class="crayon-num" data-line="crayon-5d5de958a1bcd173085216-13">13</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bcd173085216-14">14</div></div>
</td>
<td class="crayon-code"><div class="crayon-pre" style="font-size: 15px !important; line-height: 22px !important; -moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4;"><div class="crayon-line" id="crayon-5d5de958a1bcd173085216-1"><span class="crayon-r">import</span><span class="crayon-h"> </span><span class="crayon-i">java</span><span class="crayon-sy">.</span><span class="crayon-i">util</span><span class="crayon-sy">.</span><span class="crayon-i">regex</span><span class="crayon-sy">.</span><span class="crayon-i">Matcher</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bcd173085216-2"><span class="crayon-r">import</span><span class="crayon-h"> </span><span class="crayon-i">java</span><span class="crayon-sy">.</span><span class="crayon-i">util</span><span class="crayon-sy">.</span><span class="crayon-i">regex</span><span class="crayon-sy">.</span><span class="crayon-i">Pattern</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bcd173085216-3">&nbsp;</div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bcd173085216-4"><span class="crayon-m">public</span><span class="crayon-h"> </span><span class="crayon-t">class</span><span class="crayon-h"> </span><span class="crayon-e">Main</span><span class="crayon-h"> </span><span class="crayon-sy">{</span></div><div class="crayon-line" id="crayon-5d5de958a1bcd173085216-5"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-m">public</span><span class="crayon-h"> </span><span class="crayon-m">static</span><span class="crayon-h"> </span><span class="crayon-t">void</span><span class="crayon-h"> </span><span class="crayon-e">main</span><span class="crayon-sy">(</span><span class="crayon-t">String</span><span class="crayon-sy">[</span><span class="crayon-sy">]</span><span class="crayon-h"> </span><span class="crayon-i">args</span><span class="crayon-sy">)</span><span class="crayon-h"> </span><span class="crayon-sy">{</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bcd173085216-6">&nbsp;</div><div class="crayon-line" id="crayon-5d5de958a1bcd173085216-7"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-c">// 検索する文字列を用意</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bcd173085216-8"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-t">String</span><span class="crayon-h"> </span><span class="crayon-i">str</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-s">"東京都千代田区 123-4567"</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bcd173085216-9">&nbsp;</div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bcd173085216-10"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-c">// 正規表現のパターンを作成</span></div><div class="crayon-line" id="crayon-5d5de958a1bcd173085216-11"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">Pattern</span><span class="crayon-h"> </span><span class="crayon-i">p</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-i">Pattern</span><span class="crayon-sy">.</span><span class="crayon-e">compile</span><span class="crayon-sy">(</span><span class="crayon-s">"[0-9]{3}-[0-9]{4}"</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bcd173085216-12"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">Matcher</span><span class="crayon-h"> </span><span class="crayon-i">m</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-i">p</span><span class="crayon-sy">.</span><span class="crayon-e">matcher</span><span class="crayon-sy">(</span><span class="crayon-i">str</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bcd173085216-13"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-sy">}</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bcd173085216-14"><span class="crayon-sy">}</span></div></div></td>
</tr>
</table>
</div>
</div>

このプログラムでは、文字列「東京都千代田区 123-4567」と正規表現のパターン「[0-9]{3}-[0-9]{4}」のMatcherオブジェクトを作成しています。</p>
<p>このMatcherオブジェクトを使ってチェックや抽出する方法をこれから解説していきます！</p>
<h3><span id="Matcher">一致するかチェックする方法(Matcherクラス)</span></h3>
<p>ここではMatcherクラスのfindメソッドを使って、<span class="red_strong">文字列が正規表現のパターンに一致するかをチェックする方法</span>を解説します。</p>
<p>Matcherクラスのfindメソッドは、文字列の中に正規表現のパターンが含まれる場合に”true”を返し、それ以外の場合には”false”を返します。</p>
<p>次のプログラムでfindメソッドの使い方を確認してみましょう。</p>
[findメソッドの使い方のサンプルコード]

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bcf238412064" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">
<div class="crayon-plain-wrap"><textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 15px !important; line-height: 22px !important;">
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
}</textarea></div>
<div class="crayon-main" style="">
<table class="crayon-table">
<tr class="crayon-row">
<td class="crayon-nums " data-settings="show">
<div class="crayon-nums-content" style="font-size: 15px !important; line-height: 22px !important;"><div class="crayon-num" data-line="crayon-5d5de958a1bcf238412064-1">1</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bcf238412064-2">2</div><div class="crayon-num" data-line="crayon-5d5de958a1bcf238412064-3">3</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bcf238412064-4">4</div><div class="crayon-num" data-line="crayon-5d5de958a1bcf238412064-5">5</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bcf238412064-6">6</div><div class="crayon-num" data-line="crayon-5d5de958a1bcf238412064-7">7</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bcf238412064-8">8</div><div class="crayon-num" data-line="crayon-5d5de958a1bcf238412064-9">9</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bcf238412064-10">10</div><div class="crayon-num" data-line="crayon-5d5de958a1bcf238412064-11">11</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bcf238412064-12">12</div><div class="crayon-num" data-line="crayon-5d5de958a1bcf238412064-13">13</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bcf238412064-14">14</div><div class="crayon-num" data-line="crayon-5d5de958a1bcf238412064-15">15</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bcf238412064-16">16</div></div>
</td>
<td class="crayon-code"><div class="crayon-pre" style="font-size: 15px !important; line-height: 22px !important; -moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4;"><div class="crayon-line" id="crayon-5d5de958a1bcf238412064-1"><span class="crayon-r">import</span><span class="crayon-h"> </span><span class="crayon-i">java</span><span class="crayon-sy">.</span><span class="crayon-i">util</span><span class="crayon-sy">.</span><span class="crayon-i">regex</span><span class="crayon-sy">.</span><span class="crayon-i">Matcher</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bcf238412064-2"><span class="crayon-r">import</span><span class="crayon-h"> </span><span class="crayon-i">java</span><span class="crayon-sy">.</span><span class="crayon-i">util</span><span class="crayon-sy">.</span><span class="crayon-i">regex</span><span class="crayon-sy">.</span><span class="crayon-i">Pattern</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bcf238412064-3">&nbsp;</div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bcf238412064-4"><span class="crayon-m">public</span><span class="crayon-h"> </span><span class="crayon-t">class</span><span class="crayon-h"> </span><span class="crayon-e">Main</span><span class="crayon-h"> </span><span class="crayon-sy">{</span></div><div class="crayon-line" id="crayon-5d5de958a1bcf238412064-5"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-m">public</span><span class="crayon-h"> </span><span class="crayon-m">static</span><span class="crayon-h"> </span><span class="crayon-t">void</span><span class="crayon-h"> </span><span class="crayon-e">main</span><span class="crayon-sy">(</span><span class="crayon-t">String</span><span class="crayon-sy">[</span><span class="crayon-sy">]</span><span class="crayon-h"> </span><span class="crayon-i">args</span><span class="crayon-sy">)</span><span class="crayon-h"> </span><span class="crayon-sy">{</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bcf238412064-6">&nbsp;</div><div class="crayon-line" id="crayon-5d5de958a1bcf238412064-7"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-c">// 検索する文字列を用意</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bcf238412064-8"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-t">String</span><span class="crayon-h"> </span><span class="crayon-i">str</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-s">"東京都千代田区 123-4567"</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bcf238412064-9">&nbsp;</div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bcf238412064-10"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-c">// 正規表現のパターンを作成</span></div><div class="crayon-line" id="crayon-5d5de958a1bcf238412064-11"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">Pattern</span><span class="crayon-h"> </span><span class="crayon-i">p</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-i">Pattern</span><span class="crayon-sy">.</span><span class="crayon-e">compile</span><span class="crayon-sy">(</span><span class="crayon-s">"[0-9]{3}-[0-9]{4}"</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bcf238412064-12"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">Matcher</span><span class="crayon-h"> </span><span class="crayon-i">m</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-i">p</span><span class="crayon-sy">.</span><span class="crayon-e">matcher</span><span class="crayon-sy">(</span><span class="crayon-i">str</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bcf238412064-13">&nbsp;</div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bcf238412064-14"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">System</span><span class="crayon-sy">.</span><span class="crayon-i">out</span><span class="crayon-sy">.</span><span class="crayon-e">println</span><span class="crayon-sy">(</span><span class="crayon-i">m</span><span class="crayon-sy">.</span><span class="crayon-e">find</span><span class="crayon-sy">(</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bcf238412064-15"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-sy">}</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bcf238412064-16"><span class="crayon-sy">}</span></div></div></td>
</tr>
</table>
</div>
</div>

[実行結果]

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bd0945552279" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">
<div class="crayon-plain-wrap"><textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 15px !important; line-height: 22px !important;">
true</textarea></div>
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
[groupメソッドのサンプルコード]

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bd2500274253" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">
<div class="crayon-plain-wrap"><textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 15px !important; line-height: 22px !important;">
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
}</textarea></div>
<div class="crayon-main" style="">
<table class="crayon-table">
<tr class="crayon-row">
<td class="crayon-nums " data-settings="show">
<div class="crayon-nums-content" style="font-size: 15px !important; line-height: 22px !important;"><div class="crayon-num" data-line="crayon-5d5de958a1bd2500274253-1">1</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bd2500274253-2">2</div><div class="crayon-num" data-line="crayon-5d5de958a1bd2500274253-3">3</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bd2500274253-4">4</div><div class="crayon-num" data-line="crayon-5d5de958a1bd2500274253-5">5</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bd2500274253-6">6</div><div class="crayon-num" data-line="crayon-5d5de958a1bd2500274253-7">7</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bd2500274253-8">8</div><div class="crayon-num" data-line="crayon-5d5de958a1bd2500274253-9">9</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bd2500274253-10">10</div><div class="crayon-num" data-line="crayon-5d5de958a1bd2500274253-11">11</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bd2500274253-12">12</div><div class="crayon-num" data-line="crayon-5d5de958a1bd2500274253-13">13</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bd2500274253-14">14</div><div class="crayon-num" data-line="crayon-5d5de958a1bd2500274253-15">15</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bd2500274253-16">16</div><div class="crayon-num" data-line="crayon-5d5de958a1bd2500274253-17">17</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bd2500274253-18">18</div></div>
</td>
<td class="crayon-code"><div class="crayon-pre" style="font-size: 15px !important; line-height: 22px !important; -moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4;"><div class="crayon-line" id="crayon-5d5de958a1bd2500274253-1"><span class="crayon-r">import</span><span class="crayon-h"> </span><span class="crayon-i">java</span><span class="crayon-sy">.</span><span class="crayon-i">util</span><span class="crayon-sy">.</span><span class="crayon-i">regex</span><span class="crayon-sy">.</span><span class="crayon-i">Matcher</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bd2500274253-2"><span class="crayon-r">import</span><span class="crayon-h"> </span><span class="crayon-i">java</span><span class="crayon-sy">.</span><span class="crayon-i">util</span><span class="crayon-sy">.</span><span class="crayon-i">regex</span><span class="crayon-sy">.</span><span class="crayon-i">Pattern</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bd2500274253-3">&nbsp;</div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bd2500274253-4"><span class="crayon-m">public</span><span class="crayon-h"> </span><span class="crayon-t">class</span><span class="crayon-h"> </span><span class="crayon-e">Main</span><span class="crayon-h"> </span><span class="crayon-sy">{</span></div><div class="crayon-line" id="crayon-5d5de958a1bd2500274253-5"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-m">public</span><span class="crayon-h"> </span><span class="crayon-m">static</span><span class="crayon-h"> </span><span class="crayon-t">void</span><span class="crayon-h"> </span><span class="crayon-e">main</span><span class="crayon-sy">(</span><span class="crayon-t">String</span><span class="crayon-sy">[</span><span class="crayon-sy">]</span><span class="crayon-h"> </span><span class="crayon-i">args</span><span class="crayon-sy">)</span><span class="crayon-h"> </span><span class="crayon-sy">{</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bd2500274253-6">&nbsp;</div><div class="crayon-line" id="crayon-5d5de958a1bd2500274253-7"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-c">// 検索する文字列を用意</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bd2500274253-8"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-t">String</span><span class="crayon-h"> </span><span class="crayon-i">str</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-s">"東京都千代田区 123-4567, 東京都渋谷区 111-2233"</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bd2500274253-9">&nbsp;</div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bd2500274253-10"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-c">// 正規表現のパターンを作成</span></div><div class="crayon-line" id="crayon-5d5de958a1bd2500274253-11"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">Pattern</span><span class="crayon-h"> </span><span class="crayon-i">p</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-i">Pattern</span><span class="crayon-sy">.</span><span class="crayon-e">compile</span><span class="crayon-sy">(</span><span class="crayon-s">"[0-9]{3}-[0-9]{4}"</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bd2500274253-12"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">Matcher</span><span class="crayon-h"> </span><span class="crayon-i">m</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-i">p</span><span class="crayon-sy">.</span><span class="crayon-e">matcher</span><span class="crayon-sy">(</span><span class="crayon-i">str</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bd2500274253-13">&nbsp;</div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bd2500274253-14"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-st">while</span><span class="crayon-h"> </span><span class="crayon-sy">(</span><span class="crayon-i">m</span><span class="crayon-sy">.</span><span class="crayon-e">find</span><span class="crayon-sy">(</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-h"> </span><span class="crayon-sy">{</span></div><div class="crayon-line" id="crayon-5d5de958a1bd2500274253-15"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">System</span><span class="crayon-sy">.</span><span class="crayon-i">out</span><span class="crayon-sy">.</span><span class="crayon-e">println</span><span class="crayon-sy">(</span><span class="crayon-s">"一致した部分は : "</span><span class="crayon-h"> </span>+<span class="crayon-h"> </span><span class="crayon-i">m</span><span class="crayon-sy">.</span><span class="crayon-e">group</span><span class="crayon-sy">(</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bd2500274253-16"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-sy">}</span></div><div class="crayon-line" id="crayon-5d5de958a1bd2500274253-17"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-sy">}</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bd2500274253-18"><span class="crayon-sy">}</span></div></div></td>
</tr>
</table>
</div>
</div>

[実行結果]

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bd4381980823" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">
<div class="crayon-plain-wrap"><textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 15px !important; line-height: 22px !important;">
一致した部分は : 123-4567
一致した部分は : 111-2233</textarea></div>
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
<p>\ * + . ? { } ( ) [ ] ^ $ &#8211; |</p>
<p>エスケープ処理の例は以下のとおりです。</p>
<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bd6201799099" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">
<div class="crayon-plain-wrap"><textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 15px !important; line-height: 22px !important;">
\  →  \\\
*  →  \\*
{} →  \\{\\}</textarea></div>
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
<div class="crayon-plain-wrap"><textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 15px !important; line-height: 22px !important;">
^[0-9]+$</textarea></div>
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
[数字のチェックをするサンプルコード]

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bd9496513020" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">
<div class="crayon-plain-wrap"><textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 15px !important; line-height: 22px !important;">
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
}</textarea></div>
<div class="crayon-main" style="">
<table class="crayon-table">
<tr class="crayon-row">
<td class="crayon-nums " data-settings="show">
<div class="crayon-nums-content" style="font-size: 15px !important; line-height: 22px !important;"><div class="crayon-num" data-line="crayon-5d5de958a1bd9496513020-1">1</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bd9496513020-2">2</div><div class="crayon-num" data-line="crayon-5d5de958a1bd9496513020-3">3</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bd9496513020-4">4</div><div class="crayon-num" data-line="crayon-5d5de958a1bd9496513020-5">5</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bd9496513020-6">6</div><div class="crayon-num" data-line="crayon-5d5de958a1bd9496513020-7">7</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bd9496513020-8">8</div><div class="crayon-num" data-line="crayon-5d5de958a1bd9496513020-9">9</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bd9496513020-10">10</div><div class="crayon-num" data-line="crayon-5d5de958a1bd9496513020-11">11</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bd9496513020-12">12</div><div class="crayon-num" data-line="crayon-5d5de958a1bd9496513020-13">13</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bd9496513020-14">14</div><div class="crayon-num" data-line="crayon-5d5de958a1bd9496513020-15">15</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bd9496513020-16">16</div></div>
</td>
<td class="crayon-code"><div class="crayon-pre" style="font-size: 15px !important; line-height: 22px !important; -moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4;"><div class="crayon-line" id="crayon-5d5de958a1bd9496513020-1"><span class="crayon-r">import</span><span class="crayon-h"> </span><span class="crayon-i">java</span><span class="crayon-sy">.</span><span class="crayon-i">util</span><span class="crayon-sy">.</span><span class="crayon-i">regex</span><span class="crayon-sy">.</span><span class="crayon-i">Matcher</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bd9496513020-2"><span class="crayon-r">import</span><span class="crayon-h"> </span><span class="crayon-i">java</span><span class="crayon-sy">.</span><span class="crayon-i">util</span><span class="crayon-sy">.</span><span class="crayon-i">regex</span><span class="crayon-sy">.</span><span class="crayon-i">Pattern</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bd9496513020-3"><span class="crayon-h"> </span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bd9496513020-4"><span class="crayon-m">public</span><span class="crayon-h"> </span><span class="crayon-t">class</span><span class="crayon-h"> </span><span class="crayon-e">Main</span><span class="crayon-h"> </span><span class="crayon-sy">{</span></div><div class="crayon-line" id="crayon-5d5de958a1bd9496513020-5"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-m">public</span><span class="crayon-h"> </span><span class="crayon-m">static</span><span class="crayon-h"> </span><span class="crayon-t">void</span><span class="crayon-h"> </span><span class="crayon-e">main</span><span class="crayon-sy">(</span><span class="crayon-t">String</span><span class="crayon-sy">[</span><span class="crayon-sy">]</span><span class="crayon-h"> </span><span class="crayon-i">args</span><span class="crayon-sy">)</span><span class="crayon-h"> </span><span class="crayon-sy">{</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bd9496513020-6"><span class="crayon-h"> </span></div><div class="crayon-line" id="crayon-5d5de958a1bd9496513020-7"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-c">// 検索する文字列を用意</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bd9496513020-8"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-t">String</span><span class="crayon-h"> </span><span class="crayon-i">str</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-s">"123"</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bd9496513020-9"><span class="crayon-h"> </span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bd9496513020-10"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-c">// 正規表現のパターンを作成</span></div><div class="crayon-line" id="crayon-5d5de958a1bd9496513020-11"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">Pattern</span><span class="crayon-h"> </span><span class="crayon-i">p</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-i">Pattern</span><span class="crayon-sy">.</span><span class="crayon-e">compile</span><span class="crayon-sy">(</span><span class="crayon-s">"^[0-9]+$"</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bd9496513020-12"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">Matcher</span><span class="crayon-h"> </span><span class="crayon-i">m</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-i">p</span><span class="crayon-sy">.</span><span class="crayon-e">matcher</span><span class="crayon-sy">(</span><span class="crayon-i">str</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bd9496513020-13"><span class="crayon-h"> </span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bd9496513020-14"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">System</span><span class="crayon-sy">.</span><span class="crayon-i">out</span><span class="crayon-sy">.</span><span class="crayon-e">println</span><span class="crayon-sy">(</span><span class="crayon-i">m</span><span class="crayon-sy">.</span><span class="crayon-e">find</span><span class="crayon-sy">(</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bd9496513020-15"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-sy">}</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bd9496513020-16"><span class="crayon-sy">}</span></div></div></td>
</tr>
</table>
</div>
</div>

[実行結果]

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bda012944083" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">
<div class="crayon-plain-wrap"><textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 15px !important; line-height: 22px !important;">
true</textarea></div>
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
<div class="crayon-plain-wrap"><textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 15px !important; line-height: 22px !important;">
^[0-9a-zA-Z]+$</textarea></div>
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
[英数字のチェックをするサンプルコード]

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bdd902497003" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">
<div class="crayon-plain-wrap"><textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 15px !important; line-height: 22px !important;">
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
}</textarea></div>
<div class="crayon-main" style="">
<table class="crayon-table">
<tr class="crayon-row">
<td class="crayon-nums " data-settings="show">
<div class="crayon-nums-content" style="font-size: 15px !important; line-height: 22px !important;"><div class="crayon-num" data-line="crayon-5d5de958a1bdd902497003-1">1</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bdd902497003-2">2</div><div class="crayon-num" data-line="crayon-5d5de958a1bdd902497003-3">3</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bdd902497003-4">4</div><div class="crayon-num" data-line="crayon-5d5de958a1bdd902497003-5">5</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bdd902497003-6">6</div><div class="crayon-num" data-line="crayon-5d5de958a1bdd902497003-7">7</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bdd902497003-8">8</div><div class="crayon-num" data-line="crayon-5d5de958a1bdd902497003-9">9</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bdd902497003-10">10</div><div class="crayon-num" data-line="crayon-5d5de958a1bdd902497003-11">11</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bdd902497003-12">12</div><div class="crayon-num" data-line="crayon-5d5de958a1bdd902497003-13">13</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bdd902497003-14">14</div><div class="crayon-num" data-line="crayon-5d5de958a1bdd902497003-15">15</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bdd902497003-16">16</div></div>
</td>
<td class="crayon-code"><div class="crayon-pre" style="font-size: 15px !important; line-height: 22px !important; -moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4;"><div class="crayon-line" id="crayon-5d5de958a1bdd902497003-1"><span class="crayon-r">import</span><span class="crayon-h"> </span><span class="crayon-i">java</span><span class="crayon-sy">.</span><span class="crayon-i">util</span><span class="crayon-sy">.</span><span class="crayon-i">regex</span><span class="crayon-sy">.</span><span class="crayon-i">Matcher</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bdd902497003-2"><span class="crayon-r">import</span><span class="crayon-h"> </span><span class="crayon-i">java</span><span class="crayon-sy">.</span><span class="crayon-i">util</span><span class="crayon-sy">.</span><span class="crayon-i">regex</span><span class="crayon-sy">.</span><span class="crayon-i">Pattern</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bdd902497003-3"><span class="crayon-h"> </span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bdd902497003-4"><span class="crayon-m">public</span><span class="crayon-h"> </span><span class="crayon-t">class</span><span class="crayon-h"> </span><span class="crayon-e">Main</span><span class="crayon-h"> </span><span class="crayon-sy">{</span></div><div class="crayon-line" id="crayon-5d5de958a1bdd902497003-5"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-m">public</span><span class="crayon-h"> </span><span class="crayon-m">static</span><span class="crayon-h"> </span><span class="crayon-t">void</span><span class="crayon-h"> </span><span class="crayon-e">main</span><span class="crayon-sy">(</span><span class="crayon-t">String</span><span class="crayon-sy">[</span><span class="crayon-sy">]</span><span class="crayon-h"> </span><span class="crayon-i">args</span><span class="crayon-sy">)</span><span class="crayon-h"> </span><span class="crayon-sy">{</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bdd902497003-6"><span class="crayon-h"> </span></div><div class="crayon-line" id="crayon-5d5de958a1bdd902497003-7"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-c">// 検索する文字列を用意</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bdd902497003-8"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-t">String</span><span class="crayon-h"> </span><span class="crayon-i">str</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-s">"SAMURAI123"</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bdd902497003-9"><span class="crayon-h"> </span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bdd902497003-10"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-c">// 正規表現のパターンを作成</span></div><div class="crayon-line" id="crayon-5d5de958a1bdd902497003-11"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">Pattern</span><span class="crayon-h"> </span><span class="crayon-i">p</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-i">Pattern</span><span class="crayon-sy">.</span><span class="crayon-e">compile</span><span class="crayon-sy">(</span><span class="crayon-s">"^[0-9a-zA-Z]+$"</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bdd902497003-12"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">Matcher</span><span class="crayon-h"> </span><span class="crayon-i">m</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-i">p</span><span class="crayon-sy">.</span><span class="crayon-e">matcher</span><span class="crayon-sy">(</span><span class="crayon-i">str</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bdd902497003-13"><span class="crayon-h"> </span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bdd902497003-14"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">System</span><span class="crayon-sy">.</span><span class="crayon-i">out</span><span class="crayon-sy">.</span><span class="crayon-e">println</span><span class="crayon-sy">(</span><span class="crayon-i">m</span><span class="crayon-sy">.</span><span class="crayon-e">find</span><span class="crayon-sy">(</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bdd902497003-15"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-sy">}</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bdd902497003-16"><span class="crayon-sy">}</span></div></div></td>
</tr>
</table>
</div>
</div>

[実行結果]

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bde110765501" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">
<div class="crayon-plain-wrap"><textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 15px !important; line-height: 22px !important;">
true</textarea></div>
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
<div class="crayon-plain-wrap"><textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 15px !important; line-height: 22px !important;">
^[0-9]{4}/[0-9]{2}/[0-9]{2}$</textarea></div>
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
<p>日付が正しいかをSimpleDateFormatクラスのsetLenientメソッドを使用して厳密にチェックする方法はこちらで詳しく解説しているので参考にしてください。</p>
<a href="https://www.sejuku.net/blog/20994" class="blog-card-anchor">
<div class="blog-card">
<div class="blog-card-content">
<div class="blog-card-thumbnail"><img class="blog-card-thumbnail-img" src="https://s3-ap-northeast-1.amazonaws.com/samurai-blog-media/blog/wp-content/uploads/2017/04/JAVA-209941-150x85.jpg"></div>
<div class="blog-card-title-wrap">
<div class="blog-card-title">【Java入門】SimpleDateFormatで日付フォーマットの設定</div>
<div class="blog-card-update-date">更新日 : 2019年5月28日</div>
</div>
</div>
</div>
</a>
<p><span class="red_strong">正規表現で簡易的にチェックする方法</span>を次のプログラムで確認してみましょう。</p>
[日付のチェックをするサンプルコード]

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1be1187390847" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">
<div class="crayon-plain-wrap"><textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 15px !important; line-height: 22px !important;">
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
}</textarea></div>
<div class="crayon-main" style="">
<table class="crayon-table">
<tr class="crayon-row">
<td class="crayon-nums " data-settings="show">
<div class="crayon-nums-content" style="font-size: 15px !important; line-height: 22px !important;"><div class="crayon-num" data-line="crayon-5d5de958a1be1187390847-1">1</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1be1187390847-2">2</div><div class="crayon-num" data-line="crayon-5d5de958a1be1187390847-3">3</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1be1187390847-4">4</div><div class="crayon-num" data-line="crayon-5d5de958a1be1187390847-5">5</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1be1187390847-6">6</div><div class="crayon-num" data-line="crayon-5d5de958a1be1187390847-7">7</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1be1187390847-8">8</div><div class="crayon-num" data-line="crayon-5d5de958a1be1187390847-9">9</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1be1187390847-10">10</div><div class="crayon-num" data-line="crayon-5d5de958a1be1187390847-11">11</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1be1187390847-12">12</div><div class="crayon-num" data-line="crayon-5d5de958a1be1187390847-13">13</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1be1187390847-14">14</div><div class="crayon-num" data-line="crayon-5d5de958a1be1187390847-15">15</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1be1187390847-16">16</div></div>
</td>
<td class="crayon-code"><div class="crayon-pre" style="font-size: 15px !important; line-height: 22px !important; -moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4;"><div class="crayon-line" id="crayon-5d5de958a1be1187390847-1"><span class="crayon-r">import</span><span class="crayon-h"> </span><span class="crayon-i">java</span><span class="crayon-sy">.</span><span class="crayon-i">util</span><span class="crayon-sy">.</span><span class="crayon-i">regex</span><span class="crayon-sy">.</span><span class="crayon-i">Matcher</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1be1187390847-2"><span class="crayon-r">import</span><span class="crayon-h"> </span><span class="crayon-i">java</span><span class="crayon-sy">.</span><span class="crayon-i">util</span><span class="crayon-sy">.</span><span class="crayon-i">regex</span><span class="crayon-sy">.</span><span class="crayon-i">Pattern</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1be1187390847-3"><span class="crayon-h"> </span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1be1187390847-4"><span class="crayon-m">public</span><span class="crayon-h"> </span><span class="crayon-t">class</span><span class="crayon-h"> </span><span class="crayon-e">Main</span><span class="crayon-h"> </span><span class="crayon-sy">{</span></div><div class="crayon-line" id="crayon-5d5de958a1be1187390847-5"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-m">public</span><span class="crayon-h"> </span><span class="crayon-m">static</span><span class="crayon-h"> </span><span class="crayon-t">void</span><span class="crayon-h"> </span><span class="crayon-e">main</span><span class="crayon-sy">(</span><span class="crayon-t">String</span><span class="crayon-sy">[</span><span class="crayon-sy">]</span><span class="crayon-h"> </span><span class="crayon-i">args</span><span class="crayon-sy">)</span><span class="crayon-h"> </span><span class="crayon-sy">{</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1be1187390847-6"><span class="crayon-h"> </span></div><div class="crayon-line" id="crayon-5d5de958a1be1187390847-7"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-c">// 検索する文字列を用意</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1be1187390847-8"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-t">String</span><span class="crayon-h"> </span><span class="crayon-i">str</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-s">"2017/06/28"</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1be1187390847-9"><span class="crayon-h"> </span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1be1187390847-10"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-c">// 正規表現のパターンを作成</span></div><div class="crayon-line" id="crayon-5d5de958a1be1187390847-11"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">Pattern</span><span class="crayon-h"> </span><span class="crayon-i">p</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-i">Pattern</span><span class="crayon-sy">.</span><span class="crayon-e">compile</span><span class="crayon-sy">(</span><span class="crayon-s">"^[0-9]{4}/[0-9]{2}/[0-9]{2}$"</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1be1187390847-12"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">Matcher</span><span class="crayon-h"> </span><span class="crayon-i">m</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-i">p</span><span class="crayon-sy">.</span><span class="crayon-e">matcher</span><span class="crayon-sy">(</span><span class="crayon-i">str</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1be1187390847-13"><span class="crayon-h"> </span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1be1187390847-14"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">System</span><span class="crayon-sy">.</span><span class="crayon-i">out</span><span class="crayon-sy">.</span><span class="crayon-e">println</span><span class="crayon-sy">(</span><span class="crayon-i">m</span><span class="crayon-sy">.</span><span class="crayon-e">find</span><span class="crayon-sy">(</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1be1187390847-15"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-sy">}</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1be1187390847-16"><span class="crayon-sy">}</span></div></div></td>
</tr>
</table>
</div>
</div>

[実行結果]

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1be2174456946" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">
<div class="crayon-plain-wrap"><textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 15px !important; line-height: 22px !important;">
true</textarea></div>
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
<div class="crayon-plain-wrap"><textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 15px !important; line-height: 22px !important;">
^[0-9]{3}-[0-9]{4}-[0-9]{4}$</textarea></div>
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
[電話番号のチェックをするサンプルコード]

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1be5105843512" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">
<div class="crayon-plain-wrap"><textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 15px !important; line-height: 22px !important;">
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
}</textarea></div>
<div class="crayon-main" style="">
<table class="crayon-table">
<tr class="crayon-row">
<td class="crayon-nums " data-settings="show">
<div class="crayon-nums-content" style="font-size: 15px !important; line-height: 22px !important;"><div class="crayon-num" data-line="crayon-5d5de958a1be5105843512-1">1</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1be5105843512-2">2</div><div class="crayon-num" data-line="crayon-5d5de958a1be5105843512-3">3</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1be5105843512-4">4</div><div class="crayon-num" data-line="crayon-5d5de958a1be5105843512-5">5</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1be5105843512-6">6</div><div class="crayon-num" data-line="crayon-5d5de958a1be5105843512-7">7</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1be5105843512-8">8</div><div class="crayon-num" data-line="crayon-5d5de958a1be5105843512-9">9</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1be5105843512-10">10</div><div class="crayon-num" data-line="crayon-5d5de958a1be5105843512-11">11</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1be5105843512-12">12</div><div class="crayon-num" data-line="crayon-5d5de958a1be5105843512-13">13</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1be5105843512-14">14</div><div class="crayon-num" data-line="crayon-5d5de958a1be5105843512-15">15</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1be5105843512-16">16</div></div>
</td>
<td class="crayon-code"><div class="crayon-pre" style="font-size: 15px !important; line-height: 22px !important; -moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4;"><div class="crayon-line" id="crayon-5d5de958a1be5105843512-1"><span class="crayon-r">import</span><span class="crayon-h"> </span><span class="crayon-i">java</span><span class="crayon-sy">.</span><span class="crayon-i">util</span><span class="crayon-sy">.</span><span class="crayon-i">regex</span><span class="crayon-sy">.</span><span class="crayon-i">Matcher</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1be5105843512-2"><span class="crayon-r">import</span><span class="crayon-h"> </span><span class="crayon-i">java</span><span class="crayon-sy">.</span><span class="crayon-i">util</span><span class="crayon-sy">.</span><span class="crayon-i">regex</span><span class="crayon-sy">.</span><span class="crayon-i">Pattern</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1be5105843512-3"><span class="crayon-h"> </span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1be5105843512-4"><span class="crayon-m">public</span><span class="crayon-h"> </span><span class="crayon-t">class</span><span class="crayon-h"> </span><span class="crayon-e">Main</span><span class="crayon-h"> </span><span class="crayon-sy">{</span></div><div class="crayon-line" id="crayon-5d5de958a1be5105843512-5"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-m">public</span><span class="crayon-h"> </span><span class="crayon-m">static</span><span class="crayon-h"> </span><span class="crayon-t">void</span><span class="crayon-h"> </span><span class="crayon-e">main</span><span class="crayon-sy">(</span><span class="crayon-t">String</span><span class="crayon-sy">[</span><span class="crayon-sy">]</span><span class="crayon-h"> </span><span class="crayon-i">args</span><span class="crayon-sy">)</span><span class="crayon-h"> </span><span class="crayon-sy">{</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1be5105843512-6"><span class="crayon-h"> </span></div><div class="crayon-line" id="crayon-5d5de958a1be5105843512-7"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-c">// 検索する文字列を用意</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1be5105843512-8"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-t">String</span><span class="crayon-h"> </span><span class="crayon-i">str</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-s">"090-1234-5678"</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1be5105843512-9"><span class="crayon-h"> </span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1be5105843512-10"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-c">// 正規表現のパターンを作成</span></div><div class="crayon-line" id="crayon-5d5de958a1be5105843512-11"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">Pattern</span><span class="crayon-h"> </span><span class="crayon-i">p</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-i">Pattern</span><span class="crayon-sy">.</span><span class="crayon-e">compile</span><span class="crayon-sy">(</span><span class="crayon-s">"^[0-9]{3}-[0-9]{4}-[0-9]{4}$"</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1be5105843512-12"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">Matcher</span><span class="crayon-h"> </span><span class="crayon-i">m</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-i">p</span><span class="crayon-sy">.</span><span class="crayon-e">matcher</span><span class="crayon-sy">(</span><span class="crayon-i">str</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1be5105843512-13"><span class="crayon-h"> </span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1be5105843512-14"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">System</span><span class="crayon-sy">.</span><span class="crayon-i">out</span><span class="crayon-sy">.</span><span class="crayon-e">println</span><span class="crayon-sy">(</span><span class="crayon-i">m</span><span class="crayon-sy">.</span><span class="crayon-e">find</span><span class="crayon-sy">(</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1be5105843512-15"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-sy">}</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1be5105843512-16"><span class="crayon-sy">}</span></div></div></td>
</tr>
</table>
</div>
</div>

[実行結果]

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1be6217131078" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">
<div class="crayon-plain-wrap"><textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 15px !important; line-height: 22px !important;">
true</textarea></div>
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
<div class="crayon-plain-wrap"><textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 15px !important; line-height: 22px !important;">
^[0-9]{3}-[0-9]{4}$</textarea></div>
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
[郵便番号のチェックをするサンプルコード]

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1be9304545844" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">
<div class="crayon-plain-wrap"><textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 15px !important; line-height: 22px !important;">
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
}</textarea></div>
<div class="crayon-main" style="">
<table class="crayon-table">
<tr class="crayon-row">
<td class="crayon-nums " data-settings="show">
<div class="crayon-nums-content" style="font-size: 15px !important; line-height: 22px !important;"><div class="crayon-num" data-line="crayon-5d5de958a1be9304545844-1">1</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1be9304545844-2">2</div><div class="crayon-num" data-line="crayon-5d5de958a1be9304545844-3">3</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1be9304545844-4">4</div><div class="crayon-num" data-line="crayon-5d5de958a1be9304545844-5">5</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1be9304545844-6">6</div><div class="crayon-num" data-line="crayon-5d5de958a1be9304545844-7">7</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1be9304545844-8">8</div><div class="crayon-num" data-line="crayon-5d5de958a1be9304545844-9">9</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1be9304545844-10">10</div><div class="crayon-num" data-line="crayon-5d5de958a1be9304545844-11">11</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1be9304545844-12">12</div><div class="crayon-num" data-line="crayon-5d5de958a1be9304545844-13">13</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1be9304545844-14">14</div><div class="crayon-num" data-line="crayon-5d5de958a1be9304545844-15">15</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1be9304545844-16">16</div></div>
</td>
<td class="crayon-code"><div class="crayon-pre" style="font-size: 15px !important; line-height: 22px !important; -moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4;"><div class="crayon-line" id="crayon-5d5de958a1be9304545844-1"><span class="crayon-r">import</span><span class="crayon-h"> </span><span class="crayon-i">java</span><span class="crayon-sy">.</span><span class="crayon-i">util</span><span class="crayon-sy">.</span><span class="crayon-i">regex</span><span class="crayon-sy">.</span><span class="crayon-i">Matcher</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1be9304545844-2"><span class="crayon-r">import</span><span class="crayon-h"> </span><span class="crayon-i">java</span><span class="crayon-sy">.</span><span class="crayon-i">util</span><span class="crayon-sy">.</span><span class="crayon-i">regex</span><span class="crayon-sy">.</span><span class="crayon-i">Pattern</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1be9304545844-3"><span class="crayon-h"> </span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1be9304545844-4"><span class="crayon-m">public</span><span class="crayon-h"> </span><span class="crayon-t">class</span><span class="crayon-h"> </span><span class="crayon-e">Main</span><span class="crayon-h"> </span><span class="crayon-sy">{</span></div><div class="crayon-line" id="crayon-5d5de958a1be9304545844-5"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-m">public</span><span class="crayon-h"> </span><span class="crayon-m">static</span><span class="crayon-h"> </span><span class="crayon-t">void</span><span class="crayon-h"> </span><span class="crayon-e">main</span><span class="crayon-sy">(</span><span class="crayon-t">String</span><span class="crayon-sy">[</span><span class="crayon-sy">]</span><span class="crayon-h"> </span><span class="crayon-i">args</span><span class="crayon-sy">)</span><span class="crayon-h"> </span><span class="crayon-sy">{</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1be9304545844-6"><span class="crayon-h"> </span></div><div class="crayon-line" id="crayon-5d5de958a1be9304545844-7"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-c">// 検索する文字列を用意</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1be9304545844-8"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-t">String</span><span class="crayon-h"> </span><span class="crayon-i">str</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-s">"123-4567"</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1be9304545844-9"><span class="crayon-h"> </span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1be9304545844-10"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-c">// 正規表現のパターンを作成</span></div><div class="crayon-line" id="crayon-5d5de958a1be9304545844-11"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">Pattern</span><span class="crayon-h"> </span><span class="crayon-i">p</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-i">Pattern</span><span class="crayon-sy">.</span><span class="crayon-e">compile</span><span class="crayon-sy">(</span><span class="crayon-s">"^[0-9]{3}-[0-9]{4}$"</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1be9304545844-12"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">Matcher</span><span class="crayon-h"> </span><span class="crayon-i">m</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-i">p</span><span class="crayon-sy">.</span><span class="crayon-e">matcher</span><span class="crayon-sy">(</span><span class="crayon-i">str</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1be9304545844-13"><span class="crayon-h"> </span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1be9304545844-14"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">System</span><span class="crayon-sy">.</span><span class="crayon-i">out</span><span class="crayon-sy">.</span><span class="crayon-e">println</span><span class="crayon-sy">(</span><span class="crayon-i">m</span><span class="crayon-sy">.</span><span class="crayon-e">find</span><span class="crayon-sy">(</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1be9304545844-15"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-sy">}</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1be9304545844-16"><span class="crayon-sy">}</span></div></div></td>
</tr>
</table>
</div>
</div>

[実行結果]

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bea430642836" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">
<div class="crayon-plain-wrap"><textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 15px !important; line-height: 22px !important;">
true</textarea></div>
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
<div class="crayon-plain-wrap"><textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 15px !important; line-height: 22px !important;">
^(([1-9]?[0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])\\.){3}([1-9]?[0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])$</textarea></div>
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
[IPv4のアドレスのチェックをするサンプルコード]

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bed790164709" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">
<div class="crayon-plain-wrap"><textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 15px !important; line-height: 22px !important;">
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
}</textarea></div>
<div class="crayon-main" style="">
<table class="crayon-table">
<tr class="crayon-row">
<td class="crayon-nums " data-settings="show">
<div class="crayon-nums-content" style="font-size: 15px !important; line-height: 22px !important;"><div class="crayon-num" data-line="crayon-5d5de958a1bed790164709-1">1</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bed790164709-2">2</div><div class="crayon-num" data-line="crayon-5d5de958a1bed790164709-3">3</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bed790164709-4">4</div><div class="crayon-num" data-line="crayon-5d5de958a1bed790164709-5">5</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bed790164709-6">6</div><div class="crayon-num" data-line="crayon-5d5de958a1bed790164709-7">7</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bed790164709-8">8</div><div class="crayon-num" data-line="crayon-5d5de958a1bed790164709-9">9</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bed790164709-10">10</div><div class="crayon-num" data-line="crayon-5d5de958a1bed790164709-11">11</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bed790164709-12">12</div><div class="crayon-num" data-line="crayon-5d5de958a1bed790164709-13">13</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bed790164709-14">14</div><div class="crayon-num" data-line="crayon-5d5de958a1bed790164709-15">15</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bed790164709-16">16</div><div class="crayon-num" data-line="crayon-5d5de958a1bed790164709-17">17</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bed790164709-18">18</div></div>
</td>
<td class="crayon-code"><div class="crayon-pre" style="font-size: 15px !important; line-height: 22px !important; -moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4;"><div class="crayon-line" id="crayon-5d5de958a1bed790164709-1"><span class="crayon-r">import</span><span class="crayon-h"> </span><span class="crayon-i">java</span><span class="crayon-sy">.</span><span class="crayon-i">util</span><span class="crayon-sy">.</span><span class="crayon-i">regex</span><span class="crayon-sy">.</span><span class="crayon-i">Matcher</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bed790164709-2"><span class="crayon-r">import</span><span class="crayon-h"> </span><span class="crayon-i">java</span><span class="crayon-sy">.</span><span class="crayon-i">util</span><span class="crayon-sy">.</span><span class="crayon-i">regex</span><span class="crayon-sy">.</span><span class="crayon-i">Pattern</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bed790164709-3"><span class="crayon-h"> </span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bed790164709-4"><span class="crayon-m">public</span><span class="crayon-h"> </span><span class="crayon-t">class</span><span class="crayon-h"> </span><span class="crayon-e">Main</span><span class="crayon-h"> </span><span class="crayon-sy">{</span></div><div class="crayon-line" id="crayon-5d5de958a1bed790164709-5"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-m">public</span><span class="crayon-h"> </span><span class="crayon-m">static</span><span class="crayon-h"> </span><span class="crayon-t">void</span><span class="crayon-h"> </span><span class="crayon-e">main</span><span class="crayon-sy">(</span><span class="crayon-t">String</span><span class="crayon-sy">[</span><span class="crayon-sy">]</span><span class="crayon-h"> </span><span class="crayon-i">args</span><span class="crayon-sy">)</span><span class="crayon-h"> </span><span class="crayon-sy">{</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bed790164709-6"><span class="crayon-h"> </span></div><div class="crayon-line" id="crayon-5d5de958a1bed790164709-7"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-c">// 検索する文字列を用意</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bed790164709-8"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-t">String</span><span class="crayon-h"> </span><span class="crayon-i">str</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-s">"192.168.0.10"</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bed790164709-9"><span class="crayon-h"> </span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bed790164709-10"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-c">// 正規表現のパターンを作成</span></div><div class="crayon-line" id="crayon-5d5de958a1bed790164709-11"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">Pattern</span><span class="crayon-h"> </span><span class="crayon-i">p</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-i">Pattern</span><span class="crayon-sy">.</span><span class="crayon-e">compile</span><span class="crayon-sy">(</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bed790164709-12"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-s">"^(([1-9]?[0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])\\.){3}([1-9]?[0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])$"</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bed790164709-13"><span class="crayon-h"> </span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bed790164709-14"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">Matcher</span><span class="crayon-h"> </span><span class="crayon-i">m</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-i">p</span><span class="crayon-sy">.</span><span class="crayon-e">matcher</span><span class="crayon-sy">(</span><span class="crayon-i">str</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bed790164709-15"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">System</span><span class="crayon-sy">.</span><span class="crayon-i">out</span><span class="crayon-sy">.</span><span class="crayon-e">println</span><span class="crayon-sy">(</span><span class="crayon-i">m</span><span class="crayon-sy">.</span><span class="crayon-e">find</span><span class="crayon-sy">(</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bed790164709-16"><span class="crayon-h"> </span></div><div class="crayon-line" id="crayon-5d5de958a1bed790164709-17"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-sy">}</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bed790164709-18"><span class="crayon-sy">}</span></div></div></td>
</tr>
</table>
</div>
</div>

[実行結果]

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bef147717154" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">
<div class="crayon-plain-wrap"><textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 15px !important; line-height: 22px !important;">
true</textarea></div>
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
<div class="crayon-plain-wrap"><textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 15px !important; line-height: 22px !important;">
^\\s*((([0-9A-Fa-f]{1,4}:){7}([0-9A-Fa-f]{1,4}|:))|(([0-9A-Fa-f]{1,4}:){6}(:[0-9A-Fa-f]{1,4}|((25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)(\\.(25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)){3})|:))|(([0-9A-Fa-f]{1,4}:){5}(((:[0-9A-Fa-f]{1,4}){1,2})|:((25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)(\\.(25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)){3})|:))|(([0-9A-Fa-f]{1,4}:){4}(((:[0-9A-Fa-f]{1,4}){1,3})|((:[0-9A-Fa-f]{1,4})?:((25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)(\\.(25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)){3}))|:))|(([0-9A-Fa-f]{1,4}:){3}(((:[0-9A-Fa-f]{1,4}){1,4})|((:[0-9A-Fa-f]{1,4}){0,2}:((25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)(\\.(25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)){3}))|:))|(([0-9A-Fa-f]{1,4}:){2}(((:[0-9A-Fa-f]{1,4}){1,5})|((:[0-9A-Fa-f]{1,4}){0,3}:((25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)(\\.(25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)){3}))|:))|(([0-9A-Fa-f]{1,4}:){1}(((:[0-9A-Fa-f]{1,4}){1,6})|((:[0-9A-Fa-f]{1,4}){0,4}:((25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)(\\.(25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)){3}))|:))|(:(((:[0-9A-Fa-f]{1,4}){1,7})|((:[0-9A-Fa-f]{1,4}){0,5}:((25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)(\\.(25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)){3}))|:)))(%.+)?\\s*$</textarea></div>
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
[IPv6のアドレスのチェックをするサンプルコード]

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bf3958824701" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">
<div class="crayon-plain-wrap"><textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 15px !important; line-height: 22px !important;">
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
}</textarea></div>
<div class="crayon-main" style="">
<table class="crayon-table">
<tr class="crayon-row">
<td class="crayon-nums " data-settings="show">
<div class="crayon-nums-content" style="font-size: 15px !important; line-height: 22px !important;"><div class="crayon-num" data-line="crayon-5d5de958a1bf3958824701-1">1</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bf3958824701-2">2</div><div class="crayon-num" data-line="crayon-5d5de958a1bf3958824701-3">3</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bf3958824701-4">4</div><div class="crayon-num" data-line="crayon-5d5de958a1bf3958824701-5">5</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bf3958824701-6">6</div><div class="crayon-num" data-line="crayon-5d5de958a1bf3958824701-7">7</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bf3958824701-8">8</div><div class="crayon-num" data-line="crayon-5d5de958a1bf3958824701-9">9</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bf3958824701-10">10</div><div class="crayon-num" data-line="crayon-5d5de958a1bf3958824701-11">11</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bf3958824701-12">12</div><div class="crayon-num" data-line="crayon-5d5de958a1bf3958824701-13">13</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bf3958824701-14">14</div><div class="crayon-num" data-line="crayon-5d5de958a1bf3958824701-15">15</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bf3958824701-16">16</div><div class="crayon-num" data-line="crayon-5d5de958a1bf3958824701-17">17</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bf3958824701-18">18</div><div class="crayon-num" data-line="crayon-5d5de958a1bf3958824701-19">19</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bf3958824701-20">20</div><div class="crayon-num" data-line="crayon-5d5de958a1bf3958824701-21">21</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bf3958824701-22">22</div><div class="crayon-num" data-line="crayon-5d5de958a1bf3958824701-23">23</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bf3958824701-24">24</div><div class="crayon-num" data-line="crayon-5d5de958a1bf3958824701-25">25</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bf3958824701-26">26</div><div class="crayon-num" data-line="crayon-5d5de958a1bf3958824701-27">27</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bf3958824701-28">28</div><div class="crayon-num" data-line="crayon-5d5de958a1bf3958824701-29">29</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bf3958824701-30">30</div><div class="crayon-num" data-line="crayon-5d5de958a1bf3958824701-31">31</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bf3958824701-32">32</div><div class="crayon-num" data-line="crayon-5d5de958a1bf3958824701-33">33</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bf3958824701-34">34</div><div class="crayon-num" data-line="crayon-5d5de958a1bf3958824701-35">35</div></div>
</td>
<td class="crayon-code"><div class="crayon-pre" style="font-size: 15px !important; line-height: 22px !important; -moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4;"><div class="crayon-line" id="crayon-5d5de958a1bf3958824701-1"><span class="crayon-r">import</span><span class="crayon-h"> </span><span class="crayon-i">java</span><span class="crayon-sy">.</span><span class="crayon-i">util</span><span class="crayon-sy">.</span><span class="crayon-i">regex</span><span class="crayon-sy">.</span><span class="crayon-i">Matcher</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bf3958824701-2"><span class="crayon-r">import</span><span class="crayon-h"> </span><span class="crayon-i">java</span><span class="crayon-sy">.</span><span class="crayon-i">util</span><span class="crayon-sy">.</span><span class="crayon-i">regex</span><span class="crayon-sy">.</span><span class="crayon-i">Pattern</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bf3958824701-3"><span class="crayon-h"> </span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bf3958824701-4"><span class="crayon-m">public</span><span class="crayon-h"> </span><span class="crayon-t">class</span><span class="crayon-h"> </span><span class="crayon-e">Main</span><span class="crayon-h"> </span><span class="crayon-sy">{</span></div><div class="crayon-line" id="crayon-5d5de958a1bf3958824701-5"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-m">public</span><span class="crayon-h"> </span><span class="crayon-m">static</span><span class="crayon-h"> </span><span class="crayon-t">void</span><span class="crayon-h"> </span><span class="crayon-e">main</span><span class="crayon-sy">(</span><span class="crayon-t">String</span><span class="crayon-sy">[</span><span class="crayon-sy">]</span><span class="crayon-h"> </span><span class="crayon-i">args</span><span class="crayon-sy">)</span><span class="crayon-h"> </span><span class="crayon-sy">{</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bf3958824701-6"><span class="crayon-h"> </span></div><div class="crayon-line" id="crayon-5d5de958a1bf3958824701-7"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-c">// 検索する文字列を用意</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bf3958824701-8"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-t">String</span><span class="crayon-h"> </span><span class="crayon-i">str1</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-s">"1000:2000:3000:4000:5000:6000:7000:abcd"</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bf3958824701-9"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-t">String</span><span class="crayon-h"> </span><span class="crayon-i">str2</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-s">"1:2:3:4:5:6:7:a"</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bf3958824701-10"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-t">String</span><span class="crayon-h"> </span><span class="crayon-i">str3</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-s">"1:2:3::6:7:a"</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bf3958824701-11"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-t">String</span><span class="crayon-h"> </span><span class="crayon-i">str4</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-s">"1:2::a"</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bf3958824701-12"><span class="crayon-h"> </span></div><div class="crayon-line" id="crayon-5d5de958a1bf3958824701-13"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-c">// 正規表現のパターンを作成</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bf3958824701-14"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">Pattern</span><span class="crayon-h"> </span><span class="crayon-i">p</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-i">Pattern</span><span class="crayon-sy">.</span><span class="crayon-e">compile</span><span class="crayon-sy">(</span><span class="crayon-s">"^\\s*((([0-9A-Fa-f]{1,4}:){7}([0-9A-Fa-f]{1,4}|:))|"</span></div><div class="crayon-line" id="crayon-5d5de958a1bf3958824701-15"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>+<span class="crayon-h"> </span><span class="crayon-s">"(([0-9A-Fa-f]{1,4}:){6}(:[0-9A-Fa-f]{1,4}|((25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)(\\.(25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)){3})|:))|"</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bf3958824701-16"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>+<span class="crayon-h"> </span><span class="crayon-s">"(([0-9A-Fa-f]{1,4}:){5}(((:[0-9A-Fa-f]{1,4}){1,2})|:((25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)(\\.(25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)){3})|:))|"</span></div><div class="crayon-line" id="crayon-5d5de958a1bf3958824701-17"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>+<span class="crayon-h"> </span><span class="crayon-s">"(([0-9A-Fa-f]{1,4}:){4}(((:[0-9A-Fa-f]{1,4}){1,3})|((:[0-9A-Fa-f]{1,4})?:((25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)(\\.(25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)){3}))|:))|"</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bf3958824701-18"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>+<span class="crayon-h"> </span><span class="crayon-s">"(([0-9A-Fa-f]{1,4}:){3}(((:[0-9A-Fa-f]{1,4}){1,4})|((:[0-9A-Fa-f]{1,4}){0,2}:((25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)(\\.(25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)){3}))|:))|"</span></div><div class="crayon-line" id="crayon-5d5de958a1bf3958824701-19"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>+<span class="crayon-h"> </span><span class="crayon-s">"(([0-9A-Fa-f]{1,4}:){2}(((:[0-9A-Fa-f]{1,4}){1,5})|((:[0-9A-Fa-f]{1,4}){0,3}:((25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)(\\.(25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)){3}))|:))|"</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bf3958824701-20"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>+<span class="crayon-h"> </span><span class="crayon-s">"(([0-9A-Fa-f]{1,4}:){1}(((:[0-9A-Fa-f]{1,4}){1,6})|((:[0-9A-Fa-f]{1,4}){0,4}:((25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)(\\.(25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)){3}))|:))|"</span></div><div class="crayon-line" id="crayon-5d5de958a1bf3958824701-21"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>+<span class="crayon-h"> </span><span class="crayon-s">"(:(((:[0-9A-Fa-f]{1,4}){1,7})|((:[0-9A-Fa-f]{1,4}){0,5}:((25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)(\\.(25[0-5]|2[0-4]\\d|1\\d\\d|[1-9]?\\d)){3}))|:)))(%.+)?\\s*$"</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bf3958824701-22"><span class="crayon-h"> </span></div><div class="crayon-line" id="crayon-5d5de958a1bf3958824701-23"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">Matcher</span><span class="crayon-h"> </span><span class="crayon-i">m</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-i">p</span><span class="crayon-sy">.</span><span class="crayon-e">matcher</span><span class="crayon-sy">(</span><span class="crayon-i">str1</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bf3958824701-24"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">System</span><span class="crayon-sy">.</span><span class="crayon-i">out</span><span class="crayon-sy">.</span><span class="crayon-e">println</span><span class="crayon-sy">(</span><span class="crayon-i">m</span><span class="crayon-sy">.</span><span class="crayon-e">find</span><span class="crayon-sy">(</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bf3958824701-25"><span class="crayon-h"> </span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bf3958824701-26"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">m</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-i">p</span><span class="crayon-sy">.</span><span class="crayon-e">matcher</span><span class="crayon-sy">(</span><span class="crayon-i">str2</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bf3958824701-27"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">System</span><span class="crayon-sy">.</span><span class="crayon-i">out</span><span class="crayon-sy">.</span><span class="crayon-e">println</span><span class="crayon-sy">(</span><span class="crayon-i">m</span><span class="crayon-sy">.</span><span class="crayon-e">find</span><span class="crayon-sy">(</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bf3958824701-28"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span></div><div class="crayon-line" id="crayon-5d5de958a1bf3958824701-29"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">m</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-i">p</span><span class="crayon-sy">.</span><span class="crayon-e">matcher</span><span class="crayon-sy">(</span><span class="crayon-i">str3</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bf3958824701-30"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">System</span><span class="crayon-sy">.</span><span class="crayon-i">out</span><span class="crayon-sy">.</span><span class="crayon-e">println</span><span class="crayon-sy">(</span><span class="crayon-i">m</span><span class="crayon-sy">.</span><span class="crayon-e">find</span><span class="crayon-sy">(</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bf3958824701-31"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bf3958824701-32"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">m</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-i">p</span><span class="crayon-sy">.</span><span class="crayon-e">matcher</span><span class="crayon-sy">(</span><span class="crayon-i">str4</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bf3958824701-33"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">System</span><span class="crayon-sy">.</span><span class="crayon-i">out</span><span class="crayon-sy">.</span><span class="crayon-e">println</span><span class="crayon-sy">(</span><span class="crayon-i">m</span><span class="crayon-sy">.</span><span class="crayon-e">find</span><span class="crayon-sy">(</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bf3958824701-34"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-sy">}</span></div><div class="crayon-line" id="crayon-5d5de958a1bf3958824701-35"><span class="crayon-sy">}</span></div></div></td>
</tr>
</table>
</div>
</div>

[実行結果]

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bf6862146072" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">
<div class="crayon-plain-wrap"><textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 15px !important; line-height: 22px !important;">
true
true
true
true</textarea></div>
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
<div class="crayon-plain-wrap"><textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 15px !important; line-height: 22px !important;">
^(([0-9a-zA-Z!#\\$%&amp;'\\*\\+\\-/=\\?\\^_`\\{\\}\\|~]+(\\.[0-9a-zA-Z!#\\$%&amp;'\\*\\+\\-/=\\?\\^_`\\{\\}\\|~]+)*)|(\"[^\"]*\"))@[0-9a-zA-Z!#\\$%&amp;'\\*\\+\\-/=\\?\\^_`\\{\\}\\|~]+(\\.[0-9a-zA-Z!#\\$%&amp;'\\*\\+\\-/=\\?\\^_`\\{\\}\\|~]+)*$</textarea></div>
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
[メールアドレスのチェックをするサンプルコード]

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bf9973872259" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">
<div class="crayon-plain-wrap"><textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 15px !important; line-height: 22px !important;">
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
}</textarea></div>
<div class="crayon-main" style="">
<table class="crayon-table">
<tr class="crayon-row">
<td class="crayon-nums " data-settings="show">
<div class="crayon-nums-content" style="font-size: 15px !important; line-height: 22px !important;"><div class="crayon-num" data-line="crayon-5d5de958a1bf9973872259-1">1</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bf9973872259-2">2</div><div class="crayon-num" data-line="crayon-5d5de958a1bf9973872259-3">3</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bf9973872259-4">4</div><div class="crayon-num" data-line="crayon-5d5de958a1bf9973872259-5">5</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bf9973872259-6">6</div><div class="crayon-num" data-line="crayon-5d5de958a1bf9973872259-7">7</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bf9973872259-8">8</div><div class="crayon-num" data-line="crayon-5d5de958a1bf9973872259-9">9</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bf9973872259-10">10</div><div class="crayon-num" data-line="crayon-5d5de958a1bf9973872259-11">11</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bf9973872259-12">12</div><div class="crayon-num" data-line="crayon-5d5de958a1bf9973872259-13">13</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bf9973872259-14">14</div><div class="crayon-num" data-line="crayon-5d5de958a1bf9973872259-15">15</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bf9973872259-16">16</div><div class="crayon-num" data-line="crayon-5d5de958a1bf9973872259-17">17</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bf9973872259-18">18</div><div class="crayon-num" data-line="crayon-5d5de958a1bf9973872259-19">19</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bf9973872259-20">20</div><div class="crayon-num" data-line="crayon-5d5de958a1bf9973872259-21">21</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bf9973872259-22">22</div><div class="crayon-num" data-line="crayon-5d5de958a1bf9973872259-23">23</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bf9973872259-24">24</div><div class="crayon-num" data-line="crayon-5d5de958a1bf9973872259-25">25</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bf9973872259-26">26</div><div class="crayon-num" data-line="crayon-5d5de958a1bf9973872259-27">27</div></div>
</td>
<td class="crayon-code"><div class="crayon-pre" style="font-size: 15px !important; line-height: 22px !important; -moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4;"><div class="crayon-line" id="crayon-5d5de958a1bf9973872259-1"><span class="crayon-r">import</span><span class="crayon-h"> </span><span class="crayon-i">java</span><span class="crayon-sy">.</span><span class="crayon-i">util</span><span class="crayon-sy">.</span><span class="crayon-i">regex</span><span class="crayon-sy">.</span><span class="crayon-i">Matcher</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bf9973872259-2"><span class="crayon-r">import</span><span class="crayon-h"> </span><span class="crayon-i">java</span><span class="crayon-sy">.</span><span class="crayon-i">util</span><span class="crayon-sy">.</span><span class="crayon-i">regex</span><span class="crayon-sy">.</span><span class="crayon-i">Pattern</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bf9973872259-3"><span class="crayon-h"> </span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bf9973872259-4"><span class="crayon-m">public</span><span class="crayon-h"> </span><span class="crayon-t">class</span><span class="crayon-h"> </span><span class="crayon-e">Main</span><span class="crayon-h"> </span><span class="crayon-sy">{</span></div><div class="crayon-line" id="crayon-5d5de958a1bf9973872259-5"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-m">public</span><span class="crayon-h"> </span><span class="crayon-m">static</span><span class="crayon-h"> </span><span class="crayon-t">void</span><span class="crayon-h"> </span><span class="crayon-e">main</span><span class="crayon-sy">(</span><span class="crayon-t">String</span><span class="crayon-sy">[</span><span class="crayon-sy">]</span><span class="crayon-h"> </span><span class="crayon-i">args</span><span class="crayon-sy">)</span><span class="crayon-h"> </span><span class="crayon-sy">{</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bf9973872259-6"><span class="crayon-h"> </span></div><div class="crayon-line" id="crayon-5d5de958a1bf9973872259-7"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-c">// 検索する文字列を用意</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bf9973872259-8"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-t">String</span><span class="crayon-h"> </span><span class="crayon-i">str1</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-s">"<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="adfeccc0d8dfccc486e8c3cac4c3c8c8df839c9f9eedcac0ccc4c183cec2c0">[email&#160;protected]</a>"</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bf9973872259-9"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-t">String</span><span class="crayon-h"> </span><span class="crayon-i">str2</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-s">"<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="eab98b879f988b83c4c4af848d83848f8f98aa8d878b8386c4898587">[email&#160;protected]</a>"</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bf9973872259-10"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-t">String</span><span class="crayon-h"> </span><span class="crayon-i">str3</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-s">"\"Samurai..Engineer\"@gmail.com"</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bf9973872259-11"><span class="crayon-h"> </span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bf9973872259-12"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-c">// 正規表現のパターンを作成</span></div><div class="crayon-line" id="crayon-5d5de958a1bf9973872259-13"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">Pattern</span><span class="crayon-h"> </span><span class="crayon-i">p</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-i">Pattern</span><span class="crayon-sy">.</span><span class="crayon-e">compile</span><span class="crayon-sy">(</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bf9973872259-14"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-s">"^(([0-9a-zA-Z!#\\$%&amp;'\\*\\+\\-/=\\?\\^_`\\{\\}\\|~]+(\\.[0-9a-zA-Z!#\\$%&amp;'\\*\\+\\-/=\\?\\^_`\\{\\}\\|~]+)*)|(\"[^\"]*\"))"</span></div><div class="crayon-line" id="crayon-5d5de958a1bf9973872259-15"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>+<span class="crayon-h"> </span><span class="crayon-s">"@[0-9a-zA-Z!#\\$%&amp;'\\*\\+\\-/=\\?\\^_`\\{\\}\\|~]+"</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bf9973872259-16"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>+<span class="crayon-h"> </span><span class="crayon-s">"(\\.[0-9a-zA-Z!#\\$%&amp;'\\*\\+\\-/=\\?\\^_`\\{\\}\\|~]+)*$"</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bf9973872259-17"><span class="crayon-h"> </span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bf9973872259-18"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">Matcher</span><span class="crayon-h"> </span><span class="crayon-i">m</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-i">p</span><span class="crayon-sy">.</span><span class="crayon-e">matcher</span><span class="crayon-sy">(</span><span class="crayon-i">str1</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bf9973872259-19"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">System</span><span class="crayon-sy">.</span><span class="crayon-i">out</span><span class="crayon-sy">.</span><span class="crayon-e">println</span><span class="crayon-sy">(</span><span class="crayon-i">str1</span><span class="crayon-h"> </span>+<span class="crayon-h"> </span><span class="crayon-s">" = "</span><span class="crayon-h"> </span>+<span class="crayon-h"> </span><span class="crayon-i">m</span><span class="crayon-sy">.</span><span class="crayon-e">find</span><span class="crayon-sy">(</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bf9973872259-20">&nbsp;</div><div class="crayon-line" id="crayon-5d5de958a1bf9973872259-21"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">m</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-i">p</span><span class="crayon-sy">.</span><span class="crayon-e">matcher</span><span class="crayon-sy">(</span><span class="crayon-i">str2</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bf9973872259-22"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">System</span><span class="crayon-sy">.</span><span class="crayon-i">out</span><span class="crayon-sy">.</span><span class="crayon-e">println</span><span class="crayon-sy">(</span><span class="crayon-i">str2</span><span class="crayon-h"> </span>+<span class="crayon-h"> </span><span class="crayon-s">" = "</span><span class="crayon-h"> </span>+<span class="crayon-h"> </span><span class="crayon-i">m</span><span class="crayon-sy">.</span><span class="crayon-e">find</span><span class="crayon-sy">(</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bf9973872259-23">&nbsp;</div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bf9973872259-24"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">m</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-i">p</span><span class="crayon-sy">.</span><span class="crayon-e">matcher</span><span class="crayon-sy">(</span><span class="crayon-i">str3</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bf9973872259-25"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">System</span><span class="crayon-sy">.</span><span class="crayon-i">out</span><span class="crayon-sy">.</span><span class="crayon-e">println</span><span class="crayon-sy">(</span><span class="crayon-i">str3</span><span class="crayon-h"> </span>+<span class="crayon-h"> </span><span class="crayon-s">" = "</span><span class="crayon-h"> </span>+<span class="crayon-h"> </span><span class="crayon-i">m</span><span class="crayon-sy">.</span><span class="crayon-e">find</span><span class="crayon-sy">(</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bf9973872259-26"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-sy">}</span></div><div class="crayon-line" id="crayon-5d5de958a1bf9973872259-27"><span class="crayon-sy">}</span></div></div></td>
</tr>
</table>
</div>
</div>

[実行結果]

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bfb107983660" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">
<div class="crayon-plain-wrap"><textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 15px !important; line-height: 22px !important;">
Samurai+Engineer.123@gmail.com = true
Samurai..Engineer@gmail.com = false
"Samurai..Engineer"@gmail.com = true</textarea></div>
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
<div class="crayon-plain-wrap"><textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 15px !important; line-height: 22px !important;">
^https?://[a-z\\.:/\\+\\-\\#\\?\\=\\&amp;\\;\\%\\~]+$</textarea></div>
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
[URLのチェックをするサンプルコード]

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bfd002406925" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">
<div class="crayon-plain-wrap"><textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 15px !important; line-height: 22px !important;">
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
}</textarea></div>
<div class="crayon-main" style="">
<table class="crayon-table">
<tr class="crayon-row">
<td class="crayon-nums " data-settings="show">
<div class="crayon-nums-content" style="font-size: 15px !important; line-height: 22px !important;"><div class="crayon-num" data-line="crayon-5d5de958a1bfd002406925-1">1</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bfd002406925-2">2</div><div class="crayon-num" data-line="crayon-5d5de958a1bfd002406925-3">3</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bfd002406925-4">4</div><div class="crayon-num" data-line="crayon-5d5de958a1bfd002406925-5">5</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bfd002406925-6">6</div><div class="crayon-num" data-line="crayon-5d5de958a1bfd002406925-7">7</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bfd002406925-8">8</div><div class="crayon-num" data-line="crayon-5d5de958a1bfd002406925-9">9</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bfd002406925-10">10</div><div class="crayon-num" data-line="crayon-5d5de958a1bfd002406925-11">11</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bfd002406925-12">12</div><div class="crayon-num" data-line="crayon-5d5de958a1bfd002406925-13">13</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bfd002406925-14">14</div><div class="crayon-num" data-line="crayon-5d5de958a1bfd002406925-15">15</div><div class="crayon-num crayon-striped-num" data-line="crayon-5d5de958a1bfd002406925-16">16</div></div>
</td>
<td class="crayon-code"><div class="crayon-pre" style="font-size: 15px !important; line-height: 22px !important; -moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4;"><div class="crayon-line" id="crayon-5d5de958a1bfd002406925-1"><span class="crayon-r">import</span><span class="crayon-h"> </span><span class="crayon-i">java</span><span class="crayon-sy">.</span><span class="crayon-i">util</span><span class="crayon-sy">.</span><span class="crayon-i">regex</span><span class="crayon-sy">.</span><span class="crayon-i">Matcher</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bfd002406925-2"><span class="crayon-r">import</span><span class="crayon-h"> </span><span class="crayon-i">java</span><span class="crayon-sy">.</span><span class="crayon-i">util</span><span class="crayon-sy">.</span><span class="crayon-i">regex</span><span class="crayon-sy">.</span><span class="crayon-i">Pattern</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bfd002406925-3"><span class="crayon-h"> </span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bfd002406925-4"><span class="crayon-m">public</span><span class="crayon-h"> </span><span class="crayon-t">class</span><span class="crayon-h"> </span><span class="crayon-e">Main</span><span class="crayon-h"> </span><span class="crayon-sy">{</span></div><div class="crayon-line" id="crayon-5d5de958a1bfd002406925-5"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-m">public</span><span class="crayon-h"> </span><span class="crayon-m">static</span><span class="crayon-h"> </span><span class="crayon-t">void</span><span class="crayon-h"> </span><span class="crayon-e">main</span><span class="crayon-sy">(</span><span class="crayon-t">String</span><span class="crayon-sy">[</span><span class="crayon-sy">]</span><span class="crayon-h"> </span><span class="crayon-i">args</span><span class="crayon-sy">)</span><span class="crayon-h"> </span><span class="crayon-sy">{</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bfd002406925-6"><span class="crayon-h"> </span></div><div class="crayon-line" id="crayon-5d5de958a1bfd002406925-7"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-c">// 検索する文字列を用意</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bfd002406925-8"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-t">String</span><span class="crayon-h"> </span><span class="crayon-i">str</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-s">"http://www.sejuku.net/blog"</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bfd002406925-9"><span class="crayon-h"> </span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bfd002406925-10"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-c">// 正規表現のパターンを作成</span></div><div class="crayon-line" id="crayon-5d5de958a1bfd002406925-11"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">Pattern</span><span class="crayon-h"> </span><span class="crayon-i">p</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-i">Pattern</span><span class="crayon-sy">.</span><span class="crayon-e">compile</span><span class="crayon-sy">(</span><span class="crayon-s">"^https?://[a-z\\.:/\\+\\-\\#\\?\\=\\&amp;\\;\\%\\~]+$"</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bfd002406925-12"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">Matcher</span><span class="crayon-h"> </span><span class="crayon-i">m</span><span class="crayon-h"> </span>=<span class="crayon-h"> </span><span class="crayon-i">p</span><span class="crayon-sy">.</span><span class="crayon-e">matcher</span><span class="crayon-sy">(</span><span class="crayon-i">str</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bfd002406925-13"><span class="crayon-h"> </span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bfd002406925-14"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-i">System</span><span class="crayon-sy">.</span><span class="crayon-i">out</span><span class="crayon-sy">.</span><span class="crayon-e">println</span><span class="crayon-sy">(</span><span class="crayon-i">m</span><span class="crayon-sy">.</span><span class="crayon-e">find</span><span class="crayon-sy">(</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">;</span></div><div class="crayon-line" id="crayon-5d5de958a1bfd002406925-15"><span class="crayon-h">&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="crayon-sy">}</span></div><div class="crayon-line crayon-striped-line" id="crayon-5d5de958a1bfd002406925-16"><span class="crayon-sy">}</span></div></div></td>
</tr>
</table>
</div>
</div>

[実行結果]

<link rel="stylesheet" type="text/css" href="https://www.sejuku.net/blog/wp-content/plugins/crayon-syntax-highlighter/themes/tomorrow-night/tomorrow-night.css" />
<div id="crayon-5d5de958a1bff394109116" class="crayon-syntax crayon-theme-tomorrow-night crayon-font-liberation-mono crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover" style=" margin-bottom: 6px; font-size: 15px !important; line-height: 22px !important;">
<div class="crayon-plain-wrap"><textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly style="-moz-tab-size:4; -o-tab-size:4; -webkit-tab-size:4; tab-size:4; font-size: 15px !important; line-height: 22px !important;">
true</textarea></div>
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
