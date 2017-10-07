---
title: "úlohy aaaWhat si chcete chránit pomocí Azure Site Recovery?"
description: "Azure Site Recovery chrání úlohy a aplikace ve spolupráci hello replikace, převzetí služeb při selhání a obnovení místní virtuální počítače a fyzické servery tooAzure nebo tooa sekundární místní lokalitu"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: cfreeman
editor: 
ms.assetid: 4953948f-26c0-4699-8fe7-59d3bfc1d3da
ms.service: site-recovery
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/08/2017
ms.author: raynew
ms.openlocfilehash: cab2e1ce3c2b7b2c5f899d957219f5c12eb5965c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-workloads-can-you-protect-with-azure-site-recovery"></a>Jaké úlohy je možné chránit pomocí Azure Site Recovery?
Tento článek popisuje úlohy a aplikace, které můžete provádět replikaci s hello služba Azure Site Recovery.

POST jakékoli dotazy nebo připomínky v hello dolní části tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Přehled
Organizace potřebují kontinuity podnikových procesů a po havárii (BCDR) obnovení strategie tookeep úlohy a data bezpečné a dostupné během plánovaných a neplánovaných výpadků a co nejdříve obnovit tooregular pracovní podmínky.

Site Recovery je služba Azure, která přispívá strategie BCDR tooyour. Pomocí Site Recovery, můžete nasadit aplikace s cílem replikace toohello cloud nebo tooa sekundární lokality. Jestli vaše aplikace jsou Windows nebo systémem Linux, běžící na fyzických serverech, VMware nebo Hyper-V, můžete pomocí Site Recovery tooorchestrate replikace, provést testování obnovení po havárii a spustit převzetí služeb při selhání a navrácení služeb po obnovení.

Site Recovery se integruje s aplikacemi Microsoftu, mezi které patří SharePoint, Exchange, Dynamics, SQL Server a Active Directory. Microsoft také úzce spolupracuje s předními dodavateli včetně Oracle, SAP, IBM a Red Hat. Řešení replikace můžete přizpůsobit na bázi jednotlivých aplikací.

## <a name="why-use-site-recovery-for-application-replication"></a>Proč pro replikaci aplikací používat Site Recovery?
Site Recovery přispívá tooapplication úroveň ochrany a obnovení následujícím způsobem:

* Nerozlišování aplikací a poskytování replikace pro jakoukoli úlohu spuštěnou na podporovaném počítači
* Téměř synchronní replikace s rpo stanovené na 30 sekund toomeet hello potřebám nejdůležitějších firemních aplikací.
* Snímky konzistentní s jednovrstvými i vícevrstvými aplikacemi
* Integrace s SQL Server AlwaysOn a partnerství s jinými technologiemi pro replikaci na úrovni aplikací, jako je replikace AD, SQL AlwaysOn, skupiny dostupnosti databáze Exchange (DAG) a Oracle Data Guard
* Flexibilní plány obnovení, které umožňují toorecover celý zásobník aplikací jediným kliknutím a také tooinclude externí skripty a ručně prováděné akce v plánu hello.
* Pokročilá správa sítě v Site Recovery a Azure toosimplify požadavků na aplikační síť, včetně hello možnost tooreserve IP adresy, konfigurovat Vyrovnávání zatížení a integrace Azure Traffic Manager, pro nízkou switchovers RTO sítě.
* Bohatá automatizační knihovna obsahující předpřipravené skripty specifické pro aplikace, které je možné stáhnout a integrovat do plánů obnovení

## <a name="workload-summary"></a>Souhrn úloh
Site Recovery dokáže replikovat jakoukoli aplikaci spuštěnou na podporovaném počítači. Kromě toho jsme ve spolupráci s toocarry týmy produktu se dodatečné testování specifické pro aplikaci.

