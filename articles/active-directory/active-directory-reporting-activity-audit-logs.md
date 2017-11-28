---
title: "Aktivita aaaAudit sestavy v portálu Azure Active Directory hello | Microsoft Docs"
description: "Úvod toohello audit sestavy aktivit v portálu Azure Active Directory hello"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: a1f93126-77d1-4345-ab7d-561066041161
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/19/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 1567673f5030fc707b017c069f2ba7587962e5cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="audit-activity-reports-in-hello-azure-active-directory-portal"></a><span data-ttu-id="c0fe2-103">Audit aktivity sestav na portálu Azure Active Directory hello</span><span class="sxs-lookup"><span data-stu-id="c0fe2-103">Audit activity reports in hello Azure Active Directory portal</span></span> 

<span data-ttu-id="c0fe2-104">S vytvářením sestav v Azure Active Directory (Azure AD), můžete získat hello informace, které potřebujete toodetermine úspěšnost prostředí.</span><span class="sxs-lookup"><span data-stu-id="c0fe2-104">With reporting in Azure Active Directory (Azure AD), you can get hello information you need toodetermine how your environment is doing.</span></span>

<span data-ttu-id="c0fe2-105">Hello reporting architektura ve službě Azure AD se skládá z hello následující součásti:</span><span class="sxs-lookup"><span data-stu-id="c0fe2-105">hello reporting architecture in Azure AD consists of hello following components:</span></span>

- <span data-ttu-id="c0fe2-106">**Aktivita**</span><span class="sxs-lookup"><span data-stu-id="c0fe2-106">**Activity**</span></span> 
    - <span data-ttu-id="c0fe2-107">**Přihlašovací aktivity** – informace o využití hello spravovaných aplikací a aktivit přihlášení uživatelů</span><span class="sxs-lookup"><span data-stu-id="c0fe2-107">**Sign-in activities** – Information about hello usage of managed applications and user sign-in activities</span></span>
    - <span data-ttu-id="c0fe2-108">**Protokoly auditu** – informace aktivit systému o správě uživatelů a skupin, spravovaných aplikacích a aktivitách adresářů</span><span class="sxs-lookup"><span data-stu-id="c0fe2-108">**Audit logs** - System activity information about users and group management, your managed applications and directory activities.</span></span>
- <span data-ttu-id="c0fe2-109">**Zabezpečení**</span><span class="sxs-lookup"><span data-stu-id="c0fe2-109">**Security**</span></span> 
    - <span data-ttu-id="c0fe2-110">**Rizikové přihlášení** -rizikové přihlášení je indikátorem pro pokusu přihlášení, který se možná prováděly uživatelem, který není vlastníkem legitimní hello uživatelského účtu.</span><span class="sxs-lookup"><span data-stu-id="c0fe2-110">**Risky sign-ins** - A risky sign-in is an indicator for a sign-in attempt that might have been performed by someone who is not hello legitimate owner of a user account.</span></span> <span data-ttu-id="c0fe2-111">Další podrobnosti najdete v tématu Riziková přihlášení.</span><span class="sxs-lookup"><span data-stu-id="c0fe2-111">For more details, see Risky sign-ins.</span></span>
    - <span data-ttu-id="c0fe2-112">**Uživatelé označení příznakem rizika** – Rizikový uživatel je indikátorem uživatelského účtu, který mohl být ohrožený.</span><span class="sxs-lookup"><span data-stu-id="c0fe2-112">**Users flagged for risk** - A risky user is an indicator for a user account that might have been compromised.</span></span> <span data-ttu-id="c0fe2-113">Další podrobnosti najdete v tématu Uživatelé označení příznakem rizika.</span><span class="sxs-lookup"><span data-stu-id="c0fe2-113">For more details, see Users flagged for risk.</span></span>

<span data-ttu-id="c0fe2-114">Toto téma poskytuje přehled hello audit aktivity.</span><span class="sxs-lookup"><span data-stu-id="c0fe2-114">This topic gives you an overview of hello audit activities.</span></span>
 
