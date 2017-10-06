---
title: "aaaViews v řešení pro správu Operations Management Suite (OMS) | Microsoft Docs"
description: "Řešení pro správu v Operations Management Suite (OMS) by měl obvykle zahrnovat jeden nebo více toovisualize dat zobrazení.  Tento článek popisuje, jak tooexport zobrazení vytvořené hello Návrhář zobrazení a její zahrnutí do řešení pro správu. "
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 570b278c-2d47-4e5a-9828-7f01f31ddf8c
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/11/2017
ms.author: bwren
ms.openlocfilehash: 303861465014a27289f831332b3d95925c0ae66d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="views-in-operations-management-suite-oms-management-solutions-preview"></a>Zobrazení v řešení pro správu Operations Management Suite (OMS) (Preview)
> [!NOTE]
> Toto je předběžná dokumentace pro vytváření řešení pro správu v OMS, které jsou aktuálně ve verzi preview. Žádné schéma níže popsané je toochange subjektu.    
>
>

[Řešení pro správu v Operations Management Suite (OMS)](operations-management-suite-solutions.md) by měl obvykle zahrnovat jeden nebo více toovisualize dat zobrazení.  Tento článek popisuje, jak tooexport zobrazení vytvořené hello [Návrhář zobrazení](../log-analytics/log-analytics-view-designer.md) a její zahrnutí do řešení pro správu.  

> [!NOTE]
> Použijte parametry a proměnné, které jsou buď požadované, nebo běžné toomanagement řešení a popsané v zprostředkovatele Hello ukázky v tomto článku [vytváření řešení pro správu v Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md)
>
>

## <a name="prerequisites"></a>Požadavky
Tento článek předpokládá, že jste již obeznámeni s postupy příliš[vytvoření řešení správy](operations-management-suite-solutions-creating.md) a struktuře hello soubor řešení.

## <a name="overview"></a>Přehled
tooinclude zobrazení v rámci řešení pro správu, vytvoříte **prostředků** pro něj v hello [soubor řešení](operations-management-suite-solutions-creating.md).  Hello formátu JSON, který popisuje konfiguraci podrobné zobrazení hello je obvykle komplexní ale a není něco že autor typické řešení by bylo možné toocreate ručně.  Hello nejběžnější metodou je toocreate hello zobrazení pomocí hello [Návrhář zobrazení](../log-analytics/log-analytics-view-designer.md), ho exportovat a poté přidejte jeho řešení toohello podrobnou konfiguraci.

základní kroky tooadd Hello řešení tooa zobrazení jsou následujícím způsobem.  Každý krok je podrobně popsán dále v následujících částech hello.

1. Exportujte soubor tooa zobrazení hello.
2. Vytvořte prostředek hello zobrazení v řešení hello.
3. Přidejte hello zobrazit podrobnosti.

## <a name="export-hello-view-tooa-file"></a>Exportovat soubor tooa zobrazení hello
Postupujte podle pokynů hello [Návrhář zobrazení Log Analytics](../log-analytics/log-analytics-view-designer.md) tooexport soubor tooa zobrazení.  Hello exportovaný soubor bude mít ve formátu JSON s hello stejné [elementy jako soubor řešení hello](operations-management-suite-solutions-solution-file.md).  

Hello **prostředky** prvek hello zobrazení souboru bude mít prostředek s typem **Microsoft.OperationalInsights/workspaces** , představuje hello pracovním prostorem OMS.  Tento prvek bude mít dílčí prvek s typem **zobrazení** , představuje hello zobrazení a obsahuje podrobné konfiguraci.  Bude kopírovat hello podrobnosti tohoto elementu a zkopírujte jej do řešení.

## <a name="create-hello-view-resource-in-hello-solution"></a>Vytvořit prostředek hello zobrazení v řešení hello
Přidejte následující zobrazení prostředků toohello hello **prostředky** element souboru řešení.  Tato služba využívá proměnné, které jsou popsané níže, musíte taky přidat.  Všimněte si, že hello **řídicí panel** a **OverviewTile** vlastnosti jsou zástupné symboly, které bude přepsat hello odpovídající vlastnosti z hello exportovaný soubor zobrazení.

    {
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
        "type": "Microsoft.OperationalInsights/workspaces/views",
        "location": "[parameters('workspaceregionId')]",
        "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
        "dependson": [
            ],
        "properties": {
            "Id": "[variables('ViewName')]",
            "Name": "[variables('ViewName')]",
            "DisplayName": "[variables('ViewName')]",
            "Description": "",
            "Author": "[variables('ViewAuthor')]",
            "Source": "Local",
            "Dashboard": ,
            "OverviewTile":
        }
    }

