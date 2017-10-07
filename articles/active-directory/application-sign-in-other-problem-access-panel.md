---
title: "přihlášení aplikace tooan z hello přístupový panel aaaProblems | Microsoft Docs"
description: "Jak problémy tootroubleshoot přístupu k aplikaci z hello Microsoft přístupový Panel Azure AD v myapps.microsoft.com"
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
ms.reviewer: japere
ms.openlocfilehash: 346c4da06416bb9b330bdd5b1201253af19ba58b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="problems-signing-in-tooan-application-from-hello-access-panel"></a><span data-ttu-id="eda51-103">Potíže s přihlášením tooan aplikace hello přístupového panelu</span><span class="sxs-lookup"><span data-stu-id="eda51-103">Problems signing in tooan application from hello access panel</span></span>

<span data-ttu-id="eda51-104">Hello přístupový Panel je webový portál, který umožňuje uživatele, který má pracovní nebo školní účet v Azure Active Directory (Azure AD) tooview a spusťte cloudové aplikace této hello správce Azure AD udělil je přístup k.</span><span class="sxs-lookup"><span data-stu-id="eda51-104">hello Access Panel is a web-based portal which enables a user with a work or school account in Azure Active Directory (Azure AD) tooview and start cloud-based applications that hello Azure AD administrator has granted them access to.</span></span> 

<span data-ttu-id="eda51-105">Tyto aplikace jsou konfigurovány jménem uživatele hello na portálu Azure AD hello.</span><span class="sxs-lookup"><span data-stu-id="eda51-105">These applications are configured on behalf of hello user in hello Azure AD portal.</span></span> <span data-ttu-id="eda51-106">Hello aplikace musí být správně nakonfigurované a přiřazené toohello uživatele nebo skupiny hello uživatel je členem toosee hello aplikace hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="eda51-106">hello application must be configured properly and assigned toohello user or a group hello user is a member of toosee hello application in hello Access Panel.</span></span>

<span data-ttu-id="eda51-107">Typ Hello aplikací, které zobrazuje uživatel možná spadat hello následujících kategorií:</span><span class="sxs-lookup"><span data-stu-id="eda51-107">hello type of apps a user may be seeing fall in hello following categories:</span></span>

-   <span data-ttu-id="eda51-108">Aplikace Office 365</span><span class="sxs-lookup"><span data-stu-id="eda51-108">Office 365 Applications</span></span>

-   <span data-ttu-id="eda51-109">Společnost Microsoft a třetích stran aplikace, které jsou nakonfigurované pomocí jednotného přihlašování na základě federation</span><span class="sxs-lookup"><span data-stu-id="eda51-109">Microsoft and third-party applications configured with federation-based SSO</span></span>

-   <span data-ttu-id="eda51-110">Aplikace založené na heslech jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="eda51-110">Password-based SSO applications</span></span>

-   <span data-ttu-id="eda51-111">Aplikace s stávajících řešení pro jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="eda51-111">Applications with existing SSO solutions</span></span>

## <a name="general-issues-toocheck-first"></a><span data-ttu-id="eda51-112">Obecné problémy toocheck nejprve</span><span class="sxs-lookup"><span data-stu-id="eda51-112">General issues toocheck first</span></span>

-   <span data-ttu-id="eda51-113">Zajistěte, aby vaše použití **prohlížeče** , splňuje minimální požadavky hello hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="eda51-113">Make sure your using a **browser** that meets hello minimum requirements for hello Access Panel.</span></span>

-   <span data-ttu-id="eda51-114">Zajistěte, aby prohlížeče hello uživatele přidala hello URL tooits aplikace hello **Důvěryhodné servery**.</span><span class="sxs-lookup"><span data-stu-id="eda51-114">Make sure hello user’s browser has added hello URL of hello application tooits **trusted sites**.</span></span>

-   <span data-ttu-id="eda51-115">Zkontrolujte, zda je aplikace hello toocheck **nakonfigurované** správně.</span><span class="sxs-lookup"><span data-stu-id="eda51-115">Make sure toocheck hello application is **configured** correctly.</span></span>

-   <span data-ttu-id="eda51-116">Zkontrolujte, zda text hello uživatelský účet je **povoleno** pro přihlášení.</span><span class="sxs-lookup"><span data-stu-id="eda51-116">Make sure hello user’s account is **enabled** for sign ins.</span></span>

-   <span data-ttu-id="eda51-117">Zkontrolujte, zda text hello uživatelský účet je **není uzamčen.**</span><span class="sxs-lookup"><span data-stu-id="eda51-117">Make sure hello user’s account is **not locked out.**</span></span>

-   <span data-ttu-id="eda51-118">Ujistěte se, zda text hello uživatele **není platnost nebo zapomenete heslo.**</span><span class="sxs-lookup"><span data-stu-id="eda51-118">Make sure hello user’s **password is not expired or forgotten.**</span></span>

-   <span data-ttu-id="eda51-119">Zajistěte, aby **Multi-Factor Authentication** neblokuje přístup uživatelů.</span><span class="sxs-lookup"><span data-stu-id="eda51-119">Make sure **Multi-Factor Authentication** is not blocking user access.</span></span>

-   <span data-ttu-id="eda51-120">Zajistěte, aby **zásady podmíněného přístupu** nebo **Identity Protection** zásad neblokuje přístup uživatelů.</span><span class="sxs-lookup"><span data-stu-id="eda51-120">Make sure a **Conditional Access policy** or **Identity Protection** policy is not blocking user access.</span></span>

-   <span data-ttu-id="eda51-121">Ujistěte se, že uživatele **ověřování kontaktní údaje** je toodate tooallow ověřování Multi-Factor Authentication nebo podmíněného přístupu zásady toobe vynucené.</span><span class="sxs-lookup"><span data-stu-id="eda51-121">Make sure that a user’s **authentication contact info** is up toodate tooallow Multi-Factor Authentication or Conditional Access policies toobe enforced.</span></span>

-   <span data-ttu-id="eda51-122">Zkontrolujte že tooalso zkuste vymazání souborů cookie v prohlížeči a toosign v zkusit to znovu.</span><span class="sxs-lookup"><span data-stu-id="eda51-122">Make sure tooalso try clearing your browser’s cookies and trying toosign in again.</span></span>

## <a name="meeting-browser-requirements-for-hello-access-panel"></a><span data-ttu-id="eda51-123">Splňuje požadavky na prohlížeč pro hello přístupového panelu</span><span class="sxs-lookup"><span data-stu-id="eda51-123">Meeting browser requirements for hello Access Panel</span></span>

