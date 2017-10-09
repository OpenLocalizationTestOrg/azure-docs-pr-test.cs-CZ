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
# <a name="azure-active-directory-sign-in-activity-report-api-samples"></a><span data-ttu-id="57664-103">Ukázek Azure sestavy API přihlašovací aktivita služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="57664-103">Azure Active Directory sign-in activity report API samples</span></span>
<span data-ttu-id="57664-104">Toto téma je součástí kolekce témat o hello Azure Active Directory reporting rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="57664-104">This topic is part of a collection of topics about hello Azure Active Directory reporting API.</span></span>  
<span data-ttu-id="57664-105">Generování sestav služby Azure AD poskytuje rozhraní API, které vám umožní tooaccess přihlašovací aktivita dat pomocí kódu nebo související nástroje.</span><span class="sxs-lookup"><span data-stu-id="57664-105">Azure AD reporting provides you with an API that enables you tooaccess sign-in activity data using code or related tools.</span></span>  
<span data-ttu-id="57664-106">Hello obor tohoto tématu je k ukázkový kód hello tooprovide **aktivity API přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="57664-106">hello scope of this topic is tooprovide you with sample code for hello **sign-in activity API**.</span></span>

<span data-ttu-id="57664-107">Přejděte na téma:</span><span class="sxs-lookup"><span data-stu-id="57664-107">See:</span></span>

* <span data-ttu-id="57664-108">[Protokoly auditu](active-directory-reporting-azure-portal.md#activity-reports) další koncepční informace</span><span class="sxs-lookup"><span data-stu-id="57664-108">[Audit logs](active-directory-reporting-azure-portal.md#activity-reports)  for more conceptual information</span></span>
* <span data-ttu-id="57664-109">[Začínáme s Azure Active Directory Reporting API hello](active-directory-reporting-api-getting-started.md) Další informace o hello reporting rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="57664-109">[Getting started with hello Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md) for more information about hello reporting API.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="57664-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="57664-110">Prerequisites</span></span>
<span data-ttu-id="57664-111">Než použijete hello ukázky v tomto tématu, musíte toocomplete hello [požadavky tooaccess hello Azure AD reporting API](active-directory-reporting-api-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="57664-111">Before you can use hello samples in this topic, you need toocomplete hello [prerequisites tooaccess hello Azure AD reporting API](active-directory-reporting-api-prerequisites.md).</span></span>  

## <a name="powershell-script"></a><span data-ttu-id="57664-112">Skript PowerShellu</span><span class="sxs-lookup"><span data-stu-id="57664-112">PowerShell script</span></span>
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




## <a name="executing-hello-script"></a><span data-ttu-id="57664-113">Provádění skriptu hello</span><span class="sxs-lookup"><span data-stu-id="57664-113">Executing hello script</span></span>
<span data-ttu-id="57664-114">Jednou dokončíte úpravy hello skriptu, spouštět a ověřte, zda že tento hello očekává, že se vrátí data z hello sestavy protokolů auditu.</span><span class="sxs-lookup"><span data-stu-id="57664-114">Once you finish editing hello script, run it and verify that hello expected data from hello Audit logs report is returned.</span></span>

<span data-ttu-id="57664-115">Hello skript vrátí výstupní hello přihlášení sestavy ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="57664-115">hello script returns output from hello sign-in report in JSON format.</span></span> <span data-ttu-id="57664-116">Vytvoří také `SigninActivities.json` soubor s hello stejný výstup.</span><span class="sxs-lookup"><span data-stu-id="57664-116">It also creates an `SigninActivities.json` file with hello same output.</span></span> <span data-ttu-id="57664-117">Můžete experimentovat změnou hello skriptu tooreturn data z jiných sestavy a komentář hello výstupní formáty, které nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="57664-117">You can experiment by modifying hello script tooreturn data from other reports, and comment out hello output formats that you do not need.</span></span>

## <a name="next-steps"></a><span data-ttu-id="57664-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="57664-118">Next Steps</span></span>
* <span data-ttu-id="57664-119">Chcete, aby toocustomize hello ukázky v tomto tématu?</span><span class="sxs-lookup"><span data-stu-id="57664-119">Would you like toocustomize hello samples in this topic?</span></span> <span data-ttu-id="57664-120">Podívejte se na hello [referenční dokumentace rozhraní API služby Azure Active Directory přihlašovací aktivita](active-directory-reporting-api-sign-in-activity-reference.md).</span><span class="sxs-lookup"><span data-stu-id="57664-120">Check out hello [Azure Active Directory sign-in activity API reference](active-directory-reporting-api-sign-in-activity-reference.md).</span></span> 
* <span data-ttu-id="57664-121">Pokud chcete, aby toosee úplný přehled pomocí hello Azure Active Directory, vytváření sestav rozhraní API najdete v tématu [Začínáme s Azure Active Directory, vytváření sestav API hello](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="57664-121">If you want toosee a complete overview of using hello Azure Active Directory reporting API, see [Getting started with hello Azure Active Directory reporting API](active-directory-reporting-api-getting-started.md).</span></span>
* <span data-ttu-id="57664-122">Pokud chcete toofind Další informace o vytváření sestav Azure Active Directory, přečtěte si téma hello [Azure Active Directory průvodce vytvářením sestav](active-directory-reporting-guide.md).</span><span class="sxs-lookup"><span data-stu-id="57664-122">If you would like toofind out more about Azure Active Directory reporting, see hello [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).</span></span>  

