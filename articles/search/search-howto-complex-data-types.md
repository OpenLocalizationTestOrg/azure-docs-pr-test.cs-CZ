---
title: "aaaHow toomodel komplexní datové typy ve službě Azure Search | Microsoft Docs"
description: "Vnořené nebo hierarchické datové struktury můžete modelován v indexu Azure Search pomocí plochou řádků a typu dat kolekce."
services: search
documentationcenter: 
author: LiamCa
manager: pablocas
editor: 
tags: complex data types; compound data types; aggregate data types
ms.assetid: e4bf86b4-497a-4179-b09f-c1b56c3c0bb2
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 05/01/2017
ms.author: liamca
ms.openlocfilehash: b330c5b322f4f33123a454be11733b977684b9e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomodel-complex-data-types-in-azure-search"></a>Jak toomodel rozšířené datové typy ve službě Azure Search
Externích datových sad použít toopopulate indexu Azure Search někdy obsahovat hierarchické nebo vnořená používání dílčích struktur, které nejsou v tabulkovém řádků přehledně rozdělení. Příkladem takových struktury může zahrnovat více umístění a telefonních čísel pro jednoho zákazníka, více barvy a velikosti pro jednu SKU, více autorů jeden knihy a tak dále. V modelování podmínky, můžete vidět těchto struktur označuje tooas *komplexními datovými typy*, *složené datové typy*, *složené datové typy*, nebo *agregační datové typy*, tooname a několika.

Komplexní datové typy nejsou podporovány nativně ve službě Azure Search, ale Principy řešení zahrnuje dvoustupňový proces sloučení hello struktura a pak pomocí **kolekce** datový typ vnitřních struktura tooreconstitute hello. Následující hello technik popsaných v tomto článku umožňuje obsahu toobe hello vyhledávaná, Fasetové, filtrovat a seřazeny.

## <a name="example-of-a-complex-data-structure"></a>Příklad rozšířené datové struktury
Data hello dotyčném se obvykle nachází jako sada dokumentů XML nebo JSON, nebo jako položky v úložišti NoSQL například Azure Cosmos DB. Strukturálně hello výzvy byl vyvolán s více podřízené položky, které je třeba toobe vyhledávat a filtrovat.  Jako výchozí bod pro znázornění hello alternativní řešení proveďte následující dokument JSON, který obsahuje sadu kontakty jako příklad hello:

~~~~~
[
  {
    "id": "1",
    "name": "John Smith",
    "company": "Adventureworks",
    "locations": [
      {
        "id": "1",
        "description": "Adventureworks Headquarters"
      },
      {
        "id": "2",
        "description": "Home Office"
      }
    ]
  }, 
  {
    "id": "2",
    "name": "Jen Campbell",
    "company": "Northwind",
    "locations": [
      {
        "id": "3",
        "description": "Northwind Headquarter"
      },
      {
        "id": "4",
        "description": "Home Office"
      }
    ]
}]
~~~~~

Při hello pole s názvem "id", "název" a "společnost" lze snadno mapovat 1: 1 jako pole v indexu Azure Search, hello 'umístění' pole obsahuje pole umístění, i skupinu ID umístění, jakož i popisy umístění. Vzhledem k tomu, že Azure Search nemá datový typ, který to podporuje, budeme potřebovat toomodel jiný způsob, jak ve službě Azure Search. 

