---
title: "Začínáme s Azure AD reporting rozhraní API na portálu Azure AD classic | Microsoft Docs"
description: "Jak začít pracovat s Azure Active Directory, vytváření sestav rozhraní API"
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: 8813b911-a4ec-4234-8474-2eef9afea11e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/18/2017
ms.author: dhanyahk;markvi
ms.openlocfilehash: 5e98b660fe19bb8abebf1c3b996b6295a6c4e728
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="getting-started-with-the-azure-active-directory-reporting-api-on-the-azure-ad-classic-portal"></a><span data-ttu-id="0b531-103">Začínáme s Azure Active Directory, vytváření sestav rozhraní API na portálu Azure AD classic</span><span class="sxs-lookup"><span data-stu-id="0b531-103">Getting started with the Azure Active Directory reporting API on the Azure AD classic portal</span></span>
<span data-ttu-id="0b531-104">*Toto téma je součástí [Azure Active Directory průvodce vytvářením sestav](active-directory-reporting-guide.md).*</span><span class="sxs-lookup"><span data-stu-id="0b531-104">*This topic is part of the [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).*</span></span>

<span data-ttu-id="0b531-105">Azure Active Directory nabízí celou řadu sestav.</span><span class="sxs-lookup"><span data-stu-id="0b531-105">Azure Active Directory provides you with a variety of reports.</span></span> <span data-ttu-id="0b531-106">Data z těchto sestav můžou být velmi užitečná pro vaše aplikace, jako jsou systémy SIEM nebo nástroje pro auditování a business intelligence.</span><span class="sxs-lookup"><span data-stu-id="0b531-106">The data of these reports can be very useful to your applications, such as SIEM systems, audit, and business intelligence tools.</span></span> <span data-ttu-id="0b531-107">Rozhraní API pro generování sestav v Azure AD poskytují programový přístup k těmto datům prostřednictvím sady rozhraní API založených na REST.</span><span class="sxs-lookup"><span data-stu-id="0b531-107">The Azure AD reporting APIs provide programmatic access to the data through a set of REST-based APIs.</span></span> <span data-ttu-id="0b531-108">Tato rozhraní API můžete volat z nejrůznějších programovacích jazyků a nástrojů.</span><span class="sxs-lookup"><span data-stu-id="0b531-108">You can call these APIs from a variety of programming languages and tools.</span></span>

<span data-ttu-id="0b531-109">Tento článek vám poskytne informace, které potřebujete začít s Azure AD reporting rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0b531-109">This article provides you with the information you need to get started with the Azure AD reporting APIs.</span></span>
<span data-ttu-id="0b531-110">V další části můžete najít další podrobnosti o používání auditu a přihlaste se rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0b531-110">In the next section, you find more details about using the audit and sign-in APIs.</span></span> <span data-ttu-id="0b531-111">Všechny ostatní rozhraní API, najdete v článku [sestav Azure AD a events(preview)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-reports-and-events-preview) článku.</span><span class="sxs-lookup"><span data-stu-id="0b531-111">For all other APIs, see the [Azure AD reports and events(preview)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-reports-and-events-preview) article.</span></span>

<span data-ttu-id="0b531-112">Pro dotazy, problémy nebo připomínky, obraťte se na [AAD Reporting pomoci](mailto:aadreportinghelp@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="0b531-112">For questions, issues or feedback, please contact [AAD Reporting Help](mailto:aadreportinghelp@microsoft.com).</span></span>

## <a name="learning-map"></a><span data-ttu-id="0b531-113">Výuková mapa</span><span class="sxs-lookup"><span data-stu-id="0b531-113">Learning map</span></span>
1. <span data-ttu-id="0b531-114">**Příprava** -mohli otestovat vaši ukázky rozhraní API, které potřebujete k dokončení [požadavky pro přístup k Azure AD reporting rozhraní API](active-directory-reporting-api-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="0b531-114">**Prepare** - Before you can test your API samples, you need to complete the [prerequisites to access the Azure AD reporting API](active-directory-reporting-api-prerequisites.md).</span></span>
2. <span data-ttu-id="0b531-115">**Prozkoumejte** -získal první dojem o rozhraní API pro vytváření sestav:</span><span class="sxs-lookup"><span data-stu-id="0b531-115">**Explore** - Get a first impression of the reporting APIs:</span></span>
   
   * [<span data-ttu-id="0b531-116">Pomocí ukázky pro audit rozhraní API</span><span class="sxs-lookup"><span data-stu-id="0b531-116">Using the samples for the audit API</span></span>](active-directory-reporting-api-audit-samples.md) 
   * [<span data-ttu-id="0b531-117">Pomocí ukázky pro sestavu přihlašovací aktivita rozhraní API</span><span class="sxs-lookup"><span data-stu-id="0b531-117">Using the samples for the sign-in activity report API</span></span>](active-directory-reporting-api-sign-in-activity-samples.md)
3. <span data-ttu-id="0b531-118">**Přizpůsobení** -vytvořit vlastní řešení:</span><span class="sxs-lookup"><span data-stu-id="0b531-118">**Customize** -  Create your own solution:</span></span> 
   
   * [<span data-ttu-id="0b531-119">Pomocí auditu referenční dokumentace rozhraní API</span><span class="sxs-lookup"><span data-stu-id="0b531-119">Using the audit API reference</span></span>](active-directory-reporting-api-audit-reference.md) 
   * [<span data-ttu-id="0b531-120">Pomocí odkaz na sestavu API přihlašovací aktivita</span><span class="sxs-lookup"><span data-stu-id="0b531-120">Using the sign-in activity report API reference</span></span>](active-directory-reporting-api-sign-in-activity-reference.md)

## <a name="next-steps"></a><span data-ttu-id="0b531-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0b531-121">Next Steps</span></span>
<span data-ttu-id="0b531-122">Pokud jste zvědaví zobrazíte všechny koncové body k dispozici Azure AD Graph API přechodem na [https://graph.windows.net/tenant-name/reports/$ metadata? api-version = beta](https://graph.windows.net/tenant-name/reports/$metadata?api-version=beta).</span><span class="sxs-lookup"><span data-stu-id="0b531-122">If you are curious to see all of the available Azure AD Graph API endpoints by navigating to [https://graph.windows.net/tenant-name/reports/$metadata?api-version=beta](https://graph.windows.net/tenant-name/reports/$metadata?api-version=beta).</span></span>

