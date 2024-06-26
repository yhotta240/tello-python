# PythonでTelloドローンを操作する


## はじめに

Telloドローンは、その手軽さと高度な機能から、多くのドローン愛好者や開発者に人気です。本記事では、Pythonを使ってTelloドローンを操作する方法について説明します。今回は、ライブラリを極力使わずに、UDPソケットを用いて直接Telloにコマンドを送信するシンプルな方法を紹介します。

## 必要なもの

- Telloドローン
- Python 3.x
- Wi-Fi接続できるPC

## Tello SDKの基本

Telloドローンは、SDK（Software Development Kit）を通じてコマンドを受け取ります。これらのコマンドは、UDPプロトコルを使用して送信されます。以下は、基本的なコマンドの例です：

- `command`：SDKモードを開始
- `takeoff`：離陸
- `land`：着陸
- `forward x`：前進（xはセンチメートル）
- `back x`：後退（xはセンチメートル）
- `cw x`：時計回りに旋回（xは度）

## サンプルコード

### 1. ソケットの作成

まず、UDPソケットを作成します。このソケットは、Telloドローンにコマンドを送信するために使用されます。

```python
import socket

# TelloのIPアドレスとポート
TELLO_IP = '192.168.10.1'
TELLO_PORT = 8889

# コマンドを送信するためのソケットを作成
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
tello_address = (TELLO_IP, TELLO_PORT)
```

### 2. コマンド送信

次に、Telloドローンにコマンドを送信するための関数を定義します。

```python
def send_command(command):
    """
    Telloにコマンドを送信する関数
    """
    try:
        sock.sendto(command.encode('utf-8'), tello_address)
        print(f"Sending command: {command}")
    except Exception as e:
        print(f"Error sending command: {e}")
```

### 3. レスポンス受信

Telloドローンからのレスポンスを受信するための関数も定義します。

```python
def receive_response():
    """
    Telloからのレスポンスを受信する関数
    """
    try:
        response, _ = sock.recvfrom(1024)
        print(f"Received response: {response.decode('utf-8')}")
    except Exception as e:
        print(f"Error receiving response: {e}")
```

### 4. メイン関数の定義

最後に、ドローンの基本的な操作（離陸、前進、後退、旋回、着陸）を行うメイン関数を定義します。

```python
def main():
    # コマンドモードを開始
    send_command("command")
    time.sleep(2)
    receive_response()

    # 離陸
    send_command("takeoff")
    time.sleep(5)
    receive_response()

    # 前進（100cm）
    send_command("forward 100")
    time.sleep(5)
    receive_response()

    # 後進（100cm）
    send_command("back 100")
    time.sleep(5)
    receive_response()

    # 旋回（時計回りに90度）
    send_command("cw 90")
    time.sleep(5)
    receive_response()

    # 着陸
    send_command("land")
    time.sleep(5)
    receive_response()

    # ソケットを閉じる
    sock.close()

if __name__ == "__main__":
    main()
```

## 実行手順

1. Telloドローンの電源を入れ、Wi-Fiに接続します。
2. 上記のPythonコードを実行します。

このサンプルコードでは、Telloドローンが離陸し、前進、後退、旋回、そして着陸する一連の動作を行います。各コマンドの後にレスポンスを受信しているため、コマンドが正常に送信されたかどうかを確認できます。

## まとめ

今回は、Pythonを使用してTelloドローンを操作するシンプルな方法を紹介しました。ライブラリを使用せずに直接コマンドを送信することで、Telloドローンの基本的な操作を理解することができます。この方法を基に、さらに高度な操作や自動化を実装することも可能です。ぜひ試してみてください。

## 参考

Tello SDK 2.0 User Guide

https://dl-cdn.ryzerobotics.com/downloads/Tello/Tello%20SDK%202.0%20User%20Guide.pdf
