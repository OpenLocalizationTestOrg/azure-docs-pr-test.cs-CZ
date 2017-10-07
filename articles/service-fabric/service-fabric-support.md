---
title: "aaaLearn o možnostech podpory infrastruktury služby Azure | Microsoft Docs"
description: "Verze clusteru Azure Service Fabric podporována a propojuje toofile lístky žádostí o podporu"
services: service-fabric
documentationcenter: .net
author: pkc
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/15/2017
ms.author: pkc
ms.openlocfilehash: 42394e2cd9dad2040d37d3a2ff3600ee040d8720
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-fabric-support-options"></a>Možnosti podpory Azure Service Fabric

načte toodeliver hello odpovídající podpora pro clusterů Service Fabric spuštěný aplikace práci na, nastavíme různé možnosti pro vás. V závislosti na hello úroveň podpory potřebné a závažnost hello hello problém, získáte toopick správné možnosti hello. 


<a id="getlivesitesupportonazure"></a>

## <a name="report-production-or-live-site-issues-or-request-paid-support-for-azure"></a>Sestavy web live problémy nebo produkční nebo požádejte placené podporu pro Azure.

Pro vytváření sestav za provozu lokality problémy v clusteru Service Fabric nasazené v Azure, otevřete lístek pro odborníky v oblasti podpory [na portálu Azure](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) nebo [portál podpory Microsoft](http://support.microsoft.com/oas/default.aspx?prid=16146).

Další informace o:
 
- [Profesionální podporu společnosti Microsoft pro Azure](https://azure.microsoft.com/en-us/support/plans/?b=16.44).
- [Microsoft premier support](https://support.microsoft.com/en-us/premier).

<a id="getlivesitesupportonprem"></a>

## <a name="report-production-or-live-site-issues-or-request-paid-support-for-standalone-service-fabric-clusters"></a>Sestavy web live problémy nebo produkční nebo požádejte podporu placené pro samostatnou službu, kterou clusterů Service Fabric

Pro hlášení problémů za provozu lokality v clusteru Service Fabric nasadit místně nebo na ostatních cloudů, otevřete lístek pro odborníky v oblasti podpory na [portál podpory Microsoft](http://support.microsoft.com/oas/default.aspx?prid=16146).

Další informace o:

- [Profesionální podporu společnosti Microsoft pro místní](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).
- [Microsoft premier support](https://support.microsoft.com/en-us/premier).


<a id="getsupportonissues"></a>
## <a name="report-azure-service-fabric-issues"></a>Problémy sestavy Azure Service Fabric 
Nastavili jsme úložiště GitHub pro hlášení problémů Service Fabric.  Také aktivně monitorujete hello následující fóra.

### <a name="github-repo"></a>Úložiště GitHub 
Hlášení problémů prostředků infrastruktury služby Azure na [problémů služby prostředků infrastruktury úložiště git](https://github.com/Azure/service-fabric-issues). Toto úložiště je určený pro generování sestav a sledování problémů s Azure Service Fabric a pro provedení žádosti o malé funkce. **Nepoužívejte tento tooreport live-site problémy**.

### <a name="stackoverflow-and-msdn-forums"></a>Fóra StackOverflow a MSDN

Hello [značky Service Fabric na StackOverflow] [ stackoverflow] a hello [Service Fabric fórum na webu MSDN] [ msdn-forum] jsou nejlepší pro klást otázky o Jak funguje hello platformy a jak může provést určité úlohy s ním.

### <a name="azure-feedback-forum"></a>Azure fóru pro zpětnou vazbu

Hello [Azure fóru pro zpětnou vazbu pro Service Fabric] [ uservoice-forum] je hello nejlepší místo pro odesílání big funkce nápady máte hello produktu, jako jsme zkontrolujte hello nejoblíbenější požadavky jsou součástí naší střední toolong termín plánování. Doporučujeme vám toorally podporu pro vaše návrhy v rámci hello komunity.


<a id="releasesuport"></a>
## <a name="supported-service-fabric-versions"></a>Podporované verze Service Fabric.

Ujistěte se, že cluster bude vždy spuštěn na podporovanou verzi Service Fabric. Jak a kdy jsme oznamujeme hello vydání nové verze aplikace Service Fabric, předchozí verze hello je označen pro ukončení podpory po 60 dnů od tohoto dne. Hello nové verze není zmínka [na blogu týmu Service Fabric hello](https://blogs.msdn.microsoft.com/azureservicefabric/).

Odkazovat toohello následující dokumenty na podrobnosti o tom, tookeep clusteru spuštěna podporovaná verze Service Fabric.

- [Upgrade verze Service Fabric v clusteru služby Azure](service-fabric-cluster-upgrade.md)
- [Upgrade verze Service Fabric na samostatný server clusteru systému windows](service-fabric-cluster-upgrade-windows-server.md)
 
Tady jsou hello seznam podporovaných verzích hello Service Fabric a koncovým datem jejich podporu.

| **Cluster modulu runtime Service Fabric** | **Kompatibilní SDK / verze balíčku NuGet** | **Koncové datum podpory** |
| --- | --- | --- |
| Všechny clusteru too5.3.121 předchozí verze |Menší než nebo rovna tooversion 2.3 |20. ledna 2017 |
| 5.3.* |Menší než nebo rovna tooversion 2.3 |24. února 2017 |
| 5.4.* |Menší než nebo rovna tooversion 2.4 |Může 10,2017     |
| 5.5.* |Menší než nebo rovna tooversion 2.5 |Srpen 10,2017    |
| 5.6.* |Menší než nebo rovna tooversion 2.6 |Říjen 13,2017    |
| 5.7.* |Menší než nebo rovna tooversion 2.7 |Aktuální verze a proto žádné datum ukončení

<a id="previewversion"></a>
## <a name="service-fabric-preview-versions---unsupported-for-production-use"></a>Verze Preview prostředků infrastruktury služby - nepodporovaný pro použití v provozním prostředí.
Z tootime času vydáváme verze, které mají významných funkcí, které chceme zpětné vazby, které vydávají jako verze Preview. Tato verze preview lze používat pouze pro účely testování. Provozní cluster měli vždycky používat verzi podporován, stabilní, Service Fabric. Ve verzi preview vždycky začíná číslem hlavní verze a podverze 255. Například pokud se zobrazí Service Fabric verze 255.255.5703.949, tuto verzi je pouze toobe použít v testovacích clusterů a je ve verzi preview. Tato verze preview jsou také oznámeny na hello [blog týmu Service Fabric](https://blogs.msdn.microsoft.com/azureservicefabric) a bude mít podrobnosti na hello funkce.

Neexistuje žádná možnost placené podporu pro tyto verze preview. Použijte jednu z možností hello uvedené v části [sestavy Azure Service Fabric problémy](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-support#report-azure-service-fabric-issues) tooask otázky, nebo poskytnout zpětnou vazbu.

## <a name="next-steps"></a>Další kroky

- [Verze fabric upgradu služby v Azure clusteru](service-fabric-cluster-upgrade.md)
- [Upgrade verze Service Fabric na samostatný server clusteru systému windows](service-fabric-cluster-upgrade-windows-server.md)

<!--references-->
[msdn-forum]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureServiceFabric
[stackoverflow]: http://stackoverflow.com/questions/tagged/azure-service-fabric
[uservoice-forum]: https://feedback.azure.com/forums/293901-service-fabric
[acom-docs]: http://aka.ms/servicefabricdocs
[sample-repos]: http://aka.ms/servicefabricsamples
