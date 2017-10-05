---
title: "Řešení potíží: Chybějící data v protokolech aktivity služby Azure Active Directory | Dokumentace Microsoftu"
description: "Obsahuje seznam různých dostupných sestav pro Azure Active Directory."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 7cbe4337-bb77-4ee0-b254-3e368be06db7
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 47617f8f727027de113a0f503308c8accc58859e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="i-cant-find-some-actions-that-i-performed-in-the-azure-active-directory-activity-log"></a><span data-ttu-id="94d20-103">Nemůžu v protokolu aktivity Azure Active Directory najít některé provedené akce</span><span class="sxs-lookup"><span data-stu-id="94d20-103">I can’t find some actions that I performed in the Azure Active Directory activity log</span></span>


## <a name="symptoms"></a><span data-ttu-id="94d20-104">Příznaky</span><span class="sxs-lookup"><span data-stu-id="94d20-104">Symptoms</span></span>

<span data-ttu-id="94d20-105">Provedl jsem nějaké akce na webu Azure Portal a očekával jsem pro tyto akce zobrazení protokolu auditu v okně `Activity logs > Audit Logs`, ale nemůžu je najít.</span><span class="sxs-lookup"><span data-stu-id="94d20-105">I performed some actions in the Azure portal and expected to see the audit logs for those actions in the `Activity logs > Audit Logs` blade, but I can’t find them.</span></span>

 ![Vytváření sestav](./media/active-directory-reporting-troubleshoot-missing-audit-data/01.png)
 

## <a name="cause"></a><span data-ttu-id="94d20-107">Příčina</span><span class="sxs-lookup"><span data-stu-id="94d20-107">Cause</span></span>

<span data-ttu-id="94d20-108">Akce se v protokolu auditu aktivity nezobrazí okamžitě.</span><span class="sxs-lookup"><span data-stu-id="94d20-108">Actions don’t appear immediately in the Activity Audit log.</span></span> <span data-ttu-id="94d20-109">Zobrazení operací v protokolech auditu na portálu může od jejich provedení trvat 15 minut až jednu hodinu.</span><span class="sxs-lookup"><span data-stu-id="94d20-109">It can take anywhere from 15 minutes to an hour to see the audit logs in the portal from the time the operation is performed.</span></span>

## <a name="resolution"></a><span data-ttu-id="94d20-110">Řešení</span><span class="sxs-lookup"><span data-stu-id="94d20-110">Resolution</span></span>

<span data-ttu-id="94d20-111">Počkejte 15 minut až hodinu a pak se podívejte, jestli se akce v protokolu zobrazily.</span><span class="sxs-lookup"><span data-stu-id="94d20-111">Wait for 15 minutes to an hour and see if the actions appear in the log.</span></span> <span data-ttu-id="94d20-112">Pokud je stále nevidíte, vytvořte prosím lístek podpory a my se na to podíváme.</span><span class="sxs-lookup"><span data-stu-id="94d20-112">If you still don’t see them, please raise a support ticket with us and we will look into it.</span></span>


## <a name="next-steps"></a><span data-ttu-id="94d20-113">Další kroky</span><span class="sxs-lookup"><span data-stu-id="94d20-113">Next steps</span></span>
<span data-ttu-id="94d20-114">Přečtěte si [nejčastější dotazy ke generování sestav v Azure Active Directory](active-directory-reporting-faq.md).</span><span class="sxs-lookup"><span data-stu-id="94d20-114">See the [Azure Active Directory reporting FAQ](active-directory-reporting-faq.md).</span></span>

