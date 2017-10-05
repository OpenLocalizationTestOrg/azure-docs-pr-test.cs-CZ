---
title: "Co je na přístupovém panelu v Azure Active Directory? | Dokumentace Microsoftu"
description: "Další informace o použití variace přístupový panel (webový prohlížeč, aplikace pro Android, aplikace iPhone a iPad) pro přístup k aplikacím SaaS."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: c0252d01-7e6e-4f79-a70e-600479577dfd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: markvi
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bd9066c251188c0f18fe1a9403baa2beaeeb987c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="what-is-the-access-panel"></a><span data-ttu-id="29c09-104">Co je na přístupovém panelu?</span><span class="sxs-lookup"><span data-stu-id="29c09-104">What is the access panel?</span></span>

<span data-ttu-id="29c09-105">Přístupový panel je webový portál.</span><span class="sxs-lookup"><span data-stu-id="29c09-105">The access panel is a web-based portal.</span></span> <span data-ttu-id="29c09-106">Umožňuje, aby uživatele, který má pracovní nebo školní účet v Azure Active Directory k zobrazení a spuštění aplikace založené na cloudu správce Azure AD udělil je přístup k.</span><span class="sxs-lookup"><span data-stu-id="29c09-106">It enables a user with a work or school account in Azure Active Directory to view and start cloud-based applications an Azure AD administrator has granted them access to.</span></span> <span data-ttu-id="29c09-107">Můžete vytvořit také skupinu samoobslužné služby a možnosti správy aplikace prostřednictvím na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="29c09-107">You can also use self-service group and app management capabilities through the access panel.</span></span>

<span data-ttu-id="29c09-108">Přístupový panel je nezávislý na portálu Azure a nemá můžete mít předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="29c09-108">The access panel is separate from the Azure portal and does not you to have an Azure subscription.</span></span>

![Přístupový panel][1]

<span data-ttu-id="29c09-110">Přístupový panel umožňuje upravit některé z nastavení profilu, včetně možnosti:</span><span class="sxs-lookup"><span data-stu-id="29c09-110">The access panel enables you to edit some of your profile settings, including the ability to:</span></span>

- <span data-ttu-id="29c09-111">Změňte heslo přidružené k pracovní nebo školní účet</span><span class="sxs-lookup"><span data-stu-id="29c09-111">Change the password associated with a work or school account</span></span>

- <span data-ttu-id="29c09-112">Upravit nastavení resetování hesla</span><span class="sxs-lookup"><span data-stu-id="29c09-112">Edit password reset settings</span></span>

- <span data-ttu-id="29c09-113">Upravit nastavení kontaktů a předvoleb týkající se vícefaktorového ověřování (pro účty, které byly vyžaduje použití ho správce)</span><span class="sxs-lookup"><span data-stu-id="29c09-113">Edit contact and preference settings related to multi-factor authentication (for accounts that have been required to use it by an administrator)</span></span>

- <span data-ttu-id="29c09-114">Zobrazit účet, který podrobnosti, jako je například ID uživatele, alternativní e-mailu a mobile a office telefonní čísla a zařízení</span><span class="sxs-lookup"><span data-stu-id="29c09-114">View account details, such as user ID, alternate email, and mobile and office phone numbers, and devices</span></span>

- <span data-ttu-id="29c09-115">Zobrazit a spustit cloudové aplikace, které je správce Azure AD udělil přístup.</span><span class="sxs-lookup"><span data-stu-id="29c09-115">View and start cloud-based applications that the Azure AD administrator has granted them access to.</span></span> <span data-ttu-id="29c09-116">Další informace o panel přístupu z hlediska uživatelů najdete v tématu pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="29c09-116">For more information about the access panel from the users’ perspective, see Using the access panel.</span></span> 

