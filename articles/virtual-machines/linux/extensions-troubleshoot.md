---
title: "virtuální počítač s Linuxem aaaTroubleshooting rozšíření selhání | Microsoft Docs"
description: "Další informace o řešení potíží se přes rozšíření virtuálního počítače Azure s Linuxem"
services: virtual-machines-linux
documentationcenter: 
author: kundanap
manager: timlt
editor: 
tags: top-support-issue,azure-resource-manager
ms.assetid: f05d93f3-42fc-4a09-9798-d92f7929c417
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/29/2016
ms.author: kundanap
ms.openlocfilehash: 29a0ca34207421e0014380000a313d3c44e7e594
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-azure-linux-vm-extension-failures"></a>Řešení potíží se přes rozšíření virtuálního počítače Azure s Linuxem
[!INCLUDE [virtual-machines-common-extensions-troubleshoot](../../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a>Zobrazení stavu rozšíření
Šablony Azure Resource Manager lze spustit z příkazového řádku Azure CLI hello. Jakmile se spustí hello šablony, lze zobrazit stav rozšíření hello z Průzkumníka prostředků Azure nebo hello nástroje příkazového řádku.

Zde naleznete příklad:

Rozhraní příkazového řádku Azure:

      azure vm get-instance-view


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

## <a name="troubleshooting-extenson-failures"></a>Řešení potíží s Extenson selhání:
### <a name="re-running-hello-extension-on-hello-vm"></a>Opětovné spuštění rozšíření hello na hello virtuálních počítačů
Pokud používáte skripty na hello virtuálního počítače pomocí rozšíření vlastních skriptů, může někdy spustit došlo k chybě, kde virtuálního počítače byla úspěšně vytvořena, ale došlo k selhání skriptu hello. Za těchto podmínek hello doporučená toorecover způsob, jak z této chyby je tooremove hello rozšíření a znovu spustit šablonu hello.
Poznámka: V budoucnosti, tato funkce bude rozšířené tooremove hello potřebu odinstalace hello rozšíření.

#### <a name="remove-hello-extension-from-azure-cli"></a>Odebrat rozšíření hello z příkazového řádku Azure
      azure vm extension set --resource-group "KPRG1" --vm-name "kundanapdemo" --publisher-name "Microsoft.Compute.CustomScriptExtension" --name "myCustomScriptExtension" --version 1.4 --uninstall

Kde "publsher-name" odpovídá typu rozšíření toohello z výstupu hello "virtuální počítač azure get-instance zobrazení" a název je název hello hello rozšíření prostředků z šablony hello

Po odebrání rozšíření hello hello šablonou může být znovu spustit toorun hello skripty na hello virtuálních počítačů.

