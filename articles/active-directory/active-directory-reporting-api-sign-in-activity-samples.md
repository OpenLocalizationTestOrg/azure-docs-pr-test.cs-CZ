---
title: "ukázky sestavy API aktivity aaaAzure přihlášení služby Active Directory | Microsoft Docs"
description: Jak tooget pracovat s Azure Active Directory Reporting API hello
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: c41c1489-726b-4d3f-81d6-83beb932df9c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: d4fbbea95fe0b52828673b997681ae37481e21bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-sign-in-activity-report-api-samples"></a>Ukázek Azure sestavy API přihlašovací aktivita služby Active Directory
Toto téma je součástí kolekce témat o hello Azure Active Directory reporting rozhraní API.  
Generování sestav služby Azure AD poskytuje rozhraní API, které vám umožní tooaccess přihlašovací aktivita dat pomocí kódu nebo související nástroje.  
Hello obor tohoto tématu je k ukázkový kód hello tooprovide **aktivity API přihlášení**.

Přejděte na téma:

* [Protokoly auditu](active-directory-reporting-azure-portal.md#activity-reports) další koncepční informace
* [Začínáme s Azure Active Directory Reporting API hello](active-directory-reporting-api-getting-started.md) Další informace o hello reporting rozhraní API.


## <a name="prerequisites"></a>Požadavky
Než použijete hello ukázky v tomto tématu, musíte toocomplete hello [požadavky tooaccess hello Azure AD reporting API](active-directory-reporting-api-prerequisites.md).  

## <a name="powershell-script"></a>Skript PowerShellu
    # This script will require hello Web Application and permissions setup in Azure Active Directory
    $ClientID       = "<clientId>"             # Should be a ~35 character string insert your info here
    $ClientSecret   = "<clientSecret>"         # Should be a ~44 character string insert your info here
    $loginURL       = "https://login.microsoftonline.com/"
    $tenantdomain   = "<tenantDomain>"
    $ daterange            # For example, contoso.onmicrosoft.com

    $7daysago = "{0:s}" -f (get-date).AddDays(-7) + "Z"
    # or, AddMinutes(-5)

    Write-Output $7daysago

    # Get an Oauth 2 access token based on client id, secret and tenant domain
    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}

    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    if ($oauth.access_token -ne $null) {
    $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

    $url = "https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&`$filter=signinDateTime ge $7daysago"

    $i=0

    Do{
        Write-Output "Fetching data using Uri: $url"
        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
        Write-Output "Save hello output tooa file SigninActivities$i.json"
        Write-Output "---------------------------------------------"
        $myReport.Content | Out-File -FilePath SigninActivities$i.json -Force
        $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
        $i = $i+1
    } while($url -ne $null)

    } else {

        Write-Host "ERROR: No Access Token"
    }




## <a name="executing-hello-script"></a>Provádění skriptu hello
Jednou dokončíte úpravy hello skriptu, spouštět a ověřte, zda že tento hello očekává, že se vrátí data z hello sestavy protokolů auditu.

Hello skript vrátí výstupní hello přihlášení sestavy ve formátu JSON. Vytvoří také `SigninActivities.json` soubor s hello stejný výstup. Můžete experimentovat změnou hello skriptu tooreturn data z jiných sestavy a komentář hello výstupní formáty, které nepotřebujete.

## <a name="next-steps"></a>Další kroky
* Chcete, aby toocustomize hello ukázky v tomto tématu? Podívejte se na hello [referenční dokumentace rozhraní API služby Azure Active Directory přihlašovací aktivita](active-directory-reporting-api-sign-in-activity-reference.md). 
* Pokud chcete, aby toosee úplný přehled pomocí hello Azure Active Directory, vytváření sestav rozhraní API najdete v tématu [Začínáme s Azure Active Directory, vytváření sestav API hello](active-directory-reporting-api-getting-started.md).
* Pokud chcete toofind Další informace o vytváření sestav Azure Active Directory, přečtěte si téma hello [Azure Active Directory průvodce vytvářením sestav](active-directory-reporting-guide.md).  

