---
title: "Instalace úlohy elastické databáze | Microsoft Docs"
description: "Provede procesem instalace funkce elastické úlohy."
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: cbe0aa2b-17e3-4b6f-a16f-6ebc1f5a66af
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: e7a2d6dbcefbb31d76257eaf96ccc235d7a29416
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="installing-elastic-database-jobs-overview"></a><span data-ttu-id="49bb1-103">Instalace úlohy elastické databáze – přehled</span><span class="sxs-lookup"><span data-stu-id="49bb1-103">Installing Elastic Database jobs overview</span></span>
<span data-ttu-id="49bb1-104">[**Elastické databáze úlohy** ](sql-database-elastic-jobs-overview.md) lze nainstalovat pomocí prostředí PowerShell nebo prostřednictvím Azure Classic Portal.You mohou získat přístup k vytvářet a spravovat úlohy pomocí rozhraní API prostředí PowerShell, pouze v případě, že budete instalovat balíček prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="49bb1-104">[**Elastic Database jobs**](sql-database-elastic-jobs-overview.md) can be installed via PowerShell or through the Azure Classic Portal.You can gain access to create and manage jobs using the PowerShell API only if you install the PowerShell package.</span></span> <span data-ttu-id="49bb1-105">Kromě toho rozhraní API prostředí PowerShell poskytuje výrazně víc funkcí než portálu v daném okamžiku.</span><span class="sxs-lookup"><span data-stu-id="49bb1-105">Additionally, the PowerShell APIs provide significantly more functionality than the portal at this point in time.</span></span>

