---
title: "aaaAutomate Azure Application Insights zpracuje pomocí Logic Apps."
description: "Zjistěte, jak rychle procesy můžete automatizovat opakované přidáním hello Application Insights konektor tooyour logiku aplikace."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: bwren
ms.openlocfilehash: 8565aadf0644ffb10da8a0964821901907db2e27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automate-application-insights-processes-by-using-logic-apps"></a>Automatizovat procesy Application Insights pomocí Logic Apps

Se přistihnete opakovaně spuštění hello stejné dotazů na váš telemetrická data toocheck zda správně pracuje služby? Jsou vypadající tooautomate můžete tyto dotazy pro hledání trendů a anomálií a následně vytvořit vlastní pracovní postupy je obcházet? Hello konektor služby Azure Application Insights (preview) pro Logic Apps je hello nejvhodnější nástroj pro tento účel.

Díky této integraci můžete automatizovat procesy množství bez nutnosti napsat jediný řádek kódu. Můžete vytvořit aplikaci logiky s hello Application Insights konektor tooquickly automatizovat jakýkoli proces Application Insights. 

Můžete přidat i další akce. Díky funkci Hello Logic Apps služby Azure App Service stovky akce k dispozici. Například pomocí aplikace logiky můžete můžete automaticky odesílat e-mailových oznámení nebo vytvoření chyby ve Visual Studio Team Services. Můžete použít také jeden z hello k dispozici mnoho [šablony](https://docs.microsoft.com/azure/logic-apps/logic-apps-use-logic-app-templates) toohelp urychlení hello proces vytváření aplikace logiky. 

## <a name="create-a-logic-app-for-application-insights"></a>Vytvoření aplikace logiky pro službu Application Insights

V tomto kurzu zjistíte, jak toocreate aplikace logiky, která používá hello Analytics autocluster algoritmus toogroup atributy v hello data pro webovou aplikaci. tok Hello automaticky odesílá hello výsledky e-mailem, pouze příklad, jak můžete použít Application Insights analýzy a Logic Apps společně. 

### <a name="step-1-create-a-logic-app"></a>Krok 1: Vytvoření aplikace logiky
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. V hello **nový** podokně, vyberte **Web + mobilní**a potom vyberte **aplikace logiky**.

    ![Nové okno aplikace logiky](./media/automate-with-logic-apps/logicapp1.png)

### <a name="step-2-create-a-trigger-for-your-logic-app"></a>Krok 2: Vytvoření aktivační událost pro svou aplikaci logiky
1. V hello **návrhář aplikace na základě logiky** okno, v části **začínat běžné aktivační událost**, vyberte **opakování**.

    ![Okno Návrhář aplikace logiky](./media/automate-with-logic-apps/logicapp2.png)

2. V hello **frekvence** vyberte **den** a pak na hello **Interval** zadejte **1**.

    ![Návrhář aplikace logiky "Recurrence" okna](./media/automate-with-logic-apps/step2b.png)

### <a name="step-3-add-an-application-insights-action"></a>Krok 3: Přidání Application Insights akce
1. Klikněte na tlačítko **nový krok**a potom klikněte na **přidat akci**.

2. V hello **vybrat akci** vyhledávací pole, typ **Azure Application Insights**.

3. V části **akce**, klikněte na tlačítko **Azure Application Insights – vizualizovat analýzy dotaz Preview**.

    !["Vyberte akci" návrhář aplikace na základě logiky – okno](./media/automate-with-logic-apps/flow2.png)

### <a name="step-4-connect-tooan-application-insights-resource"></a>Krok 4: Připojení tooan prostředek Application Insights

toocomplete tohoto kroku budete potřebovat ID aplikací a klíč rozhraní API pro prostředek. Můžete je znovu načíst z hello portál Azure, jak je znázorněno v následujícím diagramu hello:

![ID aplikace v hello portálu Azure](./media/automate-with-logic-apps/appid.png) 

Zadejte název vašeho připojení, ID aplikace hello a klíč rozhraní API hello.

![Okno připojení toku návrhář aplikace na základě logiky](./media/automate-with-logic-apps/flow3.png)

### <a name="step-5-specify-hello-analytics-query-and-chart-type"></a>Krok 5: Zadejte hello Analytics dotazu a typ grafu
V následujícím příkladu hello hello dotaz vybere hello se nezdařilo požadavky v rámci hello poslední den a jejich koreluje s výjimkami, k nimž došlo v rámci operace hello. Analýza korelaci požadavky hello se nezdařilo, založené na identifikátor operation_Id hello. dotaz Hello pak segmenty hello výsledky pomocí algoritmu autocluster hello. 

Když vytvoříte vlastní dotazy, ověřte, že fungují správně v Analytics předtím, než ho přidáte tooyour toku.

1. V hello **dotazu** pole, přidejte následující dotaz Analytics hello: 

    ```
    requests
    | where timestamp > ago(1d)
    | where success == "False"
    | project name, operation_Id
    | join ( exceptions
        | project problemId, outerMessage, operation_Id
    ) on operation_Id
    | evaluate autocluster()
    ```

2. V hello **typ grafu** vyberte **tabulky Html**.

    ![Okno Konfigurace analýzy dotazu](./media/automate-with-logic-apps/flow4.png)

### <a name="step-6-configure-hello-logic-app-toosend-email"></a>Krok 6: Konfigurace hello logiku aplikace toosend e-mailu

1. Klikněte na tlačítko **nový krok**a potom vyberte **přidat akci**.

2. Hello vyhledávacího pole zadejte **Office 365 Outlook**.

3. Klikněte na tlačítko **Office 365 Outlook – e-mailu**.

    ![Výběr aplikace Outlook Office 365](./media/automate-with-logic-apps/flow2b.png)

4. V hello **e-mailovou zprávu** okně hello následující:

   a. Zadejte e-mailová adresa příjemce hello hello.

   b. Zadejte předmět e-mailu hello.

   c. Klikněte kamkoli do hello **textu** pole a zvolte v nabídce hello dynamického obsahu které se otevře ve správné hello **textu**.

   d. Klikněte na tlačítko **zobrazit rozšířené možnosti**.

      ![Konfigurace aplikace Outlook Office 365](./media/automate-with-logic-apps/flow5.png)

5. V nabídce dynamického obsahu hello hello následující:

    a. Vyberte **název přílohy**.

    b. Vyberte **obsah přílohy**.
    
    c. V hello **je HTML** vyberte **Ano**.

      ![Konfigurační obrazovce pro Office 365 e-mailu](./media/automate-with-logic-apps/flow7.png)

### <a name="step-7-save-and-test-your-logic-app"></a>Krok 7: Uložit a testování aplikace logiky
* Klikněte na tlačítko **Uložit** toosave změny.

Počkejte, až aplikace logiky hello aktivační událost toorun hello, nebo můžete spustit aplikace logiky hello okamžitě výběrem **spustit**.

![Obrazovky vytvoření aplikace logiky](./media/automate-with-logic-apps/step7.png)

Při spuštění aplikace logiky, hello příjemce, které jste zadali v seznamu e-mailu hello obdrží e-mailu, který může vypadat hello následující:

![Logiku aplikace e-mailové zprávy](./media/automate-with-logic-apps/flow9.png)

## <a name="next-steps"></a>Další kroky

- Další informace o vytváření [analytické dotazy](app-insights-analytics-using.md).
- Další informace o [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps).



<!--Link references-->





