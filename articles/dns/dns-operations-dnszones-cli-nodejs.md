---
title: "aaaManage DNS zóny v Azure DNS - 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Můžete spravovat pomocí Azure CLI 1.0 zóny DNS. Tento článek ukazuje, jak tooupdate, odstraňte a vytvořte zóny DNS na Azure DNS."
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
ms.openlocfilehash: cb9790cc46626ef7f38a43edb57511104fe6057e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-dns-zones-in-azure-dns-using-hello-azure-cli-10"></a><span data-ttu-id="8e00c-104">Jak hello toomanage zóny DNS v Azure DNS pomocí Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="8e00c-104">How toomanage DNS Zones in Azure DNS using hello Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="8e00c-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="8e00c-105">Portal</span></span>](dns-operations-dnszones-portal.md)
> * [<span data-ttu-id="8e00c-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8e00c-106">PowerShell</span></span>](dns-operations-dnszones.md)
> * [<span data-ttu-id="8e00c-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="8e00c-107">Azure CLI 1.0</span></span>](dns-operations-dnszones-cli-nodejs.md)
> * [<span data-ttu-id="8e00c-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="8e00c-108">Azure CLI 2.0</span></span>](dns-operations-dnszones-cli.md)

<span data-ttu-id="8e00c-109">Tato příručka ukazuje, jak toomanage DNS zóny pomocí hello platformě Azure CLI 1.0, která je k dispozici pro Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="8e00c-109">This guide shows how toomanage your DNS zones by using hello cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="8e00c-110">Můžete také spravovat zóny DNS pomocí [prostředí Azure PowerShell](dns-operations-dnszones.md) nebo hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8e00c-110">You can also manage your DNS zones using [Azure PowerShell](dns-operations-dnszones.md) or hello Azure portal.</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="8e00c-111">Úloha hello toocomplete verze rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="8e00c-111">CLI versions toocomplete hello task</span></span>

<span data-ttu-id="8e00c-112">Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="8e00c-112">You can complete hello task using one of hello following CLI versions:</span></span>

* <span data-ttu-id="8e00c-113">[Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md) -naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modely.</span><span class="sxs-lookup"><span data-stu-id="8e00c-113">[Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md) - our CLI for hello classic and resource management deployment models.</span></span>
* <span data-ttu-id="8e00c-114">[Azure CLI 2.0](dns-operations-dnszones-cli.md) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="8e00c-114">[Azure CLI 2.0](dns-operations-dnszones-cli.md) - our next generation CLI for hello resource management deployment model.</span></span>

## <a name="introduction"></a><span data-ttu-id="8e00c-115">Úvod</span><span class="sxs-lookup"><span data-stu-id="8e00c-115">Introduction</span></span>

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

[!INCLUDE [dns-cli-setup](../../includes/dns-cli-setup-include.md)]

## <a name="getting-help"></a><span data-ttu-id="8e00c-116">Získání nápovědy</span><span class="sxs-lookup"><span data-stu-id="8e00c-116">Getting help</span></span>

<span data-ttu-id="8e00c-117">Všechny příkazy rozhraní příkazového řádku 1.0 týkající se tooAzure DNS začínat `azure network dns`.</span><span class="sxs-lookup"><span data-stu-id="8e00c-117">All CLI 1.0 commands relating tooAzure DNS start with `azure network dns`.</span></span> <span data-ttu-id="8e00c-118">Je k dispozici u každého příkazu pomocí hello Nápověda `--help` možnost (krátký tvar `-h`).</span><span class="sxs-lookup"><span data-stu-id="8e00c-118">Help is available for each command using hello `--help` option (short form `-h`).</span></span>  <span data-ttu-id="8e00c-119">Například:</span><span class="sxs-lookup"><span data-stu-id="8e00c-119">For example:</span></span>

```azurecli
azure network dns -h
azure network dns zone -h
azure network dns zone create -h
```

## <a name="create-a-dns-zone"></a><span data-ttu-id="8e00c-120">Vytvoření zóny DNS</span><span class="sxs-lookup"><span data-stu-id="8e00c-120">Create a DNS zone</span></span>

<span data-ttu-id="8e00c-121">Zóny DNS je vytvořený pomocí hello `azure network dns zone create` příkaz.</span><span class="sxs-lookup"><span data-stu-id="8e00c-121">A DNS zone is created using hello `azure network dns zone create` command.</span></span> <span data-ttu-id="8e00c-122">Nápovědu získáte příkazem `azure network dns zone create -h`.</span><span class="sxs-lookup"><span data-stu-id="8e00c-122">For help, see `azure network dns zone create -h`.</span></span>

<span data-ttu-id="8e00c-123">Hello následující příklad vytvoří zónu DNS s názvem *contoso.com* v hello skupinu prostředků s názvem *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="8e00c-123">hello following example creates a DNS zone called *contoso.com* in hello resource group called *MyResourceGroup*:</span></span>

```azurecli
azure network dns zone create MyResourceGroup contoso.com
```

### <a name="toocreate-a-dns-zone-with-tags"></a><span data-ttu-id="8e00c-124">toocreate zóny DNS pomocí značek</span><span class="sxs-lookup"><span data-stu-id="8e00c-124">toocreate a DNS zone with tags</span></span>

<span data-ttu-id="8e00c-125">Hello následující příklad ukazuje, jak toocreate DNS zóny se dvěma [Azure Resource Manager značky](dns-zones-records.md#tags), *project = demo* a *env = test*, pomocí hello `--tags` parametr (krátký tvar `-t`):</span><span class="sxs-lookup"><span data-stu-id="8e00c-125">hello following example shows how toocreate a DNS zone with two [Azure Resource Manager tags](dns-zones-records.md#tags), *project = demo* and *env = test*, by using hello `--tags` parameter (short form `-t`):</span></span>

```azurecli
azure network dns zone create MyResourceGroup contoso.com -t "project=demo";"env=test"
```

## <a name="get-a-dns-zone"></a><span data-ttu-id="8e00c-126">Získat zóny DNS</span><span class="sxs-lookup"><span data-stu-id="8e00c-126">Get a DNS zone</span></span>

<span data-ttu-id="8e00c-127">tooretrieve zóny DNS, použijte `azure network dns zone show`.</span><span class="sxs-lookup"><span data-stu-id="8e00c-127">tooretrieve a DNS zone, use `azure network dns zone show`.</span></span> <span data-ttu-id="8e00c-128">Nápovědu získáte příkazem `azure network dns zone show -h`.</span><span class="sxs-lookup"><span data-stu-id="8e00c-128">For help, see `azure network dns zone show -h`.</span></span>

<span data-ttu-id="8e00c-129">Hello následující příklad vrací zóny DNS hello *contoso.com* a související data ze skupiny prostředků *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="8e00c-129">hello following example returns hello DNS zone *contoso.com* and its associated data from resource group *MyResourceGroup*.</span></span> 

```azurecli
azure network dns zone show MyResourceGroup contoso.com
```

<span data-ttu-id="8e00c-130">Následující ukázka Hello je hello odpovědi.</span><span class="sxs-lookup"><span data-stu-id="8e00c-130">hello following example is hello response.</span></span>

```
info:    Executing command network dns zone show
+ Looking up hello dns zone "contoso.com"
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

<span data-ttu-id="8e00c-131">Všimněte si, že nejsou vrácené záznamy DNS `azure network dns zone show`.</span><span class="sxs-lookup"><span data-stu-id="8e00c-131">Note that DNS records are not returned by `azure network dns zone show`.</span></span> <span data-ttu-id="8e00c-132">toolist záznamy DNS, použijte `azure network dns record-set list`.</span><span class="sxs-lookup"><span data-stu-id="8e00c-132">toolist DNS records, use `azure network dns record-set list`.</span></span>


## <a name="list-dns-zones"></a><span data-ttu-id="8e00c-133">Seznam zón DNS</span><span class="sxs-lookup"><span data-stu-id="8e00c-133">List DNS zones</span></span>

<span data-ttu-id="8e00c-134">tooenumerate zóny DNS, použijte `azure network dns zone list`.</span><span class="sxs-lookup"><span data-stu-id="8e00c-134">tooenumerate DNS zones, use `azure network dns zone list`.</span></span> <span data-ttu-id="8e00c-135">Nápovědu získáte příkazem `azure network dns zone list -h`.</span><span class="sxs-lookup"><span data-stu-id="8e00c-135">For help, see `azure network dns zone list -h`.</span></span>

<span data-ttu-id="8e00c-136">Zadání skupiny prostředků hello uvádí pouze pásma v rámci skupiny prostředků hello:</span><span class="sxs-lookup"><span data-stu-id="8e00c-136">Specifying hello resource group lists only those zones within hello resource group:</span></span>

```azurecli
azure network dns zone list MyResourceGroup
```

<span data-ttu-id="8e00c-137">Vynechání skupiny prostředků hello seznam všech zón v předplatném hello:</span><span class="sxs-lookup"><span data-stu-id="8e00c-137">Omitting hello resource group lists all zones in hello subscription:</span></span>

```azurecli
azure network dns zone list 
```

## <a name="update-a-dns-zone"></a><span data-ttu-id="8e00c-138">Aktualizaci zóny DNS</span><span class="sxs-lookup"><span data-stu-id="8e00c-138">Update a DNS zone</span></span>

<span data-ttu-id="8e00c-139">Změny tooa prostředků zónu DNS můžete provést pomocí `azure network dns zone set`.</span><span class="sxs-lookup"><span data-stu-id="8e00c-139">Changes tooa DNS zone resource can be made using `azure network dns zone set`.</span></span> <span data-ttu-id="8e00c-140">Nápovědu získáte příkazem `azure network dns zone set -h`.</span><span class="sxs-lookup"><span data-stu-id="8e00c-140">For help, see `azure network dns zone set -h`.</span></span>

<span data-ttu-id="8e00c-141">Tento příkaz nelze aktualizovat všechny hello sad záznamů DNS v rámci zóny hello (viz [jak záznamy DNS tooManage](dns-operations-recordsets-cli-nodejs.md)).</span><span class="sxs-lookup"><span data-stu-id="8e00c-141">This command does not update any of hello DNS record sets within hello zone (see [How tooManage DNS records](dns-operations-recordsets-cli-nodejs.md)).</span></span> <span data-ttu-id="8e00c-142">Je pouze použité tooupdate vlastnosti prostředku zóny hello, sám sebe.</span><span class="sxs-lookup"><span data-stu-id="8e00c-142">It is only used tooupdate properties of hello zone resource itself.</span></span> <span data-ttu-id="8e00c-143">Tyto vlastnosti jsou aktuálně omezená toohello [Azure Resource Manager, značky'](dns-zones-records.md#tags) pro prostředek zóny hello.</span><span class="sxs-lookup"><span data-stu-id="8e00c-143">These properties are currently limited toohello [Azure Resource Manager 'tags'](dns-zones-records.md#tags) for hello zone resource.</span></span>

<span data-ttu-id="8e00c-144">Hello následující příklad ukazuje, jak tooupdate hello značky na zónu DNS.</span><span class="sxs-lookup"><span data-stu-id="8e00c-144">hello following example shows how tooupdate hello tags on a DNS zone.</span></span> <span data-ttu-id="8e00c-145">existující značky Hello se nahrazují hello hodnota.</span><span class="sxs-lookup"><span data-stu-id="8e00c-145">hello existing tags are replaced by hello value specified.</span></span>

```azurecli
azure network dns zone set MyResourceGroup contoso.com -t "team=support"
```

## <a name="delete-a-dns-zone"></a><span data-ttu-id="8e00c-146">Odstranit zónu DNS</span><span class="sxs-lookup"><span data-stu-id="8e00c-146">Delete a DNS Zone</span></span>

<span data-ttu-id="8e00c-147">Zóny DNS lze odstranit pomocí `azure network dns zone delete`.</span><span class="sxs-lookup"><span data-stu-id="8e00c-147">DNS zones can be deleted using `azure network dns zone delete`.</span></span> <span data-ttu-id="8e00c-148">Nápovědu získáte příkazem `azure network dns zone delete -h`.</span><span class="sxs-lookup"><span data-stu-id="8e00c-148">For help, see `azure network dns zone delete -h`.</span></span>

> [!NOTE]
> <span data-ttu-id="8e00c-149">Odstranění zóny DNS také odstraní všechny záznamy DNS v rámci zóny hello.</span><span class="sxs-lookup"><span data-stu-id="8e00c-149">Deleting a DNS zone also deletes all DNS records within hello zone.</span></span> <span data-ttu-id="8e00c-150">Tuto operaci nelze vrátit zpět.</span><span class="sxs-lookup"><span data-stu-id="8e00c-150">This operation cannot be undone.</span></span> <span data-ttu-id="8e00c-151">Pokud zónu DNS hello se používá, službám pomocí hello zóny se nezdaří, při odstranění zóny hello.</span><span class="sxs-lookup"><span data-stu-id="8e00c-151">If hello DNS zone is in use, services using hello zone will fail when hello zone is deleted.</span></span>
>
><span data-ttu-id="8e00c-152">najdete v části tooprotect proti náhodnému zóny odstranění [jak tooprotect DNS zóny a zaznamenává](dns-protect-zones-recordsets.md).</span><span class="sxs-lookup"><span data-stu-id="8e00c-152">tooprotect against accidental zone deletion, see [How tooprotect DNS zones and records](dns-protect-zones-recordsets.md).</span></span>

<span data-ttu-id="8e00c-153">Tento příkaz zobrazí výzvu k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="8e00c-153">This command prompts for confirmation.</span></span> <span data-ttu-id="8e00c-154">Hello volitelné `--quiet` přepínače (krátký tvar `-q`) potlačí tuto výzvu.</span><span class="sxs-lookup"><span data-stu-id="8e00c-154">hello optional `--quiet` switch (short form `-q`) suppresses this prompt.</span></span>

<span data-ttu-id="8e00c-155">Hello následující příklad ukazuje, jak toodelete hello zóny *contoso.com* ze skupiny prostředků *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="8e00c-155">hello following example shows how toodelete hello zone *contoso.com* from resource group *MyResourceGroup*.</span></span>

```azurecli
azure network dns zone delete MyResourceGroup contoso.com
```

## <a name="next-steps"></a><span data-ttu-id="8e00c-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8e00c-156">Next steps</span></span>

<span data-ttu-id="8e00c-157">Zjistěte, jak příliš[spravovat sady záznamů a záznamy](dns-getstarted-create-recordset-cli-nodejs.md) ve vaší zóně DNS.</span><span class="sxs-lookup"><span data-stu-id="8e00c-157">Learn how too[manage record sets and records](dns-getstarted-create-recordset-cli-nodejs.md) in your DNS zone.</span></span>

<span data-ttu-id="8e00c-158">Zjistěte, jak příliš[delegovat tooAzure vaší domény DNS](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="8e00c-158">Learn how too[delegate your domain tooAzure DNS](dns-domain-delegation.md).</span></span>

