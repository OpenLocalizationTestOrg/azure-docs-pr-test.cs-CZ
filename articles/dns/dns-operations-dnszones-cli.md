---
title: "aaaManage DNS zóny v Azure DNS - 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Můžete spravovat pomocí Azure CLI 2.0 zóny DNS. Tento článek ukazuje, jak tooupdate, odstraňte a vytvořte zóny DNS na Azure DNS."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 8ab63bc4-5135-4ed8-8c0b-5f0712b9afed
ms.service: dns
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2017
ms.author: gwallace
ms.openlocfilehash: 3945a558b2db3490e50678d8395a47e55a85c8fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-dns-zones-in-azure-dns-using-hello-azure-cli-20"></a><span data-ttu-id="f5671-104">Jak hello toomanage zóny DNS v Azure DNS pomocí Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="f5671-104">How toomanage DNS Zones in Azure DNS using hello Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="f5671-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f5671-105">Portal</span></span>](dns-operations-dnszones-portal.md)
> * [<span data-ttu-id="f5671-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f5671-106">PowerShell</span></span>](dns-operations-dnszones.md)
> * [<span data-ttu-id="f5671-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="f5671-107">Azure CLI 1.0</span></span>](dns-operations-dnszones-cli-nodejs.md)
> * [<span data-ttu-id="f5671-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="f5671-108">Azure CLI 2.0</span></span>](dns-operations-dnszones-cli.md)


<span data-ttu-id="f5671-109">Tato příručka ukazuje, jak toomanage DNS zóny pomocí hello platformě Azure CLI, která je k dispozici pro Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="f5671-109">This guide shows how toomanage your DNS zones by using hello cross-platform Azure CLI, which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="f5671-110">Můžete také spravovat zóny DNS pomocí [prostředí Azure PowerShell](dns-operations-dnszones.md) nebo hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f5671-110">You can also manage your DNS zones using [Azure PowerShell](dns-operations-dnszones.md) or hello Azure portal.</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="f5671-111">Úloha hello toocomplete verze rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="f5671-111">CLI versions toocomplete hello task</span></span>

<span data-ttu-id="f5671-112">Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="f5671-112">You can complete hello task using one of hello following CLI versions:</span></span>

* <span data-ttu-id="f5671-113">[Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md) -naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modely.</span><span class="sxs-lookup"><span data-stu-id="f5671-113">[Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md) - our CLI for hello classic and resource management deployment models.</span></span>
* <span data-ttu-id="f5671-114">[Azure CLI 2.0](dns-operations-dnszones-cli.md) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="f5671-114">[Azure CLI 2.0](dns-operations-dnszones-cli.md) - our next generation CLI for hello resource management deployment model.</span></span>

## <a name="introduction"></a><span data-ttu-id="f5671-115">Úvod</span><span class="sxs-lookup"><span data-stu-id="f5671-115">Introduction</span></span>

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="set-up-azure-cli-20-for-azure-dns"></a><span data-ttu-id="f5671-116">Nastavení Azure CLI 2.0 pro Azure DNS</span><span class="sxs-lookup"><span data-stu-id="f5671-116">Set up Azure CLI 2.0 for Azure DNS</span></span>

### <a name="before-you-begin"></a><span data-ttu-id="f5671-117">Než začnete</span><span class="sxs-lookup"><span data-stu-id="f5671-117">Before you begin</span></span>

<span data-ttu-id="f5671-118">Ověřte, zda máte následující položky před zahájením konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="f5671-118">Verify that you have hello following items before beginning your configuration.</span></span>

* <span data-ttu-id="f5671-119">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="f5671-119">An Azure subscription.</span></span> <span data-ttu-id="f5671-120">Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f5671-120">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="f5671-121">Nainstalujte nejnovější verzi hello hello 2.0 rozhraní příkazového řádku Azure pro Windows, Linux nebo MAC.</span><span class="sxs-lookup"><span data-stu-id="f5671-121">Install hello latest version of hello Azure CLI 2.0, available for Windows, Linux, or MAC.</span></span> <span data-ttu-id="f5671-122">Další informace najdete v [hello instalace Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="f5671-122">More information is available at [Install hello Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

### <a name="sign-in-tooyour-azure-account"></a><span data-ttu-id="f5671-123">Přihlaste se tooyour účet Azure</span><span class="sxs-lookup"><span data-stu-id="f5671-123">Sign in tooyour Azure account</span></span>

<span data-ttu-id="f5671-124">Otevřete okno konzoly a proveďte ověření pomocí svých přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="f5671-124">Open a console window and authenticate with your credentials.</span></span> <span data-ttu-id="f5671-125">Další informace naleznete v protokolu v tooAzure z hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="f5671-125">For more information, see Log in tooAzure from hello Azure CLI</span></span>

```
az login
```

### <a name="select-hello-subscription"></a><span data-ttu-id="f5671-126">Vyberte předplatné hello</span><span class="sxs-lookup"><span data-stu-id="f5671-126">Select hello subscription</span></span>

<span data-ttu-id="f5671-127">Zkontrolujte předplatná hello pro účet hello.</span><span class="sxs-lookup"><span data-stu-id="f5671-127">Check hello subscriptions for hello account.</span></span>

```
az account list
```

<span data-ttu-id="f5671-128">Zvolte, které vaše toouse předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="f5671-128">Choose which of your Azure subscriptions toouse.</span></span>

```azurecli
az account set --subscription "subscription name"
```

### <a name="create-a-resource-group"></a><span data-ttu-id="f5671-129">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="f5671-129">Create a resource group</span></span>

<span data-ttu-id="f5671-130">Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění.</span><span class="sxs-lookup"><span data-stu-id="f5671-130">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="f5671-131">To slouží jako hello výchozí umístění pro prostředky v příslušné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="f5671-131">This is used as hello default location for resources in that resource group.</span></span> <span data-ttu-id="f5671-132">Ale vzhledem k tomu, že všechny prostředky DNS jsou globální, a ne místní, hello Volba umístění skupiny prostředků nemá žádný vliv na Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="f5671-132">However, because all DNS resources are global, not regional, hello choice of resource group location has no impact on Azure DNS.</span></span>

<span data-ttu-id="f5671-133">Pokud používáte některou ze stávajících skupin prostředků, můžete tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="f5671-133">You can skip this step if you are using an existing resource group.</span></span>

```azurecli
az group create --name myresourcegroup --location "West US"
```

## <a name="getting-help"></a><span data-ttu-id="f5671-134">Získání nápovědy</span><span class="sxs-lookup"><span data-stu-id="f5671-134">Getting help</span></span>

<span data-ttu-id="f5671-135">Všechny příkazy rozhraní příkazového řádku 2.0 týkající se tooAzure DNS začínat `az network dns`.</span><span class="sxs-lookup"><span data-stu-id="f5671-135">All CLI 2.0 commands relating tooAzure DNS start with `az network dns`.</span></span> <span data-ttu-id="f5671-136">Je k dispozici u každého příkazu pomocí hello Nápověda `--help` možnost (krátký tvar `-h`).</span><span class="sxs-lookup"><span data-stu-id="f5671-136">Help is available for each command using hello `--help` option (short form `-h`).</span></span>  <span data-ttu-id="f5671-137">Například:</span><span class="sxs-lookup"><span data-stu-id="f5671-137">For example:</span></span>

```azurecli
az network dns --help
az network dns zone --help
az network dns zone create --help
```

## <a name="create-a-dns-zone"></a><span data-ttu-id="f5671-138">Vytvoření zóny DNS</span><span class="sxs-lookup"><span data-stu-id="f5671-138">Create a DNS zone</span></span>

<span data-ttu-id="f5671-139">Zóny DNS je vytvořený pomocí hello `az network dns zone create` příkaz.</span><span class="sxs-lookup"><span data-stu-id="f5671-139">A DNS zone is created using hello `az network dns zone create` command.</span></span> <span data-ttu-id="f5671-140">Nápovědu získáte příkazem `az network dns zone create -h`.</span><span class="sxs-lookup"><span data-stu-id="f5671-140">For help, see `az network dns zone create -h`.</span></span>

<span data-ttu-id="f5671-141">Hello následující příklad vytvoří zónu DNS s názvem *contoso.com* v hello skupinu prostředků s názvem *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="f5671-141">hello following example creates a DNS zone called *contoso.com* in hello resource group called *MyResourceGroup*:</span></span>

```azurecli
az network dns zone create --resource-group MyResourceGroup --name contoso.com
```

### <a name="toocreate-a-dns-zone-with-tags"></a><span data-ttu-id="f5671-142">toocreate zóny DNS pomocí značek</span><span class="sxs-lookup"><span data-stu-id="f5671-142">toocreate a DNS zone with tags</span></span>

<span data-ttu-id="f5671-143">Hello následující příklad ukazuje, jak toocreate DNS zóny se dvěma [Azure Resource Manager značky](dns-zones-records.md#tags), *project = demo* a *env = test*, pomocí hello `--tags` parametr (krátký tvar `-t`):</span><span class="sxs-lookup"><span data-stu-id="f5671-143">hello following example shows how toocreate a DNS zone with two [Azure Resource Manager tags](dns-zones-records.md#tags), *project = demo* and *env = test*, by using hello `--tags` parameter (short form `-t`):</span></span>

```azurecli
az network dns zone create --resource-group MyResourceGroup --name contoso.com --tags "project=demo" "env=test"
```

## <a name="get-a-dns-zone"></a><span data-ttu-id="f5671-144">Získat zóny DNS</span><span class="sxs-lookup"><span data-stu-id="f5671-144">Get a DNS zone</span></span>

<span data-ttu-id="f5671-145">tooretrieve zóny DNS, použijte `az network dns zone show`.</span><span class="sxs-lookup"><span data-stu-id="f5671-145">tooretrieve a DNS zone, use `az network dns zone show`.</span></span> <span data-ttu-id="f5671-146">Nápovědu získáte příkazem `az network dns zone show --help`.</span><span class="sxs-lookup"><span data-stu-id="f5671-146">For help, see `az network dns zone show --help`.</span></span>

<span data-ttu-id="f5671-147">Hello následující příklad vrací zóny DNS hello *contoso.com* a související data ze skupiny prostředků *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="f5671-147">hello following example returns hello DNS zone *contoso.com* and its associated data from resource group *MyResourceGroup*.</span></span> 

```azurecli
az network dns zone show --resource-group myresourcegroup --name contoso.com
```

<span data-ttu-id="f5671-148">Následující ukázka Hello je hello odpovědi.</span><span class="sxs-lookup"><span data-stu-id="f5671-148">hello following example is hello response.</span></span>

```json
{
  "etag": "00000002-0000-0000-3d4d-64aa3689d201",
  "id": "/subscriptions/147a22e9-2356-4e56-b3de-1f5842ae4a3b/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "contoso.com",
  "nameServers": [
    "ns1-04.azure-dns.com.",
    "ns2-04.azure-dns.net.",
    "ns3-04.azure-dns.org.",
    "ns4-04.azure-dns.info."
  ],
  "numberOfRecordSets": 4,
  "resourceGroup": "myresourcegroup",
  "tags": {},
  "type": "Microsoft.Network/dnszones"
}
```

<span data-ttu-id="f5671-149">Všimněte si, že nejsou vrácené záznamy DNS `az network dns zone show`.</span><span class="sxs-lookup"><span data-stu-id="f5671-149">Note that DNS records are not returned by `az network dns zone show`.</span></span> <span data-ttu-id="f5671-150">toolist záznamy DNS, použijte `az network dns record-set list`.</span><span class="sxs-lookup"><span data-stu-id="f5671-150">toolist DNS records, use `az network dns record-set list`.</span></span>


## <a name="list-dns-zones"></a><span data-ttu-id="f5671-151">Seznam zón DNS</span><span class="sxs-lookup"><span data-stu-id="f5671-151">List DNS zones</span></span>

<span data-ttu-id="f5671-152">tooenumerate zóny DNS, použijte `az network dns zone list`.</span><span class="sxs-lookup"><span data-stu-id="f5671-152">tooenumerate DNS zones, use `az network dns zone list`.</span></span> <span data-ttu-id="f5671-153">Nápovědu získáte příkazem `az network dns zone list --help`.</span><span class="sxs-lookup"><span data-stu-id="f5671-153">For help, see `az network dns zone list --help`.</span></span>

<span data-ttu-id="f5671-154">Zadání skupiny prostředků hello uvádí pouze pásma v rámci skupiny prostředků hello:</span><span class="sxs-lookup"><span data-stu-id="f5671-154">Specifying hello resource group lists only those zones within hello resource group:</span></span>

```azurecli
az network dns zone list --resource-group MyResourceGroup
```

<span data-ttu-id="f5671-155">Vynechání skupiny prostředků hello seznam všech zón v předplatném hello:</span><span class="sxs-lookup"><span data-stu-id="f5671-155">Omitting hello resource group lists all zones in hello subscription:</span></span>

```azurecli
az network dns zone list 
```

## <a name="update-a-dns-zone"></a><span data-ttu-id="f5671-156">Aktualizaci zóny DNS</span><span class="sxs-lookup"><span data-stu-id="f5671-156">Update a DNS zone</span></span>

<span data-ttu-id="f5671-157">Změny tooa prostředků zónu DNS můžete provést pomocí `az network dns zone update`.</span><span class="sxs-lookup"><span data-stu-id="f5671-157">Changes tooa DNS zone resource can be made using `az network dns zone update`.</span></span> <span data-ttu-id="f5671-158">Nápovědu získáte příkazem `az network dns zone update --help`.</span><span class="sxs-lookup"><span data-stu-id="f5671-158">For help, see `az network dns zone update --help`.</span></span>

<span data-ttu-id="f5671-159">Tento příkaz nelze aktualizovat všechny hello sad záznamů DNS v rámci zóny hello (viz [jak záznamy DNS tooManage](dns-operations-recordsets-cli.md)).</span><span class="sxs-lookup"><span data-stu-id="f5671-159">This command does not update any of hello DNS record sets within hello zone (see [How tooManage DNS records](dns-operations-recordsets-cli.md)).</span></span> <span data-ttu-id="f5671-160">Je pouze použité tooupdate vlastnosti prostředku zóny hello, sám sebe.</span><span class="sxs-lookup"><span data-stu-id="f5671-160">It is only used tooupdate properties of hello zone resource itself.</span></span> <span data-ttu-id="f5671-161">Tyto vlastnosti jsou aktuálně omezená toohello [Azure Resource Manager, značky'](dns-zones-records.md#tags) pro prostředek zóny hello.</span><span class="sxs-lookup"><span data-stu-id="f5671-161">These properties are currently limited toohello [Azure Resource Manager 'tags'](dns-zones-records.md#tags) for hello zone resource.</span></span>

<span data-ttu-id="f5671-162">Hello následující příklad ukazuje, jak tooupdate hello značky na zónu DNS.</span><span class="sxs-lookup"><span data-stu-id="f5671-162">hello following example shows how tooupdate hello tags on a DNS zone.</span></span> <span data-ttu-id="f5671-163">existující značky Hello se nahrazují hello hodnota.</span><span class="sxs-lookup"><span data-stu-id="f5671-163">hello existing tags are replaced by hello value specified.</span></span>

```azurecli
az network dns zone update --resource-group myresourcegroup --name contoso.com --set tags.team=support
```

## <a name="delete-a-dns-zone"></a><span data-ttu-id="f5671-164">Odstranit zónu DNS</span><span class="sxs-lookup"><span data-stu-id="f5671-164">Delete a DNS zone</span></span>

<span data-ttu-id="f5671-165">Zóny DNS lze odstranit pomocí `az network dns zone delete`.</span><span class="sxs-lookup"><span data-stu-id="f5671-165">DNS zones can be deleted using `az network dns zone delete`.</span></span> <span data-ttu-id="f5671-166">Nápovědu získáte příkazem `az network dns zone delete --help`.</span><span class="sxs-lookup"><span data-stu-id="f5671-166">For help, see `az network dns zone delete --help`.</span></span>

> [!NOTE]
> <span data-ttu-id="f5671-167">Odstranění zóny DNS také odstraní všechny záznamy DNS v rámci zóny hello.</span><span class="sxs-lookup"><span data-stu-id="f5671-167">Deleting a DNS zone also deletes all DNS records within hello zone.</span></span> <span data-ttu-id="f5671-168">Tuto operaci nelze vrátit zpět.</span><span class="sxs-lookup"><span data-stu-id="f5671-168">This operation cannot be undone.</span></span> <span data-ttu-id="f5671-169">Pokud zónu DNS hello se používá, službám pomocí hello zóny se nezdaří, při odstranění zóny hello.</span><span class="sxs-lookup"><span data-stu-id="f5671-169">If hello DNS zone is in use, services using hello zone will fail when hello zone is deleted.</span></span>
>
><span data-ttu-id="f5671-170">najdete v části tooprotect proti náhodnému zóny odstranění [jak tooprotect DNS zóny a zaznamenává](dns-protect-zones-recordsets.md).</span><span class="sxs-lookup"><span data-stu-id="f5671-170">tooprotect against accidental zone deletion, see [How tooprotect DNS zones and records](dns-protect-zones-recordsets.md).</span></span>

<span data-ttu-id="f5671-171">Tento příkaz zobrazí výzvu k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="f5671-171">This command prompts for confirmation.</span></span> <span data-ttu-id="f5671-172">Hello volitelné `--yes` přepínač potlačí tuto výzvu.</span><span class="sxs-lookup"><span data-stu-id="f5671-172">hello optional `--yes` switch suppresses this prompt.</span></span>

<span data-ttu-id="f5671-173">Hello následující příklad ukazuje, jak toodelete hello zóny *contoso.com* ze skupiny prostředků *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="f5671-173">hello following example shows how toodelete hello zone *contoso.com* from resource group *MyResourceGroup*.</span></span>

```azurecli
az network dns zone delete --resource-group myresourcegroup --name contoso.com
```

## <a name="next-steps"></a><span data-ttu-id="f5671-174">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f5671-174">Next steps</span></span>

<span data-ttu-id="f5671-175">Zjistěte, jak příliš[spravovat sady záznamů a záznamy](dns-getstarted-create-recordset-cli.md) ve vaší zóně DNS.</span><span class="sxs-lookup"><span data-stu-id="f5671-175">Learn how too[manage record sets and records](dns-getstarted-create-recordset-cli.md) in your DNS zone.</span></span>

<span data-ttu-id="f5671-176">Zjistěte, jak příliš[delegovat tooAzure vaší domény DNS](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="f5671-176">Learn how too[delegate your domain tooAzure DNS](dns-domain-delegation.md).</span></span>

