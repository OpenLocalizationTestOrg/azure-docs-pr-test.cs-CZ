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
ms.openlocfilehash: 0ea2c73436e650fdb37535538baccc565369fa9d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="scale-an-azure-container-service-cluster"></a><span data-ttu-id="5518a-104">Škálování clusteru Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="5518a-104">Scale an Azure Container Service Cluster</span></span>

<span data-ttu-id="5518a-105">Tato ukázka měřítek a Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="5518a-105">This sample scales and Azure Container Service.</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="5518a-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="5518a-106">Sample script</span></span>

```azurecli
az acs scale --resource-group myResourceGroup --name myK8SCluster --new-agent-count 5
```

## <a name="clean-up-deployment"></a><span data-ttu-id="5518a-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="5518a-107">Clean up deployment</span></span> 

<span data-ttu-id="5518a-108">Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="5518a-108">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="5518a-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="5518a-109">Script explanation</span></span>

<span data-ttu-id="5518a-110">Tento skript používá následující příkazy k vytvoření nasazení.</span><span class="sxs-lookup"><span data-stu-id="5518a-110">This script uses the following commands to create the deployment.</span></span> <span data-ttu-id="5518a-111">Každou položku v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="5518a-111">Each item in the table links to command specific documentation.</span></span>

| <span data-ttu-id="5518a-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="5518a-112">Command</span></span> | <span data-ttu-id="5518a-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="5518a-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="5518a-114">škálování služby acs az</span><span class="sxs-lookup"><span data-stu-id="5518a-114">az acs scale</span></span>](/cli/azure/acs#scale) | <span data-ttu-id="5518a-115">Škálování clusteru služby ACS.</span><span class="sxs-lookup"><span data-stu-id="5518a-115">Scale an ACS cluster.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="5518a-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5518a-116">Next steps</span></span>

<span data-ttu-id="5518a-117">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="5518a-117">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="5518a-118">Další ukázky skript příkazového řádku Azure Container Service najdete v [dokumentace Azure Container Service](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="5518a-118">Additional Azure Container Service CLI script samples can be found in the [Azure Container Service documentation](../cli-samples.md).</span></span>

