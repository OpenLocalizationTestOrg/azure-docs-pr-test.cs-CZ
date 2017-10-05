---
title: "Generování sestav Azure Active Directory: Začínáme | Dokumentace Microsoftu"
description: "Obsahuje seznam různých dostupných sestav generovaných v Azure Active Directory"
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: 7ac99919-8df5-4424-9298-fc7c025ba949
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/16/2017
ms.author: dhanyahk;markvi
ms.openlocfilehash: 5cd1ae6196d9cd63f97dc9d302442280ece23e40
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="getting-started-with-azure-active-directory-reporting"></a><span data-ttu-id="23bdb-103">Začínáme s generováním sestav v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="23bdb-103">Getting started with Azure Active Directory Reporting</span></span>
## <a name="what-it-is"></a><span data-ttu-id="23bdb-104">Co to je</span><span class="sxs-lookup"><span data-stu-id="23bdb-104">What it is</span></span>
<span data-ttu-id="23bdb-105">Azure Active Directory (Azure AD) obsahuje různé sestavy zabezpečení, aktivit a auditu pro váš adresář.</span><span class="sxs-lookup"><span data-stu-id="23bdb-105">Azure Active Directory (Azure AD) includes security, activity, and audit reports for your directory.</span></span> <span data-ttu-id="23bdb-106">Zde je seznam zahrnutých sestav:</span><span class="sxs-lookup"><span data-stu-id="23bdb-106">Here's a list of the reports included:</span></span>

### <a name="security-reports"></a><span data-ttu-id="23bdb-107">Sestavy zabezpečení</span><span class="sxs-lookup"><span data-stu-id="23bdb-107">Security reports</span></span>
* <span data-ttu-id="23bdb-108">Přihlášení z neznámých zdrojů</span><span class="sxs-lookup"><span data-stu-id="23bdb-108">Sign-ins from unknown sources</span></span>
* <span data-ttu-id="23bdb-109">Přihlášení po několika neúspěších</span><span class="sxs-lookup"><span data-stu-id="23bdb-109">Sign-ins after multiple failures</span></span>
* <span data-ttu-id="23bdb-110">Přihlášení z více geografických poloh</span><span class="sxs-lookup"><span data-stu-id="23bdb-110">Sign-ins from multiple geographies</span></span>
* <span data-ttu-id="23bdb-111">Přihlášení z IP adres s podezřelou aktivitou</span><span class="sxs-lookup"><span data-stu-id="23bdb-111">Sign-ins from IP addresses with suspicious activity</span></span>
* <span data-ttu-id="23bdb-112">Nestandardní přihlašovací aktivita</span><span class="sxs-lookup"><span data-stu-id="23bdb-112">Irregular sign-in activity</span></span>
* <span data-ttu-id="23bdb-113">Přihlášení z možných nakažených zařízení</span><span class="sxs-lookup"><span data-stu-id="23bdb-113">Sign-ins from possibly infected devices</span></span>
* <span data-ttu-id="23bdb-114">Uživatelé s neobvyklou přihlašovací aktivitou</span><span class="sxs-lookup"><span data-stu-id="23bdb-114">Users with anomalous sign-in activity</span></span>

### <a name="activity-reports"></a><span data-ttu-id="23bdb-115">Sestavy aktivit</span><span class="sxs-lookup"><span data-stu-id="23bdb-115">Activity reports</span></span>
* <span data-ttu-id="23bdb-116">Využití aplikací: souhrn</span><span class="sxs-lookup"><span data-stu-id="23bdb-116">Application usage: summary</span></span>
* <span data-ttu-id="23bdb-117">Využití aplikací: podrobnosti</span><span class="sxs-lookup"><span data-stu-id="23bdb-117">Application usage: detailed</span></span>
* <span data-ttu-id="23bdb-118">Řídicí panel aplikací</span><span class="sxs-lookup"><span data-stu-id="23bdb-118">Application dashboard</span></span>
* <span data-ttu-id="23bdb-119">Chyby zřizování účtů</span><span class="sxs-lookup"><span data-stu-id="23bdb-119">Account provisioning errors</span></span>
* <span data-ttu-id="23bdb-120">Zařízení jednotlivých uživatelů</span><span class="sxs-lookup"><span data-stu-id="23bdb-120">Individual user devices</span></span>
* <span data-ttu-id="23bdb-121">Aktivity jednotlivých uživatelů</span><span class="sxs-lookup"><span data-stu-id="23bdb-121">Individual user Activity</span></span>
* <span data-ttu-id="23bdb-122">Sestava aktivit skupin</span><span class="sxs-lookup"><span data-stu-id="23bdb-122">Groups activity report</span></span>
* <span data-ttu-id="23bdb-123">Sestava aktivit registrace resetování hesla</span><span class="sxs-lookup"><span data-stu-id="23bdb-123">Password Reset Registration Activity Report</span></span>
* <span data-ttu-id="23bdb-124">Aktivity resetování hesla</span><span class="sxs-lookup"><span data-stu-id="23bdb-124">Password reset activity</span></span>

