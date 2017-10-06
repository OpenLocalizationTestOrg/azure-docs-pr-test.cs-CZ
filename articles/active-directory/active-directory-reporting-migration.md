---
title: "Aktivita aaaFind sestavy v hello portálu Azure | Microsoft Docs"
description: "Zjistěte, jak toofind aktivita služby Azure Active Directory sestavy v hello portálu Azure."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: d93521f8-dc21-4feb-aaff-4bb300f04812
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/19/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: f8d7a968403e10ccc5319f27fedad38b1553ded0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="find-activity-reports-in-hello-azure-portal"></a><span data-ttu-id="0c6f7-103">Najít sestavy aktivit v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="0c6f7-103">Find activity reports in hello Azure portal</span></span>

<span data-ttu-id="0c6f7-104">Pokud přecházíte z hello Azure classic portálu toohello portálu Azure, můžete získat nový pohled na protokoly aktivity Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="0c6f7-104">If you are moving from hello Azure classic portal toohello Azure portal, you get a new look at Azure Active Directory (Azure AD) activity logs.</span></span> <span data-ttu-id="0c6f7-105">V poslední [příspěvku na blogu](https://blogs.technet.microsoft.com/enterprisemobility/2016/11/08/azuread-weve-just-turned-on-detailed-auditing-and-sign-in-logs-in-the-new-azure-portal/), vám vysvětlíme, jak vidíte aktivitu protokolů v kontextu hello hello prostředku pracujete v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0c6f7-105">In a recent [blog post](https://blogs.technet.microsoft.com/enterprisemobility/2016/11/08/azuread-weve-just-turned-on-detailed-auditing-and-sign-in-logs-in-the-new-azure-portal/), we explain how you can see activity logs in hello context of hello resource you are working on in hello Azure portal.</span></span> <span data-ttu-id="0c6f7-106">V tomto článku jsme popisují, jak toofind sestavy, který jste použili v hello portál Azure classic hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0c6f7-106">In this article, we describe how toofind reports that you used in hello Azure classic portal in hello Azure portal.</span></span>

## <a name="whats-new"></a><span data-ttu-id="0c6f7-107">Co je nového</span><span class="sxs-lookup"><span data-stu-id="0c6f7-107">What's new</span></span>

<span data-ttu-id="0c6f7-108">Sestavy v hello portál Azure classic jsou rozdělené do kategorií:</span><span class="sxs-lookup"><span data-stu-id="0c6f7-108">Reports in hello Azure classic portal are separated into categories:</span></span>

1.  <span data-ttu-id="0c6f7-109">Sestavy zabezpečení</span><span class="sxs-lookup"><span data-stu-id="0c6f7-109">Security reports</span></span>
2.  <span data-ttu-id="0c6f7-110">Sestavy aktivit</span><span class="sxs-lookup"><span data-stu-id="0c6f7-110">Activity reports</span></span>
3.  <span data-ttu-id="0c6f7-111">Sestavy integrované aplikace</span><span class="sxs-lookup"><span data-stu-id="0c6f7-111">Integrated app reports</span></span>

### <a name="activity-and-integrated-app-reports"></a><span data-ttu-id="0c6f7-112">Aktivity a integrované aplikace sestavy</span><span class="sxs-lookup"><span data-stu-id="0c6f7-112">Activity and integrated app reports</span></span>

<span data-ttu-id="0c6f7-113">Na základě kontextu vytváření sestav v hello portálu Azure, existující sestavy jsou sloučeny do jednoho zobrazení.</span><span class="sxs-lookup"><span data-stu-id="0c6f7-113">For context-based reporting in hello Azure portal, existing reports are merged into a single view.</span></span> <span data-ttu-id="0c6f7-114">Jeden, základní API poskytuje zobrazení toohello dat hello.</span><span class="sxs-lookup"><span data-stu-id="0c6f7-114">A single, underlying API provides hello data toohello view.</span></span>

<span data-ttu-id="0c6f7-115">Toto zobrazení na hello toosee **Azure Active Directory** okno, v části **aktivity**, vyberte **protokoly auditu**.</span><span class="sxs-lookup"><span data-stu-id="0c6f7-115">toosee this view, on hello **Azure Active Directory** blade, under **ACTIVITY**, select **Audit logs**.</span></span>

<span data-ttu-id="0c6f7-116">![Protokoly auditu](./media/active-directory-reporting-migration/482.png "Protokoly auditu")</span><span class="sxs-lookup"><span data-stu-id="0c6f7-116">![Audit logs](./media/active-directory-reporting-migration/482.png "Audit logs")</span></span>

<span data-ttu-id="0c6f7-117">v tomto zobrazení jsou konsolidovány Hello následující sestavy:</span><span class="sxs-lookup"><span data-stu-id="0c6f7-117">hello following reports are consolidated in this view:</span></span>

-   <span data-ttu-id="0c6f7-118">Sestava auditu</span><span class="sxs-lookup"><span data-stu-id="0c6f7-118">Audit report</span></span>
-   <span data-ttu-id="0c6f7-119">Aktivity resetování hesla</span><span class="sxs-lookup"><span data-stu-id="0c6f7-119">Password reset activity</span></span>
-   <span data-ttu-id="0c6f7-120">Registrace aktivita resetování hesla</span><span class="sxs-lookup"><span data-stu-id="0c6f7-120">Password reset registration activity</span></span>
-   <span data-ttu-id="0c6f7-121">Aktivita samoobslužných skupin</span><span class="sxs-lookup"><span data-stu-id="0c6f7-121">Self-service groups activity</span></span>
-   <span data-ttu-id="0c6f7-122">Změny názvu skupiny Office 365</span><span class="sxs-lookup"><span data-stu-id="0c6f7-122">Office365 Group Name Changes</span></span>
-   <span data-ttu-id="0c6f7-123">Účet zřizování aktivity</span><span class="sxs-lookup"><span data-stu-id="0c6f7-123">Account provisioning activity</span></span>
-   <span data-ttu-id="0c6f7-124">Stav Změna hesla</span><span class="sxs-lookup"><span data-stu-id="0c6f7-124">Password rollover status</span></span>
-   <span data-ttu-id="0c6f7-125">Chyby zřizování účtů</span><span class="sxs-lookup"><span data-stu-id="0c6f7-125">Account provisioning errors</span></span>


<span data-ttu-id="0c6f7-126">Hello sestav využití aplikace je vylepšený a je součástí hello **přihlášení** zobrazení.</span><span class="sxs-lookup"><span data-stu-id="0c6f7-126">hello Application Usage report has been enhanced and is included in hello **Sign-ins** view.</span></span> <span data-ttu-id="0c6f7-127">Toto zobrazení na hello toosee **Azure Active Directory** okno, v části **aktivity**, vyberte **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="0c6f7-127">toosee this view, on hello **Azure Active Directory** blade, under **ACTIVITY**, select **Sign-ins**.</span></span>

<span data-ttu-id="0c6f7-128">![Zobrazení přihlášení](./media/active-directory-reporting-migration/483.png "zobrazení přihlášení")</span><span class="sxs-lookup"><span data-stu-id="0c6f7-128">![Sign-ins view](./media/active-directory-reporting-migration/483.png "Sign-ins view")</span></span>

<span data-ttu-id="0c6f7-129">Hello **přihlášení** zobrazení zahrnuje všechny přihlášení uživatele. Můžete použít informace o použití této informace tooget aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0c6f7-129">hello **Sign-ins** view includes all user sign-ins. You can use this information tooget application usage information.</span></span> <span data-ttu-id="0c6f7-130">Také můžete zobrazit informace o použití aplikace v hello **podnikové aplikace, které** přehled v hello **SPRAVOVAT** části.</span><span class="sxs-lookup"><span data-stu-id="0c6f7-130">You also can view application usage information in hello **Enterprise applications** overview, in hello **MANAGE** section.</span></span>

<span data-ttu-id="0c6f7-131">![Podnikové aplikace, které](./media/active-directory-reporting-migration/484.png "podnikové aplikace")</span><span class="sxs-lookup"><span data-stu-id="0c6f7-131">![Enterprise applications](./media/active-directory-reporting-migration/484.png "Enterprise applications")</span></span>

## <a name="access-a-specific-report"></a><span data-ttu-id="0c6f7-132">Přístup k určité sestavy</span><span class="sxs-lookup"><span data-stu-id="0c6f7-132">Access a specific report</span></span>

<span data-ttu-id="0c6f7-133">I když hello portál Azure nabízí jediné zobrazení, také můžete si prohlédnout konkrétní sestavy.</span><span class="sxs-lookup"><span data-stu-id="0c6f7-133">Although hello Azure portal offers a single view, you also can look at specific reports.</span></span>

### <a name="audit-logs"></a><span data-ttu-id="0c6f7-134">Protokoly auditu</span><span class="sxs-lookup"><span data-stu-id="0c6f7-134">Audit logs</span></span>

<span data-ttu-id="0c6f7-135">V odpovědi toocustomer zpětnou vazbu v hello portál Azure, můžete použít rozšířené filtrování tooaccess hello dat, které chcete.</span><span class="sxs-lookup"><span data-stu-id="0c6f7-135">In response toocustomer feedback, in hello Azure portal, you can use advanced filtering tooaccess hello data you want.</span></span> <span data-ttu-id="0c6f7-136">Je jeden filtr, můžete použít *kategorie aktivity*, který uvádí různé typy aktivit hello protokoly ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0c6f7-136">One filter you can use is an *activity category*, which lists hello different types of activity logs in Azure AD.</span></span> <span data-ttu-id="0c6f7-137">toonarrow výsledků toowhat, které hledáte, můžete vybrat kategorii.</span><span class="sxs-lookup"><span data-stu-id="0c6f7-137">toonarrow results toowhat you are looking for, you can select a category.</span></span>

<span data-ttu-id="0c6f7-138">Například pokud vás zajímá jenom v resetování hesla související služby tooself aktivity, můžete zvolit hello **Samoobslužná správa hesel** kategorie.</span><span class="sxs-lookup"><span data-stu-id="0c6f7-138">For example, if you are interested only in activities related tooself-service password resets, you can choose hello **Self-service Password Management** category.</span></span> <span data-ttu-id="0c6f7-139">Hello kategorií, které vidíte jsou založené na hello prostředků, které pracují v.</span><span class="sxs-lookup"><span data-stu-id="0c6f7-139">hello categories you see are based on hello resource you are working in.</span></span>  

<span data-ttu-id="0c6f7-140">![Kategorie možnosti na stránce protokoly auditu filtru hello](./media/active-directory-reporting-migration/06.png "kategorie možnosti na stránce protokoly auditu filtru hello")</span><span class="sxs-lookup"><span data-stu-id="0c6f7-140">![Category options on hello Filter Audit Logs page](./media/active-directory-reporting-migration/06.png "Category options on hello Filter Audit Logs page")</span></span>

<span data-ttu-id="0c6f7-141">Kategorie aktivit patří:</span><span class="sxs-lookup"><span data-stu-id="0c6f7-141">Activity categories include:</span></span>

- <span data-ttu-id="0c6f7-142">Základní adresář</span><span class="sxs-lookup"><span data-stu-id="0c6f7-142">Core Directory</span></span>
- <span data-ttu-id="0c6f7-143">Samoobslužná správa hesel</span><span class="sxs-lookup"><span data-stu-id="0c6f7-143">Self-service Password Management</span></span>
- <span data-ttu-id="0c6f7-144">Samoobslužná správa skupin</span><span class="sxs-lookup"><span data-stu-id="0c6f7-144">Self-service Group Management</span></span>
- <span data-ttu-id="0c6f7-145">Zřizování účtů</span><span class="sxs-lookup"><span data-stu-id="0c6f7-145">Account Provisioning</span></span>

### <a name="application-usage"></a><span data-ttu-id="0c6f7-146">Využití aplikací</span><span class="sxs-lookup"><span data-stu-id="0c6f7-146">Application usage</span></span>

<span data-ttu-id="0c6f7-147">tooview podrobnosti o použití aplikací pro všechny aplikace nebo pro jenom jedna aplikace, v části **aktivity**, vyberte **přihlášení**. toonarrow hello výsledky můžete filtrovat podle uživatelské jméno nebo název aplikace.</span><span class="sxs-lookup"><span data-stu-id="0c6f7-147">tooview details about application usage for all apps or for a single app, under **ACTIVITY**, select **Sign-ins**. toonarrow hello results, you can filter on user name or application name.</span></span>

<span data-ttu-id="0c6f7-148">![Stránka filtru událostí přihlášení](./media/active-directory-reporting-migration/07.png "události přihlášení filtru stránky")</span><span class="sxs-lookup"><span data-stu-id="0c6f7-148">![Filter Sign-In Events page](./media/active-directory-reporting-migration/07.png "Filter Sign-In Events page")</span></span>

### <a name="security-reports"></a><span data-ttu-id="0c6f7-149">Sestavy zabezpečení</span><span class="sxs-lookup"><span data-stu-id="0c6f7-149">Security reports</span></span>

#### <a name="azure-ad-anomalous-activity-reports"></a><span data-ttu-id="0c6f7-150">Azure AD neobvyklé aktivity sestavy</span><span class="sxs-lookup"><span data-stu-id="0c6f7-150">Azure AD anomalous activity reports</span></span>

<span data-ttu-id="0c6f7-151">Zabezpečení Azure AD neobvyklé aktivity sestavy z portálu Azure classic hello byly konsolidované tooprovide můžete pomocí nich centrální zobrazení.</span><span class="sxs-lookup"><span data-stu-id="0c6f7-151">Azure AD anomalous activity security reports from hello Azure classic portal have been consolidated tooprovide you with one, central view.</span></span> <span data-ttu-id="0c6f7-152">Toto zobrazení uvádí všechny události související se zabezpečením riziko, že Azure AD můžete zjišťovat a tvorba sestav o.</span><span class="sxs-lookup"><span data-stu-id="0c6f7-152">This view shows all security-related risk events that Azure AD can detect and report on.</span></span>

<span data-ttu-id="0c6f7-153">Hello následující tabulka uvádí sestavy zabezpečení neobvyklé aktivity hello Azure AD a odpovídající typy událostí riziko v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0c6f7-153">hello following table lists hello Azure AD anomalous activity security reports, and corresponding risk event types in hello Azure portal.</span></span>

| <span data-ttu-id="0c6f7-154">Sestava neobvyklé aktivity Azure AD</span><span class="sxs-lookup"><span data-stu-id="0c6f7-154">Azure AD anomalous activity report</span></span> |  <span data-ttu-id="0c6f7-155">Typ události riziko ochranu identity</span><span class="sxs-lookup"><span data-stu-id="0c6f7-155">Identity protection risk event type</span></span>|
| :--- | :--- |
| <span data-ttu-id="0c6f7-156">Uživatelé s uniklými přihlašovacími údaji</span><span class="sxs-lookup"><span data-stu-id="0c6f7-156">Users with leaked credentials</span></span> | <span data-ttu-id="0c6f7-157">Uniklé přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="0c6f7-157">Leaked credentials</span></span> |
| <span data-ttu-id="0c6f7-158">Nestandardní přihlašovací aktivita</span><span class="sxs-lookup"><span data-stu-id="0c6f7-158">Irregular sign-in activity</span></span> | <span data-ttu-id="0c6f7-159">Neuskutečnitelná cesta tooatypical umístění</span><span class="sxs-lookup"><span data-stu-id="0c6f7-159">Impossible travel tooatypical locations</span></span> |
| <span data-ttu-id="0c6f7-160">Přihlášení z možných nakažených zařízení</span><span class="sxs-lookup"><span data-stu-id="0c6f7-160">Sign-ins from possibly infected devices</span></span> | <span data-ttu-id="0c6f7-161">Přihlášení z nakažených zařízení</span><span class="sxs-lookup"><span data-stu-id="0c6f7-161">Sign-ins from infected devices</span></span>|
| <span data-ttu-id="0c6f7-162">Přihlášení z neznámých zdrojů</span><span class="sxs-lookup"><span data-stu-id="0c6f7-162">Sign-ins from unknown sources</span></span> | <span data-ttu-id="0c6f7-163">Přihlášení z anonymních IP adres</span><span class="sxs-lookup"><span data-stu-id="0c6f7-163">Sign-ins from anonymous IP addresses</span></span> |
| <span data-ttu-id="0c6f7-164">Přihlášení z IP adres s podezřelou aktivitou</span><span class="sxs-lookup"><span data-stu-id="0c6f7-164">Sign-ins from IP addresses with suspicious activity</span></span> | <span data-ttu-id="0c6f7-165">Přihlášení z IP adres s podezřelou aktivitou</span><span class="sxs-lookup"><span data-stu-id="0c6f7-165">Sign-ins from IP addresses with suspicious activity</span></span> |
| - | <span data-ttu-id="0c6f7-166">Přihlášení z neznámých míst</span><span class="sxs-lookup"><span data-stu-id="0c6f7-166">Sign-ins from unfamiliar locations</span></span> |

<span data-ttu-id="0c6f7-167">Hello následující zabezpečení Azure AD neobvyklé aktivity sestavy nejsou zahrnuta jako riziko události v hello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="0c6f7-167">hello following Azure AD anomalous activity security reports are not included as risk events in hello Azure portal:</span></span>

* <span data-ttu-id="0c6f7-168">Přihlášení po několika neúspěších</span><span class="sxs-lookup"><span data-stu-id="0c6f7-168">Sign-ins after multiple failures</span></span>
* <span data-ttu-id="0c6f7-169">Přihlášení z více geografických poloh</span><span class="sxs-lookup"><span data-stu-id="0c6f7-169">Sign-ins from multiple geographies</span></span>

<span data-ttu-id="0c6f7-170">Tyto sestavy jsou stále k dispozici v hello portál Azure classic, ale přestanou někdy v budoucnu hello.</span><span class="sxs-lookup"><span data-stu-id="0c6f7-170">These reports are still available in hello Azure classic portal, but they will be deprecated at some time in hello future.</span></span>

<span data-ttu-id="0c6f7-171">Další informace najdete v tématu věnovaném [rizikovým událostem služby Azure Active Directory](active-directory-identity-protection-risk-events.md).</span><span class="sxs-lookup"><span data-stu-id="0c6f7-171">For more information, see [Azure Active Directory risk events](active-directory-identity-protection-risk-events.md).</span></span>  


#### <a name="detected-risk-events"></a><span data-ttu-id="0c6f7-172">Zjištěné rizikových událostí</span><span class="sxs-lookup"><span data-stu-id="0c6f7-172">Detected risk events</span></span>

<span data-ttu-id="0c6f7-173">V hello portálu Azure, můžete přistupovat k sestavám o zjištěných rizikových událostí na hello **Azure Active Directory** okno, v části **zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="0c6f7-173">In hello Azure portal, you can access reports about detected risk events on hello **Azure Active Directory** blade, under **SECURITY**.</span></span> <span data-ttu-id="0c6f7-174">Zjištěné rizikových událostí jsou sledovány v hello následující sestavy:</span><span class="sxs-lookup"><span data-stu-id="0c6f7-174">Detected risk events are tracked in hello following reports:</span></span>   

- <span data-ttu-id="0c6f7-175">Ohrožených uživatelích</span><span class="sxs-lookup"><span data-stu-id="0c6f7-175">Users at Risk</span></span>
- <span data-ttu-id="0c6f7-176">Riziková přihlášení</span><span class="sxs-lookup"><span data-stu-id="0c6f7-176">Risky Sign-ins</span></span>

<span data-ttu-id="0c6f7-177">![Sestavy zabezpečení](./media/active-directory-reporting-migration/04.png "sestavy zabezpečení")</span><span class="sxs-lookup"><span data-stu-id="0c6f7-177">![Security reports](./media/active-directory-reporting-migration/04.png "Security reports")</span></span>

<span data-ttu-id="0c6f7-178">Další informace o zabezpečení najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="0c6f7-178">For more information about security reports, see:</span></span>

- [<span data-ttu-id="0c6f7-179">Uživatelé na riziko zabezpečení sestav na portálu Azure Active Directory hello</span><span class="sxs-lookup"><span data-stu-id="0c6f7-179">Users at Risk security report in hello Azure Active Directory portal</span></span>](active-directory-reporting-security-user-at-risk.md)
- [<span data-ttu-id="0c6f7-180">Rizikové přihlášení sestavy v portálu Azure Active Directory hello</span><span class="sxs-lookup"><span data-stu-id="0c6f7-180">Risky Sign-ins report in hello Azure Active Directory portal</span></span>](active-directory-reporting-security-risky-sign-ins.md)


## <a name="activity-reports-in-hello-azure-classic-portal-vs-hello-azure-portal"></a><span data-ttu-id="0c6f7-181">Sestavy aktivit v hello portál Azure classic oproti hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="0c6f7-181">Activity reports in hello Azure classic portal vs. hello Azure portal</span></span>

<span data-ttu-id="0c6f7-182">Tabulka Hello v této části uvádí existující sestavy v hello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="0c6f7-182">hello table in this section lists existing reports in hello Azure classic portal.</span></span> <span data-ttu-id="0c6f7-183">Také popisuje, jak můžete získat hello stejné informace v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0c6f7-183">It also describes how you can get hello same information in hello Azure portal.</span></span>

<span data-ttu-id="0c6f7-184">všechny tooview auditování dat na hello **Azure Active Directory** okno, v části **aktivity**, přejděte příliš**protokoly auditu**.</span><span class="sxs-lookup"><span data-stu-id="0c6f7-184">tooview all auditing data, on hello **Azure Active Directory** blade, under **ACTIVITY**, go too**Audit logs**.</span></span>

<span data-ttu-id="0c6f7-185">![Protokoly auditu](./media/active-directory-reporting-migration/61.png "Protokoly auditu")</span><span class="sxs-lookup"><span data-stu-id="0c6f7-185">![Audit logs](./media/active-directory-reporting-migration/61.png "Audit logs")</span></span>

| <span data-ttu-id="0c6f7-186">klasický portál Azure</span><span class="sxs-lookup"><span data-stu-id="0c6f7-186">Azure classic portal</span></span>                 | <span data-ttu-id="0c6f7-187">toofind v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="0c6f7-187">toofind in hello Azure portal</span></span>                                                         |
| ---                                  | ---                                                                        |
| <span data-ttu-id="0c6f7-188">Protokoly auditu</span><span class="sxs-lookup"><span data-stu-id="0c6f7-188">Audit logs</span></span>                           | <span data-ttu-id="0c6f7-189">Pro **kategorie aktivity**, vyberte **základní Directory**.</span><span class="sxs-lookup"><span data-stu-id="0c6f7-189">For **Activity Category**, select **Core Directory**.</span></span>                       |
| <span data-ttu-id="0c6f7-190">Aktivity resetování hesla</span><span class="sxs-lookup"><span data-stu-id="0c6f7-190">Password reset activity</span></span>              | <span data-ttu-id="0c6f7-191">Pro **kategorie aktivity**, vyberte **Samoobslužná správa hesel**.</span><span class="sxs-lookup"><span data-stu-id="0c6f7-191">For **Activity Category**, select **Self-service Password Management**.</span></span> |
| <span data-ttu-id="0c6f7-192">Registrace aktivita resetování hesla</span><span class="sxs-lookup"><span data-stu-id="0c6f7-192">Password reset registration activity</span></span> | <span data-ttu-id="0c6f7-193">Pro **kategorie aktivity**, vyberte **Samoobslužná správa hesel**.</span><span class="sxs-lookup"><span data-stu-id="0c6f7-193">For **Activity Category**, select **Self-service Password Management**.</span></span>     |
| <span data-ttu-id="0c6f7-194">Aktivita samoobslužných skupin</span><span class="sxs-lookup"><span data-stu-id="0c6f7-194">Self-service groups activity</span></span>         | <span data-ttu-id="0c6f7-195">Pro **kategorie aktivity**, vyberte **Samoobslužná správa skupiny**.</span><span class="sxs-lookup"><span data-stu-id="0c6f7-195">For **Activity Category**, select **Self-service Group Management**.</span></span>        |
| <span data-ttu-id="0c6f7-196">Účet zřizování aktivity</span><span class="sxs-lookup"><span data-stu-id="0c6f7-196">Account provisioning activity</span></span>        | <span data-ttu-id="0c6f7-197">Pro **kategorie aktivity**, vyberte **zřizování účtu uživatele**.</span><span class="sxs-lookup"><span data-stu-id="0c6f7-197">For **Activity Category**, select **Account User Provisioning**.</span></span>         |
| <span data-ttu-id="0c6f7-198">Stav Změna hesla</span><span class="sxs-lookup"><span data-stu-id="0c6f7-198">Password rollover status</span></span>             | <span data-ttu-id="0c6f7-199">Pro **kategorie aktivity**, vyberte **Automatická změna hesla aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0c6f7-199">For **Activity Category**, select **Automatic App Password Rollover**.</span></span>      |
| <span data-ttu-id="0c6f7-200">Chyby zřizování účtů</span><span class="sxs-lookup"><span data-stu-id="0c6f7-200">Account provisioning errors</span></span>          | <span data-ttu-id="0c6f7-201">Pro **kategorie aktivity**, vyberte **zřizování účtu uživatele**.</span><span class="sxs-lookup"><span data-stu-id="0c6f7-201">For **Activity Category**, select **Account User Provisioning**.</span></span>        |
| <span data-ttu-id="0c6f7-202">Změny názvu skupiny Office 365</span><span class="sxs-lookup"><span data-stu-id="0c6f7-202">Office365 Group Name Changes</span></span>         | <span data-ttu-id="0c6f7-203">Pro **kategorie aktivity**, vyberte **Samoobslužná správa hesel**.</span><span class="sxs-lookup"><span data-stu-id="0c6f7-203">For **Activity Category**, select **Self-service Password Management**.</span></span> <span data-ttu-id="0c6f7-204">Pro **typ prostředku aktivity**, vyberte **skupiny**.</span><span class="sxs-lookup"><span data-stu-id="0c6f7-204">For **Activity Resource Type**, select **Group**.</span></span> <span data-ttu-id="0c6f7-205">Pro **zdroj aktivity**, vyberte **O365 skupiny**.</span><span class="sxs-lookup"><span data-stu-id="0c6f7-205">For **Activity Source**, select **O365 groups**.</span></span>|

<span data-ttu-id="0c6f7-206">tooview hello **využití aplikace** zprávu o hello **Azure Active Directory** okno, v části **SPRAVOVAT**, vyberte **podnikové aplikace, které**a potom vyberte **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="0c6f7-206">tooview hello **Application Usage** report, on hello **Azure Active Directory** blade, under **MANAGE**, select **Enterprise Applications**, and then select **Sign-ins**.</span></span>


<span data-ttu-id="0c6f7-207">![Sestava podnikové aplikace přihlášení](./media/active-directory-reporting-migration/199.png "podnikové aplikace přihlášení sestavy")</span><span class="sxs-lookup"><span data-stu-id="0c6f7-207">![Enterprise Applications Sign-Ins report](./media/active-directory-reporting-migration/199.png "Enterprise Applications Sign-Ins report")</span></span>

## <a name="next-steps"></a><span data-ttu-id="0c6f7-208">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0c6f7-208">Next steps</span></span>

<span data-ttu-id="0c6f7-209">Přehled vytváření sestav najdete v tématu hello [generování sestav Azure Active Directory](active-directory-reporting-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="0c6f7-209">For an overview of reporting, see hello [Azure Active Directory reporting](active-directory-reporting-azure-portal.md).</span></span>
