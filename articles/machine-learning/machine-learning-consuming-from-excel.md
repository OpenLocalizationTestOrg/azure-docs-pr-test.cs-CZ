---
title: "Webová služba Machine Learning z Excelu aaaConsume | Microsoft Docs"
description: "Využívají webové služby Azure Machine Learning z Excelu"
services: machine-learning
documentationcenter: 
author: tedway
manager: jhubbard
editor: cgronlun
ms.assetid: 3f3cdd2f-1816-487e-ab78-530e01e9788f
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 2/13/2017
ms.author: tedway
ms.openlocfilehash: e2e8bbf7ba75b6618a0285539555ce175ec03c1a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="consuming-an-azure-machine-learning-web-service-from-excel"></a>Využívání webové služby Azure Machine Learning z Excelu
 Azure Machine Learning Studio umožňuje snadno toocall webové služby přímo z aplikace Excel bez hello potřebovat toowrite žádný kód.

Pokud používáte aplikace Excel 2013 (nebo novější) nebo Excel Online, pak doporučujeme použít hello Excel [doplněk aplikace Excel](machine-learning-excel-add-in-for-web-services.md).

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="steps"></a>Kroky
Publikování webové služby. [Tato stránka](machine-learning-walkthrough-5-publish-web-service.md) vysvětluje, jak toodo ho. Aktuálně funkce sešitu aplikace Excel hello je podporována pouze pro služby požadavků a odpovědí, které mají jediného výstupu (tedy jediný vyhodnocování štítek). 

Až budete mít webovou službu, klikněte na hello **webové služby** části na hello nalevo od hello studio a potom vyberte hello webové služby tooconsume z Excelu.

**Classic webové služby**

1. Na hello **řídicí panel** kartě hello webové služby je řádek pro hello **požadavků a odpovědí** služby. Pokud tato služba měli jediného výstupu, měli byste vidět hello **stáhnout sešitu aplikace Excel** odkaz v daném řádku.
   
    ![][1]
2. Klikněte na **stáhnout sešitu aplikace Excel**.

**Novou webovou službu**

1. Webová služba Azure Machine Learning portálu hello, vyberte **spotřebě**.
2. Na stránce spotřebě hello, hello **webové služby spotřeba možnosti** klikněte ikonu hello aplikace Excel.

**Sešitem hello**

1. Sešit otevřít hello.
2. Zobrazí se upozornění zabezpečení; Klikněte na hello **povolit úpravy** tlačítko.
   
    ![][2]
3. Zobrazí se upozornění zabezpečení. Klikněte na hello **povolit obsah** tlačítko toorun makra tabulky.
   
    ![][3]
4. Jakmile jsou povolené makra, je generována tabulku. Sloupce v modré jsou požadované jako vstup do hello RRS webové služby, nebo **parametry**. Všimněte si výstup hello hello RRS služby **PŘEDPOVĚDĚT hodnoty** zeleně. Když jsou vyplněny všechny sloupce pro daný řádek, hello sešitu automaticky volá hello vyhodnocování rozhraní API a zobrazí hello skóre pro magnitudu výsledky.
   
    ![][4]
5. tooscore více než jeden řádek, výplň hello druhý řádek s daty a hello předpovědět, že vytváří hodnoty. Několik řádků můžete vložit i najednou.

Můžete použít některou z funkcí aplikace Excel hello (grafů, power map, podmíněného formátování, atd.) s hello předpovědět hodnoty toohelp vizualizovat hello data.    

## <a name="sharing-your-workbook"></a>Sdílení sešitu
Klíč rozhraní API toowork hello makra, musí být součástí hello tabulky. To znamená, že by měly sdílet hello sešitu pouze s entit nebo jednotlivce, kterým důvěřujete.

## <a name="automatic-updates"></a>Automatické aktualizace
Volání na záznamy o prostředku se provádí v těchto dvou situacích:

1. Hello poprvé řádek má obsah ve všech jeho **parametry**
2. Kdykoli se některé z hello **parametry** změny v řádku, který měl všechny jeho **parametry** zadali.

[1]: ./media/machine-learning-consuming-from-excel/excellink.png
[2]: ./media/machine-learning-consuming-from-excel/enableeditting.png
[3]: ./media/machine-learning-consuming-from-excel/enablecontent.png
[4]: ./media/machine-learning-consuming-from-excel/sampletable.png
