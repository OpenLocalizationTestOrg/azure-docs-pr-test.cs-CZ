---
title: "Operace Správce synchronizace služby Azure AD Connect | Microsoft Docs"
description: "Porozumět hello kartu operace v hello Synchronization Service Manager pro Azure AD Connect."
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
ms.openlocfilehash: decbc53613d456a71cd116c40c5e1fd761efd4af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-sync-service-manager-operations-tab"></a><span data-ttu-id="90f75-103">Pomocí hello kartu operace synchronizace Service Manager</span><span class="sxs-lookup"><span data-stu-id="90f75-103">Using hello Sync Service Manager Operations tab</span></span>

![Správce synchronizace služby](./media/active-directory-aadconnectsync-service-manager-ui/operations.png)

<span data-ttu-id="90f75-105">Hello operations kartě se zobrazují hello výsledky z nejnovější operace hello.</span><span class="sxs-lookup"><span data-stu-id="90f75-105">hello operations tab shows hello results from hello most recent operations.</span></span> <span data-ttu-id="90f75-106">Na této kartě je klíče toounderstand a řešení problémů.</span><span class="sxs-lookup"><span data-stu-id="90f75-106">This tab is key toounderstand and troubleshoot issues.</span></span>

## <a name="understand-hello-information-visible-in-hello-operations-tab"></a><span data-ttu-id="90f75-107">Pochopení hello informace zobrazené na kartě operations hello</span><span class="sxs-lookup"><span data-stu-id="90f75-107">Understand hello information visible in hello operations tab</span></span>
<span data-ttu-id="90f75-108">horní polovině Hello ukazuje všechny běží v chronologickém pořadí.</span><span class="sxs-lookup"><span data-stu-id="90f75-108">hello top half shows all runs in chronological order.</span></span> <span data-ttu-id="90f75-109">Ve výchozím nastavení, protokol operations hello udržuje informace o hello posledních sedmi dnů, ale toto nastavení lze změnit pomocí hello [Plánovač](active-directory-aadconnectsync-feature-scheduler.md).</span><span class="sxs-lookup"><span data-stu-id="90f75-109">By default, hello operations log keeps information about hello last seven days, but this setting can be changed with hello [scheduler](active-directory-aadconnectsync-feature-scheduler.md).</span></span> <span data-ttu-id="90f75-110">Chcete toolook pro všechny spuštění, které nejsou zobrazeny stav úspěšného dokončení.</span><span class="sxs-lookup"><span data-stu-id="90f75-110">You want toolook for any run that does not show a success status.</span></span> <span data-ttu-id="90f75-111">Můžete změnit hello řazení kliknutím na záhlaví hello.</span><span class="sxs-lookup"><span data-stu-id="90f75-111">You can change hello sorting by clicking hello headers.</span></span>

<span data-ttu-id="90f75-112">Hello **stav** sloupec je nejdůležitější informace hello a ukazuje hello nejzávažnějšího problém pro spuštění.</span><span class="sxs-lookup"><span data-stu-id="90f75-112">hello **Status** column is hello most important information and shows hello most severe problem for a run.</span></span> <span data-ttu-id="90f75-113">Zde je stručný hello nejběžnější stavů v pořadí podle priority tooinvestigate (kde * znamenat několik řetězců možná chyba).</span><span class="sxs-lookup"><span data-stu-id="90f75-113">Here is a quick summary of hello most common statuses in order of priority tooinvestigate (where * indicate several possible error strings).</span></span>

