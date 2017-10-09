---
title: "vytváření sestav aaaAccess - Azure RBAC | Microsoft Docs"
description: "Vygenerujte sestavu, že zobrazí všechny změny v přístupu tooyour předplatných Azure pomocí řízení přístupu na základě rolí přes hello posledních 90 dnů."
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
ms.assetid: 2bc68595-145e-4de3-8b71-3a21890d13d9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/17/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9ad85d3d8e66ce167032638a35e4afffb46d3892
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-access-report-for-role-based-access-control"></a>Vytvoření sestavy přístup k řízení přístupu na základě rolí
Vždy, když někdo uděluje nebo odvolá přístup v rámci vašich předplatných hello změny se budou protokolovat do Azure událostí. Můžete vytvořit toosee sestavy historie změn přístupu všechny změny pro hello posledních 90 dnů.

## <a name="create-a-report-with-azure-powershell"></a>Vytvoření sestavy pomocí prostředí Azure PowerShell
Sestava historie v prostředí PowerShell, použijte hello změnit toocreate přístup [Get-AzureRMAuthorizationChangeLog](/powershell/module/azurerm.resources/get-azurermauthorizationchangelog) příkaz.

Při volání tento příkaz můžete určit kterou vlastnost tohoto přiřazení hello uveden v seznamu, včetně hello následující:

| Vlastnost | Popis |
| --- | --- |
| **Akce** |Jestli byl přístup udělen nebo odebrán |
| **Volající** |změnit vlastníka Hello zodpovědná za přístup hello |
| **PrincipalId** | Jedinečný identifikátor Hello hello uživatele, skupiny nebo aplikace, kterému byla přiřazena hello role |
| **PrincipalName** |Název Hello hello uživatele, skupiny nebo aplikace |
| **PrincipalType** |Jestli hello přiřazení byla pro uživatele, skupiny nebo aplikace |
| **Hodnoty vlastnosti RoleDefinitionId** |Hello GUID hello role, který byl udělen nebo odebrán |
| **RoleName** |Hello role, který byl udělen nebo odebrán |
| **Rozsah** | Jedinečný identifikátor Hello hello předplatné, skupinu prostředků nebo prostředek, který hello přiřazení platí příliš| 
| **ScopeName** |Název Hello hello předplatné, skupinu prostředků nebo prostředek |
| **ScopeType** |Jestli hello přiřazení byl v hello předplatné, skupinu prostředků nebo prostředek oboru |
| **Časové razítko** |Hello datum a čas, která byla změněna přístup |

Příkaz v tomto příkladu jsou uvedeny všechny změny v přístupu v hello předplatné pro hello posledních sedmi dnech:

```
Get-AzureRMAuthorizationChangeLog -StartTime ([DateTime]::Now - [TimeSpan]::FromDays(7)) | FT Caller,Action,RoleName,PrincipalType,PrincipalName,ScopeType,ScopeName
```

![Prostředí PowerShell Get-AzureRMAuthorizationChangeLog – snímek obrazovky](./media/role-based-access-control-configure/access-change-history.png)

## <a name="create-a-report-with-azure-cli"></a>Vytvoření sestavy pomocí rozhraní příkazového řádku Azure
toocreate sestavy historie změn přístupu v hello rozhraní příkazového řádku Azure (CLI), použijte hello `azure role assignment changelog list` příkaz.

## <a name="export-tooa-spreadsheet"></a>Export tooa tabulka
toosave hello sestavy nebo pracovat s daty hello, export hello přístup změny do souboru CSV. Potom můžete zobrazit sestavy hello v tabulce ke kontrole.

![Protokol změn zobrazit jako tabulku – snímek obrazovky](./media/role-based-access-control-configure/change-history-spreadsheet.png)

## <a name="next-steps"></a>Další kroky
* Práce s [vlastní role v Azure RBAC](role-based-access-control-custom-roles.md)
* Zjistěte, jak toomanage [RBAC Azure pomocí prostředí powershell](role-based-access-control-manage-access-powershell.md)

