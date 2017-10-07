---
title: "aaaView hello měsíční odhadované testovacím trend náklady v Azure DevTest Labs | Microsoft Docs"
description: "Další informace o hello Azure DevTest Labs měsíční odhadované náklady trend grafu."
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
ms.openlocfilehash: 47c7dd4cf91b76de74b502c50f05e79cd501ee35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="view-hello-monthly-estimated-lab-cost-trend-in-azure-devtest-labs"></a><span data-ttu-id="36a2d-103">Zobrazení hello měsíční odhadované testovacím trend náklady v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="36a2d-103">View hello monthly estimated lab cost trend in Azure DevTest Labs</span></span>
<span data-ttu-id="36a2d-104">Funkce náklady na správu Hello DevTest Labs vám pomůže sledovat hello náklady na vašem testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="36a2d-104">hello Cost Management feature of DevTest Labs helps you track hello cost of your lab.</span></span> <span data-ttu-id="36a2d-105">Tento článek ukazuje, jak toouse hello **měsíční odhadované náklady Trend** grafu tooview hello aktuální kalendářní měsíc odhadované náklady na začátku a hello předpokládané koncový měsíc náklady pro hello aktuální kalendářní měsíc.</span><span class="sxs-lookup"><span data-stu-id="36a2d-105">This article illustrates how toouse hello **Monthly Estimated Cost Trend** chart tooview hello current calendar month's estimated cost-to-date and hello projected end-of-month cost for hello current calendar month.</span></span> <span data-ttu-id="36a2d-106">V tomto článku se dozvíte, jak tooview hello měsíční odhadované náklady graf trendů v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="36a2d-106">In this article, you learn how tooview hello monthly estimated cost trend chart in hello Azure portal.</span></span>

## <a name="viewing-hello-monthly-estimated-cost-trend-chart"></a><span data-ttu-id="36a2d-107">Zobrazení grafu měsíční odhadované náklady Trend hello</span><span class="sxs-lookup"><span data-stu-id="36a2d-107">Viewing hello Monthly Estimated Cost Trend chart</span></span>
<span data-ttu-id="36a2d-108">tooview hello měsíční Trend odhadované náklady na graf, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="36a2d-108">tooview hello Monthly Estimated Cost Trend chart, follow these steps:</span></span> 

1. <span data-ttu-id="36a2d-109">Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="36a2d-109">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="36a2d-110">Vyberte **více služeb**a potom vyberte **DevTest Labs** hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="36a2d-110">Select **More Services**, and then select **DevTest Labs** from hello list.</span></span>
3. <span data-ttu-id="36a2d-111">Ze seznamu hello labs vyberte požadované prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="36a2d-111">From hello list of labs, select hello desired lab.</span></span>   
4. <span data-ttu-id="36a2d-112">V okně prostředí hello vyberte **náklady nastavení**.</span><span class="sxs-lookup"><span data-stu-id="36a2d-112">On hello lab's blade, select **Cost settings**.</span></span>
5. <span data-ttu-id="36a2d-113">V testovacím hello **náklady nastavení** vyberte **trend náklady na testovacím**.</span><span class="sxs-lookup"><span data-stu-id="36a2d-113">On hello lab's **Cost settings** blade, select **Lab cost trend**.</span></span>
6. <span data-ttu-id="36a2d-114">Hello následující snímek obrazovky ukazuje příklad grafu náklady.</span><span class="sxs-lookup"><span data-stu-id="36a2d-114">hello following screen shot shows an example of a cost chart.</span></span> 
   
    ![Cenově grafu](./media/devtest-lab-configure-cost-management/graph.png)

