---
title: "aaaAzure ukázkový skript prostředí PowerShell - vytvořit cluster Service Fabric | Microsoft Docs"
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
ms.openlocfilehash: 12fdc201bd51688cb850cd456b1e00442b79c22d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-service-fabric-cluster"></a><span data-ttu-id="b35de-103">Vytvořit cluster Service Fabric</span><span class="sxs-lookup"><span data-stu-id="b35de-103">Create a Service Fabric cluster</span></span>

<span data-ttu-id="b35de-104">Tento ukázkový skript vytvoří cluster Service Fabric pěti uzly clusteru s podporou zabezpečená certifikátem X.509.</span><span class="sxs-lookup"><span data-stu-id="b35de-104">This sample script creates a Service Fabric cluster a five-node cluster secured with an X.509 certificate.</span></span>  <span data-ttu-id="b35de-105">příkaz Hello vytvoří certifikát podepsaný svým držitelem a odešle ho tooa nového trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="b35de-105">hello command creates a self-signed certificate and uploads it tooa new key vault.</span></span> <span data-ttu-id="b35de-106">Hello certifikát je také zkopírovaný tooa místního adresáře.</span><span class="sxs-lookup"><span data-stu-id="b35de-106">hello certificate is also copied tooa local directory.</span></span>  <span data-ttu-id="b35de-107">Sada hello *-OS* parametr toochoose hello verzi systému Windows nebo Linux, který běží na uzly clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="b35de-107">Set hello *-OS* parameter toochoose hello version of Windows or Linux that runs on hello cluster nodes.</span></span>  <span data-ttu-id="b35de-108">Parametry hello přizpůsobte podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="b35de-108">Customize hello parameters as needed.</span></span>

<span data-ttu-id="b35de-109">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí hello instrukce najít v hello hello [prostředí Azure PowerShell průvodce](/powershell/azure/overview) a spusťte `Login-AzureRmAccount` toocreate připojení s Azure.</span><span class="sxs-lookup"><span data-stu-id="b35de-109">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview) and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span> 

## <a name="sample-script"></a><span data-ttu-id="b35de-110">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="b35de-110">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/service-fabric/create-secure-cluster/create-secure-cluster.ps1 "Create a Service Fabric cluster")]

## <a name="clean-up-deployment"></a><span data-ttu-id="b35de-111">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="b35de-111">Clean up deployment</span></span> 

<span data-ttu-id="b35de-112">Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello, clusteru a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="b35de-112">After hello script sample has been run, hello following command can be used tooremove hello resource group, cluster, and all related resources.</span></span>

```powershell
$groupname="mysfclustergroup"
Remove-AzureRmResourceGroup -Name $groupname -Force
```

## <a name="script-explanation"></a><span data-ttu-id="b35de-113">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="b35de-113">Script explanation</span></span>

<span data-ttu-id="b35de-114">Tento skript používá hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="b35de-114">This script uses hello following commands.</span></span> <span data-ttu-id="b35de-115">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="b35de-115">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="b35de-116">Příkaz</span><span class="sxs-lookup"><span data-stu-id="b35de-116">Command</span></span> | <span data-ttu-id="b35de-117">Poznámky</span><span class="sxs-lookup"><span data-stu-id="b35de-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="b35de-118">Nové AzureRmServiceFabricCluster</span><span class="sxs-lookup"><span data-stu-id="b35de-118">New-AzureRmServiceFabricCluster</span></span>](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) | <span data-ttu-id="b35de-119">Vytvoří nový cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="b35de-119">Creates a new Service Fabric cluster.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="b35de-120">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b35de-120">Next steps</span></span>

<span data-ttu-id="b35de-121">Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b35de-121">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="b35de-122">Další ukázky pro Azure Service Fabric Azure Powershell lze nalézt v hello [prostředí Azure PowerShell ukázky](../service-fabric-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="b35de-122">Additional Azure Powershell samples for Azure Service Fabric can be found in hello [Azure PowerShell samples](../service-fabric-powershell-samples.md).</span></span>
