---
title: "Přihlašovací aktivity sestav Azure Active Directory referenční dokumentace rozhraní API | Microsoft Docs"
description: "Referenční dokumentace rozhraní API sestavy přihlašovací aktivita služby Azure Active Directory"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ddcd9ae0-f6b7-4f13-a5e1-6cbf51a25634
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: d83f1a899ba38dab2c1c1661adede87db6f88c20
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-sign-in-activity-report-api-reference"></a><span data-ttu-id="9c592-103">Přihlašovací aktivity sestav Azure Active Directory referenční dokumentace rozhraní API</span><span class="sxs-lookup"><span data-stu-id="9c592-103">Azure Active Directory sign-in activity report API reference</span></span>
<span data-ttu-id="9c592-104">Toto téma je součástí kolekce témat o službě Azure Active Directory, vytváření sestav rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="9c592-104">This topic is part of a collection of topics about the Azure Active Directory reporting API.</span></span>  
<span data-ttu-id="9c592-105">Generování sestav služby Azure AD poskytuje rozhraní API, která umožňuje přístup k datům sestavy přihlašovací aktivita pomocí kódu nebo související nástroje.</span><span class="sxs-lookup"><span data-stu-id="9c592-105">Azure AD reporting provides you with an API that enables you to access sign-in activity report data using code or related tools.</span></span>
<span data-ttu-id="9c592-106">Obor tohoto tématu je k poskytování referenční informace o **API sestavy aktivity přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="9c592-106">The scope of this topic is to provide you with reference information about the **sign-in activity report API**.</span></span>

<span data-ttu-id="9c592-107">Přejděte na téma:</span><span class="sxs-lookup"><span data-stu-id="9c592-107">See:</span></span>

