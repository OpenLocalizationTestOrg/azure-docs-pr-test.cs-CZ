---
Title: aaa "Azure Analysis Services kurz Lekce 2: získání dat | Microsoft Docs"Popis: Popisuje, jak hello tooget a import dat v kurzu projektu Azure Analysis Services. služby: documentationcenter služby analysis services: '' Autor: minewiskan správce: erikre editor: '' značky: "

MS.AssetID: ms.service: ms.devlang služby analysis services: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 01/06/2017 ms.author: owend
---

# <a name="lesson-2-get-data"></a>Lekce 2: Získání dat

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

V této lekci použijete načíst Data v rozšíření SSDT tooconnect toohello AdventureWorksDW2014 ukázkovou databázi, vyberte data, náhled a filtr a potom importovat do pracovního prostoru modelu.  
  
Pomocí funkce Získání dat můžete importovat data z celé řady zdrojů: Azure SQL Database, Oracle, Sybase, kanál OData, Teradata, soubory a další. Data umožňují také dotazy pomocí výrazu se vzorci Power Query M.
  
Odhadovaný čas toocomplete této lekci: **10 minut**  
  
## <a name="prerequisites"></a>Požadavky  
Toto téma je součástí kurzu tabelárního modelování, který by se měl dokončit v daném pořadí. Před provedením úlohy hello v této lekci, by měl mít dokončit předchozí lekci hello: [lekci 1: vytvoření nového projektu tabulkový model](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md).  
  
## <a name="create-a-connection"></a>Vytvoření připojení  
  
#### <a name="toocreate-a-connection-toohello-adventureworksdw2014-database"></a>toocreate databázi toohello AdventureWorksDW2014 připojení  
  
1.  V Průzkumníku tabelárních modelů klikněte pravým tlačítkem na **Zdroje dat** > **Importovat ze zdroje dat**.  
  
    Spustí se získat Data, která vás provede připojování zdroje dat tooa. Pokud nevidíte v tabulkovém modelu Průzkumníku **Průzkumníku řešení**, dvakrát klikněte na **Model.bim** tooopen hello modelu v Návrháři hello. 
    
    ![aas-lesson2-getdata](../tutorials/media/aas-lesson2-getdata.png)
  
2.  Ve funkci Získání dat klikněte na **Databáze** > **Databáze SQL Serveru** > **Připojit**.  
  
3.  V hello **databáze systému SQL Server** dialogové okno, v **Server**, zadejte název hello hello serveru, kam jste nainstalovali hello AdventureWorksDW2014 databáze a pak klikněte na tlačítko **Connect**.  

4.  Po zobrazení výzvy tooenter přihlašovací údaje, je nutné toospecify hello pověření služby Analysis Services používá tooconnect toohello zdroj dat při importu a zpracování dat. V části **Režim zosobnění** vyberte **Zosobnit účet**, zadejte přihlašovací údaje a klikněte na **Připojit**. Doporučuje se, že používáte účet, kde není hello heslo vyprší.

    ![aas-lesson2-account](../tutorials/media/aas-lesson2-account.png)
  
    > [!NOTE]  
    > Pomocí uživatelského účtu systému Windows a hesla poskytuje nejbezpečnější metodou hello připojování zdroje dat tooa.
  
5.  V tabulce dat, vyberte hello **AdventureWorksDW2014** databáze a potom klikněte na **OK**. Tím se vytvoří databáze toohello hello připojení. 
  
6.  V tabulce dat, vyberte hello zaškrtávací políčko pro hello následujících tabulek: **DimCustomer**, **DimDate**, **DimGeography**, **DimProduct**,  **DimProductCategory**, **DimProductSubcategory**, a **FactInternetSales**.  

    ![aas-lesson2-select-tables](../tutorials/media/aas-lesson2-select-tables.png)
  
Po kliknutí na OK se otevře Editor dotazů. V další části hello vyberte pouze hello data, která chcete tooimport.

  
## <a name="filter-hello-table-data"></a>Filtrování dat v tabulce hello  
Tabulky v ukázkové databázi AdventureWorksDW2014 hello mají data, která není nutné tooinclude v modelu. Pokud je to možné, budete chtít toofilter se místo v paměti toosave nepotřebných dat používaný modelem hello. Můžete filtrovat některé z hello sloupce z tabulky, aby nejsou importovány do databáze pracovního prostoru hello hello model databáze, nebo po jeho nasazení. 
  
