---
title: "Sestava aktivit aaaAzure přihlášení služby Active Directory referenční dokumentace rozhraní API | Microsoft Docs"
description: "Referenční dokumentace pro sestavu aktivit hello přihlášení k Azure Active Directory rozhraní API"
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
ms.openlocfilehash: 8123f308a291503f2c61ac5de26696806c6402ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-sign-in-activity-report-api-reference"></a><span data-ttu-id="766fd-103">Přihlašovací aktivity sestav Azure Active Directory referenční dokumentace rozhraní API</span><span class="sxs-lookup"><span data-stu-id="766fd-103">Azure Active Directory sign-in activity report API reference</span></span>
<span data-ttu-id="766fd-104">Toto téma je součástí kolekce témat o hello Azure Active Directory reporting rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="766fd-104">This topic is part of a collection of topics about hello Azure Active Directory reporting API.</span></span>  
<span data-ttu-id="766fd-105">Generování sestav služby Azure AD poskytuje rozhraní API, které vám umožní tooaccess přihlašovací aktivita sestavu dat pomocí kódu nebo související nástroje.</span><span class="sxs-lookup"><span data-stu-id="766fd-105">Azure AD reporting provides you with an API that enables you tooaccess sign-in activity report data using code or related tools.</span></span>
<span data-ttu-id="766fd-106">Hello obor tohoto tématu je tooprovide vám referenční informace o hello **API sestavy aktivity přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="766fd-106">hello scope of this topic is tooprovide you with reference information about hello **sign-in activity report API**.</span></span>

<span data-ttu-id="766fd-107">Přejděte na téma:</span><span class="sxs-lookup"><span data-stu-id="766fd-107">See:</span></span>

