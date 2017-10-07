---
title: "aaaHow tooUse řízení přístupu na základě rolí v Azure API Management | Microsoft Docs"
description: "Zjistěte, jak toouse hello předdefinované role a vytvářet vlastní role v Azure API Management"
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 364cd53e-88fb-4301-a093-f132fa1f88f5
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2017
ms.author: apimpm
ms.openlocfilehash: c51da2ff6886ebcaf796022e3a759c67f36670a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-role-based-access-control-in-azure-api-management"></a>Jak tooUse na základě rolí řízení přístupu ve službě Azure API Management
Azure API Management spoléhá na řízení řízení přístupu (RBAC) tooenable vyladění správy přístupu pro služby API Management a entit (například rozhraní API, zásady). Tento článek nabízí přehled hello předdefinované a vlastní rolí ve službě API Management. Pokud chcete další informace o řízení přístupu v hello portálu Azure, najdete v části [Začínáme se správou přístupu v hello portálu Azure](https://azure.microsoft.com/en-us/documentation/articles/role-based-access-control-what-is/)

## <a name="built-in-roles"></a>Předdefinované role
API Management aktuálně poskytuje 3 předdefinovaných rolí a přidá 2 více rolí v blízké budoucnosti hello. Tyto role mohou být přiřazeny na různých místech včetně předplatné, skupinu prostředků a jednotlivé instance služby API Management. Například pokud hello "Azure API Management Service čtečky" role je přiřazená tooan uživatel na úrovni skupiny prostředků hello, pak hello uživatele bude mít přístup pro čtení instance API Management tooall uvnitř skupiny prostředků hello. 

Hello následující tabulka obsahuje stručný popis hello předdefinované role. Tyto role pomocí hello portálu Azure nebo jiných nástrojů, včetně Azure můžete přiřadit [prostředí PowerShell](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-manage-access-powershell)Azure [rozhraní příkazového řádku](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-manage-access-azure-cli), a [REST API](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-manage-access-rest). Podrobnosti o tooassign předdefinovaných rolí, najdete v části [používat roli přiřazení toomanage přístup tooyour předplatného Azure prostředky](https://azure.microsoft.com/en-us/documentation/articles/role-based-access-control-what-is/).

| Role          | Přístup pro čtení<sup>[1]</sup> | Přístup pro zápis<sup>[2]</sup> | Vytvoření služby, odstranění, změny velikosti, VPN a konfigurace vlastní domény | Přístup k toolegacy Publsiher portálu | Popis
| ------------- | ---- | ---- | ---- | ---- | ---- | ---- |
| Přispěvatel služby Azure API Management | ✓ | ✓ | ✓ | ✓ | Superuživatele. Má úplná CRUD přístup tooAPI Správa služby a entit (například rozhraní API, zásady). Má portál starší verze vydavatele toohello přístup. |
| Čtecí modul služby Azure API Management | ✓ | | || Má oprávnění jen pro čtení tooAPI Správa služby a entity. |
| Operátor služby Azure API Management | ✓ | | ✓ | | Můžete spravovat služby API Management, ale není entity.|
| Azure API Management Service editoru<sup>*</sup> | ✓ | ✓ | |  | Můžete spravovat, ale není služby API Management entity.|
| Azure API Management Content Manager<sup>*</sup> | ✓ | | | ✓ | Můžete spravovat portál pro vývojáře. Tooservices oprávnění jen pro čtení a entity.|

<sup>[1] přístup pro čtení tooAPI Správa služby a entit (například rozhraní API, zásady)</sup>

<sup>[2] oprávnění k zápisu tooAPI Správa služby a entity s výjimkou opeartions následující: 1) Instance vytvoření, odstranění a škálování nastavení název domény 3) vlastní konfigurace sítě VPN 2)</sup>

<sup>\*role služby Editor Hello bude k dispozici po migraci jsme všechny správce uživatelského rozhraní ze hello, existující vydavatele, portálu, toohello portálu Azure. Hello role správce obsahu bude k dispozici po hello portálu vydavatele je teď vyčleněný tooonly obsahovat portál pro vývojáře funkce související toomanaging hello.</sup>  


## <a name="custom-roles"></a>Vlastní role
Pokud žádná z předdefinovaných rolí hello splňovat specifické potřeby, vlastní role lze vytvořit tooprovide podrobnější access management pro API Management entity. Například můžete vytvořit vlastní roli, který má oprávnění jen pro čtení tooan služba API Management, ale má pouze oprávnění k zápisu tooone konkrétní API. Podrobnosti o vlastních rolích toolearn najdete v části [vlastní role v Azure RBAC](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-custom-roles). 

Když vytvoříte vlastní roli, je snazší toostart s některou z předdefinovaných rolí hello. Upravit atributy tooadd hello hello akce, NotActions nebo AssignableScopes a potom hello změny uložit jako novou roli. Hello následující příklad začíná textem hello "Azure API Management Service čtečky" role a vytvoří vlastní role s názvem "Editor rozhraní API kalkulačky". vlastní role Hello lze přiřadit pouze tooa, které konkrétní API proto bude pouze má toothat přístup k rozhraní API. 

```
$role = Get-AzureRmRoleDefinition "API Management Service Reader Role"
$role.Id = $null
$role.Name = 'Calculator API Contributor'
$role.Description = 'Has read access tooContoso APIM instance and write access toohello Calculator API.'
$role.Actions.Add('Microsoft.ApiManagement/service/apis/write')
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add('/subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.ApiManagement/service/<service name>/apis/<api ID>')
New-AzureRmRoleDefinition -Role $role
New-AzureRmRoleAssignment -ObjectId <object ID of hello user account> -RoleDefinitionName 'Calculator API Contributor' -Scope '/subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.ApiManagement/service/<service name>/apis/<api ID>'
```

## <a name="watch-a-video-overview"></a>Podívejte se na Video s přehledem

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Role-Based-Access-Control-in-API-Management/player]
> 
> 

## <a name="next-steps"></a>Další kroky

* Další informace o řízení přístupu na základě rolí v Azure
  * [Začínáme se správou přístupu v hello portálu Azure](https://azure.microsoft.com/en-us/documentation/articles/role-based-access-control-what-is/)
  * [Používat roli přiřazení toomanage přístup tooyour předplatného Azure prostředky](https://azure.microsoft.com/en-us/documentation/articles/role-based-access-control-what-is/)
  * [Vlastní role v Azure RBAC](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-custom-roles)
