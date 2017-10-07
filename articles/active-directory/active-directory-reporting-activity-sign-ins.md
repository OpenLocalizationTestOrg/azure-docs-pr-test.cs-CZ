---
title: "sestavy aktivit aaaSign v portálu Azure Active Directory hello | Microsoft Docs"
description: "Úvod aktivitu toosign sestav na portálu Azure Active Directory hello"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/19/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 49590d625a08d7dc189a629b89bab2261c2b4780
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sign-in-activity-reports-in-hello-azure-active-directory-portal"></a><span data-ttu-id="5425d-103">Přihlašovací aktivity sestav na portálu Azure Active Directory hello</span><span class="sxs-lookup"><span data-stu-id="5425d-103">Sign-in activity reports in hello Azure Active Directory portal</span></span>

<span data-ttu-id="5425d-104">S vytvářením sestav Azure Active Directory (Azure AD) v hello [portál Azure](https://portal.azure.com), můžete získat hello informace, které potřebujete toodetermine úspěšnost prostředí.</span><span class="sxs-lookup"><span data-stu-id="5425d-104">With Azure Active Directory (Azure AD) reporting in hello [Azure portal](https://portal.azure.com), you can get hello information you need toodetermine how your environment is doing.</span></span>

<span data-ttu-id="5425d-105">Hello reporting architektury v Azure Active Directory se skládá z hello následující součásti:</span><span class="sxs-lookup"><span data-stu-id="5425d-105">hello reporting architecture in Azure Active Directory consists of hello following components:</span></span>

- <span data-ttu-id="5425d-106">**Aktivita**</span><span class="sxs-lookup"><span data-stu-id="5425d-106">**Activity**</span></span> 
    - <span data-ttu-id="5425d-107">**Přihlašovací aktivity** – informace o využití hello spravovaných aplikací a aktivit přihlášení uživatelů</span><span class="sxs-lookup"><span data-stu-id="5425d-107">**Sign-in activities** – Information about hello usage of managed applications and user sign-in activities</span></span>
    - <span data-ttu-id="5425d-108">**Protokoly auditu** – informace aktivit systému o správě uživatelů a skupin, spravovaných aplikacích a aktivitách adresářů</span><span class="sxs-lookup"><span data-stu-id="5425d-108">**Audit logs** - System activity information about users and group management, your managed applications and directory activities.</span></span>
- <span data-ttu-id="5425d-109">**Zabezpečení**</span><span class="sxs-lookup"><span data-stu-id="5425d-109">**Security**</span></span> 
    - <span data-ttu-id="5425d-110">**Rizikové přihlášení** -rizikové přihlášení je indikátorem pro pokusu přihlášení, který se možná prováděly uživatelem, který není vlastníkem legitimní hello uživatelského účtu.</span><span class="sxs-lookup"><span data-stu-id="5425d-110">**Risky sign-ins** - A risky sign-in is an indicator for a sign-in attempt that might have been performed by someone who is not hello legitimate owner of a user account.</span></span> <span data-ttu-id="5425d-111">Další podrobnosti najdete v tématu Riziková přihlášení.</span><span class="sxs-lookup"><span data-stu-id="5425d-111">For more details, see Risky sign-ins.</span></span>
    - <span data-ttu-id="5425d-112">**Uživatelé označení příznakem rizika** – Rizikový uživatel je indikátorem uživatelského účtu, který mohl být ohrožený.</span><span class="sxs-lookup"><span data-stu-id="5425d-112">**Users flagged for risk** - A risky user is an indicator for a user account that might have been compromised.</span></span> <span data-ttu-id="5425d-113">Další podrobnosti najdete v tématu Uživatelé označení příznakem rizika.</span><span class="sxs-lookup"><span data-stu-id="5425d-113">For more details, see Users flagged for risk.</span></span>

<span data-ttu-id="5425d-114">Toto téma poskytuje přehled hello přihlašovací aktivity.</span><span class="sxs-lookup"><span data-stu-id="5425d-114">This topic gives you an overview of hello sign-in activities.</span></span>

## <a name="pre-requisite"></a><span data-ttu-id="5425d-115">Předpoklad</span><span class="sxs-lookup"><span data-stu-id="5425d-115">Pre-requisite</span></span>

### <a name="who-can-access-hello-data"></a><span data-ttu-id="5425d-116">Kdo může přistupovat k datům hello?</span><span class="sxs-lookup"><span data-stu-id="5425d-116">Who can access hello data?</span></span>
* <span data-ttu-id="5425d-117">Uživatelé v roli správce zabezpečení nebo zabezpečení čtečky hello</span><span class="sxs-lookup"><span data-stu-id="5425d-117">Users in hello Security Admin or Security Reader role</span></span>
* <span data-ttu-id="5425d-118">Globální správci</span><span class="sxs-lookup"><span data-stu-id="5425d-118">Global Admins</span></span>
* <span data-ttu-id="5425d-119">Každý uživatel (bez oprávnění správce) může přistupovat k vlastnímu přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5425d-119">Any user (non-admins) can access their own sign-ins</span></span> 

### <a name="what-azure-ad-license-do-you-need-tooaccess-sign-in-activity"></a><span data-ttu-id="5425d-120">Jaké licence Azure AD potřebujete přihlašovací aktivity tooaccess?</span><span class="sxs-lookup"><span data-stu-id="5425d-120">What Azure AD license do you need tooaccess sign-in activity?</span></span>
* <span data-ttu-id="5425d-121">Váš klient musí mít licenci na Azure AD Premium s ním spojená toosee hello všechny až přihlašovací aktivity sestavy</span><span class="sxs-lookup"><span data-stu-id="5425d-121">Your tenant must have an Azure AD Premium license associated with it toosee hello all up sign-in activity report</span></span>


## <a name="signs-in-activities"></a><span data-ttu-id="5425d-122">Aktivity přihlašování</span><span class="sxs-lookup"><span data-stu-id="5425d-122">Signs-in activities</span></span>

<span data-ttu-id="5425d-123">S hello informací uvedených v sestavě přihlášení uživatele hello najít tooquestions odpovědi, například:</span><span class="sxs-lookup"><span data-stu-id="5425d-123">With hello information provided by hello user sign-in report, you find answers tooquestions such as:</span></span>

* <span data-ttu-id="5425d-124">Co je hello přihlášení vzor uživatele?</span><span class="sxs-lookup"><span data-stu-id="5425d-124">What is hello sign-in pattern of a user?</span></span>
* <span data-ttu-id="5425d-125">Kolik uživatelů se přihlásilo za týden?</span><span class="sxs-lookup"><span data-stu-id="5425d-125">How many users have users signed in over a week?</span></span>
* <span data-ttu-id="5425d-126">Co je hello stav těchto přihlášení?</span><span class="sxs-lookup"><span data-stu-id="5425d-126">What’s hello status of these sign-ins?</span></span>

<span data-ttu-id="5425d-127">Vaše první vstupní bod tooall přihlašovací aktivity data **přihlášení** v části aktivity hello **Azure Active**.</span><span class="sxs-lookup"><span data-stu-id="5425d-127">Your first entry point tooall sign-in activities data is **Sign-ins** in hello Activity section of **Azure Active**.</span></span>


<span data-ttu-id="5425d-128">![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/61.png "Aktivita přihlašování")</span><span class="sxs-lookup"><span data-stu-id="5425d-128">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/61.png "Sign-in activity")</span></span>


