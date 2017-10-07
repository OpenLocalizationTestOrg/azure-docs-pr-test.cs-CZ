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
# <a name="sqlfilter-syntax"></a><span data-ttu-id="6dac0-103">Syntaxe SQLFilter</span><span class="sxs-lookup"><span data-stu-id="6dac0-103">SQLFilter syntax</span></span>

<span data-ttu-id="6dac0-104">A *SqlFilter* je instance hello [SqlFilter třída](/dotnet/api/microsoft.servicebus.messaging.sqlfilter)a představuje výraz filtru na základě jazyka SQL, který se vyhodnotí proti [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span><span class="sxs-lookup"><span data-stu-id="6dac0-104">A *SqlFilter* is an instance of hello [SqlFilter class](/dotnet/api/microsoft.servicebus.messaging.sqlfilter), and represents a SQL language-based filter expression that is evaluated against a [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span></span> <span data-ttu-id="6dac0-105">SqlFilter podporuje podmnožinu hello SQL 92 standard.</span><span class="sxs-lookup"><span data-stu-id="6dac0-105">A SqlFilter supports a subset of hello SQL-92 standard.</span></span>  
  
 <span data-ttu-id="6dac0-106">Toto téma obsahuje podrobnosti o SqlFilter gramatika.</span><span class="sxs-lookup"><span data-stu-id="6dac0-106">This topic lists details about SqlFilter grammar.</span></span>  
  
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
  
## <a name="arguments"></a><span data-ttu-id="6dac0-107">Argumenty</span><span class="sxs-lookup"><span data-stu-id="6dac0-107">Arguments</span></span>  
  
-   <span data-ttu-id="6dac0-108">`<scope>`je volitelný řetězec označující hello oboru hello `<property_name>`.</span><span class="sxs-lookup"><span data-stu-id="6dac0-108">`<scope>` is an optional string indicating hello scope of hello `<property_name>`.</span></span> <span data-ttu-id="6dac0-109">Platné hodnoty jsou `sys` nebo `user`.</span><span class="sxs-lookup"><span data-stu-id="6dac0-109">Valid values are `sys` or `user`.</span></span> <span data-ttu-id="6dac0-110">Hello `sys` hodnota určuje rozsah systémů kde `<property_name>` je název veřejné vlastnosti hello [BrokeredMessage třída](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span><span class="sxs-lookup"><span data-stu-id="6dac0-110">hello `sys` value indicates system scope where `<property_name>` is a public property name of hello [BrokeredMessage class](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).</span></span> <span data-ttu-id="6dac0-111">`user`označuje oboru uživatele kde `<property_name>` je klíč hello [BrokeredMessage třída](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) slovníku.</span><span class="sxs-lookup"><span data-stu-id="6dac0-111">`user` indicates user scope where `<property_name>` is a key of hello [BrokeredMessage class](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) dictionary.</span></span> <span data-ttu-id="6dac0-112">`user`rozsah je výchozí obor hello, pokud `<scope>` není zadán.</span><span class="sxs-lookup"><span data-stu-id="6dac0-112">`user` scope is hello default scope if `<scope>` is not specified.</span></span>  
  
## <a name="remarks"></a><span data-ttu-id="6dac0-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="6dac0-113">Remarks</span></span>

<span data-ttu-id="6dac0-114">Tooaccess pokusu o systému neexistující vlastnost je k chybě při vlastnost uživatele pokusu o tooaccess neexistující není chyba.</span><span class="sxs-lookup"><span data-stu-id="6dac0-114">An attempt tooaccess a non-existent system property is an error, while an attempt tooaccess a non-existent user property is not an error.</span></span> <span data-ttu-id="6dac0-115">Místo toho vlastnost uživatele neexistující interně vyhodnotí jako neznámou hodnotou.</span><span class="sxs-lookup"><span data-stu-id="6dac0-115">Instead, a non-existent user property is internally evaluated as an unknown value.</span></span> <span data-ttu-id="6dac0-116">Neznámá hodnota považuje speciálně během vyhodnocení operátor.</span><span class="sxs-lookup"><span data-stu-id="6dac0-116">An unknown value is treated specially during operator evaluation.</span></span>  
  
## <a name="propertyname"></a><span data-ttu-id="6dac0-117">%{Property_Name/</span><span class="sxs-lookup"><span data-stu-id="6dac0-117">property_name</span></span>  
  
```  
<property_name> ::=  
     <identifier>  
     | <delimited_identifier>  
  
<identifier> ::=  
     <regular_identifier> | <quoted_identifier> | <delimited_identifier>  
  
```  
  
### <a name="arguments"></a><span data-ttu-id="6dac0-118">Argumenty</span><span class="sxs-lookup"><span data-stu-id="6dac0-118">Arguments</span></span>  

 <span data-ttu-id="6dac0-119">`<regular_identifier>`je řetězec reprezentována hello následující regulární výraz:</span><span class="sxs-lookup"><span data-stu-id="6dac0-119">`<regular_identifier>` is a string represented by hello following regular expression:</span></span>  
  
```  
[[:IsLetter:]][_[:IsLetter:][:IsDigit:]]*  
```  
  
<span data-ttu-id="6dac0-120">Tato gramatika znamená libovolný řetězec, který začíná písmenem a následuje jeden nebo více podtržítko nebo písmeno nebo číslice.</span><span class="sxs-lookup"><span data-stu-id="6dac0-120">This grammar means any string that starts with a letter and is followed by one or more underscore/letter/digit.</span></span>  
  
<span data-ttu-id="6dac0-121">`[:IsLetter:]`znamená libovolný znak Unicode, který je zařazený do kategorie jako písmeno kódování Unicode.</span><span class="sxs-lookup"><span data-stu-id="6dac0-121">`[:IsLetter:]` means any Unicode character that is categorized as a Unicode letter.</span></span> <span data-ttu-id="6dac0-122">`System.Char.IsLetter(c)`Vrátí `true` Pokud `c` písmeno kódování Unicode.</span><span class="sxs-lookup"><span data-stu-id="6dac0-122">`System.Char.IsLetter(c)` returns `true` if `c` is a Unicode letter.</span></span>  
  
<span data-ttu-id="6dac0-123">`[:IsDigit:]`znamená libovolný znak Unicode, který je zařazený do kategorie jako desítková číslice.</span><span class="sxs-lookup"><span data-stu-id="6dac0-123">`[:IsDigit:]` means any Unicode character that is categorized as a decimal digit.</span></span> <span data-ttu-id="6dac0-124">`System.Char.IsDigit(c)`Vrátí `true` Pokud `c` je číslice kódování Unicode.</span><span class="sxs-lookup"><span data-stu-id="6dac0-124">`System.Char.IsDigit(c)` returns `true` if `c` is a Unicode digit.</span></span>  
  
<span data-ttu-id="6dac0-125">A `<regular_identifier>` nemůže být rezervované klíčové slovo.</span><span class="sxs-lookup"><span data-stu-id="6dac0-125">A `<regular_identifier>` cannot be a reserved keyword.</span></span>  
  
<span data-ttu-id="6dac0-126">`<delimited_identifier>`je řetězec, který je uzavřena s levé nebo pravé hranaté závorky ([]).</span><span class="sxs-lookup"><span data-stu-id="6dac0-126">`<delimited_identifier>` is any string that is enclosed with left/right square brackets ([]).</span></span> <span data-ttu-id="6dac0-127">Pravou hranatou závorku je reprezentován jako dvě pravé hranaté závorky.</span><span class="sxs-lookup"><span data-stu-id="6dac0-127">A right square bracket is represented as two right square brackets.</span></span> <span data-ttu-id="6dac0-128">Hello Následují příklady `<delimited_identifier>`:</span><span class="sxs-lookup"><span data-stu-id="6dac0-128">hello following are examples of `<delimited_identifier>`:</span></span>  
  
```  
[Property With Space]  
[HR-EmployeeID]  
  
```  
  
<span data-ttu-id="6dac0-129">`<quoted_identifier>`je řetězec, který je uzavřena dvojitých uvozovek nahoře.</span><span class="sxs-lookup"><span data-stu-id="6dac0-129">`<quoted_identifier>` is any string that is enclosed with double quotation marks.</span></span> <span data-ttu-id="6dac0-130">Dvojité uvozovky v identifikátoru je reprezentován jako dva znaky uvozovek.</span><span class="sxs-lookup"><span data-stu-id="6dac0-130">A double quotation mark in identifier is represented as two double quotation marks.</span></span> <span data-ttu-id="6dac0-131">Není doporučeno, protože můžete snadno Nezaměňovat s konstantní řetězec v uvozovkách toouse identifikátory.</span><span class="sxs-lookup"><span data-stu-id="6dac0-131">It is not recommended toouse quoted identifiers because it can easily be confused with a string constant.</span></span> <span data-ttu-id="6dac0-132">Pokud je to možné použijte s oddělovači identifikátor.</span><span class="sxs-lookup"><span data-stu-id="6dac0-132">Use a delimited identifier if possible.</span></span> <span data-ttu-id="6dac0-133">Hello tady je příklad `<quoted_identifier>`:</span><span class="sxs-lookup"><span data-stu-id="6dac0-133">hello following is an example of `<quoted_identifier>`:</span></span>  
  
```  
"Contoso & Northwind"  
```  
  
## <a name="pattern"></a><span data-ttu-id="6dac0-134">vzor</span><span class="sxs-lookup"><span data-stu-id="6dac0-134">pattern</span></span>  
  
```  
<pattern> ::=  
      <expression>  
```  
  
### <a name="remarks"></a><span data-ttu-id="6dac0-135">Poznámky</span><span class="sxs-lookup"><span data-stu-id="6dac0-135">Remarks</span></span>
  
<span data-ttu-id="6dac0-136">`<pattern>`musí být výraz, který se vyhodnotí jako řetězec.</span><span class="sxs-lookup"><span data-stu-id="6dac0-136">`<pattern>` must be an expression that is evaluated as a string.</span></span> <span data-ttu-id="6dac0-137">Použije se jako vzor pro hello jako operátor.</span><span class="sxs-lookup"><span data-stu-id="6dac0-137">It is used as a pattern for hello LIKE operator.</span></span>      <span data-ttu-id="6dac0-138">Může obsahovat hello následující zástupné znaky:</span><span class="sxs-lookup"><span data-stu-id="6dac0-138">It can contain hello following wildcard characters:</span></span>  
  
-   <span data-ttu-id="6dac0-139">`%`: Řetězec nula nebo více znaků.</span><span class="sxs-lookup"><span data-stu-id="6dac0-139">`%`:  Any string of zero or more characters.</span></span>  
  
-   <span data-ttu-id="6dac0-140">`_`: Jakémukoliv jedinému znaku.</span><span class="sxs-lookup"><span data-stu-id="6dac0-140">`_`: Any single character.</span></span>  
  
## <a name="escapechar"></a><span data-ttu-id="6dac0-141">escape_char</span><span class="sxs-lookup"><span data-stu-id="6dac0-141">escape_char</span></span>  
  
```  
<escape_char> ::=  
      <expression>  
```  
  
### <a name="remarks"></a><span data-ttu-id="6dac0-142">Poznámky</span><span class="sxs-lookup"><span data-stu-id="6dac0-142">Remarks</span></span>  

<span data-ttu-id="6dac0-143">`<escape_char>`musí být výraz, který se vyhodnotí jako řetězec o délce 1.</span><span class="sxs-lookup"><span data-stu-id="6dac0-143">`<escape_char>` must be an expression that is evaluated as a string of length 1.</span></span> <span data-ttu-id="6dac0-144">Slouží jako řídicí znak pro hello jako operátor.</span><span class="sxs-lookup"><span data-stu-id="6dac0-144">It is used as an escape character for hello LIKE operator.</span></span>  
  
 <span data-ttu-id="6dac0-145">Například `property LIKE 'ABC\%' ESCAPE '\'` odpovídá `ABC%` místo řetězec, který začíná `ABC`.</span><span class="sxs-lookup"><span data-stu-id="6dac0-145">For example, `property LIKE 'ABC\%' ESCAPE '\'` matches `ABC%` rather than a string that starts with `ABC`.</span></span>  
  
## <a name="constant"></a><span data-ttu-id="6dac0-146">Konstantní</span><span class="sxs-lookup"><span data-stu-id="6dac0-146">constant</span></span>  
  
```  
<constant> ::=  
      <integer_constant> | <decimal_constant> | <approximate_number_constant> | <boolean_constant> | NULL  
```  
  
### <a name="arguments"></a><span data-ttu-id="6dac0-147">Argumenty</span><span class="sxs-lookup"><span data-stu-id="6dac0-147">Arguments</span></span>  
  
-   <span data-ttu-id="6dac0-148">`<integer_constant>`je řetězec čísel, která se nenacházejí v uvozovkách a neobsahují desetinných míst.</span><span class="sxs-lookup"><span data-stu-id="6dac0-148">`<integer_constant>` is a string of numbers that are not enclosed in quotation marks and do not contain decimal points.</span></span> <span data-ttu-id="6dac0-149">Hello hodnoty se uloží jako `System.Int64` interně, a postupujte podle hello stejný rozsah.</span><span class="sxs-lookup"><span data-stu-id="6dac0-149">hello values are stored as `System.Int64` internally, and follow hello same range.</span></span>  
  
     <span data-ttu-id="6dac0-150">Toto jsou příklady dlouho konstanty:</span><span class="sxs-lookup"><span data-stu-id="6dac0-150">These are examples of long constants:</span></span>  
  
    ```  
    1894  
    2  
    ```  
  
-   <span data-ttu-id="6dac0-151">`<decimal_constant>`je řetězec čísel, která se nenacházejí v uvozovkách a obsahovat desetinné čárky.</span><span class="sxs-lookup"><span data-stu-id="6dac0-151">`<decimal_constant>` is a string of numbers that are not enclosed in quotation marks, and contain a decimal point.</span></span> <span data-ttu-id="6dac0-152">Hello hodnoty se uloží jako `System.Double` interně a postupujte podle hello stejný rozsah nebo přesnosti.</span><span class="sxs-lookup"><span data-stu-id="6dac0-152">hello values are stored as `System.Double` internally, and follow hello same range/precision.</span></span>  
  
     <span data-ttu-id="6dac0-153">V budoucí verzi, toto číslo může být uložena v jiný datový toosupport přesné číslo Sémantika typu, takže byste neměli spoléhat na hello fakt hello základní datový typ je `System.Double` pro `<decimal_constant>`.</span><span class="sxs-lookup"><span data-stu-id="6dac0-153">In a future version, this number might be stored in a different data type toosupport exact number semantics, so you should not rely on hello fact hello underlying data type is `System.Double` for `<decimal_constant>`.</span></span>  
  
     <span data-ttu-id="6dac0-154">Hello Následují příklady decimal konstanty:</span><span class="sxs-lookup"><span data-stu-id="6dac0-154">hello following are examples of decimal constants:</span></span>  
  
    ```  
    1894.1204  
    2.0  
    ```  
  
-   <span data-ttu-id="6dac0-155">`<approximate_number_constant>`je číslo napsaných v exponenciální notace.</span><span class="sxs-lookup"><span data-stu-id="6dac0-155">`<approximate_number_constant>` is a number written in scientific notation.</span></span> <span data-ttu-id="6dac0-156">Hello hodnoty se uloží jako `System.Double` interně a postupujte podle hello stejný rozsah nebo přesnosti.</span><span class="sxs-lookup"><span data-stu-id="6dac0-156">hello values are stored as `System.Double` internally, and follow hello same range/precision.</span></span> <span data-ttu-id="6dac0-157">Hello Následují příklady přibližnou číselné konstanty:</span><span class="sxs-lookup"><span data-stu-id="6dac0-157">hello following are examples of approximate number constants:</span></span>  
  
    ```  
    101.5E5  
    0.5E-2  
    ```  
  
## <a name="booleanconstant"></a><span data-ttu-id="6dac0-158">boolean_constant</span><span class="sxs-lookup"><span data-stu-id="6dac0-158">boolean_constant</span></span>  
  
```  
<boolean_constant> :=  
      TRUE | FALSE  
```  
  
### <a name="remarks"></a><span data-ttu-id="6dac0-159">Poznámky</span><span class="sxs-lookup"><span data-stu-id="6dac0-159">Remarks</span></span>  

<span data-ttu-id="6dac0-160">Logická hodnota konstanty jsou reprezentované pomocí klíčových slov hello **TRUE** nebo **FALSE**.</span><span class="sxs-lookup"><span data-stu-id="6dac0-160">Boolean constants are represented by hello keywords **TRUE** or **FALSE**.</span></span> <span data-ttu-id="6dac0-161">Hello hodnoty se uloží jako `System.Boolean`.</span><span class="sxs-lookup"><span data-stu-id="6dac0-161">hello values are stored as `System.Boolean`.</span></span>  
  
## <a name="stringconstant"></a><span data-ttu-id="6dac0-162">string_constant</span><span class="sxs-lookup"><span data-stu-id="6dac0-162">string_constant</span></span>  
  
```  
<string_constant>  
```  
  
### <a name="remarks"></a><span data-ttu-id="6dac0-163">Poznámky</span><span class="sxs-lookup"><span data-stu-id="6dac0-163">Remarks</span></span>  

<span data-ttu-id="6dac0-164">Řetězcové konstanty jsou uzavřené v jednoduchých uvozovkách a zahrnují všechny platné znaky kódování Unicode.</span><span class="sxs-lookup"><span data-stu-id="6dac0-164">String constants are enclosed in single quotation marks and include any valid Unicode characters.</span></span> <span data-ttu-id="6dac0-165">Jednoduché uvozovky, vložené do řetězcová konstanta je reprezentována jako dvě jednoduché uvozovky.</span><span class="sxs-lookup"><span data-stu-id="6dac0-165">A single quotation mark embedded in a string constant is represented as two single quotation marks.</span></span>  
  
## <a name="function"></a><span data-ttu-id="6dac0-166">Funkce</span><span class="sxs-lookup"><span data-stu-id="6dac0-166">function</span></span>  
  
```  
<function> :=  
      newid() |  
      property(name) | p(name)  
```  
  
### <a name="remarks"></a><span data-ttu-id="6dac0-167">Poznámky</span><span class="sxs-lookup"><span data-stu-id="6dac0-167">Remarks</span></span>
  
<span data-ttu-id="6dac0-168">Hello `newid()` funkce vrátí **System.Guid** generované hello `System.Guid.NewGuid()` metoda.</span><span class="sxs-lookup"><span data-stu-id="6dac0-168">hello `newid()` function returns a **System.Guid** generated by hello `System.Guid.NewGuid()` method.</span></span>  
  
<span data-ttu-id="6dac0-169">Hello `property(name)` funkce vrátí hodnotu hello hello vlastnosti odkazuje `name`.</span><span class="sxs-lookup"><span data-stu-id="6dac0-169">hello `property(name)` function returns hello value of hello property referenced by `name`.</span></span> <span data-ttu-id="6dac0-170">Hello `name` hodnota může být libovolný platný výraz, který vrací řetězcovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="6dac0-170">hello `name` value can be any valid expression that returns a string value.</span></span>  
  
## <a name="considerations"></a><span data-ttu-id="6dac0-171">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6dac0-171">Considerations</span></span>
  
<span data-ttu-id="6dac0-172">Vezměte v úvahu následující hello [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) sémantiku:</span><span class="sxs-lookup"><span data-stu-id="6dac0-172">Consider hello following [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) semantics:</span></span>  
  
-   <span data-ttu-id="6dac0-173">Názvy vlastností se velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="6dac0-173">Property names are case-insensitive.</span></span>  
  
-   <span data-ttu-id="6dac0-174">Operátory podle jazyka C# implicitní převod sémantiku kdykoli je to možné.</span><span class="sxs-lookup"><span data-stu-id="6dac0-174">Operators follow C# implicit conversion semantics whenever possible.</span></span>  
  
-   <span data-ttu-id="6dac0-175">Vlastnosti systému jsou veřejné vlastnosti v [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) instance.</span><span class="sxs-lookup"><span data-stu-id="6dac0-175">System properties are public properties exposed in [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) instances.</span></span>  
  
    <span data-ttu-id="6dac0-176">Vezměte v úvahu následující hello `IS [NOT] NULL` sémantiku:</span><span class="sxs-lookup"><span data-stu-id="6dac0-176">Consider hello following `IS [NOT] NULL` semantics:</span></span>  
  
    -   <span data-ttu-id="6dac0-177">`property IS NULL`vyhodnotí jako `true` Pokud buď vlastnost hello nebude existovat nebo hello hodnota vlastnosti je `null`.</span><span class="sxs-lookup"><span data-stu-id="6dac0-177">`property IS NULL` is evaluated as `true` if either hello property doesn't exist or hello property's value is `null`.</span></span>  
  
### <a name="property-evaluation-semantics"></a><span data-ttu-id="6dac0-178">Sémantika vyhodnocení vlastnosti</span><span class="sxs-lookup"><span data-stu-id="6dac0-178">Property evaluation semantics</span></span>  
  
-   <span data-ttu-id="6dac0-179">Pokusu o tooevaluate systému neexistující vlastnost vyvolá [FilterException](/dotnet/api/microsoft.servicebus.messaging.filterexception) výjimka.</span><span class="sxs-lookup"><span data-stu-id="6dac0-179">An attempt tooevaluate a non-existent system property throws a [FilterException](/dotnet/api/microsoft.servicebus.messaging.filterexception) exception.</span></span>  
  
-   <span data-ttu-id="6dac0-180">Vlastnost, která neexistuje interně vyhodnotí jako **neznámé**.</span><span class="sxs-lookup"><span data-stu-id="6dac0-180">A property that does not exist is internally evaluated as **unknown**.</span></span>  
  
 <span data-ttu-id="6dac0-181">Neznámý vyhodnocení v aritmetické operátory:</span><span class="sxs-lookup"><span data-stu-id="6dac0-181">Unknown evaluation in arithmetic operators:</span></span>  
  
-   <span data-ttu-id="6dac0-182">Pro binární operátory, pokud buď hello levé nebo pravé straně operandy vyhodnotí jako **neznámé**, pak je výsledek hello **neznámé**.</span><span class="sxs-lookup"><span data-stu-id="6dac0-182">For binary operators, if either hello left and/or right side of operands is evaluated as **unknown**, then hello result is **unknown**.</span></span>  
  
-   <span data-ttu-id="6dac0-183">Pro unární operátory, pokud operand vyhodnotí jako **neznámé**, pak je výsledek hello **neznámé**.</span><span class="sxs-lookup"><span data-stu-id="6dac0-183">For unary operators, if an operand is evaluated as **unknown**, then hello result is **unknown**.</span></span>  
  
 <span data-ttu-id="6dac0-184">Neznámý vyhodnocení v binární porovnání operátory:</span><span class="sxs-lookup"><span data-stu-id="6dac0-184">Unknown evaluation in binary comparison operators:</span></span>  
  
-   <span data-ttu-id="6dac0-185">Pokud buď hello levé nebo pravé straně operandy vyhodnotí jako **neznámé**, pak je výsledek hello **neznámé**.</span><span class="sxs-lookup"><span data-stu-id="6dac0-185">If either hello left and/or right side of operands is evaluated as **unknown**, then hello result is **unknown**.</span></span>  
  
 <span data-ttu-id="6dac0-186">Neznámý vyhodnocení v `[NOT] LIKE`:</span><span class="sxs-lookup"><span data-stu-id="6dac0-186">Unknown evaluation in `[NOT] LIKE`:</span></span>  
  
-   <span data-ttu-id="6dac0-187">Pokud operandem any vyhodnotí jako **neznámé**, pak je výsledek hello **neznámé**.</span><span class="sxs-lookup"><span data-stu-id="6dac0-187">If any operand is evaluated as **unknown**, then hello result is **unknown**.</span></span>  
  
 <span data-ttu-id="6dac0-188">Neznámý vyhodnocení v `[NOT] IN`:</span><span class="sxs-lookup"><span data-stu-id="6dac0-188">Unknown evaluation in `[NOT] IN`:</span></span>  
  
-   <span data-ttu-id="6dac0-189">Pokud hello levý operand vyhodnotí jako **neznámé**, pak je výsledek hello **neznámé**.</span><span class="sxs-lookup"><span data-stu-id="6dac0-189">If hello left operand is evaluated as **unknown**, then hello result is **unknown**.</span></span>  
  
 <span data-ttu-id="6dac0-190">Neznámý vyhodnocení v **a** operátor:</span><span class="sxs-lookup"><span data-stu-id="6dac0-190">Unknown evaluation in **AND** operator:</span></span>  
  
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
  
 <span data-ttu-id="6dac0-191">Neznámý vyhodnocení v **nebo** operátor:</span><span class="sxs-lookup"><span data-stu-id="6dac0-191">Unknown evaluation in **OR** operator:</span></span>  
  
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
  
### <a name="operator-binding-semantics"></a><span data-ttu-id="6dac0-192">Operátor sémantiku vazby</span><span class="sxs-lookup"><span data-stu-id="6dac0-192">Operator binding semantics</span></span>
  
-   <span data-ttu-id="6dac0-193">Operátory porovnání jako `>`, `>=`, `<`, `<=`, `!=`, a `=` postupujte podle hello stejnou sémantiku jako operátor hello C# vazby v datech zadejte reklamními nabídkami a implicitní převody.</span><span class="sxs-lookup"><span data-stu-id="6dac0-193">Comparison operators such as `>`, `>=`, `<`, `<=`, `!=`, and `=` follow hello same semantics as hello C# operator binding in data type promotions and implicit conversions.</span></span>  
  
-   <span data-ttu-id="6dac0-194">Aritmetické operátory jako `+`, `-`, `*`, `/`, a `%` postupujte podle hello stejnou sémantiku jako operátor hello C# vazby v datech zadejte reklamními nabídkami a implicitní převody.</span><span class="sxs-lookup"><span data-stu-id="6dac0-194">Arithmetic operators such as `+`, `-`, `*`, `/`, and `%` follow hello same semantics as hello C# operator binding in data type promotions and implicit conversions.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6dac0-195">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6dac0-195">Next steps</span></span>

- [<span data-ttu-id="6dac0-196">SQLFilter – třída</span><span class="sxs-lookup"><span data-stu-id="6dac0-196">SQLFilter class</span></span>](/dotnet/api/microsoft.servicebus.messaging.sqlfilter)
- [<span data-ttu-id="6dac0-197">SQLRuleAction – třída</span><span class="sxs-lookup"><span data-stu-id="6dac0-197">SQLRuleAction class</span></span>](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction)