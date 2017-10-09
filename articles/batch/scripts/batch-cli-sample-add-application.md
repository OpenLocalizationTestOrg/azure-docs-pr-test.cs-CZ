---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku – přidat aplikaci ve službě Batch | Microsoft Docs"
description: "Rozhraní příkazového řádku Azure skriptu ukázka – přidat aplikaci ve službě Batch"
services: batch
documentationcenter: 
author: annatisch
manager: daryls
editor: tysonn
ms.assetid: 
ms.service: batch
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/02/2017
ms.author: antisch
ms.openlocfilehash: cb33b3a7b30610011b19954a987995cc5f0257c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="adding-applications-tooazure-batch-with-azure-cli"></a>Přidání tooAzure aplikace Batch pomocí rozhraní příkazového řádku Azure

Tento skript ukazuje, jak tooset si aplikaci pro použití s fondu Azure Batch nebo úloh. tooset si aplikaci, balíček vaší spustitelný soubor, společně s všechny závislosti, do souboru .zip. Soubor spustitelný soubor zip tento příklad hello se nazývá "Moje-aplikace-exe.zip'.

## <a name="prerequisites"></a>Požadavky

- Instalace hello hello pokyny uvedené v hello pomocí rozhraní příkazového řádku Azure [Průvodce instalací Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli), pokud jste tak již neučinili.
- Pokud jste již nemáte, vytvořte účet Batch. V tématu [vytvořit dávkový účet s hello rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) ukázkový skript, který vytvoří účet.

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli[main](../../../cli_scripts/batch/add-application/add-application.sh "Add Application")]

## <a name="clean-up-application"></a>Vyčištění aplikace

Po spuštění hello výše ukázkový skript, spusťte následující příkazy tooremove hello aplikace a všech jeho balíčků nahrané aplikace.

```azurecli
az batch application package delete -g myresourcegroup -n mybatchaccount --application-id myapp --version 1.0 --yes
az batch application delete -g myresourcegroup -n mybatchaccount --application-id myapp --yes
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy toocreate hello aplikaci a nahrajte balíček aplikace.
Každý příkaz v dokumentaci k toocommand specifické hello tabulky odkazů.

| Příkaz | Poznámky |
|---|---|
| [vytvořit aplikaci služby batch az](https://docs.microsoft.com/cli/azure/batch/application#create) | Vytvoří aplikace.  |
| [Sada aplikací batch az](https://docs.microsoft.com/cli/azure/batch/application#set) | Aktualizace vlastností aplikace.  |
| [Vytvoření balíčku aplikace batch az](https://docs.microsoft.com/cli/azure/batch/application/package#create) | Přidá toohello balíčku aplikace zadaná aplikace.  |

## <a name="next-steps"></a>Další kroky

Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skriptu rozhraní příkazového řádku Batch lze nalézt v hello [dokumentaci k rozhraní příkazového řádku Azure Batch](../batch-cli-samples.md).
