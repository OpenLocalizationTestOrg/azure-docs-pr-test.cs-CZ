---
title: "Správce tenanta zvýšení přístupu – Azure AD | Microsoft Docs"
description: "Toto téma popisuje předdefinovaných do rolí pro řízení přístupu na základě role (RBAC)."
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
ms.openlocfilehash: bf64a92b359a6f68d84fa5ee17eda64ed6371990
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="elevate-access-as-a-tenant-admin-with-role-based-access-control"></a><span data-ttu-id="16128-103">Zvýšení přístupu jako správce klienta s řízením přístupu na základě rolí</span><span class="sxs-lookup"><span data-stu-id="16128-103">Elevate access as a tenant admin with Role-Based Access Control</span></span>

<span data-ttu-id="16128-104">Řízení přístupu na základě rolí pomáhá správci klientů získat dočasný – zvýšení úrovní oprávnění přístupu tak, aby se můžete udělit oprávnění vyšší než normální.</span><span class="sxs-lookup"><span data-stu-id="16128-104">Role-based Access Control helps tenant administrators get temporary elevations in access so that they can grant higher permissions than normal.</span></span> <span data-ttu-id="16128-105">Správce klienta můžete zvýšit samu sebe roli správce přístupu uživatelů v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="16128-105">A tenant admin can elevate herself to the User Access Administrator role when needed.</span></span> <span data-ttu-id="16128-106">Tato role poskytuje klienta oprávnění správce k udělení samu sebe nebo ostatní role v oboru "/".</span><span class="sxs-lookup"><span data-stu-id="16128-106">That role gives the tenant admin permissions to grant herself or others roles at the "/" scope.</span></span>

<span data-ttu-id="16128-107">Tato funkce je důležitá, protože umožňuje Správce tenanta zobrazíte všechny odběry, které existují v organizaci.</span><span class="sxs-lookup"><span data-stu-id="16128-107">This feature is important because it allows the tenant admin to see all the subscriptions that exist in an organization.</span></span> <span data-ttu-id="16128-108">Také to umožňuje pro aplikace, automatizace (jako je fakturace a auditování) pro přístup k Všechna předplatná a poskytují přesné informace o stavu organizace pro správu fakturace nebo asset.</span><span class="sxs-lookup"><span data-stu-id="16128-108">It also allows for automation apps (like invoicing and auditing) to access all the subscriptions and provide an accurate view of the state of the organization for billing or asset management.</span></span>  

## <a name="how-to-use-elevateaccess-to-give-tenant-access"></a><span data-ttu-id="16128-109">Jak používat elevateAccess přístup klienta</span><span class="sxs-lookup"><span data-stu-id="16128-109">How to use elevateAccess to give tenant access</span></span>

<span data-ttu-id="16128-110">Základní proces funguje pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="16128-110">The basic process works with the following steps:</span></span>

1. <span data-ttu-id="16128-111">Pomocí REST, volání *elevateAccess*, která vám uděluje role správce přístupu uživatelů v "/" oboru.</span><span class="sxs-lookup"><span data-stu-id="16128-111">Using REST, call *elevateAccess*, which grants you the User Access Administrator role at "/" scope.</span></span>

    ```
    POST https://management.azure.com/providers/Microsoft.Authorization/elevateAccess?api-version=2016-07-01
    ```

2. <span data-ttu-id="16128-112">Vytvoření [přiřazení role](/rest/api/authorization/roleassignments) přiřadit žádnou roli v jakémkoli oboru.</span><span class="sxs-lookup"><span data-stu-id="16128-112">Create a [role assignment](/rest/api/authorization/roleassignments) to assign any role at any scope.</span></span> <span data-ttu-id="16128-113">Následující příklad ukazuje vlastnosti přiřazení role čtenáře v "/" oboru:</span><span class="sxs-lookup"><span data-stu-id="16128-113">The following example shows the properties for assigning the Reader role at "/" scope:</span></span>

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

3. <span data-ttu-id="16128-114">Při přístupu správce uživatele můžete můžete také odstranit přiřazení role v "/" oboru.</span><span class="sxs-lookup"><span data-stu-id="16128-114">While a User Access Admin, you can also delete role assignments at "/" scope.</span></span>

4. <span data-ttu-id="16128-115">Vaše oprávnění správce přístupu uživatelů odvolat, dokud se znovu požadován.</span><span class="sxs-lookup"><span data-stu-id="16128-115">Revoke your User Access Admin privileges until they're needed again.</span></span>


