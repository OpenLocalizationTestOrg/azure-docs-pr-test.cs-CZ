---
title: "aaaCreate první funkce z hello portálu Azure | Microsoft Docs"
description: "Zjistěte, jak hello toocreate vaše první Azure fungovat pro provádění bez serveru pomocí portálu Azure."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 96cf87b9-8db6-41a8-863a-abb828e3d06d
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/07/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 84283d7d4bc6015061946af4589f9a70ae61f36b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-function-in-hello-azure-portal"></a>Vytvoření první funkce v hello portálu Azure

Azure Functions umožňuje spuštění kódu v prostředí bez serveru bez nutnosti toofirst vytvoření virtuálního počítače nebo publikování webové aplikace. V tomto tématu se dozvíte, jak toouse funkce toocreate funkci "hello, world" v hello portálu Azure.

![Vytvoření aplikace pro funkce v hello portálu Azure](./media/functions-create-first-azure-function/function-app-in-portal-editor.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-tooazure"></a>Přihlaste se tooAzure

Přihlaste se toohello [portál Azure](https://portal.azure.com/).

## <a name="create-a-function-app"></a>Vytvoření Function App

Musíte mít funkce aplikace toohost hello provádění funkcí. Aplikace Function App umožňuje seskupit funkce jako logickou jednotku pro snadnější správu, nasazování a sdílení prostředků. 

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

Dál vytvořte funkci v nové funkce aplikace hello.

## <a name="create-function"></a>Vytvoření funkce aktivované protokolem HTTP

1. Rozbalte nové funkce aplikace, a poté klikněte na tlačítko hello  **+**  tlačítko vedle příliš**funkce**.

2.  V hello **rychle začít** vyberte **WebHook + API**, **vybrat jazyk** pro svou funkci a klikněte na **vytvořit tuto funkci** . 
   
    ![Funkce Rychlé spuštění v hello portálu Azure.](./media/functions-create-first-azure-function/function-app-quickstart-node-webhook.png)

Funkce je vytvořená ve zvoleného jazyka pomocí hello šablony pro HTTP aktivované funkce. Můžete spustit novou funkci hello odesláním požadavku HTTP.

## <a name="test-hello-function"></a>Testování funkce hello

1. V nové funkci klikněte na **</> Získat adresu URL funkce**, vyberte **výchozí (klíč funkce)** a pak na **Kopírovat**. 

    ![Zkopírujte adresu URL funkce hello z hello portálu Azure](./media/functions-create-first-azure-function/function-app-develop-tab-testing.png)

2. Vložte adresu URL funkce hello do panelu Adresa prohlížeče. Připojit řetězec dotazu hello `&name=<yourname>` toothis adresy URL a stiskněte klávesu hello `Enter` klíče na vaši žádost hello tooexecute klávesnice. Hello tady je příklad hello odpovědi vrácené funkcí hello v prohlížeči Edge hello:

    ![Funkce odpověď v prohlížeči hello.](./media/functions-create-first-azure-function/function-app-browser-testing.png)

    požadavek Hello adresa URL obsahuje klíč, který je ve výchozím nastavení, tooaccess požadována funkce prostřednictvím protokolu HTTP.   

3. Když je funkce spuštěná, se zapisují informace o trasování toohello protokoly. toosee hello výstup trasování z předchozí zpracování hello, vrátí funkce tooyour hello portálu a klikněte na tlačítko hello v hello dolní části obrazovky tooexpand hello šipka nahoru **protokoly**. 

   ![Funkce přihlášení prohlížeč hello portálu Azure.](./media/functions-create-first-azure-function/function-view-logs.png)

## <a name="clean-up-resources"></a>Vyčištění prostředků

[!INCLUDE [Clean up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Další kroky

Vytvořili jste aplikaci Function App s jednoduchou funkcí aktivovanou protokolem HTTP.  

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Další informace najdete v tématu [Vazby protokolu HTTP služby Azure Functions a vazby webhooku](functions-bindings-http-webhook.md).



