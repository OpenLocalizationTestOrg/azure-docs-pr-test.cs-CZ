---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vytvoření webové aplikace a nasazení kódu tooa pracovní prostředí | Microsoft Docs"
description: "Ukázka skriptu Azure CLI – vytvoření webové aplikace a nasazení kódu tooa pracovní prostředí"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 2b995dcd-e471-4355-9fda-00babcdb156e
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/19/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: cd07f5eda31041effd7b7334f5ecc04e6c1a0514
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-and-deploy-code-tooa-staging-environment"></a>Vytvoření webové aplikace a nasazení kódu tooa pracovní prostředí

Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se další nasazovací slot názvem "přípravy" a pak nasadí ukázkové aplikace toohello "pracovní" slot.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-deployment-slot/deploy-deployment-slot.sh "Create a web app and deploy code tooa staging environment")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá hello následující příkazy. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvořit plán aplikační služby az](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Vytvoří plán služby App Service. |
| [Vytvoření az webapp](https://docs.microsoft.com/cli/azure/webapp#create) | Vytvoří webové aplikace Azure. |
| [Vytvoření az webapp nasazovací slot.](https://docs.microsoft.com/cli/azure/webapp/deployment/slot#create) | Vytvořte nasazovací slot. |
| [AZ webapp nasazení zdroj konfigurace](https://docs.microsoft.com/cli/azure/webapp/deployment/source#config) | Přidruží úložiště Git nebo Mercurial webové aplikace Azure. |
| [Procházet az webapp](https://docs.microsoft.com/cli/azure/webapp#browse) | Webové aplikace Azure, otevřete v prohlížeči. |
| [Prohození slotů nasazení az webapp](https://docs.microsoft.com/cli/azure/webapp/deployment/slot#swap) | Prohodit slot zadané nasazení do produkčního prostředí. |

## <a name="next-steps"></a>Další kroky

Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skriptu rozhraní příkazového řádku služby aplikace naleznete v hello [dokumentaci služby Azure App Service](../app-service-cli-samples.md).
