# S.Z 基本技術および知識まとめ

本書を読むための前提となる基本技術および知識をまとめます。

## Linuxの基礎コマンド

Linuxのコマンドはさまざまな部分で基本的な知識として必要になります。

### apt-get

Debian/Ubuntu系Linuxでパッケージ（ソフトウェア）をインストール・更新・削除するためのコマンド。

```bash
# パッケージリストを更新
sudo apt-get update

# パッケージをインストール
sudo apt-get install curl

# 複数パッケージを一度にインストール
sudo apt-get install -y vim git wget

# パッケージをアンインストール
sudo apt-get remove curl

# 不要なパッケージを削除
sudo apt-get autoremove
```

### echo 

文字列を標準出力に表示したり、ファイルに書き込むためのコマンド。

```bash
# 文字列を表示
echo "Hello World"

# 環境変数を表示
echo $PATH

# ファイルに書き込み（上書き）
echo "データ分析基盤" > output.txt

# ファイルに追記
echo "2行目のデータ" >> output.txt

# 改行なしで出力
echo -n "改行なし"
```

### mkdir

ディレクトリ（フォルダ）を作成するコマンド。

```bash
# ディレクトリを作成
mkdir data

# 複数のディレクトリを作成
mkdir logs temp backups

# 親ディレクトリも含めて作成（-pオプション）
mkdir -p /home/user/project/data/raw

# パーミッション指定で作成
mkdir -m 755 public_data
```

### sudo

管理者権限でコマンドを実行するためのコマンド。

```bash
# root権限でファイルを編集
sudo vim /etc/hosts

# root権限でパッケージをインストール
sudo apt-get install docker

# rootユーザーに切り替え
sudo su -

# 特定ユーザーとしてコマンド実行
sudo -u postgres psql
```

### chmod

ファイルやディレクトリのアクセス権限（パーミッション）を変更するコマンド。

```bash
# 実行権限を付与
chmod +x script.sh

# 所有者のみ読み書き実行可能に設定
chmod 700 private_file.txt

# 全員に読み取り権限を付与
chmod 644 public_file.txt

# ディレクトリとその中身すべてに権限を付与（再帰的）
chmod -R 755 /var/www/html

# 数値表記の意味:
# 4=読み取り(r), 2=書き込み(w), 1=実行(x)
# 7=4+2+1(rwx), 6=4+2(rw-), 5=4+1(r-x)
```

### useradd/groupadd

ユーザーやグループを作成・管理するコマンド。

```bash
# 新規ユーザーを作成
sudo useradd spark_user

# ホームディレクトリとシェルを指定してユーザー作成
sudo useradd -m -s /bin/bash dataeng

# ユーザーにパスワードを設定
sudo passwd dataeng

# 新規グループを作成
sudo groupadd data_team

# ユーザーをグループに追加
sudo usermod -aG data_team dataeng

# ユーザーの所属グループを確認
groups dataeng
```

### pip

Pythonパッケージを管理するコマンド。

```bash
# パッケージをインストール
pip install pandas

# 特定バージョンを指定してインストール
pip install numpy==1.24.0

# requirements.txtから一括インストール
pip install -r requirements.txt

# インストール済みパッケージ一覧
pip list

# パッケージをアンインストール
pip uninstall pandas

# パッケージのバージョンアップ
pip install --upgrade pyspark
```

### tar

ファイルをアーカイブ（圧縮・展開）するコマンド。

```bash
# ディレクトリを圧縮（gzip形式）
tar -czf backup.tar.gz /path/to/directory

# 圧縮ファイルを展開
tar -xzf backup.tar.gz

# 圧縮せずにアーカイブ
tar -cf archive.tar /path/to/directory

# アーカイブの中身を確認
tar -tzf backup.tar.gz

# 特定ディレクトリに展開
tar -xzf backup.tar.gz -C /opt/data

# オプション説明:
# -c: 作成(create)
# -x: 展開(extract)
# -z: gzip圧縮
# -f: ファイル名指定
# -v: 詳細表示(verbose)
```

### ln

リンク（ショートカット）を作成するコマンド。

