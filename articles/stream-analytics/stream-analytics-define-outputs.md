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
ms.openlocfilehash: 33d0b9aa37cc92dda27f1cf21f1d393b42b8c09b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="stream-analytics-outputs-options-for-storage-analysis"></a>Stream Analytics výstupy: možnosti pro úložiště, analýzy
Při vytváření úlohy Stream Analytics, zvažte, jak budou využívat výsledných datech. Jak se vám zobrazí výsledky úlohy Stream Analytics, kam se budou ukládat?

Pokud chcete povolit různé vzorce aplikací, má Azure Stream Analytics různé možnosti pro ukládání výstupu a zobrazit výsledky analýzy. To umožňuje snadno zobrazit výstup úlohy a umožňuje flexibilitu při využívání a úložiště výstup úlohy datového skladu a jiné účely. Před zahájení úlohy a události spuštění toku musí existovat žádný výstup nakonfigurované v úloze. Například pokud použijete jako výstupu úložiště objektů Blob, úloha nebude vytvořit účet úložiště automaticky. Musí být před spuštěním úlohy ASA vytvořený uživatelem.

## <a name="azure-data-lake-store"></a>Azure Data Lake Store
Stream Analytics podporuje [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/). Toto úložiště umožňuje ukládání dat z jakékoli velikosti, typu a rychlosti příjmu pro provozní a zjišťovací analýzy. Další Stream Analytics musí mít oprávnění pro přístup k Data Lake Store. Podrobnosti o ověřování a jak se můžete zaregistrovat pro Data Lake Store (v případě potřeby), jsou popsané v [Data Lake výstup článku](stream-analytics-data-lake-output.md).

### <a name="authorize-an-azure-data-lake-store"></a>Autorizace Azure Data Lake Store
Pokud Data Lake Storage je vybraná jako výstupu na portálu Azure, budete vyzváni k autorizaci připojení k existující Data Lake Store.  

![Autorizovat Data Lake Store](./media/stream-analytics-define-outputs/06-stream-analytics-define-outputs.png)  

Pak vyplňte vlastnosti pro Data Lake Store výstup jak vidíte níže:

![Autorizovat Data Lake Store](./media/stream-analytics-define-outputs/07-stream-analytics-define-outputs.png)  

Následující tabulka uvádí názvy vlastností a jejich popis potřebné pro vytvoření výstupní Data Lake Store.

