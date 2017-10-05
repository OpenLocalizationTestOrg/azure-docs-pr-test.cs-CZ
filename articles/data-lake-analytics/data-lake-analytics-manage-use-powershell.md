---
title: "Správa Azure Data Lake Analytics pomocí Azure PowerShell | Microsoft Docs"
description: "Naučte se spravovat účty Data Lake Analytics, zdrojů dat, úlohy a položky katalogu. "
services: data-lake-analytics
documentationcenter: 
author: matt1883
manager: jhubbard
editor: cgronlun
ms.assetid: ad14d53c-fed4-478d-ab4b-6d2e14ff2097
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/23/2017
ms.author: mahi
ms.openlocfilehash: 862e9551f1e129b7bba06651fbae94e337c92dcb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="manage-azure-data-lake-analytics-using-azure-powershell"></a><span data-ttu-id="b3ad1-103">Správa Azure Data Lake Analytics pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="b3ad1-103">Manage Azure Data Lake Analytics using Azure PowerShell</span></span>
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

<span data-ttu-id="b3ad1-104">Naučte se spravovat účty Azure Data Lake Analytics, zdrojů dat, úlohy a položky katalogu pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-104">Learn how to manage Azure Data Lake Analytics accounts, data sources, jobs, and catalog items using Azure PowerShell.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="b3ad1-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b3ad1-105">Prerequisites</span></span>

<span data-ttu-id="b3ad1-106">Při vytváření účtu Data Lake Analytics, musíte znát:</span><span class="sxs-lookup"><span data-stu-id="b3ad1-106">When creating a Data Lake Analytics account, you need to know:</span></span>

* <span data-ttu-id="b3ad1-107">**ID předplatného**: ID předplatného Azure, ve kterém se nachází váš účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-107">**Subscription ID**: The Azure subscription ID under which your Data Lake Analytics account resides.</span></span>
* <span data-ttu-id="b3ad1-108">**Skupina prostředků**: název skupiny prostředků Azure, která obsahuje váš účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-108">**Resource group**: The name of the Azure resource group that contains your Data Lake Analytics account.</span></span>
* <span data-ttu-id="b3ad1-109">**Název účtu data Lake Analytics**: název účtu musí obsahovat jenom malá písmena a číslice.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-109">**Data Lake Analytics account name**: The account name must only contain lowercase letters and numbers.</span></span>
* <span data-ttu-id="b3ad1-110">**Výchozí účet Data Lake Store:** Každý účet Data Lake Analytics má výchozí účet Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-110">**Default Data Lake Store account**: Each Data Lake Analytics account has a default Data Lake Store account.</span></span> <span data-ttu-id="b3ad1-111">Tyto účty musí být ve stejném umístění.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-111">These accounts must be in the same location.</span></span>
* <span data-ttu-id="b3ad1-112">**Umístění**: umístění vašeho účtu Data Lake Analytics, jako je například "East US 2" nebo jiné podporované umístění.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-112">**Location**: The location of your Data Lake Analytics account, such as "East US 2" or other supported locations.</span></span> <span data-ttu-id="b3ad1-113">Podporovaná umístění můžete zobrazit na našem [stránce s cenami](https://azure.microsoft.com/pricing/details/data-lake-analytics/).</span><span class="sxs-lookup"><span data-stu-id="b3ad1-113">Supported locations can be seen on our [pricing page](https://azure.microsoft.com/pricing/details/data-lake-analytics/).</span></span>

<span data-ttu-id="b3ad1-114">Fragment kódu PowerShellu v tomto kurzu používá následující proměnné k ukládání příslušných informací:</span><span class="sxs-lookup"><span data-stu-id="b3ad1-114">The PowerShell snippets in this tutorial use these variables to store this information</span></span>

```powershell
$subId = "<SubscriptionId>"
$rg = "<ResourceGroupName>"
$adla = "<DataLakeAnalyticsAccountName>"
$adls = "<DataLakeStoreAccountName>"
$location = "<Location>"
```

## <a name="log-in"></a><span data-ttu-id="b3ad1-115">Přihlásit se</span><span class="sxs-lookup"><span data-stu-id="b3ad1-115">Log in</span></span>

<span data-ttu-id="b3ad1-116">Přihlaste se pomocí id předplatného.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-116">Log in using a subscription id.</span></span>

```powershell
Login-AzureRmAccount -SubscriptionId $subId
```

<span data-ttu-id="b3ad1-117">Přihlaste se pomocí název odběru.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-117">Log in using a subscription name.</span></span>

```
Login-AzureRmAccount -SubscriptionName $subname 
```

<span data-ttu-id="b3ad1-118">`Login-AzureRmAccount` Rutiny vždycky zobrazí výzvu k zadání pověření.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-118">The `Login-AzureRmAccount` cmdlet  always prompts for credentials.</span></span> <span data-ttu-id="b3ad1-119">Můžete vyhnout se zobrazí výzva, a to pomocí následujících rutin:</span><span class="sxs-lookup"><span data-stu-id="b3ad1-119">You can avoid being prompted by using the following cmdlets:</span></span>

```powershell
# Save login session information
Save-AzureRmProfile -Path D:\profile.json  

# Load login session information
Select-AzureRmProfile -Path D:\profile.json 
```

## <a name="managing-accounts"></a><span data-ttu-id="b3ad1-120">Správa účtů</span><span class="sxs-lookup"><span data-stu-id="b3ad1-120">Managing accounts</span></span>

### <a name="create-a-data-lake-analytics-account"></a><span data-ttu-id="b3ad1-121">Vytvoření účtu Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="b3ad1-121">Create a Data Lake Analytics account</span></span>

