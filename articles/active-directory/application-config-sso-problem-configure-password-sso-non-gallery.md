---
title: "Konfigurace hesla jednotné přihlašování pro aplikace bez Galerie aaaProblem | Microsoft Docs"
description: "Pochopení hello běžné problémy uživatelé setkávají při konfiguraci hesla jednotné přihlašování pro vlastní aplikace bez galerie, které nejsou uvedené v hello Azure AD Application Gallery"
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
ms.openlocfilehash: 3aee0a4c525bb3da338da2da0882ec572cf0e5e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="problem-configuring-password-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="2198c-103">Problém konfigurace hesla jednotné přihlašování pro aplikace bez Galerie</span><span class="sxs-lookup"><span data-stu-id="2198c-103">Problem configuring password single sign-on for a non-gallery application</span></span>

<span data-ttu-id="2198c-104">Tento článek vám pomůže toounderstand hello běžné problémy uživatelé setkávají při konfiguraci **heslo jednotné přihlašování** s aplikací bez galerie.</span><span class="sxs-lookup"><span data-stu-id="2198c-104">This article help you toounderstand hello common problems people face when configuring **Password single sign-on** with a non-gallery application.</span></span>

## <a name="how-toocapture-sign-in-fields-for-an-application"></a><span data-ttu-id="2198c-105">Jak přihlášení toocapture polí pro aplikaci</span><span class="sxs-lookup"><span data-stu-id="2198c-105">How toocapture sign-in fields for an application</span></span>

<span data-ttu-id="2198c-106">Zachycení pole přihlášení je podporována pouze pro podporujících formát HTML přihlašovací stránky a je **není podporováno pro nestandardní přihlašovací stránky**, jako jsou ty, které používáte Flash nebo jiné technologie nepodporujícím HTML.</span><span class="sxs-lookup"><span data-stu-id="2198c-106">Sign-in field capture is only supported for HTML-enabled sign-in pages and is **not supported for non-standard sign-in pages**, like those that use Flash, or other non-HTML-enabled technologies.</span></span>

<span data-ttu-id="2198c-107">Existují dva způsoby, můžete zaznamenávat pole přihlašování pro vaše vlastní aplikace:</span><span class="sxs-lookup"><span data-stu-id="2198c-107">There are two ways you can capture sign-in fields for your custom applications:</span></span>

-   <span data-ttu-id="2198c-108">Zachycení pole Automatické přihlášení</span><span class="sxs-lookup"><span data-stu-id="2198c-108">Automatic sign-in field capture</span></span>

-   <span data-ttu-id="2198c-109">Zachycení pole Ruční přihlášení</span><span class="sxs-lookup"><span data-stu-id="2198c-109">Manual sign-in field capture</span></span>

<span data-ttu-id="2198c-110">**Automatické přihlášení pole zachycení** funguje dobře u většiny podporujících formát HTML přihlašovací stránky, pokud používají **dobře známé identifikátory DIV hello uživatelské jméno a heslo zadejte** pole.</span><span class="sxs-lookup"><span data-stu-id="2198c-110">**Automatic sign-in field capture** works well with most HTML-enabled sign-in pages, if they use **well-known DIV IDs for hello username and password input** field.</span></span> <span data-ttu-id="2198c-111">Hello tak, jak to funguje je oškrabávání hello HTML na stránce toofind hello DIV ID, které splňují určitá kritéria a potom uložení aby metadata pro tuto aplikaci, tak opětovným přehráním hesla tooit později.</span><span class="sxs-lookup"><span data-stu-id="2198c-111">hello way this works is by scraping hello HTML on hello page toofind DIV IDs that match certain criteria and by then saving that metadata for this application so we can replay passwords tooit later.</span></span>

<span data-ttu-id="2198c-112">**Ruční pole přihlášení zachycení** lze použít v případě hello, který hello aplikace **dodavatele neoznačí** hello vstupních polí použitých pro přihlášení.</span><span class="sxs-lookup"><span data-stu-id="2198c-112">**Manual sign-in field capture** can be used in hello case that hello application **vendor does not label** hello input fields used for sign in.</span></span> <span data-ttu-id="2198c-113">Zachycení ruční přihlášení pole lze také v případě hello při hello **dodavatele vykreslí více polí** který nemůže být automaticky nalezeny.</span><span class="sxs-lookup"><span data-stu-id="2198c-113">Manual sign-in field capture can also be used in hello case when hello **vendor renders multiple fields** which cannot be auto-detected.</span></span> <span data-ttu-id="2198c-114">Azure AD můžete ukládat data pro jako počet polí na hello přihlašovací stránky, tak dlouho, dokud Řekněte nám kde tato pole jsou na stránce hello.</span><span class="sxs-lookup"><span data-stu-id="2198c-114">Azure AD can store data for as many fields as are on hello sign in page, so long as you tell us where those fields are on hello page.</span></span>