```bash
# シンボリックリンク（ソフトリンク）を作成
ln -s /opt/spark/conf/spark-defaults.conf ~/spark-defaults.conf

# ハードリンクを作成
ln /path/to/original /path/to/link

# 既存リンクを上書き
ln -sf /new/target /path/to/link

# シンボリックリンクの確認
ls -la ~/spark-defaults.conf
# 出力例: lrwxr-xr-x 1 user user 35 Jan 1 12:00 spark-defaults.conf -> /opt/spark/conf/spark-defaults.conf
```

### rm

ファイルやディレクトリを削除するコマンド。

```bash
# ファイルを削除
rm test.txt

# 確認なしで削除
rm -f test.txt

# ディレクトリを削除（再帰的）
rm -r temp_directory

# 確認なしでディレクトリごと削除（危険！）
rm -rf /path/to/directory

# ワイルドカードで複数ファイルを削除
rm *.log

# 空のディレクトリのみ削除
rmdir empty_directory
```

### find

ファイルやディレクトリを検索するコマンド。

```bash
# カレントディレクトリ以下でファイル名検索
find . -name "*.log"

# 特定ディレクトリ配下を検索
find /var/log -name "error.log"

# 7日以上前のファイルを検索
find /tmp -type f -mtime +7

# 検索結果を削除
find /tmp -name "*.tmp" -delete

# 検索結果に対してコマンド実行
find . -name "*.py" -exec chmod +x {} \;

# ファイルサイズで検索（100MB以上）
find /home -type f -size +100M

# 特定ユーザーが所有するファイルを検索
find /data -user spark_user
```

### cp

ファイルやディレクトリをコピーするコマンド。

```bash
# ファイルをコピー
cp source.txt destination.txt

# ディレクトリを再帰的にコピー
cp -r /source/directory /destination/directory

# 上書き確認付きでコピー
cp -i important.txt backup.txt

# タイムスタンプを保持してコピー
cp -p original.txt copy.txt

# 複数ファイルを一度にコピー
cp file1.txt file2.txt file3.txt /destination/

# ワイルドカードでコピー
cp *.csv /data/backup/

# シンボリックリンクをそのままコピー
cp -d link_file /destination/
```

### デーモン

バックグラウンドで常駐し、特定のサービスを提供し続けるプログラム。

```bash
# デーモンの状態確認（systemctl）
sudo systemctl status docker

# デーモンを起動
sudo systemctl start postgresql

# デーモンを停止
sudo systemctl stop apache2

# デーモンを再起動
sudo systemctl restart nginx

# OS起動時に自動起動を設定
sudo systemctl enable redis

# 自動起動を無効化
sudo systemctl disable mysql

# 実行中のデーモン一覧
systemctl list-units --type=service --state=running

# 主なデーモンの例:
# - sshd: SSH接続を受け付けるデーモン
# - httpd/nginx: Webサーバーデーモン
# - mysqld: MySQLデータベースデーモン
# - dockerd: Docker管理デーモン
# - airflow-scheduler: Airflowスケジューラーデーモン
```

**デーモンプロセスの確認:**
```bash
# プロセス一覧からデーモンを確認
ps aux | grep docker

# デーモンのログを確認
sudo journalctl -u docker -f

# デーモンの詳細情報
sudo systemctl show docker
```
## プログラミング言語

データ分析基盤とは関係なしに、本書を読むために理解していることが望ましい用語について紹介します。

### Python

本書では、PysparkやPythonをメインとして利用しています。
Pythonはスラスラ読めるレベルを前提としており、Pysparkは本書内の付録で紹介しています。

### JavaとMaven

データの世界では、Javaは非常に多くのプロダクトで利用されています。
そのため、Javaに関する知識はデータ分析基盤を理解する上で重要です。

#### Java
汎用プログラミング言語のひとつ。  
特に大規模システムや企業向けアプリケーション開発に広く使われている。  
Javaでは、プログラムをコンパイルして`JAR`（Java ARchive）ファイルにまとめ、再利用可能な形で配布・利用できる。

#### JVM
Java Virtual Machine の略。JavaやScalaで書かれたプログラムを動かすための実行環境。
SparkやKafkaなど、データ分析基盤で使われる多くのソフトウェアは JVM 上で動作する。