* <span data-ttu-id="766fd-108">[Přihlašovací aktivity](active-directory-reporting-azure-portal.md#activity-reports) další koncepční informace</span><span class="sxs-lookup"><span data-stu-id="766fd-108">[Sign-in activities](active-directory-reporting-azure-portal.md#activity-reports) for more conceptual information</span></span>
* <span data-ttu-id="766fd-109">[Začínáme s Azure Active Directory Reporting API hello](active-directory-reporting-api-getting-started.md) Další informace o hello reporting rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="766fd-109">[Getting started with hello Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md) for more information about hello reporting API.</span></span>


## <a name="who-can-access-hello-api-data"></a><span data-ttu-id="766fd-110">Kdo může přistupovat k datům hello rozhraní API?</span><span class="sxs-lookup"><span data-stu-id="766fd-110">Who can access hello API data?</span></span>
* <span data-ttu-id="766fd-111">Uživatelé a objekty služby v roli správce zabezpečení nebo zabezpečení čtečky hello</span><span class="sxs-lookup"><span data-stu-id="766fd-111">Users and Service Principals in hello Security Admin or Security Reader role</span></span>
* <span data-ttu-id="766fd-112">Globální správci</span><span class="sxs-lookup"><span data-stu-id="766fd-112">Global Admins</span></span>
* <span data-ttu-id="766fd-113">Jakékoli aplikaci, která má autorizaci tooaccess hello rozhraní API (autorizace služby app lze pouze na základě oprávnění globálního správce)</span><span class="sxs-lookup"><span data-stu-id="766fd-113">Any app that has authorization tooaccess hello API (app authorization can be setup only based on Global Admin’s permission)</span></span>

<span data-ttu-id="766fd-114">tooconfigure přístup aplikaci tooaccess zabezpečení rozhraní API jako jsou například události přihlášení, použijte hello následující aplikace hello tooadd prostředí PowerShell objektu služby do role zabezpečení čtečky hello</span><span class="sxs-lookup"><span data-stu-id="766fd-114">tooconfigure access for an application tooaccess security APIs such as signin events, use hello following PowerShell tooadd hello applications Service Principal into hello Security Reader role</span></span>

```PowerShell
Connect-MsolService
$servicePrincipal = Get-MsolServicePrincipal -AppPrincipalId "<app client id>"
$role = Get-MsolRole | ? Name -eq "Security Reader"
Add-MsolRoleMember -RoleObjectId $role.ObjectId -RoleMemberType ServicePrincipal -RoleMemberObjectId $servicePrincipal.ObjectId
```

## <a name="prerequisites"></a><span data-ttu-id="766fd-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="766fd-115">Prerequisites</span></span>
<span data-ttu-id="766fd-116">tooaccess to sestavy prostřednictvím hello reporting rozhraní API, musíte mít:</span><span class="sxs-lookup"><span data-stu-id="766fd-116">tooaccess this report through hello reporting API, you must have:</span></span>

* <span data-ttu-id="766fd-117">[Edici Azure Active Directory Premium P1 a P2](active-directory-editions.md)</span><span class="sxs-lookup"><span data-stu-id="766fd-117">An [Azure Active Directory Premium P1 or P2 edition](active-directory-editions.md)</span></span>
* <span data-ttu-id="766fd-118">Dokončené hello [požadavky tooaccess hello Azure AD reporting API](active-directory-reporting-api-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="766fd-118">Completed hello [prerequisites tooaccess hello Azure AD reporting API](active-directory-reporting-api-prerequisites.md).</span></span> 

## <a name="accessing-hello-api"></a><span data-ttu-id="766fd-119">Přístup k hello API</span><span class="sxs-lookup"><span data-stu-id="766fd-119">Accessing hello API</span></span>
<span data-ttu-id="766fd-120">Toto rozhraní API můžete buď přistupovat prostřednictvím hello [grafu Explorer](https://graphexplorer2.cloudapp.net) nebo prostřednictvím kódu programu, například pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="766fd-120">You can either access this API through hello [Graph Explorer](https://graphexplorer2.cloudapp.net) or programmatically using, for example, PowerShell.</span></span> <span data-ttu-id="766fd-121">V pořadí pro prostředí PowerShell toocorrectly interpretovat se syntaxí filtru OData hello používá při voláních REST grafu AAD, je nutné použít hello backtick (neboli: čárka) znak příliš "znaku" hello $.</span><span class="sxs-lookup"><span data-stu-id="766fd-121">In order for PowerShell toocorrectly interpret hello OData filter syntax used in AAD Graph REST calls, you must use hello backtick (aka: grave accent) character too“escape” hello $ character.</span></span> <span data-ttu-id="766fd-122">Hello backtick znak slouží jako [Powershellu řídicí znak](https://technet.microsoft.com/library/hh847755.aspx), povolení prostředí PowerShell toodo literálu výklad hello znak $ a zabránit složitá jako název proměnné prostředí PowerShell (ie: $filter).</span><span class="sxs-lookup"><span data-stu-id="766fd-122">hello backtick character serves as [PowerShell’s escape character](https://technet.microsoft.com/library/hh847755.aspx), allowing PowerShell toodo a literal interpretation of hello $ character, and avoid confusing it as a PowerShell variable name (ie: $filter).</span></span>

<span data-ttu-id="766fd-123">hello grafu Explorer je aktivní Hello tohoto tématu.</span><span class="sxs-lookup"><span data-stu-id="766fd-123">hello focus of this topic is on hello Graph Explorer.</span></span> <span data-ttu-id="766fd-124">V příkladu prostředí PowerShell najdete [skript prostředí PowerShell](active-directory-reporting-api-sign-in-activity-samples.md#powershell-script).</span><span class="sxs-lookup"><span data-stu-id="766fd-124">For a PowerShell example, see this [PowerShell script](active-directory-reporting-api-sign-in-activity-samples.md#powershell-script).</span></span>

## <a name="api-endpoint"></a><span data-ttu-id="766fd-125">Koncový bod rozhraní API</span><span class="sxs-lookup"><span data-stu-id="766fd-125">API Endpoint</span></span>
<span data-ttu-id="766fd-126">Toto rozhraní API pomocí hello následující základní identifikátor URI se můžete dostat:</span><span class="sxs-lookup"><span data-stu-id="766fd-126">You can access this API using hello following base URI:</span></span>  

    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta  



<span data-ttu-id="766fd-127">Z důvodu toohello objem dat toto rozhraní API může mít jeden milión vrácené záznamy.</span><span class="sxs-lookup"><span data-stu-id="766fd-127">Due toohello volume of data, this API has a limit of one million returned records.</span></span> 

<span data-ttu-id="766fd-128">Toto volání se vrátí hello data v dávkách.</span><span class="sxs-lookup"><span data-stu-id="766fd-128">This call returns hello data in batches.</span></span> <span data-ttu-id="766fd-129">Má každé dávky nesmí být delší než 1 000 záznamů.</span><span class="sxs-lookup"><span data-stu-id="766fd-129">Each batch has a maximum of 1000 records.</span></span>  
<span data-ttu-id="766fd-130">hello další dávku tooget záznamů, použijte odkaz Další hello.</span><span class="sxs-lookup"><span data-stu-id="766fd-130">tooget hello next batch of records, use hello Next link.</span></span> <span data-ttu-id="766fd-131">Získat hello [skiptoken](https://msdn.microsoft.com/library/dd942121.aspx) informace z první sady hello vrácené záznamy.</span><span class="sxs-lookup"><span data-stu-id="766fd-131">Get hello [skiptoken](https://msdn.microsoft.com/library/dd942121.aspx) information from hello first set of returned records.</span></span> <span data-ttu-id="766fd-132">token přeskočit Hello bude na konci hello hello sadu výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="766fd-132">hello skip token will be at hello end of hello result set.</span></span>  

    https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&%24skiptoken=-1339686058


## <a name="supported-filters"></a><span data-ttu-id="766fd-133">Podporované filtry</span><span class="sxs-lookup"><span data-stu-id="766fd-133">Supported filters</span></span>
<span data-ttu-id="766fd-134">Můžete zúžit hello počet záznamů, které se vrátí pomocí rozhraní API volat v podobě filtru.</span><span class="sxs-lookup"><span data-stu-id="766fd-134">You can narrow down hello number of records that are returned by an API call in form of a filter.</span></span>  
<span data-ttu-id="766fd-135">Pro přihlášení rozhraní API související data, hello následující filtry jsou podporovány:</span><span class="sxs-lookup"><span data-stu-id="766fd-135">For sign-in API related data, hello following filters are supported:</span></span>

* <span data-ttu-id="766fd-136">**$top =\<počet toobe záznamů vrácených\>**  -toolimit hello počet vrácených záznamů.</span><span class="sxs-lookup"><span data-stu-id="766fd-136">**$top=\<number of records toobe returned\>** - toolimit hello number of returned records.</span></span> <span data-ttu-id="766fd-137">Toto je náročná operace.</span><span class="sxs-lookup"><span data-stu-id="766fd-137">This is an expensive operation.</span></span> <span data-ttu-id="766fd-138">Pokud chcete, aby tooreturn tisíc objektů, které byste neměli používat tento filtr.</span><span class="sxs-lookup"><span data-stu-id="766fd-138">You should not use this filter if you want tooreturn thousands of objects.</span></span>  
* <span data-ttu-id="766fd-139">**$filter =\<údajů filtru\>**  -toospecify na základě hello podporovaný filtr polí, hello typ záznamy, na kterých vám nejvíc záleží</span><span class="sxs-lookup"><span data-stu-id="766fd-139">**$filter=\<your filter statement\>** - toospecify, on hello basis of supported filter fields, hello type of records you care about</span></span>

## <a name="supported-filter-fields-and-operators"></a><span data-ttu-id="766fd-140">Pole podporovaný filtr a operátory</span><span class="sxs-lookup"><span data-stu-id="766fd-140">Supported filter fields and operators</span></span>
<span data-ttu-id="766fd-141">toospecify hello typu záznamů, které se zajímáte o, můžete vytvořit filtr příkaz, který může obsahovat jedno nebo kombinaci hello následující pole filtru:</span><span class="sxs-lookup"><span data-stu-id="766fd-141">toospecify hello type of records you care about, you can build a filter statement that can contain either one or a combination of hello following filter fields:</span></span>

* <span data-ttu-id="766fd-142">[signinDateTime](#signindatetime) -definuje datum nebo rozsah dat</span><span class="sxs-lookup"><span data-stu-id="766fd-142">[signinDateTime](#signindatetime) - defines a date or date range</span></span>
* <span data-ttu-id="766fd-143">[ID uživatele](#userid) -definuje ID uživatele konkrétního uživatele na základě hello.</span><span class="sxs-lookup"><span data-stu-id="766fd-143">[userId](#userid) - defines a specific user based hello user's ID.</span></span>
* <span data-ttu-id="766fd-144">[userPrincipalName](#userprincipalname) -definuje uživatele hello konkrétního uživatele na základě hlavní název uživatele (UPN)</span><span class="sxs-lookup"><span data-stu-id="766fd-144">[userPrincipalName](#userprincipalname) - defines a specific user based hello user's user principal name (UPN)</span></span>
* <span data-ttu-id="766fd-145">[appId](#appid) -definuje ID aplikace konkrétní aplikaci na základě hello</span><span class="sxs-lookup"><span data-stu-id="766fd-145">[appId](#appid) - defines a specific app based hello app's ID</span></span>
* <span data-ttu-id="766fd-146">[appDisplayName](#appdisplayname) -definuje aplikace hello konkrétní aplikaci na základě zobrazovaný název</span><span class="sxs-lookup"><span data-stu-id="766fd-146">[appDisplayName](#appdisplayname) - defines a specific app based hello app's display name</span></span>
* <span data-ttu-id="766fd-147">[loginStatus](#loginStatus) -definuje hello stav přihlášení hello (úspěch nebo chyba)</span><span class="sxs-lookup"><span data-stu-id="766fd-147">[loginStatus](#loginStatus) - defines hello status of hello logins (success / failure)</span></span>

> [!NOTE]
> <span data-ttu-id="766fd-148">Při používání nástroje Průzkumník grafu, třeba můžete hello toouse hello správný, že je vaše pole Filtr případu pro každý písmeno.</span><span class="sxs-lookup"><span data-stu-id="766fd-148">When using Graph Explorer, you hello need toouse hello correct case for each letter is your filter fields.</span></span>
> 
> 

<span data-ttu-id="766fd-149">toonarrow dolů hello oboru hello vrátil data, můžete vytvořit kombinace hello podporované filtry a pole filtru.</span><span class="sxs-lookup"><span data-stu-id="766fd-149">toonarrow down hello scope of hello returned data, you can build combinations of hello supported filters and filter fields.</span></span> <span data-ttu-id="766fd-150">Například hello následující příkaz vrátí hello top 10 záznamy mezi 1. července 2016 a července 2016 6.:</span><span class="sxs-lookup"><span data-stu-id="766fd-150">For example, hello following statement returns hello top 10 records between July 1st 2016 and July 6th 2016:</span></span>

    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta&$top=10&$filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T00:00:00Z


- - -
### <a name="signindatetime"></a><span data-ttu-id="766fd-151">signinDateTime</span><span class="sxs-lookup"><span data-stu-id="766fd-151">signinDateTime</span></span>
<span data-ttu-id="766fd-152">**Podporované operátory**: eq, ge, le, gt, lt</span><span class="sxs-lookup"><span data-stu-id="766fd-152">**Supported operators**: eq, ge, le, gt, lt</span></span>

<span data-ttu-id="766fd-153">**Příklad**:</span><span class="sxs-lookup"><span data-stu-id="766fd-153">**Example**:</span></span>

<span data-ttu-id="766fd-154">Pomocí konkrétní datum</span><span class="sxs-lookup"><span data-stu-id="766fd-154">Using a specific date</span></span>

    $filter=signinDateTime+eq+2016-04-25T23:59:00Z    



<span data-ttu-id="766fd-155">Použití rozsahu dat.</span><span class="sxs-lookup"><span data-stu-id="766fd-155">Using a date range</span></span>    

    $filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T17:05:21Z


<span data-ttu-id="766fd-156">**Poznámky k**:</span><span class="sxs-lookup"><span data-stu-id="766fd-156">**Notes**:</span></span>

<span data-ttu-id="766fd-157">Parametr Hello data a času musí být ve formátu UTC hello</span><span class="sxs-lookup"><span data-stu-id="766fd-157">hello datetime parameter should be in hello UTC format</span></span> 

- - -
### <a name="userid"></a><span data-ttu-id="766fd-158">ID uživatele</span><span class="sxs-lookup"><span data-stu-id="766fd-158">userId</span></span>
<span data-ttu-id="766fd-159">**Podporované operátory**: eq</span><span class="sxs-lookup"><span data-stu-id="766fd-159">**Supported operators**: eq</span></span>

<span data-ttu-id="766fd-160">**Příklad**:</span><span class="sxs-lookup"><span data-stu-id="766fd-160">**Example**:</span></span>

    $filter=userId+eq+’00000000-0000-0000-0000-000000000000’

<span data-ttu-id="766fd-161">**Poznámky k**:</span><span class="sxs-lookup"><span data-stu-id="766fd-161">**Notes**:</span></span>

<span data-ttu-id="766fd-162">Hello hodnota userId je řetězcová hodnota</span><span class="sxs-lookup"><span data-stu-id="766fd-162">hello value of userId is a string value</span></span>

- - -
### <a name="userprincipalname"></a><span data-ttu-id="766fd-163">UserPrincipalName</span><span class="sxs-lookup"><span data-stu-id="766fd-163">userPrincipalName</span></span>
<span data-ttu-id="766fd-164">**Podporované operátory**: eq</span><span class="sxs-lookup"><span data-stu-id="766fd-164">**Supported operators**: eq</span></span>

<span data-ttu-id="766fd-165">**Příklad**:</span><span class="sxs-lookup"><span data-stu-id="766fd-165">**Example**:</span></span>

    $filter=userPrincipalName+eq+'audrey.oliver@wingtiptoysonline.com' 


<span data-ttu-id="766fd-166">**Poznámky k**:</span><span class="sxs-lookup"><span data-stu-id="766fd-166">**Notes**:</span></span>

<span data-ttu-id="766fd-167">Hodnota Hello userPrincipalName je řetězcová hodnota</span><span class="sxs-lookup"><span data-stu-id="766fd-167">hello value of userPrincipalName is a string value</span></span>

- - -
### <a name="appid"></a><span data-ttu-id="766fd-168">appId</span><span class="sxs-lookup"><span data-stu-id="766fd-168">appId</span></span>
<span data-ttu-id="766fd-169">**Podporované operátory**: eq</span><span class="sxs-lookup"><span data-stu-id="766fd-169">**Supported operators**: eq</span></span>

<span data-ttu-id="766fd-170">**Příklad**:</span><span class="sxs-lookup"><span data-stu-id="766fd-170">**Example**:</span></span>

    $filter=appId+eq+’00000000-0000-0000-0000-000000000000’



<span data-ttu-id="766fd-171">**Poznámky k**:</span><span class="sxs-lookup"><span data-stu-id="766fd-171">**Notes**:</span></span>

<span data-ttu-id="766fd-172">Hodnota Hello appId je řetězcová hodnota</span><span class="sxs-lookup"><span data-stu-id="766fd-172">hello value of appId is a string value</span></span>

- - -
### <a name="appdisplayname"></a><span data-ttu-id="766fd-173">appDisplayName</span><span class="sxs-lookup"><span data-stu-id="766fd-173">appDisplayName</span></span>
<span data-ttu-id="766fd-174">**Podporované operátory**: eq</span><span class="sxs-lookup"><span data-stu-id="766fd-174">**Supported operators**: eq</span></span>

<span data-ttu-id="766fd-175">**Příklad**:</span><span class="sxs-lookup"><span data-stu-id="766fd-175">**Example**:</span></span>

    $filter=appDisplayName+eq+'Azure+Portal' 


<span data-ttu-id="766fd-176">**Poznámky k**:</span><span class="sxs-lookup"><span data-stu-id="766fd-176">**Notes**:</span></span>

<span data-ttu-id="766fd-177">Hodnota Hello appDisplayName je řetězcová hodnota</span><span class="sxs-lookup"><span data-stu-id="766fd-177">hello value of appDisplayName is a string value</span></span>

- - -
### <a name="loginstatus"></a><span data-ttu-id="766fd-178">loginStatus</span><span class="sxs-lookup"><span data-stu-id="766fd-178">loginStatus</span></span>
<span data-ttu-id="766fd-179">**Podporované operátory**: eq</span><span class="sxs-lookup"><span data-stu-id="766fd-179">**Supported operators**: eq</span></span>

<span data-ttu-id="766fd-180">**Příklad**:</span><span class="sxs-lookup"><span data-stu-id="766fd-180">**Example**:</span></span>

    $filter=loginStatus+eq+'1'  


<span data-ttu-id="766fd-181">**Poznámky k**:</span><span class="sxs-lookup"><span data-stu-id="766fd-181">**Notes**:</span></span>

<span data-ttu-id="766fd-182">Existují dvě možnosti pro hello loginStatus: 0 - Úspěch, 1 – Chyba</span><span class="sxs-lookup"><span data-stu-id="766fd-182">There are two options for hello loginStatus: 0 - success, 1 - failure</span></span>

- - -
## <a name="next-steps"></a><span data-ttu-id="766fd-183">Další kroky</span><span class="sxs-lookup"><span data-stu-id="766fd-183">Next steps</span></span>
* <span data-ttu-id="766fd-184">Chcete pro filtrovaný přihlašovací aktivity toosee příklady?</span><span class="sxs-lookup"><span data-stu-id="766fd-184">Do you want toosee examples for filtered sign-in activities?</span></span> <span data-ttu-id="766fd-185">Podívejte se na hello [ukázky sestavy rozhraní API služby Azure Active Directory přihlašovací aktivita](active-directory-reporting-api-sign-in-activity-samples.md).</span><span class="sxs-lookup"><span data-stu-id="766fd-185">Check out hello [Azure Active Directory sign-in activity report API samples](active-directory-reporting-api-sign-in-activity-samples.md).</span></span>
* <span data-ttu-id="766fd-186">Chcete, aby tooknow Další informace o vytváření sestav API hello Azure AD?</span><span class="sxs-lookup"><span data-stu-id="766fd-186">Do you want tooknow more about hello Azure AD reporting API?</span></span> <span data-ttu-id="766fd-187">V tématu [Začínáme s Azure Active Directory Reporting API hello](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="766fd-187">See [Getting started with hello Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).</span></span>

