---
title: "aaaCustom rozšíření skriptů na virtuální počítač s Windows | Microsoft Docs"
description: "Automatizovat úkoly konfigurace virtuálního počítače Azure pomocí skriptů prostředí PowerShell toorun rozšíření vlastních skriptů hello na vzdálené virtuální počítač s Windows"
services: virtual-machines-windows
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: ebb7340a-8f61-4d3c-a290-d7bf8de2d0bd
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 01/17/2017
ms.author: nepeters
ms.openlocfilehash: cf7bb895dd211f07fd010dc03b68cd77df1127b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="custom-script-extension-for-windows-using-hello-classic-deployment-model"></a>Vlastní skript rozšíření pro systém Windows s použitím modelu nasazení classic hello

> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello. Zjistěte, jak příliš[proveďte tyto kroky, pomocí modelu Resource Manager hello](../extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Hello rozšíření vlastních skriptů stahuje a spouští skripty na virtuálních počítačích Azure. Toto rozšíření je užitečné pro konfiguraci nasazení post, instalace softwaru nebo jakoukoli jinou konfiguraci, nebo úlohu správy. Skripty můžete stáhnout z úložiště Azure nebo GitHub nebo zadat toohello portál Azure na dobu běhu rozšíření. Hello rozšíření vlastních skriptů se integruje s šablon Azure Resource Manageru a můžete také spustit pomocí hello rozhraní příkazového řádku Azure, PowerShell, portálu Azure nebo hello REST API pro virtuální počítač Azure.

Tento dokument podrobně popisuje, jak pomocí rozšíření vlastních skriptů hello toouse hello modul Azure PowerShell, šablon Azure Resource Manageru a podrobnosti o řešení potíží s kroky v systémech Windows.

## <a name="prerequisites"></a>Požadavky

### <a name="operating-system"></a>Operační systém

Hello rozšíření vlastních skriptů pro pro Windows Server 2008 R2, můžete spouštět Windows 2012, 2012 R2 a 2016 uvolní.

### <a name="script-location"></a>Umístění skriptu

skript Hello musí toobe uložení do úložiště Azure, nebo jakéhokoli jiného umístění, které jsou přístupné prostřednictvím platnou adresu URL.

### <a name="internet-connectivity"></a>Připojení k Internetu

Hello vlastní skript rozšíření pro Windows vyžaduje, aby hello cílový virtuální počítač je připojený toohello Internetu. 

## <a name="extension-schema"></a>Rozšíření schématu

Hello následujícím kódu JSON ukazuje hello schéma pro hello rozšíření vlastních skriptů. rozšíření Hello vyžaduje umístění skriptu (Azure Storage nebo jiného umístění s platnou adresu URL) a příkaz tooexecute. Pokud používáte Azure Storage jako zdroj skriptu hello, je třeba klíč účtu úložiště Azure účet a název. Tyto položky by měl být považován za citlivá data a zadaný v konfiguraci chráněných nastavení rozšíření hello. Azure data virtuálního počítače chráněný rozšíření nastavení je zašifrovaná a dešifrovat jenom na hello cílového virtuálního počítače.

```json
{
    "name": "config-app",
    "type": "Microsoft.ClassicCompute/virtualMachines/extensions",
    "location": "[resourceGroup().location]",
    "apiVersion": "2015-06-01",
    "properties": {
        "publisher": "Microsoft.Compute",
        "extension": "CustomScriptExtension",
        "version": "1.8",
        "parameters": {
            "public": {
                "fileUris": "[myScriptLocation]"
            },
            "private": {
                "commandToExecute": "[myExecutionString]"
            }
        }
    }
}
```

### <a name="property-values"></a>Hodnoty vlastností

| Name (Název) | Hodnota nebo příklad |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Vydavatele | Microsoft.Compute |
| Rozšíření | CustomScriptExtension |
| typeHandlerVersion | 1.8 |
| fileUris (např.) | https://RAW.githubusercontent.com/Microsoft/DotNet-Core-Sample-Templates/Master/DotNet-Core-Music-Windows/Scripts/Configure-Music-App.ps1 |
| commandToExecute (např.) | prostředí PowerShell - ExecutionPolicy Unrestricted - souboru konfigurace app.ps1 Hudba |

## <a name="template-deployment"></a>Nasazení šablon

Rozšíření virtuálního počítače Azure se dá nasadit pomocí šablon Azure Resource Manager. popsané v předchozí části hello schématu JSON Hello lze použít v toorun hello šablony Azure Resource Manager rozšíření vlastních skriptů při nasazení šablony Azure Resource Manager. Ukázka šablony, která obsahuje hello rozšíření vlastních skriptů je zde uveden, [Githubu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="powershell-deployment"></a>Nasazení prostředí PowerShell

Hello `Set-AzureVMCustomScriptExtension` příkaz lze použít tooadd hello vlastní skript rozšíření tooan existující virtuální počítač. Další informace najdete v tématu [Set-AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).

```powershell
# create vm object
$vm = Get-AzureVM -Name 2016clas -ServiceName 2016clas1313

# set extension
Set-AzureVMCustomScriptExtension -VM $vm -FileUri myFileUri -Run 'create-file.ps1'

# update vm
$vm | Update-AzureVM
```

## <a name="troubleshoot-and-support"></a>Řešení potíží a podpora

### <a name="troubleshoot"></a>Řešení potíží

Data o stavu hello nasazení rozšíření mohou být načteny z hello portál Azure a pomocí modulu Azure PowerShell hello. Stav nasazení toosee hello rozšíření pro daný virtuální počítač, spusťte následující příkaz hello.

```powershell
Get-AzureVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Rozšíření spuštění výstupu nám zaznamenané toofiles najít v hello následující adresáře na hello cílového virtuálního počítače.

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

samotný skript Hello se stáhne do hello následující adresáře na hello cílového virtuálního počítače.

```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads
```

### <a name="support"></a>Podpora

Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na hello Azure odborníky na hello [fórech MSDN Azure a Stack Overflow](https://azure.microsoft.com/en-us/support/forums/). Alternativně můžete soubor incidentu podpory Azure. Přejděte toohello [podporu Azure lokality](https://azure.microsoft.com/en-us/support/options/) a vyberte Get podpory. Informace o používání Azure podporovat, najdete v tématu hello [podporu Microsoft Azure – nejčastější dotazy](https://azure.microsoft.com/en-us/support/faq/).
