
---
title: 在服务端生成 Token
description: 
platform: Java
updatedAt: Mon Jul 06 2020 07:36:32 GMT+0800 (CST)
---
# 在服务端生成 Token
本页为 Agora Native SDK v2.1+、Agora Web SDK v2.4+、Agora Recording SDK v2.1+ 以及 Agora RTSA SDK  的用户演示如何使用我们提供的 Demo 快速生成一个 RTC Token，并提供 Token 生成相关的 Java API 参考。

## Token 代码仓库说明

你需要在业务服务器自行部署 Token 生成器生成 Token。我们的 [GitHub 开源仓库](https://github.com/AgoraIO/Tools/tree/master/DynamicKey/AgoraDynamicKey) 为你提供了 Token 生成源码以及使用这些源码生成 Token 的简单示例。我们目前支持 C++、Java、Python、PHP、Ruby、Node.js、Go、C# 等语言。

[GitHub 开源仓库](https://github.com/AgoraIO/Tools/tree/master/DynamicKey/AgoraDynamicKey) 的 <b>./\<language\>/src</b> 文件夹下包含生成各种版本的 Dynamic key 和 Token 的源码。其中：**AccessToken** 和 **RtcTokenBuilder** 用于为以下 SDK 生成 Token：

- Agora Native SDK (Java, Objective-C, C++, Electron) v2.1+
- Agora Web SDK v2.4+
- Agora Recording SDK v2.1+ 
- Agora RTSA SDK

我们推荐使用 **RtcTokenBuilder** 而不是 **AccessToken** 生成 Token。**AccessToken** 实现了底层的核心算法，**RtcTokenBuilder** 实际上对 **AccessToken** 又进行了一层封装，提供了更为简化易懂的 Token 生成接口。

开源仓库的 **./\<language\>/sample** 文件夹下包含用于演示 Token 生成的示例代码。其中， **RtcTokenBuilderSample** 是我们基于 **RtcTokenBuilder** 编写的一个简单的 Token 生成器示例程序。你可以根据自己的业务逻辑对我们的示例程序做相应调整。

## 快速生成 RTC Token

<div class="alert note">体验该示例项目需要 JDK 1.8 或以上环境。</div>

下面我们以 **RtcTokenBuilderSample.java** 为例演示 Token 生成的过程：

1. 将 GitHub 仓库同步到本地。
2.  将 **/DynamicKey/AgoraDynamicKey/java** 作为项目导入到你的 Eclipse IDE 中。
3. 打开 **/java/src/io/agora/sample/RtcTokenBuilderSample.java** 。
4. 用你自己的 App ID、App 证书、用户 ID 以及 Channel Name 替换示例代码中的伪码。关于如何获取 App ID 和 App 证书，详见[校验用户权限](https://docs.agora.io/cn/Agora%20Platform/token?platform=All%20Platforms#app-id)。
    - 如果你使用 int 型 uid 加入频道，请注释掉以下代码段：
```Java
        String result = token.buildTokenWithUserAccount(appId, appCertificate,  
        		 channelName, userAccount, Role.Role_Publisher, timestamp);
        System.out.println(result);
```
    - 如果你使用 string 型 userAccount 加入频道，请注释掉以下代码段：
```Java
        String result = token.buildTokenWithUid(appId, appCertificate,  
       		 channelName, uid, Role.Role_Publisher, timestamp);
        System.out.println(result);
```
5. 　鼠标右击 **/java/src/io/agora/sample/RtcTokenBuilderSample.java** 并选择 **Run as a Java application**。新生成的 RTC Token 会在你的本地终端显示。


## API 参考

你可以通过调用 **RtcTokenBuilder.java** 提供的公开方法创建自己的 Token 生成器。请注意，**RtcTokenBuilder.java** 既支持 int 型 uid 也支持 string 型 userAccount，请根据需要选择合适的生成方法。


### buildTokenWithUid

```Java
   public String buildTokenWithUid(String appId, String appCertificate, 
    		String channelName, int uid, Role role, int privilegeTs)
```

该方法支持用 int 型 uid 生成 Token。

| **参数**    | **描述**                                              |
| ---------------- | ------------------------------------------------------------ |
| `appId`          | 你的项目 ID。详见[获取 App ID](https://docs.agora.io/cn/Agora%20Platform/token/#app-id)。| 
| `appCertificate` | 你的项目 App 证书。详见[开启 App 证书](https://docs.agora.io/cn/Agora%20Platform/token?platform=All%20Platforms#app-certificate)。|
| `channelName`    | 标识通话的频道名称，长度在64字节以内的字符串。以下为支持的字符集范围（共89个字符）: <li>26 个小写英文字母 a-z<li>26 个大写英文字母 A-Z<li>10 个数字 0-9<li>空格<li>"!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "*_", " {", "}", "\|", "~", "," |
| `uid`            | 用户 ID，32 位无符号整数。建议设置范围：1 到 (2<sup>32</sup>-1)，并保证唯一性。 |
| `role`          | 用户角色。暂时只支持一种角色，请使用默认值 `Role_Publisher = 1`。 |
| `privilegeTs`     | Token 有效时间戳，格式为自 1970 年 1 月 1 日零时起经过的秒数。比如，如果你将 `privilegeExpiredTs` 设为当前时间戳再加 600 秒，那么 Token 会在生成 10 分钟后过期。Token 的最大有效期为 24 小时。如果你设为 0，或超过 24 小时，则 Token 有效期依然是 24 小时。 |

### buildTokenWithUserAccount

```Java
  public String buildTokenWithUserAccount(String appId, String appCertificate, 
    		String channelName, String account, Role role, int privilegeTs)
```

该方法支持用 string 型 userAccount 生成 Token。

| **参数**    | **描述**                                             |
| ---------------- | ------------------------------------------------------------ |
| `appId`          | 你的项目 ID。详见[获取 App ID](https://docs.agora.io/cn/Agora%20Platform/token/#app-id)。| 
| `appCertificate` | 你的项目 App 证书。详见[开启 App 证书](https://docs.agora.io/cn/Agora%20Platform/token?platform=All%20Platforms#app-certificate)。|
| `channelName`    | 标识通话的频道名称，长度在64字节以内的字符串。以下为支持的字符集范围（共89个字符）: <li>26 个小写英文字母 a-z<li>26 个大写英文字母 A-Z<li>10 个数字 0-9<li>空格<li>"!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "*_", " {", "}", "\|", "~", "," |
|`userAccount` | 用户 User Account。该参数为必需，最大不超过 255 字节，不可为 null。请确保加入频道的 User Account 的唯一性。 以下为支持的字符集范围（共 89 个字符）：<li>26 个小写英文字母 a-z<li>26 个大写英文字母 A-Z<li>10 个数字 0-9<li>空格<li>"!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "*_", " {", "}", "\|", "~", "," |
| `role`          | 用户角色。暂时只支持一种角色，请使用默认值 `Role_Publisher = 1`。 |
| `privilegeTs`     | Token 有效时间戳，格式为自 1970 年 1 月 1 日零时起经过的秒数。比如，如果你将 `privilegeExpiredTs` 设为当前时间戳再加 600 秒，那么 Token 会在生成 10 分钟后过期。Token 的最大有效期为 24 小时。如果你设为 0，或超过 24 小时，则 Token 有效期依然是 24 小时。 |
	
	
## 相关链接
	
生成 Token 过程中，你也可以参考如下文档：

- [如何处理 RTC SDK 中 Token 相关错误码？](https://docs.agora.io/cn/faq/token_error)
- [如何处理云端录制 101 错误码？](https://docs.agora.io/cn/faq/101_error)
