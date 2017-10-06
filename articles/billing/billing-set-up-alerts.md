---
title: "aaaSet až fakturace nebo platební výstrahy pro předplatná Azure | Microsoft Docs"
description: "Popisuje, jak můžete na můžete nastavit výstrahy faktury Azure, se můžete vyhnout fakturace výskyt nečekaných událostí."
keywords: "platební výstraze, fakturace výstrahu"
services: 
documentationcenter: 
author: vikdesai
manager: tonguyen
editor: 
tags: billing
ms.assetid: 9b7b3eeb-cd9d-4690-86a3-51b1e2a8974f
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2017
ms.author: vikdesai
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 711b9c72c59874792b0e229cdc5ec0fa517c24c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-billing-or-credit-alerts-for-your-microsoft-azure-subscriptions"></a><span data-ttu-id="8ab6d-104">Nastavení výstrah fakturace nebo platební pro vaše předplatné Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="8ab6d-104">Set up billing or credit alerts for your Microsoft Azure subscriptions</span></span>
<span data-ttu-id="8ab6d-105">Pokud jste hello správce účtů pro předplatné Azure, můžete použít hello fakturace výstrah služby Azure toocreate přizpůsobit fakturace výstrahy, které pomáhají sledovat a spravovat fakturace aktivity pro vaše účty Azure.</span><span class="sxs-lookup"><span data-stu-id="8ab6d-105">If you’re hello Account Admin for an Azure subscription, you can use hello Azure Billing Alert Service toocreate customized billing alerts that help you monitor and manage billing activity for your Azure accounts.</span></span>

<span data-ttu-id="8ab6d-106">Tato služba je ve verzi preview, proto musíte tooenable ho na stránce funkce verze Preview hello nejdřív.</span><span class="sxs-lookup"><span data-stu-id="8ab6d-106">This service is in preview, so you need tooenable it in hello Preview Features page first.</span></span>

