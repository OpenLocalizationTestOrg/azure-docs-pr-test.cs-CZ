---
title: aaaReplicate aplikace s SQL serverem a Azure Site Recovery | Microsoft Docs
description: "Tento článek popisuje, jak tooreplicate systému SQL Server pomocí Azure Site Recovery pro funkce systému SQL Server po havárii."
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: pratshar
ms.openlocfilehash: 99755f2cd2f7e924071f1e230ac4a0bda88f0a39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="protect-sql-server-using-sql-server-disaster-recovery-and-azure-site-recovery"></a>Ochrana systému SQL Server pomocí zotavení po havárii serveru SQL a Azure Site Recovery

Tento článek popisuje, jak tooprotect hello systému SQL Server back-end aplikace pomocí kombinace kontinuity podnikových procesů serveru SQL Server a po havárii (BCDR) obnovení technologií, a [Azure Site Recovery](site-recovery-overview.md).

Než začnete, ujistěte se, že rozumíte možnosti obnovení po havárii systému SQL Server, včetně clustering převzetí služeb při selhání skupiny dostupnosti Always On, zrcadlení databáze a přesouvání protokolu.


## <a name="sql-server-deployments"></a>Nasazení systému SQL Server

Řadu úloh použít jako základ systém SQL Server a lze ji integrovat s aplikacemi, jako je například SharePoint, Dynamics a SAP, tooimplement datové služby.  SQL Server se dá nasadit v mnoha různými způsoby:

* **Samostatný Server SQL**: SQL Server a všechny databáze jsou hostované na jednom počítači (fyzický nebo virtuální). Když Virtualizovat, clustering hostitele se používá pro místní vysokou dostupnost. Vysoká dostupnost úrovni hosta nebyla implementována.
* **Instance systému SQL Server Failover Clustering (vždy na FCI)**: dva nebo víc uzlů systémem SQL Server instance s sdílené disky jsou nakonfigurované v clusteru s podporou převzetí služeb při selhání systému Windows. Pokud uzel je vypnutý, hello clusteru můžete převzetí služeb při selhání systému SQL Server tooanother instance. Tato instalace je obvykle používanými tooimplement vysoké dostupnosti v primární lokalitě. Toto nasazení nebude chránit proti selhání nebo výpadek ve vrstvě hello sdílené úložiště. Sdílený disk může být implementovaná pomocí iSCSI, fiber channel nebo sdílené vhdx.
* **SQL skupin dostupnosti Always On**: dva nebo víc uzlů se nastavují v sdílené nic cluster s databází systému SQL Server, které jsou nakonfigurované ve skupině dostupnosti s synchronní replikace a automatické převzetí služeb při selhání.

 Tento článek využívá hello následující nativní technologiemi pro zotavení po havárii SQL pro obnovení databází tooa vzdálené lokality:

* SQL skupin dostupnosti Always On, tooprovide pro zotavení po havárii pro SQL Server 2012 nebo 2014 Enterprise Edition.
* SQL zrcadlení databáze v režimu vysoké zabezpečení, pro SQL Server Standard edition (všechny verze), nebo pro SQL Server 2008 R2.

## <a name="site-recovery-support"></a>Podpora pro obnovení lokality

### <a name="supported-scenarios"></a>Podporované scénáře
Site Recovery může chránit SQL Server, jak je shrnuto v tabulce hello.

**Scénář** | **tooa sekundární lokality** | **tooAzure**
--- | --- | ---
**Hyper-V** | Ano | Ano
**VMware** | Ano | Ano
**Fyzický server** | Ano | Ano

### <a name="supported-sql-server-versions"></a>Podporované verze systému SQL Server
Tyto verze systému SQL Server jsou podporovány pro hello Podporované scénáře:

* SQL Server 2016 Enterprise a Standard
* SQL Server 2014 Enterprise a Standard
* SQL Server 2012 Enterprise a Standard
* SQL Server 2008 R2 Enterprise a Standard

### <a name="supported-sql-server-integration"></a>Podporované integrace systému SQL Server

Obnovení lokality lze integrovat s nativní SQL serveru BCDR technologie shrnuté v tabulce hello, tooprovide řešení zotavení po havárii.

