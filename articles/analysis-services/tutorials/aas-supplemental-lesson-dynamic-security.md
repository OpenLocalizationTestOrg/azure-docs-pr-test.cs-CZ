---
Title: aaa "kurz dodatečné lekce Azure Analysis Services: dynamické zabezpečení | Microsoft Docs"Popis: Popisuje, jak filtry toouse dynamické zabezpečení pomocí řádek v kurzu hello Azure Analysis Services.
služby: documentationcenter služby analysis services: '' Autor: minewiskan správce: erikre editor: '' značky: "

MS.AssetID: ms.service: ms.devlang služby analysis services: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26 nebo 2017 ms.author: owend
---
# <a name="supplemental-lesson---dynamic-security"></a>Doplňková lekce – Dynamické zabezpečení

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

V této doplňkové lekci vytvoříte další roli, která implementuje dynamické zabezpečení. Dynamické funkce zabezpečení poskytuje zabezpečení na úrovni řádků podle hello id uživatele, název nebo přihlášení aktuálně přihlášený uživatel hello. 
  
tooimplement dynamické zabezpečení, můžete přidat model tooyour tabulky obsahující hello uživatelská jména na uživatele, kteří můžou připojit toohello modelu a procházet objekty modelu a data. Hello modelu, který vytvoříte pomocí tohoto kurzu je v kontextu hello společnosti Adventure Works; ale toocomplete to lekce, je nutné přidat tabulku obsahující uživatelé z vlastní domény. Není nutné hello hesel pro uživatelská jména hello, které jsou přidány. toocreate tabulku EmployeeSecurity se malé ukázkové uživatelů z vlastní domény použijete hello vložení funkci, vkládání dat zaměstnance z tabulky aplikace Excel. Ve scénáři real-world hello tabulku obsahující uživatelská jména obvykle by tabulku ze samotné databázi jako zdroj dat; například skutečné DimEmployee tabulku.  
  
