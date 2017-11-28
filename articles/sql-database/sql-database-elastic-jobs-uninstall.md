---
title: "aaaHow toouninstall elastické databáze úlohy nástroje"
description: "Jak toouninstall hello nástroj úlohy elastické databáze"
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
ms.openlocfilehash: 3bc1e889d5042bc3eaa9fd9da89816737e0b8df8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="uninstall-elastic-database-jobs-components"></a><span data-ttu-id="2a940-103">Odinstalace součástí úlohy elastické databáze</span><span class="sxs-lookup"><span data-stu-id="2a940-103">Uninstall Elastic Database jobs components</span></span>
<span data-ttu-id="2a940-104">**Elastické databáze úlohy** součásti lze odinstalovat pomocí hello portál nebo prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2a940-104">**Elastic Database jobs** components can be uninstalled using either hello Portal or PowerShell.</span></span>

## <a name="uninstall-elastic-database-jobs-components-using-hello-azure-portal"></a><span data-ttu-id="2a940-105">Odinstalace součástí úlohy elastické databáze pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="2a940-105">Uninstall Elastic Database jobs components using hello Azure portal</span></span>
1. <span data-ttu-id="2a940-106">Otevřete hello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="2a940-106">Open hello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="2a940-107">Přejděte toohello odběr, který obsahuje **úlohy elastické databáze** součásti, konkrétně hello předplatné, ve které elastické databáze součásti úlohy nebyly nainstalovány.</span><span class="sxs-lookup"><span data-stu-id="2a940-107">Navigate toohello subscription that contains **Elastic Database jobs** components, namely hello subscription in which Elastic Database jobs components were installed.</span></span>
3. <span data-ttu-id="2a940-108">Klikněte na tlačítko **Procházet** a klikněte na tlačítko **skupiny prostředků**.</span><span class="sxs-lookup"><span data-stu-id="2a940-108">Click **Browse** and click **Resource groups**.</span></span>
4. <span data-ttu-id="2a940-109">Vyberte hello skupinu prostředků s názvem "__ElasticDatabaseJob".</span><span class="sxs-lookup"><span data-stu-id="2a940-109">Select hello resource group named "__ElasticDatabaseJob".</span></span>
5. <span data-ttu-id="2a940-110">Odstraňte skupinu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="2a940-110">Delete hello resource group.</span></span>

## <a name="uninstall--elastic-database-jobs-components-using-powershell"></a><span data-ttu-id="2a940-111">Odinstalace součástí úlohy elastické databáze pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="2a940-111">Uninstall  Elastic Database jobs components using PowerShell</span></span>
1. <span data-ttu-id="2a940-112">Spusťte příkazové okno Microsoft Azure PowerShell a přejděte toohello nástroje podadresář ve složce Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x hello: typ **cd nástroje**.</span><span class="sxs-lookup"><span data-stu-id="2a940-112">Launch a Microsoft Azure PowerShell command window and navigate toohello tools sub-directory under hello Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x folder: Type **cd tools**.</span></span>
   
     <span data-ttu-id="2a940-113">PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x* > disk cd nástroje</span><span class="sxs-lookup"><span data-stu-id="2a940-113">PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools</span></span>
2. <span data-ttu-id="2a940-114">Spusťte skript prostředí PowerShell.\UninstallElasticDatabaseJobs.ps1 hello.</span><span class="sxs-lookup"><span data-stu-id="2a940-114">Execute hello .\UninstallElasticDatabaseJobs.ps1 PowerShell script.</span></span>
   
     <span data-ttu-id="2a940-115">PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools > odblokovat soubor.\UninstallElasticDatabaseJobs.ps1 PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools >.\UninstallElasticDatabaseJobs.ps1</span><span class="sxs-lookup"><span data-stu-id="2a940-115">PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\UninstallElasticDatabaseJobs.ps1   PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\UninstallElasticDatabaseJobs.ps1</span></span>

<span data-ttu-id="2a940-116">Nebo jednoduše, spustit hello následující skript, za předpokladu, že výchozí hodnoty, kde je použít pro instalaci součástí hello:</span><span class="sxs-lookup"><span data-stu-id="2a940-116">Or simply, execute hello following script, assuming default values where used on installation of hello components:</span></span>

        $ResourceGroupName = "__ElasticDatabaseJob"
        Switch-AzureMode AzureResourceManager

        $resourceGroup = Get-AzureResourceGroup -Name $ResourceGroupName
        if(!$resourceGroup)
        {
            Write-Host "hello Azure Resource Group: $ResourceGroupName has already been deleted.  Elastic database job components are uninstalled."
            return
        }

        Write-Host "Removing hello Azure Resource Group: $ResourceGroupName.  This may take a few minutes.”
        Remove-AzureResourceGroup -Name $ResourceGroupName -Force
        Write-Host "Completed removing hello Azure Resource Group: $ResourceGroupName.  Elastic database job compoennts are now uninstalled."

## <a name="next-steps"></a><span data-ttu-id="2a940-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2a940-117">Next steps</span></span>
<span data-ttu-id="2a940-118">úlohy elastické databáze toore instalace, najdete v části [instalací služby úlohy elastické databáze hello](sql-database-elastic-jobs-service-installation.md)</span><span class="sxs-lookup"><span data-stu-id="2a940-118">toore-install Elastic Database jobs, see [Installing hello Elastic Database job service](sql-database-elastic-jobs-service-installation.md)</span></span>

<span data-ttu-id="2a940-119">Přehled úlohy elastické databáze najdete v tématu [přehled úlohy elastické databáze](sql-database-elastic-jobs-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2a940-119">For an overview of Elastic Database jobs, see [Elastic Database jobs overview](sql-database-elastic-jobs-overview.md).</span></span>

<!--Image references-->


