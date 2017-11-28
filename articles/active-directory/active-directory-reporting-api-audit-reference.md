---
title: "auditování služby Active Directory aaaAzure referenční dokumentace rozhraní API | Microsoft Docs"
description: "Jak tooget pracovat s hello API auditování Azure Active Directory"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 44e46be8-09e5-4981-be2b-d474aaa92792
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/05/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 5f33b62ede9be445f35704739e328580dc454368
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-audit-api-reference"></a><span data-ttu-id="de427-103">Azure Active Directory auditu referenční dokumentace rozhraní API</span><span class="sxs-lookup"><span data-stu-id="de427-103">Azure Active Directory audit API reference</span></span>
<span data-ttu-id="de427-104">Toto téma je součástí kolekce témat o hello Azure Active Directory reporting rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="de427-104">This topic is part of a collection of topics about hello Azure Active Directory reporting API.</span></span>  
<span data-ttu-id="de427-105">Generování sestav služby Azure AD poskytuje rozhraní API, které vám umožní tooaccess auditu dat pomocí kódu nebo související nástroje.</span><span class="sxs-lookup"><span data-stu-id="de427-105">Azure AD reporting provides you with an API that enables you tooaccess audit data using code or related tools.</span></span>
<span data-ttu-id="de427-106">Hello obor tohoto tématu je tooprovide vám referenční informace o hello **audit rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="de427-106">hello scope of this topic is tooprovide you with reference information about hello **audit API**.</span></span>

<span data-ttu-id="de427-107">Přejděte na téma:</span><span class="sxs-lookup"><span data-stu-id="de427-107">See:</span></span>

