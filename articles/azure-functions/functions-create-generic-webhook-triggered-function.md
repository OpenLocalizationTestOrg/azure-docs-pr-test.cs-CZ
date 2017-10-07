---
title: "aaaCreate funkce v Azure aktivovány obecný webhook | Microsoft Docs"
description: "Pomocí Azure Functions toocreate bez serveru funkci, která je vyvolat webhook v Azure."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.assetid: fafc10c0-84da-4404-b4fa-eea03c7bf2b1
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/12/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 0a4868da91d216c8d20930ce7ec82eaa059c75ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-triggered-by-a-generic-webhook"></a>Vytvoření funkce aktivovány obecný webhook

Azure Functions umožňuje spuštění kódu v prostředí bez serveru bez nutnosti toofirst vytvoření virtuálního počítače nebo publikování webové aplikace. Můžete například nakonfigurovat funkce toobe, aktivuje výstrahu aktivováno monitorování Azure. Toto téma ukazuje, jak přidat tooexecute kód C# Pokud skupina prostředků je tooyour předplatné.   

![Obecný webhook aktivuje funkce v hello portálu Azure](./media/functions-create-generic-webhook-triggered-function/function-completed.png)

## <a name="prerequisites"></a>Požadavky 

toocomplete v tomto kurzu:

+ Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Vytvoření aplikace Azure Function App

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

Dál vytvořte funkci v nové funkce aplikace hello.

## <a name="create-function"></a>Vytvoření obecný webhook aktivované funkce

1. Rozšířit funkce aplikace a klikněte na tlačítko hello  **+**  tlačítko vedle příliš**funkce**. Pokud je tato funkce je hello první z nich v aplikaci funkce, vyberte **vlastní funkce**. Zobrazí se hello kompletní sada šablon funkcí.

    ![Funkce Rychlé spuštění stránku hello portálu Azure](./media/functions-create-generic-webhook-triggered-function/add-first-function.png)

2. Vyberte hello **obecný WebHook - C#** šablony. Zadejte název pro funkce C# a pak vyberte **vytvořit**.

     ![Vytvoří funkci, obecný webhook aktivuje v hello portálu Azure](./media/functions-create-generic-webhook-triggered-function/functions-create-generic-webhook-trigger.png) 

2. V nové funkce, klikněte na **URL funkce <> / Get**, zkopírujte a uložte hello hodnotu. Můžete použít tuto hodnotu tooconfigure hello webhooku. 

    ![Zkontrolujte kód funkce hello](./media/functions-create-generic-webhook-triggered-function/functions-copy-function-url.png)
         
Dále vytvoříte koncový bod webhooku ve výstraze protokolu aktivit v Azure monitorování. 

## <a name="create-an-activity-log-alert"></a>Vytvořit výstrahu protokolu aktivit

1. V hello portálu Azure, přejděte toohello **monitorování** služby, vyberte **výstrahy**a klikněte na tlačítko **přidat aktivitu protokolu upozornění**.   

    ![Monitorování](./media/functions-create-generic-webhook-triggered-function/functions-monitor-add-alert.png)

2. Použijte hello nastavení uvedeného v tabulce hello:

    ![Vytvořit výstrahu protokolu aktivit](./media/functions-create-generic-webhook-triggered-function/functions-monitor-add-alert-settings.png)

    | Nastavení      |  Navrhovaná hodnota   | Popis                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Název výstrahy protokolu aktivit** | prostředek skupiny vytvořit – upozornění | Název výstrahy protokolu hello aktivity. |
    | **Předplatné** | Vaše předplatné | Hello odběr, který používáte pro účely tohoto kurzu. | 
    |  **Skupina prostředků** | myResourceGroup | skupiny prostředků Hello hello výstrahy prostředky jsou nasazeny do. Pomocí hello stejné skupině prostředků jako funkce aplikace umožňuje snazší tooclean po dokončení kurzu hello. |
    | **Kategorie události** | Pro správu | Tato kategorie zahrnuje změny tooAzure prostředky.  |
    | **Typ prostředku** | Skupiny prostředků | Filtrování výstrah tooresource skupiny aktivit. |
    | **Skupina prostředků**<br/>a **prostředků** | Všechny | Monitorujte všechny prostředky. |
    | **Název operace** | Vytvoření skupiny prostředků | Filtrování výstrah toocreate operace. |
    | **Úroveň** | Informační | Zahrnout informační úrovně výstrahy. | 
    | **Stav** | Úspěch | Filtry tooactions výstrahy, které byly úspěšně dokončeny. |
    | **Akce skupiny** | Nový | Vytvořte novou skupinu akce, který definuje hello akce trvá, když je vydána výstraha. |
    | **Název skupiny akce** | Funkce webhooku | Skupina akce název tooidentify hello.  | 
    | **Krátký název** | funcwebhook | Krátký název pro skupinu akce hello. |  

