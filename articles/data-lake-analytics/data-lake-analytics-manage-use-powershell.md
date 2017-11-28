---
title: "aaaManage Azure Data Lake Analytics pomocí Azure PowerShell | Microsoft Docs"
description: "Zjistěte, jak toomanage účtů Data Lake Analytics, datových zdrojů, úloh a položek katalogu. "
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
ms.openlocfilehash: 5954f0efb7d5a9778727edfccae83aec046343bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-data-lake-analytics-using-azure-powershell"></a><span data-ttu-id="dd5fb-103">Správa Azure Data Lake Analytics pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="dd5fb-103">Manage Azure Data Lake Analytics using Azure PowerShell</span></span>
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

<span data-ttu-id="dd5fb-104">Zjistěte, jak toomanage účtů Azure Data Lake Analytics, datových zdrojů, úloh a položky katalogu pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-104">Learn how toomanage Azure Data Lake Analytics accounts, data sources, jobs, and catalog items using Azure PowerShell.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="dd5fb-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="dd5fb-105">Prerequisites</span></span>

<span data-ttu-id="dd5fb-106">Při vytváření účtu Data Lake Analytics, je třeba tooknow:</span><span class="sxs-lookup"><span data-stu-id="dd5fb-106">When creating a Data Lake Analytics account, you need tooknow:</span></span>

* <span data-ttu-id="dd5fb-107">**ID předplatného**: hello ID předplatného Azure, ve kterém se nachází váš účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-107">**Subscription ID**: hello Azure subscription ID under which your Data Lake Analytics account resides.</span></span>
* <span data-ttu-id="dd5fb-108">**Skupina prostředků**: hello název skupiny prostředků Azure hello, která obsahuje váš účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-108">**Resource group**: hello name of hello Azure resource group that contains your Data Lake Analytics account.</span></span>
* <span data-ttu-id="dd5fb-109">**Název účtu data Lake Analytics**: hello účet název musí obsahovat jenom malá písmena a číslice.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-109">**Data Lake Analytics account name**: hello account name must only contain lowercase letters and numbers.</span></span>
* <span data-ttu-id="dd5fb-110">**Výchozí účet Data Lake Store:** Každý účet Data Lake Analytics má výchozí účet Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-110">**Default Data Lake Store account**: Each Data Lake Analytics account has a default Data Lake Store account.</span></span> <span data-ttu-id="dd5fb-111">Tyto účty musí být v hello stejné umístění.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-111">These accounts must be in hello same location.</span></span>
* <span data-ttu-id="dd5fb-112">**Umístění**: hello umístění vašeho účtu Data Lake Analytics, jako je například "East US 2" nebo jiné podporované umístění.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-112">**Location**: hello location of your Data Lake Analytics account, such as "East US 2" or other supported locations.</span></span> <span data-ttu-id="dd5fb-113">Podporovaná umístění můžete zobrazit na našem [stránce s cenami](https://azure.microsoft.com/pricing/details/data-lake-analytics/).</span><span class="sxs-lookup"><span data-stu-id="dd5fb-113">Supported locations can be seen on our [pricing page](https://azure.microsoft.com/pricing/details/data-lake-analytics/).</span></span>

<span data-ttu-id="dd5fb-114">fragmenty kódu Hello prostředí PowerShell v tomto kurzu tyto informace použít tyto proměnné toostore</span><span class="sxs-lookup"><span data-stu-id="dd5fb-114">hello PowerShell snippets in this tutorial use these variables toostore this information</span></span>

```powershell
$subId = "<SubscriptionId>"
$rg = "<ResourceGroupName>"
$adla = "<DataLakeAnalyticsAccountName>"
$adls = "<DataLakeStoreAccountName>"
$location = "<Location>"
```

## <a name="log-in"></a><span data-ttu-id="dd5fb-115">Přihlásit se</span><span class="sxs-lookup"><span data-stu-id="dd5fb-115">Log in</span></span>

<span data-ttu-id="dd5fb-116">Přihlaste se pomocí id předplatného.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-116">Log in using a subscription id.</span></span>

```powershell
Login-AzureRmAccount -SubscriptionId $subId
```

<span data-ttu-id="dd5fb-117">Přihlaste se pomocí název odběru.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-117">Log in using a subscription name.</span></span>

```
Login-AzureRmAccount -SubscriptionName $subname 
```

<span data-ttu-id="dd5fb-118">Hello `Login-AzureRmAccount` rutiny vždycky zobrazí výzvu k zadání pověření.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-118">hello `Login-AzureRmAccount` cmdlet  always prompts for credentials.</span></span> <span data-ttu-id="dd5fb-119">Dotaz, zda pomocí následující rutiny hello se můžete vyhnout:</span><span class="sxs-lookup"><span data-stu-id="dd5fb-119">You can avoid being prompted by using hello following cmdlets:</span></span>

```powershell
# Save login session information
Save-AzureRmProfile -Path D:\profile.json  

# Load login session information
Select-AzureRmProfile -Path D:\profile.json 
```

## <a name="managing-accounts"></a><span data-ttu-id="dd5fb-120">Správa účtů</span><span class="sxs-lookup"><span data-stu-id="dd5fb-120">Managing accounts</span></span>

### <a name="create-a-data-lake-analytics-account"></a><span data-ttu-id="dd5fb-121">Vytvoření účtu Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="dd5fb-121">Create a Data Lake Analytics account</span></span>

