---
title: "生成我的第一个声明感知 WCF 服务 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: e0e6d091-9a97-4888-8f2c-cbcee42d90ee
caps.latest.revision: 7
author: "BrucePerlerMS"
ms.author: "bruceper"
manager: "mbaldwin"
caps.handback.revision: 7
---
# 生成我的第一个声明感知 WCF 服务
## 适用于  
  
-   Windows Identity Foundation \(WIF\)  
  
-   Windows Communication Foundation \(WCF\)  
  
## 概述  
 本主题概述了使用 WIF 生成声明感知 WCF 服务的方案。  声明感知 Web 服务方案中通常涉及三个参与者：Web 服务本身、最终用户和安全令牌服务 \(STS\)。  下图描述了此方案：  
  
 ![WIF 基本声明识别 WCF 服务](../../../docs/framework/security/media/wifbasicclaimsawarewcfservice.png "WIFBasicClaimsAwareWCFService")  
  
1.  WCF 服务客户端（有时称为代理）使用 WIF 将凭据发送到 STS，在成功完成身份验证后，STS 将向此代理颁发令牌。  
  
2.  代理会将 STS 颁发的令牌发送到 WCF 服务。  
  
3.  声明感知 WCF 服务将配置为信任此 STS 及其颁发的令牌。  声明感知 WCF 服务使用 WIF 验证此令牌并对其进行分析。  开发人员使用适当的 WIF API 和类型（例如**“ClaimsPrincipal”**）来满足应用程序的需要，如对其实现授权。  
  
 从 .NET 4.5 开始，WIF 便已成为 .NET Framework 包的一部分。  直接在框架中提供 WIF 类可在 .NET 平台中进一步集成基于声明的标识，从而更轻松地使用声明。  如果使用 WIF 4.5，则无需安装任何带外组件即可开始开发声明感知 Web 应用程序。  WIF 类现在分布在各种程序集中，主要为 System.Security.Claims、System.IdentityModel 和 System.IdentityModel.Services。  
  
 STS 是一项在身份验证成功后颁发令牌的服务。  Microsoft 提供两个行业标准 STS：  
  
-   [Active Directory 联合身份验证服务 \(AD FS\) 2.0](http://go.microsoft.com/fwlink/?LinkID=247516) \(http:\/\/go.microsoft.com\/fwlink\/?LinkID\=247516\)  
  
-   [Microsoft Azure 访问控制服务 \(ACS\)](http://go.microsoft.com/fwlink/?LinkID=247517) \(http:\/\/go.microsoft.com\/fwlink\/?LinkID\=247517\)。  
  
 AD FS 2.0 是 Windows Server R2 的一部分并可用作本地方案的 STS。  Azure Active Directory 访问控制（也称为访问控制服务或 ACS）是作为 Microsoft Azure 的一部分提供的云服务。  出于测试或教学目的，您还可以使用其他 STS 以生成声明感知应用程序。  例如，你可以使用作为免费联机提供的[适用于 Visual Studio 的标识和访问工具](http://go.microsoft.com/fwlink/?LinkID=245849) \(http:\/\/go.microsoft.com\/fwlink\/?LinkID\=245849\) 一部分的本地开发 STS。  
  
 若要使用 WIF 生成第一个声明感知 WCF 服务，请参阅[How To: Build Claims\-Aware WCF Service Using WIF](http://msdn.microsoft.com/zh-cn/431e6415-62ed-4a9f-af03-f14d2b4dfe6d)。  
  
## 请参阅  
 [WIF 入门](../../../docs/framework/security/getting-started-with-wif.md)