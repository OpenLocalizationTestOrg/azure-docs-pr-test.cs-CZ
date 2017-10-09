---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vytvoření clusteru ACS Windows Kubernetes | Microsoft Docs"
description: "Rozhraní příkazového řádku Azure ukázkový skript – vytvoření clusteru Kubernetes ACS systému Windows"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, Kontejnery, mikroslužby, Kubernetes, DC/OS, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/30/2017
ms.author: nepeters
ms.openlocfilehash: afbaf17fb1d5310b50a2f181061339cb2ab87fd9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-container-service-kubernetes-windows-cluster"></a><span data-ttu-id="4b107-104">Vytvoření clusteru Azure Container Service Kubernetes Windows</span><span class="sxs-lookup"><span data-stu-id="4b107-104">Create an Azure Container Service Kubernetes Windows Cluster</span></span>

<span data-ttu-id="4b107-105">Tato ukázka vytvoří cluster služby Azure Container Service spuštěných kontejnerů Kubernetes pro Windows na základě.</span><span class="sxs-lookup"><span data-stu-id="4b107-105">This sample creates an Azure Container Service cluster running Kubernetes for Windows based containers.</span></span>

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="4b107-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="4b107-106">Sample script</span></span>

```azurecli
az group create --name myResourceGroup --location eastus

az acs create \
  --orchestrator-type kubernetes \
  --resource-group myResourceGroup \
  --name myK8SCluster \
  --generate-ssh-keys \
  --admin-username azureuser \
  --admin-password Password12 \
  --windows
```

## <a name="clean-up-deployment"></a><span data-ttu-id="4b107-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="4b107-107">Clean up deployment</span></span> 

<span data-ttu-id="4b107-108">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="4b107-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="4b107-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="4b107-109">Script explanation</span></span>

<span data-ttu-id="4b107-110">Tento skript používá hello následující příkazy toocreate hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="4b107-110">This script uses hello following commands toocreate hello deployment.</span></span> <span data-ttu-id="4b107-111">Každou položku v tabulce hello propojí toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="4b107-111">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="4b107-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="4b107-112">Command</span></span> | <span data-ttu-id="4b107-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="4b107-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="4b107-114">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="4b107-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="4b107-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="4b107-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="4b107-116">vytvoření služby acs az</span><span class="sxs-lookup"><span data-stu-id="4b107-116">az acs create</span></span>](https://docs.microsoft.com/cli/azure/acs#create) | <span data-ttu-id="4b107-117">Vytvoří a cluster služby ACS.</span><span class="sxs-lookup"><span data-stu-id="4b107-117">Creates and ACS cluster.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="4b107-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4b107-118">Next steps</span></span>

<span data-ttu-id="4b107-119">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4b107-119">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="4b107-120">Další ukázky skript příkazového řádku Azure Container Service najdete v hello [dokumentace Azure Container Service](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="4b107-120">Additional Azure Container Service CLI script samples can be found in hello [Azure Container Service documentation](../cli-samples.md).</span></span>
