---
title: "aaaAzure AD v2 JS SPA na základě nastavení – Úvod | Microsoft Docs"
description: "Jak může aplikace JavaScript SPA volat rozhraní API, které vyžadují přístupové tokeny bodem v2 Azure Active Directory"
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
ms.date: 06/01/2017
ms.author: andret
ms.openlocfilehash: 7e4250ccca837a17489a99603da167009cc2e3d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="call-hello-microsoft-graph-api-from-a-javascript-single-page-application-spa"></a><span data-ttu-id="cf7e2-103">Volání hello Microsoft Graph API z JavaScript jedné stránky aplikace (SPA)</span><span class="sxs-lookup"><span data-stu-id="cf7e2-103">Call hello Microsoft Graph API from a JavaScript Single Page Application (SPA)</span></span>

<span data-ttu-id="cf7e2-104">Tato příručka ukazuje, jak můžete JavaScript aplikace pro jednu stránku (SPA) přihlásit osobní pracovní a školní účty, získání přístupového tokenu a volání hello Microsoft Graph API nebo jiná rozhraní API, které vyžadují přístupové tokeny z hello koncový bod v2 Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="cf7e2-104">This guide demonstrates how a JavaScript Single Page Application (SPA) can sign in personal, work and school accounts, get an access token, and call hello Microsoft Graph API or other APIs that require access tokens from hello Azure Active Directory v2 endpoint.</span></span>

### <a name="how-this-guide-works"></a><span data-ttu-id="cf7e2-105">Jak funguje tato příručka</span><span class="sxs-lookup"><span data-stu-id="cf7e2-105">How this guide works</span></span>

![Jak funguje hello ukázkové aplikace generované tímto průvodcem](media/active-directory-singlepageapp-javascriptspa-introduction/javascriptspa-intro.png)

<!--start-collapse-->
### <a name="more-information"></a><span data-ttu-id="cf7e2-107">Další informace</span><span class="sxs-lookup"><span data-stu-id="cf7e2-107">More Information</span></span>

<span data-ttu-id="cf7e2-108">Hello ukázkové aplikace vytvořené v této příručce umožňuje JavaScript SPA tooquery hello Microsoft Graph API nebo webového rozhraní API, které přijímá tokeny z koncového bodu v2 Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="cf7e2-108">hello sample application created by this guide enables a JavaScript SPA tooquery hello Microsoft Graph API or a Web API that accepts tokens from Azure Active Directory v2 endpoint.</span></span> <span data-ttu-id="cf7e2-109">V tomto scénáři poté, co uživatel přihlásí, je token přístupu tooHTTP požadovaný a přidání požadavků prostřednictvím hello autorizační hlavičky.</span><span class="sxs-lookup"><span data-stu-id="cf7e2-109">For this scenario, after a user signs-in, an access token is requested and added tooHTTP requests via hello authorization header.</span></span> <span data-ttu-id="cf7e2-110">Získání tokenu a obnovení, jsou zpracovávány hello Microsoft ověřování knihovny (MSAL).</span><span class="sxs-lookup"><span data-stu-id="cf7e2-110">Token acquisition and renewal are handled by hello Microsoft Authentication Library (MSAL).</span></span>

<!--end-collapse-->

<!--start-collapse-->
### <a name="libraries"></a><span data-ttu-id="cf7e2-111">Knihovny</span><span class="sxs-lookup"><span data-stu-id="cf7e2-111">Libraries</span></span>

<span data-ttu-id="cf7e2-112">Tato příručka používá hello následující knihovny:</span><span class="sxs-lookup"><span data-stu-id="cf7e2-112">This guide uses hello following library:</span></span>

|<span data-ttu-id="cf7e2-113">Knihovna</span><span class="sxs-lookup"><span data-stu-id="cf7e2-113">Library</span></span>|<span data-ttu-id="cf7e2-114">Popis</span><span class="sxs-lookup"><span data-stu-id="cf7e2-114">Description</span></span>|
|---|---|
|[<span data-ttu-id="cf7e2-115">msal.js</span><span class="sxs-lookup"><span data-stu-id="cf7e2-115">msal.js</span></span>](https://github.com/AzureAD/microsoft-authentication-library-for-js)|<span data-ttu-id="cf7e2-116">Knihovna Microsoft ověřování pro JavaScript Preview</span><span class="sxs-lookup"><span data-stu-id="cf7e2-116">Microsoft Authentication Library for JavaScript Preview</span></span>|

> [!NOTE]
> <span data-ttu-id="cf7e2-117">*msal.js* cíle hello *koncový bod služby Azure Active Directory v2* – což umožňuje toosign osobní, školní i pracovní účty v a získávat tokeny.</span><span class="sxs-lookup"><span data-stu-id="cf7e2-117">*msal.js* targets hello *Azure Active Directory v2 endpoint* - which enables personal, school and work accounts toosign in and acquire tokens.</span></span> <span data-ttu-id="cf7e2-118">Hello *koncový bod služby Azure Active Directory v2* má [určitá omezení](..\active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="cf7e2-118">hello *Azure Active Directory v2 endpoint* has [some limitations](..\active-directory-v2-limitations.md).</span></span> <span data-ttu-id="cf7e2-119">Pokud vás zajímá jenom pracovní a školní účty, použijte *adal.js* a hello *koncový bod V1*.</span><span class="sxs-lookup"><span data-stu-id="cf7e2-119">If you are interested only in school and work accounts, use *adal.js* and hello *V1 endpoint*.</span></span> <span data-ttu-id="cf7e2-120">toounderstand rozdíly mezi koncovými body v1 a v2 hello číst hello [v1-v2 porovnání](..\active-directory-v2-compare.md).</span><span class="sxs-lookup"><span data-stu-id="cf7e2-120">toounderstand differences between hello v1 and v2 endpoints read hello [v1-v2 comparison](..\active-directory-v2-compare.md).</span></span>

<!--end-collapse-->
