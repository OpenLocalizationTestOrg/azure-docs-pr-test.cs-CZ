---
title: "Najít sestavy aktivity na portálu Azure | Microsoft Docs"
description: "Zjistěte, jak najít sestavy aktivita služby Azure Active Directory na portálu Azure."
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
ms.openlocfilehash: f1875582476c3817b9eb0082b6548cc15043cb98
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="find-activity-reports-in-the-azure-portal"></a><span data-ttu-id="36e54-103">Najít sestavy aktivity na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="36e54-103">Find activity reports in the Azure portal</span></span>

<span data-ttu-id="36e54-104">Pokud přecházíte z portálu Azure classic na portálu Azure, můžete získat nový pohled na protokoly aktivity Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="36e54-104">If you are moving from the Azure classic portal to the Azure portal, you get a new look at Azure Active Directory (Azure AD) activity logs.</span></span> <span data-ttu-id="36e54-105">V poslední [příspěvku na blogu](https://blogs.technet.microsoft.com/enterprisemobility/2016/11/08/azuread-weve-just-turned-on-detailed-auditing-and-sign-in-logs-in-the-new-azure-portal/), vám vysvětlíme, jak vidíte aktivitu protokolů v rámci prostředků pracujete na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="36e54-105">In a recent [blog post](https://blogs.technet.microsoft.com/enterprisemobility/2016/11/08/azuread-weve-just-turned-on-detailed-auditing-and-sign-in-logs-in-the-new-azure-portal/), we explain how you can see activity logs in the context of the resource you are working on in the Azure portal.</span></span> <span data-ttu-id="36e54-106">V tomto článku jsme popisují, jak najít sestavy, které jste použili na portálu Azure classic na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="36e54-106">In this article, we describe how to find reports that you used in the Azure classic portal in the Azure portal.</span></span>

## <a name="whats-new"></a><span data-ttu-id="36e54-107">Co je nového</span><span class="sxs-lookup"><span data-stu-id="36e54-107">What's new</span></span>

<span data-ttu-id="36e54-108">Sestavy v portálu Azure classic jsou rozdělené do kategorií:</span><span class="sxs-lookup"><span data-stu-id="36e54-108">Reports in the Azure classic portal are separated into categories:</span></span>

1.  <span data-ttu-id="36e54-109">Sestavy zabezpečení</span><span class="sxs-lookup"><span data-stu-id="36e54-109">Security reports</span></span>
2.  <span data-ttu-id="36e54-110">Sestavy aktivit</span><span class="sxs-lookup"><span data-stu-id="36e54-110">Activity reports</span></span>
3.  <span data-ttu-id="36e54-111">Sestavy integrované aplikace</span><span class="sxs-lookup"><span data-stu-id="36e54-111">Integrated app reports</span></span>

### <a name="activity-and-integrated-app-reports"></a><span data-ttu-id="36e54-112">Aktivity a integrované aplikace sestavy</span><span class="sxs-lookup"><span data-stu-id="36e54-112">Activity and integrated app reports</span></span>

<span data-ttu-id="36e54-113">Na základě kontextu vytváření sestav v portálu Azure, existující sestavy jsou sloučeny do jednoho zobrazení.</span><span class="sxs-lookup"><span data-stu-id="36e54-113">For context-based reporting in the Azure portal, existing reports are merged into a single view.</span></span> <span data-ttu-id="36e54-114">Jedinou základní rozhraní API poskytuje data k zobrazení.</span><span class="sxs-lookup"><span data-stu-id="36e54-114">A single, underlying API provides the data to the view.</span></span>

<span data-ttu-id="36e54-115">V tomto zobrazení **Azure Active Directory** okno, v části **aktivity**, vyberte **protokoly auditu**.</span><span class="sxs-lookup"><span data-stu-id="36e54-115">To see this view, on the **Azure Active Directory** blade, under **ACTIVITY**, select **Audit logs**.</span></span>

<span data-ttu-id="36e54-116">![Protokoly auditu](./media/active-directory-reporting-migration/482.png "Protokoly auditu")</span><span class="sxs-lookup"><span data-stu-id="36e54-116">![Audit logs](./media/active-directory-reporting-migration/482.png "Audit logs")</span></span>

<span data-ttu-id="36e54-117">V tomto zobrazení jsou konsolidovány následující sestavy:</span><span class="sxs-lookup"><span data-stu-id="36e54-117">The following reports are consolidated in this view:</span></span>

-   <span data-ttu-id="36e54-118">Sestava auditu</span><span class="sxs-lookup"><span data-stu-id="36e54-118">Audit report</span></span>
-   <span data-ttu-id="36e54-119">Aktivity resetování hesla</span><span class="sxs-lookup"><span data-stu-id="36e54-119">Password reset activity</span></span>
-   <span data-ttu-id="36e54-120">Registrace aktivita resetování hesla</span><span class="sxs-lookup"><span data-stu-id="36e54-120">Password reset registration activity</span></span>
-   <span data-ttu-id="36e54-121">Aktivita samoobslužných skupin</span><span class="sxs-lookup"><span data-stu-id="36e54-121">Self-service groups activity</span></span>
-   <span data-ttu-id="36e54-122">Změny názvu skupiny Office 365</span><span class="sxs-lookup"><span data-stu-id="36e54-122">Office365 Group Name Changes</span></span>
-   <span data-ttu-id="36e54-123">Účet zřizování aktivity</span><span class="sxs-lookup"><span data-stu-id="36e54-123">Account provisioning activity</span></span>
-   <span data-ttu-id="36e54-124">Stav Změna hesla</span><span class="sxs-lookup"><span data-stu-id="36e54-124">Password rollover status</span></span>
-   <span data-ttu-id="36e54-125">Chyby zřizování účtů</span><span class="sxs-lookup"><span data-stu-id="36e54-125">Account provisioning errors</span></span>


<span data-ttu-id="36e54-126">Sestava využití aplikace je vylepšený a je součástí **přihlášení** zobrazení.</span><span class="sxs-lookup"><span data-stu-id="36e54-126">The Application Usage report has been enhanced and is included in the **Sign-ins** view.</span></span> <span data-ttu-id="36e54-127">V tomto zobrazení **Azure Active Directory** okno, v části **aktivity**, vyberte **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="36e54-127">To see this view, on the **Azure Active Directory** blade, under **ACTIVITY**, select **Sign-ins**.</span></span>

<span data-ttu-id="36e54-128">![Zobrazení přihlášení](./media/active-directory-reporting-migration/483.png "zobrazení přihlášení")</span><span class="sxs-lookup"><span data-stu-id="36e54-128">![Sign-ins view](./media/active-directory-reporting-migration/483.png "Sign-ins view")</span></span>

<span data-ttu-id="36e54-129">**Přihlášení** zobrazení zahrnuje všechny přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="36e54-129">The **Sign-ins** view includes all user sign-ins.</span></span> <span data-ttu-id="36e54-130">Tyto informace můžete získat informace o využití aplikace.</span><span class="sxs-lookup"><span data-stu-id="36e54-130">You can use this information to get application usage information.</span></span> <span data-ttu-id="36e54-131">Také můžete zobrazit informace o použití aplikace v **podnikové aplikace, které** přehled v **SPRAVOVAT** části.</span><span class="sxs-lookup"><span data-stu-id="36e54-131">You also can view application usage information in the **Enterprise applications** overview, in the **MANAGE** section.</span></span>

<span data-ttu-id="36e54-132">![Podnikové aplikace, které](./media/active-directory-reporting-migration/484.png "podnikové aplikace")</span><span class="sxs-lookup"><span data-stu-id="36e54-132">![Enterprise applications](./media/active-directory-reporting-migration/484.png "Enterprise applications")</span></span>

## <a name="access-a-specific-report"></a><span data-ttu-id="36e54-133">Přístup k určité sestavy</span><span class="sxs-lookup"><span data-stu-id="36e54-133">Access a specific report</span></span>

<span data-ttu-id="36e54-134">I když na portál Azure nabízí jediné zobrazení, také můžete si prohlédnout konkrétní sestavy.</span><span class="sxs-lookup"><span data-stu-id="36e54-134">Although the Azure portal offers a single view, you also can look at specific reports.</span></span>

### <a name="audit-logs"></a><span data-ttu-id="36e54-135">Protokoly auditu</span><span class="sxs-lookup"><span data-stu-id="36e54-135">Audit logs</span></span>

<span data-ttu-id="36e54-136">V reakci na názory zákazníků na portálu Azure můžete upřesnit filtrování pro přístup k datům, které chcete.</span><span class="sxs-lookup"><span data-stu-id="36e54-136">In response to customer feedback, in the Azure portal, you can use advanced filtering to access the data you want.</span></span> <span data-ttu-id="36e54-137">Je jeden filtr, můžete použít *kategorie aktivity*, který uvádí různé typy aktivit protokoly ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="36e54-137">One filter you can use is an *activity category*, which lists the different types of activity logs in Azure AD.</span></span> <span data-ttu-id="36e54-138">Chcete-li zúžit výsledky co hledáte, můžete vybrat kategorii.</span><span class="sxs-lookup"><span data-stu-id="36e54-138">To narrow results to what you are looking for, you can select a category.</span></span>

<span data-ttu-id="36e54-139">Například pokud vás zajímá jenom v činnosti týkající se resetování hesla pomocí samoobslužné služby, můžete **Samoobslužná správa hesel** kategorie.</span><span class="sxs-lookup"><span data-stu-id="36e54-139">For example, if you are interested only in activities related to self-service password resets, you can choose the **Self-service Password Management** category.</span></span> <span data-ttu-id="36e54-140">Kategorie, které vidíte jsou založené na prostředek, který pracujete v.</span><span class="sxs-lookup"><span data-stu-id="36e54-140">The categories you see are based on the resource you are working in.</span></span>  

<span data-ttu-id="36e54-141">![Kategorie možnosti na stránce protokoly auditu filtru](./media/active-directory-reporting-migration/06.png "kategorie možnosti na stránce protokoly auditu filtru")</span><span class="sxs-lookup"><span data-stu-id="36e54-141">![Category options on the Filter Audit Logs page](./media/active-directory-reporting-migration/06.png "Category options on the Filter Audit Logs page")</span></span>

<span data-ttu-id="36e54-142">Kategorie aktivit patří:</span><span class="sxs-lookup"><span data-stu-id="36e54-142">Activity categories include:</span></span>

- <span data-ttu-id="36e54-143">Základní adresář</span><span class="sxs-lookup"><span data-stu-id="36e54-143">Core Directory</span></span>
- <span data-ttu-id="36e54-144">Samoobslužná správa hesel</span><span class="sxs-lookup"><span data-stu-id="36e54-144">Self-service Password Management</span></span>
- <span data-ttu-id="36e54-145">Samoobslužná správa skupin</span><span class="sxs-lookup"><span data-stu-id="36e54-145">Self-service Group Management</span></span>
- <span data-ttu-id="36e54-146">Zřizování účtů</span><span class="sxs-lookup"><span data-stu-id="36e54-146">Account Provisioning</span></span>

### <a name="application-usage"></a><span data-ttu-id="36e54-147">Využití aplikací</span><span class="sxs-lookup"><span data-stu-id="36e54-147">Application usage</span></span>

<span data-ttu-id="36e54-148">Chcete-li zobrazit podrobnosti o použití aplikací pro všechny aplikace nebo pro jenom jedna aplikace, v části **aktivity**, vyberte **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="36e54-148">To view details about application usage for all apps or for a single app, under **ACTIVITY**, select **Sign-ins**.</span></span> <span data-ttu-id="36e54-149">Chcete-li zúžit výsledky, můžete filtrovat podle uživatelské jméno nebo název aplikace.</span><span class="sxs-lookup"><span data-stu-id="36e54-149">To narrow the results, you can filter on user name or application name.</span></span>

<span data-ttu-id="36e54-150">![Stránka filtru událostí přihlášení](./media/active-directory-reporting-migration/07.png "události přihlášení filtru stránky")</span><span class="sxs-lookup"><span data-stu-id="36e54-150">![Filter Sign-In Events page](./media/active-directory-reporting-migration/07.png "Filter Sign-In Events page")</span></span>

### <a name="security-reports"></a><span data-ttu-id="36e54-151">Sestavy zabezpečení</span><span class="sxs-lookup"><span data-stu-id="36e54-151">Security reports</span></span>

#### <a name="azure-ad-anomalous-activity-reports"></a><span data-ttu-id="36e54-152">Azure AD neobvyklé aktivity sestavy</span><span class="sxs-lookup"><span data-stu-id="36e54-152">Azure AD anomalous activity reports</span></span>

<span data-ttu-id="36e54-153">Zabezpečení Azure AD neobvyklé aktivity konsolidovaný sestavy z portálu Azure classic, kde přinášejí jedno, centrální zobrazení.</span><span class="sxs-lookup"><span data-stu-id="36e54-153">Azure AD anomalous activity security reports from the Azure classic portal have been consolidated to provide you with one, central view.</span></span> <span data-ttu-id="36e54-154">Toto zobrazení uvádí všechny události související se zabezpečením riziko, že Azure AD můžete zjišťovat a tvorba sestav o.</span><span class="sxs-lookup"><span data-stu-id="36e54-154">This view shows all security-related risk events that Azure AD can detect and report on.</span></span>

<span data-ttu-id="36e54-155">Následující tabulka seznamy Azure AD neobvyklé aktivity zabezpečení sestavy a odpovídající typy událostí riziko na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="36e54-155">The following table lists the Azure AD anomalous activity security reports, and corresponding risk event types in the Azure portal.</span></span>

| <span data-ttu-id="36e54-156">Sestava neobvyklé aktivity Azure AD</span><span class="sxs-lookup"><span data-stu-id="36e54-156">Azure AD anomalous activity report</span></span> |  <span data-ttu-id="36e54-157">Typ události riziko ochranu identity</span><span class="sxs-lookup"><span data-stu-id="36e54-157">Identity protection risk event type</span></span>|
| :--- | :--- |
| <span data-ttu-id="36e54-158">Uživatelé s uniklými přihlašovacími údaji</span><span class="sxs-lookup"><span data-stu-id="36e54-158">Users with leaked credentials</span></span> | <span data-ttu-id="36e54-159">Uniklé přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="36e54-159">Leaked credentials</span></span> |
| <span data-ttu-id="36e54-160">Nestandardní přihlašovací aktivita</span><span class="sxs-lookup"><span data-stu-id="36e54-160">Irregular sign-in activity</span></span> | <span data-ttu-id="36e54-161">Nemožná cesta do netypických míst</span><span class="sxs-lookup"><span data-stu-id="36e54-161">Impossible travel to atypical locations</span></span> |
| <span data-ttu-id="36e54-162">Přihlášení z možných nakažených zařízení</span><span class="sxs-lookup"><span data-stu-id="36e54-162">Sign-ins from possibly infected devices</span></span> | <span data-ttu-id="36e54-163">Přihlášení z nakažených zařízení</span><span class="sxs-lookup"><span data-stu-id="36e54-163">Sign-ins from infected devices</span></span>|
| <span data-ttu-id="36e54-164">Přihlášení z neznámých zdrojů</span><span class="sxs-lookup"><span data-stu-id="36e54-164">Sign-ins from unknown sources</span></span> | <span data-ttu-id="36e54-165">Přihlášení z anonymních IP adres</span><span class="sxs-lookup"><span data-stu-id="36e54-165">Sign-ins from anonymous IP addresses</span></span> |
| <span data-ttu-id="36e54-166">Přihlášení z IP adres s podezřelou aktivitou</span><span class="sxs-lookup"><span data-stu-id="36e54-166">Sign-ins from IP addresses with suspicious activity</span></span> | <span data-ttu-id="36e54-167">Přihlášení z IP adres s podezřelou aktivitou</span><span class="sxs-lookup"><span data-stu-id="36e54-167">Sign-ins from IP addresses with suspicious activity</span></span> |
| - | <span data-ttu-id="36e54-168">Přihlášení z neznámých míst</span><span class="sxs-lookup"><span data-stu-id="36e54-168">Sign-ins from unfamiliar locations</span></span> |

<span data-ttu-id="36e54-169">Následující zabezpečení Azure AD neobvyklé aktivity sestavy nejsou zahrnuta jako riziko události na portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="36e54-169">The following Azure AD anomalous activity security reports are not included as risk events in the Azure portal:</span></span>

* <span data-ttu-id="36e54-170">Přihlášení po několika neúspěších</span><span class="sxs-lookup"><span data-stu-id="36e54-170">Sign-ins after multiple failures</span></span>
* <span data-ttu-id="36e54-171">Přihlášení z více geografických poloh</span><span class="sxs-lookup"><span data-stu-id="36e54-171">Sign-ins from multiple geographies</span></span>

<span data-ttu-id="36e54-172">Tyto sestavy jsou stále k dispozici na portálu Azure classic, ale přestanou někdy v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="36e54-172">These reports are still available in the Azure classic portal, but they will be deprecated at some time in the future.</span></span>

<span data-ttu-id="36e54-173">Další informace najdete v tématu věnovaném [rizikovým událostem služby Azure Active Directory](active-directory-identity-protection-risk-events.md).</span><span class="sxs-lookup"><span data-stu-id="36e54-173">For more information, see [Azure Active Directory risk events](active-directory-identity-protection-risk-events.md).</span></span>  


#### <a name="detected-risk-events"></a><span data-ttu-id="36e54-174">Zjištěné rizikových událostí</span><span class="sxs-lookup"><span data-stu-id="36e54-174">Detected risk events</span></span>

<span data-ttu-id="36e54-175">Na portálu Azure, je k dispozici sestavy o zjištěných rizikových událostí na **Azure Active Directory** okno, v části **zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="36e54-175">In the Azure portal, you can access reports about detected risk events on the **Azure Active Directory** blade, under **SECURITY**.</span></span> <span data-ttu-id="36e54-176">Zjištěné rizikových událostí jsou sledovány v následující sestavy:</span><span class="sxs-lookup"><span data-stu-id="36e54-176">Detected risk events are tracked in the following reports:</span></span>   

- <span data-ttu-id="36e54-177">Ohrožených uživatelích</span><span class="sxs-lookup"><span data-stu-id="36e54-177">Users at Risk</span></span>
- <span data-ttu-id="36e54-178">Riziková přihlášení</span><span class="sxs-lookup"><span data-stu-id="36e54-178">Risky Sign-ins</span></span>

<span data-ttu-id="36e54-179">![Sestavy zabezpečení](./media/active-directory-reporting-migration/04.png "sestavy zabezpečení")</span><span class="sxs-lookup"><span data-stu-id="36e54-179">![Security reports](./media/active-directory-reporting-migration/04.png "Security reports")</span></span>

<span data-ttu-id="36e54-180">Další informace o zabezpečení najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="36e54-180">For more information about security reports, see:</span></span>

- [<span data-ttu-id="36e54-181">Uživatelé na riziko zabezpečení sestav na portálu Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="36e54-181">Users at Risk security report in the Azure Active Directory portal</span></span>](active-directory-reporting-security-user-at-risk.md)
- [<span data-ttu-id="36e54-182">Rizikové přihlášení sestav na portálu Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="36e54-182">Risky Sign-ins report in the Azure Active Directory portal</span></span>](active-directory-reporting-security-risky-sign-ins.md)


## <a name="activity-reports-in-the-azure-classic-portal-vs-the-azure-portal"></a><span data-ttu-id="36e54-183">Sestavy aktivit v portálu Azure classic oproti portálu Azure</span><span class="sxs-lookup"><span data-stu-id="36e54-183">Activity reports in the Azure classic portal vs. the Azure portal</span></span>

<span data-ttu-id="36e54-184">V tabulce v této části jsou uvedeny stávajících sestav na portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="36e54-184">The table in this section lists existing reports in the Azure classic portal.</span></span> <span data-ttu-id="36e54-185">Také popisuje, jak stejné informace můžete získat na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="36e54-185">It also describes how you can get the same information in the Azure portal.</span></span>

<span data-ttu-id="36e54-186">Pokud chcete zobrazit všechna data auditu, na **Azure Active Directory** okno, v části **aktivity**, přejděte na **protokoly auditu**.</span><span class="sxs-lookup"><span data-stu-id="36e54-186">To view all auditing data, on the **Azure Active Directory** blade, under **ACTIVITY**, go to **Audit logs**.</span></span>

<span data-ttu-id="36e54-187">![Protokoly auditu](./media/active-directory-reporting-migration/61.png "Protokoly auditu")</span><span class="sxs-lookup"><span data-stu-id="36e54-187">![Audit logs](./media/active-directory-reporting-migration/61.png "Audit logs")</span></span>

| <span data-ttu-id="36e54-188">klasický portál Azure</span><span class="sxs-lookup"><span data-stu-id="36e54-188">Azure classic portal</span></span>                 | <span data-ttu-id="36e54-189">Najít na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="36e54-189">To find in the Azure portal</span></span>                                                         |
| ---                                  | ---                                                                        |
| <span data-ttu-id="36e54-190">Protokoly auditu</span><span class="sxs-lookup"><span data-stu-id="36e54-190">Audit logs</span></span>                           | <span data-ttu-id="36e54-191">Pro **kategorie aktivity**, vyberte **základní Directory**.</span><span class="sxs-lookup"><span data-stu-id="36e54-191">For **Activity Category**, select **Core Directory**.</span></span>                       |
| <span data-ttu-id="36e54-192">Aktivity resetování hesla</span><span class="sxs-lookup"><span data-stu-id="36e54-192">Password reset activity</span></span>              | <span data-ttu-id="36e54-193">Pro **kategorie aktivity**, vyberte **Samoobslužná správa hesel**.</span><span class="sxs-lookup"><span data-stu-id="36e54-193">For **Activity Category**, select **Self-service Password Management**.</span></span> |
| <span data-ttu-id="36e54-194">Registrace aktivita resetování hesla</span><span class="sxs-lookup"><span data-stu-id="36e54-194">Password reset registration activity</span></span> | <span data-ttu-id="36e54-195">Pro **kategorie aktivity**, vyberte **Samoobslužná správa hesel**.</span><span class="sxs-lookup"><span data-stu-id="36e54-195">For **Activity Category**, select **Self-service Password Management**.</span></span>     |
| <span data-ttu-id="36e54-196">Aktivita samoobslužných skupin</span><span class="sxs-lookup"><span data-stu-id="36e54-196">Self-service groups activity</span></span>         | <span data-ttu-id="36e54-197">Pro **kategorie aktivity**, vyberte **Samoobslužná správa skupiny**.</span><span class="sxs-lookup"><span data-stu-id="36e54-197">For **Activity Category**, select **Self-service Group Management**.</span></span>        |
| <span data-ttu-id="36e54-198">Účet zřizování aktivity</span><span class="sxs-lookup"><span data-stu-id="36e54-198">Account provisioning activity</span></span>        | <span data-ttu-id="36e54-199">Pro **kategorie aktivity**, vyberte **zřizování účtu uživatele**.</span><span class="sxs-lookup"><span data-stu-id="36e54-199">For **Activity Category**, select **Account User Provisioning**.</span></span>         |
| <span data-ttu-id="36e54-200">Stav Změna hesla</span><span class="sxs-lookup"><span data-stu-id="36e54-200">Password rollover status</span></span>             | <span data-ttu-id="36e54-201">Pro **kategorie aktivity**, vyberte **Automatická změna hesla aplikace**.</span><span class="sxs-lookup"><span data-stu-id="36e54-201">For **Activity Category**, select **Automatic App Password Rollover**.</span></span>      |
| <span data-ttu-id="36e54-202">Chyby zřizování účtů</span><span class="sxs-lookup"><span data-stu-id="36e54-202">Account provisioning errors</span></span>          | <span data-ttu-id="36e54-203">Pro **kategorie aktivity**, vyberte **zřizování účtu uživatele**.</span><span class="sxs-lookup"><span data-stu-id="36e54-203">For **Activity Category**, select **Account User Provisioning**.</span></span>        |
| <span data-ttu-id="36e54-204">Změny názvu skupiny Office 365</span><span class="sxs-lookup"><span data-stu-id="36e54-204">Office365 Group Name Changes</span></span>         | <span data-ttu-id="36e54-205">Pro **kategorie aktivity**, vyberte **Samoobslužná správa hesel**.</span><span class="sxs-lookup"><span data-stu-id="36e54-205">For **Activity Category**, select **Self-service Password Management**.</span></span> <span data-ttu-id="36e54-206">Pro **typ prostředku aktivity**, vyberte **skupiny**.</span><span class="sxs-lookup"><span data-stu-id="36e54-206">For **Activity Resource Type**, select **Group**.</span></span> <span data-ttu-id="36e54-207">Pro **zdroj aktivity**, vyberte **O365 skupiny**.</span><span class="sxs-lookup"><span data-stu-id="36e54-207">For **Activity Source**, select **O365 groups**.</span></span>|

<span data-ttu-id="36e54-208">Chcete-li zobrazit **využití aplikace** sestavy na **Azure Active Directory** okno v části **SPRAVOVAT**, vyberte **podnikové aplikace, které**a potom vyberte **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="36e54-208">To view the **Application Usage** report, on the **Azure Active Directory** blade, under **MANAGE**, select **Enterprise Applications**, and then select **Sign-ins**.</span></span>


<span data-ttu-id="36e54-209">![Sestava podnikové aplikace přihlášení](./media/active-directory-reporting-migration/199.png "podnikové aplikace přihlášení sestavy")</span><span class="sxs-lookup"><span data-stu-id="36e54-209">![Enterprise Applications Sign-Ins report](./media/active-directory-reporting-migration/199.png "Enterprise Applications Sign-Ins report")</span></span>

## <a name="next-steps"></a><span data-ttu-id="36e54-210">Další kroky</span><span class="sxs-lookup"><span data-stu-id="36e54-210">Next steps</span></span>

<span data-ttu-id="36e54-211">Přehled generování sestav najdete v tématu [Generování sestav v Azure Active Directory](active-directory-reporting-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="36e54-211">For an overview of reporting, see the [Azure Active Directory reporting](active-directory-reporting-azure-portal.md).</span></span>
