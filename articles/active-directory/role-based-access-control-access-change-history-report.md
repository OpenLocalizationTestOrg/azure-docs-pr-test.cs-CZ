---
title: "Přístup k vytváření sestav - Azure RBAC | Microsoft Docs"
description: "Vygenerujte sestavu, která uvádí všechny změny v přístupu k vašemu předplatnému Azure pomocí řízení přístupu na základě rolí za posledních 90 dnů."
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
ms.openlocfilehash: 4e8028ab43ed02ef0c0a1374326b07f72f97d9d9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-an-access-report-for-role-based-access-control"></a><span data-ttu-id="c692a-103">Vytvoření sestavy přístup k řízení přístupu na základě rolí</span><span class="sxs-lookup"><span data-stu-id="c692a-103">Create an access report for Role-Based Access Control</span></span>
<span data-ttu-id="c692a-104">Vždy, když někdo uděluje nebo odvolá přístup v rámci vašich předplatných, změny se budou protokolovat do Azure událostí.</span><span class="sxs-lookup"><span data-stu-id="c692a-104">Any time someone grants or revokes access within your subscriptions, the changes get logged in Azure events.</span></span> <span data-ttu-id="c692a-105">Můžete vytvořit sestavy historie změn přístupu zobrazíte všechny změny za posledních 90 dnů.</span><span class="sxs-lookup"><span data-stu-id="c692a-105">You can create access change history reports to see all changes for the past 90 days.</span></span>

## <a name="create-a-report-with-azure-powershell"></a><span data-ttu-id="c692a-106">Vytvoření sestavy pomocí prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="c692a-106">Create a report with Azure PowerShell</span></span>
<span data-ttu-id="c692a-107">K vytvoření sestavy historie změn přístupu v prostředí PowerShell, použijte [Get-AzureRMAuthorizationChangeLog](/powershell/module/azurerm.resources/get-azurermauthorizationchangelog) příkaz.</span><span class="sxs-lookup"><span data-stu-id="c692a-107">To create an access change history report in PowerShell, use the [Get-AzureRMAuthorizationChangeLog](/powershell/module/azurerm.resources/get-azurermauthorizationchangelog) command.</span></span>

<span data-ttu-id="c692a-108">Při volání tento příkaz můžete určit kterou vlastnost tohoto přiřazení uveden v seznamu, včetně následujících:</span><span class="sxs-lookup"><span data-stu-id="c692a-108">When you call this command, you can specify which property of the assignments you want listed, including the following:</span></span>

| <span data-ttu-id="c692a-109">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="c692a-109">Property</span></span> | <span data-ttu-id="c692a-110">Popis</span><span class="sxs-lookup"><span data-stu-id="c692a-110">Description</span></span> |
| --- | --- |
| <span data-ttu-id="c692a-111">**Akce**</span><span class="sxs-lookup"><span data-stu-id="c692a-111">**Action**</span></span> |<span data-ttu-id="c692a-112">Jestli byl přístup udělen nebo odebrán</span><span class="sxs-lookup"><span data-stu-id="c692a-112">Whether access was granted or revoked</span></span> |
| <span data-ttu-id="c692a-113">**Volající**</span><span class="sxs-lookup"><span data-stu-id="c692a-113">**Caller**</span></span> |<span data-ttu-id="c692a-114">Vlastník zodpovědná za změnu přístup</span><span class="sxs-lookup"><span data-stu-id="c692a-114">The owner responsible for the access change</span></span> |
| <span data-ttu-id="c692a-115">**PrincipalId**</span><span class="sxs-lookup"><span data-stu-id="c692a-115">**PrincipalId**</span></span> | <span data-ttu-id="c692a-116">Jedinečný identifikátor uživatele, skupiny nebo aplikace, kterému byla přiřazena role</span><span class="sxs-lookup"><span data-stu-id="c692a-116">The unique identifier of the user, group, or application that was assigned the role</span></span> |
| <span data-ttu-id="c692a-117">**PrincipalName**</span><span class="sxs-lookup"><span data-stu-id="c692a-117">**PrincipalName**</span></span> |<span data-ttu-id="c692a-118">Jméno uživatele, skupiny nebo aplikace</span><span class="sxs-lookup"><span data-stu-id="c692a-118">The name of the user, group, or application</span></span> |
| <span data-ttu-id="c692a-119">**PrincipalType**</span><span class="sxs-lookup"><span data-stu-id="c692a-119">**PrincipalType**</span></span> |<span data-ttu-id="c692a-120">Jestli přiřazení byla pro uživatele, skupiny nebo aplikace</span><span class="sxs-lookup"><span data-stu-id="c692a-120">Whether the assignment was for a user, group, or application</span></span> |
| <span data-ttu-id="c692a-121">**Hodnoty vlastnosti RoleDefinitionId**</span><span class="sxs-lookup"><span data-stu-id="c692a-121">**RoleDefinitionId**</span></span> |<span data-ttu-id="c692a-122">Identifikátor GUID role, který byl udělen nebo odebrán</span><span class="sxs-lookup"><span data-stu-id="c692a-122">The GUID of the role that was granted or revoked</span></span> |
| <span data-ttu-id="c692a-123">**RoleName**</span><span class="sxs-lookup"><span data-stu-id="c692a-123">**RoleName**</span></span> |<span data-ttu-id="c692a-124">Role, který byl udělen nebo odebrán</span><span class="sxs-lookup"><span data-stu-id="c692a-124">The role that was granted or revoked</span></span> |
| <span data-ttu-id="c692a-125">**Rozsah**</span><span class="sxs-lookup"><span data-stu-id="c692a-125">**Scope**</span></span> | <span data-ttu-id="c692a-126">Jedinečný identifikátor předplatné, skupinu prostředků nebo prostředek, který se vztahuje na přiřazení</span><span class="sxs-lookup"><span data-stu-id="c692a-126">The unique identifier of the subscription, resource group, or resource that the assignment applies to</span></span> | 
| <span data-ttu-id="c692a-127">**ScopeName**</span><span class="sxs-lookup"><span data-stu-id="c692a-127">**ScopeName**</span></span> |<span data-ttu-id="c692a-128">Název předplatné, skupinu prostředků nebo prostředek</span><span class="sxs-lookup"><span data-stu-id="c692a-128">The name of the subscription, resource group, or resource</span></span> |
| <span data-ttu-id="c692a-129">**ScopeType**</span><span class="sxs-lookup"><span data-stu-id="c692a-129">**ScopeType**</span></span> |<span data-ttu-id="c692a-130">Jestli přiřazení byl v předplatné, skupinu prostředků nebo prostředek oboru</span><span class="sxs-lookup"><span data-stu-id="c692a-130">Whether the assignment was at the subscription, resource group, or resource scope</span></span> |
| <span data-ttu-id="c692a-131">**Časové razítko**</span><span class="sxs-lookup"><span data-stu-id="c692a-131">**Timestamp**</span></span> |<span data-ttu-id="c692a-132">Datum a čas, která byla změněna přístup</span><span class="sxs-lookup"><span data-stu-id="c692a-132">The date and time that access was changed</span></span> |

