---
title: "iOS v2 aaaAzure AD Začínáme – Úvod | Microsoft Docs"
description: "Jak aplikace pro iOS (Swift) můžete volat rozhraní API, které vyžadují přístupové tokeny bodem v2 Azure Active Directory"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.openlocfilehash: f40aebbb75490912e533aecc7eedfb2b2dcd8c6c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="call-hello-microsoft-graph-api-from-an-ios-app"></a>Volání hello Microsoft Graph API z aplikace pro iOS

Tato příručka ukazuje, jak získat přístupový token a volání hello Microsoft Graph API nebo jiná rozhraní API, které vyžadují přístupové tokeny z koncového bodu Azure Active Directory v2 aplikace nativní aplikace pro iOS (Swift).

Na konci hello tohoto průvodce, vaše aplikace bude být schopný toocall chráněné API pomocí osobní účty (včetně live.com, outlook.com a dalších) a také pracovní a školní účty v jakémkoli společnosti nebo organizace, která má Azure Active Directory.

> ### <a name="pre-requisites"></a>Požadavky
> - XCode 8.x je vyžadována pro tento průvodce. Si můžete stáhnout XCode [odsud](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "XCode stáhnout URL").
> - [Carthage](https://github.com/Carthage/Carthage) pro správy balíčků

### <a name="how-this-guide-works"></a>Jak funguje tato příručka

![Jak funguje tato příručka](media/active-directory-mobileanddesktopapp-ios-introduction/iosintro.png)

Hello ukázkové aplikace vytvořené v této příručce umožňuje tooquery hello aplikace iOS Microsoft Graph API nebo webového rozhraní API, které přijímá tokeny z koncového bodu v2 Azure Active Directory. V tomto scénáři se přidá token tooHTTP požadavky prostřednictvím hello autorizační hlavičky. Získání tokenu a obnova zpracovává hello Microsoft ověřování knihovny (MSAL).


### <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Zpracování získání tokenu pro přístup k chráněné webové rozhraní API

Po ověření uživatele hello hello ukázkovou aplikaci přijme token, který se dá použít tooquery hello Microsoft Graph API nebo webového rozhraní API zabezpečeny v2 Microsoft Azure Active Directory.

Rozhraní API, jako je Microsoft Graph vyžadují k přístupu tooallow tokenu přístupu konkrétním prostředkům – například tooread uživatelského profilu, přístup k kalendáře uživatele nebo e-mailovou zprávu. Aplikace může požadovat token přístupu pomocí MSAL zadáním obory rozhraní API. Tento přístupový token to přidané toohello HTTP autorizační hlavičky pro každé volání proti hello chráněný prostředek.

MSAL spravuje ukládání do mezipaměti a aktualizaci přístupových tokenů, aby vaše aplikace nemusí.


### <a name="nuget-packages"></a>Balíčky NuGet

Tato příručka používá následující balíčky NuGet hello:

|Knihovna|Popis|
|---|---|
|[MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)|Microsoft ověřování knihovny Preview pro iOS|

