---
title: "aaaDesired stavu konfigurace pro přehled Azure | Microsoft Docs"
description: "Přehled pro používání hello Microsoft Azure rozšíření obslužné rutiny pro konfigurace požadovaného stavu prostředí PowerShell. Včetně požadavky, architektura, rutiny..."
services: virtual-machines-windows
documentationcenter: 
author: zjalexander
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
keywords: 
ms.assetid: bbacbc93-1e7b-4611-a3ec-e3320641f9ba
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 01/09/2017
ms.author: zachal
ms.openlocfilehash: b0337a2f1124f35e5e40c1478bd7530427e59d44
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toohello-azure-desired-state-configuration-extension-handler"></a>Obslužná rutina rozšíření Úvod toohello konfigurace požadovaného stavu Azure
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Hello agenta virtuálního počítače Azure a související rozšíření jsou součástí hello infrastrukturní služby Microsoft Azure. Rozšíření virtuálního počítače jsou softwarové komponenty, které rozšiřovat funkce virtuálního počítače hello a zjednodušit různé operace správy virtuálních počítačů. Například může být použité tooreset hesla správce hello rozšíření VMAccess, nebo hello vlastní skript rozšíření lze použít tooexecute skript na hello virtuálních počítačů.

Tento článek představuje hello rozšíření prostředí PowerShell požadovaného stavu konfigurace (DSC) pro virtuální počítače Azure v rámci hello Azure PowerShell SDK. Můžete použít nové rutiny tooupload a použití konfigurace DSC prostředí PowerShell ve virtuálním počítači Azure s hello rozšíření DSC prostředí PowerShell povolené. volání rozšíření DSC Powershellu Hello do prostředí PowerShell DSC tooenact hello přijal konfiguraci DSC na hello virtuálních počítačů. Tato funkce je také k dispozici prostřednictvím hello portálu Azure.

## <a name="prerequisites"></a>Požadavky
**Místní počítač** toointeract s hello rozšíření virtuálního počítače Azure, třeba toouse buď hello portál Azure nebo Azure PowerShell SDK hello. 

**Agent hosta** hello virtuálního počítače Azure, který je nakonfigurovaný hello DSC konfigurace musí toobe operační systém, který podporuje buď Windows Management Framework (WMF) 4.0 nebo 5.0. Hello úplný seznam podporovaných verzí operačního systému najdete na hello [historie verzí rozšíření DSC](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/).

## <a name="terms-and-concepts"></a>Podmínky a koncepty
Tato příručka předpokládá znalost hello následující koncepty:

Konfigurace – dokumentu konfigurace DSC. 

Uzel - cíle pro konfigurace DSC. V tomto dokumentu odkazuje "uzel" vždy tooan virtuálního počítače Azure.

Konfigurační Data - .psd1 soubor obsahující data prostředí pro konfiguraci

## <a name="architectural-overview"></a>Přehled architektury
Hello rozšíření Azure DSC používá toodeliver framework hello agenta virtuálního počítače Azure, uplatní a ohlásí o konfiguracích DSC běžící na virtuálních počítačích Azure. Hello rozšíření DSC očekává soubor ZIP obsahující alespoň dokumentu konfigurace a sadu parametrů poskytuje prostřednictvím hello Azure PowerShell SDK nebo prostřednictvím hello portálu Azure.

Při rozšíření hello volání pro hello poprvé, spustí instalační proces. Tento proces nainstaluje verzi hello Windows Management Framework (WMF) pomocí následujících logiku hello:

1. Pokud hello operačním systémem virtuálního počítače Azure se systémem Windows Server 2016, nebyla provedena žádná akce. Windows Server 2016 již hello nejnovější verzi prostředí PowerShell nainstalovaný.
2. Pokud hello `wmfVersion` je zadána vlastnost, je nainstalovaná této verze hello WMF, pokud není kompatibilní s operačním systémem hello Virtuálního počítače.
3. Pokud žádné `wmfVersion` vlastnost určena, hello nejnovější použít hello WMF je nainstalovaná verze.

Instalace hello WMF vyžaduje restart. Po restartování hello rozšíření stáhne soubor ZIP hello zadaný v hello `modulesUrl` vlastnost. Pokud toto umístění je v Azure blob storage, SAS token lze zadat v hello `sasToken` vlastnost tooaccess hello souboru. Po stažení a vybaleno hello .zip hello konfigurace funkci definovanou v `configurationFunction` spuštění souboru MOF toogenerate hello. pak spustí Hello rozšíření `Start-DscConfiguration -Force` v souboru MOF hello vygenerovat. rozšíření Hello zaznamená výstup a zapíše zpět na toohello kanál stavu Azure. Z tohoto bodu na hello DSC LCM zpracovává monitorování a oprava jako normální. 

