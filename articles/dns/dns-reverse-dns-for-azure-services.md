---
title: "aaaReverse DNS pro služby Azure | Microsoft Docs"
description: "Zjistěte, jak tooconfigure zpětné vyhledávání DNS pro služby hostované v Azure"
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
ms.openlocfilehash: c6fe1d80232f124be86dd7fc57abc20699be7eba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-reverse-dns-for-services-hosted-in-azure"></a><span data-ttu-id="faecb-103">Konfigurace zpětné DNS pro služby hostované v Azure</span><span class="sxs-lookup"><span data-stu-id="faecb-103">Configure reverse DNS for services hosted in Azure</span></span>

<span data-ttu-id="faecb-104">Tento článek vysvětluje, jak tooconfigure zpětné vyhledávání DNS pro služby hostované v Azure.</span><span class="sxs-lookup"><span data-stu-id="faecb-104">This article explains how tooconfigure reverse DNS lookups for services hosted in Azure.</span></span>

<span data-ttu-id="faecb-105">Služby v Azure pomocí IP adresy přiřazené službou Azure a vlastnictví společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="faecb-105">Services in Azure use IP addresses assigned by Azure and owned by Microsoft.</span></span> <span data-ttu-id="faecb-106">Tyto zpětné záznamy DNS (záznam PTR) musí být vytvořen v hello odpovídající ve vlastnictví společnosti Microsoft zpětné vyhledávání zóny DNS.</span><span class="sxs-lookup"><span data-stu-id="faecb-106">These reverse DNS records (PTR records) must be created in hello corresponding Microsoft-owned reverse DNS lookup zones.</span></span> <span data-ttu-id="faecb-107">Tento článek vysvětluje, jak toodo to.</span><span class="sxs-lookup"><span data-stu-id="faecb-107">This article explains how toodo this.</span></span>

<span data-ttu-id="faecb-108">Tento scénář by se neměly zaměňovat s hello možnost příliš[hostitele hello zpětné vyhledávání zóny DNS pro vaše přiřazené rozsahy IP v Azure DNS](dns-reverse-dns-hosting.md).</span><span class="sxs-lookup"><span data-stu-id="faecb-108">This scenario should not be confused with hello ability too[host hello reverse DNS lookup zones for your assigned IP ranges in Azure DNS](dns-reverse-dns-hosting.md).</span></span> <span data-ttu-id="faecb-109">V takovém případě rozsahy IP hello reprezentována zóny zpětného vyhledávání hello musí být přiřazen tooyour organizace, obvykle podle vašeho poskytovatele internetových služeb.</span><span class="sxs-lookup"><span data-stu-id="faecb-109">In this case, hello IP ranges represented by hello reverse lookup zone must be assigned tooyour organization, typically by your ISP.</span></span>

