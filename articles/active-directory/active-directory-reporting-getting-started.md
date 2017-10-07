---
title: "Generování sestav Azure Active Directory: Začínáme | Dokumentace Microsoftu"
description: "Seznamy hello různých dostupných sestav generovaných v generování sestav Azure Active Directory"
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
ms.openlocfilehash: f47875708398391dd7f3efdc56a741fdba273b76
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-active-directory-reporting"></a><span data-ttu-id="de0c1-103">Začínáme s generováním sestav v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="de0c1-103">Getting started with Azure Active Directory Reporting</span></span>
## <a name="what-it-is"></a><span data-ttu-id="de0c1-104">Co to je</span><span class="sxs-lookup"><span data-stu-id="de0c1-104">What it is</span></span>
<span data-ttu-id="de0c1-105">Azure Active Directory (Azure AD) obsahuje různé sestavy zabezpečení, aktivit a auditu pro váš adresář.</span><span class="sxs-lookup"><span data-stu-id="de0c1-105">Azure Active Directory (Azure AD) includes security, activity, and audit reports for your directory.</span></span> <span data-ttu-id="de0c1-106">Tady je seznam hello sestavy zahrnuté:</span><span class="sxs-lookup"><span data-stu-id="de0c1-106">Here's a list of hello reports included:</span></span>

### <a name="security-reports"></a><span data-ttu-id="de0c1-107">Sestavy zabezpečení</span><span class="sxs-lookup"><span data-stu-id="de0c1-107">Security reports</span></span>
* <span data-ttu-id="de0c1-108">Přihlášení z neznámých zdrojů</span><span class="sxs-lookup"><span data-stu-id="de0c1-108">Sign-ins from unknown sources</span></span>
* <span data-ttu-id="de0c1-109">Přihlášení po několika neúspěších</span><span class="sxs-lookup"><span data-stu-id="de0c1-109">Sign-ins after multiple failures</span></span>
* <span data-ttu-id="de0c1-110">Přihlášení z více geografických poloh</span><span class="sxs-lookup"><span data-stu-id="de0c1-110">Sign-ins from multiple geographies</span></span>
* <span data-ttu-id="de0c1-111">Přihlášení z IP adres s podezřelou aktivitou</span><span class="sxs-lookup"><span data-stu-id="de0c1-111">Sign-ins from IP addresses with suspicious activity</span></span>
* <span data-ttu-id="de0c1-112">Nestandardní přihlašovací aktivita</span><span class="sxs-lookup"><span data-stu-id="de0c1-112">Irregular sign-in activity</span></span>
* <span data-ttu-id="de0c1-113">Přihlášení z možných nakažených zařízení</span><span class="sxs-lookup"><span data-stu-id="de0c1-113">Sign-ins from possibly infected devices</span></span>
* <span data-ttu-id="de0c1-114">Uživatelé s neobvyklou přihlašovací aktivitou</span><span class="sxs-lookup"><span data-stu-id="de0c1-114">Users with anomalous sign-in activity</span></span>

### <a name="activity-reports"></a><span data-ttu-id="de0c1-115">Sestavy aktivit</span><span class="sxs-lookup"><span data-stu-id="de0c1-115">Activity reports</span></span>
* <span data-ttu-id="de0c1-116">Využití aplikací: souhrn</span><span class="sxs-lookup"><span data-stu-id="de0c1-116">Application usage: summary</span></span>
* <span data-ttu-id="de0c1-117">Využití aplikací: podrobnosti</span><span class="sxs-lookup"><span data-stu-id="de0c1-117">Application usage: detailed</span></span>
* <span data-ttu-id="de0c1-118">Řídicí panel aplikací</span><span class="sxs-lookup"><span data-stu-id="de0c1-118">Application dashboard</span></span>
* <span data-ttu-id="de0c1-119">Chyby zřizování účtů</span><span class="sxs-lookup"><span data-stu-id="de0c1-119">Account provisioning errors</span></span>
* <span data-ttu-id="de0c1-120">Zařízení jednotlivých uživatelů</span><span class="sxs-lookup"><span data-stu-id="de0c1-120">Individual user devices</span></span>
* <span data-ttu-id="de0c1-121">Aktivity jednotlivých uživatelů</span><span class="sxs-lookup"><span data-stu-id="de0c1-121">Individual user Activity</span></span>
* <span data-ttu-id="de0c1-122">Sestava aktivit skupin</span><span class="sxs-lookup"><span data-stu-id="de0c1-122">Groups activity report</span></span>
* <span data-ttu-id="de0c1-123">Sestava aktivit registrace resetování hesla</span><span class="sxs-lookup"><span data-stu-id="de0c1-123">Password Reset Registration Activity Report</span></span>
* <span data-ttu-id="de0c1-124">Aktivity resetování hesla</span><span class="sxs-lookup"><span data-stu-id="de0c1-124">Password reset activity</span></span>

