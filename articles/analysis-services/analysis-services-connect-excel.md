---
title: "aaaConnect tooAzure služby Analysis Services v aplikaci Excel | Microsoft Docs"
description: "Zjistěte, jak tooconnect tooan Azure Analysis Services serveru pomocí aplikace Excel."
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
ms.openlocfilehash: 175b83e7d936716a626aa4b3bf22b5598bb983d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-with-excel"></a>Propojit s Excelem

Jakmile jste vytvořili serveru v Azure a nasadí tooit tabulkový model, jste připravené tooconnect a začít zkoumat data.


## <a name="connect-in-excel"></a>Připojení v aplikaci Excel

Připojení serveru tooa v aplikaci Excel pomocí příkazu Get Data v aplikaci Excel 2016 podporuje. Připojení pomocí hello Průvodce importem tabulky v doplňku Power Pivot není podporováno. 

**tooconnect v aplikaci Excel 2016**

1. V aplikaci Excel 2016 na hello **Data** pásu karet, klikněte na tlačítko **načíst externí Data** > **z jiných zdrojů** > **ze služby Analysis Services** .

2. V hello Průvodce datovým připojením v **název serveru**, zadejte název serveru hello včetně protokolu a identifikátor URI. Potom v **přihlašovací údaje**, vyberte **hello použijte následující uživatelské jméno a heslo**a pak zadejte hello organizační uživatelské jméno, například nancy@adventureworks.coma heslo.

    ![Připojení z aplikace Excel přihlášení](./media/analysis-services-connect-excel/aas-connect-excel-logon.png)

3. V **vybrat databázi a tabulku**, vyberte databázi hello a modelu nebo perspektivy a pak klikněte na tlačítko **Dokončit**.
   
    ![Připojení z vyberte model aplikace Excel](./media/analysis-services-connect-excel/aas-connect-excel-select.png)


## <a name="see-also"></a>Viz také
[Knihovny klienta](analysis-services-data-providers.md)   
[Správa serveru](analysis-services-manage.md)     


