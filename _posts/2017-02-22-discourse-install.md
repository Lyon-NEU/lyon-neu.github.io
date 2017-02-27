---
layout: post
title: "Discourse Install"
description: ""
category: 
tags: []
---
{% include JB/setup %}

## Discourse Install
<hr>

1. Create a /var/discourse folder, clone the Official Discourse Docker Image into it:

```bash
sudo -s
mkdir /var/discourse
git clone https://github.com/discourse/discourse_docker.git /var/discourse
cd /var/discourse
```

You will need to be root through the rest of the setup and bootstrap process.

2. Edit Discourse Configuration

Launch the setup tool at

> ./discourse-setup
Answer the following questions when prompted:

```baseh
Hostname for your Discourse? [discourse.example.com]: 
Email address for admin account? [me@example.com]: 
SMTP server address? [smtp.example.com]: 
SMTP user name? [postmaster@discourse.example.com]: 
SMTP port [587]:
SMTP password? []: 
```

This will generate an app.yml configuration file on your behalf, and then kicks off bootstrap. Bootstrapping takes between 2-8 minutes to set up your Discourse. If you need to change or fix these settings after bootstrapping, edit your /containers/app.yml file and ./launcher rebuild app, otherwise your changes will not take effect.

3. Start Discourse

Once bootstrapping is complete, your Discourse should be accessible in your web browser via the domain name `discourse.example.com` you entered earlier, provided you configured DNS. If not, you can visit the server IP directly, e.g. `http://192.168.1.1.` 

### 安装过程中出现的问题

- 由于我是在Ubuntu 16.04的虚拟机上安装测试的，所以真实环境可能有所不同
- 整个安装过程需要的时间比较长，而且由于需要访问*buby*的镜像，所以很有可能安装失败，网上有建议修改配置文件，加入几个模板，但我尝试几次也失败。过了几天，我删除原先的文件，重新安装的时候，居然一次通过。
- 关于配置，在初次安装的时候，它会提示你依次输入各个参数，需要根据实际情况填写，由于没有主机名，我有随便添写了localhost,不过可以直接通过ip地址来访问。邮箱我注册的是企er的，配置好邮箱名和端口号即可
- 系统编译完成后会自动运行程序，等到提示完成后打开浏览器，输入localhost，便看到成功标志。

[Discourse GitHub](https://github.com/discourse/discourse/blob/master/docs/INSTALL-cloud.md)