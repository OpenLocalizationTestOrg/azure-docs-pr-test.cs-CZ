---
title: "Problém konfigurace hesla jednotné přihlašování pro aplikace bez Galerie | Microsoft Docs"
description: "Pochopení tučné osoby běžné problémy při konfiguraci hesla jednotné přihlašování pro vlastní aplikace bez galerie, které nejsou uvedené v galerii aplikací Azure AD"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 9c76b6f3495e2dd759a156fcef97b57aece8d632
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="problem-configuring-password-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="4c0ef-103">Problém konfigurace hesla jednotné přihlašování pro aplikace bez Galerie</span><span class="sxs-lookup"><span data-stu-id="4c0ef-103">Problem configuring password single sign-on for a non-gallery application</span></span>

<span data-ttu-id="4c0ef-104">Tento článek vám pomůže lépe porozumět tučné osoby běžné problémy při konfiguraci **heslo jednotné přihlašování** s aplikací bez galerie.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-104">This article help you to understand the common problems people face when configuring **Password single sign-on** with a non-gallery application.</span></span>

## <a name="how-to-capture-sign-in-fields-for-an-application"></a><span data-ttu-id="4c0ef-105">Jak zachytit pole přihlášení pro aplikaci</span><span class="sxs-lookup"><span data-stu-id="4c0ef-105">How to capture sign-in fields for an application</span></span>

<span data-ttu-id="4c0ef-106">Zachycení pole přihlášení je podporována pouze pro podporujících formát HTML přihlašovací stránky a je **není podporováno pro nestandardní přihlašovací stránky**, jako jsou ty, které používáte Flash nebo jiné technologie nepodporujícím HTML.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-106">Sign-in field capture is only supported for HTML-enabled sign-in pages and is **not supported for non-standard sign-in pages**, like those that use Flash, or other non-HTML-enabled technologies.</span></span>

<span data-ttu-id="4c0ef-107">Existují dva způsoby, můžete zaznamenávat pole přihlašování pro vaše vlastní aplikace:</span><span class="sxs-lookup"><span data-stu-id="4c0ef-107">There are two ways you can capture sign-in fields for your custom applications:</span></span>

-   <span data-ttu-id="4c0ef-108">Zachycení pole Automatické přihlášení</span><span class="sxs-lookup"><span data-stu-id="4c0ef-108">Automatic sign-in field capture</span></span>

-   <span data-ttu-id="4c0ef-109">Zachycení pole Ruční přihlášení</span><span class="sxs-lookup"><span data-stu-id="4c0ef-109">Manual sign-in field capture</span></span>

<span data-ttu-id="4c0ef-110">**Automatické přihlášení pole zachycení** funguje dobře u většiny podporujících formát HTML přihlašovací stránky, pokud používají **dobře známé DIV ID pro uživatelské jméno a heslo zadejte** pole.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-110">**Automatic sign-in field capture** works well with most HTML-enabled sign-in pages, if they use **well-known DIV IDs for the username and password input** field.</span></span> <span data-ttu-id="4c0ef-111">Tento postup funguje způsob je oškrabávání HTML na stránce Najít DIV ID, které splňují určitá kritéria a potom uložení aby metadata pro tuto aplikaci, můžete opětovným přehráním hesla k němu později.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-111">The way this works is by scraping the HTML on the page to find DIV IDs that match certain criteria and by then saving that metadata for this application so we can replay passwords to it later.</span></span>

<span data-ttu-id="4c0ef-112">**Ruční pole přihlášení zachycení** lze použít v případě, který aplikace **dodavatele neoznačí** vstupních polí použitých pro přihlášení.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-112">**Manual sign-in field capture** can be used in the case that the application **vendor does not label** the input fields used for sign in.</span></span> <span data-ttu-id="4c0ef-113">Zachycení ruční přihlášení pole lze také v případě, že při **dodavatele vykreslí více polí** který nemůže být automaticky nalezeny.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-113">Manual sign-in field capture can also be used in the case when the **vendor renders multiple fields** which cannot be auto-detected.</span></span> <span data-ttu-id="4c0ef-114">Azure AD můžete ukládat data pro libovolný počet polí na přihlašovací stránce, tak dlouho, dokud Řekněte nám kde tato pole jsou na stránce.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-114">Azure AD can store data for as many fields as are on the sign in page, so long as you tell us where those fields are on the page.</span></span>

