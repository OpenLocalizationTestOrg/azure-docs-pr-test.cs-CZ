---
title: "aaaBuild bez serveru aplikace v sadě Visual Studio | Microsoft Docs"
description: "Začínáme s první aplikaci bez serveru v této příručce na vytváření, nasazování a Správa aplikace hello v sadě Visual Studio."
keywords: 
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 74530eea6060ffe2139f7c9d6daab8a46f808162
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-serverless-app-in-visual-studio-with-logic-apps-and-functions"></a>Sestavení bez serveru aplikace v sadě Visual Studio s Logic Apps a funkce

Pro rychlý vývoj a nasazení cloudových aplikací umožňují bez serveru nástroje a možnosti v Azure.  Tento dokument se zaměřuje na tooget spuštěna v sadě Visual Studio vytváření aplikaci bez serveru.  Přehled bez serveru v Azure [najdete v tomto článku](logic-apps-serverless-overview.md).

## <a name="getting-everything-ready"></a>Příprava vše

Zde jsou hello součásti potřebné toobuild aplikaci bez serveru ze sady Visual Studio:

* [Visual Studio 2017](https://www.visualstudio.com/vs/) nebo Visual Studio 2015 – Community, Professional nebo Enterprise
* [Logiku aplikace nástrojů pro Visual Studio](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551)
* [Nejnovější sadu Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 nebo vyšší)
* [Azure PowerShell](https://github.com/Azure/azure-powershell#installation)
* [Nástroje Azure základní funkce](https://www.npmjs.com/package/azure-functions-core-tools) toodebug funkce místně
* Návrhář aplikace logiky vložených toohello webového přístupu při použití hello

## <a name="getting-started-with-a-deployment-template"></a>Začínáme s šablony nasazení

Správa prostředků v Azure se provádějí v rámci skupiny prostředků.  Skupina prostředků je logické seskupení prostředků.  Skupiny prostředků povolit nasazení a správu kolekcí prostředků.  Pro aplikaci bez serveru v Azure obsahuje naše skupiny prostředků Azure Logic Apps i Azure Functions.  Pomocí hello projekt skupiny prostředků v sadě Visual Studio je možné toodevelop nám, správu a nasazení hello celá aplikace jako jednoho datového zdroje.

### <a name="create-a-resource-group-project-in-visual-studio"></a>Vytvoření projektu skupiny prostředků v sadě Visual Studio

1. V sadě Visual Studio, klikněte na tlačítko tooadd **nový projekt**
1. V hello **cloudu** kategorie, vyberte toocreate Azure **skupiny prostředků** projektu  
 * Pokud nevidíte hello kategorie nebo projektu uvedené, ujistěte se, že máte hello Azure SDK pro Visual Studio nainstalována
1. Název a umístění poskytnout hello projekt a vyberte **Ok** toocreate Visual Studio vyzve tooselect šablonu.  Můžete třeba vybrat toostart z prázdné, začněte s logiku aplikace nebo jiný prostředek.  V takovém případě však používáme tooget šablony Azure rychlý start, které nám začít s bez serveru aplikace.
1. Vyberte šablony tooshow z **Azure Quickstart** ![výběr Azure Quickstart šablony][1]
1. Vyberte hello bez serveru rychlý start šablony: **101-logic-app-and-function-app** a klikněte na tlačítko **Ok**

šablony rychlý start Hello vytvoří šablonu nasazení ve vašem projektu skupiny prostředků.  Šablona Hello obsahuje jednoduchou aplikaci logiky, která volá Azure Functions a vrátí výsledek hello.  Pokud otevřete hello `azuredeploy.json` hello soubor v Průzkumníku řešení, uvidíte hello prostředků pro aplikaci bez serveru hello.

## <a name="deploying-hello-serverless-application"></a>Nasazení aplikace bez serveru hello

Před vizuálního návrháře hello aplikace logiky můžete otevřít v sadě Visual Studio, je potřeba toobe předem nasazené skupiny prostředků Azure.  To umožňuje návrháře toocreate hello a použití připojení tooresources a služby v aplikaci logiky hello.  tooget spuštěna, potřebujeme jen řešení hello toodeploy vytvořili.

1. Klikněte pravým tlačítkem na projekt hello v sadě Visual Studio, vyberte **nasadit**a vytvořte **nový** nasazení ![výběr nového nasazení prostředků][2]
1. Vyberte platné předplatné Azure a skupiny prostředků
1. Vyberte příliš**nasadit** hello řešení
1. Zadejte název hello hello aplikace logiky a hello funkce aplikace Azure.  název funkce Azure Hello potřebuje toobe globálně jedinečný

řešení bez serveru Hello nasadí do hello zadaná skupina prostředků.  Pokud si prohlédnete hello **výstup** v sadě Visual Studio se zobrazí stav hello hello nasazení.

## <a name="editing-hello-logic-app-in-visual-studio"></a>Úpravy aplikace logiky hello v sadě Visual Studio

Jakmile hello řešení byla nasazena do libovolné skupině prostředků, hello vizuálního návrháře lze použít tooedit a vytvoření aplikace logiky toohello změny.

1. Klikněte pravým tlačítkem na hello `azuredeploy.json` v hello Průzkumníku řešení soubor a vyberte **otevřít s Logic Apps Návrhář**
1. Vyberte hello **skupiny prostředků** a **umístění** řešení hello byl nasazen tooand vyberte **OK**

Hello aplikace logiky vizuálního návrháře by teď měly být viditelné pomocí sady Visual Studio.  Můžete pokračovat tooadd kroky, upravte hello pracovního postupu a uložte změny.  Aplikace logiky můžete také vytvořit ze sady Visual Studio.  Pokud kliknete pravým tlačítkem na hello **prostředky** v hello Navigátor šablony, můžete zvolit tooadd **aplikace logiky** toohello projektu.  Aplikace logiky prázdný načíst v hello vizuálního návrháře bez předem nasadit do skupiny prostředků.

### <a name="managing-and-viewing-run-history-for-a-deployed-logic-app"></a>Správa a zobrazování historie spouštění aplikace nasazené logiky

Můžete také spravovat a zobrazit historii hello spustit pro logiku aplikace nasazené v Azure.  Pokud otevřete hello **Průzkumník cloudu** nástroje v sadě Visual Studio, klikněte pravým tlačítkem na všechny aplikace logiky a zvolte tooedit, zakázat, zobrazit vlastnosti, nebo zobrazení historie spouštění.  Kliknutím na tlačítko Upravit umožňuje také toodownload aplikace logiky publikované do projektu skupiny prostředků Visual Studio.  To znamená, že i v případě, že jste spustili, vytvářet aplikaci logiky v hello portálu Azure, můžete přesto ho importovat a spravovat ze sady Visual Studio.

## <a name="developing-an-azure-function-in-visual-studio"></a>Vývoj Azure funkce v sadě Visual Studio

Hello šablonu nasazení nasadí všechny funkce Azure, které jsou obsaženy v hello řešení pro úložiště git hello zadaný v hello `azuredeploy.json` proměnné.  Pokud vytváříte projekt funkce v rámci řešení hello, zkontrolujte jej do správy zdrojového kódu (GitHub, Visual Studio Team Services atd.) a aktualizujte hello `repo` proměnné, budou šablony hello nasazovat hello funkce Azure.

### <a name="creating-an-azure-function-project"></a>Vytvoření projektu Azure – funkce

Pokud pomocí jazyka JavaScript, Python, F #, Bash, Batch nebo prostředí PowerShell, postupujte podle hello [kroky hello funkce CLI](../azure-functions/functions-run-local.md) toocreate projektu.  Pokud vývoj funkce v jazyce C#, můžete použít [knihovny tříd jazyka C#](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/16/publishing-a-net-class-library-as-a-function-app/) v aktuálním řešení hello pro hello funkce Azure.

## <a name="next-steps"></a>Další kroky

* [Zjistěte, jak toobuild bez serveru sociálních řídicí panel](logic-apps-scenario-social-serverless.md)
* [Správa aplikace logiky z cloudu Průzkumníka Visual Studio](logic-apps-manage-from-vs.md)
* [Jazyk definic workflowů funkce Logic aplikace](logic-apps-workflow-definition-language.md)

<!-- Image references -->
[1]: ./media/logic-apps-serverless-get-started-vs/select-template.png
[2]: ./media/logic-apps-serverless-get-started-vs/deploy.png
