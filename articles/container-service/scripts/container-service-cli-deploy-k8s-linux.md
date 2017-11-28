---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vytvoření clusteru Kubernetes Linux ACS | Microsoft Docs"
description: "Rozhraní příkazového řádku Azure ukázkový skript – vytvoření clusteru Kubernetes Linux ACS"
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
ms.openlocfilehash: 811929eea4fdf62d8c3de54e81cbc7f6cda858c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-container-service-kubernetes-linux-cluster"></a><span data-ttu-id="eff92-104">Vytvoření clusteru Azure Container Service Kubernetes Linux</span><span class="sxs-lookup"><span data-stu-id="eff92-104">Create an Azure Container Service Kubernetes Linux Cluster</span></span>

<span data-ttu-id="eff92-105">Tato ukázka vytvoří cluster služby Azure Container Service spuštěná Kubernetes pro kontejnery založenými na systému Linux.</span><span class="sxs-lookup"><span data-stu-id="eff92-105">This sample creates an Azure Container Service cluster running Kubernetes for Linux based containers.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="eff92-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="eff92-106">Sample script</span></span>

```azurecli
az group create --name myResourceGroup --location eastus

az acs create \
  --orchestrator-type kubernetes \
  --resource-group myResourceGroup \
  --name myK8SCluster \
  --generate-ssh-keys
```

## <a name="clean-up-deployment"></a><span data-ttu-id="eff92-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="eff92-107">Clean up deployment</span></span> 

<span data-ttu-id="eff92-108">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="eff92-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="eff92-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="eff92-109">Script explanation</span></span>

<span data-ttu-id="eff92-110">Tento skript používá hello následující příkazy toocreate hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="eff92-110">This script uses hello following commands toocreate hello deployment.</span></span> <span data-ttu-id="eff92-111">Každou položku v tabulce hello propojí toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="eff92-111">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="eff92-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="eff92-112">Command</span></span> | <span data-ttu-id="eff92-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="eff92-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="eff92-114">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="eff92-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="eff92-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="eff92-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="eff92-116">vytvoření služby acs az</span><span class="sxs-lookup"><span data-stu-id="eff92-116">az acs create</span></span>](https://docs.microsoft.com/cli/azure/acs#create) | <span data-ttu-id="eff92-117">Vytvoří a cluster služby ACS.</span><span class="sxs-lookup"><span data-stu-id="eff92-117">Creates and ACS cluster.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="eff92-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="eff92-118">Next steps</span></span>

<span data-ttu-id="eff92-119">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="eff92-119">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="eff92-120">Další ukázky skript příkazového řádku Azure Container Service najdete v hello [dokumentace Azure Container Service](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="eff92-120">Additional Azure Container Service CLI script samples can be found in hello [Azure Container Service documentation](../cli-samples.md).</span></span>

