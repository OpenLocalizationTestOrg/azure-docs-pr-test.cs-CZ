---
title: "Vytvoření první funkce na webu Azure Portal | Dokumentace Microsoftu"
description: "Naučíte se postup vytvoření první funkce Azure Function pro provádění pomocí webu Azure Portal bez serveru."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.assetid: 96cf87b9-8db6-41a8-863a-abb828e3d06d
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/17/2017
ms.author: glenga
ms.custom: mvc, devcenter
ms.openlocfilehash: 754ca6e5297c3be9166efa7a40a5ba3714911c99
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/12/2018
---
# <a name="create-your-first-function-in-the-azure-portal"></a>Vytvoření první funkce na webu Azure Portal

Služba Azure Functions umožňuje spuštění kódu v prostředí [bez serveru](https://azure.microsoft.com/overview/serverless-computing/), aniž byste nejdřív museli vytvořit virtuální počítač nebo publikovat webovou aplikaci. V tomto tématu se dozvíte, jak pomocí služby Functions vytvořit na webu Azure Portal funkci Hello World.

![Vytvoření aplikace Function App na webu Azure Portal](./media/functions-create-first-azure-function/function-app-in-portal-editor.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="sign-in-to-azure"></a>Přihlášení k Azure

Otevřete web Azure Portal. Uděláte to tak, že se přihlásíte k webu [Azure Portal](https://portal.azure.com/) pomocí svého účtu Azure.

## <a name="create-a-function-app"></a>Vytvoření Function App

K hostování provádění funkcí musíte mít aplikaci Function App. Aplikace Function App umožňuje seskupit funkce jako logickou jednotku pro snadnější správu, nasazování a sdílení prostředků. 

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

Dál vytvoříte v nové aplikaci Function App funkci.

## <a name="create-function"></a>Vytvoření funkce aktivované protokolem HTTP

1. Rozbalte novou aplikaci Function App a potom klikněte na tlačítko  **+**  vedle položky **Funkce**.

2.  Na stránce **Začněte rychle** vyberte **Webhook + API**, **Zvolte jazyk** funkce a klikněte na **Vytvořit tuto funkci**. 
   
    ![Stručný úvod do služby Functions na webu Azure Portal.](./media/functions-create-first-azure-function/function-app-quickstart-node-webhook.png)

Funkce ve vybraném jazyce se vytvoří pomocí šablony funkce aktivované protokolem HTTP. Toto téma ukazuje funkci skriptu jazyka C# na portálu, ale můžete vytvořit funkci v libovolném [podporovaném jazyku](supported-languages.md). 

Novou funkci můžete spustit odesláním požadavku HTTP.

## <a name="test-the-function"></a>Testování funkce

1. V nové funkci klikněte vpravo nahoře na **</> Získat adresu URL funkce**, vyberte **výchozí (klíč funkce)** a potom klikněte na **Kopírovat**. 

    ![Kopírování adresy URL funkce z webu Azure Portal](./media/functions-create-first-azure-function/function-app-develop-tab-testing.png)

2. Vložte adresu URL funkce do panelu Adresa vašeho prohlížeče. Na konec této adresy URL připojte hodnotu řetězce dotazu `&name=<yourname>` a stisknutím klávesy `Enter` na klávesnici požadavek proveďte. V prohlížeči by se měla zobrazit odpověď, kterou funkce vrátila.  

    Následuje příklad odpovědi v prohlížeči Edge (ostatní prohlížeče mohou zahrnovat zobrazený kód XML):

    ![Odezva funkce v prohlížeči.](./media/functions-create-first-azure-function/function-app-browser-testing.png)

    Adresa URL požadavku obsahuje klíč, který je ve výchozím nastavení nezbytný pro přístup k funkci přes protokol HTTP.   

3. Při spuštění funkce se do protokolů zaznamenávají informace o trasování. Pokud chcete zobrazit výstup trasování z předchozího zpracování, vraťte se k funkci na portálu a kliknutím na šipku ve spodní části obrazovky rozbalte položku **Protokoly**. 

   ![Prohlížeč protokolu funkcí na webu Azure Portal.](./media/functions-create-first-azure-function/function-view-logs.png)

## <a name="clean-up-resources"></a>Vyčištění prostředků

[!INCLUDE [Clean up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Další kroky

Vytvořili jste aplikaci Function App s jednoduchou funkcí aktivovanou protokolem HTTP.  

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Další informace najdete v tématu [Vazby protokolu HTTP služby Azure Functions a vazby webhooku](functions-bindings-http-webhook.md).



