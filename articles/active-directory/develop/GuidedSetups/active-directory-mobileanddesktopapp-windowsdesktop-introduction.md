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
# <a name="call-hello-microsoft-graph-api-from-a-windows-desktop-app"></a><span data-ttu-id="9d1c0-103">Volání hello Microsoft Graph API z aplikace Windows Desktop</span><span class="sxs-lookup"><span data-stu-id="9d1c0-103">Call hello Microsoft Graph API from a Windows Desktop app</span></span>

<span data-ttu-id="9d1c0-104">Tato příručka ukazuje, jak získat přístupový token a volání Microsoft Graph API nebo jiná rozhraní API, které vyžadují přístupové tokeny z koncového bodu Azure Active Directory v2 nativní aplikace Windows Desktop .NET (XAML).</span><span class="sxs-lookup"><span data-stu-id="9d1c0-104">This guide demonstrates how a native Windows Desktop .NET (XAML) application can get an access token and call Microsoft Graph API or other APIs that require access tokens from Azure Active Directory v2 endpoint.</span></span>

<span data-ttu-id="9d1c0-105">Na konci hello tohoto průvodce, vaše aplikace bude být schopný toocall chráněné API pomocí osobní účty (včetně live.com, outlook.com a dalších) a také pracovní a školní účty v jakémkoli společnosti nebo organizace, která má Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="9d1c0-105">At hello end of this guide, your application will be able toocall a protected API using personal accounts (including outlook.com, live.com, and others) as well as work and school accounts from any company or organization that has Azure Active Directory.</span></span>  

> <span data-ttu-id="9d1c0-106">Tato příručka vyžaduje Visual Studio 2015 Update 3 nebo Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="9d1c0-106">This guide requires Visual Studio 2015 Update 3 or Visual Studio 2017.</span></span>  <span data-ttu-id="9d1c0-107">Nemáte ho?</span><span class="sxs-lookup"><span data-stu-id="9d1c0-107">Don’t have it?</span></span> [<span data-ttu-id="9d1c0-108">Stáhněte si Visual Studio 2017 zdarma</span><span class="sxs-lookup"><span data-stu-id="9d1c0-108">Download Visual Studio 2017 for free</span></span>](https://www.visualstudio.com/downloads/)

### <a name="how-this-guide-works"></a><span data-ttu-id="9d1c0-109">Jak funguje tato příručka</span><span class="sxs-lookup"><span data-stu-id="9d1c0-109">How this guide works</span></span>

![Jak funguje tato příručka](media/active-directory-mobileanddesktopapp-windowsdesktop-intro/windesktophowitworks.png)

<span data-ttu-id="9d1c0-111">Hello ukázkové aplikace vytvořené v této příručce umožňuje aplikace Windows Desktop tooquery Microsoft Graph API nebo webového rozhraní API, které přijímá tokeny z koncového bodu v2 Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="9d1c0-111">hello sample application created by this guide enables a Windows Desktop Application tooquery Microsoft Graph API or a Web API that accepts tokens from Azure Active Directory v2 endpoint.</span></span> <span data-ttu-id="9d1c0-112">V tomto scénáři se přidá token tooHTTP požadavky prostřednictvím hello autorizační hlavičky.</span><span class="sxs-lookup"><span data-stu-id="9d1c0-112">For this scenario, a token is added tooHTTP requests via hello Authorization header.</span></span> <span data-ttu-id="9d1c0-113">Získání tokenu a obnova zpracovává hello Microsoft ověřování knihovny (MSAL).</span><span class="sxs-lookup"><span data-stu-id="9d1c0-113">Token acquisition and renewal is handled by hello Microsoft Authentication Library (MSAL).</span></span>


### <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a><span data-ttu-id="9d1c0-114">Zpracování získání tokenu pro přístup k chráněné webové rozhraní API</span><span class="sxs-lookup"><span data-stu-id="9d1c0-114">Handling token acquisition for accessing protected Web APIs</span></span>

<span data-ttu-id="9d1c0-115">Po ověření uživatele hello hello ukázkovou aplikaci přijme token, který lze použít tooquery Microsoft Graph API nebo webového rozhraní API zabezpečeny v2 Microsoft Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="9d1c0-115">After hello user authenticates, hello sample application receives a token that can be used tooquery Microsoft Graph API or a Web API secured by Microsoft Azure Active Directory v2.</span></span>

<span data-ttu-id="9d1c0-116">Rozhraní API, jako je Microsoft Graph vyžadují k přístupu tooallow tokenu přístupu konkrétním prostředkům – například tooread uživatelského profilu, přístup k kalendáře uživatele nebo e-mailovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="9d1c0-116">APIs such as Microsoft Graph require an access token tooallow accessing specific resources – for example, tooread a user’s profile, access user’s calendar or send an email.</span></span> <span data-ttu-id="9d1c0-117">Aplikace může požadovat token přístupu pomocí MSAL tooaccess tyto prostředky tak, že zadáte obory rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="9d1c0-117">Your application can request an access token using MSAL tooaccess these resources by specifying API scopes.</span></span> <span data-ttu-id="9d1c0-118">Tento přístupový token to přidané toohello HTTP autorizační hlavičky pro každé volání proti hello chráněný prostředek.</span><span class="sxs-lookup"><span data-stu-id="9d1c0-118">This access token is then added toohello HTTP Authorization header for every call made against hello protected resource.</span></span> 

<span data-ttu-id="9d1c0-119">MSAL spravuje ukládání do mezipaměti a aktualizaci přístupových tokenů, aby vaše aplikace nemusí.</span><span class="sxs-lookup"><span data-stu-id="9d1c0-119">MSAL manages caching and refreshing access tokens for you, so your application doesn't need to.</span></span>


### <a name="nuget-packages"></a><span data-ttu-id="9d1c0-120">Balíčky NuGet</span><span class="sxs-lookup"><span data-stu-id="9d1c0-120">NuGet Packages</span></span>

<span data-ttu-id="9d1c0-121">Tato příručka používá následující balíčky NuGet hello:</span><span class="sxs-lookup"><span data-stu-id="9d1c0-121">This guide uses hello following NuGet packages:</span></span>

|<span data-ttu-id="9d1c0-122">Knihovna</span><span class="sxs-lookup"><span data-stu-id="9d1c0-122">Library</span></span>|<span data-ttu-id="9d1c0-123">Popis</span><span class="sxs-lookup"><span data-stu-id="9d1c0-123">Description</span></span>|
|---|---|
|[<span data-ttu-id="9d1c0-124">Microsoft.Identity.Client</span><span class="sxs-lookup"><span data-stu-id="9d1c0-124">Microsoft.Identity.Client</span></span>](https://www.nuget.org/packages/Microsoft.Identity.Client)|<span data-ttu-id="9d1c0-125">Knihovna Microsoft ověřování (MSAL)</span><span class="sxs-lookup"><span data-stu-id="9d1c0-125">Microsoft Authentication Library (MSAL)</span></span>|

