---
title: "Řízení přístupu na základě role se zbytkem – Azure AD | Microsoft Docs"
description: "Správa řízení přístupu na základě rolí pomocí rozhraní REST API"
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
ms.openlocfilehash: a5c19fd87ce1ae3e199bf1dfc8cf82f5653baac2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="manage-role-based-access-control-with-the-rest-api"></a><span data-ttu-id="e738a-103">Správa řízení přístupu na základě rolí pomocí rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="e738a-103">Manage Role-Based Access Control with the REST API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e738a-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e738a-104">PowerShell</span></span>](role-based-access-control-manage-access-powershell.md)
> * [<span data-ttu-id="e738a-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e738a-105">Azure CLI</span></span>](role-based-access-control-manage-access-azure-cli.md)
> * [<span data-ttu-id="e738a-106">REST API</span><span class="sxs-lookup"><span data-stu-id="e738a-106">REST API</span></span>](role-based-access-control-manage-access-rest.md)

<span data-ttu-id="e738a-107">Na základě rolí řízení přístupu (RBAC) na portálu Azure a rozhraní API služby Azure Resource Manager můžete spravovat přístup k vaše předplatné a prostředky na velice přesné úrovni.</span><span class="sxs-lookup"><span data-stu-id="e738a-107">Role-Based Access Control (RBAC) in the Azure portal and Azure Resource Manager API helps you manage access to your subscription and resources at a fine-grained level.</span></span> <span data-ttu-id="e738a-108">Pomocí této funkce můžete udělit přístup pro uživatele, skupiny nebo objekty služby Active Directory přiřazením některé role je v určitém rozsahu.</span><span class="sxs-lookup"><span data-stu-id="e738a-108">With this feature, you can grant access for Active Directory users, groups, or service principals by assigning some roles to them at a particular scope.</span></span>

## <a name="list-all-role-assignments"></a><span data-ttu-id="e738a-109">Zobrazí seznam všech přiřazení rolí</span><span class="sxs-lookup"><span data-stu-id="e738a-109">List all role assignments</span></span>
<span data-ttu-id="e738a-110">Zobrazí seznam všech přiřazení role v zadaném oboru a subscopes.</span><span class="sxs-lookup"><span data-stu-id="e738a-110">Lists all the role assignments at the specified scope and subscopes.</span></span>

