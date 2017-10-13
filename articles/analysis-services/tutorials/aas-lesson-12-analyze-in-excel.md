---
title: "Kurz služby Azure Analysis Services – Lekce 12: Analýza v aplikaci Excel | Dokumentace Microsoftu"
description: "Popisuje, jak používat funkci Analýza v aplikaci Excel v projektu Kurz služby Azure Analysis Services."
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
ms.date: 09/20/2017
ms.author: owend
ms.openlocfilehash: e257862a88d39b96360703117f544c43e82b0e3d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="lesson-12-analyze-in-excel"></a>Lekce 12: Analýza v aplikaci Excel

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

V této lekci pomocí funkce Analýza v aplikaci Excel otevřete aplikaci Microsoft Excel, automaticky vytvoříte připojení k pracovnímu prostoru modelu a automaticky do listu přidáte kontingenční tabulku. Smyslem funkce Analýza v aplikaci Excel je poskytnout rychlý a snadný způsob, jak otestovat efektivnost návrhu modelu před jeho nasazením. V této lekci nebudete provádět žádné analýzy dat. Účelem této lekce je seznámit vás jako autora modelu s nástroji, pomocí kterých můžete otestovat návrh modelu.   
  
K dokončení této lekce je nutné, aby byla aplikace Excel nainstalována na stejném počítači jako SSDT.
  
Odhadovaný čas dokončení této lekce: **5 minut**  
  
## <a name="prerequisites"></a>Požadavky  
Toto téma je součástí kurzu tabelárního modelování, který by se měl dokončit v daném pořadí. Před provedením úkolů v této lekci byste měli mít dokončenou předchozí lekci: [Lekce 11: Vytvoření rolí](../tutorials/aas-lesson-11-create-roles.md).  
  
## <a name="browse-using-the-default-and-internet-sales-perspectives"></a>Procházení s využitím výchozí perspektivy a perspektivy Internet Sales  
V rámci těchto prvních úkolů budete procházet model s využitím výchozí perspektivy, která zahrnuje všechny objekty modelu, a také s využitím perspektivy Internet Sales, kterou jste vytvořili dříve. Perspektiva Internet Sales nezahrnuje objekt tabulky se zákazníky.  
  
#### <a name="to-browse-by-using-the-default-perspective"></a>Procházení s využitím výchozí perspektivy  
  
1.  Klikněte na nabídku **Model** > **Analyzovat v aplikaci Excel**.  
  
2.  V dialogovém okně **Analýza v aplikaci Excel** klikněte na **OK**.  
  
    Otevře se aplikace Excel s novým sešitem. Vytvoří se připojení ke zdroji dat pomocí aktuálního uživatelského účtu a k definování zobrazitelných polí se použije výchozí perspektiva. Do listu se automaticky přidá kontingenční tabulka.  
  
3.  V aplikaci Excel si všimněte, že v části **Seznam polí kontingenční tabulky** se zobrazí skupiny měr **DimDate** a **FactInternetSales**. Zobrazí se také tabulky **DimCustomer**, **DimDate**, **DimGeography**, **DimProduct**, **DimProductCategory**, **DimProductSubcategory** a **FactInternetSales** s příslušnými sloupci.  
  
4.  Zavřete aplikaci Excel bez ukládání sešitu.  
  
#### <a name="to-browse-by-using-the-internet-sales-perspective"></a>Procházení s využitím perspektivy Internet Sales  
  
1.  Klikněte na nabídku **Model** a potom na **Analyzovat v aplikaci Excel**.  
  
2.  V dialogovém okně **Analýza v aplikaci Excel** ponechte vybranou možnost **Aktuální uživatel systému Windows**, v rozevíracím seznamu **Perspektiva** vyberte **Internet Sales** a potom klikněte na **OK**. 
    
    ![aas-lesson12-perspective](../tutorials/media/aas-lesson12-perspective.png)
    
3.  V aplikaci Excel si všimněte, že v části **Pole kontingenční tabulky** není tabulka DimCustomer zahrnuta do seznamu polí.  
    
    ![aas-lesson12-fields](../tutorials/media/aas-lesson12-fields.png)
    
4.  Zavřete aplikaci Excel bez ukládání sešitu.  
  
## <a name="browse-by-using-roles"></a>Procházení s využitím rolí  
Role jsou důležitou součástí každého tabelárního modelu. Pokud nemáte alespoň jednu roli, do které se uživatelé přidávají jako členové, pak uživatelé nebudou mít přístup k datům a nebudou je moci analyzovat pomocí vašeho modelu. Funkce Analýza v aplikaci Excel nabízí způsob, jak otestovat role, které jste definovali.  
  
#### <a name="to-browse-by-using-the-sales-manager-user-role"></a>Procházení s využitím role uživatele Manažer prodeje  
  
1.  V SSDT klikněte na nabídku **Model** a potom na **Analyzovat v aplikaci Excel**.  
  
2.  V části **Zadejte uživatelské jméno nebo roli pro připojení k modelu** vyberte **Role**, v rozevíracím seznamu vyberte **Manažer prodeje** a potom klikněte na **OK**.  
  
    Otevře se aplikace Excel s novým sešitem. Automaticky se vytvoří kontingenční tabulka. Seznam polí kontingenční tabulky zahrnuje všechna datová pole, která jsou v novém modelu k dispozici.  
      
3.  Zavřete aplikaci Excel bez ukládání sešitu.  
  
## <a name="whats-next"></a>Co dále?
Přejděte na další lekci: [Lekce 13: Nasazení](../tutorials/aas-lesson-13-deploy.md).

  
  
  
