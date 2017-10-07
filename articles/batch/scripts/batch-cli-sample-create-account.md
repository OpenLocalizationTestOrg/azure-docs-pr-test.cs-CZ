---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vytvořit účet Batch | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření účtu Batch"
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
ms.openlocfilehash: 62b640eebbafdd1081822a638fd0720121ef067a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-batch-account-with-hello-azure-cli"></a>Vytvoření účtu Batch se hello rozhraní příkazového řádku Azure

Tento skript vytvoří účet Azure Batch a ukazuje, jak budou různé vlastnosti účtu hello můžete dotaz a aktualizovat.

## <a name="prerequisites"></a>Požadavky

Instalace hello hello pokyny uvedené v hello pomocí rozhraní příkazového řádku Azure [Průvodce instalací Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli), pokud jste tak již neučinili.

## <a name="batch-account-sample-script"></a>Ukázkový skript účtu batch

Při vytváření účtu Batch, ve výchozím nastavení jeho výpočetní uzly se přiřadila interně hello služby Batch. Přidělené výpočetní uzly budou subjektu tooa samostatné základní kvóta a hello účet může být ověřen prostřednictvím pověření sdílený klíč nebo Azure Active Dirctory tokenu.

[!code-azurecli[main](../../../cli_scripts/batch/create-account/create-account.sh "Create Account")]

## <a name="batch-account-using-user-subscription-sample-script"></a>Účet batch pomocí uživatele předplatné ukázkový skript

Můžete také zvolit toohave Batch vytvořit jeho výpočetní uzly v rámci vlastní předplatného Azure.
Účty, které přidělit výpočetní uzly do předplatného musí být ověřeny pomocí tokenu Azure Active Directory a hello výpočetních uzlů přidělených se bude započítávat kvóty předplatného. toocreate účet v tomto režimu, při vytváření účtu hello jeden musíte zadat odkaz Key Vault.

[!code-azurecli[main](../../../cli_scripts/batch/create-account/create-account-user-subscription.sh  "Create Account using User Subscription")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení

Po spuštění buď hello výše ukázkové skripty, spusťte následující příkaz tooremove hello skupinu prostředků a všechny související prostředky (včetně účty Batch, účty Azure Storage a Azure trezorů klíčů).

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá hello následující příkazy toocreate skupinu prostředků, účtu Batch a všechny související prostředky. Každý příkaz v dokumentaci k toocommand specifické hello tabulky odkazů.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvoření účtu batch az](https://docs.microsoft.com/cli/azure/batch/account#create) | Vytvoří účet Batch hello.  |
| [nastaven účet batch az](https://docs.microsoft.com/cli/azure/batch/account#set) | Aktualizuje vlastnosti hello účtu Batch.  |
| [Zobrazit účet batch az](https://docs.microsoft.com/cli/azure/batch/account#show) | Načte podrobnosti o hello zadat účet Batch.  |
| [seznam klíčů účtu batch az](https://docs.microsoft.com/cli/azure/batch/account/keys#list) | Načte hello přístup klíče hello zadat účet Batch.  |
| [připojení k účtu batch az](https://docs.microsoft.com/cli/azure/batch/account#login) | Ověří proti hello zadán účet pro další interakce rozhraní příkazového řádku Batch.  |
| [Vytvořit účet úložiště az](https://docs.microsoft.com/cli/azure/storage/account#create) | Vytvoří účet úložiště. |
| [Vytvoření az keyvault](https://docs.microsoft.com/cli/azure/keyvault#create) | Vytvoří trezoru klíčů. |
| [AZ keyvault set zásad](https://docs.microsoft.com/cli/azure/keyvault#set-policy) | Aktualizujte zásady zabezpečení hello hello zadaný trezor klíčů. |
| [Odstranění skupiny az](https://docs.microsoft.com/cli/azure/group#delete) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |

## <a name="next-steps"></a>Další kroky

Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Další ukázky skriptu rozhraní příkazového řádku Batch lze nalézt v hello [dokumentaci k rozhraní příkazového řádku Azure Batch](../batch-cli-samples.md).