## <a name="who-can-access-hello-data"></a><span data-ttu-id="c0fe2-115">Kdo může přistupovat k datům hello?</span><span class="sxs-lookup"><span data-stu-id="c0fe2-115">Who can access hello data?</span></span>
* <span data-ttu-id="c0fe2-116">Uživatelé v roli správce zabezpečení nebo zabezpečení čtečky hello</span><span class="sxs-lookup"><span data-stu-id="c0fe2-116">Users in hello Security Admin or Security Reader role</span></span>
* <span data-ttu-id="c0fe2-117">Globální správci</span><span class="sxs-lookup"><span data-stu-id="c0fe2-117">Global Admins</span></span>
* <span data-ttu-id="c0fe2-118">Jednotliví uživatelé (bez oprávnění správce) mohou zobrazit své vlastní aktivity.</span><span class="sxs-lookup"><span data-stu-id="c0fe2-118">Individual users (non-admins) can see their own activities</span></span>


## <a name="audit-logs"></a><span data-ttu-id="c0fe2-119">Protokoly auditu</span><span class="sxs-lookup"><span data-stu-id="c0fe2-119">Audit logs</span></span>

<span data-ttu-id="c0fe2-120">protokoly auditu Hello v Azure Active Directory zadejte záznamů aktivit systému pro dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="c0fe2-120">hello audit logs in Azure Active Directory provide records of system activities for compliance.</span></span>  
<span data-ttu-id="c0fe2-121">Je vaší první tooall bodu položka auditování dat **protokoly auditu** v hello **aktivity** části **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="c0fe2-121">Your first entry point tooall auditing data is **Audit logs** in hello **Activity** section of **Azure Active Directory**.</span></span>

<span data-ttu-id="c0fe2-122">![Protokoly auditu](./media/active-directory-reporting-activity-audit-logs/61.png "Protokoly auditu")</span><span class="sxs-lookup"><span data-stu-id="c0fe2-122">![Audit logs](./media/active-directory-reporting-activity-audit-logs/61.png "Audit logs")</span></span>

<span data-ttu-id="c0fe2-123">Protokol auditu má výchozí zobrazení seznamu, které obsahuje následující položky:</span><span class="sxs-lookup"><span data-stu-id="c0fe2-123">An audit log has a default list view that shows:</span></span>

- <span data-ttu-id="c0fe2-124">Hello datum a čas výskytu hello</span><span class="sxs-lookup"><span data-stu-id="c0fe2-124">hello date and time of hello occurrence</span></span>
- <span data-ttu-id="c0fe2-125">Hello iniciátor nebo objektu actor (*kdo*) aktivity</span><span class="sxs-lookup"><span data-stu-id="c0fe2-125">hello initiator / actor (*who*) of an activity</span></span> 
- <span data-ttu-id="c0fe2-126">Hello aktivity (*co*)</span><span class="sxs-lookup"><span data-stu-id="c0fe2-126">hello activity (*what*)</span></span> 
- <span data-ttu-id="c0fe2-127">cíl Hello</span><span class="sxs-lookup"><span data-stu-id="c0fe2-127">hello target</span></span>

<span data-ttu-id="c0fe2-128">![Protokoly auditu](./media/active-directory-reporting-activity-audit-logs/18.png "Protokoly auditu")</span><span class="sxs-lookup"><span data-stu-id="c0fe2-128">![Audit logs](./media/active-directory-reporting-activity-audit-logs/18.png "Audit logs")</span></span>

<span data-ttu-id="c0fe2-129">Kliknutím můžete přizpůsobit zobrazení seznamu hello **sloupce** v panelu nástrojů hello.</span><span class="sxs-lookup"><span data-stu-id="c0fe2-129">You can customize hello list view by clicking **Columns** in hello toolbar.</span></span>

<span data-ttu-id="c0fe2-130">![Protokoly auditu](./media/active-directory-reporting-activity-audit-logs/19.png "Protokoly auditu")</span><span class="sxs-lookup"><span data-stu-id="c0fe2-130">![Audit logs](./media/active-directory-reporting-activity-audit-logs/19.png "Audit logs")</span></span>

<span data-ttu-id="c0fe2-131">To vám umožní toodisplay další pole nebo odeberte pole, které jsou již zobrazen.</span><span class="sxs-lookup"><span data-stu-id="c0fe2-131">This enables you toodisplay additional fields or remove fields that are already displayed.</span></span>

<span data-ttu-id="c0fe2-132">![Protokoly auditu](./media/active-directory-reporting-activity-audit-logs/21.png "Protokoly auditu")</span><span class="sxs-lookup"><span data-stu-id="c0fe2-132">![Audit logs](./media/active-directory-reporting-activity-audit-logs/21.png "Audit logs")</span></span>


