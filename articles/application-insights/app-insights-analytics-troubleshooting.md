---
title: "aaaTroubleshoot Analytics ve službě Azure Application Insights | Microsoft Docs"
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
ms.openlocfilehash: e3be2fbc0237440d3b8a736484434a9f276296c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-analytics-in-application-insights"></a><span data-ttu-id="152ed-104">Řešení potíží s analýzami v nástroji Application Insights</span><span class="sxs-lookup"><span data-stu-id="152ed-104">Troubleshoot Analytics in Application Insights</span></span>
<span data-ttu-id="152ed-105">Problémy s [Application Insights Analytics](app-insights-analytics.md)?</span><span class="sxs-lookup"><span data-stu-id="152ed-105">Problems with [Application Insights Analytics](app-insights-analytics.md)?</span></span> <span data-ttu-id="152ed-106">Začněte tady.</span><span class="sxs-lookup"><span data-stu-id="152ed-106">Start here.</span></span> <span data-ttu-id="152ed-107">Analytics je nástroj pro efektivní vyhledávání hello služby Azure Application Insights.</span><span class="sxs-lookup"><span data-stu-id="152ed-107">Analytics is hello powerful search tool of Azure Application Insights.</span></span>

## <a name="limits"></a><span data-ttu-id="152ed-108">Omezení</span><span class="sxs-lookup"><span data-stu-id="152ed-108">Limits</span></span>
* <span data-ttu-id="152ed-109">V současné době výsledky dotazu jsou omezené toojust přes týden posledních dat.</span><span class="sxs-lookup"><span data-stu-id="152ed-109">At present, query results are limited toojust over a week of past data.</span></span>
* <span data-ttu-id="152ed-110">Prohlížeče jsme test na: nejnovější vydání systému Chrome, okraj a prohlížeče Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="152ed-110">Browsers we test on: latest editions of Chrome, Edge, and Internet Explorer.</span></span>

## <a name="known-incompatible-browser-extensions"></a><span data-ttu-id="152ed-111">Rozšíření známé kompatibilní prohlížeče</span><span class="sxs-lookup"><span data-stu-id="152ed-111">Known incompatible browser extensions</span></span>
* <span data-ttu-id="152ed-112">Ghostery</span><span class="sxs-lookup"><span data-stu-id="152ed-112">Ghostery</span></span>

<span data-ttu-id="152ed-113">Zakázat rozšíření hello nebo použijte jiný prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="152ed-113">Disable hello extension or use a different browser.</span></span>

## <span data-ttu-id="152ed-114"><a name="e-a"></a>"Neočekávané error.</span><span class="sxs-lookup"><span data-stu-id="152ed-114"><a name="e-a"></a> "Unexpected error"</span></span>
![Došlo k neočekávané chybě obrazovky](./media/app-insights-analytics-troubleshooting/010.png)

<span data-ttu-id="152ed-116">Běhovém portálu – neošetřené výjimky došlo k vnitřní chybě.</span><span class="sxs-lookup"><span data-stu-id="152ed-116">Internal error occurred during portal runtime – unhandled exception.</span></span>

* <span data-ttu-id="152ed-117">Vyčištění mezipaměti hello prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="152ed-117">Clean hello browser's cache.</span></span> 

## <span data-ttu-id="152ed-118"><a name="e-b"></a>403... Zkuste to prosím tooreload</span><span class="sxs-lookup"><span data-stu-id="152ed-118"><a name="e-b"></a>403 ... please try tooreload</span></span>
![403... Zkuste to prosím tooreload](./media/app-insights-analytics-troubleshooting/020.png)

<span data-ttu-id="152ed-120">(Během ověřování nebo během generování tokenů přístupu) došlo k chybě související s ověřování.</span><span class="sxs-lookup"><span data-stu-id="152ed-120">An authentication related error occurred (during authentication or during access token generation).</span></span> <span data-ttu-id="152ed-121">portál Hello může nijak příliš obnovit beze změny nastavení prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="152ed-121">hello portal may have no way too recover without changing browser settings.</span></span>

