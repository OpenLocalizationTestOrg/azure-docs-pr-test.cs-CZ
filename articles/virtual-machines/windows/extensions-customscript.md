---
title: "Rozšíření Azure vlastních skriptů pro Windows | Microsoft Docs"
description: "Automatizovat úkoly konfigurace virtuálního počítače s Windows pomocí rozšíření vlastních skriptů"
services: virtual-machines-windows
documentationcenter: 
author: danielsollondon
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
ms.author: danis
ms.openlocfilehash: 9a8eae62d2dcb4c422b707909a27c84c7bf1aab3
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/04/2018
---
# <a name="custom-script-extension-for-windows"></a>Rozšíření vlastních skriptů pro Windows

Rozšíření vlastních skriptů stahuje a spouští skripty na virtuálních počítačích Azure. Toto rozšíření je užitečné pro konfiguraci nasazení post, instalace softwaru nebo jakoukoli jinou konfiguraci, nebo úlohu správy. Skripty můžete stáhnout z úložiště Azure nebo GitHub nebo zadat na portál Azure na dobu běhu rozšíření. Rozšíření vlastních skriptů se integruje s šablon Azure Resource Manageru a můžete také spustit pomocí rozhraní příkazového řádku Azure, PowerShell, portálu Azure nebo REST API pro virtuální počítač Azure.

Tento dokument podrobně popisuje postup používání rozšíření vlastních skriptů pomocí modulu Azure PowerShell, šablon Azure Resource Manageru a podrobnosti o řešení potíží s kroky v systémech Windows.

## <a name="prerequisites"></a>Požadavky

> [!NOTE]  
> Vzhledem k tomu, že bude čekat na sobě spustit aktualizaci-AzureRmVM s stejného virtuálního počítače jako jeho parametr nepoužívejte rozšíření vlastních skriptů.  
>   
> 

### <a name="operating-system"></a>Operační systém

Spuštěním skriptu rozšíření vlastní pro systém Windows pro klienta Windows 10, Windows Server 2008 R2, 2012, 2012 R2 a 2016 uvolní.

### <a name="script-location"></a>Umístění skriptu

Skript musí být uložen do úložiště objektů Blob v Azure, nebo jakéhokoli jiného umístění, které jsou přístupné prostřednictvím platnou adresu URL.

### <a name="internet-connectivity"></a>Připojení k Internetu

Vlastní skript rozšíření pro Windows vyžaduje, aby cílový virtuální počítač je připojený k Internetu. 

## <a name="extension-schema"></a>Rozšíření schématu

Následujícím kódu JSON znázorňuje schéma pro rozšíření vlastních skriptů. Rozšíření vyžaduje umístění skriptu (Azure Storage nebo jiného umístění s platnou adresu URL) a příkaz ke spuštění. Pokud používáte Azure Storage jako zdroj skriptu, je třeba klíč účtu úložiště Azure účet a název. Tyto položky by měl být považován za citlivá data a zadaný v konfiguraci chráněných nastavení rozšíření. Data Azure nastavení rozšíření chráněný virtuální počítač je zašifrovaná a dešifrovat jenom na cílový virtuální počítač.

```json
{
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Compute/virtualMachines/extensions",
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

| Název | Hodnota nebo příklad |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Vydavatele | Microsoft.Compute |
| type | Rozšíření |
| typeHandlerVersion | 1.9 |
| fileUris (např.) | https://RAW.githubusercontent.com/Microsoft/DotNet-Core-Sample-Templates/Master/DotNet-Core-Music-Windows/Scripts/Configure-Music-App.ps1 |
| commandToExecute (např.) | prostředí PowerShell - ExecutionPolicy Unrestricted - souboru konfigurace app.ps1 Hudba |
| storageAccountName (např.) | examplestorageacct |
| storageAccountKey (např.) | TmJK/1N3AbAZ3q / + hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg == |

**Poznámka:** -názvy těchto vlastností jsou velká a malá písmena. Používejte názvy, jak je vidět výše, abyste předešli problémům s nasazení.

## <a name="template-deployment"></a>Nasazení šablon

Rozšíření virtuálního počítače Azure se dá nasadit pomocí šablon Azure Resource Manager. Schéma JSON, které jsou popsané v předchozí části lze použít v šablonu Azure Resource Manager ke spuštění rozšíření vlastních skriptů při nasazení šablony Azure Resource Manager. Ukázka šablony, která obsahuje rozšíření vlastních skriptů je zde uveden, [Githubu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="powershell-deployment"></a>Nasazení prostředí PowerShell

`Set-AzureRmVMCustomScriptExtension` Příkaz lze použít k přidání rozšíření vlastních skriptů do existujícího virtuálního počítače. Další informace najdete v tématu [Set-AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).
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

Data o stavu nasazení rozšíření mohou být načteny z portálu Azure a pomocí modulu Azure PowerShell. Pokud chcete zobrazit stav nasazení rozšíření pro daný virtuální počítač, spusťte následující příkaz.

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Soubory, které jsou v adresáři následující na cílový virtuální počítač je protokolovány výstupu spuštění rozšíření.
```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

Zadané soubory staženy do následujícího adresáře na cílový virtuální počítač.
```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads\<n>
```
kde `<n>` je desítkové celé číslo, které může změnit mezi jednotlivými spuštěními rozšíření.  `1.*` Hodnota odpovídá skutečným, aktuální `typeHandlerVersion` hodnotu rozšíření.  Skutečný adresář může být například `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2`.  

Při provádění `commandToExecute` příkaz, bude mít tento adresář nastavit rozšíření (například `...\Downloads\2`) jako aktuální pracovní adresář. To umožňuje použití relativní cesty vyhledejte soubory stáhli prostřednictvím `fileURIs` vlastnost. Najdete v následující tabulce příklady.

Vzhledem k tomu, že cesta pro stažení absolutní se mohou lišit v čase, je lepší zvolit relativní skriptu nebo cesty k souboru v `commandToExecute` řetězce, kdykoli je to možné. Příklad:
```json
    "commandToExecute": "powershell.exe . . . -File './scripts/myscript.ps1'"
```

Informace o cestě po první segment identifikátoru URI se zachovává kvůli soubory stažené prostřednictvím `fileUris` seznam vlastností.  Jak je znázorněno v následující tabulce, stažené soubory jsou mapovány na stažení podadresáře tak, aby odrážela strukturu `fileUris` hodnoty.  

#### <a name="examples-of-downloaded-files"></a>Příklady stažené soubory

| Identifikátor URI v fileUris | Relativní umístění stažené | Absolutní stáhli umístění * |
| ---- | ------- |:--- |
| `https://someAcct.blob.core.windows.net/aContainer/scripts/myscript.ps1` | `./scripts/myscript.ps1` |`C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\scripts\myscript.ps1`  |
| `https://someAcct.blob.core.windows.net/aContainer/topLevel.ps1` | `./topLevel.ps1` | `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\topLevel.ps1` |

\*Jako výš, cesty absolutní adresářů se změní za dobu virtuálního počítače, ale není v rámci jednoho spuštění rozšíření CustomScript.

### <a name="support"></a>Podpora

Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na Azure odborníky na [MSDN Azure a Stack Overflow fórech] (https://azure.microsoft.com/en-us/support/forums/). Alternativně můžete soubor incidentu podpory Azure. Přejděte na [podporu Azure lokality](https://azure.microsoft.com/en-us/support/options/) a vyberte Get podpory. Informace o používání Azure podporovat, najdete v tématu [podporu Microsoft Azure – nejčastější dotazy](https://azure.microsoft.com/en-us/support/faq/).
