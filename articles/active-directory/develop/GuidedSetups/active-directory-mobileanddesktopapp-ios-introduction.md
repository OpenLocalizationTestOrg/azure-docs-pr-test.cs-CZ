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
# <a name="call-hello-microsoft-graph-api-from-an-ios-app"></a><span data-ttu-id="d775d-103">Volání hello Microsoft Graph API z aplikace pro iOS</span><span class="sxs-lookup"><span data-stu-id="d775d-103">Call hello Microsoft Graph API from an iOS app</span></span>

<span data-ttu-id="d775d-104">Tato příručka ukazuje, jak získat přístupový token a volání hello Microsoft Graph API nebo jiná rozhraní API, které vyžadují přístupové tokeny z koncového bodu Azure Active Directory v2 aplikace nativní aplikace pro iOS (Swift).</span><span class="sxs-lookup"><span data-stu-id="d775d-104">This guide demonstrates how a native iOS application (Swift) can get an access token and call hello Microsoft Graph API or other APIs that require access tokens from Azure Active Directory v2 endpoint.</span></span>

<span data-ttu-id="d775d-105">Na konci hello tohoto průvodce, vaše aplikace bude být schopný toocall chráněné API pomocí osobní účty (včetně live.com, outlook.com a dalších) a také pracovní a školní účty v jakémkoli společnosti nebo organizace, která má Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d775d-105">At hello end of this guide, your application will be able toocall a protected API using personal accounts (including outlook.com, live.com, and others) as well as work and school accounts from any company or organization that has Azure Active Directory.</span></span>

> ### <a name="pre-requisites"></a><span data-ttu-id="d775d-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d775d-106">Pre-requisites</span></span>
> - <span data-ttu-id="d775d-107">XCode 8.x je vyžadována pro tento průvodce.</span><span class="sxs-lookup"><span data-stu-id="d775d-107">XCode 8.x is required for this guide.</span></span> <span data-ttu-id="d775d-108">Si můžete stáhnout XCode [odsud](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "XCode stáhnout URL").</span><span class="sxs-lookup"><span data-stu-id="d775d-108">You can download XCode [from here](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "XCode Download URL").</span></span>
> - <span data-ttu-id="d775d-109">[Carthage](https://github.com/Carthage/Carthage) pro správy balíčků</span><span class="sxs-lookup"><span data-stu-id="d775d-109">[Carthage](https://github.com/Carthage/Carthage) for package management</span></span>

### <a name="how-this-guide-works"></a><span data-ttu-id="d775d-110">Jak funguje tato příručka</span><span class="sxs-lookup"><span data-stu-id="d775d-110">How this guide works</span></span>

![Jak funguje tato příručka](media/active-directory-mobileanddesktopapp-ios-introduction/iosintro.png)

<span data-ttu-id="d775d-112">Hello ukázkové aplikace vytvořené v této příručce umožňuje tooquery hello aplikace iOS Microsoft Graph API nebo webového rozhraní API, které přijímá tokeny z koncového bodu v2 Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d775d-112">hello sample application created by this guide enables an iOS application tooquery hello Microsoft Graph API or a Web API that accepts tokens from Azure Active Directory v2 endpoint.</span></span> <span data-ttu-id="d775d-113">V tomto scénáři se přidá token tooHTTP požadavky prostřednictvím hello autorizační hlavičky.</span><span class="sxs-lookup"><span data-stu-id="d775d-113">For this scenario, a token is added tooHTTP requests via hello Authorization header.</span></span> <span data-ttu-id="d775d-114">Získání tokenu a obnova zpracovává hello Microsoft ověřování knihovny (MSAL).</span><span class="sxs-lookup"><span data-stu-id="d775d-114">Token acquisition and renewal is handled by hello Microsoft Authentication Library (MSAL).</span></span>


### <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a><span data-ttu-id="d775d-115">Zpracování získání tokenu pro přístup k chráněné webové rozhraní API</span><span class="sxs-lookup"><span data-stu-id="d775d-115">Handling token acquisition for accessing protected Web APIs</span></span>

<span data-ttu-id="d775d-116">Po ověření uživatele hello hello ukázkovou aplikaci přijme token, který se dá použít tooquery hello Microsoft Graph API nebo webového rozhraní API zabezpečeny v2 Microsoft Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d775d-116">After hello user authenticates, hello sample application receives a token that can be used tooquery hello Microsoft Graph API or a Web API secured by Microsoft Azure Active Directory v2.</span></span>

<span data-ttu-id="d775d-117">Rozhraní API, jako je Microsoft Graph vyžadují k přístupu tooallow tokenu přístupu konkrétním prostředkům – například tooread uživatelského profilu, přístup k kalendáře uživatele nebo e-mailovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="d775d-117">APIs such as Microsoft Graph require an access token tooallow accessing specific resources – for example, tooread a user’s profile, access user’s calendar or send an email.</span></span> <span data-ttu-id="d775d-118">Aplikace může požadovat token přístupu pomocí MSAL zadáním obory rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d775d-118">Your application can request an access token using MSAL by specifying API scopes.</span></span> <span data-ttu-id="d775d-119">Tento přístupový token to přidané toohello HTTP autorizační hlavičky pro každé volání proti hello chráněný prostředek.</span><span class="sxs-lookup"><span data-stu-id="d775d-119">This access token is then added toohello HTTP Authorization header for every call made against hello protected resource.</span></span>

<span data-ttu-id="d775d-120">MSAL spravuje ukládání do mezipaměti a aktualizaci přístupových tokenů, aby vaše aplikace nemusí.</span><span class="sxs-lookup"><span data-stu-id="d775d-120">MSAL manages caching and refreshing access tokens for you, so your application doesn't need to.</span></span>


### <a name="nuget-packages"></a><span data-ttu-id="d775d-121">Balíčky NuGet</span><span class="sxs-lookup"><span data-stu-id="d775d-121">NuGet packages</span></span>

<span data-ttu-id="d775d-122">Tato příručka používá následující balíčky NuGet hello:</span><span class="sxs-lookup"><span data-stu-id="d775d-122">This guide uses hello following NuGet packages:</span></span>

|<span data-ttu-id="d775d-123">Knihovna</span><span class="sxs-lookup"><span data-stu-id="d775d-123">Library</span></span>|<span data-ttu-id="d775d-124">Popis</span><span class="sxs-lookup"><span data-stu-id="d775d-124">Description</span></span>|
|---|---|
|[<span data-ttu-id="d775d-125">MSAL.framework</span><span class="sxs-lookup"><span data-stu-id="d775d-125">MSAL.framework</span></span>](https://github.com/AzureAD/microsoft-authentication-library-for-objc)|<span data-ttu-id="d775d-126">Microsoft ověřování knihovny Preview pro iOS</span><span class="sxs-lookup"><span data-stu-id="d775d-126">Microsoft Authentication Library Preview for iOS</span></span>|