## <a name="powershell-cmdlets"></a>Rutiny prostředí PowerShell
Rutiny prostředí PowerShell je možné pomocí Azure Resource Manageru nebo hello toopackage modelu nasazení classic, publikovat a monitorování nasazení rozšíření DSC. Hello jsou následující rutiny uvedené moduly hello nasazení classic, ale "Azure" lze nahradit pomocí modelu "AzureRm" toouse hello Azure Resource Manager. Například `Publish-AzureVMDscConfiguration` používá hello modelu nasazení classic, kde `Publish-AzureRmVMDscConfiguration` používá Azure Resource Manager. 

`Publish-AzureVMDscConfiguration`trvá v konfiguračním souboru, hledá závislé prostředky DSC a vytvoří soubor ZIP obsahující hello konfigurace a konfigurace DSC prostředky potřebné tooenact hello. Můžete také vytvořit balíček hello místně pomocí hello `-ConfigurationArchivePath` parametr. V opačném případě publikuje úložiště objektů blob tooAzure soubor .zip hello a zabezpečuje s tokenem SAS.

soubor ZIP Hello vytvořených touto rutinou má hello .ps1 konfigurační skript v hello kořenové složky archivu hello. Prostředky mít složku modulu hello umístěny ve složce archivu hello. 

`Set-AzureVMDscExtension`Vloží hello nastavení vyžadovaná hello rozšíření DSC prostředí PowerShell do objekt konfigurace virtuálního počítače. V modelu nasazení classic hello hello změny virtuálního počítače musí být použité tooan virtuální počítač Azure s `Update-AzureVM`. 

`Get-AzureVMDscExtension`načte stav rozšíření DSC hello konkrétní virtuálního počítače. 

`Get-AzureVMDscExtensionStatus`načte hello stav konfigurace DSC hello vydat hello DSC rozšíření obslužnou rutinou. Tuto akci lze provést na jeden virtuální počítač nebo skupinu virtuálních počítačů.

`Remove-AzureVMDscExtension`Odebere obslužnou rutinu rozšíření hello z daného virtuálního počítače. Tato rutina nemá **není** odebrat konfiguraci hello, odinstalovat hello WMF nebo změnit nastavení hello použita na virtuálním počítači hello. Odebere pouze hello rozšíření obslužné rutiny. 

**Hlavní rozdíly ve rutiny ASM a Azure Resource Manager**

* Rutiny Azure Resource Manager jsou synchronní. ASM rutiny jsou asynchronní.
* Název skupiny prostředků, VMName, ArchiveStorageAccountName, verzi a umístění jsou všechny požadované parametry ve službě Správce prostředků Azure.
* ArchiveResourceGroupName je nový volitelný parametr pro Azure Resource Manager. Tento parametr můžete zadat, když váš účet úložiště patří tooa jiné skupině prostředků než hello jeden, kde se má vytvořit hello virtuálního počítače.
* ConfigurationArchive nazývá ArchiveBlobName ve službě Správce prostředků Azure
* ContainerName nazývá ArchiveContainerName ve službě Správce prostředků Azure
* StorageEndpointSuffix nazývá ArchiveStorageEndpointSuffix ve službě Správce prostředků Azure
* přepínač Hello automatických aktualizací byla přidána tooAzure Resource Manager tooenable automatické aktualizace hello obslužná rutina toohello nejnovější verze rozšíření jako a v případě, že je k dispozici. Všimněte si, že tento parametr má hello potenciální toocause restartování na hello virtuální počítač při hello WMF vydání nové verze. 

## <a name="azure-portal-functionality"></a>Funkce Azure portálu
Procházejte tooa virtuálních počítačů. V části Nastavení -> Obecné klikněte na tlačítko "Rozšíření". Vytvoří se nové podokno. Klikněte na tlačítko "Přidat" a vyberte DSC prostředí PowerShell.

portál Hello vyžaduje vstup.
**Konfigurace moduly nebo skriptu**: Toto pole je povinné. Vyžaduje souboru s příponou .ps1 obsahující konfigurační skript, nebo soubor ZIP s konfigurační skript .ps1 hello kořenové a všechny závislé zdroje ve složkách modul v rámci hello .zip. Vytvořením s hello `Publish-AzureVMDscConfiguration -ConfigurationArchivePath` rutiny, které jsou součástí hello Azure PowerShell SDK. soubor ZIP Hello nahraje do úložiště objektů blob uživatele zabezpečeny SAS token. 

