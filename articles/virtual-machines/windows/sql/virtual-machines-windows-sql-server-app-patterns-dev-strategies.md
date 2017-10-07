---
title: "aaaSQL vzory aplikace Server na virtuálních počítačích | Microsoft Docs"
description: "Tento článek se zabývá vzory aplikace pro systém SQL Server na virtuálních počítačích Azure. Poskytuje řešení architekty a vývojáře základ pro dobrý aplikace architektury a návrhu."
services: virtual-machines-windows
documentationcenter: na
author: ninarn
manager: jhubbard
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 41863c8d-f3a3-4584-ad86-b95094365e05
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/31/2017
ms.author: ninarn
ms.openlocfilehash: e18597598fdf9fe534ed07331d6f531d4dca0601
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-patterns-and-development-strategies-for-sql-server-in-azure-virtual-machines"></a>Modely aplikací a vývojové strategie pro SQL Server v Azure Virtual Machines
[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="summary"></a>Shrnutí:
Určení, které vzor aplikací nebo toouse vzory pro vaše aplikace na serveru SQL v Azure prostředí je rozhodnutí o návrhu důležité a vyžaduje důkladná znalost jak spolupracují systému SQL Server a jednotlivé komponenty infrastruktury Azure . Se systémem SQL Server ve službách infrastruktury Azure můžete snadno migrovat, údržbu a monitorování vaší existující systém SQL Server aplikací postavené na systému Windows Server toovirtual počítačů v Azure.

Hello cílem tohoto článku je tooprovide řešení architekty a vývojáře a základ pro dobrý aplikace architektury a návrhu, které může kliknout, při migraci stávající aplikace tooAzure jako jako vývoj nových aplikací v Azure.

Pro každou aplikaci vzor zjistíte, místní scénář, jeho odpovídající řešení využívajících cloud, a hello související technických doporučení. Kromě toho hello článek popisuje specifické pro Azure vývoj strategie tak, aby vaše aplikace můžete navrhnout správně. Kvůli toohello mnoha schémat možné aplikace, se doporučuje architekty a vývojáře se by měl vybrat nejvhodnější vzor hello jejich aplikace a uživatele.

**Přispěvatele do technického:** Leoš Carlos Vargas sleďů, Madhan Arumugam Ramakrishnan

**Odborní recenzenti:** Corey Sanders, Drew McDaniel, Narayan Annamalai, Nir Mashkowski, Sanjay Mishra, Silvano Coriani, Stefan Schackow Tim Hickey, Tim Wieman, Xin Jin

## <a name="introduction"></a>Úvod
Mnoho typů vícevrstvé aplikace můžete vyvíjet oddělením hello součástí hello jinou aplikaci vrstvy na různé počítače stejně jako samostatné součásti. Například můžete umístit hello klientská aplikace a obchodní pravidla součásti v jeden počítač, front-end webové vrstvy a součásti úroveň přístupu dat v jiném počítači a vrstvu databázi back-end v jiném počítači. Tento druh strukturování pomáhá izolovat jednotlivých úrovní od sebe navzájem. Pokud změníte, kde vycházejí z data, není potřeba toochange hello klienta nebo webovou aplikaci ale pouze hello součásti úroveň přístupu data.

Typické *n vrstvá* aplikace obsahuje hello prezentační vrstvy, hello obchodní vrstvou a datovou vrstvou hello:

| Úroveň | Popis |
| --- | --- |
| **Prezentace** |Hello *prezentační vrstvy* (webová vrstva z vrstvy front-end) je hello vrstvy, ve kterém uživatelé pracují s aplikací. |
| **Firmy** |Hello *obchodní vrstvy* (střední úroveň) je vrstva hello této hello prezentační vrstvou a hello data vrstvy použití toocommunicate mezi sebou a zahrnuje hello základní funkce systému hello. |
| **Data** |Hello *datové vrstvy* je v podstatě hello serveru, která ukládá data aplikace (například serveru se systémem SQL Server). |

Aplikačními vrstvami popisují hello logická seskupení hello funkce a součásti v aplikaci; zatímco vrstev popisují hello fyzické distribuce hello funkce a součásti na samostatné fyzické servery, počítače, sítě nebo vzdálené umístění. Hello vrstvy aplikace může nacházet v hello stejný fyzický počítač (hello stejné vrstvě) nebo mohou být distribuovány v samostatných počítačích (n vrstvá) a hello součásti v každé vrstvě komunikovat s komponentami v jiných vrstev prostřednictvím dobře definovaných rozhraní. Si můžete představit hello termín vrstvy jako odkazující toophysical distribuční vzory, jako je dvouvrstvá a třívrstvá, n vrstvá. A **2vrstvý vzor aplikací** obsahuje dvě úrovně aplikace: aplikační server a databázový server. dochází k Hello přímou komunikaci mezi hello aplikační server a databázový server hello. Hello aplikační server obsahuje součásti webovou vrstvu a obchodní vrstvy. V **3vrstvý vzor aplikací**, existují tři aplikačními vrstvami: webový server, aplikační server, který obsahuje vrstvu obchodní logiky hello nebo součástí obchodní vrstvy data access components a databázový server hello. přes hello aplikační server se stane Hello komunikace mezi hello webový server a databázový server hello. Podrobné informace o aplikaci vrstvy a vrstvy, najdete v části [Průvodce architekturou aplikace Microsoft](https://msdn.microsoft.com/library/ff650706.aspx).

Než začnete přečtení tohoto článku, měli byste mít znalosti na základní koncepty hello systému SQL Server a Azure. Informace najdete v tématu [SQL Server Books Online](https://msdn.microsoft.com/library/bb545450.aspx), [systému SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md) a [Azure.com](https://azure.microsoft.com/).

Tento článek popisuje několik vzory aplikace, které může být vhodné pro vaše jednoduché aplikace, jakož i hello vysoce komplexní podnikové aplikace. Před s podrobnostmi o jednotlivých vzor, doporučujeme, aby byste si měli přečíst s hello dostupných dat služby úložiště v Azure, jako například [Azure Storage](../../../storage/common/storage-introduction.md), [Azure SQL Database](../../../sql-database/sql-database-technical-overview.md), a [ SQL Server ve virtuálním počítači Azure](virtual-machines-windows-sql-server-iaas-overview.md). toomake hello nejlepší rozhodnutí o návrhu pro vaše aplikace a pochopit, kdy toouse, jaké úložiště dat služby jasně.

### <a name="choose-sql-server-in-an-azure-virtual-machine-when"></a>Zvolte systému SQL Server ve virtuálním počítači Azure, když:
* Je nutné ovládací prvek v systému SQL Server a Windows. Například to může zahrnovat hello verze systému SQL Server, speciální opravy hotfix, konfigurace výkonu atd.
* Potřebujete úplnou kompatibilitu s místním SQL serverem a mají toomove existující aplikace tooAzure jako-je.
* Chcete tooleverage hello možnosti hello prostředí Azure, ale Azure SQL Database nepodporuje všechny funkce hello, které vaše aplikace vyžaduje. To může zahrnovat hello následující oblasti:
  
  * **Velikost databáze**: během hello Tento článek byl aktualizován, databáze SQL podporuje až too1 TB dat databázi systému. Pokud vaše aplikace vyžaduje více než 1 TB dat a nechcete, aby tooimplement vlastní horizontálního dělení řešení, doporučuje se použití systému SQL Server v virtuálního počítače Azure. Hello nejnovější informace najdete v tématu [škálování se databáze SQL Azure](https://msdn.microsoft.com/library/azure/dn495641.aspx) a [úrovních služby databáze SQL Azure a úrovně výkonu](../../../sql-database/sql-database-service-tiers.md).
  * **Dodržování předpisů HIPAA**: zdravotní péče zákazníků a nezávislí výrobci softwaru (ISV) mohou vybrat [systému SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md) místo [Azure SQL Database](../../../sql-database/sql-database-technical-overview.md) protože SQL Server virtuálním počítači Azure je zahrnutý ve HIPAA obchodní přidružit smlouvy (BÁ). Informace o dodržování předpisů najdete v tématu [Microsoft Azure Trust Center: dodržování předpisů](https://azure.microsoft.com/support/trust-center/compliance/).
  * **Funkce na úrovni instance**: V tomto okamžiku SQL Database nepodporuje funkce, které za provozu mimo hello databáze (například odkazované servery agenta úlohy, FileStream, služby Service Broker, atd.). Další informace najdete v tématu [pokyny databáze SQL Azure a omezení](https://msdn.microsoft.com/library/azure/ff394102.aspx).

## <a name="1-tier-simple-single-virtual-machine"></a>vrstvě 1 (jednoduchý): jeden virtuální počítač
V tomto vzoru aplikaci nasadíte aplikaci systému SQL Server a databáze tooa samostatný virtuální počítač v Azure. Hello stejný virtuální počítač obsahuje klienta nebo webové aplikace, komponenty business, vrstva přístupu k datům a databázový server hello. Hello prezentační, obchodní a datové přístupového kódu jsou logicky oddělené ale se fyzicky nacházejí v počítači s jedním serverem. Většina zákazníků začínat tento vzor aplikací a pak škálovat tak, že přidáte další webové role nebo virtuálních počítačů systému tootheir.

Tento vzor aplikací je užitečné, když:

* Chcete tooperform Jednoduchá migrace tooAzure platformy tooevaluate zda hello platformy přijme požadavky na aplikace, nebo ne.
* Chcete tookeep všechny hello aplikačními vrstvami hostované v hello stejné virtuální počítače v hello stejné Azure datového centra tooreduce hello latenci mezi vrstvami.
* Chcete tooquickly zřídit vývoj a testování prostředí na krátkou dobu.
* Chcete tooperform zátěžové testování u různých úrovně zatížení, ale ve stejnou dobu, není vhodné tooown a udržovat mnoho fyzického počítače celou dobu hello hello.

Hello následující diagram ukazuje jednoduchý místní scénář a nasazení jeho řešení cloud povolené na jednom virtuálním počítači v Azure.

![1vrstvý vzor aplikací](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728008.png)

Nasazení hello obchodní vrstvy (obchodní logiku a data přístup k součástem) na hello stejné fyzické vrstvě jako hello prezentační vrstva můžete zvýšit výkon aplikace, pokud je nutné použít samostatné vrstvy kvůli tooscalability nebo bezpečnostní otázky.

Vzhledem k tomu, že to je velmi běžné toostart vzor s, můžete se setkat hello následujícího článku na migraci užitečná pro přesun vaše data tooyour virtuální počítač SQL Server: [migrace databáze tooSQL Server na virtuálním počítači Azure](virtual-machines-windows-migrate-sql.md).

## <a name="3-tier-simple-multiple-virtual-machines"></a>3vrstvá (jednoduchý): více virtuálních počítačů
V tomto vzoru aplikace nasadit 3vrstvé aplikace v Azure tak, že každý aplikační vrstvě jiný virtuální počítač. To poskytuje flexibilní prostředí pro snadné škálování zatížení a škálování scénáře. Pokud jeden virtuální počítač obsahuje klienta nebo webové aplikace, hello jiného hostitelem součásti vaší firmy a hello další databázový server hello jednoho hostitele.

Tento vzor aplikací je užitečné, když:

* Chcete tooperform migrace komplexní databáze aplikace tooAzure virtuálních počítačů.
* Chcete toobe vrstev různé aplikace hostované v různých oblastech. Může mít například sdílené databáze, které jsou nasazené toomultiple oblasti pro účely vytváření sestav.
* Budete chtít toomove podnikové aplikace z místního virtualizované platformy tooAzure virtuálních počítačů. Podrobné informace o podnikové aplikace, najdete v části [co je podniková aplikace](https://msdn.microsoft.com/library/aa267045.aspx).
* Chcete tooquickly zřídit vývoj a testování prostředí na krátkou dobu.
* Chcete tooperform zátěžové testování u různých úrovně zatížení, ale ve stejnou dobu, není vhodné tooown a udržovat mnoho fyzického počítače celou dobu hello hello.

Hello následující diagram ukazuje, jak můžete umístit jednoduché 3vrstvé aplikace v Azure tak, že každý aplikační vrstvě jiný virtuální počítač.

![vzor 3vrstvé aplikace](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728009.png)

V tomto vzoru aplikace není v jednotlivých vrstvách jenom jeden virtuální počítač (VM). Pokud máte víc virtuálních počítačů v Azure, doporučujeme, abyste nastavili virtuální sítě. [Virtuální síť Azure](../../../virtual-network/virtual-networks-overview.md) vytvoří hranici zabezpečení pro důvěryhodné a zároveň umožňuje toocommunicate virtuální počítače mezi sebou přes hello privátní IP adresu. Kromě toho vždy ujistěte se, že všechna připojení k Internetu, jdou pouze toohello prezentační vrstvy. Podle tohoto vzoru aplikace, spravovat hello zabezpečení skupiny pravidel toocontrol přístup k síti. Další informace najdete v tématu [povolit externí přístup tooyour virtuální počítač pomocí portálu Azure hello](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

V diagramu hello může být Internetové protokoly TCP, UDP, HTTP nebo HTTPS.

> [!NOTE]
> Nastavení virtuální sítě v Azure je zdarma. Však budou účtovat hello brány VPN, který se připojuje tooon místní. Tento poplatek je založena na hello množství času, který je zřízený a dostupné připojení.
> 
> 

## <a name="2-tier-and-3-tier-with-presentation-tier-scale-out"></a>úroveň 2 a 3 úrovně s prezentační vrstvy Škálováním na více systémů
V tomto vzoru aplikace nasadit vrstvy 2 nebo 3 úrovně databáze aplikace tooAzure virtuální počítače tak, že každý aplikační vrstvě jiný virtuální počítač. Kromě toho můžete škálovat hello prezentační vrstvy kvůli tooincreased objem příchozích požadavků klienta.

Tento vzor aplikací je užitečné, když:

* Budete chtít toomove podnikové aplikace z místního virtualizované platformy tooAzure virtuálních počítačů.
* Chcete tooscale out hello prezentační vrstvy kvůli tooincreased objem příchozích požadavků klienta.
* Chcete tooquickly zřídit vývoj a testování prostředí na krátkou dobu.
* Chcete tooperform zátěžové testování u různých úrovně zatížení, ale ve stejnou dobu, není vhodné tooown a udržovat mnoho fyzického počítače celou dobu hello hello.
* Chcete tooown prostředí infrastruktury, které můžete škálovat nahoru a dolů na vyžádání.

Hello následující diagram ukazuje, jak můžete umístit hello aplikačními vrstvami ve více virtuálních počítačů v Azure pomocí škálování hello prezentační vrstvy kvůli tooincreased objem příchozích požadavků klienta. Jak je vidět v diagramu hello, je zodpovědná za distribuci přenosů mezi několik virtuálních počítačů a také určit, které tooconnect webového serveru na Vyrovnávání zatížení Azure. S více instancí hello webových serverů za službou Vyrovnávání zatížení zajistí vysokou dostupnost hello hello prezentační vrstvy.

![Vzor aplikací – prezentační vrstvy s více instancemi](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728010.png)

### <a name="best-practices-for-2-tier-3-tier-or-n-tier-patterns-that-have-multiple-vms-in-one-tier"></a>Osvědčené postupy pro vrstvy 2, 3 úrovně nebo n vrstvá vzorů, které mají víc virtuálních počítačů v jedné vrstvy
Doporučujeme umístit hello virtuálních počítačů, které patří toohello stejné vrstvy v hello stejné cloudové služby a v hello stejné hello sady dostupnosti. Například, umístěte sadu webové servery v **CloudService1** a **AvailabilitySet1** a sada databázových serverů v **CloudService2** a  **AvailabilitySet2**. V Azure sadu dostupnosti umožňuje tooplace hello vysokou dostupnost uzly do samostatné selhání domén a domén upgradu.

tooleverage více instancí virtuálního počítače z vrstvy, je nutné tooconfigure nástroj pro vyrovnávání zatížení Azure mezi aplikačními vrstvami. tooconfigure Vyrovnávání zatížení na jednotlivých úrovních samostatně vytvořit koncový bod Vyrovnávání zatížení na jednotlivých úrovních virtuálních počítačích. Pro konkrétní úroveň, nejprve vytvořit virtuální počítače v hello stejné cloudové služby. To zajistí, že budou mít hello stejné veřejná virtuální IP adresy. Dále vytvořte koncový bod na jednom z hello virtuální počítače v této vrstvě. Pak přiřaďte hello stejný koncový bod toohello jinými virtuálními počítači v této vrstvě služby Vyrovnávání zatížení. Vytvořením sady vyrovnáváním zatížení distribuovat provoz napříč více virtuálních počítačů a také povolit hello Vyrovnávání zatížení toodetermine které tooconnect uzlu při selhání uzlu virtuálního počítače back-end. Například s více instancí hello webových serverů za službou Vyrovnávání zatížení zajistí vysokou dostupnost hello hello prezentační vrstvy.

Jako osvědčený postup vždy ujistěte, že všechna připojení k Internetu nejprve přejděte toohello prezentační vrstvy. Prezentační vrstva Hello přistupuje k hello obchodní vrstvy a následně hello obchodní vrstvy přistupuje k hello datové vrstvy. Další informace o přístupu toohello prezentační vrstvy tooallow najdete v tématu [povolit externí přístup tooyour virtuální počítač pomocí portálu Azure hello](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Všimněte si, že hello Vyrovnávání zatížení v Azure funguje podobně jako tooload vyrovnávání v místním prostředí. Další informace najdete v tématu [pro infrastrukturu Azure služby Vyrovnávání zatížení](../tutorial-load-balancer.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Kromě toho doporučujeme, abyste nastavili privátní sítě pro virtuální počítače pomocí Azure Virtual Network. To jim umožňuje toocommunicate mezi sebou přes hello privátní IP adresu. Další informace najdete v tématu [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md).

## <a name="2-tier-and-3-tier-with-business-tier-scale-out"></a>úroveň 2 a 3 úrovně s obchodní vrstvy Škálováním na více systémů
V tomto vzoru aplikace nasadit vrstvy 2 nebo 3 úrovně databáze aplikace tooAzure virtuální počítače tak, že každý aplikační vrstvě jiný virtuální počítač. Kromě toho můžete chtít toodistribute hello aplikace server součásti toomultiple virtuálních počítačů z důvodu složitosti toohello vaší aplikace.

Tento vzor aplikací je užitečné, když:

* Budete chtít toomove podnikové aplikace z místního virtualizované platformy tooAzure virtuálních počítačů.
* Chcete toodistribute hello aplikace server součásti toomultiple virtuálních počítačů z důvodu složitosti toohello vaší aplikace.
* Chcete toomove obchodní logiky velkou místní LOB (-obchodní) aplikace tooAzure virtuálních počítačů. Aplikace LOB jsou sady důležitá počítačová aplikace, které jsou důležité toorunning podniku, jako jsou monitorování účtů, lidských zdrojů, mzdy, řízení a prostředek plánování aplikace.
* Chcete tooquickly zřídit vývoj a testování prostředí na krátkou dobu.
* Chcete tooperform zátěžové testování u různých úrovně zatížení, ale ve stejnou dobu, není vhodné tooown a udržovat mnoho fyzického počítače celou dobu hello hello.
* Chcete tooown prostředí infrastruktury, které můžete škálovat nahoru a dolů na vyžádání.

Hello následující diagram ukazuje scénářem místní a jeho řešení pro cloud povolené. V tomto scénáři umístit hello aplikačními vrstvami ve více virtuálních počítačů v Azure pomocí škálování hello obchodní vrstvy, obsahující hello vrstvu obchodní logiky a součásti přístup k datům. Jak je vidět v diagramu hello, je zodpovědná za distribuci přenosů mezi několik virtuálních počítačů a také určit, které tooconnect webového serveru na Vyrovnávání zatížení Azure. Má několik instancí serverů aplikace hello za službou Vyrovnávání zatížení zajistí vysokou dostupnost hello hello obchodní vrstvy. Další informace najdete v tématu [osvědčené postupy pro vzory vrstvy 2, 3 úrovně nebo vícevrstvé aplikace, které mají více virtuálních počítačů v jedné vrstvy](#best-practices-for-2-tier-3-tier-or-n-tier-patterns-that-have-multiple-vms-in-one-tier).

![Vzor aplikací s obchodní vrstvy s více instancemi](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728011.png)

## <a name="2-tier-and-3-tier-with-presentation-and-business-tiers-scale-out-and-hadr"></a>úroveň 2 a 3 úrovně s prezentační a obchodní vrstvy Škálováním na více systémů a funkci hadr současně
V tomto vzoru aplikace distribuci hello prezentační vrstvy (webový server) nasadit vrstvy 2 nebo 3 úrovně databáze aplikace tooAzure virtuální počítače a hello virtuální počítače součásti toomultiple obchodní vrstvy (aplikačního serveru). Kromě toho implementovat řešení pro zotavení vysokou dostupnost a po havárii pro vaše databáze ve virtuálních počítačích Azure.

Tento vzor aplikací je užitečné, když:

* Chcete toomove podnikové aplikace z virtualizované platformy místní tooAzure implementací systému SQL Server vysokou dostupnost a po havárii možnosti obnovení.
* Chcete tooscale hello prezentační vrstvou a obchodní vrstvou hello kvůli tooincreased objem příchozí žádosti klientů a složitost hello vaší aplikace.
* Chcete tooquickly zřídit vývoj a testování prostředí na krátkou dobu.
* Chcete tooperform zátěžové testování u různých úrovně zatížení, ale ve stejnou dobu, není vhodné tooown a udržovat mnoho fyzického počítače celou dobu hello hello.
* Chcete tooown prostředí infrastruktury, které můžete škálovat nahoru a dolů na vyžádání.

Hello následující diagram ukazuje scénářem místní a jeho řešení pro cloud povolené. V tomto scénáři můžete škálovat hello prezentační vrstvou a součásti vrstvy hello firmy v více virtuálních počítačů v Azure. Kromě toho implementovat vysokou dostupnost a po havárii obnovení (HADR) techniky pro databáze serveru SQL v Azure.

Více kopií aplikace spuštěné v různých virtuálních počítačů Ujistěte se, že jste Vyrovnávání zatížení požadavky napříč je. Pokud máte více virtuálních počítačů, je potřeba toomake se, že všechny virtuální počítače jsou přístupné a spuštěné v jednom bodě v čase. Při konfiguraci služby Vyrovnávání zatížení, Vyrovnávání zatížení Azure sleduje hello stavu virtuálních počítačů a směruje příchozí volání toohello pořádku funkční virtuálních počítačů uzly správně. Informace o tom najdete v části tooset až Vyrovnávání zatížení hello virtuálních počítačů, [pro infrastrukturu Azure služby Vyrovnávání zatížení](../tutorial-load-balancer.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). S více instancí webové a aplikační servery za službou Vyrovnávání zatížení zajistí vysokou dostupnost hello hello prezentační a obchodní vrstvy.

![Škálováním na více systémů a vysoké dostupnosti](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728012.png)

### <a name="best-practices-for-application-patterns-requiring-sql-hadr"></a>Osvědčené postupy pro vzory aplikace vyžadující SQL HADR
Při nastavování vysoká dostupnost SQL serveru a řešení zotavení po havárii v Azure Virtual Machines, nastavení virtuální sítě pro virtuální počítače pomocí [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md) je povinný.  Virtuální počítače v rámci virtuální sítě i po výpadku služby bude mít stabilní privátní IP adresy, proto se můžete vyhnout hello aktualizace čas potřebný pro překlad názvu DNS. Kromě toho hello virtuální sítě vám umožní tooextend místní sítě tooAzure a vytvoří hranici zabezpečení důvěryhodné. Pokud aplikace obsahuje omezení podnikové doméně (například ověřování systému Windows, služby Active Directory), například nastavení [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md) je nezbytné.

Většina zákazníků, kteří běží produkčním kódu v Azure, jsou udržuje primární a sekundární repliky v Azure.

Kurzy o vysoké dostupnosti a techniky pro zotavení po havárii a komplexní informace najdete v tématu [vysokou dostupnost a zotavení po havárii pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="2-tier-and-3-tier-using-azure-vms-and-cloud-services"></a>úroveň 2 a 3 vrstvě pomocí cloudových služeb a virtuálních počítačích Azure
V tomto vzoru aplikace můžete nasadit aplikace vrstvy 2 nebo 3 úrovně tooAzure pomocí obě [Azure Cloud Services](../../../cloud-services/cloud-services-choose-me.md#tellmecs) (webových a pracovních rolí – platforma jako služba (PaaS)) a [virtuální počítače Azure](../overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) () Infrastruktura jako služba (IaaS)). Pomocí [Azure Cloud Services](https://azure.microsoft.com/documentation/services/cloud-services/) pro hello prezentační vrstvy nebo obchodní vrstvou a SQL Server v [virtuální počítače Azure](../overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) pro hello data úroveň je výhodné pro většinu aplikací běžících v Azure. Hello důvodem je, že má do výpočetní instance spuštěné v cloudové služby poskytuje snadnou správu, nasazení, monitorování a Škálováním na více systémů.

S cloudovými službami Azure udržuje hello infrastruktury pro vás, provádí se pravidelná údržba, opravy hello operačních systémů a pokusí toorecover selhání služby a hardwaru. Pokud aplikace potřebuje Škálováním na více systémů, automatickou a ruční škálování možnosti jsou dostupné pro projekt cloudové služby zvýšením nebo snížením hello počet instancí nebo virtuálních počítačů, které jsou používány vaší aplikace. Kromě toho můžete vytvořit místní sady Visual Studio toodeploy projekt aplikace tooa cloudové služby v Azure.

V souhrnu Pokud nechcete, aby tooown rozsáhlé úlohy správy pro hello prezentace nebo obchodní vrstvou a aplikace nevyžaduje žádné složité konfiguraci softwaru nebo hello operačního systému, použijte Azure Cloud Services. Pokud Azure SQL Database nepodporuje všechny funkce hello, které hledáte, použití systému SQL Server v Azure virtuálního počítače pro hello datové vrstvy. Spuštění aplikace na Azure Cloud Services a ukládání dat v Azure Virtual Machines kombinuje výhody hello obě služby. Podrobné porovnání najdete v tématu hello části v tomto tématu na [porovnávání vývoj strategie v Azure](#comparing-web-development-strategies-in-azure).

V tomto vzoru aplikace hello prezentační vrstva obsahuje webové role, která v prostředí Azure provádění hello běží komponentu cloudové služby a je přizpůsobené pro programování webové aplikace podporuje službu IIS a ASP.NET. úroveň Hello firmy nebo back-end zahrnuje roli pracovního procesu, který komponentu cloudové služby běží v prostředí Azure provádění hello a je vhodný pro vývoj zobecněný a může provádět zpracování na pozadí pro webovou roli. Hello databázové vrstvy se nachází v virtuálního počítače s SQL serverem v Azure. Hello komunikace mezi hello prezentační vrstvou a databázové vrstvy hello se stane, přímo nebo přes hello obchodní vrstvy – součásti role pracovního procesu.

Tento vzor aplikací je užitečné, když:

* Chcete toomove podnikové aplikace z virtualizované platformy místní tooAzure implementací systému SQL Server vysokou dostupnost a po havárii možnosti obnovení.
* Chcete tooown prostředí infrastruktury, které můžete škálovat nahoru a dolů na vyžádání.
* Azure SQL Database nepodporuje všechny funkce hello, které musí vaše aplikace nebo databáze.
* Chcete tooperform zátěžové testování u různých úrovně zatížení, ale ve stejnou dobu, není vhodné tooown a udržovat mnoho fyzického počítače celou dobu hello hello.

Hello následující diagram ukazuje scénářem místní a jeho řešení pro cloud povolené. V tomto scénáři můžete umístit hello prezentační vrstvy v webové role, vrstvy obchodní hello rolí pracovního procesu, ale hello datové vrstvy ve virtuálních počítačích v Azure. Tooload vyrovnávat požadavky na spuštění více kopií hello prezentační vrstvy v jiných webových rolí zajišťuje mezi nimi. Když zkombinujete Azure Cloud Services pomocí Azure Virtual Machines, doporučujeme, abyste nastavili [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md) také. S [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md), může mít stabilní a trvalé privátní IP adresy v rámci stejné cloudové služby v hello hello cloudu. Jakmile definovat virtuální sítě pro virtuální počítače a cloudové služby, můžou spustit komunikaci mezi sebou přes hello privátní IP adresu. Kromě toho mají virtuální počítače a Azure web/role pracovního procesu v hello stejné [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md) zajišťuje nízkou latencí a bezpečnější připojení. Další informace najdete v tématu [co je Cloudová služba](../../../cloud-services/cloud-services-choose-me.md).

Jak je vidět v diagramu hello, nástroj pro vyrovnávání zatížení Azure rozděluje zatížení mezi více virtuálních počítačů a také určuje, které webový server nebo aplikačního serveru tooconnect k. S více instancí hello webové a aplikační servery za službou Vyrovnávání zatížení zajistí vysokou dostupnost hello hello prezentační vrstvou a obchodní vrstvou hello. Další informace najdete v tématu [osvědčených postupů pro aplikace vzory vyžadují SQL HADR](#best-practices-for-application-patterns-requiring-sql-hadr).

![Vzory aplikace s cloudovými službami](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728013.png)

Jiný přístup tooimplement tento vzor aplikací je toouse konsolidované webovou roli, která obsahuje prezentační vrstvou a obchodní vrstvy součásti, jak je znázorněno v následující hello diagram. Tento vzor aplikací je užitečná pro aplikace, které vyžadují stavová návrhu. Vzhledem k tomu, že Azure poskytuje bezstavové výpočetní uzly na webových a pracovních rolí, doporučujeme implementovat logiku toostore relace stavu pomocí jedné z následujících technologií hello: [ukládání do mezipaměti Azure](https://azure.microsoft.com/documentation/services/redis-cache/), [Azure Table Storage](../../../cosmos-db/table-storage-how-to-use-dotnet.md) nebo [databáze Azure SQL](../../../sql-database/sql-database-technical-overview.md).

![Vzory aplikace s cloudovými službami](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728014.png)

## <a name="pattern-with-azure-vms-azure-sql-database-and-azure-app-service-web-apps"></a>Vzor s virtuální počítače Azure, Azure SQL Database a Azure App Service (webové aplikace)
primární cílem Hello tohoto vzoru aplikace je tooshow můžete jak toocombine infrastrukturu Azure jako součásti služby (IaaS) s Azure součásti platformy jako služby (PaaS) ve vašem řešení. Tento vzor se zaměřuje na Azure SQL Database pro relační datové úložiště. Neobsahuje systému SQL Server ve virtuálním počítači Azure, který je součástí hello infrastrukturu Azure jako nabídky služeb.

V tomto vzoru aplikaci nasadíte tooAzure aplikace databáze tím, že umístíte hello prezentace a hello podnikové úrovně ve stejné virtuální počítač a přístup k databázi v servery Azure SQL Database (databáze SQL). Prezentační vrstva hello můžete implementovat pomocí tradičních webovou službu IIS řešení. Nebo můžete implementovat kombinované prezentační a obchodní vrstvou pomocí [Azure Web Apps](https://azure.microsoft.com/documentation/services/app-service/web/).

Tento vzor aplikací je užitečné, když:

* Už máte existující databázi SQL server nakonfigurovaný v Azure a chcete tootest aplikace rychle.
* Chcete tootest hello možnosti prostředí Azure.
* Chcete tooquickly zřídit vývoj a testování prostředí na krátkou dobu.
* Vaše obchodní logiku a data komponenty přístup může být obsaženy v rámci webové aplikace.

Hello následující diagram ukazuje scénářem místní a jeho řešení pro cloud povolené. V tomto scénáři můžete umístit hello aplikačními vrstvami na jednom virtuálním počítači v Azure a přístup dat ve službě Azure SQL Database.

![Vzor smíšený aplikací](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728015.png)

Pokud se rozhodnete tooimplement kombinované webové a aplikační vrstvy pomocí Azure Web Apps, doporučujeme zachovat úroveň střední vrstvy nebo aplikace hello jako dynamické knihovny (DLL) v kontextu hello webové aplikace.

Kromě toho zkontrolujte hello doporučení uvedených v hello [porovnávání webové vývoj strategie v Azure](#comparing-web-development-strategies-in-azure) části na hello konci tohoto článku toolearn informace o programování techniky.

## <a name="n-tier-hybrid-application-pattern"></a>Vzor hybridní vícevrstvé aplikace
V hybridní vícevrstvé aplikace vzoru implementovat aplikaci na více úrovních distribuovány mezi místními a Azure. Proto je flexibilní a opakovaně použitelné hybridní systému, kterou můžete změnit nebo přidat vytvořit konkrétní úroveň beze změny hello jiných vrstev. cloud tooextend toohello vaší podnikové síti, je použít [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md) služby.

Tento vzor hybridní aplikace je užitečné, když:

* Chcete toobuild aplikace, které běží částečně v cloudu hello a částečně místně.
* Chcete toomigrate některé nebo všechny elementy ve stávající místní aplikace toohello cloudu.
* Chcete toomove podnikové aplikace z místní virtualizované tooAzure platformy.
* Chcete tooown prostředí infrastruktury, které můžete škálovat nahoru a dolů na vyžádání.
* Chcete tooquickly zřídit vývoj a testování prostředí na krátkou dobu.
* Chcete cenově dostupný způsob tootake zálohy pro podnikové aplikace databáze.

Hello následující diagram ukazuje vzor aplikací hybridní n vrstvá, které platí pro místní a Azure. Jak je vidět v diagramu hello, místní infrastruktury patří [Active Directory Domain Services](https://technet.microsoft.com/library/hh831484.aspx) ověření uživatele toosupport řadiče domény a autorizaci. Všimněte si, že hello diagram ukazuje scénář, kde některé části hello datové vrstvy za provozu v datovém centru místní zatímco některé části hello datové vrstvy za provozu v Azure. V závislosti na potřebách vaší aplikace můžete implementovat několik dalších hybridní scénáře. Například může uchovávat hello prezentační vrstvou a obchodní vrstvou hello v vrstvou místní prostředí, ale hello dat v Azure.

![N-vrstvý vzor aplikací](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728016.png)

V Azure, můžete použít služby Active Directory jako samostatné cloudu adresář pro vaši organizaci, nebo můžete také integrovat existující místní služby Active Directory s [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/). Jak je vidět v diagramu hello, hello obchodní vrstvy součásti můžete přístup k toomultiple datových zdrojů, například příliš[systému SQL Server v Azure](virtual-machines-windows-sql-server-iaas-overview.md) prostřednictvím privátní interní IP adresy, tooon místní SQL Server prostřednictvím [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md), nebo příliš[SQL Database](../../../sql-database/sql-database-technical-overview.md) pomocí technologie zprostředkovatele dat .NET Framework hello. V tomto diagramu je Azure SQL Database služby storage volitelnými daty.

Ve vzoru hybridní vícevrstvé aplikace můžete implementovat hello následující pracovní postup v uvedeném pořadí hello:

1. Identifikovat enterprise databázové aplikace, které je třeba přesunout nahoru toocloud pomocí hello toobe [Microsoft Assessment and Planning (MAP) Toolkit](http://microsoft.com/map). Hello MAP Toolkit shromáždí data inventáře a výkonu z počítačů, které je třeba zajistit pro virtualizaci a obsahuje doporučení týkající se kapacity a plánování hodnocení.
2. Plánování hello prostředků a konfigurace je potřeba v hello platformy Azure, jako je například účty úložiště a virtuálních počítačů.
3. Nastavit připojení k síti mezi hello podnikové sítě na místě a [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md). tooset se hello propojení mezi hello podnikové síti místní a virtuální počítač v Azure, použijte jednu z následujících dvou metod hello:
   
   1. Navázat připojení mezi místními a Azure prostřednictvím veřejné koncové body na virtuálním počítači v Azure. Tato metoda poskytuje snadno instalační program a umožní vám toouse ověřování serveru SQL Server ve virtuálním počítači. Kromě toho nastavte toohello toocontrol veřejné provoz vaší sítě zabezpečení skupiny pravidel virtuálních počítačů. Další informace najdete v tématu [povolit externí přístup tooyour virtuální počítač pomocí portálu Azure hello](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
   2. Navázat připojení mezi místními a Azure přes tunelové propojení Azure virtuální privátní sítě (VPN). Tato metoda vám umožní tooextend domény zásady tooa virtuálního počítače v Azure. Kromě toho můžete nastavit pravidla brány firewall a použít ověřování systému Windows ve virtuálním počítači. Azure v současné době podporuje bezpečné site-to-site VPN a připojení point-to-site VPN:
      
      * S zabezpečené připojení site-to-site můžete vytvořit připojení k síti mezi vaší místní sítí a virtuální sítí v Azure. Doporučuje se pro připojení vaší místní data center prostředí tooAzure.
      * S zabezpečené připojení point-to-site můžete vytvořit připojení k síti mezi vaší virtuální sítě v Azure a vaše jednotlivé počítače se systémem kdekoli. Většinou doporučujeme pro vývojová a testovací účely.
      
      Informace o tom tooconnect tooSQL Server v Azure, najdete v části [připojit tooa virtuálního počítače systému SQL Server na platformě Azure](virtual-machines-windows-sql-connect.md).
4. Nastavte naplánované úlohy a výstrahy, které zálohovat data místně v disku virtuálního počítače v Azure. Další informace najdete v tématu [zálohování systému SQL Server a obnovení služby úložiště objektů Blob Azure](https://msdn.microsoft.com/library/jj919148.aspx) a [zálohování a obnovení pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).
5. V závislosti na potřebách vaší aplikace můžete implementovat jednu hello následující tři běžné scénáře:
   
   1. Na databázovém serveru v Azure můžete chránit váš webový server, aplikační server a malých a velkých písmen data, zatímco zachovat hello citlivá data místně.
   2. Abyste zajistili, že váš webový server a aplikační server místní vzhledem k tomu hello databázový server ve virtuálním počítači v Azure.
   3. Můžete ponechat databázového serveru, webového serveru a aplikačního serveru místní zatímco zachovat repliky databáze hello ve virtuálních počítačích v Azure. Toto nastavení umožňuje hello místní webové servery nebo vytváření sestav aplikace tooaccess hello repliky databáze v Azure. Proto můžete dosáhnout toolower hello zatížení v místní databázi. Doporučujeme vám, že můžete implementovat tento scénář pro těžký číst úlohy a pro účely vývoje. Informace o vytvoření repliky databáze v Azure najdete v tématu skupiny dostupnosti AlwaysOn v [vysokou dostupnost a zotavení po havárii pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="comparing-web-development-strategies-in-azure"></a>Porovnání webových vývoj strategie v Azure
tooimplement a nasazovat vícevrstvé aplikace založené na systému SQL Server v Azure, můžete použít jednu z následujících dvou metod programovací hello:

* V databázích Azure a přístup v systému SQL Server v Azure Virtual Machines nastavte tradiční webový server (IIS - Internetová informační služba).
* Implementace a nasaďte tooAzure cloudové služby. Pak se ujistěte, že tuto cloudovou službu můžete přístup k databází v systému SQL Server v Azure virtuální počítače. Cloudové služby může obsahovat několik webových a pracovních rolí.

Hello následující tabulka obsahuje porovnání tradiční webové vývoj s Azure Cloud Services a Azure Web Apps s ohledem tooSQL Server v Azure Virtual Machines. Hello tabulka obsahuje Azure Web Apps, protože je možné toouse systému SQL Server ve virtuálním počítači Azure jako zdroj dat pro webové aplikace Azure přes jeho veřejné virtuální IP adresa nebo název DNS.

|  | Vývoj tradiční webu v Azure Virtual Machines | Cloudové služby v Azure | Webového hostingu s webové aplikace Azure |
| --- | --- | --- | --- |
| **Migrace aplikací z místní** |Existující aplikace jako-je. |Aplikace potřebují webových a pracovních rolí. |Existující aplikace jako-ale hodí se pro samostatný webové aplikace a webové služby, které vyžadují rychlou škálovatelnost. |
| **Vývoj a nasazení** |Visual Studio, služba WebMatrix, Visual Web Developer, WebDeploy, FTP, sady TFS, Správce služby IIS, prostředí PowerShell. |Visual Studio, Azure SDK a sady TFS, prostředí PowerShell. Jednotlivých cloudových služeb má dva toowhich prostředí můžete nasadit balíček služby a konfigurace: pracovní a provozní. Můžete nasadit cloudové služby toohello pracovní prostředí tootest ho před zvýšením úrovně je tooproduction. |Visual Studio, služba WebMatrix, Visual Web Developer, FTP, GIT, BitBucket, CodePlex, DropBox, Githubu, Mercurial, sady TFS, webové nasadit, prostředí PowerShell. |
| **Správa a instalace** |Jste zodpovědní za správu úlohy na hello aplikace, data, pravidla brány firewall, virtuální sítě a operačního systému. |Jste zodpovědní za správu úlohy na hello aplikace, data, pravidla brány firewall a virtuální sítě. |Jste zodpovědní za úlohy správy na hello aplikace a pouze data. |
| **Vysoká dostupnost a zotavení po havárii (HADR)** |Doporučujeme umísťovat virtuální počítače v hello stejné dostupnosti nastavení a v hello stejné cloudové služby. Zachovat virtuální počítače stejné hello sady dostupnosti. umožňuje Azure tooplace hello vysokou dostupnost uzly do samostatné selhání domén a domén upgradu. Podobně zachovat virtuální počítače v hello stejné cloudové služby umožňuje Vyrovnávání zatížení a virtuální počítače může komunikovat přímo mezi sebou hello místní síti v rámci datové centrum Azure.<br/><br/>Jste zodpovědní za implementaci vysoké dostupnosti a po havárii řešení obnovení pro SQL Server v Azure Virtual Machines tooavoid žádné výpadky. Podporované HADR technologií, najdete v části [vysokou dostupnost a zotavení po havárii pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md).<br/><br/>Jste zodpovědní za zálohování dat a aplikací.<br/><br/>Azure můžete přesunout virtuální počítače, pokud hello hostitelský počítač v datovém centru hello nezdaří z důvodu toohardware problémy. Kromě toho je možné, plánované výpadky vašeho virtuálního počítače po aktualizaci hello hostitelský počítač pro zabezpečení nebo softwarové aktualizace. Proto doporučujeme udržovat aspoň dva virtuální počítače v jednotlivých vrstvy tooensure hello nepřetržité dostupnosti aplikace. Azure neposkytuje SLA pro jeden virtuální počítač. Další informace najdete v tématu [technické pokyny Azure odolnosti](../../../resiliency/resiliency-technical-guidance.md). |Spravuje Azure hello selhání vyplývající z hello základní hardware nebo operační systém softwaru. Doporučujeme vám, že můžete implementovat několik instancí web nebo worker role tooensure hello vysokou dostupnost vaší aplikace. Informace najdete v tématu [cloudové služby, virtuální počítače a virtuální sítě smlouvy o úrovni služeb](http://www.microsoft.com/download/details.aspx?id=38427) a [zotavení po havárii a vysoká dostupnost pro aplikace Azure](../../../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)<br/><br/>Jste zodpovědní za zálohování dat a aplikací.<br/><br/>Pro databáze, které se nacházejí v databázi systému SQL Server ve virtuálním počítači Azure jste zodpovědní za implementaci vysokou dostupnost a po havárii obnovení řešení tooavoid žádné výpadky. Podporované HDAR technologií najdete v části vysokou dostupnost a zotavení po havárii pro SQL Server v Azure Virtual Machines.<br/><br/>**Zrcadlení databáze serveru SQL**: použití se službou Azure Cloud Services (webové/role pracovního procesu). Virtuálním počítačům systému SQL Server a projekt cloudové služby může být v hello stejné virtuální síti Azure. Pokud virtuální počítač SQL Server není ve stejné virtuální síti text hello, musíte toocreate zástupce serveru SQL tooroute komunikace toohello instance systému SQL Server. Název aliasu hello kromě toho musí odpovídat názvu systému SQL Server hello. |Zajištění vysoké dostupnosti je zděděn z role pracovního procesu systému Azure, Azure blob storage a Azure SQL Database. Například Azure úložiště udržuje tři repliky všech objektů blob, tabulky a fronty data. Současně, Azure SQL Database udržuje tři repliky dat systémem – jedna primární replika a dvě sekundární repliky. Další informace najdete v tématu [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) a [Azure SQL Database](../../../sql-database/sql-database-technical-overview.md).<br/><br/>Při použití SQL serveru ve virtuálním počítači Azure jako zdroj dat pro Azure Web Apps, mějte na paměti, že Azure Web Apps nepodporuje virtuální sítě Azure. Jinými slovy všechna připojení z Azure Web Apps tooSQL serveru virtuálních počítačů v Azure musí projít veřejné koncové body virtuálních počítačů. To může způsobit určitá omezení pro vysokou dostupnost a scénářů zotavení po havárii. Hello klientskou aplikaci v Azure Web Apps připojení tooSQL virtuální počítač Server s zrcadlení databáze například nebude možné tooconnect toohello novým primárním serverem, protože zrcadlení databáze vyžaduje, abyste nastavili Azure Virtual Network mezi virtuálními počítači hostitele systému SQL Server v Azure. Proto pomocí **zrcadlení databáze serveru SQL** s webovými aplikacemi Azure v současné době nepodporují.<br/><br/>**Skupiny dostupnosti AlwaysOn serveru SQL**: skupiny dostupnosti AlwaysOn můžete nastavit při používání Azure Web Apps s virtuálním počítačům systému SQL Server v Azure. Ale musíte tooconfigure naslouchací proces skupiny dostupnosti AlwaysOn tooroute hello komunikace toohello primární repliky prostřednictvím veřejných koncových bodů s vyrovnáváním zatížení. |
| **Připojení mezi různými místy** |Můžete použít místní tooon tooconnect Azure Virtual Network. |Můžete použít místní tooon tooconnect Azure Virtual Network. |Virtuální síť Azure je podporováno. Další informace najdete v tématu [integrace virtuální sítě webové aplikace](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/). |
| **Škálovatelnost** |Škálování je k dispozici zvýšením velikosti virtuálních počítačů hello nebo přidání více disků. Další informace o velikosti virtuálních počítačů najdete v tématu [velikostí virtuálních počítačů pro Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).<br/><br/>**Pro Server databáze**: Škálováním na více systémů je k dispozici prostřednictvím rozdělení techniky databáze a skupiny dostupnosti AlwaysOn serveru SQL Server.<br/><br/>Pro náročných úloh pro čtení, můžete použít [skupiny dostupnosti AlwaysOn](https://msdn.microsoft.com/library/hh510230.aspx) na více sekundární uzly také replikace SQL serveru.<br/><br/>Pro úlohy velkou zápisu můžete implementovat vodorovné rozdělení dat mezi několik fyzických serverů tooprovide aplikace Škálováním na více systémů.<br/><br/>Kromě toho můžete implementovat Škálováním na více systémů pomocí [systému SQL Server se směrováním závislé Data](https://technet.microsoft.com/library/cc966448.aspx). S dat závislé směrování (DDR), budete potřebovat tooimplement hello dělení mechanismus v aplikaci klienta hello, obvykle ve vrstvě vrstvy hello firmy, databáze hello tooroute požadavky toomultiple uzlů serveru SQL. Hello obchodní vrstva obsahuje mapování toohow hello dat je rozdělena na oddíly a který uzel obsahuje hello data.<br/><br/>Je možné škálovat aplikace, které jsou spuštěné virtuální počítače. Další informace najdete v tématu [jak tooScale aplikace](../../../cloud-services/cloud-services-how-to-scale.md).<br/><br/>**Důležitá poznámka**: hello **škálování** funkce v Azure vám umožní tooautomatically zvýšení nebo snížení hello virtuálních počítačů, které jsou používány vaší aplikace. Tato funkce zaručuje, že činnost koncového uživatele hello nemá negativní vliv špičky a jsou virtuální počítače vypnout při nízkém vyžádání hello. Doporučujeme, nenastavujte možnost škálování hello vaší cloudové služby, pokud obsahuje virtuálním počítačům systému SQL Server. Hello důvodem je, že funkci škálování hello umožní Azure tooturn na virtuálním počítači v případě hello využití procesoru v tohoto virtuálního počítače je vyšší než některé prahovou hodnotu a tooturn vypnout virtuální počítač přejde hello využití procesoru nižší než. funkce škálování Hello je užitečná pro bezstavové aplikace, jako jsou třeba webové servery, kde žádné virtuální počítače můžete spravovat hello zatížení bez jakékoli odkazy tooany předchozího stavu. Funkce škálování hello však není užitečné stavových aplikací, jako je SQL Server, kde pouze jedna instance umožňuje zápis toohello databáze. |Škálování je k dispozici v několika webových a pracovních rolí. Další informace o velikosti virtuálních počítačů pro webových rolí a rolí pracovního procesu naleznete v tématu [konfigurace velikosti pro cloudové služby](../../../cloud-services/cloud-services-sizes-specs.md).<br/><br/>Při použití **cloudové služby**, můžete definovat více rolí toodistribute zpracování a také dosáhnout flexibilní škálování aplikace. Jednotlivých cloudových služeb obsahuje jeden nebo více webové role nebo rolí pracovního procesu, každou s vlastní soubory aplikace a konfigurace. Můžete škálování cloudové služby zvýšením hello počet instancí role (virtuálních počítačů), nasazení pro roli a vertikální snížení kapacity Cloudová služba, a tím snižují hello počet instancí role. Podrobné informace najdete v tématu [Azure provádění modely](../../../cloud-services/cloud-services-choose-me.md).<br/><br/>Škálováním na více systémů je k dispozici prostřednictvím podpory Azure integrovanou vysokou dostupnost prostřednictvím [cloudové služby, virtuální počítače a virtuální sítě smlouvy o úrovni služeb](http://www.microsoft.com/download/details.aspx?id=38427) a nástroj pro vyrovnávání zatížení.<br/><br/>Vícevrstvé aplikace doporučujeme připojení serveru aplikací toodatabase web nebo worker role virtuálních počítačů prostřednictvím Azure Virtual Network. Kromě toho Azure poskytuje Vyrovnávání zatížení pro virtuální počítače v hello stejné cloudové služby, šíření požadavky uživatelů mezi nimi. Virtuální počítače připojené tímto způsobem může komunikovat přímo s navzájem hello místní síti v rámci datové centrum Azure.<br/><br/>Můžete nastavit **škálování** na hello portál Azure a také hello plánování doby. Další informace najdete v tématu [jak tooconfigure automatické škálování pro cloudové služby v rámci portálu hello](../../../cloud-services/cloud-services-how-to-scale-portal.md). |**Škálovat nahoru a dolů**: můžete můžete zvětšit nebo zmenšit hello velikost instance hello (VM) vyhrazena pro svůj web.<br/><br/>Horizontální navýšení kapacity: můžete přidat více vyhrazenou instancí (VM) pro svůj web.<br/><br/>Můžete nastavit **škálování** na portál hello, jakož i hello plánování doby. Další informace najdete v tématu [jak tooScale webové aplikace](../../../app-service-web/web-sites-scale.md). |

Další informace o výběru mezi tyto programovací metody naleznete v tématu [Azure Web Apps, cloudové služby a virtuální počítače: když toouse který](../../../app-service-web/choose-web-site-cloud-service-vm.md).

## <a name="next-steps"></a>Další kroky
Další informace o spouštění systému SQL Server v Azure virtuální počítače, naleznete v části [SQL Server na Přehled virtuálních počítačů Azure](virtual-machines-windows-sql-server-iaas-overview.md).

