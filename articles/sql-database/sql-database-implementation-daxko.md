---
title: "aaaAzure SQL databáze Azure Případová studie - Daxko/CSI | Microsoft Docs"
description: "Další informace o tom, jak Daxko/CSI používá SQL Database tooaccelerate jeho cyklu vývoje a tooenhance jeho zákaznické a výkonu"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 00c8a713-f20c-4d6b-b8b7-0c1b9ba5f05b
ms.service: sql-database
ms.custom: reference
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/10/2017
ms.author: carlrab
ms.openlocfilehash: 3e3d58a1d9c3c919fc0e4cdb2765f680719c19d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="daxkocsi-used-azure-tooaccelerate-its-development-cycle-and-tooenhance-its-customer-services-and-performance"></a>Daxko/CSI používá Azure tooaccelerate jeho cyklu vývoje a tooenhance jeho zákaznické a výkonu
![Logo Daxko/CSI](./media/sql-database-implementation-daxko/csidaxkologo25.png)

Software Daxko/CSI potýkají výzvu: zákaznické základny vhodnosti a rekonstrukce Center byl narůstá, Děkujeme toohello úspěch jeho řešení rozsáhlé podnikové softwaru, ale zachová s hello infrastruktury IT potřebuje pro tento rostoucí Zákaznická základna byl testování hello společnosti zaměstnanců IT. Hello společnosti byl omezené stále rostoucí operations režie, hlavně pro správu její rostoucí databáze. Horší byl tento režijní náklady na operace vyjímání do vývoj prostředky pro nové iniciativy, jako jsou nové funkce mobility pro software společnosti hello.

Podle tooDavid Molina, ředitel produktu vývoj v Daxko/CSI, Azure zadané CSI Software s hello platforma jako služba (PaaS) modelu, že ho nepotřebují toosimplify Správa databáze, zvýšení škálovatelnosti a uvolní prostředky toofocus na Software místo ops. "Azure SQL Database je skvělou možnost pro nás. Nemá tooworry o údržbu systému SQL Server clusteru s podporou převzetí služeb při selhání a všechny hello jiných potřeb infrastruktury je ideální pro nás."

Vzhledem k tomu migraci tooAzure, CSI softwaru potřebuje provozní personál z jenom dva toomanage přes 600 databáze zákazníka. Společnost Hello používá Azure SQL Database elastické fondy databáze toomove zákazníka na základě velikosti a potřebují.

Pokračuje Molina "naše zákazníky popisovač hello změnit okamžitě. Před elastické fondy někdy měly vypršení časových limitů a další potíže během období shluků. S Azure elastické fondy můžou burst podle potřeby a používat hello softwaru bez problémů."

Kromě tooimproving výkonu pro zákazníky, Azure elastické fondy uvolnit systémové prostředky CSI softwaru toofocus na vývoj nových služeb a funkcí, místo práci s operace a správa. Tyto prostředky IT pomohly zvýšit jeho podnikového softwaru nabídky, SpectrumNG, toohelp softwaru CSI zaujmout sebou členy, zlepšení efektivity zaměstnanci a poskytují zaměstnanci a členy mobilní přístup pro interaktivní úlohy a oznámení v reálném čase.

Azure má také pomohl CSI Software urychlit a zlepšení hello vývoj a zajištění kvality (QA) cyklus povolením možnosti automatizace. S Azure implementací hello společnosti může správce sestavení zabalit komponenty s hello klepnutím na tlačítko. Jak popisuje Molina, "jako součást cyklus vydání hello QA je nyní možné toodeploy tooa testovacího prostředí v Azure, které úzce napodobuje naše produkční zásobníku. Můžeme nasadit sestavení okamžitě tooour dev prostředí toovet změny. Který je velký win pro nás, protože nebyly k dispozici parita pro testování před."

## <a name="offloading-toohello-cloud"></a>Snižování zátěže toohello cloudu
Před přesunutím toohello cloud, byl CSI softwaru úspěšně vytvářet vlastní víceklientské infrastruktury v datacentru místní v Houston. Rozbalené hello společnosti, se potýkají roste důsledně Kvetoucí z nákupu, zřizování a údržbě všechny hello hardwaru a softwaru potřebných toosupport svým zákazníkům. IT personálního oddělení toohandle se stala jiné problémové místo, která vedla tooa zpomalení zřizování nových prostředků a zavádí nové služby toocustomers.