| <span data-ttu-id="90f75-114">Status</span><span class="sxs-lookup"><span data-stu-id="90f75-114">Status</span></span> | <span data-ttu-id="90f75-115">Komentář</span><span class="sxs-lookup"><span data-stu-id="90f75-115">Comment</span></span> |
| --- | --- |
| <span data-ttu-id="90f75-116">ukončeno-*</span><span class="sxs-lookup"><span data-stu-id="90f75-116">stopped-*</span></span> |<span data-ttu-id="90f75-117">Hello spuštění se nepodařilo dokončit.</span><span class="sxs-lookup"><span data-stu-id="90f75-117">hello run could not complete.</span></span> <span data-ttu-id="90f75-118">Například pokud hello vzdálený systém je vypnutý a nelze kontaktovat.</span><span class="sxs-lookup"><span data-stu-id="90f75-118">For example, if hello remote system is down and cannot be contacted.</span></span> |
| <span data-ttu-id="90f75-119">Zastavit limit chyb</span><span class="sxs-lookup"><span data-stu-id="90f75-119">stopped-error-limit</span></span> |<span data-ttu-id="90f75-120">Existuje více než 5 000 chyby.</span><span class="sxs-lookup"><span data-stu-id="90f75-120">There are more than 5,000 errors.</span></span> <span data-ttu-id="90f75-121">Hello spustit automaticky zastavil z důvodu toohello velký počet chyb.</span><span class="sxs-lookup"><span data-stu-id="90f75-121">hello run was automatically stopped due toohello large number of errors.</span></span> |
| <span data-ttu-id="90f75-122">dokončené -\*– chyby</span><span class="sxs-lookup"><span data-stu-id="90f75-122">completed-\*-errors</span></span> |<span data-ttu-id="90f75-123">Hello spustit dokončené, ale nejsou chyby (méně než 5 000), které by se měly prozkoumat.</span><span class="sxs-lookup"><span data-stu-id="90f75-123">hello run completed, but there are errors (fewer than 5,000) that should be investigated.</span></span> |
| <span data-ttu-id="90f75-124">dokončené -\*– upozornění</span><span class="sxs-lookup"><span data-stu-id="90f75-124">completed-\*-warnings</span></span> |<span data-ttu-id="90f75-125">Hello spustit dokončilo, ale některá data se nenachází ve stavu hello očekává.</span><span class="sxs-lookup"><span data-stu-id="90f75-125">hello run completed, but some data is not in hello expected state.</span></span> <span data-ttu-id="90f75-126">Pokud máte chyby, pak tato zpráva je obvykle jenom příznakem.</span><span class="sxs-lookup"><span data-stu-id="90f75-126">If you have errors, then this message is usually only a symptom.</span></span> <span data-ttu-id="90f75-127">Dokud nebude mít řešit chyby, by neměl prozkoumat upozornění.</span><span class="sxs-lookup"><span data-stu-id="90f75-127">Until you have addressed errors, you should not investigate warnings.</span></span> |
| <span data-ttu-id="90f75-128">úspěch</span><span class="sxs-lookup"><span data-stu-id="90f75-128">success</span></span> |<span data-ttu-id="90f75-129">Žádné problémy.</span><span class="sxs-lookup"><span data-stu-id="90f75-129">No issues.</span></span> |

<span data-ttu-id="90f75-130">Když vyberete řádek, aktualizuje hello dolní tooshow hello podrobnosti této spustit.</span><span class="sxs-lookup"><span data-stu-id="90f75-130">When you select a row, hello bottom updates tooshow hello details of that run.</span></span> <span data-ttu-id="90f75-131">toohello daleko pravé dolní hello, můžete mít tom, že seznam **krok #**.</span><span class="sxs-lookup"><span data-stu-id="90f75-131">toohello far left of hello bottom, you might have a list saying **Step #**.</span></span> <span data-ttu-id="90f75-132">Tento seznam se zobrazí, jenom Pokud máte více domén v doménové struktuře každou doménu, kde je reprezentována krok.</span><span class="sxs-lookup"><span data-stu-id="90f75-132">This list only appears if you have multiple domains in your forest where each domain is represented by a step.</span></span> <span data-ttu-id="90f75-133">název domény Hello naleznete pod nadpisem hello **oddílu**.</span><span class="sxs-lookup"><span data-stu-id="90f75-133">hello domain name can be found under hello heading **Partition**.</span></span> <span data-ttu-id="90f75-134">V části **Statistika synchronizace**, můžete najít další informace o hello počet změn, které byly zpracovány.</span><span class="sxs-lookup"><span data-stu-id="90f75-134">Under **Synchronization Statistics**, you can find more information about hello number of changes that were processed.</span></span> <span data-ttu-id="90f75-135">Můžete kliknout na hello odkazy tooget seznam objektů hello změnit.</span><span class="sxs-lookup"><span data-stu-id="90f75-135">You can click hello links tooget a list of hello changed objects.</span></span> <span data-ttu-id="90f75-136">Pokud máte objektů s chybami, tyto chyby, zobrazí na **chyby synchronizace**.</span><span class="sxs-lookup"><span data-stu-id="90f75-136">If you have objects with errors, those errors show up under **Synchronization Errors**.</span></span>

<span data-ttu-id="90f75-137">Další informace najdete v tématu [řešení potíží s objekt, který není synchronizace](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md)</span><span class="sxs-lookup"><span data-stu-id="90f75-137">For more information, see [troubleshoot an object that is not synchronizing](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md)</span></span>

## <a name="next-steps"></a><span data-ttu-id="90f75-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="90f75-138">Next steps</span></span>
<span data-ttu-id="90f75-139">Další informace o hello [synchronizace Azure AD Connect](active-directory-aadconnectsync-whatis.md) konfigurace.</span><span class="sxs-lookup"><span data-stu-id="90f75-139">Learn more about hello [Azure AD Connect sync](active-directory-aadconnectsync-whatis.md) configuration.</span></span>

<span data-ttu-id="90f75-140">Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="90f75-140">Learn more about [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
