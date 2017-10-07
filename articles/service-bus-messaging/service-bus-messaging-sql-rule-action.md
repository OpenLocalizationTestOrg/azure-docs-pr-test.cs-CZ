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
# <a name="sqlruleaction-syntax"></a><span data-ttu-id="37c9f-103">Syntaxe SQLRuleAction</span><span class="sxs-lookup"><span data-stu-id="37c9f-103">SQLRuleAction syntax</span></span>

<span data-ttu-id="37c9f-104">A *SqlRuleAction* je instance hello [SqlRuleAction](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction) třídy a představuje sadu akcí, které jsou napsané v jazyce SQL na základě syntaxi, která bude provedena [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span><span class="sxs-lookup"><span data-stu-id="37c9f-104">A *SqlRuleAction* is an instance of hello [SqlRuleAction](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction) class, and represents set of actions written in SQL-language based syntax that is performed against a [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span></span>   
  
<span data-ttu-id="37c9f-105">Toto téma obsahuje podrobnosti o hello gramatiky akce pravidla SQL.</span><span class="sxs-lookup"><span data-stu-id="37c9f-105">This topic lists details about hello SQL rule action grammar.</span></span>  
  
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
  
## <a name="arguments"></a><span data-ttu-id="37c9f-106">Argumenty</span><span class="sxs-lookup"><span data-stu-id="37c9f-106">Arguments</span></span>  
  
-   <span data-ttu-id="37c9f-107">`<scope>`je volitelný řetězec označující hello oboru hello `<property_name>`.</span><span class="sxs-lookup"><span data-stu-id="37c9f-107">`<scope>` is an optional string indicating hello scope of hello `<property_name>`.</span></span> <span data-ttu-id="37c9f-108">Platné hodnoty jsou `sys` nebo `user`.</span><span class="sxs-lookup"><span data-stu-id="37c9f-108">Valid values are `sys` or `user`.</span></span> <span data-ttu-id="37c9f-109">Hello `sys` hodnota určuje rozsah systémů kde `<property_name>` je název veřejné vlastnosti hello [BrokeredMessage třída](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span><span class="sxs-lookup"><span data-stu-id="37c9f-109">hello `sys` value indicates system scope where `<property_name>` is a public property name of hello [BrokeredMessage Class](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span></span> <span data-ttu-id="37c9f-110">`user`označuje oboru uživatele kde `<property_name>` je klíč hello [BrokeredMessage třída](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) slovníku.</span><span class="sxs-lookup"><span data-stu-id="37c9f-110">`user` indicates user scope where `<property_name>` is a key of hello [BrokeredMessage Class](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) dictionary.</span></span> <span data-ttu-id="37c9f-111">`user`rozsah je výchozí obor hello, pokud `<scope>` není zadán.</span><span class="sxs-lookup"><span data-stu-id="37c9f-111">`user` scope is hello default scope if `<scope>` is not specified.</span></span>  
  
### <a name="remarks"></a><span data-ttu-id="37c9f-112">Poznámky</span><span class="sxs-lookup"><span data-stu-id="37c9f-112">Remarks</span></span>  

<span data-ttu-id="37c9f-113">Tooaccess pokusu o systému neexistující vlastnost je k chybě při vlastnost uživatele pokusu o tooaccess neexistující není chyba.</span><span class="sxs-lookup"><span data-stu-id="37c9f-113">An attempt tooaccess a non-existent system property is an error, while an attempt tooaccess a non-existent user property is not an error.</span></span> <span data-ttu-id="37c9f-114">Místo toho vlastnost uživatele neexistující interně vyhodnotí jako neznámou hodnotou.</span><span class="sxs-lookup"><span data-stu-id="37c9f-114">Instead, a non-existent user property is internally evaluated as an unknown value.</span></span> <span data-ttu-id="37c9f-115">Neznámá hodnota považuje speciálně během vyhodnocení operátor.</span><span class="sxs-lookup"><span data-stu-id="37c9f-115">An unknown value is treated specially during operator evaluation.</span></span>  
  
## <a name="propertyname"></a><span data-ttu-id="37c9f-116">%{Property_Name/</span><span class="sxs-lookup"><span data-stu-id="37c9f-116">property_name</span></span>  
  
```  
<property_name> ::=  
     <identifier>  
     | <delimited_identifier>  
  
<identifier> ::=  
     <regular_identifier> | <quoted_identifier> | <delimited_identifier>  
  
```  
  
### <a name="arguments"></a><span data-ttu-id="37c9f-117">Argumenty</span><span class="sxs-lookup"><span data-stu-id="37c9f-117">Arguments</span></span>  
 <span data-ttu-id="37c9f-118">`<regular_identifier>`je řetězec reprezentována hello následující regulární výraz:</span><span class="sxs-lookup"><span data-stu-id="37c9f-118">`<regular_identifier>` is a string represented by hello following regular expression:</span></span>  
  
```  
[[:IsLetter:]][_[:IsLetter:][:IsDigit:]]*  
```  
  
 <span data-ttu-id="37c9f-119">To znamená libovolný řetězec, který začíná písmenem a následuje jeden nebo více podtržítko nebo písmeno nebo číslice.</span><span class="sxs-lookup"><span data-stu-id="37c9f-119">This means any string that starts with a letter and is followed by one or more underscore/letter/digit.</span></span>  
  
 <span data-ttu-id="37c9f-120">`[:IsLetter:]`znamená libovolný znak Unicode, který je zařazený do kategorie jako písmeno kódování Unicode.</span><span class="sxs-lookup"><span data-stu-id="37c9f-120">`[:IsLetter:]` means any Unicode character that is categorized as a Unicode letter.</span></span> <span data-ttu-id="37c9f-121">`System.Char.IsLetter(c)`Vrátí `true` Pokud `c` písmeno kódování Unicode.</span><span class="sxs-lookup"><span data-stu-id="37c9f-121">`System.Char.IsLetter(c)` returns `true` if `c` is a Unicode letter.</span></span>  
  
 <span data-ttu-id="37c9f-122">`[:IsDigit:]`znamená libovolný znak Unicode, který je zařazený do kategorie jako desítková číslice.</span><span class="sxs-lookup"><span data-stu-id="37c9f-122">`[:IsDigit:]` means any Unicode character that is categorized as a decimal digit.</span></span> <span data-ttu-id="37c9f-123">`System.Char.IsDigit(c)`Vrátí `true` Pokud `c` je číslice kódování Unicode.</span><span class="sxs-lookup"><span data-stu-id="37c9f-123">`System.Char.IsDigit(c)` returns `true` if `c` is a Unicode digit.</span></span>  
  
 <span data-ttu-id="37c9f-124">A `<regular_identifier>` nemůže být rezervované klíčové slovo.</span><span class="sxs-lookup"><span data-stu-id="37c9f-124">A `<regular_identifier>` cannot be a reserved keyword.</span></span>  
  
 <span data-ttu-id="37c9f-125">`<delimited_identifier>`je řetězec, který je uzavřena s levé nebo pravé hranaté závorky ([]).</span><span class="sxs-lookup"><span data-stu-id="37c9f-125">`<delimited_identifier>` is any string that is enclosed with left/right square brackets ([]).</span></span> <span data-ttu-id="37c9f-126">Pravou hranatou závorku je reprezentován jako dvě pravé hranaté závorky.</span><span class="sxs-lookup"><span data-stu-id="37c9f-126">A right square bracket is represented as two right square brackets.</span></span> <span data-ttu-id="37c9f-127">Hello Následují příklady `<delimited_identifier>`:</span><span class="sxs-lookup"><span data-stu-id="37c9f-127">hello following are examples of `<delimited_identifier>`:</span></span>  
  
```  
[Property With Space]  
[HR-EmployeeID]  
  
```  
  
 <span data-ttu-id="37c9f-128">`<quoted_identifier>`je řetězec, který je uzavřena dvojitých uvozovek nahoře.</span><span class="sxs-lookup"><span data-stu-id="37c9f-128">`<quoted_identifier>` is any string that is enclosed with double quotation marks.</span></span> <span data-ttu-id="37c9f-129">Dvojité uvozovky v identifikátoru je reprezentován jako dva znaky uvozovek.</span><span class="sxs-lookup"><span data-stu-id="37c9f-129">A double quotation mark in identifier is represented as two double quotation marks.</span></span> <span data-ttu-id="37c9f-130">Není doporučeno, protože můžete snadno Nezaměňovat s konstantní řetězec v uvozovkách toouse identifikátory.</span><span class="sxs-lookup"><span data-stu-id="37c9f-130">It is not recommended toouse quoted identifiers because it can easily be confused with a string constant.</span></span> <span data-ttu-id="37c9f-131">Pokud je to možné použijte s oddělovači identifikátor.</span><span class="sxs-lookup"><span data-stu-id="37c9f-131">Use a delimited identifier if possible.</span></span> <span data-ttu-id="37c9f-132">Hello tady je příklad `<quoted_identifier>`:</span><span class="sxs-lookup"><span data-stu-id="37c9f-132">hello following is an example of `<quoted_identifier>`:</span></span>  
  
```  
"Contoso & Northwind"  
```  
  
## <a name="pattern"></a><span data-ttu-id="37c9f-133">vzor</span><span class="sxs-lookup"><span data-stu-id="37c9f-133">pattern</span></span>  
  
```  
<pattern> ::=  
      <expression>  
```  
  
### <a name="remarks"></a><span data-ttu-id="37c9f-134">Poznámky</span><span class="sxs-lookup"><span data-stu-id="37c9f-134">Remarks</span></span>
  
 <span data-ttu-id="37c9f-135">`<pattern>`musí být výraz, který se vyhodnotí jako řetězec.</span><span class="sxs-lookup"><span data-stu-id="37c9f-135">`<pattern>` must be an expression that is evaluated as a string.</span></span> <span data-ttu-id="37c9f-136">Použije se jako vzor pro hello jako operátor.</span><span class="sxs-lookup"><span data-stu-id="37c9f-136">It is used as a pattern for hello LIKE operator.</span></span>      <span data-ttu-id="37c9f-137">Může obsahovat hello následující zástupné znaky:</span><span class="sxs-lookup"><span data-stu-id="37c9f-137">It can contain hello following wildcard characters:</span></span>  
  
-   <span data-ttu-id="37c9f-138">`%`: Řetězec nula nebo více znaků.</span><span class="sxs-lookup"><span data-stu-id="37c9f-138">`%`:  Any string of zero or more characters.</span></span>  
  
-   <span data-ttu-id="37c9f-139">`_`: Jakémukoliv jedinému znaku.</span><span class="sxs-lookup"><span data-stu-id="37c9f-139">`_`: Any single character.</span></span>  
  
## <a name="escapechar"></a><span data-ttu-id="37c9f-140">escape_char</span><span class="sxs-lookup"><span data-stu-id="37c9f-140">escape_char</span></span>  
  
```  
<escape_char> ::=  
      <expression>  
```  
  
### <a name="remarks"></a><span data-ttu-id="37c9f-141">Poznámky</span><span class="sxs-lookup"><span data-stu-id="37c9f-141">Remarks</span></span>
  
 <span data-ttu-id="37c9f-142">`<escape_char>`musí být výraz, který se vyhodnotí jako řetězec o délce 1.</span><span class="sxs-lookup"><span data-stu-id="37c9f-142">`<escape_char>` must be an expression that is evaluated as a string of length 1.</span></span> <span data-ttu-id="37c9f-143">Slouží jako řídicí znak pro hello jako operátor.</span><span class="sxs-lookup"><span data-stu-id="37c9f-143">It is used as an escape character for hello LIKE operator.</span></span>  
  
 <span data-ttu-id="37c9f-144">Například `property LIKE 'ABC\%' ESCAPE '\'` odpovídá `ABC%` místo řetězec, který začíná `ABC`.</span><span class="sxs-lookup"><span data-stu-id="37c9f-144">For example, `property LIKE 'ABC\%' ESCAPE '\'` matches `ABC%` rather than a string that starts with `ABC`.</span></span>  
  
## <a name="constant"></a><span data-ttu-id="37c9f-145">Konstantní</span><span class="sxs-lookup"><span data-stu-id="37c9f-145">constant</span></span>  
  
```  
<constant> ::=  
      <integer_constant> | <decimal_constant> | <approximate_number_constant> | <boolean_constant> | NULL  
```  
  
### <a name="arguments"></a><span data-ttu-id="37c9f-146">Argumenty</span><span class="sxs-lookup"><span data-stu-id="37c9f-146">Arguments</span></span>  
  
-   <span data-ttu-id="37c9f-147">`<integer_constant>`je řetězec čísel, která se nenacházejí v uvozovkách a neobsahují desetinných míst.</span><span class="sxs-lookup"><span data-stu-id="37c9f-147">`<integer_constant>` is a string of numbers that are not enclosed in quotation marks and do not contain decimal points.</span></span> <span data-ttu-id="37c9f-148">Hello hodnoty se uloží jako `System.Int64` interně, a postupujte podle hello stejný rozsah.</span><span class="sxs-lookup"><span data-stu-id="37c9f-148">hello values are stored as `System.Int64` internally, and follow hello same range.</span></span>  
  
     <span data-ttu-id="37c9f-149">Hello Následují příklady dlouho konstanty:</span><span class="sxs-lookup"><span data-stu-id="37c9f-149">hello following are examples of long constants:</span></span>  
  
    ```  
    1894  
    2  
    ```  
  
-   <span data-ttu-id="37c9f-150">`<decimal_constant>`je řetězec čísel, která se nenacházejí v uvozovkách a obsahovat desetinné čárky.</span><span class="sxs-lookup"><span data-stu-id="37c9f-150">`<decimal_constant>` is a string of numbers that are not enclosed in quotation marks, and contain a decimal point.</span></span> <span data-ttu-id="37c9f-151">Hello hodnoty se uloží jako `System.Double` interně a postupujte podle hello stejný rozsah nebo přesnosti.</span><span class="sxs-lookup"><span data-stu-id="37c9f-151">hello values are stored as `System.Double` internally, and follow hello same range/precision.</span></span>  
  
     <span data-ttu-id="37c9f-152">V budoucí verzi, toto číslo může být uložena v jiný datový toosupport přesné číslo Sémantika typu, takže byste neměli spoléhat na hello fakt hello základní datový typ je `System.Double` pro `<decimal_constant>`.</span><span class="sxs-lookup"><span data-stu-id="37c9f-152">In a future version, this number might be stored in a different data type toosupport exact number semantics, so you should not rely on hello fact hello underlying data type is `System.Double` for `<decimal_constant>`.</span></span>  
  
     <span data-ttu-id="37c9f-153">Hello Následují příklady decimal konstanty:</span><span class="sxs-lookup"><span data-stu-id="37c9f-153">hello following are examples of decimal constants:</span></span>  
  
    ```  
    1894.1204  
    2.0  
    ```  
  
-   <span data-ttu-id="37c9f-154">`<approximate_number_constant>`je číslo napsaných v exponenciální notace.</span><span class="sxs-lookup"><span data-stu-id="37c9f-154">`<approximate_number_constant>` is a number written in scientific notation.</span></span> <span data-ttu-id="37c9f-155">Hello hodnoty se uloží jako `System.Double` interně a postupujte podle hello stejný rozsah nebo přesnosti.</span><span class="sxs-lookup"><span data-stu-id="37c9f-155">hello values are stored as `System.Double` internally, and follow hello same range/precision.</span></span> <span data-ttu-id="37c9f-156">Hello Následují příklady přibližnou číselné konstanty:</span><span class="sxs-lookup"><span data-stu-id="37c9f-156">hello following are examples of approximate number constants:</span></span>  
  
    ```  
    101.5E5  
    0.5E-2  
    ```  
  
## <a name="booleanconstant"></a><span data-ttu-id="37c9f-157">boolean_constant</span><span class="sxs-lookup"><span data-stu-id="37c9f-157">boolean_constant</span></span>  
  
```  
<boolean_constant> :=  
      TRUE | FALSE  
```  
  
### <a name="remarks"></a><span data-ttu-id="37c9f-158">Poznámky</span><span class="sxs-lookup"><span data-stu-id="37c9f-158">Remarks</span></span>
  
<span data-ttu-id="37c9f-159">Logická hodnota konstanty jsou reprezentované pomocí klíčových slov hello `TRUE` nebo `FALSE`.</span><span class="sxs-lookup"><span data-stu-id="37c9f-159">Boolean constants are represented by hello keywords `TRUE` or `FALSE`.</span></span> <span data-ttu-id="37c9f-160">Hello hodnoty se uloží jako `System.Boolean`.</span><span class="sxs-lookup"><span data-stu-id="37c9f-160">hello values are stored as `System.Boolean`.</span></span>  
  
## <a name="stringconstant"></a><span data-ttu-id="37c9f-161">string_constant</span><span class="sxs-lookup"><span data-stu-id="37c9f-161">string_constant</span></span>  
  
```  
<string_constant>  
```  
  
### <a name="remarks"></a><span data-ttu-id="37c9f-162">Poznámky</span><span class="sxs-lookup"><span data-stu-id="37c9f-162">Remarks</span></span>
  
<span data-ttu-id="37c9f-163">Řetězcové konstanty jsou uzavřené v jednoduchých uvozovkách a zahrnují všechny platné znaky kódování Unicode.</span><span class="sxs-lookup"><span data-stu-id="37c9f-163">String constants are enclosed in single quotation marks and include any valid Unicode characters.</span></span> <span data-ttu-id="37c9f-164">Jednoduché uvozovky, vložené do řetězcová konstanta je reprezentována jako dvě jednoduché uvozovky.</span><span class="sxs-lookup"><span data-stu-id="37c9f-164">A single quotation mark embedded in a string constant is represented as two single quotation marks.</span></span>  
  
## <a name="function"></a><span data-ttu-id="37c9f-165">Funkce</span><span class="sxs-lookup"><span data-stu-id="37c9f-165">function</span></span>  
  
```  
<function> :=  
      newid() |  
      property(name) | p(name)  
```  
  
### <a name="remarks"></a><span data-ttu-id="37c9f-166">Poznámky</span><span class="sxs-lookup"><span data-stu-id="37c9f-166">Remarks</span></span>  

<span data-ttu-id="37c9f-167">Hello `newid()` funkce vrátí **System.Guid** generované hello `System.Guid.NewGuid()` metoda.</span><span class="sxs-lookup"><span data-stu-id="37c9f-167">hello `newid()` function returns a **System.Guid** generated by hello `System.Guid.NewGuid()` method.</span></span>  
  
<span data-ttu-id="37c9f-168">Hello `property(name)` funkce vrátí hodnotu hello hello vlastnosti odkazuje `name`.</span><span class="sxs-lookup"><span data-stu-id="37c9f-168">hello `property(name)` function returns hello value of hello property referenced by `name`.</span></span> <span data-ttu-id="37c9f-169">Hello `name` hodnota může být libovolný platný výraz, který vrací řetězcovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="37c9f-169">hello `name` value can be any valid expression that returns a string value.</span></span>  
  
## <a name="considerations"></a><span data-ttu-id="37c9f-170">Požadavky</span><span class="sxs-lookup"><span data-stu-id="37c9f-170">Considerations</span></span>

- <span data-ttu-id="37c9f-171">Sada je použité toocreate novou hodnotu hello vlastnost nebo aktualizovat existující vlastnost.</span><span class="sxs-lookup"><span data-stu-id="37c9f-171">SET is used toocreate a new property or update hello value of an existing property.</span></span>
- <span data-ttu-id="37c9f-172">ODEBRAT je použité tooremove vlastnost.</span><span class="sxs-lookup"><span data-stu-id="37c9f-172">REMOVE is used tooremove a property.</span></span>
- <span data-ttu-id="37c9f-173">Pokud typ výrazu hello a existující typ vlastnosti hello se liší, provede sadu implicitní převod Pokud je to možné.</span><span class="sxs-lookup"><span data-stu-id="37c9f-173">SET performs implicit conversion if possible when hello expression type and hello existing property type are different.</span></span>
- <span data-ttu-id="37c9f-174">Akce selže, pokud odkazované vlastnosti neexistující systému.</span><span class="sxs-lookup"><span data-stu-id="37c9f-174">Action fails if non-existent system properties were referenced.</span></span>
- <span data-ttu-id="37c9f-175">Akce neselže, pokud byly na něj odkazovalo vlastnosti neexistující uživatele.</span><span class="sxs-lookup"><span data-stu-id="37c9f-175">Action does not fail if non-existent user properties were referenced.</span></span>
- <span data-ttu-id="37c9f-176">Vlastnost uživatele neexistující interně vyhodnotí jako "Neznámý", následující hello stejnou sémantiku jako [SQLFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) při vyhodnocování operátory.</span><span class="sxs-lookup"><span data-stu-id="37c9f-176">A non-existent user property is evaluated as "Unknown" internally, following hello same semantics as [SQLFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) when evaluating operators.</span></span>

## <a name="next-steps"></a><span data-ttu-id="37c9f-177">Další kroky</span><span class="sxs-lookup"><span data-stu-id="37c9f-177">Next steps</span></span>

- [<span data-ttu-id="37c9f-178">SQLRuleAction – třída</span><span class="sxs-lookup"><span data-stu-id="37c9f-178">SQLRuleAction class</span></span>](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction)
- [<span data-ttu-id="37c9f-179">SQLFilter – třída</span><span class="sxs-lookup"><span data-stu-id="37c9f-179">SQLFilter class</span></span>](/dotnet/api/microsoft.servicebus.messaging.sqlfilter)
