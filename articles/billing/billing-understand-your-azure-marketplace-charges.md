---
title: "Pochopení Azure externí poplatky za | Microsoft Docs"
description: "Další informace o fakturaci externích služeb, dřív označované jako Marketplace, poplatky v Azure."
services: 
documentationcenter: 
author: adpick
manager: tonguyen
editor: 
tags: billing
ms.assetid: 5e0e2a3c-d111-4054-8508-0c111c1b749b
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: adpick
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 11701ce0162113ef6c8e056d3a30fe1d8f702f92
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="understand-your-azure-billing-for-external-service-charges"></a><span data-ttu-id="5d6c7-103">Pochopení vašeho Azure fakturace poplatků za externí služby</span><span class="sxs-lookup"><span data-stu-id="5d6c7-103">Understand your Azure billing for external service charges</span></span>
<span data-ttu-id="5d6c7-104">Externí služby použít k volání Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="5d6c7-104">External services used to be called Azure Marketplace.</span></span> <span data-ttu-id="5d6c7-105">Obecně platí že služby publikována třetích stran k dispozici pro Azure ale jsou plně integrované v rámci Azure.</span><span class="sxs-lookup"><span data-stu-id="5d6c7-105">Generally, they're services published by third-parties available for Azure but are integrated completely within Azure.</span></span> <span data-ttu-id="5d6c7-106">Například ClearDB a sendgrid vám umožňuje jsou externích služeb, které můžete zakoupit v Azure, ale nejsou vydané společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="5d6c7-106">For example, ClearDB and SendGrid are external services that you can purchase in Azure, but are not published by Microsoft.</span></span>

<span data-ttu-id="5d6c7-107">Při zřizování nové externí služby nebo prostředku, upozornění se zobrazí:</span><span class="sxs-lookup"><span data-stu-id="5d6c7-107">When you provision a new external service or resource, a warning is shown:</span></span>

![Marketplace zakoupit upozornění](./media/billing-understand-your-azure-marketplace-charges/marketplace-warning.PNG)

> [!NOTE]
> <span data-ttu-id="5d6c7-109">Externích služeb jsou publikovány společností, které nejsou Microsoft, ale někdy jsou produkty společnosti Microsoft také klasifikovány jako externích služeb.</span><span class="sxs-lookup"><span data-stu-id="5d6c7-109">External services are published by companies that are not Microsoft, but sometimes Microsoft products are also categorized as external services.</span></span>
> 
> 

## <a name="how-external-services-are-billed"></a><span data-ttu-id="5d6c7-110">Jak se účtují externích služeb</span><span class="sxs-lookup"><span data-stu-id="5d6c7-110">How external services are billed</span></span>
- <span data-ttu-id="5d6c7-111">Externích služeb se účtují samostatně.</span><span class="sxs-lookup"><span data-stu-id="5d6c7-111">External services are billed separately.</span></span> <span data-ttu-id="5d6c7-112">Jsou považovány za jednotlivé příkazy v rámci vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="5d6c7-112">They are treated as individual orders within your Azure subscription.</span></span> <span data-ttu-id="5d6c7-113">Fakturační období pro každou službu nastavena při nákupu služby.</span><span class="sxs-lookup"><span data-stu-id="5d6c7-113">The billing period for each service is set when you purchase the service.</span></span> <span data-ttu-id="5d6c7-114">Termín nelze zaměňovat s fakturační období předplatného, pod kterým jste ho koupili.</span><span class="sxs-lookup"><span data-stu-id="5d6c7-114">Not to be confused with the billing period of the subscription under which you purchased it.</span></span> <span data-ttu-id="5d6c7-115">Zobrazí také samostatné faktur a platební karty je účtována samostatně.</span><span class="sxs-lookup"><span data-stu-id="5d6c7-115">You also receive separate bills and your credit card is charged separately.</span></span>
- <span data-ttu-id="5d6c7-116">Každá externí služba má jiný model fakturace.</span><span class="sxs-lookup"><span data-stu-id="5d6c7-116">Each external service has a different billing model.</span></span> <span data-ttu-id="5d6c7-117">Některé služby se účtují průběžnými platbami způsobem, jiné zase měsíční modelu na základě platby.</span><span class="sxs-lookup"><span data-stu-id="5d6c7-117">Some services are billed in a pay-as-you-go fashion while others use a monthly based payment model.</span></span> <span data-ttu-id="5d6c7-118">Potřebujete platební karty pro externí služby Azure, nelze koupit externích služeb s platím faktury.</span><span class="sxs-lookup"><span data-stu-id="5d6c7-118">You need a credit card for Azure external services, you can't buy external services with invoice pay.</span></span>
- <span data-ttu-id="5d6c7-119">Měsíční kredity zdarma nelze použít pro externích služeb.</span><span class="sxs-lookup"><span data-stu-id="5d6c7-119">You can't use monthly free credits for external services.</span></span> <span data-ttu-id="5d6c7-120">Pokud používáte předplatné Azure, která zahrnuje [volné kredity](https://azure.microsoft.com/pricing/spending-limits/), nelze použít externí služba faktur.</span><span class="sxs-lookup"><span data-stu-id="5d6c7-120">If you are using an Azure subscription that includes [free credits](https://azure.microsoft.com/pricing/spending-limits/), they can't be applied to external service bills.</span></span> <span data-ttu-id="5d6c7-121">Chcete-li zakoupit externích služeb používají platební kartu.</span><span class="sxs-lookup"><span data-stu-id="5d6c7-121">Use a credit card to purchase external services.</span></span>


