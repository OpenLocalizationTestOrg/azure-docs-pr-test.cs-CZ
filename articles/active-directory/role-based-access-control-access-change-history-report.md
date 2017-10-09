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
# <a name="create-an-access-report-for-role-based-access-control"></a><span data-ttu-id="7caa3-103">Vytvoření sestavy přístup k řízení přístupu na základě rolí</span><span class="sxs-lookup"><span data-stu-id="7caa3-103">Create an access report for Role-Based Access Control</span></span>
<span data-ttu-id="7caa3-104">Vždy, když někdo uděluje nebo odvolá přístup v rámci vašich předplatných hello změny se budou protokolovat do Azure událostí.</span><span class="sxs-lookup"><span data-stu-id="7caa3-104">Any time someone grants or revokes access within your subscriptions, hello changes get logged in Azure events.</span></span> <span data-ttu-id="7caa3-105">Můžete vytvořit toosee sestavy historie změn přístupu všechny změny pro hello posledních 90 dnů.</span><span class="sxs-lookup"><span data-stu-id="7caa3-105">You can create access change history reports toosee all changes for hello past 90 days.</span></span>

## <a name="create-a-report-with-azure-powershell"></a><span data-ttu-id="7caa3-106">Vytvoření sestavy pomocí prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="7caa3-106">Create a report with Azure PowerShell</span></span>
<span data-ttu-id="7caa3-107">Sestava historie v prostředí PowerShell, použijte hello změnit toocreate přístup [Get-AzureRMAuthorizationChangeLog](/powershell/module/azurerm.resources/get-azurermauthorizationchangelog) příkaz.</span><span class="sxs-lookup"><span data-stu-id="7caa3-107">toocreate an access change history report in PowerShell, use hello [Get-AzureRMAuthorizationChangeLog](/powershell/module/azurerm.resources/get-azurermauthorizationchangelog) command.</span></span>

<span data-ttu-id="7caa3-108">Při volání tento příkaz můžete určit kterou vlastnost tohoto přiřazení hello uveden v seznamu, včetně hello následující:</span><span class="sxs-lookup"><span data-stu-id="7caa3-108">When you call this command, you can specify which property of hello assignments you want listed, including hello following:</span></span>

| <span data-ttu-id="7caa3-109">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="7caa3-109">Property</span></span> | <span data-ttu-id="7caa3-110">Popis</span><span class="sxs-lookup"><span data-stu-id="7caa3-110">Description</span></span> |
| --- | --- |
| <span data-ttu-id="7caa3-111">**Akce**</span><span class="sxs-lookup"><span data-stu-id="7caa3-111">**Action**</span></span> |<span data-ttu-id="7caa3-112">Jestli byl přístup udělen nebo odebrán</span><span class="sxs-lookup"><span data-stu-id="7caa3-112">Whether access was granted or revoked</span></span> |
| <span data-ttu-id="7caa3-113">**Volající**</span><span class="sxs-lookup"><span data-stu-id="7caa3-113">**Caller**</span></span> |<span data-ttu-id="7caa3-114">změnit vlastníka Hello zodpovědná za přístup hello</span><span class="sxs-lookup"><span data-stu-id="7caa3-114">hello owner responsible for hello access change</span></span> |
| <span data-ttu-id="7caa3-115">**PrincipalId**</span><span class="sxs-lookup"><span data-stu-id="7caa3-115">**PrincipalId**</span></span> | <span data-ttu-id="7caa3-116">Jedinečný identifikátor Hello hello uživatele, skupiny nebo aplikace, kterému byla přiřazena hello role</span><span class="sxs-lookup"><span data-stu-id="7caa3-116">hello unique identifier of hello user, group, or application that was assigned hello role</span></span> |
| <span data-ttu-id="7caa3-117">**PrincipalName**</span><span class="sxs-lookup"><span data-stu-id="7caa3-117">**PrincipalName**</span></span> |<span data-ttu-id="7caa3-118">Název Hello hello uživatele, skupiny nebo aplikace</span><span class="sxs-lookup"><span data-stu-id="7caa3-118">hello name of hello user, group, or application</span></span> |
| <span data-ttu-id="7caa3-119">**PrincipalType**</span><span class="sxs-lookup"><span data-stu-id="7caa3-119">**PrincipalType**</span></span> |<span data-ttu-id="7caa3-120">Jestli hello přiřazení byla pro uživatele, skupiny nebo aplikace</span><span class="sxs-lookup"><span data-stu-id="7caa3-120">Whether hello assignment was for a user, group, or application</span></span> |
| <span data-ttu-id="7caa3-121">**Hodnoty vlastnosti RoleDefinitionId**</span><span class="sxs-lookup"><span data-stu-id="7caa3-121">**RoleDefinitionId**</span></span> |<span data-ttu-id="7caa3-122">Hello GUID hello role, který byl udělen nebo odebrán</span><span class="sxs-lookup"><span data-stu-id="7caa3-122">hello GUID of hello role that was granted or revoked</span></span> |
| <span data-ttu-id="7caa3-123">**RoleName**</span><span class="sxs-lookup"><span data-stu-id="7caa3-123">**RoleName**</span></span> |<span data-ttu-id="7caa3-124">Hello role, který byl udělen nebo odebrán</span><span class="sxs-lookup"><span data-stu-id="7caa3-124">hello role that was granted or revoked</span></span> |
| <span data-ttu-id="7caa3-125">**Rozsah**</span><span class="sxs-lookup"><span data-stu-id="7caa3-125">**Scope**</span></span> | <span data-ttu-id="7caa3-126">Jedinečný identifikátor Hello hello předplatné, skupinu prostředků nebo prostředek, který hello přiřazení platí příliš</span><span class="sxs-lookup"><span data-stu-id="7caa3-126">hello unique identifier of hello subscription, resource group, or resource that hello assignment applies too</span></span>| 
| <span data-ttu-id="7caa3-127">**ScopeName**</span><span class="sxs-lookup"><span data-stu-id="7caa3-127">**ScopeName**</span></span> |<span data-ttu-id="7caa3-128">Název Hello hello předplatné, skupinu prostředků nebo prostředek</span><span class="sxs-lookup"><span data-stu-id="7caa3-128">hello name of hello subscription, resource group, or resource</span></span> |
| <span data-ttu-id="7caa3-129">**ScopeType**</span><span class="sxs-lookup"><span data-stu-id="7caa3-129">**ScopeType**</span></span> |<span data-ttu-id="7caa3-130">Jestli hello přiřazení byl v hello předplatné, skupinu prostředků nebo prostředek oboru</span><span class="sxs-lookup"><span data-stu-id="7caa3-130">Whether hello assignment was at hello subscription, resource group, or resource scope</span></span> |
| <span data-ttu-id="7caa3-131">**Časové razítko**</span><span class="sxs-lookup"><span data-stu-id="7caa3-131">**Timestamp**</span></span> |<span data-ttu-id="7caa3-132">Hello datum a čas, která byla změněna přístup</span><span class="sxs-lookup"><span data-stu-id="7caa3-132">hello date and time that access was changed</span></span> |

