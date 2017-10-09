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
# <a name="adding-applications-tooazure-batch-with-azure-cli"></a><span data-ttu-id="d65d5-103">Přidání tooAzure aplikace Batch pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="d65d5-103">Adding applications tooAzure Batch with Azure CLI</span></span>

<span data-ttu-id="d65d5-104">Tento skript ukazuje, jak tooset si aplikaci pro použití s fondu Azure Batch nebo úloh.</span><span class="sxs-lookup"><span data-stu-id="d65d5-104">This script demonstrates how tooset up an application for use with an Azure Batch pool or task.</span></span> <span data-ttu-id="d65d5-105">tooset si aplikaci, balíček vaší spustitelný soubor, společně s všechny závislosti, do souboru .zip.</span><span class="sxs-lookup"><span data-stu-id="d65d5-105">tooset up an application, package your executable, together with any dependencies, into a .zip file.</span></span> <span data-ttu-id="d65d5-106">Soubor spustitelný soubor zip tento příklad hello se nazývá "Moje-aplikace-exe.zip'.</span><span class="sxs-lookup"><span data-stu-id="d65d5-106">In this example hello executable zip file is called 'my-application-exe.zip'.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d65d5-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d65d5-107">Prerequisites</span></span>

- <span data-ttu-id="d65d5-108">Instalace hello hello pokyny uvedené v hello pomocí rozhraní příkazového řádku Azure [Průvodce instalací Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli), pokud jste tak již neučinili.</span><span class="sxs-lookup"><span data-stu-id="d65d5-108">Install hello Azure CLI using hello instructions provided in hello [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli), if you have not already done so.</span></span>
- <span data-ttu-id="d65d5-109">Pokud jste již nemáte, vytvořte účet Batch.</span><span class="sxs-lookup"><span data-stu-id="d65d5-109">Create a Batch account if you don't already have one.</span></span> <span data-ttu-id="d65d5-110">V tématu [vytvořit dávkový účet s hello rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) ukázkový skript, který vytvoří účet.</span><span class="sxs-lookup"><span data-stu-id="d65d5-110">See [Create a Batch account with hello Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) for a sample script that creates an account.</span></span>

## <a name="sample-script"></a><span data-ttu-id="d65d5-111">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="d65d5-111">Sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/batch/add-application/add-application.sh "Add Application")]

## <a name="clean-up-application"></a><span data-ttu-id="d65d5-112">Vyčištění aplikace</span><span class="sxs-lookup"><span data-stu-id="d65d5-112">Clean up application</span></span>

<span data-ttu-id="d65d5-113">Po spuštění hello výše ukázkový skript, spusťte následující příkazy tooremove hello aplikace a všech jeho balíčků nahrané aplikace.</span><span class="sxs-lookup"><span data-stu-id="d65d5-113">After you run hello above sample script, run hello following commands tooremove the application and all of its uploaded application packages.</span></span>

```azurecli
az batch application package delete -g myresourcegroup -n mybatchaccount --application-id myapp --version 1.0 --yes
az batch application delete -g myresourcegroup -n mybatchaccount --application-id myapp --yes
```

## <a name="script-explanation"></a><span data-ttu-id="d65d5-114">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="d65d5-114">Script explanation</span></span>

<span data-ttu-id="d65d5-115">Tento skript používá následující příkazy toocreate hello aplikaci a nahrajte balíček aplikace.</span><span class="sxs-lookup"><span data-stu-id="d65d5-115">This script uses hello following commands toocreate an application and upload an application package.</span></span>
<span data-ttu-id="d65d5-116">Každý příkaz v dokumentaci k toocommand specifické hello tabulky odkazů.</span><span class="sxs-lookup"><span data-stu-id="d65d5-116">Each command in hello table links toocommand-specific documentation.</span></span>

| <span data-ttu-id="d65d5-117">Příkaz</span><span class="sxs-lookup"><span data-stu-id="d65d5-117">Command</span></span> | <span data-ttu-id="d65d5-118">Poznámky</span><span class="sxs-lookup"><span data-stu-id="d65d5-118">Notes</span></span> |
|---|---|
| [<span data-ttu-id="d65d5-119">vytvořit aplikaci služby batch az</span><span class="sxs-lookup"><span data-stu-id="d65d5-119">az batch application create</span></span>](https://docs.microsoft.com/cli/azure/batch/application#create) | <span data-ttu-id="d65d5-120">Vytvoří aplikace.</span><span class="sxs-lookup"><span data-stu-id="d65d5-120">Creates an application.</span></span>  |
| [<span data-ttu-id="d65d5-121">Sada aplikací batch az</span><span class="sxs-lookup"><span data-stu-id="d65d5-121">az batch application set</span></span>](https://docs.microsoft.com/cli/azure/batch/application#set) | <span data-ttu-id="d65d5-122">Aktualizace vlastností aplikace.</span><span class="sxs-lookup"><span data-stu-id="d65d5-122">Updates properties of an application.</span></span>  |
| [<span data-ttu-id="d65d5-123">Vytvoření balíčku aplikace batch az</span><span class="sxs-lookup"><span data-stu-id="d65d5-123">az batch application package create</span></span>](https://docs.microsoft.com/cli/azure/batch/application/package#create) | <span data-ttu-id="d65d5-124">Přidá toohello balíčku aplikace zadaná aplikace.</span><span class="sxs-lookup"><span data-stu-id="d65d5-124">Adds an application package toohello specified application.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="d65d5-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d65d5-125">Next steps</span></span>

<span data-ttu-id="d65d5-126">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d65d5-126">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="d65d5-127">Další ukázky skriptu rozhraní příkazového řádku Batch lze nalézt v hello [dokumentaci k rozhraní příkazového řádku Azure Batch](../batch-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="d65d5-127">Additional Batch CLI script samples can be found in hello [Azure Batch CLI documentation](../batch-cli-samples.md).</span></span>
