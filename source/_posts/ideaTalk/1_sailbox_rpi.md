---
title: SAIL BOX
categories: 学习琐事
date: 2021-10-24 22:11:30
comments: false
toc: true
reward: false
top: 
tags:
	- Raspberry
	- sailbox
---

## 0x01 名称由来

Sail，可以意为启航，表示即将开启一段新的征程。

云雀

## 0x02 基本的配置过程

[jaspar-client](https://github.com/jasperproject/jasper-client)

（1）选择Expand Filesystem，并重启
``` bash
sudo raspi-config
```
（2）安装必要的工具
``` bash
sudo apt-get update
sudo apt-get upgrade --yes
sudo apt-get install nano git-core python-dev bison libasound2-dev libportaudio-dev python-pyaudio --yes
sudo apt-get remove python-pip
# error for installing pip（需要源码先安装setup-tools，再源码编译安装pip）
# sudo easy_install pip
```
（3）插上USB麦克风，创建一个ALSA的配置文件，内容如下
```bash
sudo nano /lib/modprobe.d/sailbox.conf
```
```bash
# Load USB audio before the internal soundcard
options snd_usb_audio index=0
options snd_bcm2835 index=1

# Make sure the sound cards are ordered the correct way in ALSA
options snd slots=snd_usb_audio,snd_bcm2835
```
（4）重启RPi
```bash
sudo shutdown -r now
```
（5）记录一些声音文件
```bash
arecord temp.wav
```
（6）回放声音文件
```bash
aplay -D hw:1,0 temp.wav
```
（7）修改'.bash_profile'，添加如下内容
```bash
export LD_LIBRARY_PATH="/usr/local/lib"
source .bashrc
```
（8）修改'.bashrc'，添加如下内容
```bash
LD_LIBRARY_PATH="/usr/local/lib"
export LD_LIBRARY_PATH
PATH=$PATH:/usr/local/lib/
export PATH
```

## 0x03 安装Jasper

（1）下载jasper源码
```bash
git clone https://github.com/jasperproject/jasper-client.git jasper
```
（2）安装必须的Python库文件
```bash
sudo pip install --upgrade setuptools
sudo pip install -r jasper/client/requirements.txt
```

替换requirements.txt中的版本号：
```bash
# Jasper core dependencies
APScheduler==3.8.1
argparse==1.4.0
mock==4.0.3
pytz==2021.3
PyYAML==6.0
requests==2.26.0

# Pocketsphinx STT engine
cmuclmtk==0.1.5

# HN module
beautifulsoup4==4.10.0
semantic==1.0.3

# Birthday/Notifications modules
facebook-sdk==3.1.0

# Weather/News modules
feedparser==6.0.8

# Gmail module
python-dateutil==2.8.2

# MPDControl module
python-mpd==0.3.0
```

安装依赖报错时，执行下面的命令：
```bash
sudo apt-get install python3.7-dev
```

```bash
sudo apt-get install python-numpy python-scipy python-matplotlib  python-pandas python-sympy python-nose
```
再次执行：
```bash
sudo pip install -r jasper/client/requirements.txt
```

（3）给jasper.py添加可执行权限
```bash
chmod +x jasper/jasper.py
```

## 0x04 安装STT（语音识别）引擎和TTS（文本转文本）引擎

由于被动唤醒会试图识别所有听到的内容，出于隐私保护的目的，使用离线的语音识别引擎，因此选PocketSphinx或者Julius。

### 安装 STT 引擎（Choosing an STT engine）

#### PocketSphinx STT engine

（1）sphinxbase & pocketsphinx

```bash
sudo apt-get update
sudo apt-get install pocketsphinx python-pocketsphinx
```
and:

```bash
wget http://downloads.sourceforge.net/project/cmusphinx/sphinxbase/0.8/sphinxbase-0.8.tar.gz
tar -zxvf sphinxbase-0.8.tar.gz
cd ~/sphinxbase-0.8/
./configure --enable-fixed
make
sudo make install
```
```bash
wget http://downloads.sourceforge.net/project/cmusphinx/pocketsphinx/0.8/pocketsphinx-0.8.tar.gz
tar -zxvf pocketsphinx-0.8.tar.gz
cd ~/pocketsphinx-0.8/
./configure
make
sudo make install
cd ..

# sudo easy_install pocketsphinx
```
（2）CMUCLMTK
安装依赖文件
```bash
sudo apt-get install subversion autoconf libtool automake gfortran g++ --yes
```

```bash
svn co https://svn.code.sf.net/p/cmusphinx/code/trunk/cmuclmtk/
cd cmuclmtk/
./autogen.sh && make && sudo make install
cd ..
```
（3）MIT Language Modeling Toolkit
（4）m2m-aligner
（5）OpenFST & Phonetisaurus

```bash
wget http://distfiles.macports.org/openfst/openfst-1.5.2.tar.gz
wget https://github.com/mitlm/mitlm/releases/download/v0.4.2/mitlm-0.4.2.tar.xz
wget https://storage.googleapis.com/google-code-archive-downloads/v2/code.google.com/m2m-aligner/m2m-aligner-1.2.tar.gz
wget https://storage.googleapis.com/google-code-archive-downloads/v2/code.google.com/phonetisaurus/is2013-conversion.tgz

tar -xzvf m2m-aligner-1.2.tar.gz
tar -xzvf openfst-1.5.2.tar.gz
tar -xzvf is2013-conversion.tgz
tar -xvJf mitlm_0.4.2.tar.xz
```

Build OpenFST:
```bash
cd openfst-1.5.2/
sudo ./configure --enable-compact-fsts --enable-const-fsts --enable-far --enable-lookahead-fsts --enable-pdt
# 修改1198行为fst_.InitArcIterator(state_, &cache_data_);
nano src/include/fst/replace.h
sudo make install # come back after a really long time, half an hour
```

Build M2M:
```bash
cd m2m-aligner-1.2/
sudo make
```

Build MITLMT:
```bash
cd mitlm-0.4.2/
sudo ./configure
sudo make install
```

Build Phonetisaurus:
```bash
cd is2013-conversion/phonetisaurus/src
sudo make
```

Move some of the compiled files:
```bash
sudo cp ~/m2m-aligner-1.2/m2m-aligner /usr/local/bin/m2m-aligner
sudo cp ~/is2013-conversion/bin/phonetisaurus-g2p /usr/local/bin/phonetisaurus-g2p
```

Building the Phonetisaurus FST model
```bash
#wget https://www.dropbox.com/s/kfht75czdwucni1/g014b2b.tgz
https://pan.baidu.com/s/1o7MrWIA #百度网盘下载g014b2b.tgz
tar -xzvf g014b2b.tgz
```

Build Phonetisaurus model:
```bash
cd g014b2b/
./compile-fst.sh
cd ..
```

Rename the following folder for convenience:
```bash
mv ~/g014b2b ~/phonetisaurus
```

Finally, restart your Pi.
```bash
sudo shutdown -r now  ##重启
sudo shutdown -h now  ##关机
```

#### Julius STT engine

（6）Install Dependencies for Julius STT engine
```bash
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev flex libasound2-dev libesd0-dev libsndfile1-dev

wget https://osdn.net/projects/julius/downloads/60273/julius-4.3.1.tar.gz
cd ~/julius
./configure --enable-words-int
make
sudo make install
```

### 安装 TTS 引擎（Choosing a TTS engine）

（7）Install Dependencies for eSpeak TTS engine
```bash
sudo apt-get update
sudo apt-get install espeak
```

（8）Install Dependencies for Festival TTS engine
```bash
sudo apt-get update
sudo apt-get install festival festvox-don
```

（9）Install Dependencies for Flite TTS engine
```bash
sudo apt-get update
sudo apt-get install flite
```

（10）Install Dependencies for SVOX Pico TTS engine
```bash
sudo apt-get update
sudo apt-get install libttspico-utils
```

（11）Install Dependencies for Google TTS engine
```bash
sudo apt-get update
sudo apt-get install python-pymad
sudo pip install --upgrade gTTS
```

（12）Install Dependencies for Ivona TTS engine
```bash
sudo apt-get update
sudo apt-get install python-pymad
sudo pip install --upgrade pyvona
```

## 0x05 安装百度语音引擎


## 0x06 配置文件（.jasper/profile.yml）

### 生成配置文件



```bash
cd ~/jasper/client
python populate.py
```
出现报错：
ImportError: No module named yaml
ImportError: No module named feedparser
```bash
sudo apt-get install python-yaml
sudo apt-get install python-feedparser
```

```bash
nano .jasper/profile.yml

stt_engine: sphinx
pocketsphinx:
  fst_model: '../phonetisaurus/g014b2b.fst'
  hmm_dir: '/usr/local/share/pocketsphinx/model/hmm/en_US/hub4wsj_sc_8k'
tts_engine: espeak-tts
espeak-tts:
  voice: 'default+m3'   # optional
  pitch_adjustment: 40  # optional
  words_per_minute: 160 # optional
```

### 配置Spotify

```bash
wget -q -O - http://apt.mopidy.com/mopidy.gpg | sudo apt-key add -
sudo wget -q -O /etc/apt/sources.list.d/mopidy.list http://apt.mopidy.com/mopidy.list
sudo apt-get update

sudo apt-get install python3-pykka
sudo apt-get install python3-spotify
sudo apt-get install mopidy mopidy-spotify --yes

sudo modprobe ipv6
echo ipv6 | sudo tee -a /etc/modules
```
```bash
sudo nano /root/.asoundrc

pcm.!default {
        type hw
        card 1
}
ctl.!default {
        type hw
        card 1
}
```
```bash
sudo mkdir /root/.config
sudo mkdir /root/.config/mopidy
sudo rm /etc/init.d/mopidy

sudo nano /root/.config/mopidy/mopidy.conf

[spotify]
username = YOUR_SPOTIFY_USERNAME
password = YOUR_SPOTIFY_PASSWORD

[mpd]
hostname = ::

[local]
media_dir = ~/music

[scrobbler]
enabled = false

[audio]
output = alsasink
```

设置开机自启
```bash
sudo crontab -e
@reboot mopidy;
```

## 0x07 如何使用

开启jasper
```bash
/home/pi/project/intelligent_voice/jasper/jasper.py
```
ImportError: No module named requests
ImportError: No module named pip.req
ImportError: No module named apscheduler.schedulers.background
```bash
sudo apt-get install python-requests
#curl https://bootstrap.pypa.io/pip/2.7/get-pip.py --output get-pip.py
#sudo python2 get-pip.py
#python2 -m pip install requests
python2.7 -m pip install req
sudo pip2 install apscheduler
```

NameError: global name 'cmuclmtk' is not defined
```bash
# 注释vocabcompiler.py的290行
```

设置开机自启
```bash
sudo crontab -e
@reboot /home/pi/project/intelligent_voice/jasper/jasper.py;
```

修改唤醒的名称


使用命令行唤醒
Set the JASPER_HOME environment variable, then run main.py as follows: JASPER_HOME="/home/pi" && export JASPER_HOME && python main.py --local. The --local flag enables the command-line interface (CLI)

小模块
（1）status
```bash
sudo pip install psutil
git clone https://github.com/edouardpoitras/jasper-status.git status
cp ~/project/intelligent_voice/modules_jasper/status/Status.py ~/project/intelligent_voice/jasper/client/modules/
sudo shutdown -r now
```
（2）Reboot
```bash
git clone https://github.com/ArtBIT/jasper-module-reboot.git reboot
cp reboot/Reboot.py /usr/local/lib/jasper/client/modules/
sudo reboot
```
（3）shutdown
```bash
git clone https://github.com/ArtBIT/jasper-module-shutdown.git shutdown
cp shutdown/Shutdown.py /usr/local/lib/jasper/client/modules/
sudo shutdown
```


## 0x08 其他

普通 USB 摄像头，需要安装一个 fswebcam 拍摄软件:
```bash
sudo apt-get install fswebcam
```

查看树莓派的温度：
```bash
vcgencmd measure_temp
```

Macbook pro的上传和下载：
```bash
# upload
scp -r local_folder remote_username@remote_ip:remote_folder
# scp -r ./setuptools-58.3.0.tar.gz sulay@192.168.0.108:/Users/sulay/Downloads
```
```bash
# download
scp -r remote_username@remote_ip:remote_folder local_folder
# scp -r sulay@192.168.0.108:/Users/sulay/Downloads/g014b2b.zip ./
```
```bash
# 
1.-v 显示进度
2.-r 递归处理
3.-C 压缩选项
4.-P 选择端口
```

FAQ:

（1）Why can’t I login to my Raspberry Pi after running Jasper?
Jasper’s attempts to connect with a wifi network may be interfering with your attempts to make an ethernet connection. To resolve this, disconnect your wifi adapter, restart your Jasper, then plug in the ethernet cable only after Jasper fails to find a wireless network.

（2）Can I control the volume or make Jasper more sensitive to sound levels?
Run alsamixer on your Raspberry Pi for microphone and speaker level controls. Raising the microphone level will help Jasper to hear your voice from a greater distance.

（3）Why do I get a segmentation fault when running main.py?
This is usually caused by the speech dictionary not being created during the boot process. Make sure you’ve added the boot script to crontab, then restart your Pi. If a dictionary.dic file is still not present in your client/ directory, just run jasper/boot/boot.py manually to force the dictionary generation.

（4）Can I change Jasper’s name?
Sure, you can just replace the “Jasper” name in main.py and musicmode.py. Then, create a new dictionary_persona.dic and languagemodel_persona.lm to include the new name you decide. We recommend the online lmtool to regenerate the dictionary and language model. More information is available here.


## 0x09 profile的书写

```bash
robot_name: 'DINGDANG'  # 必须使用大写
robot_name_cn: '叮当'
first_name: '伟洲'
last_name: '潘'
timezone: HKT
location: '深圳'

# 是否接入微信
wechat: true

# 当微信发送语音时，是直接播放语音还是执行语音命令？
# true：直接播放
# false：执行语音命令（只支持百度STT，其他两种STT识别不准）
wechat_echo: false

# 除了自己之外，还能响应 echo 指令的好友微信名单
# 如果填写 ['ALL'] 表示响应所有微信好友
# 如果填写 [] 表示不响应任何好友
wechat_echo_text_friends: ['小Q', 'HaHack']

# 除了自己之外，还能直接播放语音的好友微信名单
# 如果填写 ['ALL'] 表示播放所有微信好友的语音
# 如果填写 [] 表示不播放任何好友的语音
wechat_echo_voice_friends: ['小Q']

# 当有邮件时，是否朗读邮件标题
read_email_title: true

# 当内容过长（> 200个字）时，是否继续朗读
# true：读
# false：改为发送内容
read_long_content: false

# 最长朗读内容（仅当 read_long_content 为 false 时有效）
max_length: 200

# 是否使用邮箱发送长内容而不是微信
prefers_email: false

# 勿扰模式，该时间段内不执行通知检查
do_not_bother:
    enable: true # 开启勿扰模式
    since: 23    # 开始时间
    till: 9      # 结束时间，如果比 since 小表示第二天

# wav声音播放配置
# 可选值：
# aplay         - 子进程aplay播放
# pyaudio       - pyaudio模块播放
sound_engine: aplay

# mp3文件播放配置
# 可选值：
# play          - 子进程play播放
# pygame        - pygame库播放(树莓派python默认自带，推荐配置)
# vlc           - vlc库播放(短音频可能播放有问题)
music_engine: play

# 语音合成服务配置
# 可选值：
# baidu-tts     - 百度语音识别
# iflytek-tts   - 讯飞语音合成
# ali-tts       - 阿里语音合成
# google-tts    - 谷歌语音合成
tts_engine: baidu-tts

# STT 服务配置
# 可选值：
# sphinx        - pocketsphinx离线识别引擎（需训练，参考修改唤醒词教程）
# baidu-stt     - 百度在线语音识别
# iflytek-stt   - 讯飞语音识别
# ali-stt       - 阿里语音识别
# google-stt    - 谷歌语音合成
stt_engine: baidu-stt

# 离线唤醒 SST 引擎
# 可选值：
# sphinx        - pocketspinx离线唤醒                                                                                                                                           
# snowboy-stt   - snowboy离线唤醒
stt_passive_engine: sphinx

# pocketsphinx 唤醒SST引擎（默认）
pocketsphinx:
    fst_model: '/home/pi/g014b2b/g014b2b.fst'
    hmm_dir: '/usr/share/pocketsphinx/model/hmm/en_US/hub4wsj_sc_8k'

# snowboy 唤醒SST引擎（可选）
# https://snowboy.kitt.ai/dashboard
snowboy:
    model: '/home/pi/dingdang/client/snowboy/dingdangdingdang.pmdl'  # 唤醒词模型
    sensitivity: "0.5"  # 敏感度

# 百度语音服务
# http://yuyin.baidu.com/
baidu_yuyin:
    api_key: '填写你的百度应用的API Key'
    secret_key: '填写你的百度应用的Secret Key'
    per: 0  # 发音人选择 0：女生；1：男生；3：度逍遥；4：度丫丫

# 讯飞语音服务
# api_id 及 api_key 需前往
# http://aiui.xfyun.cn/webApi
# 注册获取（注意创建的是WebAPI应用），仅使用语音合成无需注册
# 然后将主板的ip地址添加进ip白名单（建议使用中转服务器的ip地址 101.132.139.80）
iflytek_yuyin:
    api_id: '填写你的讯飞应用的Api ID'
    api_key: '填写你的讯飞应用的Api Key'  # 没看到这个说明不是注册的WebAPI应用，请改注册个WebAPI应用
    vid: '67100' #语音合成选项： 60120为小桃丸 67100为颖儿 60170为萌小新 更多音色见wiki
    url: 'http://api.musiiot.top/stt.php' # 白名单ip中转服务器（可选）
    tts:
        api_id: '***' # 这项不填可以使用上层配置
        api_key: '**********************'
        voice_name: xiaoyan
        proxy: 'http://123.207.49.217:8028'

# 阿里云语音
# ak_id及ak_secret需前往
# https://data.aliyun.com/product/nls
# 注册获取
ali_yuyin:
    ak_id: '填写你的阿里云应用的AcessKey ID'
    ak_secret: '填写你的阿里云应用的AcessKey Secret'
    voice_name: 'xiaoyun' #xiaoyun为女生，xiaogang为男生

# 谷歌语音
# api_key 的获取方式：
# 1. Join the Chromium Dev group:
#     https://groups.google.com/a/chromium.org/forum/?fromgroups#!forum/chromium-dev
# 2. Create a project through the Google Developers console:
#     https://console.developers.google.com/project
# 3. Select your project. In the sidebar, navigate to "APIs & Auth." Activate
#     the Speech API.
# 4. Under "APIs & Auth," navigate to "Credentials." Create a new key for
#     public API access.
google_yuyin:
    language: 'zh-CN'
    api_key: ''

# 聊天机器人
# 可选值：
# tuling    - 图灵机器人
# emotibot  - 小影机器人
robot: tuling

# 图灵机器人
# http://www.tuling123.com
tuling:
    tuling_key: '填写你的图灵机器人API Key'

# 小影机器人
# http://botfactory.emotibot.com/
emotibot:
    appid: '填写你的 emotibot appid'
    active_mode: true  # 是否主动说更多点话

# 信号灯(可选)
# 将普通led接入树莓派GPIO， 唤醒后常亮，思考及说话时闪亮
signal_led:
    enable: false
    gpio_mode: "bcm" # "bcm" 或 "board"
    pin: 24 # led 正极接脚， 负极接GND

# 邮箱
# 如果使用网易邮箱，还需设置允许第三方客户端收发邮件
email:
    enable: true
    address: '你的邮箱地址'
    password: '你的邮箱密码'  # 如果是网易邮箱，须填写应用授权密码而不是登录密码！
    smtp_server: 'smtp.163.com'
    smtp_port: '25'  # 这里填写非SSL协议端口号
    imap_server: 'imap.163.com'
    imap_port: '143'  # 这里填写非SSL协议端口号


# 拍照
# 需接入摄像头才能使用
camera:
    enable: false
    dest_path: "/home/pi/camera" # 保存目录
    quality: 5            # 成像质量（0~100）
    vertical_flip: true     # 竖直翻转
    horizontal_flip: false  # 水平翻转
    count_down: 3           # 倒计时（秒），仅当开启倒计时时有效
    sendToUser: true        # 拍完照是否发送到邮箱/微信    
    sound: true             # 是否有拍照音效
    usb_camera: false       # 是否使用USB摄像头（默认是树莓派5MP摄像头）


#######################
# 第三方插件的配置
#######################

# 在这里放第三方插件的配置
# https://github.com/wzpan/dingdang-contrib
```
#### 修改唤醒词

如果使用的是 PocketSphinx

在 profile.yml 配置文件中修改 robot_name 和 robot_name_cn 配置项；
编写一个 keyword.txt 文件，包含至少两个名字的全拼：
DINGDANG
ROBOT
其中 ROBOT 替换为你需要的机器人名字的全拼。

到 [lmtool](http://www.speech.cs.cmu.edu/tools/lmtool-new.html) 里上传你刚刚创建的 keyword.txt 并编译成模型。
把得到的 .dic 文件和 .lm 文件分别重命名为 dictionary 和 languagemodel，替换 /home/pi/.dingdang/vocabularies/pocketsphinx-vocabulary/keyword 下的同名文件。
重新运行 dingdang ，看看新的唤醒词灵敏度如何。如果不理想，换成别的唤醒词.

