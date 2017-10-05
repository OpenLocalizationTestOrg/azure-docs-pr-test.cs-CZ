---
title: "Potíže při přihlašování k aplikaci na přístupovém panelu | Microsoft Docs"
description: "Řešení potíží s problémy, přístup k aplikaci z Microsoft Azure AD přístupovému panelu na adrese myapps.microsoft.com"
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
ms.openlocfilehash: 188a00db59b0aa8d26facc678fb52d96272183b6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="problems-signing-in-to-an-application-from-the-access-panel"></a><span data-ttu-id="23c36-103">Potíže při přihlašování k aplikaci na přístupovém panelu</span><span class="sxs-lookup"><span data-stu-id="23c36-103">Problems signing in to an application from the access panel</span></span>

<span data-ttu-id="23c36-104">Přístupový Panel je webový portál, který umožňuje uživatele, který má pracovní nebo školní účet ve službě Azure Active Directory (Azure AD) k zobrazení a spustit cloudové aplikace, aby správce Azure AD má je přístup povolen.</span><span class="sxs-lookup"><span data-stu-id="23c36-104">The Access Panel is a web-based portal which enables a user with a work or school account in Azure Active Directory (Azure AD) to view and start cloud-based applications that the Azure AD administrator has granted them access to.</span></span> 

<span data-ttu-id="23c36-105">Tyto aplikace jsou konfigurovány jménem uživatele na portálu Azure AD.</span><span class="sxs-lookup"><span data-stu-id="23c36-105">These applications are configured on behalf of the user in the Azure AD portal.</span></span> <span data-ttu-id="23c36-106">Aplikace musí být správně nakonfigurovaná a přiřadit uživatele nebo skupiny, kterých je uživatel členem skupiny chcete vidět aplikaci na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="23c36-106">The application must be configured properly and assigned to the user or a group the user is a member of to see the application in the Access Panel.</span></span>

<span data-ttu-id="23c36-107">Typ aplikace, které zobrazuje uživatel možná spadají do následujících kategorií:</span><span class="sxs-lookup"><span data-stu-id="23c36-107">The type of apps a user may be seeing fall in the following categories:</span></span>

-   <span data-ttu-id="23c36-108">Aplikace Office 365</span><span class="sxs-lookup"><span data-stu-id="23c36-108">Office 365 Applications</span></span>

-   <span data-ttu-id="23c36-109">Společnost Microsoft a třetích stran aplikace, které jsou nakonfigurované pomocí jednotného přihlašování na základě federation</span><span class="sxs-lookup"><span data-stu-id="23c36-109">Microsoft and third-party applications configured with federation-based SSO</span></span>

-   <span data-ttu-id="23c36-110">Aplikace založené na heslech jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="23c36-110">Password-based SSO applications</span></span>

-   <span data-ttu-id="23c36-111">Aplikace s stávajících řešení pro jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="23c36-111">Applications with existing SSO solutions</span></span>

## <a name="general-issues-to-check-first"></a><span data-ttu-id="23c36-112">Běžné problémy a proveďte nejprve kontrolu</span><span class="sxs-lookup"><span data-stu-id="23c36-112">General issues to check first</span></span>

-   <span data-ttu-id="23c36-113">Zajistěte, aby vaše použití **prohlížeče** , splňuje minimální požadavky na přístupový Panel.</span><span class="sxs-lookup"><span data-stu-id="23c36-113">Make sure your using a **browser** that meets the minimum requirements for the Access Panel.</span></span>

-   <span data-ttu-id="23c36-114">Zajistěte, aby prohlížeče uživatele přidala adresu URL aplikace pro jeho **Důvěryhodné servery**.</span><span class="sxs-lookup"><span data-stu-id="23c36-114">Make sure the user’s browser has added the URL of the application to its **trusted sites**.</span></span>

-   <span data-ttu-id="23c36-115">Ujistěte se, zkontrolujte, že aplikace je **nakonfigurované** správně.</span><span class="sxs-lookup"><span data-stu-id="23c36-115">Make sure to check the application is **configured** correctly.</span></span>

-   <span data-ttu-id="23c36-116">Zkontrolujte, zda je účet uživatele **povoleno** pro přihlášení.</span><span class="sxs-lookup"><span data-stu-id="23c36-116">Make sure the user’s account is **enabled** for sign ins.</span></span>

-   <span data-ttu-id="23c36-117">Zkontrolujte, zda je účet uživatele **není uzamčen.**</span><span class="sxs-lookup"><span data-stu-id="23c36-117">Make sure the user’s account is **not locked out.**</span></span>

-   <span data-ttu-id="23c36-118">Zkontrolujte, že uživatele **není platnost nebo zapomenete heslo.**</span><span class="sxs-lookup"><span data-stu-id="23c36-118">Make sure the user’s **password is not expired or forgotten.**</span></span>

-   <span data-ttu-id="23c36-119">Zajistěte, aby **Multi-Factor Authentication** neblokuje přístup uživatelů.</span><span class="sxs-lookup"><span data-stu-id="23c36-119">Make sure **Multi-Factor Authentication** is not blocking user access.</span></span>

-   <span data-ttu-id="23c36-120">Zajistěte, aby **zásady podmíněného přístupu** nebo **Identity Protection** zásad neblokuje přístup uživatelů.</span><span class="sxs-lookup"><span data-stu-id="23c36-120">Make sure a **Conditional Access policy** or **Identity Protection** policy is not blocking user access.</span></span>

-   <span data-ttu-id="23c36-121">Ujistěte se, že uživatele **ověřování kontaktní údaje** je aktuální povolit Multi-Factor Authentication nebo podmíněného přístupu zásad, která budou vynucena.</span><span class="sxs-lookup"><span data-stu-id="23c36-121">Make sure that a user’s **authentication contact info** is up to date to allow Multi-Factor Authentication or Conditional Access policies to be enforced.</span></span>

-   <span data-ttu-id="23c36-122">Zkontrolujte, zda také zkuste vymazání souborů cookie v prohlížeči a zkusit se přihlásit znovu.</span><span class="sxs-lookup"><span data-stu-id="23c36-122">Make sure to also try clearing your browser’s cookies and trying to sign in again.</span></span>

## <a name="meeting-browser-requirements-for-the-access-panel"></a><span data-ttu-id="23c36-123">Splňuje požadavky na prohlížeč pro přístup k panelu</span><span class="sxs-lookup"><span data-stu-id="23c36-123">Meeting browser requirements for the Access Panel</span></span>

