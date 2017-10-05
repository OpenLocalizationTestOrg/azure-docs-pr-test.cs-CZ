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
ms.openlocfilehash: 52180b760046d273c5a75720df0a73532d0343ad
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="accessing-usage-reports-in-azure-ad-b2c-via-the-reporting-api"></a><span data-ttu-id="6dab8-103">Přístup k použití sestav v Azure AD B2C prostřednictvím rozhraní API pro generování sestav</span><span class="sxs-lookup"><span data-stu-id="6dab8-103">Accessing usage reports in Azure AD B2C via the reporting API</span></span>

<span data-ttu-id="6dab8-104">Azure Active Directory B2C (Azure AD B2C) zajišťuje ověřování na základě přihlášení uživatele a ověřování Azure Multi-Factor Authentication.</span><span class="sxs-lookup"><span data-stu-id="6dab8-104">Azure Active Directory B2C (Azure AD B2C) provides authentication based on user sign-in and Azure Multi-Factor Authentication.</span></span> <span data-ttu-id="6dab8-105">Ověřování se poskytuje koncovým uživatelům vaší aplikace rodiny napříč poskytovatelů identit.</span><span class="sxs-lookup"><span data-stu-id="6dab8-105">Authentication is provided for end users of your application family across identity providers.</span></span> <span data-ttu-id="6dab8-106">Když víte, počet uživatelů, registrované v klientovi, zprostředkovatele, který se používá k registraci a počet ověřování podle typu, je zodpovědět otázky jako:</span><span class="sxs-lookup"><span data-stu-id="6dab8-106">When you know the number of users registered in the tenant, the providers they used to register, and the number of authentications by type, you can answer questions like:</span></span>
* <span data-ttu-id="6dab8-107">Kolik uživatelů z každého typu zprostředkovatele identity (například účet Microsoft nebo LinkedIn) registrovali v posledních 10 dnů?</span><span class="sxs-lookup"><span data-stu-id="6dab8-107">How many users from each type of identity provider (for example, a Microsoft or LinkedIn account) have registered in the last 10 days?</span></span>
* <span data-ttu-id="6dab8-108">Kolik ověřování pomocí služby Multi-Factor Authentication byly úspěšně dokončeny za minulý měsíc?</span><span class="sxs-lookup"><span data-stu-id="6dab8-108">How many authentications using Multi-Factor Authentication have completed successfully in the last month?</span></span>
* <span data-ttu-id="6dab8-109">Kolik ověřování přihlášení v základě byly dokončeny tohoto měsíce?</span><span class="sxs-lookup"><span data-stu-id="6dab8-109">How many sign-in-based authentications were completed this month?</span></span> <span data-ttu-id="6dab8-110">Za den?</span><span class="sxs-lookup"><span data-stu-id="6dab8-110">Per day?</span></span> <span data-ttu-id="6dab8-111">Na aplikaci?</span><span class="sxs-lookup"><span data-stu-id="6dab8-111">Per application?</span></span>
* <span data-ttu-id="6dab8-112">Jak můžete odhadnout očekávané měsíční náklady na Moje aktivity klienta Azure AD B2C?</span><span class="sxs-lookup"><span data-stu-id="6dab8-112">How can I estimate the expected monthly cost of my Azure AD B2C tenant activity?</span></span>

