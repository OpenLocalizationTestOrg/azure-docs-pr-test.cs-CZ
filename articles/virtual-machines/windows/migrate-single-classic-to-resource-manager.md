---
title: "aaaMigrate tooan Classic virtuálních počítačů ARM spravované disku virtuálního počítače | Microsoft Docs"
description: "Migrovat jeden virtuální počítač Azure z nasazení classic hello modelu tooManaged disky v modelu nasazení Resource Manager hello."
services: virtual-machines-windows
documentationcenter: 
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
ms.date: 06/15/2017
ms.author: cynthn
ms.openlocfilehash: d8c4b9431f5dd8a071fcbc2ee36581a33f76ba62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manually-migrate-a-classic-vm-tooa-new-arm-managed-disk-vm-from-hello-vhd"></a>Ruční migraci Classic virtuálních počítačů tooa nového ARM spravované disku virtuálního počítače z hello virtuálního pevného disku 


Tato část pomůže vám toomigrate existující virtuální počítače Azure z modelu nasazení classic hello příliš[spravované disky](managed-disks-overview.md) v modelu nasazení Resource Manager hello.


## <a name="plan-for-hello-migration-toomanaged-disks"></a>Plánování migrace hello tooManaged disky

Tato část vám pomůže toomake hello nejlepší rozhodnutí o typy virtuálního počítače a disku.


### <a name="location"></a>Umístění

