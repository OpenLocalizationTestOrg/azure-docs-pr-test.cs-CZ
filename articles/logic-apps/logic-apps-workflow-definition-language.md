---
title: aaaWorkflow schema Definition Language - Azure Logic Apps | Microsoft Docs
description: "Definovat pracovní postupy založené na hello pracovní postup definice schématu pro Azure Logic Apps"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 26c94308-aa0d-4730-97b6-de848bffff91
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 03/21/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: f12440e9ca269a9236132deeb888dbde7fb734d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="workflow-definition-language-schema-for-azure-logic-apps"></a>Schema Definition Language pracovního postupu pro Azure Logic Apps

Definice pracovního postupu obsahuje hello skutečné logiku, která spustí jako součást aplikace logiky. Tato definice zahrnuje jednu nebo více aktivačních událostí, které začínají aplikace logiky hello a jednu nebo více akcí pro tootake aplikace logiky hello.  
  
## <a name="basic-workflow-definition-structure"></a>Základní pracovní postup definice struktury

Tady je základní strukturu hello Definice pracovního postupu:  
  
```json
{
    "$schema": "<schema-of the-definition>",
    "contentVersion": "<version-number-of-definition>",
    "parameters": { <parameter-definitions-of-definition> },
    "triggers": [ { <definition-of-flow-triggers> } ],
    "actions": [ { <definition-of-flow-actions> } ],
    "outputs": { <output-of-definition> }
}
```
  
> [!NOTE]
> Hello [rozhraní API REST správy pracovního postupu](https://docs.microsoft.com/rest/api/logic/workflows) dokumentace obsahuje informace o tom, toocreate a spravovat pracovní postupy aplikace logiky.
  
|Název elementu|Požaduje se|Popis|  
|------------------|--------------|-----------------|  
|$schema|Ne|Určuje umístění hello hello JSON schématu souboru, který popisuje verze hello hello definice jazyka. Toto umístění je požadováno, když odkazujete definici externě. Tento dokument je hello umístění: <p>`https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2015-08-01-preview/workflowdefinition.json#`|  
|contentVersion|Ne|Určuje verzi definice hello. Když nasadíte pomocí hello Definice pracovního postupu, můžete tuto hodnotu toomake jistotu, že se používá správné definice hello.|  
|parameters|Ne|Určuje, že parametry použít tooinput data do definice hello. Může být definováno maximálně 50 parametry.|  
|Aktivační události|Ne|Určuje informace o hello aktivační události, které zahájí hello pracovního postupu. Může být definováno maximálně 10 aktivační události.|  
|Akce|Ne|Určuje akce, které se provádějí jako provede hello toku. Maximálně 250 akce může být definováno.|  
|Výstupy|Ne|Určuje informace o hello nasazení prostředků. Může být definováno maximálně 10 výstupy.|  
  
## <a name="parameters"></a>Parametry

V této části určuje všechny hello parametry, které se používají v hello Definice pracovního postupu v době nasazení. Všechny parametry musí být deklarován v této části, před použitím v dalších částech hello definice.  
  
Hello následující příklad ukazuje hello struktura definici parametru:  

```json
"parameters": {
    "<parameter-name>" : {
        "type" : "<type-of-parameter-value>",
        "defaultValue": <default-value-of-parameter>,
        "allowedValues": [ <array-of-allowed-values> ],
        "metadata" : { "key": { "name": "value"} }
    }
}
```

|Název elementu|Požaduje se|Popis|  
|------------------|--------------|-----------------|  
|type|Ano|**Typ**: řetězec <p> **Deklarace**:`"parameters": {"parameter1": {"type": "string"}` <p> **Specifikace**:`"parameters": {"parameter1": {"value": "myparamvalue1"}}` <p> **Typ**: securestring <p> **Deklarace**:`"parameters": {"parameter1": {"type": "securestring"}}` <p> **Specifikace**:`"parameters": {"parameter1": {"value": "myparamvalue1"}}` <p> **Typ**: int <p> **Deklarace**:`"parameters": {"parameter1": {"type": "int"}}` <p> **Specifikace**:`"parameters": {"parameter1": {"value" : 5}}` <p> **Typ**: bool <p> **Deklarace**:`"parameters": {"parameter1": {"type": "bool"}}` <p> **Specifikace**:`"parameters": {"parameter1": { "value": true }}` <p> **Typ**: pole <p> **Deklarace**:`"parameters": {"parameter1": {"type": "array"}}` <p> **Specifikace**:`"parameters": {"parameter1": { "value": [ array-of-values ]}}` <p> **Typ**: objekt <p> **Deklarace**:`"parameters": {"parameter1": {"type": "object"}}` <p> **Specifikace**:`"parameters": {"parameter1": { "value": { JSON-object } }}` <p> **Typ**: secureobject <p> **Deklarace**:`"parameters": {"parameter1": {"type": "object"}}` <p> **Specifikace**:`"parameters": {"parameter1": { "value": { JSON-object } }}` <p> **Poznámka:** hello `securestring` a `secureobject` typy nebudou zobrazeny v `GET` operace. Tento typ měli používat hesla, klíče a tajné klíče.|  
|Výchozí hodnota|Ne|Určuje hello výchozí hodnota pro parametr hello, pokud není zadaná žádná hodnota ve hello dobu, kdy je vytvořen prostředek hello.|  
|allowedValues|Ne|Určuje pole povolených hodnot pro parametr hello.|  
|Metadata|Ne|Určuje další informace o parametru hello, jako je čitelný popis nebo návrhu dat používané v sadě Visual Studio nebo jiných nástrojů.|  
  
Tento příklad ukazuje, jak můžete použít parametr v části textu hello akce:  
  
```json
"body" :
{
  "property1": "@parameters('parameter1')"
}
```

 Parametry můžete použít také v výstupy.  
  
## <a name="triggers-and-actions"></a>Aktivační události a akce  

Triggery a akce zadejte hello volání, které se můžou zapojit při provádění pracovního postupu. Podrobnosti o této části najdete v tématu [akce pracovního postupu a aktivační události](logic-apps-workflow-actions-triggers.md).
  
## <a name="outputs"></a>Výstupy  

Výstupy zadejte informace, které mohou být vráceny z pracovního postupu spustit. Například pokud máte konkrétní stav nebo hodnotu, kterou chcete tootrack při každém spuštění, můžete zahrnout, data v hello spustit výstupy. Hello data se zobrazí v hello REST API pro správu, pro které budou spouštěny a v hello uživatelské rozhraní pro správu pro tohoto spustit v hello portálu Azure. Tyto výstupy tooother externími systémy jako PowerBI může obtékat také pro vytváření řídicích panelů. Výstupy jsou *není* použít toorespond tooincoming požadavky na hello rozhraní API služby REST. toorespond tooan příchozího požadavku pomocí hello `response` typ akce, tady je příklad:
  
```json
"outputs": {  
  "key1": {  
    "value": "value1",  
    "type" : "<type-of-value>"  
  }  
} 
```

|Název elementu|Požaduje se|Popis|  
|------------------|--------------|-----------------|  
|key1|Ano|Určuje identifikátor hello klíče pro výstup hello. Nahraďte **key1** s názvem, které chcete toouse tooidentify hello výstup.|  
|hodnota|Ano|Určuje hodnotu hello hello výstupu.|  
|type|Ano|Určuje typ hello hello hodnotu, která byla zadána. Možné typy hodnoty jsou: <ul><li>`string`</li><li>`securestring`</li><li>`int`</li><li>`bool`</li><li>`array`</li><li>`object`</li></ul>|
  
## <a name="expressions"></a>Výrazy  

Hodnoty JSON v definici hello mohou být literál nebo můžou být výrazů, které jsou vyhodnocována po hello definice se používá. Například:  
  
```json
"name": "value"
```

 nebo  
  
```json
"name": "@parameters('password') "
```

> [!NOTE]
> Některé výrazy získat své hodnoty z modulu runtime akce, které nemusí existovat na začátku hello provádění hello. Můžete použít **funkce** toohelp načíst některé z těchto hodnot.  
  
Výrazy může vyskytovat kdekoli v hodnotu řetězce formátu JSON a vždy mít za následek jinou hodnotu JSON. Při hodnotu formátu JSON, musí být určené toobe výrazu, hello textu hello výrazu je extrahován odebráním hello-zavináče (@). Pokud je potřeba řetězcový literál, který začíná @, že řetězec, je nutné uvést pomocí@. Hello následující příklady ukazují, jak se vyhodnocují výrazy.  
  
|Hodnota JSON|výsledek|  
|----------------|------------|  
|"parametry"|Hello znaků, parametry, jsou vráceny.|  
|"parametry [1]"|jsou vráceny znaky Hello 'parametry [1]'.|  
|"@@"|Řetězec 1 znak, který obsahuje ' @' je vrácen.|  
|" @"|Řetězec 2 znak, který obsahuje ' @' je vrácen.|  
  
S *řetězec interpolace*, výrazy se může zobrazit i uvnitř řetězců, kde jsou výrazy uzavřen do `@{ ... }`. Například: <p>`"name" : "First Name: @{parameters('firstName')} Last Name: @{parameters('lastName')}"`

Hello výsledek je vždy řetězec, takže je tato funkce podobné toohello `concat` funkce. Předpokládejme, že jste definovali `myNumber` jako `42` a `myString` jako `sampleString`:  
  
|Hodnota JSON|výsledek|  
|----------------|------------|  
|"@parameters(myString)"|Vrátí `sampleString` jako řetězec.|  
|"@{parameters('myString')}"|Vrátí `sampleString` jako řetězec.|  
|"@parameters(myNumber)"|Vrátí `42` jako *číslo*.|  
|"@{parameters('myNumber')}"|Vrátí `42` jako *řetězec*.|  
|"Odpověď je: @{parameters('myNumber')}"|Vrátí hello řetězec `Answer is: 42`.|  
|"@concat(' Odpověď je: ', string(parameters('myNumber')))"|Vrátí řetězec hello`Answer is: 42`|  
|"Odpověď je: @@ {parameters('myNumber')}"|Vrátí hello řetězec `Answer is: @{parameters('myNumber')}`.|  
  
## <a name="operators"></a>Operátory  

Operátory jsou hello znaků, které můžete použít ve výrazech nebo funkce. 
  
|Operátor|Popis|  
|--------------|-----------------|  
|.|tečka – operátor Hello vám umožní tooreference vlastnosti objektu|  
|?|operátor otazník Hello umožňuje hodnotu null vlastnosti objektu bezchybně runtime odkazovat. Například můžete použít tuto hodnotu null výrazu toohandle aktivovat výstupy: <p>`@coalesce(trigger().outputs?.body?.property1, 'my default value')`|  
|'|jednoduché uvozovky Hello je hello pouze způsob toowrap textové literály. Dvojité uvozovky uvnitř výrazy nelze použít, protože tohoto rozdělení je v konfliktu s uvozovky hello JSON, který zabalí hello celý výraz.|  
|[]|Hello hranaté závorky lze použít tooget hodnota z pole s indexem konkrétní. Například, pokud máte akci, která předá `range(0,10)`v toohello `forEach` funkce, můžete použít položky této funkce tooget out polí:  <p>`myArray[item()]`|  
  
## <a name="functions"></a>Funkce  

Také můžete volat funkce v rámci výrazy. Hello následující tabulka uvádí hello funkce, které můžete použít ve výrazu.  
  
|výraz|Vyhodnocení|  
|----------------|----------------|  
|"@function("Hello")"|Volání hello funkce členem hello Definice řetězcový literál hello Hello jako první parametr hello.|  
|"@function('Je nástrojů!") "|Volání členských funkce hello hello definice s hello řetězcového literálu, je nástrojů!' jako první parametr hello|  
|"@function.prop1 ()"|Vrátí hello hodnotou vlastnosti vlastnost1 hello hello `myfunction` členem hello definice.|  
|"@function.Prop1 ("hello")"|Volání funkce členem hello Definice řetězcový literál hello "Hello" hello jako hello první parametr a vrátí hello vlastnost1 vlastnost objektu hello.|  
|"@function(parameters('Hello'))"|Vyhodnotí hello Hello parametr a předá hodnotu toofunction hello|  
  