<span data-ttu-id="6dab8-113">Tento článek se zaměřuje na sestavy, které jsou svázané s fakturační aktivity, která je založena na počtu uživatelů, fakturovatelný sign v based ověřování a ověřování službou Multi-Factor Authentication.</span><span class="sxs-lookup"><span data-stu-id="6dab8-113">This article focuses on reports tied to billing activity, which is based on the number of users, billable sign-in-based authentications, and multi-factor authentications.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="6dab8-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6dab8-114">Prerequisites</span></span>
<span data-ttu-id="6dab8-115">Než začnete, musíte dokončit kroky v [požadavky pro přístup k rozhraní API pro vytváření sestav Azure AD](https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/).</span><span class="sxs-lookup"><span data-stu-id="6dab8-115">Before you get started, you need to complete the steps in [Prerequisites to access the Azure AD reporting APIs](https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/).</span></span> <span data-ttu-id="6dab8-116">Vytvořit aplikaci, získat tajný klíč a jí udělit přístup práva k sestavy klienta Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="6dab8-116">Create an application, obtain a secret for it, and grant it access rights to your Azure AD B2C tenant’s reports.</span></span> <span data-ttu-id="6dab8-117">*Bash skriptu* a *skript v jazyce Python* příklady jsou také uvedeny zde.</span><span class="sxs-lookup"><span data-stu-id="6dab8-117">*Bash script* and *Python script* examples are also provided here.</span></span> 

## <a name="powershell-script"></a><span data-ttu-id="6dab8-118">Skript PowerShellu</span><span class="sxs-lookup"><span data-stu-id="6dab8-118">PowerShell script</span></span>
<span data-ttu-id="6dab8-119">Tento skript ukazuje vytvoření čtyři sestavy využití pomocí `TimeStamp` parametr a `ApplicationId` filtru.</span><span class="sxs-lookup"><span data-stu-id="6dab8-119">This script demonstrates the creation of four usage reports by using the `TimeStamp` parameter and the `ApplicationId` filter.</span></span>

```powershell
# This script will require the Web Application and permissions setup in Azure Active Directory

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

    Write-host Data from the tenantUserCount report
    Write-host ====================================================
     # Returns a JSON document for the report
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/tenantUserCount?api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the tenantUserCount report with datetime filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/tenantUserCount?%24filter=TimeStamp+gt+2016-10-15&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cAuthenticationCountSummary report
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cAuthenticationCountSummary?api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cAuthenticationCount report with datetime filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cAuthenticationCount?%24filter=TimeStamp+gt+2016-09-20+and+TimeStamp+lt+2016-10-03&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cAuthenticationCount report with ApplicationId filter
    Write-host ====================================================
    # Returns a JSON document for the " " report
        $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cAuthenticationCount?%24filter=ApplicationId+eq+ada78934-a6da-4e69-b816-10de0d79db1d&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cMfaRequestCountSummary
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cMfaRequestCountSummary?api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cMfaRequestCount report with datetime filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cMfaRequestCount?%24filter=TimeStamp+gt+2016-09-10+and+TimeStamp+lt+2016-10-04&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cMfaRequestCount report with ApplicationId filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cMfaRequestCountSummary?%24filter=ApplicationId+eq+ada78934-a6da-4e69-b816-10de0d79db1d&api-version=beta")
     Write-host $myReport.Content

} else {
    Write-Host "ERROR: No Access Token"
    }
```


## <a name="usage-report-definitions"></a><span data-ttu-id="6dab8-120">Definice sestavy využití</span><span class="sxs-lookup"><span data-stu-id="6dab8-120">Usage report definitions</span></span>
* <span data-ttu-id="6dab8-121">**tenantUserCount**: počet uživatelů v klientovi typem zprostředkovatele identity, za den za posledních 30 dní.</span><span class="sxs-lookup"><span data-stu-id="6dab8-121">**tenantUserCount**: The number of users in the tenant by type of identity provider, per day in the last 30 days.</span></span> <span data-ttu-id="6dab8-122">(Volitelně `TimeStamp` filtr poskytuje počty uživatelů ze zadaného data na aktuální datum).</span><span class="sxs-lookup"><span data-stu-id="6dab8-122">(Optionally, a `TimeStamp` filter provides user counts from a specified date to the current date).</span></span> <span data-ttu-id="6dab8-123">Sestava obsahuje:</span><span class="sxs-lookup"><span data-stu-id="6dab8-123">The report provides:</span></span>
  * <span data-ttu-id="6dab8-124">**TotalUserCount**: počet všechny uživatelské objekty.</span><span class="sxs-lookup"><span data-stu-id="6dab8-124">**TotalUserCount**: The number of all user objects.</span></span>
  * <span data-ttu-id="6dab8-125">**OtherUserCount**: počet uživatelů Azure Active Directory (nikoli uživatelům Azure AD B2C).</span><span class="sxs-lookup"><span data-stu-id="6dab8-125">**OtherUserCount**: The number of Azure Active Directory users (not Azure AD B2C users).</span></span>
  * <span data-ttu-id="6dab8-126">**LocalUserCount**: počet uživatelských účtů Azure AD B2C vytvořit s přihlašovacími údaji místního klienta Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="6dab8-126">**LocalUserCount**: The number of Azure AD B2C user accounts created with credentials local to the Azure AD B2C tenant.</span></span>

