---
title: "Začínáme s DNS Azure pomocí Azure CLI 1.0 | Dokumentace Microsoftu"
description: "Naučíte se vytvořit zónu a záznam DNS v DNS Azure. Pomocí tohoto podrobného průvodce můžete vytvořit a spravovat první zónu a záznam DNS pomocí Azure CLI 1.0."
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fb0aa0a6-d096-4d6a-b2f6-eda1c64f6182
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2017
ms.author: jonatul
ms.openlocfilehash: f7943b71bbd16c36df09436973d92539eb62b210
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-dns-using-azure-cli-10"></a><span data-ttu-id="2c372-104">Začínáme s DNS Azure pomocí Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="2c372-104">Get started with Azure DNS using Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="2c372-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2c372-105">Azure portal</span></span>](dns-getstarted-portal.md)
> * [<span data-ttu-id="2c372-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="2c372-106">PowerShell</span></span>](dns-getstarted-powershell.md)
> * [<span data-ttu-id="2c372-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="2c372-107">Azure CLI 1.0</span></span>](dns-getstarted-cli-nodejs.md)
> * [<span data-ttu-id="2c372-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="2c372-108">Azure CLI 2.0</span></span>](dns-getstarted-cli.md)

<span data-ttu-id="2c372-109">Tento článek vás provede kroky k vytvoření první zóny a záznamu DNS pomocí Azure CLI 1.0 pro různé platformy, které je dostupné pro Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="2c372-109">This article walks you through the steps to create your first DNS zone and record using the cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="2c372-110">Tyto kroky můžete provést také pomocí webu Azure Portal nebo Azure PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="2c372-110">You can also perform these steps using the Azure portal or Azure PowerShell.</span></span>

<span data-ttu-id="2c372-111">K hostování záznamů DNS v určité doméně se používá zóna DNS.</span><span class="sxs-lookup"><span data-stu-id="2c372-111">A DNS zone is used to host the DNS records for a particular domain.</span></span> <span data-ttu-id="2c372-112">Pokud chcete začít hostovat svou doménu v DNS Azure, musíte vytvořit zónu DNS pro daný název domény.</span><span class="sxs-lookup"><span data-stu-id="2c372-112">To start hosting your domain in Azure DNS, you need to create a DNS zone for that domain name.</span></span> <span data-ttu-id="2c372-113">Všechny záznamy DNS pro vaši doménu se pak vytvoří v této zóně DNS.</span><span class="sxs-lookup"><span data-stu-id="2c372-113">Each DNS record for your domain is then created inside this DNS zone.</span></span> <span data-ttu-id="2c372-114">Nakonec, pokud chcete zónu DNS publikovat na internetu, bude potřeba nakonfigurovat pro doménu názvové servery.</span><span class="sxs-lookup"><span data-stu-id="2c372-114">Finally, to publish your DNS zone to the Internet, you need to configure the name servers for the domain.</span></span> <span data-ttu-id="2c372-115">Jednotlivé kroky jsou popsány níže.</span><span class="sxs-lookup"><span data-stu-id="2c372-115">Each of these steps is described below.</span></span>

<span data-ttu-id="2c372-116">Tyto pokyny předpokládají, že již máte nainstalované Azure CLI 1.0 a jste k němu přihlášeni.</span><span class="sxs-lookup"><span data-stu-id="2c372-116">These instructions assume you have already installed and signed in to Azure CLI 1.0.</span></span> <span data-ttu-id="2c372-117">Nápovědu získáte v tématu [Správa zón DNS pomocí Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="2c372-117">For help, see [How to manage DNS zones using Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md).</span></span>

## <a name="create-the-resource-group"></a><span data-ttu-id="2c372-118">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="2c372-118">Create the resource group</span></span>

<span data-ttu-id="2c372-119">Před vytvořením zóny DNS se vytvoří skupina prostředků, která bude obsahovat zónu DNS.</span><span class="sxs-lookup"><span data-stu-id="2c372-119">Before creating the DNS zone, a resource group is created to contain the DNS Zone.</span></span> <span data-ttu-id="2c372-120">Následuje ukázka příkazu.</span><span class="sxs-lookup"><span data-stu-id="2c372-120">The following shows the command.</span></span>

```azurecli
azure group create --name MyResourceGroup --location "West US"
```

## <a name="create-a-dns-zone"></a><span data-ttu-id="2c372-121">Vytvoření zóny DNS</span><span class="sxs-lookup"><span data-stu-id="2c372-121">Create a DNS zone</span></span>

<span data-ttu-id="2c372-122">Zóna DNS se vytvoří příkazem `azure network dns zone create`.</span><span class="sxs-lookup"><span data-stu-id="2c372-122">A DNS zone is created using the `azure network dns zone create` command.</span></span> <span data-ttu-id="2c372-123">Pokud chcete zobrazit nápovědu k tomuto příkazu, zadejte `azure network dns zone create -h`.</span><span class="sxs-lookup"><span data-stu-id="2c372-123">To see help for this command, type `azure network dns zone create -h`.</span></span>

<span data-ttu-id="2c372-124">Následující příklad vytvoří zónu DNS s názvem *contoso.com* ve skupině prostředků s názvem *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="2c372-124">The following example creates a DNS zone called *contoso.com* in the resource group called *MyResourceGroup*.</span></span> <span data-ttu-id="2c372-125">Nahraďte hodnoty vlastními a použijte tento příklad k vytvoření zóny DNS.</span><span class="sxs-lookup"><span data-stu-id="2c372-125">Use the example to create a DNS zone, substituting the values for your own.</span></span>

```azurecli
azure network dns zone create MyResourceGroup contoso.com
```


## <a name="create-a-dns-record"></a><span data-ttu-id="2c372-126">Vytvoření záznamu DNS</span><span class="sxs-lookup"><span data-stu-id="2c372-126">Create a DNS record</span></span>

<span data-ttu-id="2c372-127">K vytvoření záznamu DNS použijte příkaz `azure network dns record-set add-record`.</span><span class="sxs-lookup"><span data-stu-id="2c372-127">To create a DNS record, use the `azure network dns record-set add-record` command.</span></span> <span data-ttu-id="2c372-128">Nápovědu získáte příkazem `azure network dns record-set add-record -h`.</span><span class="sxs-lookup"><span data-stu-id="2c372-128">For help, see `azure network dns record-set add-record -h`.</span></span>

<span data-ttu-id="2c372-129">Následující příklad vytvoří záznam s relativním názvem „www“ v zóně DNS „contoso.com“ ve skupině prostředků „MyResourceGroup“.</span><span class="sxs-lookup"><span data-stu-id="2c372-129">The following example creates a record with the relative name "www" in the DNS Zone "contoso.com", in resource group "MyResourceGroup".</span></span> <span data-ttu-id="2c372-130">Plně kvalifikovaný název sady záznamů je „www.contoso.com“.</span><span class="sxs-lookup"><span data-stu-id="2c372-130">The fully-qualified name of the record set is "www.contoso.com".</span></span> <span data-ttu-id="2c372-131">Typ záznamu je A a používá se IP adresa 1.2.3.4 a výchozí hodnota TTL 3 600 sekund (1 hodina).</span><span class="sxs-lookup"><span data-stu-id="2c372-131">The record type is "A", with IP address "1.2.3.4", and a default TTL of 3600 seconds (1 hour) is used.</span></span>

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www A -a 1.2.3.4
```

<span data-ttu-id="2c372-132">Informace o dalších typech záznamů, sadách záznamů s více než jedním záznamem, alternativních hodnotách TTL a úpravě existujících záznamů najdete v tématu [Správa záznamů a sad záznamů DNS pomocí Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="2c372-132">For other record types, for record sets with more than one record, for alternative TTL values, and to modify existing records, see [Manage DNS records and record sets using the Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md).</span></span>


## <a name="view-records"></a><span data-ttu-id="2c372-133">Zobrazení záznamů</span><span class="sxs-lookup"><span data-stu-id="2c372-133">View records</span></span>

<span data-ttu-id="2c372-134">K výpisu záznamů DNS ve vaší zóně použijte:</span><span class="sxs-lookup"><span data-stu-id="2c372-134">To list the DNS records in your zone, use:</span></span>

```azurecli
azure network dns record-set list MyResourceGroup contoso.com
```


## <a name="update-name-servers"></a><span data-ttu-id="2c372-135">Aktualizace názvových serverů</span><span class="sxs-lookup"><span data-stu-id="2c372-135">Update name servers</span></span>

<span data-ttu-id="2c372-136">Jakmile budete spokojeni se správným nastavením zóny a záznamů DNS, bude potřeba nakonfigurovat váš název domény tak, aby používal názvové servery DNS Azure.</span><span class="sxs-lookup"><span data-stu-id="2c372-136">Once you are satisfied that your DNS zone and records have been set up correctly, you need to configure your domain name to use the Azure DNS name servers.</span></span> <span data-ttu-id="2c372-137">Tím umožníte ostatním uživatelům na internetu najít vaše záznamy DNS.</span><span class="sxs-lookup"><span data-stu-id="2c372-137">This enables other users on the Internet to find your DNS records.</span></span>

<span data-ttu-id="2c372-138">Názvové servery vaší zóny můžete zobrazit příkazem `azure network dns zone show`:</span><span class="sxs-lookup"><span data-stu-id="2c372-138">The name servers for your zone are given by the `azure network dns zone show` command:</span></span>

```azurecli
azure network dns zone show MyResourceGroup contoso.com

info:    Executing command network dns zone show
+ Looking up the dns zone "contoso.com"
data:    Id                              : /subscriptions/a385a691-bd93-41b0-8084-8213ebc5bff7/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com
data:    Name                            : contoso.com
data:    Type                            : Microsoft.Network/dnszones
data:    Location                        : global
data:    Number of record sets           : 3
data:    Max number of record sets       : 5000
data:    Name servers:
data:        ns1-01.azure-dns.com.
data:        ns2-01.azure-dns.net.
data:        ns3-01.azure-dns.org.
data:        ns4-01.azure-dns.info.
data:    Tags                            :
info:    network dns zone show command OK
```

<span data-ttu-id="2c372-139">Tyto názvové servery by měly být nakonfigurované u registrátora názvu domény (u kterého jste zakoupili název domény).</span><span class="sxs-lookup"><span data-stu-id="2c372-139">These name servers should be configured with the domain name registrar (where you purchased the domain name).</span></span> <span data-ttu-id="2c372-140">Registrátor vám nabídne možnost nastavit názvové servery pro doménu.</span><span class="sxs-lookup"><span data-stu-id="2c372-140">Your registrar will offer the option to set up the name servers for the domain.</span></span> <span data-ttu-id="2c372-141">Další informace najdete v tématu [Delegování domény do DNS Azure](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="2c372-141">For more information, see [Delegate your domain to Azure DNS](dns-domain-delegation.md).</span></span>

## <a name="delete-all-resources"></a><span data-ttu-id="2c372-142">Odstranění všech prostředků</span><span class="sxs-lookup"><span data-stu-id="2c372-142">Delete all resources</span></span>
 
<span data-ttu-id="2c372-143">Pokud chcete odstranit všechny prostředky vytvořené v rámci tohoto článku, proveďte následující krok:</span><span class="sxs-lookup"><span data-stu-id="2c372-143">To delete all resources created in this article, take the following step:</span></span>

```azurecli
azure group delete --name MyResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="2c372-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2c372-144">Next steps</span></span>

<span data-ttu-id="2c372-145">Další informace o DNS Azure najdete v tématu [Přehled DNS Azure](dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2c372-145">To learn more about Azure DNS, see [Azure DNS overview](dns-overview.md).</span></span>

<span data-ttu-id="2c372-146">Další informace o správě zón DNS v DNS Azure najdete v tématu [Správa zón DNS v DNS Azure pomocí Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="2c372-146">To learn more about managing DNS zones in Azure DNS, see [Manage DNS zones in Azure DNS using Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md).</span></span>

<span data-ttu-id="2c372-147">Další informace o správě záznamů DNS v DNS Azure najdete v tématu [Správa záznamů a sad záznamů DNS v DNS Azure pomocí Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="2c372-147">To learn more about managing DNS records in Azure DNS, see [Manage DNS records and record sets in Azure DNS using Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md).</span></span>

