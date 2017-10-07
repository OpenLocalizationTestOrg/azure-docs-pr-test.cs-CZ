---
title: "aaaProblems přihlášení tooan aplikace Azure AD Galerie konfigurovaná pro heslo jednotného přihlašování | Microsoft Docs"
description: "Popisuje problémových oblastí, které poskytují pokyny tootroubleshoot problémy související toosigning v tooAzure AD Galerie aplikace, které jsou nakonfigurované pro heslo jednotné přihlašování"
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
ms.openlocfilehash: f53ef4176db37dc6b1da2d61027155a6ba8f331e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="problems-signing-in-tooan-azure-ad-gallery-application-configured-for-password-single-sign-on"></a><span data-ttu-id="5f755-103">Potíže s přihlášením tooan Galerie Azure AD aplikace konfigurovaná pro heslo jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5f755-103">Problems signing in tooan Azure AD Gallery application configured for password single sign-on</span></span>

<span data-ttu-id="5f755-104">Hello přístupový Panel je, že je webový portál, který umožňuje uživatel, který má pracovní nebo školní účet v Azure Active Directory (Azure AD) tooview a spouštět cloudové aplikace tento správce hello Azure AD udělil přístup.</span><span class="sxs-lookup"><span data-stu-id="5f755-104">hello Access Panel is a web-based portal which enables a user who has a work or school account in Azure Active Directory (Azure AD) tooview and launch cloud-based applications that hello Azure AD administrator has granted them access to.</span></span> <span data-ttu-id="5f755-105">Uživatel, který má edice Azure AD můžete také skupiny samoobslužné služby a možnosti správy aplikace prostřednictvím hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="5f755-105">A user who has Azure AD editions can also use self-service group and app management capabilities through hello Access Panel.</span></span> <span data-ttu-id="5f755-106">Hello přístupový Panel je oddělená od hello portál Azure a nevyžaduje, aby uživatelé toohave předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="5f755-106">hello Access Panel is separate from hello Azure portal and does not require users toohave an Azure subscription.</span></span>

<span data-ttu-id="5f755-107">toouse založené na heslech jednotné přihlašování (SSO) v hello přístupový Panel hello přístupový Panel rozšíření musí být nainstalován v prohlížeči hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="5f755-107">toouse password-based single sign-on (SSO) in hello Access Panel, hello Access Panel extension must be installed in hello user’s browser.</span></span> <span data-ttu-id="5f755-108">Toto rozšíření je stažen automaticky, když uživatel vybere aplikaci, která je nakonfigurována pro jednotné přihlašování založené na heslech.</span><span class="sxs-lookup"><span data-stu-id="5f755-108">This extension is downloaded automatically when a user selects an application that is configured for password-based SSO.</span></span>

## <a name="meeting-browser-requirements-for-hello-access-panel"></a><span data-ttu-id="5f755-109">Splňuje požadavky na prohlížeč pro hello přístupového panelu</span><span class="sxs-lookup"><span data-stu-id="5f755-109">Meeting browser requirements for hello Access Panel</span></span>

<span data-ttu-id="5f755-110">Hello přístupový Panel vyžaduje prohlížeč, který podporuje jazyk JavaScript a aktivoval šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="5f755-110">hello Access Panel requires a browser that supports JavaScript and has CSS enabled.</span></span> <span data-ttu-id="5f755-111">toouse založené na heslech jednotné přihlašování (SSO) v hello přístupový Panel hello přístupový Panel rozšíření musí být nainstalován v prohlížeči hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="5f755-111">toouse password-based single sign-on (SSO) in hello Access Panel, hello Access Panel extension must be installed in hello user’s browser.</span></span> <span data-ttu-id="5f755-112">Toto rozšíření je stažen automaticky, když uživatel vybere aplikaci, která je nakonfigurována pro jednotné přihlašování založené na heslech.</span><span class="sxs-lookup"><span data-stu-id="5f755-112">This extension is downloaded automatically when a user selects an application that is configured for password-based SSO.</span></span>

<span data-ttu-id="5f755-113">Pro jednotné přihlašování založené na heslech může být prohlížeče hello koncového uživatele:</span><span class="sxs-lookup"><span data-stu-id="5f755-113">For password-based SSO, hello end user’s browsers can be:</span></span>

-   <span data-ttu-id="5f755-114">Internet Explorer 8, 9, 10, 11 – v systému Windows 7 nebo novější</span><span class="sxs-lookup"><span data-stu-id="5f755-114">Internet Explorer 8, 9, 10, 11 -- on Windows 7 or later</span></span>