<span data-ttu-id="5425d-129">Protokol auditu má výchozí zobrazení seznamu, které obsahuje následující položky:</span><span class="sxs-lookup"><span data-stu-id="5425d-129">An audit log has a default list view that shows:</span></span>

- <span data-ttu-id="5425d-130">související uživatelské Hello</span><span class="sxs-lookup"><span data-stu-id="5425d-130">hello related user</span></span>
- <span data-ttu-id="5425d-131">uživatel hello aplikace Hello má přihlášeného k</span><span class="sxs-lookup"><span data-stu-id="5425d-131">hello application hello user has signed-in to</span></span>
- <span data-ttu-id="5425d-132">Stav přihlášení Hello</span><span class="sxs-lookup"><span data-stu-id="5425d-132">hello sign-in status</span></span>
- <span data-ttu-id="5425d-133">čas přihlášení Hello</span><span class="sxs-lookup"><span data-stu-id="5425d-133">hello sign-in time</span></span>

<span data-ttu-id="5425d-134">![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/41.png "Aktivita přihlašování")</span><span class="sxs-lookup"><span data-stu-id="5425d-134">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/41.png "Sign-in activity")</span></span>

<span data-ttu-id="5425d-135">Kliknutím můžete přizpůsobit zobrazení seznamu hello **sloupce** v panelu nástrojů hello.</span><span class="sxs-lookup"><span data-stu-id="5425d-135">You can customize hello list view by clicking **Columns** in hello toolbar.</span></span>

