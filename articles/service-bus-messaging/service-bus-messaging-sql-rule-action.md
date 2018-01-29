---
title: Reference syntaxe SQLRuleAction v Azure | Microsoft Docs
description: Podrobnosti o SQLRuleAction gramatika.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/07/2017
ms.author: sethm
ms.openlocfilehash: 83b4f76a171cd8a860e7ab43462c976bf4df941a
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/07/2017
---
# <a name="sqlruleaction-syntax"></a>Syntaxe SQLRuleAction

A *SqlRuleAction* je instance [SqlRuleAction](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction) třídy a představuje sadu akcí, které jsou napsané v jazyce SQL na základě syntaxi, která bude provedena [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).   
  
V tomto článku jsou uvedeny podrobnosti o gramatiku SQL pravidlo akce.  
  
```  
<statements> ::=
    <statement> [, ...n]  
  
```  
  
```  
<statement> ::=
    <action> [;]
    Remarks
    -------
    Semicolon is optional.  
  
```  
  
```  
<action> ::=
    SET <property> = <expression>
    REMOVE <property>  
```

```
<expression> ::=
    <constant>
    | <function>
    | <property>
    | <expression> { + | - | * | / | % } <expression>
    | { + | - } <expression>
    | ( <expression> )
``` 

```
<property> := 
    [<scope> .] <property_name>
``` 
  
## <a name="arguments"></a>Argumenty  
  
-   `<scope>`je volitelný řetězec označující oboru `<property_name>`. Platné hodnoty jsou `sys` nebo `user`. `sys` Hodnota označuje rozsah systémů kde `<property_name>` je název veřejné vlastnosti [BrokeredMessage třída](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage). `user`označuje oboru uživatele kde `<property_name>` je klíč z [BrokeredMessage třída](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) slovníku. `user`rozsah je výchozí obor, pokud `<scope>` není zadán.  
  
### <a name="remarks"></a>Poznámky  

Pokus o přístup k systému neexistující vlastnost je k chybě při pokusu o přístup k neexistující uživatele vlastnost není chyba. Místo toho vlastnost uživatele neexistující interně vyhodnotí jako neznámou hodnotou. Neznámá hodnota považuje speciálně během vyhodnocení operátor.  
  
## <a name="propertyname"></a>%{Property_Name/  
  
```  
<property_name> ::=  
     <identifier>  
     | <delimited_identifier>  
  
<identifier> ::=  
     <regular_identifier> | <quoted_identifier> | <delimited_identifier>  
  
```  
  
### <a name="arguments"></a>Argumenty  
 `<regular_identifier>`řetězec reprezentována následujícímu regulárnímu výrazu:  
  
```  
[[:IsLetter:]][_[:IsLetter:][:IsDigit:]]*  
```  
  
 To znamená libovolný řetězec, který začíná písmenem a následuje jeden nebo více podtržítko nebo písmeno nebo číslice.  
  
 `[:IsLetter:]`znamená libovolný znak Unicode, který je zařazený do kategorie jako písmeno kódování Unicode. `System.Char.IsLetter(c)`Vrátí `true` Pokud `c` písmeno kódování Unicode.  
  
 `[:IsDigit:]`znamená libovolný znak Unicode, který je zařazený do kategorie jako desítková číslice. `System.Char.IsDigit(c)`Vrátí `true` Pokud `c` je číslice kódování Unicode.  
  
 A `<regular_identifier>` nemůže být rezervované klíčové slovo.  
  
 `<delimited_identifier>`je řetězec, který je uzavřena s levé nebo pravé hranaté závorky ([]). Pravou hranatou závorku je reprezentován jako dvě pravé hranaté závorky. Následují příklady `<delimited_identifier>`:  
  
```  
[Property With Space]  
[HR-EmployeeID]  
  
```  
  
 `<quoted_identifier>`je řetězec, který je uzavřena dvojitých uvozovek nahoře. Dvojité uvozovky v identifikátoru je reprezentován jako dva znaky uvozovek. Není doporučeno použít identifikátory v uvozovkách, protože můžete snadno Nezaměňovat s řetězcová konstanta. Pokud je to možné použijte s oddělovači identifikátor. Tady je příklad `<quoted_identifier>`:  
  
```  
"Contoso & Northwind"  
```  
  
## <a name="pattern"></a>vzor  
  
```  
<pattern> ::=  
      <expression>  
```  
  
