---
title: "aaaScale kvóty a limity ve svém testovacím prostředí v Azure DevTest Labs | Microsoft Docs"
description: "Zjistěte, jak tooscale testovacího prostředí v Azure DevTest Labs"
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
ms.openlocfilehash: 7fb429c0aabdfbce3a4a5aa6d9259daa2ee270d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scale-quotas-and-limits-in-devtest-labs"></a><span data-ttu-id="1a118-103">Škálování kvóty a omezení v DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="1a118-103">Scale quotas and limits in DevTest Labs</span></span>
<span data-ttu-id="1a118-104">Při práci v DevTest Labs, můžete si všimnout, že existují určitá omezení toosome výchozí prostředky Azure, což může ovlivnit hello DevTest Labs služby.</span><span class="sxs-lookup"><span data-stu-id="1a118-104">As you work in DevTest Labs, you might notice that there are certain default limits toosome Azure resources, which can affect hello DevTest Labs service.</span></span> <span data-ttu-id="1a118-105">Tato omezení jsou odkazované tooas **kvóty**.</span><span class="sxs-lookup"><span data-stu-id="1a118-105">These limits are referred tooas **quotas**.</span></span>

> [!NOTE]
> <span data-ttu-id="1a118-106">Hello DevTest Labs služby nebude ukládat žádné kvóty.</span><span class="sxs-lookup"><span data-stu-id="1a118-106">hello DevTest Labs service doesn't impose any quotas.</span></span> <span data-ttu-id="1a118-107">Žádné kvóty, kterými se můžete setkat se výchozí omezení hello celkové předplatné.</span><span class="sxs-lookup"><span data-stu-id="1a118-107">Any quotas you might encounter are default constraints of hello overall Azure subscription.</span></span>

<span data-ttu-id="1a118-108">Každý prostředek Azure můžete použít, dokud se nedostanete dosáhlo kvóty.</span><span class="sxs-lookup"><span data-stu-id="1a118-108">You can use each Azure resource until you reach its quota.</span></span> <span data-ttu-id="1a118-109">Každé předplatné má samostatné kvóty a sledování využití podle předplatného.</span><span class="sxs-lookup"><span data-stu-id="1a118-109">Each subscription has separate quotas and usage is tracked per subscription.</span></span>

<span data-ttu-id="1a118-110">Například každé předplatné má výchozí kvótu 20 jader.</span><span class="sxs-lookup"><span data-stu-id="1a118-110">For example, each subscription has a default quota of 20 cores.</span></span> <span data-ttu-id="1a118-111">Ano při vytváření virtuálních počítačů ve svém testovacím prostředí s čtyři jader, pak můžete pouze vytvořit pět virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="1a118-111">So, if you are creating VMs in your lab with four cores each, then you can only create five VMs.</span></span> 