* <span data-ttu-id="152ed-122">Ověřte [jsou povolené soubory cookie třetích stran](#cookies) v prohlížeči hello.</span><span class="sxs-lookup"><span data-stu-id="152ed-122">Verify [third party cookies are enabled](#cookies) in hello browser.</span></span> 

## <span data-ttu-id="152ed-123"><a name="authentication"></a>403... Ověřte zóny zabezpečení</span><span class="sxs-lookup"><span data-stu-id="152ed-123"><a name="authentication"></a>403 ... verify security zone</span></span>
![403.. .verify zóny zabezpečení](./media/app-insights-analytics-troubleshooting/030.png)

<span data-ttu-id="152ed-125">(Během ověřování nebo během generování tokenů přístupu) došlo k chybě související s ověřování.</span><span class="sxs-lookup"><span data-stu-id="152ed-125">An authentication related error occurred (during authentication or during access token generation).</span></span> <span data-ttu-id="152ed-126">portál Hello může nijak příliš obnovit beze změny nastavení prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="152ed-126">hello portal may have no way too recover without changing browser settings.</span></span>

1. <span data-ttu-id="152ed-127">Ověřte [jsou povolené soubory cookie třetích stran](#cookies) v prohlížeči hello.</span><span class="sxs-lookup"><span data-stu-id="152ed-127">Verify [third party cookies are enabled](#cookies) in hello browser.</span></span> 
2. <span data-ttu-id="152ed-128">Použijete Oblíbené položky, záložku nebo portálu analýza hello tooopen uložený odkaz?</span><span class="sxs-lookup"><span data-stu-id="152ed-128">Did you use a favorite, bookmark or saved link tooopen hello Analytics portal?</span></span> <span data-ttu-id="152ed-129">Jsou jste přihlášeni s použitím různých přihlašovacích údajů, než jste použili při uložení hello odkaz?</span><span class="sxs-lookup"><span data-stu-id="152ed-129">Are you signed in with different credentials than you used when you saved hello link?</span></span>
3. <span data-ttu-id="152ed-130">Zkuste použít okno prohlížeče v privátní nebo incognito (po zavření všechny takové windows).</span><span class="sxs-lookup"><span data-stu-id="152ed-130">Try using an in-private/incognito browser window (after closing all such windows).</span></span> <span data-ttu-id="152ed-131">Tooprovide budete mít přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="152ed-131">You'll have tooprovide your credentials.</span></span> 
4. <span data-ttu-id="152ed-132">Otevře další okno (obyčejnou) prohlížeče a přejděte příliš[Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="152ed-132">Open another (ordinary) browser window and go too[Azure](https://portal.azure.com).</span></span> <span data-ttu-id="152ed-133">Odhlaste se. Potom otevřete odkaz na vaši a přihlaste se pomocí hello správné přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="152ed-133">Sign out. Then open your link and sign in with hello correct credentials.</span></span>
5. <span data-ttu-id="152ed-134">Uživatelé okraj a prohlížeče Internet Explorer můžete také získat tuto chybu při nastavení zóny důvěryhodných serverů nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="152ed-134">Edge and Internet Explorer users can also get this error when trusted zone settings are not supported.</span></span>
   
    <span data-ttu-id="152ed-135">Ověřte obě [portálu analýza](https://analytics.applicationinsights.io) a [portál Azure Active Directory](https://portal.azure.com) jsou v hello stejné zóny zabezpečení:</span><span class="sxs-lookup"><span data-stu-id="152ed-135">Verify both [Analytics portal](https://analytics.applicationinsights.io) and [Azure Active Directory portal](https://portal.azure.com) are in hello same security zone:</span></span>
   
   * <span data-ttu-id="152ed-136">V Internet Exploreru otevřete **Možnosti Internetu**, **zabezpečení**, **Důvěryhodné servery**, **lokality**:</span><span class="sxs-lookup"><span data-stu-id="152ed-136">In Internet Explorer, open **Internet Options**, **Security**, **Trusted sites**, **Sites**:</span></span>
     
     ![Dialogové okno Možnosti Internetu, přidání webu tooTrusted lokalit](./media/app-insights-analytics-troubleshooting/033.png)
     
     <span data-ttu-id="152ed-138">V seznamu webů hello, pokud některá z následující adresy URL hello zahrnuty, ujistěte se, že hello jiné jsou zahrnuty také:</span><span class="sxs-lookup"><span data-stu-id="152ed-138">In hello Websites list, if any of hello following URLs are included, make sure that hello others are included also:</span></span>
     
     <span data-ttu-id="152ed-139">https://Analytics.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="152ed-139">https://analytics.applicationinsights.io</span></span><br/>
     <span data-ttu-id="152ed-140">https://login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="152ed-140">https://login.microsoftonline.com</span></span><br/>
     <span data-ttu-id="152ed-141">https://login.windows.net</span><span class="sxs-lookup"><span data-stu-id="152ed-141">https://login.windows.net</span></span>

## <span data-ttu-id="152ed-142"><a name="e-d"></a>404 ... Prostředek se nenašel</span><span class="sxs-lookup"><span data-stu-id="152ed-142"><a name="e-d"></a>404 ... Resource not found</span></span>
![404... prostředek nebyl nalezen](./media/app-insights-analytics-troubleshooting/040.png)

<span data-ttu-id="152ed-144">Prostředek aplikace byla odstraněna z Application Insights a už není dostupný.</span><span class="sxs-lookup"><span data-stu-id="152ed-144">Application resource was deleted from Application Insights and isn’t available anymore.</span></span> <span data-ttu-id="152ed-145">To může dojít, pokud jste uložili hello URL toohello analýza stránky.</span><span class="sxs-lookup"><span data-stu-id="152ed-145">This can happen if you saved hello URL toohello Analytics page.</span></span>

## <span data-ttu-id="152ed-146"><a name="e-e"></a>403 ... Žádné oprávnění</span><span class="sxs-lookup"><span data-stu-id="152ed-146"><a name="e-e"></a>403 ... No authorization</span></span>
![403... neautorizovaných](./media/app-insights-analytics-troubleshooting/050.png)

<span data-ttu-id="152ed-148">Nemáte oprávnění tooopen tuto aplikaci v Analytics.</span><span class="sxs-lookup"><span data-stu-id="152ed-148">You don't have permission tooopen this application in Analytics.</span></span>

* <span data-ttu-id="152ed-149">Obdrželi jste od někoho jiného hello odkaz?</span><span class="sxs-lookup"><span data-stu-id="152ed-149">Did you get hello link from someone else?</span></span> <span data-ttu-id="152ed-150">Požádejte je, že jste v hello toomake [čtečky nebo přispěvatele pro tuto skupinu prostředků](app-insights-resources-roles-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="152ed-150">Ask them toomake sure you are in hello [readers or contributors for this resource group](app-insights-resources-roles-access-control.md).</span></span>
* <span data-ttu-id="152ed-151">Uložíte hello odkaz použitím různých přihlašovacích údajů?</span><span class="sxs-lookup"><span data-stu-id="152ed-151">Did you save hello link using different credentials?</span></span> <span data-ttu-id="152ed-152">Otevřete hello [portál Azure](https://portal.azure.com), odhlásit se a potom zkuste tohoto propojení znovu, poskytuje hello správné přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="152ed-152">Open hello [Azure portal](https://portal.azure.com), sign out, and then try this link again, providing hello correct credentials.</span></span>

## <span data-ttu-id="152ed-153"><a name="html-storage"></a>403 ... Úložiště HTML5</span><span class="sxs-lookup"><span data-stu-id="152ed-153"><a name="html-storage"></a>403 ... HTML5 Storage</span></span>
<span data-ttu-id="152ed-154">Náš portál používá HTML5 localStorage a sessionStorage.</span><span class="sxs-lookup"><span data-stu-id="152ed-154">Our portal uses HTML5 localStorage and sessionStorage.</span></span>

* <span data-ttu-id="152ed-155">Chrome: Nastavení ochrany osobních údajů, nastavení obsahu.</span><span class="sxs-lookup"><span data-stu-id="152ed-155">Chrome: Settings, privacy, content settings.</span></span>
* <span data-ttu-id="152ed-156">Internet Explorer: Možnosti Internetu, karta Upřesnit, zabezpečení, povolte úložiště DOM</span><span class="sxs-lookup"><span data-stu-id="152ed-156">Internet Explorer: Internet Options, Advanced tab, Security, Enable DOM Storage</span></span>

![403... Zkuste tooenable HTML5 úložiště](./media/app-insights-analytics-troubleshooting/060.png)

## <span data-ttu-id="152ed-158"><a name="e-g"></a>404 ... Odběr nebyl nalezen</span><span class="sxs-lookup"><span data-stu-id="152ed-158"><a name="e-g"></a>404 ... Subscription not found</span></span>
![404 ... Odběr nebyl nalezen](./media/app-insights-analytics-troubleshooting/070.png)

<span data-ttu-id="152ed-160">Adresa URL Hello je neplatná.</span><span class="sxs-lookup"><span data-stu-id="152ed-160">hello URL is invalid.</span></span> 

* <span data-ttu-id="152ed-161">Otevřete prostředek aplikace hello v [portál Application Insights](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="152ed-161">Open hello app resource in [Application Insights portal](https://portal.azure.com).</span></span> <span data-ttu-id="152ed-162">Potom pomocí tlačítka Analytics hello.</span><span class="sxs-lookup"><span data-stu-id="152ed-162">Then use hello Analytics button.</span></span>

## <span data-ttu-id="152ed-163"><a name="e-h"></a>404... stránka neexistuje</span><span class="sxs-lookup"><span data-stu-id="152ed-163"><a name="e-h"></a>404 ... page doesn't exist</span></span>
![404 ... Stránka neexistuje.](./media/app-insights-analytics-troubleshooting/080.png)

<span data-ttu-id="152ed-165">Adresa URL Hello je neplatná.</span><span class="sxs-lookup"><span data-stu-id="152ed-165">hello URL is invalid.</span></span>

* <span data-ttu-id="152ed-166">Otevřete prostředek aplikace hello v [portál Application Insights](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="152ed-166">Open hello app resource in [Application Insights portal](https://portal.azure.com).</span></span> <span data-ttu-id="152ed-167">Potom pomocí tlačítka Analytics hello.</span><span class="sxs-lookup"><span data-stu-id="152ed-167">Then use hello Analytics button.</span></span>

## <span data-ttu-id="152ed-168"><a name="cookies"></a>Povolit soubory cookie třetích stran</span><span class="sxs-lookup"><span data-stu-id="152ed-168"><a name="cookies"></a>Enable third-party cookies</span></span>
  <span data-ttu-id="152ed-169">V tématu [jak toodisable třetí strany soubory cookie](http://www.digitalcitizen.life/how-disable-third-party-cookies-all-major-browsers), ale Všimněte si, potřebujeme příliš**povolit** je.</span><span class="sxs-lookup"><span data-stu-id="152ed-169">See [how toodisable third party cookies](http://www.digitalcitizen.life/how-disable-third-party-cookies-all-major-browsers), but notice we need too**enable** them.</span></span>


[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

