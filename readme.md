![](https://raw.githubusercontent.com/yKesamaru/crontab_and_system-reInstall/master/assets/eye-catch1.webp)

## はじめに
システム起動時のスタートアップアプリケーション設定についてまとめます。

わたしの場合、システムの再インストール時に、自動的に設定を復元する必要があります。その点についても書き添えたいと思います。

## 環境
```bash
$ inxi -S
System:
  Host: terms Kernel: 6.5.0-28-generic x86_64 bits: 64 Desktop: GNOME 42.9
    Distro: Ubuntu 22.04.4 LTS (Jammy Jellyfish)
```

## スタートアップアプリケーションを設定する複数の方法
基本的に２パターン存在します。
1. `~/.config/autostart/**.desktop`ファイルを作成する方法
2. `crontab`により設定する方法

### `~/.config/autostart/**.desktop`ファイルを作成する方法
`~/.config/autostart/**.desktop`ファイルは`デスクトップ設定ファイル`であり、MIMEタイプは`application/x-desktop`となります。

わたしの場合、このデスクトップ設定ファイルから自作のbashスクリプトを呼び出すようにする場合もあります。この方法の利点は視認性が良く、バックアップからの復元も容易いことにあります。

設定を手助けするGUIアプリケーションとして`gnome-tweaks`や`gnome-session-properties`が存在します。

これらで登録する場合、`~/.config/autostart/**.desktop`ファイルが作成されます。

また、`~/.config/autostart/**.desktop`ファイルは直に作成することもできます。
例えば、`gnome-tweaks`からシステムモニターを起動時アプリケーションとして登録すると`~/.config/autostart/**.desktop`ファイルは以下のように作られます。
```bash:~/.config/autostart/gnome-system-monitor.desktop
[Desktop Entry]
Name=System Monitor
Comment=View current processes and monitor system state
TryExec=gnome-system-monitor
Exec=gnome-system-monitor
# Translators: Do NOT translate or transliterate this text (this is an icon file name)!
Icon=org.gnome.SystemMonitor
Terminal=false
Type=Application
StartupNotify=true
Categories=GNOME;GTK;System;Monitor;
NotShowIn=KDE;
X-GNOME-Bugzilla-Bugzilla=GNOME
X-GNOME-Bugzilla-Product=system-monitor
X-GNOME-Bugzilla-Component=general
X-GNOME-Bugzilla-Version=42.0
# Translators: Search terms to find this application. Do NOT translate or localize the semicolons! The list MUST also end with a semicolon!
Keywords=Monitor;System;Process;CPU;Memory;Network;History;Usage;Performance;Task;Manager;Activity;
X-Ubuntu-Gettext-Domain=gnome-system-monitor
```
`gnome-tweaks`を使用して`.desktop`ファイルを作成した場合、コマンド引数を指定することができません。
コマンド引数を必要とする場合は、作成された`.desktop`ファイルを直に編集するか、最初から`gnome-session-properties`を使用する必要があります。

#### 復元方法
バックアップされた`~/.config/autostart/`ディレクトリをコピーするだけです。

### crontabを用いる方法
こちらは少々煩雑なのでわたしは用いません。
ここでは簡単な紹介にとどめます。
#### Cronジョブを編集
`crontab -e`
#### Cronジョブにコマンドを追加
`@reboot /usr/bin/vlc (コマンドライン引数)`
あるいは
`@reboot /path/to/vlc.sh`
#### crontabのバックアップ
`crontab -l crontab_backup.txt`
#### crontabの復元方法
`crontab crontab_backup.txt`

以上です。ありがとうございました。
