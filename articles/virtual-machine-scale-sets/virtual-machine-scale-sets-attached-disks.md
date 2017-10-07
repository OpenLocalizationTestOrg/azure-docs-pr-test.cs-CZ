---
title: "Virtuální počítač škálování sady dat disky připojené aaaAzure | Microsoft Docs"
description: "Zjistěte, jak připojit toouse datových disků s sady škálování virtuálního počítače"
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
ms.date: 4/25/2017
ms.author: guybo
ms.openlocfilehash: 77b66f80934c0aaf7bb1ad0de00a738052a878ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-vm-scale-sets-and-attached-data-disks"></a>Škálovací sady virtuálních počítačů Azure a připojené datové disky
[Škálovací sady virtuálních počítačů](/azure/virtual-machine-scale-sets/) Azure teď podporují virtuální počítače s připojenými datovými disky. V profilu úložiště hello sady škálování, které byly vytvořeny s disky spravované Azure může být definováno datových disků. Dříve hello pouze možnosti přímo připojené úložiště k dispozici s virtuálními počítači v sadách škálování byly jednotky hello operačního systému a dočasného jednotek.

> [!NOTE]
>  Při vytváření škálování s připojené datových disků, které jsou definované, budete potřebovat toomount a formát hello disky z v rámci virtuálního počítače toouse je (pro samostatnou virtuální počítače Azure jenom jako). Toodo pohodlný způsob je toouse rozšíření vlastních skriptů, které volá toopartition standardní skripty a naformátovat všechny hello datové disky na virtuálním počítači.

