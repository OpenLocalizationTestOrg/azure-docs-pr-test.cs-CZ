---
title: "aaaDifferences mezi cloudové služby a Service Fabric | Microsoft Docs"
description: "Koncepční přehled migrace aplikací z cloudové služby tooService prostředků infrastruktury."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 0b87b1d3-88ad-4658-a465-9f05a3376dee
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: bbc5ef4fe0fe1b0da55454cb6b766925030198fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="learn-about-hello-differences-between-cloud-services-and-service-fabric-before-migrating-applications"></a>Další informace o hello rozdíly mezi cloudové služby a Service Fabric před migrací aplikace.
Microsoft Azure Service Fabric je hello generace cloudové aplikace platforma pro vysoce škálovatelnou a vysoce spolehlivé distribuované aplikace. Přináší mnoho nových funkcí pro balení, nasazování, upgrade a správu distribuovaných cloudové aplikace. 

Jedná se úvodní příručka toomigrating aplikací z cloudové služby tooService prostředků infrastruktury. Ho se zaměřují hlavně na architektury a návrhu rozdíly mezi cloudové služby a Service Fabric.

## <a name="applications-and-infrastructure"></a>Aplikace a infrastrukturu
Základní rozdíl mezi cloudové služby a Service Fabric se hello vztah mezi virtuálními počítači, úlohy a aplikace. Zde zatížení je definován jako kód hello zápisu tooperform konkrétní úlohu nebo poskytovat služby.

* **Cloudové služby je o nasazení aplikací jako virtuální počítače.** Hello kód napsaný je úzce párované tooa instance virtuálního počítače, jako je Web nebo Role pracovního procesu. toodeploy úloh ve cloudové služby je toodeploy jeden nebo více virtuálních počítačů instance této úlohy spuštění hello. Neexistuje žádné oddělení aplikací a virtuální počítače, a proto neexistuje formální definice aplikace. Aplikace můžete představit jako sadu instancí webu nebo Role pracovního procesu v rámci nasazení cloudové služby nebo celé nasazení cloudové služby. V tomto příkladu se aplikace zobrazí jako sada instancí role.

![Cloudové služby aplikace a topologie][1]

* **Service Fabric je o nasazení aplikace tooexisting virtuální počítače nebo počítačů Service Fabric systémem Windows nebo Linux.** Hello služby, které můžete psát jsou zcela odpojeného z hello základní infrastrukturu, která je abstrahované rychle platformu aplikací Service Fabric hello, takže aplikace může být nasazený toomultiple prostředí. Úlohy v Service Fabric se nazývá "služba" a jednu nebo více služeb jsou seskupené v oficiálně definované aplikací, který běží na platformu aplikací Service Fabric hello. Více aplikací může být nasazený tooa jeden cluster Service Fabric.

![Aplikace Service Fabric a topologie][2]

Service Fabric, samotné je aplikační platforma vrstvu, která běží na systému Windows nebo Linux, zatímco cloudové služby je systém pro nasazení virtuálních počítačů Azure spravovat pomocí úlohy připojený.
model aplikace Service Fabric Hello přináší řadu výhod:

* Rychlé nasazení časy. Vytváření instancí virtuálních počítačů může být časově náročná. V Service Fabric jsou virtuální počítače nasazené pouze po tooform cluster, který je hostitelem hello platformu aplikací Service Fabric. Od této chvíle balíčky aplikací lze nasazené toohello clusteru velmi rychle.
* Hostování s vysokou hustotou. Virtuální počítač Role pracovního procesu v cloudové služby, hostuje jednu úlohu. V Service Fabric jsou oddělené od hello virtuálních počítačů, které spustit, což znamená, že můžete nasadit velký počet aplikací tooa malý počet virtuálních počítačů, které můžete snížit celkové náklady pro rozsáhlejší nasazení aplikace.
* Hello Service Fabric platformy můžete spustit odkudkoli, má Windows Server nebo Linux počítače, jestli je Azure nebo místní. Hello platformy poskytuje abstraktní vrstvu nad základní infrastrukturou hello, takže lze aplikaci spustit v různých prostředích. 
* Správa distribuovaných aplikací. Service Fabric je platforma, že pouze hostitelé distribuované aplikace, ale také pomáhá spravovat životní cyklus nezávisle na hello hostování virtuálních počítačů nebo počítač životního cyklu.

## <a name="application-architecture"></a>Architektura aplikace
Hello Architektura aplikace cloudové služby obvykle obsahuje mnoho závislostí externí služby, jako Service Bus, Azure Table a úložiště objektů Blob, SQL, Redis a dalších toomanage hello stav a data aplikace a komunikace mezi Web a rolí pracovního procesu v nasazení cloudové služby. Příkladem dokončení aplikace cloudové služby může vypadat například takto:  

![Architektura cloudových služeb][9]

