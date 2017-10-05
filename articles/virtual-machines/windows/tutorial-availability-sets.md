---
title: "Kurz sady dostupnosti pro virtuální počítače Windows v Azure | Microsoft Docs"
description: "Další informace o skupiny dostupnosti pro virtuální počítače Windows v Azure."
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
ms.openlocfilehash: d918362106ef93cf47620e0018d363cd510884b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-availability-sets"></a>Jak používat skupiny dostupnosti

V tomto kurzu se dozvíte, jak zvýšit dostupnost a spolehlivost řešení virtuálního počítače v Azure pomocí funkce názvem skupiny dostupnosti. Skupiny dostupnosti Ujistěte se, že virtuálních počítačů nasadit v Azure jsou distribuovány na více clusterů izolované hardwaru. To zajišťuje, že pokud dojde selhání hardwaru nebo softwaru v rámci Azure, bude mít vliv pouze dílčí sadu virtuálních počítačů a že celkového řešení zůstanou dostupné a funkční z perspektivy zákazníky ho pomocí. 

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření skupiny dostupnosti
> * Vytvoření virtuálního počítače v nastavení dostupnosti
> * Zkontrolujte dostupné velikosti virtuálních počítačů

Tento kurz vyžaduje modul Azure PowerShell verze 3.6 nebo novější. Verzi zjistíte spuštěním příkazu ` Get-Module -ListAvailable AzureRM`. Pokud je třeba upgradovat, přečtěte si téma [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="availability-set-overview"></a>Přehled sady dostupnosti

Skupiny dostupnosti je logické seskupení funkci, která můžete použít v Azure k zajištění, že prostředky virtuálních počítačů, které umístíte v něm jsou od sebe navzájem oddělené, když jsou nasazeny v rámci datového centra Azure. Azure zajistí, že virtuální počítače umístíte do skupiny dostupnosti spustit více fyzických serverů, výpočetní stojany, jednotky úložiště a síťové přepínače. To zajistí, že v případě hardwaru nebo softwaru Azure selhání, bude mít vliv jenom podmnožinu virtuální počítače a aplikace celkové zůstane nahoru a nadále k dispozici pro vaše zákazníky. Pomocí nastavení dostupnosti je nezbytné schopnost využít, pokud chcete vytvářet spolehlivé cloudové řešení.

Pojďme typické řešení virtuálních počítačů na základě kterých může mít 4 front-end webové servery a používat 2 back-end virtuální počítače, které jsou hostiteli databáze. S Azure, by chcete definovat dvě sady dostupnosti před nasazením virtuálních počítačů: jednu sadu dostupnosti pro vrstvu "web" a jednu sadu dostupnosti pro vrstvu "databáze". Při vytváření nového virtuálního počítače můžete určit skupiny dostupnosti jako parametr pro virtuální počítač az vytvoření příkazu a Azure automaticky zajistí, že virtuální počítače, vytvořte v rámci sady k dispozici jsou izolované napříč několika prostředcích fyzický hardware. To znamená, že pokud fyzický hardware, který webový Server nebo virtuální počítače databáze serveru běží na problém, víte, že ostatní instance webového serveru a databáze virtuální počítače zůstanou spuštěné bez problémů vzhledem k tomu, že jsou na jiný hardware.

Pokud chcete nasadit spolehlivé řešení virtuálních počítačů na základě v rámci Azure byste měli vždycky používat skupiny dostupnosti.

## <a name="create-an-availability-set"></a>Vytvoření skupiny dostupnosti

Můžete vytvořit pomocí sadu dostupnosti [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset). V tomto příkladu jsme nastavit počet aktualizací a chyby domén v *2* pro skupinu dostupnosti s názvem *myAvailabilitySet* v *myResourceGroupAvailability* Skupina prostředků.

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

Virtuální počítače musí být vytvořen v rámci skupinu dostupnosti a ujistěte se, že jsou správně rozmístěny v hardwaru. Nelze přidat existující virtuální počítač po vytvoření sadu dostupnosti. 

Hardware v umístění je rozděleno v několika aktualizace domén a domén selhání. **Aktualizace domény** je skupina virtuální počítače a základní fyzický hardware, který může být restartován ve stejnou dobu. Virtuální počítače ve stejné **doména selhání** sdílejí společné úložiště, jakož i běžné zdroje a sítě vypínač. 

Při vytváření virtuálního počítače pomocí konfigurace pomocí [New-AzureRMVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) zadáte skupinu dostupnosti pomocí `-AvailabilitySetId` parametru určete ID skupiny dostupnosti.

Vytvoření 2 virtuální počítače s [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) ve dostupnosti nastavit.

```powershell
$availabilitySet = Get-AzureRmAvailabilitySet `
    -ResourceGroupName myResourceGroupAvailability `
    -Name myAvailabilitySet

$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

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

   # Here is where we specify the availability set
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

Trvá několik minut vytvořit a nakonfigurovat oba virtuální počítače. Po dokončení bude mít virtuální počítače 2 rozložené mezi základní hardware. 

## <a name="check-for-available-vm-sizes"></a>Zkontrolujte dostupné velikosti virtuálních počítačů 

Můžete přidat další virtuální počítače pro skupinu dostupnosti později, ale musíte vědět, jaké velikosti virtuálních počítačů jsou k dispozici na hardware. Použití [Get-AzureRMVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) seznam všech dostupných velikostí na hardwaru clusteru pro skupinu dostupnosti.

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

Přechodu na v dalším kurzu se dozvíte o sady škálování virtuálního počítače.

> [!div class="nextstepaction"]
> [Vytvoření škálovací sady virtuálních počítačů](tutorial-create-vmss.md)