### <a name="audit-reports"></a><span data-ttu-id="23bdb-125">Sestavy auditu</span><span class="sxs-lookup"><span data-stu-id="23bdb-125">Audit reports</span></span>
* <span data-ttu-id="23bdb-126">Sestava auditu adresáře</span><span class="sxs-lookup"><span data-stu-id="23bdb-126">Directory audit report</span></span>

> [!TIP]
> <span data-ttu-id="23bdb-127">Další dokumentaci k sestavám Azure AD najdete v článku o [zobrazení sestav přístupu a využití](active-directory-view-access-usage-reports.md).</span><span class="sxs-lookup"><span data-stu-id="23bdb-127">For more documentation on Azure AD Reporting, check out [View your access and usage reports](active-directory-view-access-usage-reports.md).</span></span>
> 
> 

## <a name="how-it-works"></a><span data-ttu-id="23bdb-128">Jak to funguje</span><span class="sxs-lookup"><span data-stu-id="23bdb-128">How it works</span></span>
### <a name="reporting-pipeline"></a><span data-ttu-id="23bdb-129">Kanál generování sestav</span><span class="sxs-lookup"><span data-stu-id="23bdb-129">Reporting pipeline</span></span>
<span data-ttu-id="23bdb-130">Kanál generování sestav zahrnuje tři hlavní kroky.</span><span class="sxs-lookup"><span data-stu-id="23bdb-130">The reporting pipeline consists of three main steps.</span></span> <span data-ttu-id="23bdb-131">Pokaždé, když se uživatel přihlásí nebo je provedeno ověření, stane se toto:</span><span class="sxs-lookup"><span data-stu-id="23bdb-131">Every time a user signs in, or an authentication is made, the following happens:</span></span>

* <span data-ttu-id="23bdb-132">Nejprve je uživatel ověřen (úspěšně nebo neúspěšně) a výsledek je uložen do databází služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="23bdb-132">First, the user is authenticated (successfully or unsuccessfully), and the result is stored in the Azure Active Directory service databases.</span></span>
* <span data-ttu-id="23bdb-133">V pravidelných intervalech jsou zpracovávána všechna poslední přihlášení.</span><span class="sxs-lookup"><span data-stu-id="23bdb-133">At regular intervals, all recent sign ins are processed.</span></span> <span data-ttu-id="23bdb-134">V tom okamžiku naše algoritmy zabezpečení a neobvyklých aktivit vyhledávají a zkouší identifikovat podezřelé aktivity ve všech posledních přihlášeních.</span><span class="sxs-lookup"><span data-stu-id="23bdb-134">At this point, our security and anomalous activity algorithms are searching all recent sign ins for suspicious activity.</span></span>
* <span data-ttu-id="23bdb-135">Po zpracování jsou vytvořeny sestavy, které jsou uloženy a poskytovány na portálu Azure Classic.</span><span class="sxs-lookup"><span data-stu-id="23bdb-135">After processing, the reports are written, cached, and served in the Azure classic portal.</span></span>

