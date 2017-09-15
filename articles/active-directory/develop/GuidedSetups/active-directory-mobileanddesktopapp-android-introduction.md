---
title: "Azure AD v2 Android získávání spuštěno – Úvod | Microsoft Docs"
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
ms.openlocfilehash: d933781713456f73aa76db557fdf35672dfb2a29
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="call-the-microsoft-graph-api-from-an-android-app"></a><span data-ttu-id="f727f-103">Volání Microsoft Graph API z aplikace pro Android</span><span class="sxs-lookup"><span data-stu-id="f727f-103">Call the Microsoft Graph API from an Android app</span></span>

<span data-ttu-id="f727f-104">Tato příručka ukazuje, jak získat přístupový token a volání Microsoft Graph API nebo jiná rozhraní API, které vyžadují přístupové tokeny z koncového bodu Azure Active Directory v2 nativní aplikace pro Android.</span><span class="sxs-lookup"><span data-stu-id="f727f-104">This guide demonstrates how a native Android application can get an access token and call Microsoft Graph API or other APIs that require access tokens from Azure Active Directory v2 endpoint.</span></span>

<span data-ttu-id="f727f-105">Na konci tohoto průvodce bude moci volat chráněné API používání osobních účtů (včetně live.com, outlook.com a dalších) a také pracovní a školní účty z jakékoli společnosti nebo organizace, která má Azure Active Directory vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="f727f-105">At the end of this guide, your application will be able to call a protected API using personal accounts (including outlook.com, live.com, and others) as well as work and school accounts from any company or organization that has Azure Active Directory.</span></span>  

### <a name="how-this-sample-works"></a><span data-ttu-id="f727f-106">Fungování této ukázky</span><span class="sxs-lookup"><span data-stu-id="f727f-106">How this sample works</span></span>
![Fungování této ukázky](media/active-directory-mobileanddesktopapp-android-intro/android-intro.png)

<span data-ttu-id="f727f-108">Ukázka vytvořit v této příručce je založena na scénář, kde aplikace pro Android se používá k dotazování webové rozhraní API, které přijímá tokeny z Azure Active Directory v2 koncového bodu – v tomto případě Microsoft Graph API.</span><span class="sxs-lookup"><span data-stu-id="f727f-108">The sample created by this guide is based on a scenario where an Android application is used to query a Web API that accepts tokens from Azure Active Directory v2 endpoint – in this case, Microsoft Graph API.</span></span> <span data-ttu-id="f727f-109">V tomto scénáři se token přidá na požadavky HTTP přes autorizační hlavičky.</span><span class="sxs-lookup"><span data-stu-id="f727f-109">For this scenario, a token is added to HTTP requests via the Authorization header.</span></span> <span data-ttu-id="f727f-110">Získání tokenu a obnova zpracovává pomocí knihovny Microsoft ověřování (MSAL).</span><span class="sxs-lookup"><span data-stu-id="f727f-110">Token acquisition and renewal is handled by the Microsoft Authentication Library (MSAL).</span></span>

### <a name="pre-requisites"></a><span data-ttu-id="f727f-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f727f-111">Pre-requisites</span></span>
* <span data-ttu-id="f727f-112">Tuto s průvodcem instalací se zaměřuje na Android Studio, ale žádné jiné vývojové prostředí Android aplikace je také přijatelné.</span><span class="sxs-lookup"><span data-stu-id="f727f-112">This guided setup is focused on Android Studio, but any other Android application development environment is also acceptable.</span></span> 
* <span data-ttu-id="f727f-113">Je vyžadován Android SDK 21 nebo novější (SDK 25 je doporučeno).</span><span class="sxs-lookup"><span data-stu-id="f727f-113">Android SDK 21 or newer is required (SDK 25 is recommended).</span></span>
* <span data-ttu-id="f727f-114">Google Chrome nebo pomocí webového prohlížeče pomocí vlastní karty je požadovaný pro tuto verzi systému Microsoft ověřování knihovny (MSAL) pro Android.</span><span class="sxs-lookup"><span data-stu-id="f727f-114">Google Chrome or a web browser using Custom Tabs is required for this release of Microsoft Authentication Library (MSAL) for Android.</span></span>

