---
title: "vytváření sestav služby Active Directory aaaAzure audit rozhraní API ukázky | Microsoft Docs"
description: Jak tooget pracovat s Azure Active Directory Reporting API hello
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: de8b8ec3-49b3-4aa8-93fb-e38f52c99743
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/02/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 6ada8a7184d7baacaba5ba9c1b9130653b1cf7fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-reporting-audit-api-samples"></a><span data-ttu-id="457f3-103">Azure Active Directory, vytváření sestav ukázky auditu rozhraní API</span><span class="sxs-lookup"><span data-stu-id="457f3-103">Azure Active Directory reporting audit API samples</span></span>
<span data-ttu-id="457f3-104">Toto téma je součástí kolekce témat o hello Azure Active Directory reporting rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="457f3-104">This topic is part of a collection of topics about hello Azure Active Directory reporting API.</span></span>  
<span data-ttu-id="457f3-105">Generování sestav služby Azure AD poskytuje rozhraní API, které vám umožní tooaccess auditu dat pomocí kódu nebo související nástroje.</span><span class="sxs-lookup"><span data-stu-id="457f3-105">Azure AD reporting provides you with an API that enables you tooaccess audit data using code or related tools.</span></span>
<span data-ttu-id="457f3-106">Hello obor tohoto tématu je k ukázkový kód hello tooprovide **audit rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="457f3-106">hello scope of this topic is tooprovide you with sample code for hello **audit API**.</span></span>

<span data-ttu-id="457f3-107">Přejděte na téma:</span><span class="sxs-lookup"><span data-stu-id="457f3-107">See:</span></span>

