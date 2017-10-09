---
title: "aaaCreate, vytvářet a nasazovat aplikace logiky v sadě Visual Studio – Azure Logic Apps | Microsoft Docs"
description: "Vytváření projektů sady Visual Studio, můžete navrhnout, vytvořit a nasadit Azure Logic Apps."
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: e484e5ce-77e9-4fa9-bcbe-f851b4eb42a6
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 2/14/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 5154cb05f9a48e9f0f2381a6953947217f7bb114
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="design-build-and-deploy-azure-logic-apps-in-visual-studio"></a>Návrh, vytvoření a nasazení Azure Logic Apps v sadě Visual Studio

I když hello [portál Azure](https://portal.azure.com/) nabízí skvělý způsob pro vás toocreate a spravovat Azure Logic Apps, Visual Studio můžete použít pro navrhování, sestavování a nasazení aplikace logiky. Visual Studio poskytuje bohaté nástroje, například hello logiku aplikace Návrhář vám aplikace logiky toocreate, nakonfigurujte šablony nasazení a automatizace a nasaďte tooany prostředí. 

Další tooget začít s Azure Logic Apps, [jak toocreate svou první aplikaci logiky v hello portál Azure](logic-apps-create-a-logic-app.md).

## <a name="installation-steps"></a>Postup instalace

tooinstall a konfigurace nástrojů Visual Studio pro Azure Logic Apps, postupujte podle těchto kroků.

### <a name="prerequisites"></a>Požadavky

* [Visual Studio 2017](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx) nebo Visual Studio 2015
* [Nejnovější sadu Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 nebo vyšší)
* [Azure PowerShell](https://github.com/Azure/azure-powershell#installation)
* Přístup k toohello webové při použití vložených Návrhář hello

### <a name="install-visual-studio-tools-for-azure-logic-apps"></a>Instalace nástrojů Visual Studio pro Azure Logic Apps

Po instalaci hello požadavky:

1. Otevřete sadu Visual Studio. Na hello **nástroje** nabídce vyberte možnost **rozšíření a aktualizace**.
2. Rozbalte hello **Online** kategorie, můžete vyhledat online.
3. Procházet nebo Hledat **Logic Apps** vyhledejte **nástroje aplikace logiky Azure pro sadu Visual Studio**.
4. toodownload a příponu hello instalace, klikněte na tlačítko **Stáhnout**.
5. Po instalaci, restartujte Visual Studio.

> [!NOTE]
> Můžete také stáhnout [nástroje aplikace logiky Azure pro Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551) a hello [nástroje aplikace logiky Azure pro sadu Visual Studio 2015](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio) přímo z hello Visual Studio Marketplace.

Po dokončení instalace, můžete vytvořit projekt skupiny prostředků Azure hello pomocí návrháře aplikace logiky.

## <a name="create-your-project"></a>Vytvoření projektu

1. Na hello **soubor** nabídky, přejděte příliš**nový**a vyberte **projektu**. Nebo tooadd vašeho projektu tooan existující řešení, přejděte příliš**přidat**a vyberte **nový projekt**.

    ![Nabídka Soubor](./media/logic-apps-deploy-from-vs/filemenu.png)

2. V hello **nový projekt** okně Najít **cloudu**a vyberte **skupiny prostředků Azure**. Název projektu a klikněte na tlačítko **OK**.

    ![Přidat nový projekt](./media/logic-apps-deploy-from-vs/addnewproject.png)

3. Vyberte hello **aplikace logiky** šablony, která pro vás toouse vytvoří šablonu nasazení prázdné logiku aplikace. Vyberte šablonu a klikněte na tlačítko **OK**.

    ![Vyberte šablonu aplikace logiky](./media/logic-apps-deploy-from-vs/selectazuretemplate1.png)

    Přidali jste nyní řešení tooyour projektu aplikace logiky. 
    V Průzkumníku řešení hello by se zobrazit váš soubor nasazení.

    ![Soubor nasazení](./media/logic-apps-deploy-from-vs/deployment.png)

## <a name="create-your-logic-app-with-logic-app-designer"></a>Vytvoření aplikace logiky pomocí návrháře aplikace logiky

Pokud máte projekt skupiny prostředků Azure, který obsahuje aplikace logiky, můžete otevřít hello návrhář aplikace na základě logiky v sadě Visual Studio toocreate pracovního postupu. 

> [!NOTE]
> Návrhář Hello vyžaduje připojení k Internetu příliš dotaz konektory pro dostupné vlastnosti a data. Například pokud použijete konektor hello Dynamics CRM Online, Návrhář hello dotazy k dispozici vlastní tooshow CRM instance a výchozí vlastnosti.

1. Klikněte pravým tlačítkem na vaše `<template>.json` soubor a vyberte **otevřete pomocí návrháře aplikace logiky**. (`Ctrl+L`)

2. Zvolte si předplatné, skupinu prostředků a umístění pro šablonu nasazení.

    > [!NOTE]
    > Návrh aplikace logiky vytvoří připojení k rozhraní API prostředky tento dotaz na vlastnosti při návrhu. Visual Studio použije vaše toocreate vybraného prostředku skupiny tato připojení v době návrhu. tooview nebo změnit všech připojení k rozhraní API, přejděte toohello portál Azure a vyhledejte **rozhraní API připojení**.

    ![Výběr předplatného.](./media/logic-apps-deploy-from-vs/designer_picker.png)

    Návrhář Hello používá hello definici v hello `<template>.json` soubor pro vykreslování.

4. Vytváření a návrhu aplikace logiky. Šablona nasazení aktualizována s vašimi změnami.

    ![Návrhář aplikace logiky v sadě Visual Studio](./media/logic-apps-deploy-from-vs/designer_in_vs.png)

Visual Studio. přidá `Microsoft.Web/connections` prostředky příliš váš zdrojový soubor pro všechna připojení aplikace logiky musí toofunction. Tyto vlastnosti připojení lze nastavit při nasazení a spravovat poté, co nasadíte v **rozhraní API připojení** v hello portálu Azure.

### <a name="switch-toojson-code-view"></a>Zobrazení kódu tooJSON přepínače

tooshow hello reprezentace JSON pro svou aplikaci logiky, vyberte hello **zobrazení kódu** karta v dolní části hello hello návrháře.

tooswitch zpět toohello úplné prostředků JSON, klikněte pravým tlačítkem na hello `<template>.json` soubor a vyberte **otevřete**.

### <a name="add-references-for-dependent-resources-toovisual-studio-deployment-templates"></a>Přidání odkazů pro závislé prostředky tooVisual Studio nasazení šablony

Pokud chcete prostředky závislé tooreference aplikace logiky, můžete použít [funkce šablon Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-functions) v šabloně nasazení aplikace logiky. Například můžete vaše logiku aplikace tooreference účet funkce Azure nebo integrace, které chcete toodeploy společně se svou aplikaci logiky. Postupujte podle těchto pokynů o tom, jak toouse parametry v šabloně nasazení, který hello návrhář aplikace na základě logiky vykreslí správně. 

Používáte logiku aplikace parametry v těchto druhů triggery a akce:

*   Podřízené pracovní postup
*   Funkce aplikace
*   APIM volání
*   Adresa URL rozhraní API připojení modulu runtime
*   Cesta připojení rozhraní API

A můžete použít šablonu funkce jako je například parametry, proměnné, resourceId, concat atd. Můžete zde je ukázka, jak můžete nahradit ID prostředku hello funkce Azure:

```
"parameters":{
    "functionName": {
        "type":"string",
        "minLength":1,
        "defaultValue":"<FunctionName>"
    }
},
```

A kde byste použili parametry:

```
"MyFunction": {
    "type": "Function",
    "inputs": {
        "body":{},
        "function":{
            "id":"[resourceid('Microsoft.Web/sites/functions','functionApp',parameters('functionName'))]"
        }
    },
    "runAfter":{}
}
```
Další příklad můžete parametrizovat hello Service Bus odeslat zprávu operace:

```
"Send_message": {
    "type": "ApiConnection",
        "inputs": {
            "host": {
                "connection": {
                    "name": "@parameters('$connections')['servicebus']['connectionId']"
                }
            },
            "method": "post",
            "path": "[concat('/@{encodeURIComponent(''', parameters('queueuname'), ''')}/messages')]",
            "body": {
                "ContentData": "@{base64(triggerBody())}"
            },
            "queries": {
                "systemProperties": "None"
            }
        },
        "runAfter": {}
    }
```
> [!NOTE] 
> host.runtimeUrl je volitelné a lze odebrat ze šablony, pokud je k dispozici.
> 


> [!NOTE] 
> Pro hello návrhář aplikace na základě logiky toowork při použití parametrů, je nutné zadat výchozí hodnoty, například:
> 
> ```
> "parameters": {
>     "IntegrationAccount": {
>     "type":"string",
>     "minLength":1,
>     "defaultValue":"/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.Logic/integrationAccounts/<integrationAccountName>"
>     }
> },
> ```

### <a name="save-your-logic-app"></a>Uložení aplikace logiky

toosave svou aplikaci logiky na kdykoli, přejděte příliš**soubor** > **Uložit**. (`Ctrl+S`) 

Pokud svou aplikaci logiky všechny chyby při ukládání aplikace, se objeví v sadě Visual Studio hello **výstupy** okno.

## <a name="deploy-your-logic-app-from-visual-studio"></a>Nasazení aplikace logiky ze sady Visual Studio

Po dokončení konfigurace aplikace, můžete nasadit přímo ze sady Visual Studio v několika krocích. 

1. V Průzkumníku řešení klikněte pravým tlačítkem na projekt a přejděte příliš**nasadit** > **nové nasazení...**

    ![Nové nasazení](./media/logic-apps-deploy-from-vs/newdeployment.png)

2. Když se zobrazí výzva, přihlaste se tooyour předplatného Azure. 

3. Nyní je třeba vybrat hello podrobnosti pro skupinu prostředků hello místo toodeploy svou aplikaci logiky. Až budete hotoví, vyberte **nasadit**.

    > [!NOTE]
    > Ujistěte se, že jste vybrali hello správnou šablonu a soubor parametrů pro skupinu prostředků hello. Například pokud chcete toodeploy tooa provozním prostředí, vyberte soubor parametrů produkční hello.

    ![Nasazení tooresource skupiny](./media/logic-apps-deploy-from-vs/deploytoresourcegroup.png)

    Stav nasazení Hello se zobrazí v hello **výstup** okno. 
    Můžete mít tooselect **Azure zřizování** v hello **zobrazit výstup z** seznamu.

    ![Výstup stavu nasazení](./media/logic-apps-deploy-from-vs/output.png)

V budoucích hello můžete upravit aplikaci logiky ve správě zdrojového kódu a použít nové verze sady Visual Studio toodeploy.

> [!NOTE]
> Pokud změníte hello definice v hello portál Azure přímo, tyto změny se přepíší při nasazení ze sady Visual Studio příště. 

## <a name="add-your-logic-app-tooan-existing-resource-group-project"></a>Přidejte logiku aplikace tooan existující skupinu prostředků projektu

Pokud máte existující projekt skupiny prostředků, můžete přidat projektu logiku aplikace toothat v okně osnovy JSON hello. Můžete také přidat další aplikace logiky spolu s hello aplikace, které jste vytvořili.

1. Otevřete hello `<template>.json` souboru.

2. tooopen hello okno osnovy JSON, přejděte příliš**zobrazení** > **ostatní okna** > **osnovy JSON**.

3. Klikněte na tlačítko tooadd soubor prostředků toohello šablony **přidat prostředek** hello horní části okna osnovy JSON hello. Nebo v okně hello osnovou JSON, klikněte pravým tlačítkem na **prostředky**a vyberte **přidat nový prostředek**.

    ![Osnova JSON – okno](./media/logic-apps-deploy-from-vs/jsonoutline.png)
    
4. V hello **přidat prostředek** dialogové okno, vyhledejte a vyberte **aplikace logiky**. Název aplikace logiky a vyberte **přidat**.

    ![Přidání prostředku](./media/logic-apps-deploy-from-vs/addresource.png)

## <a name="next-steps"></a>Další kroky

* [Správa aplikací logiky v Průzkumníku cloudu Visual Studio](logic-apps-manage-from-vs.md)
* [Zobrazení běžných příkladů a scénářů](logic-apps-examples-and-scenarios.md).
* [Zjistěte, jak tooautomate obchodní procesy službou Azure Logic Apps](http://channel9.msdn.com/Events/Build/2016/T694)
* [Zjistěte, jak toointegrate vaše systémy službou Azure Logic Apps](http://channel9.msdn.com/Events/Build/2016/P462)
