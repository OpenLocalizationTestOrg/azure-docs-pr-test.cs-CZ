---
title: "aaaSQL (PaaS) Database vs. SQL Server v cloudu hello na virtuálních počítačích (IaaS) | Microsoft Docs"
description: "Další informace, které Cloudová možnost SQL serveru nejlépe odpovídá vaší aplikaci: Azure SQL (PaaS) Database nebo SQL Server v cloudu hello ve virtuálních počítačích Azure."
services: sql-database, virtual-machines
keywords: "Cloudu systému SQL Server, SQL Server v cloudu hello, databáze PaaS cloudu systému SQL Server, DBaaS"
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: cjgronlund
ms.assetid: 7467f422-b77d-4b60-9cb5-0f1ec17ec565
ms.service: sql-database
ms.custom: DBs & servers
ms.workload: data-management
ms.tgt_pltfrm: vm-windows-sql-server
ms.devlang: na
ms.topic: article
ms.date: 02/01/2017
ms.author: carlrab
ms.openlocfilehash: 1b462a9a822d04dc5deb8422ed505a5d09279253
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="choose-a-cloud-sql-server-option-azure-sql-paas-database-or-sql-server-on-azure-vms-iaas"></a>Volba cloudového řešení systému SQL Server: Azure SQL (PaaS) Database nebo SQL Server na virtuálních počítačích Azure (IaaS)
Azure nabízí pro hostování úloh SQL Serveru v Microsoft Azure dvě možnosti:

