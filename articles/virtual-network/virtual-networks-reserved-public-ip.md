---
title: "aaaManage Azure rezervovaných IP adres (klasické) – prostředí PowerShell | Microsoft Docs"
description: "Pochopení vyhrazené IP adresy (klasické) a jak toomanage je pomocí prostředí PowerShell."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: 34652a55-3ab8-4c2d-8fb2-43684033b191
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/10/2016
ms.author: jdial
ms.openlocfilehash: c0a77b2ab8b1ab9bef6015c903eb735ea4358a35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="reserved-ip-addresses-classic"></a><span data-ttu-id="33924-103">Vyhrazené IP adresy (klasické)</span><span class="sxs-lookup"><span data-stu-id="33924-103">Reserved IP addresses (Classic)</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="33924-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="33924-104">Azure portal</span></span>](virtual-network-deploy-static-pip-arm-portal.md)
> * [<span data-ttu-id="33924-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="33924-105">PowerShell</span></span>](virtual-network-deploy-static-pip-arm-ps.md)
> * [<span data-ttu-id="33924-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="33924-106">Azure CLI</span></span>](virtual-network-deploy-static-pip-arm-cli.md)
> * [<span data-ttu-id="33924-107">Šablona</span><span class="sxs-lookup"><span data-stu-id="33924-107">Template</span></span>](virtual-network-deploy-static-pip-arm-template.md)
> * [<span data-ttu-id="33924-108">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="33924-108">PowerShell (Classic)</span></span>](virtual-networks-reserved-public-ip.md)

<span data-ttu-id="33924-109">IP adresy v Azure lze rozdělit do dvou kategorií: dynamické a vyhrazené.</span><span class="sxs-lookup"><span data-stu-id="33924-109">IP addresses in Azure fall into two categories: dynamic and reserved.</span></span> <span data-ttu-id="33924-110">Veřejné IP adresy, které spravuje Azure je dynamická ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="33924-110">Public IP addresses managed by Azure are dynamic by default.</span></span> <span data-ttu-id="33924-111">Aby znamená, že hello IP adresy použít pro dané cloudové služby (VIP) nebo tooaccess virtuálního počítače nebo přímo instance role (splnění) můžete změnit čas tootime, když jsou prostředky vypnout nebo zastavena (deallocated).</span><span class="sxs-lookup"><span data-stu-id="33924-111">That means that hello IP address used for a given cloud service (VIP) or tooaccess a VM or role instance directly (ILPIP) can change from time tootime, when resources are shut down or stopped (deallocated).</span></span>

<span data-ttu-id="33924-112">IP adresy tooprevent změnu, je možné rezervovat IP adresu.</span><span class="sxs-lookup"><span data-stu-id="33924-112">tooprevent IP addresses from changing, you can reserve an IP address.</span></span> <span data-ttu-id="33924-113">Vyhrazené IP adresy lze použít pouze jako virtuální IP adresu, zajistit, že hello IP adresu pro hello Cloudová služba zůstane stejné, hello to i v případě prostředky jsou vypnout nebo zastavená (deallocated).</span><span class="sxs-lookup"><span data-stu-id="33924-113">Reserved IPs can be used only as a VIP, ensuring that hello IP address for hello cloud service remains hello same, even as resources are shut down or stopped (deallocated).</span></span> <span data-ttu-id="33924-114">Kromě toho můžete převést existující dynamické IP adresy použít jako VIP tooa vyhrazená IP adresa.</span><span class="sxs-lookup"><span data-stu-id="33924-114">Furthermore, you can convert existing dynamic IPs used as a VIP tooa reserved IP address.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="33924-115">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="33924-115">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="33924-116">Tento článek se zabývá pomocí modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="33924-116">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="33924-117">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="33924-117">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="33924-118">Zjistěte, jak tooreserve statickou veřejnou IP adresu pomocí hello [modelu nasazení Resource Manager](virtual-network-ip-addresses-overview-arm.md).</span><span class="sxs-lookup"><span data-stu-id="33924-118">Learn how tooreserve a static public IP address using hello [Resource Manager deployment model](virtual-network-ip-addresses-overview-arm.md).</span></span>

<span data-ttu-id="33924-119">toolearn Další informace o IP adresy v Azure, přečtěte si hello [IP adresy](virtual-network-ip-addresses-overview-classic.md) článku.</span><span class="sxs-lookup"><span data-stu-id="33924-119">toolearn more about IP addresses in Azure, read hello [IP addresses](virtual-network-ip-addresses-overview-classic.md) article.</span></span>

## <a name="when-do-i-need-a-reserved-ip"></a><span data-ttu-id="33924-120">Co je potřeba vyhrazená IP adresa?</span><span class="sxs-lookup"><span data-stu-id="33924-120">When do I need a reserved IP?</span></span>
* <span data-ttu-id="33924-121">**Chcete, aby v rámci vašeho předplatného je vyhrazena tooensure, který hello IP**.</span><span class="sxs-lookup"><span data-stu-id="33924-121">**You want tooensure that hello IP is reserved in your subscription**.</span></span> <span data-ttu-id="33924-122">Pokud chcete tooreserve IP adresu, která se neuvolní ze svého předplatného za žádných okolností, měli byste použít vyhrazený veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="33924-122">If you want tooreserve an IP address that is not released from your subscription under any circumstance, you should use a reserved public IP.</span></span>  
* <span data-ttu-id="33924-123">**Chcete vaší IP toostay s cloudovou službou i napříč zastavena nebo navrácena stavu (VM)**.</span><span class="sxs-lookup"><span data-stu-id="33924-123">**You want your IP toostay with your cloud service even across stopped or deallocated state (VMs)**.</span></span> <span data-ttu-id="33924-124">Pokud chcete, aby vaše služba toobe přistupovat pomocí IP adresu, která se nezmění, i v případě, že virtuální počítače v hello cloudové služby jsou vypnout nebo zastavit (deallocated).</span><span class="sxs-lookup"><span data-stu-id="33924-124">If you want your service toobe accessed by using an IP address that doesn't change, even when VMs in hello cloud service are shut down or stop (deallocated).</span></span>
* <span data-ttu-id="33924-125">**Chcete-li tooensure, odchozí provoz z Azure používá předvídatelný IP adresu**.</span><span class="sxs-lookup"><span data-stu-id="33924-125">**You want tooensure that outbound traffic from Azure uses a predictable IP address**.</span></span> <span data-ttu-id="33924-126">Můžete mít místní brány firewall nakonfigurované tooallow pouze provozu z konkrétní IP adresy.</span><span class="sxs-lookup"><span data-stu-id="33924-126">You may have your on-premises firewall configured tooallow only traffic from specific IP addresses.</span></span> <span data-ttu-id="33924-127">Vyhrazením IP adresy, víte hello zdrojovou IP adres a nepotřebujete tooupdate pravidla brány firewall kvůli změně IP tooan.</span><span class="sxs-lookup"><span data-stu-id="33924-127">By reserving an IP, you know hello source IP address, and don't need tooupdate your firewall rules due tooan IP change.</span></span>

## <a name="faq"></a><span data-ttu-id="33924-128">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="33924-128">FAQ</span></span>
1. <span data-ttu-id="33924-129">Můžete použít vyhrazenou IP adresu pro všechny služby Azure?</span><span class="sxs-lookup"><span data-stu-id="33924-129">Can I use a reserved IP for all Azure services?</span></span> <br>
    <span data-ttu-id="33924-130">Ne.</span><span class="sxs-lookup"><span data-stu-id="33924-130">No.</span></span> <span data-ttu-id="33924-131">Vyhrazené IP adresy lze použít pouze pro virtuální počítače a instance role cloudové služby vystavenou přes virtuální IP adresu.</span><span class="sxs-lookup"><span data-stu-id="33924-131">Reserved IPs can only be used for VMs and cloud service instance roles exposed through a VIP.</span></span>
2. <span data-ttu-id="33924-132">Kolik vyhrazené IP adresy, které může mít</span><span class="sxs-lookup"><span data-stu-id="33924-132">How many reserved IPs can I have?</span></span> <br>
    <span data-ttu-id="33924-133">Podrobnosti najdete v tématu hello [Azure omezuje](../azure-subscription-service-limits.md#networking-limits) článku.</span><span class="sxs-lookup"><span data-stu-id="33924-133">For details, see hello [Azure limits](../azure-subscription-service-limits.md#networking-limits) article.</span></span>
3. <span data-ttu-id="33924-134">Je pro vyhrazené IP adresy zdarma?</span><span class="sxs-lookup"><span data-stu-id="33924-134">Is there a charge for reserved IPs?</span></span> <br>
    <span data-ttu-id="33924-135">V některých případech.</span><span class="sxs-lookup"><span data-stu-id="33924-135">Sometimes.</span></span> <span data-ttu-id="33924-136">Podrobnosti o cenách najdete v části hello [vyhrazené IP adresy podrobnosti o cenách](http://go.microsoft.com/fwlink/?LinkID=398482) stránky.</span><span class="sxs-lookup"><span data-stu-id="33924-136">For pricing details, see hello [Reserved IP Address Pricing Details](http://go.microsoft.com/fwlink/?LinkID=398482) page.</span></span>
4. <span data-ttu-id="33924-137">Jak rezervovat IP adresu?</span><span class="sxs-lookup"><span data-stu-id="33924-137">How do I reserve an IP address?</span></span> <br>
    <span data-ttu-id="33924-138">Můžete použít PowerShell, hello [REST API pro správu Azure](https://msdn.microsoft.com/library/azure/dn722420.aspx), nebo hello [portál Azure](https://portal.azure.com) tooreserve IP adresu v oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="33924-138">You can use PowerShell, hello [Azure Management REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx), or hello [Azure portal](https://portal.azure.com) tooreserve an IP address in an Azure region.</span></span> <span data-ttu-id="33924-139">Vyhrazená IP adresa je přidružená tooyour předplatné.</span><span class="sxs-lookup"><span data-stu-id="33924-139">A reserved IP address is associated tooyour subscription.</span></span>
5. <span data-ttu-id="33924-140">Můžete použít vyhrazenou IP adresu na základě skupiny vztahů sítě vnet?</span><span class="sxs-lookup"><span data-stu-id="33924-140">Can I use a reserved IP with affinity group-based VNets?</span></span> <br>
    <span data-ttu-id="33924-141">Ne.</span><span class="sxs-lookup"><span data-stu-id="33924-141">No.</span></span> <span data-ttu-id="33924-142">Vyhrazené IP adresy jsou podporovány pouze v regionální virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="33924-142">Reserved IPs are only supported in regional VNets.</span></span> <span data-ttu-id="33924-143">Vyhrazené IP adresy nejsou podporovány pro virtuální sítě, které jsou přidruženy skupiny vztahů.</span><span class="sxs-lookup"><span data-stu-id="33924-143">Reserved IPs are not supported for VNets that are associated with affinity groups.</span></span> <span data-ttu-id="33924-144">Další informace o přiřazení virtuální síť s oblast nebo skupinu vztahů, najdete v části hello [o virtuální místní sítě a skupiny vztahů](virtual-networks-migrate-to-regional-vnet.md) článku.</span><span class="sxs-lookup"><span data-stu-id="33924-144">For more information about associating a VNet with a region or affinity group, see hello [About Regional VNets and Affinity Groups](virtual-networks-migrate-to-regional-vnet.md) article.</span></span>

## <a name="manage-reserved-vips"></a><span data-ttu-id="33924-145">Spravovat vyhrazené virtuální IP adresy</span><span class="sxs-lookup"><span data-stu-id="33924-145">Manage reserved VIPs</span></span>

<span data-ttu-id="33924-146">Ujistěte se, je nainstalován a nakonfigurován prostředí PowerShell pomocí kroků hello v hello [instalace a konfigurace prostředí PowerShell](/powershell/azure/overview) článku.</span><span class="sxs-lookup"><span data-stu-id="33924-146">Ensure you have installed and configured PowerShell by completing hello steps in hello [Install and configure PowerShell](/powershell/azure/overview) article.</span></span> 

<span data-ttu-id="33924-147">Před použitím vyhrazené IP adresy, je třeba přidat ji tooyour předplatné.</span><span class="sxs-lookup"><span data-stu-id="33924-147">Before you can use reserved IPs, you must add it tooyour subscription.</span></span> <span data-ttu-id="33924-148">toocreate vyhrazenou IP adresu z fondu hello z veřejných IP adres k dispozici v hello *střed USA* umístění, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="33924-148">toocreate a reserved IP from hello pool of public IP addresses available in hello *Central US* location, run hello following command:</span></span>

```powershell
New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"
```

<span data-ttu-id="33924-149">Všimněte si, ale není možné zadat co IP je vyhrazena.</span><span class="sxs-lookup"><span data-stu-id="33924-149">Notice, however, that you cannot specify what IP is being reserved.</span></span> <span data-ttu-id="33924-150">tooview jaké IP adresy jsou vyhrazené v rámci vašeho předplatného, spusťte následující příkaz prostředí PowerShell, hello a Všimněte si hello hodnoty pro *ReservedIPName* a *adresu*:</span><span class="sxs-lookup"><span data-stu-id="33924-150">tooview what IP addresses are reserved in your subscription, run hello following PowerShell command, and notice hello values for *ReservedIPName* and *Address*:</span></span>

```powershell
Get-AzureReservedIP
```

<span data-ttu-id="33924-151">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="33924-151">Expected output:</span></span>

    ReservedIPName       : MyReservedIP
    Address              : 23.101.114.211
    Id                   : d73be9dd-db12-4b5e-98c8-bc62e7c42041
    Label                :
    Location             : Central US
    State                : Created
    InUse                : False
    ServiceName          :
    DeploymentName       :
    OperationDescription : Get-AzureReservedIP
    OperationId          : 55e4f245-82e4-9c66-9bd8-273e815ce30a
    OperationStatus      : Succeeded

>[!NOTE]
><span data-ttu-id="33924-152">Když vytvoříte vyhrazenou IP adresu pomocí prostředí PowerShell, nelze zadat prostředku skupiny toocreate hello vyhrazené IP v.</span><span class="sxs-lookup"><span data-stu-id="33924-152">When you create a reserved IP address with PowerShell, you cannot specify a resource group toocreate hello reserved IP in.</span></span> <span data-ttu-id="33924-153">Azure místech do skupiny prostředků s názvem *výchozí sítě* automaticky.</span><span class="sxs-lookup"><span data-stu-id="33924-153">Azure places it into a resource group named *Default-Networking* automatically.</span></span> <span data-ttu-id="33924-154">Pokud vytvoříte hello vyhrazené IP pomocí hello [portál Azure](http://portal.azure.com), můžete zadat všechny skupiny prostředků, které zvolíte.</span><span class="sxs-lookup"><span data-stu-id="33924-154">If you create hello reserved IP using hello [Azure portal](http://portal.azure.com), you can specify any resource group you choose.</span></span> <span data-ttu-id="33924-155">Pokud vytvoříte hello vyhrazené IP ve skupině prostředků než *výchozí sítě* však vždy, když odkazujete hello vyhrazené IP adresy s příkazy, jako `Get-AzureReservedIP` a `Remove-AzureReservedIP`, musí odkazovat hello název *Vyhrazený název skupiny prostředků ip název skupiny*.</span><span class="sxs-lookup"><span data-stu-id="33924-155">If you create hello reserved IP in a resource group other than *Default-Networking* however, whenever you reference hello reserved IP with commands such as `Get-AzureReservedIP` and `Remove-AzureReservedIP`, you must reference hello name *Group resource-group-name reserved-ip-name*.</span></span>  <span data-ttu-id="33924-156">Například pokud vytvoříte vyhrazená IP adresa s názvem *myReservedIP* ve skupině prostředků s názvem *myResourceGroup*, musí odkazovat hello název hello vyhrazená IP adresa jako *myResourceGroup skupiny myReservedIP*.</span><span class="sxs-lookup"><span data-stu-id="33924-156">For example, if you create a reserved IP named *myReservedIP* in a resource group named *myResourceGroup*, you must reference hello name of hello reserved IP as *Group myResourceGroup myReservedIP*.</span></span>   

<span data-ttu-id="33924-157">Jakmile je vyhrazené IP adresy, zůstane předplatné přidružené tooyour, odstraňte jej.</span><span class="sxs-lookup"><span data-stu-id="33924-157">Once an IP is reserved, it remains associated tooyour subscription until you delete it.</span></span> <span data-ttu-id="33924-158">toodelete vyhrazená IP adresa, spusťte hello následující příkaz prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="33924-158">toodelete a reserved IP, run hello following PowerShell command:</span></span>

```powershell
Remove-AzureReservedIP -ReservedIPName "MyReservedIP"
```

## <a name="reserve-hello-ip-address-of-an-existing-cloud-service"></a><span data-ttu-id="33924-159">Rezervujte hello IP adresu existující cloudové služby</span><span class="sxs-lookup"><span data-stu-id="33924-159">Reserve hello IP address of an existing cloud service</span></span>
<span data-ttu-id="33924-160">IP adresa hello stávající cloudovou službu můžete vyhradit přidáním hello `-ServiceName` parametr.</span><span class="sxs-lookup"><span data-stu-id="33924-160">You can reserve hello IP address of an existing cloud service by adding hello `-ServiceName` parameter.</span></span> <span data-ttu-id="33924-161">IP adresa hello tooreserve cloudové služby *TestService* v hello *střed USA* umístění, spusťte následující příkaz prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="33924-161">tooreserve hello IP address of a cloud service *TestService* in hello *Central US* location, run hello following PowerShell command:</span></span>

```powershell
New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US" -ServiceName TestService
```

## <a name="associate-a-reserved-ip-tooa-new-cloud-service"></a><span data-ttu-id="33924-162">Přidružení vyhrazené IP tooa novou cloudovou službu</span><span class="sxs-lookup"><span data-stu-id="33924-162">Associate a reserved IP tooa new cloud service</span></span>
<span data-ttu-id="33924-163">Hello následující skript vytvoří nové vyhrazená IP adresa a přidruží ji tooa novou cloudovou službu s názvem *TestService*.</span><span class="sxs-lookup"><span data-stu-id="33924-163">hello following script creates a new reserved IP, then associates it tooa new cloud service named *TestService*.</span></span>

```powershell
New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"

$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}

New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| New-AzureVM -ServiceName TestService -ReservedIPName MyReservedIP -Location "Central US"
```

> [!NOTE]
> <span data-ttu-id="33924-164">Když vytvoříte vyhrazené IP toouse ke cloudové službě, je stále odkazovat toohello virtuálních počítačů pomocí *VIP:&lt;číslo portu >* pro příchozí komunikaci.</span><span class="sxs-lookup"><span data-stu-id="33924-164">When you create a reserved IP toouse with a cloud service, you still refer toohello VM by using *VIP:&lt;port number>* for inbound communication.</span></span> <span data-ttu-id="33924-165">Rezervace IP neznamená, že toohello virtuální počítač může připojit přímo.</span><span class="sxs-lookup"><span data-stu-id="33924-165">Reserving an IP does not mean you can connect toohello VM directly.</span></span> <span data-ttu-id="33924-166">Hello vyhrazená IP adresa je přiřazen toohello cloudu tohoto hello virtuálního počítače byla nasazena do služby.</span><span class="sxs-lookup"><span data-stu-id="33924-166">hello reserved IP is assigned toohello cloud service that hello VM has been deployed to.</span></span> <span data-ttu-id="33924-167">Pokud chcete tooconnect tooa virtuální počítač podle IP přímo, máte tooconfigure úrovni instance veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="33924-167">If you want tooconnect tooa VM by IP directly, you have tooconfigure an instance-level public IP.</span></span> <span data-ttu-id="33924-168">Veřejná IP adresa úrovni instance je typ veřejné IP (nazývané splnění), který je přiřazen přímo tooyour virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="33924-168">An instance-level public IP is a type of public IP (called an ILPIP) that is assigned directly tooyour VM.</span></span> <span data-ttu-id="33924-169">Nelze rezervovat.</span><span class="sxs-lookup"><span data-stu-id="33924-169">It cannot be reserved.</span></span> <span data-ttu-id="33924-170">Další informace najdete v tématu hello [úrovni Instance veřejné IP splnění](virtual-networks-instance-level-public-ip.md) článku.</span><span class="sxs-lookup"><span data-stu-id="33924-170">For more information, read hello [Instance-level Public IP (ILPIP)](virtual-networks-instance-level-public-ip.md) article.</span></span>
> 

## <a name="remove-a-reserved-ip-from-a-running-deployment"></a><span data-ttu-id="33924-171">Odebrání spuštěného nasazení vyhrazená IP adresa</span><span class="sxs-lookup"><span data-stu-id="33924-171">Remove a reserved IP from a running deployment</span></span>
<span data-ttu-id="33924-172">tooremove vyhrazená IP adresa byla přidána tooa novou cloudovou službu, spusťte následující příkaz prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="33924-172">tooremove a reserved IP added tooa new cloud service, run hello following PowerShell command:</span></span>

```powershell
Remove-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService
```

> [!NOTE]
> <span data-ttu-id="33924-173">Odebrání vyhrazenou IP adresu z spuštěného nasazení neodebere hello rezervaci ze svého předplatného.</span><span class="sxs-lookup"><span data-stu-id="33924-173">Removing a reserved IP from a running deployment does not remove hello reservation from your subscription.</span></span> <span data-ttu-id="33924-174">Uvolní jednoduše toobe IP hello používán jiným prostředkem v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="33924-174">It simply frees hello IP toobe used by another resource in your subscription.</span></span>
> 

## <a name="associate-a-reserved-ip-tooa-running-deployment"></a><span data-ttu-id="33924-175">Přidružení vyhrazené IP tooa běží nasazení</span><span class="sxs-lookup"><span data-stu-id="33924-175">Associate a reserved IP tooa running deployment</span></span>
<span data-ttu-id="33924-176">Hello následující příkazy vytvoření cloudové služby s názvem *TestService2* s nový virtuální počítač s názvem *TestVM2*.</span><span class="sxs-lookup"><span data-stu-id="33924-176">hello following commands create a cloud service named *TestService2* with a new VM named *TestVM2*.</span></span> <span data-ttu-id="33924-177">Hello existující vyhrazené IP adresy s názvem *MyReservedIP* pak přidružené toohello Cloudová služba.</span><span class="sxs-lookup"><span data-stu-id="33924-177">hello existing reserved IP named *MyReservedIP* is then associated toohello cloud service.</span></span>

```powershell
$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}

New-AzureVMConfig -Name TestVM2 -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| New-AzureVM -ServiceName TestService2 -Location "Central US"

Set-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService2
```

## <a name="associate-a-reserved-ip-tooa-cloud-service-by-using-a-service-configuration-file"></a><span data-ttu-id="33924-178">Pomocí konfigurační soubor služby přidružení vyhrazené IP tooa cloudové služby</span><span class="sxs-lookup"><span data-stu-id="33924-178">Associate a reserved IP tooa cloud service by using a service configuration file</span></span>
<span data-ttu-id="33924-179">Pomocí souboru konfigurace (CSCFG) služby lze přiřadit vyhrazené IP tooa cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="33924-179">You can also associate a reserved IP tooa cloud service by using a service configuration (CSCFG) file.</span></span> <span data-ttu-id="33924-180">Hello následující ukázkový kód xml ukazuje, jak tooconfigure cloudové služby toouse vyhrazené virtuální IP adresy s názvem *MyReservedIP*:</span><span class="sxs-lookup"><span data-stu-id="33924-180">hello following sample xml shows how tooconfigure a cloud service toouse a reserved VIP named *MyReservedIP*:</span></span>

    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ReservedIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
        <ConfigurationSettings>
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
        </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <AddressAssignments>
          <ReservedIPs>
           <ReservedIP name="MyReservedIP"/>
          </ReservedIPs>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>

## <a name="next-steps"></a><span data-ttu-id="33924-181">Další kroky</span><span class="sxs-lookup"><span data-stu-id="33924-181">Next steps</span></span>
* <span data-ttu-id="33924-182">Pochopit, jak [IP adresování](virtual-network-ip-addresses-overview-classic.md) funguje v modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="33924-182">Understand how [IP addressing](virtual-network-ip-addresses-overview-classic.md) works in hello classic deployment model.</span></span>
* <span data-ttu-id="33924-183">Další informace o [vyhrazené soukromé IP adresy](virtual-networks-reserved-private-ip.md).</span><span class="sxs-lookup"><span data-stu-id="33924-183">Learn about [reserved private IP addresses](virtual-networks-reserved-private-ip.md).</span></span>
* <span data-ttu-id="33924-184">Další informace o [Instance úroveň veřejné IP splnění adresy](virtual-networks-instance-level-public-ip.md).</span><span class="sxs-lookup"><span data-stu-id="33924-184">Learn about [Instance Level Public IP (ILPIP) addresses](virtual-networks-instance-level-public-ip.md).</span></span>

