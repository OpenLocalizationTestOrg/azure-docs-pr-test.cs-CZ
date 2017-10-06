---
title: "Poznámky k aaaRelease pro Brána pro správu dat | Microsoft Docs"
description: "Brána pro správu dat, text poznámky k verzi"
services: data-factory
author: nabhishek
manager: jhubbard
editor: monicar
ms.assetid: 14762e82-76d9-41c4-ba9f-14a54da29c36
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: abnarain
published: True
ms.openlocfilehash: 3165d7537410a0531e0bb7f7fe584767f9155574
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="release-notes-for-data-management-gateway"></a>Poznámky k verzi pro Bránu pro správu dat
Jedním z hello výzvy pro integraci moderní dat je toomove tooand data z místně toocloud. Objekt pro vytváření dat umožňuje integraci se Brána pro správu dat, což je agenta můžete nainstalovat přesun dat hybridní tooenable místně.

V tématu hello následující články podrobné informace o Brána pro správu dat a jak toouse ho:

*  [Brána správy dat](data-factory-data-management-gateway.md)
*  [Přesun dat mezi místními a cloudu pomocí Azure Data Factory](data-factory-move-data-between-onprem-and-cloud.md)


## <a name="current-version-21063477"></a>AKTUÁLNÍ VERZE (2.10.6347.7)

