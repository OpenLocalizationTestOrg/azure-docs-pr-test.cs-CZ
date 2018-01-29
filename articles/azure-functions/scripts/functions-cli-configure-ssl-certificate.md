---
title: "Ukázka skriptu Azure CLI - vazby SSL vlastní certifikát do aplikaci funkce | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - vazby vlastní certifikát SSL pro funkce aplikace v Azure"
services: functions
documentationcenter: 
author: ggailey777
manager: cfowler
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
ms.openlocfilehash: f8e8570d9c3093b5f49b000916644888304eed4e
ms.sourcegitcommit: 719dd33d18cc25c719572cd67e4e6bce29b1d6e7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/08/2018
---
# <a name="bind-a-custom-ssl-certificate-to-a-function-app"></a>Vlastní certifikát SSL vazbu na aplikaci – funkce

Tento ukázkový skript vytvoří aplikaci funkce ve službě App Service se jeho souvisejících prostředků a potom vytvoří vazbu certifikátu SSL vlastního názvu domény. Tato ukázka je třeba:

* Přístup na stránku konfigurace DNS doménového registrátora.
* Platné. Soubor PFX a heslo pro certifikát SSL chcete nahrát a jejich vazby.

Funkce aplikace k vytvoření vazby certifikátu SSL, musí být vytvořený v plán služby App Service a nejsou v plánu spotřeby.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, musíte používat Azure CLI verze 2.0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate to a web app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#az_group_create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvořit plán aplikační služby az](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Vytvoří plán služby App Service potřebné k vytvoření vazby certifikátů SSL. |
| [Vytvoření az functionapp]() | Vytvoří aplikaci funkce. |
| [Přidat az název hostitele konfigurace webové služby App Service](https://docs.microsoft.com/cli/azure/appservice/web/config/hostname#az_appservice_web_config_hostname_add) | Vlastní doména se mapuje na aplikaci funkce. |
| [AZ služby App Service web konfigurace ssl nahrávání](https://docs.microsoft.com/cli/azure/appservice/web/config/ssl#az_appservice_web_config_ssl_upload) | Certifikát SSL odešle do aplikaci funkce. |
| [AZ služby App Service web konfigurace protokolu ssl vazby](https://docs.microsoft.com/cli/azure/appservice/web/config/ssl#az_appservice_web_config_ssl_bind) | Váže nahraný certifikát SSL k aplikaci funkce. |

## <a name="next-steps"></a>Další postup

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skript aplikace služby rozhraní příkazového řádku najdete v [dokumentaci služby Azure App Service]().