## <a name="set-hello-alert-threshold-and-email-recipients"></a><span data-ttu-id="8ab6d-107">Nastavit výstrahy prahovou hodnotu a e-mailu příjemce hello</span><span class="sxs-lookup"><span data-stu-id="8ab6d-107">Set hello alert threshold and email recipients</span></span>
1. <span data-ttu-id="8ab6d-108">Navštivte [stránku hello funkce Preview](https://account.windowsazure.com/PreviewFeatures) a povolte **fakturace výstrah služby**.</span><span class="sxs-lookup"><span data-stu-id="8ab6d-108">Visit [hello Preview Features page](https://account.windowsazure.com/PreviewFeatures) and enable **Billing Alert Service**.</span></span>

1. <span data-ttu-id="8ab6d-109">Po obdržení hello e-mailu potvrzení, že je zapnutý hello fakturace služby pro vaše předplatné, navštivte [stránku hello odběry](https://account.windowsazure.com/Subscriptions) hello portálu účtů.</span><span class="sxs-lookup"><span data-stu-id="8ab6d-109">After you receive hello email confirmation that hello billing service is turned on for your subscription, visit [hello Subscriptions page](https://account.windowsazure.com/Subscriptions) in hello account portal.</span></span> <span data-ttu-id="8ab6d-110">Klikněte na předplatné hello toomonitor a pak klikněte na **výstrahy**.</span><span class="sxs-lookup"><span data-stu-id="8ab6d-110">Click hello subscription you want toomonitor, and then click **Alerts**.</span></span>

    ![Snímek obrazovky zobrazení odběry hello centra účtů Azure, se zvýrazněnou výstrahy][Image1]

2. <span data-ttu-id="8ab6d-112">Klikněte na tlačítko **přidat výstrahy** toocreate vaše první z nich.</span><span class="sxs-lookup"><span data-stu-id="8ab6d-112">Next, click **Add Alert** toocreate your first one.</span></span> <span data-ttu-id="8ab6d-113">Můžete nastavit celkem pět fakturace výstrahy na předplatné s různé prahové hodnoty a až tootwo příjemců e-mailu pro každou výstrahu.</span><span class="sxs-lookup"><span data-stu-id="8ab6d-113">You can set up a total of five billing alerts per subscription, with a different threshold and up tootwo email recipients for each alert.</span></span>

    ![Snímek obrazovky hello zobrazení výstrahy, kde můžete přidat výstrahy][Image2]

3. <span data-ttu-id="8ab6d-115">Přidáte-li výstrahu, můžete zadat jedinečný název, zvolte útraty prahovou hodnotu a vyberte hello e-mailové adresy, které jsou odesílány výstrahy.</span><span class="sxs-lookup"><span data-stu-id="8ab6d-115">When you add an alert, you give it a unique name, choose a spending threshold, and choose hello email addresses where alerts are sent.</span></span> <span data-ttu-id="8ab6d-116">Při nastavování hello prahovou hodnotu, můžete buď **fakturace celkový** nebo **peněžního kreditu, který** z hello **výstrahy pro** seznamu.</span><span class="sxs-lookup"><span data-stu-id="8ab6d-116">When setting up hello threshold, you can choose either a **Billing Total** or a **Monetary Credit** from hello **Alert For** list.</span></span> <span data-ttu-id="8ab6d-117">Fakturace celkem zobrazí se výstraha Pokud předplatné výdaje překročí prahovou hodnotu hello.</span><span class="sxs-lookup"><span data-stu-id="8ab6d-117">For a billing total, an alert is sent when subscription spending exceeds hello threshold.</span></span> <span data-ttu-id="8ab6d-118">Pro peněžního kreditu zobrazí se výstraha při peněžní kredity neklesla pod hello limit.</span><span class="sxs-lookup"><span data-stu-id="8ab6d-118">For a monetary credit, an alert is sent when monetary credits drop below hello limit.</span></span> <span data-ttu-id="8ab6d-119">Peněžní kredity obvykle platí odběry tooFree zkušební verze a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8ab6d-119">Monetary credits usually apply tooFree Trial and Visual Studio subscriptions.</span></span>

    ![Snímek obrazovky zobrazení výstrah přidání hello, kde můžete konfigurovat příjemce][Image3]

<span data-ttu-id="8ab6d-121">Azure podporuje všechny e-mailovou adresu, ale není ověřte, že funguje hello e-mailovou adresu, proto tuto možnost pro překlepům.</span><span class="sxs-lookup"><span data-stu-id="8ab6d-121">Azure supports any email address but doesn't verify that hello email address works, so double-check for typos.</span></span>

## <a name="check-on-your-alerts"></a><span data-ttu-id="8ab6d-122">Zkontrolujte na upozornění</span><span class="sxs-lookup"><span data-stu-id="8ab6d-122">Check on your alerts</span></span>
<span data-ttu-id="8ab6d-123">Po nastavení výstrahy hello centra účtů je uvádí a ukazuje, kolik více můžete nastavit.</span><span class="sxs-lookup"><span data-stu-id="8ab6d-123">After you set up alerts, hello Account Center lists them and shows how many more you can set up.</span></span> <span data-ttu-id="8ab6d-124">Pro každou výstrahu zobrazí hello datum a čas, který byl odeslán, jestli je výstraha fakturace celkový nebo peněžního kreditu, který a hello limit, kterou vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="8ab6d-124">For each alert, you see hello date and time it was sent, whether it’s an alert for Billing Total or Monetary Credit, and hello limit you set up.</span></span> <span data-ttu-id="8ab6d-125">Datum hello je formát rrrr mm-dd Hello formát data a času je 24 hodin koordinaci světový čas (UTC).</span><span class="sxs-lookup"><span data-stu-id="8ab6d-125">hello date and time format is 24-hour Universal Time Coordinate (UTC) and hello date is yyyy-mm-dd format.</span></span> <span data-ttu-id="8ab6d-126">Klikněte na tlačítko hello plus přihlášení pro výstrahu v seznamu tooedit hello nebo klikněte na hello odpadkového koše toodelete ho.</span><span class="sxs-lookup"><span data-stu-id="8ab6d-126">Click hello plus sign for an alert in hello list tooedit it, or click hello trash-can toodelete it.</span></span>

## <a name="billing-alerts-for-enterprise-agreement-ea-customers"></a><span data-ttu-id="8ab6d-127">Výstrahy fakturace pro zákazníky Enterprise Agreement (EA)</span><span class="sxs-lookup"><span data-stu-id="8ab6d-127">Billing alerts for Enterprise Agreement (EA) customers</span></span>
<span data-ttu-id="8ab6d-128">Zákazníci EA můžete získat výstrahy pro každé oddělení v zápisu nastavení výdaje kvóty.</span><span class="sxs-lookup"><span data-stu-id="8ab6d-128">EA customers can get alerts for each department under an enrollment by setting spending quotas.</span></span> <span data-ttu-id="8ab6d-129">V tématu [oddělení výdaje kvóty](https://ea.azure.com/helpdocs/departmentSpendingQuotas) v hello EA portálu tooget spuštěna.</span><span class="sxs-lookup"><span data-stu-id="8ab6d-129">See [Department Spending Quotas](https://ea.azure.com/helpdocs/departmentSpendingQuotas) in hello EA portal tooget started.</span></span>

## <a name="learn-more-about-azure-cost-management"></a><span data-ttu-id="8ab6d-130">Další informace o Azure náklady na správu</span><span class="sxs-lookup"><span data-stu-id="8ab6d-130">Learn more about Azure cost management</span></span>
- <span data-ttu-id="8ab6d-131">Odhad nákladů pomocí hello [cenové kalkulačky](https://azure.microsoft.com/pricing/calculator/), [celkové náklady na vlastnictví kalkulačky](https://aka.ms/azure-tco-calculator), a když přidáte službu.</span><span class="sxs-lookup"><span data-stu-id="8ab6d-131">Estimate costs using hello [pricing calculator](https://azure.microsoft.com/pricing/calculator/), [total cost of ownership calculator](https://aka.ms/azure-tco-calculator), and when you add a service.</span></span>
- <span data-ttu-id="8ab6d-132">[Zkontrolujte využití a náklady na portálu Azure pravidelně](billing-getting-started.md#costs).</span><span class="sxs-lookup"><span data-stu-id="8ab6d-132">[Review your usage and costs regularly in Azure portal](billing-getting-started.md#costs).</span></span>
- <span data-ttu-id="8ab6d-133">Zapnout [Azure Advisor náklady doporučení](../advisor/advisor-cost-recommendations.md).</span><span class="sxs-lookup"><span data-stu-id="8ab6d-133">Turn on [Azure Advisor cost recommendations](../advisor/advisor-cost-recommendations.md).</span></span>

<span data-ttu-id="8ab6d-134">Další, najdete v části toolearn [Azure náklady správy pokyny](billing-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="8ab6d-134">toolearn more, see [Azure cost management guidance](billing-getting-started.md).</span></span>

[Image1]: ./media/azure-billing-set-up-alerts/billingalert1.png 
[Image2]: ./media/azure-billing-set-up-alerts/billingalert2.png
[Image3]: ./media/azure-billing-set-up-alerts/billingalerts3.png 
