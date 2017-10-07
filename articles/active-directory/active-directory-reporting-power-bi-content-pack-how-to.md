---
title: "aaaHow toouse hello Azure Active Directory Power BI balíček obsahu | Microsoft Docs"
description: "Zjistěte, jak toouse hello Azure Active Directory Power BI balíček obsahu"
services: active-directory
author: MarkusVi
manager: femila
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: d07d678aedbe3089c4ea5f981f72311bdb389a17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-azure-active-directory-power-bi-content-pack"></a><span data-ttu-id="72922-103">Jak toouse hello Azure Active Directory Power BI balíček obsahu</span><span class="sxs-lookup"><span data-stu-id="72922-103">How toouse hello Azure Active Directory Power BI Content Pack</span></span>

<span data-ttu-id="72922-104">Pro vás jako správce IT je naprosto nezbytné pochopit, jakým způsobem uživatelé přijímají a používají funkce služby Azure Active Directory. Umožňuje vám tooplan vaší IT infrastruktury a komunikace tooincrease využití a tooget hello nejvíce mimo funkce AAD.</span><span class="sxs-lookup"><span data-stu-id="72922-104">Understanding how your users adopt and use Azure Active Directory features is critical for you as an IT admin. It allows you tooplan your IT infrastructure and communication tooincrease usage and tooget hello most out of AAD features.</span></span> <span data-ttu-id="72922-105">Pack obsah Power BI pro Azure Active Directory poskytuje hello toofurther možnost analyzovat vaše data toounderstand použití tato data toogather bohatší přehledy co se děje s Azure Active Directory pro hello různé možnosti můžete výraznou spoléhají na.</span><span class="sxs-lookup"><span data-stu-id="72922-105">Power BI Content Pack for Azure Active Directory gives you hello ability toofurther analyze your data toounderstand how you can use this data toogather richer insights into what’s going on with their Azure Active Directory for hello various capabilities you heavily rely on.</span></span>  <span data-ttu-id="72922-106">S hello integrace Azure Active Directory, rozhraní API do Power BI můžete snadno stáhnout hello balíčky předem připraveného obsahu a získáte přehled o tooall hello aktivity v rámci služby Azure Active Directory pomocí Power BI nabízí bohaté vizualizace prostředí.</span><span class="sxs-lookup"><span data-stu-id="72922-106">With hello integration of Azure Active Directory APIs into Power BI, you can easily download hello pre-built content packs and gain insights tooall hello activities within your Azure Active Directory using rich visualization experience Power BI offers.</span></span> <span data-ttu-id="72922-107">Můžete si vytvořit vlastní řídicí panel a snadno ho sdílet se všemi uživateli ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="72922-107">You can create your own dashboard and share it easily with anyone in your organization.</span></span> 

<span data-ttu-id="72922-108">Toto téma poskytuje vám podrobný návod, jak tooinstall a používání hello obsah pack ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="72922-108">This topic provides you with step-by-step instructions on how tooinstall and use hello content pack in your environment.</span></span>

## <a name="installation"></a><span data-ttu-id="72922-109">Instalace</span><span class="sxs-lookup"><span data-stu-id="72922-109">Installation</span></span>  

<span data-ttu-id="72922-110">**tooinstall hello balíček obsahu Power BI:**</span><span class="sxs-lookup"><span data-stu-id="72922-110">**tooinstall hello Power BI Content Pack:**</span></span>

