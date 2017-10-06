---
title: "aaaCreate a spravovat elastické úlohy pomocí prostředí PowerShell | Microsoft Docs"
description: "Fondy Azure SQL Database toomanage používá prostředí PowerShell"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
ms.assetid: 737d8d13-5632-4e18-9cb0-4d3b8a19e495
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: f6c18aecfa7e8c0b102a3b7cd2f266f5542ae400
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-sql-database-elastic-jobs-using-powershell-preview"></a><span data-ttu-id="feb24-103">Vytvářet a spravovat úlohy elastické databáze SQL pomocí prostředí PowerShell (preview)</span><span class="sxs-lookup"><span data-stu-id="feb24-103">Create and manage SQL Database elastic jobs using PowerShell (preview)</span></span>

<span data-ttu-id="feb24-104">Hello rozhraní API prostředí PowerShell pro **úlohy elastické databáze** (ve verzi preview), umožňují definovat skupiny databází, na které budou spuštěny skripty.</span><span class="sxs-lookup"><span data-stu-id="feb24-104">hello PowerShell APIs for **Elastic Database jobs** (in preview), let you define a group of databases against which scripts will execute.</span></span> <span data-ttu-id="feb24-105">Tento článek ukazuje, jak toocreate a spravovat **úlohy elastické databáze** pomocí rutin prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="feb24-105">This article shows how toocreate and manage **Elastic Database jobs** using PowerShell cmdlets.</span></span> <span data-ttu-id="feb24-106">V tématu [elastické úlohy přehled](sql-database-elastic-jobs-overview.md).</span><span class="sxs-lookup"><span data-stu-id="feb24-106">See [Elastic jobs overview](sql-database-elastic-jobs-overview.md).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="feb24-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="feb24-107">Prerequisites</span></span>
* <span data-ttu-id="feb24-108">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="feb24-108">An Azure subscription.</span></span> <span data-ttu-id="feb24-109">Bezplatná zkušební verze, najdete v části [bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="feb24-109">For a free trial, see [Free one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="feb24-110">Sadu databází, které jsou vytvořené pomocí nástrojů pro elastické databáze hello.</span><span class="sxs-lookup"><span data-stu-id="feb24-110">A set of databases created with hello Elastic Database tools.</span></span> <span data-ttu-id="feb24-111">V tématu [začít pracovat s nástroji elastické databáze](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="feb24-111">See [Get started with Elastic Database tools](sql-database-elastic-scale-get-started.md).</span></span>
* <span data-ttu-id="feb24-112">Azure Powershell</span><span class="sxs-lookup"><span data-stu-id="feb24-112">Azure PowerShell.</span></span> <span data-ttu-id="feb24-113">Podrobné informace najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="feb24-113">For detailed information, see [How tooinstall and configure Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span></span>
* <span data-ttu-id="feb24-114">**Elastické databáze úlohy** balíček prostředí PowerShell: najdete v části [úlohy instalace elastické databáze](sql-database-elastic-jobs-service-installation.md)</span><span class="sxs-lookup"><span data-stu-id="feb24-114">**Elastic Database jobs** PowerShell package: See [Installing Elastic Database jobs](sql-database-elastic-jobs-service-installation.md)</span></span>

### <a name="select-your-azure-subscription"></a><span data-ttu-id="feb24-115">Vyberte předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="feb24-115">Select your Azure subscription</span></span>
<span data-ttu-id="feb24-116">tooselect hello předplatné, je třeba Id předplatného (**- SubscriptionId**) nebo název odběru (**- Název_předplatného**).</span><span class="sxs-lookup"><span data-stu-id="feb24-116">tooselect hello subscription you need your subscription Id (**-SubscriptionId**) or subscription name (**-SubscriptionName**).</span></span> <span data-ttu-id="feb24-117">Pokud máte více předplatných můžete spustit hello **Get-AzureRmSubscription** rutiny a zkopírujte hello potřeby informace o předplatném ze sady výsledků hello.</span><span class="sxs-lookup"><span data-stu-id="feb24-117">If you have multiple subscriptions you can run hello **Get-AzureRmSubscription** cmdlet and copy hello desired subscription information from hello result set.</span></span> <span data-ttu-id="feb24-118">Až budete mít informace o vašem předplatném, spusťte následující příkaz tooset hello toto předplatné jako výchozí hello, konkrétně hello cíl pro vytváření a Správa úloh:</span><span class="sxs-lookup"><span data-stu-id="feb24-118">Once you have your subscription information, run hello following commandlet tooset this subscription as hello default, namely hello target for creating and managing jobs:</span></span>

    Select-AzureRmSubscription -SubscriptionId {SubscriptionID}

<span data-ttu-id="feb24-119">Hello [prostředí PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx) se doporučuje pro využití toodevelop a spustit skripty prostředí PowerShell proti hello úlohy elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="feb24-119">hello [PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx) is recommended for usage toodevelop and execute PowerShell scripts against hello Elastic Database jobs.</span></span>

## <a name="elastic-database-jobs-objects"></a><span data-ttu-id="feb24-120">Objekty úlohy elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="feb24-120">Elastic Database jobs objects</span></span>
<span data-ttu-id="feb24-121">Hello následující tabulce jsou uvedeny na všech typech objekt hello **úlohy elastické databáze** spolu s jeho popis a příslušná rozhraní API prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="feb24-121">hello following table lists out all hello object types of **Elastic Database jobs** along with its description and relevant PowerShell APIs.</span></span>

<table style="width:100%">
  <tr>
    <th><span data-ttu-id="feb24-122">Typ objektu</span><span class="sxs-lookup"><span data-stu-id="feb24-122">Object Type</span></span></th>
    <th><span data-ttu-id="feb24-123">Popis</span><span class="sxs-lookup"><span data-stu-id="feb24-123">Description</span></span></th>
    <th><span data-ttu-id="feb24-124">Související rozhraní API prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="feb24-124">Related PowerShell APIs</span></span></th>
  </tr>
  <tr>
    <td><span data-ttu-id="feb24-125">Přihlašovací údaj</span><span class="sxs-lookup"><span data-stu-id="feb24-125">Credential</span></span></td>
    <td><span data-ttu-id="feb24-126">Uživatelské jméno a heslo toouse při připojování toodatabases pro spouštění skriptů nebo aplikace DACPACs.</span><span class="sxs-lookup"><span data-stu-id="feb24-126">Username and password toouse when connecting toodatabases for execution of scripts or application of DACPACs.</span></span> <p><span data-ttu-id="feb24-127">Hello heslo je zašifrováno před odesláním tooand ukládání do databáze elastické databáze úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="feb24-127">hello password is encrypted before sending tooand storing in hello Elastic Database Jobs database.</span></span>  <span data-ttu-id="feb24-128">hello service úlohy elastické databáze pomocí přihlašovacích údajů hello vytvořen a odesláno z hello instalační skript se dešifrovat heslo Hello.</span><span class="sxs-lookup"><span data-stu-id="feb24-128">hello password is decrypted by hello Elastic Database Jobs service via hello credential created and uploaded from hello installation script.</span></span></td>
    <td><p><span data-ttu-id="feb24-129">Get-AzureSqlJobCredential</span><span class="sxs-lookup"><span data-stu-id="feb24-129">Get-AzureSqlJobCredential</span></span></p>
    <p><span data-ttu-id="feb24-130">Nové AzureSqlJobCredential</span><span class="sxs-lookup"><span data-stu-id="feb24-130">New-AzureSqlJobCredential</span></span></p><p><span data-ttu-id="feb24-131">Set-AzureSqlJobCredential</span><span class="sxs-lookup"><span data-stu-id="feb24-131">Set-AzureSqlJobCredential</span></span></p></td></td>
  </tr>

  <tr>
    <td><span data-ttu-id="feb24-132">Skript</span><span class="sxs-lookup"><span data-stu-id="feb24-132">Script</span></span></td>
    <td><span data-ttu-id="feb24-133">Příkaz Transact-SQL skriptu toobe používané pro spuštění v rámci celé databáze.</span><span class="sxs-lookup"><span data-stu-id="feb24-133">Transact-SQL script toobe used for execution across databases.</span></span>  <span data-ttu-id="feb24-134">Hello skriptu by měl být vytvořené toobe idempotent, protože hello služby bude opakovat akci během spuštění skriptu hello při selhání.</span><span class="sxs-lookup"><span data-stu-id="feb24-134">hello script should be authored toobe idempotent since hello service will retry execution of hello script upon failures.</span></span>
    </td>
    <td>
    <p><span data-ttu-id="feb24-135">Get-AzureSqlJobContent</span><span class="sxs-lookup"><span data-stu-id="feb24-135">Get-AzureSqlJobContent</span></span></p>
    <p><span data-ttu-id="feb24-136">Get-AzureSqlJobContentDefinition</span><span class="sxs-lookup"><span data-stu-id="feb24-136">Get-AzureSqlJobContentDefinition</span></span></p>
    <p><span data-ttu-id="feb24-137">Nové AzureSqlJobContent</span><span class="sxs-lookup"><span data-stu-id="feb24-137">New-AzureSqlJobContent</span></span></p>
    <p><span data-ttu-id="feb24-138">Set-AzureSqlJobContentDefinition</span><span class="sxs-lookup"><span data-stu-id="feb24-138">Set-AzureSqlJobContentDefinition</span></span></p>
    </td>
  </tr>

  <tr>
    <td><span data-ttu-id="feb24-139">DACPAC</span><span class="sxs-lookup"><span data-stu-id="feb24-139">DACPAC</span></span></td>
    <td><span data-ttu-id="feb24-140"><a href="https://msdn.microsoft.com/library/ee210546.aspx">Aplikace na datové vrstvě </a> balíček toobe použít mezi databázemi.</span><span class="sxs-lookup"><span data-stu-id="feb24-140"><a href="https://msdn.microsoft.com/library/ee210546.aspx">Data-tier application </a> package toobe applied across databases.</span></span>

    </td>
    <td>
    <p><span data-ttu-id="feb24-141">Get-AzureSqlJobContent</span><span class="sxs-lookup"><span data-stu-id="feb24-141">Get-AzureSqlJobContent</span></span></p>
    <p><span data-ttu-id="feb24-142">Nové AzureSqlJobContent</span><span class="sxs-lookup"><span data-stu-id="feb24-142">New-AzureSqlJobContent</span></span></p>
    <p><span data-ttu-id="feb24-143">Set-AzureSqlJobContentDefinition</span><span class="sxs-lookup"><span data-stu-id="feb24-143">Set-AzureSqlJobContentDefinition</span></span></p>
    </td>
  </tr>
  <tr>
    <td><span data-ttu-id="feb24-144">Cílové databáze</span><span class="sxs-lookup"><span data-stu-id="feb24-144">Database Target</span></span></td>
    <td><span data-ttu-id="feb24-145">Databáze a serveru název polohovací tooan Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="feb24-145">Database and server name pointing tooan Azure SQL Database.</span></span>

    </td>
    <td>
    <p><span data-ttu-id="feb24-146">Get-AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="feb24-146">Get-AzureSqlJobTarget</span></span></p>
    <p><span data-ttu-id="feb24-147">Nové AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="feb24-147">New-AzureSqlJobTarget</span></span></p>
    </td>
  </tr>
  <tr>
    <td><span data-ttu-id="feb24-148">Cíl horizontálního oddílu mapy</span><span class="sxs-lookup"><span data-stu-id="feb24-148">Shard Map Target</span></span></td>
    <td><span data-ttu-id="feb24-149">Použít kombinaci cíl databáze a přihlašovacích údajů toobe toodetermine informace uložené v rámci mapování horizontálních elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="feb24-149">Combination of a database target and a credential toobe used toodetermine information stored within an Elastic Database shard map.</span></span>
    </td>
    <td>
    <p><span data-ttu-id="feb24-150">Get-AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="feb24-150">Get-AzureSqlJobTarget</span></span></p>
    <p><span data-ttu-id="feb24-151">Nové AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="feb24-151">New-AzureSqlJobTarget</span></span></p>
    <p><span data-ttu-id="feb24-152">Set-AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="feb24-152">Set-AzureSqlJobTarget</span></span></p>
    </td>
  </tr>
<tr>
    <td><span data-ttu-id="feb24-153">Cíl vlastní kolekce</span><span class="sxs-lookup"><span data-stu-id="feb24-153">Custom Collection Target</span></span></td>
    <td><span data-ttu-id="feb24-154">Definované skupiny databází toocollectively použijte pro provedení.</span><span class="sxs-lookup"><span data-stu-id="feb24-154">Defined group of databases toocollectively use for execution.</span></span></td>
    <td>
    <p><span data-ttu-id="feb24-155">Get-AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="feb24-155">Get-AzureSqlJobTarget</span></span></p>
    <p><span data-ttu-id="feb24-156">Nové AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="feb24-156">New-AzureSqlJobTarget</span></span></p>
    </td>
  </tr>
<tr>
    <td><span data-ttu-id="feb24-157">Cíl podřízené vlastní kolekce</span><span class="sxs-lookup"><span data-stu-id="feb24-157">Custom Collection Child Target</span></span></td>
    <td><span data-ttu-id="feb24-158">Cílové databáze, který se odkazuje z vlastní kolekce.</span><span class="sxs-lookup"><span data-stu-id="feb24-158">Database target that is referenced from a custom collection.</span></span></td>
    <td>
    <p><span data-ttu-id="feb24-159">Přidat AzureSqlJobChildTarget</span><span class="sxs-lookup"><span data-stu-id="feb24-159">Add-AzureSqlJobChildTarget</span></span></p>
    <p><span data-ttu-id="feb24-160">Odebrat AzureSqlJobChildTarget</span><span class="sxs-lookup"><span data-stu-id="feb24-160">Remove-AzureSqlJobChildTarget</span></span></p>
    </td>
  </tr>

<tr>
    <td><span data-ttu-id="feb24-161">Úloha</span><span class="sxs-lookup"><span data-stu-id="feb24-161">Job</span></span></td>
    <td>
    <p><span data-ttu-id="feb24-162">Definice parametrů pro úlohu, která se dá použít tootrigger provádění nebo toofulfill plánu.</span><span class="sxs-lookup"><span data-stu-id="feb24-162">Definition of parameters for a job that can be used tootrigger execution or toofulfill a schedule.</span></span></p>
    </td>
    <td>
    <p><span data-ttu-id="feb24-163">Get-AzureSqlJob</span><span class="sxs-lookup"><span data-stu-id="feb24-163">Get-AzureSqlJob</span></span></p>
    <p><span data-ttu-id="feb24-164">Nové AzureSqlJob</span><span class="sxs-lookup"><span data-stu-id="feb24-164">New-AzureSqlJob</span></span></p>
    <p><span data-ttu-id="feb24-165">Set-AzureSqlJob</span><span class="sxs-lookup"><span data-stu-id="feb24-165">Set-AzureSqlJob</span></span></p>
    </td>
  </tr>

<tr>
    <td><span data-ttu-id="feb24-166">Provádění úlohy</span><span class="sxs-lookup"><span data-stu-id="feb24-166">Job Execution</span></span></td>
    <td>
    <p><span data-ttu-id="feb24-167">Kontejner úlohy nezbytné toofulfill provádění skriptu nebo použití cíl tooa DACPAC pomocí přihlašovacích údajů pro připojení databáze s chybami zpracování v souladu zásady spouštění tooan.</span><span class="sxs-lookup"><span data-stu-id="feb24-167">Container of tasks necessary toofulfill either executing a script or applying a DACPAC tooa target using credentials for database connections with failures handled in accordance tooan execution policy.</span></span></p>
    </td>
    <td>
    <p><span data-ttu-id="feb24-168">Get-AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="feb24-168">Get-AzureSqlJobExecution</span></span></p>
    <p><span data-ttu-id="feb24-169">Počáteční AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="feb24-169">Start-AzureSqlJobExecution</span></span></p>
    <p><span data-ttu-id="feb24-170">Stop-AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="feb24-170">Stop-AzureSqlJobExecution</span></span></p>
    <p><span data-ttu-id="feb24-171">Počkejte AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="feb24-171">Wait-AzureSqlJobExecution</span></span></p>
  </tr>

<tr>
    <td><span data-ttu-id="feb24-172">Provádění úloh úkolu</span><span class="sxs-lookup"><span data-stu-id="feb24-172">Job Task Execution</span></span></td>
    <td>
    <p><span data-ttu-id="feb24-173">Jedna jednotka toofulfill pracovní úlohy.</span><span class="sxs-lookup"><span data-stu-id="feb24-173">Single unit of work toofulfill a job.</span></span></p>
    <p><span data-ttu-id="feb24-174">Pokud úkol není možné toosuccessfully spuštění, bude do protokolu hello výsledné zpráva o výjimce a nové odpovídající úkol bude vytvořen a spustit v souladu toohello zadané zásady spouštění.</span><span class="sxs-lookup"><span data-stu-id="feb24-174">If a job task is not able toosuccessfully execute, hello resulting exception message will be logged and a new matching job task will be created and executed in accordance toohello specified execution policy.</span></span></p></p>
    </td>
    <td>
    <p><span data-ttu-id="feb24-175">Get-AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="feb24-175">Get-AzureSqlJobExecution</span></span></p>
    <p><span data-ttu-id="feb24-176">Počáteční AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="feb24-176">Start-AzureSqlJobExecution</span></span></p>
    <p><span data-ttu-id="feb24-177">Stop-AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="feb24-177">Stop-AzureSqlJobExecution</span></span></p>
    <p><span data-ttu-id="feb24-178">Počkejte AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="feb24-178">Wait-AzureSqlJobExecution</span></span></p>
  </tr>

<tr>
    <td><span data-ttu-id="feb24-179">Zásady spouštění úlohy</span><span class="sxs-lookup"><span data-stu-id="feb24-179">Job Execution Policy</span></span></td>
    <td>
    <p><span data-ttu-id="feb24-180">Ovládací prvky úlohy vypršení časových limitů provádění, limity opakování a intervaly mezi opakovanými pokusy.</span><span class="sxs-lookup"><span data-stu-id="feb24-180">Controls job execution timeouts, retry limits and intervals between retries.</span></span></p>
    <p><span data-ttu-id="feb24-181">Elastické databáze úlohy obsahuje výchozí zásady spouštění úlohy, což způsobí, že v podstatě nekonečné opakování selhání úkolů úloh s exponenciálního omezení rychlosti intervalů mezi každou opakování.</span><span class="sxs-lookup"><span data-stu-id="feb24-181">Elastic Database jobs includes a default job execution policy which cause essentially infinite retries of job task failures with exponential backoff of intervals between each retry.</span></span></p>
    </td>
    <td>
    <p><span data-ttu-id="feb24-182">Get-AzureSqlJobExecutionPolicy</span><span class="sxs-lookup"><span data-stu-id="feb24-182">Get-AzureSqlJobExecutionPolicy</span></span></p>
    <p><span data-ttu-id="feb24-183">Nové AzureSqlJobExecutionPolicy</span><span class="sxs-lookup"><span data-stu-id="feb24-183">New-AzureSqlJobExecutionPolicy</span></span></p>
    <p><span data-ttu-id="feb24-184">Set-AzureSqlJobExecutionPolicy</span><span class="sxs-lookup"><span data-stu-id="feb24-184">Set-AzureSqlJobExecutionPolicy</span></span></p>
    </td>
  </tr>

<tr>
    <td><span data-ttu-id="feb24-185">Plán</span><span class="sxs-lookup"><span data-stu-id="feb24-185">Schedule</span></span></td>
    <td>
    <p><span data-ttu-id="feb24-186">Čas na základě specifikace pro provádění tootake místo v opakovaném intervalu nebo najednou.</span><span class="sxs-lookup"><span data-stu-id="feb24-186">Time based specification for execution tootake place either on a reoccurring interval or at a single time.</span></span></p>
    </td>
    <td>
    <p><span data-ttu-id="feb24-187">Get-AzureSqlJobSchedule</span><span class="sxs-lookup"><span data-stu-id="feb24-187">Get-AzureSqlJobSchedule</span></span></p>
    <p><span data-ttu-id="feb24-188">Nové AzureSqlJobSchedule</span><span class="sxs-lookup"><span data-stu-id="feb24-188">New-AzureSqlJobSchedule</span></span></p>
    <p><span data-ttu-id="feb24-189">Set-AzureSqlJobSchedule</span><span class="sxs-lookup"><span data-stu-id="feb24-189">Set-AzureSqlJobSchedule</span></span></p>
    </td>
  </tr>

<tr>
    <td><span data-ttu-id="feb24-190">Aktivační události úlohy</span><span class="sxs-lookup"><span data-stu-id="feb24-190">Job Triggers</span></span></td>
    <td>
    <p><span data-ttu-id="feb24-191">Mapování mezi úlohu a provádění tootrigger úlohy plán, podle plánu toohello.</span><span class="sxs-lookup"><span data-stu-id="feb24-191">A mapping between a job and a schedule tootrigger job execution according toohello schedule.</span></span></p>
    </td>
    <td>
    <p><span data-ttu-id="feb24-192">Nové AzureSqlJobTrigger</span><span class="sxs-lookup"><span data-stu-id="feb24-192">New-AzureSqlJobTrigger</span></span></p>
    <p><span data-ttu-id="feb24-193">Odebrat AzureSqlJobTrigger</span><span class="sxs-lookup"><span data-stu-id="feb24-193">Remove-AzureSqlJobTrigger</span></span></p>
    </td>
  </tr>
</table>

## <a name="supported-elastic-database-jobs-group-types"></a><span data-ttu-id="feb24-194">Podporované úlohy elastické databáze skupiny typy</span><span class="sxs-lookup"><span data-stu-id="feb24-194">Supported Elastic Database jobs group types</span></span>
<span data-ttu-id="feb24-195">Hello úloha spustí skripty jazyka Transact-SQL (T-SQL) nebo aplikace DACPACs napříč skupinou databází.</span><span class="sxs-lookup"><span data-stu-id="feb24-195">hello job executes Transact-SQL (T-SQL) scripts or application of DACPACs across a group of databases.</span></span> <span data-ttu-id="feb24-196">Úlohy po odeslaná toobe provést napříč požadovanou skupinu databází hello úlohy "rozšíří" hello na podřízené úlohy, kde každá má hello vůči jedné databáze ve skupině hello.</span><span class="sxs-lookup"><span data-stu-id="feb24-196">When a job is submitted toobe executed across a group of databases, hello job “expands” hello into child jobs where each performs hello requested execution against a single database in hello group.</span></span> 

<span data-ttu-id="feb24-197">Existují dva typy skupin, které můžete vytvořit:</span><span class="sxs-lookup"><span data-stu-id="feb24-197">There are two types of groups that you can create:</span></span> 

* <span data-ttu-id="feb24-198">[Mapování horizontálních](sql-database-elastic-scale-shard-map-management.md) skupiny: odeslaná tootarget mapu horizontálního oddílu po úlohu dotazuje hello horizontálního oddílu mapy toodetermine jeho aktuální sadu horizontálních oddílů hello úlohy a potom vytvoří podřízené úlohy pro každý horizontálního oddílu v mapě hello horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="feb24-198">[Shard Map](sql-database-elastic-scale-shard-map-management.md) group: When a job is submitted tootarget a shard map, hello job queries hello shard map toodetermine its current set of shards, and then creates child jobs for each shard in hello shard map.</span></span>
* <span data-ttu-id="feb24-199">Vlastní skupiny kolekce: definované vlastní sadu databází.</span><span class="sxs-lookup"><span data-stu-id="feb24-199">Custom Collection group: A custom defined set of databases.</span></span> <span data-ttu-id="feb24-200">Když úloha cílem vlastní kolekce, vytvoří podřízené úlohy pro každou databázi aktuálně v hello vlastní kolekce.</span><span class="sxs-lookup"><span data-stu-id="feb24-200">When a job targets a custom collection, it creates child jobs for each database currently in hello custom collection.</span></span>

## <a name="tooset-hello-elastic-database-jobs-connection"></a><span data-ttu-id="feb24-201">tooset hello připojení úlohy elastické databáze</span><span class="sxs-lookup"><span data-stu-id="feb24-201">tooset hello Elastic Database jobs connection</span></span>
<span data-ttu-id="feb24-202">Připojení musí toobe sady toohello úlohy *řízení databáze* předchozí toousing hello úlohy rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="feb24-202">A connection needs toobe set toohello jobs *control database* prior toousing hello jobs APIs.</span></span> <span data-ttu-id="feb24-203">Tuto rutinu spustíte aktivuje toopop okno přihlašovacích údajů se požaduje hello uživatelské jméno a heslo, které vytvořili při instalaci úlohy elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="feb24-203">Running this cmdlet triggers a credential window toopop up requesting hello user name and password created when installing Elastic Database jobs.</span></span> <span data-ttu-id="feb24-204">Všechny příklady uvedené v tomto tématu předpokládají, že už jsou hotové tento první krok.</span><span class="sxs-lookup"><span data-stu-id="feb24-204">All examples provided within this topic assume that this first step has already been performed.</span></span>

<span data-ttu-id="feb24-205">Otevřete úloh připojení toohello elastické databáze:</span><span class="sxs-lookup"><span data-stu-id="feb24-205">Open a connection toohello Elastic Database jobs:</span></span>

    Use-AzureSqlJobConnection -CurrentAzureSubscription 

## <a name="encrypted-credentials-within-hello-elastic-database-jobs"></a><span data-ttu-id="feb24-206">Zašifrované přihlašovací údaje v rámci úlohy elastické databáze hello</span><span class="sxs-lookup"><span data-stu-id="feb24-206">Encrypted credentials within hello Elastic Database jobs</span></span>
<span data-ttu-id="feb24-207">Přihlašovací údaje databáze lze vložit do úlohy hello *řízení databáze* s jeho heslo šifrované.</span><span class="sxs-lookup"><span data-stu-id="feb24-207">Database credentials can be inserted into hello jobs *control database*  with its password encrypted.</span></span> <span data-ttu-id="feb24-208">Je nutné toostore pověření tooenable úlohy toobe provést později, (pomocí plány úloh).</span><span class="sxs-lookup"><span data-stu-id="feb24-208">It is necessary toostore credentials tooenable jobs toobe executed at a later time, (using job schedules).</span></span>

<span data-ttu-id="feb24-209">Šifrování funguje prostřednictvím certifikát vytvořen jako součást hello instalační skript.</span><span class="sxs-lookup"><span data-stu-id="feb24-209">Encryption works through a certificate created as part of hello installation script.</span></span> <span data-ttu-id="feb24-210">Vytvoří Hello instalační skript a nahrávání hello certifikát do hello Azure Cloud Service k dešifrování hello uložený šifrovaná hesla.</span><span class="sxs-lookup"><span data-stu-id="feb24-210">hello installation script creates and uploads hello certificate into hello Azure Cloud Service for decryption of hello stored encrypted passwords.</span></span> <span data-ttu-id="feb24-211">Cloudová služba Azure Hello později ukládá hello veřejný klíč v rámci úlohy hello *řízení databáze* což umožňuje hello rozhraní API prostředí PowerShell nebo portálu Azure Classic rozhraní tooencrypt poskytnuté heslo bez nutnosti hello certifikátu toobe místně nainstalován.</span><span class="sxs-lookup"><span data-stu-id="feb24-211">hello Azure Cloud Service later stores hello public key within hello jobs *control database* which enables hello PowerShell API or Azure Classic Portal interface tooencrypt a provided password without requiring hello certificate toobe locally installed.</span></span>

<span data-ttu-id="feb24-212">Hello pověření hesla jsou zašifrované a zabezpečené od uživatelů s objekty úloh databáze tooElastic oprávnění jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="feb24-212">hello credential passwords are encrypted and secure from users with read-only access tooElastic Database jobs objects.</span></span> <span data-ttu-id="feb24-213">Ale je možné, uživatel se zlými úmysly s přístup pro čtení a zápis tooElastic databáze úlohy objekty tooextract heslo.</span><span class="sxs-lookup"><span data-stu-id="feb24-213">But it is possible for a malicious user with read-write access tooElastic Database Jobs objects tooextract a password.</span></span> <span data-ttu-id="feb24-214">Přihlašovací údaje jsou navrženou toobe opětovně použít napříč spuštění úlohy.</span><span class="sxs-lookup"><span data-stu-id="feb24-214">Credentials are designed toobe reused across job executions.</span></span> <span data-ttu-id="feb24-215">Přihlašovací údaje jsou předány tootarget databáze, při navazování připojení.</span><span class="sxs-lookup"><span data-stu-id="feb24-215">Credentials are passed tootarget databases when establishing connections.</span></span> <span data-ttu-id="feb24-216">Aktuálně neexistují žádná omezení hello cílové databáze používané pro každý přihlašovací údaje, uživatel se zlými úmysly může přidat cíl databáze pro databázi v rámci hello uživatelem se zlými úmysly.</span><span class="sxs-lookup"><span data-stu-id="feb24-216">There are currently no restrictions on hello target databases used for each credential, malicious user could add a database target for a database under hello malicious user's control.</span></span> <span data-ttu-id="feb24-217">Hello uživatel může následně spustit úlohu cílení na této databázi toogain hello pověření heslo.</span><span class="sxs-lookup"><span data-stu-id="feb24-217">hello user could subsequently start a job targeting this database toogain hello credential's password.</span></span>

<span data-ttu-id="feb24-218">Osvědčené postupy zabezpečení pro úlohy elastické databáze patří:</span><span class="sxs-lookup"><span data-stu-id="feb24-218">Security best practices for Elastic Database jobs include:</span></span>

* <span data-ttu-id="feb24-219">Omezit využití hello rozhraní API tootrusted jednotlivce.</span><span class="sxs-lookup"><span data-stu-id="feb24-219">Limit usage of hello APIs tootrusted individuals.</span></span>
* <span data-ttu-id="feb24-220">Přihlašovací údaje by měl mít hello minimálně úkol hello tooperform nezbytná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="feb24-220">Credentials should have hello least privileges necessary tooperform hello job task.</span></span>  <span data-ttu-id="feb24-221">Další informace si můžete prohlédnout v rámci to [autorizace a oprávnění](https://msdn.microsoft.com/library/bb669084.aspx) článku na webu MSDN SQL Server.</span><span class="sxs-lookup"><span data-stu-id="feb24-221">More information can be seen within this [Authorization and Permissions](https://msdn.microsoft.com/library/bb669084.aspx) SQL Server MSDN article.</span></span>

### <a name="toocreate-an-encrypted-credential-for-job-execution-across-databases"></a><span data-ttu-id="feb24-222">toocreate šifrované pověření pro provádění úlohy mezi databázemi</span><span class="sxs-lookup"><span data-stu-id="feb24-222">toocreate an encrypted credential for job execution across databases</span></span>
<span data-ttu-id="feb24-223">toocreate nový šifrovat přihlašovací údaje, hello [ **rutiny Get-Credential** ](https://technet.microsoft.com/library/hh849815.aspx) vyzve k zadání uživatelského jména a hesla, které lze předat toohello [ **New-AzureSqlJobCredential rutiny**](/powershell/module/elasticdatabasejobs/new-azuresqljobcredential).</span><span class="sxs-lookup"><span data-stu-id="feb24-223">toocreate a new encrypted credential, hello [**Get-Credential cmdlet**](https://technet.microsoft.com/library/hh849815.aspx) prompts for a user name and password that can be passed toohello [**New-AzureSqlJobCredential cmdlet**](/powershell/module/elasticdatabasejobs/new-azuresqljobcredential).</span></span>

    $credentialName = "{Credential Name}"
    $databaseCredential = Get-Credential
    $credential = New-AzureSqlJobCredential -Credential $databaseCredential -CredentialName $credentialName
    Write-Output $credential

### <a name="tooupdate-credentials"></a><span data-ttu-id="feb24-224">přihlašovací údaje tooupdate</span><span class="sxs-lookup"><span data-stu-id="feb24-224">tooupdate credentials</span></span>
<span data-ttu-id="feb24-225">Při změně hesla, použijte hello [ **rutiny Set-AzureSqlJobCredential** ](/powershell/module/elasticdatabasejobs/set-azuresqljobcredential) a sadu hello **CredentialName** parametr.</span><span class="sxs-lookup"><span data-stu-id="feb24-225">When passwords change, use hello [**Set-AzureSqlJobCredential cmdlet**](/powershell/module/elasticdatabasejobs/set-azuresqljobcredential) and set hello **CredentialName** parameter.</span></span>

    $credentialName = "{Credential Name}"
    Set-AzureSqlJobCredential -CredentialName $credentialName -Credential $credential 

## <a name="toodefine-an-elastic-database-shard-map-target"></a><span data-ttu-id="feb24-226">toodefine cíl elastické databáze horizontálního oddílu mapy</span><span class="sxs-lookup"><span data-stu-id="feb24-226">toodefine an Elastic Database shard map target</span></span>
<span data-ttu-id="feb24-227">tooexecute úlohu pro všechny databáze v sadě horizontálního oddílu (vytvořený [klientské knihovny pro elastické databáze](sql-database-elastic-database-client-library.md)), použít mapování horizontálních jako cíl hello databáze.</span><span class="sxs-lookup"><span data-stu-id="feb24-227">tooexecute a job against all databases in a shard set (created using [Elastic Database client library](sql-database-elastic-database-client-library.md)), use a shard map as hello database target.</span></span> <span data-ttu-id="feb24-228">Tento příklad vyžaduje horizontálně dělené aplikace vytvořené pomocí klientské knihovny pro elastické databáze hello.</span><span class="sxs-lookup"><span data-stu-id="feb24-228">This example requires a sharded application created using hello Elastic Database client library.</span></span> <span data-ttu-id="feb24-229">V tématu [Začínáme s ukázkou nástroje elastické databáze](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="feb24-229">See [Getting started with Elastic Database tools sample](sql-database-elastic-scale-get-started.md).</span></span>

<span data-ttu-id="feb24-230">Hello horizontálního oddílu mapa správce databáze musí být nastavena jako databáze cíl a poté mapy hello konkrétní horizontálního oddílu musí být zadány jako cíl.</span><span class="sxs-lookup"><span data-stu-id="feb24-230">hello shard map manager database must be set as a database target and then hello specific shard map must be specified as a target.</span></span>

    $shardMapCredentialName = "{Credential Name}"
    $shardMapDatabaseName = "{ShardMapDatabaseName}" #example: ElasticScaleStarterKit_ShardMapManagerDb
    $shardMapDatabaseServerName = "{ShardMapServerName}"
    $shardMapName = "{MyShardMap}" #example: CustomerIDShardMap
    $shardMapDatabaseTarget = New-AzureSqlJobTarget -DatabaseName $shardMapDatabaseName -ServerName $shardMapDatabaseServerName
    $shardMapTarget = New-AzureSqlJobTarget -ShardMapManagerCredentialName $shardMapCredentialName -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapDatabaseServerName -ShardMapName $shardMapName
    Write-Output $shardMapTarget

## <a name="create-a-t-sql-script-for-execution-across-databases"></a><span data-ttu-id="feb24-231">Vytvoření skriptu T-SQL pro provedení mezi databází</span><span class="sxs-lookup"><span data-stu-id="feb24-231">Create a T-SQL Script for execution across databases</span></span>
<span data-ttu-id="feb24-232">Při vytváření skriptů T-SQL pro spuštění, důrazně doporučujeme toobuild je toobe [idempotent](https://en.wikipedia.org/wiki/Idempotence) a odolné proti selhání.</span><span class="sxs-lookup"><span data-stu-id="feb24-232">When creating T-SQL scripts for execution, it is highly recommended toobuild them toobe [idempotent](https://en.wikipedia.org/wiki/Idempotence) and resilient against failures.</span></span> <span data-ttu-id="feb24-233">Vždy, když dojde k selhání, bez ohledu na to hello klasifikace hello selhání spuštění, bude opakovat úlohy elastické databáze provádění skriptu.</span><span class="sxs-lookup"><span data-stu-id="feb24-233">Elastic Database jobs will retry execution of a script whenever execution encounters a failure, regardless of hello classification of hello failure.</span></span>

<span data-ttu-id="feb24-234">Použití hello [ **rutiny New-AzureSqlJobContent** ](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent) toocreate a uložíte skript pro spuštění a nastavte hello **- ContentName** a **- CommandText**parametry.</span><span class="sxs-lookup"><span data-stu-id="feb24-234">Use hello [**New-AzureSqlJobContent cmdlet**](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent) toocreate and save a script for execution and set hello **-ContentName** and **-CommandText** parameters.</span></span>

    $scriptName = "Create a TestTable"

    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
        CREATE TABLE TestTable(
            TestTableId INT PRIMARY KEY IDENTITY,
            InsertionTime DATETIME2
        );
    END
    GO
    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO"

    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="create-a-new-script-from-a-file"></a><span data-ttu-id="feb24-235">Vytvořit nový skript ze souboru</span><span class="sxs-lookup"><span data-stu-id="feb24-235">Create a new script from a file</span></span>
<span data-ttu-id="feb24-236">Pokud hello skriptu T-SQL je definována v souboru, použijte tento skript tooimport hello:</span><span class="sxs-lookup"><span data-stu-id="feb24-236">If hello T-SQL script is defined within a file, use this tooimport hello script:</span></span>

    $scriptName = "My Script Imported from a File"
    $scriptPath = "{Path tooSQL File}"
    $scriptCommandText = Get-Content -Path $scriptPath
    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="tooupdate-a-t-sql-script-for-execution-across-databases"></a><span data-ttu-id="feb24-237">skript tooupdate T-SQL pro provádění mezi databázemi</span><span class="sxs-lookup"><span data-stu-id="feb24-237">tooupdate a T-SQL script for execution across databases</span></span>
<span data-ttu-id="feb24-238">Tato aktualizace skript prostředí PowerShell text hello text příkazů T-SQL pro existující skript.</span><span class="sxs-lookup"><span data-stu-id="feb24-238">This PowerShell script updates hello T-SQL command text for an existing script.</span></span>

<span data-ttu-id="feb24-239">Sada hello následující proměnné tooreflect hello potřeby sadu toobe definice skriptu:</span><span class="sxs-lookup"><span data-stu-id="feb24-239">Set hello following variables tooreflect hello desired script definition toobe set:</span></span>

    $scriptName = "Create a TestTable"
    $scriptUpdateComment = "Adding AdditionalInformation column tooTestTable"
    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
    CREATE TABLE TestTable(
        TestTableId INT PRIMARY KEY IDENTITY,
        InsertionTime DATETIME2
    );
    END
    GO

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN
    ALTER TABLE TestTable
    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO"

### <a name="tooupdate-hello-definition-tooan-existing-script"></a><span data-ttu-id="feb24-240">tooupdate hello Definice tooan stávajícího skriptu</span><span class="sxs-lookup"><span data-stu-id="feb24-240">tooupdate hello definition tooan existing script</span></span>
    Set-AzureSqlJobContentDefinition -ContentName $scriptName -CommandText $scriptCommandText -Comment $scriptUpdateComment 

## <a name="toocreate-a-job-tooexecute-a-script-across-a-shard-map"></a><span data-ttu-id="feb24-241">toocreate tooexecute úlohy skriptu napříč horizontálního oddílu mapy</span><span class="sxs-lookup"><span data-stu-id="feb24-241">toocreate a job tooexecute a script across a shard map</span></span>
<span data-ttu-id="feb24-242">Tento skript prostředí PowerShell spustí úlohu pro spuštění skriptu mezi každou horizontálního oddílu v mapě horizontálního oddílu služby elastické škálování.</span><span class="sxs-lookup"><span data-stu-id="feb24-242">This PowerShell script starts a job for execution of a script across each shard in an Elastic Scale shard map.</span></span>

<span data-ttu-id="feb24-243">Sada hello následující proměnné tooreflect hello potřeby skriptu a cíle:</span><span class="sxs-lookup"><span data-stu-id="feb24-243">Set hello following variables tooreflect hello desired script and target:</span></span>

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $credentialName = "{Credential Name}"
    $shardMapTarget = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName 
    $job = New-AzureSqlJob -ContentName $scriptName -CredentialName $credentialName -JobName $jobName -TargetId $shardMapTarget.TargetId
    Write-Output $job

## <a name="tooexecute-a-job"></a><span data-ttu-id="feb24-244">tooexecute úlohy</span><span class="sxs-lookup"><span data-stu-id="feb24-244">tooexecute a job</span></span>
<span data-ttu-id="feb24-245">Tento skript PowerShell spouští stávající úloze:</span><span class="sxs-lookup"><span data-stu-id="feb24-245">This PowerShell script executes an existing job:</span></span>

<span data-ttu-id="feb24-246">Aktualizace hello proměnné tooreflect hello potřeby úlohy název toohave provést následující:</span><span class="sxs-lookup"><span data-stu-id="feb24-246">Update hello following variable tooreflect hello desired job name toohave executed:</span></span>

    $jobName = "{Job Name}"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName 
    Write-Output $jobExecution

## <a name="tooretrieve-hello-state-of-a-single-job-execution"></a><span data-ttu-id="feb24-247">Stav hello tooretrieve provádění jedné úlohy</span><span class="sxs-lookup"><span data-stu-id="feb24-247">tooretrieve hello state of a single job execution</span></span>
<span data-ttu-id="feb24-248">Použití hello [ **rutiny Get-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/get-azuresqljobexecution) a sadu hello **JobExecutionId** parametr tooview hello stav provádění úlohy.</span><span class="sxs-lookup"><span data-stu-id="feb24-248">Use hello [**Get-AzureSqlJobExecution cmdlet**](/powershell/module/elasticdatabasejobs/get-azuresqljobexecution) and set hello **JobExecutionId** parameter tooview hello state of job execution.</span></span>

    $jobExecutionId = "{Job Execution Id}"
    $jobExecution = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId
    Write-Output $jobExecution

<span data-ttu-id="feb24-249">Použití hello stejné **Get-AzureSqlJobExecution** rutiny s hello **metoda IncludeChildren** parametr tooview hello stav spuštěních podřízené úlohy, a to hello určitý stav pro každé spuštění úlohy proti jednotlivým databáze cílové úlohou hello.</span><span class="sxs-lookup"><span data-stu-id="feb24-249">Use hello same **Get-AzureSqlJobExecution** cmdlet with hello **IncludeChildren** parameter tooview hello state of child job executions, namely hello specific state for each job execution against each database targeted by hello job.</span></span>

    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions 

## <a name="tooview-hello-state-across-multiple-job-executions"></a><span data-ttu-id="feb24-250">Stav hello tooview mezi jednotlivými spuštěními více úloh</span><span class="sxs-lookup"><span data-stu-id="feb24-250">tooview hello state across multiple job executions</span></span>
<span data-ttu-id="feb24-251">Hello [ **rutiny Get-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/new-azuresqljob) má více volitelné parametry, které se dají použít toodisplay více spuštěních úloh filtrované prostřednictvím hello zadané parametry.</span><span class="sxs-lookup"><span data-stu-id="feb24-251">hello [**Get-AzureSqlJobExecution cmdlet**](/powershell/module/elasticdatabasejobs/new-azuresqljob) has multiple optional parameters that can be used toodisplay multiple job executions, filtered through hello provided parameters.</span></span> <span data-ttu-id="feb24-252">Následující Hello ukazuje některé možné způsoby, jak toouse hello Get-AzureSqlJobExecution:</span><span class="sxs-lookup"><span data-stu-id="feb24-252">hello following demonstrates some of hello possible ways toouse Get-AzureSqlJobExecution:</span></span>

<span data-ttu-id="feb24-253">Načtěte všechny aktivní nejvyšší úrovně úloha spuštění:</span><span class="sxs-lookup"><span data-stu-id="feb24-253">Retrieve all active top level job executions:</span></span>

    Get-AzureSqlJobExecution

<span data-ttu-id="feb24-254">Načtení všech spuštěních nejvyšší úrovně úlohy, včetně spuštěních neaktivní úlohy:</span><span class="sxs-lookup"><span data-stu-id="feb24-254">Retrieve all top level job executions, including inactive job executions:</span></span>

    Get-AzureSqlJobExecution -IncludeInactive

<span data-ttu-id="feb24-255">Načtěte všechny podřízené úlohy spuštěních zadaná úloha spuštění ID, včetně spuštěních neaktivní úlohy:</span><span class="sxs-lookup"><span data-stu-id="feb24-255">Retrieve all child job executions of a provided job execution ID, including inactive job executions:</span></span>

    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId -IncludeInactive -IncludeChildren

<span data-ttu-id="feb24-256">Načíst všechny úlohy spuštěních vytvořený plán / úlohy kombinaci, včetně neaktivní úlohy:</span><span class="sxs-lookup"><span data-stu-id="feb24-256">Retrieve all job executions created using a schedule / job combination, including inactive jobs:</span></span>

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive

<span data-ttu-id="feb24-257">Načtěte všechny úlohy cílení na mapě zadaný horizontálního oddílu, včetně neaktivní úlohy:</span><span class="sxs-lookup"><span data-stu-id="feb24-257">Retrieve all jobs targeting a specified shard map, including inactive jobs:</span></span>

    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive

<span data-ttu-id="feb24-258">Načtěte všechny úlohy cílení na vlastní kolekce, včetně neaktivní úlohy:</span><span class="sxs-lookup"><span data-stu-id="feb24-258">Retrieve all jobs targeting a specified custom collection, including inactive jobs:</span></span>

    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive

<span data-ttu-id="feb24-259">Načtení seznamu hello spuštěních úloh úlohy v rámci provedení určité úlohy:</span><span class="sxs-lookup"><span data-stu-id="feb24-259">Retrieve hello list of job task executions within a specific job execution:</span></span>

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions 

<span data-ttu-id="feb24-260">Načtěte podrobnosti o provádění úkolů úlohy:</span><span class="sxs-lookup"><span data-stu-id="feb24-260">Retrieve job task execution details:</span></span>

<span data-ttu-id="feb24-261">Následující skript prostředí PowerShell Hello lze použít tooview hello podrobnosti o provádění úloh úkolu, který je zvláště užitečná při ladění selhání spuštění.</span><span class="sxs-lookup"><span data-stu-id="feb24-261">hello following PowerShell script can be used tooview hello details of a job task execution, which is particularly useful when debugging execution failures.</span></span>

    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution

## <a name="tooretrieve-failures-within-job-task-executions"></a><span data-ttu-id="feb24-262">tooretrieve selhání v rámci úlohy spuštěních úloh</span><span class="sxs-lookup"><span data-stu-id="feb24-262">tooretrieve failures within job task executions</span></span>
<span data-ttu-id="feb24-263">Hello **JobTaskExecution objekt** zahrnuje vlastnost hello životní cyklus úlohy hello společně s vlastností zpráv.</span><span class="sxs-lookup"><span data-stu-id="feb24-263">hello **JobTaskExecution object** includes a property for hello lifecycle of hello task along with a message property.</span></span> <span data-ttu-id="feb24-264">Pokud se nezdařilo provádění úloh úkolu, vlastnost hello životního cyklu bude nastavena příliš*se nezdařilo* a vlastnost zprávy hello se nastaví toohello výsledné zpráva o výjimce a jeho zásobníku.</span><span class="sxs-lookup"><span data-stu-id="feb24-264">If a job task execution failed, hello lifecycle property will be set too*Failed* and hello message property will be set toohello resulting exception message and its stack.</span></span> <span data-ttu-id="feb24-265">Pokud úloha nebyla úspěšná, je důležité tooview hello podrobnosti úlohy, které se nezdařilo pro danou úlohu.</span><span class="sxs-lookup"><span data-stu-id="feb24-265">If a job did not succeed, it is important tooview hello details of job tasks that did not succeed for a given job.</span></span>

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions) 
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }

## <a name="toowait-for-a-job-execution-toocomplete"></a><span data-ttu-id="feb24-266">toowait pro provádění toocomplete úlohy</span><span class="sxs-lookup"><span data-stu-id="feb24-266">toowait for a job execution toocomplete</span></span>
<span data-ttu-id="feb24-267">Hello následující skript prostředí PowerShell může být použité toowait pro toocomplete úlohy úlohy:</span><span class="sxs-lookup"><span data-stu-id="feb24-267">hello following PowerShell script can be used toowait for a job task toocomplete:</span></span>

    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId 

## <a name="create-a-custom-execution-policy"></a><span data-ttu-id="feb24-268">Vytvořit zásadu vlastní spuštění</span><span class="sxs-lookup"><span data-stu-id="feb24-268">Create a custom execution policy</span></span>
<span data-ttu-id="feb24-269">Elastické databáze úlohy podporuje vytváření vlastní provádění zásad, které mohou být použity při spouštění úloh.</span><span class="sxs-lookup"><span data-stu-id="feb24-269">Elastic Database jobs supports creating custom execution policies that can be applied when starting jobs.</span></span>

<span data-ttu-id="feb24-270">Zásady spouštění aktuálně povolit pro definování:</span><span class="sxs-lookup"><span data-stu-id="feb24-270">Execution policies currently allow for defining:</span></span>

* <span data-ttu-id="feb24-271">Název: Identifikátor pro zásady spouštění hello.</span><span class="sxs-lookup"><span data-stu-id="feb24-271">Name: Identifier for hello execution policy.</span></span>
* <span data-ttu-id="feb24-272">Časový limit úlohy: Celkový čas před úlohy budou zrušeny úlohami elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="feb24-272">Job Timeout: Total time before a job will be canceled by Elastic Database Jobs.</span></span>
* <span data-ttu-id="feb24-273">Počáteční Interval opakování: Interval toowait před první opakování.</span><span class="sxs-lookup"><span data-stu-id="feb24-273">Initial Retry Interval: Interval toowait before first retry.</span></span>
* <span data-ttu-id="feb24-274">Maximální Interval opakování: Cap z toouse intervalech zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="feb24-274">Maximum Retry Interval: Cap of retry intervals toouse.</span></span>
* <span data-ttu-id="feb24-275">Koeficient omezení rychlosti Interval opakování: Koeficient použít toocalculate hello další interval mezi opakovanými pokusy.</span><span class="sxs-lookup"><span data-stu-id="feb24-275">Retry Interval Backoff Coefficient: Coefficient used toocalculate hello next interval between retries.</span></span>  <span data-ttu-id="feb24-276">Hello použije následující vzorec: (počáteční opakujte Interval) * Math.pow ((Interval omezení rychlosti koeficient), (počet pokusů o) - 2).</span><span class="sxs-lookup"><span data-stu-id="feb24-276">hello following formula is used: (Initial Retry Interval) * Math.pow((Interval Backoff Coefficient), (Number of Retries) - 2).</span></span> 
* <span data-ttu-id="feb24-277">Maximální počet pokusů: hello maximální počet opakování pokusů o tooperform v rámci úlohy.</span><span class="sxs-lookup"><span data-stu-id="feb24-277">Maximum Attempts: hello maximum number of retry attempts tooperform within a job.</span></span>

<span data-ttu-id="feb24-278">Zásady spouštění výchozí Hello používá hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="feb24-278">hello default execution policy uses hello following values:</span></span>

* <span data-ttu-id="feb24-279">Název: Zásady spouštění výchozí</span><span class="sxs-lookup"><span data-stu-id="feb24-279">Name: Default execution policy</span></span>
* <span data-ttu-id="feb24-280">Časový limit úlohy: 1 týden</span><span class="sxs-lookup"><span data-stu-id="feb24-280">Job Timeout: 1 week</span></span>
* <span data-ttu-id="feb24-281">Počáteční Interval opakování: 100 milisekund</span><span class="sxs-lookup"><span data-stu-id="feb24-281">Initial Retry Interval:  100 milliseconds</span></span>
* <span data-ttu-id="feb24-282">Maximální Interval opakování: 30 minut</span><span class="sxs-lookup"><span data-stu-id="feb24-282">Maximum Retry Interval: 30 minutes</span></span>
* <span data-ttu-id="feb24-283">Opakujte koeficient Interval: 2</span><span class="sxs-lookup"><span data-stu-id="feb24-283">Retry Interval Coefficient: 2</span></span>
* <span data-ttu-id="feb24-284">Maximální počet pokusů: 2 147 483 647</span><span class="sxs-lookup"><span data-stu-id="feb24-284">Maximum Attempts: 2,147,483,647</span></span>

<span data-ttu-id="feb24-285">Vytvoření zásady spouštění hello potřeby:</span><span class="sxs-lookup"><span data-stu-id="feb24-285">Create hello desired execution policy:</span></span>

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 10
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval 
    -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $executionPolicy

### <a name="update-a-custom-execution-policy"></a><span data-ttu-id="feb24-286">Aktualizovat zásady vlastní spuštění</span><span class="sxs-lookup"><span data-stu-id="feb24-286">Update a custom execution policy</span></span>
<span data-ttu-id="feb24-287">Aktualizace hello potřeby tooupdate zásad spouštění:</span><span class="sxs-lookup"><span data-stu-id="feb24-287">Update hello desired execution policy tooupdate:</span></span>

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 15
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $updatedExecutionPolicy = Set-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $updatedExecutionPolicy

## <a name="cancel-a-job"></a><span data-ttu-id="feb24-288">Zrušení úlohy</span><span class="sxs-lookup"><span data-stu-id="feb24-288">Cancel a job</span></span>
<span data-ttu-id="feb24-289">Elastické databáze úlohy podporuje požadavků na zrušení úloh.</span><span class="sxs-lookup"><span data-stu-id="feb24-289">Elastic Database Jobs supports cancellation requests of jobs.</span></span>  <span data-ttu-id="feb24-290">Pokud úlohy elastické databáze zjistí žádost o zrušení úlohy se spouští, se pokusí toostop hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="feb24-290">If Elastic Database Jobs detects a cancellation request for a job currently being executed, it will attempt toostop hello job.</span></span>

<span data-ttu-id="feb24-291">Že úlohy elastické databáze můžete provést zrušení dvěma různými způsoby:</span><span class="sxs-lookup"><span data-stu-id="feb24-291">There are two different ways that Elastic Database Jobs can perform a cancellation:</span></span>

1. <span data-ttu-id="feb24-292">Zrušit aktuálně spuštěných úloh: Pokud zrušení se zjistí, zatímco úloha je aktuálně spuštěna, zrušení se pokusí v rámci hello aktuálně spuštěných aspekt úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="feb24-292">Cancel currently executing tasks: If a cancellation is detected while a task is currently running, a cancellation will be attempted within hello currently executing aspect of hello task.</span></span>  <span data-ttu-id="feb24-293">Například: Pokud je aktuálně provést při pokusu o zrušení dlouho spuštěných dotazu, bude dotaz pokus o toocancel hello.</span><span class="sxs-lookup"><span data-stu-id="feb24-293">For example: If there is a long running query currently being performed when a cancellation is attempted, there will be an attempt toocancel hello query.</span></span>
2. <span data-ttu-id="feb24-294">Zrušení úloh opakování: v případě zrušení zjištění hello řízení vlákno předtím, než se spustí úloha pro spuštění, bude hello řízení vláken vyhnout, spouštění úloh hello a deklarovat hello žádost jako zrušená.</span><span class="sxs-lookup"><span data-stu-id="feb24-294">Canceling task retries: If a cancellation is detected by hello control thread before a task is launched for execution, hello control thread will avoid launching hello task and declare hello request as canceled.</span></span>

<span data-ttu-id="feb24-295">Pokud zrušení úlohy je požadováno pro nadřazené úloze, bude požadavek na zrušení hello dodržet pro hello nadřazené úloze a všechny jeho podřízené úlohy.</span><span class="sxs-lookup"><span data-stu-id="feb24-295">If a job cancellation is requested for a parent job, hello cancellation request will be honored for hello parent job and for all of its child jobs.</span></span>

<span data-ttu-id="feb24-296">toosubmit žádost o zrušení použít hello [ **rutinu Stop-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) a sadu hello **JobExecutionId** parametr.</span><span class="sxs-lookup"><span data-stu-id="feb24-296">toosubmit a cancellation request, use hello [**Stop-AzureSqlJobExecution cmdlet**](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) and set hello **JobExecutionId** parameter.</span></span>

    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId

## <a name="toodelete-a-job-and-job-history-asynchronously"></a><span data-ttu-id="feb24-297">toodelete úlohy a úlohy historie asynchronně</span><span class="sxs-lookup"><span data-stu-id="feb24-297">toodelete a job and job history asynchronously</span></span>
<span data-ttu-id="feb24-298">Elastické databáze úlohy podporuje asynchronní odstranění úloh.</span><span class="sxs-lookup"><span data-stu-id="feb24-298">Elastic Database jobs supports asynchronous deletion of jobs.</span></span> <span data-ttu-id="feb24-299">Úloha může být označený k odstranění a hello systému odstraní hello úlohy a všechny jeho historie úlohy po dokončení všech spuštěních úloh pro úlohu hello.</span><span class="sxs-lookup"><span data-stu-id="feb24-299">A job can be marked for deletion and hello system will delete hello job and all its job history after all job executions have completed for hello job.</span></span> <span data-ttu-id="feb24-300">Hello systému nebude automaticky zrušit spuštěních aktivní úlohy.</span><span class="sxs-lookup"><span data-stu-id="feb24-300">hello system will not automatically cancel active job executions.</span></span>  

<span data-ttu-id="feb24-301">Vyvolání [ **Stop-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) spuštěních toocancel aktivní úlohy.</span><span class="sxs-lookup"><span data-stu-id="feb24-301">Invoke [**Stop-AzureSqlJobExecution**](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) toocancel active job executions.</span></span>

<span data-ttu-id="feb24-302">Odstranění úlohy tootrigger, použijte hello [ **rutinu Remove-AzureSqlJob** ](/powershell/module/elasticdatabasejobs/remove-azuresqljob) a sadu hello **JobName** parametr.</span><span class="sxs-lookup"><span data-stu-id="feb24-302">tootrigger job deletion, use hello [**Remove-AzureSqlJob cmdlet**](/powershell/module/elasticdatabasejobs/remove-azuresqljob) and set hello **JobName** parameter.</span></span>

    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName

## <a name="toocreate-a-custom-database-target"></a><span data-ttu-id="feb24-303">toocreate cíl vlastní databázi</span><span class="sxs-lookup"><span data-stu-id="feb24-303">toocreate a custom database target</span></span>
<span data-ttu-id="feb24-304">Můžete definovat vlastní databázi cíle pro přímé spouštění nebo pro zahrnutí do skupiny vlastní databázi.</span><span class="sxs-lookup"><span data-stu-id="feb24-304">You can define custom database targets either for direct execution or for inclusion within a custom database group.</span></span> <span data-ttu-id="feb24-305">Například protože **elastické fondy** není dosud podporován přímo pomocí rozhraní API prostředí PowerShell, můžete vytvořit vlastní databázi cíle a cílové kolekce vlastní databázi, který zahrnuje všechny hello databáze ve fondu hello.</span><span class="sxs-lookup"><span data-stu-id="feb24-305">For example, because **elastic pools** are not yet directly supported using PowerShell APIs, you can create a custom database target and custom database collection target which encompasses all hello databases in hello pool.</span></span>

<span data-ttu-id="feb24-306">Nastavte následující informace o databázi proměnné tooreflect hello potřeby hello:</span><span class="sxs-lookup"><span data-stu-id="feb24-306">Set hello following variables tooreflect hello desired database information:</span></span>

    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName 

## <a name="toocreate-a-custom-database-collection-target"></a><span data-ttu-id="feb24-307">toocreate cíl kolekce vlastní databázi</span><span class="sxs-lookup"><span data-stu-id="feb24-307">toocreate a custom database collection target</span></span>
<span data-ttu-id="feb24-308">Použití hello [ **New-AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) toodefine rutiny vlastní databázi kolekce tooenable provádění cílů napříč více definovaných databázových cílů.</span><span class="sxs-lookup"><span data-stu-id="feb24-308">Use hello [**New-AzureSqlJobTarget**](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) cmdlet toodefine a custom database collection target tooenable execution across multiple defined database targets.</span></span> <span data-ttu-id="feb24-309">Po vytvoření databáze skupiny, může být přidružen hello vlastní kolekce cílové databáze.</span><span class="sxs-lookup"><span data-stu-id="feb24-309">After creating a database group, databases can be associated with hello custom collection target.</span></span>

<span data-ttu-id="feb24-310">Nastavte hello následující konfigurace cílového proměnné tooreflect hello požadovanou vlastní kolekce:</span><span class="sxs-lookup"><span data-stu-id="feb24-310">Set hello following variables tooreflect hello desired custom collection target configuration:</span></span>

    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 

### <a name="tooadd-databases-tooa-custom-database-collection-target"></a><span data-ttu-id="feb24-311">tooadd databáze tooa vlastní databázi kolekce cíl</span><span class="sxs-lookup"><span data-stu-id="feb24-311">tooadd databases tooa custom database collection target</span></span>
<span data-ttu-id="feb24-312">tooadd databáze tooa určité vlastní kolekci pomocí hello [ **přidat AzureSqlJobChildTarget** ](/powershell/module/elasticdatabasejobs/add-azuresqljobchildtarget) rutiny.</span><span class="sxs-lookup"><span data-stu-id="feb24-312">tooadd a database tooa specific custom collection use hello [**Add-AzureSqlJobChildTarget**](/powershell/module/elasticdatabasejobs/add-azuresqljobchildtarget) cmdlet.</span></span>

    $databaseServerName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName 

#### <a name="review-hello-databases-within-a-custom-database-collection-target"></a><span data-ttu-id="feb24-313">Zkontrolujte hello databází v rámci kolekce cíl vlastní databázi</span><span class="sxs-lookup"><span data-stu-id="feb24-313">Review hello databases within a custom database collection target</span></span>
<span data-ttu-id="feb24-314">Použití hello [ **Get-AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) rutiny tooretrieve hello podřízené databází v rámci kolekce cíl vlastní databázi.</span><span class="sxs-lookup"><span data-stu-id="feb24-314">Use hello [**Get-AzureSqlJobTarget**](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) cmdlet tooretrieve hello child databases within a custom database collection target.</span></span> 

    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets

### <a name="create-a-job-tooexecute-a-script-across-a-custom-database-collection-target"></a><span data-ttu-id="feb24-315">Vytvořit úlohu tooexecute skript pro cílovou kolekci vlastní databázi</span><span class="sxs-lookup"><span data-stu-id="feb24-315">Create a job tooexecute a script across a custom database collection target</span></span>
<span data-ttu-id="feb24-316">Použití hello [ **New-AzureSqlJob** ](/powershell/module/elasticdatabasejobs/new-azuresqljob) rutiny toocreate úloh pro skupinu databází definované cílovou kolekci vlastní databázi.</span><span class="sxs-lookup"><span data-stu-id="feb24-316">Use hello [**New-AzureSqlJob**](/powershell/module/elasticdatabasejobs/new-azuresqljob) cmdlet toocreate a job against a group of databases defined by a custom database collection target.</span></span> <span data-ttu-id="feb24-317">Úlohy elastické databáze bude rozšiřovat hello úlohy do více podřízených úloh každé příslušné databáze s tooa přidružený cílové kolekce hello vlastní databázi a zkontrolujte, zda je pro každou databázi hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="feb24-317">Elastic Database jobs will expand hello job into multiple child jobs each corresponding tooa database associated with hello custom database collection target and ensure that hello script is executed against each database.</span></span> <span data-ttu-id="feb24-318">Znovu je důležité, aby skripty jsou odolné tooretries idempotent toobe.</span><span class="sxs-lookup"><span data-stu-id="feb24-318">Again, it is important that scripts are idempotent toobe resilient tooretries.</span></span>

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job

## <a name="data-collection-across-databases"></a><span data-ttu-id="feb24-319">Shromažďování dat mezi databázemi</span><span class="sxs-lookup"><span data-stu-id="feb24-319">Data collection across databases</span></span>
<span data-ttu-id="feb24-320">Můžete použít tooexecute úlohu dotazu v rámci skupiny databází a poslat hello výsledky tooa určité tabulky.</span><span class="sxs-lookup"><span data-stu-id="feb24-320">You can use a job tooexecute a query across a group of databases and send hello results tooa specific table.</span></span> <span data-ttu-id="feb24-321">Tabulka Hello můžete položit dotaz na po hello fakt toosee hello výsledků dotazu z každé databáze.</span><span class="sxs-lookup"><span data-stu-id="feb24-321">hello table can be queried after hello fact toosee hello query’s results from each database.</span></span> <span data-ttu-id="feb24-322">To poskytuje asynchronní metody tooexecute dotazu mezi mnoha databázemi.</span><span class="sxs-lookup"><span data-stu-id="feb24-322">This provides an asynchronous method tooexecute a query across many databases.</span></span> <span data-ttu-id="feb24-323">Neúspěšných pokusů o přihlášení se zpracovávají automaticky prostřednictvím opakování.</span><span class="sxs-lookup"><span data-stu-id="feb24-323">Failed attempts are handled automatically via retries.</span></span>

<span data-ttu-id="feb24-324">Hello zadané cílové tabulky se automaticky vytvoří, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="feb24-324">hello specified destination table will be automatically created if it does not yet exist.</span></span> <span data-ttu-id="feb24-325">Nová tabulka Hello odpovídá hello schéma hello vrátila sadu výsledků.</span><span class="sxs-lookup"><span data-stu-id="feb24-325">hello new table matches hello schema of hello returned result set.</span></span> <span data-ttu-id="feb24-326">Pokud skript vrátí více sad výsledků dotazu, úlohy elastické databáze odešle hello první toohello cílové tabulky.</span><span class="sxs-lookup"><span data-stu-id="feb24-326">If a script returns multiple result sets, Elastic Database jobs will only send hello first toohello destination table.</span></span>

<span data-ttu-id="feb24-327">Hello následující skript PowerShell spouští skript a shromažďuje své výsledky do zadané tabulky.</span><span class="sxs-lookup"><span data-stu-id="feb24-327">hello following PowerShell script executes a script and collects its results into a specified table.</span></span> <span data-ttu-id="feb24-328">Tento skript předpokládá, že skriptu T-SQL byl vytvořen který výstupy jedné sadě výsledků a vytvořený cíl kolekce vlastní databázi.</span><span class="sxs-lookup"><span data-stu-id="feb24-328">This script assumes that a T-SQL script has been created which outputs a single result set and that a custom database collection target has been created.</span></span>

<span data-ttu-id="feb24-329">Tento skript používá hello [ **Get-AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) rutiny.</span><span class="sxs-lookup"><span data-stu-id="feb24-329">This script uses hello [**Get-AzureSqlJobTarget**](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) cmdlet.</span></span> <span data-ttu-id="feb24-330">Nastavte hello parametry skriptu, přihlašovací údaje a provádění cíle:</span><span class="sxs-lookup"><span data-stu-id="feb24-330">Set hello parameters for script, credentials, and execution target:</span></span>

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $executionCredentialName = "{Execution Credential Name}"
    $customCollectionName = "{Custom Collection Name}"
    $destinationCredentialName = "{Destination Credential Name}"
    $destinationServerName = "{Destination Server Name}"
    $destinationDatabaseName = "{Destination Database Name}"
    $destinationSchemaName = "{Destination Schema Name}"
    $destinationTableName = "{Destination Table Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName

### <a name="toocreate-and-start-a-job-for-data-collection-scenarios"></a><span data-ttu-id="feb24-331">toocreate a spusťte úlohu pro scénáře kolekce dat</span><span class="sxs-lookup"><span data-stu-id="feb24-331">toocreate and start a job for data collection scenarios</span></span>
<span data-ttu-id="feb24-332">Tento skript používá hello [ **Start-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/start-azuresqljobexecution) rutiny.</span><span class="sxs-lookup"><span data-stu-id="feb24-332">This script uses hello [**Start-AzureSqlJobExecution**](/powershell/module/elasticdatabasejobs/start-azuresqljobexecution) cmdlet.</span></span>

    $job = New-AzureSqlJob -JobName $jobName 
    -CredentialName $executionCredentialName 
    -ContentName $scriptName 
    -ResultSetDestinationServerName $destinationServerName 
    -ResultSetDestinationDatabaseName $destinationDatabaseName 
    -ResultSetDestinationSchemaName $destinationSchemaName 
    -ResultSetDestinationTableName $destinationTableName 
    -ResultSetDestinationCredentialName $destinationCredentialName 
    -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution

## <a name="tooschedule-a-job-execution-trigger"></a><span data-ttu-id="feb24-333">tooschedule provádění aktivační události úlohy</span><span class="sxs-lookup"><span data-stu-id="feb24-333">tooschedule a job execution trigger</span></span>
<span data-ttu-id="feb24-334">Hello následující skript prostředí PowerShell se dá použít toocreate plánu opakování.</span><span class="sxs-lookup"><span data-stu-id="feb24-334">hello following PowerShell script can be used toocreate a recurring schedule.</span></span> <span data-ttu-id="feb24-335">Tento skript používá intervalu minutu, ale [ **New-AzureSqlJobSchedule** ](/powershell/module/elasticdatabasejobs/new-azuresqljobschedule) také podporuje – DayInterval, - HourInterval, - MonthInterval a - WeekInterval parametry.</span><span class="sxs-lookup"><span data-stu-id="feb24-335">This script uses a minute interval, but [**New-AzureSqlJobSchedule**](/powershell/module/elasticdatabasejobs/new-azuresqljobschedule) also supports -DayInterval, -HourInterval, -MonthInterval, and -WeekInterval parameters.</span></span> <span data-ttu-id="feb24-336">Plány, které jsou spouštěny pouze jednou lze vytvořit pomocí předávání - jednorázově.</span><span class="sxs-lookup"><span data-stu-id="feb24-336">Schedules that execute only once can be created by passing -OneTime.</span></span>

<span data-ttu-id="feb24-337">Vytvoření nového plánu:</span><span class="sxs-lookup"><span data-stu-id="feb24-337">Create a new schedule:</span></span>

    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule 
    -MinuteInterval $minuteInterval 
    -ScheduleName $scheduleName 
    -StartTime $startTime 
    Write-Output $schedule

### <a name="tootrigger-a-job-executed-on-a-time-schedule"></a><span data-ttu-id="feb24-338">tootrigger úlohu provést podle časového plánu</span><span class="sxs-lookup"><span data-stu-id="feb24-338">tootrigger a job executed on a time schedule</span></span>
<span data-ttu-id="feb24-339">Aktivační události úlohy může být definovaná toohave časového plánu podle tooa úlohu provést.</span><span class="sxs-lookup"><span data-stu-id="feb24-339">A job trigger can be defined toohave a job executed according tooa time schedule.</span></span> <span data-ttu-id="feb24-340">Následující skript prostředí PowerShell Hello lze použít toocreate aktivační události úlohy.</span><span class="sxs-lookup"><span data-stu-id="feb24-340">hello following PowerShell script can be used toocreate a job trigger.</span></span>

<span data-ttu-id="feb24-341">Použití [New-AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/new-azuresqljobtrigger) a sadu hello následující proměnné toocorrespond toohello požadované úlohy a plán:</span><span class="sxs-lookup"><span data-stu-id="feb24-341">Use [New-AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/new-azuresqljobtrigger) and set hello following variables toocorrespond toohello desired job and schedule:</span></span>

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger
    -ScheduleName $scheduleName
    -JobName $jobName
    Write-Output $jobTrigger

### <a name="tooremove-a-scheduled-association-toostop-job-from-executing-on-schedule"></a><span data-ttu-id="feb24-342">tooremove úlohu naplánované přidružení toostop ve spouštění podle plánu.</span><span class="sxs-lookup"><span data-stu-id="feb24-342">tooremove a scheduled association toostop job from executing on schedule</span></span>
<span data-ttu-id="feb24-343">toodiscontinue nadále provádění úlohy prostřednictvím aktivační události úlohy, aktivační události úlohy hello lze odebrat.</span><span class="sxs-lookup"><span data-stu-id="feb24-343">toodiscontinue reoccurring job execution through a job trigger, hello job trigger can be removed.</span></span> <span data-ttu-id="feb24-344">Odebrání toostop aktivační události úlohy úloha spouštěna podle plánu tooa pomocí hello [ **rutinu Remove-AzureSqlJobTrigger**](/powershell/module/elasticdatabasejobs/remove-azuresqljobtrigger).</span><span class="sxs-lookup"><span data-stu-id="feb24-344">Remove a job trigger toostop a job from being executed according tooa schedule using hello [**Remove-AzureSqlJobTrigger cmdlet**](/powershell/module/elasticdatabasejobs/remove-azuresqljobtrigger).</span></span>

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger 
    -ScheduleName $scheduleName 
    -JobName $jobName

### <a name="retrieve-job-triggers-bound-tooa-time-schedule"></a><span data-ttu-id="feb24-345">Načtení úlohy aktivační události vázané tooa časového plánu</span><span class="sxs-lookup"><span data-stu-id="feb24-345">Retrieve job triggers bound tooa time schedule</span></span>
<span data-ttu-id="feb24-346">Hello následující skript prostředí PowerShell může být použité tooobtain a zobrazit hello úlohy aktivační události registrované tooa konkrétního časového plánu.</span><span class="sxs-lookup"><span data-stu-id="feb24-346">hello following PowerShell script can be used tooobtain and display hello job triggers registered tooa particular time schedule.</span></span>

    $scheduleName = "{Schedule Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -ScheduleName $scheduleName
    Write-Output $jobTriggers

### <a name="tooretrieve-job-triggers-bound-tooa-job"></a><span data-ttu-id="feb24-347">aktivační události úlohy tooretrieve vázaný tooa úlohy</span><span class="sxs-lookup"><span data-stu-id="feb24-347">tooretrieve job triggers bound tooa job</span></span>
<span data-ttu-id="feb24-348">Použití [Get-AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/get-azuresqljobtrigger) plány tooobtain a zobrazení obsahující úlohu registrované.</span><span class="sxs-lookup"><span data-stu-id="feb24-348">Use [Get-AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/get-azuresqljobtrigger) tooobtain and display schedules containing a registered job.</span></span>

    $jobName = "{Job Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -JobName $jobName
    Write-Output $jobTriggers

## <a name="toocreate-a-data-tier-application-dacpac-for-execution-across-databases"></a><span data-ttu-id="feb24-349">toocreate aplikace na datové vrstvě (DACPAC) pro provedení mezi databázemi</span><span class="sxs-lookup"><span data-stu-id="feb24-349">toocreate a data-tier application (DACPAC) for execution across databases</span></span>
<span data-ttu-id="feb24-350">toocreate DACPAC, najdete v části [aplikace datové vrstvy](https://msdn.microsoft.com/library/ee210546.aspx).</span><span class="sxs-lookup"><span data-stu-id="feb24-350">toocreate a DACPAC, see [Data-Tier applications](https://msdn.microsoft.com/library/ee210546.aspx).</span></span> <span data-ttu-id="feb24-351">toodeploy DACPAC, použijte hello [rutiny New-AzureSqlJobContent](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent).</span><span class="sxs-lookup"><span data-stu-id="feb24-351">toodeploy a DACPAC, use hello [New-AzureSqlJobContent cmdlet](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent).</span></span> <span data-ttu-id="feb24-352">Hello DACPAC musí být přístupné toohello služba.</span><span class="sxs-lookup"><span data-stu-id="feb24-352">hello DACPAC must be accessible toohello service.</span></span> <span data-ttu-id="feb24-353">Je doporučeno tooupload vytvořený tooAzure DACPAC úložiště a vytvořte [sdíleného přístupového podpisu](../storage/common/storage-dotnet-shared-access-signature-part-1.md) pro hello DACPAC.</span><span class="sxs-lookup"><span data-stu-id="feb24-353">It is recommended tooupload a created DACPAC tooAzure Storage and create a [Shared Access Signature](../storage/common/storage-dotnet-shared-access-signature-part-1.md) for hello DACPAC.</span></span>

    $dacpacUri = "{Uri}"
    $dacpacName = "{Dacpac Name}"
    $dacpac = New-AzureSqlJobContent -DacpacUri $dacpacUri -ContentName $dacpacName 
    Write-Output $dacpac

### <a name="tooupdate-a-data-tier-application-dacpac-for-execution-across-databases"></a><span data-ttu-id="feb24-354">tooupdate aplikace na datové vrstvě (DACPAC) pro provedení mezi databázemi</span><span class="sxs-lookup"><span data-stu-id="feb24-354">tooupdate a data-tier application (DACPAC) for execution across databases</span></span>
<span data-ttu-id="feb24-355">Existující DACPACs zaregistrován v rámci úlohy elastické databáze může být aktualizované toopoint toonew identifikátory URI.</span><span class="sxs-lookup"><span data-stu-id="feb24-355">Existing DACPACs registered within Elastic Database Jobs can be updated toopoint toonew URIs.</span></span> <span data-ttu-id="feb24-356">Použití hello [ **rutiny Set-AzureSqlJobContentDefinition** ](/powershell/module/elasticdatabasejobs/set-azuresqljobcontentdefinition) tooupdate hello DACPAC URI na existujícím zaregistrován DACPAC:</span><span class="sxs-lookup"><span data-stu-id="feb24-356">Use hello [**Set-AzureSqlJobContentDefinition cmdlet**](/powershell/module/elasticdatabasejobs/set-azuresqljobcontentdefinition) tooupdate hello DACPAC URI on an existing registered DACPAC:</span></span>

    $dacpacName = "{Dacpac Name}"
    $newDacpacUri = "{Uri}"
    $updatedDacpac = Set-AzureSqlJobDacpacDefinition -ContentName $dacpacName -DacpacUri $newDacpacUri
    Write-Output $updatedDacpac

## <a name="toocreate-a-job-tooapply-a-data-tier-application-dacpac-across-databases"></a><span data-ttu-id="feb24-357">toocreate tooapply úlohy aplikace na datové vrstvě (DACPAC) mezi databázemi</span><span class="sxs-lookup"><span data-stu-id="feb24-357">toocreate a job tooapply a data-tier application (DACPAC) across databases</span></span>
<span data-ttu-id="feb24-358">Po vytvoření DACPAC v rámci úlohy elastické databáze, můžete úlohu vytvořit tooapply hello DACPAC napříč skupinou databází.</span><span class="sxs-lookup"><span data-stu-id="feb24-358">After a DACPAC has been created within Elastic Database Jobs, a job can be created tooapply hello DACPAC across a group of databases.</span></span> <span data-ttu-id="feb24-359">Hello následující skript prostředí PowerShell může být použité toocreate úlohu DACPAC v kolekci vlastních databází:</span><span class="sxs-lookup"><span data-stu-id="feb24-359">hello following PowerShell script can be used toocreate a DACPAC job across a custom collection of databases:</span></span>

    $jobName = "{Job Name}"
    $dacpacName = "{Dacpac Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget 
    -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob 
    -JobName $jobName 
    -CredentialName $credentialName 
    -ContentName $dacpacName -TargetId $target.TargetId
    Write-Output $job 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-powershell/cmd-prompt.png
[2]: ./media/sql-database-elastic-jobs-powershell/portal.png
<!--anchors-->