**Funkce** | **Podrobnosti** | **SQL Server** |
--- | --- | ---
**Skupiny dostupnosti Always On** | Více samostatných instancí systému SQL Server spustit v clusteru s podporou převzetí služeb při selhání, který má více uzly.<br/><br/>Databáze je možné seskupit do skupin převzetí služeb při selhání, které je možné zkopírovat (zrcadlení) na instance systému SQL Server tak, že je potřeba žádné sdílené úložiště.<br/><br/>Poskytuje zotavení po havárii mezi primární lokalitou a jeden nebo více sekundárních lokalit. Dva uzly lze nastavit v sdílenou nic cluster s databází serveru SQL Server nakonfigurován ve skupině dostupnosti s synchronní replikace a automatické převzetí služeb při selhání. | SQL Server 2014 & 2012 Enterprise edition
**Převzetí služeb clusteringu (vždy na FCI)** | SQL Server využívá Windows převzetí služeb při selhání clusteringu pro vysokou dostupnost úloh, místní systém SQL Server.<br/><br/>Instance systému SQL Server s sdílené disky uzly konfigurované v clusteru s podporou převzetí služeb při selhání. Pokud instance je mimo provoz clusteru hello převezme toodifferent jeden.<br/><br/>Hello clusteru nepodporuje ochranu proti výpadkům ve sdíleném úložišti nebo selhání. Hello sdílený disk může být implementováno s rozhraní iSCSI, fiber channel, nebo sdílené soubory Vhdx. | SQL Server Enterprise Edition<br/><br/>SQL Server Standard edition (pouze omezený tootwo uzlů)
**Databáze zrcadlení (vysokou bezpečnost režim)** | Chrání jeden tooa jedné sekundární kopii databáze. K dispozici v obou vysokou bezpečnost (synchronní) a vysoký výkon (asynchronní) replikaci režimy. Nevyžaduje clusteru s podporou převzetí služeb při selhání. | SQL Server 2008 R2<br/><br/>SQL Server Enterprise všechny edice
**Samostatný systém SQL Server** | Hello systému SQL Server a databáze jsou hostované na jednom serveru (fyzické nebo virtuální). Pokud je virtuální hello server, použije se hostitele clustering pro vysokou dostupnost. Žádné úrovni hosta vysokou dostupnost. | Standard nebo Enterprise edition

## <a name="deployment-recommendations"></a>Doporučení pro nasazení

Tato tabulka shrnuje Naše doporučení pro integraci technologiemi BCDR SQL serveru pomocí Site Recovery.

| **Verze** | **Edice** | **Nasazení** | **Místní tooon místní** | **Místní tooAzure** |
| --- | --- | --- | --- | --- |
| SQL Server 2014 nebo 2012 |Enterprise |Instance clusteru převzetí služeb při selhání |Skupiny dostupnosti Always On |Skupiny dostupnosti Always On |
|| Enterprise |Always On skupin dostupnosti pro zajištění vysoké dostupnosti |Skupiny dostupnosti Always On |Skupiny dostupnosti Always On | |
|| Standard |Instance clusteru převzetí služeb při selhání (FCI) |Replikace obnovení lokality s místní zrcadlení |Replikace obnovení lokality s místní zrcadlení | |
|| Standard nebo Enterprise |Standalone |Replikace obnovení lokality |Replikace obnovení lokality | |
| SQL Server 2008 R2 nebo 2008 |Standard nebo Enterprise |Instance clusteru převzetí služeb při selhání (FCI) |Replikace obnovení lokality s místní zrcadlení |Replikace obnovení lokality s místní zrcadlení |
|| Standard nebo Enterprise |Standalone |Replikace obnovení lokality |Replikace obnovení lokality | |
| SQL Server (všechny verze) |Standard nebo Enterprise |Instance clusteru převzetí služeb při selhání - DTC aplikace |Replikace obnovení lokality |Nepodporuje se |

## <a name="deployment-prerequisites"></a>Požadavky nasazení

