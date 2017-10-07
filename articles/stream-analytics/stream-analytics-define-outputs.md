---
title: "Stream Analytics výstupy: možnosti pro úložiště, analýzu | Microsoft Docs"
description: "Informace o cílení na možnosti výstupy Stream Analytics dat včetně Power BI pro analysis výsledky."
keywords: "transformace dat, výsledky analýzy, možnosti ukládání dat"
services: stream-analytics,documentdb,sql-database,event-hubs,service-bus,storage
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: ba6697ac-e90f-4be3-bafd-5cfcf4bd8f1f
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 99f8113f0464960e898293397fbe3de90d669857
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="stream-analytics-outputs-options-for-storage-analysis"></a>Stream Analytics výstupy: možnosti pro úložiště, analýzy
Při vytváření úlohy Stream Analytics, zvažte, jak budou využívat hello Výsledná data. Jak se vám zobrazí hello výsledky úlohy Stream Analytics hello, kam se budou ukládat?

V pořadí tooenable řadu vzorů aplikací má Azure Stream Analytics různé možnosti pro ukládání výstupu a zobrazit výsledky analýzy. To umožňuje snadno tooview výstup úlohy a umožňuje flexibilitu při využívání hello a úložiště výstup hello úlohy datového skladu a jiné účely. Žádný výstup nakonfigurované v úloze hello musí existovat hello úloha se spustila a události spuštění toku. Například pokud použijete jako výstupu úložiště objektů Blob, úlohy hello nebude vytvořit účet úložiště automaticky. Potřebuje toobe vytvořené uživatelem hello před zahájení úlohy ASA hello.

## <a name="azure-data-lake-store"></a>Azure Data Lake Store
Stream Analytics podporuje [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/). Toto úložiště umožňuje toostore data jakékoli velikosti, typu a rychlosti příjmu pro provozní a zjišťovací analýzy. Další Stream Analytics musí toobe oprávnění tooaccess hello Data Lake Store. Podrobnosti o ověřování a jak toosign službu hello Data Lake Store (v případě potřeby) jsou popsané v hello [Data Lake výstup článku](stream-analytics-data-lake-output.md).

### <a name="authorize-an-azure-data-lake-store"></a>Autorizace Azure Data Lake Store
Pokud Data Lake Storage je vybraná jako výstupu v hello portálu Azure, bude výzvami tooauthorize připojení tooan existující Data Lake Store.  

![Autorizovat Data Lake Store](./media/stream-analytics-define-outputs/06-stream-analytics-define-outputs.png)  

Pak vyplňte vlastnosti hello hello Data Lake Store výstup, jak vidíte níže:

![Autorizovat Data Lake Store](./media/stream-analytics-define-outputs/07-stream-analytics-define-outputs.png)  

Následující tabulka Hello uvádí hello názvy vlastností a jejich popis potřebné pro vytvoření výstupní Data Lake Store.