#### JAR（Java ARchive）
Javaクラスファイルやリソースファイルをまとめたアーカイブファイル。  
JARをプロジェクトに追加することで、機能拡張や外部ライブラリの利用ができる。

#### Maven
Java向けのビルド＆依存管理ツール。  
プロジェクト内で必要なライブラリ（JARファイル）を、中央リポジトリ（Maven Central）から自動的に取得・管理できる。  
依存関係を`pom.xml`ファイルに記述することで、ライブラリ導入を簡単に行う仕組み。

mvnコマンドを用いて操作します。

### ロギング

なお、時代背景的にデータ分析関連のプロダクトはJava系のプロダクトが多いのだが、Java系ではログの出力管理はLog4jというライブラリで行っている。
そのため、Log4jと出てきたらログ関連だと思えばいい。

余談ですが、Javaは多くのプロダクトに利用されています。
エコシステムの大きさと成熟具合を感じるのでした。

| **プロダクト名**     | **Javaの使用**             | **特徴・理由**                                                                     |
| -------------------- | -------------------------- | ---------------------------------------------------------------------------------- |
| **Apache Hadoop**    | 主にJava                   | 分散ストレージと処理（HDFSとMapReduce）のために設計されたデータ基盤。              |
| **Apache Kafka**     | 主にJava                   | 高スループットのメッセージングシステムをJavaで効率的に実現。                       |
| **Apache Flink**     | 主にJava（Scalaも使用）    | リアルタイムストリーミング処理に特化。                                             |
| **Apache Druid**     | 主にJava                   | 時系列データやイベントデータの高速クエリ処理を実現。                               |
| **PrestoDB (Trino)** | 主にJava                   | 異種データソースを統一的にクエリできる分散SQLエンジン。                            |
| **Elasticsearch**    | 主にJava                   | Luceneをベースとした高性能分散検索エンジン。                                       |
| **Apache Solr**      | 主にJava                   | Luceneの上に構築され、高度な全文検索やファセット検索が可能。                       |
| **Apache Hive**      | 主にJava                   | Hadoop上で動作するデータウェアハウス。SQLライクなクエリが可能。                    |
| **Apache Spark**     | 主にJava（Scalaも使用）    | 大規模データのバッチ処理、ストリーミング処理、機械学習のための汎用フレームワーク。 |
| **OpenMetadata**     | 主にJava                   | メタデータ管理、データリネージュ、品質管理を提供するオープンソースツール。         |
| **Apache Beam**      | 主にJava（他の言語も対応） | バッチとストリーミングの統一モデルを提供するデータ処理フレームワーク。             |
| **Metabase**         | Java + Clojure             | オープンソースのBIツールで、直感的なインターフェースと可視化機能を提供。           |

## 2.x Dockerの基礎コマンドを学んでおこう

<lead>
本書はツールの使い方的な説明は（必要でない限り）ほぼ実施はしない予定だ。
しかしDOckerファイルの見方が分からなければ全くいみふめいになってしまう可能性があるので味方について例を持って紹介する。
すべては紹介できないことはご了承いただきたいが、Dockerファイルにはコメントを逐次付与しているため必要に応じて確認できるようにてありますので必要に応じて確認してください。
</lead>

###　Dockerについて

Dockerはコンテナ型の仮想化技術です。
ホストOSのカーネルを共有しつつ、コンテナごとに独立した環境を提供します。

そのコンテナを作成するための設計図がDockerファイルです。
Dockerファイルをもとにコンテナイメージを作成し、そのイメージをもとにコンテナを起動します。

### Dockerファイルの見方

Dockerファイルにはさまざまなコマンドがありますが、よく使うものを紹介します。

#### FROMコマンド

どのイメージをベースにするかを指定します。

```
FROM ubuntu:22.04
```

#### RUNコマンド

RUN `Linuxコマンド`でコンテナ内でコマンドを実行します。

```
RUN apt update && apt install -y curl jq
```

#### COPYコマンド

ホストOSのファイルをコンテナ内にコピーします。

```
COPY ./localfile /containerfile
```

#### ENVコマンド

環境変数を設定します。

