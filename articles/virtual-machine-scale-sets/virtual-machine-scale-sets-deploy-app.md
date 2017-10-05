---
title: "Nasazení aplikace na sady škálování virtuálního počítače"
description: "Rozšíření do depoy aplikace použijte na sady škálování virtuálního počítače Azure."
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
ms.openlocfilehash: fa7d9d3bef4cb326844ede76171e8c566e87116b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-your-application-on-virtual-machine-scale-sets"></a>Nasazení aplikace na sady škálování virtuálního počítače

Tento článek popisuje různé způsoby instalace softwaru v době byly sadou škálování je zřízený.

Můžete zkontrolovat [přehled návrhu nastavení škálování](virtual-machine-scale-sets-design-overview.md) článek, který popisuje některé z omezení, která sady škálování virtuálního počítače.

## <a name="capture-and-reuse-an-image"></a>Zaznamenání a znovu použít bitovou kopii

Můžete vytvořit virtuální počítač nastavené v Azure a připravit bitovou kopii základní pro vaši škálování. Tento proces vytvoří ve vašem účtu úložiště, které můžete použít jako základní bitovou kopii pro škálovací sadu spravovaných disků. 

Proveďte následující kroky:

1. Vytvořit virtuální počítač Azure
   * [Linux][linux-vm-create]
   * [Windows][windows-vm-create]

2. Vzdálené do virtuálního počítače a přizpůsobení systému libovolně.

   Pokud chcete, můžete nainstalovat aplikace hned. Však vědět, že nainstalováním aplikace teď, může být upgrade vaší aplikace složitější, protože je nutné nejprve odeberte. Místo toho můžete tento krok nainstalovat všechny požadované součásti, které aplikace může vyžadovat, podobně jako u konkrétní funkce runtime nebo operačního systému.

3. Postupujte podle kurzu "zachytit počítač" pro buď [Linux] [ linux-vm-capture] nebo [Windows][windows-vm-capture].

4. Vytvoření [sadu škálování virtuálního počítače] [ vmss-create] s bitovou kopií URI zachytili v předchozím kroku.

Další informace o discích najdete v tématu [přehled disky spravované](../virtual-machines/windows/managed-disks-overview.md) a [datové disky připojené k použití](virtual-machine-scale-sets-attached-disks.md).

## <a name="install-when-the-scale-set-is-provisioned"></a>Nainstaluje, když byly sadou škálování je zřízený.

Rozšíření virtuálního počítače je použít pro škálovací sadu virtuálních počítačů. S rozšířením virtuálního počítače můžete přizpůsobit virtuálních počítačů ve škále nastavit jako pro celou skupinu. Další informace o rozšířeních najdete v tématu [rozšíření virtuálního počítače](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Existují tři hlavní rozšíření, které můžete použít, podle toho, pokud váš operační systém se systémem Linux nebo systému Windows.

### <a name="windows"></a>Windows

Pro operační systém na základě systému Windows, použijte buď **vlastní skript v1.8** rozšíření, nebo **DSC Powershellu** rozšíření.

#### <a name="custom-script"></a>Vlastní skript

Rozšíření vlastních skriptů spouští skript na každou instanci virtuálního počítače v sadě škálování. Konfigurační soubor nebo proměnná označuje soubory, které se stáhnou do virtuálního počítače, a poté co příkaz spustí. Můžete to použít ke spuštění instalačního programu, skript, dávkového souboru, jakéhokoliv spustitelného souboru, třeba.

Zatřiďovací tabulku používá prostředí PowerShell pro nastavení. Tento příklad konfiguruje rozšíření vlastních skriptů spustit skript prostředí PowerShell, který nainstaluje službu IIS.

```powershell
# Setup extension configuration hashtable variable
$customConfig = @{
  "fileUris" = @("https://raw.githubusercontent.com/MicrosoftDocs/azure-cloud-services-files/temp/install-iis.ps1");
  "commandToExecute" = "PowerShell -ExecutionPolicy Unrestricted .\install-iis.ps1 >> `"%TEMP%\StartupLog.txt`" 2>&1";
};

# Add the extension to the config
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig -Publisher Microsoft.Compute -Type CustomScriptExtension -TypeHandlerVersion 1.8 -Name "customscript1" -Setting $customConfig

# Send the new config to Azure
Update-AzureRmVmss -ResourceGroupName $rg -Name "MyVmssTest143"  -VirtualMachineScaleSet $vmssConfig
```

>[!IMPORTANT]
>Použití `-ProtectedSetting` přepínač pro všechna nastavení, která mohou obsahovat citlivé informace.

---------


Rozhraní příkazového řádku Azure používá soubor json pro nastavení. Tento příklad konfiguruje rozšíření vlastních skriptů spustit skript prostředí PowerShell, který nainstaluje službu IIS. Uložte následující soubor json jako _settings.json_.

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
>Použití `--protected-settings` přepínač pro všechna nastavení, která mohou obsahovat citlivé informace.

### <a name="powershell-dsc"></a>Prostředí PowerShell DSC

DSC prostředí PowerShell můžete použít k přizpůsobení instance škálovací sady virtuálních počítačů. **DSC** rozšíření, které zveřejnil **Microsoft.Powershell** nasadí a běží Zadaná konfigurace DSC na každou instanci virtuálního počítače. Konfiguračním souboru nebo proměnná informuje rozšíření kde *.zip* balíček je a který _funkce skriptu_ kombinace ke spuštění.

Zatřiďovací tabulku používá prostředí PowerShell pro nastavení. Tento příklad nasadí DSC balíček, který nainstaluje službu IIS.

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

# Add the extension to the config
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig -Publisher Microsoft.Powershell -Type DSC -TypeHandlerVersion 2.24 -Name "dsc1" -Setting $dscConfig

# Send the new config to Azure
Update-AzureRmVmss -ResourceGroupName $rg -Name "myscaleset1"  -VirtualMachineScaleSet $vmssConfig
```

