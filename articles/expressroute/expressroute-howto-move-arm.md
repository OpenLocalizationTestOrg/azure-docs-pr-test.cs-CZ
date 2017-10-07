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
# <a name="move-expressroute-circuits-from-hello-classic-toohello-resource-manager-deployment-model-using-powershell"></a>Přesun okruhů ExpressRoute z hello toohello klasického modelu nasazení Resource Manager pomocí prostředí PowerShell

toouse okruhu ExpressRoute pro hello classic a modelech nasazení Resource Manager, musíte přesunout modelu nasazení Resource Manager toohello okruhu hello. Hello následující části vám pomohou přesunutí okruhu pomocí prostředí PowerShell.

## <a name="before-you-begin"></a>Než začnete

* Ověřte, že máte nejnovější verzi modulů prostředí Azure PowerShell hello hello (minimálně verze 1.0). Další informace najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).
* Ujistěte se, že jste si přečetli hello [požadavky](expressroute-prerequisites.md), [požadavky na směrování](expressroute-routing.md), a [pracovních](expressroute-workflows.md) před zahájením konfigurace.
* Zkontrolujte hello informace jsou poskytovány na základě [přesun okruhu ExpressRoute z classic tooResource Manager](expressroute-move.md). Ujistěte se, že rozumíte plně hello limity a omezení.
* Ověřte, že hello okruh v modelu nasazení classic hello plně funkční.
* Ujistěte se, že máte skupinu prostředků, který byl vytvořen v modelu nasazení Resource Manager hello.

## <a name="move-an-expressroute-circuit"></a>Přesun okruhu ExpressRoute

### <a name="step-1-gather-circuit-details-from-hello-classic-deployment-model"></a>Krok 1: Shromáždění podrobností okruh z modelu nasazení classic hello

Přihlaste se toohello prostředí Azure classic a shromažďovat klíče služby hello.

1. Přihlaste se tooyour účet Azure.

  ```powershell
  Add-AzureAccount
  ```

2. Vyberte příslušné předplatné Azure hello.

  ```powershell
  Select-AzureSubscription "<Enter Subscription Name here>"
  ```

3. Importujte hello moduly Powershellu pro Azure a ExpressRoute.

  ```powershell
  Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
  Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'
  ```

4. Pomocí rutiny hello níže tooget hello služby klíče pro všechny vaše okruhy ExpressRoute. Po načtení hello klíče, zkopírujte hello **klíč služby** hello okruhu, které chcete modelu nasazení Resource Manager toohello toomove.

  ```powershell
  Get-AzureDedicatedCircuit
  ```

### <a name="step-2-sign-in-and-create-a-resource-group"></a>Krok 2: Přihlaste se a vytvořte skupinu prostředků

Přihlaste se toohello Resource Manager prostředí a vytvořit novou skupinu prostředků.

1. Přihlaste se tooyour prostředí Azure Resource Manager.

  ```powershell
  Login-AzureRmAccount
  ```

2. Vyberte příslušné předplatné Azure hello.

  ```powershell
  Get-AzureRmSubscription -SubscriptionName "<Enter Subscription Name here>" | Select-AzureRmSubscription
  ```

3. Upravte hello fragment kódu níže toocreate novou skupinu prostředků, pokud ještě nemáte skupinu prostředků.

  ```powershell
  New-AzureRmResourceGroup -Name "DemoRG" -Location "West US"
  ```

### <a name="step-3-move-hello-expressroute-circuit-toohello-resource-manager-deployment-model"></a>Krok 3: Přesunutí okruhu ExpressRoute hello, toohello modelu nasazení Resource Manager

Můžete je nyní připraven toomove váš okruh ExpressRoute z modelu nasazení classic hello, toohello modelu nasazení Resource Manager. Než budete pokračovat, zkontrolujte hello informací uvedených v [přesun okruhu ExpressRoute z modelu nasazení Resource Manager classic toohello hello](expressroute-move.md).

toomove váš okruh, upravit a spusťte hello následující fragment kódu:

```powershell
Move-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "DemoRG" -Location "West US" -ServiceKey "<Service-key>"
```

> [!NOTE]
> Po dokončení přesunu hello hello nový název, který je uvedený v předchozí rutiny hello bude použité tooaddress hello prostředků. Hello okruhu bude v podstatě přejmenovat.
> 

## <a name="modify-circuit-access"></a>Úprava okruhu přístup

### <a name="tooenable-expressroute-circuit-access-for-both-deployment-models"></a>tooenable přístup okruhu ExpressRoute pro oba modely nasazení

Po přesunutí classic Resource Manager nasazení modelu toohello okruh ExpressRoute, můžete povolit modely nasazení tooboth přístup. Spusťte následující rutiny tooenable přístup tooboth nasazení modely hello:

1. Získáte podrobnosti o okruhu hello.

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"
  ```

2. Nastavit tooTRUE "Povolit klasické operace".

  ```powershell
  $ckt.AllowClassicOperations = $true
  ```

3. Aktualizujte hello okruh. Po úspěšném dokončení této operace bude možné tooview hello okruh v modelu nasazení classic hello.

  ```powershell
  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

4. Spusťte následující rutinu tooget hello podrobnosti o hello okruh ExpressRoute hello. Musí být schopný toosee hello služby klíč uvedené.

  ```powershell
  get-azurededicatedcircuit
  ```

5. Teď můžete spravovat okruh ExpressRoute toohello odkazů pomocí příkazů modelu nasazení classic hello virtuální sítě classic a Resource Manager příkazy hello pro virtuální sítě Resource Manager. Hello následující články vám pomůžou spravovat odkazy toohello okruh ExpressRoute:

    * [Propojení vaší virtuální sítě tooyour okruh ExpressRoute v modelu nasazení Resource Manager hello](expressroute-howto-linkvnet-arm.md)
    * [Propojení vaší virtuální sítě tooyour okruh ExpressRoute v modelu nasazení classic hello](expressroute-howto-linkvnet-classic.md)

### <a name="toodisable-expressroute-circuit-access-toohello-classic-deployment-model"></a>okruh ExpressRoute toodisable, přístup k modelu nasazení classic toohello

Spusťte následující rutiny toodisable přístup k modelu nasazení classic pro toohello hello.

1. Získáte podrobnosti o hello okruh ExpressRoute.

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"
  ```

2. Nastavit tooFALSE "Povolit klasické operace".

  ```powershell
  $ckt.AllowClassicOperations = $false
  ```

3. Aktualizujte hello okruh. Po úspěšném dokončení této operace nebudou moct tooview hello okruh v modelu nasazení classic hello.

  ```powershell
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

## <a name="next-steps"></a>Další kroky

* [Vytvoření a úprava směrování pro okruhu ExpressRoute](expressroute-howto-routing-arm.md)
* [Propojení vaší virtuální sítě tooyour okruh ExpressRoute](expressroute-howto-linkvnet-arm.md)
