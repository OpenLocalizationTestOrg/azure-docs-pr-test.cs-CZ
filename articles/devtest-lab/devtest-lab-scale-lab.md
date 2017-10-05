---
title: "Škálování kvóty a limity ve svém testovacím prostředí v Azure DevTest Labs | Microsoft Docs"
description: "Zjistěte, jak změnit velikost testovacího prostředí v Azure DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: ae624155-9181-45fa-bd2b-1983339b7e0e
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: tarcher
ms.openlocfilehash: f11ed42b474e4f208eac92689bfa33fb188d15a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="scale-quotas-and-limits-in-devtest-labs"></a><span data-ttu-id="95cb6-103">Škálování kvóty a omezení v DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="95cb6-103">Scale quotas and limits in DevTest Labs</span></span>
<span data-ttu-id="95cb6-104">Při práci v DevTest Labs, můžete si všimnout, že existují určitá omezení výchozí některé prostředky Azure, což může ovlivnit DevTest Labs služby.</span><span class="sxs-lookup"><span data-stu-id="95cb6-104">As you work in DevTest Labs, you might notice that there are certain default limits to some Azure resources, which can affect the DevTest Labs service.</span></span> <span data-ttu-id="95cb6-105">Tato omezení jsou označovány jako **kvóty**.</span><span class="sxs-lookup"><span data-stu-id="95cb6-105">These limits are referred to as **quotas**.</span></span>

> [!NOTE]
> <span data-ttu-id="95cb6-106">Službu DevTest Labs nebude ukládat žádné kvóty.</span><span class="sxs-lookup"><span data-stu-id="95cb6-106">The DevTest Labs service doesn't impose any quotas.</span></span> <span data-ttu-id="95cb6-107">Žádné kvóty, kterými se můžete setkat se výchozí omezení celkového předplatné.</span><span class="sxs-lookup"><span data-stu-id="95cb6-107">Any quotas you might encounter are default constraints of the overall Azure subscription.</span></span>

<span data-ttu-id="95cb6-108">Každý prostředek Azure můžete použít, dokud se nedostanete dosáhlo kvóty.</span><span class="sxs-lookup"><span data-stu-id="95cb6-108">You can use each Azure resource until you reach its quota.</span></span> <span data-ttu-id="95cb6-109">Každé předplatné má samostatné kvóty a sledování využití podle předplatného.</span><span class="sxs-lookup"><span data-stu-id="95cb6-109">Each subscription has separate quotas and usage is tracked per subscription.</span></span>

<span data-ttu-id="95cb6-110">Například každé předplatné má výchozí kvótu 20 jader.</span><span class="sxs-lookup"><span data-stu-id="95cb6-110">For example, each subscription has a default quota of 20 cores.</span></span> <span data-ttu-id="95cb6-111">Ano při vytváření virtuálních počítačů ve svém testovacím prostředí s čtyři jader, pak můžete pouze vytvořit pět virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="95cb6-111">So, if you are creating VMs in your lab with four cores each, then you can only create five VMs.</span></span> 

