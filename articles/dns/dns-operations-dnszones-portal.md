---
title: "aaaManage DNS zóny v Azure DNS - portálu Azure | Microsoft Docs"
description: "Můžete spravovat pomocí portálu Azure hello zóny DNS. Tento článek popisuje, jak tooupdate, odstraňte a vytvořte zóny DNS na Azure DNS"
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
ms.openlocfilehash: 0d8ce302bb7126dfe8077a6f3e33418e16fcea64
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-dns-zones-in-hello-azure-portal"></a><span data-ttu-id="18492-104">Jak hello toomanage zóny DNS v portálu Azure</span><span class="sxs-lookup"><span data-stu-id="18492-104">How toomanage DNS Zones in hello Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="18492-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="18492-105">Portal</span></span>](dns-operations-dnszones-portal.md)
> * [<span data-ttu-id="18492-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="18492-106">PowerShell</span></span>](dns-operations-dnszones.md)
> * [<span data-ttu-id="18492-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="18492-107">Azure CLI 1.0</span></span>](dns-operations-dnszones-cli-nodejs.md)
> * [<span data-ttu-id="18492-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="18492-108">Azure CLI 2.0</span></span>](dns-operations-dnszones-cli.md)

<span data-ttu-id="18492-109">Tento článek ukazuje, jak toomanage DNS zóny pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="18492-109">This article shows you how toomanage your DNS zones by using hello Azure portal.</span></span> <span data-ttu-id="18492-110">Můžete také spravovat zóny DNS pomocí hello napříč platformami [rozhraní příkazového řádku Azure](dns-operations-dnszones-cli.md) nebo hello Azure [prostředí PowerShell](dns-operations-dnszones.md).</span><span class="sxs-lookup"><span data-stu-id="18492-110">You can also manage your DNS zones using hello cross-platform [Azure CLI](dns-operations-dnszones-cli.md) or hello Azure [PowerShell](dns-operations-dnszones.md).</span></span>

## <a name="create-a-dns-zone"></a><span data-ttu-id="18492-111">Vytvoření zóny DNS</span><span class="sxs-lookup"><span data-stu-id="18492-111">Create a DNS zone</span></span>

1. <span data-ttu-id="18492-112">Přihlaste se toohello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="18492-112">Sign in toohello Azure portal</span></span>
2. <span data-ttu-id="18492-113">V nabídce centra hello, klikněte na tlačítko a klikněte na tlačítko **nový > sítě >** a pak klikněte na **zónu DNS** tooopen hello vytvoření DNS zóny okno.</span><span class="sxs-lookup"><span data-stu-id="18492-113">On hello Hub menu, click and click **New > Networking >** and then click **DNS zone** tooopen hello Create DNS zone blade.</span></span>

    ![Zóna DNS](./media/dns-operations-dnszones-portal/openzone650.png)

4. <span data-ttu-id="18492-115">Na hello **zóny DNS vytvořte** okno Zadejte hello následující hodnoty a pak klikněte na **vytvořit**:</span><span class="sxs-lookup"><span data-stu-id="18492-115">On hello **Create DNS zone** blade enter hello following values, then click **Create**:</span></span>


   | <span data-ttu-id="18492-116">**Nastavení**</span><span class="sxs-lookup"><span data-stu-id="18492-116">**Setting**</span></span> | <span data-ttu-id="18492-117">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="18492-117">**Value**</span></span> | <span data-ttu-id="18492-118">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="18492-118">**Details**</span></span> |
   |---|---|---|
   |<span data-ttu-id="18492-119">**Název**</span><span class="sxs-lookup"><span data-stu-id="18492-119">**Name**</span></span>|<span data-ttu-id="18492-120">contoso.com</span><span class="sxs-lookup"><span data-stu-id="18492-120">contoso.com</span></span>|<span data-ttu-id="18492-121">Název zóny DNS hello Hello</span><span class="sxs-lookup"><span data-stu-id="18492-121">hello name of hello DNS zone</span></span>|
   |<span data-ttu-id="18492-122">**Předplatné**</span><span class="sxs-lookup"><span data-stu-id="18492-122">**Subscription**</span></span>|<span data-ttu-id="18492-123">[Vaše předplatné]</span><span class="sxs-lookup"><span data-stu-id="18492-123">[Your subscription]</span></span>|<span data-ttu-id="18492-124">Vyberte předplatné zóny DNS hello toocreate v.</span><span class="sxs-lookup"><span data-stu-id="18492-124">Select a subscription toocreate hello DNS zone in.</span></span>|
   |<span data-ttu-id="18492-125">**Skupina prostředků**</span><span class="sxs-lookup"><span data-stu-id="18492-125">**Resource group**</span></span>|<span data-ttu-id="18492-126">**Vytvořit novou:** contosoDNSRG</span><span class="sxs-lookup"><span data-stu-id="18492-126">**Create new:** contosoDNSRG</span></span>|<span data-ttu-id="18492-127">Vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="18492-127">Create a resource group.</span></span> <span data-ttu-id="18492-128">Název skupiny prostředků Hello musí být jedinečný v rámci předplatného hello, který jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="18492-128">hello resource group name must be unique within hello subscription you selected.</span></span> <span data-ttu-id="18492-129">Další informace o skupinách prostředků, přečtěte si hello toolearn [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) článek s přehledem.</span><span class="sxs-lookup"><span data-stu-id="18492-129">toolearn more about resource groups, read hello [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) overview article.</span></span>|
   |<span data-ttu-id="18492-130">**Umístění**</span><span class="sxs-lookup"><span data-stu-id="18492-130">**Location**</span></span>|<span data-ttu-id="18492-131">Západní USA</span><span class="sxs-lookup"><span data-stu-id="18492-131">West US</span></span>||

> [!NOTE]
> <span data-ttu-id="18492-132">Skupina prostředků Hello odkazuje toohello umístění skupiny prostředků hello a nemá žádný vliv na zónu DNS hello.</span><span class="sxs-lookup"><span data-stu-id="18492-132">hello resource group refers toohello location of hello resource group, and has no impact on hello DNS zone.</span></span> <span data-ttu-id="18492-133">umístění zóny DNS Hello je vždy "globální" a není zobrazen.</span><span class="sxs-lookup"><span data-stu-id="18492-133">hello DNS zone location is always "global", and is not shown.</span></span>

## <a name="list-dns-zones"></a><span data-ttu-id="18492-134">Seznam zón DNS</span><span class="sxs-lookup"><span data-stu-id="18492-134">List DNS zones</span></span>

<span data-ttu-id="18492-135">V hello portálu Azure, přejděte příliš**další služby** > **sítě** > **zóny DNS**.</span><span class="sxs-lookup"><span data-stu-id="18492-135">In hello Azure portal, navigate too**More services** > **Networking** > **DNS zones**.</span></span> <span data-ttu-id="18492-136">Každé zóny DNS se jedná vlastní prostředek, informace, jako je počet sad záznamů a názvové servery je možné zobrazit z tohoto zobrazení.</span><span class="sxs-lookup"><span data-stu-id="18492-136">Each DNS zone is it's own resource, information such as number of record-sets and name servers are viewable from this view.</span></span> <span data-ttu-id="18492-137">sloupec Hello **NÁZVOVÉ servery** není v hello výchozí zobrazení tooadd klikněte na tlačítko **sloupce**, vyberte **názvové servery** a klikněte na tlačítko **provádí**.</span><span class="sxs-lookup"><span data-stu-id="18492-137">hello column **NAME SERVERS** is not in hello default view, tooadd it click **Columns**, select **Name servers** and click **Done**.</span></span>

![výpis zóny DNS](./media/dns-operations-dnszones-portal/listzones.png)

## <a name="delete-a-dns-zone"></a><span data-ttu-id="18492-139">Odstranit zónu DNS</span><span class="sxs-lookup"><span data-stu-id="18492-139">Delete a DNS zone</span></span>

<span data-ttu-id="18492-140">Přejděte zóny DNS tooa hello portálu.</span><span class="sxs-lookup"><span data-stu-id="18492-140">Navigate tooa DNS zone in hello portal.</span></span> <span data-ttu-id="18492-141">Na hello **zónu DNS** okně klikněte na tlačítko **odstranit zónu**.</span><span class="sxs-lookup"><span data-stu-id="18492-141">On hello **DNS zone** blade, click **Delete zone**.</span></span> <span data-ttu-id="18492-142">Jste výzvami tooconfirm jsou podmínka mimo zóny DNS toodelete hello.</span><span class="sxs-lookup"><span data-stu-id="18492-142">You are prompted tooconfirm you are wanting toodelete hello DNS zone.</span></span> <span data-ttu-id="18492-143">Odstranění zóny DNS také odstraní všechny hello záznamy, které jsou obsaženy v zóně hello.</span><span class="sxs-lookup"><span data-stu-id="18492-143">Deleting a DNS zone also deletes all hello records that are contained in hello zone.</span></span>

## <a name="next-steps"></a><span data-ttu-id="18492-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="18492-144">Next steps</span></span>

<span data-ttu-id="18492-145">Zjistěte, jak toowork s zóny DNS a záznamy, navštivte stránky [Začínáme s Azure DNS pomocí portálu Azure hello](dns-getstarted-portal.md).</span><span class="sxs-lookup"><span data-stu-id="18492-145">Learn how toowork with your DNS Zone and records by visiting [Get started with Azure DNS using hello Azure portal](dns-getstarted-portal.md).</span></span>
