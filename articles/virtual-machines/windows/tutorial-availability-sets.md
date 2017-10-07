---
title: "aaaAvailability nastaví kurz pro virtuální počítače Windows v Azure | Microsoft Docs"
description: "Další informace o hello dostupnosti nastaví pro virtuální počítače Windows v Azure."
documentationcenter: 
services: virtual-machines-windows
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 853775c5f126dd815c1933f9d71d2274a75ea661
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-availability-sets"></a>Jak nastaví toouse dostupnosti

V tomto kurzu se dozvíte, jak tooincrease hello dostupnost a spolehlivost řešení virtuálního počítače v Azure pomocí funkce názvem skupiny dostupnosti. Skupiny dostupnosti Ujistěte se, že hello virtuálních počítačů nasadit v Azure jsou distribuovány na více clusterů izolované hardwaru. To zajišťuje, že pokud dojde selhání hardwaru nebo softwaru v rámci Azure, bude mít vliv pouze dílčí sadu virtuálních počítačů a že celkového řešení zůstanou dostupné a funkční z hlediska hello zákazníků použití. 

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření skupiny dostupnosti
> * Vytvoření virtuálního počítače v nastavení dostupnosti
> * Zkontrolujte dostupné velikosti virtuálních počítačů

Tento kurz vyžaduje hello prostředí Azure PowerShell verze modulu 3,6 nebo novější. Spustit ` Get-Module -ListAvailable AzureRM` toofind hello verze. Pokud potřebujete tooupgrade, přečtěte si [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="availability-set-overview"></a>Přehled sady dostupnosti

Skupiny dostupnosti je logické seskupení schopností, který můžete použít v Azure tooensure že hello prostředky virtuálních počítačů, které umístíte v něm jsou od sebe navzájem oddělené, když jsou nasazeny v rámci datového centra Azure. Azure zajistí, že hello virtuální počítače umístíte do skupiny dostupnosti spustit více fyzických serverů, výpočetní stojany, jednotky úložiště a síťové přepínače. To zajistí, že hello události hardwaru nebo softwaru Azure chyby, bude mít vliv jenom podmnožinu virtuální počítače a aplikace celkový zůstane nahoru a bude pokračovat zákazníky k dispozici tooyour toobe. Pomocí nastavení dostupnosti je tooleverage nezbytné funkce, pokud chcete toobuild spolehlivé cloudové řešení.

Pojďme typické řešení virtuálních počítačů na základě kterých může mít 4 front-end webové servery a používat 2 back-end virtuální počítače, které jsou hostiteli databáze. S Azure, je vhodnější toodefine dvě sady dostupnosti před nasazením virtuálních počítačů: sadu jeden dostupnosti pro vrstvu "web" hello a jednu sadu dostupnosti pro vrstvu "databáze" hello. Při vytváření nového virtuálního počítače můžete určit hello dostupnosti jako virtuální počítač parametr toohello az vytvoření příkazu a Azure automaticky zajistí, že hello virtuálních počítačů, které vytvoříte v rámci hello k dispozici sady jsou izolovány napříč několika prostředcích fyzický hardware. To znamená, že pokud hello fyzický hardware, který webový Server nebo virtuální počítače databáze serveru běží na problém, víte, že hello ostatní instance webového serveru a databáze virtuální počítače zůstanou spuštěné bez problémů vzhledem k tomu, že jsou na jiný hardware.

Pokud chcete toodeploy spolehlivé řešení virtuálních počítačů na základě v rámci Azure byste měli vždycky používat skupiny dostupnosti.

## <a name="create-an-availability-set"></a>Vytvoření skupiny dostupnosti

Můžete vytvořit pomocí sadu dostupnosti [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset). V tomto příkladu jsme nastavit počet aktualizací a chyby domén v obou hello *2* pro sadu s názvem dostupnosti hello *myAvailabilitySet* v hello *myResourceGroupAvailability*skupinu prostředků.

Vytvořte skupinu prostředků.

```powershell
New-AzureRmResourceGroup -Name myResourceGroupAvailability -Location EastUS
```


```powershell
New-AzureRmAvailabilitySet `
   -Location EastUS `
   -Name myAvailabilitySet `
   -ResourceGroupName myResourceGroupAvailability `
   -Managed `
   -PlatformFaultDomainCount 2 `
   -PlatformUpdateDomainCount 2
```

## <a name="create-vms-inside-an-availability-set"></a>Vytvořit virtuální počítače uvnitř skupiny dostupnosti

Virtuální počítače nutné toobe vytvořen v rámci toomake sadu dostupnosti hello se, že jsou správně distribuovány mezi hello hardwaru. Nelze přidat sadu po vytvoření existující tooan dostupnosti virtuálních počítačů. 

Hello hardwaru na místo je rozděleno v toomultiple aktualizace domén a domén selhání. **Aktualizace domény** je skupina virtuální počítače a základní fyzický hardware, který může být restartován v hello stejnou dobu. Virtuální počítače ve stejné hello **doména selhání** sdílejí společné úložiště, jakož i běžné zdroje a sítě vypínač. 

Při vytváření virtuálního počítače pomocí konfigurace pomocí [New-AzureRMVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) zadejte hello dostupnosti pomocí hello `-AvailabilitySetId` parametr toospecify hello ID sady dostupnosti hello.

Vytvoření 2 virtuální počítače s [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) v sadě dostupnosti hello.

```powershell
$availabilitySet = Get-AzureRmAvailabilitySet `
    -ResourceGroupName myResourceGroupAvailability `
    -Name myAvailabilitySet

$cred = Get-Credential -Message "Enter a username and password for hello virtual machine."

$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24
$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName myResourceGroupAvailability `
    -Location EastUS `
    -Name MYvNET `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

for ($i=1; $i -le 2; $i++)
{
   $pip = New-AzureRmPublicIpAddress `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -Name "mypublicdns$(Get-Random)" `
        -AllocationMethod Static `
        -IdleTimeoutInMinutes 4

   $nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
        -Name myNetworkSecurityGroupRuleRDP$i `
        -Protocol Tcp `
        -Direction Inbound `
        -Priority 1000 `
        -SourceAddressPrefix * `
        -SourcePortRange * `
        -DestinationAddressPrefix * `
        -DestinationPortRange 3389 `
        -Access Allow

   $nsg = New-AzureRmNetworkSecurityGroup `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -Name myNetworkSecurityGroup$i `
        -SecurityRules $nsgRuleRDP

   $nic = New-AzureRmNetworkInterface `
        -Name myNic$i `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -SubnetId $vnet.Subnets[0].Id `
        -PublicIpAddressId $pip.Id `
        -NetworkSecurityGroupId $nsg.Id

   # Here is where we specify hello availability set
   $vm = New-AzureRmVMConfig `
        -VMName myVM$i `
        -VMSize Standard_D1 `
        -AvailabilitySetId $availabilitySet.Id

   $vm = Set-AzureRmVMOperatingSystem `
        -VM $vm `
        -Windows -ComputerName myVM$i `   
        -Credential $cred `
        -ProvisionVMAgent `
        -EnableAutoUpdate
   $vm = Set-AzureRmVMSourceImage `
        -VM $vm `
        -PublisherName MicrosoftWindowsServer `
        -Offer WindowsServer `
        -Skus 2016-Datacenter `
        -Version latest
   $vm = Set-AzureRmVMOSDisk `
        -VM $vm `
        -Name myOsDisk$i `
        -DiskSizeInGB 128 `
        -CreateOption FromImage `
        -Caching ReadWrite
   $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
   New-AzureRmVM `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -VM $vm
}

```

Trvá několik minut toocreate a nakonfigurujte oba virtuální počítače. Po dokončení, budete mít 2 virtuální počítače distribuované ve hello základní hardware. 

## <a name="check-for-available-vm-sizes"></a>Zkontrolujte dostupné velikosti virtuálních počítačů 

Můžete přidat další virtuální počítače toohello dostupnosti později, ale potřebujete tooknow jaké velikosti virtuálních počítačů jsou k dispozici na hello hardwaru. Použití [Get-AzureRMVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) toolist všech dostupných velikostí hello na hello hardwaru clusteru pro sadu dostupnosti hello.

```powershell
Get-AzureRmVMSize `
   -AvailabilitySetName myAvailabilitySet `
   -ResourceGroupName myResourceGroupAvailability  
```

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Vytvoření skupiny dostupnosti
> * Vytvoření virtuálního počítače v nastavení dostupnosti
> * Zkontrolujte dostupné velikosti virtuálních počítačů

Posunutí další kurz toolearn toohello o sady škálování virtuálního počítače.

> [!div class="nextstepaction"]
> [Vytvoření škálovací sady virtuálních počítačů](tutorial-create-vmss.md)


