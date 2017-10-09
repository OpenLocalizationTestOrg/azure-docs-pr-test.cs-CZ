---
title: "aplikace logiky aaaManage v sadě Visual Studio – Azure Logic Apps | Microsoft Docs"
description: "Správa aplikace logiky a dalších prostředků Azure pomocí Průzkumníka cloudové služby Visual Studio"
author: klam
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 12/19/2016
ms.author: LADocs; klam
ms.openlocfilehash: 419f83eb062b56e4ac2642dea4de1a025f747521
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-logic-apps-with-visual-studio-cloud-explorer"></a>Správa aplikace logiky s cloudu Průzkumníka Visual Studio

I když hello [portál Azure](https://portal.azure.com/) nabízí skvělý způsob pro vás toodesign a spravovat Azure Logic Apps, Průzkumník cloudu Visual Studio můžete použít pro správu mnoho prostředků Azure, včetně aplikace logiky. Průzkumník cloudu Visual Studio umožňuje procházet, spravovat, upravit, a stažení publikované aplikace logiky. Úlohy správy zahrnují povolit, zakázat a spustit zobrazení historie. 

Než budete moct přístup a Správa aplikace logiky v sadě Visual Studio, nainstalujte a nakonfigurujte tyto nástroje Visual Studio pro Azure Logic Apps. 

## <a name="prerequisites"></a>Požadavky

* [Visual Studio 2015 nebo Visual Studio 2017](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
* [Nejnovější sadu Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 nebo vyšší)
* [Průzkumník cloudu sady Visual Studio](https://marketplace.visualstudio.com/items?itemName=MicrosoftCloudExplorer.CloudExplorerforVisualStudio2015)
* Přístup k toohello webové při použití vložených Návrhář hello

## <a name="install-visual-studio-tools-for-logic-apps"></a>Instalace nástrojů Visual Studio pro Logic Apps

Po instalaci hello požadavky, stáhněte a nainstalujte nástroje aplikace logiky hello Azure pro sadu Visual Studio.

1. Otevřete sadu Visual Studio. Na hello **nástroje** nabídce vyberte možnost **rozšíření a aktualizace**.
2. Rozbalte hello **Online** kategorie, můžete hledat online v hello Galerie sady Visual Studio.
3. Procházet nebo Hledat **Logic Apps** vyhledejte **nástroje aplikace logiky Azure pro sadu Visual Studio**.
4. toodownload a příponu hello instalace, klikněte na tlačítko **Stáhnout**.
5. Po instalaci, restartujte Visual Studio.

> [!NOTE]
> hello toodownload nástroje aplikace logiky Azure pro sadu Visual Studio přejděte přímo, toohello [Visual Studio Marketplace](https://visualstudiogallery.msdn.microsoft.com/e25ad307-46cf-412e-8ba5-5b555d53d2d9).

## <a name="browse-for-logic-apps-in-cloud-explorer"></a>Procházením vyhledejte aplikace logiky v Průzkumníku cloudu

1.  tooopen Průzkumník cloudu na hello **zobrazení** nabídce zvolte **Průzkumník cloudu**.
2.  Procházením vyhledejte aplikaci logiky, skupinu prostředků nebo typ prostředku. 

    * Pokud podle typů prostředků, vyberte předplatné Azure, rozbalte položku hello **Logic Apps** a vyberte svou aplikaci logiky. 
    * Pokud ve skupině prostředků, rozbalte skupinu prostředků hello, který má svou aplikaci logiky a vyberte svou aplikaci logiky.

    příkazy tooview pro svou aplikaci logiky buď klikněte pravým tlačítkem na svou aplikaci logiky, nebo v hello dolní části Průzkumník cloudu, zvolte z hello **akce** nabídky.

    ![Procházením vyhledejte aplikaci logiky](./media/logic-apps-manage-from-vs/browse.png)

## <a name="edit-your-logic-app-with-logic-apps-designer"></a>Upravit aplikaci logiky pomocí návrháře aplikace logiky

V Průzkumníku cloudu, můžete otevřít aplikace logiky aktuálně nasazená v hello stejné designer, které budete používat v hello portálu Azure. 

* tooedit aplikace logiky v Průzkumníku cloudu, klikněte pravým tlačítkem na svou aplikaci logiky a vyberte **otevřete pomocí editoru aplikace logiky**. 

* toopublish cloudu vaší toohello aktualizace, zvolte **publikovat**. 

* Zvolte toostart spuštění nové **spustit aktivační událost**.

![Návrhář aplikace logiky](./media/logic-apps-manage-from-vs/designer.png)

Z Návrháře hello, můžete také **Stáhnout** aplikace logiky. Tato akce automaticky parameterizes definici aplikace logiky hello a uloží hello definice jako šablonu nasazení Azure Resource Manager. Můžete přidat tento projekt skupiny prostředků Azure tooyour šablonu nasazení.

## <a name="browse-your-logic-app-run-history"></a>Procházet aplikace logiky historie spouštění

Klikněte pravým tlačítkem na svou aplikaci logiky tooview hello historie pro svou aplikaci logiky, spouštění a vyberte **historie spouštění otevřete**. na základě vaší historie spouštění tooreorder na žádném z záhlaví sloupce hello uvedené, vyberte vlastnosti hello.

![Historie spouštění](media/logic-apps-manage-from-vs/runs.png)

hello tooshow historie pro instanci spouštění, můžete zkontrolovat hello spustit výsledky včetně hello vstupy a výstupy z každého kroku, dvakrát klikněte na spustit instance hello.

![Výsledky historie spouštění, vstupy a výstupy z kroků](./media/logic-apps-manage-from-vs/history.png)

## <a name="next-steps"></a>Další kroky

* [Vytvoření první aplikace logiky](logic-apps-create-a-logic-app.md)
* [Návrh, vytvoření a nasazení aplikace logiky v sadě Visual Studio](logic-apps-deploy-from-vs.md)
* [Zobrazení běžných příkladů a scénářů](logic-apps-examples-and-scenarios.md).
* [Video: Automatizovat firemní procesy službou Azure Logic Apps](http://channel9.msdn.com/Events/Build/2016/T694)
* [Video: Integrate systémy s Azure Logic Apps](http://channel9.msdn.com/Events/Build/2016/P462)