<span data-ttu-id="1a118-112">[Azure předplatné a omezení služby](https://docs.microsoft.com/azure/azure-subscription-service-limits) uvádí některé z nejběžnějších kvóty hello k prostředkům Azure.</span><span class="sxs-lookup"><span data-stu-id="1a118-112">[Azure Subscription and Service Limits](https://docs.microsoft.com/azure/azure-subscription-service-limits) lists some of hello most common quotas for Azure resources.</span></span> <span data-ttu-id="1a118-113">Hello prostředky nejčastěji používané v testovacím prostředí a pro které můžete narazit kvóty, zahrnete jader virtuálního počítače, veřejné IP adresy, síťové rozhraní, spravované disky, přiřazení role RBAC, okruhy ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="1a118-113">hello resources most commonly used in a lab, and for which you might encounter quotas, include VM cores, public IP addresses, network interface, managed disks, RBAC role assignment, and ExpressRoute circuits.</span></span>

## <a name="view-your-usage-and-quotas"></a><span data-ttu-id="1a118-114">Zobrazit využití a kvóty</span><span class="sxs-lookup"><span data-stu-id="1a118-114">View your usage and quotas</span></span>
<span data-ttu-id="1a118-115">Tyto kroky vám ukážou, jak tooview hello aktuální kvóty ve vašem předplatném pro konkrétní prostředky Azure a toosee, jaké procento každé kvóty jste už použili.</span><span class="sxs-lookup"><span data-stu-id="1a118-115">These steps show you how tooview hello current quotas in your subscription for specific Azure resources, and toosee what percentage of each quota you have used.</span></span>

1. <span data-ttu-id="1a118-116">Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="1a118-116">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
1. <span data-ttu-id="1a118-117">Vyberte **více služeb**a potom vyberte **fakturace** hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="1a118-117">Select **More Services**, and then select **Billing** from hello list.</span></span>
1. <span data-ttu-id="1a118-118">V okně hello fakturace vyberte předplatné.</span><span class="sxs-lookup"><span data-stu-id="1a118-118">In hello Billing blade, select a subscription.</span></span>
4. <span data-ttu-id="1a118-119">Vyberte **využití + kvóty**.</span><span class="sxs-lookup"><span data-stu-id="1a118-119">Select **Usage + quotas**.</span></span>

   ![Tlačítko využití a kvóty](./media/devtest-lab-scale-lab/devtestlab-usage-and-quotas.png)

   <span data-ttu-id="1a118-121">Hello využití + kvóty zobrazí se okno se seznamem různé prostředky, které jsou k dispozici v procentech tohoto odběru a hello hello kvóty, který se používá na prostředek.</span><span class="sxs-lookup"><span data-stu-id="1a118-121">hello Usage + quotas blade appears, listing different resources available in that subscription and hello percentage of hello quota that is being used per resource.</span></span>

   ![Kvóty a využití](./media/devtest-lab-scale-lab/devtestlab-view-quotas.png)

## <a name="requesting-more-resources-in-your-subscription"></a><span data-ttu-id="1a118-123">Vyžaduje další prostředky ve vašem předplatném</span><span class="sxs-lookup"><span data-stu-id="1a118-123">Requesting more resources in your subscription</span></span>
<span data-ttu-id="1a118-124">Dosažení limitu kvóty hello výchozí limit prostředku v předplatném je možné zvýšit až maximální limit tooa, jak je popsáno v [předplatné Azure a omezení služby](https://docs.microsoft.com/azure/azure-subscription-service-limits).</span><span class="sxs-lookup"><span data-stu-id="1a118-124">If you reach a quota cap, hello default limit of a resource in a subscription can be increased up tooa maximum limit, as described in [Azure Subscription and Service Limits](https://docs.microsoft.com/azure/azure-subscription-service-limits).</span></span>

<span data-ttu-id="1a118-125">Tyto kroky ukazují, jak toorequest kvótu zvýšit prostřednictvím hello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="1a118-125">These steps show you how toorequest a quota increase through hello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

1. <span data-ttu-id="1a118-126">Vyberte **více služeb**, vyberte **fakturace**a potom vyberte **využití + kvóty**.</span><span class="sxs-lookup"><span data-stu-id="1a118-126">Select **More Services**, select **Billing**, and then select **Usage + quotas**.</span></span>
1. <span data-ttu-id="1a118-127">V okně kvóty + hello využití, vyberte hello **požádat o zvýšení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1a118-127">In hello Usage + quotas blade, select hello **Request Increase** button.</span></span>

   ![Žádost o zvýšení tlačítko](./media/devtest-lab-scale-lab/devtestlab-request-increase.png)

1. <span data-ttu-id="1a118-129">toocomplete a odeslat žádost o hello, vyplňte hello požadované informace o všech tří karet hello **nová žádost o podporu** formuláře.</span><span class="sxs-lookup"><span data-stu-id="1a118-129">toocomplete and submit hello request, fill out hello required information on all three tabs of hello **New support request** form.</span></span>

   ![Formulář žádosti o zvýšení](./media/devtest-lab-scale-lab/devtestlab-support-form.png)

<span data-ttu-id="1a118-131">[Pochopení omezení Azure a zvyšuje](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests/) poskytuje další informace o kontaktování podpory Azure toorequest kvótu zvýšit.</span><span class="sxs-lookup"><span data-stu-id="1a118-131">[Understanding Azure Limits and Increases](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests/) provides more information about contacting Azure support toorequest a quota increase.</span></span>



[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

### <a name="next-steps"></a><span data-ttu-id="1a118-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1a118-132">Next steps</span></span>
* <span data-ttu-id="1a118-133">Prozkoumejte hello [Galerie šablon DevTest Labs Azure Resource Manager QuickStart](https://github.com/Azure/azure-devtestlab/tree/master/Samples).</span><span class="sxs-lookup"><span data-stu-id="1a118-133">Explore hello [DevTest Labs Azure Resource Manager QuickStart template gallery](https://github.com/Azure/azure-devtestlab/tree/master/Samples).</span></span>
