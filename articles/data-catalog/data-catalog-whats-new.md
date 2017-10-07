---
title: "aaaWhat je nového ve službě Azure Data Catalog | Microsoft Docs"
description: "Tento článek obsahuje přehled nových funkcí přidat tooAzure katalogu Data Catalog."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 1201f8d4-6f26-4182-af3f-91e758a12303
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/22/2017
ms.author: maroche
ms.openlocfilehash: f60130487ece39e110446b68544945089d2ab37e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="whats-new-in-azure-data-catalog"></a>Co je nového v Azure Data Catalog
Aktualizace příliš**Azure Data Catalog** jsou vydávány pravidelně. Ne každý verze obsahuje nové funkce zobrazující se uživatelům, jako některých vydání se zaměřuje na funkce back endové službě. Tato stránka označuje nové služby Azure Data Catalog přidané toohello možnosti zobrazující se uživatelům.

## <a name="whats-new-for-august-2017"></a>Co je nového u 2017 srpen 
Od srpna 2017 hello následující možnosti přidané tooAzure katalogu Data Catalog:

*   Nových vzorků vývojáře je k dispozici pro vytváření a správu vztah metadat pomocí hello REST API katalogu Data Catalog. Hello *importovat informace o vztahu do katalogu Data Catalog* ukázka je dostupná na hello [stránce ukázek kódu katalogu Data Catalog](https://azure.microsoft.com/resources/samples/?service=data-catalog&sort=0). 
* Podpora pro extrahování připojení relace metadata ze zdroje dat Teradata při registraci související tabulky pomocí nástroj registrace zdroje dat hello.
* Podpora pro funkce vracející tabulky systém SQL Server (TVF) objekty při registraci zdroje dat systému SQL Server pomocí nástroj registrace zdroje dat hello.
* Více aktualizací a obecnější tooincrease hello výkonnost a použitelnost hello katalogu Data Catalog portálu.

## <a name="whats-new-for-july-2017"></a>Co je nového u července 2017 
Od července 2017 hello následující možnosti přidané tooAzure katalogu Data Catalog:
*   Podpora pro podrobnější řízení činnosti rozhodného metadat, včetně:
    - Katalog správci můžou omezit značky toocontribute možnost uživatelů a související metadata toohello katalogu, povolení katalogu toohello oprávnění jen pro čtení.
    - Katalog správci můžou omezit uživatelů možnost tooregister nové zdroje dat v katalogu hello.
    - Katalog správci můžou omezit uživatelů možnost tootake vlastnictví data asset metadata v katalogu hello.
    - TooAzure skupin zabezpečení služby Active Directory a uživatelů pro snadnou správu oprávnění lze udělit oprávnění.
* Podpora pro vztahy mezi registrovaných datových prostředků a zjišťování prostředků souvisejících dat portálu hello katalogu Data Catalog, včetně:
    - Extrakce metadat relace ze serveru SQL Server (včetně Azure SQL Database), Oracle a MySQL zdroje dat při použití hello nástroj registrace zdroje dat katalogu Data Catalog.
    - Zjišťování prostředků souvisejících dat při zobrazení asset metadata hello katalogu Data Catalog portálu.
    - Operace toodefine, zjišťovat a spravovat vztahy mezi datové prostředky pomocí hello REST API katalogu Data Catalog.

Další podrobnosti týkající se správy oprávnění v katalogu Data Catalog najdete v tématu [přístupu toodata katalogu a data prostředků toosecure](data-catalog-how-to-secure-catalog.md).
Další informace o vztahy v katalogu Data Catalog najdete v tématu [jak tooview související datových prostředcích v Azure Data Catalog](data-catalog-how-to-view-related-data-assets.md).

## <a name="whats-new-for-june-2017"></a>Co je nového u června 2017 
Od června 2017 hello následující možnosti přidané tooAzure katalogu Data Catalog:
*   Podpora pro zdroje dat databáze Sybase, Apache Cassandra a MongoDB. Uživatelé teď můžete registrovat a zjišťovat Cassandra a MongoDB databáze a tabulky a databáze Sybase, tabulky a zobrazení.

> [!NOTE]
> Při registraci MongoDB a Cassandra tabulky, které zahrnují tyto sloupce, sloupce s komplexními datovými typy, jako je například záznamy a pole nebudou zahrnuty v metadatech hello přidat tooData katalogu.

## <a name="whats-new-for-may-2017"></a>Co je nového u může 2017 
Od verze může 2017 hello následující možnosti přidané tooAzure katalogu Data Catalog:
*   Nových vzorků vývojáře je k dispozici pro hello hromadný import obchodní Glosář termínů. Hello Glosář hromadným importem ukázka je dostupná na hello [katalogu Data Catalog vývojáře ukázky stránky](https://docs.microsoft.com/azure/data-catalog/data-catalog-samples). 
*   Podpora pro úpravy informace o připojení ODBC v portálu Azure Data Catalog hello. Vlastníci asset data a Data Catalog správci nyní můžete upravit informace o registrovaných zdroje dat ODBC hello připojení bez nutnosti toore registrace zdroje dat hello.
*   Podpora pro prokliknutelný adresy URL v obchodní Glosář definice a popisy. Adresy URL jsou zahrnutá ve vlastnostech popis a definice hello pro firmy slovníku pojmů se hello adresy URL se zobrazí jako hypertextové odkazy v portálu hello katalogu Data Catalog.
*   Podpora pro zobrazení expert názvy kromě tooexpert e-mailové adresy. Při zobrazení Odborníci v hello vlastnosti pro datový prostředek hello katalogu Data Catalog portálu, zobrazí se v popisku hello hello expert úplný název ze služby Azure Active Directory.

## <a name="whats-new-for-april-2017"></a>Co je nového u dubna 2017 
Od verze dubna 2017 hello následující možnosti přidané tooAzure katalogu Data Catalog:
*   Podpora pro zdroje dat ODBC. Uživatelé teď můžete registrovat a zjišťovat databází, tabulek a zobrazení pomocí nástroj registrace zdroje dat katalogu Data Catalog hello ODBC.

## <a name="whats-new-for-march-2017"></a>Co je nového u března 2017 
Od verze března 2017 hello následující možnosti přidané tooAzure katalogu Data Catalog:
*   Podpora pro použití skupin zabezpečení AAD správcům katalogu Data Catalog.
*   Azure Data Catalog je k dispozici v nové oblasti Azure. Zákazníci tak můžou zřizovat hello Azure Data Catalog v hello – Západ střední USA oblasti, v přidání tooEast USA, západ USA, západní Evropa a Austrálie – východ, Severní Evropa a jihovýchodní Asie. Další informace najdete v tématu [oblasti Azure](https://azure.microsoft.com/regions/).

## <a name="whats-new-for-february-2017"></a>Co je nového u února 2017 
Od února 2017 hello následující možnosti přidané tooAzure katalogu Data Catalog:
*   Podpora pro pokročilé nastavení v nástroj registrace zdroje dat katalogu Data Catalog hello. Při registraci zdroje velkého množství dat, mohou uživatelé zadat příkaz hodnoty časového limitu.
*   Podpora pro FTP a PostgreSQL datové zdroje. Uživatelé teď můžete registrovat a zjišťovat FTP soubory a adresáře a databáze PostgreSQL, tabulky a zobrazení.

## <a name="whats-new-for-january-2017"></a>Co je nového u ledna 2017 
Od ledna 2017 hello následující možnosti přidané tooAzure katalogu Data Catalog:
*   Azure Data Catalog je nyní [CSA HVĚZDIČKY](https://www.microsoft.com/trustcenter/compliance/csa-star-certification) kompatibilní.
*   Integrace s [získat & transformaci v aplikaci Excel 2016 a Power Query pro Excel](https://support.office.com/article/Introduction-to-Microsoft-Power-Query-for-Excel-6E92E2F4-2079-4E1F-BAD5-89F6269CD605). Aplikace Excel uživatelé mohli sdílet dotazy a dotazy pomocí Azure Data Catalog ze zjišťování v aplikaci Excel. Tato funkce je k dispozici toousers s Power BI Pro licence.

## <a name="whats-new-for-december-2016"></a>Co je nového pro prosinec 2016
Od prosince 2016 hello následující možnosti přidané tooAzure katalogu Data Catalog:
*   Azure Data Catalog je nyní [HIPAA](https://www.microsoft.com/trustcenter/Compliance/HIPAA) a [modelové doložky EU](https://www.microsoft.com/TrustCenter/Compliance/EU-Model-Clauses) kompatibilní.
*   Podpora pro úpravy informace o připojení zdroje dat. Vlastníkům asset dat a Data Catalog správci nyní můžete upravit informace o připojení hello registrovaných datových zdrojů bez nutnosti toore registrace zdroje dat hello.
*   Podpora pro zdroje dat Salesforce.com. Uživatelé teď můžete registrovat a zjišťovat objekty služby Salesforce.


## <a name="whats-new-for-november-2016"></a>Co je nového pro listopadu 2016
Od listopadu 2016 hello následující možnosti přidané tooAzure katalogu Data Catalog:
*   Azure Data Catalog je nyní [ISO/IEC 27001](https://www.microsoft.com/trustcenter/compliance/iso-iec-27001) a [ISO/IEC 27018](https://www.microsoft.com/TrustCenter/Compliance/iso-iec-27018) kompatibilní.
*   Podpora pro hello ruční registraci zdroje dat ODBC pomocí portálu pro katalog Data Catalog hello a REST API.

## <a name="whats-new-for-september-2016"></a>Co je nového u září 2016
Od září 2016 hello následující možnosti přidané tooAzure katalogu Data Catalog:

* Podpora pro zdroje dat IBM DB2. Uživatelé teď můžete registrovat a zjišťovat databází DB2, tabulky a zobrazení.
* Podpora pro Azure Cosmos DB datové zdroje. Uživatelé teď můžete registrovat a zjišťovat Cosmos DB databáze a kolekce.
* Podpora pro přizpůsobení hello název katalogu Data Catalog portálu hello. Teď umožňuje správcům katalogu zobrazit text, který se zobrazí v nadpis portálu hello, jako je například název organizace hello.

## <a name="whats-new-for-august-2016"></a>Co je nového u srpna 2016
Od srpna 2016 následující možnosti hello Přibyla tooAzure katalogu Data Catalog:

* Vylepšení pro registraci zdroje dat SQL Server Master Data Services (MDS). Uživatelé mohou nyní zahrnují profily verze Preview a data při registraci pomocí nástroj registrace zdroje dat katalogu Data Catalog hello entity služby MDS.
* Podpora pro definované správcem organizační uložená hledání. Při ukládání vyhledávání v portálu hello katalogu Data Catalog, správci katalogu Data Catalog teď můžete zvolit toosave hledání pro osobní použití nebo pro všechny uživatele katalogu. Organizační uložená hledání jsou sdíleny se všemi uživateli katalogu a může poskytnout standardizované počáteční body pro zjišťování zdroje dat.
* Aktualizuje zobrazení vlastností toohello portálu hello katalogu Data Catalog. Všechny vlastnosti datového zdroje dat se nyní zobrazují a spravovat v jednom, umožňující změnu velikosti podokna tooprovide více konzistentní a zjistitelný prostředí.

## <a name="whats-new-for-july-2016"></a>Co je nového pro července 2016
Od července 2016 hello následující možnosti přidané tooAzure katalogu Data Catalog:

* Podpora pro zdroje dat SQL Server Master Data Services (MDS). Uživatelé teď můžete registrovat a zjišťovat MDS modelů a entit.
* Podpora pro uložené procedury SQL serveru. Uživatelé teď můžete registrovat a zjišťovat objekty uložené procedury v datové zdroje systému SQL Server.
* Podpora dalších jazyků na portálu Azure Data Catalog hello hello nástroj registrace zdroje dat, celkem 18 podporované jazyky. Hello uživatelské prostředí Azure Data Catalog je lokalizované podle nastavení v systému Windows nebo ve webovém prohlížeči hello hello jazykové předvolby.
* Aktualizace a vylepšení pro hello katalogu Data Catalog domovské stránky portálu, včetně vylepšení výkonu a efektivní uživatelské prostředí.

## <a name="whats-new-for-june-2016"></a>Novinky z června 2016
Od června 2016 hello následující možnosti přidané tooAzure katalogu Data Catalog:

* Podpora pro změnu velikosti sloupce v zobrazení seznamu hello při zjišťování datových prostředků portálu hello katalogu Data Catalog. Uživatelé nyní změnit velikost jednotlivých sloupců toomore snadno přečíst metadata zdlouhavé asset například značky a popisy.
* Power Query pro Excel byla přidána toohello "Otevřít v" nabídku katalogu Data Catalog portálu hello. Uživatelé nyní lze otevřít podporovaných zdrojů dat v aplikaci Excel 2016 nebo v aplikaci Excel 2010 a Excel 2013 s hello [Power Query pro Excel](https://support.office.com/article/Introduction-to-Microsoft-Power-Query-for-Excel-6E92E2F4-2079-4E1F-BAD5-89F6269CD605) nainstalovaným doplňkem.
* Podpora pro zdroje dat úložiště tabulek Azure. Uživatelé teď můžete registrovat a zjišťovat objekty tabulky v zdrojů dat úložiště Azure.

## <a name="whats-new-for-may-2016"></a>Co je nového pro května 2016
Od může 2016 hello následující možnosti přidané tooAzure katalogu Data Catalog:

* Obchodní glosář, který umožňuje správcům katalogu toodefine firemní podmínky a hierarchií toocreate běžné obchodní termínů. Uživatele můžete označit registrované datové prostředky s Glosář podmínky toomake je snazší toodiscover a pochopit hello obsah hello katalogu. Další informace najdete v tématu [jak tooset až hello obchodní Glosář řízeným přidáváním značek](data-catalog-how-to-business-glossary.md)  
* Vylepšení toohello Data katalogu obchodní glosář, který umožňuje uživatelům tooupdate více slovníku pojmů v rámci jedné operace. Uživatelé mohou vybrat více hello tooedit podmínky následující pole:
  * Nadřazené termín: hello uživatel může vybrat novou nadřazenou termín a všechny vybrané podmínky jsou aktualizované toobe podřízené hello vybrané nadřazené podmínek. Pokud hello vybrané podmínky, které mají hello stejnou nadřazenou položku a pak tento nadřazený se zobrazí v textovém poli hello, jinak hello nadřazené termín pole je sada tooblank.   
  * Značky a zúčastněnými stranami: Uživatelé můžete přidávat a odebírat značky a zúčastněnými stranami pro více slovníku pojmů pomocí hello stejné prostředí jako označování více datových prostředků.

> [!NOTE]
> obchodní Glosář Hello je k dispozici pouze v hello Azure Data Catalog, Standard Edition. Edice Free Hello nenabízí možnosti pro upraveny označování nebo obchodní Glosář.

## <a name="whats-new-for-march-2016"></a>Co je nového u března 2016
Od. března 2016 hello následující možnosti přidané tooAzure Data katalogu: g:

* Konsolidované endpoint REST API k programovému přístupu k hello hledání funkce a možnosti správy katalogu asset hello služby Azure Data Catalog. Tento koncový bod hledání koncový bod a katalog API rozhraní API byla zastaralá a zastaví 21. března 2016. Neexistují žádné změny toohello sémantiku hello rozhraní API. Pouze koncový bod hello změnit identifikátor URI. Další informace najdete v tématu hello [referenční dokumentace rozhraní API Azure Data Catalog REST](https://msdn.microsoft.com/library/azure/mt267595.aspx). Ukázky rozhraní API naleznete v části [ukázky pro vývojáře Azure Data Catalog](data-catalog-samples.md).

## <a name="whats-new-for-february-2016"></a>Co je nového u února 2016
Od února 2016 hello následující možnosti přidané tooAzure katalogu Data Catalog:

* Výběr zdroje dat nově přepracovanou služeb na základě zkušeností nástroj registrace zdroje dat Azure Data Catalog hello. Hello nástroj registrace zdroje dat byla aktualizovaná toomake it jednodušší je toolocate a vybrat ze zdroje dat hello podporované pomocí Azure Data Catalog.
* Podpora dalších 10 jazyků na portálu Azure Data Catalog hello nástroj registrace zdroje dat hello. Kromě toho tooEnglish, hello prostředí Azure Data Catalog je teď dostupná v němčina, španělština, francouzština, italština, japonština, korejština, brazilská portugalština, ruština, zjednodušená čínština a tradiční čínštině. Hello Azure Data Catalog činnost koncového uživatele je lokalizované podle nastavení v systému Windows nebo ve webovém prohlížeči hello uživatele hello jazykové předvolby.
* Podpora pro geografická replikace dat Azure Data Catalog pro obchodní kontinuitu a zotavení po havárii. Veškerý obsah v Azure Data Catalog, včetně metadata a crowdsourcovaná poznámky zdroje dat jsou nyní replikují mezi dvěma oblastí Azure na toocustomers bez dalších nákladů. Hello oblastí Azure jsou předem spárovat, alespoň 500 miles od sebe a postupujte podle hello mapování, jak je popsáno v [obchodní kontinuitu a zotavení po havárii (BCDR): spárovat oblasti Azure](../best-practices-availability-paired-regions.md).
* Podpora pro změnu hello předplatné Azure použité pomocí Azure Data Catalog. Azure Data Catalog mohou správci stránku hello nastavení v portálu Azure Data Catalog hello – tooselect jiného předplatného Azure pro účely fakturace.

## <a name="whats-new-for-january-2016"></a>Co je nového pro leden 2016
Od ledna 2016 hello následující možnosti přidané tooAzure katalogu Data Catalog:

* Podpora pro ruční registraci dalších zdrojů dat. Teď můžete použít "Vytvoření ruční položku" v portálu Azure Data Catalog hello nebo použít hello Azure Data Catalog REST API tooregister hello následující zdroje dat:
  * OData - funkce sady entit a kontejneru entit
  * HTTP – soubor, koncový bod, sestavy a lokality
  * Systém souborů – soubor
  * SharePoint – seznam
  * FTP – souborů a adresářů
  * Salesforce.com – objekt
  * DB2 - tabulky, zobrazení a databáze
  * PostgreSQL - tabulky, zobrazení a databáze
* Podpora pro "Otevřít v SQL Server Data Tools" zdroje dat serveru SQL Server (včetně databáze SQL Azure a Azure SQL Data Warehouse).  
* Podpora pro registraci a zjišťování SAP HANA zobrazení a balíčků. Můžete zaregistrovat SAP HANA datového zdroje pomocí Azure Data Catalog dat hello zdroje nástroj pro registraci a můžete opatřit poznámkami a zjistit registrované SAP HANA zdroje dat pomocí portálu Azure Data Catalog hello.
* Hello možnost toopin a Odepnout datových prostředcích v portálu Azure Data Catalog hello. Můžete zvolit toopin data prostředky toomake je snazší toorediscover a opakované použití.
* Nově přepracovanou domovské stránky portálu Azure Data Catalog hello. nové domovskou stránku Hello obsahuje přehled o hello aktuální uživatelé aktivita – včetně nedávno publikovaných prostředky připnutý prostředky a uložená hledání - a přehledy o hello aktivity v hello katalogu jako celek.
* Podpora pro trvalé uživatelských nastavení na portálu Azure Data Catalog hello. Nastavení uživatelského prostředí – včetně zobrazení mřížky nebo dlaždice hello počet výsledků na stránce a zvýrazňování zapnout nebo vypnout - jsou nastavené jako trvalé mezi uživatelských relací.
* Azure Data Catalog je teď dostupná v dva nové oblastech Azure. Zákazníci tak můžou zřizovat hello Azure Data Catalog do hello severní Evropy a Asie a Tichomoří – jihovýchod oblastí v přidání tooEast USA, západ USA, západní Evropa a Austrálie – východ. Další informace najdete v tématu [oblasti Azure](https://azure.microsoft.com/regions/).

> [!NOTE]
> "Open in SQL Server Data Tools" vyžaduje Visual Studio 2013 s aktualizacemi Update 4 a nástroji SQL Server mohla toobe nainstalována. tooinstall hello nejnovější verzi SQL Server Data Tools, navštivte [stáhnout SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx).


## <a name="whats-new-for-december-2015"></a>Co je nového pro prosinec 2015
Od prosince 2015 hello následující možnosti přidané tooAzure katalogu Data Catalog:

* Podpora pro profily data pro zdroje dat Azure SQL Data Warehouse. Při registraci Azure SQL Data Warehouse tabulek a zobrazení, uživatelé mohou tooinclude data profilu metriky s hello metadat extrahovaných ze zdroje dat hello.
* Podpora pro registraci a zjišťování objektů MySQL a databází. Uživatelé mohou registrovat MySQL datového zdroje pomocí Azure Data Catalog dat hello zdroje nástroj pro registraci a můžete opatřit poznámkami a zjistit registrované MySQL zdroje dat pomocí portálu Azure Data Catalog hello.
* Podpora ověřování SPNEGO a systému Windows pro zdroje dat Teradata. Při registraci Teradata tabulek a zobrazení, uživatelé mohou tooconnect tooTeradata pomocí SPNEGO a systému Windows a LDAP a TD2 ověřování.
* Podpora pro zdroje dat Azure Data Lake Store. Uživatelé teď můžete registrovat a zjišťovat zdroje dat Azure Data Lake Store pomocí Azure Data Catalog.
* Podpora pro ruční zadání nastavení proxy sítě v nástroj registrace zdroje dat Azure Data Catalog hello. Uživatelé "Upravit nastavení proxy serveru" můžete vybrat z úvodní stránku hello nástroj a můžete zadat adresu a port toobe serveru hello proxy používané nástrojem hello.

## <a name="whats-new-for-november-2015"></a>Co je nového pro listopad 2015
Od listopadu 2015 hello následující možnosti přidané tooAzure katalogu Data Catalog:

* Hello možnost tooview a zkopírujte připojovací řetězce z portálu Azure Data Catalog hello (včetně Azure SQL Database) systému SQL Server a Oracle datových zdrojů. Uživatele můžete kliknout na odkaz "Zobrazit připojovací řetězce" hello informace o připojení pro SQL Server nebo Oracle tabulka, zobrazení nebo databáze, toosee hello připojovací řetězce použít zdroj dat toohello tooconnect. ADO.NET, rozhraní ODBC, OLEDB a JDBC připojovací řetězce jsou k dispozici pro datové zdroje systému SQL Server. ODBC a OLEDB připojovací řetězce jsou uvedené pro zdroje dat Oracle.
* Podpora včetně profily dat při registraci Teradata tabulek a zobrazení.
* Podpora pro "Otevřít v Power BI Desktop" pro SQL Server (včetně databáze SQL Azure a Azure SQL Data Warehouse), SQL Server Analysis Services, Azure Storage a HDFS zdroje.  
* Podpora pro ověřování pomocí protokolu LDAP pro zdroje dat Teradata. Při registraci Teradata tabulek a zobrazení, uživatelé mohou tooconnect tooTeradata pomocí protokolu LDAP a TD2 ověřování.
* Podpora pro "Otevřít v aplikaci Excel" zdroje dat Teradata.
* Podpora pro poslední hledaný text v portálu Azure Data Catalog hello. Při hledání hello portálu, uživatelé mohou vybrat z naposledy použité vyhledávání podmínky tooaccelerate hello zjišťování prostředí.
* Podpora pro verzi preview pro zdroje dat Teradata. Při registraci Teradata tabulek a zobrazení, uživatelé mohou tooinclude snímku záznamy s hello metadat extrahovaných ze zdroje dat hello.
* Podpora pro "Otevřít v aplikaci Excel" zdroje dat Azure SQL Data Warehouse.
* Podpora pro definování a úprava schémata úrovni sloupce pro ručně registrované datové prostředky. Po vytvoření ručně pomocí portálu Azure Data Catalog hello datový prostředek, můžete přidat uživatele v hello vlastnosti datového zdroje dat, definice sloupců.
* Podpora pro dotazy "má" při hledání Azure Data Catalog, tooenable hello zjišťování registrovaných datových prostředků, které měl konkrétních metadat. Syntaxe dotazu Azure Data Catalog nyní zahrnuje:

| Syntaxe dotazu | Účel |
| --- | --- |
| `has:previews` |Vyhledá datové prostředky, které zahrnují náhled |
| `has:documentation` |Vyhledá datových prostředků, pro které bylo zadáno dokumentace |
| `has:tableDataProfiles` |Vyhledá datové prostředky se informace o profilu úrovni tabulky dat |
| `has:columnsDataProfiles` |Vyhledá datové prostředky se informace o profilu data na úrovni sloupce |


> [!NOTE]
> "Otevřít v Power BI Desktop" vyžaduje aktuální verzi toobe aplikace Power BI Desktop hello nainstalována. Pokud narazíte na problémy nebo chyby při použití této funkce, ujistěte se, že máte nejnovější verzi aplikace Power BI Desktop z hello [PowerBI.com](https://powerbi.com).


## <a name="whats-new-for-october-2015"></a>Co je nového pro říjen 2015
Od října 2015 hello následující možnosti přidané tooAzure katalogu Data Catalog:

* Podporu pro šifrování v klidovém stavu dat profily verze Preview a dat pro registrované datové zdroje. Azure Data Catalog transparentně šifruje všechny záznamy preview a data profily zdroje dat registrované službou hello bez nutnosti pro správu klíčů správci katalogu.
* Podpora pro zdroje dat Teradata. Uživatelé teď můžete registrovat a zjišťovat Teradata tabulek a zobrazení.
* Podpora pro místní zdroje dat Hive. Uživatelé teď můžete registrovat a zjišťovat tabulek Hive pro Apache Hive v Hadoop místní datové zdroje.
* Podpora pro uložená hledání na portálu Azure Data Catalog hello. Uživatele můžete uložit hledaných termínů a tooeasily výběr filtru zopakujte předchozí hledání a definujte užitečné zobrazení katalogu hello obsah. Uživatele můžete také označit uloženého hledání jako jejich výchozí vyhledávání. Když uživatel klikne na ikonu hledání hello "lupy" z hello Azure Data Catalog domovské stránky portálu nebo ze stránky "Začínáme" hello, je převzat hello uživatele přímo toohello uložené hledání, které jsou označené jako výchozí.
* Podpora pro RTF dokumentace pro registrované datové prostředky a kontejnery v portálu Azure Data Catalog hello. Uživatelé nyní zadat dokumentace pro datové prostředky, jako je například tabulek, zobrazení a sestavy a pro kontejnery, jako jsou databáze a modely pro scénáře, kde značky a popisy nejsou dostatečná.
* Podpora pro ruční registraci data známé typy zdrojů. Uživatele můžete ručně zadat informace o zdroji dat pomocí portálu Azure Data Catalog hello pro všechny typy zdrojů dat podporovaných Azure Data Catalog.
* Podpora pro ověřování skupiny zabezpečení Azure Active Directory. Katalog správci mohou povolit přístup toosecurity skupiny katalogu, uživatelské účty, takže je jednodušší toomanage přístup tooAzure katalogu Data Catalog.
* Podpora pro otevírání podregistru zdroje dat v aplikaci Excel z portálu Azure Data Catalog hello.

> [!NOTE]
> Pro aktuální verzi hello je podporováno pouze ověřování Teradata TD2. Uvolní další ověřovací mechanismy jsou podporovány v budoucnosti.

> [!NOTE]
> toouse funkce "Otevřít v aplikaci Excel" hello zdroje dat Hive, uživatelům musí mít nainstalované ovladače ODBC hello pro Hive.

## <a name="whats-new-for-september-2015"></a>Co je nového u září 2015
Od září 2015 hello následující možnosti přidané tooAzure katalogu Data Catalog:

* Podpora včetně profily dat při registraci zdroje dat Hive.
* Podpora pro zjišťování prostřednictvím kódu programu hello rozhraní API katalogu, usnadňuje toointegrate aplikací s Azure Data Catalog.
* Nová "Začínáme" data source práci s funkcí zjišťování v portálu Azure Data Catalog hello. Když uživatelé zadají hello "objevit" stránky portálu Azure Data Catalog hello bez zadávání hledaný termín, zobrazí se přehled hello katalogu obsah, včetně hello nejčastěji používá značky, odborníky, typy zdrojů dat a typy objektů.
* Podpora pro registraci a zjišťování objektů Azure SQL datového skladu a databází. Další informace o Azure SQL Data Warehouse, najdete v části [SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).
* Podpora pro registraci a zjišťování servery SQL Server Reporting Services jako kontejnery a modely služby SQL Server Analysis Services. Při registraci služby SSAS a SSRS objekty, Azure Data Catalog vytvoří položku pro model služby SSAS hello a serveru služby SSRS a pro sestavy hello jiné objekty. kontejnery Hello může být zjištěn a pomocí portálu Azure Data Catalog hello. Uživatelé mohou také hledat a filtr hello obsah modelu nebo serveru v přidání toosearching a filtrování hello obsah hello katalogu.
* Podpora pro registraci a zjišťování objektů SQL Server Analysis Services přes HTTP nebo HTTPS. Uživatelé nyní můžete přihlásit tooSSAS serverů pomocí adresy URL (například https://servername/olap/msmdpump.dll) namísto název serveru a v přidání tooWindows ověřování můžete použít základní ověřování a anonymní připojení. Další informace o tooSSAS připojení protokolu HTTP nebo HTTPS, v tématu [konfigurovat přístup HTTP tooAnalysis služby](https://msdn.microsoft.com/library/gg492140.aspx).
* Podpora pro zdroje dat Hive v HDInsight. Uživatelé teď můžete registrovat a zjišťovat tabulek Hive pro Apache Hive v Hadoop v HDInsight datové zdroje. Další informace o Hive v HDInsight, naleznete v části hello [centru dokumentace HDInsight](../hdinsight/hdinsight-use-hive.md).
* Podpora pro registraci a zjišťování Oracle – databáze a clustery HDFS jako kontejnery. Při registraci Oracle tabulek a zobrazení nebo HDFS, Azure Data Catalog vytvoří položku hello databáze, tabulky a zobrazení. Hello databáze může být zjištěn a pomocí portálu Azure Data Catalog hello. Uživatelé mohou také hledat a filtr hello obsahu databáze nebo clusteru kromě toosearching a filtrování hello obsah hello katalogu.
* Podpora pro ruční registraci typy zdrojů dat je neznámý. Uživatelé ručně zadejte informace o zdroji dat pomocí portálu Azure Data Catalog hello, takže zdroje dat není explicitně nepodporuje hello nástroj registrace zdroje dat můžete opatřeny poznámkami a zjistit.
* Podpora pro registraci a zjišťování databází systému SQL Server jako kontejnery. Při registraci serveru SQL Server tabulek a zobrazení, Azure Data Catalog vytvoří položku hello databáze, tabulky a zobrazení. Hello databáze může být zjištěn a pomocí portálu Azure Data Catalog hello. Uživatelé mohou také hledat a filtr hello obsahu databáze přidání toosearching a filtrování hello obsah hello katalogu.

> [!NOTE]
> SSAS a SSRS objektů, které byly registrované předchozí toohello září 18 verze musí znovu registrovat pomocí hello nástroj registrace zdroje dat před hello modelu nebo server bude přidána položka toohello katalogu. Znovu registraci zdroje dat nemá vliv na žádné poznámky, které jsou přidané uživatele na portálu Azure Data Catalog hello.

> [!NOTE]
> Oracle tabulek a zobrazení a HDFS soubory a adresáře, které byly registrované toohello předchozí verze 11. září musí znovu registrovat pomocí hello nástroj registrace zdroje dat před hello databáze nebo clusteru je přidána položka toohello katalogu. Znovu registraci zdroje dat nemá vliv na žádné poznámky, které jsou přidané uživatele na portálu Azure Data Catalog hello.

> [!NOTE]
> SQL Server tabulek a zobrazení, které byly registrované toohello předchozí verze 4 září musí znovu registrovat pomocí hello nástroj registrace zdroje dat před přidáním položky databáze hello je toohello katalogu. Znovu registraci zdroje dat nemá vliv na žádné poznámky, které jsou přidané uživatele na portálu Azure Data Catalog hello.

## <a name="whats-new-for-august-2015"></a>Co je nového u srpen 2015
K srpnu 2015 hello následující možnosti přidané tooAzure katalogu Data Catalog:

* Podpora profilace data z datové zdroje systému SQL Server a Oracle. Při registraci serveru SQL Server a Oracle tabulek a zobrazení, uživatelé mohou informace o tooinclude data profilu pro objekty hello registrována. profil Hello dat zahrnuje statistik úrovni objektů a úrovni sloupce.
* Podpora pro zdroje dat Hadoop HDFS. Uživatelé teď můžete registrovat a zjišťovat HDFS souborů a adresářů.
* Podpora pro poskytování informací o žádosti o přístup pro registrované datové zdroje. Pro všechny registrované datový prostředek uživatelé mohou nyní poskytují pokyny pro vyžádání přístupu, včetně e-mailu odkazy nebo adresy URL, tooeasily integrovat existující nástroje a procesy.
* Popisy tlačítek pro značky a odborníky, toomake je snazší toodiscover co uživatelé zadali jaká metadata pro registrované datové prostředky.
* Přidali jsme novou "User" tlačítka a nabídky tooour horní navigační panel. Tato nabídka umožňuje hello uživatele najdete v části hello účet používaný toolog na tooAzure katalogu Data Catalog a toosign se v případě potřeby. Tato nabídka zobrazí název katalogu hello, což je cenné toodevelopers pomocí hello Azure Data Catalog REST API.
* Pouze edice Standard: Při přidávání vlastníky toodata prostředky, Azure Data Catalog teď podporuje uživatelské účty a skupiny zabezpečení jako vlastníci. tooadd skupinu zabezpečení jako vlastníka pro vybrané datové prostředky, můžete zadat buď hello skupiny zobrazovaný název nebo skupiny hello (UPN) e-mailovou adresu, pokud jej obsahuje.
* Podpora pro zdroje dat Azure Blob Storage. Uživatelé teď můžete registrovat a zjišťovat objekty BLOB úložiště Azure a adresářů.