### <a name="remarks"></a>Poznámky
  
 `<pattern>`musí být výraz, který se vyhodnotí jako řetězec. Použije se jako vzor pro operátor LIKE.      Může obsahovat následující znaky:  
  
-   `%`: Řetězec nula nebo více znaků.  
  
-   `_`: Jakémukoliv jedinému znaku.  
  
## <a name="escapechar"></a>escape_char  
  
```  
<escape_char> ::=  
      <expression>  
```  
  
### <a name="remarks"></a>Poznámky
  
 `<escape_char>`musí být výraz, který se vyhodnotí jako řetězec o délce 1. Slouží jako řídicí znak pro operátor LIKE.  
  
 Například `property LIKE 'ABC\%' ESCAPE '\'` odpovídá `ABC%` místo řetězec, který začíná `ABC`.  
  
## <a name="constant"></a>Konstantní  
  
```  
<constant> ::=  
      <integer_constant> | <decimal_constant> | <approximate_number_constant> | <boolean_constant> | NULL  
```  
  
### <a name="arguments"></a>Argumenty  
  
-   `<integer_constant>`je řetězec čísel, která se nenacházejí v uvozovkách a neobsahují desetinných míst. Hodnoty jsou uloženy jako `System.Int64` interně a postupujte podle stejného rozsahu.  
  
     Následují příklady dlouho konstant:  
  
    ```  
    1894  
    2  
    ```  
  
-   `<decimal_constant>`je řetězec čísel, která se nenacházejí v uvozovkách a obsahovat desetinné čárky. Hodnoty jsou uloženy jako `System.Double` interně a postupujte podle stejné rozsah nebo přesnosti.  
  
     V budoucí verzi, může být tento číslo uložené v na jiný datový typ pro podporu přesné číslo sémantiku, takže byste neměli spoléhat na skutečnost, základní datový typ je `System.Double` pro `<decimal_constant>`.  
  
     Následují příklady decimal konstant:  
  
    ```  
    1894.1204  
    2.0  
    ```  
  
-   `<approximate_number_constant>`je číslo napsaných v exponenciální notace. Hodnoty jsou uloženy jako `System.Double` interně a postupujte podle stejné rozsah nebo přesnosti. Následují příklady přibližnou číselné konstanty:  
  
    ```  
    101.5E5  
    0.5E-2  
    ```  
  
## <a name="booleanconstant"></a>boolean_constant  
  
```  
<boolean_constant> :=  
      TRUE | FALSE  
```  
  
### <a name="remarks"></a>Poznámky
  
Logická hodnota konstanty jsou reprezentované pomocí klíčová slova `TRUE` nebo `FALSE`. Hodnoty jsou uloženy jako `System.Boolean`.  
  
## <a name="stringconstant"></a>string_constant  
  
```  
<string_constant>  
```  
  
### <a name="remarks"></a>Poznámky
  
Řetězcové konstanty jsou uzavřené v jednoduchých uvozovkách a zahrnují všechny platné znaky kódování Unicode. Jednoduché uvozovky, vložené do řetězcová konstanta je reprezentována jako dvě jednoduché uvozovky.  
  
## <a name="function"></a>Funkce  
  
```  
<function> :=  
      newid() |  
      property(name) | p(name)  
```  
  
### <a name="remarks"></a>Poznámky  

`newid()` Funkce vrátí **System.Guid** vygenerované `System.Guid.NewGuid()` metoda.  
  
`property(name)` Funkce vrátí hodnotu vlastnosti odkazuje `name`. `name` Hodnota může být libovolný platný výraz, který vrací řetězcovou hodnotu.  
  
## <a name="considerations"></a>Požadavky

- Sada se používá k vytvoření nové vlastnosti nebo aktualizujte hodnotu existující vlastnost.
- ODEBRAT slouží k odebrání vlastnosti.
- Pokud typ výrazu a existující typ vlastnosti se liší, provede sadu implicitní převod Pokud je to možné.
- Akce selže, pokud odkazované vlastnosti neexistující systému.
- Akce neselže, pokud byly na něj odkazovalo vlastnosti neexistující uživatele.
- Vlastnost uživatele neexistující vyhodnotí jako "Neznámý" interně následující stejnou sémantiku jako [SQLFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) při vyhodnocování operátory.

## <a name="next-steps"></a>Další kroky

- [SQLRuleAction – třída](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction)
- [SQLFilter – třída](/dotnet/api/microsoft.servicebus.messaging.sqlfilter)
