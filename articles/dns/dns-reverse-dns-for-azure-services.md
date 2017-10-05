---
title: "Reverse DNS pro služby Azure | Microsoft Docs"
description: "Naučte se konfigurovat zpětné vyhledávání DNS pro služby hostované v Azure"
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/29/2017
ms.author: jonatul
ms.openlocfilehash: 63701e1ce0c1c6dcf2ce02ebce272b8280395e7f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-reverse-dns-for-services-hosted-in-azure"></a><span data-ttu-id="86c75-103">Konfigurace zpětné DNS pro služby hostované v Azure</span><span class="sxs-lookup"><span data-stu-id="86c75-103">Configure reverse DNS for services hosted in Azure</span></span>

<span data-ttu-id="86c75-104">Tento článek vysvětluje postup konfigurace zpětného vyhledávání DNS pro služby hostované v Azure.</span><span class="sxs-lookup"><span data-stu-id="86c75-104">This article explains how to configure reverse DNS lookups for services hosted in Azure.</span></span>

<span data-ttu-id="86c75-105">Služby v Azure pomocí IP adresy přiřazené službou Azure a vlastnictví společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="86c75-105">Services in Azure use IP addresses assigned by Azure and owned by Microsoft.</span></span> <span data-ttu-id="86c75-106">Tyto zpětné záznamy DNS (záznam PTR) musí být vytvořen v odpovídající ve vlastnictví společnosti Microsoft zpětného vyhledávání zóny DNS.</span><span class="sxs-lookup"><span data-stu-id="86c75-106">These reverse DNS records (PTR records) must be created in the corresponding Microsoft-owned reverse DNS lookup zones.</span></span> <span data-ttu-id="86c75-107">Tento článek vysvětluje, jak to udělat.</span><span class="sxs-lookup"><span data-stu-id="86c75-107">This article explains how to do this.</span></span>

<span data-ttu-id="86c75-108">Tento scénář Nezaměňovat s schopnost [hostitele zpětné vyhledávání zóny DNS pro vaše přiřazené rozsahy IP v Azure DNS](dns-reverse-dns-hosting.md).</span><span class="sxs-lookup"><span data-stu-id="86c75-108">This scenario should not be confused with the ability to [host the reverse DNS lookup zones for your assigned IP ranges in Azure DNS](dns-reverse-dns-hosting.md).</span></span> <span data-ttu-id="86c75-109">V takovém případě rozsahy IP reprezentována zóny zpětného vyhledávání musí být přiřadila pro vaši organizaci, obvykle vašeho poskytovatele internetových služeb.</span><span class="sxs-lookup"><span data-stu-id="86c75-109">In this case, the IP ranges represented by the reverse lookup zone must be assigned to your organization, typically by your ISP.</span></span>

