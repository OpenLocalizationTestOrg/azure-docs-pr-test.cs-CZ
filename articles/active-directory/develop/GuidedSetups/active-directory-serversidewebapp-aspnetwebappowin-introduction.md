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
# <a name="add-sign-in-with-microsoft-tooan-aspnet-web-app"></a><span data-ttu-id="3b4c0-103">Přidání přihlášení s webové aplikace ASP.NET tooan Microsoft</span><span class="sxs-lookup"><span data-stu-id="3b4c0-103">Add sign-in with Microsoft tooan ASP.NET web app</span></span>

<span data-ttu-id="3b4c0-104">Tato příručka ukazuje, jak tooimplement přihlásit se pomocí rozhraní ASP.NET MVC řešení s tradiční webové aplikace založené na prohlížeči pomocí OpenID Connect společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="3b4c0-104">This guide demonstrates how tooimplement sign-in with Microsoft using an ASP.NET MVC solution with a traditional web browser-based application using OpenID Connect.</span></span> 

<span data-ttu-id="3b4c0-105">Na konci hello tohoto průvodce bude vaše aplikace možné tooaccept sign in osobní účty (včetně live.com, outlook.com a dalších) a také pracovní a školní účty z jakékoli společnosti nebo organizace, která má integrované s Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3b4c0-105">At hello end of this guide, your application will be able tooaccept sign ins of personal accounts (including outlook.com, live.com, and others) as well as work and school accounts from any company or organization that has integrated with Azure Active Directory.</span></span> 

> <span data-ttu-id="3b4c0-106">Tato příručka vyžaduje Visual Studio 2015 Update 3 nebo Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="3b4c0-106">This guide requires Visual Studio 2015 Update 3 or Visual Studio 2017.</span></span>  <span data-ttu-id="3b4c0-107">Nemáte ho?</span><span class="sxs-lookup"><span data-stu-id="3b4c0-107">Don’t have it?</span></span>  [<span data-ttu-id="3b4c0-108">Stáhněte si Visual Studio 2017 zdarma</span><span class="sxs-lookup"><span data-stu-id="3b4c0-108">Download Visual Studio 2017 for free</span></span>](https://www.visualstudio.com/downloads/)

## <a name="how-this-guide-works"></a><span data-ttu-id="3b4c0-109">Jak funguje tato příručka</span><span class="sxs-lookup"><span data-stu-id="3b4c0-109">How this guide works</span></span>

![Jak funguje tato příručka](media/active-directory-serversidewebapp-aspnetwebappowin-intro/aspnetbrowsergeneral.png)

<span data-ttu-id="3b4c0-111">Tato příručka je založena na hello scénář, kde prohlížeče přistupuje ke webovou stránku ASP.NET požaduje tooauthenticate uživatele prostřednictvím přihlášení tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3b4c0-111">This guide is based on hello scenario where a browser accesses an ASP.NET web site, requesting a user tooauthenticate via a sign-in button.</span></span> <span data-ttu-id="3b4c0-112">V tomto scénáři většinu hello pracovní toorender hello webové stránky dojde na straně serveru hello.</span><span class="sxs-lookup"><span data-stu-id="3b4c0-112">In this scenario, most of hello work toorender hello web page occurs on hello server side.</span></span>

## <a name="libraries"></a><span data-ttu-id="3b4c0-113">Knihovny</span><span class="sxs-lookup"><span data-stu-id="3b4c0-113">Libraries</span></span>

<span data-ttu-id="3b4c0-114">Tato příručka používá hello následující knihovny:</span><span class="sxs-lookup"><span data-stu-id="3b4c0-114">This guide uses hello following libraries:</span></span>

|<span data-ttu-id="3b4c0-115">Knihovna</span><span class="sxs-lookup"><span data-stu-id="3b4c0-115">Library</span></span>|<span data-ttu-id="3b4c0-116">Popis</span><span class="sxs-lookup"><span data-stu-id="3b4c0-116">Description</span></span>|
|---|---|
|[<span data-ttu-id="3b4c0-117">Microsoft.Owin.Security.OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="3b4c0-117">Microsoft.Owin.Security.OpenIdConnect</span></span>](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/)|<span data-ttu-id="3b4c0-118">Middleware, který umožňuje aplikaci toouse OpenIdConnect pro ověřování</span><span class="sxs-lookup"><span data-stu-id="3b4c0-118">Middleware that enables an application toouse OpenIdConnect for authentication</span></span>|
|[<span data-ttu-id="3b4c0-119">Microsoft.Owin.Security.Cookies</span><span class="sxs-lookup"><span data-stu-id="3b4c0-119">Microsoft.Owin.Security.Cookies</span></span>](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)|<span data-ttu-id="3b4c0-120">Middleware, který umožňuje aplikaci toomaintain uživatelské relace pomocí souborů cookie</span><span class="sxs-lookup"><span data-stu-id="3b4c0-120">Middleware that enables an application toomaintain user session using cookies</span></span>|
|[<span data-ttu-id="3b4c0-121">Microsoft.Owin.Host.SystemWeb</span><span class="sxs-lookup"><span data-stu-id="3b4c0-121">Microsoft.Owin.Host.SystemWeb</span></span>](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)|<span data-ttu-id="3b4c0-122">Umožňuje aplikacím pro OWIN toorun ve službě IIS pomocí kanálu požadavku ASP.NET hello</span><span class="sxs-lookup"><span data-stu-id="3b4c0-122">Enables OWIN-based applications toorun on IIS using hello ASP.NET request pipeline</span></span>|