| **Úloha** | **Replikace virtuálních počítačů Hyper-V tooa sekundární lokality** | **Replikovat tooAzure virtuálních počítačů Hyper-V** | **Replikovat virtuální počítače VMware tooa sekundární lokality** | **Replikovat virtuální počítače VMware tooAzure** |
| --- | --- | --- | --- | --- |
| Active Directory, DNS |Ano |Ano |Ano |Ano |
| Webové aplikace (IIS, SQL) |Ano |Ano |Ano |Ano |
| System Center Operations Manager |Ano |Ano |Ano |Ano |
| SharePoint |Ano |Ano |Ano |Ano |
| SAP<br/><br/>Replikovat SAP tooAzure lokality pro jiný cluster |Ano (testováno Microsoftem) |Ano (testováno Microsoftem) |Ano (testováno Microsoftem) |Ano (testováno Microsoftem) |
| Exchange (ne DAG) |Ano |Ano |Ano |Ano |
| Vzdálená plocha/VDI |Ano |Ano |Ano |– |
| Linux (operační systém a aplikace) |Ano (testováno Microsoftem) |Ano (testováno Microsoftem) |Ano (testováno Microsoftem) |Ano (testováno Microsoftem) |
| Dynamics AX |Ano |Ano |Ano |Ano |
| Dynamics CRM |Ano |Již brzy |Ano |Připravuje se |
| Oracle |Ano (testováno Microsoftem) |Ano (testováno Microsoftem) |Ano (testováno Microsoftem) |Ano (testováno Microsoftem) |
| Souborový server systému Windows |Ano |Ano |Ano |Ano |
| Citrix XenApp a XenDesktop |– |Ano |Není k dispozici |Ano |

## <a name="replicate-active-directory-and-dns"></a>Replikace služby Active Directory a DNS
Infrastruktury služby Active Directory a DNS jsou nezbytné toomost podnikové aplikace. Během zotavení po havárii budete potřebovat tooprotect a obnovit tyto součástí infrastruktury před obnovením aplikací a úloh.

Site Recovery toocreate dokončení automatizované zotavení po havárii můžete použít pro služby Active Directory a DNS. Například pokud chcete toofail přes Sharepointu a SAP ze sekundární lokality primární tooa, můžete nastavit plán obnovení, který nejprve převezme Active Directory, a pak obnovení dodatečné konkrétní aplikaci naplánovat toofail přes hello jiných aplikací, které jsou závislé na aktivní Adresář.

[Zde jsou další informace](site-recovery-active-directory.md) o ochraně Active Directory a DNS.

## <a name="protect-sql-server"></a>Ochrana SQL Serveru
SQL Server poskytuje základnu pro datové služby využívané mnoha firemními aplikacemi v lokálním datovém centru.  Obnovení lokality lze použít společně s technologiemi SQL Server HA/DR, tooprotect vícevrstvých firemních aplikací, které používají systém SQL Server. Site Recovery poskytuje následující:

* Jednoduché a nákladově efektivní řešení zotavení po havárii pro SQL Server; Replikovat různé verze a edice systému SQL Server samostatných serverů a clusterů, tooAzure nebo tooa sekundární lokality.  
* Integrace s skupiny dostupnosti SQL AlwaysOn, toomanage převzetí služeb při selhání a navrácení služeb po obnovení do plánů obnovení Azure Site Recovery.
* Začátku do konce plány obnovení pro hello všechny vrstvy v aplikaci, včetně databází systému SQL Server hello.
* Škálování SQL Serveru podle zatížení ve špičce pomocí Site Recovery, a to rozložením zátěže na větší virtuální počítače IaaS v Azure
* Snadné testování zotavení SQL Serveru po havárii: Můžete spustit test převzetí služeb při selhání tooanalyze data a kontrolovat dodržování předpisů, bez dopadu na produkční prostředí.

[Zde jsou další informace](site-recovery-sql.md) o ochraně SQL Serveru.

## <a name="protect-sharepoint"></a>Ochrana SharePointu
Azure Site Recovery pomáhá chránit nasazení SharePointu následujícím způsobem:

