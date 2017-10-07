---
title: "Řízení přístupu na základě aaaRole se zbytkem – Azure AD | Microsoft Docs"
description: "Správa řízení přístupu na základě rolí pomocí rozhraní REST API hello"
services: active-directory
documentationcenter: na
author: andredm7
manager: femila
editor: 
ms.assetid: 1f90228a-7aac-4ea7-ad82-b57d222ab128
ms.service: active-directory
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: andredm
ms.openlocfilehash: ccd402fd4fe4583288076cac23753dd067694681
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-role-based-access-control-with-hello-rest-api"></a>Správa řízení přístupu na základě rolí pomocí rozhraní REST API hello
> [!div class="op_single_selector"]
> * [PowerShell](role-based-access-control-manage-access-powershell.md)
> * [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
> * [REST API](role-based-access-control-manage-access-rest.md)

Na základě rolí řízení přístupu (RBAC) v hello portál Azure a rozhraní API služby Azure Resource Manager pomáhá spravovat předplatné tooyour přístup a prostředky na velice přesné úrovni. Pomocí této funkce můžete udělit přístup pro uživatele, skupiny nebo objekty služby Active Directory přiřazením některé role toothem v určitém rozsahu.

## <a name="list-all-role-assignments"></a>Zobrazí seznam všech přiřazení rolí
Zobrazí všechna přiřazení rolí hello v hello zadaný obor a subscopes.

přiřazení rolí toolist, musíte mít přístup příliš`Microsoft.Authorization/roleAssignments/read` operace v oboru hello. Všechny hello předdefinované role jsou udělena toothis operace přístupu. Další informace o přiřazení rolí a správu přístupu k prostředkům Azure najdete v tématu [řízení přístupu](role-based-access-control-configure.md).

### <a name="request"></a>Žádost
Použití hello **získat** metoda s hello následující identifikátor URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version={api-version}&$filter={filter}

V rámci hello identifikátor URI proveďte hello následující náhrady toocustomize vaší žádosti:

1. Nahraďte *{oboru}* hello oboru, pro kterou chcete toolist hello přiřazení rolí. Hello následující příklady ukazují, jak toospecify hello oboru pro různé úrovně:

   * Předplatné: /subscriptions/ {id předplatného}  
   * Skupina prostředků: /subscriptions/ {id předplatného} / Skupinyprostředků/myresourcegroup1  
   * Prostředek: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Nahraďte *{api-version}* s 2015-07-01.
3. Nahraďte *{filtru}* s podmínkou hello chcete tooapply toofilter hello role přiřazení seznamu:

   * Seznam přiřazení rolí pro pouze hello zadaný obor, není včetně hello přiřazení rolí v subscopes:`atScope()`    
   * Seznam přiřazení rolí pro konkrétního uživatele, skupinu nebo aplikaci:`principalId%20eq%20'{objectId of user, group, or service principal}'`  
   * Seznam přiřazení rolí pro konkrétního uživatele, včetně těch, které jsou zděděno od skupiny |`assignedTo('{objectId of user}')`

### <a name="response"></a>Odpověď
Stavový kód: 200

```
{
  "value": [
    {
      "properties": {
        "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
        "principalId": "2f9d4375-cbf1-48e8-83c9-2a0be4cb33fb",
        "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
        "createdOn": "2015-10-08T07:28:24.3905077Z",
        "updatedOn": "2015-10-08T07:28:24.3905077Z",
        "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
        "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleAssignments/baa6e199-ad19-4667-b768-623fde31aedd",
      "type": "Microsoft.Authorization/roleAssignments",
      "name": "baa6e199-ad19-4667-b768-623fde31aedd"
    }
  ],
  "nextLink": null
}

```

## <a name="get-information-about-a-role-assignment"></a>Získat informace o přiřazení role
Získá informace o přiřazení role jednoho zadaného identifikátorem přiřazení role hello.

tooget informace o přiřazení role, je nutné mít přístup příliš`Microsoft.Authorization/roleAssignments/read` operaci. Všechny hello předdefinované role jsou udělena toothis operace přístupu. Další informace o přiřazení rolí a správu přístupu k prostředkům Azure najdete v tématu [řízení přístupu](role-based-access-control-configure.md).

### <a name="request"></a>Žádost
Použití hello **získat** metoda s hello následující identifikátor URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

V rámci hello identifikátor URI proveďte hello následující náhrady toocustomize vaší žádosti:

1. Nahraďte *{oboru}* hello oboru, pro kterou chcete toolist hello přiřazení rolí. Hello následující příklady ukazují, jak toospecify hello oboru pro různé úrovně:

   * Předplatné: /subscriptions/ {id předplatného}  
   * Skupina prostředků: /subscriptions/ {id předplatného} / Skupinyprostředků/myresourcegroup1  
   * Prostředek: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Nahraďte *{id role přiřazení}* s identifikátorem GUID hello hello přiřazení role.
3. Nahraďte *{api-version}* s 2015-07-01.

### <a name="response"></a>Odpověď
Stavový kód: 200

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "principalId": "672f1afa-526a-4ef6-819c-975c7cd79022",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "createdOn": "2015-10-05T08:36:26.4014813Z",
    "updatedOn": "2015-10-05T08:36:26.4014813Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleAssignments/196965ae-6088-4121-a92a-f1e33fdcc73e",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "196965ae-6088-4121-a92a-f1e33fdcc73e"
}

```

## <a name="create-a-role-assignment"></a>Umožňuje vytvořit přiřazení Role
Umožňuje vytvořit roli přiřazení v hello zadaný obor pro hello zadaný hlavní poskytující hello zadané roli.

toocreate přiřazení role, je nutné mít přístup příliš`Microsoft.Authorization/roleAssignments/write` operaci. Hello předdefinovaných rolí, pouze *vlastníka* a *správce uživatelského přístupu* jsou udělena toothis operace přístupu. Další informace o přiřazení rolí a správu přístupu k prostředkům Azure najdete v tématu [řízení přístupu](role-based-access-control-configure.md).

### <a name="request"></a>Žádost
Použití hello **PUT** metoda s hello následující identifikátor URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

V rámci hello identifikátor URI proveďte hello následující náhrady toocustomize vaší žádosti:

1. Nahraďte *{oboru}* hello oboru, ve kterém chcete toocreate přiřazení rolí hello. Když vytvoříte přiřazení role v nadřazeném oboru, že všechny podřízené obory dědí hello stejné přiřazení role. Hello následující příklady ukazují, jak toospecify hello oboru pro různé úrovně:

   * Předplatné: /subscriptions/ {id předplatného}  
   * Skupina prostředků: /subscriptions/ {id předplatného} / Skupinyprostředků/myresourcegroup1   
   * Prostředek: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Nahraďte *{id role přiřazení}* s nový identifikátor GUID, který se stane identifikátor GUID hello hello nové přiřazení role.
3. Nahraďte *{api-version}* s 2015-07-01.

Hello tělo žádosti zadejte hodnoty hello v hello následující formát:

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b"
  }
}

```

| Název elementu | Požaduje se | Typ | Popis |
| --- | --- | --- | --- |
| hodnoty vlastnosti roleDefinitionId |Ano |Řetězec |identifikátor Hello hello role. Formát Hello hello identifikátoru je:`{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id-guid}` |
| principalId |Ano |Řetězec |objectId objektu hello Azure AD (uživatele, skupiny nebo instanční objekt) toowhich hello role je přiřazená. |

### <a name="response"></a>Odpověď
Stavový kód: 201

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND",
    "createdOn": "2015-12-16T00:27:19.6447515Z",
    "updatedOn": "2015-12-16T00:27:19.6447515Z",
    "createdBy": null,
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleAssignments/2e9e86c8-0e91-4958-b21f-20f51f27bab2",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "2e9e86c8-0e91-4958-b21f-20f51f27bab2"
}

```

## <a name="delete-a-role-assignment"></a>Umožňuje odstranit přiřazení Role
Odstranit přiřazení role v hello zadaný obor.

toodelete přiřazení role musí mít přístup toohello `Microsoft.Authorization/roleAssignments/delete` operaci. Hello předdefinovaných rolí, pouze *vlastníka* a *správce uživatelského přístupu* jsou udělena toothis operace přístupu. Další informace o přiřazení rolí a správu přístupu k prostředkům Azure najdete v tématu [řízení přístupu](role-based-access-control-configure.md).

### <a name="request"></a>Žádost
Použití hello **odstranit** metoda s hello následující identifikátor URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

V rámci hello identifikátor URI proveďte hello následující náhrady toocustomize vaší žádosti:

1. Nahraďte *{oboru}* hello oboru, ve kterém chcete toocreate přiřazení rolí hello. Hello následující příklady ukazují, jak toospecify hello oboru pro různé úrovně:

   * Předplatné: /subscriptions/ {id předplatného}  
   * Skupina prostředků: /subscriptions/ {id předplatného} / Skupinyprostředků/myresourcegroup1  
   * Prostředek: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Nahraďte *{id role přiřazení}* s id přiřazení role hello identifikátor GUID.
3. Nahraďte *{api-version}* s 2015-07-01.

### <a name="response"></a>Odpověď
Stavový kód: 200

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND",
    "createdOn": "2015-12-17T23:21:40.8921564Z",
    "updatedOn": "2015-12-17T23:21:40.8921564Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleAssignments/5eec22ee-ea5c-431e-8f41-82c560706fd2",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "5eec22ee-ea5c-431e-8f41-82c560706fd2"
}

```