* [Azure SQL Database](https://azure.microsoft.com/services/sql-database/): A SQL databáze nativní toohello cloudu, také známé jako platforma jako služba (PaaS) database nebo databáze jako služba (DBaaS), která je optimalizovaná pro vývoj aplikací pro software jako služba (SaaS). Nabízí kompatibilitu s většinou funkcí systému SQL Server. Další informace o modelu PaaS najdete v tématu [Co je PaaS](https://azure.microsoft.com/overview/what-is-paas/).
* [SQL Server na virtuálních počítačích Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/): SQL Server nainstalovaný a hostovaný v cloudu hello na Windows Server virtuálních počítačů (VM) spuštěné v Azure, také označované jako infrastruktura jako služba (IaaS).
  SQL Server na virtuálních počítačích Azure je optimalizovaný pro migraci stávajících aplikací systému SQL Server. Všechny hello verze a edice systému SQL Server jsou k dispozici. Nabízí 100 % kompatibilitu s SQL serverem, povolení toohost počet databází jako potřebné a provádění mezidatabázové transakce. Nabízí úplné řízení systému SQL Server a Windows.

Zjistěte, jak jednotlivé možnosti zapadá do datová platforma Microsoft hello a získání nápovědy odpovídající hello správné volby tooyour podnikové požadavky. Jestli určit prioritu úspora nákladů nebo minimální správu před všechno ostatní, tento článek vám může pomoci rozhodnout, jaký přístup přináší proti hello obchodní požadavky, že vám nejvíc záleží.

## <a name="microsofts-data-platform"></a>Datová platforma společnosti Microsoft
Jedním z hello první věcí toounderstand všech diskusí Azure a místní databáze SQL serveru je, že můžete použít všechny. Datová platforma společnosti Microsoft využívá technologii SQL Serveru a zpřístupňuje ji napříč fyzickými místními počítači, prostředími privátního cloudu, prostředími privátního cloudu hostovanými třetí stranou a veřejným cloudem. SQL Server na virtuálních počítačích Azure vám umožní toomeet jedinečný a rozmanitým potřebám firmy pomocí kombinace místní a nasazení hostovaného v cloudu, při použití hello stejnou sadu serverové produkty, nástroje pro vývoj a odborných znalostí mezi tyto prostředí.

   ![Možnosti SQL serveru v cloudu: databáze systému SQL server na IaaS nebo SaaS SQL v cloudu hello.](./media/sql-database-paas-vs-sql-server-iaas/SQLIAAS_SQL_Server_Cloud_Continuum.png)

Jak je vidět v diagramu hello, každou nabídku je možné charakterizovat úrovní hello správy, kterou máte nad infrastrukturou hello (na ose hello X) a hello stupněm nákladové efektivity dosahovaným konsolidací úrovně databáze a automatizace (na ose hello Y).

Při navrhování aplikace jsou k dispozici pro hostování hello systému SQL Server součástí aplikace hello čtyři základní možnosti:

* SQL Server na nevirtualizovaných fyzických počítačích
* SQL Server v místních virtualizovaných počítačích (privátní cloud)
* SQL Server na virtuálním počítači Azure (veřejný cloud Microsoftu)
* Azure SQL Database (veřejný cloud Microsoftu)

V následujících částech hello, můžete další informace o systému SQL Server v hello veřejného cloudu Microsoftu: Azure SQL Database a SQL Server na virtuálních počítačích Azure. Kromě toho prozkoumáte obvyklé motivační faktory firem pro určení toho, která možnost je pro konkrétní aplikaci nejvhodnější.

## <a name="a-closer-look-at-azure-sql-database-and-sql-server-on-azure-vms"></a>Bližší pohled na Azure SQL Database a SQL Server na virtuálních počítačích Azure
**Azure SQL Database** je relační databáze jako služba (DBaaS) hostovaná v cloudu Azure, která spadá do oborových kategorií hello hello *softwaru jako služba (SaaS)* a *platforma jako služba (PaaS)* . [SQL Database](sql-database-technical-overview.md) je postavená na standardizovaném hardwaru a softwaru, který vlastní, hostuje a spravuje Microsoft. S databází SQL provádět vývoj přímo na hello služby pomocí integrovaných funkcí. Při použití SQL Database platíte průběžnými platbami s možností tooscale nebo horizontálně navýšit kapacitu pro dosažení vyššího výkonu bez přerušení.

**SQL Server na virtuálních počítačích Azure (VM)** spadá do oborové kategorie hello *infrastruktury jako služba (IaaS)* a umožňuje vám toorun systému SQL Server uvnitř virtuálního počítače v cloudu hello. Podobně jako tooSQL databáze, je založen na standardizovaném hardwaru, který je ve vlastnictví, hostován a spravován společností Microsoft. Když na virtuálním počítači používáte SQL Server, můžete průběžně platit za licenci systému SQL Server obsaženou v imagi systému SQL Server nebo jednoduše použít stávající licenci. Můžete také snadno hello stupnice – číselník a pozastavení nebo obnovení virtuálního počítače podle potřeby.

Obecně jsou tyto dvě možnosti SQL optimalizovány pro různé účely:

* **Azure SQL Database** je optimalizované tooreduce celkové náklady toohello minimální pro zřizování a správu velkého počtu databází. Protože všechny virtuální počítače, operační systém nebo databázový software nemají toomanage snižuje náklady na průběžnou správu. Nemáte toomanage upgrady, vysokou dostupnost, nebo [zálohování](sql-database-automated-backups.md). Obecně platí, Azure SQL Database může výrazně zvýšit hello počet databází, které spravuje jeden IT nebo vývojářský zdroj.
* **SQL Server běžící na virtuálních počítačích Azure** je optimalizovaná pro migraci stávající tooAzure aplikace nebo rozšíření stávající místní aplikace toohello cloudu v hybridním nasazení. Kromě toho můžete použít systém SQL Server ve virtuálním počítači toodevelop a testovat tradiční aplikace SQL Server. Se systémem SQL Server na virtuálních počítačích Azure máte hello plná práva pro správu přes vyhrazenou instanci systému SQL Server a cloudové virtuální počítače. Je ideální volbou v případě, že organizace už má IT prostředky k dispozici toomaintain hello virtuálních počítačů. Tyto funkce umožňují toobuild vysoce přizpůsobený systém tooaddress vaší aplikace konkrétní požadavky na výkon a dostupnost.

Hello následující tabulka shrnuje hlavní vlastnosti hello SQL Database a SQL Server na virtuálních počítačích Azure:

| **Nejvhodnější pro:** | **Azure SQL Database** | **SQL Server na virtuálním počítači Azure** |
| --- | --- | --- |
|  |Nové aplikace navržené pro cloud, které mají časová omezení z hlediska vývoje a marketingu. |Existující aplikace, které vyžadují rychlou migraci toohello cloudu s minimálními změnami. Rychlý vývoj a testování scénáře, kdy nechcete, aby toobuy místní neprodukční prostředí systému SQL Server hardware. |
|  | Týmy, které potřebují integrovanou vysokou dostupnost a zotavení po havárii, upgradu pro databázi hello. |Týmy, které mohou konfigurovat a spravovat vysokou dostupnost, zotavení po havárii a použití dílčích oprav systému SQL Server. Některé poskytované automatizované funkce to značně zjednodušují. | |
|  | Týmy, které nechcete, aby toomanage hello příslušný operační systém a nastavení konfigurace. |Budete potřebovat přizpůsobené prostředí s plná práva pro správu. | |
|  | Databáze až too4 TB nebo větší databáze, které může být [vodorovně nebo svisle na oddíly](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling) pomocí vzoru Škálováním na více systémů. |Instance systému SQL Server s až too64 TB úložiště. Hello instance může podporovat tolik databáze podle potřeby. | |
|  | [Sestavování aplikací s modelem SaaS (Software-as-a-Service)](sql-database-design-patterns-multi-tenancy-saas-applications.md). |Migrace a sestavování podnikových a hybridních aplikací. | |
|  | | |
| **Zdroje a prostředky:** |Nechcete tooemploy IT prostředky pro konfiguraci a správu hello základní infrastrukturu, ale chcete toofocus na aplikační vrstvu hello. |Máte některé prostředky IT ke konfiguraci a správě. Některé poskytované automatizované funkce to značně zjednodušují. |
| **Celkové náklady na vlastnictví:** |Eliminuje náklady na hardware a snižuje náklady na správu. |Eliminuje náklady na hardware. |
| **Kontinuita podnikových procesů:** |V možnosti přidání toobuilt v odolnost proti chybám infrastruktury, Azure SQL Database poskytuje funkce, jako například [automatizované zálohování](sql-database-automated-backups.md), [v daném okamžiku obnovení](sql-database-recovery-using-backups.md#point-in-time-restore), [geografické obnovení](sql-database-recovery-using-backups.md#geo-restore), a [aktivní geografickou replikací](sql-database-geo-replication-overview.md) tooincrease kontinuity podnikových procesů. Další informace najdete v tématu [Databáze SQL – kontinuita podnikových procesů (přehled)](sql-database-business-continuity.md). |SQL Server na virtuálních počítačích Azure umožňuje nastavit vysoce dostupné řešení s možností zotavení po havárii pro konkrétní potřeby vaší databáze. Můžete tak mít systém, který je vysoce optimalizovaný pro vaši aplikaci. Sami podle potřeby můžete otestovat a spustit převzetí služeb při selhání. Další informace najdete v tématu [Vysoká dostupnost a zotavení po havárii pro SQL Server v Azure Virtual Machines](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md). |
| **Hybridní cloud:** |Vaše místní aplikace mohou přistupovat k datům v Azure SQL Database. |Se systémem SQL Server na virtuálních počítačích Azure, můžete máte aplikace, které běží částečně v cloudu hello a částečně místně. Například můžete rozšířit místní síť a doména služby Active Directory toohello cloudu pomocí [Azure Virtual Network](../virtual-network/virtual-networks-overview.md). Kromě toho můžete uložit místní datové soubory v úložišti Azure pomocí [datových souborů SQL Serveru v Azure](http://msdn.microsoft.com/library/dn385720.aspx). Další informace najdete v tématu [Úvod tooSQL serveru 2014 hybridní Cloud](http://msdn.microsoft.com/library/dn606154.aspx). |
|  | Podporuje [transakční replikace systému SQL Server](https://msdn.microsoft.com/library/mt589530.aspx) jako datový tooreplicate odběratele. |Plně podporuje [transakční replikace systému SQL Server](https://msdn.microsoft.com/library/mt589530.aspx), [skupiny dostupnosti AlwaysOn](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md), Integration Services a data tooreplicate přesouvání protokolu. Navíc jsou plně podporované tradiční zálohy systému SQL Server. | |
|  | | |

## <a name="business-motivations-for-choosing-azure-sql-database-or-sql-server-on-azure-vms"></a>Motivace firem pro zvolení Azure SQL Database nebo SQL Serveru na virtuálních počítačích Azure
### <a name="cost"></a>Náklady
Jestli jste začínající společnost peněžních, která nebo tým v zavedené společnosti, který pracuje s limitovaným rozpočtem, omezená výše prostředků je často hello primární ovladačů při rozhodování o tom, jak toohost vaší databáze. V této části se dozvíte o hello fakturace a licencování v Azure s jde o toothese dvě volby relačních databází: SQL Database a SQL Server na virtuálních počítačích Azure. Můžete také informace o výpočtu hello celkové náklady na aplikaci.

#### <a name="billing-and-licensing-basics"></a>Základy fakturace a licencování
**Databáze SQL** toocustomers se prodává jako služba, není s licencí.  [SQL Server na virtuálních počítačích Azure](../virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md) je prodáván se zahrnutou licencí, za kterou platíte po minutách. Máte-li stávající licenci, můžete ji také použít.  

V současné době **SQL Database** je k dispozici v několika úrovně služeb, všechny z nich se účtují HODINOVĚ s pevnou sazbou na základě hello služby a výkonu zvolíte. Kromě toho se vám účtuje odchozí přenos přes internet podle běžných [sazeb za přenos dat](https://azure.microsoft.com/pricing/details/data-transfers/). Hello Basic, Standard a Premium a služby Premium RS úrovně jsou navrženy toodeliver předvídatelného výkonu s více toomatch úrovně výkonu požadavky na aplikace ve špičce. Můžete změnit mezi úrovně služeb a toomatch úrovně výkonu, musí rozmanitých propustnost vaší aplikace. Pokud vaše databáze má vysokou transakční svazek a musí toosupport více souběžných uživatelů, doporučujeme úroveň služeb Premium hello. Hello nejnovější informace o úrovních služeb hello aktuální podporovány, naleznete v části [úrovních služby databáze SQL Azure](sql-database-service-tiers.md). Můžete také vytvořit [elastické fondy](sql-database-elastic-pool.md) tooshare výkonu prostředků mezi instancí databáze.

S **SQL Database**, hello databázový software je automaticky nakonfigurované, opravit a upgradovat společností Microsoft, což snižuje náklady na správu. Kromě toho vám [integrované funkce zálohování](sql-database-automated-backups.md) pomůžou dosáhnout výrazných úspor nákladů, hlavně v případě, že máte velký počet databází.

S **systému SQL Server na virtuálních počítačích Azure**, můžete použít některou z hello poskytovanou platformou imagí SQL serveru (které zahrnuje licenci) nebo přepněte licenci SQL Server. Hello všechny podporované verze systému SQL Server (2008 R2, 2012, 2014, 2016) a edice (Developer, Express, Web, Standard, Enterprise) jsou k dispozici. Kromě toho jsou dostupné verze Bring-vaše-vlastníte-licence (BYOL) hello bitových kopií. Pokud používáte hello Azure poskytuje bitové kopie, hello provozní náklady záviset na hello velikost virtuálního počítače a hello edici systému SQL Server, které zvolíte. Bez ohledu na velikost virtuálního počítače nebo edici SQL serveru platíte za minutu licenční náklady na SQL Server a Windows Server, společně s hello náklady na úložiště Azure pro hello disky virtuálních počítačů. možnost fakturace za minutu Hello umožňuje toouse systému SQL Server dlouho bez přidání Nákup licencí systému SQL Server. Pokud jste přineste si vlastní tooAzure licencí systému SQL Server, budou se vám účtovat pro Windows Server a pouze náklady na úložiště. Další informace o používání vlastní licence (BYOL) najdete v tématu [Mobilita licencí v rámci programu Software Assurance na Azure](https://azure.microsoft.com/pricing/license-mobility/).

#### <a name="calculating-hello-total-application-cost"></a>Výpočet hello celkové náklady na aplikaci
Když začnete používat cloudovou platformu, hello náklady na provozování vaší aplikace obsahuje hello vývoj a náklady na správu a také náklady služby platformy hello veřejného cloudu.

Zde je podrobný je výpočet nákladů na vaši aplikaci běžící v SQL Database a SQL Server na virtuálních počítačích Azure hello:

**Při používání Azure SQL Database:**

*Celkové náklady na aplikaci = vysoce minimalizované náklady na správu + náklady na vývoj softwaru + náklady na službu SQL Database*

**Při používání SQL Serveru na virtuálních počítačích Azure:**

*Celkové náklady na aplikaci = minimalizované náklady na vývoj softwaru + náklady na správu + náklady na licence systému SQL Server a Windows Server + náklady na službu Azure Storage*

Další informace o cenách najdete v tématu hello následující prostředky:

* [SQL Database – ceny](https://azure.microsoft.com/pricing/details/sql-database/)
* [Virtual Machines – ceny](https://azure.microsoft.com/pricing/details/virtual-machines/) pro [SQL](https://azure.microsoft.com/pricing/details/virtual-machines/#sql) a [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/#windows)
* [Cenová kalkulačka funkcí Azure](https://azure.microsoft.com/pricing/calculator/)

> [!NOTE]
> SQL Server má malou podmnožinu funkcí, které nejsou platné nebo dostupné při používání SQL Database. Další informace najdete v tématech [Funkce služby SQL Database](sql-database-features.md) a [Informace o jazyce Transact-SQL služby SQL Database](sql-database-transact-sql-information.md). Pokud přesouváte ve stávající cloudu toohello řešení systému SQL Server, přečtěte si téma [migrace tooAzure databáze systému SQL Server databáze SQL](sql-database-cloud-migrate.md). Když provádíte migraci existující místní systém SQL Server aplikace tooSQL databáze, zvažte aktualizaci hello aplikace tootake výhod hello možnosti, které nabízejí cloudové služby. Například byste mohli zvážit použití [Azure Web App Service](https://azure.microsoft.com/services/app-service/web/) nebo [Azure Cloud Services](https://azure.microsoft.com/services/cloud-services/) toohost tooincrease vrstvy vaší aplikace náklady výhody.
> 
> 

### <a name="administration"></a>Správa
Pro mnoho firem hello rozhodnutí tootransition tooa Cloudová služba je jako o snížení složitosti správy, jako je mnohem je náklady. S **SQL Database**, Microsoft spravuje hello základní hardware. Microsoft automaticky replikuje všechna data tooprovide vysoké dostupnosti, konfiguruje a upgraduje hello databázový software, spravuje Vyrovnávání zatížení a nemá transparentní převzetí služeb při selhání, pokud dojde k selhání serveru. Můžete pokračovat tooadminister vaší databáze, ale již nepotřebujete toomanage hello databázový stroj, serverový operační systém nebo hardwaru.  Příklady položek můžete dál tooadminister zahrnují databází a přihlášení, index a ladění dotazu a auditování a zabezpečení.

S **systému SQL Server na virtuálních počítačích Azure**, máte plnou kontrolu nad hello operačního systému a konfigurace instance systému SQL Server. V případě virtuálních počítačů, je to tooyou toodecide při hello tooupdate nebo upgradovat operační systém a databáze softwaru a při tooinstall žádný další software, jako je antivirový. Některé automatizované funkce jsou k dispozici toodramatically zjednodušit oprav, zálohování a vysoké dostupnosti. Kromě toho můžete řídit hello velikost hello virtuálních počítačů, hello počet disků a jejich konfigurace úložiště. Azure vám umožní toochange hello velikost virtuálního počítače podle potřeby. Informace najdete v tématu věnovaném [velikostem virtuálních počítačů a cloudových služeb pro Azure](../virtual-machines/windows/sizes.md). 

### <a name="service-level-agreement-sla"></a>Smlouvy o úrovni služeb (SLA)
Pro řadu IT oddělení je nejvyšší prioritou plnit povinnosti z hlediska garantované doby provozuschopnosti vyplývající ze Smlouvy o úrovni služeb (SLA). V této části se podíváme na co smlouva SLA se vztahuje tooeach z možností hostování databáze.

Pro **SQL Database** s cenovou úrovní služeb Basic, Standard, Premium a Premium RS Premium poskytuje Microsoft smlouvu SLA s dostupností 99,99 %. Hello nejnovější informace najdete v tématu [smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/sql-database/). Hello nejnovější informace o úrovních služby databáze SQL a plánech kontinuity podnikových procesů hello podporovány, naleznete v části [úrovně služeb](sql-database-service-tiers.md).

Pro **SQL Server běžící na virtuálních počítačích Azure**, společnost Microsoft poskytuje 99,95 % smlouvu SLA s dostupností zahrnuje právě hello virtuálního počítače. Tato smlouva SLA nepokrývá procesy hello (například SQL Server) systémem hello virtuálních počítačů a vyžaduje, abyste aspoň dvě instance virtuálních počítačů v nastavení dostupnosti. Hello nejnovější informace najdete v tématu hello [SLA k Virtuálním počítačům](https://azure.microsoft.com/support/legal/sla/virtual-machines/). Pro databáze vysokou dostupnost (HA) v rámci virtuálních počítačů, měli byste nakonfigurovat jednu z možností vysoké dostupnosti hello podporované v systému SQL Server, jako [skupiny dostupnosti AlwaysOn](http://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx). Pomocí možnosti podporované vysokou dostupnost neposkytuje další SLA, ale můžete tooachieve > 99,99 % dostupnost databáze.

### <a name="market"></a>Čas toomarket
**Databáze SQL** je hello správným řešením pro aplikace navržené pro cloud, když je důležité produktivita vývojářů a rychlé čas na trh. Pomocí funkce programových funkcí podobných DBA je ideální pro cloudové architekty a vývojáře protože snižuje potřebu hello správy hello příslušný operační systém a databáze. Například můžete použít hello [REST API](http://msdn.microsoft.com/library/azure/dn505719.aspx) a [rutiny prostředí PowerShell](http://msdn.microsoft.com/library/mt740629.aspx) tooautomate a spravovat operace správy pro tisíce databází. Funkce, jako [elastické fondy](sql-database-elastic-pool.md) umožňují toofocus na aplikační vrstvu hello a poskytovat rychlejší trhu toohello vaše řešení.

**SQL Server běžící na virtuálních počítačích Azure** je ideální, pokud existující nebo nové aplikace vyžadují velké databáze, vzájemně souvisejících databáze, nebo získat přístup k funkcím tooall v systému SQL Server nebo Windows. Je také vhodné, pokud chcete, aby toomigrate stávající místní aplikace a databáze tooAzure jako-je. Vzhledem k tomu, že není nutné toochange hello prezentace, aplikace a datové vrstvy, ušetříte čas a nároky na vynaložit existující řešení. Místo toho můžete soustředit na migraci všech řešení tooAzure a provedení některých optimalizací výkonu, které můžou vyžadovat hello platformy Azure. Další informace najdete v tématu [Osvědčené postupy z hlediska výkonu pro SQL Server ve službě Azure Virtual Machines](../virtual-machines/windows/sql/virtual-machines-windows-sql-performance.md).

## <a name="summary"></a>Souhrn
Tento článek se věnoval SQL Database a SQL Serveru na virtuálních počítačích Azure a běžným motivačním faktorům firem, které by mohly mít vliv na vaše rozhodnutí. Hello Následuje souhrn návrhů pro tooconsider můžete:

**Databázi SQL Azure** zvolte, pokud:

* Jste vytváření nové cloudové aplikace poskytují tootake využít hello náklady na výkon a úspory optimalizace, které cloudové služby. Tento přístup přináší výhody hello plně spravované cloudové služby, pomáhá nižší počáteční čas na trh a může poskytnout dlouhodobou optimalizaci nákladů.
* Chcete-li toohave Microsoft prováděl běžné operace správy vašich databází a vyžadovat pro databáze smlouvy SLA s vyšší dostupností.

**SQL Server na virtuálních počítačích Azure** zvolte, pokud:

* Máte stávající místní aplikace má toomigrate nebo rozšířit toohello cloudu, nebo pokud chcete podnikové aplikace, které toobuild větší než 4 TB. Tento přístup poskytuje výhodu hello 100 % SQL kompatibility, kapacity velké databáze plnou kontrolu nad systému SQL Server a Windows a zabezpečené tunelové tooon místní. Tento přístup minimalizuje náklady na vývoj a úpravy stávajících aplikací.
* Máte stávající prostředky IT a zvládnete zajistit použití dílčích oprav, zálohování a vysokou dostupnost databáze. Všimněte si, že některé automatizované funkce tyto operace značně zjednodušují. 

## <a name="next-steps"></a>Další kroky
* V tématu [svoji první databázi SQL Azure](sql-database-get-started-portal.md) tooget začít s SQL Database.
* Viz [SQL Database – ceny](https://azure.microsoft.com/pricing/details/sql-database/)
* V tématu [zřízení virtuálního počítače s SQL serverem v Azure](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md) tooget začít s SQL Server na virtuálních počítačích Azure.

