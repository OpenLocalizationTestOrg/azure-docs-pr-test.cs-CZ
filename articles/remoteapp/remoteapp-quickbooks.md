---
title: aaaDeploy QuickBooks v Azure Remoteappu | Microsoft Docs
description: "Zjistěte, jak tooshare QuickBooks s Azure Remoteappem."
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
ms.openlocfilehash: c21b1ac311449be2281f9ce157828260e856f55d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-do-you-deploy-quickbooks-in-azure-remoteapp"></a>Jak se nasadit QuickBooks v Azure Remoteappu?
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Použijte následující informace tooshare QuickBooks jako aplikace v Azure Remoteappu hello.

QuickBooks 2015 Enterprise můžete sdílet s Azure Remoteappem v kolekci hybridní nebo cloud. soubor společnosti Hello se musí nacházet na virtuální počítač, který je spuštěn server QuickBooks databáze, která je oddělená od serverů Azure RemoteApp hello. Nikdy neukládají soubor společnosti hello na image Azure Remoteappu – ztráta dat je očekáváno, pokud to uděláte. Pouze QuickBooks Enterprise podporuje hostování souborů QuickBooks hello externí sdílené složky s QuickBooks databázový server přístupné přes standardní sítě systému Windows.   

> [!IMPORTANT]
> Hello QuickBooks databázový server, který je hostitelem soubor společnosti hello se musí nacházet na samostatný virtuální počítač v rámci hello stejné virtuální síti jako hello kolekci Azure Remoteappu.  
> 
> 

## <a name="steps-toodeploy-quickbooks"></a>Kroky toodeploy QuickBooks
1. Vytvořit virtuální počítač Azure a nainstalujte QuickBooks QuickBooks databázový server a umístit soubor společnosti hello na virtuálním počítači Azure.  Ujistěte se, že tooproperly konfigurace pravidel brány firewall.
2. Nainstalujte QuickBooks [vlastní image](remoteapp-imageoptions.md) a vytvoření [kolekci Azure Remoteappu](remoteapp-collections.md), cloud nebo hybridní v rámci hello přesnou stejnou virtuální síť, kde hello virtuální počítač hostitelem hello QuickBooks databázový server s soubory společnosti nachází. 
3. [Publikování](remoteapp-publish.md) toousers QuickBooks aplikace
4. Spusťte hello hostovaných v Azure Remoteappu QuickBooks klienta, přejděte pomocí standardní síťový toohello virtuálních počítačů hostování hello QuickBooks databázový server Windows a otevřete soubor společnosti hello. 

## <a name="documentation-references"></a>Odkazy na dokumentaci
* QuickBooks [podporované konfigurace](http://enterprisesuite.intuit.com/products/enterprise-solutions/technical/#top)
* QuickBooks [možnosti nasazení](http://enterprisesuite.intuit.com/everythingenterprise/launchpad/new-user/)

Můžete se taky podívat na prezentace Ignite [základy pro Microsoft Azure RemoteApp správu a správu](https://channel9.msdn.com/Events/Ignite/2015/BRK3868) -převinutí vpřed too1:02:45 tooget toohello QuickBooks část.

## <a name="deployment-architecture"></a>Architektura nasazení služby
![QuickBooks + nasazení Azure RemoteApp](./media/remoteapp-quickbooks/ra-quickbooks.png)