<span data-ttu-id="c0fe2-133">Kliknutím na položku v zobrazení seznamu hello zobrazí všechny dostupné podrobné informace o něm.</span><span class="sxs-lookup"><span data-stu-id="c0fe2-133">By clicking an item in hello list view, you get all available details about it.</span></span>

<span data-ttu-id="c0fe2-134">![Protokoly auditu](./media/active-directory-reporting-activity-audit-logs/22.png "Protokoly auditu")</span><span class="sxs-lookup"><span data-stu-id="c0fe2-134">![Audit logs](./media/active-directory-reporting-activity-audit-logs/22.png "Audit logs")</span></span>


## <a name="filtering-audit-logs"></a><span data-ttu-id="c0fe2-135">Filtrování protokolů auditu</span><span class="sxs-lookup"><span data-stu-id="c0fe2-135">Filtering audit logs</span></span>

<span data-ttu-id="c0fe2-136">toonarrow dolů hello nahlášené tooa dat na úrovni, funguje pro vás, můžete filtrovat data auditu hello pomocí hello následující pole:</span><span class="sxs-lookup"><span data-stu-id="c0fe2-136">toonarrow down hello reported data tooa level that works for you, you can filter hello audit data using hello following fields:</span></span>

- <span data-ttu-id="c0fe2-137">Rozsah dat</span><span class="sxs-lookup"><span data-stu-id="c0fe2-137">Date range</span></span>
- <span data-ttu-id="c0fe2-138">Spustil(a) (činitel)</span><span class="sxs-lookup"><span data-stu-id="c0fe2-138">Initiated by (Actor)</span></span>
- <span data-ttu-id="c0fe2-139">Kategorie</span><span class="sxs-lookup"><span data-stu-id="c0fe2-139">Category</span></span>
- <span data-ttu-id="c0fe2-140">Typ prostředku aktivity</span><span class="sxs-lookup"><span data-stu-id="c0fe2-140">Activity resource type</span></span>
- <span data-ttu-id="c0fe2-141">Aktivita</span><span class="sxs-lookup"><span data-stu-id="c0fe2-141">Activity</span></span>

<span data-ttu-id="c0fe2-142">![Protokoly auditu](./media/active-directory-reporting-activity-audit-logs/23.png "Protokoly auditu")</span><span class="sxs-lookup"><span data-stu-id="c0fe2-142">![Audit logs](./media/active-directory-reporting-activity-audit-logs/23.png "Audit logs")</span></span>


<span data-ttu-id="c0fe2-143">Hello **rozsah dat** filtr povoluje tooyou toodefine časový rámec pro hello vrátil data.</span><span class="sxs-lookup"><span data-stu-id="c0fe2-143">hello **date range** filter enables tooyou toodefine a timeframe for hello returned data.</span></span>  
<span data-ttu-id="c0fe2-144">Možné hodnoty:</span><span class="sxs-lookup"><span data-stu-id="c0fe2-144">Possible values are:</span></span>

- <span data-ttu-id="c0fe2-145">1 měsíc</span><span class="sxs-lookup"><span data-stu-id="c0fe2-145">1 month</span></span>
- <span data-ttu-id="c0fe2-146">7 dní</span><span class="sxs-lookup"><span data-stu-id="c0fe2-146">7 days</span></span>
- <span data-ttu-id="c0fe2-147">24 hodin</span><span class="sxs-lookup"><span data-stu-id="c0fe2-147">24 hours</span></span>
- <span data-ttu-id="c0fe2-148">Vlastní</span><span class="sxs-lookup"><span data-stu-id="c0fe2-148">Custom</span></span>

<span data-ttu-id="c0fe2-149">Když vyberete vlastní časový rámec, můžete nakonfigurovat počáteční a koncový čas.</span><span class="sxs-lookup"><span data-stu-id="c0fe2-149">When you select a custom timeframe, you can configure a start time and an end time.</span></span>

<span data-ttu-id="c0fe2-150">Hello **iniciovaná** filtr umožňuje vám toodefine objektu actor název nebo jeho universal hlavní název (UPN).</span><span class="sxs-lookup"><span data-stu-id="c0fe2-150">hello **initiated by** filter enables you toodefine an actor's name or its universal principal name (UPN).</span></span>

<span data-ttu-id="c0fe2-151">Hello **kategorie** filtru vám umožní tooselect jeden hello následující filtru:</span><span class="sxs-lookup"><span data-stu-id="c0fe2-151">hello **category** filter enables you tooselect one of hello following filter:</span></span>

