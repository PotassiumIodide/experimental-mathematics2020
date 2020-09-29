# 2020年度 実験数学C サンプルプログラム
2020年度実験数学C(第3ターム)及びD(第4ターム)のサンプルプログラム

**皆本晃弥『C言語による数値計算入門 解法・アルゴリズム・プログラム (UNIX & Information Science)』サイエンス社, 2005**
のサンプルプログラムをC言語およびPythonで実装しています。

## C言語のコンパイル方法及び実行方法について
`program.c`ファイルのコンパイルはgccコマンドを使って以下のように行います。
```bash
$ gcc -o program.exe program.c
```
すると`program.exe`という名前の実行ファイルができます(名前は任意に指定できます。拡張子も必ずしもつける必要はありません)。<br>
ただし、mathライブラリなどを利用している場合(ファイルの最初に`#include<math.h>`とある場合)は`-lm`オプションが必要となるので注意です。<br>
すなわち、以下のようにしてコンパイルします。
```bash
$ gcc -o program.exe program.c -lm
```
このようにしてできた実行ファイルは以下のようにして実行することができます:
```bash
$ ./program.exe
```

### テキストの誤植(？)

#### プログラム4.3

プログラム4.3の実行結果が、テキストでは
```
初期値x0を入力してください
1.1
答えは x=1.000000 で, 重複次数は 2.000085 です
```
となっていますが、C言語、Python共に実行結果は
```
初期値 x0 を入力してください
1.1
答えは x=1.000000 で, 重複次数は3.841109です
```
となります。重複次数の値から、恐らくテキストのC言語プログラムに誤植があると推測されます。

#### プログラム9.3

プログラム9.3の実行結果が、テキストでは
```
QR法の結果は
-24.1485206      0.8743997       1.1467835       1.9115289
-0.0000000      21.5793450       0.0034205      -2.6903577
 0.0000000      -0.0000000      14.3382192      -2.4552665
 0.0000000       0.0000000       0.0000000      12.2309564
固有値は
-24.1485206     21.5793450      14.3382192      12.2309564
```
となっていますが、C言語、Python共に実行結果は
```
QR法の結果は
-24.1485206     -0.8743997      -1.1467835       1.8344712
-0.0000000      21.5793450       0.0034205       2.7408570
 0.0000000      -0.0000000      14.3382192       2.4581943
 0.0000000       0.0000000      -0.0000000      12.2309564
固有値は
-24.1485206     21.5793450      14.3382192      12.2309564
```
となります(正負符号に加え4列目の値も少しだけ異なっています)。<br>
【追記】ちなみに、このQR法のプログラムを利用するプログラム9.4では、このままの実装でC言語、
Python共にテキストと同じ結果を出力しています。このことから、テキストの出力結果に誤植があると推測されます。


C言語のプログラムは一応見返しましたが見落としがある可能性もあるので、確認の程、よろしくお願いします。

## 実装の仕様について

最後にバグを修正するために役に立つかもしれない点や、実装の意図について書き留めておきます。

### inputファイルの違い(※重要)

Pythonによる実装に際して、Pythonの`input`関数(や、`file.readline`メソッド)とC言語の`scanf`関数の違いにより、一部`input.dat`ファイルの様式を変更させていただいています。
> これによるC言語プログラムへの影響はありませんが、元の様式でPythonのプログラムを実行すると、エラーが生じます。これはC言語が数値1つ1つに対する入力を持つのに対し、Pythonが
> 行ごとに入力を受け取るためです。

- `ch02/input.dat` -> 変更なし
- `ch03/program3_1/input.dat` -> 変更なし
- `ch03/program3_2/input.dat` -> **変更あり**

