---
title: "Vytvoření první funkce v Azure pomocí sady Visual Studio | Dokumentace Microsoftu"
description: "Vytvořte a publikujte do Azure jednoduchou funkci aktivovanou protokolem HTTP pomocí Azure Functions Tools for Visual Studio."
services: functions
documentationcenter: na
author: rachelappel
manager: erikre
editor: 
tags: 
keywords: azure functions, functions, event processing, compute, serverless architecture
ms.assetid: 82db1177-2295-4e39-bd42-763f6082e796
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 07/05/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 04370558725d76ffe83d8aaf5d16c88fd2803ba9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-your-first-function-using-visual-studio"></a>Vytvoření první funkce pomocí sady Visual Studio

Služba Azure Functions umožňuje spuštění kódu v prostředí bez serveru, aniž byste nejdřív museli vytvořit virtuální počítač nebo publikovat webovou aplikaci.

V tomto tématu se dozvíte, jak používat nástroje Visual Studio 2017 pro Azure Functions k vytvoření a otestování místně funkci "hello, world". Kód funkce potom budete publikovat do Azure. Tyto nástroje jsou dostupné jako součást sady funkcí Vývoj pro Azure v sadě Visual Studio 2017 verze 15.3 nebo novější.

![Kód služby Azure Functions v projektu sady Visual Studio](./media/functions-create-your-first-function-visual-studio/functions-vstools-intro.png)

## <a name="prerequisites"></a>Požadavky

Pro absolvování tohoto kurzu nainstalujte:

* [Visual Studio 2017 verze 15.3](https://www.visualstudio.com/vs/preview/), včetně sady funkcí **Vývoj pro Azure**.

    ![Instalace sady Visual Studio 2017 se sadou funkcí Vývoj pro Azure](./media/functions-create-your-first-function-visual-studio/functions-vs-workloads.png)
    
    >[!NOTE]  
    Po instalaci nebo upgradu na Visual Studio 2017 verze 15.3, budete také muset ručně aktualizovat nástroje Visual Studio 2017 pro Azure Functions. Můžete je aktualizovat z nástrojů **nástroje** nabídky v části **rozšíření a aktualizace...**   >  **Aktualizace** > **sady Visual Studio Marketplace** > **Azure Functions a webové úlohy nástroje**  >  **Aktualizace**. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 

## <a name="create-an-azure-functions-project-in-visual-studio"></a>Vytvoření projektu Azure Functions v sadě Visual Studio

[!INCLUDE [Create a project using the Azure Functions template](../../includes/functions-vstools-create.md)]

Teď, když jste vytvořili projekt, můžete vytvořit svou první funkci.

## <a name="create-the-function"></a>Vytvoření funkce

1. V **Průzkumníku řešení** klikněte pravým tlačítkem na uzel projektu a vyberte **Přidat** > **Nová položka**. Vyberte **Funkce Azure Functions** a klikněte na **Přidat**.

2. Vyberte **HttpTrigger**, zadejte **Název funkce**, jako **Přístupová práva** vyberte **Anonymní** a klikněte na **Vytvořit**. Vytvořená funkce je přístupná prostřednictvím požadavku HTTP z jakéhokoli klienta. 

    ![Vytvoření nové funkce Azure Functions](./media/functions-create-your-first-function-visual-studio/functions-vstools-add-new-function-2.png)

    Soubor kódu se přidá do projektu, který obsahuje třídu, která implementuje funkce kódu. Tento kód je založena na šablonu, která přijímá název hodnota a toto využití zpátky. **%{FunctionName/** atribut nastaví název funkce. **HttpTrigger** atribut určuje zprávu, která aktivuje funkce. 

    ![Funkce souboru kódu](./media/functions-create-your-first-function-visual-studio/functions-code-page.png)

Teď máte vytvořenou funkci aktivovanou protokolem HTTP a můžete ji otestovat na místním počítači.

## <a name="test-the-function-locally"></a>Místní testování funkce

Nástroje Azure Functions Core umožňují spouštět projekt Azure Functions na místním počítači pro vývoj. K instalaci těchto nástrojů budete vyzváni při prvním spuštění funkce ze sady Visual Studio.  

1. Pokud chcete funkci otestovat, stiskněte F5. Po výzvě přijměte požadavek ze sady Visual Studio na stažení a instalaci nástrojů Azure Functions Core (CLI).  Může být také potřeba povolit výjimku brány firewall, aby nástroje mohly zpracovávat požadavky HTTP.

2. Zkopírujte adresu URL vaší funkce z výstupu modulu runtime služby Azure Functions.  

    ![Místní modul runtime Azure](./media/functions-create-your-first-function-visual-studio/functions-vstools-f5.png)

3. Vložte adresu URL pro požadavek HTTP do panelu adresy prohlížeče. K této adrese URL připojte řetězec dotazu `&name=<yourname>` a proveďte požadavek. Následuje ukázka odezvy na místní požadavek GET vrácené funkcí v prohlížeči: 

    ![Odezva místního hostitele funkce v prohlížeči](./media/functions-create-your-first-function-visual-studio/functions-test-local-browser.png)

4. Pokud chcete zastavit ladění, klikněte na tlačítko **Zastavit** na panelu nástrojů sady Visual Studio.

Po ověření správného fungování funkce na místním počítači je na čase publikovat projekt do Azure.

## <a name="publish-the-project-to-azure"></a>Publikování projektu do Azure

Před publikováním projektu musíte mít v předplatném Azure aplikaci funkcí. Aplikaci funkcí můžete vytvořit přímo v sadě Visual Studio.

[!INCLUDE [Publish the project to Azure](../../includes/functions-vstools-publish.md)]

## <a name="test-your-function-in-azure"></a>Testování funkce v Azure

1. Zkopírujte základní adresu URL aplikace funkcí ze stránky Publikovat profil. Nahraďte část adresy URL použité při místním testování funkce, která obsahuje `localhost:port`, novou základní adresou URL. Stejně jako dříve nezapomeňte k této adrese URL připojit řetězec dotazu `&name=<yourname>` a provést požadavek.

    Adresa URL, která volá vaši funkci aktivovanou protokolem HTTP, vypadá nějak takto:

        http://<functionappname>.azurewebsites.net/api/<functionname>?name=<yourname> 

2. Vložte tuto novou adresu URL pro požadavek HTTP do panelu Adresa prohlížeče. Následuje ukázka odezvy na vzdálený požadavek GET vrácené funkcí v prohlížeči: 

    ![Odezva funkce v prohlížeči](./media/functions-create-your-first-function-visual-studio/functions-test-remote-browser.png)
 
## <a name="next-steps"></a>Další kroky

Pomocí sady Visual Studio jste vytvořili aplikaci funkcí jazyka C# s jednoduchou funkcí aktivovanou protokolem HTTP. 

+ Pokud chcete zjistit, jak nakonfigurovat projekt tak, aby podporoval další typy triggerů a vazeb, přečtěte si část [Konfigurace projektu pro místní vývoj](functions-develop-vs.md#configure-the-project-for-local-development) v tématu [Azure Functions Tools for Visual Studio](functions-develop-vs.md).
+ Další informace o místním testování a ladění pomocí základních funkcí služby Azure Functions najdete v tématu [Místní kódování a testování služby Azure Functions](functions-run-local.md). 
+ Další informace o vývoji funkcí jako knihoven tříd .NET najdete v tématu [Použití knihoven tříd .NET se službou Azure Functions](functions-dotnet-class-library.md). 

