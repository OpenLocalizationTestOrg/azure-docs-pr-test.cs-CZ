---
title: "Nastaví aaaDeploy aplikace na škálování virtuálních počítačů"
description: "Pomocí rozšíření toodepoy aplikace na sady škálování virtuálního počítače Azure."
services: virtual-machine-scale-sets
documentationcenter: 
author: thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f8892199-f2e2-4b82-988a-28ca8a7fd1eb
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: adegeo
ms.openlocfilehash: 5f3988b9511d80370a8be1fc042c21fee212506e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-application-on-virtual-machine-scale-sets"></a>Nasazení aplikace na sady škálování virtuálního počítače

Tento článek popisuje různé způsoby, jak nastavit tooinstall softwaru škálované hello čas hello je zřízený.

Může být vhodné tooreview hello [přehled návrhu nastavení škálování](virtual-machine-scale-sets-design-overview.md) článek, který popisuje některé z hello omezení, která sady škálování virtuálního počítače.

## <a name="capture-and-reuse-an-image"></a>Zaznamenání a znovu použít bitovou kopii

Můžete vytvořit virtuální počítač nastavené v Azure tooprepare bitovou kopii základní pro vaši škálování. Tento proces vytvoří ve vašem účtu úložiště, které můžete použít jako základní bitová kopie hello pro škálovací sadu spravovaných disků. 

Hello následující kroky:

1. Vytvořit virtuální počítač Azure
   * [Linux][linux-vm-create]
   * [Windows][windows-vm-create]

2. Vzdálené do hello virtuálního počítače a přizpůsobení hello systému tooyour libosti.

   Pokud chcete, můžete nainstalovat aplikace hned. Však vědět, že nainstalováním aplikace teď může provedete upgrade vaší aplikace složitější, protože může být nutné tooremove ho první. Tento krok tooinstall místo toho můžete použít všechny požadované součásti, které aplikace může vyžadovat, podobně jako u konkrétní funkce runtime nebo operačního systému.

3. Postupujte podle kurzu hello "zachytit počítač" pro buď [Linux] [ linux-vm-capture] nebo [Windows][windows-vm-capture].

4. Vytvoření [sadu škálování virtuálního počítače] [ vmss-create] s hello bitové kopie URI zachytili v předchozím kroku hello.

Další informace o discích najdete v tématu [přehled disky spravované](../virtual-machines/windows/managed-disks-overview.md) a [datové disky připojené k použití](virtual-machine-scale-sets-attached-disks.md).

## <a name="install-when-hello-scale-set-is-provisioned"></a>Nainstaluje, když je zřízený sadě škálování hello

Rozšíření virtuálního počítače může být použita škálovací sadu virtuálních počítačů tooa. S rozšířením virtuálního počítače můžete přizpůsobit hello virtuálních počítačů v škálování nastavit jako pro celou skupinu. Další informace o rozšířeních najdete v tématu [rozšíření virtuálního počítače](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Existují tři hlavní rozšíření, které můžete použít, podle toho, pokud váš operační systém se systémem Linux nebo systému Windows.

### <a name="windows"></a>Windows

Pro operační systém na základě systému Windows, použijte buď hello **vlastní skript v1.8** rozšíření nebo hello **DSC Powershellu** rozšíření.

#### <a name="custom-script"></a>Vlastní skript

Hello rozšíření vlastních skriptů spouští skript na každou instanci virtuálního počítače ve škálovací sadě hello. Konfigurační soubor nebo proměnná Určuje, které soubory jsou stažené toohello virtuálního počítače, a poté co příkaz spustí. Můžete použít tento toorun instalační program, skript, dávkového souboru jakéhokoliv spustitelného souboru, třeba.

Zatřiďovací tabulku používá prostředí PowerShell pro nastavení hello. Tento příklad konfiguruje toorun rozšíření vlastních skriptů hello Powershellový skript, který nainstaluje službu IIS.

```powershell
# Setup extension configuration hashtable variable
$customConfig = @{
  "fileUris" = @("https://raw.githubusercontent.com/MicrosoftDocs/azure-cloud-services-files/temp/install-iis.ps1");
  "commandToExecute" = "PowerShell -ExecutionPolicy Unrestricted .\install-iis.ps1 >> `"%TEMP%\StartupLog.txt`" 2>&1";
};

# Add hello extension toohello config
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig -Publisher Microsoft.Compute -Type CustomScriptExtension -TypeHandlerVersion 1.8 -Name "customscript1" -Setting $customConfig

