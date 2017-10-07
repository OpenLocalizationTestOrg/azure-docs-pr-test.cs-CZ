---
title: "aaaCommon dotazy týkající se služby Microsoft Azure Service Fabric | Microsoft Docs"
description: "Nejčastější dotazy ohledně Service Fabric a jejich odpovědi"
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: 
ms.assetid: 5a179703-ff0c-4b8e-98cd-377253295d12
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2017
ms.author: chackdan
ms.openlocfilehash: 4cbe92d2a03f7a1ea5d077807fdc982288220a7e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="commonly-asked-service-fabric-questions"></a>Často kladené otázky Service Fabric

Existuje mnoho nejčastější dotazy týkající se co můžete udělat Service Fabric a jak se má použít. Tento dokument popisuje mnoho z těchto běžné otázky a odpovědi.

## <a name="cluster-setup-and-management"></a>Instalace a správy

### <a name="can-i-create-a-cluster-that-spans-multiple-azure-regions-or-my-own-datacenters"></a>Můžete vytvořit cluster, který zahrnuje několik oblastí Azure nebo vlastní datových centrech?

Ano. 

Hello základní technologie clusteringu Service Fabric lze použít toocombine počítačů spuštěných kdekoli v hello, world, tak dlouho, dokud budou mít síťové připojení tooeach jiné. Však vytváření a spouštění takového clusteru může být složité.

