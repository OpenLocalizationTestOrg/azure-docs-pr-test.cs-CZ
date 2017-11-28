---
title: "Potíže při přihlašování k aplikaci pomocí odkazu deeplink | Microsoft Docs"
description: "Řešení potíží při přístupu k aplikaci z adresy URL odkazu deeplink pomocí služby Azure AD"
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
ms.openlocfilehash: 798015ab68afc65378cffc75afec9c7f91fc1926
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="problems-signing-in-to-an-application-using-a-deeplink"></a><span data-ttu-id="00da9-103">Potíže při přihlašování k aplikaci pomocí odkazu deeplink</span><span class="sxs-lookup"><span data-stu-id="00da9-103">Problems signing in to an application using a deeplink</span></span>

<span data-ttu-id="00da9-104">Přístupový Panel je webový portál, který umožňuje uživatele, který má pracovní nebo školní účet ve službě Azure Active Directory (Azure AD) k zobrazení a spustit cloudové aplikace, aby správce Azure AD má je přístup povolen.</span><span class="sxs-lookup"><span data-stu-id="00da9-104">The Access Panel is a web-based portal which enables a user with a work or school account in Azure Active Directory (Azure AD) to view and start cloud-based applications that the Azure AD administrator has granted them access to.</span></span> 

<span data-ttu-id="00da9-105">Tyto aplikace jsou konfigurovány jménem uživatele na portálu Azure AD.</span><span class="sxs-lookup"><span data-stu-id="00da9-105">These applications are configured on behalf of the user in the Azure AD portal.</span></span> <span data-ttu-id="00da9-106">Aplikace musí být správně nakonfigurovaná a přiřadit uživatele nebo skupiny, kterých je uživatel členem skupiny chcete vidět aplikaci na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="00da9-106">The application must be configured properly and assigned to the user or a group the user is a member of to see the application in the Access Panel.</span></span>

<span data-ttu-id="00da9-107">Přístup uživatele nebo přímých odkazů URL jsou odkazy, které vaši uživatelé mohou využít k přístupu k aplikacím heslo SSO přímo z jejich řádky adresu URL prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="00da9-107">Deep links or User access URLs are links your users may use to access their password-SSO applications directly from their browsers URL bars.</span></span> <span data-ttu-id="00da9-108">Přechodem na tento odkaz, uživatelé budou automaticky přihlášení k aplikaci bez nutnosti nejprve přejít na Panel přístupu.</span><span class="sxs-lookup"><span data-stu-id="00da9-108">By navigating to this link, users be automatically signed into the application without having to go to the Access Panel first.</span></span> <span data-ttu-id="00da9-109">Toto je stejný odkaz, který uživatelé používat pro přístup k těmto aplikacím z Spouštěč aplikace Office 365.</span><span class="sxs-lookup"><span data-stu-id="00da9-109">This is the same link that users use to access these applications from the Office 365 application launcher.</span></span>

## <a name="general-issues-to-check-first"></a><span data-ttu-id="00da9-110">Běžné problémy a proveďte nejprve kontrolu</span><span class="sxs-lookup"><span data-stu-id="00da9-110">General issues to check first</span></span>

-   <span data-ttu-id="00da9-111">Zajistěte, aby vaše použití **prohlížeče** , splňuje minimální požadavky na přístupový Panel.</span><span class="sxs-lookup"><span data-stu-id="00da9-111">Make sure your using a **browser** that meets the minimum requirements for the Access Panel.</span></span>

-   <span data-ttu-id="00da9-112">Zajistěte, aby prohlížeče uživatele přidala adresu URL aplikace pro jeho **Důvěryhodné servery**.</span><span class="sxs-lookup"><span data-stu-id="00da9-112">Make sure the user’s browser has added the URL of the application to its **trusted sites**.</span></span>

-   <span data-ttu-id="00da9-113">Ujistěte se, zkontrolujte, že aplikace je **nakonfigurované** správně.</span><span class="sxs-lookup"><span data-stu-id="00da9-113">Make sure to check the application is **configured** correctly.</span></span>

-   <span data-ttu-id="00da9-114">Zkontrolujte, zda je účet uživatele **povoleno** pro přihlášení.</span><span class="sxs-lookup"><span data-stu-id="00da9-114">Make sure the user’s account is **enabled** for sign ins.</span></span>