<span data-ttu-id="c692a-133">Příkaz v tomto příkladu jsou uvedeny všechny změny v přístupu v rámci předplatného pro posledních sedmi dnech:</span><span class="sxs-lookup"><span data-stu-id="c692a-133">This example command lists all access changes in the subscription for the past seven days:</span></span>

```
Get-AzureRMAuthorizationChangeLog -StartTime ([DateTime]::Now - [TimeSpan]::FromDays(7)) | FT Caller,Action,RoleName,PrincipalType,PrincipalName,ScopeType,ScopeName
```

![Prostředí PowerShell Get-AzureRMAuthorizationChangeLog – snímek obrazovky](./media/role-based-access-control-configure/access-change-history.png)

## <a name="create-a-report-with-azure-cli"></a><span data-ttu-id="c692a-135">Vytvoření sestavy pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="c692a-135">Create a report with Azure CLI</span></span>
<span data-ttu-id="c692a-136">K vytvoření sestavy historie změn přístupu v rozhraní příkazového řádku Azure (CLI), použijte `azure role assignment changelog list` příkaz.</span><span class="sxs-lookup"><span data-stu-id="c692a-136">To create an access change history report in the Azure command-line interface (CLI), use the `azure role assignment changelog list` command.</span></span>

## <a name="export-to-a-spreadsheet"></a><span data-ttu-id="c692a-137">Export do tabulky</span><span class="sxs-lookup"><span data-stu-id="c692a-137">Export to a spreadsheet</span></span>
<span data-ttu-id="c692a-138">Pokud chcete sestavu uložit nebo manipulovat s daty, exportujte přístup změny do souboru CSV.</span><span class="sxs-lookup"><span data-stu-id="c692a-138">To save the report, or manipulate the data, export the access changes into a .csv file.</span></span> <span data-ttu-id="c692a-139">Potom můžete zobrazit zprávu v tabulce ke kontrole.</span><span class="sxs-lookup"><span data-stu-id="c692a-139">You can then view the report in a spreadsheet for review.</span></span>

![Protokol změn zobrazit jako tabulku – snímek obrazovky](./media/role-based-access-control-configure/change-history-spreadsheet.png)

## <a name="next-steps"></a><span data-ttu-id="c692a-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c692a-141">Next steps</span></span>
* <span data-ttu-id="c692a-142">Práce s [vlastní role v Azure RBAC](role-based-access-control-custom-roles.md)</span><span class="sxs-lookup"><span data-stu-id="c692a-142">Work with [Custom roles in Azure RBAC](role-based-access-control-custom-roles.md)</span></span>
* <span data-ttu-id="c692a-143">Naučte se spravovat [RBAC Azure pomocí prostředí powershell](role-based-access-control-manage-access-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="c692a-143">Learn how to manage [Azure RBAC with powershell](role-based-access-control-manage-access-powershell.md)</span></span>

