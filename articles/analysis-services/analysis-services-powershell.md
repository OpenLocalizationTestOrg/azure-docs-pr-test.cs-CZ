---
title: "Správa služby Azure Analysis Services pomocí prostředí PowerShell | Microsoft Docs"
description: "Správa Azure Analysis Services pomocí prostředí PowerShell."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: owend
ms.openlocfilehash: 95593053950f96a83e093c29516e9f66ebad53bf
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="manage-azure-analysis-services-with-powershell"></a><span data-ttu-id="6a9c6-103">Správa služby Azure Analysis Services pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="6a9c6-103">Manage Azure Analysis Services with PowerShell</span></span>

<span data-ttu-id="6a9c6-104">Tento článek popisuje rutiny prostředí PowerShell použít k provádění serveru Azure Analysis Services a úlohy správy databáze.</span><span class="sxs-lookup"><span data-stu-id="6a9c6-104">This article describes PowerShell cmdlets used to perform Azure Analysis Services server and database management tasks.</span></span> 

<span data-ttu-id="6a9c6-105">Úlohy správy serveru jako je například vytváření nebo odstraňování serveru, pozastavení nebo obnovení operací serveru nebo změna úrovně služeb (vrstvy) použít rutiny Azure Resource Manager (AzureRM).</span><span class="sxs-lookup"><span data-stu-id="6a9c6-105">Server management tasks such as creating or deleting a server, suspending or resuming server operations, or changing the service level (tier) use Azure Resource Manager (AzureRM) cmdlets.</span></span> <span data-ttu-id="6a9c6-106">Další úlohy pro správu databáze například přidáním nebo odebráním členy role zpracování nebo rozdělení do oddílů pomocí rutiny zahrnuté ve stejném modulu SqlServer jako SQL Server Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="6a9c6-106">Other tasks for managing databases such as adding or removing role members, processing, or partitioning use cmdlets included in the same SqlServer module as SQL Server Analysis Services.</span></span>

## <a name="permissions"></a><span data-ttu-id="6a9c6-107">Oprávnění</span><span class="sxs-lookup"><span data-stu-id="6a9c6-107">Permissions</span></span>
<span data-ttu-id="6a9c6-108">Většinu úloh prostředí PowerShell vyžadují, že abyste měli oprávnění správce na serveru služby Analysis Services, který spravujete.</span><span class="sxs-lookup"><span data-stu-id="6a9c6-108">Most PowerShell tasks require you have Admin privileges on the Analysis Services server you are managing.</span></span> <span data-ttu-id="6a9c6-109">Naplánované úlohy prostředí PowerShell jsou bezobslužné operace.</span><span class="sxs-lookup"><span data-stu-id="6a9c6-109">Scheduled PowerShell tasks are unattended operations.</span></span> <span data-ttu-id="6a9c6-110">Účet, který spouští Plánovač musí mít oprávnění správce na serveru služby Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="6a9c6-110">The account running the scheduler must have Admin privileges on the Analysis Services server.</span></span> 

