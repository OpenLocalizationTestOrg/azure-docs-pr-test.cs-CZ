---
title: "Azure CLI skriptu ukázkové – vytvoření účtu Batch | Microsoft Docs"
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
ms.openlocfilehash: 698978fd717091c49a1375e222f46f4325431223
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-batch-account-with-the-azure-cli"></a><span data-ttu-id="bdb87-103">Vytvoření účtu Batch pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="bdb87-103">Create a Batch account with the Azure CLI</span></span>

<span data-ttu-id="bdb87-104">Tento skript vytvoří účet Azure Batch a ukazuje, jak různé vlastnosti účtu může být dotaz a aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="bdb87-104">This script creates an Azure Batch account and shows how various properties of the account can be queried and updated.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bdb87-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="bdb87-105">Prerequisites</span></span>

<span data-ttu-id="bdb87-106">Instalace rozhraní příkazového řádku Azure pomocí pokynů uvedených v [Průvodce instalací Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli), pokud jste tak již neučinili.</span><span class="sxs-lookup"><span data-stu-id="bdb87-106">Install the Azure CLI using the instructions provided in the [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli), if you have not already done so.</span></span>

## <a name="batch-account-sample-script"></a><span data-ttu-id="bdb87-107">Ukázkový skript účtu batch</span><span class="sxs-lookup"><span data-stu-id="bdb87-107">Batch account sample script</span></span>

<span data-ttu-id="bdb87-108">Při vytváření účtu Batch, ve výchozím nastavení jeho výpočetní uzly jsou přiřazeny interně službou Batch.</span><span class="sxs-lookup"><span data-stu-id="bdb87-108">When you create a Batch account, by default its compute nodes are assigned internally by the Batch service.</span></span> <span data-ttu-id="bdb87-109">Přidělené výpočetní uzly budou platit samostatné základní kvóta a účet může být ověřen prostřednictvím pověření sdílený klíč nebo Azure Active Dirctory tokenu.</span><span class="sxs-lookup"><span data-stu-id="bdb87-109">Allocated compute nodes will be subject to a separate core quota and the account can be authenticated either via Shared Key credentials or an Azure Active Dirctory token.</span></span>

<span data-ttu-id="bdb87-110">[!code-azurecli[hlavní](../../../cli_scripts/batch/create-account/create-account.sh "vytvořit účet")]</span><span class="sxs-lookup"><span data-stu-id="bdb87-110">[!code-azurecli[main](../../../cli_scripts/batch/create-account/create-account.sh "Create Account")]</span></span>

## <a name="batch-account-using-user-subscription-sample-script"></a><span data-ttu-id="bdb87-111">Účet batch pomocí uživatele předplatné ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="bdb87-111">Batch account using user subscription sample script</span></span>

<span data-ttu-id="bdb87-112">Můžete také zvolit Batch vytvořit jeho výpočetní uzly v rámci vlastní předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="bdb87-112">You can also opt to have Batch create its compute nodes in your own Azure subscription.</span></span>
<span data-ttu-id="bdb87-113">Účty, které přidělit výpočetní uzly do předplatného musí být ověřeny pomocí tokenu Azure Active Directory a výpočetních uzlů přidělených se bude započítávat kvóty předplatného.</span><span class="sxs-lookup"><span data-stu-id="bdb87-113">Accounts that allocate compute nodes into your subscription must be authenticated via an Azure Active Directory token and the compute nodes allocated will count towards your subscription quota.</span></span> <span data-ttu-id="bdb87-114">Chcete-li vytvořit účet v tomto režimu, jeden musíte zadat odkaz Key Vault při vytváření účtu.</span><span class="sxs-lookup"><span data-stu-id="bdb87-114">To create an account in this mode, one must specify a Key Vault reference when creating the account.</span></span>

