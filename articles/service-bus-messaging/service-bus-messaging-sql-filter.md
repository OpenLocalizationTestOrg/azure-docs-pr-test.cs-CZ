---
title: Reference syntaxe Azure Service Bus SQLFilter | Microsoft Docs
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
ms.openlocfilehash: 3aaec8f9b6a3bbcf814f771405c3b589de6f7ae0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="sqlfilter-syntax"></a><span data-ttu-id="9091b-103">Syntaxe SQLFilter</span><span class="sxs-lookup"><span data-stu-id="9091b-103">SQLFilter syntax</span></span>

<span data-ttu-id="9091b-104">A *SqlFilter* je instance [SqlFilter třída](/dotnet/api/microsoft.servicebus.messaging.sqlfilter)a představuje výraz filtru na základě jazyka SQL, který se vyhodnotí proti [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span><span class="sxs-lookup"><span data-stu-id="9091b-104">A *SqlFilter* is an instance of the [SqlFilter class](/dotnet/api/microsoft.servicebus.messaging.sqlfilter), and represents a SQL language-based filter expression that is evaluated against a [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span></span> <span data-ttu-id="9091b-105">SqlFilter podporuje podmnožinu standardní SQL 92.</span><span class="sxs-lookup"><span data-stu-id="9091b-105">A SqlFilter supports a subset of the SQL-92 standard.</span></span>  
  
 <span data-ttu-id="9091b-106">Toto téma obsahuje podrobnosti o SqlFilter gramatika.</span><span class="sxs-lookup"><span data-stu-id="9091b-106">This topic lists details about SqlFilter grammar.</span></span>  
  
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
  
## <a name="arguments"></a><span data-ttu-id="9091b-107">Argumenty</span><span class="sxs-lookup"><span data-stu-id="9091b-107">Arguments</span></span>  
  
-   <span data-ttu-id="9091b-108">`<scope>`je volitelný řetězec označující oboru `<property_name>`.</span><span class="sxs-lookup"><span data-stu-id="9091b-108">`<scope>` is an optional string indicating the scope of the `<property_name>`.</span></span> <span data-ttu-id="9091b-109">Platné hodnoty jsou `sys` nebo `user`.</span><span class="sxs-lookup"><span data-stu-id="9091b-109">Valid values are `sys` or `user`.</span></span> <span data-ttu-id="9091b-110">`sys` Hodnota označuje rozsah systémů kde `<property_name>` je název veřejné vlastnosti [BrokeredMessage třída](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span><span class="sxs-lookup"><span data-stu-id="9091b-110">The `sys` value indicates system scope where `<property_name>` is a public property name of the [BrokeredMessage class](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span></span> <span data-ttu-id="9091b-111">`user`označuje oboru uživatele kde `<property_name>` je klíč z [BrokeredMessage třída](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) slovníku.</span><span class="sxs-lookup"><span data-stu-id="9091b-111">`user` indicates user scope where `<property_name>` is a key of the [BrokeredMessage class](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) dictionary.</span></span> <span data-ttu-id="9091b-112">`user`rozsah je výchozí obor, pokud `<scope>` není zadán.</span><span class="sxs-lookup"><span data-stu-id="9091b-112">`user` scope is the default scope if `<scope>` is not specified.</span></span>  
  
## <a name="remarks"></a><span data-ttu-id="9091b-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="9091b-113">Remarks</span></span>

<span data-ttu-id="9091b-114">Pokus o přístup k systému neexistující vlastnost je k chybě při pokusu o přístup k neexistující uživatele vlastnost není chyba.</span><span class="sxs-lookup"><span data-stu-id="9091b-114">An attempt to access a non-existent system property is an error, while an attempt to access a non-existent user property is not an error.</span></span> <span data-ttu-id="9091b-115">Místo toho vlastnost uživatele neexistující interně vyhodnotí jako neznámou hodnotou.</span><span class="sxs-lookup"><span data-stu-id="9091b-115">Instead, a non-existent user property is internally evaluated as an unknown value.</span></span> <span data-ttu-id="9091b-116">Neznámá hodnota považuje speciálně během vyhodnocení operátor.</span><span class="sxs-lookup"><span data-stu-id="9091b-116">An unknown value is treated specially during operator evaluation.</span></span>  
  
## <a name="propertyname"></a><span data-ttu-id="9091b-117">%{Property_Name/</span><span class="sxs-lookup"><span data-stu-id="9091b-117">property_name</span></span>  
  
```  
<property_name> ::=  
     <identifier>  
     | <delimited_identifier>  
  
<identifier> ::=  
     <regular_identifier> | <quoted_identifier> | <delimited_identifier>  
  
```  
  
### <a name="arguments"></a><span data-ttu-id="9091b-118">Argumenty</span><span class="sxs-lookup"><span data-stu-id="9091b-118">Arguments</span></span>  

 <span data-ttu-id="9091b-119">`<regular_identifier>`řetězec reprezentována následujícímu regulárnímu výrazu:</span><span class="sxs-lookup"><span data-stu-id="9091b-119">`<regular_identifier>` is a string represented by the following regular expression:</span></span>  
  
```  
[[:IsLetter:]][_[:IsLetter:][:IsDigit:]]*  
```  
  
<span data-ttu-id="9091b-120">Tato gramatika znamená libovolný řetězec, který začíná písmenem a následuje jeden nebo více podtržítko nebo písmeno nebo číslice.</span><span class="sxs-lookup"><span data-stu-id="9091b-120">This grammar means any string that starts with a letter and is followed by one or more underscore/letter/digit.</span></span>  
  
<span data-ttu-id="9091b-121">`[:IsLetter:]`znamená libovolný znak Unicode, který je zařazený do kategorie jako písmeno kódování Unicode.</span><span class="sxs-lookup"><span data-stu-id="9091b-121">`[:IsLetter:]` means any Unicode character that is categorized as a Unicode letter.</span></span> <span data-ttu-id="9091b-122">`System.Char.IsLetter(c)`Vrátí `true` Pokud `c` písmeno kódování Unicode.</span><span class="sxs-lookup"><span data-stu-id="9091b-122">`System.Char.IsLetter(c)` returns `true` if `c` is a Unicode letter.</span></span>  
  
<span data-ttu-id="9091b-123">`[:IsDigit:]`znamená libovolný znak Unicode, který je zařazený do kategorie jako desítková číslice.</span><span class="sxs-lookup"><span data-stu-id="9091b-123">`[:IsDigit:]` means any Unicode character that is categorized as a decimal digit.</span></span> <span data-ttu-id="9091b-124">`System.Char.IsDigit(c)`Vrátí `true` Pokud `c` je číslice kódování Unicode.</span><span class="sxs-lookup"><span data-stu-id="9091b-124">`System.Char.IsDigit(c)` returns `true` if `c` is a Unicode digit.</span></span>  
  
<span data-ttu-id="9091b-125">A `<regular_identifier>` nemůže být rezervované klíčové slovo.</span><span class="sxs-lookup"><span data-stu-id="9091b-125">A `<regular_identifier>` cannot be a reserved keyword.</span></span>  
  
<span data-ttu-id="9091b-126">`<delimited_identifier>`je řetězec, který je uzavřena s levé nebo pravé hranaté závorky ([]).</span><span class="sxs-lookup"><span data-stu-id="9091b-126">`<delimited_identifier>` is any string that is enclosed with left/right square brackets ([]).</span></span> <span data-ttu-id="9091b-127">Pravou hranatou závorku je reprezentován jako dvě pravé hranaté závorky.</span><span class="sxs-lookup"><span data-stu-id="9091b-127">A right square bracket is represented as two right square brackets.</span></span> <span data-ttu-id="9091b-128">Následují příklady `<delimited_identifier>`:</span><span class="sxs-lookup"><span data-stu-id="9091b-128">The following are examples of `<delimited_identifier>`:</span></span>  
  
```  
[Property With Space]  
[HR-EmployeeID]  
  
```  
  
<span data-ttu-id="9091b-129">`<quoted_identifier>`je řetězec, který je uzavřena dvojitých uvozovek nahoře.</span><span class="sxs-lookup"><span data-stu-id="9091b-129">`<quoted_identifier>` is any string that is enclosed with double quotation marks.</span></span> <span data-ttu-id="9091b-130">Dvojité uvozovky v identifikátoru je reprezentován jako dva znaky uvozovek.</span><span class="sxs-lookup"><span data-stu-id="9091b-130">A double quotation mark in identifier is represented as two double quotation marks.</span></span> <span data-ttu-id="9091b-131">Není doporučeno použít identifikátory v uvozovkách, protože můžete snadno Nezaměňovat s řetězcová konstanta.</span><span class="sxs-lookup"><span data-stu-id="9091b-131">It is not recommended to use quoted identifiers because it can easily be confused with a string constant.</span></span> <span data-ttu-id="9091b-132">Pokud je to možné použijte s oddělovači identifikátor.</span><span class="sxs-lookup"><span data-stu-id="9091b-132">Use a delimited identifier if possible.</span></span> <span data-ttu-id="9091b-133">Tady je příklad `<quoted_identifier>`:</span><span class="sxs-lookup"><span data-stu-id="9091b-133">The following is an example of `<quoted_identifier>`:</span></span>  
  
```  
"Contoso & Northwind"  
```  
  
## <a name="pattern"></a><span data-ttu-id="9091b-134">vzor</span><span class="sxs-lookup"><span data-stu-id="9091b-134">pattern</span></span>  
  
```  
<pattern> ::=  
      <expression>  
```  
  
### <a name="remarks"></a><span data-ttu-id="9091b-135">Poznámky</span><span class="sxs-lookup"><span data-stu-id="9091b-135">Remarks</span></span>
  
<span data-ttu-id="9091b-136">`<pattern>`musí být výraz, který se vyhodnotí jako řetězec.</span><span class="sxs-lookup"><span data-stu-id="9091b-136">`<pattern>` must be an expression that is evaluated as a string.</span></span> <span data-ttu-id="9091b-137">Použije se jako vzor pro operátor LIKE.</span><span class="sxs-lookup"><span data-stu-id="9091b-137">It is used as a pattern for the LIKE operator.</span></span>      <span data-ttu-id="9091b-138">Může obsahovat následující znaky:</span><span class="sxs-lookup"><span data-stu-id="9091b-138">It can contain the following wildcard characters:</span></span>  
  
-   <span data-ttu-id="9091b-139">`%`: Řetězec nula nebo více znaků.</span><span class="sxs-lookup"><span data-stu-id="9091b-139">`%`:  Any string of zero or more characters.</span></span>  
  
-   <span data-ttu-id="9091b-140">`_`: Jakémukoliv jedinému znaku.</span><span class="sxs-lookup"><span data-stu-id="9091b-140">`_`: Any single character.</span></span>  
  
## <a name="escapechar"></a><span data-ttu-id="9091b-141">escape_char</span><span class="sxs-lookup"><span data-stu-id="9091b-141">escape_char</span></span>  
  
```  
<escape_char> ::=  
      <expression>  
```  
  
### <a name="remarks"></a><span data-ttu-id="9091b-142">Poznámky</span><span class="sxs-lookup"><span data-stu-id="9091b-142">Remarks</span></span>  

<span data-ttu-id="9091b-143">`<escape_char>`musí být výraz, který se vyhodnotí jako řetězec o délce 1.</span><span class="sxs-lookup"><span data-stu-id="9091b-143">`<escape_char>` must be an expression that is evaluated as a string of length 1.</span></span> <span data-ttu-id="9091b-144">Slouží jako řídicí znak pro operátor LIKE.</span><span class="sxs-lookup"><span data-stu-id="9091b-144">It is used as an escape character for the LIKE operator.</span></span>  
  
 <span data-ttu-id="9091b-145">Například `property LIKE 'ABC\%' ESCAPE '\'` odpovídá `ABC%` místo řetězec, který začíná `ABC`.</span><span class="sxs-lookup"><span data-stu-id="9091b-145">For example, `property LIKE 'ABC\%' ESCAPE '\'` matches `ABC%` rather than a string that starts with `ABC`.</span></span>  
  
## <a name="constant"></a><span data-ttu-id="9091b-146">Konstantní</span><span class="sxs-lookup"><span data-stu-id="9091b-146">constant</span></span>  
  
```  
<constant> ::=  
      <integer_constant> | <decimal_constant> | <approximate_number_constant> | <boolean_constant> | NULL  
```  
  
### <a name="arguments"></a><span data-ttu-id="9091b-147">Argumenty</span><span class="sxs-lookup"><span data-stu-id="9091b-147">Arguments</span></span>  
  
-   <span data-ttu-id="9091b-148">`<integer_constant>`je řetězec čísel, která se nenacházejí v uvozovkách a neobsahují desetinných míst.</span><span class="sxs-lookup"><span data-stu-id="9091b-148">`<integer_constant>` is a string of numbers that are not enclosed in quotation marks and do not contain decimal points.</span></span> <span data-ttu-id="9091b-149">Hodnoty jsou uloženy jako `System.Int64` interně a postupujte podle stejného rozsahu.</span><span class="sxs-lookup"><span data-stu-id="9091b-149">The values are stored as `System.Int64` internally, and follow the same range.</span></span>  
  
     <span data-ttu-id="9091b-150">Toto jsou příklady dlouho konstanty:</span><span class="sxs-lookup"><span data-stu-id="9091b-150">These are examples of long constants:</span></span>  
  
    ```  
    1894  
    2  
    ```  
  
-   <span data-ttu-id="9091b-151">`<decimal_constant>`je řetězec čísel, která se nenacházejí v uvozovkách a obsahovat desetinné čárky.</span><span class="sxs-lookup"><span data-stu-id="9091b-151">`<decimal_constant>` is a string of numbers that are not enclosed in quotation marks, and contain a decimal point.</span></span> <span data-ttu-id="9091b-152">Hodnoty jsou uloženy jako `System.Double` interně a postupujte podle stejné rozsah nebo přesnosti.</span><span class="sxs-lookup"><span data-stu-id="9091b-152">The values are stored as `System.Double` internally, and follow the same range/precision.</span></span>  
  
     <span data-ttu-id="9091b-153">V budoucí verzi, může být tento číslo uložené v na jiný datový typ pro podporu přesné číslo sémantiku, takže byste neměli spoléhat na skutečnost, základní datový typ je `System.Double` pro `<decimal_constant>`.</span><span class="sxs-lookup"><span data-stu-id="9091b-153">In a future version, this number might be stored in a different data type to support exact number semantics, so you should not rely on the fact the underlying data type is `System.Double` for `<decimal_constant>`.</span></span>  
  
     <span data-ttu-id="9091b-154">Následují příklady decimal konstant:</span><span class="sxs-lookup"><span data-stu-id="9091b-154">The following are examples of decimal constants:</span></span>  
  
    ```  
    1894.1204  
    2.0  
    ```  
  
-   <span data-ttu-id="9091b-155">`<approximate_number_constant>`je číslo napsaných v exponenciální notace.</span><span class="sxs-lookup"><span data-stu-id="9091b-155">`<approximate_number_constant>` is a number written in scientific notation.</span></span> <span data-ttu-id="9091b-156">Hodnoty jsou uloženy jako `System.Double` interně a postupujte podle stejné rozsah nebo přesnosti.</span><span class="sxs-lookup"><span data-stu-id="9091b-156">The values are stored as `System.Double` internally, and follow the same range/precision.</span></span> <span data-ttu-id="9091b-157">Následují příklady přibližnou číselné konstanty:</span><span class="sxs-lookup"><span data-stu-id="9091b-157">The following are examples of approximate number constants:</span></span>  
  
    ```  
    101.5E5  
    0.5E-2  
    ```  
  
## <a name="booleanconstant"></a><span data-ttu-id="9091b-158">boolean_constant</span><span class="sxs-lookup"><span data-stu-id="9091b-158">boolean_constant</span></span>  
  
```  
<boolean_constant> :=  
      TRUE | FALSE  
```  
  
### <a name="remarks"></a><span data-ttu-id="9091b-159">Poznámky</span><span class="sxs-lookup"><span data-stu-id="9091b-159">Remarks</span></span>  

<span data-ttu-id="9091b-160">Logická hodnota konstanty jsou reprezentované pomocí klíčová slova **TRUE** nebo **FALSE**.</span><span class="sxs-lookup"><span data-stu-id="9091b-160">Boolean constants are represented by the keywords **TRUE** or **FALSE**.</span></span> <span data-ttu-id="9091b-161">Hodnoty jsou uloženy jako `System.Boolean`.</span><span class="sxs-lookup"><span data-stu-id="9091b-161">The values are stored as `System.Boolean`.</span></span>  
  
## <a name="stringconstant"></a><span data-ttu-id="9091b-162">string_constant</span><span class="sxs-lookup"><span data-stu-id="9091b-162">string_constant</span></span>  
  
```  
<string_constant>  
```  
  
### <a name="remarks"></a><span data-ttu-id="9091b-163">Poznámky</span><span class="sxs-lookup"><span data-stu-id="9091b-163">Remarks</span></span>  

<span data-ttu-id="9091b-164">Řetězcové konstanty jsou uzavřené v jednoduchých uvozovkách a zahrnují všechny platné znaky kódování Unicode.</span><span class="sxs-lookup"><span data-stu-id="9091b-164">String constants are enclosed in single quotation marks and include any valid Unicode characters.</span></span> <span data-ttu-id="9091b-165">Jednoduché uvozovky, vložené do řetězcová konstanta je reprezentována jako dvě jednoduché uvozovky.</span><span class="sxs-lookup"><span data-stu-id="9091b-165">A single quotation mark embedded in a string constant is represented as two single quotation marks.</span></span>  
  
## <a name="function"></a><span data-ttu-id="9091b-166">Funkce</span><span class="sxs-lookup"><span data-stu-id="9091b-166">function</span></span>  
  
```  
<function> :=  
      newid() |  
      property(name) | p(name)  
```  
  
### <a name="remarks"></a><span data-ttu-id="9091b-167">Poznámky</span><span class="sxs-lookup"><span data-stu-id="9091b-167">Remarks</span></span>
  
<span data-ttu-id="9091b-168">`newid()` Funkce vrátí **System.Guid** vygenerované `System.Guid.NewGuid()` metoda.</span><span class="sxs-lookup"><span data-stu-id="9091b-168">The `newid()` function returns a **System.Guid** generated by the `System.Guid.NewGuid()` method.</span></span>  
  
<span data-ttu-id="9091b-169">`property(name)` Funkce vrátí hodnotu vlastnosti odkazuje `name`.</span><span class="sxs-lookup"><span data-stu-id="9091b-169">The `property(name)` function returns the value of the property referenced by `name`.</span></span> <span data-ttu-id="9091b-170">`name` Hodnota může být libovolný platný výraz, který vrací řetězcovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9091b-170">The `name` value can be any valid expression that returns a string value.</span></span>  
  
## <a name="considerations"></a><span data-ttu-id="9091b-171">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9091b-171">Considerations</span></span>
  
<span data-ttu-id="9091b-172">Vezměte v úvahu následující [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) sémantiku:</span><span class="sxs-lookup"><span data-stu-id="9091b-172">Consider the following [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) semantics:</span></span>  
  
-   <span data-ttu-id="9091b-173">Názvy vlastností se velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="9091b-173">Property names are case-insensitive.</span></span>  
  
-   <span data-ttu-id="9091b-174">Operátory podle jazyka C# implicitní převod sémantiku kdykoli je to možné.</span><span class="sxs-lookup"><span data-stu-id="9091b-174">Operators follow C# implicit conversion semantics whenever possible.</span></span>  
  
-   <span data-ttu-id="9091b-175">Vlastnosti systému jsou veřejné vlastnosti v [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) instance.</span><span class="sxs-lookup"><span data-stu-id="9091b-175">System properties are public properties exposed in [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) instances.</span></span>  
  
    <span data-ttu-id="9091b-176">Vezměte v úvahu následující `IS [NOT] NULL` sémantiku:</span><span class="sxs-lookup"><span data-stu-id="9091b-176">Consider the following `IS [NOT] NULL` semantics:</span></span>  
  
    -   <span data-ttu-id="9091b-177">`property IS NULL`vyhodnotí jako `true` Pokud vlastnost neexistuje nebo je hodnota vlastnosti `null`.</span><span class="sxs-lookup"><span data-stu-id="9091b-177">`property IS NULL` is evaluated as `true` if either the property doesn't exist or the property's value is `null`.</span></span>  
  
### <a name="property-evaluation-semantics"></a><span data-ttu-id="9091b-178">Sémantika vyhodnocení vlastnosti</span><span class="sxs-lookup"><span data-stu-id="9091b-178">Property evaluation semantics</span></span>  
  
-   <span data-ttu-id="9091b-179">Vyvolá pokus o vyhodnocení vlastnost neexistující systému [FilterException](/dotnet/api/microsoft.servicebus.messaging.filterexception) výjimka.</span><span class="sxs-lookup"><span data-stu-id="9091b-179">An attempt to evaluate a non-existent system property throws a [FilterException](/dotnet/api/microsoft.servicebus.messaging.filterexception) exception.</span></span>  
  
-   <span data-ttu-id="9091b-180">Vlastnost, která neexistuje interně vyhodnotí jako **neznámé**.</span><span class="sxs-lookup"><span data-stu-id="9091b-180">A property that does not exist is internally evaluated as **unknown**.</span></span>  
  
 <span data-ttu-id="9091b-181">Neznámý vyhodnocení v aritmetické operátory:</span><span class="sxs-lookup"><span data-stu-id="9091b-181">Unknown evaluation in arithmetic operators:</span></span>  
  
-   <span data-ttu-id="9091b-182">Pro binární operátory, pokud zadaný levé nebo pravé straně operandy vyhodnotí jako **neznámé**, potom je **neznámé**.</span><span class="sxs-lookup"><span data-stu-id="9091b-182">For binary operators, if either the left and/or right side of operands is evaluated as **unknown**, then the result is **unknown**.</span></span>  
  
-   <span data-ttu-id="9091b-183">Pro unární operátory, pokud operand vyhodnotí jako **neznámé**, potom je **neznámé**.</span><span class="sxs-lookup"><span data-stu-id="9091b-183">For unary operators, if an operand is evaluated as **unknown**, then the result is **unknown**.</span></span>  
  
 <span data-ttu-id="9091b-184">Neznámý vyhodnocení v binární porovnání operátory:</span><span class="sxs-lookup"><span data-stu-id="9091b-184">Unknown evaluation in binary comparison operators:</span></span>  
  
-   <span data-ttu-id="9091b-185">Pokud v levé nebo pravé straně operandy vyhodnotí jako **neznámé**, potom je **neznámé**.</span><span class="sxs-lookup"><span data-stu-id="9091b-185">If either the left and/or right side of operands is evaluated as **unknown**, then the result is **unknown**.</span></span>  
  
 <span data-ttu-id="9091b-186">Neznámý vyhodnocení v `[NOT] LIKE`:</span><span class="sxs-lookup"><span data-stu-id="9091b-186">Unknown evaluation in `[NOT] LIKE`:</span></span>  
  
-   <span data-ttu-id="9091b-187">Pokud operandem any vyhodnotí jako **neznámé**, potom je **neznámé**.</span><span class="sxs-lookup"><span data-stu-id="9091b-187">If any operand is evaluated as **unknown**, then the result is **unknown**.</span></span>  
  
 <span data-ttu-id="9091b-188">Neznámý vyhodnocení v `[NOT] IN`:</span><span class="sxs-lookup"><span data-stu-id="9091b-188">Unknown evaluation in `[NOT] IN`:</span></span>  
  
-   <span data-ttu-id="9091b-189">Pokud je levý operand vyhodnoceny jako **neznámé**, potom je **neznámé**.</span><span class="sxs-lookup"><span data-stu-id="9091b-189">If the left operand is evaluated as **unknown**, then the result is **unknown**.</span></span>  
  
 <span data-ttu-id="9091b-190">Neznámý vyhodnocení v **a** operátor:</span><span class="sxs-lookup"><span data-stu-id="9091b-190">Unknown evaluation in **AND** operator:</span></span>  
  
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
  
 <span data-ttu-id="9091b-191">Neznámý vyhodnocení v **nebo** operátor:</span><span class="sxs-lookup"><span data-stu-id="9091b-191">Unknown evaluation in **OR** operator:</span></span>  
  
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
  
### <a name="operator-binding-semantics"></a><span data-ttu-id="9091b-192">Operátor sémantiku vazby</span><span class="sxs-lookup"><span data-stu-id="9091b-192">Operator binding semantics</span></span>
  
-   <span data-ttu-id="9091b-193">Operátory porovnání jako `>`, `>=`, `<`, `<=`, `!=`, a `=` použijte stejnou sémantiku jako vazby ve povýšení typ dat a implicitní převody operátor C#.</span><span class="sxs-lookup"><span data-stu-id="9091b-193">Comparison operators such as `>`, `>=`, `<`, `<=`, `!=`, and `=` follow the same semantics as the C# operator binding in data type promotions and implicit conversions.</span></span>  
  
-   <span data-ttu-id="9091b-194">Aritmetické operátory jako `+`, `-`, `*`, `/`, a `%` použijte stejnou sémantiku jako vazby ve povýšení typ dat a implicitní převody operátor C#.</span><span class="sxs-lookup"><span data-stu-id="9091b-194">Arithmetic operators such as `+`, `-`, `*`, `/`, and `%` follow the same semantics as the C# operator binding in data type promotions and implicit conversions.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9091b-195">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9091b-195">Next steps</span></span>

- [<span data-ttu-id="9091b-196">SQLFilter – třída</span><span class="sxs-lookup"><span data-stu-id="9091b-196">SQLFilter class</span></span>](/dotnet/api/microsoft.servicebus.messaging.sqlfilter)
- [<span data-ttu-id="9091b-197">SQLRuleAction – třída</span><span class="sxs-lookup"><span data-stu-id="9091b-197">SQLRuleAction class</span></span>](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction)