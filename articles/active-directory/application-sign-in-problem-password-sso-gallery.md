---
title: "aaaProblems přihlášení tooan aplikace Azure AD Galerie konfigurovaná pro federované jednotné přihlašování | Microsoft Docs"
description: "Jak tootroubleshoot problémy s aplikací Azure AD Galerie nakonfigurovaný pro heslo jednotné přihlašování"
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
ms.openlocfilehash: 7ba28765e1d1f13025d740790a2f87654ae0daed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="problems-signing-in-tooan-azure-ad-gallery-application-configured-for-federated-single-sign-on"></a><span data-ttu-id="6993d-103">Potíže s přihlášením tooan aplikace Azure AD Galerie konfigurovaná pro federované jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6993d-103">Problems signing in tooan Azure AD Gallery application configured for federated single sign-on</span></span>

<span data-ttu-id="6993d-104">Hello přístupový Panel je, že je webový portál, který umožňuje uživatel, který má pracovní nebo školní účet v Azure Active Directory (Azure AD) tooview a spouštět cloudové aplikace tento správce hello Azure AD udělil přístup.</span><span class="sxs-lookup"><span data-stu-id="6993d-104">hello Access Panel is a web-based portal which enables a user who has a work or school account in Azure Active Directory (Azure AD) tooview and launch cloud-based applications that hello Azure AD administrator has granted them access to.</span></span> <span data-ttu-id="6993d-105">Uživatel, který má edice Azure AD můžete také skupiny samoobslužné služby a možnosti správy aplikace prostřednictvím hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="6993d-105">A user who has Azure AD editions can also use self-service group and app management capabilities through hello Access Panel.</span></span> <span data-ttu-id="6993d-106">Hello přístupový Panel je oddělená od hello portál Azure a nevyžaduje, aby uživatelé toohave předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="6993d-106">hello Access Panel is separate from hello Azure portal and does not require users toohave an Azure subscription.</span></span>

<span data-ttu-id="6993d-107">toouse založené na heslech jednotné přihlašování (SSO) v hello přístupový Panel hello přístupový Panel rozšíření musí být nainstalován v prohlížeči hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="6993d-107">toouse password-based single sign-on (SSO) in hello Access Panel, hello Access Panel extension must be installed in hello user’s browser.</span></span> <span data-ttu-id="6993d-108">Toto rozšíření je stažen automaticky, když uživatel vybere aplikaci, která je nakonfigurována pro jednotné přihlašování založené na heslech.</span><span class="sxs-lookup"><span data-stu-id="6993d-108">This extension is downloaded automatically when a user selects an application that is configured for password-based SSO.</span></span>

## <a name="meeting-browser-requirements-for-hello-access-panel"></a><span data-ttu-id="6993d-109">Splňuje požadavky na prohlížeč pro hello přístupového panelu</span><span class="sxs-lookup"><span data-stu-id="6993d-109">Meeting browser requirements for hello Access Panel</span></span>

<span data-ttu-id="6993d-110">Hello přístupový Panel vyžaduje prohlížeč, který podporuje jazyk JavaScript a aktivoval šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="6993d-110">hello Access Panel requires a browser that supports JavaScript and has CSS enabled.</span></span> <span data-ttu-id="6993d-111">toouse založené na heslech jednotné přihlašování (SSO) v hello přístupový Panel hello přístupový Panel rozšíření musí být nainstalován v prohlížeči hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="6993d-111">toouse password-based single sign-on (SSO) in hello Access Panel, hello Access Panel extension must be installed in hello user’s browser.</span></span> <span data-ttu-id="6993d-112">Toto rozšíření je stažen automaticky, když uživatel vybere aplikaci, která je nakonfigurována pro jednotné přihlašování založené na heslech.</span><span class="sxs-lookup"><span data-stu-id="6993d-112">This extension is downloaded automatically when a user selects an application that is configured for password-based SSO.</span></span>

