---
title: "zásady přístupu aaaData v Azure časové řady přehledy | Microsoft Docs"
description: "V tomto kurzu zjistíte, toomanage zásady přístupu k datům v přehledy časové řady"
keywords: 
services: time-series-insights
documentationcenter: 
author: op-ravi
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/01/2017
ms.author: omravi
ms.openlocfilehash: f286d26c8c5c851c523e9384760dc4b10711fa3f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="grant-data-access-tooa-time-series-insights-environment-using-azure-portal"></a>Udělení přístupu tooa časové řady Statistika prostředí datového pomocí portálu Azure

V prostředích Time Series Insights jsou dva nezávislé typy zásad přístupu:

* Zásady přístupu ke správě
* Zásady přístupu k datům

Oba typy zásad udělují objektům zabezpečení Azure Active Directory (uživatelům a aplikacím) různá oprávnění ke konkrétnímu prostředí. Hello objekty (uživatelé a aplikace) musí patřit toohello active directory (nebo "Klientovi Azure") přidružené k předplatnému hello obsahující hello prostředí.

Zásady správy přístupu udělit oprávnění související toohello konfiguraci hello prostředí, například
*   Vytváření a odstraňování hello prostředí zdroje událostí referenční datové sady, a
*   Správa hello zásady přístupu k datům.

Zásady přístupu k datům udělit oprávnění tooissue dotazy na data, pracovat s daty odkaz v prostředí hello a sdílet uložené dotazy a perspektivy přidružený hello prostředí.

Hello dva druhy zásad povolí jasně oddělené mezi toohello správu přístupu hello prostředí a přístup k datům toohello uvnitř hello prostředí. Například je možné toosetup prostředí tak, aby hello vlastníka nebo autora hello prostředí se odebere z přístupu k datům hello. A také uživatelům a službám, které jsou povoleny tooread data z prostředí hello může být poskytnuta žádná konfigurace toohello přístup hello prostředí.

## <a name="grant-data-access"></a>Udělení přístupu k datům
Hello následující kroky ukazují, jak přístup k datům toogrant pro hlavní název uživatele:

1.  Přihlaste se toohello [portál Azure](https://portal.azure.com).
2.  V nabídce hello na levé straně hello hello portálu Azure klikněte na tlačítko "Všechny prostředky".
3.  Vyberte vaše prostředí Time Series Insights.

  ![Spravovat hello časové řady Statistika zdroj - prostředí](media/data-access/getstarted-grant-data-access1.png)

4.  Vyberte Přístup k rovině dat a klikněte na Přidat.

  ![Spravovat zdroje časové řady Statistika hello – přidání](media/data-access/getstarted-grant-data-access2.png)

5.  Klikněte na Vybrat uživatele.
6.  Vyhledat a vybrat uživatele hello e-mailem.
7.  Klikněte na Vybrat v okně Vybrat uživatele.

  ![Spravovat hello časové řady Statistika zdroj - vybrat uživatele](media/data-access/getstarted-grant-data-access3.png)

8.  Klikněte na Vybrat roli.
9.  Pokud chcete tooallow uživatele toochange referenční data a sdílet s jinými uživateli hello prostředí uložené dotazy a perspektivy, vyberte "Přispěvatel". V opačném případě vyberte "Čtečky" tooallow uživatele dotaz na data v prostředí hello a ukládejte dotazy za osobní (není sdílený) v prostředí hello.
10. V okně "Vybrat roli" hello, klikněte na tlačítko "Ok".

  ![Spravovat hello časové řady Statistika zdrojem – vyberte role](media/data-access/getstarted-grant-data-access4.png)

11. V okně "Vybrat roli uživatele" hello, klikněte na tlačítko "Ok".
12. Měli byste vidět tohle:

  ![Spravovat hello časové řady Statistika zdrojem – výsledky](media/data-access/getstarted-grant-data-access5.png)

## <a name="next-steps"></a>Další kroky

* [Vytvoření zdroje událostí](time-series-insights-add-event-source.md)
* [Odesílání událostí](time-series-insights-send-events.md) toohello zdroj události
* Zobrazení prostředí na [portálu Time Series Insights](https://insights.timeseries.azure.com)