<table>
<tbody>
<tr>
<td><B>NÁZEV VLASTNOSTI</B></td>
<td><B>POPIS</B></td>
</tr>
<tr>
<td>Alias pro výstup</td>
<td>Toto je popisný název používaný v dotazy toodirect hello dotazu výstup toothis Data Lake Store.</td>
</tr>
<tr>
<td>Název účtu</td>
<td>Název Hello hello účet Data Lake Storage, kde jsou odesílání výstupu. Zobrazí se rozevírací seznam účtů Data Lake Store, které má uživatel hello toowhich přihlášení toohello portálu přístup k.</td>
</tr>
<tr>
<td>Předpony vzorek cesty [<I>volitelné</I>]</td>
<td>Hello souboru cesta používaná toowrite vaše soubory v rámci hello zadaný účet Data Lake Store. <BR>{date}, {time}<BR>Příklad 1: složku1/logs / {date} / {time}<BR>Příklad 2: složku1/logs / {date}</td>
</tr>
<tr>
<td>Datum formátu [<I>volitelné</I>]</td>
<td>Pokud token kalendářního data hello se používá v cestě hello předponu, můžete vybrat formát data hello, ve kterém jsou uspořádány soubory. Příklad: Rrrr/MM/DD</td>
</tr>
<tr>
<td>Formát času [<I>volitelné</I>]</td>
<td>Pokud token čas hello se používá v cestě hello předponu, zadejte formát času hello, ve kterém jsou uspořádány soubory. Aktuálně hello podporována pouze hodnota je HH.</td>
</tr>
<tr>
<td>Formát serializace událostí</td>
<td>Formát serializace pro výstupní data. Jsou podporovány JSON, CSV a Avro.</td>
</tr>
<tr>
<td>Encoding</td>
<td>Pokud formátu CSV nebo formátu JSON, kódování musí být zadán. Znakové sady UTF-8 je hello pouze v současné době podporovaný formát kódování.</td>
</tr>
<tr>
<td>Oddělovač</td>
<td>Platí jenom pro serializaci sdílených svazků clusteru. Stream Analytics podporuje řadu běžných oddělovačů pro serializaci dat sdíleného svazku clusteru. Podporované hodnoty jsou čárkami, středník, místo, karta a svislá čára.</td>
</tr>
<tr>
<td>Formát</td>
<td>Platí jenom pro serializaci JSON. Řádcích: Určuje, že se bude formátovat výstup hello tak, že každý objekt JSON oddělených nový řádek. Pole určuje, že hello výstup naformátovaný jako pole objektů JSON.</td>
</tr>
</tbody>
</table>

### <a name="renew-data-lake-store-authorization"></a>Obnovit autorizační Data Lake Store
Budete potřebovat toore-ověření svého účtu Data Lake Store, pokud došlo ke změně jeho heslo vzhledem k tomu, že vaše úlohy vytvoření nebo poslední ověření.

![Autorizovat Data Lake Store](./media/stream-analytics-define-outputs/08-stream-analytics-define-outputs.png)  

