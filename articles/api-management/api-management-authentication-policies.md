---
title: "zásady ověřování aaaAzure API Management | Microsoft Docs"
description: "Další informace o zásadách ověřování hello k dispozici pro použití v Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 061702a7-3a78-472b-a54a-f3b1e332490d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: ce93cced66cb67520e97c7c15f3685bffb08e1f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="api-management-authentication-policies"></a>Zásady ověřování služby API Management
Toto téma obsahuje odkaz pro hello následující zásady služby API Management. Informace o přidávání a konfiguraci zásad najdete v tématu [zásady ve službě API Management](http://go.microsoft.com/fwlink/?LinkID=398186).  
  
##  <a name="AuthenticationPolicies"></a>Zásady ověřování  
  
-   [Ověřování Basic](api-management-authentication-policies.md#Basic) -ověření pomocí back-end službu pomocí základního ověřování.  
  
-   [Ověřování pomocí certifikátu klienta](api-management-authentication-policies.md#ClientCertificate) -ověření pomocí back-end službu pomocí klientských certifikátů.  
  
##  <a name="Basic"></a>Ověřování Basic  
 Použití hello `authentication-basic` zásad tooauthenticate s back-end službu pomocí základního ověřování. Tato zásada nastavuje toohello záhlaví HTTP autorizace hello odpovídající údaje toohello hodnota poskytnutá v zásadě hello.  
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
```xml  
<authentication-basic username="username" password="password" />  
```  
  
### <a name="example"></a>Příklad  
  
```xml  
<authentication-basic username="testuser" password="testpassword" />  
```  
  
### <a name="elements"></a>Elementy  
  
|Name (Název)|Popis|Požaduje se|  
|----------|-----------------|--------------|  
|ověřování – základní|Kořenový element.|Ano|  
  
### <a name="attributes"></a>Atributy  
  
|Name (Název)|Popis|Požaduje se|Výchozí|  
|----------|-----------------|--------------|-------------|  
|uživatelské jméno|Určuje uživatelské jméno hello hello základní pověření.|Ano|Není k dispozici|  
|heslo|Určuje heslo hello hello základní pověření.|Ano|Není k dispozici|  
  
### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** příchozí  
  
-   **Zásady obory:** rozhraní API  
  
##  <a name="ClientCertificate"></a>Ověřování pomocí certifikátu klienta  
 Použití hello `authentication-certificate` zásad tooauthenticate službě back-end pomocí klientského certifikátu. certifikát Hello musí toobe [nainstalován do rozhraní API správy](http://go.microsoft.com/fwlink/?LinkID=511599) první a je určený podle jeho kryptografický otisk.  
  
### <a name="policy-statement"></a>Prohlášení o zásadách  
  
```xml  
<authentication-certificate thumbprint="thumbprint" />  
```  
  
### <a name="example"></a>Příklad  
  
```xml  
<authentication-certificate thumbprint="....." />  
```  
  
### <a name="elements"></a>Elementy  
  
|Name (Název)|Popis|Požaduje se|  
|----------|-----------------|--------------|  
|ověřovací certifikát|Kořenový element.|Ano|  
  
### <a name="attributes"></a>Atributy  
  
|Name (Název)|Popis|Požaduje se|Výchozí|  
|----------|-----------------|--------------|-------------|  
|Kryptografický otisk|Hello kryptografický otisk certifikátu klienta hello.|Ano|Není k dispozici|  
  
### <a name="usage"></a>Využití  
 Tuto zásadu lze použít v následujících zásad hello [části](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) a [obory](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Části zásady:** příchozí  
  
-   **Zásady obory:** rozhraní API  
  

## <a name="next-steps"></a>Další kroky
Práce se zásadami pro další informace najdete v tématu [zásady ve službě API Management](api-management-howto-policies.md).  