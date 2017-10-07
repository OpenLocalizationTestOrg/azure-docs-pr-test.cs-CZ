---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - škálování clusteru služby ACS | Microsoft Docs"
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
ms.openlocfilehash: 1e07518fc2ca67476d9ef64bb22d75f848a37e43
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scale-an-azure-container-service-cluster"></a><span data-ttu-id="dff41-104">Škálování clusteru Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="dff41-104">Scale an Azure Container Service Cluster</span></span>

<span data-ttu-id="dff41-105">Tato ukázka měřítek a Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="dff41-105">This sample scales and Azure Container Service.</span></span> 

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="dff41-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="dff41-106">Sample script</span></span>

```azurecli
az acs scale --resource-group myResourceGroup --name myK8SCluster --new-agent-count 5
```

## <a name="clean-up-deployment"></a><span data-ttu-id="dff41-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="dff41-107">Clean up deployment</span></span> 

<span data-ttu-id="dff41-108">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="dff41-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="dff41-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="dff41-109">Script explanation</span></span>

<span data-ttu-id="dff41-110">Tento skript používá hello následující příkazy toocreate hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="dff41-110">This script uses hello following commands toocreate hello deployment.</span></span> <span data-ttu-id="dff41-111">Každou položku v tabulce hello propojí toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="dff41-111">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="dff41-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="dff41-112">Command</span></span> | <span data-ttu-id="dff41-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="dff41-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="dff41-114">škálování služby acs az</span><span class="sxs-lookup"><span data-stu-id="dff41-114">az acs scale</span></span>](/cli/azure/acs#scale) | <span data-ttu-id="dff41-115">Škálování clusteru služby ACS.</span><span class="sxs-lookup"><span data-stu-id="dff41-115">Scale an ACS cluster.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="dff41-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dff41-116">Next steps</span></span>

<span data-ttu-id="dff41-117">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="dff41-117">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="dff41-118">Další ukázky skript příkazového řádku Azure Container Service najdete v hello [dokumentace Azure Container Service](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="dff41-118">Additional Azure Container Service CLI script samples can be found in hello [Azure Container Service documentation](../cli-samples.md).</span></span>