<span data-ttu-id="5425d-136">![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/19.png "Aktivita přihlašování")</span><span class="sxs-lookup"><span data-stu-id="5425d-136">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/19.png "Sign-in activity")</span></span>

<span data-ttu-id="5425d-137">To vám umožní toodisplay další pole nebo odeberte pole, které jsou již zobrazen.</span><span class="sxs-lookup"><span data-stu-id="5425d-137">This enables you toodisplay additional fields or remove fields that are already displayed.</span></span>

<span data-ttu-id="5425d-138">![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/42.png "Aktivita přihlašování")</span><span class="sxs-lookup"><span data-stu-id="5425d-138">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/42.png "Sign-in activity")</span></span>

<span data-ttu-id="5425d-139">Kliknutím na položku v zobrazení seznamu hello zobrazí všechny dostupné podrobné informace o něm.</span><span class="sxs-lookup"><span data-stu-id="5425d-139">By clicking an item in hello list view, you get all available details about it.</span></span>

<span data-ttu-id="5425d-140">![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/43.png "Aktivita přihlašování")</span><span class="sxs-lookup"><span data-stu-id="5425d-140">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/43.png "Sign-in activity")</span></span>


## <a name="filtering-sign-in-activities"></a><span data-ttu-id="5425d-141">Filtrování aktivit přihlašování</span><span class="sxs-lookup"><span data-stu-id="5425d-141">Filtering sign-in activities</span></span>

<span data-ttu-id="5425d-142">toonarrow dolů hello nahlášené tooa dat na úrovni, funguje pro vás, data lze filtrovat hello přihlášení pomocí hello následující pole:</span><span class="sxs-lookup"><span data-stu-id="5425d-142">toonarrow down hello reported data tooa level that works for you, you can filter hello sign-ins data using hello following fields:</span></span>

- <span data-ttu-id="5425d-143">Časový interval</span><span class="sxs-lookup"><span data-stu-id="5425d-143">Time interval</span></span>
- <span data-ttu-id="5425d-144">Uživatel</span><span class="sxs-lookup"><span data-stu-id="5425d-144">User</span></span>
- <span data-ttu-id="5425d-145">Aplikace</span><span class="sxs-lookup"><span data-stu-id="5425d-145">Application</span></span>
- <span data-ttu-id="5425d-146">Klient</span><span class="sxs-lookup"><span data-stu-id="5425d-146">Client</span></span>
- <span data-ttu-id="5425d-147">Stav přihlášení</span><span class="sxs-lookup"><span data-stu-id="5425d-147">Sign-in status</span></span>

<span data-ttu-id="5425d-148">![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/44.png "Aktivita přihlašování")</span><span class="sxs-lookup"><span data-stu-id="5425d-148">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/44.png "Sign-in activity")</span></span>


<span data-ttu-id="5425d-149">Hello **časový interval** filtr povoluje tooyou toodefine časový rámec pro hello vrátil data.</span><span class="sxs-lookup"><span data-stu-id="5425d-149">hello **time interval** filter enables tooyou toodefine a timeframe for hello returned data.</span></span>  
<span data-ttu-id="5425d-150">Možné hodnoty:</span><span class="sxs-lookup"><span data-stu-id="5425d-150">Possible values are:</span></span>

- <span data-ttu-id="5425d-151">1 měsíc</span><span class="sxs-lookup"><span data-stu-id="5425d-151">1 month</span></span>
- <span data-ttu-id="5425d-152">7 dní</span><span class="sxs-lookup"><span data-stu-id="5425d-152">7 days</span></span>
- <span data-ttu-id="5425d-153">24 hodin</span><span class="sxs-lookup"><span data-stu-id="5425d-153">24 hours</span></span>
- <span data-ttu-id="5425d-154">Vlastní</span><span class="sxs-lookup"><span data-stu-id="5425d-154">Custom</span></span>

