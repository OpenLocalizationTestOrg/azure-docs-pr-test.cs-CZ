---
title: aaaAzure reference syntaxe Service Bus SQLFilter | Microsoft Docs
description: Podrobnosti o SQLFilter gramatika.
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
ms.date: 06/27/2017
ms.author: sethm
ms.openlocfilehash: ea49d42e343a6b324eb34c7831ff6be2855346e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sqlfilter-syntax"></a>Syntaxe SQLFilter

A *SqlFilter* je instance hello [SqlFilter třída](/dotnet/api/microsoft.servicebus.messaging.sqlfilter)a představuje výraz filtru na základě jazyka SQL, který se vyhodnotí proti [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage). SqlFilter podporuje podmnožinu hello SQL 92 standard.  
  
 Toto téma obsahuje podrobnosti o SqlFilter gramatika.  
  
```  
<predicate ::=  
      { NOT <predicate> }  
      | <predicate> AND <predicate>  
      | <predicate> OR <predicate>  
      | <expression> { = | <> | != | > | >= | < | <= } <expression>  
      | <property> IS [NOT] NULL  
      | <expression> [NOT] IN ( <expression> [, ...n] )  
      | <expression> [NOT] LIKE <pattern> [ESCAPE <escape_char>]  
      | EXISTS ( <property> )  
      | ( <predicate> )  
  
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
  
## <a name="remarks"></a>Poznámky

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
  
Tato gramatika znamená libovolný řetězec, který začíná písmenem a následuje jeden nebo více podtržítko nebo písmeno nebo číslice.  
  
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
  
     Toto jsou příklady dlouho konstanty:  
  
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

Logická hodnota konstanty jsou reprezentované pomocí klíčových slov hello **TRUE** nebo **FALSE**. Hello hodnoty se uloží jako `System.Boolean`.  
  
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
  
Vezměte v úvahu následující hello [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) sémantiku:  
  
-   Názvy vlastností se velká a malá písmena.  
  
-   Operátory podle jazyka C# implicitní převod sémantiku kdykoli je to možné.  
  
-   Vlastnosti systému jsou veřejné vlastnosti v [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) instance.  
  
    Vezměte v úvahu následující hello `IS [NOT] NULL` sémantiku:  
  
    -   `property IS NULL`vyhodnotí jako `true` Pokud buď vlastnost hello nebude existovat nebo hello hodnota vlastnosti je `null`.  
  
### <a name="property-evaluation-semantics"></a>Sémantika vyhodnocení vlastnosti  
  
-   Pokusu o tooevaluate systému neexistující vlastnost vyvolá [FilterException](/dotnet/api/microsoft.servicebus.messaging.filterexception) výjimka.  
  
-   Vlastnost, která neexistuje interně vyhodnotí jako **neznámé**.  
  
 Neznámý vyhodnocení v aritmetické operátory:  
  
-   Pro binární operátory, pokud buď hello levé nebo pravé straně operandy vyhodnotí jako **neznámé**, pak je výsledek hello **neznámé**.  
  
-   Pro unární operátory, pokud operand vyhodnotí jako **neznámé**, pak je výsledek hello **neznámé**.  
  
 Neznámý vyhodnocení v binární porovnání operátory:  
  
-   Pokud buď hello levé nebo pravé straně operandy vyhodnotí jako **neznámé**, pak je výsledek hello **neznámé**.  
  
 Neznámý vyhodnocení v `[NOT] LIKE`:  
  
-   Pokud operandem any vyhodnotí jako **neznámé**, pak je výsledek hello **neznámé**.  
  
 Neznámý vyhodnocení v `[NOT] IN`:  
  
-   Pokud hello levý operand vyhodnotí jako **neznámé**, pak je výsledek hello **neznámé**.  
  
 Neznámý vyhodnocení v **a** operátor:  
  
```  
+---+---+---+---+  
|AND| T | F | U |  
+---+---+---+---+  
| T | T | F | U |  
+---+---+---+---+  
| F | F | F | F |  
+---+---+---+---+  
| U | U | F | U |  
+---+---+---+---+  
```  
  
 Neznámý vyhodnocení v **nebo** operátor:  
  
```  
+---+---+---+---+  
|OR | T | F | U |  
+---+---+---+---+  
| T | T | T | T |  
+---+---+---+---+  
| F | T | F | U |  
+---+---+---+---+  
| U | T | U | U |  
+---+---+---+---+  
```  
  
### <a name="operator-binding-semantics"></a>Operátor sémantiku vazby
  
-   Operátory porovnání jako `>`, `>=`, `<`, `<=`, `!=`, a `=` postupujte podle hello stejnou sémantiku jako operátor hello C# vazby v datech zadejte reklamními nabídkami a implicitní převody.  
  
-   Aritmetické operátory jako `+`, `-`, `*`, `/`, a `%` postupujte podle hello stejnou sémantiku jako operátor hello C# vazby v datech zadejte reklamními nabídkami a implicitní převody.

## <a name="next-steps"></a>Další kroky

- [SQLFilter – třída](/dotnet/api/microsoft.servicebus.messaging.sqlfilter)
- [SQLRuleAction – třída](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction)