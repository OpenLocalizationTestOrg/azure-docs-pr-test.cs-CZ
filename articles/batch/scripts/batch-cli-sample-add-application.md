---
title: "Rozhraní příkazového řádku Azure skriptu ukázka – přidat aplikaci ve službě Batch | Microsoft Docs"
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
ms.openlocfilehash: 5d057eaf32867aedc95d58c5185e2be1f9385ec0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="adding-applications-to-azure-batch-with-azure-cli"></a><span data-ttu-id="49e34-103">Přidání aplikací do Azure Batch pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="49e34-103">Adding applications to Azure Batch with Azure CLI</span></span>

<span data-ttu-id="49e34-104">Tento skript ukazuje, jak nastavit aplikaci pro použití s fondu Azure Batch nebo úloh.</span><span class="sxs-lookup"><span data-stu-id="49e34-104">This script demonstrates how to set up an application for use with an Azure Batch pool or task.</span></span> <span data-ttu-id="49e34-105">Pokud chcete nastavit aplikaci, balíček vaší spustitelný soubor, společně s všechny závislosti, do souboru .zip.</span><span class="sxs-lookup"><span data-stu-id="49e34-105">To set up an application, package your executable, together with any dependencies, into a .zip file.</span></span> <span data-ttu-id="49e34-106">V tomto příkladu se nazývá souboru spustitelný soubor zip, Moje-aplikace-exe.zip'.</span><span class="sxs-lookup"><span data-stu-id="49e34-106">In this example the executable zip file is called 'my-application-exe.zip'.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="49e34-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="49e34-107">Prerequisites</span></span>

- <span data-ttu-id="49e34-108">Instalace rozhraní příkazového řádku Azure pomocí pokynů uvedených v [Průvodce instalací Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli), pokud jste tak již neučinili.</span><span class="sxs-lookup"><span data-stu-id="49e34-108">Install the Azure CLI using the instructions provided in the [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli), if you have not already done so.</span></span>
- <span data-ttu-id="49e34-109">Pokud jste již nemáte, vytvořte účet Batch.</span><span class="sxs-lookup"><span data-stu-id="49e34-109">Create a Batch account if you don't already have one.</span></span> <span data-ttu-id="49e34-110">V tématu [vytvořit účet Batch pomocí rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) ukázkový skript, který vytvoří účet.</span><span class="sxs-lookup"><span data-stu-id="49e34-110">See [Create a Batch account with the Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) for a sample script that creates an account.</span></span>

## <a name="sample-script"></a><span data-ttu-id="49e34-111">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="49e34-111">Sample script</span></span>

<span data-ttu-id="49e34-112">[!code-azurecli[hlavní](../../../cli_scripts/batch/add-application/add-application.sh "přidat aplikaci")]</span><span class="sxs-lookup"><span data-stu-id="49e34-112">[!code-azurecli[main](../../../cli_scripts/batch/add-application/add-application.sh "Add Application")]</span></span>

## <a name="clean-up-application"></a><span data-ttu-id="49e34-113">Vyčištění aplikace</span><span class="sxs-lookup"><span data-stu-id="49e34-113">Clean up application</span></span>

<span data-ttu-id="49e34-114">Po spuštění výše uvedený ukázkový skript, spusťte následující příkazy pro odebrání aplikace a všech jeho balíčků nahrané aplikace.</span><span class="sxs-lookup"><span data-stu-id="49e34-114">After you run the above sample script, run the following commands to remove the application and all of its uploaded application packages.</span></span>

```azurecli
az batch application package delete -g myresourcegroup -n mybatchaccount --application-id myapp --version 1.0 --yes
az batch application delete -g myresourcegroup -n mybatchaccount --application-id myapp --yes
```

## <a name="script-explanation"></a><span data-ttu-id="49e34-115">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="49e34-115">Script explanation</span></span>

<span data-ttu-id="49e34-116">Tento skript používá následující příkazy k vytvoření aplikace a nahrajte balíček aplikace.</span><span class="sxs-lookup"><span data-stu-id="49e34-116">This script uses the following commands to create an application and upload an application package.</span></span>
<span data-ttu-id="49e34-117">Každý příkaz v tabulce odkazy na dokumentaci specifické pro příkaz.</span><span class="sxs-lookup"><span data-stu-id="49e34-117">Each command in the table links to command-specific documentation.</span></span>

| <span data-ttu-id="49e34-118">Příkaz</span><span class="sxs-lookup"><span data-stu-id="49e34-118">Command</span></span> | <span data-ttu-id="49e34-119">Poznámky</span><span class="sxs-lookup"><span data-stu-id="49e34-119">Notes</span></span> |
|---|---|
| [<span data-ttu-id="49e34-120">vytvořit aplikaci služby batch az</span><span class="sxs-lookup"><span data-stu-id="49e34-120">az batch application create</span></span>](https://docs.microsoft.com/cli/azure/batch/application#create) | <span data-ttu-id="49e34-121">Vytvoří aplikace.</span><span class="sxs-lookup"><span data-stu-id="49e34-121">Creates an application.</span></span>  |
| [<span data-ttu-id="49e34-122">Sada aplikací batch az</span><span class="sxs-lookup"><span data-stu-id="49e34-122">az batch application set</span></span>](https://docs.microsoft.com/cli/azure/batch/application#set) | <span data-ttu-id="49e34-123">Aktualizace vlastností aplikace.</span><span class="sxs-lookup"><span data-stu-id="49e34-123">Updates properties of an application.</span></span>  |
| [<span data-ttu-id="49e34-124">Vytvoření balíčku aplikace batch az</span><span class="sxs-lookup"><span data-stu-id="49e34-124">az batch application package create</span></span>](https://docs.microsoft.com/cli/azure/batch/application/package#create) | <span data-ttu-id="49e34-125">Přidá balíček aplikace pro zadanou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="49e34-125">Adds an application package to the specified application.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="49e34-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="49e34-126">Next steps</span></span>

<span data-ttu-id="49e34-127">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="49e34-127">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="49e34-128">Další ukázky skriptu Batch rozhraní příkazového řádku najdete v [dokumentaci k rozhraní příkazového řádku Azure Batch](../batch-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="49e34-128">Additional Batch CLI script samples can be found in the [Azure Batch CLI documentation](../batch-cli-samples.md).</span></span>