- <span data-ttu-id="c0fe2-152">Všechny</span><span class="sxs-lookup"><span data-stu-id="c0fe2-152">All</span></span>
- <span data-ttu-id="c0fe2-153">Základní kategorie</span><span class="sxs-lookup"><span data-stu-id="c0fe2-153">Core category</span></span>
- <span data-ttu-id="c0fe2-154">Základní adresář</span><span class="sxs-lookup"><span data-stu-id="c0fe2-154">Core directory</span></span>
- <span data-ttu-id="c0fe2-155">Samoobslužná správa hesel</span><span class="sxs-lookup"><span data-stu-id="c0fe2-155">Self-service password management</span></span>
- <span data-ttu-id="c0fe2-156">Samoobslužná správa skupin</span><span class="sxs-lookup"><span data-stu-id="c0fe2-156">Self-service group management</span></span>
- <span data-ttu-id="c0fe2-157">Zřizování účtů – automatická změna hesel</span><span class="sxs-lookup"><span data-stu-id="c0fe2-157">Account provisioning- Automated password rollover</span></span>
- <span data-ttu-id="c0fe2-158">Pozvaní uživatelé</span><span class="sxs-lookup"><span data-stu-id="c0fe2-158">Invited users</span></span>
- <span data-ttu-id="c0fe2-159">Služba MIM</span><span class="sxs-lookup"><span data-stu-id="c0fe2-159">MIM service</span></span>
- <span data-ttu-id="c0fe2-160">Identity Protection</span><span class="sxs-lookup"><span data-stu-id="c0fe2-160">Identity Protection</span></span>
- <span data-ttu-id="c0fe2-161">B2C</span><span class="sxs-lookup"><span data-stu-id="c0fe2-161">B2C</span></span>

<span data-ttu-id="c0fe2-162">Hello **typ prostředku aktivity** filtru vám umožní tooselect jedna z následujících hello filtry:</span><span class="sxs-lookup"><span data-stu-id="c0fe2-162">hello **activity resource type** filter enables you tooselect one of hello following filters:</span></span>

- <span data-ttu-id="c0fe2-163">Všechny</span><span class="sxs-lookup"><span data-stu-id="c0fe2-163">All</span></span> 
- <span data-ttu-id="c0fe2-164">Skupina</span><span class="sxs-lookup"><span data-stu-id="c0fe2-164">Group</span></span>
- <span data-ttu-id="c0fe2-165">Adresář</span><span class="sxs-lookup"><span data-stu-id="c0fe2-165">Directory</span></span>
- <span data-ttu-id="c0fe2-166">Uživatel</span><span class="sxs-lookup"><span data-stu-id="c0fe2-166">User</span></span>
- <span data-ttu-id="c0fe2-167">Aplikace</span><span class="sxs-lookup"><span data-stu-id="c0fe2-167">Application</span></span>
- <span data-ttu-id="c0fe2-168">Zásada</span><span class="sxs-lookup"><span data-stu-id="c0fe2-168">Policy</span></span>
- <span data-ttu-id="c0fe2-169">Zařízení</span><span class="sxs-lookup"><span data-stu-id="c0fe2-169">Device</span></span>
- <span data-ttu-id="c0fe2-170">Ostatní</span><span class="sxs-lookup"><span data-stu-id="c0fe2-170">Other</span></span>

<span data-ttu-id="c0fe2-171">Když vyberete **skupiny** jako **typ prostředku aktivity**, dojde další filtr kategorii, která vám umožní tooalso poskytují **zdroj**:</span><span class="sxs-lookup"><span data-stu-id="c0fe2-171">When you select **Group** as **activity resource type**, you get an additional filter category that enables you tooalso provide a **Source**:</span></span>

- <span data-ttu-id="c0fe2-172">Azure AD</span><span class="sxs-lookup"><span data-stu-id="c0fe2-172">Azure AD</span></span>
- <span data-ttu-id="c0fe2-173">O365</span><span class="sxs-lookup"><span data-stu-id="c0fe2-173">O365</span></span>


<span data-ttu-id="c0fe2-174">Hello **aktivity** filtru je založené na kategorii hello a výběr typu prostředku aktivity provedete.</span><span class="sxs-lookup"><span data-stu-id="c0fe2-174">hello **activity** filter is based on hello category and Activity resource type selection you make.</span></span> <span data-ttu-id="c0fe2-175">Můžete vybrat konkrétní aktivity mají toosee nebo zvolit vše.</span><span class="sxs-lookup"><span data-stu-id="c0fe2-175">You can select a specific activity you want toosee or choose all.</span></span> 