* <span data-ttu-id="6dab8-127">**AlternateIdUserCount**: počet uživatelů, Azure AD B2C zaregistrována zprostředkovatelů externí identity (například Facebook, účet Microsoft nebo jiného klienta Azure Active Directory, také označuje jako `OrgId`).</span><span class="sxs-lookup"><span data-stu-id="6dab8-127">**AlternateIdUserCount**: The number of Azure AD B2C users registered with external identity providers (for example, Facebook, a Microsoft account, or another Azure Active Directory tenant, also referred to as an `OrgId`).</span></span>

* <span data-ttu-id="6dab8-128">**b2cAuthenticationCountSummary**: Souhrn denní počet fakturovatelný ověření za posledních 30 dní, podle dne a typu toku ověřování.</span><span class="sxs-lookup"><span data-stu-id="6dab8-128">**b2cAuthenticationCountSummary**: Summary of the daily number of billable authentications over the last 30 days, by day and type of authentication flow.</span></span>

* <span data-ttu-id="6dab8-129">**b2cAuthenticationCount**: počet ověření v časovém období.</span><span class="sxs-lookup"><span data-stu-id="6dab8-129">**b2cAuthenticationCount**: The number of authentications within a time period.</span></span> <span data-ttu-id="6dab8-130">Výchozí hodnota je za posledních 30 dní.</span><span class="sxs-lookup"><span data-stu-id="6dab8-130">The default is the last 30 days.</span></span>  <span data-ttu-id="6dab8-131">(Volitelné: na začátek a konec `TimeStamp` parametry definovat za určité časové období.) Výstup obsahuje `StartTimeStamp` (nejdřívější datum aktivity pro tohoto klienta) a `EndTimeStamp` (nejnovější aktualizace).</span><span class="sxs-lookup"><span data-stu-id="6dab8-131">(Optional: The beginning and ending `TimeStamp` parameters define a specific time period.) The output includes `StartTimeStamp` (earliest date of activity for this tenant) and `EndTimeStamp` (latest update).</span></span>

* <span data-ttu-id="6dab8-132">**b2cMfaRequestCountSummary**: Souhrn denní počet ověřování službou Multi-Factor Authentication podle dne a typu (SMS nebo hlasové).</span><span class="sxs-lookup"><span data-stu-id="6dab8-132">**b2cMfaRequestCountSummary**: Summary of the daily number of multi-factor authentications, by day and type (SMS or voice).</span></span>


