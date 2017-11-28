---
title: "aaaGet začít s Azure DNS pomocí prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak toocreate DNS zóna a záznam v Azure DNS. Toto je podrobný průvodce toocreate a spravovat vaše první zónu DNS a zaznamenejte pomocí prostředí PowerShell."
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
ms.openlocfilehash: 0f9dead1e4b44fcc74c84a024c41cdfaeb02b5d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-dns-using-powershell"></a><span data-ttu-id="70bfb-104">Začínáme s DNS Azure pomocí PowerShellu</span><span class="sxs-lookup"><span data-stu-id="70bfb-104">Get Started with Azure DNS using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="70bfb-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="70bfb-105">Azure portal</span></span>](dns-getstarted-portal.md)
> * [<span data-ttu-id="70bfb-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="70bfb-106">PowerShell</span></span>](dns-getstarted-powershell.md)
> * [<span data-ttu-id="70bfb-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="70bfb-107">Azure CLI 1.0</span></span>](dns-getstarted-cli-nodejs.md)
> * [<span data-ttu-id="70bfb-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="70bfb-108">Azure CLI 2.0</span></span>](dns-getstarted-cli.md)

<span data-ttu-id="70bfb-109">Tento článek vás provede kroky toocreate hello první zónu DNS a záznam pomocí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="70bfb-109">This article walks you through hello steps toocreate your first DNS zone and record using Azure PowerShell.</span></span> <span data-ttu-id="70bfb-110">Můžete také provést tyto kroky, pomocí hello portál Azure nebo hello rozhraní příkazového řádku Azure napříč platformami.</span><span class="sxs-lookup"><span data-stu-id="70bfb-110">You can also perform these steps using hello Azure portal or hello cross-platform Azure CLI.</span></span>

<span data-ttu-id="70bfb-111">Zóny DNS je použité toohost hello záznamy DNS pro konkrétní domény.</span><span class="sxs-lookup"><span data-stu-id="70bfb-111">A DNS zone is used toohost hello DNS records for a particular domain.</span></span> <span data-ttu-id="70bfb-112">toostart hostovat svoji doménu v Azure DNS, je nutné toocreate zóny DNS pro název domény.</span><span class="sxs-lookup"><span data-stu-id="70bfb-112">toostart hosting your domain in Azure DNS, you need toocreate a DNS zone for that domain name.</span></span> <span data-ttu-id="70bfb-113">Všechny záznamy DNS pro vaši doménu se pak vytvoří v této zóně DNS.</span><span class="sxs-lookup"><span data-stu-id="70bfb-113">Each DNS record for your domain is then created inside this DNS zone.</span></span> <span data-ttu-id="70bfb-114">Nakonec toopublish DNS zóny toohello Internet, musíte tooconfigure hello názvové servery pro doménu hello.</span><span class="sxs-lookup"><span data-stu-id="70bfb-114">Finally, toopublish your DNS zone toohello Internet, you need tooconfigure hello name servers for hello domain.</span></span> <span data-ttu-id="70bfb-115">Jednotlivé kroky jsou popsány níže.</span><span class="sxs-lookup"><span data-stu-id="70bfb-115">Each of these steps is described below.</span></span>

<span data-ttu-id="70bfb-116">Tyto pokyny předpokládají, je již nainstalován a přihlášení tooAzure prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="70bfb-116">These instructions assume you have already installed and signed in tooAzure PowerShell.</span></span> <span data-ttu-id="70bfb-117">Nápovědu najdete v tématu [jak toomanage DNS zóny pomocí prostředí PowerShell](dns-operations-dnszones.md).</span><span class="sxs-lookup"><span data-stu-id="70bfb-117">For help, see [How toomanage DNS zones using PowerShell](dns-operations-dnszones.md).</span></span>

## <a name="create-hello-resource-group"></a><span data-ttu-id="70bfb-118">Vytvořte skupinu prostředků hello</span><span class="sxs-lookup"><span data-stu-id="70bfb-118">Create hello resource group</span></span>

<span data-ttu-id="70bfb-119">Před vytvořením zóny DNS hello, skupinu prostředků se vytvoří zónu DNS toocontain hello.</span><span class="sxs-lookup"><span data-stu-id="70bfb-119">Before creating hello DNS zone, a resource group is created toocontain hello DNS Zone.</span></span> <span data-ttu-id="70bfb-120">Následující Hello ukazuje příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="70bfb-120">hello following shows hello command.</span></span>

```powershell
New-AzureRMResourceGroup -name MyResourceGroup -location "westus"
```

## <a name="create-a-dns-zone"></a><span data-ttu-id="70bfb-121">Vytvoření zóny DNS</span><span class="sxs-lookup"><span data-stu-id="70bfb-121">Create a DNS zone</span></span>

<span data-ttu-id="70bfb-122">Zóny DNS je vytvořená pomocí hello `New-AzureRmDnsZone` rutiny.</span><span class="sxs-lookup"><span data-stu-id="70bfb-122">A DNS zone is created by using hello `New-AzureRmDnsZone` cmdlet.</span></span> <span data-ttu-id="70bfb-123">Hello následující příklad vytvoří zónu DNS s názvem *contoso.com* v hello skupinu prostředků s názvem *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="70bfb-123">hello following example creates a DNS zone called *contoso.com* in hello resource group called *MyResourceGroup*.</span></span> <span data-ttu-id="70bfb-124">Použijte příklad hello toocreate zóny DNS, nahraďte hello hodnoty vlastními.</span><span class="sxs-lookup"><span data-stu-id="70bfb-124">Use hello example toocreate a DNS zone, substituting hello values for your own.</span></span>

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyResourceGroup
```

## <a name="create-a-dns-record"></a><span data-ttu-id="70bfb-125">Vytvoření záznamu DNS</span><span class="sxs-lookup"><span data-stu-id="70bfb-125">Create a DNS record</span></span>

<span data-ttu-id="70bfb-126">Vytvoření sady záznamů pomocí hello `New-AzureRmDnsRecordSet` rutiny.</span><span class="sxs-lookup"><span data-stu-id="70bfb-126">You create record sets by using hello `New-AzureRmDnsRecordSet` cmdlet.</span></span> <span data-ttu-id="70bfb-127">Hello následující příklad vytvoří záznam s relativním názvem "www" v zóně DNS "contoso.com" ve skupině prostředků "MyResourceGroup" hello hello.</span><span class="sxs-lookup"><span data-stu-id="70bfb-127">hello following example creates a record with hello relative name "www" in hello DNS Zone "contoso.com", in resource group "MyResourceGroup".</span></span> <span data-ttu-id="70bfb-128">Hello plně kvalifikovaný název sady záznamů hello je "www.contoso.com".</span><span class="sxs-lookup"><span data-stu-id="70bfb-128">hello fully-qualified name of hello record set is "www.contoso.com".</span></span> <span data-ttu-id="70bfb-129">Typ záznamu Hello je "A" s IP adresou "1.2.3.4" a je hello TTL 3 600 sekund.</span><span class="sxs-lookup"><span data-stu-id="70bfb-129">hello record type is "A", with IP address "1.2.3.4", and hello TTL is 3600 seconds.</span></span>

```powershell
New-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName contoso.com -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4")
```

<span data-ttu-id="70bfb-130">U jiných typů záznamů s více než jeden záznam a existující záznamy toomodify sady záznamů, najdete v části [záznamy DNS spravovat a sad záznamů pomocí Azure PowerShell](dns-operations-recordsets.md).</span><span class="sxs-lookup"><span data-stu-id="70bfb-130">For other record types, for record sets with more than one record, and toomodify existing records, see [Manage DNS records and record sets using Azure PowerShell](dns-operations-recordsets.md).</span></span> 


## <a name="view-records"></a><span data-ttu-id="70bfb-131">Zobrazení záznamů</span><span class="sxs-lookup"><span data-stu-id="70bfb-131">View records</span></span>

<span data-ttu-id="70bfb-132">toolist hello záznamy DNS v zóně, použijte:</span><span class="sxs-lookup"><span data-stu-id="70bfb-132">toolist hello DNS records in your zone, use:</span></span>

```powershell
Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyResourceGroup
```


## <a name="update-name-servers"></a><span data-ttu-id="70bfb-133">Aktualizace názvových serverů</span><span class="sxs-lookup"><span data-stu-id="70bfb-133">Update name servers</span></span>

<span data-ttu-id="70bfb-134">Jakmile jste uspokojit, že zóna DNS a záznamy byly nastaveny správně, musíte tooconfigure váš název domény toouse názvových serverů Azure DNS hello.</span><span class="sxs-lookup"><span data-stu-id="70bfb-134">Once you are satisfied that your DNS zone and records have been set up correctly, you need tooconfigure your domain name toouse hello Azure DNS name servers.</span></span> <span data-ttu-id="70bfb-135">To umožňuje jiných uživatelů v Internetu toofind hello svoje záznamy DNS.</span><span class="sxs-lookup"><span data-stu-id="70bfb-135">This enables other users on hello Internet toofind your DNS records.</span></span>

<span data-ttu-id="70bfb-136">Hello názvové servery zóny jsou poskytují hello `Get-AzureRmDnsZone` rutiny:</span><span class="sxs-lookup"><span data-stu-id="70bfb-136">hello name servers for your zone are given by hello `Get-AzureRmDnsZone` cmdlet:</span></span>

```powershell
Get-AzureRmDnsZone -ZoneName contoso.com -ResourceGroupName MyResourceGroup

Name                  : contoso.com
ResourceGroupName     : myresourcegroup
Etag                  : 00000003-0000-0000-b40d-0996b97ed101
Tags                  : {}
NameServers           : {ns1-01.azure-dns.com., ns2-01.azure-dns.net., ns3-01.azure-dns.org., ns4-01.azure-dns.info.}
NumberOfRecordSets    : 3
MaxNumberOfRecordSets : 5000
```

<span data-ttu-id="70bfb-137">Tyto názvové servery musí být nakonfigurovaný u registrátora názvu domény hello (kde jste zakoupili hello název domény).</span><span class="sxs-lookup"><span data-stu-id="70bfb-137">These name servers should be configured with hello domain name registrar (where you purchased hello domain name).</span></span> <span data-ttu-id="70bfb-138">Vám Registrátor nabídne možnost tooset hello až hello názvové servery pro doménu hello.</span><span class="sxs-lookup"><span data-stu-id="70bfb-138">Your registrar will offer hello option tooset up hello name servers for hello domain.</span></span> <span data-ttu-id="70bfb-139">Další informace najdete v tématu [delegovat tooAzure vaší domény DNS](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="70bfb-139">For more information, see [Delegate your domain tooAzure DNS](dns-domain-delegation.md).</span></span>

## <a name="delete-all-resources"></a><span data-ttu-id="70bfb-140">Odstranění všech prostředků</span><span class="sxs-lookup"><span data-stu-id="70bfb-140">Delete all resources</span></span>

<span data-ttu-id="70bfb-141">toodelete vytvořit všechny prostředky v tomto článku, hello proveďte následující krok:</span><span class="sxs-lookup"><span data-stu-id="70bfb-141">toodelete all resources created in this article, take hello following step:</span></span>

```powershell
Remove-AzureRMResourceGroup -Name MyResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="70bfb-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="70bfb-142">Next steps</span></span>

<span data-ttu-id="70bfb-143">toolearn Další informace o Azure DNS, najdete v části [přehled Azure DNS](dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="70bfb-143">toolearn more about Azure DNS, see [Azure DNS overview](dns-overview.md).</span></span>

<span data-ttu-id="70bfb-144">toolearn Další informace o správě zóny DNS v Azure DNS, najdete v části [zóny DNS spravovat v Azure DNS pomocí prostředí PowerShell](dns-operations-dnszones.md).</span><span class="sxs-lookup"><span data-stu-id="70bfb-144">toolearn more about managing DNS zones in Azure DNS, see [Manage DNS zones in Azure DNS using PowerShell](dns-operations-dnszones.md).</span></span>

<span data-ttu-id="70bfb-145">toolearn Další informace o správě záznamů DNS v Azure DNS, najdete v části [sad záznamů a záznamů DNS spravovat v Azure DNS pomocí prostředí PowerShell](dns-operations-recordsets.md).</span><span class="sxs-lookup"><span data-stu-id="70bfb-145">toolearn more about managing DNS records in Azure DNS, see [Manage DNS records and record sets in Azure DNS using PowerShell](dns-operations-recordsets.md).</span></span>

