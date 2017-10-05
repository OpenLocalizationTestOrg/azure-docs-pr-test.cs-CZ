---
title: "Správa zón DNS v Azure DNS - 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Můžete spravovat pomocí Azure CLI 2.0 zóny DNS. Tento článek ukazuje, jak k aktualizaci, odstranění a vytvoření zóny DNS na Azure DNS."
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
ms.openlocfilehash: 1414baf9e51d648cc3a46c4f8635040b4d276910
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-manage-dns-zones-in-azure-dns-using-the-azure-cli-20"></a><span data-ttu-id="a33da-104">Správa zón DNS v Azure DNS pomocí Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="a33da-104">How to manage DNS Zones in Azure DNS using the Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="a33da-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a33da-105">Portal</span></span>](dns-operations-dnszones-portal.md)
> * [<span data-ttu-id="a33da-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a33da-106">PowerShell</span></span>](dns-operations-dnszones.md)
> * [<span data-ttu-id="a33da-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="a33da-107">Azure CLI 1.0</span></span>](dns-operations-dnszones-cli-nodejs.md)
> * [<span data-ttu-id="a33da-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="a33da-108">Azure CLI 2.0</span></span>](dns-operations-dnszones-cli.md)


<span data-ttu-id="a33da-109">Tato příručka ukazuje, jak spravovat zóny DNS pomocí rozhraní příkazového řádku Azure a platformy, která je k dispozici pro Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="a33da-109">This guide shows how to manage your DNS zones by using the cross-platform Azure CLI, which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="a33da-110">Můžete také spravovat zóny DNS pomocí [prostředí Azure PowerShell](dns-operations-dnszones.md) nebo portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a33da-110">You can also manage your DNS zones using [Azure PowerShell](dns-operations-dnszones.md) or the Azure portal.</span></span>

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="a33da-111">Verze rozhraní příkazového řádku pro dokončení úlohy</span><span class="sxs-lookup"><span data-stu-id="a33da-111">CLI versions to complete the task</span></span>

<span data-ttu-id="a33da-112">K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="a33da-112">You can complete the task using one of the following CLI versions:</span></span>

* <span data-ttu-id="a33da-113">[Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md) – naše rozhraní příkazového řádku pro klasické modely nasazení a modely nasazení správy prostředků</span><span class="sxs-lookup"><span data-stu-id="a33da-113">[Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md) - our CLI for the classic and resource management deployment models.</span></span>
* <span data-ttu-id="a33da-114">[Azure CLI 2.0](dns-operations-dnszones-cli.md) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků</span><span class="sxs-lookup"><span data-stu-id="a33da-114">[Azure CLI 2.0](dns-operations-dnszones-cli.md) - our next generation CLI for the resource management deployment model.</span></span>

## <a name="introduction"></a><span data-ttu-id="a33da-115">Úvod</span><span class="sxs-lookup"><span data-stu-id="a33da-115">Introduction</span></span>

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="set-up-azure-cli-20-for-azure-dns"></a><span data-ttu-id="a33da-116">Nastavení Azure CLI 2.0 pro Azure DNS</span><span class="sxs-lookup"><span data-stu-id="a33da-116">Set up Azure CLI 2.0 for Azure DNS</span></span>

### <a name="before-you-begin"></a><span data-ttu-id="a33da-117">Než začnete</span><span class="sxs-lookup"><span data-stu-id="a33da-117">Before you begin</span></span>

<span data-ttu-id="a33da-118">Před zahájením konfigurace ověřte, zda máte následující.</span><span class="sxs-lookup"><span data-stu-id="a33da-118">Verify that you have the following items before beginning your configuration.</span></span>

* <span data-ttu-id="a33da-119">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="a33da-119">An Azure subscription.</span></span> <span data-ttu-id="a33da-120">Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a33da-120">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="a33da-121">Nainstalujte nejnovější verzi rozhraní Azure CLI 2.0, k dispozici pro Windows, Linux nebo MAC.</span><span class="sxs-lookup"><span data-stu-id="a33da-121">Install the latest version of the Azure CLI 2.0, available for Windows, Linux, or MAC.</span></span> <span data-ttu-id="a33da-122">Další informace najdete v tématu [Instalace rozhraní příkazového řádku Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="a33da-122">More information is available at [Install the Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

### <a name="sign-in-to-your-azure-account"></a><span data-ttu-id="a33da-123">Přihlášení k účtu Azure</span><span class="sxs-lookup"><span data-stu-id="a33da-123">Sign in to your Azure account</span></span>

<span data-ttu-id="a33da-124">Otevřete okno konzoly a proveďte ověření pomocí svých přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="a33da-124">Open a console window and authenticate with your credentials.</span></span> <span data-ttu-id="a33da-125">Další informace najdete v tématu Přihlášení k Azure z rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="a33da-125">For more information, see Log in to Azure from the Azure CLI</span></span>

```
az login
```

### <a name="select-the-subscription"></a><span data-ttu-id="a33da-126">Výběr předplatného</span><span class="sxs-lookup"><span data-stu-id="a33da-126">Select the subscription</span></span>

<span data-ttu-id="a33da-127">Zkontrolujte předplatná pro příslušný účet.</span><span class="sxs-lookup"><span data-stu-id="a33da-127">Check the subscriptions for the account.</span></span>

```
az account list
```

<span data-ttu-id="a33da-128">Zvolte předplatné Azure, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="a33da-128">Choose which of your Azure subscriptions to use.</span></span>

```azurecli
az account set --subscription "subscription name"
```

### <a name="create-a-resource-group"></a><span data-ttu-id="a33da-129">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="a33da-129">Create a resource group</span></span>

<span data-ttu-id="a33da-130">Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění.</span><span class="sxs-lookup"><span data-stu-id="a33da-130">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="a33da-131">To slouží jako výchozí umístění pro prostředky v příslušné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="a33da-131">This is used as the default location for resources in that resource group.</span></span> <span data-ttu-id="a33da-132">Všechny prostředky DNS jsou ale globální, a ne místní, takže volba umístění skupiny prostředků nemá na Azure DNS žádný vliv.</span><span class="sxs-lookup"><span data-stu-id="a33da-132">However, because all DNS resources are global, not regional, the choice of resource group location has no impact on Azure DNS.</span></span>

<span data-ttu-id="a33da-133">Pokud používáte některou ze stávajících skupin prostředků, můžete tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="a33da-133">You can skip this step if you are using an existing resource group.</span></span>

```azurecli
az group create --name myresourcegroup --location "West US"
```

## <a name="getting-help"></a><span data-ttu-id="a33da-134">Získání nápovědy</span><span class="sxs-lookup"><span data-stu-id="a33da-134">Getting help</span></span>

<span data-ttu-id="a33da-135">Všechny příkazy rozhraní příkazového řádku 2.0 týkající se Azure DNS začínat `az network dns`.</span><span class="sxs-lookup"><span data-stu-id="a33da-135">All CLI 2.0 commands relating to Azure DNS start with `az network dns`.</span></span> <span data-ttu-id="a33da-136">Je k dispozici pro každý příkaz pomocí Nápověda `--help` možnost (krátký tvar `-h`).</span><span class="sxs-lookup"><span data-stu-id="a33da-136">Help is available for each command using the `--help` option (short form `-h`).</span></span>  <span data-ttu-id="a33da-137">Například:</span><span class="sxs-lookup"><span data-stu-id="a33da-137">For example:</span></span>

```azurecli
az network dns --help
az network dns zone --help
az network dns zone create --help
```

## <a name="create-a-dns-zone"></a><span data-ttu-id="a33da-138">Vytvoření zóny DNS</span><span class="sxs-lookup"><span data-stu-id="a33da-138">Create a DNS zone</span></span>

<span data-ttu-id="a33da-139">Zóna DNS se vytvoří příkazem `az network dns zone create`.</span><span class="sxs-lookup"><span data-stu-id="a33da-139">A DNS zone is created using the `az network dns zone create` command.</span></span> <span data-ttu-id="a33da-140">Nápovědu získáte příkazem `az network dns zone create -h`.</span><span class="sxs-lookup"><span data-stu-id="a33da-140">For help, see `az network dns zone create -h`.</span></span>

<span data-ttu-id="a33da-141">Následující příklad vytvoří zónu DNS s názvem *contoso.com* ve skupině prostředků s názvem *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="a33da-141">The following example creates a DNS zone called *contoso.com* in the resource group called *MyResourceGroup*:</span></span>

```azurecli
az network dns zone create --resource-group MyResourceGroup --name contoso.com
```

### <a name="to-create-a-dns-zone-with-tags"></a><span data-ttu-id="a33da-142">Vytvoření zóny DNS pomocí značek</span><span class="sxs-lookup"><span data-stu-id="a33da-142">To create a DNS zone with tags</span></span>

<span data-ttu-id="a33da-143">Následující příklad ukazuje, jak vytvořit zónu DNS se dvěma [Azure Resource Manager značky](dns-zones-records.md#tags), *projektu = demo* a *env = test*, pomocí `--tags` parametr (krátký tvar `-t`):</span><span class="sxs-lookup"><span data-stu-id="a33da-143">The following example shows how to create a DNS zone with two [Azure Resource Manager tags](dns-zones-records.md#tags), *project = demo* and *env = test*, by using the `--tags` parameter (short form `-t`):</span></span>

```azurecli
az network dns zone create --resource-group MyResourceGroup --name contoso.com --tags "project=demo" "env=test"
```

## <a name="get-a-dns-zone"></a><span data-ttu-id="a33da-144">Získat zóny DNS</span><span class="sxs-lookup"><span data-stu-id="a33da-144">Get a DNS zone</span></span>

<span data-ttu-id="a33da-145">Chcete-li načíst zónu DNS, použijte `az network dns zone show`.</span><span class="sxs-lookup"><span data-stu-id="a33da-145">To retrieve a DNS zone, use `az network dns zone show`.</span></span> <span data-ttu-id="a33da-146">Nápovědu získáte příkazem `az network dns zone show --help`.</span><span class="sxs-lookup"><span data-stu-id="a33da-146">For help, see `az network dns zone show --help`.</span></span>

<span data-ttu-id="a33da-147">Následující příklad vrací zónu DNS *contoso.com* a související data ze skupiny prostředků *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="a33da-147">The following example returns the DNS zone *contoso.com* and its associated data from resource group *MyResourceGroup*.</span></span> 

```azurecli
az network dns zone show --resource-group myresourcegroup --name contoso.com
```

<span data-ttu-id="a33da-148">Dalším příkladem je tato odpověď.</span><span class="sxs-lookup"><span data-stu-id="a33da-148">The following example is the response.</span></span>

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

<span data-ttu-id="a33da-149">Všimněte si, že nejsou vrácené záznamy DNS `az network dns zone show`.</span><span class="sxs-lookup"><span data-stu-id="a33da-149">Note that DNS records are not returned by `az network dns zone show`.</span></span> <span data-ttu-id="a33da-150">Chcete-li seznam záznamů DNS, použijte `az network dns record-set list`.</span><span class="sxs-lookup"><span data-stu-id="a33da-150">To list DNS records, use `az network dns record-set list`.</span></span>


## <a name="list-dns-zones"></a><span data-ttu-id="a33da-151">Seznam zón DNS</span><span class="sxs-lookup"><span data-stu-id="a33da-151">List DNS zones</span></span>

<span data-ttu-id="a33da-152">Chcete-li vytvořit výčet zóny DNS, použijte `az network dns zone list`.</span><span class="sxs-lookup"><span data-stu-id="a33da-152">To enumerate DNS zones, use `az network dns zone list`.</span></span> <span data-ttu-id="a33da-153">Nápovědu získáte příkazem `az network dns zone list --help`.</span><span class="sxs-lookup"><span data-stu-id="a33da-153">For help, see `az network dns zone list --help`.</span></span>

<span data-ttu-id="a33da-154">Zadání skupiny prostředků jsou uvedeny pouze pásma v rámci skupiny prostředků:</span><span class="sxs-lookup"><span data-stu-id="a33da-154">Specifying the resource group lists only those zones within the resource group:</span></span>

```azurecli
az network dns zone list --resource-group MyResourceGroup
```

<span data-ttu-id="a33da-155">Vynechání skupina prostředků obsahuje seznam všech zón v předplatném:</span><span class="sxs-lookup"><span data-stu-id="a33da-155">Omitting the resource group lists all zones in the subscription:</span></span>

```azurecli
az network dns zone list 
```

## <a name="update-a-dns-zone"></a><span data-ttu-id="a33da-156">Aktualizaci zóny DNS</span><span class="sxs-lookup"><span data-stu-id="a33da-156">Update a DNS zone</span></span>

<span data-ttu-id="a33da-157">Změny k prostředku zóny DNS lze provést pomocí `az network dns zone update`.</span><span class="sxs-lookup"><span data-stu-id="a33da-157">Changes to a DNS zone resource can be made using `az network dns zone update`.</span></span> <span data-ttu-id="a33da-158">Nápovědu získáte příkazem `az network dns zone update --help`.</span><span class="sxs-lookup"><span data-stu-id="a33da-158">For help, see `az network dns zone update --help`.</span></span>

<span data-ttu-id="a33da-159">Tento příkaz nelze aktualizovat všechny sady záznamů DNS v rámci zóny (viz [postup záznamy DNS spravovat](dns-operations-recordsets-cli.md)).</span><span class="sxs-lookup"><span data-stu-id="a33da-159">This command does not update any of the DNS record sets within the zone (see [How to Manage DNS records](dns-operations-recordsets-cli.md)).</span></span> <span data-ttu-id="a33da-160">Toto pravidlo se používá pouze k aktualizaci vlastností prostředku zóny sám sebe.</span><span class="sxs-lookup"><span data-stu-id="a33da-160">It is only used to update properties of the zone resource itself.</span></span> <span data-ttu-id="a33da-161">Tyto vlastnosti jsou aktuálně omezená na [Azure Resource Manager, značky'](dns-zones-records.md#tags) prostředku zóny.</span><span class="sxs-lookup"><span data-stu-id="a33da-161">These properties are currently limited to the [Azure Resource Manager 'tags'](dns-zones-records.md#tags) for the zone resource.</span></span>

<span data-ttu-id="a33da-162">Následující příklad ukazuje, jak k aktualizaci značek u zóny DNS.</span><span class="sxs-lookup"><span data-stu-id="a33da-162">The following example shows how to update the tags on a DNS zone.</span></span> <span data-ttu-id="a33da-163">Zadaná hodnota se nahradí stávající značky.</span><span class="sxs-lookup"><span data-stu-id="a33da-163">The existing tags are replaced by the value specified.</span></span>

```azurecli
az network dns zone update --resource-group myresourcegroup --name contoso.com --set tags.team=support
```

## <a name="delete-a-dns-zone"></a><span data-ttu-id="a33da-164">Odstranit zónu DNS</span><span class="sxs-lookup"><span data-stu-id="a33da-164">Delete a DNS zone</span></span>

<span data-ttu-id="a33da-165">Zóny DNS lze odstranit pomocí `az network dns zone delete`.</span><span class="sxs-lookup"><span data-stu-id="a33da-165">DNS zones can be deleted using `az network dns zone delete`.</span></span> <span data-ttu-id="a33da-166">Nápovědu získáte příkazem `az network dns zone delete --help`.</span><span class="sxs-lookup"><span data-stu-id="a33da-166">For help, see `az network dns zone delete --help`.</span></span>

> [!NOTE]
> <span data-ttu-id="a33da-167">Odstranění zóny DNS se také odstraní všechny záznamy DNS v rámci zóny.</span><span class="sxs-lookup"><span data-stu-id="a33da-167">Deleting a DNS zone also deletes all DNS records within the zone.</span></span> <span data-ttu-id="a33da-168">Tuto operaci nelze vrátit zpět.</span><span class="sxs-lookup"><span data-stu-id="a33da-168">This operation cannot be undone.</span></span> <span data-ttu-id="a33da-169">Pokud je zóna DNS se používá, službám pomocí zóny selže při odstranění zóny.</span><span class="sxs-lookup"><span data-stu-id="a33da-169">If the DNS zone is in use, services using the zone will fail when the zone is deleted.</span></span>
>
><span data-ttu-id="a33da-170">Chcete-li chránit proti náhodnému zóny odstranění, přečtěte si téma [jak chránit zóny DNS a záznamy](dns-protect-zones-recordsets.md).</span><span class="sxs-lookup"><span data-stu-id="a33da-170">To protect against accidental zone deletion, see [How to protect DNS zones and records](dns-protect-zones-recordsets.md).</span></span>

<span data-ttu-id="a33da-171">Tento příkaz zobrazí výzvu k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="a33da-171">This command prompts for confirmation.</span></span> <span data-ttu-id="a33da-172">Volitelné `--yes` přepínač potlačí tuto výzvu.</span><span class="sxs-lookup"><span data-stu-id="a33da-172">The optional `--yes` switch suppresses this prompt.</span></span>

<span data-ttu-id="a33da-173">Následující příklad ukazuje, jak odstranit zónu *contoso.com* ze skupiny prostředků *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="a33da-173">The following example shows how to delete the zone *contoso.com* from resource group *MyResourceGroup*.</span></span>

```azurecli
az network dns zone delete --resource-group myresourcegroup --name contoso.com
```

## <a name="next-steps"></a><span data-ttu-id="a33da-174">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a33da-174">Next steps</span></span>

<span data-ttu-id="a33da-175">Zjistěte, jak [spravovat sady záznamů a záznamy](dns-getstarted-create-recordset-cli.md) ve vaší zóně DNS.</span><span class="sxs-lookup"><span data-stu-id="a33da-175">Learn how to [manage record sets and records](dns-getstarted-create-recordset-cli.md) in your DNS zone.</span></span>

<span data-ttu-id="a33da-176">Zjistěte, jak [delegování domény do Azure DNS](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="a33da-176">Learn how to [delegate your domain to Azure DNS](dns-domain-delegation.md).</span></span>