<span data-ttu-id="4c0ef-115">Obecně platí **Pokud pole Automatické přihlášení zachycení nefunguje, vždy doporučujeme při ruční.**</span><span class="sxs-lookup"><span data-stu-id="4c0ef-115">In general, **if automatic sign-in field capture does not work, we always suggest trying the manual option.**</span></span>

### <a name="how-to-automatically-capture-sign-in-fields-for-an-application"></a><span data-ttu-id="4c0ef-116">Jak automaticky zachytit pole přihlášení pro aplikaci</span><span class="sxs-lookup"><span data-stu-id="4c0ef-116">How to automatically capture sign-in fields for an application</span></span>

<span data-ttu-id="4c0ef-117">Ke konfiguraci **založené na heslech jednotné přihlašování** pro aplikace pomocí **pole Automatické přihlášení zachycení**, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="4c0ef-117">To configure **Password-based Single Sign-on** for an application using **automatic sign-in field capture**, follow the steps below:</span></span>

1.  <span data-ttu-id="4c0ef-118">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="4c0ef-118">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="4c0ef-119">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-119">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4c0ef-120">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-120">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4c0ef-121">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-121">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="4c0ef-122">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-122">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="4c0ef-123">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="4c0ef-123">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="4c0ef-124">Vyberte aplikaci, kterou chcete konfigurovat jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-124">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="4c0ef-125">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-125">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="4c0ef-126">Vyberte režim **založené na heslech přihlášení.**</span><span class="sxs-lookup"><span data-stu-id="4c0ef-126">Select the mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="4c0ef-127">Zadejte **přihlašovací adresa URL**.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-127">Enter the **Sign-on URL**.</span></span> <span data-ttu-id="4c0ef-128">Toto je adresa URL, kde uživatelé zadat svoje uživatelské jméno a heslo pro přihlášení k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-128">This is the URL where users enter their username and password to sign in to.</span></span> <span data-ttu-id="4c0ef-129">**Ujistěte se, přihlášení pole jsou viditelné na adrese URL je zadat**.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-129">**Ensure the sign in fields are visible at the URL you provide**.</span></span>

10. <span data-ttu-id="4c0ef-130">Klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-130">Click the **Save** button.</span></span>

11. <span data-ttu-id="4c0ef-131">Jakmile to uděláte, jsme budete automaticky scrape tuto adresu URL pro uživatelské jméno a heslo vstupní pole a vám umožní používat Azure AD bezpečně přenést hesla k dané aplikaci pomocí rozšíření přístup k panelu prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-131">Once you do that, we’ll automatically scrape that URL for a username and password input box and allow you to use Azure AD to securely transmit passwords to that application using the access panel browser extension.</span></span>

## <a name="how-to-manually-capture-sign-in-fields-for-an-application"></a><span data-ttu-id="4c0ef-132">Jak ručně zachytit pole přihlášení pro aplikaci</span><span class="sxs-lookup"><span data-stu-id="4c0ef-132">How to manually capture sign-in fields for an application</span></span>

