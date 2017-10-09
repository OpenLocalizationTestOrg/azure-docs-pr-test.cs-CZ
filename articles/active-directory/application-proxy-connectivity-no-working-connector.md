---
title: "Najít aaaNo pracovní konektor skupinu pro aplikaci Proxy aplikace | Microsoft Docs"
description: "Řešení problémů, které se můžete setkat, když není žádný funkční konektor ve skupině konektor pro vaši aplikaci s hello proxy aplikace služby Azure AD"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 4c4baf296b316db131929c9a7c618fb9960713e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="no-working-connector-group-found-for-an-application-proxy-application"></a><span data-ttu-id="cf9c2-103">Žádná pracovní skupina konektor najít pro aplikaci Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="cf9c2-103">No working connector group found for an Application Proxy application</span></span>

<span data-ttu-id="cf9c2-104">Tento článek nápovědy vám tooresolve hello běžné problémy potýkají při konektor pro aplikaci Proxy aplikace zjištěn není integrované s Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="cf9c2-104">This article help you tooresolve hello common issues faced when there is not a connector detected for an Application Proxy application integrated with Azure Active Directory.</span></span>

## <a name="overview-of-steps"></a><span data-ttu-id="cf9c2-105">Přehled kroků</span><span class="sxs-lookup"><span data-stu-id="cf9c2-105">Overview of steps</span></span>
<span data-ttu-id="cf9c2-106">Pokud není žádný funkční konektor ve skupině konektor pro vaši aplikaci, existuje několik způsobů tooresolve hello problému:</span><span class="sxs-lookup"><span data-stu-id="cf9c2-106">If there is no working Connector in a Connector Group for your application, there are a few ways tooresolve hello problem:</span></span>

-   <span data-ttu-id="cf9c2-107">Pokud máte ve skupině hello žádné konektory, můžete:</span><span class="sxs-lookup"><span data-stu-id="cf9c2-107">If you have no connectors in hello group, you can:</span></span>

    -   <span data-ttu-id="cf9c2-108">Stáhněte si nový konektor na hello správné místního serveru a přiřaďte ho toothis skupiny</span><span class="sxs-lookup"><span data-stu-id="cf9c2-108">Download a new Connector on hello right on-prem server, and assign it toothis group</span></span>

    -   <span data-ttu-id="cf9c2-109">Konektor služby active přesunout do skupiny hello</span><span class="sxs-lookup"><span data-stu-id="cf9c2-109">Move an active Connector into hello group</span></span>

-   <span data-ttu-id="cf9c2-110">Pokud máte ve skupině hello žádné aktivní konektory, můžete:</span><span class="sxs-lookup"><span data-stu-id="cf9c2-110">If you have no active connectors in hello group, you can:</span></span>

    -   <span data-ttu-id="cf9c2-111">Vaše konektor je neaktivní hello příčiny identifikovat a vyřešit</span><span class="sxs-lookup"><span data-stu-id="cf9c2-111">Identify hello reason your Connector is inactive and resolve</span></span>

    -   <span data-ttu-id="cf9c2-112">Konektor služby active přesunout do skupiny hello</span><span class="sxs-lookup"><span data-stu-id="cf9c2-112">Move an active Connector into hello group</span></span>

<span data-ttu-id="cf9c2-113">tooknow, který z nich je hello problém, otevřete nabídku "Proxy aplikace" hello ve vaší aplikaci a podívejte se na uvítací zprávu upozornění konektor skupiny.</span><span class="sxs-lookup"><span data-stu-id="cf9c2-113">tooknow which of these is hello issue, open hello “Application Proxy” menu in your Application, and look at hello Connector Group warning message.</span></span> <span data-ttu-id="cf9c2-114">Zadejte ho této skupiny hello musí mít alespoň jeden konektor (nemáte žádné ve skupině hello) nebo že nemá žádné aktivní konektory (i když máte pravděpodobně neaktivní konektory).</span><span class="sxs-lookup"><span data-stu-id="cf9c2-114">It specify either that hello group needs at least one Connector (you have none in hello group) or that it has no active Connectors (though you likely have inactive Connectors).</span></span>

   ![Výběr skupiny konektor portálu Azure](./media/application-proxy-connectivity-no-working-connector/no-active-connector.png)