Vyberte umístění, kde Azure spravované disky jsou dostupné. Při migraci disků spravovaných tooPremium, ujistěte se také, že úložiště Premium je k dispozici v hello oblasti, kde jsou plánování toomigrate k. V tématu [služeb Azure byRegion](https://azure.microsoft.com/regions/#services) aktuální informace o dostupných umístění.

### <a name="vm-sizes"></a>Velikost virtuálních počítačů

Při migraci disků spravovaných tooPremium, máte tooupdate hello velikost virtuálního počítače tooPremium hello podporující velikost úložiště k dispozici v hello oblasti, kde je umístěn virtuální počítač. Zkontrolujte hello velikosti virtuálních počítačů, které jsou podporující Storage úrovně Premium. Specifikace velikosti virtuálního počítače Azure Hello jsou uvedeny v [velikosti virtuálních počítačů](sizes.md).
Zkontrolujte hello výkonové charakteristiky virtuálních počítačů, které pracovat s Storage úrovně Premium a zvolte hello nejvhodnější velikost virtuálního počítače, který nejlépe vyhovuje vaší zatížení. Ujistěte se, zda je dostatečnou šířku pásma k dispozici na disku provozu hello toodrive virtuálních počítačů.

### <a name="disk-sizes"></a>Velikost disků

**Premium spravované disky**

Existují sedm typy disků spravované premium, které lze použít s vaší virtuálních počítačů a každá z nich má konkrétní IOPs a propustnost omezení. Při výběru hello Premium typ disku pro virtuální počítač podle potřeb hello vaší aplikace z hlediska kapacity, výkon, škálovatelnost a načte ve špičce, zvažte tyto limity.

| Disky typu Premium  | P4    | P6    | P10   | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|
| Velikost disku           | 128 GB| 512 GB| 128 GB| 512 GB            | 1024 GB (1 TB)    | 2 048 GB (2 TB)    | 4095 GB (4 TB)    | 
| Vstupně-výstupní operace za sekundu / disk       | 120   | 240   | 500   | 2300              | 5000              | 7500              | 7500              | 
| Propustnost / disk | 25 MB za sekundu  | 50 MB za sekundu  | 100 MB za sekundu | 150 MB za sekundu | 200 MB za sekundu | 250 MB za sekundu | 250 MB za sekundu | 

**Standardní disky spravované**

Existují sedm typy spravované standardní disky, které lze použít s virtuálního počítače. Každý z nich mají různé kapacity ale mít stejné IOPS a omezení propustnosti. Vyberte typ hello standardní spravovaných disků na základě potřeb kapacity hello vaší aplikace.

| Disk typu Standard  | S4               | S6               | S10              | S20              | S30              | S40              | S50              | 
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------| 
| Velikost disku           | 30 GB            | 64 GB            | 128 GB           | 512 GB           | 1024 GB (1 TB)   | 2 048 GB (2TB)    | 4095 GB (4 TB)   | 
| Vstupně-výstupní operace za sekundu / disk       | 500              | 500              | 500              | 500              | 500              | 500             | 500              | 
| Propustnost / disk | 60 MB za sekundu | 60 MB za sekundu | 60 MB za sekundu | 60 MB za sekundu | 60 MB za sekundu | 60 MB za sekundu | 60 MB za sekundu | 


### <a name="disk-caching-policy"></a>Zásady ukládání do mezipaměti na disku 

**Premium spravované disky**

Ve výchozím nastavení, je disk ukládání do mezipaměti zásad *jen pro čtení* pro všechny hello Premium datových disků, a *pro čtení a zápis* pro disk operačního systému Premium hello připojené toohello virtuálních počítačů. Toto nastavení konfigurace se doporučuje tooachieve hello optimální výkon pro aplikace IOs. Těžký zápisu nebo pouze pro zápis datové disky (například soubory protokolu serveru SQL Server) zakažte ukládání do mezipaměti disku, takže můžete dosáhnout lepší výkon aplikace.

### <a name="pricing"></a>Ceny

Zkontrolujte hello [ceny pro spravované disky](https://azure.microsoft.com/en-us/pricing/details/managed-disks/). Ceny disků spravované Premium je stejný jako hello nespravované prémiové disky. Ale ceny pro standardní disky spravované se liší od standardní nespravované disky.


## <a name="checklist"></a>Kontrolní seznam

1.  Při migraci disků spravovaných tooPremium, zkontrolujte, zda že je k dispozici v hello oblasti, které provádíte migraci do.

2.  Rozhodněte, hello nový virtuální počítač řad, které budete používat. Při migraci disků spravovaných tooPremium by mělo být schopný úložiště Premium.

3.  Rozhodněte, hello přesnou velikost virtuálního počítače, které budete používat, které jsou k dispozici v hello oblasti, které provádíte migraci do. Velikost virtuálního počítače musí toobe dostatečně velké na to toosupport hello počet datových disků, které máte. Například pokud máte čtyři datových disků, hello virtuálního počítače musí mít dva nebo víc jader. Zvažte také výpočetní výkon, paměti a šířky pásma sítě musí.

4.  Máte hello aktuální virtuální počítač podrobnosti o užitečný, včetně hello seznam disků a odpovídající BLOB VHD.

Připravte aplikaci výpadek. toodo čisté migrace, máte toostop veškeré zpracování hello v aktuálním systému hello. Pak můžete ho získat tooconsistent stavu, který je možné migrovat toohello nová platforma. Doba trvání výpadku závisí na množství dat v hello disky toomigrate hello.


## <a name="migrate-hello-vm"></a>Migrace hello virtuálních počítačů

Připravte aplikaci výpadek. toodo čisté migrace, máte toostop veškeré zpracování hello v aktuálním systému hello. Pak můžete ho získat tooconsistent stavu, který je možné migrovat toohello nová platforma. Doba trvání výpadku závisí hello množství dat v toomigrate disky hello.


1.  Nastavte nejprve, hello společné parametry:

    ```powershell
    $resourceGroupName = 'yourResourceGroupName'
    
    $location = 'your location' 
    
    $virtualNetworkName = 'yourExistingVirtualNetworkName'
    
    $virtualMachineName = 'yourVMName'
    
    $virtualMachineSize = 'Standard_DS3'
    
    $adminUserName = "youradminusername"
    
    $adminPassword = "yourpassword" | ConvertTo-SecureString -AsPlainText -Force
    
    $imageName = 'yourImageName'
    
    $osVhdUri = 'https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd'
    
    $dataVhdUri = 'https://storageaccount.blob.core.windows.net/vhdcontainer/datadisk1.vhd'
    
    $dataDiskName = 'dataDisk1'
    ```

2.  Vytvoření spravovaného disku operačního systému pomocí hello virtuálního pevného disku z hello classic virtuálních počítačů.

    Ujistěte se, že máte zadaný identifikátor URI virtuálního pevného disku operačního systému toohello $osVhdUri parametr hello dokončení hello. Zadejte také **- AccountType** jako **PremiumLRS** nebo **StandardLRS** na základě typu disků (Standard nebo Premium) při migraci do.

    ```powershell
    $osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk (New-AzureRmDiskConfig '
    -AccountType PremiumLRS -Location $location -CreateOption Import -SourceUri $osVhdUri) '
    -ResourceGroupName $resourceGroupName
    ```

3.  Připojte hello operačního systému disku toohello nového virtuálního počítače.

    ```powershell
    $VirtualMachine = New-AzureRmVMConfig -VMName $virtualMachineName -VMSize $virtualMachineSize
    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -ManagedDiskId $osDisk.Id '
    -StorageAccountType PremiumLRS -DiskSizeInGB 128 -CreateOption Attach -Windows
    ```

4.  Vytvoření disku spravovaných dat ze souboru VHD hello dat a přidejte ji toohello nového virtuálního počítače.

    ```powershell
    $dataDisk1 = New-AzureRmDisk -DiskName $dataDiskName -Disk (New-AzureRmDiskConfig '
    -AccountType PremiumLRS -Location $location -CreationDataCreateOption Import '
    -SourceUri $dataVhdUri ) -ResourceGroupName $resourceGroupName
    
    $VirtualMachine = Add-AzureRmVMDataDisk -VM $VirtualMachine -Name $dataDiskName '
    -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1
    ```

5.  Vytvoření nového virtuálního počítače hello nastavením veřejné IP adresy, virtuální sítě a síťový adaptér.

    ```powershell
    $publicIp = New-AzureRmPublicIpAddress -Name ($VirtualMachineName.ToLower()+'_ip') '
    -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
    
    $vnet = Get-AzureRmVirtualNetwork -Name $virtualNetworkName -ResourceGroupName $resourceGroupName
    
    $nic = New-AzureRmNetworkInterface -Name ($VirtualMachineName.ToLower()+'_nic') '
    -ResourceGroupName $resourceGroupName -Location $location -SubnetId $vnet.Subnets[0].Id '
    -PublicIpAddressId $publicIp.Id
    
    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $nic.Id
    
    New-AzureRmVM -VM $VirtualMachine -ResourceGroupName $resourceGroupName -Location $location
    ```

> [!NOTE]
>Mohou existovat další kroky potřebné toosupport aplikace, který je nesmí být zahrnuté v této příručce.
>
>

## <a name="next-steps"></a>Další kroky

- Připojte toohello virtuálního počítače. Pokyny najdete v tématu [jak se tooconnect a protokol na tooan Azure virtuální počítač, systém Windows spuštěný](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

