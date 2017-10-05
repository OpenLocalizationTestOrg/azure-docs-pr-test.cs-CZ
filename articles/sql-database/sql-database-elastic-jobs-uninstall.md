---
title: "Tom, jak odinstalovat nástroj úlohy elastické databáze"
description: "Postup odinstalace nástroje úlohy elastické databáze"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: bfc9d820-edbd-4fca-bfbf-1f339cfcc448
ms.service: sql-database
ms.workload: sql-database
ms.custom: scale out apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: ae7f0bce452a0a86f6f1e4d9b0c93a0fa1727f21
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="uninstall-elastic-database-jobs-components"></a><span data-ttu-id="8eea3-103">Odinstalace součástí úlohy elastické databáze</span><span class="sxs-lookup"><span data-stu-id="8eea3-103">Uninstall Elastic Database jobs components</span></span>
<span data-ttu-id="8eea3-104">**Elastické databáze úlohy** součásti lze odinstalovat pomocí portálu nebo prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8eea3-104">**Elastic Database jobs** components can be uninstalled using either the Portal or PowerShell.</span></span>

## <a name="uninstall-elastic-database-jobs-components-using-the-azure-portal"></a><span data-ttu-id="8eea3-105">Odinstalace součástí úlohy elastické databáze pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="8eea3-105">Uninstall Elastic Database jobs components using the Azure portal</span></span>
1. <span data-ttu-id="8eea3-106">Otevřete web [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="8eea3-106">Open the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="8eea3-107">Přejděte na předplatné, které obsahuje **úlohy elastické databáze** součásti, konkrétně předplatné, ve které elastické databáze součásti úlohy nebyly nainstalovány.</span><span class="sxs-lookup"><span data-stu-id="8eea3-107">Navigate to the subscription that contains **Elastic Database jobs** components, namely the subscription in which Elastic Database jobs components were installed.</span></span>
3. <span data-ttu-id="8eea3-108">Klikněte na tlačítko **Procházet** a klikněte na tlačítko **skupiny prostředků**.</span><span class="sxs-lookup"><span data-stu-id="8eea3-108">Click **Browse** and click **Resource groups**.</span></span>
4. <span data-ttu-id="8eea3-109">Vyberte skupinu prostředků s názvem "__ElasticDatabaseJob".</span><span class="sxs-lookup"><span data-stu-id="8eea3-109">Select the resource group named "__ElasticDatabaseJob".</span></span>
5. <span data-ttu-id="8eea3-110">Odstraňte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="8eea3-110">Delete the resource group.</span></span>

## <a name="uninstall--elastic-database-jobs-components-using-powershell"></a><span data-ttu-id="8eea3-111">Odinstalace součástí úlohy elastické databáze pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="8eea3-111">Uninstall  Elastic Database jobs components using PowerShell</span></span>
1. <span data-ttu-id="8eea3-112">Spusťte příkazové okno Microsoft Azure PowerShell a přejděte na nástroje podadresář ve složce Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x: typ **cd nástroje**.</span><span class="sxs-lookup"><span data-stu-id="8eea3-112">Launch a Microsoft Azure PowerShell command window and navigate to the tools sub-directory under the Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x folder: Type **cd tools**.</span></span>
   
     <span data-ttu-id="8eea3-113">PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x* > disk cd nástroje</span><span class="sxs-lookup"><span data-stu-id="8eea3-113">PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools</span></span>
2. <span data-ttu-id="8eea3-114">Spusťte.\UninstallElasticDatabaseJobs.ps1 skript prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8eea3-114">Execute the .\UninstallElasticDatabaseJobs.ps1 PowerShell script.</span></span>
   
     <span data-ttu-id="8eea3-115">PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools > odblokovat soubor.\UninstallElasticDatabaseJobs.ps1 PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools >.\UninstallElasticDatabaseJobs.ps1</span><span class="sxs-lookup"><span data-stu-id="8eea3-115">PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\UninstallElasticDatabaseJobs.ps1   PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\UninstallElasticDatabaseJobs.ps1</span></span>

<span data-ttu-id="8eea3-116">Nebo jednoduše, spusťte následující skript, za předpokladu, že výchozí hodnoty, kde je použít pro instalaci součástí:</span><span class="sxs-lookup"><span data-stu-id="8eea3-116">Or simply, execute the following script, assuming default values where used on installation of the components:</span></span>

        $ResourceGroupName = "__ElasticDatabaseJob"
        Switch-AzureMode AzureResourceManager

        $resourceGroup = Get-AzureResourceGroup -Name $ResourceGroupName
        if(!$resourceGroup)
        {
            Write-Host "The Azure Resource Group: $ResourceGroupName has already been deleted.  Elastic database job components are uninstalled."
            return
        }

        Write-Host "Removing the Azure Resource Group: $ResourceGroupName.  This may take a few minutes.”
        Remove-AzureResourceGroup -Name $ResourceGroupName -Force
        Write-Host "Completed removing the Azure Resource Group: $ResourceGroupName.  Elastic database job compoennts are now uninstalled."

## <a name="next-steps"></a><span data-ttu-id="8eea3-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8eea3-117">Next steps</span></span>
<span data-ttu-id="8eea3-118">Pokud chcete znovu nainstalovat úlohy elastické databáze, přečtěte si téma [nainstalovat službu úlohy elastické databáze](sql-database-elastic-jobs-service-installation.md)</span><span class="sxs-lookup"><span data-stu-id="8eea3-118">To re-install Elastic Database jobs, see [Installing the Elastic Database job service](sql-database-elastic-jobs-service-installation.md)</span></span>

<span data-ttu-id="8eea3-119">Přehled úlohy elastické databáze najdete v tématu [přehled úlohy elastické databáze](sql-database-elastic-jobs-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8eea3-119">For an overview of Elastic Database jobs, see [Elastic Database jobs overview](sql-database-elastic-jobs-overview.md).</span></span>

<!--Image references-->