Přidejte následující proměnné toohello proměnné element soubor řešení hello hello a nahraďte hello toothose hodnoty pro vaše řešení.

    "LogAnalyticsApiVersion": "2015-11-01-preview",
    "ViewAuthor": "Your name."
    "ViewDescription": "Optional description of hello view."
    "ViewName": "Provide a name for hello view here."


Všimněte si, že může kopírovat hello celého zobrazení prostředek z exportovaného zobrazení souboru, ale potřebovali byste toomake hello následující změny pro něj toowork ve vašem řešení.  

* Hello **typ** pro zobrazení hello prostředků musí toobe se změnil z **zobrazení** příliš**Microsoft.OperationalInsights/workspaces**.
* Hello **název** vlastnost pro prostředek zobrazení hello musí název pracovního prostoru hello tooinclude toobe změnit.
* Hello závislost na hello prostoru musí toobe odebrat, protože hello prostoru prostředek není definován v řešení hello.
* **DisplayName** toobe vlastnost potřeby přidat toohello zobrazení.  Hello **Id**, **název**, a **DisplayName** musí všechny shodovat.
* Názvy parametrů musí být změněno toomatch hello vyžaduje sadu parametrů.
* Proměnné by měl definovaný v hello řešení a použít v příslušných vlastností hello.

## <a name="add-hello-view-details"></a>Přidat hello zobrazit podrobnosti
zobrazení prostředků Hello v hello exportovat soubor bude obsahovat dva elementy v hello zobrazení **vlastnosti** element s názvem **řídicí panel** a **OverviewTile** které obsahují hello podrobnou konfiguraci hello zobrazení.  Zkopírujte tyto dva prvky a jejich obsah do hello **vlastnosti** element hello zobrazení prostředků v souboru řešení.

## <a name="example"></a>Příklad
Například následující ukázkový text hello ukazuje soubor simple řešení se zobrazením.  Výpustky (...) se zobrazují pro hello **řídicí panel** a **OverviewTile** obsah z důvodů místa.

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspaceName": {
                "type": "string"
            },
            "accountName": {
                "type": "string"
            },
            "workspaceRegionId": {
                "type": "string"
            },
            "regionId": {
                "type": "string"
            },
            "pricingTier": {
                "type": "string"
            }
        },
        "variables": {
            "SolutionVersion": "1.1",
            "SolutionPublisher": "Contoso",
            "SolutionName": "Contoso Solution",
            "LogAnalyticsApiVersion": "2015-11-01-preview",
            "ViewAuthor":  "user@contoso.com",
            "ViewDescription":  "This is a sample view.",
            "ViewName":  "Contoso View"
        },
        "resources": [
            {
                "name": "[concat(variables('SolutionName'), '(' ,parameters('workspacename'), ')')]",
                "location": "[parameters('workspaceRegionId')]",
                "tags": { },
                "type": "Microsoft.OperationsManagement/solutions",
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspacename'), '/views/', variables('ViewName'))]"
                ],
                "properties": {
                    "workspaceResourceId": "[concat(resourceGroup().id, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspacename'))]",
                    "referencedResources": [
                    ],
                    "containedResources": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
                    ]
                },
                "plan": {
                    "name": "[concat(variables('SolutionName'), '(' ,parameters('workspaceName'), ')')]",
                    "Version": "[variables('SolutionVersion')]",
                    "product": "ContosoSolution",
                    "publisher": "[variables('SolutionPublisher')]",
                    "promotionCode": ""
                }
            },
            {
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
                "type": "Microsoft.OperationalInsights/workspaces/views",
                "location": "[parameters('workspaceregionId')]",
                "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
                "dependson": [
                ],
                "properties": {
                    "Id": "[variables('ViewName')]",
                    "Name": "[variables('ViewName')]",
                    "DisplayName": "[variables('ViewName')]",
                    "Description": "[variables('ViewDescription')]",
                    "Author": "[variables('ViewAuthor')]",
                    "Source": "Local",
                    "Dashboard": ...,
                    "OverviewTile": ...
                }
            }
          ]
    }




## <a name="next-steps"></a>Další kroky
* Další podrobné informace o vytváření [řešení pro správu](operations-management-suite-solutions-creating.md).
* Zahrnout [runbooků služeb automatizace ve vašem řešení správy](operations-management-suite-solutions-resources-automation.md).
