---
title: "aaaAzure AD v2 Android Začínáme – Úvod | Microsoft Docs"
description: "Jak získat přístupový token a volání rozhraní API, které vyžadují přístupové tokeny z koncového bodu Azure Active Directory v2 nebo Microsoft Graph API aplikace pro Android"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 7c76ab8bbf1a6c934ab672cccf85ae82f03f601a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="call-hello-microsoft-graph-api-from-an-android-app"></a>Volání hello Microsoft Graph API z aplikace pro Android

Tato příručka ukazuje, jak získat přístupový token a volání Microsoft Graph API nebo jiná rozhraní API, které vyžadují přístupové tokeny z koncového bodu Azure Active Directory v2 nativní aplikace pro Android.

Na konci hello tohoto průvodce, vaše aplikace bude být schopný toocall chráněné API pomocí osobní účty (včetně live.com, outlook.com a dalších) a také pracovní a školní účty v jakémkoli společnosti nebo organizace, která má Azure Active Directory.  

### <a name="how-this-sample-works"></a>Fungování této ukázky
![Fungování této ukázky](media/active-directory-mobileanddesktopapp-android-intro/android-intro.png)

Ukázka Hello vytvořit v této příručce je založena na scénář, kde aplikace pro Android je použité tooquery webového rozhraní API, které přijímá tokeny z Azure Active Directory v2 koncového bodu – v tomto případě Microsoft Graph API. V tomto scénáři se přidá token tooHTTP požadavky prostřednictvím hello autorizační hlavičky. Získání tokenu a obnova zpracovává hello Microsoft ověřování knihovny (MSAL).

### <a name="pre-requisites"></a>Požadavky
* Tuto s průvodcem instalací se zaměřuje na Android Studio, ale žádné jiné vývojové prostředí Android aplikace je také přijatelné. 
* Je vyžadován Android SDK 21 nebo novější (SDK 25 je doporučeno).
* Google Chrome nebo pomocí webového prohlížeče pomocí vlastní karty je požadovaný pro tuto verzi systému Microsoft ověřování knihovny (MSAL) pro Android.

> Poznámka: Google Chrome není součástí Visual Studio Emulator for Android. Doporučujeme tootest tento kód na emulátoru s 25 rozhraní API nebo bitovou kopii s 21 rozhraní API nebo novější, která má s Google Chrome nainstalovanou.


### <a name="how-toohandle-token-acquisition-tooaccess-a-protected-web-api"></a>Jak toohandle token pořízení tooaccess chráněné webového rozhraní API

Po ověření uživatele hello hello ukázkovou aplikaci přijme token, který lze použít tooquery Microsoft Graph API nebo webového rozhraní API zabezpečeny v2 Microsoft Azure Active Directory.

Rozhraní API, jako je Microsoft Graph vyžadují k přístupu tooallow tokenu přístupu konkrétním prostředkům – například tooread uživatelského profilu, přístup k kalendáře uživatele nebo e-mailovou zprávu. Aplikace může požadovat token přístupu pomocí MSAL tooaccess tyto prostředky tak, že zadáte obory rozhraní API. Tento přístupový token to přidané toohello HTTP autorizační hlavičky pro každé volání proti hello chráněný prostředek. 

MSAL spravuje ukládání do mezipaměti a aktualizaci přístupových tokenů, aby vaše aplikace nemusí.

### <a name="libraries"></a>Knihovny

Tato příručka používá hello následující knihovny:

|Knihovna|Popis|
|---|---|
|[com.microsoft.identity.Client](http://javadoc.io/doc/com.microsoft.identity.client/msal)|Knihovna Microsoft ověřování (MSAL)|
