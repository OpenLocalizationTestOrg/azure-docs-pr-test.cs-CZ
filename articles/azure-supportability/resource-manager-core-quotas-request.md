---
title: "Azure Resource Manager základní kvóta zvýšit požadavky | Microsoft Docs"
description: "Azure Resource Manager základní kvóta zvýšit požadavky"
author: ganganarayanan
ms.author: gangan
ms.date: 1/18/2017
ms.topic: article
ms.service: microsoft-docs
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: cb6c5b3e86f126d4110d1cd29d8c9891e356e414
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="resource-manager-core-quota-increase-requests"></a><span data-ttu-id="f2cf5-103">Základní kvóta správce prostředků zvýšit požadavky</span><span class="sxs-lookup"><span data-stu-id="f2cf5-103">Resource Manager core quota increase requests</span></span>

<span data-ttu-id="f2cf5-104">Základní kvóty správce prostředků se vynucují na úrovni rodiny SKU a úrovně oblast.</span><span class="sxs-lookup"><span data-stu-id="f2cf5-104">Resource Manager core quotas are enforced at the region level and SKU family level.</span></span>
<span data-ttu-id="f2cf5-105">Další informace o tom, jak se vynucují kvóty na [předplatného Azure a omezení služby](http://aka.ms/quotalimits) stránky.</span><span class="sxs-lookup"><span data-stu-id="f2cf5-105">Learn more about how quotas are enforced on the [Azure subscription and service limits](http://aka.ms/quotalimits) page.</span></span>
<span data-ttu-id="f2cf5-106">Další informace o SKU rodiny, může porovnat nákladů a výkonu na [ceny služeb Virtual Machines](http://aka.ms/pricingcompute) stránky.</span><span class="sxs-lookup"><span data-stu-id="f2cf5-106">To learn more about SKU Families, you may compare cost and performance on the [Virtual Machines Pricing](http://aka.ms/pricingcompute) page.</span></span>

<span data-ttu-id="f2cf5-107">Chcete-li požádat o zvýšení, vytvoření případu podpory kvóty pro počet jader na portálu Azure [https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f2cf5-107">To request an increase, create a Quota support case for Cores in the Azure portal, [https://portal.azure.com](https://portal.azure.com).</span></span>

> [!NOTE]
> <span data-ttu-id="f2cf5-108">Zjistěte, jak [vytvořit žádost o podporu](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="f2cf5-108">Learn how to [create a support request](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) in the Azure portal</span></span>

1. <span data-ttu-id="f2cf5-109">Na stránce nové žádosti o podporu vyberte typ problému jako "Kvóty" a typ kvóty jako "Jader".</span><span class="sxs-lookup"><span data-stu-id="f2cf5-109">On the new support request page, select Issue type as "Quota" and Quota type as "Cores".</span></span>

    ![Okno základy kvóty](./media/resource-manager-core-quotas-request/Basics-blade.png)

2. <span data-ttu-id="f2cf5-111">Vyberte model nasazení jako "Resource Manager" a vyberte umístění.</span><span class="sxs-lookup"><span data-stu-id="f2cf5-111">Select Deployment model as "Resource Manager" and select a location.</span></span>

    ![Okno problém kvóty](./media/resource-manager-core-quotas-request/Problem-step.png)

3. <span data-ttu-id="f2cf5-113">Vyberte rodiny SKU, které vyžadují zvětšit.</span><span class="sxs-lookup"><span data-stu-id="f2cf5-113">Select the SKU Families that require an increase.</span></span>

    ![Řada SKU vybrané](./media/resource-manager-core-quotas-request/SKU-selected.png)

4. <span data-ttu-id="f2cf5-115">Zadejte nové omezení, která má být u předplatného.</span><span class="sxs-lookup"><span data-stu-id="f2cf5-115">Enter the new limits you would like on the subscription.</span></span>

    ![Skladová položka novou žádost o kvótu](./media/resource-manager-core-quotas-request/SKU-new-quota.png)

- <span data-ttu-id="f2cf5-117">Chcete-li odebrat, zrušte zaškrtnutí políčka SKU z rozevíracího seznamu rodiny SKU nebo klikněte na ikonu zahození "x".</span><span class="sxs-lookup"><span data-stu-id="f2cf5-117">To remove a line, uncheck the SKU from the SKU family dropdown or click the discard "x" icon.</span></span>
<span data-ttu-id="f2cf5-118">Po zadání požadovaných kvóty pro každou řadu SKU, klikněte na tlačítko "Další" na stránce problém krok a pokračujte vytvoření žádosti o podporu.</span><span class="sxs-lookup"><span data-stu-id="f2cf5-118">After entering the desired quota for each SKU family, click "Next" on the Problem step page to continue with the support request creation.</span></span>
