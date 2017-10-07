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
# <a name="manage-role-based-access-control-with-hello-rest-api"></a><span data-ttu-id="06b10-103">Správa řízení přístupu na základě rolí pomocí rozhraní REST API hello</span><span class="sxs-lookup"><span data-stu-id="06b10-103">Manage Role-Based Access Control with hello REST API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="06b10-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="06b10-104">PowerShell</span></span>](role-based-access-control-manage-access-powershell.md)
> * [<span data-ttu-id="06b10-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="06b10-105">Azure CLI</span></span>](role-based-access-control-manage-access-azure-cli.md)
> * [<span data-ttu-id="06b10-106">REST API</span><span class="sxs-lookup"><span data-stu-id="06b10-106">REST API</span></span>](role-based-access-control-manage-access-rest.md)

<span data-ttu-id="06b10-107">Na základě rolí řízení přístupu (RBAC) v hello portál Azure a rozhraní API služby Azure Resource Manager pomáhá spravovat předplatné tooyour přístup a prostředky na velice přesné úrovni.</span><span class="sxs-lookup"><span data-stu-id="06b10-107">Role-Based Access Control (RBAC) in hello Azure portal and Azure Resource Manager API helps you manage access tooyour subscription and resources at a fine-grained level.</span></span> <span data-ttu-id="06b10-108">Pomocí této funkce můžete udělit přístup pro uživatele, skupiny nebo objekty služby Active Directory přiřazením některé role toothem v určitém rozsahu.</span><span class="sxs-lookup"><span data-stu-id="06b10-108">With this feature, you can grant access for Active Directory users, groups, or service principals by assigning some roles toothem at a particular scope.</span></span>

## <a name="list-all-role-assignments"></a><span data-ttu-id="06b10-109">Zobrazí seznam všech přiřazení rolí</span><span class="sxs-lookup"><span data-stu-id="06b10-109">List all role assignments</span></span>
<span data-ttu-id="06b10-110">Zobrazí všechna přiřazení rolí hello v hello zadaný obor a subscopes.</span><span class="sxs-lookup"><span data-stu-id="06b10-110">Lists all hello role assignments at hello specified scope and subscopes.</span></span>

<span data-ttu-id="06b10-111">přiřazení rolí toolist, musíte mít přístup příliš`Microsoft.Authorization/roleAssignments/read` operace v oboru hello.</span><span class="sxs-lookup"><span data-stu-id="06b10-111">toolist role assignments, you must have access too`Microsoft.Authorization/roleAssignments/read` operation at hello scope.</span></span> <span data-ttu-id="06b10-112">Všechny hello předdefinované role jsou udělena toothis operace přístupu.</span><span class="sxs-lookup"><span data-stu-id="06b10-112">All hello built-in roles are granted access toothis operation.</span></span> <span data-ttu-id="06b10-113">Další informace o přiřazení rolí a správu přístupu k prostředkům Azure najdete v tématu [řízení přístupu](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="06b10-113">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="06b10-114">Žádost</span><span class="sxs-lookup"><span data-stu-id="06b10-114">Request</span></span>
<span data-ttu-id="06b10-115">Použití hello **získat** metoda s hello následující identifikátor URI:</span><span class="sxs-lookup"><span data-stu-id="06b10-115">Use hello **GET** method with hello following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version={api-version}&$filter={filter}

<span data-ttu-id="06b10-116">V rámci hello identifikátor URI proveďte hello následující náhrady toocustomize vaší žádosti:</span><span class="sxs-lookup"><span data-stu-id="06b10-116">Within hello URI, make hello following substitutions toocustomize your request:</span></span>

1. <span data-ttu-id="06b10-117">Nahraďte *{oboru}* hello oboru, pro kterou chcete toolist hello přiřazení rolí.</span><span class="sxs-lookup"><span data-stu-id="06b10-117">Replace *{scope}* with hello scope for which you wish toolist hello role assignments.</span></span> <span data-ttu-id="06b10-118">Hello následující příklady ukazují, jak toospecify hello oboru pro různé úrovně:</span><span class="sxs-lookup"><span data-stu-id="06b10-118">hello following examples show how toospecify hello scope for different levels:</span></span>

   * <span data-ttu-id="06b10-119">Předplatné: /subscriptions/ {id předplatného}</span><span class="sxs-lookup"><span data-stu-id="06b10-119">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="06b10-120">Skupina prostředků: /subscriptions/ {id předplatného} / Skupinyprostředků/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="06b10-120">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="06b10-121">Prostředek: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="06b10-121">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="06b10-122">Nahraďte *{api-version}* s 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="06b10-122">Replace *{api-version}* with 2015-07-01.</span></span>
3. <span data-ttu-id="06b10-123">Nahraďte *{filtru}* s podmínkou hello chcete tooapply toofilter hello role přiřazení seznamu:</span><span class="sxs-lookup"><span data-stu-id="06b10-123">Replace *{filter}* with hello condition that you wish tooapply toofilter hello role assignment list:</span></span>

   * <span data-ttu-id="06b10-124">Seznam přiřazení rolí pro pouze hello zadaný obor, není včetně hello přiřazení rolí v subscopes:`atScope()`</span><span class="sxs-lookup"><span data-stu-id="06b10-124">List role assignments for only hello specified scope, not including hello role assignments at subscopes: `atScope()`</span></span>    
   * <span data-ttu-id="06b10-125">Seznam přiřazení rolí pro konkrétního uživatele, skupinu nebo aplikaci:`principalId%20eq%20'{objectId of user, group, or service principal}'`</span><span class="sxs-lookup"><span data-stu-id="06b10-125">List role assignments for a specific user, group, or application: `principalId%20eq%20'{objectId of user, group, or service principal}'`</span></span>  
   * <span data-ttu-id="06b10-126">Seznam přiřazení rolí pro konkrétního uživatele, včetně těch, které jsou zděděno od skupiny |`assignedTo('{objectId of user}')`</span><span class="sxs-lookup"><span data-stu-id="06b10-126">List role assignments for a specific user, including ones inherited from groups | `assignedTo('{objectId of user}')`</span></span>

### <a name="response"></a><span data-ttu-id="06b10-127">Odpověď</span><span class="sxs-lookup"><span data-stu-id="06b10-127">Response</span></span>
<span data-ttu-id="06b10-128">Stavový kód: 200</span><span class="sxs-lookup"><span data-stu-id="06b10-128">Status code: 200</span></span>

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

## <a name="get-information-about-a-role-assignment"></a><span data-ttu-id="06b10-129">Získat informace o přiřazení role</span><span class="sxs-lookup"><span data-stu-id="06b10-129">Get information about a role assignment</span></span>
<span data-ttu-id="06b10-130">Získá informace o přiřazení role jednoho zadaného identifikátorem přiřazení role hello.</span><span class="sxs-lookup"><span data-stu-id="06b10-130">Gets information about a single role assignment specified by hello role assignment identifier.</span></span>

<span data-ttu-id="06b10-131">tooget informace o přiřazení role, je nutné mít přístup příliš`Microsoft.Authorization/roleAssignments/read` operaci.</span><span class="sxs-lookup"><span data-stu-id="06b10-131">tooget information about a role assignment, you must have access too`Microsoft.Authorization/roleAssignments/read` operation.</span></span> <span data-ttu-id="06b10-132">Všechny hello předdefinované role jsou udělena toothis operace přístupu.</span><span class="sxs-lookup"><span data-stu-id="06b10-132">All hello built-in roles are granted access toothis operation.</span></span> <span data-ttu-id="06b10-133">Další informace o přiřazení rolí a správu přístupu k prostředkům Azure najdete v tématu [řízení přístupu](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="06b10-133">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="06b10-134">Žádost</span><span class="sxs-lookup"><span data-stu-id="06b10-134">Request</span></span>
<span data-ttu-id="06b10-135">Použití hello **získat** metoda s hello následující identifikátor URI:</span><span class="sxs-lookup"><span data-stu-id="06b10-135">Use hello **GET** method with hello following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

<span data-ttu-id="06b10-136">V rámci hello identifikátor URI proveďte hello následující náhrady toocustomize vaší žádosti:</span><span class="sxs-lookup"><span data-stu-id="06b10-136">Within hello URI, make hello following substitutions toocustomize your request:</span></span>

1. <span data-ttu-id="06b10-137">Nahraďte *{oboru}* hello oboru, pro kterou chcete toolist hello přiřazení rolí.</span><span class="sxs-lookup"><span data-stu-id="06b10-137">Replace *{scope}* with hello scope for which you wish toolist hello role assignments.</span></span> <span data-ttu-id="06b10-138">Hello následující příklady ukazují, jak toospecify hello oboru pro různé úrovně:</span><span class="sxs-lookup"><span data-stu-id="06b10-138">hello following examples show how toospecify hello scope for different levels:</span></span>

   * <span data-ttu-id="06b10-139">Předplatné: /subscriptions/ {id předplatného}</span><span class="sxs-lookup"><span data-stu-id="06b10-139">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="06b10-140">Skupina prostředků: /subscriptions/ {id předplatného} / Skupinyprostředků/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="06b10-140">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="06b10-141">Prostředek: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="06b10-141">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="06b10-142">Nahraďte *{id role přiřazení}* s identifikátorem GUID hello hello přiřazení role.</span><span class="sxs-lookup"><span data-stu-id="06b10-142">Replace *{role-assignment-id}* with hello GUID identifier of hello role assignment.</span></span>
3. <span data-ttu-id="06b10-143">Nahraďte *{api-version}* s 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="06b10-143">Replace *{api-version}* with 2015-07-01.</span></span>

### <a name="response"></a><span data-ttu-id="06b10-144">Odpověď</span><span class="sxs-lookup"><span data-stu-id="06b10-144">Response</span></span>
<span data-ttu-id="06b10-145">Stavový kód: 200</span><span class="sxs-lookup"><span data-stu-id="06b10-145">Status code: 200</span></span>

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

## <a name="create-a-role-assignment"></a><span data-ttu-id="06b10-146">Umožňuje vytvořit přiřazení Role</span><span class="sxs-lookup"><span data-stu-id="06b10-146">Create a Role Assignment</span></span>
<span data-ttu-id="06b10-147">Umožňuje vytvořit roli přiřazení v hello zadaný obor pro hello zadaný hlavní poskytující hello zadané roli.</span><span class="sxs-lookup"><span data-stu-id="06b10-147">Create a role assignment at hello specified scope for hello specified principal granting hello specified role.</span></span>

<span data-ttu-id="06b10-148">toocreate přiřazení role, je nutné mít přístup příliš`Microsoft.Authorization/roleAssignments/write` operaci.</span><span class="sxs-lookup"><span data-stu-id="06b10-148">toocreate a role assignment, you must have access too`Microsoft.Authorization/roleAssignments/write` operation.</span></span> <span data-ttu-id="06b10-149">Hello předdefinovaných rolí, pouze *vlastníka* a *správce uživatelského přístupu* jsou udělena toothis operace přístupu.</span><span class="sxs-lookup"><span data-stu-id="06b10-149">Of hello built-in roles, only *Owner* and *User Access Administrator* are granted access toothis operation.</span></span> <span data-ttu-id="06b10-150">Další informace o přiřazení rolí a správu přístupu k prostředkům Azure najdete v tématu [řízení přístupu](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="06b10-150">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="06b10-151">Žádost</span><span class="sxs-lookup"><span data-stu-id="06b10-151">Request</span></span>
<span data-ttu-id="06b10-152">Použití hello **PUT** metoda s hello následující identifikátor URI:</span><span class="sxs-lookup"><span data-stu-id="06b10-152">Use hello **PUT** method with hello following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

<span data-ttu-id="06b10-153">V rámci hello identifikátor URI proveďte hello následující náhrady toocustomize vaší žádosti:</span><span class="sxs-lookup"><span data-stu-id="06b10-153">Within hello URI, make hello following substitutions toocustomize your request:</span></span>

1. <span data-ttu-id="06b10-154">Nahraďte *{oboru}* hello oboru, ve kterém chcete toocreate přiřazení rolí hello.</span><span class="sxs-lookup"><span data-stu-id="06b10-154">Replace *{scope}* with hello scope at which you wish toocreate hello role assignments.</span></span> <span data-ttu-id="06b10-155">Když vytvoříte přiřazení role v nadřazeném oboru, že všechny podřízené obory dědí hello stejné přiřazení role.</span><span class="sxs-lookup"><span data-stu-id="06b10-155">When you create a role assignment at a parent scope, all child scopes inherit hello same role assignment.</span></span> <span data-ttu-id="06b10-156">Hello následující příklady ukazují, jak toospecify hello oboru pro různé úrovně:</span><span class="sxs-lookup"><span data-stu-id="06b10-156">hello following examples show how toospecify hello scope for different levels:</span></span>

   * <span data-ttu-id="06b10-157">Předplatné: /subscriptions/ {id předplatného}</span><span class="sxs-lookup"><span data-stu-id="06b10-157">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="06b10-158">Skupina prostředků: /subscriptions/ {id předplatného} / Skupinyprostředků/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="06b10-158">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>   
   * <span data-ttu-id="06b10-159">Prostředek: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="06b10-159">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="06b10-160">Nahraďte *{id role přiřazení}* s nový identifikátor GUID, který se stane identifikátor GUID hello hello nové přiřazení role.</span><span class="sxs-lookup"><span data-stu-id="06b10-160">Replace *{role-assignment-id}* with a new GUID, which becomes hello GUID identifier of hello new role assignment.</span></span>
3. <span data-ttu-id="06b10-161">Nahraďte *{api-version}* s 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="06b10-161">Replace *{api-version}* with 2015-07-01.</span></span>

<span data-ttu-id="06b10-162">Hello tělo žádosti zadejte hodnoty hello v hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="06b10-162">For hello request body, provide hello values in hello following format:</span></span>

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b"
  }
}

```

| <span data-ttu-id="06b10-163">Název elementu</span><span class="sxs-lookup"><span data-stu-id="06b10-163">Element Name</span></span> | <span data-ttu-id="06b10-164">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="06b10-164">Required</span></span> | <span data-ttu-id="06b10-165">Typ</span><span class="sxs-lookup"><span data-stu-id="06b10-165">Type</span></span> | <span data-ttu-id="06b10-166">Popis</span><span class="sxs-lookup"><span data-stu-id="06b10-166">Description</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="06b10-167">hodnoty vlastnosti roleDefinitionId</span><span class="sxs-lookup"><span data-stu-id="06b10-167">roleDefinitionId</span></span> |<span data-ttu-id="06b10-168">Ano</span><span class="sxs-lookup"><span data-stu-id="06b10-168">Yes</span></span> |<span data-ttu-id="06b10-169">Řetězec</span><span class="sxs-lookup"><span data-stu-id="06b10-169">String</span></span> |<span data-ttu-id="06b10-170">identifikátor Hello hello role.</span><span class="sxs-lookup"><span data-stu-id="06b10-170">hello identifier of hello role.</span></span> <span data-ttu-id="06b10-171">Formát Hello hello identifikátoru je:`{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id-guid}`</span><span class="sxs-lookup"><span data-stu-id="06b10-171">hello format of hello identifier is: `{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id-guid}`</span></span> |
| <span data-ttu-id="06b10-172">principalId</span><span class="sxs-lookup"><span data-stu-id="06b10-172">principalId</span></span> |<span data-ttu-id="06b10-173">Ano</span><span class="sxs-lookup"><span data-stu-id="06b10-173">Yes</span></span> |<span data-ttu-id="06b10-174">Řetězec</span><span class="sxs-lookup"><span data-stu-id="06b10-174">String</span></span> |<span data-ttu-id="06b10-175">objectId objektu hello Azure AD (uživatele, skupiny nebo instanční objekt) toowhich hello role je přiřazená.</span><span class="sxs-lookup"><span data-stu-id="06b10-175">objectId of hello Azure AD principal (user, group, or service principal) toowhich hello role is assigned.</span></span> |

### <a name="response"></a><span data-ttu-id="06b10-176">Odpověď</span><span class="sxs-lookup"><span data-stu-id="06b10-176">Response</span></span>
<span data-ttu-id="06b10-177">Stavový kód: 201</span><span class="sxs-lookup"><span data-stu-id="06b10-177">Status code: 201</span></span>

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

## <a name="delete-a-role-assignment"></a><span data-ttu-id="06b10-178">Umožňuje odstranit přiřazení Role</span><span class="sxs-lookup"><span data-stu-id="06b10-178">Delete a Role Assignment</span></span>
<span data-ttu-id="06b10-179">Odstranit přiřazení role v hello zadaný obor.</span><span class="sxs-lookup"><span data-stu-id="06b10-179">Delete a role assignment at hello specified scope.</span></span>

<span data-ttu-id="06b10-180">toodelete přiřazení role musí mít přístup toohello `Microsoft.Authorization/roleAssignments/delete` operaci.</span><span class="sxs-lookup"><span data-stu-id="06b10-180">toodelete a role assignment, you must have access toohello `Microsoft.Authorization/roleAssignments/delete` operation.</span></span> <span data-ttu-id="06b10-181">Hello předdefinovaných rolí, pouze *vlastníka* a *správce uživatelského přístupu* jsou udělena toothis operace přístupu.</span><span class="sxs-lookup"><span data-stu-id="06b10-181">Of hello built-in roles, only *Owner* and *User Access Administrator* are granted access toothis operation.</span></span> <span data-ttu-id="06b10-182">Další informace o přiřazení rolí a správu přístupu k prostředkům Azure najdete v tématu [řízení přístupu](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="06b10-182">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="06b10-183">Žádost</span><span class="sxs-lookup"><span data-stu-id="06b10-183">Request</span></span>
<span data-ttu-id="06b10-184">Použití hello **odstranit** metoda s hello následující identifikátor URI:</span><span class="sxs-lookup"><span data-stu-id="06b10-184">Use hello **DELETE** method with hello following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

<span data-ttu-id="06b10-185">V rámci hello identifikátor URI proveďte hello následující náhrady toocustomize vaší žádosti:</span><span class="sxs-lookup"><span data-stu-id="06b10-185">Within hello URI, make hello following substitutions toocustomize your request:</span></span>

1. <span data-ttu-id="06b10-186">Nahraďte *{oboru}* hello oboru, ve kterém chcete toocreate přiřazení rolí hello.</span><span class="sxs-lookup"><span data-stu-id="06b10-186">Replace *{scope}* with hello scope at which you wish toocreate hello role assignments.</span></span> <span data-ttu-id="06b10-187">Hello následující příklady ukazují, jak toospecify hello oboru pro různé úrovně:</span><span class="sxs-lookup"><span data-stu-id="06b10-187">hello following examples show how toospecify hello scope for different levels:</span></span>

   * <span data-ttu-id="06b10-188">Předplatné: /subscriptions/ {id předplatného}</span><span class="sxs-lookup"><span data-stu-id="06b10-188">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="06b10-189">Skupina prostředků: /subscriptions/ {id předplatného} / Skupinyprostředků/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="06b10-189">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="06b10-190">Prostředek: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="06b10-190">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="06b10-191">Nahraďte *{id role přiřazení}* s id přiřazení role hello identifikátor GUID.</span><span class="sxs-lookup"><span data-stu-id="06b10-191">Replace *{role-assignment-id}* with hello role assignment id GUID.</span></span>
3. <span data-ttu-id="06b10-192">Nahraďte *{api-version}* s 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="06b10-192">Replace *{api-version}* with 2015-07-01.</span></span>

### <a name="response"></a><span data-ttu-id="06b10-193">Odpověď</span><span class="sxs-lookup"><span data-stu-id="06b10-193">Response</span></span>
<span data-ttu-id="06b10-194">Stavový kód: 200</span><span class="sxs-lookup"><span data-stu-id="06b10-194">Status code: 200</span></span>

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

## <a name="list-all-roles"></a><span data-ttu-id="06b10-195">Zobrazí seznam všech rolí</span><span class="sxs-lookup"><span data-stu-id="06b10-195">List all Roles</span></span>
<span data-ttu-id="06b10-196">Zobrazí seznam všech rolí hello, které jsou k dispozici pro přiřazení v hello zadaný obor.</span><span class="sxs-lookup"><span data-stu-id="06b10-196">Lists all hello roles that are available for assignment at hello specified scope.</span></span>

<span data-ttu-id="06b10-197">toolist role, je nutné mít přístup příliš`Microsoft.Authorization/roleDefinitions/read` operace v oboru hello.</span><span class="sxs-lookup"><span data-stu-id="06b10-197">toolist roles, you must have access too`Microsoft.Authorization/roleDefinitions/read` operation at hello scope.</span></span> <span data-ttu-id="06b10-198">Všechny hello předdefinované role jsou udělena toothis operace přístupu.</span><span class="sxs-lookup"><span data-stu-id="06b10-198">All hello built-in roles are granted access toothis operation.</span></span> <span data-ttu-id="06b10-199">Další informace o přiřazení rolí a správu přístupu k prostředkům Azure najdete v tématu [řízení přístupu](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="06b10-199">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="06b10-200">Žádost</span><span class="sxs-lookup"><span data-stu-id="06b10-200">Request</span></span>
<span data-ttu-id="06b10-201">Použití hello **získat** metoda s hello následující identifikátor URI:</span><span class="sxs-lookup"><span data-stu-id="06b10-201">Use hello **GET** method with hello following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version={api-version}&$filter={filter}

<span data-ttu-id="06b10-202">V rámci hello identifikátor URI proveďte hello následující náhrady toocustomize vaší žádosti:</span><span class="sxs-lookup"><span data-stu-id="06b10-202">Within hello URI, make hello following substitutions toocustomize your request:</span></span>

1. <span data-ttu-id="06b10-203">Nahraďte *{oboru}* hello oboru, pro kterou chcete toolist hello role.</span><span class="sxs-lookup"><span data-stu-id="06b10-203">Replace *{scope}* with hello scope for which you wish toolist hello roles.</span></span> <span data-ttu-id="06b10-204">Hello následující příklady ukazují, jak toospecify hello oboru pro různé úrovně:</span><span class="sxs-lookup"><span data-stu-id="06b10-204">hello following examples show how toospecify hello scope for different levels:</span></span>

   * <span data-ttu-id="06b10-205">Předplatné: /subscriptions/ {id předplatného}</span><span class="sxs-lookup"><span data-stu-id="06b10-205">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="06b10-206">Skupina prostředků: /subscriptions/ {id předplatného} / Skupinyprostředků/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="06b10-206">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="06b10-207">/Subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1 prostředků</span><span class="sxs-lookup"><span data-stu-id="06b10-207">Resource /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="06b10-208">Nahraďte *{api-version}* s 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="06b10-208">Replace *{api-version}* with 2015-07-01.</span></span>
3. <span data-ttu-id="06b10-209">Nahraďte *{filtru}* s podmínkou hello chcete tooapply toofilter hello seznamu rolí:</span><span class="sxs-lookup"><span data-stu-id="06b10-209">Replace *{filter}* with hello condition that you wish tooapply toofilter hello list of roles:</span></span>

   * <span data-ttu-id="06b10-210">Seznam rolí, které jsou k dispozici pro přiřazení v hello zadaný obor a všechny její podřízené obory:`atScopeAndBelow()`</span><span class="sxs-lookup"><span data-stu-id="06b10-210">List roles available for assignment at hello specified scope and any of its child scopes: `atScopeAndBelow()`</span></span>
   * <span data-ttu-id="06b10-211">Vyhledejte roli pomocí přesný zobrazovaný název: `roleName%20eq%20'{role-display-name}'`.</span><span class="sxs-lookup"><span data-stu-id="06b10-211">Search for a role using exact display name: `roleName%20eq%20'{role-display-name}'`.</span></span> <span data-ttu-id="06b10-212">Použijte hello kódovaná adresou URL formu hello přesný zobrazovaný název role hello.</span><span class="sxs-lookup"><span data-stu-id="06b10-212">Use hello URL encoded form of hello exact display name of hello role.</span></span> <span data-ttu-id="06b10-213">Pro instanci,`$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |</span><span class="sxs-lookup"><span data-stu-id="06b10-213">For instance, `$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |</span></span>

### <a name="response"></a><span data-ttu-id="06b10-214">Odpověď</span><span class="sxs-lookup"><span data-stu-id="06b10-214">Response</span></span>
<span data-ttu-id="06b10-215">Stavový kód: 200</span><span class="sxs-lookup"><span data-stu-id="06b10-215">Status code: 200</span></span>

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

## <a name="get-information-about-a-role"></a><span data-ttu-id="06b10-216">Získat informace o roli</span><span class="sxs-lookup"><span data-stu-id="06b10-216">Get information about a Role</span></span>
<span data-ttu-id="06b10-217">Získá informace o jedné role určeného identifikátor definice role hello.</span><span class="sxs-lookup"><span data-stu-id="06b10-217">Gets information about a single role specified by hello role definition identifier.</span></span> <span data-ttu-id="06b10-218">tooget informace o jedné role pomocí názvu zobrazení, naleznete v [seznam všech rolí](role-based-access-control-manage-access-rest.md#list-all-roles).</span><span class="sxs-lookup"><span data-stu-id="06b10-218">tooget information about a single role using its display name, see [List all roles](role-based-access-control-manage-access-rest.md#list-all-roles).</span></span>

<span data-ttu-id="06b10-219">tooget informace o roli, musíte mít přístup příliš`Microsoft.Authorization/roleDefinitions/read` operaci.</span><span class="sxs-lookup"><span data-stu-id="06b10-219">tooget information about a role, you must have access too`Microsoft.Authorization/roleDefinitions/read` operation.</span></span> <span data-ttu-id="06b10-220">Všechny hello předdefinované role jsou udělena toothis operace přístupu.</span><span class="sxs-lookup"><span data-stu-id="06b10-220">All hello built-in roles are granted access toothis operation.</span></span> <span data-ttu-id="06b10-221">Další informace o přiřazení rolí a správu přístupu k prostředkům Azure najdete v tématu [řízení přístupu](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="06b10-221">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="06b10-222">Žádost</span><span class="sxs-lookup"><span data-stu-id="06b10-222">Request</span></span>
<span data-ttu-id="06b10-223">Použití hello **získat** metoda s hello následující identifikátor URI:</span><span class="sxs-lookup"><span data-stu-id="06b10-223">Use hello **GET** method with hello following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

<span data-ttu-id="06b10-224">V rámci hello identifikátor URI proveďte hello následující náhrady toocustomize vaší žádosti:</span><span class="sxs-lookup"><span data-stu-id="06b10-224">Within hello URI, make hello following substitutions toocustomize your request:</span></span>

1. <span data-ttu-id="06b10-225">Nahraďte *{oboru}* hello oboru, pro kterou chcete toolist hello přiřazení rolí.</span><span class="sxs-lookup"><span data-stu-id="06b10-225">Replace *{scope}* with hello scope for which you wish toolist hello role assignments.</span></span> <span data-ttu-id="06b10-226">Hello následující příklady ukazují, jak toospecify hello oboru pro různé úrovně:</span><span class="sxs-lookup"><span data-stu-id="06b10-226">hello following examples show how toospecify hello scope for different levels:</span></span>

   * <span data-ttu-id="06b10-227">Předplatné: /subscriptions/ {id předplatného}</span><span class="sxs-lookup"><span data-stu-id="06b10-227">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="06b10-228">Skupina prostředků: /subscriptions/ {id předplatného} / Skupinyprostředků/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="06b10-228">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="06b10-229">Prostředek: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="06b10-229">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="06b10-230">Nahraďte *{id role definice}* s identifikátorem GUID hello definice role hello.</span><span class="sxs-lookup"><span data-stu-id="06b10-230">Replace *{role-definition-id}* with hello GUID identifier of hello role definition.</span></span>
3. <span data-ttu-id="06b10-231">Nahraďte *{api-version}* s 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="06b10-231">Replace *{api-version}* with 2015-07-01.</span></span>

### <a name="response"></a><span data-ttu-id="06b10-232">Odpověď</span><span class="sxs-lookup"><span data-stu-id="06b10-232">Response</span></span>
<span data-ttu-id="06b10-233">Stavový kód: 200</span><span class="sxs-lookup"><span data-stu-id="06b10-233">Status code: 200</span></span>

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

## <a name="create-a-custom-role"></a><span data-ttu-id="06b10-234">Vytvořit vlastní roli</span><span class="sxs-lookup"><span data-stu-id="06b10-234">Create a Custom Role</span></span>
<span data-ttu-id="06b10-235">Vytvořte vlastní roli.</span><span class="sxs-lookup"><span data-stu-id="06b10-235">Create a custom role.</span></span>

<span data-ttu-id="06b10-236">toocreate vlastní roli, musíte mít přístup příliš`Microsoft.Authorization/roleDefinitions/write` operace na všechny hello `AssignableScopes`.</span><span class="sxs-lookup"><span data-stu-id="06b10-236">toocreate a custom role, you must have access too`Microsoft.Authorization/roleDefinitions/write` operation on all hello `AssignableScopes`.</span></span> <span data-ttu-id="06b10-237">Hello předdefinovaných rolí, pouze *vlastníka* a *správce uživatelského přístupu* jsou udělena toothis operace přístupu.</span><span class="sxs-lookup"><span data-stu-id="06b10-237">Of hello built-in roles, only *Owner* and *User Access Administrator* are granted access toothis operation.</span></span> <span data-ttu-id="06b10-238">Další informace o přiřazení rolí a správu přístupu k prostředkům Azure najdete v tématu [řízení přístupu](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="06b10-238">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="06b10-239">Žádost</span><span class="sxs-lookup"><span data-stu-id="06b10-239">Request</span></span>
<span data-ttu-id="06b10-240">Použití hello **PUT** metoda s hello následující identifikátor URI:</span><span class="sxs-lookup"><span data-stu-id="06b10-240">Use hello **PUT** method with hello following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

<span data-ttu-id="06b10-241">V rámci hello identifikátor URI proveďte hello následující náhrady toocustomize vaší žádosti:</span><span class="sxs-lookup"><span data-stu-id="06b10-241">Within hello URI, make hello following substitutions toocustomize your request:</span></span>

1. <span data-ttu-id="06b10-242">Nahraďte *{oboru}* s hello první *AssignableScope* hello vlastní role.</span><span class="sxs-lookup"><span data-stu-id="06b10-242">Replace *{scope}* with hello first *AssignableScope* of hello custom role.</span></span> <span data-ttu-id="06b10-243">Hello následující příklady ukazují, jak toospecify hello oboru pro různé úrovně.</span><span class="sxs-lookup"><span data-stu-id="06b10-243">hello following examples show how toospecify hello scope for different levels.</span></span>

   * <span data-ttu-id="06b10-244">Předplatné: /subscriptions/ {id předplatného}</span><span class="sxs-lookup"><span data-stu-id="06b10-244">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="06b10-245">Skupina prostředků: /subscriptions/ {id předplatného} / Skupinyprostředků/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="06b10-245">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="06b10-246">Prostředek: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="06b10-246">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="06b10-247">Nahraďte *{id role definice}* s nový identifikátor GUID, který se stane identifikátor GUID hello hello novou vlastní roli.</span><span class="sxs-lookup"><span data-stu-id="06b10-247">Replace *{role-definition-id}* with a new GUID, which becomes hello GUID identifier of hello new custom role.</span></span>
3. <span data-ttu-id="06b10-248">Nahraďte *{api-version}* s 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="06b10-248">Replace *{api-version}* with 2015-07-01.</span></span>

<span data-ttu-id="06b10-249">Hello tělo žádosti zadejte hodnoty hello v hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="06b10-249">For hello request body, provide hello values in hello following format:</span></span>

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

| <span data-ttu-id="06b10-250">Název elementu</span><span class="sxs-lookup"><span data-stu-id="06b10-250">Element Name</span></span> | <span data-ttu-id="06b10-251">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="06b10-251">Required</span></span> | <span data-ttu-id="06b10-252">Typ</span><span class="sxs-lookup"><span data-stu-id="06b10-252">Type</span></span> | <span data-ttu-id="06b10-253">Popis</span><span class="sxs-lookup"><span data-stu-id="06b10-253">Description</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="06b10-254">jméno</span><span class="sxs-lookup"><span data-stu-id="06b10-254">name</span></span> |<span data-ttu-id="06b10-255">Ano</span><span class="sxs-lookup"><span data-stu-id="06b10-255">Yes</span></span> |<span data-ttu-id="06b10-256">Řetězec</span><span class="sxs-lookup"><span data-stu-id="06b10-256">String</span></span> |<span data-ttu-id="06b10-257">Identifikátor GUID hello vlastní role.</span><span class="sxs-lookup"><span data-stu-id="06b10-257">GUID identifier of hello custom role.</span></span> |
| <span data-ttu-id="06b10-258">properties.roleName</span><span class="sxs-lookup"><span data-stu-id="06b10-258">properties.roleName</span></span> |<span data-ttu-id="06b10-259">Ano</span><span class="sxs-lookup"><span data-stu-id="06b10-259">Yes</span></span> |<span data-ttu-id="06b10-260">Řetězec</span><span class="sxs-lookup"><span data-stu-id="06b10-260">String</span></span> |<span data-ttu-id="06b10-261">Zobrazovaný název hello vlastní role.</span><span class="sxs-lookup"><span data-stu-id="06b10-261">Display name of hello custom role.</span></span> <span data-ttu-id="06b10-262">Maximální velikost 128 znaků.</span><span class="sxs-lookup"><span data-stu-id="06b10-262">Maximum size 128 characters.</span></span> |
| <span data-ttu-id="06b10-263">Properties.Description</span><span class="sxs-lookup"><span data-stu-id="06b10-263">properties.description</span></span> |<span data-ttu-id="06b10-264">Ne</span><span class="sxs-lookup"><span data-stu-id="06b10-264">No</span></span> |<span data-ttu-id="06b10-265">Řetězec</span><span class="sxs-lookup"><span data-stu-id="06b10-265">String</span></span> |<span data-ttu-id="06b10-266">Popis vlastní role hello.</span><span class="sxs-lookup"><span data-stu-id="06b10-266">Description of hello custom role.</span></span> <span data-ttu-id="06b10-267">Maximální velikost 1024 znaků.</span><span class="sxs-lookup"><span data-stu-id="06b10-267">Maximum size 1024 characters.</span></span> |
| <span data-ttu-id="06b10-268">Properties.Type</span><span class="sxs-lookup"><span data-stu-id="06b10-268">properties.type</span></span> |<span data-ttu-id="06b10-269">Ano</span><span class="sxs-lookup"><span data-stu-id="06b10-269">Yes</span></span> |<span data-ttu-id="06b10-270">Řetězec</span><span class="sxs-lookup"><span data-stu-id="06b10-270">String</span></span> |<span data-ttu-id="06b10-271">Nastavení příliš "CustomRole."</span><span class="sxs-lookup"><span data-stu-id="06b10-271">Set too"CustomRole."</span></span> |
| <span data-ttu-id="06b10-272">Properties.Permissions.Actions</span><span class="sxs-lookup"><span data-stu-id="06b10-272">properties.permissions.actions</span></span> |<span data-ttu-id="06b10-273">Ano</span><span class="sxs-lookup"><span data-stu-id="06b10-273">Yes</span></span> |<span data-ttu-id="06b10-274">řetězec]</span><span class="sxs-lookup"><span data-stu-id="06b10-274">String[]</span></span> |<span data-ttu-id="06b10-275">Pole Akce řetězce zadání hello operations udělují hello vlastní role.</span><span class="sxs-lookup"><span data-stu-id="06b10-275">An array of action strings specifying hello operations granted by hello custom role.</span></span> |
| <span data-ttu-id="06b10-276">properties.permissions.notActions</span><span class="sxs-lookup"><span data-stu-id="06b10-276">properties.permissions.notActions</span></span> |<span data-ttu-id="06b10-277">Ne</span><span class="sxs-lookup"><span data-stu-id="06b10-277">No</span></span> |<span data-ttu-id="06b10-278">řetězec]</span><span class="sxs-lookup"><span data-stu-id="06b10-278">String[]</span></span> |<span data-ttu-id="06b10-279">Pole řetězců akce zadání hello operations tooexclude hello operacích udělují hello vlastní role.</span><span class="sxs-lookup"><span data-stu-id="06b10-279">An array of action strings specifying hello operations tooexclude from hello operations granted by hello custom role.</span></span> |
| <span data-ttu-id="06b10-280">properties.assignableScopes</span><span class="sxs-lookup"><span data-stu-id="06b10-280">properties.assignableScopes</span></span> |<span data-ttu-id="06b10-281">Ano</span><span class="sxs-lookup"><span data-stu-id="06b10-281">Yes</span></span> |<span data-ttu-id="06b10-282">řetězec]</span><span class="sxs-lookup"><span data-stu-id="06b10-282">String[]</span></span> |<span data-ttu-id="06b10-283">Pole obory, ve které hello je možné vlastní role.</span><span class="sxs-lookup"><span data-stu-id="06b10-283">An array of scopes in which hello custom role can be used.</span></span> |

### <a name="response"></a><span data-ttu-id="06b10-284">Odpověď</span><span class="sxs-lookup"><span data-stu-id="06b10-284">Response</span></span>
<span data-ttu-id="06b10-285">Stavový kód: 201</span><span class="sxs-lookup"><span data-stu-id="06b10-285">Status code: 201</span></span>

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

## <a name="update-a-custom-role"></a><span data-ttu-id="06b10-286">Aktualizovat vlastní Role</span><span class="sxs-lookup"><span data-stu-id="06b10-286">Update a Custom Role</span></span>
<span data-ttu-id="06b10-287">Upravte vlastní roli.</span><span class="sxs-lookup"><span data-stu-id="06b10-287">Modify a custom role.</span></span>

<span data-ttu-id="06b10-288">toomodify vlastní roli, musíte mít přístup příliš`Microsoft.Authorization/roleDefinitions/write` operace na všechny hello `AssignableScopes`.</span><span class="sxs-lookup"><span data-stu-id="06b10-288">toomodify a custom role, you must have access too`Microsoft.Authorization/roleDefinitions/write` operation on all hello `AssignableScopes`.</span></span> <span data-ttu-id="06b10-289">Hello předdefinovaných rolí, pouze *vlastníka* a *správce uživatelského přístupu* jsou udělena toothis operace přístupu.</span><span class="sxs-lookup"><span data-stu-id="06b10-289">Of hello built-in roles, only *Owner* and *User Access Administrator* are granted access toothis operation.</span></span> <span data-ttu-id="06b10-290">Další informace o přiřazení rolí a správu přístupu k prostředkům Azure najdete v tématu [řízení přístupu](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="06b10-290">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="06b10-291">Žádost</span><span class="sxs-lookup"><span data-stu-id="06b10-291">Request</span></span>
<span data-ttu-id="06b10-292">Použití hello **PUT** metoda s hello následující identifikátor URI:</span><span class="sxs-lookup"><span data-stu-id="06b10-292">Use hello **PUT** method with hello following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

<span data-ttu-id="06b10-293">V rámci hello identifikátor URI proveďte hello následující náhrady toocustomize vaší žádosti:</span><span class="sxs-lookup"><span data-stu-id="06b10-293">Within hello URI, make hello following substitutions toocustomize your request:</span></span>

1. <span data-ttu-id="06b10-294">Nahraďte *{oboru}* s hello první *AssignableScope* hello vlastní role.</span><span class="sxs-lookup"><span data-stu-id="06b10-294">Replace *{scope}* with hello first *AssignableScope* of hello custom role.</span></span> <span data-ttu-id="06b10-295">Hello následující příklady ukazují, jak toospecify hello oboru pro různé úrovně:</span><span class="sxs-lookup"><span data-stu-id="06b10-295">hello following examples show how toospecify hello scope for different levels:</span></span>

   * <span data-ttu-id="06b10-296">Předplatné: /subscriptions/ {id předplatného}</span><span class="sxs-lookup"><span data-stu-id="06b10-296">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="06b10-297">Skupina prostředků: /subscriptions/ {id předplatného} / Skupinyprostředků/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="06b10-297">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="06b10-298">Prostředek: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="06b10-298">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="06b10-299">Nahraďte *{id role definice}* s identifikátorem GUID hello hello vlastní role.</span><span class="sxs-lookup"><span data-stu-id="06b10-299">Replace *{role-definition-id}* with hello GUID identifier of hello custom role.</span></span>
3. <span data-ttu-id="06b10-300">Nahraďte *{api-version}* s 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="06b10-300">Replace *{api-version}* with 2015-07-01.</span></span>

<span data-ttu-id="06b10-301">Hello tělo žádosti zadejte hodnoty hello v hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="06b10-301">For hello request body, provide hello values in hello following format:</span></span>

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

| <span data-ttu-id="06b10-302">Název elementu</span><span class="sxs-lookup"><span data-stu-id="06b10-302">Element Name</span></span> | <span data-ttu-id="06b10-303">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="06b10-303">Required</span></span> | <span data-ttu-id="06b10-304">Typ</span><span class="sxs-lookup"><span data-stu-id="06b10-304">Type</span></span> | <span data-ttu-id="06b10-305">Popis</span><span class="sxs-lookup"><span data-stu-id="06b10-305">Description</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="06b10-306">jméno</span><span class="sxs-lookup"><span data-stu-id="06b10-306">name</span></span> |<span data-ttu-id="06b10-307">Ano</span><span class="sxs-lookup"><span data-stu-id="06b10-307">Yes</span></span> |<span data-ttu-id="06b10-308">Řetězec</span><span class="sxs-lookup"><span data-stu-id="06b10-308">String</span></span> |<span data-ttu-id="06b10-309">Identifikátor GUID hello vlastní role.</span><span class="sxs-lookup"><span data-stu-id="06b10-309">GUID identifier of hello custom role.</span></span> |
| <span data-ttu-id="06b10-310">properties.roleName</span><span class="sxs-lookup"><span data-stu-id="06b10-310">properties.roleName</span></span> |<span data-ttu-id="06b10-311">Ano</span><span class="sxs-lookup"><span data-stu-id="06b10-311">Yes</span></span> |<span data-ttu-id="06b10-312">Řetězec</span><span class="sxs-lookup"><span data-stu-id="06b10-312">String</span></span> |<span data-ttu-id="06b10-313">Zobrazovaný název hello aktualizovat vlastní role.</span><span class="sxs-lookup"><span data-stu-id="06b10-313">Display name of hello updated custom role.</span></span> |
| <span data-ttu-id="06b10-314">Properties.Description</span><span class="sxs-lookup"><span data-stu-id="06b10-314">properties.description</span></span> |<span data-ttu-id="06b10-315">Ne</span><span class="sxs-lookup"><span data-stu-id="06b10-315">No</span></span> |<span data-ttu-id="06b10-316">Řetězec</span><span class="sxs-lookup"><span data-stu-id="06b10-316">String</span></span> |<span data-ttu-id="06b10-317">Popis hello aktualizovat vlastní role.</span><span class="sxs-lookup"><span data-stu-id="06b10-317">Description of hello updated custom role.</span></span> |
| <span data-ttu-id="06b10-318">Properties.Type</span><span class="sxs-lookup"><span data-stu-id="06b10-318">properties.type</span></span> |<span data-ttu-id="06b10-319">Ano</span><span class="sxs-lookup"><span data-stu-id="06b10-319">Yes</span></span> |<span data-ttu-id="06b10-320">Řetězec</span><span class="sxs-lookup"><span data-stu-id="06b10-320">String</span></span> |<span data-ttu-id="06b10-321">Nastavení příliš "CustomRole."</span><span class="sxs-lookup"><span data-stu-id="06b10-321">Set too"CustomRole."</span></span> |
| <span data-ttu-id="06b10-322">Properties.Permissions.Actions</span><span class="sxs-lookup"><span data-stu-id="06b10-322">properties.permissions.actions</span></span> |<span data-ttu-id="06b10-323">Ano</span><span class="sxs-lookup"><span data-stu-id="06b10-323">Yes</span></span> |<span data-ttu-id="06b10-324">řetězec]</span><span class="sxs-lookup"><span data-stu-id="06b10-324">String[]</span></span> |<span data-ttu-id="06b10-325">Pole řetězců akce zadání hello operations toowhich hello aktualizovat vlastní role uděluje přístup.</span><span class="sxs-lookup"><span data-stu-id="06b10-325">An array of action strings specifying hello operations toowhich hello updated custom role grants access.</span></span> |
| <span data-ttu-id="06b10-326">properties.permissions.notActions</span><span class="sxs-lookup"><span data-stu-id="06b10-326">properties.permissions.notActions</span></span> |<span data-ttu-id="06b10-327">Ne</span><span class="sxs-lookup"><span data-stu-id="06b10-327">No</span></span> |<span data-ttu-id="06b10-328">řetězec]</span><span class="sxs-lookup"><span data-stu-id="06b10-328">String[]</span></span> |<span data-ttu-id="06b10-329">Pole Akce řetězce zadání tooexclude operations hello hello operacích, které hello aktualizovat vlastní role uděluje.</span><span class="sxs-lookup"><span data-stu-id="06b10-329">An array of action strings specifying hello operations tooexclude from hello operations which hello updated custom role grants.</span></span> |
| <span data-ttu-id="06b10-330">properties.assignableScopes</span><span class="sxs-lookup"><span data-stu-id="06b10-330">properties.assignableScopes</span></span> |<span data-ttu-id="06b10-331">Ano</span><span class="sxs-lookup"><span data-stu-id="06b10-331">Yes</span></span> |<span data-ttu-id="06b10-332">řetězec]</span><span class="sxs-lookup"><span data-stu-id="06b10-332">String[]</span></span> |<span data-ttu-id="06b10-333">Pole obory, ve které hello je možné aktualizované vlastní role.</span><span class="sxs-lookup"><span data-stu-id="06b10-333">An array of scopes in which hello updated custom role can be used.</span></span> |

### <a name="response"></a><span data-ttu-id="06b10-334">Odpověď</span><span class="sxs-lookup"><span data-stu-id="06b10-334">Response</span></span>
<span data-ttu-id="06b10-335">Stavový kód: 201</span><span class="sxs-lookup"><span data-stu-id="06b10-335">Status code: 201</span></span>

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

## <a name="delete-a-custom-role"></a><span data-ttu-id="06b10-336">Odstranit vlastní roli</span><span class="sxs-lookup"><span data-stu-id="06b10-336">Delete a Custom Role</span></span>
<span data-ttu-id="06b10-337">Odstraňte vlastní roli.</span><span class="sxs-lookup"><span data-stu-id="06b10-337">Delete a custom role.</span></span>

<span data-ttu-id="06b10-338">toodelete vlastní roli, musíte mít přístup příliš`Microsoft.Authorization/roleDefinitions/delete` operace na všechny hello `AssignableScopes`.</span><span class="sxs-lookup"><span data-stu-id="06b10-338">toodelete a custom role, you must have access too`Microsoft.Authorization/roleDefinitions/delete` operation on all hello `AssignableScopes`.</span></span> <span data-ttu-id="06b10-339">Hello předdefinovaných rolí, pouze *vlastníka* a *správce uživatelského přístupu* jsou udělena toothis operace přístupu.</span><span class="sxs-lookup"><span data-stu-id="06b10-339">Of hello built-in roles, only *Owner* and *User Access Administrator* are granted access toothis operation.</span></span> <span data-ttu-id="06b10-340">Další informace o přiřazení rolí a správu přístupu k prostředkům Azure najdete v tématu [řízení přístupu](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="06b10-340">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="06b10-341">Žádost</span><span class="sxs-lookup"><span data-stu-id="06b10-341">Request</span></span>
<span data-ttu-id="06b10-342">Použití hello **odstranit** metoda s hello následující identifikátor URI:</span><span class="sxs-lookup"><span data-stu-id="06b10-342">Use hello **DELETE** method with hello following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

<span data-ttu-id="06b10-343">V rámci hello identifikátor URI proveďte hello následující náhrady toocustomize vaší žádosti:</span><span class="sxs-lookup"><span data-stu-id="06b10-343">Within hello URI, make hello following substitutions toocustomize your request:</span></span>

1. <span data-ttu-id="06b10-344">Nahraďte *{oboru}* hello oboru, ve kterém chcete definice role toodelete hello.</span><span class="sxs-lookup"><span data-stu-id="06b10-344">Replace *{scope}* with hello scope at which you wish toodelete hello role definition.</span></span> <span data-ttu-id="06b10-345">Hello následující příklady ukazují, jak toospecify hello oboru pro různé úrovně:</span><span class="sxs-lookup"><span data-stu-id="06b10-345">hello following examples show how toospecify hello scope for different levels:</span></span>

   * <span data-ttu-id="06b10-346">Předplatné: /subscriptions/ {id předplatného}</span><span class="sxs-lookup"><span data-stu-id="06b10-346">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="06b10-347">Skupina prostředků: /subscriptions/ {id předplatného} / Skupinyprostředků/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="06b10-347">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="06b10-348">Prostředek: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="06b10-348">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="06b10-349">Nahraďte *{id role definice}* s id definice role GUID hello hello vlastní role.</span><span class="sxs-lookup"><span data-stu-id="06b10-349">Replace *{role-definition-id}* with hello GUID role definition id of hello custom role.</span></span>
3. <span data-ttu-id="06b10-350">Nahraďte *{api-version}* s 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="06b10-350">Replace *{api-version}* with 2015-07-01.</span></span>

### <a name="response"></a><span data-ttu-id="06b10-351">Odpověď</span><span class="sxs-lookup"><span data-stu-id="06b10-351">Response</span></span>
<span data-ttu-id="06b10-352">Stavový kód: 200</span><span class="sxs-lookup"><span data-stu-id="06b10-352">Status code: 200</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="06b10-353">Další kroky</span><span class="sxs-lookup"><span data-stu-id="06b10-353">Next steps</span></span>

[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
