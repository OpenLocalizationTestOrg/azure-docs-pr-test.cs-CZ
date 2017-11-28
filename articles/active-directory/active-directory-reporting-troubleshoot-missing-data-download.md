---
title: "Řešení potíží: Chybějící data v hello stáhli protokoly aktivity Azure Active Directory | Microsoft Docs"
description: "Poskytuje data toomissing řešení stažené protokolů aktivita služby Azure Active Directory."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 027b70e6efc570f81d3c836f50ee52aaa89be71a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="i-cant-find-any-data-in-hello-azure-active-directory-activity-logs-i-have-downloaded"></a><span data-ttu-id="dfe70-103">Nelze najít v protokolech hello Azure Active Directory aktivity, které staženým žádná data</span><span class="sxs-lookup"><span data-stu-id="dfe70-103">I can’t find any data in hello Azure Active Directory activity logs I have downloaded</span></span>


## <a name="symptoms"></a><span data-ttu-id="dfe70-104">Příznaky</span><span class="sxs-lookup"><span data-stu-id="dfe70-104">Symptoms</span></span>

<span data-ttu-id="dfe70-105">Protokoly aktivity hello (auditu nebo přihlášení) byl stažen a všechny záznamy hello se nezobrazí dobu hello, který byl vybrán.</span><span class="sxs-lookup"><span data-stu-id="dfe70-105">I downloaded hello activity logs (audit or sign-ins) and I don’t see all hello records for hello time I chose.</span></span> <span data-ttu-id="dfe70-106">Proč?</span><span class="sxs-lookup"><span data-stu-id="dfe70-106">Why?</span></span> 

 ![Vytváření sestav](./media/active-directory-reporting-troubleshoot-missing-data-download/01.png)
 

## <a name="cause"></a><span data-ttu-id="dfe70-108">Příčina</span><span class="sxs-lookup"><span data-stu-id="dfe70-108">Cause</span></span>

<span data-ttu-id="dfe70-109">Při stahování protokolů aktivity v hello portál Azure je omezená hello škálování too120K záznamy, seřazené podle většinu poslední.</span><span class="sxs-lookup"><span data-stu-id="dfe70-109">When you download activity logs in hello Azure portal, we limit hello scale too120K records, sorted by most recent.</span></span> 

## <a name="resolution"></a><span data-ttu-id="dfe70-110">Řešení</span><span class="sxs-lookup"><span data-stu-id="dfe70-110">Resolution</span></span>

<span data-ttu-id="dfe70-111">Můžete využít [rozhraní API pro vytváření sestav Azure AD](active-directory-reporting-api-getting-started.md) toofetch až tooa milionů záznamy v libovolném časovém okamžiku.</span><span class="sxs-lookup"><span data-stu-id="dfe70-111">You can leverage [Azure AD Reporting APIs](active-directory-reporting-api-getting-started.md) toofetch up tooa million records at any given point.</span></span> <span data-ttu-id="dfe70-112">Naše doporučený postup je toorun skript na základě plánu, který volá rozhraní API toofetch reporting hello zaznamenává přírůstkové způsobem přes v časovém intervalu (například denně nebo týdně).</span><span class="sxs-lookup"><span data-stu-id="dfe70-112">Our recommended approach is toorun a script on a scheduled basis that calls hello reporting APIs toofetch records in an incremental fashion over a period of time (e.g., daily or weekly).</span></span>

## <a name="next-steps"></a><span data-ttu-id="dfe70-113">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dfe70-113">Next steps</span></span>
<span data-ttu-id="dfe70-114">V tématu hello [Azure Active Directory, vytváření sestav – nejčastější dotazy](active-directory-reporting-faq.md).</span><span class="sxs-lookup"><span data-stu-id="dfe70-114">See hello [Azure Active Directory reporting FAQ](active-directory-reporting-faq.md).</span></span>

