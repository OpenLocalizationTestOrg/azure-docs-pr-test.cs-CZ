---
title: "aaaAzure AD v2 Windows Desktop Začínáme – Úvod | Microsoft Docs"
description: "Jak aplikace Windows Desktop .NET (XAML) můžete volat rozhraní API, které vyžadují přístupové tokeny bodem v2 Azure Active Directory"
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
ms.openlocfilehash: 89d98fc46190ba9e47b7c3095f91e32eca455fcc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="call-hello-microsoft-graph-api-from-a-windows-desktop-app"></a>Volání hello Microsoft Graph API z aplikace Windows Desktop

Tato příručka ukazuje, jak získat přístupový token a volání Microsoft Graph API nebo jiná rozhraní API, které vyžadují přístupové tokeny z koncového bodu Azure Active Directory v2 nativní aplikace Windows Desktop .NET (XAML).

Na konci hello tohoto průvodce, vaše aplikace bude být schopný toocall chráněné API pomocí osobní účty (včetně live.com, outlook.com a dalších) a také pracovní a školní účty v jakémkoli společnosti nebo organizace, která má Azure Active Directory.  

> Tato příručka vyžaduje Visual Studio 2015 Update 3 nebo Visual Studio 2017.  Nemáte ho? [Stáhněte si Visual Studio 2017 zdarma](https://www.visualstudio.com/downloads/)

### <a name="how-this-guide-works"></a>Jak funguje tato příručka

![Jak funguje tato příručka](media/active-directory-mobileanddesktopapp-windowsdesktop-intro/windesktophowitworks.png)

Hello ukázkové aplikace vytvořené v této příručce umožňuje aplikace Windows Desktop tooquery Microsoft Graph API nebo webového rozhraní API, které přijímá tokeny z koncového bodu v2 Azure Active Directory. V tomto scénáři se přidá token tooHTTP požadavky prostřednictvím hello autorizační hlavičky. Získání tokenu a obnova zpracovává hello Microsoft ověřování knihovny (MSAL).


### <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Zpracování získání tokenu pro přístup k chráněné webové rozhraní API

Po ověření uživatele hello hello ukázkovou aplikaci přijme token, který lze použít tooquery Microsoft Graph API nebo webového rozhraní API zabezpečeny v2 Microsoft Azure Active Directory.

Rozhraní API, jako je Microsoft Graph vyžadují k přístupu tooallow tokenu přístupu konkrétním prostředkům – například tooread uživatelského profilu, přístup k kalendáře uživatele nebo e-mailovou zprávu. Aplikace může požadovat token přístupu pomocí MSAL tooaccess tyto prostředky tak, že zadáte obory rozhraní API. Tento přístupový token to přidané toohello HTTP autorizační hlavičky pro každé volání proti hello chráněný prostředek. 

MSAL spravuje ukládání do mezipaměti a aktualizaci přístupových tokenů, aby vaše aplikace nemusí.


### <a name="nuget-packages"></a>Balíčky NuGet

Tato příručka používá následující balíčky NuGet hello:

|Knihovna|Popis|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Knihovna Microsoft ověřování (MSAL)|