-   <span data-ttu-id="00da9-115">Zkontrolujte, zda je účet uživatele **není uzamčen.**</span><span class="sxs-lookup"><span data-stu-id="00da9-115">Make sure the user’s account is **not locked out.**</span></span>

-   <span data-ttu-id="00da9-116">Zkontrolujte, že uživatele **není platnost nebo zapomenete heslo.**</span><span class="sxs-lookup"><span data-stu-id="00da9-116">Make sure the user’s **password is not expired or forgotten.**</span></span>

-   <span data-ttu-id="00da9-117">Zajistěte, aby **Multi-Factor Authentication** neblokuje přístup uživatelů.</span><span class="sxs-lookup"><span data-stu-id="00da9-117">Make sure **Multi-Factor Authentication** is not blocking user access.</span></span>

-   <span data-ttu-id="00da9-118">Zajistěte, aby **zásady podmíněného přístupu** nebo **Identity Protection** zásad neblokuje přístup uživatelů.</span><span class="sxs-lookup"><span data-stu-id="00da9-118">Make sure a **Conditional Access policy** or **Identity Protection** policy is not blocking user access.</span></span>

-   <span data-ttu-id="00da9-119">Ujistěte se, že uživatele **ověřování kontaktní údaje** je aktuální povolit Multi-Factor Authentication nebo podmíněného přístupu zásad, která budou vynucena.</span><span class="sxs-lookup"><span data-stu-id="00da9-119">Make sure that a user’s **authentication contact info** is up to date to allow Multi-Factor Authentication or Conditional Access policies to be enforced.</span></span>

-   <span data-ttu-id="00da9-120">Zkontrolujte, zda také zkuste vymazání souborů cookie v prohlížeči a zkusit se přihlásit znovu.</span><span class="sxs-lookup"><span data-stu-id="00da9-120">Make sure to also try clearing your browser’s cookies and trying to sign in again.</span></span>

## <a name="checking-the-deeplink"></a><span data-ttu-id="00da9-121">Kontrola odkazu deeplink</span><span class="sxs-lookup"><span data-stu-id="00da9-121">Checking the deeplink</span></span>

<span data-ttu-id="00da9-122">Pokud chcete zkontrolovat, jestli máte správné odkazu deeplink, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="00da9-122">To check if you have the correct deeplink, follow the steps below:</span></span>

1.  <span data-ttu-id="00da9-123">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="00da9-123">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="00da9-124">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="00da9-124">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="00da9-125">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="00da9-125">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="00da9-126">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="00da9-126">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="00da9-127">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="00da9-127">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="00da9-128">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="00da9-128">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="00da9-129">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="00da9-129">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

7.  <span data-ttu-id="00da9-130">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="00da9-130">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

8.  <span data-ttu-id="00da9-131">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="00da9-131">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

9.  <span data-ttu-id="00da9-132">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="00da9-132">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

10. <span data-ttu-id="00da9-133">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="00da9-133">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="00da9-134">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="00da9-134">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

11. <span data-ttu-id="00da9-135">Vyberte aplikaci, kterou chcete kontrolu odkazu deeplink pro.</span><span class="sxs-lookup"><span data-stu-id="00da9-135">Select the application you want the check the deeplink for.</span></span>

12. <span data-ttu-id="00da9-136">Najít popisek **adresu URL pro přístup uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="00da9-136">Find the label **User Access URL**.</span></span> <span data-ttu-id="00da9-137">Tato adresa URL by měl odpovídat můžete odkazu deeplink.</span><span class="sxs-lookup"><span data-stu-id="00da9-137">You deeplink should match this URL.</span></span>

## <a name="how-to-install-the-access-panel-browser-extension"></a><span data-ttu-id="00da9-138">Postup instalace rozšíření přístup k panelu prohlížeče</span><span class="sxs-lookup"><span data-stu-id="00da9-138">How to install the Access Panel Browser extension</span></span>

<span data-ttu-id="00da9-139">Chcete-li nainstalovat rozšíření přístup k panelu prohlížeče, postupujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="00da9-139">To install the Access Panel Browser extension, follow the steps below:</span></span>

