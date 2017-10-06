---
Title: aaa "kurz: vytvoření prvního indexu Azure Search na portálu hello | Microsoft Docs"Popis: V hello portálu Azure, používání předdefinovaných ukázková data toogenerate indexu. Prozkoumejte fulltextové vyhledávání, filtry, omezující vlastnosti, vyhledávání přibližných shod, geografické vyhledávání a další funkce.
služby: vyhledávání documentationcenter: "Autor: HeidiSteen správce: jhubbard editor: '' značky: portálu azure

MS.AssetID: 21adc351-69bb-4a39-bc59-598c60c8f958 ms.service: vyhledávání ms.devlang: na ms.workload: vyhledávání ms.topic: hero-article ms.tgt_pltfrm: na ms.date: ms.author 06/26 nebo 2017: heidist

---
# <a name="tutorial-create-your-first-azure-search-index-in-hello-portal"></a>Kurz: Vytvoření prvního indexu Azure Search na portálu hello

V hello portálu Azure, začněte s tooquickly datovou sadu předdefinovaných ukázka generovat indexu pomocí hello **importovat data** průvodce. Prozkoumejte fulltextové vyhledávání, filtry, omezující vlastnosti, vyhledávání přibližných shod a geografické vyhledávání pomocí **průzkumníka služby Search**.  

Tento úvod bez kódu vám pomůže začít s předdefinovanými daty, abyste mohli hned začít psát zajímavé dotazy. Nástroje portálu nejsou náhradou za kód, ale jsou užitečné pro tyto úlohy:

+ Praktická výuka s minimálním zdržením
+ Vytvoření prototypu indexu před vlastním psaním kódu s využitím **importu dat**
+ Testovací dotazy a syntaxe analyzátoru v **průzkumníku služby Search**
+ Zobrazit existující index publikované tooyour služby a vyhledat jeho atributy

**časový odhad:** Přibližně 15 minut, ale déle, pokud se vyžaduje také registrace účtu nebo služby. 

Můžete také rychle pochopit práci pomocí [tooprogramming založené na kódu Úvod Azure Search v rozhraní .NET](search-howto-dotnet-sdk.md).

## <a name="prerequisites"></a>Požadavky

Tento kurz předpokládá [předplatné Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) a [službu Azure Search](search-create-service-portal.md). 