```
ENV MY_ENV_VAR=value
```
#### WORKDIRコマンド

作業ディレクトリを設定します。

```
WORKDIR /app
```

以降のコマンドはこのディレクトリを基準に実行されます。

#### ARGコマンド

共通の変数を設定します。

```
ARG VERSION=1.0
RUN echo $VERSION
``` 

#### USERコマンド

コマンドを発行するユーザを指定します。

```
USER root
```

### Docker Compose

Docker Composeは複数のコンテナをまとめて管理するためのツールです。
`docker-compose.yml`ファイルに複数のサービスを定義し、一括で起動・停止できます。
このようなサービスをコンテナオーケストレーションサービスと呼びます。
他のコンテナオーケストレーションサービスにはKubernetes（k8s）やDocker Swarmなどがあります。

本書で利用している代表的なdocker-compose.ymlの設定例を示します。

```
  spark-worker:
    # ── Spark Workerコンテナの定義
    build: .  # カレントディレクトリのDockerfileを使用してビルド
    
    # ── コンテナ名とホスト名（スケーラビリティのためコメントアウト）
    # workerはホスト名を指定しない。スケールが楽になるので
    # container_name: spark-worker.local.data.platform  # 固定名にすると複数起動できない
    # hostname: spark-worker.local.data.platform  # Docker Composeが自動で割り振るため不要
    
    # ── 環境変数の設定（ファイルから読み込み）
    env_file:
      - ./conf/spark/worker.env  # Worker固有の環境変数（SPARK_MASTER_URLなど）を定義
    
    # ── ボリュームマウント（設定ファイルの共有）
    volumes:
      - ./conf/spark:/etc/spark/conf  # Spark設定ファイル（spark-defaults.confなど）をマウント
      - ./conf/hive:/etc/hadoop/conf  # Hive/Hadoop設定ファイル（hive-site.xmlなど）をマウント
      # - ../resource_manager/conf/yarn-site.xml:/home/pyspark/spark/conf/yarn-site.xml  # YARN連携時に使用
    
    # ── ネットワーク設定
    networks:
      - backend  # backendネットワークに接続（Masterや他サービスと通信可能）
    
    # ── 起動順序の制御
    depends_on:
      - spark-master  # Spark Masterが起動してからWorkerを起動
```

本書においてローカル環境での解説(1~9章)はDocker Compseを利用して解説を行います。

### シークレット情報の取り扱い

わかりやすさのため、シークレット情報はコンテナのイメージに直書きをしています。
本来はシークレットにて管理することによって、コンテナ起動時にシークレット情報をインジェクションするのが通例のためご留意ください。

#### 本書での書き方(直書き)

非推奨ですが、本書ではシンプルにするため以下の記法を用いています。

```
# MINIOの操作で利用するアクセスキーとシークレット
# 利便性のため設定しています。本当はENVに入れるのはダメです。GH_TOKENのようにsecretなどで設定しましょう。
ENV AWS_ACCESS_KEY_ID=5nCJP6jHFJd7PDsLlT3a
ENV AWS_SECRET_ACCESS_KEY=FXn6MFKDbNamyMzxiMBGIpgTDFu2r1IfymESfRJd
```

#### 本来のシークレット活用例

実際に適用する際は以下のようにシークレット情報を別で管理します。

```

# 本来のシークレットの扱い(利便性のため他のdocker-composeは直書きする)
secrets:
    my_secret:
        file: ./.secret

secret
GH_TOKEN=adafasdfsafa...
とすると、起動時にインジェクションされGH_TOKENが環境変数として適用されます。

```

## SQL講座

SQLの操作について簡単に学んでおきましょう。

### データベースに接続
ワーキングコンテナから、データソース側のデータベースへ繋具コマンド。

```
psql -h host.docker.internal -p 5432 -U postgres
```

postgres基本コマンド

データベースの一覧
postgres=# \l

テーブルの一覧
postgres=# \d

```
                      List of relations
 Schema |             Name              |   Type   |  Owner   
--------+-------------------------------+----------+----------
 public | orders                        | table    | postgres
 public | people                        | table    | postgres
 public | product_master                | table    | postgres
 public | product_master_product_id_seq | sequence | postgres
 public | products                      | table    | postgres
 public | purchase                      | table    | postgres
 public | purchase_purchase_id_seq      | sequence | postgres
(7 rows)

```

