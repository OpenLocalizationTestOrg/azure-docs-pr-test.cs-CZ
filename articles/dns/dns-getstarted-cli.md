---
title: "Začínáme s DNS Azure pomocí Azure CLI 2.0 | Dokumentace Microsoftu"
description: "Naučíte se vytvořit zónu a záznam DNS v DNS Azure. Pomocí tohoto podrobného průvodce můžete vytvořit a spravovat první zónu a záznam DNS pomocí Azure CLI 2.0."
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
ms.openlocfilehash: 6958d61b29961f59cb22f62bec55f2d467e7e7cb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-dns-using-azure-cli-20"></a><span data-ttu-id="5efc7-104">Začínáme s DNS Azure pomocí Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="5efc7-104">Get started with Azure DNS using Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="5efc7-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5efc7-105">Azure portal</span></span>](dns-getstarted-portal.md)
> * [<span data-ttu-id="5efc7-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="5efc7-106">PowerShell</span></span>](dns-getstarted-powershell.md)
> * [<span data-ttu-id="5efc7-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="5efc7-107">Azure CLI 1.0</span></span>](dns-getstarted-cli-nodejs.md)
> * [<span data-ttu-id="5efc7-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="5efc7-108">Azure CLI 2.0</span></span>](dns-getstarted-cli.md)

<span data-ttu-id="5efc7-109">Tento článek vás provede kroky k vytvoření první zóny a záznamu DNS pomocí Azure CLI 2.0 pro různé platformy, které je dostupné pro Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="5efc7-109">This article walks you through the steps to create your first DNS zone and record using the cross-platform Azure CLI 2.0, which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="5efc7-110">Tyto kroky můžete provést také pomocí webu Azure Portal nebo Azure PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="5efc7-110">You can also perform these steps using the Azure portal or Azure PowerShell.</span></span>

<span data-ttu-id="5efc7-111">K hostování záznamů DNS v určité doméně se používá zóna DNS.</span><span class="sxs-lookup"><span data-stu-id="5efc7-111">A DNS zone is used to host the DNS records for a particular domain.</span></span> <span data-ttu-id="5efc7-112">Pokud chcete začít hostovat svou doménu v DNS Azure, musíte vytvořit zónu DNS pro daný název domény.</span><span class="sxs-lookup"><span data-stu-id="5efc7-112">To start hosting your domain in Azure DNS, you need to create a DNS zone for that domain name.</span></span> <span data-ttu-id="5efc7-113">Všechny záznamy DNS pro vaši doménu se pak vytvoří v této zóně DNS.</span><span class="sxs-lookup"><span data-stu-id="5efc7-113">Each DNS record for your domain is then created inside this DNS zone.</span></span> <span data-ttu-id="5efc7-114">Nakonec, pokud chcete zónu DNS publikovat na internetu, bude potřeba nakonfigurovat pro doménu názvové servery.</span><span class="sxs-lookup"><span data-stu-id="5efc7-114">Finally, to publish your DNS zone to the Internet, you need to configure the name servers for the domain.</span></span> <span data-ttu-id="5efc7-115">Jednotlivé kroky jsou popsány níže.</span><span class="sxs-lookup"><span data-stu-id="5efc7-115">Each of these steps is described below.</span></span>

<span data-ttu-id="5efc7-116">Tyto pokyny předpokládají, že již máte nainstalované Azure CLI 2.0 a jste k němu přihlášeni.</span><span class="sxs-lookup"><span data-stu-id="5efc7-116">These instructions assume you have already installed and signed in to Azure CLI 2.0.</span></span> <span data-ttu-id="5efc7-117">Nápovědu získáte v tématu [Správa zón DNS pomocí Azure CLI 2.0](dns-operations-dnszones-cli.md).</span><span class="sxs-lookup"><span data-stu-id="5efc7-117">For help, see [How to manage DNS zones using Azure CLI 2.0](dns-operations-dnszones-cli.md).</span></span>

## <a name="create-the-resource-group"></a><span data-ttu-id="5efc7-118">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="5efc7-118">Create the resource group</span></span>

<span data-ttu-id="5efc7-119">Před vytvořením zóny DNS se vytvoří skupina prostředků, která bude obsahovat zónu DNS.</span><span class="sxs-lookup"><span data-stu-id="5efc7-119">Before creating the DNS zone, a resource group is created to contain the DNS Zone.</span></span> <span data-ttu-id="5efc7-120">Následuje ukázka příkazu.</span><span class="sxs-lookup"><span data-stu-id="5efc7-120">The following shows the command.</span></span>

```azurecli
az group create --name MyResourceGroup --location "West US"
```

## <a name="create-a-dns-zone"></a><span data-ttu-id="5efc7-121">Vytvoření zóny DNS</span><span class="sxs-lookup"><span data-stu-id="5efc7-121">Create a DNS zone</span></span>

<span data-ttu-id="5efc7-122">Zóna DNS se vytvoří příkazem `az network dns zone create`.</span><span class="sxs-lookup"><span data-stu-id="5efc7-122">A DNS zone is created using the `az network dns zone create` command.</span></span> <span data-ttu-id="5efc7-123">Pokud chcete zobrazit nápovědu k tomuto příkazu, zadejte `az network dns zone create -h`.</span><span class="sxs-lookup"><span data-stu-id="5efc7-123">To see help for this command, type `az network dns zone create -h`.</span></span>

<span data-ttu-id="5efc7-124">Následující příklad vytvoří zónu DNS s názvem *contoso.com* ve skupině prostředků *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="5efc7-124">The following example creates a DNS zone called *contoso.com* in the resource group *MyResourceGroup*.</span></span> <span data-ttu-id="5efc7-125">Nahraďte hodnoty vlastními a použijte tento příklad k vytvoření zóny DNS.</span><span class="sxs-lookup"><span data-stu-id="5efc7-125">Use the example to create a DNS zone, substituting the values for your own.</span></span>

```azurecli
az network dns zone create -g MyResourceGroup -n contoso.com
```


## <a name="create-a-dns-record"></a><span data-ttu-id="5efc7-126">Vytvoření záznamu DNS</span><span class="sxs-lookup"><span data-stu-id="5efc7-126">Create a DNS record</span></span>

<span data-ttu-id="5efc7-127">K vytvoření záznamu DNS použijte příkaz `az network dns record-set [record type] add-record`.</span><span class="sxs-lookup"><span data-stu-id="5efc7-127">To create a DNS record, use the `az network dns record-set [record type] add-record` command.</span></span> <span data-ttu-id="5efc7-128">Například nápovědu k záznamům A získáte příkazem `azure network dns record-set A add-record -h`.</span><span class="sxs-lookup"><span data-stu-id="5efc7-128">For help, for A records for example, see `azure network dns record-set A add-record -h`.</span></span>

<span data-ttu-id="5efc7-129">Následující příklad vytvoří záznam s relativním názvem „www“ v zóně DNS „contoso.com“ ve skupině prostředků „MyResourceGroup“.</span><span class="sxs-lookup"><span data-stu-id="5efc7-129">The following example creates a record with the relative name "www" in the DNS Zone "contoso.com", in resource group "MyResourceGroup".</span></span> <span data-ttu-id="5efc7-130">Plně kvalifikovaný název sady záznamů je „www.contoso.com“.</span><span class="sxs-lookup"><span data-stu-id="5efc7-130">The fully-qualified name of the record set is "www.contoso.com".</span></span> <span data-ttu-id="5efc7-131">Typ záznamu je A a používá se IP adresa 1.2.3.4 a výchozí hodnota TTL 3 600 sekund (1 hodina).</span><span class="sxs-lookup"><span data-stu-id="5efc7-131">The record type is "A", with IP address "1.2.3.4", and a default TTL of 3600 seconds (1 hour) is used.</span></span>

```azurecli
az network dns record-set a add-record -g MyResourceGroup -z contoso.com -n www -a 1.2.3.4
```

<span data-ttu-id="5efc7-132">Informace o dalších typech záznamů, sadách záznamů s více než jedním záznamem, alternativních hodnotách TTL a úpravě existujících záznamů najdete v tématu [Správa záznamů a sad záznamů DNS pomocí Azure CLI 2.0](dns-operations-recordsets-cli.md).</span><span class="sxs-lookup"><span data-stu-id="5efc7-132">For other record types, for record sets with more than one record, for alternative TTL values, and to modify existing records, see [Manage DNS records and record sets using the Azure CLI 2.0](dns-operations-recordsets-cli.md).</span></span>


## <a name="view-records"></a><span data-ttu-id="5efc7-133">Zobrazení záznamů</span><span class="sxs-lookup"><span data-stu-id="5efc7-133">View records</span></span>

<span data-ttu-id="5efc7-134">K výpisu záznamů DNS ve vaší zóně použijte:</span><span class="sxs-lookup"><span data-stu-id="5efc7-134">To list the DNS records in your zone, use:</span></span>

```azurecli
az network dns record-set list -g MyResourceGroup -z contoso.com
```


## <a name="update-name-servers"></a><span data-ttu-id="5efc7-135">Aktualizace názvových serverů</span><span class="sxs-lookup"><span data-stu-id="5efc7-135">Update name servers</span></span>

<span data-ttu-id="5efc7-136">Jakmile budete spokojeni se správným nastavením zóny a záznamů DNS, bude potřeba nakonfigurovat váš název domény tak, aby používal názvové servery DNS Azure.</span><span class="sxs-lookup"><span data-stu-id="5efc7-136">Once you are satisfied that your DNS zone and records have been set up correctly, you need to configure your domain name to use the Azure DNS name servers.</span></span> <span data-ttu-id="5efc7-137">Tím umožníte ostatním uživatelům na internetu najít vaše záznamy DNS.</span><span class="sxs-lookup"><span data-stu-id="5efc7-137">This enables other users on the Internet to find your DNS records.</span></span>

<span data-ttu-id="5efc7-138">Názvové servery vaší zóny můžete zobrazit příkazem `az network dns zone show`.</span><span class="sxs-lookup"><span data-stu-id="5efc7-138">The name servers for your zone are given by the `az network dns zone show` command.</span></span> <span data-ttu-id="5efc7-139">Chcete-li zobrazit názvy názvových serverů, použijte výstup ve formátu JSON, jak je ukázáno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="5efc7-139">To see the name server names, use JSON output, as shown in the following example.</span></span>

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

<span data-ttu-id="5efc7-140">Tyto názvové servery by měly být nakonfigurované u registrátora názvu domény (u kterého jste zakoupili název domény).</span><span class="sxs-lookup"><span data-stu-id="5efc7-140">These name servers should be configured with the domain name registrar (where you purchased the domain name).</span></span> <span data-ttu-id="5efc7-141">Registrátor vám nabídne možnost nastavit názvové servery pro doménu.</span><span class="sxs-lookup"><span data-stu-id="5efc7-141">Your registrar will offer the option to set up the name servers for the domain.</span></span> <span data-ttu-id="5efc7-142">Další informace najdete v tématu [Delegování domény do DNS Azure](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="5efc7-142">For more information, see [Delegate your domain to Azure DNS](dns-domain-delegation.md).</span></span>

## <a name="delete-all-resources"></a><span data-ttu-id="5efc7-143">Odstranění všech prostředků</span><span class="sxs-lookup"><span data-stu-id="5efc7-143">Delete all resources</span></span>
 
<span data-ttu-id="5efc7-144">Pokud chcete odstranit všechny prostředky vytvořené v rámci tohoto článku, proveďte následující krok:</span><span class="sxs-lookup"><span data-stu-id="5efc7-144">To delete all resources created in this article, take the following step:</span></span>

```azurecli
az group delete --name MyResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="5efc7-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5efc7-145">Next steps</span></span>

<span data-ttu-id="5efc7-146">Další informace o DNS Azure najdete v tématu [Přehled DNS Azure](dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5efc7-146">To learn more about Azure DNS, see [Azure DNS overview](dns-overview.md).</span></span>

<span data-ttu-id="5efc7-147">Další informace o správě zón DNS v DNS Azure najdete v tématu [Správa zón DNS v DNS Azure pomocí Azure CLI 2.0](dns-operations-dnszones-cli.md).</span><span class="sxs-lookup"><span data-stu-id="5efc7-147">To learn more about managing DNS zones in Azure DNS, see [Manage DNS zones in Azure DNS using Azure CLI 2.0](dns-operations-dnszones-cli.md).</span></span>

<span data-ttu-id="5efc7-148">Další informace o správě záznamů DNS v DNS Azure najdete v tématu [Správa záznamů a sad záznamů DNS v DNS Azure pomocí Azure CLI 2.0](dns-operations-recordsets-cli.md).</span><span class="sxs-lookup"><span data-stu-id="5efc7-148">To learn more about managing DNS records in Azure DNS, see [Manage DNS records and record sets in Azure DNS using Azure CLI 2.0](dns-operations-recordsets-cli.md).</span></span>
