---
title: "aaaAutomate Azure Application Insights zpracovává s Flow Microsoft"
description: "Zjistěte, jak lze pomocí Microsoft Flow tooquickly automatizovat opakované procesy pomocí konektoru hello Application Insights."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/25/2017
ms.author: bwren
ms.openlocfilehash: b34488a6b8b8b0a6add960a67f1426cbbbc13552
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automate-azure-application-insights-processes-with-hello-connector-for-microsoft-flow"></a>Automatizovat procesy Azure Application Insights s hello connector pro Microsoft Flow

Se přistihnete opakovaně spuštění hello stejné dotazů na váš toocheck data telemetrie, které vaše služba funguje správně? Jsou vypadající tooautomate můžete tyto dotazy pro hledání trendů a anomálií a následně vytvořit vlastní pracovní postupy je obcházet? Hello konektor služby Azure Application Insights (preview) pro Microsoft Flow je pro tyto účely hello nejvhodnější nástroj.

Díky této integraci teď procesy můžete automatizovat množství bez nutnosti napsat jediný řádek kódu. Po vytvoření tokem pomocí akce Application Insights, hello toku automaticky spustí dotaz Application Insights Analytics. 

Můžete přidat i další akce. Microsoft Flow zpřístupní stovky akce. Můžete například použít Microsoft Flow tooautomatically odesílání e-mailových oznámení nebo vytvoření chyby ve Visual Studio Team Services. Můžete také použít jeden z hello mnoho [šablony](https://ms.flow.microsoft.com/en-us/connectors/shared_applicationinsights/?slug=azure-application-insights) které jsou k dispozici pro hello konektor pro Flow společnosti Microsoft. Tyto šablony zrychlení hello proces vytváření k toku. 

<!--hello Application Insights connector also works with [Azure Power Apps](https://powerapps.microsoft.com/en-us/) and [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/?v=17.23h). --> 

## <a name="create-a-flow-for-application-insights"></a>Vytvoření postup pro službu Application Insights

V tomto kurzu se dozvíte, jak toocreate toku, který používá hello Analytics clusteru automaticky algoritmus toogroup atributy v hello data pro webovou aplikaci. tok Hello automaticky odesílá hello výsledky e-mailem, pouze příklad, jak můžete používat Microsoft Flow a analýza Statistika aplikace společně. 

### <a name="step-1-create-a-flow"></a>Krok 1: Vytvoření toku
1. Přihlaste se příliš[Microsoft Flow](http://flow.microsoft.com)a potom vyberte **Moje toků**.
2. Klikněte na tlačítko **vytvořit tokem z prázdné**.

### <a name="step-2-create-a-trigger-for-your-flow"></a>Krok 2: Vytvoření aktivační událost pro vaše tok
1. Vyberte **plán**a potom vyberte **plán - opakování**.
2. V hello **frekvence** vyberte **den**a v hello **Interval** zadejte **1**.

    ![Dialogové okno Microsoft Flow aktivační události](./media/app-insights-automate-with-flow/flow1.png)


### <a name="step-3-add-an-application-insights-action"></a>Krok 3: Přidání Application Insights akce
1. Klikněte na tlačítko **nový krok**a potom klikněte na **přidat akci**.
2. Vyhledejte **Azure Application Insights**.
3. Klikněte na tlačítko **Azure Application Insights – vizualizovat analýzy dotaz Preview**.

    ![Spusťte okno dotazu Analytics](./media/app-insights-automate-with-flow/flow2.png)

### <a name="step-4-connect-tooan-application-insights-resource"></a>Krok 4: Připojení tooan prostředek Application Insights

toocomplete tohoto kroku budete potřebovat ID aplikací a klíč rozhraní API pro prostředek. Můžete je znovu načíst z hello portál Azure, jak je znázorněno v následujícím diagramu hello:

![ID aplikace v hello portálu Azure](./media/app-insights-automate-with-flow/appid.png) 

- Zadejte název připojení, spolu s hello aplikace ID a klíč API.

    ![Okno Microsoft Flow připojení](./media/app-insights-automate-with-flow/flow3.png)

### <a name="step-5-specify-hello-analytics-query-and-chart-type"></a>Krok 5: Zadejte hello Analytics dotazu a typ grafu
Tento příklad dotaz vybere hello se nezdařilo požadavky v rámci hello poslední den a je koreluje s výjimkami, k nimž došlo v rámci operace hello. Analýza korelaci je založena na hello operation_Id identifikátoru. dotaz Hello pak segmenty hello výsledky pomocí algoritmu autocluster hello. 

Když vytvoříte vlastní dotazy, ověřte, že fungují správně v Analytics předtím, než ho přidáte tooyour toku.

- Přidejte následující dotaz Analytics hello a pak vyberte typ grafu tabulky HTML hello. 

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
    
    ![Okno Konfigurace analýzy dotazu](./media/app-insights-automate-with-flow/flow4.png)

### <a name="step-6-configure-hello-flow-toosend-email"></a>Krok 6: Konfigurace hello toku toosend e-mailu

1. Klikněte na tlačítko **nový krok**a potom klikněte na **přidat akci**.
2. Vyhledejte **Office 365 Outlook**.
3. Klikněte na tlačítko **Office 365 Outlook – e-mailu**.

    ![Okno Výběr Outlook Office 365](./media/app-insights-automate-with-flow/flow2b.png)

4. V hello **e-mailovou zprávu** okně hello následující:

   a. Zadejte e-mailová adresa příjemce hello hello.

   b. Zadejte předmět e-mailu hello.

   c. Klikněte kamkoli do hello **textu** pole a zvolte v nabídce hello dynamického obsahu které se otevře ve správné hello **textu**.

   d. Klikněte na tlačítko **zobrazit rozšířené možnosti**.

    ![Konfigurace aplikace Outlook Office 365](./media/app-insights-automate-with-flow/flow5.png)

5. V nabídce dynamického obsahu hello hello následující:

    a. Vyberte **název přílohy**.

    b. Vyberte **obsah přílohy**.
    
    c. V hello **je HTML** vyberte **Ano**.

    ![Okno Konfigurace e-mailu Office 365](./media/app-insights-automate-with-flow/flow7.png)

### <a name="step-7-save-and-test-your-flow"></a>Krok 7: Uložit a testování vaší toku
- V hello **toku název** pole, přidejte název vaší toku a pak klikněte na tlačítko **vytvořit toku**.

    ![Postup vytvoření okna](./media/app-insights-automate-with-flow/flow8.png)

Počkejte, až hello aktivační událost toorun tuto akci, nebo můžete spustit hello toku okamžitě nástrojem [spuštěná aktivační událost hello na vyžádání](https://flow.microsoft.com/blog/run-now-and-six-more-services/).

Po spuštění hello toku hello příjemce, které jste zadali v seznamu e-mailu hello přijímat e-mailovou zprávu, která vypadá jako hello následující:

![Ukázkového e-mailu](./media/app-insights-automate-with-flow/flow9.png)


## <a name="next-steps"></a>Další kroky

- Další informace o vytváření [analytické dotazy](app-insights-analytics-using.md).
- Další informace o [Microsoft Flow](https://ms.flow.microsoft.com).



<!--Link references-->