<span data-ttu-id="bdb87-115">[!code-azurecli[hlavní](../../../cli_scripts/batch/create-account/create-account-user-subscription.sh  "vytvořit účet pomocí předplatného uživatele")]</span><span class="sxs-lookup"><span data-stu-id="bdb87-115">[!code-azurecli[main](../../../cli_scripts/batch/create-account/create-account-user-subscription.sh  "Create Account using User Subscription")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="bdb87-116">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="bdb87-116">Clean up deployment</span></span>

<span data-ttu-id="bdb87-117">Po spuštění některý z výše uvedené ukázkové skripty, pomocí následujícího příkazu odeberte skupinu zdrojů a všechny související prostředky (včetně účty Batch, účty Azure Storage a Azure trezorů klíčů).</span><span class="sxs-lookup"><span data-stu-id="bdb87-117">After you run either of the above sample scripts, run the following command to remove the resource group and all related resources (including Batch accounts, Azure Storage accounts and Azure key vaults).</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="bdb87-118">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="bdb87-118">Script explanation</span></span>

<span data-ttu-id="bdb87-119">Tento skript používá následující příkazy k vytvoření skupiny prostředků účtu Batch a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="bdb87-119">This script uses the following commands to create a resource group, Batch account, and all related resources.</span></span> <span data-ttu-id="bdb87-120">Každý příkaz v tabulce odkazy na dokumentaci specifické pro příkaz.</span><span class="sxs-lookup"><span data-stu-id="bdb87-120">Each command in the table links to command-specific documentation.</span></span>

| <span data-ttu-id="bdb87-121">Příkaz</span><span class="sxs-lookup"><span data-stu-id="bdb87-121">Command</span></span> | <span data-ttu-id="bdb87-122">Poznámky</span><span class="sxs-lookup"><span data-stu-id="bdb87-122">Notes</span></span> |
|---|---|
| [<span data-ttu-id="bdb87-123">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="bdb87-123">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="bdb87-124">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="bdb87-124">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="bdb87-125">Vytvoření účtu batch az</span><span class="sxs-lookup"><span data-stu-id="bdb87-125">az batch account create</span></span>](https://docs.microsoft.com/cli/azure/batch/account#create) | <span data-ttu-id="bdb87-126">Vytvoří účet Batch.</span><span class="sxs-lookup"><span data-stu-id="bdb87-126">Creates the Batch account.</span></span>  |
| [<span data-ttu-id="bdb87-127">nastaven účet batch az</span><span class="sxs-lookup"><span data-stu-id="bdb87-127">az batch account set</span></span>](https://docs.microsoft.com/cli/azure/batch/account#set) | <span data-ttu-id="bdb87-128">Aktualizace vlastnosti účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="bdb87-128">Updates properties of the Batch account.</span></span>  |
| [<span data-ttu-id="bdb87-129">Zobrazit účet batch az</span><span class="sxs-lookup"><span data-stu-id="bdb87-129">az batch account show</span></span>](https://docs.microsoft.com/cli/azure/batch/account#show) | <span data-ttu-id="bdb87-130">Načte podrobnosti zadaného účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="bdb87-130">Retrieves details of the specified Batch account.</span></span>  |
| [<span data-ttu-id="bdb87-131">seznam klíčů účtu batch az</span><span class="sxs-lookup"><span data-stu-id="bdb87-131">az batch account keys list</span></span>](https://docs.microsoft.com/cli/azure/batch/account/keys#list) | <span data-ttu-id="bdb87-132">Načte přístupové klíče zadaného účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="bdb87-132">Retrieves the access keys of the specified Batch account.</span></span>  |
| [<span data-ttu-id="bdb87-133">připojení k účtu batch az</span><span class="sxs-lookup"><span data-stu-id="bdb87-133">az batch account login</span></span>](https://docs.microsoft.com/cli/azure/batch/account#login) | <span data-ttu-id="bdb87-134">Ověří proti zadaný účet Batch pro další interakce rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="bdb87-134">Authenticates against the specified Batch account for further CLI interaction.</span></span>  |
| [<span data-ttu-id="bdb87-135">Vytvořit účet úložiště az</span><span class="sxs-lookup"><span data-stu-id="bdb87-135">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/storage/account#create) | <span data-ttu-id="bdb87-136">Vytvoří účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="bdb87-136">Creates a storage account.</span></span> |
| [<span data-ttu-id="bdb87-137">Vytvoření az keyvault</span><span class="sxs-lookup"><span data-stu-id="bdb87-137">az keyvault create</span></span>](https://docs.microsoft.com/cli/azure/keyvault#create) | <span data-ttu-id="bdb87-138">Vytvoří trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="bdb87-138">Creates a key vault.</span></span> |
| [<span data-ttu-id="bdb87-139">AZ keyvault set zásad</span><span class="sxs-lookup"><span data-stu-id="bdb87-139">az keyvault set-policy</span></span>](https://docs.microsoft.com/cli/azure/keyvault#set-policy) | <span data-ttu-id="bdb87-140">Aktualizujte zásady zabezpečení zadaný trezor klíčů.</span><span class="sxs-lookup"><span data-stu-id="bdb87-140">Update the security policy of the specified key vault.</span></span> |
| [<span data-ttu-id="bdb87-141">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="bdb87-141">az group delete</span></span>](https://docs.microsoft.com/cli/azure/group#delete) | <span data-ttu-id="bdb87-142">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="bdb87-142">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="bdb87-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bdb87-143">Next steps</span></span>

<span data-ttu-id="bdb87-144">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="bdb87-144">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="bdb87-145">Další ukázky skriptu Batch rozhraní příkazového řádku najdete v [dokumentaci k rozhraní příkazového řádku Azure Batch](../batch-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="bdb87-145">Additional Batch CLI script samples can be found in the [Azure Batch CLI documentation](../batch-cli-samples.md).</span></span>
