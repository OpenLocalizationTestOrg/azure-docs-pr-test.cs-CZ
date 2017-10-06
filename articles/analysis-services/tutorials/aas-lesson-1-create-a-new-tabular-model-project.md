---
Title: aaa "Azure Analysis Services kurz Lekce 1: vytvoření nového projektu tabulkový model. | Microsoft Docs"Popis: Popisuje, jak server toocreate nové Azure Analysis Services kurz projektu. služby: documentationcenter služby analysis services: '' Autor: minewiskan správce: erikre editor: '' značky: "

MS.AssetID: ms.service: ms.devlang služby analysis services: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 01/06/2017 ms.author: owend
---
# <a name="lesson-1-create-a-tabular-model-project"></a>Lekce 1: Vytvoření projektu s tabelárním modelem

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

V této lekci použijete SQL Server Data Tools (SSDT) toocreate nový projekt tabulkový model na úroveň kompatibility 1400 hello. Jakmile bude nový projekt vytvořený, můžete začít přidávat data a vytvářet model. V této lekci také poskytuje stručný úvod toohello tabulkový model pro tvorbu prostředí v sadě SSDT.  
  
Odhadovaný čas toocomplete této lekci: **10 minut**  
  
## <a name="prerequisites"></a>Požadavky  
Toto téma je první lekce hello v tabulkový model vytváření kurzu. toocomplete to lekce, existuje několik předpokladů musíte toohave na místě. Další, najdete v části toolearn [Azure Analysis Services - společnosti Adventure Works kurzu](../tutorials/aas-adventure-works-tutorial.md).  
  
## <a name="create-a-new-tabular-model-project"></a>Vytvoření nového projektu s tabelárním modelem  
  
#### <a name="toocreate-a-new-tabular-model-project"></a>toocreate nový projekt tabulkový model.  
  
1.  V sadě SSDT na hello **soubor** nabídky, klikněte na tlačítko **nový** > **projektu**.  
  
2.  V hello **nový projekt** dialogové okno, rozbalte seznam **nainstalovaná** > **Business Intelligence** > **služby Analysis Services**a potom klikněte na **tabulkový projekt Analysis Services**.  
  
3.  V **název**, typ **AW Internet prodej**a poté zadejte umístění pro soubory projektu hello.  
  
    Ve výchozím nastavení **název řešení** hello stejný jako název projektu hello; však můžete zadat název jiné řešení.  
  
4.  Klikněte na **OK**.  
  
5.  V hello **tabulkový model designer** dialogové okno, vyberte **integrované prostoru**.  
  
    pracovní prostor Hello hostuje databázi tabulkového modelu s hello stejný název jako projekt hello během vytváření modelu. Integrované prostoru znamená, že rozšíření SSDT používá integrované instanci, odstraňuje hello nutné tooinstall samostatné instanci služby Analysis Services serveru jen pro vytváření modelu.
      
6.  V části **Úroveň kompatibility** vyberte **SQL Server 2017 / Azure Analysis Services (1400)**.   
 
    ![aas-lesson1-tmd](../tutorials/media/aas-lesson1-tmd.png)
      
    Pokud nevidíte SQL Server 2017 / Azure Analysis Services (1400) v úrovni listbox hello kompatibility, že nepoužíváte hello nejnovější verzi SQL Server Data Tools. tooget hello nejnovější verzi, najdete v části [nainstalovat SQL Server Data tools](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt).  
      
  
## <a name="understanding-hello-ssdt-tabular-model-authoring-environment"></a>Principy hello SSDT tabulkový model pro tvorbu prostředí  
Teď, když jste vytvořili nový projekt tabulkový model, Podívejme se na chvíli tooexplore hello tabulkový model pro tvorbu prostředí v sadě SSDT.  
  