Pokud nechcete tooprovision služby okamžitě, můžete sledovat 6 minut ukázka hello kroky v tomto kurzu, počínaje přibližně tři minuty do této [přehled Azure Search video](https://channel9.msdn.com/Events/Connect/2016/138).

## <a name="find-your-service"></a>Vyhledání služby
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Otevřete řídicí panel služby hello služby Azure Search. Pokud připnete nebyla hello řídicí panel tooyour dlaždici služby, můžete najít služby tímto způsobem: 
   
   * V hello vlevo, klikněte na **další služby** dole hello hello levém navigačním podokně.
   * Hello vyhledávacího pole zadejte *vyhledávání* tooget seznam hledání služeb pro vaše předplatné. Služby by se zobrazit v seznamu hello. 

## <a name="check-for-space"></a>Kontrola místa
Mnoho zákazníků začíná s bezplatnou službou hello. Tato verze je omezená toothree indexy, tři zdroje dat a tři indexery. Než začnete, ujistěte se, že máte místo pro další položky. V tomto kurzu se vytváří od každého objektu jeden. 

> [!TIP] 
> Dlaždice na řídicím panelu služby hello zobrazit, kolik indexy, indexery a zdroje dat už máte. dlaždici Indexer Hello znázorňuje ukazatele úspěchy a chyby. Klikněte na tlačítko hello dlaždice tooview hello indexer count. 
>
> ![Dlaždice pro indexery a zdroje dat][1]
>

## <a name="create-index"></a>Vytvoření indexu a načtení dat
Vyhledávací dotazy provádějí iterace *indexu* obsahujícího data s možností vyhledávání, metadata a konstrukce používané k optimalizaci určitého chování vyhledávání.

tookeep tato úloha založené na portálu, používáme vestavěné ukázkový datovou sadu, která lze procházet pomocí indexeru prostřednictvím hello **importovat data** průvodce. 

#### <a name="step-1-start-hello-import-data-wizard"></a>Krok 1: Spuštění Průvodce importem dat hello
1. Na řídicím panelu služby Azure Search, klikněte na tlačítko **importovat data** v hello příkazového řádku toostart průvodce, který vytvoří a naplní index.
   
    ![Příkaz pro import dat][2]

2. V Průvodci hello, klikněte na tlačítko **zdroj dat** > **ukázky** > **realestate-us-sample**. Pro tento zdroj dat je předem nakonfigurovaný název, typ a informace o připojení. Po vytvoření se z něj stane „existující zdroj dat“, který je možné využít v dalších operacích importu.

    ![Výběr ukázkové datové sady][9]

3. Klikněte na tlačítko **OK** toouse ho.

#### <a name="step-2-define-hello-index"></a>Krok 2: Definování indexu hello
Vytvoření indexu je většinou ruční a založené na kódu, ale hello Průvodce může generovat index u každého zdroje dat, které může procházet. Minimálně index vyžaduje název a kolekci polí, s jedním polem označeným jako hello klíče toouniquely dokumentu identifikovat každého dokumentu.

Pole mají datové typy a atributy. jsou Hello zaškrtávací políčka v horní části hello *atributy indexu* řízení použití pole hello. 

* **Retrievable** (Zobrazitelné) znamená, že se zobrazí v seznamu výsledků vyhledávání. Jednotlivá pole můžete označit jako zakázaná pro výsledky hledání zrušením zaškrtnutí tohoto políčka, například když se pole používají jenom ve výrazech filtru. 
* **Filterable** (Filtrovatelné), **Sortable** (Seřaditelné) a **Facetable** (Kategorizovatelné) určují, zda lze pole použít ve filtru, v řazení nebo ve struktuře fasetové navigace. 
* **Searchable** (Prohledávatelné) znamená, že je pole součástí fulltextové vyhledávání. Řetězce je možné prohledávat. Číselná pole a logická pole jsou často označena jako neprohledávatelné. 

Ve výchozím nastavení Průvodce hello kontroluje hello zdroj dat pro jedinečné identifikátory jako hello základ pro klíčové pole hello. Řetězce mají atributy Retrievable a Searchable (zobrazitelné a prohledávatelné). Celá čísla mají atributy Retrievable, Filterable, Sortable a Facetable (zobrazitelné, filtrovatelné, seřaditelné a kategorizovatelné).

  ![Vytvořený index realestate][3]

Klikněte na tlačítko **OK** toocreate hello index.

#### <a name="step-3-define-hello-indexer"></a>Krok 3: Definování indexeru hello
Stále v hello **importovat data** průvodce, klikněte na tlačítko **Indexer** > **název**a zadejte název indexeru hello. 

Tento objekt definuje spustitelný proces. Může ho dát opakovaného plánu, ale teď pomocí hello výchozí možnost toorun hello indexeru po okamžitě, když kliknete na tlačítko **OK**.  

  ![Indxer realestate][8]

## <a name="check-progress"></a>Kontrola průběhu
toomonitor data importovat, přejděte zpět toohello řídicí panel služby, přejděte dolů a dvakrát klikněte na hello **indexery** seznam indexerů hello tooopen dlaždice. Měli byste vidět indexer hello nově vytvořený v seznamu hello, která určuje stav "v průběhu" nebo úspěch a s hello počet dokumentů indexovaných.

   ![Zpráva indexeru o průběhu][4]

## <a name="query-index"></a>Dotazování indexu hello
Nyní máte index vyhledávání, který je připraven tooquery. **Průzkumník služby Search** je integrovaná do portálu hello nástroj pro dotazování. Poskytuje vyhledávací pole, abyste mohli ověřit, jestli výsledky hledání odpovídají vašemu očekávání. 

> [!TIP]
> V hello [přehled Azure Search video](https://channel9.msdn.com/Events/Connect/2016/138), hello postupem je ukázán v 6m08s videu hello.
>

1. Klikněte na tlačítko **Průzkumník služby Search** na panelu příkazů hello.

   ![Příkaz průzkumníka služby Search][5]

2. Klikněte na tlačítko **změnu index** na hello příkaz panelu tooswitch příliš*realestate-us-sample*.

   ![Příkazy rozhraní API a index][6]

3. Klikněte na tlačítko **verze rozhraní API nastavit** na hello příkazového řádku toosee, které jsou k dispozici rozhraní REST API. Zobrazte náhled rozhraní API pro udělení přístupu toonew funkce obecně ještě není vydané. Pro dotazy hello níže hello všeobecně dostupná verzi (2016-09-01) použijte, pokud není výslovně. 

    > [!NOTE]
    > [REST API služby Azure Search](https://docs.microsoft.com/rest/api/searchservice/search-documents) a hello [knihovny .NET](search-howto-dotnet-sdk.md#core-scenarios) plně ekvivalentní, ale **Průzkumník služby Search** pouze vybavené toohandle volání REST. Přijímá syntaxe pro obě [jednoduchá syntaxe dotazů](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) a [úplné analyzátor dotazů Lucene](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), plus všechny hello parametry hledání, které jsou k dispozici v [vyhledávání dokumentů](https://docs.microsoft.com/rest/api/searchservice/search-documents) operace.
    > 

4. V panelu vyhledávání hello, zadejte níže řetězce dotazu hello a klikněte na **vyhledávání**.

  ![Příklad vyhledávacího dotazu][7]

**`search=seattle`**

+ Hello `search` parametr je použité tooinput centru pro fulltextové vyhledávání, v takovém případě vrácení výpisech ve stavu král okres, Washington, obsahující *Praha* existuje prohledávatelné pole v dokumentu hello. 

+ **Průzkumník služby Search** vrátí výsledky v kódu JSON, která je podrobné a pevné tooread, pokud dokumenty mají hustých strukturu. V závislosti na dokumentů, které bude pravděpodobně nutné toowrite kódu, obslužné rutiny Hledat výsledky tooextract důležité elementy. 

+ Dokumenty se skládají z pole označená jako získat v indexu hello. atributy indexu tooview hello portálu, klikněte na tlačítko *realestate-us-sample* v hello **indexy** dlaždici.

**`search=seattle&$count=true&$top=100`**

+ Hello `&` symbol je použité tooappend vyhledávání parametry, které lze zadat v libovolném pořadí. 

+  Hello `$count=true` parametr vrátí počet pro všechny dokumenty vrácené hello součet. Monitorováním změn hlášených parametrem `$count=true` můžete ověřovat filtrovací dotazy. 

+ Hello `$top=100` hello vrátí nejvyšší seřazeny 100 dokumenty mimo celkový hello. Ve výchozím nastavení vrátí Azure Search hello prvních 50 nejlepší shody. Můžete zvýšit nebo snížit množství hello prostřednictvím `$top`.

**`search=*&facet=city&$top=2`**

+ Parametr `search=*` znamená prázdné vyhledávání. Prázdné vyhledávání prohledává všechno. Jedním z důvodů pro odesílání prázdný dotazu je příliš filtrovat nebo omezující vlastnost přes hello kompletní sadu dokumenty. Například chcete používání omezujících vlastností navigační strukturu tooconsist všechny měst v indexu hello.

+  `facet`Vrátí navigační struktury, že můžete předat tooa kontrolní mechanismus uživatelského rozhraní. Vrací kategorie a počet. V takovém případě jsou kategorií podle počtu hello měst. Ve službě Azure Search neexistuje agregace, ale můžete ji odhadnout pomocí parametru `facet`, který vrací počet dokumentů v každé kategorii.

+ `$top=2`přináší zpět dva dokumenty, ilustrující, které můžete použít `top` tooboth snížit nebo zvýšit výsledky.

**`search=seattle&facet=beds`**

+ Tento dotaz je omezující vlastností na postele v textovém vyhledávání výrazu *Seattle*. `"beds"`lze zadat jako omezující vlastnost, protože pole hello je označen jako dá načíst, filtrování a kategorizovatelných v hello index a hello hodnoty ho obsahuje (číselný, 1 až 5), jsou vhodné pro zařazení výpisech do skupin (výpisech s ložnic 3, 4 ložnic). 

+ Kategorizovat je možné pouze filtrovatelná pole. Ve výsledcích hello se může vracet jenom dá načíst pole.

**`search=seattle&$filter=beds gt 3`**

+ Hello `filter` parametr vrátí výsledků odpovídajících kritériím hello jste zadali. V tomto případě víc než 3 ložnice. 

+ Syntaxe parametru Filter je založená na konstruktech jazyka OData. Další informace najdete v tématu věnovaném [syntaxi jazyka OData pro filtry](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search).

**`search=granite countertops&highlight=description`**

+ Přístupů zvýraznění tooformatting odkazuje na textu hello – klíčové slovo, danou odpovídá jsou nalezeny odpovídající v určitém poli. Pokud je hledaný termín hluboko schovaný v popis, můžete přidat přístupů zvýraznění toomake je snazší toospot. V takovém případě hello formátu frázi `"granite countertops"` je snazší toosee v poli Popis hello.

**`search=mice&highlight=description`**

+ Fulltextové vyhledávání vyhledá tvary slov s podobnou sémantikou. V takovém případě výsledky hledání obsahují zvýrazněnému textu pro "myš" Domů, které mají myši z počítače, v odpovědi tooa – klíčové slovo vyhledávání v "myši". Různé formy hello stejné word může zobrazit ve výsledcích kvůli lingvistické analýzy. 

+ Azure Search podporuje 56 analyzátorů od společností Lucene a Microsoft. Výchozí hodnota Hello používá Azure Search je standardní analyzátor Lucene hello. 

**`search=samamish`**

+ Překlepu slova, jako je "samamish' pro hello Samammish plateau v hello oblasti Seattle, nezdaří tooreturn odpovídá v typické vyhledávací. pravopisné chyby toohandle, můžete použít přibližné vyhledávání, popsané v následujícím příkladu hello.

**`search=samamish~&queryType=full`**

+ Přibližné vyhledávání je povoleno, když zadáte hello `~` symbolů a použít analyzátor hello celý dotaz, který interpretuje a správně analyzuje hello `~` syntaxe. 

+ Přibližné vyhledávání je k dispozici, pokud můžete vyjádřit výslovný souhlas pro analyzátor hello celý dotaz, který nastane, když nastavíte `queryType=full`. Další informace o scénářích dotazu povoleno analyzátorem hello celý dotaz najdete v tématu [syntaxe dotazů Lucene ve službě Azure Search](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search).

+ Když `queryType` je tento parametr nezadáte, se používá hello výchozí jednoduchý dotaz analyzátor. Analyzátor jednoduchý dotaz Hello je rychlejší, ale pokud budete potřebovat přibližné vyhledávání, regulární výrazy, vyhledávání blízkých výrazů nebo jiných typů rozšířený dotaz, budete potřebovat úplnou syntaxí hello. 

**`search=*&$count=true&$filter=geo.distance(location,geography'POINT(-122.121513 47.673988)') le 5`**

+ Geoprostorové vyhledávání je podporováno prostřednictvím hello [edm. Datový typ GeographyPoint](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) na pole obsahující souřadnice. Geoprostorové hledání je typ filtru určený v [syntaxi jazyka OData pro filtry](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search). 

+ Příklad dotazu Hello filtruje všechny výsledky pro poziční data, kde jsou výsledky méně než 5 kilometrech od k danému bodu (zadaný jako zeměpisné šířky a délky). Přidáním `$count`, můžete zobrazit, kolik výsledky se vrátí, když změníte hello vzdálenost nebo hello souřadnice. 

+ Geoprostorové hledání je užitečné, pokud vaše vyhledávací aplikace obsahuje funkci typu „najít poblíž“ nebo používá navigaci podle map. Není to ale fulltextové vyhledávání. Pokud máte uživatelské požadavky pro vyhledávání na název města nebo země podle názvu, přidejte pole obsahující názvy městě nebo zemi, v přidání toocoordinates.

## <a name="next-steps"></a>Další kroky

+ Upravte hello objekty, které jste právě vytvořili. Po spuštění Průvodce hello jednou, můžete přejít zpět a zobrazit nebo upravit jednotlivé komponenty: index, indexer nebo zdroj dat.. Některé úpravy, jako je například změna datového typu pole hello, hello nejsou povoleny na hello index, ale většinu vlastností a nastavení lze měnit.

  tooview jednotlivé komponenty, klikněte na tlačítko hello **Index**, **Indexer**, nebo **zdroje dat** dlaždice na váš řídicí panel toodisplay seznam existujících objektů. toolearn Další informace o indexu úpravy, které nevyžadují opětovném sestavení, najdete v části [aktualizace indexu (REST Azure Search API)](https://docs.microsoft.com/rest/api/searchservice/update-index).

+ Zkuste hello nástroje a postup s jinými zdroji dat.. Hello ukázkovou datovou sadu, `realestate-us-sample`, z databáze SQL Azure, které může procházet služba Azure Search. Kromě služby Azure SQL Database může Azure Search procházet také ploché struktury dat (a odvozovat z nich indexy) ve službách Azure Table Storage, Blob Storage, SQL Server na virtuálním počítači Azure a ve službě Azure Cosmos DB. Všechny tyto zdroje dat jsou podporovány v Průvodci hello. V kódu můžete index snadno vytvořit a naplnit pomocí *indexeru*.

+ Všechny ostatní zdroje dat jiný indexer jsou podporované pomocí model push, kde kód nové nabízených oznámení a změnit sady řádků v indexu tooyour JSON. Další informace najdete v tématu věnovaném [přidání, aktualizaci a odstranění dokumentů ve službě Azure Search](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents).

Další informace o ostatních funkcích uvedených v tomto článku najdete pomocí těchto odkazů:

* [Přehled indexerů](search-indexer-overview.md)
* [Vytvoření indexu (zahrnuje podrobné vysvětlení atributy indexu hello)](https://docs.microsoft.com/rest/api/searchservice/create-index)
* [Průzkumník služby Search](search-explorer.md)
* [Hledání dokumentů (zahrnuje příklady syntaxe dotazů)](https://docs.microsoft.com/rest/api/searchservice/search-documents)


<!--Image references-->
[1]: ./media/search-get-started-portal/tiles-indexers-datasources2.png
[2]: ./media/search-get-started-portal/import-data-cmd2.png
[3]: ./media/search-get-started-portal/realestateindex2.png
[4]: ./media/search-get-started-portal/indexers-inprogress2.png
[5]: ./media/search-get-started-portal/search-explorer-cmd2.png
[6]: ./media/search-get-started-portal/search-explorer-changeindex-se2.png
[7]: ./media/search-get-started-portal/search-explorer-query2.png
[8]: ./media/search-get-started-portal/realestate-indexer2.png
[9]: ./media/search-get-started-portal/import-datasource-sample2.png