### テーブルの基本操作

select * from orders limt 1;

```
postgres=# select * from orders limit 1;
 id | user_id | product_id |      subtotal      | tax  |       total        | discount |       created_at        | quantity 
----+---------+------------+--------------------+------+--------------------+----------+-------------------------+----------
  1 |       1 |         14 | 37.648145389078365 | 2.07 | 39.718145389078366 |          | 2019-02-11 21:40:27.892 |        2
(1 row)
```

#### 条件付き

```
select * from orders where id=74;
```

#### 値の比較

```
select * from orders where created_at < '2018-11-11';

```

#### 複数条件(and 且つ条件)

```
select * from orders where id >70 and tax <4;
```

#### 複数条件(or または条件)

```
select * from orders where id >70 or tax <4;
```

#### Like

```
select Product_id from orders where cast(Product_id as varchar) like '%36%';
※%はなんでもありという意味
```

テーブルの検索結果を条件にして検索をする事ができる#### 副問い合わせ

```
select * from orders 
where id = (
    select id from orders where id % 2 =0 and id <10 limit 1
);
```

```
select 
 * 
from 
 (select 1 ) hoge;

```

#### Case文

もしXXXであったら？というように条件分岐をする事ができる

```
select 
    CASE id
        WHEN 1 THEN 'hoge'
        WHEN 3 THEN 'peke'
        ELSE 'gggggg'
    END as peken
    , id
from
 orders
where
 id in(1,2,3);
```

#### CTE

withによる繰り返しの回避と処理途中結果の中間点作成
繰り返し行うような処理におすすめです

```
WITH orders_with AS (
  SELECT tax,created_at
  FROM
    orders od
  WHERE
    od.tax > 4
)
select 
    *
from 
    orders_with
union all 
select 
    *
from 
    orders_with;
```

### 結合

複数テーブルを操作する join
inner joinとleft join
product_idはproduct テーブルのidと紐ついています。そのためproduct_idとidを結合してみます。
product_idだけではわからなかった、productの名前がわかるようになります。

```

select distinct od.product_id,pd.title
from 
ORDERS od
inner join PRODUCTS pd on od.product_id = pd.id;


```

### 変換・集計

#### null を別の値で置き換える
平均値などで埋め合わせることもある

```
with hoge as (
    select 1 as seq
    union all select 2 as seq
    union all select null as seq
)
SELECT COALESCE(seq, 99) FROM hoge;
```

#### lpad/rpad

桁数を揃える

```
select distinct lpad(cast(user_id as varchar), 8 , '0') from orders;
```

#### Window関数

Window(窓)を作成し、その窓ごとに集計を行う

```

select 
    product_id,total,
    row_number() over(
        partition by product_id
        order by total
    )
from
orders;

```

## マテリアライズドビュー
クエリ結果を事前に実体として保持するビュー。
集計結果などをあらかじめ保存しておくことで、毎回計算する負荷を減らし、参照性能の向上を図れる。

## 他

関連する、いくつかの基礎知識を紹介します。

### API
　API（Application Programming Interface）とは、サービスを利用する人が事前に定められた形式に従っ
て使いたい機能や情報を添えて「リクエスト」（要求）します。それに対して、サービス側はリクエス
トを受け取ると、送信された条件をサーバー側で処理して「レスポンス」（応答）を返します。

### 監視

監視は、システムやアプリケーションの状態を把握するための事前に定義されたメトリクスやログ、アラートを使った手法です。
監視ツールでは、CPU使用率、メモリ使用量、レスポンスタイム、エラーレートなどのメトリクスを収集し、異常が発生した場合にアラートを送る仕組みが一般的です。
これは、既知の問題や異常を検知するために使われます。

以下は監視でよく設定される項目です。

1. ストレージ管理
2. ヘルスチェック
3. 外形監視
4. キューの長さと消費率
5. トレンドの確認
6. メモリ・CPU使用率
7. ネットワークI/O
8. ディスクI/O