<span data-ttu-id="95cb6-112">[Azure předplatné a omezení služby](https://docs.microsoft.com/azure/azure-subscription-service-limits) uvádí některé z nejběžnějších kvóty pro prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="95cb6-112">[Azure Subscription and Service Limits](https://docs.microsoft.com/azure/azure-subscription-service-limits) lists some of the most common quotas for Azure resources.</span></span> <span data-ttu-id="95cb6-113">Prostředky se nejčastěji používá v testovacím prostředí a pro které můžete narazit kvóty, zahrnete jader virtuálního počítače, veřejné IP adresy, síťové rozhraní, spravované disky, přiřazení role RBAC, okruhy ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="95cb6-113">The resources most commonly used in a lab, and for which you might encounter quotas, include VM cores, public IP addresses, network interface, managed disks, RBAC role assignment, and ExpressRoute circuits.</span></span>

## <a name="view-your-usage-and-quotas"></a><span data-ttu-id="95cb6-114">Zobrazit využití a kvóty</span><span class="sxs-lookup"><span data-stu-id="95cb6-114">View your usage and quotas</span></span>
<span data-ttu-id="95cb6-115">Tyto kroky ukazují, jak chcete zobrazit aktuální kvóty ve vašem předplatném pro konkrétní prostředky Azure a chcete zobrazit, jaké procento každé kvóty jste už použili.</span><span class="sxs-lookup"><span data-stu-id="95cb6-115">These steps show you how to view the current quotas in your subscription for specific Azure resources, and to see what percentage of each quota you have used.</span></span>

1. <span data-ttu-id="95cb6-116">Přihlaste se k webu [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="95cb6-116">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
1. <span data-ttu-id="95cb6-117">Vyberte **více služeb**a potom vyberte **fakturace** ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="95cb6-117">Select **More Services**, and then select **Billing** from the list.</span></span>
1. <span data-ttu-id="95cb6-118">V okně fakturace vyberte předplatné.</span><span class="sxs-lookup"><span data-stu-id="95cb6-118">In the Billing blade, select a subscription.</span></span>
4. <span data-ttu-id="95cb6-119">Vyberte **využití + kvóty**.</span><span class="sxs-lookup"><span data-stu-id="95cb6-119">Select **Usage + quotas**.</span></span>

   ![Tlačítko využití a kvóty](./media/devtest-lab-scale-lab/devtestlab-usage-and-quotas.png)

   <span data-ttu-id="95cb6-121">Využití + kvóty okno se zobrazí, výpis různé prostředky k dispozici v tomto předplatném a procento kvóty, který se používá na prostředek.</span><span class="sxs-lookup"><span data-stu-id="95cb6-121">The Usage + quotas blade appears, listing different resources available in that subscription and the percentage of the quota that is being used per resource.</span></span>

   ![Kvóty a využití](./media/devtest-lab-scale-lab/devtestlab-view-quotas.png)

## <a name="requesting-more-resources-in-your-subscription"></a><span data-ttu-id="95cb6-123">Vyžaduje další prostředky ve vašem předplatném</span><span class="sxs-lookup"><span data-stu-id="95cb6-123">Requesting more resources in your subscription</span></span>
<span data-ttu-id="95cb6-124">Dosažení limitu kvóty výchozí limit prostředků v předplatném může být zvýšeno až maximální limit, jak je popsáno v [předplatné Azure a omezení služby](https://docs.microsoft.com/azure/azure-subscription-service-limits).</span><span class="sxs-lookup"><span data-stu-id="95cb6-124">If you reach a quota cap, the default limit of a resource in a subscription can be increased up to a maximum limit, as described in [Azure Subscription and Service Limits](https://docs.microsoft.com/azure/azure-subscription-service-limits).</span></span>

<span data-ttu-id="95cb6-125">Tyto kroky ukazují, jak požádat o zvýšení kvóty prostřednictvím [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="95cb6-125">These steps show you how to request a quota increase through the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

1. <span data-ttu-id="95cb6-126">Vyberte **více služeb**, vyberte **fakturace**a potom vyberte **využití + kvóty**.</span><span class="sxs-lookup"><span data-stu-id="95cb6-126">Select **More Services**, select **Billing**, and then select **Usage + quotas**.</span></span>
1. <span data-ttu-id="95cb6-127">V využití + kvóty vyberte **požádat o zvýšení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="95cb6-127">In the Usage + quotas blade, select the **Request Increase** button.</span></span>

   ![Žádost o zvýšení tlačítko](./media/devtest-lab-scale-lab/devtestlab-request-increase.png)

1. <span data-ttu-id="95cb6-129">K dokončení a odeslat žádost o, zadejte požadované informace o všech tří karet **nová žádost o podporu** formuláře.</span><span class="sxs-lookup"><span data-stu-id="95cb6-129">To complete and submit the request, fill out the required information on all three tabs of the **New support request** form.</span></span>

   ![Formulář žádosti o zvýšení](./media/devtest-lab-scale-lab/devtestlab-support-form.png)

<span data-ttu-id="95cb6-131">[Pochopení omezení Azure a zvyšuje](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests/) poskytuje další informace o kontaktovat podporu Azure o navýšení kvóty.</span><span class="sxs-lookup"><span data-stu-id="95cb6-131">[Understanding Azure Limits and Increases](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests/) provides more information about contacting Azure support to request a quota increase.</span></span>



[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

### <a name="next-steps"></a><span data-ttu-id="95cb6-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="95cb6-132">Next steps</span></span>
* <span data-ttu-id="95cb6-133">Prozkoumejte [Galerie šablon DevTest Labs Azure Resource Manager QuickStart](https://github.com/Azure/azure-devtestlab/tree/master/Samples).</span><span class="sxs-lookup"><span data-stu-id="95cb6-133">Explore the [DevTest Labs Azure Resource Manager QuickStart template gallery](https://github.com/Azure/azure-devtestlab/tree/master/Samples).</span></span>
