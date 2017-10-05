---
title: "Začínáme s Azure AD reporting rozhraní API | Microsoft Docs"
description: "Jak začít pracovat s Azure Active Directory, vytváření sestav rozhraní API"
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
ms.openlocfilehash: 9944cbd2b1b7c4acb18d37da1394c0bbc170f77d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="getting-started-with-the-azure-active-directory-reporting-api"></a><span data-ttu-id="b6db7-103">Začínáme s Azure Active Directory, vytváření sestav rozhraní API</span><span class="sxs-lookup"><span data-stu-id="b6db7-103">Getting started with the Azure Active Directory reporting API</span></span>

<span data-ttu-id="b6db7-104">Azure Active Directory nabízí celou řadu sestav.</span><span class="sxs-lookup"><span data-stu-id="b6db7-104">Azure Active Directory provides you with a variety of reports.</span></span> <span data-ttu-id="b6db7-105">Data z těchto sestav můžou být velmi užitečná pro vaše aplikace, jako jsou systémy SIEM nebo nástroje pro auditování a business intelligence.</span><span class="sxs-lookup"><span data-stu-id="b6db7-105">The data of these reports can be very useful to your applications, such as SIEM systems, audit, and business intelligence tools.</span></span> <span data-ttu-id="b6db7-106">Rozhraní API pro generování sestav v Azure AD poskytují programový přístup k těmto datům prostřednictvím sady rozhraní API založených na REST.</span><span class="sxs-lookup"><span data-stu-id="b6db7-106">The Azure AD reporting APIs provide programmatic access to the data through a set of REST-based APIs.</span></span> <span data-ttu-id="b6db7-107">Tato rozhraní API můžete volat z nejrůznějších programovacích jazyků a nástrojů.</span><span class="sxs-lookup"><span data-stu-id="b6db7-107">You can call these APIs from a variety of programming languages and tools.</span></span>

<span data-ttu-id="b6db7-108">Tento článek vám poskytne informace, které potřebujete začít s Azure AD reporting rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b6db7-108">This article provides you with the information you need to get started with the Azure AD reporting APIs.</span></span>
<span data-ttu-id="b6db7-109">V další části můžete najít další podrobnosti o používání auditu a přihlaste se rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b6db7-109">In the next section, you find more details about using the audit and sign-in APIs.</span></span> 

<span data-ttu-id="b6db7-110">Nejčastější dotazy, přečtěte si naše [– nejčastější dotazy](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-faq).</span><span class="sxs-lookup"><span data-stu-id="b6db7-110">For frequently asked questions,read our [FAQ](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-faq).</span></span> <span data-ttu-id="b6db7-111">O problémech, prosím [souboru lístek podpory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-troubleshooting-support-howto)</span><span class="sxs-lookup"><span data-stu-id="b6db7-111">For issues, please [file a support ticket](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-troubleshooting-support-howto)</span></span>

## <a name="learning-map"></a><span data-ttu-id="b6db7-112">Výuková mapa</span><span class="sxs-lookup"><span data-stu-id="b6db7-112">Learning map</span></span>
1. <span data-ttu-id="b6db7-113">**Příprava** -mohli otestovat vaši ukázky rozhraní API, které potřebujete k dokončení [požadavky pro přístup k Azure AD reporting rozhraní API](active-directory-reporting-api-prerequisites-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b6db7-113">**Prepare** - Before you can test your API samples, you need to complete the [prerequisites to access the Azure AD reporting API](active-directory-reporting-api-prerequisites-azure-portal.md).</span></span>
2. <span data-ttu-id="b6db7-114">**Prozkoumejte** -získal první dojem o rozhraní API pro vytváření sestav:</span><span class="sxs-lookup"><span data-stu-id="b6db7-114">**Explore** - Get a first impression of the reporting APIs:</span></span>
   
   * [<span data-ttu-id="b6db7-115">Pomocí ukázky pro audit rozhraní API</span><span class="sxs-lookup"><span data-stu-id="b6db7-115">Using the samples for the audit API</span></span>](active-directory-reporting-api-audit-samples.md) 
   * [<span data-ttu-id="b6db7-116">Pomocí ukázky pro sestavu přihlašovací aktivita rozhraní API</span><span class="sxs-lookup"><span data-stu-id="b6db7-116">Using the samples for the sign-in activity report API</span></span>](active-directory-reporting-api-sign-in-activity-samples.md)
3. <span data-ttu-id="b6db7-117">**Přizpůsobení** -vytvořit vlastní řešení:</span><span class="sxs-lookup"><span data-stu-id="b6db7-117">**Customize** -  Create your own solution:</span></span> 
   
   * [<span data-ttu-id="b6db7-118">Pomocí auditu referenční dokumentace rozhraní API</span><span class="sxs-lookup"><span data-stu-id="b6db7-118">Using the audit API reference</span></span>](active-directory-reporting-api-audit-reference.md) 
   * [<span data-ttu-id="b6db7-119">Pomocí odkaz na sestavu API přihlašovací aktivita</span><span class="sxs-lookup"><span data-stu-id="b6db7-119">Using the sign-in activity report API reference</span></span>](active-directory-reporting-api-sign-in-activity-reference.md)

## <a name="next-steps"></a><span data-ttu-id="b6db7-120">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b6db7-120">Next Steps</span></span>
<span data-ttu-id="b6db7-121">Pokud jste zvědaví zobrazíte všechny koncové body Azure AD Graph API k dispozici, použijte tento odkaz: [https://graph.windows.net/tenant-name/activities/$ metadata? api-version = beta](https://graph.windows.net/tenant-name/activities/$metadata?api-version=beta).</span><span class="sxs-lookup"><span data-stu-id="b6db7-121">If you are curious to see all available Azure AD Graph API endpoints, use this link: [https://graph.windows.net/tenant-name/activities/$metadata?api-version=beta](https://graph.windows.net/tenant-name/activities/$metadata?api-version=beta).</span></span>

