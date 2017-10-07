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
# <a name="create-a-batch-account-with-hello-azure-cli"></a><span data-ttu-id="60e32-103">Vytvoření účtu Batch se hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="60e32-103">Create a Batch account with hello Azure CLI</span></span>

<span data-ttu-id="60e32-104">Tento skript vytvoří účet Azure Batch a ukazuje, jak budou různé vlastnosti účtu hello můžete dotaz a aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="60e32-104">This script creates an Azure Batch account and shows how various properties of hello account can be queried and updated.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="60e32-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="60e32-105">Prerequisites</span></span>

<span data-ttu-id="60e32-106">Instalace hello hello pokyny uvedené v hello pomocí rozhraní příkazového řádku Azure [Průvodce instalací Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli), pokud jste tak již neučinili.</span><span class="sxs-lookup"><span data-stu-id="60e32-106">Install hello Azure CLI using hello instructions provided in hello [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli), if you have not already done so.</span></span>

## <a name="batch-account-sample-script"></a><span data-ttu-id="60e32-107">Ukázkový skript účtu batch</span><span class="sxs-lookup"><span data-stu-id="60e32-107">Batch account sample script</span></span>

<span data-ttu-id="60e32-108">Při vytváření účtu Batch, ve výchozím nastavení jeho výpočetní uzly se přiřadila interně hello služby Batch.</span><span class="sxs-lookup"><span data-stu-id="60e32-108">When you create a Batch account, by default its compute nodes are assigned internally by hello Batch service.</span></span> <span data-ttu-id="60e32-109">Přidělené výpočetní uzly budou subjektu tooa samostatné základní kvóta a hello účet může být ověřen prostřednictvím pověření sdílený klíč nebo Azure Active Dirctory tokenu.</span><span class="sxs-lookup"><span data-stu-id="60e32-109">Allocated compute nodes will be subject tooa separate core quota and hello account can be authenticated either via Shared Key credentials or an Azure Active Dirctory token.</span></span>

[!code-azurecli[main](../../../cli_scripts/batch/create-account/create-account.sh "Create Account")]

## <a name="batch-account-using-user-subscription-sample-script"></a><span data-ttu-id="60e32-110">Účet batch pomocí uživatele předplatné ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="60e32-110">Batch account using user subscription sample script</span></span>

<span data-ttu-id="60e32-111">Můžete také zvolit toohave Batch vytvořit jeho výpočetní uzly v rámci vlastní předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="60e32-111">You can also opt toohave Batch create its compute nodes in your own Azure subscription.</span></span>
<span data-ttu-id="60e32-112">Účty, které přidělit výpočetní uzly do předplatného musí být ověřeny pomocí tokenu Azure Active Directory a hello výpočetních uzlů přidělených se bude započítávat kvóty předplatného.</span><span class="sxs-lookup"><span data-stu-id="60e32-112">Accounts that allocate compute nodes into your subscription must be authenticated via an Azure Active Directory token and hello compute nodes allocated will count towards your subscription quota.</span></span> <span data-ttu-id="60e32-113">toocreate účet v tomto režimu, při vytváření účtu hello jeden musíte zadat odkaz Key Vault.</span><span class="sxs-lookup"><span data-stu-id="60e32-113">toocreate an account in this mode, one must specify a Key Vault reference when creating hello account.</span></span>

[!code-azurecli[main](../../../cli_scripts/batch/create-account/create-account-user-subscription.sh  "Create Account using User Subscription")]

## <a name="clean-up-deployment"></a><span data-ttu-id="60e32-114">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="60e32-114">Clean up deployment</span></span>

<span data-ttu-id="60e32-115">Po spuštění buď hello výše ukázkové skripty, spusťte následující příkaz tooremove hello skupinu prostředků a všechny související prostředky (včetně účty Batch, účty Azure Storage a Azure trezorů klíčů).</span><span class="sxs-lookup"><span data-stu-id="60e32-115">After you run either of hello above sample scripts, run hello following command tooremove the resource group and all related resources (including Batch accounts, Azure Storage accounts and Azure key vaults).</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="60e32-116">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="60e32-116">Script explanation</span></span>

<span data-ttu-id="60e32-117">Tento skript používá hello následující příkazy toocreate skupinu prostředků, účtu Batch a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="60e32-117">This script uses hello following commands toocreate a resource group, Batch account, and all related resources.</span></span> <span data-ttu-id="60e32-118">Každý příkaz v dokumentaci k toocommand specifické hello tabulky odkazů.</span><span class="sxs-lookup"><span data-stu-id="60e32-118">Each command in hello table links toocommand-specific documentation.</span></span>