<span data-ttu-id="eda51-124">Hello přístupový Panel vyžaduje prohlížeč, který podporuje jazyk JavaScript a aktivoval šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="eda51-124">hello Access Panel requires a browser that supports JavaScript and has CSS enabled.</span></span> <span data-ttu-id="eda51-125">toouse založené na heslech jednotné přihlašování (SSO) v hello přístupový Panel hello přístupový Panel rozšíření musí být nainstalován v prohlížeči hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="eda51-125">toouse password-based single sign-on (SSO) in hello Access Panel, hello Access Panel extension must be installed in hello user’s browser.</span></span> <span data-ttu-id="eda51-126">Toto rozšíření je stažen automaticky, když uživatel vybere aplikaci, která je nakonfigurována pro jednotné přihlašování založené na heslech.</span><span class="sxs-lookup"><span data-stu-id="eda51-126">This extension is downloaded automatically when a user selects an application that is configured for password-based SSO.</span></span>

<span data-ttu-id="eda51-127">Pro jednotné přihlašování založené na heslech může být prohlížeče hello koncového uživatele:</span><span class="sxs-lookup"><span data-stu-id="eda51-127">For password-based SSO, hello end user’s browsers can be:</span></span>

-   <span data-ttu-id="eda51-128">Internet Explorer 8, 9, 10, 11 – v systému Windows 7 nebo novější</span><span class="sxs-lookup"><span data-stu-id="eda51-128">Internet Explorer 8, 9, 10, 11 -- on Windows 7 or later</span></span>

-   <span data-ttu-id="eda51-129">Hraniční Windows 10 Anniversary Edition nebo novější.</span><span class="sxs-lookup"><span data-stu-id="eda51-129">Edge on Windows 10 Anniversary Edition or later</span></span>

-   <span data-ttu-id="eda51-130">Chrome – V systému Windows 7 nebo novější a v systému MacOS X nebo novější</span><span class="sxs-lookup"><span data-stu-id="eda51-130">Chrome -- on Windows 7 or later, and on MacOS X or later</span></span>

-   <span data-ttu-id="eda51-131">Firefox 26.0 nebo později – do systému Windows XP SP2 nebo novější a v systému Mac OS X 10,6 nebo novější</span><span class="sxs-lookup"><span data-stu-id="eda51-131">Firefox 26.0 or later -- on Windows XP SP2 or later, and on Mac OS X 10.6 or later</span></span>

## <a name="how-tooinstall-hello-access-panel-browser-extension"></a><span data-ttu-id="eda51-132">Jak tooinstall hello rozšíření přístup k panelu prohlížeče</span><span class="sxs-lookup"><span data-stu-id="eda51-132">How tooinstall hello Access Panel Browser extension</span></span>

<span data-ttu-id="eda51-133">tooinstall hello rozšíření prohlížeče panely přístup, postupujte podle kroků hello níže:</span><span class="sxs-lookup"><span data-stu-id="eda51-133">tooinstall hello Access Panel Browser extension, follow hello steps below:</span></span>

