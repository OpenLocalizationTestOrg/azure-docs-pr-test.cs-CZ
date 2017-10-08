---
title: "aaaCreate Azure škálovací sady virtuálních počítačů | Microsoft Docs"
description: "Vytvoření a nasazení škálování virtuálních počítačů Linux nebo Windows Azure nastavit pomocí rozhraní příkazového řádku Azure, prostředí PowerShell, šablonu nebo Visual Studio."
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 07/21/2017
ms.author: adegeo
ms.openlocfilehash: 73de25c1dd2424e64655b3accfea848926e72f69
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-deploy-a-virtual-machine-scale-set"></a>Vytvoření a nasazení škálovací sadu virtuálních počítačů
Sady škálování virtuálního počítače můžete snadno můžete toodeploy a spravovat identickými virtuálními počítači jako sada. Sady škálování pro aplikace hyperškálovatelný systém zajistit vysoce škálovatelného a přizpůsobitelné výpočetní vrstvy a podporují Image pro platformu Windows, Image platformy Linux, vlastních bitových kopií a rozšíření. Další informace o sadách škálování najdete v tématu [sady škálování virtuálního počítače](virtual-machine-scale-sets-overview.md).

Tento kurz ukazuje, jak toocreate škálovací sady virtuálních počítačů **bez** pomocí hello portálu Azure. Informace o tom, jak toouse hello portálu Azure najdete v tématu [jak toocreate škálovací sady virtuálních počítačů s hello portál Azure](virtual-machine-scale-sets-portal-create.md).

