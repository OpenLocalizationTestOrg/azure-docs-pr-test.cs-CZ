---
title: "úlohy elastické databáze aaaInstalling | Microsoft Docs"
description: "Provede procesem instalace funkce elastické úlohy hello."
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
ms.openlocfilehash: 0349f66a4428f81d00d43681d7f2177f273ec032
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="installing-elastic-database-jobs-overview"></a><span data-ttu-id="1128a-103">Instalace úlohy elastické databáze – přehled</span><span class="sxs-lookup"><span data-stu-id="1128a-103">Installing Elastic Database jobs overview</span></span>
<span data-ttu-id="1128a-104">[**Elastické databáze úlohy** ](sql-database-elastic-jobs-overview.md) lze nainstalovat pomocí prostředí PowerShell nebo prostřednictvím hello Azure Classic Portal.You můžete získat přístup k toocreate a spravovat úlohy pomocí hello rozhraní API prostředí PowerShell, pouze v případě, že budete instalovat balíček hello prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1128a-104">[**Elastic Database jobs**](sql-database-elastic-jobs-overview.md) can be installed via PowerShell or through hello Azure Classic Portal.You can gain access toocreate and manage jobs using hello PowerShell API only if you install hello PowerShell package.</span></span> <span data-ttu-id="1128a-105">Kromě toho hello rozhraní API prostředí PowerShell poskytuje výrazně víc funkcí než hello portál v tuto chvíli.</span><span class="sxs-lookup"><span data-stu-id="1128a-105">Additionally, hello PowerShell APIs provide significantly more functionality than hello portal at this point in time.</span></span>

