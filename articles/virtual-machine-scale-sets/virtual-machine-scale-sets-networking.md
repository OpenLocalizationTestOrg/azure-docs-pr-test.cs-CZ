---
title: "aaaNetworking pro sady škálování virtuálního počítače Azure | Microsoft Docs"
description: "Konfigurace síťových vlastností pro škálovací sady virtuálních počítačů Azure."
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: guybo
ms.openlocfilehash: ef3f0cfe648d2195c051a73987e654f0e15d13bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="networking-for-azure-virtual-machine-scale-sets"></a><span data-ttu-id="4517b-103">Síťové služby pro škálovací sady virtuálních počítačů Azure</span><span class="sxs-lookup"><span data-stu-id="4517b-103">Networking for Azure virtual machine scale sets</span></span>

<span data-ttu-id="4517b-104">Při nasazení sad prostřednictvím portálu hello škálování virtuálního počítače Azure, jsou uvedena určité vlastnosti sítě, například Vyrovnávání zatížení Azure příchozí pravidla NAT.</span><span class="sxs-lookup"><span data-stu-id="4517b-104">When you deploy an Azure virtual machine scale set through hello portal, certain network properties are defaulted, for example an Azure Load Balancer with inbound NAT rules.</span></span> <span data-ttu-id="4517b-105">Tento článek popisuje, jak nastaví toouse některé hello pokročilejší síťových funkcí, které můžete nakonfigurovat s měřítkem.</span><span class="sxs-lookup"><span data-stu-id="4517b-105">This article describes how toouse some of hello more advanced networking features that you can configure with scale sets.</span></span>

<span data-ttu-id="4517b-106">Můžete nakonfigurovat všechny funkce hello popsaná v tomto článku pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="4517b-106">You can configure all of hello features covered in this article using Azure Resource Manager templates.</span></span> <span data-ttu-id="4517b-107">Pro vybrané funkce jsou zahrnuté také příklady Azure CLI a PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="4517b-107">Azure CLI and PowerShell examples are also included for selected features.</span></span> <span data-ttu-id="4517b-108">Použijte CLI 2.10 a PowerShell 4.2.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="4517b-108">Use CLI 2.10, and PowerShell 4.2.0 or later.</span></span>

## <a name="accelerated-networking"></a><span data-ttu-id="4517b-109">Akcelerované síťové služby</span><span class="sxs-lookup"><span data-stu-id="4517b-109">Accelerated Networking</span></span>
<span data-ttu-id="4517b-110">Azure [Accelerated sítě](../virtual-network/virtual-network-create-vm-accelerated-networking.md) zlepšuje výkon sítě povolením jeden kořenový vstupně-výstupních operací virtualizace (SR-IOV) tooa virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4517b-110">Azure [Accelerated Networking](../virtual-network/virtual-network-create-vm-accelerated-networking.md) improves  network performance by enabling single root I/O virtualization (SR-IOV) tooa virtual machine.</span></span> <span data-ttu-id="4517b-111">toouse accelerated práce se sítěmi pomocí sady škálování, nastavte enableAcceleratedNetworking příliš**true** v sadě škálování Networkinterfaceconfiguration nastavení.</span><span class="sxs-lookup"><span data-stu-id="4517b-111">toouse accelerated networking with scale sets, set enableAcceleratedNetworking too**true** in your scale set's networkInterfaceConfigurations settings.</span></span> <span data-ttu-id="4517b-112">Například:</span><span class="sxs-lookup"><span data-stu-id="4517b-112">For example:</span></span>
```json
"networkProfile": {
    "networkInterfaceConfigurations": [
    {
      "name": "niconfig1",
      "properties": {
        "primary": true,
        "enableAcceleratedNetworking" : true,
        "ipConfigurations": [
          ...
        ]
      }
    }
   ]
}
```

## <a name="create-a-scale-set-that-references-an-existing-azure-load-balancer"></a><span data-ttu-id="4517b-113">Vytvoření škálovací sady odkazující na existující Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="4517b-113">Create a scale set that references an existing Azure Load Balancer</span></span>
<span data-ttu-id="4517b-114">Když sadu škálování je vytvořený pomocí hello portálu Azure, je většina možností konfigurace vytvoří nové služby Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="4517b-114">When a scale set is created using hello Azure portal, a new load balancer is created for most configuration options.</span></span> <span data-ttu-id="4517b-115">Pokud vytvoříte sadu škálování, který se musí tooreference existující pro vyrovnávání zatížení, můžete k tomu pomocí rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="4517b-115">If you create a scale set, which needs tooreference an existing load balancer, you can do this using CLI.</span></span> <span data-ttu-id="4517b-116">Následující ukázkový skript Hello vytváří nástroj pro vyrovnávání zatížení a potom vytvoří sada škálování, které na ni odkazuje:</span><span class="sxs-lookup"><span data-stu-id="4517b-116">hello following example script creates a load balancer and then creates a scale set, which references it:</span></span>
```bash
az network lb create -g lbtest -n mylb --vnet-name myvnet --subnet mysubnet --public-ip-address-allocation Static --backend-pool-name mybackendpool

az vmss create -g lbtest -n myvmss --image Canonical:UbuntuServer:16.04-LTS:latest --admin-username negat --ssh-key-value /home/myuser/.ssh/id_rsa.pub --upgrade-policy-mode Automatic --instance-count 3 --vnet-name myvnet --subnet mysubnet --lb mylb --backend-pool-name mybackendpool

```

## <a name="configurable-dns-settings"></a><span data-ttu-id="4517b-117">Konfigurovatelná nastavení DNS</span><span class="sxs-lookup"><span data-stu-id="4517b-117">Configurable DNS Settings</span></span>
<span data-ttu-id="4517b-118">Ve výchozím nastavení na konkrétní nastavení DNS hello hello virtuální síť a podsíť, které byly vytvořeny v trvat sady škálování.</span><span class="sxs-lookup"><span data-stu-id="4517b-118">By default, scale sets take on hello specific DNS settings of hello VNET and subnet they were created in.</span></span> <span data-ttu-id="4517b-119">Ale můžete nakonfigurovat nastavení DNS hello horizontálního nastavovat přímo.</span><span class="sxs-lookup"><span data-stu-id="4517b-119">You can however, configure hello DNS settings for a scale set directly.</span></span>
~
### <a name="creating-a-scale-set-with-configurable-dns-servers"></a><span data-ttu-id="4517b-120">Vytvoření škálovací sady s konfigurovatelnými servery DNS</span><span class="sxs-lookup"><span data-stu-id="4517b-120">Creating a scale set with configurable DNS servers</span></span>
<span data-ttu-id="4517b-121">toocreate škálování nastavit pomocí vlastní konfigurace DNS pomocí rozhraní příkazového řádku 2.0, přidejte hello **servery – dns** argument toohello **vmss vytvořit** příkazu, za nímž následuje místo oddělené ip adres serveru.</span><span class="sxs-lookup"><span data-stu-id="4517b-121">toocreate a scale set with a custom DNS configuration using CLI 2.0, add hello **--dns-servers** argument toohello **vmss create** command, followed by space separated server ip addresses.</span></span> <span data-ttu-id="4517b-122">Například:</span><span class="sxs-lookup"><span data-stu-id="4517b-122">For example:</span></span>
```bash
--dns-servers 10.0.0.6 10.0.0.5
```
<span data-ttu-id="4517b-123">tooconfigure vlastní servery DNS v šablony Azure, přidejte škálování dnsSettings vlastnost toohello nastavení Networkinterfaceconfiguration sekce.</span><span class="sxs-lookup"><span data-stu-id="4517b-123">tooconfigure custom DNS servers in an Azure template, add a dnsSettings property toohello scale set networkInterfaceConfigurations section.</span></span> <span data-ttu-id="4517b-124">Například:</span><span class="sxs-lookup"><span data-stu-id="4517b-124">For example:</span></span>
```json
"dnsSettings":{
    "dnsServers":["10.0.0.6", "10.0.0.5"]
}
```

### <a name="creating-a-scale-set-with-configurable-virtual-machine-domain-names"></a><span data-ttu-id="4517b-125">Vytvoření škálovací sady s konfigurovatelnými názvy domén virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="4517b-125">Creating a scale set with configurable virtual machine domain names</span></span>
<span data-ttu-id="4517b-126">toocreate škálování nastavit vlastní název DNS pro virtuální počítače pomocí rozhraní příkazového řádku 2.0, přidejte hello **název domény virtuálního počítače –** argument toohello **vmss vytvořit** příkazu, za nímž následuje řetězec představující název domény hello.</span><span class="sxs-lookup"><span data-stu-id="4517b-126">toocreate a scale set with a custom DNS name for virtual machines using CLI 2.0, add hello **--vm-domain-name** argument toohello **vmss create** command, followed by a string representing hello domain name.</span></span>

