---
title: "aaaSchema aktualizace června-1-2016 - Azure Logic Apps | Microsoft Docs"
description: "Vytvoření definic JSON pro Azure Logic Apps s verzí schématu 2016-06-01"
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 349d57e8-f62b-4ec6-a92f-a6e0242d6c0e
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 07/25/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: b0347fbbd692a93b63a2f8b741402a225450b35a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="schema-updates-for-azure-logic-apps---june-1-2016"></a>Aktualizace schématu pro Azure Logic Apps – 1. června 2016

Toto nové schéma a rozhraní API verze pro Azure Logic Apps zahrnuje klíčových vylepšení, které aplikace logiky více spolehlivé a snadnější toouse:

* [Obory](#scopes) umožňují seskupení nebo vnoření akce jako sadu akcí.
* [Podmínky a smyčky](#conditions-loops) jsou nyní prvotřídní akce.
* Přesnější řazení pro akce s hello `runAfter` vlastnost nahrazení`dependsOn`

aplikace logiky z hello 1 srpen 2015 tooupgrade náhled schématu toohello 1. června 2016 schématu, [podívejte se na část upgrade hello](##upgrade-your-schema).

<a name="scopes"></a>
## <a name="scopes"></a>Obory

Toto schéma zahrnuje obory, které umožňují akce skupiny společně nebo akce vnoření uvnitř sebe navzájem. Podmínku může například obsahovat jiné podmínky. Další informace o [obor syntaxe](../logic-apps/logic-apps-loops-and-scopes.md), nebo zkontrolujte tento příklad základní obor:

```
{
    "actions": {
        "My_Scope": {
            "type": "scope",
            "actions": {                
                "Http": {
                    "inputs": {
                        "method": "GET",
                        "uri": "http://www.bing.com"
                    },
                    "runAfter": {},
                    "type": "Http"
                }
            }
        }
    }
}
```

<a name="conditions-loops"></a>
## <a name="conditions-and-loops-changes"></a>Podmínky a smyčky změny

Ve schématu předchozí verze, podmínky a smyčky byly parametry přidružené k jedné akce. Toto schéma zruší toto omezení, aby podmínky a smyčky se nyní zobrazí jako typy akcí. Další informace o [smyčky a oborů](../logic-apps/logic-apps-loops-and-scopes.md), nebo zkontrolujte tento základní příklad pro podmínku akce:

```
{
    "If_trigger_is_some-trigger": {
        "type": "If",
        "expression": "@equals(triggerBody(), 'some-trigger')",
        "runAfter": { },
        "actions": {
            "Http_2": {
                "inputs": {
                    "method": "GET",
                    "uri": "http://www.bing.com"
                },
                "runAfter": {},
                "type": "Http"
            }
        },
        "else": 
        {
            "if_trigger_is_another-trigger": "..."
        }      
    }
}
```

<a name="run-after"></a>
## <a name="runafter-property"></a>Vlastnost 'runAfter.

Hello `runAfter` vlastnost nahradí `dependsOn`, poskytuje další přesnost, když zadáte hello spustit pořadí pro akce na základě hello stavu předchozí akcí.

Hello `dependsOn` vlastnost byla totožná s "hello akce byla spuštěna a bylo úspěšné", bez ohledu na to, jak často chtěli byste se nezdařilo. tooexecute akce, podle hello předchozí akce, jestli byla úspěšná nebo přeskočena. Hello `runAfter` vlastnost poskytuje tuto flexibilitu jako objekt, který určuje všechny hello názvy akcí, po které hello objektu spouští. Tato vlastnost také definuje pole stavy, které mohou být použity jako aktivační události. Například pokud byste chtěli toorun po kroku A úspěšné a také po kroku B úspěšná nebo neúspěšná, vytvoříte tím `runAfter` vlastnost:

```
{
    "...",
    "runAfter": {
        "A": ["Succeeded"],
        "B": ["Succeeded", "Failed"]
    }
}
```

## <a name="upgrade-your-schema"></a>Upgradu vašeho schématu

Upgrade toohello nové schéma pouze trvá několik kroků. Hello procesu upgradu zahrnuje spuštění skriptu aktualizace hello ukládání jako novou aplikaci logiky a pokud chcete, které by mohly mít přepsal hello předchozí aplikaci logiky.

1. V hello portálu Azure otevřete aplikaci logiky.

2. Přejděte příliš**přehled**. Na panelu nástrojů aplikace logiky hello, zvolte **aktualizovat schéma**.
   
    ![Zvolte Aktualizovat schéma][1]
   
    Hello upgradovaný definice se vrátí, které můžete zkopírovat a vložit do definice prostředků v případě potřeby. 
    Ale jsme **důrazně doporučujeme** zvolíte **uložit jako** toomake se, že všechny odkazy na připojení jsou platné v hello upgradovat aplikaci logiky.

3. V panelu nástrojů upgradu okno hello, zvolte **uložit jako**.

4. Zadejte název logiku hello a stav. toodeploy aplikace logiky upgradovaný zvolte **vytvořit**.

5. Potvrďte, že aplikace logiky upgradovaný funguje podle očekávání.
   
   > [!NOTE]
   > Pokud používáte ruční nebo žádostí o aktivační událost, změní se adresa URL hello zpětného volání v nové aplikace logiky. Test hello nové adresy URL toomake zda hello k kompletní prostředí funguje. toopreserve předchozí adresy URL, může klonovat přes existující aplikace logiky.

6. *Volitelné* zvolte aplikaci logiky předchozích verzí hello nové schéma, na panelu nástrojů hello toooverwrite **klon**, další příliš**aktualizovat schéma**. Tento krok je nutný pouze v případě, že chcete tookeep hello stejné ID nebo žádostí o aktivační události URL prostředku aplikace logiky.

### <a name="upgrade-tool-notes"></a>Poznámky k upgradu nástroje

#### <a name="mapping-conditions"></a>Mapování podmínky

V definici hello upgradovat nástroj hello umožňuje usilovně na seskupování true a false větve akce jako obor. Konkrétně hello návrháře vzor `@equals(actions('a').status, 'Skipped')` mají zobrazit jako `else` akce. Ale pokud hello nástroj zjistí nelze rozpoznat vzory, hello nástroj může vytvořit oddělené podmínky pro hello true a false větve hello. Můžete změnit mapování akcí po upgradu, v případě potřeby.

#### <a name="foreach-loop-with-condition"></a>smyčka 'typu foreach' s podmínkou

V nové schéma hello, můžete použít hello filtru akce tooreplicate hello vzor `foreach` smyčky s podmínkou na položku, ale tato změna automaticky dojde při upgradu. Podmínka Hello stane filtr akce před hello smyčka typu foreach pro vrácení pouze pole položek, které vyhovují podmínce hello a tohoto pole je předána do hello foreach akce. Příklad, naleznete v části [smyčky a obory](../logic-apps/logic-apps-loops-and-scopes.md).

#### <a name="resource-tags"></a>Značky prostředku

Po upgradu se značky prostředku se odeberou, takže je nutné obnovit pro pracovní postup hello upgradovat.

## <a name="other-changes"></a>Další změny

### <a name="renamed-manual-trigger-toorequest-trigger"></a>Přejmenovat too'request "Ruční" aktivační událost ' aktivační událost

Hello `manual` typ aktivační událost byla zastaralé a přejmenovat příliš`request` s typem `http`. Tato změna vytvoří další konzistence pro hello druh vzor, který hello aktivační události je použité toobuild.

### <a name="new-filter-action"></a>Nová akce 'filtru.

toofilter velké pole dolů tooa menší sadu položek hello nové `filter` typu přijímá pole a podmínku, vyhodnotí hello podmínku pro každou položku a vrátí pole s položkami splnění podmínky hello.

### <a name="restrictions-for-foreach-and-until-actions"></a>Omezení pro "foreach" a "do" akce

Hello `foreach` a `until` smyčky jsou omezené tooa jedné akce.

### <a name="new-trackedproperties-for-actions"></a>Nový 'trackedProperties' pro akce

Akce nyní můžete mít další vlastnost s názvem `trackedProperties`, který je na stejné úrovni toohello `runAfter` a `type` vlastnosti. Tento objekt určuje určité akce vstupy nebo výstupy, které chcete tooinclude v Azure diagnostiky telemetrii hello vygenerované jako součást pracovního postupu. Například:

```
{                
    "Http": {
        "inputs": {
            "method": "GET",
            "uri": "http://www.bing.com"
        },
        "runAfter": {},
        "type": "Http",
        "trackedProperties": {
            "responseCode": "@action().outputs.statusCode",
            "uri": "@action().inputs.uri"
        }
    }
}
```

## <a name="next-steps"></a>Další kroky
* [Vytvoření definice pracovního postupu pro logic apps](../logic-apps/logic-apps-author-definitions.md)
* [Vytvoření šablony pro nasazení pro logic apps](../logic-apps/logic-apps-create-deploy-template.md)

<!-- Image references -->
[1]: ./media/logic-apps-schema-2016-04-01/upgradeButton.png
