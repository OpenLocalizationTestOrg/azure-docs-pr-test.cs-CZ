---
title: "Přesun okruhů ExpressRoute z classic do Resource Manager: prostředí PowerShell: Azure | Microsoft Docs"
description: "Tato stránka popisuje, jak přesunout classic okruhu do modelu nasazení Resource Manager pomocí prostředí PowerShell."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 08152836-23e7-42d1-9a56-8306b341cd91
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/03/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: c407e01e6d881cb8adcfe55faa246468669be883
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="move-expressroute-circuits-from-the-classic-to-the-resource-manager-deployment-model-using-powershell"></a><span data-ttu-id="16252-103">Přesun okruhů ExpressRoute z classic do modelu nasazení Resource Manager pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="16252-103">Move ExpressRoute circuits from the classic to the Resource Manager deployment model using PowerShell</span></span>

<span data-ttu-id="16252-104">Pokud chcete použít okruhu ExpressRoute pro classic i modelech nasazení Resource Manager, je třeba přesunout okruh do modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="16252-104">To use an ExpressRoute circuit for both the classic and Resource Manager deployment models, you must move the circuit to the Resource Manager deployment model.</span></span> <span data-ttu-id="16252-105">V následujících částech můžete přesunout váš okruh pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="16252-105">The following sections help you move your circuit by using PowerShell.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="16252-106">Než začnete</span><span class="sxs-lookup"><span data-stu-id="16252-106">Before you begin</span></span>