## <a name="how-to-undo-the-elevateaccess-action"></a><span data-ttu-id="16128-116">Vrácení zpět elevateAccess akce</span><span class="sxs-lookup"><span data-stu-id="16128-116">How to undo the elevateAccess action</span></span>

<span data-ttu-id="16128-117">Při volání *elevateAccess* vytvořit přiřazení role pro sebe, aby tato oprávnění odvolat, budete muset odstranit přiřazení.</span><span class="sxs-lookup"><span data-stu-id="16128-117">When you call *elevateAccess* you create a role assignment for yourself, so to revoke those privileges you need to delete the assignment.</span></span>

1.  <span data-ttu-id="16128-118">Volání [GET roleDefinitions](/rest/api/authorization/roledefinitions#RoleDefinitions_Get) kde roleName = správce přístupu uživatelů k určení názvu GUID role správce přístupu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="16128-118">Call [GET roleDefinitions](/rest/api/authorization/roledefinitions#RoleDefinitions_Get) where roleName = User Access Administrator to determine the name GUID of the User Access Administrator role.</span></span> <span data-ttu-id="16128-119">Odpověď by měla vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="16128-119">The response should look like this:</span></span>

    ```
    {"value":[{"properties":{
    "roleName":"User Access Administrator",
    "type":"BuiltInRole",
    "description":"Lets you manage user access to Azure resources.",
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

    <span data-ttu-id="16128-120">Uložte identifikátor GUID z *název* parametr v tomto případě **18d7d88d-d35e-4fb5-a5c3-7773c20a72d9**.</span><span class="sxs-lookup"><span data-stu-id="16128-120">Save the GUID from the *name* parameter, in this case **18d7d88d-d35e-4fb5-a5c3-7773c20a72d9**.</span></span>

2. <span data-ttu-id="16128-121">Volání [GET roleAssignments](/rest/api/authorization/roleassignments#RoleAssignments_Get) kde principalId = vlastní ObjectId.</span><span class="sxs-lookup"><span data-stu-id="16128-121">Call [GET roleAssignments](/rest/api/authorization/roleassignments#RoleAssignments_Get) where principalId = your own ObjectId.</span></span> <span data-ttu-id="16128-122">Rutina Vypíše seznam všech přiřazení v klientovi.</span><span class="sxs-lookup"><span data-stu-id="16128-122">This lists all your assignments in the tenant.</span></span> <span data-ttu-id="16128-123">Vyhledejte ten, kde je oboru "/" a končí hodnoty vlastnosti RoleDefinitionId s názvem role GUID, které jste získali v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="16128-123">Look for the one where the scope is "/" and the RoleDefinitionId ends with the role name GUID you found in step 1.</span></span> <span data-ttu-id="16128-124">Přiřazení role by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="16128-124">The role assignment should look like this:</span></span>

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

    <span data-ttu-id="16128-125">Znovu, uložte identifikátor GUID z *název* parametr v tomto případě **e7dd75bc-06f6-4e71-9014-ee96a929d099**.</span><span class="sxs-lookup"><span data-stu-id="16128-125">Again, save the GUID from the *name* parameter, in this case **e7dd75bc-06f6-4e71-9014-ee96a929d099**.</span></span>

3. <span data-ttu-id="16128-126">Nakonec volání [roleAssignments odstranění](/rest/api/authorization/roleassignments#RoleAssignments_DeleteById) kde roleAssignmentId = název GUID, které jste získali v kroku 2.</span><span class="sxs-lookup"><span data-stu-id="16128-126">Finally, call [DELETE roleAssignments](/rest/api/authorization/roleassignments#RoleAssignments_DeleteById) where roleAssignmentId = the name GUID you found in step 2.</span></span>

## <a name="next-steps"></a><span data-ttu-id="16128-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="16128-127">Next steps</span></span>

- <span data-ttu-id="16128-128">Další informace o [Správa řízení přístupu na základě rolí pomocí REST](role-based-access-control-manage-access-rest.md)</span><span class="sxs-lookup"><span data-stu-id="16128-128">Learn more about [managing Role-Based Access Control with REST](role-based-access-control-manage-access-rest.md)</span></span>

- <span data-ttu-id="16128-129">[Spravovat přístup k přiřazení](role-based-access-control-manage-assignments.md) na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="16128-129">[Manage access assignments](role-based-access-control-manage-assignments.md) in the Azure portal</span></span>