<table>
<tbody>
<tr>
<td><B>NÁZEV VLASTNOSTI</B></td>
<td><B>POPIS</B></td>
</tr>
<tr>
<td>Alias pro výstup</td>
<td>Toto je popisný název používaný v dotazech na přesměrujte výstup dotazu do tohoto Data Lake Store.</td>
</tr>
<tr>
<td>Název účtu</td>
<td>Název účtu Data Lake Storage, kde jsou odesílání výstupu. Zobrazí se rozevírací seznam účtů Data Lake Store, ke kterým má uživatel přihlásí k portálu přístup k.</td>
</tr>
<tr>
<td>Předpona vzorek cesty</td>
<td>Pojmenovávání souborů bude podle následující konvence: <BR>{Cesta předponu Pattern}/schemaHashcode_Guid_Number.extension <BR> <BR>Příklad výstupní soubory:<BR>Myoutput/20170901/00/45434_gguid_1.csv <BR>Myoutput/20170901/01/45434_gguid_1.csv <BR> <BR>Zde jsou také situacích, kde se má vytvořit nový soubor:<BR>1.Změnit ve výstupu schématu <BR>2.Vnější nebo vnitřní restartovat úlohy<BR><BR>Kromě toho pokud vzorek cesty soubor neobsahuje koncový znak "/", poslední vzor v cestě k souboru budou zpracovány jako předponu názvu souboru.<BR><BR>Příklad:<BR>Pro vzorek cesty: složku1/logs/HH, vygenerovaný soubor bude vypadat podobně jako: folder1/logs/02_134343_gguid_1.csv</td>
</tr>
<tr>
<td>Datum formátu [<I>volitelné</I>]</td>
<td>Pokud se v cestě předponu používá token kalendářního data, můžete vybrat formát data, ve kterém jsou uspořádány soubory. Příklad: Rrrr/MM/DD</td>
</tr>
<tr>
<td>Formát času [<I>volitelné</I>]</td>
<td>Pokud token čas se používá v cestě předponu, zadejte formát času, ve kterém jsou uspořádány soubory. Aktuálně jedinou podporovanou hodnotou je HH.</td>
</tr>
<tr>
<td>Formát serializace událostí</td>
<td>Formát serializace pro výstupní data. Jsou podporovány JSON, CSV a Avro.</td>
</tr>
<tr>
<td>Encoding</td>
<td>Pokud formátu CSV nebo formátu JSON, kódování musí být zadán. V tuto chvíli je jediným podporovaným formátem kódování UTF-8.</td>
</tr>
<tr>
<td>Oddělovač</td>
<td>Platí jenom pro serializaci sdílených svazků clusteru. Stream Analytics podporuje řadu běžných oddělovačů pro serializaci dat sdíleného svazku clusteru. Podporované hodnoty jsou čárkami, středník, místo, karta a svislá čára.</td>
</tr>
<tr>
<td>Formát</td>
<td>Platí jenom pro serializaci JSON. Řádcích: Určuje, že výstup se bude formátovat tak, že každý objekt JSON oddělených nový řádek. Pole určuje, že výstup naformátovaný jako pole objektů JSON. Toto pole se zavřou jenom v případě, že je úloha pozastavena nebo Stream Analytics se přesunul další časový interval. Obecně platí, je vhodnější použít řádku oddělené formát JSON, protože nevyžaduje žádné zvláštní zpracování, při výstupní soubor je stále zápisu do.</td>
</tr>
</tbody>
</table>

### <a name="renew-data-lake-store-authorization"></a>Obnovit autorizační Data Lake Store
Musíte se znovu provést ověření účtu Data Lake Store, pokud došlo ke změně jeho heslo vzhledem k tomu, že vaše úlohy vytvoření nebo poslední ověření.

![Autorizovat Data Lake Store](./media/stream-analytics-define-outputs/08-stream-analytics-define-outputs.png)  