* Místní nasazení systému SQL Server, spuštěna podporovaná verze systému SQL Server. Obvykle je také nutné služby Active Directory pro SQL server.
* požadavky Hello hello scénáři chcete toodeploy. Další informace o požadavcích na podporu pro [replikace tooAzure](site-recovery-support-matrix-to-azure.md) a [místní](site-recovery-support-matrix.md), a [požadavky nasazení](site-recovery-prereq.md).
* tooset až obnovení v Azure, spusťte hello [posouzení připravenosti na virtuální počítač Azure](http://www.microsoft.com/download/details.aspx?id=40898) nástroj na virtuální počítače systému SQL Server, toomake, že jsou kompatibilní s Azure a Site Recovery.

## <a name="set-up-active-directory"></a>Nastavení služby Active Directory

Nastavení služby Active Directory v hello obnovení sekundární lokality, pro SQL Server toorun správně.

* **Malý podnik**– s malý počet aplikací a jeden řadič domény pro hello místní lokalitu, pokud chcete toofail přes hello celý web, doporučujeme používat Site Recovery replikace tooreplicate hello řadič domény toohello sekundárního datacentra, nebo tooAzure.
* **Střední toolarge enterprise**– Pokud máte velký počet aplikací, doménové struktury služby Active Directory a chcete toofail nepřevezme aplikace nebo zatížení, doporučujeme nastavit další řadič domény do sekundárního datacentra hello nebo Azure. Pokud používáte vždy na dostupnosti skupiny toorecover tooa vzdálené lokality, doporučujeme že nastavit jiný další řadič domény v sekundární lokalitě hello nebo v Azure, toouse pro instanci systému SQL Server hello obnovit.

Hello pokyny v tomto článku předpokládá, že je řadič domény k dispozici v hello sekundárního umístění. [Další informace](site-recovery-active-directory.md) o ochraně Active Directory pomocí Site Recovery.


## <a name="integrate-with-sql-server-always-on-for-replication-tooazure"></a>Integraci se službou SQL serveru Always On pro tooAzure replikace

Zde je budete potřebovat toodo:

1. Skripty importovat do účtu Azure Automation. Tato položka obsahuje hello skripty toofailover skupiny dostupnosti SQL v [správce prostředků virtuálního počítače](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/asr-automation-recovery/scripts/ASR-SQL-FailoverAG.ps1) a [klasické virtuální počítač](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/asr-automation-recovery/scripts/ASR-SQL-FailoverAGClassic.ps1).

    [![Nasazení tooAzure](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)


1. Přidání automatické obnovení systému-SQL-FailoverAG jako akce před první skupiny hello hello plánu obnovení.

1. Postupujte podle pokynů hello k dispozici v hello skriptu toocreate název automatizace proměnné tooprovide hello skupin dostupnosti hello.

### <a name="steps-toodo-a-test-failover"></a>Kroky toodo testovací převzetí služeb

SQL Always On nenabízí nativní podporu převzetí služeb při selhání. Proto doporučujeme hello následující:

1. Nastavit [Azure Backup](../backup/backup-azure-vms.md) hello virtuálního počítače, který je hostitelem repliky skupiny dostupnosti hello v Azure.

1. Před spuštěním testu převzetí služeb při selhání hello plánu obnovení, obnovení ze zálohy hello prováděné v předchozím kroku hello hello virtuálního počítače.

    ![Obnovit ze zálohy Azure ](./media/site-recovery-sql/restore-from-backup.png)

1. [Vynucení kvora](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/force-a-wsfc-cluster-to-start-without-a-quorum#PowerShellProcedure) ve virtuálním počítači hello obnovena ze zálohy.

1. Aktualizujte IP hello naslouchací proces tooan IP k dispozici v hello testovací převzetí služeb při selhání sítě.

    ![Aktualizovat naslouchací proces IP](./media/site-recovery-sql/update-listener-ip.png)

1. Přepněte online naslouchací proces.

    ![Naslouchací proces převedení do online režimu](./media/site-recovery-sql/bring-listener-online.png)

1. Vytvořte nástroj pro vyrovnávání zatížení s jedna IP adresa vytvořil v rámci front-endovou IP fond odpovídající tooeach naslouchací proces skupiny dostupnosti a virtuální počítač SQL hello přidán do fondu back-end hello.

     ![Vytvořit nástroj pro vyrovnávání zatížení - fondu IP front-endu ](./media/site-recovery-sql/create-load-balancer1.png)

    ![Vytvořit nástroj pro vyrovnávání zatížení - fond back-end ](./media/site-recovery-sql/create-load-balancer2.png)

1. Proveďte testovací převzetí služeb hello plánu obnovení.

### <a name="steps-toodo-a-failover"></a>Kroky toodo převzetí služeb při selhání

Po přidání hello skript v plánu obnovení hello a plán obnovení ověřené hello pomocí tohoto postupu převzetí služeb při selhání, můžete provést převzetí služeb při selhání plánu obnovení hello.


## <a name="integrate-with-sql-server-always-on-for-replication-tooa-secondary-on-premises-site"></a>Integraci se službou SQL serveru Always On pro replikaci tooa sekundární místní lokalitu

Pokud hello systému SQL Server nepoužívá skupiny dostupnosti pro vysokou dostupnost (nebo FCI), doporučujeme, abyste na webovém serveru obnovení hello také použití skupin dostupnosti. Všimněte si, že platí tooapps, která nepoužívají distribuované transakce.

1. [Konfigurovat databáze](https://msdn.microsoft.com/library/hh213078.aspx) do skupiny dostupnosti.
1. Vytvoření virtuální sítě na sekundární lokalitě hello.
1. Nastavte připojení site-to-site VPN mezi hello virtuální sítě a hello primární lokality.
1. Vytvoření virtuálního počítače na hello obnovení lokality a na něj nainstalovat SQL Server.
1. Rozšíření hello existující vždy na dostupnosti skupiny toohello nový virtuální počítač SQL Server. Konfiguraci této instance systému SQL Server jako asynchronní repliky kopie.
1. Vytvořit naslouchací proces skupiny dostupnosti nebo aktualizovat hello existující naslouchací proces tooinclude hello asynchronní virtuální počítač repliky.
1. Zajistěte, aby že příslušné farmy aplikace hello se nastavuje pomocí hello naslouchací proces. Pokud je instalační program díky hello název databázového serveru, aktualizovat naslouchací proces toouse hello, takže není nutné tooreconfigure ho po převzetí služeb při selhání hello.

Pro aplikace, které používají distribuované transakce, doporučujeme nasadit Site Recovery s [VMware nebo fyzický server site-to-site replikace](site-recovery-vmware-to-vmware.md).

### <a name="recovery-plan-considerations"></a>Důležité informace o plánu obnovení
1. Tento ukázkový skript toohello knihovny VMM, přidejte na hello primárních a sekundárních lokalit.

        Param(
        [string]$SQLAvailabilityGroupPath
        )
        import-module sqlps
        Switch-SqlAvailabilityGroup -Path $SQLAvailabilityGroupPath -AllowDataLoss -force

1. Když vytvoříte plán obnovení pro aplikace hello, přidejte před akce tooGroup-1 skriptované krok, vyvolávající hello skriptu toofail přes skupiny dostupnosti.

## <a name="protect-a-standalone-sql-server"></a>Chránit samostatný systém SQL Server

V tomto scénáři doporučujeme použít počítači serveru SQL Server hello tooprotect replikace Site Recovery. přesný postup Hello záviset jestli je SQL Server virtuálním počítači nebo fyzickém serveru a jestli chcete, aby tooreplicate tooAzure nebo sekundární místní lokalitu. Další informace o [scénáře obnovení lokality](site-recovery-overview.md).

## <a name="protect-a-sql-server-cluster-standard-editionwindows-server-2008-r2"></a>Ochraně clusteru SQL serveru (standard edition nebo Windows Server 2008 R2)

U clusteru se systémem SQL Server Standard edition nebo SQL Server 2008 R2 doporučujeme že používat Site Recovery tooprotect replikace systému SQL Server.

### <a name="on-premises-tooon-premises"></a>Místní tooon místní

* Pokud aplikace hello používá distribuované transakce doporučujeme nasadíte [Site Recovery s replikací sítě SAN](site-recovery-vmm-san.md) pro prostředí Hyper-V, nebo [VMware nebo fyzický server tooVMware](site-recovery-vmware-to-vmware.md) pro prostředí VMware.
* Pro aplikace bez DTC použijte hello výše přístup toorecover hello clusteru jako samostatný server s využitím místní vysokou bezpečnost zrcadlení databáze.

### <a name="on-premises-tooazure"></a>Místní tooAzure

Site Recovery neposkytuje hostovaného clusteru podporu při replikaci tooAzure. SQL Server také neposkytuje řešení pro zotavení po havárii nízkonákladové pro Standard edition. V tomto scénáři doporučujeme chránit hello místní systém SQL Server cluster tooa samostatný systém SQL Server a obnovit v Azure.

1. Nakonfigurujte další samostatné instance systému SQL Server na hello místního webu.
1. Konfigurace hello instance tooserve jako zrcadlení pro hello databáze, které chcete tooprotect. Konfigurace zrcadlení v režimu vysokou bezpečnost.
1. Konfigurace Site Recovery na hello na místní lokalitu, pro ([technologie Hyper-V](site-recovery-hyper-v-site-to-azure.md) nebo [virtuální počítače VMware nebo fyzické servery)](site-recovery-vmware-to-azure-classic.md).
1. Pomocí Site Recovery replikaci tooreplicate hello nového systému SQL Server instance tooAzure. Vzhledem k tomu, že je zrcadlení kopie vysokou bezpečnost, bude synchronizován s hello primární clusteru, ale bude replikované tooAzure pomocí Site Recovery replikace.


![Cluster Standard](./media/site-recovery-sql/standalone-cluster-local.png)

### <a name="failback-considerations"></a>Důležité informace o navrácení služeb po obnovení

Navrácení služeb po obnovení po neplánovaném převzetí služeb při selhání pro SQL Server Standard clusterů, vyžaduje zálohování systému SQL server a obnovení ze hello zrcadlení instance toohello původní cluster s reestablishment hello zrcadlení.

## <a name="next-steps"></a>Další kroky
[Další informace](site-recovery-components.md) o architektuře Site Recovery.