## <a name="list-all-roles"></a>Zobrazí seznam všech rolí
Zobrazí seznam všech rolí hello, které jsou k dispozici pro přiřazení v hello zadaný obor.

toolist role, je nutné mít přístup příliš`Microsoft.Authorization/roleDefinitions/read` operace v oboru hello. Všechny hello předdefinované role jsou udělena toothis operace přístupu. Další informace o přiřazení rolí a správu přístupu k prostředkům Azure najdete v tématu [řízení přístupu](role-based-access-control-configure.md).

### <a name="request"></a>Žádost
Použití hello **získat** metoda s hello následující identifikátor URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version={api-version}&$filter={filter}

V rámci hello identifikátor URI proveďte hello následující náhrady toocustomize vaší žádosti:

1. Nahraďte *{oboru}* hello oboru, pro kterou chcete toolist hello role. Hello následující příklady ukazují, jak toospecify hello oboru pro různé úrovně:

   * Předplatné: /subscriptions/ {id předplatného}  
   * Skupina prostředků: /subscriptions/ {id předplatného} / Skupinyprostředků/myresourcegroup1  
   * /Subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1 prostředků  
2. Nahraďte *{api-version}* s 2015-07-01.
3. Nahraďte *{filtru}* s podmínkou hello chcete tooapply toofilter hello seznamu rolí:

   * Seznam rolí, které jsou k dispozici pro přiřazení v hello zadaný obor a všechny její podřízené obory:`atScopeAndBelow()`
   * Vyhledejte roli pomocí přesný zobrazovaný název: `roleName%20eq%20'{role-display-name}'`. Použijte hello kódovaná adresou URL formu hello přesný zobrazovaný název role hello. Pro instanci,`$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |

### <a name="response"></a>Odpověď
Stavový kód: 200

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access toothem, and not hello virtual network or storage account they\u2019re connected to.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "Microsoft.Authorization/*/read",
              "Microsoft.Compute/availabilitySets/*",
              "Microsoft.Compute/locations/*",
              "Microsoft.Compute/virtualMachines/*",
              "Microsoft.Compute/virtualMachineScaleSets/*",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
              "Microsoft.Network/loadBalancers/read",
              "Microsoft.Network/locations/*",
              "Microsoft.Network/networkInterfaces/*",
              "Microsoft.Network/networkSecurityGroups/join/action",
              "Microsoft.Network/networkSecurityGroups/read",
              "Microsoft.Network/publicIPAddresses/join/action",
              "Microsoft.Network/publicIPAddresses/read",
              "Microsoft.Network/virtualNetworks/read",
              "Microsoft.Network/virtualNetworks/subnets/join/action",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Storage/storageAccounts/listKeys/action",
              "Microsoft.Storage/storageAccounts/read",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "2015-06-02T00:18:27.3542698Z",
        "updatedOn": "2015-12-08T03:16:55.6170255Z",
        "createdBy": null,
        "updatedBy": null
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
      "type": "Microsoft.Authorization/roleDefinitions",
      "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
    }
  ],
  "nextLink": null
}

```

