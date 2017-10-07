---
title: reference syntaxe aaaSQLRuleAction v Azure | Microsoft Docs
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
ms.date: 06/28/2017
ms.author: sethm
ms.openlocfilehash: 8ef281f942847bcc535b83a5ffb30d03539734f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sqlruleaction-syntax"></a>Syntaxe SQLRuleAction

A *SqlRuleAction* je instance hello [SqlRuleAction](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction) třídy a představuje sadu akcí, které jsou napsané v jazyce SQL na základě syntaxi, která bude provedena [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).   
  
Toto téma obsahuje podrobnosti o hello gramatiky akce pravidla SQL.  
  
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
  
-   `<scope>`je volitelný řetězec označující hello oboru hello `<property_name>`. Platné hodnoty jsou `sys` nebo `user`. Hello `sys` hodnota určuje rozsah systémů kde `<property_name>` je název veřejné vlastnosti hello [BrokeredMessage třída](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage). `user`označuje oboru uživatele kde `<property_name>` je klíč hello [BrokeredMessage třída](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) slovníku. `user`rozsah je výchozí obor hello, pokud `<scope>` není zadán.  
  
### <a name="remarks"></a>Poznámky  

Tooaccess pokusu o systému neexistující vlastnost je k chybě při vlastnost uživatele pokusu o tooaccess neexistující není chyba. Místo toho vlastnost uživatele neexistující interně vyhodnotí jako neznámou hodnotou. Neznámá hodnota považuje speciálně během vyhodnocení operátor.  
  
## <a name="propertyname"></a>%{Property_Name/  
  
```  
<property_name> ::=  
     <identifier>  
     | <delimited_identifier>  
  
<identifier> ::=  
     <regular_identifier> | <quoted_identifier> | <delimited_identifier>  
  
```  
  
### <a name="arguments"></a>Argumenty  
 `<regular_identifier>`je řetězec reprezentována hello následující regulární výraz:  
  
```  
[[:IsLetter:]][_[:IsLetter:][:IsDigit:]]*  
```  
  
 To znamená libovolný řetězec, který začíná písmenem a následuje jeden nebo více podtržítko nebo písmeno nebo číslice.  
  
 `[:IsLetter:]`znamená libovolný znak Unicode, který je zařazený do kategorie jako písmeno kódování Unicode. `System.Char.IsLetter(c)`Vrátí `true` Pokud `c` písmeno kódování Unicode.  
  
 `[:IsDigit:]`znamená libovolný znak Unicode, který je zařazený do kategorie jako desítková číslice. `System.Char.IsDigit(c)`Vrátí `true` Pokud `c` je číslice kódování Unicode.  
  
 A `<regular_identifier>` nemůže být rezervované klíčové slovo.  
  
 `<delimited_identifier>`je řetězec, který je uzavřena s levé nebo pravé hranaté závorky ([]). Pravou hranatou závorku je reprezentován jako dvě pravé hranaté závorky. Hello Následují příklady `<delimited_identifier>`:  
  
```  
[Property With Space]  
[HR-EmployeeID]  
  
```  
  
 `<quoted_identifier>`je řetězec, který je uzavřena dvojitých uvozovek nahoře. Dvojité uvozovky v identifikátoru je reprezentován jako dva znaky uvozovek. Není doporučeno, protože můžete snadno Nezaměňovat s konstantní řetězec v uvozovkách toouse identifikátory. Pokud je to možné použijte s oddělovači identifikátor. Hello tady je příklad `<quoted_identifier>`:  
  
```  
"Contoso & Northwind"  
```  
  
## <a name="pattern"></a>vzor  
  
```  
<pattern> ::=  
      <expression>  
```  
  
### <a name="remarks"></a>Poznámky
  
 `<pattern>`musí být výraz, který se vyhodnotí jako řetězec. Použije se jako vzor pro hello jako operátor.      Může obsahovat hello následující zástupné znaky:  
  
-   `%`: Řetězec nula nebo více znaků.  
  
-   `_`: Jakémukoliv jedinému znaku.  
  
## <a name="escapechar"></a>escape_char  
  
```  
<escape_char> ::=  
      <expression>  
```  
  
### <a name="remarks"></a>Poznámky
  
 `<escape_char>`musí být výraz, který se vyhodnotí jako řetězec o délce 1. Slouží jako řídicí znak pro hello jako operátor.  
  
 Například `property LIKE 'ABC\%' ESCAPE '\'` odpovídá `ABC%` místo řetězec, který začíná `ABC`.  
  
## <a name="constant"></a>Konstantní  
  
```  
<constant> ::=  
      <integer_constant> | <decimal_constant> | <approximate_number_constant> | <boolean_constant> | NULL  
```  
  
### <a name="arguments"></a>Argumenty  
  
-   `<integer_constant>`je řetězec čísel, která se nenacházejí v uvozovkách a neobsahují desetinných míst. Hello hodnoty se uloží jako `System.Int64` interně, a postupujte podle hello stejný rozsah.  
  
     Hello Následují příklady dlouho konstanty:  
  
    ```  
    1894  
    2  
    ```  
  
-   `<decimal_constant>`je řetězec čísel, která se nenacházejí v uvozovkách a obsahovat desetinné čárky. Hello hodnoty se uloží jako `System.Double` interně a postupujte podle hello stejný rozsah nebo přesnosti.  
  
     V budoucí verzi, toto číslo může být uložena v jiný datový toosupport přesné číslo Sémantika typu, takže byste neměli spoléhat na hello fakt hello základní datový typ je `System.Double` pro `<decimal_constant>`.  
  
     Hello Následují příklady decimal konstanty:  
  
    ```  
    1894.1204  
    2.0  
    ```  
  
-   `<approximate_number_constant>`je číslo napsaných v exponenciální notace. Hello hodnoty se uloží jako `System.Double` interně a postupujte podle hello stejný rozsah nebo přesnosti. Hello Následují příklady přibližnou číselné konstanty:  
  
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
  
Logická hodnota konstanty jsou reprezentované pomocí klíčových slov hello `TRUE` nebo `FALSE`. Hello hodnoty se uloží jako `System.Boolean`.  
  
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

Hello `newid()` funkce vrátí **System.Guid** generované hello `System.Guid.NewGuid()` metoda.  
  
Hello `property(name)` funkce vrátí hodnotu hello hello vlastnosti odkazuje `name`. Hello `name` hodnota může být libovolný platný výraz, který vrací řetězcovou hodnotu.  
  
## <a name="considerations"></a>Požadavky

- Sada je použité toocreate novou hodnotu hello vlastnost nebo aktualizovat existující vlastnost.
- ODEBRAT je použité tooremove vlastnost.
- Pokud typ výrazu hello a existující typ vlastnosti hello se liší, provede sadu implicitní převod Pokud je to možné.
- Akce selže, pokud odkazované vlastnosti neexistující systému.
- Akce neselže, pokud byly na něj odkazovalo vlastnosti neexistující uživatele.
- Vlastnost uživatele neexistující interně vyhodnotí jako "Neznámý", následující hello stejnou sémantiku jako [SQLFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) při vyhodnocování operátory.

## <a name="next-steps"></a>Další kroky

- [SQLRuleAction – třída](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction)
- [SQLFilter – třída](/dotnet/api/microsoft.servicebus.messaging.sqlfilter)
