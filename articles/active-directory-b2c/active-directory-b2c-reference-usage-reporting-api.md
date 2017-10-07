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
# <a name="accessing-usage-reports-in-azure-ad-b2c-via-hello-reporting-api"></a>Přístup k použití sestav v Azure AD B2C prostřednictvím hello reporting rozhraní API

Azure Active Directory B2C (Azure AD B2C) zajišťuje ověřování na základě přihlášení uživatele a ověřování Azure Multi-Factor Authentication. Ověřování se poskytuje koncovým uživatelům vaší aplikace rodiny napříč poskytovatelů identit. Když víte, hello počet uživatelů, které jsou zaregistrované v klientovi hello, zprostředkovatelé hello používají tooregister a hello počet ověřování podle typu, je zodpovědět otázky jako:
* Kolik uživatelů z každého typu zprostředkovatele identity (například účet Microsoft nebo LinkedIn) registrovali v hello posledních 10 dnů?
* Kolik ověřování pomocí služby Multi-Factor Authentication byly úspěšně dokončeny v hello minulý měsíc?
* Kolik ověřování přihlášení v základě byly dokončeny tohoto měsíce? Za den? Na aplikaci?
* Jak můžete odhadnout, že hello očekává měsíční náklady na Moje aktivity klienta Azure AD B2C?

Tento článek se zaměřuje na sestavy svázané toobilling aktivity, která je založena na hello počet uživatelů, fakturovatelný sign v based ověřování a ověřování službou Multi-Factor Authentication.


## <a name="prerequisites"></a>Požadavky
Než začnete, musíte toocomplete hello kroky v [tooaccess požadavky hello vytvářením sestav Azure AD rozhraní API](https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/). Vytvořit aplikaci, získat tajný klíč a udělte ho přístup rights tooyour Azure AD B2C klienta sestav. *Bash skriptu* a *skript v jazyce Python* příklady jsou také uvedeny zde. 

## <a name="powershell-script"></a>Skript PowerShellu
Tento skript ukazuje vytvoření hello čtyři sestavy využití pomocí hello `TimeStamp` parametr a hello `ApplicationId` filtru.

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


## <a name="usage-report-definitions"></a>Definice sestavy využití
* **tenantUserCount**: hello počet uživatelů v klientovi hello podle typu zprostředkovatele identity, za den v hello posledních 30 dnů. (Volitelně `TimeStamp` filtr poskytuje počty uživatelů ze zadané datum toohello aktuální datum). Sestava Hello obsahuje:
  * **TotalUserCount**: hello počet všechny uživatelské objekty.
  * **OtherUserCount**: hello počet uživatelů Azure Active Directory (nikoli uživatelům Azure AD B2C).
  * **LocalUserCount**: hello počet uživatelské účty Azure AD B2C vytvořené pomocí přihlašovacích údajů místního toohello Azure AD B2C klienta.

* **AlternateIdUserCount**: hello počet uživatelů Azure AD B2C registrovaných zprostředkovatelům externí identity (například Facebook, účet Microsoft nebo jiného klienta Azure Active Directory, nazývaná také jen tooas `OrgId`).

* **b2cAuthenticationCountSummary**: Souhrn hello denní počet fakturovatelný ověření přes hello posledních 30 dní, podle dne a typu toku ověřování.

* **b2cAuthenticationCount**: hello počet ověření v časovém období. Výchozí hodnota Hello je hello posledních 30 dnů.  (Volitelné: hello počáteční a koncové `TimeStamp` definovat parametry na určité časové období.) zahrnuje výstup hello `StartTimeStamp` (nejdřívější datum aktivity pro tohoto klienta) a `EndTimeStamp` (nejnovější aktualizace).

* **b2cMfaRequestCountSummary**: Souhrn hello denní počet ověřování službou Multi-Factor Authentication podle dne a typu (SMS nebo hlasové).


## <a name="limitations"></a>Omezení
Uživatel počet data se aktualizují každých 24 hodin too48. Ověření se aktualizují několikrát za den. Při použití hello `ApplicationId` filtr, může být odpověď prázdné sestavy kvůli tooone z následujících podmínek:
  * ID aplikace Hello neexistuje v klientovi hello. Zkontrolujte, zda že je správný.
  * ID aplikace Hello existuje, ale v hello období vytváření sestav nebyla nalezena žádná data. Zkontrolujte parametry data a času.


## <a name="next-steps"></a>Další kroky
### <a name="monthly-bill-estimates-for-azure-ad"></a>Odhadne měsíčních nákladů pro Azure AD
V kombinaci s [hello nejaktuálnější Azure AD B2C ceny k dispozici](https://azure.microsoft.com/pricing/details/active-directory-b2c/), můžete odhadnout denní, týdenní a měsíční využití platformy Azure.  Odhad je obzvláště užitečná při plánování změny v chování klienta, který může mít vliv na celkové náklady. Můžete zkontrolovat skutečné náklady v vaše [přidružené předplatné Azure](active-directory-b2c-how-to-enable-billing.md).

### <a name="options-for-other-output-formats"></a>Možnosti pro ostatní formáty výstupu
Hello následující kód ukazuje příklady odesílání výstupu tooJSON, seznam hodnot název a XML:
```powershell
# toooutput tooJSON use following line in hello PowerShell sample
$myReport.Content | Out-File -FilePath b2cUserJourneySummaryEvents.json -Force

# toooutput hello content tooa name value list
($myReport.Content | ConvertFrom-Json).value | Out-File -FilePath name-your-file.txt -Force

# toooutput hello content in XML use hello following line
(($myReport.Content | ConvertFrom-Json).value | ConvertTo-Xml).InnerXml | Out-File -FilePath name-your-file.xml -Force
```
