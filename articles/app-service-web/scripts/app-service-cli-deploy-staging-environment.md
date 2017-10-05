---
title: "Ukázka skriptu Azure CLI – vytvoření webové aplikace a nasazení kódu do pracovního prostředí | Microsoft Docs"
description: "Ukázka skriptu Azure CLI – vytvoření webové aplikace a nasazení kódu do pracovního prostředí"
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
ms.openlocfilehash: d586b50258c32e44f55859aad0a89475e9e4d2eb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-web-app-and-deploy-code-to-a-staging-environment"></a>Vytvoření webové aplikace a nasazení kódu do pracovního prostředí

Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se další nasazovací slot názvem "přípravy" a pak nasadí ukázkovou aplikaci na "pracovní" slot.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[hlavní](../../../cli_scripts/app-service/deploy-deployment-slot/deploy-deployment-slot.sh "vytvoření webové aplikace a nasazení kódu do pracovního prostředí")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

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

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skript aplikace služby rozhraní příkazového řádku najdete v [dokumentaci služby Azure App Service](../app-service-cli-samples.md).