<span data-ttu-id="1128a-106">Pokud jste již nainstalovali **úlohy elastické databáze** prostřednictvím hello portálu ze stávajícího **elastický fond**, hello nejnovější prostředí Powershell preview zahrnuje skripty tooupgrade vaši existující instalaci.</span><span class="sxs-lookup"><span data-stu-id="1128a-106">If you have already installed **Elastic Database jobs** through hello Portal from an existing **elastic pool**, hello latest Powershell preview includes scripts tooupgrade your existing installation.</span></span> <span data-ttu-id="1128a-107">Vysoce je doporučeno tooupgrade vaší instalace toohello nejnovější **úlohy elastické databáze** hello součásti v pořadí tootake využívat nové funkce, které jsou zveřejňovány prostřednictvím rozhraní API prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1128a-107">It is highly recommended tooupgrade your installation toohello latest **Elastic Database jobs** components in order tootake advantage of new functionality exposed via hello PowerShell APIs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1128a-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1128a-108">Prerequisites</span></span>
* <span data-ttu-id="1128a-109">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="1128a-109">An Azure subscription.</span></span> <span data-ttu-id="1128a-110">Bezplatná zkušební verze, najdete v části [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1128a-110">For a free trial, see [Free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="1128a-111">Azure Powershell</span><span class="sxs-lookup"><span data-stu-id="1128a-111">Azure PowerShell.</span></span> <span data-ttu-id="1128a-112">Nainstalujte nejnovější verzi hello pomocí hello [instalačního programu webové platformy](http://go.microsoft.com/fwlink/p/?linkid=320376).</span><span class="sxs-lookup"><span data-stu-id="1128a-112">Install hello latest version using hello [Web Platform Installer](http://go.microsoft.com/fwlink/p/?linkid=320376).</span></span> <span data-ttu-id="1128a-113">Podrobné informace najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1128a-113">For detailed information, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="1128a-114">[Nástroj příkazového řádku NuGet](https://nuget.org/nuget.exe) je použité tooinstall hello elastické databáze úlohy balíček.</span><span class="sxs-lookup"><span data-stu-id="1128a-114">[NuGet Command-line Utility](https://nuget.org/nuget.exe) is used tooinstall hello Elastic Database jobs package.</span></span> <span data-ttu-id="1128a-115">Další informace najdete v tématu http://docs.nuget.org/docs/start-here/installing-nuget.</span><span class="sxs-lookup"><span data-stu-id="1128a-115">For more information, see http://docs.nuget.org/docs/start-here/installing-nuget.</span></span>

## <a name="download-and-import-hello-elastic-database-jobs-powershell-package"></a><span data-ttu-id="1128a-116">Stahování a import balíčku prostředí PowerShell úlohy elastické databáze hello</span><span class="sxs-lookup"><span data-stu-id="1128a-116">Download and import hello Elastic Database jobs PowerShell package</span></span>
1. <span data-ttu-id="1128a-117">Spusťte příkazové okno prostředí PowerShell Microsoft Azure a přejděte toohello adresáře, kam jste stáhli Nástroj příkazového řádku NuGet (nuget.exe).</span><span class="sxs-lookup"><span data-stu-id="1128a-117">Launch Microsoft Azure PowerShell command window and navigate toohello directory where you downloaded NuGet Command-line Utility (nuget.exe).</span></span>
2. <span data-ttu-id="1128a-118">Stahování a import **úlohy elastické databáze** balíček do aktuální adresář hello s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="1128a-118">Download and import **Elastic Database jobs** package into hello current directory with hello following command:</span></span>
   
        PS C:\>.\nuget install Microsoft.Azure.SqlDatabase.Jobs -prerelease
   
    <span data-ttu-id="1128a-119">Hello **úlohy elastické databáze** soubory jsou umístěny do místního adresáře hello ve složce s názvem **Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x** kde *x.x.xxxx.x* Zobrazuje číslo verze hello.</span><span class="sxs-lookup"><span data-stu-id="1128a-119">hello **Elastic Database jobs** files are placed in hello local directory in a folder named **Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x** where *x.x.xxxx.x* reflects hello version number.</span></span> <span data-ttu-id="1128a-120">rutiny prostředí PowerShell Hello (včetně klienta knihoven DLL) jsou umístěné v hello **tools\ElasticDatabaseJobs** podadresář a hello tooinstall skripty prostředí PowerShell, upgradu a odinstalaci také nacházet v hello  **Nástroje pro** podadresář.</span><span class="sxs-lookup"><span data-stu-id="1128a-120">hello PowerShell cmdlets (including required client .dlls) are located in hello **tools\ElasticDatabaseJobs** sub-directory and hello PowerShell scripts tooinstall, upgrade and uninstall also reside in hello **tools** sub-directory.</span></span>
3. <span data-ttu-id="1128a-121">Přejděte toohello nástroje podadresář ve složce Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x hello zadáním disk cd nástroje, například:</span><span class="sxs-lookup"><span data-stu-id="1128a-121">Navigate toohello tools sub-directory under hello Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x folder by typing cd tools, for example:</span></span>
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools

4. <span data-ttu-id="1128a-122">Spusťte hello.\InstallElasticDatabaseJobsCmdlets.ps1 skriptu toocopy hello ElasticDatabaseJobs adresáře do $home\Documents\WindowsPowerShell\Modules.</span><span class="sxs-lookup"><span data-stu-id="1128a-122">Execute hello .\InstallElasticDatabaseJobsCmdlets.ps1 script toocopy hello ElasticDatabaseJobs directory into $home\Documents\WindowsPowerShell\Modules.</span></span> <span data-ttu-id="1128a-123">To také automaticky importuje hello modulu pro použití, například:</span><span class="sxs-lookup"><span data-stu-id="1128a-123">This will also automatically import hello module for use, for example:</span></span>
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobsCmdlets.ps1
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\InstallElasticDatabaseJobsCmdlets.ps1

## <a name="install-hello-elastic-database-jobs-components-using-powershell"></a><span data-ttu-id="1128a-124">Instalace součásti úlohy hello elastické databáze pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="1128a-124">Install hello Elastic Database jobs components using PowerShell</span></span>
1. <span data-ttu-id="1128a-125">Spusťte příkazové okno Microsoft Azure PowerShell a přejděte toohello \tools podadresář ve složce Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x hello: Zadejte \tools disku cd</span><span class="sxs-lookup"><span data-stu-id="1128a-125">Launch a Microsoft Azure PowerShell command window and navigate toohello \tools sub-directory under hello Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x folder: Type cd \tools</span></span>
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools

2. <span data-ttu-id="1128a-126">Spustit skript prostředí PowerShell.\InstallElasticDatabaseJobs.ps1 hello a poskytnout hodnoty pro její požadované proměnné.</span><span class="sxs-lookup"><span data-stu-id="1128a-126">Execute hello .\InstallElasticDatabaseJobs.ps1 PowerShell script and supply values for its requested variables.</span></span> <span data-ttu-id="1128a-127">Tento skript vytvoří hello komponent popsaných v [úlohy elastické databáze součásti a ceny](sql-database-elastic-jobs-overview.md#components-and-pricing) společně s konfigurací hello Azure Cloud Service tooappropriately použít hello závislé součásti.</span><span class="sxs-lookup"><span data-stu-id="1128a-127">This script will create hello components described in [Elastic Database jobs components and pricing](sql-database-elastic-jobs-overview.md#components-and-pricing) along with configuring hello Azure Cloud Service tooappropriately use hello dependent components.</span></span>

        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobs.ps1
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\InstallElasticDatabaseJobs.ps1

<span data-ttu-id="1128a-128">Když spustíte tento příkaz, okno otevře požadující **uživatelské jméno** a **heslo**.</span><span class="sxs-lookup"><span data-stu-id="1128a-128">When you run this command a window opens asking for a **user name** and **password**.</span></span> <span data-ttu-id="1128a-129">Toto není vaše přihlašovací údaje Azure, zadejte hello uživatelské jméno a heslo, které budou přihlašovací údaje správce hello toocreate chcete pro nový server hello.</span><span class="sxs-lookup"><span data-stu-id="1128a-129">This is not your Azure credentials, enter hello user name and password that will be hello administrator credentials you want toocreate for hello new server.</span></span>

<span data-ttu-id="1128a-130">pro požadovaná nastavení můžete upravit parametry Hello k dispozici na toto ukázkové volání.</span><span class="sxs-lookup"><span data-stu-id="1128a-130">hello parameters provided on this sample invocation can be modified for your desired settings.</span></span> <span data-ttu-id="1128a-131">Následující Hello poskytuje další informace o chování hello každý parametr:</span><span class="sxs-lookup"><span data-stu-id="1128a-131">hello following provides more information on hello behavior of each parameter:</span></span>

<table style="width:100%">
  <tr>
    <th><span data-ttu-id="1128a-132">Parametr</span><span class="sxs-lookup"><span data-stu-id="1128a-132">Parameter</span></span></th>
    <th><span data-ttu-id="1128a-133">Popis</span><span class="sxs-lookup"><span data-stu-id="1128a-133">Description</span></span></th>
  </tr>

<tr>
    <td><span data-ttu-id="1128a-134">Název skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="1128a-134">ResourceGroupName</span></span></td>
    <td><span data-ttu-id="1128a-135">Poskytuje název skupiny prostředků Azure hello vytvořit toocontain hello nově vytvořený Azure součásti.</span><span class="sxs-lookup"><span data-stu-id="1128a-135">Provides hello Azure resource group name created toocontain hello newly created Azure components.</span></span> <span data-ttu-id="1128a-136">Tento parametr výchozí příliš "__ElasticDatabaseJob".</span><span class="sxs-lookup"><span data-stu-id="1128a-136">This parameter defaults too“__ElasticDatabaseJob”.</span></span> <span data-ttu-id="1128a-137">Není doporučeno toochange tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="1128a-137">It is not recommended toochange this value.</span></span></td>
    </tr>

</tr>

    <tr>
    <td><span data-ttu-id="1128a-138">ResourceGroupLocation</span><span class="sxs-lookup"><span data-stu-id="1128a-138">ResourceGroupLocation</span></span></td>
    <td><span data-ttu-id="1128a-139">Poskytuje toobe umístění Azure hello používá pro nově vytvořený Azure součásti hello.</span><span class="sxs-lookup"><span data-stu-id="1128a-139">Provides hello Azure location toobe used for hello newly created Azure components.</span></span> <span data-ttu-id="1128a-140">Tento parametr výchozí toohello střed USA umístění.</span><span class="sxs-lookup"><span data-stu-id="1128a-140">This parameter defaults toohello Central US location.</span></span></td>
</tr>

<tr>
    <td><span data-ttu-id="1128a-141">ServiceWorkerCount</span><span class="sxs-lookup"><span data-stu-id="1128a-141">ServiceWorkerCount</span></span></td>
    <td><span data-ttu-id="1128a-142">Poskytuje hello počet tooinstall pracovníky služby.</span><span class="sxs-lookup"><span data-stu-id="1128a-142">Provides hello number of service workers tooinstall.</span></span> <span data-ttu-id="1128a-143">Tento parametr výchozí too1.</span><span class="sxs-lookup"><span data-stu-id="1128a-143">This parameter defaults too1.</span></span> <span data-ttu-id="1128a-144">Vyšší počet pracovních procesů může být použité tooscale out hello služby a tooprovide vysokou dostupnost.</span><span class="sxs-lookup"><span data-stu-id="1128a-144">A higher number of workers can be used tooscale out hello service and tooprovide high availability.</span></span> <span data-ttu-id="1128a-145">Doporučujeme toouse "2" pro nasazení, která vyžadují vysokou dostupnost služby hello.</span><span class="sxs-lookup"><span data-stu-id="1128a-145">It is recommended toouse “2” for deployments that require high availability of hello service.</span></span></td>
    </tr>

</tr>
    <tr>
    <td><span data-ttu-id="1128a-146">ServiceVmSize</span><span class="sxs-lookup"><span data-stu-id="1128a-146">ServiceVmSize</span></span></td>
    <td><span data-ttu-id="1128a-147">Poskytuje hello velikost virtuálního počítače pro použití v rámci hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="1128a-147">Provides hello VM size for usage within hello Cloud Service.</span></span> <span data-ttu-id="1128a-148">Tento parametr výchozí tooA0.</span><span class="sxs-lookup"><span data-stu-id="1128a-148">This parameter defaults tooA0.</span></span> <span data-ttu-id="1128a-149">Hodnoty parametrů A0 nebo A1 nebo A2 nebo A3, jsou přijaty které způsobit hello pracovní toouse role pro velikost: mimořádně malý nebo malá nebo střední nebo velké.</span><span class="sxs-lookup"><span data-stu-id="1128a-149">Parameters values of A0/A1/A2/A3 are accepted which cause hello worker role toouse an ExtraSmall/Small/Medium/Large size, respectively.</span></span> <span data-ttu-id="1128a-150">Další informace o velikosti role pracovního procesu, viz Fo [úlohy elastické databáze součásti a ceny](sql-database-elastic-jobs-overview.md#components-and-pricing).</span><span class="sxs-lookup"><span data-stu-id="1128a-150">Fo more information on worker role sizes, see [Elastic Database jobs components and pricing](sql-database-elastic-jobs-overview.md#components-and-pricing).</span></span></td>
</tr>

</tr>
    <tr>
    <td><span data-ttu-id="1128a-151">SqlServerDatabaseSlo</span><span class="sxs-lookup"><span data-stu-id="1128a-151">SqlServerDatabaseSlo</span></span></td>
    <td><span data-ttu-id="1128a-152">Poskytuje hello cíle na úrovni služby pro Standard edition.</span><span class="sxs-lookup"><span data-stu-id="1128a-152">Provides hello service level objective for a Standard edition.</span></span> <span data-ttu-id="1128a-153">Tento parametr výchozí tooS0.</span><span class="sxs-lookup"><span data-stu-id="1128a-153">This parameter defaults tooS0.</span></span> <span data-ttu-id="1128a-154">Hodnoty parametru S0/S1 nebo S2/S3 přijímají, které způsobují hello Azure SQL Database toouse hello příslušných SLO.</span><span class="sxs-lookup"><span data-stu-id="1128a-154">Parameter values of S0/S1/S2/S3 are accepted which cause hello Azure SQL Database toouse hello respective SLO.</span></span> <span data-ttu-id="1128a-155">Další informace o slo databáze SQL najdete v tématu [úlohy elastické databáze součásti a ceny](sql-database-elastic-jobs-overview.md#components-and-pricing).</span><span class="sxs-lookup"><span data-stu-id="1128a-155">For more information on SQL Database SLOs, see [Elastic Database jobs components and pricing](sql-database-elastic-jobs-overview.md#components-and-pricing).</span></span></td>
</tr>

</tr>
    <tr>
    <td><span data-ttu-id="1128a-156">SqlServerAdministratorUserName</span><span class="sxs-lookup"><span data-stu-id="1128a-156">SqlServerAdministratorUserName</span></span></td>
    <td><span data-ttu-id="1128a-157">Poskytuje hello uživatelské jméno správce pro hello nově vytvořený serveru Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="1128a-157">Provides hello admin user name for hello newly created Azure SQL Database server.</span></span> <span data-ttu-id="1128a-158">Není-li zadána, otevře se okno přihlašovací údaje prostředí PowerShell tooprompt hello přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="1128a-158">When not specified, a PowerShell credentials window will open tooprompt for hello credentials.</span></span></td>
</tr>

</tr>
    <tr>
    <td><span data-ttu-id="1128a-159">SqlServerAdministratorPassword</span><span class="sxs-lookup"><span data-stu-id="1128a-159">SqlServerAdministratorPassword</span></span></td>
    <td><span data-ttu-id="1128a-160">Poskytuje hello heslo správce pro hello nově vytvořený serveru Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="1128a-160">Provides hello admin password for hello newly created Azure SQL Database server.</span></span> <span data-ttu-id="1128a-161">Když není zadaná, otevře se okno přihlašovací údaje prostředí PowerShell tooprompt hello přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="1128a-161">When not provided, a PowerShell credentials window will open tooprompt for hello credentials.</span></span></td>
</tr>
</table>

<span data-ttu-id="1128a-162">Pro systémy, které cílí má velký počet úloh spuštěných současně pro velký počet databází, se doporučuje toospecify parametry, jako: - ServiceWorkerCount 2 - ServiceVmSize A2 - SqlServerDatabaseSlo S2.</span><span class="sxs-lookup"><span data-stu-id="1128a-162">For systems that target having large numbers of jobs running in parallel against a large number of databases, it is recommended toospecify parameters such as: -ServiceWorkerCount 2 -ServiceVmSize A2 -SqlServerDatabaseSlo S2.</span></span>

    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobs.ps1
    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>.\InstallElasticDatabaseJobs.ps1 -ServiceWorkerCount 2 -ServiceVmSize A2 -SqlServerDatabaseSlo S2

## <a name="update-an-existing-elastic-database-jobs-components-installation-using-powershell"></a><span data-ttu-id="1128a-163">Aktualizace stávající elastické databáze úlohy součásti instalace pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="1128a-163">Update an existing Elastic Database jobs components installation using PowerShell</span></span>
<span data-ttu-id="1128a-164">**Elastické databáze úlohy** můžete aktualizovat v rámci existující instalace pro škálování a vysokou dostupností.</span><span class="sxs-lookup"><span data-stu-id="1128a-164">**Elastic Database jobs** can be updated within an existing installation for scale and high-availability.</span></span> <span data-ttu-id="1128a-165">Tento proces umožňuje pro budoucí upgrady kódu služby bez nutnosti toodrop a znovu vytvořte hello řízení databáze.</span><span class="sxs-lookup"><span data-stu-id="1128a-165">This process allows for future upgrades of service code without having toodrop and recreate hello control database.</span></span> <span data-ttu-id="1128a-166">Tento proces lze také v rámci hello stejné verze toomodify hello služby počet virtuálních počítačů velikost nebo hello serveru worker.</span><span class="sxs-lookup"><span data-stu-id="1128a-166">This process can also be used within hello same version toomodify hello service VM size or hello server worker count.</span></span>

<span data-ttu-id="1128a-167">velikost virtuálního počítače tooupdate hello instalace, spusťte hello následující skript s parametry aktualizovat toohello hodnoty podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="1128a-167">tooupdate hello VM size of an installation, run hello following script with parameters updated toohello values of your choice.</span></span>

    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>Unblock-File .\UpdateElasticDatabaseJobs.ps1
    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>.\UpdateElasticDatabaseJobs.ps1 -ServiceVmSize A1 -ServiceWorkerCount 2

<table style="width:100%">
  <tr>
  <th><span data-ttu-id="1128a-168">Parametr</span><span class="sxs-lookup"><span data-stu-id="1128a-168">Parameter</span></span></th>
  <th><span data-ttu-id="1128a-169">Popis</span><span class="sxs-lookup"><span data-stu-id="1128a-169">Description</span></span></th>
</tr>

  <tr>
    <td><span data-ttu-id="1128a-170">Název skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="1128a-170">ResourceGroupName</span></span></td>
    <td><span data-ttu-id="1128a-171">Určuje název skupiny prostředků Azure hello používá při hello elastické databáze úlohy součásti byly původně nainstaloval.</span><span class="sxs-lookup"><span data-stu-id="1128a-171">Identifies hello Azure resource group name used when hello Elastic Database job components were initially installed.</span></span> <span data-ttu-id="1128a-172">Tento parametr výchozí příliš "__ElasticDatabaseJob".</span><span class="sxs-lookup"><span data-stu-id="1128a-172">This parameter defaults too“__ElasticDatabaseJob”.</span></span> <span data-ttu-id="1128a-173">Vzhledem k tomu, že není doporučeno toochange tuto hodnotu toospecify by neměl mít tento parametr.</span><span class="sxs-lookup"><span data-stu-id="1128a-173">Since it is not recommended toochange this value, you shouldn't have toospecify this parameter.</span></span></td>
    </tr>
</tr>

</tr>

  <tr>
    <td><span data-ttu-id="1128a-174">ServiceWorkerCount</span><span class="sxs-lookup"><span data-stu-id="1128a-174">ServiceWorkerCount</span></span></td>
    <td><span data-ttu-id="1128a-175">Poskytuje hello počet tooinstall pracovníky služby.</span><span class="sxs-lookup"><span data-stu-id="1128a-175">Provides hello number of service workers tooinstall.</span></span>  <span data-ttu-id="1128a-176">Tento parametr výchozí too1.</span><span class="sxs-lookup"><span data-stu-id="1128a-176">This parameter defaults too1.</span></span>  <span data-ttu-id="1128a-177">Vyšší počet pracovních procesů může být použité tooscale out hello služby a tooprovide vysokou dostupnost.</span><span class="sxs-lookup"><span data-stu-id="1128a-177">A higher number of workers can be used tooscale out hello service and tooprovide high availability.</span></span>  <span data-ttu-id="1128a-178">Doporučujeme toouse "2" pro nasazení, která vyžadují vysokou dostupnost služby hello.</span><span class="sxs-lookup"><span data-stu-id="1128a-178">It is recommended toouse “2” for deployments that require high availability of hello service.</span></span></td>
</tr>

</tr>

    <tr>
    <td><span data-ttu-id="1128a-179">ServiceVmSize</span><span class="sxs-lookup"><span data-stu-id="1128a-179">ServiceVmSize</span></span></td>
    <td><span data-ttu-id="1128a-180">Poskytuje hello velikost virtuálního počítače pro použití v rámci hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="1128a-180">Provides hello VM size for usage within hello Cloud Service.</span></span> <span data-ttu-id="1128a-181">Tento parametr výchozí tooA0.</span><span class="sxs-lookup"><span data-stu-id="1128a-181">This parameter defaults tooA0.</span></span> <span data-ttu-id="1128a-182">Hodnoty parametrů A0 nebo A1 nebo A2 nebo A3, jsou přijaty které způsobit hello pracovní toouse role pro velikost: mimořádně malý nebo malá nebo střední nebo velké.</span><span class="sxs-lookup"><span data-stu-id="1128a-182">Parameters values of A0/A1/A2/A3 are accepted which cause hello worker role toouse an ExtraSmall/Small/Medium/Large size, respectively.</span></span> <span data-ttu-id="1128a-183">Další informace o velikosti role pracovního procesu, viz Fo [úlohy elastické databáze součásti a ceny](sql-database-elastic-jobs-overview.md#components-and-pricing).</span><span class="sxs-lookup"><span data-stu-id="1128a-183">Fo more information on worker role sizes, see [Elastic Database jobs components and pricing](sql-database-elastic-jobs-overview.md#components-and-pricing).</span></span></td>
</tr>

</table>

## <a name="install-hello-elastic-database-jobs-components-using-hello-portal"></a><span data-ttu-id="1128a-184">Nainstalujte komponenty úlohy elastické databáze hello pomocí hello portálu</span><span class="sxs-lookup"><span data-stu-id="1128a-184">Install hello Elastic Database jobs components using hello Portal</span></span>
<span data-ttu-id="1128a-185">Jakmile máte [vytvoření fondu elastické databáze](sql-database-elastic-pool-manage-portal.md), můžete nainstalovat **úlohy elastické databáze** součásti tooenable provádění úloh správy na každou databázi v hello elastického fondu.</span><span class="sxs-lookup"><span data-stu-id="1128a-185">Once you have [created an elastic pool](sql-database-elastic-pool-manage-portal.md), you can install **Elastic Database jobs** components tooenable execution of administrative tasks against each database in hello elastic pool.</span></span> <span data-ttu-id="1128a-186">Na rozdíl od při použití hello **úlohy elastické databáze** rozhraní API prostředí PowerShell, rozhraní portálu hello je momentálně omezené tooonly provádění na existující fond.</span><span class="sxs-lookup"><span data-stu-id="1128a-186">Unlike when using hello **Elastic Database jobs** PowerShell APIs, hello portal interface is currently restricted tooonly executing against an existing pool.</span></span>

<span data-ttu-id="1128a-187">**Odhadovaný čas toocomplete:** 10 minut.</span><span class="sxs-lookup"><span data-stu-id="1128a-187">**Estimated time toocomplete:** 10 minutes.</span></span>

1. <span data-ttu-id="1128a-188">V zobrazení řídicího panelu hello hello elastického fondu prostřednictvím hello [portálu Azure](https://portal.azure.com/#) , klikněte na tlačítko **vytvořit úlohu**.</span><span class="sxs-lookup"><span data-stu-id="1128a-188">From hello dashboard view of hello elastic pool via hello [Azure Portal](https://portal.azure.com/#) , click **Create job**.</span></span>
2. <span data-ttu-id="1128a-189">Pokud vytváříte úlohu pro hello poprvé, musíte nainstalovat **úlohy elastické databáze** kliknutím **podmínky**.</span><span class="sxs-lookup"><span data-stu-id="1128a-189">If you are creating a job for hello first time, you must install **Elastic Database jobs** by clicking **PREVIEW TERMS**.</span></span>
3. <span data-ttu-id="1128a-190">Přijměte podmínky hello kliknutím hello zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="1128a-190">Accept hello terms by clicking hello checkbox.</span></span>
4. <span data-ttu-id="1128a-191">V zobrazení hello "instalace služby", klikněte na tlačítko **úlohy pověření**.</span><span class="sxs-lookup"><span data-stu-id="1128a-191">In hello "Install services" view, click **JOB CREDENTIALS**.</span></span>
   
    ![Instalace služby hello][1]
5. <span data-ttu-id="1128a-193">Zadejte uživatelské jméno a heslo pro správce databáze Jako součást instalace hello vytvoří se nový server Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="1128a-193">Type a user name and password for a database admin. As part of hello installation, a new Azure SQL Database server is created.</span></span> <span data-ttu-id="1128a-194">Novou databázi, označuje jako hello řízení databáze, v rámci tohoto nového serveru se vytvoří a používá toocontain hello metadata pro úlohy elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="1128a-194">Within this new server, a new database, known as hello control database, is created and used toocontain hello meta data for Elastic Database jobs.</span></span> <span data-ttu-id="1128a-195">Hello uživatelské jméno a heslo vytvořená zde se používají pro účel hello protokolování v databázi toohello ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="1128a-195">hello user name and password created here are used for hello purpose of logging in toohello control database.</span></span> <span data-ttu-id="1128a-196">Samostatné přihlašovací údaje se používá pro spuštění skriptu s databázemi hello v rámci fondu hello.</span><span class="sxs-lookup"><span data-stu-id="1128a-196">A separate credential is used for script execution against hello databases within hello pool.</span></span>
   
    ![Vytvořte uživatelské jméno a heslo][2]
6. <span data-ttu-id="1128a-198">Klikněte na tlačítko OK hello.</span><span class="sxs-lookup"><span data-stu-id="1128a-198">Click hello OK button.</span></span> <span data-ttu-id="1128a-199">Hello součásti jsou vytvořeny pro vás za pár minut v nové [skupiny prostředků](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1128a-199">hello components are created for you in a few minutes in a new [Resource group](../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="1128a-200">Hello novou skupinu prostředků je připnutá toohello spustit Tabule, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="1128a-200">hello new resource group is pinned toohello start board, as shown below.</span></span> <span data-ttu-id="1128a-201">Úlohy po vytvoření, elastické databáze (Cloudová služba, databáze SQL, Service Bus a úložiště) jsou vytvořeny ve skupině hello.</span><span class="sxs-lookup"><span data-stu-id="1128a-201">Once created, elastic database jobs (Cloud Service, SQL Database, Service Bus, and Storage) are all created in hello group.</span></span>
   
    ![Skupina prostředků v Tabule start][3]
7. <span data-ttu-id="1128a-203">Pokud pokus toocreate nebo spravovat úlohy, při instalaci úlohy elastické databáze, pokud poskytuje **pověření** zobrazí se následující zpráva hello.</span><span class="sxs-lookup"><span data-stu-id="1128a-203">If you attempt toocreate or manage a job while elastic database jobs is installing, when providing **Credentials** you will see hello following message.</span></span>
   
    ![Stále probíhá nasazení][4]

<span data-ttu-id="1128a-205">Pokud odinstalaci je potřeba, odstraňte skupinu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="1128a-205">If uninstallation is required, delete hello resource group.</span></span> <span data-ttu-id="1128a-206">V tématu [jak toouninstall hello elastické databáze úlohy součásti](sql-database-elastic-jobs-uninstall.md).</span><span class="sxs-lookup"><span data-stu-id="1128a-206">See [How toouninstall hello Elastic Database job components](sql-database-elastic-jobs-uninstall.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="1128a-207">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1128a-207">Next steps</span></span>
<span data-ttu-id="1128a-208">Zkontrolujte přihlašovací údaje s hello příslušná práva pro spuštění skriptu se vytvoří na každou databázi ve skupině hello, další informace najdete v tématu [zabezpečení databáze SQL](sql-database-manage-logins.md).</span><span class="sxs-lookup"><span data-stu-id="1128a-208">Ensure a credential with hello appropriate rights for script execution is created on each database in hello group, for more information see [Securing your SQL Database](sql-database-manage-logins.md).</span></span>
<span data-ttu-id="1128a-209">V tématu [vytváření a Správa úloh elastické databáze](sql-database-elastic-jobs-create-and-manage.md) tooget spuštěna.</span><span class="sxs-lookup"><span data-stu-id="1128a-209">See [Creating and managing an Elastic Database jobs](sql-database-elastic-jobs-create-and-manage.md) tooget started.</span></span>

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-service-installation/screen-1.png
[2]: ./media/sql-database-elastic-jobs-service-installation/credentials.png
[3]: ./media/sql-database-elastic-jobs-service-installation/start-board.png
[4]: ./media/sql-database-elastic-jobs-service-installation/not-done.png