<span data-ttu-id="b3ad1-122">Pokud ještě nemáte [skupiny prostředků](../azure-resource-manager/resource-group-overview.md#resource-groups) Pokud chcete používat, vytvořte jeden.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-122">If you don't already have a [resource group](../azure-resource-manager/resource-group-overview.md#resource-groups) to use, create one.</span></span> 

```powershell
New-AzureRmResourceGroup -Name  $rg -Location $location
```

<span data-ttu-id="b3ad1-123">Každý účet Data Lake Analytics vyžaduje výchozí účet Data Lake Store, který slouží k ukládání protokolů.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-123">Every Data Lake Analytics account requires a default Data Lake Store account that it uses for storing logs.</span></span> <span data-ttu-id="b3ad1-124">Můžete znovu použít existující účet nebo si vytvořit účet.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-124">You can reuse an existing account or create an account.</span></span> 

```powershell
New-AdlStore -ResourceGroupName $rg -Name $adls -Location $location
```

<span data-ttu-id="b3ad1-125">Jakmile budete mít k dispozici skupinu prostředků a účet Data Lake Store, vytvořte účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-125">Once a Resource Group and Data Lake Store account is available, create a Data Lake Analytics account.</span></span>

```powershell
New-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla -Location $location -DefaultDataLake $adls
```

### <a name="get-information-about-an-account"></a><span data-ttu-id="b3ad1-126">Získání informací o účtu</span><span class="sxs-lookup"><span data-stu-id="b3ad1-126">Get information about an account</span></span>

<span data-ttu-id="b3ad1-127">Získejte podrobnosti o účtu.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-127">Get details about an account.</span></span>

```powershell
Get-AdlAnalyticsAccount -Name $adla
```

<span data-ttu-id="b3ad1-128">Zkontrolujte existenci určitého účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-128">Check the existence of a specific Data Lake Analytics account.</span></span> <span data-ttu-id="b3ad1-129">Vrátí rutina `True` nebo `False`.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-129">The cmdlet returns either `True` or `False`.</span></span>

```powershell
Test-AdlAnalyticsAccount -Name $adla
```

<span data-ttu-id="b3ad1-130">Zkontrolujte existenci určitého účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-130">Check the existence of a specific Data Lake Store account.</span></span> <span data-ttu-id="b3ad1-131">Vrátí rutina `True` nebo `False`.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-131">The cmdlet returns either `True` or `False`.</span></span>

```powershell
Test-AdlStoreAccount -Name $adls
```

### <a name="listing-accounts"></a><span data-ttu-id="b3ad1-132">Výpis účtů</span><span class="sxs-lookup"><span data-stu-id="b3ad1-132">Listing accounts</span></span>

<span data-ttu-id="b3ad1-133">Seznam Data Lake Analytics účty v aktuálním předplatném.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-133">List Data Lake Analytics accounts within the current subscription.</span></span>

```powershell
Get-AdlAnalyticsAccount
```

<span data-ttu-id="b3ad1-134">Seznam Data Lake Analytics účtů v rámci určité skupiny zdrojů.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-134">List Data Lake Analytics accounts within a specific resource group.</span></span>

```powershell
Get-AdlAnalyticsAccount -ResourceGroupName $rg
```

## <a name="managing-firewall-rules"></a><span data-ttu-id="b3ad1-135">Správa pravidel brány firewall</span><span class="sxs-lookup"><span data-stu-id="b3ad1-135">Managing firewall rules</span></span>

<span data-ttu-id="b3ad1-136">Zobrazí seznam pravidel brány firewall.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-136">List firewall rules.</span></span>

```powershell
Get-AdlAnalyticsFirewallRule -Account $adla
```

<span data-ttu-id="b3ad1-137">Přidejte pravidlo brány firewall.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-137">Add a firewall rule.</span></span>

```powershell
$ruleName = "Allow access from on-prem server"
$startIpAddress = "<start IP address>"
$endIpAddress = "<end IP address>"

Add-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName -StartIpAddress $startIpAddress -EndIpAddress $endIpAddress
```

<span data-ttu-id="b3ad1-138">Změňte pravidlo brány firewall.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-138">Change a firewall rule.</span></span>

```powershell
Set-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName -StartIpAddress $startIpAddress -EndIpAddress $endIpAddress
```

<span data-ttu-id="b3ad1-139">Odeberte pravidlo brány firewall.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-139">Remove a firewall rule.</span></span>

```powershell
Remove-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName
```



<span data-ttu-id="b3ad1-140">Povolit Azure IP adresy.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-140">Allow Azure IP addresses.</span></span>

```powershell
Set-AdlAnalyticsAccount -Name $adla -AllowAzureIpState Enabled
```

```powershell
Set-AdlAnalyticsAccount -Name $adla -FirewallState Enabled
Set-AdlAnalyticsAccount -Name $adla -FirewallState Disabled
```

## <a name="managing-data-sources"></a><span data-ttu-id="b3ad1-141">Správa zdrojů dat</span><span class="sxs-lookup"><span data-stu-id="b3ad1-141">Managing data sources</span></span>
<span data-ttu-id="b3ad1-142">Azure Data Lake Analytics teď podporuje následující zdroje dat:</span><span class="sxs-lookup"><span data-stu-id="b3ad1-142">Azure Data Lake Analytics currently supports the following data sources:</span></span>

* [<span data-ttu-id="b3ad1-143">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="b3ad1-143">Azure Data Lake Store</span></span>](../data-lake-store/data-lake-store-overview.md)
* [<span data-ttu-id="b3ad1-144">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="b3ad1-144">Azure Storage</span></span>](../storage/common/storage-introduction.md)

<span data-ttu-id="b3ad1-145">Když vytvoříte účet Analytics, je třeba určit účet Data Lake Store jako zdroj dat výchozí.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-145">When you create an Analytics account, you must designate a Data Lake Store account to be the default data source.</span></span> <span data-ttu-id="b3ad1-146">Výchozí účet Data Lake Store se používá k ukládání metadat a úlohy auditu v protokolech úloh.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-146">The default Data Lake Store account is used to store job metadata and job audit logs.</span></span> <span data-ttu-id="b3ad1-147">Po vytvoření účtu Data Lake Analytics, můžete přidat další účty Data Lake Store a účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-147">After you have created a Data Lake Analytics account, you can add additional Data Lake Store accounts and/or Storage accounts.</span></span> 

### <a name="find-the-default-data-lake-store-account"></a><span data-ttu-id="b3ad1-148">Najít výchozího účtu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="b3ad1-148">Find the default Data Lake Store account</span></span>

```powershell
$adla_acct = Get-AdlAnalyticsAccount -Name $adla
$dataLakeStoreName = $adla_acct.DefaultDataLakeAccount
```

<span data-ttu-id="b3ad1-149">Výchozí účet Data Lake Store můžete najít pomocí filtrování seznamu zdrojů dat pomocí `IsDefault` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="b3ad1-149">You can find the default Data Lake Store account by filtering the list of datasources by the `IsDefault` property:</span></span>

```powershell
Get-AdlAnalyticsDataSource -Account $adla  | ? { $_.IsDefault } 
```

### <a name="add-a-data-source"></a><span data-ttu-id="b3ad1-150">Přidání zdroje dat</span><span class="sxs-lookup"><span data-stu-id="b3ad1-150">Add a data source</span></span>

```powershell

# Add an additional Storage (Blob) account.
$AzureStorageAccountName = "<AzureStorageAccountName>"
$AzureStorageAccountKey = "<AzureStorageAccountKey>"
Add-AdlAnalyticsDataSource -Account $adla -Blob $AzureStorageAccountName -AccessKey $AzureStorageAccountKey

# Add an additional Data Lake Store account.
$AzureDataLakeStoreName = "<AzureDataLakeStoreAccountName"
Add-AdlAnalyticsDataSource -Account $adla -DataLakeStore $AzureDataLakeStoreName 
```

### <a name="list-data-sources"></a><span data-ttu-id="b3ad1-151">Seznam zdrojů dat</span><span class="sxs-lookup"><span data-stu-id="b3ad1-151">List data sources</span></span>

```powershell
# List all the data sources
Get-AdlAnalyticsDataSource -Name $adla

# List attached Data Lake Store accounts
Get-AdlAnalyticsDataSource -Name $adla | where -Property Type -EQ "DataLakeStore"

# List attached Storage accounts
Get-AdlAnalyticsDataSource -Name $adla | where -Property Type -EQ "Blob"
```

## <a name="submit-u-sql-jobs"></a><span data-ttu-id="b3ad1-152">Odesílání úloh U-SQL</span><span class="sxs-lookup"><span data-stu-id="b3ad1-152">Submit U-SQL jobs</span></span>

### <a name="submit-a-string-as-a-u-sql-script"></a><span data-ttu-id="b3ad1-153">Odeslat řetězec jako skript U-SQL</span><span class="sxs-lookup"><span data-stu-id="b3ad1-153">Submit a string as a U-SQL script</span></span>

```powershell
$script = @"
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
"@

$scriptpath = "d:\test.usql"
$script | Out-File $scriptpath 

Submit-AdlJob -AccountName $adla -Script $script -Name "Demo"
```


### <a name="submit-a-file-as-a-u-sql-script"></a><span data-ttu-id="b3ad1-154">Odeslat soubor jako skript U-SQL</span><span class="sxs-lookup"><span data-stu-id="b3ad1-154">Submit a file as a U-SQL script</span></span>

```powershell
$scriptpath = "d:\test.usql"
$script | Out-File $scriptpath 
Submit-AdlJob -AccountName $adla –ScriptPath $scriptpath -Name "Demo"
```

## <a name="list-jobs-in-an-account"></a><span data-ttu-id="b3ad1-155">Seznam úloh v účtu</span><span class="sxs-lookup"><span data-stu-id="b3ad1-155">List jobs in an account</span></span>

### <a name="list-all-the-jobs-in-the-account"></a><span data-ttu-id="b3ad1-156">Vypište všechny úlohy v rámci účtu.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-156">List all the jobs in the account.</span></span> 

<span data-ttu-id="b3ad1-157">Výstup zahrnuje aktuálně spuštěné úlohy a nedávno dokončené úlohy.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-157">The output includes the currently running jobs and those jobs that have recently completed.</span></span>

```powershell
Get-AdlJob -Account $adla
```


### <a name="list-a-specific-number-of-jobs"></a><span data-ttu-id="b3ad1-158">Seznam určitý počet úloh</span><span class="sxs-lookup"><span data-stu-id="b3ad1-158">List a specific number of jobs</span></span>

<span data-ttu-id="b3ad1-159">Ve výchozím nastavení seřazeny seznam úloh na odeslání čas.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-159">By default the list of jobs is sorted on submit time.</span></span> <span data-ttu-id="b3ad1-160">Proto zobrazí první naposledy odeslaných úlohy.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-160">So the most recently submitted jobs appear first.</span></span> <span data-ttu-id="b3ad1-161">Ve výchozím nastavení ADLA účet pamatuje úlohy na 180 dní, ale Ge AdlJob rutina ve výchozím nastavení vrací pouze první 500.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-161">By default, The ADLA account remembers jobs for 180 days, but the Ge-AdlJob  cmdlet by default returns only the first 500.</span></span> <span data-ttu-id="b3ad1-162">Pomocí - nejvyšší parametr seznam určitý počet úloh.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-162">Use -Top parameter to list a specific number of jobs.</span></span>

```powershell
$jobs = Get-AdlJob -Account $adla -Top 10
```


### <a name="list-jobs-based-on-the-value-of-job-property"></a><span data-ttu-id="b3ad1-163">Seznam úloh na základě hodnoty vlastnosti úlohy</span><span class="sxs-lookup"><span data-stu-id="b3ad1-163">List jobs based on the value of job property</span></span>

<span data-ttu-id="b3ad1-164">Pomocí `-State` parametr.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-164">Using the `-State` parameter.</span></span> <span data-ttu-id="b3ad1-165">Zkombinováním některou z těchto hodnot:</span><span class="sxs-lookup"><span data-stu-id="b3ad1-165">You can combine any of these values:</span></span>

* `Accepted`
* `Compiling`
* `Ended`
* `New`
* `Paused`
* `Queued`
* `Running`
* `Scheduling`
* `Start`

```powershell
# List the running jobs
Get-AdlJob -Account $adla -State Running

# List the jobs that have completed
Get-AdlJob -Account $adla -State Ended

# List the jobs that have not started yet
Get-AdlJob -Account $adla -State Accepted,Compiling,New,Paused,Scheduling,Start
```

<span data-ttu-id="b3ad1-166">Použití `-Result` parametr ke zjištění, zda zakončeno úlohy byl úspěšně dokončen.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-166">Use the `-Result` parameter to detect whether ended jobs completed successfully.</span></span> <span data-ttu-id="b3ad1-167">Obsahuje tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="b3ad1-167">It has these values:</span></span>

* <span data-ttu-id="b3ad1-168">Zrušena</span><span class="sxs-lookup"><span data-stu-id="b3ad1-168">Cancelled</span></span>
* <span data-ttu-id="b3ad1-169">Se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="b3ad1-169">Failed</span></span>
* <span data-ttu-id="b3ad1-170">Žádný</span><span class="sxs-lookup"><span data-stu-id="b3ad1-170">None</span></span>
* <span data-ttu-id="b3ad1-171">Úspěch</span><span class="sxs-lookup"><span data-stu-id="b3ad1-171">Succeeded</span></span>

``` powershell
# List Successful jobs.
Get-AdlJob -Account $adla -State Ended -Result Succeeded

# List Failed jobs.
Get-AdlJob -Account $adla -State Ended -Result Failed
```


<span data-ttu-id="b3ad1-172">`-Submitter` Parametr pomáhá identifikovat, kdo odeslání úlohy.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-172">The `-Submitter` parameter helps you identify who submitted a job.</span></span>

```powershell
Get-AdlJob -Account $adla -Submitter "joe@contoso.com"
```

<span data-ttu-id="b3ad1-173">`-SubmittedAfter` Je užitečný při filtrování časový rozsah.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-173">The `-SubmittedAfter` is useful in filtering to a time range.</span></span>


```powershell
# List  jobs submitted in the last day.
$d = [DateTime]::Now.AddDays(-1)
Get-AdlJob -Account $adla -SubmittedAfter $d

# List  jobs submitted in the last seven day.
$d = [DateTime]::Now.AddDays(-7)
Get-AdlJob -Account $adla -SubmittedAfter $d
```

### <a name="common-scenarios-for-listing-jobs"></a><span data-ttu-id="b3ad1-174">Časté scénáře pro seznam úloh</span><span class="sxs-lookup"><span data-stu-id="b3ad1-174">Common scenarios for listing jobs</span></span>


```
# List jobs submitted in the last five days and that successfully completed.
$d = (Get-Date).AddDays(-5)
Get-AdlJob -Account $adla -SubmittedAfter $d -State Ended -Result Succeeded

# List all failed jobs submitted by "joe@contoso.com" within the past seven days.
Get-AdlJob -Account $adla `
    -Submitter "joe@contoso.com" `
    -SubmittedAfter (Get-Date).AddDays(-7) `
    -Result Failed
```

## <a name="filtering-a-list-of-jobs"></a><span data-ttu-id="b3ad1-175">Filtrování seznamu úloh</span><span class="sxs-lookup"><span data-stu-id="b3ad1-175">Filtering a list of jobs</span></span>

<span data-ttu-id="b3ad1-176">Jakmile máte seznam úloh v aktuální relaci prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-176">Once you have a list of jobs in your current PowerShell session.</span></span> <span data-ttu-id="b3ad1-177">Normální rutiny prostředí PowerShell můžete použít pro filtrování seznamu.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-177">You can use normal PowerShell cmdlets to filter the list.</span></span>

<span data-ttu-id="b3ad1-178">Filtrovat seznam úloh pro úlohy, odeslané za posledních 24 hodin</span><span class="sxs-lookup"><span data-stu-id="b3ad1-178">Filter a list of jobs to the jobs submitted in the last 24 hours</span></span>

```
$upperdate = Get-Date
$lowerdate = $upperdate.AddHours(-24)
$jobs | Where-Object { $_.EndTime -ge $lowerdate }
```

<span data-ttu-id="b3ad1-179">Filtrovat seznam úloh úlohy, jejichž skončila v posledních 24 hodin</span><span class="sxs-lookup"><span data-stu-id="b3ad1-179">Filter a list of jobs to the jobs that ended in the last 24 hours</span></span>

```
$upperdate = Get-Date
$lowerdate = $upperdate.AddHours(-24)
$jobs | Where-Object { $_.SubmitTime -ge $lowerdate }
```

<span data-ttu-id="b3ad1-180">Filtrovat seznam úloh úlohy, jejichž spuštění.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-180">Filter a list of jobs to the jobs that started running.</span></span> <span data-ttu-id="b3ad1-181">Úloha může selhat v době kompilace - a proto se nespustí.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-181">A job might fail at compile time - and so it never starts.</span></span> <span data-ttu-id="b3ad1-182">Podívejme se na neúspěšné úlohy, které ve skutečnosti spuštění a pak se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-182">Let's look at the failed jobs that actually started running and then failed.</span></span>

```powershell
$jobs | Where-Object { $_.StartTime -ne $null }
```

### <a name="analyzing-a-list-of-jobs"></a><span data-ttu-id="b3ad1-183">Analýza seznam úloh</span><span class="sxs-lookup"><span data-stu-id="b3ad1-183">Analyzing a list of jobs</span></span>

<span data-ttu-id="b3ad1-184">Použití `Group-Object` rutiny k analýze seznam úloh.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-184">Use the `Group-Object` cmdlet to analyze a list of jobs.</span></span>

```
# Count the number of jobs by Submitter
$jobs | Group-Object Submitter | Select -Property Count,Name

# Count the number of jobs by Result
$jobs | Group-Object Result | Select -Property Count,Name

# Count the number of jobs by State
$jobs | Group-Object State | Select -Property Count,Name

#  Count the number of jobs by DegreeOfParallelism
$jobs | Group-Object DegreeOfParallelism | Select -Property Count,Name
```
<span data-ttu-id="b3ad1-185">Při provádění analýzu, může být užitečné k přidání vlastností do objektů úloh, aby filtrování a seskupení jednodušší.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-185">When performing an analysis, it can be useful to add properties to the Job objects to make filtering and grouping simpler.</span></span> <span data-ttu-id="b3ad1-186">Následující fragment kódu ukazuje, jak opatřit poznámkami informací o úloze s počítané vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-186">The following  snippet shows how to annotate a JobInfo with calculated properties.</span></span>

```
function annotate_job( $j )
{
    $dic1 = @{
        Label='AUHours';
        Expression={ ($_.DegreeOfParallelism * ($_.EndTime-$_.StartTime).TotalHours)}}
    $dic2 = @{
        Label='DurationSeconds';
        Expression={ ($_.EndTime-$_.StartTime).TotalSeconds}}
    $dic3 = @{
        Label='DidRun';
        Expression={ ($_.StartTime -ne $null)}}

    $j2 = $j | select *, $dic1, $dic2, $dic3
    $j2
}

$jobs = Get-AdlJob -Account $adla -Top 10
$jobs = $jobs | %{ annotate_job( $_ ) }
```

## <a name="get-information-about-pipelines-and-recurrences"></a><span data-ttu-id="b3ad1-187">Získání informací o kanálů a opakování</span><span class="sxs-lookup"><span data-stu-id="b3ad1-187">Get information about pipelines and recurrences</span></span>

<span data-ttu-id="b3ad1-188">Použití `Get-AdlJobPipeline` rutiny, které chcete zobrazit informace o kanálu předchozích odeslaných úlohy.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-188">Use the `Get-AdlJobPipeline` cmdlet to see the pipeline information previously submitted jobs.</span></span>

```powershell
$pipelines = Get-AdlJobPipeline -Account $adla

$pipeline = Get-AdlJobPipeline -Account $adla -PipelineId "<pipeline ID>"
```

<span data-ttu-id="b3ad1-189">Použití `Get-AdlJobRecurrence` rutiny zobrazíte informace o opakování pro dříve odeslání úlohy.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-189">Use the `Get-AdlJobRecurrence` cmdlet to see the recurrence information for previously submitted jobs.</span></span>

```powershell
$recurrences = Get-AdlJobRecurrence -Account $adla

$recurrence = Get-AdlJobRecurrence -Account $adla -RecurrenceId "<recurrence ID>"
```

## <a name="get-information-about-a-job"></a><span data-ttu-id="b3ad1-190">Získání informací o úlohy</span><span class="sxs-lookup"><span data-stu-id="b3ad1-190">Get information about a job</span></span>

### <a name="get-job-status"></a><span data-ttu-id="b3ad1-191">Získat stav úlohy</span><span class="sxs-lookup"><span data-stu-id="b3ad1-191">Get job status</span></span>

<span data-ttu-id="b3ad1-192">Získejte stav konkrétní úlohy.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-192">Get the status of a specific job.</span></span>

```powershell
Get-AdlJob -AccountName $adla -JobId $job.JobId
```

### <a name="examine-the-job-outputs"></a><span data-ttu-id="b3ad1-193">Zkontrolujte výstupy úlohy</span><span class="sxs-lookup"><span data-stu-id="b3ad1-193">Examine the job outputs</span></span>

<span data-ttu-id="b3ad1-194">Po ukončení úlohy, zkontrolujte, jestli výstupní soubor existuje tak, že uvedete souborů ve složce.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-194">After the job has ended, check if the output file exists by listing the files in a folder.</span></span>

```powershell
Get-AdlStoreChildItem -Account $adls -Path "/"
```

## <a name="manage-running-jobs"></a><span data-ttu-id="b3ad1-195">Spravovat spuštěné úlohy</span><span class="sxs-lookup"><span data-stu-id="b3ad1-195">Manage running jobs</span></span>

### <a name="cancel-a-job"></a><span data-ttu-id="b3ad1-196">Zrušení úlohy</span><span class="sxs-lookup"><span data-stu-id="b3ad1-196">Cancel a job</span></span>

```powershell
Stop-AdlJob -Account $adls -JobID $jobID
```

### <a name="wait-for-a-job-to-finish"></a><span data-ttu-id="b3ad1-197">Počkejte na dokončení úlohy</span><span class="sxs-lookup"><span data-stu-id="b3ad1-197">Wait for a job to finish</span></span>

<span data-ttu-id="b3ad1-198">Místo opakující se `Get-AdlAnalyticsJob` , dokud nebude dokončeno úlohu, můžete použít `Wait-AdlJob` rutiny čekat na ukončení úlohy.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-198">Instead of repeating `Get-AdlAnalyticsJob` until a job finishes, you can use the `Wait-AdlJob` cmdlet to wait for the job to end.</span></span>

```powershell
Wait-AdlJob -Account $adla -JobId $job.JobId
```

## <a name="manage-compute-policies"></a><span data-ttu-id="b3ad1-199">Správa zásad výpočetní</span><span class="sxs-lookup"><span data-stu-id="b3ad1-199">Manage compute policies</span></span>

### <a name="list-existing-compute-policies"></a><span data-ttu-id="b3ad1-200">Seznam existujících zásad výpočetní</span><span class="sxs-lookup"><span data-stu-id="b3ad1-200">List existing compute policies</span></span>

<span data-ttu-id="b3ad1-201">`Get-AdlAnalyticsComputePolicy` Rutina načte informace o výpočetní zásady pro účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-201">The `Get-AdlAnalyticsComputePolicy` cmdlet retrieves info about compute policies for a Data Lake Analytics account.</span></span>

```powershell
$policies = Get-AdlAnalyticsComputePolicy -Account $adla
```

### <a name="create-a-compute-policy"></a><span data-ttu-id="b3ad1-202">Vytvořit zásadu výpočetní</span><span class="sxs-lookup"><span data-stu-id="b3ad1-202">Create a compute policy</span></span>

<span data-ttu-id="b3ad1-203">`New-AdlAnalyticsComputePolicy` Rutina vytvoří novou zásadu výpočetní pro účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-203">The `New-AdlAnalyticsComputePolicy` cmdlet creates a new compute policy for a Data Lake Analytics account.</span></span> <span data-ttu-id="b3ad1-204">Tento příklad nastaví maximální Austrálie k dispozici pro určeného uživatele na 50 a priority minimální úloh na 250.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-204">This example sets  the maximum AUs available to the specified user to 50, and the minimum job priority to 250.</span></span>

```powershell
$userObjectId = (Get-AzureRmAdUser -SearchString "garymcdaniel@contoso.com").Id

New-AdlAnalyticsComputePolicy -Account $adla -Name "GaryMcDaniel" -ObjectId $objectId -ObjectType User -MaxDegreeOfParallelismPerJob 50 -MinPriorityPerJob 250
```

## <a name="check-for-the-existence-of-a-file"></a><span data-ttu-id="b3ad1-205">Zkontrolujte, jestli soubor existuje.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-205">Check for the existence of a file.</span></span>

```powershell
Test-AdlStoreItem -Account $adls -Path "/data.csv"
```

## <a name="uploading-and-downloading"></a><span data-ttu-id="b3ad1-206">Odesílání a stahování</span><span class="sxs-lookup"><span data-stu-id="b3ad1-206">Uploading and downloading</span></span>

<span data-ttu-id="b3ad1-207">Nahrajte soubor.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-207">Upload a file.</span></span>

```powershell
Import-AdlStoreItem -AccountName $adls -Path "c:\data.tsv" -Destination "/data_copy.csv" 
```

<span data-ttu-id="b3ad1-208">Nahrajte rekurzivnímu celou složku.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-208">Upload an entire folder recursively.</span></span>

```powershell
Import-AdlStoreItem -AccountName $adls -Path "c:\myData\" -Destination "/myData/" -Recurse
```

<span data-ttu-id="b3ad1-209">Stažení souboru.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-209">Download a file.</span></span>

```powershell
Export-AdlStoreItem -AccountName $adls -Path "/data.csv" -Destination "c:\data.csv"
```

<span data-ttu-id="b3ad1-210">Stáhněte si rekurzivnímu celou složku.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-210">Download an entire folder recursively.</span></span>

```powershell
Export-AdlStoreItem -AccountName $adls -Path "/" -Destination "c:\myData\" -Recurse
```

> [!NOTE]
> <span data-ttu-id="b3ad1-211">Pokud dojde k přerušení procesu nahrávání nebo stahování, můžete se pokusit obnovit proces spuštěním rutiny znovu s ``-Resume`` příznak.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-211">If the upload or download process is interrupted, you can attempt to resume the process by running the cmdlet again with the ``-Resume`` flag.</span></span>

## <a name="manage-catalog-items"></a><span data-ttu-id="b3ad1-212">Spravovat položky katalogu</span><span class="sxs-lookup"><span data-stu-id="b3ad1-212">Manage catalog items</span></span>

<span data-ttu-id="b3ad1-213">Katalogu U-SQL se používá k struktury kódu a dat, může být sdílen skriptů U-SQL.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-213">The U-SQL catalog is used to structure data and code so they can be shared by U-SQL scripts.</span></span> <span data-ttu-id="b3ad1-214">Katalogu umožňuje nejvyšší možný s daty v Azure Data Lake výkon.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-214">The catalog enables the highest performance possible with data in Azure Data Lake.</span></span> <span data-ttu-id="b3ad1-215">Další informace najdete v tématu [Použití katalogu U-SQL](data-lake-analytics-use-u-sql-catalog.md).</span><span class="sxs-lookup"><span data-stu-id="b3ad1-215">For more information, see [Use U-SQL catalog](data-lake-analytics-use-u-sql-catalog.md).</span></span>

### <a name="list-items-in-the-u-sql-catalog"></a><span data-ttu-id="b3ad1-216">Položky seznamu v katalogu U-SQL</span><span class="sxs-lookup"><span data-stu-id="b3ad1-216">List items in the U-SQL catalog</span></span>

```powershell
# List U-SQL databases
Get-AdlCatalogItem -Account $adla -ItemType Database 

# List tables within a database
Get-AdlCatalogItem -Account $adla -ItemType Table -Path "database"

# List tables within a schema.
Get-AdlCatalogItem -Account $adla -ItemType Table -Path "database.schema"
```

<span data-ttu-id="b3ad1-217">Zobrazí seznam všech sestaveních ve všech databází na ADLA účtu.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-217">List all the assemblies in all the databases in an ADLA Account.</span></span>

```powershell
$dbs = Get-AdlCatalogItem -Account $adla -ItemType Database

foreach ($db in $dbs)
{
    $asms = Get-AdlCatalogItem -Account $adla -ItemType Assembly -Path $db.Name

    foreach ($asm in $asms)
    {
        $asmname = "[" + $db.Name + "].[" + $asm.Name + "]"
        Write-Host $asmname
    }
}
```

### <a name="get-details-about-a-catalog-item"></a><span data-ttu-id="b3ad1-218">Získání podrobností o položek katalogu</span><span class="sxs-lookup"><span data-stu-id="b3ad1-218">Get details about a catalog item</span></span>

```powershell
# Get details of a table
Get-AdlCatalogItem  -Account $adla -ItemType Table -Path "master.dbo.mytable"

# Test existence of a U-SQL database.
Test-AdlCatalogItem  -Account $adla -ItemType Database -Path "master"
```

### <a name="create-credentials-in-a-catalog"></a><span data-ttu-id="b3ad1-219">Vytvořit přihlašovací údaje v katalogu</span><span class="sxs-lookup"><span data-stu-id="b3ad1-219">Create credentials in a catalog</span></span>

<span data-ttu-id="b3ad1-220">V databázi U-SQL vytvořte objekt přihlašovacích údajů pro databázi hostované v Azure.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-220">Within a U-SQL database, create a credential object for a database hosted in Azure.</span></span> <span data-ttu-id="b3ad1-221">Přihlašovací údaje U-SQL v současné době jsou jediným typem položek katalogu, které můžete vytvořit pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-221">Currently, U-SQL credentials are the only type of catalog item that you can create through PowerShell.</span></span>

```powershell
$dbName = "master"
$credentialName = "ContosoDbCreds"
$dbUri = "https://contoso.database.windows.net:8080"

New-AdlCatalogCredential -AccountName $adla `
          -DatabaseName $db `
          -CredentialName $credentialName `
          -Credential (Get-Credential) `
          -Uri $dbUri
```

### <a name="get-basic-information-about-an-adla-account"></a><span data-ttu-id="b3ad1-222">Získejte základní informace o účtu ADLA</span><span class="sxs-lookup"><span data-stu-id="b3ad1-222">Get basic information about an ADLA account</span></span>

<span data-ttu-id="b3ad1-223">Zadaný název účtu, vyhledá následující kód základní informace o účtu</span><span class="sxs-lookup"><span data-stu-id="b3ad1-223">Given an account name, the following code looks up basic information about the account</span></span>

```
$adla_acct = Get-AdlAnalyticsAccount -Name "saveenrdemoadla"
$adla_name = $adla_acct.Name
$adla_subid = $adla_acct.Id.Split("/")[2]
$adla_sub = Get-AzureRmSubscription -SubscriptionId $adla_subid
$adla_subname = $adla_sub.Name
$adla_defadls_datasource = Get-AdlAnalyticsDataSource -Account $adla_name  | ? { $_.IsDefault } 
$adla_defadlsname = $adla_defadls_datasource.Name

Write-Host "ADLA Account Name" $adla_name
Write-Host "Subscription Id" $adla_subid
Write-Host "Subscription Name" $adla_subname
Write-Host "Defautl ADLS Store" $adla_defadlsname
Write-Host 

Write-Host '$subname' " = ""$adla_subname"" "
Write-Host '$subid' " = ""$adla_subid"" "
Write-Host '$adla' " = ""$adla_name"" "
Write-Host '$adls' " = ""$adla_defadlsname"" "
```

## <a name="working-with-azure"></a><span data-ttu-id="b3ad1-224">Práce s Azure</span><span class="sxs-lookup"><span data-stu-id="b3ad1-224">Working with Azure</span></span>

### <a name="get-details-of-azurerm-errors"></a><span data-ttu-id="b3ad1-225">Získat podrobnosti o chybách AzureRm</span><span class="sxs-lookup"><span data-stu-id="b3ad1-225">Get details of AzureRm errors</span></span>

```powershell
Resolve-AzureRmError -Last
```

### <a name="verify-if-you-are-running-as-an-administrator"></a><span data-ttu-id="b3ad1-226">Ověřte, zda používáte jako správce</span><span class="sxs-lookup"><span data-stu-id="b3ad1-226">Verify if you are running as an administrator</span></span>

```powershell
function Test-Administrator  
{  
    $user = [Security.Principal.WindowsIdentity]::GetCurrent();
    $p = New-Object Security.Principal.WindowsPrincipal $user
    $p.IsInRole([Security.Principal.WindowsBuiltinRole]::Administrator)  
}
```

### <a name="find-a-tenantid"></a><span data-ttu-id="b3ad1-227">Najít TenantID</span><span class="sxs-lookup"><span data-stu-id="b3ad1-227">Find a TenantID</span></span>

<span data-ttu-id="b3ad1-228">Z názvu předplatného:</span><span class="sxs-lookup"><span data-stu-id="b3ad1-228">From a subscription name:</span></span>

```powershell
function Get-TenantIdFromSubcriptionName( [string] $subname )
{
    $sub = (Get-AzureRmSubscription -SubscriptionName $subname)
    $sub.TenantId
}

Get-TenantIdFromSubcriptionName "ADLTrainingMS"
```

<span data-ttu-id="b3ad1-229">Z id předplatného:</span><span class="sxs-lookup"><span data-stu-id="b3ad1-229">From a subscription id:</span></span>

```powershell
function Get-TenantIdFromSubcriptionId( [string] $subid )
{
    $sub = (Get-AzureRmSubscription -SubscriptionId $subid)
    $sub.TenantId
}

$subid = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
Get-TenantIdFromSubcriptionId $subid
```

<span data-ttu-id="b3ad1-230">Z adresy domény jako je například "contoso.com"</span><span class="sxs-lookup"><span data-stu-id="b3ad1-230">From a domain address such as "contoso.com"</span></span>


```powershell
function Get-TenantIdFromDomain( $domain )
{
    $url = "https://login.windows.net/" + $domain + "/.well-known/openid-configuration"
    return (Invoke-WebRequest $url|ConvertFrom-Json).token_endpoint.Split('/')[3]
}

$domain = "contoso.com"
Get-TenantIdFromDomain $domain
```

### <a name="list-all-your-subscriptions-and-tenant-ids"></a><span data-ttu-id="b3ad1-231">Zobrazí seznam všech odběrů a ID klienta</span><span class="sxs-lookup"><span data-stu-id="b3ad1-231">List all your subscriptions and tenant ids</span></span>

```powershell
$subs = Get-AzureRmSubscription
foreach ($sub in $subs)
{
    Write-Host $sub.Name "("  $sub.Id ")"
    Write-Host "`tTenant Id" $sub.TenantId
}
```

## <a name="create-a-data-lake-analytics-account-using-a-template"></a><span data-ttu-id="b3ad1-232">Vytvoření účtu Data Lake Analytics pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="b3ad1-232">Create a Data Lake Analytics account using a template</span></span>

<span data-ttu-id="b3ad1-233">Můžete také použít šablonu skupiny prostředků Azure pomocí následujícího skriptu prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="b3ad1-233">You can also use an Azure Resource Group template using the following  PowerShell script:</span></span>

```powershell
$subId = "<Your Azure Subscription ID>"

$rg = "<New Azure Resource Group Name>"
$location = "<Location (such as East US 2)>"
$adls = "<New Data Lake Store Account Name>"
$adla = "<New Data Lake Analytics Account Name>"

$deploymentName = "MyDataLakeAnalyticsDeployment"
$armTemplateFile = "<LocalFolderPath>\azuredeploy.json"  # update the JSON template path 

# Log in to Azure
Login-AzureRmAccount -SubscriptionId $subId

# Create the resource group
New-AzureRmResourceGroup -Name $rg -Location $location

# Create the Data Lake Analytics account with the default Data Lake Store account.
$parameters = @{"adlAnalyticsName"=$adla; "adlStoreName"=$adls}
New-AzureRmResourceGroupDeployment -Name $deploymentName -ResourceGroupName $rg -TemplateFile $armTemplateFile -TemplateParameterObject $parameters 
```

<span data-ttu-id="b3ad1-234">Další informace najdete v tématu [nasazení aplikace pomocí šablony Azure Resource Manageru](../azure-resource-manager/resource-group-template-deploy.md) a [šablon pro tvorbu Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="b3ad1-234">For more information, see [Deploy an application with Azure Resource Manager template](../azure-resource-manager/resource-group-template-deploy.md) and [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="b3ad1-235">**Příklad šablony**</span><span class="sxs-lookup"><span data-stu-id="b3ad1-235">**Example template**</span></span>

<span data-ttu-id="b3ad1-236">Uložit jako následující text `.json` souboru a pak použijte předchozí skript prostředí PowerShell použít šablonu.</span><span class="sxs-lookup"><span data-stu-id="b3ad1-236">Save the following text as a `.json` file, and then use the preceding PowerShell script to use the template.</span></span> 

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adlAnalyticsName": {
      "type": "string",
      "metadata": {
        "description": "The name of the Data Lake Analytics account to create."
      }
    },
    "adlStoreName": {
      "type": "string",
      "metadata": {
        "description": "The name of the Data Lake Store account to create."
      }
    }
  },
  "resources": [
    {
      "name": "[parameters('adlStoreName')]",
      "type": "Microsoft.DataLakeStore/accounts",
      "location": "East US 2",
      "apiVersion": "2015-10-01-preview",
      "dependsOn": [ ],
      "tags": { }
    },
    {
      "name": "[parameters('adlAnalyticsName')]",
      "type": "Microsoft.DataLakeAnalytics/accounts",
      "location": "East US 2",
      "apiVersion": "2015-10-01-preview",
      "dependsOn": [ "[concat('Microsoft.DataLakeStore/accounts/',parameters('adlStoreName'))]" ],
      "tags": { },
      "properties": {
        "defaultDataLakeStoreAccount": "[parameters('adlStoreName')]",
        "dataLakeStoreAccounts": [
          { "name": "[parameters('adlStoreName')]" }
        ]
      }
    }
  ],
  "outputs": {
    "adlAnalyticsAccount": {
      "type": "object",
      "value": "[reference(resourceId('Microsoft.DataLakeAnalytics/accounts',parameters('adlAnalyticsName')))]"
    },
    "adlStoreAccount": {
      "type": "object",
      "value": "[reference(resourceId('Microsoft.DataLakeStore/accounts',parameters('adlStoreName')))]"
    }
  }
}
```

## <a name="next-steps"></a><span data-ttu-id="b3ad1-237">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b3ad1-237">Next steps</span></span>
* [<span data-ttu-id="b3ad1-238">Přehled služby Microsoft Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="b3ad1-238">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* <span data-ttu-id="b3ad1-239">Začínáme s Data Lake Analytics pomocí [portál Azure](data-lake-analytics-get-started-portal.md) | [prostředí Azure PowerShell](data-lake-analytics-get-started-powershell.md) | [2.0 rozhraní příkazového řádku](data-lake-analytics-get-started-cli2.md)</span><span class="sxs-lookup"><span data-stu-id="b3ad1-239">Get started with Data Lake Analytics using [Azure portal](data-lake-analytics-get-started-portal.md) | [Azure PowerShell](data-lake-analytics-get-started-powershell.md) | [CLI 2.0](data-lake-analytics-get-started-cli2.md)</span></span>
* <span data-ttu-id="b3ad1-240">Správa Azure Data Lake Analytics pomocí [portál Azure](data-lake-analytics-manage-use-portal.md) | [prostředí Azure PowerShell](data-lake-analytics-manage-use-powershell.md) | [rozhraní příkazového řádku](data-lake-analytics-manage-use-cli.md)</span><span class="sxs-lookup"><span data-stu-id="b3ad1-240">Manage Azure Data Lake Analytics using [Azure portal](data-lake-analytics-manage-use-portal.md) | [Azure PowerShell](data-lake-analytics-manage-use-powershell.md) | [CLI](data-lake-analytics-manage-use-cli.md)</span></span> 