【テキスト】
```
2.00 4.00 1.00 -3.00
-1.00 -2.00 2.00 4.00
4.00 2.00 -3.00 5.00
5.00 -4.00 -3.00 1.00

 0.00
10.00
 2.00
 6.00
```
【本ファイル】
```
2.00 4.00 1.00 -3.00
-1.00 -2.00 2.00 4.00
4.00 2.00 -3.00 5.00
5.00 -4.00 -3.00 1.00

0.00 10.00 2.00 6.00
```
- `ch03/program3_3/input_lu.dat` -> 変更なし
- `ch03/program3_4/input_cho.dat` -> 変更なし
- `ch05/output_sp.dat` -> 変更なし
- `ch06/input_func.dat` -> 変更なし
- `ch06/input_lag.dat` -> 変更なし
- `ch09/input_eigen.dat` -> **変更あり**

【テキスト】
```
16.00	-1.00	 1.00	 2.00
 2.00	12.00	 1.00	-1.00
 1.00	 3.00	-24.00   2.00
 4.00	-2.00	 1.00	20.00

0.50
0.50
0.50
0.50
```
【本ファイル】
```
16.00	-1.00	 1.00	 2.00
 2.00	12.00	 1.00	-1.00
 1.00	 3.00	-24.00   2.00
 4.00	-2.00	 1.00	20.00

0.50 0.50 0.50 0.50
```

### ファイル構造(※重要)

