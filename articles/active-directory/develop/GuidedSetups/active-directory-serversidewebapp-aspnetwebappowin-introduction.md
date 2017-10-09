---
title: "aaaAzure AD v2 ASP.NET Web Server Začínáme – Úvod | Microsoft Docs"
description: "Implementace přihlašování společnosti Microsoft na řešení technologie ASP.NET s tradiční webovou aplikací využívajících prohlížeč pomocí OpenID Connect standard"
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
ms.openlocfilehash: d6449926af2bdad24cbc8e91f74885a08f909103
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-sign-in-with-microsoft-tooan-aspnet-web-app"></a>Přidání přihlášení s webové aplikace ASP.NET tooan Microsoft

Tato příručka ukazuje, jak tooimplement přihlásit se pomocí rozhraní ASP.NET MVC řešení s tradiční webové aplikace založené na prohlížeči pomocí OpenID Connect společností Microsoft. 

Na konci hello tohoto průvodce bude vaše aplikace možné tooaccept sign in osobní účty (včetně live.com, outlook.com a dalších) a také pracovní a školní účty z jakékoli společnosti nebo organizace, která má integrované s Azure Active Directory. 

> Tato příručka vyžaduje Visual Studio 2015 Update 3 nebo Visual Studio 2017.  Nemáte ho?  [Stáhněte si Visual Studio 2017 zdarma](https://www.visualstudio.com/downloads/)

## <a name="how-this-guide-works"></a>Jak funguje tato příručka

![Jak funguje tato příručka](media/active-directory-serversidewebapp-aspnetwebappowin-intro/aspnetbrowsergeneral.png)

Tato příručka je založena na hello scénář, kde prohlížeče přistupuje ke webovou stránku ASP.NET požaduje tooauthenticate uživatele prostřednictvím přihlášení tlačítko. V tomto scénáři většinu hello pracovní toorender hello webové stránky dojde na straně serveru hello.

## <a name="libraries"></a>Knihovny

Tato příručka používá hello následující knihovny:

|Knihovna|Popis|
|---|---|
|[Microsoft.Owin.Security.OpenIdConnect](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/)|Middleware, který umožňuje aplikaci toouse OpenIdConnect pro ověřování|
|[Microsoft.Owin.Security.Cookies](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)|Middleware, který umožňuje aplikaci toomaintain uživatelské relace pomocí souborů cookie|
|[Microsoft.Owin.Host.SystemWeb](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)|Umožňuje aplikacím pro OWIN toorun ve službě IIS pomocí kanálu požadavku ASP.NET hello|

