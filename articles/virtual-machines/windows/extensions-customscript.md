---
title: "aaaAzure vlastní skript rozšíření pro Windows | Microsoft Docs"
description: "Automatizovat úkoly konfigurace virtuálního počítače s Windows pomocí rozšíření vlastních skriptů hello"
services: virtual-machines-windows
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f4181fee-7a9d-4a1c-b517-52956f5b7fa1
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/16/2017
ms.author: nepeters
ms.openlocfilehash: 97e065242e9fed116ee20b074f4e302a0cd10585
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="custom-script-extension-for-windows"></a>Rozšíření vlastních skriptů pro Windows

Hello rozšíření vlastních skriptů stahuje a spouští skripty na virtuálních počítačích Azure. Toto rozšíření je užitečné pro konfiguraci nasazení post, instalace softwaru nebo jakoukoli jinou konfiguraci, nebo úlohu správy. Skripty můžete stáhnout z úložiště Azure nebo GitHub nebo zadat toohello portál Azure na dobu běhu rozšíření. Hello rozšíření vlastních skriptů se integruje s šablon Azure Resource Manageru a můžete také spustit pomocí hello rozhraní příkazového řádku Azure, PowerShell, portálu Azure nebo hello REST API pro virtuální počítač Azure.

Tento dokument podrobně popisuje, jak pomocí rozšíření vlastních skriptů hello toouse hello modul Azure PowerShell, šablon Azure Resource Manageru a podrobnosti o řešení potíží s kroky v systémech Windows.

## <a name="prerequisites"></a>Požadavky

### <a name="operating-system"></a>Operační systém

Hello rozšíření vlastních skriptů pro pro Windows Server 2008 R2, můžete spouštět Windows 2012, 2012 R2 a 2016 uvolní.

### <a name="script-location"></a>Umístění skriptu

skript Hello musí toobe uložení do úložiště objektů Blob v Azure nebo jakéhokoli jiného umístění, které jsou přístupné prostřednictvím platnou adresu URL.

### <a name="internet-connectivity"></a>Připojení k Internetu

Hello vlastní skript rozšíření pro Windows vyžaduje, aby hello cílový virtuální počítač je připojený toohello Internetu. 

## <a name="extension-schema"></a>Rozšíření schématu

Hello následujícím kódu JSON ukazuje hello schéma pro hello rozšíření vlastních skriptů. rozšíření Hello vyžaduje umístění skriptu (Azure Storage nebo jiného umístění s platnou adresu URL) a příkaz tooexecute. Pokud používáte Azure Storage jako zdroj skriptu hello, je třeba klíč účtu úložiště Azure účet a název. Tyto položky by měl být považován za citlivá data a zadaný v konfiguraci chráněných nastavení rozšíření hello. Azure data virtuálního počítače chráněný rozšíření nastavení je zašifrovaná a dešifrovat jenom na hello cílového virtuálního počítače.

```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
        "[variables('musicstoresqlName')]"
    ],
    "tags": {
        "displayName": "config-app"
    },
    "properties": {
        "publisher": "Microsoft.Compute",
        "type": "CustomScriptExtension",
        "typeHandlerVersion": "1.9",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "fileUris": [
                "script location"
            ]
        },
        "protectedSettings": {
            "commandToExecute": "myExecutionCommand",
            "storageAccountName": "myStorageAccountName",
            "storageAccountKey": "myStorageAccountKey"
        }
    }
}
```

### <a name="property-values"></a>Hodnoty vlastností

| Name (Název) | Hodnota nebo příklad |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Vydavatele | Microsoft.Compute |
| type | Rozšíření |
| typeHandlerVersion | 1.9 |
| fileUris (např.) | https://RAW.githubusercontent.com/Microsoft/DotNet-Core-Sample-Templates/Master/DotNet-Core-Music-Windows/Scripts/Configure-Music-App.ps1 |
| commandToExecute (např.) | prostředí PowerShell - ExecutionPolicy Unrestricted - souboru konfigurace app.ps1 Hudba |
| storageAccountName (např.) | examplestorageacct |
| storageAccountKey (např.) | TmJK/1N3AbAZ3q / + hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg == |

**Poznámka:** -názvy těchto vlastností jsou velká a malá písmena. Pomocí názvů hello, jak je vidět výše tooavoid problémy při nasazení.

