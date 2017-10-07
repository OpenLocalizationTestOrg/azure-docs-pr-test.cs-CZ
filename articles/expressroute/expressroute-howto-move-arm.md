---
title: "Přesun okruhů ExpressRoute z classic tooResource správce: prostředí PowerShell: Azure | Microsoft Docs"
description: "Tato stránka popisuje, jak model toomove toohello classic okruh nasazení Resource Manager pomocí prostředí PowerShell."
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
ms.openlocfilehash: 8dcadafca5e4f40773902cec5786eba1dbe133eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-expressroute-circuits-from-hello-classic-toohello-resource-manager-deployment-model-using-powershell"></a><span data-ttu-id="19571-103">Přesun okruhů ExpressRoute z hello toohello klasického modelu nasazení Resource Manager pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="19571-103">Move ExpressRoute circuits from hello classic toohello Resource Manager deployment model using PowerShell</span></span>

<span data-ttu-id="19571-104">toouse okruhu ExpressRoute pro hello classic a modelech nasazení Resource Manager, musíte přesunout modelu nasazení Resource Manager toohello okruhu hello.</span><span class="sxs-lookup"><span data-stu-id="19571-104">toouse an ExpressRoute circuit for both hello classic and Resource Manager deployment models, you must move hello circuit toohello Resource Manager deployment model.</span></span> <span data-ttu-id="19571-105">Hello následující části vám pomohou přesunutí okruhu pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="19571-105">hello following sections help you move your circuit by using PowerShell.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="19571-106">Než začnete</span><span class="sxs-lookup"><span data-stu-id="19571-106">Before you begin</span></span>

* <span data-ttu-id="19571-107">Ověřte, že máte nejnovější verzi modulů prostředí Azure PowerShell hello hello (minimálně verze 1.0).</span><span class="sxs-lookup"><span data-stu-id="19571-107">Verify that you have hello latest version of hello Azure PowerShell modules (at least version 1.0).</span></span> <span data-ttu-id="19571-108">Další informace najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="19571-108">For more information, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="19571-109">Ujistěte se, že jste si přečetli hello [požadavky](expressroute-prerequisites.md), [požadavky na směrování](expressroute-routing.md), a [pracovních](expressroute-workflows.md) před zahájením konfigurace.</span><span class="sxs-lookup"><span data-stu-id="19571-109">Make sure that you have reviewed hello [prerequisites](expressroute-prerequisites.md), [routing requirements](expressroute-routing.md), and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>
* <span data-ttu-id="19571-110">Zkontrolujte hello informace jsou poskytovány na základě [přesun okruhu ExpressRoute z classic tooResource Manager](expressroute-move.md).</span><span class="sxs-lookup"><span data-stu-id="19571-110">Review hello information that is provided under [Moving an ExpressRoute circuit from classic tooResource Manager](expressroute-move.md).</span></span> <span data-ttu-id="19571-111">Ujistěte se, že rozumíte plně hello limity a omezení.</span><span class="sxs-lookup"><span data-stu-id="19571-111">Make sure that you fully understand hello limits and limitations.</span></span>
* <span data-ttu-id="19571-112">Ověřte, že hello okruh v modelu nasazení classic hello plně funkční.</span><span class="sxs-lookup"><span data-stu-id="19571-112">Verify that hello circuit is fully operational in hello classic deployment model.</span></span>
* <span data-ttu-id="19571-113">Ujistěte se, že máte skupinu prostředků, který byl vytvořen v modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="19571-113">Ensure that you have a resource group that was created in hello Resource Manager deployment model.</span></span>

## <a name="move-an-expressroute-circuit"></a><span data-ttu-id="19571-114">Přesun okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="19571-114">Move an ExpressRoute circuit</span></span>

### <a name="step-1-gather-circuit-details-from-hello-classic-deployment-model"></a><span data-ttu-id="19571-115">Krok 1: Shromáždění podrobností okruh z modelu nasazení classic hello</span><span class="sxs-lookup"><span data-stu-id="19571-115">Step 1: Gather circuit details from hello classic deployment model</span></span>

<span data-ttu-id="19571-116">Přihlaste se toohello prostředí Azure classic a shromažďovat klíče služby hello.</span><span class="sxs-lookup"><span data-stu-id="19571-116">Sign in toohello Azure classic environment and gather hello service key.</span></span>

1. <span data-ttu-id="19571-117">Přihlaste se tooyour účet Azure.</span><span class="sxs-lookup"><span data-stu-id="19571-117">Sign in tooyour Azure account.</span></span>

  ```powershell
  Add-AzureAccount
  ```

2. <span data-ttu-id="19571-118">Vyberte příslušné předplatné Azure hello.</span><span class="sxs-lookup"><span data-stu-id="19571-118">Select hello appropriate Azure subscription.</span></span>

  ```powershell
  Select-AzureSubscription "<Enter Subscription Name here>"
  ```

3. <span data-ttu-id="19571-119">Importujte hello moduly Powershellu pro Azure a ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="19571-119">Import hello PowerShell modules for Azure and ExpressRoute.</span></span>

  ```powershell
  Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
  Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'
  ```