<span data-ttu-id="7caa3-133">Příkaz v tomto příkladu jsou uvedeny všechny změny v přístupu v hello předplatné pro hello posledních sedmi dnech:</span><span class="sxs-lookup"><span data-stu-id="7caa3-133">This example command lists all access changes in hello subscription for hello past seven days:</span></span>

```
Get-AzureRMAuthorizationChangeLog -StartTime ([DateTime]::Now - [TimeSpan]::FromDays(7)) | FT Caller,Action,RoleName,PrincipalType,PrincipalName,ScopeType,ScopeName
```

![Prostředí PowerShell Get-AzureRMAuthorizationChangeLog – snímek obrazovky](./media/role-based-access-control-configure/access-change-history.png)

## <a name="create-a-report-with-azure-cli"></a><span data-ttu-id="7caa3-135">Vytvoření sestavy pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="7caa3-135">Create a report with Azure CLI</span></span>
<span data-ttu-id="7caa3-136">toocreate sestavy historie změn přístupu v hello rozhraní příkazového řádku Azure (CLI), použijte hello `azure role assignment changelog list` příkaz.</span><span class="sxs-lookup"><span data-stu-id="7caa3-136">toocreate an access change history report in hello Azure command-line interface (CLI), use hello `azure role assignment changelog list` command.</span></span>

## <a name="export-tooa-spreadsheet"></a><span data-ttu-id="7caa3-137">Export tooa tabulka</span><span class="sxs-lookup"><span data-stu-id="7caa3-137">Export tooa spreadsheet</span></span>
<span data-ttu-id="7caa3-138">toosave hello sestavy nebo pracovat s daty hello, export hello přístup změny do souboru CSV.</span><span class="sxs-lookup"><span data-stu-id="7caa3-138">toosave hello report, or manipulate hello data, export hello access changes into a .csv file.</span></span> <span data-ttu-id="7caa3-139">Potom můžete zobrazit sestavy hello v tabulce ke kontrole.</span><span class="sxs-lookup"><span data-stu-id="7caa3-139">You can then view hello report in a spreadsheet for review.</span></span>

![Protokol změn zobrazit jako tabulku – snímek obrazovky](./media/role-based-access-control-configure/change-history-spreadsheet.png)

## <a name="next-steps"></a><span data-ttu-id="7caa3-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7caa3-141">Next steps</span></span>
* <span data-ttu-id="7caa3-142">Práce s [vlastní role v Azure RBAC](role-based-access-control-custom-roles.md)</span><span class="sxs-lookup"><span data-stu-id="7caa3-142">Work with [Custom roles in Azure RBAC](role-based-access-control-custom-roles.md)</span></span>
* <span data-ttu-id="7caa3-143">Zjistěte, jak toomanage [RBAC Azure pomocí prostředí powershell](role-based-access-control-manage-access-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="7caa3-143">Learn how toomanage [Azure RBAC with powershell](role-based-access-control-manage-access-powershell.md)</span></span>

