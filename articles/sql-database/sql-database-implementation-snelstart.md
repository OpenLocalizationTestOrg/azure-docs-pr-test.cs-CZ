---
title: "aaaAzure SQL databáze Azure Případová studie - Snelstart | Microsoft Docs"
description: "Další informace o tom, jak SnelStart používá databázi SQL toorapidly rozšířit jeho firemní služby ve výši 1 000 nové databáze SQL Azure, za měsíc"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: fab506b2-439d-4f1a-bdc5-d1d25c80d267
ms.service: sql-database
ms.custom: reference
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/10/2017
ms.author: carlrab
ms.openlocfilehash: 69572393f41a9baf44f41b4f5f4ebbef347de851
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="with-azure-snelstart-has-rapidly-expanded-its-business-services-at-a-rate-of-1000-new-azure-sql-databases-per-month"></a>S Azure se rozšířila SnelStart rychle jeho firemní služby ve výši 1 000 nové databáze SQL Azure, za měsíc
![SnelStartLogo](./media/sql-database-implementation-snelstart/snelstartlogo.png)

SnelStart díky oblíbených finanční a obchodní-software pro správu pro malé a střední firmy (SMB) je v Nizozemí hello. Jeho 55,000 zákazníci jsou obsluhovány pomocí pracovníci 110 zaměstnanců, včetně vlastní IT oddělení 35. Přesunutím z nabídky softwaru jako služba (SaaS) tooa desktopového softwaru založený na Azure SnelStart provedené hello nejvíce předdefinovaných služeb, automatizaci správy pomocí známém prostředí v jazyce C# a optimalizace výkonu a škálovatelnosti podle ani přes - nebo v části zřizování firmy používající elastické fondy. Použití Azure poskytuje SnelStart hello plynulost přesun zákazníkům mezi místními a hello cloudu.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-Case-Study-SnelStart/player]
> 
> 

## <a name="why-snelstart-extended-services-from-hello-desktop-toohello-cloud"></a>Proč SnelStart rozšířené služby z cloudu plochy toohello hello
> "Práce s Azure rozumí jsme může poskytovat rychlejší softwaru, rychle reagovat toocustomer požadavky a škálování řešení při zvýšit nároky."
> 
> – Henry byla architekti softwaru
> 
> 

SnelStart spustili úspěšné softwaru obchodní let, pomocí modelu tradiční vývoj: kód, balíčků, odeslání a opakujte. V průběhu času hello krok změny vzrostla rychlejší a rychleji. Předpisy často mění a zákazníci potřebné jednodušší způsoby tooprocess finanční záznamy a spolupracovat s jejich účetní a government tookeep si s těmito změnami.

"Přesouvání toocustomers softwaru je nákladné a omezení," podle tooHenry byla architekti softwaru. "Výroby, balení a nákladů přesouvání omezené jsme jak často zveřejnit software. Narazili jsme na toopackage aktualizace pro pravidelné dodávky, ale který dostal pevný toomeet naše zákazníky změna vyžaduje. Nikdy jsme může zaručit, že naše zákazníky upgradovali toohello nejnovější verzi produktu hello. Proto jsme měli toosupport několik verzí, provedení hello a podporovat úlohy velmi obtížné toodo."

Byla přidá, "jsme chtěli způsob tooprogram a verze aktualizace v Zrychlený rychle, proto jsme může inovacemi. Zajistěte rychlejší a vytvoření nové služby pro naše zákazníky. Můžeme také chtěli, že způsob tooautomate více procesů v pořadí toosimplify potřebám firmy správy našich zákazníků."

Pro SnelStart, hello řešení byla tooextend jeho služby podle stal poskytovatele cloudové SaaS. Platforma Azure SQL Database Hello pomohl SnelStart tam dostat, aniž by docházelo k hello hlavní režii IT oddělení, by vyžadoval tento postup řešení infrastruktury jako služba (IaaS).

model cloudu Hello také umožňuje SnelStart toofix chyby a zadejte nové funkce rychle, bez zákazníkům, kteří potřebují toodownload a aktualizace softwaru. Podle tooBeen, "pomocí cloudových služeb Azure nám umožňuje tooquickly act po změně požadavků od jiných výrobců. Místo nutnosti tooship nové toothousands verze zákazníků, jsme můžete přizpůsobit informace odesílané ze naše formáty toonew desktopová aplikace vyžadují třetí strany."