## <a name="template-deployment"></a>Nasazení šablon

Rozšíření virtuálního počítače Azure se dá nasadit pomocí šablon Azure Resource Manager. popsané v předchozí části hello schématu JSON Hello lze použít v toorun hello šablony Azure Resource Manager rozšíření vlastních skriptů při nasazení šablony Azure Resource Manager. Ukázka šablony, která obsahuje hello rozšíření vlastních skriptů je zde uveden, [Githubu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="powershell-deployment"></a>Nasazení prostředí PowerShell

Hello `Set-AzureRmVMCustomScriptExtension` příkaz lze použít tooadd hello vlastní skript rozšíření tooan existující virtuální počítač. Další informace najdete v tématu [Set-AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).
```powershell
Set-AzureRmVMCustomScriptExtension -ResourceGroupName myResourceGroup `
    -VMName myVM `
    -Location myLocation `
    -FileUri myURL `
    -Run 'myScript.ps1' `
    -Name DemoScriptExtension
```

## <a name="troubleshoot-and-support"></a>Řešení potíží a podpora

### <a name="troubleshoot"></a>Řešení potíží

Data o stavu hello nasazení rozšíření mohou být načteny z hello portál Azure a pomocí modulu Azure PowerShell hello. Stav nasazení toosee hello rozšíření pro daný virtuální počítač, spusťte následující příkaz hello.

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Rozšíření spuštění výstupu je zaznamenané toofiles v hello následující adresáře na hello cílového virtuálního počítače.
```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

Hello zadat, že soubory se stahují do hello následující adresáře na hello cílového virtuálního počítače.
```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads\<n>
```
kde `<n>` je desítkové celé číslo, které může změnit mezi jednotlivými spuštěními hello rozšíření.  Hello `1.*` hodnota odpovídá skutečným, aktuální hello `typeHandlerVersion` hodnota hello rozšíření.  Například může být hello skutečný adresář `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2`.  

Při provádění hello `commandToExecute` příkaz hello rozšíření bude nastavili tento adresář (například `...\Downloads\2`) jako hello aktuální pracovní adresář. Tato umožňuje hello použít relativní cesty toolocate hello souborů stažené prostřednictvím hello `fileURIs` vlastnost. Viz následující příklady tabulce hello.

Vzhledem k tomu, že hello stažení absolutní cesta se může lišit v čase je lepší tooopt pro relativní skriptu nebo cesty k souboru v hello `commandToExecute` řetězce, kdykoli je to možné. Například:
```json
    "commandToExecute": "powershell.exe . . . -File './scripts/myscript.ps1'"
```

Informace o cestě po první segment identifikátoru URI hello se zachovává kvůli soubory stáhli prostřednictvím hello `fileUris` seznam vlastností.  Jak ukazuje následující tabulka hello, stažené soubory jsou mapované na stažení podadresáře tooreflect hello struktura hello `fileUris` hodnoty.  

#### <a name="examples-of-downloaded-files"></a>Příklady stažené soubory

| Identifikátor URI v fileUris | Relativní umístění stažené | Absolutní stáhli umístění * |
| ---- | ------- |:--- |
| `https://someAcct.blob.core.windows.net/aContainer/scripts/myscript.ps1` | `./scripts/myscript.ps1` |`C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\scripts\myscript.ps1`  |
| `https://someAcct.blob.core.windows.net/aContainer/topLevel.ps1` | `./topLevel.ps1` | `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\topLevel.ps1` |

\*Jako výš, hello absolutní adresářové cesty se změní průběhu životnosti hello hello virtuálních počítačů, ale není v rámci jednoho spuštění rozšíření CustomScript hello.

### <a name="support"></a>Podpora

Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na hello Azure odborníky na hello [fórech MSDN Azure a Stack Overflow] (https://azure.microsoft.com/en-us/support/forums/). Alternativně můžete soubor incidentu podpory Azure. Přejděte toohello [podporu Azure lokality](https://azure.microsoft.com/en-us/support/options/) a vyberte Get podpory. Informace o používání Azure podporovat, najdete v tématu hello [podporu Microsoft Azure – nejčastější dotazy](https://azure.microsoft.com/en-us/support/faq/).
