---
title: "Azure Active Directory auditu referenční dokumentace rozhraní API | Microsoft Docs"
description: "Jak začít pracovat s rozhraním API auditování Azure Active Directory"
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
ms.openlocfilehash: 573e940c5390e7b990d889681eb37b73c5b253d9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-audit-api-reference"></a><span data-ttu-id="ec199-103">Azure Active Directory auditu referenční dokumentace rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ec199-103">Azure Active Directory audit API reference</span></span>
<span data-ttu-id="ec199-104">Toto téma je součástí kolekce témat o službě Azure Active Directory, vytváření sestav rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="ec199-104">This topic is part of a collection of topics about the Azure Active Directory reporting API.</span></span>  
<span data-ttu-id="ec199-105">Generování sestav služby Azure AD poskytuje rozhraní API, která umožňuje přístup k datům auditování pomocí kódu nebo související nástroje.</span><span class="sxs-lookup"><span data-stu-id="ec199-105">Azure AD reporting provides you with an API that enables you to access audit data using code or related tools.</span></span>
<span data-ttu-id="ec199-106">Obor tohoto tématu je k poskytování referenční informace o **audit rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="ec199-106">The scope of this topic is to provide you with reference information about the **audit API**.</span></span>

<span data-ttu-id="ec199-107">Přejděte na téma:</span><span class="sxs-lookup"><span data-stu-id="ec199-107">See:</span></span>

