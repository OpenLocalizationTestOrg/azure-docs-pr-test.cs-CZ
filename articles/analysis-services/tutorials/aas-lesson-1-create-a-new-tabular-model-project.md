---
title: "Kurz služby Azure Analysis Services – Lekce 1: Vytvoření nového projektu s tabelárním modelem | Dokumentace Microsoftu"
description: "Popisuje, jak vytvořit nový projekt Kurz služby Azure Analysis Services."
services: analysis-services
documentationcenter: 
author: Minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 06/01/2017
ms.author: owend
ms.openlocfilehash: d523e3e103b4c351d01af6f1eb3c396f9a63016a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="lesson-1-create-a-tabular-model-project"></a>Lekce 1: Vytvoření projektu s tabelárním modelem

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

V této lekci pomocí SQL Server Data Tools (SSDT) vytvoříte nový projekt s tabelárním modelem na úrovni kompatibility 1400. Jakmile bude nový projekt vytvořený, můžete začít přidávat data a vytvářet model. Tato lekce vás také stručně seznámí s prostředím pro vytváření tabelárních modelů v SSDT.  
  
Odhadovaný čas dokončení této lekce: **10 minut**  
  
## <a name="prerequisites"></a>Požadavky  
Toto téma je první lekcí kurzu vytváření tabelárního modelu. K dokončení této lekce je nutné splnit několik požadavků. Další informace najdete v tématu [Azure Analysis Services – Kurz Adventure Works](../tutorials/aas-adventure-works-tutorial.md).  
  
## <a name="create-a-new-tabular-model-project"></a>Vytvoření nového projektu s tabelárním modelem  
  
#### <a name="to-create-a-new-tabular-model-project"></a>Vytvoření nového projektu s tabelárním modelem  
  
1.  V SSDT v nabídce **Soubor** klikněte na **Nový** > **Projekt**.  
  
2.  V dialogovém okně **Nový projekt** rozbalte **Nainstalované** > **Business Intelligence** > **Analysis Services** a potom klikněte na **Tabelární projekt Analysis Services**.  
  
3.  V části **Název** zadejte **AW Internet Sales** a potom zadejte umístění pro soubory projektu.  
  
    **Název řešení** je ve výchozím nastavení stejný jako název projektu, můžete ale zadat jiný název řešení.  
  
4.  Klikněte na **OK**.  
  
5.  V dialogovém okně **Návrhář tabelárních modelů** vyberte **Integrovaný pracovní prostor**.  
  
    Pracovní prostor je během vytváření modelu hostitelem databáze tabelárního modelu se stejným názvem jako projekt. Integrovaný pracovní prostor znamená, že SSDT využívá integrovanou instanci a tím eliminuje nutnost instalace samostatné instance serveru Analysis Services jenom pro účely vytváření modelu.
      
6.  V části **Úroveň kompatibility** vyberte **SQL Server 2017 / Azure Analysis Services (1400)**.   
 
    ![aas-lesson1-tmd](../tutorials/media/aas-lesson1-tmd.png)
      
    Pokud se v seznamu Úroveň kompatibility možnost SQL Server 2017 / Azure Analysis Services (1400) nezobrazuje, nepoužíváte nejnovější verzi SQL Server Data Tools. Pokud chcete získat nejnovější verzi, přečtěte si článek o [instalaci SQL Server Data Tools](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt).  
      
  
## <a name="understanding-the-ssdt-tabular-model-authoring-environment"></a>Principy prostředí pro vytváření tabelárních modelů v SSDT  
Teď, když jste vytvořili nový projekt s tabelárním modelem, věnujte chvíli seznámení se s prostředím pro vytváření tabelárních modelů v SSDT.  
  
Jakmile se váš projekt vytvoří, otevře se v SSDT. Na pravé straně v **Průzkumníku tabelárních projektů** je stromové zobrazení objektů ve vašem modelu. Vzhledem k tomu, že jste ještě neimportovali žádná data, jsou složky prázdné. Na složku objektů můžete kliknout pravým tlačítkem a provádět akce podobně jako na řádku nabídek. V jednotlivých krocích tohoto kurzu budete Průzkumníka tabelárních modelů používat k navigaci mezi různými objekty v modelu s projektem.

![aas-lesson1-tme](../tutorials/media/aas-lesson1-tme.png)

Klikněte na kartu **Průzkumník řešení**. Zde se zobrazí váš soubor **Model.bim**. Pokud se okno návrháře na levé straně (prázdné okno s kartou Model.bim) nezobrazuje, na kartě **Průzkumník řešení** v části **Projekt AW Internet Sales** dvakrát klikněte na soubor **Model.bim**. Soubor Model.bim obsahuje metadata pro váš projekt s modelem. 

![aas-lesson1-se](../tutorials/media/aas-lesson1-se.png)
  
Klikněte na **Model.bim**. V okně **Vlastnosti** se zobrazí vlastnosti projektu, z nichž nejdůležitější je vlastnost **Režim DirectQuery**. Tato vlastnost určuje, jestli je model nasazený v režimu In-Memory (vypnuto) nebo DirectQuery (zapnuto). V tomto kurzu model vytvoříte a nasadíte v režimu In-Memory.

![aas-lesson1-properties](../tutorials/media/aas-lesson1-properties.png)
  
Při vytvoření projektu s modelem se určité vlastnosti nastaví automaticky podle nastavení Modelování dat, které můžete zadat v nabídce **Nástroje** > dialogové okno **Možnosti**. Vlastnosti Zálohování dat, Uchování pracovního prostoru a Server pracovního prostoru určují, jak a kde se zálohuje, uchovává v paměti a sestavuje databáze pracovního prostoru (vaše databáze pro vytvoření modelu). V případě potřeby můžete tato nastavení později změnit, ale prozatím ponechte tyto vlastnosti tak, jak jsou.  

V **Průzkumníku řešení** klikněte pravým tlačítkem na **AW Internet Sales** (projekt) a potom klikněte na **Vlastnosti**. Zobrazí se dialogové **Okno vlastností AW Internet Sales**. Některé z těchto vlastností nastavíte později při nasazování modelu.  
  
Při instalaci SSDT se do prostředí sady Visual Studio přidalo několik nových položek nabídky. Klikněte na nabídku **Model**. Odtud můžete importovat data, aktualizovat data pracovního prostoru, procházet model v aplikaci Excel, vytvářet perspektivy a role, vybírat zobrazení modelu a nastavovat možnosti výpočtů. Klikněte na nabídku **Tabulka**. Odtud můžete vytvářet a spravovat relace, zadávat nastavení tabulky kalendářních dat, vytvářet oddíly a upravovat vlastnosti tabulky. Pokud kliknete na nabídku **Sloupec**, můžete přidávat a odstraňovat sloupce tabulky, ukotvovat sloupce a určovat pořadí řazení. SSDT také přidává několik tlačítek na panel nástrojů. Nejužitečnější je funkce AutoSum, která vytvoří standardní míru agregace pro vybraný sloupec. Další tlačítka na panelu nástrojů poskytují rychlý přístup k často používaným funkcím a příkazům.  
  
Prozkoumejte některé z dialogových oken a umístění různých funkcí specifických pro vytváření tabelárních modelů. Přestože některé položky ještě nejsou aktivní, můžete získat dobrou představu o prostředí pro vytváření tabelárních modelů.  
  

## <a name="whats-next"></a>Co dále?
[Lekce 2: Získání dat](../tutorials/aas-lesson-2-get-data.md)

  
  
  
