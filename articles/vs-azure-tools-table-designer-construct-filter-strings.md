---
title: "Vytváření řetězců filtru pro návrháře tabulky | Microsoft Docs"
description: "Vytváření řetězců filtru pro návrháře tabulky"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: a1a10ea1-687a-4ee1-a952-6b24c2fe1a22
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2016
ms.author: kraigb
ms.openlocfilehash: 069224d84462b4955912ce1462a65298a5acc04a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="constructing-filter-strings-for-the-table-designer"></a><span data-ttu-id="c4261-103">Vytváření řetězců filtru pro návrháře tabulky</span><span class="sxs-lookup"><span data-stu-id="c4261-103">Constructing Filter Strings for the Table Designer</span></span>
## <a name="overview"></a><span data-ttu-id="c4261-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="c4261-104">Overview</span></span>
<span data-ttu-id="c4261-105">Filtrování dat v Azure tabulky, který se zobrazí v sadě Visual Studio **návrháře tabulky**, můžete vytvořit řetězec filtru a zadejte do pole filtru.</span><span class="sxs-lookup"><span data-stu-id="c4261-105">To filter data in an Azure table that is displayed in the Visual Studio **Table Designer**, you construct a filter string and enter it into the filter field.</span></span> <span data-ttu-id="c4261-106">Řetězec syntaxe filtru je definována služby WCF Data Services a je podobná SQL klauzule WHERE, ale je odeslána do služby Table prostřednictvím požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="c4261-106">The filter string syntax is defined by the WCF Data Services and is similar to a SQL WHERE clause, but is sent to the Table service via an HTTP request.</span></span> <span data-ttu-id="c4261-107">**Návrháře tabulky** zpracovává správné kódování pro vás, tak k filtrování na hodnotu požadovanou vlastnost, můžete potřebovat pouze zadejte název vlastnosti, operátor porovnání, hodnotu pro kritéria a volitelně logická hodnota operátor do pole filtru.</span><span class="sxs-lookup"><span data-stu-id="c4261-107">The **Table Designer** handles the proper encoding for you, so to filter on a desired property value, you need only enter the property name, comparison operator, criteria value, and optionally, Boolean operator in the filter field.</span></span> <span data-ttu-id="c4261-108">Nemusíte zahrnovat možnost dotazu $filter, jako byste, pokud byly generuje adresu URL a dotazovat tabulky pomocí [úložiště referenční dokumentace rozhraní API REST služby](http://go.microsoft.com/fwlink/p/?LinkId=400447).</span><span class="sxs-lookup"><span data-stu-id="c4261-108">You do not need to include the $filter query option as you would if you were constructing a URL to query the table via the [Storage Services REST API Reference](http://go.microsoft.com/fwlink/p/?LinkId=400447).</span></span>

<span data-ttu-id="c4261-109">Služby WCF Data Services jsou založené na [protokol](http://go.microsoft.com/fwlink/p/?LinkId=214805) (OData).</span><span class="sxs-lookup"><span data-stu-id="c4261-109">The WCF Data Services are based on the [Open Data Protocol](http://go.microsoft.com/fwlink/p/?LinkId=214805) (OData).</span></span> <span data-ttu-id="c4261-110">Podrobnosti o možnost dotazu filter systému (**$filter**), najdete v článku [konvence prostředí OData pro identifikátor URI specifikace](http://go.microsoft.com/fwlink/p/?LinkId=214806).</span><span class="sxs-lookup"><span data-stu-id="c4261-110">For details on the filter system query option (**$filter**), see the [OData URI Conventions specification](http://go.microsoft.com/fwlink/p/?LinkId=214806).</span></span>

## <a name="comparison-operators"></a><span data-ttu-id="c4261-111">Operátory porovnání</span><span class="sxs-lookup"><span data-stu-id="c4261-111">Comparison Operators</span></span>
<span data-ttu-id="c4261-112">Pro všechny typy vlastností jsou podporovány následující logické operátory:</span><span class="sxs-lookup"><span data-stu-id="c4261-112">The following logical operators are supported for all property types:</span></span>

| <span data-ttu-id="c4261-113">Logický operátor</span><span class="sxs-lookup"><span data-stu-id="c4261-113">Logical operator</span></span> | <span data-ttu-id="c4261-114">Popis</span><span class="sxs-lookup"><span data-stu-id="c4261-114">Description</span></span> | <span data-ttu-id="c4261-115">Příklad řetězec filtru</span><span class="sxs-lookup"><span data-stu-id="c4261-115">Example filter string</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c4261-116">EQ</span><span class="sxs-lookup"><span data-stu-id="c4261-116">eq</span></span> |<span data-ttu-id="c4261-117">Rovná</span><span class="sxs-lookup"><span data-stu-id="c4261-117">Equal</span></span> |<span data-ttu-id="c4261-118">Město eq 'Redmond.</span><span class="sxs-lookup"><span data-stu-id="c4261-118">City eq 'Redmond'</span></span> |
| <span data-ttu-id="c4261-119">gt</span><span class="sxs-lookup"><span data-stu-id="c4261-119">gt</span></span> |<span data-ttu-id="c4261-120">Více než</span><span class="sxs-lookup"><span data-stu-id="c4261-120">Greater than</span></span> |<span data-ttu-id="c4261-121">Cena gt 20</span><span class="sxs-lookup"><span data-stu-id="c4261-121">Price gt 20</span></span> |
| <span data-ttu-id="c4261-122">ge</span><span class="sxs-lookup"><span data-stu-id="c4261-122">ge</span></span> |<span data-ttu-id="c4261-123">Větší než nebo rovno</span><span class="sxs-lookup"><span data-stu-id="c4261-123">Greater than or equal to</span></span> |<span data-ttu-id="c4261-124">Cena ge 10</span><span class="sxs-lookup"><span data-stu-id="c4261-124">Price ge 10</span></span> |
| <span data-ttu-id="c4261-125">lt</span><span class="sxs-lookup"><span data-stu-id="c4261-125">lt</span></span> |<span data-ttu-id="c4261-126">Méně než</span><span class="sxs-lookup"><span data-stu-id="c4261-126">Less than</span></span> |<span data-ttu-id="c4261-127">Cena lt 20</span><span class="sxs-lookup"><span data-stu-id="c4261-127">Price lt 20</span></span> |
| <span data-ttu-id="c4261-128">Le</span><span class="sxs-lookup"><span data-stu-id="c4261-128">le</span></span> |<span data-ttu-id="c4261-129">Menší než nebo rovno</span><span class="sxs-lookup"><span data-stu-id="c4261-129">Less than or equal</span></span> |<span data-ttu-id="c4261-130">Cena le 100</span><span class="sxs-lookup"><span data-stu-id="c4261-130">Price le 100</span></span> |
| <span data-ttu-id="c4261-131">Ne</span><span class="sxs-lookup"><span data-stu-id="c4261-131">ne</span></span> |<span data-ttu-id="c4261-132">Není rovno</span><span class="sxs-lookup"><span data-stu-id="c4261-132">Not equal</span></span> |<span data-ttu-id="c4261-133">Ne města, Londýn,</span><span class="sxs-lookup"><span data-stu-id="c4261-133">City ne 'London'</span></span> |
| <span data-ttu-id="c4261-134">a</span><span class="sxs-lookup"><span data-stu-id="c4261-134">and</span></span> |<span data-ttu-id="c4261-135">A</span><span class="sxs-lookup"><span data-stu-id="c4261-135">And</span></span> |<span data-ttu-id="c4261-136">Cena le 200 a cena gt 3.5</span><span class="sxs-lookup"><span data-stu-id="c4261-136">Price le 200 and Price gt 3.5</span></span> |
| <span data-ttu-id="c4261-137">nebo</span><span class="sxs-lookup"><span data-stu-id="c4261-137">or</span></span> |<span data-ttu-id="c4261-138">Nebo</span><span class="sxs-lookup"><span data-stu-id="c4261-138">Or</span></span> |<span data-ttu-id="c4261-139">Cena le 3.5 nebo cena gt 200</span><span class="sxs-lookup"><span data-stu-id="c4261-139">Price le 3.5 or Price gt 200</span></span> |
| <span data-ttu-id="c4261-140">není</span><span class="sxs-lookup"><span data-stu-id="c4261-140">not</span></span> |<span data-ttu-id="c4261-141">není</span><span class="sxs-lookup"><span data-stu-id="c4261-141">Not</span></span> |<span data-ttu-id="c4261-142">není isAvailable</span><span class="sxs-lookup"><span data-stu-id="c4261-142">not isAvailable</span></span> |

<span data-ttu-id="c4261-143">Při vytváření řetězec filtru, jsou důležité následující pravidla:</span><span class="sxs-lookup"><span data-stu-id="c4261-143">When constructing a filter string, the following rules are important:</span></span>

* <span data-ttu-id="c4261-144">Logické operátory slouží k porovnání vlastnost na hodnotu.</span><span class="sxs-lookup"><span data-stu-id="c4261-144">Use the logical operators to compare a property to a value.</span></span> <span data-ttu-id="c4261-145">Všimněte si, že není možné k porovnání vlastnost na hodnotu dynamické; jedna strana výrazu musí být konstanta.</span><span class="sxs-lookup"><span data-stu-id="c4261-145">Note that it is not possible to compare a property to a dynamic value; one side of the expression must be a constant.</span></span>
* <span data-ttu-id="c4261-146">Všechny části řetězec filtru rozlišují velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="c4261-146">All parts of the filter string are case-sensitive.</span></span>
* <span data-ttu-id="c4261-147">Hodnota konstanty musí být stejného typu dat jako vlastnost v pořadí pro filtr vracet výsledky platný.</span><span class="sxs-lookup"><span data-stu-id="c4261-147">The constant value must be of the same data type as the property in order for the filter to return valid results.</span></span> <span data-ttu-id="c4261-148">Další informace o typech podporovaných vlastnost najdete v tématu [Principy datového modelu služby Table](http://go.microsoft.com/fwlink/p/?LinkId=400448).</span><span class="sxs-lookup"><span data-stu-id="c4261-148">For more information about supported property types, see [Understanding the Table Service Data Model](http://go.microsoft.com/fwlink/p/?LinkId=400448).</span></span>

## <a name="filtering-on-string-properties"></a><span data-ttu-id="c4261-149">Filtrování na vlastnosti řetězce.</span><span class="sxs-lookup"><span data-stu-id="c4261-149">Filtering on String Properties</span></span>
<span data-ttu-id="c4261-150">Při filtrování pro vlastnosti string, uzavřete řetězcová konstanta v jednoduchých uvozovkách.</span><span class="sxs-lookup"><span data-stu-id="c4261-150">When you filter on string properties, enclose the string constant in single quotation marks.</span></span>

<span data-ttu-id="c4261-151">Následující příklad filtry na **PartitionKey** a **RowKey** vlastnosti; neklíčové další vlastnosti nebylo možné přidat také do řetězec filtru:</span><span class="sxs-lookup"><span data-stu-id="c4261-151">The following example filters on the **PartitionKey** and **RowKey** properties; additional non-key properties could also be added to the filter string:</span></span>

    PartitionKey eq 'Partition1' and RowKey eq '00001'

<span data-ttu-id="c4261-152">I když není potřeba, můžete v závorkách, uzavřete každý výraz filtru:</span><span class="sxs-lookup"><span data-stu-id="c4261-152">You can enclose each filter expression in parentheses, although it is not required:</span></span>

    (PartitionKey eq 'Partition1') and (RowKey eq '00001')

<span data-ttu-id="c4261-153">Upozorňujeme, že služby Table nepodporuje zástupné dotazů a nejsou podporovány v Návrháři tabulky buď.</span><span class="sxs-lookup"><span data-stu-id="c4261-153">Note that the Table service does not support wildcard queries, and they are not supported in the Table Designer either.</span></span> <span data-ttu-id="c4261-154">Můžete však provést předponu odpovídající pomocí relačních operátorů na požadovanou předponu.</span><span class="sxs-lookup"><span data-stu-id="c4261-154">However, you can perform prefix matching by using comparison operators on the desired prefix.</span></span> <span data-ttu-id="c4261-155">Následující příklad vrací entity, LastName vlastnost, začíná písmenem "A":</span><span class="sxs-lookup"><span data-stu-id="c4261-155">The following example returns entities with a LastName property beginning with the letter 'A':</span></span>

    LastName ge 'A' and LastName lt 'B'

## <a name="filtering-on-numeric-properties"></a><span data-ttu-id="c4261-156">Filtrování na číselné vlastnosti</span><span class="sxs-lookup"><span data-stu-id="c4261-156">Filtering on Numeric Properties</span></span>
<span data-ttu-id="c4261-157">Chcete-li filtrovat na celé číslo nebo číslo s plovoucí desetinnou čárkou, zadejte počet bez uvozovek.</span><span class="sxs-lookup"><span data-stu-id="c4261-157">To filter on an integer or floating-point number, specify the number without quotation marks.</span></span>

<span data-ttu-id="c4261-158">Tento příklad vrací všechny entity s vlastností stáří jehož hodnota je větší než 30:</span><span class="sxs-lookup"><span data-stu-id="c4261-158">This example returns all entities with an Age property whose value is greater than 30:</span></span>

    Age gt 30

<span data-ttu-id="c4261-159">Tento příklad vrací všechny entity s vlastností AmountDue jehož hodnota je menší než nebo rovna 100.25:</span><span class="sxs-lookup"><span data-stu-id="c4261-159">This example returns all entities with an AmountDue property whose value is less than or equal to 100.25:</span></span>

    AmountDue le 100.25

## <a name="filtering-on-boolean-properties"></a><span data-ttu-id="c4261-160">Filtrování na logická hodnota vlastnosti</span><span class="sxs-lookup"><span data-stu-id="c4261-160">Filtering on Boolean Properties</span></span>
<span data-ttu-id="c4261-161">Chcete-li filtrovat na logickou hodnotu, zadejte **true** nebo **false** bez uvozovek.</span><span class="sxs-lookup"><span data-stu-id="c4261-161">To filter on a Boolean value, specify **true** or **false** without quotation marks.</span></span>

<span data-ttu-id="c4261-162">Následující příklad vrací všechny entity, kde je vlastnost IsActive nastavena na **true**:</span><span class="sxs-lookup"><span data-stu-id="c4261-162">The following example returns all entities where the IsActive property is set to **true**:</span></span>

    IsActive eq true

<span data-ttu-id="c4261-163">Je také možné zapsat tento výraz filtru bez logický operátor.</span><span class="sxs-lookup"><span data-stu-id="c4261-163">You can also write this filter expression without the logical operator.</span></span> <span data-ttu-id="c4261-164">V následujícím příkladu služby Table také vrátí všechny entity kde isActive – je **true**:</span><span class="sxs-lookup"><span data-stu-id="c4261-164">In the following example, the Table service will also return all entities where IsActive is **true**:</span></span>

    IsActive

<span data-ttu-id="c4261-165">Pokud chcete vrátit všechny entity, pokud má hodnotu false IsActive, můžete použít not operátor:</span><span class="sxs-lookup"><span data-stu-id="c4261-165">To return all entities where IsActive is false, you can use the not operator:</span></span>

    not IsActive

## <a name="filtering-on-datetime-properties"></a><span data-ttu-id="c4261-166">Filtrování na vlastnosti data a času</span><span class="sxs-lookup"><span data-stu-id="c4261-166">Filtering on DateTime Properties</span></span>
<span data-ttu-id="c4261-167">Chcete-li filtrovat na hodnotu data a času, zadejte **data a času** – klíčové slovo, za nímž následuje konstanta datum a čas v jednoduchých uvozovkách.</span><span class="sxs-lookup"><span data-stu-id="c4261-167">To filter on a DateTime value, specify the **datetime** keyword, followed by the date/time constant in single quotation marks.</span></span> <span data-ttu-id="c4261-168">Konstanta data a času musí být ve formátu UTC kombinované, jak je popsáno v [formátování data a času hodnoty vlastností](http://go.microsoft.com/fwlink/p/?LinkId=400449).</span><span class="sxs-lookup"><span data-stu-id="c4261-168">The date/time constant must be in combined UTC format, as described in [Formatting DateTime Property Values](http://go.microsoft.com/fwlink/p/?LinkId=400449).</span></span>

<span data-ttu-id="c4261-169">Následující příklad vrací entity, kde je vlastnost CustomerSince rovno 10. července 2008:</span><span class="sxs-lookup"><span data-stu-id="c4261-169">The following example returns entities where the CustomerSince property is equal to July 10, 2008:</span></span>

    CustomerSince eq datetime'2008-07-10T00:00:00Z'
