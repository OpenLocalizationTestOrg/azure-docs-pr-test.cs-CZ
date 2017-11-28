---
title: "řetězce aaaConstructing filtru pro návrháře tabulky hello | Microsoft Docs"
description: "Vytváření řetězců filtru pro návrháře tabulky hello"
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
ms.openlocfilehash: 48b38d27b97936064daa875e41881d51546bc11f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="constructing-filter-strings-for-hello-table-designer"></a><span data-ttu-id="35a1e-103">Vytváření řetězců filtru pro hello návrháře tabulky</span><span class="sxs-lookup"><span data-stu-id="35a1e-103">Constructing Filter Strings for hello Table Designer</span></span>
## <a name="overview"></a><span data-ttu-id="35a1e-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="35a1e-104">Overview</span></span>
<span data-ttu-id="35a1e-105">hello toofilter data v Azure tabulky, který se zobrazí v sadě Visual Studio **návrháře tabulky**, můžete vytvořit řetězec filtru a zadejte do pole filtru hello.</span><span class="sxs-lookup"><span data-stu-id="35a1e-105">toofilter data in an Azure table that is displayed in hello Visual Studio **Table Designer**, you construct a filter string and enter it into hello filter field.</span></span> <span data-ttu-id="35a1e-106">Hello se syntaxí filtru řetězce je definována hello služby WCF Data Services a je podobné tooa SQL klauzule WHERE, ale je odeslán toohello služby Table prostřednictvím požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="35a1e-106">hello filter string syntax is defined by hello WCF Data Services and is similar tooa SQL WHERE clause, but is sent toohello Table service via an HTTP request.</span></span> <span data-ttu-id="35a1e-107">Hello **návrháře tabulky** popisovače hello správné kódování pro vás, tak toofilter na hodnotu požadovanou vlastnost, jenom musíte zadat název vlastnosti hello, operátor porovnání hodnotu pro kritéria, a můžete také filtrovat logický operátor ve hello pole.</span><span class="sxs-lookup"><span data-stu-id="35a1e-107">hello **Table Designer** handles hello proper encoding for you, so toofilter on a desired property value, you need only enter hello property name, comparison operator, criteria value, and optionally, Boolean operator in hello filter field.</span></span> <span data-ttu-id="35a1e-108">Jako při byly sestavování tabulky hello tooquery adresu URL prostřednictvím hello nemusí být povolena možnost dotazu hello $filter tooinclude [úložiště referenční dokumentace rozhraní API REST služby](http://go.microsoft.com/fwlink/p/?LinkId=400447).</span><span class="sxs-lookup"><span data-stu-id="35a1e-108">You do not need tooinclude hello $filter query option as you would if you were constructing a URL tooquery hello table via hello [Storage Services REST API Reference](http://go.microsoft.com/fwlink/p/?LinkId=400447).</span></span>

<span data-ttu-id="35a1e-109">Hello součásti WCF Data Services jsou založené na hello [protokol](http://go.microsoft.com/fwlink/p/?LinkId=214805) (OData).</span><span class="sxs-lookup"><span data-stu-id="35a1e-109">hello WCF Data Services are based on hello [Open Data Protocol](http://go.microsoft.com/fwlink/p/?LinkId=214805) (OData).</span></span> <span data-ttu-id="35a1e-110">Podrobnosti o možností dotazu systému filtru hello (**$filter**), najdete v části hello [konvence prostředí OData pro identifikátor URI specifikace](http://go.microsoft.com/fwlink/p/?LinkId=214806).</span><span class="sxs-lookup"><span data-stu-id="35a1e-110">For details on hello filter system query option (**$filter**), see hello [OData URI Conventions specification](http://go.microsoft.com/fwlink/p/?LinkId=214806).</span></span>

## <a name="comparison-operators"></a><span data-ttu-id="35a1e-111">Operátory porovnání</span><span class="sxs-lookup"><span data-stu-id="35a1e-111">Comparison Operators</span></span>
<span data-ttu-id="35a1e-112">Hello následující logické operátory jsou podporovány pro všechny typy vlastností:</span><span class="sxs-lookup"><span data-stu-id="35a1e-112">hello following logical operators are supported for all property types:</span></span>

| <span data-ttu-id="35a1e-113">Logický operátor</span><span class="sxs-lookup"><span data-stu-id="35a1e-113">Logical operator</span></span> | <span data-ttu-id="35a1e-114">Popis</span><span class="sxs-lookup"><span data-stu-id="35a1e-114">Description</span></span> | <span data-ttu-id="35a1e-115">Příklad řetězec filtru</span><span class="sxs-lookup"><span data-stu-id="35a1e-115">Example filter string</span></span> |
| --- | --- | --- |
| <span data-ttu-id="35a1e-116">EQ</span><span class="sxs-lookup"><span data-stu-id="35a1e-116">eq</span></span> |<span data-ttu-id="35a1e-117">Rovná</span><span class="sxs-lookup"><span data-stu-id="35a1e-117">Equal</span></span> |<span data-ttu-id="35a1e-118">Město eq 'Redmond.</span><span class="sxs-lookup"><span data-stu-id="35a1e-118">City eq 'Redmond'</span></span> |
| <span data-ttu-id="35a1e-119">gt</span><span class="sxs-lookup"><span data-stu-id="35a1e-119">gt</span></span> |<span data-ttu-id="35a1e-120">Více než</span><span class="sxs-lookup"><span data-stu-id="35a1e-120">Greater than</span></span> |<span data-ttu-id="35a1e-121">Cena gt 20</span><span class="sxs-lookup"><span data-stu-id="35a1e-121">Price gt 20</span></span> |
| <span data-ttu-id="35a1e-122">ge</span><span class="sxs-lookup"><span data-stu-id="35a1e-122">ge</span></span> |<span data-ttu-id="35a1e-123">Větší než nebo rovna příliš</span><span class="sxs-lookup"><span data-stu-id="35a1e-123">Greater than or equal too</span></span>|<span data-ttu-id="35a1e-124">Cena ge 10</span><span class="sxs-lookup"><span data-stu-id="35a1e-124">Price ge 10</span></span> |
| <span data-ttu-id="35a1e-125">lt</span><span class="sxs-lookup"><span data-stu-id="35a1e-125">lt</span></span> |<span data-ttu-id="35a1e-126">Méně než</span><span class="sxs-lookup"><span data-stu-id="35a1e-126">Less than</span></span> |<span data-ttu-id="35a1e-127">Cena lt 20</span><span class="sxs-lookup"><span data-stu-id="35a1e-127">Price lt 20</span></span> |
| <span data-ttu-id="35a1e-128">Le</span><span class="sxs-lookup"><span data-stu-id="35a1e-128">le</span></span> |<span data-ttu-id="35a1e-129">Menší než nebo rovno</span><span class="sxs-lookup"><span data-stu-id="35a1e-129">Less than or equal</span></span> |<span data-ttu-id="35a1e-130">Cena le 100</span><span class="sxs-lookup"><span data-stu-id="35a1e-130">Price le 100</span></span> |
| <span data-ttu-id="35a1e-131">Ne</span><span class="sxs-lookup"><span data-stu-id="35a1e-131">ne</span></span> |<span data-ttu-id="35a1e-132">Není rovno</span><span class="sxs-lookup"><span data-stu-id="35a1e-132">Not equal</span></span> |<span data-ttu-id="35a1e-133">Ne města, Londýn,</span><span class="sxs-lookup"><span data-stu-id="35a1e-133">City ne 'London'</span></span> |
| <span data-ttu-id="35a1e-134">a</span><span class="sxs-lookup"><span data-stu-id="35a1e-134">and</span></span> |<span data-ttu-id="35a1e-135">A</span><span class="sxs-lookup"><span data-stu-id="35a1e-135">And</span></span> |<span data-ttu-id="35a1e-136">Cena le 200 a cena gt 3.5</span><span class="sxs-lookup"><span data-stu-id="35a1e-136">Price le 200 and Price gt 3.5</span></span> |
| <span data-ttu-id="35a1e-137">nebo</span><span class="sxs-lookup"><span data-stu-id="35a1e-137">or</span></span> |<span data-ttu-id="35a1e-138">Nebo</span><span class="sxs-lookup"><span data-stu-id="35a1e-138">Or</span></span> |<span data-ttu-id="35a1e-139">Cena le 3.5 nebo cena gt 200</span><span class="sxs-lookup"><span data-stu-id="35a1e-139">Price le 3.5 or Price gt 200</span></span> |
| <span data-ttu-id="35a1e-140">není</span><span class="sxs-lookup"><span data-stu-id="35a1e-140">not</span></span> |<span data-ttu-id="35a1e-141">není</span><span class="sxs-lookup"><span data-stu-id="35a1e-141">Not</span></span> |<span data-ttu-id="35a1e-142">není isAvailable</span><span class="sxs-lookup"><span data-stu-id="35a1e-142">not isAvailable</span></span> |

<span data-ttu-id="35a1e-143">Při vytváření řetězec filtru, hello následující pravidla jsou důležité:</span><span class="sxs-lookup"><span data-stu-id="35a1e-143">When constructing a filter string, hello following rules are important:</span></span>

* <span data-ttu-id="35a1e-144">Použijte hello logické operátory toocompare tooa hodnotu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="35a1e-144">Use hello logical operators toocompare a property tooa value.</span></span> <span data-ttu-id="35a1e-145">Všimněte si, že není možné toocompare tooa dynamické hodnotu vlastnosti; jedna strana hello výrazu musí být konstanta.</span><span class="sxs-lookup"><span data-stu-id="35a1e-145">Note that it is not possible toocompare a property tooa dynamic value; one side of hello expression must be a constant.</span></span>
* <span data-ttu-id="35a1e-146">Všechny části řetězec filtru hello rozlišují velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="35a1e-146">All parts of hello filter string are case-sensitive.</span></span>
* <span data-ttu-id="35a1e-147">Hello konstantní hodnota musí být hello stejný datový typ jako vlastnost hello v pořadí hello filtru tooreturn platný výsledků.</span><span class="sxs-lookup"><span data-stu-id="35a1e-147">hello constant value must be of hello same data type as hello property in order for hello filter tooreturn valid results.</span></span> <span data-ttu-id="35a1e-148">Další informace o typech podporovaných vlastnost najdete v tématu [hello Principy datového modelu služby Table](http://go.microsoft.com/fwlink/p/?LinkId=400448).</span><span class="sxs-lookup"><span data-stu-id="35a1e-148">For more information about supported property types, see [Understanding hello Table Service Data Model](http://go.microsoft.com/fwlink/p/?LinkId=400448).</span></span>

## <a name="filtering-on-string-properties"></a><span data-ttu-id="35a1e-149">Filtrování na vlastnosti řetězce.</span><span class="sxs-lookup"><span data-stu-id="35a1e-149">Filtering on String Properties</span></span>
<span data-ttu-id="35a1e-150">Při filtrování pro vlastnosti string, uzavřete hello řetězcová konstanta v jednoduchých uvozovkách.</span><span class="sxs-lookup"><span data-stu-id="35a1e-150">When you filter on string properties, enclose hello string constant in single quotation marks.</span></span>

<span data-ttu-id="35a1e-151">Následující příklad filtry na hello Hello **PartitionKey** a **RowKey** vlastnosti; neklíčové další vlastnosti může také přidat řetězec filtru toohello:</span><span class="sxs-lookup"><span data-stu-id="35a1e-151">hello following example filters on hello **PartitionKey** and **RowKey** properties; additional non-key properties could also be added toohello filter string:</span></span>

    PartitionKey eq 'Partition1' and RowKey eq '00001'

<span data-ttu-id="35a1e-152">I když není potřeba, můžete v závorkách, uzavřete každý výraz filtru:</span><span class="sxs-lookup"><span data-stu-id="35a1e-152">You can enclose each filter expression in parentheses, although it is not required:</span></span>

    (PartitionKey eq 'Partition1') and (RowKey eq '00001')

<span data-ttu-id="35a1e-153">Upozorňujeme, že hello služby Table nepodporuje zástupné dotazů a nejsou podporovány v hello návrháře tabulky buď.</span><span class="sxs-lookup"><span data-stu-id="35a1e-153">Note that hello Table service does not support wildcard queries, and they are not supported in hello Table Designer either.</span></span> <span data-ttu-id="35a1e-154">Můžete však provést předponu odpovídající pomocí operátory porovnání na požadovanou předponu hello.</span><span class="sxs-lookup"><span data-stu-id="35a1e-154">However, you can perform prefix matching by using comparison operators on hello desired prefix.</span></span> <span data-ttu-id="35a1e-155">Hello následující příklad vrací entity, LastName vlastnost, začíná písmenem hello "A":</span><span class="sxs-lookup"><span data-stu-id="35a1e-155">hello following example returns entities with a LastName property beginning with hello letter 'A':</span></span>

    LastName ge 'A' and LastName lt 'B'

## <a name="filtering-on-numeric-properties"></a><span data-ttu-id="35a1e-156">Filtrování na číselné vlastnosti</span><span class="sxs-lookup"><span data-stu-id="35a1e-156">Filtering on Numeric Properties</span></span>
<span data-ttu-id="35a1e-157">toofilter na celé číslo nebo číslo s plovoucí desetinnou čárkou, zadejte číslo hello bez uvozovek.</span><span class="sxs-lookup"><span data-stu-id="35a1e-157">toofilter on an integer or floating-point number, specify hello number without quotation marks.</span></span>

<span data-ttu-id="35a1e-158">Tento příklad vrací všechny entity s vlastností stáří jehož hodnota je větší než 30:</span><span class="sxs-lookup"><span data-stu-id="35a1e-158">This example returns all entities with an Age property whose value is greater than 30:</span></span>

    Age gt 30

<span data-ttu-id="35a1e-159">Tento příklad vrací všechny entity s vlastností AmountDue jehož hodnota je menší než nebo roven hodnotě too100.25:</span><span class="sxs-lookup"><span data-stu-id="35a1e-159">This example returns all entities with an AmountDue property whose value is less than or equal too100.25:</span></span>

    AmountDue le 100.25

## <a name="filtering-on-boolean-properties"></a><span data-ttu-id="35a1e-160">Filtrování na logická hodnota vlastnosti</span><span class="sxs-lookup"><span data-stu-id="35a1e-160">Filtering on Boolean Properties</span></span>
<span data-ttu-id="35a1e-161">Zadejte toofilter na logickou hodnotu, **true** nebo **false** bez uvozovek.</span><span class="sxs-lookup"><span data-stu-id="35a1e-161">toofilter on a Boolean value, specify **true** or **false** without quotation marks.</span></span>

<span data-ttu-id="35a1e-162">Hello následující příklad vrací všechny entity kde hello isActive – vlastnost je nastaven příliš**true**:</span><span class="sxs-lookup"><span data-stu-id="35a1e-162">hello following example returns all entities where hello IsActive property is set too**true**:</span></span>

    IsActive eq true

<span data-ttu-id="35a1e-163">Je také možné zapsat tento výraz filtru bez hello logický operátor.</span><span class="sxs-lookup"><span data-stu-id="35a1e-163">You can also write this filter expression without hello logical operator.</span></span> <span data-ttu-id="35a1e-164">V následujícím příkladu hello, hello služby Table také vrátí všechny entity kde isActive – je **true**:</span><span class="sxs-lookup"><span data-stu-id="35a1e-164">In hello following example, hello Table service will also return all entities where IsActive is **true**:</span></span>

    IsActive

<span data-ttu-id="35a1e-165">tooreturn všechny entity, pokud má hodnotu false, můžete použít IsActive hello není operátor:</span><span class="sxs-lookup"><span data-stu-id="35a1e-165">tooreturn all entities where IsActive is false, you can use hello not operator:</span></span>

    not IsActive

## <a name="filtering-on-datetime-properties"></a><span data-ttu-id="35a1e-166">Filtrování na vlastnosti data a času</span><span class="sxs-lookup"><span data-stu-id="35a1e-166">Filtering on DateTime Properties</span></span>
<span data-ttu-id="35a1e-167">toofilter na hodnotu data a času, zadejte hello **data a času** – klíčové slovo, za nímž následuje hello konstanta datum a čas v jednoduchých uvozovkách.</span><span class="sxs-lookup"><span data-stu-id="35a1e-167">toofilter on a DateTime value, specify hello **datetime** keyword, followed by hello date/time constant in single quotation marks.</span></span> <span data-ttu-id="35a1e-168">Konstanta Hello data a času musí být ve formátu UTC kombinované, jak je popsáno v [formátování data a času hodnoty vlastností](http://go.microsoft.com/fwlink/p/?LinkId=400449).</span><span class="sxs-lookup"><span data-stu-id="35a1e-168">hello date/time constant must be in combined UTC format, as described in [Formatting DateTime Property Values](http://go.microsoft.com/fwlink/p/?LinkId=400449).</span></span>

<span data-ttu-id="35a1e-169">Hello následující příklad vrací entity, kde vlastnost CustomerSince hello je rovna tooJuly 10, 2008:</span><span class="sxs-lookup"><span data-stu-id="35a1e-169">hello following example returns entities where hello CustomerSince property is equal tooJuly 10, 2008:</span></span>

    CustomerSince eq datetime'2008-07-10T00:00:00Z'
