---
title: "aaaAzure ukázka skriptu prostředí PowerShell - směrovat provoz pro vysokou dostupnost aplikací | Microsoft Docs"
description: "Azure ukázkový skript prostředí PowerShell - směrovat provoz pro vysokou dostupnost aplikací"
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
manager: timlt
editor: georgewallace
tags: azure-infrastructure
ms.assetid: 
ms.service: traffic-manager
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: traffic-manager
ms.date: 05/16/2017
ms.author: gwallace
ms.openlocfilehash: 11d15780403b4ed79e85d7b3495bc5d674bfdaee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="route-traffic-for-high-availability-of-applications"></a><span data-ttu-id="fc342-103">Směrovat provoz pro vysokou dostupnost aplikací</span><span class="sxs-lookup"><span data-stu-id="fc342-103">Route traffic for high availability of applications</span></span>

<span data-ttu-id="fc342-104">Tento skript vytvoří skupinu prostředků, dva druhy služeb aplikace, dva webové aplikace, profil správce provozu a dva koncové body správce provozu.</span><span class="sxs-lookup"><span data-stu-id="fc342-104">This script creates a resource group, two app service plans, two web apps, a traffic manager profile, and two traffic manager endpoints.</span></span> <span data-ttu-id="fc342-105">Traffic Manager směrovat provoz toohello aplikaci v jedné oblasti jako primární oblasti hello a sekundární oblasti toohello když aplikace hello v primární oblasti hello není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="fc342-105">Traffic Manager directs traffic toohello application in one region as hello primary region, and toohello secondary region when hello application in hello primary region is unavailable.</span></span> <span data-ttu-id="fc342-106">Před provedením hello skriptu, je nutné změnit hello MyWebApp, MyWebAppL1 a MyWebAppL2 hodnoty toounique hodnoty v Azure.</span><span class="sxs-lookup"><span data-stu-id="fc342-106">Before executing hello script, you must change hello MyWebApp, MyWebAppL1 and MyWebAppL2 values toounique values across Azure.</span></span> <span data-ttu-id="fc342-107">Po spuštění skriptu hello, budete mít přístup aplikace hello v hello primární oblasti s mywebapp.trafficmanager.net hello adresy URL.</span><span class="sxs-lookup"><span data-stu-id="fc342-107">After running hello script, you can access hello app in hello primary region with hello URL mywebapp.trafficmanager.net.</span></span>

<span data-ttu-id="fc342-108">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí hello instrukce najít v hello hello [prostředí Azure PowerShell průvodce](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/)a poté spusťte `Login-AzureRmAccount` toocreate připojení s Azure.</span><span class="sxs-lookup"><span data-stu-id="fc342-108">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="fc342-109">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="fc342-109">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/traffic-manager/direct-traffic-for-increased-application-availability/direct-traffic-for-increased-application-availability.ps1 "Route traffic for high availability")]


<span data-ttu-id="fc342-110">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="fc342-110">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup1
Remove-AzureRmResourceGroup -Name myResourceGroup2
```


## <a name="script-explanation"></a><span data-ttu-id="fc342-111">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="fc342-111">Script explanation</span></span>

<span data-ttu-id="fc342-112">Tento skript používá hello následující příkazy toocreate skupinu prostředků, webové aplikace, profil správce provozu a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="fc342-112">This script uses hello following commands toocreate a resource group, web app, traffic manager profile, and all related resources.</span></span> <span data-ttu-id="fc342-113">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="fc342-113">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="fc342-114">Příkaz</span><span class="sxs-lookup"><span data-stu-id="fc342-114">Command</span></span> | <span data-ttu-id="fc342-115">Poznámky</span><span class="sxs-lookup"><span data-stu-id="fc342-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="fc342-116">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="fc342-116">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup)  | <span data-ttu-id="fc342-117">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="fc342-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="fc342-118">Nové AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="fc342-118">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="fc342-119">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="fc342-119">Creates an App Service plan.</span></span> <span data-ttu-id="fc342-120">Toto je jako serverové farmy pro Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="fc342-120">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="fc342-121">Nové AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="fc342-121">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="fc342-122">Vytvoří webové aplikace Azure v rámci hello plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="fc342-122">Creates an Azure web app within hello App Service plan.</span></span> |
| [<span data-ttu-id="fc342-123">Set-AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="fc342-123">Set-AzureRmResource</span></span>](/powershell/module/azurerm.resources/new-azurermresource) | <span data-ttu-id="fc342-124">Vytvoří webové aplikace Azure v rámci hello plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="fc342-124">Creates an Azure web app within hello App Service plan.</span></span> |
| [<span data-ttu-id="fc342-125">Nové AzureRmTrafficManagerProfile</span><span class="sxs-lookup"><span data-stu-id="fc342-125">New-AzureRmTrafficManagerProfile</span></span>](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerprofile) | <span data-ttu-id="fc342-126">Vytvoří profilu Azure Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="fc342-126">Creates an Azure Traffic Manager profile.</span></span> |
| [<span data-ttu-id="fc342-127">Nové AzureRmTrafficManagerEndpoint</span><span class="sxs-lookup"><span data-stu-id="fc342-127">New-AzureRmTrafficManagerEndpoint</span></span>](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerendpoint) | <span data-ttu-id="fc342-128">Přidá tooan koncový bod profilu služby Traffic Manager Azure.</span><span class="sxs-lookup"><span data-stu-id="fc342-128">Adds a endpoint tooan Azure Traffic Manager Profile.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="fc342-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fc342-129">Next steps</span></span>

<span data-ttu-id="fc342-130">Další informace o hello prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="fc342-130">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/azure/overview).</span></span>

<span data-ttu-id="fc342-131">Další ukázky sítě skript prostředí PowerShell najdete v hello [přehled sítě Azure dokumentaci](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="fc342-131">Additional networking PowerShell script samples can be found in hello [Azure Networking Overview documentation](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span></span>
