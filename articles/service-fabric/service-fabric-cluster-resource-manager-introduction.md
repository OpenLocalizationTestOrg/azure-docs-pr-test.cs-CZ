---
title: "aaaIntroducing hello správce prostředků clusteru Service Fabric | Microsoft Docs"
description: "Toohello Úvod správce prostředků clusteru Service Fabric."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: cfab735b-923d-4246-a2a8-220d4f4e0c64
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: e815925880e2f3a755294de1dcfb9b88fbdde08a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introducing-hello-service-fabric-cluster-resource-manager"></a>Představení správce prostředků clusteru Service Fabric hello
Tradičně Správa systémy IT nebo online služby určené vyhradit konkrétní fyzické nebo virtuální počítače toothose konkrétní služby nebo systémy. Služby byly navržen jako vrstev. By "web" vrstvu a vrstvu "data" nebo "úložiště". Aplikace by měla mít zasílání zpráv úrovně, kde plynoucích požadavky a odhlašování, a také sadu vyhrazené toocaching počítače. Jednotlivé typy úlohy nebo vrstvě měl vyhrazené tooit konkrétních počítačů: databáze hello několik získali několik, počítače vyhrazené tooit, hello webové servery. Pokud konkrétní typ zatížení způsobeno hello počítačů, které bylo na toorun příliš aktivní, jste přidali další počítače pomocí této stejné toothat vrstvě konfigurace. Ale ne všechny úlohy může tak snadno škálovat – zejména s hello datové vrstvy obvykle nahradíte počítače, které mají větší počítače. Snadné. Pokud na počítači se nezdařilo, spustili část celkového aplikace hello na nižší kapacitu, dokud hello počítač může být obnovit. Stále poměrně snadno (Pokud je to nutně zábavné).

Teď ale hello world služby a architektura softwaru se změnila. Je dnes běžné, že aplikace přijaly návrh Škálováním na více systémů. Vytváření aplikací s kontejnery nebo mikroslužeb (nebo obě) je běžné. Teď když stále může mít pouze několik počítačů, neběží jenom jednu instanci zatížení. Se může i běžet několik různých úloh v hello stejnou dobu. Nyní máte desítek různých typů služeb (none využívání úplné počítač za prostředky), případně stovky různých instancí těchto služeb. Každé pojmenované instance má jeden nebo více instancí nebo replik pro vysokou dostupnost (HA). V závislosti na velikosti hello těchto zatížení, a jak jsou zaneprázdněny může najít sami se stovkami nebo tisíci počítačů. 

