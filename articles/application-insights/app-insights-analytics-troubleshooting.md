---
title: "Řešení potíží s Analytics ve službě Azure Application Insights | Microsoft Docs"
description: "Problémy s Application Insights analytics? Začněte tady. "
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 9bbd5859-3584-4d80-9b6d-d5910fa48baa
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/11/2016
ms.author: bwren
ms.openlocfilehash: 02df117908fc1790e8cfb9ec0a7218c1b8be856c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="troubleshoot-analytics-in-application-insights"></a><span data-ttu-id="b1d6b-104">Řešení potíží s analýzami v nástroji Application Insights</span><span class="sxs-lookup"><span data-stu-id="b1d6b-104">Troubleshoot Analytics in Application Insights</span></span>
<span data-ttu-id="b1d6b-105">Problémy s [Application Insights Analytics](app-insights-analytics.md)?</span><span class="sxs-lookup"><span data-stu-id="b1d6b-105">Problems with [Application Insights Analytics](app-insights-analytics.md)?</span></span> <span data-ttu-id="b1d6b-106">Začněte tady.</span><span class="sxs-lookup"><span data-stu-id="b1d6b-106">Start here.</span></span> <span data-ttu-id="b1d6b-107">Analytics je nástroj výkonné vyhledávání systému Azure Application Insights.</span><span class="sxs-lookup"><span data-stu-id="b1d6b-107">Analytics is the powerful search tool of Azure Application Insights.</span></span>

## <a name="limits"></a><span data-ttu-id="b1d6b-108">Omezení</span><span class="sxs-lookup"><span data-stu-id="b1d6b-108">Limits</span></span>
* <span data-ttu-id="b1d6b-109">V současné době jsou omezená na právě přes týden posledních dat výsledky dotazu.</span><span class="sxs-lookup"><span data-stu-id="b1d6b-109">At present, query results are limited to just over a week of past data.</span></span>
* <span data-ttu-id="b1d6b-110">Prohlížeče jsme test na: nejnovější vydání systému Chrome, okraj a prohlížeče Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="b1d6b-110">Browsers we test on: latest editions of Chrome, Edge, and Internet Explorer.</span></span>

## <a name="known-incompatible-browser-extensions"></a><span data-ttu-id="b1d6b-111">Rozšíření známé kompatibilní prohlížeče</span><span class="sxs-lookup"><span data-stu-id="b1d6b-111">Known incompatible browser extensions</span></span>
* <span data-ttu-id="b1d6b-112">Ghostery</span><span class="sxs-lookup"><span data-stu-id="b1d6b-112">Ghostery</span></span>

<span data-ttu-id="b1d6b-113">Rozšíření zakázat, nebo použijte jiný prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="b1d6b-113">Disable the extension or use a different browser.</span></span>

## <span data-ttu-id="b1d6b-114"><a name="e-a"></a>"Neočekávané error.</span><span class="sxs-lookup"><span data-stu-id="b1d6b-114"><a name="e-a"></a> "Unexpected error"</span></span>
![Došlo k neočekávané chybě obrazovky](./media/app-insights-analytics-troubleshooting/010.png)

<span data-ttu-id="b1d6b-116">Běhovém portálu – neošetřené výjimky došlo k vnitřní chybě.</span><span class="sxs-lookup"><span data-stu-id="b1d6b-116">Internal error occurred during portal runtime – unhandled exception.</span></span>

* <span data-ttu-id="b1d6b-117">Vyčištění mezipaměti prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="b1d6b-117">Clean the browser's cache.</span></span> 

## <span data-ttu-id="b1d6b-118"><a name="e-b"></a>403... Zkuste to prosím znovu načíst</span><span class="sxs-lookup"><span data-stu-id="b1d6b-118"><a name="e-b"></a>403 ... please try to reload</span></span>
![403... Zkuste to prosím znovu načíst](./media/app-insights-analytics-troubleshooting/020.png)

