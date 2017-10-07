---
title: "aaaGet začít s Azure DNS pomocí Azure CLI 1.0 | Microsoft Docs"
description: "Zjistěte, jak toocreate DNS zóna a záznam v Azure DNS. Toto je podrobný průvodce toocreate a spravovat první zónu DNS a záznam pomocí hello Azure CLI 1.0."
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
ms.openlocfilehash: e200c848ad261160e593306dbb8a1d92bf26880b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-dns-using-azure-cli-10"></a><span data-ttu-id="85737-104">Začínáme s DNS Azure pomocí Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="85737-104">Get started with Azure DNS using Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="85737-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="85737-105">Azure portal</span></span>](dns-getstarted-portal.md)
> * [<span data-ttu-id="85737-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="85737-106">PowerShell</span></span>](dns-getstarted-powershell.md)
> * [<span data-ttu-id="85737-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="85737-107">Azure CLI 1.0</span></span>](dns-getstarted-cli-nodejs.md)
> * [<span data-ttu-id="85737-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="85737-108">Azure CLI 2.0</span></span>](dns-getstarted-cli.md)

<span data-ttu-id="85737-109">Tento článek vás provede kroky toocreate hello první zónu DNS a pomocí záznamu hello 1.0 rozhraní příkazového řádku Azure a platformy, která je dostupná pro Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="85737-109">This article walks you through hello steps toocreate your first DNS zone and record using hello cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="85737-110">Můžete také provést tyto kroky, pomocí hello portál Azure nebo Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="85737-110">You can also perform these steps using hello Azure portal or Azure PowerShell.</span></span>

<span data-ttu-id="85737-111">Zóny DNS je použité toohost hello záznamy DNS pro konkrétní domény.</span><span class="sxs-lookup"><span data-stu-id="85737-111">A DNS zone is used toohost hello DNS records for a particular domain.</span></span> <span data-ttu-id="85737-112">toostart hostovat svoji doménu v Azure DNS, je nutné toocreate zóny DNS pro název domény.</span><span class="sxs-lookup"><span data-stu-id="85737-112">toostart hosting your domain in Azure DNS, you need toocreate a DNS zone for that domain name.</span></span> <span data-ttu-id="85737-113">Všechny záznamy DNS pro vaši doménu se pak vytvoří v této zóně DNS.</span><span class="sxs-lookup"><span data-stu-id="85737-113">Each DNS record for your domain is then created inside this DNS zone.</span></span> <span data-ttu-id="85737-114">Nakonec toopublish DNS zóny toohello Internet, musíte tooconfigure hello názvové servery pro doménu hello.</span><span class="sxs-lookup"><span data-stu-id="85737-114">Finally, toopublish your DNS zone toohello Internet, you need tooconfigure hello name servers for hello domain.</span></span> <span data-ttu-id="85737-115">Jednotlivé kroky jsou popsány níže.</span><span class="sxs-lookup"><span data-stu-id="85737-115">Each of these steps is described below.</span></span>

<span data-ttu-id="85737-116">Tyto pokyny předpokládají, je již nainstalován a přihlášení tooAzure 1.0 rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="85737-116">These instructions assume you have already installed and signed in tooAzure CLI 1.0.</span></span> <span data-ttu-id="85737-117">Nápovědu najdete v tématu [jak toomanage DNS zóny pomocí Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="85737-117">For help, see [How toomanage DNS zones using Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md).</span></span>

## <a name="create-hello-resource-group"></a><span data-ttu-id="85737-118">Vytvořte skupinu prostředků hello</span><span class="sxs-lookup"><span data-stu-id="85737-118">Create hello resource group</span></span>

<span data-ttu-id="85737-119">Před vytvořením zóny DNS hello, skupinu prostředků se vytvoří zónu DNS toocontain hello.</span><span class="sxs-lookup"><span data-stu-id="85737-119">Before creating hello DNS zone, a resource group is created toocontain hello DNS Zone.</span></span> <span data-ttu-id="85737-120">Následující Hello ukazuje příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="85737-120">hello following shows hello command.</span></span>

```azurecli
azure group create --name MyResourceGroup --location "West US"
```

## <a name="create-a-dns-zone"></a><span data-ttu-id="85737-121">Vytvoření zóny DNS</span><span class="sxs-lookup"><span data-stu-id="85737-121">Create a DNS zone</span></span>

<span data-ttu-id="85737-122">Zóny DNS je vytvořený pomocí hello `azure network dns zone create` příkaz.</span><span class="sxs-lookup"><span data-stu-id="85737-122">A DNS zone is created using hello `azure network dns zone create` command.</span></span> <span data-ttu-id="85737-123">Zadejte toosee nápovědu pro tento příkaz `azure network dns zone create -h`.</span><span class="sxs-lookup"><span data-stu-id="85737-123">toosee help for this command, type `azure network dns zone create -h`.</span></span>

<span data-ttu-id="85737-124">Hello následující příklad vytvoří zónu DNS s názvem *contoso.com* v hello skupinu prostředků s názvem *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="85737-124">hello following example creates a DNS zone called *contoso.com* in hello resource group called *MyResourceGroup*.</span></span> <span data-ttu-id="85737-125">Použijte příklad hello toocreate zóny DNS, nahraďte hello hodnoty vlastními.</span><span class="sxs-lookup"><span data-stu-id="85737-125">Use hello example toocreate a DNS zone, substituting hello values for your own.</span></span>

```azurecli
azure network dns zone create MyResourceGroup contoso.com
```


## <a name="create-a-dns-record"></a><span data-ttu-id="85737-126">Vytvoření záznamu DNS</span><span class="sxs-lookup"><span data-stu-id="85737-126">Create a DNS record</span></span>

<span data-ttu-id="85737-127">toocreate záznam DNS použijte hello `azure network dns record-set add-record` příkaz.</span><span class="sxs-lookup"><span data-stu-id="85737-127">toocreate a DNS record, use hello `azure network dns record-set add-record` command.</span></span> <span data-ttu-id="85737-128">Nápovědu získáte příkazem `azure network dns record-set add-record -h`.</span><span class="sxs-lookup"><span data-stu-id="85737-128">For help, see `azure network dns record-set add-record -h`.</span></span>

<span data-ttu-id="85737-129">Hello následující příklad vytvoří záznam s relativním názvem "www" v zóně DNS "contoso.com" ve skupině prostředků "MyResourceGroup" hello hello.</span><span class="sxs-lookup"><span data-stu-id="85737-129">hello following example creates a record with hello relative name "www" in hello DNS Zone "contoso.com", in resource group "MyResourceGroup".</span></span> <span data-ttu-id="85737-130">Hello plně kvalifikovaný název sady záznamů hello je "www.contoso.com".</span><span class="sxs-lookup"><span data-stu-id="85737-130">hello fully-qualified name of hello record set is "www.contoso.com".</span></span> <span data-ttu-id="85737-131">Hello typ záznamu je "A" s IP adresou "1.2.3.4" a použije se výchozí TTL 3 600 sekund (1 hodina).</span><span class="sxs-lookup"><span data-stu-id="85737-131">hello record type is "A", with IP address "1.2.3.4", and a default TTL of 3600 seconds (1 hour) is used.</span></span>

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www A -a 1.2.3.4
```

<span data-ttu-id="85737-132">U jiných typů záznamů sady záznamů s více než jeden záznam pro alternativní hodnoty TTL a toomodify existující záznamy, najdete v části [záznamy DNS spravovat a sad záznamů pomocí Azure CLI 1.0 hello](dns-operations-recordsets-cli-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="85737-132">For other record types, for record sets with more than one record, for alternative TTL values, and toomodify existing records, see [Manage DNS records and record sets using hello Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md).</span></span>


## <a name="view-records"></a><span data-ttu-id="85737-133">Zobrazení záznamů</span><span class="sxs-lookup"><span data-stu-id="85737-133">View records</span></span>

<span data-ttu-id="85737-134">toolist hello záznamy DNS v zóně, použijte:</span><span class="sxs-lookup"><span data-stu-id="85737-134">toolist hello DNS records in your zone, use:</span></span>

```azurecli
azure network dns record-set list MyResourceGroup contoso.com
```


## <a name="update-name-servers"></a><span data-ttu-id="85737-135">Aktualizace názvových serverů</span><span class="sxs-lookup"><span data-stu-id="85737-135">Update name servers</span></span>

<span data-ttu-id="85737-136">Jakmile jste uspokojit, že zóna DNS a záznamy byly nastaveny správně, musíte tooconfigure váš název domény toouse názvových serverů Azure DNS hello.</span><span class="sxs-lookup"><span data-stu-id="85737-136">Once you are satisfied that your DNS zone and records have been set up correctly, you need tooconfigure your domain name toouse hello Azure DNS name servers.</span></span> <span data-ttu-id="85737-137">To umožňuje jiných uživatelů v Internetu toofind hello svoje záznamy DNS.</span><span class="sxs-lookup"><span data-stu-id="85737-137">This enables other users on hello Internet toofind your DNS records.</span></span>

<span data-ttu-id="85737-138">Hello názvové servery zóny jsou poskytují hello `azure network dns zone show` příkaz:</span><span class="sxs-lookup"><span data-stu-id="85737-138">hello name servers for your zone are given by hello `azure network dns zone show` command:</span></span>

```azurecli
azure network dns zone show MyResourceGroup contoso.com

info:    Executing command network dns zone show
+ Looking up hello dns zone "contoso.com"
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

<span data-ttu-id="85737-139">Tyto názvové servery musí být nakonfigurovaný u registrátora názvu domény hello (kde jste zakoupili hello název domény).</span><span class="sxs-lookup"><span data-stu-id="85737-139">These name servers should be configured with hello domain name registrar (where you purchased hello domain name).</span></span> <span data-ttu-id="85737-140">Vám Registrátor nabídne možnost tooset hello až hello názvové servery pro doménu hello.</span><span class="sxs-lookup"><span data-stu-id="85737-140">Your registrar will offer hello option tooset up hello name servers for hello domain.</span></span> <span data-ttu-id="85737-141">Další informace najdete v tématu [delegovat tooAzure vaší domény DNS](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="85737-141">For more information, see [Delegate your domain tooAzure DNS](dns-domain-delegation.md).</span></span>

## <a name="delete-all-resources"></a><span data-ttu-id="85737-142">Odstranění všech prostředků</span><span class="sxs-lookup"><span data-stu-id="85737-142">Delete all resources</span></span>
 
<span data-ttu-id="85737-143">toodelete vytvořit všechny prostředky v tomto článku, hello proveďte následující krok:</span><span class="sxs-lookup"><span data-stu-id="85737-143">toodelete all resources created in this article, take hello following step:</span></span>

```azurecli
azure group delete --name MyResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="85737-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="85737-144">Next steps</span></span>

<span data-ttu-id="85737-145">toolearn Další informace o Azure DNS, najdete v části [přehled Azure DNS](dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="85737-145">toolearn more about Azure DNS, see [Azure DNS overview](dns-overview.md).</span></span>

<span data-ttu-id="85737-146">toolearn Další informace o správě zóny DNS v Azure DNS, najdete v části [zóny DNS spravovat v Azure DNS pomocí Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="85737-146">toolearn more about managing DNS zones in Azure DNS, see [Manage DNS zones in Azure DNS using Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md).</span></span>

<span data-ttu-id="85737-147">toolearn Další informace o správě záznamů DNS v Azure DNS, najdete v části [sad záznamů a záznamů DNS spravovat v Azure DNS pomocí Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="85737-147">toolearn more about managing DNS records in Azure DNS, see [Manage DNS records and record sets in Azure DNS using Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md).</span></span>