<span data-ttu-id="2198c-115">Obecně platí **Pokud pole Automatické přihlášení zachycení nefunguje, doporučujeme vždy snažíme hello ruční.**</span><span class="sxs-lookup"><span data-stu-id="2198c-115">In general, **if automatic sign-in field capture does not work, we always suggest trying hello manual option.**</span></span>

### <a name="how-tooautomatically-capture-sign-in-fields-for-an-application"></a><span data-ttu-id="2198c-116">Jak zachytit tooautomatically pole přihlášení pro aplikaci</span><span class="sxs-lookup"><span data-stu-id="2198c-116">How tooautomatically capture sign-in fields for an application</span></span>

<span data-ttu-id="2198c-117">tooconfigure **založené na heslech jednotné přihlašování** pro aplikace pomocí **pole Automatické přihlášení zachycení**, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="2198c-117">tooconfigure **Password-based Single Sign-on** for an application using **automatic sign-in field capture**, follow hello steps below:</span></span>

1.  <span data-ttu-id="2198c-118">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="2198c-118">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="2198c-119">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="2198c-119">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="2198c-120">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="2198c-120">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="2198c-121">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="2198c-121">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="2198c-122">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="2198c-122">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="2198c-123">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="2198c-123">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="2198c-124">Vyberte hello aplikaci tooconfigure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2198c-124">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="2198c-125">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="2198c-125">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="2198c-126">Vyberte hello režimu **založené na heslech přihlášení.**</span><span class="sxs-lookup"><span data-stu-id="2198c-126">Select hello mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="2198c-127">Zadejte hello **přihlašovací adresa URL**.</span><span class="sxs-lookup"><span data-stu-id="2198c-127">Enter hello **Sign-on URL**.</span></span> <span data-ttu-id="2198c-128">Toto je hello URL, kde uživatelé zadat toosign své uživatelské jméno a heslo v k.</span><span class="sxs-lookup"><span data-stu-id="2198c-128">This is hello URL where users enter their username and password toosign in to.</span></span> <span data-ttu-id="2198c-129">**Ujistěte se, hello přihlášení pole jsou viditelné na adrese URL hello zadáte**.</span><span class="sxs-lookup"><span data-stu-id="2198c-129">**Ensure hello sign in fields are visible at hello URL you provide**.</span></span>

10. <span data-ttu-id="2198c-130">Klikněte na tlačítko hello **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2198c-130">Click hello **Save** button.</span></span>

11. <span data-ttu-id="2198c-131">Jakmile to uděláte, jsme budete automaticky scrape tuto adresu URL pro uživatelské jméno a heslo vstupní pole a umožňují toouse Azure AD toosecurely přenášet hesla toothat aplikace pomocí rozšíření prohlížeče panely hello přístup.</span><span class="sxs-lookup"><span data-stu-id="2198c-131">Once you do that, we’ll automatically scrape that URL for a username and password input box and allow you toouse Azure AD toosecurely transmit passwords toothat application using hello access panel browser extension.</span></span>

## <a name="how-toomanually-capture-sign-in-fields-for-an-application"></a><span data-ttu-id="2198c-132">Jak zachytit toomanually pole přihlášení pro aplikaci</span><span class="sxs-lookup"><span data-stu-id="2198c-132">How toomanually capture sign-in fields for an application</span></span>