- <span data-ttu-id="29c09-117">Samoobslužná správa skupin.</span><span class="sxs-lookup"><span data-stu-id="29c09-117">Self-manage groups.</span></span> <span data-ttu-id="29c09-118">Přesněji řečeno správce můžete vytvořit a spravovat skupiny zabezpečení a žádosti o členství ve skupinách zabezpečení ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="29c09-118">More specifically, the administrator can create and manage security groups and request security group memberships in Azure AD.</span></span> <span data-ttu-id="29c09-119">Další informace najdete v tématu [Samoobslužná správa skupin uživatelů ve službě Azure AD](active-directory-accessmanagement-self-service-group-management.md) a [spravovat vaše skupiny](active-directory-manage-groups.md).</span><span class="sxs-lookup"><span data-stu-id="29c09-119">For more information, see [Self-service group management for users in Azure AD](active-directory-accessmanagement-self-service-group-management.md) and [Manage your groups](active-directory-manage-groups.md).</span></span>




## <a name="accessing-the-access-panel"></a><span data-ttu-id="29c09-120">Přístup k přístupovému panelu</span><span class="sxs-lookup"><span data-stu-id="29c09-120">Accessing the access panel</span></span>

<span data-ttu-id="29c09-121">Když přejdete následující adresu URL ve webovém prohlížeči se můžete dostat na přístupovém panelu:`http://myapps.microsoft.com`</span><span class="sxs-lookup"><span data-stu-id="29c09-121">You can access the access panel by visiting the following URL in a web browser: `http://myapps.microsoft.com`</span></span>

<span data-ttu-id="29c09-122">Pokud máte vlastní branding nakonfigurovaný pro přihlašovací stránku, můžete načíst branding připojením domény vaší organizace na konec adresy URL:`http://myapps.microsoft.com/<your domain>.com`</span><span class="sxs-lookup"><span data-stu-id="29c09-122">If you have custom branding configured for your sign-in page, you can load this branding by appending your organization’s domain to the end of the URL: `http://myapps.microsoft.com/<your domain>.com`</span></span>

<span data-ttu-id="29c09-123">V takovém případě můžete použít libovolný název aktivní nebo ověřené domény, který byl nakonfigurován na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="29c09-123">In this case, you can use any active or verified domain name that has been configured in your Azure portal.</span></span>

![Název domény Wingtip Toys][2]  

<span data-ttu-id="29c09-125">Budete muset distribuovat adresu URL pro všechny uživatele, kteří se přihlašují k aplikacím, které jsou integrované s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="29c09-125">You need to distribute the URL to all users who will sign in to applications that are integrated with Azure AD.</span></span>

## <a name="authentication"></a><span data-ttu-id="29c09-126">Authentication</span><span class="sxs-lookup"><span data-stu-id="29c09-126">Authentication</span></span>

<span data-ttu-id="29c09-127">Pokud chcete dostat na přístupovém panelu, musíte být ověřeni přes pracovní nebo školní účet ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="29c09-127">To reach the access panel, you must be authenticated through a work or school account in Azure AD.</span></span> <span data-ttu-id="29c09-128">Vám mohou být ověřeni pro Azure AD přímo.</span><span class="sxs-lookup"><span data-stu-id="29c09-128">You can be authenticated to Azure AD directly.</span></span> <span data-ttu-id="29c09-129">Případně pokud organizace nakonfiguroval federace pomocí služby Active Directory Federation Services (AD FS) nebo jiných technologií, vám může být ověřen pomocí Windows Server Active Directory.</span><span class="sxs-lookup"><span data-stu-id="29c09-129">Alternatively, if an organization has configured federation by using Active Directory Federation Services (AD FS) or other technologies, you can be authenticated by Windows Server Active Directory.</span></span>

