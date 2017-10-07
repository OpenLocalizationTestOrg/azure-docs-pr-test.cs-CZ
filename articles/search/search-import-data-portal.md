---
title: "aaaImport data do Azure Search na portálu hello | Microsoft Docs"
description: "Použijte hello Průvodce importem dat Azure Search v portálu Azure toocrawl hello Azure data z NoSQL Azure Cosmos DB, úložiště objektů Blob, úložiště table, SQL Database a SQL Server na virtuálních počítačích Azure."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: Azure Portal
ms.assetid: f40fe07a-0536-485d-8dfa-8226eb72e2cd
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 05/01/2017
ms.author: heidist
ms.openlocfilehash: 00b0e59594560c0cdaea748df196518e9fba3834
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="import-data-tooazure-search-using-hello-portal"></a>Import dat tooAzure vyhledávání pomocí portálu hello
poskytuje technologie Hello portál Azure **importovat data** Průvodce na řídicí panel Azure Search hello pro načítání dat do indexu. 

  ![Umožňuje importovat Data na panelu příkazů hello][1]

Interně hello Průvodce konfiguruje a vyvolá *indexer*, automatizaci několik kroků hello indexování proces: 

* Připojit tooan externí zdroj dat v hello stejného předplatného Azure
* Generovat schéma upravitelnými index založené na struktura hello zdroje dat
* Načtení dokumenty JSON do indexu pomocí sady řádků načíst ze zdroje dat hello

Tento pracovní postup můžete vyzkoušet s použitím ukázkových dat ve službě Azure Cosmos DB. Navštivte [Začínáme s Azure Search v portálu Azure hello](search-get-started-portal.md) pokyny.

> [!NOTE]
> Můžete spustit hello **importovat data** z toosimplify řídicí panel Azure Cosmos DB hello indexování pro tento zdroj dat průvodce. V levém navigačním panelu přejděte příliš**kolekce** > **přidat Azure Search** tooget spuštěna.

## <a name="data-sources-supported-by-hello-import-data-wizard"></a>Zdroje dat podporované rozhraním hello Průvodce importem dat
Průvodce importem dat Hello podporuje hello následující zdroje dat: 

* Azure SQL Database
* Relační data systému SQL Server na virtuálním počítači Azure
* Azure Cosmos DB
* Azure Blob Storage
* Azure Table Storage

Plochá datová sada je požadovaný vstup. Importovat můžete pouze z jedné tabulky, jednoho zobrazení databáze nebo ekvivalentní datové struktury. Měli byste vytvořit datovou strukturu před spuštěním Průvodce hello.

