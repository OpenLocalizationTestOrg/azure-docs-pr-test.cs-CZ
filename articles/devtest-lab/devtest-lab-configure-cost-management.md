---
title: "Zobrazit měsíční náklady na trend odhadované testovacího prostředí v Azure DevTest Labs | Microsoft Docs"
description: "Další informace o graf trendů Azure DevTest Labs měsíční odhadované náklady."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 1f46fdc5-d917-46e3-a1ea-f6dd41212ba4
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2016
ms.author: tarcher
ms.openlocfilehash: b3ad1ead522908d4b41b7cca98d20ac91664998e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="view-the-monthly-estimated-lab-cost-trend-in-azure-devtest-labs"></a><span data-ttu-id="17f42-103">Zobrazit měsíční náklady na trend odhadované testovacího prostředí v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="17f42-103">View the monthly estimated lab cost trend in Azure DevTest Labs</span></span>
<span data-ttu-id="17f42-104">Náklady na správu funkce DevTest Labs vám pomůže sledovat náklady na vašem testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="17f42-104">The Cost Management feature of DevTest Labs helps you track the cost of your lab.</span></span> <span data-ttu-id="17f42-105">Tento článek ukazuje, jak používat **měsíční odhadované náklady Trend** grafu zobrazíte aktuální kalendářní měsíc odhadované náklady na začátku a předpokládané náklady na konci měsíce pro aktuální měsíc kalendáře.</span><span class="sxs-lookup"><span data-stu-id="17f42-105">This article illustrates how to use the **Monthly Estimated Cost Trend** chart to view the current calendar month's estimated cost-to-date and the projected end-of-month cost for the current calendar month.</span></span> <span data-ttu-id="17f42-106">V tomto článku zjistěte, jak chcete-li zobrazit měsíční graf trendů odhadované náklady na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="17f42-106">In this article, you learn how to view the monthly estimated cost trend chart in the Azure portal.</span></span>

## <a name="viewing-the-monthly-estimated-cost-trend-chart"></a><span data-ttu-id="17f42-107">Zobrazení grafu měsíční odhadované náklady trendů</span><span class="sxs-lookup"><span data-stu-id="17f42-107">Viewing the Monthly Estimated Cost Trend chart</span></span>
<span data-ttu-id="17f42-108">Chcete-li zobrazit graf měsíční odhadované náklady trendů, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="17f42-108">To view the Monthly Estimated Cost Trend chart, follow these steps:</span></span> 