### <a name="audit-reports"></a><span data-ttu-id="de0c1-125">Sestavy auditu</span><span class="sxs-lookup"><span data-stu-id="de0c1-125">Audit reports</span></span>
* <span data-ttu-id="de0c1-126">Sestava auditu adresáře</span><span class="sxs-lookup"><span data-stu-id="de0c1-126">Directory audit report</span></span>

> [!TIP]
> <span data-ttu-id="de0c1-127">Další dokumentaci k sestavám Azure AD najdete v článku o [zobrazení sestav přístupu a využití](active-directory-view-access-usage-reports.md).</span><span class="sxs-lookup"><span data-stu-id="de0c1-127">For more documentation on Azure AD Reporting, check out [View your access and usage reports](active-directory-view-access-usage-reports.md).</span></span>
> 
> 

## <a name="how-it-works"></a><span data-ttu-id="de0c1-128">Jak to funguje</span><span class="sxs-lookup"><span data-stu-id="de0c1-128">How it works</span></span>
### <a name="reporting-pipeline"></a><span data-ttu-id="de0c1-129">Kanál generování sestav</span><span class="sxs-lookup"><span data-stu-id="de0c1-129">Reporting pipeline</span></span>
<span data-ttu-id="de0c1-130">Hello kanál generování sestav zahrnuje tři hlavní kroky.</span><span class="sxs-lookup"><span data-stu-id="de0c1-130">hello reporting pipeline consists of three main steps.</span></span> <span data-ttu-id="de0c1-131">Pokaždé, když se uživatel přihlásí nebo je provedeno ověření, se stane hello následující:</span><span class="sxs-lookup"><span data-stu-id="de0c1-131">Every time a user signs in, or an authentication is made, hello following happens:</span></span>

* <span data-ttu-id="de0c1-132">Nejprve hello uživatel ověřen (úspěšně nebo neúspěšně) a hello výsledek je uložen do databází služby Azure Active Directory hello.</span><span class="sxs-lookup"><span data-stu-id="de0c1-132">First, hello user is authenticated (successfully or unsuccessfully), and hello result is stored in hello Azure Active Directory service databases.</span></span>
* <span data-ttu-id="de0c1-133">V pravidelných intervalech jsou zpracovávána všechna poslední přihlášení.</span><span class="sxs-lookup"><span data-stu-id="de0c1-133">At regular intervals, all recent sign ins are processed.</span></span> <span data-ttu-id="de0c1-134">V tom okamžiku naše algoritmy zabezpečení a neobvyklých aktivit vyhledávají a zkouší identifikovat podezřelé aktivity ve všech posledních přihlášeních.</span><span class="sxs-lookup"><span data-stu-id="de0c1-134">At this point, our security and anomalous activity algorithms are searching all recent sign ins for suspicious activity.</span></span>
* <span data-ttu-id="de0c1-135">Po zpracování hello sestavy jsou zapsány, do mezipaměti a zpracuje v hello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="de0c1-135">After processing, hello reports are written, cached, and served in hello Azure classic portal.</span></span>

