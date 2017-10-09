---
Title: aaa "Azure Analysis Services kurz lekce 11: vytvoření role | Microsoft Docs"Popis: Popisuje, jak hello toocreate rolí v kurzu projektu Azure Analysis Services. služby: documentationcenter služby analysis services: '' Autor: minewiskan správce: erikre editor: '' značky: "

MS.AssetID: ms.service: ms.devlang služby analysis services: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26 nebo 2017 ms.author: owend
---
# <a name="lesson-11-create-roles"></a>Lekce 11: Vytvoření rolí

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

V této lekci vytvoříte role. Role poskytují modelu zabezpečení objektů a dat databázi omezením přístupu tooonly uživatelům, kteří jsou členy role. Každá role je definována s jediným oprávněním: Žádné, Čtení, Čtení a zpracování, Zpracování nebo Správce. Role je možné definovat při vytváření modelu pomocí Správce rolí. Po nasazení modelu můžete role spravovat pomocí aplikace SQL Server Management Studio (SSMS). Další, najdete v části toolearn [role](https://docs.microsoft.com/sql/analysis-services/tabular-models/roles-ssas-tabular).
  
> [!NOTE]  
> Vytváření rolí není nutné toocomplete v tomto kurzu. Ve výchozím nastavení hello účet, který jste právě přihlášeni, má oprávnění správce na hello modelu. Ale pro ostatní uživatelé ve vaší organizaci toobrowse pomocí sestav klienta, musíte vytvořit alespoň jednu roli čtení oprávnění a tyto uživatele přidejte jako členy.  
  
Vytvoříte tři role:  
  
-   **Vedoucí prodeje** – tato role může obsahovat uživatele ve vaší organizaci, pro které chcete objekty modelu tooall oprávnění pro čtení toohave a data.  
  
-   **Prodejní analytik USA** – tato role může obsahovat uživatele ve vaší organizaci, pro které chcete jenom data možné toobrowse toobe související toosales ve Spojených státech amerických hello. Pro tuto roli použijete toodefine vzorce DAX *řádkový filtr*, což omezuje členy toobrowse dat pouze pro hello Spojených státech amerických.  
  
-   **Správce** – tato role může obsahovat uživatele, pro které chcete toohave oprávnění správce, které umožňuje neomezený přístup a oprávnění tooperform úlohy správy na hello modelové databáze.  
  
Protože jsou jedinečné účty uživatelů a skupin systému Windows ve vaší organizaci, můžete přidat účty z vaší organizace konkrétní toomembers. Ale pro účely tohoto kurzu můžete také ponechat hello členy prázdné. Testování účinku hello každou roli později v lekce 12: analyzovat v aplikaci Excel.  
  
Odhadovaný čas toocomplete této lekci: **15 minut**  
  
## <a name="prerequisites"></a>Požadavky  
Toto téma je součástí kurzu tabelárního modelování, který by se měl dokončit v daném pořadí. Před provedením úlohy hello v této lekci, by měl mít dokončit předchozí lekci hello: [lekce 10: vytvoření oddílů](../tutorials/aas-lesson-10-create-partitions.md).  
  
## <a name="create-roles"></a>Vytvoření rolí  
  
#### <a name="toocreate-a-sales-manager-user-role"></a>toocreate roli uživatele správce prodeje  
  
1.  V Průzkumníku tabelárních modelů klikněte pravým tlačítkem na **Role** > **Role**.  
  
2.  Ve Správci rolí klikněte na **Nový**.  
  
3.  Klikněte na tlačítko Nová role hello a potom v hello **název** sloupce, přejmenujte roli hello příliš**vedoucí prodeje**.  
  
4.  V hello **oprávnění** sloupce, klikněte na tlačítko hello rozevíracího seznamu a pak vyberte hello **čtení** oprávnění. 

    ![aas-lesson11-new-role](../tutorials/media/aas-lesson11-new-role.png) 
  
5.  Volitelné: Klikněte na tlačítko hello **členy** a pak klikněte **přidat**. V hello **vybrat uživatele nebo skupiny** dialogové okno, zadejte uživatele Windows hello nebo skupiny z vaší organizace mají tooinclude v roli hello.  
  
#### <a name="toocreate-a-sales-analyst-us-user-role"></a>toocreate roli uživatele analytik prodej USA  
  
1.  Ve Správci rolí klikněte na **Nový**.    
  
2.  Přejmenujte roli hello příliš**prodej analytik USA**.  
  
3.  Přidělte této roli oprávnění **Čtení**.  
  
4.  Klikněte na kartu filtry řádek hello a pak pro hello **DimGeography** tabulky, sloupce filtru DAX hello, typ hello následující vzorec:  
  
    ```Administrator
    =DimGeography[CountryRegionCode] = "US" 
    ```
    
    Vzorec A řádkový filtr je třeba vyřešit tooa hodnota logická hodnota (TRUE/FALSE). Pomocí tohoto vzorce určíte, že pouze řádky s hello kód země/oblasti hodnotu "US" jsou viditelné toohello uživatele.  
    ![aas-lesson11-role-filter](../tutorials/media/aas-lesson11-role-filter.png) 
  
6.  Volitelné: Klikněte na tlačítko hello **členy** a pak klikněte **přidat**. V hello **vybrat uživatele nebo skupiny** dialogové okno, zadejte uživatele Windows hello nebo skupiny z vaší organizace mají tooinclude v roli hello.  
  
#### <a name="toocreate-an-administrator-user-role"></a>toocreate role uživatele správce  
  
1.  Klikněte na možnost **Nové**.  
  
2.  Přejmenujte roli hello příliš**správce**.  
  
3.  Přidělte této roli oprávnění **Správce**.  
  
4.  Volitelné: Klikněte na tlačítko hello **členy** a pak klikněte **přidat**. V hello **vybrat uživatele nebo skupiny** dialogové okno, zadejte uživatele Windows hello nebo skupiny z vaší organizace mají tooinclude v roli hello. 
  
  
## <a name="whats-next"></a>Co dále?
[Lekce 12: Analýza v aplikaci Excel](../tutorials/aas-lesson-12-analyze-in-excel.md)

  
  