* <span data-ttu-id="9c592-108">[Přihlašovací aktivity](active-directory-reporting-azure-portal.md#activity-reports) další koncepční informace</span><span class="sxs-lookup"><span data-stu-id="9c592-108">[Sign-in activities](active-directory-reporting-azure-portal.md#activity-reports) for more conceptual information</span></span>
* <span data-ttu-id="9c592-109">[Začínáme s Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md) Další informace o rozhraní API pro generování sestav.</span><span class="sxs-lookup"><span data-stu-id="9c592-109">[Getting started with the Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md) for more information about the reporting API.</span></span>


## <a name="who-can-access-the-api-data"></a><span data-ttu-id="9c592-110">Kdo má přístup k rozhraní API data?</span><span class="sxs-lookup"><span data-stu-id="9c592-110">Who can access the API data?</span></span>
* <span data-ttu-id="9c592-111">Uživatelé a objekty služby v roli správce zabezpečení nebo čtečky zabezpečení</span><span class="sxs-lookup"><span data-stu-id="9c592-111">Users and Service Principals in the Security Admin or Security Reader role</span></span>
* <span data-ttu-id="9c592-112">Globální správci</span><span class="sxs-lookup"><span data-stu-id="9c592-112">Global Admins</span></span>
* <span data-ttu-id="9c592-113">Jakékoli aplikaci, která má oprávnění pro přístup k rozhraní API (autorizace služby app lze pouze na základě oprávnění globálního správce)</span><span class="sxs-lookup"><span data-stu-id="9c592-113">Any app that has authorization to access the API (app authorization can be setup only based on Global Admin’s permission)</span></span>

<span data-ttu-id="9c592-114">Pokud chcete konfigurovat přístup k aplikaci pro přístup k zabezpečení rozhraní API, jako jsou například události přihlášení, použijte tuto Powershellovou přidat aplikace objektu služby do role Čtenář zabezpečení</span><span class="sxs-lookup"><span data-stu-id="9c592-114">To configure access for an application to access security APIs such as signin events, use the following PowerShell to add the applications Service Principal into the Security Reader role</span></span>

```PowerShell
Connect-MsolService
$servicePrincipal = Get-MsolServicePrincipal -AppPrincipalId "<app client id>"
$role = Get-MsolRole | ? Name -eq "Security Reader"
Add-MsolRoleMember -RoleObjectId $role.ObjectId -RoleMemberType ServicePrincipal -RoleMemberObjectId $servicePrincipal.ObjectId
```

## <a name="prerequisites"></a><span data-ttu-id="9c592-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9c592-115">Prerequisites</span></span>
<span data-ttu-id="9c592-116">Přístup k této sestavě prostřednictvím rozhraní API pro vytváření sestav, musíte mít:</span><span class="sxs-lookup"><span data-stu-id="9c592-116">To access this report through the reporting API, you must have:</span></span>

* <span data-ttu-id="9c592-117">[Edici Azure Active Directory Premium P1 a P2](active-directory-editions.md)</span><span class="sxs-lookup"><span data-stu-id="9c592-117">An [Azure Active Directory Premium P1 or P2 edition](active-directory-editions.md)</span></span>
* <span data-ttu-id="9c592-118">Byla dokončena [požadavky pro přístup k Azure AD reporting rozhraní API](active-directory-reporting-api-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="9c592-118">Completed the [prerequisites to access the Azure AD reporting API](active-directory-reporting-api-prerequisites.md).</span></span> 

## <a name="accessing-the-api"></a><span data-ttu-id="9c592-119">Přístup k rozhraní API</span><span class="sxs-lookup"><span data-stu-id="9c592-119">Accessing the API</span></span>
<span data-ttu-id="9c592-120">Můžete buď přístup toto rozhraní API prostřednictvím [Explorer grafu](https://graphexplorer2.cloudapp.net) nebo prostřednictvím kódu programu, například pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9c592-120">You can either access this API through the [Graph Explorer](https://graphexplorer2.cloudapp.net) or programmatically using, for example, PowerShell.</span></span> <span data-ttu-id="9c592-121">Aby PowerShell správně interpretovat syntaxe filtru OData, který se používá při voláních REST grafu AAD, je nutné použít backtick (neboli: čárka) znak "řídicí" znak $.</span><span class="sxs-lookup"><span data-stu-id="9c592-121">In order for PowerShell to correctly interpret the OData filter syntax used in AAD Graph REST calls, you must use the backtick (aka: grave accent) character to “escape” the $ character.</span></span> <span data-ttu-id="9c592-122">Backtick znak, který slouží jako [Powershellu řídicí znak](https://technet.microsoft.com/library/hh847755.aspx), povolení prostředí PowerShell provést literálu výklad znak $, a zamezit tak složitá jako název proměnné prostředí PowerShell (ie: $filter).</span><span class="sxs-lookup"><span data-stu-id="9c592-122">The backtick character serves as [PowerShell’s escape character](https://technet.microsoft.com/library/hh847755.aspx), allowing PowerShell to do a literal interpretation of the $ character, and avoid confusing it as a PowerShell variable name (ie: $filter).</span></span>

<span data-ttu-id="9c592-123">Graf Explorer je aktivní v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="9c592-123">The focus of this topic is on the Graph Explorer.</span></span> <span data-ttu-id="9c592-124">V příkladu prostředí PowerShell najdete [skript prostředí PowerShell](active-directory-reporting-api-sign-in-activity-samples.md#powershell-script).</span><span class="sxs-lookup"><span data-stu-id="9c592-124">For a PowerShell example, see this [PowerShell script](active-directory-reporting-api-sign-in-activity-samples.md#powershell-script).</span></span>

## <a name="api-endpoint"></a><span data-ttu-id="9c592-125">Koncový bod rozhraní API</span><span class="sxs-lookup"><span data-stu-id="9c592-125">API Endpoint</span></span>
<span data-ttu-id="9c592-126">Toto rozhraní API pomocí následující základní identifikátor URI se můžete dostat:</span><span class="sxs-lookup"><span data-stu-id="9c592-126">You can access this API using the following base URI:</span></span>  

    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta  



<span data-ttu-id="9c592-127">Z důvodu objem dat toto rozhraní API může mít jeden milión vrácené záznamy.</span><span class="sxs-lookup"><span data-stu-id="9c592-127">Due to the volume of data, this API has a limit of one million returned records.</span></span> 

<span data-ttu-id="9c592-128">Toto volání se vrátí data v dávkách.</span><span class="sxs-lookup"><span data-stu-id="9c592-128">This call returns the data in batches.</span></span> <span data-ttu-id="9c592-129">Má každé dávky nesmí být delší než 1 000 záznamů.</span><span class="sxs-lookup"><span data-stu-id="9c592-129">Each batch has a maximum of 1000 records.</span></span>  
<span data-ttu-id="9c592-130">Chcete-li získat další dávky záznamů, použijte odkaz na další.</span><span class="sxs-lookup"><span data-stu-id="9c592-130">To get the next batch of records, use the Next link.</span></span> <span data-ttu-id="9c592-131">Získat [skiptoken](https://msdn.microsoft.com/library/dd942121.aspx) informace z první sady vrácené záznamy.</span><span class="sxs-lookup"><span data-stu-id="9c592-131">Get the [skiptoken](https://msdn.microsoft.com/library/dd942121.aspx) information from the first set of returned records.</span></span> <span data-ttu-id="9c592-132">Token přeskočit bude na konci výsledek nastaveno.</span><span class="sxs-lookup"><span data-stu-id="9c592-132">The skip token will be at the end of the result set.</span></span>  

    https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&%24skiptoken=-1339686058


## <a name="supported-filters"></a><span data-ttu-id="9c592-133">Podporované filtry</span><span class="sxs-lookup"><span data-stu-id="9c592-133">Supported filters</span></span>
<span data-ttu-id="9c592-134">Počet záznamů, které se vrátí pomocí rozhraní API můžete zúžit volání v podobě filtru.</span><span class="sxs-lookup"><span data-stu-id="9c592-134">You can narrow down the number of records that are returned by an API call in form of a filter.</span></span>  
<span data-ttu-id="9c592-135">Pro přihlášení rozhraní API související data, jsou podporovány následující filtry:</span><span class="sxs-lookup"><span data-stu-id="9c592-135">For sign-in API related data, the following filters are supported:</span></span>

* <span data-ttu-id="9c592-136">**$top =\<počet vrácených\>**  – Pokud chcete omezit počet vrácených záznamů.</span><span class="sxs-lookup"><span data-stu-id="9c592-136">**$top=\<number of records to be returned\>** - to limit the number of returned records.</span></span> <span data-ttu-id="9c592-137">Toto je náročná operace.</span><span class="sxs-lookup"><span data-stu-id="9c592-137">This is an expensive operation.</span></span> <span data-ttu-id="9c592-138">Tento filtr byste neměli používat, pokud chcete vrátit tisíce objektů.</span><span class="sxs-lookup"><span data-stu-id="9c592-138">You should not use this filter if you want to return thousands of objects.</span></span>  
* <span data-ttu-id="9c592-139">**$filter =\<údajů filtru\>**  – Pokud chcete zadat typ záznamy, na kterých vám nejvíc záleží na základě pole podporovaný filtr</span><span class="sxs-lookup"><span data-stu-id="9c592-139">**$filter=\<your filter statement\>** - to specify, on the basis of supported filter fields, the type of records you care about</span></span>

## <a name="supported-filter-fields-and-operators"></a><span data-ttu-id="9c592-140">Pole podporovaný filtr a operátory</span><span class="sxs-lookup"><span data-stu-id="9c592-140">Supported filter fields and operators</span></span>
<span data-ttu-id="9c592-141">Pokud chcete zadat typ záznamů, které se zajímáte o, můžete vytvořit filtr příkaz, který může obsahovat jedno nebo kombinaci následující pole filtru:</span><span class="sxs-lookup"><span data-stu-id="9c592-141">To specify the type of records you care about, you can build a filter statement that can contain either one or a combination of the following filter fields:</span></span>

* <span data-ttu-id="9c592-142">[signinDateTime](#signindatetime) -definuje datum nebo rozsah dat</span><span class="sxs-lookup"><span data-stu-id="9c592-142">[signinDateTime](#signindatetime) - defines a date or date range</span></span>
* <span data-ttu-id="9c592-143">[ID uživatele](#userid) -definuje konkrétní uživatele na základě ID uživatele.</span><span class="sxs-lookup"><span data-stu-id="9c592-143">[userId](#userid) - defines a specific user based the user's ID.</span></span>
* <span data-ttu-id="9c592-144">[userPrincipalName](#userprincipalname) -definuje konkrétní uživatele na základě uživatele hlavní název uživatele (UPN)</span><span class="sxs-lookup"><span data-stu-id="9c592-144">[userPrincipalName](#userprincipalname) - defines a specific user based the user's user principal name (UPN)</span></span>
* <span data-ttu-id="9c592-145">[appId](#appid) -definuje konkrétní aplikace na základě ID aplikace</span><span class="sxs-lookup"><span data-stu-id="9c592-145">[appId](#appid) - defines a specific app based the app's ID</span></span>
* <span data-ttu-id="9c592-146">[appDisplayName](#appdisplayname) -definuje konkrétní aplikace na základě aplikace zobrazovaný název</span><span class="sxs-lookup"><span data-stu-id="9c592-146">[appDisplayName](#appdisplayname) - defines a specific app based the app's display name</span></span>
* <span data-ttu-id="9c592-147">[loginStatus](#loginStatus) -definuje stav přihlášení (úspěch nebo chyba)</span><span class="sxs-lookup"><span data-stu-id="9c592-147">[loginStatus](#loginStatus) - defines the status of the logins (success / failure)</span></span>

> [!NOTE]
> <span data-ttu-id="9c592-148">Při použití Průzkumníku grafu, je potřeba mít správnou velikost pro každý písmeno je vaše pole filtru.</span><span class="sxs-lookup"><span data-stu-id="9c592-148">When using Graph Explorer, you the need to use the correct case for each letter is your filter fields.</span></span>
> 
> 

<span data-ttu-id="9c592-149">Chcete-li zúžit rozsah vrácená data, můžete vytvořit kombinace podporované filtry a pole filtru.</span><span class="sxs-lookup"><span data-stu-id="9c592-149">To narrow down the scope of the returned data, you can build combinations of the supported filters and filter fields.</span></span> <span data-ttu-id="9c592-150">Například následující příkaz vrátí top 10 záznamy mezi 1. července 2016 a července 2016 6.:</span><span class="sxs-lookup"><span data-stu-id="9c592-150">For example, the following statement returns the top 10 records between July 1st 2016 and July 6th 2016:</span></span>

    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta&$top=10&$filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T00:00:00Z


- - -
### <a name="signindatetime"></a><span data-ttu-id="9c592-151">signinDateTime</span><span class="sxs-lookup"><span data-stu-id="9c592-151">signinDateTime</span></span>
<span data-ttu-id="9c592-152">**Podporované operátory**: eq, ge, le, gt, lt</span><span class="sxs-lookup"><span data-stu-id="9c592-152">**Supported operators**: eq, ge, le, gt, lt</span></span>

<span data-ttu-id="9c592-153">**Příklad**:</span><span class="sxs-lookup"><span data-stu-id="9c592-153">**Example**:</span></span>

<span data-ttu-id="9c592-154">Pomocí konkrétní datum</span><span class="sxs-lookup"><span data-stu-id="9c592-154">Using a specific date</span></span>

    $filter=signinDateTime+eq+2016-04-25T23:59:00Z    



<span data-ttu-id="9c592-155">Použití rozsahu dat.</span><span class="sxs-lookup"><span data-stu-id="9c592-155">Using a date range</span></span>    

    $filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T17:05:21Z


<span data-ttu-id="9c592-156">**Poznámky k**:</span><span class="sxs-lookup"><span data-stu-id="9c592-156">**Notes**:</span></span>

<span data-ttu-id="9c592-157">Parametr data a času musí být ve formátu UTC</span><span class="sxs-lookup"><span data-stu-id="9c592-157">The datetime parameter should be in the UTC format</span></span> 

- - -
### <a name="userid"></a><span data-ttu-id="9c592-158">ID uživatele</span><span class="sxs-lookup"><span data-stu-id="9c592-158">userId</span></span>
<span data-ttu-id="9c592-159">**Podporované operátory**: eq</span><span class="sxs-lookup"><span data-stu-id="9c592-159">**Supported operators**: eq</span></span>

<span data-ttu-id="9c592-160">**Příklad**:</span><span class="sxs-lookup"><span data-stu-id="9c592-160">**Example**:</span></span>

    $filter=userId+eq+’00000000-0000-0000-0000-000000000000’

<span data-ttu-id="9c592-161">**Poznámky k**:</span><span class="sxs-lookup"><span data-stu-id="9c592-161">**Notes**:</span></span>

<span data-ttu-id="9c592-162">Hodnota ID uživatele je řetězcová hodnota</span><span class="sxs-lookup"><span data-stu-id="9c592-162">The value of userId is a string value</span></span>

- - -
### <a name="userprincipalname"></a><span data-ttu-id="9c592-163">UserPrincipalName</span><span class="sxs-lookup"><span data-stu-id="9c592-163">userPrincipalName</span></span>
<span data-ttu-id="9c592-164">**Podporované operátory**: eq</span><span class="sxs-lookup"><span data-stu-id="9c592-164">**Supported operators**: eq</span></span>

<span data-ttu-id="9c592-165">**Příklad**:</span><span class="sxs-lookup"><span data-stu-id="9c592-165">**Example**:</span></span>

    $filter=userPrincipalName+eq+'audrey.oliver@wingtiptoysonline.com' 


<span data-ttu-id="9c592-166">**Poznámky k**:</span><span class="sxs-lookup"><span data-stu-id="9c592-166">**Notes**:</span></span>

<span data-ttu-id="9c592-167">Hodnota userPrincipalName je řetězcová hodnota</span><span class="sxs-lookup"><span data-stu-id="9c592-167">The value of userPrincipalName is a string value</span></span>

- - -
### <a name="appid"></a><span data-ttu-id="9c592-168">appId</span><span class="sxs-lookup"><span data-stu-id="9c592-168">appId</span></span>
<span data-ttu-id="9c592-169">**Podporované operátory**: eq</span><span class="sxs-lookup"><span data-stu-id="9c592-169">**Supported operators**: eq</span></span>

<span data-ttu-id="9c592-170">**Příklad**:</span><span class="sxs-lookup"><span data-stu-id="9c592-170">**Example**:</span></span>

    $filter=appId+eq+’00000000-0000-0000-0000-000000000000’



<span data-ttu-id="9c592-171">**Poznámky k**:</span><span class="sxs-lookup"><span data-stu-id="9c592-171">**Notes**:</span></span>

<span data-ttu-id="9c592-172">Hodnota appId je řetězcová hodnota</span><span class="sxs-lookup"><span data-stu-id="9c592-172">The value of appId is a string value</span></span>

- - -
### <a name="appdisplayname"></a><span data-ttu-id="9c592-173">appDisplayName</span><span class="sxs-lookup"><span data-stu-id="9c592-173">appDisplayName</span></span>
<span data-ttu-id="9c592-174">**Podporované operátory**: eq</span><span class="sxs-lookup"><span data-stu-id="9c592-174">**Supported operators**: eq</span></span>

<span data-ttu-id="9c592-175">**Příklad**:</span><span class="sxs-lookup"><span data-stu-id="9c592-175">**Example**:</span></span>

    $filter=appDisplayName+eq+'Azure+Portal' 


<span data-ttu-id="9c592-176">**Poznámky k**:</span><span class="sxs-lookup"><span data-stu-id="9c592-176">**Notes**:</span></span>

<span data-ttu-id="9c592-177">Hodnota appDisplayName je řetězcová hodnota</span><span class="sxs-lookup"><span data-stu-id="9c592-177">The value of appDisplayName is a string value</span></span>

- - -
### <a name="loginstatus"></a><span data-ttu-id="9c592-178">loginStatus</span><span class="sxs-lookup"><span data-stu-id="9c592-178">loginStatus</span></span>
<span data-ttu-id="9c592-179">**Podporované operátory**: eq</span><span class="sxs-lookup"><span data-stu-id="9c592-179">**Supported operators**: eq</span></span>

<span data-ttu-id="9c592-180">**Příklad**:</span><span class="sxs-lookup"><span data-stu-id="9c592-180">**Example**:</span></span>

    $filter=loginStatus+eq+'1'  


<span data-ttu-id="9c592-181">**Poznámky k**:</span><span class="sxs-lookup"><span data-stu-id="9c592-181">**Notes**:</span></span>

<span data-ttu-id="9c592-182">Existují dvě možnosti pro loginStatus: 0 - Úspěch, 1 – Chyba</span><span class="sxs-lookup"><span data-stu-id="9c592-182">There are two options for the loginStatus: 0 - success, 1 - failure</span></span>

- - -
## <a name="next-steps"></a><span data-ttu-id="9c592-183">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9c592-183">Next steps</span></span>
* <span data-ttu-id="9c592-184">Chcete příklady pro filtrovaný přihlašovací aktivity?</span><span class="sxs-lookup"><span data-stu-id="9c592-184">Do you want to see examples for filtered sign-in activities?</span></span> <span data-ttu-id="9c592-185">Podívejte se [ukázky sestavy rozhraní API služby Azure Active Directory přihlašovací aktivita](active-directory-reporting-api-sign-in-activity-samples.md).</span><span class="sxs-lookup"><span data-stu-id="9c592-185">Check out the [Azure Active Directory sign-in activity report API samples](active-directory-reporting-api-sign-in-activity-samples.md).</span></span>
* <span data-ttu-id="9c592-186">Opravdu chcete získat další informace o generování sestav rozhraní API Azure AD?</span><span class="sxs-lookup"><span data-stu-id="9c592-186">Do you want to know more about the Azure AD reporting API?</span></span> <span data-ttu-id="9c592-187">V tématu [Začínáme s Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="9c592-187">See [Getting started with the Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).</span></span>