<span data-ttu-id="6993d-113">Pro jednotné přihlašování založené na heslech může být prohlížeče hello koncového uživatele:</span><span class="sxs-lookup"><span data-stu-id="6993d-113">For password-based SSO, hello end user’s browsers can be:</span></span>

-   <span data-ttu-id="6993d-114">Internet Explorer 8, 9, 10, 11 – Windows 7 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="6993d-114">Internet Explorer 8, 9, 10, 11 - on Windows 7 or later</span></span>

-   <span data-ttu-id="6993d-115">Chrome – v systému Windows 7 nebo novější a v systému MacOS X nebo novější</span><span class="sxs-lookup"><span data-stu-id="6993d-115">Chrome - on Windows 7 or later, and on MacOS X or later</span></span>

-   <span data-ttu-id="6993d-116">Firefox 26.0 nebo později - do systému Windows XP SP2 nebo novější a v systému Mac OS X 10,6 nebo novější</span><span class="sxs-lookup"><span data-stu-id="6993d-116">Firefox 26.0 or later - on Windows XP SP2 or later, and on Mac OS X 10.6 or later</span></span>

>[!NOTE]
><span data-ttu-id="6993d-117">rozšíření jednotného přihlašování založené na heslech Hello k dispozici pro Edge ve Windows 10, při rozšíření prohlížeče stát podporované pro okraj.</span><span class="sxs-lookup"><span data-stu-id="6993d-117">hello password-based SSO extension become available for Edge in Windows 10 when browser extensions become supported for Edge.</span></span>
>
>

## <a name="how-tooinstall-hello-access-panel-browser-extension"></a><span data-ttu-id="6993d-118">Jak tooinstall hello rozšíření přístup k panelu prohlížeče</span><span class="sxs-lookup"><span data-stu-id="6993d-118">How tooinstall hello Access Panel Browser extension</span></span>

<span data-ttu-id="6993d-119">tooinstall hello rozšíření prohlížeče panely přístup, postupujte podle kroků hello níže:</span><span class="sxs-lookup"><span data-stu-id="6993d-119">tooinstall hello Access Panel Browser extension, follow hello steps below:</span></span>