## <a name="sql-database"></a>SQL Database
[Azure SQL Database](https://azure.microsoft.com/services/sql-database/) lze použít jako výstup pro data, která je povahou relační, nebo pro aplikace, které závisí na obsahu hostovaném v relační databázi. Úlohy Stream Analytics bude zapisovat do existující tabulky v databázi SQL Azure.  Všimněte si, že schéma tabulky musí přesně shodovat pole a jejich typy se výstup z úlohy. [Azure SQL Data Warehouse](https://azure.microsoft.com/documentation/services/sql-data-warehouse/) dá se zadat taky jako výstupu prostřednictvím SQL Database možnost output také (Toto je funkce preview). Následující tabulka uvádí názvy vlastností a jejich popis pro vytvoření databáze SQL výstup.

| Název vlastnosti | Popis |
| --- | --- |
| Alias pro výstup |Toto je popisný název používaný v dotazech na přesměrujte výstup dotazu do této databáze. |
| Databáze |Název databáze, kde jsou odesílání výstupu |
| Název serveru |Název databáze SQL serveru |
| Uživatelské jméno |Uživatelské jméno, který má přístup k zápisu do databáze |
| Heslo |Heslo pro připojení k databázi |
| Table |Název tabulky, kde bude zapsán výstup. Název tabulky je velká a malá písmena a schéma této tabulky by se měl shodovat přesně počet polí a jejich typy generovaných výstupu úlohy. |

> [!NOTE]
> Pro výstup úlohy v Stream Analytics je aktuálně podporuje nabídku Azure SQL Database. Virtuální počítač Azure s systému SQL Server databáze připojené však není podporován. Toto je může v budoucích verzích změnit.
> 
> 

## <a name="blob-storage"></a>Blob Storage
Úložiště BLOB nabízí cenově výhodné a škálovatelné řešení pro ukládání velkého objemu nestrukturovaných dat v cloudu.  Úvod do Azure Blob storage a jeho použití, najdete v dokumentaci na [jak používat objekty BLOB](../storage/blobs/storage-dotnet-how-to-use-blobs.md).

Následující tabulka uvádí názvy vlastností a jejich popis vytvoření výstupního objektu blob.

<table>
<tbody>
<tr>
<td>NÁZEV VLASTNOSTI</td>
<td>POPIS</td>
</tr>
<tr>
<td>Alias pro výstup</td>
<td>Toto je popisný název používaný v dotazech na přesměrujte výstup dotazu do tohoto úložiště objektů blob.</td>
</tr>
<tr>
<td>Účet úložiště</td>
<td>Název účtu úložiště, kde jsou odesílání výstupu.</td>
</tr>
<tr>
<td>Klíče účtu úložiště.</td>
<td>Tajný klíč přidružený k účtu úložiště.</td>
</tr>
<tr>
<td>Kontejner úložiště</td>
<td>Kontejnery poskytují možnost logického seskupování pro objekty BLOB uložené ve službě Microsoft Azure Blob. Při nahrávání do objektu blob ve službě Blob, je nutné zadat kontejner pro tento objekt blob.</td>
</tr>
<tr>
<td>Předpony vzorek cesty [Nepovinné]</td>
<td>Vzor cesty souborů používá k zápisu objektů BLOB v rámci zadaného kontejneru. <BR> Ve vzoru cestu můžete použít jednu nebo více instancí následujících 2 proměnných pro určení četnosti, které jsou napsané objekty BLOB: <BR> {date}, {time} <BR> Příklad 1: cluster1/logs / {date} / {time} <BR> Příklad 2: cluster1/logs / {date} <BR> <BR> Pojmenovávání souborů bude podle následující konvence: <BR> {Cesta předponu Pattern}/schemaHashcode_Guid_Number.extension <BR> <BR> Příklad výstupní soubory: <BR> Myoutput/20170901/00/45434_gguid_1.csv <BR> Myoutput/20170901/01/45434_gguid_1.csv <BR> <BR> Zde jsou také situacích, kde se má vytvořit nový soubor: <BR> 1.Aktuální soubor překračuje maximální povolený počet bloků (aktuálně 50 000) <BR> 2.Změnit ve výstupu schématu <BR> 3.Vnější nebo vnitřní restartování úlohy  </td>
</tr>
<tr>
<td>[Nepovinné] formát data</td>
<td>Pokud se v cestě předponu používá token kalendářního data, můžete vybrat formát data, ve kterém jsou uspořádány soubory. Příklad: Rrrr/MM/DD</td>
</tr>
<tr>
<td>Formát času [Nepovinné]</td>
<td>Pokud token čas se používá v cestě předponu, zadejte formát času, ve kterém jsou uspořádány soubory. Aktuálně jedinou podporovanou hodnotou je HH.</td>
</tr>
<tr>
<td>Formát serializace událostí</td>
<td>Formát serializace pro výstupní data.  Jsou podporovány JSON, CSV a Avro.</td>
</tr>
<tr>
<td>Encoding</td>
<td>Pokud formátu CSV nebo formátu JSON, kódování musí být zadán. V tuto chvíli je jediným podporovaným formátem kódování UTF-8.</td>
</tr>
<tr>
<td>Oddělovač</td>
<td>Platí jenom pro serializaci sdílených svazků clusteru. Stream Analytics podporuje řadu běžných oddělovačů pro serializaci dat sdíleného svazku clusteru. Podporované hodnoty jsou čárkami, středník, místo, karta a svislá čára.</td>
</tr>
<tr>
<td>Formát</td>
<td>Platí jenom pro serializaci JSON. Řádcích: Určuje, že výstup se bude formátovat tak, že každý objekt JSON oddělených nový řádek. Pole určuje, že výstup naformátovaný jako pole objektů JSON. Toto pole se zavřou jenom v případě, že je úloha pozastavena nebo Stream Analytics se přesunul další časový interval. Obecně platí, je vhodnější použít řádku oddělené formát JSON, protože nevyžaduje žádné zvláštní zpracování, při výstupní soubor je stále zápisu do.</td>
</tr>
</tbody>
</table>

## <a name="event-hub"></a>Centrum událostí
[Služba Event Hubs](https://azure.microsoft.com/services/event-hubs/) je vysoce škálovatelná publikování a odběru na přijímač událostí. Ho může shromažďovat miliony událostí za sekundu.  Jedno použití centra událostí jako výstup je při výstup úlohy Stream Analytics bude vstup jinou úlohu streamování.

Existuje několik parametrů, které jsou potřeba ke konfiguraci datové proudy centra událostí jako výstup.

| Název vlastnosti | Popis |
| --- | --- |
| Alias pro výstup |Toto je popisný název používaný v dotazech na přesměrujte výstup dotazu do tohoto centra událostí. |
| Namespace Service Bus |Obor názvů sběrnice je kontejner sady entit pro zasílání zpráv. Při vytváření nového centra událostí taky vytvoříte obor názvů sběrnice |
| Centrum událostí |Název výstupu centra událostí |
| Název zásady centra událostí |Zásady sdíleného přístupu, které se dají vytvořit na kartě Konfigurace centra událostí. Každá zásada sdíleného přístupu bude mít název, oprávnění, která nastavíte, a přístupové klíče |
| Klíč události rozbočovače zásad |Sdílený přístupový klíč použitý k ověření přístupu k oboru názvů Service Bus |
| Sloupec klíče oddílu [Nepovinné] |Tento sloupec obsahuje klíč oddílu centra událostí výstupu. |
| Formát serializace událostí |Formát serializace pro výstupní data.  Jsou podporovány JSON, CSV a Avro. |
| Encoding |Pro sdílený svazek clusteru a JSON UTF-8 v tuto chvíli je jediným podporovaným formátem kódování |
| Oddělovač |Platí jenom pro serializaci sdílených svazků clusteru. Stream Analytics podporuje celou řadu běžných oddělovačů pro serializaci dat ve formátu CSV. Podporované hodnoty jsou čárkami, středník, místo, karta a svislá čára. |
| Formát |Platí jenom pro serializaci JSON. Řádcích: Určuje, že výstup se bude formátovat tak, že každý objekt JSON oddělených nový řádek. Pole určuje, že výstup naformátovaný jako pole objektů JSON. Toto pole se zavřou jenom v případě, že je úloha pozastavena nebo Stream Analytics se přesunul další časový interval. Obecně platí, je vhodnější použít řádku oddělené formát JSON, protože nevyžaduje žádné zvláštní zpracování, při výstupní soubor je stále zápisu do. |

## <a name="power-bi"></a>Power BI
[Power BI](https://powerbi.microsoft.com/) lze použít jako výstup úlohy Stream Analytics k poskytování prostředí bohaté vizualizace výsledků analýzy. Tato funkce slouží pro provozní řídicí panely, generování sestav a metrika řízené generování sestav.

### <a name="authorize-a-power-bi-account"></a>Autorizovat účet Power BI
1. Pokud Power BI je vybraná jako výstupu na portálu Azure, zobrazí se výzva k autorizaci stávajícímu uživateli Power BI nebo vytvořte nový účet Power BI.  
   
   ![Autorizace uživatele Power BI](./media/stream-analytics-define-outputs/01-stream-analytics-define-outputs.png)  
2. Vytvořte nový účet, pokud nemáte ještě mít jeden a pak klikněte na autorizovat.  Zobrazí obrazovka podobně jako tento.  
   
   ![Účet Azure Power BI](./media/stream-analytics-define-outputs/02-stream-analytics-define-outputs.png)  
3. V tomto kroku zadejte pracovní nebo školní účet pro ověřování výstup Power BI. Pokud nejsou se už přihlásili pro Power BI, zvolte přihlašovací nahoru. Může být liší od účtu předplatného Azure, který jste právě přihlášení pomocí pracovního nebo školního účtu, který použijete pro Power BI.

### <a name="configure-the-power-bi-output-properties"></a>Konfigurovat vlastnosti výstup Power BI
Až budete mít účet Power BI ověřený, můžete konfigurovat vlastnosti pro výstup Power BI. V následující tabulce je seznam názvů vlastností a jejich popis konfigurace výstupu Power BI.

| Název vlastnosti | Popis |
| --- | --- |
| Alias pro výstup |Toto je popisný název používaný v dotazech na přesměrujte výstup dotaz na tento výstup PowerBI. |
| Pracovní prostor skupiny |Chcete-li povolit sdílení dat s dalšími uživateli Power BI můžete vybrat skupiny uvnitř svůj účet Power BI nebo zvolte "Pracovní prostor", pokud nechcete pro zápis do skupiny.  Aktualizuje existující skupinu vyžaduje obnovování ověřování Power BI. |
| Název datové sady |Zadejte název datové sady, který je požadován pro výstup Power BI k použití |
| Název tabulky |Zadejte název tabulky v rámci datové sady výstup Power BI. V současné době Power BI výstup z úlohy Stream Analytics může mít pouze jednu tabulku v datové sadě |

Návod konfigurace výstup Power BI a řídicí panel, najdete v tématu [Azure Stream Analytics & Power BI](stream-analytics-power-bi-dashboard.md) článku.

> [!NOTE]
> Nevytvářejte explicitně datovou sadu a tabulkou v řídicím panelu Power BI. Datovou sadu a tabulka bude automaticky vyplňovat při zahájení úlohy a úlohy spustí čerpací výstup do Power BI. Všimněte si, že pokud dotaz úlohy není generovat výsledky, datové sady a tabulka nevytvoří. Také mějte na paměti, že pokud Power BI již bylo datovou sadu a tabulka se stejným názvem jako ten, který je součástí této úlohy Stream Analytics, stávající data budou přepsány.
> 
> 

### <a name="schema-creation"></a>Vytvoření schématu
Azure Stream Analytics vytvoří na datovou sadu Power BI a tabulku jménem uživatele, pokud ještě neexistuje. Ve všech ostatních případech se v tabulce aktualizuje s novými hodnotami. V současné době není omezení, které pouze jedna tabulka může existovat v rámci datové sady.

### <a name="data-type-conversion-from-asa-to-power-bi"></a>Datový typ – převod z ASA k Power BI.
Azure Stream Analytics aktualizuje datový model dynamicky za běhu, pokud se změní schéma výstupu. Změny názvu sloupce, sloupec typu změny a přidání nebo odebrání sloupců sledovány všechny.

Tato tabulka obsahuje konverze datových typů z [Stream Analytics datové typy](https://msdn.microsoft.com/library/azure/dn835065.aspx) k Power BIs [Entity Data Model (EDM) typy](https://powerbi.microsoft.com/documentation/powerbi-developer-walkthrough-push-data/) Pokud na datovou sadu POWER BI a tabulku neexistují.


Ze služby Stream Analytics | K Power BI.
-----|-----|------------
bigint | Int64
nvarchar(max) | Řetězec
Data a času | Data a času
Plovoucí desetinná čárka | Double
Pole záznamu | Řetězec typu konstantní hodnoty "IRecord" nebo "IArray"

### <a name="schema-update"></a>Aktualizace schématu
Stream Analytics odvodí schéma modelu dat podle první sadu událostí ve výstupu. Později v případě potřeby schéma modelu dat se aktualizuje zohlednit příchozí události, které nemusí začlenit do původní schématu.

`SELECT *` Dotazu je nutno, aby se zabránilo aktualizace dynamické schématu více řádků. Kromě potenciální ovlivnit výkon to může také způsobit indeterminacy čas potřebný pro dané výsledky. Měla by být vybrána přesný pole, které je potřeba se zobrazí na řídicí panel Power BI. Kromě toho musí být hodnoty data kompatibilní s vybraným datovým typem.


Předchozí/aktuální | Int64 | Řetězec | Data a času | Double
-----------------|-------|--------|----------|-------
Int64 | Int64 | Řetězec | Řetězec | Double
Double | Double | Řetězec | Řetězec | Double
Řetězec | Řetězec | Řetězec | Řetězec |  | Řetězec | 
Data a času | Řetězec | Řetězec |  Data a času | Řetězec


### <a name="renew-power-bi-authorization"></a>Obnovit autorizace Power BI
Bude muset znovu ověřit svůj účet Power BI, pokud došlo ke změně jeho heslo vzhledem k tomu, že vaše úlohy vytvoření nebo poslední ověření. Pokud aplikace Multi-Factor Authentication (MFA) je nakonfigurován na vašeho klienta Azure Active Directory (AAD), budete také muset obnovit Power BI autorizace každé 2 týdny. Příznakem tohoto problému je žádný výstup úlohy a "ověřit uživatele chyba" v protokoly operací:

  ![Chyba aktualizace tokenu Power BI](./media/stream-analytics-define-outputs/03-stream-analytics-define-outputs.png)  

Chcete-li vyřešit tento problém, přejděte na výstupu Power BI a zastavit spuštěná úloha.  Klikněte na odkaz "Autorizace obnovení" a restartujte úlohu od poslední zastavena času, abyste se vyhnuli ztrátě dat.

  ![Power BI obnovit ověřování](./media/stream-analytics-define-outputs/04-stream-analytics-define-outputs.png)  

## <a name="table-storage"></a>Table Storage
[Azure Table storage](../storage/common/storage-introduction.md) nabízí vysoce dostupné, enormně škálovatelné úložiště, takže aplikace může automaticky škálovat podle požadavků uživatelů. Úložiště Table je úložiště klíčů/atributů NoSQL společnosti Microsoft, která můžete využít pro strukturovaná data s méně omezení na schéma. Azure Table storage můžete použít k ukládání dat pro trvalosti a efektivní načtení.

Následující tabulka uvádí názvy vlastností a jejich popis vytváření výstupní tabulka.

| Název vlastnosti | Popis |
| --- | --- |
| Alias pro výstup |Toto je popisný název používaný v dotazech na přesměrujte výstup dotazu do této tabulky úložiště. |
| Účet úložiště |Název účtu úložiště, kde jsou odesílání výstupu. |
| Klíče účtu úložiště. |Přístupový klíč přidružený k účtu úložiště. |
| Název tabulky |Název tabulky. Pokud neexistuje budou vytvořeny v tabulce. |
| Klíč oddílu |Název výstupního sloupce obsahujícího klíč oddílu. Klíč oddílu je jedinečný identifikátor pro oddíl v dané tabulce, která tvoří první část primárního klíče entity. Je řetězcovou hodnotu, která může mít velikost až 1 KB. |
| Klíč řádku |Název výstupního sloupce obsahujícího klíč řádku. Klíč řádku je jedinečný identifikátor entity v daném oddílu. Tvoří druhou část primárního klíče entity. Klíč řádku je řetězcová hodnota, která může mít velikost až 1 KB. |
| Velikost dávky |Počet záznamů pro dávkové operace. Výchozí hodnota je obvykle dostatečné pro většinu úloh, najdete [specifikace tabulky dávkovou operaci](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.aspx) další podrobnosti o úpravě toto nastavení. |
 
## <a name="service-bus-queues"></a>Fronty služby Service Bus
[Fronty služby Service Bus](https://msdn.microsoft.com/library/azure/hh367516.aspx) nabízejí First In doručení zpráv první Out (FIFO) na jeden nebo několik konkurenčních spotřebitelů. Obvykle se očekává zprávy, přijímají a zpracovávají v pořadí, ve kterém byly přidány do fronty, a každou zprávu přijme a zpracuje jenom jeden příjemce zprávy.

Následující tabulka uvádí názvy vlastností a jejich popis vytváření výstupní fronty.

| Název vlastnosti | Popis |
| --- | --- |
| Alias pro výstup |Toto je popisný název používaný v dotazech na přesměrujte výstup dotazu do této fronty Service Bus. |
| Namespace Service Bus |Obor názvů sběrnice je kontejner sady entit pro zasílání zpráv. |
| Název fronty |Název fronty Service Bus. |
| Název zásady fronty |Když vytvoříte frontu, můžete také vytvořit zásady sdíleného přístupu na kartě Konfigurace fronty. Každá zásada sdíleného přístupu bude mít název, oprávnění, která nastavíte, a přístupové klíče. |
| Klíč zásad fronty |Sdílený přístupový klíč použitý k ověření přístupu k oboru názvů Service Bus |
| Formát serializace událostí |Formát serializace pro výstupní data.  Jsou podporovány JSON, CSV a Avro. |
| Encoding |Pro sdílený svazek clusteru a JSON UTF-8 v tuto chvíli je jediným podporovaným formátem kódování |
| Oddělovač |Platí jenom pro serializaci sdílených svazků clusteru. Stream Analytics podporuje celou řadu běžných oddělovačů pro serializaci dat ve formátu CSV. Podporované hodnoty jsou čárkami, středník, místo, karta a svislá čára. |
| Formát |Platí jenom pro typ formátu JSON. Řádcích: Určuje, že výstup se bude formátovat tak, že každý objekt JSON oddělených nový řádek. Pole určuje, že výstup naformátovaný jako pole objektů JSON. |

## <a name="service-bus-topics"></a>Témata služby Service Bus
Zatímco fronty služby Service Bus poskytují způsob komunikace k jedné z odesílatele k příjemce, [témata služby Service Bus](https://msdn.microsoft.com/library/azure/hh367516.aspx) zadejte formuláři na více komunikace.

Následující tabulka uvádí názvy vlastností a jejich popis vytváření výstupní tabulka.

| Název vlastnosti | Popis |
| --- | --- |
| Alias pro výstup |Toto je popisný název používaný v dotazech na přesměrujte výstup dotazu k tomuto tématu Service Bus. |
| Namespace Service Bus |Obor názvů sběrnice je kontejner sady entit pro zasílání zpráv. Při vytváření nového centra událostí taky vytvoříte obor názvů sběrnice |
| Název tématu |Témata jsou entity pro zasílání zpráv podobné centrům událostí a frontám. Jsou navržené ke shromažďování streamů událostí z mnoha různých zařízení a služeb. Při vytváření téma je rovněž dán určitý název. Zprávy odeslané do tématu nebudou k dispozici, pokud předplatné není vytvořená, tak zajistěte, jeden nebo více odběrů v tématu |
| Název zásady tématu |Když vytvoříte téma, můžete také vytvořit zásady sdíleného přístupu na kartě konfigurace tématu. Každá zásada sdíleného přístupu bude mít název, oprávnění, která nastavíte, a přístupové klíče |
| Klíč tématu Zásady |Sdílený přístupový klíč použitý k ověření přístupu k oboru názvů Service Bus |
| Formát serializace událostí |Formát serializace pro výstupní data.  Jsou podporovány JSON, CSV a Avro. |
 | Encoding |Pokud formátu CSV nebo formátu JSON, kódování musí být zadán. Znakové sady UTF-8 v tuto chvíli je jediným podporovaným formátem kódování |
| Oddělovač |Platí jenom pro serializaci sdílených svazků clusteru. Stream Analytics podporuje celou řadu běžných oddělovačů pro serializaci dat ve formátu CSV. Podporované hodnoty jsou čárkami, středník, místo, karta a svislá čára. |

## <a name="azure-cosmos-db"></a>Azure Cosmos DB
[Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) je globálně distribuované databáze, více modelu služby, aby neomezenou elastické škálování nabízí kolem celém světě, plnohodnotný dotazovací a automatické indexování přes vázané na schéma datové modely, garantované s nízkou latencí a špičkový komplexní SLA.

Následujícím seznamu podrobnosti názvy vlastností a jejich popis pro vytváření výstupu Azure Cosmos DB.

* **Výstup Alias** – na alias odkazovat tento výstup v dotazu ASA  
* **Název účtu** – název nebo identifikátor URI účtu Cosmos DB koncového bodu.  
* **Účet klíč** – sdílený přístupový klíč pro účet Cosmos DB.  
* **Databáze** – název databáze DB Cosmos.  
* **Vzor názvu** – název kolekce nebo jejich vzor pro kolekce, který se má použít. Formát názvu kolekce se dá vytvořit pomocí tokenu volitelná {partition}, na kterém oddíly začínají od 0. Ukázka platné vstupní hodnoty jsou následující:  
  1\) kolekce – jednu kolekci s názvem "Kolekce" musí existovat.  
  2\) kolekce {partition} – takových kolekcí, musí existovat – "MyCollection0", "MyCollection1", "MyCollection2" a tak dále.  
* **Klíč oddílu** – volitelné. To je potřeba jenom Pokud používáte token {vytvoření oddílu} ve vzoru názvu vaší kolekce. Název pole ve výstupních událostech používaný k určení klíče pro rozdělení výstupu do kolekcí Pro výstup jedinou kolekci všechny libovolný výstupního sloupce lze použít například ID oddílu.  
* **ID dokumentu** – volitelné. Název pole ve výstupních událostech používaný k určení primárního klíče, na které insert nebo update jsou založené operace.  

## <a name="azure-functions-in-preview"></a>Azure Functions (ve verzi Preview)
Azure Functions je výpočetní služba bez serveru umožňující spouštění kódu na vyžádání bez nutnosti explicitně zřizovat nebo spravovat infrastrukturu. Umožňuje implementovat kód, který je aktivovaného událostmi, ke kterým dochází v Azure nebo služby třetích stran.  Tato schopnost Azure Functions reagovat na aktivační události usnadňuje přirozené výstup Azure Stream Analytics. Tento výstup adaptér umožňuje uživatelům připojení k Azure Functions Stream Analytics a spusťte skript nebo část kódu v reakci na celou řadu událostí.

Azure Stream Analytics vyvolá Azure Functions prostřednictvím protokolu HTTP aktivační události. Nový adaptér výstup funkce Azure je k dispozici následující konfigurovatelné vlastnosti:

| Název vlastnosti | Popis |
| --- | --- |
| Funkce aplikace |Název aplikace Azure Functions |
| Funkce |Název funkce v aplikaci funkce Azure |
| Velikost dávky maximální |Tuto vlastnost lze nastavit maximální velikost každé výstup dávky, která bude odeslána do funkce Azure. Ve výchozím nastavení tato hodnota je 256 KB |
| Maximální počet dávek  |Jako název značí, tato vlastnost umožňuje určit maximální počet událostí v každé dávce, která se odešlou do Azure Functions. Počet maximální dávky výchozí hodnota je 100 |
| Klíč |Pokud chcete používat funkci Azure z jiné předplatné, můžete tak učinit zadáním klíče pro přístup k vaší – funkce |

Všimněte si, že když Azure Stream Analytics obdrží 413 (http požadavku Entity příliš velký) výjimky z funkce Azure, snižuje velikost dávky, který odesílá Azure Functions. V kódu funkce Azure používejte výjimku a ujistěte se, že Azure Stream Analytics neodešle nadměrné velikosti dávky. Prosím ujistěte se také, že jsou konzistentní s hodnotou zadanou v portálu Stream Analytics batch maximální počet a velikost hodnoty používané ve funkci. 

Také v situaci, kde není žádná událost cílová stránka časové okno, nevygeneruje žádný výstup. V důsledku toho nebude computeResult funkce volána. Toto chování je konzistentní s integrovanou agregační funkce vracející.


## <a name="get-help"></a>Podpora
Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky
Přečetli jste si úvod do služby Stream Analytics, spravované služby pro analýzy datových proudů z platformy Internet věcí. Další informace o této službě najdete v následujících článcích:

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