>[!NOTE]
>Další informace o prostředky Azure Resource Manager najdete v tématu [Azure Resource Manager oproti nasazení classic](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="sign-in-tooazure"></a>Přihlaste se tooAzure

Pokud používáte Azure CLI 2.0 nebo Azure PowerShell toocreate škálování nastavit, musíte nejprve toosign v tooyour předplatného.

Další informace o tom, jak vidíte tooinstall, nastavení a přihlaste se pomocí rozhraní příkazového řádku Azure nebo prostředí PowerShell, tooAzure [Začínáme s Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md) nebo [Začínáme pomocí rutin prostředí Azure PowerShell](/powershell/azure/overview).

```azurecli
az login
```

```powershell
Login-AzureRmAccount
```

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Je nutné nejprve toocreate, který je přidružen skupinu prostředků, hello škálovací sady virtuálních počítačů.

```azurecli
az group create --location westus2 --name MyResourceGroup1
```

```powershell
New-AzureRmResourceGroup -Location westus2 -Name MyResourceGroup1
```

## <a name="create-from-azure-cli"></a>Vytvoření z rozhraní příkazového řádku Azure

Pomocí rozhraní příkazového řádku Azure můžete vytvořit škálování virtuálního počítače s minimálním úsilím. Pokud nezadáte výchozí hodnoty, poskytnou jsou pro vás. Například pokud nezadáte žádné informace o virtuální síti, se pro vás vytvoří virtuální síť. Pokud vynecháte hello následující části jsou vytvořeny pro můžete: 
- Nástroj pro vyrovnávání zatížení
- Virtuální síť
- Veřejnou IP adresu

Při výběru hello bitovou kopii virtuálního počítače, které chcete toouse na škálovací sadu virtuálních počítačů hello, máte několik možností:

- NÁZEV URN  
Hello identifikátor prostředku:  
**Win2012R2Datacenter**

- Název URN alias  
Hello popisný název název URN:  
**MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest**

- Vlastní prostředek id  
Cesta tooan Hello prostředků Azure:  
**/subscriptions/Subscription-GUID/resourceGroups/MyResourceGroup/providers/Microsoft.COMPUTE/Images/MyImage**

- Webové prostředky  
Cesta tooan Hello HTTP URI:  
**http://contoso.BLOB.Core.Windows.NET/vhds/osdiskimage.VHD**

>[!TIP]
>Můžete získat seznam dostupných imagí s `az vm image list`.

toocreate škálovací sady virtuálních počítačů, je nutné zadat následující hello:

- Skupina prostředků 
- Name (Název)
- Bitové kopie operačního systému
- Informace o ověřování 
 
Hello následující ukázka vytvoří sada škálování základní virtuální počítač (Tento krok může trvat několik minut).

```azurecli
az vmss create --resource-group MyResourceGroup1 --name MyScaleSet --image UbuntuLTS --authentication-type password --admin-username azureuser --admin-password P@ssw0rd!
```

Po dokončení příkazu hello bude mít teď vaše sad vytvořený škálování virtuálního počítače. IP adresa hello tooget hello virtuálního počítače může být nutné, aby mohl připojit tooit. Mnoho různé informace o virtuálním počítači hello (včetně hello IP adresu) můžete získat s hello následující příkaz. 

```azurecli
az vmss list-instance-connection-info --resource-group MyResourceGroup1 --name MyScaleSet
```

## <a name="create-from-powershell"></a>Vytvoření z prostředí PowerShell

PowerShell je složitější toouse než rozhraní příkazového řádku Azure. Zatímco rozhraní příkazového řádku Azure poskytuje výchozí hodnoty pro souvisejícím se sítí prostředky (například nástroje pro vyrovnávání zatížení, IP adresy a virtuální sítě), prostředí PowerShell nemá. Odkazování na bitovou kopii pomocí prostředí PowerShell je něco víc složitá příliš. Bitové kopie můžete získat pomocí hello následující rutiny:

1. Get-AzureRMVMImagePublisher
2. Get-AzureRMVMImageOffer
3. Get-AzureRmVMImageSku

Hello rutiny pracovní lze předat v pořadí. Tady je příklad, jak tooget všechny bitové kopie pro hello **západní USA 2** oblasti s vydavatele, který má název hello **microsoft** v ní.

```powershell
Get-AzureRMVMImagePublisher -Location WestUS2 | Where-Object PublisherName -Like *microsoft* | Get-AzureRMVMImageOffer | Get-AzureRmVMImageSku | Select-Object PublisherName, Offer, Skus
```

```
PublisherName              Offer                    Skus
-------------              -----                    ----
microsoft-ads              linux-data-science-vm    linuxdsvm
microsoft-ads              standard-data-science-vm standard-data-science-vm
MicrosoftAzureSiteRecovery Process-Server           Windows-2012-R2-Datacenter
MicrosoftBizTalkServer     BizTalk-Server           2013-R2-Enterprise
MicrosoftBizTalkServer     BizTalk-Server           2013-R2-Standard
MicrosoftBizTalkServer     BizTalk-Server           2016-Developer
MicrosoftBizTalkServer     BizTalk-Server           2016-Enterprise
...
```

Hello pracovní postup pro vytváření škálovací sadu virtuálních počítačů je následující:

1. Vytvořte objekt konfigurace, který obsahuje informace o sadě škálování hello.
2. Referenční dokumentace hello základní image OS.
3. Nakonfigurujte nastavení operačního systému hello: ověřování, předpona názvu virtuálního počítače a uživatele nebo heslo.
4. Konfigurace sítí.
5. Vytvořte sadu škálování hello.

Tento příklad vytvoří základní dvě instance škále nastavení v počítači, který má nainstalovaný Windows Server 2016.

```powershell
# Resource group name from above
$rg = "MyResourceGroup1"
$location = "WestUS2"

# Create a config object
$vmssConfig = New-AzureRmVmssConfig -Location $location -SkuCapacity 2 -SkuName Standard_A0  -UpgradePolicyMode Automatic

# Reference a virtual machine image from hello gallery
Set-AzureRmVmssStorageProfile $vmssConfig -ImageReferencePublisher MicrosoftWindowsServer -ImageReferenceOffer WindowsServer -ImageReferenceSku 2016-Datacenter -ImageReferenceVersion latest

# Set up information for authenticating with hello virtual machine
Set-AzureRmVmssOsProfile $vmssConfig -AdminUsername azureuser -AdminPassword P@ssw0rd! -ComputerNamePrefix myvmssvm

# Create hello virtual network resources

## Basics
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name "my-subnet" -AddressPrefix 10.0.0.0/24
$vnet = New-AzureRmVirtualNetwork -Name "my-network" -ResourceGroupName $rg -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $subnet

## Load balancer
$publicIP = New-AzureRmPublicIpAddress -Name "PublicIP" -ResourceGroupName $rg -Location $location -AllocationMethod Static -DomainNameLabel "myuniquedomain"
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontend" -PublicIpAddress $publicIP
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "LB-backend"
$probe = New-AzureRmLoadBalancerProbeConfig -Name "HealthProbe" -Protocol Tcp -Port 80 -IntervalInSeconds 15 -ProbeCount 2
$inboundNATRule1= New-AzureRmLoadBalancerRuleConfig -Name "webserver" -FrontendIpConfiguration $frontendIP -Protocol Tcp -FrontendPort 80 -BackendPort 80 -IdleTimeoutInMinutes 15 -Probe $probe -BackendAddressPool $backendPool
$inboundNATPool1 = New-AzureRmLoadBalancerInboundNatPoolConfig -Name "RDP" -FrontendIpConfigurationId $frontendIP.Id -Protocol TCP -FrontendPortRangeStart 53380 -FrontendPortRangeEnd 53390 -BackendPort 3389

New-AzureRmLoadBalancer -ResourceGroupName $rg -Name "LB1" -Location $location -FrontendIpConfiguration $frontendIP -LoadBalancingRule $inboundNATRule1 -InboundNatPool $inboundNATPool1 -BackendAddressPool $backendPool -Probe $probe

## IP address config
$ipConfig = New-AzureRmVmssIpConfig -Name "my-ipaddress" -LoadBalancerBackendAddressPoolsId $backendPool.Id -SubnetId $vnet.Subnets[0].Id -LoadBalancerInboundNatPoolsId $inboundNATPool1.Id

# Attach hello virtual network toohello IP object
Add-AzureRmVmssNetworkInterfaceConfiguration -VirtualMachineScaleSet $vmssConfig -Name "network-config" -Primary $true -IPConfiguration $ipConfig

# Create hello scale set with hello config object (this step might take a few minutes)
New-AzureRmVmss -ResourceGroupName $rg -Name "MyScaleSet1" -VirtualMachineScaleSet $vmssConfig
```

### <a name="using-a-custom-virtual-machine-image"></a>Pomocí bitové kopie vlastního virtuálního počítače
Pokud vytváříte škálování nastavit od svoji vlastní image, místo odkazující na bitovou kopii virtuálního počítače z Galerie hello hello _Set-AzureRmVmssStorageProfile_ příkaz bude vypadat takto:
```PowerShell
Set-AzureRmVmssStorageProfile -OsDiskCreateOption FromImage -ManagedDisk PremiumLRS -OsDiskCaching "None" -OsDiskOsType Linux -ImageReferenceId (Get-AzureRmImage -ImageName $VMImage -ResourceGroupName $rg).id
```

## <a name="create-from-a-template"></a>Vytvoření ze šablony

Můžete nasadit škálování virtuálních počítačů, nastavit pomocí šablony Azure Resource Manager. Můžete vytvořit vlastní šablonu nebo použijte jednu z hello [šablony úložiště](https://azure.microsoft.com/resources/templates/?term=vmss). Tyto šablony se dá nasadit přímo tooyour předplatného Azure.

>[!NOTE]
>toocreate vlastní šablonu, vytvořte textový soubor JSON. Obecné informace o toocreate a přizpůsobení šablony, najdete v části [šablon Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).

Vzorové šablony je k dispozici [na Githubu](https://github.com/gatneil/mvss/tree/minimum-viable-scale-set). Další informace o tom, jak toocreate a které ukázkové, najdete v části [minimální přijatelná škálovací sadu](.\virtual-machine-scale-sets-mvss-start.md).

## <a name="create-from-visual-studio"></a>Vytvořit ze sady Visual Studio

Pomocí sady Visual Studio můžete vytvořit projekt skupiny prostředků Azure a přidat šablony tooit sady škálování virtuálních počítačů. Můžete zvolit, jestli chcete tooimport z Githubu nebo hello Galerie webových aplikací Azure. Nasazení skript prostředí PowerShell je vytvořena také za vás. Další informace najdete v tématu [jak toocreate škálovací sady virtuálních počítačů pomocí sady Visual Studio](virtual-machine-scale-sets-vs-create.md).

## <a name="create-from-hello-azure-portal"></a>Vytvoření z hello portálu Azure

Hello portál Azure poskytuje pohodlný způsob tooquickly vytvoření sady škálování. Další informace najdete v tématu [jak toocreate škálovací sady virtuálních počítačů s hello portál Azure](virtual-machine-scale-sets-portal-create.md).

## <a name="next-steps"></a>Další kroky

Další informace o [datových disků](virtual-machine-scale-sets-attached-disks.md).

Zjistěte, jak příliš[správě aplikací](virtual-machine-scale-sets-deploy-app.md).
