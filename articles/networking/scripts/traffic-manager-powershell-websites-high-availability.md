---
title: "Azure ukázkový skript prostředí PowerShell - směrovat provoz pro vysokou dostupnost aplikací | Microsoft Docs"
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
ms.openlocfilehash: 2f0ac4fd1779661aab04bafb217e64af5d619a2f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="route-traffic-for-high-availability-of-applications"></a><span data-ttu-id="2bef9-103">Směrovat provoz pro vysokou dostupnost aplikací</span><span class="sxs-lookup"><span data-stu-id="2bef9-103">Route traffic for high availability of applications</span></span>

<span data-ttu-id="2bef9-104">Tento skript vytvoří skupinu prostředků, dva druhy služeb aplikace, dva webové aplikace, profil správce provozu a dva koncové body správce provozu.</span><span class="sxs-lookup"><span data-stu-id="2bef9-104">This script creates a resource group, two app service plans, two web apps, a traffic manager profile, and two traffic manager endpoints.</span></span> <span data-ttu-id="2bef9-105">Správce provozu přesměruje přenosy na aplikaci v jedné oblasti jako primární oblasti a sekundární oblast, když aplikace v primární oblasti není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="2bef9-105">Traffic Manager directs traffic to the application in one region as the primary region, and to the secondary region when the application in the primary region is unavailable.</span></span> <span data-ttu-id="2bef9-106">Před spuštěním skriptu, musíte změnit hodnoty MyWebApp, MyWebAppL1 a MyWebAppL2 jedinečné hodnoty mezi Azure.</span><span class="sxs-lookup"><span data-stu-id="2bef9-106">Before executing the script, you must change the MyWebApp, MyWebAppL1 and MyWebAppL2 values to unique values across Azure.</span></span> <span data-ttu-id="2bef9-107">Po spuštění skriptu, můžete přístup k aplikaci v primární oblasti s mywebapp.trafficmanager.net adresy URL.</span><span class="sxs-lookup"><span data-stu-id="2bef9-107">After running the script, you can access the app in the primary region with the URL mywebapp.trafficmanager.net.</span></span>

<span data-ttu-id="2bef9-108">V případě potřeby nainstalujte prostředí Azure PowerShell pomocí instrukce v nalezen [prostředí Azure PowerShell průvodce](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/)a poté spusťte `Login-AzureRmAccount` vytvořit připojení s Azure.</span><span class="sxs-lookup"><span data-stu-id="2bef9-108">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), and then run `Login-AzureRmAccount` to create a connection with Azure.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="2bef9-109">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="2bef9-109">Sample script</span></span>

<span data-ttu-id="2bef9-110">[!code-powershell[hlavní](../../../powershell_scripts/traffic-manager/direct-traffic-for-increased-application-availability/direct-traffic-for-increased-application-availability.ps1 "směrování provozu pro zajištění vysoké dostupnosti")]</span><span class="sxs-lookup"><span data-stu-id="2bef9-110">[!code-powershell[main](../../../powershell_scripts/traffic-manager/direct-traffic-for-increased-application-availability/direct-traffic-for-increased-application-availability.ps1 "Route traffic for high availability")]</span></span>


<span data-ttu-id="2bef9-111">Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="2bef9-111">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup1
Remove-AzureRmResourceGroup -Name myResourceGroup2
```


## <a name="script-explanation"></a><span data-ttu-id="2bef9-112">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="2bef9-112">Script explanation</span></span>

<span data-ttu-id="2bef9-113">Tento skript používá následující příkazy k vytvoření skupiny prostředků, webové aplikace, profil správce provozu a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="2bef9-113">This script uses the following commands to create a resource group, web app, traffic manager profile, and all related resources.</span></span> <span data-ttu-id="2bef9-114">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="2bef9-114">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="2bef9-115">Příkaz</span><span class="sxs-lookup"><span data-stu-id="2bef9-115">Command</span></span> | <span data-ttu-id="2bef9-116">Poznámky</span><span class="sxs-lookup"><span data-stu-id="2bef9-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="2bef9-117">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="2bef9-117">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup)  | <span data-ttu-id="2bef9-118">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="2bef9-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="2bef9-119">Nové AzureRmAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="2bef9-119">New-AzureRmAppServicePlan</span></span>](/powershell/module/azurerm.websites/new-azurermappserviceplan) | <span data-ttu-id="2bef9-120">Vytvoří plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="2bef9-120">Creates an App Service plan.</span></span> <span data-ttu-id="2bef9-121">Toto je jako serverové farmy pro Azure webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="2bef9-121">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="2bef9-122">Nové AzureRmWebApp</span><span class="sxs-lookup"><span data-stu-id="2bef9-122">New-AzureRmWebApp</span></span>](/powershell/module/azurerm.websites/new-azurermwebapp) | <span data-ttu-id="2bef9-123">Vytvoří webové aplikace Azure v rámci plánu služby App Service.</span><span class="sxs-lookup"><span data-stu-id="2bef9-123">Creates an Azure web app within the App Service plan.</span></span> |
| [<span data-ttu-id="2bef9-124">Set-AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="2bef9-124">Set-AzureRmResource</span></span>](/powershell/module/azurerm.resources/new-azurermresource) | <span data-ttu-id="2bef9-125">Vytvoří webové aplikace Azure v rámci plánu služby App Service.</span><span class="sxs-lookup"><span data-stu-id="2bef9-125">Creates an Azure web app within the App Service plan.</span></span> |
| [<span data-ttu-id="2bef9-126">Nové AzureRmTrafficManagerProfile</span><span class="sxs-lookup"><span data-stu-id="2bef9-126">New-AzureRmTrafficManagerProfile</span></span>](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerprofile) | <span data-ttu-id="2bef9-127">Vytvoří profilu Azure Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="2bef9-127">Creates an Azure Traffic Manager profile.</span></span> |
| [<span data-ttu-id="2bef9-128">Nové AzureRmTrafficManagerEndpoint</span><span class="sxs-lookup"><span data-stu-id="2bef9-128">New-AzureRmTrafficManagerEndpoint</span></span>](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerendpoint) | <span data-ttu-id="2bef9-129">Koncový bod se přidá do profilu Azure Traffic Manager.</span><span class="sxs-lookup"><span data-stu-id="2bef9-129">Adds a endpoint to an Azure Traffic Manager Profile.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="2bef9-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2bef9-130">Next steps</span></span>

<span data-ttu-id="2bef9-131">Další informace o prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="2bef9-131">For more information on the Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/azure/overview).</span></span>

<span data-ttu-id="2bef9-132">Další ukázky sítě skript prostředí PowerShell najdete v [přehled sítě Azure dokumentaci](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2bef9-132">Additional networking PowerShell script samples can be found in the [Azure Networking Overview documentation](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span></span>