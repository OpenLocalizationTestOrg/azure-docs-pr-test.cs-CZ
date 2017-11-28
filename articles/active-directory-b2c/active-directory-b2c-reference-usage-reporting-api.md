---
title: "Azure Active Directory B2C: Využití sestav ukázky rozhraní API a definice | Microsoft Docs"
description: "Průvodce a ukázky na získávání sestavy na klienta Azure AD B2C, uživatelé, ověřování a ověřování službou Multi-Factor Authentication"
services: active-directory-b2c
documentationcenter: dev-center-name
author: rojasja
manager: mbaldwin
ms.service: active-directory-b2c
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/04/2017
ms.author: joroja
ms.openlocfilehash: f01f0d1d12464e38f892f1fdd5c7408a3b4cd1aa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-usage-reports-in-azure-ad-b2c-via-hello-reporting-api"></a><span data-ttu-id="83273-103">Přístup k použití sestav v Azure AD B2C prostřednictvím hello reporting rozhraní API</span><span class="sxs-lookup"><span data-stu-id="83273-103">Accessing usage reports in Azure AD B2C via hello reporting API</span></span>

<span data-ttu-id="83273-104">Azure Active Directory B2C (Azure AD B2C) zajišťuje ověřování na základě přihlášení uživatele a ověřování Azure Multi-Factor Authentication.</span><span class="sxs-lookup"><span data-stu-id="83273-104">Azure Active Directory B2C (Azure AD B2C) provides authentication based on user sign-in and Azure Multi-Factor Authentication.</span></span> <span data-ttu-id="83273-105">Ověřování se poskytuje koncovým uživatelům vaší aplikace rodiny napříč poskytovatelů identit.</span><span class="sxs-lookup"><span data-stu-id="83273-105">Authentication is provided for end users of your application family across identity providers.</span></span> <span data-ttu-id="83273-106">Když víte, hello počet uživatelů, které jsou zaregistrované v klientovi hello, zprostředkovatelé hello používají tooregister a hello počet ověřování podle typu, je zodpovědět otázky jako:</span><span class="sxs-lookup"><span data-stu-id="83273-106">When you know hello number of users registered in hello tenant, hello providers they used tooregister, and hello number of authentications by type, you can answer questions like:</span></span>
* <span data-ttu-id="83273-107">Kolik uživatelů z každého typu zprostředkovatele identity (například účet Microsoft nebo LinkedIn) registrovali v hello posledních 10 dnů?</span><span class="sxs-lookup"><span data-stu-id="83273-107">How many users from each type of identity provider (for example, a Microsoft or LinkedIn account) have registered in hello last 10 days?</span></span>
* <span data-ttu-id="83273-108">Kolik ověřování pomocí služby Multi-Factor Authentication byly úspěšně dokončeny v hello minulý měsíc?</span><span class="sxs-lookup"><span data-stu-id="83273-108">How many authentications using Multi-Factor Authentication have completed successfully in hello last month?</span></span>
* <span data-ttu-id="83273-109">Kolik ověřování přihlášení v základě byly dokončeny tohoto měsíce?</span><span class="sxs-lookup"><span data-stu-id="83273-109">How many sign-in-based authentications were completed this month?</span></span> <span data-ttu-id="83273-110">Za den?</span><span class="sxs-lookup"><span data-stu-id="83273-110">Per day?</span></span> <span data-ttu-id="83273-111">Na aplikaci?</span><span class="sxs-lookup"><span data-stu-id="83273-111">Per application?</span></span>
* <span data-ttu-id="83273-112">Jak můžete odhadnout, že hello očekává měsíční náklady na Moje aktivity klienta Azure AD B2C?</span><span class="sxs-lookup"><span data-stu-id="83273-112">How can I estimate hello expected monthly cost of my Azure AD B2C tenant activity?</span></span>