## <a name="limitations"></a><span data-ttu-id="6dab8-133">Omezení</span><span class="sxs-lookup"><span data-stu-id="6dab8-133">Limitations</span></span>
<span data-ttu-id="6dab8-134">Uživatel počet data se aktualizují každých 24 až 48 hodin.</span><span class="sxs-lookup"><span data-stu-id="6dab8-134">User count data is refreshed every 24 to 48 hours.</span></span> <span data-ttu-id="6dab8-135">Ověření se aktualizují několikrát za den.</span><span class="sxs-lookup"><span data-stu-id="6dab8-135">Authentications are updated several times a day.</span></span> <span data-ttu-id="6dab8-136">Při použití `ApplicationId` odpověď prázdná sestavy filtr, může být kvůli jednomu z následujících podmínek:</span><span class="sxs-lookup"><span data-stu-id="6dab8-136">When using the `ApplicationId` filter, an empty report response can be due to one of following conditions:</span></span>
  * <span data-ttu-id="6dab8-137">ID aplikace v klientovi neexistuje.</span><span class="sxs-lookup"><span data-stu-id="6dab8-137">The application ID does not exist in the tenant.</span></span> <span data-ttu-id="6dab8-138">Zkontrolujte, zda že je správný.</span><span class="sxs-lookup"><span data-stu-id="6dab8-138">Make sure it is correct.</span></span>
  * <span data-ttu-id="6dab8-139">ID aplikace existuje, ale v období vytváření sestav nebyla nalezena žádná data.</span><span class="sxs-lookup"><span data-stu-id="6dab8-139">The application ID exists, but no data was found in the reporting period.</span></span> <span data-ttu-id="6dab8-140">Zkontrolujte parametry data a času.</span><span class="sxs-lookup"><span data-stu-id="6dab8-140">Review your date/time parameters.</span></span>


## <a name="next-steps"></a><span data-ttu-id="6dab8-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6dab8-141">Next steps</span></span>
### <a name="monthly-bill-estimates-for-azure-ad"></a><span data-ttu-id="6dab8-142">Odhadne měsíčních nákladů pro Azure AD</span><span class="sxs-lookup"><span data-stu-id="6dab8-142">Monthly bill estimates for Azure AD</span></span>
<span data-ttu-id="6dab8-143">V kombinaci s [nejaktuálnější Azure AD B2C cenách dostupné](https://azure.microsoft.com/pricing/details/active-directory-b2c/), můžete odhadnout denní, týdenní a měsíční využití platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="6dab8-143">When combined with [the most current Azure AD B2C pricing available](https://azure.microsoft.com/pricing/details/active-directory-b2c/), you can estimate daily, weekly, and monthly Azure consumption.</span></span>  <span data-ttu-id="6dab8-144">Odhad je obzvláště užitečná při plánování změny v chování klienta, který může mít vliv na celkové náklady.</span><span class="sxs-lookup"><span data-stu-id="6dab8-144">An estimate is especially useful when you plan for changes in tenant behavior that might impact overall cost.</span></span> <span data-ttu-id="6dab8-145">Můžete zkontrolovat skutečné náklady v vaše [přidružené předplatné Azure](active-directory-b2c-how-to-enable-billing.md).</span><span class="sxs-lookup"><span data-stu-id="6dab8-145">You can review actual costs in your [linked Azure subscription](active-directory-b2c-how-to-enable-billing.md).</span></span>

### <a name="options-for-other-output-formats"></a><span data-ttu-id="6dab8-146">Možnosti pro ostatní formáty výstupu</span><span class="sxs-lookup"><span data-stu-id="6dab8-146">Options for other output formats</span></span>
<span data-ttu-id="6dab8-147">Následující kód ukazuje příklady odeslání výstupu do formátu JSON, seznam hodnot název a XML:</span><span class="sxs-lookup"><span data-stu-id="6dab8-147">The following code shows examples of sending output to JSON, a name value list, and XML:</span></span>
```powershell
# to output to JSON use following line in the PowerShell sample
$myReport.Content | Out-File -FilePath b2cUserJourneySummaryEvents.json -Force

# to output the content to a name value list
($myReport.Content | ConvertFrom-Json).value | Out-File -FilePath name-your-file.txt -Force

# to output the content in XML use the following line
(($myReport.Content | ConvertFrom-Json).value | ConvertTo-Xml).InnerXml | Out-File -FilePath name-your-file.xml -Force
```