<span data-ttu-id="4c0ef-133">Pokud chcete zaznamenat ručně přihlášení pole, nejprve musí mít rozšíření přístup k panelu prohlížeče, nainstalovat a **nebyla spuštěna v režimu se službou inPrivate, incognito nebo soukromé.**</span><span class="sxs-lookup"><span data-stu-id="4c0ef-133">To manually capture sign in fields, you must first have the Access Panel Browser extension installed and **not be running in inPrivate, incognito, or private mode.**</span></span> <span data-ttu-id="4c0ef-134">K instalaci rozšíření prohlížeče, postupujte podle kroků v [postup instalace rozšíření přístup k panelu prohlížeče](#i-cannot-manually-detect-sign-in-fields-for-my-application) části.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-134">To install the browser extension, follow the steps in the [How to install the Access Panel Browser extension](#i-cannot-manually-detect-sign-in-fields-for-my-application) section.</span></span>

<span data-ttu-id="4c0ef-135">Ke konfiguraci **založené na heslech jednotné přihlašování** pro aplikace pomocí **ruční pole přihlášení zachycení**, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="4c0ef-135">To configure **Password-based Single Sign-on** for an application using **manual sign-in field capture**, follow the steps below:</span></span>

1.  <span data-ttu-id="4c0ef-136">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="4c0ef-136">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="4c0ef-137">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-137">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4c0ef-138">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-138">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4c0ef-139">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-139">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="4c0ef-140">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-140">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="4c0ef-141">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="4c0ef-141">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="4c0ef-142">Vyberte aplikaci, kterou chcete konfigurovat jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-142">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="4c0ef-143">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-143">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="4c0ef-144">Vyberte režim **založené na heslech přihlášení.**</span><span class="sxs-lookup"><span data-stu-id="4c0ef-144">Select the mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="4c0ef-145">Zadejte **přihlašovací adresa URL**.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-145">Enter the **Sign-on URL**.</span></span> <span data-ttu-id="4c0ef-146">Toto je adresa URL, kde uživatelé zadat svoje uživatelské jméno a heslo pro přihlášení k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-146">This is the URL where users enter their username and password to sign in to.</span></span> <span data-ttu-id="4c0ef-147">**Ujistěte se, přihlášení pole jsou viditelné na adrese URL je zadat**.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-147">**Ensure the sign in fields are visible at the URL you provide**.</span></span>

10. <span data-ttu-id="4c0ef-148">Klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-148">Click the **Save** button.</span></span>

11. <span data-ttu-id="4c0ef-149">Jakmile to uděláte, jsme budete automaticky scrape tuto adresu URL pro uživatelské jméno a heslo vstupní pole a vám umožní používat Azure AD bezpečně přenést hesla k dané aplikaci pomocí rozšíření přístup k panelu prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-149">Once you do that, we’ll automatically scrape that URL for a username and password input box and allow you to use Azure AD to securely transmit passwords to that application using the access panel browser extension.</span></span> <span data-ttu-id="4c0ef-150">V případě, že tato možnost selže, můžete **změnit režim přihlášení k ruční pole přihlášení zaznamenáte** podle pokračovat v kroku 12.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-150">In the case that this fails, you can **change the sign-in mode to use manual sign-in field capture** by continuing to step 12.</span></span>

12. <span data-ttu-id="4c0ef-151">Klikněte na tlačítko **konfigurace &lt;appname&gt; nastavení hesla jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-151">click **Configure &lt;appname&gt; Password Single Sign-on Settings**.</span></span>

13. <span data-ttu-id="4c0ef-152">Vyberte **ručně zjistit přihlášení pole** možnosti konfigurace.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-152">Select the **Manually detect sign-in fields** configuration option.</span></span>

14. <span data-ttu-id="4c0ef-153">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-153">Click **Ok**.</span></span>

15. <span data-ttu-id="4c0ef-154">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-154">Click **Save**.</span></span>

16. <span data-ttu-id="4c0ef-155">Postupujte podle pokynů na obrazovce a použít na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-155">Follow the on screen instructions to use the Access Panel.</span></span>

## <a name="i-see-a-we-couldnt-find-any-sign-in-fields-at-that-url-error"></a><span data-ttu-id="4c0ef-156">Zobrazuje chybu "Jsme nenašli žádná pole přihlášení na této adrese URL."</span><span class="sxs-lookup"><span data-stu-id="4c0ef-156">I see a “We couldn’t find any sign-in fields at that URL” error</span></span>

<span data-ttu-id="4c0ef-157">Tato chyba zobrazí při automatické zjišťování pole přihlášení selže.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-157">You see this error when automatic detection of sign-in fields fails.</span></span> <span data-ttu-id="4c0ef-158">Chcete-li vyřešit tento problém, zkuste detekce pole Ruční přihlášení pomocí následujících kroků v [ručně zaznamenání pole přihlášení pro aplikaci](#how-to-manually-capture-sign-in-fields-for-an-application) části.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-158">To resolve this issue, try manual sign-in field detection by following the steps in the [How to manually capture sign-in fields for an application](#how-to-manually-capture-sign-in-fields-for-an-application) section.</span></span>

## <a name="i-see-an-unable-to-save-single-sign-on-configuration-error"></a><span data-ttu-id="4c0ef-159">Najdete v části "Nelze pro uložení konfigurace jednotného přihlašování" Chyba</span><span class="sxs-lookup"><span data-stu-id="4c0ef-159">I see an “Unable to save Single Sign-on configuration” error</span></span>

<span data-ttu-id="4c0ef-160">V některých výjimečných případech aktualizuje se konfigurace přihlášení může selhat.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-160">In certain rare cases, updating the single sign-on configuration can fail.</span></span> <span data-ttu-id="4c0ef-161">Chcete-li vyřešit, vyzkoušejte ukládání jedné přihlašování konfiguraci znovu.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-161">TO resolve this try saving the single sign-on configuration again.</span></span>

<span data-ttu-id="4c0ef-162">Pokud tento postup se opakuje selhání konzistentně, otevřete případu podpory a poskytnout informace shromážděné ve [postup najdete v části Podrobnosti o portálu oznámení](#i-cannot-manually-detect-sign-in-fields-for-my-application) a [jak získat nápovědu zasláním oznámení podrobnosti pracovníkovi podpory](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) oddíly.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-162">If this continues to fail consistently, open a support case and provide the information gathered in the [How to see the details of a portal notification](#i-cannot-manually-detect-sign-in-fields-for-my-application) and [How to get help by sending notification details to a support engineer](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) sections.</span></span>

## <a name="i-cannot-manually-detect-sign-in-fields-for-my-application"></a><span data-ttu-id="4c0ef-163">I nelze detekovat ručně přihlašovací v polích pro Moje aplikace</span><span class="sxs-lookup"><span data-stu-id="4c0ef-163">I cannot manually detect sign in fields for my application</span></span>

<span data-ttu-id="4c0ef-164">Některá chování, které se můžete setkat při nepracuje ruční detekce patří:</span><span class="sxs-lookup"><span data-stu-id="4c0ef-164">Some of the behaviors you might see when manual detection is not working include:</span></span>

-   <span data-ttu-id="4c0ef-165">Proces ručního zachycení zobrazovaly pracovat, ale pole zachytit nebyla správná</span><span class="sxs-lookup"><span data-stu-id="4c0ef-165">The manual capture process appeared to work, but the fields captured were not correct</span></span>

-   <span data-ttu-id="4c0ef-166">Pole pravé nemáte získat zvýrazněná při provádění proces zachycení</span><span class="sxs-lookup"><span data-stu-id="4c0ef-166">The right fields don’t get highlighted when performing the capture process</span></span>

-   <span data-ttu-id="4c0ef-167">Proces zachycení trvá mi na přihlašovací stránku aplikace podle očekávání, ale nic nestane</span><span class="sxs-lookup"><span data-stu-id="4c0ef-167">The capture process takes me to the application’s login page as expected, but nothing happens</span></span>

-   <span data-ttu-id="4c0ef-168">Ruční zachycení pravděpodobně fungovat, ale jednotného přihlašování nemá dojít v případě, že moje uživatelé přejdou do aplikace na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-168">Manual capture appears to work, but SSO doesn’t happen when my users navigate to the application from the Access Panel.</span></span>

<span data-ttu-id="4c0ef-169">Pokud narazíte na některý z těchto problémů zkontrolujte následující:</span><span class="sxs-lookup"><span data-stu-id="4c0ef-169">check the following if you encounter any of these issues:</span></span>

-   <span data-ttu-id="4c0ef-170">Zajistěte, abyste měli nejnovější verzi rozšíření přístup k panelu prohlížeče **nainstalován** a **povoleno** podle kroků v [postup instalace rozšíření přístup k panelu prohlížeče](#how-to-install-the-access-panel-browser-extension) části.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-170">Ensure that you have the latest version of the access panel browser extension **installed** and **enabled** by following the steps in the [How to install the Access Panel Browser extension](#how-to-install-the-access-panel-browser-extension) section.</span></span>

-   <span data-ttu-id="4c0ef-171">Ujistěte se, že proces zachycení neprovádíte při prohlížeč v **privátní, se službou inPrivate nebo incognito režimu**.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-171">Ensure that you are not attempting the capture process while your browser in **incognito, inPrivate, or Private mode**.</span></span> <span data-ttu-id="4c0ef-172">Rozšíření přístup k panelu v těchto režimech nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-172">The access panel extension is not supported in these modes.</span></span>

-   <span data-ttu-id="4c0ef-173">Ujistěte se, že uživatelé nejsou pokouší přihlásit k aplikaci na přístupovém panelu při v **privátní, se službou inPrivate nebo incognito režimu**.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-173">Ensure that your users are not trying to sign in to the application from the access panel while in **incognito, inPrivate, or Private mode**.</span></span> <span data-ttu-id="4c0ef-174">Rozšíření přístup k panelu v těchto režimech nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-174">The access panel extension is not supported in these modes.</span></span>

-   <span data-ttu-id="4c0ef-175">Zkuste ruční proces zachycení znovu, zajištění, že red značky jsou přes správná pole.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-175">Try the manual capture process again, ensuring the red markers are over the correct fields.</span></span>

-   <span data-ttu-id="4c0ef-176">Pokud proces ruční zachycení zdá se, že zablokuje nebo stránku pro přihlášení neprovádí proces ruční zachycení nic (případě 3 výše), opakujte akci.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-176">If the manual capture process seems to hang, or the sign in page doesn’t do anything (case 3 above), try the manual capture process again.</span></span> <span data-ttu-id="4c0ef-177">Ale tentokrát po dokončení procesu, stiskněte **F12** tlačítko otevřete konzolu pro vývojáře v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-177">But, this time after completing the process, press the **F12** button to open your browser’s developer console.</span></span> <span data-ttu-id="4c0ef-178">Jednou existuje, otevřete **konzoly** a typ **window.location= "&lt;zadejte přihlašovací v adrese url, které jste zadali při konfiguraci aplikace&gt;"** a potom stiskněte klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-178">Once there, open the **console** and type **window.location=”&lt;enter the sign in url you specified when configuring the app&gt;”** and then press **Enter**.</span></span> <span data-ttu-id="4c0ef-179">Tato síla stránka přesměrování, které končí proces zachycení a uložení pole, která byla zachycena.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-179">This force a page redirect which end the capture process and store the fields that have been captured.</span></span>

<span data-ttu-id="4c0ef-180">Pokud žádný z těchto postupů fungovat pro vás, pomůžeme vám.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-180">If none of these approaches work for you, we can help.</span></span> <span data-ttu-id="4c0ef-181">Otevření případu podpory podrobnosti co jste se pokusili, a také informace shromážděné ve [postup najdete v části Podrobnosti o portálu oznámení](#i-cannot-manually-detect-sign-in-fields-for-my-application) a [jak získat nápovědu zasláním oznámení podrobnosti pracovníkovi podpory](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) částech (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="4c0ef-181">Open a support case with the details of what you tried, as well as the information gathered in the [How to see the details of a portal notification](#i-cannot-manually-detect-sign-in-fields-for-my-application) and [How to get help by sending notification details to a support engineer](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) sections (if applicable).</span></span>

## <a name="how-to-install-the-access-panel-browser-extension"></a><span data-ttu-id="4c0ef-182">Postup instalace rozšíření přístup k panelu prohlížeče</span><span class="sxs-lookup"><span data-stu-id="4c0ef-182">How to install the Access Panel Browser extension</span></span>

<span data-ttu-id="4c0ef-183">Chcete-li nainstalovat rozšíření přístup k panelu prohlížeče, postupujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="4c0ef-183">To install the Access Panel Browser extension, follow the steps below:</span></span>

1.  <span data-ttu-id="4c0ef-184">Otevřete [přístupový Panel](https://myapps.microsoft.com) v jednom z podporovaných prohlížečích a přihlaste se jako **uživatele** ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-184">Open the [Access Panel](https://myapps.microsoft.com) in one of the supported browsers and sign in as a **user** in your Azure AD.</span></span>

2.  <span data-ttu-id="4c0ef-185">Klikněte na tlačítko **SSO heslo aplikace** na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-185">click a **password-SSO application** in the Access Panel.</span></span>

3.  <span data-ttu-id="4c0ef-186">V řádku, požádá o instalaci softwaru, vyberte **instalovat nyní**.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-186">In the prompt asking to install the software, select **Install Now**.</span></span>

4.  <span data-ttu-id="4c0ef-187">Založené na prohlížeči budete přesměrováni na odkaz ke stažení.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-187">Based on your browser you be directed to the download link.</span></span> <span data-ttu-id="4c0ef-188">**Přidat** rozšíření do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-188">**Add** the extension to your browser.</span></span>

5.  <span data-ttu-id="4c0ef-189">Pokud prohlížeč zobrazí dotaz, vyberte buď **povolit** nebo **povolit** rozšíření.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-189">If your browser asks, select to either **Enable** or **Allow** the extension.</span></span>

6.  <span data-ttu-id="4c0ef-190">Po instalaci **restartujte** relaci prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-190">Once installed, **restart** your browser session.</span></span>

7.  <span data-ttu-id="4c0ef-191">Přihlaste se do přístupového panelu a zobrazit, pokud můžete **spusťte** SSO hesla aplikací.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-191">Sign in into the Access Panel and see if you can **launch** your password-SSO applications.</span></span>

<span data-ttu-id="4c0ef-192">Rozšíření pro Chrome a Firefox může také stáhnout z přímé odkazy níže:</span><span class="sxs-lookup"><span data-stu-id="4c0ef-192">You may also download the extension for Chrome and Firefox from the direct links below:</span></span>

-   [<span data-ttu-id="4c0ef-193">Rozšíření pro Chrome přístup panely</span><span class="sxs-lookup"><span data-stu-id="4c0ef-193">Chrome Access Panel Extension</span></span>](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [<span data-ttu-id="4c0ef-194">Rozšíření panelu Firefox přístup</span><span class="sxs-lookup"><span data-stu-id="4c0ef-194">Firefox Access Panel Extension</span></span>](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="how-to-see-the-details-of-a-portal-notification"></a><span data-ttu-id="4c0ef-195">Postup najdete v části Podrobnosti o portálu oznámení</span><span class="sxs-lookup"><span data-stu-id="4c0ef-195">How to see the details of a portal notification</span></span>

<span data-ttu-id="4c0ef-196">Pomocí následujícího postupu můžete zobrazit podrobnosti o portálu oznámení:</span><span class="sxs-lookup"><span data-stu-id="4c0ef-196">You can see the details of any portal notification by following the steps below:</span></span>

1.  <span data-ttu-id="4c0ef-197">klikněte **oznámení** ikonu (zvonku) v pravém horním rohu stránky na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="4c0ef-197">click the **Notifications** icon (the bell) in the upper right of the Azure Portal</span></span>

2.  <span data-ttu-id="4c0ef-198">Vyberte všechna oznámení v **chyba** stavu (ty se zobrazí červený (!) vedle je).</span><span class="sxs-lookup"><span data-stu-id="4c0ef-198">Select any notification in an **Error** state (those with a red (!) next to them).</span></span>

  ><span data-ttu-id="4c0ef-199">! Všimněte si] je nelze kliknout oznámení v **úspěšné** nebo **probíhá** stavu.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-199">!NOTE] You cannot click notifications in a **Successful** or **In Progress** state.</span></span>
  >
  >

3.  <span data-ttu-id="4c0ef-200">Tento otevřený **podrobnosti oznámení** okno.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-200">This open the **Notification Details** blade.</span></span>

4.  <span data-ttu-id="4c0ef-201">Tyto informace použít sami, abyste porozuměli další podrobnosti o problému.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-201">Use this information yourself to understand more details about the problem.</span></span>

5.  <span data-ttu-id="4c0ef-202">Pokud stále potřebujete pomoc, můžete také sdílet tyto informace se pracovníka podpory nebo product group získat pomoc s váš problém.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-202">If you still need help, you can also share this information with a support engineer or the product group to get help with your problem.</span></span>

6.  <span data-ttu-id="4c0ef-203">Klikněte na tlačítko **kopie** **ikonu** napravo od **Chyba kopírování** textbox zkopírovat všechny podrobnosti oznámení sdílet s skupiny pracovník podpory nebo produktu.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-203">Click the **copy** **icon** to the right of the **Copy error** textbox to copy all the notification details to share with a support or product group engineer.</span></span>

## <a name="how-to-get-help-by-sending-notification-details-to-a-support-engineer"></a><span data-ttu-id="4c0ef-204">Získání nápovědy poslat podrobnosti oznámení na pracovníka podpory</span><span class="sxs-lookup"><span data-stu-id="4c0ef-204">How to get help by sending notification details to a support engineer</span></span>

<span data-ttu-id="4c0ef-205">To je velmi důležité, že můžete sdílet **všechny níže uvedené podrobnosti** s pracovníka podpory, pokud potřebujete pomoc, tak, aby se vám může pomoct rychle.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-205">It is very important that you share **all the details listed below** with a support engineer if you need help, so that they can help you quickly.</span></span> <span data-ttu-id="4c0ef-206">Můžete to provést pomocí snadno **uděláte snímek,** nebo kliknutím **ikona chyby kopie**, který se nachází vpravo od **Chyba kopírování** textové pole.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-206">You can do this easily by **taking a screenshot,** or by clicking the **Copy error icon**, found to the right of the **Copy error** textbox.</span></span>

## <a name="notification-details-explained"></a><span data-ttu-id="4c0ef-207">Vysvětlení podrobnosti oznámení</span><span class="sxs-lookup"><span data-stu-id="4c0ef-207">Notification Details Explained</span></span>

<span data-ttu-id="4c0ef-208">Níže se dozvíte více co každý oznámení položky znamená a jsou uvedeny příklady každý z nich.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-208">The below explains more what each of the notification items means, and gives examples of each of them.</span></span>

### <a name="essential-notification-items"></a><span data-ttu-id="4c0ef-209">Základní oznámení položky</span><span class="sxs-lookup"><span data-stu-id="4c0ef-209">Essential Notification Items</span></span>

-   <span data-ttu-id="4c0ef-210">**Název** – popisný název oznámení.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-210">**Title** – the descriptive title of the notification</span></span>

    -   <span data-ttu-id="4c0ef-211">Příklad – **nastavení proxy aplikace**</span><span class="sxs-lookup"><span data-stu-id="4c0ef-211">Example – **Application proxy settings**</span></span>

-   <span data-ttu-id="4c0ef-212">**Popis** – popis co došlo k chybě v důsledku operaci</span><span class="sxs-lookup"><span data-stu-id="4c0ef-212">**Description** – the description of what occurred as a result of the operation</span></span>

    -   <span data-ttu-id="4c0ef-213">Příklad – **zadaná interní adresa url je již používán jinou aplikací**</span><span class="sxs-lookup"><span data-stu-id="4c0ef-213">Example – **Internal url entered is already being used by another application**</span></span>

-   <span data-ttu-id="4c0ef-214">**Id oznámení** – jedinečné id oznámení.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-214">**Notification Id** – the unique id of the notification</span></span>

    -   <span data-ttu-id="4c0ef-215">Příklad – **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**</span><span class="sxs-lookup"><span data-stu-id="4c0ef-215">Example – **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**</span></span>

-   <span data-ttu-id="4c0ef-216">**Id žádosti klienta** – id konkrétní žádosti provedené prohlížeč</span><span class="sxs-lookup"><span data-stu-id="4c0ef-216">**Client Request Id** – the specific request id made by your browser</span></span>

    -   <span data-ttu-id="4c0ef-217">Příklad – **302fd775-3329-4670-a9f3-bea37004f0bc**</span><span class="sxs-lookup"><span data-stu-id="4c0ef-217">Example – **302fd775-3329-4670-a9f3-bea37004f0bc**</span></span>

-   <span data-ttu-id="4c0ef-218">**Čas UTC razítko** – časové razítko, během které oznámení došlo k chybě, v UTC</span><span class="sxs-lookup"><span data-stu-id="4c0ef-218">**Time Stamp UTC** – the timestamp during which the notification occurred, in UTC</span></span>

    -   <span data-ttu-id="4c0ef-219">Příklad – **2017-03-23T19:50:43.7583681Z**</span><span class="sxs-lookup"><span data-stu-id="4c0ef-219">Example – **2017-03-23T19:50:43.7583681Z**</span></span>

-   <span data-ttu-id="4c0ef-220">**Interní Id transakce** – interní ID můžeme použít k vyhledání Chyba v našem systému</span><span class="sxs-lookup"><span data-stu-id="4c0ef-220">**Internal Transaction Id** – the internal ID we can use to look the error up in our systems</span></span>

    -   <span data-ttu-id="4c0ef-221">Příklad – **71a2f329-ca29-402f-aa72-bc00a7aca603**</span><span class="sxs-lookup"><span data-stu-id="4c0ef-221">Example – **71a2f329-ca29-402f-aa72-bc00a7aca603**</span></span>

-   <span data-ttu-id="4c0ef-222">**Hlavní název uživatele** – uživatel, který provedl operaci</span><span class="sxs-lookup"><span data-stu-id="4c0ef-222">**UPN** – the user who performed the operation</span></span>

    -   <span data-ttu-id="4c0ef-223">Příklad –**tperkins@f128.info**</span><span class="sxs-lookup"><span data-stu-id="4c0ef-223">Example – **tperkins@f128.info**</span></span>

-   <span data-ttu-id="4c0ef-224">**Id klienta** – jedinečné ID klienta, který uživatel, který provedl operaci byl uživatel členem</span><span class="sxs-lookup"><span data-stu-id="4c0ef-224">**Tenant Id** – the unique ID of the tenant that the user who performed the operation was a member of</span></span>

    -   <span data-ttu-id="4c0ef-225">Příklad – **7918d4b5-0442-4a97-be2d-36f9f9962ece**</span><span class="sxs-lookup"><span data-stu-id="4c0ef-225">Example – **7918d4b5-0442-4a97-be2d-36f9f9962ece**</span></span>

-   <span data-ttu-id="4c0ef-226">**Objekt uživatele Id** – jedinečné ID uživatele, který provádí operaci</span><span class="sxs-lookup"><span data-stu-id="4c0ef-226">**User object Id** – the unique ID of the user who performed the operation</span></span>

    -   <span data-ttu-id="4c0ef-227">Příklad – **17f84be4-51f8-483a-b533-383791227a99**</span><span class="sxs-lookup"><span data-stu-id="4c0ef-227">Example – **17f84be4-51f8-483a-b533-383791227a99**</span></span>

### <a name="detailed-notification-items"></a><span data-ttu-id="4c0ef-228">Podrobné oznámení položky</span><span class="sxs-lookup"><span data-stu-id="4c0ef-228">Detailed Notification Items</span></span>

-   <span data-ttu-id="4c0ef-229">**Zobrazovaný název** – **(nesmí být prázdné)** podrobnější zobrazovaný název chyby</span><span class="sxs-lookup"><span data-stu-id="4c0ef-229">**Display Name** – **(can be empty)** a more detailed display name for the error</span></span>

    -   <span data-ttu-id="4c0ef-230">Příklad * – **nastavení proxy aplikace**</span><span class="sxs-lookup"><span data-stu-id="4c0ef-230">Example *– **Application proxy settings**</span></span>

-   <span data-ttu-id="4c0ef-231">**Stav** – konkrétní stav oznámení.</span><span class="sxs-lookup"><span data-stu-id="4c0ef-231">**Status** – the specific status of the notification</span></span>

    -   <span data-ttu-id="4c0ef-232">Příklad * – **se nezdařilo**</span><span class="sxs-lookup"><span data-stu-id="4c0ef-232">Example *– **Failed**</span></span>

-   <span data-ttu-id="4c0ef-233">**Id objektu** – **(nesmí být prázdné)** ID objektu, pro kterou byla provedena operace</span><span class="sxs-lookup"><span data-stu-id="4c0ef-233">**Object Id** – **(can be empty)** the object ID against which the operation was performed</span></span>

    -   <span data-ttu-id="4c0ef-234">Příklad – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**</span><span class="sxs-lookup"><span data-stu-id="4c0ef-234">Example – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**</span></span>

-   <span data-ttu-id="4c0ef-235">**Podrobnosti o** – podrobný popis co došlo k chybě v důsledku operaci</span><span class="sxs-lookup"><span data-stu-id="4c0ef-235">**Details** – the detailed description of what occurred as a result of the operation</span></span>

    -   <span data-ttu-id="4c0ef-236">Příklad – **interní adresa url, http://bing.com/' je neplatná, protože je již používán**</span><span class="sxs-lookup"><span data-stu-id="4c0ef-236">Example – **Internal url 'http://bing.com/' is invalid since it is already in use**</span></span>

-   <span data-ttu-id="4c0ef-237">**Chyba při kopírování** – klikněte na tlačítko **ikona kopírování** napravo od **Chyba kopírování** textbox zkopírovat všechny podrobnosti oznámení sdílet s skupiny pracovník podpory nebo produkt</span><span class="sxs-lookup"><span data-stu-id="4c0ef-237">**Copy error** – Click the **copy icon** to the right of the **Copy error** textbox to copy all the notification details to share with a support or product group engineer</span></span>

    -   <span data-ttu-id="4c0ef-238">Příklad –```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```</span><span class="sxs-lookup"><span data-stu-id="4c0ef-238">Example – ```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```</span></span>

## <a name="next-steps"></a><span data-ttu-id="4c0ef-239">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4c0ef-239">Next steps</span></span>
[<span data-ttu-id="4c0ef-240">Zadejte jednotné přihlašování pro vaše aplikace s Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="4c0ef-240">Provide single sign-on to your apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)

