---
title: "Použití řízení přístupu na základě Role v Azure API Management | Microsoft Docs"
description: "Naučte se používat předdefinované role a vytvářet vlastní role v Azure API Management"
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 364cd53e-88fb-4301-a093-f132fa1f88f5
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/27/2017
ms.author: apimpm
ms.openlocfilehash: c775780a39c4d423c62bf88f55d35675c70442c7
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2017
---
# <a name="how-to-use-role-based-access-control-in-azure-api-management"></a>Použití řízení přístupu na základě Role v Azure API Management
Azure API Management spoléhá na řízení řízení přístupu (RBAC) Chcete-li povolit vyladění správy přístupu pro služby API Management a entit (například rozhraní API a zásady). Tento článek obsahuje přehled rolí předdefinované a vlastní ve službě API Management. Další informace o řízení přístupu na portálu Azure najdete v tématu [Začínáme se správou přístupu na portálu Azure](https://azure.microsoft.com/documentation/articles/role-based-access-control-what-is/).

## <a name="built-in-roles"></a>Vestavěné role
API Management aktuálně poskytuje tři předdefinované role a přidá dva další role v blízké budoucnosti. Tyto role mohou být přiřazeny na různých místech, včetně předplatné, skupinu prostředků a jednotlivé instance služby API Management. Například pokud uživatel na úrovni skupiny prostředků přiřadíte roli "Azure API Management Service čtečky", pak uživatel má přístup pro čtení pro všechny instance API Management uvnitř skupiny prostředků. 

Následující tabulka obsahuje stručný popis předdefinovaných rolí. Tyto role lze přiřadit pomocí portálu Azure nebo jiných nástrojů, včetně Azure [prostředí PowerShell](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-powershell), [rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-azure-cli), a [REST API](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-rest). Podrobnosti o tom, jak přiřadit předdefinovaných rolí najdete v tématu [použití přiřazení rolí ke správě přístupu k prostředkům předplatného Azure](https://azure.microsoft.com/documentation/articles/role-based-access-control-what-is/).

| Role          | Přístup pro čtení<sup>[1]</sup> | Přístup pro zápis<sup>[2]</sup> | Vytvoření služby, odstranění, změny velikosti, VPN a konfigurace vlastní domény | Přístup k portálu vydavatele starší verze | Popis
| ------------- | ---- | ---- | ---- | ---- | ---- | ---- |
| Přispěvatel služby Azure API Management | ✓ | ✓ | ✓ | ✓ | Superuživatele. Má úplný přístup CRUD služby API Management a entit (například rozhraní API a zásady). Má přístup k portálu starší verze vydavatele. |
| Čtecí modul služby Azure API Management | ✓ | | || Má přístup jen pro čtení k služby API Management a entity. |
| Operátor služby Azure API Management | ✓ | | ✓ | | Můžete spravovat služby API Management, ale není entity.|
| Azure API Management Service editoru<sup>*</sup> | ✓ | ✓ | |  | Můžete spravovat entity API Management, ale ne služby.|
| Azure API Management Content Manager<sup>*</sup> | ✓ | | | ✓ | Můžete spravovat portál pro vývojáře. Jen pro čtení přístup ke službám a entity.|

<sup>[1] přístup pro čtení služby API Management a entit (například rozhraní API a zásady).</sup>

<sup>[2] oprávnění k zápisu do služby API Management a entity s výjimkou následující operace: instance, vytváření, odstraňování a škálování; Konfigurace sítě VPN; a nastavení vlastní domény.</sup>

<sup>\*Role služby Editor bude k dispozici po jsme migrovat všechny správce uživatelského rozhraní na portálu vydavatele existující k portálu Azure. Role správce obsahu bude k dispozici po portálu vydavatele je teď vyčleněný, aby obsahovala pouze funkce související se správou portálu pro vývojáře.</sup>  

## <a name="custom-roles"></a>Vlastní role
Pokud žádná z předdefinovaných rolí splňovat specifické potřeby, lze vytvořit vlastní role a zajistit tak podrobnější správu přístupu pro API Management entity. Například můžete vytvořit vlastní roli, která má přístup jen pro čtení do služby API Management, ale pouze oprávnění k zápisu pro jedno konkrétní rozhraní API. Další informace o vlastních rolích naleznete v tématu [vlastní role v Azure RBAC](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles). 

Když vytvoříte vlastní roli, je snazší začít s některou z předdefinovaných rolí. Upravit atributy, které se přidat **akce**, **NotActions**, nebo **AssignableScopes**a potom uložte změny jako novou roli. V následujícím příkladu začíná roli "Azure API Management Service čtečky" a vytvoří vlastní role s názvem "Editor rozhraní API kalkulačky." Vlastní roli můžete přiřadit konkrétní rozhraní API. V důsledku toho tato role má přístup pouze k toto rozhraní API. 

```
$role = Get-AzureRmRoleDefinition "API Management Service Reader Role"
$role.Id = $null
$role.Name = 'Calculator API Contributor'
$role.Description = 'Has read access to Contoso APIM instance and write access to the Calculator API.'
$role.Actions.Add('Microsoft.ApiManagement/service/apis/write')
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add('/subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.ApiManagement/service/<service name>/apis/<api ID>')
New-AzureRmRoleDefinition -Role $role
New-AzureRmRoleAssignment -ObjectId <object ID of the user account> -RoleDefinitionName 'Calculator API Contributor' -Scope '/subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.ApiManagement/service/<service name>/apis/<api ID>'
```

[Operace poskytovatele prostředků Azure Resource Manager](../active-directory/role-based-access-control-resource-provider-operations.md#microsoftapimanagement) článek obsahuje seznam oprávnění, která může na úroveň API Management.

## <a name="video"></a>Video


> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Role-Based-Access-Control-in-API-Management/player]
>
>

## <a name="next-steps"></a>Další kroky

Další informace o řízení přístupu na základě rolí v Azure, najdete v následujících článcích:
  * [Začínáme se správou přístupu na webu Azure Portal](https://azure.microsoft.com/documentation/articles/role-based-access-control-what-is/)
  * [Použití přiřazení rolí ke správě přístupu k prostředkům předplatného Azure](https://azure.microsoft.com/documentation/articles/role-based-access-control-what-is/)
  * [Vlastní role v Azure RBAC](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles)
  * [Operace zprostředkovatele prostředků v Azure Resource Manager](../active-directory/role-based-access-control-resource-provider-operations.md#microsoftapimanagement)