#### <a name="toofilter-hello-table-data-before-importing"></a>data tabulky hello toofilter před importem  
  
1.  V editoru dotazů vyberte hello **DimCustomer** tabulky. Hello DimCustomer tabulky na zdroj dat hello (ukázkové databázi AdventureWorksDWQ2014) se zobrazí. 
  
2.  Vyberte (Ctrl + kliknutí) sloupce **SpanishEducation**, **FrenchEducation**, **SpanishOccupation** a **FrenchOccupation**, klikněte pravým tlačítkem a potom klikněte na **Odebrat sloupce**. 

    ![aas-lesson2-remove-columns](../tutorials/media/aas-lesson2-remove-columns.png)
  
    Vzhledem k tomu, že hello hodnoty pro tyto sloupce nejsou relevantní tooInternet prodejní analýzy, není tooimport bez nutnosti tyto sloupce. Díky odstranění nepotřebných sloupců bude váš model menší a efektivnější.  
  
4.  Filtrovat hello zbývající tabulky odebráním hello následující sloupce v každé tabulce:  
    
    **DimDate**
    
      |Sloupec|  
      |--------|  
      |DateKey|  
      |**SpanishDayNameOfWeek**|  
      |**FrenchDayNameOfWeek**|  
      |**SpanishMonthName**|  
      |**FrenchMonthName**|  
  
    **DimGeography**
  
      |Sloupec|  
      |-------------|  
      |**SpanishCountryRegionName**|  
      |**FrenchCountryRegionName**|  
      |**IpAddressLocator**|  
  
    **DimProduct**
  
      |Sloupec|  
      |-----------|  
      |**SpanishProductName**|  
      |**FrenchProductName**|  
      |**FrenchDescription**|  
      |**ChineseDescription**|  
      |**ArabicDescription**|  
      |**HebrewDescription**|  
      |**ThaiDescription**|  
      |**GermanDescription**|  
      |**JapaneseDescription**|  
      |**TurkishDescription**|  
  
    **DimProductCategory**
  
      |Sloupec|  
      |--------------------|  
      |**SpanishProductCategoryName**|  
      |**FrenchProductCategoryName**|  
  
    **DimProductSubcategory**
  
      |Sloupec|  
      |-----------------------|  
      |**SpanishProductSubcategoryName**|  
      |**FrenchProductSubcategoryName**|  
  
    **FactInternetSales**
  
      |Sloupec|  
      |------------------|  
      |**OrderDateKey**|  
      |**DueDateKey**|  
      |**ShipDateKey**|   
  
## <a name="Import"></a>Import hello vybrané tabulky a sloupce dat  
Teď, když jste náhled a odfiltrovat nepotřebná data, můžete importovat hello zbytek hello data, která ho. Hello Průvodce naimportuje data tabulky hello společně s všechny vztahy mezi tabulkami. Nové tabulky a sloupce, které jsou vytvořené v modelu hello a data, která můžete odfiltrovat není importován.  
  
#### <a name="tooimport-hello-selected-tables-and-column-data"></a>tooimport hello vybrané tabulky a sloupce dat  
  
1.  Zkontrolujte váš výběr. Pokud vše vypadá v pořádku, klikněte na **Importovat**. Dialogové okno Hello zpracování dat se zobrazuje stav hello dat importovaných z zdroj dat do databáze pracovního prostoru.
  
    ![aas-lesson2-success](../tutorials/media/aas-lesson2-success.png) 
  
2.  Klikněte na **Zavřít**.  

  
## <a name="save-your-model-project"></a>Uložení projektu s modelem  
Je důležité toofrequently uložit projektu modelu.  
  
#### <a name="toosave-hello-model-project"></a>projekt modelu toosave hello  
  
-   Klikněte na **Soubor** > **Uložit vše**.  
  
## <a name="whats-next"></a>Co dále?
[Lekce 3: Označení jako tabulky kalendářních dat](../tutorials/aas-lesson-3-mark-as-date-table.md)

  
  