<span data-ttu-id="b1d6b-120">(Během ověřování nebo během generování tokenů přístupu) došlo k chybě související s ověřování.</span><span class="sxs-lookup"><span data-stu-id="b1d6b-120">An authentication related error occurred (during authentication or during access token generation).</span></span> <span data-ttu-id="b1d6b-121">Na portálu může mít žádný způsob, jak obnovit beze změny nastavení prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="b1d6b-121">The portal may have no way to  recover without changing browser settings.</span></span>

* <span data-ttu-id="b1d6b-122">Ověřte [jsou povolené soubory cookie třetích stran](#cookies) v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="b1d6b-122">Verify [third party cookies are enabled](#cookies) in the browser.</span></span> 

## <span data-ttu-id="b1d6b-123"><a name="authentication"></a>403... Ověřte zóny zabezpečení</span><span class="sxs-lookup"><span data-stu-id="b1d6b-123"><a name="authentication"></a>403 ... verify security zone</span></span>
![403.. .verify zóny zabezpečení](./media/app-insights-analytics-troubleshooting/030.png)

<span data-ttu-id="b1d6b-125">(Během ověřování nebo během generování tokenů přístupu) došlo k chybě související s ověřování.</span><span class="sxs-lookup"><span data-stu-id="b1d6b-125">An authentication related error occurred (during authentication or during access token generation).</span></span> <span data-ttu-id="b1d6b-126">Na portálu může mít žádný způsob, jak obnovit beze změny nastavení prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="b1d6b-126">The portal may have no way to  recover without changing browser settings.</span></span>

1. <span data-ttu-id="b1d6b-127">Ověřte [jsou povolené soubory cookie třetích stran](#cookies) v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="b1d6b-127">Verify [third party cookies are enabled](#cookies) in the browser.</span></span> 
2. <span data-ttu-id="b1d6b-128">Můžete pomocí Oblíbené položky., záložku nebo uložený odkaz otevřete portál Analytics?</span><span class="sxs-lookup"><span data-stu-id="b1d6b-128">Did you use a favorite, bookmark or saved link to open the Analytics portal?</span></span> <span data-ttu-id="b1d6b-129">Jsou jste přihlášeni s použitím různých přihlašovacích údajů, než jste použili při uložení odkazu?</span><span class="sxs-lookup"><span data-stu-id="b1d6b-129">Are you signed in with different credentials than you used when you saved the link?</span></span>
3. <span data-ttu-id="b1d6b-130">Zkuste použít okno prohlížeče v privátní nebo incognito (po zavření všechny takové windows).</span><span class="sxs-lookup"><span data-stu-id="b1d6b-130">Try using an in-private/incognito browser window (after closing all such windows).</span></span> <span data-ttu-id="b1d6b-131">Budete muset zadávat svoje přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="b1d6b-131">You'll have to provide your credentials.</span></span> 
4. <span data-ttu-id="b1d6b-132">Otevře další okno (obyčejnou) prohlížeče a přejděte na [Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b1d6b-132">Open another (ordinary) browser window and go to [Azure](https://portal.azure.com).</span></span> <span data-ttu-id="b1d6b-133">Odhlaste se. Poté otevřete odkaz a přihlaste se pomocí správné přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="b1d6b-133">Sign out. Then open your link and sign in with the correct credentials.</span></span>
5. <span data-ttu-id="b1d6b-134">Uživatelé okraj a prohlížeče Internet Explorer můžete také získat tuto chybu při nastavení zóny důvěryhodných serverů nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="b1d6b-134">Edge and Internet Explorer users can also get this error when trusted zone settings are not supported.</span></span>
   
    <span data-ttu-id="b1d6b-135">Ověřte obě [portálu analýza](https://analytics.applicationinsights.io) a [portál Azure Active Directory](https://portal.azure.com) jsou ve stejné zóny zabezpečení:</span><span class="sxs-lookup"><span data-stu-id="b1d6b-135">Verify both [Analytics portal](https://analytics.applicationinsights.io) and [Azure Active Directory portal](https://portal.azure.com) are in the same security zone:</span></span>
   
   * <span data-ttu-id="b1d6b-136">V Internet Exploreru otevřete **Možnosti Internetu**, **zabezpečení**, **Důvěryhodné servery**, **lokality**:</span><span class="sxs-lookup"><span data-stu-id="b1d6b-136">In Internet Explorer, open **Internet Options**, **Security**, **Trusted sites**, **Sites**:</span></span>
     
     ![Dialogové okno Možnosti Internetu, přidání webu do důvěryhodných serverů](./media/app-insights-analytics-troubleshooting/033.png)
     
     <span data-ttu-id="b1d6b-138">V seznamu webů Pokud žádné z těchto adres URL jsou zahrnuty, ujistěte se, že ostatní jsou zahrnuty také:</span><span class="sxs-lookup"><span data-stu-id="b1d6b-138">In the Websites list, if any of the following URLs are included, make sure that the others are included also:</span></span>
     
     <span data-ttu-id="b1d6b-139">https://Analytics.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="b1d6b-139">https://analytics.applicationinsights.io</span></span><br/>
     <span data-ttu-id="b1d6b-140">https://login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="b1d6b-140">https://login.microsoftonline.com</span></span><br/>
     <span data-ttu-id="b1d6b-141">https://login.windows.net</span><span class="sxs-lookup"><span data-stu-id="b1d6b-141">https://login.windows.net</span></span>

## <span data-ttu-id="b1d6b-142"><a name="e-d"></a>404 ... Prostředek se nenašel</span><span class="sxs-lookup"><span data-stu-id="b1d6b-142"><a name="e-d"></a>404 ... Resource not found</span></span>
![404... prostředek nebyl nalezen](./media/app-insights-analytics-troubleshooting/040.png)

<span data-ttu-id="b1d6b-144">Prostředek aplikace byla odstraněna z Application Insights a už není dostupný.</span><span class="sxs-lookup"><span data-stu-id="b1d6b-144">Application resource was deleted from Application Insights and isn’t available anymore.</span></span> <span data-ttu-id="b1d6b-145">To může dojít, pokud jste uložili adresu URL na stránku Analytics.</span><span class="sxs-lookup"><span data-stu-id="b1d6b-145">This can happen if you saved the URL to the Analytics page.</span></span>

## <span data-ttu-id="b1d6b-146"><a name="e-e"></a>403 ... Žádné oprávnění</span><span class="sxs-lookup"><span data-stu-id="b1d6b-146"><a name="e-e"></a>403 ... No authorization</span></span>
![403... neautorizovaných](./media/app-insights-analytics-troubleshooting/050.png)

<span data-ttu-id="b1d6b-148">Nemáte oprávnění k otevření této aplikace v Analytics.</span><span class="sxs-lookup"><span data-stu-id="b1d6b-148">You don't have permission to open this application in Analytics.</span></span>

* <span data-ttu-id="b1d6b-149">Obdrželi jste od někoho jiného odkaz?</span><span class="sxs-lookup"><span data-stu-id="b1d6b-149">Did you get the link from someone else?</span></span> <span data-ttu-id="b1d6b-150">Požádejte je o zkontrolujte, zda jsou v [čtečky nebo přispěvatele pro tuto skupinu prostředků](app-insights-resources-roles-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="b1d6b-150">Ask them to make sure you are in the [readers or contributors for this resource group](app-insights-resources-roles-access-control.md).</span></span>
* <span data-ttu-id="b1d6b-151">Uložíte odkaz použitím různých přihlašovacích údajů?</span><span class="sxs-lookup"><span data-stu-id="b1d6b-151">Did you save the link using different credentials?</span></span> <span data-ttu-id="b1d6b-152">Otevřete [portál Azure](https://portal.azure.com), odhlásit se a potom zkuste tohoto propojení znovu, zajištění správné přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="b1d6b-152">Open the [Azure portal](https://portal.azure.com), sign out, and then try this link again, providing the correct credentials.</span></span>

## <span data-ttu-id="b1d6b-153"><a name="html-storage"></a>403 ... Úložiště HTML5</span><span class="sxs-lookup"><span data-stu-id="b1d6b-153"><a name="html-storage"></a>403 ... HTML5 Storage</span></span>
<span data-ttu-id="b1d6b-154">Náš portál používá HTML5 localStorage a sessionStorage.</span><span class="sxs-lookup"><span data-stu-id="b1d6b-154">Our portal uses HTML5 localStorage and sessionStorage.</span></span>

* <span data-ttu-id="b1d6b-155">Chrome: Nastavení ochrany osobních údajů, nastavení obsahu.</span><span class="sxs-lookup"><span data-stu-id="b1d6b-155">Chrome: Settings, privacy, content settings.</span></span>
* <span data-ttu-id="b1d6b-156">Internet Explorer: Možnosti Internetu, karta Upřesnit, zabezpečení, povolte úložiště DOM</span><span class="sxs-lookup"><span data-stu-id="b1d6b-156">Internet Explorer: Internet Options, Advanced tab, Security, Enable DOM Storage</span></span>

![403... Zkuste povolit úložiště HTML5](./media/app-insights-analytics-troubleshooting/060.png)

## <span data-ttu-id="b1d6b-158"><a name="e-g"></a>404 ... Odběr nebyl nalezen</span><span class="sxs-lookup"><span data-stu-id="b1d6b-158"><a name="e-g"></a>404 ... Subscription not found</span></span>
![404 ... Odběr nebyl nalezen](./media/app-insights-analytics-troubleshooting/070.png)

<span data-ttu-id="b1d6b-160">Adresa URL je neplatná.</span><span class="sxs-lookup"><span data-stu-id="b1d6b-160">The URL is invalid.</span></span> 

* <span data-ttu-id="b1d6b-161">Otevřete prostředek aplikace v [portál Application Insights](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b1d6b-161">Open the app resource in [Application Insights portal](https://portal.azure.com).</span></span> <span data-ttu-id="b1d6b-162">Potom pomocí tlačítka Analytics.</span><span class="sxs-lookup"><span data-stu-id="b1d6b-162">Then use the Analytics button.</span></span>

## <span data-ttu-id="b1d6b-163"><a name="e-h"></a>404... stránka neexistuje</span><span class="sxs-lookup"><span data-stu-id="b1d6b-163"><a name="e-h"></a>404 ... page doesn't exist</span></span>
![404 ... Stránka neexistuje.](./media/app-insights-analytics-troubleshooting/080.png)

<span data-ttu-id="b1d6b-165">Adresa URL je neplatná.</span><span class="sxs-lookup"><span data-stu-id="b1d6b-165">The URL is invalid.</span></span>

* <span data-ttu-id="b1d6b-166">Otevřete prostředek aplikace v [portál Application Insights](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b1d6b-166">Open the app resource in [Application Insights portal](https://portal.azure.com).</span></span> <span data-ttu-id="b1d6b-167">Potom pomocí tlačítka Analytics.</span><span class="sxs-lookup"><span data-stu-id="b1d6b-167">Then use the Analytics button.</span></span>

## <span data-ttu-id="b1d6b-168"><a name="cookies"></a>Povolit soubory cookie třetích stran</span><span class="sxs-lookup"><span data-stu-id="b1d6b-168"><a name="cookies"></a>Enable third-party cookies</span></span>
  <span data-ttu-id="b1d6b-169">V tématu [zakázání soubory cookie třetích stran](http://www.digitalcitizen.life/how-disable-third-party-cookies-all-major-browsers), ale Všimněte si, musíme **povolit** je.</span><span class="sxs-lookup"><span data-stu-id="b1d6b-169">See [how to disable third party cookies](http://www.digitalcitizen.life/how-disable-third-party-cookies-all-major-browsers), but notice we need to **enable** them.</span></span>


[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