Pokud vás zajímá v tomto scénáři, doporučujeme tooget v kontaktu buď prostřednictvím hello [Service Fabric Githubu problémy seznamu](https://github.com/azure/service-fabric-issues) nebo vaším zástupcem podpory v pořadí tooobtain další pokyny. Hello Service Fabric tým pracuje další přehlednost tooprovide, pokyny a doporučení pro tento scénář. 

Tooconsider některé věci: 

1. Hello prostředku clusteru Service Fabric v Azure je regionální dnes, jako jsou sady škálování virtuálních počítačů hello hello clusteru je založený na. To znamená, že v případě hello místní selhání může ztratíte hello možnost toomanage hello clusteru přes hello Azure Resource Manager nebo hello portálu Azure. To může dojít, i když hello clusteru zůstane spuštěný a bude moct toointeract s ním přímo. Kromě toho Azure ještě dnes nenabízí možnost toohave hello jedné virtuální sítě, které lze použít v oblastech. To znamená, že cluster více oblasti v Azure vyžaduje buď [veřejné IP adresy pro každý virtuální počítač v hello škálovatelné sady virtuálních počítačů](../virtual-machine-scale-sets/virtual-machine-scale-sets-networking.md#public-ipv4-per-virtual-machine) nebo [Azure VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md). Tyto možnosti sítě mají různé dopady na náklady, výkonu a toosome stupeň návrh aplikace, takže pozor, analýze a plánování je vyžadována před stojící až takovém prostředí.
2. Hello údržby, Správa a monitorování těchto počítačů se může stát složitá, zejména v případě, že rozloženy _typy_ prostředí, například mezi zprostředkovatelé jiný cloud, nebo mezi místními prostředky a Azure . Musí dát pozor, tooensure, který upgraduje, monitorování, Správa a Diagnostika jsou pochopeny hello clusteru a hello aplikace před spuštěním úlohy v produkčním prostředí v takovém prostředí. Pokud již máte spoustu prostředí řešení těchto problémů v Azure nebo v rámci vlastní datových center, je pravděpodobné, že tyto stejné řešení může být použitá při vytváření nebo spuštění clusteru Service Fabric. 

### <a name="do-service-fabric-nodes-automatically-receive-os-updates"></a>Service Fabric uzly automaticky přijímat aktualizace operačního systému?

Není v současné době ale toto je také žádost běžné, že Azure má v úmyslu toodeliver.

V dočasné hello, máme [zadané aplikace](service-fabric-patch-orchestration-application.md) že hello operační systémy pod uzly Service Fabric zůstat opravený a až toodate.

Hello výzvy s aktualizacemi operačního systému je, že obvykle vyžadují restart počítače hello, což vede k dočasné dostupnosti ztráty. Samostatně, který se nejedná o problém, protože Service Fabric se automaticky přesměrování provozu pro tyto uzly tooother služby. Pokud v clusteru hello nejsou koordinované aktualizacím operačního systému, je však hello potenciální, který mnoho uzly přejděte dolů najednou. Takové souběžných restartování může způsobit ztrátu dokončení dostupnosti pro službu nebo v minimálně u konkrétního oddílu (pro stavové služby).

V budoucích hello plánujeme zásady aktualizace toosupport operační systém, které je plně automatizované a koordinované napříč doménami aktualizace, zajistíte, že se zachová dostupnosti, i když restartování počítače a jiné neočekávané chyby.

### <a name="can-i-use-large-virtual-machine-scale-sets-in-my-sf-cluster"></a>Můžete použít velké sady škálování virtuálního počítače v clusteru Moje SF? 

**Krátkodobých odpovědí** – ne. 

**Dlouho zodpovědět** – i když hello velké sady škálování virtuálního počítače umožňují tooscale sady škálování virtuálního počítače maximálně 1000 instance virtuálních počítačů, učiní tak pomocí hello umístění skupin (PGs). Domén selhání (FDs) a aktualizace domény (UDs) jsou jenom konzistentní v rámci umístění skupiny Service fabric používá FDs a UDs rozhodnutí umístění toomake instancí služby repliky nebo služby. Vzhledem k tomu, že jsou porovnatelný z hlediska hello FDs a UDs pouze v rámci skupiny umístění SF nemůžete ji použít. Například pokud má VM1 v so1 topologii FD = 0 a VM9 v SO2 má topologii FD = 4, neznamená, že VM1 virtuálního počítače 2 je na dvou různých stojany hardwaru a proto nelze použít hodnoty hello FD v toto rozhodnutí o umístění případu toomake SF.

Jako hello nedostatečná úroveň 4 načíst vyrovnávání podporu existují další problémy s sady škálování velký virtuální počítač v současné době. Odkazovat toofor [podrobnosti o velké škálování sady](../virtual-machine-scale-sets/virtual-machine-scale-sets-placement-groups.md)



### <a name="what-is-hello-minimum-size-of-a-service-fabric-cluster-why-cant-it-be-smaller"></a>Jaká je minimální velikost hello clusteru Service Fabric? Proč nemůže být menší?

Hello minimální podporovaná velikost clusteru Service Fabric spuštění úlohy v produkčním prostředí je pět uzlů. Pro scénáře vývoje/testování podporujeme clustery tři uzly.

Tyto minimálních neexistuje, protože cluster Service Fabric hello spouští sadu stavová systémových služeb, včetně služby pojmenování hello a manager hello převzetí služeb při selhání. Tyto služby, které sledují, co byly služby nasadit toohello clusteru a kde se aktuálně hostuje, závisí na silnou konzistenci. Tento silnou konzistenci zase, závisí na hello možnost tooacquire *kvora* jakékoli dané aktualizace stavu toohello těchto služeb, kde kvorum představuje striktní většinou hello repliky (N/2 + 1) pro danou službu.

S na pozadí Podívejme se na některé konfigurace možné clusteru:

**Jeden uzel**: Tato možnost nebude poskytovat vysokou dostupnost, protože hello ztráta hello jednoho uzlu z jakéhokoli důvodu znamená hello ztrátě hello celý cluster.

**Dva uzly**: kvora pro službu nasadit mezi dva uzly (N = 2) je 2 (2/2 + 1 = 2). Při ztrátě jednu repliku je možné toocreate kvora. Vzhledem k tomu, že provádění aktualizace služby vyžaduje dočasně trvá dolů repliku, nejedná užitečné konfigurace.

**Tři uzly**: tři uzly (N = 3), hello požadavek toocreate kvora je stále dva uzly (3/2 + 1 = 2). To znamená, že můžete ztratit jednotlivých uzlu a udržení kvora.

Hello tři konfigurace uzlu clusteru je podporována pro vývojové a testovací vzhledem k tomu, že můžete provádět upgrade a zůstanou platné i po selhání jednotlivých uzlů, dokud není nastat současně. Pro úlohy v produkčním prostředí musí být odolné toosuch současné selhání, takže potřebujete pět uzlů.

### <a name="can-i-turn-off-my-cluster-at-nightweekends-toosave-costs"></a>Můžete vypnout Moje clusteru v noci nebo víkendů toosave náklady?

Obecně to možné není. Service Fabric ukládá stav na místní dočasné disků, což znamená, že pokud je virtuální počítač hello přesunutý tooa jiného hostitele, hello data nepřesouvá s ním. V běžném provozu, který není problém jako nový uzel hello zapínají toodate jinými uzly. Pokud zastavíte všechny uzly a je restartovat později, existuje však významné možnost, že většina uzlů hello spustit na nové hostitele a nelze toorecover hello systému.

Pokud chcete toocreate clustery pro testování vašich aplikací před nasazením, doporučujeme dynamicky vytvořte tyto clustery jako součást vaší [nepřetržité integrace/průběžné kanálu nasazení](service-fabric-set-up-continuous-integration.md).


### <a name="how-do-i-upgrade-my-operating-system-for-example-from-windows-server-2012-toowindows-server-2016"></a>Jak mohu provést upgrade operačního systému (například ze systému Windows Server 2012 tooWindows Server 2016)?

Při pracujeme na vylepšené uživatelské prostředí, v současné době jste zodpovědní za hello upgradu. Je nutné upgradovat image hello operačního systému na hello virtuální počítače hello clusteru jeden virtuální počítač najednou. 

## <a name="container-support"></a>Podpora kontejnerů

### <a name="why-are-my-containers-that-are-deployed-toosf-unable-tooresolve-dns-addresses"></a>Proč se Moje kontejnery, které jsou nasazené tooSF nelze tooresolve DNS adresy?

Potíže hlášené v clusterech, které jsou na 5.6.204.9494 verze 

**Zmírnění dopadů** : postupujte podle [tento dokument](service-fabric-dnsservice.md) tooenable hello DNS služby service fabric v clusteru.

**Opravte** : upgradu tooa podporované verze clusteru, které je vyšší než 5.6.204.9494, jakmile bude k dispozici. Pokud je váš cluster nastavená tooautomatic upgrady, pak v clusteru hello automaticky upgradovat toohello verzi, která má potíže pevné.

  
## <a name="application-design"></a>Návrh aplikace

### <a name="whats-hello-best-way-tooquery-data-across-partitions-of-a-reliable-collection"></a>Co je hello nejlepší způsob, jak tooquery dat napříč oddíly spolehlivé kolekce?

Spolehlivé kolekce jsou obvykle [oddílů](service-fabric-concepts-partitioning.md) tooenable škálování pro zvýšení výkonu a propustnosti. To znamená, že může být hello stavu pro danou službu rozloženy 10s nebo 100s počítačů. operace tooperform přes tento úplné datové sady, máte několik možností:

- Vytvoření služby, který se dotazuje všechny oddíly z jiné služby toopull v datech hello vyžaduje.
- Vytvoření služby, který může přijímat data z všechny oddíly z jiné služby.
- Pravidelně nabízet data z jednotlivých obchodů externí tooan služby. Tento přístup je jenom vhodné v případě hello dotazy, které provádíte nejsou součástí základní obchodní logiky.


### <a name="whats-hello-best-way-tooquery-data-across-my-actors"></a>Co je hello nejlepší způsob, jak tooquery dat napříč Moje aktéři?

Aktéři jsou nezávislé jednotky navrženou toobe stavu a výpočty, takže není doporučeno tooperform široký dotazy stavu objektu actor za běhu. Pokud máte třeba tooquery napříč hello úplnou sadu stavu objektu actor, je třeba zvážit buď:

- Nahraďte vaše služby objektu actor stavová spolehlivé služby tak, aby hello počet síťových požadavků toogather všechna data z hello počet aktéři toohello počet oddílů ve službě.
- Navrhování vaší nabízené tooperiodically aktéři jejich stavu tooan externí úložiště pro snazší dotazování. Jako výše, tento přístup je pouze navíc nefungovalo, pokud nejsou potřeba pro vaše modul runtime chování hello dotazy, které provádíte.

### <a name="how-much-data-can-i-store-in-a-reliable-collection"></a>Kolik dat můžete ukládat v kolekci spolehlivé?

Spolehlivé služby jsou obvykle rozdělena na oddíly, takže hello částku, kterou můžete ukládat je omezena pouze hello počtu počítačů, které máte v clusteru hello a hello množství paměti k dispozici v těchto počítačích.

Jako příklad předpokládejme, že máme spolehlivé kolekce ve službě se 100 oddíly a 3 repliky, ukládání objektů, které Průměrná velikost 1kb. Nyní předpokládejme, že máte cluster 10 počítače s 16gb paměti na jeden počítač. Pro jednoduchost a toobe velmi konzervativní předpokládá, že hello operačního systému a systémových služeb, modulu runtime Service Fabric hello a vašim službám využívat 6gb paměti, pak 10gb, které jsou k dispozici na počítač, nebo 100gb pro hello cluster.

Pamatujte, že každý objekt musí být uložené tři časy (primární a dvě repliky), byste měli dostatek paměti pro přibližně 35 miliónů objektů v kolekci, při fungování v celém rozsahu. Doporučujeme však probíhá odolné toohello souběžných ztrátě domény selhání a upgradovací doméně, která představuje asi 1/3 kapacitu a by snížení hello číslo tooroughly milionů 23.

Všimněte si, že tento výpočet také předpokládá:

- Distribuce hello dat mezi oddílů hello je přibližně uniform nebo zda jste generování sestav toohello metriky zatížení clusteru Resource Manager. Ve výchozím nastavení bude vyrovnávat na základě počtu repliky zatížení Service Fabric. V našem příkladu výše, která by v každém uzlu v clusteru hello uveďte 10 primární repliky a 20 sekundární repliky. Že to funguje dobře pro zatížení, která je rovnoměrně rozdělené mezi oddílů hello. Pokud není zatížení i, je nutné, aby hello Resource Manager můžete společně pack menší repliky a povolit více paměti větší tooconsume repliky na jednotlivých uzlech hlásit zatížení.

- Zda je hello spolehlivá služba dotyčném hello pouze jeden ukládání stavu v clusteru hello. Vzhledem k tomu, že bude možné nasadit více služeb tooa clusteru, je nutné toobe s vědomím hello prostředky každý potřebovat toorun a spravovat stav.

- Tento samotného clusteru hello není rostoucí nebo zmenšení. Pokud přidáte více počítačů, Service Fabric se znovu vyvážit dodatečnou kapacitu vaší repliky tooleverage hello, dokud hello počet počítačů převyšuje hello počet oddílů ve vaší služby, protože některé replice nemůžou zahrnovat počítače. Naopak jestli můžete snížit velikost hello hello clusteru odebráním počítače, bude balí více úzce repliky a mít menší celkové kapacity.

### <a name="how-much-data-can-i-store-in-an-actor"></a>Kolik dat můžete ukládat do objektu actor?

Stejně jako u spolehlivé služby hello množství dat, které můžete ukládat do služby objektu actor je omezen pouze hello celkového místa na disku a dostupné paměti mezi hello uzly v clusteru. Jednotlivé aktéři jsou však co nejúčinnější, když jsou použité tooencapsulate malé množství stavu a přidružených obchodní logiku. Obecně platí musí mít jednotlivé objektu actor stavu, který se měří v kilobajtech.

## <a name="other-questions"></a>Další otázky

### <a name="how-does-service-fabric-relate-toocontainers"></a>Jak Service Fabric souvisí toocontainers?

Kontejnery nabízejí jednoduchý způsob toopackage služeb a jejich závislosti tak, aby se spustit konzistentně ve všech prostředích a mohou pracovat izolovaným způsobem na jednom počítači. Service Fabric nabízí způsob toodeploy a spravovat služby, včetně [služby, které byly zabaleny v kontejneru](service-fabric-containers-overview.md).

### <a name="are-you-planning-tooopen-source-service-fabric"></a>Pokud plánujete tooopen zdroj Service Fabric?

Jsme chcete tooopen zdroj hello reliable services a spolehlivé aktéři rozhraní Githubu a bude přijímat projekty toothose příspěvky ze strany komunity. Postupujte podle hello [Service Fabric blog](https://blogs.msdn.microsoft.com/azureservicefabric/) další podrobnosti, jak se budou oznámeny.

Hello aktuálně nejsou žádné plány tooopen zdroj hello modulu runtime Service Fabric.

## <a name="next-steps"></a>Další kroky

- [Další informace o klíčových konceptech Service Fabric a osvědčené postupy](https://mva.microsoft.com/en-us/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965)