<span data-ttu-id="83273-113">Tento článek se zaměřuje na sestavy svázané toobilling aktivity, která je založena na hello počet uživatelů, fakturovatelný sign v based ověřování a ověřování službou Multi-Factor Authentication.</span><span class="sxs-lookup"><span data-stu-id="83273-113">This article focuses on reports tied toobilling activity, which is based on hello number of users, billable sign-in-based authentications, and multi-factor authentications.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="83273-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="83273-114">Prerequisites</span></span>
<span data-ttu-id="83273-115">Než začnete, musíte toocomplete hello kroky v [tooaccess požadavky hello vytvářením sestav Azure AD rozhraní API](https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/).</span><span class="sxs-lookup"><span data-stu-id="83273-115">Before you get started, you need toocomplete hello steps in [Prerequisites tooaccess hello Azure AD reporting APIs](https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/).</span></span> <span data-ttu-id="83273-116">Vytvořit aplikaci, získat tajný klíč a udělte ho přístup rights tooyour Azure AD B2C klienta sestav.</span><span class="sxs-lookup"><span data-stu-id="83273-116">Create an application, obtain a secret for it, and grant it access rights tooyour Azure AD B2C tenant’s reports.</span></span> <span data-ttu-id="83273-117">*Bash skriptu* a *skript v jazyce Python* příklady jsou také uvedeny zde.</span><span class="sxs-lookup"><span data-stu-id="83273-117">*Bash script* and *Python script* examples are also provided here.</span></span> 

## <a name="powershell-script"></a><span data-ttu-id="83273-118">Skript PowerShellu</span><span class="sxs-lookup"><span data-stu-id="83273-118">PowerShell script</span></span>
<span data-ttu-id="83273-119">Tento skript ukazuje vytvoření hello čtyři sestavy využití pomocí hello `TimeStamp` parametr a hello `ApplicationId` filtru.</span><span class="sxs-lookup"><span data-stu-id="83273-119">This script demonstrates hello creation of four usage reports by using hello `TimeStamp` parameter and hello `ApplicationId` filter.</span></span>

```powershell
# This script will require hello Web Application and permissions setup in Azure Active Directory

# Constants
$ClientID      = "your-client-application-id-here"  
$ClientSecret  = "your-client-application-secret-here"
$loginURL      = "https://login.microsoftonline.com"
$tenantdomain  = "your-b2c-tenant-domain.onmicrosoft.com"  
# Get an Oauth 2 access token based on client id, secret and tenant domain
$body          = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
$oauth         = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body
if ($oauth.access_token -ne $null) {
    $headerParams  = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

    Write-host Data from hello tenantUserCount report
    Write-host ====================================================
     # Returns a JSON document for hello report
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/tenantUserCount?api-version=beta")
    Write-host $myReport.Content

    Write-host Data from hello tenantUserCount report with datetime filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/tenantUserCount?%24filter=TimeStamp+gt+2016-10-15&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from hello b2cAuthenticationCountSummary report
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cAuthenticationCountSummary?api-version=beta")
    Write-host $myReport.Content

    Write-host Data from hello b2cAuthenticationCount report with datetime filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cAuthenticationCount?%24filter=TimeStamp+gt+2016-09-20+and+TimeStamp+lt+2016-10-03&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from hello b2cAuthenticationCount report with ApplicationId filter
    Write-host ====================================================
    # Returns a JSON document for hello " " report
        $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cAuthenticationCount?%24filter=ApplicationId+eq+ada78934-a6da-4e69-b816-10de0d79db1d&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from hello b2cMfaRequestCountSummary
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cMfaRequestCountSummary?api-version=beta")
    Write-host $myReport.Content

    Write-host Data from hello b2cMfaRequestCount report with datetime filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cMfaRequestCount?%24filter=TimeStamp+gt+2016-09-10+and+TimeStamp+lt+2016-10-04&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from hello b2cMfaRequestCount report with ApplicationId filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cMfaRequestCountSummary?%24filter=ApplicationId+eq+ada78934-a6da-4e69-b816-10de0d79db1d&api-version=beta")
     Write-host $myReport.Content

} else {
    Write-Host "ERROR: No Access Token"
    }
```