<span data-ttu-id="dd5fb-122">Pokud ještě nemáte [skupiny prostředků](../azure-resource-manager/resource-group-overview.md#resource-groups) toouse, vytvořte ho.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-122">If you don't already have a [resource group](../azure-resource-manager/resource-group-overview.md#resource-groups) toouse, create one.</span></span> 

```powershell
New-AzureRmResourceGroup -Name  $rg -Location $location
```

<span data-ttu-id="dd5fb-123">Každý účet Data Lake Analytics vyžaduje výchozí účet Data Lake Store, který slouží k ukládání protokolů.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-123">Every Data Lake Analytics account requires a default Data Lake Store account that it uses for storing logs.</span></span> <span data-ttu-id="dd5fb-124">Můžete znovu použít existující účet nebo si vytvořit účet.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-124">You can reuse an existing account or create an account.</span></span> 

```powershell
New-AdlStore -ResourceGroupName $rg -Name $adls -Location $location
```

<span data-ttu-id="dd5fb-125">Jakmile budete mít k dispozici skupinu prostředků a účet Data Lake Store, vytvořte účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-125">Once a Resource Group and Data Lake Store account is available, create a Data Lake Analytics account.</span></span>

```powershell
New-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla -Location $location -DefaultDataLake $adls
```

### <a name="get-information-about-an-account"></a><span data-ttu-id="dd5fb-126">Získání informací o účtu</span><span class="sxs-lookup"><span data-stu-id="dd5fb-126">Get information about an account</span></span>

<span data-ttu-id="dd5fb-127">Získejte podrobnosti o účtu.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-127">Get details about an account.</span></span>

```powershell
Get-AdlAnalyticsAccount -Name $adla
```

<span data-ttu-id="dd5fb-128">Zkontrolujte existenci hello určitého účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-128">Check hello existence of a specific Data Lake Analytics account.</span></span> <span data-ttu-id="dd5fb-129">Hello rutina vrací buď `True` nebo `False`.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-129">hello cmdlet returns either `True` or `False`.</span></span>

```powershell
Test-AdlAnalyticsAccount -Name $adla
```

<span data-ttu-id="dd5fb-130">Zkontrolujte existenci hello určitého účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-130">Check hello existence of a specific Data Lake Store account.</span></span> <span data-ttu-id="dd5fb-131">Hello rutina vrací buď `True` nebo `False`.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-131">hello cmdlet returns either `True` or `False`.</span></span>

```powershell
Test-AdlStoreAccount -Name $adls
```

### <a name="listing-accounts"></a><span data-ttu-id="dd5fb-132">Výpis účtů</span><span class="sxs-lookup"><span data-stu-id="dd5fb-132">Listing accounts</span></span>

<span data-ttu-id="dd5fb-133">Seznam Data Lake Analytics účtů v rámci aktuálního předplatného hello.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-133">List Data Lake Analytics accounts within hello current subscription.</span></span>

```powershell
Get-AdlAnalyticsAccount
```

<span data-ttu-id="dd5fb-134">Seznam Data Lake Analytics účtů v rámci určité skupiny zdrojů.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-134">List Data Lake Analytics accounts within a specific resource group.</span></span>

```powershell
Get-AdlAnalyticsAccount -ResourceGroupName $rg
```

## <a name="managing-firewall-rules"></a><span data-ttu-id="dd5fb-135">Správa pravidel brány firewall</span><span class="sxs-lookup"><span data-stu-id="dd5fb-135">Managing firewall rules</span></span>

<span data-ttu-id="dd5fb-136">Zobrazí seznam pravidel brány firewall.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-136">List firewall rules.</span></span>

```powershell
Get-AdlAnalyticsFirewallRule -Account $adla
```

<span data-ttu-id="dd5fb-137">Přidejte pravidlo brány firewall.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-137">Add a firewall rule.</span></span>

```powershell
$ruleName = "Allow access from on-prem server"
$startIpAddress = "<start IP address>"
$endIpAddress = "<end IP address>"

Add-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName -StartIpAddress $startIpAddress -EndIpAddress $endIpAddress
```

<span data-ttu-id="dd5fb-138">Změňte pravidlo brány firewall.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-138">Change a firewall rule.</span></span>

```powershell
Set-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName -StartIpAddress $startIpAddress -EndIpAddress $endIpAddress
```

<span data-ttu-id="dd5fb-139">Odeberte pravidlo brány firewall.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-139">Remove a firewall rule.</span></span>

```powershell
Remove-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName
```



<span data-ttu-id="dd5fb-140">Povolit Azure IP adresy.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-140">Allow Azure IP addresses.</span></span>

```powershell
Set-AdlAnalyticsAccount -Name $adla -AllowAzureIpState Enabled
```

```powershell
Set-AdlAnalyticsAccount -Name $adla -FirewallState Enabled
Set-AdlAnalyticsAccount -Name $adla -FirewallState Disabled
```

## <a name="managing-data-sources"></a><span data-ttu-id="dd5fb-141">Správa zdrojů dat</span><span class="sxs-lookup"><span data-stu-id="dd5fb-141">Managing data sources</span></span>
<span data-ttu-id="dd5fb-142">Azure Data Lake Analytics teď podporuje hello následující zdroje dat:</span><span class="sxs-lookup"><span data-stu-id="dd5fb-142">Azure Data Lake Analytics currently supports hello following data sources:</span></span>

* [<span data-ttu-id="dd5fb-143">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="dd5fb-143">Azure Data Lake Store</span></span>](../data-lake-store/data-lake-store-overview.md)
* [<span data-ttu-id="dd5fb-144">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="dd5fb-144">Azure Storage</span></span>](../storage/common/storage-introduction.md)

<span data-ttu-id="dd5fb-145">Když vytvoříte účet Analytics, je třeba určit Data Lake Store účtu toobe hello výchozí datový zdroj.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-145">When you create an Analytics account, you must designate a Data Lake Store account toobe hello default data source.</span></span> <span data-ttu-id="dd5fb-146">Hello výchozího účtu Data Lake Store se používá protokoly auditu metadata toostore úlohy a úlohy.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-146">hello default Data Lake Store account is used toostore job metadata and job audit logs.</span></span> <span data-ttu-id="dd5fb-147">Po vytvoření účtu Data Lake Analytics, můžete přidat další účty Data Lake Store a účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-147">After you have created a Data Lake Analytics account, you can add additional Data Lake Store accounts and/or Storage accounts.</span></span> 

### <a name="find-hello-default-data-lake-store-account"></a><span data-ttu-id="dd5fb-148">Najít hello výchozího účtu Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="dd5fb-148">Find hello default Data Lake Store account</span></span>

```powershell
$adla_acct = Get-AdlAnalyticsAccount -Name $adla
$dataLakeStoreName = $adla_acct.DefaultDataLakeAccount
```

<span data-ttu-id="dd5fb-149">Hello výchozího účtu Data Lake Store můžete najít pomocí filtrování hello seznam zdrojů dat pomocí hello `IsDefault` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="dd5fb-149">You can find hello default Data Lake Store account by filtering hello list of datasources by hello `IsDefault` property:</span></span>

```powershell
Get-AdlAnalyticsDataSource -Account $adla  | ? { $_.IsDefault } 
```

### <a name="add-a-data-source"></a><span data-ttu-id="dd5fb-150">Přidání zdroje dat</span><span class="sxs-lookup"><span data-stu-id="dd5fb-150">Add a data source</span></span>

```powershell

# Add an additional Storage (Blob) account.
$AzureStorageAccountName = "<AzureStorageAccountName>"
$AzureStorageAccountKey = "<AzureStorageAccountKey>"
Add-AdlAnalyticsDataSource -Account $adla -Blob $AzureStorageAccountName -AccessKey $AzureStorageAccountKey

# Add an additional Data Lake Store account.
$AzureDataLakeStoreName = "<AzureDataLakeStoreAccountName"
Add-AdlAnalyticsDataSource -Account $adla -DataLakeStore $AzureDataLakeStoreName 
```

### <a name="list-data-sources"></a><span data-ttu-id="dd5fb-151">Seznam zdrojů dat</span><span class="sxs-lookup"><span data-stu-id="dd5fb-151">List data sources</span></span>

```powershell
# List all hello data sources
Get-AdlAnalyticsDataSource -Name $adla

# List attached Data Lake Store accounts
Get-AdlAnalyticsDataSource -Name $adla | where -Property Type -EQ "DataLakeStore"

# List attached Storage accounts
Get-AdlAnalyticsDataSource -Name $adla | where -Property Type -EQ "Blob"
```

## <a name="submit-u-sql-jobs"></a><span data-ttu-id="dd5fb-152">Odesílání úloh U-SQL</span><span class="sxs-lookup"><span data-stu-id="dd5fb-152">Submit U-SQL jobs</span></span>

### <a name="submit-a-string-as-a-u-sql-script"></a><span data-ttu-id="dd5fb-153">Odeslat řetězec jako skript U-SQL</span><span class="sxs-lookup"><span data-stu-id="dd5fb-153">Submit a string as a U-SQL script</span></span>

```powershell
$script = @"
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS D( customer, amount );
OUTPUT @a
    too"/data.csv"
    USING Outputters.Csv();
"@

$scriptpath = "d:\test.usql"
$script | Out-File $scriptpath 

Submit-AdlJob -AccountName $adla -Script $script -Name "Demo"
```


### <a name="submit-a-file-as-a-u-sql-script"></a><span data-ttu-id="dd5fb-154">Odeslat soubor jako skript U-SQL</span><span class="sxs-lookup"><span data-stu-id="dd5fb-154">Submit a file as a U-SQL script</span></span>

```powershell
$scriptpath = "d:\test.usql"
$script | Out-File $scriptpath 
Submit-AdlJob -AccountName $adla –ScriptPath $scriptpath -Name "Demo"
```

## <a name="list-jobs-in-an-account"></a><span data-ttu-id="dd5fb-155">Seznam úloh v účtu</span><span class="sxs-lookup"><span data-stu-id="dd5fb-155">List jobs in an account</span></span>

### <a name="list-all-hello-jobs-in-hello-account"></a><span data-ttu-id="dd5fb-156">Zobrazí seznam všech úloh hello v účtu hello.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-156">List all hello jobs in hello account.</span></span> 

<span data-ttu-id="dd5fb-157">výstup Hello zahrnuje hello aktuálně spuštěných úloh a tyto úlohy, které byly nedávno dokončeny.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-157">hello output includes hello currently running jobs and those jobs that have recently completed.</span></span>

```powershell
Get-AdlJob -Account $adla
```


### <a name="list-a-specific-number-of-jobs"></a><span data-ttu-id="dd5fb-158">Seznam určitý počet úloh</span><span class="sxs-lookup"><span data-stu-id="dd5fb-158">List a specific number of jobs</span></span>

<span data-ttu-id="dd5fb-159">Ve výchozím nastavení je hello seznam úloh seřadit na odeslání čas.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-159">By default hello list of jobs is sorted on submit time.</span></span> <span data-ttu-id="dd5fb-160">Tak hello naposledy odeslání úlohy se zobrazí na prvním.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-160">So hello most recently submitted jobs appear first.</span></span> <span data-ttu-id="dd5fb-161">Ve výchozím nastavení hello ADLA účet pamatuje úlohy na 180 dní, ale hello Ge AdlJob rutina ve výchozím nastavení vrátí hello pouze první 500.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-161">By default, hello ADLA account remembers jobs for 180 days, but hello Ge-AdlJob  cmdlet by default returns only hello first 500.</span></span> <span data-ttu-id="dd5fb-162">Pomocí - nejvyšší parametr toolist určitý počet úloh.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-162">Use -Top parameter toolist a specific number of jobs.</span></span>

```powershell
$jobs = Get-AdlJob -Account $adla -Top 10
```


### <a name="list-jobs-based-on-hello-value-of-job-property"></a><span data-ttu-id="dd5fb-163">Seznam úloh na základě hello hodnoty vlastnosti úlohy</span><span class="sxs-lookup"><span data-stu-id="dd5fb-163">List jobs based on hello value of job property</span></span>

<span data-ttu-id="dd5fb-164">Pomocí hello `-State` parametr.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-164">Using hello `-State` parameter.</span></span> <span data-ttu-id="dd5fb-165">Zkombinováním některou z těchto hodnot:</span><span class="sxs-lookup"><span data-stu-id="dd5fb-165">You can combine any of these values:</span></span>

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
# List hello running jobs
Get-AdlJob -Account $adla -State Running

# List hello jobs that have completed
Get-AdlJob -Account $adla -State Ended

# List hello jobs that have not started yet
Get-AdlJob -Account $adla -State Accepted,Compiling,New,Paused,Scheduling,Start
```

<span data-ttu-id="dd5fb-166">Použití hello `-Result` parametr toodetect zda zakončeno úlohy byl úspěšně dokončen.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-166">Use hello `-Result` parameter toodetect whether ended jobs completed successfully.</span></span> <span data-ttu-id="dd5fb-167">Obsahuje tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="dd5fb-167">It has these values:</span></span>

* <span data-ttu-id="dd5fb-168">Zrušena</span><span class="sxs-lookup"><span data-stu-id="dd5fb-168">Cancelled</span></span>
* <span data-ttu-id="dd5fb-169">Se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="dd5fb-169">Failed</span></span>
* <span data-ttu-id="dd5fb-170">Žádný</span><span class="sxs-lookup"><span data-stu-id="dd5fb-170">None</span></span>
* <span data-ttu-id="dd5fb-171">Úspěch</span><span class="sxs-lookup"><span data-stu-id="dd5fb-171">Succeeded</span></span>

``` powershell
# List Successful jobs.
Get-AdlJob -Account $adla -State Ended -Result Succeeded

# List Failed jobs.
Get-AdlJob -Account $adla -State Ended -Result Failed
```


<span data-ttu-id="dd5fb-172">Hello `-Submitter` parametr pomáhá identifikovat, kdo odeslání úlohy.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-172">hello `-Submitter` parameter helps you identify who submitted a job.</span></span>

```powershell
Get-AdlJob -Account $adla -Submitter "joe@contoso.com"
```

<span data-ttu-id="dd5fb-173">Hello `-SubmittedAfter` je užitečný při filtrování tooa časový rozsah.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-173">hello `-SubmittedAfter` is useful in filtering tooa time range.</span></span>


```powershell
# List  jobs submitted in hello last day.
$d = [DateTime]::Now.AddDays(-1)
Get-AdlJob -Account $adla -SubmittedAfter $d

# List  jobs submitted in hello last seven day.
$d = [DateTime]::Now.AddDays(-7)
Get-AdlJob -Account $adla -SubmittedAfter $d
```

### <a name="common-scenarios-for-listing-jobs"></a><span data-ttu-id="dd5fb-174">Časté scénáře pro seznam úloh</span><span class="sxs-lookup"><span data-stu-id="dd5fb-174">Common scenarios for listing jobs</span></span>


```
# List jobs submitted in hello last five days and that successfully completed.
$d = (Get-Date).AddDays(-5)
Get-AdlJob -Account $adla -SubmittedAfter $d -State Ended -Result Succeeded

# List all failed jobs submitted by "joe@contoso.com" within hello past seven days.
Get-AdlJob -Account $adla `
    -Submitter "joe@contoso.com" `
    -SubmittedAfter (Get-Date).AddDays(-7) `
    -Result Failed
```

## <a name="filtering-a-list-of-jobs"></a><span data-ttu-id="dd5fb-175">Filtrování seznamu úloh</span><span class="sxs-lookup"><span data-stu-id="dd5fb-175">Filtering a list of jobs</span></span>

<span data-ttu-id="dd5fb-176">Jakmile máte seznam úloh v aktuální relaci prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-176">Once you have a list of jobs in your current PowerShell session.</span></span> <span data-ttu-id="dd5fb-177">Můžete použít normální prostředí PowerShell rutiny toofilter hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-177">You can use normal PowerShell cmdlets toofilter hello list.</span></span>

<span data-ttu-id="dd5fb-178">Filtr seznam úloh toohello úlohy odeslání v hello posledních 24 hodin</span><span class="sxs-lookup"><span data-stu-id="dd5fb-178">Filter a list of jobs toohello jobs submitted in hello last 24 hours</span></span>

```
$upperdate = Get-Date
$lowerdate = $upperdate.AddHours(-24)
$jobs | Where-Object { $_.EndTime -ge $lowerdate }
```

<span data-ttu-id="dd5fb-179">Filtrovat seznam úloh toohello úlohy, které skončila v hello posledních 24 hodin</span><span class="sxs-lookup"><span data-stu-id="dd5fb-179">Filter a list of jobs toohello jobs that ended in hello last 24 hours</span></span>

```
$upperdate = Get-Date
$lowerdate = $upperdate.AddHours(-24)
$jobs | Where-Object { $_.SubmitTime -ge $lowerdate }
```

<span data-ttu-id="dd5fb-180">Filtrovat seznam úloh toohello úlohy, které spuštění.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-180">Filter a list of jobs toohello jobs that started running.</span></span> <span data-ttu-id="dd5fb-181">Úloha může selhat v době kompilace - a proto se nespustí.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-181">A job might fail at compile time - and so it never starts.</span></span> <span data-ttu-id="dd5fb-182">Podívejme se na úlohy hello se nezdařilo, které ve skutečnosti spuštění a pak se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-182">Let's look at hello failed jobs that actually started running and then failed.</span></span>

```powershell
$jobs | Where-Object { $_.StartTime -ne $null }
```

### <a name="analyzing-a-list-of-jobs"></a><span data-ttu-id="dd5fb-183">Analýza seznam úloh</span><span class="sxs-lookup"><span data-stu-id="dd5fb-183">Analyzing a list of jobs</span></span>

<span data-ttu-id="dd5fb-184">Použití hello `Group-Object` rutiny tooanalyze seznam úloh.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-184">Use hello `Group-Object` cmdlet tooanalyze a list of jobs.</span></span>

```
# Count hello number of jobs by Submitter
$jobs | Group-Object Submitter | Select -Property Count,Name

# Count hello number of jobs by Result
$jobs | Group-Object Result | Select -Property Count,Name

# Count hello number of jobs by State
$jobs | Group-Object State | Select -Property Count,Name

#  Count hello number of jobs by DegreeOfParallelism
$jobs | Group-Object DegreeOfParallelism | Select -Property Count,Name
```
<span data-ttu-id="dd5fb-185">Při provádění analýzu, může být užitečné tooadd vlastnosti toohello úlohy objekty toomake filtrování a seskupení jednodušší.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-185">When performing an analysis, it can be useful tooadd properties toohello Job objects toomake filtering and grouping simpler.</span></span> <span data-ttu-id="dd5fb-186">Hello následující fragment kódu ukazuje, jak tooannotate a informací o úloze s vypočítat vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-186">hello following  snippet shows how tooannotate a JobInfo with calculated properties.</span></span>

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

## <a name="get-information-about-pipelines-and-recurrences"></a><span data-ttu-id="dd5fb-187">Získání informací o kanálů a opakování</span><span class="sxs-lookup"><span data-stu-id="dd5fb-187">Get information about pipelines and recurrences</span></span>

<span data-ttu-id="dd5fb-188">Použití hello `Get-AdlJobPipeline` rutiny toosee hello kanálu informace dříve odeslání úlohy.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-188">Use hello `Get-AdlJobPipeline` cmdlet toosee hello pipeline information previously submitted jobs.</span></span>

```powershell
$pipelines = Get-AdlJobPipeline -Account $adla

$pipeline = Get-AdlJobPipeline -Account $adla -PipelineId "<pipeline ID>"
```

<span data-ttu-id="dd5fb-189">Použití hello `Get-AdlJobRecurrence` rutiny toosee hello opakování informace pro dříve odeslaný úlohy.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-189">Use hello `Get-AdlJobRecurrence` cmdlet toosee hello recurrence information for previously submitted jobs.</span></span>

```powershell
$recurrences = Get-AdlJobRecurrence -Account $adla

$recurrence = Get-AdlJobRecurrence -Account $adla -RecurrenceId "<recurrence ID>"
```

## <a name="get-information-about-a-job"></a><span data-ttu-id="dd5fb-190">Získání informací o úlohy</span><span class="sxs-lookup"><span data-stu-id="dd5fb-190">Get information about a job</span></span>

### <a name="get-job-status"></a><span data-ttu-id="dd5fb-191">Získat stav úlohy</span><span class="sxs-lookup"><span data-stu-id="dd5fb-191">Get job status</span></span>

<span data-ttu-id="dd5fb-192">Získáte stav hello určité úlohy.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-192">Get hello status of a specific job.</span></span>

```powershell
Get-AdlJob -AccountName $adla -JobId $job.JobId
```

### <a name="examine-hello-job-outputs"></a><span data-ttu-id="dd5fb-193">Zkontrolujte výstupy úlohy hello</span><span class="sxs-lookup"><span data-stu-id="dd5fb-193">Examine hello job outputs</span></span>

<span data-ttu-id="dd5fb-194">Po ukončení úlohy hello, zkontrolujte, jestli hello výstupní soubor existuje tak, že uvedete hello souborů ve složce.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-194">After hello job has ended, check if hello output file exists by listing hello files in a folder.</span></span>

```powershell
Get-AdlStoreChildItem -Account $adls -Path "/"
```

## <a name="manage-running-jobs"></a><span data-ttu-id="dd5fb-195">Spravovat spuštěné úlohy</span><span class="sxs-lookup"><span data-stu-id="dd5fb-195">Manage running jobs</span></span>

### <a name="cancel-a-job"></a><span data-ttu-id="dd5fb-196">Zrušení úlohy</span><span class="sxs-lookup"><span data-stu-id="dd5fb-196">Cancel a job</span></span>

```powershell
Stop-AdlJob -Account $adls -JobID $jobID
```

### <a name="wait-for-a-job-toofinish"></a><span data-ttu-id="dd5fb-197">Počkejte toofinish úlohy</span><span class="sxs-lookup"><span data-stu-id="dd5fb-197">Wait for a job toofinish</span></span>

<span data-ttu-id="dd5fb-198">Místo opakující se `Get-AdlAnalyticsJob` , dokud nebude dokončeno úlohu, můžete použít hello `Wait-AdlJob` toowait rutiny pro úlohy tooend hello.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-198">Instead of repeating `Get-AdlAnalyticsJob` until a job finishes, you can use hello `Wait-AdlJob` cmdlet toowait for hello job tooend.</span></span>

```powershell
Wait-AdlJob -Account $adla -JobId $job.JobId
```

## <a name="manage-compute-policies"></a><span data-ttu-id="dd5fb-199">Správa zásad výpočetní</span><span class="sxs-lookup"><span data-stu-id="dd5fb-199">Manage compute policies</span></span>

### <a name="list-existing-compute-policies"></a><span data-ttu-id="dd5fb-200">Seznam existujících zásad výpočetní</span><span class="sxs-lookup"><span data-stu-id="dd5fb-200">List existing compute policies</span></span>

<span data-ttu-id="dd5fb-201">Hello `Get-AdlAnalyticsComputePolicy` rutina načte informace o výpočetní zásady pro účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-201">hello `Get-AdlAnalyticsComputePolicy` cmdlet retrieves info about compute policies for a Data Lake Analytics account.</span></span>

```powershell
$policies = Get-AdlAnalyticsComputePolicy -Account $adla
```

### <a name="create-a-compute-policy"></a><span data-ttu-id="dd5fb-202">Vytvořit zásadu výpočetní</span><span class="sxs-lookup"><span data-stu-id="dd5fb-202">Create a compute policy</span></span>

<span data-ttu-id="dd5fb-203">Hello `New-AdlAnalyticsComputePolicy` rutina vytvoří novou zásadu výpočetní pro účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-203">hello `New-AdlAnalyticsComputePolicy` cmdlet creates a new compute policy for a Data Lake Analytics account.</span></span> <span data-ttu-id="dd5fb-204">Tento příklad, že nastaví hello maximální dostupné toohello Austrálie zadané uživatele too50 a too250 s prioritou hello minimální úlohy.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-204">This example sets  hello maximum AUs available toohello specified user too50, and hello minimum job priority too250.</span></span>

```powershell
$userObjectId = (Get-AzureRmAdUser -SearchString "garymcdaniel@contoso.com").Id

New-AdlAnalyticsComputePolicy -Account $adla -Name "GaryMcDaniel" -ObjectId $objectId -ObjectType User -MaxDegreeOfParallelismPerJob 50 -MinPriorityPerJob 250
```

## <a name="check-for-hello-existence-of-a-file"></a><span data-ttu-id="dd5fb-205">Kontrola hello existenci souboru.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-205">Check for hello existence of a file.</span></span>

```powershell
Test-AdlStoreItem -Account $adls -Path "/data.csv"
```

## <a name="uploading-and-downloading"></a><span data-ttu-id="dd5fb-206">Odesílání a stahování</span><span class="sxs-lookup"><span data-stu-id="dd5fb-206">Uploading and downloading</span></span>

<span data-ttu-id="dd5fb-207">Nahrajte soubor.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-207">Upload a file.</span></span>

```powershell
Import-AdlStoreItem -AccountName $adls -Path "c:\data.tsv" -Destination "/data_copy.csv" 
```

<span data-ttu-id="dd5fb-208">Nahrajte rekurzivnímu celou složku.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-208">Upload an entire folder recursively.</span></span>

```powershell
Import-AdlStoreItem -AccountName $adls -Path "c:\myData\" -Destination "/myData/" -Recurse
```

<span data-ttu-id="dd5fb-209">Stažení souboru.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-209">Download a file.</span></span>

```powershell
Export-AdlStoreItem -AccountName $adls -Path "/data.csv" -Destination "c:\data.csv"
```

<span data-ttu-id="dd5fb-210">Stáhněte si rekurzivnímu celou složku.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-210">Download an entire folder recursively.</span></span>

```powershell
Export-AdlStoreItem -AccountName $adls -Path "/" -Destination "c:\myData\" -Recurse
```

> [!NOTE]
> <span data-ttu-id="dd5fb-211">Pokud hello nahrát nebo dojde k přerušení procesu stahování, můžete se pokusit tooresume hello proces spuštěný rutiny hello znovu s hello ``-Resume`` příznak.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-211">If hello upload or download process is interrupted, you can attempt tooresume hello process by running hello cmdlet again with hello ``-Resume`` flag.</span></span>

## <a name="manage-catalog-items"></a><span data-ttu-id="dd5fb-212">Spravovat položky katalogu</span><span class="sxs-lookup"><span data-stu-id="dd5fb-212">Manage catalog items</span></span>

<span data-ttu-id="dd5fb-213">katalog Hello U-SQL je použité toostructure dat a kódu, takže může být sdílen skriptů U-SQL.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-213">hello U-SQL catalog is used toostructure data and code so they can be shared by U-SQL scripts.</span></span> <span data-ttu-id="dd5fb-214">katalog Hello umožňuje hello nejvyšší možný výkon s daty v Azure Data Lake.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-214">hello catalog enables hello highest performance possible with data in Azure Data Lake.</span></span> <span data-ttu-id="dd5fb-215">Další informace najdete v tématu [Použití katalogu U-SQL](data-lake-analytics-use-u-sql-catalog.md).</span><span class="sxs-lookup"><span data-stu-id="dd5fb-215">For more information, see [Use U-SQL catalog](data-lake-analytics-use-u-sql-catalog.md).</span></span>

### <a name="list-items-in-hello-u-sql-catalog"></a><span data-ttu-id="dd5fb-216">Položky seznamu v katalogu hello U-SQL</span><span class="sxs-lookup"><span data-stu-id="dd5fb-216">List items in hello U-SQL catalog</span></span>

```powershell
# List U-SQL databases
Get-AdlCatalogItem -Account $adla -ItemType Database 

# List tables within a database
Get-AdlCatalogItem -Account $adla -ItemType Table -Path "database"

# List tables within a schema.
Get-AdlCatalogItem -Account $adla -ItemType Table -Path "database.schema"
```

<span data-ttu-id="dd5fb-217">Zobrazí seznam všech hello sestavení v všechny databáze hello ADLA účtu.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-217">List all hello assemblies in all hello databases in an ADLA Account.</span></span>

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

### <a name="get-details-about-a-catalog-item"></a><span data-ttu-id="dd5fb-218">Získání podrobností o položek katalogu</span><span class="sxs-lookup"><span data-stu-id="dd5fb-218">Get details about a catalog item</span></span>

```powershell
# Get details of a table
Get-AdlCatalogItem  -Account $adla -ItemType Table -Path "master.dbo.mytable"

# Test existence of a U-SQL database.
Test-AdlCatalogItem  -Account $adla -ItemType Database -Path "master"
```

### <a name="create-credentials-in-a-catalog"></a><span data-ttu-id="dd5fb-219">Vytvořit přihlašovací údaje v katalogu</span><span class="sxs-lookup"><span data-stu-id="dd5fb-219">Create credentials in a catalog</span></span>

<span data-ttu-id="dd5fb-220">V databázi U-SQL vytvořte objekt přihlašovacích údajů pro databázi hostované v Azure.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-220">Within a U-SQL database, create a credential object for a database hosted in Azure.</span></span> <span data-ttu-id="dd5fb-221">V současné době jsou přihlašovací údaje U-SQL hello jediným typem položek katalogu, které můžete vytvořit pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-221">Currently, U-SQL credentials are hello only type of catalog item that you can create through PowerShell.</span></span>

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

### <a name="get-basic-information-about-an-adla-account"></a><span data-ttu-id="dd5fb-222">Získejte základní informace o účtu ADLA</span><span class="sxs-lookup"><span data-stu-id="dd5fb-222">Get basic information about an ADLA account</span></span>

<span data-ttu-id="dd5fb-223">Zadaný název účtu vyhledává hello následující kód základní informace o účtu hello</span><span class="sxs-lookup"><span data-stu-id="dd5fb-223">Given an account name, hello following code looks up basic information about hello account</span></span>

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

## <a name="working-with-azure"></a><span data-ttu-id="dd5fb-224">Práce s Azure</span><span class="sxs-lookup"><span data-stu-id="dd5fb-224">Working with Azure</span></span>

### <a name="get-details-of-azurerm-errors"></a><span data-ttu-id="dd5fb-225">Získat podrobnosti o chybách AzureRm</span><span class="sxs-lookup"><span data-stu-id="dd5fb-225">Get details of AzureRm errors</span></span>

```powershell
Resolve-AzureRmError -Last
```

### <a name="verify-if-you-are-running-as-an-administrator"></a><span data-ttu-id="dd5fb-226">Ověřte, zda používáte jako správce</span><span class="sxs-lookup"><span data-stu-id="dd5fb-226">Verify if you are running as an administrator</span></span>

```powershell
function Test-Administrator  
{  
    $user = [Security.Principal.WindowsIdentity]::GetCurrent();
    $p = New-Object Security.Principal.WindowsPrincipal $user
    $p.IsInRole([Security.Principal.WindowsBuiltinRole]::Administrator)  
}
```

### <a name="find-a-tenantid"></a><span data-ttu-id="dd5fb-227">Najít TenantID</span><span class="sxs-lookup"><span data-stu-id="dd5fb-227">Find a TenantID</span></span>

<span data-ttu-id="dd5fb-228">Z názvu předplatného:</span><span class="sxs-lookup"><span data-stu-id="dd5fb-228">From a subscription name:</span></span>

```powershell
function Get-TenantIdFromSubcriptionName( [string] $subname )
{
    $sub = (Get-AzureRmSubscription -SubscriptionName $subname)
    $sub.TenantId
}

Get-TenantIdFromSubcriptionName "ADLTrainingMS"
```

<span data-ttu-id="dd5fb-229">Z id předplatného:</span><span class="sxs-lookup"><span data-stu-id="dd5fb-229">From a subscription id:</span></span>

```powershell
function Get-TenantIdFromSubcriptionId( [string] $subid )
{
    $sub = (Get-AzureRmSubscription -SubscriptionId $subid)
    $sub.TenantId
}

$subid = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
Get-TenantIdFromSubcriptionId $subid
```

<span data-ttu-id="dd5fb-230">Z adresy domény jako je například "contoso.com"</span><span class="sxs-lookup"><span data-stu-id="dd5fb-230">From a domain address such as "contoso.com"</span></span>


```powershell
function Get-TenantIdFromDomain( $domain )
{
    $url = "https://login.windows.net/" + $domain + "/.well-known/openid-configuration"
    return (Invoke-WebRequest $url|ConvertFrom-Json).token_endpoint.Split('/')[3]
}

$domain = "contoso.com"
Get-TenantIdFromDomain $domain
```

### <a name="list-all-your-subscriptions-and-tenant-ids"></a><span data-ttu-id="dd5fb-231">Zobrazí seznam všech odběrů a ID klienta</span><span class="sxs-lookup"><span data-stu-id="dd5fb-231">List all your subscriptions and tenant ids</span></span>

```powershell
$subs = Get-AzureRmSubscription
foreach ($sub in $subs)
{
    Write-Host $sub.Name "("  $sub.Id ")"
    Write-Host "`tTenant Id" $sub.TenantId
}
```

## <a name="create-a-data-lake-analytics-account-using-a-template"></a><span data-ttu-id="dd5fb-232">Vytvoření účtu Data Lake Analytics pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="dd5fb-232">Create a Data Lake Analytics account using a template</span></span>

<span data-ttu-id="dd5fb-233">Můžete také použít šablonu skupiny prostředků Azure pomocí hello následující skript prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="dd5fb-233">You can also use an Azure Resource Group template using hello following  PowerShell script:</span></span>

```powershell
$subId = "<Your Azure Subscription ID>"

