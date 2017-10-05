---
title: "Nastavení výstrah fakturace nebo platební pro předplatná Azure | Microsoft Docs"
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
ms.openlocfilehash: 7a1b579fdde831fdc3afa0a2aee4c24890216ed1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-billing-or-credit-alerts-for-your-microsoft-azure-subscriptions"></a><span data-ttu-id="2ad13-104">Nastavení výstrah fakturace nebo platební pro vaše předplatné Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="2ad13-104">Set up billing or credit alerts for your Microsoft Azure subscriptions</span></span>
<span data-ttu-id="2ad13-105">Pokud jste správce účtů pro předplatné Azure, můžete použít Azure fakturace služby oznámení. Chcete-li vytvořit vlastní fakturace výstrahy, které vám pomůžou monitorovat a spravovat fakturace aktivity pro vaše účty Azure.</span><span class="sxs-lookup"><span data-stu-id="2ad13-105">If you’re the Account Admin for an Azure subscription, you can use the Azure Billing Alert Service to create customized billing alerts that help you monitor and manage billing activity for your Azure accounts.</span></span>

<span data-ttu-id="2ad13-106">Tato služba je ve verzi preview, takže je nutné povolit na stránce funkce verze Preview.</span><span class="sxs-lookup"><span data-stu-id="2ad13-106">This service is in preview, so you need to enable it in the Preview Features page first.</span></span>

