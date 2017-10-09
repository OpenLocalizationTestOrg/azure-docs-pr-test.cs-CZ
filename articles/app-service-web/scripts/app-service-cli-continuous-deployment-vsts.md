---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vytvořit webovou aplikaci s průběžné nasazování z Visual Studio Team Services | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření webové aplikace s průběžné nasazování z Visual Studio Team Services"
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 389d3bd3-cd8e-4715-a3a1-031ec061d385
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: f8d0c2645ec5311296ca9b2df20f97e77bab2283
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-with-continuous-deployment-from-visual-studio-team-services"></a>Vytvoření webové aplikace s průběžné nasazování z Visual Studio Team Services

Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se jeho souvisejících prostředků a poté nastaví průběžné nasazování z úložiště Visual Studio Team Services. Pro tuto ukázku budete potřebovat:

* Visual Studio Team Services úložiště s kódem aplikace, které mají oprávnění pro správu.
* A [Personal Access Token (Jan)](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) pro váš účet Visual Studio Team Services.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-vsts-continuous/deploy-vsts-continuous.sh?highlight=3-4 "Create a web app with continuous deployment from Visual Studio Team Services")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá hello následující příkazy. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvořit plán aplikační služby az](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Vytvoří plán služby App Service. |
| [Vytvoření az webapp](https://docs.microsoft.com/cli/azure/webapp#create) | Vytvoří webové aplikace Azure. |
| [AZ webapp nasazení zdroj konfigurace](https://docs.microsoft.com/cli/azure/webapp/deployment/source#config) | Přidruží úložiště Git nebo Mercurial webové aplikace Azure. |
| [Procházet az webapp](https://docs.microsoft.com/cli/azure/webapp#browse) | Webové aplikace Azure, otevřete v prohlížeči. |

## <a name="next-steps"></a>Další kroky

Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skriptu rozhraní příkazového řádku služby aplikace naleznete v hello [dokumentaci služby Azure App Service](../app-service-cli-samples.md).
