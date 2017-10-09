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
# <a name="call-hello-microsoft-graph-api-from-an-android-app"></a><span data-ttu-id="a0146-103">Volání hello Microsoft Graph API z aplikace pro Android</span><span class="sxs-lookup"><span data-stu-id="a0146-103">Call hello Microsoft Graph API from an Android app</span></span>

<span data-ttu-id="a0146-104">Tato příručka ukazuje, jak získat přístupový token a volání Microsoft Graph API nebo jiná rozhraní API, které vyžadují přístupové tokeny z koncového bodu Azure Active Directory v2 nativní aplikace pro Android.</span><span class="sxs-lookup"><span data-stu-id="a0146-104">This guide demonstrates how a native Android application can get an access token and call Microsoft Graph API or other APIs that require access tokens from Azure Active Directory v2 endpoint.</span></span>

<span data-ttu-id="a0146-105">Na konci hello tohoto průvodce, vaše aplikace bude být schopný toocall chráněné API pomocí osobní účty (včetně live.com, outlook.com a dalších) a také pracovní a školní účty v jakémkoli společnosti nebo organizace, která má Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="a0146-105">At hello end of this guide, your application will be able toocall a protected API using personal accounts (including outlook.com, live.com, and others) as well as work and school accounts from any company or organization that has Azure Active Directory.</span></span>  

### <a name="how-this-sample-works"></a><span data-ttu-id="a0146-106">Fungování této ukázky</span><span class="sxs-lookup"><span data-stu-id="a0146-106">How this sample works</span></span>
![Fungování této ukázky](media/active-directory-mobileanddesktopapp-android-intro/android-intro.png)

<span data-ttu-id="a0146-108">Ukázka Hello vytvořit v této příručce je založena na scénář, kde aplikace pro Android je použité tooquery webového rozhraní API, které přijímá tokeny z Azure Active Directory v2 koncového bodu – v tomto případě Microsoft Graph API.</span><span class="sxs-lookup"><span data-stu-id="a0146-108">hello sample created by this guide is based on a scenario where an Android application is used tooquery a Web API that accepts tokens from Azure Active Directory v2 endpoint – in this case, Microsoft Graph API.</span></span> <span data-ttu-id="a0146-109">V tomto scénáři se přidá token tooHTTP požadavky prostřednictvím hello autorizační hlavičky.</span><span class="sxs-lookup"><span data-stu-id="a0146-109">For this scenario, a token is added tooHTTP requests via hello Authorization header.</span></span> <span data-ttu-id="a0146-110">Získání tokenu a obnova zpracovává hello Microsoft ověřování knihovny (MSAL).</span><span class="sxs-lookup"><span data-stu-id="a0146-110">Token acquisition and renewal is handled by hello Microsoft Authentication Library (MSAL).</span></span>

### <a name="pre-requisites"></a><span data-ttu-id="a0146-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a0146-111">Pre-requisites</span></span>
* <span data-ttu-id="a0146-112">Tuto s průvodcem instalací se zaměřuje na Android Studio, ale žádné jiné vývojové prostředí Android aplikace je také přijatelné.</span><span class="sxs-lookup"><span data-stu-id="a0146-112">This guided setup is focused on Android Studio, but any other Android application development environment is also acceptable.</span></span> 
* <span data-ttu-id="a0146-113">Je vyžadován Android SDK 21 nebo novější (SDK 25 je doporučeno).</span><span class="sxs-lookup"><span data-stu-id="a0146-113">Android SDK 21 or newer is required (SDK 25 is recommended).</span></span>
* <span data-ttu-id="a0146-114">Google Chrome nebo pomocí webového prohlížeče pomocí vlastní karty je požadovaný pro tuto verzi systému Microsoft ověřování knihovny (MSAL) pro Android.</span><span class="sxs-lookup"><span data-stu-id="a0146-114">Google Chrome or a web browser using Custom Tabs is required for this release of Microsoft Authentication Library (MSAL) for Android.</span></span>

