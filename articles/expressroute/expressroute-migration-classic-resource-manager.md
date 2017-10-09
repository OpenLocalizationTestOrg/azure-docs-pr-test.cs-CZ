---
title: "Migrace z classic tooResource Manager ExpressRoute přidružené virtuální sítě: Azure: prostředí PowerShell | Microsoft Docs"
description: "Tato stránka popisuje, jak toomigrate přidružené virtuální sítě tooResource Manager po přesunutí okruhu."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/06/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: e64506c6909296f98c5dd23b1437bc0b81f31c75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-expressroute-associated-virtual-networks-from-classic-tooresource-manager"></a>Migrace z classic tooResource Manager ExpressRoute přidružené virtuální sítě

Tento článek vysvětluje, jak toomigrate Azure ExpressRoute přidružené virtuální sítě z modelu nasazení classic hello, toohello modelu nasazení Azure Resource Manager po přesunutí okruhu ExpressRoute. 


## <a name="before-you-begin"></a>Než začnete
* Ověřte, zda máte nejnovější verzi modulů prostředí Azure PowerShell hello hello. Další informace najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).
* Ujistěte se, že jste si přečetli hello [požadavky](expressroute-prerequisites.md), [požadavky na směrování](expressroute-routing.md), a [pracovních](expressroute-workflows.md) před zahájením konfigurace.
* Zkontrolujte hello informace jsou poskytovány na základě [přesun okruhu ExpressRoute z classic tooResource Manager](expressroute-move.md). Ujistěte se, že rozumíte plně hello limity a omezení.
* Ověřte, že hello okruh v modelu nasazení classic hello plně funkční.
* Ujistěte se, že máte skupinu prostředků, který byl vytvořen v modelu nasazení Resource Manager hello.
* Projděte si následující dokumentace k migraci prostředků hello:

    * [Podporované platformy migrace prostředky infrastruktury z classic tooAzure Resource Manager](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
    * [Technické podrobné informace o platformy podporované migrace z klasického tooAzure Resource Manager](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
    * [Nejčastější dotazy: Migrace podporované platformy IaaS prostředků z classic tooAzure Resource Manager](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
    * [Zkontrolujte nejběžnějších chyb, migrace a jejich zmírnění](../virtual-machines/windows/migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="supported-and-unsupported-scenarios"></a>Podporované a nepodporované scénáře

* Z prostředí Resource Manager classic toohello hello bez odstávky můžete přesunout okruh ExpressRoute. Jakýkoli okruh ExpressRoute můžete přesunout z hello classic toohello Resource Manager prostředí bez výpadků. Postupujte podle pokynů hello v [přesun okruhů ExpressRoute z hello toohello klasického modelu nasazení Resource Manager pomocí prostředí PowerShell](expressroute-howto-move-arm.md). Toto je virtuální sítě připojený toohello požadovaných toomove prostředky.
* Virtuální sítě, bran a přidružené nasazení v rámci hello virtuální sítě, které jsou připojené tooan okruh ExpressRoute v hello, který může být stejné předplatné migrovat toohello Resource Manager prostředí bez odstávky. Můžete postupovat podle hello kroky popsané novější toomigrate prostředkům, například virtuální sítě, bran a virtuálních počítačů nasazených v rámci virtuální sítě hello. Je nutné zajistit, že hello virtuální sítě jsou správně nakonfigurovaná, před jejich migrací. 
* Virtuální sítě, bran a související nasazení v rámci hello virtuální sítě, které nejsou v hello stejného předplatného jako hello okruh ExpressRoute, vyžadují některé výpadek toocomplete hello migraci. poslední část Hello hello dokumentu popisuje prostředky toomigrate toobe postupovali podle kroků hello.
* Nelze migrovat virtuální síť s bránu ExpressRoute i bránu VPN.

## <a name="move-an-expressroute-circuit-from-classic-tooresource-manager"></a>Přesun okruhu ExpressRoute z classic tooResource Manager
Je třeba přesunout, že okruhu ExpressRoute z hello classic toohello prostředí správce prostředků, než se pokusíte toomigrate prostředky, které jsou připojené toohello okruh ExpressRoute. tooaccomplish to úloh, najdete v části hello následující články:

* Zkontrolujte hello informace jsou poskytovány na základě [přesun okruhu ExpressRoute z classic tooResource Manager](expressroute-move.md).
* [Přesun okruhu z classic tooResource Manager pomocí Azure PowerShell](expressroute-howto-move-arm.md).
* Pomocí portálu Azure Service Management hello. Můžete postupovat podle pracovního postupu hello příliš[vytvořit nové okruh ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md) a zvolte možnost importovat hello. 

Tato operace nezahrnuje výpadku. Tootransfer dat mezi místním a společnosti Microsoft můžete pokračovat, zatímco probíhá migrace hello.

## <a name="migrate-virtual-networks-gateways-and-associated-deployments"></a>Migrovat virtuální sítě, bran a související nasazení

Hello kroky podle toomigrate závisí na tom, jestli vaše prostředky jsou v hello stejného předplatného, různých předplatných nebo obojí.

### <a name="migrate-virtual-networks-gateways-and-associated-deployments-in-hello-same-subscription-as-hello-expressroute-circuit"></a>Migrujte virtuální sítě, brány, a související nasazení v hello stejného předplatného jako hello okruh ExpressRoute
Tato část popisuje toomigrate toobe postupovali podle kroků hello virtuální sítě, bránu a přidružené nasazení v hello stejného předplatného jako hello okruh ExpressRoute. Odstávka je přidružen k této migrace. Můžete dál toouse všechny prostředky procesem migrace hello. Hello správu roviny uzamčeno, když probíhá migrace hello. 

1. Ujistěte se, že okruh ExpressRoute hello přesunula z prostředí Resource Manager classic toohello hello.
2. Zajistěte, aby že správně připraven hello virtuální síti pro migraci hello.
3. Registrace předplatného pro migraci prostředků. tooregister vaše předplatné pro migraci prostředků, použijte hello následující fragment kódu prostředí PowerShell:

  ```powershell 
  Select-AzureRmSubscription -SubscriptionName <Your Subscription Name>
  Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
  Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
  ```
4. Ověření, přípravě a migrovat. toomove hello virtuální síť, použijte hello následující fragment kódu prostředí PowerShell:

  ```powershell
  Move-AzureVirtualNetwork -Prepare $vnetName  
  Move-AzureVirtualNetwork -Commit $vnetName
  ```

  Migrace může přerušit také spuštěním následující rutiny prostředí PowerShell hello:

  ```powershell
  Move-AzureVirtualNetwork -Abort $vnetName
  ```

## <a name="next-steps"></a>Další kroky
* [Podporované platformy migrace prostředky infrastruktury z classic tooAzure Resource Manager](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
* [Technické podrobné informace o platformy podporované migrace z klasického tooAzure Resource Manager](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
* [Nejčastější dotazy: Migrace podporované platformy IaaS prostředků z classic tooAzure Resource Manager](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
* [Zkontrolujte nejběžnějších chyb, migrace a jejich zmírnění](../virtual-machines/windows/migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