* <span data-ttu-id="de427-108">[Protokoly auditu](active-directory-reporting-azure-portal.md#activity-reports) další koncepční informace</span><span class="sxs-lookup"><span data-stu-id="de427-108">[Audit logs](active-directory-reporting-azure-portal.md#activity-reports)  for more conceptual information</span></span>

* <span data-ttu-id="de427-109">[Začínáme s Azure Active Directory Reporting API hello](active-directory-reporting-api-getting-started.md) Další informace o hello reporting rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="de427-109">[Getting started with hello Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md) for more information about hello reporting API.</span></span>


<span data-ttu-id="de427-110">Pro:</span><span class="sxs-lookup"><span data-stu-id="de427-110">For:</span></span>

- <span data-ttu-id="de427-111">Nejčastější dotazy, přečtěte si naše [– nejčastější dotazy](active-directory-reporting-faq.md)</span><span class="sxs-lookup"><span data-stu-id="de427-111">Frequently asked questions, read our [FAQ](active-directory-reporting-faq.md)</span></span> 

- <span data-ttu-id="de427-112">Problémy prosím [souboru lístek podpory](active-directory-troubleshooting-support-howto.md)</span><span class="sxs-lookup"><span data-stu-id="de427-112">Issues, please [file a support ticket](active-directory-troubleshooting-support-howto.md)</span></span> 


## <a name="who-can-access-hello-data"></a><span data-ttu-id="de427-113">Kdo může přistupovat k datům hello?</span><span class="sxs-lookup"><span data-stu-id="de427-113">Who can access hello data?</span></span>
* <span data-ttu-id="de427-114">Uživatelé v roli správce zabezpečení nebo zabezpečení čtečky hello</span><span class="sxs-lookup"><span data-stu-id="de427-114">Users in hello Security Admin or Security Reader role</span></span>
* <span data-ttu-id="de427-115">Globální správci</span><span class="sxs-lookup"><span data-stu-id="de427-115">Global Admins</span></span>
* <span data-ttu-id="de427-116">Jakékoli aplikaci, která má autorizaci tooaccess hello rozhraní API (autorizace služby app lze pouze na základě oprávnění globálního správce)</span><span class="sxs-lookup"><span data-stu-id="de427-116">Any app that has authorization tooaccess hello API (app authorization can be setup only based on Global Admin’s permission)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="de427-117">Požadavky</span><span class="sxs-lookup"><span data-stu-id="de427-117">Prerequisites</span></span>
<span data-ttu-id="de427-118">V pořadí tooaccess to sestavy prostřednictvím hello Reporting rozhraní API, musíte mít:</span><span class="sxs-lookup"><span data-stu-id="de427-118">In order tooaccess this report through hello Reporting API, you must have:</span></span>

* <span data-ttu-id="de427-119">[Lepší edici nebo Azure Active Directory volné](active-directory-editions.md)</span><span class="sxs-lookup"><span data-stu-id="de427-119">An [Azure Active Directory Free or better edition](active-directory-editions.md)</span></span>
* <span data-ttu-id="de427-120">Dokončené hello [požadavky tooaccess hello Azure AD reporting API](active-directory-reporting-api-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="de427-120">Completed hello [prerequisites tooaccess hello Azure AD reporting API](active-directory-reporting-api-prerequisites.md).</span></span> 

## <a name="accessing-hello-api"></a><span data-ttu-id="de427-121">Přístup k hello API</span><span class="sxs-lookup"><span data-stu-id="de427-121">Accessing hello API</span></span>
<span data-ttu-id="de427-122">Toto rozhraní API můžete buď přistupovat prostřednictvím hello [grafu Explorer](https://graphexplorer2.cloudapp.net) nebo prostřednictvím kódu programu, například pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="de427-122">You can either access this API through hello [Graph Explorer](https://graphexplorer2.cloudapp.net) or programmatically using, for example, PowerShell.</span></span> <span data-ttu-id="de427-123">V pořadí pro prostředí PowerShell toocorrectly interpretovat se syntaxí filtru OData hello používá při voláních REST grafu AAD, je nutné použít hello backtick (neboli: čárka) znak příliš "znaku" hello $.</span><span class="sxs-lookup"><span data-stu-id="de427-123">In order for PowerShell toocorrectly interpret hello OData filter syntax used in AAD Graph REST calls, you must use hello backtick (aka: grave accent) character too“escape” hello $ character.</span></span> <span data-ttu-id="de427-124">Hello backtick znak slouží jako [Powershellu řídicí znak](https://technet.microsoft.com/library/hh847755.aspx), povolení prostředí PowerShell toodo literálu výklad hello znak $ a zabránit složitá jako název proměnné prostředí PowerShell (ie: $filter).</span><span class="sxs-lookup"><span data-stu-id="de427-124">hello backtick character serves as [PowerShell’s escape character](https://technet.microsoft.com/library/hh847755.aspx), allowing PowerShell toodo a literal interpretation of hello $ character, and avoid confusing it as a PowerShell variable name (ie: $filter).</span></span>

<span data-ttu-id="de427-125">hello grafu Explorer je aktivní Hello tohoto tématu.</span><span class="sxs-lookup"><span data-stu-id="de427-125">hello focus of this topic is on hello Graph Explorer.</span></span> <span data-ttu-id="de427-126">V příkladu prostředí PowerShell najdete [skript prostředí PowerShell](active-directory-reporting-api-audit-samples.md#powershell-script).</span><span class="sxs-lookup"><span data-stu-id="de427-126">For a PowerShell example, see this [PowerShell script](active-directory-reporting-api-audit-samples.md#powershell-script).</span></span>

## <a name="api-endpoint"></a><span data-ttu-id="de427-127">Koncový bod rozhraní API</span><span class="sxs-lookup"><span data-stu-id="de427-127">API Endpoint</span></span>
<span data-ttu-id="de427-128">Toto rozhraní API pomocí hello následující identifikátor URI se můžete dostat:</span><span class="sxs-lookup"><span data-stu-id="de427-128">You can access this API using hello following URI:</span></span>  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta

<span data-ttu-id="de427-129">Neexistuje žádné omezení na hello počet záznamů vrácených API auditu hello Azure AD (pomocí stránkování OData).</span><span class="sxs-lookup"><span data-stu-id="de427-129">There is no limit on hello number of records returned by hello Azure AD audit API (using OData pagination).</span></span>
<span data-ttu-id="de427-130">Pro uchování omezení pro vytváření sestav dat, podívejte se na [Reporting zásady uchovávání informací](active-directory-reporting-retention.md).</span><span class="sxs-lookup"><span data-stu-id="de427-130">For retention limits on reporting data, check out [Reporting Retention Policies](active-directory-reporting-retention.md).</span></span>

<span data-ttu-id="de427-131">Toto volání se vrátí hello data v dávkách.</span><span class="sxs-lookup"><span data-stu-id="de427-131">This call returns hello data in batches.</span></span> <span data-ttu-id="de427-132">Má každé dávky nesmí být delší než 1 000 záznamů.</span><span class="sxs-lookup"><span data-stu-id="de427-132">Each batch has a maximum of 1000 records.</span></span>  
<span data-ttu-id="de427-133">hello další dávku tooget záznamů, použijte odkaz Další hello.</span><span class="sxs-lookup"><span data-stu-id="de427-133">tooget hello next batch of records, use hello Next link.</span></span> <span data-ttu-id="de427-134">Získáte informace o skiptoken hello z první sady hello vrácené záznamy.</span><span class="sxs-lookup"><span data-stu-id="de427-134">Get hello skiptoken information from hello first set of returned records.</span></span> <span data-ttu-id="de427-135">token přeskočit Hello bude na konci hello hello sadu výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="de427-135">hello skip token will be at hello end of hello result set.</span></span>  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta&%24skiptoken=-1339686058




## <a name="supported-filters"></a><span data-ttu-id="de427-136">Podporované filtry</span><span class="sxs-lookup"><span data-stu-id="de427-136">Supported filters</span></span>
<span data-ttu-id="de427-137">Můžete zúžit hello počet záznamů, které se vrátí pomocí rozhraní API volat v podobě filtru.</span><span class="sxs-lookup"><span data-stu-id="de427-137">You can narrow down hello number of records that are returned by an API call in form of a filter.</span></span>  
<span data-ttu-id="de427-138">Pro přihlášení rozhraní API související data, hello následující filtry jsou podporovány:</span><span class="sxs-lookup"><span data-stu-id="de427-138">For sign-in API related data, hello following filters are supported:</span></span>

* <span data-ttu-id="de427-139">**$top =\<počet toobe záznamů vrácených\>**  -toolimit hello počet vrácených záznamů.</span><span class="sxs-lookup"><span data-stu-id="de427-139">**$top=\<number of records toobe returned\>** - toolimit hello number of returned records.</span></span> <span data-ttu-id="de427-140">Toto je náročná operace.</span><span class="sxs-lookup"><span data-stu-id="de427-140">This is an expensive operation.</span></span> <span data-ttu-id="de427-141">Pokud chcete, aby tooreturn tisíc objektů, které byste neměli používat tento filtr.</span><span class="sxs-lookup"><span data-stu-id="de427-141">You should not use this filter if you want tooreturn thousands of objects.</span></span>     
* <span data-ttu-id="de427-142">**$filter =\<údajů filtru\>**  -toospecify na základě hello podporovaný filtr polí, hello typ záznamy, na kterých vám nejvíc záleží</span><span class="sxs-lookup"><span data-stu-id="de427-142">**$filter=\<your filter statement\>** - toospecify, on hello basis of supported filter fields, hello type of records you care about</span></span>

## <a name="supported-filter-fields-and-operators"></a><span data-ttu-id="de427-143">Pole podporovaný filtr a operátory</span><span class="sxs-lookup"><span data-stu-id="de427-143">Supported filter fields and operators</span></span>
<span data-ttu-id="de427-144">toospecify hello typu záznamů, které se zajímáte o, můžete vytvořit filtr příkaz, který může obsahovat jedno nebo kombinaci hello následující pole filtru:</span><span class="sxs-lookup"><span data-stu-id="de427-144">toospecify hello type of records you care about, you can build a filter statement that can contain either one or a combination of hello following filter fields:</span></span>

* <span data-ttu-id="de427-145">[Datum](#activitydate) -definuje datum nebo rozsah dat</span><span class="sxs-lookup"><span data-stu-id="de427-145">[activityDate](#activitydate)  - defines a date or date range</span></span>
* <span data-ttu-id="de427-146">[kategorie](#category) – definuje kategorie hello chcete toofilter na.</span><span class="sxs-lookup"><span data-stu-id="de427-146">[category](#category) - defines hello category you want toofilter on.</span></span>
* <span data-ttu-id="de427-147">[activityStatus](#activitystatus) -definuje hello stav aktivity</span><span class="sxs-lookup"><span data-stu-id="de427-147">[activityStatus](#activitystatus) - defines hello status of an activity</span></span>
* <span data-ttu-id="de427-148">[activityType](#activitytype) -definuje hello typ aktivity</span><span class="sxs-lookup"><span data-stu-id="de427-148">[activityType](#activitytype)  - defines hello type of an activity</span></span>
* <span data-ttu-id="de427-149">[aktivita](#activity) -definuje hello aktivitu jako řetězec</span><span class="sxs-lookup"><span data-stu-id="de427-149">[activity](#activity) - defines hello activity as string</span></span>  
* <span data-ttu-id="de427-150">[objektu actor nebo název](#actorname) -definuje hello objektu actor v podobě hello actor názvu</span><span class="sxs-lookup"><span data-stu-id="de427-150">[actor/name](#actorname) -   defines hello actor in form of hello actor's name</span></span>
* <span data-ttu-id="de427-151">[objektu actor/objectid](#actorobjectid) -definuje hello objektu actor v podobě hello actor ID</span><span class="sxs-lookup"><span data-stu-id="de427-151">[actor/objectid](#actorobjectid) - defines hello actor in form of hello actor's ID</span></span>   
* <span data-ttu-id="de427-152">[objektu actor/upn](#actorupn) -definuje hello objektu actor v podobě hello actor název Princip uživatele (UPN)</span><span class="sxs-lookup"><span data-stu-id="de427-152">[actor/upn](#actorupn)  - defines hello actor in form of hello actor's user principle name (UPN)</span></span> 
* <span data-ttu-id="de427-153">[Cílová nebo](#targetname) -definuje hello cíl v podobě hello actor názvu</span><span class="sxs-lookup"><span data-stu-id="de427-153">[target/name](#targetname)  - defines hello target in form of hello actor's name</span></span>
* <span data-ttu-id="de427-154">[cíl/objectid](#targetobjectid) -definuje hello cíl v podobě ID cíle hello</span><span class="sxs-lookup"><span data-stu-id="de427-154">[target/objectid](#targetobjectid) - defines hello target in form of hello target's ID</span></span>  
* <span data-ttu-id="de427-155">[cíl/upn](#targetupn) -definuje hello objektu actor v podobě hello actor název Princip uživatele (UPN)</span><span class="sxs-lookup"><span data-stu-id="de427-155">[target/upn](#targetupn) - defines hello actor in form of hello actor's user principle name (UPN)</span></span>   

- - -
### <a name="activitydate"></a><span data-ttu-id="de427-156">Datum</span><span class="sxs-lookup"><span data-stu-id="de427-156">activityDate</span></span>
<span data-ttu-id="de427-157">**Podporované operátory**: eq, ge, le, gt, lt</span><span class="sxs-lookup"><span data-stu-id="de427-157">**Supported operators**: eq, ge, le, gt, lt</span></span>

<span data-ttu-id="de427-158">**Příklad**:</span><span class="sxs-lookup"><span data-stu-id="de427-158">**Example**:</span></span>

    $filter=tdomain + 'activities/audit?api-version=beta&`$filter=activityDate gt ' + $7daysago    

<span data-ttu-id="de427-159">**Poznámky k**:</span><span class="sxs-lookup"><span data-stu-id="de427-159">**Notes**:</span></span>

<span data-ttu-id="de427-160">data a času musí být ve formátu UTC</span><span class="sxs-lookup"><span data-stu-id="de427-160">datetime should be in UTC format</span></span>

- - -
### <a name="category"></a><span data-ttu-id="de427-161">category</span><span class="sxs-lookup"><span data-stu-id="de427-161">category</span></span>

<span data-ttu-id="de427-162">**Podporované hodnoty**:</span><span class="sxs-lookup"><span data-stu-id="de427-162">**Supported values**:</span></span>

| <span data-ttu-id="de427-163">Kategorie</span><span class="sxs-lookup"><span data-stu-id="de427-163">Category</span></span>                         | <span data-ttu-id="de427-164">Hodnota</span><span class="sxs-lookup"><span data-stu-id="de427-164">Value</span></span>     |
| :--                              | ---       |
| <span data-ttu-id="de427-165">Základní adresář</span><span class="sxs-lookup"><span data-stu-id="de427-165">Core Directory</span></span>                   | <span data-ttu-id="de427-166">Adresář</span><span class="sxs-lookup"><span data-stu-id="de427-166">Directory</span></span> |
| <span data-ttu-id="de427-167">Samoobslužná správa hesel</span><span class="sxs-lookup"><span data-stu-id="de427-167">Self-service Password Management</span></span> | <span data-ttu-id="de427-168">SSPR</span><span class="sxs-lookup"><span data-stu-id="de427-168">SSPR</span></span>      |
| <span data-ttu-id="de427-169">Samoobslužná správa skupin</span><span class="sxs-lookup"><span data-stu-id="de427-169">Self-service Group Management</span></span>    | <span data-ttu-id="de427-170">SSGM</span><span class="sxs-lookup"><span data-stu-id="de427-170">SSGM</span></span>      |
| <span data-ttu-id="de427-171">Zřizování účtů</span><span class="sxs-lookup"><span data-stu-id="de427-171">Account Provisioning</span></span>             | <span data-ttu-id="de427-172">Sync</span><span class="sxs-lookup"><span data-stu-id="de427-172">Sync</span></span>      |
| <span data-ttu-id="de427-173">Automatická změna hesel</span><span class="sxs-lookup"><span data-stu-id="de427-173">Automated Password Rollover</span></span>      | <span data-ttu-id="de427-174">Automatická změna hesel</span><span class="sxs-lookup"><span data-stu-id="de427-174">Automated Password Rollover</span></span> |
| <span data-ttu-id="de427-175">Identity Protection</span><span class="sxs-lookup"><span data-stu-id="de427-175">Identity Protection</span></span>              | <span data-ttu-id="de427-176">IdentityProtection</span><span class="sxs-lookup"><span data-stu-id="de427-176">IdentityProtection</span></span> |
| <span data-ttu-id="de427-177">Pozvaní uživatelé</span><span class="sxs-lookup"><span data-stu-id="de427-177">Invited Users</span></span>                    | <span data-ttu-id="de427-178">Pozvaní uživatelé</span><span class="sxs-lookup"><span data-stu-id="de427-178">Invited Users</span></span> |
| <span data-ttu-id="de427-179">Služba MIM</span><span class="sxs-lookup"><span data-stu-id="de427-179">MIM Service</span></span>                      | <span data-ttu-id="de427-180">Služba MIM</span><span class="sxs-lookup"><span data-stu-id="de427-180">MIM Service</span></span> |



<span data-ttu-id="de427-181">**Podporované operátory**: eq</span><span class="sxs-lookup"><span data-stu-id="de427-181">**Supported operators**: eq</span></span>

<span data-ttu-id="de427-182">**Příklad**:</span><span class="sxs-lookup"><span data-stu-id="de427-182">**Example**:</span></span>

    $filter=category eq 'SSPR'
- - -
### <a name="activitystatus"></a><span data-ttu-id="de427-183">ActivityStatus</span><span class="sxs-lookup"><span data-stu-id="de427-183">activityStatus</span></span>

<span data-ttu-id="de427-184">**Podporované hodnoty**:</span><span class="sxs-lookup"><span data-stu-id="de427-184">**Supported values**:</span></span>

| <span data-ttu-id="de427-185">Stav aktivity</span><span class="sxs-lookup"><span data-stu-id="de427-185">Activity Status</span></span> | <span data-ttu-id="de427-186">Hodnota</span><span class="sxs-lookup"><span data-stu-id="de427-186">Value</span></span> |
| :--             | ---   |
| <span data-ttu-id="de427-187">Úspěch</span><span class="sxs-lookup"><span data-stu-id="de427-187">Success</span></span>         | <span data-ttu-id="de427-188">0</span><span class="sxs-lookup"><span data-stu-id="de427-188">0</span></span>     |
| <span data-ttu-id="de427-189">Selhání</span><span class="sxs-lookup"><span data-stu-id="de427-189">Failure</span></span>         | <span data-ttu-id="de427-190">- 1</span><span class="sxs-lookup"><span data-stu-id="de427-190">- 1</span></span>   |

<span data-ttu-id="de427-191">**Podporované operátory**: eq</span><span class="sxs-lookup"><span data-stu-id="de427-191">**Supported operators**: eq</span></span>

<span data-ttu-id="de427-192">**Příklad**:</span><span class="sxs-lookup"><span data-stu-id="de427-192">**Example**:</span></span>

    $filter=activityStatus eq -1    

---
### <a name="activitytype"></a><span data-ttu-id="de427-193">activityType</span><span class="sxs-lookup"><span data-stu-id="de427-193">activityType</span></span>
<span data-ttu-id="de427-194">**Podporované operátory**: eq</span><span class="sxs-lookup"><span data-stu-id="de427-194">**Supported operators**: eq</span></span>

<span data-ttu-id="de427-195">**Příklad**:</span><span class="sxs-lookup"><span data-stu-id="de427-195">**Example**:</span></span>

    $filter=activityType eq 'User'    

<span data-ttu-id="de427-196">**Poznámky k**:</span><span class="sxs-lookup"><span data-stu-id="de427-196">**Notes**:</span></span>

<span data-ttu-id="de427-197">malá a velká písmena</span><span class="sxs-lookup"><span data-stu-id="de427-197">case-sensitive</span></span>

- - -
### <a name="activity"></a><span data-ttu-id="de427-198">Aktivity</span><span class="sxs-lookup"><span data-stu-id="de427-198">activity</span></span>
<span data-ttu-id="de427-199">**Podporované operátory**: eq, obsahuje, startsWith</span><span class="sxs-lookup"><span data-stu-id="de427-199">**Supported operators**: eq, contains, startsWith</span></span>

<span data-ttu-id="de427-200">**Příklad**:</span><span class="sxs-lookup"><span data-stu-id="de427-200">**Example**:</span></span>

    $filter=activity eq 'Add application' or contains(activity, 'Application') or startsWith(activity, 'Add')    

<span data-ttu-id="de427-201">**Poznámky k**:</span><span class="sxs-lookup"><span data-stu-id="de427-201">**Notes**:</span></span>

<span data-ttu-id="de427-202">malá a velká písmena</span><span class="sxs-lookup"><span data-stu-id="de427-202">case-sensitive</span></span>

- - -
### <a name="actorname"></a><span data-ttu-id="de427-203">objektu actor nebo název</span><span class="sxs-lookup"><span data-stu-id="de427-203">actor/name</span></span>
<span data-ttu-id="de427-204">**Podporované operátory**: eq, obsahuje, startsWith</span><span class="sxs-lookup"><span data-stu-id="de427-204">**Supported operators**: eq, contains, startsWith</span></span>

<span data-ttu-id="de427-205">**Příklad**:</span><span class="sxs-lookup"><span data-stu-id="de427-205">**Example**:</span></span>

    $filter=actor/name eq 'test' or contains(actor/name, 'test') or startswith(actor/name, 'test')    

<span data-ttu-id="de427-206">**Poznámky k**:</span><span class="sxs-lookup"><span data-stu-id="de427-206">**Notes**:</span></span>

<span data-ttu-id="de427-207">Velká a malá písmena</span><span class="sxs-lookup"><span data-stu-id="de427-207">case-insensitive</span></span>

- - -
### <a name="actorobjectid"></a><span data-ttu-id="de427-208">objektu actor/objectId</span><span class="sxs-lookup"><span data-stu-id="de427-208">actor/objectId</span></span>
<span data-ttu-id="de427-209">**Podporované operátory**: eq</span><span class="sxs-lookup"><span data-stu-id="de427-209">**Supported operators**: eq</span></span>

<span data-ttu-id="de427-210">**Příklad**:</span><span class="sxs-lookup"><span data-stu-id="de427-210">**Example**:</span></span>

    $filter=actor/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba'    


- - -
### <a name="targetname"></a><span data-ttu-id="de427-211">Cílová nebo</span><span class="sxs-lookup"><span data-stu-id="de427-211">target/name</span></span>
<span data-ttu-id="de427-212">**Podporované operátory**: eq, obsahuje, startsWith</span><span class="sxs-lookup"><span data-stu-id="de427-212">**Supported operators**: eq, contains, startsWith</span></span>

<span data-ttu-id="de427-213">**Příklad**:</span><span class="sxs-lookup"><span data-stu-id="de427-213">**Example**:</span></span>

    $filter=targets/any(t: t/name eq 'some name')    

<span data-ttu-id="de427-214">**Poznámky k**:</span><span class="sxs-lookup"><span data-stu-id="de427-214">**Notes**:</span></span>

<span data-ttu-id="de427-215">Velká a malá písmena</span><span class="sxs-lookup"><span data-stu-id="de427-215">Case-insensitive</span></span>

- - -
### <a name="targetupn"></a><span data-ttu-id="de427-216">cíl/upn</span><span class="sxs-lookup"><span data-stu-id="de427-216">target/upn</span></span>
<span data-ttu-id="de427-217">**Podporované operátory**: eq startsWith</span><span class="sxs-lookup"><span data-stu-id="de427-217">**Supported operators**: eq, startsWith</span></span>

<span data-ttu-id="de427-218">**Příklad**:</span><span class="sxs-lookup"><span data-stu-id="de427-218">**Example**:</span></span>

    $filter=targets/any(t: startswith(t/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity/userPrincipalName,'abc'))    

<span data-ttu-id="de427-219">**Poznámky k**:</span><span class="sxs-lookup"><span data-stu-id="de427-219">**Notes**:</span></span>

* <span data-ttu-id="de427-220">Velká a malá písmena</span><span class="sxs-lookup"><span data-stu-id="de427-220">Case-insensitive</span></span>
* <span data-ttu-id="de427-221">Je třeba při dotazování Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity tooadd hello úplný obor názvů</span><span class="sxs-lookup"><span data-stu-id="de427-221">You need tooadd hello full namespace when querying  Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity</span></span>

- - -
### <a name="targetobjectid"></a><span data-ttu-id="de427-222">cíl/objectId</span><span class="sxs-lookup"><span data-stu-id="de427-222">target/objectId</span></span>
<span data-ttu-id="de427-223">**Podporované operátory**: eq</span><span class="sxs-lookup"><span data-stu-id="de427-223">**Supported operators**: eq</span></span>

<span data-ttu-id="de427-224">**Příklad**:</span><span class="sxs-lookup"><span data-stu-id="de427-224">**Example**:</span></span>

    $filter=targets/any(t: t/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba')    

- - -
### <a name="actorupn"></a><span data-ttu-id="de427-225">objektu actor/upn</span><span class="sxs-lookup"><span data-stu-id="de427-225">actor/upn</span></span>
<span data-ttu-id="de427-226">**Podporované operátory**: eq startsWith</span><span class="sxs-lookup"><span data-stu-id="de427-226">**Supported operators**: eq, startsWith</span></span>

<span data-ttu-id="de427-227">**Příklad**:</span><span class="sxs-lookup"><span data-stu-id="de427-227">**Example**:</span></span>

    $filter=startswith(actor/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity/userPrincipalName,'abc')    

<span data-ttu-id="de427-228">**Poznámky k**:</span><span class="sxs-lookup"><span data-stu-id="de427-228">**Notes**:</span></span>

* <span data-ttu-id="de427-229">Velká a malá písmena</span><span class="sxs-lookup"><span data-stu-id="de427-229">Case-insensitive</span></span> 
* <span data-ttu-id="de427-230">Je třeba při dotazování Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity tooadd hello úplný obor názvů</span><span class="sxs-lookup"><span data-stu-id="de427-230">You need tooadd hello full namespace when querying Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity</span></span>

- - -
## <a name="next-steps"></a><span data-ttu-id="de427-231">Další kroky</span><span class="sxs-lookup"><span data-stu-id="de427-231">Next Steps</span></span>
* <span data-ttu-id="de427-232">Chcete pro filtrovaný systému aktivity toosee příklady?</span><span class="sxs-lookup"><span data-stu-id="de427-232">Do you want toosee examples for filtered system activities?</span></span> <span data-ttu-id="de427-233">Podívejte se na hello [ukázky auditu rozhraní API služby Azure Active Directory](active-directory-reporting-api-audit-samples.md).</span><span class="sxs-lookup"><span data-stu-id="de427-233">Check out hello [Azure Active Directory audit API samples](active-directory-reporting-api-audit-samples.md).</span></span>
* <span data-ttu-id="de427-234">Chcete, aby tooknow Další informace o vytváření sestav API hello Azure AD?</span><span class="sxs-lookup"><span data-stu-id="de427-234">Do you want tooknow more about hello Azure AD reporting API?</span></span> <span data-ttu-id="de427-235">V tématu [Začínáme s Azure Active Directory Reporting API hello](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="de427-235">See [Getting started with hello Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).</span></span>