## <a name="sql-database"></a>SQL Database
[Azure SQL Database](https://azure.microsoft.com/services/sql-database/) lze použít jako výstup pro data, která je povahou relační, nebo pro aplikace, které závisí na obsahu hostovaném v relační databázi. Úlohy Stream Analytics bude zapisovat tooan existující tabulky v databázi SQL Azure.  Všimněte si, že hello schématu tabulky se musí přesně shodovat hello pole a jejich typy se výstup z úlohy. [Azure SQL Data Warehouse](https://azure.microsoft.com/documentation/services/sql-data-warehouse/) dá se zadat taky jako výstupu prostřednictvím hello SQL Database možnost output také (Toto je funkce preview). Následující tabulka Hello uvádí hello názvy vlastností a jejich popis pro vytvoření databáze SQL výstup.

| Název vlastnosti | Popis |
| --- | --- |
| Alias pro výstup |Toto je popisný název používaný v dotazy toodirect hello dotazu výstup toothis databázi. |
| Databáze |Název Hello hello databáze, kde jsou odesílání výstupu |
| Název serveru |název serveru SQL Database Hello |
| Uživatelské jméno |Hello uživatelské jméno, který má přístup toowrite toohello databáze |
| Heslo |Hello heslo tooconnect toohello databáze |
| Table |Název tabulky Hello zápis výstup hello. Název tabulky Hello je velká a malá písmena a schéma hello této tabulky by měl odpovídat přesně toohello počet polí a jejich typy generovaných výstupu úlohy. |

> [!NOTE]
> Pro výstup úlohy v Stream Analytics je aktuálně podporuje hello nabídky Azure SQL Database. Virtuální počítač Azure s systému SQL Server databáze připojené však není podporován. Toto je toochange subjektu v budoucích verzích.
> 
> 

## <a name="blob-storage"></a>Blob Storage
Úložiště BLOB nabízí cenově výhodné a škálovatelné řešení pro ukládání velkého objemu nestrukturovaných dat v cloudu hello.  Úvod do Azure Blob storage a jeho použití, najdete v dokumentaci hello v [jak objekty BLOB toouse](../storage/blobs/storage-dotnet-how-to-use-blobs.md).

Následující tabulka Hello uvádí hello názvy vlastností a jejich popis vytvoření výstupního objektu blob.

<table>
<tbody>
<tr>
<td>NÁZEV VLASTNOSTI</td>
<td>POPIS</td>
</tr>
<tr>
<td>Alias pro výstup</td>
<td>Toto je popisný název používaný v úložišti objektů blob toothis dotazu dotazy toodirect hello výstup.</td>
</tr>
<tr>
<td>Účet úložiště</td>
<td>Hello název účtu úložiště hello kde odesílání výstupu.</td>
</tr>
<tr>
<td>Klíče účtu úložiště.</td>
<td>Hello tajný klíč přidružený účet úložiště hello.</td>
</tr>
<tr>
<td>Kontejner úložiště</td>
<td>Kontejnery poskytují možnost logického seskupování pro objekty BLOB uložené v hello služby Microsoft Azure Blob. Při nahrávání toohello objektu blob služby objektů Blob, je nutné zadat kontejner pro tento objekt blob.</td>
</tr>
<tr>
<td>Předpony vzorek cesty [Nepovinné]</td>
<td>Cesta k souboru Hello používá toowrite objektů BLOB v hello zadaného kontejneru.<BR>V cestě hello můžete se rozhodnout toouse jeden nebo více instancí hello následující 2 frekvence hello toospecify proměnné, které jsou napsané objekty BLOB:<BR>{date}, {time}<BR>Příklad 1: cluster1/logs / {date} / {time}<BR>Příklad 2: cluster1/logs / {date}</td>
</tr>
<tr>
<td>[Nepovinné] formát data</td>
<td>Pokud token kalendářního data hello se používá v cestě hello předponu, můžete vybrat formát data hello, ve kterém jsou uspořádány soubory. Příklad: Rrrr/MM/DD</td>
</tr>
<tr>
<td>Formát času [Nepovinné]</td>
<td>Pokud token čas hello se používá v cestě hello předponu, zadejte formát času hello, ve kterém jsou uspořádány soubory. Aktuálně hello podporována pouze hodnota je HH.</td>
</tr>
<tr>
<td>Formát serializace událostí</td>
<td>Formát serializace pro výstupní data.  Jsou podporovány JSON, CSV a Avro.</td>
</tr>
<tr>
<td>Encoding</td>
<td>Pokud formátu CSV nebo formátu JSON, kódování musí být zadán. Znakové sady UTF-8 je hello pouze v současné době podporovaný formát kódování.</td>
</tr>
<tr>
<td>Oddělovač</td>
<td>Platí jenom pro serializaci sdílených svazků clusteru. Stream Analytics podporuje řadu běžných oddělovačů pro serializaci dat sdíleného svazku clusteru. Podporované hodnoty jsou čárkami, středník, místo, karta a svislá čára.</td>
</tr>
<tr>
<td>Formát</td>
<td>Platí jenom pro serializaci JSON. Řádcích: Určuje, že se bude formátovat výstup hello tak, že každý objekt JSON oddělených nový řádek. Pole určuje, že hello výstup naformátovaný jako pole objektů JSON. Toto pole se zavřou, pouze když hello úloha pozastavena nebo Stream Analytics se přesunul na toohello další časový interval. Obecně platí je vhodnější toouse řádku oddělené JSON, protože nevyžaduje žádné zvláštní zpracování, zatímco hello výstupního souboru stále probíhá zápis k.</td>
</tr>
</tbody>
</table>

## <a name="event-hub"></a>Centrum událostí
[Služba Event Hubs](https://azure.microsoft.com/services/event-hubs/) je vysoce škálovatelná publikování a odběru na přijímač událostí. Ho může shromažďovat miliony událostí za sekundu.  Jedno použití centra událostí jako výstup je při hello výstup úlohy Stream Analytics bude hello vstup jiné úlohy, streamování.

Existuje několik parametrů, které jsou potřebné tooconfigure centra událostí datové proudy jako výstup.

| Název vlastnosti | Popis |
| --- | --- |
| Alias pro výstup |Toto je popisný název používaný v dotazy toodirect hello dotazu výstup toothis centra událostí. |
| Namespace Service Bus |Obor názvů sběrnice je kontejner sady entit pro zasílání zpráv. Při vytváření nového centra událostí taky vytvoříte obor názvů sběrnice |
| Centrum událostí |Hello název výstupu centra událostí |
| Název zásady centra událostí |Hello zásady sdíleného přístupu, které se dají vytvořit na kartě Konfigurace centra událostí hello. Každá zásada sdíleného přístupu bude mít název, oprávnění, která nastavíte, a přístupové klíče |
| Klíč události rozbočovače zásad |Sdílený přístupový klíč Hello používá tooauthenticate oboru názvů Service Bus toohello přístup |
| Sloupec klíče oddílu [Nepovinné] |Tento sloupec obsahuje hello klíč oddílu centra událostí výstupu. |
| Formát serializace událostí |Formát serializace pro výstupní data.  Jsou podporovány JSON, CSV a Avro. |
| Encoding |Pro sdílený svazek clusteru a JSON UTF-8 je hello pouze v současné době podporovaný formát kódování |
| Oddělovač |Platí jenom pro serializaci sdílených svazků clusteru. Stream Analytics podporuje celou řadu běžných oddělovačů pro serializaci dat ve formátu CSV. Podporované hodnoty jsou čárkami, středník, místo, karta a svislá čára. |
| Formát |Platí jenom pro typ formátu JSON. Řádcích: Určuje, že se bude formátovat výstup hello tak, že každý objekt JSON oddělených nový řádek. Pole určuje, že hello výstup naformátovaný jako pole objektů JSON. |

## <a name="power-bi"></a>Power BI
[Power BI](https://powerbi.microsoft.com/) lze použít jako výstup pro tooprovide úlohy Stream Analytics prostředí bohaté vizualizace výsledků analýzy. Tato funkce slouží pro provozní řídicí panely, generování sestav a metrika řízené generování sestav.

### <a name="authorize-a-power-bi-account"></a>Autorizovat účet Power BI
1. Pokud Power BI je vybraná jako výstupu v hello portálu Azure, budete mít výzvami tooauthorize existujícímu uživateli Power BI nebo toocreate nový účet Power BI.  
   
   ![Autorizace uživatele Power BI](./media/stream-analytics-define-outputs/01-stream-analytics-define-outputs.png)  
2. Vytvořte nový účet, pokud nemáte ještě mít jeden a pak klikněte na autorizovat.  Zobrazí obrazovka jako následující hello.  
   
   ![Účet Azure Power BI](./media/stream-analytics-define-outputs/02-stream-analytics-define-outputs.png)  
3. V tomto kroku zadejte hello pracovní nebo školní účet pro ověřování výstup hello Power BI. Pokud nejsou se už přihlásili pro Power BI, zvolte přihlašovací nahoru. Hello pracovní nebo školní účet, který použijete pro Power BI může lišit od hello účet předplatného Azure, který jste právě přihlášení s.

### <a name="configure-hello-power-bi-output-properties"></a>Konfigurovat vlastnosti výstup hello Power BI
Až budete mít účet Power BI hello ověřený, můžete nakonfigurovat vlastnosti hello pro výstup Power BI. Následující tabulka Hello je hello seznam názvů vlastností a jejich popis tooconfigure výstupu Power BI.

| Název vlastnosti | Popis |
| --- | --- |
| Alias pro výstup |Toto je popisný název používaný v dotazy toodirect hello dotazu výstup toothis PowerBI výstup. |
| Pracovní prostor skupiny |tooenable sdílení dat s jinými uživateli Power BI můžete vybrat účet skupiny v Power BI, nebo zvolte "Pracovní prostor", pokud nechcete, aby toowrite tooa skupiny.  Aktualizuje existující skupinu vyžaduje obnovení hello Power BI ověřování. |
| Název datové sady |Zadejte, že název datové sady, že se požaduje pro hello Power BI výstup toouse |
| Název tabulky |Zadejte název tabulky v rámci hello datové sady hello výstup Power BI. V současné době Power BI výstup z úlohy Stream Analytics může mít pouze jednu tabulku v datové sadě |

Návod konfigurace výstup Power BI a řídicí panel, najdete v tématu hello [Azure Stream Analytics & Power BI](stream-analytics-power-bi-dashboard.md) článku.

> [!NOTE]
> Nevytvářejte explicitně hello datovou sadu a tabulkou v řídicí panel Power BI hello. Hello datovou sadu a tabulka bude automaticky vyplňovat při spuštění úlohy hello a hello úloha spustí čerpací výstup do Power BI. Pamatujte, že pokud dotaz úlohy hello není generovat žádné výsledky, hello datovou sadu a tabulky se nevytvoří. Také pamatujte, že pokud Power BI již bylo na datovou sadu a tabulku s hello stejný název jako hello jeden zadaný v této úloze Stream Analytics, přepíše stávající data hello.
> 
> 

### <a name="schema-creation"></a>Vytvoření schématu
Azure Stream Analytics vytvoří na datovou sadu Power BI a tabulku jménem uživatele hello, pokud ještě neexistuje. Ve všech ostatních případech se aktualizuje hello tabulku s novými hodnotami. V současné době není hello omezení že pouze jedna tabulka může existovat v rámci datové sady.

### <a name="data-type-conversion-from-asa-toopower-bi"></a>Datový typ – převod z ASA tooPower BI
Azure Stream Analytics aktualizuje hello datový model dynamicky za běhu, pokud hello výstup změny schématu. Změny názvu sloupce, sloupec typu změny a hello přidání nebo odebrání sloupců sledovány všechny.

Tato tabulka obsahuje hello konverze datových typů z [Stream Analytics datové typy](https://msdn.microsoft.com/library/azure/dn835065.aspx) tooPower BIs [Entity Data Model (EDM) typy](https://powerbi.microsoft.com/documentation/powerbi-developer-walkthrough-push-data/) Pokud na datovou sadu POWER BI a tabulku neexistují.


Ze služby Stream Analytics | tooPower BI
-----|-----|------------
bigint | Int64
nvarchar(max) | Řetězec
Data a času | Data a času
Plovoucí desetinná čárka | Double
Pole záznamu | Řetězec typu konstantní hodnoty "IRecord" nebo "IArray"

### <a name="schema-update"></a>Aktualizace schématu
Stream Analytics odvodí schéma modelu dat hello podle hello první sadu událostí ve výstupu hello. V případě potřeby je schéma modelu dat hello později, aktualizované tooaccommodate příchozí události, které nemusí začlenit do původní schématu hello.

Hello `SELECT *` dotazu je nutno aktualizace dynamické schématu tooprevent více řádků. Kromě toho toopotential ovlivnit výkon, to může také způsobit indeterminacy hello doba hello výsledky. měla by být vybrána Hello přesný pole, které je třeba toobe zobrazený na řídicí panel Power BI. Hodnoty data hello kromě toho by měla být kompatibilní s hello vybrali datový typ.


Předchozí/aktuální | Int64 | Řetězec | Data a času | Double
-----------------|-------|--------|----------|-------
Int64 | Int64 | Řetězec | Řetězec | Double
Double | Double | Řetězec | Řetězec | Double
Řetězec | Řetězec | Řetězec | Řetězec |  | Řetězec | 
Data a času | Řetězec | Řetězec |  Data a času | Řetězec


### <a name="renew-power-bi-authorization"></a>Obnovit autorizace Power BI
Budete potřebovat toore-ověřit svůj účet Power BI, pokud došlo ke změně jeho heslo vzhledem k tomu, že vaše úlohy vytvoření nebo poslední ověření. Pokud je na vašeho klienta Azure Active Directory (AAD) nakonfigurovaná služba Multi-Factor Authentication (MFA) je nutné také toorenew Power BI autorizace každé 2 týdny. Příznakem tohoto problému je žádný výstup úlohy a "ověřit uživatele chyba" v protokoly operací hello:

  ![Chyba aktualizace tokenu Power BI](./media/stream-analytics-define-outputs/03-stream-analytics-define-outputs.png)  

tooresolve tento problém, zastavte spuštěná úloha a přejděte tooyour výstup Power BI.  Kliknutím na odkaz "Autorizace obnovení" hello a restartujte úlohu z hello ztráty dat tooavoid naposledy zastaveno.

  ![Power BI obnovit ověřování](./media/stream-analytics-define-outputs/04-stream-analytics-define-outputs.png)  

## <a name="table-storage"></a>Table Storage
[Azure Table storage](../storage/common/storage-introduction.md) nabízí vysoce dostupné, enormně škálovatelné úložiště, takže aplikace může automaticky škálovat toomeet žádost uživatele. Úložiště Table je úložiště klíčů/atributů NoSQL společnosti Microsoft, která můžete využít pro strukturovaná data s méně omezení hello schématu. Azure Table storage lze použít toostore data pro trvalosti a efektivní načtení.

Následující tabulka Hello uvádí hello názvy vlastností a jejich popis vytváření výstupní tabulka.

| Název vlastnosti | Popis |
| --- | --- |
| Alias pro výstup |Toto je popisný název používaný v dotazy toodirect hello dotazu výstup toothis tabulka úložiště. |
| Účet úložiště |Hello název účtu úložiště hello kde odesílání výstupu. |
| Klíče účtu úložiště. |přístupový klíč Hello přidružené k účtu úložiště hello. |
| Název tabulky |Hello název tabulky hello. Tabulka Hello získat vytvoří, pokud neexistuje. |
| Klíč oddílu |Název Hello hello výstupního sloupce obsahujícího hello klíč oddílu. klíč oddílu Hello je jedinečný identifikátor pro hello oddíl v dané tabulce, která tvoří hello první část primárního klíče entity. Je řetězcová hodnota, kterou může být až velikost too1 KB. |
| Klíč řádku |Název Hello hello výstupního sloupce obsahujícího hello klíč řádku. Hello klíč řádku je jedinečný identifikátor entity v daném oddílu. Tvoří hello druhou část primárního klíče entity. Hello klíč řádku je řetězcová hodnota, kterou může být až velikost too1 KB. |
| Velikost dávky |Hello počet záznamů pro dávkové operace. Toohello výchozí hello je obvykle dostatečné pro většinu úloh, najdete v [tabulky dávkovou operaci specifikace](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.aspx) další podrobnosti o úpravě tohoto nastavení. |

## <a name="service-bus-queues"></a>Fronty služby Service Bus
[Fronty služby Service Bus](https://msdn.microsoft.com/library/azure/hh367516.aspx) nabízejí First In tooone doručení zpráv první Out (FIFO) nebo další konkurenčních spotřebitelů. Zprávy jsou obvykle očekávané toobe přijme a zpracuje hello příjemci v hello dočasné pořadí, ve kterém byly přidány toohello fronty, a každou zprávu přijme a zpracuje jenom jeden příjemce zprávy.

Následující tabulka Hello uvádí hello názvy vlastností a jejich popis vytváření výstupní fronty.

| Název vlastnosti | Popis |
| --- | --- |
| Alias pro výstup |Toto je popisný název používaný v dotazy toodirect hello dotazu výstup toothis frontou Service Bus. |
| Namespace Service Bus |Obor názvů sběrnice je kontejner sady entit pro zasílání zpráv. |
| Název fronty |Název Hello hello frontou Service Bus. |
| Název zásady fronty |Když vytvoříte frontu, můžete také vytvořit zásady sdíleného přístupu na kartě Konfigurace fronty hello. Každá zásada sdíleného přístupu bude mít název, oprávnění, která nastavíte, a přístupové klíče. |
| Klíč zásad fronty |Sdílený přístupový klíč Hello používá tooauthenticate oboru názvů Service Bus toohello přístup |
| Formát serializace událostí |Formát serializace pro výstupní data.  Jsou podporovány JSON, CSV a Avro. |
| Encoding |Pro sdílený svazek clusteru a JSON UTF-8 je hello pouze v současné době podporovaný formát kódování |
| Oddělovač |Platí jenom pro serializaci sdílených svazků clusteru. Stream Analytics podporuje celou řadu běžných oddělovačů pro serializaci dat ve formátu CSV. Podporované hodnoty jsou čárkami, středník, místo, karta a svislá čára. |
| Formát |Platí jenom pro typ formátu JSON. Řádcích: Určuje, že se bude formátovat výstup hello tak, že každý objekt JSON oddělených nový řádek. Pole určuje, že hello výstup naformátovaný jako pole objektů JSON. |

## <a name="service-bus-topics"></a>Témata služby Service Bus
Zatímco fronty služby Service Bus poskytují jeden způsob komunikace tooone z odesílatele tooreceiver [témata služby Service Bus](https://msdn.microsoft.com/library/azure/hh367516.aspx) zadejte formuláři na více komunikace.

Následující tabulka Hello uvádí hello názvy vlastností a jejich popis vytváření výstupní tabulka.

| Název vlastnosti | Popis |
| --- | --- |
| Alias pro výstup |Toto je popisný název používaný v dotazy toodirect hello dotazu výstup toothis témata sběrnice. |
| Namespace Service Bus |Obor názvů sběrnice je kontejner sady entit pro zasílání zpráv. Při vytváření nového centra událostí taky vytvoříte obor názvů sběrnice |
| Název tématu |Témata jsou zasílání zpráv entity, podobně jako tooevent rozbočovače a fronty. Jsou navrženou toocollect streamů událostí z mnoha různých zařízení a služeb. Při vytváření téma je rovněž dán určitý název. odesílají zprávy Hello tooa tématu nebudete mít k dispozici, pokud není vytvořená předplatné, zajistěte proto jsou jeden nebo více odběrů v části tématu hello |
| Název zásady tématu |Když vytvoříte téma, můžete také vytvořit zásady sdíleného přístupu na kartě konfigurace tématu hello. Každá zásada sdíleného přístupu bude mít název, oprávnění, která nastavíte, a přístupové klíče |
| Klíč tématu Zásady |Sdílený přístupový klíč Hello používá tooauthenticate oboru názvů Service Bus toohello přístup |
| Formát serializace událostí |Formát serializace pro výstupní data.  Jsou podporovány JSON, CSV a Avro. |
| Encoding |Pokud formátu CSV nebo formátu JSON, kódování musí být zadán. Znakové sady UTF-8 je hello pouze v současné době podporovaný formát kódování |
| Oddělovač |Platí jenom pro serializaci sdílených svazků clusteru. Stream Analytics podporuje celou řadu běžných oddělovačů pro serializaci dat ve formátu CSV. Podporované hodnoty jsou čárkami, středník, místo, karta a svislá čára. |

## <a name="azure-cosmos-db"></a>Azure Cosmos DB
[Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) je plně spravovaná služba databáze dokumentů NoSQL nabízí dotazu a transakce než data bez schémat, předvídatelný a spolehlivý výkon a rychlý vývoj.

Hello pod názvy vlastností hello podrobnosti seznamu a jejich popis pro vytváření výstupu Azure Cosmos DB.

* **Výstup Alias** – alias toorefer tento výstup v dotazu ASA  
* **Název účtu** – hello název nebo identifikátor URI hello Cosmos DB účet koncový bod.  
* **Účet klíč** – hello sdílený přístupový klíč pro hello účet Cosmos DB.  
* **Databáze** – název databáze hello Cosmos DB.  
* **Vzor názvu** – název kolekce hello nebo jejich vzor toobe kolekce hello používá. Formát názvu kolekce Hello lze sestavit pomocí tokenu hello volitelné {partition}, na kterém oddíly začínají od 0. Ukázka platné vstupní hodnoty jsou následující:  
  1\) kolekce – jednu kolekci s názvem "Kolekce" musí existovat.  
  2\) kolekce {partition} – takových kolekcí, musí existovat – "MyCollection0", "MyCollection1", "MyCollection2" a tak dále.  
* **Klíč oddílu** – volitelné. To je potřeba jenom Pokud používáte token {vytvoření oddílu} ve vzoru názvu vaší kolekce. Název Hello hello pole ve výstupních událostech používaný toospecify hello klíče pro rozdělení výstupu do kolekcí. Pro výstup jedinou kolekci všechny libovolný výstupního sloupce lze použít například ID oddílu.  
* **ID dokumentu** – volitelné. Název Hello hello pole ve výstupních událostech používá toospecify hello primární klíč, na které insert nebo update jsou založené operace.  


## <a name="get-help"></a>Podpora
Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky
Byli jste přináší tooStream Analytics, spravované služby pro analýzy z hello Internet věcí datových proudů. toolearn Další informace o této služby najdete v části:

* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
