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
## ホーム以下のディレクトリの英語化
```bash
LANG=C xdg-user-dirs-gtk-update
```

## cuda11.2のインストール

#### 前準備
関連ファイルやパッケージの削除
```bash
sudo apt --purge remove -y cuda* libcuda* nvidia* libnvidia* && sudo apt autoremove -y && sudo apt clean -y
reboot
```

#### nvidia-driverのインストール
```bash
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt update
ubuntu-drivers devices
sudo apt install nvidia-driver-495　（最新バージョン）
reboot
```
#### cuda-11-2 toolkitのインストール
```bash
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-ubuntu2004.pin
sudo mv cuda-ubuntu2004.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/11.2.0/local_installers/cuda-repo-ubuntu2004-11-2-local_11.2.0-460.27.04-1_amd64.deb
sudo dpkg -i cuda-repo-ubuntu2004-11-2-local_11.2.0-460.27.04-1_amd64.deb
sudo apt-key add /var/cuda-repo-ubuntu2004-11-2-local/7fa2af80.pub
sudo apt-get update
sudo apt-get -y install cuda-11-2
```

## xrdpのインストール

```bash
sudo apt install xrdp
sudo sed -e 's/^new_cursors=true/new_cursors=false/g' -i /etc/xrdp/xrdp.ini
cat <<EOF > ~/.xsessionrc
export GNOME_SHELL_SESSION_MODE=ubuntu
export XDG_CURRENT_DESKTOP=ubuntu:GNOME
export XDG_DATA_DIRS=/usr/share/ubuntu:/usr/local/share:/usr/share:/var/lib/snapd/desktop
export XDG_CONFIG_DIRS=/etc/xdg/xdg-ubuntu:/etc/xdg
EOF

cat <<EOF | sudo tee /etc/polkit-1/localauthority/50-local.d/xrdp-color-manager.pkla
[Netowrkmanager]
Identity=unix-user:*
Action=org.freedesktop.color-manager.create-device
ResultAny=no
ResultInactive=no
ResultActive=yes
EOF

sudo systemctl restart polkit
sudo systemctl restart xrdp

```
