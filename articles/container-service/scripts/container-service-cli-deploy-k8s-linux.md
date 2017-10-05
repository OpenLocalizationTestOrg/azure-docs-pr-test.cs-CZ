---
title: "Rozhraní příkazového řádku Azure ukázkový skript – vytvoření clusteru Kubernetes Linux ACS | Microsoft Docs"
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
ms.openlocfilehash: 46bc37ab9949d070d19a3d8946fd5b00acf7f5d9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-container-service-kubernetes-linux-cluster"></a><span data-ttu-id="383e4-104">Vytvoření clusteru Azure Container Service Kubernetes Linux</span><span class="sxs-lookup"><span data-stu-id="383e4-104">Create an Azure Container Service Kubernetes Linux Cluster</span></span>

<span data-ttu-id="383e4-105">Tato ukázka vytvoří cluster služby Azure Container Service spuštěná Kubernetes pro kontejnery založenými na systému Linux.</span><span class="sxs-lookup"><span data-stu-id="383e4-105">This sample creates an Azure Container Service cluster running Kubernetes for Linux based containers.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="383e4-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="383e4-106">Sample script</span></span>

```azurecli
az group create --name myResourceGroup --location eastus

az acs create \
  --orchestrator-type kubernetes \
  --resource-group myResourceGroup \
  --name myK8SCluster \
  --generate-ssh-keys
```

## <a name="clean-up-deployment"></a><span data-ttu-id="383e4-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="383e4-107">Clean up deployment</span></span> 

<span data-ttu-id="383e4-108">Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="383e4-108">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="383e4-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="383e4-109">Script explanation</span></span>

<span data-ttu-id="383e4-110">Tento skript používá následující příkazy k vytvoření nasazení.</span><span class="sxs-lookup"><span data-stu-id="383e4-110">This script uses the following commands to create the deployment.</span></span> <span data-ttu-id="383e4-111">Každou položku v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="383e4-111">Each item in the table links to command specific documentation.</span></span>

| <span data-ttu-id="383e4-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="383e4-112">Command</span></span> | <span data-ttu-id="383e4-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="383e4-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="383e4-114">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="383e4-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="383e4-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="383e4-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="383e4-116">vytvoření služby acs az</span><span class="sxs-lookup"><span data-stu-id="383e4-116">az acs create</span></span>](https://docs.microsoft.com/cli/azure/acs#create) | <span data-ttu-id="383e4-117">Vytvoří a cluster služby ACS.</span><span class="sxs-lookup"><span data-stu-id="383e4-117">Creates and ACS cluster.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="383e4-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="383e4-118">Next steps</span></span>

<span data-ttu-id="383e4-119">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="383e4-119">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="383e4-120">Další ukázky skript příkazového řádku Azure Container Service najdete v [dokumentace Azure Container Service](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="383e4-120">Additional Azure Container Service CLI script samples can be found in the [Azure Container Service documentation](../cli-samples.md).</span></span>