-   <span data-ttu-id="5f755-115">Chrome – V systému Windows 7 nebo novější a v systému MacOS X nebo novější</span><span class="sxs-lookup"><span data-stu-id="5f755-115">Chrome -- on Windows 7 or later, and on MacOS X or later</span></span>

-   <span data-ttu-id="5f755-116">Firefox 26.0 nebo později – do systému Windows XP SP2 nebo novější a v systému Mac OS X 10,6 nebo novější</span><span class="sxs-lookup"><span data-stu-id="5f755-116">Firefox 26.0 or later -- on Windows XP SP2 or later, and on Mac OS X 10.6 or later</span></span>

>[!NOTE]
><span data-ttu-id="5f755-117">rozšíření jednotného přihlašování založené na heslech Hello k dispozici pro Edge ve Windows 10, při rozšíření prohlížeče stát podporované pro okraj.</span><span class="sxs-lookup"><span data-stu-id="5f755-117">hello password-based SSO extension become available for Edge in Windows 10 when browser extensions become supported for Edge.</span></span>
>
>

## <a name="how-tooinstall-hello-access-panel-browser-extension"></a><span data-ttu-id="5f755-118">Jak tooinstall hello rozšíření přístup k panelu prohlížeče</span><span class="sxs-lookup"><span data-stu-id="5f755-118">How tooinstall hello Access Panel Browser extension</span></span>

<span data-ttu-id="5f755-119">tooinstall hello rozšíření prohlížeče panely přístup, postupujte podle kroků hello níže:</span><span class="sxs-lookup"><span data-stu-id="5f755-119">tooinstall hello Access Panel Browser extension, follow hello steps below:</span></span>