Náhle Správa prostředí není tak jednoduché, správu několika typů počítače vyhrazené toosingle úlohy. Vaše servery jsou virtuální a už nebude mít názvy (Přepnuli jste mindsets z [mazlíčků toocattle](http://www.slideshare.net/randybias/architectures-for-open-and-scalable-clouds/20) po všech). Konfigurace je menší o hello počítače a informace o službách hello, sami. Hardware, který je vyhrazený tooa jednu instanci zatížení je do značné míry co posledních hello. Služby, sami staly malé distribuovaných systémů, které jsou rozmístěny v několika menší části komoditním hardwaru.

Vzhledem k tomu, že vaše aplikace není nadále řadu monoliths rozloženy několik vrstev, nyní máte mnoho další toodeal kombinace s. Kdo rozhodne, co můžete spustit typy úloh, na které hardwaru nebo kolik? Jaké úlohy dobré fungování i na hello stejný hardware a který konfliktu? Kdy je počítač přestane dolů jak víte, co byla spuštěna existuje na tomto počítači? Kdo má na starosti a ujistěte se, že úlohy spustí znovu spustit? Počkat na hello (virtuální)? počítač toocome zpět nebo úlohy automaticky převzetí služeb při selhání tooother počítače a zachovat systémem? Je potřeba lidského zásahu? Co upgrady pro toto prostředí?

Jako vývojáři a operátory týkajících se v tomto prostředí vytvoříme toowant nápovědy Správa této složitost. A pronájem binge a zkusit toohide hello složitost s uživateli, je pravděpodobně není pravé odpovědi hello, takže co můžeme udělat?

## <a name="introducing-orchestrators"></a>Představení orchestrators
"Orchestrator" je hello obecný termín pro softwarového produktu, který pomáhá správcům spravovat tyto typy prostředí. Orchestrators jsou hello součásti, které provést v žádostech o jako "Nastavit jako pět kopií této služby v mé prostředí." Toomake hello shodu hello požadovaného stavu prostředí, bez ohledu na to, co se stane, snaží.

Orchestrators (ne člověka) jsou co provést akci při selhání na počítači nebo zatížení ukončí neočekávané z nějakého důvodu. Většina orchestrators více než jen nakládat s chybou. Další funkce, které spravujete nová nasazení, upgrady zpracování a týkající se spotřeby prostředků a zásad správného řízení. Všechny orchestrators jsou zásadně o údržbu některé požadovaný stav konfigurace v prostředí hello. Chcete mít tootell toobe orchestrator mají a mějte ho hello lifting náročné. Všechny příklady orchestrators jsou programu Aurora nad Mesos, Docker Datacenter nebo Docker Swarm, Kubernetes a Service Fabric. Tyto orchestrators probíhá aktivně vyvinuté toomeet hello potřeby skutečné zatížení v produkčním prostředí. 

## <a name="orchestration-as-a-service"></a>Orchestration jako služby
Hello správce prostředků clusteru je součástí systému hello, která zpracovává orchestration v Service Fabric. Hello prostředků Správce clusteru pro úlohy je rozdělit do tří částí:

1. Vynucení pravidla
2. Optimalizace prostředí
3. Pomoc s jinými procesy

toosee jak funguje hello správce prostředků clusteru, podívejte se na následující videa Microsoft Virtual Academy hello:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=d4tka66yC_5706218965">
<img src="./media/service-fabric-cluster-resource-manager-introduction/ConceptsAndDemoVid.png" WIDTH="360" HEIGHT="244">
</a></center>

### <a name="what-it-isnt"></a>Co není
V aplikacích vrstvy tradiční N, je vždy [nástroj pro vyrovnávání zatížení](https://en.wikipedia.org/wiki/Load_balancing_(computing)). Obvykle se to nástroje pro vyrovnávání zatížení sítě (NLB) nebo aplikaci zatížení vyrovnávání (ALB) v závislosti na tom, kde ji byla ve hello síťového zásobníku. Některé nástroje pro vyrovnávání zatížení jsou založené na hardwaru jako je F5 Big-IP nabídky, jiné jsou na základě softwaru jako je Microsoft je služba Vyrovnávání zatížení sítě. V jiných prostředích může se zobrazit něco jako HAProxy, nginx, Istio nebo Envoy v této roli. V případě architektur se tyto úlohy hello služby Vyrovnávání zatížení je tooensure Bezstavová zatížení přijímat (přibližně) hello stejné množství práce. Strategie pro vyrovnávání zatížení rozmanitých. Některé nástroje pro vyrovnávání byste odesílali každý jiný volání tooa jiný server. Ostatní zadat Připnutí/dlouhodobost relace. Pokročilejší vyrovnávání použít vlastní operaci načtení odhad nebo generování sestav tooroute volání na základě jejího očekávané náklady a aktuálního zatížení počítače.

Nástroje pro vyrovnávání sítě nebo zprávy směrovače se pokusila tooensure hello web nebo worker vrstvy zůstaly zhruba vyrovnáváním. Strategie pro vyrovnávání hello datové vrstvy měla jiné a závislé na mechanizmus pro ukládání dat hello. Vyrovnávání hello datové vrstvy účinné horizontálního dělení dat, ukládání do mezipaměti, spravované zobrazení, uložené procedury a další mechanismy daného obchodu.

Některé z těchto strategií jsou zajímavé, hello správce prostředků clusteru Service Fabric není nic jako nástroj pro vyrovnávání zatížení sítě nebo mezipaměti. Nástroj pro vyrovnávání zatížení sítě vyrovnává frontends tak, že se provoz mezi frontends. Hello správce prostředků clusteru Service Fabric má jinou strategii. Zásadně, Service Fabric přesune *služby* toowhere provádění hello smysluplná, byla očekávána provozu nebo zatížení toofollow. Například je může přesunout toonodes služby, které jsou aktuálně studenou, protože nejsou hello služby, které jsou to množství práce. vzhledem k tomu, že byly hello služby, které byly odstraněny nebo přesunout jinam, může být cold Hello uzly. Například může hello správce prostředků clusteru služby od počítače s také přesunout. Možná hello počítače je o toobe upgradovat nebo je z důvodu tooa Špička energie přetížené hello služby v rámci něj spuštěna. Alernatively, může mít vyšší požadavky na prostředky služby hello. V důsledku nejsou k dispozici dostatečné prostředky na tento počítač toocontinue jeho spuštění. 

Protože hello clusteru Resource Manager je odpovědná za přesunutí služby kolem, obsahuje sada porovnání toowhat různé funkce, by vás nástroj pro vyrovnávání zatížení sítě. Toto je nástroje pro vyrovnávání zatížení sítě přinášejí toowhere provoz sítě, které již jsou služby, a to i v případě, že umístění není ideální pro spouštění samotnou službu hello. Hello správce prostředků clusteru Service Fabric aktivuje se podstatně liší strategie pro zajištění, že jsou efektivně využité hello prostředků v clusteru hello.

## <a name="next-steps"></a>Další kroky
- Informace o hello architektura a informačního toku v rámci hello správce prostředků clusteru, podívejte se na [v tomto článku](service-fabric-cluster-resource-manager-architecture.md)
- Hello správce prostředků clusteru má mnoho možností pro popisující hello clusteru. toofind Další informace o metriky, projděte si tento článek na [popisující cluster Service Fabric](service-fabric-cluster-resource-manager-cluster-description.md)
- Další informace o konfiguraci služby [Další informace o konfiguraci služby](service-fabric-cluster-resource-manager-configure-services.md)(service-fabric-cluster-resource-manager-configure-services.md)
- Metriky se, jak hello správce prostředků clusteru Service Fabric spravuje využívání a kapacity v clusteru hello. Další informace o metriky a jak tooconfigure je rezervovat toolearn [v tomto článku](service-fabric-cluster-resource-manager-metrics.md)
- Hello clusteru Resource Manager spolupracuje se službou Service Fabric možnosti správy. Přečtěte si další informace o integraci, toofind [v tomto článku](service-fabric-cluster-resource-manager-management-integration.md)
- toofind si o tom, jak hello správce prostředků clusteru spravuje a vyrovnává zatížení v clusteru hello, podívejte se na článek hello na [Vyrovnávání zatížení](service-fabric-cluster-resource-manager-balancing.md)
