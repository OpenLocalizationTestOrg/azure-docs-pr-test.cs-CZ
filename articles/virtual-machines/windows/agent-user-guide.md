---
title: "aaaAzure virtuální počítač agenta přehled | Microsoft Docs"
description: "Přehled agenta virtuálního počítače Azure"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 0a1f212e-053e-4a39-9910-8d622959f594
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 11/17/2016
ms.author: nepeters
ms.openlocfilehash: b03542b9a9c711000fab18ed82e9b17ee5510bbf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-virtual-machine-agent-overview"></a>Přehled služby Azure agenta virtuálního počítače

Hello agenta virtuálního počítače Microsoft Azure (AM Agent) je zabezpečené, lightweight proces, který spravuje interakci virtuálních počítačů s hello Kontroleru prostředků infrastruktury Azure. Hello agenta virtuálního počítače má primární roli v povolení a spuštění rozšíření virtuálního počítače Azure. Konfigurace nasazení virtuálních počítačů, jako je instalace a konfigurace softwaru příspěvku povolení rozšíření virtuálního počítače. Rozšíření virtuálního počítače také povolit obnovení funkce jako je resetování hesla pro správu hello virtuálního počítače. Bez hello agenta virtuálního počítače Azure nelze spustit, rozšíření virtuálního počítače.

Tento dokument podrobně popisuje instalaci, zjištění a odebrání hello agenta virtuálního počítače Azure.

## <a name="install-hello-vm-agent"></a>Nainstalujte hello agenta virtuálního počítače

### <a name="azure-gallery-image"></a>Obrázek v galerii Azure

Hello agenta virtuálního počítače Azure je nainstalován na všechny nasazené z Galerie Azure bitové kopie virtuálního počítače Windows ve výchozím nastavení. Při nasazování bitové kopie Galerie Azure z hello portál, prostředí PowerShell, rozhraní příkazového řádku nebo šablonu Azure Resource Manager, hello agenta virtuálního počítače Azure se taky nainstalovat. 

### <a name="manual-installation"></a>Ruční instalace

agent virtuálního počítače Windows Hello lze ručně nainstalovat pomocí balíčku Instalační služby systému Windows. Ruční instalace může být nutné při vytvoření image vlastní virtuální počítač, který se nasadí v Azure. hello toomanually instalace agenta virtuálního počítače Windows, stažení instalačního programu hello agenta virtuálního počítače z tohoto umístění [stáhnout agenta virtuálního počítače Azure Windows](http://go.microsoft.com/fwlink/?LinkID=394789). 

dvakrát klikněte na soubor Instalační služby systému windows hello může být instalován Hello agenta virtuálního počítače. Pro automatizované nebo bezobslužné instalace agenta virtuálního počítače hello spusťte následující příkaz hello.

```cmd
msiexec.exe /i WindowsAzureVmAgent.2.7.1198.778.rd_art_stable.160617-1120.fre /quiet
```

## <a name="detect-hello-vm-agent"></a>Zjištění hello agenta virtuálního počítače

### <a name="powershell"></a>PowerShell

modul Powershellu pro Azure Resource Manager Hello lze použít tooretrieve informace o virtuálních počítačích Azure. Spuštění `Get-AzureRmVM` vrátí s bit informace včetně hello zřizování stav hello agenta virtuálního počítače Azure.

```PowerShell
Get-AzureRmVM
```

Hello následuje pouze podmnožinu hello `Get-AzureRmVM` výstup. Všimněte si hello `ProvisionVMAgent` vlastnost vnořit `OSProfile`, tato vlastnost může být použité toodetermine, pokud agent virtuálního počítače hello byl nasazený toohello virtuálního počítače.

```PowerShell
OSProfile                  :
  ComputerName             : myVM
  AdminUsername            : muUserName
  WindowsConfiguration     :
    ProvisionVMAgent       : True
    EnableAutomaticUpdates : True
```

Hello následující skript se dá použít tooreturn stručným seznam názvy virtuálních počítačů a stav hello hello agenta virtuálního počítače.

```PowerShell
$vms = Get-AzureRmVM

foreach ($vm in $vms) {
    $agent = $vm | Select -ExpandProperty OSProfile | Select -ExpandProperty Windowsconfiguration | Select ProvisionVMAgent
    Write-Host $vm.Name $agent.ProvisionVMAgent
}
```

### <a name="manual-detection"></a>Ruční detekce

Při přihlášení tooa virtuálního počítače Windows Azure, Správce úloh může být použité tooexamine spuštěných procesů. toocheck pro hello agenta virtuálního počítače Azure, otevřete Správce úloh > klikněte na kartu s podrobnostmi hello a vyhledejte název procesu `WindowsAzureGuestAgent.exe`. Hello přítomnost tento proces naznačuje, že je nainstalovaný agent virtuálního počítače tohoto hello.

## <a name="upgrade-hello-vm-agent"></a>Upgrade hello agenta virtuálního počítače

Hello Azure virtuální počítač agenta pro Windows je automaticky upgradován. Nové virtuální počítače jsou nasazené tooAzure, obdrží hello nejnovější agenta virtuálního počítače. Vlastní Image virtuálních počítačů musí být ručně aktualizovaných tooinclude hello nového virtuálního počítače agenta.