1.  <span data-ttu-id="5f755-120">Otevřete hello [přístupový Panel](https://myapps.microsoft.com) v jednom z hello podporované prohlížeče a přihlaste se jako **uživatele** ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5f755-120">Open hello [Access Panel](https://myapps.microsoft.com) in one of hello supported browsers and sign in as a **user** in your Azure AD.</span></span>

2.  <span data-ttu-id="5f755-121">Klikněte na tlačítko **SSO heslo aplikace** v hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="5f755-121">click a **password-SSO application** in hello Access Panel.</span></span>

3.  <span data-ttu-id="5f755-122">V hello výzva s dotazem tooinstall hello softwaru, vyberte **instalovat nyní**.</span><span class="sxs-lookup"><span data-stu-id="5f755-122">In hello prompt asking tooinstall hello software, select **Install Now**.</span></span>

4.  <span data-ttu-id="5f755-123">Založené na prohlížeči, je možné směrovanou toohello odkaz ke stažení.</span><span class="sxs-lookup"><span data-stu-id="5f755-123">Based on your browser you be directed toohello download link.</span></span> <span data-ttu-id="5f755-124">**Přidat** hello rozšíření tooyour prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="5f755-124">**Add** hello extension tooyour browser.</span></span>

5.  <span data-ttu-id="5f755-125">Pokud prohlížeč zobrazí dotaz, vyberte tooeither **povolit** nebo **povolit** hello rozšíření.</span><span class="sxs-lookup"><span data-stu-id="5f755-125">If your browser asks, select tooeither **Enable** or **Allow** hello extension.</span></span>

6.  <span data-ttu-id="5f755-126">Po instalaci **restartujte** relaci prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="5f755-126">Once installed, **restart** your browser session.</span></span>

7.  <span data-ttu-id="5f755-127">Přihlaste se do hello přístupového panelu a zobrazit, pokud můžete **spusťte** aplikace heslo SSO</span><span class="sxs-lookup"><span data-stu-id="5f755-127">Sign in into hello Access Panel and see if you can **launch** your password-SSO applications</span></span>

<span data-ttu-id="5f755-128">Může také stáhnout hello rozšíření pro Chrome a Firefox z hello přímé odkazy níže:</span><span class="sxs-lookup"><span data-stu-id="5f755-128">You may also download hello extension for Chrome and Firefox from hello direct links below:</span></span>

-   [<span data-ttu-id="5f755-129">Rozšíření pro Chrome přístup panely</span><span class="sxs-lookup"><span data-stu-id="5f755-129">Chrome Access Panel Extension</span></span>](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [<span data-ttu-id="5f755-130">Rozšíření panelu Firefox přístup</span><span class="sxs-lookup"><span data-stu-id="5f755-130">Firefox Access Panel Extension</span></span>](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="setting-up-a-group-policy-for-internet-explorer"></a><span data-ttu-id="5f755-131">Nastavení zásad skupiny pro Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="5f755-131">Setting up a group policy for Internet Explorer</span></span>

<span data-ttu-id="5f755-132">Můžete nastavit zásady skupiny, které umožňují tooremotely instalace hello přístupový Panel rozšíření pro Internet Explorer na počítačích uživatelů.</span><span class="sxs-lookup"><span data-stu-id="5f755-132">You can setup a group policy that allow you tooremotely install hello Access Panel extension for Internet Explorer on your users' machines.</span></span>

<span data-ttu-id="5f755-133">Hello požadavky patří:</span><span class="sxs-lookup"><span data-stu-id="5f755-133">hello prerequisites include:</span></span>

-   <span data-ttu-id="5f755-134">Jste nastavili [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), a připojení uživatelů počítačů tooyour domény.</span><span class="sxs-lookup"><span data-stu-id="5f755-134">You have set up [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), and you have joined your users' machines tooyour domain.</span></span>

-   <span data-ttu-id="5f755-135">Musíte mít hello "Upravit nastavení" oprávnění tooedit hello objekt zásad skupiny (GPO).</span><span class="sxs-lookup"><span data-stu-id="5f755-135">You must have hello "Edit settings" permission tooedit hello Group Policy Object (GPO).</span></span> <span data-ttu-id="5f755-136">Ve výchozím nastavení, toto oprávnění mají členové hello následující skupiny zabezpečení: Domain Administrators, Enterprise Administrators a Group Policy Creator Owners.</span><span class="sxs-lookup"><span data-stu-id="5f755-136">By default, members of hello following security groups have this permission: Domain Administrators, Enterprise Administrators, and Group Policy Creator Owners.</span></span> <span data-ttu-id="5f755-137">[Další informace](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).</span><span class="sxs-lookup"><span data-stu-id="5f755-137">[Learn more](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).</span></span>

<span data-ttu-id="5f755-138">Postupujte podle kurzu hello [jak tooDeploy hello rozšíření přístup k panelu pro Internet Explorer pomocí zásad skupiny](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-group-policy) pro podrobné pokyny, jak tooconfigure hello zásad skupiny a nasadit ji toousers.</span><span class="sxs-lookup"><span data-stu-id="5f755-138">Follow hello tutorial [How tooDeploy hello Access Panel Extension for Internet Explorer using Group Policy](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-group-policy) for step by step instructions on how tooconfigure hello group policy and deploy it toousers.</span></span>

## <a name="troubleshoot-hello-access-panel-in-internet-explorer"></a><span data-ttu-id="5f755-139">Řešení potíží s hello přístupového panelu v aplikaci Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="5f755-139">Troubleshoot hello Access Panel in Internet Explorer</span></span>

<span data-ttu-id="5f755-140">Postupujte podle hello [Poradce při potížích hello rozšíření panely přístup pro prohlížeč Internet Explorer](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-Troubleshoot) Průvodce pro přístup k nástroj pro diagnostiku a podrobné informace o konfiguraci hello rozšíření pro aplikaci Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="5f755-140">Follow hello [Troubleshoot hello Access Panel Extension for Internet Explorer](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-Troubleshoot) guide for access a diagnostics tool and step by step instructions on configuring hello extension for IE.</span></span>

## <a name="how-tooconfigure-password-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="5f755-141">Jak tooconfigure heslo jednotného přihlašování pro aplikace bez Galerie</span><span class="sxs-lookup"><span data-stu-id="5f755-141">How tooconfigure password single sign-on for a non-gallery application</span></span>

<span data-ttu-id="5f755-142">tooconfigure aplikaci z Galerie Azure AD hello budete muset:</span><span class="sxs-lookup"><span data-stu-id="5f755-142">tooconfigure an application from hello Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="5f755-143">Přidání aplikace bez Galerie</span><span class="sxs-lookup"><span data-stu-id="5f755-143">Add a non-gallery application</span></span>](#add-a-non-gallery-application)

-   [<span data-ttu-id="5f755-144">Konfigurace aplikace hello pro heslo jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5f755-144">Configure hello application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

-   [<span data-ttu-id="5f755-145">Přiřazení uživatelů toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="5f755-145">Assign users toohello application</span></span>](#assign-users-to-the-application)

### <a name="add-a-non-gallery-application"></a><span data-ttu-id="5f755-146">Přidání aplikace bez Galerie</span><span class="sxs-lookup"><span data-stu-id="5f755-146">Add a non-gallery application</span></span>

<span data-ttu-id="5f755-147">tooadd aplikace hello Galerie Azure AD, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="5f755-147">tooadd an application from hello Azure AD Gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="5f755-148">Otevřete hello [portálu Azure](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**</span><span class="sxs-lookup"><span data-stu-id="5f755-148">Open hello [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="5f755-149">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="5f755-149">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5f755-150">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="5f755-150">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5f755-151">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="5f755-151">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="5f755-152">Klikněte na tlačítko hello **přidat** tlačítko v pravém horním rohu hello na hello **podnikové aplikace, které** okno</span><span class="sxs-lookup"><span data-stu-id="5f755-152">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade</span></span>

6.  <span data-ttu-id="5f755-153">Klikněte na tlačítko **Galerie jiná aplikace.**</span><span class="sxs-lookup"><span data-stu-id="5f755-153">click **Non-gallery application.**</span></span>

7.  <span data-ttu-id="5f755-154">Zadejte název vaší aplikace hello v hello **název** textové pole.</span><span class="sxs-lookup"><span data-stu-id="5f755-154">Enter hello name of your application in hello **Name** textbox.</span></span> <span data-ttu-id="5f755-155">Vyberte **přidat.**</span><span class="sxs-lookup"><span data-stu-id="5f755-155">Select **Add.**</span></span>

<span data-ttu-id="5f755-156">Po krátké době být okno Konfigurace aplikací mít toosee hello.</span><span class="sxs-lookup"><span data-stu-id="5f755-156">After a short period, you be able toosee hello application’s configuration blade.</span></span>

### <a name="configure-hello-application-for-password-single-sign-on"></a><span data-ttu-id="5f755-157">Konfigurace aplikace hello pro heslo jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5f755-157">Configure hello application for password single sign-on</span></span>

<span data-ttu-id="5f755-158">tooconfigure jednotné přihlašování pro aplikace, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="5f755-158">tooconfigure single sign-on for an application, follow hello steps below:</span></span>

1.  <span data-ttu-id="5f755-159">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="5f755-159">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="5f755-160">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="5f755-160">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5f755-161">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="5f755-161">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5f755-162">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="5f755-162">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="5f755-163">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="5f755-163">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="5f755-164">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="5f755-164">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="5f755-165">Vyberte hello aplikaci tooconfigure jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5f755-165">Select hello application you want tooconfigure single sign-on</span></span>

7.  <span data-ttu-id="5f755-166">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="5f755-166">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="5f755-167">Vyberte hello režimu **založené na heslech přihlášení.**</span><span class="sxs-lookup"><span data-stu-id="5f755-167">Select hello mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="5f755-168">Zadejte hello **přihlašovací adresa URL**.</span><span class="sxs-lookup"><span data-stu-id="5f755-168">Enter hello **Sign-on URL**.</span></span> <span data-ttu-id="5f755-169">Toto je hello URL, kde uživatelé zadat toosign své uživatelské jméno a heslo v k.</span><span class="sxs-lookup"><span data-stu-id="5f755-169">This is hello URL where users enter their username and password toosign in to.</span></span> <span data-ttu-id="5f755-170">Zkontrolujte, zda jsou viditelné na adrese URL hello hello přihlášení pole.</span><span class="sxs-lookup"><span data-stu-id="5f755-170">Ensure hello sign in fields are visible at hello URL.</span></span>

10. <span data-ttu-id="5f755-171">Přiřazení uživatelů toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="5f755-171">Assign users toohello application.</span></span>

11. <span data-ttu-id="5f755-172">Kromě toho můžete taky zadat přihlašovací údaje jménem uživatele hello výběrem řádků hello hello uživatelů a kliknete na **pověření aktualizace** a zadáním jménem uživatelů hello hello uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="5f755-172">Additionally, you can also provide credentials on behalf of hello user by selecting hello rows of hello users and clicking on **Update Credentials** and entering hello username and password on behalf of hello users.</span></span> <span data-ttu-id="5f755-173">Uživatelé, jinak nebudou výzvami tooenter hello přihlašovací údaje sami při spuštění.</span><span class="sxs-lookup"><span data-stu-id="5f755-173">Otherwise, users be prompted tooenter hello credentials themselves upon launch.</span></span>

### <a name="assign-users-toohello-application"></a><span data-ttu-id="5f755-174">Přiřazení uživatelů toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="5f755-174">Assign users toohello application</span></span>

<span data-ttu-id="5f755-175">tooassign jeden nebo více uživatelů tooan aplikace přímo, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="5f755-175">tooassign one or more users tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="5f755-176">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="5f755-176">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="5f755-177">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="5f755-177">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="5f755-178">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="5f755-178">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="5f755-179">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="5f755-179">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="5f755-180">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="5f755-180">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="5f755-181">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="5f755-181">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="5f755-182">Vyberte aplikaci hello chcete tooassign seznam uživatele toofrom hello.</span><span class="sxs-lookup"><span data-stu-id="5f755-182">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="5f755-183">Po načtení hello aplikace, klikněte na **uživatelů a skupin** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="5f755-183">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="5f755-184">Klikněte na tlačítko hello **přidat** tlačítko nad hello **uživatelů a skupin** seznamu tooopen hello **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="5f755-184">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="5f755-185">Klikněte na tlačítko hello **uživatelů a skupin** selektor z hello **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="5f755-185">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="5f755-186">Typ v hello **celý název** nebo **e-mailová adresa** hello uživatele, které vás zajímají přiřazení do hello **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="5f755-186">Type in hello **full name** or **email address** of hello user you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="5f755-187">Pozastavte ukazatel myši nad hello **uživatele** v seznamu tooreveal hello **políčko**.</span><span class="sxs-lookup"><span data-stu-id="5f755-187">Hover over hello **user** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="5f755-188">Klikněte na profil hello políčko další toohello uživatele fotografie nebo logo tooadd vaše uživatele toohello **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="5f755-188">Click hello checkbox next toohello user’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="5f755-189">**Volitelné:** Pokud byste chtěli příliš**přidat více než jeden uživatel**, zadejte v jiném **celý název** nebo **e-mailová adresa** do hello **vyhledávání podle názvu nebo e-mailová adresa** pole pro vyhledávání a klikněte na zaškrtávací políčko tooadd hello tento uživatel toohello **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="5f755-189">**Optional:** If you would like too**add more than one user**, type in another **full name** or **email address** into hello **Search by name or email address** search box, and click hello checkbox tooadd this user toohello **Selected** list.</span></span>

13. <span data-ttu-id="5f755-190">Po dokončení výběru uživatelů klikněte na hello **vyberte** tlačítko tooadd je toohello seznam uživatelů a skupin toobe přiřazené toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="5f755-190">When you are finished selecting users, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="5f755-191">**Volitelné:** klikněte na tlačítko hello **vybrat roli** selektor v hello **přidat přiřazení** okno tooselect roli uživatele toohello tooassign jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="5f755-191">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello users you have selected.</span></span>

15. <span data-ttu-id="5f755-192">Klikněte na tlačítko hello **přiřadit** tlačítko tooassign hello aplikace toohello vybraných uživatelů.</span><span class="sxs-lookup"><span data-stu-id="5f755-192">Click hello **Assign** button tooassign hello application toohello selected users.</span></span>

<span data-ttu-id="5f755-193">Po krátké době hello uživatelů, které jste vybrali být schopný toolaunch tyto aplikace v hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="5f755-193">After a short period, hello users you have selected be able toolaunch these applications in hello Access Panel.</span></span>

## <a name="if-these-troubleshooting-steps-do-not-hello-resolve-hello-issue"></a><span data-ttu-id="5f755-194">Pokud tento postup řešení není hello hello problém vyřešte</span><span class="sxs-lookup"><span data-stu-id="5f755-194">If these troubleshooting steps do not hello resolve hello issue</span></span>

<span data-ttu-id="5f755-195">Otevřete lístek podpory se hello, pokud je k dispozici následující informace:</span><span class="sxs-lookup"><span data-stu-id="5f755-195">open a support ticket with hello following information if available:</span></span>

-   <span data-ttu-id="5f755-196">ID chyby korelace</span><span class="sxs-lookup"><span data-stu-id="5f755-196">Correlation error ID</span></span>

-   <span data-ttu-id="5f755-197">Hlavní název uživatele (e-mailovou adresu uživatele)</span><span class="sxs-lookup"><span data-stu-id="5f755-197">UPN (user email address)</span></span>

-   <span data-ttu-id="5f755-198">TenantID</span><span class="sxs-lookup"><span data-stu-id="5f755-198">TenantID</span></span>

-   <span data-ttu-id="5f755-199">Typ prohlížeče</span><span class="sxs-lookup"><span data-stu-id="5f755-199">Browser type</span></span>

-   <span data-ttu-id="5f755-200">Dojde k časové pásmo a čas nebo období při chybě</span><span class="sxs-lookup"><span data-stu-id="5f755-200">Time zone and time/timeframe during error occurs</span></span>

-   <span data-ttu-id="5f755-201">Fiddler trasování</span><span class="sxs-lookup"><span data-stu-id="5f755-201">Fiddler traces</span></span>

## <a name="next-steps"></a><span data-ttu-id="5f755-202">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5f755-202">Next steps</span></span>
[<span data-ttu-id="5f755-203">Zadejte tooyour přihlašování aplikací pomocí Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="5f755-203">Provide single sign-on tooyour apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)