<span data-ttu-id="5425d-155">Když vyberete vlastní časový rámec, můžete nakonfigurovat počáteční a koncový čas.</span><span class="sxs-lookup"><span data-stu-id="5425d-155">When you select a custom timeframe, you can configure a start time and an end time.</span></span>

<span data-ttu-id="5425d-156">Hello **uživatele** filtru vám umožní toospecify hello název nebo hello hlavní název uživatele (UPN) uživatele hello kterých vám nejvíc záleží.</span><span class="sxs-lookup"><span data-stu-id="5425d-156">hello **user** filter enables you toospecify hello name or hello user principal name (UPN) of hello user you care about.</span></span>

<span data-ttu-id="5425d-157">Hello **aplikace** filtru vám umožní toospecify hello název aplikace hello kterých vám nejvíc záleží.</span><span class="sxs-lookup"><span data-stu-id="5425d-157">hello **application** filter enables you toospecify hello name of hello application you care about.</span></span>

<span data-ttu-id="5425d-158">Hello **klienta** filtru vám umožní toospecify informace o zařízení hello kterých vám nejvíc záleží.</span><span class="sxs-lookup"><span data-stu-id="5425d-158">hello **client** filter enables you toospecify information about hello device you care about.</span></span>

<span data-ttu-id="5425d-159">Hello **stav přihlášení** filtru vám umožní tooselect jeden hello následující filtru:</span><span class="sxs-lookup"><span data-stu-id="5425d-159">hello **sign-in status** filter enables you tooselect one of hello following filter:</span></span>

- <span data-ttu-id="5425d-160">Všechny</span><span class="sxs-lookup"><span data-stu-id="5425d-160">All</span></span>
- <span data-ttu-id="5425d-161">Úspěch</span><span class="sxs-lookup"><span data-stu-id="5425d-161">Success</span></span>
- <span data-ttu-id="5425d-162">Selhání</span><span class="sxs-lookup"><span data-stu-id="5425d-162">Failure</span></span>


## <a name="sign-in-activities-shortcuts"></a><span data-ttu-id="5425d-163">Zkratky pro aktivity přihlašování</span><span class="sxs-lookup"><span data-stu-id="5425d-163">Sign-in activities shortcuts</span></span>

<span data-ttu-id="5425d-164">Kromě tooAzure služby Active Directory, hello portál Azure poskytuje dva další vstupní body aktivity toosign v údaje:</span><span class="sxs-lookup"><span data-stu-id="5425d-164">In addition tooAzure Active Directory, hello Azure portal provides you with two additional entry points toosign-in activities data:</span></span>

- <span data-ttu-id="5425d-165">Uživatelé a skupiny</span><span class="sxs-lookup"><span data-stu-id="5425d-165">Users and groups</span></span>
- <span data-ttu-id="5425d-166">Podnikové aplikace</span><span class="sxs-lookup"><span data-stu-id="5425d-166">Enterprise applications</span></span>


### <a name="users-and-groups-sign-ins-activities"></a><span data-ttu-id="5425d-167">Aktivity přihlašování uživatelů a skupin</span><span class="sxs-lookup"><span data-stu-id="5425d-167">Users and groups sign-ins activities</span></span>

<span data-ttu-id="5425d-168">S hello informací uvedených v sestavě přihlášení uživatele hello najít tooquestions odpovědi, například:</span><span class="sxs-lookup"><span data-stu-id="5425d-168">With hello information provided by hello user sign-in report, you find answers tooquestions such as:</span></span>

- <span data-ttu-id="5425d-169">Co je hello přihlášení vzor uživatele?</span><span class="sxs-lookup"><span data-stu-id="5425d-169">What is hello sign-in pattern of a user?</span></span>
- <span data-ttu-id="5425d-170">Kolik uživatelů se přihlásilo za týden?</span><span class="sxs-lookup"><span data-stu-id="5425d-170">How many users have users signed in over a week?</span></span>
- <span data-ttu-id="5425d-171">Co je hello stav těchto přihlášení?</span><span class="sxs-lookup"><span data-stu-id="5425d-171">What’s hello status of these sign-ins?</span></span>



