---
title: "aaaCreate a spravovat virtuální počítače Windows s hello modul Azure PowerShell | Microsoft Docs"
description: "Kurz – vytvoření a správa virtuálních počítačů Windows s hello modul Azure PowerShell"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 20adcb673ef4de683e6ad82d048a9625a1dc838c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-windows-vms-with-hello-azure-powershell-module"></a>Vytvoření a správa virtuálních počítačů Windows s hello modul Azure PowerShell

Virtuální počítače Azure, zadejte plně konfigurovatelné a flexibilní výpočetního prostředí. Tento kurz se zaměřuje na základní virtuální počítač Azure nasazení položky například vyberete velikost virtuálního počítače, výběr image virtuálního počítače a nasazení virtuálního počítače. Získáte informace o těchto tématech:

> [!div class="checklist"]
> * Vytvořit a připojit tooa virtuálních počítačů
> * Vyberte a použijte Image virtuálních počítačů
> * Zobrazení a používání určité velikosti virtuálních počítačů
> * Změna velikosti virtuálního počítače
> * Zobrazení a pochopit stav virtuálního počítače

Tento kurz vyžaduje hello prostředí Azure PowerShell verze modulu 3,6 nebo novější. Spustit ` Get-Module -ListAvailable AzureRM` toofind hello verze. Pokud potřebujete tooupgrade, přečtěte si [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="create-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků s hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) příkaz. 

Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. Před virtuálního počítače je třeba vytvořit skupinu prostředků. V tomto příkladu skupinu prostředků s názvem *myResourceGroupVM* je vytvořen v hello *EastUS* oblast. 

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupVM -Location EastUS
```

Skupina prostředků Hello je zadána při vytváření nebo úpravách virtuálního počítače, které jsou viditelné v rámci tohoto kurzu.

## <a name="create-virtual-machine"></a>Vytvoření virtuálního počítače

Virtuální počítač musí být připojené tooa virtuální sítě. Byste komunikovat s hello virtuálního počítače pomocí veřejnou IP adresu prostřednictvím karty síťového rozhraní.

### <a name="create-virtual-network"></a>Vytvoření virtuální sítě

Vytvořte podsíť s [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig):

```powershell
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24
```

Vytvoření virtuální sítě s [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):

```powershell
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupVM `
  -Location EastUS `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 ` 
  -Subnet $subnetConfig
```
### <a name="create-public-ip-address"></a>Vytvoření veřejné IP adresy

Vytvoření veřejné IP adresy s [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):

```powershell
$pip = New-AzureRmPublicIpAddress ` 
  -ResourceGroupName myResourceGroupVM `
  -Location EastUS ` 
  -AllocationMethod Static `
  -Name myPublicIPAddress
```

### <a name="create-network-interface-card"></a>Vytvoření karty síťového rozhraní

Vytvoření síťová karta s [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface):

```powershell
$nic = New-AzureRmNetworkInterface `
  -ResourceGroupName myResourceGroupVM  `
  -Location EastUS `
  -Name myNic `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id
```

### <a name="create-network-security-group"></a>Vytvořit skupinu zabezpečení sítě

Azure [skupinu zabezpečení sítě](../../virtual-network/virtual-networks-nsg.md) (NSG) řídí příchozí a odchozí přenosy pro jeden nebo více virtuálních počítačů. Pravidla skupiny zabezpečení sítě, povolit nebo odepřít síťový provoz na konkrétní port nebo rozsah portů. Tato pravidla může zahrnovat předpona zdrojové adresy tak, aby jenom přenosy v předdefinované zdroj může komunikovat s virtuálním počítačem. tooaccess hello IIS webový server, kterou instalujete, je nutné přidat příchozí pravidlo NSG.

použít toocreate příchozího pravidla NSG, [přidat AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/add-azurermnetworksecurityruleconfig). Hello následující příklad vytvoří pravidlo NSG s názvem *myNSGRule* , otevře port *3389* hello virtuálního počítače:

```powershell
$nsgRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myNSGRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 1000 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 3389 `
  -Access Allow
```

Vytvořte pomocí NSG hello *myNSGRule* s [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup):

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupVM `
    -Location EastUS `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRule
```

Přidat hello NSG toohello podsíť ve virtuální síti hello s [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig):

```powershell
Set-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -VirtualNetwork $vnet `
    -NetworkSecurityGroup $nsg `
    -AddressPrefix 192.168.1.0/24
```

Aktualizace hello virtuální síť s [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork):

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="create-virtual-machine"></a>Vytvoření virtuálního počítače

Při vytváření virtuálního počítače, jsou k dispozici jako jsou bitové kopie operačního systému, přihlašovací údaje velikost a správu disku několik možností. V tomto příkladu se vytvoří virtuální počítač s názvem *Můjvp* spuštěné hello nejnovější verzi systému Windows Server 2016 Datacenter.

Nastavte hello uživatelské jméno a heslo, které potřebuje pro účet správce hello na hello virtuální počítač s [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```powershell
$cred = Get-Credential
```

Vytvořit hello počáteční konfiguraci pro virtuální počítač hello s [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig):

```powershell
$vm = New-AzureRmVMConfig -VMName myVM -VMSize Standard_D1
```

Přidat konfiguraci virtuálního počítače hello operačního systému informace toohello s [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem):

```powershell
$vm = Set-AzureRmVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName myVM `
    -Credential $cred `
    -ProvisionVMAgent -EnableAutoUpdate
```

Přidat konfiguraci virtuálního počítače hello image informace toohello s [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage):

```powershell
$vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
```

Přidat hello konfigurace operačního systému disku nastavení toohello virtuální počítač s [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk):

```powershell
$vm = Set-AzureRmVMOSDisk `
    -VM $vm `
    -Name myOsDisk `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite
```

Přidat hello síťové rozhraní karty, který jste dříve vytvořili toohello konfiguraci virtuálního počítače s [přidat AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):

```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

Vytvoření virtuálního počítače hello s [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).

```powershell
New-AzureRmVM -ResourceGroupName myResourceGroupVM -Location EastUS -VM $vm
```

## <a name="connect-toovm"></a>Připojit tooVM

Po dokončení nasazení hello vytvořte připojení ke vzdálené ploše se hello virtuálního počítače.

Spusťte následující příkazy tooreturn hello veřejnou IP adresu virtuálního počítače hello hello. Poznamenejte si tuto IP adresu, tooit můžete propojit s váš prohlížeč tootest připojení k webu v příštím kroku.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroupVM  | Select IpAddress
```

Použití hello následující příkaz toocreate a relace vzdálené plochy s hello virtuálního počítače. Nahraďte IP adresu hello hello *publicIPAddress* virtuálního počítače. Po zobrazení výzvy zadejte přihlašovací údaje hello použité při vytváření hello virtuálního počítače.

```powershell
mstsc /v:<publicIpAddress>
```

## <a name="understand-vm-images"></a>Pochopení Image virtuálních počítačů

Hello Azure marketplace obsahuje mnoho bitové kopie virtuálních počítačů, které se dají použít toocreate nového virtuálního počítače. V předchozích krocích hello byl vytvořen virtuální počítač pomocí bitové kopie systému Windows Server 2016-Datacenter hello. V tomto kroku modul prostředí PowerShell hello je použité toosearch hello marketplace pro jiné Image systému Windows, které může taky jako základ pro nové virtuální počítače. Tento proces se skládá z hledání hello vydavatele, nabídky a název bitové kopie hello (Sku). 

Použití hello [Get-AzureRmVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher) tooreturn příkaz seznam vydavatelů bitové kopie.  

```powersehll
Get-AzureRmVMImagePublisher -Location "EastUS"
```

Použití hello [Get-AzureRmVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer) tooreturn seznam nabídek bitové kopie. Pomocí tohoto příkazu hello vrátit seznam je filtrovaný na zadaný vydavatel hello. 

```powershell
Get-AzureRmVMImageOffer -Location "EastUS" -PublisherName "MicrosoftWindowsServer"
```

```powershell
Offer             PublisherName          Location
-----             -------------          -------- 
Windows-HUB       MicrosoftWindowsServer EastUS 
WindowsServer     MicrosoftWindowsServer EastUS   
WindowsServer-HUB MicrosoftWindowsServer EastUS   
```

Hello [Get-AzureRmVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) příkaz bude potom filtrovat tooreturn název vydavatele a nabídka hello seznam názvů bitové kopie.

```powershell
Get-AzureRmVMImageSku -Location "EastUS" -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer"
```

```powershell
Skus                            Offer         PublisherName          Location
----                            -----         -------------          --------
2008-R2-SP1                     WindowsServer MicrosoftWindowsServer EastUS  
2008-R2-SP1-BYOL                WindowsServer MicrosoftWindowsServer EastUS  
2012-Datacenter                 WindowsServer MicrosoftWindowsServer EastUS  
2012-Datacenter-BYOL            WindowsServer MicrosoftWindowsServer EastUS  
2012-R2-Datacenter              WindowsServer MicrosoftWindowsServer EastUS  
2012-R2-Datacenter-BYOL         WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter                 WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter-Server-Core     WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter-with-Containers WindowsServer MicrosoftWindowsServer EastUS  
2016-Nano-Server                WindowsServer MicrosoftWindowsServer EastUS
```

Tato informace může být použité toodeploy virtuální počítač s konkrétní image. Tento příklad nastaví název bitové kopie hello hello objekt virtuálního počítače. V tomto kurzu postup dokončení nasazení naleznete v předchozích příkladech toohello.

```powershell
$vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter-with-Containers `
    -Version latest
```

## <a name="understand-vm-sizes"></a>Pochopení velikosti virtuálních počítačů

Velikost virtuálního počítače určuje hello množství výpočetní prostředky, například procesoru, grafického procesoru a paměti, které jsou vytvářeny k dispozici toohello virtuálního počítače. Třeba virtuální počítače toobe vytvořené s velikostí vhodné pro hello očekávat pracovní zatížení. Hodnota se zvyšuje zatížení, můžete ke změně velikosti existujícího virtuálního počítače.

### <a name="vm-sizes"></a>Velikosti virtuálních počítačů

Následující tabulka Hello rozděluje velikosti do případy použití.  

| Typ                     | Velikosti           |    Popis       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| Obecné účely         |DSv2, Dv2, DS, D, Av2, A0 7| Vyrovnáváním procesoru do paměti. Ideální pro vývojáře nebo test a řešení pro malé toomedium aplikacím a datům.  |
| Optimalizované z hlediska výpočetních služeb      | Služby FS, F             | Vysoké využití procesoru do paměti. Je vhodný pro střední provoz aplikace, síťových zařízení a dávkových procesů.        |
| Optimalizované z hlediska paměti       | GS, G, DSv2, DS, Dv2, D   | Vysoká paměti na core. Výborně hodí pro relačních databází, střední toolarge mezipaměti a analýzy v paměti.                 |
| Optimalizované z hlediska úložiště       | Ls                | Vysoká propustnost disku a V/V. Ideální pro databáze NoSQL, SQL a velké objemy dat.                                                         |
| GPU           | VS, NC            | Specializované virtuální počítače cílené pro velkou grafické vykreslování a úpravy videa.       |
| Vysoký výkon | H, A8 11          | Naše nejúčinnějších procesoru virtuálních počítačů s volitelné vysokou propustností síťová rozhraní (počítače RDMA). 


### <a name="find-available-vm-sizes"></a>Najít dostupných velikostí virtuálních počítačů

toosee seznam virtuálních počítačů velikostí k dispozici v určité oblasti, použijte hello [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) příkaz.

```powershell
Get-AzureRmVMSize -Location EastUS
```

## <a name="resize-a-vm"></a>Změna velikosti virtuálního počítače

Po nasazení virtuálního počítače, může být změněnou tooincrease nebo snížit přidělení prostředků.

Před změnou velikosti virtuálního počítače, zkontrolujte, zda hello požadovaná velikost je k dispozici na hello aktuální cluster virtuálních počítačů. Hello [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) příkaz vrátí seznam velikostí. 

```powershell
Get-AzureRmVMSize -ResourceGroupName myResourceGroupVM -VMName myVM 
```

V případě, že velikost je k dispozici potřeby hello hello virtuálních počítačů velikost lze změnit ze stavu na zapnuté, ale po restartu během operace hello.

```powershell
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroupVM  -VMName myVM 
$vm.HardwareProfile.VmSize = "Standard_D4"
Update-AzureRmVM -VM $vm -ResourceGroupName myResourceGroupVM 
```

V případě potřeby hello velikost není v aktuální clusteru hello hello virtuálních počítačů potřeb, které může dojít toobe navrácena před hello změnit velikost operace. Poznámka: když hello virtuální počítač je zapnutý zpět, se odeberou všechna data na dočasné disku hello a hello veřejnou IP adresu změnit, dokud se používá statickou IP adresu. 

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroupVM -Name "myVM" -Force
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroupVM  -VMName myVM
$vm.HardwareProfile.VmSize = "Standard_F4s"
Update-AzureRmVM -VM $vm -ResourceGroupName myResourceGroupVM 
Start-AzureRmVM -ResourceGroupName myResourceGroupVM  -Name $vm.name
```

## <a name="vm-power-states"></a>Stavy spotřeby virtuálních počítačů

Virtuální počítač Azure může mít jednu z mnoha snížené spotřeby energie. Tento stav představuje hello aktuální stav hello virtuálních počítačů z hlediska hello hello hypervisoru. 

### <a name="power-states"></a>Stavy spotřeby.

| Stav napájení | Popis
|----|----|
| Spouštění | Označuje, že se spouští hello virtuálního počítače. |
| Běžící (Spuštěno) | Označuje, že aby hello virtuální počítač běží. |
| Zastavování | Označuje, že Probíhá zastavování virtuálního počítače hello. | 
| Zastaveno | Označuje, že tento virtuální počítač hello je zastavena. Všimněte si, že virtuální počítače ve stavu Zastaveno hello stále platit poplatky výpočty.  |
| Rušení přidělování | Označuje, že tento virtuální počítač hello je navrácena. |
| Zrušeno | Označuje, že tento virtuální počítač hello je úplně odebrat z hello hypervisoru, ale stále k dispozici v rovině řízení hello. Virtuální počítače v hello Deallocated stavu nevznikají poplatky za výpočty. |
| - | Označuje, že stav napájení hello hello virtuálního počítače neznámý. |

### <a name="find-power-state"></a>Najít stav napájení

Stav hello tooretrieve konkrétní virtuální počítač, použijte hello [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) příkaz. Zda toospecify být platný název pro virtuální počítač a skupinu prostředků. 

```powershell
Get-AzureRmVM `
    -ResourceGroupName myResourceGroup `
    -Name myVM `
    -Status | Select @{n="Status"; e={$_.Statuses[1].Code}}
```

Výstup:

```powershell
Status
------
PowerState/running
```

## <a name="management-tasks"></a>Úlohy správy

Během životního cyklu hello virtuálního počítače můžete úlohy správy toorun například spuštění, zastavení nebo odstranění virtuálního počítače. Kromě toho můžete toocreate skripty tooautomate opakovaných nebo komplexní úlohy. Pomocí Azure PowerShell, mnoho běžné úlohy správy lze spustit z příkazového řádku hello nebo ve skriptech.

### <a name="stop-virtual-machine"></a>Zastavit virtuální počítač

Zastavení a zrušit přidělení virtuálního počítače s [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm):

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroupVM -Name "myVM" -Force
```

Pokud chcete tookeep hello virtuální počítač ve stavu, zřízený, použijte parametr - StayProvisioned hello.

### <a name="start-virtual-machine"></a>Spustit virtuální počítač

```powershell
Start-AzureRmVM -ResourceGroupName myResourceGroupVM -Name myVM
```

### <a name="delete-resource-group"></a>Odstraňte skupinu prostředků

Odstranění skupiny prostředků se také odstraní všechny prostředky obsažené v rámci.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroupVM -Force
```

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se dozvěděli o základní vytvoření virtuálního počítače a správy, jako například:

> [!div class="checklist"]
> * Vytvořit a připojit tooa virtuálních počítačů
> * Vyberte a použijte Image virtuálních počítačů
> * Zobrazení a používání určité velikosti virtuálních počítačů
> * Změna velikosti virtuálního počítače
> * Zobrazení a pochopit stav virtuálního počítače

Posunutí další kurz toolearn toohello o disky virtuálních počítačů.  

> [!div class="nextstepaction"]
> [Vytvoření a správa virtuálních počítačů disky](./tutorial-manage-data-disk.md)
