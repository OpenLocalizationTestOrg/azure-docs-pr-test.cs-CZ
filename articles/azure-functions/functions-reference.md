---
title: "aaaGuidance pro vývoj Azure Functions | Microsoft Docs"
description: "Další koncepty Azure Functions hello a techniky, je nutné, aby toodevelop funkcí v Azure, ve všech programovací jazyky a vazeb."
services: functions
documentationcenter: na
author: christopheranderson
manager: erikre
editor: 
tags: 
keywords: "vývojáře průvodce azure funkce, funkce, událostí zpracování, webhooků, dynamické výpočetní, bez serveru architektura"
ms.assetid: d8efe41a-bef8-4167-ba97-f3e016fcd39e
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/30/2017
ms.author: chrande
ms.openlocfilehash: 86d37dae5333f615faafc966e9da6e08e0a6354e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-developers-guide"></a>Příručka pro vývojáře Azure funkce
V Azure Functions se konkrétní funkce sdílet několik klíčových technických konceptech a součásti, bez ohledu na jazyk hello nebo vazby, které používáte. Před přechodem do učení podrobnosti o konkrétní tooa danou jazyk nebo vazba být jisti tooread prostřednictvím tento přehled, která se použije tooall z nich.

Tento článek předpokládá, že jste si přečetli již hello [přehled Azure Functions](functions-overview.md) a jsou obeznámeni s [WebJobs SDK koncepty, jako jsou aktivační události, vazby a hello JobHost runtime](../app-service-web/websites-dotnet-webjobs-sdk.md). Azure Functions je založena na hello WebJobs SDK. 

## <a name="function-code"></a>Kód – funkce
A *funkce* je primární koncept hello v Azure Functions. Psaní kódu pro funkce v jazyce podle vaší volby a uložte hello kódu a konfigurační soubory v hello stejné složce. Konfigurace Hello jmenuje `function.json`, která obsahuje konfigurační data JSON. Podporuje různé jazyky a každé z nich má mírně odlišné prostředí optimalizované toowork nejvhodnější pro tento jazyk. 

soubor function.json Hello definuje hello vazby funkcí a dalších nastavení konfigurace. modul runtime Hello používá tento soubor toodetermine hello události toomonitor a jak toopass dat do a vrátit data z funkce provádění. Hello následuje příklad souboru function.json.

```json
{
    "disabled":false,
    "bindings":[
        // ... bindings here
        {
            "type": "bindingType",
            "direction": "in",
            "name": "myParamName",
            // ... more depending on binding
        }
    ]
}
```

Sada hello `disabled` vlastnost příliš`true` tooprevent hello funkce z spouštěna.

Hello `bindings` vlastnost je, kde můžete konfigurovat triggerů a vazeb. Každou vazbu sdílí několik běžných nastavení a některá nastavení, které jsou specifické tooa konkrétní typ vazby. Každé vazby vyžaduje hello následující nastavení:

| Vlastnost | Hodnoty nebo typy | Komentáře |
| --- | --- | --- |
| `type` |Řetězec |Typ vazby. Například, `queueTrigger`. |
| `direction` |v out |Určuje, zda hello vazby pro příjem dat do funkce hello nebo odeslání dat z funkce hello. |
| `name` |Řetězec |Hello název, který se používá pro hello vázaných dat v hello funkce. Pro jazyk C# to je název argumentu; pro jazyk JavaScript to je hello klíče v seznamu klíč/hodnota. |

## <a name="function-app"></a>Funkce aplikace
Funkce aplikace se skládá z jedné nebo více jednotlivých funkcí, které se spravují dohromady službou Azure App Service. Všechny funkce hello ve sdílené složce, funkce aplikace hello stejné ceny plán, průběžné nasazování a verze modulu runtime. Funkce, které jsou napsané v může více jazyků, že všechny sdílené složky hello stejné funkce aplikace. Vezměte v úvahu funkce aplikace jako způsob tooorganize a souhrnně spravovat funkcí. 

## <a name="runtime-script-host-and-web-host"></a>Modul runtime (hostitel skriptu a webového hostitele)
Hello runtime nebo skriptu hostitele, je hello základní sada WebJobs SDK hostitele, který naslouchá událostem, shromažďuje a odesílá data a nakonec spustí váš kód. 

aktivační události toofacilitate HTTP, je také webového hostitele, který je navrženou toosit před hello skriptu hostitele v produkčních scénářích. Má dva hostitele pomáhá tooisolate hello skriptu hostitele od provozu front-endu hello spravuje hello webového hostitele.