## <a name="a-modern-company-with-traditional-roots"></a>Moderní společnost s tradiční kořenových certifikačních autorit
SnelStart je moderní, agilní, špičkovými obchodní s humble kořeny případě aktualizace too1964, při spuštění hello umístit hello společnosti jako výrobce MIDI částí. Hello srdcem hello firmy softwaru SnelStart skutečně spuštěna signálu v hello 1980s, během hello, jak narůstá počet hello osobní počítače. Hello společnosti potřebné lepší alternativní toohello monitorování účtů softwaru k dispozici v době hello, takže trvalo věci do vlastní rukou. V 1982 vytvořili hello společnosti hello znalosti toho, co by přestat SnelStart monitorování účtů softwaru. Od začátku hello se pro jednoduchost a rychlost admired softwaru hello, takže většina tak, aby hello společnosti nakonec změnit fokus a reinvented samotné do softwarové společnosti.

V roce 1995 vydala SnelStart její první účetnictví aplikací pro Windows. aplikace Hello, založený na 1.0 Microsoft Visual Basic a Microsoft Access databáze pomohl růst hello zákazníka základní toomore než 5 000 uživatelů.

V současné době SnelStart je hlavní zprostředkovatel-obchodní (LOB) a správa obchodních aplikací zaměřený na Holandská SMB a nezávislém vedoucími podniků. Jak uvádí Carlo Kuip, IT Architekti, "Naším cílem je tooprovide 100 procent automation obchodní správy služeb pro naše zákazníky."

## <a name="optimizing-performance-and-cost-with-elastic-pools"></a>Optimalizace výkonu a nákladů s elastické fondy
SnelStart byl ve velkém měřítku včasným elastické fondy. Elastické fondy pomáhají hello společnosti limit náklady a efektivněji spravovat požadavky na výkon. Podle tooBeen, "s použitím elastické fondy, jsme můžete optimalizovat výkon podle potřeb hello našich zákazníků bez předimenzování. Pokud jsme měli tooprovision podle zatížení ve špičce, by bylo velmi nákladné. Místo toho hello možnost tooshare prostředků mezi několika databází nízkého umožňuje nám toocreate řešení, které provede dobře a je nákladově efektivní. "

## <a name="azure-sql-databases-help-containerize-data-for-isolation-and-security"></a>Databáze Azure SQL pomoci containerize data pro izolace a zabezpečení
Azure SQL Database umožňuje SnelStart tooeasily a transparentně přesouvat zákazníků místní Správa obchodních dat tooAzure. Hello Azure SQL Database je vhodné kontejner, který poskytuje izolaci, hranice pro ověřování, autorizaci a snadno funkce zálohování a obnovení. Databáze poskytují vhodným konceptuálního modelu pro správu firmy. Podle tooCarlo Kuip, IT Architekti, "položky v rámci této hranice kontejneru obsahují citlivá data, která je zásadní tooa firmy a ukládání těchto položek v udržuje izolované databáze, který je dobře chráněný. Můžeme spravovat ověřování na úrovni databáze hello a i bez nutnosti správce databáze (DBAs) na zaměstnanci automatizace hello správy a horizontální těchto databází."

Azure SQL Data Warehouse také hrají roli při zabezpečení a správu scénáře SnelStart hello tím pomáhá společnosti hello shromažďuje telemetrická data, jako je například zjišťování neoprávněných vniknutí, protokolování aktivit uživatelů a připojení.

## <a name="azure-removes-overhead-so-that-developers-can-spend-more-time-delivering-value"></a>Azure odebere režie tak, aby vývojáři mohou věnovat více času doručování hodnota
model platformy Azure Hello odebrat režijní náklady na infrastrukturu a povolené SnelStart tooautomate nasazení pomocí správy knihovny jazyka C#. Jako Kuip stavy, "nebylo možné toogrow naše aktuální operace s velmi málo zaměstnanců při současně zvýšení škálovatelnosti, rychlosti a zotavení po havárii možnosti pro naše klienty. vývoj tooservices shift Hello uvolnit systémové prostředky toofocus na nové služby a funkce, místo právě aktualizaci existující kód tookeep až s nové kódy předpisy nebo daň." Přidá, "Automatizaci správy a použitím hello SaaS nabídky, jsme se možné toodeliver další hodnoty pro naše klienty bez nutnosti toomake velkých investic v provozní pracovníci." Například pomocí Azure a elastické fondy SnelStart byl schopný tooadd celou řadu nových funkcí, včetně integrace robustnější data zákazníků s bankami, nové fakturace služby, kontroly malé firmy a e-mailové služby.

> "V právě hello první několik měsíců 2016, jsme rozšířit naše nasazení Azure SQL Database z přibližně 5 500 toomore než 12 000 a aktuálně je právě přidávána asi 1000 databáze za měsíc."
> 
> – Henry byla architekti softwaru
> 
> 