1.  <span data-ttu-id="6993d-120">Otevřete hello [přístupový Panel](https://myapps.microsoft.com) v jednom z hello podporované prohlížeče a přihlaste se jako **uživatele** ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6993d-120">Open hello [Access Panel](https://myapps.microsoft.com) in one of hello supported browsers and sign in as a **user** in your Azure AD.</span></span>

2.  <span data-ttu-id="6993d-121">Klikněte na tlačítko **SSO heslo aplikace** v hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="6993d-121">click a **password-SSO application** in hello Access Panel.</span></span>

3.  <span data-ttu-id="6993d-122">V hello výzva s dotazem tooinstall hello softwaru, vyberte **instalovat nyní**.</span><span class="sxs-lookup"><span data-stu-id="6993d-122">In hello prompt asking tooinstall hello software, select **Install Now**.</span></span>

4.  <span data-ttu-id="6993d-123">Založené na prohlížeči, je možné směrovanou toohello odkaz ke stažení.</span><span class="sxs-lookup"><span data-stu-id="6993d-123">Based on your browser you be directed toohello download link.</span></span> <span data-ttu-id="6993d-124">**Přidat** hello rozšíření tooyour prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="6993d-124">**Add** hello extension tooyour browser.</span></span>

5.  <span data-ttu-id="6993d-125">Pokud prohlížeč zobrazí dotaz, vyberte tooeither **povolit** nebo **povolit** hello rozšíření.</span><span class="sxs-lookup"><span data-stu-id="6993d-125">If your browser asks, select tooeither **Enable** or **Allow** hello extension.</span></span>

6.  <span data-ttu-id="6993d-126">Po instalaci **restartujte** relaci prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="6993d-126">Once installed, **restart** your browser session.</span></span>

7.  <span data-ttu-id="6993d-127">Přihlaste se do hello přístupového panelu a zobrazit, pokud můžete **spusťte** aplikace heslo SSO</span><span class="sxs-lookup"><span data-stu-id="6993d-127">Sign in into hello Access Panel and see if you can **launch** your password-SSO applications</span></span>

<span data-ttu-id="6993d-128">Může také stáhnout hello rozšíření pro Chrome a Firefox z hello přímé odkazy níže:</span><span class="sxs-lookup"><span data-stu-id="6993d-128">You may also download hello extension for Chrome and Firefox from hello direct links below:</span></span>

-   [<span data-ttu-id="6993d-129">Rozšíření pro Chrome přístup panely</span><span class="sxs-lookup"><span data-stu-id="6993d-129">Chrome Access Panel Extension</span></span>](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [<span data-ttu-id="6993d-130">Rozšíření panelu Firefox přístup</span><span class="sxs-lookup"><span data-stu-id="6993d-130">Firefox Access Panel Extension</span></span>](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="setting-up-a-group-policy-for-internet-explorer"></a><span data-ttu-id="6993d-131">Nastavení zásad skupiny pro Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="6993d-131">Setting up a group policy for Internet Explorer</span></span>

<span data-ttu-id="6993d-132">Můžete nastavit zásady skupiny, které umožňují tooremotely instalace hello přístupový Panel rozšíření pro Internet Explorer na počítačích uživatelů.</span><span class="sxs-lookup"><span data-stu-id="6993d-132">You can setup a group policy that allow you tooremotely install hello Access Panel extension for Internet Explorer on your users' machines.</span></span>

<span data-ttu-id="6993d-133">Hello požadavky patří:</span><span class="sxs-lookup"><span data-stu-id="6993d-133">hello prerequisites include:</span></span>

-   <span data-ttu-id="6993d-134">Jste nastavili [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), a připojení uživatelů počítačů tooyour domény.</span><span class="sxs-lookup"><span data-stu-id="6993d-134">You have set up [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), and you have joined your users' machines tooyour domain.</span></span>

-   <span data-ttu-id="6993d-135">Musíte mít hello "Upravit nastavení" oprávnění tooedit hello objekt zásad skupiny (GPO).</span><span class="sxs-lookup"><span data-stu-id="6993d-135">You must have hello "Edit settings" permission tooedit hello Group Policy Object (GPO).</span></span> <span data-ttu-id="6993d-136">Ve výchozím nastavení, toto oprávnění mají členové hello následující skupiny zabezpečení: Domain Administrators, Enterprise Administrators a Group Policy Creator Owners.</span><span class="sxs-lookup"><span data-stu-id="6993d-136">By default, members of hello following security groups have this permission: Domain Administrators, Enterprise Administrators, and Group Policy Creator Owners.</span></span> <span data-ttu-id="6993d-137">[Další informace](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).</span><span class="sxs-lookup"><span data-stu-id="6993d-137">[Learn more](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).</span></span>

<span data-ttu-id="6993d-138">Postupujte podle kurzu hello [jak tooDeploy hello rozšíření přístup k panelu pro Internet Explorer pomocí zásad skupiny](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-group-policy) pro podrobné pokyny, jak tooconfigure hello zásad skupiny a nasadit ji toousers.</span><span class="sxs-lookup"><span data-stu-id="6993d-138">Follow hello tutorial [How tooDeploy hello Access Panel Extension for Internet Explorer using Group Policy](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-group-policy) for step by step instructions on how tooconfigure hello group policy and deploy it toousers.</span></span>

## <a name="troubleshoot-hello-access-panel-in-internet-explorer"></a><span data-ttu-id="6993d-139">Řešení potíží s hello přístupového panelu v aplikaci Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="6993d-139">Troubleshoot hello Access Panel in Internet Explorer</span></span>

<span data-ttu-id="6993d-140">Postupujte podle hello [Poradce při potížích hello rozšíření panely přístup pro prohlížeč Internet Explorer](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-Troubleshoot) Průvodce pro přístup k nástroj pro diagnostiku a podrobné informace o konfiguraci hello rozšíření pro aplikaci Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="6993d-140">Follow hello [Troubleshoot hello Access Panel Extension for Internet Explorer](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-Troubleshoot) guide for access a diagnostics tool and step by step instructions on configuring hello extension for IE.</span></span>

## <a name="how-tooconfigure-password-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="6993d-141">Jak tooconfigure heslo jednotného přihlašování pro aplikaci Galerie Azure AD</span><span class="sxs-lookup"><span data-stu-id="6993d-141">How tooconfigure password single sign-on for an Azure AD gallery application</span></span>

<span data-ttu-id="6993d-142">tooconfigure aplikaci z Galerie Azure AD hello budete muset:</span><span class="sxs-lookup"><span data-stu-id="6993d-142">tooconfigure an application from hello Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="6993d-143">Přidat aplikaci z Galerie Azure AD hello</span><span class="sxs-lookup"><span data-stu-id="6993d-143">Add an application from hello Azure AD gallery</span></span>](#_Add_an_application)

-   [<span data-ttu-id="6993d-144">Konfigurace aplikace hello pro heslo jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6993d-144">Configure hello application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

-   [<span data-ttu-id="6993d-145">Přiřazení uživatelů toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="6993d-145">Assign users toohello application</span></span>](#assign-users-to-the-application)

### <a name="add-an-application-from-hello-azure-ad-gallery"></a><span data-ttu-id="6993d-146">Přidat aplikaci z Galerie Azure AD hello</span><span class="sxs-lookup"><span data-stu-id="6993d-146">Add an application from hello Azure AD gallery</span></span>

<span data-ttu-id="6993d-147">tooadd aplikace hello Galerie Azure AD, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="6993d-147">tooadd an application from hello Azure AD Gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="6993d-148">Otevřete hello [portálu Azure](https://portal.azure.com) a přihlaste se jako **globálního správce** nebo **spolusprávce**</span><span class="sxs-lookup"><span data-stu-id="6993d-148">Open hello [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="6993d-149">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="6993d-149">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="6993d-150">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="6993d-150">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="6993d-151">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="6993d-151">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="6993d-152">Klikněte na tlačítko hello **přidat** tlačítko v pravém horním rohu hello na hello **podnikové aplikace, které** okno.</span><span class="sxs-lookup"><span data-stu-id="6993d-152">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="6993d-153">V hello **zadejte název** textbox z hello **přidat z Galerie hello** části, název typu hello aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="6993d-153">In hello **Enter a name** textbox from hello **Add from hello gallery** section, type hello name of hello application.</span></span>

7.  <span data-ttu-id="6993d-154">Vyberte hello aplikaci tooconfigure pro jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6993d-154">Select hello application you want tooconfigure for single sign-on.</span></span>

8.  <span data-ttu-id="6993d-155">Před přidáním hello aplikace, můžete změnit její název hello **název** textové pole.</span><span class="sxs-lookup"><span data-stu-id="6993d-155">Before adding hello application, you can change its name from hello **Name** textbox.</span></span>

9.  <span data-ttu-id="6993d-156">Klikněte na tlačítko **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="6993d-156">Click **Add** button, tooadd hello application.</span></span>

<span data-ttu-id="6993d-157">Po krátké době být okno Konfigurace aplikací mít toosee hello.</span><span class="sxs-lookup"><span data-stu-id="6993d-157">After a short period, you be able toosee hello application’s configuration blade.</span></span>

### <a name="configure-hello-application-for-password-single-sign-on"></a><span data-ttu-id="6993d-158">Konfigurace aplikace hello pro heslo jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6993d-158">Configure hello application for password single sign-on</span></span>

<span data-ttu-id="6993d-159">tooconfigure jednotné přihlašování pro aplikace, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="6993d-159">tooconfigure single sign-on for an application, follow hello steps below:</span></span>

1.  <span data-ttu-id="6993d-160">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**</span><span class="sxs-lookup"><span data-stu-id="6993d-160">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="6993d-161">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="6993d-161">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="6993d-162">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="6993d-162">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="6993d-163">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="6993d-163">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="6993d-164">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="6993d-164">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="6993d-165">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="6993d-165">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="6993d-166">Vyberte hello aplikaci tooconfigure jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6993d-166">Select hello application you want tooconfigure single sign-on</span></span>

7.  <span data-ttu-id="6993d-167">Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="6993d-167">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="6993d-168">Vyberte hello režimu **založené na heslech přihlášení.**</span><span class="sxs-lookup"><span data-stu-id="6993d-168">Select hello mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="6993d-169">[Přiřazení uživatelů s aplikací toohello](#_How_to_assign).</span><span class="sxs-lookup"><span data-stu-id="6993d-169">[Assign users toohello application](#_How_to_assign).</span></span>

10. <span data-ttu-id="6993d-170">Kromě toho můžete taky zadat přihlašovací údaje jménem uživatele hello výběrem řádků hello hello uživatelů a kliknete na **pověření aktualizace** a zadáním jménem uživatelů hello hello uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="6993d-170">Additionally, you can also provide credentials on behalf of hello user by selecting hello rows of hello users and clicking on **Update Credentials** and entering hello username and password on behalf of hello users.</span></span> <span data-ttu-id="6993d-171">Uživatelé, jinak nebudou výzvami tooenter hello přihlašovací údaje sami při spuštění.</span><span class="sxs-lookup"><span data-stu-id="6993d-171">Otherwise, users be prompted tooenter hello credentials themselves upon launch.</span></span>

### <a name="assign-users-toohello-application"></a><span data-ttu-id="6993d-172">Přiřazení uživatelů toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="6993d-172">Assign users toohello application</span></span>

<span data-ttu-id="6993d-173">tooassign jeden nebo více uživatelů tooan aplikace přímo, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="6993d-173">tooassign one or more users tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="6993d-174">Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**</span><span class="sxs-lookup"><span data-stu-id="6993d-174">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="6993d-175">Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="6993d-175">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="6993d-176">Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.</span><span class="sxs-lookup"><span data-stu-id="6993d-176">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="6993d-177">Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="6993d-177">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="6993d-178">Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.</span><span class="sxs-lookup"><span data-stu-id="6993d-178">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="6993d-179">Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**</span><span class="sxs-lookup"><span data-stu-id="6993d-179">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="6993d-180">Vyberte aplikaci hello chcete tooassign seznam uživatele toofrom hello.</span><span class="sxs-lookup"><span data-stu-id="6993d-180">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="6993d-181">Po načtení hello aplikace, klikněte na **uživatelů a skupin** z aplikace hello levém navigační nabídky.</span><span class="sxs-lookup"><span data-stu-id="6993d-181">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="6993d-182">Klikněte na tlačítko hello **přidat** tlačítko nad hello **uživatelů a skupin** seznamu tooopen hello **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="6993d-182">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="6993d-183">Klikněte na tlačítko hello **uživatelů a skupin** selektor z hello **přidat přiřazení** okno.</span><span class="sxs-lookup"><span data-stu-id="6993d-183">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="6993d-184">Typ v hello **celý název** nebo **e-mailová adresa** hello uživatele, které vás zajímají přiřazení do hello **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="6993d-184">Type in hello **full name** or **email address** of hello user you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="6993d-185">Pozastavte ukazatel myši nad hello **uživatele** v seznamu tooreveal hello **políčko**.</span><span class="sxs-lookup"><span data-stu-id="6993d-185">Hover over hello **user** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="6993d-186">Klikněte na profil hello políčko další toohello uživatele fotografie nebo logo tooadd vaše uživatele toohello **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="6993d-186">Click hello checkbox next toohello user’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="6993d-187">**Volitelné:** Pokud byste chtěli příliš**přidat více než jeden uživatel**, zadejte v jiném **celý název** nebo **e-mailová adresa** do hello **vyhledávání podle názvu nebo e-mailová adresa** pole pro vyhledávání a klikněte na zaškrtávací políčko tooadd hello tento uživatel toohello **vybrané** seznamu.</span><span class="sxs-lookup"><span data-stu-id="6993d-187">**Optional:** If you would like too**add more than one user**, type in another **full name** or **email address** into hello **Search by name or email address** search box, and click hello checkbox tooadd this user toohello **Selected** list.</span></span>

13. <span data-ttu-id="6993d-188">Po dokončení výběru uživatelů klikněte na hello **vyberte** tlačítko tooadd je toohello seznam uživatelů a skupin toobe přiřazené toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="6993d-188">When you are finished selecting users, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="6993d-189">**Volitelné:** klikněte na tlačítko hello **vybrat roli** selektor v hello **přidat přiřazení** okno tooselect roli uživatele toohello tooassign jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="6993d-189">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello users you have selected.</span></span>

15. <span data-ttu-id="6993d-190">Klikněte na tlačítko hello **přiřadit** tlačítko tooassign hello aplikace toohello vybraných uživatelů.</span><span class="sxs-lookup"><span data-stu-id="6993d-190">Click hello **Assign** button tooassign hello application toohello selected users.</span></span>

<span data-ttu-id="6993d-191">Po krátké době hello uživatelů, které jste vybrali být schopný toolaunch tyto aplikace v hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="6993d-191">After a short period, hello users you have selected be able toolaunch these applications in hello Access Panel.</span></span>

## <a name="if-these-troubleshoot-steps-dont-resolve-hello-issue"></a><span data-ttu-id="6993d-192">Pokud toto řešení potíží s kroky nevyřeší problém hello</span><span class="sxs-lookup"><span data-stu-id="6993d-192">If these troubleshoot steps don't resolve hello issue</span></span> 
<span data-ttu-id="6993d-193">Otevřete lístek podpory se hello, pokud je k dispozici následující informace:</span><span class="sxs-lookup"><span data-stu-id="6993d-193">open a support ticket with hello following information if available:</span></span>

-   <span data-ttu-id="6993d-194">ID chyby korelace</span><span class="sxs-lookup"><span data-stu-id="6993d-194">Correlation error ID</span></span>

-   <span data-ttu-id="6993d-195">Hlavní název uživatele (e-mailovou adresu uživatele)</span><span class="sxs-lookup"><span data-stu-id="6993d-195">UPN (user email address)</span></span>

-   <span data-ttu-id="6993d-196">TenantID</span><span class="sxs-lookup"><span data-stu-id="6993d-196">TenantID</span></span>

-   <span data-ttu-id="6993d-197">Typ prohlížeče</span><span class="sxs-lookup"><span data-stu-id="6993d-197">Browser type</span></span>

-   <span data-ttu-id="6993d-198">Dojde k časové pásmo a čas nebo období při chybě</span><span class="sxs-lookup"><span data-stu-id="6993d-198">Time zone and time/timeframe during error occurs</span></span>

-   <span data-ttu-id="6993d-199">Fiddler trasování</span><span class="sxs-lookup"><span data-stu-id="6993d-199">Fiddler traces</span></span>

## <a name="next-steps"></a><span data-ttu-id="6993d-200">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6993d-200">Next steps</span></span>
[<span data-ttu-id="6993d-201">Zadejte tooyour přihlašování aplikací pomocí Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="6993d-201">Provide single sign-on tooyour apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)