<span data-ttu-id="faecb-110">Před přečtení tohoto článku, měli byste se seznámit s tím [přehled zpětné DNS a podpory v Azure](dns-reverse-dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="faecb-110">Before reading this article, you should be familiar with this [Overview of reverse DNS and support in Azure](dns-reverse-dns-overview.md).</span></span>

<span data-ttu-id="faecb-111">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="faecb-111">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>
* <span data-ttu-id="faecb-112">V modelu nasazení Resource Manager hello výpočetní prostředky (třeba virtuální počítače sady škálování virtuálního počítače nebo clusterů Service Fabric) jsou zveřejňovány prostřednictvím PublicIpAddress prostředků.</span><span class="sxs-lookup"><span data-stu-id="faecb-112">In hello Resource Manager deployment model, compute resources (such as virtual machines, virtual machine scale sets, or Service Fabric clusters) are exposed via a PublicIpAddress resource.</span></span> <span data-ttu-id="faecb-113">Zpětné vyhledávání DNS se konfiguruje pomocí vlastnosti 'ReverseFqdn' hello hello PublicIpAddress.</span><span class="sxs-lookup"><span data-stu-id="faecb-113">Reverse DNS lookups are configured using hello 'ReverseFqdn' property of hello PublicIpAddress.</span></span>
* <span data-ttu-id="faecb-114">V modelu nasazení Classic hello se zveřejňují výpočetní prostředky, cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="faecb-114">In hello Classic deployment model, compute resources are exposed using Cloud Services.</span></span> <span data-ttu-id="faecb-115">Zpětné vyhledávání DNS se konfiguruje pomocí vlastnosti 'ReverseDnsFqdn' hello hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="faecb-115">Reverse DNS lookups are configured using hello 'ReverseDnsFqdn' property of hello Cloud Service.</span></span>

<span data-ttu-id="faecb-116">Zpětné DNS není aktuálně podporován pro hello Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="faecb-116">Reverse DNS is not currently supported for hello Azure App Service.</span></span>

## <a name="validation-of-reverse-dns-records"></a><span data-ttu-id="faecb-117">Ověření záznamy zpětného vyhledávání DNS</span><span class="sxs-lookup"><span data-stu-id="faecb-117">Validation of reverse DNS records</span></span>

<span data-ttu-id="faecb-118">Třetí strany by neměl být schopný toocreate zpětné záznamy DNS pro jejich službu Azure mapování tooyour DNS domény.</span><span class="sxs-lookup"><span data-stu-id="faecb-118">A third party should not be able toocreate reverse DNS records for their Azure service mapping tooyour DNS domains.</span></span> <span data-ttu-id="faecb-119">tooprevent, Azure umožňuje pouze hello vytvoření zpětné záznam DNS, kde je název domény určený v záznamu DNS zpětného hello hello stejný jako, nebo překládá, hello název DNS nebo IP adresu PublicIpAddress nebo cloudové služby v hello stejné předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="faecb-119">tooprevent this, Azure only allows hello creation of a reverse DNS record where domain name specified in hello reverse DNS record is hello same as, or resolves to, hello DNS name or IP address of a PublicIpAddress or Cloud Service in hello same Azure subscription.</span></span>

<span data-ttu-id="faecb-120">Toto ověření se provádí pouze při nastavit nebo změnit hello zpětné záznamu DNS.</span><span class="sxs-lookup"><span data-stu-id="faecb-120">This validation is only performed when hello reverse DNS record is set or modified.</span></span> <span data-ttu-id="faecb-121">Pravidelně opakované ověření není provedena.</span><span class="sxs-lookup"><span data-stu-id="faecb-121">Periodic re-validation is not performed.</span></span>

<span data-ttu-id="faecb-122">Příklad: Předpokládejme, že hello PublicIpAddress prostředků má contosoapp1.northus.cloudapp.azure.com název hello DNS a IP adresu 23.96.52.53.</span><span class="sxs-lookup"><span data-stu-id="faecb-122">For example: suppose hello PublicIpAddress resource has hello DNS name contosoapp1.northus.cloudapp.azure.com and IP address 23.96.52.53.</span></span> <span data-ttu-id="faecb-123">ReverseFqdn Hello hello PublicIpAddress lze zadat jako:</span><span class="sxs-lookup"><span data-stu-id="faecb-123">hello ReverseFqdn for hello PublicIpAddress can be specified as:</span></span>
* <span data-ttu-id="faecb-124">název DNS Hello hello PublicIpAddress, contosoapp1.northus.cloudapp.azure.com</span><span class="sxs-lookup"><span data-stu-id="faecb-124">hello DNS name for hello PublicIpAddress, contosoapp1.northus.cloudapp.azure.com</span></span>
* <span data-ttu-id="faecb-125">Hello na názvy DNS pro různé PublicIpAddress v hello stejné předplatné, jako například contosoapp2.westus.cloudapp.azure.com</span><span class="sxs-lookup"><span data-stu-id="faecb-125">hello DNS name for a different PublicIpAddress in hello same subscription, such as contosoapp2.westus.cloudapp.azure.com</span></span>
* <span data-ttu-id="faecb-126">Jednoduché DNS název, jako je například app1.contoso.com, tak dlouho, dokud tento název se *první* nakonfigurované jako CNAME toocontosoapp1.northus.cloudapp.azure.com nebo tooa různých PublicIpAddress v hello stejného předplatného.</span><span class="sxs-lookup"><span data-stu-id="faecb-126">A vanity DNS name, such as app1.contoso.com, so long as this name is *first* configured as a CNAME toocontosoapp1.northus.cloudapp.azure.com, or tooa different PublicIpAddress in hello same subscription.</span></span>
* <span data-ttu-id="faecb-127">Jednoduché DNS název, jako je například app1.contoso.com, tak dlouho, dokud tento název se *první* nakonfigurovaný jako IP adresu A záznamů toohello 23.96.52.53 nebo toohello IP adresu na jinou PublicIpAddress v hello stejného předplatného.</span><span class="sxs-lookup"><span data-stu-id="faecb-127">A vanity DNS name, such as app1.contoso.com, so long as this name is *first* configured as an A record toohello IP address 23.96.52.53, or toohello IP address of a different PublicIpAddress in hello same subscription.</span></span>

<span data-ttu-id="faecb-128">Hello stejné omezení platí tooreverse DNS pro cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="faecb-128">hello same constraints apply tooreverse DNS for Cloud Services.</span></span>


## <a name="reverse-dns-for-publicipaddress-resources"></a><span data-ttu-id="faecb-129">Reverse DNS pro prostředky PublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="faecb-129">Reverse DNS for PublicIpAddress resources</span></span>

<span data-ttu-id="faecb-130">Tato část obsahuje podrobné pokyny, jak tooconfigure reverse DNS pro prostředky PublicIpAddress v modelu nasazení Resource Manager hello, pomocí prostředí Azure PowerShell, Azure CLI 1.0 nebo 2.0 rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="faecb-130">This section provides detailed instructions for how tooconfigure reverse DNS for PublicIpAddress resources in hello Resource Manager deployment model, using either Azure PowerShell, Azure CLI 1.0, or Azure CLI 2.0.</span></span> <span data-ttu-id="faecb-131">Konfigurace zpětné DNS pro prostředky PublicIpAddress není aktuálně podporován prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="faecb-131">Configuring reverse DNS for PublicIpAddress resources is not currently supported via hello Azure portal.</span></span>

<span data-ttu-id="faecb-132">Azure aktuálně podporuje reverse DNS pouze pro prostředky IPv4 PublicIpAddress.</span><span class="sxs-lookup"><span data-stu-id="faecb-132">Azure currently supports reverse DNS only for IPv4 PublicIpAddress resources.</span></span> <span data-ttu-id="faecb-133">Není podporováno pro protokol IPv6.</span><span class="sxs-lookup"><span data-stu-id="faecb-133">It is not supported for IPv6.</span></span>

### <a name="add-reverse-dns-tooan-existing-publicipaddresses"></a><span data-ttu-id="faecb-134">Přidat existující publicipaddresses, na které zpětné DNS tooan</span><span class="sxs-lookup"><span data-stu-id="faecb-134">Add reverse DNS tooan existing PublicIpAddresses</span></span>

#### <a name="powershell"></a><span data-ttu-id="faecb-135">PowerShell</span><span class="sxs-lookup"><span data-stu-id="faecb-135">PowerShell</span></span>

<span data-ttu-id="faecb-136">tooadd reverse existující PublicIpAddress tooan DNS:</span><span class="sxs-lookup"><span data-stu-id="faecb-136">tooadd reverse DNS tooan existing PublicIpAddress:</span></span>

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

<span data-ttu-id="faecb-137">tooadd zpětně DNS tooan existující PublicIpAddress, který ještě nemá název DNS, musíte taky zadat název DNS:</span><span class="sxs-lookup"><span data-stu-id="faecb-137">tooadd reverse DNS tooan existing PublicIpAddress that doesn't already have a DNS name, you must also specify a DNS name:</span></span>

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings = New-Object -TypeName "Microsoft.Azure.Commands.Network.Models.PSPublicIpAddressDnsSettings"
$pip.DnsSettings.DomainNameLabel = "contosoapp1"
$pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

#### <a name="azure-cli-10"></a><span data-ttu-id="faecb-138">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="faecb-138">Azure CLI 1.0</span></span>

<span data-ttu-id="faecb-139">tooadd reverse existující PublicIpAddress tooan DNS:</span><span class="sxs-lookup"><span data-stu-id="faecb-139">tooadd reverse DNS tooan existing PublicIpAddress:</span></span>

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup -f contosoapp1.westus.cloudapp.azure.com.
```

<span data-ttu-id="faecb-140">tooadd zpětně DNS tooan existující PublicIpAddress, který ještě nemá název DNS, musíte taky zadat název DNS:</span><span class="sxs-lookup"><span data-stu-id="faecb-140">tooadd reverse DNS tooan existing PublicIpAddress that doesn't already have a DNS name, you must also specify a DNS name:</span></span>

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup -d contosoapp1 -f contosoapp1.westus.cloudapp.azure.com.
```

#### <a name="azure-cli-20"></a><span data-ttu-id="faecb-141">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="faecb-141">Azure CLI 2.0</span></span>

<span data-ttu-id="faecb-142">tooadd reverse existující PublicIpAddress tooan DNS:</span><span class="sxs-lookup"><span data-stu-id="faecb-142">tooadd reverse DNS tooan existing PublicIpAddress:</span></span>

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn contosoapp1.westus.cloudapp.azure.com.
```

<span data-ttu-id="faecb-143">tooadd zpětně DNS tooan existující PublicIpAddress, který ještě nemá název DNS, musíte taky zadat název DNS:</span><span class="sxs-lookup"><span data-stu-id="faecb-143">tooadd reverse DNS tooan existing PublicIpAddress that doesn't already have a DNS name, you must also specify a DNS name:</span></span>

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn contosoapp1.westus.cloudapp.azure.com --dns-name contosoapp1
```

### <a name="create-a-public-ip-address-with-reverse-dns"></a><span data-ttu-id="faecb-144">Vytvoření veřejné IP adresy s zpětné DNS</span><span class="sxs-lookup"><span data-stu-id="faecb-144">Create a Public IP Address with reverse DNS</span></span>

<span data-ttu-id="faecb-145">toocreate nové PublicIpAddress s hello zpětné již zadanou vlastnost DNS:</span><span class="sxs-lookup"><span data-stu-id="faecb-145">toocreate a new PublicIpAddress with hello reverse DNS property already specified:</span></span>

#### <a name="powershell"></a><span data-ttu-id="faecb-146">PowerShell</span><span class="sxs-lookup"><span data-stu-id="faecb-146">PowerShell</span></span>

```powershell
New-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup" -Location "WestUS" -AllocationMethod Dynamic -DomainNameLabel "contosoapp2" -ReverseFqdn "contosoapp2.westus.cloudapp.azure.com."
```

#### <a name="azure-cli-10"></a><span data-ttu-id="faecb-147">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="faecb-147">Azure CLI 1.0</span></span>

```azurecli
azure network public-ip create -n PublicIp -g MyResourceGroup -l westus -d contosoapp3 -f contosoapp3.westus.cloudapp.azure.com.
```

#### <a name="azure-cli-20"></a><span data-ttu-id="faecb-148">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="faecb-148">Azure CLI 2.0</span></span>

```azurecli
az network public-ip create --name PublicIp --resource-group MyResourceGroup --location westcentralus --dns-name contosoapp1 --reverse-fqdn contosoapp1.westcentralus.cloudapp.azure.com
```

### <a name="view-reverse-dns-for-an-existing-publicipaddress"></a><span data-ttu-id="faecb-149">Zobrazení zpětného DNS pro existující PublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="faecb-149">View reverse DNS for an existing PublicIpAddress</span></span>

<span data-ttu-id="faecb-150">pro existující PublicIpAddress tooview hello nakonfigurovat hodnotu:</span><span class="sxs-lookup"><span data-stu-id="faecb-150">tooview hello configured value for an existing PublicIpAddress:</span></span>

#### <a name="powershell"></a><span data-ttu-id="faecb-151">PowerShell</span><span class="sxs-lookup"><span data-stu-id="faecb-151">PowerShell</span></span>

```powershell
Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
```

#### <a name="azure-cli-10"></a><span data-ttu-id="faecb-152">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="faecb-152">Azure CLI 1.0</span></span>

```azurecli
azure network public-ip show -n PublicIp -g MyResourceGroup
```

#### <a name="azure-cli-20"></a><span data-ttu-id="faecb-153">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="faecb-153">Azure CLI 2.0</span></span>

```azurecli
az network public-ip show --name PublicIp --resource-group MyResourceGroup
```

### <a name="remove-reverse-dns-from-existing-public-ip-addresses"></a><span data-ttu-id="faecb-154">Odeberte zpětné DNS z existující veřejné IP adresy</span><span class="sxs-lookup"><span data-stu-id="faecb-154">Remove reverse DNS from existing Public IP Addresses</span></span>

<span data-ttu-id="faecb-155">tooremove zpětné DNS vlastnosti z existující PublicIpAddress:</span><span class="sxs-lookup"><span data-stu-id="faecb-155">tooremove a reverse DNS property from an existing PublicIpAddress:</span></span>

#### <a name="powershell"></a><span data-ttu-id="faecb-156">PowerShell</span><span class="sxs-lookup"><span data-stu-id="faecb-156">PowerShell</span></span>

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings.ReverseFqdn = ""
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

#### <a name="azure-cli-10"></a><span data-ttu-id="faecb-157">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="faecb-157">Azure CLI 1.0</span></span>

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup –f ""
```

#### <a name="azure-cli-20"></a><span data-ttu-id="faecb-158">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="faecb-158">Azure CLI 2.0</span></span>

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn ""
```


## <a name="configure-reverse-dns-for-cloud-services"></a><span data-ttu-id="faecb-159">Konfigurace zpětné DNS pro cloudové služby</span><span class="sxs-lookup"><span data-stu-id="faecb-159">Configure reverse DNS for Cloud Services</span></span>

<span data-ttu-id="faecb-160">Tato část obsahuje podrobné pokyny, jak tooconfigure reverse DNS pro cloudové služby v modelu nasazení Classic hello, pomocí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="faecb-160">This section provides detailed instructions for how tooconfigure reverse DNS for Cloud Services in hello Classic deployment model, using Azure PowerShell.</span></span> <span data-ttu-id="faecb-161">Konfigurace zpětné DNS pro cloudové služby není podporována prostřednictvím hello portál Azure, Azure CLI 1.0 nebo 2.0 rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="faecb-161">Configuring reverse DNS for Cloud Services is not supported via hello Azure portal, Azure CLI 1.0, or Azure CLI 2.0.</span></span>

### <a name="add-reverse-dns-tooexisting-cloud-services"></a><span data-ttu-id="faecb-162">Přidat zpětné DNS tooexisting cloudové služby</span><span class="sxs-lookup"><span data-stu-id="faecb-162">Add reverse DNS tooexisting Cloud Services</span></span>

<span data-ttu-id="faecb-163">tooadd tooan zpětné záznamů DNS existující cloudové služby:</span><span class="sxs-lookup"><span data-stu-id="faecb-163">tooadd a reverse DNS record tooan existing Cloud Service:</span></span>

```powershell
Set-AzureService –ServiceName "contosoapp1" –Description "App1 with Reverse DNS" –ReverseDnsFqdn "contosoapp1.cloudapp.net."
```

### <a name="create-a-cloud-service-with-reverse-dns"></a><span data-ttu-id="faecb-164">Vytvoření cloudové služby se zpětné DNS</span><span class="sxs-lookup"><span data-stu-id="faecb-164">Create a Cloud Service with reverse DNS</span></span>

<span data-ttu-id="faecb-165">toocreate novou Cloudovou službu se hello zpětné již zadanou vlastností DNS:</span><span class="sxs-lookup"><span data-stu-id="faecb-165">toocreate a new Cloud Service with hello reverse DNS property already specified:</span></span>

```powershell
New-AzureService –ServiceName "contosoapp1" –Location "West US" –Description "App1 with Reverse DNS" –ReverseDnsFqdn "contosoapp1.cloudapp.net."
```

### <a name="view-reverse-dns-for-existing-cloud-services"></a><span data-ttu-id="faecb-166">Zobrazení zpětného DNS pro existující cloudové služby</span><span class="sxs-lookup"><span data-stu-id="faecb-166">View reverse DNS for existing Cloud Services</span></span>

<span data-ttu-id="faecb-167">tooview hello reverse vlastnosti DNS pro stávající Cloudovou službu:</span><span class="sxs-lookup"><span data-stu-id="faecb-167">tooview hello reverse DNS property for an existing Cloud Service:</span></span>

```powershell
Get-AzureService "contosoapp1"
```

### <a name="remove-reverse-dns-from-existing-cloud-services"></a><span data-ttu-id="faecb-168">Odeberte zpětné DNS z existující cloudové služby</span><span class="sxs-lookup"><span data-stu-id="faecb-168">Remove reverse DNS from existing Cloud Services</span></span>

<span data-ttu-id="faecb-169">tooremove zpětné vlastnosti DNS z existující cloudové služby:</span><span class="sxs-lookup"><span data-stu-id="faecb-169">tooremove a reverse DNS property from an existing Cloud Service:</span></span>

```powershell
Set-AzureService –ServiceName "contosoapp1" –Description "App1 with Reverse DNS" –ReverseDnsFqdn ""
```

## <a name="faq"></a><span data-ttu-id="faecb-170">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="faecb-170">FAQ</span></span>

### <a name="how-much-do-reverse-dns-records-cost"></a><span data-ttu-id="faecb-171">Kolik reverse náklady na záznamy DNS?</span><span class="sxs-lookup"><span data-stu-id="faecb-171">How much do reverse DNS records cost?</span></span>

<span data-ttu-id="faecb-172">Jsou zdarma!</span><span class="sxs-lookup"><span data-stu-id="faecb-172">They're free!</span></span>  <span data-ttu-id="faecb-173">Není k dispozici bez dalších nákladů pro zpětné záznamy DNS nebo dotazy.</span><span class="sxs-lookup"><span data-stu-id="faecb-173">There is no additional cost for reverse DNS records or queries.</span></span>

### <a name="will-my-reverse-dns-records-resolve-from-hello-internet"></a><span data-ttu-id="faecb-174">Bude Moje zpětné vyřešte záznamy DNS z hello Internetu?</span><span class="sxs-lookup"><span data-stu-id="faecb-174">Will my reverse DNS records resolve from hello internet?</span></span>

<span data-ttu-id="faecb-175">Ano.</span><span class="sxs-lookup"><span data-stu-id="faecb-175">Yes.</span></span> <span data-ttu-id="faecb-176">Jakmile jednou nastavíte vlastnost DNS zpětného hello služby Azure, Azure spravuje všechny hello delegování DNS a zón DNS požadované přeloží tooensure, který reverse záznam DNS pro všechny uživatele na Internetu.</span><span class="sxs-lookup"><span data-stu-id="faecb-176">Once you set hello reverse DNS property for your Azure service, Azure manages all hello DNS delegations and DNS zones required tooensure that reverse DNS record resolves for all Internet users.</span></span>

### <a name="are-default-reverse-dns-records-created-for-my-azure-services"></a><span data-ttu-id="faecb-177">Vytvářejí výchozí záznamy zpětného vyhledávání DNS pro Moje služby Azure?</span><span class="sxs-lookup"><span data-stu-id="faecb-177">Are default reverse DNS records created for my Azure services?</span></span>

<span data-ttu-id="faecb-178">Ne.</span><span class="sxs-lookup"><span data-stu-id="faecb-178">No.</span></span> <span data-ttu-id="faecb-179">Zpětné DNS je funkce přihlášení.</span><span class="sxs-lookup"><span data-stu-id="faecb-179">Reverse DNS is an opt-in feature.</span></span> <span data-ttu-id="faecb-180">Zpětné záznamy DNS jsou vytvořen, pokud si zvolíte tooconfigure není žádné výchozí je.</span><span class="sxs-lookup"><span data-stu-id="faecb-180">No default reverse DNS records are created if you choose not tooconfigure them.</span></span>

### <a name="what-is-hello-format-for-hello-fully-qualified-domain-name-fqdn"></a><span data-ttu-id="faecb-181">Co je hello formát pro hello plně kvalifikovaný název domény (FQDN)?</span><span class="sxs-lookup"><span data-stu-id="faecb-181">What is hello format for hello fully-qualified domain name (FQDN)?</span></span>

<span data-ttu-id="faecb-182">Plně kvalifikované názvy domény jsou zadány popořadě a musí být ukončen tečkou (například "app1.contoso.com.").</span><span class="sxs-lookup"><span data-stu-id="faecb-182">FQDNs are specified in forward order, and must be terminated by a dot (for example, "app1.contoso.com.").</span></span>

### <a name="what-happens-if-hello-validation-check-for-hello-reverse-dns-ive-specified-fails"></a><span data-ttu-id="faecb-183">Co se stane, pokud ověření zkontrolujte hello hello reverse DNS I jste určili nezdaří?</span><span class="sxs-lookup"><span data-stu-id="faecb-183">What happens if hello validation check for hello reverse DNS I've specified fails?</span></span>

<span data-ttu-id="faecb-184">Kde hello zpětné DNS kontrolu ověření selže, selže hello operaci tooconfigure hello zpětné záznam DNS.</span><span class="sxs-lookup"><span data-stu-id="faecb-184">Where hello reverse DNS validation check fails, hello operation tooconfigure hello reverse DNS record fails.</span></span> <span data-ttu-id="faecb-185">Správné hello zpětné hodnotu DNS podle potřeby a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="faecb-185">Correct hello reverse DNS value as required, and retry.</span></span>

### <a name="can-i-configure-reverse-dns-for-azure-app-service"></a><span data-ttu-id="faecb-186">Můžete nakonfigurovat zpětné DNS pro službu Azure App Service?</span><span class="sxs-lookup"><span data-stu-id="faecb-186">Can I configure reverse DNS for Azure App Service?</span></span>

<span data-ttu-id="faecb-187">Ne.</span><span class="sxs-lookup"><span data-stu-id="faecb-187">No.</span></span> <span data-ttu-id="faecb-188">Zpětné DNS není podporována pro hello Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="faecb-188">Reverse DNS is not supported for hello Azure App Service.</span></span>

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-azure-service"></a><span data-ttu-id="faecb-189">Můžete nakonfigurovat více zpětné záznamy DNS pro Moje služba Azure?</span><span class="sxs-lookup"><span data-stu-id="faecb-189">Can I configure multiple reverse DNS records for my Azure service?</span></span>

<span data-ttu-id="faecb-190">Ne.</span><span class="sxs-lookup"><span data-stu-id="faecb-190">No.</span></span> <span data-ttu-id="faecb-191">Azure podporuje jeden zpětné záznam DNS pro každé cloudové služby Azure nebo PublicIpAddress.</span><span class="sxs-lookup"><span data-stu-id="faecb-191">Azure supports a single reverse DNS record for each Azure Cloud Service or PublicIpAddress.</span></span>

### <a name="can-i-configure-reverse-dns-for-ipv6-publicipaddress-resources"></a><span data-ttu-id="faecb-192">Můžete nakonfigurovat zpětné DNS pro prostředky IPv6 PublicIpAddress?</span><span class="sxs-lookup"><span data-stu-id="faecb-192">Can I configure reverse DNS for IPv6 PublicIpAddress resources?</span></span>

<span data-ttu-id="faecb-193">Ne.</span><span class="sxs-lookup"><span data-stu-id="faecb-193">No.</span></span> <span data-ttu-id="faecb-194">Azure aktuálně podporuje reverse DNS pouze pro prostředky IPv4 PublicIpAddress a cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="faecb-194">Azure currently supports reverse DNS only for IPv4 PublicIpAddress resources and Cloud Services.</span></span>

### <a name="can-i-send-emails-tooexternal-domains-from-my-azure-compute-services"></a><span data-ttu-id="faecb-195">Můžete odesílat e-mailů tooexternal domén z mé Azure výpočetní služby?</span><span class="sxs-lookup"><span data-stu-id="faecb-195">Can I send emails tooexternal domains from my Azure Compute services?</span></span>

<span data-ttu-id="faecb-196">Ne.</span><span class="sxs-lookup"><span data-stu-id="faecb-196">No.</span></span> [<span data-ttu-id="faecb-197">Azure výpočetní služby nepodporují odesílání e-mailů tooexternal domén</span><span class="sxs-lookup"><span data-stu-id="faecb-197">Azure Compute services do not support sending emails tooexternal domains</span></span>](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/)

## <a name="next-steps"></a><span data-ttu-id="faecb-198">Další kroky</span><span class="sxs-lookup"><span data-stu-id="faecb-198">Next steps</span></span>

<span data-ttu-id="faecb-199">Další informace o zpětné DNS najdete v tématu [zpětného vyhledávání DNS na webu Wikipedia](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).</span><span class="sxs-lookup"><span data-stu-id="faecb-199">For more information on reverse DNS, see [reverse DNS lookup on Wikipedia](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).</span></span>
<br>
<span data-ttu-id="faecb-200">Zjistěte, jak příliš[zóny zpětného vyhledávání hello hostitele pro rozsah vašeho poskytovatele internetových služeb přiřadit IP v Azure DNS](dns-reverse-dns-for-azure-services.md).</span><span class="sxs-lookup"><span data-stu-id="faecb-200">Learn how too[host hello reverse lookup zone for your ISP-assigned IP range in Azure DNS](dns-reverse-dns-for-azure-services.md).</span></span>