Správa databáze je další automatizované pomocí funkce elastické úlohy hello. Jako Kuip stavy, "vysoce Děkujeme hello automatické zjišťování databází [server] instance databáze SQL." To umožňuje SnelStart tooexecute operace správy napříč dynamicky rostoucí zákazníkovi základní bez dalších zásahů.

SnelStart také vyvíjí rozhraní API, který funguje jako zprostředkovatel mezi zákaznická data a aplikace vytvořené pomocí softwaru třetí strany partnery. Jako Kuip stavy "Toto rozhraní API vám umožní jiných dodavatelů tooadd funkce tooour software, například odstraňuje datová položka pro faktury a jiné dokumenty." Zákazníci, nebudou mít toomanually typ faktury do své malé firmy monitorování účtů softwaru; Hello SnelStart softwaru bude exchange hello nezbytné informace přímo. To umožňuje zákazníkům toojoin své firmy správy úloh s hello ekosystém informace, které je vycházejících z digitální transformace v odvětví hello.  

## <a name="how-azure-services-enable-saas-for-snelstart"></a>Jak povolit službám Azure SaaS pro SnelStart
Pomocí služby Azure, může sloužit SnelStart svým zákazníkům a jejich účetní více bezproblémově v cloudu hello. Například zákazníků a účetní umožňuje sdílení informací pomocí přímý přístup k rozhraní API klienta na SnelStart, hostovanou v Azure. Stavy Kuip "tyto opakovaně použitelné služby jsou k dispozici tooour zákazníkem webové aplikace a poskytují společnou infrastrukturu a funkce správy toobusiness tooallow přístup pro zákazníky a toothird výrobce softwaru používaných ve našich zákazníků. Mezi příklady patří poskytování konfiguraci produktu, Správa pravidel brány firewall a správa dlouho běžící procesy, například záloh."

> Naším cílem je tooprovide 100 procent automatizace obchodní správy služeb pro naše zákazníky." 
> 
> – Carlo Kuip, IT architekti
> 
> 

Kromě toho SnelStart webové služby umožňují zákazníků a účetní tooeasily přístup k datům v Azure SQL Database elastické fondy. Tento model SaaS kombinaci s databáze pružnost a Azure Resource Manager poskytuje SnelStart škálovatelnost funkce, které doplňují každé nasazení Azure. implementace Hello je plně automatizovat pomocí správy knihovny jazyka C#.

![Architektura SnelStart](./media/sql-database-implementation-snelstart/figure1.png)

Obrázek 1. Od června 2016 SnelStart má více než 11 000 databází a více než 50 elastické fondy

## <a name="simplicity-from-hello-cloud"></a>Jednoduchost z cloudu hello
Od přesunutí tooan Azure cloudové řešení, SnelStart bylo možné toosupport rychlé zákazníka růstu zároveň nabízí inovativní funkce a služby. Podle tooBeen, "s Azure, jsme doručovat prakticky nepřetržité aktualizace pro naše zákazníky bez rozbalení naše provozní personál. A se nám získat všechny hello Další skvělé funkce Azure – jako škálovatelnost a zotavení po havárii – seskupeny ve. "

> "Pokud se ve skutečnosti přes v Redmondu... Zobrazila volání od vývojáře zpět v hello Nizozemsko telefonují mě o konkrétním problému. Jsme měla mít tooget Microsoft toodeliver změnu ve svém provozním prostředí v rámci 48 hodin toosolve naše problém. "
> 
> – Henry byla architekti softwaru
> 
> 

SnelStart také oceňuje hello silné partnerství, které jste vyvinuté s týmem Microsoft Azure SQL DB hello. Jako Kuip stavy "máme diskusí na funkce, a jak toouse technologii, která se vezme v úvahu na obou stranách."
Hello okamžitou cílem SnelStart je tookeep rostoucí základní jeho splněna zákazníka. Jak se stavy, "Bez hello technické a omezení prostředků, které jsme měli jako ISV, neexistuje žádné omezení toohow daleko jsme můžou růst."

## <a name="more-information"></a>Další informace
* toolearn Další informace o Azure elastické fondy, najdete v části [elastické fondy](sql-database-elastic-pool.md).
* toolearn Další informace o webových rolí a rolí pracovního procesu, najdete v části [rolí pracovního procesu](../fundamentals-introduction-to-azure.md#compute).    
* toolearn Další informace o Azure SQL Data Warehouse, najdete v části [SQL Data Warehouse](https://azure.microsoft.com/documentation/services/sql-data-warehouse/)
* toolearn Další informace o SnelStart, najdete v části [SnelStart](http://www.snelstart.nl).