Jakmile se váš projekt vytvoří, otevře se v SSDT. Na pravé strany v hello **tabulkový Model Explorer**, naleznete v zobrazení stromu hello objektů v modelu. Vzhledem k tomu, že ještě nebyly naimportovány dat, hello složky jsou prázdné. Kliknete pravým tlačítkem na objekt složky tooperform akce, podobně jako toohello řádku nabídek. Krocích tohoto kurzu použijete hello tabulkový Model Explorer toonavigate různé objekty ve vašem projektu modelu.

![aas-lesson1-tme](../tutorials/media/aas-lesson1-tme.png)

Klikněte na tlačítko hello **Průzkumníku řešení** kartě. Zde se zobrazí váš soubor **Model.bim**. Pokud nevidíte hello návrháře okno toohello doleva (hello prázdné okno s kartou Model.bim hello), v **Průzkumníku řešení**v části **AW Internet prodej projektu**, dvakrát klikněte na hello  **Model.bim** souboru. Hello Model.bim soubor obsahuje hello metadat pro váš projekt modelu. 

![aas-lesson1-se](../tutorials/media/aas-lesson1-se.png)
  
Klikněte na **Model.bim**. V hello **vlastnosti** okně se zobrazí vlastnosti modelu hello, nejdůležitější je hello **režimu DirectQuery** vlastnost. Tato vlastnost určuje, pokud hello model je nasazena v režimu v paměti (vypnuto) nebo v režimu DirectQuery (zapnuto). V tomto kurzu model vytvoříte a nasadíte v režimu In-Memory.

![aas-lesson1-properties](../tutorials/media/aas-lesson1-properties.png)
  
Když vytvoříte projekt modelu, jsou nastavené určité vlastnosti modelu automaticky podle toohello modelování dat nastavení, které lze zadat v hello **nástroje** nabídky > **možnosti** dialogové okno. Vlastnosti zálohování dat, prostoru uchovávání a Server pracovního prostoru zadejte, jak a kde databáze pracovního prostoru hello (model vytváření databáze) zálohovaných, uchovávají v paměti a vytvořené. V případě potřeby můžete tato nastavení později změnit, ale prozatím ponechte tyto vlastnosti tak, jak jsou.  

V **Průzkumníku řešení** klikněte pravým tlačítkem na **AW Internet Sales** (projekt) a potom klikněte na **Vlastnosti**. Hello **stránky vlastností prodej Internet AW** zobrazí se dialogové okno. Některé z těchto vlastností nastavíte později při nasazování modelu.  
  
Při instalaci rozšíření SSDT několika nových položek nabídky byly přidány toohello prostředí Visual Studio. Klikněte na tlačítko hello **modelu** nabídky. Tady můžete importovat data, aktualizujte data pracovního prostoru, procházení modelu v aplikaci Excel, vytvořte perspektivy a rolí, vyberte hello modelu zobrazení a nastavte možnosti výpočtu. Klikněte na tlačítko hello **tabulky** nabídky. Odtud můžete vytvářet a spravovat relace, zadávat nastavení tabulky kalendářních dat, vytvářet oddíly a upravovat vlastnosti tabulky. Pokud kliknete na tlačítko hello **sloupec** nabídky, můžete přidat a odstranit sloupce v tabulce, zmrazení sloupců a zadat pořadí řazení. Rozšíření SSDT přidává také některé tlačítek panelu toohello. Nejvhodnější je toocreate funkce AutoSum hello standardní agregaci měr pro vybraný sloupec. Poskytují další tlačítka panelu nástrojů Rychlý přístup toofrequently používá funkce a příkazy.  
  
Prozkoumání některých hello dialogová okna a umístění pro různé funkce konkrétní tooauthoring tabulkové modely. Při některé položky nejsou aktivní, můžete získat představu o hello tabulkový model pro tvorbu prostředí.  
  

## <a name="whats-next"></a>Co dále?
[Lekce 2: Získání dat](../tutorials/aas-lesson-2-get-data.md)

  
  
  