$rg = "<New Azure Resource Group Name>"
$location = "<Location (such as East US 2)>"
$adls = "<New Data Lake Store Account Name>"
$adla = "<New Data Lake Analytics Account Name>"

$deploymentName = "MyDataLakeAnalyticsDeployment"
$armTemplateFile = "<LocalFolderPath>\azuredeploy.json"  # update hello JSON template path 

# Log in tooAzure
Login-AzureRmAccount -SubscriptionId $subId

# Create hello resource group
New-AzureRmResourceGroup -Name $rg -Location $location

# Create hello Data Lake Analytics account with hello default Data Lake Store account.
$parameters = @{"adlAnalyticsName"=$adla; "adlStoreName"=$adls}
New-AzureRmResourceGroupDeployment -Name $deploymentName -ResourceGroupName $rg -TemplateFile $armTemplateFile -TemplateParameterObject $parameters 
```

<span data-ttu-id="dd5fb-234">Další informace najdete v tématu [nasazení aplikace pomocí šablony Azure Resource Manageru](../azure-resource-manager/resource-group-template-deploy.md) a [šablon pro tvorbu Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="dd5fb-234">For more information, see [Deploy an application with Azure Resource Manager template](../azure-resource-manager/resource-group-template-deploy.md) and [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="dd5fb-235">**Příklad šablony**</span><span class="sxs-lookup"><span data-stu-id="dd5fb-235">**Example template**</span></span>

<span data-ttu-id="dd5fb-236">Uložte následující text jako hello `.json` souboru a pak použijte hello předcházející šablony hello toouse skript prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="dd5fb-236">Save hello following text as a `.json` file, and then use hello preceding PowerShell script toouse hello template.</span></span> 

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adlAnalyticsName": {
      "type": "string",
      "metadata": {
        "description": "hello name of hello Data Lake Analytics account toocreate."
      }
    },
    "adlStoreName": {
      "type": "string",
      "metadata": {
        "description": "hello name of hello Data Lake Store account toocreate."
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

## <a name="next-steps"></a><span data-ttu-id="dd5fb-237">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dd5fb-237">Next steps</span></span>
* [<span data-ttu-id="dd5fb-238">Přehled služby Microsoft Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="dd5fb-238">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* <span data-ttu-id="dd5fb-239">Začínáme s Data Lake Analytics pomocí [portál Azure](data-lake-analytics-get-started-portal.md) | [prostředí Azure PowerShell](data-lake-analytics-get-started-powershell.md) | [2.0 rozhraní příkazového řádku](data-lake-analytics-get-started-cli2.md)</span><span class="sxs-lookup"><span data-stu-id="dd5fb-239">Get started with Data Lake Analytics using [Azure portal](data-lake-analytics-get-started-portal.md) | [Azure PowerShell](data-lake-analytics-get-started-powershell.md) | [CLI 2.0](data-lake-analytics-get-started-cli2.md)</span></span>
* <span data-ttu-id="dd5fb-240">Správa Azure Data Lake Analytics pomocí [portál Azure](data-lake-analytics-manage-use-portal.md) | [prostředí Azure PowerShell](data-lake-analytics-manage-use-powershell.md) | [rozhraní příkazového řádku](data-lake-analytics-manage-use-cli.md)</span><span class="sxs-lookup"><span data-stu-id="dd5fb-240">Manage Azure Data Lake Analytics using [Azure portal](data-lake-analytics-manage-use-portal.md) | [Azure PowerShell](data-lake-analytics-manage-use-powershell.md) | [CLI](data-lake-analytics-manage-use-cli.md)</span></span> 
