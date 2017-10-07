---
title: "aaaUpgrade Azure škálovací sady virtuálních počítačů | Microsoft Docs"
description: "Upgrade sadu škálování virtuálního počítače Azure"
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e229664e-ee4e-4f12-9d2e-a4f456989e5d
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: guybo
ms.openlocfilehash: 068e98503f8d37ea71e45b8673a01da2e814f521
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-a-virtual-machine-scale-set"></a>Upgradovat škálovací sadu virtuálních počítačů
Tento článek popisuje, jak můžete vrátit se OS aktualizace tooan virtuální počítač Azure sad škálování bez odstávky. V tomto kontextu aktualizaci operačního systému zahrnuje změna verze hello nebo SKU hello operačního systému nebo změna hello URI vlastní image. Aktualizace bez výpadku prostředků aktualizace virtuálních počítačů najednou, nebo ve skupinách (například jeden doména selhání najednou) namísto všechny najednou. Díky tomu můžete spouštět všechny virtuální počítače, které nejsou probíhá upgrade.

nejednoznačnosti tooavoid umožňuje rozlišit čtyři typy aktualizace operačního systému může být vhodné tooperform:

* Změna verze hello nebo SKU image platformy. Například změna Ubuntu 14.04.2-LTS verzi z 14.04.201506100 too14.04.201507060 nebo změna hello Ubuntu 15.10/nejnovější SKU too16.04.0-LTS/latest. Tento scénář je popsaná v tomto článku.
* Změna hello identifikátor URI, který ukazuje tooa novou verzi vlastní image, který jste vytvořili (**vlastnosti** > **virtualMachineProfile** > **storageProfile**  >  **osDisk** > **image** > **uri**). Tento scénář je popsaná v tomto článku.
* Změna odkaz na obrázek hello škálovací sady, která byla vytvořena pomocí Azure spravované disky.
* Opravy hello operačního systému z virtuálního počítače (to příklady instalace opravy zabezpečení a spuštění služby Windows Update). Tento scénář je podporují, ale nejsou zahrnuté v tomto článku.

Sady škálování virtuálního počítače, které jsou nasazeny jako součást [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) clusteru nejsou popsané v tomto poli. V tématu [opravy operačního systému Windows v clusteru Service Fabric](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-patch-orchestration-application) Další informace o použití dílčích oprav Service Fabric.

Hello základní pořadí pro změnu hello verze operačního systému skladová položka Image platformy nebo hello URI vlastní image vypadá takto:

1. Získáte modelu sady škálování virtuálního počítače hello.
2. Změnit hello verzi, SKU, odkaz na obrázek nebo hodnota identifikátoru URI v modelu hello.
3. Aktualizace modelu hello.
4. Proveďte *manualUpgrade* volání na hello virtuální počítače ve škálovací sadě hello. Tento krok platí jen pokud *upgradePolicy* je nastaven příliš**ruční** v sadě vaší škálování. Pokud je nastaveno příliš**automatické**, všechny virtuální počítače hello upgradují najednou, což způsobuje výpadek.

Tyto informace v paměti a podíváme, jak může aktualizovat verzi hello měřítka, nastavte v prostředí PowerShell a pomocí hello REST API. Tyto příklady zahrnují hello malá image platformy, ale tento článek poskytuje dostatek informací pro tooadapt můžete tento proces tooa vlastní image.

## <a name="powershell"></a>PowerShell
Tento příklad aktualizuje sadu škálování virtuálního počítače systému Windows (vytváření toohello novou verzi 4.0.20160229. Po aktualizaci hello modelu se dělá aktualizace jedna instance virtuálního počítače v čase.

```powershell
$rgname = "myrg"
$vmssname = "myvmss"
$newversion = "4.0.20160229"
$instanceid = "1"

# get hello VMSS model
$vmss = Get-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname

# set hello new version in hello model data
$vmss.virtualMachineProfile.storageProfile.imageReference.version = $newversion

# update hello virtual machine scale set model
Update-AzureRmVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $vmss

# now start updating instances
Update-AzureRmVmssInstance -ResourceGroupName $rgname -VMScaleSetName $vmssname -InstanceId $instanceId
```

Pokud aktualizujete hello identifikátor URI pro vlastní image místo změna image verze platformy, nahraďte hello "nastavit hello novou verzi" řádku pomocí příkazu, který se aktualizuje hello zdrojové bitové kopie identifikátor URI. Například pokud hello škálovací sadu byl vytvořen bez použití Azure spravované disky, hello aktualizace by vypadat takto:

```powershell
# set hello new version in hello model data
$vmss.virtualMachineProfile.storageProfile.osDisk.image.uri= $newURI
```

Pokud vlastní image na základě škálovací sadu byl vytvořen pomocí spravované disky Azure, pak by aktualizovat odkaz na obrázek hello. Například:

```powershell
# set hello new version in hello model data
$vmss.virtualMachineProfile.storageProfile.imageReference.id = $newImageReference
```

## <a name="hello-rest-api"></a>Hello REST API
Tady je několik příkladů Python, které pomocí rozhraní REST API Azure tooroll hello se aktualizaci verze operačního systému. Používejte hello lightweight [azurerm](https://pypi.python.org/pypi/azurerm) knihovny rozhraní REST API Azure obálku funkce toodo GET na škálování hello nastavit modelu, za nímž následuje PUT s aktualizovaném modelu. Se taky podívejte se na zobrazení instance virtuálního počítače tooidentify hello virtuální počítače pomocí aktualizaci domény.

### <a name="vmssupgrade"></a>Vmssupgrade
 [Vmssupgrade](https://github.com/gbowerman/vmsstools) je skript Python, který byl použit tooroll se upgradu tooa OS spuštění virtuálního počítače nastavit jednu aktualizační doménu najednou.

![Skript Vmssupgrade pro výběr virtuálních počítačů nebo v doméně aktualizace](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssupgrade-screenshot.png)

Tento skript umožňuje vybrat tooupdate konkrétních virtuálních počítačů nebo zadat doménu aktualizace. Podporuje změna image verze platformy nebo změna hello URI vlastní image.

### <a name="vmsseditor"></a>Vmsseditor
[Vmsseditor](https://github.com/gbowerman/vmssdashboard) je obecný editor pro sady škálování virtuálního počítače, které ukazuje virtuální počítač stav jako heatmap, kde jeden řádek představuje jednu aktualizační doménu. Kromě jiných věcí můžete aktualizovat hello model pro sadu škálování s novou verzi, SKU nebo vlastní image URI a pak vyberte tooupgrade domény selhání. Pokud tak učiníte, jsou všechny hello virtuální počítače v této doméně aktualizace upgradovaného toohello nový model. Alternativně můžete provést postupného upgradu na základě velikosti dávky hello podle svého výběru.  

Hello následující snímek obrazovky ukazuje model škálování pro Ubuntu 14.04-2LTS verze 14.04.201507060 nastavit. Mnoho dalších možností přidané toothis nástroj od doby pořízení tomto snímku obrazovky.

![Model Vmsseditor měřítka, nastavte pro Ubuntu 14.04-2LTS](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor1.png)

Po kliknutí na tlačítko **Upgrade** a potom **získat podrobnosti o**, virtuální počítače v UD 0 spustí tooupdate.

![Probíhá aktualizace zobrazení Vmsseditor](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor2.png)