CSI Software prostudování možností cloudu k odstranění režie, tak, aby se může soustředit na jeho kód, namísto jeho operací. Hello společnosti zjistí, že mnoho poskytovatelů nejvyšší cloudu hello nabízejí pouze řešení infrastruktury jako služby (IaaS), které stále vyžadují velký hello IaaS zásobník toomanage pracovníci IT. V hello end CSI softwaru určit, že bylo řešení Azure PaaS hello hello nejlepší přizpůsobit pro své potřeby. Vysvětluje Molina "Azure získá hello hardwaru a systému softwaru mimo hello způsob tak jsme mohli soustředit na našem nabídek softwaru při současném snížení naše režii IT oddělení."

## <a name="making-hello-transition-tooazure"></a>Provedení přechodu tooAzure hello
Po výběru Azure pro své řešení PaaS, CSI softwaru začal migrace jeho cloudu pro toohello infrastruktury a databáze back-end. Předchozí tooAzure SpectrumNG zákazníci potřebné tooinstall klientskou aplikaci, která oznamovat pomocí služby Windows Communication Foundation (WCF) na back-end hello. Podle tooMolina, "i když někteří zákazníci hostované všechno ve svých vlastních datových center, jsme vytvořili out víceklientské toobe hello produktu. Jsme hostované vše v datacentru v Houston, pomocí systému SQL Server jako úložiště dat hello.

"Našich produktů nabídka také zahrnuty člen směřujících webového portálu (napsané v ASP.net), která byla webová služba navrženou toobe s popiskem white toomatch hello zákazníka a online stránky SOAP API toosupport hello a nějakou integraci třetích stran."

Hello migrace toohello cloudu nepřijala dlouhé pro architekturu hello. Podle tooMolina, "hello většina hello úsilí vyřešit změnou hello způsobem, že jsme přečetli informace o souboru config, změna centralizované připojovacího řetězce, a automatizaci hello balení, odesílání a nasazení naše verzí."

toodevelop hello sestavit automatizaci, technici CSI Software používá prostředí Azure PowerShell a rozhraní REST API toocreate balíčků a k jejich nahrávání tooa pracovní prostředí pro verzi každou noc.
Hello celkové přechod tooan nasazení založené na cloudu Azure se rychle a bezproblémově pro hello CSI softwaru IT tým. Vysvětluje Molina "ve všech, jsme měli prostředí beta v cloudu hello do tří týdnů toofour pořízení na hello projektu. Který byl překvapivé win pro nás."

Po konfiguraci a testování hello prostředí, CSI softwaru začal migrace zákazníků. Pro zákazníky už používá hostování softwaru CSI byla téměř bezproblémové hello přechodu. Pro zákazníky migraci z místního nasazení, trvalo některé další dobu hello migrace toohello cloudu, ale byla stále především problémové bez pro zákazníky a CSI softwaru.

Pro nové zákazníky, CSI softwaru pro pracovníky IT používat hello následující proces tooon panelu je tooAzure:

1. Azure skriptů prostředí PowerShell jsou použité toospin si novou databázi pro zákazníka hello; všem zákazníkům spustí na tooensure úroveň premium dostatek počáteční propustnost pro přechod hello.
2. Pokud je to možné, CSI Software používá hello Průvodce migrací SQL Azure toomove existující data tooan Azure SQL Database instance.
3. Nakonec jsou Microsoft integrační služby SSIS (SQL Server) používané tooreconcile nesrovnalostí v hello data nebo tooperform vyčištění dat jako vyžaduje.

V současné době o 99 procent zákazníků CSI softwaru jsou hostované v Azure, napříč čtyři regionální datová centra (centrální – sever, Jižní střední, – východ a – západ). Tak, že datových centrech v zeměpisné oblasti každého zákazníka, latence je udržováno tooa minimální.

## <a name="azure-elastic-pools-free-up-it-resources"></a>Azure elastické fondy uvolněte IT zdroje
Některé funkce služby Azure pomohly CSI softwaru shift nebudou infrastruktury a operací cílených toobeing funkce a vývoj zaměřuje. Možná největších benefit hello byl z elastické fondy.

CSI Software aktuálně poskytuje o 550 databáze pro zákazníky. Před elastické fondy, bylo obtížné toomanage tolika databází v rámci struktury vrstvy. Správci OPS měl tooassign úrovně výkonu na základě potřeb shluků hello zákazníků, které vyžaduje významné IT prostředky režie. S elastické fondy správce můžete přiřadit klienty premium nebo standardní fond, podle potřeby a potom přesunout zákazníkům na základě velikosti a potřebují. Zákazníci popisovač hello důsledky elastické fondy hello téměř okamžitě; před elastické fondy zákazníků měla vypršení časových limitů a další potíže během období shluků využití, ale s elastické fondy, zákazníků může zaznamenat shluky aktivity podle potřeby a může i dál toouse SpectrumNG bez problémů.