4. <span data-ttu-id="19571-120">Pomocí rutiny hello níže tooget hello služby klíče pro všechny vaše okruhy ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="19571-120">Use hello cmdlet below tooget hello service keys for all of your ExpressRoute circuits.</span></span> <span data-ttu-id="19571-121">Po načtení hello klíče, zkopírujte hello **klíč služby** hello okruhu, které chcete modelu nasazení Resource Manager toohello toomove.</span><span class="sxs-lookup"><span data-stu-id="19571-121">After retrieving hello keys, copy hello **service key** of hello circuit that you want toomove toohello Resource Manager deployment model.</span></span>

  ```powershell
  Get-AzureDedicatedCircuit
  ```

### <a name="step-2-sign-in-and-create-a-resource-group"></a><span data-ttu-id="19571-122">Krok 2: Přihlaste se a vytvořte skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="19571-122">Step 2: Sign in and create a resource group</span></span>

<span data-ttu-id="19571-123">Přihlaste se toohello Resource Manager prostředí a vytvořit novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="19571-123">Sign in toohello Resource Manager environment and create a new resource group.</span></span>

1. <span data-ttu-id="19571-124">Přihlaste se tooyour prostředí Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="19571-124">Sign in tooyour Azure Resource Manager environment.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

2. <span data-ttu-id="19571-125">Vyberte příslušné předplatné Azure hello.</span><span class="sxs-lookup"><span data-stu-id="19571-125">Select hello appropriate Azure subscription.</span></span>

  ```powershell
  Get-AzureRmSubscription -SubscriptionName "<Enter Subscription Name here>" | Select-AzureRmSubscription
  ```

3. <span data-ttu-id="19571-126">Upravte hello fragment kódu níže toocreate novou skupinu prostředků, pokud ještě nemáte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="19571-126">Modify hello snippet below toocreate a new resource group if you don't already have a resource group.</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name "DemoRG" -Location "West US"
  ```

### <a name="step-3-move-hello-expressroute-circuit-toohello-resource-manager-deployment-model"></a><span data-ttu-id="19571-127">Krok 3: Přesunutí okruhu ExpressRoute hello, toohello modelu nasazení Resource Manager</span><span class="sxs-lookup"><span data-stu-id="19571-127">Step 3: Move hello ExpressRoute circuit toohello Resource Manager deployment model</span></span>

<span data-ttu-id="19571-128">Můžete je nyní připraven toomove váš okruh ExpressRoute z modelu nasazení classic hello, toohello modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="19571-128">You are now ready toomove your ExpressRoute circuit from hello classic deployment model toohello Resource Manager deployment model.</span></span> <span data-ttu-id="19571-129">Než budete pokračovat, zkontrolujte hello informací uvedených v [přesun okruhu ExpressRoute z modelu nasazení Resource Manager classic toohello hello](expressroute-move.md).</span><span class="sxs-lookup"><span data-stu-id="19571-129">Before proceeding, review hello information provided in [Moving an ExpressRoute circuit from hello classic toohello Resource Manager deployment model](expressroute-move.md).</span></span>

<span data-ttu-id="19571-130">toomove váš okruh, upravit a spusťte hello následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="19571-130">toomove your circuit, modify and run hello following snippet:</span></span>

```powershell
Move-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "DemoRG" -Location "West US" -ServiceKey "<Service-key>"
```

> [!NOTE]
> <span data-ttu-id="19571-131">Po dokončení přesunu hello hello nový název, který je uvedený v předchozí rutiny hello bude použité tooaddress hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="19571-131">After hello move has finished, hello new name that is listed in hello previous cmdlet will be used tooaddress hello resource.</span></span> <span data-ttu-id="19571-132">Hello okruhu bude v podstatě přejmenovat.</span><span class="sxs-lookup"><span data-stu-id="19571-132">hello circuit will essentially be renamed.</span></span>
> 

## <a name="modify-circuit-access"></a><span data-ttu-id="19571-133">Úprava okruhu přístup</span><span class="sxs-lookup"><span data-stu-id="19571-133">Modify circuit access</span></span>

### <a name="tooenable-expressroute-circuit-access-for-both-deployment-models"></a><span data-ttu-id="19571-134">tooenable přístup okruhu ExpressRoute pro oba modely nasazení</span><span class="sxs-lookup"><span data-stu-id="19571-134">tooenable ExpressRoute circuit access for both deployment models</span></span>

<span data-ttu-id="19571-135">Po přesunutí classic Resource Manager nasazení modelu toohello okruh ExpressRoute, můžete povolit modely nasazení tooboth přístup.</span><span class="sxs-lookup"><span data-stu-id="19571-135">After moving your classic ExpressRoute circuit toohello Resource Manager deployment model, you can enable access tooboth deployment models.</span></span> <span data-ttu-id="19571-136">Spusťte následující rutiny tooenable přístup tooboth nasazení modely hello:</span><span class="sxs-lookup"><span data-stu-id="19571-136">Run hello following cmdlets tooenable access tooboth deployment models:</span></span>

1. <span data-ttu-id="19571-137">Získáte podrobnosti o okruhu hello.</span><span class="sxs-lookup"><span data-stu-id="19571-137">Get hello circuit details.</span></span>

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"
  ```

