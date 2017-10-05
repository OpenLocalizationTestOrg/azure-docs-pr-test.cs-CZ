---
title: "Vytvářet a spravovat elastické úlohy pomocí prostředí PowerShell | Microsoft Docs"
description: "Prostředí PowerShell použít ke správě fondů databáze SQL Azure"
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
ms.openlocfilehash: b4c97e8f51581f9a3f7c5a8d8e82562255fe7b48
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-and-manage-sql-database-elastic-jobs-using-powershell-preview"></a><span data-ttu-id="c2784-103">Vytvářet a spravovat úlohy elastické databáze SQL pomocí prostředí PowerShell (preview)</span><span class="sxs-lookup"><span data-stu-id="c2784-103">Create and manage SQL Database elastic jobs using PowerShell (preview)</span></span>

<span data-ttu-id="c2784-104">Rozhraní API prostředí PowerShell pro **úlohy elastické databáze** (ve verzi preview), umožňují definovat skupiny databází, na které budou spuštěny skripty.</span><span class="sxs-lookup"><span data-stu-id="c2784-104">The PowerShell APIs for **Elastic Database jobs** (in preview), let you define a group of databases against which scripts will execute.</span></span> <span data-ttu-id="c2784-105">Tento článek ukazuje, jak vytvořit a spravovat **úlohy elastické databáze** pomocí rutin prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c2784-105">This article shows how to create and manage **Elastic Database jobs** using PowerShell cmdlets.</span></span> <span data-ttu-id="c2784-106">V tématu [elastické úlohy přehled](sql-database-elastic-jobs-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c2784-106">See [Elastic jobs overview](sql-database-elastic-jobs-overview.md).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="c2784-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c2784-107">Prerequisites</span></span>
* <span data-ttu-id="c2784-108">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="c2784-108">An Azure subscription.</span></span> <span data-ttu-id="c2784-109">Bezplatná zkušební verze, najdete v části [bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c2784-109">For a free trial, see [Free one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="c2784-110">Sadu databází, které jsou vytvořené pomocí nástrojů pro elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="c2784-110">A set of databases created with the Elastic Database tools.</span></span> <span data-ttu-id="c2784-111">V tématu [začít pracovat s nástroji elastické databáze](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="c2784-111">See [Get started with Elastic Database tools](sql-database-elastic-scale-get-started.md).</span></span>
* <span data-ttu-id="c2784-112">Azure Powershell</span><span class="sxs-lookup"><span data-stu-id="c2784-112">Azure PowerShell.</span></span> <span data-ttu-id="c2784-113">Podrobné informace najdete v tématu [Instalace a konfigurace prostředí Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c2784-113">For detailed information, see [How to install and configure Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span></span>
* <span data-ttu-id="c2784-114">**Elastické databáze úlohy** balíček prostředí PowerShell: najdete v části [úlohy instalace elastické databáze](sql-database-elastic-jobs-service-installation.md)</span><span class="sxs-lookup"><span data-stu-id="c2784-114">**Elastic Database jobs** PowerShell package: See [Installing Elastic Database jobs](sql-database-elastic-jobs-service-installation.md)</span></span>

### <a name="select-your-azure-subscription"></a><span data-ttu-id="c2784-115">Vyberte předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="c2784-115">Select your Azure subscription</span></span>
<span data-ttu-id="c2784-116">Vyberte předplatné, je třeba Id předplatného (**- SubscriptionId**) nebo název odběru (**- Název_předplatného**).</span><span class="sxs-lookup"><span data-stu-id="c2784-116">To select the subscription you need your subscription Id (**-SubscriptionId**) or subscription name (**-SubscriptionName**).</span></span> <span data-ttu-id="c2784-117">Pokud máte více předplatných můžete spustit **Get-AzureRmSubscription** rutiny a zkopírujte nastavit informace o požadované předplatné od výsledku.</span><span class="sxs-lookup"><span data-stu-id="c2784-117">If you have multiple subscriptions you can run the **Get-AzureRmSubscription** cmdlet and copy the desired subscription information from the result set.</span></span> <span data-ttu-id="c2784-118">Až budete mít informace o vašem předplatném, spusťte následující příkaz pro nastavení toto předplatné jako výchozí, konkrétně cíle pro vytváření a Správa úloh:</span><span class="sxs-lookup"><span data-stu-id="c2784-118">Once you have your subscription information, run the following commandlet to set this subscription as the default, namely the target for creating and managing jobs:</span></span>

    Select-AzureRmSubscription -SubscriptionId {SubscriptionID}

<span data-ttu-id="c2784-119">[Prostředí PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx) se doporučuje pro použití při vývoji a spustit skripty prostředí PowerShell pro úlohy elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="c2784-119">The [PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx) is recommended for usage to develop and execute PowerShell scripts against the Elastic Database jobs.</span></span>

## <a name="elastic-database-jobs-objects"></a><span data-ttu-id="c2784-120">Objekty úlohy elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="c2784-120">Elastic Database jobs objects</span></span>
<span data-ttu-id="c2784-121">V následující tabulce jsou uvedeny na všechny typy objektů z **úlohy elastické databáze** spolu s jeho popis a příslušná rozhraní API prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c2784-121">The following table lists out all the object types of **Elastic Database jobs** along with its description and relevant PowerShell APIs.</span></span>

<table style="width:100%">
  <tr>
    <th><span data-ttu-id="c2784-122">Typ objektu</span><span class="sxs-lookup"><span data-stu-id="c2784-122">Object Type</span></span></th>
    <th><span data-ttu-id="c2784-123">Popis</span><span class="sxs-lookup"><span data-stu-id="c2784-123">Description</span></span></th>
    <th><span data-ttu-id="c2784-124">Související rozhraní API prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="c2784-124">Related PowerShell APIs</span></span></th>
  </tr>
  <tr>
    <td><span data-ttu-id="c2784-125">Přihlašovací údaj</span><span class="sxs-lookup"><span data-stu-id="c2784-125">Credential</span></span></td>
    <td><span data-ttu-id="c2784-126">Uživatelské jméno a heslo používané k připojení k databázím pro spouštění skriptů nebo aplikace DACPACs.</span><span class="sxs-lookup"><span data-stu-id="c2784-126">Username and password to use when connecting to databases for execution of scripts or application of DACPACs.</span></span> <p><span data-ttu-id="c2784-127">Heslo je zašifrováno před odesláním a ukládání do databáze elastické databáze úlohy.</span><span class="sxs-lookup"><span data-stu-id="c2784-127">The password is encrypted before sending to and storing in the Elastic Database Jobs database.</span></span>  <span data-ttu-id="c2784-128">Heslo se dešifruje pomocí služby úlohy elastické databáze pomocí přihlašovacích údajů vytvořen a odesláno z instalační skript.</span><span class="sxs-lookup"><span data-stu-id="c2784-128">The password is decrypted by the Elastic Database Jobs service via the credential created and uploaded from the installation script.</span></span></td>
    <td><p><span data-ttu-id="c2784-129">Get-AzureSqlJobCredential</span><span class="sxs-lookup"><span data-stu-id="c2784-129">Get-AzureSqlJobCredential</span></span></p>
    <p><span data-ttu-id="c2784-130">Nové AzureSqlJobCredential</span><span class="sxs-lookup"><span data-stu-id="c2784-130">New-AzureSqlJobCredential</span></span></p><p><span data-ttu-id="c2784-131">Set-AzureSqlJobCredential</span><span class="sxs-lookup"><span data-stu-id="c2784-131">Set-AzureSqlJobCredential</span></span></p></td></td>
  </tr>

  <tr>
    <td><span data-ttu-id="c2784-132">Skript</span><span class="sxs-lookup"><span data-stu-id="c2784-132">Script</span></span></td>
    <td><span data-ttu-id="c2784-133">Skript Transact-SQL, který se má použít pro provedení mezi databázemi.</span><span class="sxs-lookup"><span data-stu-id="c2784-133">Transact-SQL script to be used for execution across databases.</span></span>  <span data-ttu-id="c2784-134">Skript by měl být vytvořené jako idempotent vzhledem k tomu, že služba bude opakovat akci během provádění skriptu při selhání.</span><span class="sxs-lookup"><span data-stu-id="c2784-134">The script should be authored to be idempotent since the service will retry execution of the script upon failures.</span></span>
    </td>
    <td>
    <p><span data-ttu-id="c2784-135">Get-AzureSqlJobContent</span><span class="sxs-lookup"><span data-stu-id="c2784-135">Get-AzureSqlJobContent</span></span></p>
    <p><span data-ttu-id="c2784-136">Get-AzureSqlJobContentDefinition</span><span class="sxs-lookup"><span data-stu-id="c2784-136">Get-AzureSqlJobContentDefinition</span></span></p>
    <p><span data-ttu-id="c2784-137">Nové AzureSqlJobContent</span><span class="sxs-lookup"><span data-stu-id="c2784-137">New-AzureSqlJobContent</span></span></p>
    <p><span data-ttu-id="c2784-138">Set-AzureSqlJobContentDefinition</span><span class="sxs-lookup"><span data-stu-id="c2784-138">Set-AzureSqlJobContentDefinition</span></span></p>
    </td>
  </tr>

  <tr>
    <td><span data-ttu-id="c2784-139">DACPAC</span><span class="sxs-lookup"><span data-stu-id="c2784-139">DACPAC</span></span></td>
    <td><span data-ttu-id="c2784-140"><a href="https://msdn.microsoft.com/library/ee210546.aspx">Aplikace na datové vrstvě </a> balíčku má být použita mezi databázemi.</span><span class="sxs-lookup"><span data-stu-id="c2784-140"><a href="https://msdn.microsoft.com/library/ee210546.aspx">Data-tier application </a> package to be applied across databases.</span></span>

    </td>
    <td>
    <p><span data-ttu-id="c2784-141">Get-AzureSqlJobContent</span><span class="sxs-lookup"><span data-stu-id="c2784-141">Get-AzureSqlJobContent</span></span></p>
    <p><span data-ttu-id="c2784-142">Nové AzureSqlJobContent</span><span class="sxs-lookup"><span data-stu-id="c2784-142">New-AzureSqlJobContent</span></span></p>
    <p><span data-ttu-id="c2784-143">Set-AzureSqlJobContentDefinition</span><span class="sxs-lookup"><span data-stu-id="c2784-143">Set-AzureSqlJobContentDefinition</span></span></p>
    </td>
  </tr>
  <tr>
    <td><span data-ttu-id="c2784-144">Cílové databáze</span><span class="sxs-lookup"><span data-stu-id="c2784-144">Database Target</span></span></td>
    <td><span data-ttu-id="c2784-145">Název databáze a serveru odkazující na Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="c2784-145">Database and server name pointing to an Azure SQL Database.</span></span>

    </td>
    <td>
    <p><span data-ttu-id="c2784-146">Get-AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="c2784-146">Get-AzureSqlJobTarget</span></span></p>
    <p><span data-ttu-id="c2784-147">Nové AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="c2784-147">New-AzureSqlJobTarget</span></span></p>
    </td>
  </tr>
  <tr>
    <td><span data-ttu-id="c2784-148">Cíl horizontálního oddílu mapy</span><span class="sxs-lookup"><span data-stu-id="c2784-148">Shard Map Target</span></span></td>
    <td><span data-ttu-id="c2784-149">Kombinace cílové databáze a pověření pro použije k určení informací uložených v rámci mapování horizontálních elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="c2784-149">Combination of a database target and a credential to be used to determine information stored within an Elastic Database shard map.</span></span>
    </td>
    <td>
    <p><span data-ttu-id="c2784-150">Get-AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="c2784-150">Get-AzureSqlJobTarget</span></span></p>
    <p><span data-ttu-id="c2784-151">Nové AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="c2784-151">New-AzureSqlJobTarget</span></span></p>
    <p><span data-ttu-id="c2784-152">Set-AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="c2784-152">Set-AzureSqlJobTarget</span></span></p>
    </td>
  </tr>
<tr>
    <td><span data-ttu-id="c2784-153">Cíl vlastní kolekce</span><span class="sxs-lookup"><span data-stu-id="c2784-153">Custom Collection Target</span></span></td>
    <td><span data-ttu-id="c2784-154">Definované skupiny databází souhrnně používat pro provedení.</span><span class="sxs-lookup"><span data-stu-id="c2784-154">Defined group of databases to collectively use for execution.</span></span></td>
    <td>
    <p><span data-ttu-id="c2784-155">Get-AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="c2784-155">Get-AzureSqlJobTarget</span></span></p>
    <p><span data-ttu-id="c2784-156">Nové AzureSqlJobTarget</span><span class="sxs-lookup"><span data-stu-id="c2784-156">New-AzureSqlJobTarget</span></span></p>
    </td>
  </tr>
<tr>
    <td><span data-ttu-id="c2784-157">Cíl podřízené vlastní kolekce</span><span class="sxs-lookup"><span data-stu-id="c2784-157">Custom Collection Child Target</span></span></td>
    <td><span data-ttu-id="c2784-158">Cílové databáze, který se odkazuje z vlastní kolekce.</span><span class="sxs-lookup"><span data-stu-id="c2784-158">Database target that is referenced from a custom collection.</span></span></td>
    <td>
    <p><span data-ttu-id="c2784-159">Přidat AzureSqlJobChildTarget</span><span class="sxs-lookup"><span data-stu-id="c2784-159">Add-AzureSqlJobChildTarget</span></span></p>
    <p><span data-ttu-id="c2784-160">Odebrat AzureSqlJobChildTarget</span><span class="sxs-lookup"><span data-stu-id="c2784-160">Remove-AzureSqlJobChildTarget</span></span></p>
    </td>
  </tr>

<tr>
    <td><span data-ttu-id="c2784-161">Úloha</span><span class="sxs-lookup"><span data-stu-id="c2784-161">Job</span></span></td>
    <td>
    <p><span data-ttu-id="c2784-162">Definice parametrů pro úlohu, která lze použít k aktivaci provádění nebo ke splnění plánu.</span><span class="sxs-lookup"><span data-stu-id="c2784-162">Definition of parameters for a job that can be used to trigger execution or to fulfill a schedule.</span></span></p>
    </td>
    <td>
    <p><span data-ttu-id="c2784-163">Get-AzureSqlJob</span><span class="sxs-lookup"><span data-stu-id="c2784-163">Get-AzureSqlJob</span></span></p>
    <p><span data-ttu-id="c2784-164">Nové AzureSqlJob</span><span class="sxs-lookup"><span data-stu-id="c2784-164">New-AzureSqlJob</span></span></p>
    <p><span data-ttu-id="c2784-165">Set-AzureSqlJob</span><span class="sxs-lookup"><span data-stu-id="c2784-165">Set-AzureSqlJob</span></span></p>
    </td>
  </tr>

<tr>
    <td><span data-ttu-id="c2784-166">Provádění úlohy</span><span class="sxs-lookup"><span data-stu-id="c2784-166">Job Execution</span></span></td>
    <td>
    <p><span data-ttu-id="c2784-167">Kontejner úlohy, které jsou nutné ke splnění provádění skriptu nebo použití DACPAC k cíli pomocí přihlašovacích údajů pro připojení databáze s chybami zpracovány v souladu zásady spouštění.</span><span class="sxs-lookup"><span data-stu-id="c2784-167">Container of tasks necessary to fulfill either executing a script or applying a DACPAC to a target using credentials for database connections with failures handled in accordance to an execution policy.</span></span></p>
    </td>
    <td>
    <p><span data-ttu-id="c2784-168">Get-AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="c2784-168">Get-AzureSqlJobExecution</span></span></p>
    <p><span data-ttu-id="c2784-169">Počáteční AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="c2784-169">Start-AzureSqlJobExecution</span></span></p>
    <p><span data-ttu-id="c2784-170">Stop-AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="c2784-170">Stop-AzureSqlJobExecution</span></span></p>
    <p><span data-ttu-id="c2784-171">Počkejte AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="c2784-171">Wait-AzureSqlJobExecution</span></span></p>
  </tr>

<tr>
    <td><span data-ttu-id="c2784-172">Provádění úloh úkolu</span><span class="sxs-lookup"><span data-stu-id="c2784-172">Job Task Execution</span></span></td>
    <td>
    <p><span data-ttu-id="c2784-173">Jedné jednotky práce ke splnění úlohy.</span><span class="sxs-lookup"><span data-stu-id="c2784-173">Single unit of work to fulfill a job.</span></span></p>
    <p><span data-ttu-id="c2784-174">Pokud úkol není schopna úspěšně provést, bude do protokolu výsledné zpráva o výjimce a nové odpovídající úkol bude vytvořen a provést v souladu zásady zadaný spouštění.</span><span class="sxs-lookup"><span data-stu-id="c2784-174">If a job task is not able to successfully execute, the resulting exception message will be logged and a new matching job task will be created and executed in accordance to the specified execution policy.</span></span></p></p>
    </td>
    <td>
    <p><span data-ttu-id="c2784-175">Get-AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="c2784-175">Get-AzureSqlJobExecution</span></span></p>
    <p><span data-ttu-id="c2784-176">Počáteční AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="c2784-176">Start-AzureSqlJobExecution</span></span></p>
    <p><span data-ttu-id="c2784-177">Stop-AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="c2784-177">Stop-AzureSqlJobExecution</span></span></p>
    <p><span data-ttu-id="c2784-178">Počkejte AzureSqlJobExecution</span><span class="sxs-lookup"><span data-stu-id="c2784-178">Wait-AzureSqlJobExecution</span></span></p>
  </tr>

<tr>
    <td><span data-ttu-id="c2784-179">Zásady spouštění úlohy</span><span class="sxs-lookup"><span data-stu-id="c2784-179">Job Execution Policy</span></span></td>
    <td>
    <p><span data-ttu-id="c2784-180">Ovládací prvky úlohy vypršení časových limitů provádění, limity opakování a intervaly mezi opakovanými pokusy.</span><span class="sxs-lookup"><span data-stu-id="c2784-180">Controls job execution timeouts, retry limits and intervals between retries.</span></span></p>
    <p><span data-ttu-id="c2784-181">Elastické databáze úlohy obsahuje výchozí zásady spouštění úlohy, což způsobí, že v podstatě nekonečné opakování selhání úkolů úloh s exponenciálního omezení rychlosti intervalů mezi každou opakování.</span><span class="sxs-lookup"><span data-stu-id="c2784-181">Elastic Database jobs includes a default job execution policy which cause essentially infinite retries of job task failures with exponential backoff of intervals between each retry.</span></span></p>
    </td>
    <td>
    <p><span data-ttu-id="c2784-182">Get-AzureSqlJobExecutionPolicy</span><span class="sxs-lookup"><span data-stu-id="c2784-182">Get-AzureSqlJobExecutionPolicy</span></span></p>
    <p><span data-ttu-id="c2784-183">Nové AzureSqlJobExecutionPolicy</span><span class="sxs-lookup"><span data-stu-id="c2784-183">New-AzureSqlJobExecutionPolicy</span></span></p>
    <p><span data-ttu-id="c2784-184">Set-AzureSqlJobExecutionPolicy</span><span class="sxs-lookup"><span data-stu-id="c2784-184">Set-AzureSqlJobExecutionPolicy</span></span></p>
    </td>
  </tr>

<tr>
    <td><span data-ttu-id="c2784-185">Plán</span><span class="sxs-lookup"><span data-stu-id="c2784-185">Schedule</span></span></td>
    <td>
    <p><span data-ttu-id="c2784-186">Čas na základě specifikace pro provedení proběhla v opakovaném intervalu nebo najednou.</span><span class="sxs-lookup"><span data-stu-id="c2784-186">Time based specification for execution to take place either on a reoccurring interval or at a single time.</span></span></p>
    </td>
    <td>
    <p><span data-ttu-id="c2784-187">Get-AzureSqlJobSchedule</span><span class="sxs-lookup"><span data-stu-id="c2784-187">Get-AzureSqlJobSchedule</span></span></p>
    <p><span data-ttu-id="c2784-188">Nové AzureSqlJobSchedule</span><span class="sxs-lookup"><span data-stu-id="c2784-188">New-AzureSqlJobSchedule</span></span></p>
    <p><span data-ttu-id="c2784-189">Set-AzureSqlJobSchedule</span><span class="sxs-lookup"><span data-stu-id="c2784-189">Set-AzureSqlJobSchedule</span></span></p>
    </td>
  </tr>

<tr>
    <td><span data-ttu-id="c2784-190">Aktivační události úlohy</span><span class="sxs-lookup"><span data-stu-id="c2784-190">Job Triggers</span></span></td>
    <td>
    <p><span data-ttu-id="c2784-191">Mapování mezi úlohy a plán, který chcete provádění aktivační události úlohy podle plánu.</span><span class="sxs-lookup"><span data-stu-id="c2784-191">A mapping between a job and a schedule to trigger job execution according to the schedule.</span></span></p>
    </td>
    <td>
    <p><span data-ttu-id="c2784-192">Nové AzureSqlJobTrigger</span><span class="sxs-lookup"><span data-stu-id="c2784-192">New-AzureSqlJobTrigger</span></span></p>
    <p><span data-ttu-id="c2784-193">Odebrat AzureSqlJobTrigger</span><span class="sxs-lookup"><span data-stu-id="c2784-193">Remove-AzureSqlJobTrigger</span></span></p>
    </td>
  </tr>
</table>

## <a name="supported-elastic-database-jobs-group-types"></a><span data-ttu-id="c2784-194">Podporované úlohy elastické databáze skupiny typy</span><span class="sxs-lookup"><span data-stu-id="c2784-194">Supported Elastic Database jobs group types</span></span>
<span data-ttu-id="c2784-195">Úloha spustí skripty jazyka Transact-SQL (T-SQL) nebo aplikace DACPACs napříč skupinou databází.</span><span class="sxs-lookup"><span data-stu-id="c2784-195">The job executes Transact-SQL (T-SQL) scripts or application of DACPACs across a group of databases.</span></span> <span data-ttu-id="c2784-196">Když je úloha odeslána spouštění napříč skupinou databází, úloha "rozšíří" do podřízené úlohy, kde každá má požadovaný spuštění na jedné databáze ve skupině.</span><span class="sxs-lookup"><span data-stu-id="c2784-196">When a job is submitted to be executed across a group of databases, the job “expands” the into child jobs where each performs the requested execution against a single database in the group.</span></span> 

<span data-ttu-id="c2784-197">Existují dva typy skupin, které můžete vytvořit:</span><span class="sxs-lookup"><span data-stu-id="c2784-197">There are two types of groups that you can create:</span></span> 

* <span data-ttu-id="c2784-198">[Mapování horizontálních](sql-database-elastic-scale-shard-map-management.md) skupiny: když je úloha odeslána cílovou mapu horizontálního oddílu, dotazuje mapy horizontálního oddílu k určení jeho aktuální sadu horizontálních oddílů úlohy a potom vytvoří podřízené úlohy pro každý horizontálního oddílu v mapě horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="c2784-198">[Shard Map](sql-database-elastic-scale-shard-map-management.md) group: When a job is submitted to target a shard map, the job queries the shard map to determine its current set of shards, and then creates child jobs for each shard in the shard map.</span></span>
* <span data-ttu-id="c2784-199">Vlastní skupiny kolekce: definované vlastní sadu databází.</span><span class="sxs-lookup"><span data-stu-id="c2784-199">Custom Collection group: A custom defined set of databases.</span></span> <span data-ttu-id="c2784-200">Když úloha cílem vlastní kolekce, vytvoří podřízené úlohy pro každou databázi aktuálně v vlastní kolekci.</span><span class="sxs-lookup"><span data-stu-id="c2784-200">When a job targets a custom collection, it creates child jobs for each database currently in the custom collection.</span></span>

## <a name="to-set-the-elastic-database-jobs-connection"></a><span data-ttu-id="c2784-201">Chcete-li nastavit elastické databáze úlohy připojení</span><span class="sxs-lookup"><span data-stu-id="c2784-201">To set the Elastic Database jobs connection</span></span>
<span data-ttu-id="c2784-202">Připojení musí být nastavena na úlohy *řízení databáze* před použitím úlohy rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c2784-202">A connection needs to be set to the jobs *control database* prior to using the jobs APIs.</span></span> <span data-ttu-id="c2784-203">Tuto rutinu spustíte aktivuje okno pověření objevil požaduje uživatelské jméno a heslo, které vytvořili při instalaci úlohy elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="c2784-203">Running this cmdlet triggers a credential window to pop up requesting the user name and password created when installing Elastic Database jobs.</span></span> <span data-ttu-id="c2784-204">Všechny příklady uvedené v tomto tématu předpokládají, že už jsou hotové tento první krok.</span><span class="sxs-lookup"><span data-stu-id="c2784-204">All examples provided within this topic assume that this first step has already been performed.</span></span>

<span data-ttu-id="c2784-205">Otevření připojení do úlohy elastické databáze:</span><span class="sxs-lookup"><span data-stu-id="c2784-205">Open a connection to the Elastic Database jobs:</span></span>

    Use-AzureSqlJobConnection -CurrentAzureSubscription 

## <a name="encrypted-credentials-within-the-elastic-database-jobs"></a><span data-ttu-id="c2784-206">Zašifrované přihlašovací údaje v rámci úlohy elastické databáze</span><span class="sxs-lookup"><span data-stu-id="c2784-206">Encrypted credentials within the Elastic Database jobs</span></span>
<span data-ttu-id="c2784-207">Přihlašovací údaje databáze lze vložit do úlohy *řízení databáze* s jeho heslo šifrované.</span><span class="sxs-lookup"><span data-stu-id="c2784-207">Database credentials can be inserted into the jobs *control database*  with its password encrypted.</span></span> <span data-ttu-id="c2784-208">Je nezbytné k ukládání pověření a umožněte úloh provést později, (pomocí plány úloh).</span><span class="sxs-lookup"><span data-stu-id="c2784-208">It is necessary to store credentials to enable jobs to be executed at a later time, (using job schedules).</span></span>

<span data-ttu-id="c2784-209">Šifrování funguje prostřednictvím certifikát vytvořen jako součást instalační skript.</span><span class="sxs-lookup"><span data-stu-id="c2784-209">Encryption works through a certificate created as part of the installation script.</span></span> <span data-ttu-id="c2784-210">Instalační skript se vytvoří a odešle certifikát do cloudové služby Azure pro dešifrování uložené šifrovaná hesla.</span><span class="sxs-lookup"><span data-stu-id="c2784-210">The installation script creates and uploads the certificate into the Azure Cloud Service for decryption of the stored encrypted passwords.</span></span> <span data-ttu-id="c2784-211">Cloudová služba Azure později ukládá veřejný klíč v rámci úlohy *řízení databáze* což umožňuje rozhraní API prostředí PowerShell nebo portálu Azure Classic k šifrování poskytnuté heslo bez nutnosti certifikát, který má být místně nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="c2784-211">The Azure Cloud Service later stores the public key within the jobs *control database* which enables the PowerShell API or Azure Classic Portal interface to encrypt a provided password without requiring the certificate to be locally installed.</span></span>

<span data-ttu-id="c2784-212">Hesla přihlašovací údaje jsou šifrované a zabezpečení od uživatelů s přístupem jen pro čtení k objektům úlohy elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="c2784-212">The credential passwords are encrypted and secure from users with read-only access to Elastic Database jobs objects.</span></span> <span data-ttu-id="c2784-213">Ale je možné, uživatel se zlými úmysly přístup pro čtení a zápis k objektům elastické databáze úlohy extrahování heslo.</span><span class="sxs-lookup"><span data-stu-id="c2784-213">But it is possible for a malicious user with read-write access to Elastic Database Jobs objects to extract a password.</span></span> <span data-ttu-id="c2784-214">Přihlašovací údaje jsou navrženy pro opětovné použití mezi jednotlivými spuštěními úlohy.</span><span class="sxs-lookup"><span data-stu-id="c2784-214">Credentials are designed to be reused across job executions.</span></span> <span data-ttu-id="c2784-215">Přihlašovací údaje jsou předány k cílovým databázím při navazování připojení.</span><span class="sxs-lookup"><span data-stu-id="c2784-215">Credentials are passed to target databases when establishing connections.</span></span> <span data-ttu-id="c2784-216">Aktuálně neexistují žádná omezení na cílové databáze používané pro každý přihlašovací údaje, uživatel se zlými úmysly může přidat cíl databáze pro databázi pod kontrolou uživateli se zlými úmysly.</span><span class="sxs-lookup"><span data-stu-id="c2784-216">There are currently no restrictions on the target databases used for each credential, malicious user could add a database target for a database under the malicious user's control.</span></span> <span data-ttu-id="c2784-217">Uživatel může následně spustit úlohu cílení na tuto databázi k získání hesla přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="c2784-217">The user could subsequently start a job targeting this database to gain the credential's password.</span></span>

<span data-ttu-id="c2784-218">Osvědčené postupy zabezpečení pro úlohy elastické databáze patří:</span><span class="sxs-lookup"><span data-stu-id="c2784-218">Security best practices for Elastic Database jobs include:</span></span>

* <span data-ttu-id="c2784-219">Omezit využití rozhraní API důvěryhodným osobám.</span><span class="sxs-lookup"><span data-stu-id="c2784-219">Limit usage of the APIs to trusted individuals.</span></span>
* <span data-ttu-id="c2784-220">Přihlašovací údaje by měl mít alespoň oprávnění potřebná k provedení úlohy projektu.</span><span class="sxs-lookup"><span data-stu-id="c2784-220">Credentials should have the least privileges necessary to perform the job task.</span></span>  <span data-ttu-id="c2784-221">Další informace si můžete prohlédnout v rámci to [autorizace a oprávnění](https://msdn.microsoft.com/library/bb669084.aspx) článku na webu MSDN SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c2784-221">More information can be seen within this [Authorization and Permissions](https://msdn.microsoft.com/library/bb669084.aspx) SQL Server MSDN article.</span></span>

### <a name="to-create-an-encrypted-credential-for-job-execution-across-databases"></a><span data-ttu-id="c2784-222">Chcete-li vytvořit šifrovaný pověření pro provádění úlohy mezi databázemi</span><span class="sxs-lookup"><span data-stu-id="c2784-222">To create an encrypted credential for job execution across databases</span></span>
<span data-ttu-id="c2784-223">K vytvoření nových přihlašovacích údajů šifrovaná, [ **rutiny Get-Credential** ](https://technet.microsoft.com/library/hh849815.aspx) vyzve k zadání uživatelského jména a hesla, které lze předat [ **rutiny New-AzureSqlJobCredential** ](/powershell/module/elasticdatabasejobs/new-azuresqljobcredential).</span><span class="sxs-lookup"><span data-stu-id="c2784-223">To create a new encrypted credential, the [**Get-Credential cmdlet**](https://technet.microsoft.com/library/hh849815.aspx) prompts for a user name and password that can be passed to the [**New-AzureSqlJobCredential cmdlet**](/powershell/module/elasticdatabasejobs/new-azuresqljobcredential).</span></span>

    $credentialName = "{Credential Name}"
    $databaseCredential = Get-Credential
    $credential = New-AzureSqlJobCredential -Credential $databaseCredential -CredentialName $credentialName
    Write-Output $credential

### <a name="to-update-credentials"></a><span data-ttu-id="c2784-224">Chcete-li aktualizovat přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="c2784-224">To update credentials</span></span>
<span data-ttu-id="c2784-225">Při změně hesla, použijte [ **rutiny Set-AzureSqlJobCredential** ](/powershell/module/elasticdatabasejobs/set-azuresqljobcredential) a nastavte **CredentialName** parametr.</span><span class="sxs-lookup"><span data-stu-id="c2784-225">When passwords change, use the [**Set-AzureSqlJobCredential cmdlet**](/powershell/module/elasticdatabasejobs/set-azuresqljobcredential) and set the **CredentialName** parameter.</span></span>

    $credentialName = "{Credential Name}"
    Set-AzureSqlJobCredential -CredentialName $credentialName -Credential $credential 

## <a name="to-define-an-elastic-database-shard-map-target"></a><span data-ttu-id="c2784-226">Chcete-li definovat cíl elastické databáze horizontálního oddílu mapy</span><span class="sxs-lookup"><span data-stu-id="c2784-226">To define an Elastic Database shard map target</span></span>
<span data-ttu-id="c2784-227">K provedení úlohy pro všechny databáze v sadě horizontálního oddílu (vytvořený [klientské knihovny pro elastické databáze](sql-database-elastic-database-client-library.md)), použít mapování horizontálních jako cílové databáze.</span><span class="sxs-lookup"><span data-stu-id="c2784-227">To execute a job against all databases in a shard set (created using [Elastic Database client library](sql-database-elastic-database-client-library.md)), use a shard map as the database target.</span></span> <span data-ttu-id="c2784-228">Tento příklad vyžaduje horizontálně dělené aplikace vytvořené pomocí klientské knihovny pro elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="c2784-228">This example requires a sharded application created using the Elastic Database client library.</span></span> <span data-ttu-id="c2784-229">V tématu [Začínáme s ukázkou nástroje elastické databáze](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="c2784-229">See [Getting started with Elastic Database tools sample](sql-database-elastic-scale-get-started.md).</span></span>

<span data-ttu-id="c2784-230">Databáze manager mapy horizontálního oddílu musí být nastavena jako databáze cíl a poté mapy konkrétní horizontálního oddílu musí být zadány jako cíl.</span><span class="sxs-lookup"><span data-stu-id="c2784-230">The shard map manager database must be set as a database target and then the specific shard map must be specified as a target.</span></span>

    $shardMapCredentialName = "{Credential Name}"
    $shardMapDatabaseName = "{ShardMapDatabaseName}" #example: ElasticScaleStarterKit_ShardMapManagerDb
    $shardMapDatabaseServerName = "{ShardMapServerName}"
    $shardMapName = "{MyShardMap}" #example: CustomerIDShardMap
    $shardMapDatabaseTarget = New-AzureSqlJobTarget -DatabaseName $shardMapDatabaseName -ServerName $shardMapDatabaseServerName
    $shardMapTarget = New-AzureSqlJobTarget -ShardMapManagerCredentialName $shardMapCredentialName -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapDatabaseServerName -ShardMapName $shardMapName
    Write-Output $shardMapTarget

## <a name="create-a-t-sql-script-for-execution-across-databases"></a><span data-ttu-id="c2784-231">Vytvoření skriptu T-SQL pro provedení mezi databází</span><span class="sxs-lookup"><span data-stu-id="c2784-231">Create a T-SQL Script for execution across databases</span></span>
<span data-ttu-id="c2784-232">Při vytváření skriptů T-SQL pro spuštění, důrazně doporučujeme vytvořit mají být [idempotent](https://en.wikipedia.org/wiki/Idempotence) a odolné proti selhání.</span><span class="sxs-lookup"><span data-stu-id="c2784-232">When creating T-SQL scripts for execution, it is highly recommended to build them to be [idempotent](https://en.wikipedia.org/wiki/Idempotence) and resilient against failures.</span></span> <span data-ttu-id="c2784-233">Vždy, když dojde k selhání, bez ohledu na klasifikaci selhání spuštění, bude opakovat úlohy elastické databáze provádění skriptu.</span><span class="sxs-lookup"><span data-stu-id="c2784-233">Elastic Database jobs will retry execution of a script whenever execution encounters a failure, regardless of the classification of the failure.</span></span>

<span data-ttu-id="c2784-234">Použití [ **rutiny New-AzureSqlJobContent** ](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent) vytvoříte a uložíte skript pro spuštění a sadu **- ContentName** a **- CommandText** Parametry.</span><span class="sxs-lookup"><span data-stu-id="c2784-234">Use the [**New-AzureSqlJobContent cmdlet**](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent) to create and save a script for execution and set the **-ContentName** and **-CommandText** parameters.</span></span>

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

### <a name="create-a-new-script-from-a-file"></a><span data-ttu-id="c2784-235">Vytvořit nový skript ze souboru</span><span class="sxs-lookup"><span data-stu-id="c2784-235">Create a new script from a file</span></span>
<span data-ttu-id="c2784-236">Pokud skriptu T-SQL je definována v souboru, můžete to použijte import skript:</span><span class="sxs-lookup"><span data-stu-id="c2784-236">If the T-SQL script is defined within a file, use this to import the script:</span></span>

    $scriptName = "My Script Imported from a File"
    $scriptPath = "{Path to SQL File}"
    $scriptCommandText = Get-Content -Path $scriptPath
    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="to-update-a-t-sql-script-for-execution-across-databases"></a><span data-ttu-id="c2784-237">Chcete-li aktualizovat skriptu T-SQL pro provedení mezi databázemi</span><span class="sxs-lookup"><span data-stu-id="c2784-237">To update a T-SQL script for execution across databases</span></span>
<span data-ttu-id="c2784-238">Tento skript prostředí PowerShell aktualizuje text příkazu T-SQL pro existující skript.</span><span class="sxs-lookup"><span data-stu-id="c2784-238">This PowerShell script updates the T-SQL command text for an existing script.</span></span>

<span data-ttu-id="c2784-239">Nastavte následující proměnné tak, aby odrážela definici skriptu požadované nastavení:</span><span class="sxs-lookup"><span data-stu-id="c2784-239">Set the following variables to reflect the desired script definition to be set:</span></span>

    $scriptName = "Create a TestTable"
    $scriptUpdateComment = "Adding AdditionalInformation column to TestTable"
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

### <a name="to-update-the-definition-to-an-existing-script"></a><span data-ttu-id="c2784-240">Aktualizovat definici do existujícího skriptu</span><span class="sxs-lookup"><span data-stu-id="c2784-240">To update the definition to an existing script</span></span>
    Set-AzureSqlJobContentDefinition -ContentName $scriptName -CommandText $scriptCommandText -Comment $scriptUpdateComment 

## <a name="to-create-a-job-to-execute-a-script-across-a-shard-map"></a><span data-ttu-id="c2784-241">Chcete-li vytvořit úlohu pro spuštění skriptu napříč horizontálního oddílu mapy</span><span class="sxs-lookup"><span data-stu-id="c2784-241">To create a job to execute a script across a shard map</span></span>
<span data-ttu-id="c2784-242">Tento skript prostředí PowerShell spustí úlohu pro spuštění skriptu mezi každou horizontálního oddílu v mapě horizontálního oddílu služby elastické škálování.</span><span class="sxs-lookup"><span data-stu-id="c2784-242">This PowerShell script starts a job for execution of a script across each shard in an Elastic Scale shard map.</span></span>

<span data-ttu-id="c2784-243">Nastavte následující proměnné tak, aby odrážela požadované skriptu a cíle:</span><span class="sxs-lookup"><span data-stu-id="c2784-243">Set the following variables to reflect the desired script and target:</span></span>

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $credentialName = "{Credential Name}"
    $shardMapTarget = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName 
    $job = New-AzureSqlJob -ContentName $scriptName -CredentialName $credentialName -JobName $jobName -TargetId $shardMapTarget.TargetId
    Write-Output $job

## <a name="to-execute-a-job"></a><span data-ttu-id="c2784-244">K provedení úlohy</span><span class="sxs-lookup"><span data-stu-id="c2784-244">To execute a job</span></span>
<span data-ttu-id="c2784-245">Tento skript PowerShell spouští stávající úloze:</span><span class="sxs-lookup"><span data-stu-id="c2784-245">This PowerShell script executes an existing job:</span></span>

<span data-ttu-id="c2784-246">Aktualizujte tak, aby odrážela název požadované úlohy, který provedli následující proměnnou:</span><span class="sxs-lookup"><span data-stu-id="c2784-246">Update the following variable to reflect the desired job name to have executed:</span></span>

    $jobName = "{Job Name}"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName 
    Write-Output $jobExecution

## <a name="to-retrieve-the-state-of-a-single-job-execution"></a><span data-ttu-id="c2784-247">Při načítání stavu provádění jedné úlohy</span><span class="sxs-lookup"><span data-stu-id="c2784-247">To retrieve the state of a single job execution</span></span>
<span data-ttu-id="c2784-248">Použití [ **rutiny Get-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/get-azuresqljobexecution) a nastavte **JobExecutionId** parametr, pokud chcete zobrazit stav provádění úlohy.</span><span class="sxs-lookup"><span data-stu-id="c2784-248">Use the [**Get-AzureSqlJobExecution cmdlet**](/powershell/module/elasticdatabasejobs/get-azuresqljobexecution) and set the **JobExecutionId** parameter to view the state of job execution.</span></span>

    $jobExecutionId = "{Job Execution Id}"
    $jobExecution = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId
    Write-Output $jobExecution

<span data-ttu-id="c2784-249">Použijte stejný **Get-AzureSqlJobExecution** rutiny s **metoda IncludeChildren** parametr, pokud chcete zobrazit stav podřízených spuštění úlohy, konkrétně určitém stavu pro každé spuštění úlohy každou databázi cílem úlohy.</span><span class="sxs-lookup"><span data-stu-id="c2784-249">Use the same **Get-AzureSqlJobExecution** cmdlet with the **IncludeChildren** parameter to view the state of child job executions, namely the specific state for each job execution against each database targeted by the job.</span></span>

    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions 

## <a name="to-view-the-state-across-multiple-job-executions"></a><span data-ttu-id="c2784-250">Pokud chcete zobrazit stav mezi jednotlivými spuštěními více úloh</span><span class="sxs-lookup"><span data-stu-id="c2784-250">To view the state across multiple job executions</span></span>
<span data-ttu-id="c2784-251">[ **Rutiny Get-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/new-azuresqljob) má více volitelné parametry, které lze použít k zobrazení více spuštění úlohy, filtrovaný pomocí zadané parametry.</span><span class="sxs-lookup"><span data-stu-id="c2784-251">The [**Get-AzureSqlJobExecution cmdlet**](/powershell/module/elasticdatabasejobs/new-azuresqljob) has multiple optional parameters that can be used to display multiple job executions, filtered through the provided parameters.</span></span> <span data-ttu-id="c2784-252">Následující ukazuje některé možné způsoby, jak používat Get-AzureSqlJobExecution:</span><span class="sxs-lookup"><span data-stu-id="c2784-252">The following demonstrates some of the possible ways to use Get-AzureSqlJobExecution:</span></span>

<span data-ttu-id="c2784-253">Načtěte všechny aktivní nejvyšší úrovně úloha spuštění:</span><span class="sxs-lookup"><span data-stu-id="c2784-253">Retrieve all active top level job executions:</span></span>

    Get-AzureSqlJobExecution

<span data-ttu-id="c2784-254">Načtení všech spuštěních nejvyšší úrovně úlohy, včetně spuštěních neaktivní úlohy:</span><span class="sxs-lookup"><span data-stu-id="c2784-254">Retrieve all top level job executions, including inactive job executions:</span></span>

    Get-AzureSqlJobExecution -IncludeInactive

<span data-ttu-id="c2784-255">Načtěte všechny podřízené úlohy spuštěních zadaná úloha spuštění ID, včetně spuštěních neaktivní úlohy:</span><span class="sxs-lookup"><span data-stu-id="c2784-255">Retrieve all child job executions of a provided job execution ID, including inactive job executions:</span></span>

    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId -IncludeInactive -IncludeChildren

<span data-ttu-id="c2784-256">Načíst všechny úlohy spuštěních vytvořený plán / úlohy kombinaci, včetně neaktivní úlohy:</span><span class="sxs-lookup"><span data-stu-id="c2784-256">Retrieve all job executions created using a schedule / job combination, including inactive jobs:</span></span>

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive

<span data-ttu-id="c2784-257">Načtěte všechny úlohy cílení na mapě zadaný horizontálního oddílu, včetně neaktivní úlohy:</span><span class="sxs-lookup"><span data-stu-id="c2784-257">Retrieve all jobs targeting a specified shard map, including inactive jobs:</span></span>

    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive

<span data-ttu-id="c2784-258">Načtěte všechny úlohy cílení na vlastní kolekce, včetně neaktivní úlohy:</span><span class="sxs-lookup"><span data-stu-id="c2784-258">Retrieve all jobs targeting a specified custom collection, including inactive jobs:</span></span>

    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId -IncludeInactive

<span data-ttu-id="c2784-259">Načtení seznamu spuštěních úloh úlohy v rámci provedení určité úlohy:</span><span class="sxs-lookup"><span data-stu-id="c2784-259">Retrieve the list of job task executions within a specific job execution:</span></span>

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions 

<span data-ttu-id="c2784-260">Načtěte podrobnosti o provádění úkolů úlohy:</span><span class="sxs-lookup"><span data-stu-id="c2784-260">Retrieve job task execution details:</span></span>

<span data-ttu-id="c2784-261">Následující skript prostředí PowerShell slouží k zobrazení podrobností o provádění úloh úkolu, který je zvláště užitečná při ladění selhání spuštění.</span><span class="sxs-lookup"><span data-stu-id="c2784-261">The following PowerShell script can be used to view the details of a job task execution, which is particularly useful when debugging execution failures.</span></span>

    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution

## <a name="to-retrieve-failures-within-job-task-executions"></a><span data-ttu-id="c2784-262">Načtení selhání v rámci úlohy spuštěních úloh</span><span class="sxs-lookup"><span data-stu-id="c2784-262">To retrieve failures within job task executions</span></span>
<span data-ttu-id="c2784-263">**JobTaskExecution objekt** zahrnuje vlastnost pro životní cyklus úlohy společně s vlastností zpráv.</span><span class="sxs-lookup"><span data-stu-id="c2784-263">The **JobTaskExecution object** includes a property for the lifecycle of the task along with a message property.</span></span> <span data-ttu-id="c2784-264">Pokud se nezdařilo provádění úloh úkolu, vlastnost životního cyklu bude nutné nastavit *se nezdařilo* a vlastnosti zprávy se nastaví výsledné zpráva o výjimce a jeho zásobníku.</span><span class="sxs-lookup"><span data-stu-id="c2784-264">If a job task execution failed, the lifecycle property will be set to *Failed* and the message property will be set to the resulting exception message and its stack.</span></span> <span data-ttu-id="c2784-265">Pokud úloha nebyla úspěšná, je důležité k zobrazení podrobností úlohy, které se nezdařilo pro danou úlohu.</span><span class="sxs-lookup"><span data-stu-id="c2784-265">If a job did not succeed, it is important to view the details of job tasks that did not succeed for a given job.</span></span>

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions) 
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }

## <a name="to-wait-for-a-job-execution-to-complete"></a><span data-ttu-id="c2784-266">Čekání na dokončení provedení úlohy</span><span class="sxs-lookup"><span data-stu-id="c2784-266">To wait for a job execution to complete</span></span>
<span data-ttu-id="c2784-267">Následující skript prostředí PowerShell umožňuje počkejte na dokončení úlohy úlohy:</span><span class="sxs-lookup"><span data-stu-id="c2784-267">The following PowerShell script can be used to wait for a job task to complete:</span></span>

    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId 

## <a name="create-a-custom-execution-policy"></a><span data-ttu-id="c2784-268">Vytvořit zásadu vlastní spuštění</span><span class="sxs-lookup"><span data-stu-id="c2784-268">Create a custom execution policy</span></span>
<span data-ttu-id="c2784-269">Elastické databáze úlohy podporuje vytváření vlastní provádění zásad, které mohou být použity při spouštění úloh.</span><span class="sxs-lookup"><span data-stu-id="c2784-269">Elastic Database jobs supports creating custom execution policies that can be applied when starting jobs.</span></span>

<span data-ttu-id="c2784-270">Zásady spouštění aktuálně povolit pro definování:</span><span class="sxs-lookup"><span data-stu-id="c2784-270">Execution policies currently allow for defining:</span></span>

* <span data-ttu-id="c2784-271">Název: Identifikátor pro zásady spouštění.</span><span class="sxs-lookup"><span data-stu-id="c2784-271">Name: Identifier for the execution policy.</span></span>
* <span data-ttu-id="c2784-272">Časový limit úlohy: Celkový čas před úlohy budou zrušeny úlohami elastické databáze.</span><span class="sxs-lookup"><span data-stu-id="c2784-272">Job Timeout: Total time before a job will be canceled by Elastic Database Jobs.</span></span>
* <span data-ttu-id="c2784-273">Počáteční Interval opakování: Interval čekání před první opakování.</span><span class="sxs-lookup"><span data-stu-id="c2784-273">Initial Retry Interval: Interval to wait before first retry.</span></span>
* <span data-ttu-id="c2784-274">Maximální Interval opakování: Limitu opakování intervalů používat.</span><span class="sxs-lookup"><span data-stu-id="c2784-274">Maximum Retry Interval: Cap of retry intervals to use.</span></span>
* <span data-ttu-id="c2784-275">Koeficient omezení rychlosti Interval opakování: Koeficient používá k výpočtu další interval mezi opakovanými pokusy.</span><span class="sxs-lookup"><span data-stu-id="c2784-275">Retry Interval Backoff Coefficient: Coefficient used to calculate the next interval between retries.</span></span>  <span data-ttu-id="c2784-276">Se používá následující vzorec: (počáteční opakujte Interval) * Math.pow ((Interval omezení rychlosti koeficient), (počet pokusů o) - 2).</span><span class="sxs-lookup"><span data-stu-id="c2784-276">The following formula is used: (Initial Retry Interval) * Math.pow((Interval Backoff Coefficient), (Number of Retries) - 2).</span></span> 
* <span data-ttu-id="c2784-277">Maximální počet pokusů: Maximální počet opakování pokusů provést v rámci úlohy.</span><span class="sxs-lookup"><span data-stu-id="c2784-277">Maximum Attempts: The maximum number of retry attempts to perform within a job.</span></span>

<span data-ttu-id="c2784-278">Výchozí zásadu spouštění používá následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="c2784-278">The default execution policy uses the following values:</span></span>

* <span data-ttu-id="c2784-279">Název: Zásady spouštění výchozí</span><span class="sxs-lookup"><span data-stu-id="c2784-279">Name: Default execution policy</span></span>
* <span data-ttu-id="c2784-280">Časový limit úlohy: 1 týden</span><span class="sxs-lookup"><span data-stu-id="c2784-280">Job Timeout: 1 week</span></span>
* <span data-ttu-id="c2784-281">Počáteční Interval opakování: 100 milisekund</span><span class="sxs-lookup"><span data-stu-id="c2784-281">Initial Retry Interval:  100 milliseconds</span></span>
* <span data-ttu-id="c2784-282">Maximální Interval opakování: 30 minut</span><span class="sxs-lookup"><span data-stu-id="c2784-282">Maximum Retry Interval: 30 minutes</span></span>
* <span data-ttu-id="c2784-283">Opakujte koeficient Interval: 2</span><span class="sxs-lookup"><span data-stu-id="c2784-283">Retry Interval Coefficient: 2</span></span>
* <span data-ttu-id="c2784-284">Maximální počet pokusů: 2 147 483 647</span><span class="sxs-lookup"><span data-stu-id="c2784-284">Maximum Attempts: 2,147,483,647</span></span>

<span data-ttu-id="c2784-285">Vytvořte zásadu požadované spouštění:</span><span class="sxs-lookup"><span data-stu-id="c2784-285">Create the desired execution policy:</span></span>

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 10
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval 
    -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $executionPolicy

### <a name="update-a-custom-execution-policy"></a><span data-ttu-id="c2784-286">Aktualizovat zásady vlastní spuštění</span><span class="sxs-lookup"><span data-stu-id="c2784-286">Update a custom execution policy</span></span>
<span data-ttu-id="c2784-287">Aktualizujte zásady spouštění požadované aktualizace:</span><span class="sxs-lookup"><span data-stu-id="c2784-287">Update the desired execution policy to update:</span></span>

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 15
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $updatedExecutionPolicy = Set-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $updatedExecutionPolicy

## <a name="cancel-a-job"></a><span data-ttu-id="c2784-288">Zrušení úlohy</span><span class="sxs-lookup"><span data-stu-id="c2784-288">Cancel a job</span></span>
<span data-ttu-id="c2784-289">Elastické databáze úlohy podporuje požadavků na zrušení úloh.</span><span class="sxs-lookup"><span data-stu-id="c2784-289">Elastic Database Jobs supports cancellation requests of jobs.</span></span>  <span data-ttu-id="c2784-290">Pokud elastické databáze úlohy zjistí žádost o zrušení úlohy se spouští, se ho pokusí zastavit úlohu.</span><span class="sxs-lookup"><span data-stu-id="c2784-290">If Elastic Database Jobs detects a cancellation request for a job currently being executed, it will attempt to stop the job.</span></span>

<span data-ttu-id="c2784-291">Že úlohy elastické databáze můžete provést zrušení dvěma různými způsoby:</span><span class="sxs-lookup"><span data-stu-id="c2784-291">There are two different ways that Elastic Database Jobs can perform a cancellation:</span></span>

1. <span data-ttu-id="c2784-292">Zrušit aktuálně spuštěných úloh: Pokud zrušení se zjistí, zatímco úloha je aktuálně spuštěna, zrušení se pokusí v rámci aktuálně prováděné aspekt úlohy.</span><span class="sxs-lookup"><span data-stu-id="c2784-292">Cancel currently executing tasks: If a cancellation is detected while a task is currently running, a cancellation will be attempted within the currently executing aspect of the task.</span></span>  <span data-ttu-id="c2784-293">Například: Pokud je aktuálně provést při pokusu o zrušení dlouho spuštěných dotazu, bude pokus o dotaz zrušíte.</span><span class="sxs-lookup"><span data-stu-id="c2784-293">For example: If there is a long running query currently being performed when a cancellation is attempted, there will be an attempt to cancel the query.</span></span>
2. <span data-ttu-id="c2784-294">Zrušení úloh opakování: v případě zrušení zjištění vlákno řízení předtím, než se spustí úloha pro spuštění, bude vlákno řízení vyhnout, spouští se úloha a deklarovat požadavek, protože došlo ke zrušení.</span><span class="sxs-lookup"><span data-stu-id="c2784-294">Canceling task retries: If a cancellation is detected by the control thread before a task is launched for execution, the control thread will avoid launching the task and declare the request as canceled.</span></span>

<span data-ttu-id="c2784-295">Pokud zrušení úlohy je požadováno pro nadřazené úloze, bude pro nadřazené úloze a všechny jeho podřízené úlohy dodržet žádost o zrušení.</span><span class="sxs-lookup"><span data-stu-id="c2784-295">If a job cancellation is requested for a parent job, the cancellation request will be honored for the parent job and for all of its child jobs.</span></span>

<span data-ttu-id="c2784-296">Odeslat žádost o zrušení, použijte [ **rutinu Stop-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) a nastavte **JobExecutionId** parametr.</span><span class="sxs-lookup"><span data-stu-id="c2784-296">To submit a cancellation request, use the [**Stop-AzureSqlJobExecution cmdlet**](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) and set the **JobExecutionId** parameter.</span></span>

    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId

## <a name="to-delete-a-job-and-job-history-asynchronously"></a><span data-ttu-id="c2784-297">K odstranění úlohy a úlohy historie asynchronně</span><span class="sxs-lookup"><span data-stu-id="c2784-297">To delete a job and job history asynchronously</span></span>
<span data-ttu-id="c2784-298">Elastické databáze úlohy podporuje asynchronní odstranění úloh.</span><span class="sxs-lookup"><span data-stu-id="c2784-298">Elastic Database jobs supports asynchronous deletion of jobs.</span></span> <span data-ttu-id="c2784-299">Úloha může být označený k odstranění a systém bude odstranění úlohy a všechny jeho historie úlohy po dokončení všech spuštěních úloh pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="c2784-299">A job can be marked for deletion and the system will delete the job and all its job history after all job executions have completed for the job.</span></span> <span data-ttu-id="c2784-300">Systém nebude automaticky zrušit spuštěních aktivní úlohy.</span><span class="sxs-lookup"><span data-stu-id="c2784-300">The system will not automatically cancel active job executions.</span></span>  

<span data-ttu-id="c2784-301">Vyvolání [ **Stop-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) zrušit spuštěních aktivní úlohy.</span><span class="sxs-lookup"><span data-stu-id="c2784-301">Invoke [**Stop-AzureSqlJobExecution**](/powershell/module/elasticdatabasejobs/stop-azuresqljobexecution) to cancel active job executions.</span></span>

<span data-ttu-id="c2784-302">Chcete-li aktivovat odstranění úlohy, použijte [ **rutinu Remove-AzureSqlJob** ](/powershell/module/elasticdatabasejobs/remove-azuresqljob) a nastavte **JobName** parametr.</span><span class="sxs-lookup"><span data-stu-id="c2784-302">To trigger job deletion, use the [**Remove-AzureSqlJob cmdlet**](/powershell/module/elasticdatabasejobs/remove-azuresqljob) and set the **JobName** parameter.</span></span>

    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName

## <a name="to-create-a-custom-database-target"></a><span data-ttu-id="c2784-303">Chcete-li vytvořit cíl vlastní databázi</span><span class="sxs-lookup"><span data-stu-id="c2784-303">To create a custom database target</span></span>
<span data-ttu-id="c2784-304">Můžete definovat vlastní databázi cíle pro přímé spouštění nebo pro zahrnutí do skupiny vlastní databázi.</span><span class="sxs-lookup"><span data-stu-id="c2784-304">You can define custom database targets either for direct execution or for inclusion within a custom database group.</span></span> <span data-ttu-id="c2784-305">Například protože **elastické fondy** není dosud podporován přímo pomocí rozhraní API prostředí PowerShell, můžete vytvořit vlastní databázi cíle a cílové kolekce vlastní databázi, která zahrnuje všechny databáze ve fondu.</span><span class="sxs-lookup"><span data-stu-id="c2784-305">For example, because **elastic pools** are not yet directly supported using PowerShell APIs, you can create a custom database target and custom database collection target which encompasses all the databases in the pool.</span></span>

<span data-ttu-id="c2784-306">Nastavte následující proměnné tak, aby odrážela informace o požadované databázi:</span><span class="sxs-lookup"><span data-stu-id="c2784-306">Set the following variables to reflect the desired database information:</span></span>

    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName 

## <a name="to-create-a-custom-database-collection-target"></a><span data-ttu-id="c2784-307">Chcete-li vytvořit cíl kolekce vlastní databázi</span><span class="sxs-lookup"><span data-stu-id="c2784-307">To create a custom database collection target</span></span>
<span data-ttu-id="c2784-308">Použití [ **New-AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) rutiny můžete definovat vlastní databázi kolekce cíl povolit spuštění v rámci více definovaných databázových cílů.</span><span class="sxs-lookup"><span data-stu-id="c2784-308">Use the [**New-AzureSqlJobTarget**](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) cmdlet to define a custom database collection target to enable execution across multiple defined database targets.</span></span> <span data-ttu-id="c2784-309">Po vytvoření skupiny databáze, databáze může být přidružený k cíli vlastní kolekce.</span><span class="sxs-lookup"><span data-stu-id="c2784-309">After creating a database group, databases can be associated with the custom collection target.</span></span>

<span data-ttu-id="c2784-310">Nastavte následující proměnné tak, aby odrážela konfigurace cílového požadovanou vlastní kolekce:</span><span class="sxs-lookup"><span data-stu-id="c2784-310">Set the following variables to reflect the desired custom collection target configuration:</span></span>

    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 

### <a name="to-add-databases-to-a-custom-database-collection-target"></a><span data-ttu-id="c2784-311">Přidání databází do kolekce cíl vlastní databázi</span><span class="sxs-lookup"><span data-stu-id="c2784-311">To add databases to a custom database collection target</span></span>
<span data-ttu-id="c2784-312">Přidání databáze pro použití konkrétní vlastní kolekce [ **přidat AzureSqlJobChildTarget** ](/powershell/module/elasticdatabasejobs/add-azuresqljobchildtarget) rutiny.</span><span class="sxs-lookup"><span data-stu-id="c2784-312">To add a database to a specific custom collection use the [**Add-AzureSqlJobChildTarget**](/powershell/module/elasticdatabasejobs/add-azuresqljobchildtarget) cmdlet.</span></span>

    $databaseServerName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName 

#### <a name="review-the-databases-within-a-custom-database-collection-target"></a><span data-ttu-id="c2784-313">Zkontrolujte databází v rámci kolekce cíl vlastní databázi</span><span class="sxs-lookup"><span data-stu-id="c2784-313">Review the databases within a custom database collection target</span></span>
<span data-ttu-id="c2784-314">Použití [ **Get-AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) rutiny načíst podřízené databází v rámci kolekce cíl vlastní databázi.</span><span class="sxs-lookup"><span data-stu-id="c2784-314">Use the [**Get-AzureSqlJobTarget**](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) cmdlet to retrieve the child databases within a custom database collection target.</span></span> 

    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets

### <a name="create-a-job-to-execute-a-script-across-a-custom-database-collection-target"></a><span data-ttu-id="c2784-315">Vytvořit úlohu pro spuštění skriptu mezi cílovou kolekci vlastní databázi</span><span class="sxs-lookup"><span data-stu-id="c2784-315">Create a job to execute a script across a custom database collection target</span></span>
<span data-ttu-id="c2784-316">Použití [ **New-AzureSqlJob** ](/powershell/module/elasticdatabasejobs/new-azuresqljob) rutiny vytvořit úlohu pro skupinu databází definované cílovou kolekci vlastní databázi.</span><span class="sxs-lookup"><span data-stu-id="c2784-316">Use the [**New-AzureSqlJob**](/powershell/module/elasticdatabasejobs/new-azuresqljob) cmdlet to create a job against a group of databases defined by a custom database collection target.</span></span> <span data-ttu-id="c2784-317">Elastické databáze úlohy se úloha rozšířit více podřízených úloh, každou odpovídající databázi přidruženého cílové kolekce vlastní databázi a ujistěte se, že skript se spustí na každou databázi.</span><span class="sxs-lookup"><span data-stu-id="c2784-317">Elastic Database jobs will expand the job into multiple child jobs each corresponding to a database associated with the custom database collection target and ensure that the script is executed against each database.</span></span> <span data-ttu-id="c2784-318">Znovu je důležité, aby skripty se idempotent chcete být odolní vůči opakování.</span><span class="sxs-lookup"><span data-stu-id="c2784-318">Again, it is important that scripts are idempotent to be resilient to retries.</span></span>

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job

## <a name="data-collection-across-databases"></a><span data-ttu-id="c2784-319">Shromažďování dat mezi databázemi</span><span class="sxs-lookup"><span data-stu-id="c2784-319">Data collection across databases</span></span>
<span data-ttu-id="c2784-320">Úlohu můžete použít k provedení dotazu napříč skupinou databází a odešlou výsledky do určité tabulky.</span><span class="sxs-lookup"><span data-stu-id="c2784-320">You can use a job to execute a query across a group of databases and send the results to a specific table.</span></span> <span data-ttu-id="c2784-321">V tabulce můžete položit dotaz na ve skutečnosti zobrazíte výsledky dotazu z každé databáze.</span><span class="sxs-lookup"><span data-stu-id="c2784-321">The table can be queried after the fact to see the query’s results from each database.</span></span> <span data-ttu-id="c2784-322">To poskytuje asynchronní metodu při spuštění dotazu mezi mnoha databázemi.</span><span class="sxs-lookup"><span data-stu-id="c2784-322">This provides an asynchronous method to execute a query across many databases.</span></span> <span data-ttu-id="c2784-323">Neúspěšných pokusů o přihlášení se zpracovávají automaticky prostřednictvím opakování.</span><span class="sxs-lookup"><span data-stu-id="c2784-323">Failed attempts are handled automatically via retries.</span></span>

<span data-ttu-id="c2784-324">Zadané cílové tabulky se automaticky vytvoří, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="c2784-324">The specified destination table will be automatically created if it does not yet exist.</span></span> <span data-ttu-id="c2784-325">Nová tabulka odpovídá schématu vrácené výsledné sady.</span><span class="sxs-lookup"><span data-stu-id="c2784-325">The new table matches the schema of the returned result set.</span></span> <span data-ttu-id="c2784-326">Pokud skript vrátí více sad výsledků dotazu, odeslat úlohy elastické databáze pouze první do cílové tabulky.</span><span class="sxs-lookup"><span data-stu-id="c2784-326">If a script returns multiple result sets, Elastic Database jobs will only send the first to the destination table.</span></span>

<span data-ttu-id="c2784-327">Následující skript PowerShell spouští skript a shromažďuje své výsledky do zadané tabulky.</span><span class="sxs-lookup"><span data-stu-id="c2784-327">The following PowerShell script executes a script and collects its results into a specified table.</span></span> <span data-ttu-id="c2784-328">Tento skript předpokládá, že skriptu T-SQL byl vytvořen který výstupy jedné sadě výsledků a vytvořený cíl kolekce vlastní databázi.</span><span class="sxs-lookup"><span data-stu-id="c2784-328">This script assumes that a T-SQL script has been created which outputs a single result set and that a custom database collection target has been created.</span></span>

<span data-ttu-id="c2784-329">Tento skript používá [ **Get-AzureSqlJobTarget** ](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) rutiny.</span><span class="sxs-lookup"><span data-stu-id="c2784-329">This script uses the [**Get-AzureSqlJobTarget**](/powershell/module/elasticdatabasejobs/new-azuresqljobtarget) cmdlet.</span></span> <span data-ttu-id="c2784-330">Nastavte parametry skriptu, přihlašovací údaje a provádění cíle:</span><span class="sxs-lookup"><span data-stu-id="c2784-330">Set the parameters for script, credentials, and execution target:</span></span>

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

### <a name="to-create-and-start-a-job-for-data-collection-scenarios"></a><span data-ttu-id="c2784-331">Vytvoření a spuštění úlohy scénáře kolekce dat</span><span class="sxs-lookup"><span data-stu-id="c2784-331">To create and start a job for data collection scenarios</span></span>
<span data-ttu-id="c2784-332">Tento skript používá [ **Start-AzureSqlJobExecution** ](/powershell/module/elasticdatabasejobs/start-azuresqljobexecution) rutiny.</span><span class="sxs-lookup"><span data-stu-id="c2784-332">This script uses the [**Start-AzureSqlJobExecution**](/powershell/module/elasticdatabasejobs/start-azuresqljobexecution) cmdlet.</span></span>

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

## <a name="to-schedule-a-job-execution-trigger"></a><span data-ttu-id="c2784-333">Při plánování provádění aktivační události úlohy</span><span class="sxs-lookup"><span data-stu-id="c2784-333">To schedule a job execution trigger</span></span>
<span data-ttu-id="c2784-334">Následující skript prostředí PowerShell můžete použít k vytvoření plánu opakování.</span><span class="sxs-lookup"><span data-stu-id="c2784-334">The following PowerShell script can be used to create a recurring schedule.</span></span> <span data-ttu-id="c2784-335">Tento skript používá intervalu minutu, ale [ **New-AzureSqlJobSchedule** ](/powershell/module/elasticdatabasejobs/new-azuresqljobschedule) také podporuje – DayInterval, - HourInterval, - MonthInterval a - WeekInterval parametry.</span><span class="sxs-lookup"><span data-stu-id="c2784-335">This script uses a minute interval, but [**New-AzureSqlJobSchedule**](/powershell/module/elasticdatabasejobs/new-azuresqljobschedule) also supports -DayInterval, -HourInterval, -MonthInterval, and -WeekInterval parameters.</span></span> <span data-ttu-id="c2784-336">Plány, které jsou spouštěny pouze jednou lze vytvořit pomocí předávání - jednorázově.</span><span class="sxs-lookup"><span data-stu-id="c2784-336">Schedules that execute only once can be created by passing -OneTime.</span></span>

<span data-ttu-id="c2784-337">Vytvoření nového plánu:</span><span class="sxs-lookup"><span data-stu-id="c2784-337">Create a new schedule:</span></span>

    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule 
    -MinuteInterval $minuteInterval 
    -ScheduleName $scheduleName 
    -StartTime $startTime 
    Write-Output $schedule

### <a name="to-trigger-a-job-executed-on-a-time-schedule"></a><span data-ttu-id="c2784-338">K aktivaci provést podle časového plánu úlohy</span><span class="sxs-lookup"><span data-stu-id="c2784-338">To trigger a job executed on a time schedule</span></span>
<span data-ttu-id="c2784-339">Aktivační události úlohy lze definovat za účelem mít úlohu provést podle časového plánu.</span><span class="sxs-lookup"><span data-stu-id="c2784-339">A job trigger can be defined to have a job executed according to a time schedule.</span></span> <span data-ttu-id="c2784-340">Následující skript prostředí PowerShell slouží k vytvoření aktivační události úlohy.</span><span class="sxs-lookup"><span data-stu-id="c2784-340">The following PowerShell script can be used to create a job trigger.</span></span>

<span data-ttu-id="c2784-341">Použití [New-AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/new-azuresqljobtrigger) a nastavte následující proměnné tak, aby odpovídaly požadované úlohy a plán:</span><span class="sxs-lookup"><span data-stu-id="c2784-341">Use [New-AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/new-azuresqljobtrigger) and set the following variables to correspond to the desired job and schedule:</span></span>

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger
    -ScheduleName $scheduleName
    -JobName $jobName
    Write-Output $jobTrigger

### <a name="to-remove-a-scheduled-association-to-stop-job-from-executing-on-schedule"></a><span data-ttu-id="c2784-342">Odebrání naplánované přidružení o zastavení úlohy ve spouštění podle plánu.</span><span class="sxs-lookup"><span data-stu-id="c2784-342">To remove a scheduled association to stop job from executing on schedule</span></span>
<span data-ttu-id="c2784-343">Odebrání ze opakovaném provádění úlohy prostřednictvím aktivační události úlohy, můžete odebrat aktivační události úlohy.</span><span class="sxs-lookup"><span data-stu-id="c2784-343">To discontinue reoccurring job execution through a job trigger, the job trigger can be removed.</span></span> <span data-ttu-id="c2784-344">Odebrat aktivační události úlohy zastavení úlohy z se spouští podle plánu pomocí [ **rutinu Remove-AzureSqlJobTrigger**](/powershell/module/elasticdatabasejobs/remove-azuresqljobtrigger).</span><span class="sxs-lookup"><span data-stu-id="c2784-344">Remove a job trigger to stop a job from being executed according to a schedule using the [**Remove-AzureSqlJobTrigger cmdlet**](/powershell/module/elasticdatabasejobs/remove-azuresqljobtrigger).</span></span>

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger 
    -ScheduleName $scheduleName 
    -JobName $jobName

### <a name="retrieve-job-triggers-bound-to-a-time-schedule"></a><span data-ttu-id="c2784-345">Načíst aktivační události úlohy vázána na časového plánu</span><span class="sxs-lookup"><span data-stu-id="c2784-345">Retrieve job triggers bound to a time schedule</span></span>
<span data-ttu-id="c2784-346">Následující skript prostředí PowerShell slouží k získání a zobrazení aktivačních událostí úlohy registrované na konkrétní časového plánu.</span><span class="sxs-lookup"><span data-stu-id="c2784-346">The following PowerShell script can be used to obtain and display the job triggers registered to a particular time schedule.</span></span>

    $scheduleName = "{Schedule Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -ScheduleName $scheduleName
    Write-Output $jobTriggers

### <a name="to-retrieve-job-triggers-bound-to-a-job"></a><span data-ttu-id="c2784-347">Načíst aktivační události úlohy vázána na úlohu</span><span class="sxs-lookup"><span data-stu-id="c2784-347">To retrieve job triggers bound to a job</span></span>
<span data-ttu-id="c2784-348">Použití [Get-AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/get-azuresqljobtrigger) můžete získat a zobrazit plány obsahující úlohu registrované.</span><span class="sxs-lookup"><span data-stu-id="c2784-348">Use [Get-AzureSqlJobTrigger](/powershell/module/elasticdatabasejobs/get-azuresqljobtrigger) to obtain and display schedules containing a registered job.</span></span>

    $jobName = "{Job Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -JobName $jobName
    Write-Output $jobTriggers

## <a name="to-create-a-data-tier-application-dacpac-for-execution-across-databases"></a><span data-ttu-id="c2784-349">Vytvořit aplikace na datové vrstvě (DACPAC) pro spuštění v rámci databáze</span><span class="sxs-lookup"><span data-stu-id="c2784-349">To create a data-tier application (DACPAC) for execution across databases</span></span>
<span data-ttu-id="c2784-350">Vytvoření DACPAC naleznete v tématu [aplikace datové vrstvy](https://msdn.microsoft.com/library/ee210546.aspx).</span><span class="sxs-lookup"><span data-stu-id="c2784-350">To create a DACPAC, see [Data-Tier applications](https://msdn.microsoft.com/library/ee210546.aspx).</span></span> <span data-ttu-id="c2784-351">Chcete-li nasadit DACPAC, použijte [rutiny New-AzureSqlJobContent](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent).</span><span class="sxs-lookup"><span data-stu-id="c2784-351">To deploy a DACPAC, use the [New-AzureSqlJobContent cmdlet](/powershell/module/elasticdatabasejobs/new-azuresqljobcontent).</span></span> <span data-ttu-id="c2784-352">DACPAC musí být přístupný pro službu.</span><span class="sxs-lookup"><span data-stu-id="c2784-352">The DACPAC must be accessible to the service.</span></span> <span data-ttu-id="c2784-353">Doporučuje se nahrát vytvořený DACPAC do služby Azure Storage a vytvoření [sdíleného přístupového podpisu](../storage/common/storage-dotnet-shared-access-signature-part-1.md) pro DACPAC.</span><span class="sxs-lookup"><span data-stu-id="c2784-353">It is recommended to upload a created DACPAC to Azure Storage and create a [Shared Access Signature](../storage/common/storage-dotnet-shared-access-signature-part-1.md) for the DACPAC.</span></span>

    $dacpacUri = "{Uri}"
    $dacpacName = "{Dacpac Name}"
    $dacpac = New-AzureSqlJobContent -DacpacUri $dacpacUri -ContentName $dacpacName 
    Write-Output $dacpac

### <a name="to-update-a-data-tier-application-dacpac-for-execution-across-databases"></a><span data-ttu-id="c2784-354">Aktualizace aplikace na datové vrstvě (DACPAC) pro provedení mezi databázemi</span><span class="sxs-lookup"><span data-stu-id="c2784-354">To update a data-tier application (DACPAC) for execution across databases</span></span>
<span data-ttu-id="c2784-355">Existující DACPACs zaregistrován v rámci úlohy elastické databáze můžete aktualizovat tak, aby odkazoval na nový identifikátory URI.</span><span class="sxs-lookup"><span data-stu-id="c2784-355">Existing DACPACs registered within Elastic Database Jobs can be updated to point to new URIs.</span></span> <span data-ttu-id="c2784-356">Použití [ **rutiny Set-AzureSqlJobContentDefinition** ](/powershell/module/elasticdatabasejobs/set-azuresqljobcontentdefinition) aktualizovat identifikátor URI DACPAC na existujícím zaregistrován DACPAC:</span><span class="sxs-lookup"><span data-stu-id="c2784-356">Use the [**Set-AzureSqlJobContentDefinition cmdlet**](/powershell/module/elasticdatabasejobs/set-azuresqljobcontentdefinition) to update the DACPAC URI on an existing registered DACPAC:</span></span>

    $dacpacName = "{Dacpac Name}"
    $newDacpacUri = "{Uri}"
    $updatedDacpac = Set-AzureSqlJobDacpacDefinition -ContentName $dacpacName -DacpacUri $newDacpacUri
    Write-Output $updatedDacpac

## <a name="to-create-a-job-to-apply-a-data-tier-application-dacpac-across-databases"></a><span data-ttu-id="c2784-357">Chcete-li vytvořit úlohu použít aplikaci na datové vrstvě (DACPAC) mezi databázemi</span><span class="sxs-lookup"><span data-stu-id="c2784-357">To create a job to apply a data-tier application (DACPAC) across databases</span></span>
<span data-ttu-id="c2784-358">Po vytvoření DACPAC v rámci úlohy elastické databáze, lze vytvořit úlohu pro použití DACPAC napříč skupinou databází.</span><span class="sxs-lookup"><span data-stu-id="c2784-358">After a DACPAC has been created within Elastic Database Jobs, a job can be created to apply the DACPAC across a group of databases.</span></span> <span data-ttu-id="c2784-359">Následující skript prostředí PowerShell slouží k vytvoření úlohy DACPAC v kolekci vlastních databází:</span><span class="sxs-lookup"><span data-stu-id="c2784-359">The following PowerShell script can be used to create a DACPAC job across a custom collection of databases:</span></span>

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
