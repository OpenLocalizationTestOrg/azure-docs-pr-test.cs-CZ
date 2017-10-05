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
ms.date: 06/28/2017
ms.author: sethm
ms.openlocfilehash: 7379b7f58563675f28d77928d933c0d9c7992e71
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="sqlruleaction-syntax"></a><span data-ttu-id="8717d-103">Syntaxe SQLRuleAction</span><span class="sxs-lookup"><span data-stu-id="8717d-103">SQLRuleAction syntax</span></span>

<span data-ttu-id="8717d-104">A *SqlRuleAction* je instance [SqlRuleAction](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction) třídy a představuje sadu akcí, které jsou napsané v jazyce SQL na základě syntaxi, která bude provedena [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span><span class="sxs-lookup"><span data-stu-id="8717d-104">A *SqlRuleAction* is an instance of the [SqlRuleAction](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction) class, and represents set of actions written in SQL-language based syntax that is performed against a [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span></span>   
  
<span data-ttu-id="8717d-105">Toto téma obsahuje podrobnosti o gramatiku SQL pravidlo akce.</span><span class="sxs-lookup"><span data-stu-id="8717d-105">This topic lists details about the SQL rule action grammar.</span></span>  
  
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
  
## <a name="arguments"></a><span data-ttu-id="8717d-106">Argumenty</span><span class="sxs-lookup"><span data-stu-id="8717d-106">Arguments</span></span>  
  
-   <span data-ttu-id="8717d-107">`<scope>`je volitelný řetězec označující oboru `<property_name>`.</span><span class="sxs-lookup"><span data-stu-id="8717d-107">`<scope>` is an optional string indicating the scope of the `<property_name>`.</span></span> <span data-ttu-id="8717d-108">Platné hodnoty jsou `sys` nebo `user`.</span><span class="sxs-lookup"><span data-stu-id="8717d-108">Valid values are `sys` or `user`.</span></span> <span data-ttu-id="8717d-109">`sys` Hodnota označuje rozsah systémů kde `<property_name>` je název veřejné vlastnosti [BrokeredMessage třída](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span><span class="sxs-lookup"><span data-stu-id="8717d-109">The `sys` value indicates system scope where `<property_name>` is a public property name of the [BrokeredMessage Class](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span></span> <span data-ttu-id="8717d-110">`user`označuje oboru uživatele kde `<property_name>` je klíč z [BrokeredMessage třída](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) slovníku.</span><span class="sxs-lookup"><span data-stu-id="8717d-110">`user` indicates user scope where `<property_name>` is a key of the [BrokeredMessage Class](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) dictionary.</span></span> <span data-ttu-id="8717d-111">`user`rozsah je výchozí obor, pokud `<scope>` není zadán.</span><span class="sxs-lookup"><span data-stu-id="8717d-111">`user` scope is the default scope if `<scope>` is not specified.</span></span>  
  
### <a name="remarks"></a><span data-ttu-id="8717d-112">Poznámky</span><span class="sxs-lookup"><span data-stu-id="8717d-112">Remarks</span></span>  

<span data-ttu-id="8717d-113">Pokus o přístup k systému neexistující vlastnost je k chybě při pokusu o přístup k neexistující uživatele vlastnost není chyba.</span><span class="sxs-lookup"><span data-stu-id="8717d-113">An attempt to access a non-existent system property is an error, while an attempt to access a non-existent user property is not an error.</span></span> <span data-ttu-id="8717d-114">Místo toho vlastnost uživatele neexistující interně vyhodnotí jako neznámou hodnotou.</span><span class="sxs-lookup"><span data-stu-id="8717d-114">Instead, a non-existent user property is internally evaluated as an unknown value.</span></span> <span data-ttu-id="8717d-115">Neznámá hodnota považuje speciálně během vyhodnocení operátor.</span><span class="sxs-lookup"><span data-stu-id="8717d-115">An unknown value is treated specially during operator evaluation.</span></span>  
  
## <a name="propertyname"></a><span data-ttu-id="8717d-116">%{Property_Name/</span><span class="sxs-lookup"><span data-stu-id="8717d-116">property_name</span></span>  
  
```  
<property_name> ::=  
     <identifier>  
     | <delimited_identifier>  
  
<identifier> ::=  
     <regular_identifier> | <quoted_identifier> | <delimited_identifier>  
  
```  
  
### <a name="arguments"></a><span data-ttu-id="8717d-117">Argumenty</span><span class="sxs-lookup"><span data-stu-id="8717d-117">Arguments</span></span>  
 <span data-ttu-id="8717d-118">`<regular_identifier>`řetězec reprezentována následujícímu regulárnímu výrazu:</span><span class="sxs-lookup"><span data-stu-id="8717d-118">`<regular_identifier>` is a string represented by the following regular expression:</span></span>  
  
```  
[[:IsLetter:]][_[:IsLetter:][:IsDigit:]]*  
```  
  
 <span data-ttu-id="8717d-119">To znamená libovolný řetězec, který začíná písmenem a následuje jeden nebo více podtržítko nebo písmeno nebo číslice.</span><span class="sxs-lookup"><span data-stu-id="8717d-119">This means any string that starts with a letter and is followed by one or more underscore/letter/digit.</span></span>  
  
 <span data-ttu-id="8717d-120">`[:IsLetter:]`znamená libovolný znak Unicode, který je zařazený do kategorie jako písmeno kódování Unicode.</span><span class="sxs-lookup"><span data-stu-id="8717d-120">`[:IsLetter:]` means any Unicode character that is categorized as a Unicode letter.</span></span> <span data-ttu-id="8717d-121">`System.Char.IsLetter(c)`Vrátí `true` Pokud `c` písmeno kódování Unicode.</span><span class="sxs-lookup"><span data-stu-id="8717d-121">`System.Char.IsLetter(c)` returns `true` if `c` is a Unicode letter.</span></span>  
  
 <span data-ttu-id="8717d-122">`[:IsDigit:]`znamená libovolný znak Unicode, který je zařazený do kategorie jako desítková číslice.</span><span class="sxs-lookup"><span data-stu-id="8717d-122">`[:IsDigit:]` means any Unicode character that is categorized as a decimal digit.</span></span> <span data-ttu-id="8717d-123">`System.Char.IsDigit(c)`Vrátí `true` Pokud `c` je číslice kódování Unicode.</span><span class="sxs-lookup"><span data-stu-id="8717d-123">`System.Char.IsDigit(c)` returns `true` if `c` is a Unicode digit.</span></span>  
  
 <span data-ttu-id="8717d-124">A `<regular_identifier>` nemůže být rezervované klíčové slovo.</span><span class="sxs-lookup"><span data-stu-id="8717d-124">A `<regular_identifier>` cannot be a reserved keyword.</span></span>  
  
 <span data-ttu-id="8717d-125">`<delimited_identifier>`je řetězec, který je uzavřena s levé nebo pravé hranaté závorky ([]).</span><span class="sxs-lookup"><span data-stu-id="8717d-125">`<delimited_identifier>` is any string that is enclosed with left/right square brackets ([]).</span></span> <span data-ttu-id="8717d-126">Pravou hranatou závorku je reprezentován jako dvě pravé hranaté závorky.</span><span class="sxs-lookup"><span data-stu-id="8717d-126">A right square bracket is represented as two right square brackets.</span></span> <span data-ttu-id="8717d-127">Následují příklady `<delimited_identifier>`:</span><span class="sxs-lookup"><span data-stu-id="8717d-127">The following are examples of `<delimited_identifier>`:</span></span>  
  
```  
[Property With Space]  
[HR-EmployeeID]  
  
```  
  
 <span data-ttu-id="8717d-128">`<quoted_identifier>`je řetězec, který je uzavřena dvojitých uvozovek nahoře.</span><span class="sxs-lookup"><span data-stu-id="8717d-128">`<quoted_identifier>` is any string that is enclosed with double quotation marks.</span></span> <span data-ttu-id="8717d-129">Dvojité uvozovky v identifikátoru je reprezentován jako dva znaky uvozovek.</span><span class="sxs-lookup"><span data-stu-id="8717d-129">A double quotation mark in identifier is represented as two double quotation marks.</span></span> <span data-ttu-id="8717d-130">Není doporučeno použít identifikátory v uvozovkách, protože můžete snadno Nezaměňovat s řetězcová konstanta.</span><span class="sxs-lookup"><span data-stu-id="8717d-130">It is not recommended to use quoted identifiers because it can easily be confused with a string constant.</span></span> <span data-ttu-id="8717d-131">Pokud je to možné použijte s oddělovači identifikátor.</span><span class="sxs-lookup"><span data-stu-id="8717d-131">Use a delimited identifier if possible.</span></span> <span data-ttu-id="8717d-132">Tady je příklad `<quoted_identifier>`:</span><span class="sxs-lookup"><span data-stu-id="8717d-132">The following is an example of `<quoted_identifier>`:</span></span>  
  
```  
"Contoso & Northwind"  
```  
  
## <a name="pattern"></a><span data-ttu-id="8717d-133">vzor</span><span class="sxs-lookup"><span data-stu-id="8717d-133">pattern</span></span>  
  
```  
<pattern> ::=  
      <expression>  
```  
  
### <a name="remarks"></a><span data-ttu-id="8717d-134">Poznámky</span><span class="sxs-lookup"><span data-stu-id="8717d-134">Remarks</span></span>
  
 <span data-ttu-id="8717d-135">`<pattern>`musí být výraz, který se vyhodnotí jako řetězec.</span><span class="sxs-lookup"><span data-stu-id="8717d-135">`<pattern>` must be an expression that is evaluated as a string.</span></span> <span data-ttu-id="8717d-136">Použije se jako vzor pro operátor LIKE.</span><span class="sxs-lookup"><span data-stu-id="8717d-136">It is used as a pattern for the LIKE operator.</span></span>      <span data-ttu-id="8717d-137">Může obsahovat následující znaky:</span><span class="sxs-lookup"><span data-stu-id="8717d-137">It can contain the following wildcard characters:</span></span>  
  
-   <span data-ttu-id="8717d-138">`%`: Řetězec nula nebo více znaků.</span><span class="sxs-lookup"><span data-stu-id="8717d-138">`%`:  Any string of zero or more characters.</span></span>  
  
-   <span data-ttu-id="8717d-139">`_`: Jakémukoliv jedinému znaku.</span><span class="sxs-lookup"><span data-stu-id="8717d-139">`_`: Any single character.</span></span>  
  
## <a name="escapechar"></a><span data-ttu-id="8717d-140">escape_char</span><span class="sxs-lookup"><span data-stu-id="8717d-140">escape_char</span></span>  
  
```  
<escape_char> ::=  
      <expression>  
```  
  
### <a name="remarks"></a><span data-ttu-id="8717d-141">Poznámky</span><span class="sxs-lookup"><span data-stu-id="8717d-141">Remarks</span></span>
  
 <span data-ttu-id="8717d-142">`<escape_char>`musí být výraz, který se vyhodnotí jako řetězec o délce 1.</span><span class="sxs-lookup"><span data-stu-id="8717d-142">`<escape_char>` must be an expression that is evaluated as a string of length 1.</span></span> <span data-ttu-id="8717d-143">Slouží jako řídicí znak pro operátor LIKE.</span><span class="sxs-lookup"><span data-stu-id="8717d-143">It is used as an escape character for the LIKE operator.</span></span>  
  
 <span data-ttu-id="8717d-144">Například `property LIKE 'ABC\%' ESCAPE '\'` odpovídá `ABC%` místo řetězec, který začíná `ABC`.</span><span class="sxs-lookup"><span data-stu-id="8717d-144">For example, `property LIKE 'ABC\%' ESCAPE '\'` matches `ABC%` rather than a string that starts with `ABC`.</span></span>  
  
## <a name="constant"></a><span data-ttu-id="8717d-145">Konstantní</span><span class="sxs-lookup"><span data-stu-id="8717d-145">constant</span></span>  
  
```  
<constant> ::=  
      <integer_constant> | <decimal_constant> | <approximate_number_constant> | <boolean_constant> | NULL  
```  
  
### <a name="arguments"></a><span data-ttu-id="8717d-146">Argumenty</span><span class="sxs-lookup"><span data-stu-id="8717d-146">Arguments</span></span>  
  
-   <span data-ttu-id="8717d-147">`<integer_constant>`je řetězec čísel, která se nenacházejí v uvozovkách a neobsahují desetinných míst.</span><span class="sxs-lookup"><span data-stu-id="8717d-147">`<integer_constant>` is a string of numbers that are not enclosed in quotation marks and do not contain decimal points.</span></span> <span data-ttu-id="8717d-148">Hodnoty jsou uloženy jako `System.Int64` interně a postupujte podle stejného rozsahu.</span><span class="sxs-lookup"><span data-stu-id="8717d-148">The values are stored as `System.Int64` internally, and follow the same range.</span></span>  
  
     <span data-ttu-id="8717d-149">Následují příklady dlouho konstant:</span><span class="sxs-lookup"><span data-stu-id="8717d-149">The following are examples of long constants:</span></span>  
  
    ```  
    1894  
    2  
    ```  
  
-   <span data-ttu-id="8717d-150">`<decimal_constant>`je řetězec čísel, která se nenacházejí v uvozovkách a obsahovat desetinné čárky.</span><span class="sxs-lookup"><span data-stu-id="8717d-150">`<decimal_constant>` is a string of numbers that are not enclosed in quotation marks, and contain a decimal point.</span></span> <span data-ttu-id="8717d-151">Hodnoty jsou uloženy jako `System.Double` interně a postupujte podle stejné rozsah nebo přesnosti.</span><span class="sxs-lookup"><span data-stu-id="8717d-151">The values are stored as `System.Double` internally, and follow the same range/precision.</span></span>  
  
     <span data-ttu-id="8717d-152">V budoucí verzi, může být tento číslo uložené v na jiný datový typ pro podporu přesné číslo sémantiku, takže byste neměli spoléhat na skutečnost, základní datový typ je `System.Double` pro `<decimal_constant>`.</span><span class="sxs-lookup"><span data-stu-id="8717d-152">In a future version, this number might be stored in a different data type to support exact number semantics, so you should not rely on the fact the underlying data type is `System.Double` for `<decimal_constant>`.</span></span>  
  
     <span data-ttu-id="8717d-153">Následují příklady decimal konstant:</span><span class="sxs-lookup"><span data-stu-id="8717d-153">The following are examples of decimal constants:</span></span>  
  
    ```  
    1894.1204  
    2.0  
    ```  
  
-   <span data-ttu-id="8717d-154">`<approximate_number_constant>`je číslo napsaných v exponenciální notace.</span><span class="sxs-lookup"><span data-stu-id="8717d-154">`<approximate_number_constant>` is a number written in scientific notation.</span></span> <span data-ttu-id="8717d-155">Hodnoty jsou uloženy jako `System.Double` interně a postupujte podle stejné rozsah nebo přesnosti.</span><span class="sxs-lookup"><span data-stu-id="8717d-155">The values are stored as `System.Double` internally, and follow the same range/precision.</span></span> <span data-ttu-id="8717d-156">Následují příklady přibližnou číselné konstanty:</span><span class="sxs-lookup"><span data-stu-id="8717d-156">The following are examples of approximate number constants:</span></span>  
  
    ```  
    101.5E5  
    0.5E-2  
    ```  
  
## <a name="booleanconstant"></a><span data-ttu-id="8717d-157">boolean_constant</span><span class="sxs-lookup"><span data-stu-id="8717d-157">boolean_constant</span></span>  
  
```  
<boolean_constant> :=  
      TRUE | FALSE  
```  
  
### <a name="remarks"></a><span data-ttu-id="8717d-158">Poznámky</span><span class="sxs-lookup"><span data-stu-id="8717d-158">Remarks</span></span>
  
<span data-ttu-id="8717d-159">Logická hodnota konstanty jsou reprezentované pomocí klíčová slova `TRUE` nebo `FALSE`.</span><span class="sxs-lookup"><span data-stu-id="8717d-159">Boolean constants are represented by the keywords `TRUE` or `FALSE`.</span></span> <span data-ttu-id="8717d-160">Hodnoty jsou uloženy jako `System.Boolean`.</span><span class="sxs-lookup"><span data-stu-id="8717d-160">The values are stored as `System.Boolean`.</span></span>  
  
## <a name="stringconstant"></a><span data-ttu-id="8717d-161">string_constant</span><span class="sxs-lookup"><span data-stu-id="8717d-161">string_constant</span></span>  
  
```  
<string_constant>  
```  
  
### <a name="remarks"></a><span data-ttu-id="8717d-162">Poznámky</span><span class="sxs-lookup"><span data-stu-id="8717d-162">Remarks</span></span>
  
<span data-ttu-id="8717d-163">Řetězcové konstanty jsou uzavřené v jednoduchých uvozovkách a zahrnují všechny platné znaky kódování Unicode.</span><span class="sxs-lookup"><span data-stu-id="8717d-163">String constants are enclosed in single quotation marks and include any valid Unicode characters.</span></span> <span data-ttu-id="8717d-164">Jednoduché uvozovky, vložené do řetězcová konstanta je reprezentována jako dvě jednoduché uvozovky.</span><span class="sxs-lookup"><span data-stu-id="8717d-164">A single quotation mark embedded in a string constant is represented as two single quotation marks.</span></span>  
  
## <a name="function"></a><span data-ttu-id="8717d-165">Funkce</span><span class="sxs-lookup"><span data-stu-id="8717d-165">function</span></span>  
  
```  
<function> :=  
      newid() |  
      property(name) | p(name)  
```  
  
### <a name="remarks"></a><span data-ttu-id="8717d-166">Poznámky</span><span class="sxs-lookup"><span data-stu-id="8717d-166">Remarks</span></span>  

<span data-ttu-id="8717d-167">`newid()` Funkce vrátí **System.Guid** vygenerované `System.Guid.NewGuid()` metoda.</span><span class="sxs-lookup"><span data-stu-id="8717d-167">The `newid()` function returns a **System.Guid** generated by the `System.Guid.NewGuid()` method.</span></span>  
  
<span data-ttu-id="8717d-168">`property(name)` Funkce vrátí hodnotu vlastnosti odkazuje `name`.</span><span class="sxs-lookup"><span data-stu-id="8717d-168">The `property(name)` function returns the value of the property referenced by `name`.</span></span> <span data-ttu-id="8717d-169">`name` Hodnota může být libovolný platný výraz, který vrací řetězcovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="8717d-169">The `name` value can be any valid expression that returns a string value.</span></span>  
  
## <a name="considerations"></a><span data-ttu-id="8717d-170">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8717d-170">Considerations</span></span>

- <span data-ttu-id="8717d-171">Sada se používá k vytvoření nové vlastnosti nebo aktualizujte hodnotu existující vlastnost.</span><span class="sxs-lookup"><span data-stu-id="8717d-171">SET is used to create a new property or update the value of an existing property.</span></span>
- <span data-ttu-id="8717d-172">ODEBRAT slouží k odebrání vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="8717d-172">REMOVE is used to remove a property.</span></span>
- <span data-ttu-id="8717d-173">Pokud typ výrazu a existující typ vlastnosti se liší, provede sadu implicitní převod Pokud je to možné.</span><span class="sxs-lookup"><span data-stu-id="8717d-173">SET performs implicit conversion if possible when the expression type and the existing property type are different.</span></span>
- <span data-ttu-id="8717d-174">Akce selže, pokud odkazované vlastnosti neexistující systému.</span><span class="sxs-lookup"><span data-stu-id="8717d-174">Action fails if non-existent system properties were referenced.</span></span>
- <span data-ttu-id="8717d-175">Akce neselže, pokud byly na něj odkazovalo vlastnosti neexistující uživatele.</span><span class="sxs-lookup"><span data-stu-id="8717d-175">Action does not fail if non-existent user properties were referenced.</span></span>
- <span data-ttu-id="8717d-176">Vlastnost uživatele neexistující vyhodnotí jako "Neznámý" interně následující stejnou sémantiku jako [SQLFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) při vyhodnocování operátory.</span><span class="sxs-lookup"><span data-stu-id="8717d-176">A non-existent user property is evaluated as "Unknown" internally, following the same semantics as [SQLFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) when evaluating operators.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8717d-177">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8717d-177">Next steps</span></span>

- [<span data-ttu-id="8717d-178">SQLRuleAction – třída</span><span class="sxs-lookup"><span data-stu-id="8717d-178">SQLRuleAction class</span></span>](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction)
- [<span data-ttu-id="8717d-179">SQLFilter – třída</span><span class="sxs-lookup"><span data-stu-id="8717d-179">SQLFilter class</span></span>](/dotnet/api/microsoft.servicebus.messaging.sqlfilter)
