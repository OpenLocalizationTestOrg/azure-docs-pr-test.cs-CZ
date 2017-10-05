---
title: "Správa zón DNS v Azure DNS - portálu Azure | Microsoft Docs"
description: "Můžete spravovat zóny DNS pomocí portálu Azure. Tento článek popisuje, jak k aktualizaci, odstranění a vytvoření zóny DNS na Azure DNS"
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/18/2017
ms.author: gwallace
ms.openlocfilehash: 69a509612e6204fc93dd42bf2fe69cb165b5777c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-manage-dns-zones-in-the-azure-portal"></a><span data-ttu-id="d9fc4-104">Správa zón DNS na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="d9fc4-104">How to manage DNS Zones in the Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="d9fc4-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d9fc4-105">Portal</span></span>](dns-operations-dnszones-portal.md)
> * [<span data-ttu-id="d9fc4-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="d9fc4-106">PowerShell</span></span>](dns-operations-dnszones.md)
> * [<span data-ttu-id="d9fc4-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="d9fc4-107">Azure CLI 1.0</span></span>](dns-operations-dnszones-cli-nodejs.md)
> * [<span data-ttu-id="d9fc4-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="d9fc4-108">Azure CLI 2.0</span></span>](dns-operations-dnszones-cli.md)

<span data-ttu-id="d9fc4-109">Tento článek ukazuje, jak spravovat zóny DNS pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d9fc4-109">This article shows you how to manage your DNS zones by using the Azure portal.</span></span> <span data-ttu-id="d9fc4-110">Můžete také spravovat zóny DNS pomocí napříč platformami [rozhraní příkazového řádku Azure](dns-operations-dnszones-cli.md) nebo Azure [prostředí PowerShell](dns-operations-dnszones.md).</span><span class="sxs-lookup"><span data-stu-id="d9fc4-110">You can also manage your DNS zones using the cross-platform [Azure CLI](dns-operations-dnszones-cli.md) or the Azure [PowerShell](dns-operations-dnszones.md).</span></span>

## <a name="create-a-dns-zone"></a><span data-ttu-id="d9fc4-111">Vytvoření zóny DNS</span><span class="sxs-lookup"><span data-stu-id="d9fc4-111">Create a DNS zone</span></span>

1. <span data-ttu-id="d9fc4-112">Přihlášení k webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d9fc4-112">Sign in to the Azure portal</span></span>
2. <span data-ttu-id="d9fc4-113">V nabídce centra klikněte na **Nový > Sítě >** a potom kliknutím na **Zóna DNS** otevřete okno Vytvořit zónu DNS.</span><span class="sxs-lookup"><span data-stu-id="d9fc4-113">On the Hub menu, click and click **New > Networking >** and then click **DNS zone** to open the Create DNS zone blade.</span></span>

    ![Zóna DNS](./media/dns-operations-dnszones-portal/openzone650.png)

4. <span data-ttu-id="d9fc4-115">V okně **Vytvořit zónu DNS** zadejte následující hodnoty a pak klikněte na **Vytvořit**:</span><span class="sxs-lookup"><span data-stu-id="d9fc4-115">On the **Create DNS zone** blade enter the following values, then click **Create**:</span></span>


   | <span data-ttu-id="d9fc4-116">**Nastavení**</span><span class="sxs-lookup"><span data-stu-id="d9fc4-116">**Setting**</span></span> | <span data-ttu-id="d9fc4-117">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="d9fc4-117">**Value**</span></span> | <span data-ttu-id="d9fc4-118">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="d9fc4-118">**Details**</span></span> |
   |---|---|---|
   |<span data-ttu-id="d9fc4-119">**Název**</span><span class="sxs-lookup"><span data-stu-id="d9fc4-119">**Name**</span></span>|<span data-ttu-id="d9fc4-120">contoso.com</span><span class="sxs-lookup"><span data-stu-id="d9fc4-120">contoso.com</span></span>|<span data-ttu-id="d9fc4-121">Název zóny DNS</span><span class="sxs-lookup"><span data-stu-id="d9fc4-121">The name of the DNS zone</span></span>|
   |<span data-ttu-id="d9fc4-122">**Předplatné**</span><span class="sxs-lookup"><span data-stu-id="d9fc4-122">**Subscription**</span></span>|<span data-ttu-id="d9fc4-123">[Vaše předplatné]</span><span class="sxs-lookup"><span data-stu-id="d9fc4-123">[Your subscription]</span></span>|<span data-ttu-id="d9fc4-124">Vyberte předplatné, ve kterém chcete vytvořit zónu DNS.</span><span class="sxs-lookup"><span data-stu-id="d9fc4-124">Select a subscription to create the DNS zone in.</span></span>|
   |<span data-ttu-id="d9fc4-125">**Skupina prostředků**</span><span class="sxs-lookup"><span data-stu-id="d9fc4-125">**Resource group**</span></span>|<span data-ttu-id="d9fc4-126">**Vytvořit novou:** contosoDNSRG</span><span class="sxs-lookup"><span data-stu-id="d9fc4-126">**Create new:** contosoDNSRG</span></span>|<span data-ttu-id="d9fc4-127">Vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="d9fc4-127">Create a resource group.</span></span> <span data-ttu-id="d9fc4-128">Název skupiny prostředků musí být v rámci vybraného předplatného jedinečný.</span><span class="sxs-lookup"><span data-stu-id="d9fc4-128">The resource group name must be unique within the subscription you selected.</span></span> <span data-ttu-id="d9fc4-129">Další informace o skupinách prostředků najdete v článku s přehledem [Resource Manageru](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups).</span><span class="sxs-lookup"><span data-stu-id="d9fc4-129">To learn more about resource groups, read the [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) overview article.</span></span>|
   |<span data-ttu-id="d9fc4-130">**Umístění**</span><span class="sxs-lookup"><span data-stu-id="d9fc4-130">**Location**</span></span>|<span data-ttu-id="d9fc4-131">Západní USA</span><span class="sxs-lookup"><span data-stu-id="d9fc4-131">West US</span></span>||

> [!NOTE]
> <span data-ttu-id="d9fc4-132">Skupina prostředků označuje umístění skupiny prostředků a nemá žádný vliv na zónu DNS.</span><span class="sxs-lookup"><span data-stu-id="d9fc4-132">The resource group refers to the location of the resource group, and has no impact on the DNS zone.</span></span> <span data-ttu-id="d9fc4-133">Umístění zóny DNS je vždy globální a není zobrazeno.</span><span class="sxs-lookup"><span data-stu-id="d9fc4-133">The DNS zone location is always "global", and is not shown.</span></span>

## <a name="list-dns-zones"></a><span data-ttu-id="d9fc4-134">Seznam zón DNS</span><span class="sxs-lookup"><span data-stu-id="d9fc4-134">List DNS zones</span></span>

<span data-ttu-id="d9fc4-135">Na portálu Azure přejděte do **další služby** > **sítě** > **zóny DNS**.</span><span class="sxs-lookup"><span data-stu-id="d9fc4-135">In the Azure portal, navigate to **More services** > **Networking** > **DNS zones**.</span></span> <span data-ttu-id="d9fc4-136">Každé zóny DNS se jedná vlastní prostředek, informace, jako je počet sad záznamů a názvové servery je možné zobrazit z tohoto zobrazení.</span><span class="sxs-lookup"><span data-stu-id="d9fc4-136">Each DNS zone is it's own resource, information such as number of record-sets and name servers are viewable from this view.</span></span> <span data-ttu-id="d9fc4-137">Sloupec **NÁZVOVÉ servery** není ve výchozím zobrazení, klikněte na tlačítko Přidat **sloupce**, vyberte **názvové servery** a klikněte na tlačítko **provádí**.</span><span class="sxs-lookup"><span data-stu-id="d9fc4-137">The column **NAME SERVERS** is not in the default view, to add it click **Columns**, select **Name servers** and click **Done**.</span></span>

![výpis zóny DNS](./media/dns-operations-dnszones-portal/listzones.png)

## <a name="delete-a-dns-zone"></a><span data-ttu-id="d9fc4-139">Odstranit zónu DNS</span><span class="sxs-lookup"><span data-stu-id="d9fc4-139">Delete a DNS zone</span></span>

<span data-ttu-id="d9fc4-140">Přejděte do zóny DNS na portálu.</span><span class="sxs-lookup"><span data-stu-id="d9fc4-140">Navigate to a DNS zone in the portal.</span></span> <span data-ttu-id="d9fc4-141">Na **zónu DNS** okně klikněte na tlačítko **odstranit zónu**.</span><span class="sxs-lookup"><span data-stu-id="d9fc4-141">On the **DNS zone** blade, click **Delete zone**.</span></span> <span data-ttu-id="d9fc4-142">Zobrazí se výzva k potvrzení, zda že se chtějí odstranit zónu DNS.</span><span class="sxs-lookup"><span data-stu-id="d9fc4-142">You are prompted to confirm you are wanting to delete the DNS zone.</span></span> <span data-ttu-id="d9fc4-143">Odstranění zóny DNS také odstraní všechny záznamy, které jsou obsaženy v zóně.</span><span class="sxs-lookup"><span data-stu-id="d9fc4-143">Deleting a DNS zone also deletes all the records that are contained in the zone.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d9fc4-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d9fc4-144">Next steps</span></span>

<span data-ttu-id="d9fc4-145">Naučte se pracovat s zóny DNS a záznamy, navštivte stránky [Začínáme s Azure DNS pomocí portálu Azure](dns-getstarted-portal.md).</span><span class="sxs-lookup"><span data-stu-id="d9fc4-145">Learn how to work with your DNS Zone and records by visiting [Get started with Azure DNS using the Azure portal](dns-getstarted-portal.md).</span></span>