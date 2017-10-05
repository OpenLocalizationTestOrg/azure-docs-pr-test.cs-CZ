---
title: "Azure skript prostředí PowerShell ukázkový – vytvořit cluster Service Fabric | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – vytvořit cluster Service Fabric."
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 0f9c8bc5-3789-4eb3-8deb-ae6e2200795a
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 7cbeb3da695af3815ba660f9cc2e3388abb6f87d
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="create-a-service-fabric-cluster"></a><span data-ttu-id="e0d8f-103">Vytvořit cluster Service Fabric</span><span class="sxs-lookup"><span data-stu-id="e0d8f-103">Create a Service Fabric cluster</span></span>

<span data-ttu-id="e0d8f-104">Tento ukázkový skript vytvoří cluster Service Fabric pěti uzly clusteru s podporou zabezpečená certifikátem X.509.</span><span class="sxs-lookup"><span data-stu-id="e0d8f-104">This sample script creates a Service Fabric cluster a five-node cluster secured with an X.509 certificate.</span></span>  <span data-ttu-id="e0d8f-105">Příkaz vytvoří certifikát podepsaný svým držitelem a odešle ji do nového trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="e0d8f-105">The command creates a self-signed certificate and uploads it to a new key vault.</span></span> <span data-ttu-id="e0d8f-106">Certifikát je také zkopírován do místního adresáře.</span><span class="sxs-lookup"><span data-stu-id="e0d8f-106">The certificate is also copied to a local directory.</span></span>  <span data-ttu-id="e0d8f-107">Nastavte *-OS* parametru zvolte verzi systému Windows nebo Linux, který běží na uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="e0d8f-107">Set the *-OS* parameter to choose the version of Windows or Linux that runs on the cluster nodes.</span></span>  <span data-ttu-id="e0d8f-108">Podle potřeby upravte požadované parametry.</span><span class="sxs-lookup"><span data-stu-id="e0d8f-108">Customize the parameters as needed.</span></span>

<span data-ttu-id="e0d8f-109">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí instrukce v nalezen [prostředí Azure PowerShell průvodce](/powershell/azure/overview) a spusťte `Login-AzureRmAccount` vytvořit připojení s Azure.</span><span class="sxs-lookup"><span data-stu-id="e0d8f-109">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](/powershell/azure/overview) and then run `Login-AzureRmAccount` to create a connection with Azure.</span></span> 

## <a name="sample-script"></a><span data-ttu-id="e0d8f-110">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="e0d8f-110">Sample script</span></span>

<span data-ttu-id="e0d8f-111">[!code-powershell[hlavní](../../../powershell_scripts/service-fabric/create-secure-cluster/create-secure-cluster.ps1 "vytvořit cluster Service Fabric")]</span><span class="sxs-lookup"><span data-stu-id="e0d8f-111">[!code-powershell[main](../../../powershell_scripts/service-fabric/create-secure-cluster/create-secure-cluster.ps1 "Create a Service Fabric cluster")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="e0d8f-112">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="e0d8f-112">Clean up deployment</span></span> 

<span data-ttu-id="e0d8f-113">Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků clusteru a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="e0d8f-113">After the script sample has been run, the following command can be used to remove the resource group, cluster, and all related resources.</span></span>

```powershell
$groupname="mysfclustergroup"
Remove-AzureRmResourceGroup -Name $groupname -Force
```

## <a name="script-explanation"></a><span data-ttu-id="e0d8f-114">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="e0d8f-114">Script explanation</span></span>

<span data-ttu-id="e0d8f-115">Tento skript používá následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="e0d8f-115">This script uses the following commands.</span></span> <span data-ttu-id="e0d8f-116">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="e0d8f-116">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="e0d8f-117">Příkaz</span><span class="sxs-lookup"><span data-stu-id="e0d8f-117">Command</span></span> | <span data-ttu-id="e0d8f-118">Poznámky</span><span class="sxs-lookup"><span data-stu-id="e0d8f-118">Notes</span></span> |
|---|---|
| [<span data-ttu-id="e0d8f-119">Nové AzureRmServiceFabricCluster</span><span class="sxs-lookup"><span data-stu-id="e0d8f-119">New-AzureRmServiceFabricCluster</span></span>](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) | <span data-ttu-id="e0d8f-120">Vytvoří nový cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="e0d8f-120">Creates a new Service Fabric cluster.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="e0d8f-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e0d8f-121">Next steps</span></span>

<span data-ttu-id="e0d8f-122">Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="e0d8f-122">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="e0d8f-123">Další ukázky pro Azure Service Fabric Azure Powershell lze nalézt v [prostředí Azure PowerShell ukázky](../service-fabric-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="e0d8f-123">Additional Azure Powershell samples for Azure Service Fabric can be found in the [Azure PowerShell samples](../service-fabric-powershell-samples.md).</span></span>
