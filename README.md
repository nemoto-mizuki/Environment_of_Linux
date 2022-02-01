# 環境構築手順など

## swap領域の追加
```bash
sudo fallocate -l 1G /swapfile
```
アクセス権限の確認と変更
```bash
ls -lh /swapfile
sudo chmod 600 /swapfile
ls -lh /swapfile

出力
-rw-r--r-- 1 root root 40G  2月  1 14:03 /media/Data/.swapfile
↓
-rw------- 1 root root 40G  2月  1 14:03 /media/Data/.swapfile
```
スワップ領域としてマーク
```bash
sudo mkswap /swapfile
```
スワップファイルを有効にする
```bash
sudo swapon /swapfile
```
確認
```bash
sudo swapon --show
```