> <span data-ttu-id="a0146-115">Poznámka: Google Chrome není součástí Visual Studio Emulator for Android.</span><span class="sxs-lookup"><span data-stu-id="a0146-115">Note: Google Chrome is not included on Visual Studio Emulator for Android.</span></span> <span data-ttu-id="a0146-116">Doporučujeme tootest tento kód na emulátoru s 25 rozhraní API nebo bitovou kopii s 21 rozhraní API nebo novější, která má s Google Chrome nainstalovanou.</span><span class="sxs-lookup"><span data-stu-id="a0146-116">We recommend you tootest this code on an Emulator with API 25 or an image with API 21 or newer that has with Google Chrome installed.</span></span>


### <a name="how-toohandle-token-acquisition-tooaccess-a-protected-web-api"></a><span data-ttu-id="a0146-117">Jak toohandle token pořízení tooaccess chráněné webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="a0146-117">How toohandle token acquisition tooaccess a protected Web API</span></span>

<span data-ttu-id="a0146-118">Po ověření uživatele hello hello ukázkovou aplikaci přijme token, který lze použít tooquery Microsoft Graph API nebo webového rozhraní API zabezpečeny v2 Microsoft Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="a0146-118">After hello user authenticates, hello sample application receives a token that can be used tooquery Microsoft Graph API or a Web API secured by Microsoft Azure Active Directory v2.</span></span>

<span data-ttu-id="a0146-119">Rozhraní API, jako je Microsoft Graph vyžadují k přístupu tooallow tokenu přístupu konkrétním prostředkům – například tooread uživatelského profilu, přístup k kalendáře uživatele nebo e-mailovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="a0146-119">APIs such as Microsoft Graph require an access token tooallow accessing specific resources – for example, tooread a user’s profile, access user’s calendar or send an email.</span></span> <span data-ttu-id="a0146-120">Aplikace může požadovat token přístupu pomocí MSAL tooaccess tyto prostředky tak, že zadáte obory rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="a0146-120">Your application can request an access token using MSAL tooaccess these resources by specifying API scopes.</span></span> <span data-ttu-id="a0146-121">Tento přístupový token to přidané toohello HTTP autorizační hlavičky pro každé volání proti hello chráněný prostředek.</span><span class="sxs-lookup"><span data-stu-id="a0146-121">This access token is then added toohello HTTP Authorization header for every call made against hello protected resource.</span></span> 

<span data-ttu-id="a0146-122">MSAL spravuje ukládání do mezipaměti a aktualizaci přístupových tokenů, aby vaše aplikace nemusí.</span><span class="sxs-lookup"><span data-stu-id="a0146-122">MSAL manages caching and refreshing access tokens for you, so your application doesn't need to.</span></span>

### <a name="libraries"></a><span data-ttu-id="a0146-123">Knihovny</span><span class="sxs-lookup"><span data-stu-id="a0146-123">Libraries</span></span>

<span data-ttu-id="a0146-124">Tato příručka používá hello následující knihovny:</span><span class="sxs-lookup"><span data-stu-id="a0146-124">This guide uses hello following libraries:</span></span>

|<span data-ttu-id="a0146-125">Knihovna</span><span class="sxs-lookup"><span data-stu-id="a0146-125">Library</span></span>|<span data-ttu-id="a0146-126">Popis</span><span class="sxs-lookup"><span data-stu-id="a0146-126">Description</span></span>|
|---|---|
|[<span data-ttu-id="a0146-127">com.microsoft.identity.Client</span><span class="sxs-lookup"><span data-stu-id="a0146-127">com.microsoft.identity.client</span></span>](http://javadoc.io/doc/com.microsoft.identity.client/msal)|<span data-ttu-id="a0146-128">Knihovna Microsoft ověřování (MSAL)</span><span class="sxs-lookup"><span data-stu-id="a0146-128">Microsoft Authentication Library (MSAL)</span></span>|