## <a name="get-information-about-a-role"></a>Získat informace o roli
Získá informace o jedné role určeného identifikátor definice role hello. tooget informace o jedné role pomocí názvu zobrazení, naleznete v [seznam všech rolí](role-based-access-control-manage-access-rest.md#list-all-roles).

tooget informace o roli, musíte mít přístup příliš`Microsoft.Authorization/roleDefinitions/read` operaci. Všechny hello předdefinované role jsou udělena toothis operace přístupu. Další informace o přiřazení rolí a správu přístupu k prostředkům Azure najdete v tématu [řízení přístupu](role-based-access-control-configure.md).

### <a name="request"></a>Žádost
Použití hello **získat** metoda s hello následující identifikátor URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

V rámci hello identifikátor URI proveďte hello následující náhrady toocustomize vaší žádosti:

1. Nahraďte *{oboru}* hello oboru, pro kterou chcete toolist hello přiřazení rolí. Hello následující příklady ukazují, jak toospecify hello oboru pro různé úrovně:

   * Předplatné: /subscriptions/ {id předplatného}  
   * Skupina prostředků: /subscriptions/ {id předplatného} / Skupinyprostředků/myresourcegroup1  
   * Prostředek: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Nahraďte *{id role definice}* s identifikátorem GUID hello definice role hello.
3. Nahraďte *{api-version}* s 2015-07-01.

### <a name="response"></a>Odpověď
Stavový kód: 200

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access toothem, and not hello virtual network or storage account they\u2019re connected to.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "Microsoft.Authorization/*/read",
              "Microsoft.Compute/availabilitySets/*",
              "Microsoft.Compute/locations/*",
              "Microsoft.Compute/virtualMachines/*",
              "Microsoft.Compute/virtualMachineScaleSets/*",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
              "Microsoft.Network/loadBalancers/read",
              "Microsoft.Network/locations/*",
              "Microsoft.Network/networkInterfaces/*",
              "Microsoft.Network/networkSecurityGroups/join/action",
              "Microsoft.Network/networkSecurityGroups/read",
              "Microsoft.Network/publicIPAddresses/join/action",
              "Microsoft.Network/publicIPAddresses/read",
              "Microsoft.Network/virtualNetworks/read",
              "Microsoft.Network/virtualNetworks/subnets/join/action",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Storage/storageAccounts/listKeys/action",
              "Microsoft.Storage/storageAccounts/read",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "2015-06-02T00:18:27.3542698Z",
        "updatedOn": "2015-12-08T03:16:55.6170255Z",
        "createdBy": null,
        "updatedBy": null
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
      "type": "Microsoft.Authorization/roleDefinitions",
      "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
    }
  ],
  "nextLink": null
}

```

## <a name="create-a-custom-role"></a>Vytvořit vlastní roli
Vytvořte vlastní roli.

toocreate vlastní roli, musíte mít přístup příliš`Microsoft.Authorization/roleDefinitions/write` operace na všechny hello `AssignableScopes`. Hello předdefinovaných rolí, pouze *vlastníka* a *správce uživatelského přístupu* jsou udělena toothis operace přístupu. Další informace o přiřazení rolí a správu přístupu k prostředkům Azure najdete v tématu [řízení přístupu](role-based-access-control-configure.md).

### <a name="request"></a>Žádost
Použití hello **PUT** metoda s hello následující identifikátor URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

V rámci hello identifikátor URI proveďte hello následující náhrady toocustomize vaší žádosti:

1. Nahraďte *{oboru}* s hello první *AssignableScope* hello vlastní role. Hello následující příklady ukazují, jak toospecify hello oboru pro různé úrovně.

   * Předplatné: /subscriptions/ {id předplatného}  
   * Skupina prostředků: /subscriptions/ {id předplatného} / Skupinyprostředků/myresourcegroup1  
   * Prostředek: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Nahraďte *{id role definice}* s nový identifikátor GUID, který se stane identifikátor GUID hello hello novou vlastní roli.
3. Nahraďte *{api-version}* s 2015-07-01.

Hello tělo žádosti zadejte hodnoty hello v hello následující formát:

```
{
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "properties": {
    "roleName": "Virtual Machine Operator",
    "description": "Lets you monitor virtual machines and restart them.",
    "type": "CustomRole",
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
  }
}

```

| Název elementu | Požaduje se | Typ | Popis |
| --- | --- | --- | --- |
| jméno |Ano |Řetězec |Identifikátor GUID hello vlastní role. |
| properties.roleName |Ano |Řetězec |Zobrazovaný název hello vlastní role. Maximální velikost 128 znaků. |
| Properties.Description |Ne |Řetězec |Popis vlastní role hello. Maximální velikost 1024 znaků. |
| Properties.Type |Ano |Řetězec |Nastavení příliš "CustomRole." |
| Properties.Permissions.Actions |Ano |řetězec] |Pole Akce řetězce zadání hello operations udělují hello vlastní role. |
| properties.permissions.notActions |Ne |řetězec] |Pole řetězců akce zadání hello operations tooexclude hello operacích udělují hello vlastní role. |
| properties.assignableScopes |Ano |řetězec] |Pole obory, ve které hello je možné vlastní role. |

### <a name="response"></a>Odpověď
Stavový kód: 201

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-18T00:10:51.4662695Z",
    "updatedOn": "2015-12-18T00:10:51.4662695Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7"
}

```