<span data-ttu-id="23c36-124">Přístupový Panel vyžaduje prohlížeč, který podporuje jazyk JavaScript a aktivoval šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="23c36-124">The Access Panel requires a browser that supports JavaScript and has CSS enabled.</span></span> <span data-ttu-id="23c36-125">Pokud chcete používat založené na heslech jednotné přihlašování (SSO) na přístupovém panelu, musí být nainstalované rozšíření přístupového panelu v prohlížeče uživatele.</span><span class="sxs-lookup"><span data-stu-id="23c36-125">To use password-based single sign-on (SSO) in the Access Panel, the Access Panel extension must be installed in the user’s browser.</span></span> <span data-ttu-id="23c36-126">Toto rozšíření je stažen automaticky, když uživatel vybere aplikaci, která je nakonfigurována pro jednotné přihlašování založené na heslech.</span><span class="sxs-lookup"><span data-stu-id="23c36-126">This extension is downloaded automatically when a user selects an application that is configured for password-based SSO.</span></span>

<span data-ttu-id="23c36-127">Pro jednotné přihlašování založené na heslech může být prohlížeče koncového uživatele:</span><span class="sxs-lookup"><span data-stu-id="23c36-127">For password-based SSO, the end user’s browsers can be:</span></span>

-   <span data-ttu-id="23c36-128">Internet Explorer 8, 9, 10, 11 – v systému Windows 7 nebo novější</span><span class="sxs-lookup"><span data-stu-id="23c36-128">Internet Explorer 8, 9, 10, 11 -- on Windows 7 or later</span></span>

-   <span data-ttu-id="23c36-129">Hraniční Windows 10 Anniversary Edition nebo novější.</span><span class="sxs-lookup"><span data-stu-id="23c36-129">Edge on Windows 10 Anniversary Edition or later</span></span>

-   <span data-ttu-id="23c36-130">Chrome – V systému Windows 7 nebo novější a v systému MacOS X nebo novější</span><span class="sxs-lookup"><span data-stu-id="23c36-130">Chrome -- on Windows 7 or later, and on MacOS X or later</span></span>

-   <span data-ttu-id="23c36-131">Firefox 26.0 nebo později – do systému Windows XP SP2 nebo novější a v systému Mac OS X 10,6 nebo novější</span><span class="sxs-lookup"><span data-stu-id="23c36-131">Firefox 26.0 or later -- on Windows XP SP2 or later, and on Mac OS X 10.6 or later</span></span>

## <a name="how-to-install-the-access-panel-browser-extension"></a><span data-ttu-id="23c36-132">Postup instalace rozšíření přístup k panelu prohlížeče</span><span class="sxs-lookup"><span data-stu-id="23c36-132">How to install the Access Panel Browser extension</span></span>

<span data-ttu-id="23c36-133">Chcete-li nainstalovat rozšíření přístup k panelu prohlížeče, postupujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="23c36-133">To install the Access Panel Browser extension, follow the steps below:</span></span>