### <a name="report-generation-times"></a><span data-ttu-id="23bdb-136">Časy generování sestav</span><span class="sxs-lookup"><span data-stu-id="23bdb-136">Report generation times</span></span>
<span data-ttu-id="23bdb-137">Vzhledem k velkému objemu ověření a přihlášení zpracovávaných platformou Azure AD jsou nejnověji zpracovaná přihlášení v průměru hodinu stará.</span><span class="sxs-lookup"><span data-stu-id="23bdb-137">Due to the large volume of authentications and sign ins processed by the Azure AD platform, the most recent sign-ins processed are, on average, one hour old.</span></span> <span data-ttu-id="23bdb-138">Ve výjimečných případech může zpracování nejnovějších přihlášení trvat až 8 hodin.</span><span class="sxs-lookup"><span data-stu-id="23bdb-138">In rare cases, it may take up to 8 hours to process the most recent sign-ins.</span></span>

<span data-ttu-id="23bdb-139">Nejnovější zpracované přihlášení zjistíte v pomocném textu v horní části každé sestavy.</span><span class="sxs-lookup"><span data-stu-id="23bdb-139">You can find the most recent processed sign-in by examining the help text at the top of each report.</span></span>

![Pomocný text v horní části každé sestavy](./media/active-directory-reporting-getting-started/reportingWatermark.PNG)

> [!TIP]
> <span data-ttu-id="23bdb-141">Další dokumentaci k sestavám Azure AD najdete v článku o [zobrazení sestav přístupu a využití](active-directory-view-access-usage-reports.md).</span><span class="sxs-lookup"><span data-stu-id="23bdb-141">For more documentation on Azure AD Reporting, check out [View your access and usage reports](active-directory-view-access-usage-reports.md).</span></span>
> 
> 