## <a name="usage-report-definitions"></a><span data-ttu-id="83273-120">Definice sestavy využití</span><span class="sxs-lookup"><span data-stu-id="83273-120">Usage report definitions</span></span>
* <span data-ttu-id="83273-121">**tenantUserCount**: hello počet uživatelů v klientovi hello podle typu zprostředkovatele identity, za den v hello posledních 30 dnů.</span><span class="sxs-lookup"><span data-stu-id="83273-121">**tenantUserCount**: hello number of users in hello tenant by type of identity provider, per day in hello last 30 days.</span></span> <span data-ttu-id="83273-122">(Volitelně `TimeStamp` filtr poskytuje počty uživatelů ze zadané datum toohello aktuální datum).</span><span class="sxs-lookup"><span data-stu-id="83273-122">(Optionally, a `TimeStamp` filter provides user counts from a specified date toohello current date).</span></span> <span data-ttu-id="83273-123">Sestava Hello obsahuje:</span><span class="sxs-lookup"><span data-stu-id="83273-123">hello report provides:</span></span>
  * <span data-ttu-id="83273-124">**TotalUserCount**: hello počet všechny uživatelské objekty.</span><span class="sxs-lookup"><span data-stu-id="83273-124">**TotalUserCount**: hello number of all user objects.</span></span>
  * <span data-ttu-id="83273-125">**OtherUserCount**: hello počet uživatelů Azure Active Directory (nikoli uživatelům Azure AD B2C).</span><span class="sxs-lookup"><span data-stu-id="83273-125">**OtherUserCount**: hello number of Azure Active Directory users (not Azure AD B2C users).</span></span>
  * <span data-ttu-id="83273-126">**LocalUserCount**: hello počet uživatelské účty Azure AD B2C vytvořené pomocí přihlašovacích údajů místního toohello Azure AD B2C klienta.</span><span class="sxs-lookup"><span data-stu-id="83273-126">**LocalUserCount**: hello number of Azure AD B2C user accounts created with credentials local toohello Azure AD B2C tenant.</span></span>

* <span data-ttu-id="83273-127">**AlternateIdUserCount**: hello počet uživatelů Azure AD B2C registrovaných zprostředkovatelům externí identity (například Facebook, účet Microsoft nebo jiného klienta Azure Active Directory, nazývaná také jen tooas `OrgId`).</span><span class="sxs-lookup"><span data-stu-id="83273-127">**AlternateIdUserCount**: hello number of Azure AD B2C users registered with external identity providers (for example, Facebook, a Microsoft account, or another Azure Active Directory tenant, also referred tooas an `OrgId`).</span></span>

* <span data-ttu-id="83273-128">**b2cAuthenticationCountSummary**: Souhrn hello denní počet fakturovatelný ověření přes hello posledních 30 dní, podle dne a typu toku ověřování.</span><span class="sxs-lookup"><span data-stu-id="83273-128">**b2cAuthenticationCountSummary**: Summary of hello daily number of billable authentications over hello last 30 days, by day and type of authentication flow.</span></span>

* <span data-ttu-id="83273-129">**b2cAuthenticationCount**: hello počet ověření v časovém období.</span><span class="sxs-lookup"><span data-stu-id="83273-129">**b2cAuthenticationCount**: hello number of authentications within a time period.</span></span> <span data-ttu-id="83273-130">Výchozí hodnota Hello je hello posledních 30 dnů.</span><span class="sxs-lookup"><span data-stu-id="83273-130">hello default is hello last 30 days.</span></span>  <span data-ttu-id="83273-131">(Volitelné: hello počáteční a koncové `TimeStamp` definovat parametry na určité časové období.) zahrnuje výstup hello `StartTimeStamp` (nejdřívější datum aktivity pro tohoto klienta) a `EndTimeStamp` (nejnovější aktualizace).</span><span class="sxs-lookup"><span data-stu-id="83273-131">(Optional: hello beginning and ending `TimeStamp` parameters define a specific time period.) hello output includes `StartTimeStamp` (earliest date of activity for this tenant) and `EndTimeStamp` (latest update).</span></span>

* <span data-ttu-id="83273-132">**b2cMfaRequestCountSummary**: Souhrn hello denní počet ověřování službou Multi-Factor Authentication podle dne a typu (SMS nebo hlasové).</span><span class="sxs-lookup"><span data-stu-id="83273-132">**b2cMfaRequestCountSummary**: Summary of hello daily number of multi-factor authentications, by day and type (SMS or voice).</span></span>