### <a name="enhancements-"></a>Vylepšení-
- Můžete přidat DNS položky toowhitelist služby service bus, místo vytvoření seznamu povolených všechny Azure IP adresy z brány firewall (v případě potřeby). Odpovídající položka DNS můžete najít na portálu Azure ('Vytvořit a nasadit' -> objekt pro vytváření dat -> 'Brány' -> "serviceUrls" (ve formátu JSON)
- Konektor HDFS nyní podporuje veřejný certifikát podepsaný svým držitelem tím, že umožňuje přeskočit ověřování SSL.
- Opravené: Problém s bránou offline během aktualizace (z důvodu tooclock zkosení)



## <a name="earlier-versions"></a>Starší verze

## <a name="2963132"></a>2.9.6313.2
### <a name="enhancements-"></a>Vylepšení-
-   Můžete přidat toowhitelist položky DNS Service Bus, místo vytvoření seznamu povolených všechny Azure IP adresy z brány firewall (v případě potřeby). Další informace v tomto poli.
-   Nyní můžete zkopírovat data z objektu blob jeden blok až too4.75 TB, což je maximální hello podporované velikost objektu blob bloku. (dřívější limit byl 195 GB).
-   Opravené: Nedostatek paměti problém při rozbalení několik malých souborů během aktivity kopírování.
-   Pevné: Index je mimo rozsah problém při kopírování z dokumentu DB tooan místní systém SQL Server s funkcí idempotenci.
-   Opravené: Skript pro vyčištění SQL nefunguje s místní SQL Server z Průvodce kopírováním.
-   Opravené: Název sloupce s místem na konci hello nefunguje v aktivitě kopírování.

## <a name="28662833"></a>2.8.66283.3
### <a name="enhancements-"></a>Vylepšení-
- Opravené: Problém s chybějícími přihlašovací údaje u restartování počítače brány.
- Opravené: Problém s registrací během brány obnovit záložního souboru.


## <a name="2762401"></a>2.7.6240.1
### <a name="enhancements-"></a>Vylepšení-
- Opravené: Nesprávné čtení desetinnou hodnotu null z databáze Oracle jako zdroj.

## <a name="2661922"></a>2.6.6192.2
### <a name="whats-new"></a>Co je nového
- Zákazníci můžete poskytnout zpětnou vazbu na bránu zaregistrujete činnost.
- Podpora nový formát komprese: ZIP (Deflate)

### <a name="enhancements-"></a>Vylepšení-
- Zlepšení výkonu pro Oracle jímky, HDFS zdroje.
- Oprava chyby pro brány automatické aktualizace, gateway paralelní zpracování kapacity.


## <a name="2561641"></a>2.5.6164.1
### <a name="enhancements"></a>Vylepšení
- Vylepšené a robustnější brány registrace zkušeností – teď můžete sledovat průběh během procesu registrace brány hello, takže je registrace hello prostředí rychlejšího.
- Zlepšení brány obnovit proces - je stále možné obnovit bránu i v případě, že nemáte hello brány záložní soubor s touto aktualizací. To se vyžaduje pověření tooreset propojené služby v portálu.
- Oprava chyby.

## <a name="2461511"></a>2.4.6151.1

### <a name="whats-new"></a>Co je nového

- Nyní můžete ukládat přihlašovací údaje zdroje dat místně. Hello přihlašovací údaje jsou šifrované. Hello přihlašovacích údajů zdroje dat lze obnovit a obnovit pomocí hello záložní soubor, který se dá vyexportovat ze hello existující bránu, všechny místní.

### <a name="enhancements-"></a>Vylepšení-

- Vylepšené a robustnější brány registrace prostředí.
- Podpora formátu textu v Průvodce kopírováním automatické zjišťování QuoteChar konfigurace a zlepšit hello celkové formátu detekce přesnost.

## <a name="2361002"></a>2.3.6100.2

- Podpora firstRowAsHeader a SkipLineCount automatické zjišťování v Průvodce kopírováním textových souborů v systému souborů na místě a HDFS.
- Zvýšení stability hello síťové připojení mezi bránou a Service Bus
- Několik oprav chyb


## <a name="2260721"></a>2.2.6072.1

*  Podporuje nastavení proxy serveru HTTP pro použití brány hello hello Správce konfigurace brány. Pokud nakonfigurovaný, objektů Blob v Azure, Azure Table, Azure Data Lake a Documentdb jsou přístupné prostřednictvím proxy serveru HTTP.
*  Hlavička podporuje zpracování TextFormat při kopírování dat z / tooAzure objektů Blob, Azure Data Lake Store, místní systém souborů a místní HDFS.
*  Podporuje kopírování dat z objektu Blob připojit a objektů Blob stránky spolu s hello již nepodporuje objekt Blob bloku.
*  Zavádí nový stav brány **Online (s omezením)**, což znamená, že hlavní funkce hello hello brány funguje s výjimkou hello interaktivní operace podporu pro Průvodce kopírováním.
*  Zvyšuje odolnost hello registrace brány pomocí registračního klíče.

## <a name="216040"></a>2.1.6040.

*  Ovladač DB2 je součástí instalační balíček brány hello teď. Není nutné tooinstall ho samostatně.
*  Teď podporuje DB2 ovladače z/OS a DB2 pro i (AS / 400) společně s hello platformách, již podporuje (Linux, Unix a Windows).
*  Podporuje používání Azure Cosmos DB jako zdrojový nebo cílový pro místní úložiště dat
*  Podporuje kopírování dat z/toocold/horkou objektu blob úložiště společně s hello již nepodporuje účet úložiště pro obecné účely.
*  Umožňuje tooon místní tooconnect systému SQL Server prostřednictvím brány s oprávněními vzdálené přihlášení.  

## <a name="2060131"></a>2.0.6013.1

*  Můžete vybrat toobe jazyků a kultur hello používá bránu při ruční instalaci.

*  Pokud brána nebude fungovat podle očekávání, můžete protokoly brány toosend posledních sedmi dnech tooMicrosoft toofacilitate řešení potíží s hello problém. Pokud brána není připojen toohello cloudovou službu, můžete zvolit toosave a archivaci protokoly gateway.  

*  Vylepšení uživatelského rozhraní pro správce konfigurace brány:

    *  Nastavit stav brány. víc viditelný na kartě Domů hello.

    *  Ovládací prvky reorganizovány všechny a jednodušší.

    *  Můžete zkopírovat data z úložiště pomocí hello [nástroj preview bez kódu kopírování](data-factory-copy-data-wizard-tutorial.md). V tématu [připravený kopie](data-factory-copy-activity-performance.md#staged-copy) obecné podrobnosti o této funkci.
*  Brána pro správu dat tooingress data přímo z místní databáze systému SQL Server můžete použít do Azure Machine Learning.

*  Vylepšení výkonu

    * Zlepšení výkonu na zobrazení schématu nebo Preview proti systému SQL Server v nástroji preview bez kódu kopírování.

## <a name="11259531"></a>1.12.5953.1

*  Opravy chyb

## <a name="11159181"></a>1.11.5918.1

*  Maximální velikost protokolu událostí brány hello zvýšila od 1 MB too40 MB.

*  V případě, že během automatickou aktualizaci brány je zapotřebí restartování, zobrazí se dialog s upozorněním. Můžete toorestart vpravo pak nebo novější.

*  V případě, že automatické aktualizace nezdaří, instalační program brány opakuje, automatické aktualizace třikrát za maximální.

*  Vylepšení výkonu

    * Zlepšení výkonu pro načítání velké tabulky z místního serveru ve scénáři bez kódu kopírování.

*  Opravy chyb

## <a name="11058921"></a>1.10.5892.1

*  Vylepšení výkonu

*  Opravy chyb

## <a name="1958652"></a>1.9.5865.2

*  Nulové funkcí touch automatické aktualizace
*  Ikona nový panelu s indikátory stavu brány
*  Možnost příliš "teď"z aktualizovat hello klienta
*  Čas plánované aktualizace tooset možnost
*  Skript prostředí PowerShell pro přepnutím zapnout nebo vypnout automatickou aktualizaci
*  Podpora formátu JSON  
*  Vylepšení výkonu
*  Opravy chyb

## <a name="1858221"></a>1.8.5822.1

*  Lepší řešení potíží
*  Vylepšení výkonu
*  Opravy chyb

### <a name="1757951"></a>1.7.5795.1

*  Vylepšení výkonu
*  Opravy chyb

### <a name="1757641"></a>1.7.5764.1

*  Vylepšení výkonu
*  Opravy chyb

### <a name="1657351"></a>1.6.5735.1

*  Podpora místní HDFS zdroj/jímka
*  Vylepšení výkonu
*  Opravy chyb

### <a name="1656961"></a>1.6.5696.1

*  Vylepšení výkonu
*  Opravy chyb

### <a name="1656761"></a>1.6.5676.1

*  Diagnostické nástroje podpory na Configuration Manager
*  Podpora sloupců tabulky pro tabulkové zdroje dat pro Azure Data Factory
*  Podpora pro vytváření dat Azure SQL DW
*  Podpora pro vytváření dat Azure Reclusive v BlobSource a FileSource
*  Podpora CopyBehavior – MergeFiles, PreserveHierarchy a FlattenHierarchy v BlobSink a FileSink s binární kopie pro datovou továrnu Azure
*  Podpora aktivity kopírování hlášení průběhu pro Azure Data Factory
*  Ověření připojení zdroje dat podporu pro vytváření dat Azure
*  Opravy chyb

### <a name="1656721"></a>1.6.5672.1

*  Podpora pro Azure Data Factory název tabulky zdroje dat ODBC
*  Vylepšení výkonu
*  Opravy chyb

### <a name="1656581"></a>1.6.5658.1

*  Podpora souboru jímky pro datovou továrnu Azure
*  Podpora pro Azure Data Factory zachování hierarchie v binární kopie
*  Podpora idempotenci aktivity kopírování pro vytváření dat Azure
*  Opravy chyb

### <a name="1656401"></a>1.6.5640.1

*  Podpora 3 Další zdroje dat pro Azure Data Factory (rozhraní ODBC, OData, HDFS)
*  Podpora znak uvozovky csv analyzátoru pro Azure Data Factory
*  Podpora komprese (BZip2)
*  Opravy chyb

### <a name="1556121"></a>1.5.5612.1

*  Podpora pět relačních databází pro Azure Data Factory (MySQL, PostgreSQL, DB2, Teradata a Sybase)
*  Podpora komprese (Gzip a Deflate)
*  Vylepšení výkonu
*  Opravy chyb

### <a name="1455491"></a>1.4.5549.1

*  Přidání podpory zdroje dat Oracle pro Azure Data Factory
*  Vylepšení výkonu
*  Opravy chyb

### <a name="1454921"></a>1.4.5492.1

*  Jednotná binární soubor, který podporuje služby Microsoft Azure Data Factory a Office 365 Power BI
*  Upřesnit hello procesu uživatelské rozhraní konfigurace a registrace
*  Azure Data Factory – Azure příchozí a odchozí podporu pro zdroj dat systému SQL Server

### <a name="1253031"></a>1.2.5303.1

*  Opravte problém toosupport časový limit více časově náročné připojení ke zdroji dat.

### <a name="1155268"></a>1.1.5526.8

*  Vyžaduje rozhraní .NET Framework 4.5.1 předpokladem je během instalace.

### <a name="1051442"></a>1.0.5144.2

*  Žádné změny, které ovlivňují scénáře Azure Data Factory.
