---
title: "Ukázek Azure sestavy API přihlašovací aktivita služby Active Directory | Microsoft Docs"
description: "Jak začít pracovat s Azure Active Directory Reporting API"
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
ms.openlocfilehash: 7fc2b59fe37ed2ffe85925c457300ef8fd83c3c7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-sign-in-activity-report-api-samples"></a><span data-ttu-id="57c08-103">Ukázek Azure sestavy API přihlašovací aktivita služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="57c08-103">Azure Active Directory sign-in activity report API samples</span></span>
<span data-ttu-id="57c08-104">Toto téma je součástí kolekce témat o službě Azure Active Directory, vytváření sestav rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="57c08-104">This topic is part of a collection of topics about the Azure Active Directory reporting API.</span></span>  
<span data-ttu-id="57c08-105">Generování sestav služby Azure AD poskytuje rozhraní API, která umožňuje přístup k datům přihlašovací aktivita pomocí kódu nebo související nástroje.</span><span class="sxs-lookup"><span data-stu-id="57c08-105">Azure AD reporting provides you with an API that enables you to access sign-in activity data using code or related tools.</span></span>  
<span data-ttu-id="57c08-106">Obor tohoto tématu je poskytnout ukázkový kód pro **aktivity API přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="57c08-106">The scope of this topic is to provide you with sample code for the **sign-in activity API**.</span></span>

<span data-ttu-id="57c08-107">Přejděte na téma:</span><span class="sxs-lookup"><span data-stu-id="57c08-107">See:</span></span>

* <span data-ttu-id="57c08-108">[Protokoly auditu](active-directory-reporting-azure-portal.md#activity-reports) další koncepční informace</span><span class="sxs-lookup"><span data-stu-id="57c08-108">[Audit logs](active-directory-reporting-azure-portal.md#activity-reports)  for more conceptual information</span></span>
* <span data-ttu-id="57c08-109">[Začínáme s Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md) Další informace o rozhraní API pro generování sestav.</span><span class="sxs-lookup"><span data-stu-id="57c08-109">[Getting started with the Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md) for more information about the reporting API.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="57c08-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="57c08-110">Prerequisites</span></span>
<span data-ttu-id="57c08-111">Před použitím ukázky v tomto tématu, které potřebujete k dokončení [požadavky pro přístup k Azure AD reporting rozhraní API](active-directory-reporting-api-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="57c08-111">Before you can use the samples in this topic, you need to complete the [prerequisites to access the Azure AD reporting API](active-directory-reporting-api-prerequisites.md).</span></span>  

## <a name="powershell-script"></a><span data-ttu-id="57c08-112">Skript PowerShellu</span><span class="sxs-lookup"><span data-stu-id="57c08-112">PowerShell script</span></span>
    # This script will require the Web Application and permissions setup in Azure Active Directory
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
        Write-Output "Save the output to a file SigninActivities$i.json"
        Write-Output "---------------------------------------------"
        $myReport.Content | Out-File -FilePath SigninActivities$i.json -Force
        $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
        $i = $i+1
    } while($url -ne $null)

    } else {

        Write-Host "ERROR: No Access Token"
    }




## <a name="executing-the-script"></a><span data-ttu-id="57c08-113">Provádění skriptu</span><span class="sxs-lookup"><span data-stu-id="57c08-113">Executing the script</span></span>
<span data-ttu-id="57c08-114">Se vrátí po ukončení úprav skript, spouštět a ověřte, že očekávaná data z auditu protokoluje sestavy.</span><span class="sxs-lookup"><span data-stu-id="57c08-114">Once you finish editing the script, run it and verify that the expected data from the Audit logs report is returned.</span></span>

<span data-ttu-id="57c08-115">Skript vrátí výstupní ze sestavy přihlášení ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="57c08-115">The script returns output from the sign-in report in JSON format.</span></span> <span data-ttu-id="57c08-116">Vytvoří také `SigninActivities.json` soubor s stejný výstup.</span><span class="sxs-lookup"><span data-stu-id="57c08-116">It also creates an `SigninActivities.json` file with the same output.</span></span> <span data-ttu-id="57c08-117">Můžete experimentovat změnou skript, který chcete vrátit data z jiných sestavy a komentář výstupní formáty, které nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="57c08-117">You can experiment by modifying the script to return data from other reports, and comment out the output formats that you do not need.</span></span>

## <a name="next-steps"></a><span data-ttu-id="57c08-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="57c08-118">Next Steps</span></span>
* <span data-ttu-id="57c08-119">Chcete přizpůsobit ukázky v tomto tématu?</span><span class="sxs-lookup"><span data-stu-id="57c08-119">Would you like to customize the samples in this topic?</span></span> <span data-ttu-id="57c08-120">Podívejte se [referenční dokumentace rozhraní API služby Azure Active Directory přihlašovací aktivita](active-directory-reporting-api-sign-in-activity-reference.md).</span><span class="sxs-lookup"><span data-stu-id="57c08-120">Check out the [Azure Active Directory sign-in activity API reference](active-directory-reporting-api-sign-in-activity-reference.md).</span></span> 
* <span data-ttu-id="57c08-121">Pokud chcete zobrazit úplný přehled pomocí Azure Active Directory, vytváření sestav rozhraní API, najdete v části [Začínáme s Azure Active Directory, vytváření sestav rozhraní API](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="57c08-121">If you want to see a complete overview of using the Azure Active Directory reporting API, see [Getting started with the Azure Active Directory reporting API](active-directory-reporting-api-getting-started.md).</span></span>
* <span data-ttu-id="57c08-122">Pokud chcete získat další informace o vytváření sestav Azure Active Directory, přečtěte si téma [Azure Active Directory průvodce vytvářením sestav](active-directory-reporting-guide.md).</span><span class="sxs-lookup"><span data-stu-id="57c08-122">If you would like to find out more about Azure Active Directory reporting, see the [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).</span></span>  

