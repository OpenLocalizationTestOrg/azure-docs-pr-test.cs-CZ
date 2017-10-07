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
# <a name="securing-access-tooazure-cosmos-db-data"></a>Zabezpečení přístupu tooAzure Cosmos DB dat
Tento článek obsahuje přehled zabezpečení toodata přístupu uložené v [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).

Azure Cosmos DB používá dva typy klíčů tooauthenticate uživatelů a poskytnout přístup k tooits datům a prostředkům. 

|Typ klíče|Zdroje|
|---|---|
|[Hlavní klíče](#master-keys) |Použít pro správu prostředků: databáze účty, databází, uživatelů a oprávnění|
|[Tokeny prostředků](#resource-tokens)|Použít pro prostředky aplikace: kolekcí, dokumentů, přílohy, uložené procedury, triggery a UDF|

<a id="master-keys"></a>

## <a name="master-keys"></a>Hlavní klíče 

Hlavní klíče zadejte toohello přístup všechny prostředky pro správu hello hello databázového účtu. Hlavní klíče:  
- Zadejte tooaccounts přístupu, databáze, uživatele a oprávnění. 
- Nelze použít tooprovide granulární přístup toocollections a dokumenty.
- Při vytváření hello účet vytvořen.
- Může být kdykoli znovu vygenerovat.

Každý účet se skládá ze dvou hlavních klíčů: primární klíč a sekundární klíč. účelem Hello dva klíče je, abyste mohli znovu vygenerovat nebo vrátit klíče, poskytující účet tooyour neustálý přístup a data. 

V přidání toohello dva hlavní klíče pro účet hello Cosmos DB existují dva klíče jen pro čtení. Tyto klíče jen pro čtení Povolit jenom operací čtení na účet hello. Jen pro čtení klíčů neposkytuje přístup tooread oprávnění prostředky.

Primární, sekundární, jen pro čtení, a pro čtení a zápis hlavního klíče se dá načíst a znovu vygenerovat pomocí hello portálu Azure. Pokyny najdete v tématu [zobrazení, kopírování a opětovné vytváření přístupových klíčů](manage-account.md#keys).

![Řízení přístupu (IAM) v hello portál Azure – ukázka zabezpečení databáze NoSQL](./media/secure-access-to-data/nosql-database-security-master-key-portal.png)

proces Hello otáčení hlavní klíč je jednoduché. Přejděte toohello Azure portálu tooretrieve sekundární klíč, pak primární klíč nahradit sekundární klíč v aplikaci a potom otočit hello primární klíč v hello portálu Azure.

![Hlavní klíč otočení v hello portál Azure – ukázka zabezpečení databáze NoSQL](./media/secure-access-to-data/nosql-database-security-master-key-rotate-workflow.png)

### <a name="code-sample-toouse-a-master-key"></a>Ukázka toouse hlavní klíč kódu

Hello následující ukázka kódu ukazuje, jak toouse Cosmos DB účet koncový bod a hlavní klíč tooinstantiate DocumentClient a vytvořit databázi. 

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

## <a name="resource-tokens"></a>Tokeny prostředků

Tokeny prostředků poskytovat přístup k toohello prostředků aplikace v databázi. Tokeny prostředků:
- Zadejte přístup toospecific kolekce, klíče oddílů, dokumenty, přílohy, uložené procedury, triggery a UDF.
- Jsou vytvářeny, pokud [uživatele](#users) jsou udělena [oprávnění](#permissions) tooa konkrétní prostředek.
- Jsou vytvořeny při prostředek oprávnění je reagovali na ni na při volání metody POST, GET a PUT.
- Použijte token prostředků hash vytvořená speciálně pro hello uživatelů a prostředků a oprávnění.
- Když vázán s dobou platnosti přizpůsobitelné. Neplatný časový interval výchozí Hello je jedna hodina. Životnost tokenu, ale je možné explicitně zadat, až tooa maximálně pět hodin.
- Zadejte bezpečné alternativní toogiving out hello hlavní klíč. 
- Povolte klientům tooread, zápisu a odstranění prostředky v účtu Cosmos DB hello podle toohello oprávnění, která jste byla udělena.

Token prostředku můžete použít (vytvořením Cosmos DB uživatele a oprávnění) Pokud chcete tooprovide tooresources přístup do vaší databáze Cosmos účtem tooa klienta, který nemůže být považován za důvěryhodný hlavní klíč hello.  

Tokeny prostředků cosmos DB zadejte bezpečné alternativu, která umožňuje klientům tooread, zápisu a odstranění prostředků ve vašem účtu Cosmos DB podle toohello oprávnění, která jste udělena a bez nutnosti buď hlavní nebo čtení pouze klíče.

Zde je typické návrhový vzor, kterým může požadovaný prostředek tokeny a způsob jejich generované a doručit tooclients:

1. Střední vrstvě služby nastavená tooserve fotografie uživatele tooshare mobilních aplikací. 
2. Hello střední vrstvě služby má hello hlavní klíč hello účet Cosmos DB.
3. Hello fotografií aplikace je nainstalovaná na mobilní zařízení koncového uživatele. 
4. Přihlašování vytvoří hello fotografií aplikace hello identitu uživatele hello službou hello střední vrstvě. Tento mechanismus identity zařízení je čistě až toohello aplikace.
5. Po vytvoření hello identity žádosti o službu střední vrstvě hello oprávnění na základě hello identity.
6. Hello střední vrstvě služby odešle prostředků tokenu back toohello telefonní aplikace.
7. Hello telefonní aplikace můžete pokračovat toouse hello prostředků tokenu toodirectly přístup k Cosmos DB prostředkům definována token prostředku hello a pro interval hello povolenou token prostředku hello hello oprávnění. 
8. Když vyprší platnost tokenu hello prostředků, následných žádostí přijímat 401 neoprávněný výjimka.  V tomto okamžiku hello telefonní aplikaci znovu naváže hello identity a vyžádá nový token prostředku.

    ![Prostředek Azure Cosmos DB tokeny pracovního postupu](./media/secure-access-to-data/resourcekeyworkflow.png)

Generování tokenů prostředků a správy se zpracovává souborem hello nativní knihovny klienta Cosmos DB; ale pokud používáte REST je nutné vytvořit hlavičky požadavku nebo ověřování hello. Další informace o vytváření ověřovacím hlavičkám pro REST naleznete v tématu [řízení přístupu u prostředků DB Cosmos](https://docs.microsoft.com/rest/api/documentdb/access-control-on-documentdb-resources) nebo hello [zdrojový kód pro naše sady SDK](https://github.com/Azure/azure-documentdb-node/blob/master/source/lib/auth.js).

Příklad služby střední vrstvy používá tokeny toogenerate nebo zprostředkovatele prostředků, najdete v části hello [ResourceTokenBroker aplikace](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin/UserItems/ResourceTokenBroker/ResourceTokenBroker/Controllers).

<a id="users"></a>

## <a name="users"></a>Uživatelé
Uživatelé DB cosmos jsou přidruženy k databázi Cosmos DB.  Každá databáze může obsahovat nula nebo více uživatelů Cosmos DB.  Dobrý den, jak následující kód ukazuje ukázka toocreate prostředek uživatele Cosmos DB.

```csharp
//Create a user.
User docUser = new User
{
    Id = "mobileuser"
};

docUser = await client.CreateUserAsync(UriFactory.CreateDatabaseUri("db"), docUser);
```

> [!NOTE]
> Každý uživatel Cosmos DB má PermissionsLink vlastnost, která lze použít tooretrieve hello seznamu [oprávnění](#permissions) přidružené hello uživateli.
> 
> 

<a id="permissions"></a>

## <a name="permissions"></a>Oprávnění
Prostředek Cosmos DB oprávnění je přidružen uživatele Cosmos DB.  Každý uživatel může obsahovat nula nebo více oprávnění Cosmos DB.  Prostředek oprávnění poskytuje přístup tooa token zabezpečení hello musí uživatel při pokusu o tooaccess prostředek konkrétní aplikaci.
Existují dvě úrovně přístupu k dispozici, které může být k dispozici prostředek oprávnění:

* All: hello uživatel má úplná oprávnění na hello prostředků.
* Přečtěte si: hello uživatele může číst pouze obsah hello hello prostředku, ale nemůže provádět zápis, operace aktualizace nebo odstranění na hello prostředku.

> [!NOTE]
> V pořadí toorun Cosmos DB uložené procedury hello uživatel musí mít hello všechna oprávnění na hello kolekci, ve které hello se spustí uloženou proceduru.
> 
> 

### <a name="code-sample-toocreate-permission"></a>Oprávnění toocreate ukázka kódu

Hello následující ukázka kódu ukazuje, jak toocreate prostředek oprávnění číst hello token prostředku hello oprávnění prostředku a oprávnění hello přidružit hello [uživatele](#users) vytvořili výše.

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

Pokud jste zadali, že klíč oddílu pro svoji kolekci, pak hello oprávnění pro kolekci, dokumentů a příloh prostředky musí také obsahovat hello ResourcePartitionKey v přidání toohello ResourceLink.

### <a name="code-sample-tooread-permissions-for-user"></a>Oprávnění tooread ukázkový kód pro uživatele

tooeasily získat všechny prostředky oprávnění, které jsou spojené s konkrétní uživatele, Cosmos DB zpřístupní oprávnění kanálu pro každý objekt uživatele.  Hello následující fragment kódu ukazuje, jak oprávnění hello tooretrieve přidružené uživateli hello vytvořili výše, vytvořit seznam oprávnění a vytvořit nové instance DocumentClient jménem uživatele hello.

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

## <a name="next-steps"></a>Další kroky
* toolearn Další informace o zabezpečení databáze Cosmos DB, najdete v části [Cosmos databáze: databáze zabezpečení](database-security.md).
* toolearn o správě klíče hlavní a jen pro čtení, najdete v části [jak toomanage účet Azure Cosmos DB](manage-account.md#keys).
* jak zjistit, tokeny autorizace v Azure Cosmos DB tooconstruct toolearn [řízení přístupu na prostředky Azure Cosmos DB](https://docs.microsoft.com/rest/api/documentdb/access-control-on-documentdb-resources).