| <span data-ttu-id="60e32-119">Příkaz</span><span class="sxs-lookup"><span data-stu-id="60e32-119">Command</span></span> | <span data-ttu-id="60e32-120">Poznámky</span><span class="sxs-lookup"><span data-stu-id="60e32-120">Notes</span></span> |
|---|---|
| [<span data-ttu-id="60e32-121">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="60e32-121">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="60e32-122">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="60e32-122">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="60e32-123">Vytvoření účtu batch az</span><span class="sxs-lookup"><span data-stu-id="60e32-123">az batch account create</span></span>](https://docs.microsoft.com/cli/azure/batch/account#create) | <span data-ttu-id="60e32-124">Vytvoří účet Batch hello.</span><span class="sxs-lookup"><span data-stu-id="60e32-124">Creates hello Batch account.</span></span>  |
| [<span data-ttu-id="60e32-125">nastaven účet batch az</span><span class="sxs-lookup"><span data-stu-id="60e32-125">az batch account set</span></span>](https://docs.microsoft.com/cli/azure/batch/account#set) | <span data-ttu-id="60e32-126">Aktualizuje vlastnosti hello účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="60e32-126">Updates properties of hello Batch account.</span></span>  |
| [<span data-ttu-id="60e32-127">Zobrazit účet batch az</span><span class="sxs-lookup"><span data-stu-id="60e32-127">az batch account show</span></span>](https://docs.microsoft.com/cli/azure/batch/account#show) | <span data-ttu-id="60e32-128">Načte podrobnosti o hello zadat účet Batch.</span><span class="sxs-lookup"><span data-stu-id="60e32-128">Retrieves details of hello specified Batch account.</span></span>  |
| [<span data-ttu-id="60e32-129">seznam klíčů účtu batch az</span><span class="sxs-lookup"><span data-stu-id="60e32-129">az batch account keys list</span></span>](https://docs.microsoft.com/cli/azure/batch/account/keys#list) | <span data-ttu-id="60e32-130">Načte hello přístup klíče hello zadat účet Batch.</span><span class="sxs-lookup"><span data-stu-id="60e32-130">Retrieves hello access keys of hello specified Batch account.</span></span>  |
| [<span data-ttu-id="60e32-131">připojení k účtu batch az</span><span class="sxs-lookup"><span data-stu-id="60e32-131">az batch account login</span></span>](https://docs.microsoft.com/cli/azure/batch/account#login) | <span data-ttu-id="60e32-132">Ověří proti hello zadán účet pro další interakce rozhraní příkazového řádku Batch.</span><span class="sxs-lookup"><span data-stu-id="60e32-132">Authenticates against hello specified Batch account for further CLI interaction.</span></span>  |
| [<span data-ttu-id="60e32-133">Vytvořit účet úložiště az</span><span class="sxs-lookup"><span data-stu-id="60e32-133">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/storage/account#create) | <span data-ttu-id="60e32-134">Vytvoří účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="60e32-134">Creates a storage account.</span></span> |
| [<span data-ttu-id="60e32-135">Vytvoření az keyvault</span><span class="sxs-lookup"><span data-stu-id="60e32-135">az keyvault create</span></span>](https://docs.microsoft.com/cli/azure/keyvault#create) | <span data-ttu-id="60e32-136">Vytvoří trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="60e32-136">Creates a key vault.</span></span> |
| [<span data-ttu-id="60e32-137">AZ keyvault set zásad</span><span class="sxs-lookup"><span data-stu-id="60e32-137">az keyvault set-policy</span></span>](https://docs.microsoft.com/cli/azure/keyvault#set-policy) | <span data-ttu-id="60e32-138">Aktualizujte zásady zabezpečení hello hello zadaný trezor klíčů.</span><span class="sxs-lookup"><span data-stu-id="60e32-138">Update hello security policy of hello specified key vault.</span></span> |
| [<span data-ttu-id="60e32-139">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="60e32-139">az group delete</span></span>](https://docs.microsoft.com/cli/azure/group#delete) | <span data-ttu-id="60e32-140">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="60e32-140">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="60e32-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="60e32-141">Next steps</span></span>

<span data-ttu-id="60e32-142">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="60e32-142">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="60e32-143">Další ukázky skriptu rozhraní příkazového řádku Batch lze nalézt v hello [dokumentaci k rozhraní příkazového řádku Azure Batch](../batch-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="60e32-143">Additional Batch CLI script samples can be found in hello [Azure Batch CLI documentation](../batch-cli-samples.md).</span></span>