<span data-ttu-id="cf9c2-116">Podrobnosti na každém z těchto možností najdete v tématu hello odpovídající části.</span><span class="sxs-lookup"><span data-stu-id="cf9c2-116">For details on each of these options, see hello corresponding section below.</span></span> <span data-ttu-id="cf9c2-117">Každá z těchto předpokládá, že začínáte ze stránky správy konektor hello.</span><span class="sxs-lookup"><span data-stu-id="cf9c2-117">Each of these assumes that you are starting from hello Connector management page.</span></span> <span data-ttu-id="cf9c2-118">Pokud se díváte hello chybová zpráva výše, můžete přejít toothis stránku kliknutím na uvítací zprávu upozornění.</span><span class="sxs-lookup"><span data-stu-id="cf9c2-118">If you are looking at hello error message above, you can go toothis page by clicking on hello warning message.</span></span> <span data-ttu-id="cf9c2-119">V opačném případě lze najít tak, že přejdete příliš**Azure Active Directory**, kliknutím na na **podnikové aplikace, které**, pak **Proxy aplikace.**</span><span class="sxs-lookup"><span data-stu-id="cf9c2-119">Otherwise this can be found by going too**Azure Active Directory**, clicking on **Enterprise Applications**, then **Application Proxy.**</span></span>

   ![Správa skupin konektor portálu Azure](./media/application-proxy-connectivity-no-working-connector/app-proxy.png)

## <a name="download-a-new-connector"></a><span data-ttu-id="cf9c2-121">Stáhněte si nový konektor</span><span class="sxs-lookup"><span data-stu-id="cf9c2-121">Download a new Connector</span></span>

<span data-ttu-id="cf9c2-122">toodownload nový konektor, použijte tlačítko "Stáhnout konektor" hello hello horní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="cf9c2-122">toodownload a new Connector, use hello “Download Connector” button at hello top of hello page.</span></span>

<span data-ttu-id="cf9c2-123">hello Poznámka hello konektor potřebám toobe nainstalovaná na počítači s přímé směrem pohledu toohello back-end aplikace a je obvykle umístěny na stejném serveru jako aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="cf9c2-123">note hello Connector needs toobe installed on a machine with direct line of sight toohello backend application, and is typically placed on hello same server as hello application.</span></span> <span data-ttu-id="cf9c2-124">Po stažení, by hello konektor zobrazit v této nabídky.</span><span class="sxs-lookup"><span data-stu-id="cf9c2-124">After downloading, hello Connector should appear in this menu.</span></span> <span data-ttu-id="cf9c2-125">Klikněte na hello konektoru a použít toomake rozevíracího seznamu "Konektor skupinu" hello se, že patří toohello správné skupiny.</span><span class="sxs-lookup"><span data-stu-id="cf9c2-125">click hello Connector, and use hello “Connector Group” drop-down toomake sure it belongs toohello right group.</span></span> <span data-ttu-id="cf9c2-126">Uložte změnu hello.</span><span class="sxs-lookup"><span data-stu-id="cf9c2-126">Save hello change.</span></span>

   ![Stáhnout hello konektor z hello portálu Azure](./media/application-proxy-connectivity-no-working-connector/download-connector.png)
   
## <a name="move-an-active-connector"></a><span data-ttu-id="cf9c2-128">Přesunout konektoru služby Active</span><span class="sxs-lookup"><span data-stu-id="cf9c2-128">Move an Active Connector</span></span>

<span data-ttu-id="cf9c2-129">Pokud máte active konektor, který by měly patřit toohello skupiny a má směrem pohledu toohello cílová back-end aplikace, můžete přesunout hello konektor do skupiny hello přiřazen.</span><span class="sxs-lookup"><span data-stu-id="cf9c2-129">If you have an active Connector that should belong toohello group and has line of sight toohello target backend application, you can move hello Connector into hello assigned group.</span></span> <span data-ttu-id="cf9c2-130">toodo proto, klikněte na hello konektor.</span><span class="sxs-lookup"><span data-stu-id="cf9c2-130">toodo so, click hello Connector.</span></span> <span data-ttu-id="cf9c2-131">V poli "Konektor skupinu" hello použijte správnou skupinu hello hello tooselect rozevíracího seznamu a klikněte na Uložit.</span><span class="sxs-lookup"><span data-stu-id="cf9c2-131">In hello “Connector Group” field, use hello drop-down tooselect hello correct group, and click Save.</span></span>

## <a name="resolve-an-inactive-connector"></a><span data-ttu-id="cf9c2-132">Vyřešte neaktivní konektoru</span><span class="sxs-lookup"><span data-stu-id="cf9c2-132">Resolve an inactive Connector</span></span>

<span data-ttu-id="cf9c2-133">Pokud hello konektory ve skupině hello jsou neaktivní, pouze je pravděpodobné, že na počítači, který nemá mít všechny porty potřebné hello odblokováno.</span><span class="sxs-lookup"><span data-stu-id="cf9c2-133">If hello only Connectors in hello group are inactive, they are likely on a machine that does not have all hello necessary ports unblocked.</span></span>

<span data-ttu-id="cf9c2-134">Viz hello porty Poradce při potížích dokumentu podrobnosti na odstranění příčin tohoto problému.</span><span class="sxs-lookup"><span data-stu-id="cf9c2-134">see hello ports Troubleshoot document for details on investigating this problem.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cf9c2-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cf9c2-135">Next steps</span></span>
[<span data-ttu-id="cf9c2-136">Pochopení konektory proxy aplikace služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="cf9c2-136">Understand Azure AD Application Proxy connectors</span></span>](application-proxy-understand-connectors.md)