<span data-ttu-id="4517b-127">Přidejte název domény hello tooset šablony Azure, **dnsSettings** vlastnost toohello škálovací sadu **Networkinterfaceconfiguration** části.</span><span class="sxs-lookup"><span data-stu-id="4517b-127">tooset hello domain name in an Azure template, add a **dnsSettings** property toohello scale set **networkInterfaceConfigurations** section.</span></span> <span data-ttu-id="4517b-128">Například:</span><span class="sxs-lookup"><span data-stu-id="4517b-128">For example:</span></span>

```json
"networkProfile": {
  "networkInterfaceConfigurations": [
    {
    "name": "nic1",
    "properties": {
      "primary": "true",
      "ipConfigurations": [
      {
        "name": "ip1",
        "properties": {
          "subnet": {
            "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
          },
          "publicIPAddressconfiguration": {
            "name": "publicip",
            "properties": {
            "idleTimeoutInMinutes": 10,
              "dnsSettings": {
                "domainNameLabel": "[parameters('vmssDnsName')]"
              }
            }
          }
        }
      }
    ]
    }
}
```

<span data-ttu-id="4517b-129">výstup Hello pro virtuální počítač zvlášť název dns by byl v hello následující formulář:</span><span class="sxs-lookup"><span data-stu-id="4517b-129">hello output, for an individual virtual machine dns name would be in hello following form:</span></span> 
```
<vm><vmindex>.<specifiedVmssDomainNameLabel>
```

## <a name="public-ipv4-per-virtual-machine"></a><span data-ttu-id="4517b-130">Veřejná IPv4 adresa na virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="4517b-130">Public IPv4 per virtual machine</span></span>
<span data-ttu-id="4517b-131">Obecně platí, že virtuální počítače Azure ve škálovací sadě nevyžadují vlastní veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="4517b-131">In general, Azure scale set virtual machines do not require their own public IP addresses.</span></span> <span data-ttu-id="4517b-132">Pro většinu scénářů je tooassociate veřejnou IP adresu tooa zatížení vyrovnávání nebo tooan jednotlivé virtuální počítač (neboli jumpbox), který pak směruje příchozí připojení tooscale sada virtuálních počítačů podle potřeby (například prostřednictvím více ekonomické a zabezpečení Příchozí pravidla NAT).</span><span class="sxs-lookup"><span data-stu-id="4517b-132">For most scenarios, it is more economical and secure tooassociate a public IP address tooa load balancer or tooan individual virtual machine (aka a jumpbox), which then routes incoming connections tooscale set virtual machines as needed (for example, through inbound NAT rules).</span></span>

<span data-ttu-id="4517b-133">Ale některé scénáře vyžadují sady škálování virtuálních počítačů toohave svá vlastní veřejná IP adresy.</span><span class="sxs-lookup"><span data-stu-id="4517b-133">However, some scenarios do require scale set virtual machines toohave their own public IP addresses.</span></span> <span data-ttu-id="4517b-134">Hry je příklad, kdy konzoly je toomake přímé připojení tooa cloudu virtuálního počítače, který provádí herní fyziky zpracování.</span><span class="sxs-lookup"><span data-stu-id="4517b-134">An example is gaming, where a console needs toomake a direct connection tooa cloud virtual machine, which is doing game physics processing.</span></span> <span data-ttu-id="4517b-135">Dalším příkladem je, kde virtuální počítače musí toomake externí připojení tooone jiné v oblastech v distribuované databáze.</span><span class="sxs-lookup"><span data-stu-id="4517b-135">Another example is where virtual machines need toomake external connections tooone another across regions in a distributed database.</span></span>

