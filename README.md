安装方式一：
Liunx
```
yum -y install epel-release
yum -y install google-authenticator 
```

ubuntu
```
sudo apt update
sudo add-apt-repository universe
sudo apt install libpam-google-authenticator
```
安装方式二：
Liunx
```
yum install -y git make gcc libtool pam-devel qrencode ntpdate
```
```
git clone https://github.com/google/google-authenticator-libpam.git
cd   google-authenticator-libpam/
./bootstrap.sh
./configure
make
make install
```
ubuntu
```
sudo apt-get  -y install autoconf  git make gcc libtool libpam0g-dev qrencode ntpdate
```
```
git clone https://github.com/google/google-authenticator-libpam.git
cd   google-authenticator-libpam/
./bootstrap.sh
./configure
sudo make
sudo make install
```
配置ssh
```
vim /etc/ssh/sshd_config    #ubuntu   sudo
```
修改如下的配置项：
```
ChallengeResponseAuthentication yes
UsePAM yes
```
配置PAM
```
vim /etc/pam.d/sshd      #ubuntu   sudo
```
Liunx
```
#%PAM-1.0
auth      required     pam_google_authenticator.so   #添加至第一行
```
ubuntu
```
auth      required      pam_google_authenticator.so       #末尾添加
```
重启ssh
```
systemctl restart sshd     #ubuntu   sudo
```
配置google authenticator
首要条件：先切换到你需要设置的帐号 
```
google-authenticator
```
```
Do you want authentication tokens to be time-based (y/n)  #基于时间生成身份验证
#已经安装qrencode会产生一个二维码，二维码连接也可以URL显示
Your new secret key is ：***********   # 密钥key
Your verification code is : #code 动态码
Your emergency scratch codes are:  #  生成5 个紧急救助码
Do you want me to update your "/root/.google_authenticator" file? (y/n)  #一直确认下去
# 生成了一个 .google_authenticator 文件

your chances to notice or even prevent man-in-the-middle attacks (y/n)  #一直确认下去
Do you want to do so? (y/n)   #一直确认下去
Do you want to enable rate-limiting? (y/n)  #设置完成

#上面的意思大概为：禁止多次使用相同的身份验证，限制每30秒登录一次，移动端每30秒更新一次，移动端和客户端时间误差30秒，30秒内不能超过3次登录。
``` 

如果需要删除一个用户的Google验证，删除这个用户下产生的`home/.google_authenticator`文件即可