## <a name="view-external-service-spending-and-history-in-the-azure-portal"></a><span data-ttu-id="5d6c7-122">Zobrazení externí služba výdaje a historie na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5d6c7-122">View external service spending and history in the Azure portal</span></span>
<span data-ttu-id="5d6c7-123">Můžete zobrazit seznam externích služeb, které jsou v každém předplatném v rámci [portál Azure](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="5d6c7-123">You can view a list of the external services that are on each subscription within the [Azure portal](https://portal.azure.com/):</span></span> 

1. <span data-ttu-id="5d6c7-124">Přihlaste se k [portál Azure](https://portal.azure.com/) jako správce účtu.</span><span class="sxs-lookup"><span data-stu-id="5d6c7-124">Sign in to the [Azure portal](https://portal.azure.com/) as the account administrator.</span></span>
2. <span data-ttu-id="5d6c7-125">V nabídce centra vyberte **odběry**.</span><span class="sxs-lookup"><span data-stu-id="5d6c7-125">In the Hub menu, select **Subscriptions**.</span></span>
   
    ![V nabídce centra vyberte odběrů](./media/billing-understand-your-azure-marketplace-charges/sub-button.png) 
3. <span data-ttu-id="5d6c7-127">V **odběry** , vyberte předplatné, které chcete zobrazit a pak vyberte **externích služeb**.</span><span class="sxs-lookup"><span data-stu-id="5d6c7-127">In the **Subscriptions** blade, select the subscription that you want to view, and then select **External services**.</span></span>
   
    ![Vyberte předplatné, v okně fakturace](./media/billing-understand-your-azure-marketplace-charges/select-sub-external-services.png)
4. <span data-ttu-id="5d6c7-129">Měli byste vidět všechny vaše externí služba objednávky, název vydavatele, úroveň služby, které jste si zakoupili, název dáte prostředek a aktuální stav pořadí.</span><span class="sxs-lookup"><span data-stu-id="5d6c7-129">You should see each of your external service orders, the publisher name, service tier you bought, name you gave the resource, and the current order status.</span></span> <span data-ttu-id="5d6c7-130">V minulosti faktur, vyberte externí služby.</span><span class="sxs-lookup"><span data-stu-id="5d6c7-130">To see past bills, select an external service.</span></span>
   
    ![Vyberte externí služby](./media/billing-understand-your-azure-marketplace-charges/external-service-blade2.png)
5. <span data-ttu-id="5d6c7-132">Tady můžete zobrazit po faktury objemy včetně rozpis daň.</span><span class="sxs-lookup"><span data-stu-id="5d6c7-132">From here, you can view past bill amounts including the tax breakdown.</span></span>
   
    ![Zobrazení historie fakturace externích služeb](./media/billing-understand-your-azure-marketplace-charges/billing-overview-blade.png)

## <a name="view-external-service-spending-for-enterprise-agreement-ea-customers"></a><span data-ttu-id="5d6c7-134">Zobrazení externí služba výdaje pro zákazníky Enterprise Agreement (EA)</span><span class="sxs-lookup"><span data-stu-id="5d6c7-134">View external service spending for Enterprise Agreement (EA) customers</span></span>
<span data-ttu-id="5d6c7-135">Zákazníci EA můžete zobrazit externí služba výdaje a stáhnout sestav na portálu EA.</span><span class="sxs-lookup"><span data-stu-id="5d6c7-135">EA customers can see external service spending and download reports in the EA portal.</span></span> <span data-ttu-id="5d6c7-136">V tématu [Azure Marketplace pro zákazníky, EA](https://ea.azure.com/helpdocs/azureMarketplace) začít pracovat.</span><span class="sxs-lookup"><span data-stu-id="5d6c7-136">See [Azure Marketplace for EA Customers](https://ea.azure.com/helpdocs/azureMarketplace) to get started.</span></span>

## <a name="manage-payment-methods-for-external-service-orders"></a><span data-ttu-id="5d6c7-137">Spravovat způsoby platby pro objednávky externí služby</span><span class="sxs-lookup"><span data-stu-id="5d6c7-137">Manage payment methods for external service orders</span></span>
<span data-ttu-id="5d6c7-138">Aktualizace vašeho způsoby platby pro externí služba objednávky z [centra účtů](https://account.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="5d6c7-138">Update your payment methods for external service orders from the [Account Center](https://account.windowsazure.com/).</span></span>

> [!NOTE]
> <span data-ttu-id="5d6c7-139">Pokud jste zakoupili předplatné s pracovního nebo školního účtu, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) změnit způsob platby.</span><span class="sxs-lookup"><span data-stu-id="5d6c7-139">If you purchased your subscription with a Work or School account, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) to make changes to your payment method.</span></span>
> 
> 

1. <span data-ttu-id="5d6c7-140">Přihlaste se k [centra účtů](https://account.windowsazure.com/) a [přejděte do **marketplace** karta](https://account.windowsazure.com/Store)</span><span class="sxs-lookup"><span data-stu-id="5d6c7-140">Sign in to the [Account Center](https://account.windowsazure.com/) and [navigate to the **marketplace** tab](https://account.windowsazure.com/Store)</span></span>
   
    ![V centru účtů vyberte marketplace.](./media/billing-understand-your-azure-marketplace-charges/select-marketplace.png)
2. <span data-ttu-id="5d6c7-142">Vyberte externí službu, kterou chcete spravovat</span><span class="sxs-lookup"><span data-stu-id="5d6c7-142">Select the external service you want to manage</span></span>
   
    ![Vyberte externí službu, kterou chcete spravovat](./media/billing-understand-your-azure-marketplace-charges/select-ext-service.png)
3. <span data-ttu-id="5d6c7-144">Klikněte na tlačítko **změnit způsob platby** na pravé straně stránky.</span><span class="sxs-lookup"><span data-stu-id="5d6c7-144">Click **Change payment method** on the right side of the page.</span></span> <span data-ttu-id="5d6c7-145">Tento odkaz vám přináší pro jiný portál pro správu váš způsob platby.</span><span class="sxs-lookup"><span data-stu-id="5d6c7-145">This link brings you to a different portal to manage your payment method.</span></span>
   
    ![Souhrn pořadí](./media/billing-understand-your-azure-marketplace-charges/change-payment.PNG)
4. <span data-ttu-id="5d6c7-147">Klikněte na tlačítko **upravit informace o** a postupujte podle pokynů k aktualizaci vašich platebních informací.</span><span class="sxs-lookup"><span data-stu-id="5d6c7-147">Click **Edit info** and follow instructions to update your payment information.</span></span>
   
    ![Vyberte možnost upravit informace o](./media/billing-understand-your-azure-marketplace-charges/edit-info.png)

## <a name="cancel-an-external-service-order"></a><span data-ttu-id="5d6c7-149">Zrušení objednávky externí služby</span><span class="sxs-lookup"><span data-stu-id="5d6c7-149">Cancel an external service order</span></span>
<span data-ttu-id="5d6c7-150">Pokud chcete zrušit vaše externí služba pořadí, odstranit prostředek [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5d6c7-150">If you want to cancel your external service order, delete the resource in the [Azure portal](https://portal.azure.com).</span></span>

![Odstranit prostředek](./media/billing-understand-your-azure-marketplace-charges/deleteMarketplaceOrder.PNG)

## <a name="need-help-contact-support"></a><span data-ttu-id="5d6c7-152">Potřebujete pomoct?</span><span class="sxs-lookup"><span data-stu-id="5d6c7-152">Need help?</span></span> <span data-ttu-id="5d6c7-153">Obraťte se na podporu.</span><span class="sxs-lookup"><span data-stu-id="5d6c7-153">Contact support.</span></span>
<span data-ttu-id="5d6c7-154">Pokud máte otázky, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) získat rychle vyřešit problém.</span><span class="sxs-lookup"><span data-stu-id="5d6c7-154">If you still have questions, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) to get your issue resolved quickly.</span></span>

