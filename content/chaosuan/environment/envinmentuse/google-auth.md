---
title: "google"
description: Test description
draft: false
enableToc: false
keyword: test
weight: 4
---

## Google Authenticator

###### Google身份验证器是一款TOTP与HOTP的两步验证软件令牌，此软件用于Google的认证服务。此项服务所使用的算法已列于 RFC 6238 和 RFC 4226 中。

###### Google身份验证器给予用户一个六位到八位的一次性密码用于进行登录Google或其他站点时的附加验证。其同样可以给第三方应用生成口令，例如密码管理员或网络硬盘。

***
#### 一、安装使用
1. 软件安装

    a. 二进制安装
    ```bash
    yum install -y epel-* mercurial autoconf automake libtool pam-devel

    yum install -y google-authenticator
    ```
    b. 编译安装
    ```bash
    yum install -y epel-* mercurial autoconf automake libtool pam-devel git 

    git clone https://github.com/google/google-authenticator-libpam.git

    cd google-authenticator-libpam-master/  #进入目录
    chmod +x bootstrap.sh  #设置可执行权限
    ./bootstrap.sh  
    ./configure
    make install 
    ln -s /usr/local/lib/security/pam_google_authenticator.so /usr/lib64/security/pam_google_authenticator.so
    ```

2. PAM配置
    ```bash
    vim /etc/pam.d/sshd
    auth required pam_google_authenticator.so
    # 或者
    echo   "auth       required     pam_google_authenticator.so" >>/etc/pam.d/sshd
    ```

3. SSH配置
    ```bash
    vim /etc/ssh/sshd_config
    # 将
    ChallengeResponseAuthentication no
    # 改为
    ChallengeResponseAuthentication yes
    # 或者
    sed -i 's/ChallengeResponseAuthentication no/ChallengeResponseAuthentication yes/g' /etc/ssh/sshd_config
    # 重启sshd服务
    systemctl restart sshd.service
    ```

4. google-authenticator运行配置

    总的来说，首先运行google-authenticator二进制程序，然后一路y下来即可，中间使用二维码，或者密钥与手机端进行绑定，下方是具体过程。
    ```bash
    google-authenticator
    Do you want authentication tokens to be time-based (y/n) y
    Warning: pasting the following URL into your browser exposes the OTP secret to Google:
    https://www.google.com/chart?chs=200x200&chld=M|0&cht=qr&chl=otpauth://totp/root@demo%3Fsecret%3DXQ2WB526GLPJ7SI64Z3RZISOEE%26issuer%3Ddemo
                                                            
                                                                                    
                                                                                    
                                                                                    
                这里会有一个二维码，需要在手机上下载`googleauthenticator`APP扫码绑定
                安卓 IOS手机都可以在应用商店搜索安装
                                                                                    
                                                                                    
                                                                                                                                                                        
    Your new secret key is: XQ2WB526GLPJ7SI64Z3RZISOEE
    Your verification code is 917990
    Your emergency scratch codes are:
    42623319
    72314571
    14476695
    95764389
    38976136

    Do you want me to update your "/root/.google_authenticator" file? (y/n) y

    Do you want to disallow multiple uses of the same authentication
    token? This restricts you to one login about every 30s, but it increases
    your chances to notice or even prevent man-in-the-middle attacks (y/n) y

    By default, a new token is generated every 30 seconds by the mobile app.
    In order to compensate for possible time-skew between the client and the server,
    we allow an extra token before and after the current time. This allows for a
    time skew of up to 30 seconds between authentication server and client. If you
    experience problems with poor time synchronization, you can increase the window
    from its default size of 3 permitted codes (one previous code, the current
    code, the next code) to 17 permitted codes (the 8 previous codes, the current
    code, and the 8 next codes). This will permit for a time skew of up to 4 minutes
    between client and server.
    Do you want to do so? (y/n) y

    If the computer that you are logging into isn't hardened against brute-force
    login attempts, you can enable rate-limiting for the authentication module.
    By default, this limits attackers to no more than 3 login attempts every 30s.
    Do you want to enable rate-limiting? (y/n) y
    ```

5. 登录

    a. xshell
    ![svg](../../0-image/xshell-google-au.png)
