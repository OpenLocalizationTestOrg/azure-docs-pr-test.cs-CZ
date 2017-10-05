---
title: "Operace Správce synchronizace služby Azure AD Connect | Microsoft Docs"
description: "Porozumět kartu operace ve Správci služby synchronizace Azure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 97a26565-618f-4313-8711-5925eeb47cdc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a1475e4fcd11eb008badba49665f4af6029a1697
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="using-the-sync-service-manager-operations-tab"></a><span data-ttu-id="c249b-103">Pomocí karty operace synchronizace Service Manager</span><span class="sxs-lookup"><span data-stu-id="c249b-103">Using the Sync Service Manager Operations tab</span></span>

![Správce synchronizace služby](./media/active-directory-aadconnectsync-service-manager-ui/operations.png)

<span data-ttu-id="c249b-105">Na kartě operations zobrazuje výsledky z nejnovější operace.</span><span class="sxs-lookup"><span data-stu-id="c249b-105">The operations tab shows the results from the most recent operations.</span></span> <span data-ttu-id="c249b-106">Na této kartě je klíčem k pochopení a řešení problémů.</span><span class="sxs-lookup"><span data-stu-id="c249b-106">This tab is key to understand and troubleshoot issues.</span></span>

## <a name="understand-the-information-visible-in-the-operations-tab"></a><span data-ttu-id="c249b-107">Pochopení informace zobrazené na kartě operace</span><span class="sxs-lookup"><span data-stu-id="c249b-107">Understand the information visible in the operations tab</span></span>
<span data-ttu-id="c249b-108">V horní polovině ukazuje všechny běží v chronologickém pořadí.</span><span class="sxs-lookup"><span data-stu-id="c249b-108">The top half shows all runs in chronological order.</span></span> <span data-ttu-id="c249b-109">Ve výchozím nastavení, operace protokolu udržuje informace o posledních sedmi dnů, ale toto nastavení lze změnit pomocí [Plánovač](active-directory-aadconnectsync-feature-scheduler.md).</span><span class="sxs-lookup"><span data-stu-id="c249b-109">By default, the operations log keeps information about the last seven days, but this setting can be changed with the [scheduler](active-directory-aadconnectsync-feature-scheduler.md).</span></span> <span data-ttu-id="c249b-110">Chcete-li vyhledat všechny spuštění, které nejsou zobrazeny stav úspěšného dokončení.</span><span class="sxs-lookup"><span data-stu-id="c249b-110">You want to look for any run that does not show a success status.</span></span> <span data-ttu-id="c249b-111">Řazení kliknutím na záhlaví, můžete změnit.</span><span class="sxs-lookup"><span data-stu-id="c249b-111">You can change the sorting by clicking the headers.</span></span>

<span data-ttu-id="c249b-112">**Stav** je nejdůležitější informace a ukazuje nejzávažnějšího problém pro spuštění.</span><span class="sxs-lookup"><span data-stu-id="c249b-112">The **Status** column is the most important information and shows the most severe problem for a run.</span></span> <span data-ttu-id="c249b-113">Zde je stručný nejběžnější stavů v pořadí podle priority prozkoumat (kde * znamenat několik řetězců možná chyba).</span><span class="sxs-lookup"><span data-stu-id="c249b-113">Here is a quick summary of the most common statuses in order of priority to investigate (where * indicate several possible error strings).</span></span>

