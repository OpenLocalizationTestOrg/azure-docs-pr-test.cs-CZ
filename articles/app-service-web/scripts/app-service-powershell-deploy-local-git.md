---
title: "aaaAzure ukázkový skript prostředí PowerShell - vytvoření webové aplikace a nasazení kódu z místního úložiště Git | Microsoft Docs"
description: "Ukázkový skript prostředí PowerShell Azure – vytvoření webové aplikace a nasazení kódu z místního úložiště Git"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 5a927f23-8e70-45fd-9aae-980d4e7a007d
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: a90996658bf0b609315460324d0dcd3a411a6512
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-and-deploy-code-from-a-local-git-repository"></a>Vytvoření webové aplikace a nasazení kódu z místního úložiště Git

Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se jeho souvisejících prostředků a poté nasadí kódu webové aplikace v místní úložiště Git.

V případě potřeby nainstalujte prostředí Azure PowerShell pomocí hello instrukce najít v hello hello [prostředí Azure PowerShell průvodce](/powershell/azure/overview)a poté spusťte `Login-AzureRmAccount` toocreate připojení s Azure. Kód aplikace také musí toobe zapsána do místního úložiště Git.

## <a name="sample-script"></a>Ukázkový skript

[!code-powershell[main](../../../powershell_scripts/app-service/deploy-local-git/deploy-local-git.ps1?highlight=1 "Create a web app and deploy code from a local Git repository")]

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
| [Set-AzureRmResource](/powershell/module/azurerm.resources/set-azurermresource) | Upravuje prostředků ve skupině prostředků. |
| [Get-AzureRmWebAppPublishingProfile](/powershell/module/azurerm.websites/get-azurermwebapppublishingprofile) | Získáte profil publikování webové aplikace. |

## <a name="next-steps"></a>Další kroky

Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).

Další ukázky prostředí Azure Powershell pro Azure App Service Web Apps naleznete v hello [prostředí Azure PowerShell ukázky](../app-service-powershell-samples.md).
