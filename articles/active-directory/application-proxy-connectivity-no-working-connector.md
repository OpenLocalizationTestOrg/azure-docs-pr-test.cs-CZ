---
title: "Pro aplikaci Proxy aplikace nalezeny žádná skupina konektor pracovní | Microsoft Docs"
description: "Řešení problémů, které se můžete setkat, když není žádný funkční konektor ve skupině pro vaši aplikaci s Azure AD Application Proxy Connector"
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
ms.openlocfilehash: 4945958deedc8a1d9989ff901192c03a5363b4dc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="no-working-connector-group-found-for-an-application-proxy-application"></a><span data-ttu-id="da4cf-103">Žádná pracovní skupina konektor najít pro aplikaci Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="da4cf-103">No working connector group found for an Application Proxy application</span></span>

<span data-ttu-id="da4cf-104">Tento článek nápovědy, můžete použít k řešení běžných problémů potýkají při konektor pro aplikaci Proxy aplikace zjištěn není integrované s Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="da4cf-104">This article help you to resolve the common issues faced when there is not a connector detected for an Application Proxy application integrated with Azure Active Directory.</span></span>

## <a name="overview-of-steps"></a><span data-ttu-id="da4cf-105">Přehled kroků</span><span class="sxs-lookup"><span data-stu-id="da4cf-105">Overview of steps</span></span>
<span data-ttu-id="da4cf-106">Pokud není žádný funkční konektor ve skupině konektor pro vaši aplikaci, existuje několik způsobů, jak problém vyřešit:</span><span class="sxs-lookup"><span data-stu-id="da4cf-106">If there is no working Connector in a Connector Group for your application, there are a few ways to resolve the problem:</span></span>

-   <span data-ttu-id="da4cf-107">Pokud máte žádné konektory ve skupině, můžete:</span><span class="sxs-lookup"><span data-stu-id="da4cf-107">If you have no connectors in the group, you can:</span></span>

    -   <span data-ttu-id="da4cf-108">Stáhněte si nový konektor na pravém místního serveru a přiřaďte ho do této skupiny</span><span class="sxs-lookup"><span data-stu-id="da4cf-108">Download a new Connector on the right on-prem server, and assign it to this group</span></span>

    -   <span data-ttu-id="da4cf-109">Konektor služby active přesuňte do skupiny</span><span class="sxs-lookup"><span data-stu-id="da4cf-109">Move an active Connector into the group</span></span>

-   <span data-ttu-id="da4cf-110">Pokud máte žádné aktivní konektory ve skupině, můžete:</span><span class="sxs-lookup"><span data-stu-id="da4cf-110">If you have no active connectors in the group, you can:</span></span>

    -   <span data-ttu-id="da4cf-111">Z důvodu, kterou vaše konektor je neaktivní identifikovat a vyřešit</span><span class="sxs-lookup"><span data-stu-id="da4cf-111">Identify the reason your Connector is inactive and resolve</span></span>

    -   <span data-ttu-id="da4cf-112">Konektor služby active přesuňte do skupiny</span><span class="sxs-lookup"><span data-stu-id="da4cf-112">Move an active Connector into the group</span></span>

<span data-ttu-id="da4cf-113">Potřebujete vědět, který z nich je problém, otevřete nabídku "Proxy aplikace" v aplikaci a podívejte se na zprávu upozornění konektor skupiny.</span><span class="sxs-lookup"><span data-stu-id="da4cf-113">To know which of these is the issue, open the “Application Proxy” menu in your Application, and look at the Connector Group warning message.</span></span> <span data-ttu-id="da4cf-114">Ho zadat, že skupině potřebuje alespoň jeden konektor (nemáte žádné ve skupině), nebo že nemá žádné aktivní konektory (i když máte pravděpodobně neaktivní konektory).</span><span class="sxs-lookup"><span data-stu-id="da4cf-114">It specify either that the group needs at least one Connector (you have none in the group) or that it has no active Connectors (though you likely have inactive Connectors).</span></span>

   ![Výběr skupiny konektor portálu Azure](./media/application-proxy-connectivity-no-working-connector/no-active-connector.png)