<span data-ttu-id="e738a-111">K přiřazení rolí seznamu je nutné mít přístup k `Microsoft.Authorization/roleAssignments/read` operace v oboru.</span><span class="sxs-lookup"><span data-stu-id="e738a-111">To list role assignments, you must have access to `Microsoft.Authorization/roleAssignments/read` operation at the scope.</span></span> <span data-ttu-id="e738a-112">Mezi integrované role mají přístup k této operace.</span><span class="sxs-lookup"><span data-stu-id="e738a-112">All the built-in roles are granted access to this operation.</span></span> <span data-ttu-id="e738a-113">Další informace o přiřazení rolí a správu přístupu k prostředkům Azure najdete v tématu [řízení přístupu](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="e738a-113">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="e738a-114">Žádost</span><span class="sxs-lookup"><span data-stu-id="e738a-114">Request</span></span>
<span data-ttu-id="e738a-115">Použití **získat** metoda s následující identifikátor URI:</span><span class="sxs-lookup"><span data-stu-id="e738a-115">Use the **GET** method with the following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version={api-version}&$filter={filter}

<span data-ttu-id="e738a-116">V rámci identifikátoru URI proveďte následující náhradu k přizpůsobení vaší žádosti:</span><span class="sxs-lookup"><span data-stu-id="e738a-116">Within the URI, make the following substitutions to customize your request:</span></span>

1. <span data-ttu-id="e738a-117">Nahraďte *{oboru}* s oborem, pro který chcete zobrazit seznam přiřazení rolí.</span><span class="sxs-lookup"><span data-stu-id="e738a-117">Replace *{scope}* with the scope for which you wish to list the role assignments.</span></span> <span data-ttu-id="e738a-118">Následující příklady ukazují, jak lze určit obor pro různé úrovně:</span><span class="sxs-lookup"><span data-stu-id="e738a-118">The following examples show how to specify the scope for different levels:</span></span>

   * <span data-ttu-id="e738a-119">Předplatné: /subscriptions/ {id předplatného}</span><span class="sxs-lookup"><span data-stu-id="e738a-119">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="e738a-120">Skupina prostředků: /subscriptions/ {id předplatného} / Skupinyprostředků/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="e738a-120">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="e738a-121">Prostředek: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="e738a-121">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="e738a-122">Nahraďte *{api-version}* s 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="e738a-122">Replace *{api-version}* with 2015-07-01.</span></span>
3. <span data-ttu-id="e738a-123">Nahraďte *{filtru}* s podmínku, kterou chcete použít pro filtrování seznamu přiřazení role:</span><span class="sxs-lookup"><span data-stu-id="e738a-123">Replace *{filter}* with the condition that you wish to apply to filter the role assignment list:</span></span>

   * <span data-ttu-id="e738a-124">Seznam přiřazení rolí pro pouze zadaný obor, není včetně přiřazení rolí v subscopes:`atScope()`</span><span class="sxs-lookup"><span data-stu-id="e738a-124">List role assignments for only the specified scope, not including the role assignments at subscopes: `atScope()`</span></span>    
   * <span data-ttu-id="e738a-125">Seznam přiřazení rolí pro konkrétního uživatele, skupinu nebo aplikaci:`principalId%20eq%20'{objectId of user, group, or service principal}'`</span><span class="sxs-lookup"><span data-stu-id="e738a-125">List role assignments for a specific user, group, or application: `principalId%20eq%20'{objectId of user, group, or service principal}'`</span></span>  
   * <span data-ttu-id="e738a-126">Seznam přiřazení rolí pro konkrétního uživatele, včetně těch, které jsou zděděno od skupiny |`assignedTo('{objectId of user}')`</span><span class="sxs-lookup"><span data-stu-id="e738a-126">List role assignments for a specific user, including ones inherited from groups | `assignedTo('{objectId of user}')`</span></span>

### <a name="response"></a><span data-ttu-id="e738a-127">Odpověď</span><span class="sxs-lookup"><span data-stu-id="e738a-127">Response</span></span>
<span data-ttu-id="e738a-128">Stavový kód: 200</span><span class="sxs-lookup"><span data-stu-id="e738a-128">Status code: 200</span></span>

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

## <a name="get-information-about-a-role-assignment"></a><span data-ttu-id="e738a-129">Získat informace o přiřazení role</span><span class="sxs-lookup"><span data-stu-id="e738a-129">Get information about a role assignment</span></span>
<span data-ttu-id="e738a-130">Získá informace o přiřazení jedné role určeného identifikátor přiřazení role.</span><span class="sxs-lookup"><span data-stu-id="e738a-130">Gets information about a single role assignment specified by the role assignment identifier.</span></span>

<span data-ttu-id="e738a-131">Chcete-li získat informace o přiřazení role, musíte mít přístup k `Microsoft.Authorization/roleAssignments/read` operaci.</span><span class="sxs-lookup"><span data-stu-id="e738a-131">To get information about a role assignment, you must have access to `Microsoft.Authorization/roleAssignments/read` operation.</span></span> <span data-ttu-id="e738a-132">Mezi integrované role mají přístup k této operace.</span><span class="sxs-lookup"><span data-stu-id="e738a-132">All the built-in roles are granted access to this operation.</span></span> <span data-ttu-id="e738a-133">Další informace o přiřazení rolí a správu přístupu k prostředkům Azure najdete v tématu [řízení přístupu](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="e738a-133">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="e738a-134">Žádost</span><span class="sxs-lookup"><span data-stu-id="e738a-134">Request</span></span>
<span data-ttu-id="e738a-135">Použití **získat** metoda s následující identifikátor URI:</span><span class="sxs-lookup"><span data-stu-id="e738a-135">Use the **GET** method with the following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

<span data-ttu-id="e738a-136">V rámci identifikátoru URI proveďte následující náhradu k přizpůsobení vaší žádosti:</span><span class="sxs-lookup"><span data-stu-id="e738a-136">Within the URI, make the following substitutions to customize your request:</span></span>

1. <span data-ttu-id="e738a-137">Nahraďte *{oboru}* s oborem, pro který chcete zobrazit seznam přiřazení rolí.</span><span class="sxs-lookup"><span data-stu-id="e738a-137">Replace *{scope}* with the scope for which you wish to list the role assignments.</span></span> <span data-ttu-id="e738a-138">Následující příklady ukazují, jak lze určit obor pro různé úrovně:</span><span class="sxs-lookup"><span data-stu-id="e738a-138">The following examples show how to specify the scope for different levels:</span></span>

   * <span data-ttu-id="e738a-139">Předplatné: /subscriptions/ {id předplatného}</span><span class="sxs-lookup"><span data-stu-id="e738a-139">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="e738a-140">Skupina prostředků: /subscriptions/ {id předplatného} / Skupinyprostředků/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="e738a-140">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="e738a-141">Prostředek: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="e738a-141">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="e738a-142">Nahraďte *{id role přiřazení}* s identifikátorem GUID přiřazení role.</span><span class="sxs-lookup"><span data-stu-id="e738a-142">Replace *{role-assignment-id}* with the GUID identifier of the role assignment.</span></span>
3. <span data-ttu-id="e738a-143">Nahraďte *{api-version}* s 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="e738a-143">Replace *{api-version}* with 2015-07-01.</span></span>

### <a name="response"></a><span data-ttu-id="e738a-144">Odpověď</span><span class="sxs-lookup"><span data-stu-id="e738a-144">Response</span></span>
<span data-ttu-id="e738a-145">Stavový kód: 200</span><span class="sxs-lookup"><span data-stu-id="e738a-145">Status code: 200</span></span>

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

## <a name="create-a-role-assignment"></a><span data-ttu-id="e738a-146">Umožňuje vytvořit přiřazení Role</span><span class="sxs-lookup"><span data-stu-id="e738a-146">Create a Role Assignment</span></span>
<span data-ttu-id="e738a-147">Vytvořte přiřazení role v zadaném oboru pro zadaný objekt zabezpečení udělení zadané roli.</span><span class="sxs-lookup"><span data-stu-id="e738a-147">Create a role assignment at the specified scope for the specified principal granting the specified role.</span></span>

<span data-ttu-id="e738a-148">Pokud chcete vytvořit přiřazení role, musíte mít přístup k `Microsoft.Authorization/roleAssignments/write` operaci.</span><span class="sxs-lookup"><span data-stu-id="e738a-148">To create a role assignment, you must have access to `Microsoft.Authorization/roleAssignments/write` operation.</span></span> <span data-ttu-id="e738a-149">Z předdefinovaných rolí pouze *vlastníka* a *správce přístupu uživatelů* mají udělen přístup k této operaci.</span><span class="sxs-lookup"><span data-stu-id="e738a-149">Of the built-in roles, only *Owner* and *User Access Administrator* are granted access to this operation.</span></span> <span data-ttu-id="e738a-150">Další informace o přiřazení rolí a správu přístupu k prostředkům Azure najdete v tématu [řízení přístupu](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="e738a-150">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="e738a-151">Žádost</span><span class="sxs-lookup"><span data-stu-id="e738a-151">Request</span></span>
<span data-ttu-id="e738a-152">Použití **PUT** metoda s následující identifikátor URI:</span><span class="sxs-lookup"><span data-stu-id="e738a-152">Use the **PUT** method with the following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

<span data-ttu-id="e738a-153">V rámci identifikátoru URI proveďte následující náhradu k přizpůsobení vaší žádosti:</span><span class="sxs-lookup"><span data-stu-id="e738a-153">Within the URI, make the following substitutions to customize your request:</span></span>

1. <span data-ttu-id="e738a-154">Nahraďte *{oboru}* k oboru, ve kterém chcete vytvořit přiřazení role.</span><span class="sxs-lookup"><span data-stu-id="e738a-154">Replace *{scope}* with the scope at which you wish to create the role assignments.</span></span> <span data-ttu-id="e738a-155">Když vytvoříte přiřazení role v nadřazeném oboru, všechny podřízené obory dědí stejné přiřazení role.</span><span class="sxs-lookup"><span data-stu-id="e738a-155">When you create a role assignment at a parent scope, all child scopes inherit the same role assignment.</span></span> <span data-ttu-id="e738a-156">Následující příklady ukazují, jak lze určit obor pro různé úrovně:</span><span class="sxs-lookup"><span data-stu-id="e738a-156">The following examples show how to specify the scope for different levels:</span></span>

   * <span data-ttu-id="e738a-157">Předplatné: /subscriptions/ {id předplatného}</span><span class="sxs-lookup"><span data-stu-id="e738a-157">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="e738a-158">Skupina prostředků: /subscriptions/ {id předplatného} / Skupinyprostředků/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="e738a-158">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>   
   * <span data-ttu-id="e738a-159">Prostředek: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="e738a-159">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="e738a-160">Nahraďte *{id role přiřazení}* s nový identifikátor GUID, který se stane identifikátor GUID nové přiřazení role.</span><span class="sxs-lookup"><span data-stu-id="e738a-160">Replace *{role-assignment-id}* with a new GUID, which becomes the GUID identifier of the new role assignment.</span></span>
3. <span data-ttu-id="e738a-161">Nahraďte *{api-version}* s 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="e738a-161">Replace *{api-version}* with 2015-07-01.</span></span>

<span data-ttu-id="e738a-162">Tělo žádosti zadejte hodnoty v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="e738a-162">For the request body, provide the values in the following format:</span></span>

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b"
  }
}

```

| <span data-ttu-id="e738a-163">Název elementu</span><span class="sxs-lookup"><span data-stu-id="e738a-163">Element Name</span></span> | <span data-ttu-id="e738a-164">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="e738a-164">Required</span></span> | <span data-ttu-id="e738a-165">Typ</span><span class="sxs-lookup"><span data-stu-id="e738a-165">Type</span></span> | <span data-ttu-id="e738a-166">Popis</span><span class="sxs-lookup"><span data-stu-id="e738a-166">Description</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="e738a-167">hodnoty vlastnosti roleDefinitionId</span><span class="sxs-lookup"><span data-stu-id="e738a-167">roleDefinitionId</span></span> |<span data-ttu-id="e738a-168">Ano</span><span class="sxs-lookup"><span data-stu-id="e738a-168">Yes</span></span> |<span data-ttu-id="e738a-169">Řetězec</span><span class="sxs-lookup"><span data-stu-id="e738a-169">String</span></span> |<span data-ttu-id="e738a-170">Identifikátor role.</span><span class="sxs-lookup"><span data-stu-id="e738a-170">The identifier of the role.</span></span> <span data-ttu-id="e738a-171">Formát identifikátoru je:`{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id-guid}`</span><span class="sxs-lookup"><span data-stu-id="e738a-171">The format of the identifier is: `{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id-guid}`</span></span> |
| <span data-ttu-id="e738a-172">principalId</span><span class="sxs-lookup"><span data-stu-id="e738a-172">principalId</span></span> |<span data-ttu-id="e738a-173">Ano</span><span class="sxs-lookup"><span data-stu-id="e738a-173">Yes</span></span> |<span data-ttu-id="e738a-174">Řetězec</span><span class="sxs-lookup"><span data-stu-id="e738a-174">String</span></span> |<span data-ttu-id="e738a-175">objectId přiřazenou roli hlavního Azure AD (uživatele, skupinu nebo objekt služby).</span><span class="sxs-lookup"><span data-stu-id="e738a-175">objectId of the Azure AD principal (user, group, or service principal) to which the role is assigned.</span></span> |

### <a name="response"></a><span data-ttu-id="e738a-176">Odpověď</span><span class="sxs-lookup"><span data-stu-id="e738a-176">Response</span></span>
<span data-ttu-id="e738a-177">Stavový kód: 201</span><span class="sxs-lookup"><span data-stu-id="e738a-177">Status code: 201</span></span>

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

## <a name="delete-a-role-assignment"></a><span data-ttu-id="e738a-178">Umožňuje odstranit přiřazení Role</span><span class="sxs-lookup"><span data-stu-id="e738a-178">Delete a Role Assignment</span></span>
<span data-ttu-id="e738a-179">Umožňuje odstranit přiřazení role v zadaném oboru.</span><span class="sxs-lookup"><span data-stu-id="e738a-179">Delete a role assignment at the specified scope.</span></span>

<span data-ttu-id="e738a-180">Pokud chcete odstranit přiřazení role, musíte mít přístup k `Microsoft.Authorization/roleAssignments/delete` operaci.</span><span class="sxs-lookup"><span data-stu-id="e738a-180">To delete a role assignment, you must have access to the `Microsoft.Authorization/roleAssignments/delete` operation.</span></span> <span data-ttu-id="e738a-181">Z předdefinovaných rolí pouze *vlastníka* a *správce přístupu uživatelů* mají udělen přístup k této operaci.</span><span class="sxs-lookup"><span data-stu-id="e738a-181">Of the built-in roles, only *Owner* and *User Access Administrator* are granted access to this operation.</span></span> <span data-ttu-id="e738a-182">Další informace o přiřazení rolí a správu přístupu k prostředkům Azure najdete v tématu [řízení přístupu](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="e738a-182">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="e738a-183">Žádost</span><span class="sxs-lookup"><span data-stu-id="e738a-183">Request</span></span>
<span data-ttu-id="e738a-184">Použití **odstranit** metoda s následující identifikátor URI:</span><span class="sxs-lookup"><span data-stu-id="e738a-184">Use the **DELETE** method with the following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

<span data-ttu-id="e738a-185">V rámci identifikátoru URI proveďte následující náhradu k přizpůsobení vaší žádosti:</span><span class="sxs-lookup"><span data-stu-id="e738a-185">Within the URI, make the following substitutions to customize your request:</span></span>

1. <span data-ttu-id="e738a-186">Nahraďte *{oboru}* k oboru, ve kterém chcete vytvořit přiřazení role.</span><span class="sxs-lookup"><span data-stu-id="e738a-186">Replace *{scope}* with the scope at which you wish to create the role assignments.</span></span> <span data-ttu-id="e738a-187">Následující příklady ukazují, jak lze určit obor pro různé úrovně:</span><span class="sxs-lookup"><span data-stu-id="e738a-187">The following examples show how to specify the scope for different levels:</span></span>

   * <span data-ttu-id="e738a-188">Předplatné: /subscriptions/ {id předplatného}</span><span class="sxs-lookup"><span data-stu-id="e738a-188">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="e738a-189">Skupina prostředků: /subscriptions/ {id předplatného} / Skupinyprostředků/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="e738a-189">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="e738a-190">Prostředek: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="e738a-190">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="e738a-191">Nahraďte *{id role přiřazení}* s id přiřazení role identifikátor GUID.</span><span class="sxs-lookup"><span data-stu-id="e738a-191">Replace *{role-assignment-id}* with the role assignment id GUID.</span></span>
3. <span data-ttu-id="e738a-192">Nahraďte *{api-version}* s 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="e738a-192">Replace *{api-version}* with 2015-07-01.</span></span>

### <a name="response"></a><span data-ttu-id="e738a-193">Odpověď</span><span class="sxs-lookup"><span data-stu-id="e738a-193">Response</span></span>
<span data-ttu-id="e738a-194">Stavový kód: 200</span><span class="sxs-lookup"><span data-stu-id="e738a-194">Status code: 200</span></span>

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

## <a name="list-all-roles"></a><span data-ttu-id="e738a-195">Zobrazí seznam všech rolí</span><span class="sxs-lookup"><span data-stu-id="e738a-195">List all Roles</span></span>
<span data-ttu-id="e738a-196">Obsahuje seznam všech rolí, které jsou k dispozici pro přiřazení v zadaném oboru.</span><span class="sxs-lookup"><span data-stu-id="e738a-196">Lists all the roles that are available for assignment at the specified scope.</span></span>

<span data-ttu-id="e738a-197">Seznam rolí musíte mít přístup k `Microsoft.Authorization/roleDefinitions/read` operace v oboru.</span><span class="sxs-lookup"><span data-stu-id="e738a-197">To list roles, you must have access to `Microsoft.Authorization/roleDefinitions/read` operation at the scope.</span></span> <span data-ttu-id="e738a-198">Mezi integrované role mají přístup k této operace.</span><span class="sxs-lookup"><span data-stu-id="e738a-198">All the built-in roles are granted access to this operation.</span></span> <span data-ttu-id="e738a-199">Další informace o přiřazení rolí a správu přístupu k prostředkům Azure najdete v tématu [řízení přístupu](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="e738a-199">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="e738a-200">Žádost</span><span class="sxs-lookup"><span data-stu-id="e738a-200">Request</span></span>
<span data-ttu-id="e738a-201">Použití **získat** metoda s následující identifikátor URI:</span><span class="sxs-lookup"><span data-stu-id="e738a-201">Use the **GET** method with the following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version={api-version}&$filter={filter}

<span data-ttu-id="e738a-202">V rámci identifikátoru URI proveďte následující náhradu k přizpůsobení vaší žádosti:</span><span class="sxs-lookup"><span data-stu-id="e738a-202">Within the URI, make the following substitutions to customize your request:</span></span>

1. <span data-ttu-id="e738a-203">Nahraďte *{oboru}* s oborem, pro který chcete zobrazit seznam rolí.</span><span class="sxs-lookup"><span data-stu-id="e738a-203">Replace *{scope}* with the scope for which you wish to list the roles.</span></span> <span data-ttu-id="e738a-204">Následující příklady ukazují, jak lze určit obor pro různé úrovně:</span><span class="sxs-lookup"><span data-stu-id="e738a-204">The following examples show how to specify the scope for different levels:</span></span>

   * <span data-ttu-id="e738a-205">Předplatné: /subscriptions/ {id předplatného}</span><span class="sxs-lookup"><span data-stu-id="e738a-205">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="e738a-206">Skupina prostředků: /subscriptions/ {id předplatného} / Skupinyprostředků/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="e738a-206">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="e738a-207">/Subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1 prostředků</span><span class="sxs-lookup"><span data-stu-id="e738a-207">Resource /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="e738a-208">Nahraďte *{api-version}* s 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="e738a-208">Replace *{api-version}* with 2015-07-01.</span></span>
3. <span data-ttu-id="e738a-209">Nahraďte *{filtru}* s podmínku, kterou chcete použít pro filtrování seznamu rolí:</span><span class="sxs-lookup"><span data-stu-id="e738a-209">Replace *{filter}* with the condition that you wish to apply to filter the list of roles:</span></span>

   * <span data-ttu-id="e738a-210">Seznam rolí, které jsou k dispozici pro přiřazení v zadaném oboru a všechny její podřízené obory:`atScopeAndBelow()`</span><span class="sxs-lookup"><span data-stu-id="e738a-210">List roles available for assignment at the specified scope and any of its child scopes: `atScopeAndBelow()`</span></span>
   * <span data-ttu-id="e738a-211">Vyhledejte roli pomocí přesný zobrazovaný název: `roleName%20eq%20'{role-display-name}'`.</span><span class="sxs-lookup"><span data-stu-id="e738a-211">Search for a role using exact display name: `roleName%20eq%20'{role-display-name}'`.</span></span> <span data-ttu-id="e738a-212">Pomocí formuláře kódovaná adresou URL přesný zobrazovaného názvu role.</span><span class="sxs-lookup"><span data-stu-id="e738a-212">Use the URL encoded form of the exact display name of the role.</span></span> <span data-ttu-id="e738a-213">Pro instanci,`$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |</span><span class="sxs-lookup"><span data-stu-id="e738a-213">For instance, `$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |</span></span>

### <a name="response"></a><span data-ttu-id="e738a-214">Odpověď</span><span class="sxs-lookup"><span data-stu-id="e738a-214">Response</span></span>
<span data-ttu-id="e738a-215">Stavový kód: 200</span><span class="sxs-lookup"><span data-stu-id="e738a-215">Status code: 200</span></span>

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they\u2019re connected to.",
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

## <a name="get-information-about-a-role"></a><span data-ttu-id="e738a-216">Získat informace o roli</span><span class="sxs-lookup"><span data-stu-id="e738a-216">Get information about a Role</span></span>
<span data-ttu-id="e738a-217">Získá informace o jedné role určeného identifikátor definice role.</span><span class="sxs-lookup"><span data-stu-id="e738a-217">Gets information about a single role specified by the role definition identifier.</span></span> <span data-ttu-id="e738a-218">Chcete-li získat informace o jedné role pomocí názvu zobrazení, přečtěte si téma [seznam všech rolí](role-based-access-control-manage-access-rest.md#list-all-roles).</span><span class="sxs-lookup"><span data-stu-id="e738a-218">To get information about a single role using its display name, see [List all roles](role-based-access-control-manage-access-rest.md#list-all-roles).</span></span>

<span data-ttu-id="e738a-219">Chcete-li získat informace o roli, musíte mít přístup k `Microsoft.Authorization/roleDefinitions/read` operaci.</span><span class="sxs-lookup"><span data-stu-id="e738a-219">To get information about a role, you must have access to `Microsoft.Authorization/roleDefinitions/read` operation.</span></span> <span data-ttu-id="e738a-220">Mezi integrované role mají přístup k této operace.</span><span class="sxs-lookup"><span data-stu-id="e738a-220">All the built-in roles are granted access to this operation.</span></span> <span data-ttu-id="e738a-221">Další informace o přiřazení rolí a správu přístupu k prostředkům Azure najdete v tématu [řízení přístupu](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="e738a-221">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="e738a-222">Žádost</span><span class="sxs-lookup"><span data-stu-id="e738a-222">Request</span></span>
<span data-ttu-id="e738a-223">Použití **získat** metoda s následující identifikátor URI:</span><span class="sxs-lookup"><span data-stu-id="e738a-223">Use the **GET** method with the following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

<span data-ttu-id="e738a-224">V rámci identifikátoru URI proveďte následující náhradu k přizpůsobení vaší žádosti:</span><span class="sxs-lookup"><span data-stu-id="e738a-224">Within the URI, make the following substitutions to customize your request:</span></span>

1. <span data-ttu-id="e738a-225">Nahraďte *{oboru}* s oborem, pro který chcete zobrazit seznam přiřazení rolí.</span><span class="sxs-lookup"><span data-stu-id="e738a-225">Replace *{scope}* with the scope for which you wish to list the role assignments.</span></span> <span data-ttu-id="e738a-226">Následující příklady ukazují, jak lze určit obor pro různé úrovně:</span><span class="sxs-lookup"><span data-stu-id="e738a-226">The following examples show how to specify the scope for different levels:</span></span>

   * <span data-ttu-id="e738a-227">Předplatné: /subscriptions/ {id předplatného}</span><span class="sxs-lookup"><span data-stu-id="e738a-227">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="e738a-228">Skupina prostředků: /subscriptions/ {id předplatného} / Skupinyprostředků/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="e738a-228">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="e738a-229">Prostředek: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="e738a-229">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="e738a-230">Nahraďte *{id role definice}* s identifikátorem GUID definice role.</span><span class="sxs-lookup"><span data-stu-id="e738a-230">Replace *{role-definition-id}* with the GUID identifier of the role definition.</span></span>
3. <span data-ttu-id="e738a-231">Nahraďte *{api-version}* s 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="e738a-231">Replace *{api-version}* with 2015-07-01.</span></span>

### <a name="response"></a><span data-ttu-id="e738a-232">Odpověď</span><span class="sxs-lookup"><span data-stu-id="e738a-232">Response</span></span>
<span data-ttu-id="e738a-233">Stavový kód: 200</span><span class="sxs-lookup"><span data-stu-id="e738a-233">Status code: 200</span></span>

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they\u2019re connected to.",
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

## <a name="create-a-custom-role"></a><span data-ttu-id="e738a-234">Vytvořit vlastní roli</span><span class="sxs-lookup"><span data-stu-id="e738a-234">Create a Custom Role</span></span>
<span data-ttu-id="e738a-235">Vytvořte vlastní roli.</span><span class="sxs-lookup"><span data-stu-id="e738a-235">Create a custom role.</span></span>

<span data-ttu-id="e738a-236">Pokud chcete vytvořit vlastní roli, musíte mít přístup k `Microsoft.Authorization/roleDefinitions/write` operaci na všech `AssignableScopes`.</span><span class="sxs-lookup"><span data-stu-id="e738a-236">To create a custom role, you must have access to `Microsoft.Authorization/roleDefinitions/write` operation on all the `AssignableScopes`.</span></span> <span data-ttu-id="e738a-237">Z předdefinovaných rolí pouze *vlastníka* a *správce přístupu uživatelů* mají udělen přístup k této operaci.</span><span class="sxs-lookup"><span data-stu-id="e738a-237">Of the built-in roles, only *Owner* and *User Access Administrator* are granted access to this operation.</span></span> <span data-ttu-id="e738a-238">Další informace o přiřazení rolí a správu přístupu k prostředkům Azure najdete v tématu [řízení přístupu](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="e738a-238">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="e738a-239">Žádost</span><span class="sxs-lookup"><span data-stu-id="e738a-239">Request</span></span>
<span data-ttu-id="e738a-240">Použití **PUT** metoda s následující identifikátor URI:</span><span class="sxs-lookup"><span data-stu-id="e738a-240">Use the **PUT** method with the following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

<span data-ttu-id="e738a-241">V rámci identifikátoru URI proveďte následující náhradu k přizpůsobení vaší žádosti:</span><span class="sxs-lookup"><span data-stu-id="e738a-241">Within the URI, make the following substitutions to customize your request:</span></span>

1. <span data-ttu-id="e738a-242">Nahraďte *{oboru}* s prvním *AssignableScope* vlastní role.</span><span class="sxs-lookup"><span data-stu-id="e738a-242">Replace *{scope}* with the first *AssignableScope* of the custom role.</span></span> <span data-ttu-id="e738a-243">Následující příklady ukazují, jak lze určit obor pro různé úrovně.</span><span class="sxs-lookup"><span data-stu-id="e738a-243">The following examples show how to specify the scope for different levels.</span></span>

   * <span data-ttu-id="e738a-244">Předplatné: /subscriptions/ {id předplatného}</span><span class="sxs-lookup"><span data-stu-id="e738a-244">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="e738a-245">Skupina prostředků: /subscriptions/ {id předplatného} / Skupinyprostředků/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="e738a-245">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="e738a-246">Prostředek: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="e738a-246">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="e738a-247">Nahraďte *{id role definice}* s nový identifikátor GUID, který se stane identifikátor GUID novou vlastní roli.</span><span class="sxs-lookup"><span data-stu-id="e738a-247">Replace *{role-definition-id}* with a new GUID, which becomes the GUID identifier of the new custom role.</span></span>
3. <span data-ttu-id="e738a-248">Nahraďte *{api-version}* s 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="e738a-248">Replace *{api-version}* with 2015-07-01.</span></span>

<span data-ttu-id="e738a-249">Tělo žádosti zadejte hodnoty v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="e738a-249">For the request body, provide the values in the following format:</span></span>

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

| <span data-ttu-id="e738a-250">Název elementu</span><span class="sxs-lookup"><span data-stu-id="e738a-250">Element Name</span></span> | <span data-ttu-id="e738a-251">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="e738a-251">Required</span></span> | <span data-ttu-id="e738a-252">Typ</span><span class="sxs-lookup"><span data-stu-id="e738a-252">Type</span></span> | <span data-ttu-id="e738a-253">Popis</span><span class="sxs-lookup"><span data-stu-id="e738a-253">Description</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="e738a-254">jméno</span><span class="sxs-lookup"><span data-stu-id="e738a-254">name</span></span> |<span data-ttu-id="e738a-255">Ano</span><span class="sxs-lookup"><span data-stu-id="e738a-255">Yes</span></span> |<span data-ttu-id="e738a-256">Řetězec</span><span class="sxs-lookup"><span data-stu-id="e738a-256">String</span></span> |<span data-ttu-id="e738a-257">Identifikátor GUID vlastní role.</span><span class="sxs-lookup"><span data-stu-id="e738a-257">GUID identifier of the custom role.</span></span> |
| <span data-ttu-id="e738a-258">properties.roleName</span><span class="sxs-lookup"><span data-stu-id="e738a-258">properties.roleName</span></span> |<span data-ttu-id="e738a-259">Ano</span><span class="sxs-lookup"><span data-stu-id="e738a-259">Yes</span></span> |<span data-ttu-id="e738a-260">Řetězec</span><span class="sxs-lookup"><span data-stu-id="e738a-260">String</span></span> |<span data-ttu-id="e738a-261">Zobrazovaný název vlastní role.</span><span class="sxs-lookup"><span data-stu-id="e738a-261">Display name of the custom role.</span></span> <span data-ttu-id="e738a-262">Maximální velikost 128 znaků.</span><span class="sxs-lookup"><span data-stu-id="e738a-262">Maximum size 128 characters.</span></span> |
| <span data-ttu-id="e738a-263">Properties.Description</span><span class="sxs-lookup"><span data-stu-id="e738a-263">properties.description</span></span> |<span data-ttu-id="e738a-264">Ne</span><span class="sxs-lookup"><span data-stu-id="e738a-264">No</span></span> |<span data-ttu-id="e738a-265">Řetězec</span><span class="sxs-lookup"><span data-stu-id="e738a-265">String</span></span> |<span data-ttu-id="e738a-266">Popis vlastní role.</span><span class="sxs-lookup"><span data-stu-id="e738a-266">Description of the custom role.</span></span> <span data-ttu-id="e738a-267">Maximální velikost 1024 znaků.</span><span class="sxs-lookup"><span data-stu-id="e738a-267">Maximum size 1024 characters.</span></span> |
| <span data-ttu-id="e738a-268">Properties.Type</span><span class="sxs-lookup"><span data-stu-id="e738a-268">properties.type</span></span> |<span data-ttu-id="e738a-269">Ano</span><span class="sxs-lookup"><span data-stu-id="e738a-269">Yes</span></span> |<span data-ttu-id="e738a-270">Řetězec</span><span class="sxs-lookup"><span data-stu-id="e738a-270">String</span></span> |<span data-ttu-id="e738a-271">Nastavte na "CustomRole."</span><span class="sxs-lookup"><span data-stu-id="e738a-271">Set to "CustomRole."</span></span> |
| <span data-ttu-id="e738a-272">Properties.Permissions.Actions</span><span class="sxs-lookup"><span data-stu-id="e738a-272">properties.permissions.actions</span></span> |<span data-ttu-id="e738a-273">Ano</span><span class="sxs-lookup"><span data-stu-id="e738a-273">Yes</span></span> |<span data-ttu-id="e738a-274">řetězec]</span><span class="sxs-lookup"><span data-stu-id="e738a-274">String[]</span></span> |<span data-ttu-id="e738a-275">Pole řetězců akce zadání operace udělit pomocí vlastní role.</span><span class="sxs-lookup"><span data-stu-id="e738a-275">An array of action strings specifying the operations granted by the custom role.</span></span> |
| <span data-ttu-id="e738a-276">properties.permissions.notActions</span><span class="sxs-lookup"><span data-stu-id="e738a-276">properties.permissions.notActions</span></span> |<span data-ttu-id="e738a-277">Ne</span><span class="sxs-lookup"><span data-stu-id="e738a-277">No</span></span> |<span data-ttu-id="e738a-278">řetězec]</span><span class="sxs-lookup"><span data-stu-id="e738a-278">String[]</span></span> |<span data-ttu-id="e738a-279">Pole řetězců akce zadání operace, které chcete vyloučit z operace udělit pomocí vlastní role.</span><span class="sxs-lookup"><span data-stu-id="e738a-279">An array of action strings specifying the operations to exclude from the operations granted by the custom role.</span></span> |
| <span data-ttu-id="e738a-280">properties.assignableScopes</span><span class="sxs-lookup"><span data-stu-id="e738a-280">properties.assignableScopes</span></span> |<span data-ttu-id="e738a-281">Ano</span><span class="sxs-lookup"><span data-stu-id="e738a-281">Yes</span></span> |<span data-ttu-id="e738a-282">řetězec]</span><span class="sxs-lookup"><span data-stu-id="e738a-282">String[]</span></span> |<span data-ttu-id="e738a-283">Pole obory, kde můžete použít vlastní role.</span><span class="sxs-lookup"><span data-stu-id="e738a-283">An array of scopes in which the custom role can be used.</span></span> |

### <a name="response"></a><span data-ttu-id="e738a-284">Odpověď</span><span class="sxs-lookup"><span data-stu-id="e738a-284">Response</span></span>
<span data-ttu-id="e738a-285">Stavový kód: 201</span><span class="sxs-lookup"><span data-stu-id="e738a-285">Status code: 201</span></span>

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

## <a name="update-a-custom-role"></a><span data-ttu-id="e738a-286">Aktualizovat vlastní Role</span><span class="sxs-lookup"><span data-stu-id="e738a-286">Update a Custom Role</span></span>
<span data-ttu-id="e738a-287">Upravte vlastní roli.</span><span class="sxs-lookup"><span data-stu-id="e738a-287">Modify a custom role.</span></span>

<span data-ttu-id="e738a-288">Pokud chcete upravit vlastní roli, musíte mít přístup k `Microsoft.Authorization/roleDefinitions/write` operaci na všech `AssignableScopes`.</span><span class="sxs-lookup"><span data-stu-id="e738a-288">To modify a custom role, you must have access to `Microsoft.Authorization/roleDefinitions/write` operation on all the `AssignableScopes`.</span></span> <span data-ttu-id="e738a-289">Z předdefinovaných rolí pouze *vlastníka* a *správce přístupu uživatelů* mají udělen přístup k této operaci.</span><span class="sxs-lookup"><span data-stu-id="e738a-289">Of the built-in roles, only *Owner* and *User Access Administrator* are granted access to this operation.</span></span> <span data-ttu-id="e738a-290">Další informace o přiřazení rolí a správu přístupu k prostředkům Azure najdete v tématu [řízení přístupu](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="e738a-290">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="e738a-291">Žádost</span><span class="sxs-lookup"><span data-stu-id="e738a-291">Request</span></span>
<span data-ttu-id="e738a-292">Použití **PUT** metoda s následující identifikátor URI:</span><span class="sxs-lookup"><span data-stu-id="e738a-292">Use the **PUT** method with the following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

<span data-ttu-id="e738a-293">V rámci identifikátoru URI proveďte následující náhradu k přizpůsobení vaší žádosti:</span><span class="sxs-lookup"><span data-stu-id="e738a-293">Within the URI, make the following substitutions to customize your request:</span></span>

1. <span data-ttu-id="e738a-294">Nahraďte *{oboru}* s prvním *AssignableScope* vlastní role.</span><span class="sxs-lookup"><span data-stu-id="e738a-294">Replace *{scope}* with the first *AssignableScope* of the custom role.</span></span> <span data-ttu-id="e738a-295">Následující příklady ukazují, jak lze určit obor pro různé úrovně:</span><span class="sxs-lookup"><span data-stu-id="e738a-295">The following examples show how to specify the scope for different levels:</span></span>

   * <span data-ttu-id="e738a-296">Předplatné: /subscriptions/ {id předplatného}</span><span class="sxs-lookup"><span data-stu-id="e738a-296">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="e738a-297">Skupina prostředků: /subscriptions/ {id předplatného} / Skupinyprostředků/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="e738a-297">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="e738a-298">Prostředek: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="e738a-298">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="e738a-299">Nahraďte *{id role definice}* s identifikátorem GUID vlastní role.</span><span class="sxs-lookup"><span data-stu-id="e738a-299">Replace *{role-definition-id}* with the GUID identifier of the custom role.</span></span>
3. <span data-ttu-id="e738a-300">Nahraďte *{api-version}* s 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="e738a-300">Replace *{api-version}* with 2015-07-01.</span></span>

<span data-ttu-id="e738a-301">Tělo žádosti zadejte hodnoty v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="e738a-301">For the request body, provide the values in the following format:</span></span>

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

| <span data-ttu-id="e738a-302">Název elementu</span><span class="sxs-lookup"><span data-stu-id="e738a-302">Element Name</span></span> | <span data-ttu-id="e738a-303">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="e738a-303">Required</span></span> | <span data-ttu-id="e738a-304">Typ</span><span class="sxs-lookup"><span data-stu-id="e738a-304">Type</span></span> | <span data-ttu-id="e738a-305">Popis</span><span class="sxs-lookup"><span data-stu-id="e738a-305">Description</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="e738a-306">jméno</span><span class="sxs-lookup"><span data-stu-id="e738a-306">name</span></span> |<span data-ttu-id="e738a-307">Ano</span><span class="sxs-lookup"><span data-stu-id="e738a-307">Yes</span></span> |<span data-ttu-id="e738a-308">Řetězec</span><span class="sxs-lookup"><span data-stu-id="e738a-308">String</span></span> |<span data-ttu-id="e738a-309">Identifikátor GUID vlastní role.</span><span class="sxs-lookup"><span data-stu-id="e738a-309">GUID identifier of the custom role.</span></span> |
| <span data-ttu-id="e738a-310">properties.roleName</span><span class="sxs-lookup"><span data-stu-id="e738a-310">properties.roleName</span></span> |<span data-ttu-id="e738a-311">Ano</span><span class="sxs-lookup"><span data-stu-id="e738a-311">Yes</span></span> |<span data-ttu-id="e738a-312">Řetězec</span><span class="sxs-lookup"><span data-stu-id="e738a-312">String</span></span> |<span data-ttu-id="e738a-313">Zobrazovaný název aktualizované vlastní role.</span><span class="sxs-lookup"><span data-stu-id="e738a-313">Display name of the updated custom role.</span></span> |
| <span data-ttu-id="e738a-314">Properties.Description</span><span class="sxs-lookup"><span data-stu-id="e738a-314">properties.description</span></span> |<span data-ttu-id="e738a-315">Ne</span><span class="sxs-lookup"><span data-stu-id="e738a-315">No</span></span> |<span data-ttu-id="e738a-316">Řetězec</span><span class="sxs-lookup"><span data-stu-id="e738a-316">String</span></span> |<span data-ttu-id="e738a-317">Popis aktualizované vlastní role.</span><span class="sxs-lookup"><span data-stu-id="e738a-317">Description of the updated custom role.</span></span> |
| <span data-ttu-id="e738a-318">Properties.Type</span><span class="sxs-lookup"><span data-stu-id="e738a-318">properties.type</span></span> |<span data-ttu-id="e738a-319">Ano</span><span class="sxs-lookup"><span data-stu-id="e738a-319">Yes</span></span> |<span data-ttu-id="e738a-320">Řetězec</span><span class="sxs-lookup"><span data-stu-id="e738a-320">String</span></span> |<span data-ttu-id="e738a-321">Nastavte na "CustomRole."</span><span class="sxs-lookup"><span data-stu-id="e738a-321">Set to "CustomRole."</span></span> |
| <span data-ttu-id="e738a-322">Properties.Permissions.Actions</span><span class="sxs-lookup"><span data-stu-id="e738a-322">properties.permissions.actions</span></span> |<span data-ttu-id="e738a-323">Ano</span><span class="sxs-lookup"><span data-stu-id="e738a-323">Yes</span></span> |<span data-ttu-id="e738a-324">řetězec]</span><span class="sxs-lookup"><span data-stu-id="e738a-324">String[]</span></span> |<span data-ttu-id="e738a-325">Pole řetězců akce zadání operací, u kterých aktualizované vlastní role uděluje přístup.</span><span class="sxs-lookup"><span data-stu-id="e738a-325">An array of action strings specifying the operations to which the updated custom role grants access.</span></span> |
| <span data-ttu-id="e738a-326">properties.permissions.notActions</span><span class="sxs-lookup"><span data-stu-id="e738a-326">properties.permissions.notActions</span></span> |<span data-ttu-id="e738a-327">Ne</span><span class="sxs-lookup"><span data-stu-id="e738a-327">No</span></span> |<span data-ttu-id="e738a-328">řetězec]</span><span class="sxs-lookup"><span data-stu-id="e738a-328">String[]</span></span> |<span data-ttu-id="e738a-329">Pole určující operace, které chcete vyloučit z operací, které aktualizované vlastní role uděluje řetězce akce.</span><span class="sxs-lookup"><span data-stu-id="e738a-329">An array of action strings specifying the operations to exclude from the operations which the updated custom role grants.</span></span> |
| <span data-ttu-id="e738a-330">properties.assignableScopes</span><span class="sxs-lookup"><span data-stu-id="e738a-330">properties.assignableScopes</span></span> |<span data-ttu-id="e738a-331">Ano</span><span class="sxs-lookup"><span data-stu-id="e738a-331">Yes</span></span> |<span data-ttu-id="e738a-332">řetězec]</span><span class="sxs-lookup"><span data-stu-id="e738a-332">String[]</span></span> |<span data-ttu-id="e738a-333">Pole obory, ve kterých lze použít aktualizované vlastní role.</span><span class="sxs-lookup"><span data-stu-id="e738a-333">An array of scopes in which the updated custom role can be used.</span></span> |

### <a name="response"></a><span data-ttu-id="e738a-334">Odpověď</span><span class="sxs-lookup"><span data-stu-id="e738a-334">Response</span></span>
<span data-ttu-id="e738a-335">Stavový kód: 201</span><span class="sxs-lookup"><span data-stu-id="e738a-335">Status code: 201</span></span>

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

## <a name="delete-a-custom-role"></a><span data-ttu-id="e738a-336">Odstranit vlastní roli</span><span class="sxs-lookup"><span data-stu-id="e738a-336">Delete a Custom Role</span></span>
<span data-ttu-id="e738a-337">Odstraňte vlastní roli.</span><span class="sxs-lookup"><span data-stu-id="e738a-337">Delete a custom role.</span></span>

<span data-ttu-id="e738a-338">Chcete-li odstranit vlastní roli, musíte mít přístup k `Microsoft.Authorization/roleDefinitions/delete` operaci na všech `AssignableScopes`.</span><span class="sxs-lookup"><span data-stu-id="e738a-338">To delete a custom role, you must have access to `Microsoft.Authorization/roleDefinitions/delete` operation on all the `AssignableScopes`.</span></span> <span data-ttu-id="e738a-339">Z předdefinovaných rolí pouze *vlastníka* a *správce přístupu uživatelů* mají udělen přístup k této operaci.</span><span class="sxs-lookup"><span data-stu-id="e738a-339">Of the built-in roles, only *Owner* and *User Access Administrator* are granted access to this operation.</span></span> <span data-ttu-id="e738a-340">Další informace o přiřazení rolí a správu přístupu k prostředkům Azure najdete v tématu [řízení přístupu](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="e738a-340">For more information about role assignments and managing access for Azure resources, see [Azure Role-Based Access Control](role-based-access-control-configure.md).</span></span>

### <a name="request"></a><span data-ttu-id="e738a-341">Žádost</span><span class="sxs-lookup"><span data-stu-id="e738a-341">Request</span></span>
<span data-ttu-id="e738a-342">Použití **odstranit** metoda s následující identifikátor URI:</span><span class="sxs-lookup"><span data-stu-id="e738a-342">Use the **DELETE** method with the following URI:</span></span>

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

<span data-ttu-id="e738a-343">V rámci identifikátoru URI proveďte následující náhradu k přizpůsobení vaší žádosti:</span><span class="sxs-lookup"><span data-stu-id="e738a-343">Within the URI, make the following substitutions to customize your request:</span></span>

1. <span data-ttu-id="e738a-344">Nahraďte *{oboru}* s oborem, na které chcete odstranit definici role.</span><span class="sxs-lookup"><span data-stu-id="e738a-344">Replace *{scope}* with the scope at which you wish to delete the role definition.</span></span> <span data-ttu-id="e738a-345">Následující příklady ukazují, jak lze určit obor pro různé úrovně:</span><span class="sxs-lookup"><span data-stu-id="e738a-345">The following examples show how to specify the scope for different levels:</span></span>

   * <span data-ttu-id="e738a-346">Předplatné: /subscriptions/ {id předplatného}</span><span class="sxs-lookup"><span data-stu-id="e738a-346">Subscription: /subscriptions/{subscription-id}</span></span>  
   * <span data-ttu-id="e738a-347">Skupina prostředků: /subscriptions/ {id předplatného} / Skupinyprostředků/myresourcegroup1</span><span class="sxs-lookup"><span data-stu-id="e738a-347">Resource Group: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1</span></span>  
   * <span data-ttu-id="e738a-348">Prostředek: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span><span class="sxs-lookup"><span data-stu-id="e738a-348">Resource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1</span></span>  
2. <span data-ttu-id="e738a-349">Nahraďte *{id role definice}* s id definice role GUID vlastní role.</span><span class="sxs-lookup"><span data-stu-id="e738a-349">Replace *{role-definition-id}* with the GUID role definition id of the custom role.</span></span>
3. <span data-ttu-id="e738a-350">Nahraďte *{api-version}* s 2015-07-01.</span><span class="sxs-lookup"><span data-stu-id="e738a-350">Replace *{api-version}* with 2015-07-01.</span></span>

### <a name="response"></a><span data-ttu-id="e738a-351">Odpověď</span><span class="sxs-lookup"><span data-stu-id="e738a-351">Response</span></span>
<span data-ttu-id="e738a-352">Stavový kód: 200</span><span class="sxs-lookup"><span data-stu-id="e738a-352">Status code: 200</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="e738a-353">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e738a-353">Next steps</span></span>

[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