# Send hello new config tooAzure
Update-AzureRmVmss -ResourceGroupName $rg -Name "MyVmssTest143"  -VirtualMachineScaleSet $vmssConfig
```

>[!IMPORTANT]
>Použití hello `-ProtectedSetting` přepínač pro všechna nastavení, která mohou obsahovat citlivé informace.

---------


Rozhraní příkazového řádku Azure používá soubor json pro nastavení hello. Tento příklad konfiguruje toorun rozšíření vlastních skriptů hello Powershellový skript, který nainstaluje službu IIS. Uložte následující soubor json jako hello _settings.json_.

```json
{
  "fileUris": [
    "https://raw.githubusercontent.com/MicrosoftDocs/azure-cloud-services-files/temp/install-iis.ps1"
  ],
  "commandToExecute": "PowerShell -ExecutionPolicy Unrestricted .\install-iis.ps1 >> \"%TEMP%\StartupLog.txt\" 2>&1"
}
```

Potom spusťte tento příkaz rozhraní příkazového řádku Azure.

```azurecli
az vmss extension set --publisher Microsoft.Compute --version 1.8 --name CustomScriptExtension --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

>[!IMPORTANT]
>Použití hello `--protected-settings` přepínač pro všechna nastavení, která mohou obsahovat citlivé informace.

### <a name="powershell-dsc"></a>Prostředí PowerShell DSC

Můžete použít PowerShell DSC toocustomize hello škálovací sadu virtuálních počítačů instance. Hello **DSC** rozšíření, které zveřejnil **Microsoft.Powershell** nasadí a spustí konfigurace DSC hello zadaný na každou instanci virtuálního počítače. Konfiguračním souboru nebo proměnná informuje hello rozšíření kde *.zip* balíček je a který _funkce skriptu_ toorun kombinaci.

Zatřiďovací tabulku používá prostředí PowerShell pro nastavení hello. Tento příklad nasadí DSC balíček, který nainstaluje službu IIS.

```powershell
# Setup extension configuration hashtable variable
$dscConfig = @{
  "wmfVersion" = "latest";
  "configuration" = @{
    "url" = "https://github.com/MicrosoftDocs/azure-cloud-services-files/raw/temp/dsc.zip";
    "script" = "configure-http.ps1";
    "function" = "WebsiteTest";
  };
}

# Add hello extension toohello config
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig -Publisher Microsoft.Powershell -Type DSC -TypeHandlerVersion 2.24 -Name "dsc1" -Setting $dscConfig

# Send hello new config tooAzure
Update-AzureRmVmss -ResourceGroupName $rg -Name "myscaleset1"  -VirtualMachineScaleSet $vmssConfig
```

>[!IMPORTANT]
>Použití hello `-ProtectedSetting` přepínač pro všechna nastavení, která mohou obsahovat citlivé informace.

-----------

Rozhraní příkazového řádku Azure používá json pro nastavení hello. Tento příklad nasadí DSC balíček, který nainstaluje službu IIS. Uložte následující soubor json jako hello _settings.json_.

```json
{
  "wmfVersion": "latest",
  "configuration": {
    "url": "https://github.com/MicrosoftDocs/azure-cloud-services-files/raw/temp/dsc.zip",
    "script": "configure-http.ps1",
    "function": "WebsiteTest"
  }
}
```

Potom spusťte tento příkaz rozhraní příkazového řádku Azure.

```azurecli
az vmss extension set --publisher Microsoft.Powershell --version 2.24 --name DSC --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

>[!IMPORTANT]
>Použití hello `--protected-settings` přepínač pro všechna nastavení, která mohou obsahovat citlivé informace.

### <a name="linux"></a>Linux

Linux můžete použít buď hello **vlastní skript v2.0** rozšíření nebo použijte **cloudu init** během vytváření.

Vlastní skript je jednoduchý rozšíření, která stáhne instancí virtuálního počítače toohello soubory a spustí příkaz.

#### <a name="custom-script"></a>Vlastní skript

Uložte následující soubor json jako hello _settings.json_.

```json
{
  "fileUris": [
    "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/installserver.sh",
    "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/workserver.py"
  ],
  "commandToExecute": "bash installserver.sh"
}
```

Pomocí rozhraní příkazového řádku Azure tooadd hello toto rozšíření tooan existující škálovací sadu virtuálních počítačů. Každý virtuální počítač ve škálovací hello nastavit automaticky spustí hello rozšíření.

```azurecli
az vmss extension set --publisher Microsoft.Azure.Extensions --version 2.0 --name CustomScript --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

>[!IMPORTANT]
>Použití hello `--protected-settings` přepínač pro všechna nastavení, která mohou obsahovat citlivé informace.