## <a name="folder-structure"></a>Struktura složek
[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Při vytváření projektu pro nasazení funkce tooa funkce aplikace v Azure App Service, můžete tuto strukturu považovat kódu lokality. Můžete použít stávající nástroje, například průběžnou integraci a nasazení, nebo vlastní nasazení skriptů pro tuto činnost čas instalace balíčku nasazení nebo code transpilation.

> [!NOTE]
> Ujistěte se, že toodeploy vaše `host.json` složek souborů a funkce přímo toohello `wwwroot` složky. Nezahrnovat hello `wwwroot` složky ve vašem nasazení. Jinak, v níž se `wwwroot\wwwroot` složek. 
> 
> 

## <a id="fileupdate"></a>Jak tooupdate funkce soubory aplikace
editor Funkce Hello součástí hello portál Azure vám umožní aktualizovat hello *function.json* soubor a soubor hello kód pro funkci. tooupload nebo aktualizace jiných souborů, jako *package.json* nebo *project.json* nebo závislosti, máte toouse jiné metody nasazení.

Funkce aplikace jsou postaveny na aplikační služby, takže všechny hello [možnosti nasazení webové aplikace k dispozici toostandard](../app-service-web/web-sites-deploy.md) jsou také k dispozici pro funkce aplikace. Tady jsou některé metody, můžete použít tooupload nebo funkce aplikace soubory aktualizace. 

#### <a name="toouse-app-service-editor"></a>toouse Editor služby aplikace
1. Na portálu Azure Functions hello, klikněte na tlačítko **funkce nastavení aplikace**.
2. V hello **Upřesnit nastavení** klikněte na tlačítko **přejít nastavení služby tooApp**.
3. Klikněte na tlačítko **aplikace služby Editor** v nabídce aplikace navigaci v části **nástroje pro vývoj**.
4. Klikněte na tlačítko **přejděte**.
   
   Po načtení Editor služby aplikace se zobrazí hello *host.json* složek souborů a funkce v části *wwwroot*. 
5. Tooedit otevřených souborů, nebo přetažení z vaší vývoj počítač tooupload souborů.

#### <a name="toouse-hello-function-apps-scm-kudu-endpoint"></a>koncový bod toouse hello funkce aplikace SCM (Kudu)
1. Přejděte do: `https://<function_app_name>.scm.azurewebsites.net`.
2. Klikněte na tlačítko **ladění konzoly > CMD**.
3. Přejděte příliš`D:\home\site\wwwroot\` tooupdate *host.json* nebo `D:\home\site\wwwroot\<function_name>` tooupdate soubory funkce.
4. Přetažení myší souboru chcete tooupload do příslušné složky hello v mřížce souboru hello. Existují dvě oblasti v mřížce hello souboru, kde můžete vložit do souboru. Pro *.zip* soubory, zobrazí se pole s popiskem hello "Přetáhněte sem tooupload a rozbalte." Pro ostatní typy souborů vyřaďte hello soubor mřížky, ale mimo pole "rozbalte" hello.

<!--NOTE: I've removed documentation on FTP, because it does not sync triggers on hello consumption plan --DonnaM -->

#### <a name="toouse-continuous-deployment"></a>průběžné nasazování toouse
Postupujte podle pokynů hello v tématu hello [průběžné nasazování pro Azure Functions](functions-continuous-deployment.md).

## <a name="parallel-execution"></a>Paralelní provádění
Dojde-li více spouštěcí událostí rychleji, než funkce jednovláknové runtime dokáže zpracovat, může hello runtime vyvolají funkci hello vícekrát paralelně.  Pokud funkce aplikace používá hello [spotřeba hostování plán](functions-scale.md#how-the-consumption-plan-works), může automaticky škálovat aplikaci funkce hello.  Každou instanci hello funkce aplikace, zda text hello aplikace běží hello spotřeba hostování plán nebo běžný [hostování plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), může zpracovat volání souběžných funkce paralelně pomocí více vláken.  maximální počet souběžných funkce volání v každé instanci funkce aplikace Hello se liší podle typu hello používá i hello prostředky využívané třídou jiných funkcí v rámci aplikace hello funkce aktivační události.

## <a name="functions-runtime-versioning"></a>Verze runtime funkce

Můžete nakonfigurovat hello verzi modulu runtime funkce hello použijte hello `FUNCTIONS_EXTENSION_VERSION` nastavení aplikace. Například hello hodnotu "~ 1" označuje, že funkce aplikace bude používat 1 jako jeho hlavní verzi. Funkce aplikace jsou upgradovaný tooeach nové podverze při jejich vydání. Hello přesnou verzi vaší aplikace funkce si můžete prohlédnout v hello **nastavení** ve hello portálu Azure.

## <a name="repositories"></a>Úložiště
Hello kód pro Azure Functions je open source a uložené v úložišť GitHub:

* [Azure Functions runtime](https://github.com/Azure/azure-webjobs-sdk-script/)
* [Azure Functions na portálu](https://github.com/projectkudu/AzureFunctionsPortal)
* [Šablony funkcí Azure](https://github.com/Azure/azure-webjobs-sdk-templates/)
* [Sada Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/)
* [Sada Azure WebJobs SDK rozšíření](https://github.com/Azure/azure-webjobs-sdk-extensions/)

## <a name="bindings"></a>Vazby
Zde je tabulku všechny podporované vazby.

[!INCLUDE [dynamic compute](../../includes/functions-bindings.md)]

## <a name="reporting-issues"></a>Hlášení problémů
[!INCLUDE [Reporting Issues](../../includes/functions-reporting-issues.md)]

## <a name="next-steps"></a>Další kroky
Další informace najdete v tématu hello následující prostředky:

* [Osvědčené postupy pro službu Azure Functions](functions-best-practices.md)
* [Azure funkcí jazyka C# referenční informace pro vývojáře](functions-reference-csharp.md)
* [Azure funkce F # referenční informace pro vývojáře](functions-reference-fsharp.md)
* [Referenční informace pro vývojáře Azure funkce NodeJS](functions-reference-node.md)
* [Azure funkce triggerů a vazeb](functions-triggers-bindings.md)
* [Azure Functions: hello cesty](https://blogs.msdn.microsoft.com/appserviceteam/2016/04/27/azure-functions-the-journey/) na blog týmu Azure App Service hello. Historie jak byla vyvinuta Azure Functions.