### <a name="report-generation-times"></a><span data-ttu-id="de0c1-136">Časy generování sestav</span><span class="sxs-lookup"><span data-stu-id="de0c1-136">Report generation times</span></span>
<span data-ttu-id="de0c1-137">Z důvodu velkého objemu toohello ověřování a přihlašování, že in zpracovává hello platformě Azure AD hello nejnovější zpracovaná přihlášení se v průměru hodinu stará.</span><span class="sxs-lookup"><span data-stu-id="de0c1-137">Due toohello large volume of authentications and sign ins processed by hello Azure AD platform, hello most recent sign-ins processed are, on average, one hour old.</span></span> <span data-ttu-id="de0c1-138">Ve výjimečných případech to může trvat až too8 hodin tooprocess hello poslední přihlášení.</span><span class="sxs-lookup"><span data-stu-id="de0c1-138">In rare cases, it may take up too8 hours tooprocess hello most recent sign-ins.</span></span>

<span data-ttu-id="de0c1-139">Hello nejnovější zpracované přihlášení najdete tak, že prověří textu hello nápovědy hello horní části každé sestavy.</span><span class="sxs-lookup"><span data-stu-id="de0c1-139">You can find hello most recent processed sign-in by examining hello help text at hello top of each report.</span></span>

![Pomocný text v hello horní části každé sestavy](./media/active-directory-reporting-getting-started/reportingWatermark.PNG)

> [!TIP]
> <span data-ttu-id="de0c1-141">Další dokumentaci k sestavám Azure AD najdete v článku o [zobrazení sestav přístupu a využití](active-directory-view-access-usage-reports.md).</span><span class="sxs-lookup"><span data-stu-id="de0c1-141">For more documentation on Azure AD Reporting, check out [View your access and usage reports](active-directory-view-access-usage-reports.md).</span></span>
> 
> 

