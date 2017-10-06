---
Title: aaa "Azure Analysis Services kurz lekce 12: analyzovat v aplikaci Excel | Microsoft Docs"Popis: Popisuje, jak toouse analyzovat v aplikaci Excel v hello Azure Analysis Services kurz projektu. služby: documentationcenter služby analysis services: '' Autor: minewiskan správce: erikre editor: '' značky: "

MS.AssetID: ms.service: ms.devlang služby analysis services: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26 nebo 2017 ms.author: owend
---
# <a name="lesson-12-analyze-in-excel"></a>Lekce 12: Analýza v aplikaci Excel

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

V této lekci použijete hello analyzovat v aplikaci Excel funkce tooopen Microsoft Excel, automaticky vytvořit pracovní prostor připojení toohello modelu a automaticky přidat do kontingenční tabulky toohello listu. Hello analyzovat v aplikaci Excel funkci je určen tooprovide rychlý a snadný způsob tootest hello účinnosti modelu návrh předchozí toodeploying modelu. V této lekci nebudete provádět žádné analýzy dat. účelem této lekci Hello je toofamiliarize, vytvářet hello model, pomocí nástrojů hello můžete použít tootest návrhu modelu.   
  
toocomplete této lekci Excel musí být nainstalován na hello stejného počítače jako rozšíření SSDT.
  
Odhadovaný čas toocomplete této lekci: **pět minut**  
  
## <a name="prerequisites"></a>Požadavky  
Toto téma je součástí kurzu tabelárního modelování, který by se měl dokončit v daném pořadí. Před provedením úlohy hello v této lekci, by měl mít dokončit předchozí lekci hello: [lekce 11: vytvoření role](../tutorials/aas-lesson-11-create-roles.md).  
  
## <a name="browse-using-hello-default-and-internet-sales-perspectives"></a>Procházet pomocí perspektivy výchozí a Internetu prodejní hello  
V těchto první úloh, můžete procházet váš model pomocí obou hello výchozí perspektivu, která obsahuje všechny objekty modelu, a také pomocí perspektivy internetového prodeje hello jste dříve. Hello perspektivy internetového prodeje vyloučí hello zákazníka tabulky objektu.  
  
#### <a name="toobrowse-by-using-hello-default-perspective"></a>toobrowse pomocí hello výchozí perspektivu  
  
1.  Klikněte na tlačítko hello **modelu** nabídky > **analyzovat v aplikaci Excel**.  
  
2.  V hello **analyzovat v aplikaci Excel** dialogové okno, klikněte na tlačítko **OK**.  
  
    Otevře se aplikace Excel s novým sešitem. Připojení ke zdroji dat je vytvořen pomocí hello aktuální uživatelský účet a hello výchozí perspektivu je použité toodefine zobrazit pole. Kontingenční tabulky se automaticky přidá toohello listu.  
  
3.  V aplikaci Excel v hello **seznamu polí kontingenční tabulky**, Všimněte si hello **DimDate** a **FactInternetSales** zobrazí skupiny měr. Hello **DimCustomer**, **DimDate**, **DimGeography**, **DimProduct**, **DimProductCategory**, **DimProductSubcategory**, a **FactInternetSales** tabulky s jejich příslušné sloupce se také zobrazí.  
  
4.  Zavřete bez uložení hello sešitu aplikace Excel.  
  
#### <a name="toobrowse-by-using-hello-internet-sales-perspective"></a>toobrowse pomocí perspektivy internetového prodeje hello  
  
1.  Klikněte na tlačítko hello **modelu** nabídce a pak klikněte na tlačítko **analyzovat v aplikaci Excel**.  
  
2.  V hello **analyzovat v aplikaci Excel** dialogové okno, ponechejte **aktuálního uživatele systému Windows** vybraná, pak v hello **perspektivy** listbox rozevíracího seznamu, vyberte **Internet prodeje** a potom klikněte na **OK**. 
    
    ![aas-lesson12-perspective](../tutorials/media/aas-lesson12-perspective.png)
    
3.  V aplikaci Excel v **pole kontingenční tabulky**, Všimněte si hello DimCustomer tabulky jsou vyloučeny ze seznamu polí hello.  
    
    ![aas-lesson12-fields](../tutorials/media/aas-lesson12-fields.png)
    
4.  Zavřete bez uložení hello sešitu aplikace Excel.  
  
## <a name="browse-by-using-roles"></a>Procházení s využitím rolí  
Role jsou důležitou součástí každého tabelárního modelu. Bez alespoň jednu roli uživatele toowhich přidány jako členové, uživatelé nemůže získat přístup a analýza dat pomocí modelu. Hello analyzovat v aplikaci Excel funkce poskytuje způsob, jak můžete tootest hello rolí, které jste definovali.  
  
#### <a name="toobrowse-by-using-hello-sales-manager-user-role"></a>toobrowse pomocí role uživatele Správce prodejních hello  
  
1.  V sadě SSDT, klikněte na tlačítko hello **modelu** nabídce a pak klikněte na tlačítko **analyzovat v aplikaci Excel**.  
  
2.  V **zadejte hello uživatelské jméno nebo roli toouse tooconnect toohello modelu**, vyberte **Role**a poté v hello listbox rozevíracího seznamu, vyberte **vedoucí prodeje**a pak klikněte na tlačítko  **OK**.  
  
    Otevře se aplikace Excel s novým sešitem. Automaticky se vytvoří kontingenční tabulka. Hello seznamu polí kontingenční tabulka obsahuje všechny hello data dostupná pole v novém modelu.  
      
3.  Zavřete bez uložení hello sešitu aplikace Excel.  
  
## <a name="whats-next"></a>Co dále?
Další lekce přejděte toohello: [lekce 13: nasazení](../tutorials/aas-lesson-13-deploy.md).

  
  
  