Aplikace Service Fabric můžete také zvolit toouse hello stejné externích služeb v hotové aplikace. V tomto příkladu architektura cloudové služby, hello nejjednodušší cesta migrace z cloudové služby tooService prostředků infrastruktury je tooreplace pouze hello cloudové služby nasazení s aplikací Service Fabric, udržování hello přehled architektury hello stejné. Hello webové a pracovní role lze přenést tooService Fabric bezstavové služby s minimálními změnami kódu.

![Architektura Service Fabric po jednoduché migrace][10]

V této fázi, by měly pokračovat ve hello systému toowork hello stejná jako předtím. Pořízení využít stavové funkcí Service Fabric, externí stav úložiště můžete internalized jako stavové služby, kde je to možné. Toto je složitější než jednoduchá migrace webových a rolí pracovního procesu tooService Fabric bezstavové služby, protože vyžaduje zápis vlastní služby, které poskytují ekvivalentní funkce tooyour aplikace jako předtím hello externích služeb. Díky tomu Hello výhody patří: 

* Odebrání externí závislosti 
* Sjednotit hello nasazení, správu a modely upgradu. 

Příklad výsledné architekturu internalizing těchto služeb může vypadat například takto:

![Architektura Service Fabric po plné migraci][11]

## <a name="communication-and-workflow"></a>Komunikace a pracovní postup
Většina aplikací Cloudová služba se skládá z více než jednu úroveň. Podobně aplikace Service Fabric se skládá z více než jedna služba (obvykle mnoho služby). Dvě běžné komunikace modely jsou přímou komunikaci a prostřednictvím externí odolné úložiště.

### <a name="direct-communication"></a>Přímé komunikaci
Pomocí přímou komunikaci vrstev komunikovat přímo prostřednictvím koncového bodu vystavené každou úroveň. V bezstavové prostředích, jako je cloudové služby, tato znamená výběr instanci role virtuálního počítače, buď náhodně nebo zatížení pomocí kruhového dotazování toobalance a připojování koncového bodu tooits přímo.

![Cloudové služby přímé komunikaci][5]

 Přímé komunikaci je běžný komunikační model v Service Fabric. Hello klíče rozdíl mezi Service Fabric a Cloud Services je, že v cloudové služby připojíte tooa virtuálních počítačů, zatímco v Service Fabric připojit tooa služby. Jde o důležité odlišení několik důvodů:

* Služby v Service Fabric nejsou vázané toohello virtuálních počítačů, které jsou hostiteli je; služby mohou pohyb v clusteru hello a ve skutečnosti jsou očekávané toomove kolem z různých důvodů: prostředek vyrovnávání, převzetí služeb při selhání, aplikace a infrastrukturu upgrady a omezení umístění nebo zatížení. To znamená, že můžete kdykoli změnit adresu instance služby. 
* Virtuální počítač v Service Fabric může být hostitelem více služeb, každý s koncovými body jedinečný.

Service Fabric nabízí mechanismus zjišťování služby názvem hello pojmenování služby, který lze použít tooresolve adresy koncových bodů služby. 

![Přímé komunikaci Service Fabric][6]

### <a name="queues"></a>Fronty
Běžné mechanismus komunikace mezi vrstvami v bezstavové prostředích, jako je cloudové služby je toouse externí úložiště fronty toodurably uložit pracovní úkoly z jedné vrstvy tooanother. Obvyklým scénářem je webová vrstva, která odešle tooan úlohy Azure Queue nebo Service Bus, kde můžete instancí Role pracovního procesu dequeue – a zpracování úlohy hello.

![Cloud Services fronty komunikace][7]

Hello stejný model komunikace lze použít v Service Fabric. To může být užitečné při migraci stávající tooService aplikace cloudové služby prostředků infrastruktury. 

![Přímé komunikaci Service Fabric][8]

## <a name="next-steps"></a>Další kroky
Hello nejjednodušší cesta migrace z cloudové služby tooService, prostředků infrastruktury je tooreplace pouze hello nasazení cloudové služby s aplikace Service Fabric, udržování hello přehled architektury aplikace zhruba hello stejné. Hello následující článek poskytuje návod, jak převést toohelp webu nebo Role pracovního procesu tooa bezstavové služby Service Fabric.

* [Jednoduchá migrace: převést webu nebo Role pracovního procesu tooa bezstavové služby Service Fabric](service-fabric-cloud-services-migration-worker-role-stateless-service.md)

<!--Image references-->
[1]: ./media/service-fabric-cloud-services-migration-differences/topology-cloud-services.png
[2]: ./media/service-fabric-cloud-services-migration-differences/topology-service-fabric.png
[5]: ./media/service-fabric-cloud-services-migration-differences/cloud-service-communication-direct.png
[6]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-communication-direct.png
[7]: ./media/service-fabric-cloud-services-migration-differences/cloud-service-communication-queues.png
[8]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-communication-queues.png
[9]: ./media/service-fabric-cloud-services-migration-differences/cloud-services-architecture.png
[10]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-architecture-simple.png
[11]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-architecture-full.png