1.  <span data-ttu-id="23c36-134">Otevřete [přístupový Panel](https://myapps.microsoft.com) v jednom z podporovaných prohlížečích a přihlaste se jako **uživatele** ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="23c36-134">Open the [Access Panel](https://myapps.microsoft.com) in one of the supported browsers and sign in as a **user** in your Azure AD.</span></span>

2.  <span data-ttu-id="23c36-135">Klikněte na tlačítko **SSO heslo aplikace** na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="23c36-135">Click a **password-SSO application** in the Access Panel.</span></span>

3.  <span data-ttu-id="23c36-136">V řádku, požádá o instalaci softwaru, vyberte **instalovat nyní**.</span><span class="sxs-lookup"><span data-stu-id="23c36-136">In the prompt asking to install the software, select **Install Now**.</span></span>

4.  <span data-ttu-id="23c36-137">Založené na prohlížeči budete přesměrováni na odkaz ke stažení.</span><span class="sxs-lookup"><span data-stu-id="23c36-137">Based on your browser you be directed to the download link.</span></span> <span data-ttu-id="23c36-138">**Přidat** rozšíření do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="23c36-138">**Add** the extension to your browser.</span></span>

5.  <span data-ttu-id="23c36-139">Pokud prohlížeč zobrazí dotaz, vyberte buď **povolit** nebo **povolit** rozšíření.</span><span class="sxs-lookup"><span data-stu-id="23c36-139">If your browser asks, select to either **Enable** or **Allow** the extension.</span></span>

6.  <span data-ttu-id="23c36-140">Po instalaci **restartujte** relaci prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="23c36-140">Once installed, **restart** your browser session.</span></span>

7.  <span data-ttu-id="23c36-141">Přihlaste se do přístupového panelu a zobrazit, pokud můžete **spusťte** aplikace heslo SSO</span><span class="sxs-lookup"><span data-stu-id="23c36-141">Sign in into the Access Panel and see if you can **launch** your password-SSO applications</span></span>

<span data-ttu-id="23c36-142">Rozšíření pro Chrome a hraniční může také stáhnout z přímé odkazy níže:</span><span class="sxs-lookup"><span data-stu-id="23c36-142">You may also download the extension for Chrome and Edge from the direct links below:</span></span>

-   [<span data-ttu-id="23c36-143">Rozšíření pro Chrome přístup panely</span><span class="sxs-lookup"><span data-stu-id="23c36-143">Chrome Access Panel Extension</span></span>](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [<span data-ttu-id="23c36-144">Rozšíření panelu přístup Edge</span><span class="sxs-lookup"><span data-stu-id="23c36-144">Edge Access Panel Extension</span></span>](https://www.microsoft.com/store/apps/9pc9sckkzk84)

## <a name="how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="23c36-145">Postup konfigurace federované jednotné přihlašování pro aplikaci Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="23c36-145">How to configure federated single sign-on for an Azure AD gallery application</span></span>

<span data-ttu-id="23c36-146">Všechny aplikace v galerii Azure AD povolit funkce Enterprise Single Sign-On má podrobný kurz k dispozici.</span><span class="sxs-lookup"><span data-stu-id="23c36-146">All application in the Azure AD gallery enabled with Enterprise Single Sign-On capability has a step-by-step tutorial available.</span></span> <span data-ttu-id="23c36-147">Dostanete [seznam kurzů k integraci aplikací SaaS v Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) podrobností podrobné pokyny.</span><span class="sxs-lookup"><span data-stu-id="23c36-147">You can access the [List of tutorials on how to integrate SaaS apps with Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) for a detail step-by-step guidance.</span></span>

<span data-ttu-id="23c36-148">Pro konfiguraci aplikace z Galerie Azure AD, které potřebujete:</span><span class="sxs-lookup"><span data-stu-id="23c36-148">To configure an application from the Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="23c36-149">Přidat aplikaci z Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="23c36-149">Add an application from the Azure AD gallery</span></span>](#add-an-application)

-   [<span data-ttu-id="23c36-150">Nakonfigurujte hodnoty metadata aplikace ve službě Azure AD (přihlašování adresu URL, identifikátor, adresa URL odpovědi)</span><span class="sxs-lookup"><span data-stu-id="23c36-150">Configure the application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="23c36-151">Vyberte uživatelský identifikátor a přidejte atributy uživatele k odeslání do aplikace</span><span class="sxs-lookup"><span data-stu-id="23c36-151">Select User Identifier and add user attributes to be sent to the application</span></span>](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [<span data-ttu-id="23c36-152">Načtení metadat Azure AD a certifikátů</span><span class="sxs-lookup"><span data-stu-id="23c36-152">Retrieve Azure AD metadata and certificate</span></span>](#download-the-azure-ad-metadata-or-certificate)

-   [<span data-ttu-id="23c36-153">Konfigurovat Azure AD metadata hodnoty v aplikaci (přihlašování adresu URL, vystavitele, adresy URL odhlašovací a certifikát)</span><span class="sxs-lookup"><span data-stu-id="23c36-153">Configure Azure AD metadata values in the application (Sign on URL, Issuer, Logout URL and certificate)</span></span>](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="23c36-154">Přiřazení uživatelů k aplikaci</span><span class="sxs-lookup"><span data-stu-id="23c36-154">Assign users to the application</span></span>](#assign-users-to-the-application)

### <a name="add-an-application-from-the-azure-ad-gallery"></a><span data-ttu-id="23c36-155">Přidat aplikaci z Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="23c36-155">Add an application from the Azure AD gallery</span></span>

<span data-ttu-id="23c36-156">Pokud chcete přidat aplikaci z Galerie Azure AD, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="23c36-156">To add an application from the Azure AD Gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="23c36-157">Otevřete [portálu Azure](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**</span><span class="sxs-lookup"><span data-stu-id="23c36-157">Open the [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="23c36-158">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="23c36-158">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="23c36-159">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="23c36-159">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="23c36-160">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="23c36-160">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="23c36-161">klikněte **přidat** v pravém horním rohu na tlačítko **podnikové aplikace, které** okno</span><span class="sxs-lookup"><span data-stu-id="23c36-161">click the **Add** button at the top-right corner on the **Enterprise Applications** blade</span></span>

6.  <span data-ttu-id="23c36-162">V **zadejte název** textbox z **přidat z Galerie** části, zadejte název aplikace.</span><span class="sxs-lookup"><span data-stu-id="23c36-162">In the **Enter a name** textbox from the **Add from the gallery** section, type the name of the application.</span></span>

7.  <span data-ttu-id="23c36-163">Vyberte aplikaci, kterou chcete nakonfigurovat pro jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="23c36-163">Select the application you want to configure for single sign-on.</span></span>

8.  <span data-ttu-id="23c36-164">Před přidáním aplikace, můžete změnit její název ze **název** textové pole.</span><span class="sxs-lookup"><span data-stu-id="23c36-164">Before adding the application, you can change its name from the **Name** textbox.</span></span>

9.  <span data-ttu-id="23c36-165">Klikněte na tlačítko **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="23c36-165">Click **Add** button, to add the application.</span></span>

<span data-ttu-id="23c36-166">Po krátké době nebudete moci zobrazit okno Konfigurace aplikace.</span><span class="sxs-lookup"><span data-stu-id="23c36-166">After a short period, you be able to see the application’s configuration blade.</span></span>

### <a name="configure-single-sign-on-for-an-application-from-the-azure-ad-gallery"></a><span data-ttu-id="23c36-167">Konfigurovat jednotné přihlašování pro aplikace z Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="23c36-167">Configure single sign-on for an application from the Azure AD gallery</span></span>

<span data-ttu-id="23c36-168">Pokud chcete konfigurovat jednotné přihlašování pro aplikace, postupujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="23c36-168">To configure single sign-on for an application, follow the steps below:</span></span>

1.  <span data-ttu-id="23c36-169"><span id="_Hlk477187909" class="anchor"><span id="_Hlk477001983" class="anchor"></span></span>Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="23c36-169"><span id="_Hlk477187909" class="anchor"><span id="_Hlk477001983" class="anchor"></span></span>Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="23c36-170">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="23c36-170">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="23c36-171">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="23c36-171">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="23c36-172">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="23c36-172">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="23c36-173">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="23c36-173">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="23c36-174">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="23c36-174">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="23c36-175">Vyberte aplikaci, kterou chcete konfigurovat jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="23c36-175">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="23c36-176">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="23c36-176">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="23c36-177">Vyberte **na základě SAML přihlašování** z **režimu** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="23c36-177">Select **SAML-based Sign-on** from the **Mode** dropdown.</span></span>

9.  <span data-ttu-id="23c36-178">Zadejte požadované hodnoty v **domény a adresy URL.**</span><span class="sxs-lookup"><span data-stu-id="23c36-178">Enter the required values in **Domain and URLs.**</span></span> <span data-ttu-id="23c36-179">Tyto hodnoty by měly získat od dodavatele aplikace.</span><span class="sxs-lookup"><span data-stu-id="23c36-179">You should get these values from the application vendor.</span></span>

   1. <span data-ttu-id="23c36-180">Chcete-li nakonfigurovat aplikaci jako spouštěná SP jednotného přihlašování adresa URL je požadovaná hodnota.</span><span class="sxs-lookup"><span data-stu-id="23c36-180">To configure the application as SP-initiated SSO, the Sign on URL it’s a required value.</span></span> <span data-ttu-id="23c36-181">Některé aplikace je identifikátor také požadovaná hodnota.</span><span class="sxs-lookup"><span data-stu-id="23c36-181">For some applications, the Identifier is also a required value.</span></span>

   2. <span data-ttu-id="23c36-182">Chcete-li nakonfigurovat aplikaci jako spouštěná IdP SSO adresa URL odpovědi je požadovaná hodnota.</span><span class="sxs-lookup"><span data-stu-id="23c36-182">To configure the application as IdP-initiated SSO, the Reply URL it’s a required value.</span></span> <span data-ttu-id="23c36-183">Některé aplikace je identifikátor také požadovaná hodnota.</span><span class="sxs-lookup"><span data-stu-id="23c36-183">For some applications, the Identifier is also a required value.</span></span>

10. <span data-ttu-id="23c36-184">**Volitelné:** klikněte na tlačítko **zobrazit upřesňující nastavení adresy URL** Pokud budete chtít prohlédnout-požadované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="23c36-184">**Optional:** click **Show advanced URL settings** if you want to see the non-required values.</span></span>

11. <span data-ttu-id="23c36-185">V **uživatelské atributy**, vyberte jedinečný identifikátor pro uživatele v **uživatelský identifikátor** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="23c36-185">In the **User attributes**, select the unique identifier for your users in the **User Identifier** dropdown.</span></span>

12. <span data-ttu-id="23c36-186">**Volitelné:** klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** upravit atributy, které se při přihlášení uživatele odeslat do aplikace v tokenu SAML.</span><span class="sxs-lookup"><span data-stu-id="23c36-186">**Optional:** click **View and edit all other user attributes** to edit the attributes to be sent to the application in the SAML token when user sign in.</span></span>

   <span data-ttu-id="23c36-187">Chcete-li přidat atribut:</span><span class="sxs-lookup"><span data-stu-id="23c36-187">To add an attribute:</span></span>

   1. <span data-ttu-id="23c36-188">Klikněte na tlačítko **přidat atribut**.</span><span class="sxs-lookup"><span data-stu-id="23c36-188">click **Add attribute**.</span></span> <span data-ttu-id="23c36-189">Zadejte **název** a vyberte položku **hodnotu** z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="23c36-189">Enter the **Name** and the select the **Value** from the dropdown.</span></span>

   2. <span data-ttu-id="23c36-190">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="23c36-190">Click **Save.**</span></span> <span data-ttu-id="23c36-191">Zobrazí nový atribut v tabulce.</span><span class="sxs-lookup"><span data-stu-id="23c36-191">You see the new attribute in the table.</span></span>

13. <span data-ttu-id="23c36-192">Klikněte na tlačítko **konfigurace &lt;název aplikace&gt;**  dokumentace přístupu na tom, jak nakonfigurovat jednotné přihlašování v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="23c36-192">click **Configure &lt;application name&gt;** to access documentation on how to configure single sign-on in the application.</span></span> <span data-ttu-id="23c36-193">Navíc má adres URL metadat a certifikát nutný k nastavení jednotného přihlašování k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="23c36-193">Also, you has the metadata URLs and certificate required to setup SSO with the application.</span></span>

14. <span data-ttu-id="23c36-194">Klikněte na tlačítko **Uložit** konfiguraci uložíte.</span><span class="sxs-lookup"><span data-stu-id="23c36-194">Click **Save** to save the configuration.</span></span>

15. <span data-ttu-id="23c36-195">Přiřazení uživatelů k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="23c36-195">Assign users to the application.</span></span>

### <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a><span data-ttu-id="23c36-196">Vyberte uživatelský identifikátor a přidejte atributy uživatele k odeslání do aplikace</span><span class="sxs-lookup"><span data-stu-id="23c36-196">Select User Identifier and add user attributes to be sent to the application</span></span>

<span data-ttu-id="23c36-197">K výběru identifikátor uživatele nebo přidat atributy uživatele, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="23c36-197">To select the User Identifier or add user attributes, follow the steps below:</span></span>

1.  <span data-ttu-id="23c36-198">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="23c36-198">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="23c36-199">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="23c36-199">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="23c36-200">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="23c36-200">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="23c36-201">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="23c36-201">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="23c36-202">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="23c36-202">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="23c36-203">Pokud nevidíte aplikaci chcete zde zobrazí, použijte **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="23c36-203">If you do not see the application you want to show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="23c36-204">Vyberte aplikaci, které jste nakonfigurovali jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="23c36-204">Select the application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="23c36-205">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="23c36-205">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="23c36-206">V části **uživatelské atributy** vyberte jedinečný identifikátor pro uživatele v **uživatelský identifikátor** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="23c36-206">Under the **User attributes** section, select the unique identifier for your users in the **User Identifier** dropdown.</span></span> <span data-ttu-id="23c36-207">Vybraná možnost musí odpovídat očekávaná hodnota v aplikaci k ověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="23c36-207">The selected option needs to match the expected value in the application to authenticate the user.</span></span>

    >[!NOTE]
    ><span data-ttu-id="23c36-208">Azure AD vyberte formát pro atribut NameID (identifikátor uživatele) na základě hodnoty vybraných nebo formát požadovanou aplikaci v SAML AuthRequest.</span><span class="sxs-lookup"><span data-stu-id="23c36-208">Azure AD select the format for the NameID attribute (User Identifier) based on the value selected or the format requested by the application in the SAML AuthRequest.</span></span> <span data-ttu-id="23c36-209">Další informace najdete v článku [protokolu SAML přihlašování](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) části NameIDPolicy.</span><span class="sxs-lookup"><span data-stu-id="23c36-209">For more information visit the article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under the section NameIDPolicy.</span></span>
    >
    >

9.  <span data-ttu-id="23c36-210">Chcete-li přidat atributy uživatele, klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** upravit atributy, které se při přihlášení uživatele odeslat do aplikace v tokenu SAML.</span><span class="sxs-lookup"><span data-stu-id="23c36-210">To add user attributes, click **View and edit all other user attributes** to edit the attributes to be sent to the application in the SAML token when user sign in.</span></span>

   <span data-ttu-id="23c36-211">Chcete-li přidat atribut:</span><span class="sxs-lookup"><span data-stu-id="23c36-211">To add an attribute:</span></span>

   1. <span data-ttu-id="23c36-212">Klikněte na tlačítko **přidat atribut**.</span><span class="sxs-lookup"><span data-stu-id="23c36-212">click **Add attribute**.</span></span> <span data-ttu-id="23c36-213">Zadejte **název** a vyberte položku **hodnotu** z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="23c36-213">Enter the **Name** and the select the **Value** from the dropdown.</span></span>

   2. <span data-ttu-id="23c36-214">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="23c36-214">Click **Save.**</span></span> <span data-ttu-id="23c36-215">Zobrazí nový atribut v tabulce.</span><span class="sxs-lookup"><span data-stu-id="23c36-215">You see the new attribute in the table.</span></span>

### <a name="download-the-azure-ad-metadata-or-certificate"></a><span data-ttu-id="23c36-216">Stáhnout Azure AD metadata nebo certifikát</span><span class="sxs-lookup"><span data-stu-id="23c36-216">Download the Azure AD metadata or certificate</span></span>

<span data-ttu-id="23c36-217">Chcete-li stáhnout metadata aplikace nebo certifikát z Azure AD, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="23c36-217">To download the application metadata or certificate from Azure AD, follow the steps below:</span></span>

1.  <span data-ttu-id="23c36-218">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="23c36-218">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="23c36-219">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="23c36-219">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="23c36-220">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="23c36-220">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="23c36-221">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="23c36-221">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="23c36-222">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="23c36-222">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="23c36-223">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="23c36-223">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="23c36-224">Vyberte aplikaci, které jste nakonfigurovali jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="23c36-224">Select the application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="23c36-225">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="23c36-225">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="23c36-226">Přejděte na **SAML podpisový certifikát** části a pak klikněte na **Stáhnout** hodnota sloupce.</span><span class="sxs-lookup"><span data-stu-id="23c36-226">Go to **SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="23c36-227">V závislosti na tom, jaké aplikace vyžaduje konfiguraci jednotné přihlašování uvidíte buď možnost Stáhnout soubor XML s metadaty nebo certifikát.</span><span class="sxs-lookup"><span data-stu-id="23c36-227">Depending on what the application requires configuring single sign-on, you see either the option to download the Metadata XML or the Certificate.</span></span>

    <span data-ttu-id="23c36-228">Azure AD neposkytuje adresu URL se získat metadata.</span><span class="sxs-lookup"><span data-stu-id="23c36-228">Azure AD doesn’t provide a URL to get the metadata.</span></span> <span data-ttu-id="23c36-229">Metadata mohou být načteny pouze jako soubor XML.</span><span class="sxs-lookup"><span data-stu-id="23c36-229">The metadata can only be retrieved as a XML file.</span></span>

## <a name="how-to-configure-federated-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="23c36-230">Postup konfigurace federované jednotné přihlašování pro aplikace bez Galerie</span><span class="sxs-lookup"><span data-stu-id="23c36-230">How to configure federated single sign-on for a non-gallery application</span></span>

<span data-ttu-id="23c36-231">Konfigurace aplikace bez galerie, potřebujete Azure AD premium a aplikace podporuje SAML 2.0.</span><span class="sxs-lookup"><span data-stu-id="23c36-231">To configure a non-gallery application, you need to have Azure AD premium and the application supports SAML 2.0.</span></span> <span data-ttu-id="23c36-232">Další informace o verzích služby Azure AD, najdete v článku [ceny služby Azure AD](https://azure.microsoft.com/pricing/details/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="23c36-232">For more information about Azure AD versions, visit [Azure AD pricing](https://azure.microsoft.com/pricing/details/active-directory/).</span></span>

-   [<span data-ttu-id="23c36-233">Nakonfigurujte hodnoty metadata aplikace ve službě Azure AD (přihlašování adresu URL, identifikátor, adresa URL odpovědi)</span><span class="sxs-lookup"><span data-stu-id="23c36-233">Configure the application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>](#configuring-single-sign-on)

-   [<span data-ttu-id="23c36-234">Vyberte uživatelský identifikátor a přidejte atributy uživatele k odeslání do aplikace</span><span class="sxs-lookup"><span data-stu-id="23c36-234">Select User Identifier and add user attributes to be sent to the application</span></span>](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [<span data-ttu-id="23c36-235">Načtení metadat Azure AD a certifikátů</span><span class="sxs-lookup"><span data-stu-id="23c36-235">Retrieve Azure AD metadata and certificate</span></span>](#download-the-azure-ad-metadata-or-certificate)

-   [<span data-ttu-id="23c36-236">Konfigurovat Azure AD metadata hodnoty v aplikaci (přihlašování adresu URL, vystavitele, adresy URL odhlašovací a certifikát)</span><span class="sxs-lookup"><span data-stu-id="23c36-236">Configure Azure AD metadata values in the application (Sign on URL, Issuer, Logout URL and certificate)</span></span>](#configuring-single-sign-on)

### <a name="configure-the-applications-metadata-values-in-azure-ad-sign-on-url-identifier-reply-url"></a><span data-ttu-id="23c36-237">Nakonfigurujte hodnoty metadata aplikace ve službě Azure AD (přihlašování adresu URL, identifikátor, adresa URL odpovědi)</span><span class="sxs-lookup"><span data-stu-id="23c36-237">Configure the application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>

<span data-ttu-id="23c36-238">Pokud chcete konfigurovat jednotné přihlašování pro aplikace, která není v galerii Azure AD, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="23c36-238">To configure single sign-on for an application that is not in the Azure AD gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="23c36-239">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="23c36-239">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="23c36-240">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="23c36-240">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="23c36-241">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="23c36-241">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="23c36-242">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="23c36-242">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="23c36-243">klikněte **přidat** v pravém horním rohu na tlačítko **podnikové aplikace, které** okno</span><span class="sxs-lookup"><span data-stu-id="23c36-243">click the **Add** button at the top-right corner on the **Enterprise Applications** blade</span></span>

6.  <span data-ttu-id="23c36-244">Klikněte na tlačítko **aplikace bez Galerie** v **přidat vlastní aplikaci** části</span><span class="sxs-lookup"><span data-stu-id="23c36-244">click **Non-gallery application** in the **Add your own app** section</span></span>

7.  <span data-ttu-id="23c36-245">Zadejte název aplikace **název** textové pole.</span><span class="sxs-lookup"><span data-stu-id="23c36-245">Enter the name of the application in the **Name** textbox.</span></span>

8.  <span data-ttu-id="23c36-246">Klikněte na tlačítko **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="23c36-246">Click **Add** button, to add the application.</span></span>

9.  <span data-ttu-id="23c36-247">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="23c36-247">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

10. <span data-ttu-id="23c36-248">Vyberte **na základě SAML přihlašování** v **režimu** rozevíracího seznamu</span><span class="sxs-lookup"><span data-stu-id="23c36-248">Select **SAML-based Sign-on** in the **Mode** dropdown</span></span>

11. <span data-ttu-id="23c36-249">Zadejte požadované hodnoty v **domény a adresy URL.**</span><span class="sxs-lookup"><span data-stu-id="23c36-249">Enter the required values in **Domain and URLs.**</span></span> <span data-ttu-id="23c36-250">Tyto hodnoty by měly získat od dodavatele aplikace.</span><span class="sxs-lookup"><span data-stu-id="23c36-250">You should get these values from the application vendor.</span></span>

  1. <span data-ttu-id="23c36-251">Chcete-li nakonfigurovat aplikaci jako jednotné přihlašování iniciované deklarací identity, zadejte adresa URL odpovědi a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="23c36-251">To configure the application as IdP-initiated SSO, enter the Reply URL and the Identifier.</span></span>

  2. <span data-ttu-id="23c36-252">**Volitelné:** konfigurace aplikace jako spouštěná SP jednotného přihlašování adresa URL je požadovaná hodnota.</span><span class="sxs-lookup"><span data-stu-id="23c36-252">**Optional:** To configure the application as SP-initiated SSO, the Sign on URL it’s a required value.</span></span>

12. <span data-ttu-id="23c36-253">V **uživatelské atributy**, vyberte jedinečný identifikátor pro uživatele v **uživatelský identifikátor** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="23c36-253">In the **User attributes**, select the unique identifier for your users in the **User Identifier** dropdown.</span></span>

13. <span data-ttu-id="23c36-254">**Volitelné:** klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** upravit atributy, které se při přihlášení uživatele odeslat do aplikace v tokenu SAML.</span><span class="sxs-lookup"><span data-stu-id="23c36-254">**Optional:** click **View and edit all other user attributes** to edit the attributes to be sent to the application in the SAML token when user sign in.</span></span>

   <span data-ttu-id="23c36-255">Chcete-li přidat atribut:</span><span class="sxs-lookup"><span data-stu-id="23c36-255">To add an attribute:</span></span>

   1. <span data-ttu-id="23c36-256">Klikněte na tlačítko **přidat atribut**.</span><span class="sxs-lookup"><span data-stu-id="23c36-256">click **Add attribute**.</span></span> <span data-ttu-id="23c36-257">Zadejte **název** a vyberte položku **hodnotu** z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="23c36-257">Enter the **Name** and the select the **Value** from the dropdown.</span></span>

   2. <span data-ttu-id="23c36-258">Klikněte na tlačítko **uložit.**</span><span class="sxs-lookup"><span data-stu-id="23c36-258">Click **Save.**</span></span> <span data-ttu-id="23c36-259">Zobrazí nový atribut v tabulce.</span><span class="sxs-lookup"><span data-stu-id="23c36-259">You see the new attribute in the table.</span></span>

14. <span data-ttu-id="23c36-260">Klikněte na tlačítko **konfigurace &lt;název aplikace&gt;**  dokumentace přístupu na tom, jak nakonfigurovat jednotné přihlašování v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="23c36-260">click **Configure &lt;application name&gt;** to access documentation on how to configure single sign-on in the application.</span></span> <span data-ttu-id="23c36-261">Navíc má Azure AD adresy URL a certifikát nutný pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="23c36-261">Also, you has Azure AD URLs and certificate required for the application.</span></span>

### <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a><span data-ttu-id="23c36-262">Vyberte uživatelský identifikátor a přidejte atributy uživatele k odeslání do aplikace</span><span class="sxs-lookup"><span data-stu-id="23c36-262">Select User Identifier and add user attributes to be sent to the application</span></span>

<span data-ttu-id="23c36-263">K výběru identifikátor uživatele nebo přidat atributy uživatele, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="23c36-263">To select the User Identifier or add user attributes, follow the steps below:</span></span>

1.  <span data-ttu-id="23c36-264">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="23c36-264">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="23c36-265">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="23c36-265">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="23c36-266">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="23c36-266">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="23c36-267">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="23c36-267">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="23c36-268">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="23c36-268">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="23c36-269">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="23c36-269">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="23c36-270">Vyberte aplikaci, které jste nakonfigurovali jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="23c36-270">Select the application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="23c36-271">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="23c36-271">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="23c36-272">V části **uživatelské atributy** vyberte jedinečný identifikátor pro uživatele v **uživatelský identifikátor** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="23c36-272">Under the **User attributes** section, select the unique identifier for your users in the **User Identifier** dropdown.</span></span> <span data-ttu-id="23c36-273">Vybraná možnost musí odpovídat očekávaná hodnota v aplikaci k ověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="23c36-273">The selected option needs to match the expected value in the application to authenticate the user.</span></span>

   >[!NOTE]
   ><span data-ttu-id="23c36-274">Azure AD vyberte formát pro atribut NameID (identifikátor uživatele) na základě hodnoty vybraných nebo formát požadovanou aplikaci v SAML AuthRequest.</span><span class="sxs-lookup"><span data-stu-id="23c36-274">Azure AD select the format for the NameID attribute (User Identifier) based on the value selected or the format requested by the application in the SAML AuthRequest.</span></span> <span data-ttu-id="23c36-275">Další informace najdete v článku [protokolu SAML přihlašování](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) části NameIDPolicy.</span><span class="sxs-lookup"><span data-stu-id="23c36-275">For more information visit the article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under the section NameIDPolicy.</span></span>
   >
   >

9.  <span data-ttu-id="23c36-276">Chcete-li přidat atributy uživatele, klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** upravit atributy, které se při přihlášení uživatele odeslat do aplikace v tokenu SAML.</span><span class="sxs-lookup"><span data-stu-id="23c36-276">To add user attributes, click **View and edit all other user attributes** to edit the attributes to be sent to the application in the SAML token when user sign in.</span></span>

   <span data-ttu-id="23c36-277">Chcete-li přidat atribut:</span><span class="sxs-lookup"><span data-stu-id="23c36-277">To add an attribute:</span></span>

   <span data-ttu-id="23c36-278">1. klikněte na tlačítko **přidat atribut**.</span><span class="sxs-lookup"><span data-stu-id="23c36-278">1.click **Add attribute**.</span></span> <span data-ttu-id="23c36-279">Zadejte **název** a vyberte položku **hodnotu** z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="23c36-279">Enter the **Name** and the select the **Value** from the dropdown.</span></span>

   <span data-ttu-id="23c36-280">Klikněte na 2 **uložit.**</span><span class="sxs-lookup"><span data-stu-id="23c36-280">2 Click **Save.**</span></span> <span data-ttu-id="23c36-281">Zobrazí nový atribut v tabulce.</span><span class="sxs-lookup"><span data-stu-id="23c36-281">You see the new attribute in the table.</span></span>

### <a name="download-the-azure-ad-metadata-or-certificate"></a><span data-ttu-id="23c36-282">Stáhnout Azure AD metadata nebo certifikát</span><span class="sxs-lookup"><span data-stu-id="23c36-282">Download the Azure AD metadata or certificate</span></span>

<span data-ttu-id="23c36-283">Chcete-li stáhnout metadata aplikace nebo certifikát z Azure AD, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="23c36-283">To download the application metadata or certificate from Azure AD, follow the steps below:</span></span>

1.  <span data-ttu-id="23c36-284">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="23c36-284">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="23c36-285">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="23c36-285">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="23c36-286">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="23c36-286">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="23c36-287">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="23c36-287">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="23c36-288">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="23c36-288">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="23c36-289">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="23c36-289">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="23c36-290">Vyberte aplikaci, které jste nakonfigurovali jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="23c36-290">Select the application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="23c36-291">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="23c36-291">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="23c36-292">Přejděte na **SAML podpisový certifikát** části a pak klikněte na **Stáhnout** hodnota sloupce.</span><span class="sxs-lookup"><span data-stu-id="23c36-292">Go to **SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="23c36-293">V závislosti na tom, jaké aplikace vyžaduje konfiguraci jednotné přihlašování uvidíte buď možnost Stáhnout soubor XML s metadaty nebo certifikát.</span><span class="sxs-lookup"><span data-stu-id="23c36-293">Depending on what the application requires configuring single sign-on, you see either the option to download the Metadata XML or the Certificate.</span></span>

    <span data-ttu-id="23c36-294">Azure AD neposkytuje adresu URL se získat metadata.</span><span class="sxs-lookup"><span data-stu-id="23c36-294">Azure AD doesn’t provide a URL to get the metadata.</span></span> <span data-ttu-id="23c36-295">Metadata mohou být načteny pouze jako soubor XML.</span><span class="sxs-lookup"><span data-stu-id="23c36-295">The metadata can only be retrieved as a XML file.</span></span>

## <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="23c36-296">Postup konfigurace hesel jednotné přihlašování pro aplikaci Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="23c36-296">How to configure password single sign-on for an Azure AD gallery application</span></span>

<span data-ttu-id="23c36-297">Pro konfiguraci aplikace z Galerie Azure AD, které potřebujete:</span><span class="sxs-lookup"><span data-stu-id="23c36-297">To configure an application from the Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="23c36-298">Přidat aplikaci z Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="23c36-298">Add an application from the Azure AD gallery</span></span>](#add-an-application)

-   [<span data-ttu-id="23c36-299">Konfigurace aplikace pro heslo jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="23c36-299">Configure the application for password single sign-on</span></span>](#configure-the-application)

### <a name="add-an-application-from-the-azure-ad-gallery"></a><span data-ttu-id="23c36-300">Přidat aplikaci z Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="23c36-300">Add an application from the Azure AD gallery</span></span>

<span data-ttu-id="23c36-301">Pokud chcete přidat aplikaci z Galerie Azure AD, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="23c36-301">To add an application from the Azure AD Gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="23c36-302">Otevřete [portálu Azure](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**</span><span class="sxs-lookup"><span data-stu-id="23c36-302">Open the [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="23c36-303">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="23c36-303">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="23c36-304">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="23c36-304">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="23c36-305">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="23c36-305">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="23c36-306">klikněte **přidat** v pravém horním rohu na tlačítko **podnikové aplikace, které** okno</span><span class="sxs-lookup"><span data-stu-id="23c36-306">click the **Add** button at the top-right corner on the **Enterprise Applications** blade</span></span>

6.  <span data-ttu-id="23c36-307">V **zadejte název** textbox z **přidat z Galerie** části, zadejte název aplikace</span><span class="sxs-lookup"><span data-stu-id="23c36-307">In the **Enter a name** textbox from the **Add from the gallery** section, type the name of the application</span></span>

7.  <span data-ttu-id="23c36-308">Vyberte aplikaci, kterou chcete nakonfigurovat pro jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="23c36-308">Select the application you want to configure for single sign-on</span></span>

8.  <span data-ttu-id="23c36-309">Před přidáním aplikace, můžete změnit její název ze **název** textové pole.</span><span class="sxs-lookup"><span data-stu-id="23c36-309">Before adding the application, you can change its name from the **Name** textbox.</span></span>

9.  <span data-ttu-id="23c36-310">Klikněte na tlačítko **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="23c36-310">Click **Add** button, to add the application.</span></span>

<span data-ttu-id="23c36-311">Po krátké době nebudete moci zobrazit okno Konfigurace aplikace.</span><span class="sxs-lookup"><span data-stu-id="23c36-311">After a short period, you be able to see the application’s configuration blade.</span></span>

### <a name="configure-the-application-for-password-single-sign-on"></a><span data-ttu-id="23c36-312">Konfigurace aplikace pro heslo jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="23c36-312">Configure the application for password single sign-on</span></span>

<span data-ttu-id="23c36-313">Pokud chcete konfigurovat jednotné přihlašování pro aplikace, postupujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="23c36-313">To configure single sign-on for an application, follow the steps below:</span></span>

1.  <span data-ttu-id="23c36-314">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="23c36-314">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="23c36-315">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="23c36-315">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="23c36-316">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="23c36-316">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="23c36-317">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="23c36-317">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="23c36-318">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="23c36-318">click **All Applications** to view a list of all your applications.</span></span>

 * <span data-ttu-id="23c36-319">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="23c36-319">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="23c36-320">Vyberte aplikaci, kterou chcete konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="23c36-320">Select the application you want to configure single sign-on</span></span>

7.  <span data-ttu-id="23c36-321">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="23c36-321">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="23c36-322">Vyberte režim **založené na heslech přihlášení.**</span><span class="sxs-lookup"><span data-stu-id="23c36-322">Select the mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="23c36-323">Přiřazení uživatelů k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="23c36-323">Assign users to the application.</span></span>

10. <span data-ttu-id="23c36-324">Kromě toho můžete taky zadat přihlašovací údaje jménem uživatele výběrem řádky uživatelů a kliknete na **pověření aktualizace** a zadání uživatelského jména a hesla jménem uživatele.</span><span class="sxs-lookup"><span data-stu-id="23c36-324">Additionally, you can also provide credentials on behalf of the user by selecting the rows of the users and clicking on **Update Credentials** and entering the username and password on behalf of the users.</span></span> <span data-ttu-id="23c36-325">Uživatelé, jinak se výzva k zadání sami přihlašovací údaje při spuštění.</span><span class="sxs-lookup"><span data-stu-id="23c36-325">Otherwise, users be prompted to enter the credentials themselves upon launch.</span></span>

## <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="23c36-326">Postup konfigurace hesel jednotné přihlašování pro aplikace bez Galerie</span><span class="sxs-lookup"><span data-stu-id="23c36-326">How to configure password single sign-on for a non-gallery application</span></span>

<span data-ttu-id="23c36-327">Pro konfiguraci aplikace z Galerie Azure AD, které potřebujete:</span><span class="sxs-lookup"><span data-stu-id="23c36-327">To configure an application from the Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="23c36-328">Přidání aplikace bez Galerie</span><span class="sxs-lookup"><span data-stu-id="23c36-328">Add a non-gallery application</span></span>](#add-a-non-gallery-application)

-   [<span data-ttu-id="23c36-329">Konfigurace aplikace pro heslo jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="23c36-329">Configure the application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

### <a name="add-a-non-gallery-application"></a><span data-ttu-id="23c36-330">Přidání aplikace bez Galerie</span><span class="sxs-lookup"><span data-stu-id="23c36-330">Add a non-gallery application</span></span>

<span data-ttu-id="23c36-331">Pokud chcete přidat aplikaci z Galerie Azure AD, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="23c36-331">To add an application from the Azure AD Gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="23c36-332">Otevřete [portálu Azure](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**</span><span class="sxs-lookup"><span data-stu-id="23c36-332">Open the [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="23c36-333">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="23c36-333">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="23c36-334">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="23c36-334">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="23c36-335">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="23c36-335">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="23c36-336">klikněte **přidat** v pravém horním rohu na tlačítko **podnikové aplikace, které** okno</span><span class="sxs-lookup"><span data-stu-id="23c36-336">click the **Add** button at the top-right corner on the **Enterprise Applications** blade</span></span>

6.  <span data-ttu-id="23c36-337">Klikněte na tlačítko **Galerie jiná aplikace.**</span><span class="sxs-lookup"><span data-stu-id="23c36-337">click **Non-gallery application.**</span></span>

7.  <span data-ttu-id="23c36-338">Zadejte název vaší aplikace v **název** textové pole.</span><span class="sxs-lookup"><span data-stu-id="23c36-338">Enter the name of your application in the **Name** textbox.</span></span> <span data-ttu-id="23c36-339">Vyberte **přidat.**</span><span class="sxs-lookup"><span data-stu-id="23c36-339">Select **Add.**</span></span>

<span data-ttu-id="23c36-340">Po krátké době nebudete moci zobrazit okno Konfigurace aplikace.</span><span class="sxs-lookup"><span data-stu-id="23c36-340">After a short period, you be able to see the application’s configuration blade.</span></span>

### <a name="configure-the-application-for-password-single-sign-on"></a><span data-ttu-id="23c36-341">Konfigurace aplikace pro heslo jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="23c36-341">Configure the application for password single sign-on</span></span>

<span data-ttu-id="23c36-342">Pokud chcete konfigurovat jednotné přihlašování pro aplikace, postupujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="23c36-342">To configure single sign-on for an application, follow the steps below:</span></span>

1.  <span data-ttu-id="23c36-343">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="23c36-343">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="23c36-344">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="23c36-344">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="23c36-345">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="23c36-345">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="23c36-346">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="23c36-346">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="23c36-347">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="23c36-347">click **All Applications** to view a list of all your applications.</span></span>

 * <span data-ttu-id="23c36-348">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="23c36-348">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="23c36-349">Vyberte aplikaci, kterou chcete konfigurovat jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="23c36-349">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="23c36-350">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="23c36-350">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="23c36-351">Vyberte režim **založené na heslech přihlášení.**</span><span class="sxs-lookup"><span data-stu-id="23c36-351">Select the mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="23c36-352">Zadejte **přihlašovací adresa URL**.</span><span class="sxs-lookup"><span data-stu-id="23c36-352">Enter the **Sign-on URL**.</span></span> <span data-ttu-id="23c36-353">Toto je adresa URL, kde uživatelé zadat svoje uživatelské jméno a heslo pro přihlášení k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="23c36-353">This is the URL where users enter their username and password to sign in to.</span></span> <span data-ttu-id="23c36-354">Zkontrolujte, zda že přihlašovací pole jsou viditelné na adrese URL.</span><span class="sxs-lookup"><span data-stu-id="23c36-354">Ensure the sign in fields are visible at the URL.</span></span>

10. <span data-ttu-id="23c36-355">Přiřazení uživatelů k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="23c36-355">Assign users to the application.</span></span>

11. <span data-ttu-id="23c36-356">Kromě toho můžete taky zadat přihlašovací údaje jménem uživatele výběrem řádky uživatelů a kliknete na **pověření aktualizace** a zadání uživatelského jména a hesla jménem uživatele.</span><span class="sxs-lookup"><span data-stu-id="23c36-356">Additionally, you can also provide credentials on behalf of the user by selecting the rows of the users and clicking on **Update Credentials** and entering the username and password on behalf of the users.</span></span> <span data-ttu-id="23c36-357">Uživatelé, jinak se výzva k zadání sami přihlašovací údaje při spuštění.</span><span class="sxs-lookup"><span data-stu-id="23c36-357">Otherwise, users be prompted to enter the credentials themselves upon launch.</span></span>

## <a name="how-to-assign-a-user-to-an-application-directly"></a><span data-ttu-id="23c36-358">Postup přiřazení uživatele k aplikaci přímo</span><span class="sxs-lookup"><span data-stu-id="23c36-358">How to assign a user to an application directly</span></span>

<span data-ttu-id="23c36-359">Jeden nebo více uživatelů přiřadit přímo k aplikaci, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="23c36-359">To assign one or more users to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="23c36-360">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="23c36-360">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="23c36-361">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="23c36-361">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="23c36-362">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="23c36-362">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="23c36-363">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="23c36-363">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="23c36-364">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="23c36-364">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="23c36-365">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="23c36-365">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="23c36-366">Vyberte aplikaci, kterou chcete přiřadit uživatele k ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="23c36-366">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="23c36-367">Po načtení aplikace, klikněte na **uživatelů a skupin** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="23c36-367">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="23c36-368">Klikněte na tlačítko **přidat** tlačítko na **uživatelů a skupin** seznamu otevřete **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="23c36-368">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="23c36-369">Klikněte na tlačítko **uživatelů a skupin** pro výběr **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="23c36-369">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="23c36-370">Zadejte **celý název** nebo **e-mailová adresa** uživatele vás zajímá přiřazení do **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="23c36-370">Type in the **full name** or **email address** of the user you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="23c36-371">Najeďte myší **uživatele** v seznamu na nich **políčko**.</span><span class="sxs-lookup"><span data-stu-id="23c36-371">Hover over the **user** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="23c36-372">Klikněte na zaškrtávací políčko vedle profilové fotky nebo logo pro přidání uživatelů do uživatele **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="23c36-372">Click the checkbox next to the user’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="23c36-373">**Volitelné:** Pokud byste chtěli **přidat více než jeden uživatel**, typ v jiném **celý název** nebo **e-mailová adresa** do **hledat podle jména nebo e-mailové adresy** pole pro vyhledávání a klikněte na zaškrtávací políčko, chcete-li přidat tento uživatel **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="23c36-373">**Optional:** If you would like to **add more than one user**, type in another **full name** or **email address** into the **Search by name or email address** search box, and click the checkbox to add this user to the **Selected** list.</span></span>

13. <span data-ttu-id="23c36-374">Po dokončení výběru uživatelů klikněte na **vyberte** tlačítko, které chcete přidat do seznamu uživatelů a skupin, které chcete přiřadit k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="23c36-374">When you are finished selecting users, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="23c36-375">**Volitelné:** klikněte na tlačítko **vybrat roli** selektor v **přidat přiřazení** okna Vybrat roli přiřadit uživatele, který jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="23c36-375">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the users you have selected.</span></span>

15. <span data-ttu-id="23c36-376">Klikněte **přiřadit** tlačítko přiřadit aplikace pro vybraného uživatele.</span><span class="sxs-lookup"><span data-stu-id="23c36-376">Click the **Assign** button to assign the application to the selected users.</span></span>

<span data-ttu-id="23c36-377">Po krátké době uživatele, které jste vybrali moci spustit tyto aplikace na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="23c36-377">After a short period, the users you have selected be able to launch these applications in the Access Panel.</span></span>

## <a name="if-these-troubleshooting-steps-do-not-the-resolve-the-issue"></a><span data-ttu-id="23c36-378">Pokud tyto kroky řešení potíží se není vyřešit problém</span><span class="sxs-lookup"><span data-stu-id="23c36-378">If these troubleshooting steps do not the resolve the issue</span></span>

<span data-ttu-id="23c36-379">Otevřete lístek podpory s následujícími informacemi, pokud je k dispozici:</span><span class="sxs-lookup"><span data-stu-id="23c36-379">open a support ticket with the following information if available:</span></span>

-   <span data-ttu-id="23c36-380">ID chyby korelace</span><span class="sxs-lookup"><span data-stu-id="23c36-380">Correlation error ID</span></span>

-   <span data-ttu-id="23c36-381">Hlavní název uživatele (e-mailovou adresu uživatele)</span><span class="sxs-lookup"><span data-stu-id="23c36-381">UPN (user email address)</span></span>

-   <span data-ttu-id="23c36-382">TenantID</span><span class="sxs-lookup"><span data-stu-id="23c36-382">TenantID</span></span>

-   <span data-ttu-id="23c36-383">Typ prohlížeče</span><span class="sxs-lookup"><span data-stu-id="23c36-383">Browser type</span></span>

-   <span data-ttu-id="23c36-384">Dojde k časové pásmo a čas nebo období při chybě</span><span class="sxs-lookup"><span data-stu-id="23c36-384">Time zone and time/timeframe during error occurs</span></span>

-   <span data-ttu-id="23c36-385">Fiddler trasování</span><span class="sxs-lookup"><span data-stu-id="23c36-385">Fiddler traces</span></span>

## <a name="next-steps"></a><span data-ttu-id="23c36-386">Další kroky</span><span class="sxs-lookup"><span data-stu-id="23c36-386">Next steps</span></span>
[<span data-ttu-id="23c36-387">Zadejte jednotné přihlašování pro vaše aplikace s Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="23c36-387">Provide single sign-on to your apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)

