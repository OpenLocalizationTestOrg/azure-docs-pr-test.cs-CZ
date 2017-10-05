---
title: "Ukázka skriptu rozhraní příkazového řádku Azure - škálování clusteru služby ACS | Microsoft Docs"
description: "Ukázka skriptu rozhraní příkazového řádku Azure - škálování clusteru služby ACS"
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
ms.openlocfilehash: 14e9f9d85bc0c1428240f15831632eafe2a0f80e
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="scale-an-azure-container-service-cluster"></a><span data-ttu-id="de0f2-104">Škálování clusteru Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="de0f2-104">Scale an Azure Container Service Cluster</span></span>

<span data-ttu-id="de0f2-105">Tato ukázka měřítek a Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="de0f2-105">This sample scales and Azure Container Service.</span></span> 

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="de0f2-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="de0f2-106">Sample script</span></span>

```azurecli
az acs scale --resource-group myResourceGroup --name myK8SCluster --new-agent-count 5
```

## <a name="clean-up-deployment"></a><span data-ttu-id="de0f2-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="de0f2-107">Clean up deployment</span></span> 

<span data-ttu-id="de0f2-108">Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="de0f2-108">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="de0f2-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="de0f2-109">Script explanation</span></span>

<span data-ttu-id="de0f2-110">Tento skript používá následující příkazy k vytvoření nasazení.</span><span class="sxs-lookup"><span data-stu-id="de0f2-110">This script uses the following commands to create the deployment.</span></span> <span data-ttu-id="de0f2-111">Každou položku v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="de0f2-111">Each item in the table links to command specific documentation.</span></span>

| <span data-ttu-id="de0f2-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="de0f2-112">Command</span></span> | <span data-ttu-id="de0f2-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="de0f2-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="de0f2-114">škálování služby acs az</span><span class="sxs-lookup"><span data-stu-id="de0f2-114">az acs scale</span></span>](/cli/azure/acs#scale) | <span data-ttu-id="de0f2-115">Škálování clusteru služby ACS.</span><span class="sxs-lookup"><span data-stu-id="de0f2-115">Scale an ACS cluster.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="de0f2-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="de0f2-116">Next steps</span></span>

<span data-ttu-id="de0f2-117">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="de0f2-117">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="de0f2-118">Další ukázky skript příkazového řádku Azure Container Service najdete v [dokumentace Azure Container Service](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="de0f2-118">Additional Azure Container Service CLI script samples can be found in the [Azure Container Service documentation](../cli-samples.md).</span></span>

