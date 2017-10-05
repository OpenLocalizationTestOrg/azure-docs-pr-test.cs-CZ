---
title: "Nasazení QuickBooks v Azure Remoteappu | Microsoft Docs"
description: "Naučte se sdílet QuickBooks s Azure Remoteappem."
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
ms.assetid: c5d00753-77c0-4f0d-a5df-9451b46a31d3
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: bbfac45220f6ef36c9951daae0ced1974e8ddabb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-do-you-deploy-quickbooks-in-azure-remoteapp"></a>Jak se nasadit QuickBooks v Azure Remoteappu?
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Použijte tyto informace sdílet QuickBooks jako aplikace v Azure Remoteappu.

QuickBooks 2015 Enterprise můžete sdílet s Azure Remoteappem v kolekci hybridní nebo cloud. Soubor společnosti se musí nacházet na virtuální počítač, který je spuštěn server QuickBooks databáze, která je oddělená od serverů Azure RemoteApp. Nikdy uložit soubor společnosti na image Azure Remoteappu – ztráta dat je očekáváno, pokud to uděláte. Pouze QuickBooks Enterprise podporuje hostování soubor QuickBooks externí sdílené složky s QuickBooks databázový server přístupné přes standardní sítě systému Windows.   

> [!IMPORTANT]
> QuickBooks databázový server, který je hostitelem soubor společnosti se musí nacházet na samostatný virtuální počítač v rámci stejné virtuální síti jako kolekci Azure Remoteappu.  
> 
> 

## <a name="steps-to-deploy-quickbooks"></a>Postup nasazení QuickBooks
1. Vytvořit virtuální počítač Azure a nainstalujte QuickBooks QuickBooks databázový server a umístit soubor společnosti na virtuálním počítači Azure.  Ujistěte se, že jste správně nakonfigurovat pravidla brány firewall.
2. Nainstalujte QuickBooks [vlastní image](remoteapp-imageoptions.md) a vytvoření [kolekci Azure Remoteappu](remoteapp-collections.md), cloud nebo hybridní v rámci přesně stejnou virtuální síť, kde je umístěn virtuální počítač hostuje QuickBooks databázový server s firemním souborům. 
3. [Publikování](remoteapp-publish.md) QuickBooks aplikaci pro uživatele
4. Spusťte klienta QuickBooks hostovaných v Azure Remoteappu, přejděte pomocí standardní sítě systému Windows na virtuální počítač, který je hostitelem databáze serveru QuickBooks a otevřete soubor společnosti. 

## <a name="documentation-references"></a>Odkazy na dokumentaci
* QuickBooks [podporované konfigurace](http://enterprisesuite.intuit.com/products/enterprise-solutions/technical/#top)
* QuickBooks [možnosti nasazení](http://enterprisesuite.intuit.com/everythingenterprise/launchpad/new-user/)

Můžete se taky podívat na prezentace Ignite [základy pro Microsoft Azure RemoteApp správu a správu](https://channel9.msdn.com/Events/Ignite/2015/BRK3868) -rychlé převíjení vpřed na 1:02:45 získat QuickBooks část.

## <a name="deployment-architecture"></a>Architektura nasazení služby
![QuickBooks + nasazení Azure RemoteApp](./media/remoteapp-quickbooks/ra-quickbooks.png)

