---
title: "Jak používat balíček obsahu Azure Active Directory Power BI Content Pack | Dokumentace Microsoftu"
description: "Naučte se používat balíček obsahu Azure Active Directory Power BI Content Pack."
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
ms.openlocfilehash: 5c642bb814a279756e8391f12fdc86b6ec0b4a8f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-use-the-azure-active-directory-power-bi-content-pack"></a><span data-ttu-id="19f5e-103">Jak používat balíček obsahu Azure Active Directory Power BI Content Pack</span><span class="sxs-lookup"><span data-stu-id="19f5e-103">How to use the Azure Active Directory Power BI Content Pack</span></span>

<span data-ttu-id="19f5e-104">Pro vás jako správce IT je naprosto nezbytné pochopit, jakým způsobem uživatelé přijímají a používají funkce služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="19f5e-104">Understanding how your users adopt and use Azure Active Directory features is critical for you as an IT admin.</span></span> <span data-ttu-id="19f5e-105">Umožní vám to plánovat infrastrukturu IT a komunikaci za účelem zvýšení míry využití a získání maxima z funkcí AAD.</span><span class="sxs-lookup"><span data-stu-id="19f5e-105">It allows you to plan your IT infrastructure and communication to increase usage and to get the most out of AAD features.</span></span> <span data-ttu-id="19f5e-106">Balíček obsahu Power BI pro Azure Active Directory vám umožňuje podrobněji analyzovat data, abyste pochopili, jakým způsobem je můžete používat k získání lepšího přehledu o tom, jak služba Azure Active Directory funguje s různými možnostmi, na které spoléháte.</span><span class="sxs-lookup"><span data-stu-id="19f5e-106">Power BI Content Pack for Azure Active Directory gives you the ability to further analyze your data to understand how you can use this data to gather richer insights into what’s going on with their Azure Active Directory for the various capabilities you heavily rely on.</span></span>  <span data-ttu-id="19f5e-107">Integrace rozhraní Azure Active Directory API do Power BI umožňuje snadné stažení předem vytvořených balíčků obsahu a získání přehledu o veškerých aktivitách v rámci služby Azure Active Directory pomocí bohatých funkcí vizualizace, které Power BI nabízí.</span><span class="sxs-lookup"><span data-stu-id="19f5e-107">With the integration of Azure Active Directory APIs into Power BI, you can easily download the pre-built content packs and gain insights to all the activities within your Azure Active Directory using rich visualization experience Power BI offers.</span></span> <span data-ttu-id="19f5e-108">Můžete si vytvořit vlastní řídicí panel a snadno ho sdílet se všemi uživateli ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="19f5e-108">You can create your own dashboard and share it easily with anyone in your organization.</span></span> 

<span data-ttu-id="19f5e-109">Toto téma obsahuje podrobné pokyny pro instalaci a použití balíčku obsahu ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="19f5e-109">This topic provides you with step-by-step instructions on how to install and use the content pack in your environment.</span></span>

## <a name="installation"></a><span data-ttu-id="19f5e-110">Instalace</span><span class="sxs-lookup"><span data-stu-id="19f5e-110">Installation</span></span>  

<span data-ttu-id="19f5e-111">**Instalace balíčku obsahu Power BI:**</span><span class="sxs-lookup"><span data-stu-id="19f5e-111">**To install the Power BI Content Pack:**</span></span>