#### <a name="cloud-init"></a>Init cloudu

Init cloudu se používá, když se vytvoří sada škálování hello. Nejprve vytvořte místní soubor s názvem _cloudu init.txt_ a přidejte tooit vaší konfigurace. Například v tématu [tento gist](https://gist.github.com/Thraka/27bd66b1fb79e11904fb62b7de08a8a6#file-cloud-init-txt)

Nastavit hello pomocí rozhraní příkazového řádku Azure toocreate škálování. Hello `--custom-data` pole lze zadat název souboru hello cloudu init skriptu.

```azurecli
az vmss create \
  --resource-group myResourceGroupScaleSet \
  --name myScaleSet \
  --image Canonical:UbuntuServer:14.04.4-LTS:latest \
  --upgrade-policy-mode automatic \
  --custom-data cloud-init.txt \
  --admin-username azureuser \
  --generate-ssh-keys      
```

## <a name="how-do-i-manage-application-updates"></a>Jak lze spravovat aktualizace aplikace?

Pokud jste nasadili aplikaci prostřednictvím rozšíření, změnit definice rozšíření hello nějakým způsobem. Tato změna způsobí, že instance virtuálních počítačů pro tooall hello rozšíření toobe znovu nasazena. Něco **musí** změnit o hello rozšíření, jako je například přejmenování odkazované souboru, jinak, Azure nemá došlo ke změně není najdete, který hello rozšíření.

Pokud jste zaručená aplikace hello do vlastní bitovou kopii operačního systému, použijte kanál automatického nasazení aktualizací aplikací. Návrh vaší architektury toofacilitate rychlé odkládací dvoufázové instalace měřítka nastavit do produkčního prostředí. Dobrým příkladem tohoto přístupu je hello [Azure Spinnaker ovladač pracovní](https://github.com/spinnaker/deck/tree/master/app/scripts/modules/azure) - [http://www.spinnaker.io/](http://www.spinnaker.io/).

[Balírna](https://www.packer.io/) a [Terraform](https://www.terraform.io/) podpory Azure Resource Manager, tak můžete také definovat obrázků "jako kód" a sestavení je v Azure, pak použít hello virtuální pevný disk ve škálovací sadě. Ale to proto by se stal problematické pro Image marketplace, kde rozšíření nebo vlastní skripty se jsou důležitější vzhledem k tomu, že nemáte přímo upravit bits z webu marketplace.

## <a name="what-happens-when-a-scale-set-scales-out"></a>Co se stane, když sady měřítek škálování se?
Při přidání jedné nebo více virtuálních počítačů tooa škálovací sadu aplikace hello je automaticky nainstalována. Pro příklad nastaveného hello škálování má rozšíření definovaná, poběží na novém virtuálním počítači pokaždé, když je vytvořena. Pokud hello škálovací sada je založená na vlastní image, všechny nové virtuální počítač je kopii hello zdroj vlastní image. Pokud hello škálovací sadu virtuálních počítačů jsou hostitelé kontejneru, může být spuštění kódu tooload hello kontejnery v rozšíření vlastních skriptů. Nebo rozšíření může nainstalovat agenta, který registruje clusteru orchestrator, jako je například Azure Container Service.


## <a name="how-do-you-roll-out-an-os-update-across-update-domains"></a>Jak budete zavádět aktualizaci operačního systému napříč doménami aktualizace?
Předpokládejme, že chcete bitové kopie operačního systému pro tooupdate zachováním škálování virtuálních počítačů hello nastavit systémem. Prostředí PowerShell a hello rozhraní příkazového řádku Azure můžete aktualizovat hello Image virtuálního počítače, jeden virtuální počítač najednou. Hello [upgradovat sadu škálování virtuálního počítače](./virtual-machine-scale-sets-upgrade-scale-set.md) článek obsahuje také další informace o jaké možnosti jsou k dispozici tooperform operační systém upgradovat napříč škálovací sadu virtuálních počítačů.

## <a name="next-steps"></a>Další kroky

* [Pomocí prostředí PowerShell toomanage škálovací sadu.](virtual-machine-scale-sets-windows-manage.md)
* [Vytvořte šablonu sady škálování.](virtual-machine-scale-sets-mvss-start.md)


[linux-vm-create]: ../virtual-machines/linux/tutorial-manage-vm.md
[windows-vm-create]: ../virtual-machines/windows/tutorial-manage-vm.md
[linux-vm-capture]: ../virtual-machines/linux/capture-image.md
[windows-vm-capture]: ../virtual-machines/windows/capture-image.md 
[vmss-create]: virtual-machine-scale-sets-create.md

