---
title: "Správa zón DNS v Azure DNS - 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Můžete spravovat pomocí Azure CLI 1.0 zóny DNS. Tento článek ukazuje, jak k aktualizaci, odstranění a vytvoření zóny DNS na Azure DNS."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 8ab63bc4-5135-4ed8-8c0b-5f0712b9afed
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/21/2016
ms.author: gwallace
ms.openlocfilehash: 588c87749f049eff5b9e0729f6769c8367ba41e4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-manage-dns-zones-in-azure-dns-using-the-azure-cli-10"></a><span data-ttu-id="87df0-104">Správa zón DNS v Azure DNS pomocí Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="87df0-104">How to manage DNS Zones in Azure DNS using the Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="87df0-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="87df0-105">Portal</span></span>](dns-operations-dnszones-portal.md)
> * [<span data-ttu-id="87df0-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="87df0-106">PowerShell</span></span>](dns-operations-dnszones.md)
> * [<span data-ttu-id="87df0-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="87df0-107">Azure CLI 1.0</span></span>](dns-operations-dnszones-cli-nodejs.md)
> * [<span data-ttu-id="87df0-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="87df0-108">Azure CLI 2.0</span></span>](dns-operations-dnszones-cli.md)

<span data-ttu-id="87df0-109">Tato příručka ukazuje, jak spravovat zóny DNS pomocí 1.0 rozhraní příkazového řádku Azure a platformy, která je dostupná pro Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="87df0-109">This guide shows how to manage your DNS zones by using the cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="87df0-110">Můžete také spravovat zóny DNS pomocí [prostředí Azure PowerShell](dns-operations-dnszones.md) nebo portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="87df0-110">You can also manage your DNS zones using [Azure PowerShell](dns-operations-dnszones.md) or the Azure portal.</span></span>

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="87df0-111">Verze rozhraní příkazového řádku pro dokončení úlohy</span><span class="sxs-lookup"><span data-stu-id="87df0-111">CLI versions to complete the task</span></span>

<span data-ttu-id="87df0-112">K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="87df0-112">You can complete the task using one of the following CLI versions:</span></span>

* <span data-ttu-id="87df0-113">[Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md) – naše rozhraní příkazového řádku pro klasické modely nasazení a modely nasazení správy prostředků</span><span class="sxs-lookup"><span data-stu-id="87df0-113">[Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md) - our CLI for the classic and resource management deployment models.</span></span>
* <span data-ttu-id="87df0-114">[Azure CLI 2.0](dns-operations-dnszones-cli.md) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků</span><span class="sxs-lookup"><span data-stu-id="87df0-114">[Azure CLI 2.0](dns-operations-dnszones-cli.md) - our next generation CLI for the resource management deployment model.</span></span>

## <a name="introduction"></a><span data-ttu-id="87df0-115">Úvod</span><span class="sxs-lookup"><span data-stu-id="87df0-115">Introduction</span></span>

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

[!INCLUDE [dns-cli-setup](../../includes/dns-cli-setup-include.md)]

## <a name="getting-help"></a><span data-ttu-id="87df0-116">Získání nápovědy</span><span class="sxs-lookup"><span data-stu-id="87df0-116">Getting help</span></span>

<span data-ttu-id="87df0-117">Všechny příkazy rozhraní příkazového řádku 1.0 týkající se Azure DNS začínat `azure network dns`.</span><span class="sxs-lookup"><span data-stu-id="87df0-117">All CLI 1.0 commands relating to Azure DNS start with `azure network dns`.</span></span> <span data-ttu-id="87df0-118">Je k dispozici pro každý příkaz pomocí Nápověda `--help` možnost (krátký tvar `-h`).</span><span class="sxs-lookup"><span data-stu-id="87df0-118">Help is available for each command using the `--help` option (short form `-h`).</span></span>  <span data-ttu-id="87df0-119">Například:</span><span class="sxs-lookup"><span data-stu-id="87df0-119">For example:</span></span>

```azurecli
azure network dns -h
azure network dns zone -h
azure network dns zone create -h
```

## <a name="create-a-dns-zone"></a><span data-ttu-id="87df0-120">Vytvoření zóny DNS</span><span class="sxs-lookup"><span data-stu-id="87df0-120">Create a DNS zone</span></span>

<span data-ttu-id="87df0-121">Zóna DNS se vytvoří příkazem `azure network dns zone create`.</span><span class="sxs-lookup"><span data-stu-id="87df0-121">A DNS zone is created using the `azure network dns zone create` command.</span></span> <span data-ttu-id="87df0-122">Nápovědu získáte příkazem `azure network dns zone create -h`.</span><span class="sxs-lookup"><span data-stu-id="87df0-122">For help, see `azure network dns zone create -h`.</span></span>

<span data-ttu-id="87df0-123">Následující příklad vytvoří zónu DNS s názvem *contoso.com* ve skupině prostředků s názvem *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="87df0-123">The following example creates a DNS zone called *contoso.com* in the resource group called *MyResourceGroup*:</span></span>

```azurecli
azure network dns zone create MyResourceGroup contoso.com
```

### <a name="to-create-a-dns-zone-with-tags"></a><span data-ttu-id="87df0-124">Vytvoření zóny DNS pomocí značek</span><span class="sxs-lookup"><span data-stu-id="87df0-124">To create a DNS zone with tags</span></span>

<span data-ttu-id="87df0-125">Následující příklad ukazuje, jak vytvořit zónu DNS se dvěma [Azure Resource Manager značky](dns-zones-records.md#tags), *projektu = demo* a *env = test*, pomocí `--tags` parametr (krátký tvar `-t`):</span><span class="sxs-lookup"><span data-stu-id="87df0-125">The following example shows how to create a DNS zone with two [Azure Resource Manager tags](dns-zones-records.md#tags), *project = demo* and *env = test*, by using the `--tags` parameter (short form `-t`):</span></span>

```azurecli
azure network dns zone create MyResourceGroup contoso.com -t "project=demo";"env=test"
```

## <a name="get-a-dns-zone"></a><span data-ttu-id="87df0-126">Získat zóny DNS</span><span class="sxs-lookup"><span data-stu-id="87df0-126">Get a DNS zone</span></span>

<span data-ttu-id="87df0-127">Chcete-li načíst zónu DNS, použijte `azure network dns zone show`.</span><span class="sxs-lookup"><span data-stu-id="87df0-127">To retrieve a DNS zone, use `azure network dns zone show`.</span></span> <span data-ttu-id="87df0-128">Nápovědu získáte příkazem `azure network dns zone show -h`.</span><span class="sxs-lookup"><span data-stu-id="87df0-128">For help, see `azure network dns zone show -h`.</span></span>

<span data-ttu-id="87df0-129">Následující příklad vrací zónu DNS *contoso.com* a související data ze skupiny prostředků *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="87df0-129">The following example returns the DNS zone *contoso.com* and its associated data from resource group *MyResourceGroup*.</span></span> 

```azurecli
azure network dns zone show MyResourceGroup contoso.com
```

<span data-ttu-id="87df0-130">Dalším příkladem je tato odpověď.</span><span class="sxs-lookup"><span data-stu-id="87df0-130">The following example is the response.</span></span>

```
info:    Executing command network dns zone show
+ Looking up the dns zone "contoso.com"
data:    Id                              : /subscriptions/.../contoso.com
data:    Name                            : contoso.com
data:    Type                            : Microsoft.Network/dnszones
data:    Location                        : global
data:    Number of record sets           : 2
data:    Max number of record sets       : 5000
data:    Name servers:
data:        ns1-01.azure-dns.com.
data:        ns2-01.azure-dns.net.
data:        ns3-01.azure-dns.org.
data:        ns4-01.azure-dns.info.
data:    Tags                            : project=demo;env=test
info:    network dns zone show command OK
```

<span data-ttu-id="87df0-131">Všimněte si, že nejsou vrácené záznamy DNS `azure network dns zone show`.</span><span class="sxs-lookup"><span data-stu-id="87df0-131">Note that DNS records are not returned by `azure network dns zone show`.</span></span> <span data-ttu-id="87df0-132">Chcete-li seznam záznamů DNS, použijte `azure network dns record-set list`.</span><span class="sxs-lookup"><span data-stu-id="87df0-132">To list DNS records, use `azure network dns record-set list`.</span></span>


## <a name="list-dns-zones"></a><span data-ttu-id="87df0-133">Seznam zón DNS</span><span class="sxs-lookup"><span data-stu-id="87df0-133">List DNS zones</span></span>

<span data-ttu-id="87df0-134">Chcete-li vytvořit výčet zóny DNS, použijte `azure network dns zone list`.</span><span class="sxs-lookup"><span data-stu-id="87df0-134">To enumerate DNS zones, use `azure network dns zone list`.</span></span> <span data-ttu-id="87df0-135">Nápovědu získáte příkazem `azure network dns zone list -h`.</span><span class="sxs-lookup"><span data-stu-id="87df0-135">For help, see `azure network dns zone list -h`.</span></span>

<span data-ttu-id="87df0-136">Zadání skupiny prostředků jsou uvedeny pouze pásma v rámci skupiny prostředků:</span><span class="sxs-lookup"><span data-stu-id="87df0-136">Specifying the resource group lists only those zones within the resource group:</span></span>

```azurecli
azure network dns zone list MyResourceGroup
```

<span data-ttu-id="87df0-137">Vynechání skupina prostředků obsahuje seznam všech zón v předplatném:</span><span class="sxs-lookup"><span data-stu-id="87df0-137">Omitting the resource group lists all zones in the subscription:</span></span>

```azurecli
azure network dns zone list 
```

## <a name="update-a-dns-zone"></a><span data-ttu-id="87df0-138">Aktualizaci zóny DNS</span><span class="sxs-lookup"><span data-stu-id="87df0-138">Update a DNS zone</span></span>

<span data-ttu-id="87df0-139">Změny k prostředku zóny DNS lze provést pomocí `azure network dns zone set`.</span><span class="sxs-lookup"><span data-stu-id="87df0-139">Changes to a DNS zone resource can be made using `azure network dns zone set`.</span></span> <span data-ttu-id="87df0-140">Nápovědu získáte příkazem `azure network dns zone set -h`.</span><span class="sxs-lookup"><span data-stu-id="87df0-140">For help, see `azure network dns zone set -h`.</span></span>

<span data-ttu-id="87df0-141">Tento příkaz nelze aktualizovat všechny sady záznamů DNS v rámci zóny (viz [postup záznamy DNS spravovat](dns-operations-recordsets-cli-nodejs.md)).</span><span class="sxs-lookup"><span data-stu-id="87df0-141">This command does not update any of the DNS record sets within the zone (see [How to Manage DNS records](dns-operations-recordsets-cli-nodejs.md)).</span></span> <span data-ttu-id="87df0-142">Toto pravidlo se používá pouze k aktualizaci vlastností prostředku zóny sám sebe.</span><span class="sxs-lookup"><span data-stu-id="87df0-142">It is only used to update properties of the zone resource itself.</span></span> <span data-ttu-id="87df0-143">Tyto vlastnosti jsou aktuálně omezená na [Azure Resource Manager, značky'](dns-zones-records.md#tags) prostředku zóny.</span><span class="sxs-lookup"><span data-stu-id="87df0-143">These properties are currently limited to the [Azure Resource Manager 'tags'](dns-zones-records.md#tags) for the zone resource.</span></span>

<span data-ttu-id="87df0-144">Následující příklad ukazuje, jak k aktualizaci značek u zóny DNS.</span><span class="sxs-lookup"><span data-stu-id="87df0-144">The following example shows how to update the tags on a DNS zone.</span></span> <span data-ttu-id="87df0-145">Zadaná hodnota se nahradí stávající značky.</span><span class="sxs-lookup"><span data-stu-id="87df0-145">The existing tags are replaced by the value specified.</span></span>

```azurecli
azure network dns zone set MyResourceGroup contoso.com -t "team=support"
```

## <a name="delete-a-dns-zone"></a><span data-ttu-id="87df0-146">Odstranit zónu DNS</span><span class="sxs-lookup"><span data-stu-id="87df0-146">Delete a DNS Zone</span></span>

<span data-ttu-id="87df0-147">Zóny DNS lze odstranit pomocí `azure network dns zone delete`.</span><span class="sxs-lookup"><span data-stu-id="87df0-147">DNS zones can be deleted using `azure network dns zone delete`.</span></span> <span data-ttu-id="87df0-148">Nápovědu získáte příkazem `azure network dns zone delete -h`.</span><span class="sxs-lookup"><span data-stu-id="87df0-148">For help, see `azure network dns zone delete -h`.</span></span>

> [!NOTE]
> <span data-ttu-id="87df0-149">Odstranění zóny DNS se také odstraní všechny záznamy DNS v rámci zóny.</span><span class="sxs-lookup"><span data-stu-id="87df0-149">Deleting a DNS zone also deletes all DNS records within the zone.</span></span> <span data-ttu-id="87df0-150">Tuto operaci nelze vrátit zpět.</span><span class="sxs-lookup"><span data-stu-id="87df0-150">This operation cannot be undone.</span></span> <span data-ttu-id="87df0-151">Pokud je zóna DNS se používá, službám pomocí zóny selže při odstranění zóny.</span><span class="sxs-lookup"><span data-stu-id="87df0-151">If the DNS zone is in use, services using the zone will fail when the zone is deleted.</span></span>
>
><span data-ttu-id="87df0-152">Chcete-li chránit proti náhodnému zóny odstranění, přečtěte si téma [jak chránit zóny DNS a záznamy](dns-protect-zones-recordsets.md).</span><span class="sxs-lookup"><span data-stu-id="87df0-152">To protect against accidental zone deletion, see [How to protect DNS zones and records](dns-protect-zones-recordsets.md).</span></span>

<span data-ttu-id="87df0-153">Tento příkaz zobrazí výzvu k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="87df0-153">This command prompts for confirmation.</span></span> <span data-ttu-id="87df0-154">Volitelné `--quiet` přepínače (krátký tvar `-q`) potlačí tuto výzvu.</span><span class="sxs-lookup"><span data-stu-id="87df0-154">The optional `--quiet` switch (short form `-q`) suppresses this prompt.</span></span>

<span data-ttu-id="87df0-155">Následující příklad ukazuje, jak odstranit zónu *contoso.com* ze skupiny prostředků *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="87df0-155">The following example shows how to delete the zone *contoso.com* from resource group *MyResourceGroup*.</span></span>

```azurecli
azure network dns zone delete MyResourceGroup contoso.com
```

## <a name="next-steps"></a><span data-ttu-id="87df0-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="87df0-156">Next steps</span></span>

<span data-ttu-id="87df0-157">Zjistěte, jak [spravovat sady záznamů a záznamy](dns-getstarted-create-recordset-cli-nodejs.md) ve vaší zóně DNS.</span><span class="sxs-lookup"><span data-stu-id="87df0-157">Learn how to [manage record sets and records](dns-getstarted-create-recordset-cli-nodejs.md) in your DNS zone.</span></span>

<span data-ttu-id="87df0-158">Zjistěte, jak [delegování domény do Azure DNS](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="87df0-158">Learn how to [delegate your domain to Azure DNS](dns-domain-delegation.md).</span></span>