1. <span data-ttu-id="19f5e-112">Přihlaste se k [Power BI](https://app.powerbi.com/groups/me/getdata/services) pomocí svého účtu Power BI (jedná se o stejný účet jako pro O365 nebo Azure AD).</span><span class="sxs-lookup"><span data-stu-id="19f5e-112">Log into [Power BI](https://app.powerbi.com/groups/me/getdata/services) with your Power BI Account (this is the same account as your O365 or Azure AD Account).</span></span>

2. <span data-ttu-id="19f5e-113">V dolní části levého navigačního podokna vyberte **Získat data**.</span><span class="sxs-lookup"><span data-stu-id="19f5e-113">At the bottom of the left navigation pane, select **Get Data**.</span></span>

    ![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/01.png)
 
3. <span data-ttu-id="19f5e-115">V okně **Služby** klikněte na **Získat**.</span><span class="sxs-lookup"><span data-stu-id="19f5e-115">In the **Services** box, click **Get**.</span></span>
   
    ![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/02.png)

4.  <span data-ttu-id="19f5e-117">Vyhledejte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="19f5e-117">Search for **Azure Active Directory**.</span></span>

    ![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/03.png)
 
5.  <span data-ttu-id="19f5e-119">Po zobrazení výzvy zadejte ID klienta služby Azure AD a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="19f5e-119">When prompted, type your Azure AD Tenant ID, and then click **Next**.</span></span>

    > [!TIP] 
    > <span data-ttu-id="19f5e-120">Rychlý způsob, jak získat ID klienta vašeho klienta Office 365 / Azure AD, je přihlásit se k portálu Azure AD, přejít k adresáři a zkopírovat ID z následující adresy URL: https://manage.windowsazure.com/woodgroveonline.com#Workspaces/ActiveDirectoryExtension/Directory/<tenantid>/directoryQuickStart</span><span class="sxs-lookup"><span data-stu-id="19f5e-120">A quick way to get the Tenant Id for your Office 365 / Azure AD tenant is to login to the Azure AD Portal, drill down to the directory and copy the ID from the following URL: https://manage.windowsazure.com/woodgroveonline.com#Workspaces/ActiveDirectoryExtension/Directory/<tenantid>/directoryQuickStart</span></span>

    ![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/04.png) 

6.  <span data-ttu-id="19f5e-122">Klikněte na **Přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="19f5e-122">Click **Sign-in**.</span></span> 
 
    ![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/05.png) 



7.  <span data-ttu-id="19f5e-124">Zadejte uživatelské jméno a heslo a pak klikněte na **Přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="19f5e-124">Enter your username and password, and then click **Sign in**.</span></span>
 
    ![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/06.png) 

8.  <span data-ttu-id="19f5e-126">V dialogovém okně souhlasu aplikace klikněte na **Přijmout**.</span><span class="sxs-lookup"><span data-stu-id="19f5e-126">On the app consent dialog, click **Accept**.</span></span>
 
9.  <span data-ttu-id="19f5e-127">Jakmile řídicí panel protokolů aktivity služby Azure Active Directory vytvoříte, klikněte na něj.</span><span class="sxs-lookup"><span data-stu-id="19f5e-127">When your Azure Active Directory Activity logs dashboard has been created, click it.</span></span>
 
    ![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/08.png) 

## <a name="what-can-i-do-with-this-content-pack"></a><span data-ttu-id="19f5e-129">Co můžu s tímto balíčkem obsahu dělat?</span><span class="sxs-lookup"><span data-stu-id="19f5e-129">What can I do with this content pack?</span></span>

<span data-ttu-id="19f5e-130">Než se začneme zabývat tím, co s tímto balíčkem obsahu můžete dělat, podívejme se na rychlý přehled různých sestav, které balíček obsahu obsahuje.</span><span class="sxs-lookup"><span data-stu-id="19f5e-130">Before we jump into what you can do with this content pack, here’s quick preview of the various reports in the content pack.</span></span> <span data-ttu-id="19f5e-131">Data sestavy se týkají **posledních 30 dní**.</span><span class="sxs-lookup"><span data-stu-id="19f5e-131">Report data goes back to the **last 30 days**.</span></span>

### <a name="reports-included-in-this-version-of-azure-active-directory-logs-content-pack"></a><span data-ttu-id="19f5e-132">Sestavy zahrnuté v této verzi balíčku obsahu protokolů služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="19f5e-132">Reports included in this version of Azure Active Directory logs Content Pack</span></span>

<span data-ttu-id="19f5e-133">**Sestava využití aplikací a trendů**: Získejte přehled o aplikacích používaných ve vaší organizaci a o tom, které se používají nejvíce a kdy.</span><span class="sxs-lookup"><span data-stu-id="19f5e-133">**App Usage and Trend report**:  Get insights into the apps used in your organization and which ones are being used the most and when.</span></span> <span data-ttu-id="19f5e-134">Tato sestava slouží k získání přehledu o tom, jak se aplikace, kterou jste ve vaší organizaci nedávno zavedli, používá, nebo ke zjištění oblíbených aplikací.</span><span class="sxs-lookup"><span data-stu-id="19f5e-134">You can use this report to gather insights into how an app you recently rolled out in your organization is being used or find out which apps are popular.</span></span> <span data-ttu-id="19f5e-135">Umožní vám vylepšit využití v případě, že vidíte, že se aplikace nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="19f5e-135">By doing this, you can improve usage if you see if the app is not being used.</span></span>

<span data-ttu-id="19f5e-136">**Přihlášení podle umístění a uživatelů**: Získejte přehled o veškerých přihlášeních provedených pomocí Azure Identity a přehled o identitách uživatelů.</span><span class="sxs-lookup"><span data-stu-id="19f5e-136">**Sign-ins by location and users**: Get insights into all the sign-ins performed using Azure Identity and gives insights into the identity of the users.</span></span> <span data-ttu-id="19f5e-137">Můžete podrobněji analyzovat individuální přihlášení a odpovědět si na otázky jako:</span><span class="sxs-lookup"><span data-stu-id="19f5e-137">With this, you can dig deeper into individual sign-ins and answer questions like:</span></span>

- <span data-ttu-id="19f5e-138">Odkud se tento uživatel přihlásil?</span><span class="sxs-lookup"><span data-stu-id="19f5e-138">From where did this user sign-ins?</span></span>
- <span data-ttu-id="19f5e-139">Který uživatel má nejvíce přihlášení a odkud se přihlašuje?</span><span class="sxs-lookup"><span data-stu-id="19f5e-139">Which user has the most sign-ins and where do they sign-in from?</span></span> 
- <span data-ttu-id="19f5e-140">Bylo přihlášení úspěšné?</span><span class="sxs-lookup"><span data-stu-id="19f5e-140">Was the sign-in successful?</span></span>  
 
<span data-ttu-id="19f5e-141">Podrobnosti můžete rozbalit kliknutím na konkrétní datum nebo umístění.</span><span class="sxs-lookup"><span data-stu-id="19f5e-141">You can drill into details by clicking on a specific date or location.</span></span>

<span data-ttu-id="19f5e-142">**Jedineční uživatelé na aplikaci**: Získejte přehled všech jedinečných uživatelů, kteří danou aplikaci využívají.</span><span class="sxs-lookup"><span data-stu-id="19f5e-142">**Unique users per app**:  Get a view of all unique users using a given app.</span></span> <span data-ttu-id="19f5e-143">Patří sem pouze uživatelé, kteří se k aplikaci *úspěšně* přihlásili.</span><span class="sxs-lookup"><span data-stu-id="19f5e-143">This includes only users who have “*successfully*” signed into an application.</span></span>

<span data-ttu-id="19f5e-144">**Přihlášení pomocí zařízení**: Získejte přehled o typu operačního systému a prohlížečích používaných uživateli ve vaší organizaci s podrobnými informacemi o uživatelích, včetně:</span><span class="sxs-lookup"><span data-stu-id="19f5e-144">**Device sign-ins**: Get a view of the type of OS and browsers are being used by users in your organization with detailed information on the users including:</span></span>

- <span data-ttu-id="19f5e-145">Uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="19f5e-145">User Name</span></span>
- <span data-ttu-id="19f5e-146">IP adresa</span><span class="sxs-lookup"><span data-stu-id="19f5e-146">IP Address</span></span>
- <span data-ttu-id="19f5e-147">Umístění</span><span class="sxs-lookup"><span data-stu-id="19f5e-147">Location</span></span> 
- <span data-ttu-id="19f5e-148">Stav přihlášení</span><span class="sxs-lookup"><span data-stu-id="19f5e-148">Sign-in status</span></span> 

<span data-ttu-id="19f5e-149">Tato sestava vám pomůže pochopit různé profily zařízení používané ve vaší organizaci a určit zásady zařízení podle toho, co se používá.</span><span class="sxs-lookup"><span data-stu-id="19f5e-149">With this report, you can understand the various device profiles used within your organization and determine device policies based on what’s used</span></span>

<span data-ttu-id="19f5e-150">**Trychtýř SSPR**: Získejte představu o tom, jakým způsobem se ve vaší organizaci resetují hesla.</span><span class="sxs-lookup"><span data-stu-id="19f5e-150">**SSPR Funnel**: Get an understanding on how password resets are being done in your organization.</span></span> <span data-ttu-id="19f5e-151">Zjistěte, kolik pokusů o resetování hesla bylo provedeno prostřednictvím nástroje SSPR a kolik z nich bylo úspěšných.</span><span class="sxs-lookup"><span data-stu-id="19f5e-151">Get a peek into how many password resets were attempted through the SSPR tool and how many of them were successful.</span></span> <span data-ttu-id="19f5e-152">Podívejte se podrobněji na chyby při resetování hesel pomocí trychtýře SSPR a zjistěte, proč k některým chybám došlo.</span><span class="sxs-lookup"><span data-stu-id="19f5e-152">Dig deeper into the Password resets failure using the SSPR funnel and understand why certain failures occurred.</span></span> <span data-ttu-id="19f5e-153">Tato sestava umožňuje lépe pochopit, jakým způsobem se nástroj SSPR ve vaší organizaci používá. Umožní vám pak správně se rozhodovat.</span><span class="sxs-lookup"><span data-stu-id="19f5e-153">This report gives a deeper understanding of how the SSPR tool is used within your organization so you can make the right decisions.</span></span>

## <a name="customizing-azure-ad-activity-content-pack"></a><span data-ttu-id="19f5e-154">Přizpůsobení balíčku obsahu aktivit služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="19f5e-154">Customizing Azure AD Activity content pack</span></span>

<span data-ttu-id="19f5e-155">**Změna vizualizace**: Vizualizaci sestavy můžete změnit kliknutím na **Upravit sestavu** a výběrem požadované vizualizace.</span><span class="sxs-lookup"><span data-stu-id="19f5e-155">**Change Visualization**:  You can change a report visualization by clicking **Edit Report** and select the visualization you want.</span></span>
 
![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/09.png) 
 
![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/10.png) 

<span data-ttu-id="19f5e-158">**Zahrnutí dalších polí**: Pole můžete do sestavy přidat nebo ho z ní odebrat výběrem vizuálu, do kterého chcete pole přidat nebo z kterého ho chcete odebrat.</span><span class="sxs-lookup"><span data-stu-id="19f5e-158">**Include additional fields**:  You can add a field to the report or remove it by selecting the visual to which you want to add/remove the field.</span></span> <span data-ttu-id="19f5e-159">V následujícím příkladu se přidává pole „stav přihlášení“ do zobrazení tabulky.</span><span class="sxs-lookup"><span data-stu-id="19f5e-159">In the example below, I am adding “sign-in status” field to the table view.</span></span> 
 
![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/11.png) 

<span data-ttu-id="19f5e-161">**Připnutí vizualizací na řídicí panel**: Řídicí panel můžete přizpůsobit, do sestavy můžete zahrnout vlastní vizualizace a sestavu pak připnout na řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="19f5e-161">**Pin visualizations to your dashboard**:  You can customize your dashboard and include your own visualizations to the report and pin it to the dashboard.</span></span> <span data-ttu-id="19f5e-162">V následujícím příkladu se přidává nový filtr s názvem „stav přihlášení“ a zahrnuje se do sestavy.</span><span class="sxs-lookup"><span data-stu-id="19f5e-162">In the example below, I added a new filter called “Sign-in Status” and included it in the report.</span></span> <span data-ttu-id="19f5e-163">Došlo také ke změně vizualizace z pruhového grafu na spojnicový. Tento nový vizuál se dá připnout na řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="19f5e-163">I also changed the visualization from bar chart to a line chart and can pin this new visual to the dashboard.</span></span>

![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/12.png) 

![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/13.png) 
 

 


<span data-ttu-id="19f5e-166">**Sdílení řídicího panelu**: Jakmile vytvoříte požadovaný obsah, můžete řídicí panel sdílet s uživateli ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="19f5e-166">**Sharing your dashboard**: Once you have created the content you want, you can share the dashboard with the users in your organization.</span></span> <span data-ttu-id="19f5e-167">Mějte prosím na paměti, že jakmile sestavu sdílíte, můžou uživatelé vidět pole, která jste v sestavě vybrali.</span><span class="sxs-lookup"><span data-stu-id="19f5e-167">Please remember that once you share the report, they can see the fields you have selected in the report.</span></span>
 
![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/14.png) 



## <a name="scheduling-a-daily-refresh-of-your-power-bi-report"></a><span data-ttu-id="19f5e-169">Plánování denní aktualizace sestavy Power BI</span><span class="sxs-lookup"><span data-stu-id="19f5e-169">Scheduling a daily refresh of your Power BI report</span></span>

<span data-ttu-id="19f5e-170">Pokud chcete naplánovat každodenní aktualizaci sestavy Power BI, přejděte na **Datové sady > Nastavení > Naplánovat aktualizaci** a nastavte ji podle příkladu níže.</span><span class="sxs-lookup"><span data-stu-id="19f5e-170">To schedule a daily refresh of your Power BI report, go to **Datasets > Settings > Schedule Refresh** and set it as shown below.</span></span>
 
![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/15.png) 

## <a name="updating-to-newer-version-of-content-pack"></a><span data-ttu-id="19f5e-172">Aktualizace na novější verzi balíčku obsahu</span><span class="sxs-lookup"><span data-stu-id="19f5e-172">Updating to newer version of content pack</span></span>

<span data-ttu-id="19f5e-173">Postup aktualizace balíčku obsahu na novější verzi:</span><span class="sxs-lookup"><span data-stu-id="19f5e-173">If you want to update your content pack to get a newer version:</span></span>

- <span data-ttu-id="19f5e-174">Stáhněte si nový balíček obsahu a nastavte ho podle pokynů uvedených v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="19f5e-174">Download the new content pack and set it up as per instructions listed in this article.</span></span>

- <span data-ttu-id="19f5e-175">Jakmile ho nastavíte, přejděte na **Zdroj dat > Nastavení > Přihlašovací údaje ke zdroji dat** a znovu zadejte přihlašovací údaje, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="19f5e-175">Once you have set it up, go to **Data Source > Settings > Data source credentials** and re-enter your credentials as shown below</span></span>

    ![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/16.png) 

<span data-ttu-id="19f5e-177">Jakmile bude nová verze balíčku obsahu funkční, můžete v případě potřeby odebrat starou verzi odstraněním podkladových sestav a databází přidružených danému balíčku obsahu.</span><span class="sxs-lookup"><span data-stu-id="19f5e-177">As soon as the new version of the content pack is working, you can remove the old version if needed by deleting the underlying reports and datasets associated with that content pack.</span></span>

## <a name="still-having-issues"></a><span data-ttu-id="19f5e-178">Pořád máte problémy?</span><span class="sxs-lookup"><span data-stu-id="19f5e-178">Still having issues?</span></span> 

<span data-ttu-id="19f5e-179">Podívejte se na [průvodce odstraňováním potíží](active-directory-reporting-troubleshoot-content-pack.md).</span><span class="sxs-lookup"><span data-stu-id="19f5e-179">Check out our [troubleshooting guide](active-directory-reporting-troubleshoot-content-pack.md).</span></span> <span data-ttu-id="19f5e-180">Obecnou nápovědu k Power BI najdete v těchto [článcích nápovědy](https://powerbi.microsoft.com/en-us/documentation/powerbi-service-get-started/).</span><span class="sxs-lookup"><span data-stu-id="19f5e-180">For general help with Power BI, check out these [help articles](https://powerbi.microsoft.com/en-us/documentation/powerbi-service-get-started/).</span></span>
 

## <a name="next-steps"></a><span data-ttu-id="19f5e-181">Další kroky</span><span class="sxs-lookup"><span data-stu-id="19f5e-181">Next steps</span></span>

<span data-ttu-id="19f5e-182">Přehled generování sestav najdete v tématu [Generování sestav v Azure Active Directory](active-directory-reporting-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="19f5e-182">For an overview of reporting, see the [Azure Active Directory reporting](active-directory-reporting-azure-portal.md).</span></span>