<span data-ttu-id="c0fe2-176">Můžete získat hello seznam všech aktivit auditu pomocí hello rozhraní Graph API https://graph.windows.net/$ tenantdomain/aktivity nebo auditActivityTypes? api-version = beta, kde $tenantdomain = název domény nebo najdete v článku toohello [audit sestavy události](active-directory-reporting-audit-events.md).</span><span class="sxs-lookup"><span data-stu-id="c0fe2-176">You can get hello list of all Audit Activities using hello Graph API https://graph.windows.net/$tenantdomain/activities/auditActivityTypes?api-version=beta, where $tenantdomain = your domain name or refer toohello article [audit report events](active-directory-reporting-audit-events.md).</span></span>


## <a name="audit-logs-shortcuts"></a><span data-ttu-id="c0fe2-177">Zástupci pro protokoly auditu</span><span class="sxs-lookup"><span data-stu-id="c0fe2-177">Audit logs shortcuts</span></span>

<span data-ttu-id="c0fe2-178">Kromě toho příliš**Azure Active Directory**, hello portál Azure poskytuje dva další vstupní body tooaudit dat:</span><span class="sxs-lookup"><span data-stu-id="c0fe2-178">In addition too**Azure Active Directory**, hello Azure portal provides you with two additional entry points tooaudit data:</span></span>

- <span data-ttu-id="c0fe2-179">Uživatelé a skupiny</span><span class="sxs-lookup"><span data-stu-id="c0fe2-179">Users and groups</span></span>
- <span data-ttu-id="c0fe2-180">Podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="c0fe2-180">Enterprise applications</span></span>

### <a name="users-and-groups-audit-logs"></a><span data-ttu-id="c0fe2-181">Protokoly auditu uživatelů a skupin</span><span class="sxs-lookup"><span data-stu-id="c0fe2-181">Users and groups audit logs</span></span>

<span data-ttu-id="c0fe2-182">Uživatele a sestavy na základě skupiny auditu můžete získat odpovědi tooquestions jako:</span><span class="sxs-lookup"><span data-stu-id="c0fe2-182">With user and group-based audit reports, you can get answers tooquestions such as:</span></span>

- <span data-ttu-id="c0fe2-183">Jaké typy aktualizací byly použité hello uživatelů?</span><span class="sxs-lookup"><span data-stu-id="c0fe2-183">What types of updates have been applied hello users?</span></span>

- <span data-ttu-id="c0fe2-184">Kolik uživatelů bylo změněno?</span><span class="sxs-lookup"><span data-stu-id="c0fe2-184">How many users were changed?</span></span>

- <span data-ttu-id="c0fe2-185">Kolik hesel bylo změněno?</span><span class="sxs-lookup"><span data-stu-id="c0fe2-185">How many passwords were changed?</span></span>

- <span data-ttu-id="c0fe2-186">Co provedl správce v adresáři?</span><span class="sxs-lookup"><span data-stu-id="c0fe2-186">What has an administrator done in a directory?</span></span>

- <span data-ttu-id="c0fe2-187">Co jsou hello skupiny, které byly přidány?</span><span class="sxs-lookup"><span data-stu-id="c0fe2-187">What are hello groups that have been added?</span></span>

- <span data-ttu-id="c0fe2-188">Došlo u některých skupin ke změnám členství?</span><span class="sxs-lookup"><span data-stu-id="c0fe2-188">Are there groups with membership changes?</span></span>

- <span data-ttu-id="c0fe2-189">Byl změněn hello vlastníků skupiny?</span><span class="sxs-lookup"><span data-stu-id="c0fe2-189">Have hello owners of group been changed?</span></span>

- <span data-ttu-id="c0fe2-190">Jaké licence byly přiřazeny tooa skupinu nebo uživatele?</span><span class="sxs-lookup"><span data-stu-id="c0fe2-190">What licenses have been assigned tooa group or a user?</span></span>

<span data-ttu-id="c0fe2-191">Pokud chcete tooreview auditování data, která je související toousers a skupiny, můžete najít v části filtrované zobrazení **protokoly auditu** v hello **aktivity** části hello **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="c0fe2-191">If you just want tooreview auditing data that is related toousers and groups, you can find a filtered view under **Audit logs** in hello **Activity** section of hello **Users and Groups**.</span></span> <span data-ttu-id="c0fe2-192">Tento vstupní bod má jako **Typ prostředku aktivity** předem vybranou možnost **Uživatelé a skupiny**.</span><span class="sxs-lookup"><span data-stu-id="c0fe2-192">This entry point has **Users and groups** as preselected **Activity Resource Type**.</span></span>

