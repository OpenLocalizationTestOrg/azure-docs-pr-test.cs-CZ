---
title: "aaaGetting začít s hello Azure AD reporting API na portálu classic hello Azure AD | Microsoft Docs"
description: "Jak tooget pracovat s hello reporting rozhraní API Azure Active Directory."
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
ms.openlocfilehash: 52e22d442650731fc6ed28991fc65f9182af0540
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-hello-azure-active-directory-reporting-api-on-hello-azure-ad-classic-portal"></a><span data-ttu-id="87c5a-103">Začínáme s Azure Active Directory reporting rozhraní API na portálu classic hello Azure AD hello</span><span class="sxs-lookup"><span data-stu-id="87c5a-103">Getting started with hello Azure Active Directory reporting API on hello Azure AD classic portal</span></span>
<span data-ttu-id="87c5a-104">*Toto téma je součástí hello [Azure Active Directory průvodce vytvářením sestav](active-directory-reporting-guide.md).*</span><span class="sxs-lookup"><span data-stu-id="87c5a-104">*This topic is part of hello [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).*</span></span>

<span data-ttu-id="87c5a-105">Azure Active Directory nabízí celou řadu sestav.</span><span class="sxs-lookup"><span data-stu-id="87c5a-105">Azure Active Directory provides you with a variety of reports.</span></span> <span data-ttu-id="87c5a-106">Hello data tyto sestavy může být velmi užitečná tooyour aplikace, jako je například systémů SIEM, auditování a nástroje business intelligence.</span><span class="sxs-lookup"><span data-stu-id="87c5a-106">hello data of these reports can be very useful tooyour applications, such as SIEM systems, audit, and business intelligence tools.</span></span> <span data-ttu-id="87c5a-107">rozhraní API poskytují programový přístup k datům toohello pomocí sady založené na REST API, vytváření sestav Hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="87c5a-107">hello Azure AD reporting APIs provide programmatic access toohello data through a set of REST-based APIs.</span></span> <span data-ttu-id="87c5a-108">Tato rozhraní API můžete volat z nejrůznějších programovacích jazyků a nástrojů.</span><span class="sxs-lookup"><span data-stu-id="87c5a-108">You can call these APIs from a variety of programming languages and tools.</span></span>

<span data-ttu-id="87c5a-109">Tento článek poskytuje hello informace je třeba spustit s vytvářením sestav Azure AD hello tooget rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="87c5a-109">This article provides you with hello information you need tooget started with hello Azure AD reporting APIs.</span></span>
<span data-ttu-id="87c5a-110">V další části hello můžete najít další podrobnosti o používání hello audit a přihlaste se rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="87c5a-110">In hello next section, you find more details about using hello audit and sign-in APIs.</span></span> <span data-ttu-id="87c5a-111">Všechny ostatní rozhraní API, najdete v části hello [sestav Azure AD a events(preview)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-reports-and-events-preview) článku.</span><span class="sxs-lookup"><span data-stu-id="87c5a-111">For all other APIs, see hello [Azure AD reports and events(preview)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-reports-and-events-preview) article.</span></span>

<span data-ttu-id="87c5a-112">Pro dotazy, problémy nebo připomínky, obraťte se na [AAD Reporting pomoci](mailto:aadreportinghelp@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="87c5a-112">For questions, issues or feedback, please contact [AAD Reporting Help](mailto:aadreportinghelp@microsoft.com).</span></span>

## <a name="learning-map"></a><span data-ttu-id="87c5a-113">Výuková mapa</span><span class="sxs-lookup"><span data-stu-id="87c5a-113">Learning map</span></span>
1. <span data-ttu-id="87c5a-114">**Příprava** -mohli otestovat vaši ukázky rozhraní API, musíte toocomplete hello [požadavky tooaccess hello Azure AD reporting API](active-directory-reporting-api-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="87c5a-114">**Prepare** - Before you can test your API samples, you need toocomplete hello [prerequisites tooaccess hello Azure AD reporting API](active-directory-reporting-api-prerequisites.md).</span></span>
2. <span data-ttu-id="87c5a-115">**Prozkoumejte** -získal první dojem o hello reporting rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="87c5a-115">**Explore** - Get a first impression of hello reporting APIs:</span></span>
   
   * [<span data-ttu-id="87c5a-116">Pomocí hello ukázky pro audit hello rozhraní API</span><span class="sxs-lookup"><span data-stu-id="87c5a-116">Using hello samples for hello audit API</span></span>](active-directory-reporting-api-audit-samples.md) 
   * [<span data-ttu-id="87c5a-117">Použití hello ukázky pro rozhraní API sestavy hello přihlašovací aktivita</span><span class="sxs-lookup"><span data-stu-id="87c5a-117">Using hello samples for hello sign-in activity report API</span></span>](active-directory-reporting-api-sign-in-activity-samples.md)
3. <span data-ttu-id="87c5a-118">**Přizpůsobení** -vytvořit vlastní řešení:</span><span class="sxs-lookup"><span data-stu-id="87c5a-118">**Customize** -  Create your own solution:</span></span> 
   
   * [<span data-ttu-id="87c5a-119">Pomocí referenční dokumentace rozhraní API auditu hello</span><span class="sxs-lookup"><span data-stu-id="87c5a-119">Using hello audit API reference</span></span>](active-directory-reporting-api-audit-reference.md) 
   * [<span data-ttu-id="87c5a-120">Pomocí sestavy přihlašovací aktivita hello referenční dokumentace rozhraní API</span><span class="sxs-lookup"><span data-stu-id="87c5a-120">Using hello sign-in activity report API reference</span></span>](active-directory-reporting-api-sign-in-activity-reference.md)

## <a name="next-steps"></a><span data-ttu-id="87c5a-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="87c5a-121">Next Steps</span></span>
<span data-ttu-id="87c5a-122">Pokud jste zvědaví toosee všechny koncové body k dispozici Azure AD Graph API hello přechodem příliš[https://graph.windows.net/tenant-name/reports/$ metadata? api-version = beta](https://graph.windows.net/tenant-name/reports/$metadata?api-version=beta).</span><span class="sxs-lookup"><span data-stu-id="87c5a-122">If you are curious toosee all of hello available Azure AD Graph API endpoints by navigating too[https://graph.windows.net/tenant-name/reports/$metadata?api-version=beta](https://graph.windows.net/tenant-name/reports/$metadata?api-version=beta).</span></span>