<span data-ttu-id="2198c-133">zachycení přihlašovací toomanually pole, je nejdříve nutné rozšíření přístup k panelu prohlížeče hello nainstalovat a **nebyla spuštěna v režimu se službou inPrivate, incognito nebo soukromé.**</span><span class="sxs-lookup"><span data-stu-id="2198c-133">toomanually capture sign in fields, you must first have hello Access Panel Browser extension installed and **not be running in inPrivate, incognito, or private mode.**</span></span> <span data-ttu-id="2198c-134">rozšíření prohlížeče tooinstall hello, postupujte podle kroků hello v hello [jak tooinstall hello rozšíření přístup k panelu prohlížeče](#i-cannot-manually-detect-sign-in-fields-for-my-application) části.</span><span class="sxs-lookup"><span data-stu-id="2198c-134">tooinstall hello browser extension, follow hello steps in hello [How tooinstall hello Access Panel Browser extension](#i-cannot-manually-detect-sign-in-fields-for-my-application) section.</span></span>

<span data-ttu-id="2198c-135">tooconfigure **založené na heslech jednotné přihlašování** pro aplikace pomocí **ruční pole přihlášení zachycení**, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="2198c-135">tooconfigure **Password-based Single Sign-on** for an application using **manual sign-in field capture**, follow hello steps below:</span></span>

1.  <span data-ttu-id="2198c-136">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="2198c-136">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="2198c-137">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="2198c-137">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="2198c-138">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="2198c-138">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="2198c-139">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="2198c-139">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="2198c-140">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="2198c-140">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="2198c-141">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="2198c-141">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="2198c-142">Vyberte hello aplikaci tooconfigure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2198c-142">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="2198c-143">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="2198c-143">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="2198c-144">Vyberte hello režimu **založené na heslech přihlášení.**</span><span class="sxs-lookup"><span data-stu-id="2198c-144">Select hello mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="2198c-145">Zadejte hello **přihlašovací adresa URL**.</span><span class="sxs-lookup"><span data-stu-id="2198c-145">Enter hello **Sign-on URL**.</span></span> <span data-ttu-id="2198c-146">Toto je hello URL, kde uživatelé zadat toosign své uživatelské jméno a heslo v k.</span><span class="sxs-lookup"><span data-stu-id="2198c-146">This is hello URL where users enter their username and password toosign in to.</span></span> <span data-ttu-id="2198c-147">**Ujistěte se, hello přihlášení pole jsou viditelné na adrese URL hello zadáte**.</span><span class="sxs-lookup"><span data-stu-id="2198c-147">**Ensure hello sign in fields are visible at hello URL you provide**.</span></span>

10. <span data-ttu-id="2198c-148">Klikněte na tlačítko hello **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2198c-148">Click hello **Save** button.</span></span>

11. <span data-ttu-id="2198c-149">Jakmile to uděláte, jsme budete automaticky scrape tuto adresu URL pro uživatelské jméno a heslo vstupní pole a umožňují toouse Azure AD toosecurely přenášet hesla toothat aplikace pomocí rozšíření prohlížeče panely hello přístup.</span><span class="sxs-lookup"><span data-stu-id="2198c-149">Once you do that, we’ll automatically scrape that URL for a username and password input box and allow you toouse Azure AD toosecurely transmit passwords toothat application using hello access panel browser extension.</span></span> <span data-ttu-id="2198c-150">V případě hello, který tato možnost selže, můžete **změnit hello přihlášení režimu toouse ruční pole přihlášení zachycení** pokračováním toostep 12.</span><span class="sxs-lookup"><span data-stu-id="2198c-150">In hello case that this fails, you can **change hello sign-in mode toouse manual sign-in field capture** by continuing toostep 12.</span></span>

12. <span data-ttu-id="2198c-151">Klikněte na tlačítko **konfigurace &lt;appname&gt; nastavení hesla jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="2198c-151">click **Configure &lt;appname&gt; Password Single Sign-on Settings**.</span></span>

13. <span data-ttu-id="2198c-152">Vyberte hello **ručně zjistit přihlášení pole** možnosti konfigurace.</span><span class="sxs-lookup"><span data-stu-id="2198c-152">Select hello **Manually detect sign-in fields** configuration option.</span></span>

14. <span data-ttu-id="2198c-153">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="2198c-153">Click **Ok**.</span></span>

15. <span data-ttu-id="2198c-154">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="2198c-154">Click **Save**.</span></span>

16. <span data-ttu-id="2198c-155">Postupujte podle hello na obrazovce pokyny toouse hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="2198c-155">Follow hello on screen instructions toouse hello Access Panel.</span></span>

## <a name="i-see-a-we-couldnt-find-any-sign-in-fields-at-that-url-error"></a><span data-ttu-id="2198c-156">Zobrazuje chybu "Jsme nenašli žádná pole přihlášení na této adrese URL."</span><span class="sxs-lookup"><span data-stu-id="2198c-156">I see a “We couldn’t find any sign-in fields at that URL” error</span></span>

<span data-ttu-id="2198c-157">Tato chyba zobrazí při automatické zjišťování pole přihlášení selže.</span><span class="sxs-lookup"><span data-stu-id="2198c-157">You see this error when automatic detection of sign-in fields fails.</span></span> <span data-ttu-id="2198c-158">Tento problém, zkuste detekce pole Ruční přihlášení pomocí následujících hello kroky hello tooresolve [jak toomanually zaznamenat pole přihlášení pro aplikaci](#how-to-manually-capture-sign-in-fields-for-an-application) části.</span><span class="sxs-lookup"><span data-stu-id="2198c-158">tooresolve this issue, try manual sign-in field detection by following hello steps in hello [How toomanually capture sign-in fields for an application](#how-to-manually-capture-sign-in-fields-for-an-application) section.</span></span>

## <a name="i-see-an-unable-toosave-single-sign-on-configuration-error"></a><span data-ttu-id="2198c-159">Zobrazuje chybu "nelze toosave jednotné přihlašování konfigurace"</span><span class="sxs-lookup"><span data-stu-id="2198c-159">I see an “Unable toosave Single Sign-on configuration” error</span></span>

<span data-ttu-id="2198c-160">V některých výjimečných případech může selhat aktualizace hello jeden přihlašování konfigurace.</span><span class="sxs-lookup"><span data-stu-id="2198c-160">In certain rare cases, updating hello single sign-on configuration can fail.</span></span> <span data-ttu-id="2198c-161">tooresolve, vyzkoušejte ukládání hello jeden přihlašování konfiguraci znovu.</span><span class="sxs-lookup"><span data-stu-id="2198c-161">tooresolve this try saving hello single sign-on configuration again.</span></span>

<span data-ttu-id="2198c-162">Pokud tento postup se opakuje toofail konzistentně, otevřete případu podpory a zadejte hello informace získané v hello [jak toosee hello podrobnosti o portálu oznámení](#i-cannot-manually-detect-sign-in-fields-for-my-application) a [jak pomoci tooget zasláním oznámení podrobnosti tooa pracovník odborné pomoci](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) oddíly.</span><span class="sxs-lookup"><span data-stu-id="2198c-162">If this continues toofail consistently, open a support case and provide hello information gathered in hello [How toosee hello details of a portal notification](#i-cannot-manually-detect-sign-in-fields-for-my-application) and [How tooget help by sending notification details tooa support engineer](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) sections.</span></span>

## <a name="i-cannot-manually-detect-sign-in-fields-for-my-application"></a><span data-ttu-id="2198c-163">I nelze detekovat ručně přihlašovací v polích pro Moje aplikace</span><span class="sxs-lookup"><span data-stu-id="2198c-163">I cannot manually detect sign in fields for my application</span></span>

<span data-ttu-id="2198c-164">Mezi hello chování, mohou být zobrazeny při nepracuje ruční detekce patří:</span><span class="sxs-lookup"><span data-stu-id="2198c-164">Some of hello behaviors you might see when manual detection is not working include:</span></span>

-   <span data-ttu-id="2198c-165">Proces ručního zachycení Hello zobrazovaly toowork, ale pole hello zachytit nebyla správná</span><span class="sxs-lookup"><span data-stu-id="2198c-165">hello manual capture process appeared toowork, but hello fields captured were not correct</span></span>

-   <span data-ttu-id="2198c-166">pole pravé Hello nemáte získat zvýrazněná při provádění proces zachycení hello</span><span class="sxs-lookup"><span data-stu-id="2198c-166">hello right fields don’t get highlighted when performing hello capture process</span></span>

-   <span data-ttu-id="2198c-167">Proces zachycení Hello trvá mi toohello na přihlašovací stránku aplikace podle očekávání, ale nic nestane</span><span class="sxs-lookup"><span data-stu-id="2198c-167">hello capture process takes me toohello application’s login page as expected, but nothing happens</span></span>

-   <span data-ttu-id="2198c-168">Ruční zachycení zobrazí toowork, ale jednotného přihlašování nemá dojít v případě, že moje uživatelé přejdou toohello aplikace hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="2198c-168">Manual capture appears toowork, but SSO doesn’t happen when my users navigate toohello application from hello Access Panel.</span></span>

<span data-ttu-id="2198c-169">Zkontrolujte následující hello, pokud dojde k některé z těchto problémů:</span><span class="sxs-lookup"><span data-stu-id="2198c-169">check hello following if you encounter any of these issues:</span></span>

-   <span data-ttu-id="2198c-170">Zajistěte, abyste měli nejnovější verzi rozšíření prohlížeče panely přístup hello hello **nainstalován** a **povoleno** podle následujících kroků hello v hello [jak tooinstall hello prohlížeče Panel přístupu rozšíření](#how-to-install-the-access-panel-browser-extension) části.</span><span class="sxs-lookup"><span data-stu-id="2198c-170">Ensure that you have hello latest version of hello access panel browser extension **installed** and **enabled** by following hello steps in hello [How tooinstall hello Access Panel Browser extension](#how-to-install-the-access-panel-browser-extension) section.</span></span>

-   <span data-ttu-id="2198c-171">Ujistěte se, že proces zachycení hello neprovádíte při prohlížeč v **privátní, se službou inPrivate nebo incognito režimu**.</span><span class="sxs-lookup"><span data-stu-id="2198c-171">Ensure that you are not attempting hello capture process while your browser in **incognito, inPrivate, or Private mode**.</span></span> <span data-ttu-id="2198c-172">v těchto režimech nepodporuje rozšíření panely Hello přístup.</span><span class="sxs-lookup"><span data-stu-id="2198c-172">hello access panel extension is not supported in these modes.</span></span>

-   <span data-ttu-id="2198c-173">Ujistěte se, že uživatelé nejsou při toosign v aplikaci toohello z hello přístupového panelu při v **privátní, se službou inPrivate nebo incognito režimu**.</span><span class="sxs-lookup"><span data-stu-id="2198c-173">Ensure that your users are not trying toosign in toohello application from hello access panel while in **incognito, inPrivate, or Private mode**.</span></span> <span data-ttu-id="2198c-174">v těchto režimech nepodporuje rozšíření panely Hello přístup.</span><span class="sxs-lookup"><span data-stu-id="2198c-174">hello access panel extension is not supported in these modes.</span></span>

-   <span data-ttu-id="2198c-175">Proces ručního zachycení hello zkusit znovu, zajistíte hello red značek přes hello správná pole.</span><span class="sxs-lookup"><span data-stu-id="2198c-175">Try hello manual capture process again, ensuring hello red markers are over hello correct fields.</span></span>

-   <span data-ttu-id="2198c-176">Pokud proces ruční zachycení hello zdá, že toohang nebo neprovádí hello přihlašovací stránku hello proces ruční zachycení nic (případě 3 výše), opakujte akci.</span><span class="sxs-lookup"><span data-stu-id="2198c-176">If hello manual capture process seems toohang, or hello sign in page doesn’t do anything (case 3 above), try hello manual capture process again.</span></span> <span data-ttu-id="2198c-177">Ale tentokrát po dokončení procesu hello, stiskněte klávesu hello **F12** tlačítko tooopen vývojářské konzole prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="2198c-177">But, this time after completing hello process, press hello **F12** button tooopen your browser’s developer console.</span></span> <span data-ttu-id="2198c-178">Jednou existuje, otevřete hello **konzoly** a typ **window.location= "&lt;zadejte přihlašovací hello v adrese url, které jste zadali při konfiguraci aplikace hello&gt;"** a potom stiskněte klávesu  **Zadejte**.</span><span class="sxs-lookup"><span data-stu-id="2198c-178">Once there, open hello **console** and type **window.location=”&lt;enter hello sign in url you specified when configuring hello app&gt;”** and then press **Enter**.</span></span> <span data-ttu-id="2198c-179">Tato síla na stránce přesměrování, který ukončit proces zachycení hello a ukládání hello pole, která byla zachycena.</span><span class="sxs-lookup"><span data-stu-id="2198c-179">This force a page redirect which end hello capture process and store hello fields that have been captured.</span></span>

<span data-ttu-id="2198c-180">Pokud žádný z těchto postupů fungovat pro vás, pomůžeme vám.</span><span class="sxs-lookup"><span data-stu-id="2198c-180">If none of these approaches work for you, we can help.</span></span> <span data-ttu-id="2198c-181">Otevře případu podpory s podrobnostmi hello jste zkusili, jakož i hello informace získané v hello [jak toosee hello podrobnosti o portálu oznámení](#i-cannot-manually-detect-sign-in-fields-for-my-application) a [jak pomoci tooget zasláním oznámení podrobnosti tooa podpory pracovník](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) částech (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="2198c-181">Open a support case with hello details of what you tried, as well as hello information gathered in hello [How toosee hello details of a portal notification](#i-cannot-manually-detect-sign-in-fields-for-my-application) and [How tooget help by sending notification details tooa support engineer](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) sections (if applicable).</span></span>

## <a name="how-tooinstall-hello-access-panel-browser-extension"></a><span data-ttu-id="2198c-182">Jak tooinstall hello rozšíření přístup k panelu prohlížeče</span><span class="sxs-lookup"><span data-stu-id="2198c-182">How tooinstall hello Access Panel Browser extension</span></span>

<span data-ttu-id="2198c-183">tooinstall hello rozšíření prohlížeče panely přístup, postupujte podle kroků hello níže:</span><span class="sxs-lookup"><span data-stu-id="2198c-183">tooinstall hello Access Panel Browser extension, follow hello steps below:</span></span>

1.  <span data-ttu-id="2198c-184">Otevřete hello [přístupový Panel](https://myapps.microsoft.com) v jednom z hello podporované prohlížeče a přihlaste se jako **uživatele** ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2198c-184">Open hello [Access Panel](https://myapps.microsoft.com) in one of hello supported browsers and sign in as a **user** in your Azure AD.</span></span>

2.  <span data-ttu-id="2198c-185">Klikněte na tlačítko **SSO heslo aplikace** v hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="2198c-185">click a **password-SSO application** in hello Access Panel.</span></span>

3.  <span data-ttu-id="2198c-186">V hello výzva s dotazem tooinstall hello softwaru, vyberte **instalovat nyní**.</span><span class="sxs-lookup"><span data-stu-id="2198c-186">In hello prompt asking tooinstall hello software, select **Install Now**.</span></span>

4.  <span data-ttu-id="2198c-187">Založené na prohlížeči, je možné směrovanou toohello odkaz ke stažení.</span><span class="sxs-lookup"><span data-stu-id="2198c-187">Based on your browser you be directed toohello download link.</span></span> <span data-ttu-id="2198c-188">**Přidat** hello rozšíření tooyour prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="2198c-188">**Add** hello extension tooyour browser.</span></span>

5.  <span data-ttu-id="2198c-189">Pokud prohlížeč zobrazí dotaz, vyberte tooeither **povolit** nebo **povolit** hello rozšíření.</span><span class="sxs-lookup"><span data-stu-id="2198c-189">If your browser asks, select tooeither **Enable** or **Allow** hello extension.</span></span>

6.  <span data-ttu-id="2198c-190">Po instalaci **restartujte** relaci prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="2198c-190">Once installed, **restart** your browser session.</span></span>

7.  <span data-ttu-id="2198c-191">Přihlaste se do hello přístupového panelu a zobrazit, pokud můžete **spusťte** SSO hesla aplikací.</span><span class="sxs-lookup"><span data-stu-id="2198c-191">Sign in into hello Access Panel and see if you can **launch** your password-SSO applications.</span></span>

<span data-ttu-id="2198c-192">Může také stáhnout hello rozšíření pro Chrome a Firefox z hello přímé odkazy níže:</span><span class="sxs-lookup"><span data-stu-id="2198c-192">You may also download hello extension for Chrome and Firefox from hello direct links below:</span></span>

-   [<span data-ttu-id="2198c-193">Rozšíření pro Chrome přístup panely</span><span class="sxs-lookup"><span data-stu-id="2198c-193">Chrome Access Panel Extension</span></span>](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [<span data-ttu-id="2198c-194">Rozšíření panelu Firefox přístup</span><span class="sxs-lookup"><span data-stu-id="2198c-194">Firefox Access Panel Extension</span></span>](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="how-toosee-hello-details-of-a-portal-notification"></a><span data-ttu-id="2198c-195">Jak toosee hello podrobnosti o portálu oznámení</span><span class="sxs-lookup"><span data-stu-id="2198c-195">How toosee hello details of a portal notification</span></span>

<span data-ttu-id="2198c-196">Podle následujících kroků hello, můžete zjistit podrobnosti o hello všechna portálu oznámení:</span><span class="sxs-lookup"><span data-stu-id="2198c-196">You can see hello details of any portal notification by following hello steps below:</span></span>

1.  <span data-ttu-id="2198c-197">Klikněte na tlačítko hello **oznámení** ikonu (hello zvonku) v pravé horní hello části hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="2198c-197">click hello **Notifications** icon (hello bell) in hello upper right of hello Azure Portal</span></span>

2.  <span data-ttu-id="2198c-198">Vyberte všechna oznámení v **chyba** stavu (ty s další toothem červený (!)).</span><span class="sxs-lookup"><span data-stu-id="2198c-198">Select any notification in an **Error** state (those with a red (!) next toothem).</span></span>

  ><span data-ttu-id="2198c-199">! Všimněte si] je nelze kliknout oznámení v **úspěšné** nebo **probíhá** stavu.</span><span class="sxs-lookup"><span data-stu-id="2198c-199">!NOTE] You cannot click notifications in a **Successful** or **In Progress** state.</span></span>
  >
  >

3.  <span data-ttu-id="2198c-200">Tento otevřený hello **podrobnosti oznámení** okno.</span><span class="sxs-lookup"><span data-stu-id="2198c-200">This open hello **Notification Details** blade.</span></span>

4.  <span data-ttu-id="2198c-201">Tyto informace použijte sami toounderstand další podrobnosti o problému hello.</span><span class="sxs-lookup"><span data-stu-id="2198c-201">Use this information yourself toounderstand more details about hello problem.</span></span>

5.  <span data-ttu-id="2198c-202">Pokud stále potřebujete pomoc, můžete také sdílet tyto informace se na podporu pracovníka nebo hello skupiny tooget Nápověda k produktu s váš problém.</span><span class="sxs-lookup"><span data-stu-id="2198c-202">If you still need help, you can also share this information with a support engineer or hello product group tooget help with your problem.</span></span>

6.  <span data-ttu-id="2198c-203">Klikněte na tlačítko hello **kopie** **ikonu** toohello napravo od hello **Chyba kopírování** textbox toocopy všechny hello tooshare podrobnosti oznámení s skupiny pracovník podpory nebo produktu.</span><span class="sxs-lookup"><span data-stu-id="2198c-203">Click hello **copy** **icon** toohello right of hello **Copy error** textbox toocopy all hello notification details tooshare with a support or product group engineer.</span></span>

## <a name="how-tooget-help-by-sending-notification-details-tooa-support-engineer"></a><span data-ttu-id="2198c-204">Jak pomoci tooget odesláním pracovníka podpory tooa podrobnosti oznámení</span><span class="sxs-lookup"><span data-stu-id="2198c-204">How tooget help by sending notification details tooa support engineer</span></span>

<span data-ttu-id="2198c-205">To je velmi důležité, že můžete sdílet **všechny níže uvedené podrobnosti o hello** s pracovníka podpory, pokud potřebujete pomoc, tak, aby se vám může pomoct rychle.</span><span class="sxs-lookup"><span data-stu-id="2198c-205">It is very important that you share **all hello details listed below** with a support engineer if you need help, so that they can help you quickly.</span></span> <span data-ttu-id="2198c-206">Můžete to provést pomocí snadno **uděláte snímek,** nebo kliknutím hello **ikona chyby kopie**, najít toohello napravo od hello **Chyba kopírování** textové pole.</span><span class="sxs-lookup"><span data-stu-id="2198c-206">You can do this easily by **taking a screenshot,** or by clicking hello **Copy error icon**, found toohello right of hello **Copy error** textbox.</span></span>

## <a name="notification-details-explained"></a><span data-ttu-id="2198c-207">Vysvětlení podrobnosti oznámení</span><span class="sxs-lookup"><span data-stu-id="2198c-207">Notification Details Explained</span></span>

<span data-ttu-id="2198c-208">Hello níže vysvětluje více co každý hello oznámení položek znamená a poskytuje příklady každého z nich.</span><span class="sxs-lookup"><span data-stu-id="2198c-208">hello below explains more what each of hello notification items means, and gives examples of each of them.</span></span>

### <a name="essential-notification-items"></a><span data-ttu-id="2198c-209">Základní oznámení položky</span><span class="sxs-lookup"><span data-stu-id="2198c-209">Essential Notification Items</span></span>

-   <span data-ttu-id="2198c-210">**Název** – hello popisný název hello oznámení</span><span class="sxs-lookup"><span data-stu-id="2198c-210">**Title** – hello descriptive title of hello notification</span></span>

    -   <span data-ttu-id="2198c-211">Příklad – **nastavení proxy aplikace**</span><span class="sxs-lookup"><span data-stu-id="2198c-211">Example – **Application proxy settings**</span></span>

-   <span data-ttu-id="2198c-212">**Popis** – hello popis co došlo k chybě v důsledku operace hello</span><span class="sxs-lookup"><span data-stu-id="2198c-212">**Description** – hello description of what occurred as a result of hello operation</span></span>

    -   <span data-ttu-id="2198c-213">Příklad – **zadaná interní adresa url je již používán jinou aplikací**</span><span class="sxs-lookup"><span data-stu-id="2198c-213">Example – **Internal url entered is already being used by another application**</span></span>

-   <span data-ttu-id="2198c-214">**Id oznámení** – hello jedinečné id oznámení hello</span><span class="sxs-lookup"><span data-stu-id="2198c-214">**Notification Id** – hello unique id of hello notification</span></span>

    -   <span data-ttu-id="2198c-215">Příklad – **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**</span><span class="sxs-lookup"><span data-stu-id="2198c-215">Example – **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**</span></span>

-   <span data-ttu-id="2198c-216">**Id žádosti klienta** – id konkrétního požadavku hello provedené prohlížeč</span><span class="sxs-lookup"><span data-stu-id="2198c-216">**Client Request Id** – hello specific request id made by your browser</span></span>

    -   <span data-ttu-id="2198c-217">Příklad – **302fd775-3329-4670-a9f3-bea37004f0bc**</span><span class="sxs-lookup"><span data-stu-id="2198c-217">Example – **302fd775-3329-4670-a9f3-bea37004f0bc**</span></span>

-   <span data-ttu-id="2198c-218">**Čas UTC razítko** – hello časové razítko, během které hello oznámení došlo k chybě, v UTC</span><span class="sxs-lookup"><span data-stu-id="2198c-218">**Time Stamp UTC** – hello timestamp during which hello notification occurred, in UTC</span></span>

    -   <span data-ttu-id="2198c-219">Příklad – **2017-03-23T19:50:43.7583681Z**</span><span class="sxs-lookup"><span data-stu-id="2198c-219">Example – **2017-03-23T19:50:43.7583681Z**</span></span>

-   <span data-ttu-id="2198c-220">**Interní Id transakce** – hello interní ID můžeme použít toolook hello Chyba v našem systému</span><span class="sxs-lookup"><span data-stu-id="2198c-220">**Internal Transaction Id** – hello internal ID we can use toolook hello error up in our systems</span></span>

    -   <span data-ttu-id="2198c-221">Příklad – **71a2f329-ca29-402f-aa72-bc00a7aca603**</span><span class="sxs-lookup"><span data-stu-id="2198c-221">Example – **71a2f329-ca29-402f-aa72-bc00a7aca603**</span></span>

-   <span data-ttu-id="2198c-222">**Hlavní název uživatele** – hello uživatele, který provedl operaci hello</span><span class="sxs-lookup"><span data-stu-id="2198c-222">**UPN** – hello user who performed hello operation</span></span>

    -   <span data-ttu-id="2198c-223">Příklad –**tperkins@f128.info**</span><span class="sxs-lookup"><span data-stu-id="2198c-223">Example – **tperkins@f128.info**</span></span>

-   <span data-ttu-id="2198c-224">**Id klienta** – hello jedinečné ID klienta hello, který hello uživatele, který provedl operaci hello byl členem</span><span class="sxs-lookup"><span data-stu-id="2198c-224">**Tenant Id** – hello unique ID of hello tenant that hello user who performed hello operation was a member of</span></span>

    -   <span data-ttu-id="2198c-225">Příklad – **7918d4b5-0442-4a97-be2d-36f9f9962ece**</span><span class="sxs-lookup"><span data-stu-id="2198c-225">Example – **7918d4b5-0442-4a97-be2d-36f9f9962ece**</span></span>

-   <span data-ttu-id="2198c-226">**Objekt uživatele Id** – hello jedinečné ID hello uživatele, který provedl operaci hello</span><span class="sxs-lookup"><span data-stu-id="2198c-226">**User object Id** – hello unique ID of hello user who performed hello operation</span></span>

    -   <span data-ttu-id="2198c-227">Příklad – **17f84be4-51f8-483a-b533-383791227a99**</span><span class="sxs-lookup"><span data-stu-id="2198c-227">Example – **17f84be4-51f8-483a-b533-383791227a99**</span></span>

### <a name="detailed-notification-items"></a><span data-ttu-id="2198c-228">Podrobné oznámení položky</span><span class="sxs-lookup"><span data-stu-id="2198c-228">Detailed Notification Items</span></span>

-   <span data-ttu-id="2198c-229">**Zobrazovaný název** – **(nesmí být prázdné)** podrobnější zobrazovaný název pro chybu hello</span><span class="sxs-lookup"><span data-stu-id="2198c-229">**Display Name** – **(can be empty)** a more detailed display name for hello error</span></span>

    -   <span data-ttu-id="2198c-230">Příklad * – **nastavení proxy aplikace**</span><span class="sxs-lookup"><span data-stu-id="2198c-230">Example *– **Application proxy settings**</span></span>

-   <span data-ttu-id="2198c-231">**Stav** – hello konkrétní stav hello oznámení</span><span class="sxs-lookup"><span data-stu-id="2198c-231">**Status** – hello specific status of hello notification</span></span>

    -   <span data-ttu-id="2198c-232">Příklad * – **se nezdařilo**</span><span class="sxs-lookup"><span data-stu-id="2198c-232">Example *– **Failed**</span></span>

-   <span data-ttu-id="2198c-233">**Id objektu** – **(nesmí být prázdné)** hello ID objektu, podle které hello byla provedena operace</span><span class="sxs-lookup"><span data-stu-id="2198c-233">**Object Id** – **(can be empty)** hello object ID against which hello operation was performed</span></span>

    -   <span data-ttu-id="2198c-234">Příklad – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**</span><span class="sxs-lookup"><span data-stu-id="2198c-234">Example – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**</span></span>

-   <span data-ttu-id="2198c-235">**Podrobnosti o** – hello podrobný popis co došlo k chybě v důsledku operace hello</span><span class="sxs-lookup"><span data-stu-id="2198c-235">**Details** – hello detailed description of what occurred as a result of hello operation</span></span>

    -   <span data-ttu-id="2198c-236">Příklad – **interní adresa url, http://bing.com/' je neplatná, protože je již používán**</span><span class="sxs-lookup"><span data-stu-id="2198c-236">Example – **Internal url 'http://bing.com/' is invalid since it is already in use**</span></span>

-   <span data-ttu-id="2198c-237">**Chyba při kopírování** – klikněte na tlačítko hello **ikona kopírování** toohello napravo od hello **Chyba kopírování** textbox toocopy všechny hello tooshare podrobnosti oznámení s skupiny pracovník podpory nebo produkt</span><span class="sxs-lookup"><span data-stu-id="2198c-237">**Copy error** – Click hello **copy icon** toohello right of hello **Copy error** textbox toocopy all hello notification details tooshare with a support or product group engineer</span></span>

    -   <span data-ttu-id="2198c-238">Příklad –```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```</span><span class="sxs-lookup"><span data-stu-id="2198c-238">Example – ```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```</span></span>

## <a name="next-steps"></a><span data-ttu-id="2198c-239">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2198c-239">Next steps</span></span>
[<span data-ttu-id="2198c-240">Zadejte tooyour přihlašování aplikací pomocí Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="2198c-240">Provide single sign-on tooyour apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)

