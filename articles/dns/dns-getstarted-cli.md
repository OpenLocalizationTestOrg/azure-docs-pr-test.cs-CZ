---
title: "aaaGet začít s Azure DNS pomocí Azure CLI 2.0 | Microsoft Docs"
description: "Zjistěte, jak toocreate DNS zóna a záznam v Azure DNS. Toto je podrobný průvodce toocreate a spravovat první zónu DNS a záznam pomocí hello 2.0 rozhraní příkazového řádku Azure."
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fb0aa0a6-d096-4d6a-b2f6-eda1c64f6182
ms.service: dns
ms.devlang: azurecli
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2017
ms.author: jonatul
ms.openlocfilehash: 8a894941e9910d5cc35394a1be9dbca9792613f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-dns-using-azure-cli-20"></a><span data-ttu-id="1b9ae-104">Začínáme s DNS Azure pomocí Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="1b9ae-104">Get started with Azure DNS using Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="1b9ae-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="1b9ae-105">Azure portal</span></span>](dns-getstarted-portal.md)
> * [<span data-ttu-id="1b9ae-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="1b9ae-106">PowerShell</span></span>](dns-getstarted-powershell.md)
> * [<span data-ttu-id="1b9ae-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="1b9ae-107">Azure CLI 1.0</span></span>](dns-getstarted-cli-nodejs.md)
> * [<span data-ttu-id="1b9ae-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="1b9ae-108">Azure CLI 2.0</span></span>](dns-getstarted-cli.md)

<span data-ttu-id="1b9ae-109">Tento článek vás provede kroky toocreate hello první zónu DNS a pomocí záznamu hello a platformy Azure CLI 2.0, která je dostupná pro Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="1b9ae-109">This article walks you through hello steps toocreate your first DNS zone and record using hello cross-platform Azure CLI 2.0, which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="1b9ae-110">Můžete také provést tyto kroky, pomocí hello portál Azure nebo Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1b9ae-110">You can also perform these steps using hello Azure portal or Azure PowerShell.</span></span>

<span data-ttu-id="1b9ae-111">Zóny DNS je použité toohost hello záznamy DNS pro konkrétní domény.</span><span class="sxs-lookup"><span data-stu-id="1b9ae-111">A DNS zone is used toohost hello DNS records for a particular domain.</span></span> <span data-ttu-id="1b9ae-112">toostart hostovat svoji doménu v Azure DNS, je nutné toocreate zóny DNS pro název domény.</span><span class="sxs-lookup"><span data-stu-id="1b9ae-112">toostart hosting your domain in Azure DNS, you need toocreate a DNS zone for that domain name.</span></span> <span data-ttu-id="1b9ae-113">Všechny záznamy DNS pro vaši doménu se pak vytvoří v této zóně DNS.</span><span class="sxs-lookup"><span data-stu-id="1b9ae-113">Each DNS record for your domain is then created inside this DNS zone.</span></span> <span data-ttu-id="1b9ae-114">Nakonec toopublish DNS zóny toohello Internet, musíte tooconfigure hello názvové servery pro doménu hello.</span><span class="sxs-lookup"><span data-stu-id="1b9ae-114">Finally, toopublish your DNS zone toohello Internet, you need tooconfigure hello name servers for hello domain.</span></span> <span data-ttu-id="1b9ae-115">Jednotlivé kroky jsou popsány níže.</span><span class="sxs-lookup"><span data-stu-id="1b9ae-115">Each of these steps is described below.</span></span>

<span data-ttu-id="1b9ae-116">Tyto pokyny předpokládají, je již nainstalován a přihlášení tooAzure 2.0 rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="1b9ae-116">These instructions assume you have already installed and signed in tooAzure CLI 2.0.</span></span> <span data-ttu-id="1b9ae-117">Nápovědu najdete v tématu [jak toomanage DNS zóny pomocí Azure CLI 2.0](dns-operations-dnszones-cli.md).</span><span class="sxs-lookup"><span data-stu-id="1b9ae-117">For help, see [How toomanage DNS zones using Azure CLI 2.0](dns-operations-dnszones-cli.md).</span></span>

## <a name="create-hello-resource-group"></a><span data-ttu-id="1b9ae-118">Vytvořte skupinu prostředků hello</span><span class="sxs-lookup"><span data-stu-id="1b9ae-118">Create hello resource group</span></span>

<span data-ttu-id="1b9ae-119">Před vytvořením zóny DNS hello, skupinu prostředků se vytvoří zónu DNS toocontain hello.</span><span class="sxs-lookup"><span data-stu-id="1b9ae-119">Before creating hello DNS zone, a resource group is created toocontain hello DNS Zone.</span></span> <span data-ttu-id="1b9ae-120">Následující Hello ukazuje příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="1b9ae-120">hello following shows hello command.</span></span>

```azurecli
az group create --name MyResourceGroup --location "West US"
```

## <a name="create-a-dns-zone"></a><span data-ttu-id="1b9ae-121">Vytvoření zóny DNS</span><span class="sxs-lookup"><span data-stu-id="1b9ae-121">Create a DNS zone</span></span>

<span data-ttu-id="1b9ae-122">Zóny DNS je vytvořený pomocí hello `az network dns zone create` příkaz.</span><span class="sxs-lookup"><span data-stu-id="1b9ae-122">A DNS zone is created using hello `az network dns zone create` command.</span></span> <span data-ttu-id="1b9ae-123">Zadejte toosee nápovědu pro tento příkaz `az network dns zone create -h`.</span><span class="sxs-lookup"><span data-stu-id="1b9ae-123">toosee help for this command, type `az network dns zone create -h`.</span></span>

<span data-ttu-id="1b9ae-124">Hello následující příklad vytvoří zónu DNS s názvem *contoso.com* ve skupině prostředků hello *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="1b9ae-124">hello following example creates a DNS zone called *contoso.com* in hello resource group *MyResourceGroup*.</span></span> <span data-ttu-id="1b9ae-125">Použijte příklad hello toocreate zóny DNS, nahraďte hello hodnoty vlastními.</span><span class="sxs-lookup"><span data-stu-id="1b9ae-125">Use hello example toocreate a DNS zone, substituting hello values for your own.</span></span>

```azurecli
az network dns zone create -g MyResourceGroup -n contoso.com
```


## <a name="create-a-dns-record"></a><span data-ttu-id="1b9ae-126">Vytvoření záznamu DNS</span><span class="sxs-lookup"><span data-stu-id="1b9ae-126">Create a DNS record</span></span>

<span data-ttu-id="1b9ae-127">toocreate záznam DNS použijte hello `az network dns record-set [record type] add-record` příkaz.</span><span class="sxs-lookup"><span data-stu-id="1b9ae-127">toocreate a DNS record, use hello `az network dns record-set [record type] add-record` command.</span></span> <span data-ttu-id="1b9ae-128">Například nápovědu k záznamům A získáte příkazem `azure network dns record-set A add-record -h`.</span><span class="sxs-lookup"><span data-stu-id="1b9ae-128">For help, for A records for example, see `azure network dns record-set A add-record -h`.</span></span>

<span data-ttu-id="1b9ae-129">Hello následující příklad vytvoří záznam s relativním názvem "www" v zóně DNS "contoso.com" ve skupině prostředků "MyResourceGroup" hello hello.</span><span class="sxs-lookup"><span data-stu-id="1b9ae-129">hello following example creates a record with hello relative name "www" in hello DNS Zone "contoso.com", in resource group "MyResourceGroup".</span></span> <span data-ttu-id="1b9ae-130">Hello plně kvalifikovaný název sady záznamů hello je "www.contoso.com".</span><span class="sxs-lookup"><span data-stu-id="1b9ae-130">hello fully-qualified name of hello record set is "www.contoso.com".</span></span> <span data-ttu-id="1b9ae-131">Hello typ záznamu je "A" s IP adresou "1.2.3.4" a použije se výchozí TTL 3 600 sekund (1 hodina).</span><span class="sxs-lookup"><span data-stu-id="1b9ae-131">hello record type is "A", with IP address "1.2.3.4", and a default TTL of 3600 seconds (1 hour) is used.</span></span>

```azurecli
az network dns record-set a add-record -g MyResourceGroup -z contoso.com -n www -a 1.2.3.4
```

<span data-ttu-id="1b9ae-132">U jiných typů záznamů sady záznamů s více než jeden záznam pro alternativní hodnoty TTL a toomodify existující záznamy, najdete v části [záznamy DNS spravovat a sad záznamů pomocí Azure CLI 2.0 hello](dns-operations-recordsets-cli.md).</span><span class="sxs-lookup"><span data-stu-id="1b9ae-132">For other record types, for record sets with more than one record, for alternative TTL values, and toomodify existing records, see [Manage DNS records and record sets using hello Azure CLI 2.0](dns-operations-recordsets-cli.md).</span></span>


## <a name="view-records"></a><span data-ttu-id="1b9ae-133">Zobrazení záznamů</span><span class="sxs-lookup"><span data-stu-id="1b9ae-133">View records</span></span>

<span data-ttu-id="1b9ae-134">toolist hello záznamy DNS v zóně, použijte:</span><span class="sxs-lookup"><span data-stu-id="1b9ae-134">toolist hello DNS records in your zone, use:</span></span>

```azurecli
az network dns record-set list -g MyResourceGroup -z contoso.com
```


## <a name="update-name-servers"></a><span data-ttu-id="1b9ae-135">Aktualizace názvových serverů</span><span class="sxs-lookup"><span data-stu-id="1b9ae-135">Update name servers</span></span>

<span data-ttu-id="1b9ae-136">Jakmile jste uspokojit, že zóna DNS a záznamy byly nastaveny správně, musíte tooconfigure váš název domény toouse názvových serverů Azure DNS hello.</span><span class="sxs-lookup"><span data-stu-id="1b9ae-136">Once you are satisfied that your DNS zone and records have been set up correctly, you need tooconfigure your domain name toouse hello Azure DNS name servers.</span></span> <span data-ttu-id="1b9ae-137">To umožňuje jiných uživatelů v Internetu toofind hello svoje záznamy DNS.</span><span class="sxs-lookup"><span data-stu-id="1b9ae-137">This enables other users on hello Internet toofind your DNS records.</span></span>

<span data-ttu-id="1b9ae-138">Hello názvové servery zóny jsou poskytují hello `az network dns zone show` příkaz.</span><span class="sxs-lookup"><span data-stu-id="1b9ae-138">hello name servers for your zone are given by hello `az network dns zone show` command.</span></span> <span data-ttu-id="1b9ae-139">názvy toosee hello názvových serverů, použijte výstup, ve formátu JSON, jak ukazuje následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="1b9ae-139">toosee hello name server names, use JSON output, as shown in hello following example.</span></span>

```azurecli
az network dns zone show -g MyResourceGroup -n contoso.com -o json

{
  "etag": "00000003-0000-0000-b40d-0996b97ed101",
  "id": "/subscriptions/a385a691-bd93-41b0-8084-8213ebc5bff7/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "contoso.com",
  "nameServers": [
    "ns1-01.azure-dns.com.",
    "ns2-01.azure-dns.net.",
    "ns3-01.azure-dns.org.",
    "ns4-01.azure-dns.info."
  ],
  "numberOfRecordSets": 3,
  "resourceGroup": "myresourcegroup",
  "tags": {},
  "type": "Microsoft.Network/dnszones"
}
```

<span data-ttu-id="1b9ae-140">Tyto názvové servery musí být nakonfigurovaný u registrátora názvu domény hello (kde jste zakoupili hello název domény).</span><span class="sxs-lookup"><span data-stu-id="1b9ae-140">These name servers should be configured with hello domain name registrar (where you purchased hello domain name).</span></span> <span data-ttu-id="1b9ae-141">Vám Registrátor nabídne možnost tooset hello až hello názvové servery pro doménu hello.</span><span class="sxs-lookup"><span data-stu-id="1b9ae-141">Your registrar will offer hello option tooset up hello name servers for hello domain.</span></span> <span data-ttu-id="1b9ae-142">Další informace najdete v tématu [delegovat tooAzure vaší domény DNS](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="1b9ae-142">For more information, see [Delegate your domain tooAzure DNS](dns-domain-delegation.md).</span></span>

## <a name="delete-all-resources"></a><span data-ttu-id="1b9ae-143">Odstranění všech prostředků</span><span class="sxs-lookup"><span data-stu-id="1b9ae-143">Delete all resources</span></span>
 
<span data-ttu-id="1b9ae-144">toodelete vytvořit všechny prostředky v tomto článku, hello proveďte následující krok:</span><span class="sxs-lookup"><span data-stu-id="1b9ae-144">toodelete all resources created in this article, take hello following step:</span></span>

```azurecli
az group delete --name MyResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="1b9ae-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1b9ae-145">Next steps</span></span>

<span data-ttu-id="1b9ae-146">toolearn Další informace o Azure DNS, najdete v části [přehled Azure DNS](dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1b9ae-146">toolearn more about Azure DNS, see [Azure DNS overview](dns-overview.md).</span></span>

<span data-ttu-id="1b9ae-147">toolearn Další informace o správě zóny DNS v Azure DNS, najdete v části [zóny DNS spravovat v Azure DNS pomocí Azure CLI 2.0](dns-operations-dnszones-cli.md).</span><span class="sxs-lookup"><span data-stu-id="1b9ae-147">toolearn more about managing DNS zones in Azure DNS, see [Manage DNS zones in Azure DNS using Azure CLI 2.0](dns-operations-dnszones-cli.md).</span></span>

<span data-ttu-id="1b9ae-148">toolearn Další informace o správě záznamů DNS v Azure DNS, najdete v části [sad záznamů a záznamů DNS spravovat v Azure DNS pomocí Azure CLI 2.0](dns-operations-recordsets-cli.md).</span><span class="sxs-lookup"><span data-stu-id="1b9ae-148">toolearn more about managing DNS records in Azure DNS, see [Manage DNS records and record sets in Azure DNS using Azure CLI 2.0](dns-operations-recordsets-cli.md).</span></span>