<span data-ttu-id="c0fe2-193">![Protokoly auditu](./media/active-directory-reporting-activity-audit-logs/93.png "Protokoly auditu")</span><span class="sxs-lookup"><span data-stu-id="c0fe2-193">![Audit logs](./media/active-directory-reporting-activity-audit-logs/93.png "Audit logs")</span></span>

### <a name="enterprise-applications-audit-logs"></a><span data-ttu-id="c0fe2-194">Protokoly auditu podnikových aplikací</span><span class="sxs-lookup"><span data-stu-id="c0fe2-194">Enterprise applications audit logs</span></span>

<span data-ttu-id="c0fe2-195">S založené na aplikaci audit sestavy, můžete získat odpovědi tooquestions jako:</span><span class="sxs-lookup"><span data-stu-id="c0fe2-195">With application-based audit reports, you can get answers tooquestions such as:</span></span>

* <span data-ttu-id="c0fe2-196">Jaké jsou hello aplikace, které byl přidán nebo aktualizován?</span><span class="sxs-lookup"><span data-stu-id="c0fe2-196">What are hello applications that have been added or updated?</span></span>
* <span data-ttu-id="c0fe2-197">Jaké jsou hello aplikace, které byly odebrány?</span><span class="sxs-lookup"><span data-stu-id="c0fe2-197">What are hello applications that have been removed?</span></span>
* <span data-ttu-id="c0fe2-198">Změnil se instanční objekt pro aplikaci?</span><span class="sxs-lookup"><span data-stu-id="c0fe2-198">Has a service principle for an application changed?</span></span>
* <span data-ttu-id="c0fe2-199">Byl změněn hello názvy aplikací?</span><span class="sxs-lookup"><span data-stu-id="c0fe2-199">Have hello names of applications been changed?</span></span>
* <span data-ttu-id="c0fe2-200">Kdo jste dali souhlas tooan aplikace?</span><span class="sxs-lookup"><span data-stu-id="c0fe2-200">Who gave consent tooan application?</span></span>

<span data-ttu-id="c0fe2-201">Pokud chcete tooreview auditování data, která jsou aplikace související tooyour, můžete najít filtrované zobrazení pod **protokoly auditu** v hello **aktivity** části hello **podnikové aplikace**  okno.</span><span class="sxs-lookup"><span data-stu-id="c0fe2-201">If you just want tooreview auditing data that is related tooyour applications, you can find a filtered view under **Audit logs** in hello **Activity** section of hello **Enterprise applications** blade.</span></span> <span data-ttu-id="c0fe2-202">Tento vstupní bod má jako **Typ prostředku aktivity** předem vybranou možnost **Podniková aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c0fe2-202">This entry point has **Enterprise applications** as preselected **Activity Resource Type**.</span></span>

<span data-ttu-id="c0fe2-203">![Protokoly auditu](./media/active-directory-reporting-activity-audit-logs/134.png "Protokoly auditu")</span><span class="sxs-lookup"><span data-stu-id="c0fe2-203">![Audit logs](./media/active-directory-reporting-activity-audit-logs/134.png "Audit logs")</span></span>

<span data-ttu-id="c0fe2-204">Můžete filtrovat toto zobrazení další dolů toojust **skupiny** nebo právě **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="c0fe2-204">You can filter this view further down toojust **groups** or just **users**.</span></span>

<span data-ttu-id="c0fe2-205">![Protokoly auditu](./media/active-directory-reporting-activity-audit-logs/25.png "Protokoly auditu")</span><span class="sxs-lookup"><span data-stu-id="c0fe2-205">![Audit logs](./media/active-directory-reporting-activity-audit-logs/25.png "Audit logs")</span></span>


## <a name="next-steps"></a><span data-ttu-id="c0fe2-206">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c0fe2-206">Next steps</span></span>

<span data-ttu-id="c0fe2-207">Přehled vytváření sestav najdete v tématu hello [generování sestav Azure Active Directory](active-directory-reporting-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c0fe2-207">For an overview of reporting, see hello [Azure Active Directory reporting](active-directory-reporting-azure-portal.md).</span></span>