3. V **akce**, přidat akci pomocí nastavení hello uvedených v tabulce hello: 

    ![Přidat skupinu akce](./media/functions-create-generic-webhook-triggered-function/functions-monitor-add-alert-settings-2.png)

    | Nastavení      |  Navrhovaná hodnota   | Popis                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Název** | CallFunctionWebhook | Název akce hello. |
    | **Typ akce** | Webhook | Hello odpovědi toohello upozornění je, že adresa URL Webhooku se nazývá. |
    | **Podrobnosti** | URL – funkce | Vložte adresy URL webhooku hello hello funkce, který jste zkopírovali dříve. |v

4. Klikněte na tlačítko **OK** toocreate hello výstrahy a akce skupiny.  

Hello webhooku se nyní nazývá při vytvoření skupiny prostředků v rámci vašeho předplatného. Potom aktualizujte hello kód ve vaší funkce toohandle hello dat protokolu JSON v textu hello hello požadavku.   

## <a name="update-hello-function-code"></a>Aktualizovat kód funkce hello

1. Přejděte zpět tooyour funkce aplikace hello portálu a rozšířit funkce. 

2. Nahraďte kód skriptu hello C# ve funkci hello portálu hello hello následující kód:

    ```csharp
    #r "Newtonsoft.Json"
    
    using System;
    using System.Net;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    
    public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
    {
        log.Info($"Webhook was triggered!");
    
        // Get hello activityLog object from hello JSON in hello message body.
        string jsonContent = await req.Content.ReadAsStringAsync();
        JToken activityLog = JObject.Parse(jsonContent.ToString())
            .SelectToken("data.context.activityLog");
    
        // Return an error if hello resource in hello activity log isn't a resource group. 
        if (activityLog == null || !string.Equals((string)activityLog["resourceType"], 
            "Microsoft.Resources/subscriptions/resourcegroups"))
        {
            log.Error("An error occured");
            return req.CreateResponse(HttpStatusCode.BadRequest, new
            {
                error = "Unexpected message payload or wrong alert received."
            });
        }
    
        // Write information about hello created resource group toohello streaming log.
        log.Info(string.Format("Resource group '{0}' was {1} on {2}.",
            (string)activityLog["resourceGroupName"],
            ((string)activityLog["subStatus"]).ToLower(), 
            (DateTime)activityLog["submissionTimestamp"]));
    
        return req.CreateResponse(HttpStatusCode.OK);    
    }
    ```

Nyní můžete otestovat hello funkce tak, že vytvoříte novou skupinu prostředků ve vašem předplatném.

## <a name="test-hello-function"></a>Testování funkce hello

1. Klikněte na ikonu skupiny prostředků hello v hello nalevo od hello portál Azure, vyberte **+ přidat**, zadejte **název skupiny prostředků**a vyberte **vytvořit** toocreate prázdné skupině prostředků.
    
    ![Vytvořte skupinu prostředků.](./media/functions-create-generic-webhook-triggered-function/functions-create-resource-group.png)

2. Přejděte zpět tooyour funkce a rozbalte hello **protokoly** okno. Po vytvoření skupiny prostředků hello hello výstrahy aktivační události protokolu aktivit hello webhooku a provede hello funkce. Zobrazí název nové skupiny prostředků hello zapisují protokoly toohello hello.  

    ![Přidáte nastavení aplikace testu.](./media/functions-create-generic-webhook-triggered-function/function-view-logs.png)

3. (Volitelné) Přejděte zpět a odstraňte skupinu prostředků hello, kterou jste vytvořili. Všimněte si, že tuto aktivitu neaktivuje hello funkce. Důvodem je, že odstranění operations filtrují hello výstraha. 

## <a name="clean-up-resources"></a>Vyčištění prostředků

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Další kroky

Funkce, která se spouští při přijetí požadavku z obecný webhook jste vytvořili. 

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Další informace o aktivačních událostech webhooků najdete v tématu [Vazby protokolu HTTP služby Azure Functions a vazby webhooku](functions-bindings-http-webhook.md). toolearn Další informace o vývoji funkce v jazyce C#, najdete v části [Azure funkcí jazyka C# skript referenční informace pro vývojáře](functions-reference-csharp.md).