<span data-ttu-id="da4cf-116">Podrobnosti o každá z těchto možností najdete v části odpovídající níže.</span><span class="sxs-lookup"><span data-stu-id="da4cf-116">For details on each of these options, see the corresponding section below.</span></span> <span data-ttu-id="da4cf-117">Každá z těchto předpokládá, že začínáte na stránce Správa konektor.</span><span class="sxs-lookup"><span data-stu-id="da4cf-117">Each of these assumes that you are starting from the Connector management page.</span></span> <span data-ttu-id="da4cf-118">Pokud hledáte na výše uvedené chybová zpráva, můžete přejít na tuto stránku kliknutím na upozornění.</span><span class="sxs-lookup"><span data-stu-id="da4cf-118">If you are looking at the error message above, you can go to this page by clicking on the warning message.</span></span> <span data-ttu-id="da4cf-119">V opačném případě lze najít na webu **Azure Active Directory**, kliknutím na na **podnikové aplikace, které**, pak **Proxy aplikace.**</span><span class="sxs-lookup"><span data-stu-id="da4cf-119">Otherwise this can be found by going to **Azure Active Directory**, clicking on **Enterprise Applications**, then **Application Proxy.**</span></span>

   ![Správa skupin konektor portálu Azure](./media/application-proxy-connectivity-no-working-connector/app-proxy.png)

## <a name="download-a-new-connector"></a><span data-ttu-id="da4cf-121">Stáhněte si nový konektor</span><span class="sxs-lookup"><span data-stu-id="da4cf-121">Download a new Connector</span></span>

<span data-ttu-id="da4cf-122">Pokud chcete stáhnout nový konektor, použijte tlačítko "Stáhnout konektor" v horní části stránky.</span><span class="sxs-lookup"><span data-stu-id="da4cf-122">To download a new Connector, use the “Download Connector” button at the top of the page.</span></span>

<span data-ttu-id="da4cf-123">Poznámka: tento konektor musí být nainstalovaný na počítači s přímé směrem pohledu na back-end aplikace a je obvykle umístěny na stejném serveru jako aplikace.</span><span class="sxs-lookup"><span data-stu-id="da4cf-123">note the Connector needs to be installed on a machine with direct line of sight to the backend application, and is typically placed on the same server as the application.</span></span> <span data-ttu-id="da4cf-124">Po stažení, konektor objevit v této nabídky.</span><span class="sxs-lookup"><span data-stu-id="da4cf-124">After downloading, the Connector should appear in this menu.</span></span> <span data-ttu-id="da4cf-125">Klikněte na konektoru a ujistěte se, že patří do správné skupiny pomocí "Konektor skupinu" rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="da4cf-125">click the Connector, and use the “Connector Group” drop-down to make sure it belongs to the right group.</span></span> <span data-ttu-id="da4cf-126">Uložte změnu.</span><span class="sxs-lookup"><span data-stu-id="da4cf-126">Save the change.</span></span>

   ![Stáhnout konektor z portálu Azure](./media/application-proxy-connectivity-no-working-connector/download-connector.png)
   
## <a name="move-an-active-connector"></a><span data-ttu-id="da4cf-128">Přesunout konektoru služby Active</span><span class="sxs-lookup"><span data-stu-id="da4cf-128">Move an Active Connector</span></span>

<span data-ttu-id="da4cf-129">Pokud máte aktivní konektor, musí patřit do skupiny a má směrem pohledu do cílové back-end aplikace, můžete konektor přesunout do skupiny přiřazené.</span><span class="sxs-lookup"><span data-stu-id="da4cf-129">If you have an active Connector that should belong to the group and has line of sight to the target backend application, you can move the Connector into the assigned group.</span></span> <span data-ttu-id="da4cf-130">Chcete-li to provést, klikněte na tlačítko konektor.</span><span class="sxs-lookup"><span data-stu-id="da4cf-130">To do so, click the Connector.</span></span> <span data-ttu-id="da4cf-131">V poli "Konektor skupina" pomocí rozevíracího seznamu vyberte správné skupině, a klikněte na Uložit.</span><span class="sxs-lookup"><span data-stu-id="da4cf-131">In the “Connector Group” field, use the drop-down to select the correct group, and click Save.</span></span>

## <a name="resolve-an-inactive-connector"></a><span data-ttu-id="da4cf-132">Vyřešte neaktivní konektoru</span><span class="sxs-lookup"><span data-stu-id="da4cf-132">Resolve an inactive Connector</span></span>

<span data-ttu-id="da4cf-133">Pokud se pouze konektory ve skupině jsou neaktivní, jsou pravděpodobně na počítači, který nemá všechny nezbytné porty odblokováno.</span><span class="sxs-lookup"><span data-stu-id="da4cf-133">If the only Connectors in the group are inactive, they are likely on a machine that does not have all the necessary ports unblocked.</span></span>

<span data-ttu-id="da4cf-134">na odstranění příčin tohoto problému naleznete v dokumentu Poradce při potížích porty podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="da4cf-134">see the ports Troubleshoot document for details on investigating this problem.</span></span>

## <a name="next-steps"></a><span data-ttu-id="da4cf-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="da4cf-135">Next steps</span></span>
[<span data-ttu-id="da4cf-136">Pochopení konektory proxy aplikace služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="da4cf-136">Understand Azure AD Application Proxy connectors</span></span>](application-proxy-understand-connectors.md)