### <a name="referencing-functions"></a>Odkazování na funkce  

Můžete použít tyto funkce tooreference výstupy z jiné akce v aplikaci logiky hello nebo hodnoty předané při vytvoření aplikace logiky hello. Například můžete odkazovat hello data z jednoho kroku toouse ji v jiném.  
  
|Název funkce|Popis|  
|-------------------|-----------------|  
|parameters|Vrátí hodnotu parametru, která je definována v definici hello. <p>`parameters('password')` <p> **Parametr číslo**: 1 <p> **Název**: parametr <p> **Popis**: vyžaduje. Hello název parametru hello, jejichž hodnoty se mají.|  
|Akce|Umožňuje výrazu tooderive svou hodnotu z jiných JSON název a hodnotu páry nebo hello výstup hello aktuální akce. modul runtime. Vlastnost Hello reprezentována propertyPath v hello následující ukázka je nepovinná. Pokud není zadán propertyPath, hello odkaz je objekt toohello celé akce. Tuto funkci lze použít pouze uvnitř proveďte – dokud podmínky akce. <p>`action().outputs.body.propertyPath`|  
|Akce|Umožňuje výrazu tooderive svou hodnotu z jiných JSON název a hodnotu páry nebo hello výstup hello runtime akce. Tyto výrazy explicitně deklarovat, že jedna akce závisí na jiné akce. Vlastnost Hello reprezentována propertyPath v hello následující ukázka je nepovinná. Pokud není zadán propertyPath, hello odkaz je objekt toohello celé akce. Můžete použít buď tento element nebo hello podmínky element toospecify závislosti, ale nepotřebujete toouse i pro hello stejné závislý prostředek. <p>`actions('myAction').outputs.body.propertyPath` <p> **Parametr číslo**: 1 <p> **Název**: název akce <p> **Popis**: vyžaduje. Název akce hello jejichž hodnoty se mají Hello. <p> Jsou k dispozici vlastnosti objektu hello akce: <ul><li>`name`</li><li>`startTime`</li><li>`endTime`</li><li>`inputs`</li><li>`outputs`</li><li>`status`</li><li>`code`</li><li>`trackingId`</li><li>`clientTrackingId`</li></ul> <p>V tématu hello [Rest API](http://go.microsoft.com/fwlink/p/?LinkID=850646) podrobnosti o tyto vlastnosti.|
|Aktivační události|Umožňuje výrazu tooderive svou hodnotu z jiných název JSON a páry hodnota nebo hello výstup hello runtime aktivační události. Vlastnost Hello reprezentována propertyPath v hello následující ukázka je nepovinná. Pokud není zadán propertyPath, hello odkaz je toohello celou aktivační události objektu. <p>`trigger().outputs.body.propertyPath` <p>Při použití uvnitř vstupy aktivační události, vrátí funkce hello hello výstupy hello předchozí zpracování. Ale pokud se použije uvnitř aktivační podmínka, hello `trigger` funkce vrátí hello výstupy aktuální provádění hello. <p> Jsou k dispozici vlastnosti hello aktivační události objektu: <ul><li>`name`</li><li>`scheduledTime`</li><li>`startTime`</li><li>`endTime`</li><li>`inputs`</li><li>`outputs`</li><li>`status`</li><li>`code`</li><li>`trackingId`</li><li>`clientTrackingId`</li></ul> <p>V tématu hello [Rest API](http://go.microsoft.com/fwlink/p/?LinkID=850644) podrobnosti o tyto vlastnosti.|
|actionOutputs|Tato funkce je sdružená vlastnost`actions('actionName').outputs` <p> **Parametr číslo**: 1 <p> **Název**: název akce <p> **Popis**: vyžaduje. Název akce hello jejichž hodnoty se mají Hello.|  
|actionBody a text|Tyto funkce jsou sdružená vlastnost`actions('actionName').outputs.body` <p> **Parametr číslo**: 1 <p> **Název**: název akce <p> **Popis**: vyžaduje. Název akce hello jejichž hodnoty se mají Hello.|  
|triggerOutputs|Tato funkce je sdružená vlastnost`trigger().outputs`|  
|triggerBody|Tato funkce je sdružená vlastnost`trigger().outputs.body`|  
|Položka|Při použití uvnitř opakující se akce, funkce vrátí hodnotu hello položku, která je v poli hello pro tento iterace hello akce. Pokud máte akci, která spouští pro každou položku pole zpráv, můžete například použijte následující syntaxi: <p>`"input1" : "@item().subject"`| 
  
### <a name="collection-functions"></a>Kolekce funkcí  

Tyto funkce pracovat s kolekcí a tooArrays, řetězce a někdy slovník se obecně používají.  
  
|Název funkce|Popis|  
|-------------------|-----------------|  
|Obsahuje|Vrátí hodnotu PRAVDA, pokud slovník obsahuje seznam klíčů, obsahuje hodnotu nebo řetězec obsahuje dílčí řetězec. Například funkce vrátí hodnotu `true`: <p>`contains('abacaba','aca')` <p> **Parametr číslo**: 1 <p> **Název**: v rámci kolekce <p> **Popis**: vyžaduje. kolekce toosearch Hello v rámci. <p> **Parametr číslo**: 2 <p> **Název**: najít objekt <p> **Popis**: vyžaduje. Hello objekt toofind uvnitř hello **v rámci kolekce**.|  
|Délka|Vrátí hello počet elementů v pole nebo řetězec. Například funkce vrátí hodnotu `3`:  <p>`length('abc')` <p> **Parametr číslo**: 1 <p> **Název**: kolekce <p> **Popis**: vyžaduje. Hello kolekce pro tooget hello znaků.|  
|prázdný|Vrátí hodnotu true Pokud objekt, pole nebo řetězec je prázdný. Například funkce vrátí hodnotu `true`: <p>`empty('')` <p> **Parametr číslo**: 1 <p> **Název**: kolekce <p> **Popis**: vyžaduje. kolekce toocheck Hello, pokud je prázdná.|  
|průnik|Vrací jednu pole nebo objekt, který má společné prvky mezi pole nebo objekty předaná. Například funkce vrátí hodnotu `[1, 2]`: <p>`intersection([1, 2, 3], [101, 2, 1, 10],[6, 8, 1, 2])` <p>Hello parametry pro funkci hello může být buď sadu objektů, nebo sadu pole (ne kombinaci těchto dvou možností). Pokud jsou dva objekty s hello stejný název, hello poslední objekt s tímto názvem se zobrazí v posledním objektu hello. <p> **Parametr číslo**: 1...*n* <p> **Název**: kolekce*n* <p> **Popis**: vyžaduje. kolekce tooevaluate Hello. Objekt musí být ve všech kolekcích předaná tooappear ve výsledku hello.|  
|sjednocení|Vrátí objekt s všechny hello prvky, které jsou v pole nebo objekt předaný funkci toothis nebo jednoho pole. Například funkce vrátí hodnotu `[1, 2, 3, 10, 101]`: <p>`union([1, 2, 3], [101, 2, 1, 10])` <p>Hello parametry pro funkci hello může být buď sadu objektů, nebo sadu pole (není jejich směs). Pokud jsou dva objekty s hello stejný název v závěrečný výstup hello, se zobrazí v konečné objekt hello hello poslední objekt se stejným názvem. <p> **Parametr číslo**: 1...*n* <p> **Název**: kolekce*n* <p> **Popis**: vyžaduje. kolekce tooevaluate Hello. Objekt, který se zobrazí v některé z kolekcí hello se zobrazí také v hello výsledek.|  
|první|Vrátí hello první prvek v poli hello nebo předaný řetězec. Například funkce vrátí hodnotu `0`: <p>`first([0,2,3])` <p> **Parametr číslo**: 1 <p> **Název**: kolekce <p> **Popis**: vyžaduje. Hello tootake hello první objekt kolekce z.|  
|poslední|Vrátí hello posledním prvkem v hello pole nebo řetězec předaná. Například funkce vrátí hodnotu `3`: <p>`last('0123')` <p> **Parametr číslo**: 1 <p> **Název**: kolekce <p> **Popis**: vyžaduje. Hello tootake hello poslední objekt kolekce z.|  
|proveďte|Vrátí první hello **počet** předaná elementy z hello pole nebo řetězec. Například funkce vrátí hodnotu `[1, 2]`:  <p>`take([1, 2, 3, 4], 2)` <p> **Parametr číslo**: 1 <p> **Název**: kolekce <p> **Popis**: vyžaduje. kolekce Hello z kde tootake hello nejprve **počet** objekty. <p> **Parametr číslo**: 2 <p> **Název**: počet <p> **Popis**: vyžaduje. počet objektů tootake z hello Hello **kolekce**. Musí být kladné celé číslo.|  
|Přeskočit|Vrátí hello prvků v poli hello začínající na indexu **počet**. Například funkce vrátí hodnotu `[3, 4]`: <p>`skip([1, 2 ,3 ,4], 2)` <p> **Parametr číslo**: 1 <p> **Název**: kolekce <p> **Popis**: vyžaduje. nejprve Hello kolekce tooskip hello **počet** objekty z. <p> **Parametr číslo**: 2 <p> **Název**: počet <p> **Popis**: vyžaduje. počet objektů tooremove z hello před Hello **kolekce**. Musí být kladné celé číslo.|  
|join|Vrátí řetězec s každou položkou pole spojena oddělovač, například tento příkaz vrátí `"1,2,3,4"`:<br /><br /> `join([1, 2, 3, 4], ',')`<br /><br /> **Parametr číslo**: 1<br /><br /> **Název**: kolekce<br /><br /> **Popis**: vyžaduje. Hello kolekce toojoin položky z.<br /><br /> **Parametr číslo**: 2<br /><br /> **Název**: oddělovač<br /><br /> **Popis**: vyžaduje. Hello řetězec toodelimit položky s.|  
  
### <a name="string-functions"></a>Řetězcové funkce

Následující funkce pouze Hello použít toostrings. Můžete taky některé funkce kolekce řetězce.  
  
|Název funkce|Popis|  
|-------------------|-----------------|  
|concat|Kombinuje libovolný počet řetězce společně. Například, pokud je parametr 1 `p1`, funkce vrátí hodnotu `somevalue-p1-somevalue`: <p>`concat('somevalue-',parameters('parameter1'),'-somevalue')` <p> **Parametr číslo**: 1...*n* <p> **Název**: řetězec*n* <p> **Popis**: vyžaduje. Hello toocombine řetězce do jednoho řetězce.|  
|dílčí řetězec|Vrátí podmnožinu znaků z řetězce. Například funkce vrátí hodnotu `abc`: <p>`substring('somevalue-abc-somevalue',10,3)` <p> **Parametr číslo**: 1 <p> **Název**: řetězec <p> **Popis**: vyžaduje. Hello řetězec, ze které hello je provedena dílčí řetězec. <p> **Parametr číslo**: 2 <p> **Název**: počáteční index <p> **Popis**: vyžaduje. index Hello kde začíná hello podřetězec v parametru 1. <p> **Parametr číslo**: 3 <p> **Název**: délka <p> **Popis**: vyžaduje. Délka Hello hello dílčí řetězec.|  
|Nahradit|Nahradí řetězec daný řetězec. Například funkce vrátí hodnotu `hello new string`: <p>`replace('hello old string', 'old', 'new')` <p> **Parametr číslo**: 1 <p> **Název**: řetězec <p> **Popis**: vyžaduje. Hello řetězec, který je hledán parametr 2 a aktualizovat s parametrem 3, když v parametru 1 je nalezen parametr 2. <p> **Parametr číslo**: 2 <p> **Název**: původní řetězec <p> **Popis**: vyžaduje. Hello řetězec tooreplace s parametrem 3, pokud je nalezena shoda v parametru 1 <p> **Parametr číslo**: 3 <p> **Název**: nový řetězec <p> **Popis**: vyžaduje. Hello řetězec, který je použité tooreplace hello řetězec v parametru 2 Pokud je nalezena shoda v parametru 1.|  
|Identifikátor GUID|Tato funkce generuje řetězec globálně jedinečný (identifikátor GUID). Tuto funkci můžete například vygenerovat tento identifikátor GUID:`c2ecc88d-88c8-4096-912c-d6f2e2b138ce` <p>`guid()` <p> **Parametr číslo**: 1 <p> **Název**: formát <p> **Popis**: volitelné. Specifikátor jednoho formátu, který označuje [jak tooformat hello hodnotu identifikátoru Guid](https://msdn.microsoft.com/library/97af8hh4%28v=vs.110%29.aspx). Parametr formát Hello může být "N", "D", "B", "P" nebo "X". Pokud formát není zadaný, použije se "D".|  
|toLower|Převede řetězec toolowercase. Například funkce vrátí hodnotu `two by two is four`: <p>`toLower('Two by Two is Four')` <p> **Parametr číslo**: 1 <p> **Název**: řetězec <p> **Popis**: vyžaduje. Hello řetězec tooconvert toolower velká a malá písmena. Znak v řetězci hello nemá malá ekvivalentní, hello znak je zahrnuta v hello vrátil řetězec beze změny.|  
|toUpper|Převede řetězec toouppercase. Například funkce vrátí hodnotu `TWO BY TWO IS FOUR`: <p>`toUpper('Two by Two is Four')` <p> **Parametr číslo**: 1 <p> **Název**: řetězec <p> **Popis**: vyžaduje. Hello řetězec tooconvert tooupper velká a malá písmena. Znak v řetězci hello nemá velká ekvivalentní, hello znak je zahrnuta v hello vrátil řetězec beze změny.|  
|IndexOf|Insensitively nalézt hello index hodnoty v případě řetězec. Například funkce vrátí hodnotu `7`: <p>`indexof('hello, world.', 'world')` <p> **Parametr číslo**: 1 <p> **Název**: řetězec <p> **Popis**: vyžaduje. Hello řetězec, který může obsahovat hodnotu hello. <p> **Parametr číslo**: 2 <p> **Název**: řetězec <p> **Popis**: vyžaduje. Hello hodnota toosearch hello index.|  
|LastIndexOf|Insensitively najít hello index poslední hodnotu v případě řetězec. Například funkce vrátí hodnotu `3`: <p>`lastindexof('foofoo', 'foo')` <p> **Parametr číslo**: 1 <p> **Název**: řetězec <p> **Popis**: vyžaduje. Hello řetězec, který může obsahovat hodnotu hello. <p> **Parametr číslo**: 2 <p> **Název**: řetězec <p> **Popis**: vyžaduje. Hello hodnota toosearch hello index.|  
|StartsWith|Kontroluje, pokud hello, začíná řetězec hodnotu případu insensitively. Například funkce vrátí hodnotu `true`: <p>`startswith('hello, world', 'hello')` <p> **Parametr číslo**: 1 <p> **Název**: řetězec <p> **Popis**: vyžaduje. Hello řetězec, který může obsahovat hodnotu hello. <p> **Parametr číslo**: 2 <p> **Název**: řetězec <p> **Popis**: vyžaduje. řetězec hello Hello může začínat.|  
|EndsWith|Kontroluje, zda řetězec hello končí případem hodnotu insensitively. Například funkce vrátí hodnotu `true`: <p>`endswith('hello, world', 'world')` <p> **Parametr číslo**: 1 <p> **Název**: řetězec <p> **Popis**: vyžaduje. Hello řetězec, který může obsahovat hodnotu hello. <p> **Parametr číslo**: 2 <p> **Název**: řetězec <p> **Popis**: vyžaduje. řetězec hello Hello mohou končit.|  
|split|Rozdělí hello řetězec pomocí oddělovače. Například funkce vrátí hodnotu `["a", "b", "c"]`: <p>`split('a;b;c',';')` <p> **Parametr číslo**: 1 <p> **Název**: řetězec <p> **Popis**: vyžaduje. Hello řetězec, který je rozdělení. <p> **Parametr číslo**: 2 <p> **Název**: řetězec <p> **Popis**: vyžaduje. Oddělovač Hello.|  
  
### <a name="logical-functions"></a>Logické funkce  

Tyto funkce jsou užitečné v podmínkách a mohou být použité tooevaluate jakéhokoli typu logiky.  
  
|Název funkce|Popis|  
|-------------------|-----------------|  
|Rovná se|Pokud vrátí hodnotu true jsou dvě hodnoty rovny. Například pokud parameter1 someValue, funkce vrátí hodnotu `true`: <p>`equals(parameters('parameter1'), 'someValue')` <p> **Parametr číslo**: 1 <p> **Název**: objektu 1 <p> **Popis**: vyžaduje. objekt toocompare Hello příliš**objekt 2**. <p> **Parametr číslo**: 2 <p> **Název**: objekt 2 <p> **Popis**: vyžaduje. objekt toocompare Hello příliš**objektu 1**.|  
|menší|Vrátí hodnotu true, pokud první argument hello menší než hello druhý. Všimněte si, může být pouze hodnoty typu integer, float nebo řetězec. Například funkce vrátí hodnotu `true`: <p>`less(10,100)` <p> **Parametr číslo**: 1 <p> **Název**: objektu 1 <p> **Popis**: vyžaduje. objekt toocheck text Hello, pokud je menší než **objekt 2**. <p> **Parametr číslo**: 2 <p> **Název**: objekt 2 <p> **Popis**: vyžaduje. objekt toocheck text Hello, pokud je větší než **objektu 1**.|  
|lessOrEquals|Vrátí hodnotu PRAVDA, pokud je hello argument menší než nebo roven hodnotě toohello druhý. Všimněte si, může být pouze hodnoty typu integer, float nebo řetězec. Například funkce vrátí hodnotu `true`: <p>`lessOrEquals(10,10)` <p> **Parametr číslo**: 1 <p> **Název**: objektu 1 <p> **Popis**: vyžaduje. objekt toocheck text Hello, pokud je menší nebo roven hodnotě příliš**objekt 2**. <p> **Parametr číslo**: 2 <p> **Název**: objekt 2 <p> **Popis**: vyžaduje. objekt toocheck text Hello, pokud je větší než nebo roven hodnotě příliš**objektu 1**.|  
|větší|Vrátí hodnotu true, pokud první argument hello větší než hello druhý. Všimněte si, může být pouze hodnoty typu integer, float nebo řetězec. Například funkce vrátí hodnotu `false`:  <p>`greater(10,10)` <p> **Parametr číslo**: 1 <p> **Název**: objektu 1 <p> **Popis**: vyžaduje. objekt toocheck text Hello, pokud je větší než **objekt 2**. <p> **Parametr číslo**: 2 <p> **Název**: objekt 2 <p> **Popis**: vyžaduje. objekt toocheck text Hello, pokud je menší než **objektu 1**.|  
|greaterOrEquals|Vrátí hodnotu true, pokud první argument hello je větší než nebo rovna toohello druhý. Všimněte si, může být pouze hodnoty typu integer, float nebo řetězec. Například funkce vrátí hodnotu `false`: <p>`greaterOrEquals(10,100)` <p> **Parametr číslo**: 1 <p> **Název**: objektu 1 <p> **Popis**: vyžaduje. objekt toocheck text Hello, pokud je větší než nebo roven hodnotě příliš**objekt 2**. <p> **Parametr číslo**: 2 <p> **Název**: objekt 2 <p> **Popis**: vyžaduje. objekt toocheck text Hello, pokud je menší než nebo rovna příliš**objektu 1**.|  
|a|Vrátí hodnotu true, pokud oba parametry mají hodnotu true. Oba argumenty třeba toobe logické hodnoty. Například funkce vrátí hodnotu `false`: <p>`and(greater(1,10),equals(0,0))` <p> **Parametr číslo**: 1 <p> **Název**: logická hodnota 1 <p> **Popis**: vyžaduje. první argument, který musí být Hello `true`. <p> **Parametr číslo**: 2 <p> **Název**: logická hodnota 2 <p> **Popis**: vyžaduje. musí být druhým argumentem Hello `true`.|  
|nebo|Hodnota true, pokud buď parametr vrátí hodnotu true. Oba argumenty třeba toobe logické hodnoty. Například funkce vrátí hodnotu `true`: <p>`or(greater(1,10),equals(0,0))` <p> **Parametr číslo**: 1 <p> **Název**: logická hodnota 1 <p> **Popis**: vyžaduje. první argument, který může být Hello `true`. <p> **Parametr číslo**: 2 <p> **Název**: logická hodnota 2 <p> **Popis**: vyžaduje. může být druhým argumentem Hello `true`.|  
|není|Vrátí hodnotu PRAVDA, pokud jsou parametry hello `false`. Oba argumenty třeba toobe logické hodnoty. Například funkce vrátí hodnotu `true`: <p>`not(contains('200 Success','Fail'))` <p> **Parametr číslo**: 1 <p> **Název**: logická hodnota <p> **Popis**: vrátí hodnotu true, pokud jsou parametry hello `false`. Oba argumenty třeba toobe logické hodnoty. Funkce vrátí hodnotu `true`:`not(contains('200 Success','Fail'))`|  
|Pokud|Vrátí hodnotu zadaného v závislosti na tom, zda výraz hello výsledkem `true` nebo `false`.  Například funkce vrátí hodnotu `"yes"`: <p>`if(equals(1, 1), 'yes', 'no')` <p> **Parametr číslo**: 1 <p> **Název**: výraz <p> **Popis**: vyžaduje. Logická hodnota, která určuje, které výraz hello hodnota by měla vrátit. <p> **Parametr číslo**: 2 <p> **Název**: True <p> **Popis**: vyžaduje. Hello tooreturn hodnotu, pokud výraz hello `true`. <p> **Parametr číslo**: 3 <p> **Název**: False <p> **Popis**: vyžaduje. Hello tooreturn hodnotu, pokud výraz hello `false`.|  
  
### <a name="conversion-functions"></a>Převodní funkce  

Tyto funkce jsou použité tooconvert mezi jednotlivými hello nativní typy v jazyce hello:  
  
- Řetězec  
  
- celé číslo  
  
- Plovoucí desetinná čárka  
  
- Logická hodnota  
  
- Pole  
  
- slovník  

-   Formuláře  
  
|Název funkce|Popis|  
|-------------------|-----------------|  
|celá čísla|Převeďte hello parametr tooan celé číslo. Například funkce vrátí hodnotu 100 jako číslo, nikoli řetězec: <p>`int('100')` <p> **Parametr číslo**: 1 <p> **Název**: hodnota <p> **Popis**: vyžaduje. Hello hodnotu, která je převeden tooan celé číslo.|  
|Řetězec|Převeďte řetězec tooa hello parametrů. Například funkce vrátí hodnotu `'10'`: <p>`string(10)` <p>Také můžete převést řetězec tooa objektu. Například, pokud hello `myPar` parametr je objekt s jednu vlastnost `abc : xyz`, potom funkce vrátí hodnotu `{"abc" : "xyz"}`: <p>`string(parameters('myPar'))` <p> **Parametr číslo**: 1 <p> **Název**: hodnota <p> **Popis**: vyžaduje. Hello hodnota tedy řetězec převedený tooa.|  
|JSON|Převést hodnotu typu hello parametru tooa JSON a je hello opačným z `string()`. Například funkce vrátí hodnotu `[1,2,3]` jako pole, nikoli řetězec: <p>`json('[1,2,3]')` <p>Podobně můžete převést na objekt tooan řetězec. Například funkce vrátí hodnotu `{ "abc" : "xyz" }`: <p>`json('{"abc" : "xyz"}')` <p> **Parametr číslo**: 1 <p> **Název**: řetězec <p> **Popis**: vyžaduje. Hello řetězec, který je hodnota převedená tooa nativní typu. <p>Hello `json()` funkce podporuje příliš vstup XML. Například hello hodnota parametru: <p>`<?xml version="1.0"?> <root>   <person id='1'>     <name>Alan</name>     <occupation>Engineer</occupation>   </person> </root>` <p>je převeden toothis JSON: <p>`{ "?xml": { "@version": "1.0" },   "root": {     "person": [     {       "@id": "1",       "name": "Alan",       "occupation": "Engineer"     }   ]   } }`|  
|Plovoucí desetinná čárka|Převeďte hello parametr argument tooa na dvě desetinná čísla. Například funkce vrátí hodnotu `10.333`: <p>`float('10.333')` <p> **Parametr číslo**: 1 <p> **Název**: hodnota <p> **Popis**: vyžaduje. Hello hodnota tedy převedený tooa číslo s plovoucí desetinnou čárkou.|  
|BOOL|Převeďte hello parametr tooa logická hodnota. Například funkce vrátí hodnotu `false`: <p>`bool(0)` <p> **Parametr číslo**: 1 <p> **Název**: hodnota <p> **Popis**: vyžaduje. Hello hodnotu, která je převeden tooa logická hodnota.|  
|sloučení|Vrátí první objekt nesmí být nulová hello hello argumentů předaných v. **Poznámka:**: prázdný řetězec není null. Například, pokud nejsou definovány parametry 1 a 2, funkce vrátí hodnotu `fallback`:  <p>`coalesce(parameters('parameter1'), parameters('parameter2') ,'fallback')` <p> **Parametr číslo**: 1...*n* <p> **Název**: objekt*n* <p> **Popis**: vyžaduje. Hello toocheck objekty pro hodnotu null.|  
|formátu Base64.|Vrátí hello reprezentace hello vstupní řetězec base64. Například funkce vrátí hodnotu `c29tZSBzdHJpbmc=`: <p>`base64('some string')` <p> **Parametr číslo**: 1 <p> **Název**: řetězec 1 <p> **Popis**: vyžaduje. řetězec tooencode Hello do reprezentace base64.|  
|base64ToBinary|Vrátí binární reprezentace řetězec s kódováním base64. Například funkce vrátí hodnotu hello binární reprezentace `some string`: <p>`base64ToBinary('c29tZSBzdHJpbmc=')` <p> **Parametr číslo**: 1 <p> **Název**: řetězec <p> **Popis**: vyžaduje. řetězec kódovaný Hello base64.|  
|base64ToString|Vrátí řetězcovou reprezentaci based64 kódovaný řetězec. Například funkce vrátí hodnotu `some string`: <p>`base64ToString('c29tZSBzdHJpbmc=')` <p> **Parametr číslo**: 1 <p> **Název**: řetězec <p> **Popis**: vyžaduje. řetězec kódovaný Hello base64.|  
|Binární|Vrátí binární reprezentace hodnoty.  Například funkce vrátí hodnotu binární reprezentace `some string`: <p>`binary('some string')` <p> **Parametr číslo**: 1 <p> **Název**: hodnota <p> **Popis**: vyžaduje. Hello hodnotu, která je převeden toobinary.|  
|dataUriToBinary|Vrátí binární reprezentace dat identifikátor URI. Například funkce vrátí hodnotu hello binární reprezentace `some string`: <p>`dataUriToBinary('data:;base64,c29tZSBzdHJpbmc=')` <p> **Parametr číslo**: 1 <p> **Název**: řetězec <p> **Popis**: vyžaduje. data Hello reprezentace toobinary tooconvert identifikátoru URI.|  
|dataUriToString|Vrátí řetězcovou reprezentaci dat identifikátor URI. Například funkce vrátí hodnotu `some string`: <p>`dataUriToString('data:;base64,c29tZSBzdHJpbmc=')` <p> **Parametr číslo**: 1 <p> **Název**: řetězec<p> **Popis**: vyžaduje. data Hello reprezentace tooString tooconvert identifikátoru URI.|  
|dataUri|Vrací data URI hodnoty. Například funkce vrátí hodnotu tento identifikátor URI dat. `text/plain;charset=utf8;base64,c29tZSBzdHJpbmc=`: <p>`dataUri('some string')` <p> **Parametr číslo**: 1<p> **Název**: hodnota<p> **Popis**: vyžaduje. Hello hodnotu tooconvert toodata identifikátor URI.|  
|decodeBase64|Vrátí řetězcovou reprezentaci řetězce vstupní based64. Například funkce vrátí hodnotu `some string`: <p>`decodeBase64('c29tZSBzdHJpbmc=')` <p> **Parametr číslo**: 1 <p> **Název**: řetězec <p> **Popis**: vrátí řetězcovou reprezentaci řetězce vstupní based64.|  
|encodeuricomponent –|URL – řídicí sekvence hello řetězec, který se předává v. Například funkce vrátí hodnotu `You+Are%3ACool%2FAwesome`: <p>`encodeUriComponent('You Are:Cool/Awesome')` <p> **Parametr číslo**: 1 <p> **Název**: řetězec <p> **Popis**: vyžaduje. Hello řetězec tooescape URL unsafe znaky z.|  
|decodeuricomponent –|Zrušení – adresa URL – řídicí sekvence hello řetězec, který se předává v. Například funkce vrátí hodnotu `You Are:Cool/Awesome`: <p>`encodeUriComponent('You+Are%3ACool%2FAwesome')` <p> **Parametr číslo**: 1 <p> **Název**: řetězec <p> **Popis**: vyžaduje. Hello řetězec toodecode hello URL unsafe znaky z.|  
|decodeDataUri|Vrátí binární reprezentace vstupní data řetězce URI. Například funkce vrátí hodnotu hello binární reprezentace `some string`: <p>`decodeDataUri('data:;base64,c29tZSBzdHJpbmc=')` <p> **Parametr číslo**: 1 <p> **Název**: řetězec <p> **Popis**: vyžaduje. Hello dataURI toodecode do binární reprezentace.|  
|uriComponent|Vrátí identifikátor URI kódovaný reprezentaci hodnoty. Například funkce vrátí hodnotu `You+Are%3ACool%2FAwesome`: <p>`uriComponent('You Are:Cool/Awesome')` <p> **Parametr číslo**: 1<p> **Název**: řetězec <p> **Popis**: vyžaduje. toobe Hello řetězec kódovaný identifikátor URI.|  
|uriComponentToBinary|Vrátí že řetězec kódovaný binární reprezentace identifikátoru URI. Například funkce vrátí hodnotu binární reprezentace `You Are:Cool/Awesome`: <p>`uriComponentToBinary('You+Are%3ACool%2FAwesome')` <p> **Parametr číslo**: 1 <p> **Název**: řetězec<p> **Popis**: vyžaduje. řetězec kódovaný Hello identifikátor URI.|  
|uriComponentToString|Vrátí že řetězec kódovaný řetězcová reprezentace identifikátoru URI. Například funkce vrátí hodnotu `You Are:Cool/Awesome`: <p>`uriComponentToBinary('You+Are%3ACool%2FAwesome')` <p> **Parametr číslo**: 1<p> **Název**: řetězec<p> **Popis**: vyžaduje. řetězec kódovaný Hello identifikátor URI.|  
|xml|Vrátí reprezentaci XML hello hodnoty. Například funkce vrátí hodnotu XML obsahu reprezentována `'\<name>Alan\</name>'`: <p>`xml('\<name>Alan\</name>')` <p>Hello `xml()` funkce podporuje vstup příliš objekt JSON. Například hello parametr `{ "abc": "xyz" }` je převeden tooXML obsahu:`\<abc>xyz\</abc>` <p> **Parametr číslo**: 1<p> **Název**: hodnota<p> **Popis**: vyžaduje. tooXML tooconvert hodnotu Hello.|  
|výraz XPath|Vrátí pole z uzlů XML odpovídající výraz xpath hello hodnotu, která vyhodnotí jako výraz xpath hello. <p> **Příklad 1** <p>Předpokládejme hello hodnota parametru `p1` je řetězcová reprezentace tohoto kódu XML: <p>`<?xml version="1.0"?> <lab>   <robot>     <parts>5</parts>     <name>R1</name>   </robot>   <robot>     <parts>8</parts>     <name>R2</name>   </robot> </lab>` <p>Tento kód:`xpath(xml(parameters('p1'), '/lab/robot/name')` <p>Vrátí <p>`[ <name>R1</name>, <name>R2</name> ]` <p>Při tomto kódu: <p>`xpath(xml(parameters('p1'), ' sum(/lab/robot/parts)')` <p>Vrátí <p>`13` <p> <p> **Příklad 2** <p>Daný hello následující obsah XML: <p>`<?xml version="1.0"?> <File xmlns="http://foo.com">   <Location>bar</Location> </File>` <p>Tento kód:`@xpath(xml(body('Http')), '/*[name()=\"File\"]/*[name()=\"Location\"]')` <p>nebo tento kód: <p>`@xpath(xml(body('Http')), '/*[local-name()=\"File\" and namespace-uri()=\"http://foo.com\"]/*[local-name()=\"Location\" and namespace-uri()=\"\"]')` <p>Vrátí <p>`<Location xmlns="http://abc.com">xyz</Location>` <p>A tento kód:`@xpath(xml(body('Http')), 'string(/*[name()=\"File\"]/*[name()=\"Location\"])')` <p>Vrátí <p>``xyz`` <p> **Parametr číslo**: 1 <p> **Název**: Xml <p> **Popis**: vyžaduje. Hello XML, na které tooevaluate hello XPath výraz. <p> **Parametr číslo**: 2 <p> **Název**: XPath <p> **Popis**: vyžaduje. Hello tooevaluate výraz XPath.|  
|Pole|Převeďte hello parametr tooan pole. Například funkce vrátí hodnotu `["abc"]`: <p>`array('abc')` <p> **Parametr číslo**: 1 <p> **Název**: hodnota <p> **Popis**: vyžaduje. Hello hodnotu, která je převeden tooan pole.|
|createArray|Vytvoří pole z hello parametry. Například funkce vrátí hodnotu `["a", "c"]`: <p>`createArray('a', 'c')` <p> **Parametr číslo**: 1...*n* <p> **Název**: žádné*n* <p> **Popis**: vyžaduje. Hello toocombine hodnoty do pole.|
|triggerFormDataValue|Vrátí jednu hodnotu odpovídající název klíče hello z dat formuláře nebo výstupní formulářem kódované aktivační události.  Pokud máte více odpovídá se bude chyba.  Například následující hello vrátí `bar`:`triggerFormDataValue('foo')`<br /><br />**Parametr číslo**: 1<br /><br />**Název**: název klíče<br /><br />**Popis**: vyžaduje. Název klíče Hello z tooreturn hodnota data formuláře hello.|
|triggerFormDataMultiValues|Vrátí pole hodnot odpovídající název klíče hello z dat formuláře nebo výstupní formulářem kódované aktivační události.  Například následující hello vrátí `["bar"]`:`triggerFormDataValue('foo')`<br /><br />**Parametr číslo**: 1<br /><br />**Název**: název klíče<br /><br />**Popis**: vyžaduje. Název klíče Hello dat formuláře hello hodnoty tooreturn.|
|triggerMultipartBody|Vrátí hello textu pro část v s více částmi výstup hello aktivační události.<br /><br />**Parametr číslo**: 1<br /><br />**Název**: Index<br /><br />**Popis**: vyžaduje. index Hello tooretrieve část hello.|
|formDataValue|Vrátí jednu hodnotu odpovídající název klíče hello z dat formuláře nebo výstup formulářem kódované akce.  Pokud máte více odpovídá se bude chyba.  Například následující hello vrátí `bar`:`formDataValue('someAction', 'foo')`<br /><br />**Parametr číslo**: 1<br /><br />**Název**: název akce<br /><br />**Popis**: vyžaduje. Název Hello hello akce s dat formuláře nebo kódovaná formuláře odpovědi.<br /><br />**Parametr číslo**: 2<br /><br />**Název**: název klíče<br /><br />**Popis**: vyžaduje. Název klíče Hello z tooreturn hodnota data formuláře hello.|
|formDataMultiValues|Vrátí pole hodnot odpovídající název klíče hello z dat formuláře nebo výstup formulářem kódované akce.  Například následující hello vrátí `["bar"]`:`formDataMultiValues('someAction', 'foo')`<br /><br />**Parametr číslo**: 1<br /><br />**Název**: název akce<br /><br />**Popis**: vyžaduje. Název Hello hello akce s dat formuláře nebo kódovaná formuláře odpovědi.<br /><br />**Parametr číslo**: 2<br /><br />**Název**: název klíče<br /><br />**Popis**: vyžaduje. Název klíče Hello dat formuláře hello hodnoty tooreturn.|
|multipartBody|Vrátí hello textu pro část ve vícedílné zprávy výstupu akce.<br /><br />**Parametr číslo**: 1<br /><br />**Název**: název akce<br /><br />**Popis**: vyžaduje. Název Hello hello akce s vícedílné zprávy odpovědi.<br /><br />**Parametr číslo**: 2<br /><br />**Název**: Index<br /><br />**Popis**: vyžaduje. index Hello tooretrieve část hello.|

### <a name="math-functions"></a>Matematické funkce  

Tyto funkce lze použít pro buď typy čísel: **celá čísla** a **jako plovoucí**.  
  
|Název funkce|Popis|  
|-------------------|-----------------|  
|Přidat|Vrátí výsledek hello v přidávání hello dvou čísel. Například funkce vrátí hodnotu `20.333`: <p>`add(10,10.333)` <p> **Parametr číslo**: 1 <p> **Název**: Summand 1 <p> **Popis**: vyžaduje. Hello číslo tooadd příliš**Summand 2**. <p> **Parametr číslo**: 2 <p> **Název**: Summand 2 <p> **Popis**: vyžaduje. Hello číslo tooadd příliš**Summand 1**.|  
|Sub –|Vrátí výsledek hello z odečtením dvou čísel. Například funkce vrátí hodnotu `-0.333`: <p>`sub(10,10.333)` <p> **Parametr číslo**: 1 <p> **Název**: Minuend <p> **Popis**: vyžaduje. Hello číslo, které **Subtrahend** se odebere z. <p> **Parametr číslo**: 2 <p> **Název**: Subtrahend <p> **Popis**: vyžaduje. Hello číslo tooremove z hello **Minuend**.|  
|mul|Vrátí výsledek hello z vynásobením hello dvou čísel. Například funkce vrátí hodnotu `103.33`: <p>`mul(10,10.333)` <p> **Parametr číslo**: 1 <p> **Název**: násobenec 1 <p> **Popis**: vyžaduje. Hello číslo toomultiply **násobenec 2** s. <p> **Parametr číslo**: 2 <p> **Název**: násobenec 2 <p> **Popis**: vyžaduje. Hello číslo toomultiply **násobenec 1** s.|  
|div|Vrátí výsledek hello dělení dvou čísel hello. Například funkce vrátí hodnotu `1.0333`: <p>`div(10.333,10)` <p> **Parametr číslo**: 1 <p> **Název**: dělenec <p> **Popis**: vyžaduje. Hello číslo toodivide podle hello **dělitel**. <p> **Parametr číslo**: 2 <p> **Název**: dělitel <p> **Popis**: vyžaduje. Hello číslo toodivide hello **dělenec** podle.|  
|MOD|Vrátí hello zbytek po dělení dvou čísel hello (modulo). Například funkce vrátí hodnotu `2`: <p>`mod(10,4)` <p> **Parametr číslo**: 1 <p> **Název**: dělenec <p> **Popis**: vyžaduje. Hello číslo toodivide podle hello **dělitel**. <p> **Parametr číslo**: 2 <p> **Název**: dělitel <p> **Popis**: vyžaduje. Hello číslo toodivide hello **dělenec** podle. Po dělení hello je převzat hello zbytek.|  
|min|Existují dva různé vzorce pro volání této funkce. <p>Zde `min` přijímá pole, a vrátí hello funkce `0`: <p>`min([0,1,2])` <p>Alternativně může tuto funkci trvat seznamu hodnoty a také vrátí `0`: <p>`min(0,1,2)` <p> **Poznámka:**: všechny hodnoty musí být číslo, takže pokud hello parametr pole, pole hello má mít tooonly čísla. <p> **Parametr číslo**: 1 <p> **Název**: kolekce nebo hodnota <p> **Popis**: vyžaduje. Buď pole hodnoty toofind hello minimální hodnota nebo hello první hodnotu sady. <p> **Parametr číslo**: 2...*n* <p> **Název**: hodnota*n* <p> **Popis**: volitelné. Pokud je první parametr hello hodnotu, pak můžete předat další hodnoty a hello minimum všech hodnot předaný se vrátí.|  
|maximální počet|Existují dva různé vzorce pro volání této funkce. <p>Zde `max` přijímá pole, a vrátí hello funkce `2`: <p>`max([0,1,2])` <p>Alternativně může tuto funkci trvat seznamu hodnoty a také vrátí `2`: <p>`max(0,1,2)` <p> **Poznámka:**: všechny hodnoty musí být číslo, takže pokud hello parametr pole, pole hello má mít tooonly čísla. <p> **Parametr číslo**: 1 <p> **Název**: kolekce nebo hodnota <p> **Popis**: vyžaduje. Buď pole hodnoty toofind hello maximální hodnotu, nebo první hodnota hello sady. <p> **Parametr číslo**: 2...*n* <p> **Název**: hodnota*n* <p> **Popis**: volitelné. Pokud je první parametr hello hodnotu, pak můžete předat další hodnoty a hello maximum všech hodnot předaný se vrátí.|  
|rozsah|Generuje pole celá čísla od určitého počtu. Můžete definovat hello délka hello vrátila pole. <p>Například funkce vrátí hodnotu `[3,4,5,6]`: <p>`range(3,4)` <p> **Parametr číslo**: 1 <p> **Název**: počáteční index <p> **Popis**: vyžaduje. Hello první celé číslo v poli hello. <p> **Parametr číslo**: 2 <p> **Název**: počet <p> **Popis**: vyžaduje. Tato hodnota je číslo hello celých čísel, která je v poli hello.|  
|rand –|Generuje náhodné číslo v rámci hello zadaný rozsahu (včetně pouze na konci prvního). Tato funkce může vrátit například buď `0` nebo '1': <p>`rand(0,2)` <p> **Parametr číslo**: 1 <p> **Název**: minimální <p> **Popis**: vyžaduje. Hello nejnižší celé číslo, které mohou být vráceny. <p> **Parametr číslo**: 2 <p> **Název**: maximální <p> **Popis**: vyžaduje. Tato hodnota je celé číslo další hello po nejvyšší celé číslo hello, který může být vrácen.|  
  
### <a name="date-functions"></a>Datové funkce  
  
|Název funkce|Popis|  
|-------------------|-----------------|  
|utcnow|Vrátí hello aktuální časové razítko jako řetězec, například: `2017-03-15T13:27:36Z`: <p>`utcnow()` <p> **Parametr číslo**: 1 <p> **Název**: formát <p> **Popis**: volitelné. Buď [jeden znak specifikátor formátu](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) nebo [vlastní formát vzor](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) určující, jak tooformat hello hodnotu této časové razítko. Pokud formát není k dispozici, použije se formát hello ISO 8601 ("o").|  
|přidat_sekundy|Přidá celočíselný počet sekund tooa řetězec časové razítko předaná. Hello počet sekund může být kladná nebo záporná. Ve výchozím nastavení hello výsledkem je, řetězec ve formátu ISO 8601 formátu ("o"), pokud je k dispozici specifikátor formátu. Příklad: `2015-03-15T13:27:00Z`: <p>`addseconds('2015-03-15T13:27:36Z', -36)` <p> **Parametr číslo**: 1 <p> **Název**: časové razítko <p> **Popis**: vyžaduje. Řetězec, který obsahuje dobu hello. <p> **Parametr číslo**: 2 <p> **Název**: sekund <p> **Popis**: vyžaduje. Hello počet sekund tooadd. Může být záporné toosubtract sekund. <p> **Parametr číslo**: 3 <p> **Název**: formát <p> **Popis**: volitelné. Buď [jeden znak specifikátor formátu](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) nebo [vlastní formát vzor](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) určující, jak tooformat hello hodnotu této časové razítko. Pokud formát není k dispozici, použije se formát hello ISO 8601 ("o").|  
|addminutes|Přidá celočíselný počet minut tooa řetězec časové razítko předaná. Hello počet minut, může být kladná nebo záporná. Ve výchozím nastavení hello výsledkem je, řetězec ve formátu ISO 8601 formátu ("o"), pokud je k dispozici specifikátor formátu. Příklad: `2015-03-15T14:00:36Z`: <p>`addminutes('2015-03-15T13:27:36Z', 33)` <p> **Parametr číslo**: 1 <p> **Název**: časové razítko <p> **Popis**: vyžaduje. Řetězec, který obsahuje dobu hello. <p> **Parametr číslo**: 2 <p> **Název**: minut <p> **Popis**: vyžaduje. Hello počet minut tooadd. Může být záporné toosubtract minut. <p> **Parametr číslo**: 3 <p> **Název**: formát <p> **Popis**: volitelné. Buď [jeden znak specifikátor formátu](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) nebo [vlastní formát vzor](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) určující, jak tooformat hello hodnotu této časové razítko. Pokud formát není k dispozici, použije se formát hello ISO 8601 ("o").|  
|addhours|Přidá celočíselný počet hodin tooa řetězec časové razítko předaná. Hello počet hodin, může být kladná nebo záporná. Ve výchozím nastavení hello výsledkem je, řetězec ve formátu ISO 8601 formátu ("o"), pokud je k dispozici specifikátor formátu. Příklad: `2015-03-16T01:27:36Z`: <p>`addhours('2015-03-15T13:27:36Z', 12)` <p> **Parametr číslo**: 1 <p> **Název**: časové razítko <p> **Popis**: vyžaduje. Řetězec, který obsahuje dobu hello. <p> **Parametr číslo**: 2 <p> **Název**: čas <p> **Popis**: vyžaduje. Hello počet hodin tooadd. Může být záporné toosubtract hodin. <p> **Parametr číslo**: 3 <p> **Název**: formát <p> **Popis**: volitelné. Buď [jeden znak specifikátor formátu](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) nebo [vlastní formát vzor](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) určující, jak tooformat hello hodnotu této časové razítko. Pokud formát není k dispozici, použije se formát hello ISO 8601 ("o").|  
|přidat_dny|Přidá celočíselný počet dní tooa řetězec časové razítko předaná. Hello počet dní, může být kladná nebo záporná. Ve výchozím nastavení hello výsledkem je, řetězec ve formátu ISO 8601 formátu ("o"), pokud je k dispozici specifikátor formátu. Příklad: `2015-02-23T13:27:36Z`: <p>`addseconds('2015-03-15T13:27:36Z', -20)` <p> **Parametr číslo**: 1 <p> **Název**: časové razítko <p> **Popis**: vyžaduje. Řetězec, který obsahuje dobu hello. <p> **Parametr číslo**: 2 <p> **Název**: počet dnů <p> **Popis**: vyžaduje. Hello počet dní tooadd. Může být záporné toosubtract dnů. <p> **Parametr číslo**: 3 <p> **Název**: formát <p> **Popis**: volitelné. Buď [jeden znak specifikátor formátu](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) nebo [vlastní formát vzor](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) určující, jak tooformat hello hodnotu této časové razítko. Pokud formát není k dispozici, použije se formát hello ISO 8601 ("o").|  
|FormatDateTime|Vrací řetězec ve formátu data. Ve výchozím nastavení hello výsledkem je, řetězec ve formátu ISO 8601 formátu ("o"), pokud je k dispozici specifikátor formátu. Příklad: `2015-02-23T13:27:36Z`: <p>`formatDateTime('2015-03-15T13:27:36Z', 'o')` <p> **Parametr číslo**: 1 <p> **Název**: datum <p> **Popis**: vyžaduje. Řetězec, který obsahuje data hello. <p> **Parametr číslo**: 2 <p> **Název**: formát <p> **Popis**: volitelné. Buď [jeden znak specifikátor formátu](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) nebo [vlastní formát vzor](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) určující, jak tooformat hello hodnotu této časové razítko. Pokud formát není k dispozici, použije se formát hello ISO 8601 ("o").|  
|startOfHour|Vrátí hello začátek hello hodinu tooa řetězec časové razítko předaná. Například `2017-03-15T13:00:00Z`:<br /><br /> `startOfHour('2017-03-15T13:27:36Z')`<br /><br /> **Parametr číslo**: 1<br /><br /> **Název**: časové razítko<br /><br /> **Popis**: vyžaduje. Toto je řetězec, který obsahuje hello čas.<br /><br />**Parametr číslo**: 2<br /><br /> **Název**: formát<br /><br /> **Popis**: volitelné. Buď [jeden znak specifikátor formátu](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) nebo [vlastní formát vzor](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) určující, jak tooformat hello hodnotu této časové razítko. Pokud formát není k dispozici, použije se formát hello ISO 8601 ("o").|  
|startOfDay|Vrátí hello začátek hello den tooa řetězec časové razítko předaná. Například `2017-03-15T00:00:00Z`:<br /><br /> `startOfDay('2017-03-15T13:27:36Z')`<br /><br /> **Parametr číslo**: 1<br /><br /> **Název**: časové razítko<br /><br /> **Popis**: vyžaduje. Toto je řetězec, který obsahuje hello čas.<br /><br />**Parametr číslo**: 2<br /><br /> **Název**: formát<br /><br /> **Popis**: volitelné. Buď [jeden znak specifikátor formátu](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) nebo [vlastní formát vzor](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) určující, jak tooformat hello hodnotu této časové razítko. Pokud formát není k dispozici, použije se formát hello ISO 8601 ("o").| 
|startOfMonth|Vrátí hello začátek hello měsíc tooa řetězec časové razítko předaná. Například `2017-03-01T00:00:00Z`:<br /><br /> `startOfMonth('2017-03-15T13:27:36Z')`<br /><br /> **Parametr číslo**: 1<br /><br /> **Název**: časové razítko<br /><br /> **Popis**: vyžaduje. Toto je řetězec, který obsahuje hello čas.<br /><br />**Parametr číslo**: 2<br /><br /> **Název**: formát<br /><br /> **Popis**: volitelné. Buď [jeden znak specifikátor formátu](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) nebo [vlastní formát vzor](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) určující, jak tooformat hello hodnotu této časové razítko. Pokud formát není k dispozici, použije se formát hello ISO 8601 ("o").| 
|DayOfWeek|Vrátí hello den v týdnu součást časové razítko řetězec.  Neděle je 0, pondělí je 1 a tak dále. Například `3`:<br /><br /> `dayOfWeek('2017-03-15T13:27:36Z')`<br /><br /> **Parametr číslo**: 1<br /><br /> **Název**: časové razítko<br /><br /> **Popis**: vyžaduje. Toto je řetězec, který obsahuje hello čas.| 
|DayOfMonth|Vrátí hello den měsíce součást časové razítko řetězec. Například `15`:<br /><br /> `dayOfMonth('2017-03-15T13:27:36Z')`<br /><br /> **Parametr číslo**: 1<br /><br /> **Název**: časové razítko<br /><br /> **Popis**: vyžaduje. Toto je řetězec, který obsahuje hello čas.| 
|DayOfYear|Vrátí hello den roku součást časové razítko řetězec. Například `74`:<br /><br /> `dayOfYear('2017-03-15T13:27:36Z')`<br /><br /> **Parametr číslo**: 1<br /><br /> **Název**: časové razítko<br /><br /> **Popis**: vyžaduje. Toto je řetězec, který obsahuje hello čas.| 
|rysky|Vrátí hello značek vlastnost časového razítka řetězec. Například `1489603019`:<br /><br /> `ticks('2017-03-15T18:36:59Z')`<br /><br /> **Parametr číslo**: 1<br /><br /> **Název**: časové razítko<br /><br /> **Popis**: vyžaduje. Toto je řetězec, který obsahuje hello čas.| 
  
### <a name="workflow-functions"></a>Funkce pracovního postupu  

Tyto funkce umožňují získat informace o samotného pracovního postupu hello za běhu.  
  
|Název funkce|Popis|  
|-------------------|-----------------|  
|listCallbackUrl|Vrací řetězec toocall tooinvoke hello aktivační události nebo akce. <p> **Poznámka:**: tuto funkci lze použít pouze v **httpWebhook** a **apiConnectionWebhook**není v **ruční**, **opakování**, **http**, nebo **apiConnection**. <p>Například hello `listCallbackUrl()` funkce vrátí: <p>`https://prod-01.westus.logic.azure.com:443/workflows/1235...ABCD/triggers/manual/run?api-version=2015-08-01-preview&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=xxx...xxx` |  
|pracovní postup|Tato funkce poskytuje všechny podrobnosti hello hello samotného pracovního postupu za běhu hello. <p> Jsou k dispozici vlastnosti objektu hello pracovního postupu: <ul><li>`name`</li><li>`type`</li><li>`id`</li><li>`location`</li><li>`run`</li></ul> <p> Hello hodnotu hello `run` vlastnost je objekt s následujícími vlastnostmi: <ul><li>`name`</li><li>`type`</li><li>`id`</li></ul> <p>V tématu hello [Rest API](http://go.microsoft.com/fwlink/p/?LinkID=525617) podrobnosti o tyto vlastnosti.<p> Například tooget hello název hello aktuální spustit, použijte hello `"@workflow().run.name"` výraz. |

## <a name="next-steps"></a>Další kroky

[Triggery a akce pracovních postupů](logic-apps-workflow-actions-triggers.md)