## <a name="getting-started"></a><span data-ttu-id="23bdb-142">Začínáme</span><span class="sxs-lookup"><span data-stu-id="23bdb-142">Getting started</span></span>
### <a name="sign-into-the-azure-classic-portal"></a><span data-ttu-id="23bdb-143">Přihlášení k portálu Azure Classic</span><span class="sxs-lookup"><span data-stu-id="23bdb-143">Sign into the Azure classic portal</span></span>
<span data-ttu-id="23bdb-144">Nejprve se budete muset přihlásit k [portálu Azure Classic](https://manage.windowsazure.com) jako globální správce nebo správce dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="23bdb-144">First, you'll need to sign into the [Azure classic portal](https://manage.windowsazure.com)  as a global or compliance administrator.</span></span> <span data-ttu-id="23bdb-145">Musíte také být správce či spolusprávce služby předplatného Azure nebo používat předplatné Azure „Přístup k Azure AD“ .</span><span class="sxs-lookup"><span data-stu-id="23bdb-145">You must also be an Azure subscription service administrator or co-administrator, or be using the "Access to Azure AD" Azure subscription.</span></span>

### <a name="navigate-to-reports"></a><span data-ttu-id="23bdb-146">Přechod na Sestavy</span><span class="sxs-lookup"><span data-stu-id="23bdb-146">Navigate to Reports</span></span>
<span data-ttu-id="23bdb-147">Chcete-li zobrazit sestavy, přejděte na kartu Sestavy v horní části adresáře.</span><span class="sxs-lookup"><span data-stu-id="23bdb-147">To view Reports, navigate to the Reports tab at the top of your directory.</span></span>

<span data-ttu-id="23bdb-148">Pokud si sestavy zobrazujete poprvé, musíte před jejich zobrazením potvrdit souhlas v dialogovém okně.</span><span class="sxs-lookup"><span data-stu-id="23bdb-148">If this is your first time viewing the reports, you'll need to agree to a dialog box before you can view the reports.</span></span> <span data-ttu-id="23bdb-149">Je to z toho důvodu, abyste potvrdili, že je ve vaší organizaci přijatelné, aby správci zobrazovali tato data. Ta mohou být v některých zemích považována za osobní údaje.</span><span class="sxs-lookup"><span data-stu-id="23bdb-149">This is to ensure that it's acceptable for admins in your organization to view this data, which may be considered private information in some countries.</span></span>

![Dialogové okno](./media/active-directory-reporting-getting-started/dialogBox.png)

### <a name="explore-each-report"></a><span data-ttu-id="23bdb-151">Zkoumání jednotlivých sestav</span><span class="sxs-lookup"><span data-stu-id="23bdb-151">Explore each report</span></span>
<span data-ttu-id="23bdb-152">Přejděte na sestavu, která vás zajímá, a uvidíte shromážděná data a zpracovaná přihlášení.</span><span class="sxs-lookup"><span data-stu-id="23bdb-152">Navigate into each report to see the data being collected and the sign-ins processed.</span></span> <span data-ttu-id="23bdb-153">Zde můžete najít [seznam všech sestav](active-directory-reporting-guide.md).</span><span class="sxs-lookup"><span data-stu-id="23bdb-153">You can find a [list of all the reports here](active-directory-reporting-guide.md).</span></span>

![Všechny sestavy](./media/active-directory-reporting-getting-started/reportsMain.png)

### <a name="download-the-reports-as-csv"></a><span data-ttu-id="23bdb-155">Stažení sestavy ve formátu souboru CSV</span><span class="sxs-lookup"><span data-stu-id="23bdb-155">Download the reports as CSV</span></span>
<span data-ttu-id="23bdb-156">Každou sestavu lze stáhnout jako soubor CSV (hodnoty oddělené čárkami).</span><span class="sxs-lookup"><span data-stu-id="23bdb-156">Each report can be downloaded as a CSV (comma-separated value) file.</span></span> <span data-ttu-id="23bdb-157">Tyto soubory můžete použít v aplikacích Excel, PowerBI nebo analytických programech třetích stran k další analýze dat.</span><span class="sxs-lookup"><span data-stu-id="23bdb-157">You can use these files in Excel, PowerBI or third-party analysis programs to further analyze your data.</span></span>

<span data-ttu-id="23bdb-158">Pokud chcete některou sestavu stáhnout jako soubor CSV, přejděte na sestavu a klikněte na tlačítko „Stáhnout“ v dolní části.</span><span class="sxs-lookup"><span data-stu-id="23bdb-158">To download any report as a CSV, navigate to the report and click "Download" at the bottom.</span></span>

![Tlačítko Stáhnout](./media/active-directory-reporting-getting-started/downloadButton.png)

> [!TIP]
> <span data-ttu-id="23bdb-160">Další dokumentaci k sestavám Azure AD najdete v článku o [zobrazení sestav přístupu a využití](active-directory-view-access-usage-reports.md).</span><span class="sxs-lookup"><span data-stu-id="23bdb-160">For more documentation on Azure AD Reporting, check out [View your access and usage reports](active-directory-view-access-usage-reports.md).</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="23bdb-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="23bdb-161">Next steps</span></span>
### <a name="customize-alerts-for-anomalous-sign-in-activity"></a><span data-ttu-id="23bdb-162">Přizpůsobení upozornění na neobvyklé přihlašovací aktivity</span><span class="sxs-lookup"><span data-stu-id="23bdb-162">Customize alerts for anomalous sign in activity</span></span>
<span data-ttu-id="23bdb-163">Přejděte na kartu „Konfigurace“ adresáře.</span><span class="sxs-lookup"><span data-stu-id="23bdb-163">Navigate to the "Configure" tab of your directory.</span></span>

<span data-ttu-id="23bdb-164">Přejděte do části „Oznámení“.</span><span class="sxs-lookup"><span data-stu-id="23bdb-164">Scroll to the "Notifications" section.</span></span>

<span data-ttu-id="23bdb-165">Povolte nebo zakažte sekci „E-mailová oznámení neobvyklých přihlášení“.</span><span class="sxs-lookup"><span data-stu-id="23bdb-165">Enable or disable the "Email Notifications of Anomalous sign-ins" section.</span></span>

![Sekce Oznámení](./media/active-directory-reporting-getting-started/notificationsSection.png)

### <a name="integrate-with-the-azure-ad-reporting-api"></a><span data-ttu-id="23bdb-167">Integrace s rozhraním API pro generování sestav Azure AD</span><span class="sxs-lookup"><span data-stu-id="23bdb-167">Integrate with the Azure AD Reporting API</span></span>
<span data-ttu-id="23bdb-168">Viz [Začínáme s rozhraním API pro generování sestav](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="23bdb-168">See [Getting started with the Reporting API](active-directory-reporting-api-getting-started.md).</span></span>

### <a name="engage-multi-factor-authentication-on-users"></a><span data-ttu-id="23bdb-169">Povolení služby Multi-Factor Authentication pro uživatele</span><span class="sxs-lookup"><span data-stu-id="23bdb-169">Engage Multi-Factor Authentication on users</span></span>
<span data-ttu-id="23bdb-170">Vyberte uživatele v sestavě.</span><span class="sxs-lookup"><span data-stu-id="23bdb-170">Select a user in a report.</span></span>

<span data-ttu-id="23bdb-171">Klikněte na tlačítko „Povolit MFA“ v dolní části obrazovky.</span><span class="sxs-lookup"><span data-stu-id="23bdb-171">Click the "Enable MFA" button at the bottom of the screen.</span></span>

![Tlačítko služby Multi-Factor Authentication v dolní části obrazovky](./media/active-directory-reporting-getting-started/mfaButton.png)

> [!TIP]
> <span data-ttu-id="23bdb-173">Další dokumentaci k sestavám Azure AD najdete v článku o [zobrazení sestav přístupu a využití](active-directory-view-access-usage-reports.md).</span><span class="sxs-lookup"><span data-stu-id="23bdb-173">For more documentation on Azure AD Reporting, check out [View your access and usage reports](active-directory-view-access-usage-reports.md).</span></span>
> 
> 

## <a name="learn-more"></a><span data-ttu-id="23bdb-174">Další informace</span><span class="sxs-lookup"><span data-stu-id="23bdb-174">Learn more</span></span>
### <a name="audit-events"></a><span data-ttu-id="23bdb-175">Auditování událostí</span><span class="sxs-lookup"><span data-stu-id="23bdb-175">Audit events</span></span>
<span data-ttu-id="23bdb-176">Informace o událostech, které jsou v adresáři auditovány, získáte v článku o [auditovaných událostech pro generování sestav Azure Active Directory](active-directory-reporting-audit-events.md).</span><span class="sxs-lookup"><span data-stu-id="23bdb-176">Learn about what events are audited in the directory in [Azure Active Directory Reporting Audit Events](active-directory-reporting-audit-events.md).</span></span>

### <a name="api-integration"></a><span data-ttu-id="23bdb-177">Integrace rozhraní API</span><span class="sxs-lookup"><span data-stu-id="23bdb-177">API Integration</span></span>
<span data-ttu-id="23bdb-178">Viz [Začínáme s rozhraním API pro generování sestav](active-directory-reporting-api-getting-started.md) a [referenční dokumentace rozhraní API](https://msdn.microsoft.com/library/azure/mt126081.aspx).</span><span class="sxs-lookup"><span data-stu-id="23bdb-178">See [Getting started with the Reporting API](active-directory-reporting-api-getting-started.md) and the [API reference documentation](https://msdn.microsoft.com/library/azure/mt126081.aspx).</span></span>

### <a name="get-in-touch"></a><span data-ttu-id="23bdb-179">Kontakt</span><span class="sxs-lookup"><span data-stu-id="23bdb-179">Get in touch</span></span>
<span data-ttu-id="23bdb-180">Pro zpětnou vazbu, pomoc a případné dotazy slouží e-mailová adresa [aadreportinghelp@microsoft.com](mailto:aadreportinghelp@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="23bdb-180">Email [aadreportinghelp@microsoft.com](mailto:aadreportinghelp@microsoft.com) for feedback, help, or any questions you might have.</span></span>

> [!TIP]
> <span data-ttu-id="23bdb-181">Další dokumentaci k sestavám Azure AD najdete v článku o [zobrazení sestav přístupu a využití](active-directory-view-access-usage-reports.md).</span><span class="sxs-lookup"><span data-stu-id="23bdb-181">For more documentation on Azure AD Reporting, check out [View your access and usage reports](active-directory-view-access-usage-reports.md).</span></span>
> 
> 

