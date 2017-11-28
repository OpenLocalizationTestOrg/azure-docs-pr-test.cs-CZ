---
title: "aaaLearn přístupu toodata v Azure Cosmos DB toosecure | Microsoft Docs"
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
ms.openlocfilehash: fef7f8e14b488f6ceab0f2aa279a1e99d4416f08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="securing-access-tooazure-cosmos-db-data"></a><span data-ttu-id="73b84-103">Zabezpečení přístupu tooAzure Cosmos DB dat</span><span class="sxs-lookup"><span data-stu-id="73b84-103">Securing access tooAzure Cosmos DB data</span></span>
<span data-ttu-id="73b84-104">Tento článek obsahuje přehled zabezpečení toodata přístupu uložené v [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="73b84-104">This article provides an overview of securing access toodata stored in [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).</span></span>

<span data-ttu-id="73b84-105">Azure Cosmos DB používá dva typy klíčů tooauthenticate uživatelů a poskytnout přístup k tooits datům a prostředkům.</span><span class="sxs-lookup"><span data-stu-id="73b84-105">Azure Cosmos DB uses two types of keys tooauthenticate users and provide access tooits data and resources.</span></span> 

|<span data-ttu-id="73b84-106">Typ klíče</span><span class="sxs-lookup"><span data-stu-id="73b84-106">Key type</span></span>|<span data-ttu-id="73b84-107">Zdroje</span><span class="sxs-lookup"><span data-stu-id="73b84-107">Resources</span></span>|
|---|---|
|[<span data-ttu-id="73b84-108">Hlavní klíče</span><span class="sxs-lookup"><span data-stu-id="73b84-108">Master keys</span></span>](#master-keys) |<span data-ttu-id="73b84-109">Použít pro správu prostředků: databáze účty, databází, uživatelů a oprávnění</span><span class="sxs-lookup"><span data-stu-id="73b84-109">Used for administrative resources: database accounts, databases, users, and permissions</span></span>|
|[<span data-ttu-id="73b84-110">Tokeny prostředků</span><span class="sxs-lookup"><span data-stu-id="73b84-110">Resource tokens</span></span>](#resource-tokens)|<span data-ttu-id="73b84-111">Použít pro prostředky aplikace: kolekcí, dokumentů, přílohy, uložené procedury, triggery a UDF</span><span class="sxs-lookup"><span data-stu-id="73b84-111">Used for application resources: collections, documents, attachments, stored procedures, triggers, and UDFs</span></span>|

<a id="master-keys"></a>

## <a name="master-keys"></a><span data-ttu-id="73b84-112">Hlavní klíče</span><span class="sxs-lookup"><span data-stu-id="73b84-112">Master keys</span></span> 

<span data-ttu-id="73b84-113">Hlavní klíče zadejte toohello přístup všechny prostředky pro správu hello hello databázového účtu.</span><span class="sxs-lookup"><span data-stu-id="73b84-113">Master keys provide access toohello all hello administrative resources for hello database account.</span></span> <span data-ttu-id="73b84-114">Hlavní klíče:</span><span class="sxs-lookup"><span data-stu-id="73b84-114">Master keys:</span></span>  
- <span data-ttu-id="73b84-115">Zadejte tooaccounts přístupu, databáze, uživatele a oprávnění.</span><span class="sxs-lookup"><span data-stu-id="73b84-115">Provide access tooaccounts, databases, users, and permissions.</span></span> 
- <span data-ttu-id="73b84-116">Nelze použít tooprovide granulární přístup toocollections a dokumenty.</span><span class="sxs-lookup"><span data-stu-id="73b84-116">Cannot be used tooprovide granular access toocollections and documents.</span></span>
- <span data-ttu-id="73b84-117">Při vytváření hello účet vytvořen.</span><span class="sxs-lookup"><span data-stu-id="73b84-117">Are created during hello creation of an account.</span></span>
- <span data-ttu-id="73b84-118">Může být kdykoli znovu vygenerovat.</span><span class="sxs-lookup"><span data-stu-id="73b84-118">Can be regenerated at any time.</span></span>

<span data-ttu-id="73b84-119">Každý účet se skládá ze dvou hlavních klíčů: primární klíč a sekundární klíč.</span><span class="sxs-lookup"><span data-stu-id="73b84-119">Each account consists of two Master keys: a primary key and secondary key.</span></span> <span data-ttu-id="73b84-120">účelem Hello dva klíče je, abyste mohli znovu vygenerovat nebo vrátit klíče, poskytující účet tooyour neustálý přístup a data.</span><span class="sxs-lookup"><span data-stu-id="73b84-120">hello purpose of dual keys is so that you can regenerate, or roll keys, providing continuous access tooyour account and data.</span></span> 

<span data-ttu-id="73b84-121">V přidání toohello dva hlavní klíče pro účet hello Cosmos DB existují dva klíče jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="73b84-121">In addition toohello two master keys for hello Cosmos DB account, there are two read-only keys.</span></span> <span data-ttu-id="73b84-122">Tyto klíče jen pro čtení Povolit jenom operací čtení na účet hello.</span><span class="sxs-lookup"><span data-stu-id="73b84-122">These read-only keys only allow read operations on hello account.</span></span> <span data-ttu-id="73b84-123">Jen pro čtení klíčů neposkytuje přístup tooread oprávnění prostředky.</span><span class="sxs-lookup"><span data-stu-id="73b84-123">Read-only keys do not provide access tooread permissions resources.</span></span>

<span data-ttu-id="73b84-124">Primární, sekundární, jen pro čtení, a pro čtení a zápis hlavního klíče se dá načíst a znovu vygenerovat pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="73b84-124">Primary, secondary, read only, and read-write master keys can be retrieved and regenerated using hello Azure portal.</span></span> <span data-ttu-id="73b84-125">Pokyny najdete v tématu [zobrazení, kopírování a opětovné vytváření přístupových klíčů](manage-account.md#keys).</span><span class="sxs-lookup"><span data-stu-id="73b84-125">For instructions, see [View, copy, and regenerate access keys](manage-account.md#keys).</span></span>

![Řízení přístupu (IAM) v hello portál Azure – ukázka zabezpečení databáze NoSQL](./media/secure-access-to-data/nosql-database-security-master-key-portal.png)

<span data-ttu-id="73b84-127">proces Hello otáčení hlavní klíč je jednoduché.</span><span class="sxs-lookup"><span data-stu-id="73b84-127">hello process of rotating your master key is simple.</span></span> <span data-ttu-id="73b84-128">Přejděte toohello Azure portálu tooretrieve sekundární klíč, pak primární klíč nahradit sekundární klíč v aplikaci a potom otočit hello primární klíč v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="73b84-128">Navigate toohello Azure portal tooretrieve your secondary key, then replace your primary key with your secondary key in your application, then rotate hello primary key in hello Azure portal.</span></span>

![Hlavní klíč otočení v hello portál Azure – ukázka zabezpečení databáze NoSQL](./media/secure-access-to-data/nosql-database-security-master-key-rotate-workflow.png)

### <a name="code-sample-toouse-a-master-key"></a><span data-ttu-id="73b84-130">Ukázka toouse hlavní klíč kódu</span><span class="sxs-lookup"><span data-stu-id="73b84-130">Code sample toouse a master key</span></span>

<span data-ttu-id="73b84-131">Hello následující ukázka kódu ukazuje, jak toouse Cosmos DB účet koncový bod a hlavní klíč tooinstantiate DocumentClient a vytvořit databázi.</span><span class="sxs-lookup"><span data-stu-id="73b84-131">hello following code sample illustrates how toouse a Cosmos DB account endpoint and master key tooinstantiate a DocumentClient and create a database.</span></span> 

```csharp
//Read hello Azure Cosmos DB endpointUrl and authorization keys from config.
//These values are available from hello Azure portal on hello Azure Cosmos DB account blade under "Keys".
//NB > Keep these values in a safe and secure location. Together they provide Administrative access tooyour DocDB account.

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

## <a name="resource-tokens"></a><span data-ttu-id="73b84-132">Tokeny prostředků</span><span class="sxs-lookup"><span data-stu-id="73b84-132">Resource tokens</span></span>

<span data-ttu-id="73b84-133">Tokeny prostředků poskytovat přístup k toohello prostředků aplikace v databázi.</span><span class="sxs-lookup"><span data-stu-id="73b84-133">Resource tokens provide access toohello application resources within a database.</span></span> <span data-ttu-id="73b84-134">Tokeny prostředků:</span><span class="sxs-lookup"><span data-stu-id="73b84-134">Resource tokens:</span></span>
- <span data-ttu-id="73b84-135">Zadejte přístup toospecific kolekce, klíče oddílů, dokumenty, přílohy, uložené procedury, triggery a UDF.</span><span class="sxs-lookup"><span data-stu-id="73b84-135">Provide access toospecific collections, partition keys, documents, attachments, stored procedures, triggers, and UDFs.</span></span>
- <span data-ttu-id="73b84-136">Jsou vytvářeny, pokud [uživatele](#users) jsou udělena [oprávnění](#permissions) tooa konkrétní prostředek.</span><span class="sxs-lookup"><span data-stu-id="73b84-136">Are created when a [user](#users) is granted [permissions](#permissions) tooa specific resource.</span></span>
- <span data-ttu-id="73b84-137">Jsou vytvořeny při prostředek oprávnění je reagovali na ni na při volání metody POST, GET a PUT.</span><span class="sxs-lookup"><span data-stu-id="73b84-137">Are recreated when a permission resource is acted upon on by POST, GET, or PUT call.</span></span>
- <span data-ttu-id="73b84-138">Použijte token prostředků hash vytvořená speciálně pro hello uživatelů a prostředků a oprávnění.</span><span class="sxs-lookup"><span data-stu-id="73b84-138">Use a hash resource token specifically constructed for hello user, resource, and permission.</span></span>
- <span data-ttu-id="73b84-139">Když vázán s dobou platnosti přizpůsobitelné.</span><span class="sxs-lookup"><span data-stu-id="73b84-139">Are time bound with a customizable validity period.</span></span> <span data-ttu-id="73b84-140">Neplatný časový interval výchozí Hello je jedna hodina.</span><span class="sxs-lookup"><span data-stu-id="73b84-140">hello default valid timespan is one hour.</span></span> <span data-ttu-id="73b84-141">Životnost tokenu, ale je možné explicitně zadat, až tooa maximálně pět hodin.</span><span class="sxs-lookup"><span data-stu-id="73b84-141">Token lifetime, however, may be explicitly specified, up tooa maximum of five hours.</span></span>
- <span data-ttu-id="73b84-142">Zadejte bezpečné alternativní toogiving out hello hlavní klíč.</span><span class="sxs-lookup"><span data-stu-id="73b84-142">Provide a safe alternative toogiving out hello master key.</span></span> 
- <span data-ttu-id="73b84-143">Povolte klientům tooread, zápisu a odstranění prostředky v účtu Cosmos DB hello podle toohello oprávnění, která jste byla udělena.</span><span class="sxs-lookup"><span data-stu-id="73b84-143">Enable clients tooread, write, and delete resources in hello Cosmos DB account according toohello permissions they've been granted.</span></span>

<span data-ttu-id="73b84-144">Token prostředku můžete použít (vytvořením Cosmos DB uživatele a oprávnění) Pokud chcete tooprovide tooresources přístup do vaší databáze Cosmos účtem tooa klienta, který nemůže být považován za důvěryhodný hlavní klíč hello.</span><span class="sxs-lookup"><span data-stu-id="73b84-144">You can use a resource token (by creating Cosmos DB users and permissions) when you want tooprovide access tooresources in your Cosmos DB account tooa client that cannot be trusted with hello master key.</span></span>  

<span data-ttu-id="73b84-145">Tokeny prostředků cosmos DB zadejte bezpečné alternativu, která umožňuje klientům tooread, zápisu a odstranění prostředků ve vašem účtu Cosmos DB podle toohello oprávnění, která jste udělena a bez nutnosti buď hlavní nebo čtení pouze klíče.</span><span class="sxs-lookup"><span data-stu-id="73b84-145">Cosmos DB resource tokens provide a safe alternative that enables clients tooread, write, and delete resources in your Cosmos DB account according toohello permissions you've granted, and without need for either a master or read only key.</span></span>

<span data-ttu-id="73b84-146">Zde je typické návrhový vzor, kterým může požadovaný prostředek tokeny a způsob jejich generované a doručit tooclients:</span><span class="sxs-lookup"><span data-stu-id="73b84-146">Here is a typical design pattern whereby resource tokens may be requested, generated, and delivered tooclients:</span></span>

1. <span data-ttu-id="73b84-147">Střední vrstvě služby nastavená tooserve fotografie uživatele tooshare mobilních aplikací.</span><span class="sxs-lookup"><span data-stu-id="73b84-147">A mid-tier service is set up tooserve a mobile application tooshare user photos.</span></span> 
2. <span data-ttu-id="73b84-148">Hello střední vrstvě služby má hello hlavní klíč hello účet Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="73b84-148">hello mid-tier service possesses hello master key of hello Cosmos DB account.</span></span>
3. <span data-ttu-id="73b84-149">Hello fotografií aplikace je nainstalovaná na mobilní zařízení koncového uživatele.</span><span class="sxs-lookup"><span data-stu-id="73b84-149">hello photo app is installed on end-user mobile devices.</span></span> 
4. <span data-ttu-id="73b84-150">Přihlašování vytvoří hello fotografií aplikace hello identitu uživatele hello službou hello střední vrstvě.</span><span class="sxs-lookup"><span data-stu-id="73b84-150">On login, hello photo app establishes hello identity of hello user with hello mid-tier service.</span></span> <span data-ttu-id="73b84-151">Tento mechanismus identity zařízení je čistě až toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="73b84-151">This mechanism of identity establishment is purely up toohello application.</span></span>
5. <span data-ttu-id="73b84-152">Po vytvoření hello identity žádosti o službu střední vrstvě hello oprávnění na základě hello identity.</span><span class="sxs-lookup"><span data-stu-id="73b84-152">Once hello identity is established, hello mid-tier service requests permissions based on hello identity.</span></span>
6. <span data-ttu-id="73b84-153">Hello střední vrstvě služby odešle prostředků tokenu back toohello telefonní aplikace.</span><span class="sxs-lookup"><span data-stu-id="73b84-153">hello mid-tier service sends a resource token back toohello phone app.</span></span>
7. <span data-ttu-id="73b84-154">Hello telefonní aplikace můžete pokračovat toouse hello prostředků tokenu toodirectly přístup k Cosmos DB prostředkům definována token prostředku hello a pro interval hello povolenou token prostředku hello hello oprávnění.</span><span class="sxs-lookup"><span data-stu-id="73b84-154">hello phone app can continue toouse hello resource token toodirectly access Cosmos DB resources with hello permissions defined by hello resource token and for hello interval allowed by hello resource token.</span></span> 
8. <span data-ttu-id="73b84-155">Když vyprší platnost tokenu hello prostředků, následných žádostí přijímat 401 neoprávněný výjimka.</span><span class="sxs-lookup"><span data-stu-id="73b84-155">When hello resource token expires, subsequent requests receive a 401 unauthorized exception.</span></span>  <span data-ttu-id="73b84-156">V tomto okamžiku hello telefonní aplikaci znovu naváže hello identity a vyžádá nový token prostředku.</span><span class="sxs-lookup"><span data-stu-id="73b84-156">At this point, hello phone app re-establishes hello identity and requests a new resource token.</span></span>

    ![Prostředek Azure Cosmos DB tokeny pracovního postupu](./media/secure-access-to-data/resourcekeyworkflow.png)

<span data-ttu-id="73b84-158">Generování tokenů prostředků a správy se zpracovává souborem hello nativní knihovny klienta Cosmos DB; ale pokud používáte REST je nutné vytvořit hlavičky požadavku nebo ověřování hello.</span><span class="sxs-lookup"><span data-stu-id="73b84-158">Resource token generation and management is handled by hello native Cosmos DB client libraries; however, if you use REST you must construct hello request/authentication headers.</span></span> <span data-ttu-id="73b84-159">Další informace o vytváření ověřovacím hlavičkám pro REST naleznete v tématu [řízení přístupu u prostředků DB Cosmos](https://docs.microsoft.com/rest/api/documentdb/access-control-on-documentdb-resources) nebo hello [zdrojový kód pro naše sady SDK](https://github.com/Azure/azure-documentdb-node/blob/master/source/lib/auth.js).</span><span class="sxs-lookup"><span data-stu-id="73b84-159">For more information on creating authentication headers for REST, see [Access Control on Cosmos DB Resources](https://docs.microsoft.com/rest/api/documentdb/access-control-on-documentdb-resources) or hello [source code for our SDKs](https://github.com/Azure/azure-documentdb-node/blob/master/source/lib/auth.js).</span></span>

<span data-ttu-id="73b84-160">Příklad služby střední vrstvy používá tokeny toogenerate nebo zprostředkovatele prostředků, najdete v části hello [ResourceTokenBroker aplikace](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin/UserItems/ResourceTokenBroker/ResourceTokenBroker/Controllers).</span><span class="sxs-lookup"><span data-stu-id="73b84-160">For an example of a middle tier service used toogenerate or broker resource tokens, see hello [ResourceTokenBroker app](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin/UserItems/ResourceTokenBroker/ResourceTokenBroker/Controllers).</span></span>

<a id="users"></a>

## <a name="users"></a><span data-ttu-id="73b84-161">Uživatelé</span><span class="sxs-lookup"><span data-stu-id="73b84-161">Users</span></span>
<span data-ttu-id="73b84-162">Uživatelé DB cosmos jsou přidruženy k databázi Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="73b84-162">Cosmos DB users are associated with a Cosmos DB database.</span></span>  <span data-ttu-id="73b84-163">Každá databáze může obsahovat nula nebo více uživatelů Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="73b84-163">Each database can contain zero or more Cosmos DB users.</span></span>  <span data-ttu-id="73b84-164">Dobrý den, jak následující kód ukazuje ukázka toocreate prostředek uživatele Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="73b84-164">hello following code sample shows how toocreate a Cosmos DB user resource.</span></span>

```csharp
//Create a user.
User docUser = new User
{
    Id = "mobileuser"
};

docUser = await client.CreateUserAsync(UriFactory.CreateDatabaseUri("db"), docUser);
```

> [!NOTE]
> <span data-ttu-id="73b84-165">Každý uživatel Cosmos DB má PermissionsLink vlastnost, která lze použít tooretrieve hello seznamu [oprávnění](#permissions) přidružené hello uživateli.</span><span class="sxs-lookup"><span data-stu-id="73b84-165">Each Cosmos DB user has a PermissionsLink property that can be used tooretrieve hello list of [permissions](#permissions) associated with hello user.</span></span>
> 
> 

<a id="permissions"></a>

## <a name="permissions"></a><span data-ttu-id="73b84-166">Oprávnění</span><span class="sxs-lookup"><span data-stu-id="73b84-166">Permissions</span></span>
<span data-ttu-id="73b84-167">Prostředek Cosmos DB oprávnění je přidružen uživatele Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="73b84-167">A Cosmos DB permission resource is associated with a Cosmos DB user.</span></span>  <span data-ttu-id="73b84-168">Každý uživatel může obsahovat nula nebo více oprávnění Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="73b84-168">Each user may contain zero or more Cosmos DB permissions.</span></span>  <span data-ttu-id="73b84-169">Prostředek oprávnění poskytuje přístup tooa token zabezpečení hello musí uživatel při pokusu o tooaccess prostředek konkrétní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="73b84-169">A permission resource provides access tooa security token that hello user needs when trying tooaccess a specific application resource.</span></span>
<span data-ttu-id="73b84-170">Existují dvě úrovně přístupu k dispozici, které může být k dispozici prostředek oprávnění:</span><span class="sxs-lookup"><span data-stu-id="73b84-170">There are two available access levels that may be provided by a permission resource:</span></span>

* <span data-ttu-id="73b84-171">All: hello uživatel má úplná oprávnění na hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="73b84-171">All: hello user has full permission on hello resource.</span></span>
* <span data-ttu-id="73b84-172">Přečtěte si: hello uživatele může číst pouze obsah hello hello prostředku, ale nemůže provádět zápis, operace aktualizace nebo odstranění na hello prostředku.</span><span class="sxs-lookup"><span data-stu-id="73b84-172">Read: hello user can only read hello contents of hello resource but cannot perform write, update, or delete operations on hello resource.</span></span>

> [!NOTE]
> <span data-ttu-id="73b84-173">V pořadí toorun Cosmos DB uložené procedury hello uživatel musí mít hello všechna oprávnění na hello kolekci, ve které hello se spustí uloženou proceduru.</span><span class="sxs-lookup"><span data-stu-id="73b84-173">In order toorun Cosmos DB stored procedures hello user must have hello All permission on hello collection in which hello stored procedure will be run.</span></span>
> 
> 

### <a name="code-sample-toocreate-permission"></a><span data-ttu-id="73b84-174">Oprávnění toocreate ukázka kódu</span><span class="sxs-lookup"><span data-stu-id="73b84-174">Code sample toocreate permission</span></span>

<span data-ttu-id="73b84-175">Hello následující ukázka kódu ukazuje, jak toocreate prostředek oprávnění číst hello token prostředku hello oprávnění prostředku a oprávnění hello přidružit hello [uživatele](#users) vytvořili výše.</span><span class="sxs-lookup"><span data-stu-id="73b84-175">hello following code sample shows how toocreate a permission resource, read hello resource token of hello permission resource, and associate hello permissions with hello [user](#users) created above.</span></span>

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

<span data-ttu-id="73b84-176">Pokud jste zadali, že klíč oddílu pro svoji kolekci, pak hello oprávnění pro kolekci, dokumentů a příloh prostředky musí také obsahovat hello ResourcePartitionKey v přidání toohello ResourceLink.</span><span class="sxs-lookup"><span data-stu-id="73b84-176">If you have specified a partition key for your collection, then hello permission for collection, document, and attachment resources must also include hello ResourcePartitionKey in addition toohello ResourceLink.</span></span>

### <a name="code-sample-tooread-permissions-for-user"></a><span data-ttu-id="73b84-177">Oprávnění tooread ukázkový kód pro uživatele</span><span class="sxs-lookup"><span data-stu-id="73b84-177">Code sample tooread permissions for user</span></span>

<span data-ttu-id="73b84-178">tooeasily získat všechny prostředky oprávnění, které jsou spojené s konkrétní uživatele, Cosmos DB zpřístupní oprávnění kanálu pro každý objekt uživatele.</span><span class="sxs-lookup"><span data-stu-id="73b84-178">tooeasily obtain all permission resources associated with a particular user, Cosmos DB makes available a permission feed for each user object.</span></span>  <span data-ttu-id="73b84-179">Hello následující fragment kódu ukazuje, jak oprávnění hello tooretrieve přidružené uživateli hello vytvořili výše, vytvořit seznam oprávnění a vytvořit nové instance DocumentClient jménem uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="73b84-179">hello following code snippet shows how tooretrieve hello permission associated with hello user created above, construct a permission list, and instantiate a new DocumentClient on behalf of hello user.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="73b84-180">Další kroky</span><span class="sxs-lookup"><span data-stu-id="73b84-180">Next steps</span></span>
* <span data-ttu-id="73b84-181">toolearn Další informace o zabezpečení databáze Cosmos DB, najdete v části [Cosmos databáze: databáze zabezpečení](database-security.md).</span><span class="sxs-lookup"><span data-stu-id="73b84-181">toolearn more about Cosmos DB database security, see [Cosmos DB: Database security](database-security.md).</span></span>
* <span data-ttu-id="73b84-182">toolearn o správě klíče hlavní a jen pro čtení, najdete v části [jak toomanage účet Azure Cosmos DB](manage-account.md#keys).</span><span class="sxs-lookup"><span data-stu-id="73b84-182">toolearn about managing master and read-only keys, see [How toomanage an Azure Cosmos DB account](manage-account.md#keys).</span></span>
* <span data-ttu-id="73b84-183">jak zjistit, tokeny autorizace v Azure Cosmos DB tooconstruct toolearn [řízení přístupu na prostředky Azure Cosmos DB](https://docs.microsoft.com/rest/api/documentdb/access-control-on-documentdb-resources).</span><span class="sxs-lookup"><span data-stu-id="73b84-183">toolearn how tooconstruct Azure Cosmos DB authorization tokens, see [Access Control on Azure Cosmos DB Resources](https://docs.microsoft.com/rest/api/documentdb/access-control-on-documentdb-resources).</span></span>
