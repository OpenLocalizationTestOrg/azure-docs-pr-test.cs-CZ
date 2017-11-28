---
title: "aaaGetting spuštění s rozhraním API pro generování sestav hello Azure AD | Microsoft Docs"
description: "Jak tooget pracovat s hello reporting rozhraní API Azure Active Directory."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8813b911-a4ec-4234-8474-2eef9afea11e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/18/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: bb7d72ba445daa367d7502889c38a605a16f26d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-hello-azure-active-directory-reporting-api"></a><span data-ttu-id="92994-103">Začínáme s Azure Active Directory, vytváření sestav API hello</span><span class="sxs-lookup"><span data-stu-id="92994-103">Getting started with hello Azure Active Directory reporting API</span></span>

<span data-ttu-id="92994-104">Azure Active Directory nabízí celou řadu sestav.</span><span class="sxs-lookup"><span data-stu-id="92994-104">Azure Active Directory provides you with a variety of reports.</span></span> <span data-ttu-id="92994-105">Hello data tyto sestavy může být velmi užitečná tooyour aplikace, jako je například systémů SIEM, auditování a nástroje business intelligence.</span><span class="sxs-lookup"><span data-stu-id="92994-105">hello data of these reports can be very useful tooyour applications, such as SIEM systems, audit, and business intelligence tools.</span></span> <span data-ttu-id="92994-106">rozhraní API poskytují programový přístup k datům toohello pomocí sady založené na REST API, vytváření sestav Hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="92994-106">hello Azure AD reporting APIs provide programmatic access toohello data through a set of REST-based APIs.</span></span> <span data-ttu-id="92994-107">Tato rozhraní API můžete volat z nejrůznějších programovacích jazyků a nástrojů.</span><span class="sxs-lookup"><span data-stu-id="92994-107">You can call these APIs from a variety of programming languages and tools.</span></span>

<span data-ttu-id="92994-108">Tento článek poskytuje hello informace je třeba spustit s vytvářením sestav Azure AD hello tooget rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="92994-108">This article provides you with hello information you need tooget started with hello Azure AD reporting APIs.</span></span>
<span data-ttu-id="92994-109">V další části hello můžete najít další podrobnosti o používání hello audit a přihlaste se rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="92994-109">In hello next section, you find more details about using hello audit and sign-in APIs.</span></span> 

<span data-ttu-id="92994-110">Nejčastější dotazy, přečtěte si naše [– nejčastější dotazy](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-faq).</span><span class="sxs-lookup"><span data-stu-id="92994-110">For frequently asked questions,read our [FAQ](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-faq).</span></span> <span data-ttu-id="92994-111">O problémech, prosím [souboru lístek podpory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-troubleshooting-support-howto)</span><span class="sxs-lookup"><span data-stu-id="92994-111">For issues, please [file a support ticket](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-troubleshooting-support-howto)</span></span>

## <a name="learning-map"></a><span data-ttu-id="92994-112">Výuková mapa</span><span class="sxs-lookup"><span data-stu-id="92994-112">Learning map</span></span>
1. <span data-ttu-id="92994-113">**Příprava** -mohli otestovat vaši ukázky rozhraní API, musíte toocomplete hello [požadavky tooaccess hello Azure AD reporting API](active-directory-reporting-api-prerequisites-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="92994-113">**Prepare** - Before you can test your API samples, you need toocomplete hello [prerequisites tooaccess hello Azure AD reporting API](active-directory-reporting-api-prerequisites-azure-portal.md).</span></span>
2. <span data-ttu-id="92994-114">**Prozkoumejte** -získal první dojem o hello reporting rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="92994-114">**Explore** - Get a first impression of hello reporting APIs:</span></span>
   
   * [<span data-ttu-id="92994-115">Pomocí hello ukázky pro audit hello rozhraní API</span><span class="sxs-lookup"><span data-stu-id="92994-115">Using hello samples for hello audit API</span></span>](active-directory-reporting-api-audit-samples.md) 
   * [<span data-ttu-id="92994-116">Použití hello ukázky pro rozhraní API sestavy hello přihlašovací aktivita</span><span class="sxs-lookup"><span data-stu-id="92994-116">Using hello samples for hello sign-in activity report API</span></span>](active-directory-reporting-api-sign-in-activity-samples.md)
3. <span data-ttu-id="92994-117">**Přizpůsobení** -vytvořit vlastní řešení:</span><span class="sxs-lookup"><span data-stu-id="92994-117">**Customize** -  Create your own solution:</span></span> 
   
   * [<span data-ttu-id="92994-118">Pomocí referenční dokumentace rozhraní API auditu hello</span><span class="sxs-lookup"><span data-stu-id="92994-118">Using hello audit API reference</span></span>](active-directory-reporting-api-audit-reference.md) 
   * [<span data-ttu-id="92994-119">Pomocí sestavy přihlašovací aktivita hello referenční dokumentace rozhraní API</span><span class="sxs-lookup"><span data-stu-id="92994-119">Using hello sign-in activity report API reference</span></span>](active-directory-reporting-api-sign-in-activity-reference.md)

## <a name="next-steps"></a><span data-ttu-id="92994-120">Další kroky</span><span class="sxs-lookup"><span data-stu-id="92994-120">Next Steps</span></span>
<span data-ttu-id="92994-121">Pokud jste zvědaví toosee všechny dostupné Azure AD Graph API koncové body, použijte tento odkaz: [https://graph.windows.net/tenant-name/activities/$ metadata? api-version = beta](https://graph.windows.net/tenant-name/activities/$metadata?api-version=beta).</span><span class="sxs-lookup"><span data-stu-id="92994-121">If you are curious toosee all available Azure AD Graph API endpoints, use this link: [https://graph.windows.net/tenant-name/activities/$metadata?api-version=beta](https://graph.windows.net/tenant-name/activities/$metadata?api-version=beta).</span></span>

