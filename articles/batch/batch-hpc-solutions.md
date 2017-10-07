---
title: "aaaBatch a řešení prostředí HPC v cloudu hello - Azure | Microsoft Docs"
description: "Další informace o scénářích a možnostech řešení pro dávky (batch) a vysokovýkonné výpočetní prostředí (HPC a Big Compute) v Azure"
services: batch, virtual-machines, cloud-services
documentationcenter: 
author: dlepow
manager: timlt
editor: 
ms.assetid: aab5401d-2baf-4cf2-bf20-ad224de33888
ms.service: batch
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: big-compute
ms.date: 02/27/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c5a3c8859d1f95040bcdad15942a815d71eb4486
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="batch-and-hpc-solutions-for-large-scale-computing-workloads"></a>Řešení pro Batch a prostředí HPC pro rozsáhlé výpočetní úlohy

Azure nabízí efektivní a škálovatelná cloudová řešení pro dávky (batch) a vysokovýkonné výpočetní prostředí (HPC) – taky známé jako *Big Compute* (provádění výpočtů ve velkém měřítku). Přečtěte si zde o úlohy Big Compute a toosupport služby Azure je, nebo přejít přímo příliš[scénářů řešení](#scenarios) dále v tomto článku. Tento článek slouží především pro technická rozhodnutí ve firmě, manažery IT a nezávislé dodavatele softwaru, ale jiné Odborníci v oblasti IT a vývojáři mohou použít it toofamiliarize sami s těmito řešeními.

Organizace musí řešit rozsáhlé výpočetní problémy: technický návrh a analýza, vykreslování obrázků, komplexní modelování, simulace typu Monte Carlo, výpočty finančních rizik a další. Azure pomáhá organizacím vyřešit tyto problémy s prostředky hello, měřítko a plán, který potřebují. Díky službě Azure organizace můžou:

* Vytvořte hybridní řešení, která rozšiřují ve místní HPC clusteru toooffload ve špičce úlohy toohello cloudu
* Spouštět nástroje clusteru prostředí HPC a úlohy jen prostřednictvím služby Azure
* Používat spravované a škálovatelné služby Azure, jako [Batch](https://azure.microsoft.com/documentation/services/batch/) toorun výpočetně náročných úloh bez nutnosti toodeploy a spravovat výpočetní infrastruktury

I když nad rámec hello rámec tohoto článku, Azure také poskytuje vývojářům a partnerům úplnou sadu funkcí, možností architektur a vývoj nástroje toobuild ve velkém měřítku, vlastní pracovní postupy Big Compute. A rostoucí partnerský ekosystém je připraven toohelp můžete své úlohy Big Compute zvýšit produktivitu v hello cloudu Azure.

## <a name="batch-and-hpc-applications"></a>Batch a aplikace prostředí HPC
Na rozdíl od webových aplikací a mnoha obchodních aplikací mají batche a aplikace prostředí HPC definovaný začátek a konec a mohou se spouštět podle plánu nebo na vyžádání. Běžet mohou i několik hodin nebo i déle. Většina spadá do dvou hlavních kategorií: *vnitřně paralelní* (někdy nazývané "trapně paralelní", protože se vyřešit problémy hello samo toorunning paralelně na více počítačích nebo procesorech) a *úzce párované*. Viz následující tabulka Další informace o těchto typech aplikací hello. Některá řešení služby Azure se hodí více pro jeden typ nebo hello jiné.

> [!NOTE]
> U řešení pro Batch a prostředí HPC se spuštěná instance aplikace obvykle nazývá *úloha* (job). Každá úloha se pak může rozdělit na *úkoly* (tasks). A hello Clusterované výpočetní prostředky pro aplikaci hello se často nazývají *výpočetní uzly*.
> 
> 

| Typ | Vlastnosti | Příklady |
| --- | --- | --- |
| **Vnitřně paralelní**<br/><br/>![Vnitřně paralelní][parallel] |• Jednotlivé počítače spouštějí aplikační logiku nezávisle na sobě.<br/><br/> • Přidávání počítačů umožňuje hello aplikace tooscale a snižte dobu výpočtu.<br/><br/>• Aplikace se skládá ze samostatných spustitelných souborů nebo je rozdělená do skupiny služeb, které vyvolává klient (architektura orientovaná na služby nebo aplikace SOA). |• Modelování finančních rizik<br/><br/>• Vykreslování a zpracovávání obrázků<br/><br/>• Kódování a překódování multimédií<br/><br/>• Simulace typu Monte Carlo<br/><br/>• Testování softwaru |
| **Úzce párované**<br/><br/>![Úzce párované][coupled] |• Aplikace vyžaduje výpočetní uzly toointeract nebo exchange mezilehlých výsledků<br/><br/>• Výpočetních uzly mohou komunikovat pomocí hello rozhraní MPI (Message Passing), běžný komunikační protokol pro paralelní výpočty.<br/><br/>• hello aplikace je citlivá toonetwork latencí a šířkou pásma<br/><br/>• Výkon aplikace lze zvýšit pomocí vysokorychlostních síťových technologií, jako je například InfiniBand a přímý přístup do paměti vzdáleného počítače (RDMA). |• Modelování zásob plynu a ropy<br/><br/>• Technický návrh a analýza, například výpočet dynamiky kapaliny<br/><br/>• Fyzické simulace, například autohavárie nebo jaderné reakce<br/><br/>• Předpovědi počasí |

### <a name="considerations-for-running-batch-and-hpc-applications-in-hello-cloud"></a>Důležité informace týkající se spouštění aplikací batch a aplikace prostředí HPC v cloudu hello
Můžete snadno migrovat mnoho aplikací, které jsou navržené toorun v tooAzure clusterů HPC místní, nebo v prostředí tooa hybridní (mezi různými místy). Můžou zde ale existovat jistá omezení a požadavky, například:

* **Dostupnost prostředků cloudu** – v závislosti na typu hello výpočetních prostředků cloudu, které používáte, nemusí být možné toorely na nepřetržitou dostupnost stroje průběhu úlohy. Stav zpracování a průběh kontrolních bodů běžné techniky toohandle možných přechodných chyb a další nezbytné při použití prostředků cloudu.
* **Přístup k datům** – techniky přístupu dat běžně k dispozici v clusterech enterprise, jako je například NFS, můžou vyžadovat zvláštní konfiguraci v cloudu hello. Nebo můžete potřebovat tooadopt různých datových přístup postupy a vzory pro hello cloud.
* **Přesun dat** – pro aplikace, že proces velké objemy dat, strategie jsou potřebné toomove hello data do úložiště a toocompute prostředkům cloudu. K tomu může být nutné použít vysokorychlostní síťové služby pro spojení mezi místy, jako je například [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/). Taky je třeba vzít v potaz omezení ukládání nebo přístupu k datům týkající se práva, norem nebo zásad.
* **Licencování** – jestli se na dodavatele hello jakékoli komerční aplikace pro licencování nebo hello dalších omezení při spouštění v cloudu. Ne všichni dodavatelé nabízejí licencování formou průběžných plateb. Může potřebovat tooplan licenční server v cloudu hello pro vaše řešení, nebo připojit tooan místní licenční server.

### <a name="big-compute-or-big-data"></a>Big Compute nebo Big Data?
Hello dělení řádek mezi aplikacemi pro Big Compute a velké objemy dat není vždy zcela zřejmý a některé aplikace mohou mít vlastnosti obou. Obojí zahrnuje spouštění rozsáhlých výpočtů, obvykle v clusterech počítačů. Ale přístupy hello řešení a podpůrné nástroje se může lišit.

• **Big Compute** obvykle tooinvolve aplikace, které jsou závislé na výkonu procesoru a paměti, jako je například technické simulace, modelování finančních rizik a digitální vykreslování. Hello infrastruktury pro řešení Big Compute by mohla obsahovat počítače se specializovanými vícejádrovými procesory tooperform syrových výpočtů a specializované, vysokorychlostní hardwaru tooconnect hello počítače v síti.

• **Big Data** řeší problémy analýzy dat, které zahrnují velké objemy dat, která není možné spravovat na jednom počítači nebo v jednom systému správy databáze. Jedná se například o velké objemy webových protokolů nebo jiná data business intelligence. Velké objemy dat obvykle toorely Další informace o kapacitu disku a vstupně-výstupní výkon než o výkon procesoru. Existují také specializované nástroje velkých objemů dat, jako je například Apache Hadoop toomanage hello clusteru a oddíl hello data. (Informace o řešení Azure HDInsight a dalších řešeních Azure Hadoop najdete v článku [Hadoop](https://azure.microsoft.com/solutions/hadoop/).)

## <a name="compute-management-and-job-scheduling"></a>Správa výpočtů a plánování úloh
Spouštění aplikací Batch a prostředí HPC často zahrnují *Správce clusteru* a *plánovače úloh* toohelp spravovat Clusterované výpočetní prostředky a přidělovat je toohello aplikace, které umožňuje spouštění úloh hello. Tyto funkce můžou být řešené pomocí samostatných nástrojů nebo integrovaného nástroje nebo služby.

* **Správce clusteru** – zřizuje, uvolňuje a spravuje výpočetní prostředky (nebo výpočetní uzly). Správce clusteru může automatizovat instalaci Image operačního systému a aplikací na výpočetní uzly, škálovat výpočetní prostředky podle toodemands a sledovat výkon hello hello uzlů.
* **Plánovač úloh** -určuje hello prostředky (například procesory, paměť) aplikace vyžaduje a hello podmínky při spuštění. Plánovač úloh organizuje frontu úloh a přiděluje toothem prostředky na základě přiřazené priority nebo dalších vlastností.

Clustering a úlohy plánování nástroje pro clustery s Windows a Linux můžete migrovat dobře tooAzure. Například [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029), bezplatné řešení výpočetního clusteru pro úlohy v prostředí HPC systému Windows a Linux od společnosti Microsoft, nabízí několik možností pro spuštění ve službě Azure. Můžete také vytvořit clustery Linux toorun open source nástroje, jako je například Torque nebo SLURM. Můžete taky zahrnout tooAzure komerční mřížky řešení, například [TIBCO DataSynapse GridServer](https://azure.microsoft.com/blog/tibco-datasynapse-comes-to-the-azure-marketplace/), [IBM spektrum Symphony a Symphony LSF](https://azure.microsoft.com/blog/ibm-and-microsoft-azure-support-spectrum-symphony-and-spectrum-lsf/), a [Univa Grid Engine](http://www.univa.com/products/grid-engine).

Jak je znázorněno v následující části hello, můžete také využít výhod služeb Azure toomanage výpočetní prostředky a plánování úloh bez (nebo kromě) nástroje pro správu tradičních clusteru.

## <a name="scenarios"></a>Scénáře
Tady jsou tři běžné scénáře úlohy Big Compute toorun v Azure pomocí existující řešení clusteru prostředí HPC, služby Azure nebo kombinaci hello dva. Uvedeny jsou klíčové faktory pro výběr jednotlivých scénářů, nejedná se ale o vyčerpávající seznam. Více o hello dostupných službách Azure, které můžete použít ve vašem řešení je dále v článku hello.

| Scénář | Proč zvolit? |
| --- | --- | --- |
| **Burst tooAzure clusteru HPC**<br/><br/>[![Cluster burst][burst_cluster]](./media/batch-hpc-solutions/burst_cluster.png) <br/><br/> Další informace:<br/>• [Burst tooAzure instancí pracovního procesu pomocí sady HPC Pack](https://technet.microsoft.com/library/gg481749.aspx)<br/><br/>• [Nastavení hybridního výpočetního clusteru pomocí sady HPC Pack](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)<br/><br/>• [Burst tooAzure Batch pomocí sady HPC Pack](https://technet.microsoft.com/library/mt612877.aspx)<br/><br/> |• Zkombinujte svou sadu [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) nebo jiný místní cluster s dalšími prostředky služby Azure v rámci hybridního řešení.<br/><br/>• Rozšiřte Big Compute toorun úlohy na platformě jako instance virtuálního počítače služby (PaaS) (aktuálně pouze systém Windows Server).<br/><br/>• Přistupujte k místnímu licenčnímu serveru nebo úložišti dat pomocí volitelné virtuální sítě Azure. |
| **Vytvoření clusteru prostředí HPC zcela ve službě Azure**<br/><br/>[![Cluster in IaaS][iaas_cluster]](./media/batch-hpc-solutions/iaas_cluster.png)<br/><br/>Další informace:<br/>• [Řešení clusteru prostředí HPC ve službě Azure](big-compute-resources.md)<br/><br/> |• Rychle a konzistentně nasazujte své aplikace a nástroje clusteru na standardní nebo vlastní virtuální počítače modelu IaaS (infrastruktura jako služba) systému Windows nebo Linux.<br/><br/>• Spustit různé úlohy Big Compute pomocí řešení zvoleného plánování úloh hello.<br/><br/>• Použijte další služby Azure včetně sítě a úložiště toocreate dokončení cloudové řešení. |
| **Horizontální navýšení kapacity tooAzure paralelní aplikace**<br/><br/>[![Azure Batch][batch_proc]](./media/batch-hpc-solutions/batch_proc.png)<br/><br/>Další informace:<br/>• [Základy služby Azure Batch](batch-technical-overview.md)<br/><br/>• [Začínáme s knihovnou Azure Batch hello pro .NET](batch-dotnet-get-started.md) |• Použijte při vyvíjení [Azure Batch](https://azure.microsoft.com/documentation/services/batch/) tooscale na různých toorun úlohy Big Compute na fondy virtuálních počítačů s Windows nebo Linux.<br/><br/>• Použijte nasazení toomanage služby platformy Azure a automatické škálování virtuálních počítačů, plánování úloh, zotavení po havárii, přesunu dat, správě závislosti a nasazení aplikace. |

## <a name="azure-services-for-big-compute"></a>Služby Azure pro Big Compute
Zde je další informace o výpočetní hello, data, sítě a související služby, které můžete kombinovat pro Big Compute řešeními a pracovními postupy. Podrobné pokyny ke službám Azure, najdete v tématu hello služby Azure [dokumentaci](https://azure.microsoft.com/documentation/). Hello [scénáře](#scenarios) dříve v tomto článku ukazují jenom některé z mnoha možností použití těchto služeb.

> [!NOTE]
> Azure pravidelně zavádí nové služby, které mohou být pro váš scénář užitečné. Pokud máte dotazy, obraťte se na [partnera Azure](https://pinpoint.microsoft.com/en-US/search?keyword=azure) nebo e-mail *bigcompute@microsoft.com*.
> 
> 

### <a name="compute-services"></a>Výpočetní služby
Výpočetní služby Azure jsou jádrem řešení Big Compute a hello různé výpočetní služby jsou vhodné pro různé scénáře hello. Tyto služby na základní úrovni nabízejí různé režimy pro aplikace toorun na bázi virtuálního počítače výpočetní instance, které Azure poskytuje pomocí technologie Windows Server Hyper-V. Na těchto instancích mohou běžet standardní i vlastní nástroje a operační systémy Windows a Linux. Azure vám dává na výběr z různých [velikostí instancí](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) s různými konfiguracemi jader procesoru, paměti, kapacity disku a dalších vlastností. Podle potřeby můžete škálovat instance toothousands hello jader a pak vertikálně snížit kapacitu, až budete potřebovat méně prostředků.

> [!NOTE]
> Využít výhod hello Azure [náročné instance například hello H-series](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) tooimprove hello výkon a škálovatelnost úloh prostředí HPC. Tyto instance podporují paralelní aplikace MPI vyžadující nízkou latenci a vysokou propustnost aplikační sítě. Také k dispozici jsou [N-series](../virtual-machines/windows/sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) virtuálních počítačů s grafickými procesory NVIDIA tooexpand hello rozsah scénáře výpočetních a vizualizace v Azure.  
> 
> 

| Služba | Popis |
| --- | --- |
| **[Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/)**<br/><br/> |• Poskytuje výpočetní infrastrukturu jako službu (IaaS) pomocí technologie Microsoft Hyper-V.<br/><br/>• Umožňují tooflexibly zřizovat a spravovat trvalé cloudové počítače ze standardní bitové kopie systému Windows Server nebo Linux z hello [Azure Marketplace](https://azure.microsoft.com/marketplace/), nebo Image a datového disku, které zadáte<br/><br/>• Můžete nasadit a spravovat jako [škálovatelné sady virtuálních počítačů](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/) toobuild rozsáhlých služeb poskytovaných identickými virtuálními počítači, s tooincrease nebo snížení kapacity automatické škálování, automaticky<br/><br/>• Umožňuje spouštět místně výpočetní cluster nástrojů a aplikací zcela v cloudu hello<br/><br/> |
| **[Cloud Services](https://azure.microsoft.com/documentation/services/cloud-services/)**<br/><br/> |• Umožňuje spouštět aplikace Big Compute na instancích rolí pracovního procesu, což jsou virtuální počítače se systémem Windows Server kompletně spravované službou Azure.<br/><br/>• Umožňuje běh škálovatelných a spolehlivých aplikací s nízkou správní režií, které běží na základě modelu PaaS (platforma jako služba).<br/><br/>• Může vyžadovat další nástroje nebo vývoj toointegrate s místní řešení clusteru prostředí HPC |
| **[Batch](https://azure.microsoft.com/documentation/services/batch/)**<br/><br/> |• Umožňuje spouštět rozsáhlé paralelní a dávkové úlohy prostřednictvím plně spravované služby.<br/><br/>• Poskytuje plánování úloh a automatické škálování spravovaného fondu virtuálních počítačů.<br/><br/>• Umožňuje vývojářům toobuild a spuštění aplikace jako službu nebo cloudu povolit existující aplikace<br/> |

### <a name="storage-services"></a>Služby Storage
Řešení Big Compute obvykle pracuje se sadou vstupních dat a generuje data pro své výsledky. Mezi služby úložiště Azure hello používá v řešeních pro Big Compute, patří:

* [Blob, Table, a Queue Storage](https://azure.microsoft.com/documentation/services/storage/) – umožňují spravovat velké objemy nestrukturovaných dat, dat typu NoSQL a zpráv pro pracovní postup, v uvedeném pořadí. Například můžete použít úložiště objektů blob pro velké sady technických dat nebo vstupní Image hello nebo mediálních souborů vaše aplikace zpracovává. Fronty (queues) můžete v řešení použít k asynchronní komunikaci. V tématu [tooMicrosoft Úvod Azure Storage](../storage/common/storage-introduction.md).
* [Úložiště Azure File](https://azure.microsoft.com/services/storage/files/) -společné soubory sdílené složky a data v Azure pomocí standardního protokol SMB, který je nutný pro některá řešení clusteru prostředí HPC hello.
* [Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) -poskytuje hyperškálovatelný systém Apache Hadoop Distributed File System pro hello cloudu, užitečné pro dávku v reálném čase a interaktivní analýzy.

### <a name="data-and-analysis-services"></a>Datové a analytické služby
Některé scénáře Big Compute zahrnují rozsáhlé datové toky nebo generují data, která potřebují další zpracování nebo analýzu. Azure nabízí několik datových a analytických služeb, včetně:

* [Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) – vytváří pracovní postupy (kanály) řízené daty, které slučují, agregují a transformují data z místních, cloudových a internetových úložišť.
* [Databáze SQL](https://azure.microsoft.com/documentation/services/sql-database/) – poskytuje klíčové funkce hello systému správy relační databáze Microsoft SQL Server prostřednictvím spravované služby.
* [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/) – nasazuje a zřizuje clustery systému Windows Server nebo Apache Hadoop založené na systému Linux v hello cloudu toomanage, analyzovat a sestav velkých objemů.
* [Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) – pomáhá vám vytvářet, testovat, provozovat a spravovat řešení prediktivní analýzy prostřednictvím plně spravované služby.

### <a name="additional-services"></a>Další služby
Řešení Big Compute může být třeba další služby Azure tooconnect tooresources místně nebo v jiných prostředích. Příklady obsahují:

* [Virtuální síť](https://azure.microsoft.com/documentation/services/virtual-network/) – vytvoří logicky izolovaný oddíl v Azure tooconnect prostředky Azure tooeach jiných nebo tooyour místního datového centra. S virtuální sítí mezi různými místy mohou aplikace Big Compute přistupovat k místním datům, službám Active Directory a licenčním serverům.
* [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) – Vytváří privátní připojení mezi datovými centry společnosti Microsoft a infrastrukturou, která se nachází v místním umístění nebo v prostředí ve společném umístění. ExpressRoute nabízí vyšší zabezpečení, další spolehlivost, vyšší rychlost a nižší latenci než Typická připojení přes hello Internet.
* [Service Bus](https://azure.microsoft.com/documentation/services/service-bus/) – poskytuje pro aplikace několik mechanismů toocommunicate nebo exchange data, ať už jsou umístěna v Azure, na jiné cloudové platformě nebo v datovém centru.

## <a name="next-steps"></a>Další kroky
* V tématu [technických prostředcích pro Batch a prostředí HPC](big-compute-resources.md) toofind technické pokyny toobuild řešení.
* Prodiskutujte své možnosti Azure s našimi partnery, mezi které patří Cycle Computing, Rescale nebo UberCloud.
* Přečtěte si informace o řešeních pro Azure Big Compute, o které se podělili [Towers Watson](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18222), [Altair](https://azure.microsoft.com/blog/availability-of-altair-radioss-rdma-on-microsoft-azure/), [ANSYS](https://azure.microsoft.com/blog/ansys-cfd-and-microsoft-azure-perform-the-best-hpc-scalability-in-the-cloud/) a [d3VIEW](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=22088).
* Hello nejnovější oznámení, naleznete v části hello [blog týmu Microsoft HPC a Batch](http://blogs.technet.com/b/windowshpc/) a hello [Azure blog](https://azure.microsoft.com/blog/tag/hpc/).

<!--Image references-->
[parallel]: ./media/batch-hpc-solutions/parallel.png
[coupled]: ./media/batch-hpc-solutions/coupled.png
[iaas_cluster]: ./media/batch-hpc-solutions/iaas_cluster.png
[burst_cluster]: ./media/batch-hpc-solutions/burst_cluster.png
[batch_proc]: ./media/batch-hpc-solutions/batch_proc.png