* <span data-ttu-id="457f3-108">[Protokoly auditu](active-directory-reporting-azure-portal.md#activity-reports) další koncepční informace</span><span class="sxs-lookup"><span data-stu-id="457f3-108">[Audit logs](active-directory-reporting-azure-portal.md#activity-reports) for more conceptual information</span></span>
* <span data-ttu-id="457f3-109">[Začínáme s Azure Active Directory Reporting API hello](active-directory-reporting-api-getting-started.md) Další informace o hello reporting rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="457f3-109">[Getting started with hello Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md) for more information about hello reporting API.</span></span>

<span data-ttu-id="457f3-110">Pro dotazy, problémy nebo připomínky, obraťte se na [AAD Reporting pomoci](mailto:aadreportinghelp@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="457f3-110">For questions, issues or feedback, please contact [AAD Reporting Help](mailto:aadreportinghelp@microsoft.com).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="457f3-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="457f3-111">Prerequisites</span></span>
<span data-ttu-id="457f3-112">Než použijete hello ukázky v tomto tématu, musíte toocomplete hello [požadavky tooaccess hello Azure AD reporting API](active-directory-reporting-api-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="457f3-112">Before you can use hello samples in this topic, you need toocomplete hello [prerequisites tooaccess hello Azure AD reporting API](active-directory-reporting-api-prerequisites.md).</span></span>  

## <a name="known-issue"></a><span data-ttu-id="457f3-113">Známý problém</span><span class="sxs-lookup"><span data-stu-id="457f3-113">Known issue</span></span>
<span data-ttu-id="457f3-114">Ověřování aplikace nebude fungovat, pokud se váš klient je v oblasti EU hello.</span><span class="sxs-lookup"><span data-stu-id="457f3-114">App Auth will not work if your tenant is in hello EU region.</span></span> <span data-ttu-id="457f3-115">Použijte prosím ověřování uživatele pro přístup k hello auditu API jako alternativní řešení, dokud jsme hello problém opravte.</span><span class="sxs-lookup"><span data-stu-id="457f3-115">Please use User Auth for accessing hello Audit API as a workaround until we fix hello issue.</span></span> 

## <a name="powershell-script"></a><span data-ttu-id="457f3-116">Skript PowerShellu</span><span class="sxs-lookup"><span data-stu-id="457f3-116">PowerShell script</span></span>
    # This script will require registration of a Web Application in Azure Active Directory (see https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/)

    # Constants
    $ClientID       = "your-client-application-id-here"       # Insert your application's Client ID, a Globally Unique ID (registered by Global Admin)
    $ClientSecret   = "your-client-application-secret-here"   # Insert your application's Client Key/Secret string
    $loginURL       = "https://login.microsoftonline.com"     # AAD Instance; for example https://login.microsoftonline.com
    $tenantdomain   = "your-tenant-domain.onmicrosoft.com"    # AAD Tenant; for example, contoso.onmicrosoft.com
    $resource       = "https://graph.windows.net"             # Azure AD Graph API resource URI
    $7daysago       = "{0:s}" -f (get-date).AddDays(-7) + "Z" # Use 'AddMinutes(-5)' toodecrement minutes, for example
    Write-Output "Searching for events starting $7daysago"

    # Create HTTP header, get an OAuth2 access token based on client id, secret and tenant domain
    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    # Parse audit report items, save output toofile(s): auditX.json, where X = 0 thru n for number of nextLink pages
    if ($oauth.access_token -ne $null) {   
        $i=0
        $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}
        $url = 'https://graph.windows.net/' + $tenantdomain + '/activities/audit?api-version=beta&`$filter=activityDate gt ' + $7daysago

        # loop through each query page (1 through n)
        Do{
            # display each event on hello console window
            Write-Output "Fetching data using Uri: $url"
            $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
            foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
                Write-Output ($event | ConvertTo-Json)
            }

            # save hello query page tooan output file
            Write-Output "Save hello output tooa file audit$i.json"
            $myReport.Content | Out-File -FilePath audit$i.json -Force
            $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
            $i = $i+1
        } while($url -ne $null)
    } else {
        Write-Host "ERROR: No Access Token"
        }

    Write-Host "Press any key toocontinue ..."
    $x = $host.UI.RawUI.ReadKey("NoEcho,IncludeKeyDown")


### <a name="executing-hello-powershell-script"></a><span data-ttu-id="457f3-117">Provádění skriptu prostředí PowerShell hello</span><span class="sxs-lookup"><span data-stu-id="457f3-117">Executing hello PowerShell script</span></span>
<span data-ttu-id="457f3-118">Jednou dokončíte úpravy hello skriptu, spouštět a ověřte, zda že tento hello očekává, že se vrátí data z hello sestavy protokolů auditu.</span><span class="sxs-lookup"><span data-stu-id="457f3-118">Once you finish editing hello script, run it and verify that hello expected data from hello Audit logs report is returned.</span></span>

<span data-ttu-id="457f3-119">Hello skript vrátí výstupní hello auditu sestavy ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="457f3-119">hello script returns output from hello audit report in JSON format.</span></span> <span data-ttu-id="457f3-120">Vytvoří také `audit.json` soubor s hello stejný výstup.</span><span class="sxs-lookup"><span data-stu-id="457f3-120">It also creates an `audit.json` file with hello same output.</span></span> <span data-ttu-id="457f3-121">Můžete experimentovat změnou hello skriptu tooreturn data z jiných sestavy a komentář hello výstupní formáty, které nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="457f3-121">You can experiment by modifying hello script tooreturn data from other reports, and comment out hello output formats that you do not need.</span></span>

## <a name="bash-script"></a><span data-ttu-id="457f3-122">Bash skriptu</span><span class="sxs-lookup"><span data-stu-id="457f3-122">Bash script</span></span>
    #!/bin/bash

    # Author: Ken Hoff (kenhoff@microsoft.com)
    # Date: 2015.08.20
    # NOTE: This script requires jq (https://stedolan.github.io/jq/)

    CLIENT_ID="your-application-client-id-here"         # Should be a ~35 character string insert your info here
    CLIENT_SECRET="your-application-client-secret-here" # Should be a ~44 character string insert your info here
    LOGIN_URL="https://login.microsoftonline.com"
    TENANT_DOMAIN="your-directory-name-here.onmicrosoft.com"    # For example, contoso.onmicrosoft.com

    TOKEN_INFO=$(curl -s --data-urlencode "grant_type=client_credentials" --data-urlencode "client_id=$CLIENT_ID" --data-urlencode "client_secret=$CLIENT_SECRET" "$LOGIN_URL/$TENANT_DOMAIN/oauth2/token?api-version=1.0")

    TOKEN_TYPE=$(echo $TOKEN_INFO | ./jq-win64.exe -r '.token_type')
    ACCESS_TOKEN=$(echo $TOKEN_INFO | ./jq-win64.exe -r '.access_token')

    # get yesterday's date

    YESTERDAY=$(date --date='1 day ago' +'%Y-%m-%d')

    URL="https://graph.windows.net/$TENANT_DOMAIN/activities/audit?api-version=beta&$filter=activityDate%20gt%20$YESTERDAY"


    REPORT=$(curl -s --header "Authorization: $TOKEN_TYPE $ACCESS_TOKEN" $URL)

    echo $REPORT | ./jq-win64.exe -r '.value' | ./jq-win64.exe -r ".[]"

## <a name="python-script"></a><span data-ttu-id="457f3-123">Skript v jazyce Python</span><span class="sxs-lookup"><span data-stu-id="457f3-123">Python script</span></span>
    # Author: Michael McLaughlin (michmcla@microsoft.com)
    # Date: January 20, 2016
    # This requires hello Python Requests module: http://docs.python-requests.org

    import requests
    import datetime
    import sys

    client_id = 'your-application-client-id-here'
    client_secret = 'your-application-client-secret-here'
    login_url = 'https://login.microsoftonline.com/'
    tenant_domain = 'your-directory-name-here.onmicrosoft.com'

    # Get an OAuth access token
    bodyvals = {'client_id': client_id,
                'client_secret': client_secret,
                'grant_type': 'client_credentials'}

    request_url = login_url + tenant_domain + '/oauth2/token?api-version=1.0'
    token_response = requests.post(request_url, data=bodyvals)

    access_token = token_response.json().get('access_token')
    token_type = token_response.json().get('token_type')

    if access_token is None or token_type is None:
        print "ERROR: Couldn't get access token"
        sys.exit(1)

    # Use hello access token toomake hello API request
    yesterday = datetime.date.strftime(datetime.date.today() - datetime.timedelta(days=1), '%Y-%m-%d')

    header_params = {'Authorization': token_type + ' ' + access_token}
    request_string = 'https://graph.windows.net/' + tenant_domain + 'activities/audit?api-version=beta&$filter=activityDate%20gt%20' + yesterday   
    response = requests.get(request_string, headers = header_params)

    if response.status_code is 200:
        print response.content
    else:
        print 'ERROR: API request failed'





## <a name="next-steps"></a><span data-ttu-id="457f3-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="457f3-124">Next steps</span></span>
* <span data-ttu-id="457f3-125">Chcete, aby toocustomize hello ukázky v tomto tématu?</span><span class="sxs-lookup"><span data-stu-id="457f3-125">Would you like toocustomize hello samples in this topic?</span></span> <span data-ttu-id="457f3-126">Podívejte se na hello [auditování Azure Active Directory referenční dokumentace rozhraní API](active-directory-reporting-api-audit-reference.md).</span><span class="sxs-lookup"><span data-stu-id="457f3-126">Check out hello [Azure Active Directory audit API reference](active-directory-reporting-api-audit-reference.md).</span></span> 
* <span data-ttu-id="457f3-127">Pokud chcete, aby toosee úplný přehled pomocí hello Azure Active Directory, vytváření sestav rozhraní API najdete v tématu [Začínáme s Azure Active Directory, vytváření sestav API hello](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="457f3-127">If you want toosee a complete overview of using hello Azure Active Directory reporting API, see [Getting started with hello Azure Active Directory reporting API](active-directory-reporting-api-getting-started.md).</span></span>
* <span data-ttu-id="457f3-128">Pokud chcete toofind Další informace o vytváření sestav Azure Active Directory, přečtěte si téma hello [Azure Active Directory průvodce vytvářením sestav](active-directory-reporting-guide.md).</span><span class="sxs-lookup"><span data-stu-id="457f3-128">If you would like toofind out more about Azure Active Directory reporting, see hello [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).</span></span>  

