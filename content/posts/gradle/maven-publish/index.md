---
title: 使用Gradle发布到Maven中央仓库
date: 2020-03-21
tags:
  - Gradle
categories:
  - Gradle
  - Java
---

1. 注册`https://issues.sonatype.org/`账号
2. 使用 scoop 安装 gpg`scoop install gpg`
3. 使用 gpg 生成密钥对

   `gpg --full-generate-key`

   导出私钥

   `gpg --export-secret-keys -o secring.gpg`

   发布到 ubuntu 密钥库

   `gpg --keyserver keyserver.ubuntu.com --send-keys ***********`

   ```
      gpg --full-generate-key
      gpg (GnuPG) 2.2.20; Copyright (C) 2020 Free Software Foundation, Inc.
      This is free software: you are free to change and redistribute it.
      There is NO WARRANTY, to the extent permitted by law.

      请选择您要使用的密钥类型：
         (1) RSA 和 RSA （默认）
         (2) DSA 和 Elgamal
         (3) DSA（仅用于签名）
         (4) RSA（仅用于签名）
      RSA 密钥的长度应在 1024 位与 4096 位之间。
      您想要使用的密钥长度？(2048)
      请设定这个密钥的有效期限。
               0 = 密钥永不过期
            <n>  = 密钥在 n 天后过期
            <n>w = 密钥在 n 周后过期
            <n>m = 密钥在 n 月后过期
            <n>y = 密钥在 n 年后过期
      密钥永远不会过期
      这些内容正确吗？ (y/N) y

      GnuPG 需要构建用户标识以辨认您的密钥。

      真实姓名：
      电子邮件地址：
      注释：

      最后重复两次输入Passphase（这个在发布的时候需要，相当于密钥密码）
   ```

4. 配置 gradle 文件，这个看[官方文档](https://docs.gradle.org/current/userguide/publishing_maven.html)吧