2. <span data-ttu-id="19571-138">Nastavit tooTRUE "Povolit klasické operace".</span><span class="sxs-lookup"><span data-stu-id="19571-138">Set "Allow Classic Operations" tooTRUE.</span></span>

  ```powershell
  $ckt.AllowClassicOperations = $true
  ```

3. <span data-ttu-id="19571-139">Aktualizujte hello okruh.</span><span class="sxs-lookup"><span data-stu-id="19571-139">Update hello circuit.</span></span> <span data-ttu-id="19571-140">Po úspěšném dokončení této operace bude možné tooview hello okruh v modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="19571-140">After this operation has finished successfully, you will be able tooview hello circuit in hello classic deployment model.</span></span>

  ```powershell
  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

4. <span data-ttu-id="19571-141">Spusťte následující rutinu tooget hello podrobnosti o hello okruh ExpressRoute hello.</span><span class="sxs-lookup"><span data-stu-id="19571-141">Run hello following cmdlet tooget hello details of hello ExpressRoute circuit.</span></span> <span data-ttu-id="19571-142">Musí být schopný toosee hello služby klíč uvedené.</span><span class="sxs-lookup"><span data-stu-id="19571-142">You must be able toosee hello service key listed.</span></span>

  ```powershell
  get-azurededicatedcircuit
  ```

5. <span data-ttu-id="19571-143">Teď můžete spravovat okruh ExpressRoute toohello odkazů pomocí příkazů modelu nasazení classic hello virtuální sítě classic a Resource Manager příkazy hello pro virtuální sítě Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="19571-143">You can now manage links toohello ExpressRoute circuit using hello classic deployment model commands for classic VNets, and hello Resource Manager commands for Resource Manager VNets.</span></span> <span data-ttu-id="19571-144">Hello následující články vám pomůžou spravovat odkazy toohello okruh ExpressRoute:</span><span class="sxs-lookup"><span data-stu-id="19571-144">hello following articles help you manage links toohello ExpressRoute circuit:</span></span>

    * [<span data-ttu-id="19571-145">Propojení vaší virtuální sítě tooyour okruh ExpressRoute v modelu nasazení Resource Manager hello</span><span class="sxs-lookup"><span data-stu-id="19571-145">Link your virtual network tooyour ExpressRoute circuit in hello Resource Manager deployment model</span></span>](expressroute-howto-linkvnet-arm.md)
    * [<span data-ttu-id="19571-146">Propojení vaší virtuální sítě tooyour okruh ExpressRoute v modelu nasazení classic hello</span><span class="sxs-lookup"><span data-stu-id="19571-146">Link your virtual network tooyour ExpressRoute circuit in hello classic deployment model</span></span>](expressroute-howto-linkvnet-classic.md)

### <a name="toodisable-expressroute-circuit-access-toohello-classic-deployment-model"></a><span data-ttu-id="19571-147">okruh ExpressRoute toodisable, přístup k modelu nasazení classic toohello</span><span class="sxs-lookup"><span data-stu-id="19571-147">toodisable ExpressRoute circuit access toohello classic deployment model</span></span>

<span data-ttu-id="19571-148">Spusťte následující rutiny toodisable přístup k modelu nasazení classic pro toohello hello.</span><span class="sxs-lookup"><span data-stu-id="19571-148">Run hello following cmdlets toodisable access toohello classic deployment model.</span></span>

1. <span data-ttu-id="19571-149">Získáte podrobnosti o hello okruh ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="19571-149">Get details of hello ExpressRoute circuit.</span></span>

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"
  ```

2. <span data-ttu-id="19571-150">Nastavit tooFALSE "Povolit klasické operace".</span><span class="sxs-lookup"><span data-stu-id="19571-150">Set "Allow Classic Operations" tooFALSE.</span></span>

  ```powershell
  $ckt.AllowClassicOperations = $false
  ```

3. <span data-ttu-id="19571-151">Aktualizujte hello okruh.</span><span class="sxs-lookup"><span data-stu-id="19571-151">Update hello circuit.</span></span> <span data-ttu-id="19571-152">Po úspěšném dokončení této operace nebudou moct tooview hello okruh v modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="19571-152">After this operation has finished successfully, you will not be able tooview hello circuit in hello classic deployment model.</span></span>

  ```powershell
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

## <a name="next-steps"></a><span data-ttu-id="19571-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="19571-153">Next steps</span></span>

* [<span data-ttu-id="19571-154">Vytvoření a úprava směrování pro okruhu ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="19571-154">Create and modify routing for your ExpressRoute circuit</span></span>](expressroute-howto-routing-arm.md)
* [<span data-ttu-id="19571-155">Propojení vaší virtuální sítě tooyour okruh ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="19571-155">Link your virtual network tooyour ExpressRoute circuit</span></span>](expressroute-howto-linkvnet-arm.md)