> [!NOTE]
> Tento postup je popsán také zařízení Kirk Evans v příspěvku blogu [indexování DocumentDB s Azure Search](https://blogs.msdn.microsoft.com/kaevans/2015/03/09/indexing-documentdb-with-azure-seach/), který ukazuje techniku zvanou "sloučení hello data", které byste měli pole s názvem `locationsID` a `locationsDescription` které jsou obě [kolekce](https://msdn.microsoft.com/library/azure/dn798938.aspx) (nebo pole řetězců).   
> 
> 

## <a name="part-1-flatten-hello-array-into-individual-fields"></a>Část 1: Narovnání hello pole na jednotlivých polí
toocreate indexu Azure Search, která bude vyhovovat tuto datovou sadu vytvořit jednotlivých polí pro vnořené podkladní hello: `locationsID` a `locationsDescription` s typem dat [kolekce](https://msdn.microsoft.com/library/azure/dn798938.aspx) (nebo pole řetězců). V těchto polích by indexu hello hodnoty '1' a "2" do hello `locationsID` pole pro hodnoty John Smith a hello "3" a "4" do hello `locationsID` pole pro Jen Campbell.  

Vaše data v rámci Azure Search bude vypadat takto: 

![Ukázková data, 2 řádků](./media/search-howto-complex-data-types/sample-data.png)

## <a name="part-2-add-a-collection-field-in-hello-index-definition"></a>Část 2: Přidání kolekci polí v definici indexu hello
V hello schéma indexu definice pole hello může vypadat podobně jako příklad toothis.

~~~~
var index = new Index()
{
    Name = indexName,
    Fields = new[]
    {
        new Field("id", DataType.String) { IsKey = true },
        new Field("name", DataType.String) { IsSearchable = true, IsFilterable = false, IsSortable = false, IsFacetable = false },
        new Field("company", DataType.String) { IsSearchable = true, IsFilterable = false, IsSortable = false, IsFacetable = false },
        new Field("locationsId", DataType.Collection(DataType.String)) { IsSearchable = true, IsFilterable = true, IsFacetable = true },
        new Field("locationsDescription", DataType.Collection(DataType.String)) { IsSearchable = true, IsFilterable = true, IsFacetable = true }
    }
};
~~~~

## <a name="validate-search-behaviors-and-optionally-extend-hello-index"></a>Ověření chování vyhledávání a případně rozšířit hello indexu
Za předpokladu, že vytvořený hello index a načíst hello data, můžete prověřit provádění dotazu hello řešení tooverify vyhledávání proti hello datovou sadu. Každý **kolekce** pole by mělo být **prohledávatelné**, **filtrovatelných** a **facetable**. Měli byste mít možnost toorun dotazy jako:

* Najít všichni uživatelé, kteří pracují v hello 'Adventureworks ústředí'.
* Získáte počet hello počet lidí, kteří pracují v Home Office.  
* Hello osobám, které fungují na Home Office zobrazit jaké ostatních kancelářích fungují společně s počtem hello lidem v každém umístění.  

Kde tato technika spadá rozdělovat je, když potřebujete toodo hledání, které kombinuje id umístění hello jak hello umístění popis. Například:

* Najít všechny osoby, které mají Home Office a mají ID umístění na 4.  

Pokud si Vzpomínáte hello původní obsah hledá takto:

~~~~
   {
        id: '4',
        description: 'Home Office'
   }
~~~~

Ale teď, když budeme mít oddělené hello data do samostatných polí, jsme nijak zjistit, pokud má příliš vztah hello Home Office pro Jen Campbell`locationsID 3` nebo `locationsID 4`.  

toohandle v tomto případě definovat jiné pole v indexu hello, které se kombinuje všechna hello data do jedné kolekce.  Pro náš příklad jsme bude volat v tomto poli `locationsCombined` a jsme dojde k oddělení hello obsah s `||` však můžete zvolit jakékoli oddělovač, který se domníváte, že by jedinečnou sadu znaků pro obsah. Například: 

![Ukázková data, 2 řádky s oddělovačem](./media/search-howto-complex-data-types/sample-data-2.png)

Pomocí této `locationsCombined` pole, jsme teď zvládne i další dotazy, například:

* Zobrazí počet uživatelů, kteří pracovat na 'domovské úřadu, s umístěním Id "4".  
* Vyhledejte lidem, kteří pracují v Home Office s umístěním Id "4". 

## <a name="limitations"></a>Omezení
Tato metoda je užitečná pro různé scénáře, ale není možné v každém případě ho.  Například:

1. Pokud nemáte sadu statických polí v komplexního datového typu a se žádný způsob, jak toomap všechny možné hello typy tooa jediné pole. 
2. Aktualizace hello vnořených objektů vyžaduje některé další práci toodetermine přesně toho, co je toobe aktualizovat v indexu Azure Search hello

## <a name="sample-code"></a>Ukázka kódu
Můžete zobrazit příklad, jak tooindex komplexní data JSON nastavit Azure Search a provádět spoustu dotazy přes tuto datovou sadu v této [úložiště GitHub](https://github.com/liamca/AzureSearchComplexTypes).

## <a name="next-step"></a>Další krok
[Hlas pro nativní podpora pro komplexní datové typy](https://feedback.azure.com/forums/263029-azure-search) na hello Azure Search UserVoice stránku a všechny další zadání, který byste nám chtěli tooconsider týkající se funkce implementace. Můžete také oslovení toome přímo na Twitteru v @liamca.

