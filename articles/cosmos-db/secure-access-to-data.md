---
title: "Zjistěte, jak zabezpečit přístup k datům v Azure Cosmos DB | Microsoft Docs"
description: "Seznamte se s koncepty řízení přístupu v Azure DB Cosmos, včetně hlavního klíče, klíče jen pro čtení, uživatelé a oprávnění."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 8641225d-e839-4ba6-a6fd-d6314ae3a51c
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: mimig
ms.openlocfilehash: 383e04f91eec2f465b381ce30f2d6d24c488b731
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="securing-access-to-azure-cosmos-db-data"></a><span data-ttu-id="c4616-103">Zabezpečení přístupu k datům v Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="c4616-103">Securing access to Azure Cosmos DB data</span></span>
<span data-ttu-id="c4616-104">Tento článek obsahuje přehled zabezpečení přístupu k datům uloženým v [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="c4616-104">This article provides an overview of securing access to data stored in [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).</span></span>

<span data-ttu-id="c4616-105">Azure Cosmos DB používá dva typy klíčů k ověření uživatelů a poskytovat přístup k jeho datům a prostředkům.</span><span class="sxs-lookup"><span data-stu-id="c4616-105">Azure Cosmos DB uses two types of keys to authenticate users and provide access to its data and resources.</span></span> 

|<span data-ttu-id="c4616-106">Typ klíče</span><span class="sxs-lookup"><span data-stu-id="c4616-106">Key type</span></span>|<span data-ttu-id="c4616-107">Zdroje</span><span class="sxs-lookup"><span data-stu-id="c4616-107">Resources</span></span>|
|---|---|
|[<span data-ttu-id="c4616-108">Hlavní klíče</span><span class="sxs-lookup"><span data-stu-id="c4616-108">Master keys</span></span>](#master-keys) |<span data-ttu-id="c4616-109">Použít pro správu prostředků: databáze účty, databází, uživatelů a oprávnění</span><span class="sxs-lookup"><span data-stu-id="c4616-109">Used for administrative resources: database accounts, databases, users, and permissions</span></span>|
|[<span data-ttu-id="c4616-110">Tokeny prostředků</span><span class="sxs-lookup"><span data-stu-id="c4616-110">Resource tokens</span></span>](#resource-tokens)|<span data-ttu-id="c4616-111">Použít pro prostředky aplikace: kolekcí, dokumentů, přílohy, uložené procedury, triggery a UDF</span><span class="sxs-lookup"><span data-stu-id="c4616-111">Used for application resources: collections, documents, attachments, stored procedures, triggers, and UDFs</span></span>|

<a id="master-keys"></a>

## <a name="master-keys"></a><span data-ttu-id="c4616-112">Hlavní klíče</span><span class="sxs-lookup"><span data-stu-id="c4616-112">Master keys</span></span> 

<span data-ttu-id="c4616-113">Hlavní klíče poskytují přístup k všechny správce prostředky pro účet databáze.</span><span class="sxs-lookup"><span data-stu-id="c4616-113">Master keys provide access to the all the administrative resources for the database account.</span></span> <span data-ttu-id="c4616-114">Hlavní klíče:</span><span class="sxs-lookup"><span data-stu-id="c4616-114">Master keys:</span></span>  
- <span data-ttu-id="c4616-115">Poskytují přístup k účtům, databází, uživatelů a oprávnění.</span><span class="sxs-lookup"><span data-stu-id="c4616-115">Provide access to accounts, databases, users, and permissions.</span></span> 
- <span data-ttu-id="c4616-116">Nelze použít k poskytování granulární přístupu do kolekcí a dokumenty.</span><span class="sxs-lookup"><span data-stu-id="c4616-116">Cannot be used to provide granular access to collections and documents.</span></span>
- <span data-ttu-id="c4616-117">Při vytváření účtu se vytvoří.</span><span class="sxs-lookup"><span data-stu-id="c4616-117">Are created during the creation of an account.</span></span>
- <span data-ttu-id="c4616-118">Může být kdykoli znovu vygenerovat.</span><span class="sxs-lookup"><span data-stu-id="c4616-118">Can be regenerated at any time.</span></span>

<span data-ttu-id="c4616-119">Každý účet se skládá ze dvou hlavních klíčů: primární klíč a sekundární klíč.</span><span class="sxs-lookup"><span data-stu-id="c4616-119">Each account consists of two Master keys: a primary key and secondary key.</span></span> <span data-ttu-id="c4616-120">Účelem dva klíče je, abyste mohli znovu vygenerovat nebo vrátit klíče, zajištění nepřetržité přístupu pro váš účet a data.</span><span class="sxs-lookup"><span data-stu-id="c4616-120">The purpose of dual keys is so that you can regenerate, or roll keys, providing continuous access to your account and data.</span></span> 

<span data-ttu-id="c4616-121">Kromě dva hlavní klíče pro účet Cosmos DB existují dva klíče jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="c4616-121">In addition to the two master keys for the Cosmos DB account, there are two read-only keys.</span></span> <span data-ttu-id="c4616-122">Tyto klíče jen pro čtení povolí jenom operace čtení pro účet.</span><span class="sxs-lookup"><span data-stu-id="c4616-122">These read-only keys only allow read operations on the account.</span></span> <span data-ttu-id="c4616-123">Neposkytuje přístup ke čtení prostředků oprávnění jen pro čtení klíče.</span><span class="sxs-lookup"><span data-stu-id="c4616-123">Read-only keys do not provide access to read permissions resources.</span></span>

<span data-ttu-id="c4616-124">Primární, sekundární, jen pro čtení, a pro čtení a zápis hlavního klíče se dá načíst a znovu vytvořit pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c4616-124">Primary, secondary, read only, and read-write master keys can be retrieved and regenerated using the Azure portal.</span></span> <span data-ttu-id="c4616-125">Pokyny najdete v tématu [zobrazení, kopírování a opětovné vytváření přístupových klíčů](manage-account.md#keys).</span><span class="sxs-lookup"><span data-stu-id="c4616-125">For instructions, see [View, copy, and regenerate access keys](manage-account.md#keys).</span></span>

![Řízení přístupu (IAM) na portálu Azure – ukázka zabezpečení databáze NoSQL](./media/secure-access-to-data/nosql-database-security-master-key-portal.png)

<span data-ttu-id="c4616-127">Proces otáčení hlavní klíč je jednoduché.</span><span class="sxs-lookup"><span data-stu-id="c4616-127">The process of rotating your master key is simple.</span></span> <span data-ttu-id="c4616-128">Přejděte na portál Azure k načtení sekundární klíč a potom primární klíč nahradit sekundární klíč v aplikaci a potom otočit primární klíč v portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c4616-128">Navigate to the Azure portal to retrieve your secondary key, then replace your primary key with your secondary key in your application, then rotate the primary key in the Azure portal.</span></span>

![Otočení hlavního klíče na portálu Azure – ukázka zabezpečení databáze NoSQL](./media/secure-access-to-data/nosql-database-security-master-key-rotate-workflow.png)

### <a name="code-sample-to-use-a-master-key"></a><span data-ttu-id="c4616-130">Ukázka kódu pomocí hlavního klíče</span><span class="sxs-lookup"><span data-stu-id="c4616-130">Code sample to use a master key</span></span>

<span data-ttu-id="c4616-131">Následující příklad kódu ukazuje, jak používat koncový bod účtu Cosmos DB a hlavní klíč k vytváření instancí DocumentClient a vytvořit databázi.</span><span class="sxs-lookup"><span data-stu-id="c4616-131">The following code sample illustrates how to use a Cosmos DB account endpoint and master key to instantiate a DocumentClient and create a database.</span></span> 

```csharp
//Read the Azure Cosmos DB endpointUrl and authorization keys from config.
//These values are available from the Azure portal on the Azure Cosmos DB account blade under "Keys".
//NB > Keep these values in a safe and secure location. Together they provide Administrative access to your DocDB account.

private static readonly string endpointUrl = ConfigurationManager.AppSettings["EndPointUrl"];
private static readonly SecureString authorizationKey = ToSecureString(ConfigurationManager.AppSettings["AuthorizationKey"]);

client = new DocumentClient(new Uri(endpointUrl), authorizationKey);

// Create Database
Database database = await client.CreateDatabaseAsync(
    new Database
    {
        Id = databaseName
    });
```

<a id="resource-tokens"></a>

## <a name="resource-tokens"></a><span data-ttu-id="c4616-132">Tokeny prostředků</span><span class="sxs-lookup"><span data-stu-id="c4616-132">Resource tokens</span></span>

<span data-ttu-id="c4616-133">Tokeny prostředků poskytovat přístup k prostředkům aplikace v databázi.</span><span class="sxs-lookup"><span data-stu-id="c4616-133">Resource tokens provide access to the application resources within a database.</span></span> <span data-ttu-id="c4616-134">Tokeny prostředků:</span><span class="sxs-lookup"><span data-stu-id="c4616-134">Resource tokens:</span></span>
- <span data-ttu-id="c4616-135">Zadejte přístup ke konkrétní kolekce, klíče oddílů, dokumenty, přílohy, uložené procedury, triggery a UDF.</span><span class="sxs-lookup"><span data-stu-id="c4616-135">Provide access to specific collections, partition keys, documents, attachments, stored procedures, triggers, and UDFs.</span></span>
- <span data-ttu-id="c4616-136">Když jsou vytvořeny [uživatele](#users) jsou udělena [oprávnění](#permissions) pro konkrétní prostředek.</span><span class="sxs-lookup"><span data-stu-id="c4616-136">Are created when a [user](#users) is granted [permissions](#permissions) to a specific resource.</span></span>
- <span data-ttu-id="c4616-137">Jsou vytvořeny při prostředek oprávnění je reagovali na ni na při volání metody POST, GET a PUT.</span><span class="sxs-lookup"><span data-stu-id="c4616-137">Are recreated when a permission resource is acted upon on by POST, GET, or PUT call.</span></span>
- <span data-ttu-id="c4616-138">Použijte token prostředků hash vytvořená speciálně pro uživatele, prostředků a oprávnění.</span><span class="sxs-lookup"><span data-stu-id="c4616-138">Use a hash resource token specifically constructed for the user, resource, and permission.</span></span>
- <span data-ttu-id="c4616-139">Když vázán s dobou platnosti přizpůsobitelné.</span><span class="sxs-lookup"><span data-stu-id="c4616-139">Are time bound with a customizable validity period.</span></span> <span data-ttu-id="c4616-140">Výchozí platný časový interval je jedna hodina.</span><span class="sxs-lookup"><span data-stu-id="c4616-140">The default valid timespan is one hour.</span></span> <span data-ttu-id="c4616-141">Životnost tokenu, ale lze explicitně zadat až pět hodin.</span><span class="sxs-lookup"><span data-stu-id="c4616-141">Token lifetime, however, may be explicitly specified, up to a maximum of five hours.</span></span>
- <span data-ttu-id="c4616-142">Zadejte jinou bezpečné alternativu k poskytnutí hlavního klíče.</span><span class="sxs-lookup"><span data-stu-id="c4616-142">Provide a safe alternative to giving out the master key.</span></span> 
- <span data-ttu-id="c4616-143">Povolte klientům číst, zapisovat a odstraňte prostředky v účtu Cosmos DB podle oprávnění, která jste uděleno.</span><span class="sxs-lookup"><span data-stu-id="c4616-143">Enable clients to read, write, and delete resources in the Cosmos DB account according to the permissions they've been granted.</span></span>

<span data-ttu-id="c4616-144">Token prostředku můžete použít (vytvořením Cosmos DB uživatele a oprávnění) Pokud chcete poskytnout přístup k prostředkům ve vašem účtu Cosmos DB klientovi, který nemůže být považován za důvěryhodný hlavní klíč.</span><span class="sxs-lookup"><span data-stu-id="c4616-144">You can use a resource token (by creating Cosmos DB users and permissions) when you want to provide access to resources in your Cosmos DB account to a client that cannot be trusted with the master key.</span></span>  

<span data-ttu-id="c4616-145">Tokeny prostředků cosmos DB zadejte bezpečné alternativy, která umožňuje klientům čtení, zápisu a odstraňování prostředky ve vašem účtu Cosmos DB podle oprávnění, která jste udělena a nevyžaduje pro hlavní nebo klíče jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="c4616-145">Cosmos DB resource tokens provide a safe alternative that enables clients to read, write, and delete resources in your Cosmos DB account according to the permissions you've granted, and without need for either a master or read only key.</span></span>

<span data-ttu-id="c4616-146">Zde je typické návrhový vzor, kterým může požadovaný prostředek tokeny a způsob jejich generované a předáním klientům:</span><span class="sxs-lookup"><span data-stu-id="c4616-146">Here is a typical design pattern whereby resource tokens may be requested, generated, and delivered to clients:</span></span>

1. <span data-ttu-id="c4616-147">Střední vrstvě služby jsou nastaveny na mobilní aplikaci pro sdílení fotografií uživatele.</span><span class="sxs-lookup"><span data-stu-id="c4616-147">A mid-tier service is set up to serve a mobile application to share user photos.</span></span> 
2. <span data-ttu-id="c4616-148">Střední vrstvě služby má hlavní klíč účtu Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="c4616-148">The mid-tier service possesses the master key of the Cosmos DB account.</span></span>
3. <span data-ttu-id="c4616-149">Fotografie aplikace je nainstalovaná na mobilní zařízení koncového uživatele.</span><span class="sxs-lookup"><span data-stu-id="c4616-149">The photo app is installed on end-user mobile devices.</span></span> 
4. <span data-ttu-id="c4616-150">Přihlašování vytvoří aplikace fotografie identitu uživatele u služby střední vrstvě.</span><span class="sxs-lookup"><span data-stu-id="c4616-150">On login, the photo app establishes the identity of the user with the mid-tier service.</span></span> <span data-ttu-id="c4616-151">Tento mechanismus identity zařízení je čistě až aplikace.</span><span class="sxs-lookup"><span data-stu-id="c4616-151">This mechanism of identity establishment is purely up to the application.</span></span>
5. <span data-ttu-id="c4616-152">Po vytvoření identity služby střední vrstvě požadavků oprávnění na základě identity.</span><span class="sxs-lookup"><span data-stu-id="c4616-152">Once the identity is established, the mid-tier service requests permissions based on the identity.</span></span>
6. <span data-ttu-id="c4616-153">Střední vrstvě služby odešle token prostředků zpátky na telefonní aplikace.</span><span class="sxs-lookup"><span data-stu-id="c4616-153">The mid-tier service sends a resource token back to the phone app.</span></span>
7. <span data-ttu-id="c4616-154">Telefonní aplikace můžete nadále používat token prostředku přímý přístup k prostředkům Cosmos DB s oprávněními definována token prostředku a pro interval povolený token prostředku.</span><span class="sxs-lookup"><span data-stu-id="c4616-154">The phone app can continue to use the resource token to directly access Cosmos DB resources with the permissions defined by the resource token and for the interval allowed by the resource token.</span></span> 
8. <span data-ttu-id="c4616-155">Když vyprší platnost token prostředku, přijímat další požadavky 401 neoprávněný výjimka.</span><span class="sxs-lookup"><span data-stu-id="c4616-155">When the resource token expires, subsequent requests receive a 401 unauthorized exception.</span></span>  <span data-ttu-id="c4616-156">V tomto okamžiku telefonní aplikaci znovu naváže identitu a vyžádá nový token prostředku.</span><span class="sxs-lookup"><span data-stu-id="c4616-156">At this point, the phone app re-establishes the identity and requests a new resource token.</span></span>

    ![Prostředek Azure Cosmos DB tokeny pracovního postupu](./media/secure-access-to-data/resourcekeyworkflow.png)

<span data-ttu-id="c4616-158">Generování tokenů prostředků a správy se zpracovává souborem nativní knihovny klienta Cosmos DB; ale pokud používáte REST je nutné vytvořit hlavičky požadavku nebo ověřování.</span><span class="sxs-lookup"><span data-stu-id="c4616-158">Resource token generation and management is handled by the native Cosmos DB client libraries; however, if you use REST you must construct the request/authentication headers.</span></span> <span data-ttu-id="c4616-159">Další informace o vytváření ověřovacím hlavičkám pro REST naleznete v tématu [řízení přístupu u prostředků DB Cosmos](https://docs.microsoft.com/rest/api/documentdb/access-control-on-documentdb-resources) nebo [zdrojový kód pro naše sady SDK](https://github.com/Azure/azure-documentdb-node/blob/master/source/lib/auth.js).</span><span class="sxs-lookup"><span data-stu-id="c4616-159">For more information on creating authentication headers for REST, see [Access Control on Cosmos DB Resources](https://docs.microsoft.com/rest/api/documentdb/access-control-on-documentdb-resources) or the [source code for our SDKs](https://github.com/Azure/azure-documentdb-node/blob/master/source/lib/auth.js).</span></span>

<span data-ttu-id="c4616-160">Příklad ke generování nebo zprostředkovatel prostředků tokeny služby střední vrstvy, naleznete v části [ResourceTokenBroker aplikace](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin/UserItems/ResourceTokenBroker/ResourceTokenBroker/Controllers).</span><span class="sxs-lookup"><span data-stu-id="c4616-160">For an example of a middle tier service used to generate or broker resource tokens, see the [ResourceTokenBroker app](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin/UserItems/ResourceTokenBroker/ResourceTokenBroker/Controllers).</span></span>

<a id="users"></a>

## <a name="users"></a><span data-ttu-id="c4616-161">Uživatelé</span><span class="sxs-lookup"><span data-stu-id="c4616-161">Users</span></span>
<span data-ttu-id="c4616-162">Uživatelé DB cosmos jsou přidruženy k databázi Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="c4616-162">Cosmos DB users are associated with a Cosmos DB database.</span></span>  <span data-ttu-id="c4616-163">Každá databáze může obsahovat nula nebo více uživatelů Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="c4616-163">Each database can contain zero or more Cosmos DB users.</span></span>  <span data-ttu-id="c4616-164">Následující příklad kódu ukazuje, jak vytvořit prostředek uživatele Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="c4616-164">The following code sample shows how to create a Cosmos DB user resource.</span></span>

```csharp
//Create a user.
User docUser = new User
{
    Id = "mobileuser"
};

docUser = await client.CreateUserAsync(UriFactory.CreateDatabaseUri("db"), docUser);
```

> [!NOTE]
> <span data-ttu-id="c4616-165">Každý uživatel Cosmos DB má PermissionsLink vlastnost, která slouží k načtení seznamu [oprávnění](#permissions) přidružené k uživateli.</span><span class="sxs-lookup"><span data-stu-id="c4616-165">Each Cosmos DB user has a PermissionsLink property that can be used to retrieve the list of [permissions](#permissions) associated with the user.</span></span>
> 
> 

<a id="permissions"></a>

## <a name="permissions"></a><span data-ttu-id="c4616-166">Oprávnění</span><span class="sxs-lookup"><span data-stu-id="c4616-166">Permissions</span></span>
<span data-ttu-id="c4616-167">Prostředek Cosmos DB oprávnění je přidružen uživatele Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="c4616-167">A Cosmos DB permission resource is associated with a Cosmos DB user.</span></span>  <span data-ttu-id="c4616-168">Každý uživatel může obsahovat nula nebo více oprávnění Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="c4616-168">Each user may contain zero or more Cosmos DB permissions.</span></span>  <span data-ttu-id="c4616-169">Prostředek oprávnění poskytuje přístup k tokenu zabezpečení, který uživatel musí při pokusu o přístup k prostředku konkrétní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c4616-169">A permission resource provides access to a security token that the user needs when trying to access a specific application resource.</span></span>
<span data-ttu-id="c4616-170">Existují dvě úrovně přístupu k dispozici, které může být k dispozici prostředek oprávnění:</span><span class="sxs-lookup"><span data-stu-id="c4616-170">There are two available access levels that may be provided by a permission resource:</span></span>

* <span data-ttu-id="c4616-171">All: Uživatel má úplná oprávnění k prostředku.</span><span class="sxs-lookup"><span data-stu-id="c4616-171">All: The user has full permission on the resource.</span></span>
* <span data-ttu-id="c4616-172">Přečtěte si: Uživatel může číst pouze obsah prostředku, ale nemůže provádět zápis, operace aktualizace nebo odstranění na prostředku.</span><span class="sxs-lookup"><span data-stu-id="c4616-172">Read: The user can only read the contents of the resource but cannot perform write, update, or delete operations on the resource.</span></span>

> [!NOTE]
> <span data-ttu-id="c4616-173">Aby bylo možné spustit Cosmos DB uložené procedury, uživatel musí mít oprávnění All na kolekci, ve kterém se spustí uloženou proceduru.</span><span class="sxs-lookup"><span data-stu-id="c4616-173">In order to run Cosmos DB stored procedures the user must have the All permission on the collection in which the stored procedure will be run.</span></span>
> 
> 

### <a name="code-sample-to-create-permission"></a><span data-ttu-id="c4616-174">Ukázka kódu k vytvoření oprávnění</span><span class="sxs-lookup"><span data-stu-id="c4616-174">Code sample to create permission</span></span>

<span data-ttu-id="c4616-175">Následující příklad kódu ukazuje, jak vytvořit prostředek oprávnění, token prostředku prostředku oprávnění číst a přidružte oprávnění s [uživatele](#users) vytvořili výše.</span><span class="sxs-lookup"><span data-stu-id="c4616-175">The following code sample shows how to create a permission resource, read the resource token of the permission resource, and associate the permissions with the [user](#users) created above.</span></span>

```csharp
// Create a permission.
Permission docPermission = new Permission
{
    PermissionMode = PermissionMode.Read,
    ResourceLink = documentCollection.SelfLink,
    Id = "readperm"
};
  
docPermission = await client.CreatePermissionAsync(UriFactory.CreateUserUri("db", "user"), docPermission);
Console.WriteLine(docPermission.Id + " has token of: " + docPermission.Token);
```

<span data-ttu-id="c4616-176">Pokud jste zadali klíč oddílu pro svoji kolekci, pak tato oprávnění pro kolekce, dokumentů a příloh prostředky musí obsahovat také ResourcePartitionKey kromě ResourceLink.</span><span class="sxs-lookup"><span data-stu-id="c4616-176">If you have specified a partition key for your collection, then the permission for collection, document, and attachment resources must also include the ResourcePartitionKey in addition to the ResourceLink.</span></span>

### <a name="code-sample-to-read-permissions-for-user"></a><span data-ttu-id="c4616-177">Ukázka kódu ke čtení oprávnění pro uživatele</span><span class="sxs-lookup"><span data-stu-id="c4616-177">Code sample to read permissions for user</span></span>

<span data-ttu-id="c4616-178">Snadno získat oprávnění všechny prostředky přidružené určitého uživatele, Cosmos DB zpřístupňují oprávnění kanálu pro každý objekt uživatele.</span><span class="sxs-lookup"><span data-stu-id="c4616-178">To easily obtain all permission resources associated with a particular user, Cosmos DB makes available a permission feed for each user object.</span></span>  <span data-ttu-id="c4616-179">Následující fragment kódu ukazuje, jak načíst oprávnění přidružené k uživateli vytvořili výše, vytvořit seznam oprávnění a vytvořit nové instance DocumentClient jménem uživatele.</span><span class="sxs-lookup"><span data-stu-id="c4616-179">The following code snippet shows how to retrieve the permission associated with the user created above, construct a permission list, and instantiate a new DocumentClient on behalf of the user.</span></span>

```csharp
//Read a permission feed.
FeedResponse<Permission> permFeed = await client.ReadPermissionFeedAsync(
  UriFactory.CreateUserUri("db", "myUser"));
 List<Permission> permList = new List<Permission>();

foreach (Permission perm in permFeed)
{
    permList.Add(perm);
}

DocumentClient userClient = new DocumentClient(new Uri(endpointUrl), permList);
```

## <a name="next-steps"></a><span data-ttu-id="c4616-180">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c4616-180">Next steps</span></span>
* <span data-ttu-id="c4616-181">Další informace o zabezpečení databáze Cosmos DB najdete v tématu [Cosmos databáze: databáze zabezpečení](database-security.md).</span><span class="sxs-lookup"><span data-stu-id="c4616-181">To learn more about Cosmos DB database security, see [Cosmos DB: Database security](database-security.md).</span></span>
* <span data-ttu-id="c4616-182">Další informace o správě klíče hlavní a jen pro čtení, najdete v části [Správa účtu Azure Cosmos DB](manage-account.md#keys).</span><span class="sxs-lookup"><span data-stu-id="c4616-182">To learn about managing master and read-only keys, see [How to manage an Azure Cosmos DB account](manage-account.md#keys).</span></span>
* <span data-ttu-id="c4616-183">Naučte se vytvořit databázi Cosmos Azure autorizace tokeny, najdete v tématu [řízení přístupu na prostředky Azure Cosmos DB](https://docs.microsoft.com/rest/api/documentdb/access-control-on-documentdb-resources).</span><span class="sxs-lookup"><span data-stu-id="c4616-183">To learn how to construct Azure Cosmos DB authorization tokens, see [Access Control on Azure Cosmos DB Resources](https://docs.microsoft.com/rest/api/documentdb/access-control-on-documentdb-resources).</span></span>