## <a name="update-a-custom-role"></a>Aktualizovat vlastní Role
Upravte vlastní roli.

toomodify vlastní roli, musíte mít přístup příliš`Microsoft.Authorization/roleDefinitions/write` operace na všechny hello `AssignableScopes`. Hello předdefinovaných rolí, pouze *vlastníka* a *správce uživatelského přístupu* jsou udělena toothis operace přístupu. Další informace o přiřazení rolí a správu přístupu k prostředkům Azure najdete v tématu [řízení přístupu](role-based-access-control-configure.md).

### <a name="request"></a>Žádost
Použití hello **PUT** metoda s hello následující identifikátor URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

V rámci hello identifikátor URI proveďte hello následující náhrady toocustomize vaší žádosti:

1. Nahraďte *{oboru}* s hello první *AssignableScope* hello vlastní role. Hello následující příklady ukazují, jak toospecify hello oboru pro různé úrovně:

   * Předplatné: /subscriptions/ {id předplatného}  
   * Skupina prostředků: /subscriptions/ {id předplatného} / Skupinyprostředků/myresourcegroup1  
   * Prostředek: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Nahraďte *{id role definice}* s identifikátorem GUID hello hello vlastní role.
3. Nahraďte *{api-version}* s 2015-07-01.

Hello tělo žádosti zadejte hodnoty hello v hello následující formát:

```
{
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "properties": {
    "roleName": "Virtual Machine Operator",
    "description": "Lets you monitor virtual machines and restart them.",
    "type": "CustomRole",
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
  }
}

```

| Název elementu | Požaduje se | Typ | Popis |
| --- | --- | --- | --- |
| jméno |Ano |Řetězec |Identifikátor GUID hello vlastní role. |
| properties.roleName |Ano |Řetězec |Zobrazovaný název hello aktualizovat vlastní role. |
| Properties.Description |Ne |Řetězec |Popis hello aktualizovat vlastní role. |
| Properties.Type |Ano |Řetězec |Nastavení příliš "CustomRole." |
| Properties.Permissions.Actions |Ano |řetězec] |Pole řetězců akce zadání hello operations toowhich hello aktualizovat vlastní role uděluje přístup. |
| properties.permissions.notActions |Ne |řetězec] |Pole Akce řetězce zadání tooexclude operations hello hello operacích, které hello aktualizovat vlastní role uděluje. |
| properties.assignableScopes |Ano |řetězec] |Pole obory, ve které hello je možné aktualizované vlastní role. |

### <a name="response"></a>Odpověď
Stavový kód: 201

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-18T00:10:51.4662695Z",
    "updatedOn": "2015-12-18T00:10:51.4662695Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7"
}

```

## <a name="delete-a-custom-role"></a>Odstranit vlastní roli
Odstraňte vlastní roli.

toodelete vlastní roli, musíte mít přístup příliš`Microsoft.Authorization/roleDefinitions/delete` operace na všechny hello `AssignableScopes`. Hello předdefinovaných rolí, pouze *vlastníka* a *správce uživatelského přístupu* jsou udělena toothis operace přístupu. Další informace o přiřazení rolí a správu přístupu k prostředkům Azure najdete v tématu [řízení přístupu](role-based-access-control-configure.md).

### <a name="request"></a>Žádost
Použití hello **odstranit** metoda s hello následující identifikátor URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

V rámci hello identifikátor URI proveďte hello následující náhrady toocustomize vaší žádosti:

1. Nahraďte *{oboru}* hello oboru, ve kterém chcete definice role toodelete hello. Hello následující příklady ukazují, jak toospecify hello oboru pro různé úrovně:

   * Předplatné: /subscriptions/ {id předplatného}  
   * Skupina prostředků: /subscriptions/ {id předplatného} / Skupinyprostředků/myresourcegroup1  
   * Prostředek: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Nahraďte *{id role definice}* s id definice role GUID hello hello vlastní role.
3. Nahraďte *{api-version}* s 2015-07-01.

### <a name="response"></a>Odpověď
Stavový kód: 200

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-16T00:07:02.9236555Z",
    "updatedOn": "2015-12-16T00:07:02.9236555Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/0bd62a70-e1b8-4e0b-a7c2-75cab365c95b",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "0bd62a70-e1b8-4e0b-a7c2-75cab365c95b"
}

```

## <a name="next-steps"></a>Další kroky

[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