1.  <span data-ttu-id="00da9-140">Otevřete [přístupový Panel](https://myapps.microsoft.com) v jednom z podporovaných prohlížečích a přihlaste se jako **uživatele** ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="00da9-140">Open the [Access Panel](https://myapps.microsoft.com) in one of the supported browsers and sign in as a **user** in your Azure AD.</span></span>

2.  <span data-ttu-id="00da9-141">Klikněte na tlačítko **SSO heslo aplikace** na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="00da9-141">click a **password-SSO application** in the Access Panel.</span></span>

3.  <span data-ttu-id="00da9-142">V řádku, požádá o instalaci softwaru, vyberte **instalovat nyní**.</span><span class="sxs-lookup"><span data-stu-id="00da9-142">In the prompt asking to install the software, select **Install Now**.</span></span>

4.  <span data-ttu-id="00da9-143">Založené na prohlížeči budete přesměrováni na odkaz ke stažení.</span><span class="sxs-lookup"><span data-stu-id="00da9-143">Based on your browser you be directed to the download link.</span></span> <span data-ttu-id="00da9-144">**Přidat** rozšíření do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="00da9-144">**Add** the extension to your browser.</span></span>

5.  <span data-ttu-id="00da9-145">Pokud prohlížeč zobrazí dotaz, vyberte buď **povolit** nebo **povolit** rozšíření.</span><span class="sxs-lookup"><span data-stu-id="00da9-145">If your browser asks, select to either **Enable** or **Allow** the extension.</span></span>

6.  <span data-ttu-id="00da9-146">Po instalaci **restartujte** relaci prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="00da9-146">Once installed, **restart** your browser session.</span></span>

7.  <span data-ttu-id="00da9-147">Přihlaste se do přístupového panelu a zobrazit, pokud můžete **spusťte** aplikace heslo SSO</span><span class="sxs-lookup"><span data-stu-id="00da9-147">Sign in into the Access Panel and see if you can **launch** your password-SSO applications</span></span>

<span data-ttu-id="00da9-148">Rozšíření pro Chrome a Firefox může také stáhnout z přímé odkazy níže:</span><span class="sxs-lookup"><span data-stu-id="00da9-148">You may also download the extension for Chrome and Firefox from the direct links below:</span></span>

-   [<span data-ttu-id="00da9-149">Rozšíření pro Chrome přístup panely</span><span class="sxs-lookup"><span data-stu-id="00da9-149">Chrome Access Panel Extension</span></span>](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [<span data-ttu-id="00da9-150">Rozšíření panelu Firefox přístup</span><span class="sxs-lookup"><span data-stu-id="00da9-150">Firefox Access Panel Extension</span></span>](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="00da9-151">Postup konfigurace hesel jednotné přihlašování pro aplikaci Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="00da9-151">How to configure password single sign-on for an Azure AD gallery application</span></span>

<span data-ttu-id="00da9-152">Pro konfiguraci aplikace z Galerie Azure AD, které potřebujete:</span><span class="sxs-lookup"><span data-stu-id="00da9-152">To configure an application from the Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="00da9-153">Přidat aplikaci z Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="00da9-153">Add an application from the Azure AD gallery</span></span>](#add-an-application-from-the-Azure-AD-gallery)

-   [<span data-ttu-id="00da9-154">Konfigurace aplikace pro heslo jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="00da9-154">Configure the application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

### <a name="add-an-application-from-the-azure-ad-gallery"></a><span data-ttu-id="00da9-155">Přidat aplikaci z Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="00da9-155">Add an application from the Azure AD gallery</span></span>

<span data-ttu-id="00da9-156">Pokud chcete přidat aplikaci z Galerie Azure AD, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="00da9-156">To add an application from the Azure AD Gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="00da9-157">Otevřete [portálu Azure](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**.</span><span class="sxs-lookup"><span data-stu-id="00da9-157">Open the [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**.</span></span>

2.  <span data-ttu-id="00da9-158">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="00da9-158">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="00da9-159">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="00da9-159">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="00da9-160">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="00da9-160">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="00da9-161">klikněte **přidat** v pravém horním rohu na tlačítko **podnikové aplikace, které** okno.</span><span class="sxs-lookup"><span data-stu-id="00da9-161">click the **Add** button at the top-right corner on the **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="00da9-162">V **zadejte název** textbox z **přidat z Galerie** části, zadejte název aplikace.</span><span class="sxs-lookup"><span data-stu-id="00da9-162">In the **Enter a name** textbox from the **Add from the gallery** section, type the name of the application.</span></span>

7.  <span data-ttu-id="00da9-163">Vyberte aplikaci, kterou chcete nakonfigurovat pro jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="00da9-163">Select the application you want to configure for single sign-on.</span></span>

8.  <span data-ttu-id="00da9-164">Před přidáním aplikace, můžete změnit její název ze **název** textové pole.</span><span class="sxs-lookup"><span data-stu-id="00da9-164">Before adding the application, you can change its name from the **Name** textbox.</span></span>

9.  <span data-ttu-id="00da9-165">Klikněte na tlačítko **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="00da9-165">Click **Add** button, to add the application.</span></span>

<span data-ttu-id="00da9-166">Po krátké době nebudete moci zobrazit okno Konfigurace aplikace.</span><span class="sxs-lookup"><span data-stu-id="00da9-166">After a short period, you be able to see the application’s configuration blade.</span></span>

### <a name="configure-the-application-for-password-single-sign-on"></a><span data-ttu-id="00da9-167">Konfigurace aplikace pro heslo jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="00da9-167">Configure the application for password single sign-on</span></span>

<span data-ttu-id="00da9-168">Pokud chcete konfigurovat jednotné přihlašování pro aplikace, postupujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="00da9-168">To configure single sign-on for an application, follow the steps below:</span></span>

1.  <span data-ttu-id="00da9-169">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="00da9-169">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="00da9-170">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="00da9-170">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="00da9-171">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="00da9-171">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="00da9-172">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="00da9-172">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="00da9-173">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="00da9-173">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="00da9-174">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="00da9-174">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="00da9-175">Vyberte aplikaci, kterou chcete konfigurovat jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="00da9-175">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="00da9-176">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="00da9-176">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="00da9-177">Vyberte režim **založené na heslech přihlášení.**</span><span class="sxs-lookup"><span data-stu-id="00da9-177">Select the mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="00da9-178">[Přiřazení uživatelů k aplikaci](#how-to-assign-a-user-to-an-application-directly).</span><span class="sxs-lookup"><span data-stu-id="00da9-178">[Assign users to the application](#how-to-assign-a-user-to-an-application-directly).</span></span>

10. <span data-ttu-id="00da9-179">Kromě toho můžete taky zadat přihlašovací údaje jménem uživatele výběrem řádky uživatelů a kliknete na **pověření aktualizace** a zadání uživatelského jména a hesla jménem uživatele.</span><span class="sxs-lookup"><span data-stu-id="00da9-179">Additionally, you can also provide credentials on behalf of the user by selecting the rows of the users and clicking on **Update Credentials** and entering the username and password on behalf of the users.</span></span> <span data-ttu-id="00da9-180">Uživatelé, jinak se výzva k zadání sami přihlašovací údaje při spuštění.</span><span class="sxs-lookup"><span data-stu-id="00da9-180">Otherwise, users be prompted to enter the credentials themselves upon launch.</span></span>

## <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="00da9-181">Postup konfigurace hesel jednotné přihlašování pro aplikace bez Galerie</span><span class="sxs-lookup"><span data-stu-id="00da9-181">How to configure password single sign-on for a non-gallery application</span></span>

<span data-ttu-id="00da9-182">Pro konfiguraci aplikace z Galerie Azure AD, které potřebujete:</span><span class="sxs-lookup"><span data-stu-id="00da9-182">To configure an application from the Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="00da9-183">Přidání aplikace bez Galerie</span><span class="sxs-lookup"><span data-stu-id="00da9-183">Add a non-gallery application</span></span>](#add-a-non-gallery-application)

-   [<span data-ttu-id="00da9-184">Konfigurace aplikace pro heslo jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="00da9-184">Configure the application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

### <a name="add-a-non-gallery-application"></a><span data-ttu-id="00da9-185">Přidání aplikace bez Galerie</span><span class="sxs-lookup"><span data-stu-id="00da9-185">Add a non-gallery application</span></span>

<span data-ttu-id="00da9-186">Pokud chcete přidat aplikaci z Galerie Azure AD, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="00da9-186">To add an application from the Azure AD Gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="00da9-187">Otevřete [portálu Azure](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**.</span><span class="sxs-lookup"><span data-stu-id="00da9-187">Open the [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**.</span></span>

2.  <span data-ttu-id="00da9-188">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="00da9-188">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="00da9-189">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="00da9-189">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="00da9-190">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="00da9-190">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="00da9-191">klikněte **přidat** v pravém horním rohu na tlačítko **podnikové aplikace, které** okno.</span><span class="sxs-lookup"><span data-stu-id="00da9-191">click the **Add** button at the top-right corner on the **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="00da9-192">Klikněte na tlačítko **Galerie jiná aplikace.**</span><span class="sxs-lookup"><span data-stu-id="00da9-192">click **Non-gallery application.**</span></span>

7.  <span data-ttu-id="00da9-193">Zadejte název vaší aplikace v **název** textové pole.</span><span class="sxs-lookup"><span data-stu-id="00da9-193">Enter the name of your application in the **Name** textbox.</span></span> <span data-ttu-id="00da9-194">Vyberte **přidat.**</span><span class="sxs-lookup"><span data-stu-id="00da9-194">Select **Add.**</span></span>

<span data-ttu-id="00da9-195">Po krátké době nebudete moci zobrazit okno Konfigurace aplikace.</span><span class="sxs-lookup"><span data-stu-id="00da9-195">After a short period, you be able to see the application’s configuration blade.</span></span>

### <a name="configure-the-application-for-password-single-sign-on"></a><span data-ttu-id="00da9-196">Konfigurace aplikace pro heslo jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="00da9-196">Configure the application for password single sign-on</span></span>

<span data-ttu-id="00da9-197">Pokud chcete konfigurovat jednotné přihlašování pro aplikace, postupujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="00da9-197">To configure single sign-on for an application, follow the steps below:</span></span>

1.  <span data-ttu-id="00da9-198">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="00da9-198">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="00da9-199">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="00da9-199">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="00da9-200">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="00da9-200">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="00da9-201">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="00da9-201">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="00da9-202">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="00da9-202">click **All Applications** to view a list of all your applications.</span></span>

    1.  <span data-ttu-id="00da9-203">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="00da9-203">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="00da9-204">Vyberte aplikaci, kterou chcete konfigurovat jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="00da9-204">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="00da9-205">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="00da9-205">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="00da9-206">Vyberte režim **založené na heslech přihlášení.**</span><span class="sxs-lookup"><span data-stu-id="00da9-206">Select the mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="00da9-207">Zadejte **přihlašovací adresa URL**.</span><span class="sxs-lookup"><span data-stu-id="00da9-207">Enter the **Sign-on URL**.</span></span> <span data-ttu-id="00da9-208">Toto je adresa URL, kde uživatelé zadat svoje uživatelské jméno a heslo pro přihlášení k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="00da9-208">This is the URL where users enter their username and password to sign in to.</span></span> <span data-ttu-id="00da9-209">Zkontrolujte, zda že přihlašovací pole jsou viditelné na adrese URL.</span><span class="sxs-lookup"><span data-stu-id="00da9-209">Ensure the sign in fields are visible at the URL.</span></span>

10. <span data-ttu-id="00da9-210">Přiřazení uživatelů k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="00da9-210">Assign users to the application.</span></span>

11. <span data-ttu-id="00da9-211">Kromě toho můžete taky zadat přihlašovací údaje jménem uživatele výběrem řádky uživatelů a kliknete na **pověření aktualizace** a zadání uživatelského jména a hesla jménem uživatele.</span><span class="sxs-lookup"><span data-stu-id="00da9-211">Additionally, you can also provide credentials on behalf of the user by selecting the rows of the users and clicking on **Update Credentials** and entering the username and password on behalf of the users.</span></span> <span data-ttu-id="00da9-212">Uživatelé, jinak se výzva k zadání sami přihlašovací údaje při spuštění.</span><span class="sxs-lookup"><span data-stu-id="00da9-212">Otherwise, users be prompted to enter the credentials themselves upon launch.</span></span>

## <a name="how-to-assign-a-user-to-an-application-directly"></a><span data-ttu-id="00da9-213">Postup přiřazení uživatele k aplikaci přímo</span><span class="sxs-lookup"><span data-stu-id="00da9-213">How to assign a user to an application directly</span></span>

<span data-ttu-id="00da9-214">Jeden nebo více uživatelů přiřadit přímo k aplikaci, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="00da9-214">To assign one or more users to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="00da9-215">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="00da9-215">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="00da9-216">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="00da9-216">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="00da9-217">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="00da9-217">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="00da9-218">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="00da9-218">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="00da9-219">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="00da9-219">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="00da9-220">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="00da9-220">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="00da9-221">Vyberte aplikaci, kterou chcete přiřadit uživatele k ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="00da9-221">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="00da9-222">Po načtení aplikace, klikněte na **uživatelů a skupin** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="00da9-222">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="00da9-223">Klikněte na tlačítko **přidat** tlačítko na **uživatelů a skupin** seznamu otevřete **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="00da9-223">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="00da9-224">Klikněte na tlačítko **uživatelů a skupin** pro výběr **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="00da9-224">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="00da9-225">Zadejte **celý název** nebo **e-mailová adresa** uživatele vás zajímá přiřazení do **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="00da9-225">Type in the **full name** or **email address** of the user you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="00da9-226">Najeďte myší **uživatele** v seznamu na nich **políčko**.</span><span class="sxs-lookup"><span data-stu-id="00da9-226">Hover over the **user** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="00da9-227">Klikněte na zaškrtávací políčko vedle profilové fotky nebo logo pro přidání uživatelů do uživatele **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="00da9-227">Click the checkbox next to the user’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="00da9-228">**Volitelné:** Pokud byste chtěli **přidat více než jeden uživatel**, typ v jiném **celý název** nebo **e-mailová adresa** do **hledat podle jména nebo e-mailové adresy** pole pro vyhledávání a klikněte na zaškrtávací políčko, chcete-li přidat tento uživatel **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="00da9-228">**Optional:** If you would like to **add more than one user**, type in another **full name** or **email address** into the **Search by name or email address** search box, and click the checkbox to add this user to the **Selected** list.</span></span>

13. <span data-ttu-id="00da9-229">Po dokončení výběru uživatelů klikněte na **vyberte** tlačítko, které chcete přidat do seznamu uživatelů a skupin, které chcete přiřadit k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="00da9-229">When you are finished selecting users, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="00da9-230">**Volitelné:** klikněte na tlačítko **vybrat roli** selektor v **přidat přiřazení** okna Vybrat roli přiřadit uživatele, který jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="00da9-230">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the users you have selected.</span></span>

15. <span data-ttu-id="00da9-231">Klikněte **přiřadit** tlačítko přiřadit aplikace pro vybraného uživatele.</span><span class="sxs-lookup"><span data-stu-id="00da9-231">Click the **Assign** button to assign the application to the selected users.</span></span>

<span data-ttu-id="00da9-232">Po krátké době uživatele, které jste vybrali moci spustit tyto aplikace na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="00da9-232">After a short period, the users you have selected be able to launch these applications in the Access Panel.</span></span>

## <a name="if-these-troubleshooting-steps-do-not-the-resolve-the-issue"></a><span data-ttu-id="00da9-233">Pokud tyto kroky řešení potíží není vyřešte problém.</span><span class="sxs-lookup"><span data-stu-id="00da9-233">If these troubleshooting steps do not the resolve the issue.</span></span> 

<span data-ttu-id="00da9-234">Otevřete lístek podpory s následujícími informacemi, pokud je k dispozici:</span><span class="sxs-lookup"><span data-stu-id="00da9-234">open a support ticket with the following information if available:</span></span>

-   <span data-ttu-id="00da9-235">ID chyby korelace</span><span class="sxs-lookup"><span data-stu-id="00da9-235">Correlation error ID</span></span>

-   <span data-ttu-id="00da9-236">Hlavní název uživatele (e-mailovou adresu uživatele)</span><span class="sxs-lookup"><span data-stu-id="00da9-236">UPN (user email address)</span></span>

-   <span data-ttu-id="00da9-237">TenantID</span><span class="sxs-lookup"><span data-stu-id="00da9-237">TenantID</span></span>

-   <span data-ttu-id="00da9-238">Typ prohlížeče</span><span class="sxs-lookup"><span data-stu-id="00da9-238">Browser type</span></span>

-   <span data-ttu-id="00da9-239">Dojde k časové pásmo a čas nebo období při chybě</span><span class="sxs-lookup"><span data-stu-id="00da9-239">Time zone and time/timeframe during error occurs</span></span>

-   <span data-ttu-id="00da9-240">Fiddler trasování</span><span class="sxs-lookup"><span data-stu-id="00da9-240">Fiddler traces</span></span>

## <a name="next-steps"></a><span data-ttu-id="00da9-241">Další kroky</span><span class="sxs-lookup"><span data-stu-id="00da9-241">Next steps</span></span>
[<span data-ttu-id="00da9-242">Zadejte jednotné přihlašování pro vaše aplikace s Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="00da9-242">Provide single sign-on to your apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)