## <a name="set-the-alert-threshold-and-email-recipients"></a><span data-ttu-id="2ad13-107">Nastavit výstrahy příjemce prahovou hodnotu a e-mailu</span><span class="sxs-lookup"><span data-stu-id="2ad13-107">Set the alert threshold and email recipients</span></span>
1. <span data-ttu-id="2ad13-108">Navštivte [stránce funkce verze Preview](https://account.windowsazure.com/PreviewFeatures) a povolte **fakturace výstrah služby**.</span><span class="sxs-lookup"><span data-stu-id="2ad13-108">Visit [the Preview Features page](https://account.windowsazure.com/PreviewFeatures) and enable **Billing Alert Service**.</span></span>

1. <span data-ttu-id="2ad13-109">Po přijetí e-mailu potvrzení, že je zapnutý fakturace služby pro vaše předplatné, navštivte [stránce předplatných](https://account.windowsazure.com/Subscriptions) na portálu účtů.</span><span class="sxs-lookup"><span data-stu-id="2ad13-109">After you receive the email confirmation that the billing service is turned on for your subscription, visit [the Subscriptions page](https://account.windowsazure.com/Subscriptions) in the account portal.</span></span> <span data-ttu-id="2ad13-110">Klikněte na předplatné, které chcete monitorovat a pak klikněte na tlačítko **výstrahy**.</span><span class="sxs-lookup"><span data-stu-id="2ad13-110">Click the subscription you want to monitor, and then click **Alerts**.</span></span>

    ![Snímek obrazovky zobrazení odběry z centra účtů Azure, se zvýrazněnou výstrahy][Image1]

2. <span data-ttu-id="2ad13-112">Klikněte na tlačítko **přidat výstrahy** k vytvoření vaší první z nich.</span><span class="sxs-lookup"><span data-stu-id="2ad13-112">Next, click **Add Alert** to create your first one.</span></span> <span data-ttu-id="2ad13-113">Můžete nastavit celkem pět fakturace výstrahy na předplatné s různé prahové hodnoty a až dvě příjemců e-mailu pro každou výstrahu.</span><span class="sxs-lookup"><span data-stu-id="2ad13-113">You can set up a total of five billing alerts per subscription, with a different threshold and up to two email recipients for each alert.</span></span>

    ![Snímek obrazovky zobrazení výstrahy, ve kterém můžete přidat výstrahy][Image2]

3. <span data-ttu-id="2ad13-115">Přidáte-li výstrahu, můžete zadat jedinečný název, zvolte útraty prahovou hodnotu a zvolte e-mailové adresy, které jsou odesílány výstrahy.</span><span class="sxs-lookup"><span data-stu-id="2ad13-115">When you add an alert, you give it a unique name, choose a spending threshold, and choose the email addresses where alerts are sent.</span></span> <span data-ttu-id="2ad13-116">Při nastavování prahovou hodnotu, můžete buď **fakturace celkový** nebo **peněžního kreditu, který** z **výstrahy pro** seznamu.</span><span class="sxs-lookup"><span data-stu-id="2ad13-116">When setting up the threshold, you can choose either a **Billing Total** or a **Monetary Credit** from the **Alert For** list.</span></span> <span data-ttu-id="2ad13-117">Fakturace celkem zobrazí se výstraha při předplatné výdaje překročí prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="2ad13-117">For a billing total, an alert is sent when subscription spending exceeds the threshold.</span></span> <span data-ttu-id="2ad13-118">Pro peněžního kreditu zobrazí se výstraha při peněžní kredity neklesla pod limit.</span><span class="sxs-lookup"><span data-stu-id="2ad13-118">For a monetary credit, an alert is sent when monetary credits drop below the limit.</span></span> <span data-ttu-id="2ad13-119">Peněžní kredity obvykle platí pro odběry bezplatné zkušební verze a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2ad13-119">Monetary credits usually apply to Free Trial and Visual Studio subscriptions.</span></span>

    ![Snímek obrazovky zobrazení výstrah přidání, kde můžete konfigurovat příjemce][Image3]

<span data-ttu-id="2ad13-121">Azure podporuje všechny e-mailovou adresu, ale není ověřte, že prací e-mailovou adresu, proto tuto možnost pro překlepům.</span><span class="sxs-lookup"><span data-stu-id="2ad13-121">Azure supports any email address but doesn't verify that the email address works, so double-check for typos.</span></span>

## <a name="check-on-your-alerts"></a><span data-ttu-id="2ad13-122">Zkontrolujte na upozornění</span><span class="sxs-lookup"><span data-stu-id="2ad13-122">Check on your alerts</span></span>
<span data-ttu-id="2ad13-123">Po nastavení výstrahy centra účtů je uvádí a ukazuje, kolik více můžete nastavit.</span><span class="sxs-lookup"><span data-stu-id="2ad13-123">After you set up alerts, the Account Center lists them and shows how many more you can set up.</span></span> <span data-ttu-id="2ad13-124">Pro každou výstrahu zobrazí datum a čas, který byl odeslán, jestli je výstraha fakturace celkový nebo peněžního kreditu, který a omezení, které vytvořili.</span><span class="sxs-lookup"><span data-stu-id="2ad13-124">For each alert, you see the date and time it was sent, whether it’s an alert for Billing Total or Monetary Credit, and the limit you set up.</span></span> <span data-ttu-id="2ad13-125">Datum je formát rrrr mm-dd formát data a času je 24 hodin koordinaci světový čas (UTC).</span><span class="sxs-lookup"><span data-stu-id="2ad13-125">The date and time format is 24-hour Universal Time Coordinate (UTC) and the date is yyyy-mm-dd format.</span></span> <span data-ttu-id="2ad13-126">Kliknutím na znaménko plus pro výstrahu v seznamu, pokud ho chcete upravit, nebo klikněte na odpadkový ho odstranit.</span><span class="sxs-lookup"><span data-stu-id="2ad13-126">Click the plus sign for an alert in the list to edit it, or click the trash-can to delete it.</span></span>

## <a name="billing-alerts-for-enterprise-agreement-ea-customers"></a><span data-ttu-id="2ad13-127">Výstrahy fakturace pro zákazníky Enterprise Agreement (EA)</span><span class="sxs-lookup"><span data-stu-id="2ad13-127">Billing alerts for Enterprise Agreement (EA) customers</span></span>
<span data-ttu-id="2ad13-128">Zákazníci EA můžete získat výstrahy pro každé oddělení v zápisu nastavení výdaje kvóty.</span><span class="sxs-lookup"><span data-stu-id="2ad13-128">EA customers can get alerts for each department under an enrollment by setting spending quotas.</span></span> <span data-ttu-id="2ad13-129">V tématu [oddělení výdaje kvóty](https://ea.azure.com/helpdocs/departmentSpendingQuotas) na portálu EA začít pracovat.</span><span class="sxs-lookup"><span data-stu-id="2ad13-129">See [Department Spending Quotas](https://ea.azure.com/helpdocs/departmentSpendingQuotas) in the EA portal to get started.</span></span>

## <a name="learn-more-about-azure-cost-management"></a><span data-ttu-id="2ad13-130">Další informace o Azure náklady na správu</span><span class="sxs-lookup"><span data-stu-id="2ad13-130">Learn more about Azure cost management</span></span>
- <span data-ttu-id="2ad13-131">Odhad náklady na používání [cenové kalkulačky](https://azure.microsoft.com/pricing/calculator/), [celkové náklady na vlastnictví kalkulačky](https://aka.ms/azure-tco-calculator), a když přidáte službu.</span><span class="sxs-lookup"><span data-stu-id="2ad13-131">Estimate costs using the [pricing calculator](https://azure.microsoft.com/pricing/calculator/), [total cost of ownership calculator](https://aka.ms/azure-tco-calculator), and when you add a service.</span></span>
- <span data-ttu-id="2ad13-132">[Zkontrolujte využití a náklady na portálu Azure pravidelně](billing-getting-started.md#costs).</span><span class="sxs-lookup"><span data-stu-id="2ad13-132">[Review your usage and costs regularly in Azure portal](billing-getting-started.md#costs).</span></span>
- <span data-ttu-id="2ad13-133">Zapnout [Azure Advisor náklady doporučení](../advisor/advisor-cost-recommendations.md).</span><span class="sxs-lookup"><span data-stu-id="2ad13-133">Turn on [Azure Advisor cost recommendations](../advisor/advisor-cost-recommendations.md).</span></span>

<span data-ttu-id="2ad13-134">Další informace najdete v tématu [Azure náklady správy pokyny](billing-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="2ad13-134">To learn more, see [Azure cost management guidance](billing-getting-started.md).</span></span>

[Image1]: ./media/azure-billing-set-up-alerts/billingalert1.png 
[Image2]: ./media/azure-billing-set-up-alerts/billingalert2.png
[Image3]: ./media/azure-billing-set-up-alerts/billingalerts3.png 