>[!IMPORTANT]
>Použití `-ProtectedSetting` přepínač pro všechna nastavení, která mohou obsahovat citlivé informace.

-----------

Rozhraní příkazového řádku Azure používá json pro nastavení. Tento příklad nasadí DSC balíček, který nainstaluje službu IIS. Uložte následující soubor json jako _settings.json_.

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
>Použití `--protected-settings` přepínač pro všechna nastavení, která mohou obsahovat citlivé informace.

### <a name="linux"></a>Linux

Linux můžete použít buď **vlastní skript v2.0** rozšíření nebo použijte **cloudu init** během vytváření.

Vlastní skript je jednoduchý rozšíření, které se stahování souborů do instance virtuálních počítačů a spustí příkaz.

#### <a name="custom-script"></a>Vlastní skript

Uložte následující soubor json jako _settings.json_.

```json
{
  "fileUris": [
    "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/installserver.sh",
    "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/workserver.py"
  ],
  "commandToExecute": "bash installserver.sh"
}
```

Pomocí rozhraní příkazového řádku Azure přidáte toto rozšíření do existující sady škálování virtuálního počítače. Každý virtuální počítač ve škálovací nastavit automaticky spustí rozšíření.

```azurecli
az vmss extension set --publisher Microsoft.Azure.Extensions --version 2.0 --name CustomScript --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

>[!IMPORTANT]
>Použití `--protected-settings` přepínač pro všechna nastavení, která mohou obsahovat citlivé informace.

#### <a name="cloud-init"></a>Init cloudu

Init cloudu se používá, když se vytvoří sada škálování. Nejprve vytvořte místní soubor s názvem _cloudu init.txt_ a přidejte do ní konfiguraci. Například v tématu [tento gist](https://gist.github.com/Thraka/27bd66b1fb79e11904fb62b7de08a8a6#file-cloud-init-txt)

Pomocí rozhraní příkazového řádku Azure k vytvoření sady škálování. `--custom-data` Pole lze zadat název souboru skriptu init cloudu.

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

Pokud jste nasadili aplikaci prostřednictvím rozšíření, změnit definici rozšíření nějakým způsobem. Tato změna způsobí, že rozšíření znovu nasadit na všechny instance virtuálního počítače. Něco **musí** změnit o rozšíření, jako je například přejmenování odkazované souboru, jinak, Azure nemá není najdete, které se změnily rozšíření.

Pokud jste zaručená aplikace do vlastní bitovou kopii operačního systému, použijte kanál automatického nasazení aktualizací aplikací. Návrh vaší architektury pro usnadnění rychlé odkládací dvoufázové instalace měřítka nastavit do produkčního prostředí. Dobrým příkladem tohoto přístupu je [Azure Spinnaker ovladač pracovní](https://github.com/spinnaker/deck/tree/master/app/scripts/modules/azure) - [http://www.spinnaker.io/](http://www.spinnaker.io/).

[Balírna](https://www.packer.io/) a [Terraform](https://www.terraform.io/) podpory Azure Resource Manager, tak můžete také definovat obrázků "jako kód" a sestavení je v Azure, pak použít virtuální pevný disk ve škálovací sadě. Ale to proto by se stal problematické pro Image marketplace, kde rozšíření nebo vlastní skripty se jsou důležitější vzhledem k tomu, že nemáte přímo upravit bits z webu marketplace.

## <a name="what-happens-when-a-scale-set-scales-out"></a>Co se stane, když sady měřítek škálování se?
Při přidání jedné nebo více virtuálních počítačů k sadě škálování, je automaticky nainstalována aplikace. Pro příklad nastaveného měřítka má rozšíření definovaná, poběží na novém virtuálním počítači pokaždé, když je vytvořena. Pokud byly sadou škálování je založená na vlastní image, všechny nové virtuální počítač je kopie zdrojové vlastní Image. Pokud virtuální počítače sady škálování hostitele kontejneru, může mít kód spuštění načíst kontejnery v rozšíření vlastních skriptů. Nebo rozšíření může nainstalovat agenta, který registruje clusteru orchestrator, jako je například Azure Container Service.


## <a name="how-do-you-roll-out-an-os-update-across-update-domains"></a>Jak budete zavádět aktualizaci operačního systému napříč doménami aktualizace?
Předpokládejme, že chcete aktualizaci bitové kopie operačního systému a zachovat přitom sad systémem škálování virtuálního počítače. Prostředí PowerShell a rozhraní příkazového řádku Azure můžete aktualizovat Image virtuálního počítače, jeden virtuální počítač najednou. [Upgradovat sadu škálování virtuálního počítače](./virtual-machine-scale-sets-upgrade-scale-set.md) článek obsahuje také další informace o jaké možnosti jsou dostupné k provedení upgradu operačního systému přes škálovací sadu virtuálních počítačů.

## <a name="next-steps"></a>Další kroky

* [Použití Powershellu ke správě škálovací sadu.](virtual-machine-scale-sets-windows-manage.md)
* [Vytvořte šablonu sady škálování.](virtual-machine-scale-sets-mvss-start.md)


[linux-vm-create]: ../virtual-machines/linux/tutorial-manage-vm.md
[windows-vm-create]: ../virtual-machines/windows/tutorial-manage-vm.md
[linux-vm-capture]: ../virtual-machines/linux/capture-image.md
[windows-vm-capture]: ../virtual-machines/windows/capture-image.md 
[vmss-create]: virtual-machine-scale-sets-create.md

