---
title: "Potíže při přihlašování k aplikaci Azure AD Galerie konfigurován pro federované jednotné přihlašování | Microsoft Docs"
description: "Řešení potíží s aplikací Azure AD Galerie nakonfigurovaný pro heslo jednotné přihlašování"
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
ms.openlocfilehash: f8521d1386bba8004e84fe8862a5f59a65ea19ad
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="problems-signing-in-to-an-azure-ad-gallery-application-configured-for-federated-single-sign-on"></a><span data-ttu-id="b2a2c-103">Potíže při přihlašování k aplikaci Azure AD Galerie nakonfigurovaný pro federované jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b2a2c-103">Problems signing in to an Azure AD Gallery application configured for federated single sign-on</span></span>

<span data-ttu-id="b2a2c-104">Přístupový Panel je webový portál, který uživatel, který má pracovní nebo školní účet ve službě Azure Active Directory (Azure AD) umožňuje zobrazovat a spouštět cloudové aplikace, které je správce Azure AD udělil přístup.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-104">The Access Panel is a web-based portal which enables a user who has a work or school account in Azure Active Directory (Azure AD) to view and launch cloud-based applications that the Azure AD administrator has granted them access to.</span></span> <span data-ttu-id="b2a2c-105">Uživatel, který má edice Azure AD můžete také skupiny samoobslužné služby a možnosti správy aplikace prostřednictvím na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-105">A user who has Azure AD editions can also use self-service group and app management capabilities through the Access Panel.</span></span> <span data-ttu-id="b2a2c-106">Přístupový Panel je nezávislý na portálu Azure a nevyžaduje, aby uživatelé měli předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-106">The Access Panel is separate from the Azure portal and does not require users to have an Azure subscription.</span></span>

<span data-ttu-id="b2a2c-107">Pokud chcete používat založené na heslech jednotné přihlašování (SSO) na přístupovém panelu, musí být nainstalované rozšíření přístupového panelu v prohlížeče uživatele.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-107">To use password-based single sign-on (SSO) in the Access Panel, the Access Panel extension must be installed in the user’s browser.</span></span> <span data-ttu-id="b2a2c-108">Toto rozšíření je stažen automaticky, když uživatel vybere aplikaci, která je nakonfigurována pro jednotné přihlašování založené na heslech.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-108">This extension is downloaded automatically when a user selects an application that is configured for password-based SSO.</span></span>

## <a name="meeting-browser-requirements-for-the-access-panel"></a><span data-ttu-id="b2a2c-109">Splňuje požadavky na prohlížeč pro přístup k panelu</span><span class="sxs-lookup"><span data-stu-id="b2a2c-109">Meeting browser requirements for the Access Panel</span></span>

<span data-ttu-id="b2a2c-110">Přístupový Panel vyžaduje prohlížeč, který podporuje jazyk JavaScript a aktivoval šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-110">The Access Panel requires a browser that supports JavaScript and has CSS enabled.</span></span> <span data-ttu-id="b2a2c-111">Pokud chcete používat založené na heslech jednotné přihlašování (SSO) na přístupovém panelu, musí být nainstalované rozšíření přístupového panelu v prohlížeče uživatele.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-111">To use password-based single sign-on (SSO) in the Access Panel, the Access Panel extension must be installed in the user’s browser.</span></span> <span data-ttu-id="b2a2c-112">Toto rozšíření je stažen automaticky, když uživatel vybere aplikaci, která je nakonfigurována pro jednotné přihlašování založené na heslech.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-112">This extension is downloaded automatically when a user selects an application that is configured for password-based SSO.</span></span>

<span data-ttu-id="b2a2c-113">Pro jednotné přihlašování založené na heslech může být prohlížeče koncového uživatele:</span><span class="sxs-lookup"><span data-stu-id="b2a2c-113">For password-based SSO, the end user’s browsers can be:</span></span>

-   <span data-ttu-id="b2a2c-114">Internet Explorer 8, 9, 10, 11 – Windows 7 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-114">Internet Explorer 8, 9, 10, 11 - on Windows 7 or later</span></span>

-   <span data-ttu-id="b2a2c-115">Chrome – v systému Windows 7 nebo novější a v systému MacOS X nebo novější</span><span class="sxs-lookup"><span data-stu-id="b2a2c-115">Chrome - on Windows 7 or later, and on MacOS X or later</span></span>