> <span data-ttu-id="f727f-115">Poznámka: Google Chrome není součástí Visual Studio Emulator for Android.</span><span class="sxs-lookup"><span data-stu-id="f727f-115">Note: Google Chrome is not included on Visual Studio Emulator for Android.</span></span> <span data-ttu-id="f727f-116">Doporučujeme k otestování tohoto kódu na emulátoru s 25 rozhraní API nebo bitovou kopii s 21 rozhraní API nebo novější, Google Chrome nainstaloval.</span><span class="sxs-lookup"><span data-stu-id="f727f-116">We recommend you to test this code on an Emulator with API 25 or an image with API 21 or newer that has with Google Chrome installed.</span></span>


### <a name="how-to-handle-token-acquisition-to-access-a-protected-web-api"></a><span data-ttu-id="f727f-117">Postupy: zpracování získání tokenu pro přístup k chráněné webové rozhraní API</span><span class="sxs-lookup"><span data-stu-id="f727f-117">How to handle token acquisition to access a protected Web API</span></span>

<span data-ttu-id="f727f-118">Jakmile se uživatel ověřuje, ukázkové aplikace obdrží token, který slouží k dotazu Microsoft Graph API nebo webového rozhraní API zabezpečeny v2 Microsoft Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f727f-118">After the user authenticates, the sample application receives a token that can be used to query Microsoft Graph API or a Web API secured by Microsoft Azure Active Directory v2.</span></span>

<span data-ttu-id="f727f-119">Rozhraní API, jako je například Microsoft Graph vyžadují token přístupu umožňuje přístup ke konkrétním prostředkům – například čtení profilu uživatele, kalendáře přístup uživatele nebo poslat e-mailu.</span><span class="sxs-lookup"><span data-stu-id="f727f-119">APIs such as Microsoft Graph require an access token to allow accessing specific resources – for example, to read a user’s profile, access user’s calendar or send an email.</span></span> <span data-ttu-id="f727f-120">Aplikace může požadovat token přístupu pomocí MSAL k těmto prostředkům přistupovat tak, že zadáte obory rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f727f-120">Your application can request an access token using MSAL to access these resources by specifying API scopes.</span></span> <span data-ttu-id="f727f-121">Tento přístupový token se pak přidá do hlavičky HTTP autorizace pro každé volání proti k chráněnému prostředku.</span><span class="sxs-lookup"><span data-stu-id="f727f-121">This access token is then added to the HTTP Authorization header for every call made against the protected resource.</span></span> 

<span data-ttu-id="f727f-122">MSAL spravuje ukládání do mezipaměti a aktualizaci přístupových tokenů, aby vaše aplikace nemusí.</span><span class="sxs-lookup"><span data-stu-id="f727f-122">MSAL manages caching and refreshing access tokens for you, so your application doesn't need to.</span></span>

### <a name="libraries"></a><span data-ttu-id="f727f-123">Knihovny</span><span class="sxs-lookup"><span data-stu-id="f727f-123">Libraries</span></span>

<span data-ttu-id="f727f-124">Tato příručka používá následující knihovny:</span><span class="sxs-lookup"><span data-stu-id="f727f-124">This guide uses the following libraries:</span></span>

|<span data-ttu-id="f727f-125">Knihovna</span><span class="sxs-lookup"><span data-stu-id="f727f-125">Library</span></span>|<span data-ttu-id="f727f-126">Popis</span><span class="sxs-lookup"><span data-stu-id="f727f-126">Description</span></span>|
|---|---|
|[<span data-ttu-id="f727f-127">com.microsoft.identity.Client</span><span class="sxs-lookup"><span data-stu-id="f727f-127">com.microsoft.identity.client</span></span>](http://javadoc.io/doc/com.microsoft.identity.client/msal)|<span data-ttu-id="f727f-128">Knihovna Microsoft ověřování (MSAL)</span><span class="sxs-lookup"><span data-stu-id="f727f-128">Microsoft Authentication Library (MSAL)</span></span>|
