---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - Správa fondů ve službě Batch | Microsoft Docs"
description: "Ukázka skriptu rozhraní příkazového řádku Azure - Správa fondů ve službě Batch"
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
ms.openlocfilehash: 6c9ca9515565aff42752231a080943be8e4c810b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-azure-batch-pools-with-azure-cli"></a>Správa fondech Azure Batch pomocí rozhraní příkazového řádku Azure

Tyto skript ukazuje některé hello nástrojů dostupných v toocreate hello rozhraní příkazového řádku Azure a spravovat fondy výpočetních uzlů ve službě Azure Batch hello.

> [!NOTE]
> Hello příkazy v této ukázce vytvořit virtuální počítače Azure. Spuštěných virtuálních počítačů bude nabíhat poplatky tooyour účtu. toominimize tyto poplatky, odstraňte hello virtuální počítače po dokončení spuštěné ukázka hello. V tématu [vyčištění fondy](#clean-up-pools).

Fondy batch můžete nakonfigurovat ve dvou způsobů, buď konfigurací cloudové služby (jenom Windows) nebo konfigurace virtuálního počítače (Windows a Linux). Ukázkové skripty Hello níže ukazují, jak toocreate fondy s obě konfigurace.

## <a name="prerequisites"></a>Požadavky

- Instalace hello hello pokyny uvedené v hello pomocí rozhraní příkazového řádku Azure [Průvodce instalací Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli), pokud jste tak již neučinili.
- Pokud jste již nemáte, vytvořte účet Batch. V tématu [vytvořit dávkový účet s hello rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) ukázkový skript, který vytvoří účet.
- Pokud jste tak ještě neučinili, nakonfigurujte toorun aplikaci ze spouštěcí úkol. V tématu [přidání tooAzure aplikace Batch pomocí rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) ukázkový skript, který se vytvoří aplikace a odesílá tooAzure balíčku aplikace.

## <a name="pool-with-cloud-service-configuration-sample-script"></a>Fond se cloudové služby konfigurace ukázkový skript

[!code-azurecli[main](../../../cli_scripts/batch/manage-pool/manage-pool-windows.sh "Manage Cloud Services Pools")]

## <a name="pool-with-virtual-machine-configuration-sample-script"></a>Fond se skript ukázka konfigurace virtuálního počítače

[!code-azurecli[main](../../../cli_scripts/batch/manage-pool/manage-pool-linux.sh "Manage Virtual Machine Pools")]

## <a name="clean-up-pools"></a>Vyčištění fondy

Po spuštění hello výše ukázkový skript, spusťte následující příkaz toodelete hello fondy hello.
```azurecli
az batch pool delete --pool-id mypool-windows
az batch pool delete --pool-id mypool-linux
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá hello následující příkazy toocreate a manipulace s fondy Batch.
Každý příkaz v dokumentaci k toocommand specifické hello tabulky odkazů.

| Příkaz | Poznámky |
|---|---|
| [připojení k účtu batch az](https://docs.microsoft.com/cli/azure/batch/account#login) | Ověřování na základě účtu Batch.  |
| [souhrnný seznam aplikací batch az](https://docs.microsoft.com/cli/azure/batch/application/summary#list) | Zobrazí seznam dostupných aplikací hello hello účtu Batch.  |
| [Vytvoření fondu služby batch az](https://docs.microsoft.com/cli/azure/batch/pool#create) | Vytvoření fondu virtuálních počítačů.  |
| [Sada fondu batch az](https://docs.microsoft.com/cli/azure/batch/pool#set) | Aktualizujte vlastnosti fondu.  |
| [seznam uzlu. agent SKU fondu batch az](https://docs.microsoft.com/cli/azure/batch/pool/node-agent-skus#list) | Agent k dispozici uzel seznamu SKU a informace o obrázku.  |
| [Změna velikosti fondu batch az](https://docs.microsoft.com/cli/azure/batch/pool#resize) | Zadat počet hello změny velikosti spuštěných virtuálních počítačů v hello fondu.  |
| [Zobrazit fondu batch az](https://docs.microsoft.com/cli/azure/batch/pool#show) | Zobrazí vlastnosti hello fondu.  |
| [Odstranění fondu batch az](https://docs.microsoft.com/cli/azure/batch/pool#delete) | Odstranit hello zadaný fond.  |
| [Povolit automatické škálování fondu batch az](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#enable) | Povolit automatické škálování na fond a použít vzorec.  |
| [zakázat automatické škálování fondu batch az](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#disable) | Zakážete automatické škálování ve fondu.  |
| [seznam uzlů az batch](https://docs.microsoft.com/cli/azure/batch/node#list) | Seznam všech hello výpočetní uzel v hello zadaný fond.  |
| [restartování uzlu az batch](https://docs.microsoft.com/cli/azure/batch/node#reboot) | Restartování hello zadaný výpočetním uzlu.  |
| [Odstranění uzlu az batch](https://docs.microsoft.com/cli/azure/batch/node#delete) | Odstranění hello uvedené uzly z hello zadaný fond.  |

## <a name="next-steps"></a>Další kroky

Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skriptu rozhraní příkazového řádku Batch lze nalézt v hello [dokumentaci k rozhraní příkazového řádku Azure Batch](../batch-cli-samples.md).