## <a name="limitations"></a><span data-ttu-id="83273-133">Omezení</span><span class="sxs-lookup"><span data-stu-id="83273-133">Limitations</span></span>
<span data-ttu-id="83273-134">Uživatel počet data se aktualizují každých 24 hodin too48.</span><span class="sxs-lookup"><span data-stu-id="83273-134">User count data is refreshed every 24 too48 hours.</span></span> <span data-ttu-id="83273-135">Ověření se aktualizují několikrát za den.</span><span class="sxs-lookup"><span data-stu-id="83273-135">Authentications are updated several times a day.</span></span> <span data-ttu-id="83273-136">Při použití hello `ApplicationId` filtr, může být odpověď prázdné sestavy kvůli tooone z následujících podmínek:</span><span class="sxs-lookup"><span data-stu-id="83273-136">When using hello `ApplicationId` filter, an empty report response can be due tooone of following conditions:</span></span>
  * <span data-ttu-id="83273-137">ID aplikace Hello neexistuje v klientovi hello.</span><span class="sxs-lookup"><span data-stu-id="83273-137">hello application ID does not exist in hello tenant.</span></span> <span data-ttu-id="83273-138">Zkontrolujte, zda že je správný.</span><span class="sxs-lookup"><span data-stu-id="83273-138">Make sure it is correct.</span></span>
  * <span data-ttu-id="83273-139">ID aplikace Hello existuje, ale v hello období vytváření sestav nebyla nalezena žádná data.</span><span class="sxs-lookup"><span data-stu-id="83273-139">hello application ID exists, but no data was found in hello reporting period.</span></span> <span data-ttu-id="83273-140">Zkontrolujte parametry data a času.</span><span class="sxs-lookup"><span data-stu-id="83273-140">Review your date/time parameters.</span></span>


## <a name="next-steps"></a><span data-ttu-id="83273-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="83273-141">Next steps</span></span>
### <a name="monthly-bill-estimates-for-azure-ad"></a><span data-ttu-id="83273-142">Odhadne měsíčních nákladů pro Azure AD</span><span class="sxs-lookup"><span data-stu-id="83273-142">Monthly bill estimates for Azure AD</span></span>
<span data-ttu-id="83273-143">V kombinaci s [hello nejaktuálnější Azure AD B2C ceny k dispozici](https://azure.microsoft.com/pricing/details/active-directory-b2c/), můžete odhadnout denní, týdenní a měsíční využití platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="83273-143">When combined with [hello most current Azure AD B2C pricing available](https://azure.microsoft.com/pricing/details/active-directory-b2c/), you can estimate daily, weekly, and monthly Azure consumption.</span></span>  <span data-ttu-id="83273-144">Odhad je obzvláště užitečná při plánování změny v chování klienta, který může mít vliv na celkové náklady.</span><span class="sxs-lookup"><span data-stu-id="83273-144">An estimate is especially useful when you plan for changes in tenant behavior that might impact overall cost.</span></span> <span data-ttu-id="83273-145">Můžete zkontrolovat skutečné náklady v vaše [přidružené předplatné Azure](active-directory-b2c-how-to-enable-billing.md).</span><span class="sxs-lookup"><span data-stu-id="83273-145">You can review actual costs in your [linked Azure subscription](active-directory-b2c-how-to-enable-billing.md).</span></span>

### <a name="options-for-other-output-formats"></a><span data-ttu-id="83273-146">Možnosti pro ostatní formáty výstupu</span><span class="sxs-lookup"><span data-stu-id="83273-146">Options for other output formats</span></span>
<span data-ttu-id="83273-147">Hello následující kód ukazuje příklady odesílání výstupu tooJSON, seznam hodnot název a XML:</span><span class="sxs-lookup"><span data-stu-id="83273-147">hello following code shows examples of sending output tooJSON, a name value list, and XML:</span></span>
```powershell
# toooutput tooJSON use following line in hello PowerShell sample
$myReport.Content | Out-File -FilePath b2cUserJourneySummaryEvents.json -Force

# toooutput hello content tooa name value list
($myReport.Content | ConvertFrom-Json).value | Out-File -FilePath name-your-file.txt -Force

# toooutput hello content in XML use hello following line
(($myReport.Content | ConvertFrom-Json).value | ConvertTo-Xml).InnerXml | Out-File -FilePath name-your-file.xml -Force
```