<span data-ttu-id="86c75-110">Před přečtení tohoto článku, měli byste se seznámit s tím [přehled zpětné DNS a podpory v Azure](dns-reverse-dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="86c75-110">Before reading this article, you should be familiar with this [Overview of reverse DNS and support in Azure](dns-reverse-dns-overview.md).</span></span>

<span data-ttu-id="86c75-111">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="86c75-111">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>
* <span data-ttu-id="86c75-112">V modelu nasazení Resource Manager výpočetní prostředky (třeba virtuální počítače sady škálování virtuálního počítače nebo clusterů Service Fabric) jsou zveřejňovány prostřednictvím PublicIpAddress prostředků.</span><span class="sxs-lookup"><span data-stu-id="86c75-112">In the Resource Manager deployment model, compute resources (such as virtual machines, virtual machine scale sets, or Service Fabric clusters) are exposed via a PublicIpAddress resource.</span></span> <span data-ttu-id="86c75-113">Zpětné vyhledávání DNS se konfiguruje pomocí vlastnosti 'ReverseFqdn' PublicIpAddress.</span><span class="sxs-lookup"><span data-stu-id="86c75-113">Reverse DNS lookups are configured using the 'ReverseFqdn' property of the PublicIpAddress.</span></span>
* <span data-ttu-id="86c75-114">V modelu nasazení Classic se zveřejňují výpočetní prostředky, cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="86c75-114">In the Classic deployment model, compute resources are exposed using Cloud Services.</span></span> <span data-ttu-id="86c75-115">Zpětné vyhledávání DNS jsou nakonfigurovány pomocí vlastnosti 'ReverseDnsFqdn' cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="86c75-115">Reverse DNS lookups are configured using the 'ReverseDnsFqdn' property of the Cloud Service.</span></span>

<span data-ttu-id="86c75-116">Zpětné DNS není aktuálně podporován pro službu Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="86c75-116">Reverse DNS is not currently supported for the Azure App Service.</span></span>

## <a name="validation-of-reverse-dns-records"></a><span data-ttu-id="86c75-117">Ověření záznamy zpětného vyhledávání DNS</span><span class="sxs-lookup"><span data-stu-id="86c75-117">Validation of reverse DNS records</span></span>

<span data-ttu-id="86c75-118">Třetí strany neměli mít možnost vytvářet zpětné záznamy DNS pro jejich mapování služby Azure na vašich domén DNS.</span><span class="sxs-lookup"><span data-stu-id="86c75-118">A third party should not be able to create reverse DNS records for their Azure service mapping to your DNS domains.</span></span> <span data-ttu-id="86c75-119">Chcete-li tomu zabránit, Azure jenom umožňuje vytvoření zpětné záznam DNS, kde název domény zadaný v zpětné záznam DNS je stejný jako nebo přeloží na název DNS nebo IP adresu PublicIpAddress nebo cloudové služby ve stejném předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="86c75-119">To prevent this, Azure only allows the creation of a reverse DNS record where domain name specified in the reverse DNS record is the same as, or resolves to, the DNS name or IP address of a PublicIpAddress or Cloud Service in the same Azure subscription.</span></span>

<span data-ttu-id="86c75-120">Toto ověření se provádí pouze při nastavit nebo změnit zpětné záznamu DNS.</span><span class="sxs-lookup"><span data-stu-id="86c75-120">This validation is only performed when the reverse DNS record is set or modified.</span></span> <span data-ttu-id="86c75-121">Pravidelně opakované ověření není provedena.</span><span class="sxs-lookup"><span data-stu-id="86c75-121">Periodic re-validation is not performed.</span></span>

<span data-ttu-id="86c75-122">Příklad: Předpokládejme, že má prostředek PublicIpAddress contosoapp1.northus.cloudapp.azure.com název DNS a IP adresu 23.96.52.53.</span><span class="sxs-lookup"><span data-stu-id="86c75-122">For example: suppose the PublicIpAddress resource has the DNS name contosoapp1.northus.cloudapp.azure.com and IP address 23.96.52.53.</span></span> <span data-ttu-id="86c75-123">ReverseFqdn pro PublicIpAddress lze zadat jako:</span><span class="sxs-lookup"><span data-stu-id="86c75-123">The ReverseFqdn for the PublicIpAddress can be specified as:</span></span>
* <span data-ttu-id="86c75-124">Název DNS pro PublicIpAddress, contosoapp1.northus.cloudapp.azure.com</span><span class="sxs-lookup"><span data-stu-id="86c75-124">The DNS name for the PublicIpAddress, contosoapp1.northus.cloudapp.azure.com</span></span>
* <span data-ttu-id="86c75-125">Název DNS pro různé PublicIpAddress v rámci stejného předplatného, jako je například contosoapp2.westus.cloudapp.azure.com</span><span class="sxs-lookup"><span data-stu-id="86c75-125">The DNS name for a different PublicIpAddress in the same subscription, such as contosoapp2.westus.cloudapp.azure.com</span></span>
* <span data-ttu-id="86c75-126">Jednoduché DNS název, jako je například app1.contoso.com, tak dlouho, dokud tento název se *první* nakonfigurovaný jako záznam CNAME contosoapp1.northus.cloudapp.azure.com nebo na jinou PublicIpAddress ve stejném předplatném.</span><span class="sxs-lookup"><span data-stu-id="86c75-126">A vanity DNS name, such as app1.contoso.com, so long as this name is *first* configured as a CNAME to contosoapp1.northus.cloudapp.azure.com, or to a different PublicIpAddress in the same subscription.</span></span>
* <span data-ttu-id="86c75-127">Jednoduché DNS název, jako je například app1.contoso.com, tak dlouho, dokud tento název se *první* nakonfigurovaný jako záznam na IP adresu 23.96.52.53, nebo na IP adresu na jinou PublicIpAddress ve stejném předplatném.</span><span class="sxs-lookup"><span data-stu-id="86c75-127">A vanity DNS name, such as app1.contoso.com, so long as this name is *first* configured as an A record to the IP address 23.96.52.53, or to the IP address of a different PublicIpAddress in the same subscription.</span></span>

<span data-ttu-id="86c75-128">Stejné omezení se vztahují na zpětnou DNS pro cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="86c75-128">The same constraints apply to reverse DNS for Cloud Services.</span></span>


## <a name="reverse-dns-for-publicipaddress-resources"></a><span data-ttu-id="86c75-129">Reverse DNS pro prostředky PublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="86c75-129">Reverse DNS for PublicIpAddress resources</span></span>

<span data-ttu-id="86c75-130">Tato část obsahuje podrobné pokyny pro konfiguraci zpětné DNS pro prostředky PublicIpAddress v modelu nasazení Resource Manager pomocí prostředí Azure PowerShell, Azure CLI 1.0 nebo 2.0 rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="86c75-130">This section provides detailed instructions for how to configure reverse DNS for PublicIpAddress resources in the Resource Manager deployment model, using either Azure PowerShell, Azure CLI 1.0, or Azure CLI 2.0.</span></span> <span data-ttu-id="86c75-131">Konfigurace zpětné DNS pro prostředky PublicIpAddress není podporována aktuálně prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="86c75-131">Configuring reverse DNS for PublicIpAddress resources is not currently supported via the Azure portal.</span></span>

<span data-ttu-id="86c75-132">Azure aktuálně podporuje reverse DNS pouze pro prostředky IPv4 PublicIpAddress.</span><span class="sxs-lookup"><span data-stu-id="86c75-132">Azure currently supports reverse DNS only for IPv4 PublicIpAddress resources.</span></span> <span data-ttu-id="86c75-133">Není podporováno pro protokol IPv6.</span><span class="sxs-lookup"><span data-stu-id="86c75-133">It is not supported for IPv6.</span></span>

### <a name="add-reverse-dns-to-an-existing-publicipaddresses"></a><span data-ttu-id="86c75-134">Přidejte do existující publicipaddresses, na které zpětné DNS</span><span class="sxs-lookup"><span data-stu-id="86c75-134">Add reverse DNS to an existing PublicIpAddresses</span></span>

#### <a name="powershell"></a><span data-ttu-id="86c75-135">PowerShell</span><span class="sxs-lookup"><span data-stu-id="86c75-135">PowerShell</span></span>

<span data-ttu-id="86c75-136">Přidání zpětné DNS do existující PublicIpAddress:</span><span class="sxs-lookup"><span data-stu-id="86c75-136">To add reverse DNS to an existing PublicIpAddress:</span></span>

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

<span data-ttu-id="86c75-137">Pokud chcete přidat zpětné DNS do existující PublicIpAddress, který ještě nemá název DNS, musíte zadat také název DNS:</span><span class="sxs-lookup"><span data-stu-id="86c75-137">To add reverse DNS to an existing PublicIpAddress that doesn't already have a DNS name, you must also specify a DNS name:</span></span>

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings = New-Object -TypeName "Microsoft.Azure.Commands.Network.Models.PSPublicIpAddressDnsSettings"
$pip.DnsSettings.DomainNameLabel = "contosoapp1"
$pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

#### <a name="azure-cli-10"></a><span data-ttu-id="86c75-138">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="86c75-138">Azure CLI 1.0</span></span>

<span data-ttu-id="86c75-139">Přidání zpětné DNS do existující PublicIpAddress:</span><span class="sxs-lookup"><span data-stu-id="86c75-139">To add reverse DNS to an existing PublicIpAddress:</span></span>

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup -f contosoapp1.westus.cloudapp.azure.com.
```

<span data-ttu-id="86c75-140">Pokud chcete přidat zpětné DNS do existující PublicIpAddress, který ještě nemá název DNS, musíte zadat také název DNS:</span><span class="sxs-lookup"><span data-stu-id="86c75-140">To add reverse DNS to an existing PublicIpAddress that doesn't already have a DNS name, you must also specify a DNS name:</span></span>

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup -d contosoapp1 -f contosoapp1.westus.cloudapp.azure.com.
```

#### <a name="azure-cli-20"></a><span data-ttu-id="86c75-141">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="86c75-141">Azure CLI 2.0</span></span>

<span data-ttu-id="86c75-142">Přidání zpětné DNS do existující PublicIpAddress:</span><span class="sxs-lookup"><span data-stu-id="86c75-142">To add reverse DNS to an existing PublicIpAddress:</span></span>

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn contosoapp1.westus.cloudapp.azure.com.
```

<span data-ttu-id="86c75-143">Pokud chcete přidat zpětné DNS do existující PublicIpAddress, který ještě nemá název DNS, musíte zadat také název DNS:</span><span class="sxs-lookup"><span data-stu-id="86c75-143">To add reverse DNS to an existing PublicIpAddress that doesn't already have a DNS name, you must also specify a DNS name:</span></span>

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn contosoapp1.westus.cloudapp.azure.com --dns-name contosoapp1
```

### <a name="create-a-public-ip-address-with-reverse-dns"></a><span data-ttu-id="86c75-144">Vytvoření veřejné IP adresy s zpětné DNS</span><span class="sxs-lookup"><span data-stu-id="86c75-144">Create a Public IP Address with reverse DNS</span></span>

<span data-ttu-id="86c75-145">Vytvoření nové PublicIpAddress s vlastností zpětné DNS už zadali:</span><span class="sxs-lookup"><span data-stu-id="86c75-145">To create a new PublicIpAddress with the reverse DNS property already specified:</span></span>

#### <a name="powershell"></a><span data-ttu-id="86c75-146">PowerShell</span><span class="sxs-lookup"><span data-stu-id="86c75-146">PowerShell</span></span>

```powershell
New-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup" -Location "WestUS" -AllocationMethod Dynamic -DomainNameLabel "contosoapp2" -ReverseFqdn "contosoapp2.westus.cloudapp.azure.com."
```

#### <a name="azure-cli-10"></a><span data-ttu-id="86c75-147">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="86c75-147">Azure CLI 1.0</span></span>

```azurecli
azure network public-ip create -n PublicIp -g MyResourceGroup -l westus -d contosoapp3 -f contosoapp3.westus.cloudapp.azure.com.
```

#### <a name="azure-cli-20"></a><span data-ttu-id="86c75-148">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="86c75-148">Azure CLI 2.0</span></span>

```azurecli
az network public-ip create --name PublicIp --resource-group MyResourceGroup --location westcentralus --dns-name contosoapp1 --reverse-fqdn contosoapp1.westcentralus.cloudapp.azure.com
```

### <a name="view-reverse-dns-for-an-existing-publicipaddress"></a><span data-ttu-id="86c75-149">Zobrazení zpětného DNS pro existující PublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="86c75-149">View reverse DNS for an existing PublicIpAddress</span></span>

<span data-ttu-id="86c75-150">Chcete-li zobrazit konfigurovaná hodnota pro existující PublicIpAddress:</span><span class="sxs-lookup"><span data-stu-id="86c75-150">To view the configured value for an existing PublicIpAddress:</span></span>

#### <a name="powershell"></a><span data-ttu-id="86c75-151">PowerShell</span><span class="sxs-lookup"><span data-stu-id="86c75-151">PowerShell</span></span>

```powershell
Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
```

#### <a name="azure-cli-10"></a><span data-ttu-id="86c75-152">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="86c75-152">Azure CLI 1.0</span></span>

```azurecli
azure network public-ip show -n PublicIp -g MyResourceGroup
```

#### <a name="azure-cli-20"></a><span data-ttu-id="86c75-153">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="86c75-153">Azure CLI 2.0</span></span>

```azurecli
az network public-ip show --name PublicIp --resource-group MyResourceGroup
```

### <a name="remove-reverse-dns-from-existing-public-ip-addresses"></a><span data-ttu-id="86c75-154">Odeberte zpětné DNS z existující veřejné IP adresy</span><span class="sxs-lookup"><span data-stu-id="86c75-154">Remove reverse DNS from existing Public IP Addresses</span></span>

<span data-ttu-id="86c75-155">Odebrání stávající PublicIpAddress zpětné vlastnosti DNS:</span><span class="sxs-lookup"><span data-stu-id="86c75-155">To remove a reverse DNS property from an existing PublicIpAddress:</span></span>

#### <a name="powershell"></a><span data-ttu-id="86c75-156">PowerShell</span><span class="sxs-lookup"><span data-stu-id="86c75-156">PowerShell</span></span>

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings.ReverseFqdn = ""
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

#### <a name="azure-cli-10"></a><span data-ttu-id="86c75-157">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="86c75-157">Azure CLI 1.0</span></span>

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup –f ""
```

#### <a name="azure-cli-20"></a><span data-ttu-id="86c75-158">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="86c75-158">Azure CLI 2.0</span></span>

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn ""
```


## <a name="configure-reverse-dns-for-cloud-services"></a><span data-ttu-id="86c75-159">Konfigurace zpětné DNS pro cloudové služby</span><span class="sxs-lookup"><span data-stu-id="86c75-159">Configure reverse DNS for Cloud Services</span></span>

<span data-ttu-id="86c75-160">Tato část obsahuje podrobné pokyny pro konfiguraci DNS zpětného pro cloudové služby v modelu nasazení Classic pomocí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="86c75-160">This section provides detailed instructions for how to configure reverse DNS for Cloud Services in the Classic deployment model, using Azure PowerShell.</span></span> <span data-ttu-id="86c75-161">Konfigurace zpětné DNS pro cloudové služby není podporována prostřednictvím portálu Azure, Azure CLI 1.0 nebo 2.0 rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="86c75-161">Configuring reverse DNS for Cloud Services is not supported via the Azure portal, Azure CLI 1.0, or Azure CLI 2.0.</span></span>

### <a name="add-reverse-dns-to-existing-cloud-services"></a><span data-ttu-id="86c75-162">Přidání zpětné DNS do existující cloudové služby</span><span class="sxs-lookup"><span data-stu-id="86c75-162">Add reverse DNS to existing Cloud Services</span></span>

<span data-ttu-id="86c75-163">Chcete-li přidat zpětné záznam DNS do existující cloudové služby:</span><span class="sxs-lookup"><span data-stu-id="86c75-163">To add a reverse DNS record to an existing Cloud Service:</span></span>

```powershell
Set-AzureService –ServiceName "contosoapp1" –Description "App1 with Reverse DNS" –ReverseDnsFqdn "contosoapp1.cloudapp.net."
```

### <a name="create-a-cloud-service-with-reverse-dns"></a><span data-ttu-id="86c75-164">Vytvoření cloudové služby se zpětné DNS</span><span class="sxs-lookup"><span data-stu-id="86c75-164">Create a Cloud Service with reverse DNS</span></span>

<span data-ttu-id="86c75-165">Chcete-li vytvořit novou Cloudovou službu s vlastností zpětné DNS již zadán:</span><span class="sxs-lookup"><span data-stu-id="86c75-165">To create a new Cloud Service with the reverse DNS property already specified:</span></span>

```powershell
New-AzureService –ServiceName "contosoapp1" –Location "West US" –Description "App1 with Reverse DNS" –ReverseDnsFqdn "contosoapp1.cloudapp.net."
```

### <a name="view-reverse-dns-for-existing-cloud-services"></a><span data-ttu-id="86c75-166">Zobrazení zpětného DNS pro existující cloudové služby</span><span class="sxs-lookup"><span data-stu-id="86c75-166">View reverse DNS for existing Cloud Services</span></span>

<span data-ttu-id="86c75-167">Pokud chcete zobrazit zpětné vlastnosti DNS pro stávající Cloudovou službu:</span><span class="sxs-lookup"><span data-stu-id="86c75-167">To view the reverse DNS property for an existing Cloud Service:</span></span>

```powershell
Get-AzureService "contosoapp1"
```

### <a name="remove-reverse-dns-from-existing-cloud-services"></a><span data-ttu-id="86c75-168">Odeberte zpětné DNS z existující cloudové služby</span><span class="sxs-lookup"><span data-stu-id="86c75-168">Remove reverse DNS from existing Cloud Services</span></span>

<span data-ttu-id="86c75-169">Pokud chcete odebrat zpětné DNS z existující cloudové služby:</span><span class="sxs-lookup"><span data-stu-id="86c75-169">To remove a reverse DNS property from an existing Cloud Service:</span></span>

```powershell
Set-AzureService –ServiceName "contosoapp1" –Description "App1 with Reverse DNS" –ReverseDnsFqdn ""
```

## <a name="faq"></a><span data-ttu-id="86c75-170">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="86c75-170">FAQ</span></span>

### <a name="how-much-do-reverse-dns-records-cost"></a><span data-ttu-id="86c75-171">Kolik reverse náklady na záznamy DNS?</span><span class="sxs-lookup"><span data-stu-id="86c75-171">How much do reverse DNS records cost?</span></span>

<span data-ttu-id="86c75-172">Jsou zdarma!</span><span class="sxs-lookup"><span data-stu-id="86c75-172">They're free!</span></span>  <span data-ttu-id="86c75-173">Není k dispozici bez dalších nákladů pro zpětné záznamy DNS nebo dotazy.</span><span class="sxs-lookup"><span data-stu-id="86c75-173">There is no additional cost for reverse DNS records or queries.</span></span>

### <a name="will-my-reverse-dns-records-resolve-from-the-internet"></a><span data-ttu-id="86c75-174">Odstraní zpětné záznamy DNS z Internetu?</span><span class="sxs-lookup"><span data-stu-id="86c75-174">Will my reverse DNS records resolve from the internet?</span></span>

<span data-ttu-id="86c75-175">Ano.</span><span class="sxs-lookup"><span data-stu-id="86c75-175">Yes.</span></span> <span data-ttu-id="86c75-176">Jakmile jednou nastavíte vlastnost zpětné DNS služby Azure, Azure spravuje všechny delegování DNS a vyžaduje se pro zajištění toho, zda řeší zpětné záznam DNS pro všechny uživatele Internetu zóny DNS.</span><span class="sxs-lookup"><span data-stu-id="86c75-176">Once you set the reverse DNS property for your Azure service, Azure manages all the DNS delegations and DNS zones required to ensure that reverse DNS record resolves for all Internet users.</span></span>

### <a name="are-default-reverse-dns-records-created-for-my-azure-services"></a><span data-ttu-id="86c75-177">Vytvářejí výchozí záznamy zpětného vyhledávání DNS pro Moje služby Azure?</span><span class="sxs-lookup"><span data-stu-id="86c75-177">Are default reverse DNS records created for my Azure services?</span></span>

<span data-ttu-id="86c75-178">Ne.</span><span class="sxs-lookup"><span data-stu-id="86c75-178">No.</span></span> <span data-ttu-id="86c75-179">Zpětné DNS je funkce přihlášení.</span><span class="sxs-lookup"><span data-stu-id="86c75-179">Reverse DNS is an opt-in feature.</span></span> <span data-ttu-id="86c75-180">Žádné záznamy výchozí zpětného vyhledávání DNS se vytvoří, pokud se rozhodnete je nakonfigurovat.</span><span class="sxs-lookup"><span data-stu-id="86c75-180">No default reverse DNS records are created if you choose not to configure them.</span></span>

### <a name="what-is-the-format-for-the-fully-qualified-domain-name-fqdn"></a><span data-ttu-id="86c75-181">Co je formát plně kvalifikovaný název domény (FQDN)?</span><span class="sxs-lookup"><span data-stu-id="86c75-181">What is the format for the fully-qualified domain name (FQDN)?</span></span>

<span data-ttu-id="86c75-182">Plně kvalifikované názvy domény jsou zadány popořadě a musí být ukončen tečkou (například "app1.contoso.com.").</span><span class="sxs-lookup"><span data-stu-id="86c75-182">FQDNs are specified in forward order, and must be terminated by a dot (for example, "app1.contoso.com.").</span></span>

### <a name="what-happens-if-the-validation-check-for-the-reverse-dns-ive-specified-fails"></a><span data-ttu-id="86c75-183">Co se stane, když ověření pravosti pro zpětné DNS I jste určili nezdaří?</span><span class="sxs-lookup"><span data-stu-id="86c75-183">What happens if the validation check for the reverse DNS I've specified fails?</span></span>

<span data-ttu-id="86c75-184">Kde zpětné DNS kontrolu ověření nezdaří, operace nakonfigurovat zpětné záznam DNS se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="86c75-184">Where the reverse DNS validation check fails, the operation to configure the reverse DNS record fails.</span></span> <span data-ttu-id="86c75-185">Opravte hodnotu zpětné DNS podle potřeby a opakujte.</span><span class="sxs-lookup"><span data-stu-id="86c75-185">Correct the reverse DNS value as required, and retry.</span></span>

### <a name="can-i-configure-reverse-dns-for-azure-app-service"></a><span data-ttu-id="86c75-186">Můžete nakonfigurovat zpětné DNS pro službu Azure App Service?</span><span class="sxs-lookup"><span data-stu-id="86c75-186">Can I configure reverse DNS for Azure App Service?</span></span>

<span data-ttu-id="86c75-187">Ne.</span><span class="sxs-lookup"><span data-stu-id="86c75-187">No.</span></span> <span data-ttu-id="86c75-188">Zpětné DNS není podporována pro službu Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="86c75-188">Reverse DNS is not supported for the Azure App Service.</span></span>

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-azure-service"></a><span data-ttu-id="86c75-189">Můžete nakonfigurovat více zpětné záznamy DNS pro Moje služba Azure?</span><span class="sxs-lookup"><span data-stu-id="86c75-189">Can I configure multiple reverse DNS records for my Azure service?</span></span>

<span data-ttu-id="86c75-190">Ne.</span><span class="sxs-lookup"><span data-stu-id="86c75-190">No.</span></span> <span data-ttu-id="86c75-191">Azure podporuje jeden zpětné záznam DNS pro každé cloudové služby Azure nebo PublicIpAddress.</span><span class="sxs-lookup"><span data-stu-id="86c75-191">Azure supports a single reverse DNS record for each Azure Cloud Service or PublicIpAddress.</span></span>

### <a name="can-i-configure-reverse-dns-for-ipv6-publicipaddress-resources"></a><span data-ttu-id="86c75-192">Můžete nakonfigurovat zpětné DNS pro prostředky IPv6 PublicIpAddress?</span><span class="sxs-lookup"><span data-stu-id="86c75-192">Can I configure reverse DNS for IPv6 PublicIpAddress resources?</span></span>

<span data-ttu-id="86c75-193">Ne.</span><span class="sxs-lookup"><span data-stu-id="86c75-193">No.</span></span> <span data-ttu-id="86c75-194">Azure aktuálně podporuje reverse DNS pouze pro prostředky IPv4 PublicIpAddress a cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="86c75-194">Azure currently supports reverse DNS only for IPv4 PublicIpAddress resources and Cloud Services.</span></span>

### <a name="can-i-send-emails-to-external-domains-from-my-azure-compute-services"></a><span data-ttu-id="86c75-195">Můžete odeslat e-mailů k externími doménami z mé Azure výpočetní služby?</span><span class="sxs-lookup"><span data-stu-id="86c75-195">Can I send emails to external domains from my Azure Compute services?</span></span>

<span data-ttu-id="86c75-196">Ne.</span><span class="sxs-lookup"><span data-stu-id="86c75-196">No.</span></span> [<span data-ttu-id="86c75-197">Azure výpočetní služby nepodporují odesílání e-mailů externími doménami</span><span class="sxs-lookup"><span data-stu-id="86c75-197">Azure Compute services do not support sending emails to external domains</span></span>](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/)

## <a name="next-steps"></a><span data-ttu-id="86c75-198">Další kroky</span><span class="sxs-lookup"><span data-stu-id="86c75-198">Next steps</span></span>

<span data-ttu-id="86c75-199">Další informace o zpětné DNS najdete v tématu [zpětného vyhledávání DNS na webu Wikipedia](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).</span><span class="sxs-lookup"><span data-stu-id="86c75-199">For more information on reverse DNS, see [reverse DNS lookup on Wikipedia](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).</span></span>
<br>
<span data-ttu-id="86c75-200">Zjistěte, jak [hostitel zóny zpětného vyhledávání pro váš rozsah poskytovatele internetových služeb přiřadit IP v Azure DNS](dns-reverse-dns-for-azure-services.md).</span><span class="sxs-lookup"><span data-stu-id="86c75-200">Learn how to [host the reverse lookup zone for your ISP-assigned IP range in Azure DNS](dns-reverse-dns-for-azure-services.md).</span></span>