## <a name="azure-active-geo-replication-accelerates-reporting"></a>Azure aktivní geografickou replikací zrychluje vytváření sestav
Několik zákazníků CSI softwaru jsou také využívat výhod Azure aktivní geografickou replikací. S aktivní geografickou replikací, až toofour čitelný sekundární databáze se dá nakonfigurovat v hello oblastech datacenter stejný nebo jiný. CSI softwaru využívá aktivní geografickou replikací dvěma způsoby: nejdřív hello sekundární databáze jsou k dispozici v případě hello datacenter výpadku nebo hello nemohou tooconnect toohello primární databáze; a druhý, sekundární databáze hello načíst a může být použité toooffload jen pro čtení zatížení, jako jsou třeba úlohy sestav. Někteří zákazníci CSI softwaru použít tento benefit tooaccelerate reporting pracovních postupů.

## <a name="csi-software-application-logic-and-architecture"></a>CSI Software aplikační logiku a architektura
SpectrumNG používá webové role. Jelikož aplikace hello víceklientské, služby WCF je použité toohandle hello původní požadavek na připojení od zákazníků. Jako Molina stavy "hello požadavek identifikuje každého zákazníka, které pak umožňuje nám vytvořit připojovací řetězec na tootheir databáze toodo budeme potřebovat toodo."

Pro webovou vrstvu hello jeho služby CSI softwaru využívá Azure automatické škálování podle den a čas. Dostupné prostředky jsou automaticky vyšší tooaccommodate vyšší využití během pracovní doby, podle časového pásma toohello každé místní datové centrum. Prostředky jsou nastaveny také tooscale na víkendy, kdy jsou nižší požadavky zákazníka.

![Architektura Daxko/CSI](./media/sql-database-implementation-daxko/figure1.png)

Obrázek 1. Cloudové služby rolí pracovního procesu nevykresluje strukturovaných dat z Azure SQL Database a částečně strukturovaných dat z úložiště tabulek. SpectrumNG uživatelé komunikovat s že dat přes cloud services webové role.

## <a name="using-web-apps-and-a-web-plan-tier-for-mobile-apps"></a>Pomocí webové aplikace a vrstvy webové plán pro mobilní aplikace
Používání Azure SQL Database uvolní prostředky pro CSI softwaru tooenable nové iniciativy, včetně kompletní mobilní platformu podle vlastního rozhraní API hostované ve službě Azure web apps. Platforma Hello umožňuje členům sebou a zaměstnanci toouse mobilní zařízení toocheck plány, sešit třídy a přijímat zprávy.

Hello platforma používá orientované na služby architektura (SOA) tootake jedinou komponentu – jako jsou například POS systému (POS) nebo prodejní systému – přesunout hello překrýt tooanother webové plán a pak začne pracovat toosupport služby tuto součást, a nechat všem ostatním na plán webového původní Hello. Tato schopnost poskytuje značnou flexibilitu CSI softwaru, a pomáhá snížení nákladů.

## <a name="azure-lets-csi-software-developers-focus-on-apps-and-services"></a>Azure umožňuje zaměřit se vývojáři softwaru CSI na aplikace a služby
Azure SQL Database není právě boon tooSpectrumNG zákazníkům, kteří získejte hello rychlé, spolehlivé služby, je také velký win CSI softwaru pracovníky IT a vývojářům. Přesměrováním ops tooAzure v cloudu hello CSI softwaru omezuje jeho režii prostředků a infrastruktury, výrazně accelerated jeho vývoj cyklů a již nepotřebuje toomicromanage databáze toooptimize výkonu pro své klienty.

## <a name="more-information"></a>Další informace
* toolearn Další informace o Azure elastické fondy, najdete v části [elastické fondy](sql-database-elastic-pool.md).
* toolearn Další informace o databázi nástroje a elastické škálování, najdete v části [nástroje elastické databáze a elastické škálování](sql-database-elastic-scale-get-started.md).
* toolearn Další informace o migraci databáze systému SQL Server, najdete v tématu [migrovat tooAzure databáze systému SQL Server](sql-database-cloud-migrate.md).
* toolearn Další informace o aktivní geografickou replikaci, najdete v části [aktivní geografickou replikací](sql-database-geo-replication-overview.md).
* toolearn Další informace o webových rolí a rolí pracovního procesu, najdete v části [rolí pracovního procesu](../fundamentals-introduction-to-azure.md#compute).    
* toolearn Další informace o Azure Service Bus, najdete v části [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).
* toolearn Další informace o automatické škálování, najdete v části [škálování cloudové služby](../cloud-services/cloud-services-how-to-scale.md).