### <a name="creating-a-scale-set-with-public-ip-per-virtual-machine"></a><span data-ttu-id="4517b-136">Vytvoření škálovací sady s veřejnou IP adresou na virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="4517b-136">Creating a scale set with public IP per virtual machine</span></span>
<span data-ttu-id="4517b-137">toocreate sadu škálování, který přiřazuje veřejnou IP adresu tooeach virtuální počítač s 2.0 rozhraní příkazového řádku, přidejte hello **– veřejné ip na počítač** parametr toohello **vmss vytvořit** příkaz.</span><span class="sxs-lookup"><span data-stu-id="4517b-137">toocreate a scale set that assigns a public IP address tooeach virtual machine with CLI 2.0, add hello **--public-ip-per-vm** parameter toohello **vmss create** command.</span></span> 

<span data-ttu-id="4517b-138">toocreate škálování nastavit pomocí šablony Azure, zkontrolujte, zda hello rozhraní API verze hello Microsoft.Compute/virtualMachineScaleSets prostředků je alespoň **2017-03-30**a přidejte **publicIpAddressConfiguration**JSON vlastnost toohello škálovací sady část konfigurace IP adresy.</span><span class="sxs-lookup"><span data-stu-id="4517b-138">toocreate a scale set using an Azure template, make sure hello API version of hello Microsoft.Compute/virtualMachineScaleSets resource is at least **2017-03-30**, and add a **publicIpAddressConfiguration** JSON property toohello scale set ipConfigurations section.</span></span> <span data-ttu-id="4517b-139">Například:</span><span class="sxs-lookup"><span data-stu-id="4517b-139">For example:</span></span>

```json
"publicIpAddressConfiguration": {
    "name": "pub1",
    "properties": {
      "idleTimeoutInMinutes": 15
    }
}
```
<span data-ttu-id="4517b-140">Ukázková šablona: [201-vmss-public-ip-linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-public-ip-linux)</span><span class="sxs-lookup"><span data-stu-id="4517b-140">Example template: [201-vmss-public-ip-linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-public-ip-linux)</span></span>

### <a name="querying-hello-public-ip-addresses-of-hello-virtual-machines-in-a-scale-set"></a><span data-ttu-id="4517b-141">Dotazu hello veřejné IP adresy hello virtuálních počítačů v škálování sadu</span><span class="sxs-lookup"><span data-stu-id="4517b-141">Querying hello public IP addresses of hello virtual machines in a scale set</span></span>
<span data-ttu-id="4517b-142">toolist hello veřejné IP adresy přiřazené tooscale sada virtuálních počítačů pomocí rozhraní příkazového řádku 2.0, použijte hello **az vmss seznamu instance veřejný-IP adresy** příkaz.</span><span class="sxs-lookup"><span data-stu-id="4517b-142">toolist hello public IP addresses assigned tooscale set virtual machines using CLI 2.0, use hello **az vmss list-instance-public-ips** command.</span></span>

<span data-ttu-id="4517b-143">sady škálování toolist veřejné IP adresy pomocí prostředí PowerShell, použijte hello _Get-AzureRmPublicIpAddress_ příkaz.</span><span class="sxs-lookup"><span data-stu-id="4517b-143">toolist scale set public IP addresses using PowerShell, use hello _Get-AzureRmPublicIpAddress_ command.</span></span> <span data-ttu-id="4517b-144">Například:</span><span class="sxs-lookup"><span data-stu-id="4517b-144">For example:</span></span>
```PowerShell
PS C:\> Get-AzureRmPublicIpAddress -ResourceGroupName myrg -VirtualMachineScaleSetName myvmss
```

<span data-ttu-id="4517b-145">Můžete také dotazu hello veřejné IP adresy pomocí přímo odkazující na id prostředku hello konfiguraci hello veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="4517b-145">You can also query hello public IP addresses by referencing hello resource id of hello public IP address configuration directly.</span></span> <span data-ttu-id="4517b-146">Například:</span><span class="sxs-lookup"><span data-stu-id="4517b-146">For example:</span></span>
```PowerShell
PS C:\> Get-AzureRmPublicIpAddress -ResourceGroupName myrg -Name myvmsspip
```

