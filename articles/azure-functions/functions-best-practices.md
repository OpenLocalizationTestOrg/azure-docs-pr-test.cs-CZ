---
title: aaaBest postupy pro Azure Functions | Microsoft Docs
description: "Přečtěte si informace osvědčené postupy a vzory pro Azure Functions."
services: functions
documentationcenter: na
author: wesmc7777
manager: erikre
editor: 
tags: 
keywords: "řešení Azure functions, vzory, osvědčeným postupem, funkce, událostí zpracování, webhooků, dynamické výpočetní, bez serveru architektura"
ms.assetid: 9058fb2f-8a93-4036-a921-97a0772f503c
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 06/13/2017
ms.author: glenga
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bd3d826b75267a740e355b008943a48f71ebd339
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-hello-performance-and-reliability-of-azure-functions"></a>Optimalizace hello výkonu a spolehlivosti Azure functions

Tento článek obsahuje pokyny tooimprove hello výkonu a spolehlivosti aplikací pro funkce. 


## <a name="avoid-long-running-functions"></a>Vyhněte se dlouho běžící funkce

Velké, dlouhotrvajících funkce může způsobit neočekávané vypršení časového limitu problémy. Funkce se může stát velké z důvodu závislosti toomany Node.js. Import závislosti může také způsobit způsobit neočekávané vypršení časových limitů opakování zvýšené zátěže. Závislosti jsou načíst jak explicitně a implicitně. Jeden modul načteny kódu může načíst vlastní dalších modulů.  

Kdykoli je to možné, refactor velké funkce do menší funkce nastaví které pracují společně a rychle vrátit odpovědi. Například webhooku nebo funkce aktivace protokolu HTTP může vyžadovat odpověď potvrzení v určitých časovém limitu; je obvyklé, webhooky toorequire okamžitou reakci. Datová část aktivační událost hello HTTP můžete předat do fronty toobe, zpracovává aktivační funkce fronty. Tento přístup vám umožní toodefer hello samotnou práci a vrátí okamžitou reakci.


## <a name="cross-function-communication"></a>Mezi funkce komunikace

Při integraci víc funkcí, je obecně nejlepší postup toouse úložiště fronty pro různé funkce komunikace.  Hlavním důvodem Hello je fronty úložiště jsou levnější a mnohem snazší tooprovision. 

Jednotlivé zprávy ve frontě úložiště mají omezenou velikost too64 KB. Pokud potřebujete větší zprávy toopass mezi funkce, frontu zpráv Azure Service Bus může být použité toosupport zpráva velikosti až too256 KB.

Témata Service Bus jsou užitečné, pokud budete potřebovat filtrování před zpracováním zpráv.

Centra událostí jsou užitečné toosupport velkému komunikace.


## <a name="write-functions-toobe-stateless"></a>Zápis funkce toobe bezstavové 

Funkce by měla být bezstavové a idempotent, pokud je to možné. Přidružte žádné informace o stavu požadovaná data. Například pořadí zpracování by měla mít pravděpodobně přiřazený `state` člen. Funkce může zpracovat pořadí na základě tohoto stavu při současném zachování bezstavové hello funkce sám sebe. 

Funkce Idempotent doporučují zejména s aktivační události časovače. Například pokud máte něco, co nezbytně nutné spouštět jednou denně, zapisovat tak může probíhat kdykoli během dne hello s hello stejné výsledky. Funkce Hello můžete ukončit, i když není žádná práce pro určitý den. Také pokud předchozího spuštění nezdařilo toocomplete, hello příštím spuštění by měl vyzvedávat kde bylo přerušeno.


## <a name="write-defensive-functions"></a>Napsat Obranným funkce

Předpokládejme, že funkce může dojít k výjimce kdykoli. Návrh funkce s hello možnost toocontinue z předchozího bodu selhání při dalším spuštění hello. Vezměte v úvahu scénář, který vyžaduje hello následující akce:

1. Dotaz na 10 000 řádků v databáze.
2. Vytvořte zprávu fronty pro každou z těchto řádků tooprocess další dolů hello řádku.
 
V závislosti na tom, jak složitá je systém, můžete mít: související se situací službami pro příjem dat chovají chybně výpadky sítě nebo kvóty omezuje dosáhlo maximálního atd. Všechny tyto může ovlivnit funkce kdykoli. Je nutné toodesign vaše toobe funkce připravené pro ni.

Jak kódu reagovat Pokud dojde k selhání po vložení 5 000 těchto položek do fronty pro zpracování? Sledování položek v sadě, která po dokončení. Jinak může vložte je dalším. To může mít vážný dopad na pracovní postup. 

Pokud již byla zpracována položka fronty, povolte operaci-vaší toobe funkce.

Výhodou Obranným opatření již součástí, které použijete v platformě Azure Functions hello. Například v tématu **zpracování poškozených fronty zpráv** v dokumentaci hello [fronty Azure Storage aktivuje](functions-bindings-storage-queue.md#trigger).
 

## <a name="dont-mix-test-and-production-code-in-hello-same-function-app"></a>Nemáte kombinace testovací a produkční kód na hello stejné funkce aplikace

Funkce v rámci funkce aplikace sdílet prostředky. Například je sdílené paměti. Pokud používáte aplikaci funkce v produkčním prostředí, nepřidáte funkcí spojených se testovací a tooit prostředky. Může to způsobit neočekávané režii během provádění kódu produkční.

Dávejte pozor, zatížení ve svých aplikacích funkce produkční. Paměť je průměrem každou funkci v aplikaci hello.

Pokud máte sdílené sestavení odkazuje více funkcí rozhraní .net, můžete ji umístěte do běžné sdílené složky. Odkaz na sestavení hello s příkaz podobné toohello následující ukázka: 

    #r "..\Shared\MyAssembly.dll". 

Jinak, je snadné tooaccidentally nasadit více verzí testovací hello stejné binární soubor, který se chovat jinak mezi funkce.

Nepoužívejte podrobné protokolování v produkčním kódu. Má negativním dopadem na výkon.



## <a name="use-async-code-but-avoid-blocking-calls"></a>Použít asynchronní kód, ale zabránění blokování volání

Asynchronní programování je součástí osvědčeného postupu. Vždy-li však odkazující na hello `Result` vlastnost nebo volání `Wait` metodu `Task` instance. Tento přístup může vést toothread vyčerpání.


[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="next-steps"></a>Další kroky
Další informace najdete v tématu hello následující prostředky:

Protože Azure Functions využívá Azure App Service také měli být vědomi pokyny služby App Service.
* [Patterns and Practices optimalizací výkonu protokolu HTTP](https://docs.microsoft.com/azure/architecture/antipatterns/improper-instantiation/)

