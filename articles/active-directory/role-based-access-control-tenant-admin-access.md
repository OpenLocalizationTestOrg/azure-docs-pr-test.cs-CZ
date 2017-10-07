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
# <a name="elevate-access-as-a-tenant-admin-with-role-based-access-control"></a>Zvýšení přístupu jako správce klienta s řízením přístupu na základě rolí

Řízení přístupu na základě rolí pomáhá správci klientů získat dočasný – zvýšení úrovní oprávnění přístupu tak, aby se můžete udělit oprávnění vyšší než normální. Správce klienta může zvýšit nároky samu sebe toohello role správce přístupu uživatelů v případě potřeby. Tato role poskytuje hello klienta toogrant oprávnění správce samu sebe nebo ostatní role v hello "/" oboru.

Tato funkce je důležitá, protože umožňuje hello klienta správce toosee všechny hello odběry, které existují v organizaci. Také umožňuje automatizace aplikace (jako je fakturace a auditování) tooaccess všechny odběry hello a poskytují přesné informace o stavu hello hello organizace pro správu fakturace nebo asset.  

## <a name="how-toouse-elevateaccess-toogive-tenant-access"></a>Jak toouse elevateAccess toogive klienta přístup

Základní proces Hello funguje s hello následující kroky:

1. Pomocí REST, volání *elevateAccess*, která uděluje můžete hello role správce přístupu uživatelů v "/" oboru.

    ```
    POST https://management.azure.com/providers/Microsoft.Authorization/elevateAccess?api-version=2016-07-01
    ```

2. Vytvoření [přiřazení role](/rest/api/authorization/roleassignments) tooassign všechny role v jakémkoli oboru. Hello následující příklad ukazuje hello vlastnosti pro přiřazení hello role čtenáře v "/" oboru:

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

3. Při přístupu správce uživatele můžete můžete také odstranit přiřazení role v "/" oboru.

4. Vaše oprávnění správce přístupu uživatelů odvolat, dokud se znovu požadován.


## <a name="how-tooundo-hello-elevateaccess-action"></a>Jak tooundo hello elevateAccess akce

Při volání *elevateAccess* vytvořit přiřazení role pro sebe, tak toorevoke, ty vám oprávnění potřebovat toodelete hello přiřazení.

1.  Volání [GET roleDefinitions](/rest/api/authorization/roledefinitions#RoleDefinitions_Get) kde roleName = název hello správce přístupu uživatelů toodetermine GUID role správce přístupu uživatelů hello. Hello odpověď by měla vypadat takto:

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

    Uložit hello GUID z hello *název* parametr v tomto případě **18d7d88d-d35e-4fb5-a5c3-7773c20a72d9**.

2. Volání [GET roleAssignments](/rest/api/authorization/roleassignments#RoleAssignments_Get) kde principalId = vlastní ObjectId. Rutina Vypíše seznam všech přiřazení v klientovi hello. Vyhledejte hello jeden, kde je hello oboru "/" a končí hodnoty vlastnosti RoleDefinitionId hello s rolí hello název GUID, které jste získali v kroku 1. přiřazení role Hello by měl vypadat takto:

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

    Znovu uložit hello GUID z hello *název* parametr v tomto případě **e7dd75bc-06f6-4e71-9014-ee96a929d099**.

3. Nakonec volání [roleAssignments odstranění](/rest/api/authorization/roleassignments#RoleAssignments_DeleteById) kde roleAssignmentId = název hello GUID, které jste získali v kroku 2.

## <a name="next-steps"></a>Další kroky

- Další informace o [Správa řízení přístupu na základě rolí pomocí REST](role-based-access-control-manage-access-rest.md)

- [Spravovat přístup k přiřazení](role-based-access-control-manage-assignments.md) v hello portálu Azure
