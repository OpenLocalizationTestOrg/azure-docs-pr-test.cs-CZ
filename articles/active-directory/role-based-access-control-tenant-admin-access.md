---
title: "Správce aaaTenant zvýšení přístupu – Azure AD | Microsoft Docs"
description: "Toto téma popisuje hello součástí role pro řízení přístupu na základě role (RBAC)."
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
editor: rqureshi
ms.assetid: b547c5a5-2da2-4372-9938-481cb962d2d6
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andredm
ms.openlocfilehash: 7996f2af3277dc40e2a1766cc4a7862a2399cdef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="elevate-access-as-a-tenant-admin-with-role-based-access-control"></a><span data-ttu-id="5019b-103">Zvýšení přístupu jako správce klienta s řízením přístupu na základě rolí</span><span class="sxs-lookup"><span data-stu-id="5019b-103">Elevate access as a tenant admin with Role-Based Access Control</span></span>

<span data-ttu-id="5019b-104">Řízení přístupu na základě rolí pomáhá správci klientů získat dočasný – zvýšení úrovní oprávnění přístupu tak, aby se můžete udělit oprávnění vyšší než normální.</span><span class="sxs-lookup"><span data-stu-id="5019b-104">Role-based Access Control helps tenant administrators get temporary elevations in access so that they can grant higher permissions than normal.</span></span> <span data-ttu-id="5019b-105">Správce klienta může zvýšit nároky samu sebe toohello role správce přístupu uživatelů v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="5019b-105">A tenant admin can elevate herself toohello User Access Administrator role when needed.</span></span> <span data-ttu-id="5019b-106">Tato role poskytuje hello klienta toogrant oprávnění správce samu sebe nebo ostatní role v hello "/" oboru.</span><span class="sxs-lookup"><span data-stu-id="5019b-106">That role gives hello tenant admin permissions toogrant herself or others roles at hello "/" scope.</span></span>

<span data-ttu-id="5019b-107">Tato funkce je důležitá, protože umožňuje hello klienta správce toosee všechny hello odběry, které existují v organizaci.</span><span class="sxs-lookup"><span data-stu-id="5019b-107">This feature is important because it allows hello tenant admin toosee all hello subscriptions that exist in an organization.</span></span> <span data-ttu-id="5019b-108">Také umožňuje automatizace aplikace (jako je fakturace a auditování) tooaccess všechny odběry hello a poskytují přesné informace o stavu hello hello organizace pro správu fakturace nebo asset.</span><span class="sxs-lookup"><span data-stu-id="5019b-108">It also allows for automation apps (like invoicing and auditing) tooaccess all hello subscriptions and provide an accurate view of hello state of hello organization for billing or asset management.</span></span>  

## <a name="how-toouse-elevateaccess-toogive-tenant-access"></a><span data-ttu-id="5019b-109">Jak toouse elevateAccess toogive klienta přístup</span><span class="sxs-lookup"><span data-stu-id="5019b-109">How toouse elevateAccess toogive tenant access</span></span>

<span data-ttu-id="5019b-110">Základní proces Hello funguje s hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5019b-110">hello basic process works with hello following steps:</span></span>

1. <span data-ttu-id="5019b-111">Pomocí REST, volání *elevateAccess*, která uděluje můžete hello role správce přístupu uživatelů v "/" oboru.</span><span class="sxs-lookup"><span data-stu-id="5019b-111">Using REST, call *elevateAccess*, which grants you hello User Access Administrator role at "/" scope.</span></span>

    ```
    POST https://management.azure.com/providers/Microsoft.Authorization/elevateAccess?api-version=2016-07-01
    ```

2. <span data-ttu-id="5019b-112">Vytvoření [přiřazení role](/rest/api/authorization/roleassignments) tooassign všechny role v jakémkoli oboru.</span><span class="sxs-lookup"><span data-stu-id="5019b-112">Create a [role assignment](/rest/api/authorization/roleassignments) tooassign any role at any scope.</span></span> <span data-ttu-id="5019b-113">Hello následující příklad ukazuje hello vlastnosti pro přiřazení hello role čtenáře v "/" oboru:</span><span class="sxs-lookup"><span data-stu-id="5019b-113">hello following example shows hello properties for assigning hello Reader role at "/" scope:</span></span>

    ```
    { "properties":{
    "roleDefinitionId": "providers/Microsoft.Authorization/roleDefinitions/acdd72a7338548efbd42f606fba81ae7",
    "principalId": "cbc5e050-d7cd-4310-813b-4870be8ef5bb",
    "scope": "/"
    },
    "id": "providers/Microsoft.Authorization/roleAssignments/64736CA0-56D7-4A94-A551-973C2FE7888B",
    "type": "Microsoft.Authorization/roleAssignments",
    "name": "64736CA0-56D7-4A94-A551-973C2FE7888B"
    }
    ```

3. <span data-ttu-id="5019b-114">Při přístupu správce uživatele můžete můžete také odstranit přiřazení role v "/" oboru.</span><span class="sxs-lookup"><span data-stu-id="5019b-114">While a User Access Admin, you can also delete role assignments at "/" scope.</span></span>

4. <span data-ttu-id="5019b-115">Vaše oprávnění správce přístupu uživatelů odvolat, dokud se znovu požadován.</span><span class="sxs-lookup"><span data-stu-id="5019b-115">Revoke your User Access Admin privileges until they're needed again.</span></span>


## <a name="how-tooundo-hello-elevateaccess-action"></a><span data-ttu-id="5019b-116">Jak tooundo hello elevateAccess akce</span><span class="sxs-lookup"><span data-stu-id="5019b-116">How tooundo hello elevateAccess action</span></span>