1. <span data-ttu-id="17f42-109">Přihlaste se k webu [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="17f42-109">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="17f42-110">Vyberte **více služeb**a potom vyberte **DevTest Labs** ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="17f42-110">Select **More Services**, and then select **DevTest Labs** from the list.</span></span>
3. <span data-ttu-id="17f42-111">Ze seznamu labs vyberte požadované testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="17f42-111">From the list of labs, select the desired lab.</span></span>   
4. <span data-ttu-id="17f42-112">V okně v prostředí, vyberte **náklady nastavení**.</span><span class="sxs-lookup"><span data-stu-id="17f42-112">On the lab's blade, select **Cost settings**.</span></span>
5. <span data-ttu-id="17f42-113">V tomto prostředí **náklady nastavení** vyberte **trend náklady na testovacím**.</span><span class="sxs-lookup"><span data-stu-id="17f42-113">On the lab's **Cost settings** blade, select **Lab cost trend**.</span></span>
6. <span data-ttu-id="17f42-114">Následující snímek obrazovky ukazuje příklad grafu náklady.</span><span class="sxs-lookup"><span data-stu-id="17f42-114">The following screen shot shows an example of a cost chart.</span></span> 
   
    ![Cenově grafu](./media/devtest-lab-configure-cost-management/graph.png)

<span data-ttu-id="17f42-116">**Odhadované náklady na** hodnota je aktuální kalendářní měsíc odhadované náklady na začátku.</span><span class="sxs-lookup"><span data-stu-id="17f42-116">The **Estimated cost** value is the current calendar month's estimated cost-to-date.</span></span> <span data-ttu-id="17f42-117">**Plánované náklady** je odhadované náklady pro celou aktuální kalendářní měsíc vypočítává pomocí náklady na testovacím předchozí pět dní.</span><span class="sxs-lookup"><span data-stu-id="17f42-117">The **Projected cost** is the estimated cost for the entire current calendar month, calculated using the lab cost for the previous five days.</span></span>

<span data-ttu-id="17f42-118">Objemy náklady jsou zaokrouhlený nahoru na nejbližší celé číslo.</span><span class="sxs-lookup"><span data-stu-id="17f42-118">The cost amounts are rounded up to the next whole number.</span></span> <span data-ttu-id="17f42-119">Například:</span><span class="sxs-lookup"><span data-stu-id="17f42-119">For example:</span></span> 

* <span data-ttu-id="17f42-120">5.01 zaokrouhlí až 6</span><span class="sxs-lookup"><span data-stu-id="17f42-120">5.01 rounds up to 6</span></span> 
* <span data-ttu-id="17f42-121">5.50 zaokrouhlí až 6</span><span class="sxs-lookup"><span data-stu-id="17f42-121">5.50 rounds up to 6</span></span>
* <span data-ttu-id="17f42-122">5.99 zaokrouhlí až 6</span><span class="sxs-lookup"><span data-stu-id="17f42-122">5.99 rounds up to 6</span></span>

<span data-ttu-id="17f42-123">Jak je uvedeno výše grafu, náklady, se zobrazí v grafu jsou *odhadované* stojí pomocí [průběžné platby](https://azure.microsoft.com/offers/ms-azr-0003p/) nabízejí sazby.</span><span class="sxs-lookup"><span data-stu-id="17f42-123">As it states above the chart, the costs you see in the chart are *estimated* costs using [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) offer rates.</span></span>
<span data-ttu-id="17f42-124">Kromě toho jsou následující *není* zahrnutých do výpočtu náklady na:</span><span class="sxs-lookup"><span data-stu-id="17f42-124">Additionally, the following are *not* included in the cost calculation:</span></span>

* <span data-ttu-id="17f42-125">Předplatné Dreamspark a CSP nejsou aktuálně podporovány jako používá Azure DevTest Labs [rozhraní API Azure fakturace](../billing/billing-usage-rate-card-overview.md) vypočítat náklady, který nepodporuje zprostředkovatele kryptografických služeb nebo Dreamspark odběry testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="17f42-125">CSP and Dreamspark subscriptions are currently not supported as Azure DevTest Labs uses the [Azure billing APIs](../billing/billing-usage-rate-card-overview.md) to calculate the lab cost, which does not support CSP or Dreamspark subscriptions.</span></span>
* <span data-ttu-id="17f42-126">Nabídka sazby.</span><span class="sxs-lookup"><span data-stu-id="17f42-126">Your offer rates.</span></span> <span data-ttu-id="17f42-127">V současné době Nedokážeme používat sazby nabídka (zobrazené v rámci svého předplatného), že máte vyjedná se společnosti Microsoft nebo Microsoft partnery.</span><span class="sxs-lookup"><span data-stu-id="17f42-127">Currently, we are not able to use your offer rates (shown under your subscription) that you have negotiated with Microsoft or Microsoft partners.</span></span> <span data-ttu-id="17f42-128">Používáme průběžné platby sazby.</span><span class="sxs-lookup"><span data-stu-id="17f42-128">We are using Pay-As-You-Go rates.</span></span>
* <span data-ttu-id="17f42-129">Vaše daně</span><span class="sxs-lookup"><span data-stu-id="17f42-129">Your taxes</span></span>
* <span data-ttu-id="17f42-130">Vaše slevy</span><span class="sxs-lookup"><span data-stu-id="17f42-130">Your discounts</span></span>
* <span data-ttu-id="17f42-131">Vaše fakturační Měna.</span><span class="sxs-lookup"><span data-stu-id="17f42-131">Your billing currency.</span></span> <span data-ttu-id="17f42-132">V současné době náklady na testovacího prostředí se zobrazí jenom v USD měny.</span><span class="sxs-lookup"><span data-stu-id="17f42-132">Currently, the lab cost is displayed only in USD currency.</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a><span data-ttu-id="17f42-133">Příspěvky blogu související</span><span class="sxs-lookup"><span data-stu-id="17f42-133">Related blog posts</span></span>
* [<span data-ttu-id="17f42-134">Dva kroky zachovat vaše náklady na sledování v DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="17f42-134">Two more things to keep your cost on track in DevTest Labs</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/06/21/keep-your-cost-on-track/)
* [<span data-ttu-id="17f42-135">Proč náklady prahové hodnoty?</span><span class="sxs-lookup"><span data-stu-id="17f42-135">Why Cost Thresholds?</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/04/11/why-cost-thresholds/)

## <a name="next-steps"></a><span data-ttu-id="17f42-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="17f42-136">Next steps</span></span>
<span data-ttu-id="17f42-137">Zde jsou další vyzkoušejte:</span><span class="sxs-lookup"><span data-stu-id="17f42-137">Here are some things to try next:</span></span>

* <span data-ttu-id="17f42-138">[Definování zásad testovacího](devtest-lab-set-lab-policy.md) – zjistěte, jak nastavit různé zásady použité k řízení použití testovacího prostředí a jeho virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="17f42-138">[Define lab policies](devtest-lab-set-lab-policy.md) - Learn how to set the various policies used to govern how your lab and its VMs are used.</span></span> 
* <span data-ttu-id="17f42-139">[Vytvořit vlastní image](devtest-lab-create-template.md) – při vytváření virtuálního počítače, zadejte základ, který může být buď vlastní image nebo image pořízenou prostřednictvím Marketplace.</span><span class="sxs-lookup"><span data-stu-id="17f42-139">[Create custom image](devtest-lab-create-template.md) - When you create a VM, you specify a base, which can be either a custom image or a Marketplace image.</span></span> <span data-ttu-id="17f42-140">Tento článek ukazuje, jak vytvořit vlastní obrázek ze souboru virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="17f42-140">This article illustrates how to create a custom image from a VHD file.</span></span>
* <span data-ttu-id="17f42-141">[Konfigurace Marketplace image](devtest-lab-configure-marketplace-images.md) – DevTest Labs podporuje vytváření virtuálních počítačů založené na imagích Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="17f42-141">[Configure Marketplace images](devtest-lab-configure-marketplace-images.md) - DevTest Labs supports creating VMs based on Azure Marketplace images.</span></span> <span data-ttu-id="17f42-142">Tento článek ukazuje, jak určit, které, pokud existuje, může být Azure Marketplace Image používá při vytváření virtuálních počítačů v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="17f42-142">This article illustrates how to specify which, if any, Azure Marketplace images can be used when creating VMs in a lab.</span></span>
* <span data-ttu-id="17f42-143">[Vytvoření virtuálního počítače v testovacím prostředí](devtest-lab-add-vm-with-artifacts.md) -znázorňuje, jak vytvořit virtuální počítač z bitové kopie základní (buď vlastní nebo Marketplace) a postup práce s artefakty ve vašem virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="17f42-143">[Create a VM in a lab](devtest-lab-add-vm-with-artifacts.md) - Illustrates how to create a VM from a base image (either custom or Marketplace), and how to work with artifacts in your VM.</span></span>