## <a name="create-a-scale-set-with-attached-data-disks"></a>Vytvoření škálovací sady s připojenými datovými disky
Nastavit škálování s připojené disky jednoduchý způsob toocreate je toouse hello [rozhraní příkazového řádku Azure](https://github.com/Azure/azure-cli) _vmss vytvořit_ příkaz. Hello následující ukázka vytvoří skupinu prostředků Azure a sadu škálování virtuálního počítače 10 virtuálních Ubuntu počítačů, každý s 2 připojených datových disků, 50 GB a 100 GB.
```bash
az group create -l southcentralus -n dsktest
az vmss create -g dsktest -n dskvmss --image ubuntults --instance-count 10 --data-disk-sizes-gb 50 100
```
Všimněte si, že hello _vmss vytvořit_ příkaz výchozí určité hodnoty konfigurace, pokud nezadáte je. toosee hello dostupné možnosti, můžete přepsat zkuste:
```bash
az vmss create --help
```
Zahrnout další způsob toocreate škálování s připojené datových disků je toodefine nastavení škálování v šablonu Azure Resource Manager _dataDisks_ část v hello _storageProfile_a nasadit hello Šablona. Hello 50 GB a 100 GB disk výše uvedeném příkladu by být definován jako to v šabloně hello:
```json
"dataDisks": [
    {
    "lun": 1,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 50
    },
    {
    "lun": 2,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 100
    }
]
```
Můžete zobrazit příklad dokončena, připraveno toodeploy šablony sady škálování s připojený disk zde definované: [https://github.com/chagarw/MDPP/tree/master/101-vmss-os-data](https://github.com/chagarw/MDPP/tree/master/101-vmss-os-data).

## <a name="adding-a-data-disk-tooan-existing-scale-set"></a>Přidání existující škálování dat disku tooan nastavit
> [!NOTE]
>  Je možné ještě připojit datové disky tooa škálování sady, který byl vytvořen s [Azure spravované disky](./virtual-machine-scale-sets-managed-disks.md).

Můžete přidat datový disk tooa virtuálních počítačů měřítkem nastavit pomocí rozhraní příkazového řádku Azure _připojit az vmss disk_ příkaz. Ujistěte se, že zadáváte logickou jednotku (LUN), která se ještě nepoužívá. Následující ukázka rozhraní příkazového řádku Hello přidá 50 GB jednotky toolun 3:
```bash
az vmss disk attach -g dsktest -n dskvmss --size-gb 50 --lun 3
```

Následující příklad PowerShell Hello přidá 50 GB jednotky toolun 3:
```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName myvmssrg -VMScaleSetName myvmss
$vmss = Add-AzureRmVmssDataDisk -VirtualMachineScaleSet $vmss -Lun 3 -Caching 'ReadWrite' -CreateOption Empty -DiskSizeGB 50 -StorageAccountType StandardLRS
Update-AzureRmVmss -ResourceGroupName myvmssrg -Name myvmss -VirtualMachineScaleSet $vmss
```

> [!NOTE]
> Různé velikosti virtuálních počítačů mají různé limity z hello počty připojené disky, které podporují. Zkontrolujte hello [charakteristiky velikost virtuálního počítače](../virtual-machines/windows/sizes.md) před přidáním nového disku.

Můžete také přidat disk přidáním nové položky toohello _dataDisks_ vlastnost hello _storageProfile_ rozsahu nastavit definice a použití změn hello. tootest se najít existující definici sady škálování v hello [Průzkumníka prostředků Azure](https://resources.azure.com/). Vyberte _upravit_ a přidejte nový disk toohello seznam datových disků. Například pomocí výše uvedeném příkladu hello:
```json
"dataDisks": [
    {
    "lun": 1,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 50
    },
    {
    "lun": 2,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 100
    },
    {
    "lun": 3,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 20
    }          
]
```

Potom vyberte _PUT_ tooapply hello změny tooyour škálovací sadu. Tento příklad bude fungovat, pokud použijete velikost virtuálních počítačů, která podporuje více než dva připojené datové disky.

> [!NOTE]
> Při provádění změn tooa škálování nastavit definice například přidáním nebo odebráním datový disk, platí tooall nově vytvořený virtuální počítače, ale virtuální počítače tooexisting platí pouze v případě hello _upgradePolicy_ vlastnost je nastavena příliš "automatické". Pokud je nastaven příliš "Ruční", je třeba toomanually použít nový model tooexisting hello virtuálních počítačů. To provedete hello portálu pomocí hello _aktualizace AzureRmVmssInstance_ prostředí PowerShell příkaz nebo pomocí hello _az vmss aktualizace instance_ rozhraní příkazového řádku příkaz.

## <a name="adding-pre-populated-data-disks-tooan-existent-scale-set"></a>Přidání předem vyplněná datové disky tooan stávající škálovací sadu 
> Při přidávání disků tooan existující sady škálování model návrhu, hello disk bude vždy vytvořit prázdný. Tento scénář také zahrnuje nové instance vytvořené sadě škálování hello. Toto chování je, protože definice scaleset hello má prázdný datový disk. V pořadí toocreate předem vyplněná datové jednotky pro model sadu svědčící škálování můžete zvolit jednu z následujících dvou možností:

* Zkopírování dat z hello instanci 0 virtuálních počítačů toohello datové disky v hello ostatních virtuálních počítačů pomocí vlastního skriptu.
* Vytvoření bitové kopie spravované se disk hello operačního systému a datový disk (s daty hello vyžaduje) a vytvořte nové scaleset s bitovou kopií hello. Tímto způsobem každých vytvořit nový virtuální počítač bude mít data na disku, která je součástí definice hello hello scaleset. Vzhledem k tomu, že tuto definici se budou vztahovat tooan bitovou kopii s datový disk, který má přizpůsobený dat, každý virtuální počítač na hello scaleset bude automaticky spuštěna s tyto změny.

> Hello způsob toocreate vlastní image naleznete zde: [vytvoříte spravované bitovou kopii zobecněný virtuální počítač v Azure](/azure/virtual-machines/windows/capture-image-resource/) 

> Hello uživatel potřebuje toocapture hello instance 0 virtuálního počítače, který má hello požadovaná data a pak použijte tento virtuální pevný disk pro definici hello bitové kopie.

## <a name="removing-a-data-disk-from-a-scale-set"></a>Odebrání datového disku ze škálovací sady
Datový disk můžete ze škálovací sady virtuálních počítačů odebrat pomocí příkazu Azure CLI _az vmss disk detach_. Například hello následující příkaz odebere hello disk definované na logickou jednotku lun 2:
```bash
az vmss disk detach -g dsktest -n dskvmss --lun 2
```  
Podobně můžete také odebrání disku z škálování nastavit odebráním položku z hello _dataDisks_ vlastnost hello _storageProfile_ a použití změn hello. 

## <a name="additional-notes"></a>Další poznámky
Podpora pro Azure spravované disky a škálování, nastavte připojené datových disků je k dispozici ve verzi rozhraní API [ _2016-04-30-preview_ ](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-compute/2016-04-30-preview/swagger/compute.json) nebo novější z hello Microsoft.Compute rozhraní API.

V implementaci počáteční hello připojený disk podpory pro sady škálování nelze připojit nebo odebrat datové disky z jednotlivé virtuální počítače ve škálovací sadě.

Podpora připojených datových disků ve škálovacích sadách je na webu Azure Portal zpočátku omezená. V závislosti na požadavcích, že které můžete použít šablony Azure CLI, prostředí PowerShell, sady SDK a REST API toomanage připojenými disky.