| <span data-ttu-id="c249b-114">Status</span><span class="sxs-lookup"><span data-stu-id="c249b-114">Status</span></span> | <span data-ttu-id="c249b-115">Komentář</span><span class="sxs-lookup"><span data-stu-id="c249b-115">Comment</span></span> |
| --- | --- |
| <span data-ttu-id="c249b-116">ukončeno-*</span><span class="sxs-lookup"><span data-stu-id="c249b-116">stopped-*</span></span> |<span data-ttu-id="c249b-117">Spuštění se nepodařilo dokončit.</span><span class="sxs-lookup"><span data-stu-id="c249b-117">The run could not complete.</span></span> <span data-ttu-id="c249b-118">Například pokud vzdálený systém je vypnutý a nelze kontaktovat.</span><span class="sxs-lookup"><span data-stu-id="c249b-118">For example, if the remote system is down and cannot be contacted.</span></span> |
| <span data-ttu-id="c249b-119">Zastavit limit chyb</span><span class="sxs-lookup"><span data-stu-id="c249b-119">stopped-error-limit</span></span> |<span data-ttu-id="c249b-120">Existuje více než 5 000 chyby.</span><span class="sxs-lookup"><span data-stu-id="c249b-120">There are more than 5,000 errors.</span></span> <span data-ttu-id="c249b-121">Spustit automaticky zastavila z důvodu velkého počtu chyb.</span><span class="sxs-lookup"><span data-stu-id="c249b-121">The run was automatically stopped due to the large number of errors.</span></span> |
| <span data-ttu-id="c249b-122">dokončené -\*– chyby</span><span class="sxs-lookup"><span data-stu-id="c249b-122">completed-\*-errors</span></span> |<span data-ttu-id="c249b-123">Spustit dokončilo, ale nejsou chyby (méně než 5 000), které by se měly prozkoumat.</span><span class="sxs-lookup"><span data-stu-id="c249b-123">The run completed, but there are errors (fewer than 5,000) that should be investigated.</span></span> |
| <span data-ttu-id="c249b-124">dokončené -\*– upozornění</span><span class="sxs-lookup"><span data-stu-id="c249b-124">completed-\*-warnings</span></span> |<span data-ttu-id="c249b-125">Spustit dokončena, ale některá data, není v očekávaném stavu.</span><span class="sxs-lookup"><span data-stu-id="c249b-125">The run completed, but some data is not in the expected state.</span></span> <span data-ttu-id="c249b-126">Pokud máte chyby, pak tato zpráva je obvykle jenom příznakem.</span><span class="sxs-lookup"><span data-stu-id="c249b-126">If you have errors, then this message is usually only a symptom.</span></span> <span data-ttu-id="c249b-127">Dokud nebude mít řešit chyby, by neměl prozkoumat upozornění.</span><span class="sxs-lookup"><span data-stu-id="c249b-127">Until you have addressed errors, you should not investigate warnings.</span></span> |
| <span data-ttu-id="c249b-128">úspěch</span><span class="sxs-lookup"><span data-stu-id="c249b-128">success</span></span> |<span data-ttu-id="c249b-129">Žádné problémy.</span><span class="sxs-lookup"><span data-stu-id="c249b-129">No issues.</span></span> |

<span data-ttu-id="c249b-130">Když vyberete řádek, aktualizuje dolní zobrazit podrobnosti, které používají.</span><span class="sxs-lookup"><span data-stu-id="c249b-130">When you select a row, the bottom updates to show the details of that run.</span></span> <span data-ttu-id="c249b-131">Na levém okraji dolní, můžete mít tom, že seznam **krok #**.</span><span class="sxs-lookup"><span data-stu-id="c249b-131">To the far left of the bottom, you might have a list saying **Step #**.</span></span> <span data-ttu-id="c249b-132">Tento seznam se zobrazí, jenom Pokud máte více domén v doménové struktuře každou doménu, kde je reprezentována krok.</span><span class="sxs-lookup"><span data-stu-id="c249b-132">This list only appears if you have multiple domains in your forest where each domain is represented by a step.</span></span> <span data-ttu-id="c249b-133">Název domény naleznete v části **oddílu**.</span><span class="sxs-lookup"><span data-stu-id="c249b-133">The domain name can be found under the heading **Partition**.</span></span> <span data-ttu-id="c249b-134">V části **Statistika synchronizace**, můžete najít další informace o počtu změn, které byly zpracovány.</span><span class="sxs-lookup"><span data-stu-id="c249b-134">Under **Synchronization Statistics**, you can find more information about the number of changes that were processed.</span></span> <span data-ttu-id="c249b-135">Můžete kliknutím na odkazy získat seznam změněných objektů.</span><span class="sxs-lookup"><span data-stu-id="c249b-135">You can click the links to get a list of the changed objects.</span></span> <span data-ttu-id="c249b-136">Pokud máte objektů s chybami, tyto chyby, zobrazí na **chyby synchronizace**.</span><span class="sxs-lookup"><span data-stu-id="c249b-136">If you have objects with errors, those errors show up under **Synchronization Errors**.</span></span>

<span data-ttu-id="c249b-137">Další informace najdete v tématu [řešení potíží s objekt, který není synchronizace](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md)</span><span class="sxs-lookup"><span data-stu-id="c249b-137">For more information, see [troubleshoot an object that is not synchronizing](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md)</span></span>

## <a name="next-steps"></a><span data-ttu-id="c249b-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c249b-138">Next steps</span></span>
<span data-ttu-id="c249b-139">Další informace o [synchronizace Azure AD Connect](active-directory-aadconnectsync-whatis.md) konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c249b-139">Learn more about the [Azure AD Connect sync](active-directory-aadconnectsync-whatis.md) configuration.</span></span>

<span data-ttu-id="c249b-140">Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="c249b-140">Learn more about [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