<span data-ttu-id="5425d-172">Vstupní bod toothis dat je hello uživatele přihlásit grafu v hello **přehled** oddílu pod **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="5425d-172">Your entry point toothis data is hello user sign-in graph in hello **Overview** section under **Users and groups**.</span></span>

<span data-ttu-id="5425d-173">![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/45.png "Aktivita přihlašování")</span><span class="sxs-lookup"><span data-stu-id="5425d-173">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/45.png "Sign-in activity")</span></span>

<span data-ttu-id="5425d-174">Hello uživatele přihlásit graf znázorňuje týdenní agregací přihlašovací Plugin pro všechny uživatele v daném časovém období.</span><span class="sxs-lookup"><span data-stu-id="5425d-174">hello user sign-in graph shows weekly aggregations of sign ins for all users in a given time period.</span></span> <span data-ttu-id="5425d-175">Výchozí hodnota Hello hello časové období je 30 dní.</span><span class="sxs-lookup"><span data-stu-id="5425d-175">hello default for hello time period is 30 days.</span></span>

<span data-ttu-id="5425d-176">![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/46.png "Aktivita přihlašování")</span><span class="sxs-lookup"><span data-stu-id="5425d-176">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/46.png "Sign-in activity")</span></span>

<span data-ttu-id="5425d-177">Když kliknete na den v hello přihlášení grafu, zobrazí podrobný seznam hello přihlašovací aktivity pro tento den.</span><span class="sxs-lookup"><span data-stu-id="5425d-177">When you click on a day in hello sign-in graph, you get a detailed list of hello sign-in activities for this day.</span></span>

<span data-ttu-id="5425d-178">![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/41.png "Aktivita přihlašování")</span><span class="sxs-lookup"><span data-stu-id="5425d-178">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/41.png "Sign-in activity")</span></span>

<span data-ttu-id="5425d-179">Každý řádek v hello přihlašovací aktivity nabízí seznamu hello podrobné informace o přihlášení hello vybraný jako:</span><span class="sxs-lookup"><span data-stu-id="5425d-179">Each row in hello sign-in activities list gives you hello detailed information about hello selected sign-in such as:</span></span>

* <span data-ttu-id="5425d-180">Kdo se přihlásil?</span><span class="sxs-lookup"><span data-stu-id="5425d-180">Who has signed in?</span></span>
* <span data-ttu-id="5425d-181">Jaká byla hello související UPN?</span><span class="sxs-lookup"><span data-stu-id="5425d-181">What was hello related UPN?</span></span>
* <span data-ttu-id="5425d-182">Jaké aplikace se hello cíl hello přihlásit?</span><span class="sxs-lookup"><span data-stu-id="5425d-182">What application was hello target of hello sign-in?</span></span>
* <span data-ttu-id="5425d-183">Co je hello IP adresu hello přihlásit?</span><span class="sxs-lookup"><span data-stu-id="5425d-183">What is hello IP address of hello sign-in?</span></span>
* <span data-ttu-id="5425d-184">Jaká byla hello stav hello přihlásit?</span><span class="sxs-lookup"><span data-stu-id="5425d-184">What was hello status of hello sign-in?</span></span>

<span data-ttu-id="5425d-185">Hello **přihlášení** možnost poskytuje úplný přehled všech přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="5425d-185">hello **Sign-ins** option gives you a complete overview of all user sign-ins.</span></span>

<span data-ttu-id="5425d-186">![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/51.png "Aktivita přihlašování")</span><span class="sxs-lookup"><span data-stu-id="5425d-186">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/51.png "Sign-in activity")</span></span>



## <a name="usage-of-managed-applications"></a><span data-ttu-id="5425d-187">Použití spravovaných aplikací</span><span class="sxs-lookup"><span data-stu-id="5425d-187">Usage of managed applications</span></span>

<span data-ttu-id="5425d-188">S použitím zobrazení dat přihlašování zaměřeného na aplikace můžete odpovídat na otázky tohoto typu:</span><span class="sxs-lookup"><span data-stu-id="5425d-188">With an application-centric view of your sign-in data, you can answer questions such as:</span></span>

