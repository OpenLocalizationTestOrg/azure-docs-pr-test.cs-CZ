---
title: "chyby rozšíření virtuálního počítače s Windows aaaTroubleshooting | Microsoft Docs"
description: "Další informace o řešení potíží se přes rozšíření virtuálního počítače Windows Azure"
services: virtual-machines-windows
documentationcenter: 
author: kundanap
manager: timlt
editor: 
tags: top-support-issue,azure-resource-manager
ms.assetid: 878ab9b6-c3e6-40be-82d4-d77fecd5030f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/29/2016
ms.author: kundanap
ms.openlocfilehash: d24544743d9e0cb1a68ec9ab7718716f918054f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-azure-windows-vm-extension-failures"></a>Řešení potíží se přes rozšíření virtuálního počítače Windows Azure
[!INCLUDE [virtual-machines-common-extensions-troubleshoot](../../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a>Zobrazení stavu rozšíření
Šablony Azure Resource Manager lze spustit z prostředí Azure PowerShell. Jakmile se spustí hello šablony, lze zobrazit stav rozšíření hello z Průzkumníka prostředků Azure nebo hello nástroje příkazového řádku.

Zde naleznete příklad:

Azure PowerShell:

      Get-AzureRmVM -ResourceGroupName $RGName -Name $vmName -Status

Tady je ukázkový výstup hello:

      Extensions:  {
      "ExtensionType": "Microsoft.Compute.CustomScriptExtension",
      "Name": "myCustomScriptExtension",
      "SubStatuses": [
        {
          "Code": "ComponentStatus/StdOut/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "    Directory: C:\\temp\\n\\n\\nMode                LastWriteTime     Length Name
              \\n----                -------------     ------ ----                              \\n-a---          9/1/2015   2:03 AM         11
              test.txt                          \\n\\n",
                      "Time": null
          },
        {
          "Code": "ComponentStatus/StdErr/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "",
          "Time": null
        }
    }
  ]

## <a name="troubleshooting-extension-failures"></a>Řešení potíží s chyby rozšíření
### <a name="re-running-hello-extension-on-hello-vm"></a>Opětovné spuštění rozšíření hello na hello virtuálních počítačů
Pokud používáte skripty na hello virtuálního počítače pomocí rozšíření vlastních skriptů, může někdy spustit došlo k chybě, kde virtuálního počítače byla úspěšně vytvořena, ale došlo k selhání skriptu hello. Za těchto podmínek hello doporučená toorecover způsob, jak z této chyby je tooremove hello rozšíření a znovu spustit šablonu hello.
Poznámka: V budoucnosti, tato funkce bude rozšířené tooremove hello potřebu odinstalace hello rozšíření.

#### <a name="remove-hello-extension-from-azure-powershell"></a>Odebrat rozšíření hello z prostředí Azure PowerShell
    Remove-AzureRmVMExtension -ResourceGroupName $RGName -VMName $vmName -Name "myCustomScriptExtension"

Po odebrání rozšíření hello hello šablonou může být znovu spustit toorun hello skripty na hello virtuálních počítačů.