* <span data-ttu-id="16252-107">Ověřte, že máte nejnovější verzi modulů prostředí Azure PowerShell (minimálně verze 1.0).</span><span class="sxs-lookup"><span data-stu-id="16252-107">Verify that you have the latest version of the Azure PowerShell modules (at least version 1.0).</span></span> <span data-ttu-id="16252-108">Další informace najdete v tématu [Instalace a konfigurace Azure PowerShellu](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="16252-108">For more information, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="16252-109">Ujistěte se, že jste si přečetli [požadavky](expressroute-prerequisites.md), [požadavky na směrování](expressroute-routing.md), a [pracovních](expressroute-workflows.md) před zahájením konfigurace.</span><span class="sxs-lookup"><span data-stu-id="16252-109">Make sure that you have reviewed the [prerequisites](expressroute-prerequisites.md), [routing requirements](expressroute-routing.md), and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>
* <span data-ttu-id="16252-110">Zkontrolujte informace, které jsou poskytovány na základě [přesun okruhu ExpressRoute z classic do Resource Manager](expressroute-move.md).</span><span class="sxs-lookup"><span data-stu-id="16252-110">Review the information that is provided under [Moving an ExpressRoute circuit from classic to Resource Manager](expressroute-move.md).</span></span> <span data-ttu-id="16252-111">Ujistěte se, že rozumíte plně limity a omezení.</span><span class="sxs-lookup"><span data-stu-id="16252-111">Make sure that you fully understand the limits and limitations.</span></span>
* <span data-ttu-id="16252-112">Ověřte, že je okruh v modelu nasazení classic plně funkční.</span><span class="sxs-lookup"><span data-stu-id="16252-112">Verify that the circuit is fully operational in the classic deployment model.</span></span>
* <span data-ttu-id="16252-113">Ujistěte se, že máte skupinu prostředků, který byl vytvořen v modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="16252-113">Ensure that you have a resource group that was created in the Resource Manager deployment model.</span></span>

## <a name="move-an-expressroute-circuit"></a><span data-ttu-id="16252-114">Přesun okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="16252-114">Move an ExpressRoute circuit</span></span>

### <a name="step-1-gather-circuit-details-from-the-classic-deployment-model"></a><span data-ttu-id="16252-115">Krok 1: Shromáždění podrobností okruh z modelu nasazení classic</span><span class="sxs-lookup"><span data-stu-id="16252-115">Step 1: Gather circuit details from the classic deployment model</span></span>

<span data-ttu-id="16252-116">Přihlaste se k prostředí Azure classic a shromáždit klíč služby.</span><span class="sxs-lookup"><span data-stu-id="16252-116">Sign in to the Azure classic environment and gather the service key.</span></span>

1. <span data-ttu-id="16252-117">Přihlaste se k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="16252-117">Sign in to your Azure account.</span></span>

  ```powershell
  Add-AzureAccount
  ```

2. <span data-ttu-id="16252-118">Vyberte příslušné předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="16252-118">Select the appropriate Azure subscription.</span></span>

  ```powershell
  Select-AzureSubscription "<Enter Subscription Name here>"
  ```

3. <span data-ttu-id="16252-119">Importujte další moduly Powershellu pro Azure a ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="16252-119">Import the PowerShell modules for Azure and ExpressRoute.</span></span>

  ```powershell
  Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
  Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'
  ```

4. <span data-ttu-id="16252-120">Použijte následující rutinu k získání klíče služby pro všechny vaše okruhy ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="16252-120">Use the cmdlet below to get the service keys for all of your ExpressRoute circuits.</span></span> <span data-ttu-id="16252-121">Po načtení klíče, zkopírujte **klíč služby** okruhu, který chcete přesunout do modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="16252-121">After retrieving the keys, copy the **service key** of the circuit that you want to move to the Resource Manager deployment model.</span></span>

  ```powershell
  Get-AzureDedicatedCircuit
  ```

### <a name="step-2-sign-in-and-create-a-resource-group"></a><span data-ttu-id="16252-122">Krok 2: Přihlaste se a vytvořte skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="16252-122">Step 2: Sign in and create a resource group</span></span>

<span data-ttu-id="16252-123">Přihlaste se k prostředí Resource Manager a vytvořte novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="16252-123">Sign in to the Resource Manager environment and create a new resource group.</span></span>

1. <span data-ttu-id="16252-124">Přihlaste se do prostředí Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="16252-124">Sign in to your Azure Resource Manager environment.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

2. <span data-ttu-id="16252-125">Vyberte příslušné předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="16252-125">Select the appropriate Azure subscription.</span></span>

  ```powershell
  Get-AzureRmSubscription -SubscriptionName "<Enter Subscription Name here>" | Select-AzureRmSubscription
  ```

3. <span data-ttu-id="16252-126">Chcete-li vytvořit novou skupinu prostředků, pokud ještě nemáte skupinu prostředků fragment upravte.</span><span class="sxs-lookup"><span data-stu-id="16252-126">Modify the snippet below to create a new resource group if you don't already have a resource group.</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name "DemoRG" -Location "West US"
  ```

### <a name="step-3-move-the-expressroute-circuit-to-the-resource-manager-deployment-model"></a><span data-ttu-id="16252-127">Krok 3: Přesunout okruh ExpressRoute do modelu nasazení Resource Manager</span><span class="sxs-lookup"><span data-stu-id="16252-127">Step 3: Move the ExpressRoute circuit to the Resource Manager deployment model</span></span>

<span data-ttu-id="16252-128">Nyní jste připraveni pro přesun okruhu ExpressRoute z modelu nasazení classic do modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="16252-128">You are now ready to move your ExpressRoute circuit from the classic deployment model to the Resource Manager deployment model.</span></span> <span data-ttu-id="16252-129">Než budete pokračovat, přečtěte si informace uvedené v [přesun okruhu ExpressRoute z klasického modelu nasazení Resource Manager](expressroute-move.md).</span><span class="sxs-lookup"><span data-stu-id="16252-129">Before proceeding, review the information provided in [Moving an ExpressRoute circuit from the classic to the Resource Manager deployment model](expressroute-move.md).</span></span>

<span data-ttu-id="16252-130">Chcete-li přesunout váš okruh, upravit a spusťte následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="16252-130">To move your circuit, modify and run the following snippet:</span></span>

```powershell
Move-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "DemoRG" -Location "West US" -ServiceKey "<Service-key>"
```

> [!NOTE]
> <span data-ttu-id="16252-131">Po dokončení přesunu nový název, který je uvedený v předchozí rutiny bude používat k adresování prostředku.</span><span class="sxs-lookup"><span data-stu-id="16252-131">After the move has finished, the new name that is listed in the previous cmdlet will be used to address the resource.</span></span> <span data-ttu-id="16252-132">Okruhu bude v podstatě přejmenovat.</span><span class="sxs-lookup"><span data-stu-id="16252-132">The circuit will essentially be renamed.</span></span>
> 

## <a name="modify-circuit-access"></a><span data-ttu-id="16252-133">Úprava okruhu přístup</span><span class="sxs-lookup"><span data-stu-id="16252-133">Modify circuit access</span></span>

### <a name="to-enable-expressroute-circuit-access-for-both-deployment-models"></a><span data-ttu-id="16252-134">Chcete-li povolit přístup k okruhu ExpressRoute pro oba modely nasazení</span><span class="sxs-lookup"><span data-stu-id="16252-134">To enable ExpressRoute circuit access for both deployment models</span></span>

<span data-ttu-id="16252-135">Po přesunutí classic okruhu ExpressRoute do modelu nasazení Resource Manager, můžete povolit přístup k oběma modelům nasazení.</span><span class="sxs-lookup"><span data-stu-id="16252-135">After moving your classic ExpressRoute circuit to the Resource Manager deployment model, you can enable access to both deployment models.</span></span> <span data-ttu-id="16252-136">Spusťte následující rutiny a povolte tak přístup k oběma modelům nasazení:</span><span class="sxs-lookup"><span data-stu-id="16252-136">Run the following cmdlets to enable access to both deployment models:</span></span>

1. <span data-ttu-id="16252-137">Získání podrobností o okruhu.</span><span class="sxs-lookup"><span data-stu-id="16252-137">Get the circuit details.</span></span>

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"
  ```

2. <span data-ttu-id="16252-138">Nastavení "Povolit klasické operace" na hodnotu TRUE.</span><span class="sxs-lookup"><span data-stu-id="16252-138">Set "Allow Classic Operations" to TRUE.</span></span>

  ```powershell
  $ckt.AllowClassicOperations = $true
  ```

3. <span data-ttu-id="16252-139">Aktualizujte okruh.</span><span class="sxs-lookup"><span data-stu-id="16252-139">Update the circuit.</span></span> <span data-ttu-id="16252-140">Po úspěšném dokončení této operace bude moci zobrazit okruh v modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="16252-140">After this operation has finished successfully, you will be able to view the circuit in the classic deployment model.</span></span>

  ```powershell
  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

4. <span data-ttu-id="16252-141">Spusťte následující rutiny můžete získat podrobnosti o okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="16252-141">Run the following cmdlet to get the details of the ExpressRoute circuit.</span></span> <span data-ttu-id="16252-142">Musí umět klíč služby uvedené v tématu.</span><span class="sxs-lookup"><span data-stu-id="16252-142">You must be able to see the service key listed.</span></span>

  ```powershell
  get-azurededicatedcircuit
  ```

5. <span data-ttu-id="16252-143">Teď můžete spravovat odkazy na okruh ExpressRoute pomocí příkazů modelu nasazení classic pro virtuální sítě classic a Resource Manager příkazy pro virtuální sítě Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="16252-143">You can now manage links to the ExpressRoute circuit using the classic deployment model commands for classic VNets, and the Resource Manager commands for Resource Manager VNets.</span></span> <span data-ttu-id="16252-144">Tyto články vám pomůžou spravovat odkazy na okruh ExpressRoute:</span><span class="sxs-lookup"><span data-stu-id="16252-144">The following articles help you manage links to the ExpressRoute circuit:</span></span>

    * [<span data-ttu-id="16252-145">Propojení virtuální sítě k okruhu ExpressRoute v modelu nasazení Resource Manager</span><span class="sxs-lookup"><span data-stu-id="16252-145">Link your virtual network to your ExpressRoute circuit in the Resource Manager deployment model</span></span>](expressroute-howto-linkvnet-arm.md)
    * [<span data-ttu-id="16252-146">Propojení virtuální sítě k okruhu ExpressRoute v modelu nasazení classic</span><span class="sxs-lookup"><span data-stu-id="16252-146">Link your virtual network to your ExpressRoute circuit in the classic deployment model</span></span>](expressroute-howto-linkvnet-classic.md)

### <a name="to-disable-expressroute-circuit-access-to-the-classic-deployment-model"></a><span data-ttu-id="16252-147">Chcete-li zakázat přístup k okruhu ExpressRoute do modelu nasazení classic</span><span class="sxs-lookup"><span data-stu-id="16252-147">To disable ExpressRoute circuit access to the classic deployment model</span></span>

<span data-ttu-id="16252-148">Spusťte následující rutiny můžete zakázat přístup k modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="16252-148">Run the following cmdlets to disable access to the classic deployment model.</span></span>

1. <span data-ttu-id="16252-149">Získáte podrobnosti o okruhu ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="16252-149">Get details of the ExpressRoute circuit.</span></span>

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"
  ```

2. <span data-ttu-id="16252-150">Nastavení "Povolit klasické operace" na hodnotu FALSE.</span><span class="sxs-lookup"><span data-stu-id="16252-150">Set "Allow Classic Operations" to FALSE.</span></span>

  ```powershell
  $ckt.AllowClassicOperations = $false
  ```

3. <span data-ttu-id="16252-151">Aktualizujte okruh.</span><span class="sxs-lookup"><span data-stu-id="16252-151">Update the circuit.</span></span> <span data-ttu-id="16252-152">Po této operaci byl úspěšně dokončen, nebudete moci zobrazit okruh v modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="16252-152">After this operation has finished successfully, you will not be able to view the circuit in the classic deployment model.</span></span>

  ```powershell
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

## <a name="next-steps"></a><span data-ttu-id="16252-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="16252-153">Next steps</span></span>

* [<span data-ttu-id="16252-154">Vytvoření a úprava směrování pro okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="16252-154">Create and modify routing for your ExpressRoute circuit</span></span>](expressroute-howto-routing-arm.md)
* [<span data-ttu-id="16252-155">Propojení virtuální sítě k okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="16252-155">Link your virtual network to your ExpressRoute circuit</span></span>](expressroute-howto-linkvnet-arm.md)