* Eliminuje nutnost hello a náklady na související infrastrukturu pro farmu úsporný režim s připojením k zotavení po havárii. Pomocí Site Recovery tooreplicate celou farmu (webová, aplikační a databázové vrstvy) tooAzure nebo tooa sekundární lokality.
* Zjednodušuje nasazení a správu aplikace. Aktualizace zanesené toohello primární lokality se automaticky replikují a jsou tedy k dispozici po převzetí služeb při selhání a obnovení farmy v sekundární lokalitě. Snižuje se také složitost správy hello a náklady spojené s udržováním záložní farmy aktuální.
* Usnadňuje vývoj a testování aplikací SharePointu díky tomu, že je možné na vyžádání vytvořit repliku produkčního prostředí pro účely testování a ladění.
* Zjednodušuje přechod toohello cloud pomocí Site Recovery toomigrate SharePoint nasazení tooAzure.

[Zde jsou další informace](site-recovery-sharepoint.md) o ochraně SharePointu.

## <a name="protect-dynamics-ax"></a>Ochrana Dynamics AX
Azure Site Recovery zvyšuje ochranu vašeho řešení Dynamics AX ERP tímto způsobem:

* Orchestrace replikace celého prostředí Dynamics AX (webová a AOS vrstva, databázové vrstvy, SharePoint) tooAzure, nebo tooa sekundární lokality.
* Zjednodušení migrace Dynamics AX nasazení toohello cloudu (Azure).
* Usnadnění vývoje a testování aplikací Dynamics AX díky tomu, že je možné na vyžádání vytvořit kopii produkčního prostředí pro účely testování a ladění

[Zde jsou další informace](site-recovery-dynamicsax.md) o ochraně Dynamic AX.

## <a name="protect-rds"></a>Ochrana Vzdálené plochy
Služby Vzdálená plocha (RDS) umožňuje infrastruktury virtuálních klientských (počítačů VDI), klienty na bázi relace a aplikace, umožňuje uživatelům toowork kdekoli. S Azure Site Recovery můžete:

* Replikovat spravované nebo nespravované ve fondu virtuálních ploch tooa sekundární lokality a vzdálené aplikace a relace tooa sekundární lokality nebo Azure.
* Zde je seznam toho, co můžete replikovat:

| **Vzdálená plocha** | **Replikace virtuálních počítačů Hyper-V tooa sekundární lokality** | **Replikovat tooAzure virtuálních počítačů Hyper-V** | **Replikovat virtuální počítače VMware tooa sekundární lokality** | **Replikovat virtuální počítače VMware tooAzure** | **Replikace fyzických serverů tooa sekundární lokality** | **Replikace fyzických serverů tooAzure** |
| --- | --- | --- | --- | --- | --- | --- |
| **Virtuální desktop ve fondu (nespravovaný)** |Ano |Ne |Ano |Ne |Ano |Ne |
| **Virtuální desktop ve fondu (spravovaný a bez UPD)** |Ano |Ne |Ano |Ne |Ano |Ne |
| **Vzdálené aplikace a desktopové relace (bez UPD)** |Ano |Ano |Ano |Ano |Ano |Ano |

