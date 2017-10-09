---
title: "přihlášení aplikace tooan pomocí odkazu deeplink aaaProblems | Microsoft Docs"
description: "Jak problémy tootroubleshoot přístupu k aplikaci z adresy URL odkazu deeplink pomocí služby Azure AD"
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
ms.openlocfilehash: dc82410001ac05895cc0244c3a89ace71bcf01a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="problems-signing-in-tooan-application-using-a-deeplink"></a><span data-ttu-id="091ce-103">Potíže s přihlášením tooan aplikace pomocí odkazu deeplink</span><span class="sxs-lookup"><span data-stu-id="091ce-103">Problems signing in tooan application using a deeplink</span></span>

<span data-ttu-id="091ce-104">Hello přístupový Panel je webový portál, který umožňuje uživatele, který má pracovní nebo školní účet v Azure Active Directory (Azure AD) tooview a spusťte cloudové aplikace této hello správce Azure AD udělil je přístup k.</span><span class="sxs-lookup"><span data-stu-id="091ce-104">hello Access Panel is a web-based portal which enables a user with a work or school account in Azure Active Directory (Azure AD) tooview and start cloud-based applications that hello Azure AD administrator has granted them access to.</span></span> 

<span data-ttu-id="091ce-105">Tyto aplikace jsou konfigurovány jménem uživatele hello na portálu Azure AD hello.</span><span class="sxs-lookup"><span data-stu-id="091ce-105">These applications are configured on behalf of hello user in hello Azure AD portal.</span></span> <span data-ttu-id="091ce-106">Hello aplikace musí být správně nakonfigurované a přiřazené toohello uživatele nebo skupiny hello uživatel je členem toosee hello aplikace hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="091ce-106">hello application must be configured properly and assigned toohello user or a group hello user is a member of toosee hello application in hello Access Panel.</span></span>

<span data-ttu-id="091ce-107">Přístup uživatele nebo přímých odkazů URL jsou odkazy, které uživatelé mohou používat tooaccess jejich SSO heslo aplikace přímo z jejich adresy URL prohlížečů panely.</span><span class="sxs-lookup"><span data-stu-id="091ce-107">Deep links or User access URLs are links your users may use tooaccess their password-SSO applications directly from their browsers URL bars.</span></span> <span data-ttu-id="091ce-108">Přechodem toothis odkaz uživatelé být automaticky přihlášeni hello aplikace bez nutnosti nejprve toogo toohello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="091ce-108">By navigating toothis link, users be automatically signed into hello application without having toogo toohello Access Panel first.</span></span> <span data-ttu-id="091ce-109">Toto je hello stejného propojení, která uživatelé používají tooaccess těchto aplikací od spuštění aplikace hello Office 365.</span><span class="sxs-lookup"><span data-stu-id="091ce-109">This is hello same link that users use tooaccess these applications from hello Office 365 application launcher.</span></span>

## <a name="general-issues-toocheck-first"></a><span data-ttu-id="091ce-110">Obecné problémy toocheck nejprve</span><span class="sxs-lookup"><span data-stu-id="091ce-110">General issues toocheck first</span></span>

-   <span data-ttu-id="091ce-111">Zajistěte, aby vaše použití **prohlížeče** , splňuje minimální požadavky hello hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="091ce-111">Make sure your using a **browser** that meets hello minimum requirements for hello Access Panel.</span></span>

-   <span data-ttu-id="091ce-112">Zajistěte, aby prohlížeče hello uživatele přidala hello URL tooits aplikace hello **Důvěryhodné servery**.</span><span class="sxs-lookup"><span data-stu-id="091ce-112">Make sure hello user’s browser has added hello URL of hello application tooits **trusted sites**.</span></span>

-   <span data-ttu-id="091ce-113">Zkontrolujte, zda je aplikace hello toocheck **nakonfigurované** správně.</span><span class="sxs-lookup"><span data-stu-id="091ce-113">Make sure toocheck hello application is **configured** correctly.</span></span>

-   <span data-ttu-id="091ce-114">Zkontrolujte, zda text hello uživatelský účet je **povoleno** pro přihlášení.</span><span class="sxs-lookup"><span data-stu-id="091ce-114">Make sure hello user’s account is **enabled** for sign ins.</span></span>

-   <span data-ttu-id="091ce-115">Zkontrolujte, zda text hello uživatelský účet je **není uzamčen.**</span><span class="sxs-lookup"><span data-stu-id="091ce-115">Make sure hello user’s account is **not locked out.**</span></span>