**Soubor konfiguračních dat PSD1**: Toto pole je nepovinné. Pokud vaše konfigurace vyžaduje datový soubor konfigurace v .psd1, použijte toto pole tooselect ho a nahrajte ho tooyour úložiště objektů blob uživatele, kde je zabezpečená službou SAS token. 

**Modul kvalifikovaný název konfigurace**: soubory .ps1 může mít několik konfiguračních funkcí. Zadejte název hello hello konfigurace .ps1 skriptu následuje '\' a hello název funkce Konfigurace hello. Například pokud má váš skript .ps1 hello název "configuration.ps1" a konfigurace hello je "IisInstall", můžete zadáním:`configuration.ps1\IisInstall`

**Konfigurace argumentů**: Pokud hello konfigurace funkce přijímá argumenty, zadejte je sem ve formátu hello `argumentName1=value1,argumentName2=value2`. Všimněte si, že tento formát je do jiného formátu než jak konfigurace argumenty jsou přijímány prostřednictvím rutin prostředí PowerShell nebo šablony Resource Manageru. 

## <a name="getting-started"></a>Začínáme
Hello rozšíření Azure DSC trvá v dokumentech konfigurace DSC a představuje je na virtuálních počítačích Azure. Následuje jednoduchý příklad konfigurace. Uložte místně jako "IisInstall.ps1":

```powershell
configuration IISInstall 
{ 
    node "localhost"
    { 
        WindowsFeature IIS 
        { 
            Ensure = "Present" 
            Name = "Web-Server"                       
        } 
    } 
}
```

Následující kroky místní hello IisInstall.ps1 skriptu na hello Hello zadaný virtuální počítač, provedení konfigurace hello a zpětně hlásit stav.
###<a name="classic-model"></a>Klasického modelu
```powershell
#Azure PowerShell cmdlets are required
Import-Module Azure

#Use an existing Azure Virtual Machine, 'DscDemo1'
$demoVM = Get-AzureVM DscDemo1

#Publish hello configuration script into user storage.
Publish-AzureVMDscConfiguration -ConfigurationPath ".\IisInstall.ps1" -StorageContext $storageContext -Verbose -Force

#Set hello VM toorun hello DSC configuration
Set-AzureVMDscExtension -VM $demoVM -ConfigurationArchive "IisInstall.ps1.zip" -StorageContext $storageContext -ConfigurationName "IisInstall" -Verbose

#Update hello configuration of an Azure Virtual Machine
$demoVM | Update-AzureVM -Verbose

#check on status
Get-AzureVMDscExtensionStatus -VM $demovm -Verbose
```
###<a name="azure-resource-manager-model"></a>Azure modelu Resource Manager

```powershell
$resourceGroup = "dscVmDemo"
$location = "westus"
$vmName = "myVM"
$storageName = "demostorage"
#Publish hello configuration script into user storage
Publish-AzureRmVMDscConfiguration -ConfigurationPath .\iisInstall.ps1 -ResourceGroupName $resourceGroup -StorageAccountName $storageName -force
#Set hello VM toorun hello DSC configuration
Set-AzureRmVmDscExtension -Version 2.21 -ResourceGroupName $resourceGroup -VMName $vmName -ArchiveStorageAccountName $storageName -ArchiveBlobName iisInstall.ps1.zip -AutoUpdate:$true -ConfigurationName "IISInstall"

```

## <a name="logging"></a>Protokolování
Protokoly jsou umístěny ve:

C:\WindowsAzure\Logs\Plugins\Microsoft.PowerShell.DSC\[číslo verze]

## <a name="next-steps"></a>Další kroky
Další informace o DSC Powershellu [navštivte centru dokumentace prostředí PowerShell hello](https://msdn.microsoft.com/powershell/dsc/overview). 

Zkontrolujte hello [šablony Azure Resource Manageru pro rozšíření hello DSC](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

toofind další funkce, které můžete spravovat pomocí prostředí PowerShell DSC [procházet galerii prostředí PowerShell hello](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) pro další prostředky DSC.

Podrobnosti o předávání citlivých parametry do konfigurace najdete v tématu [spravovat pověření bezpečně s obslužnou rutinou rozšíření hello DSC](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