<span data-ttu-id="36a2d-116">Hello **odhadované náklady na** hodnota je hello aktuální kalendářní měsíc odhadované náklady na začátku.</span><span class="sxs-lookup"><span data-stu-id="36a2d-116">hello **Estimated cost** value is hello current calendar month's estimated cost-to-date.</span></span> <span data-ttu-id="36a2d-117">Hello **plánované náklady** hello odhadované náklady pro celou hello aktuální kalendářní měsíc vypočítáván hello testovacím náklady pro hello předchozí pět dní.</span><span class="sxs-lookup"><span data-stu-id="36a2d-117">hello **Projected cost** is hello estimated cost for hello entire current calendar month, calculated using hello lab cost for hello previous five days.</span></span>

<span data-ttu-id="36a2d-118">objemy Hello náklady jsou zaokrouhlit toohello nejbližší celé číslo.</span><span class="sxs-lookup"><span data-stu-id="36a2d-118">hello cost amounts are rounded up toohello next whole number.</span></span> <span data-ttu-id="36a2d-119">Například:</span><span class="sxs-lookup"><span data-stu-id="36a2d-119">For example:</span></span> 

* <span data-ttu-id="36a2d-120">5.01 zaokrouhlí too6</span><span class="sxs-lookup"><span data-stu-id="36a2d-120">5.01 rounds up too6</span></span> 
* <span data-ttu-id="36a2d-121">5.50 zaokrouhlí too6</span><span class="sxs-lookup"><span data-stu-id="36a2d-121">5.50 rounds up too6</span></span>
* <span data-ttu-id="36a2d-122">5.99 zaokrouhlí too6</span><span class="sxs-lookup"><span data-stu-id="36a2d-122">5.99 rounds up too6</span></span>