-   <span data-ttu-id="091ce-116">Ujistěte se, zda text hello uživatele **není platnost nebo zapomenete heslo.**</span><span class="sxs-lookup"><span data-stu-id="091ce-116">Make sure hello user’s **password is not expired or forgotten.**</span></span>

-   <span data-ttu-id="091ce-117">Zajistěte, aby **Multi-Factor Authentication** neblokuje přístup uživatelů.</span><span class="sxs-lookup"><span data-stu-id="091ce-117">Make sure **Multi-Factor Authentication** is not blocking user access.</span></span>

-   <span data-ttu-id="091ce-118">Zajistěte, aby **zásady podmíněného přístupu** nebo **Identity Protection** zásad neblokuje přístup uživatelů.</span><span class="sxs-lookup"><span data-stu-id="091ce-118">Make sure a **Conditional Access policy** or **Identity Protection** policy is not blocking user access.</span></span>

-   <span data-ttu-id="091ce-119">Ujistěte se, že uživatele **ověřování kontaktní údaje** je toodate tooallow ověřování Multi-Factor Authentication nebo podmíněného přístupu zásady toobe vynucené.</span><span class="sxs-lookup"><span data-stu-id="091ce-119">Make sure that a user’s **authentication contact info** is up toodate tooallow Multi-Factor Authentication or Conditional Access policies toobe enforced.</span></span>

-   <span data-ttu-id="091ce-120">Zkontrolujte že tooalso zkuste vymazání souborů cookie v prohlížeči a toosign v zkusit to znovu.</span><span class="sxs-lookup"><span data-stu-id="091ce-120">Make sure tooalso try clearing your browser’s cookies and trying toosign in again.</span></span>

## <a name="checking-hello-deeplink"></a><span data-ttu-id="091ce-121">Kontrola, zda text hello odkazu deeplink</span><span class="sxs-lookup"><span data-stu-id="091ce-121">Checking hello deeplink</span></span>

<span data-ttu-id="091ce-122">toocheck, pokud máte správný odkazu deeplink hello, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="091ce-122">toocheck if you have hello correct deeplink, follow hello steps below:</span></span>

1.  <span data-ttu-id="091ce-123">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="091ce-123">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="091ce-124">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="091ce-124">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="091ce-125">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="091ce-125">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="091ce-126">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="091ce-126">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="091ce-127">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="091ce-127">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="091ce-128">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="091ce-128">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="091ce-129">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="091ce-129">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

7.  <span data-ttu-id="091ce-130">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="091ce-130">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

8.  <span data-ttu-id="091ce-131">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="091ce-131">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

9.  <span data-ttu-id="091ce-132">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="091ce-132">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

10. <span data-ttu-id="091ce-133">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="091ce-133">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="091ce-134">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="091ce-134">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

11. <span data-ttu-id="091ce-135">Vyberte hello aplikaci, kterou chcete hello kontrola hello odkazu deeplink pro.</span><span class="sxs-lookup"><span data-stu-id="091ce-135">Select hello application you want hello check hello deeplink for.</span></span>

12. <span data-ttu-id="091ce-136">Najít hello popisek **adresu URL pro přístup uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="091ce-136">Find hello label **User Access URL**.</span></span> <span data-ttu-id="091ce-137">Tato adresa URL by měl odpovídat můžete odkazu deeplink.</span><span class="sxs-lookup"><span data-stu-id="091ce-137">You deeplink should match this URL.</span></span>

## <a name="how-tooinstall-hello-access-panel-browser-extension"></a><span data-ttu-id="091ce-138">Jak tooinstall hello rozšíření přístup k panelu prohlížeče</span><span class="sxs-lookup"><span data-stu-id="091ce-138">How tooinstall hello Access Panel Browser extension</span></span>

<span data-ttu-id="091ce-139">tooinstall hello rozšíření prohlížeče panely přístup, postupujte podle kroků hello níže:</span><span class="sxs-lookup"><span data-stu-id="091ce-139">tooinstall hello Access Panel Browser extension, follow hello steps below:</span></span>