1.  <span data-ttu-id="eda51-134">Otevřete hello [přístupový Panel](https://myapps.microsoft.com) v jednom z hello podporované prohlížeče a přihlaste se jako **uživatele** ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="eda51-134">Open hello [Access Panel](https://myapps.microsoft.com) in one of hello supported browsers and sign in as a **user** in your Azure AD.</span></span>

2.  <span data-ttu-id="eda51-135">Klikněte na tlačítko **SSO heslo aplikace** v hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="eda51-135">Click a **password-SSO application** in hello Access Panel.</span></span>

3.  <span data-ttu-id="eda51-136">V hello výzva s dotazem tooinstall hello softwaru, vyberte **instalovat nyní**.</span><span class="sxs-lookup"><span data-stu-id="eda51-136">In hello prompt asking tooinstall hello software, select **Install Now**.</span></span>

4.  <span data-ttu-id="eda51-137">Založené na prohlížeči, je možné směrovanou toohello odkaz ke stažení.</span><span class="sxs-lookup"><span data-stu-id="eda51-137">Based on your browser you be directed toohello download link.</span></span> <span data-ttu-id="eda51-138">**Přidat** hello rozšíření tooyour prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="eda51-138">**Add** hello extension tooyour browser.</span></span>

5.  <span data-ttu-id="eda51-139">Pokud prohlížeč zobrazí dotaz, vyberte tooeither **povolit** nebo **povolit** hello rozšíření.</span><span class="sxs-lookup"><span data-stu-id="eda51-139">If your browser asks, select tooeither **Enable** or **Allow** hello extension.</span></span>

6.  <span data-ttu-id="eda51-140">Po instalaci **restartujte** relaci prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="eda51-140">Once installed, **restart** your browser session.</span></span>

7.  <span data-ttu-id="eda51-141">Přihlaste se do hello přístupového panelu a zobrazit, pokud můžete **spusťte** aplikace heslo SSO</span><span class="sxs-lookup"><span data-stu-id="eda51-141">Sign in into hello Access Panel and see if you can **launch** your password-SSO applications</span></span>

<span data-ttu-id="eda51-142">Může také stáhnout hello rozšíření pro Chrome a hraniční z hello přímé odkazy níže:</span><span class="sxs-lookup"><span data-stu-id="eda51-142">You may also download hello extension for Chrome and Edge from hello direct links below:</span></span>

-   [<span data-ttu-id="eda51-143">Rozšíření pro Chrome přístup panely</span><span class="sxs-lookup"><span data-stu-id="eda51-143">Chrome Access Panel Extension</span></span>](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [<span data-ttu-id="eda51-144">Rozšíření panelu přístup Edge</span><span class="sxs-lookup"><span data-stu-id="eda51-144">Edge Access Panel Extension</span></span>](https://www.microsoft.com/store/apps/9pc9sckkzk84)

## <a name="how-tooconfigure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="eda51-145">Jak tooconfigure federovaného jednotného přihlašování pro aplikaci Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="eda51-145">How tooconfigure federated single sign-on for an Azure AD gallery application</span></span>

<span data-ttu-id="eda51-146">Všechny aplikace v galerii Azure AD hello povolit funkce Enterprise Single Sign-On má podrobný kurz k dispozici.</span><span class="sxs-lookup"><span data-stu-id="eda51-146">All application in hello Azure AD gallery enabled with Enterprise Single Sign-On capability has a step-by-step tutorial available.</span></span> <span data-ttu-id="eda51-147">Dostanete hello [seznam kurzů toointegrate SaaS aplikací s Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) podrobností podrobné pokyny.</span><span class="sxs-lookup"><span data-stu-id="eda51-147">You can access hello [List of tutorials on how toointegrate SaaS apps with Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) for a detail step-by-step guidance.</span></span>

<span data-ttu-id="eda51-148">tooconfigure aplikaci z Galerie Azure AD hello budete muset:</span><span class="sxs-lookup"><span data-stu-id="eda51-148">tooconfigure an application from hello Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="eda51-149">Přidat aplikaci z Galerie Azure AD hello</span><span class="sxs-lookup"><span data-stu-id="eda51-149">Add an application from hello Azure AD gallery</span></span>](#add-an-application)

-   [<span data-ttu-id="eda51-150">Nakonfigurujte hodnoty metadata aplikace hello ve službě Azure AD (přihlašování adresu URL, identifikátor, adresa URL odpovědi)</span><span class="sxs-lookup"><span data-stu-id="eda51-150">Configure hello application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="eda51-151">Vyberte uživatelský identifikátor a přidejte uživatele atributy toobe odeslané toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="eda51-151">Select User Identifier and add user attributes toobe sent toohello application</span></span>](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [<span data-ttu-id="eda51-152">Načtení metadat Azure AD a certifikátů</span><span class="sxs-lookup"><span data-stu-id="eda51-152">Retrieve Azure AD metadata and certificate</span></span>](#download-the-azure-ad-metadata-or-certificate)

-   [<span data-ttu-id="eda51-153">Konfigurovat Azure AD metadata hodnoty v aplikaci hello (přihlašování adresu URL, vystavitele, adresy URL odhlašovací a certifikát)</span><span class="sxs-lookup"><span data-stu-id="eda51-153">Configure Azure AD metadata values in hello application (Sign on URL, Issuer, Logout URL and certificate)</span></span>](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="eda51-154">Přiřazení uživatelů toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="eda51-154">Assign users toohello application</span></span>](#assign-users-to-the-application)

### <a name="add-an-application-from-hello-azure-ad-gallery"></a><span data-ttu-id="eda51-155">Přidat aplikaci z Galerie Azure AD hello</span><span class="sxs-lookup"><span data-stu-id="eda51-155">Add an application from hello Azure AD gallery</span></span>

<span data-ttu-id="eda51-156">tooadd aplikace hello Galerie Azure AD, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="eda51-156">tooadd an application from hello Azure AD Gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="eda51-157">Otevřete hello [portálu Azure](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**</span><span class="sxs-lookup"><span data-stu-id="eda51-157">Open hello [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="eda51-158">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="eda51-158">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="eda51-159">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="eda51-159">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="eda51-160">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="eda51-160">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="eda51-161">Klikněte na tlačítko hello **přidat** tlačítko v pravém horním rohu hello na hello **podnikové aplikace, které** okno</span><span class="sxs-lookup"><span data-stu-id="eda51-161">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade</span></span>

6.  <span data-ttu-id="eda51-162">V hello **zadejte název** textbox z hello **přidat z Galerie hello** části, název typu hello aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="eda51-162">In hello **Enter a name** textbox from hello **Add from hello gallery** section, type hello name of hello application.</span></span>

7.  <span data-ttu-id="eda51-163">Vyberte hello aplikaci tooconfigure pro jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="eda51-163">Select hello application you want tooconfigure for single sign-on.</span></span>

8.  <span data-ttu-id="eda51-164">Před přidáním hello aplikace, můžete změnit její název hello **název** textové pole.</span><span class="sxs-lookup"><span data-stu-id="eda51-164">Before adding hello application, you can change its name from hello **Name** textbox.</span></span>

9.  <span data-ttu-id="eda51-165">Klikněte na tlačítko **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="eda51-165">Click **Add** button, tooadd hello application.</span></span>

<span data-ttu-id="eda51-166">Po krátké době být okno Konfigurace aplikací mít toosee hello.</span><span class="sxs-lookup"><span data-stu-id="eda51-166">After a short period, you be able toosee hello application’s configuration blade.</span></span>

### <a name="configure-single-sign-on-for-an-application-from-hello-azure-ad-gallery"></a><span data-ttu-id="eda51-167">Konfigurovat jednotné přihlašování pro aplikace z Galerie Azure AD hello</span><span class="sxs-lookup"><span data-stu-id="eda51-167">Configure single sign-on for an application from hello Azure AD gallery</span></span>

<span data-ttu-id="eda51-168">tooconfigure jednotné přihlašování pro aplikace, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="eda51-168">tooconfigure single sign-on for an application, follow hello steps below:</span></span>

1.  <span data-ttu-id="eda51-169"><span id="_Hlk477187909" class="anchor"><span id="_Hlk477001983" class="anchor"></span></span>Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="eda51-169"><span id="_Hlk477187909" class="anchor"><span id="_Hlk477001983" class="anchor"></span></span>Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="eda51-170">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="eda51-170">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="eda51-171">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="eda51-171">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="eda51-172">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="eda51-172">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="eda51-173">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="eda51-173">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="eda51-174">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="eda51-174">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="eda51-175">Vyberte hello aplikaci tooconfigure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="eda51-175">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="eda51-176">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="eda51-176">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="eda51-177">Vyberte **na základě SAML přihlašování** z hello **režimu** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="eda51-177">Select **SAML-based Sign-on** from hello **Mode** dropdown.</span></span>

9.  <span data-ttu-id="eda51-178">Zadejte hello požadované hodnoty v **domény a adresy URL.**</span><span class="sxs-lookup"><span data-stu-id="eda51-178">Enter hello required values in **Domain and URLs.**</span></span> <span data-ttu-id="eda51-179">Tyto hodnoty by měly získat od dodavatele aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="eda51-179">You should get these values from hello application vendor.</span></span>

   1. <span data-ttu-id="eda51-180">aplikace hello tooconfigure jako jednotné přihlašování iniciované SP, hello přihlašovací na adrese URL je požadovaná hodnota.</span><span class="sxs-lookup"><span data-stu-id="eda51-180">tooconfigure hello application as SP-initiated SSO, hello Sign on URL it’s a required value.</span></span> <span data-ttu-id="eda51-181">Některé aplikace hello identifikátor je také požadovaná hodnota.</span><span class="sxs-lookup"><span data-stu-id="eda51-181">For some applications, hello Identifier is also a required value.</span></span>

   2. <span data-ttu-id="eda51-182">aplikace hello tooconfigure jako jednotné přihlašování iniciované IdP, hello adresa URL odpovědi je požadovaná hodnota.</span><span class="sxs-lookup"><span data-stu-id="eda51-182">tooconfigure hello application as IdP-initiated SSO, hello Reply URL it’s a required value.</span></span> <span data-ttu-id="eda51-183">Některé aplikace hello identifikátor je také požadovaná hodnota.</span><span class="sxs-lookup"><span data-stu-id="eda51-183">For some applications, hello Identifier is also a required value.</span></span>

10. <span data-ttu-id="eda51-184">**Volitelné:** klikněte na tlačítko **zobrazit upřesňující nastavení adresy URL** Pokud chcete, aby toosee hello-požadované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="eda51-184">**Optional:** click **Show advanced URL settings** if you want toosee hello non-required values.</span></span>

11. <span data-ttu-id="eda51-185">V hello **uživatelské atributy**, vyberte hello jedinečný identifikátor pro uživatele v hello **uživatelský identifikátor** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="eda51-185">In hello **User attributes**, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span>

12. <span data-ttu-id="eda51-186">**Volitelné:** klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** tooedit hello atributy aplikace toohello toobe odeslaných v tokenu SAML hello při přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="eda51-186">**Optional:** click **View and edit all other user attributes** tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

   <span data-ttu-id="eda51-187">tooadd atribut:</span><span class="sxs-lookup"><span data-stu-id="eda51-187">tooadd an attribute:</span></span>

   1. <span data-ttu-id="eda51-188">Klikněte na tlačítko **přidat atribut**.</span><span class="sxs-lookup"><span data-stu-id="eda51-188">click **Add attribute**.</span></span> <span data-ttu-id="eda51-189">Zadejte hello **název** a vyberte hello hello **hodnotu** z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="eda51-189">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   2. <span data-ttu-id="eda51-190">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="eda51-190">Click **Save.**</span></span> <span data-ttu-id="eda51-191">Zobrazí hello nový atribut v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="eda51-191">You see hello new attribute in hello table.</span></span>

13. <span data-ttu-id="eda51-192">Klikněte na tlačítko **konfigurace &lt;název aplikace&gt;**  tooaccess dokumentaci týkající se tooconfigure jednotné přihlašování v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="eda51-192">click **Configure &lt;application name&gt;** tooaccess documentation on how tooconfigure single sign-on in hello application.</span></span> <span data-ttu-id="eda51-193">Navíc má adres URL metadat hello a certifikát vyžadovaný toosetup jednotné přihlašování s aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="eda51-193">Also, you has hello metadata URLs and certificate required toosetup SSO with hello application.</span></span>

14. <span data-ttu-id="eda51-194">Klikněte na tlačítko **Uložit** toosave hello konfigurace.</span><span class="sxs-lookup"><span data-stu-id="eda51-194">Click **Save** toosave hello configuration.</span></span>

15. <span data-ttu-id="eda51-195">Přiřazení uživatelů toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="eda51-195">Assign users toohello application.</span></span>

### <a name="select-user-identifier-and-add-user-attributes-toobe-sent-toohello-application"></a><span data-ttu-id="eda51-196">Vyberte uživatelský identifikátor a přidejte uživatele atributy toobe odeslané toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="eda51-196">Select User Identifier and add user attributes toobe sent toohello application</span></span>

<span data-ttu-id="eda51-197">tooselect hello identifikátor uživatele nebo přidat atributy uživatele, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="eda51-197">tooselect hello User Identifier or add user attributes, follow hello steps below:</span></span>

1.  <span data-ttu-id="eda51-198">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="eda51-198">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="eda51-199">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="eda51-199">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="eda51-200">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="eda51-200">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="eda51-201">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="eda51-201">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="eda51-202">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="eda51-202">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="eda51-203">Pokud aplikace hello chcete tooshow vytvořit tady nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="eda51-203">If you do not see hello application you want tooshow up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="eda51-204">Vyberte aplikaci hello jste nakonfigurovali jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="eda51-204">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="eda51-205">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="eda51-205">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="eda51-206">V části hello **uživatelské atributy** vyberte hello jedinečný identifikátor pro uživatele v hello **uživatelský identifikátor** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="eda51-206">Under hello **User attributes** section, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span> <span data-ttu-id="eda51-207">Hello vybranou možnost musí toomatch hello očekávaná hodnota v uživatel hello tooauthenticate aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="eda51-207">hello selected option needs toomatch hello expected value in hello application tooauthenticate hello user.</span></span>

    >[!NOTE]
    ><span data-ttu-id="eda51-208">Azure AD vyberte hello formátu pro atribut NameID hello (identifikátor uživatele) na základě hodnoty hello vybrané nebo hello formátu požadoval hello aplikace hello SAML AuthRequest.</span><span class="sxs-lookup"><span data-stu-id="eda51-208">Azure AD select hello format for hello NameID attribute (User Identifier) based on hello value selected or hello format requested by hello application in hello SAML AuthRequest.</span></span> <span data-ttu-id="eda51-209">Další informace najdete článku hello [protokolu SAML přihlašování](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) pod hello části NameIDPolicy.</span><span class="sxs-lookup"><span data-stu-id="eda51-209">For more information visit hello article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello section NameIDPolicy.</span></span>
    >
    >

9.  <span data-ttu-id="eda51-210">tooadd atributy uživatele, klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** tooedit hello atributy aplikace toohello toobe odeslaných v tokenu SAML hello při přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="eda51-210">tooadd user attributes, click **View and edit all other user attributes** tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

   <span data-ttu-id="eda51-211">tooadd atribut:</span><span class="sxs-lookup"><span data-stu-id="eda51-211">tooadd an attribute:</span></span>

   1. <span data-ttu-id="eda51-212">Klikněte na tlačítko **přidat atribut**.</span><span class="sxs-lookup"><span data-stu-id="eda51-212">click **Add attribute**.</span></span> <span data-ttu-id="eda51-213">Zadejte hello **název** a vyberte hello hello **hodnotu** z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="eda51-213">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   2. <span data-ttu-id="eda51-214">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="eda51-214">Click **Save.**</span></span> <span data-ttu-id="eda51-215">Zobrazí hello nový atribut v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="eda51-215">You see hello new attribute in hello table.</span></span>

### <a name="download-hello-azure-ad-metadata-or-certificate"></a><span data-ttu-id="eda51-216">Stažení metadat hello Azure AD nebo certifikát</span><span class="sxs-lookup"><span data-stu-id="eda51-216">Download hello Azure AD metadata or certificate</span></span>

<span data-ttu-id="eda51-217">metadata aplikace hello toodownload nebo certifikát z Azure AD, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="eda51-217">toodownload hello application metadata or certificate from Azure AD, follow hello steps below:</span></span>

1.  <span data-ttu-id="eda51-218">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="eda51-218">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="eda51-219">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="eda51-219">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="eda51-220">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="eda51-220">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="eda51-221">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="eda51-221">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="eda51-222">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="eda51-222">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="eda51-223">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="eda51-223">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="eda51-224">Vyberte aplikaci hello jste nakonfigurovali jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="eda51-224">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="eda51-225">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="eda51-225">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="eda51-226">Přejděte příliš**SAML podpisový certifikát** části a pak klikněte na **Stáhnout** hodnota sloupce.</span><span class="sxs-lookup"><span data-stu-id="eda51-226">Go too**SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="eda51-227">V závislosti na tom, jaké aplikace hello vyžaduje konfiguraci jednotné přihlašování najdete v části buď možnost toodownload hello hello soubor XML s metadaty nebo hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="eda51-227">Depending on what hello application requires configuring single sign-on, you see either hello option toodownload hello Metadata XML or hello Certificate.</span></span>

    <span data-ttu-id="eda51-228">Azure AD neposkytuje adresu URL tooget hello metadat.</span><span class="sxs-lookup"><span data-stu-id="eda51-228">Azure AD doesn’t provide a URL tooget hello metadata.</span></span> <span data-ttu-id="eda51-229">Hello metadata mohou být načteny pouze jako soubor XML.</span><span class="sxs-lookup"><span data-stu-id="eda51-229">hello metadata can only be retrieved as a XML file.</span></span>

## <a name="how-tooconfigure-federated-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="eda51-230">Jak tooconfigure federovaného jednotného přihlašování pro aplikace bez Galerie</span><span class="sxs-lookup"><span data-stu-id="eda51-230">How tooconfigure federated single sign-on for a non-gallery application</span></span>

<span data-ttu-id="eda51-231">tooconfigure bez Galerie aplikace, budete muset toohave Azure AD premium a hello aplikace podporuje SAML 2.0.</span><span class="sxs-lookup"><span data-stu-id="eda51-231">tooconfigure a non-gallery application, you need toohave Azure AD premium and hello application supports SAML 2.0.</span></span> <span data-ttu-id="eda51-232">Další informace o verzích služby Azure AD, najdete v článku [ceny služby Azure AD](https://azure.microsoft.com/pricing/details/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="eda51-232">For more information about Azure AD versions, visit [Azure AD pricing](https://azure.microsoft.com/pricing/details/active-directory/).</span></span>

-   [<span data-ttu-id="eda51-233">Nakonfigurujte hodnoty metadata aplikace hello ve službě Azure AD (přihlašování adresu URL, identifikátor, adresa URL odpovědi)</span><span class="sxs-lookup"><span data-stu-id="eda51-233">Configure hello application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>](#configuring-single-sign-on)

-   [<span data-ttu-id="eda51-234">Vyberte uživatelský identifikátor a přidejte uživatele atributy toobe odeslané toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="eda51-234">Select User Identifier and add user attributes toobe sent toohello application</span></span>](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [<span data-ttu-id="eda51-235">Načtení metadat Azure AD a certifikátů</span><span class="sxs-lookup"><span data-stu-id="eda51-235">Retrieve Azure AD metadata and certificate</span></span>](#download-the-azure-ad-metadata-or-certificate)

-   [<span data-ttu-id="eda51-236">Konfigurovat Azure AD metadata hodnoty v aplikaci hello (přihlašování adresu URL, vystavitele, adresy URL odhlašovací a certifikát)</span><span class="sxs-lookup"><span data-stu-id="eda51-236">Configure Azure AD metadata values in hello application (Sign on URL, Issuer, Logout URL and certificate)</span></span>](#configuring-single-sign-on)

### <a name="configure-hello-applications-metadata-values-in-azure-ad-sign-on-url-identifier-reply-url"></a><span data-ttu-id="eda51-237">Nakonfigurujte hodnoty metadata aplikace hello ve službě Azure AD (přihlašování adresu URL, identifikátor, adresa URL odpovědi)</span><span class="sxs-lookup"><span data-stu-id="eda51-237">Configure hello application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>

<span data-ttu-id="eda51-238">tooconfigure jednotné přihlašování pro aplikace, která není v galerii Azure AD hello, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="eda51-238">tooconfigure single sign-on for an application that is not in hello Azure AD gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="eda51-239">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="eda51-239">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="eda51-240">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="eda51-240">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="eda51-241">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="eda51-241">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="eda51-242">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="eda51-242">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="eda51-243">Klikněte na tlačítko hello **přidat** tlačítko v pravém horním rohu hello na hello **podnikové aplikace, které** okno</span><span class="sxs-lookup"><span data-stu-id="eda51-243">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade</span></span>

6.  <span data-ttu-id="eda51-244">Klikněte na tlačítko **aplikace bez Galerie** v hello **přidat vlastní aplikaci** části</span><span class="sxs-lookup"><span data-stu-id="eda51-244">click **Non-gallery application** in hello **Add your own app** section</span></span>

7.  <span data-ttu-id="eda51-245">Zadejte název hello aplikace hello v hello **název** textové pole.</span><span class="sxs-lookup"><span data-stu-id="eda51-245">Enter hello name of hello application in hello **Name** textbox.</span></span>

8.  <span data-ttu-id="eda51-246">Klikněte na tlačítko **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="eda51-246">Click **Add** button, tooadd hello application.</span></span>

9.  <span data-ttu-id="eda51-247">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="eda51-247">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

10. <span data-ttu-id="eda51-248">Vyberte **na základě SAML přihlašování** v hello **režimu** rozevíracího seznamu</span><span class="sxs-lookup"><span data-stu-id="eda51-248">Select **SAML-based Sign-on** in hello **Mode** dropdown</span></span>

11. <span data-ttu-id="eda51-249">Zadejte hello požadované hodnoty v **domény a adresy URL.**</span><span class="sxs-lookup"><span data-stu-id="eda51-249">Enter hello required values in **Domain and URLs.**</span></span> <span data-ttu-id="eda51-250">Tyto hodnoty by měly získat od dodavatele aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="eda51-250">You should get these values from hello application vendor.</span></span>

  1. <span data-ttu-id="eda51-251">aplikace hello tooconfigure jako jednotné přihlašování iniciované deklarací identity, zadejte hello adresa URL odpovědi a hello identifikátor.</span><span class="sxs-lookup"><span data-stu-id="eda51-251">tooconfigure hello application as IdP-initiated SSO, enter hello Reply URL and hello Identifier.</span></span>

  2. <span data-ttu-id="eda51-252">**Volitelné:** tooconfigure hello aplikace jako jednotné přihlašování iniciované SP, hello přihlašovací na adrese URL je požadovaná hodnota.</span><span class="sxs-lookup"><span data-stu-id="eda51-252">**Optional:** tooconfigure hello application as SP-initiated SSO, hello Sign on URL it’s a required value.</span></span>

12. <span data-ttu-id="eda51-253">V hello **uživatelské atributy**, vyberte hello jedinečný identifikátor pro uživatele v hello **uživatelský identifikátor** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="eda51-253">In hello **User attributes**, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span>

13. <span data-ttu-id="eda51-254">**Volitelné:** klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** tooedit hello atributy aplikace toohello toobe odeslaných v tokenu SAML hello při přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="eda51-254">**Optional:** click **View and edit all other user attributes** tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

   <span data-ttu-id="eda51-255">tooadd atribut:</span><span class="sxs-lookup"><span data-stu-id="eda51-255">tooadd an attribute:</span></span>

   1. <span data-ttu-id="eda51-256">Klikněte na tlačítko **přidat atribut**.</span><span class="sxs-lookup"><span data-stu-id="eda51-256">click **Add attribute**.</span></span> <span data-ttu-id="eda51-257">Zadejte hello **název** a vyberte hello hello **hodnotu** z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="eda51-257">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   2. <span data-ttu-id="eda51-258">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="eda51-258">Click **Save.**</span></span> <span data-ttu-id="eda51-259">Zobrazí hello nový atribut v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="eda51-259">You see hello new attribute in hello table.</span></span>

14. <span data-ttu-id="eda51-260">Klikněte na tlačítko **konfigurace &lt;název aplikace&gt;**  tooaccess dokumentaci týkající se tooconfigure jednotné přihlašování v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="eda51-260">click **Configure &lt;application name&gt;** tooaccess documentation on how tooconfigure single sign-on in hello application.</span></span> <span data-ttu-id="eda51-261">Navíc má Azure AD adresy URL a certifikát nutný pro aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="eda51-261">Also, you has Azure AD URLs and certificate required for hello application.</span></span>

### <a name="select-user-identifier-and-add-user-attributes-toobe-sent-toohello-application"></a><span data-ttu-id="eda51-262">Vyberte uživatelský identifikátor a přidejte uživatele atributy toobe odeslané toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="eda51-262">Select User Identifier and add user attributes toobe sent toohello application</span></span>

<span data-ttu-id="eda51-263">tooselect hello identifikátor uživatele nebo přidat atributy uživatele, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="eda51-263">tooselect hello User Identifier or add user attributes, follow hello steps below:</span></span>

1.  <span data-ttu-id="eda51-264">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="eda51-264">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="eda51-265">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="eda51-265">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="eda51-266">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="eda51-266">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="eda51-267">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="eda51-267">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="eda51-268">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="eda51-268">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="eda51-269">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="eda51-269">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="eda51-270">Vyberte aplikaci hello jste nakonfigurovali jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="eda51-270">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="eda51-271">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="eda51-271">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="eda51-272">V části hello **uživatelské atributy** vyberte hello jedinečný identifikátor pro uživatele v hello **uživatelský identifikátor** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="eda51-272">Under hello **User attributes** section, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span> <span data-ttu-id="eda51-273">Hello vybranou možnost musí toomatch hello očekávaná hodnota v uživatel hello tooauthenticate aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="eda51-273">hello selected option needs toomatch hello expected value in hello application tooauthenticate hello user.</span></span>

   >[!NOTE]
   ><span data-ttu-id="eda51-274">Azure AD vyberte hello formátu pro atribut NameID hello (identifikátor uživatele) na základě hodnoty hello vybrané nebo hello formátu požadoval hello aplikace hello SAML AuthRequest.</span><span class="sxs-lookup"><span data-stu-id="eda51-274">Azure AD select hello format for hello NameID attribute (User Identifier) based on hello value selected or hello format requested by hello application in hello SAML AuthRequest.</span></span> <span data-ttu-id="eda51-275">Další informace najdete článku hello [protokolu SAML přihlašování](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) pod hello části NameIDPolicy.</span><span class="sxs-lookup"><span data-stu-id="eda51-275">For more information visit hello article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello section NameIDPolicy.</span></span>
   >
   >

9.  <span data-ttu-id="eda51-276">tooadd atributy uživatele, klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** tooedit hello atributy aplikace toohello toobe odeslaných v tokenu SAML hello při přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="eda51-276">tooadd user attributes, click **View and edit all other user attributes** tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

   <span data-ttu-id="eda51-277">tooadd atribut:</span><span class="sxs-lookup"><span data-stu-id="eda51-277">tooadd an attribute:</span></span>

   <span data-ttu-id="eda51-278">1. klikněte na tlačítko **přidat atribut**.</span><span class="sxs-lookup"><span data-stu-id="eda51-278">1.click **Add attribute**.</span></span> <span data-ttu-id="eda51-279">Zadejte hello **název** a vyberte hello hello **hodnotu** z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="eda51-279">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   <span data-ttu-id="eda51-280">Klikněte na 2 **uložit.**</span><span class="sxs-lookup"><span data-stu-id="eda51-280">2 Click **Save.**</span></span> <span data-ttu-id="eda51-281">Zobrazí hello nový atribut v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="eda51-281">You see hello new attribute in hello table.</span></span>

### <a name="download-hello-azure-ad-metadata-or-certificate"></a><span data-ttu-id="eda51-282">Stažení metadat hello Azure AD nebo certifikát</span><span class="sxs-lookup"><span data-stu-id="eda51-282">Download hello Azure AD metadata or certificate</span></span>

<span data-ttu-id="eda51-283">metadata aplikace hello toodownload nebo certifikát z Azure AD, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="eda51-283">toodownload hello application metadata or certificate from Azure AD, follow hello steps below:</span></span>

1.  <span data-ttu-id="eda51-284">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="eda51-284">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="eda51-285">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="eda51-285">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="eda51-286">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="eda51-286">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="eda51-287">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="eda51-287">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="eda51-288">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="eda51-288">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="eda51-289">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="eda51-289">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="eda51-290">Vyberte aplikaci hello jste nakonfigurovali jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="eda51-290">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="eda51-291">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="eda51-291">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="eda51-292">Přejděte příliš**SAML podpisový certifikát** části a pak klikněte na **Stáhnout** hodnota sloupce.</span><span class="sxs-lookup"><span data-stu-id="eda51-292">Go too**SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="eda51-293">V závislosti na tom, jaké aplikace hello vyžaduje konfiguraci jednotné přihlašování najdete v části buď možnost toodownload hello hello soubor XML s metadaty nebo hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="eda51-293">Depending on what hello application requires configuring single sign-on, you see either hello option toodownload hello Metadata XML or hello Certificate.</span></span>

    <span data-ttu-id="eda51-294">Azure AD neposkytuje adresu URL tooget hello metadat.</span><span class="sxs-lookup"><span data-stu-id="eda51-294">Azure AD doesn’t provide a URL tooget hello metadata.</span></span> <span data-ttu-id="eda51-295">Hello metadata mohou být načteny pouze jako soubor XML.</span><span class="sxs-lookup"><span data-stu-id="eda51-295">hello metadata can only be retrieved as a XML file.</span></span>

## <a name="how-tooconfigure-password-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="eda51-296">Jak tooconfigure heslo jednotného přihlašování pro aplikaci Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="eda51-296">How tooconfigure password single sign-on for an Azure AD gallery application</span></span>

<span data-ttu-id="eda51-297">tooconfigure aplikaci z Galerie Azure AD hello budete muset:</span><span class="sxs-lookup"><span data-stu-id="eda51-297">tooconfigure an application from hello Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="eda51-298">Přidat aplikaci z Galerie Azure AD hello</span><span class="sxs-lookup"><span data-stu-id="eda51-298">Add an application from hello Azure AD gallery</span></span>](#add-an-application)

-   [<span data-ttu-id="eda51-299">Konfigurace aplikace hello pro heslo jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="eda51-299">Configure hello application for password single sign-on</span></span>](#configure-the-application)

### <a name="add-an-application-from-hello-azure-ad-gallery"></a><span data-ttu-id="eda51-300">Přidat aplikaci z Galerie Azure AD hello</span><span class="sxs-lookup"><span data-stu-id="eda51-300">Add an application from hello Azure AD gallery</span></span>

<span data-ttu-id="eda51-301">tooadd aplikace hello Galerie Azure AD, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="eda51-301">tooadd an application from hello Azure AD Gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="eda51-302">Otevřete hello [portálu Azure](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**</span><span class="sxs-lookup"><span data-stu-id="eda51-302">Open hello [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="eda51-303">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="eda51-303">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="eda51-304">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="eda51-304">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="eda51-305">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="eda51-305">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="eda51-306">Klikněte na tlačítko hello **přidat** tlačítko v pravém horním rohu hello na hello **podnikové aplikace, které** okno</span><span class="sxs-lookup"><span data-stu-id="eda51-306">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade</span></span>

6.  <span data-ttu-id="eda51-307">V hello **zadejte název** textbox z hello **přidat z Galerie hello** části, název typu hello aplikace hello</span><span class="sxs-lookup"><span data-stu-id="eda51-307">In hello **Enter a name** textbox from hello **Add from hello gallery** section, type hello name of hello application</span></span>

7.  <span data-ttu-id="eda51-308">Vyberte hello aplikaci tooconfigure pro jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="eda51-308">Select hello application you want tooconfigure for single sign-on</span></span>

8.  <span data-ttu-id="eda51-309">Před přidáním hello aplikace, můžete změnit její název hello **název** textové pole.</span><span class="sxs-lookup"><span data-stu-id="eda51-309">Before adding hello application, you can change its name from hello **Name** textbox.</span></span>

9.  <span data-ttu-id="eda51-310">Klikněte na tlačítko **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="eda51-310">Click **Add** button, tooadd hello application.</span></span>

<span data-ttu-id="eda51-311">Po krátké době být okno Konfigurace aplikací mít toosee hello.</span><span class="sxs-lookup"><span data-stu-id="eda51-311">After a short period, you be able toosee hello application’s configuration blade.</span></span>

### <a name="configure-hello-application-for-password-single-sign-on"></a><span data-ttu-id="eda51-312">Konfigurace aplikace hello pro heslo jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="eda51-312">Configure hello application for password single sign-on</span></span>

<span data-ttu-id="eda51-313">tooconfigure jednotné přihlašování pro aplikace, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="eda51-313">tooconfigure single sign-on for an application, follow hello steps below:</span></span>

1.  <span data-ttu-id="eda51-314">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="eda51-314">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="eda51-315">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="eda51-315">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="eda51-316">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="eda51-316">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="eda51-317">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="eda51-317">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="eda51-318">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="eda51-318">click **All Applications** tooview a list of all your applications.</span></span>

 * <span data-ttu-id="eda51-319">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="eda51-319">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="eda51-320">Vyberte hello aplikaci tooconfigure jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="eda51-320">Select hello application you want tooconfigure single sign-on</span></span>

7.  <span data-ttu-id="eda51-321">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="eda51-321">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="eda51-322">Vyberte hello režimu **založené na heslech přihlášení.**</span><span class="sxs-lookup"><span data-stu-id="eda51-322">Select hello mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="eda51-323">Přiřazení uživatelů toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="eda51-323">Assign users toohello application.</span></span>

10. <span data-ttu-id="eda51-324">Kromě toho můžete taky zadat přihlašovací údaje jménem uživatele hello výběrem řádků hello hello uživatelů a kliknete na **pověření aktualizace** a zadáním jménem uživatelů hello hello uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="eda51-324">Additionally, you can also provide credentials on behalf of hello user by selecting hello rows of hello users and clicking on **Update Credentials** and entering hello username and password on behalf of hello users.</span></span> <span data-ttu-id="eda51-325">Uživatelé, jinak nebudou výzvami tooenter hello přihlašovací údaje sami při spuštění.</span><span class="sxs-lookup"><span data-stu-id="eda51-325">Otherwise, users be prompted tooenter hello credentials themselves upon launch.</span></span>

## <a name="how-tooconfigure-password-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="eda51-326">Jak tooconfigure heslo jednotného přihlašování pro aplikace bez Galerie</span><span class="sxs-lookup"><span data-stu-id="eda51-326">How tooconfigure password single sign-on for a non-gallery application</span></span>

<span data-ttu-id="eda51-327">tooconfigure aplikaci z Galerie Azure AD hello budete muset:</span><span class="sxs-lookup"><span data-stu-id="eda51-327">tooconfigure an application from hello Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="eda51-328">Přidání aplikace bez Galerie</span><span class="sxs-lookup"><span data-stu-id="eda51-328">Add a non-gallery application</span></span>](#add-a-non-gallery-application)

-   [<span data-ttu-id="eda51-329">Konfigurace aplikace hello pro heslo jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="eda51-329">Configure hello application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

### <a name="add-a-non-gallery-application"></a><span data-ttu-id="eda51-330">Přidání aplikace bez Galerie</span><span class="sxs-lookup"><span data-stu-id="eda51-330">Add a non-gallery application</span></span>

<span data-ttu-id="eda51-331">tooadd aplikace hello Galerie Azure AD, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="eda51-331">tooadd an application from hello Azure AD Gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="eda51-332">Otevřete hello [portálu Azure](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**</span><span class="sxs-lookup"><span data-stu-id="eda51-332">Open hello [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="eda51-333">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="eda51-333">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="eda51-334">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="eda51-334">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="eda51-335">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="eda51-335">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="eda51-336">Klikněte na tlačítko hello **přidat** tlačítko v pravém horním rohu hello na hello **podnikové aplikace, které** okno</span><span class="sxs-lookup"><span data-stu-id="eda51-336">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade</span></span>

6.  <span data-ttu-id="eda51-337">Klikněte na tlačítko **Galerie jiná aplikace.**</span><span class="sxs-lookup"><span data-stu-id="eda51-337">click **Non-gallery application.**</span></span>

7.  <span data-ttu-id="eda51-338">Zadejte název vaší aplikace hello v hello **název** textové pole.</span><span class="sxs-lookup"><span data-stu-id="eda51-338">Enter hello name of your application in hello **Name** textbox.</span></span> <span data-ttu-id="eda51-339">Vyberte **přidat.**</span><span class="sxs-lookup"><span data-stu-id="eda51-339">Select **Add.**</span></span>

<span data-ttu-id="eda51-340">Po krátké době být okno Konfigurace aplikací mít toosee hello.</span><span class="sxs-lookup"><span data-stu-id="eda51-340">After a short period, you be able toosee hello application’s configuration blade.</span></span>

### <a name="configure-hello-application-for-password-single-sign-on"></a><span data-ttu-id="eda51-341">Konfigurace aplikace hello pro heslo jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="eda51-341">Configure hello application for password single sign-on</span></span>

<span data-ttu-id="eda51-342">tooconfigure jednotné přihlašování pro aplikace, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="eda51-342">tooconfigure single sign-on for an application, follow hello steps below:</span></span>

1.  <span data-ttu-id="eda51-343">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="eda51-343">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="eda51-344">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="eda51-344">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="eda51-345">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="eda51-345">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="eda51-346">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="eda51-346">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="eda51-347">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="eda51-347">click **All Applications** tooview a list of all your applications.</span></span>

 * <span data-ttu-id="eda51-348">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="eda51-348">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="eda51-349">Vyberte hello aplikaci tooconfigure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="eda51-349">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="eda51-350">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="eda51-350">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="eda51-351">Vyberte hello režimu **založené na heslech přihlášení.**</span><span class="sxs-lookup"><span data-stu-id="eda51-351">Select hello mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="eda51-352">Zadejte hello **přihlašovací adresa URL**.</span><span class="sxs-lookup"><span data-stu-id="eda51-352">Enter hello **Sign-on URL**.</span></span> <span data-ttu-id="eda51-353">Toto je hello URL, kde uživatelé zadat toosign své uživatelské jméno a heslo v k.</span><span class="sxs-lookup"><span data-stu-id="eda51-353">This is hello URL where users enter their username and password toosign in to.</span></span> <span data-ttu-id="eda51-354">Zkontrolujte, zda jsou viditelné na adrese URL hello hello přihlášení pole.</span><span class="sxs-lookup"><span data-stu-id="eda51-354">Ensure hello sign in fields are visible at hello URL.</span></span>

10. <span data-ttu-id="eda51-355">Přiřazení uživatelů toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="eda51-355">Assign users toohello application.</span></span>

11. <span data-ttu-id="eda51-356">Kromě toho můžete taky zadat přihlašovací údaje jménem uživatele hello výběrem řádků hello hello uživatelů a kliknete na **pověření aktualizace** a zadáním jménem uživatelů hello hello uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="eda51-356">Additionally, you can also provide credentials on behalf of hello user by selecting hello rows of hello users and clicking on **Update Credentials** and entering hello username and password on behalf of hello users.</span></span> <span data-ttu-id="eda51-357">Uživatelé, jinak nebudou výzvami tooenter hello přihlašovací údaje sami při spuštění.</span><span class="sxs-lookup"><span data-stu-id="eda51-357">Otherwise, users be prompted tooenter hello credentials themselves upon launch.</span></span>

## <a name="how-tooassign-a-user-tooan-application-directly"></a><span data-ttu-id="eda51-358">Jak tooassign tooan aplikace uživatele přímo</span><span class="sxs-lookup"><span data-stu-id="eda51-358">How tooassign a user tooan application directly</span></span>

<span data-ttu-id="eda51-359">tooassign jeden nebo více uživatelů tooan aplikace přímo, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="eda51-359">tooassign one or more users tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="eda51-360">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="eda51-360">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="eda51-361">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="eda51-361">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="eda51-362">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="eda51-362">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="eda51-363">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="eda51-363">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="eda51-364">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="eda51-364">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="eda51-365">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="eda51-365">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="eda51-366">Vyberte aplikaci hello chcete tooassign seznam uživatele toofrom hello.</span><span class="sxs-lookup"><span data-stu-id="eda51-366">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="eda51-367">Po načtení hello aplikace, klikněte na **uživatelů a skupin** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="eda51-367">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="eda51-368">Klikněte na tlačítko hello **přidat** tlačítko nad hello **uživatelů a skupin** seznamu tooopen hello **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="eda51-368">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="eda51-369">Klikněte na tlačítko hello **uživatelů a skupin** selektor z hello **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="eda51-369">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="eda51-370">Typ v hello **celý název** nebo **e-mailová adresa** hello uživatele, které vás zajímají přiřazení do hello **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="eda51-370">Type in hello **full name** or **email address** of hello user you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="eda51-371">Pozastavte ukazatel myši nad hello **uživatele** v seznamu tooreveal hello **políčko**.</span><span class="sxs-lookup"><span data-stu-id="eda51-371">Hover over hello **user** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="eda51-372">Klikněte na profil hello políčko další toohello uživatele fotografie nebo logo tooadd vaše uživatele toohello **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="eda51-372">Click hello checkbox next toohello user’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="eda51-373">**Volitelné:** Pokud byste chtěli příliš**přidat více než jeden uživatel**, zadejte v jiném **celý název** nebo **e-mailová adresa** do hello **vyhledávání podle názvu nebo e-mailová adresa** pole pro vyhledávání a klikněte na zaškrtávací políčko tooadd hello tento uživatel toohello **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="eda51-373">**Optional:** If you would like too**add more than one user**, type in another **full name** or **email address** into hello **Search by name or email address** search box, and click hello checkbox tooadd this user toohello **Selected** list.</span></span>

13. <span data-ttu-id="eda51-374">Po dokončení výběru uživatelů klikněte na hello **vyberte** tlačítko tooadd je toohello seznam uživatelů a skupin toobe přiřazené toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="eda51-374">When you are finished selecting users, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="eda51-375">**Volitelné:** klikněte na tlačítko hello **vybrat roli** selektor v hello **přidat přiřazení** okno tooselect roli uživatele toohello tooassign jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="eda51-375">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello users you have selected.</span></span>

15. <span data-ttu-id="eda51-376">Klikněte na tlačítko hello **přiřadit** tlačítko tooassign hello aplikace toohello vybraných uživatelů.</span><span class="sxs-lookup"><span data-stu-id="eda51-376">Click hello **Assign** button tooassign hello application toohello selected users.</span></span>

<span data-ttu-id="eda51-377">Po krátké době hello uživatelů, které jste vybrali být schopný toolaunch tyto aplikace v hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="eda51-377">After a short period, hello users you have selected be able toolaunch these applications in hello Access Panel.</span></span>

## <a name="if-these-troubleshooting-steps-do-not-hello-resolve-hello-issue"></a><span data-ttu-id="eda51-378">Pokud tento postup řešení není hello hello problém vyřešte</span><span class="sxs-lookup"><span data-stu-id="eda51-378">If these troubleshooting steps do not hello resolve hello issue</span></span>

<span data-ttu-id="eda51-379">Otevřete lístek podpory se hello, pokud je k dispozici následující informace:</span><span class="sxs-lookup"><span data-stu-id="eda51-379">open a support ticket with hello following information if available:</span></span>

-   <span data-ttu-id="eda51-380">ID chyby korelace</span><span class="sxs-lookup"><span data-stu-id="eda51-380">Correlation error ID</span></span>

-   <span data-ttu-id="eda51-381">Hlavní název uživatele (e-mailovou adresu uživatele)</span><span class="sxs-lookup"><span data-stu-id="eda51-381">UPN (user email address)</span></span>

-   <span data-ttu-id="eda51-382">TenantID</span><span class="sxs-lookup"><span data-stu-id="eda51-382">TenantID</span></span>

-   <span data-ttu-id="eda51-383">Typ prohlížeče</span><span class="sxs-lookup"><span data-stu-id="eda51-383">Browser type</span></span>

-   <span data-ttu-id="eda51-384">Dojde k časové pásmo a čas nebo období při chybě</span><span class="sxs-lookup"><span data-stu-id="eda51-384">Time zone and time/timeframe during error occurs</span></span>

-   <span data-ttu-id="eda51-385">Fiddler trasování</span><span class="sxs-lookup"><span data-stu-id="eda51-385">Fiddler traces</span></span>

## <a name="next-steps"></a><span data-ttu-id="eda51-386">Další kroky</span><span class="sxs-lookup"><span data-stu-id="eda51-386">Next steps</span></span>
[<span data-ttu-id="eda51-387">Zadejte tooyour přihlašování aplikací pomocí Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="eda51-387">Provide single sign-on tooyour apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)