<span data-ttu-id="6a9c6-111">Operace serveru pomocí rutin AzureRm, váš účet nebo účet spouštějící scheduler musíte také zařadit do roli vlastníka na zdroj v [řízení řízení přístupu (RBAC)](../active-directory/role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="6a9c6-111">For server operations using AzureRm cmdlets, your account or the account running scheduler must also belong to the Owner role for the resource in [Azure Role-Based Access Control (RBAC)](../active-directory/role-based-access-control-what-is.md).</span></span> 

## <a name="server-operations"></a><span data-ttu-id="6a9c6-112">Operace serveru</span><span class="sxs-lookup"><span data-stu-id="6a9c6-112">Server operations</span></span> 
<span data-ttu-id="6a9c6-113">Rutiny Azure Analysis Services jsou součástí [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices) součást modulu.</span><span class="sxs-lookup"><span data-stu-id="6a9c6-113">Azure Analysis Services cmdlets are included in the [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices) component module.</span></span> <span data-ttu-id="6a9c6-114">Instalaci moduly rutin AzureRM naleznete v tématu [rutiny Azure Resource Manager](/powershell/azure/overview) v galerii prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6a9c6-114">To install AzureRM cmdlet modules, see [Azure Resource Manager cmdlets](/powershell/azure/overview) in the PowerShell Gallery.</span></span>

|<span data-ttu-id="6a9c6-115">Rutina</span><span class="sxs-lookup"><span data-stu-id="6a9c6-115">Cmdlet</span></span>|<span data-ttu-id="6a9c6-116">Popis</span><span class="sxs-lookup"><span data-stu-id="6a9c6-116">Description</span></span>| 
|------------|-----------------| 
|[<span data-ttu-id="6a9c6-117">Export-AzureAnalysisServicesInstance</span><span class="sxs-lookup"><span data-stu-id="6a9c6-117">Export-AzureAnalysisServicesInstance</span></span>](/powershell/module/azurerm.analysisservices/export-azureanalysisservicesinstancelog)|<span data-ttu-id="6a9c6-118">Export protokolu do souboru.</span><span class="sxs-lookup"><span data-stu-id="6a9c6-118">Exports log to file.</span></span>| 
|[<span data-ttu-id="6a9c6-119">Get-AzureRmAnalysisServicesServer</span><span class="sxs-lookup"><span data-stu-id="6a9c6-119">Get-AzureRmAnalysisServicesServer</span></span>](/powershell/module/azurerm.analysisservices/get-azurermanalysisservicesserver)|<span data-ttu-id="6a9c6-120">Získá podrobnosti instance serveru.</span><span class="sxs-lookup"><span data-stu-id="6a9c6-120">Gets details of a server instance.</span></span>|  
|[<span data-ttu-id="6a9c6-121">Nové AzureRmAnalysisServicesServer</span><span class="sxs-lookup"><span data-stu-id="6a9c6-121">New-AzureRmAnalysisServicesServer</span></span>](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver)|<span data-ttu-id="6a9c6-122">Vytvoří instanci serveru.</span><span class="sxs-lookup"><span data-stu-id="6a9c6-122">Creates a server instance.</span></span>|
|[<span data-ttu-id="6a9c6-123">Odebrat AzureRmAnalysisServicesServer</span><span class="sxs-lookup"><span data-stu-id="6a9c6-123">Remove-AzureRmAnalysisServicesServer</span></span>](/powershell/module/azurerm.analysisservices/remove-azurermanalysisservicesserver)|<span data-ttu-id="6a9c6-124">Odebere instanci serveru.</span><span class="sxs-lookup"><span data-stu-id="6a9c6-124">Removes a server instance.</span></span>|  
|[<span data-ttu-id="6a9c6-125">Pozastavit AzureRmAnalysisServicesServer</span><span class="sxs-lookup"><span data-stu-id="6a9c6-125">Suspend-AzureRmAnalysisServicesServer</span></span>](/powershell/module/azurerm.analysisservices/suspend-azurermanalysisservicesserver)|<span data-ttu-id="6a9c6-126">Pozastaví instanci serveru.</span><span class="sxs-lookup"><span data-stu-id="6a9c6-126">Suspends a server instance.</span></span>| 
|[<span data-ttu-id="6a9c6-127">Resume-AzureRmAnalysisServicesServer</span><span class="sxs-lookup"><span data-stu-id="6a9c6-127">Resume-AzureRmAnalysisServicesServer</span></span>](/powershell/module/azurerm.analysisservices/resume-azurermanalysisservicesserver)|<span data-ttu-id="6a9c6-128">Obnoví instanci serveru.</span><span class="sxs-lookup"><span data-stu-id="6a9c6-128">Resumes a server instance.</span></span>|  
|[<span data-ttu-id="6a9c6-129">Set-AzureRmAnalysisServicesServer</span><span class="sxs-lookup"><span data-stu-id="6a9c6-129">Set-AzureRmAnalysisServicesServer</span></span>](/powershell/module/azurerm.analysisservices/set-azurermanalysisservicesserver)|<span data-ttu-id="6a9c6-130">Upravuje instanci serveru.</span><span class="sxs-lookup"><span data-stu-id="6a9c6-130">Modifies a server instance.</span></span>|   
|[<span data-ttu-id="6a9c6-131">Test AzureRmAnalysisServicesServer</span><span class="sxs-lookup"><span data-stu-id="6a9c6-131">Test-AzureRmAnalysisServicesServer</span></span>](/powershell/module/azurerm.analysisservices/test-azurermanalysisservicesserver)|<span data-ttu-id="6a9c6-132">Testy existence instance serveru.</span><span class="sxs-lookup"><span data-stu-id="6a9c6-132">Tests the existence of a server  instance.</span></span>| 

## <a name="database-operations"></a><span data-ttu-id="6a9c6-133">Operace databáze</span><span class="sxs-lookup"><span data-stu-id="6a9c6-133">Database operations</span></span>

<span data-ttu-id="6a9c6-134">Operace databáze služby Analysis Services v Azure používají stejné [SqlServer](https://www.powershellgallery.com/packages/SqlServer) modulu jako SQL Server Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="6a9c6-134">Azure Analysis Services database operations use the same [SqlServer](https://www.powershellgallery.com/packages/SqlServer) module as SQL Server Analysis Services.</span></span> <span data-ttu-id="6a9c6-135">Ne všechny rutiny jsou však podporovány pro Azure Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="6a9c6-135">However, not all cmdlets are supported for Azure Analysis Services.</span></span> 

<span data-ttu-id="6a9c6-136">Modul SQL Server poskytuje rutiny správy specifických úkolů databáze a také obecné účely Invoke-ASCmd rutiny, která přijímá dotazu tabulkový Model skriptovací jazyk (TMSL) nebo skriptu.</span><span class="sxs-lookup"><span data-stu-id="6a9c6-136">The SqlServer module provides task-specific database management cmdlets as well as the general purpose Invoke-ASCmd cmdlet that accepts a Tabular Model Scripting Language (TMSL) query or script.</span></span> <span data-ttu-id="6a9c6-137">Následující rutiny v modulu SQL Server jsou podporovány pro Azure Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="6a9c6-137">The following cmdlets in the SqlServer module are supported for Azure Analysis Services.</span></span>

  
|<span data-ttu-id="6a9c6-138">Rutina</span><span class="sxs-lookup"><span data-stu-id="6a9c6-138">Cmdlet</span></span>|<span data-ttu-id="6a9c6-139">Popis</span><span class="sxs-lookup"><span data-stu-id="6a9c6-139">Description</span></span>|
|------------|-----------------| 
|[<span data-ttu-id="6a9c6-140">Přidat RoleMember</span><span class="sxs-lookup"><span data-stu-id="6a9c6-140">Add-RoleMember</span></span>](https://msdn.microsoft.com/library/hh510167.aspx)|<span data-ttu-id="6a9c6-141">Přidáte člena do role databáze.</span><span class="sxs-lookup"><span data-stu-id="6a9c6-141">Add a member to a database role.</span></span>| 
|[<span data-ttu-id="6a9c6-142">Zálohování ASDatabase</span><span class="sxs-lookup"><span data-stu-id="6a9c6-142">Backup-ASDatabase</span></span>](https://docs.microsoft.com/sql/analysis-services/powershell/backup-asdatabase-cmdlet)|<span data-ttu-id="6a9c6-143">Zálohování databáze služby Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="6a9c6-143">Backup an Analysis Services database.</span></span>|  
|[<span data-ttu-id="6a9c6-144">Odebrat RoleMember</span><span class="sxs-lookup"><span data-stu-id="6a9c6-144">Remove-RoleMember</span></span>](https://msdn.microsoft.com/library/hh510173.aspx)|<span data-ttu-id="6a9c6-145">Odebrání člena z databázové role.</span><span class="sxs-lookup"><span data-stu-id="6a9c6-145">Remove a member from a database role.</span></span>|   
|[<span data-ttu-id="6a9c6-146">Vyvolání ASCmd</span><span class="sxs-lookup"><span data-stu-id="6a9c6-146">Invoke-ASCmd</span></span>](https://msdn.microsoft.com/library/hh479579.aspx)|<span data-ttu-id="6a9c6-147">Spuštění skriptu TMSL.</span><span class="sxs-lookup"><span data-stu-id="6a9c6-147">Execute a TMSL script.</span></span>|
|[<span data-ttu-id="6a9c6-148">Vyvolání ProcessASDatabase</span><span class="sxs-lookup"><span data-stu-id="6a9c6-148">Invoke-ProcessASDatabase</span></span>](https://msdn.microsoft.com/library/mt651773.aspx)|<span data-ttu-id="6a9c6-149">Zpracování databáze.</span><span class="sxs-lookup"><span data-stu-id="6a9c6-149">Process a database.</span></span>|  
|[<span data-ttu-id="6a9c6-150">Vyvolání ProcessPartition</span><span class="sxs-lookup"><span data-stu-id="6a9c6-150">Invoke-ProcessPartition</span></span>](https://msdn.microsoft.com/library/hh510164.aspx)|<span data-ttu-id="6a9c6-151">Zpracování oddílu.</span><span class="sxs-lookup"><span data-stu-id="6a9c6-151">Process a partition.</span></span>| 
|[<span data-ttu-id="6a9c6-152">Vyvolání ProcessTable</span><span class="sxs-lookup"><span data-stu-id="6a9c6-152">Invoke-ProcessTable</span></span>](https://msdn.microsoft.com/library/mt651774.aspx)|<span data-ttu-id="6a9c6-153">Proces tabulku.</span><span class="sxs-lookup"><span data-stu-id="6a9c6-153">Process a table.</span></span>|  
|[<span data-ttu-id="6a9c6-154">Sloučení oddílů</span><span class="sxs-lookup"><span data-stu-id="6a9c6-154">Merge-Partition</span></span>](https://msdn.microsoft.com/library/hh479576.aspx)|<span data-ttu-id="6a9c6-155">Sloučit oddíl.</span><span class="sxs-lookup"><span data-stu-id="6a9c6-155">Merge a partition.</span></span>|  
|[<span data-ttu-id="6a9c6-156">Obnovení ASDatabase</span><span class="sxs-lookup"><span data-stu-id="6a9c6-156">Restore-ASDatabase</span></span>](https://docs.microsoft.com/sql/analysis-services/powershell/restore-asdatabase-cmdlet)|<span data-ttu-id="6a9c6-157">Obnovte databázi služby Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="6a9c6-157">Restore an Analysis Services database.</span></span>| 
  

## <a name="related-information"></a><span data-ttu-id="6a9c6-158">Související informace</span><span class="sxs-lookup"><span data-stu-id="6a9c6-158">Related information</span></span>

* [<span data-ttu-id="6a9c6-159">Stáhnout modul prostředí PowerShell serveru SQL</span><span class="sxs-lookup"><span data-stu-id="6a9c6-159">Download SQL Server PowerShell Module</span></span>](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [<span data-ttu-id="6a9c6-160">Stažení aplikace SSMS</span><span class="sxs-lookup"><span data-stu-id="6a9c6-160">Download SSMS</span></span>](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   
* [<span data-ttu-id="6a9c6-161">Modul SQL Server v galerii prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="6a9c6-161">SqlServer module in PowerShell Gallery</span></span>](https://www.powershellgallery.com/packages/SqlServer)    
* [<span data-ttu-id="6a9c6-162">Tabulkový Model programování pro 1200 úroveň kompatibility a vyšší</span><span class="sxs-lookup"><span data-stu-id="6a9c6-162">Tabular Model Programming for Compatibility Level 1200 and higher</span></span>](https://msdn.microsoft.com/library/mt712541.aspx)