動作確認は、本リポジトリの`src`ディレクトリ下のファイル構造で行っています。具体的には、以下のようなファイル構造です:
```
.
├── __init__.py
├── ch01
│   ├── program1_1.c
│   ├── program1_1.py
│   ├── program1_2.c
│   ├── program1_2.py
│   ├── program1_3.c
│   ├── program1_3.py
│   ├── program1_4.c
│   ├── program1_4.py
│   ├── program1_5.c
│   ├── program1_5.py
│   ├── program1_6.c
│   ├── program1_6.py
│   ├── program1_7.c
│   └── program1_7.py
├── ch02
│   ├── __init__.py
│   ├── __pycache__
│   │   ├── __init__.cpython-37.pyc
│   │   ├── __init__.cpython-38.pyc
│   │   ├── program2_1.cpython-37.pyc
│   │   ├── program2_1.cpython-38.pyc
│   │   ├── program2_11.cpython-38.pyc
│   │   ├── program2_2.cpython-37.pyc
│   │   ├── program2_2.cpython-38.pyc
│   │   ├── program2_3.cpython-37.pyc
│   │   ├── program2_3.cpython-38.pyc
│   │   ├── program2_4.cpython-37.pyc
│   │   ├── program2_4.cpython-38.pyc
│   │   ├── program2_8.cpython-37.pyc
│   │   └── program2_8.cpython-38.pyc
│   ├── input.dat
│   ├── output.dat
│   ├── program2_1.c
│   ├── program2_1.py
│   ├── program2_10.c
│   ├── program2_10.py
│   ├── program2_11.c
│   ├── program2_11.py
│   ├── program2_12.c
│   ├── program2_12.py
│   ├── program2_2.c
│   ├── program2_2.py
│   ├── program2_3.c
│   ├── program2_3.py
│   ├── program2_4.c
│   ├── program2_4.py
│   ├── program2_5.c
│   ├── program2_5.py
│   ├── program2_6.c
│   ├── program2_6.py
│   ├── program2_7.c
│   ├── program2_7.py
│   ├── program2_8.c
│   ├── program2_8.py
│   ├── program2_9.c
│   └── program2_9.py
├── ch03
│   ├── program3_1
│   │   ├── input.dat
│   │   ├── output.dat
│   │   ├── program3_1.c
│   │   └── program3_1.py
│   ├── program3_2
│   │   ├── __pycache__
│   │   │   └── program3_2.cpython-37.pyc
│   │   ├── input.dat
│   │   ├── output.dat
│   │   ├── program3_2.c
│   │   └── program3_2.py
│   ├── program3_3
│   │   ├── __pycache__
│   │   │   └── program3_3.cpython-38.pyc
│   │   ├── input_lu.dat
│   │   ├── output_lu.dat
│   │   ├── program3_3.c
│   │   └── program3_3.py
│   └── program3_4
│       ├── __pycache__
│       │   └── program3_4.cpython-38.pyc
│       ├── input_cho.dat
│       ├── output_cho.dat
│       ├── program3_4.c
│       └── program3_4.py
├── ch04
│   ├── __pycache__
│   │   └── program4_5.cpython-37.pyc
│   ├── program4_1.c
│   ├── program4_1.py
│   ├── program4_2.c
│   ├── program4_2.py
│   ├── program4_3.c
│   ├── program4_3.py
│   ├── program4_4.c
│   ├── program4_4.py
│   ├── program4_5.c
│   └── program4_5.py
├── ch05
│   ├── __pycache__
│   │   └── program5_4.cpython-37.pyc
│   ├── input_sp.dat
│   ├── output_sp.dat
│   ├── program5_1.c
│   ├── program5_1.py
│   ├── program5_2.c
│   ├── program5_2.py
│   ├── program5_3.c
│   ├── program5_3.py
│   ├── program5_4.c
│   └── program5_4.py
├── ch06
│   ├── input_func.dat
│   ├── input_lag.dat
│   ├── output_func.dat
│   ├── output_lag.dat
│   ├── program6_1.c
│   ├── program6_1.py
│   ├── program6_2.c
│   ├── program6_2.py
│   ├── program6_3.c
│   └── program6_3.py
├── ch07
│   ├── __pycache__
│   │   └── program7_1.cpython-37.pyc
│   ├── program7_1.c
│   ├── program7_1.py
│   ├── program7_2.c
│   ├── program7_2.py
│   ├── program7_3.c
│   ├── program7_3.py
│   ├── program7_4.c
│   └── program7_4.py
├── ch08
│   ├── __pycache__
│   │   ├── program8_1.cpython-37.pyc
│   │   └── program8_3.cpython-37.pyc
│   ├── program8_1.c
│   ├── program8_1.py
│   ├── program8_2.c
│   ├── program8_2.py
│   ├── program8_3.c
│   ├── program8_3.py
│   ├── program8_4.c
│   ├── program8_4.py
│   ├── program8_5.c
│   ├── program8_5.py
│   ├── program8_6.c
│   └── program8_6.py
├── ch09
│   ├── __pycache__
│   │   ├── program9_2.cpython-37.pyc
│   │   ├── program9_2.cpython-38.pyc
│   │   └── program9_3.cpython-38.pyc
│   ├── input_eigen.dat
│   ├── program9_1.c
│   ├── program9_1.py
│   ├── program9_2.c
│   ├── program9_2.py
│   ├── program9_3.c
│   ├── program9_3.py
│   ├── program9_4.c
│   ├── program9_4.py
│   └── result_eigen.dat
└── ch10
    ├── __pycache__
    │   ├── program10_1.cpython-37.pyc
    │   └── test.cpython-37.pyc
    ├── program10_1.c
    ├── program10_1.py
    ├── program10_2.c
    └── program10_2.py
```
このファイル構造でなくてもC言語プログラムの方は問題なく動作します(もちろん、入力ファイルが同じディレクトリにあるなどの条件はあります)が、PythonはC言語
プログラムと異なり、使い回されている関数・クラスはディレクトリのパスを参照して該当するプログラムからインポートしています。そのため、上のファイル構造を
壊してすべて同じディレクトリ下に置いた場合、インポートが上手く行かず、エラーを発生させる場合があります。<br>
ファイルの参照方法を理解できれば自由に配置して構いませんが、分からない場合はこのファイル構造で動作確認をお願いします。

### Dvector, Dmatrixクラス(program2_1.py, program2_2.pyで定義)を用いて実装した理由
  テキストではベクトルや行列の添字を自由に操作できるようにと、dvector, dmatrixが実装されています。簡単に説明すると、Pythonのリストのインデックスは