1. <span data-ttu-id="72922-111">Přihlaste se k [Power BI](https://app.powerbi.com/groups/me/getdata/services) pomocí účtu Power BI (Toto je hello stejný účet jako O365 nebo účtu služby Azure AD).</span><span class="sxs-lookup"><span data-stu-id="72922-111">Log into [Power BI](https://app.powerbi.com/groups/me/getdata/services) with your Power BI Account (this is hello same account as your O365 or Azure AD Account).</span></span>

2. <span data-ttu-id="72922-112">V dolní části hello hello levém navigačním podokně, vyberte **načíst Data**.</span><span class="sxs-lookup"><span data-stu-id="72922-112">At hello bottom of hello left navigation pane, select **Get Data**.</span></span>

    ![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/01.png)
 
3. <span data-ttu-id="72922-114">V hello **služby** pole, klikněte na tlačítko **získat**.</span><span class="sxs-lookup"><span data-stu-id="72922-114">In hello **Services** box, click **Get**.</span></span>
   
    ![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/02.png)

4.  <span data-ttu-id="72922-116">Vyhledejte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="72922-116">Search for **Azure Active Directory**.</span></span>

    ![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/03.png)
 
5.  <span data-ttu-id="72922-118">Po zobrazení výzvy zadejte ID klienta služby Azure AD a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="72922-118">When prompted, type your Azure AD Tenant ID, and then click **Next**.</span></span>

    > [!TIP] 
    > <span data-ttu-id="72922-119">Hello tooget rychlý způsob Id klienta pro vašeho tenanta Office 365 / Azure AD je toologin toohello portálu Azure AD, Procházet adresář toohello a zkopírujte hello ID z hello následující adresu URL: https://manage.windowsazure.com/woodgroveonline.com#Workspaces/ ActiveDirectoryExtension/Directory/<tenantid>/directoryQuickStart</span><span class="sxs-lookup"><span data-stu-id="72922-119">A quick way tooget hello Tenant Id for your Office 365 / Azure AD tenant is toologin toohello Azure AD Portal, drill down toohello directory and copy hello ID from hello following URL: https://manage.windowsazure.com/woodgroveonline.com#Workspaces/ActiveDirectoryExtension/Directory/<tenantid>/directoryQuickStart</span></span>

    ![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/04.png) 

6.  <span data-ttu-id="72922-121">Klikněte na **Přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="72922-121">Click **Sign-in**.</span></span> 
 
    ![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/05.png) 



7.  <span data-ttu-id="72922-123">Zadejte uživatelské jméno a heslo a pak klikněte na **Přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="72922-123">Enter your username and password, and then click **Sign in**.</span></span>
 
    ![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/06.png) 

8.  <span data-ttu-id="72922-125">V dialogovém okně souhlasu hello aplikace, klikněte na tlačítko **přijmout**.</span><span class="sxs-lookup"><span data-stu-id="72922-125">On hello app consent dialog, click **Accept**.</span></span>
 
9.  <span data-ttu-id="72922-126">Jakmile řídicí panel protokolů aktivity služby Azure Active Directory vytvoříte, klikněte na něj.</span><span class="sxs-lookup"><span data-stu-id="72922-126">When your Azure Active Directory Activity logs dashboard has been created, click it.</span></span>
 
    ![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/08.png) 

## <a name="what-can-i-do-with-this-content-pack"></a><span data-ttu-id="72922-128">Co můžu s tímto balíčkem obsahu dělat?</span><span class="sxs-lookup"><span data-stu-id="72922-128">What can I do with this content pack?</span></span>

<span data-ttu-id="72922-129">Před jsme přejít do co můžete dělat s Tento balíček obsahu, zde uvádíme rychlý náhled hello různé sestavy v obsahu hello aktualizací Service pack.</span><span class="sxs-lookup"><span data-stu-id="72922-129">Before we jump into what you can do with this content pack, here’s quick preview of hello various reports in hello content pack.</span></span> <span data-ttu-id="72922-130">Sestava dat přejde zpět toohello **posledních 30 dní**.</span><span class="sxs-lookup"><span data-stu-id="72922-130">Report data goes back toohello **last 30 days**.</span></span>

### <a name="reports-included-in-this-version-of-azure-active-directory-logs-content-pack"></a><span data-ttu-id="72922-131">Sestavy zahrnuté v této verzi balíčku obsahu protokolů služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="72922-131">Reports included in this version of Azure Active Directory logs Content Pack</span></span>

<span data-ttu-id="72922-132">**Sestavy využití aplikací a Trend**: získat přehled o hello aplikace použitý v organizaci a ty, které jsou používány hello nejvíce a kdy.</span><span class="sxs-lookup"><span data-stu-id="72922-132">**App Usage and Trend report**:  Get insights into hello apps used in your organization and which ones are being used hello most and when.</span></span> <span data-ttu-id="72922-133">Můžete použít tuto sestavu toogather přehledy využití aplikace, které se nedávno nasazen ve vaší organizaci nebo zjistit, které aplikace jsou oblíbených.</span><span class="sxs-lookup"><span data-stu-id="72922-133">You can use this report toogather insights into how an app you recently rolled out in your organization is being used or find out which apps are popular.</span></span> <span data-ttu-id="72922-134">Díky tomu může zvýšit využití, pokud se zobrazí, pokud aplikace hello nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="72922-134">By doing this, you can improve usage if you see if hello app is not being used.</span></span>

<span data-ttu-id="72922-135">**Přihlášení podle umístění a uživatelé**: získat přehled o všech přihlášení hello provádí pomocí Azure Identity a poskytuje přehled o hello identity uživatelů hello.</span><span class="sxs-lookup"><span data-stu-id="72922-135">**Sign-ins by location and users**: Get insights into all hello sign-ins performed using Azure Identity and gives insights into hello identity of hello users.</span></span> <span data-ttu-id="72922-136">Můžete podrobněji analyzovat individuální přihlášení a odpovědět si na otázky jako:</span><span class="sxs-lookup"><span data-stu-id="72922-136">With this, you can dig deeper into individual sign-ins and answer questions like:</span></span>

- <span data-ttu-id="72922-137">Odkud se tento uživatel přihlásil?</span><span class="sxs-lookup"><span data-stu-id="72922-137">From where did this user sign-ins?</span></span>
- <span data-ttu-id="72922-138">Které má uživatel hello většina přihlášení a kde budou přihlášení z?</span><span class="sxs-lookup"><span data-stu-id="72922-138">Which user has hello most sign-ins and where do they sign-in from?</span></span> 
- <span data-ttu-id="72922-139">Úspěšné přihlášení hello?</span><span class="sxs-lookup"><span data-stu-id="72922-139">Was hello sign-in successful?</span></span>  
 
<span data-ttu-id="72922-140">Podrobnosti můžete rozbalit kliknutím na konkrétní datum nebo umístění.</span><span class="sxs-lookup"><span data-stu-id="72922-140">You can drill into details by clicking on a specific date or location.</span></span>

<span data-ttu-id="72922-141">**Jedineční uživatelé na aplikaci**: Získejte přehled všech jedinečných uživatelů, kteří danou aplikaci využívají.</span><span class="sxs-lookup"><span data-stu-id="72922-141">**Unique users per app**:  Get a view of all unique users using a given app.</span></span> <span data-ttu-id="72922-142">Patří sem pouze uživatelé, kteří se k aplikaci *úspěšně* přihlásili.</span><span class="sxs-lookup"><span data-stu-id="72922-142">This includes only users who have “*successfully*” signed into an application.</span></span>

<span data-ttu-id="72922-143">**Přihlášení zařízení**: získat zobrazení hello typ operačního systému a prohlížeče jsou používány uživatelé ve vaší organizaci s podrobné informace o hello uživatelů, včetně:</span><span class="sxs-lookup"><span data-stu-id="72922-143">**Device sign-ins**: Get a view of hello type of OS and browsers are being used by users in your organization with detailed information on hello users including:</span></span>

- <span data-ttu-id="72922-144">Uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="72922-144">User Name</span></span>
- <span data-ttu-id="72922-145">IP adresa</span><span class="sxs-lookup"><span data-stu-id="72922-145">IP Address</span></span>
- <span data-ttu-id="72922-146">Umístění</span><span class="sxs-lookup"><span data-stu-id="72922-146">Location</span></span> 
- <span data-ttu-id="72922-147">Stav přihlášení</span><span class="sxs-lookup"><span data-stu-id="72922-147">Sign-in status</span></span> 

<span data-ttu-id="72922-148">U této sestavy můžete porozumět hello různé profily zařízení používá v rámci vaší organizace a určete, podle toho, co je použije zásady zařízení</span><span class="sxs-lookup"><span data-stu-id="72922-148">With this report, you can understand hello various device profiles used within your organization and determine device policies based on what’s used</span></span>

<span data-ttu-id="72922-149">**Trychtýř SSPR**: Získejte představu o tom, jakým způsobem se ve vaší organizaci resetují hesla.</span><span class="sxs-lookup"><span data-stu-id="72922-149">**SSPR Funnel**: Get an understanding on how password resets are being done in your organization.</span></span> <span data-ttu-id="72922-150">Získání funkce Náhled na tom, kolik heslo resetovat pokusů o pomocí nástroje SSPR hello a kolik z nich byly úspěšné.</span><span class="sxs-lookup"><span data-stu-id="72922-150">Get a peek into how many password resets were attempted through hello SSPR tool and how many of them were successful.</span></span> <span data-ttu-id="72922-151">Chyby resetování hesla hello pomocí hello SSPR trychtýřového podrobněji a pochopit, proč došlo k určitým chybám.</span><span class="sxs-lookup"><span data-stu-id="72922-151">Dig deeper into hello Password resets failure using hello SSPR funnel and understand why certain failures occurred.</span></span> <span data-ttu-id="72922-152">Tato sestava umožňuje lépe pochopili, jak nástroj SSPR hello se používá v rámci vaší organizace, abyste měli hello správné rozhodnutí.</span><span class="sxs-lookup"><span data-stu-id="72922-152">This report gives a deeper understanding of how hello SSPR tool is used within your organization so you can make hello right decisions.</span></span>

## <a name="customizing-azure-ad-activity-content-pack"></a><span data-ttu-id="72922-153">Přizpůsobení balíčku obsahu aktivit služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="72922-153">Customizing Azure AD Activity content pack</span></span>

<span data-ttu-id="72922-154">**Změnit vizualizace**: vizualizace sestavy můžete změnit kliknutím **upravit sestavu** a vyberte hello vizualizace chcete.</span><span class="sxs-lookup"><span data-stu-id="72922-154">**Change Visualization**:  You can change a report visualization by clicking **Edit Report** and select hello visualization you want.</span></span>
 
![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/09.png) 
 
![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/10.png) 

<span data-ttu-id="72922-157">**Zahrnout další pole**: můžete přidat sestavu toohello pole nebo ho odebrat výběrem hello visual toowhich chcete pole hello tooadd nebo odebrat.</span><span class="sxs-lookup"><span data-stu-id="72922-157">**Include additional fields**:  You can add a field toohello report or remove it by selecting hello visual toowhich you want tooadd/remove hello field.</span></span> <span data-ttu-id="72922-158">V příkladu hello níže přidám zobrazení tabulky toohello pole "stav přihlášení".</span><span class="sxs-lookup"><span data-stu-id="72922-158">In hello example below, I am adding “sign-in status” field toohello table view.</span></span> 
 
![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/11.png) 

<span data-ttu-id="72922-160">**Řídicí panel tooyour vizualizace PIN**: přizpůsobit svůj řídicí panel, patří vlastní sestavu toohello vizualizace a připnout toohello řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="72922-160">**Pin visualizations tooyour dashboard**:  You can customize your dashboard and include your own visualizations toohello report and pin it toohello dashboard.</span></span> <span data-ttu-id="72922-161">V příkladu hello níže I přidán nový filtr, který se nazývá "stav přihlášení" a zahrnuty v sestavě hello.</span><span class="sxs-lookup"><span data-stu-id="72922-161">In hello example below, I added a new filter called “Sign-in Status” and included it in hello report.</span></span> <span data-ttu-id="72922-162">I taky hello vizualizace se změnil z pruhový graf tooa spojnicový graf a můžete Připnout tento nový visual toohello řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="72922-162">I also changed hello visualization from bar chart tooa line chart and can pin this new visual toohello dashboard.</span></span>

![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/12.png) 

![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/13.png) 
 

 


<span data-ttu-id="72922-165">**Sdílení řídicího panelu**: Po vytvoření hello obsah, který chcete, můžete sdílet hello řídicí panel s hello uživateli ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="72922-165">**Sharing your dashboard**: Once you have created hello content you want, you can share hello dashboard with hello users in your organization.</span></span> <span data-ttu-id="72922-166">Pamatovat si prosím, že jakmile hello sestavy sdílíte, uvidí vybraných v sestavě hello polí hello.</span><span class="sxs-lookup"><span data-stu-id="72922-166">Please remember that once you share hello report, they can see hello fields you have selected in hello report.</span></span>
 
![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/14.png) 



## <a name="scheduling-a-daily-refresh-of-your-power-bi-report"></a><span data-ttu-id="72922-168">Plánování denní aktualizace sestavy Power BI</span><span class="sxs-lookup"><span data-stu-id="72922-168">Scheduling a daily refresh of your Power BI report</span></span>

<span data-ttu-id="72922-169">tooschedule denní aktualizaci sestavy Power BI, přejděte příliš**datové sady > Nastavení > naplánovat aktualizaci** a nastavte ji, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="72922-169">tooschedule a daily refresh of your Power BI report, go too**Datasets > Settings > Schedule Refresh** and set it as shown below.</span></span>
 
![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/15.png) 

## <a name="updating-toonewer-version-of-content-pack"></a><span data-ttu-id="72922-171">Aktualizace toonewer verze obsahu sady</span><span class="sxs-lookup"><span data-stu-id="72922-171">Updating toonewer version of content pack</span></span>

<span data-ttu-id="72922-172">Pokud chcete, aby tooupdate obsah pack tooget novější verze:</span><span class="sxs-lookup"><span data-stu-id="72922-172">If you want tooupdate your content pack tooget a newer version:</span></span>

- <span data-ttu-id="72922-173">Stáhněte si nový balíček obsahu hello a nastavení podle pokynů uvedených v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="72922-173">Download hello new content pack and set it up as per instructions listed in this article.</span></span>

- <span data-ttu-id="72922-174">Jakmile jste nastavili tak, přejděte příliš**zdroj dat > Nastavení > přihlašovací údaje ke zdroji dat** a znova zadejte přihlašovací údaje, jak je uvedeno níže</span><span class="sxs-lookup"><span data-stu-id="72922-174">Once you have set it up, go too**Data Source > Settings > Data source credentials** and re-enter your credentials as shown below</span></span>

    ![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/16.png) 

<span data-ttu-id="72922-176">Jakmile hello novou verzi balíčku obsahu hello funguje, můžete odebrat předchozí verzi aplikace hello v případě potřeby odstraněním hello základní sestavy a datové sady přidružené k tento balíček obsahu.</span><span class="sxs-lookup"><span data-stu-id="72922-176">As soon as hello new version of hello content pack is working, you can remove hello old version if needed by deleting hello underlying reports and datasets associated with that content pack.</span></span>

## <a name="still-having-issues"></a><span data-ttu-id="72922-177">Pořád máte problémy?</span><span class="sxs-lookup"><span data-stu-id="72922-177">Still having issues?</span></span> 

<span data-ttu-id="72922-178">Podívejte se na [průvodce odstraňováním potíží](active-directory-reporting-troubleshoot-content-pack.md).</span><span class="sxs-lookup"><span data-stu-id="72922-178">Check out our [troubleshooting guide](active-directory-reporting-troubleshoot-content-pack.md).</span></span> <span data-ttu-id="72922-179">Obecnou nápovědu k Power BI najdete v těchto [článcích nápovědy](https://powerbi.microsoft.com/en-us/documentation/powerbi-service-get-started/).</span><span class="sxs-lookup"><span data-stu-id="72922-179">For general help with Power BI, check out these [help articles](https://powerbi.microsoft.com/en-us/documentation/powerbi-service-get-started/).</span></span>
 

## <a name="next-steps"></a><span data-ttu-id="72922-180">Další kroky</span><span class="sxs-lookup"><span data-stu-id="72922-180">Next steps</span></span>

<span data-ttu-id="72922-181">Přehled vytváření sestav najdete v tématu hello [generování sestav Azure Active Directory](active-directory-reporting-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="72922-181">For an overview of reporting, see hello [Azure Active Directory reporting](active-directory-reporting-azure-portal.md).</span></span>