## <a name="connect-tooyour-data"></a>Připojení tooyour dat
1. Přihlaste se toohello [portál Azure](https://portal.azure.com) a řídicí panel služby otevřete hello. Můžete kliknout na **další služby** v hello přechod panelu toosearch pro existující "Vyhledat služby" v aktuálním předplatném hello. 
2. Klikněte na tlačítko **importovat Data** na hello příkazovém řádku okno importu dat tooslide otevřete hello.  
3. Klikněte na tlačítko **připojení dat tooyour** toospecify definici zdroje dat používanou indexerem. Pro zdroje dat uvnitř odběru průvodce hello obvykle zjistit a přečtěte si informace o připojení, minimalizovat požadavky na celkové konfiguraci.

|  |  |
| --- | --- |
| **Stávající zdroj dat** |Pokud již ve vyhledávací službě máte definované indexery, můžete pro další import vybrat stávající definici zdroje dat. |
| **Azure SQL Database** |Na stránce hello nebo prostřednictvím připojovací řetězec ADO.NET lze zadat název služby, pověření pro uživatele databáze s oprávněními ke čtení a název databáze. Zvolte hello připojovací řetězec možnost tooview nebo upravit vlastnosti. <br/><br/>na stránce hello je třeba zadat Hello tabulku nebo zobrazení, která poskytuje sada řádků hello. Tato možnost se zobrazí po připojení hello úspěšné, tak, že provedete výběr, která poskytuje rozevíracího seznamu. |
| **SQL Server na virtuálním počítači Azure** |Zadejte plně kvalifikovaný název služby, ID a heslo uživatele a databázi jako připojovací řetězec. toouse tento zdroj dat musí jste dříve nainstalovali certifikát v úložišti místní hello, který šifruje hello připojení. Pokyny najdete v tématu [tooAzure připojení virtuálního počítače s SQL vyhledávání](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md). <br/><br/>na stránce hello je třeba zadat Hello tabulku nebo zobrazení, která poskytuje sada řádků hello. Tato možnost se zobrazí po připojení hello úspěšné, tak, že provedete výběr, která poskytuje rozevíracího seznamu. |
| **Azure Cosmos DB** |Požadavky na zahrnují hello účet, databázi a kolekci. V indexu hello budou zahrnuty všechny dokumenty v kolekci hello. Můžete definovat tooflatten dotazu nebo filtru hello řádků nebo toodetect změnit dokumenty pro operace aktualizace následná data. |
| **Azure Blob Storage** |Požadavky na zahrnují hello účet úložiště a kontejneru. Volitelně Pokud názvy objektů blob podle virtuální zásady vytváření názvů pro účely seskupení, můžete zadat část virtuální adresář hello hello název jako složku v kontejneru. Další informace najdete v tématu [Indexování služby Blob Storage](search-howto-indexing-azure-blob-storage.md). |
| **Azure Table Storage** |Požadavky na zahrnují hello účet úložiště a název tabulky. Volitelně můžete zadat dotaz tooretrieve podmnožinu hello tabulky. Další informace najdete v tématu [Indexování služby Table Storage](search-howto-indexing-azure-tables.md). |

## <a name="customize-target-index"></a>Přizpůsobení cílového indexu
Předběžné index je obvykle odvodit z hello datové sady. Přidat, upravit nebo odstranit pole toocomplete hello schématu. Kromě toho nastavit atributy na úrovně toodetermine hello pole její chování následných vyhledávání.

1. V **upravit cílový index**, zadejte název hello a **klíč** použité toouniquely identifikovat každého dokumentu. Hello klíč musí být řetězec. Pokud pole hodnoty obsahovat mezery nebo spojovníky opravdu tooset rozšířené možnosti v **importovat data** toosuppress hello kontroly ověřování pro tyto znaky.
2. Zkontrolujte a zkontrolovat, jestli hello zbývající pole. Název a typ pole jsou obvykle předvyplněné. Dokud hello index se vytvoří, můžete změnit datový typ hello. Pozdější změna bude vyžadovat opětovné sestavení.
3. Pro každé pole nastavte atribut indexu:
   
   * Retrievable vrátí hello pole ve výsledcích hledání.
   * Filtrování umožňuje toobe hello pole odkazovat ve výrazech filtru.
   * Sortable umožňuje toobe hello pole při řazení.
   * Facetable (kategorizovatelné) umožňuje hello pole pro fasetovou navigaci.
   * Searchable (Prohledávatelné) – umožňuje fulltextové vyhledávání.
4. Klikněte na tlačítko hello **analyzátor** kartě, pokud chcete, aby toospecify analyzátor jazyka na úrovni pole hello. V této chvíli lze určit pouze analyzátory jazyka. Použití vlastního analyzátoru nebo nejazykového analyzátoru, jako například analyzátoru klíčových slov, vzoru a dalších, bude vyžadovat psaní kódu.
   
   * Klikněte na tlačítko **Searchable** toodesignate fulltextové vyhledávání na hello pole a povolení hello analyzátor rozevíracího seznamu.
   * Zvolte hello analyzátor, které chcete. Podrobnosti naleznete v oddílu [Vytvoření indexu pro vícejazyčné dokumenty](search-language-support.md).
5. Klikněte na tlačítko hello **modulu pro návrhy** tooenable našeptávání dotazů návrhy na vybrané pole.

## <a name="import-your-data"></a>Import dat
1. V **importovat data**, zadejte název indexeru hello. Odvolat produktu hello hello importovat Data Průvodce je indexer. Později Pokud chcete tooview, nebo ho upravit, vyberte ho z portálu hello místo opětovným spuštěním Průvodce hello. 
2. Zadejte plán hello, která je založena na hello místní časové pásmo ve kterém je služba hello zřízena.
3. Nastavte prahové hodnoty rozšířené možnosti toospecify na jestli indexování můžete pokračovat když dokumentu se zahodí. Kromě toho můžete zadat zda **klíč** pole jsou povoleny toocontain mezery a lomítka.  
4. Klikněte na tlačítko **OK** toocreate hello index a importovat hello data.

Můžete monitorovat indexování hello portálu. Jako dokumenty jsou načteny, se zvýší počet dokumentu hello hello indexu, které jste definovali. Někdy trvá několik minut, než hello stránky portálu toopick až hello nejnovější aktualizace.

Hello index je připraven tooquery hned, jak jsou načteny všechny dokumenty hello.

## <a name="query-an-index-using-search-explorer"></a>Dotazování indexu pomocí Průzkumník služby Search

portál Hello obsahuje **Průzkumník služby Search** tak, aby můžete dotazovat index bez nutnosti toowrite žádný kód. [Průzkumníka služby Search](search-explorer.md) můžete použít na jakýkoli index.

Hello vyhledávání je založeno na výchozích nastavení, jako je například hello [jednoduchý syntaktický](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) a výchozí [parametr dotazu searchMode (https://docs.microsoft.com/rest/api/searchservice/search-documents). 

Výsledky jsou vráceny ve formátu JSON, v podrobný formát tak, aby si můžete prohlédnout celý dokument hello.

## <a name="edit-an-existing-indexer"></a>Úprava existujícího indexeru
Jak jsme uvedli, vytvoří Průvodce importem dat hello **indexer**, které lze upravit jako samostatné konstrukce hello portálu.

Na řídicím panelu služby hello dvakrát klikněte na tooslide dlaždici Indexer hello se seznam všech indexerů vytvořených pro vaše předplatné. Dvakrát klikněte na indexery toorun hello, upravit nebo odstranit. Můžete nahradit jinou existující hello index, zdroj dat hello změny a nastavit možnosti pro prahové hodnoty chybu během indexování.

## <a name="edit-an-existing-index"></a>Úprava existujícího indexu
Průvodce Hello taky vytvořit **index**. Ve službě Azure Search index tooan strukturální aktualizace bude vyžadovat opětovné sestavení tohoto indexu. Nové vytvoření zahrnuje odstraňování hello index, znovu vytvořit index hello pomocí revidované schématu, který má požadované změny hello a načíst data. Strukturální aktualizace zahrnují změnu datového typu a přejmenování nebo odstranění pole.

Úpravy, které nevyžadují opětovné sestavení zahrnují přidání nového pole, změnu vyhodnocování profilů, změna navrhovatelů nebo změnu analyzátorů jazyka. Další informace naleznete v [aktualizaci indexu](https://msdn.microsoft.com/library/azure/dn800964.aspx).


## <a name="next-steps"></a>Další kroky
Projděte si tyto informace o indexery toolearn odkazy:

* [Indexování služby Azure SQL Database](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Indexování služby Azure Cosmos DB](search-howto-index-documentdb.md)
* [Indexování služby Blob Storage](search-howto-indexing-azure-blob-storage.md)
* [Indexování služby Table Storage](search-howto-indexing-azure-tables.md)

<!--Image references-->
[1]: ./media/search-import-data-portal/search-import-data-command.png