<span data-ttu-id="36a2d-123">Jak je uvedeno výše hello grafu, jsou náklady hello se zobrazí v grafu hello *odhadované* stojí pomocí [průběžné platby](https://azure.microsoft.com/offers/ms-azr-0003p/) nabízejí sazby.</span><span class="sxs-lookup"><span data-stu-id="36a2d-123">As it states above hello chart, hello costs you see in hello chart are *estimated* costs using [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) offer rates.</span></span>
<span data-ttu-id="36a2d-124">Kromě toho jsou následující hello *není* součástí hello výpočet nákladů:</span><span class="sxs-lookup"><span data-stu-id="36a2d-124">Additionally, hello following are *not* included in hello cost calculation:</span></span>

* <span data-ttu-id="36a2d-125">Předplatné Dreamspark a CSP nejsou aktuálně podporovány jako Azure DevTest Labs používá hello [rozhraní API Azure fakturace](../billing/billing-usage-rate-card-overview.md) testovacím hello toocalculate náklady, který nepodporuje zprostředkovatele kryptografických služeb nebo Dreamspark odběry.</span><span class="sxs-lookup"><span data-stu-id="36a2d-125">CSP and Dreamspark subscriptions are currently not supported as Azure DevTest Labs uses hello [Azure billing APIs](../billing/billing-usage-rate-card-overview.md) toocalculate hello lab cost, which does not support CSP or Dreamspark subscriptions.</span></span>
* <span data-ttu-id="36a2d-126">Nabídka sazby.</span><span class="sxs-lookup"><span data-stu-id="36a2d-126">Your offer rates.</span></span> <span data-ttu-id="36a2d-127">V současné době jsme nejsou možné toouse nabídka sazby (zobrazené v rámci svého předplatného), že máte vyjedná se společnosti Microsoft nebo Microsoft partnery.</span><span class="sxs-lookup"><span data-stu-id="36a2d-127">Currently, we are not able toouse your offer rates (shown under your subscription) that you have negotiated with Microsoft or Microsoft partners.</span></span> <span data-ttu-id="36a2d-128">Používáme průběžné platby sazby.</span><span class="sxs-lookup"><span data-stu-id="36a2d-128">We are using Pay-As-You-Go rates.</span></span>
* <span data-ttu-id="36a2d-129">Vaše daně</span><span class="sxs-lookup"><span data-stu-id="36a2d-129">Your taxes</span></span>
* <span data-ttu-id="36a2d-130">Vaše slevy</span><span class="sxs-lookup"><span data-stu-id="36a2d-130">Your discounts</span></span>
* <span data-ttu-id="36a2d-131">Vaše fakturační Měna.</span><span class="sxs-lookup"><span data-stu-id="36a2d-131">Your billing currency.</span></span> <span data-ttu-id="36a2d-132">V současné době náklady hello testovacího prostředí se zobrazí pouze v USD měny.</span><span class="sxs-lookup"><span data-stu-id="36a2d-132">Currently, hello lab cost is displayed only in USD currency.</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a><span data-ttu-id="36a2d-133">Příspěvky blogu související</span><span class="sxs-lookup"><span data-stu-id="36a2d-133">Related blog posts</span></span>
* [<span data-ttu-id="36a2d-134">Dva další věci tookeep vaše náklady na sledování v DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="36a2d-134">Two more things tookeep your cost on track in DevTest Labs</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/06/21/keep-your-cost-on-track/)
* [<span data-ttu-id="36a2d-135">Proč náklady prahové hodnoty?</span><span class="sxs-lookup"><span data-stu-id="36a2d-135">Why Cost Thresholds?</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/04/11/why-cost-thresholds/)

## <a name="next-steps"></a><span data-ttu-id="36a2d-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="36a2d-136">Next steps</span></span>
<span data-ttu-id="36a2d-137">Zde jsou další tootry některé věci:</span><span class="sxs-lookup"><span data-stu-id="36a2d-137">Here are some things tootry next:</span></span>

* <span data-ttu-id="36a2d-138">[Definování zásad testovacího](devtest-lab-set-lab-policy.md) – zjistěte, jak tooset hello různé zásady použít toogovern použití testovacího prostředí a jeho virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="36a2d-138">[Define lab policies](devtest-lab-set-lab-policy.md) - Learn how tooset hello various policies used toogovern how your lab and its VMs are used.</span></span> 
* <span data-ttu-id="36a2d-139">[Vytvořit vlastní image](devtest-lab-create-template.md) – při vytváření virtuálního počítače, zadejte základ, který může být buď vlastní image nebo image pořízenou prostřednictvím Marketplace.</span><span class="sxs-lookup"><span data-stu-id="36a2d-139">[Create custom image](devtest-lab-create-template.md) - When you create a VM, you specify a base, which can be either a custom image or a Marketplace image.</span></span> <span data-ttu-id="36a2d-140">Tento článek ukazuje, jak toocreate vlastní obrázek z soubor virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="36a2d-140">This article illustrates how toocreate a custom image from a VHD file.</span></span>
* <span data-ttu-id="36a2d-141">[Konfigurace Marketplace image](devtest-lab-configure-marketplace-images.md) – DevTest Labs podporuje vytváření virtuálních počítačů založené na imagích Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="36a2d-141">[Configure Marketplace images](devtest-lab-configure-marketplace-images.md) - DevTest Labs supports creating VMs based on Azure Marketplace images.</span></span> <span data-ttu-id="36a2d-142">Tento článek ukazuje, jak toospecify, který, pokud existuje, Azure Marketplace bitové kopie lze použít při vytváření virtuálních počítačů v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="36a2d-142">This article illustrates how toospecify which, if any, Azure Marketplace images can be used when creating VMs in a lab.</span></span>
* <span data-ttu-id="36a2d-143">[Vytvoření virtuálního počítače v testovacím prostředí](devtest-lab-add-vm-with-artifacts.md) -ukazuje, jak toocreate virtuální počítač z bitové kopie základní (buď vlastní nebo Marketplace) a jak toowork s artefakty ve vašem virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="36a2d-143">[Create a VM in a lab](devtest-lab-add-vm-with-artifacts.md) - Illustrates how toocreate a VM from a base image (either custom or Marketplace), and how toowork with artifacts in your VM.</span></span>

