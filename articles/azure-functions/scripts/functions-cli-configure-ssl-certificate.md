---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - Bind vlastní tooa funkce aplikace certifikát SSL | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - vazby vlastní SSL certifikát tooa funkce aplikace v Azure"
services: functions
documentationcenter: 
author: ggailey777
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: eb95d350-81ea-4145-a1e2-6eea3b7469b2
ms.service: functions
ms.workload: na
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 04/10/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 692dbc03583f2978131823083f1bfd257882664c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="bind-a-custom-ssl-certificate-tooa-function-app"></a>Vytvoření vazby vlastní tooa funkce aplikace certifikát SSL

Tento ukázkový skript vytvoří aplikaci funkce ve službě App Service se jeho souvisejících prostředků a potom vytvoří vazbu certifikátu SSL hello tooit název vlastní doménu. Tato ukázka je třeba:

* Přístup k stránku konfigurace tooyour doménového registrátora DNS.
* Platné. Soubor PFX a heslo pro hello SSL certifikát má tooupload a jejich vazby.

toobind certifikát SSL, funkce aplikace musí být vytvořeny, v rámci plánu služby App Service a nejsou v plánu spotřeby.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud zvolte tooinstall a místně pomocí hello rozhraní příkazového řádku, v tomto tématu vyžaduje, že používáte hello Azure CLI verze 2.0 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate tooa web app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá hello následující příkazy. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvořit plán aplikační služby az](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Vytvoří toobind vyžaduje plán App Service certifikáty SSL. |
| [Vytvoření az functionapp]() | Vytvoří aplikaci funkce. |
| [Přidat az název hostitele konfigurace webové služby App Service](https://docs.microsoft.com/cli/azure/appservice/web/config/hostname#add) | Mapuje aplikaci funkce toohello vlastní doménu. |
| [AZ služby App Service web konfigurace ssl nahrávání](https://docs.microsoft.com/cli/azure/appservice/web/config/ssl#upload) | Ukládání funkce aplikace tooa certifikát SSL. |
| [AZ služby App Service web konfigurace protokolu ssl vazby](https://docs.microsoft.com/en-us/cli/azure/appservice/web/config/ssl#bind) | Vytvoří vazbu nahrané tooa funkce aplikace certifikát SSL. |

## <a name="next-steps"></a>Další kroky

Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skriptu rozhraní příkazového řádku služby aplikace naleznete v hello [dokumentaci služby Azure App Service]().