1.  <span data-ttu-id="091ce-140">Otevřete hello [přístupový Panel](https://myapps.microsoft.com) v jednom z hello podporované prohlížeče a přihlaste se jako **uživatele** ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="091ce-140">Open hello [Access Panel](https://myapps.microsoft.com) in one of hello supported browsers and sign in as a **user** in your Azure AD.</span></span>

2.  <span data-ttu-id="091ce-141">Klikněte na tlačítko **SSO heslo aplikace** v hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="091ce-141">click a **password-SSO application** in hello Access Panel.</span></span>

3.  <span data-ttu-id="091ce-142">V hello výzva s dotazem tooinstall hello softwaru, vyberte **instalovat nyní**.</span><span class="sxs-lookup"><span data-stu-id="091ce-142">In hello prompt asking tooinstall hello software, select **Install Now**.</span></span>

4.  <span data-ttu-id="091ce-143">Založené na prohlížeči, je možné směrovanou toohello odkaz ke stažení.</span><span class="sxs-lookup"><span data-stu-id="091ce-143">Based on your browser you be directed toohello download link.</span></span> <span data-ttu-id="091ce-144">**Přidat** hello rozšíření tooyour prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="091ce-144">**Add** hello extension tooyour browser.</span></span>

5.  <span data-ttu-id="091ce-145">Pokud prohlížeč zobrazí dotaz, vyberte tooeither **povolit** nebo **povolit** hello rozšíření.</span><span class="sxs-lookup"><span data-stu-id="091ce-145">If your browser asks, select tooeither **Enable** or **Allow** hello extension.</span></span>

6.  <span data-ttu-id="091ce-146">Po instalaci **restartujte** relaci prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="091ce-146">Once installed, **restart** your browser session.</span></span>

7.  <span data-ttu-id="091ce-147">Přihlaste se do hello přístupového panelu a zobrazit, pokud můžete **spusťte** aplikace heslo SSO</span><span class="sxs-lookup"><span data-stu-id="091ce-147">Sign in into hello Access Panel and see if you can **launch** your password-SSO applications</span></span>

<span data-ttu-id="091ce-148">Může také stáhnout hello rozšíření pro Chrome a Firefox z hello přímé odkazy níže:</span><span class="sxs-lookup"><span data-stu-id="091ce-148">You may also download hello extension for Chrome and Firefox from hello direct links below:</span></span>

-   [<span data-ttu-id="091ce-149">Rozšíření pro Chrome přístup panely</span><span class="sxs-lookup"><span data-stu-id="091ce-149">Chrome Access Panel Extension</span></span>](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [<span data-ttu-id="091ce-150">Rozšíření panelu Firefox přístup</span><span class="sxs-lookup"><span data-stu-id="091ce-150">Firefox Access Panel Extension</span></span>](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="how-tooconfigure-password-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="091ce-151">Jak tooconfigure heslo jednotného přihlašování pro aplikaci Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="091ce-151">How tooconfigure password single sign-on for an Azure AD gallery application</span></span>

<span data-ttu-id="091ce-152">tooconfigure aplikaci z Galerie Azure AD hello budete muset:</span><span class="sxs-lookup"><span data-stu-id="091ce-152">tooconfigure an application from hello Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="091ce-153">Přidat aplikaci z Galerie Azure AD hello</span><span class="sxs-lookup"><span data-stu-id="091ce-153">Add an application from hello Azure AD gallery</span></span>](#add-an-application-from-the-Azure-AD-gallery)

-   [<span data-ttu-id="091ce-154">Konfigurace aplikace hello pro heslo jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="091ce-154">Configure hello application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

### <a name="add-an-application-from-hello-azure-ad-gallery"></a><span data-ttu-id="091ce-155">Přidat aplikaci z Galerie Azure AD hello</span><span class="sxs-lookup"><span data-stu-id="091ce-155">Add an application from hello Azure AD gallery</span></span>

<span data-ttu-id="091ce-156">tooadd aplikace hello Galerie Azure AD, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="091ce-156">tooadd an application from hello Azure AD Gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="091ce-157">Otevřete hello [portálu Azure](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**.</span><span class="sxs-lookup"><span data-stu-id="091ce-157">Open hello [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**.</span></span>

2.  <span data-ttu-id="091ce-158">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="091ce-158">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="091ce-159">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="091ce-159">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="091ce-160">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="091ce-160">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="091ce-161">Klikněte na tlačítko hello **přidat** tlačítko v pravém horním rohu hello na hello **podnikové aplikace, které** okno.</span><span class="sxs-lookup"><span data-stu-id="091ce-161">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="091ce-162">V hello **zadejte název** textbox z hello **přidat z Galerie hello** části, název typu hello aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="091ce-162">In hello **Enter a name** textbox from hello **Add from hello gallery** section, type hello name of hello application.</span></span>

7.  <span data-ttu-id="091ce-163">Vyberte hello aplikaci tooconfigure pro jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="091ce-163">Select hello application you want tooconfigure for single sign-on.</span></span>

8.  <span data-ttu-id="091ce-164">Před přidáním hello aplikace, můžete změnit její název hello **název** textové pole.</span><span class="sxs-lookup"><span data-stu-id="091ce-164">Before adding hello application, you can change its name from hello **Name** textbox.</span></span>

9.  <span data-ttu-id="091ce-165">Klikněte na tlačítko **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="091ce-165">Click **Add** button, tooadd hello application.</span></span>

<span data-ttu-id="091ce-166">Po krátké době být okno Konfigurace aplikací mít toosee hello.</span><span class="sxs-lookup"><span data-stu-id="091ce-166">After a short period, you be able toosee hello application’s configuration blade.</span></span>

### <a name="configure-hello-application-for-password-single-sign-on"></a><span data-ttu-id="091ce-167">Konfigurace aplikace hello pro heslo jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="091ce-167">Configure hello application for password single sign-on</span></span>

<span data-ttu-id="091ce-168">tooconfigure jednotné přihlašování pro aplikace, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="091ce-168">tooconfigure single sign-on for an application, follow hello steps below:</span></span>

1.  <span data-ttu-id="091ce-169">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="091ce-169">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="091ce-170">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="091ce-170">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="091ce-171">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="091ce-171">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="091ce-172">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="091ce-172">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="091ce-173">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="091ce-173">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="091ce-174">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="091ce-174">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="091ce-175">Vyberte hello aplikaci tooconfigure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="091ce-175">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="091ce-176">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="091ce-176">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="091ce-177">Vyberte hello režimu **založené na heslech přihlášení.**</span><span class="sxs-lookup"><span data-stu-id="091ce-177">Select hello mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="091ce-178">[Přiřazení uživatelů s aplikací toohello](#how-to-assign-a-user-to-an-application-directly).</span><span class="sxs-lookup"><span data-stu-id="091ce-178">[Assign users toohello application](#how-to-assign-a-user-to-an-application-directly).</span></span>

10. <span data-ttu-id="091ce-179">Kromě toho můžete taky zadat přihlašovací údaje jménem uživatele hello výběrem řádků hello hello uživatelů a kliknete na **pověření aktualizace** a zadáním jménem uživatelů hello hello uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="091ce-179">Additionally, you can also provide credentials on behalf of hello user by selecting hello rows of hello users and clicking on **Update Credentials** and entering hello username and password on behalf of hello users.</span></span> <span data-ttu-id="091ce-180">Uživatelé, jinak nebudou výzvami tooenter hello přihlašovací údaje sami při spuštění.</span><span class="sxs-lookup"><span data-stu-id="091ce-180">Otherwise, users be prompted tooenter hello credentials themselves upon launch.</span></span>

## <a name="how-tooconfigure-password-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="091ce-181">Jak tooconfigure heslo jednotného přihlašování pro aplikace bez Galerie</span><span class="sxs-lookup"><span data-stu-id="091ce-181">How tooconfigure password single sign-on for a non-gallery application</span></span>

<span data-ttu-id="091ce-182">tooconfigure aplikaci z Galerie Azure AD hello budete muset:</span><span class="sxs-lookup"><span data-stu-id="091ce-182">tooconfigure an application from hello Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="091ce-183">Přidání aplikace bez Galerie</span><span class="sxs-lookup"><span data-stu-id="091ce-183">Add a non-gallery application</span></span>](#add-a-non-gallery-application)

-   [<span data-ttu-id="091ce-184">Konfigurace aplikace hello pro heslo jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="091ce-184">Configure hello application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

### <a name="add-a-non-gallery-application"></a><span data-ttu-id="091ce-185">Přidání aplikace bez Galerie</span><span class="sxs-lookup"><span data-stu-id="091ce-185">Add a non-gallery application</span></span>

<span data-ttu-id="091ce-186">tooadd aplikace hello Galerie Azure AD, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="091ce-186">tooadd an application from hello Azure AD Gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="091ce-187">Otevřete hello [portálu Azure](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**.</span><span class="sxs-lookup"><span data-stu-id="091ce-187">Open hello [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**.</span></span>

2.  <span data-ttu-id="091ce-188">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="091ce-188">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="091ce-189">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="091ce-189">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="091ce-190">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="091ce-190">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="091ce-191">Klikněte na tlačítko hello **přidat** tlačítko v pravém horním rohu hello na hello **podnikové aplikace, které** okno.</span><span class="sxs-lookup"><span data-stu-id="091ce-191">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="091ce-192">Klikněte na tlačítko **Galerie jiná aplikace.**</span><span class="sxs-lookup"><span data-stu-id="091ce-192">click **Non-gallery application.**</span></span>

7.  <span data-ttu-id="091ce-193">Zadejte název vaší aplikace hello v hello **název** textové pole.</span><span class="sxs-lookup"><span data-stu-id="091ce-193">Enter hello name of your application in hello **Name** textbox.</span></span> <span data-ttu-id="091ce-194">Vyberte **přidat.**</span><span class="sxs-lookup"><span data-stu-id="091ce-194">Select **Add.**</span></span>

<span data-ttu-id="091ce-195">Po krátké době být okno Konfigurace aplikací mít toosee hello.</span><span class="sxs-lookup"><span data-stu-id="091ce-195">After a short period, you be able toosee hello application’s configuration blade.</span></span>

### <a name="configure-hello-application-for-password-single-sign-on"></a><span data-ttu-id="091ce-196">Konfigurace aplikace hello pro heslo jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="091ce-196">Configure hello application for password single sign-on</span></span>

<span data-ttu-id="091ce-197">tooconfigure jednotné přihlašování pro aplikace, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="091ce-197">tooconfigure single sign-on for an application, follow hello steps below:</span></span>

1.  <span data-ttu-id="091ce-198">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="091ce-198">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="091ce-199">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="091ce-199">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="091ce-200">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="091ce-200">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="091ce-201">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="091ce-201">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="091ce-202">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="091ce-202">click **All Applications** tooview a list of all your applications.</span></span>

    1.  <span data-ttu-id="091ce-203">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="091ce-203">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="091ce-204">Vyberte hello aplikaci tooconfigure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="091ce-204">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="091ce-205">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="091ce-205">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="091ce-206">Vyberte hello režimu **založené na heslech přihlášení.**</span><span class="sxs-lookup"><span data-stu-id="091ce-206">Select hello mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="091ce-207">Zadejte hello **přihlašovací adresa URL**.</span><span class="sxs-lookup"><span data-stu-id="091ce-207">Enter hello **Sign-on URL**.</span></span> <span data-ttu-id="091ce-208">Toto je hello URL, kde uživatelé zadat toosign své uživatelské jméno a heslo v k.</span><span class="sxs-lookup"><span data-stu-id="091ce-208">This is hello URL where users enter their username and password toosign in to.</span></span> <span data-ttu-id="091ce-209">Zkontrolujte, zda jsou viditelné na adrese URL hello hello přihlášení pole.</span><span class="sxs-lookup"><span data-stu-id="091ce-209">Ensure hello sign in fields are visible at hello URL.</span></span>

10. <span data-ttu-id="091ce-210">Přiřazení uživatelů toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="091ce-210">Assign users toohello application.</span></span>

11. <span data-ttu-id="091ce-211">Kromě toho můžete taky zadat přihlašovací údaje jménem uživatele hello výběrem řádků hello hello uživatelů a kliknete na **pověření aktualizace** a zadáním jménem uživatelů hello hello uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="091ce-211">Additionally, you can also provide credentials on behalf of hello user by selecting hello rows of hello users and clicking on **Update Credentials** and entering hello username and password on behalf of hello users.</span></span> <span data-ttu-id="091ce-212">Uživatelé, jinak nebudou výzvami tooenter hello přihlašovací údaje sami při spuštění.</span><span class="sxs-lookup"><span data-stu-id="091ce-212">Otherwise, users be prompted tooenter hello credentials themselves upon launch.</span></span>

## <a name="how-tooassign-a-user-tooan-application-directly"></a><span data-ttu-id="091ce-213">Jak tooassign tooan aplikace uživatele přímo</span><span class="sxs-lookup"><span data-stu-id="091ce-213">How tooassign a user tooan application directly</span></span>

<span data-ttu-id="091ce-214">tooassign jeden nebo více uživatelů tooan aplikace přímo, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="091ce-214">tooassign one or more users tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="091ce-215">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="091ce-215">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="091ce-216">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="091ce-216">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="091ce-217">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="091ce-217">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="091ce-218">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="091ce-218">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="091ce-219">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="091ce-219">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="091ce-220">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="091ce-220">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="091ce-221">Vyberte aplikaci hello chcete tooassign seznam uživatele toofrom hello.</span><span class="sxs-lookup"><span data-stu-id="091ce-221">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="091ce-222">Po načtení hello aplikace, klikněte na **uživatelů a skupin** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="091ce-222">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="091ce-223">Klikněte na tlačítko hello **přidat** tlačítko nad hello **uživatelů a skupin** seznamu tooopen hello **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="091ce-223">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="091ce-224">Klikněte na tlačítko hello **uživatelů a skupin** selektor z hello **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="091ce-224">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="091ce-225">Typ v hello **celý název** nebo **e-mailová adresa** hello uživatele, které vás zajímají přiřazení do hello **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="091ce-225">Type in hello **full name** or **email address** of hello user you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="091ce-226">Pozastavte ukazatel myši nad hello **uživatele** v seznamu tooreveal hello **políčko**.</span><span class="sxs-lookup"><span data-stu-id="091ce-226">Hover over hello **user** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="091ce-227">Klikněte na profil hello políčko další toohello uživatele fotografie nebo logo tooadd vaše uživatele toohello **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="091ce-227">Click hello checkbox next toohello user’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="091ce-228">**Volitelné:** Pokud byste chtěli příliš**přidat více než jeden uživatel**, zadejte v jiném **celý název** nebo **e-mailová adresa** do hello **vyhledávání podle názvu nebo e-mailová adresa** pole pro vyhledávání a klikněte na zaškrtávací políčko tooadd hello tento uživatel toohello **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="091ce-228">**Optional:** If you would like too**add more than one user**, type in another **full name** or **email address** into hello **Search by name or email address** search box, and click hello checkbox tooadd this user toohello **Selected** list.</span></span>

13. <span data-ttu-id="091ce-229">Po dokončení výběru uživatelů klikněte na hello **vyberte** tlačítko tooadd je toohello seznam uživatelů a skupin toobe přiřazené toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="091ce-229">When you are finished selecting users, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="091ce-230">**Volitelné:** klikněte na tlačítko hello **vybrat roli** selektor v hello **přidat přiřazení** okno tooselect roli uživatele toohello tooassign jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="091ce-230">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello users you have selected.</span></span>

15. <span data-ttu-id="091ce-231">Klikněte na tlačítko hello **přiřadit** tlačítko tooassign hello aplikace toohello vybraných uživatelů.</span><span class="sxs-lookup"><span data-stu-id="091ce-231">Click hello **Assign** button tooassign hello application toohello selected users.</span></span>

<span data-ttu-id="091ce-232">Po krátké době hello uživatelů, které jste vybrali být schopný toolaunch tyto aplikace v hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="091ce-232">After a short period, hello users you have selected be able toolaunch these applications in hello Access Panel.</span></span>

## <a name="if-these-troubleshooting-steps-do-not-hello-resolve-hello-issue"></a><span data-ttu-id="091ce-233">Pokud tento postup řešení není hello hello problém vyřešte.</span><span class="sxs-lookup"><span data-stu-id="091ce-233">If these troubleshooting steps do not hello resolve hello issue.</span></span> 

<span data-ttu-id="091ce-234">Otevřete lístek podpory se hello, pokud je k dispozici následující informace:</span><span class="sxs-lookup"><span data-stu-id="091ce-234">open a support ticket with hello following information if available:</span></span>

-   <span data-ttu-id="091ce-235">ID chyby korelace</span><span class="sxs-lookup"><span data-stu-id="091ce-235">Correlation error ID</span></span>

-   <span data-ttu-id="091ce-236">Hlavní název uživatele (e-mailovou adresu uživatele)</span><span class="sxs-lookup"><span data-stu-id="091ce-236">UPN (user email address)</span></span>

-   <span data-ttu-id="091ce-237">TenantID</span><span class="sxs-lookup"><span data-stu-id="091ce-237">TenantID</span></span>

-   <span data-ttu-id="091ce-238">Typ prohlížeče</span><span class="sxs-lookup"><span data-stu-id="091ce-238">Browser type</span></span>

-   <span data-ttu-id="091ce-239">Dojde k časové pásmo a čas nebo období při chybě</span><span class="sxs-lookup"><span data-stu-id="091ce-239">Time zone and time/timeframe during error occurs</span></span>

-   <span data-ttu-id="091ce-240">Fiddler trasování</span><span class="sxs-lookup"><span data-stu-id="091ce-240">Fiddler traces</span></span>

## <a name="next-steps"></a><span data-ttu-id="091ce-241">Další kroky</span><span class="sxs-lookup"><span data-stu-id="091ce-241">Next steps</span></span>
[<span data-ttu-id="091ce-242">Zadejte tooyour přihlašování aplikací pomocí Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="091ce-242">Provide single sign-on tooyour apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)