$$
\begin{array}{crccccccccl}
\mbox{インデックス:} &  & 0 & 1 & 2 & 3 & 4 & 5 & 6 &  \\
\mbox{リスト:}      & [ & 3 & 1 & 4 & 1 & 5 & 9 & 2 & ] 
$$
つまり、a = [3,1,4,1,5,9,2] とすると、a[0]で3が得られます。テキストのやりたいことは、インデックスの最初と最後の番号を指定して、
$$
\begin{array}{crccccccccl}
\mbox{インデックス:} &  & 3 & 4 & 5 & 6 & 7 & 8 & 9 &  \\
\mbox{リスト:}      & [ & 3 & 1 & 4 & 1 & 5 & 9 & 2 & ] 
$$
のように設定し、`a[3]`で`3`が得られるような仕様にしたいとの話です。行列も同様にインデックス番号を自由につけたいみたいです(最初のインデックスを`1`にしたいっていうのが
本質的な意図だとは思います)。
関数で管理する方法も考えましたが、`a(3)`のような呼び出しになるため、直感的にリストのような使い方ができないため、以降のプログラムで繰り返し使う`Dvector`や`Dmatrix`は
クラスとして定義し、(スライス等はできませんが)ほとんどリストと同じ感覚で書けるようにしました。もっと良い実装方法があれば提案してください。

### グローバル変数問題
もしかしたらバグがグローバル変数から生じている可能性があります。たとえば
```python
# test1.py
N = 5
def addN(a):
    return a + N
```
というプログラムの`addN`関数を`test2.py`で利用するとき
```python
# test2.py
from test1 import addN
N = 6
def main():
    print(addN(1))
```
と書いた場合、`1`に足されるのはtest2.pyの`N`(=`6`)ではなく、test1.pyの`N`(=`5`)です。各プログラムでグローバル変数を用いているため、意図しない値を用いた計算が
行われいている可能性もあります。この問題を解消する一つの方法は、test1.pyの`addN`関数を
```python
def addN(a, N=N):
    return a + N
```
として、第二引数を取り、デフォルト値としてグローバル変数のNを取る関数に書き換え、test2.pyで`print(addN(1,N))`のように利用することで、`N`をtest2.pyでの`N`とすることができます。
Pythonの方の実装に際しては、デフォルト引数を用いてこの問題を解決してあります。

### C言語とPythonの浮動小数点数の型の違い

これが原因でバグが生じるとはあまり思っていませんが、C言語での`float`型はPythonだとNumPyで提供されている`float32`型に対応し、C言語の`double`型はPythonでの`float`型ないし
NumPyの`float64`型に対応します。Pythonの`float`はC言語の`double`なので、テキストより精度が高い計算をしている可能性はなくもないです。しかし、計算に大きな影響を与える問題では
ないと思うので、これで問題が解消される見込みはあまりないと思います。

### 副作用を含む関数

テキストのプログラムでは多くの副作用を含む関数が定義されています。**副作用**(side effect)について簡単に説明すると、たとえば、ある変数`n`に`1`を加える関数を作るとき、
```python
def add1(n):
    return n + 1
```
のように定義すれば、
```python
n = 2
print(add1(n))
print(n)
```
としたとき、`print(n)`での出力結果は元の`n`の値`2`が出力されます。`n`自体の値は変更**しません**。しかし、テキストのプログラムでは
```python
def add1(n):
    n = n + 1
```
のように、`n`を上書きする形で所望の値を得る関数が度々登場します。この場合、
```python
n = 2
add1(n)
print(n)
```
とすると、`print(n)`での出力結果は`n`に`1`が加えられた`3`となります。このように、関数を呼び出す度に外部に何らかの(例えばinput関数での入力やprint関数での出力なども含める)
影響を与える関数は**副作用を持つ**と言います(不正確ではありますが、基本的に数学としての関数として書けないものは副作用を持つと考えてもらってよいです)。
Pythonプログラムの方では、簡単に副作用を解消できる関数に関しては副作用を含まない形に書き換えています。