* <span data-ttu-id="ec199-108">[Protokoly auditu](active-directory-reporting-azure-portal.md#activity-reports) další koncepční informace</span><span class="sxs-lookup"><span data-stu-id="ec199-108">[Audit logs](active-directory-reporting-azure-portal.md#activity-reports)  for more conceptual information</span></span>

* <span data-ttu-id="ec199-109">[Začínáme s Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md) Další informace o rozhraní API pro generování sestav.</span><span class="sxs-lookup"><span data-stu-id="ec199-109">[Getting started with the Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md) for more information about the reporting API.</span></span>


<span data-ttu-id="ec199-110">Pro:</span><span class="sxs-lookup"><span data-stu-id="ec199-110">For:</span></span>

- <span data-ttu-id="ec199-111">Nejčastější dotazy, přečtěte si naše [– nejčastější dotazy](active-directory-reporting-faq.md)</span><span class="sxs-lookup"><span data-stu-id="ec199-111">Frequently asked questions, read our [FAQ](active-directory-reporting-faq.md)</span></span> 

- <span data-ttu-id="ec199-112">Problémy prosím [souboru lístek podpory](active-directory-troubleshooting-support-howto.md)</span><span class="sxs-lookup"><span data-stu-id="ec199-112">Issues, please [file a support ticket](active-directory-troubleshooting-support-howto.md)</span></span> 


## <a name="who-can-access-the-data"></a><span data-ttu-id="ec199-113">Kdo má přístup k datům?</span><span class="sxs-lookup"><span data-stu-id="ec199-113">Who can access the data?</span></span>
* <span data-ttu-id="ec199-114">Uživatelé v roli Správce zabezpečení nebo Čtenář zabezpečení</span><span class="sxs-lookup"><span data-stu-id="ec199-114">Users in the Security Admin or Security Reader role</span></span>
* <span data-ttu-id="ec199-115">Globální správci</span><span class="sxs-lookup"><span data-stu-id="ec199-115">Global Admins</span></span>
* <span data-ttu-id="ec199-116">Jakékoli aplikaci, která má oprávnění pro přístup k rozhraní API (autorizace služby app lze pouze na základě oprávnění globálního správce)</span><span class="sxs-lookup"><span data-stu-id="ec199-116">Any app that has authorization to access the API (app authorization can be setup only based on Global Admin’s permission)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ec199-117">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ec199-117">Prerequisites</span></span>
<span data-ttu-id="ec199-118">Chcete-li získat přístup k této sestavě prostřednictvím rozhraní API pro vytváření sestav, musíte mít:</span><span class="sxs-lookup"><span data-stu-id="ec199-118">In order to access this report through the Reporting API, you must have:</span></span>

* <span data-ttu-id="ec199-119">[Lepší edici nebo Azure Active Directory volné](active-directory-editions.md)</span><span class="sxs-lookup"><span data-stu-id="ec199-119">An [Azure Active Directory Free or better edition](active-directory-editions.md)</span></span>
* <span data-ttu-id="ec199-120">Byla dokončena [požadavky pro přístup k Azure AD reporting rozhraní API](active-directory-reporting-api-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="ec199-120">Completed the [prerequisites to access the Azure AD reporting API](active-directory-reporting-api-prerequisites.md).</span></span> 

## <a name="accessing-the-api"></a><span data-ttu-id="ec199-121">Přístup k rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ec199-121">Accessing the API</span></span>
<span data-ttu-id="ec199-122">Můžete buď přístup toto rozhraní API prostřednictvím [Explorer grafu](https://graphexplorer2.cloudapp.net) nebo prostřednictvím kódu programu, například pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ec199-122">You can either access this API through the [Graph Explorer](https://graphexplorer2.cloudapp.net) or programmatically using, for example, PowerShell.</span></span> <span data-ttu-id="ec199-123">Aby PowerShell správně interpretovat syntaxe filtru OData, který se používá při voláních REST grafu AAD, je nutné použít backtick (neboli: čárka) znak "řídicí" znak $.</span><span class="sxs-lookup"><span data-stu-id="ec199-123">In order for PowerShell to correctly interpret the OData filter syntax used in AAD Graph REST calls, you must use the backtick (aka: grave accent) character to “escape” the $ character.</span></span> <span data-ttu-id="ec199-124">Backtick znak, který slouží jako [Powershellu řídicí znak](https://technet.microsoft.com/library/hh847755.aspx), povolení prostředí PowerShell provést literálu výklad znak $, a zamezit tak složitá jako název proměnné prostředí PowerShell (ie: $filter).</span><span class="sxs-lookup"><span data-stu-id="ec199-124">The backtick character serves as [PowerShell’s escape character](https://technet.microsoft.com/library/hh847755.aspx), allowing PowerShell to do a literal interpretation of the $ character, and avoid confusing it as a PowerShell variable name (ie: $filter).</span></span>

<span data-ttu-id="ec199-125">Graf Explorer je aktivní v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="ec199-125">The focus of this topic is on the Graph Explorer.</span></span> <span data-ttu-id="ec199-126">V příkladu prostředí PowerShell najdete [skript prostředí PowerShell](active-directory-reporting-api-audit-samples.md#powershell-script).</span><span class="sxs-lookup"><span data-stu-id="ec199-126">For a PowerShell example, see this [PowerShell script](active-directory-reporting-api-audit-samples.md#powershell-script).</span></span>

## <a name="api-endpoint"></a><span data-ttu-id="ec199-127">Koncový bod rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ec199-127">API Endpoint</span></span>
<span data-ttu-id="ec199-128">Toto rozhraní API pomocí následující identifikátor URI se můžete dostat:</span><span class="sxs-lookup"><span data-stu-id="ec199-128">You can access this API using the following URI:</span></span>  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta

<span data-ttu-id="ec199-129">Neexistuje žádné omezení počtu záznamů vrácených rozhraní API auditu Azure AD (pomocí stránkování OData).</span><span class="sxs-lookup"><span data-stu-id="ec199-129">There is no limit on the number of records returned by the Azure AD audit API (using OData pagination).</span></span>
<span data-ttu-id="ec199-130">Pro uchování omezení pro vytváření sestav dat, podívejte se na [Reporting zásady uchovávání informací](active-directory-reporting-retention.md).</span><span class="sxs-lookup"><span data-stu-id="ec199-130">For retention limits on reporting data, check out [Reporting Retention Policies](active-directory-reporting-retention.md).</span></span>

<span data-ttu-id="ec199-131">Toto volání se vrátí data v dávkách.</span><span class="sxs-lookup"><span data-stu-id="ec199-131">This call returns the data in batches.</span></span> <span data-ttu-id="ec199-132">Má každé dávky nesmí být delší než 1 000 záznamů.</span><span class="sxs-lookup"><span data-stu-id="ec199-132">Each batch has a maximum of 1000 records.</span></span>  
<span data-ttu-id="ec199-133">Chcete-li získat další dávky záznamů, použijte odkaz na další.</span><span class="sxs-lookup"><span data-stu-id="ec199-133">To get the next batch of records, use the Next link.</span></span> <span data-ttu-id="ec199-134">Získáte informace o skiptoken z první sady vrácené záznamy.</span><span class="sxs-lookup"><span data-stu-id="ec199-134">Get the skiptoken information from the first set of returned records.</span></span> <span data-ttu-id="ec199-135">Token přeskočit bude na konci výsledek nastaveno.</span><span class="sxs-lookup"><span data-stu-id="ec199-135">The skip token will be at the end of the result set.</span></span>  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta&%24skiptoken=-1339686058




## <a name="supported-filters"></a><span data-ttu-id="ec199-136">Podporované filtry</span><span class="sxs-lookup"><span data-stu-id="ec199-136">Supported filters</span></span>
<span data-ttu-id="ec199-137">Počet záznamů, které se vrátí pomocí rozhraní API můžete zúžit volání v podobě filtru.</span><span class="sxs-lookup"><span data-stu-id="ec199-137">You can narrow down the number of records that are returned by an API call in form of a filter.</span></span>  
<span data-ttu-id="ec199-138">Pro přihlášení rozhraní API související data, jsou podporovány následující filtry:</span><span class="sxs-lookup"><span data-stu-id="ec199-138">For sign-in API related data, the following filters are supported:</span></span>

* <span data-ttu-id="ec199-139">**$top =\<počet vrácených\>**  – Pokud chcete omezit počet vrácených záznamů.</span><span class="sxs-lookup"><span data-stu-id="ec199-139">**$top=\<number of records to be returned\>** - to limit the number of returned records.</span></span> <span data-ttu-id="ec199-140">Toto je náročná operace.</span><span class="sxs-lookup"><span data-stu-id="ec199-140">This is an expensive operation.</span></span> <span data-ttu-id="ec199-141">Tento filtr byste neměli používat, pokud chcete vrátit tisíce objektů.</span><span class="sxs-lookup"><span data-stu-id="ec199-141">You should not use this filter if you want to return thousands of objects.</span></span>     
* <span data-ttu-id="ec199-142">**$filter =\<údajů filtru\>**  – Pokud chcete zadat typ záznamy, na kterých vám nejvíc záleží na základě pole podporovaný filtr</span><span class="sxs-lookup"><span data-stu-id="ec199-142">**$filter=\<your filter statement\>** - to specify, on the basis of supported filter fields, the type of records you care about</span></span>

## <a name="supported-filter-fields-and-operators"></a><span data-ttu-id="ec199-143">Pole podporovaný filtr a operátory</span><span class="sxs-lookup"><span data-stu-id="ec199-143">Supported filter fields and operators</span></span>
<span data-ttu-id="ec199-144">Pokud chcete zadat typ záznamů, které se zajímáte o, můžete vytvořit filtr příkaz, který může obsahovat jedno nebo kombinaci následující pole filtru:</span><span class="sxs-lookup"><span data-stu-id="ec199-144">To specify the type of records you care about, you can build a filter statement that can contain either one or a combination of the following filter fields:</span></span>

* <span data-ttu-id="ec199-145">[Datum](#activitydate) -definuje datum nebo rozsah dat</span><span class="sxs-lookup"><span data-stu-id="ec199-145">[activityDate](#activitydate)  - defines a date or date range</span></span>
* <span data-ttu-id="ec199-146">[kategorie](#category) – definuje kategorie, kterou chcete filtrovat.</span><span class="sxs-lookup"><span data-stu-id="ec199-146">[category](#category) - defines the category you want to filter on.</span></span>
* <span data-ttu-id="ec199-147">[activityStatus](#activitystatus) -definuje stav aktivity</span><span class="sxs-lookup"><span data-stu-id="ec199-147">[activityStatus](#activitystatus) - defines the status of an activity</span></span>
* <span data-ttu-id="ec199-148">[activityType](#activitytype) -definuje typ aktivity</span><span class="sxs-lookup"><span data-stu-id="ec199-148">[activityType](#activitytype)  - defines the type of an activity</span></span>
* <span data-ttu-id="ec199-149">[aktivita](#activity) -definuje aktivitu jako řetězec</span><span class="sxs-lookup"><span data-stu-id="ec199-149">[activity](#activity) - defines the activity as string</span></span>  
* <span data-ttu-id="ec199-150">[objektu actor nebo název](#actorname) -definuje objektu actor tvar názvu objektu actor</span><span class="sxs-lookup"><span data-stu-id="ec199-150">[actor/name](#actorname) -   defines the actor in form of the actor's name</span></span>
* <span data-ttu-id="ec199-151">[objektu actor/objectid](#actorobjectid) -definuje objektu actor v podobě ID objektu actor</span><span class="sxs-lookup"><span data-stu-id="ec199-151">[actor/objectid](#actorobjectid) - defines the actor in form of the actor's ID</span></span>   
* <span data-ttu-id="ec199-152">[objektu actor/upn](#actorupn) -definuje objektu actor v podobě název Princip objektu actor uživatele (UPN)</span><span class="sxs-lookup"><span data-stu-id="ec199-152">[actor/upn](#actorupn)  - defines the actor in form of the actor's user principle name (UPN)</span></span> 
* <span data-ttu-id="ec199-153">[Cílová nebo](#targetname) -definuje cílový tvar názvu objektu actor</span><span class="sxs-lookup"><span data-stu-id="ec199-153">[target/name](#targetname)  - defines the target in form of the actor's name</span></span>
* <span data-ttu-id="ec199-154">[cíl/objectid](#targetobjectid) -definuje cíl v podobě ID cílové složky</span><span class="sxs-lookup"><span data-stu-id="ec199-154">[target/objectid](#targetobjectid) - defines the target in form of the target's ID</span></span>  
* <span data-ttu-id="ec199-155">[cíl/upn](#targetupn) -definuje objektu actor v podobě název Princip objektu actor uživatele (UPN)</span><span class="sxs-lookup"><span data-stu-id="ec199-155">[target/upn](#targetupn) - defines the actor in form of the actor's user principle name (UPN)</span></span>   

- - -
### <a name="activitydate"></a><span data-ttu-id="ec199-156">Datum</span><span class="sxs-lookup"><span data-stu-id="ec199-156">activityDate</span></span>
<span data-ttu-id="ec199-157">**Podporované operátory**: eq, ge, le, gt, lt</span><span class="sxs-lookup"><span data-stu-id="ec199-157">**Supported operators**: eq, ge, le, gt, lt</span></span>

<span data-ttu-id="ec199-158">**Příklad**:</span><span class="sxs-lookup"><span data-stu-id="ec199-158">**Example**:</span></span>

    $filter=tdomain + 'activities/audit?api-version=beta&`$filter=activityDate gt ' + $7daysago    

<span data-ttu-id="ec199-159">**Poznámky k**:</span><span class="sxs-lookup"><span data-stu-id="ec199-159">**Notes**:</span></span>

<span data-ttu-id="ec199-160">data a času musí být ve formátu UTC</span><span class="sxs-lookup"><span data-stu-id="ec199-160">datetime should be in UTC format</span></span>

- - -
### <a name="category"></a><span data-ttu-id="ec199-161">category</span><span class="sxs-lookup"><span data-stu-id="ec199-161">category</span></span>

<span data-ttu-id="ec199-162">**Podporované hodnoty**:</span><span class="sxs-lookup"><span data-stu-id="ec199-162">**Supported values**:</span></span>

| <span data-ttu-id="ec199-163">Kategorie</span><span class="sxs-lookup"><span data-stu-id="ec199-163">Category</span></span>                         | <span data-ttu-id="ec199-164">Hodnota</span><span class="sxs-lookup"><span data-stu-id="ec199-164">Value</span></span>     |
| :--                              | ---       |
| <span data-ttu-id="ec199-165">Základní adresář</span><span class="sxs-lookup"><span data-stu-id="ec199-165">Core Directory</span></span>                   | <span data-ttu-id="ec199-166">Adresář</span><span class="sxs-lookup"><span data-stu-id="ec199-166">Directory</span></span> |
| <span data-ttu-id="ec199-167">Samoobslužná správa hesel</span><span class="sxs-lookup"><span data-stu-id="ec199-167">Self-service Password Management</span></span> | <span data-ttu-id="ec199-168">SSPR</span><span class="sxs-lookup"><span data-stu-id="ec199-168">SSPR</span></span>      |
| <span data-ttu-id="ec199-169">Samoobslužná správa skupin</span><span class="sxs-lookup"><span data-stu-id="ec199-169">Self-service Group Management</span></span>    | <span data-ttu-id="ec199-170">SSGM</span><span class="sxs-lookup"><span data-stu-id="ec199-170">SSGM</span></span>      |
| <span data-ttu-id="ec199-171">Zřizování účtů</span><span class="sxs-lookup"><span data-stu-id="ec199-171">Account Provisioning</span></span>             | <span data-ttu-id="ec199-172">Sync</span><span class="sxs-lookup"><span data-stu-id="ec199-172">Sync</span></span>      |
| <span data-ttu-id="ec199-173">Automatická změna hesel</span><span class="sxs-lookup"><span data-stu-id="ec199-173">Automated Password Rollover</span></span>      | <span data-ttu-id="ec199-174">Automatická změna hesel</span><span class="sxs-lookup"><span data-stu-id="ec199-174">Automated Password Rollover</span></span> |
| <span data-ttu-id="ec199-175">Identity Protection</span><span class="sxs-lookup"><span data-stu-id="ec199-175">Identity Protection</span></span>              | <span data-ttu-id="ec199-176">IdentityProtection</span><span class="sxs-lookup"><span data-stu-id="ec199-176">IdentityProtection</span></span> |
| <span data-ttu-id="ec199-177">Pozvaní uživatelé</span><span class="sxs-lookup"><span data-stu-id="ec199-177">Invited Users</span></span>                    | <span data-ttu-id="ec199-178">Pozvaní uživatelé</span><span class="sxs-lookup"><span data-stu-id="ec199-178">Invited Users</span></span> |
| <span data-ttu-id="ec199-179">Služba MIM</span><span class="sxs-lookup"><span data-stu-id="ec199-179">MIM Service</span></span>                      | <span data-ttu-id="ec199-180">Služba MIM</span><span class="sxs-lookup"><span data-stu-id="ec199-180">MIM Service</span></span> |



<span data-ttu-id="ec199-181">**Podporované operátory**: eq</span><span class="sxs-lookup"><span data-stu-id="ec199-181">**Supported operators**: eq</span></span>

<span data-ttu-id="ec199-182">**Příklad**:</span><span class="sxs-lookup"><span data-stu-id="ec199-182">**Example**:</span></span>

    $filter=category eq 'SSPR'
- - -
### <a name="activitystatus"></a><span data-ttu-id="ec199-183">ActivityStatus</span><span class="sxs-lookup"><span data-stu-id="ec199-183">activityStatus</span></span>

<span data-ttu-id="ec199-184">**Podporované hodnoty**:</span><span class="sxs-lookup"><span data-stu-id="ec199-184">**Supported values**:</span></span>

| <span data-ttu-id="ec199-185">Stav aktivity</span><span class="sxs-lookup"><span data-stu-id="ec199-185">Activity Status</span></span> | <span data-ttu-id="ec199-186">Hodnota</span><span class="sxs-lookup"><span data-stu-id="ec199-186">Value</span></span> |
| :--             | ---   |
| <span data-ttu-id="ec199-187">Úspěch</span><span class="sxs-lookup"><span data-stu-id="ec199-187">Success</span></span>         | <span data-ttu-id="ec199-188">0</span><span class="sxs-lookup"><span data-stu-id="ec199-188">0</span></span>     |
| <span data-ttu-id="ec199-189">Selhání</span><span class="sxs-lookup"><span data-stu-id="ec199-189">Failure</span></span>         | <span data-ttu-id="ec199-190">- 1</span><span class="sxs-lookup"><span data-stu-id="ec199-190">- 1</span></span>   |

<span data-ttu-id="ec199-191">**Podporované operátory**: eq</span><span class="sxs-lookup"><span data-stu-id="ec199-191">**Supported operators**: eq</span></span>

<span data-ttu-id="ec199-192">**Příklad**:</span><span class="sxs-lookup"><span data-stu-id="ec199-192">**Example**:</span></span>

    $filter=activityStatus eq -1    

---
### <a name="activitytype"></a><span data-ttu-id="ec199-193">activityType</span><span class="sxs-lookup"><span data-stu-id="ec199-193">activityType</span></span>
<span data-ttu-id="ec199-194">**Podporované operátory**: eq</span><span class="sxs-lookup"><span data-stu-id="ec199-194">**Supported operators**: eq</span></span>

<span data-ttu-id="ec199-195">**Příklad**:</span><span class="sxs-lookup"><span data-stu-id="ec199-195">**Example**:</span></span>

    $filter=activityType eq 'User'    

<span data-ttu-id="ec199-196">**Poznámky k**:</span><span class="sxs-lookup"><span data-stu-id="ec199-196">**Notes**:</span></span>

<span data-ttu-id="ec199-197">malá a velká písmena</span><span class="sxs-lookup"><span data-stu-id="ec199-197">case-sensitive</span></span>

- - -
### <a name="activity"></a><span data-ttu-id="ec199-198">Aktivity</span><span class="sxs-lookup"><span data-stu-id="ec199-198">activity</span></span>
<span data-ttu-id="ec199-199">**Podporované operátory**: eq, obsahuje, startsWith</span><span class="sxs-lookup"><span data-stu-id="ec199-199">**Supported operators**: eq, contains, startsWith</span></span>

<span data-ttu-id="ec199-200">**Příklad**:</span><span class="sxs-lookup"><span data-stu-id="ec199-200">**Example**:</span></span>

    $filter=activity eq 'Add application' or contains(activity, 'Application') or startsWith(activity, 'Add')    

<span data-ttu-id="ec199-201">**Poznámky k**:</span><span class="sxs-lookup"><span data-stu-id="ec199-201">**Notes**:</span></span>

<span data-ttu-id="ec199-202">malá a velká písmena</span><span class="sxs-lookup"><span data-stu-id="ec199-202">case-sensitive</span></span>

- - -
### <a name="actorname"></a><span data-ttu-id="ec199-203">objektu actor nebo název</span><span class="sxs-lookup"><span data-stu-id="ec199-203">actor/name</span></span>
<span data-ttu-id="ec199-204">**Podporované operátory**: eq, obsahuje, startsWith</span><span class="sxs-lookup"><span data-stu-id="ec199-204">**Supported operators**: eq, contains, startsWith</span></span>

<span data-ttu-id="ec199-205">**Příklad**:</span><span class="sxs-lookup"><span data-stu-id="ec199-205">**Example**:</span></span>

    $filter=actor/name eq 'test' or contains(actor/name, 'test') or startswith(actor/name, 'test')    

<span data-ttu-id="ec199-206">**Poznámky k**:</span><span class="sxs-lookup"><span data-stu-id="ec199-206">**Notes**:</span></span>

<span data-ttu-id="ec199-207">Velká a malá písmena</span><span class="sxs-lookup"><span data-stu-id="ec199-207">case-insensitive</span></span>

- - -
### <a name="actorobjectid"></a><span data-ttu-id="ec199-208">objektu actor/objectId</span><span class="sxs-lookup"><span data-stu-id="ec199-208">actor/objectId</span></span>
<span data-ttu-id="ec199-209">**Podporované operátory**: eq</span><span class="sxs-lookup"><span data-stu-id="ec199-209">**Supported operators**: eq</span></span>

<span data-ttu-id="ec199-210">**Příklad**:</span><span class="sxs-lookup"><span data-stu-id="ec199-210">**Example**:</span></span>

    $filter=actor/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba'    


- - -
### <a name="targetname"></a><span data-ttu-id="ec199-211">Cílová nebo</span><span class="sxs-lookup"><span data-stu-id="ec199-211">target/name</span></span>
<span data-ttu-id="ec199-212">**Podporované operátory**: eq, obsahuje, startsWith</span><span class="sxs-lookup"><span data-stu-id="ec199-212">**Supported operators**: eq, contains, startsWith</span></span>

<span data-ttu-id="ec199-213">**Příklad**:</span><span class="sxs-lookup"><span data-stu-id="ec199-213">**Example**:</span></span>

    $filter=targets/any(t: t/name eq 'some name')    

<span data-ttu-id="ec199-214">**Poznámky k**:</span><span class="sxs-lookup"><span data-stu-id="ec199-214">**Notes**:</span></span>

<span data-ttu-id="ec199-215">Velká a malá písmena</span><span class="sxs-lookup"><span data-stu-id="ec199-215">Case-insensitive</span></span>

- - -
### <a name="targetupn"></a><span data-ttu-id="ec199-216">cíl/upn</span><span class="sxs-lookup"><span data-stu-id="ec199-216">target/upn</span></span>
<span data-ttu-id="ec199-217">**Podporované operátory**: eq startsWith</span><span class="sxs-lookup"><span data-stu-id="ec199-217">**Supported operators**: eq, startsWith</span></span>

<span data-ttu-id="ec199-218">**Příklad**:</span><span class="sxs-lookup"><span data-stu-id="ec199-218">**Example**:</span></span>

    $filter=targets/any(t: startswith(t/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity/userPrincipalName,'abc'))    

<span data-ttu-id="ec199-219">**Poznámky k**:</span><span class="sxs-lookup"><span data-stu-id="ec199-219">**Notes**:</span></span>

* <span data-ttu-id="ec199-220">Velká a malá písmena</span><span class="sxs-lookup"><span data-stu-id="ec199-220">Case-insensitive</span></span>
* <span data-ttu-id="ec199-221">Je nutné přidat úplný obor názvů při dotazování Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity</span><span class="sxs-lookup"><span data-stu-id="ec199-221">You need to add the full namespace when querying  Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity</span></span>

- - -
### <a name="targetobjectid"></a><span data-ttu-id="ec199-222">cíl/objectId</span><span class="sxs-lookup"><span data-stu-id="ec199-222">target/objectId</span></span>
<span data-ttu-id="ec199-223">**Podporované operátory**: eq</span><span class="sxs-lookup"><span data-stu-id="ec199-223">**Supported operators**: eq</span></span>

<span data-ttu-id="ec199-224">**Příklad**:</span><span class="sxs-lookup"><span data-stu-id="ec199-224">**Example**:</span></span>

    $filter=targets/any(t: t/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba')    

- - -
### <a name="actorupn"></a><span data-ttu-id="ec199-225">objektu actor/upn</span><span class="sxs-lookup"><span data-stu-id="ec199-225">actor/upn</span></span>
<span data-ttu-id="ec199-226">**Podporované operátory**: eq startsWith</span><span class="sxs-lookup"><span data-stu-id="ec199-226">**Supported operators**: eq, startsWith</span></span>

<span data-ttu-id="ec199-227">**Příklad**:</span><span class="sxs-lookup"><span data-stu-id="ec199-227">**Example**:</span></span>

    $filter=startswith(actor/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity/userPrincipalName,'abc')    

<span data-ttu-id="ec199-228">**Poznámky k**:</span><span class="sxs-lookup"><span data-stu-id="ec199-228">**Notes**:</span></span>

* <span data-ttu-id="ec199-229">Velká a malá písmena</span><span class="sxs-lookup"><span data-stu-id="ec199-229">Case-insensitive</span></span> 
* <span data-ttu-id="ec199-230">Je nutné přidat úplný obor názvů při dotazování Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity</span><span class="sxs-lookup"><span data-stu-id="ec199-230">You need to add the full namespace when querying Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity</span></span>

- - -
## <a name="next-steps"></a><span data-ttu-id="ec199-231">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ec199-231">Next Steps</span></span>
* <span data-ttu-id="ec199-232">Chcete příklady pro filtrovaný systému aktivity?</span><span class="sxs-lookup"><span data-stu-id="ec199-232">Do you want to see examples for filtered system activities?</span></span> <span data-ttu-id="ec199-233">Podívejte se [ukázky auditu rozhraní API služby Azure Active Directory](active-directory-reporting-api-audit-samples.md).</span><span class="sxs-lookup"><span data-stu-id="ec199-233">Check out the [Azure Active Directory audit API samples](active-directory-reporting-api-audit-samples.md).</span></span>
* <span data-ttu-id="ec199-234">Opravdu chcete získat další informace o generování sestav rozhraní API Azure AD?</span><span class="sxs-lookup"><span data-stu-id="ec199-234">Do you want to know more about the Azure AD reporting API?</span></span> <span data-ttu-id="ec199-235">V tématu [Začínáme s Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="ec199-235">See [Getting started with the Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).</span></span>

