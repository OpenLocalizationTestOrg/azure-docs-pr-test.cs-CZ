---
Title: aaa "Azure Analysis Services kurz Lekce 3: označte jej jako tabulka kalendářních dat | Microsoft Docs"Popis: Popisuje, jak toomark datum tabulky v kurzu projektu hello Azure Analysis Services. služby: documentationcenter služby analysis services: '' Autor: minewiskan správce: erikre editor: '' značky: "

MS.AssetID: ms.service: ms.devlang služby analysis services: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 01/06/2017 ms.author: owend
---
# <a name="lesson-3-mark-as-date-table"></a>Lekce 3: Označení jako tabulky kalendářních dat

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

V Lekci 2: Získání dat jste naimportovali tabulku dimenzí s názvem DimDate. Přestože ve vašem modelu je tato tabulka pojmenovaná DimDate, je možné ji také označit jako *tabulku kalendářních dat*, protože obsahuje údaje o datu a čase.  
  
Při každém použití funkcí časového měřítka DAX, třeba při pozdějším vytvoření měření, musíte zadat vlastnosti, které zahrnují *tabulku kalendářních dat* a jedinečný identifikátor – *sloupec Date* v této tabulce.
  
V této lekci označit hello DimDate tabulky jako hello *tabulku kalendářních dat* a ve sloupci Datum hello (v tabulce datum hello) jako hello *sloupce s datem* (jedinečný identifikátor).  

Než označíte hello sloupci tabulky a data, je vhodná doba toodo, trochu spouštění toomake jednodušší toounderstand váš model. Všimněte si v tabulce DimDate hello sloupec s názvem **FullDateAlternateKey**. Tento sloupec obsahuje jeden řádek pro každý den v roce zahrnuty v tabulce hello. Tento sloupec používáte v řadě vzorců měření a v sestavách. Ale FullDateAlternateKey skutečně není dobrý identifikátor pro tento sloupec. Přejmenujte ji příliš**datum**, takže je jednodušší tooidentify a zahrnout do vzorce. Pokud je to možné, je vhodné toorename objekty jako tabulky a sloupce toomake je snazší tooidentify v rozšíření SSDT a vytváření sestav aplikace, jako je Power BI a Excel klienta. 
  
Odhadovaný čas toocomplete této lekci: **tři minuty**  
  
## <a name="prerequisites"></a>Požadavky  
Toto téma je součástí kurzu tabelárního modelování, který by se měl dokončit v daném pořadí. Před provedením úlohy hello v této lekci, by měl mít dokončit předchozí lekci hello: [lekci 2: získání dat](../tutorials/aas-lesson-2-get-data.md). 

### <a name="toorename-hello-fulldatealternatekey-column"></a>sloupec FullDateAlternateKey toorename hello

1.  V Návrháři hello model, klikněte na tlačítko hello **DimDate** tabulky.

2.  Dvakrát klikněte na záhlaví hello pro hello **FullDateAlternateKey** sloupce a přejmenujte příliš**datum**.

  
### <a name="tooset-mark-as-date-table"></a>tooset označit jako tabulku kalendářních dat.  
  
1.  Vyberte hello **datum** sloupce a potom v hello **vlastnosti** okno, v části **datový typ**, zajistěte, aby **datum** je vybrána.  
  
2.  Klikněte na tlačítko hello **tabulky** nabídky, pak klikněte na tlačítko **datum**a potom klikněte na **označit jako tabulka kalendářních dat**.  
  
3.  V hello **označit jako tabulka kalendářních dat** dialogové okno, v hello **datum** listbox, vyberte hello **datum** sloupci jako jedinečný identifikátor hello. Ve výchozím nastavení je obvykle vybraný. Klikněte na **OK**. 

    ![aas-lesson3-date-table](../tutorials/media/aas-lesson3-date-table.png)
  

## <a name="whats-next"></a>Co dále?
[Lekce 4: Vytvoření relací](../tutorials/aas-lesson-4-create-relationships.md)
  
