## ラズパイでソフトウェアシリアルを使う
#### pigpioのインストール
[pigpio download](http://abyz.me.uk/rpi/pigpio/download.html)よりpigpioをインストールします
Dwnload and install latest versionに従い以下を順に実行します．
makeコマンドはそこそこ時間がかかります．
($は無視してください)
```
$ wget https://github.com/joan2937/pigpio/archive/master.zip  
$ unzip master.zip  
$ cd pigpio-master  
$ make
$ sudo make install
```
完了したら、pigpio.pyをプログラムが実行されるディレクトリにコピーします．
以下をターミナルで実行．
(GUI環境で操作する場合は単にファイルをコピーペーストでok)
```
~/pigpio-master$ cp pigpio.py コピー先のディレクトリのアドレス
```
#### pigpio daemonの起動
pigpioを使用する際は**pigpio daemonを起動させる必要**があります．
これは**raspiを起動するごとに終了してしまう**ので、常に起動させておきたい場合はraspiを起動する際にpigpio daemonを起動させるように設定する必要があります．
pigpio daemonを起動させるコマンドは以下の通りです．
```
sudo pigpiod
```
#### ソースコードの記述
以下はサンプルコードで．．
```python
import pigpio



baudrate = 9600

TX = 24

RX = 23

serialpi = pigpio.pi()

serialpi.set_mode(RX,pigpio.INPUT)

serialpi.set_mode(TX,pigpio.OUTPUT)

pigpio.exceptions = False

serialpi.bb_serial_read_close(RX)

pigpio.exceptions = True

serialpi.bb_serial_read_open(RX,baudrate,8)

count = 0

(count, sentence) = serial.bb_serial_read(RX)
```
`baudrate`はこれ以上の値にするとうまくデータが受け取れない可能性があります．
TXとRXにはGPIO番号を代入することでそのpinを用いてシリアル通信を行うことが出来ます．
サンプルコードの`TX`、`RX`は単なる変数です．
`(count, sentence) = serial.bb_serial_read(RX)`は`sentence`が0より大きいかどうかを調べることでデータが受け取れているかを調べることが出来ます．
データが受け取れていれば`sentence > 0`は`True`です．
また、未検証ですが複数のsoftware serialを用いて通信することも可能だと思います．
