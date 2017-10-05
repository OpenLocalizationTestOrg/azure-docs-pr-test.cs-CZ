---
title: "Průvodce návrhem úložiště Azure Table | Microsoft Docs"
description: "Návrh škálovatelné a původce tabulky ve službě Azure Table Storage"
services: cosmos-db
documentationcenter: na
author: mimig1
manager: tadb
editor: tysonn
ms.assetid: 8e228b0c-2998-4462-8101-9f16517393ca
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 02/28/2017
ms.author: mimig
ms.openlocfilehash: fd34fb135c76eed4041c29e00e98dde330dfe3f3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-storage-table-design-guide-designing-scalable-and-performant-tables"></a><span data-ttu-id="4e549-103">Průvodce návrhem tabulky úložiště Azure: Návrh škálovatelné a původce tabulky</span><span class="sxs-lookup"><span data-stu-id="4e549-103">Azure Storage Table Design Guide: Designing Scalable and Performant Tables</span></span>
[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

<span data-ttu-id="4e549-104">Návrh škálovatelné a původce tabulky je nutné zvážit několik faktorů, třeba výkon, škálovatelnost a náklady.</span><span class="sxs-lookup"><span data-stu-id="4e549-104">To design scalable and performant tables you must consider a number of factors such as performance, scalability, and cost.</span></span> <span data-ttu-id="4e549-105">Pokud dříve navrženy schémata pro relační databáze, bude snadno dokážete těchto aspektů, ale existuje některé podobnosti mezi modelu úložiště služby Azure Table a relační modely, existují také mnoho důležitých rozdílů.</span><span class="sxs-lookup"><span data-stu-id="4e549-105">If you have previously designed schemas for relational databases, these considerations will be familiar to you, but while there are some similarities between the Azure Table service storage model and relational models, there are also many important differences.</span></span> <span data-ttu-id="4e549-106">Tyto rozdíly jsou obvykle vést k velmi různých návrzích, který může vypadat counter-intuitive nebo nesprávný někomu obeznámeni s relačních databází, ale které Nedělejte představu, pokud navrhujete pro úložiště dvojic klíč/hodnota NoSQL například službu Azure Table.</span><span class="sxs-lookup"><span data-stu-id="4e549-106">These differences typically lead to very different designs that may look counter-intuitive or wrong to someone familiar with relational databases, but which do make good sense if you are designing for a NoSQL key/value store such as the Azure Table service.</span></span> <span data-ttu-id="4e549-107">Řadu váš návrh rozdíly bude odrážet fakt, že služby Table je navržen pro podporu škálování cloudové aplikace, které může obsahovat až miliardy entit (řádky v terminologii relační databáze), data nebo datové sady, které musí podporovat transakce velké svazky: tedy potřebujete jinak myslíte o tom, jak ukládat data a pochopit, jak funguje služba Table.</span><span class="sxs-lookup"><span data-stu-id="4e549-107">Many of your design differences will reflect the fact that the Table service is designed to support cloud-scale applications that can contain billions of entities (rows in relational database terminology) of data or for datasets that must support very high transaction volumes: therefore, you need to think differently about how you store your data and understand how the Table service works.</span></span> <span data-ttu-id="4e549-108">Dobře navrženým úložiště dat typu NoSQL můžete povolit řešení škálování mnohem další (a se to při nižších nákladech) než řešení, které používá relační databáze.</span><span class="sxs-lookup"><span data-stu-id="4e549-108">A well designed NoSQL data store can enable your solution to scale much further (and at a lower cost) than a solution that uses a relational database.</span></span> <span data-ttu-id="4e549-109">Tento průvodce vám pomůže s Tato témata.</span><span class="sxs-lookup"><span data-stu-id="4e549-109">This guide helps you with these topics.</span></span>  

## <a name="about-the-azure-table-service"></a><span data-ttu-id="4e549-110">O službě Azure Table</span><span class="sxs-lookup"><span data-stu-id="4e549-110">About the Azure Table service</span></span>
<span data-ttu-id="4e549-111">V této části jsou zdůrazněné některé klíčové funkce služby Table, které jsou obzvláště důležité pro návrh pro výkon a škálovatelnost.</span><span class="sxs-lookup"><span data-stu-id="4e549-111">This section highlights some of the key features of the Table service that are especially relevant to designing for performance and scalability.</span></span> <span data-ttu-id="4e549-112">Pokud jste pro Azure Storage a služby Table nové, nejdřív přečíst [Úvod do Microsoft Azure Storage](../storage/common/storage-introduction.md) a [Začínáme s Azure Table Storage pomocí rozhraní .NET](table-storage-how-to-use-dotnet.md) než si přečtete zbývající část tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="4e549-112">If you are new to Azure Storage and the Table service, first read [Introduction to Microsoft Azure Storage](../storage/common/storage-introduction.md) and [Get started with Azure Table Storage using .NET](table-storage-how-to-use-dotnet.md) before reading the remainder of this article.</span></span> <span data-ttu-id="4e549-113">I když je aktivní tohoto průvodce služby Table, bude obsahovat některé diskuzi o služby Azure Queue a objektů Blob a jak je možné použít společně s služby Table v řešení.</span><span class="sxs-lookup"><span data-stu-id="4e549-113">Although the focus of this guide is on the Table service, it will include some discussion of the Azure Queue and Blob services, and how you might use them along with the Table service in a solution.</span></span>  

<span data-ttu-id="4e549-114">Co je služba Table?</span><span class="sxs-lookup"><span data-stu-id="4e549-114">What is the Table service?</span></span> <span data-ttu-id="4e549-115">Jak byste očekávali od názvu služby Table k ukládání dat používá tabulkovém formátu.</span><span class="sxs-lookup"><span data-stu-id="4e549-115">As you might expect from the name, the Table service uses a tabular format to store data.</span></span> <span data-ttu-id="4e549-116">V standardní terminologie každý řádek v tabulce představuje entitu a sloupce ukládat různé vlastnosti dané entity.</span><span class="sxs-lookup"><span data-stu-id="4e549-116">In the standard terminology, each row of the table represents an entity, and the columns store the various properties of that entity.</span></span> <span data-ttu-id="4e549-117">Každé entity má pár klíčů k jeho jednoznačné identifikaci, a aktualizovat sloupec časového razítka, který používá služby Table můžete sledovat, kdy byl naposledy entity (k tomu dojde automaticky a nelze ručně přepsat časové razítko s libovolnou hodnotou).</span><span class="sxs-lookup"><span data-stu-id="4e549-117">Every entity has a pair of keys to uniquely identify it, and a timestamp column that the Table service uses to track when the entity was last updated (this happens automatically and you cannot manually overwrite the timestamp with an arbitrary value).</span></span> <span data-ttu-id="4e549-118">Služba Table používá ke správě optimistickou metodu souběžného tento poslední úpravy časové razítko (LMT).</span><span class="sxs-lookup"><span data-stu-id="4e549-118">The Table service uses this last-modified timestamp (LMT) to manage optimistic concurrency.</span></span>  

> [!NOTE]
> <span data-ttu-id="4e549-119">Operace REST API služby Table se taky vrátit **značka ETag** hodnotu, která je odvozena z časové razítko poslední úpravy (LMT).</span><span class="sxs-lookup"><span data-stu-id="4e549-119">The Table service REST API operations also return an **ETag** value that it derives from the last-modified timestamp (LMT).</span></span> <span data-ttu-id="4e549-120">V tomto dokumentu budeme používat podmínky ETag a LMT zcela zaměnitelným významem protože odkazují na stejnou základní data.</span><span class="sxs-lookup"><span data-stu-id="4e549-120">In this document we will use the terms ETag and LMT interchangeably because they refer to the same underlying data.</span></span>  
> 
> 

<span data-ttu-id="4e549-121">Následující příklad ukazuje návrh jednoduché tabulky k ukládání entit zaměstnanců a oddělení.</span><span class="sxs-lookup"><span data-stu-id="4e549-121">The following example shows a simple table design to store employee and department entities.</span></span> <span data-ttu-id="4e549-122">Řadu příklady uvedené v této příručce jsou založené na tento návrh jednoduché.</span><span class="sxs-lookup"><span data-stu-id="4e549-122">Many of the examples shown later in this guide are based on this simple design.</span></span>  

<table>
<tr>
<th><span data-ttu-id="4e549-123">Klíč oddílu</span><span class="sxs-lookup"><span data-stu-id="4e549-123">PartitionKey</span></span></th>
<th><span data-ttu-id="4e549-124">RowKey</span><span class="sxs-lookup"><span data-stu-id="4e549-124">RowKey</span></span></th>
<th><span data-ttu-id="4e549-125">časové razítko</span><span class="sxs-lookup"><span data-stu-id="4e549-125">Timestamp</span></span></th>
<th></th>
</tr>
<tr>
<td><span data-ttu-id="4e549-126">Marketing</span><span class="sxs-lookup"><span data-stu-id="4e549-126">Marketing</span></span></td>
<td><span data-ttu-id="4e549-127">00001</span><span class="sxs-lookup"><span data-stu-id="4e549-127">00001</span></span></td>
<td><span data-ttu-id="4e549-128">2014-08-22T00:50:32Z</span><span class="sxs-lookup"><span data-stu-id="4e549-128">2014-08-22T00:50:32Z</span></span></td>
<td>
<table>
<tr>
<th><span data-ttu-id="4e549-129">FirstName</span><span class="sxs-lookup"><span data-stu-id="4e549-129">FirstName</span></span></th>
<th><span data-ttu-id="4e549-130">Příjmení</span><span class="sxs-lookup"><span data-stu-id="4e549-130">LastName</span></span></th>
<th><span data-ttu-id="4e549-131">Věk</span><span class="sxs-lookup"><span data-stu-id="4e549-131">Age</span></span></th>
<th><span data-ttu-id="4e549-132">E-mail</span><span class="sxs-lookup"><span data-stu-id="4e549-132">Email</span></span></th>
</tr>
<tr>
<td><span data-ttu-id="4e549-133">Na ochranu</span><span class="sxs-lookup"><span data-stu-id="4e549-133">Don</span></span></td>
<td><span data-ttu-id="4e549-134">Hall</span><span class="sxs-lookup"><span data-stu-id="4e549-134">Hall</span></span></td>
<td><span data-ttu-id="4e549-135">34</span><span class="sxs-lookup"><span data-stu-id="4e549-135">34</span></span></td>
<td>donh@contoso.com</td>
</tr>
</table>
</tr>
<tr>
<td><span data-ttu-id="4e549-136">Marketing</span><span class="sxs-lookup"><span data-stu-id="4e549-136">Marketing</span></span></td>
<td><span data-ttu-id="4e549-137">00002</span><span class="sxs-lookup"><span data-stu-id="4e549-137">00002</span></span></td>
<td><span data-ttu-id="4e549-138">2014-08-22T00:50:34Z</span><span class="sxs-lookup"><span data-stu-id="4e549-138">2014-08-22T00:50:34Z</span></span></td>
<td>
<table>
<tr>
<th><span data-ttu-id="4e549-139">FirstName</span><span class="sxs-lookup"><span data-stu-id="4e549-139">FirstName</span></span></th>
<th><span data-ttu-id="4e549-140">Příjmení</span><span class="sxs-lookup"><span data-stu-id="4e549-140">LastName</span></span></th>
<th><span data-ttu-id="4e549-141">Věk</span><span class="sxs-lookup"><span data-stu-id="4e549-141">Age</span></span></th>
<th><span data-ttu-id="4e549-142">E-mail</span><span class="sxs-lookup"><span data-stu-id="4e549-142">Email</span></span></th>
</tr>
<tr>
<td><span data-ttu-id="4e549-143">Června</span><span class="sxs-lookup"><span data-stu-id="4e549-143">Jun</span></span></td>
<td><span data-ttu-id="4e549-144">CaO</span><span class="sxs-lookup"><span data-stu-id="4e549-144">Cao</span></span></td>
<td><span data-ttu-id="4e549-145">47</span><span class="sxs-lookup"><span data-stu-id="4e549-145">47</span></span></td>
<td>junc@contoso.com</td>
</tr>
</table>
</tr>
<tr>
<td><span data-ttu-id="4e549-146">Marketing</span><span class="sxs-lookup"><span data-stu-id="4e549-146">Marketing</span></span></td>
<td><span data-ttu-id="4e549-147">Oddělení</span><span class="sxs-lookup"><span data-stu-id="4e549-147">Department</span></span></td>
<td><span data-ttu-id="4e549-148">2014-08-22T00:50:30Z</span><span class="sxs-lookup"><span data-stu-id="4e549-148">2014-08-22T00:50:30Z</span></span></td>
<td>
<table>
<tr>
<th><span data-ttu-id="4e549-149">DepartmentName</span><span class="sxs-lookup"><span data-stu-id="4e549-149">DepartmentName</span></span></th>
<th><span data-ttu-id="4e549-150">EmployeeCount</span><span class="sxs-lookup"><span data-stu-id="4e549-150">EmployeeCount</span></span></th>
</tr>
<tr>
<td><span data-ttu-id="4e549-151">Marketing</span><span class="sxs-lookup"><span data-stu-id="4e549-151">Marketing</span></span></td>
<td><span data-ttu-id="4e549-152">153</span><span class="sxs-lookup"><span data-stu-id="4e549-152">153</span></span></td>
</tr>
</table>
</td>
</tr>
<tr>
<td><span data-ttu-id="4e549-153">Prodej</span><span class="sxs-lookup"><span data-stu-id="4e549-153">Sales</span></span></td>
<td><span data-ttu-id="4e549-154">00010</span><span class="sxs-lookup"><span data-stu-id="4e549-154">00010</span></span></td>
<td><span data-ttu-id="4e549-155">2014-08-22T00:50:44Z</span><span class="sxs-lookup"><span data-stu-id="4e549-155">2014-08-22T00:50:44Z</span></span></td>
<td>
<table>
<tr>
<th><span data-ttu-id="4e549-156">FirstName</span><span class="sxs-lookup"><span data-stu-id="4e549-156">FirstName</span></span></th>
<th><span data-ttu-id="4e549-157">Příjmení</span><span class="sxs-lookup"><span data-stu-id="4e549-157">LastName</span></span></th>
<th><span data-ttu-id="4e549-158">Věk</span><span class="sxs-lookup"><span data-stu-id="4e549-158">Age</span></span></th>
<th><span data-ttu-id="4e549-159">E-mail</span><span class="sxs-lookup"><span data-stu-id="4e549-159">Email</span></span></th>
</tr>
<tr>
<td><span data-ttu-id="4e549-160">Ken</span><span class="sxs-lookup"><span data-stu-id="4e549-160">Ken</span></span></td>
<td><span data-ttu-id="4e549-161">Kwok</span><span class="sxs-lookup"><span data-stu-id="4e549-161">Kwok</span></span></td>
<td><span data-ttu-id="4e549-162">23</span><span class="sxs-lookup"><span data-stu-id="4e549-162">23</span></span></td>
<td>kenk@contoso.com</td>
</tr>
</table>
</td>
</tr>
</table>


<span data-ttu-id="4e549-163">Pokud to je velmi podobná do tabulky v relační databázi s hlavní rozdíly jsou povinné sloupců a možnost ukládat více typů entit ve stejné tabulce.</span><span class="sxs-lookup"><span data-stu-id="4e549-163">So far, this looks very similar to a table in a relational database with the key differences being the mandatory columns, and the ability to store multiple entity types in the same table.</span></span> <span data-ttu-id="4e549-164">Kromě toho, každý z uživatelem definované vlastnosti, jako **FirstName** nebo **stáří** má datový typ jako celé číslo nebo řetězec, jenom jako sloupec v relační databázi.</span><span class="sxs-lookup"><span data-stu-id="4e549-164">In addition, each of the user-defined properties such as **FirstName** or **Age** has a data type, such as integer or string, just like a column in a relational database.</span></span> <span data-ttu-id="4e549-165">I když na rozdíl od v relační databázi, bez schématu povaha služby Table znamená, že vlastnost nemusí mít stejný datový typ. u každé entity.</span><span class="sxs-lookup"><span data-stu-id="4e549-165">Although unlike in a relational database, the schema-less nature of the Table service means that a property need not have the same data type on each entity.</span></span> <span data-ttu-id="4e549-166">Pokud chcete komplexními datovými typy do vlastnosti jediné, musíte použít serializovaných formátu například XML nebo JSON.</span><span class="sxs-lookup"><span data-stu-id="4e549-166">To store complex data types in a single property, you must use a serialized format such as JSON or XML.</span></span> <span data-ttu-id="4e549-167">Další informace o tabulce služby, například podporované datové typy, podporované rozsahů, pravidla pojmenování a omezení velikosti najdete v tématu [Principy datového modelu služby Table](http://msdn.microsoft.com/library/azure/dd179338.aspx).</span><span class="sxs-lookup"><span data-stu-id="4e549-167">For more information about the table service such as supported data types, supported date ranges, naming rules, and size constraints, see [Understanding the Table Service Data Model](http://msdn.microsoft.com/library/azure/dd179338.aspx).</span></span>

<span data-ttu-id="4e549-168">Jak uvidíte, vaši volbu **PartitionKey** a **RowKey** je nezbytné, aby návrh dobrý tabulky.</span><span class="sxs-lookup"><span data-stu-id="4e549-168">As you will see, your choice of **PartitionKey** and **RowKey** is fundamental to good table design.</span></span> <span data-ttu-id="4e549-169">Každou entitu uložené v tabulce musí mít jedinečnou kombinaci **PartitionKey** a **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="4e549-169">Every entity stored in a table must have a unique combination of **PartitionKey** and **RowKey**.</span></span> <span data-ttu-id="4e549-170">Stejně jako u klíče v tabulce relační databáze, **PartitionKey** a **RowKey** hodnoty jsou indexované vytvořit clusterovaný index, který umožňuje rychlé look-ups; však služby Table nevytvoří žádné sekundární indexy, takže toto jsou pouze dva indexované vlastnosti (některé vzory popsané dál zobrazit jak můžete obejít toto zřejmá omezení).</span><span class="sxs-lookup"><span data-stu-id="4e549-170">As with keys in a relational database table, the **PartitionKey** and **RowKey** values are indexed to create a clustered index that enables fast look-ups; however, the Table service does not create any secondary indexes so these are the only two indexed properties (some of the patterns described later show how you can work around this apparent limitation).</span></span>  

<span data-ttu-id="4e549-171">Tabulka se skládá z jednoho nebo více oddílů, a jak uvidíte, řadu rozhodnutí o návrhu provedete budou kolem výběr vhodný **PartitionKey** a **RowKey** k optimalizaci vašeho řešení.</span><span class="sxs-lookup"><span data-stu-id="4e549-171">A table is made up of one or more partitions, and as you will see, many of the design decisions you make will be around choosing a suitable **PartitionKey** and **RowKey** to optimize your solution.</span></span> <span data-ttu-id="4e549-172">Řešení může obsahovat pouze jednu tabulku, která obsahuje všechny vaše entity, které jsou uspořádány do oddílů, ale obvykle řešení bude mít více tabulek.</span><span class="sxs-lookup"><span data-stu-id="4e549-172">A solution could consist of just a single table that contains all your entities organized into partitions, but typically a solution will have multiple tables.</span></span> <span data-ttu-id="4e549-173">Tabulky umožňují logicky uspořádat vaší entity, které vám pomůžou spravovat přístup k datům pomocí seznamů řízení přístupu a mohou vyřadit celé tabulky pomocí operace jedno úložiště.</span><span class="sxs-lookup"><span data-stu-id="4e549-173">Tables help you to logically organize your entities, help you manage access to the data using access control lists, and you can drop an entire table using a single storage operation.</span></span>  

### <a name="table-partitions"></a><span data-ttu-id="4e549-174">Oddíly tabulky</span><span class="sxs-lookup"><span data-stu-id="4e549-174">Table partitions</span></span>
<span data-ttu-id="4e549-175">Název účtu, název tabulky a **PartitionKey** společně identifikovat oddíl v rámci služby úložiště, kde služby table ukládá entity.</span><span class="sxs-lookup"><span data-stu-id="4e549-175">The account name, table name and **PartitionKey** together identify the partition within the storage service where the table service stores the entity.</span></span> <span data-ttu-id="4e549-176">A také se součástí schéma adresování pro entity, oddíly definování oboru pro transakce (najdete v části [Entity skupiny transakce](#entity-group-transactions) níže), a vytvoří základ pro jak škáluje služby table.</span><span class="sxs-lookup"><span data-stu-id="4e549-176">As well as being part of the addressing scheme for entities, partitions define a scope for transactions (see [Entity Group Transactions](#entity-group-transactions) below), and form the basis of how the table service scales.</span></span> <span data-ttu-id="4e549-177">Další informace o oddílech najdete v části [a cíle výkonnosti služby Azure Storage Scalability](../storage/common/storage-scalability-targets.md).</span><span class="sxs-lookup"><span data-stu-id="4e549-177">For more information on partitions see [Azure Storage Scalability and Performance Targets](../storage/common/storage-scalability-targets.md).</span></span>  

<span data-ttu-id="4e549-178">Ve službě Table jednotlivých uzel služeb jeden nebo více dokončete oddílů a škálují služby tak, že dynamicky Vyrovnávání zatížení oddíly mezi uzly.</span><span class="sxs-lookup"><span data-stu-id="4e549-178">In the Table service, an individual node services one or more complete partitions and the service scales by dynamically load-balancing partitions across nodes.</span></span> <span data-ttu-id="4e549-179">Pokud uzel je zatížení, můžete služby table *rozdělení* rozsahu oddílů obsluhovány pomocí daného uzlu do jiných uzlů; při provozu subvence, můžete službu *sloučení* back rozsahy oddílu z quiet uzlů do jednoho uzlu.</span><span class="sxs-lookup"><span data-stu-id="4e549-179">If a node is under load, the table service can *split* the range of partitions serviced by that node onto different nodes; when traffic subsides, the service can *merge* the partition ranges from quiet nodes back onto a single node.</span></span>  

<span data-ttu-id="4e549-180">Další informace o interní podrobnosti služby Table, zejména jak službu spravuje oddíly, najdete v dokumentu paper [Microsoft Azure Storage: A vysoce dostupné cloudového úložiště se silnou konzistencí](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).</span><span class="sxs-lookup"><span data-stu-id="4e549-180">For more information about the internal details of the Table service, and in particular how the service manages partitions, see the paper [Microsoft Azure Storage: A Highly Available Cloud Storage Service with Strong Consistency](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).</span></span>  

### <a name="entity-group-transactions"></a><span data-ttu-id="4e549-181">Transakce skupiny entity</span><span class="sxs-lookup"><span data-stu-id="4e549-181">Entity Group Transactions</span></span>
<span data-ttu-id="4e549-182">Ve službě Table Entity skupiny transakce (EGTs) jsou pouze integrované mechanismus pro provádění atomic aktualizace napříč více entit.</span><span class="sxs-lookup"><span data-stu-id="4e549-182">In the Table service, Entity Group Transactions (EGTs) are the only built-in mechanism for performing atomic updates across multiple entities.</span></span> <span data-ttu-id="4e549-183">EGTs jsou také označovány jako *dávky transakce* v některé dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="4e549-183">EGTs are also referred to as *batch transactions* in some documentation.</span></span> <span data-ttu-id="4e549-184">EGTs mohou pracovat pouze u entit, které jsou uložené ve stejném oddílu (sdílet stejný klíč oddílu v dané tabulce), tak můžete kdykoli potřebujete atomic transakční chování napříč více entit je potřeba zajistit, že tyto entity jsou ve stejném oddílu.</span><span class="sxs-lookup"><span data-stu-id="4e549-184">EGTs can only operate on entities stored in the same partition (share the same partition key in a given table), so anytime you need atomic transactional behavior across multiple entities you need to ensure that those entities are in the same partition.</span></span> <span data-ttu-id="4e549-185">Toto je často důvod pro zachování více typů entit ve stejné tabulce (a oddíl) a nepoužíváte více tabulek pro typy různých entit.</span><span class="sxs-lookup"><span data-stu-id="4e549-185">This is often a reason for keeping multiple entity types in the same table (and partition) and not using multiple tables for different entity types.</span></span> <span data-ttu-id="4e549-186">Jeden EGT mohou pracovat na maximálně 100 entit.</span><span class="sxs-lookup"><span data-stu-id="4e549-186">A single EGT can operate on at most 100 entities.</span></span>  <span data-ttu-id="4e549-187">Pokud odešlete více souběžných EGTs pro zpracování je důležité zajistit, že tyto EGTs se nevztahují na entity, které jsou společné napříč EGTs při zpracování v opačném případě může být zpoždění.</span><span class="sxs-lookup"><span data-stu-id="4e549-187">If you submit multiple concurrent EGTs for processing it is important to ensure  those EGTs do not operate on entities that are common across EGTs as otherwise processing can be delayed.</span></span>

<span data-ttu-id="4e549-188">EGTs také zavádět potenciální kompromis pro vyhodnocení při návrhu: použití více oddíly se zvýší škálovatelnost aplikace protože Azure má většího počtu možností pro vyrovnávání zatížení požadavky napříč uzly, ale to může omezit schopnost aplikace provést jednotlivé transakce a udržovat silnou konzistenci pro vaše data.</span><span class="sxs-lookup"><span data-stu-id="4e549-188">EGTs also introduce a potential trade-off for you to evaluate in your design: using more partitions will increase the scalability of your application because Azure has more opportunities for load balancing requests across nodes, but this might limit the ability of your application to perform atomic transactions and maintain strong consistency for your data.</span></span> <span data-ttu-id="4e549-189">Kromě toho existují určité škálovatelnost cíle na úrovni oddílu, který může omezit propustnost transakce můžete očekávat, že pro jeden uzel: Další informace o cíle škálovatelnosti pro účty úložiště Azure a služby table najdete v tématu [a cíle výkonnosti služby Azure Storage Scalability](../storage/common/storage-scalability-targets.md).</span><span class="sxs-lookup"><span data-stu-id="4e549-189">Furthermore, there are specific scalability targets at the level of a partition that might limit the throughput of transactions you can expect for a single node: for more information about the scalability targets for Azure storage accounts and the table service, see [Azure Storage Scalability and Performance Targets](../storage/common/storage-scalability-targets.md).</span></span> <span data-ttu-id="4e549-190">Novější části této příručky popisují různé návrhu strategií, které vám pomohou spravovat kompromis jako je tato a popisují, jak nejlépe zvolit klíč oddílu podle specifických požadavků klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="4e549-190">Later sections of this guide discuss various design strategies that help you manage trade-offs such as this one, and discuss how best to choose your partition key based on the specific requirements of your client application.</span></span>  

### <a name="capacity-considerations"></a><span data-ttu-id="4e549-191">Aspekty kapacity</span><span class="sxs-lookup"><span data-stu-id="4e549-191">Capacity considerations</span></span>
<span data-ttu-id="4e549-192">Následující tabulka uvádí některé klíčové hodnoty znát při navrhování řešení pro službu tabulky:</span><span class="sxs-lookup"><span data-stu-id="4e549-192">The following table includes some of the key values to be aware of when you are designing a Table service solution:</span></span>  

| <span data-ttu-id="4e549-193">Celkové kapacity účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="4e549-193">Total capacity of an Azure storage account</span></span> | <span data-ttu-id="4e549-194">500 TB</span><span class="sxs-lookup"><span data-stu-id="4e549-194">500 TB</span></span> |
| --- | --- |
| <span data-ttu-id="4e549-195">Počet tabulek v účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="4e549-195">Number of tables in an Azure storage account</span></span> |<span data-ttu-id="4e549-196">Omezeno pouze kapacity účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="4e549-196">Limited only by the capacity of the storage account</span></span> |
| <span data-ttu-id="4e549-197">Počet oddílů v tabulce</span><span class="sxs-lookup"><span data-stu-id="4e549-197">Number of partitions in a table</span></span> |<span data-ttu-id="4e549-198">Omezeno pouze kapacity účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="4e549-198">Limited only by the capacity of the storage account</span></span> |
| <span data-ttu-id="4e549-199">Počet entit v oddílu</span><span class="sxs-lookup"><span data-stu-id="4e549-199">Number of entities in a partition</span></span> |<span data-ttu-id="4e549-200">Omezeno pouze kapacity účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="4e549-200">Limited only by the capacity of the storage account</span></span> |
| <span data-ttu-id="4e549-201">Velikost jednotlivých entit</span><span class="sxs-lookup"><span data-stu-id="4e549-201">Size of an individual entity</span></span> |<span data-ttu-id="4e549-202">Až 1 MB delší než 255 vlastnosti (včetně **PartitionKey**, **RowKey**, a **časové razítko**)</span><span class="sxs-lookup"><span data-stu-id="4e549-202">Up to 1 MB with a maximum of 255 properties (including the **PartitionKey**, **RowKey**, and **Timestamp**)</span></span> |
| <span data-ttu-id="4e549-203">Velikost **PartitionKey**</span><span class="sxs-lookup"><span data-stu-id="4e549-203">Size of the **PartitionKey**</span></span> |<span data-ttu-id="4e549-204">A řetězec velikost až 1 KB</span><span class="sxs-lookup"><span data-stu-id="4e549-204">A string up to 1 KB in size</span></span> |
| <span data-ttu-id="4e549-205">Velikost **RowKey**</span><span class="sxs-lookup"><span data-stu-id="4e549-205">Size of the **RowKey**</span></span> |<span data-ttu-id="4e549-206">A řetězec velikost až 1 KB</span><span class="sxs-lookup"><span data-stu-id="4e549-206">A string up to 1 KB in size</span></span> |
| <span data-ttu-id="4e549-207">Velikost skupiny Entity transakce</span><span class="sxs-lookup"><span data-stu-id="4e549-207">Size of an Entity Group Transaction</span></span> |<span data-ttu-id="4e549-208">Transakce může obsahovat maximálně 100 entit a datové části musí být menší než 4 MB.</span><span class="sxs-lookup"><span data-stu-id="4e549-208">A transaction can include at most 100 entities and the payload must be less than 4 MB in size.</span></span> <span data-ttu-id="4e549-209">EGT lze aktualizovat pouze entity jednou.</span><span class="sxs-lookup"><span data-stu-id="4e549-209">An EGT can only update an entity once.</span></span> |

<span data-ttu-id="4e549-210">Další informace najdete v tématu [Principy datového modelu služby Table](http://msdn.microsoft.com/library/azure/dd179338.aspx).</span><span class="sxs-lookup"><span data-stu-id="4e549-210">For more information, see [Understanding the Table Service Data Model](http://msdn.microsoft.com/library/azure/dd179338.aspx).</span></span>  

### <a name="cost-considerations"></a><span data-ttu-id="4e549-211">Aspekty náklady</span><span class="sxs-lookup"><span data-stu-id="4e549-211">Cost considerations</span></span>
<span data-ttu-id="4e549-212">Table storage je relativně levný, ale by měla obsahovat odhadované náklady pro využití kapacity a o množství transakcí v rámci testování jakéhokoli řešení, které používá služby Table.</span><span class="sxs-lookup"><span data-stu-id="4e549-212">Table storage is relatively inexpensive, but you should include cost estimates for both capacity usage and the quantity of transactions as part of your evaluation of any solution that uses the Table service.</span></span> <span data-ttu-id="4e549-213">V mnoha scénářích ukládání nenormalizované nebo duplicitní data za účelem zlepšení výkonu nebo škálovatelnost řešení je ale platný přístup provést.</span><span class="sxs-lookup"><span data-stu-id="4e549-213">However, in many scenarios storing denormalized or duplicate data in order to improve the performance or scalability of your solution is a valid approach to take.</span></span> <span data-ttu-id="4e549-214">Další informace o cenách najdete v tématu [Azure Storage – ceny](https://azure.microsoft.com/pricing/details/storage/).</span><span class="sxs-lookup"><span data-stu-id="4e549-214">For more information about pricing, see [Azure Storage Pricing](https://azure.microsoft.com/pricing/details/storage/).</span></span>  

## <a name="guidelines-for-table-design"></a><span data-ttu-id="4e549-215">Pokyny pro návrh tabulky</span><span class="sxs-lookup"><span data-stu-id="4e549-215">Guidelines for table design</span></span>
<span data-ttu-id="4e549-216">Tyto seznamy shrnují některé z klíčových pokyny, které jste měli vzít v úvahu při navrhování vaší tabulky a tato příručka se jimi bude zabývat v podrobněji později.</span><span class="sxs-lookup"><span data-stu-id="4e549-216">These lists summarize some of the key guidelines you should keep in mind when you are designing your tables, and this guide will address them all in more detail later in.</span></span> <span data-ttu-id="4e549-217">Tyto pokyny jsou velmi liší od pokynů, které by obvykle postupujte podle návrh relační databáze.</span><span class="sxs-lookup"><span data-stu-id="4e549-217">These guidelines are very different from the guidelines you would typically follow for relational database design.</span></span>  

<span data-ttu-id="4e549-218">Navrhování řešení služby tabulky jako *číst* efektivní:</span><span class="sxs-lookup"><span data-stu-id="4e549-218">Designing your Table service solution to be *read* efficient:</span></span>

* <span data-ttu-id="4e549-219">***Návrh pro dotazování v aplikacích číst náročné.***</span><span class="sxs-lookup"><span data-stu-id="4e549-219">***Design for querying in read-heavy applications.***</span></span> <span data-ttu-id="4e549-220">Při navrhování vaší tabulky, vezměte v úvahu dotazy (zejména latence citlivé ty), které se provedou předtím, než si myslíte o tom, jak bude aktualizovat váš entity.</span><span class="sxs-lookup"><span data-stu-id="4e549-220">When you are designing your tables, think about the queries (especially the latency sensitive ones) that you will execute before you think about how you will update your entities.</span></span> <span data-ttu-id="4e549-221">To obvykle vede efektivní a původce řešení.</span><span class="sxs-lookup"><span data-stu-id="4e549-221">This typically results in an efficient and performant solution.</span></span>  
* <span data-ttu-id="4e549-222">***Zadejte klíč oddílu a RowKey v dotazech.***</span><span class="sxs-lookup"><span data-stu-id="4e549-222">***Specify both PartitionKey and RowKey in your queries.***</span></span> <span data-ttu-id="4e549-223">*Bod dotazy* , jako jsou ty se nejúčinnější dotazů služby table.</span><span class="sxs-lookup"><span data-stu-id="4e549-223">*Point queries* such as these are the most efficient table service queries.</span></span>  
* <span data-ttu-id="4e549-224">***Zvažte uložení duplicitní kopie entit.***</span><span class="sxs-lookup"><span data-stu-id="4e549-224">***Consider storing duplicate copies of entities.***</span></span> <span data-ttu-id="4e549-225">Table storage je levných, zvažte uložení stejné entity vícekrát (s různými klíči) umožňující efektivnější dotazy.</span><span class="sxs-lookup"><span data-stu-id="4e549-225">Table storage is cheap so consider storing the same entity multiple times (with different keys) to enable more efficient queries.</span></span>  
* <span data-ttu-id="4e549-226">***Vezměte v úvahu denormalizing vaše data.***</span><span class="sxs-lookup"><span data-stu-id="4e549-226">***Consider denormalizing your data.***</span></span> <span data-ttu-id="4e549-227">Table storage je levných, vezměte v úvahu denormalizing vaše data.</span><span class="sxs-lookup"><span data-stu-id="4e549-227">Table storage is cheap so consider denormalizing your data.</span></span> <span data-ttu-id="4e549-228">Například uložte souhrn entit, tak, aby dotazy pro shromáždění dat stačí pro přístup k jedné entity.</span><span class="sxs-lookup"><span data-stu-id="4e549-228">For example, store summary entities so that queries for aggregate data only need to access a single entity.</span></span>  
* <span data-ttu-id="4e549-229">***Použijte hodnoty složeného klíče.***</span><span class="sxs-lookup"><span data-stu-id="4e549-229">***Use compound key values.***</span></span> <span data-ttu-id="4e549-230">Jsou pouze klíče máte **PartitionKey** a **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="4e549-230">The only keys you have are **PartitionKey** and **RowKey**.</span></span> <span data-ttu-id="4e549-231">Například použijte složené klíče hodnoty povolit cesty alternativní s klíčem přístupu na entity.</span><span class="sxs-lookup"><span data-stu-id="4e549-231">For example, use compound key values to enable alternate keyed access paths to entities.</span></span>  
* <span data-ttu-id="4e549-232">***Použijte projekce dotazu.***</span><span class="sxs-lookup"><span data-stu-id="4e549-232">***Use query projection.***</span></span> <span data-ttu-id="4e549-233">Můžete snížit množství dat, který přenos přes síť pomocí dotazů, které vyberte pole, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="4e549-233">You can reduce the amount of data that you transfer over the network by using queries that select just the fields you need.</span></span>  

<span data-ttu-id="4e549-234">Navrhování řešení služby tabulky jako *zápisu* efektivní:</span><span class="sxs-lookup"><span data-stu-id="4e549-234">Designing your Table service solution to be *write* efficient:</span></span>  

* <span data-ttu-id="4e549-235">***Nevytvářejte aktivní oddíly.***</span><span class="sxs-lookup"><span data-stu-id="4e549-235">***Do not create hot partitions.***</span></span> <span data-ttu-id="4e549-236">Zvolte klíče, které vám umožní rozloženy více oddílů v libovolném bodě čas své žádosti.</span><span class="sxs-lookup"><span data-stu-id="4e549-236">Choose keys that enable you to spread your requests across multiple partitions at any point of time.</span></span>  
* <span data-ttu-id="4e549-237">***Vyhněte se špičky v provozu.***</span><span class="sxs-lookup"><span data-stu-id="4e549-237">***Avoid spikes in traffic.***</span></span> <span data-ttu-id="4e549-238">Funkce Smooth provoz přiměřené období a vyhnout se špičky v provozu.</span><span class="sxs-lookup"><span data-stu-id="4e549-238">Smooth the traffic over a reasonable period of time and avoid spikes in traffic.</span></span>
* <span data-ttu-id="4e549-239">***Nevytvářejte nutně do samostatné tabulky pro každý typ entity.***</span><span class="sxs-lookup"><span data-stu-id="4e549-239">***Don't necessarily create a separate table for each type of entity.***</span></span> <span data-ttu-id="4e549-240">Pokud požadujete jednotlivé transakce mezi typy entit, můžete uložit těchto více typů entit ve stejném oddílu ve stejné tabulce.</span><span class="sxs-lookup"><span data-stu-id="4e549-240">When you require atomic transactions across entity types, you can store these multiple entity types in the same partition in the same table.</span></span>
* <span data-ttu-id="4e549-241">***Vezměte v úvahu maximální propustnost, které musí dosáhnout.***</span><span class="sxs-lookup"><span data-stu-id="4e549-241">***Consider the maximum throughput you must achieve.***</span></span> <span data-ttu-id="4e549-242">Musíte mít na paměti cíle škálovatelnosti pro službu tabulky a ujistěte se, že váš návrh nezpůsobí je delší, než je.</span><span class="sxs-lookup"><span data-stu-id="4e549-242">You must be aware of the scalability targets for the Table service and ensure that your design will not cause you to exceed them.</span></span>  

<span data-ttu-id="4e549-243">Při čtení této příručky, zobrazí se příklady, které všechny tyto zásady uvedené do praxe.</span><span class="sxs-lookup"><span data-stu-id="4e549-243">As you read this guide, you will see examples that put all of these principles into practice.</span></span>  

## <a name="design-for-querying"></a><span data-ttu-id="4e549-244">Návrh pro dotazování</span><span class="sxs-lookup"><span data-stu-id="4e549-244">Design for querying</span></span>
<span data-ttu-id="4e549-245">Řešení služby TABLE lze číst náročné na prostředky, náročné zápisu nebo kombinaci těchto dvou.</span><span class="sxs-lookup"><span data-stu-id="4e549-245">Table service solutions may be read intensive, write intensive, or a mix of the two.</span></span> <span data-ttu-id="4e549-246">Tato část se zaměřuje na skutečnosti, které je berte v úvahu při navrhování vaší služby Table pro podporu operací čtení efektivně.</span><span class="sxs-lookup"><span data-stu-id="4e549-246">This section focuses on the things to bear in mind when you are designing your Table service to support read operations efficiently.</span></span> <span data-ttu-id="4e549-247">Návrh podporuje efektivně zapisovacích operací je obvykle také efektivní pro operace zápisu.</span><span class="sxs-lookup"><span data-stu-id="4e549-247">Typically, a design that supports read operations efficiently is also efficient for write operations.</span></span> <span data-ttu-id="4e549-248">Existují však další aspekty a berte v úvahu při navrhování pro podporu operací zápisu, popsané v další části [návrhu pro úpravu dat](#design-for-data-modification).</span><span class="sxs-lookup"><span data-stu-id="4e549-248">However, there are additional considerations to bear in mind when designing to support write operations, discussed in the next section, [Design for data modification](#design-for-data-modification).</span></span>

<span data-ttu-id="4e549-249">Je to dobrý výchozí bod pro návrh řešení služby Tabulka vám umožní číst data efektivně požádat "jaké dotazy bude Moje aplikace potřeba provést k načtení dat, které potřebuje ze služby Table?"</span><span class="sxs-lookup"><span data-stu-id="4e549-249">A good starting point for designing your Table service solution to enable you to read data efficiently is to ask "What queries will my application need to execute to retrieve the data it needs from the Table service?"</span></span>  

> [!NOTE]
> <span data-ttu-id="4e549-250">Ve službě Table je potřeba získat návrh správné předem vzhledem k tomu, že je složitá a nákladná později změnit.</span><span class="sxs-lookup"><span data-stu-id="4e549-250">With the Table service, it's important to get the design correct up front because it's difficult and expensive to change it later.</span></span> <span data-ttu-id="4e549-251">Například v relační databázi je často možné problémy s výkonem adresu jednoduše přidáním indexy k existující databázi: Toto není nabízet služby Table.</span><span class="sxs-lookup"><span data-stu-id="4e549-251">For example, in a relational database it's often possible to address performance issues simply by adding indexes to an existing database: this is not an option with the Table service.</span></span>  
> 
> 

<span data-ttu-id="4e549-252">Tato část se zaměřuje na klíčových otázek, které musíte vyřešit, když tabulek pro zadávání dotazů.</span><span class="sxs-lookup"><span data-stu-id="4e549-252">This section focuses on the key issues you must address when you design your tables for querying.</span></span> <span data-ttu-id="4e549-253">Obsahuje následující témata v této části:</span><span class="sxs-lookup"><span data-stu-id="4e549-253">The topics covered in this section include:</span></span>

* [<span data-ttu-id="4e549-254">Jak vaši volbu PartitionKey a RowKey ovlivňuje výkon dotazů</span><span class="sxs-lookup"><span data-stu-id="4e549-254">How your choice of PartitionKey and RowKey impacts query performance</span></span>](#how-your-choice-of-partitionkey-and-rowkey-impacts-query-performance)
* [<span data-ttu-id="4e549-255">Výběr vhodné PartitionKey</span><span class="sxs-lookup"><span data-stu-id="4e549-255">Choosing an appropriate PartitionKey</span></span>](#choosing-an-appropriate-partitionkey)
* [<span data-ttu-id="4e549-256">Optimalizace dotazů pro služby Table</span><span class="sxs-lookup"><span data-stu-id="4e549-256">Optimizing queries for the Table service</span></span>](#optimizing-queries-for-the-table-service)
* [<span data-ttu-id="4e549-257">Řazení dat ve službě Table</span><span class="sxs-lookup"><span data-stu-id="4e549-257">Sorting data in the Table service</span></span>](#sorting-data-in-the-table-service)

### <a name="how-your-choice-of-partitionkey-and-rowkey-impacts-query-performance"></a><span data-ttu-id="4e549-258">Jak vaši volbu PartitionKey a RowKey ovlivňuje výkon dotazů</span><span class="sxs-lookup"><span data-stu-id="4e549-258">How your choice of PartitionKey and RowKey impacts query performance</span></span>
<span data-ttu-id="4e549-259">Následující příklady předpokládají služby table je ukládání entit zaměstnanec s následující strukturou (většina příkladů vynechejte **časové razítko** vlastnost pro přehlednost):</span><span class="sxs-lookup"><span data-stu-id="4e549-259">The following examples assume the table service is storing employee entities with the following structure (most of the examples omit the **Timestamp** property for clarity):</span></span>  

| <span data-ttu-id="4e549-260">*Název sloupce*</span><span class="sxs-lookup"><span data-stu-id="4e549-260">*Column name*</span></span> | <span data-ttu-id="4e549-261">*Datový typ*</span><span class="sxs-lookup"><span data-stu-id="4e549-261">*Data type*</span></span> |
| --- | --- |
| <span data-ttu-id="4e549-262">**PartitionKey** (název oddělení)</span><span class="sxs-lookup"><span data-stu-id="4e549-262">**PartitionKey** (Department Name)</span></span> |<span data-ttu-id="4e549-263">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4e549-263">String</span></span> |
| <span data-ttu-id="4e549-264">**RowKey** (Id zaměstnance)</span><span class="sxs-lookup"><span data-stu-id="4e549-264">**RowKey** (Employee Id)</span></span> |<span data-ttu-id="4e549-265">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4e549-265">String</span></span> |
| <span data-ttu-id="4e549-266">**FirstName**</span><span class="sxs-lookup"><span data-stu-id="4e549-266">**FirstName**</span></span> |<span data-ttu-id="4e549-267">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4e549-267">String</span></span> |
| <span data-ttu-id="4e549-268">**Příjmení**</span><span class="sxs-lookup"><span data-stu-id="4e549-268">**LastName**</span></span> |<span data-ttu-id="4e549-269">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4e549-269">String</span></span> |
| <span data-ttu-id="4e549-270">**Stáří**</span><span class="sxs-lookup"><span data-stu-id="4e549-270">**Age**</span></span> |<span data-ttu-id="4e549-271">Integer</span><span class="sxs-lookup"><span data-stu-id="4e549-271">Integer</span></span> |
| <span data-ttu-id="4e549-272">**EmailAddress**</span><span class="sxs-lookup"><span data-stu-id="4e549-272">**EmailAddress**</span></span> |<span data-ttu-id="4e549-273">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4e549-273">String</span></span> |

<span data-ttu-id="4e549-274">Ve výše uvedené části [Přehled služby Azure Table](#overview) popisuje některé klíčové funkce služby Azure Table, které mají přímý vliv na návrh pro dotaz.</span><span class="sxs-lookup"><span data-stu-id="4e549-274">The earlier section [Azure Table service overview](#overview) describes some of the key features of the Azure Table service that have a direct influence on designing for query.</span></span> <span data-ttu-id="4e549-275">To mít za následek následující obecné pokyny pro návrh tabulky služby dotazy.</span><span class="sxs-lookup"><span data-stu-id="4e549-275">These result in the following general guidelines for designing Table service queries.</span></span> <span data-ttu-id="4e549-276">Všimněte si, že syntaxe filtru použít v následujících příkladech je ze služby Table rozhraní REST API, další informace najdete v tématu [dotazu entity](http://msdn.microsoft.com/library/azure/dd179421.aspx).</span><span class="sxs-lookup"><span data-stu-id="4e549-276">Note that the filter syntax used in the examples below is from the Table service REST API, for more information see [Query Entities](http://msdn.microsoft.com/library/azure/dd179421.aspx).</span></span>  

* <span data-ttu-id="4e549-277">A ***bodu dotazu*** je nejúčinnější vyhledávání používat a doporučuje se má být použit pro vysoký počet vyhledávání nebo vyhledávání, které vyžadují nejnižší latenci.</span><span class="sxs-lookup"><span data-stu-id="4e549-277">A ***Point Query*** is the most efficient lookup to use and is recommended to be used for high-volume lookups or lookups requiring lowest latency.</span></span> <span data-ttu-id="4e549-278">Takové dotazy můžete použít indexy velmi efektivně najít jednotlivých entit zadáním i **PartitionKey** a **RowKey** hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4e549-278">Such a query can use the indexes to locate an individual entity very efficiently by specifying both the **PartitionKey** and **RowKey** values.</span></span> <span data-ttu-id="4e549-279">Příklad: $filter = (PartitionKey eq 'Prodej') a (RowKey eq: 2.)</span><span class="sxs-lookup"><span data-stu-id="4e549-279">For example: $filter=(PartitionKey eq 'Sales') and (RowKey eq '2')</span></span>  
* <span data-ttu-id="4e549-280">Druhý nejlépe je ***dotazu na rozsah*** používající **PartitionKey** a filtry pro celou řadu **RowKey** hodnot se má vrátit více než jedna entita.</span><span class="sxs-lookup"><span data-stu-id="4e549-280">Second best is a ***Range Query*** that uses the **PartitionKey** and filters on a range of **RowKey** values to return more than one entity.</span></span> <span data-ttu-id="4e549-281">**PartitionKey** hodnota identifikuje na konkrétní oddíl a **RowKey** hodnoty identifikovat podmnožinu entity v oddílu.</span><span class="sxs-lookup"><span data-stu-id="4e549-281">The **PartitionKey** value identifies a specific partition, and the **RowKey** values identify a subset of the entities in that partition.</span></span> <span data-ttu-id="4e549-282">Příklad: $filter = PartitionKey eq 'prodeje a RowKey ge' a RowKey lt se.</span><span class="sxs-lookup"><span data-stu-id="4e549-282">For example: $filter=PartitionKey eq 'Sales' and RowKey ge 'S' and RowKey lt 'T'</span></span>  
* <span data-ttu-id="4e549-283">Je třetí nejlepší ***oddílu kontrolovat*** používající **PartitionKey** a filtry na jiné neklíčovou vlastnost a který může vrátit více než jedna entita.</span><span class="sxs-lookup"><span data-stu-id="4e549-283">Third best is a ***Partition Scan*** that uses the **PartitionKey** and filters on another non-key property and that may return more than one entity.</span></span> <span data-ttu-id="4e549-284">**PartitionKey** hodnota identifikuje konkrétního oddílu a vyberte pro podmnožinu entity v oddílu hodnoty vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="4e549-284">The **PartitionKey** value identifies a specific partition, and the property values select for a subset of the entities in that partition.</span></span> <span data-ttu-id="4e549-285">Příklad: $filter = PartitionKey eq 'prodeje a LastName eq: Váša.</span><span class="sxs-lookup"><span data-stu-id="4e549-285">For example: $filter=PartitionKey eq 'Sales' and LastName eq 'Smith'</span></span>  
* <span data-ttu-id="4e549-286">A ***tabulky kontrolovat*** nezahrnuje **PartitionKey** a je velmi neefektivní, protože všechny oddíly, které tvoří tabulku zase pro všechny odpovídající entity vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="4e549-286">A ***Table Scan*** does not include the **PartitionKey** and is very inefficient because it searches all of the partitions that make up your table in turn for any matching entities.</span></span> <span data-ttu-id="4e549-287">Provede prohledávání tabulky bez ohledu na to, zda filtr používá **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="4e549-287">It will perform a table scan regardless of whether or not your filter uses the **RowKey**.</span></span> <span data-ttu-id="4e549-288">Příklad: $filter = LastName eq 'Petr.</span><span class="sxs-lookup"><span data-stu-id="4e549-288">For example: $filter=LastName eq 'Jones'</span></span>  
* <span data-ttu-id="4e549-289">Dotazy, které vracejí víc entit obnoví v nich seřazeny ve **PartitionKey** a **RowKey** pořadí.</span><span class="sxs-lookup"><span data-stu-id="4e549-289">Queries that return multiple entities return them sorted in **PartitionKey** and **RowKey** order.</span></span> <span data-ttu-id="4e549-290">Abyste se vyhnuli, opětné řazení entity v klientovi, vyberte **RowKey** , který definuje nejběžnější pořadí řazení.</span><span class="sxs-lookup"><span data-stu-id="4e549-290">To avoid resorting the entities in the client, choose a **RowKey** that defines the most common sort order.</span></span>  

<span data-ttu-id="4e549-291">Všimněte si, že pomocí "**nebo**" k zadání filtru na základě **RowKey** hodnoty výsledků v oddílu kontroly a nepovažuje se za dotazu na rozsah.</span><span class="sxs-lookup"><span data-stu-id="4e549-291">Note that using an "**or**" to specify a filter based on **RowKey** values results in a partition scan and is not treated as a range query.</span></span> <span data-ttu-id="4e549-292">Proto byste neměli dotazy, které pomocí filtrů, jako například: $filter = PartitionKey eq 'Prodej' a (RowKey eq '121' nebo RowKey eq "322")</span><span class="sxs-lookup"><span data-stu-id="4e549-292">Therefore, you should avoid queries that use filters such as: $filter=PartitionKey eq 'Sales' and (RowKey eq '121' or RowKey eq '322')</span></span>  

<span data-ttu-id="4e549-293">Příklady kódu na straně klienta, které používají Klientská knihovna pro úložiště na provedení efektivní dotazů najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="4e549-293">For examples of client-side code that use the Storage Client Library to execute efficient queries, see:</span></span>  

* [<span data-ttu-id="4e549-294">Spuštění dotazu na bodu pomocí klientské knihovny pro úložiště</span><span class="sxs-lookup"><span data-stu-id="4e549-294">Executing a point query using the Storage Client Library</span></span>](#executing-a-point-query-using-the-storage-client-library)
* [<span data-ttu-id="4e549-295">Načítání více entit pomocí LINQ</span><span class="sxs-lookup"><span data-stu-id="4e549-295">Retrieving multiple entities using LINQ</span></span>](#retrieving-multiple-entities-using-linq)
* [<span data-ttu-id="4e549-296">Projekce na straně serveru</span><span class="sxs-lookup"><span data-stu-id="4e549-296">Server-side projection</span></span>](#server-side-projection)  

<span data-ttu-id="4e549-297">Příklady kódu na straně klienta, který může zpracovat více typy entit, které jsou uloženy ve stejné tabulce najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="4e549-297">For examples of client-side code that can handle multiple entity types stored in the same table, see:</span></span>  

* [<span data-ttu-id="4e549-298">Práce s typy heterogenní entit</span><span class="sxs-lookup"><span data-stu-id="4e549-298">Working with heterogeneous entity types</span></span>](#working-with-heterogeneous-entity-types)  

### <a name="choosing-an-appropriate-partitionkey"></a><span data-ttu-id="4e549-299">Výběr vhodné PartitionKey</span><span class="sxs-lookup"><span data-stu-id="4e549-299">Choosing an appropriate PartitionKey</span></span>
<span data-ttu-id="4e549-300">Vaši volbu **PartitionKey** měli vyvážit potřeba umožňuje použití EGTs (k zajištění konzistence) proti požadavku, distribuovat vaší entity napříč více oddílů (aby škálovatelné řešení).</span><span class="sxs-lookup"><span data-stu-id="4e549-300">Your choice of **PartitionKey** should balance the need to enables the use of EGTs (to ensure consistency) against the requirement to distribute your entities across multiple partitions (to ensure a scalable solution).</span></span>  

<span data-ttu-id="4e549-301">V jedné extreme může ukládat všechny entity v jeden oddíl, ale to může omezit škálovatelnost řešení a by bránily schopnost Vyrovnávání zatížení požadavky služby table.</span><span class="sxs-lookup"><span data-stu-id="4e549-301">At one extreme, you could store all your entities in a single partition, but this may limit the scalability of your solution and would prevent the table service from being able to load-balance requests.</span></span> <span data-ttu-id="4e549-302">V jiných extreme může ukládat jednu entitu na oddíl, který bude vysoce škálovatelné a která umožňuje služby table na Vyrovnávání zatížení požadavky, ale které by zabránit vám v použití transakcí skupiny entity.</span><span class="sxs-lookup"><span data-stu-id="4e549-302">At the other extreme, you could store one entity per partition, which would be highly scalable and which enables the table service to load-balance requests, but which would prevent you from using entity group transactions.</span></span>  

<span data-ttu-id="4e549-303">Ideálu **PartitionKey** jeden, který vám umožňuje používat efektivní dotazy a má dostatek oddíly pro zajištění škálovatelné řešení.</span><span class="sxs-lookup"><span data-stu-id="4e549-303">An ideal **PartitionKey** is one that enables you to use efficient queries and that has sufficient partitions to ensure your solution is scalable.</span></span> <span data-ttu-id="4e549-304">Obvykle zjistíte, že vaší entity budou mít vhodný vlastnost, která distribuuje vaší entity v dostatečná oddíly.</span><span class="sxs-lookup"><span data-stu-id="4e549-304">Typically, you will find that your entities will have a suitable property that distributes your entities across sufficient partitions.</span></span>

> [!NOTE]
> <span data-ttu-id="4e549-305">V systému, která uchovává informace o uživatele nebo zaměstnancům, například ID uživatele může být dobrým PartitionKey.</span><span class="sxs-lookup"><span data-stu-id="4e549-305">For example, in a system that stores information about users or employees, UserID may be a good PartitionKey.</span></span> <span data-ttu-id="4e549-306">Může mít několik entit, které pomocí dané ID uživatele jako klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="4e549-306">You may have several entities that use a given UserID as the partition key.</span></span> <span data-ttu-id="4e549-307">Každá entita, která ukládá data o uživateli se seskupují do jednoho oddílu, a proto tyto entity jsou přístupné přes transakce skupiny entity, ale stále mít vysoce škálovatelná.</span><span class="sxs-lookup"><span data-stu-id="4e549-307">Each entity that stores data about a user is grouped into a single partition, and so these entities are accessible via entity group transactions, while still being highly scalable.</span></span>
> 
> 

<span data-ttu-id="4e549-308">Další rozhodnutí v požadovaném **PartitionKey** které se týkají jak bude vložit, aktualizovat a odstraňovat entity: najdete v části [návrhu pro úpravu dat](#design-for-data-modification) níže.</span><span class="sxs-lookup"><span data-stu-id="4e549-308">There are additional considerations in your choice of **PartitionKey** that relate to how you will insert, update, and delete entities: see the section [Design for data modification](#design-for-data-modification) below.</span></span>  

### <a name="optimizing-queries-for-the-table-service"></a><span data-ttu-id="4e549-309">Optimalizace dotazů pro služby Table</span><span class="sxs-lookup"><span data-stu-id="4e549-309">Optimizing queries for the Table service</span></span>
<span data-ttu-id="4e549-310">Služby Table automaticky indexuje entity produktu pomocí **PartitionKey** a **RowKey** hodnot v jednom clusterovaný index, proto důvod, proč bodu dotazy jsou nejúčinnější používat.</span><span class="sxs-lookup"><span data-stu-id="4e549-310">The Table service automatically indexes your entities using the **PartitionKey** and **RowKey** values in a single clustered index, hence the reason that point queries are the most efficient to use.</span></span> <span data-ttu-id="4e549-311">Však nejsou žádné indexy než tu, která na clusterovaný index na **PartitionKey** a **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="4e549-311">However, there are no indexes other than that on the clustered index on the **PartitionKey** and **RowKey**.</span></span>

<span data-ttu-id="4e549-312">Řada návrhů musí splňovat požadavky na povolení vyhledávání entit na základě několika kritérií.</span><span class="sxs-lookup"><span data-stu-id="4e549-312">Many designs must meet requirements to enable lookup of entities based on multiple criteria.</span></span> <span data-ttu-id="4e549-313">Například hledání zaměstnanec entity podle e-mailu, identifikační číslo zaměstnance nebo příjmení.</span><span class="sxs-lookup"><span data-stu-id="4e549-313">For example, locating employee entities based on email, employee id, or last name.</span></span> <span data-ttu-id="4e549-314">Tyto vzory v části [vzory návrhu tabulky](#table-design-patterns) adres tyto typy požadavek a popisují způsoby obcházet fakt, že služby Table nenabízí sekundární indexy:</span><span class="sxs-lookup"><span data-stu-id="4e549-314">The following patterns in the section [Table Design Patterns](#table-design-patterns) address these types of requirement and describe ways of working around the fact that the Table service does not provide secondary indexes:</span></span>  

* <span data-ttu-id="4e549-315">[Vzor sekundární index Intra-partition](#intra-partition-secondary-index-pattern) -uložit více kopií každou entitu s využitím různých **RowKey** hodnoty (ve stejném oddílu) k povolení rychlé a efektivní vyhledávání a alternativní pořadí řazení pomocí různých **RowKey** hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4e549-315">[Intra-partition secondary index pattern](#intra-partition-secondary-index-pattern) - Store multiple copies of each entity using different **RowKey** values (in the same partition) to enable fast and efficient lookups and alternate sort orders by using different **RowKey** values.</span></span>  
* <span data-ttu-id="4e549-316">[Vzor mezi oddíl sekundární index](#inter-partition-secondary-index-pattern) – ukládání více kopií každé entity pomocí různých hodnot RowKey v samostatné oddíly nebo v samostatných tabulkách umožňující rychlé a efektivní vyhledávání a řazení alternativní řadí pomocí různých **RowKey** hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4e549-316">[Inter-partition secondary index pattern](#inter-partition-secondary-index-pattern) - Store multiple copies of each entity using different RowKey values in separate partitions or in separate tables to enable fast and efficient lookups and alternate sort orders by using different **RowKey** values.</span></span>  
* <span data-ttu-id="4e549-317">[Index entity vzor](#index-entities-pattern) -udržovat index entity umožňující efektivní hledání, které vrátí seznamy entit.</span><span class="sxs-lookup"><span data-stu-id="4e549-317">[Index Entities Pattern](#index-entities-pattern) - Maintain index entities to enable efficient searches that return lists of entities.</span></span>  

### <a name="sorting-data-in-the-table-service"></a><span data-ttu-id="4e549-318">Řazení dat ve službě Table</span><span class="sxs-lookup"><span data-stu-id="4e549-318">Sorting data in the Table service</span></span>
<span data-ttu-id="4e549-319">Služba Table vrací entity ve vzestupném pořadí podle **PartitionKey** a potom podle **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="4e549-319">The Table service returns entities sorted in ascending order based on **PartitionKey** and then by **RowKey**.</span></span> <span data-ttu-id="4e549-320">Tyto klíče jsou hodnoty řetězce a aby správně řazení číselných hodnot, měli byste je převést na pevnou délkou a je odsadí nulami.</span><span class="sxs-lookup"><span data-stu-id="4e549-320">These keys are string values and to ensure that numeric values sort correctly, you should convert them to a fixed length and pad them with zeroes.</span></span> <span data-ttu-id="4e549-321">Například, pokud hodnota id zaměstnance, můžete použít jako **RowKey** je celočíselná hodnota, bude třeba převést identifikační číslo zaměstnance **123** k **00000123**.</span><span class="sxs-lookup"><span data-stu-id="4e549-321">For example, if the employee id value you use as the **RowKey** is an integer value, you should convert employee id **123** to **00000123**.</span></span>  

<span data-ttu-id="4e549-322">Mnoho aplikací mít požadavky pro použití dat seřazeny v různém pořadí: například řazení zaměstnanci podle názvu nebo díky připojení ke službě data.</span><span class="sxs-lookup"><span data-stu-id="4e549-322">Many applications have requirements to use data sorted in different orders: for example, sorting employees by name, or by joining date.</span></span> <span data-ttu-id="4e549-323">Tyto vzory v části [vzory návrhu tabulky](#table-design-patterns) adres postup alternativní pořadí řazení pro vaše entity:</span><span class="sxs-lookup"><span data-stu-id="4e549-323">The following patterns in the section [Table Design Patterns](#table-design-patterns) address how to alternate sort orders for your entities:</span></span>  

* <span data-ttu-id="4e549-324">[Vzor sekundární index Intra-partition](#intra-partition-secondary-index-pattern) – ukládání více kopií každé entity pomocí různých hodnot RowKey (ve stejném oddílu) umožňující rychlé a efektivní vyhledávání a řazení alternativní řadí za použití různých hodnot RowKey.</span><span class="sxs-lookup"><span data-stu-id="4e549-324">[Intra-partition secondary index pattern](#intra-partition-secondary-index-pattern) - Store multiple copies of each entity using different RowKey values (in the same partition) to enable fast and efficient lookups and alternate sort orders by using different RowKey values.</span></span>  
* <span data-ttu-id="4e549-325">[Vzor mezi oddíl sekundární index](#inter-partition-secondary-index-pattern) – ukládání více kopií každé entity pomocí různých hodnot RowKey v samostatné oddíly v samostatných tabulkách umožňující rychlé a efektivní vyhledávání a řazení alternativní řadí za použití různých hodnot RowKey.</span><span class="sxs-lookup"><span data-stu-id="4e549-325">[Inter-partition secondary index pattern](#inter-partition-secondary-index-pattern) - Store multiple copies of each entity using different RowKey values in separate partitions in separate tables to enable fast and efficient lookups and alternate sort orders by using different RowKey values.</span></span>
* <span data-ttu-id="4e549-326">[Vzor protokolu poškozené databáze](#log-tail-pattern) -načíst  *n*  entity naposledy přidané do oddílu pomocí **RowKey** hodnotu, která seřadí zpětné datum a čas pořadí.</span><span class="sxs-lookup"><span data-stu-id="4e549-326">[Log tail pattern](#log-tail-pattern) - Retrieve the *n* entities most recently added to a partition by using a **RowKey** value that sorts in reverse date and time order.</span></span>  

## <a name="design-for-data-modification"></a><span data-ttu-id="4e549-327">Návrh pro úpravu dat</span><span class="sxs-lookup"><span data-stu-id="4e549-327">Design for data modification</span></span>
<span data-ttu-id="4e549-328">Tato část se zaměřuje na aspekty návrhu pro optimalizaci vložení, aktualizace a odstraní.</span><span class="sxs-lookup"><span data-stu-id="4e549-328">This section focuses on the design considerations for optimizing inserts, updates, and deletes.</span></span> <span data-ttu-id="4e549-329">V některých případech musíte vyhodnotit kompromis mezi návrhů, které je optimální pro dotazování na návrhů, které je optimální pro úpravu dat stejným způsobem jako v návrhy pro relační databáze (i když techniky pro správu kompromisy návrhu se liší v relační databázi).</span><span class="sxs-lookup"><span data-stu-id="4e549-329">In some cases, you will need to evaluate the trade-off between designs that optimize for querying against designs that optimize for data modification just as you do in designs for relational databases (although the techniques for managing the design trade-offs are different in a relational database).</span></span> <span data-ttu-id="4e549-330">V části [vzory návrhu tabulky](#table-design-patterns) popisuje některé vzory podrobné návrhu pro službu tabulky a zvýrazňuje některé tyto kompromis.</span><span class="sxs-lookup"><span data-stu-id="4e549-330">The section [Table Design Patterns](#table-design-patterns) describes some detailed design patterns for the Table service and highlights some these trade-offs.</span></span> <span data-ttu-id="4e549-331">V praxi zjistíte, že řada návrhů optimalizované pro dotazování entity také fungovat i pro úpravy entity.</span><span class="sxs-lookup"><span data-stu-id="4e549-331">In practice, you will find that many designs optimized for querying entities also work well for modifying entities.</span></span>  

### <a name="optimizing-the-performance-of-insert-update-and-delete-operations"></a><span data-ttu-id="4e549-332">Optimalizace výkonu vložit, aktualizovat a operace odstranění</span><span class="sxs-lookup"><span data-stu-id="4e549-332">Optimizing the performance of insert, update, and delete operations</span></span>
<span data-ttu-id="4e549-333">Aktualizace nebo odstranění entity, musí být schopen identifikovat pomocí **PartitionKey** a **RowKey** hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4e549-333">To update or delete an entity, you must be able to identify it by using the **PartitionKey** and **RowKey** values.</span></span> <span data-ttu-id="4e549-334">V tomto ohledu vaši volbu **PartitionKey** a **RowKey** pro úpravy entity postupujte podobné kritéria na požadavek na podporu dotazů bod, protože chcete co možná nejefektivnější identifikovat entity.</span><span class="sxs-lookup"><span data-stu-id="4e549-334">In this respect, your choice of **PartitionKey** and **RowKey** for modifying entities should follow similar criteria to your choice to support point queries because you want to identify entities as efficiently as possible.</span></span> <span data-ttu-id="4e549-335">Nechcete pomocí neefektivní kontroly oddíl nebo tabulka, chcete-li vyhledat vyhledejte entitu **PartitionKey** a **RowKey** hodnoty, je potřeba aktualizovat nebo odstranit.</span><span class="sxs-lookup"><span data-stu-id="4e549-335">You do not want to use an inefficient partition or table scan to locate an entity in order to discover the **PartitionKey** and **RowKey** values you need to update or delete it.</span></span>  

<span data-ttu-id="4e549-336">Tyto vzory v části [vzory návrhu tabulky](#table-design-patterns) adres optimalizace výkonu nebo vaší insert, update a operace odstranění:</span><span class="sxs-lookup"><span data-stu-id="4e549-336">The following patterns in the section [Table Design Patterns](#table-design-patterns) address optimizing the performance or your insert, update, and delete operations:</span></span>  

* <span data-ttu-id="4e549-337">[Velkému odstranit vzor](#high-volume-delete-pattern) -Povolit odstranění k velkému počtu entity uložením všechny entity pro souběžné odstranění vlastní samostatné tabulky; odstranit entity odstraněním v tabulce.</span><span class="sxs-lookup"><span data-stu-id="4e549-337">[High volume delete pattern](#high-volume-delete-pattern) - Enable the deletion of a high volume of entities by storing all the entities for simultaneous deletion in their own separate table; you delete the entities by deleting the table.</span></span>  
* <span data-ttu-id="4e549-338">[Vzorek dat řady](#data-series-pattern) -úložiště dokončení datové řady v jedné entity, chcete-li minimalizovat počet požadavků, které provedete.</span><span class="sxs-lookup"><span data-stu-id="4e549-338">[Data series pattern](#data-series-pattern) - Store complete data series in a single entity to minimize the number of requests you make.</span></span>  
* <span data-ttu-id="4e549-339">[Vzor široké entity](#wide-entities-pattern) -používat více fyzických entit k uložení logických entit s více než 252 vlastností.</span><span class="sxs-lookup"><span data-stu-id="4e549-339">[Wide entities pattern](#wide-entities-pattern) - Use multiple physical entities to store logical entities with more than 252 properties.</span></span>  
* <span data-ttu-id="4e549-340">[Vzor velké entity](#large-entities-pattern) -použití úložiště objektů blob k ukládání velkých vlastnost hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4e549-340">[Large entities pattern](#large-entities-pattern) - Use blob storage to store large property values.</span></span>  

### <a name="ensuring-consistency-in-your-stored-entities"></a><span data-ttu-id="4e549-341">Zajištění konzistence v uložené entity</span><span class="sxs-lookup"><span data-stu-id="4e549-341">Ensuring consistency in your stored entities</span></span>
<span data-ttu-id="4e549-342">Další klíčovým faktorem, který ovlivňuje vaši volbu klíče pro optimalizaci změny dat je zajištění souladu s použitím jednotlivé transakce.</span><span class="sxs-lookup"><span data-stu-id="4e549-342">The other key factor that influences your choice of keys for optimizing data modifications is how to ensure consistency by using atomic transactions.</span></span> <span data-ttu-id="4e549-343">Můžete použít pouze EGT pracovat na entity, které jsou uložené ve stejném oddílu.</span><span class="sxs-lookup"><span data-stu-id="4e549-343">You can only use an EGT to operate on entities stored in the same partition.</span></span>  

<span data-ttu-id="4e549-344">Tyto vzory v části [vzory návrhu tabulky](#table-design-patterns) adresu Správa konzistence:</span><span class="sxs-lookup"><span data-stu-id="4e549-344">The following patterns in the section [Table Design Patterns](#table-design-patterns) address managing consistency:</span></span>  

* <span data-ttu-id="4e549-345">[Vzor sekundární index Intra-partition](#intra-partition-secondary-index-pattern) -uložit více kopií každou entitu s využitím různých **RowKey** hodnoty (ve stejném oddílu) k povolení rychlé a efektivní vyhledávání a alternativní pořadí řazení pomocí různých **RowKey** hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4e549-345">[Intra-partition secondary index pattern](#intra-partition-secondary-index-pattern) - Store multiple copies of each entity using different **RowKey** values (in the same partition) to enable fast and efficient lookups and alternate sort orders by using different **RowKey** values.</span></span>  
* <span data-ttu-id="4e549-346">[Vzor mezi oddíl sekundární index](#inter-partition-secondary-index-pattern) – ukládání více kopií každé entity pomocí různých hodnot RowKey v samostatné oddíly nebo v samostatných tabulkách umožňující rychlé a efektivní vyhledávání a řazení alternativní řadí pomocí různých **RowKey** hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4e549-346">[Inter-partition secondary index pattern](#inter-partition-secondary-index-pattern) - Store multiple copies of each entity using different RowKey values in separate partitions or in separate tables to enable fast and efficient lookups and alternate sort orders by using different **RowKey** values.</span></span>  
* <span data-ttu-id="4e549-347">[Nakonec byl konzistentní transakce vzor](#eventually-consistent-transactions-pattern) -povolit nakonec byl konzistentní chování v rámci hranice oddílů nebo hranice systému úložiště pomocí front Azure.</span><span class="sxs-lookup"><span data-stu-id="4e549-347">[Eventually consistent transactions pattern](#eventually-consistent-transactions-pattern) - Enable eventually consistent behavior across partition boundaries or storage system boundaries by using Azure queues.</span></span>
* <span data-ttu-id="4e549-348">[Index entity vzor](#index-entities-pattern) -udržovat index entity umožňující efektivní hledání, které vrátí seznamy entit.</span><span class="sxs-lookup"><span data-stu-id="4e549-348">[Index Entities Pattern](#index-entities-pattern) - Maintain index entities to enable efficient searches that return lists of entities.</span></span>  
* <span data-ttu-id="4e549-349">[Vzor denormalization](#denormalization-pattern) -zkombinujte související data společně v jedné entity, které vám umožňují načíst všechna data, musíte pomocí dotazu jediný bod.</span><span class="sxs-lookup"><span data-stu-id="4e549-349">[Denormalization pattern](#denormalization-pattern) - Combine related data together in a single entity to enable you to retrieve all the data you need with a single point query.</span></span>  
* <span data-ttu-id="4e549-350">[Vzorek dat řady](#data-series-pattern) -úložiště dokončení datové řady v jedné entity, chcete-li minimalizovat počet požadavků, které provedete.</span><span class="sxs-lookup"><span data-stu-id="4e549-350">[Data series pattern](#data-series-pattern) - Store complete data series in a single entity to minimize the number of requests you make.</span></span>  

<span data-ttu-id="4e549-351">Informace o transakcích skupiny entity, najdete v části [Entity skupiny transakce](#entity-group-transactions).</span><span class="sxs-lookup"><span data-stu-id="4e549-351">For information about entity group transactions, see the section [Entity Group Transactions](#entity-group-transactions).</span></span>  

### <a name="ensuring-your-design-for-efficient-modifications-facilitates-efficient-queries"></a><span data-ttu-id="4e549-352">Zajištění návrhu pro efektivní úpravy usnadňuje efektivní dotazy</span><span class="sxs-lookup"><span data-stu-id="4e549-352">Ensuring your design for efficient modifications facilitates efficient queries</span></span>
<span data-ttu-id="4e549-353">V mnoha případech by měl návrh pro efektivní dotazování výsledků v efektivní úpravy, ale vždy vyhodnocení, zda toto platí pro konkrétní scénář.</span><span class="sxs-lookup"><span data-stu-id="4e549-353">In many cases, a design for efficient querying results in efficient modifications, but you should always evaluate whether this is the case for your specific scenario.</span></span> <span data-ttu-id="4e549-354">Některé vzory v části [vzory návrhu tabulky](#table-design-patterns) explicitně vyhodnocení kompromis mezi dotazování a úprava entity a byste měli vždy vzít v úvahu počet jednotlivých typů operace.</span><span class="sxs-lookup"><span data-stu-id="4e549-354">Some of the patterns in the section [Table Design Patterns](#table-design-patterns) explicitly evaluate trade-offs between querying and modifying entities, and you should always take into account the number of each type of operation.</span></span>  

<span data-ttu-id="4e549-355">Tyto vzory v části [vzory návrhu tabulky](#table-design-patterns) adres kompromis mezi návrhu pro efektivní dotazy a návrhu pro úpravu efektivní dat:</span><span class="sxs-lookup"><span data-stu-id="4e549-355">The following patterns in the section [Table Design Patterns](#table-design-patterns) address trade-offs between designing for efficient queries and designing for efficient data modification:</span></span>  

* <span data-ttu-id="4e549-356">[Složené klíče vzor](#compound-key-pattern) -použití složené **RowKey** hodnoty, aby klient k vyhledání souvisejících dat pomocí dotazu jediný bod.</span><span class="sxs-lookup"><span data-stu-id="4e549-356">[Compound key pattern](#compound-key-pattern) - Use compound **RowKey** values to enable a client to lookup related data with a single point query.</span></span>  
* <span data-ttu-id="4e549-357">[Vzor protokolu poškozené databáze](#log-tail-pattern) -načíst  *n*  entity naposledy přidané do oddílu pomocí **RowKey** hodnotu, která seřadí zpětné datum a čas pořadí.</span><span class="sxs-lookup"><span data-stu-id="4e549-357">[Log tail pattern](#log-tail-pattern) - Retrieve the *n* entities most recently added to a partition by using a **RowKey** value that sorts in reverse date and time order.</span></span>  

## <a name="encrypting-table-data"></a><span data-ttu-id="4e549-358">Šifrování dat v tabulce</span><span class="sxs-lookup"><span data-stu-id="4e549-358">Encrypting Table Data</span></span>
<span data-ttu-id="4e549-359">Klientská knihovna pro úložiště Azure .NET podporuje šifrování vlastnosti entity řetězce pro vložení a nahrazovat operace.</span><span class="sxs-lookup"><span data-stu-id="4e549-359">The .NET Azure Storage Client Library supports encryption of string entity properties for insert and replace operations.</span></span> <span data-ttu-id="4e549-360">Šifrované řetězce jsou uložené ve službě jako binární vlastnosti a převedené zpět do řetězce po dešifrování.</span><span class="sxs-lookup"><span data-stu-id="4e549-360">The encrypted strings are stored on the service as binary properties, and they are converted back to strings after decryption.</span></span>    

<span data-ttu-id="4e549-361">Pro tabulky, kromě zásady šifrování musí uživatelé zadat vlastnosti k zašifrování.</span><span class="sxs-lookup"><span data-stu-id="4e549-361">For tables, in addition to the encryption policy, users must specify the properties to be encrypted.</span></span> <span data-ttu-id="4e549-362">To lze provést zadáním buď atribut [EncryptProperty] (pro entity objektů POCO, které jsou odvozeny od TableEntity) nebo šifrování překladač v žádosti o možnostech.</span><span class="sxs-lookup"><span data-stu-id="4e549-362">This can be done by either specifying an [EncryptProperty] attribute (for POCO entities that derive from TableEntity) or an encryption resolver in request options.</span></span> <span data-ttu-id="4e549-363">Překladač šifrování je delegáta, který přebírá klíč oddílu, klíč řádku a název vlastnosti a vrátí logickou hodnotu, která určuje, jestli by se šifrovat tuto vlastnost.</span><span class="sxs-lookup"><span data-stu-id="4e549-363">An encryption resolver is a delegate that takes a partition key, row key, and property name and returns a Boolean that indicates whether that property should be encrypted.</span></span> <span data-ttu-id="4e549-364">Během šifrování se klientské knihovny použije tyto informace se rozhodnout, jestli by se vlastnost šifrovat při zápisu do sítě.</span><span class="sxs-lookup"><span data-stu-id="4e549-364">During encryption, the client library will use this information to decide whether a property should be encrypted while writing to the wire.</span></span> <span data-ttu-id="4e549-365">Delegát taky poskytuje možnost logiku kolem jak jsou zašifrované vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="4e549-365">The delegate also provides for the possibility of logic around how properties are encrypted.</span></span> <span data-ttu-id="4e549-366">(Například pokud X, potom šifrování vlastnost A; v opačném případě šifrování vlastnosti A a B.) Všimněte si, že není nutné poskytnout tyto informace při čtení nebo dotazování entity.</span><span class="sxs-lookup"><span data-stu-id="4e549-366">(For example, if X, then encrypt property A; otherwise encrypt properties A and B.) Note that it is not necessary to provide this information while reading or querying entities.</span></span>

<span data-ttu-id="4e549-367">Všimněte si, že sloučení není aktuálně podporována.</span><span class="sxs-lookup"><span data-stu-id="4e549-367">Note that merge is not currently supported.</span></span> <span data-ttu-id="4e549-368">Vzhledem k tomu, že podmnožinu vlastností může být šifrována dříve pomocí jiného klíče, jednoduše slučování nové vlastnosti a aktualizace metadat dojde ke ztrátě dat.</span><span class="sxs-lookup"><span data-stu-id="4e549-368">Since a subset of properties may have been encrypted previously using a different key, simply merging the new properties and updating the metadata will result in data loss.</span></span> <span data-ttu-id="4e549-369">Slučování buď vyžaduje volání další služby ke čtení existující entity ze služby, nebo pomocí nového klíče na vlastnosti, které nejsou vhodné z důvodů výkonu.</span><span class="sxs-lookup"><span data-stu-id="4e549-369">Merging either requires making extra service calls to read the pre-existing entity from the service, or using a new key per property, both of which are not suitable for performance reasons.</span></span>     

<span data-ttu-id="4e549-370">Informace o šifrování dat v tabulce najdete v tématu [šifrování na straně klienta a Azure Key Vault pro Microsoft Azure Storage](../storage/common/storage-client-side-encryption.md).</span><span class="sxs-lookup"><span data-stu-id="4e549-370">For information about encrypting table data, see [Client-Side Encryption and Azure Key Vault for Microsoft Azure Storage](../storage/common/storage-client-side-encryption.md).</span></span>  

## <a name="modelling-relationships"></a><span data-ttu-id="4e549-371">Modelování vztahů</span><span class="sxs-lookup"><span data-stu-id="4e549-371">Modelling relationships</span></span>
<span data-ttu-id="4e549-372">Vytváření modelů domény je klíče krokem návrhu komplexní systémů.</span><span class="sxs-lookup"><span data-stu-id="4e549-372">Building domain models is a key step in the design of complex systems.</span></span> <span data-ttu-id="4e549-373">Proces modelování se obvykle používají pro identifikaci entity a vztahy mezi nimi jako způsob, jak pochopit obchodní domény a informujte návrh vašeho systému.</span><span class="sxs-lookup"><span data-stu-id="4e549-373">Typically, you use the modelling process to identify entities and the relationships between them as a way to understand the business domain and inform the design of your system.</span></span> <span data-ttu-id="4e549-374">Tato část se zaměřuje na tom, jak může překládat některé běžné typy vztah nacházející se v modelech domény pro návrhy pro služby Table.</span><span class="sxs-lookup"><span data-stu-id="4e549-374">This section focuses on how you can translate some of the common relationship types found in domain models to designs for the Table service.</span></span> <span data-ttu-id="4e549-375">Proces mapování z logický datový model na fyzické dat typu NoSQL na základě-model je příliš neliší od kterého při navrhování relační databáze.</span><span class="sxs-lookup"><span data-stu-id="4e549-375">The process of mapping from a logical data-model to a physical NoSQL based data-model is very different from that used when designing a relational database.</span></span> <span data-ttu-id="4e549-376">Návrh relační databáze obvykle předpokládá proces normalizace dat optimalizované pro minimalizaci redundance – a deklarativní dotazování funkci, která abstrahuje implementace jak databázi fungování.</span><span class="sxs-lookup"><span data-stu-id="4e549-376">Relational databases design typically assumes a data normalization process optimized for minimizing redundancy – and a declarative querying capability that abstracts how the implementation of how the database works.</span></span>  

### <a name="one-to-many-relationships"></a><span data-ttu-id="4e549-377">Vztahy jeden mnoho</span><span class="sxs-lookup"><span data-stu-id="4e549-377">One-to-many relationships</span></span>
<span data-ttu-id="4e549-378">Na více vztahů mezi objekty domény obchodní dojde k velmi často: například jeden oddělení má mnoho pracovníků.</span><span class="sxs-lookup"><span data-stu-id="4e549-378">One-to-many relationships between business domain objects occur very frequently: for example, one department has many employees.</span></span> <span data-ttu-id="4e549-379">Existuje několik způsobů, jak implementovat na více vztahů ve službě Table každý s výhody a nevýhody, které mohou být relevantní pro konkrétní scénář.</span><span class="sxs-lookup"><span data-stu-id="4e549-379">There are several ways to implement one-to-many relationships in the Table service each with pros and cons that may be relevant to the particular scenario.</span></span>  

<span data-ttu-id="4e549-380">Podívejte se na příklad velké korporace více national s desítkami tisíc oddělení a zaměstnanci entity, kde každé oddělení má mnoho zaměstnance a zaměstnance, jak je přidružený jeden konkrétní oddělení.</span><span class="sxs-lookup"><span data-stu-id="4e549-380">Consider the example of a large multi-national corporation with tens of thousands of departments and employee entities where every department has many employees and each employee as associated with one specific department.</span></span> <span data-ttu-id="4e549-381">Jeden z přístupů je k uložení samostatné oddělení a zaměstnanci entitami, jako je například tyto:</span><span class="sxs-lookup"><span data-stu-id="4e549-381">One approach is to store separate department and employee entities such as these:</span></span>  

![][1]

<span data-ttu-id="4e549-382">Tento příklad ukazuje implicitní vztah jeden mnoho mezi typy na základě **PartitionKey** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4e549-382">This example shows an implicit one-to-many relationship between the types based on the **PartitionKey** value.</span></span> <span data-ttu-id="4e549-383">Každé oddělení může mít mnoho pracovníků.</span><span class="sxs-lookup"><span data-stu-id="4e549-383">Each department can have many employees.</span></span>  

<span data-ttu-id="4e549-384">Tento příklad také uvádí entity oddělení a jeho souvisejících zaměstnanec entity ve stejném oddílu.</span><span class="sxs-lookup"><span data-stu-id="4e549-384">This example also shows a department entity and its related employee entities in the same partition.</span></span> <span data-ttu-id="4e549-385">Mohli byste používat různé oddíly, tabulky nebo účty úložiště i pro typy různých entit.</span><span class="sxs-lookup"><span data-stu-id="4e549-385">You could choose to use different partitions, tables, or even storage accounts for the different entity types.</span></span>  

<span data-ttu-id="4e549-386">Alternativní způsob je denormalize vaše data a uložit jenom zaměstnanec entity daty nenormalizované oddělení, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="4e549-386">An alternative approach is to denormalize your data and store only employee entities with denormalized department data as shown in the following example.</span></span> <span data-ttu-id="4e549-387">V tomto scénáři konkrétní tento nenormalizované přístup nemusí být nejvhodnější, pokud máte požadavek, abyste mohli změnit podrobnosti o oddělení manager, protože k tomu je potřeba aktualizovat všechny zaměstnance v oddělení.</span><span class="sxs-lookup"><span data-stu-id="4e549-387">In this particular scenario, this denormalized approach may not be the best if you have a requirement to be able to change the details of a department manager because to do this you need to update every employee in the department.</span></span>  

![][2]

<span data-ttu-id="4e549-388">Další informace najdete v tématu [Denormalization vzor](#denormalization-pattern) dál v této příručce.</span><span class="sxs-lookup"><span data-stu-id="4e549-388">For more information, see the [Denormalization pattern](#denormalization-pattern) later in this guide.</span></span>  

<span data-ttu-id="4e549-389">Následující tabulka shrnuje výhody a nevýhody jednotlivých přístupů uvedených výše pro ukládání zaměstnanců a oddělení entity, které mají vztah jeden mnoho.</span><span class="sxs-lookup"><span data-stu-id="4e549-389">The following table summarizes the pros and cons of each of the approaches outlined above for storing employee and department entities that have a one-to-many relationship.</span></span> <span data-ttu-id="4e549-390">Také byste měli zvážit, jak často předpokládáte provádět různé operace: může být přijatelné tak, aby měl návrh, který zahrnuje náročná operace, pokud tuto operaci pouze dochází zřídka.</span><span class="sxs-lookup"><span data-stu-id="4e549-390">You should also consider how often you expect to perform various operations: it may be acceptable to have a design that includes an expensive operation if that operation only happens infrequently.</span></span>  

<table>
<tr>
<th><span data-ttu-id="4e549-391">Přístup</span><span class="sxs-lookup"><span data-stu-id="4e549-391">Approach</span></span></th>
<th><span data-ttu-id="4e549-392">Odborníci na</span><span class="sxs-lookup"><span data-stu-id="4e549-392">Pros</span></span></th>
<th><span data-ttu-id="4e549-393">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="4e549-393">Cons</span></span></th>
</tr>
<tr>
<td><span data-ttu-id="4e549-394">Jednotlivé typy entit, stejného oddílu, stejné tabulky</span><span class="sxs-lookup"><span data-stu-id="4e549-394">Separate entity types, same partition, same table</span></span></td>
<td>
<ul>
<li><span data-ttu-id="4e549-395">Entity oddělení můžete aktualizovat pomocí jedné operace.</span><span class="sxs-lookup"><span data-stu-id="4e549-395">You can update a department entity with a single operation.</span></span></li>
<li><span data-ttu-id="4e549-396">Můžete použít EGT zachování konzistence, pokud máte požadavky k úpravě entity oddělení vždy, když jste aktualizace, insert nebo odstranění entity zaměstnanců.</span><span class="sxs-lookup"><span data-stu-id="4e549-396">You can use an EGT to maintain consistency if you have a requirement to modify a department entity whenever you update/insert/delete an employee entity.</span></span> <span data-ttu-id="4e549-397">Například pokud chcete zachovat počet zaměstnanců oddělení pro každé oddělení.</span><span class="sxs-lookup"><span data-stu-id="4e549-397">For example if you maintain a departmental employee count for each department.</span></span></li>
</ul>
</td>
<td>
<ul>
<li><span data-ttu-id="4e549-398">Musíte získat zaměstnanec a oddělení entity pro některé činnosti klienta.</span><span class="sxs-lookup"><span data-stu-id="4e549-398">You may need to retrieve both an employee and a department entity for some client activities.</span></span></li>
<li><span data-ttu-id="4e549-399">Operace úložiště dojít ve stejném oddílu.</span><span class="sxs-lookup"><span data-stu-id="4e549-399">Storage operations happen in the same partition.</span></span> <span data-ttu-id="4e549-400">Na vysoké transakce svazky proto může docházet v aktivní oblast.</span><span class="sxs-lookup"><span data-stu-id="4e549-400">At high transaction volumes, this may result in a hotspot.</span></span></li>
<li><span data-ttu-id="4e549-401">Zaměstnanec nelze přesunout do nového oddělení pomocí EGT.</span><span class="sxs-lookup"><span data-stu-id="4e549-401">You cannot move an employee to a new department using an EGT.</span></span></li>
</ul>
</td>
</tr>
<tr>
<td><span data-ttu-id="4e549-402">Typy samostatné entity, různých oddílů nebo tabulek účty úložiště</span><span class="sxs-lookup"><span data-stu-id="4e549-402">Separate entity types, different partitions or tables or storage accounts</span></span></td>
<td>
<ul>
<li><span data-ttu-id="4e549-403">Oddělení entity nebo zaměstnanec entity můžete aktualizovat pomocí jedné operace.</span><span class="sxs-lookup"><span data-stu-id="4e549-403">You can update a department entity or employee entity with a single operation.</span></span></li>
<li><span data-ttu-id="4e549-404">Na vysoké transakce svazky to pomůže rozkládá zatížení mezi více oddílů.</span><span class="sxs-lookup"><span data-stu-id="4e549-404">At high transaction volumes, this may help spread the load across more partitions.</span></span></li>
</ul>
</td>
<td>
<ul>
<li><span data-ttu-id="4e549-405">Musíte získat zaměstnanec a oddělení entity pro některé činnosti klienta.</span><span class="sxs-lookup"><span data-stu-id="4e549-405">You may need to retrieve both an employee and a department entity for some client activities.</span></span></li>
<li><span data-ttu-id="4e549-406">EGTs nelze použít k zajištění konzistence když jste update, insert nebo odstranění zaměstnanec a aktualizace oddělení.</span><span class="sxs-lookup"><span data-stu-id="4e549-406">You cannot use EGTs to maintain consistency when you update/insert/delete an employee and update a department.</span></span> <span data-ttu-id="4e549-407">Aktualizuje se například počet zaměstnanců v oddělení entity.</span><span class="sxs-lookup"><span data-stu-id="4e549-407">For example, updating an employee count in a department entity.</span></span></li>
<li><span data-ttu-id="4e549-408">Zaměstnanec nelze přesunout do nového oddělení pomocí EGT.</span><span class="sxs-lookup"><span data-stu-id="4e549-408">You cannot move an employee to a new department using an EGT.</span></span></li>
</ul>
</td>
</tr>
<tr>
<td><span data-ttu-id="4e549-409">Denormalize do jedné entity typu</span><span class="sxs-lookup"><span data-stu-id="4e549-409">Denormalize into single entity type</span></span></td>
<td>
<ul>
<li><span data-ttu-id="4e549-410">Můžete načíst všechny informace, které je nutné se jeden požadavek.</span><span class="sxs-lookup"><span data-stu-id="4e549-410">You can retrieve all the information you need with a single request.</span></span></li>
</ul>
</td>
<td>
<ul>
<li><span data-ttu-id="4e549-411">To může být nákladné zachování konzistence, pokud je potřeba aktualizovat informace o oddělení (to se vyžaduje k aktualizaci všechny zaměstnance v oddělení).</span><span class="sxs-lookup"><span data-stu-id="4e549-411">It may be expensive to maintain consistency if you need to update department information (this would require you to update all the employees in a department).</span></span></li>
</ul>
</td>
</tr>
</table>

<span data-ttu-id="4e549-412">Jak můžete vybrat mezi tyto možnosti a které výhody a nevýhody jsou nejdůležitější, závisí na konkrétní aplikaci scénářů.</span><span class="sxs-lookup"><span data-stu-id="4e549-412">How you choose between these options, and which of the pros and cons are most significant, depends on your specific application scenarios.</span></span> <span data-ttu-id="4e549-413">Například jak často upravíte entities oddělení; mají všechny dotazy zaměstnanec další oddělení informace; jak zavřít jste k omezení škálovatelnosti na oddíly nebo účtu úložiště?</span><span class="sxs-lookup"><span data-stu-id="4e549-413">For example, how often do you modify department entities; do all your employee queries need the additional departmental information; how close are you to the scalability limits on your partitions or your storage account?</span></span>  

### <a name="one-to-one-relationships"></a><span data-ttu-id="4e549-414">Relace 1: 1</span><span class="sxs-lookup"><span data-stu-id="4e549-414">One-to-one relationships</span></span>
<span data-ttu-id="4e549-415">Modely domény může zahrnovat relace 1: 1 mezi entitami.</span><span class="sxs-lookup"><span data-stu-id="4e549-415">Domain models may include one-to-one relationships between entities.</span></span> <span data-ttu-id="4e549-416">Pokud potřebujete implementovat relace ve službě Table, musíte také zvolit způsob propojení dvou entit v relaci, pokud budete potřebovat načíst z obou.</span><span class="sxs-lookup"><span data-stu-id="4e549-416">If you need to implement a one-to-one relationship in the Table service, you must also choose how to link the two related entities when you need to retrieve them both.</span></span> <span data-ttu-id="4e549-417">Tento odkaz může být buď implicitní, na základě konvencí v hodnoty klíče nebo explicitní uložením odkaz ve formě **PartitionKey** a **RowKey** hodnoty v každé entity k jeho související entity.</span><span class="sxs-lookup"><span data-stu-id="4e549-417">This link can be either implicit, based on a convention in the key values, or explicit by storing a link in the form of **PartitionKey** and **RowKey** values in each entity to its related entity.</span></span> <span data-ttu-id="4e549-418">Informace o tom, zda by měl uložit entit v relaci v stejného oddílu, najdete v části [na více vztahů](#one-to-many-relationships).</span><span class="sxs-lookup"><span data-stu-id="4e549-418">For a discussion of whether you should store the related entities in the same partition, see the section [One-to-many relationships](#one-to-many-relationships).</span></span>  

<span data-ttu-id="4e549-419">Všimněte si, že existují také aspekty implementace, které mohou vést k implementaci relace 1: 1 ve službě Table:</span><span class="sxs-lookup"><span data-stu-id="4e549-419">Note that there are also implementation considerations that might lead you to implement one-to-one relationships in the Table service:</span></span>  

* <span data-ttu-id="4e549-420">Zpracování velkých entit (Další informace najdete v tématu [velké vzor entity](#large-entities-pattern)).</span><span class="sxs-lookup"><span data-stu-id="4e549-420">Handling large entities (for more information, see [Large Entities Pattern](#large-entities-pattern)).</span></span>  
* <span data-ttu-id="4e549-421">Implementace řízení přístupu (Další informace najdete v tématu [řízení přístupu s podpisy sdíleného přístupu](#controlling-access-with-shared-access-signatures)).</span><span class="sxs-lookup"><span data-stu-id="4e549-421">Implementing access controls (for more information, see [Controlling access with Shared Access Signatures](#controlling-access-with-shared-access-signatures)).</span></span>  

### <a name="join-in-the-client"></a><span data-ttu-id="4e549-422">Připojení v klientovi</span><span class="sxs-lookup"><span data-stu-id="4e549-422">Join in the client</span></span>
<span data-ttu-id="4e549-423">I když existují způsoby, jak model vztahy ve službě Table, by neměl zapomenete, zda jsou dva prvotní důvody pro používání služby Table škálovatelnost a výkon.</span><span class="sxs-lookup"><span data-stu-id="4e549-423">Although there are ways to model relationships in the Table service, you should not forget that the two prime reasons for using the Table service are scalability and performance.</span></span> <span data-ttu-id="4e549-424">Pokud zjistíte, že jsou modelování více vztahů, které ohrožují výkon a škálovatelnost řešení, měli byste požádat sami Pokud je nutné vytvořit všechny vztahy data do návrhu tabulky.</span><span class="sxs-lookup"><span data-stu-id="4e549-424">If you find you are modelling many relationships that compromise the performance and scalability of your solution, you should ask yourself if it is necessary to build all the data relationships into your table design.</span></span> <span data-ttu-id="4e549-425">Bude pravděpodobně možné zjednodušit návrh a zlepšit škálovatelnost a výkon vašeho řešení, pokud jste povolili klientské aplikace, proveďte všechny nezbytné spojení.</span><span class="sxs-lookup"><span data-stu-id="4e549-425">You may be able to simplify the design and improve the scalability and performance of your solution if you let your client application perform any necessary joins.</span></span>  

<span data-ttu-id="4e549-426">Například pokud máte malé tabulky, které obsahují data, která se nemění příliš často, pak můžete tato data načíst jednou a uložení do mezipaměti na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="4e549-426">For example, if you have small tables that contain data that does not change very often, then you can retrieve this data once and cache it on the client.</span></span> <span data-ttu-id="4e549-427">To se můžete vyhnout opakovaných zbytečné komunikace načíst stejná data.</span><span class="sxs-lookup"><span data-stu-id="4e549-427">This can avoid repeated roundtrips to retrieve the same data.</span></span> <span data-ttu-id="4e549-428">V příkladech, které jsme jste prohlédli v této příručce bude pravděpodobně být malý a změňte zřídka díky tomu vhodným kandidátem na data, která klientská aplikace můžete stáhnout jednou a mezipaměti jako vyhledávání dat sadu oddělení v organizaci, malé.</span><span class="sxs-lookup"><span data-stu-id="4e549-428">In the examples we have looked at in this guide, the set of departments in a small organization is likely to be small and change infrequently making it a good candidate for data that client application can download once and cache as look up data.</span></span>  

### <a name="inheritance-relationships"></a><span data-ttu-id="4e549-429">Vztahy dědičnosti</span><span class="sxs-lookup"><span data-stu-id="4e549-429">Inheritance relationships</span></span>
<span data-ttu-id="4e549-430">Pokud klientské aplikace používá sadu tříd, které tvoří součást vztah dědičnosti představují obchodní entity, můžete snadno zachovat tyto entity ve službě Table.</span><span class="sxs-lookup"><span data-stu-id="4e549-430">If your client application uses a set of classes that form part of an inheritance relationship to represent business entities, you can easily persist those entities in the Table service.</span></span> <span data-ttu-id="4e549-431">Například můžete mít následující sadu tříd definovaných v aplikaci klienta kde **osoba** je abstraktní třída.</span><span class="sxs-lookup"><span data-stu-id="4e549-431">For example, you might have the following set of classes defined in your client application where **Person** is an abstract class.</span></span>

![][3]

<span data-ttu-id="4e549-432">Můžete zachovat instancí dvě konkrétní třídy ve službě Table pomocí jedné tabulky osoba pomocí entity v tomto vypadají:</span><span class="sxs-lookup"><span data-stu-id="4e549-432">You can persist instances of the two concrete classes in the Table service using a single Person table using entities in that look like this:</span></span>  

![][4]

<span data-ttu-id="4e549-433">Další informace o práci s více typy entit ve stejné tabulce v kódu klienta najdete v části [práce s typy entit heterogenní](#working-with-heterogeneous-entity-types) dál v této příručce.</span><span class="sxs-lookup"><span data-stu-id="4e549-433">For more information about working with multiple entity types in the same table in client code, see the section [Working with heterogeneous entity types](#working-with-heterogeneous-entity-types) later in this guide.</span></span> <span data-ttu-id="4e549-434">To poskytuje příklady, jak rozpoznat typ entity v kódu klienta.</span><span class="sxs-lookup"><span data-stu-id="4e549-434">This provides examples of how to recognize the entity type in client code.</span></span>  

## <a name="table-design-patterns"></a><span data-ttu-id="4e549-435">Vzory návrhu tabulky</span><span class="sxs-lookup"><span data-stu-id="4e549-435">Table Design Patterns</span></span>
<span data-ttu-id="4e549-436">V předchozích částech jste viděli, že některé podrobné diskuse o tom, jak optimalizovat váš návrh tabulky pro obě načítání dat entity pomocí dotazů a vkládání, aktualizace a odstranění dat entity.</span><span class="sxs-lookup"><span data-stu-id="4e549-436">In previous sections, you have seen some detailed discussions about how to optimize your table design for both retrieving entity data using queries and for inserting, updating, and deleting entity data.</span></span> <span data-ttu-id="4e549-437">Tato část popisuje některé vzory vhodné používat s řešeními služby Table.</span><span class="sxs-lookup"><span data-stu-id="4e549-437">This section describes some patterns appropriate for use with Table service solutions.</span></span> <span data-ttu-id="4e549-438">Kromě toho uvidíte, jak vám může prakticky adres některé z problémů a kompromis vyvolá dříve v této příručce.</span><span class="sxs-lookup"><span data-stu-id="4e549-438">In addition, you will see how you can practically address some of the issues and trade-offs raised previously in this guide.</span></span> <span data-ttu-id="4e549-439">Následující diagram shrnuje vztahy mezi různé vzorce:</span><span class="sxs-lookup"><span data-stu-id="4e549-439">The following diagram summarizes the relationships between the different patterns:</span></span>  

![][5]

<span data-ttu-id="4e549-440">Vzor mapy výše označuje některé vztahy mezi vzory (modré) a proti vzory (oranžová), které jsou popsané v této příručce.</span><span class="sxs-lookup"><span data-stu-id="4e549-440">The pattern map above highlights some relationships between patterns (blue) and anti-patterns (orange) that are documented in this guide.</span></span> <span data-ttu-id="4e549-441">Mnoho dalších vzorech, které jsou vhodné vzhledem k tomu, samozřejmě neexistují.</span><span class="sxs-lookup"><span data-stu-id="4e549-441">There are of course many other patterns that are worth considering.</span></span> <span data-ttu-id="4e549-442">Například jeden z klíčových scénářů pro služby Table je použití [Materializována vzor zobrazení](https://msdn.microsoft.com/library/azure/dn589782.aspx) z [příkaz dotazu odpovědnost oddělení (CQRS)](https://msdn.microsoft.com/library/azure/jj554200.aspx) vzor.</span><span class="sxs-lookup"><span data-stu-id="4e549-442">For example, one of the key scenarios for Table Service is to use the [Materialized View Pattern](https://msdn.microsoft.com/library/azure/dn589782.aspx) from the [Command Query Responsibility Segregation (CQRS)](https://msdn.microsoft.com/library/azure/jj554200.aspx) pattern.</span></span>  

### <a name="intra-partition-secondary-index-pattern"></a><span data-ttu-id="4e549-443">Vzor Intra-partition sekundární index</span><span class="sxs-lookup"><span data-stu-id="4e549-443">Intra-partition secondary index pattern</span></span>
<span data-ttu-id="4e549-444">Uložit více kopií každou entitu s využitím různých **RowKey** hodnoty (ve stejném oddílu) k povolení rychlé a efektivní vyhledávání a alternativní pořadí řazení pomocí různých **RowKey** hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4e549-444">Store multiple copies of each entity using different **RowKey** values (in the same partition) to enable fast and efficient lookups and alternate sort orders by using different **RowKey** values.</span></span> <span data-ttu-id="4e549-445">Aktualizace mezi kopie je možné mít konzistentní pomocí EGT společnosti.</span><span class="sxs-lookup"><span data-stu-id="4e549-445">Updates between copies can be kept consistent using EGT's.</span></span>  

#### <a name="context-and-problem"></a><span data-ttu-id="4e549-446">Kontext a problém</span><span class="sxs-lookup"><span data-stu-id="4e549-446">Context and problem</span></span>
<span data-ttu-id="4e549-447">Služba Table automaticky indexuje entity pomocí **PartitionKey** a **RowKey** hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4e549-447">The Table service automatically indexes entities using the **PartitionKey** and **RowKey** values.</span></span> <span data-ttu-id="4e549-448">To umožňuje klientskou aplikaci k načtení entity efektivně pomocí těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="4e549-448">This enables a client application to retrieve an entity efficiently using these values.</span></span> <span data-ttu-id="4e549-449">Například struktura tabulky vidíte níže, klientská aplikace pomocí dotazu bod k načtení entity jednotlivých zaměstnanců pomocí názvu oddělení a id zaměstnance ( **PartitionKey** a **RowKey** hodnoty).</span><span class="sxs-lookup"><span data-stu-id="4e549-449">For example, using the table structure shown below, a client application can use a point query to retrieve an individual employee entity by using the department name and the employee id (the **PartitionKey** and **RowKey** values).</span></span> <span data-ttu-id="4e549-450">Klienta můžete také načíst entity seřazené podle id zaměstnance v rámci každé oddělení.</span><span class="sxs-lookup"><span data-stu-id="4e549-450">A client can also retrieve entities sorted by employee id within each department.</span></span>

![][6]

<span data-ttu-id="4e549-451">Pokud chcete být schopni najít entitu zaměstnanců na základě hodnoty, jiné vlastnosti, jako je například e-mailovou adresu, musíte použít méně efektivní prohledávání oddílu k vyhledání shody.</span><span class="sxs-lookup"><span data-stu-id="4e549-451">If you also want to be able to find an employee entity based on the value of another property, such as email address, you must use a less efficient partition scan to find a match.</span></span> <span data-ttu-id="4e549-452">Je to proto, že služby table neposkytuje sekundární indexy.</span><span class="sxs-lookup"><span data-stu-id="4e549-452">This is because the table service does not provide secondary indexes.</span></span> <span data-ttu-id="4e549-453">Kromě toho není žádná možnost odeslat žádost o seznam zaměstnanců seřazený v jiném pořadí než **RowKey** pořadí.</span><span class="sxs-lookup"><span data-stu-id="4e549-453">In addition, there is no option to request a list of employees sorted in a different order than **RowKey** order.</span></span>  

#### <a name="solution"></a><span data-ttu-id="4e549-454">Řešení</span><span class="sxs-lookup"><span data-stu-id="4e549-454">Solution</span></span>
<span data-ttu-id="4e549-455">Nedostatečná sekundární indexy obejít, můžete uložit více kopií každé entity s každou kopii pomocí jiné **RowKey** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4e549-455">To work around the lack of secondary indexes, you can store multiple copies of each entity with each copy using a different **RowKey** value.</span></span> <span data-ttu-id="4e549-456">Pokud ukládáte entitu s struktury vidíte níže, můžete efektivně načíst zaměstnanec entit na základě id e-mailovou adresu nebo zaměstnanců. Předpona hodnoty **RowKey**, "empid_" a "email_" umožňují dotazu pro jeden zaměstnanců nebo rozsah zaměstnanci pomocí řadu e-mailové adresy nebo ID zaměstnance.</span><span class="sxs-lookup"><span data-stu-id="4e549-456">If you store an entity with the structures shown below, you can efficiently retrieve employee entities based on email address or employee id. The prefix values for the **RowKey**, "empid_" and "email_" enable you to query for a single employee or a range of employees by using a range of email addresses or employee ids.</span></span>  

![][7]

<span data-ttu-id="4e549-457">Následující dvě kritéria filtru, která (jeden vyhledávání podle id zaměstnance a jeden vyhledávání pomocí e-mailové adresy) i zadejte bod dotazy:</span><span class="sxs-lookup"><span data-stu-id="4e549-457">The following two filter criteria (one looking up by employee id and one looking up by email address) both specify point queries:</span></span>  

* <span data-ttu-id="4e549-458">$filter = (PartitionKey eq 'Prodej') a (RowKey eq 'empid_000223')</span><span class="sxs-lookup"><span data-stu-id="4e549-458">$filter=(PartitionKey eq 'Sales') and (RowKey eq 'empid_000223')</span></span>  
* <span data-ttu-id="4e549-459">$filter = (PartitionKey eq 'Prodej') a (RowKey eq 'email_jonesj@contoso.com')</span><span class="sxs-lookup"><span data-stu-id="4e549-459">$filter=(PartitionKey eq 'Sales') and (RowKey eq 'email_jonesj@contoso.com')</span></span>  

<span data-ttu-id="4e549-460">Pokud vyhledat rozsahu entit zaměstnanců, můžete zadat rozsah id řazení zaměstnanců nebo rozsahu seřazeny v e-mailovou adresu pořadí pomocí dotazu pro entity v příslušnou předponu **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="4e549-460">If you query for a range of employee entities, you can specify a range sorted in employee id order, or a range sorted in email address order by querying for entities with the appropriate prefix in the **RowKey**.</span></span>  

* <span data-ttu-id="4e549-461">Najít všechny zaměstnance z oddělení prodeje s id zaměstnance v rozsahu 000100 k 000199 použití: $filter = (PartitionKey eq 'Prodej') a (RowKey ge 'empid_000100') a (RowKey le 'empid_000199')</span><span class="sxs-lookup"><span data-stu-id="4e549-461">To find all the employees in the Sales department with an employee id in the range 000100 to 000199 use: $filter=(PartitionKey eq 'Sales') and (RowKey ge 'empid_000100') and (RowKey le 'empid_000199')</span></span>  
* <span data-ttu-id="4e549-462">Najít všechny zaměstnance z oddělení prodeje s e-mailovou adresu, začínající písmenem "a" použití: $filter = (PartitionKey eq 'Prodej') a (RowKey ge 'email_a') a (RowKey lt 'email_b')</span><span class="sxs-lookup"><span data-stu-id="4e549-462">To find all the employees in the Sales department with an email address starting with the letter 'a' use: $filter=(PartitionKey eq 'Sales') and (RowKey ge 'email_a') and (RowKey lt 'email_b')</span></span>  
  
  <span data-ttu-id="4e549-463">Všimněte si, že se používá ve výše uvedených příkladech syntaxe filtru je ze služby Table rozhraní REST API, další informace najdete v tématu [dotazu entity](http://msdn.microsoft.com/library/azure/dd179421.aspx).</span><span class="sxs-lookup"><span data-stu-id="4e549-463">Note that the filter syntax used in the examples above is from the Table service REST API, for more information see [Query Entities](http://msdn.microsoft.com/library/azure/dd179421.aspx).</span></span>  

#### <a name="issues-and-considerations"></a><span data-ttu-id="4e549-464">Problémy a důležité informace</span><span class="sxs-lookup"><span data-stu-id="4e549-464">Issues and considerations</span></span>
<span data-ttu-id="4e549-465">Při rozhodování o tom, jak implementovat tento vzor, zvažte následující body:</span><span class="sxs-lookup"><span data-stu-id="4e549-465">Consider the following points when deciding how to implement this pattern:</span></span>  

* <span data-ttu-id="4e549-466">Table storage je relativně levný abyste nároky na náklady na ukládání duplicitních dat nesmí být závažný problém.</span><span class="sxs-lookup"><span data-stu-id="4e549-466">Table storage is relatively cheap to use so the cost overhead of storing duplicate data should not be a major concern.</span></span> <span data-ttu-id="4e549-467">Však měli vždy vyhodnoceny náklady návrhu na základě požadavků vaší předpokládaného úložiště a přidat pouze duplicitní entity, které podporují dotazy, které budou spuštěny klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="4e549-467">However, you should always evaluate the cost of your design based on your anticipated storage requirements and only add duplicate entities to support the queries your client application will execute.</span></span>  
* <span data-ttu-id="4e549-468">Proto sekundární index entity, které ukládají do stejného oddílu jako původní entity, musíte ověřit nepřekračují cíle škálovatelnosti pro jednotlivé oddíl.</span><span class="sxs-lookup"><span data-stu-id="4e549-468">Because the secondary index entities are stored in the same partition as the original entities, you should ensure that you do not exceed the scalability targets for an individual partition.</span></span>  
* <span data-ttu-id="4e549-469">Vzájemnou konzistenci duplicitní položky můžete ponechat pomocí EGTs dvě kopie entity, která atomicky aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="4e549-469">You can keep your duplicate entities consistent with each other by using EGTs to update the two copies of the entity atomically.</span></span> <span data-ttu-id="4e549-470">Z toho vyplývá, že měli uložit všechny kopie entity v stejného oddílu.</span><span class="sxs-lookup"><span data-stu-id="4e549-470">This implies that you should store all copies of an entity in the same partition.</span></span> <span data-ttu-id="4e549-471">Další informace najdete v části [pomocí transakce skupiny Entity](#entity-group-transactions).</span><span class="sxs-lookup"><span data-stu-id="4e549-471">For more information, see the section [Using Entity Group Transactions](#entity-group-transactions).</span></span>  
* <span data-ttu-id="4e549-472">Hodnota použitá **RowKey** musí být jedinečný pro každé entity.</span><span class="sxs-lookup"><span data-stu-id="4e549-472">The value used for the **RowKey** must be unique for each entity.</span></span> <span data-ttu-id="4e549-473">Zvažte použití hodnoty složeného klíče.</span><span class="sxs-lookup"><span data-stu-id="4e549-473">Consider using compound key values.</span></span>  
* <span data-ttu-id="4e549-474">Odsazení číselných hodnot v **RowKey** (například id zaměstnance 000223), umožňuje opravte řazení a filtrování na základě horní a dolní meze.</span><span class="sxs-lookup"><span data-stu-id="4e549-474">Padding numeric values in the **RowKey** (for example, the employee id 000223), enables correct sorting and filtering based on upper and lower bounds.</span></span>  
* <span data-ttu-id="4e549-475">Nutně není potřeba duplicitní vlastnosti vaší entity.</span><span class="sxs-lookup"><span data-stu-id="4e549-475">You do not necessarily need to duplicate all the properties of your entity.</span></span> <span data-ttu-id="4e549-476">Například pokud dotazy tento vyhledávací entity pomocí e-mailu adres v **RowKey** nikdy nepotřebují stáří zaměstnance, tyto entit může mít následující strukturu:</span><span class="sxs-lookup"><span data-stu-id="4e549-476">For example, if the queries that lookup the entities using the email address in the **RowKey** never need the employee's age, these entities could have the following structure:</span></span>

![][8]

* <span data-ttu-id="4e549-477">Je obvykle lepší uložit duplicitní data a ujistěte se, že můžete načíst všechna data, je nutné se jeden dotaz, než chcete použijte jeden dotaz a vyhledejte entitu a druhý k vyhledání požadovaná data.</span><span class="sxs-lookup"><span data-stu-id="4e549-477">It is typically better to store duplicate data and ensure that you can retrieve all the data you need with a single query, than to use one query to locate an entity and another to lookup the required data.</span></span>  

#### <a name="when-to-use-this-pattern"></a><span data-ttu-id="4e549-478">Kdy použít tento vzor</span><span class="sxs-lookup"><span data-stu-id="4e549-478">When to use this pattern</span></span>
<span data-ttu-id="4e549-479">Tento vzor použijte, když klientské aplikace potřebuje k načtení entity pomocí různých odlišné klíče, když váš klient potřebuje k načtení entity v jiné pořadí řazení, a tam, kde můžete identifikovat každé entity pomocí různých jedinečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4e549-479">Use this pattern when your client application needs to retrieve entities using a variety of different keys, when your client needs to retrieve entities in different sort orders, and where you can identify each entity using a variety of unique values.</span></span> <span data-ttu-id="4e549-480">Nicméně byste měli jistotu, že nedošlo k překročení omezení škálovatelnosti oddíl při provádění entity vyhledávání pomocí různými **RowKey** hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4e549-480">However, you should be sure that you do not exceed the partition scalability limits when you are performing entity lookups using the different **RowKey** values.</span></span>  

#### <a name="related-patterns-and-guidance"></a><span data-ttu-id="4e549-481">Související vzory a pokyny</span><span class="sxs-lookup"><span data-stu-id="4e549-481">Related patterns and guidance</span></span>
<span data-ttu-id="4e549-482">Následující pokyny a vzory může být také relevantní při implementaci tohoto vzoru:</span><span class="sxs-lookup"><span data-stu-id="4e549-482">The following patterns and guidance may also be relevant when implementing this pattern:</span></span>  

* [<span data-ttu-id="4e549-483">Vzor sekundární index mezi oddílů</span><span class="sxs-lookup"><span data-stu-id="4e549-483">Inter-partition secondary index pattern</span></span>](#inter-partition-secondary-index-pattern)
* [<span data-ttu-id="4e549-484">Složené klíče vzor</span><span class="sxs-lookup"><span data-stu-id="4e549-484">Compound key pattern</span></span>](#compound-key-pattern)
* [<span data-ttu-id="4e549-485">Transakce skupiny entity</span><span class="sxs-lookup"><span data-stu-id="4e549-485">Entity Group Transactions</span></span>](#entity-group-transactions)
* [<span data-ttu-id="4e549-486">Práce s typy heterogenní entit</span><span class="sxs-lookup"><span data-stu-id="4e549-486">Working with heterogeneous entity types</span></span>](#working-with-heterogeneous-entity-types)

### <a name="inter-partition-secondary-index-pattern"></a><span data-ttu-id="4e549-487">Vzor sekundární index mezi oddílů</span><span class="sxs-lookup"><span data-stu-id="4e549-487">Inter-partition secondary index pattern</span></span>
<span data-ttu-id="4e549-488">Uložit více kopií každou entitu s využitím různých **RowKey** hodnoty v samostatných oddílů nebo v samostatné tabulky, které chcete povolit rychlé a efektivní vyhledávání a alternativní pořadí řazení pomocí různých **RowKey** hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4e549-488">Store multiple copies of each entity using different **RowKey** values in separate partitions or in separate tables to enable fast and efficient lookups and alternate sort orders by using different **RowKey** values.</span></span>  

#### <a name="context-and-problem"></a><span data-ttu-id="4e549-489">Kontext a problém</span><span class="sxs-lookup"><span data-stu-id="4e549-489">Context and problem</span></span>
<span data-ttu-id="4e549-490">Služba Table automaticky indexuje entity pomocí **PartitionKey** a **RowKey** hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4e549-490">The Table service automatically indexes entities using the **PartitionKey** and **RowKey** values.</span></span> <span data-ttu-id="4e549-491">To umožňuje klientskou aplikaci k načtení entity efektivně pomocí těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="4e549-491">This enables a client application to retrieve an entity efficiently using these values.</span></span> <span data-ttu-id="4e549-492">Například struktura tabulky vidíte níže, klientská aplikace pomocí dotazu bod k načtení entity jednotlivých zaměstnanců pomocí názvu oddělení a id zaměstnance ( **PartitionKey** a **RowKey** hodnoty).</span><span class="sxs-lookup"><span data-stu-id="4e549-492">For example, using the table structure shown below, a client application can use a point query to retrieve an individual employee entity by using the department name and the employee id (the **PartitionKey** and **RowKey** values).</span></span> <span data-ttu-id="4e549-493">Klienta můžete také načíst entity seřazené podle id zaměstnance v rámci každé oddělení.</span><span class="sxs-lookup"><span data-stu-id="4e549-493">A client can also retrieve entities sorted by employee id within each department.</span></span>  

![][9]

<span data-ttu-id="4e549-494">Pokud chcete být schopni najít entitu zaměstnanců na základě hodnoty, jiné vlastnosti, jako je například e-mailovou adresu, musíte použít méně efektivní prohledávání oddílu k vyhledání shody.</span><span class="sxs-lookup"><span data-stu-id="4e549-494">If you also want to be able to find an employee entity based on the value of another property, such as email address, you must use a less efficient partition scan to find a match.</span></span> <span data-ttu-id="4e549-495">Je to proto, že služby table neposkytuje sekundární indexy.</span><span class="sxs-lookup"><span data-stu-id="4e549-495">This is because the table service does not provide secondary indexes.</span></span> <span data-ttu-id="4e549-496">Kromě toho není žádná možnost odeslat žádost o seznam zaměstnanců seřazený v jiném pořadí než **RowKey** pořadí.</span><span class="sxs-lookup"><span data-stu-id="4e549-496">In addition, there is no option to request a list of employees sorted in a different order than **RowKey** order.</span></span>  

<span data-ttu-id="4e549-497">Jsou očekávání velmi velký objem transakcí proti tyto entity a chcete minimalizovat riziko omezení vašeho klienta služby Table.</span><span class="sxs-lookup"><span data-stu-id="4e549-497">You are anticipating a very high volume of transactions against these entities and want to minimize the risk of the Table service throttling your client.</span></span>  

#### <a name="solution"></a><span data-ttu-id="4e549-498">Řešení</span><span class="sxs-lookup"><span data-stu-id="4e549-498">Solution</span></span>
<span data-ttu-id="4e549-499">Nedostatečná sekundární indexy obejít, můžete uložit více kopií každé entity s každou kopírování pomocí různých **PartitionKey** a **RowKey** hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4e549-499">To work around the lack of secondary indexes, you can store multiple copies of each entity with each copy using different **PartitionKey** and **RowKey** values.</span></span> <span data-ttu-id="4e549-500">Pokud ukládáte entitu s struktury vidíte níže, můžete efektivně načíst zaměstnanec entit na základě id e-mailovou adresu nebo zaměstnanců. Předpona hodnoty **PartitionKey**, "empid_" a "email_" umožňují identifikovat index, který chcete použít pro dotaz.</span><span class="sxs-lookup"><span data-stu-id="4e549-500">If you store an entity with the structures shown below, you can efficiently retrieve employee entities based on email address or employee id. The prefix values for the **PartitionKey**, "empid_" and "email_" enable you to identify which index you want to use for a query.</span></span>  

![][10]

<span data-ttu-id="4e549-501">Následující dvě kritéria filtru, která (jeden vyhledávání podle id zaměstnance a jeden vyhledávání pomocí e-mailové adresy) i zadejte bod dotazy:</span><span class="sxs-lookup"><span data-stu-id="4e549-501">The following two filter criteria (one looking up by employee id and one looking up by email address) both specify point queries:</span></span>  

* <span data-ttu-id="4e549-502">$filter = (PartitionKey eq ' empid_Sales') a (RowKey eq '000223')</span><span class="sxs-lookup"><span data-stu-id="4e549-502">$filter=(PartitionKey eq 'empid_Sales') and (RowKey eq '000223')</span></span>
* <span data-ttu-id="4e549-503">$filter = (PartitionKey eq ' email_Sales') a (RowKey eq 'jonesj@contoso.com')</span><span class="sxs-lookup"><span data-stu-id="4e549-503">$filter=(PartitionKey eq 'email_Sales') and (RowKey eq 'jonesj@contoso.com')</span></span>  

<span data-ttu-id="4e549-504">Pokud vyhledat rozsahu entit zaměstnanců, můžete zadat rozsah id řazení zaměstnanců nebo rozsahu seřazeny v e-mailovou adresu pořadí pomocí dotazu pro entity v příslušnou předponu **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="4e549-504">If you query for a range of employee entities, you can specify a range sorted in employee id order, or a range sorted in email address order by querying for entities with the appropriate prefix in the **RowKey**.</span></span>  

* <span data-ttu-id="4e549-505">Najít všechny zaměstnance z oddělení prodeje s id zaměstnance v rozsahu **000100** k **000199** seřazeny ve zaměstnanec id pořadí použití: $filter = (PartitionKey eq ' empid_Sales') a (RowKey ge '000100') a (RowKey le '000199')</span><span class="sxs-lookup"><span data-stu-id="4e549-505">To find all the employees in the Sales department with an employee id in the range **000100** to **000199** sorted in employee id order use: $filter=(PartitionKey eq 'empid_Sales') and (RowKey ge '000100') and (RowKey le '000199')</span></span>  
* <span data-ttu-id="4e549-506">Najít všechny zaměstnance z oddělení prodeje s e-mailovou adresu, který začíná slovem "a" seřazených podle e-mailovou adresu pořadí použití: $filter = (PartitionKey eq ' email_Sales') a (RowKey ge "a") a (RowKey lt "b")</span><span class="sxs-lookup"><span data-stu-id="4e549-506">To find all the employees in the Sales department with an email address that starts with 'a' sorted in email address order use: $filter=(PartitionKey eq 'email_Sales') and (RowKey ge 'a') and (RowKey lt 'b')</span></span>  

<span data-ttu-id="4e549-507">Všimněte si, že se používá ve výše uvedených příkladech syntaxe filtru je ze služby Table rozhraní REST API, další informace najdete v tématu [dotazu entity](http://msdn.microsoft.com/library/azure/dd179421.aspx).</span><span class="sxs-lookup"><span data-stu-id="4e549-507">Note that the filter syntax used in the examples above is from the Table service REST API, for more information see [Query Entities](http://msdn.microsoft.com/library/azure/dd179421.aspx).</span></span>  

#### <a name="issues-and-considerations"></a><span data-ttu-id="4e549-508">Problémy a důležité informace</span><span class="sxs-lookup"><span data-stu-id="4e549-508">Issues and considerations</span></span>
<span data-ttu-id="4e549-509">Při rozhodování o tom, jak implementovat tento vzor, zvažte následující body:</span><span class="sxs-lookup"><span data-stu-id="4e549-509">Consider the following points when deciding how to implement this pattern:</span></span>  

* <span data-ttu-id="4e549-510">Můžete ponechat duplicitní položky nakonec byl konzistentní mezi sebou pomocí [nakonec byl konzistentní transakce vzor](#eventually-consistent-transactions-pattern) udržovat entity primární a sekundární index.</span><span class="sxs-lookup"><span data-stu-id="4e549-510">You can keep your duplicate entities eventually consistent with each other by using the [Eventually consistent transactions pattern](#eventually-consistent-transactions-pattern) to maintain the primary and secondary index entities.</span></span>  
* <span data-ttu-id="4e549-511">Table storage je relativně levný abyste nároky na náklady na ukládání duplicitních dat nesmí být závažný problém.</span><span class="sxs-lookup"><span data-stu-id="4e549-511">Table storage is relatively cheap to use so the cost overhead of storing duplicate data should not be a major concern.</span></span> <span data-ttu-id="4e549-512">Však měli vždy vyhodnoceny náklady návrhu na základě požadavků vaší předpokládaného úložiště a přidat pouze duplicitní entity, které podporují dotazy, které budou spuštěny klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="4e549-512">However, you should always evaluate the cost of your design based on your anticipated storage requirements and only add duplicate entities to support the queries your client application will execute.</span></span>  
* <span data-ttu-id="4e549-513">Hodnota použitá **RowKey** musí být jedinečný pro každé entity.</span><span class="sxs-lookup"><span data-stu-id="4e549-513">The value used for the **RowKey** must be unique for each entity.</span></span> <span data-ttu-id="4e549-514">Zvažte použití hodnoty složeného klíče.</span><span class="sxs-lookup"><span data-stu-id="4e549-514">Consider using compound key values.</span></span>  
* <span data-ttu-id="4e549-515">Odsazení číselných hodnot v **RowKey** (například id zaměstnance 000223), umožňuje opravte řazení a filtrování na základě horní a dolní meze.</span><span class="sxs-lookup"><span data-stu-id="4e549-515">Padding numeric values in the **RowKey** (for example, the employee id 000223), enables correct sorting and filtering based on upper and lower bounds.</span></span>  
* <span data-ttu-id="4e549-516">Nutně není potřeba duplicitní vlastnosti vaší entity.</span><span class="sxs-lookup"><span data-stu-id="4e549-516">You do not necessarily need to duplicate all the properties of your entity.</span></span> <span data-ttu-id="4e549-517">Například pokud dotazy tento vyhledávací entity pomocí e-mailu adres v **RowKey** nikdy nepotřebují stáří zaměstnance, tyto entit může mít následující strukturu:</span><span class="sxs-lookup"><span data-stu-id="4e549-517">For example, if the queries that lookup the entities using the email address in the **RowKey** never need the employee's age, these entities could have the following structure:</span></span>
  
  ![][11]
* <span data-ttu-id="4e549-518">Je obvykle lepší uložit duplicitní data a ujistěte se, že můžete načíst všechna data, která je nutné se jeden dotaz než chcete použijte jeden dotaz a vyhledejte entitu v primární index sekundární index a druhou pro vyhledávání požadovaná data.</span><span class="sxs-lookup"><span data-stu-id="4e549-518">It is typically better to store duplicate data and ensure that you can retrieve all the data you need with a single query than to use one query to locate an entity using the secondary index and another to lookup the required data in the primary index.</span></span>  

#### <a name="when-to-use-this-pattern"></a><span data-ttu-id="4e549-519">Kdy použít tento vzor</span><span class="sxs-lookup"><span data-stu-id="4e549-519">When to use this pattern</span></span>
<span data-ttu-id="4e549-520">Tento vzor použijte, když klientské aplikace potřebuje k načtení entity pomocí různých odlišné klíče, když váš klient potřebuje k načtení entity v jiné pořadí řazení, a tam, kde můžete identifikovat každé entity pomocí různých jedinečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4e549-520">Use this pattern when your client application needs to retrieve entities using a variety of different keys, when your client needs to retrieve entities in different sort orders, and where you can identify each entity using a variety of unique values.</span></span> <span data-ttu-id="4e549-521">Použít tento vzor, pokud chcete, aby nedošlo k překročení omezení škálovatelnosti oddíl při provádění entity vyhledávání pomocí různými **RowKey** hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4e549-521">Use this pattern when you want to avoid exceeding the partition scalability limits when you are performing entity lookups using the different **RowKey** values.</span></span>  

#### <a name="related-patterns-and-guidance"></a><span data-ttu-id="4e549-522">Související vzory a pokyny</span><span class="sxs-lookup"><span data-stu-id="4e549-522">Related patterns and guidance</span></span>
<span data-ttu-id="4e549-523">Následující pokyny a vzory může být také relevantní při implementaci tohoto vzoru:</span><span class="sxs-lookup"><span data-stu-id="4e549-523">The following patterns and guidance may also be relevant when implementing this pattern:</span></span>  

* [<span data-ttu-id="4e549-524">Nakonec byl konzistentní transakce vzor</span><span class="sxs-lookup"><span data-stu-id="4e549-524">Eventually consistent transactions pattern</span></span>](#eventually-consistent-transactions-pattern)  
* [<span data-ttu-id="4e549-525">Vzor Intra-partition sekundární index</span><span class="sxs-lookup"><span data-stu-id="4e549-525">Intra-partition secondary index pattern</span></span>](#intra-partition-secondary-index-pattern)  
* [<span data-ttu-id="4e549-526">Složené klíče vzor</span><span class="sxs-lookup"><span data-stu-id="4e549-526">Compound key pattern</span></span>](#compound-key-pattern)  
* [<span data-ttu-id="4e549-527">Transakce skupiny entity</span><span class="sxs-lookup"><span data-stu-id="4e549-527">Entity Group Transactions</span></span>](#entity-group-transactions)  
* [<span data-ttu-id="4e549-528">Práce s typy heterogenní entit</span><span class="sxs-lookup"><span data-stu-id="4e549-528">Working with heterogeneous entity types</span></span>](#working-with-heterogeneous-entity-types)  

### <a name="eventually-consistent-transactions-pattern"></a><span data-ttu-id="4e549-529">Nakonec byl konzistentní transakce vzor</span><span class="sxs-lookup"><span data-stu-id="4e549-529">Eventually consistent transactions pattern</span></span>
<span data-ttu-id="4e549-530">Povolte nakonec byl konzistentní chování mezi hranice oddílů nebo hranice systému úložiště pomocí front Azure.</span><span class="sxs-lookup"><span data-stu-id="4e549-530">Enable eventually consistent behavior across partition boundaries or storage system boundaries by using Azure queues.</span></span>  

#### <a name="context-and-problem"></a><span data-ttu-id="4e549-531">Kontext a problém</span><span class="sxs-lookup"><span data-stu-id="4e549-531">Context and problem</span></span>
<span data-ttu-id="4e549-532">EGTs povolit jednotlivé transakce mezi více entit, které sdílejí stejný klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="4e549-532">EGTs enable atomic transactions across multiple entities that share the same partition key.</span></span> <span data-ttu-id="4e549-533">Pro výkon a škálovatelnost, můžete rozhodnout pro ukládání entit, které mají požadavky konzistence v samostatné oddíly nebo v samostatné úložiště systému: v takové situaci nelze použít EGTs k zachování konzistence.</span><span class="sxs-lookup"><span data-stu-id="4e549-533">For performance and scalability reasons, you might decide to store entities that have consistency requirements in separate partitions or in a separate storage system: in such a scenario, you cannot use EGTs to maintain consistency.</span></span> <span data-ttu-id="4e549-534">Například můžete mít požadavek zachování konzistence typu případné mezi:</span><span class="sxs-lookup"><span data-stu-id="4e549-534">For example, you might have a requirement to maintain eventual consistency between:</span></span>  

* <span data-ttu-id="4e549-535">Entity uložené ve dvou různých oddílů ve stejné tabulce v různých tabulek ve v jiným účtům úložiště.</span><span class="sxs-lookup"><span data-stu-id="4e549-535">Entities stored in two different partitions in the same table, in different tables, in in different storage accounts.</span></span>  
* <span data-ttu-id="4e549-536">Entity uložené ve službě Table a objekt blob uložené ve službě Blob.</span><span class="sxs-lookup"><span data-stu-id="4e549-536">An entity stored in the Table service and a blob stored in the Blob service.</span></span>  
* <span data-ttu-id="4e549-537">Entity uložené ve službě Table a soubor v systému souborů.</span><span class="sxs-lookup"><span data-stu-id="4e549-537">An entity stored in the Table service and a file in a file system.</span></span>  
* <span data-ttu-id="4e549-538">Úložišti entity ve službě Table ještě indexovat pomocí služby Azure Search.</span><span class="sxs-lookup"><span data-stu-id="4e549-538">An entity store in the Table service yet indexed using the Azure Search service.</span></span>  

#### <a name="solution"></a><span data-ttu-id="4e549-539">Řešení</span><span class="sxs-lookup"><span data-stu-id="4e549-539">Solution</span></span>
<span data-ttu-id="4e549-540">Pomocí fronty Azure můžete implementovat řešení, které nabízí konzistence typu případné mezi dva nebo více oddílů nebo úložných systémů.</span><span class="sxs-lookup"><span data-stu-id="4e549-540">By using Azure queues, you can implement a solution that delivers eventual consistency across two or more partitions or storage systems.</span></span>
<span data-ttu-id="4e549-541">Pro ilustraci tohoto přístupu, předpokládá, že máte požadavek mohli archivovat starší zaměstnanec entity.</span><span class="sxs-lookup"><span data-stu-id="4e549-541">To illustrate this approach, assume you have a requirement to be able to archive old employee entities.</span></span> <span data-ttu-id="4e549-542">Starší zaměstnanec entity jsou zřídka dotaz a mají být vyloučeny z všechny aktivity, které pracují s aktuální zaměstnanci.</span><span class="sxs-lookup"><span data-stu-id="4e549-542">Old employee entities are rarely queried and should be excluded from any activities that deal with current employees.</span></span> <span data-ttu-id="4e549-543">K implementaci tento požadavek ukládáte aktivní zaměstnance v **aktuální** tabulky a původní zaměstnance v **archivu** tabulky.</span><span class="sxs-lookup"><span data-stu-id="4e549-543">To implement this requirement you store active employees in the **Current** table and old employees in the **Archive** table.</span></span> <span data-ttu-id="4e549-544">Archivace zaměstnanec vyžaduje, abyste odstranit entitu z **aktuální** tabulky a přidejte entity, která má **archivu** tabulky, ale nelze použít EGT k provedení těchto dvou operací.</span><span class="sxs-lookup"><span data-stu-id="4e549-544">Archiving an employee requires you to delete the entity from the **Current** table and add the entity to the **Archive** table, but you cannot use an EGT to perform these two operations.</span></span> <span data-ttu-id="4e549-545">Aby nedošlo k ohrožení, který selhání způsobí, že entita, než se objeví v obou nebo ani jedno z těchto tabulek, musí být operace archivování nakonec byl konzistentní.</span><span class="sxs-lookup"><span data-stu-id="4e549-545">To avoid the risk that a failure causes an entity to appear in both or neither tables, the archive operation must be eventually consistent.</span></span> <span data-ttu-id="4e549-546">Následující diagram pořadí popisuje kroky v této operaci.</span><span class="sxs-lookup"><span data-stu-id="4e549-546">The following sequence diagram outlines the steps in this operation.</span></span> <span data-ttu-id="4e549-547">Podrobněji se poskytuje pro cesty výjimka v následující text.</span><span class="sxs-lookup"><span data-stu-id="4e549-547">More detail is provided for exception paths in the text following.</span></span>  

![][12]

<span data-ttu-id="4e549-548">Klient zahájí operaci archivu umístěním zprávu na fronty Azure, v tomto příkladu k archivaci zaměstnanec #456.</span><span class="sxs-lookup"><span data-stu-id="4e549-548">A client initiates the archive operation by placing a message on an Azure queue, in this example to archive employee #456.</span></span> <span data-ttu-id="4e549-549">Role pracovního procesu dotazuje fronty pro nové zprávy; v případě, že některou najde, přečte zprávu a zůstanou skrytá kopie ve frontě.</span><span class="sxs-lookup"><span data-stu-id="4e549-549">A worker role polls the queue for new messages; when it finds one, it reads the message and leaves a hidden copy on the queue.</span></span> <span data-ttu-id="4e549-550">Role pracovního procesu vedle načte kopii entita z **aktuální** tabulky, vloží kopii v **archivu** tabulky a poté se odstraní původní z **aktuální** tabulky.</span><span class="sxs-lookup"><span data-stu-id="4e549-550">The worker role next fetches a copy of the entity from the **Current** table, inserts a copy in the **Archive** table, and then deletes the original from the **Current** table.</span></span> <span data-ttu-id="4e549-551">Nakonec pokud nedošlo k chybám z předchozích kroků, role pracovního procesu odstraní skryté zprávy z fronty.</span><span class="sxs-lookup"><span data-stu-id="4e549-551">Finally, if there were no errors from the previous steps, the worker role deletes the hidden message from the queue.</span></span>  

<span data-ttu-id="4e549-552">V tomto příkladu krok 4 vloží zaměstnanec do **archivu** tabulky.</span><span class="sxs-lookup"><span data-stu-id="4e549-552">In this example, step 4 inserts the employee into the **Archive** table.</span></span> <span data-ttu-id="4e549-553">Zaměstnanec ho přidat do objektu blob ve službě Blob nebo souboru v systému souborů.</span><span class="sxs-lookup"><span data-stu-id="4e549-553">It could add the employee to a blob in the Blob service or a file in a file system.</span></span>  

#### <a name="recovering-from-failures"></a><span data-ttu-id="4e549-554">Zotavení z chyb</span><span class="sxs-lookup"><span data-stu-id="4e549-554">Recovering from failures</span></span>
<span data-ttu-id="4e549-555">Je důležité, operace v krocích **4** a **5** musí být *idempotent* v případě, že role pracovního procesu je nutné restartovat operaci archivu.</span><span class="sxs-lookup"><span data-stu-id="4e549-555">It is important that the operations in steps **4** and **5** must be *idempotent* in case the worker role needs to restart the archive operation.</span></span> <span data-ttu-id="4e549-556">Pokud používáte službu tabulky pro krok **4** byste měli používat operace "vložení nebo nahrazení"; pro krok **5** byste měli používat "odstranit, pokud existuje" operace v knihovně klienta, který používáte.</span><span class="sxs-lookup"><span data-stu-id="4e549-556">If you are using the Table service, for step **4** you should use an "insert or replace" operation; for step **5** you should use a "delete if exists" operation in the client library you are using.</span></span> <span data-ttu-id="4e549-557">Pokud používáte jiné úložiště systému, musíte použít příslušné idempotent operace.</span><span class="sxs-lookup"><span data-stu-id="4e549-557">If you are using another storage system, you must use an appropriate idempotent operation.</span></span>  

<span data-ttu-id="4e549-558">Pokud role pracovního procesu nedokončí krok **6**, pak po vypršení časového limitu se zpráva zobrazí znovu ve frontě připravené pro roli pracovního procesu se pokuste znovu ji zpracovat.</span><span class="sxs-lookup"><span data-stu-id="4e549-558">If the worker role never completes step **6**, then after a timeout the message reappears on the queue ready for the worker role to try to reprocess it.</span></span> <span data-ttu-id="4e549-559">Role pracovního procesu můžete zkontrolovat, kolikrát byl zprávu ve frontě číst a v případě potřeby příznak je zprávu "poškozených" pro zkoumání odesláním do samostatné fronty.</span><span class="sxs-lookup"><span data-stu-id="4e549-559">The worker role can check how many times a message on the queue has been read and, if necessary, flag it is a "poison" message for investigation by sending it to a separate queue.</span></span> <span data-ttu-id="4e549-560">Další informace o čtení zprávy fronty a kontrola počtu dequeue najdete v tématu [získat zprávy](https://msdn.microsoft.com/library/azure/dd179474.aspx).</span><span class="sxs-lookup"><span data-stu-id="4e549-560">For more information about reading queue messages and checking the dequeue count, see [Get Messages](https://msdn.microsoft.com/library/azure/dd179474.aspx).</span></span>  

<span data-ttu-id="4e549-561">Některé chyby ze služby Table a Queue jsou přechodné chyby a klientské aplikace by měla obsahovat logiku vhodný opakování a mohli je zpracovat.</span><span class="sxs-lookup"><span data-stu-id="4e549-561">Some errors from the Table and Queue services are transient errors, and your client application should include suitable retry logic to handle them.</span></span>  

#### <a name="issues-and-considerations"></a><span data-ttu-id="4e549-562">Problémy a důležité informace</span><span class="sxs-lookup"><span data-stu-id="4e549-562">Issues and considerations</span></span>
<span data-ttu-id="4e549-563">Při rozhodování o tom, jak implementovat tento vzor, zvažte následující body:</span><span class="sxs-lookup"><span data-stu-id="4e549-563">Consider the following points when deciding how to implement this pattern:</span></span>  

* <span data-ttu-id="4e549-564">Toto řešení neposkytuje pro izolace transakce.</span><span class="sxs-lookup"><span data-stu-id="4e549-564">This solution does not provide for transaction isolation.</span></span> <span data-ttu-id="4e549-565">Například může číst klienta **aktuální** a **archivu** tabulky, pokud byl roli pracovního procesu mezi kroky **4** a **5**a zobrazit nekonzistentní zobrazení data.</span><span class="sxs-lookup"><span data-stu-id="4e549-565">For example, a client could read the **Current** and **Archive** tables when the worker role was between steps **4** and **5**, and see an inconsistent view of the data.</span></span> <span data-ttu-id="4e549-566">Všimněte si, že data bude konzistentní nakonec.</span><span class="sxs-lookup"><span data-stu-id="4e549-566">Note that the data will be consistent eventually.</span></span>  
* <span data-ttu-id="4e549-567">Je nutné zajistit, že jsou kroky 4 a 5 idempotent pro zajištění konzistence typu případné.</span><span class="sxs-lookup"><span data-stu-id="4e549-567">You must be sure that steps 4 and 5 are idempotent in order to ensure eventual consistency.</span></span>  
* <span data-ttu-id="4e549-568">Řešení můžete škálovat pomocí více front a instance rolí pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="4e549-568">You can scale the solution by using multiple queues and worker role instances.</span></span>  

#### <a name="when-to-use-this-pattern"></a><span data-ttu-id="4e549-569">Kdy použít tento vzor</span><span class="sxs-lookup"><span data-stu-id="4e549-569">When to use this pattern</span></span>
<span data-ttu-id="4e549-570">Tento vzor použijte, pokud chcete zaručit konzistence typu případné mezi entitami, které existují v různých oddílů nebo tabulky.</span><span class="sxs-lookup"><span data-stu-id="4e549-570">Use this pattern when you want to guarantee eventual consistency between entities that exist in different partitions or tables.</span></span> <span data-ttu-id="4e549-571">Můžete rozšířit tento vzor pro zajištění konzistence typu případné pro operace napříč služby Table a služby objektů Blob a dalších Azure úložných zdroje dat jako databázi nebo systému souborů.</span><span class="sxs-lookup"><span data-stu-id="4e549-571">You can extend this pattern to ensure eventual consistency for operations across the Table service and the Blob service and other non-Azure Storage data sources such as database or the file system.</span></span>  

#### <a name="related-patterns-and-guidance"></a><span data-ttu-id="4e549-572">Související vzory a pokyny</span><span class="sxs-lookup"><span data-stu-id="4e549-572">Related patterns and guidance</span></span>
<span data-ttu-id="4e549-573">Následující pokyny a vzory může být také relevantní při implementaci tohoto vzoru:</span><span class="sxs-lookup"><span data-stu-id="4e549-573">The following patterns and guidance may also be relevant when implementing this pattern:</span></span>  

* [<span data-ttu-id="4e549-574">Transakce skupiny entity</span><span class="sxs-lookup"><span data-stu-id="4e549-574">Entity Group Transactions</span></span>](#entity-group-transactions)  
* [<span data-ttu-id="4e549-575">Sloučení nebo nahradit</span><span class="sxs-lookup"><span data-stu-id="4e549-575">Merge or replace</span></span>](#merge-or-replace)  

> [!NOTE]
> <span data-ttu-id="4e549-576">Pokud izolace transakce je důležité pro vaše řešení, měli byste zvážit přepracování vaše tabulky, abyste mohli využívat EGTs.</span><span class="sxs-lookup"><span data-stu-id="4e549-576">If transaction isolation is important to your solution, you should consider redesigning your tables to enable you to use EGTs.</span></span>  
> 
> 

### <a name="index-entities-pattern"></a><span data-ttu-id="4e549-577">Vzor entity indexu</span><span class="sxs-lookup"><span data-stu-id="4e549-577">Index Entities Pattern</span></span>
<span data-ttu-id="4e549-578">Udržujte index entity umožňující efektivní hledání, které vrátí seznamy entit.</span><span class="sxs-lookup"><span data-stu-id="4e549-578">Maintain index entities to enable efficient searches that return lists of entities.</span></span>  

#### <a name="context-and-problem"></a><span data-ttu-id="4e549-579">Kontext a problém</span><span class="sxs-lookup"><span data-stu-id="4e549-579">Context and problem</span></span>
<span data-ttu-id="4e549-580">Služba Table automaticky indexuje entity pomocí **PartitionKey** a **RowKey** hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4e549-580">The Table service automatically indexes entities using the **PartitionKey** and **RowKey** values.</span></span> <span data-ttu-id="4e549-581">To umožňuje klientskou aplikaci k načtení entity efektivní použití bodu dotazu.</span><span class="sxs-lookup"><span data-stu-id="4e549-581">This enables a client application to retrieve an entity efficiently using a point query.</span></span> <span data-ttu-id="4e549-582">Například pomocí struktura tabulky vidíte níže, klientská aplikace můžete efektivně načíst entity jednotlivých zaměstnanců pomocí názvu oddělení a id zaměstnance ( **PartitionKey** a **RowKey**).</span><span class="sxs-lookup"><span data-stu-id="4e549-582">For example, using the table structure shown below, a client application can efficiently retrieve an individual employee entity by using the department name and the employee id (the **PartitionKey** and **RowKey**).</span></span>  

![][13]

<span data-ttu-id="4e549-583">Pokud chcete také moci načíst seznam zaměstnanec entit na základě hodnoty jiný jedinečný vlastnosti, například jejich poslední název, musíte použít méně efektivní prohledávání oddílu najít odpovídá spíše než pomocí indexu je přímo vyhledat.</span><span class="sxs-lookup"><span data-stu-id="4e549-583">If you also want to be able to retrieve a list of employee entities based on the value of another non-unique property, such as their last name, you must use a less efficient partition scan to find matches rather than using an index to look them up directly.</span></span> <span data-ttu-id="4e549-584">Je to proto, že služby table neposkytuje sekundární indexy.</span><span class="sxs-lookup"><span data-stu-id="4e549-584">This is because the table service does not provide secondary indexes.</span></span>  

#### <a name="solution"></a><span data-ttu-id="4e549-585">Řešení</span><span class="sxs-lookup"><span data-stu-id="4e549-585">Solution</span></span>
<span data-ttu-id="4e549-586">K povolení vyhledávání podle příjmení s strukturu entity uvedené výše, musíte udržovat seznam ID zaměstnance.</span><span class="sxs-lookup"><span data-stu-id="4e549-586">To enable lookup by last name with the entity structure shown above, you must maintain lists of employee ids.</span></span> <span data-ttu-id="4e549-587">Pokud chcete načíst entity Zaměstnanec s konkrétní příjmení, jako je například Petr, musíte nejprve vyhledat seznam ID zaměstnance pro zaměstnance s Petr jako své příjmení a následně načíst tyto entity zaměstnanců.</span><span class="sxs-lookup"><span data-stu-id="4e549-587">If you want to retrieve the employee entities with a particular last name, such as Jones, you must first locate the list of employee ids for employees with Jones as their last name, and then retrieve those employee entities.</span></span> <span data-ttu-id="4e549-588">Existují tři hlavní možnosti pro ukládání seznam ID zaměstnanců:</span><span class="sxs-lookup"><span data-stu-id="4e549-588">There are three main options for storing the lists of employee ids:</span></span>  

* <span data-ttu-id="4e549-589">Používání úložiště blob.</span><span class="sxs-lookup"><span data-stu-id="4e549-589">Use blob storage.</span></span>  
* <span data-ttu-id="4e549-590">Vytvořte index entity v stejného oddílu jako entity zaměstnanců.</span><span class="sxs-lookup"><span data-stu-id="4e549-590">Create index entities in the same partition as the employee entities.</span></span>  
* <span data-ttu-id="4e549-591">Vytvořte index entity v samostatném oddílu nebo tabulky.</span><span class="sxs-lookup"><span data-stu-id="4e549-591">Create index entities in a separate partition or table.</span></span>  

<span data-ttu-id="4e549-592"><u>Možnost #1: Použití objektu blob storage</u></span><span class="sxs-lookup"><span data-stu-id="4e549-592"><u>Option #1: Use blob storage</u></span></span>  

<span data-ttu-id="4e549-593">Pro první možnost vytvoříte objekt blob pro každý jedinečný příjmení a na každý objekt blob úložiště seznam **PartitionKey** (oddělení) a **RowKey** hodnoty (zaměstnanec id) pro zaměstnance, kteří mají tento příjmení.</span><span class="sxs-lookup"><span data-stu-id="4e549-593">For the first option, you create a blob for every unique last name, and in each blob store a list of the **PartitionKey** (department) and **RowKey** (employee id) values for employees that have that last name.</span></span> <span data-ttu-id="4e549-594">Při přidání nebo odstranění zaměstnanec se ujistěte, že je obsah objektu blob relevantní nakonec byl konzistentní se zaměstnanec entity.</span><span class="sxs-lookup"><span data-stu-id="4e549-594">When you add or delete an employee you should ensure that the content of the relevant blob is eventually consistent with the employee entities.</span></span>  

<span data-ttu-id="4e549-595"><u>Možnost #2:</u> vytvořte index entity v stejného oddílu</span><span class="sxs-lookup"><span data-stu-id="4e549-595"><u>Option #2:</u> Create index entities in the same partition</span></span>  

<span data-ttu-id="4e549-596">Pro druhou možnost použijte index entit, které obsahují následující údaje:</span><span class="sxs-lookup"><span data-stu-id="4e549-596">For the second option, use index entities that store the following data:</span></span>  

![][14]

<span data-ttu-id="4e549-597">**EmployeeIDs** vlastnost obsahuje seznam identifikátorů zaměstnanec pro zaměstnance s příjmení uložené v **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="4e549-597">The **EmployeeIDs** property contains a list of employee ids for employees with the last name stored in the **RowKey**.</span></span>  

<span data-ttu-id="4e549-598">Následující kroky popisují proces, který byste měli postupovat při přidávání nového zaměstnance Pokud používáte druhou možnost.</span><span class="sxs-lookup"><span data-stu-id="4e549-598">The following steps outline the process you should follow when you are adding a new employee if you are using the second option.</span></span> <span data-ttu-id="4e549-599">V tomto příkladu přidáváme zaměstnance s Id 000152 a příjmení Petr z oddělení prodeje:</span><span class="sxs-lookup"><span data-stu-id="4e549-599">In this example, we are adding an employee with Id 000152 and a last name Jones in the Sales department:</span></span>  

1. <span data-ttu-id="4e549-600">Načtení entity index s **PartitionKey** hodnotu "Prodej" a **RowKey** hodnotu "Petr."</span><span class="sxs-lookup"><span data-stu-id="4e549-600">Retrieve the index entity with a **PartitionKey** value "Sales" and the **RowKey** value "Jones."</span></span> <span data-ttu-id="4e549-601">Uložte značku ETag tuto entitu použijte v kroku 2.</span><span class="sxs-lookup"><span data-stu-id="4e549-601">Save the ETag of this entity to use in step 2.</span></span>  
2. <span data-ttu-id="4e549-602">Vytvoření skupiny transakci entity (tedy dávkovou operaci), která vloží novou entitu zaměstnanec (**PartitionKey** hodnotu "Prodej" a **RowKey** hodnotu "000152") a aktualizuje entitu indexu (**PartitionKey** hodnotu "Prodej" a **RowKey** hodnotu "Petr") přidáním nové id zaměstnance do seznamu v poli EmployeeIDs.</span><span class="sxs-lookup"><span data-stu-id="4e549-602">Create an entity group transaction (that is, a batch operation) that inserts the new employee entity (**PartitionKey** value "Sales" and **RowKey** value "000152"), and updates the index entity (**PartitionKey** value "Sales" and **RowKey** value "Jones") by adding the new employee id to the list in the EmployeeIDs field.</span></span> <span data-ttu-id="4e549-603">Další informace o transakcích skupiny entity najdete v tématu [Entity skupiny transakce](#entity-group-transactions).</span><span class="sxs-lookup"><span data-stu-id="4e549-603">For more information about entity group transactions, see [Entity Group Transactions](#entity-group-transactions).</span></span>  
3. <span data-ttu-id="4e549-604">Pokud se transakce skupiny entity nezdaří z důvodu chyby optimistickou metodu souběžného (někdo jiný právě změnil index entity), budete muset v kroku 1 znovu spustit.</span><span class="sxs-lookup"><span data-stu-id="4e549-604">If the entity group transaction fails because of an optimistic concurrency error (someone else has just modified the index entity), then you need to start over at step 1 again.</span></span>  

<span data-ttu-id="4e549-605">Můžete podobný postup k odstranění zaměstnanec, pokud používáte druhou možnost.</span><span class="sxs-lookup"><span data-stu-id="4e549-605">You can use a similar approach to deleting an employee if you are using the second option.</span></span> <span data-ttu-id="4e549-606">Změna příjmení zaměstnance je trochu složitější, protože budete muset spustit transakci skupiny entity, která aktualizuje tři entity: Entita zaměstnanců, index entity pro původní příjmení a entity index pro nové příjmení.</span><span class="sxs-lookup"><span data-stu-id="4e549-606">Changing an employee's last name is slightly more complex because you will need to execute an entity group transaction that updates three entities: the employee entity, the index entity for the old last name, and the index entity for the new last name.</span></span> <span data-ttu-id="4e549-607">Před provedením jakýchkoli změn, aby bylo možné načíst ETag hodnoty, které pak můžete použít k provedení aktualizací pomocí optimistickou metodu souběžného musí získat každé entity.</span><span class="sxs-lookup"><span data-stu-id="4e549-607">You must retrieve each entity before making any changes in order to retrieve the ETag values that you can then use to perform the updates using optimistic concurrency.</span></span>  

<span data-ttu-id="4e549-608">Následující kroky popisují proces, který byste měli postupovat, když potřebujete vyhledat všechny zaměstnance se daný příjmení v oddělení, pokud používáte druhou možnost.</span><span class="sxs-lookup"><span data-stu-id="4e549-608">The following steps outline the process you should follow when you need to look up all the employees with a given last name in a department if you are using the second option.</span></span> <span data-ttu-id="4e549-609">V tomto příkladu jsme jsou všechny zaměstnance se příjmení Petr prodejního oddělení vyhledávány:</span><span class="sxs-lookup"><span data-stu-id="4e549-609">In this example, we are looking up all the employees with last name Jones in the Sales department:</span></span>  

1. <span data-ttu-id="4e549-610">Načtení entity index s **PartitionKey** hodnotu "Prodej" a **RowKey** hodnotu "Petr."</span><span class="sxs-lookup"><span data-stu-id="4e549-610">Retrieve the index entity with a **PartitionKey** value "Sales" and the **RowKey** value "Jones."</span></span>  
2. <span data-ttu-id="4e549-611">Analyzovat seznam ID v poli EmployeeIDs zaměstnanců.</span><span class="sxs-lookup"><span data-stu-id="4e549-611">Parse the list of employee Ids in the EmployeeIDs field.</span></span>  
3. <span data-ttu-id="4e549-612">Pokud potřebujete další informace o jednotlivých tito zaměstnanci (například jejich e-mailové adresy), načtení jednotlivých entit zaměstnanec pomocí **PartitionKey** hodnotu "Prodej" a **RowKey** hodnoty ze seznamu zaměstnanců, které jste získali v kroku 2.</span><span class="sxs-lookup"><span data-stu-id="4e549-612">If you need additional information about each of these employees (such as their email addresses), retrieve each of the employee entities using **PartitionKey** value "Sales" and **RowKey** values from the list of employees you obtained in step 2.</span></span>  

<span data-ttu-id="4e549-613"><u>Možnost #3:</u> vytvořte index entity v samostatném oddílu nebo tabulky</span><span class="sxs-lookup"><span data-stu-id="4e549-613"><u>Option #3:</u> Create index entities in a separate partition or table</span></span>  

<span data-ttu-id="4e549-614">Třetí možností použijte index entit, které obsahují následující údaje:</span><span class="sxs-lookup"><span data-stu-id="4e549-614">For the third option, use index entities that store the following data:</span></span>  

![][15]

<span data-ttu-id="4e549-615">**EmployeeIDs** vlastnost obsahuje seznam identifikátorů zaměstnanec pro zaměstnance s příjmení uložené v **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="4e549-615">The **EmployeeIDs** property contains a list of employee ids for employees with the last name stored in the **RowKey**.</span></span>  

<span data-ttu-id="4e549-616">S parametrem třetí nelze použít EGTs můžete zachovat konzistenci, protože index entity, které jsou v samostatném oddílu z entit zaměstnanců.</span><span class="sxs-lookup"><span data-stu-id="4e549-616">With the third option, you cannot use EGTs to maintain consistency because the index entities are in a separate partition from the employee entities.</span></span> <span data-ttu-id="4e549-617">Měli byste zajistit, že index entity, které jsou nakonec byl konzistentní se entity, které zaměstnanec.</span><span class="sxs-lookup"><span data-stu-id="4e549-617">You should ensure that the index entities are eventually consistent with the employee entities.</span></span>  

#### <a name="issues-and-considerations"></a><span data-ttu-id="4e549-618">Problémy a důležité informace</span><span class="sxs-lookup"><span data-stu-id="4e549-618">Issues and considerations</span></span>
<span data-ttu-id="4e549-619">Při rozhodování o tom, jak implementovat tento vzor, zvažte následující body:</span><span class="sxs-lookup"><span data-stu-id="4e549-619">Consider the following points when deciding how to implement this pattern:</span></span>  

* <span data-ttu-id="4e549-620">Toto řešení vyžaduje alespoň dva dotazy k načtení odpovídající entity: jeden k dotazování indexu entity k získání seznamu **RowKey** hodnoty a pak dotazy k načtení jednotlivých entit v seznamu.</span><span class="sxs-lookup"><span data-stu-id="4e549-620">This solution requires at least two queries to retrieve matching entities: one to query the index entities to obtain the list of **RowKey** values, and then queries to retrieve each entity in the list.</span></span>  
* <span data-ttu-id="4e549-621">Vzhledem k tomu, že jednotlivé entity má maximální velikost 1 MB, možnost #2 a možnost #3 v řešení předpokládá, že seznam ID zaměstnance pro danou příjmení je nikdy větší než 1 MB.</span><span class="sxs-lookup"><span data-stu-id="4e549-621">Given that an individual entity has a maximum size of 1 MB, option #2 and option #3 in the solution assume that the list of employee ids for any given last name is never greater than 1 MB.</span></span> <span data-ttu-id="4e549-622">Pokud seznam ID zaměstnance je pravděpodobně být větší než velikost 1 MB, použijte možnost #1 a ukládat data indexu v úložišti objektů blob.</span><span class="sxs-lookup"><span data-stu-id="4e549-622">If the list of employee ids is likely to be greater than 1 MB in size, use option #1 and store the index data in blob storage.</span></span>  
* <span data-ttu-id="4e549-623">Pokud použijete možnost #2 (pomocí EGTs zpracování přidávání a odstraňování zaměstnanci a změna zaměstnance příjmení), musíte zjistit Pokud objem transakcí bude postupovat omezení škálovatelnosti v daném oddílu.</span><span class="sxs-lookup"><span data-stu-id="4e549-623">If you use option #2 (using EGTs to handle adding and deleting employees, and changing an employee's last name) you must evaluate if the volume of transactions will approach the scalability limits in a given partition.</span></span> <span data-ttu-id="4e549-624">Pokud je to tento případ, měli byste zvážit nakonec byl konzistentní řešení (možnost #1 nebo #3), které používá fronty pro zpracování žádosti o aktualizaci a umožní vám k ukládání entit indexu v samostatném oddílu z entit zaměstnanců.</span><span class="sxs-lookup"><span data-stu-id="4e549-624">If this is the case, you should consider an eventually consistent solution (option #1 or option #3) that uses queues to handle the update requests and enables you to store your index entities in a separate partition from the employee entities.</span></span>  
* <span data-ttu-id="4e549-625">Možnost #2 v tomto řešení se předpokládá, že chcete vyhledávat podle příjmení v rámci oddělení: například chcete načíst seznam zaměstnancům příjmení Petr z oddělení prodeje.</span><span class="sxs-lookup"><span data-stu-id="4e549-625">Option #2 in this solution assumes that you want to look up by last name within a department: for example, you want to retrieve a list of employees with a last name Jones in the Sales department.</span></span> <span data-ttu-id="4e549-626">Pokud chcete být schopni vyhledat všechny zaměstnance se příjmení Petr napříč celou organizaci, použijte možnost #1 nebo možnost #3.</span><span class="sxs-lookup"><span data-stu-id="4e549-626">If you want to be able to look up all the employees with a last name Jones across the whole organization, use either option #1 or option #3.</span></span>
* <span data-ttu-id="4e549-627">Můžete implementovat řešení na základě fronty, který doručí konzistence typu případné (najdete v článku [nakonec byl konzistentní transakce vzor](#eventually-consistent-transactions-pattern) podrobnosti).</span><span class="sxs-lookup"><span data-stu-id="4e549-627">You can implement a queue-based solution that delivers eventual consistency (see the [Eventually consistent transactions pattern](#eventually-consistent-transactions-pattern) for more details).</span></span>  

#### <a name="when-to-use-this-pattern"></a><span data-ttu-id="4e549-628">Kdy použít tento vzor</span><span class="sxs-lookup"><span data-stu-id="4e549-628">When to use this pattern</span></span>
<span data-ttu-id="4e549-629">Pokud chcete vyhledat sady entit pro všechny sdílené složky běžné hodnotu vlastnosti, například všechny zaměstnance se příjmení Petr, použijte tento vzor.</span><span class="sxs-lookup"><span data-stu-id="4e549-629">Use this pattern when you want to lookup a set of entities that all share a common property value, such as all employees with the last name Jones.</span></span>  

#### <a name="related-patterns-and-guidance"></a><span data-ttu-id="4e549-630">Související vzory a pokyny</span><span class="sxs-lookup"><span data-stu-id="4e549-630">Related patterns and guidance</span></span>
<span data-ttu-id="4e549-631">Následující pokyny a vzory může být také relevantní při implementaci tohoto vzoru:</span><span class="sxs-lookup"><span data-stu-id="4e549-631">The following patterns and guidance may also be relevant when implementing this pattern:</span></span>  

* [<span data-ttu-id="4e549-632">Složené klíče vzor</span><span class="sxs-lookup"><span data-stu-id="4e549-632">Compound key pattern</span></span>](#compound-key-pattern)  
* [<span data-ttu-id="4e549-633">Nakonec byl konzistentní transakce vzor</span><span class="sxs-lookup"><span data-stu-id="4e549-633">Eventually consistent transactions pattern</span></span>](#eventually-consistent-transactions-pattern)  
* [<span data-ttu-id="4e549-634">Transakce skupiny entity</span><span class="sxs-lookup"><span data-stu-id="4e549-634">Entity Group Transactions</span></span>](#entity-group-transactions)  
* [<span data-ttu-id="4e549-635">Práce s typy heterogenní entit</span><span class="sxs-lookup"><span data-stu-id="4e549-635">Working with heterogeneous entity types</span></span>](#working-with-heterogeneous-entity-types)  

### <a name="denormalization-pattern"></a><span data-ttu-id="4e549-636">Vzor denormalization</span><span class="sxs-lookup"><span data-stu-id="4e549-636">Denormalization pattern</span></span>
<span data-ttu-id="4e549-637">Kombinování souvisejících dat společně v jedné entity, aby vám umožňuje načíst všechna data, která je nutné pomocí dotazu jediný bod.</span><span class="sxs-lookup"><span data-stu-id="4e549-637">Combine related data together in a single entity to enable you to retrieve all the data you need with a single point query.</span></span>  

#### <a name="context-and-problem"></a><span data-ttu-id="4e549-638">Kontext a problém</span><span class="sxs-lookup"><span data-stu-id="4e549-638">Context and problem</span></span>
<span data-ttu-id="4e549-639">V relační databázi obvykle normalizaci dat k odebrání duplicitních výsledkem dotazy, které načtení dat z více tabulek.</span><span class="sxs-lookup"><span data-stu-id="4e549-639">In a relational database, you typically normalize data to remove duplication resulting in queries that retrieve data from multiple tables.</span></span> <span data-ttu-id="4e549-640">Pokud jste normalizaci data do tabulek Azure, musíte nastavit více zpátečních cest z klienta na server se načíst související data.</span><span class="sxs-lookup"><span data-stu-id="4e549-640">If you normalize your data in Azure tables, you must make multiple round trips from the client to the server to retrieve your related data.</span></span> <span data-ttu-id="4e549-641">Například s struktura tabulky zobrazené níže potřebovat dvě zpátečních cest k načtení podrobností pro oddělení: jednu pro načtení oddělení entita, která obsahuje id manažera a pak další žádost o načtení manažera podrobnosti v entitě zaměstnanců.</span><span class="sxs-lookup"><span data-stu-id="4e549-641">For example, with the table structure shown below you need two round trips to retrieve the details for a department: one to fetch the department entity that includes the manager's id, and then another request to fetch the manager's details in an employee entity.</span></span>  

![][16]

#### <a name="solution"></a><span data-ttu-id="4e549-642">Řešení</span><span class="sxs-lookup"><span data-stu-id="4e549-642">Solution</span></span>
<span data-ttu-id="4e549-643">Místo ukládání dat do dvou samostatných entit, denormalize data a ponechat si kopii manažera podrobnosti v entitě oddělení.</span><span class="sxs-lookup"><span data-stu-id="4e549-643">Instead of storing the data in two separate entities, denormalize the data and keep a copy of the manager's details in the department entity.</span></span> <span data-ttu-id="4e549-644">Například:</span><span class="sxs-lookup"><span data-stu-id="4e549-644">For example:</span></span>  

![][17]

<span data-ttu-id="4e549-645">S entitami oddělení uložené s těmito vlastnostmi můžete nyní načíst všechny podrobnosti, které je třeba o použití bodu dotazu oddělení.</span><span class="sxs-lookup"><span data-stu-id="4e549-645">With department entities stored with these properties, you can now retrieve all the details you need about a department using a point query.</span></span>  

#### <a name="issues-and-considerations"></a><span data-ttu-id="4e549-646">Problémy a důležité informace</span><span class="sxs-lookup"><span data-stu-id="4e549-646">Issues and considerations</span></span>
<span data-ttu-id="4e549-647">Při rozhodování o tom, jak implementovat tento vzor, zvažte následující body:</span><span class="sxs-lookup"><span data-stu-id="4e549-647">Consider the following points when deciding how to implement this pattern:</span></span>  

* <span data-ttu-id="4e549-648">Není náklady režie spojená s ukládáním některá data dvakrát.</span><span class="sxs-lookup"><span data-stu-id="4e549-648">There is some cost overhead associated with storing some data twice.</span></span> <span data-ttu-id="4e549-649">Výhody výkonu (vyplývající z méně požadavků na službu úložiště) většinou převáží okrajového vzrůst náklady na úložiště (a náklady na tento je částečně posunut snížení počtu transakcí, které budete potřebovat načíst podrobnosti o oddělení).</span><span class="sxs-lookup"><span data-stu-id="4e549-649">The performance benefit (resulting from fewer requests to the storage service) typically outweighs the marginal increase in storage costs (and this cost is partially offset by a reduction in the number of transactions you require to fetch the details of a department).</span></span>  
* <span data-ttu-id="4e549-650">Konzistence dvě entity, které obsahují informace o správcích musí zachovat.</span><span class="sxs-lookup"><span data-stu-id="4e549-650">You must maintain the consistency of the two entities that store information about managers.</span></span> <span data-ttu-id="4e549-651">Problém konzistence můžete řešit pomocí EGTs aktualizace více entit v rámci jedné transakce atomic: v takovém případě entity oddělení a zaměstnanci entity pro správce oddělení ukládají do stejného oddílu.</span><span class="sxs-lookup"><span data-stu-id="4e549-651">You can handle the consistency issue by using EGTs to update multiple entities in a single atomic transaction: in this case, the department entity, and the employee entity for the department manager are stored in the same partition.</span></span>  

#### <a name="when-to-use-this-pattern"></a><span data-ttu-id="4e549-652">Kdy použít tento vzor</span><span class="sxs-lookup"><span data-stu-id="4e549-652">When to use this pattern</span></span>
<span data-ttu-id="4e549-653">Tento vzor použijte, pokud potřebujete často vyhledat související informace.</span><span class="sxs-lookup"><span data-stu-id="4e549-653">Use this pattern when you frequently need to look up related information.</span></span> <span data-ttu-id="4e549-654">Tento vzor snižuje počet dotazů, které musíte provést načíst data, která vyžaduje vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="4e549-654">This pattern reduces the number of queries your client must make to retrieve the data it requires.</span></span>  

#### <a name="related-patterns-and-guidance"></a><span data-ttu-id="4e549-655">Související vzory a pokyny</span><span class="sxs-lookup"><span data-stu-id="4e549-655">Related patterns and guidance</span></span>
<span data-ttu-id="4e549-656">Následující pokyny a vzory může být také relevantní při implementaci tohoto vzoru:</span><span class="sxs-lookup"><span data-stu-id="4e549-656">The following patterns and guidance may also be relevant when implementing this pattern:</span></span>  

* [<span data-ttu-id="4e549-657">Složené klíče vzor</span><span class="sxs-lookup"><span data-stu-id="4e549-657">Compound key pattern</span></span>](#compound-key-pattern)  
* [<span data-ttu-id="4e549-658">Transakce skupiny entity</span><span class="sxs-lookup"><span data-stu-id="4e549-658">Entity Group Transactions</span></span>](#entity-group-transactions)  
* [<span data-ttu-id="4e549-659">Práce s typy heterogenní entit</span><span class="sxs-lookup"><span data-stu-id="4e549-659">Working with heterogeneous entity types</span></span>](#working-with-heterogeneous-entity-types)

### <a name="compound-key-pattern"></a><span data-ttu-id="4e549-660">Složené klíče vzor</span><span class="sxs-lookup"><span data-stu-id="4e549-660">Compound key pattern</span></span>
<span data-ttu-id="4e549-661">Použití složené **RowKey** hodnoty, aby klient k vyhledání souvisejících dat pomocí dotazu jediný bod.</span><span class="sxs-lookup"><span data-stu-id="4e549-661">Use compound **RowKey** values to enable a client to lookup related data with a single point query.</span></span>  

#### <a name="context-and-problem"></a><span data-ttu-id="4e549-662">Kontext a problém</span><span class="sxs-lookup"><span data-stu-id="4e549-662">Context and problem</span></span>
<span data-ttu-id="4e549-663">V relační databázi je poměrně přirozené lze pomocí spojení v dotazech vrátit související částí dat klientovi v jednom dotazu.</span><span class="sxs-lookup"><span data-stu-id="4e549-663">In a relational database, it is quite natural to use joins in queries to return related pieces of data to the client in a single query.</span></span> <span data-ttu-id="4e549-664">Id zaměstnance můžete například použít k vyhledání seznam entit v relaci, které obsahují výkonu a zkontrolujte data pro zaměstnance.</span><span class="sxs-lookup"><span data-stu-id="4e549-664">For example, you might use the employee id to look up a list of related entities that contain performance and review data for that employee.</span></span>  

<span data-ttu-id="4e549-665">Předpokládejme, že zaměstnanec entity ukládáte ve službě Table pomocí následující strukturu:</span><span class="sxs-lookup"><span data-stu-id="4e549-665">Assume you are storing employee entities in the Table service using the following structure:</span></span>  

![][18]

<span data-ttu-id="4e549-666">Musíte taky k uložení historická data týkající se recenze a výkon pro každý rok, které zaměstnanec odpracoval pro vaši organizaci a musíte být schopni přistupovat k těmto informacím o rok.</span><span class="sxs-lookup"><span data-stu-id="4e549-666">You also need to store historical data relating to reviews and performance for each year the employee has worked for your organization and you need to be able to access this information by year.</span></span> <span data-ttu-id="4e549-667">Jednou z možností je vytvořit jiné tabulky, která ukládá entity s následující strukturou:</span><span class="sxs-lookup"><span data-stu-id="4e549-667">One option is to create another table that stores entities with the following structure:</span></span>  

![][19]

<span data-ttu-id="4e549-668">Všimněte si, že s tímto přístupem můžete rozhodnout pro duplicitní některé informace (například křestní jméno a příjmení) v nové entity, která má vám umožní načíst dat pomocí jedné žádosti.</span><span class="sxs-lookup"><span data-stu-id="4e549-668">Notice that with this approach you may decide to duplicate some information (such as first name and last name) in the new entity to enable you to retrieve your data with a single request.</span></span> <span data-ttu-id="4e549-669">Nelze však udržovat silnou konzistenci, protože EGT nejde použít k aktualizaci dvě entity atomicky.</span><span class="sxs-lookup"><span data-stu-id="4e549-669">However, you cannot maintain strong consistency because you cannot use an EGT to update the two entities atomically.</span></span>  

#### <a name="solution"></a><span data-ttu-id="4e549-670">Řešení</span><span class="sxs-lookup"><span data-stu-id="4e549-670">Solution</span></span>
<span data-ttu-id="4e549-671">Uložte nový typ entity v původní tabulky pomocí entity s následující strukturou:</span><span class="sxs-lookup"><span data-stu-id="4e549-671">Store a new entity type in your original table using entities with the following structure:</span></span>  

![][20]

<span data-ttu-id="4e549-672">Všimněte si jak **RowKey** je nyní složený klíč skládá z id zaměstnance a rok zkontrolujte data, která vám umožňuje načíst zaměstnance výkonu a zkontrolujte data pomocí jedné žádosti pro jednu entitu.</span><span class="sxs-lookup"><span data-stu-id="4e549-672">Notice how the **RowKey** is now a compound key made up of the employee id and the year of the review data that enables you to retrieve the employee's performance and review data with a single request for a single entity.</span></span>  

<span data-ttu-id="4e549-673">Následující příklad popisuje, jak můžete načíst všechna data revize pro konkrétního zaměstnance (například zaměstnanci 000123 z oddělení prodeje):</span><span class="sxs-lookup"><span data-stu-id="4e549-673">The following example outlines how you can retrieve all the review data for a particular employee (such as employee 000123 in the Sales department):</span></span>  

<span data-ttu-id="4e549-674">$filter = (PartitionKey eq 'Prodej') a (RowKey ge 'empid_000123') a (RowKey lt 'empid_000124') & $select = RowKey, Manager hodnocení, sdílené hodnocení a komentáře</span><span class="sxs-lookup"><span data-stu-id="4e549-674">$filter=(PartitionKey eq 'Sales') and (RowKey ge 'empid_000123') and (RowKey lt 'empid_000124')&$select=RowKey,Manager Rating,Peer Rating,Comments</span></span>  

#### <a name="issues-and-considerations"></a><span data-ttu-id="4e549-675">Problémy a důležité informace</span><span class="sxs-lookup"><span data-stu-id="4e549-675">Issues and considerations</span></span>
<span data-ttu-id="4e549-676">Při rozhodování o tom, jak implementovat tento vzor, zvažte následující body:</span><span class="sxs-lookup"><span data-stu-id="4e549-676">Consider the following points when deciding how to implement this pattern:</span></span>  

* <span data-ttu-id="4e549-677">Měli byste použít vhodný oddělovací znak, který umožňuje snadno rozložit **RowKey** hodnota: například **000123_2012**.</span><span class="sxs-lookup"><span data-stu-id="4e549-677">You should use a suitable separator character that makes it easy to parse the **RowKey** value: for example, **000123_2012**.</span></span>  
* <span data-ttu-id="4e549-678">Tuto entitu se také ukládání do stejného oddílu jako jiné entity, které obsahují data v relaci pro stejné zaměstnance, což znamená, že EGTs můžete použít k udržování silnou konzistenci.</span><span class="sxs-lookup"><span data-stu-id="4e549-678">You are also storing this entity in the same partition as other entities that contain related data for the same employee, which means you can use EGTs to maintain strong consistency.</span></span>
* <span data-ttu-id="4e549-679">Měli byste zvážit, jak často bude dotaz na data k určení, zda je tento vzor vhodná.</span><span class="sxs-lookup"><span data-stu-id="4e549-679">You should consider how frequently you will query the data to determine whether this pattern is appropriate.</span></span>  <span data-ttu-id="4e549-680">Například pokud bude mít přístup zřídka zkontrolujte data a data hlavní zaměstnance často byste měli mít je jako samostatné entity.</span><span class="sxs-lookup"><span data-stu-id="4e549-680">For example, if you will access the review data infrequently and the main employee data often you should keep them as separate entities.</span></span>  

#### <a name="when-to-use-this-pattern"></a><span data-ttu-id="4e549-681">Kdy použít tento vzor</span><span class="sxs-lookup"><span data-stu-id="4e549-681">When to use this pattern</span></span>
<span data-ttu-id="4e549-682">Tento vzor použijte, pokud je třeba uložit jeden nebo více související entity dotazu často.</span><span class="sxs-lookup"><span data-stu-id="4e549-682">Use this pattern when you need to store one or more related entities that you query frequently.</span></span>  

#### <a name="related-patterns-and-guidance"></a><span data-ttu-id="4e549-683">Související vzory a pokyny</span><span class="sxs-lookup"><span data-stu-id="4e549-683">Related patterns and guidance</span></span>
<span data-ttu-id="4e549-684">Následující pokyny a vzory může být také relevantní při implementaci tohoto vzoru:</span><span class="sxs-lookup"><span data-stu-id="4e549-684">The following patterns and guidance may also be relevant when implementing this pattern:</span></span>  

* [<span data-ttu-id="4e549-685">Transakce skupiny entity</span><span class="sxs-lookup"><span data-stu-id="4e549-685">Entity Group Transactions</span></span>](#entity-group-transactions)  
* [<span data-ttu-id="4e549-686">Práce s typy heterogenní entit</span><span class="sxs-lookup"><span data-stu-id="4e549-686">Working with heterogeneous entity types</span></span>](#working-with-heterogeneous-entity-types)  
* [<span data-ttu-id="4e549-687">Nakonec byl konzistentní transakce vzor</span><span class="sxs-lookup"><span data-stu-id="4e549-687">Eventually consistent transactions pattern</span></span>](#eventually-consistent-transactions-pattern)  

### <a name="log-tail-pattern"></a><span data-ttu-id="4e549-688">Vzor protokolu poškozené databáze</span><span class="sxs-lookup"><span data-stu-id="4e549-688">Log tail pattern</span></span>
<span data-ttu-id="4e549-689">Načtení  *n*  entity naposledy přidané do oddílu pomocí **RowKey** hodnotu, která seřadí zpětné datum a čas pořadí.</span><span class="sxs-lookup"><span data-stu-id="4e549-689">Retrieve the *n* entities most recently added to a partition by using a **RowKey** value that sorts in reverse date and time order.</span></span>  

#### <a name="context-and-problem"></a><span data-ttu-id="4e549-690">Kontext a problém</span><span class="sxs-lookup"><span data-stu-id="4e549-690">Context and problem</span></span>
<span data-ttu-id="4e549-691">Běžným požadavkem je možné načíst nedávno vytvořených entity, například nejnovější deset výdaje odeslaný zaměstnanec deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="4e549-691">A common requirement is be able to retrieve the most recently created entities, for example the ten most recent expense claims submitted by an employee.</span></span> <span data-ttu-id="4e549-692">Tabulka dotazuje podporu **$top** dotaz operace vrátit první  *n*  entit ze sady: neprobíhá žádná operace ekvivalentní dotaz vrátit poslední n entity v sadě.</span><span class="sxs-lookup"><span data-stu-id="4e549-692">Table queries support a **$top** query operation to return the first *n* entities from a set: there is no equivalent query operation to return the last n entities in a set.</span></span>  

#### <a name="solution"></a><span data-ttu-id="4e549-693">Řešení</span><span class="sxs-lookup"><span data-stu-id="4e549-693">Solution</span></span>
<span data-ttu-id="4e549-694">Ukládání entit, použití **RowKey** že přirozeně seřadí v pořadí zpětné datum a čas pomocí tak poslední položka je vždy první z nich v tabulce.</span><span class="sxs-lookup"><span data-stu-id="4e549-694">Store the entities using a **RowKey** that naturally sorts in reverse date/time order by using so the most recent entry is always the first one in the table.</span></span>  

<span data-ttu-id="4e549-695">Abyste mohli vyhledat deset nejnovější deklarace výdajů odeslaný zaměstnanec, například můžete použít reverzní značek hodnotou odvozenou od aktuální datum a čas.</span><span class="sxs-lookup"><span data-stu-id="4e549-695">For example, to be able to retrieve the ten most recent expense claims submitted by an employee, you can use a reverse tick value derived from the current date/time.</span></span> <span data-ttu-id="4e549-696">Jeden způsob, jak vytvořit vhodný hodnota "obrácený rysky" pro znázorňuje následující ukázka kódu C# **RowKey** , seřadí z nejnovější k nejstarší:</span><span class="sxs-lookup"><span data-stu-id="4e549-696">The following C# code sample shows one way to create a suitable "inverted ticks" value for a **RowKey** that sorts from the most recent to the oldest:</span></span>  

`string invertedTicks = string.Format("{0:D19}", DateTime.MaxValue.Ticks - DateTime.UtcNow.Ticks);`  

<span data-ttu-id="4e549-697">Můžete získat zpět na hodnotu data a času pomocí následujícího kódu:</span><span class="sxs-lookup"><span data-stu-id="4e549-697">You can get back to the date time value using the following code:</span></span>  

`DateTime dt = new DateTime(DateTime.MaxValue.Ticks - Int64.Parse(invertedTicks));`  

<span data-ttu-id="4e549-698">Dotaz tabulky vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="4e549-698">The table query looks like this:</span></span>  

`https://myaccount.table.core.windows.net/EmployeeExpense(PartitionKey='empid')?$top=10`  

#### <a name="issues-and-considerations"></a><span data-ttu-id="4e549-699">Problémy a důležité informace</span><span class="sxs-lookup"><span data-stu-id="4e549-699">Issues and considerations</span></span>
<span data-ttu-id="4e549-700">Při rozhodování o tom, jak implementovat tento vzor, zvažte následující body:</span><span class="sxs-lookup"><span data-stu-id="4e549-700">Consider the following points when deciding how to implement this pattern:</span></span>  

* <span data-ttu-id="4e549-701">Musí odsadí zpětné značek hodnotu s úvodní nuly zajistit, že řetězcovou hodnotu seřadí podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="4e549-701">You must pad the reverse tick value with leading zeroes to ensure the string value sorts as expected.</span></span>  
* <span data-ttu-id="4e549-702">Musíte být vědomi škálovatelnost cílů na úrovni oddílu.</span><span class="sxs-lookup"><span data-stu-id="4e549-702">You must be aware of the scalability targets at the level of a partition.</span></span> <span data-ttu-id="4e549-703">Dávejte pozor, vytváření oddílů aktivního bodu.</span><span class="sxs-lookup"><span data-stu-id="4e549-703">Be careful not create hot spot partitions.</span></span>  

#### <a name="when-to-use-this-pattern"></a><span data-ttu-id="4e549-704">Kdy použít tento vzor</span><span class="sxs-lookup"><span data-stu-id="4e549-704">When to use this pattern</span></span>
<span data-ttu-id="4e549-705">Tento vzor použijte, pokud budete potřebovat pro přístup k entity v pořadí zpětné datum a čas nebo když potřebujete přístup k nedávno přidané entity.</span><span class="sxs-lookup"><span data-stu-id="4e549-705">Use this pattern when you need to access entities in reverse date/time order or when you need to access the most recently added entities.</span></span>  

#### <a name="related-patterns-and-guidance"></a><span data-ttu-id="4e549-706">Související vzory a pokyny</span><span class="sxs-lookup"><span data-stu-id="4e549-706">Related patterns and guidance</span></span>
<span data-ttu-id="4e549-707">Následující pokyny a vzory může být také relevantní při implementaci tohoto vzoru:</span><span class="sxs-lookup"><span data-stu-id="4e549-707">The following patterns and guidance may also be relevant when implementing this pattern:</span></span>  

* [<span data-ttu-id="4e549-708">Předřadit / append proti vzor</span><span class="sxs-lookup"><span data-stu-id="4e549-708">Prepend / append anti-pattern</span></span>](#prepend-append-anti-pattern)  
* [<span data-ttu-id="4e549-709">Načtení entit</span><span class="sxs-lookup"><span data-stu-id="4e549-709">Retrieving entities</span></span>](#retrieving-entities)  

### <a name="high-volume-delete-pattern"></a><span data-ttu-id="4e549-710">Vzor velkému odstranění</span><span class="sxs-lookup"><span data-stu-id="4e549-710">High volume delete pattern</span></span>
<span data-ttu-id="4e549-711">Povolit odstranění k velkému počtu entity uložením všechny entity pro souběžné odstranění vlastní samostatné tabulky; Odstranění entity odstraněním v tabulce.</span><span class="sxs-lookup"><span data-stu-id="4e549-711">Enable the deletion of a high volume of entities by storing all the entities for simultaneous deletion in their own separate table; you delete the entities by deleting the table.</span></span>  

#### <a name="context-and-problem"></a><span data-ttu-id="4e549-712">Kontext a problém</span><span class="sxs-lookup"><span data-stu-id="4e549-712">Context and problem</span></span>
<span data-ttu-id="4e549-713">Mnoho aplikací odstranit stará data, která už musí být k dispozici pro klientské aplikace, nebo že aplikace má archivovat na jiné úložné médium.</span><span class="sxs-lookup"><span data-stu-id="4e549-713">Many applications delete old data which no longer needs to be available to a client application, or that the application has archived to another storage medium.</span></span> <span data-ttu-id="4e549-714">Obvykle identifikovat taková data data: například máte požadavek odstranit záznamy všech žádostí o přihlášení, které jsou starší než 60 dní.</span><span class="sxs-lookup"><span data-stu-id="4e549-714">You typically identify such data by a date: for example, you have a requirement to delete records of all login requests that are more than 60 days old.</span></span>  

<span data-ttu-id="4e549-715">Jeden návrhu možné je použít datum a čas přihlášení na požadavek na **RowKey**:</span><span class="sxs-lookup"><span data-stu-id="4e549-715">One possible design is to use the date and time of the login request in the **RowKey**:</span></span>  

![][21]

<span data-ttu-id="4e549-716">Tento přístup zabraňuje hotspotům oddílu, protože aplikace můžete vložit a odstranit entity přihlášení pro každého uživatele v samostatném oddílu.</span><span class="sxs-lookup"><span data-stu-id="4e549-716">This approach avoids partition hotspots because the application can insert and delete login entities for each user in a separate partition.</span></span> <span data-ttu-id="4e549-717">Tento postup však může být nákladná a časově náročné Pokud máte velký počet entit, protože nejdřív je potřeba provést prohledávání tabulky za účelem zjištění všech entit odstranit a pak je nutné odstranit každé staré entity.</span><span class="sxs-lookup"><span data-stu-id="4e549-717">However, this approach may be costly and time consuming if you have a large number of entities because first you need to perform a table scan in order to identify all the entities to delete, and then you must delete each old entity.</span></span> <span data-ttu-id="4e549-718">Všimněte si, že můžete snížit počet zpátečních cest k serveru potřeba dávkování více žádosti o odstranění do EGTs odstraňte starý entity.</span><span class="sxs-lookup"><span data-stu-id="4e549-718">Note that you can reduce the number of round trips to the server required to delete the old entities by batching multiple delete requests into EGTs.</span></span>  

#### <a name="solution"></a><span data-ttu-id="4e549-719">Řešení</span><span class="sxs-lookup"><span data-stu-id="4e549-719">Solution</span></span>
<span data-ttu-id="4e549-720">Do samostatné tabulky použijte pro každý den pokusů o přihlášení.</span><span class="sxs-lookup"><span data-stu-id="4e549-720">Use a separate table for each day of login attempts.</span></span> <span data-ttu-id="4e549-721">Výše uvedený návrh entit můžete vyhnout hotspotům při vkládání entity a odstranění starší entity je nyní prostě odstranění jedna tabulka každý den (jedno úložiště operace) namísto hledání a odstraňování stovky a tisíce jednotlivých přihlášení entity každý den.</span><span class="sxs-lookup"><span data-stu-id="4e549-721">You can use the entity design above to avoid hotspots when you are inserting entities, and deleting old entities is now simply a question of deleting one table every day (a single storage operation) instead of finding and deleting hundreds and thousands of individual login entities every day.</span></span>  

#### <a name="issues-and-considerations"></a><span data-ttu-id="4e549-722">Problémy a důležité informace</span><span class="sxs-lookup"><span data-stu-id="4e549-722">Issues and considerations</span></span>
<span data-ttu-id="4e549-723">Při rozhodování o tom, jak implementovat tento vzor, zvažte následující body:</span><span class="sxs-lookup"><span data-stu-id="4e549-723">Consider the following points when deciding how to implement this pattern:</span></span>  

* <span data-ttu-id="4e549-724">Podporuje váš návrh další způsoby, které vaše aplikace bude používat data, jako je například vyhledávání konkrétních entit, propojení s jinými data nebo informace o generování agregační?</span><span class="sxs-lookup"><span data-stu-id="4e549-724">Does your design support other ways your application will use the data such as looking up specific entities, linking with other data, or generating aggregate information?</span></span>  
* <span data-ttu-id="4e549-725">Návrhu vyhnout aktivní body při vkládání nové entity</span><span class="sxs-lookup"><span data-stu-id="4e549-725">Does your design avoid hot spots when you are inserting new entities?</span></span>  
* <span data-ttu-id="4e549-726">Pokud chcete po odstranění ho znovu použít stejný název tabulky očekávají, že ke zpoždění.</span><span class="sxs-lookup"><span data-stu-id="4e549-726">Expect a delay if you want to reuse the same table name after deleting it.</span></span> <span data-ttu-id="4e549-727">Je lepší vždy nutné použít názvy jedinečné tabulky.</span><span class="sxs-lookup"><span data-stu-id="4e549-727">It's better to always use unique table names.</span></span>  
* <span data-ttu-id="4e549-728">Očekávají, že některé omezení při prvním použití nové tabulky při služby Table zjišťuje přístupové vzorce a distribuuje oddíly mezi uzly.</span><span class="sxs-lookup"><span data-stu-id="4e549-728">Expect some throttling when you first use a new table while the Table service learns the access patterns and distributes the partitions across nodes.</span></span> <span data-ttu-id="4e549-729">Měli byste zvážit, jak často budete muset vytvořit nové tabulky.</span><span class="sxs-lookup"><span data-stu-id="4e549-729">You should consider how frequently you need to create new tables.</span></span>  

#### <a name="when-to-use-this-pattern"></a><span data-ttu-id="4e549-730">Kdy použít tento vzor</span><span class="sxs-lookup"><span data-stu-id="4e549-730">When to use this pattern</span></span>
<span data-ttu-id="4e549-731">Tento vzor použijte, když máte k velkému počtu entit, které je nutné odstranit ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="4e549-731">Use this pattern when you have a high volume of entities that you must delete at the same time.</span></span>  

#### <a name="related-patterns-and-guidance"></a><span data-ttu-id="4e549-732">Související vzory a pokyny</span><span class="sxs-lookup"><span data-stu-id="4e549-732">Related patterns and guidance</span></span>
<span data-ttu-id="4e549-733">Následující pokyny a vzory může být také relevantní při implementaci tohoto vzoru:</span><span class="sxs-lookup"><span data-stu-id="4e549-733">The following patterns and guidance may also be relevant when implementing this pattern:</span></span>  

* [<span data-ttu-id="4e549-734">Transakce skupiny entity</span><span class="sxs-lookup"><span data-stu-id="4e549-734">Entity Group Transactions</span></span>](#entity-group-transactions)
* [<span data-ttu-id="4e549-735">Úprava entity</span><span class="sxs-lookup"><span data-stu-id="4e549-735">Modifying entities</span></span>](#modifying-entities)  

### <a name="data-series-pattern"></a><span data-ttu-id="4e549-736">Řada vzorek dat</span><span class="sxs-lookup"><span data-stu-id="4e549-736">Data series pattern</span></span>
<span data-ttu-id="4e549-737">Dokončení datové řady úložiště v jedné entity, chcete-li minimalizovat počet požadavků, které provedete.</span><span class="sxs-lookup"><span data-stu-id="4e549-737">Store complete data series in a single entity to minimize the number of requests you make.</span></span>  

#### <a name="context-and-problem"></a><span data-ttu-id="4e549-738">Kontext a problém</span><span class="sxs-lookup"><span data-stu-id="4e549-738">Context and problem</span></span>
<span data-ttu-id="4e549-739">Obvyklým scénářem je pro aplikace pro uložení řadu data, která je obvykle potřeba načíst všechny najednou.</span><span class="sxs-lookup"><span data-stu-id="4e549-739">A common scenario is for an application to store a series of data that it typically needs to retrieve all at once.</span></span> <span data-ttu-id="4e549-740">Aplikace může například záznam kolik zasílání Rychlých zpráv každý zaměstnanec odešle každou hodinu a pak tyto informace použít k vykreslení kolik zpráv každý uživatel, odešlou přes předchozích 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="4e549-740">For example, your application might record how many IM messages each employee sends every hour, and then use this information to plot how many messages each user sent over the preceding 24 hours.</span></span> <span data-ttu-id="4e549-741">Pro uložení 24 entity pro každý zaměstnanec může být jeden návrhu:</span><span class="sxs-lookup"><span data-stu-id="4e549-741">One design might be to store 24 entities for each employee:</span></span>  

![][22]

<span data-ttu-id="4e549-742">V tomto návrhu můžete snadno vyhledat a aktualizovat entity, která má aktualizace pro každý zaměstnanec vždy, když aplikace potřebuje k aktualizaci hodnota počtu zpráv.</span><span class="sxs-lookup"><span data-stu-id="4e549-742">With this design, you can easily locate and update the entity to update for each employee whenever the application needs to update the message count value.</span></span> <span data-ttu-id="4e549-743">K načtení informací k vykreslení grafu aktivity předchozích 24 hodin, ale musí získat 24 entity.</span><span class="sxs-lookup"><span data-stu-id="4e549-743">However, to retrieve the information to plot a chart of the activity for the preceding 24 hours, you must retrieve 24 entities.</span></span>  

#### <a name="solution"></a><span data-ttu-id="4e549-744">Řešení</span><span class="sxs-lookup"><span data-stu-id="4e549-744">Solution</span></span>
<span data-ttu-id="4e549-745">Použijte následující návrh s samostatné vlastností pro uložení počet zpráv pro každou hodinu:</span><span class="sxs-lookup"><span data-stu-id="4e549-745">Use the following design with a separate property to store the message count for each hour:</span></span>  

![][23]

<span data-ttu-id="4e549-746">V tomto návrhu můžete aktualizovat počet zpráv pro zaměstnance pro zadané hodiny operace sloučení.</span><span class="sxs-lookup"><span data-stu-id="4e549-746">With this design, you can use a merge operation to update the message count for an employee for a specific hour.</span></span> <span data-ttu-id="4e549-747">Teď můžete načíst všechny informace, které potřebujete k vykreslení grafu pomocí žádosti pro jednu entitu.</span><span class="sxs-lookup"><span data-stu-id="4e549-747">Now, you can retrieve all the information you need to plot the chart using a request for a single entity.</span></span>  

#### <a name="issues-and-considerations"></a><span data-ttu-id="4e549-748">Problémy a důležité informace</span><span class="sxs-lookup"><span data-stu-id="4e549-748">Issues and considerations</span></span>
<span data-ttu-id="4e549-749">Při rozhodování o tom, jak implementovat tento vzor, zvažte následující body:</span><span class="sxs-lookup"><span data-stu-id="4e549-749">Consider the following points when deciding how to implement this pattern:</span></span>  

* <span data-ttu-id="4e549-750">Pokud dokončení datové řady nevejde do jedné entity (entita může mít až 252 vlastností), použijte alternativní úložišti například objekt blob.</span><span class="sxs-lookup"><span data-stu-id="4e549-750">If your complete data series does not fit into a single entity (an entity can have up to 252 properties), use an alternative data store such as a blob.</span></span>  
* <span data-ttu-id="4e549-751">Pokud máte víc klientů současně aktualizaci entity, budete muset použít **značka ETag** implementovat optimistickou metodu souběžného.</span><span class="sxs-lookup"><span data-stu-id="4e549-751">If you have multiple clients updating an entity simultaneously, you will need to use the **ETag** to implement optimistic concurrency.</span></span> <span data-ttu-id="4e549-752">Pokud máte mnoho klientů, můžete se setkat vysoké kolizí.</span><span class="sxs-lookup"><span data-stu-id="4e549-752">If you have many clients, you may experience high contention.</span></span>  

#### <a name="when-to-use-this-pattern"></a><span data-ttu-id="4e549-753">Kdy použít tento vzor</span><span class="sxs-lookup"><span data-stu-id="4e549-753">When to use this pattern</span></span>
<span data-ttu-id="4e549-754">Tento vzor použijte, pokud je potřeba aktualizovat a načíst data řady přidružené jednotlivých entit.</span><span class="sxs-lookup"><span data-stu-id="4e549-754">Use this pattern when you need to update and retrieve a data series associated with an individual entity.</span></span>  

#### <a name="related-patterns-and-guidance"></a><span data-ttu-id="4e549-755">Související vzory a pokyny</span><span class="sxs-lookup"><span data-stu-id="4e549-755">Related patterns and guidance</span></span>
<span data-ttu-id="4e549-756">Následující pokyny a vzory může být také relevantní při implementaci tohoto vzoru:</span><span class="sxs-lookup"><span data-stu-id="4e549-756">The following patterns and guidance may also be relevant when implementing this pattern:</span></span>  

* [<span data-ttu-id="4e549-757">Vzor velkých entit</span><span class="sxs-lookup"><span data-stu-id="4e549-757">Large entities pattern</span></span>](#large-entities-pattern)  
* [<span data-ttu-id="4e549-758">Sloučení nebo nahradit</span><span class="sxs-lookup"><span data-stu-id="4e549-758">Merge or replace</span></span>](#merge-or-replace)  
* <span data-ttu-id="4e549-759">[Nakonec byl konzistentní transakce vzor](#eventually-consistent-transactions-pattern) (Pokud ukládáte datové řady do objektu BLOB)</span><span class="sxs-lookup"><span data-stu-id="4e549-759">[Eventually consistent transactions pattern](#eventually-consistent-transactions-pattern) (if you are storing the data series in a blob)</span></span>  

### <a name="wide-entities-pattern"></a><span data-ttu-id="4e549-760">Vzor široké entity</span><span class="sxs-lookup"><span data-stu-id="4e549-760">Wide entities pattern</span></span>
<span data-ttu-id="4e549-761">Slouží k ukládání logických entit s více než 252 vlastností více fyzických entit.</span><span class="sxs-lookup"><span data-stu-id="4e549-761">Use multiple physical entities to store logical entities with more than 252 properties.</span></span>  

#### <a name="context-and-problem"></a><span data-ttu-id="4e549-762">Kontext a problém</span><span class="sxs-lookup"><span data-stu-id="4e549-762">Context and problem</span></span>
<span data-ttu-id="4e549-763">Jednotlivých entit může mít více než 252 vlastností (s výjimkou vlastnosti povinné systému) a nelze uložit více než 1 MB dat celkem.</span><span class="sxs-lookup"><span data-stu-id="4e549-763">An individual entity can have no more than 252 properties (excluding the mandatory system properties) and cannot store more than 1 MB of data in total.</span></span> <span data-ttu-id="4e549-764">V relační databázi by obvykle získáte zaokrouhlí žádné omezení velikosti řádku přidáním nové tabulky a vynucování relace 1: 1 mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="4e549-764">In a relational database, you would typically get round any limits on the size of a row by adding a new table and enforcing a 1-to-1 relationship between them.</span></span>  

#### <a name="solution"></a><span data-ttu-id="4e549-765">Řešení</span><span class="sxs-lookup"><span data-stu-id="4e549-765">Solution</span></span>
<span data-ttu-id="4e549-766">Pomocí služby Table, můžete uložit více entit představují objekt jeden velký podnik s více než 252 vlastností.</span><span class="sxs-lookup"><span data-stu-id="4e549-766">Using the Table service, you can store multiple entities to represent a single large business object with more than 252 properties.</span></span> <span data-ttu-id="4e549-767">Například pokud chcete uložit počet Rychlých zpráv odeslaných každý zaměstnanec za posledních 365 dnů, můžete použít následující návrh, který používá dvě entity s různými schématy:</span><span class="sxs-lookup"><span data-stu-id="4e549-767">For example, if you want to store a count of the number of IM messages sent by each employee for the last 365 days, you could use the following design that uses two entities with different schemas:</span></span>  

![][24]

<span data-ttu-id="4e549-768">Pokud potřebujete provést změnu, která vyžaduje aktualizaci obě entity k jejich synchronizovány mezi sebou můžete použít EGT.</span><span class="sxs-lookup"><span data-stu-id="4e549-768">If you need to make a change that requires updating both entities to keep them synchronized with each other you can use an EGT.</span></span> <span data-ttu-id="4e549-769">Jinak operace sloučení jednoho můžete aktualizovat počet zpráv pro určitý den.</span><span class="sxs-lookup"><span data-stu-id="4e549-769">Otherwise, you can use a single merge operation to update the message count for a specific day.</span></span> <span data-ttu-id="4e549-770">Načíst všechna data pro jednotlivé zaměstnance musí načíst obě entity, které můžete provést dva efektivní požadavky, které používají oba **PartitionKey** a **RowKey** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4e549-770">To retrieve all the data for an individual employee you must retrieve both entities, which you can do with two efficient requests that use both a **PartitionKey** and a **RowKey** value.</span></span>  

#### <a name="issues-and-considerations"></a><span data-ttu-id="4e549-771">Problémy a důležité informace</span><span class="sxs-lookup"><span data-stu-id="4e549-771">Issues and considerations</span></span>
<span data-ttu-id="4e549-772">Při rozhodování o tom, jak implementovat tento vzor, zvažte následující body:</span><span class="sxs-lookup"><span data-stu-id="4e549-772">Consider the following points when deciding how to implement this pattern:</span></span>  

* <span data-ttu-id="4e549-773">Načítání dokončení logická entita zahrnuje alespoň dva transakce úložiště: jeden pro načtení jednotlivých fyzická entita.</span><span class="sxs-lookup"><span data-stu-id="4e549-773">Retrieving a complete logical entity involves at least two storage transactions: one to retrieve each physical entity.</span></span>  

#### <a name="when-to-use-this-pattern"></a><span data-ttu-id="4e549-774">Kdy použít tento vzor</span><span class="sxs-lookup"><span data-stu-id="4e549-774">When to use this pattern</span></span>
<span data-ttu-id="4e549-775">Použijte tento vzor, kdy je nutné uložit entity, jejichž velikost nebo počet vlastností přesahuje omezení pro jednotlivé entity ve službě Table.</span><span class="sxs-lookup"><span data-stu-id="4e549-775">Use this pattern when  need to store entities whose size or number of properties exceeds the limits for an individual entity in the Table service.</span></span>  

#### <a name="related-patterns-and-guidance"></a><span data-ttu-id="4e549-776">Související vzory a pokyny</span><span class="sxs-lookup"><span data-stu-id="4e549-776">Related patterns and guidance</span></span>
<span data-ttu-id="4e549-777">Následující pokyny a vzory může být také relevantní při implementaci tohoto vzoru:</span><span class="sxs-lookup"><span data-stu-id="4e549-777">The following patterns and guidance may also be relevant when implementing this pattern:</span></span>  

* [<span data-ttu-id="4e549-778">Transakce skupiny entity</span><span class="sxs-lookup"><span data-stu-id="4e549-778">Entity Group Transactions</span></span>](#entity-group-transactions)
* [<span data-ttu-id="4e549-779">Sloučení nebo nahradit</span><span class="sxs-lookup"><span data-stu-id="4e549-779">Merge or replace</span></span>](#merge-or-replace)

### <a name="large-entities-pattern"></a><span data-ttu-id="4e549-780">Vzor velkých entit</span><span class="sxs-lookup"><span data-stu-id="4e549-780">Large entities pattern</span></span>
<span data-ttu-id="4e549-781">Použijte úložiště objektů blob k ukládání velkých vlastnost hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4e549-781">Use blob storage to store large property values.</span></span>  

#### <a name="context-and-problem"></a><span data-ttu-id="4e549-782">Kontext a problém</span><span class="sxs-lookup"><span data-stu-id="4e549-782">Context and problem</span></span>
<span data-ttu-id="4e549-783">Jednotlivé entity Nejde uložit více než 1 MB dat celkem.</span><span class="sxs-lookup"><span data-stu-id="4e549-783">An individual entity cannot store more than 1 MB of data in total.</span></span> <span data-ttu-id="4e549-784">Pokud jeden nebo několik vlastnosti ukládá hodnoty, které způsobí celková velikost vaší entity, která má být vyšší než tato hodnota, nemůžete uložit celý entity služby Table.</span><span class="sxs-lookup"><span data-stu-id="4e549-784">If one or several of your properties store values that cause the total size of your entity to exceed this value, you cannot store the entire entity in the Table service.</span></span>  

#### <a name="solution"></a><span data-ttu-id="4e549-785">Řešení</span><span class="sxs-lookup"><span data-stu-id="4e549-785">Solution</span></span>
<span data-ttu-id="4e549-786">Pokud vaší entity překračuje 1 MB velikost, protože jeden nebo více vlastností obsahovat velké množství dat, můžete ukládat data ve službě Blob a potom uložte adresu objektu blob ve vlastnosti v entitě.</span><span class="sxs-lookup"><span data-stu-id="4e549-786">If your entity exceeds 1 MB in size because one or more properties contain a large amount of data, you can store data in the Blob service and then store the address of the blob in a property in the entity.</span></span> <span data-ttu-id="4e549-787">Například můžete ukládat fotografie zaměstnanec v úložišti objektů blob a uložte odkaz na fotografii v **fotografií** vlastnosti vaší entity zaměstnanců:</span><span class="sxs-lookup"><span data-stu-id="4e549-787">For example, you can store the photo of an employee in blob storage and store a link to the photo in the **Photo** property of your employee entity:</span></span>  

![][25]

#### <a name="issues-and-considerations"></a><span data-ttu-id="4e549-788">Problémy a důležité informace</span><span class="sxs-lookup"><span data-stu-id="4e549-788">Issues and considerations</span></span>
<span data-ttu-id="4e549-789">Při rozhodování o tom, jak implementovat tento vzor, zvažte následující body:</span><span class="sxs-lookup"><span data-stu-id="4e549-789">Consider the following points when deciding how to implement this pattern:</span></span>  

* <span data-ttu-id="4e549-790">K zajištění konzistence typu případné mezi entity ve službě Table a data ve službě Blob, použít [nakonec byl konzistentní transakce vzor](#eventually-consistent-transactions-pattern) zachování vaší entity.</span><span class="sxs-lookup"><span data-stu-id="4e549-790">To maintain eventual consistency between the entity in the Table service and the data in the Blob service, use the [Eventually consistent transactions pattern](#eventually-consistent-transactions-pattern) to maintain your entities.</span></span>
* <span data-ttu-id="4e549-791">Načítání úplnou entitu zahrnuje alespoň dva transakce úložiště: jeden pro načtení entity a jeden pro načtení dat objektů blob.</span><span class="sxs-lookup"><span data-stu-id="4e549-791">Retrieving a complete entity involves at least two storage transactions: one to retrieve the entity and one to retrieve the blob data.</span></span>  

#### <a name="when-to-use-this-pattern"></a><span data-ttu-id="4e549-792">Kdy použít tento vzor</span><span class="sxs-lookup"><span data-stu-id="4e549-792">When to use this pattern</span></span>
<span data-ttu-id="4e549-793">Pokud budete potřebovat k ukládání entit, jehož velikost překračuje omezení pro jednotlivé entity ve službě Table, použijte tento vzor.</span><span class="sxs-lookup"><span data-stu-id="4e549-793">Use this pattern when you need to store entities whose size exceeds the limits for an individual entity in the Table service.</span></span>  

#### <a name="related-patterns-and-guidance"></a><span data-ttu-id="4e549-794">Související vzory a pokyny</span><span class="sxs-lookup"><span data-stu-id="4e549-794">Related patterns and guidance</span></span>
<span data-ttu-id="4e549-795">Následující pokyny a vzory může být také relevantní při implementaci tohoto vzoru:</span><span class="sxs-lookup"><span data-stu-id="4e549-795">The following patterns and guidance may also be relevant when implementing this pattern:</span></span>  

* [<span data-ttu-id="4e549-796">Nakonec byl konzistentní transakce vzor</span><span class="sxs-lookup"><span data-stu-id="4e549-796">Eventually consistent transactions pattern</span></span>](#eventually-consistent-transactions-pattern)  
* [<span data-ttu-id="4e549-797">Vzor široké entity</span><span class="sxs-lookup"><span data-stu-id="4e549-797">Wide entities pattern</span></span>](#wide-entities-pattern)

<a name="prepend-append-anti-pattern"></a>

### <a name="prependappend-anti-pattern"></a><span data-ttu-id="4e549-798">Předřazení připojit proti vzor</span><span class="sxs-lookup"><span data-stu-id="4e549-798">Prepend/append anti-pattern</span></span>
<span data-ttu-id="4e549-799">Zvýšení škálovatelnosti, když máte k velkému počtu vloží tak, že se vložení napříč více oddílů.</span><span class="sxs-lookup"><span data-stu-id="4e549-799">Increase scalability when you have a high volume of inserts by spreading the inserts across multiple partitions.</span></span>  

#### <a name="context-and-problem"></a><span data-ttu-id="4e549-800">Kontext a problém</span><span class="sxs-lookup"><span data-stu-id="4e549-800">Context and problem</span></span>
<span data-ttu-id="4e549-801">Předřazení nebo připojování entity na vaše uložené entity obvykle výsledkem přidání nové entity na první nebo poslední oddíl posloupnosti oddíly aplikace.</span><span class="sxs-lookup"><span data-stu-id="4e549-801">Prepending or appending entities to your stored entities typically results in the application adding new entities to the first or last partition of a sequence of partitions.</span></span> <span data-ttu-id="4e549-802">V takovém případě všechny vložení v každém okamžiku dala ve stejném oddílu, vytváření aktivní oblast, která zabraňuje služby table z zatížení vyrovnávání vložení mezi několika uzly a což může způsobit vaše aplikace a stiskněte tlačítko cíle škálovatelnosti pro oddíl.</span><span class="sxs-lookup"><span data-stu-id="4e549-802">In this case, all of the inserts at any given time are taking place in the same partition, creating a hotspot that prevents the table service from load balancing inserts across multiple nodes, and possibly causing your application to hit the scalability targets for partition.</span></span> <span data-ttu-id="4e549-803">Například pokud máte aplikaci, která protokoly sítě a přístupu k prostředkům zaměstnanci, pak entity strukturu jak je uvedeno níže mohly by vést do aktuální hodiny oddílu stane aktivní oblast, pokud objem transakcí dosáhne cíle škálovatelnosti pro jednotlivé oddílu:</span><span class="sxs-lookup"><span data-stu-id="4e549-803">For example, if you have an application that logs network and resource access by employees, then an entity structure as shown below could result in the current hour's partition becoming a hotspot if the volume of transactions reaches the scalability target for an individual partition:</span></span>  

![][26]

#### <a name="solution"></a><span data-ttu-id="4e549-804">Řešení</span><span class="sxs-lookup"><span data-stu-id="4e549-804">Solution</span></span>
<span data-ttu-id="4e549-805">Následující strukturu alternativní entity zabraňuje v žádné konkrétní oddílu aktivní bod jako protokoly událostí aplikace:</span><span class="sxs-lookup"><span data-stu-id="4e549-805">The following alternative entity structure avoids a hotspot on any particular partition as the application logs events:</span></span>  

![][27]

<span data-ttu-id="4e549-806">Všimněte si tento příklad jak oba **PartitionKey** a **RowKey** jsou složeného klíče.</span><span class="sxs-lookup"><span data-stu-id="4e549-806">Notice with this example how both the **PartitionKey** and **RowKey** are compound keys.</span></span> <span data-ttu-id="4e549-807">**PartitionKey** využívá id oddělení i zaměstnanců k distribuci protokolování napříč více oddílů.</span><span class="sxs-lookup"><span data-stu-id="4e549-807">The **PartitionKey** uses both the department and employee id to distribute the logging across multiple partitions.</span></span>  

#### <a name="issues-and-considerations"></a><span data-ttu-id="4e549-808">Problémy a důležité informace</span><span class="sxs-lookup"><span data-stu-id="4e549-808">Issues and considerations</span></span>
<span data-ttu-id="4e549-809">Při rozhodování o tom, jak implementovat tento vzor, zvažte následující body:</span><span class="sxs-lookup"><span data-stu-id="4e549-809">Consider the following points when deciding how to implement this pattern:</span></span>  

* <span data-ttu-id="4e549-810">Podporuje alternativní klíče struktura, která zabraňuje vytváření aktivní oddíly na vložení efektivní dotazy, které provede klientské aplikace?</span><span class="sxs-lookup"><span data-stu-id="4e549-810">Does the alternative key structure that avoids creating hot partitions on inserts efficiently support the queries your client application makes?</span></span>  
* <span data-ttu-id="4e549-811">Předpokládaného svazku transakcí znamená, že budete chtít nejspíš k dosažení cíle škálovatelnosti pro jednotlivé oddíl a omezeny službou úložiště?</span><span class="sxs-lookup"><span data-stu-id="4e549-811">Does your anticipated volume of transactions mean that you are likely to reach the scalability targets for an individual partition and be throttled by the storage service?</span></span>  

#### <a name="when-to-use-this-pattern"></a><span data-ttu-id="4e549-812">Kdy použít tento vzor</span><span class="sxs-lookup"><span data-stu-id="4e549-812">When to use this pattern</span></span>
<span data-ttu-id="4e549-813">Připojit/prepend proti vzor Vyhněte se při svazku transakcí je pravděpodobné, aby výsledkem omezení pomocí služby úložiště při přístupu k aktivní oddíl.</span><span class="sxs-lookup"><span data-stu-id="4e549-813">Avoid the prepend/append anti-pattern when your volume of transactions is likely to result in throttling by the storage service when you access a hot partition.</span></span>  

#### <a name="related-patterns-and-guidance"></a><span data-ttu-id="4e549-814">Související vzory a pokyny</span><span class="sxs-lookup"><span data-stu-id="4e549-814">Related patterns and guidance</span></span>
<span data-ttu-id="4e549-815">Následující pokyny a vzory může být také relevantní při implementaci tohoto vzoru:</span><span class="sxs-lookup"><span data-stu-id="4e549-815">The following patterns and guidance may also be relevant when implementing this pattern:</span></span>  

* [<span data-ttu-id="4e549-816">Složené klíče vzor</span><span class="sxs-lookup"><span data-stu-id="4e549-816">Compound key pattern</span></span>](#compound-key-pattern)  
* [<span data-ttu-id="4e549-817">Vzor protokolu poškozené databáze</span><span class="sxs-lookup"><span data-stu-id="4e549-817">Log tail pattern</span></span>](#log-tail-pattern)  
* [<span data-ttu-id="4e549-818">Úprava entity</span><span class="sxs-lookup"><span data-stu-id="4e549-818">Modifying entities</span></span>](#modifying-entities)  

### <a name="log-data-anti-pattern"></a><span data-ttu-id="4e549-819">Proti vzorek dat protokolu</span><span class="sxs-lookup"><span data-stu-id="4e549-819">Log data anti-pattern</span></span>
<span data-ttu-id="4e549-820">Místo služby Table by měl obvykle používat službu objektů Blob k ukládání dat protokolu.</span><span class="sxs-lookup"><span data-stu-id="4e549-820">Typically, you should use the Blob service instead of the Table service to store log data.</span></span>  

#### <a name="context-and-problem"></a><span data-ttu-id="4e549-821">Kontext a problém</span><span class="sxs-lookup"><span data-stu-id="4e549-821">Context and problem</span></span>
<span data-ttu-id="4e549-822">Případ použití běžné pro data protokolu je načíst výběr položky protokolu pro konkrétní datum a čas rozsah: například chcete najít všechny chyby a kritické zprávy, které vaše aplikace protokolují mezi 15:04 a 15:06 na konkrétní datum.</span><span class="sxs-lookup"><span data-stu-id="4e549-822">A common use case for log data is to retrieve a selection of log entries for a specific date/time range: for example, you want to find all the error and critical messages that your application logged between 15:04 and 15:06 on a specific date.</span></span> <span data-ttu-id="4e549-823">Nechcete použít k určení oddílu uložit protokolu entity k datum a čas zprávy protokolu: který výsledkem aktivního oddílu, protože v každém okamžiku budou sdílet všechny entity služby protokolu stejné **PartitionKey** hodnotu (naleznete v části [Prepend/připojovat proti vzor](#prepend-append-anti-pattern)).</span><span class="sxs-lookup"><span data-stu-id="4e549-823">You do not want to use the date and time of the log message to determine the partition you save log entities to: that results in a hot partition because at any given time, all the log entities will share the same **PartitionKey** value (see the section [Prepend/append anti-pattern](#prepend-append-anti-pattern)).</span></span> <span data-ttu-id="4e549-824">Například následující schéma entity pro zprávu protokolu dochází v oddílu aktivní, protože aplikace pro aktuální datum a hodinu všechny zprávy protokolu zapíše do oddílu:</span><span class="sxs-lookup"><span data-stu-id="4e549-824">For example, the following entity schema for a log message results in a hot partition because the application writes all log messages to the partition for the current date and hour:</span></span>  

![][28]

<span data-ttu-id="4e549-825">V tomto příkladu **RowKey** obsahuje datum a čas zprávy protokolu zajistit, že zprávy protokolu jsou uloženy řazení datum a čas a obsahuje id zprávy v případě, že více zpráv protokolu sdílet stejné datum a čas.</span><span class="sxs-lookup"><span data-stu-id="4e549-825">In this example, the **RowKey** includes the date and time of the log message to ensure that log messages are stored sorted in date/time order, and includes a message id in case multiple log messages share the same date and time.</span></span>  

<span data-ttu-id="4e549-826">Další možností je použít **PartitionKey** zajistí, že aplikace zapíše zprávy napříč celou řadu oddíly.</span><span class="sxs-lookup"><span data-stu-id="4e549-826">Another approach is to use a **PartitionKey** that ensures that the application writes messages across a range of partitions.</span></span> <span data-ttu-id="4e549-827">Například pokud zdroj zprávy protokolu poskytuje způsob, jak distribuovat zprávy napříč mnoha oddílů, můžete použít následující schéma entity:</span><span class="sxs-lookup"><span data-stu-id="4e549-827">For example, if the source of the log message provides a way to distribute messages across many partitions, you could use the following entity schema:</span></span>  

![][29]

<span data-ttu-id="4e549-828">Problém s tímto schématem je však načíst všechny zprávy protokolu pro konkrétní časové rozpětí musí prohledávat každý oddíl v tabulce.</span><span class="sxs-lookup"><span data-stu-id="4e549-828">However, the problem with this schema is that to retrieve all the log messages for a specific time span you must search every partition in the table.</span></span>

#### <a name="solution"></a><span data-ttu-id="4e549-829">Řešení</span><span class="sxs-lookup"><span data-stu-id="4e549-829">Solution</span></span>
<span data-ttu-id="4e549-830">V předchozí části zvýrazněná problém pokusu o použití služby Table pro uložení položek protokolu a navrhované dva, nevyhovující, návrhy.</span><span class="sxs-lookup"><span data-stu-id="4e549-830">The previous section highlighted the problem of trying to use the Table service to store log entries and suggested two, unsatisfactory, designs.</span></span> <span data-ttu-id="4e549-831">Jedno řešení, která vedla k aktivní oddíl s riziko nízký výkon zápis zpráv protokolu; jiné řešení výsledkem dotaz nízký výkon z důvodu požadavek na kontrolovat každý oddíl v tabulce k načtení protokolu zprávy pro konkrétní časové období.</span><span class="sxs-lookup"><span data-stu-id="4e549-831">One solution led to a hot partition with the risk of poor performance writing log messages; the other solution resulted in poor query performance because of the requirement to scan every partition in the table to retrieve log messages for a specific time span.</span></span> <span data-ttu-id="4e549-832">Úložiště BLOB nabízí lepší řešení pro tento typ scénáře, a to je, jak Azure Storage Analytics ukládá data protokolu shromažďuje.</span><span class="sxs-lookup"><span data-stu-id="4e549-832">Blob storage offers a better solution for this type of scenario and this is how Azure Storage Analytics stores the log data it collects.</span></span>  

<span data-ttu-id="4e549-833">Tato část popisuje, jak analytika úložiště ukládá data protokolu v úložišti objektů blob jako ilustraci tohoto přístupu k ukládání dat, která obvykle dotazování podle rozsahu.</span><span class="sxs-lookup"><span data-stu-id="4e549-833">This section outlines how Storage Analytics stores log data in blob storage as an illustration of this approach to storing data that you typically query by range.</span></span>  

<span data-ttu-id="4e549-834">Analytika úložiště ukládá zprávy protokolu ve formátu odděleného do více objektů BLOB.</span><span class="sxs-lookup"><span data-stu-id="4e549-834">Storage Analytics stores log messages in a delimited format in multiple blobs.</span></span> <span data-ttu-id="4e549-835">Formát odděleného usnadňuje klientskou aplikaci k analýze dat ve zprávě protokolu.</span><span class="sxs-lookup"><span data-stu-id="4e549-835">The delimited format makes it easy for a client application to parse the data in the log message.</span></span>  

<span data-ttu-id="4e549-836">Analytika úložiště používá vytváření názvů pro objekty BLOB, které umožňuje vyhledat objekt blob (nebo objekty BLOB), která obsahují zprávy protokolu, pro které chcete najít.</span><span class="sxs-lookup"><span data-stu-id="4e549-836">Storage Analytics uses a naming convention for blobs that enables you to locate the blob (or blobs) that contain the log messages for which you are searching.</span></span> <span data-ttu-id="4e549-837">Například objekt blob s názvem "queue/2014/07/31/1800/000001.log" obsahuje zprávy protokolu, které se vztahují ke službě fronty za hodinu od 18:00 na 31 červenec 2014.</span><span class="sxs-lookup"><span data-stu-id="4e549-837">For example, a blob named "queue/2014/07/31/1800/000001.log" contains log messages that relate to the queue service for the hour starting at 18:00 on 31 July 2014.</span></span> <span data-ttu-id="4e549-838">"000001" označuje, že toto je první soubor protokolu pro toto období.</span><span class="sxs-lookup"><span data-stu-id="4e549-838">The "000001" indicates that this is the first log file for this period.</span></span> <span data-ttu-id="4e549-839">Analytika úložiště taky zaznamenává časová razítka zpráv protokolu první a poslední uložené v souboru jako součást metadata objektu blob.</span><span class="sxs-lookup"><span data-stu-id="4e549-839">Storage Analytics also records the timestamps of the first and last log messages stored in the file as part of the blob's metadata.</span></span> <span data-ttu-id="4e549-840">Rozhraní API pro objekt blob úložiště umožňuje vyhledat objekty BLOB v kontejneru na základě předpony názvu: Chcete-li vyhledat všechny objekty BLOB, které obsahují data protokolu fronty za hodinu od 18:00, můžete použít předponu "fronty/2014/07/31/1800."</span><span class="sxs-lookup"><span data-stu-id="4e549-840">The API for blob storage enables you locate blobs in a container based on a name prefix: to locate all the blobs that contain queue log data for the hour starting at 18:00, you can use the prefix "queue/2014/07/31/1800."</span></span>  

<span data-ttu-id="4e549-841">Vyrovnávací paměti Analytics úložiště interně protokolování zpráv a pravidelně aktualizuje odpovídající objekt blob nebo vytvoří nový s nejnovější dávku položky protokolu.</span><span class="sxs-lookup"><span data-stu-id="4e549-841">Storage Analytics buffers log messages internally and then periodically updates the appropriate blob or creates a new one with the latest batch of log entries.</span></span> <span data-ttu-id="4e549-842">Tím se snižuje počet zápisů, které musíte provést na služby objektů blob.</span><span class="sxs-lookup"><span data-stu-id="4e549-842">This reduces the number of writes it must perform to the blob service.</span></span>  

<span data-ttu-id="4e549-843">Při implementaci řešení podobné ve vaší vlastní aplikaci, musíte zvážit, jakým způsobem spravovat kompromis mezi spolehlivost (zápis každá položka protokolu do úložiště objektů blob jako Odehrává se) a náklady a škálovatelnost (ukládání do vyrovnávací paměti aktualizací ve vaší aplikaci a jejich zápis do úložiště objektů blob v dávkách).</span><span class="sxs-lookup"><span data-stu-id="4e549-843">If you are implementing a similar solution in your own application, you must consider how to manage the trade-off between reliability (writing every log entry to blob storage as it happens) and cost and scalability (buffering updates in your application and writing them to blob storage in batches).</span></span>  

#### <a name="issues-and-considerations"></a><span data-ttu-id="4e549-844">Problémy a důležité informace</span><span class="sxs-lookup"><span data-stu-id="4e549-844">Issues and considerations</span></span>
<span data-ttu-id="4e549-845">Při rozhodování o tom, jak ukládat data protokolu, zvažte následující body:</span><span class="sxs-lookup"><span data-stu-id="4e549-845">Consider the following points when deciding how to store log data:</span></span>  

* <span data-ttu-id="4e549-846">Pokud vytvoříte návrh tabulky, který zabraňuje potenciální aktivní oddíly, můžete zjistit, data protokolu nelze efektivní přístup.</span><span class="sxs-lookup"><span data-stu-id="4e549-846">If you create a table design that avoids potential hot partitions, you may find that you cannot access your log data efficiently.</span></span>  
* <span data-ttu-id="4e549-847">Ke zpracování dat protokolu, klient často potřebuje načíst mnoho záznamů.</span><span class="sxs-lookup"><span data-stu-id="4e549-847">To process log data, a client often needs to load many records.</span></span>  
* <span data-ttu-id="4e549-848">I když je často strukturovaná data protokolu, úložiště objektů blob může být lepším řešením.</span><span class="sxs-lookup"><span data-stu-id="4e549-848">Although log data is often structured, blob storage may be a better solution.</span></span>  

### <a name="implementation-considerations"></a><span data-ttu-id="4e549-849">Důležité informace o implementaci</span><span class="sxs-lookup"><span data-stu-id="4e549-849">Implementation considerations</span></span>
<span data-ttu-id="4e549-850">Tato část popisuje některé aspekty k berte v úvahu při implementaci vzory popsané v předchozích částech.</span><span class="sxs-lookup"><span data-stu-id="4e549-850">This section discusses some of the considerations to bear in mind when you implement the patterns described in the previous sections.</span></span> <span data-ttu-id="4e549-851">Většina Tato část používá příklady napsané v C#, které používají knihovnu klienta služby Storage (verze 4.3.0 v době psaní textu).</span><span class="sxs-lookup"><span data-stu-id="4e549-851">Most of this section uses examples written in C# that use the Storage Client Library (version 4.3.0 at the time of writing).</span></span>  

### <a name="retrieving-entities"></a><span data-ttu-id="4e549-852">Načtení entit</span><span class="sxs-lookup"><span data-stu-id="4e549-852">Retrieving entities</span></span>
<span data-ttu-id="4e549-853">Jak je popsáno v části [návrhu pro dotazování](#design-for-querying), nejvíce efektivní dotaz je dotaz bodu.</span><span class="sxs-lookup"><span data-stu-id="4e549-853">As discussed in the section [Design for querying](#design-for-querying), the most efficient query is a point query.</span></span> <span data-ttu-id="4e549-854">Ale v některých scénářích musíte k načtení více entit.</span><span class="sxs-lookup"><span data-stu-id="4e549-854">However, in some scenarios you may need to retrieve multiple entities.</span></span> <span data-ttu-id="4e549-855">Tato část popisuje některé běžné přístupy k načtení entity pomocí klientské knihovny pro úložiště.</span><span class="sxs-lookup"><span data-stu-id="4e549-855">This section describes some common approaches to retrieving entities using the Storage Client Library.</span></span>  

#### <a name="executing-a-point-query-using-the-storage-client-library"></a><span data-ttu-id="4e549-856">Spuštění dotazu na bodu pomocí klientské knihovny pro úložiště</span><span class="sxs-lookup"><span data-stu-id="4e549-856">Executing a point query using the Storage Client Library</span></span>
<span data-ttu-id="4e549-857">Nejjednodušší způsob, jak provést dotaz bod je použití **načíst** tabulky operace, jak je znázorněno následujícím C# fragment kódu, který načte entity s **PartitionKey** hodnoty "Prodej" a **RowKey** hodnoty "212":</span><span class="sxs-lookup"><span data-stu-id="4e549-857">The easiest way to execute a point query is to use the **Retrieve** table operation as shown in the following C# code snippet that retrieves an entity with a **PartitionKey** of value "Sales" and a **RowKey** of value "212":</span></span>  

```csharp
TableOperation retrieveOperation = TableOperation.Retrieve<EmployeeEntity>("Sales", "212");
var retrieveResult = employeeTable.Execute(retrieveOperation);
if (retrieveResult.Result != null)
{
    EmployeeEntity employee = (EmployeeEntity)retrieveResult.Result;
    ...
}  
```

<span data-ttu-id="4e549-858">Všimněte si, jak tento příklad očekává entity ho načte být typu **EmployeeEntity**.</span><span class="sxs-lookup"><span data-stu-id="4e549-858">Notice how this example expects the entity it retrieves to be of type **EmployeeEntity**.</span></span>  

#### <a name="retrieving-multiple-entities-using-linq"></a><span data-ttu-id="4e549-859">Načítání více entit pomocí LINQ</span><span class="sxs-lookup"><span data-stu-id="4e549-859">Retrieving multiple entities using LINQ</span></span>
<span data-ttu-id="4e549-860">Můžete načíst pomocí LINQ s Klientská knihovna pro úložiště a zadat dotaz s více entit **kde** klauzule.</span><span class="sxs-lookup"><span data-stu-id="4e549-860">You can retrieve multiple entities by using LINQ with Storage Client Library and specifying a query with a **where** clause.</span></span> <span data-ttu-id="4e549-861">Abyste se vyhnuli prohledávání tabulky, by měla vždycky obsahovat **PartitionKey** hodnotu v where klauzule a pokud je to možné **RowKey** hodnotu, aby se zabránilo prohledávání tabulky a oddíl.</span><span class="sxs-lookup"><span data-stu-id="4e549-861">To avoid a table scan, you should always include the **PartitionKey** value in the where clause, and if possible the **RowKey** value to avoid table and partition scans.</span></span> <span data-ttu-id="4e549-862">Služba table podporuje omezenou sadu operátory porovnání (větší než, větší než nebo rovna méně než je menší než nebo rovna, stejné a není rovno) pro použití v where klauzule.</span><span class="sxs-lookup"><span data-stu-id="4e549-862">The table service supports a limited set of comparison operators (greater than, greater than or equal, less than, less than or equal, equal, and not equal) to use in the where clause.</span></span> <span data-ttu-id="4e549-863">Následující fragment kódu jazyka C# vyhledá všechny zaměstnance, jejichž poslední název začíná na "B" (za předpokladu, že **RowKey** ukládá příjmení) v oddělení prodeje (za předpokladu, že **PartitionKey** ukládá název oddělení):</span><span class="sxs-lookup"><span data-stu-id="4e549-863">The following C# code snippet finds all the employees whose last name starts with "B" (assuming that the **RowKey** stores the last name) in the sales department (assuming the **PartitionKey** stores the department name):</span></span>  

```csharp
TableQuery<EmployeeEntity> employeeQuery = employeeTable.CreateQuery<EmployeeEntity>();
var query = (from employee in employeeQuery
            where employee.PartitionKey == "Sales" &&
            employee.RowKey.CompareTo("B") >= 0 &&
            employee.RowKey.CompareTo("C") < 0
            select employee).AsTableQuery();
var employees = query.Execute();  
```

<span data-ttu-id="4e549-864">Všimněte si, jak dotaz Určuje, jak **RowKey** a **PartitionKey** zajistit lepší výkon.</span><span class="sxs-lookup"><span data-stu-id="4e549-864">Notice how the query specifies both a **RowKey** and a **PartitionKey** to ensure better performance.</span></span>  

<span data-ttu-id="4e549-865">Následující příklad kódu ukazuje ekvivalentní funkce pomocí rozhraní fluent API (Další informace o rozhraní fluent API obecně platí, najdete v části [osvědčené postupy pro navrhování rozhraní Fluent API](http://visualstudiomagazine.com/articles/2013/12/01/best-practices-for-designing-a-fluent-api.aspx)):</span><span class="sxs-lookup"><span data-stu-id="4e549-865">The following code sample shows equivalent functionality using the fluent API (for more information about fluent APIs in general, see [Best Practices for Designing a Fluent API](http://visualstudiomagazine.com/articles/2013/12/01/best-practices-for-designing-a-fluent-api.aspx)):</span></span>  

```csharp
TableQuery<EmployeeEntity> employeeQuery = new TableQuery<EmployeeEntity>().Where(
    TableQuery.CombineFilters(
    TableQuery.CombineFilters(
        TableQuery.GenerateFilterCondition(
    "PartitionKey", QueryComparisons.Equal, "Sales"),
    TableOperators.And,
    TableQuery.GenerateFilterCondition(
    "RowKey", QueryComparisons.GreaterThanOrEqual, "B")
),
TableOperators.And,
TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "C")
    )
);
var employees = employeeTable.ExecuteQuery(employeeQuery);  
```

> [!NOTE]
> <span data-ttu-id="4e549-866">Ukázka vnořena více **CombineFilters** metody pro zahrnutí tři filtr podmínek.</span><span class="sxs-lookup"><span data-stu-id="4e549-866">The sample nests multiple **CombineFilters** methods to include the three filter conditions.</span></span>  
> 
> 

#### <a name="retrieving-large-numbers-of-entities-from-a-query"></a><span data-ttu-id="4e549-867">Načítání velký počet entit z dotazu</span><span class="sxs-lookup"><span data-stu-id="4e549-867">Retrieving large numbers of entities from a query</span></span>
<span data-ttu-id="4e549-868">Optimální dotaz vrátí jednotlivých entit na základě **PartitionKey** hodnotu a **RowKey** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4e549-868">An optimal query returns an individual entity based on a **PartitionKey** value and a **RowKey** value.</span></span> <span data-ttu-id="4e549-869">Ale v některých případech může mít požadavek na vrácení mnoho entit ze stejného oddílu nebo dokonce z mnoha oddílů.</span><span class="sxs-lookup"><span data-stu-id="4e549-869">However, in some scenarios you may have a requirement to return many entities from the same partition or even from many partitions.</span></span>  

<span data-ttu-id="4e549-870">V takových případech by měla vždy plně testování výkonu aplikace.</span><span class="sxs-lookup"><span data-stu-id="4e549-870">You should always fully test the performance of your application in such scenarios.</span></span>  

<span data-ttu-id="4e549-871">Dotazy na data služby table může vrátit maximálně 1000 entit najednou a může spustit maximálně pět sekund.</span><span class="sxs-lookup"><span data-stu-id="4e549-871">A query against the table service may return a maximum of 1,000 entities at one time and may execute for a maximum of five seconds.</span></span> <span data-ttu-id="4e549-872">Pokud sadu výsledků dotazu obsahuje více než 1 000 entity, pokud dotaz nebyl dokončen v pět sekund, nebo pokud dotaz protne hranice oddílu, služby Table vrátí token pokračování povolit klientskou aplikaci požádat o další sadu entit.</span><span class="sxs-lookup"><span data-stu-id="4e549-872">If the result set contains more than 1,000 entities, if the query did not complete within five seconds, or if the query crosses the partition boundary, the Table service returns a continuation token to enable the client application to request the next set of entities.</span></span> <span data-ttu-id="4e549-873">Další informace o tom, jak pokračování tokeny pracovní najdete v tématu [časový limit dotazu a Paginování](http://msdn.microsoft.com/library/azure/dd135718.aspx).</span><span class="sxs-lookup"><span data-stu-id="4e549-873">For more information about how continuation tokens work, see [Query Timeout and Pagination](http://msdn.microsoft.com/library/azure/dd135718.aspx).</span></span>  

<span data-ttu-id="4e549-874">Pokud používáte Klientská knihovna pro úložiště, může ho automaticky jako vrátí entity ze služby Table pro vás zpracovávat pokračování tokeny.</span><span class="sxs-lookup"><span data-stu-id="4e549-874">If you are using the Storage Client Library, it can automatically handle continuation tokens for you as it returns entities from the Table service.</span></span> <span data-ttu-id="4e549-875">Následující C# ukázka kódu pomocí klientské knihovny pro úložiště automaticky zpracovává pokračování tokeny, pokud je služba table vrátí v odpovědi:</span><span class="sxs-lookup"><span data-stu-id="4e549-875">The following C# code sample using the Storage Client Library automatically handles continuation tokens if the table service returns them in a response:</span></span>  

```csharp
string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);

var employees = employeeTable.ExecuteQuery(employeeQuery);
foreach (var emp in employees)
{
        ...
}  
```

<span data-ttu-id="4e549-876">Následující kód C# zpracovává pokračování tokeny explicitně:</span><span class="sxs-lookup"><span data-stu-id="4e549-876">The following C# code handles continuation tokens explicitly:</span></span>  

```csharp
string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);

TableContinuationToken continuationToken = null;

do
{
        var employees = employeeTable.ExecuteQuerySegmented(
        employeeQuery, continuationToken);
    foreach (var emp in employees)
    {
    ...
    }
    continuationToken = employees.ContinuationToken;
} while (continuationToken != null);  
```

<span data-ttu-id="4e549-877">Pomocí explicitně pokračování tokenů, můžete řídit, kdy aplikace načítá další segment dat.</span><span class="sxs-lookup"><span data-stu-id="4e549-877">By using continuation tokens explicitly, you can control when your application retrieves the next segment of data.</span></span> <span data-ttu-id="4e549-878">Například pokud klientské aplikace umožňuje uživatelům procházet entity, které jsou uložené v tabulce, uživatel může rozhodnout všechny entity načíst dotaz tak, že vaše aplikace by token pokračování používat pouze pro načtení další segment, když uživatel měl dokončí stránkování prostřednictvím všechny entity v aktuálním segmentu procházet po stránkách.</span><span class="sxs-lookup"><span data-stu-id="4e549-878">For example, if your client application enables users to page through the entities stored in a table, a user may decide not to page through all the entities retrieved by the query so your application would only use a continuation token to retrieve the next segment when the user had finished paging through all the entities in the current segment.</span></span> <span data-ttu-id="4e549-879">Tento přístup má několik výhod:</span><span class="sxs-lookup"><span data-stu-id="4e549-879">This approach has several benefits:</span></span>  

* <span data-ttu-id="4e549-880">Umožňuje omezit množství dat načíst ze služby Table a že přesouváte přes síť.</span><span class="sxs-lookup"><span data-stu-id="4e549-880">It enables you to limit the amount of data to retrieve from the Table service and that you move over the network.</span></span>  
* <span data-ttu-id="4e549-881">Umožňuje provádět asynchronní vstupně-výstupní operace v rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="4e549-881">It enables you to perform asynchronous IO in .NET.</span></span>  
* <span data-ttu-id="4e549-882">Umožňuje serializovat token pokračování do trvalého úložiště, takže můžete pokračovat v případě při selhání aplikace.</span><span class="sxs-lookup"><span data-stu-id="4e549-882">It enables you to serialize the continuation token to persistent storage so you can continue in the event of an application crash.</span></span>  

> [!NOTE]
> <span data-ttu-id="4e549-883">Token pokračování obvykle vrátí segment obsahující 1000 entity, i když může být méně.</span><span class="sxs-lookup"><span data-stu-id="4e549-883">A continuation token typically returns a segment containing 1,000 entities, although it may be fewer.</span></span> <span data-ttu-id="4e549-884">Toto se taky případě, když je omezit počet položek dotaz vrátí pomocí **trvat** vrátit první n entitami, které odpovídají zadaným kritériím vyhledávání: služby table může vrátit segment obsahující méně než n entity společně s token pokračování umožnit načtení zbývající entit.</span><span class="sxs-lookup"><span data-stu-id="4e549-884">This is also the case if you limit the number of entries a query returns by using **Take** to return the first n entities that match your lookup criteria: the table service may return a segment containing fewer than n entities along with a continuation token to enable you to retrieve the remaining entities.</span></span>  
> 
> 

<span data-ttu-id="4e549-885">Následující kód C# ukazuje, jak upravit počet entit, vrátila uvnitř segment:</span><span class="sxs-lookup"><span data-stu-id="4e549-885">The following C# code shows how to modify the number of entities returned inside a segment:</span></span>  

```csharp
employeeQuery.TakeCount = 50;  
```

#### <a name="server-side-projection"></a><span data-ttu-id="4e549-886">Projekce na straně serveru</span><span class="sxs-lookup"><span data-stu-id="4e549-886">Server-side projection</span></span>
<span data-ttu-id="4e549-887">Jedna entita může mít maximálně 255 vlastnosti a mít velikost až 1 MB.</span><span class="sxs-lookup"><span data-stu-id="4e549-887">A single entity can have up to 255 properties and be up to 1 MB in size.</span></span> <span data-ttu-id="4e549-888">Při dotazování tabulky a načtení entit, možná nebudete potřebovat všechny vlastnosti a přenosu dat zbytečně (Pokud chcete snížit latenci a náklady) se můžete vyhnout.</span><span class="sxs-lookup"><span data-stu-id="4e549-888">When you query the table and retrieve entities, you may not need all the properties and can avoid transferring data unnecessarily (to help reduce latency and cost).</span></span> <span data-ttu-id="4e549-889">Projekce na straně serveru může použít k přenosu pouze vlastnosti, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="4e549-889">You can use server-side projection to transfer just the properties you need.</span></span> <span data-ttu-id="4e549-890">V následujícím příkladu se načte jenom na **e-mailu** vlastnost (spolu s **PartitionKey**, **RowKey**, **časové razítko**, a **značka ETag**) z entit vybrána v dotazu.</span><span class="sxs-lookup"><span data-stu-id="4e549-890">The following example is retrieves just the **Email** property (along with **PartitionKey**, **RowKey**, **Timestamp**, and **ETag**) from the entities selected by the query.</span></span>  

```csharp
string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
List<string> columns = new List<string>() { "Email" };
TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter).Select(columns);

var entities = employeeTable.ExecuteQuery(employeeQuery);
foreach (var e in entities)
{
        Console.WriteLine("RowKey: {0}, EmployeeEmail: {1}", e.RowKey, e.Email);
}  
```

<span data-ttu-id="4e549-891">Všimněte si jak **RowKey** hodnota je k dispozici, přestože nebyl uveden v seznamu vlastností pro načtení.</span><span class="sxs-lookup"><span data-stu-id="4e549-891">Notice how the **RowKey** value is available even though it was not included in the list of properties to retrieve.</span></span>  

### <a name="modifying-entities"></a><span data-ttu-id="4e549-892">Úprava entity</span><span class="sxs-lookup"><span data-stu-id="4e549-892">Modifying entities</span></span>
<span data-ttu-id="4e549-893">Klientská knihovna pro úložiště umožňuje upravit vaší entity uložené ve službě table vkládání, odstraňování a aktualizaci entity.</span><span class="sxs-lookup"><span data-stu-id="4e549-893">The Storage Client Library enables you to modify your entities stored in the table service by inserting, deleting, and updating entities.</span></span> <span data-ttu-id="4e549-894">Můžete EGTs dávky více insert, update a operace odstranění společně a snížit počet zpátečních cest vyžaduje a zlepšíte výkon vašeho řešení.</span><span class="sxs-lookup"><span data-stu-id="4e549-894">You can use EGTs to batch multiple insert, update, and delete operations together to reduce the number of round trips required and improve the performance of your solution.</span></span>  

<span data-ttu-id="4e549-895">Všimněte si, že výjimky vydané když klientská knihovna pro úložiště provede EGT obvykle zahrnují index entity, která způsobila, že dávka selhání.</span><span class="sxs-lookup"><span data-stu-id="4e549-895">Note that exceptions thrown when the Storage Client Library executes an EGT typically include the index of the entity that caused the batch to fail.</span></span> <span data-ttu-id="4e549-896">To je užitečné při ladění kódu, který používá EGTs.</span><span class="sxs-lookup"><span data-stu-id="4e549-896">This is helpful when you are debugging code that uses EGTs.</span></span>  

<span data-ttu-id="4e549-897">Měli byste také zvážit, jak váš návrh ovlivňuje způsob, jakým klientské aplikace zpracovává operace souběžnosti a aktualizace.</span><span class="sxs-lookup"><span data-stu-id="4e549-897">You should also consider how your design affects how your client application handles concurrency and update operations.</span></span>  

#### <a name="managing-concurrency"></a><span data-ttu-id="4e549-898">Správa souběžnosti</span><span class="sxs-lookup"><span data-stu-id="4e549-898">Managing concurrency</span></span>
<span data-ttu-id="4e549-899">Ve výchozím nastavení, implementuje optimistickou metodu souběžného zpracování kontroluje na úrovni jednotlivých entit pro služby table **vložit**, **sloučení**, a **odstranit** operace, přestože je možné pro klienta, chcete-li vynutit služby table obejít těchto kontrol.</span><span class="sxs-lookup"><span data-stu-id="4e549-899">By default, the table service implements optimistic concurrency checks at the level of individual entities for **Insert**, **Merge**, and **Delete** operations, although it is possible for a client to force the table service to bypass these checks.</span></span> <span data-ttu-id="4e549-900">Další informace o správě služby table souběžnosti najdete v tématu [Správa souběžnost v Microsoft Azure Storage](../storage/common/storage-concurrency.md).</span><span class="sxs-lookup"><span data-stu-id="4e549-900">For more information about how the table service manages concurrency, see  [Managing Concurrency in Microsoft Azure Storage](../storage/common/storage-concurrency.md).</span></span>  

#### <a name="merge-or-replace"></a><span data-ttu-id="4e549-901">Sloučení nebo nahradit</span><span class="sxs-lookup"><span data-stu-id="4e549-901">Merge or replace</span></span>
<span data-ttu-id="4e549-902">**Nahradit** metodu **TableOperation** třída vždycky nahradí úplnou entitu ve službě Table.</span><span class="sxs-lookup"><span data-stu-id="4e549-902">The **Replace** method of the **TableOperation** class always replaces the complete entity in the Table service.</span></span> <span data-ttu-id="4e549-903">Pokud nezadáte vlastnost v žádosti o tuto vlastnost existuje v uložené entity, žádost tuto vlastnost odebere uložené entity.</span><span class="sxs-lookup"><span data-stu-id="4e549-903">If you do not include a property in the request when that property exists in the stored entity, the request removes that property from the stored entity.</span></span> <span data-ttu-id="4e549-904">Pokud chcete explicitně odebrání vlastnosti z uložené entity, musí obsahovat všech vlastností v požadavku.</span><span class="sxs-lookup"><span data-stu-id="4e549-904">Unless you want to remove a property explicitly from a stored entity, you must include every property in the request.</span></span>  

<span data-ttu-id="4e549-905">Můžete použít **sloučení** metodu **TableOperation** třídy ke snížení objemu dat, která odešlete do služby Table, pokud chcete aktualizovat entitu.</span><span class="sxs-lookup"><span data-stu-id="4e549-905">You can use the **Merge** method of the **TableOperation** class to reduce the amount of data that you send to the Table service when you want to update an entity.</span></span> <span data-ttu-id="4e549-906">**Sloučení** metoda nahradí všechny vlastnosti v entitě uložené hodnoty vlastností z entity, které jsou součástí požadavku, ale zůstanou zachovány vlastnosti uložené entity, které nejsou zahrnuté v požadavku.</span><span class="sxs-lookup"><span data-stu-id="4e549-906">The **Merge** method replaces any properties in the stored entity with property values from the entity included in the request, but leaves intact any properties in the stored entity that are not included in the request.</span></span> <span data-ttu-id="4e549-907">To je užitečné, pokud máte velké entity a potřeba jenom aktualizovat malý počet vlastností v požadavku.</span><span class="sxs-lookup"><span data-stu-id="4e549-907">This is useful if you have large entities and only need to update a small number of properties in a request.</span></span>  

> [!NOTE]
> <span data-ttu-id="4e549-908">**Nahradit** a **sloučení** metody nezdaří, pokud entita neexistuje.</span><span class="sxs-lookup"><span data-stu-id="4e549-908">The **Replace** and **Merge** methods fail if the entity does not exist.</span></span> <span data-ttu-id="4e549-909">Jako alternativu, můžete použít **InsertOrReplace** a **InsertOrMerge** metody, které vytvářejí nové entity, pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="4e549-909">As an alternative, you can use the **InsertOrReplace** and **InsertOrMerge** methods that create a new entity if it doesn't exist.</span></span>  
> 
> 

### <a name="working-with-heterogeneous-entity-types"></a><span data-ttu-id="4e549-910">Práce s typy heterogenní entit</span><span class="sxs-lookup"><span data-stu-id="4e549-910">Working with heterogeneous entity types</span></span>
<span data-ttu-id="4e549-911">Služba Table je *bez schématu* tabulky úložiště, která znamená, že na jednotlivé tabulky můžete ukládat entity více typů poskytuje flexibilitu při návrhu.</span><span class="sxs-lookup"><span data-stu-id="4e549-911">The Table service is a *schema-less* table store that means that a single table can store entities of multiple types providing great flexibility in your design.</span></span> <span data-ttu-id="4e549-912">Následující příklad ilustruje tabulku ukládání zaměstnanců a entity oddělení:</span><span class="sxs-lookup"><span data-stu-id="4e549-912">The following example illustrates a table storing both employee and department entities:</span></span>  

<table>
<tr>
<th><span data-ttu-id="4e549-913">Klíč oddílu</span><span class="sxs-lookup"><span data-stu-id="4e549-913">PartitionKey</span></span></th>
<th><span data-ttu-id="4e549-914">RowKey</span><span class="sxs-lookup"><span data-stu-id="4e549-914">RowKey</span></span></th>
<th><span data-ttu-id="4e549-915">časové razítko</span><span class="sxs-lookup"><span data-stu-id="4e549-915">Timestamp</span></span></th>
<th></th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th><span data-ttu-id="4e549-916">FirstName</span><span class="sxs-lookup"><span data-stu-id="4e549-916">FirstName</span></span></th>
<th><span data-ttu-id="4e549-917">Příjmení</span><span class="sxs-lookup"><span data-stu-id="4e549-917">LastName</span></span></th>
<th><span data-ttu-id="4e549-918">Věk</span><span class="sxs-lookup"><span data-stu-id="4e549-918">Age</span></span></th>
<th><span data-ttu-id="4e549-919">E-mail</span><span class="sxs-lookup"><span data-stu-id="4e549-919">Email</span></span></th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th><span data-ttu-id="4e549-920">FirstName</span><span class="sxs-lookup"><span data-stu-id="4e549-920">FirstName</span></span></th>
<th><span data-ttu-id="4e549-921">Příjmení</span><span class="sxs-lookup"><span data-stu-id="4e549-921">LastName</span></span></th>
<th><span data-ttu-id="4e549-922">Věk</span><span class="sxs-lookup"><span data-stu-id="4e549-922">Age</span></span></th>
<th><span data-ttu-id="4e549-923">E-mail</span><span class="sxs-lookup"><span data-stu-id="4e549-923">Email</span></span></th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th><span data-ttu-id="4e549-924">DepartmentName</span><span class="sxs-lookup"><span data-stu-id="4e549-924">DepartmentName</span></span></th>
<th><span data-ttu-id="4e549-925">EmployeeCount</span><span class="sxs-lookup"><span data-stu-id="4e549-925">EmployeeCount</span></span></th>
</tr>
<tr>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th><span data-ttu-id="4e549-926">FirstName</span><span class="sxs-lookup"><span data-stu-id="4e549-926">FirstName</span></span></th>
<th><span data-ttu-id="4e549-927">Příjmení</span><span class="sxs-lookup"><span data-stu-id="4e549-927">LastName</span></span></th>
<th><span data-ttu-id="4e549-928">Věk</span><span class="sxs-lookup"><span data-stu-id="4e549-928">Age</span></span></th>
<th><span data-ttu-id="4e549-929">E-mail</span><span class="sxs-lookup"><span data-stu-id="4e549-929">Email</span></span></th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
</table>

<span data-ttu-id="4e549-930">Všimněte si, že každá entita musí mít dál **PartitionKey**, **RowKey**, a **časové razítko** hodnoty, ale mohou mít všechny sadu vlastností.</span><span class="sxs-lookup"><span data-stu-id="4e549-930">Note that each entity must still have **PartitionKey**, **RowKey**, and **Timestamp** values, but may have any set of properties.</span></span> <span data-ttu-id="4e549-931">Kromě toho není nic k označení typu entity, pokud se rozhodnete ukládat někde tyto informace.</span><span class="sxs-lookup"><span data-stu-id="4e549-931">Furthermore, there is nothing to indicate the type of an entity unless you choose to store that information somewhere.</span></span> <span data-ttu-id="4e549-932">Existují dvě možnosti pro identifikaci typ entity:</span><span class="sxs-lookup"><span data-stu-id="4e549-932">There are two options for identifying the entity type:</span></span>  

* <span data-ttu-id="4e549-933">Předřadit typ entity, který má **RowKey** (nebo případně **PartitionKey**).</span><span class="sxs-lookup"><span data-stu-id="4e549-933">Prepend the entity type to the **RowKey** (or possibly the **PartitionKey**).</span></span> <span data-ttu-id="4e549-934">Například **EMPLOYEE_000123** nebo **DEPARTMENT_SALES** jako **RowKey** hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4e549-934">For example, **EMPLOYEE_000123** or **DEPARTMENT_SALES** as **RowKey** values.</span></span>  
* <span data-ttu-id="4e549-935">Použijte samostatné vlastnost k zaznamenání typ entity, jak je znázorněno v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="4e549-935">Use a separate property to record the entity type as shown in the table below.</span></span>  

<table>
<tr>
<th><span data-ttu-id="4e549-936">Klíč oddílu</span><span class="sxs-lookup"><span data-stu-id="4e549-936">PartitionKey</span></span></th>
<th><span data-ttu-id="4e549-937">RowKey</span><span class="sxs-lookup"><span data-stu-id="4e549-937">RowKey</span></span></th>
<th><span data-ttu-id="4e549-938">časové razítko</span><span class="sxs-lookup"><span data-stu-id="4e549-938">Timestamp</span></span></th>
<th></th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th><span data-ttu-id="4e549-939">Typ entity</span><span class="sxs-lookup"><span data-stu-id="4e549-939">EntityType</span></span></th>
<th><span data-ttu-id="4e549-940">FirstName</span><span class="sxs-lookup"><span data-stu-id="4e549-940">FirstName</span></span></th>
<th><span data-ttu-id="4e549-941">Příjmení</span><span class="sxs-lookup"><span data-stu-id="4e549-941">LastName</span></span></th>
<th><span data-ttu-id="4e549-942">Věk</span><span class="sxs-lookup"><span data-stu-id="4e549-942">Age</span></span></th>
<th><span data-ttu-id="4e549-943">E-mail</span><span class="sxs-lookup"><span data-stu-id="4e549-943">Email</span></span></th>
</tr>
<tr>
<td><span data-ttu-id="4e549-944">Zaměstnanec</span><span class="sxs-lookup"><span data-stu-id="4e549-944">Employee</span></span></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th><span data-ttu-id="4e549-945">Typ entity</span><span class="sxs-lookup"><span data-stu-id="4e549-945">EntityType</span></span></th>
<th><span data-ttu-id="4e549-946">FirstName</span><span class="sxs-lookup"><span data-stu-id="4e549-946">FirstName</span></span></th>
<th><span data-ttu-id="4e549-947">Příjmení</span><span class="sxs-lookup"><span data-stu-id="4e549-947">LastName</span></span></th>
<th><span data-ttu-id="4e549-948">Věk</span><span class="sxs-lookup"><span data-stu-id="4e549-948">Age</span></span></th>
<th><span data-ttu-id="4e549-949">E-mail</span><span class="sxs-lookup"><span data-stu-id="4e549-949">Email</span></span></th>
</tr>
<tr>
<td><span data-ttu-id="4e549-950">Zaměstnanec</span><span class="sxs-lookup"><span data-stu-id="4e549-950">Employee</span></span></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th><span data-ttu-id="4e549-951">Typ entity</span><span class="sxs-lookup"><span data-stu-id="4e549-951">EntityType</span></span></th>
<th><span data-ttu-id="4e549-952">DepartmentName</span><span class="sxs-lookup"><span data-stu-id="4e549-952">DepartmentName</span></span></th>
<th><span data-ttu-id="4e549-953">EmployeeCount</span><span class="sxs-lookup"><span data-stu-id="4e549-953">EmployeeCount</span></span></th>
</tr>
<tr>
<td><span data-ttu-id="4e549-954">Oddělení</span><span class="sxs-lookup"><span data-stu-id="4e549-954">Department</span></span></td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th><span data-ttu-id="4e549-955">Typ entity</span><span class="sxs-lookup"><span data-stu-id="4e549-955">EntityType</span></span></th>
<th><span data-ttu-id="4e549-956">FirstName</span><span class="sxs-lookup"><span data-stu-id="4e549-956">FirstName</span></span></th>
<th><span data-ttu-id="4e549-957">Příjmení</span><span class="sxs-lookup"><span data-stu-id="4e549-957">LastName</span></span></th>
<th><span data-ttu-id="4e549-958">Věk</span><span class="sxs-lookup"><span data-stu-id="4e549-958">Age</span></span></th>
<th><span data-ttu-id="4e549-959">E-mail</span><span class="sxs-lookup"><span data-stu-id="4e549-959">Email</span></span></th>
</tr>
<tr>
<td><span data-ttu-id="4e549-960">Zaměstnanec</span><span class="sxs-lookup"><span data-stu-id="4e549-960">Employee</span></span></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
</table>

<span data-ttu-id="4e549-961">První možnost předřazení entity typ, který má **RowKey**, je užitečné, pokud je možné, že dvě entity různých typů může mít stejnou hodnotu klíče.</span><span class="sxs-lookup"><span data-stu-id="4e549-961">The first option, prepending the entity type to the **RowKey**, is useful if there is a possibility that two entities of different types might have the same key value.</span></span> <span data-ttu-id="4e549-962">Skupiny také entity stejného typu společně v oddílu.</span><span class="sxs-lookup"><span data-stu-id="4e549-962">It also groups entities of the same type together in the partition.</span></span>  

<span data-ttu-id="4e549-963">Technik popsaných v této části jsou obzvláště důležité v diskusi [vztahy dědičnosti](#inheritance-relationships) výše v tomto průvodci v části [modelování vztahů](#modelling-relationships).</span><span class="sxs-lookup"><span data-stu-id="4e549-963">The techniques discussed in this section are especially relevant to the discussion [Inheritance relationships](#inheritance-relationships) earlier in this guide in the section [Modelling relationships](#modelling-relationships).</span></span>  

> [!NOTE]
> <span data-ttu-id="4e549-964">Měli byste zvážit, včetně číslo verze v hodnotě typ entity povolit klientské aplikace momentální objektů POCO a pracovat s různými verzemi.</span><span class="sxs-lookup"><span data-stu-id="4e549-964">You should consider including a version number in the entity type value to enable client applications to evolve POCO objects and work with different versions.</span></span>  
> 
> 

<span data-ttu-id="4e549-965">Zbývající část Tato část popisuje některé z funkcí v Klientská knihovna pro úložiště, které usnadňují práci s více typy entit ve stejné tabulce.</span><span class="sxs-lookup"><span data-stu-id="4e549-965">The remainder of this section describes some of the features in the Storage Client Library that facilitate working with multiple entity types in the same table.</span></span>  

#### <a name="retrieving-heterogeneous-entity-types"></a><span data-ttu-id="4e549-966">Načítání typy heterogenní entit</span><span class="sxs-lookup"><span data-stu-id="4e549-966">Retrieving heterogeneous entity types</span></span>
<span data-ttu-id="4e549-967">Pokud používáte Klientská knihovna pro úložiště, máte tři možnosti pro práci s více typy entit.</span><span class="sxs-lookup"><span data-stu-id="4e549-967">If you are using the Storage Client Library, you have three options for working with multiple entity types.</span></span>  

<span data-ttu-id="4e549-968">Pokud znáte typ entity s konkrétní **RowKey** a **PartitionKey** hodnoty, potom můžete zadat typ entity při načtení entity, jak je uvedeno v předchozí dva příklady, které načtení entity typu **EmployeeEntity**: [provádění dotazu bodu pomocí klientské knihovny pro úložiště](#executing-a-point-query-using-the-storage-client-library) a [načítání více entit pomocí LINQ](#retrieving-multiple-entities-using-linq).</span><span class="sxs-lookup"><span data-stu-id="4e549-968">If you know the type of the entity stored with a specific **RowKey** and **PartitionKey** values, then you can specify the entity type when you retrieve the entity as shown in the previous two examples that retrieve entities of type **EmployeeEntity**: [Executing a point query using the Storage Client Library](#executing-a-point-query-using-the-storage-client-library) and [Retrieving multiple entities using LINQ](#retrieving-multiple-entities-using-linq).</span></span>  

<span data-ttu-id="4e549-969">Druhou možností je používat **DynamicTableEntity** typu (kontejner objektů) namísto konkrétní typ entity objektů POCO (Tato možnost může také zvýšit výkon, protože není nutné k serializaci a deserializaci entity, která má typy .NET).</span><span class="sxs-lookup"><span data-stu-id="4e549-969">The second option is to use the **DynamicTableEntity** type (a property bag) instead of a concrete POCO entity type (this option may also improve performance because there is no need to serialize and deserialize the entity to .NET types).</span></span> <span data-ttu-id="4e549-970">Následující kód C# potenciálně načte více entit různých typů z tabulky, ale vrací všechny entity jako **DynamicTableEntity** instance.</span><span class="sxs-lookup"><span data-stu-id="4e549-970">The following C# code potentially retrieves multiple entities of different types from the table, but returns all entities as **DynamicTableEntity** instances.</span></span> <span data-ttu-id="4e549-971">Poté použije **EntityType** vlastnosti k určení typu Každá entita:</span><span class="sxs-lookup"><span data-stu-id="4e549-971">It then uses the **EntityType** property to determine the type of each entity:</span></span>  

```csharp
string filter = TableQuery.CombineFilters(
    TableQuery.GenerateFilterCondition("PartitionKey",
    QueryComparisons.Equal, "Sales"),
    TableOperators.And,
    TableQuery.CombineFilters(
    TableQuery.GenerateFilterCondition("RowKey",
                    QueryComparisons.GreaterThanOrEqual, "B"),
        TableOperators.And,
        TableQuery.GenerateFilterCondition("RowKey",
        QueryComparisons.LessThan, "F")
    )
);
TableQuery<DynamicTableEntity> entityQuery =
    new TableQuery<DynamicTableEntity>().Where(filter);
var employees = employeeTable.ExecuteQuery(entityQuery);

IEnumerable<DynamicTableEntity> entities = employeeTable.ExecuteQuery(entityQuery);
foreach (var e in entities)
{
EntityProperty entityTypeProperty;
if (e.Properties.TryGetValue("EntityType", out entityTypeProperty))
{
    if (entityTypeProperty.StringValue == "Employee")
    {
        // Use entityTypeProperty, RowKey, PartitionKey, Etag, and Timestamp
        }
    }
}  
```

<span data-ttu-id="4e549-972">Všimněte si, že pro načtení další vlastnosti, je nutné použít **TryGetValue** metodu **vlastnosti** vlastnost **DynamicTableEntity** – třída.</span><span class="sxs-lookup"><span data-stu-id="4e549-972">Note that to retrieve other properties you must use the **TryGetValue** method on the **Properties** property of the **DynamicTableEntity** class.</span></span>  

<span data-ttu-id="4e549-973">Třetí možnost je kombinovat pomocí **DynamicTableEntity** typu a **EntityResolver** instance.</span><span class="sxs-lookup"><span data-stu-id="4e549-973">A third option is to combine using the **DynamicTableEntity** type and an **EntityResolver** instance.</span></span> <span data-ttu-id="4e549-974">Díky tomu můžete použít k řešení pro více typů objektů POCO ve stejném dotazu.</span><span class="sxs-lookup"><span data-stu-id="4e549-974">This enables you to resolve to multiple POCO types in the same query.</span></span> <span data-ttu-id="4e549-975">V tomto příkladu **EntityResolver** delegáta je pomocí **EntityType** vlastnost k rozlišení mezi těmito dvěma typy entit, který vrací dotaz.</span><span class="sxs-lookup"><span data-stu-id="4e549-975">In this example, the **EntityResolver** delegate is using the **EntityType** property to distinguish between the two types of entity that the query returns.</span></span> <span data-ttu-id="4e549-976">**Vyřešit** metoda používá **překladač** delegáta vyřešit **DynamicTableEntity** instance na **TableEntity** instance.</span><span class="sxs-lookup"><span data-stu-id="4e549-976">The **Resolve** method uses the **resolver** delegate to resolve **DynamicTableEntity** instances to **TableEntity** instances.</span></span>  

```csharp
EntityResolver<TableEntity> resolver = (pk, rk, ts, props, etag) =>
{

        TableEntity resolvedEntity = null;
        if (props["EntityType"].StringValue == "Department")
        {
        resolvedEntity = new DepartmentEntity();
        }
        else if (props["EntityType"].StringValue == "Employee")
        {
        resolvedEntity = new EmployeeEntity();
        }
        else throw new ArgumentException("Unrecognized entity", "props");

        resolvedEntity.PartitionKey = pk;
        resolvedEntity.RowKey = rk;
        resolvedEntity.Timestamp = ts;
        resolvedEntity.ETag = etag;
        resolvedEntity.ReadEntity(props, null);
        return resolvedEntity;
};

string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
TableQuery<DynamicTableEntity> entityQuery =
        new TableQuery<DynamicTableEntity>().Where(filter);

var entities = employeeTable.ExecuteQuery(entityQuery, resolver);
foreach (var e in entities)
{
        if (e is DepartmentEntity)
        {
    ...
        }
        if (e is EmployeeEntity)
        {
    ...
        }
}  
```

#### <a name="modifying-heterogeneous-entity-types"></a><span data-ttu-id="4e549-977">Úprava typy heterogenní entit</span><span class="sxs-lookup"><span data-stu-id="4e549-977">Modifying heterogeneous entity types</span></span>
<span data-ttu-id="4e549-978">Není potřeba znát typ entity ho odstranit a při vložení vždy znát typ entity.</span><span class="sxs-lookup"><span data-stu-id="4e549-978">You do not need to know the type of an entity to delete it, and you always know the type of an entity when you insert it.</span></span> <span data-ttu-id="4e549-979">Můžete však použít **DynamicTableEntity** typu chcete entitu aktualizovat, aniž by věděly, její typ a bez použití třídu entity objektů POCO.</span><span class="sxs-lookup"><span data-stu-id="4e549-979">However, you can use **DynamicTableEntity** type to update an entity without knowing its type and without using a POCO entity class.</span></span> <span data-ttu-id="4e549-980">Následující ukázka kódu načte jednu entitu a zkontroluje **EmployeeCount** existuje vlastnost před aktualizací.</span><span class="sxs-lookup"><span data-stu-id="4e549-980">The following code sample retrieves a single entity, and checks the **EmployeeCount** property exists before updating it.</span></span>  

```csharp
TableResult result =
        employeeTable.Execute(TableOperation.Retrieve(partitionKey, rowKey));
DynamicTableEntity department = (DynamicTableEntity)result.Result;

EntityProperty countProperty;

if (!department.Properties.TryGetValue("EmployeeCount", out countProperty))
{
        throw new
        InvalidOperationException("Invalid entity, EmployeeCount property not found.");
}
countProperty.Int32Value += 1;
employeeTable.Execute(TableOperation.Merge(department));  
```

### <a name="controlling-access-with-shared-access-signatures"></a><span data-ttu-id="4e549-981">Řízení přístupu s podpisy sdíleného přístupu</span><span class="sxs-lookup"><span data-stu-id="4e549-981">Controlling access with Shared Access Signatures</span></span>
<span data-ttu-id="4e549-982">Tokeny sdíleného přístupového podpisu (SAS) můžete povolit klientské aplikace upravit (a dotazovat) entity tabulky přímo, aniž by bylo nutné přímé ověření u služby table.</span><span class="sxs-lookup"><span data-stu-id="4e549-982">You can use Shared Access Signature (SAS) tokens to enable client applications to modify (and query) table entities directly without the need to authenticate directly with the table service.</span></span> <span data-ttu-id="4e549-983">Obvykle jsou tři hlavní výhody použití SAS ve vaší aplikaci:</span><span class="sxs-lookup"><span data-stu-id="4e549-983">Typically, there are three main benefits to using SAS in your application:</span></span>  

* <span data-ttu-id="4e549-984">Není nutné distribuovat klíč účtu úložiště do nezabezpečené platformy (například mobilní zařízení) Chcete-li povolit toto zařízení k přístupu a úpravě entity ve službě Table.</span><span class="sxs-lookup"><span data-stu-id="4e549-984">You do not need to distribute your storage account key to an insecure platform (such as a mobile device) in order to allow that device to access and modify entities in the Table service.</span></span>  
* <span data-ttu-id="4e549-985">Snižování zátěže některé úkoly, které provádějí webové a pracovní role ve správě vaší entity do klientských zařízení, jako je například počítačích koncových uživatelů a mobilních zařízení.</span><span class="sxs-lookup"><span data-stu-id="4e549-985">You can offload some of the work that web and worker roles perform in managing your entities to client devices such as end-user computers and mobile devices.</span></span>  
* <span data-ttu-id="4e549-986">Můžete přiřadit omezené a čas omezenou sadu oprávnění pro klienta (například umožníte přístup jen pro čtení ke konkrétním prostředkům).</span><span class="sxs-lookup"><span data-stu-id="4e549-986">You can assign a constrained and time limited set of permissions to a client (such as allowing read-only access to specific resources).</span></span>  

<span data-ttu-id="4e549-987">Další informace o používání tokeny SAS s služby Table najdete v tématu [pomocí sdíleného přístupového podpisy (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md).</span><span class="sxs-lookup"><span data-stu-id="4e549-987">For more information about using SAS tokens with the Table service, see [Using Shared Access Signatures (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md).</span></span>  

<span data-ttu-id="4e549-988">Však musí i nadále generovat tokeny SAS, která udělují klientskou aplikaci pro entity ve službě table: bude třeba provést v prostředí, které má zabezpečený přístup k klíče účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="4e549-988">However, you must still generate the SAS tokens that grant a client application to the entities in the table service: you should do this in an environment that has secure access to your storage account keys.</span></span> <span data-ttu-id="4e549-989">Obvykle použijete roli web nebo worker generovat tokeny SAS a doručit do klientských aplikací, které potřebují přístup k vaší entity.</span><span class="sxs-lookup"><span data-stu-id="4e549-989">Typically, you use a web or worker role to generate the SAS tokens and deliver them to the client applications that need access to your entities.</span></span> <span data-ttu-id="4e549-990">Protože je stále režii zahrnutých v generování a doručování tokeny SAS pro klienty, zvažte, jak nejlépe snížit tento režijní náklady, zejména v vysoký počet scénáře.</span><span class="sxs-lookup"><span data-stu-id="4e549-990">Because there is still an overhead involved in generating and delivering SAS tokens to clients, you should consider how best to reduce this overhead, especially in high-volume scenarios.</span></span>  

<span data-ttu-id="4e549-991">Je možné k vygenerování tokenu SAS, která uděluje přístup k podmnožině entity v tabulce.</span><span class="sxs-lookup"><span data-stu-id="4e549-991">It is possible to generate a SAS token that grants access to a subset of the entities in a table.</span></span> <span data-ttu-id="4e549-992">Ve výchozím nastavení, můžete vytvořit token SAS pro celou tabulku, ale je také možné určit, že tokenu SAS udělit přístup k buď řadu **PartitionKey** hodnoty nebo celou řadu **PartitionKey** a **RowKey** hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4e549-992">By default, you create a SAS token for an entire table, but it is also possible to specify that the SAS token grant access to either a range of **PartitionKey** values, or a range of **PartitionKey** and **RowKey** values.</span></span> <span data-ttu-id="4e549-993">Můžete zvolit generovat tokeny SAS pro jednotlivé uživatele systému tak, aby každý uživatel tokenu SAS pouze jim umožňuje přístup k vlastní entity ve službě table.</span><span class="sxs-lookup"><span data-stu-id="4e549-993">You might choose to generate SAS tokens for individual users of your system such that each user's SAS token only allows them access to their own entities in the table service.</span></span>  

### <a name="asynchronous-and-parallel-operations"></a><span data-ttu-id="4e549-994">Operace asynchronní a paralelní</span><span class="sxs-lookup"><span data-stu-id="4e549-994">Asynchronous and parallel operations</span></span>
<span data-ttu-id="4e549-995">Zadaný své žádosti jsou šíření mezi více oddílů, můžete zlepšit propustnost a klient odezvy pomocí asynchronní nebo paralelní dotazy.</span><span class="sxs-lookup"><span data-stu-id="4e549-995">Provided you are spreading your requests across multiple partitions, you can improve throughput and client responsiveness by using asynchronous or parallel queries.</span></span>
<span data-ttu-id="4e549-996">Například můžete mít dvě nebo více pracovních instance rolí přístup k vaší tabulky paralelně.</span><span class="sxs-lookup"><span data-stu-id="4e549-996">For example, you might have two or more worker role instances accessing your tables in parallel.</span></span> <span data-ttu-id="4e549-997">Můžete mít role jednotlivým pracovním zodpovědná za konkrétní sady oddíly, nebo jednoduše mít více instancí služby worker role, každý mít přístup všechny oddíly v tabulce.</span><span class="sxs-lookup"><span data-stu-id="4e549-997">You could have individual worker roles responsible for particular sets of partitions, or simply have multiple worker role instances, each able to access all the partitions in a table.</span></span>  

<span data-ttu-id="4e549-998">V rámci instanci klienta můžete zlepšit propustnost asynchronně provádění operace úložiště.</span><span class="sxs-lookup"><span data-stu-id="4e549-998">Within a client instance, you can improve throughput by executing storage operations asynchronously.</span></span> <span data-ttu-id="4e549-999">Klientská knihovna pro úložiště snadno zápisu asynchronní dotazy a úpravy.</span><span class="sxs-lookup"><span data-stu-id="4e549-999">The Storage Client Library makes it easy to write asynchronous queries and modifications.</span></span> <span data-ttu-id="4e549-1000">Například můžete začít s synchronní metoda, která načte všechny entity v oddílu, jak je znázorněno v následujícím kódu jazyka C#:</span><span class="sxs-lookup"><span data-stu-id="4e549-1000">For example, you might start with the synchronous method that retrieves all the entities in a partition as shown in the following C# code:</span></span>  

```csharp
private static void ManyEntitiesQuery(CloudTable employeeTable, string department)
{
        string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, department);
        TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);

        TableContinuationToken continuationToken = null;

        do
        {
        var employees = employeeTable.ExecuteQuerySegmented(
                employeeQuery, continuationToken);
        foreach (var emp in employees)
    {
        ...
    }
        continuationToken = employees.ContinuationToken;
        } while (continuationToken != null);
}  
```

<span data-ttu-id="4e549-1001">Tento kód můžete snadno upravit tak, aby dotaz asynchronně následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="4e549-1001">You can easily modify this code so that the query runs asynchronously as follows:</span></span>  

```csharp
private static async Task ManyEntitiesQueryAsync(CloudTable employeeTable, string department)
{
        string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, department);
        TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);
        TableContinuationToken continuationToken = null;

        do
        {
        var employees = await employeeTable.ExecuteQuerySegmentedAsync(
                employeeQuery, continuationToken);
        foreach (var emp in employees)
        {
            ...
        }
        continuationToken = employees.ContinuationToken;
            } while (continuationToken != null);
}  
```

<span data-ttu-id="4e549-1002">V tomto příkladu asynchronní, můžete zjistit z synchronní verze následující změny:</span><span class="sxs-lookup"><span data-stu-id="4e549-1002">In this asynchronous example, you can see the following changes from the synchronous version:</span></span>  

* <span data-ttu-id="4e549-1003">Podpis metody nyní zahrnuje **asynchronní** modifikátor a vrátí **úloh** instance.</span><span class="sxs-lookup"><span data-stu-id="4e549-1003">The method signature now includes the **async** modifier and returns a **Task** instance.</span></span>  
* <span data-ttu-id="4e549-1004">Místo volání **ExecuteSegmented** metoda a načtěte výsledky, metoda nyní volá **ExecuteSegmentedAsync** metoda a používá **await** modifikátor a načtěte výsledky asynchronně.</span><span class="sxs-lookup"><span data-stu-id="4e549-1004">Instead of calling the **ExecuteSegmented** method to retrieve results, the method now calls the **ExecuteSegmentedAsync** method and uses the **await** modifier to retrieve results asynchronously.</span></span>  

<span data-ttu-id="4e549-1005">Klientská aplikace lze tuto metodu volat vícekrát (s různými hodnotami **oddělení** parametr), a každý dotaz se spustí na samostatné vlákno.</span><span class="sxs-lookup"><span data-stu-id="4e549-1005">The client application can call this method multiple times (with different values for the **department** parameter), and each query will run on a separate thread.</span></span>  

<span data-ttu-id="4e549-1006">Všimněte si, že žádné asynchronní verzi **Execute** metoda v **TableQuery** třídy, protože **rozhraní IEnumerable** rozhraní nepodporuje asynchronní výčtu.</span><span class="sxs-lookup"><span data-stu-id="4e549-1006">Note that there is no asynchronous version of the **Execute** method in the **TableQuery** class because the **IEnumerable** interface does not support asynchronous enumeration.</span></span>  

<span data-ttu-id="4e549-1007">Můžete také vložit, aktualizovat a odstranit entity asynchronně.</span><span class="sxs-lookup"><span data-stu-id="4e549-1007">You can also insert, update, and delete entities asynchronously.</span></span> <span data-ttu-id="4e549-1008">Následující příklad jazyka C# ukazuje jednoduchý, synchronní metoda vložení nebo nahrazení entity zaměstnanců:</span><span class="sxs-lookup"><span data-stu-id="4e549-1008">The following C# example shows a simple, synchronous method to insert or replace an employee entity:</span></span>  

```csharp
private static void SimpleEmployeeUpsert(CloudTable employeeTable,
        EmployeeEntity employee)
{
        TableResult result = employeeTable
        .Execute(TableOperation.InsertOrReplace(employee));
        Console.WriteLine("HTTP Status: {0}", result.HttpStatusCode);
}  
```

<span data-ttu-id="4e549-1009">Tento kód můžete snadno upravit tak, aby aktualizaci asynchronně následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="4e549-1009">You can easily modify this code so that the update runs asynchronously as follows:</span></span>  

```csharp
private static async Task SimpleEmployeeUpsertAsync(CloudTable employeeTable,
        EmployeeEntity employee)
{
        TableResult result = await employeeTable
        .ExecuteAsync(TableOperation.InsertOrReplace(employee));
        Console.WriteLine("HTTP Status: {0}", result.HttpStatusCode);
}  
```

<span data-ttu-id="4e549-1010">V tomto příkladu asynchronní, můžete zjistit z synchronní verze následující změny:</span><span class="sxs-lookup"><span data-stu-id="4e549-1010">In this asynchronous example, you can see the following changes from the synchronous version:</span></span>  

* <span data-ttu-id="4e549-1011">Podpis metody nyní zahrnuje **asynchronní** modifikátor a vrátí **úloh** instance.</span><span class="sxs-lookup"><span data-stu-id="4e549-1011">The method signature now includes the **async** modifier and returns a **Task** instance.</span></span>  
* <span data-ttu-id="4e549-1012">Místo volání **Execute** metoda aktualizovat entitu, metoda nyní volá **ExecuteAsync** metoda a používá **await** modifikátor a načtěte výsledky asynchronně.</span><span class="sxs-lookup"><span data-stu-id="4e549-1012">Instead of calling the **Execute** method to update the entity, the method now calls the **ExecuteAsync** method and uses the **await** modifier to retrieve results asynchronously.</span></span>  

<span data-ttu-id="4e549-1013">Klientská aplikace můžete volat více asynchronní metody, jako je tato, a každé vyvolání metody se spustí na samostatné vlákno.</span><span class="sxs-lookup"><span data-stu-id="4e549-1013">The client application can call multiple asynchronous methods like this one, and each method invocation will run on a separate thread.</span></span>  

### <a name="credits"></a><span data-ttu-id="4e549-1014">Kredity</span><span class="sxs-lookup"><span data-stu-id="4e549-1014">Credits</span></span>
<span data-ttu-id="4e549-1015">Rádi bychom se Děkujeme, že následující členy týmu Azure pro jejich příspěvky: Dominic Betts, Jason Hogg, Jean Ghanem, Jai Haridas, Jeff Yields, Vamshidhar Kommineni, Vinay Shaha a Serdar Ozler stejně jako tní Hollander z Microsoft DX.</span><span class="sxs-lookup"><span data-stu-id="4e549-1015">We would like to thank the following members of the Azure team for their contributions: Dominic Betts, Jason Hogg, Jean Ghanem, Jai Haridas, Jeff Irwin, Vamshidhar Kommineni, Vinay Shah and Serdar Ozler as well as  Tom Hollander from Microsoft DX.</span></span> 

<span data-ttu-id="4e549-1016">Také rádi bychom Děkujeme, že následující Microsoft MVP je pro jejich cenné zpětné vazby během zkontrolujte cykly: Igor Papirov a Edward Bakker.</span><span class="sxs-lookup"><span data-stu-id="4e549-1016">We would also like to thank the following Microsoft MVP's for their valuable feedback during review cycles: Igor Papirov and Edward Bakker.</span></span>

[1]: ./media/storage-table-design-guide/storage-table-design-IMAGE01.png
[2]: ./media/storage-table-design-guide/storage-table-design-IMAGE02.png
[3]: ./media/storage-table-design-guide/storage-table-design-IMAGE03.png
[4]: ./media/storage-table-design-guide/storage-table-design-IMAGE04.png
[5]: ./media/storage-table-design-guide/storage-table-design-IMAGE05.png
[6]: ./media/storage-table-design-guide/storage-table-design-IMAGE06.png
[7]: ./media/storage-table-design-guide/storage-table-design-IMAGE07.png
[8]: ./media/storage-table-design-guide/storage-table-design-IMAGE08.png
[9]: ./media/storage-table-design-guide/storage-table-design-IMAGE09.png
[10]: ./media/storage-table-design-guide/storage-table-design-IMAGE10.png
[11]: ./media/storage-table-design-guide/storage-table-design-IMAGE11.png
[12]: ./media/storage-table-design-guide/storage-table-design-IMAGE12.png
[13]: ./media/storage-table-design-guide/storage-table-design-IMAGE13.png
[14]: ./media/storage-table-design-guide/storage-table-design-IMAGE14.png
[15]: ./media/storage-table-design-guide/storage-table-design-IMAGE15.png
[16]: ./media/storage-table-design-guide/storage-table-design-IMAGE16.png
[17]: ./media/storage-table-design-guide/storage-table-design-IMAGE17.png
[18]: ./media/storage-table-design-guide/storage-table-design-IMAGE18.png
[19]: ./media/storage-table-design-guide/storage-table-design-IMAGE19.png
[20]: ./media/storage-table-design-guide/storage-table-design-IMAGE20.png
[21]: ./media/storage-table-design-guide/storage-table-design-IMAGE21.png
[22]: ./media/storage-table-design-guide/storage-table-design-IMAGE22.png
[23]: ./media/storage-table-design-guide/storage-table-design-IMAGE23.png
[24]: ./media/storage-table-design-guide/storage-table-design-IMAGE24.png
[25]: ./media/storage-table-design-guide/storage-table-design-IMAGE25.png
[26]: ./media/storage-table-design-guide/storage-table-design-IMAGE26.png
[27]: ./media/storage-table-design-guide/storage-table-design-IMAGE27.png
[28]: ./media/storage-table-design-guide/storage-table-design-IMAGE28.png
[29]: ./media/storage-table-design-guide/storage-table-design-IMAGE29.png

