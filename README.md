# NixOSインストール

## rootユーザーに変更
```
$ sudo -i
```

## 日本語キーボードに変更
```
# loadkeys jp106
```

## Wi-Fiに接続
```
# systemctl start wpa_supplicant
# wpa_cli

> add_network
0
> set_network 0 ssid "myhomenetwork"
OK
> set_network 0 psk "mypassword"
OK
> enable_network 0
OK
> quit
```

## パーティション切り
```
# sgdisk -Z /dev/nvme0n1
# sgdisk -n 1::+512M -n 2:: -t 1:ef00 -t 2:8304 /dev/nvme0n1
```

## フォーマット
```
# mkfs.fat -F 32 /dev/nvme0n1p1
# mkfs.btrfs /dev/nvme0n1p2
```

## マウント
```
# mount -o compres=zstd:1,noatime,space_cache=v2 /dev/nvme0n1p2 /mnt
# mount -m -o umask=077 /dev/nvme0n1p1 /mnt/boot
```

## スワップファイル作成、マウント
```
# btrfs fi m -s 8g -U clear /mnt/swapfile
# swapon /mnt/swapfile
```

## `configuration.nix`生成
```
# nixos-generate-config --root /mnt
```
