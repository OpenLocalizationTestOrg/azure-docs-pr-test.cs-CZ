---
title: "aaaAzure ukázkový skript prostředí PowerShell - připojení účtu úložiště webové aplikace tooa | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – připojit účtu úložiště tooa webové aplikace"
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: e4831bdc-2068-4883-9474-0b34c2e3e255
ms.service: app-service
ms.devlang: multiple
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 03/20/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 07693366d32fbaefe92c18df67ded81661e1a2df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-web-app-tooa-storage-account"></a>Připojení účtu úložiště tooa webové aplikace

V tomto scénáři se dozvíte, jak toocreate účet úložiště Azure a Azure webové aplikace. Potom propojíte hello úložiště účet toohello webovou aplikaci pomocí nastavení aplikace.

V případě potřeby nainstalujte prostředí Azure PowerShell pomocí hello instrukce najít v hello hello [prostředí Azure PowerShell průvodce](/powershell/azure/overview)a poté spusťte `Login-AzureRmAccount` toocreate připojení s Azure.

## <a name="sample-script"></a>Ukázkový skript

[!code-powershell[main](../../../powershell_scripts/app-service/connect-to-storage/connect-to-storage.ps1 "Connect a web app tooa storage account")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello, webové aplikace a všechny související prostředky.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá hello následující příkazy. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [Nový AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Nové AzureRmAppServicePlan](/powershell/module/azurerm.websites/new-azurermappserviceplan) | Vytvoří plán služby App Service. |
| [Nové AzureRmWebApp](/powershell/module/azurerm.websites/new-azurermwebapp) | Vytvoří webovou aplikaci. |
| [Nové AzureRMStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) | Vytvoří účet úložiště. |
| [Get-AzureRMStorageAccountKey](/powershell/module/azurerm.storage/get-azurermstorageaccountkey) | Získá hello přístupové klíče pro účet úložiště Azure. |
| [Set-AzureRmWebApp](/powershell/module/azurerm.websites/set-azurermwebapp) | Upraví konfiguraci webové aplikace. |

## <a name="next-steps"></a>Další kroky

Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).

Další ukázky prostředí Azure Powershell pro Azure App Service Web Apps naleznete v hello [prostředí Azure PowerShell ukázky](../app-service-powershell-samples.md).
