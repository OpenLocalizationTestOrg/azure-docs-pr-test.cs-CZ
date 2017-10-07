---
title: "aaaConnect tooAzure služby Analysis Services s Power BI | Microsoft Docs"
description: "Zjistěte, jak tooconnect tooan Azure Analysis Services serveru pomocí Power BI."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: f6c4cdec6edb92900ad2e552e23a4d9172ba9b84
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-with-power-bi"></a>Připojit k Power BI

Jakmile jste vytvořili serveru v Azure a nasadí tooit tabulkový model, uživatelé ve vaší organizaci jsou připravené tooconnect a začít zkoumat data. 

> [!TIP]
> Být jisti toouse hello nejnovější verzi [Power BI Desktop](https://powerbi.microsoft.com/desktop/).
> 
> 
  
## <a name="connect-in-power-bi-desktop"></a>Připojení v Power BI Desktop

1. V Power BI Desktop, klikněte na **načíst Data** > **Azure** > **databáze Azure Analysis Services**.

2. V **Server**, zadejte název serveru hello. 
    
    Být jisti tooinclude hello úplnou adresu URL. Například asazure://westcentralus.asazure.windows.net/advworks.

3. V **databáze**, pokud znáte název hello hello databázi tabulkového modelu nebo perspektivy chcete tooconnect k, vložte jej zde. Jinak můžete ponechat toto pole prázdné a vybrat databázi nebo perspektivy později.

4. Ponechte výchozí hello **připojit za provozu** možnost a potom stiskněte klávesu **Connect**. 

5. Po zobrazení výzvy zadejte své přihlašovací údaje. 

6. V **Navigátor**, rozbalte hello server a pak vyberte hello modelu nebo perspektivy chcete tooconnect k, pak klikněte na tlačítko **Connect**. Klikněte na tlačítko modelu nebo perspektivy tooshow všechny objekty hello tohoto zobrazení.

    Hello model se otevře v Power BI Desktop s prázdnou sestavou v zobrazení sestavy. Zobrazí se seznam polí Hello všechny objekty modelu nebyla skrytá. Stav připojení se zobrazí v pravém dolním rohu, hello.

## <a name="connect-in-power-bi-service"></a>Připojení v Power BI (služba)

1. Vytvořte soubor Power BI Desktop, který má model tooyour živé připojení na serveru.
2. V [Power BI](https://powerbi.microsoft.com), klikněte na tlačítko **načíst Data** > **soubory**. Vyhledejte a vyberte soubor.



## <a name="see-also"></a>Viz také
[Připojit tooAzure Analysis Services](analysis-services-connect.md)   
[Knihovny klienta](analysis-services-data-providers.md)