<span data-ttu-id="49bb1-106">Pokud jste již nainstalovali **úlohy elastické databáze** prostřednictvím portálu ze stávajícího **elastický fond**, nejnovější prostředí Powershell preview zahrnuje skripty, které upgradovat existující instalaci.</span><span class="sxs-lookup"><span data-stu-id="49bb1-106">If you have already installed **Elastic Database jobs** through the Portal from an existing **elastic pool**, the latest Powershell preview includes scripts to upgrade your existing installation.</span></span> <span data-ttu-id="49bb1-107">Důrazně doporučujeme pro upgrade na nejnovější instalace **úlohy elastické databáze** součásti, aby bylo možné využívat nové funkce, které jsou zveřejňovány prostřednictvím rozhraní API prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="49bb1-107">It is highly recommended to upgrade your installation to the latest **Elastic Database jobs** components in order to take advantage of new functionality exposed via the PowerShell APIs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="49bb1-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="49bb1-108">Prerequisites</span></span>
* <span data-ttu-id="49bb1-109">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="49bb1-109">An Azure subscription.</span></span> <span data-ttu-id="49bb1-110">Bezplatná zkušební verze, najdete v části [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="49bb1-110">For a free trial, see [Free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="49bb1-111">Azure Powershell</span><span class="sxs-lookup"><span data-stu-id="49bb1-111">Azure PowerShell.</span></span> <span data-ttu-id="49bb1-112">Nainstalujte nejnovější verzi pomocí [instalačního programu webové platformy](http://go.microsoft.com/fwlink/p/?linkid=320376).</span><span class="sxs-lookup"><span data-stu-id="49bb1-112">Install the latest version using the [Web Platform Installer](http://go.microsoft.com/fwlink/p/?linkid=320376).</span></span> <span data-ttu-id="49bb1-113">Podrobné informace najdete v tématu [Instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="49bb1-113">For detailed information, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="49bb1-114">[Nástroj příkazového řádku NuGet](https://nuget.org/nuget.exe) slouží k instalaci balíčku úlohy elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="49bb1-114">[NuGet Command-line Utility](https://nuget.org/nuget.exe) is used to install the Elastic Database jobs package.</span></span> <span data-ttu-id="49bb1-115">Další informace najdete v tématu http://docs.nuget.org/docs/start-here/installing-nuget.</span><span class="sxs-lookup"><span data-stu-id="49bb1-115">For more information, see http://docs.nuget.org/docs/start-here/installing-nuget.</span></span>

## <a name="download-and-import-the-elastic-database-jobs-powershell-package"></a><span data-ttu-id="49bb1-116">Stahování a import balíčku prostředí PowerShell úlohy elastické databáze</span><span class="sxs-lookup"><span data-stu-id="49bb1-116">Download and import the Elastic Database jobs PowerShell package</span></span>
1. <span data-ttu-id="49bb1-117">Spusťte příkazové okno prostředí PowerShell Microsoft Azure a přejděte do adresáře, kam jste stáhli Nástroj příkazového řádku NuGet (nuget.exe).</span><span class="sxs-lookup"><span data-stu-id="49bb1-117">Launch Microsoft Azure PowerShell command window and navigate to the directory where you downloaded NuGet Command-line Utility (nuget.exe).</span></span>
2. <span data-ttu-id="49bb1-118">Stahování a import **úlohy elastické databáze** balíček do aktuální adresář pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="49bb1-118">Download and import **Elastic Database jobs** package into the current directory with the following command:</span></span>
   
        PS C:\>.\nuget install Microsoft.Azure.SqlDatabase.Jobs -prerelease
   
    <span data-ttu-id="49bb1-119">**Úlohy elastické databáze** soubory jsou umístěny do místního adresáře do složky s názvem **Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x** kde *x.x.xxxx.x* zobrazuje číslo verze.</span><span class="sxs-lookup"><span data-stu-id="49bb1-119">The **Elastic Database jobs** files are placed in the local directory in a folder named **Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x** where *x.x.xxxx.x* reflects the version number.</span></span> <span data-ttu-id="49bb1-120">Rutiny prostředí PowerShell (včetně klienta knihoven DLL) jsou umístěné v **tools\ElasticDatabaseJobs** podadresář a skriptů prostředí PowerShell k instalaci, upgrade a odinstalaci jsou taky umístěné ve **nástroje** podadresář.</span><span class="sxs-lookup"><span data-stu-id="49bb1-120">The PowerShell cmdlets (including required client .dlls) are located in the **tools\ElasticDatabaseJobs** sub-directory and the PowerShell scripts to install, upgrade and uninstall also reside in the **tools** sub-directory.</span></span>
3. <span data-ttu-id="49bb1-121">Přejděte do nástroje podadresář ve složce Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x zadáním disk cd nástroje, například:</span><span class="sxs-lookup"><span data-stu-id="49bb1-121">Navigate to the tools sub-directory under the Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x folder by typing cd tools, for example:</span></span>
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools

4. <span data-ttu-id="49bb1-122">Spusťte skript.\InstallElasticDatabaseJobsCmdlets.ps1 chcete zkopírovat do adresáře ElasticDatabaseJobs do $home\Documents\WindowsPowerShell\Modules.</span><span class="sxs-lookup"><span data-stu-id="49bb1-122">Execute the .\InstallElasticDatabaseJobsCmdlets.ps1 script to copy the ElasticDatabaseJobs directory into $home\Documents\WindowsPowerShell\Modules.</span></span> <span data-ttu-id="49bb1-123">To také automaticky importuje modul pro použití, například:</span><span class="sxs-lookup"><span data-stu-id="49bb1-123">This will also automatically import the module for use, for example:</span></span>
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobsCmdlets.ps1
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\InstallElasticDatabaseJobsCmdlets.ps1

## <a name="install-the-elastic-database-jobs-components-using-powershell"></a><span data-ttu-id="49bb1-124">Nainstalujte komponenty úlohy elastické databáze pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="49bb1-124">Install the Elastic Database jobs components using PowerShell</span></span>
1. <span data-ttu-id="49bb1-125">Spusťte příkazové okno Microsoft Azure PowerShell a přejděte na \tools podadresář ve složce Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x: Zadejte \tools disku cd</span><span class="sxs-lookup"><span data-stu-id="49bb1-125">Launch a Microsoft Azure PowerShell command window and navigate to the \tools sub-directory under the Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x folder: Type cd \tools</span></span>
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools

2. <span data-ttu-id="49bb1-126">Spustit.\InstallElasticDatabaseJobs.ps1 skript prostředí PowerShell a zadejte hodnoty pro její požadované proměnné.</span><span class="sxs-lookup"><span data-stu-id="49bb1-126">Execute the .\InstallElasticDatabaseJobs.ps1 PowerShell script and supply values for its requested variables.</span></span> <span data-ttu-id="49bb1-127">Tento skript vytvoří komponent popsaných v [úlohy elastické databáze součásti a ceny](sql-database-elastic-jobs-overview.md#components-and-pricing) společně s konfigurací Azure Cloud Service odpovídajícím způsobem používat závislé součásti.</span><span class="sxs-lookup"><span data-stu-id="49bb1-127">This script will create the components described in [Elastic Database jobs components and pricing](sql-database-elastic-jobs-overview.md#components-and-pricing) along with configuring the Azure Cloud Service to appropriately use the dependent components.</span></span>

        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobs.ps1
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\InstallElasticDatabaseJobs.ps1

<span data-ttu-id="49bb1-128">Když spustíte tento příkaz, okno otevře požadující **uživatelské jméno** a **heslo**.</span><span class="sxs-lookup"><span data-stu-id="49bb1-128">When you run this command a window opens asking for a **user name** and **password**.</span></span> <span data-ttu-id="49bb1-129">Toto není vaše přihlašovací údaje Azure, zadejte uživatelské jméno a heslo, které budou přihlašovací údaje správce, kterou chcete vytvořit pro nový server.</span><span class="sxs-lookup"><span data-stu-id="49bb1-129">This is not your Azure credentials, enter the user name and password that will be the administrator credentials you want to create for the new server.</span></span>

<span data-ttu-id="49bb1-130">Parametry zadané na toto ukázkové volání lze upravit pro požadovaná nastavení.</span><span class="sxs-lookup"><span data-stu-id="49bb1-130">The parameters provided on this sample invocation can be modified for your desired settings.</span></span> <span data-ttu-id="49bb1-131">Následující poskytuje další informace o chování jednotlivých parametrů:</span><span class="sxs-lookup"><span data-stu-id="49bb1-131">The following provides more information on the behavior of each parameter:</span></span>

<table style="width:100%">
  <tr>
    <th><span data-ttu-id="49bb1-132">Parametr</span><span class="sxs-lookup"><span data-stu-id="49bb1-132">Parameter</span></span></th>
    <th><span data-ttu-id="49bb1-133">Popis</span><span class="sxs-lookup"><span data-stu-id="49bb1-133">Description</span></span></th>
  </tr>

<tr>
    <td><span data-ttu-id="49bb1-134">Název skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="49bb1-134">ResourceGroupName</span></span></td>
    <td><span data-ttu-id="49bb1-135">Poskytuje název skupiny prostředků Azure k vytvoření, která obsahují nově vytvořený Azure komponenty.</span><span class="sxs-lookup"><span data-stu-id="49bb1-135">Provides the Azure resource group name created to contain the newly created Azure components.</span></span> <span data-ttu-id="49bb1-136">Tento parametr výchozí "__ElasticDatabaseJob".</span><span class="sxs-lookup"><span data-stu-id="49bb1-136">This parameter defaults to “__ElasticDatabaseJob”.</span></span> <span data-ttu-id="49bb1-137">Není doporučeno tuto hodnotu změnit.</span><span class="sxs-lookup"><span data-stu-id="49bb1-137">It is not recommended to change this value.</span></span></td>
    </tr>

</tr>

    <tr>
    <td><span data-ttu-id="49bb1-138">ResourceGroupLocation</span><span class="sxs-lookup"><span data-stu-id="49bb1-138">ResourceGroupLocation</span></span></td>
    <td><span data-ttu-id="49bb1-139">Poskytuje umístění Azure, který se má použít pro nově vytvořený Azure součásti.</span><span class="sxs-lookup"><span data-stu-id="49bb1-139">Provides the Azure location to be used for the newly created Azure components.</span></span> <span data-ttu-id="49bb1-140">Tento parametr výchozí umístění střed USA.</span><span class="sxs-lookup"><span data-stu-id="49bb1-140">This parameter defaults to the Central US location.</span></span></td>
</tr>

<tr>
    <td><span data-ttu-id="49bb1-141">ServiceWorkerCount</span><span class="sxs-lookup"><span data-stu-id="49bb1-141">ServiceWorkerCount</span></span></td>
    <td><span data-ttu-id="49bb1-142">Obsahuje počet pracovních procesů služby pro instalaci.</span><span class="sxs-lookup"><span data-stu-id="49bb1-142">Provides the number of service workers to install.</span></span> <span data-ttu-id="49bb1-143">Tento parametr výchozí hodnotu 1.</span><span class="sxs-lookup"><span data-stu-id="49bb1-143">This parameter defaults to 1.</span></span> <span data-ttu-id="49bb1-144">Vyšší počet pracovních procesů lze škálovat službu a pro zajištění vysoké dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="49bb1-144">A higher number of workers can be used to scale out the service and to provide high availability.</span></span> <span data-ttu-id="49bb1-145">Doporučujeme použít pro nasazení, která vyžadují vysokou dostupnost služby "2".</span><span class="sxs-lookup"><span data-stu-id="49bb1-145">It is recommended to use “2” for deployments that require high availability of the service.</span></span></td>
    </tr>

</tr>
    <tr>
    <td><span data-ttu-id="49bb1-146">ServiceVmSize</span><span class="sxs-lookup"><span data-stu-id="49bb1-146">ServiceVmSize</span></span></td>
    <td><span data-ttu-id="49bb1-147">Poskytuje velikost virtuálního počítače pro použití v rámci cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="49bb1-147">Provides the VM size for usage within the Cloud Service.</span></span> <span data-ttu-id="49bb1-148">Tento parametr výchozí A0.</span><span class="sxs-lookup"><span data-stu-id="49bb1-148">This parameter defaults to A0.</span></span> <span data-ttu-id="49bb1-149">Hodnoty parametrů A0 nebo A1 nebo A2 nebo A3, jsou přijaty které způsobit role pracovního procesu sloužící velikost: mimořádně malý nebo malá nebo střední nebo velké v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="49bb1-149">Parameters values of A0/A1/A2/A3 are accepted which cause the worker role to use an ExtraSmall/Small/Medium/Large size, respectively.</span></span> <span data-ttu-id="49bb1-150">Další informace o velikosti role pracovního procesu, viz Fo [úlohy elastické databáze součásti a ceny](sql-database-elastic-jobs-overview.md#components-and-pricing).</span><span class="sxs-lookup"><span data-stu-id="49bb1-150">Fo more information on worker role sizes, see [Elastic Database jobs components and pricing](sql-database-elastic-jobs-overview.md#components-and-pricing).</span></span></td>
</tr>

</tr>
    <tr>
    <td><span data-ttu-id="49bb1-151">SqlServerDatabaseSlo</span><span class="sxs-lookup"><span data-stu-id="49bb1-151">SqlServerDatabaseSlo</span></span></td>
    <td><span data-ttu-id="49bb1-152">Poskytuje cíle na úrovni služby pro Standard edition.</span><span class="sxs-lookup"><span data-stu-id="49bb1-152">Provides the service level objective for a Standard edition.</span></span> <span data-ttu-id="49bb1-153">Tento parametr výchozí hodnotu S0.</span><span class="sxs-lookup"><span data-stu-id="49bb1-153">This parameter defaults to S0.</span></span> <span data-ttu-id="49bb1-154">Hodnoty parametru S0/S1 nebo S2/S3, které způsobují databáze SQL Azure používat příslušné SLO jsou přijaty.</span><span class="sxs-lookup"><span data-stu-id="49bb1-154">Parameter values of S0/S1/S2/S3 are accepted which cause the Azure SQL Database to use the respective SLO.</span></span> <span data-ttu-id="49bb1-155">Další informace o slo databáze SQL najdete v tématu [úlohy elastické databáze součásti a ceny](sql-database-elastic-jobs-overview.md#components-and-pricing).</span><span class="sxs-lookup"><span data-stu-id="49bb1-155">For more information on SQL Database SLOs, see [Elastic Database jobs components and pricing](sql-database-elastic-jobs-overview.md#components-and-pricing).</span></span></td>
</tr>

</tr>
    <tr>
    <td><span data-ttu-id="49bb1-156">SqlServerAdministratorUserName</span><span class="sxs-lookup"><span data-stu-id="49bb1-156">SqlServerAdministratorUserName</span></span></td>
    <td><span data-ttu-id="49bb1-157">Poskytuje uživatelské jméno správce pro nově vytvořený serveru Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="49bb1-157">Provides the admin user name for the newly created Azure SQL Database server.</span></span> <span data-ttu-id="49bb1-158">Není-li zadána, bude vyžadovat přihlašovací údaje otevřete okno přihlašovací údaje prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="49bb1-158">When not specified, a PowerShell credentials window will open to prompt for the credentials.</span></span></td>
</tr>

</tr>
    <tr>
    <td><span data-ttu-id="49bb1-159">SqlServerAdministratorPassword</span><span class="sxs-lookup"><span data-stu-id="49bb1-159">SqlServerAdministratorPassword</span></span></td>
    <td><span data-ttu-id="49bb1-160">Poskytuje heslo správce pro nově vytvořený serveru Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="49bb1-160">Provides the admin password for the newly created Azure SQL Database server.</span></span> <span data-ttu-id="49bb1-161">Když není zadaná, bude vyžadovat přihlašovací údaje otevřete okno přihlašovací údaje prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="49bb1-161">When not provided, a PowerShell credentials window will open to prompt for the credentials.</span></span></td>
</tr>
</table>

<span data-ttu-id="49bb1-162">Pro systémy, které cílí má velký počet úloh spuštěných současně pro velký počet databází, se doporučuje zadejte parametry, jako například: - ServiceWorkerCount 2 - ServiceVmSize A2 - SqlServerDatabaseSlo S2.</span><span class="sxs-lookup"><span data-stu-id="49bb1-162">For systems that target having large numbers of jobs running in parallel against a large number of databases, it is recommended to specify parameters such as: -ServiceWorkerCount 2 -ServiceVmSize A2 -SqlServerDatabaseSlo S2.</span></span>

    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobs.ps1
    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>.\InstallElasticDatabaseJobs.ps1 -ServiceWorkerCount 2 -ServiceVmSize A2 -SqlServerDatabaseSlo S2

## <a name="update-an-existing-elastic-database-jobs-components-installation-using-powershell"></a><span data-ttu-id="49bb1-163">Aktualizace stávající elastické databáze úlohy součásti instalace pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="49bb1-163">Update an existing Elastic Database jobs components installation using PowerShell</span></span>
<span data-ttu-id="49bb1-164">**Elastické databáze úlohy** můžete aktualizovat v rámci existující instalace pro škálování a vysokou dostupností.</span><span class="sxs-lookup"><span data-stu-id="49bb1-164">**Elastic Database jobs** can be updated within an existing installation for scale and high-availability.</span></span> <span data-ttu-id="49bb1-165">Tento proces umožňuje pro budoucí upgrady kódu služby bez nutnosti vyřadit a znovu vytvořte databázi ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="49bb1-165">This process allows for future upgrades of service code without having to drop and recreate the control database.</span></span> <span data-ttu-id="49bb1-166">Tento proces lze také v rámci stejné verze a změňte velikost virtuálního počítače služby nebo počet pracovního procesu serveru.</span><span class="sxs-lookup"><span data-stu-id="49bb1-166">This process can also be used within the same version to modify the service VM size or the server worker count.</span></span>

<span data-ttu-id="49bb1-167">Pokud chcete aktualizovat velikost virtuálního počítače instalace, spusťte následující skript s parametry aktualizovány na hodnoty podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="49bb1-167">To update the VM size of an installation, run the following script with parameters updated to the values of your choice.</span></span>

    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>Unblock-File .\UpdateElasticDatabaseJobs.ps1
    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>.\UpdateElasticDatabaseJobs.ps1 -ServiceVmSize A1 -ServiceWorkerCount 2

<table style="width:100%">
  <tr>
  <th><span data-ttu-id="49bb1-168">Parametr</span><span class="sxs-lookup"><span data-stu-id="49bb1-168">Parameter</span></span></th>
  <th><span data-ttu-id="49bb1-169">Popis</span><span class="sxs-lookup"><span data-stu-id="49bb1-169">Description</span></span></th>
</tr>

  <tr>
    <td><span data-ttu-id="49bb1-170">Název skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="49bb1-170">ResourceGroupName</span></span></td>
    <td><span data-ttu-id="49bb1-171">Určuje název skupiny prostředků Azure používá při byly původně nainstalované komponenty úlohy elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="49bb1-171">Identifies the Azure resource group name used when the Elastic Database job components were initially installed.</span></span> <span data-ttu-id="49bb1-172">Tento parametr výchozí "__ElasticDatabaseJob".</span><span class="sxs-lookup"><span data-stu-id="49bb1-172">This parameter defaults to “__ElasticDatabaseJob”.</span></span> <span data-ttu-id="49bb1-173">Vzhledem k tomu, že není doporučeno tuto hodnotu změnit, není nutné, zadejte tento parametr.</span><span class="sxs-lookup"><span data-stu-id="49bb1-173">Since it is not recommended to change this value, you shouldn't have to specify this parameter.</span></span></td>
    </tr>
</tr>

</tr>

  <tr>
    <td><span data-ttu-id="49bb1-174">ServiceWorkerCount</span><span class="sxs-lookup"><span data-stu-id="49bb1-174">ServiceWorkerCount</span></span></td>
    <td><span data-ttu-id="49bb1-175">Obsahuje počet pracovních procesů služby pro instalaci.</span><span class="sxs-lookup"><span data-stu-id="49bb1-175">Provides the number of service workers to install.</span></span>  <span data-ttu-id="49bb1-176">Tento parametr výchozí hodnotu 1.</span><span class="sxs-lookup"><span data-stu-id="49bb1-176">This parameter defaults to 1.</span></span>  <span data-ttu-id="49bb1-177">Vyšší počet pracovních procesů lze škálovat službu a pro zajištění vysoké dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="49bb1-177">A higher number of workers can be used to scale out the service and to provide high availability.</span></span>  <span data-ttu-id="49bb1-178">Doporučujeme použít pro nasazení, která vyžadují vysokou dostupnost služby "2".</span><span class="sxs-lookup"><span data-stu-id="49bb1-178">It is recommended to use “2” for deployments that require high availability of the service.</span></span></td>
</tr>

</tr>

    <tr>
    <td><span data-ttu-id="49bb1-179">ServiceVmSize</span><span class="sxs-lookup"><span data-stu-id="49bb1-179">ServiceVmSize</span></span></td>
    <td><span data-ttu-id="49bb1-180">Poskytuje velikost virtuálního počítače pro použití v rámci cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="49bb1-180">Provides the VM size for usage within the Cloud Service.</span></span> <span data-ttu-id="49bb1-181">Tento parametr výchozí A0.</span><span class="sxs-lookup"><span data-stu-id="49bb1-181">This parameter defaults to A0.</span></span> <span data-ttu-id="49bb1-182">Hodnoty parametrů A0 nebo A1 nebo A2 nebo A3, jsou přijaty které způsobit role pracovního procesu sloužící velikost: mimořádně malý nebo malá nebo střední nebo velké v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="49bb1-182">Parameters values of A0/A1/A2/A3 are accepted which cause the worker role to use an ExtraSmall/Small/Medium/Large size, respectively.</span></span> <span data-ttu-id="49bb1-183">Další informace o velikosti role pracovního procesu, viz Fo [úlohy elastické databáze součásti a ceny](sql-database-elastic-jobs-overview.md#components-and-pricing).</span><span class="sxs-lookup"><span data-stu-id="49bb1-183">Fo more information on worker role sizes, see [Elastic Database jobs components and pricing](sql-database-elastic-jobs-overview.md#components-and-pricing).</span></span></td>
</tr>

</table>

## <a name="install-the-elastic-database-jobs-components-using-the-portal"></a><span data-ttu-id="49bb1-184">Nainstalujte komponenty úlohy elastické databáze pomocí portálu</span><span class="sxs-lookup"><span data-stu-id="49bb1-184">Install the Elastic Database jobs components using the Portal</span></span>
<span data-ttu-id="49bb1-185">Jakmile máte [vytvoření fondu elastické databáze](sql-database-elastic-pool-manage-portal.md), můžete nainstalovat **úlohy elastické databáze** součásti povolit spouštění úlohy správy na každou databázi v elastickém fondu.</span><span class="sxs-lookup"><span data-stu-id="49bb1-185">Once you have [created an elastic pool](sql-database-elastic-pool-manage-portal.md), you can install **Elastic Database jobs** components to enable execution of administrative tasks against each database in the elastic pool.</span></span> <span data-ttu-id="49bb1-186">Na rozdíl od při použití **úlohy elastické databáze** rozhraní API prostředí PowerShell, rozhraní portálu je momentálně omezené do pouze prováděných na existující fond.</span><span class="sxs-lookup"><span data-stu-id="49bb1-186">Unlike when using the **Elastic Database jobs** PowerShell APIs, the portal interface is currently restricted to only executing against an existing pool.</span></span>

<span data-ttu-id="49bb1-187">**Odhadovaný čas dokončení:** 10 minut.</span><span class="sxs-lookup"><span data-stu-id="49bb1-187">**Estimated time to complete:** 10 minutes.</span></span>

1. <span data-ttu-id="49bb1-188">Z elastického fondu pomocí zobrazení řídicího panelu [portálu Azure](https://portal.azure.com/#) , klikněte na tlačítko **vytvořit úlohu**.</span><span class="sxs-lookup"><span data-stu-id="49bb1-188">From the dashboard view of the elastic pool via the [Azure Portal](https://portal.azure.com/#) , click **Create job**.</span></span>
2. <span data-ttu-id="49bb1-189">Pokud vytváříte úlohu poprvé, musíte nainstalovat **úlohy elastické databáze** kliknutím **podmínky**.</span><span class="sxs-lookup"><span data-stu-id="49bb1-189">If you are creating a job for the first time, you must install **Elastic Database jobs** by clicking **PREVIEW TERMS**.</span></span>
3. <span data-ttu-id="49bb1-190">Přijměte podmínky kliknutím na zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="49bb1-190">Accept the terms by clicking the checkbox.</span></span>
4. <span data-ttu-id="49bb1-191">V zobrazení "Instalace služby", klikněte na **úlohy pověření**.</span><span class="sxs-lookup"><span data-stu-id="49bb1-191">In the "Install services" view, click **JOB CREDENTIALS**.</span></span>
   
    ![Instalace služby][1]
5. <span data-ttu-id="49bb1-193">Zadejte uživatelské jméno a heslo pro správce databáze</span><span class="sxs-lookup"><span data-stu-id="49bb1-193">Type a user name and password for a database admin.</span></span> <span data-ttu-id="49bb1-194">Jako součást instalace se vytvoří nový server Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="49bb1-194">As part of the installation, a new Azure SQL Database server is created.</span></span> <span data-ttu-id="49bb1-195">V rámci tohoto serveru je novou databázi, označuje jako databázi řízení vytvořit a používat tak, aby obsahovala metadata pro úlohy elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="49bb1-195">Within this new server, a new database, known as the control database, is created and used to contain the meta data for Elastic Database jobs.</span></span> <span data-ttu-id="49bb1-196">Uživatelské jméno a heslo vytvořená zde se používají pro účely přihlášení k databázi řízení.</span><span class="sxs-lookup"><span data-stu-id="49bb1-196">The user name and password created here are used for the purpose of logging in to the control database.</span></span> <span data-ttu-id="49bb1-197">Samostatné přihlašovací údaje se používá pro spuštění skriptu s databázemi, v rámci fondu.</span><span class="sxs-lookup"><span data-stu-id="49bb1-197">A separate credential is used for script execution against the databases within the pool.</span></span>
   
    ![Vytvořte uživatelské jméno a heslo][2]
6. <span data-ttu-id="49bb1-199">Klikněte na tlačítko OK.</span><span class="sxs-lookup"><span data-stu-id="49bb1-199">Click the OK button.</span></span> <span data-ttu-id="49bb1-200">Součásti jsou vytvořeny pro vás za pár minut v nové [skupiny prostředků](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="49bb1-200">The components are created for you in a few minutes in a new [Resource group](../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="49bb1-201">Novou skupinu prostředků je připnutý na úvodní panel, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="49bb1-201">The new resource group is pinned to the start board, as shown below.</span></span> <span data-ttu-id="49bb1-202">Úlohy po vytvoření, elastické databáze (Cloudová služba, databáze SQL, Service Bus a úložiště) jsou vytvořeny ve skupině.</span><span class="sxs-lookup"><span data-stu-id="49bb1-202">Once created, elastic database jobs (Cloud Service, SQL Database, Service Bus, and Storage) are all created in the group.</span></span>
   
    ![Skupina prostředků v Tabule start][3]
7. <span data-ttu-id="49bb1-204">Pokud se pokusíte vytvořit ani spravovat úlohu při instalaci úlohy elastické databáze, pokud poskytuje **pověření** zobrazí se následující zpráva.</span><span class="sxs-lookup"><span data-stu-id="49bb1-204">If you attempt to create or manage a job while elastic database jobs is installing, when providing **Credentials** you will see the following message.</span></span>
   
    ![Stále probíhá nasazení][4]

<span data-ttu-id="49bb1-206">Pokud odinstalaci je potřeba, odstraňte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="49bb1-206">If uninstallation is required, delete the resource group.</span></span> <span data-ttu-id="49bb1-207">V tématu [postup odinstalace komponenty úlohy elastické databáze](sql-database-elastic-jobs-uninstall.md).</span><span class="sxs-lookup"><span data-stu-id="49bb1-207">See [How to uninstall the Elastic Database job components](sql-database-elastic-jobs-uninstall.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="49bb1-208">Další kroky</span><span class="sxs-lookup"><span data-stu-id="49bb1-208">Next steps</span></span>
<span data-ttu-id="49bb1-209">Zkontrolujte přihlašovací údaj se příslušná oprávnění pro spuštění skriptu se vytvoří na každou databázi ve skupině, další informace najdete v tématu [zabezpečení databáze SQL](sql-database-manage-logins.md).</span><span class="sxs-lookup"><span data-stu-id="49bb1-209">Ensure a credential with the appropriate rights for script execution is created on each database in the group, for more information see [Securing your SQL Database](sql-database-manage-logins.md).</span></span>
<span data-ttu-id="49bb1-210">V tématu [vytváření a Správa úloh elastické databáze](sql-database-elastic-jobs-create-and-manage.md) začít pracovat.</span><span class="sxs-lookup"><span data-stu-id="49bb1-210">See [Creating and managing an Elastic Database jobs](sql-database-elastic-jobs-create-and-manage.md) to get started.</span></span>

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-service-installation/screen-1.png
[2]: ./media/sql-database-elastic-jobs-service-installation/credentials.png
[3]: ./media/sql-database-elastic-jobs-service-installation/start-board.png
[4]: ./media/sql-database-elastic-jobs-service-installation/not-done.png
