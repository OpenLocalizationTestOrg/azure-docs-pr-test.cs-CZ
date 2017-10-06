---
title: "aaaManage na základě rolí řízení přístupu (RBAC) pomocí prostředí Azure PowerShell | Microsoft Docs"
description: "Jak toomanage RBAC pomocí Azure Powershellu, včetně obsahující seznam rolí, přiřazení rolí a odstraňovat přiřazení role."
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
ms.assetid: 9e225dba-9044-4b13-b573-2f30d77925a9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.openlocfilehash: fa44991113e75b345177867b0bede38de4373e04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-role-based-access-control-with-azure-powershell"></a>Správa řízení přístupu podle role pomocí prostředí Azure PowerShell
> [!div class="op_single_selector"]
> * [PowerShell](role-based-access-control-manage-access-powershell.md)
> * [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
> * [REST API](role-based-access-control-manage-access-rest.md)

Řízení přístupu na základě Role (RBAC) můžete použít v hello portál Azure a rozhraní API pro správu prostředků Azure toomanage přístup tooyour předplatné na velice přesné úrovni. Pomocí této funkce můžete udělit přístup pro uživatele, skupiny nebo objekty služby Active Directory přiřazením některé role toothem v určitém rozsahu.

Než budete moct použít PowerShell toomanage RBAC, je třeba hello následující požadavky:

* Azure PowerShell verze 0.8.8 nebo novější. tooinstall hello nejnovější verzi a přidružit ho ke svému předplatnému Azure, najdete v části [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).
* Rutiny Azure Resource Manager. Nainstalujte hello [rutiny Azure Resource Manager](/powershell/azure/overview) v prostředí PowerShell.

## <a name="list-roles"></a>Seznam rolí
### <a name="list-all-available-roles"></a>Zobrazí seznam všech dostupných rolí
role RBAC toolist, které jsou k dispozici pro přiřazení a tooinspect hello operations toowhich jejich udělit přístup, používají `Get-AzureRmRoleDefinition`.

```
Get-AzureRmRoleDefinition | FT Name, Description
```

![RBAC prostředí PowerShell-Get AzureRmRoleDefinition – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition1.png)

### <a name="list-actions-of-a-role"></a>Seznam akcí role
toolist hello akce pro určité role, využívají `Get-AzureRmRoleDefinition <role name>`.

```
Get-AzureRmRoleDefinition Contributor | FL Actions, NotActions

(Get-AzureRmRoleDefinition "Virtual Machine Contributor").Actions
```

![RBAC prostředí PowerShell-Get AzureRmRoleDefinition pro určité role – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition2.png)

## <a name="see-who-has-access"></a>Zjistit, kdo má přístup
přiřazení přístupu RBAC toolist, použijte `Get-AzureRmRoleAssignment`.

### <a name="list-role-assignments-at-a-specific-scope"></a>Seznam přiřazení rolí na konkrétní rozsah
Můžete zobrazit všechny hello přiřazení přístupu pro zadané předplatné, skupinu prostředků nebo prostředek. Například toosee hello všechna přiřazení active hello pro skupinu prostředků, použijte `Get-AzureRmRoleAssignment -ResourceGroupName <resource group name>`.

```
Get-AzureRmRoleAssignment -ResourceGroupName Pharma-Sales-ProjectForcast | FL DisplayName, RoleDefinitionName, Scope
```

![RBAC prostředí PowerShell - Get-AzureRmRoleAssignment pro skupinu prostředků – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment1.png)

### <a name="list-roles-assigned-tooa-user"></a>Seznam role přiřazené tooa uživatele
toolist všechny hello role, které jsou přiřazeny tooa zadané uživatele a hello role, které jsou přiřazeny skupiny toohello toowhich hello uživatel patří, použijte `Get-AzureRmRoleAssignment -SignInName <User email> -ExpandPrincipalGroups`.

```
Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com | FL DisplayName, RoleDefinitionName, Scope

Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com -ExpandPrincipalGroups | FL DisplayName, RoleDefinitionName, Scope
```

![RBAC prostředí PowerShell - Get-AzureRmRoleAssignment pro uživatele – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment2.png)

### <a name="list-classic-service-administrator-and-coadmin-role-assignments"></a>Seznam přiřazení rolí správce a coadmin classic služby
přiřazení přístupu toolist pro správce hello classic předplatného a coadministrators, použijte:

    Get-AzureRmRoleAssignment -IncludeClassicAdministrators

## <a name="grant-access"></a>Udělení přístupu
### <a name="search-for-object-ids"></a>Vyhledejte ID objektů
tooassign roli, je nutné tooidentify hello objektu (uživatele, skupiny nebo aplikace) a hello oboru.

Pokud si nejste jisti hello ID předplatného, můžete nějakého najít v hello **odběry** okně hello portálu Azure. jak zjistit, tooquery pro ID předplatného hello, toolearn [Get-AzureSubscription](/powershell/module/azure/get-azuresubscription?view=azuresmps-3.7.0) na webu MSDN.

ID objektu hello tooget pro skupiny služby Azure AD, použijte:

    Get-AzureRmADGroup -SearchString <group name in quotes>

ID objektu hello tooget pro objekt zabezpečení služby Azure AD nebo aplikaci, použijte:

    Get-AzureRmADServicePrincipal -SearchString <service name in quotes>

### <a name="assign-a-role-tooan-application-at-hello-subscription-scope"></a>Přiřadit k aplikaci tooan role v oboru předplatné hello
toogrant přístup tooan aplikaci v hello obor předplatného, použijte:

    New-AzureRmRoleAssignment -ObjectId <application id> -RoleDefinitionName <role name> -Scope <subscription id>

![RBAC prostředí PowerShell - nové-AzureRmRoleAssignment – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment2.png)

### <a name="assign-a-role-tooa-user-at-hello-resource-group-scope"></a>Přiřazení role uživatele tooa v oboru skupiny prostředků hello
uživatel tooa toogrant přístupu v oboru skupiny hello prostředků, použijte:

    New-AzureRmRoleAssignment -SignInName <email of user> -RoleDefinitionName <role name in quotes> -ResourceGroupName <resource group name>

![RBAC prostředí PowerShell - nové-AzureRmRoleAssignment – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment3.png)

### <a name="assign-a-role-tooa-group-at-hello-resource-scope"></a>Přiřadit role tooa skupinu v oboru prostředků hello
Skupina tooa toogrant přístupu v oboru hello prostředků, použijte:

    New-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name in quotes> -ResourceName <resource name> -ResourceType <resource type> -ParentResource <parent resource> -ResourceGroupName <resource group name>

![RBAC prostředí PowerShell - nové-AzureRmRoleAssignment – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment4.png)

## <a name="remove-access"></a>Odebrání přístupu
tooremove přístup pro uživatele, skupiny a aplikace, použijte:

    Remove-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name> -Scope <scope such as subscription id>

![RBAC prostředí PowerShell - Remove-AzureRmRoleAssignment – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/3-remove-azure-rm-role-assignment.png)

## <a name="create-a-custom-role"></a>Vytvořit vlastní roli
toocreate vlastní roli, použijte hello ```New-AzureRmRoleDefinition``` příkaz. Strukturování hello role pomocí PSRoleDefinitionObject nebo šablonu JSON dvěma způsoby. 

## <a name="get-actions-for-a-resource-provider"></a>Získat akce pro poskytovatele prostředků
Při vytváření vlastní role od začátku, je důležité tooknow všechny možné operace od poskytovatelů prostředků hello hello.
Použití hello ```Get-AzureRMProviderOperation``` příkaz tooget tyto informace.
Například pokud chcete, aby toocheck všechny operace hello k dispozici pro virtuální počítač použijte tento příkaz:

```
Get-AzureRMProviderOperation "Microsoft.Compute/virtualMachines/*" | FT OperationName, Operation , Description -AutoSize
```

### <a name="create-role-with-psroledefinitionobject"></a>Vytvořit roli s PSRoleDefinitionObject
Pokud používáte PowerShell toocreate vlastní roli, můžete začít úplně od začátku nebo použijte jednu z hello [předdefinované role](role-based-access-built-in-roles.md) jako výchozí bod. Hello příklad v této části začíná předdefinovaná role a pak ho přizpůsobí s další oprávnění. Upravit hello atributy tooadd hello *akce*, *notActions*, nebo *obory* a potom uložte změny hello jako novou roli.

Hello následující příklad začíná textem hello *Přispěvatel virtuálních počítačů* role a používá tento toocreate vlastní roli volají *virtuální počítač operátor*. Hello nové role uděluje přístup tooall číst operace *Microsoft.Compute*, *Microsoft.Storage*, a *Microsoft.Network* prostředků poskytovatelů a uděluje přístup toostart, restartování a monitorování virtuálních počítačů. vlastní role Hello lze použít ve dvou odběry.

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Contributor"
$role.Id = $null
$role.Name = "Virtual Machine Operator"
$role.Description = "Can monitor and restart virtual machines."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/*/read")
$role.Actions.Add("Microsoft.Network/*/read")
$role.Actions.Add("Microsoft.Compute/*/read")
$role.Actions.Add("Microsoft.Compute/virtualMachines/start/action")
$role.Actions.Add("Microsoft.Compute/virtualMachines/restart/action")
$role.Actions.Add("Microsoft.Authorization/*/read")
$role.Actions.Add("Microsoft.Resources/subscriptions/resourceGroups/read")
$role.Actions.Add("Microsoft.Insights/alertRules/*")
$role.Actions.Add("Microsoft.Support/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e")
$role.AssignableScopes.Add("/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624")
New-AzureRmRoleDefinition -Role $role
```

![RBAC prostředí PowerShell-Get AzureRmRoleDefinition – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/2-new-azurermroledefinition.png)

### <a name="create-role-with-json-template"></a>Vytvoření role pomocí šablony JSON
Šablonu JSON slouží jako zdroj definice hello hello vlastní role. Hello následující příklad vytvoří vlastní roli, která umožňuje přístup pro čtení toostorage a výpočetní prostředky, přístup k toosupport tuto roli přidá tootwo odběry. Vytvoření nového souboru `C:\CustomRoles\customrole1.json` s hello následující ukázka. Hello Id by mělo být nastavené příliš`null` na vytvoření počáteční role jako nové ID je generován automaticky. 

```
{
  "Name": "Custom Role 1",
  "Id": null,
  "IsCustom": true,
  "Description": "Allows for read access tooAzure storage and compute resources and access toosupport",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Storage/*/read",
    "Microsoft.Support/*"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624"
  ]
}
```
tooadd hello role toohello odběry, spusťte následující příkaz prostředí PowerShell hello:
```
New-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="modify-a-custom-role"></a>Upravit vlastní roli
Podobně jako toocreating vlastní roli, můžete upravit existující vlastní role pomocí hello PSRoleDefinitionObject nebo šablonu JSON.

### <a name="modify-role-with-psroledefinitionobject"></a>Upravit role s PSRoleDefinitionObject
toomodify vlastní role, nejprve pomocí hello `Get-AzureRmRoleDefinition` příkaz definice role tooretrieve hello. Druhý proveďte změny hello potřeby toohello definice role. Nakonec použijte hello `Set-AzureRmRoleDefinition` příkaz toosave hello změnil definici role.

Hello následující příklad přidá hello `Microsoft.Insights/diagnosticSettings/*` operace toohello *virtuální počítač operátor* vlastní role.

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
Set-AzureRmRoleDefinition -Role $role
```

![RBAC prostředí PowerShell - Set-AzureRmRoleDefinition – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-1.png)

Hello následující příklad přidá předplatnému Azure toohello přiřaditelnými obory Dobrý den *virtuální počítač operátor* vlastní role.

```
Get-AzureRmSubscription - SubscriptionName Production3

$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.AssignableScopes.Add("/subscriptions/34370e90-ac4a-4bf9-821f-85eeedead1a2")
Set-AzureRmRoleDefinition -Role $role
```

![RBAC prostředí PowerShell - Set-AzureRmRoleDefinition – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-2.png)

### <a name="modify-role-with-json-template"></a>Upravit role pomocí šablony JSON
Pomocí šablony JSON předchozí hello, můžete snadno upravit existující tooadd vlastní role nebo odebrat akce. Aktualizace šablony JSON hello a přidejte hello čtení akce pro sítě, jak ukazuje následující příklad hello. definice Hello uvedené v šabloně hello nejsou stávající definici kumulativně použité tooan, což znamená, že tato role hello se zobrazí přesně tak, jak zadáte v šabloně hello. Budete také potřebovat tooupdate hello Id pole s ID hello hello role. Pokud si nejste jisti, co je tato hodnota, můžete použít hello `Get-AzureRmRoleDefinition` rutiny tooget tyto informace.

```
{
  "Name": "Custom Role 1",
  "Id": "acce7ded-2559-449d-bcd5-e9604e50bad1",
  "IsCustom": true,
  "Description": "Allows for read access tooAzure storage and compute resources and access toosupport",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Support/*"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624"
  ]
}
```

tooupdate hello existující roli, spusťte následující příkaz prostředí PowerShell hello:
```
Set-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="delete-a-custom-role"></a>Odstranit vlastní roli
toodelete vlastní roli, použijte hello `Remove-AzureRmRoleDefinition` příkaz.

Hello následující příklad odebere hello *virtuální počítač operátor* vlastní role.

```
Get-AzureRmRoleDefinition "Virtual Machine Operator"

Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition
```

![RBAC prostředí PowerShell - Remove-AzureRmRoleDefinition – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/4-remove-azurermroledefinition.png)

## <a name="list-custom-roles"></a>Vlastní role seznamu
toolist hello role, které jsou k dispozici pro přiřazení v oboru, které používají hello `Get-AzureRmRoleDefinition` příkaz.

Hello následující příklad uvádí všechny role, které jsou k dispozici pro přiřazení v hello vybrané předplatné.

```
Get-AzureRmRoleDefinition | FT Name, IsCustom
```

![RBAC prostředí PowerShell-Get AzureRmRoleDefinition – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition-1.png)

V následujícím příkladu hello, hello *virtuální počítač operátor* vlastní role není k dispozici v hello *Production4* předplatné vzhledem k tomu, že toto předplatné není v hello  **AssignableScopes** hello role.

![RBAC prostředí PowerShell-Get AzureRmRoleDefinition – snímek obrazovky](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition2.png)

## <a name="see-also"></a>Viz také
* [Použití Azure PowerShell s Azure Resource Manager](../powershell-azure-resource-manager.md)
  [!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]

