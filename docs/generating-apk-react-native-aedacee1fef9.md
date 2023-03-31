# 生成 APK —反应自然

> 原文：<https://medium.com/analytics-vidhya/generating-apk-react-native-aedacee1fef9?source=collection_archive---------8----------------------->

我们将使用 React Native CLI 来生成 apk。

首先，我们需要确保项目成功运行，没有任何错误。然后，生成私有签名密钥，为 React 本机应用程序生成 apk。

**第一步:**

我们可以使用项目目录上的 keytool 生成一个私有签名密钥，如下所示:

```
**sudo keytool -genkey -v -keystore my-upload-key.keystore -alias my-key-alias -keyalg RSA -keysize 2048 -validity 10000**
```

我们可以把 ***your_key_name*** 改成任何我们想要的名字，还有 ***your_key_alias*** 。该命令通过以下方式提示我们:

```
**Enter your keystore password: 12345****Re-enter new password: 12345****What is your first and last name? [unknown]: Samir Khanal****What is the name of your organizational unit? [unknown]: XYZ****What is the name of your organization? [unknown]: XYZ****What is the name of your city or Locality? [unknown]: XYZ****What is the name of your State or Province? [unknown]: XYZ****What is the two-letter country code for this unit? [unknown]: XX**
```

它使用上述凭证生成一个名为 my-release-key.keystore 的文件。

**第二步:**

现在，让我们将上面生成的文件(**my-release-key . keystore**)复制到项目文件夹中的 **android/app** 目录。

**第三步:**

我们应该编辑文件**Android/gradle . properties**并添加以下内容:

```
**MYAPP_UPLOAD_STORE_FILE=my-upload-key.keystore****MYAPP_UPLOAD_KEY_ALIAS=my-key-alias****MYAPP_UPLOAD_STORE_PASSWORD=*********MYAPP_UPLOAD_KEY_PASSWORD=*******
```

我们应该用之前设置的正确的密钥库密码替换*** * * * * ***。

**步骤 4:**

现在，我们应该在我们的项目文件夹中编辑**Android/app/build . gradle**文件，并在 **buildTypes** 的最后一行 **defaultConfig{ … }** 和“**signing config signing configs . release**之后添加 **signingConfigs** ，如下所示。

```
**...****android {** **...** **defaultConfig { ... }** **signingConfigs {** **release {** **if (project.hasProperty('MYAPP_UPLOAD_STORE_FILE')) {** **storeFile file(MYAPP_UPLOAD_STORE_FILE)** **storePassword MYAPP_UPLOAD_STORE_PASSWORD** **keyAlias MYAPP_UPLOAD_KEY_ALIAS** **keyPassword MYAPP_UPLOAD_KEY_PASSWORD** **}** **}** **}** **buildTypes {** **release {** **...** **signingConfig signingConfigs.release** **}** **}****}****...**
```

现在，进入我们项目的 **android/** 目录并运行，

```
**./gradlew assembleRelease**
```

这将在**Android/app/build/outputs/apk/release/**目录下生成一个 **apk** 文件。

这样，我们得到了 React 原生项目的一个 **apk** 文件，我们可以在 google play-store 中发布或与其他人共享。

**参考文献:**

[***https://reactnative.dev/docs/signed-apk-android***](https://reactnative.dev/docs/signed-apk-android)