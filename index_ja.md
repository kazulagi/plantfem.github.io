このページでは、plantFEMの使用例を紹介します。
どちらでも好きなところから始められます。

## Pythonユーザーの方へ向けたチートシート
### [こちら](Tutorial_from_python.md)

## [推奨] ツアー

### plantFEMができること、plantFEMの使い方を短いツアーで体験しましょう。
## [入り口はこちらから](https://colab.research.google.com/drive/1VPzKZlAsqZHO-Yzda2cCXM4Te3Ysh9h0?usp=sharing)


　
　
　
これらをお手元のデバイスで実行してみませんか？
以下にその方法をご案内します。
　
　
#### ダウンロード・インストール方法 (10分)

plantFEMは、Ubuntu 16.04以降のバージョンでご利用いただけます。

Windowsを使用する場合、Windows Subsystem for Linux (WSL2) を使い、Ubuntu 20.04環境を構築してください。

それ以外の場合、Google Colabからも実行できます。

以上の環境構築が完了したら、以下のコマンドをターミナルから実行してパッケージのダウンロードとインストールを完了してください。


```
sudo apt install git && git clone https://github.com/kazulagi/plantfem.git && cd plantfem && python3 install.py
```

また、Google Colabを使う場合、コードブロックに以下の内容をペーストし、実行してください。


```
!git clone https://github.com/kazulagi/plantfem.git 
%cd ./plantfem
!python3 install.py
```

### plantFEMのディレクトリ構成・コマンド一覧　(5分)

plantfemという名前のディレクトリができますので、ターミナルでplantfemのディレクトリからコマンドを実行してplantfemを操作してください。


plantfemのディレクトリ構成は以下のようになります。

```

plantfem
├─server.f90
│
├─Tutorial/
│  ├─app/
│  │  
│  ├─std/
│  │  
│  ├─fem/
│  │  
│  ├─sim/
│  │  
│  ├─obj/
│  │
│  └─python/
│
...

```

plantfemでは ```server.f90```というスクリプトファイルにプログラムを書き、
```./plantem run```というコマンドでプログラムを実行します。


ターミナル上から実行できるplantfemコマンドの例です。

### サンプルプログラムのロード・実行

```./plantfem search``` >> プログラム例を検索します

```./plantfem new``` >> 新しいプログラムを作成

```./plantfem load [プログラム名]``` >> 新しいプログラムを作成


```./plantfem run``` >> ```server.f90```に書かれたプログラムを実行


```./plantfem cpu-core``` >> (MPI並列計算実行時)cpuコア数を指定


```./plantfem cpu-core``` >> (MPI並列計算実行時)hostfileを指定


サンプルプログラムは```Tutorial```ディレクトリ内に```.f90```の拡張子がついたものがあります。


plantFEMは、以下の4つのライブラリから構成され、```Tutorial```でもそれぞれのライブラリに対応したサンプルプログラムが提供されています。

- ```std```基本的な計算・文字列操作・ファイル操作など

- ```fem```有限要素法解析に共通して必要なライブラリ。メッシュ生成やメッシュ加工・可視化など

- ```sim```構造計算・流体計算・反応計算等のソルバーが含まれるライブラリ

- ```obj```植物、土、水、空気等のオブジェクトを生成・加工・可視化するためのライブラリ