* <span data-ttu-id="5425d-189">Kdo používá mé aplikace?</span><span class="sxs-lookup"><span data-stu-id="5425d-189">Who is using my applications?</span></span>
* <span data-ttu-id="5425d-190">Jaké jsou hlavní 3 aplikace hello ve vaší organizaci?</span><span class="sxs-lookup"><span data-stu-id="5425d-190">What are hello top 3 applications in your organization?</span></span>
* <span data-ttu-id="5425d-191">Nedávno jsem zpřístupnil aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5425d-191">I have recently rolled out an application.</span></span> <span data-ttu-id="5425d-192">Jak to s ní vypadá?</span><span class="sxs-lookup"><span data-stu-id="5425d-192">How is it doing?</span></span>

<span data-ttu-id="5425d-193">Vstupní bod toothis dat je hello hlavních 3 aplikací ve vaší organizaci v rámci hello posledních 30 dní sestavy v hello **přehled** oddílu pod **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="5425d-193">Your entry point toothis data is hello top 3 applications in your organization within hello last 30 days report in hello **Overview** section under **Enterprise applications**.</span></span>

<span data-ttu-id="5425d-194">![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/64.png "Aktivita přihlašování")</span><span class="sxs-lookup"><span data-stu-id="5425d-194">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/64.png "Sign-in activity")</span></span>

<span data-ttu-id="5425d-195">Hello aplikace využití grafu týdenní agregací přihlášení pro 3 hlavních aplikací v daném časovém období.</span><span class="sxs-lookup"><span data-stu-id="5425d-195">hello app usage graph weekly aggregations of sign ins for your top 3 applications in a given time period.</span></span> <span data-ttu-id="5425d-196">Výchozí hodnota Hello hello časové období je 30 dní.</span><span class="sxs-lookup"><span data-stu-id="5425d-196">hello default for hello time period is 30 days.</span></span>

<span data-ttu-id="5425d-197">![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/47.png "Aktivita přihlašování")</span><span class="sxs-lookup"><span data-stu-id="5425d-197">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/47.png "Sign-in activity")</span></span>

<span data-ttu-id="5425d-198">Pokud chcete, můžete nastavit hello zaměřit na konkrétní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5425d-198">If you want to, you can set hello focus on a specific application.</span></span>


<span data-ttu-id="5425d-199">![Vytváření sestav](./media/active-directory-reporting-activity-sign-ins/single_spp_usage_graph.png "Vytváření sestav")</span><span class="sxs-lookup"><span data-stu-id="5425d-199">![Reporting](./media/active-directory-reporting-activity-sign-ins/single_spp_usage_graph.png "Reporting")</span></span>

<span data-ttu-id="5425d-200">Když kliknete na den v grafu využití aplikace hello, zobrazí podrobný seznam hello přihlašovací aktivity.</span><span class="sxs-lookup"><span data-stu-id="5425d-200">When you click on a day in hello app usage graph, you get a detailed list of hello sign-in activities.</span></span>


<span data-ttu-id="5425d-201">![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/48.png "Aktivita přihlašování")</span><span class="sxs-lookup"><span data-stu-id="5425d-201">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/48.png "Sign-in activity")</span></span>


<span data-ttu-id="5425d-202">Hello **přihlášení** možnost poskytuje úplný přehled všech aplikací tooyour události přihlášení.</span><span class="sxs-lookup"><span data-stu-id="5425d-202">hello **Sign-ins** option gives you a complete overview of all sign-in events tooyour applications.</span></span>

<span data-ttu-id="5425d-203">![Aktivita přihlašování](./media/active-directory-reporting-activity-sign-ins/49.png "Aktivita přihlašování")</span><span class="sxs-lookup"><span data-stu-id="5425d-203">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/49.png "Sign-in activity")</span></span>



## <a name="next-steps"></a><span data-ttu-id="5425d-204">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5425d-204">Next steps</span></span>

<span data-ttu-id="5425d-205">Pokud chcete více informací o kódy chyb přihlašovací aktivita tooknow, najdete v části hello [přihlášení kódy chyb v portálu Azure Active Directory hello aktivity sestavy](active-directory-reporting-activity-sign-ins-errors.md).</span><span class="sxs-lookup"><span data-stu-id="5425d-205">If you want tooknow more about sign-in activity error codes, see hello [Sign-in activity report error codes in hello Azure Active Directory portal](active-directory-reporting-activity-sign-ins-errors.md).</span></span>