## <a name="getting-started"></a><span data-ttu-id="de0c1-142">Začínáme</span><span class="sxs-lookup"><span data-stu-id="de0c1-142">Getting started</span></span>
### <a name="sign-into-hello-azure-classic-portal"></a><span data-ttu-id="de0c1-143">Přihlaste se k hello portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="de0c1-143">Sign into hello Azure classic portal</span></span>
<span data-ttu-id="de0c1-144">Nejprve budete potřebovat toosign do hello [portál Azure classic](https://manage.windowsazure.com) jako dodržování předpisů nebo globální správce.</span><span class="sxs-lookup"><span data-stu-id="de0c1-144">First, you'll need toosign into hello [Azure classic portal](https://manage.windowsazure.com)  as a global or compliance administrator.</span></span> <span data-ttu-id="de0c1-145">Musí být také předplatné služby správce nebo spolusprávce nebo používat hello "přístup tooAzure AD" předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="de0c1-145">You must also be an Azure subscription service administrator or co-administrator, or be using hello "Access tooAzure AD" Azure subscription.</span></span>

### <a name="navigate-tooreports"></a><span data-ttu-id="de0c1-146">Přejděte tooReports</span><span class="sxs-lookup"><span data-stu-id="de0c1-146">Navigate tooReports</span></span>
<span data-ttu-id="de0c1-147">Sestavy tooview přejděte kartě sestavy toohello hello horní části adresáře.</span><span class="sxs-lookup"><span data-stu-id="de0c1-147">tooview Reports, navigate toohello Reports tab at hello top of your directory.</span></span>

<span data-ttu-id="de0c1-148">Pokud je to poprvé prohlížení sestav hello, budete potřebovat dialogové okno tooa tooagree před zobrazením sestavy hello.</span><span class="sxs-lookup"><span data-stu-id="de0c1-148">If this is your first time viewing hello reports, you'll need tooagree tooa dialog box before you can view hello reports.</span></span> <span data-ttu-id="de0c1-149">Toto je tooensure, že je přijatelné pro správce ve vaší organizaci tooview tato data, která lze považovat za soukromé informace v některých zemích.</span><span class="sxs-lookup"><span data-stu-id="de0c1-149">This is tooensure that it's acceptable for admins in your organization tooview this data, which may be considered private information in some countries.</span></span>

![Dialogové okno](./media/active-directory-reporting-getting-started/dialogBox.png)

### <a name="explore-each-report"></a><span data-ttu-id="de0c1-151">Zkoumání jednotlivých sestav</span><span class="sxs-lookup"><span data-stu-id="de0c1-151">Explore each report</span></span>
<span data-ttu-id="de0c1-152">Přejděte na každý shromažďovaných dat toosee hello sestavy a zpracovat hello přihlášení.</span><span class="sxs-lookup"><span data-stu-id="de0c1-152">Navigate into each report toosee hello data being collected and hello sign-ins processed.</span></span> <span data-ttu-id="de0c1-153">Můžete najít [seznam všech sestav hello zde](active-directory-reporting-guide.md).</span><span class="sxs-lookup"><span data-stu-id="de0c1-153">You can find a [list of all hello reports here](active-directory-reporting-guide.md).</span></span>

![Všechny sestavy](./media/active-directory-reporting-getting-started/reportsMain.png)

### <a name="download-hello-reports-as-csv"></a><span data-ttu-id="de0c1-155">Stáhnout hello sestavy jako sdílený svazek clusteru</span><span class="sxs-lookup"><span data-stu-id="de0c1-155">Download hello reports as CSV</span></span>
<span data-ttu-id="de0c1-156">Každou sestavu lze stáhnout jako soubor CSV (hodnoty oddělené čárkami).</span><span class="sxs-lookup"><span data-stu-id="de0c1-156">Each report can be downloaded as a CSV (comma-separated value) file.</span></span> <span data-ttu-id="de0c1-157">Můžete použít tyto soubory v aplikaci Excel, PowerBI nebo programy toofurther analyzovat data analysis třetích stran.</span><span class="sxs-lookup"><span data-stu-id="de0c1-157">You can use these files in Excel, PowerBI or third-party analysis programs toofurther analyze your data.</span></span>

<span data-ttu-id="de0c1-158">toodownload všechny sestavy jako sdílený svazek clusteru, přejděte toohello sestavy a klikněte na tlačítko "Stáhnout" v dolní části hello.</span><span class="sxs-lookup"><span data-stu-id="de0c1-158">toodownload any report as a CSV, navigate toohello report and click "Download" at hello bottom.</span></span>

![Tlačítko Stáhnout](./media/active-directory-reporting-getting-started/downloadButton.png)

> [!TIP]
> <span data-ttu-id="de0c1-160">Další dokumentaci k sestavám Azure AD najdete v článku o [zobrazení sestav přístupu a využití](active-directory-view-access-usage-reports.md).</span><span class="sxs-lookup"><span data-stu-id="de0c1-160">For more documentation on Azure AD Reporting, check out [View your access and usage reports](active-directory-view-access-usage-reports.md).</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="de0c1-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="de0c1-161">Next steps</span></span>
### <a name="customize-alerts-for-anomalous-sign-in-activity"></a><span data-ttu-id="de0c1-162">Přizpůsobení upozornění na neobvyklé přihlašovací aktivity</span><span class="sxs-lookup"><span data-stu-id="de0c1-162">Customize alerts for anomalous sign in activity</span></span>
<span data-ttu-id="de0c1-163">Přejděte toohello "Konfigurace" kartě adresáře.</span><span class="sxs-lookup"><span data-stu-id="de0c1-163">Navigate toohello "Configure" tab of your directory.</span></span>

<span data-ttu-id="de0c1-164">Posuňte se toohello části "Oznámení".</span><span class="sxs-lookup"><span data-stu-id="de0c1-164">Scroll toohello "Notifications" section.</span></span>

<span data-ttu-id="de0c1-165">Povolit nebo zakázat část "E-mailová oznámení neobvyklých přihlášení" hello.</span><span class="sxs-lookup"><span data-stu-id="de0c1-165">Enable or disable hello "Email Notifications of Anomalous sign-ins" section.</span></span>

![Hello sekce oznámení](./media/active-directory-reporting-getting-started/notificationsSection.png)

### <a name="integrate-with-hello-azure-ad-reporting-api"></a><span data-ttu-id="de0c1-167">Integrovat hello rozhraní API pro vytváření sestav Azure AD</span><span class="sxs-lookup"><span data-stu-id="de0c1-167">Integrate with hello Azure AD Reporting API</span></span>
<span data-ttu-id="de0c1-168">V tématu [Začínáme s hello Reporting rozhraní API](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="de0c1-168">See [Getting started with hello Reporting API](active-directory-reporting-api-getting-started.md).</span></span>

### <a name="engage-multi-factor-authentication-on-users"></a><span data-ttu-id="de0c1-169">Povolení služby Multi-Factor Authentication pro uživatele</span><span class="sxs-lookup"><span data-stu-id="de0c1-169">Engage Multi-Factor Authentication on users</span></span>
<span data-ttu-id="de0c1-170">Vyberte uživatele v sestavě.</span><span class="sxs-lookup"><span data-stu-id="de0c1-170">Select a user in a report.</span></span>

<span data-ttu-id="de0c1-171">Klikněte na tlačítko "Povolit MFA" hello v hello dolní části obrazovky hello.</span><span class="sxs-lookup"><span data-stu-id="de0c1-171">Click hello "Enable MFA" button at hello bottom of hello screen.</span></span>

![tlačítko služby Multi-Factor Authentication Hello v hello dolní části obrazovky hello](./media/active-directory-reporting-getting-started/mfaButton.png)

> [!TIP]
> <span data-ttu-id="de0c1-173">Další dokumentaci k sestavám Azure AD najdete v článku o [zobrazení sestav přístupu a využití](active-directory-view-access-usage-reports.md).</span><span class="sxs-lookup"><span data-stu-id="de0c1-173">For more documentation on Azure AD Reporting, check out [View your access and usage reports](active-directory-view-access-usage-reports.md).</span></span>
> 
> 

## <a name="learn-more"></a><span data-ttu-id="de0c1-174">Další informace</span><span class="sxs-lookup"><span data-stu-id="de0c1-174">Learn more</span></span>
### <a name="audit-events"></a><span data-ttu-id="de0c1-175">Auditování událostí</span><span class="sxs-lookup"><span data-stu-id="de0c1-175">Audit events</span></span>
<span data-ttu-id="de0c1-176">Další informace o událostech, které jsou v adresáři hello v auditovat [Azure Active Directory Reporting událostí auditu](active-directory-reporting-audit-events.md).</span><span class="sxs-lookup"><span data-stu-id="de0c1-176">Learn about what events are audited in hello directory in [Azure Active Directory Reporting Audit Events](active-directory-reporting-audit-events.md).</span></span>

### <a name="api-integration"></a><span data-ttu-id="de0c1-177">Integrace rozhraní API</span><span class="sxs-lookup"><span data-stu-id="de0c1-177">API Integration</span></span>
<span data-ttu-id="de0c1-178">V tématu [Začínáme s hello Reporting rozhraní API](active-directory-reporting-api-getting-started.md) a hello [referenční dokumentace rozhraní API](https://msdn.microsoft.com/library/azure/mt126081.aspx).</span><span class="sxs-lookup"><span data-stu-id="de0c1-178">See [Getting started with hello Reporting API](active-directory-reporting-api-getting-started.md) and hello [API reference documentation](https://msdn.microsoft.com/library/azure/mt126081.aspx).</span></span>

### <a name="get-in-touch"></a><span data-ttu-id="de0c1-179">Kontakt</span><span class="sxs-lookup"><span data-stu-id="de0c1-179">Get in touch</span></span>
<span data-ttu-id="de0c1-180">Pro zpětnou vazbu, pomoc a případné dotazy slouží e-mailová adresa [aadreportinghelp@microsoft.com](mailto:aadreportinghelp@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="de0c1-180">Email [aadreportinghelp@microsoft.com](mailto:aadreportinghelp@microsoft.com) for feedback, help, or any questions you might have.</span></span>

> [!TIP]
> <span data-ttu-id="de0c1-181">Další dokumentaci k sestavám Azure AD najdete v článku o [zobrazení sestav přístupu a využití](active-directory-view-access-usage-reports.md).</span><span class="sxs-lookup"><span data-stu-id="de0c1-181">For more documentation on Azure AD Reporting, check out [View your access and usage reports](active-directory-view-access-usage-reports.md).</span></span>
> 
> 