-   <span data-ttu-id="b2a2c-116">Firefox 26.0 nebo později - do systému Windows XP SP2 nebo novější a v systému Mac OS X 10,6 nebo novější</span><span class="sxs-lookup"><span data-stu-id="b2a2c-116">Firefox 26.0 or later - on Windows XP SP2 or later, and on Mac OS X 10.6 or later</span></span>

>[!NOTE]
><span data-ttu-id="b2a2c-117">Rozšíření založené na heslech jednotného přihlašování k dispozici pro Edge ve Windows 10 při rozšíření prohlížeče stát podporované pro okraj.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-117">The password-based SSO extension become available for Edge in Windows 10 when browser extensions become supported for Edge.</span></span>
>
>

## <a name="how-to-install-the-access-panel-browser-extension"></a><span data-ttu-id="b2a2c-118">Postup instalace rozšíření přístup k panelu prohlížeče</span><span class="sxs-lookup"><span data-stu-id="b2a2c-118">How to install the Access Panel Browser extension</span></span>

<span data-ttu-id="b2a2c-119">Chcete-li nainstalovat rozšíření přístup k panelu prohlížeče, postupujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="b2a2c-119">To install the Access Panel Browser extension, follow the steps below:</span></span>

1.  <span data-ttu-id="b2a2c-120">Otevřete [přístupový Panel](https://myapps.microsoft.com) v jednom z podporovaných prohlížečích a přihlaste se jako **uživatele** ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-120">Open the [Access Panel](https://myapps.microsoft.com) in one of the supported browsers and sign in as a **user** in your Azure AD.</span></span>

2.  <span data-ttu-id="b2a2c-121">Klikněte na tlačítko **SSO heslo aplikace** na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-121">click a **password-SSO application** in the Access Panel.</span></span>

3.  <span data-ttu-id="b2a2c-122">V řádku, požádá o instalaci softwaru, vyberte **instalovat nyní**.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-122">In the prompt asking to install the software, select **Install Now**.</span></span>

4.  <span data-ttu-id="b2a2c-123">Založené na prohlížeči budete přesměrováni na odkaz ke stažení.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-123">Based on your browser you be directed to the download link.</span></span> <span data-ttu-id="b2a2c-124">**Přidat** rozšíření do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-124">**Add** the extension to your browser.</span></span>

5.  <span data-ttu-id="b2a2c-125">Pokud prohlížeč zobrazí dotaz, vyberte buď **povolit** nebo **povolit** rozšíření.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-125">If your browser asks, select to either **Enable** or **Allow** the extension.</span></span>

6.  <span data-ttu-id="b2a2c-126">Po instalaci **restartujte** relaci prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-126">Once installed, **restart** your browser session.</span></span>

7.  <span data-ttu-id="b2a2c-127">Přihlaste se do přístupového panelu a zobrazit, pokud můžete **spusťte** aplikace heslo SSO</span><span class="sxs-lookup"><span data-stu-id="b2a2c-127">Sign in into the Access Panel and see if you can **launch** your password-SSO applications</span></span>

<span data-ttu-id="b2a2c-128">Rozšíření pro Chrome a Firefox může také stáhnout z přímé odkazy níže:</span><span class="sxs-lookup"><span data-stu-id="b2a2c-128">You may also download the extension for Chrome and Firefox from the direct links below:</span></span>

-   [<span data-ttu-id="b2a2c-129">Rozšíření pro Chrome přístup panely</span><span class="sxs-lookup"><span data-stu-id="b2a2c-129">Chrome Access Panel Extension</span></span>](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [<span data-ttu-id="b2a2c-130">Rozšíření panelu Firefox přístup</span><span class="sxs-lookup"><span data-stu-id="b2a2c-130">Firefox Access Panel Extension</span></span>](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="setting-up-a-group-policy-for-internet-explorer"></a><span data-ttu-id="b2a2c-131">Nastavení zásad skupiny pro Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="b2a2c-131">Setting up a group policy for Internet Explorer</span></span>

<span data-ttu-id="b2a2c-132">Můžete nastavit zásady skupiny, které vám umožní vzdáleně instalovat rozšíření Panel přístupu pro Internet Explorer na počítače uživatelů.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-132">You can setup a group policy that allow you to remotely install the Access Panel extension for Internet Explorer on your users' machines.</span></span>

<span data-ttu-id="b2a2c-133">Požadavky patří:</span><span class="sxs-lookup"><span data-stu-id="b2a2c-133">The prerequisites include:</span></span>

-   <span data-ttu-id="b2a2c-134">Jste nastavili [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), a vaši uživatelé počítačů se připojili k vaší doméně.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-134">You have set up [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), and you have joined your users' machines to your domain.</span></span>

-   <span data-ttu-id="b2a2c-135">Musíte mít oprávnění "Upravit nastavení" k úpravě objektu zásad skupiny (GPO).</span><span class="sxs-lookup"><span data-stu-id="b2a2c-135">You must have the "Edit settings" permission to edit the Group Policy Object (GPO).</span></span> <span data-ttu-id="b2a2c-136">Ve výchozím nastavení, toto oprávnění mají členy z těchto skupin zabezpečení: Domain Administrators, Enterprise Administrators a Group Policy Creator Owners.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-136">By default, members of the following security groups have this permission: Domain Administrators, Enterprise Administrators, and Group Policy Creator Owners.</span></span> <span data-ttu-id="b2a2c-137">[Další informace](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).</span><span class="sxs-lookup"><span data-stu-id="b2a2c-137">[Learn more](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).</span></span>

<span data-ttu-id="b2a2c-138">Postupujte podle kurzu [nasazení rozšíření Panel přístupu pro Internet Explorer pomocí zásad skupiny](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-group-policy) pro podrobné informace o tom, jak nakonfigurovat zásady skupiny a nasadit je uživatelům.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-138">Follow the tutorial [How to Deploy the Access Panel Extension for Internet Explorer using Group Policy](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-group-policy) for step by step instructions on how to configure the group policy and deploy it to users.</span></span>

## <a name="troubleshoot-the-access-panel-in-internet-explorer"></a><span data-ttu-id="b2a2c-139">Řešení potíží s přístupového panelu v aplikaci Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="b2a2c-139">Troubleshoot the Access Panel in Internet Explorer</span></span>

<span data-ttu-id="b2a2c-140">Postupujte podle [řešení potíží s příponou Panel přístupu pro Internet Explorer](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-Troubleshoot) Průvodce pro přístup k nástroj pro diagnostiku a podrobné informace o konfiguraci rozšíření pro aplikaci Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-140">Follow the [Troubleshoot the Access Panel Extension for Internet Explorer](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-Troubleshoot) guide for access a diagnostics tool and step by step instructions on configuring the extension for IE.</span></span>

## <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="b2a2c-141">Postup konfigurace hesel jednotné přihlašování pro aplikaci Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="b2a2c-141">How to configure password single sign-on for an Azure AD gallery application</span></span>

<span data-ttu-id="b2a2c-142">Pro konfiguraci aplikace z Galerie Azure AD, které potřebujete:</span><span class="sxs-lookup"><span data-stu-id="b2a2c-142">To configure an application from the Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="b2a2c-143">Přidat aplikaci z Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="b2a2c-143">Add an application from the Azure AD gallery</span></span>](#_Add_an_application)

-   [<span data-ttu-id="b2a2c-144">Konfigurace aplikace pro heslo jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b2a2c-144">Configure the application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

-   [<span data-ttu-id="b2a2c-145">Přiřazení uživatelů k aplikaci</span><span class="sxs-lookup"><span data-stu-id="b2a2c-145">Assign users to the application</span></span>](#assign-users-to-the-application)

### <a name="add-an-application-from-the-azure-ad-gallery"></a><span data-ttu-id="b2a2c-146">Přidat aplikaci z Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="b2a2c-146">Add an application from the Azure AD gallery</span></span>

<span data-ttu-id="b2a2c-147">Pokud chcete přidat aplikaci z Galerie Azure AD, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="b2a2c-147">To add an application from the Azure AD Gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="b2a2c-148">Otevřete [portálu Azure](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**</span><span class="sxs-lookup"><span data-stu-id="b2a2c-148">Open the [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="b2a2c-149">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-149">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="b2a2c-150">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-150">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="b2a2c-151">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-151">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="b2a2c-152">klikněte **přidat** v pravém horním rohu na tlačítko **podnikové aplikace, které** okno.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-152">click the **Add** button at the top-right corner on the **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="b2a2c-153">V **zadejte název** textbox z **přidat z Galerie** části, zadejte název aplikace.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-153">In the **Enter a name** textbox from the **Add from the gallery** section, type the name of the application.</span></span>

7.  <span data-ttu-id="b2a2c-154">Vyberte aplikaci, kterou chcete nakonfigurovat pro jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-154">Select the application you want to configure for single sign-on.</span></span>

8.  <span data-ttu-id="b2a2c-155">Před přidáním aplikace, můžete změnit její název ze **název** textové pole.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-155">Before adding the application, you can change its name from the **Name** textbox.</span></span>

9.  <span data-ttu-id="b2a2c-156">Klikněte na tlačítko **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-156">Click **Add** button, to add the application.</span></span>

<span data-ttu-id="b2a2c-157">Po krátké době nebudete moci zobrazit okno Konfigurace aplikace.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-157">After a short period, you be able to see the application’s configuration blade.</span></span>

### <a name="configure-the-application-for-password-single-sign-on"></a><span data-ttu-id="b2a2c-158">Konfigurace aplikace pro heslo jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b2a2c-158">Configure the application for password single sign-on</span></span>

<span data-ttu-id="b2a2c-159">Pokud chcete konfigurovat jednotné přihlašování pro aplikace, postupujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="b2a2c-159">To configure single sign-on for an application, follow the steps below:</span></span>

1.  <span data-ttu-id="b2a2c-160">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="b2a2c-160">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="b2a2c-161">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-161">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="b2a2c-162">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-162">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="b2a2c-163">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-163">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="b2a2c-164">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-164">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="b2a2c-165">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="b2a2c-165">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="b2a2c-166">Vyberte aplikaci, kterou chcete konfigurovat jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b2a2c-166">Select the application you want to configure single sign-on</span></span>

7.  <span data-ttu-id="b2a2c-167">Po načtení aplikace, klikněte na **jednotného přihlašování** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-167">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="b2a2c-168">Vyberte režim **založené na heslech přihlášení.**</span><span class="sxs-lookup"><span data-stu-id="b2a2c-168">Select the mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="b2a2c-169">[Přiřazení uživatelů k aplikaci](#_How_to_assign).</span><span class="sxs-lookup"><span data-stu-id="b2a2c-169">[Assign users to the application](#_How_to_assign).</span></span>

10. <span data-ttu-id="b2a2c-170">Kromě toho můžete taky zadat přihlašovací údaje jménem uživatele výběrem řádky uživatelů a kliknete na **pověření aktualizace** a zadání uživatelského jména a hesla jménem uživatele.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-170">Additionally, you can also provide credentials on behalf of the user by selecting the rows of the users and clicking on **Update Credentials** and entering the username and password on behalf of the users.</span></span> <span data-ttu-id="b2a2c-171">Uživatelé, jinak se výzva k zadání sami přihlašovací údaje při spuštění.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-171">Otherwise, users be prompted to enter the credentials themselves upon launch.</span></span>

### <a name="assign-users-to-the-application"></a><span data-ttu-id="b2a2c-172">Přiřazení uživatelů k aplikaci</span><span class="sxs-lookup"><span data-stu-id="b2a2c-172">Assign users to the application</span></span>

<span data-ttu-id="b2a2c-173">Jeden nebo více uživatelů přiřadit přímo k aplikaci, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="b2a2c-173">To assign one or more users to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="b2a2c-174">Otevřete [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="b2a2c-174">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="b2a2c-175">Otevřete **rozšíření Azure Active Directory** kliknutím **další služby** v dolní části navigační nabídce vlevo hlavní.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-175">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="b2a2c-176">Zadejte **"Azure Active Directory**" v filtru vyhledávacího pole a vyberte **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-176">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="b2a2c-177">Klikněte na tlačítko **podnikové aplikace, které** v navigační nabídce vlevo Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-177">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="b2a2c-178">Klikněte na tlačítko **všechny aplikace** Chcete-li zobrazit seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-178">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="b2a2c-179">Pokud aplikaci chcete, aby se zobrazí tady nevidíte, pomocí **filtru** ovládací prvek v horní části **seznam všech aplikací** a nastavte **zobrazit** možnost k **všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="b2a2c-179">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="b2a2c-180">Vyberte aplikaci, kterou chcete přiřadit uživatele k ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-180">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="b2a2c-181">Po načtení aplikace, klikněte na **uživatelů a skupin** navigační nabídce vlevo aplikace.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-181">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="b2a2c-182">Klikněte na tlačítko **přidat** tlačítko na **uživatelů a skupin** seznamu otevřete **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-182">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="b2a2c-183">Klikněte na tlačítko **uživatelů a skupin** pro výběr **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-183">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="b2a2c-184">Zadejte **celý název** nebo **e-mailová adresa** uživatele vás zajímá přiřazení do **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-184">Type in the **full name** or **email address** of the user you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="b2a2c-185">Najeďte myší **uživatele** v seznamu na nich **políčko**.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-185">Hover over the **user** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="b2a2c-186">Klikněte na zaškrtávací políčko vedle profilové fotky nebo logo pro přidání uživatelů do uživatele **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-186">Click the checkbox next to the user’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="b2a2c-187">**Volitelné:** Pokud byste chtěli **přidat více než jeden uživatel**, typ v jiném **celý název** nebo **e-mailová adresa** do **hledat podle jména nebo e-mailové adresy** pole pro vyhledávání a klikněte na zaškrtávací políčko, chcete-li přidat tento uživatel **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-187">**Optional:** If you would like to **add more than one user**, type in another **full name** or **email address** into the **Search by name or email address** search box, and click the checkbox to add this user to the **Selected** list.</span></span>

13. <span data-ttu-id="b2a2c-188">Po dokončení výběru uživatelů klikněte na **vyberte** tlačítko, které chcete přidat do seznamu uživatelů a skupin, které chcete přiřadit k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-188">When you are finished selecting users, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="b2a2c-189">**Volitelné:** klikněte na tlačítko **vybrat roli** selektor v **přidat přiřazení** okna Vybrat roli přiřadit uživatele, který jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-189">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the users you have selected.</span></span>

15. <span data-ttu-id="b2a2c-190">Klikněte **přiřadit** tlačítko přiřadit aplikace pro vybraného uživatele.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-190">Click the **Assign** button to assign the application to the selected users.</span></span>

<span data-ttu-id="b2a2c-191">Po krátké době uživatele, které jste vybrali moci spustit tyto aplikace na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="b2a2c-191">After a short period, the users you have selected be able to launch these applications in the Access Panel.</span></span>

## <a name="if-these-troubleshoot-steps-dont-resolve-the-issue"></a><span data-ttu-id="b2a2c-192">Pokud toto řešení nemusíte kroky k vyřešení problému</span><span class="sxs-lookup"><span data-stu-id="b2a2c-192">If these troubleshoot steps don't resolve the issue</span></span> 
<span data-ttu-id="b2a2c-193">Otevřete lístek podpory s následujícími informacemi, pokud je k dispozici:</span><span class="sxs-lookup"><span data-stu-id="b2a2c-193">open a support ticket with the following information if available:</span></span>

-   <span data-ttu-id="b2a2c-194">ID chyby korelace</span><span class="sxs-lookup"><span data-stu-id="b2a2c-194">Correlation error ID</span></span>

-   <span data-ttu-id="b2a2c-195">Hlavní název uživatele (e-mailovou adresu uživatele)</span><span class="sxs-lookup"><span data-stu-id="b2a2c-195">UPN (user email address)</span></span>

-   <span data-ttu-id="b2a2c-196">TenantID</span><span class="sxs-lookup"><span data-stu-id="b2a2c-196">TenantID</span></span>

-   <span data-ttu-id="b2a2c-197">Typ prohlížeče</span><span class="sxs-lookup"><span data-stu-id="b2a2c-197">Browser type</span></span>

-   <span data-ttu-id="b2a2c-198">Dojde k časové pásmo a čas nebo období při chybě</span><span class="sxs-lookup"><span data-stu-id="b2a2c-198">Time zone and time/timeframe during error occurs</span></span>

-   <span data-ttu-id="b2a2c-199">Fiddler trasování</span><span class="sxs-lookup"><span data-stu-id="b2a2c-199">Fiddler traces</span></span>

## <a name="next-steps"></a><span data-ttu-id="b2a2c-200">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b2a2c-200">Next steps</span></span>
[<span data-ttu-id="b2a2c-201">Zadejte jednotné přihlašování pro vaše aplikace s Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="b2a2c-201">Provide single sign-on to your apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)