[Zde jsou další informace](https://gallery.technet.microsoft.com/Remote-Desktop-DR-Solution-bdf6ddcb) o ochraně Vzdálené plochy.

## <a name="protect-exchange"></a>Ochrana Exchange
Site Recovery pomáhá chránit Exchange následujícím způsobem:

* Pro malá nasazení Exchange, jako jsou třeba servery jedním nebo samostatné Site Recovery můžete replikovat a převzít tooAzure nebo tooa sekundární lokality.
* U větších nasazení se Site Recovery integruje se skupinami dostupnosti databáze (DAG) Exchange.
* Tyto skupiny představují doporučené řešení pro zotavení po havárii serveru Exchange v podniku hello.  Plány obnovení pro obnovení lokality může zahrnovat DAG, tooorchestrate DAG převzetí služeb při selhání mezi servery.

[Zde jsou další informace](https://gallery.technet.microsoft.com/Exchange-DR-Solution-using-11a7dcb6) o ochraně Exchange.

## <a name="protect-sap"></a>Ochrana nasazení SAP
Pomocí Site Recovery tooprotect své nasazení SAP následujícím způsobem:

* Povolení ochrany SAP NetWeaver a NetWeaver produkční aplikací běžících na místních počítačích nastavením replikace tooAzure součásti.
* Povolení ochrany SAP NetWeaver a NetWeaver produkční aplikací běžících Azure, nastavením replikace součásti tooanother datového centra Azure.
* Zjednodušení migrace na cloud pomocí Site Recovery toomigrate vaše tooAzure nasazení SAP.
* Zjednodušení upgradů, testování a vytváření prototypů projektů SAP pomocí vytváření produkčního klonu na vyžádání pro účely testování aplikací SAP.

[Zde jsou další informace](site-recovery-sap.md) o ochraně nasazení SAP.

## <a name="protect-iis"></a>Ochrana IIS
Použijte Site Recovery tooprotect vaše nasazení služby IIS, následujícím způsobem:

Azure Site Recovery poskytuje zotavení po havárii nastavením replikace hello důležité součásti v prostředí tooa studenou vzdálený server nebo veřejného cloudu, jako je například Microsoft Azure. Vzhledem k tomu, že hello virtuální počítač s hello webový server a databáze hello probíhá replikované toohello obnovení lokality, neexistuje žádný požadavek toobackup konfiguračních souborů nebo certifikáty samostatně. Hello mapování aplikací a vazby závisí na proměnné prostředí, které jsou změněné post převzetí služeb při selhání se dá aktualizovat přes skripty integrovat do plánů obnovení po havárii hello. Virtuální počítače se zapínají v lokalitě obnovení hello pouze v případě hello převzetí služeb při selhání. Ne jenom to, Azure Site Recovery také vám pomůže orchestraci hello end tooend převzetí služeb při selhání podle předpokladu, že hello následující možnosti:

-   Sekvencování hello vypnutí a spuštění virtuálních počítačů v hello různých vrstev.
-   Přidávání skriptů tooallow aktualizace závislosti aplikací a vazby hello virtuálními počítači po zahájil. skripty Hello může být také použít tooupdate hello DNS server toopoint toohello obnovení lokality.
-   Přidělte IP adresy toovirtual počítače pre-převzetí služeb při selhání tak, že mapuje hello primárními a obnovovacími sítě a proto použít skripty, které není nutné aktualizovat toobe post převzetí služeb při selhání.
-   Možnost pro selhání jedním kliknutím pro více webových aplikací na webových serverech hello, čímž odstraňuje hello oboru nedorozuměním v události hello havárie.
-   Možnost tootest hello plány obnovení v izolovaném prostředí pro zotavení po Havárii podrobněji.

[Zde jsou další informace](https://aka.ms/asr-iis) o ochraně webové farmy IIS.

## <a name="protect-citrix-xenapp-and-xendesktop"></a>Ochrana pro Citrix XenApp a XenDesktop
Použijte Site Recovery tooprotect vaše nasazení Citrix XenApp a XenDesktop takto:

* Povolení ochrany hello Citrix XenApp a XenDesktop nasazení nastavením replikace různých nasazení vrstvách, včetně (AD DNS server, SQL serveru, Citrix doručení řadiči výkladní skříň serveru, XenApp Master (VDA), Citrix XenApp licenční Server databáze) tooAzure.
* Zjednodušení migrace na cloud, pomocí Site Recovery toomigrate vaše Citrix XenApp a XenDesktop tooAzure nasazení.
* Usnadněte testování pro Citrix XenApp/XenDesktop tak, že vytvoříte na vyžádání kopii produkčního prostředí pro testování a ladění.
* Toto řešení jde použít jenom pro virtuální plochy operačního systému Windows Server, a ne virtuální plochy klienta, protože virtuální plochy klienta se ještě pro licencování v Azure nepodporují.
[Další informace](https://azure.microsoft.com/pricing/licensing-faq/) týkající se licencování pro plochy klienta nebo serveru v Azure

[Další informace](site-recovery-citrix-xenapp-and-xendesktop.md) o ochraně nasazení Citrix XenApp a XenDesktop Alternativně může odkazovat hello [dokument White Paper od společnosti Citrix](https://aka.ms/citrix-xenapp-xendesktop-with-asr) s podrobnostmi o hello stejné.

## <a name="next-steps"></a>Další kroky
[Kontrola požadavků](site-recovery-prereq.md)