<span data-ttu-id="5019b-117">Při volání *elevateAccess* vytvořit přiřazení role pro sebe, tak toorevoke, ty vám oprávnění potřebovat toodelete hello přiřazení.</span><span class="sxs-lookup"><span data-stu-id="5019b-117">When you call *elevateAccess* you create a role assignment for yourself, so toorevoke those privileges you need toodelete hello assignment.</span></span>

1.  <span data-ttu-id="5019b-118">Volání [GET roleDefinitions](/rest/api/authorization/roledefinitions#RoleDefinitions_Get) kde roleName = název hello správce přístupu uživatelů toodetermine GUID role správce přístupu uživatelů hello.</span><span class="sxs-lookup"><span data-stu-id="5019b-118">Call [GET roleDefinitions](/rest/api/authorization/roledefinitions#RoleDefinitions_Get) where roleName = User Access Administrator toodetermine hello name GUID of hello User Access Administrator role.</span></span> <span data-ttu-id="5019b-119">Hello odpověď by měla vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="5019b-119">hello response should look like this:</span></span>

    ```
    {"value":[{"properties":{
    "roleName":"User Access Administrator",
    "type":"BuiltInRole",
    "description":"Lets you manage user access tooAzure resources.",
    "assignableScopes":["/"],
    "permissions":[{"actions":["*/read","Microsoft.Authorization/*","Microsoft.Support/*"],"notActions":[]}],
    "createdOn":"0001-01-01T08:00:00.0000000Z",
    "updatedOn":"2016-05-31T23:14:04.6964687Z",
    "createdBy":null,
    "updatedBy":null},
    "id":"/providers/Microsoft.Authorization/roleDefinitions/18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
    "type":"Microsoft.Authorization/roleDefinitions",
    "name":"18d7d88d-d35e-4fb5-a5c3-7773c20a72d9"}],
    "nextLink":null}
    ```

    <span data-ttu-id="5019b-120">Uložit hello GUID z hello *název* parametr v tomto případě **18d7d88d-d35e-4fb5-a5c3-7773c20a72d9**.</span><span class="sxs-lookup"><span data-stu-id="5019b-120">Save hello GUID from hello *name* parameter, in this case **18d7d88d-d35e-4fb5-a5c3-7773c20a72d9**.</span></span>

2. <span data-ttu-id="5019b-121">Volání [GET roleAssignments](/rest/api/authorization/roleassignments#RoleAssignments_Get) kde principalId = vlastní ObjectId.</span><span class="sxs-lookup"><span data-stu-id="5019b-121">Call [GET roleAssignments](/rest/api/authorization/roleassignments#RoleAssignments_Get) where principalId = your own ObjectId.</span></span> <span data-ttu-id="5019b-122">Rutina Vypíše seznam všech přiřazení v klientovi hello.</span><span class="sxs-lookup"><span data-stu-id="5019b-122">This lists all your assignments in hello tenant.</span></span> <span data-ttu-id="5019b-123">Vyhledejte hello jeden, kde je hello oboru "/" a končí hodnoty vlastnosti RoleDefinitionId hello s rolí hello název GUID, které jste získali v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="5019b-123">Look for hello one where hello scope is "/" and hello RoleDefinitionId ends with hello role name GUID you found in step 1.</span></span> <span data-ttu-id="5019b-124">přiřazení role Hello by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="5019b-124">hello role assignment should look like this:</span></span>

    ```
    {"value":[{"properties":{
    "roleDefinitionId":"/providers/Microsoft.Authorization/roleDefinitions/18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
    "principalId":"{objectID}",
    "scope":"/",
    "createdOn":"2016-08-17T19:21:16.3422480Z",
    "updatedOn":"2016-08-17T19:21:16.3422480Z",
    "createdBy":"93ce6722-3638-4222-b582-78b75c5c6d65",
    "updatedBy":"93ce6722-3638-4222-b582-78b75c5c6d65"},
    "id":"/providers/Microsoft.Authorization/roleAssignments/e7dd75bc-06f6-4e71-9014-ee96a929d099",
    "type":"Microsoft.Authorization/roleAssignments",
    "name":"e7dd75bc-06f6-4e71-9014-ee96a929d099"}],
    "nextLink":null}
    ```

    <span data-ttu-id="5019b-125">Znovu uložit hello GUID z hello *název* parametr v tomto případě **e7dd75bc-06f6-4e71-9014-ee96a929d099**.</span><span class="sxs-lookup"><span data-stu-id="5019b-125">Again, save hello GUID from hello *name* parameter, in this case **e7dd75bc-06f6-4e71-9014-ee96a929d099**.</span></span>

3. <span data-ttu-id="5019b-126">Nakonec volání [roleAssignments odstranění](/rest/api/authorization/roleassignments#RoleAssignments_DeleteById) kde roleAssignmentId = název hello GUID, které jste získali v kroku 2.</span><span class="sxs-lookup"><span data-stu-id="5019b-126">Finally, call [DELETE roleAssignments](/rest/api/authorization/roleassignments#RoleAssignments_DeleteById) where roleAssignmentId = hello name GUID you found in step 2.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5019b-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5019b-127">Next steps</span></span>

- <span data-ttu-id="5019b-128">Další informace o [Správa řízení přístupu na základě rolí pomocí REST](role-based-access-control-manage-access-rest.md)</span><span class="sxs-lookup"><span data-stu-id="5019b-128">Learn more about [managing Role-Based Access Control with REST](role-based-access-control-manage-access-rest.md)</span></span>

- <span data-ttu-id="5019b-129">[Spravovat přístup k přiřazení](role-based-access-control-manage-assignments.md) v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5019b-129">[Manage access assignments](role-based-access-control-manage-assignments.md) in hello Azure portal</span></span>