dynamické zabezpečení tooimplement, používáte dvě funkce jazyka DAX: [funkce uživatelské jméno (DAX)](http://msdn.microsoft.com/22dddc4b-1648-4c89-8c93-f1151162b93f) a [funkce LOOKUPVALUE (DAX)](http://msdn.microsoft.com/73a51c4d-131c-4c33-a139-b1342d10caab). Tyto funkce použité ve vzorci filtru řádků jsou definované v nové roli. Pomocí funkce LOOKUPVALUE hello hello vzorec určuje hodnotu z tabulky EmployeeSecurity hello. Vzorec Hello pak předá, že hodnota toohello uživatelské jméno funkce, který určuje uživatelské jméno hello hello uživateli přihlášenému patří toothis role. vyhledejte uživatele Hello můžete pouze dat zadaný hodnotou hello role řádkové filtry. V tomto scénáři zadejte prodeje zaměstnanci můžete procházet pouze data prodeje Internetu pro hello prodejní teritoria, ve kterých jsou členy.  
  
Ty úlohy, které jsou jedinečné toothis scénář tabulkový model společnosti Adventure Works, ale nebude platit scénářem z reálného prostředí tooa jsou identifikovány jako takový. Každý úkol obsahuje další informace popisující účel hello hello úlohy.  
  
Odhadovaný čas toocomplete této lekci: **30 minut**  
  
## <a name="prerequisites"></a>Požadavky  
Toto téma doplňkové lekce je součástí kurzu tabelárního modelování, který by se měl dokončit v daném pořadí. Před provedením úlohy hello v této další lekci, by měl mít dokončit všechny předchozí lekce.  
  
## <a name="add-hello-dimsalesterritory-table-toohello-aw-internet-sales-tabular-model-project"></a>Přidat hello DimSalesTerritory tabulky toohello AW Internet prodej tabulkový Model projekt  
tooimplement dynamické zabezpečení pro tento scénář společnosti Adventure Works, musíte přidat dva další tabulky tooyour modelu. Hello první tabulky, které přidáte je DimSalesTerritory (jako oblast prodeje) z hello stejnou databázi AdventureWorksDW. Řádek filtru toohello SalesTerritory tabulku, která definuje konkrétní datové hello použijete později můžete procházet hello přihlášeného uživatele.  
  
#### <a name="tooadd-hello-dimsalesterritory-table"></a>Tabulka DimSalesTerritory tooadd hello  
  
1.  V části Průzkumník tabelárních modelů > **Zdroje Dat** klikněte pravým tlačítkem na vaše připojení a potom klikněte na **Importovat nové tabulky**.  

    Pokud se zobrazí dialogové okno hello pověření k zosobnění, zadejte pověření k zosobnění hello jste použili v lekci 2: Přidat Data.
  
2.  V tabulce dat, vyberte hello **DimSalesTerritory** tabulky a potom klikněte na **OK**.    
  
3.  V editoru dotazů, klikněte na tlačítko hello **DimSalesTerritory** dotaz a pak odeberte **SalesTerritoryAlternateKey** sloupce.  
  
7.  Klikněte na **Importovat**.  
  
    Nová tabulka Hello se přidá toohello modelu prostoru. Objektů a dat z tabulky DimSalesTerritory zdroj hello jsou pak importovat do tabulkového modelu vaší AW Internet prodej.  
  
9. Po hello tabulka byla úspěšně importována, klikněte na tlačítko **Zavřít**.  

## <a name="add-a-table-with-user-name-data"></a>Přidání tabulky s daty o uživatelských jménech  
Tabulka DimEmployee Hello v ukázková databáze AdventureWorksDW hello obsahuje uživatelů z domény AdventureWorks hello. Tato uživatelská jména ve vašem prostředí neexistují. Musíte v modelu vytvořit tabulku, která bude obsahovat malý vzorek skutečných uživatelů (alespoň tři) z vaší organizace. Tito uživatelé se pak přidejte jako členy toohello novou roli. Není nutné hello hesel pro uživatelská jména ukázkový text hello, ale potřebujete skutečné uživatelských jmen systému Windows z vlastní domény.  
  
#### <a name="tooadd-an-employeesecurity-table"></a>tooadd EmployeeSecurity tabulky  
  
1.  Otevřete aplikaci Microsoft Excel a vytvořte list.  
  
2.  Hello následující tabulky, včetně hello řádek záhlaví, zkopírujte a vložte jej do listu hello.  

    ```
      |EmployeeId|SalesTerritoryId|FirstName|LastName|LoginId|  
      |---------------|----------------------|--------------|-------------|------------|  
      |1|2|<user first name>|<user last name>|\<domain\username>|  
      |1|3|<user first name>|<user last name>|\<domain\username>|  
      |2|4|<user first name>|<user last name>|\<domain\username>|  
      |3|5|<user first name>|<user last name>|\<domain\username>|  
    ```

3.  Nahraďte hello křestní jméno, příjmení a doména\uživatelské_jméno hello názvy a přihlašovací ID tři uživatelé ve vaší organizaci. Uveďte hello stejného uživatele na hello první dva řádky, pro EmployeeId 1, zobrazující tohoto uživatele patří toomore než jednu prodejní oblast. Nechte hello pole EmployeeId a SalesTerritoryId, jak jsou.  
  
4.  Uložit list hello jako **SampleEmployee**.  
  
5.  V listu hello, vyberte všechny buňky hello s daty zaměstnanců, včetně hello hlavičky, pak klikněte pravým tlačítkem na vybrané hello data a pak klikněte na tlačítko **kopie**.  
  
6.  V sadě SSDT, klikněte na tlačítko hello **upravit** nabídce a pak klikněte na tlačítko **vložení**.  
  
    Pokud je šedě vložení, klikněte na možnost žádný sloupec v tabulce v hello modelu návrháře okno a akci opakujte.  
  
7.  V hello **vložení Preview** dialogu **název tabulky**, typ **EmployeeSecurity**.  
  
8.  V **toobe Data vložit**, ověřte hello data zahrnují všechny hello uživatelských dat a záhlaví z listu SampleEmployee hello.  
  
9. Ověřte, že je zaškrtnuté políčko **Použít první řádek jako záhlaví sloupců** a potom klikněte na **Ok**.  
  
    Je vytvořena nová tabulka s názvem EmployeeSecurity zaměstnanec daty zkopírovanými z listu SampleEmployee hello.  
  
## <a name="create-relationships-between-factinternetsales-dimgeography-and-dimsalesterritory-table"></a>Vytvoření relací mezi tabulkami FactInternetSales, DimGeography a DimSalesTerritory  
Hello FactInternetSales, DimGeography a DimSalesTerritory tabulka všechny obsahovat sloupec společných SalesTerritoryId. Hello SalesTerritoryId sloupec v tabulce DimSalesTerritory hello obsahuje hodnoty jiné Id pro každý prodejní oblasti.  
  
#### <a name="toocreate-relationships-between-hello-factinternetsales-dimgeography-and-hello-dimsalesterritory-table"></a>toocreate vztahy mezi hello FactInternetSales, DimGeography a tabulku DimSalesTerritory hello  
  
1.  V zobrazení diagramu, v hello **DimGeography** tabulky, klikněte na tlačítko a počkejte hello **SalesTerritoryId** sloupec a pak přetáhněte hello kurzor toohello **SalesTerritoryId** sloupec v hello **DimSalesTerritory** tabulky a poté uvolněte.  
  
2.  V hello **FactInternetSales** tabulky, klikněte na tlačítko a počkejte hello **SalesTerritoryId** sloupec a pak přetáhněte hello kurzor toohello **SalesTerritoryId** sloupec v hello  **DimSalesTerritory** tabulky a poté uvolněte.  
  
    Všimněte si hello Active vlastnost pro tento vztah je False, což znamená, že je neaktivní. Tabulka FactInternetSales Hello již má jiný aktivní relace.  
  
## <a name="hide-hello-employeesecurity-table-from-client-applications"></a>Skrýt hello EmployeeSecurity tabulky z klientské aplikace  
V této úloze je skrýt hello EmployeeSecurity tabulky, udržování z zobrazovaných v seznamu polí klientské aplikace. Nezapomeňte, že skrytím tabulku nezabezpečíte. Uživatelé pořád můžou dotazovat data v tabulce EmployeeSecurity, pokud ví jak. toosecure hello dat v tabulce EmployeeSecurity, brání uživatelé nebudou moct tooquery dat., můžete použít filtr v úloze později.  
  
#### <a name="toohide-hello-employeesecurity-table-from-client-applications"></a>Tabulka EmployeeSecurity hello toohide z klientské aplikace  
  
-   V Návrháři hello modelu, v zobrazení diagramu, klikněte pravým tlačítkem na hello **zaměstnanec** záhlaví tabulky a potom klikněte na **skrýt v nástrojích klienta**.  
  
## <a name="create-a-sales-employees-by-territory-user-role"></a>Vytvoření role uživatele Zaměstnanci prodeje podle oblasti  
V tomto úkolu vytvoříte roli uživatele. Tato role zahrnuje definování řádky hello DimSalesTerritory tabulky, které jsou viditelné toousers řádkovém filtru. Hello filtru se potom použije v relaci hello na více tabulek směr tooall jiné související tooDimSalesTerritory. Můžete také použít filtr, který zabezpečuje hello celou tabulku EmployeeSecurity nebudou dotazovatelný žádný uživatel, který je členem hello role.  
  
> [!NOTE]  
> Hello prodeje zaměstnanci území role, které vytvoříte v této lekci omezuje členy toobrowse (nebo dotazu) pouze údaje o prodeji toowhich hello prodejní oblasti, ke které patří. Pokud přidáte uživatele jako člen toohello prodej zaměstnanci území role, která existuje také jako člen v roli vytvořené v [lekce 11: vytvoření role](../tutorials/aas-lesson-11-create-roles.md), získat kombinace oprávnění. Pokud uživatel je členem více rolí, hello oprávnění a řádkové filtry definované pro každou roli jsou kumulativní. To znamená má uživatel hello hello větší oprávnění dáno hello kombinaci rolí.  
  
#### <a name="toocreate-a-sales-employees-by-territory-user-role"></a>toocreate zaměstnanci prodej území rolí uživatele  
  
1.  V sadě SSDT, klikněte na tlačítko hello **modelu** nabídce a pak klikněte na tlačítko **role**.  
  
2.  Ve **Správci rolí** klikněte na **Nový**.  
  
    Nová role s hello přidány žádné oprávnění je toohello seznamu.  
  
3.  Klikněte na tlačítko Nová role hello a potom v hello **název** sloupce, přejmenujte roli hello příliš**zaměstnanci prodeje podle oblastí**.  
  
4.  V hello **oprávnění** sloupce, klikněte na tlačítko hello rozevíracího seznamu a pak vyberte hello **čtení** oprávnění.  
  
5.  Klikněte na tlačítko hello **členy** a pak klikněte **přidat**.  
  
6.  V hello **vybrat uživatele nebo skupinu** dialogu **Enter hello objekt s názvem tooselect**, typ hello první ukázkové uživatelské jméno jste použili při vytvoření tabulky EmployeeSecurity hello. Klikněte na tlačítko **Kontrola názvů** tooverify hello uživatelské jméno je platný a poté klikněte na tlačítko **Ok**.  
  
    Opakujte tento krok, přidání hello jiných ukázka uživatelská jména, který jste použili při vytvoření tabulky EmployeeSecurity hello.  
  
7.  Klikněte na tlačítko hello **řádkové filtry** kartě.  
  
8.  Pro hello **EmployeeSecurity** tabulky v hello **DAX filtru** sloupce, typ hello následující vzorec:  
  
    ```
      =FALSE()  
    ```
  
    Tento vzorec určuje, že všechny sloupce přeložit toohello false Boolean podmínku. Žádné sloupce pro tabulku EmployeeSecurity hello může dotazovat členem hello prodej zaměstnanci území rolí uživatele.  
  
9. Pro hello **DimSalesTerritory** tabulky, typ hello následující vzorec:  

    ```  
    ='Sales Territory'[Sales Territory Id]=LOOKUPVALUE('Employee Security'[Sales Territory Id], 
      'Employee Security'[Login Id], USERNAME(), 
      'Employee Security'[Sales Territory Id], 
      'Sales Territory'[Sales Territory Id]) 
    ```
  
    V tento vzorec hello funkce LOOKUPVALUE vrátí všechny hodnoty pro sloupec hello DimEmployeeSecurity [SalesTerritoryId], kde hello EmployeeSecurity [LoginId] je hello stejná jako aktuální přihlášení uživatelské jméno systému Windows a EmployeeSecurity [[[hello SalesTerritoryId] je hello stejné jako hello DimSalesTerritory [SalesTerritoryId].  
  
    Hello sada identifikátorů prodejní oblasti vrácený LOOKUPVALUE je pak použít toorestrict hello řádky zobrazené v hello DimSalesTerritory tabulky. Zobrazí se pouze řádky, kde hello SalesTerritoryID pro řádek hello je v sadě hello ID vrácené funkce LOOKUPVALUE hello.  
  
10. Ve Správci rolí klikněte na **Ok**.  
  
## <a name="test-hello-sales-employees-by-territory-user-role"></a>Testování hello prodej zaměstnanci území rolí uživatele  
V této úloze použijte hello analyzovat v aplikaci Excel funkce v rozšíření SSDT tootest hello účinnosti hello prodej zaměstnanci území rolí uživatele. Zadejte jednu hello uživatelských jmen jste přidali toohello EmployeeSecurity tabulky a jako člen role hello. Toto uživatelské jméno je pak použít jako hello efektivní uživatelské jméno v hello připojení vytvořit mezi aplikace Excel a hello modelu.  
  
#### <a name="tootest-hello-sales-employees-by-territory-user-role"></a>tootest hello prodej zaměstnanci území rolí uživatele  
  
1.  V sadě SSDT, klikněte na tlačítko hello **modelu** nabídce a pak klikněte na tlačítko **analyzovat v aplikaci Excel**.  
  
2.  V hello **analyzovat v aplikaci Excel** dialogovém **zadejte hello uživatelské jméno nebo roli toouse tooconnect toohello modelu**, vyberte **ostatní uživatele systému Windows**a potom klikněte na **Procházet**.  
  
3.  V hello **vybrat uživatele nebo skupinu** dialogu **zadejte tooselect název objektu hello**, zadejte uživatelské jméno zahrnuty v tabulce EmployeeSecurity hello a pak klikněte na tlačítko **Kontrola názvů**.  
  
4.  Klikněte na tlačítko **Ok** tooclose hello **vybrat uživatele nebo skupinu** dialogové okno a pak klikněte na tlačítko **Ok** tooclose hello **analyzovat v aplikaci Excel** dialogové okno.  
  
    Otevře se aplikace Excel s novým sešitem. Automaticky se vytvoří kontingenční tabulka. Hello seznamu polí kontingenční tabulku obsahuje většinu hello datová pole v novém modelu k dispozici.  
  
    Všimněte si hello EmployeeSecurity tabulka není zobrazená v seznamu polí kontingenční tabulky hello. Tuto tabulku jste skryli před klientskými nástroji v jednom z předchozích úkolů.  
  
5.  V hello **pole** v seznamu **∑ Internet prodej** (measures), vyberte hello **InternetTotalSales** měr. míra Hello je zadán do hello **hodnoty** pole.  
  
6.  Vyberte hello **SalesTerritoryId** sloupec z hello **DimSalesTerritory** tabulky. sloupec Hello je zadán do hello **popisky řádků** pole.  
  
    Všimněte si aplikaci Internet prodeje jsou zobrazeny pouze pro hello jednu oblast toowhich hello efektivní uživatelské jméno, které jste použili patří. Pokud jste vybrali jiný sloupec, například města z tabulky DimGeography hello jako popisek řádku pole, pouze města v hello prodejní oblasti toowhich hello efektivní uživatelské patří se zobrazí.  
  
    Tohoto uživatele nelze procházet nebo data žádné Internet prodejní teritoria než hello jeden náleží do dotazu. Je toto omezení, protože hello Filtr řádků definovaný pro tabulku DimSalesTerritory hello, v hello prodeje zaměstnanci území rolí uživatele, zabezpečuje data pro všechna data související s tooother prodejní teritoria.  
  
## <a name="see-also"></a>Viz také  
[Funkce USERNAME (DAX)](https://msdn.microsoft.com/library/hh230954.aspx)  
[Funkce LOOKUPVALUE (DAX)](https://msdn.microsoft.com/library/gg492170.aspx)  
[Funkce CUSTOMDATA (DAX)](https://msdn.microsoft.com/library/hh213140.aspx)  