<span data-ttu-id="4517b-147">tooquery hello veřejné IP adresy přiřazené tooscale sada virtuálních počítačů pomocí hello [Průzkumníka prostředků Azure](https://resources.azure.com), nebo hello REST API služby Azure s verzí **2017-03-30** nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="4517b-147">tooquery hello public IP addresses assigned tooscale set virtual machines using hello [Azure Resource Explorer](https://resources.azure.com), or hello Azure REST API with version **2017-03-30** or higher.</span></span>

<span data-ttu-id="4517b-148">tooview veřejné IP adresy pro škálování nastavit pomocí hello Průzkumníkem prostředků, podívejte se na hello **publicipaddresses, na které** oddílu v části škálovací sadu.</span><span class="sxs-lookup"><span data-stu-id="4517b-148">tooview public IP addresses for a scale set using hello Resource Explorer, look at hello **publicipaddresses** section under your scale set.</span></span> <span data-ttu-id="4517b-149">Například: https://resources.azure.com/subscriptions/_id_vašeho_předplatného_/resourceGroups/_vaše_skupina_prostředků_/providers/Microsoft.Compute/virtualMachineScaleSets/_vaše_škálovací_sada_virtuálních_počítačů_/publicipaddresses</span><span class="sxs-lookup"><span data-stu-id="4517b-149">For example: https://resources.azure.com/subscriptions/_your_sub_id_/resourceGroups/_your_rg_/providers/Microsoft.Compute/virtualMachineScaleSets/_your_vmss_/publicipaddresses</span></span>

```
GET https://management.azure.com/subscriptions/{your sub ID}/resourceGroups/{RG name}/providers/Microsoft.Compute/virtualMachineScaleSets/{scale set name}/publicipaddresses?api-version=2017-03-30
```

<span data-ttu-id="4517b-150">Příklad výstupu:</span><span class="sxs-lookup"><span data-stu-id="4517b-150">Example output:</span></span>
```json
{
  "value": [
    {
      "name": "pub1",
      "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/pipvmss/virtualMachines/0/networkInterfaces/pipvmssnic/ipConfigurations/yourvmssipconfig/publicIPAddresses/pub1",
      "etag": "W/\"a64060d5-4dea-4379-a11d-b23cd49a3c8d\"",
      "properties": {
        "provisioningState": "Succeeded",
        "resourceGuid": "ee8cb20f-af8e-4cd6-892f-441ae2bf701f",
        "ipAddress": "13.84.190.11",
        "publicIPAddressVersion": "IPv4",
        "publicIPAllocationMethod": "Dynamic",
        "idleTimeoutInMinutes": 15,
        "ipConfiguration": {
          "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmss/virtualMachines/0/networkInterfaces/yourvmssnic/ipConfigurations/yourvmssipconfig"
        }
      }
    },
    {
      "name": "pub1",
      "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmss/virtualMachines/3/networkInterfaces/yourvmssnic/ipConfigurations/yourvmssipconfig/publicIPAddresses/pub1",
      "etag": "W/\"5f6ff30c-a24c-4818-883c-61ebd5f9eee8\"",
      "properties": {
        "provisioningState": "Succeeded",
        "resourceGuid": "036ce266-403f-41bd-8578-d446d7397c2f",
        "ipAddress": "13.84.159.176",
        "publicIPAddressVersion": "IPv4",
        "publicIPAllocationMethod": "Dynamic",
        "idleTimeoutInMinutes": 15,
        "ipConfiguration": {
          "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmss/virtualMachines/3/networkInterfaces/yourvmssnic/ipConfigurations/yourvmssipconfig"
        }
      }
    }
```

## <a name="multiple-ip-addresses-per-nic"></a><span data-ttu-id="4517b-151">Několik IP adres na síťové rozhraní</span><span class="sxs-lookup"><span data-stu-id="4517b-151">Multiple IP addresses per NIC</span></span>
<span data-ttu-id="4517b-152">Každý síťový adaptér připojený tooa virtuální počítač ve škálovací sadě může mít jeden nebo více konfigurací IP adres, které jsou s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="4517b-152">Every NIC attached tooa VM in a scale set can have one or more IP configurations associated with it.</span></span> <span data-ttu-id="4517b-153">Každá konfigurace má přiřazenu jednu privátní IP adresu.</span><span class="sxs-lookup"><span data-stu-id="4517b-153">Each configuration is assigned one private IP address.</span></span> <span data-ttu-id="4517b-154">Každá konfigurace také může mít přiřazen jeden prostředek veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="4517b-154">Each configuration may also have one public IP address resource associated with it.</span></span> <span data-ttu-id="4517b-155">toounderstand kolik IP adresy můžete přiřadit tooa síťového adaptéru, a kolik veřejné IP adresy v předplatné Azure, můžete použít příliš[Azure omezení](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).</span><span class="sxs-lookup"><span data-stu-id="4517b-155">toounderstand how many IP addresses can be assigned tooa NIC, and how many public IP addresses you can use in an Azure subscription, refer too[Azure limits](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).</span></span>

## <a name="multiple-nics-per-virtual-machine"></a><span data-ttu-id="4517b-156">Několik síťových rozhraní na virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="4517b-156">Multiple NICs per virtual machine</span></span>
<span data-ttu-id="4517b-157">Může obsahovat maximálně too8 síťové adaptéry na virtuálním počítači, v závislosti na velikosti počítače.</span><span class="sxs-lookup"><span data-stu-id="4517b-157">You can have up too8 NICs per virtual machine, depending on machine size.</span></span> <span data-ttu-id="4517b-158">Hello maximální počet síťových adaptérů na počítač je k dispozici v hello [článku velikost virtuálního počítače](../virtual-machines/windows/sizes.md).</span><span class="sxs-lookup"><span data-stu-id="4517b-158">hello maximum number of NICs per machine is available in hello [VM size article](../virtual-machines/windows/sizes.md).</span></span> <span data-ttu-id="4517b-159">Hello následující příklad je že profil sítě s více položek síťový adaptér a víc veřejných IP adres na virtuální počítač sady škálování:</span><span class="sxs-lookup"><span data-stu-id="4517b-159">hello following example is a scale set network profile showing multiple NIC entries, and multiple public IPs per virtual machine:</span></span>
```json
"networkProfile": {
    "networkInterfaceConfigurations": [
        {
        "name": "nic1",
        "properties": {
            "primary": "true",
            "ipConfigurations": [
            {
                "name": "ip1",
                "properties": {
                "subnet": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                },
                "publicipaddressconfiguration": {
                    "name": "pub1",
                    "properties": {
                    "idleTimeoutInMinutes": 15
                    }
                },
                "loadBalancerInboundNatPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                    }
                ],
                "loadBalancerBackendAddressPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                    }
                ]
                }
            }
            ]
        }
        },
        {
        "name": "nic2",
        "properties": {
            "primary": "false",
            "ipConfigurations": [
            {
                "name": "ip1",
                "properties": {
                "subnet": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                },
                "publicipaddressconfiguration": {
                    "name": "pub1",
                    "properties": {
                    "idleTimeoutInMinutes": 15
                    }
                },
                "loadBalancerInboundNatPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                    }
                ],
                "loadBalancerBackendAddressPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                    }
                ]
                }
            }
            ]
        }
        }
    ]
}
```

## <a name="nsg-per-scale-set"></a><span data-ttu-id="4517b-160">NSG na škálovací sadu</span><span class="sxs-lookup"><span data-stu-id="4517b-160">NSG per scale set</span></span>
<span data-ttu-id="4517b-161">Skupiny zabezpečení sítě je možné použít přímo tooa škálovací sadu, přidáním odkazu toohello síťové rozhraní konfigurační sekce hello měřítka nastavit vlastnosti virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4517b-161">Network Security Groups can be applied directly tooa scale set, by adding a reference toohello network interface configuration section of hello scale set virtual machine properties.</span></span>

<span data-ttu-id="4517b-162">Například:</span><span class="sxs-lookup"><span data-stu-id="4517b-162">For example:</span></span> 
```
"networkProfile": {
    "networkInterfaceConfigurations": [
        {
            "name": "nic1",
            "properties": {
                "primary": "true",
                "ipConfigurations": [
                    {
                        "name": "ip1",
                        "properties": {
                            "subnet": {
                                "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                            }
                "loadBalancerInboundNatPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                                }
                            ],
                            "loadBalancerBackendAddressPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                                }
                            ]
                        }
                    }
                ],
                "networkSecurityGroup": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/networkSecurityGroups/', variables('nsgName'))]"
                }
            }
        }
    ]
}
```

## <a name="next-steps"></a><span data-ttu-id="4517b-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4517b-163">Next steps</span></span>
<span data-ttu-id="4517b-164">Další informace o virtuálních sítí Azure, najdete v části příliš[této dokumentace](../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4517b-164">For more information about Azure virtual networks, refer too[this documentation](../virtual-network/virtual-networks-overview.md).</span></span>