<span data-ttu-id="29c09-130">Pokud máte předplatné Azure nebo Office 365 a používáte portál Azure nebo aplikace Office 365, zobrazí se seznam aplikací bez přihlášení znovu.</span><span class="sxs-lookup"><span data-stu-id="29c09-130">If you have a subscription for Azure or Office 365 and you have been using the Azure portal or an Office 365 application, you can see the list of applications without signing-in again.</span></span> <span data-ttu-id="29c09-131">Pokud jste nejsou ověřené. zobrazí se výzva k přihlášení pomocí uživatelského jména a hesla pro váš účet ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="29c09-131">If you are are not authenticated you are prompted to sign in by using the username and password for your account in Azure AD.</span></span> <span data-ttu-id="29c09-132">Pokud vaše organizace má nakonfigurovali federaci, zadejte uživatelské jméno je dostačující.</span><span class="sxs-lookup"><span data-stu-id="29c09-132">If your organization has configured federation, typing the username is sufficient.</span></span>

<span data-ttu-id="29c09-133">Pokud jste se ověřili, můžete pracovat s aplikacemi, které správce má integrované s adresářem.</span><span class="sxs-lookup"><span data-stu-id="29c09-133">When you are authenticated, you can interact with the applications that your administrator has integrated with the directory.</span></span> <span data-ttu-id="29c09-134">Informace o integraci aplikací s Azure AD najdete v tématu [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="29c09-134">To learn how to integrate applications with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="web-browser-requirements"></a><span data-ttu-id="29c09-135">Požadavky na webový prohlížeč</span><span class="sxs-lookup"><span data-stu-id="29c09-135">Web browser requirements</span></span>

<span data-ttu-id="29c09-136">Minimálně na přístupovém panelu vyžaduje prohlížeč, který podporuje jazyk JavaScript a aktivoval šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="29c09-136">At a minimum, the access panel requires a browser that supports JavaScript and has CSS enabled.</span></span> <span data-ttu-id="29c09-137">Pro uživatele k přihlášení do aplikace prostřednictvím založené na heslech jednotné přihlašování (SSO) musí být nainstalován rozšíření přístup k panelu v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="29c09-137">For the user to be signed in to applications through password-based single sign-on (SSO), the access panel extension must be installed in your browser.</span></span> <span data-ttu-id="29c09-138">Rozšíření se stáhne automaticky, když vyberete aplikaci, která je nakonfigurována pro jednotné přihlašování založené na heslech.</span><span class="sxs-lookup"><span data-stu-id="29c09-138">The extension is downloaded automatically when you select an application that is configured for password-based SSO.</span></span>

<span data-ttu-id="29c09-139">Rozšíření přístup k panelu je aktuálně dostupné pro Internet Explorer 8 a novější, okraj a Chrome, Firefox prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="29c09-139">The access panel extension is currently available for Internet Explorer 8 and later, Edge, Chrome, and Firefox browsers.</span></span>

## <a name="mobile-app-support"></a><span data-ttu-id="29c09-140">Podpora mobilních aplikací</span><span class="sxs-lookup"><span data-stu-id="29c09-140">Mobile app support</span></span>

<span data-ttu-id="29c09-141">Tým služby Azure Active Directory publikuje mé mobilní aplikace aplikace.</span><span class="sxs-lookup"><span data-stu-id="29c09-141">The Azure Active Directory team publishes the my apps mobile app.</span></span> <span data-ttu-id="29c09-142">Při instalaci aplikace se můžete přihlásit pomocí hesla aplikace jednotného přihlašování na zařízení iOS a Android.</span><span class="sxs-lookup"><span data-stu-id="29c09-142">When you install the app, you can sign in to password-based SSO applications on iOS and Android devices.</span></span>

> [!NOTE]
> <span data-ttu-id="29c09-143">Můžete přihlásit do aplikace, které podporují federace se službou Azure AD (včetně Salesforce, Google Apps, Dropbox, pole, Concur, Workday, Office 365 a více než 70 dalších) prakticky libovolný webový prohlížeč na libovolném zařízení, bez nutnosti modul plug-in nebo mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="29c09-143">You can sign in to applications that support federation with Azure AD (including Salesforce, Google Apps, Dropbox, Box, Concur, Workday, Office 365, and more than 70 others) on virtually any web browser, on any device, without needing a plug-in or mobile app.</span></span> <span data-ttu-id="29c09-144">Všechny ostatní [přístup k panelu prostředí](https://myapps.microsoft.com/) také nevyžadují mé aplikace mobilní aplikace, který se má použít na mobilním zařízení.</span><span class="sxs-lookup"><span data-stu-id="29c09-144">All other [access panel experiences](https://myapps.microsoft.com/) do also not require the my apps mobile app to be used on a mobile device.</span></span>
>
>

### <a name="my-apps-for-android"></a><span data-ttu-id="29c09-145">Moje aplikace pro Android</span><span class="sxs-lookup"><span data-stu-id="29c09-145">My apps for Android</span></span>

<span data-ttu-id="29c09-146">Moje aplikace pro Android je podporována v jakékoli zařízení s Androidem se systémem Android verze 4.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="29c09-146">My apps for Android is supported on any Android device that is running Android version 4.1 and later.</span></span>  
<span data-ttu-id="29c09-147">Je k dispozici v [obchodu Google Play](https://play.google.com/store/apps/details?id=com.microsoft.myapps).</span><span class="sxs-lookup"><span data-stu-id="29c09-147">It is available in the [Google Play store](https://play.google.com/store/apps/details?id=com.microsoft.myapps).</span></span>

![Moje aplikace pro Android][3]   

### <a name="my-apps-for-iphone-and-ipad"></a><span data-ttu-id="29c09-149">Moje aplikace pro zařízení iPhone a iPad</span><span class="sxs-lookup"><span data-stu-id="29c09-149">My apps for iPhone and iPad</span></span>

<span data-ttu-id="29c09-150">Moje aplikace pro iOS je podporována na jakékoli zařízení iPhone nebo iPad, který používá verzi iOS 7 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="29c09-150">My apps for iOS is supported on any iPhone or iPad that is running iOS version 7 and later.</span></span>  
<span data-ttu-id="29c09-151">Je k dispozici v [Apple App Store](https://itunes.apple.com/us/app/my-apps-azure-active-directory/id824048653?mt=8).</span><span class="sxs-lookup"><span data-stu-id="29c09-151">It is available in the [Apple App Store](https://itunes.apple.com/us/app/my-apps-azure-active-directory/id824048653?mt=8).</span></span>

![Moje aplikace pro iOS][4]    



## <a name="managed-browser-for-my-apps"></a><span data-ttu-id="29c09-153">Spravovaný prohlížeč pro Moje aplikace</span><span class="sxs-lookup"><span data-stu-id="29c09-153">Managed browser for my apps</span></span>

<span data-ttu-id="29c09-154">Moje aplikace je taky integrovaná v Intune Managed Browser.</span><span class="sxs-lookup"><span data-stu-id="29c09-154">My apps is also integrated in the Intune Managed Browser.</span></span> <span data-ttu-id="29c09-155">Intune Managed Browser pro iOS a zařízení se systémem Android hraje důležitou roli v zajistíte, že jejího zabezpečení dat na mobilních zařízeních.</span><span class="sxs-lookup"><span data-stu-id="29c09-155">The Intune Managed Browser for iOS and Android devices plays a key role in ensuring that data on mobile devices stays secure.</span></span> <span data-ttu-id="29c09-156">Umožňuje bezpečně a přejděte na webové stránky, které může obsahovat informace o společnosti a poskytuje zabezpečené procházení webu prostředí.</span><span class="sxs-lookup"><span data-stu-id="29c09-156">It lets you safely view and navigate web pages that might contain company information, and provides a secure web-browsing experience.</span></span>  
<span data-ttu-id="29c09-157">Pro vás rychlý přístup k mé aplikace na domovské stránce Managed Browser a ve své záložky budete méně kliknutí k dosažení všech aplikací, které chcete získat přístup.</span><span class="sxs-lookup"><span data-stu-id="29c09-157">You find quick access to my apps on your Managed Browser homepage and in your bookmarks, giving you fewer clicks to reach any application you want to access.</span></span>

<span data-ttu-id="29c09-158">Je k dispozici v [Apple App Store](https://itunes.apple.com/us/app/microsoft-intune-managed-browser/id943264951?mt=8) a [Google Play Storu](https://play.google.com/store/apps/details?id=com.microsoft.intune.mam.managedbrowser&hl=en).</span><span class="sxs-lookup"><span data-stu-id="29c09-158">It is available in the [Apple App Store](https://itunes.apple.com/us/app/microsoft-intune-managed-browser/id943264951?mt=8) and [Google Play Store](https://play.google.com/store/apps/details?id=com.microsoft.intune.mam.managedbrowser&hl=en).</span></span>

![Mananged prohlížeče pro Moje aplikace][5]    





## <a name="tips-for-testing-the-user-experience"></a><span data-ttu-id="29c09-160">Tipy pro testování činnost koncového uživatele</span><span class="sxs-lookup"><span data-stu-id="29c09-160">Tips for testing the user experience</span></span>

<span data-ttu-id="29c09-161">Pokud jste správcem Azure a jste přihlášení k portálu Azure pomocí účtu v adresáři, budete automaticky přihlášení k přístupovému panelu jako aktuálního účtu.</span><span class="sxs-lookup"><span data-stu-id="29c09-161">If you are an Azure administrator and you are signed in to the Azure portal by using an account in the directory, you are automatically signed in to the access panel as your current account.</span></span> <span data-ttu-id="29c09-162">V takovém případě se zobrazí všechny aplikace, které vám byly přiřazeny.</span><span class="sxs-lookup"><span data-stu-id="29c09-162">In this case, you can see all applications that have been assigned to you.</span></span>

<span data-ttu-id="29c09-163">**Testování jako *různých* uživatelský účet:**</span><span class="sxs-lookup"><span data-stu-id="29c09-163">**To test as a *different* user account:**</span></span>

1. <span data-ttu-id="29c09-164">Klikněte na nabídku uživatele v pravém horním rohu portálu Azure nebo na přístupovém panelu a potom vyberte **Odhlásit**.</span><span class="sxs-lookup"><span data-stu-id="29c09-164">Click the user menu in the upper-right corner of the Azure portal or the access panel, and then select **Sign Out**.</span></span> 
2. <span data-ttu-id="29c09-165">Přejděte na [přístupový panel](http://myapps.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="29c09-165">Go to the [access panel](http://myapps.microsoft.com).</span></span>
3. <span data-ttu-id="29c09-166">Na stránce přihlášení zadejte uživatelské jméno a heslo pro účet ve vašem adresáři, kterou chcete otestovat.</span><span class="sxs-lookup"><span data-stu-id="29c09-166">On the sign-in page, type the username and password for the account in your directory you want to test.</span></span>


## <a name="starting-applications"></a><span data-ttu-id="29c09-167">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="29c09-167">Starting applications</span></span>

<span data-ttu-id="29c09-168">Několik typů aplikací, se může zobrazit na panel přístupu.</span><span class="sxs-lookup"><span data-stu-id="29c09-168">Several types of applications can appear on the access panel.</span></span>

### <a name="office-365-applications"></a><span data-ttu-id="29c09-169">Aplikace Office 365</span><span class="sxs-lookup"><span data-stu-id="29c09-169">Office 365 applications</span></span>

<span data-ttu-id="29c09-170">Pokud vaše organizace používá Office 365 aplikace a máte licenci pro ně, zobrazí se aplikace Office 365 na přístupový panel.</span><span class="sxs-lookup"><span data-stu-id="29c09-170">If your organization is using Office 365 applications and you are licensed for them, the Office 365 applications appear on your access panel.</span></span>

<span data-ttu-id="29c09-171">Když kliknete dlaždici aplikace pro aplikaci Office 365, je přesměrován do aplikace a automaticky přihlášení.</span><span class="sxs-lookup"><span data-stu-id="29c09-171">When you click an application tile for an Office 365 application, you are redirected to the application and automatically signed in.</span></span>

### <a name="microsoft-and-third-party-applications-configured-with-federation-based-sso"></a><span data-ttu-id="29c09-172">Společnost Microsoft a třetích stran aplikace, které jsou nakonfigurované pomocí jednotného přihlašování na základě federation</span><span class="sxs-lookup"><span data-stu-id="29c09-172">Microsoft and third-party applications configured with federation-based SSO</span></span>

<span data-ttu-id="29c09-173">Správce můžete přidat aplikace v oddílu služby Active Directory na portálu Azure, s režimem jednotné přihlašování, nastavte na **Azure AD jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="29c09-173">Your administrator can add applications in the Active Directory section of the Azure portal with the SSO mode set to **Azure AD Single Sign-On**.</span></span> <span data-ttu-id="29c09-174">Tyto aplikace lze zobrazit pouze pokud vám správce udělil explicitně přístup k aplikacím.</span><span class="sxs-lookup"><span data-stu-id="29c09-174">You can only see these applications if your administrator has explicitly granted you access to the applications.</span></span>

<span data-ttu-id="29c09-175">Když kliknete na dlaždici pro jednu z těchto aplikací, jsou přesměrování a automaticky přihlášení k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="29c09-175">When you click a tile for one of these applications, you are redirected and automatically signed in to the application.</span></span>

### <a name="password-based-sso-without-identity-provisioning"></a><span data-ttu-id="29c09-176">Založené na heslech jednotné přihlašování bez zřizování identity</span><span class="sxs-lookup"><span data-stu-id="29c09-176">Password-based SSO without identity provisioning</span></span>

<span data-ttu-id="29c09-177">Správce můžete přidat aplikace v oddílu služby Active Directory na portálu Azure, s režimem jednotné přihlašování, nastavte na **založené na heslech jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="29c09-177">Your administrator can add applications in the Active Directory section of the Azure portal with the SSO mode set to **Password-based Single Sign-On**.</span></span> <span data-ttu-id="29c09-178">Všechny aplikace, které jsou nakonfigurované v tomto režimu mohou prohlížet všichni uživatelé v adresáři.</span><span class="sxs-lookup"><span data-stu-id="29c09-178">All users in the directory can see all applications that have been configured in this mode.</span></span>

<span data-ttu-id="29c09-179">První kliknutí na tlačítko dlaždice pro jednu z těchto aplikací, zobrazí se výzva k instalaci modulu plug-in heslo jednotné přihlašování pro aplikace Internet Explorer nebo Chrome.</span><span class="sxs-lookup"><span data-stu-id="29c09-179">The first time, you click a tile for one of these applications, you are prompted to install the Password SSO plug-in for Internet Explorer or Chrome.</span></span> <span data-ttu-id="29c09-180">Instalace může vyžadovat restartování webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="29c09-180">The installation might require you to restart your web browser.</span></span> <span data-ttu-id="29c09-181">Po návratu do přístupového panelu a znovu klikněte na dlaždici aplikace, zobrazí se výzva k zadání uživatelského jména a hesla pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="29c09-181">When you return to the access panel and click the application tile again, you are prompted for a username and password for the application.</span></span> <span data-ttu-id="29c09-182">Když zadáte uživatelské jméno a heslo, tyto přihlašovací údaje jsou bezpečně uložené a propojený s vaším účtem ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="29c09-182">When you have entered your username and password, these credentials are securely stored and linked to your account in Azure AD.</span></span>

<span data-ttu-id="29c09-183">Při příštím kliknutí na dlaždici aplikace budete automaticky přihlášení k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="29c09-183">The next time you click the application tile, you are automatically signed in to the application.</span></span>  
<span data-ttu-id="29c09-184">Nemusíte znovu zadejte své přihlašovací údaje a nebo instalace modulu plug-in jednotné přihlašování heslo.</span><span class="sxs-lookup"><span data-stu-id="29c09-184">You don't have to enter your credentials again and or install the Password SSO plug-in.</span></span>

<span data-ttu-id="29c09-185">Pokud vaše přihlašovací údaje se změnila v cílové aplikace třetích stran, je nutné také aktualizovat svoje přihlašovací údaje, které jsou uložené ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="29c09-185">If your credentials have changed in the target third-party application, you must also update your credentials that are stored in Azure AD.</span></span> 

<span data-ttu-id="29c09-186">**Chcete-li aktualizovat přihlašovací údaje:**</span><span class="sxs-lookup"><span data-stu-id="29c09-186">**To update credentials:**</span></span>

1. <span data-ttu-id="29c09-187">Vyberte ikonu na dlaždici aplikace.</span><span class="sxs-lookup"><span data-stu-id="29c09-187">Select the icon on the application tile.</span></span>
2. <span data-ttu-id="29c09-188">Vyberte **aktualizovat přihlašovací údaje** opětovné zadání uživatelského jména a hesla pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="29c09-188">Select **update credentials** to reenter the username and password for the application.</span></span>


### <a name="password-based-sso-with-identity-provisioning"></a><span data-ttu-id="29c09-189">Založené na heslech jednotného přihlašování k při zřizování identity</span><span class="sxs-lookup"><span data-stu-id="29c09-189">Password-based SSO with identity provisioning</span></span>

<span data-ttu-id="29c09-190">Správce můžete přidat aplikace v **služby Active Directory** části portálu Azure s režimem jednotné přihlašování, nastavte na **založené na heslech jednotné přihlašování**, společně s zřizování identity.</span><span class="sxs-lookup"><span data-stu-id="29c09-190">Your administrator can add applications in the **Active Directory** section of the Azure portal with the SSO mode set to **Password-based Single Sign-On**, along with identity provisioning.</span></span>

<span data-ttu-id="29c09-191">Poprvé, klikněte dlaždici aplikace pro jednu z těchto aplikací, zobrazí se výzva k instalaci **heslo jednotného přihlašování k modulu plug-in pro aplikace Internet Explorer nebo Chrome**.</span><span class="sxs-lookup"><span data-stu-id="29c09-191">The first time, you click an application tile for one of these applications, you are prompted to install the **Password SSO plug-in for Internet Explorer or Chrome**.</span></span> <span data-ttu-id="29c09-192">Instalace může vyžadovat restartování webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="29c09-192">The installation might require you to restart your web browser.</span></span>  
<span data-ttu-id="29c09-193">Po návratu do přístupového panelu a znovu klikněte na dlaždici aplikace, budete automaticky přihlášení k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="29c09-193">When you return to the access panel and click the application tile again, you are automatically signed in to the application.</span></span>

<span data-ttu-id="29c09-194">Některé aplikace může vyžadovat změny hesla na první přihlášení.</span><span class="sxs-lookup"><span data-stu-id="29c09-194">Some applications might require you to change your password on the first sign-in.</span></span> <span data-ttu-id="29c09-195">Pokud vaše přihlašovací údaje se změnila v cílové aplikace třetích stran, je nutné také aktualizovat přihlašovací údaje, které jsou uložené ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="29c09-195">If your credentials have changed in the target third-party application, you must also update the credentials that are stored in Azure AD.</span></span> 

<span data-ttu-id="29c09-196">**Chcete-li aktualizovat přihlašovací údaje:**</span><span class="sxs-lookup"><span data-stu-id="29c09-196">**To update credentials:**</span></span>

1. <span data-ttu-id="29c09-197">Vyberte ikonu na dlaždici aplikace.</span><span class="sxs-lookup"><span data-stu-id="29c09-197">Select the icon on the application tile.</span></span>
2. <span data-ttu-id="29c09-198">Vyberte **aktualizovat přihlašovací údaje** opětovné zadání uživatelského jména a hesla pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="29c09-198">Select **update credentials** to reenter the username and password for the application.</span></span>


### <a name="application-with-existing-sso-solutions"></a><span data-ttu-id="29c09-199">Aplikace s stávajících řešení pro jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="29c09-199">Application with existing SSO solutions</span></span>

<span data-ttu-id="29c09-200">Konfigurace jednotného přihlašování pro aplikace, portál Azure poskytuje třetí možnost jako **existující jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="29c09-200">To configure SSO for an application, the Azure portal provides a third option called **Existing Single Sign-On**.</span></span> <span data-ttu-id="29c09-201">Tato možnost umožňuje správci vytvořit odkaz na aplikaci a umístěte ji na panel přístupu pro vybraného uživatele.</span><span class="sxs-lookup"><span data-stu-id="29c09-201">This option enables your administrator to create a link to an application and place it on the access panel for selected users.</span></span>

<span data-ttu-id="29c09-202">Například pokud aplikace je nakonfigurovaný k ověřování uživatelů pomocí služby AD FS 2.0, správce můžete použít **existující jednotné přihlašování** možnost pro vytvoření odkazu na ni na panel přístupu.</span><span class="sxs-lookup"><span data-stu-id="29c09-202">For example, if an application is configured to authenticate users by using AD FS 2.0, your administrator can use the **Existing Single Sign-On** option to create a link to it on the access panel.</span></span> <span data-ttu-id="29c09-203">Při přístupu k propojení, se ověřují pomocí služby AD FS 2.0 nebo jakoukoli existující jednotného přihlašování k řešení, které poskytuje aplikace.</span><span class="sxs-lookup"><span data-stu-id="29c09-203">When you access the link, you are authenticated through AD FS 2.0 or whatever existing SSO solution the application provides.</span></span>


## <a name="next-steps"></a><span data-ttu-id="29c09-204">Další kroky</span><span class="sxs-lookup"><span data-stu-id="29c09-204">Next steps</span></span>

- <span data-ttu-id="29c09-205">Pokud chcete zobrazit seznam všech témat, které se vztahují ke správě aplikací, najdete v článku [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md).</span><span class="sxs-lookup"><span data-stu-id="29c09-205">To see a list of all topics that are related to application management, see the [article index for application management in Azure Active Directory](active-directory-apps-index.md).</span></span>
 
- <span data-ttu-id="29c09-206">Informace o tom, jak integrovat SaaS aplikace do Azure AD, najdete v článku [seznam kurzů k integraci aplikací SaaS](active-directory-saas-tutorial-list.md).</span><span class="sxs-lookup"><span data-stu-id="29c09-206">To learn how to integrate a SaaS app into Azure AD, see the [list of tutorials on how to integrate SaaS apps](active-directory-saas-tutorial-list.md).</span></span>
 
- <span data-ttu-id="29c09-207">Další informace o správě aplikací s Azure AD, najdete v článku [Úvod k přístupu k jedné přihlašování a správu aplikace pomocí Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="29c09-207">To learn more about managing apps with Azure AD, see the [introduction to single sign-on and managing app access with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>
 
- <span data-ttu-id="29c09-208">Další informace o zřizování uživatelů najdete v tématu [automatizace zřizování uživatelů a rušení zajištění pro aplikace SaaS](active-directory-saas-app-provisioning.md).</span><span class="sxs-lookup"><span data-stu-id="29c09-208">To learn more about user provisioning, see [automate user provisioning and deprovisioning to SaaS applications](active-directory-saas-app-provisioning.md).</span></span>

<!--Image references-->
[1]: ./media/active-directory-saas-access-panel-introduction/01.png
[2]: ./media/active-directory-saas-access-panel-introduction/02.png
[3]: ./media/active-directory-saas-access-panel-introduction/03.png
[4]: ./media/active-directory-saas-access-panel-introduction/04.png
[5]: ./media/active-directory-saas-access-panel-introduction/05.png
