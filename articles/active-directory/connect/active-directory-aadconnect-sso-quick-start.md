---
title: "Azure AD Connect: Bezproblémové jednotné přihlašování – rychlý Start | Microsoft Docs"
description: "Tento článek popisuje, jak tooget pracovat s Azure Active Directory bezproblémové jednotné přihlašování."
services: active-directory
keywords: "Co je Azure AD Connect, instalace služby Active Directory, požadované součásti pro Azure AD, jednotné přihlašování, jednotné přihlašování"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: billmath
ms.openlocfilehash: 97d40ed41b3bfad9ff7e11ddbdb8de594ee85de1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-seamless-single-sign-on-quick-start"></a><span data-ttu-id="18e18-104">Azure Active Directory bezproblémové jednotné přihlašování: rychlý start</span><span class="sxs-lookup"><span data-stu-id="18e18-104">Azure Active Directory Seamless Single Sign-On: Quick start</span></span>

## <a name="how-toodeploy-seamless-sso"></a><span data-ttu-id="18e18-105">Jak toodeploy bezproblémové jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="18e18-105">How toodeploy Seamless SSO</span></span>

<span data-ttu-id="18e18-106">Azure Active Directory bezproblémové jednotné přihlašování (Azure AD bezproblémové jednotné přihlašování) automaticky přihlásí uživatele v když jsou v podnikové síti připojené tooyour firemních počítačů.</span><span class="sxs-lookup"><span data-stu-id="18e18-106">Azure Active Directory Seamless Single Sign-On (Azure AD Seamless SSO) automatically signs users in when they are on their corporate desktops connected tooyour corporate network.</span></span> <span data-ttu-id="18e18-107">Poskytuje snadný přístup vašich uživatelů tooyour cloudové aplikace bez nutnosti jakékoli další místní komponenty.</span><span class="sxs-lookup"><span data-stu-id="18e18-107">It provides your users easy access tooyour cloud-based applications without needing any additional on-premises components.</span></span>

>[!IMPORTANT]
><span data-ttu-id="18e18-108">funkce jednotného přihlašování bezproblémové Hello je aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="18e18-108">hello Seamless SSO feature is currently in preview.</span></span>

<span data-ttu-id="18e18-109">toodeploy bezproblémové jednotné přihlašování, budete potřebovat toofollow tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="18e18-109">toodeploy Seamless SSO, you need toofollow these steps:</span></span>

## <a name="step-1-check-prerequisites"></a><span data-ttu-id="18e18-110">Krok 1: Kontrola předpokladů</span><span class="sxs-lookup"><span data-stu-id="18e18-110">Step 1: Check prerequisites</span></span>

<span data-ttu-id="18e18-111">Ujistěte se, že hello následující požadované součásti jsou na místě:</span><span class="sxs-lookup"><span data-stu-id="18e18-111">Ensure that hello following prerequisites are in place:</span></span>

1. <span data-ttu-id="18e18-112">Nastavit server Azure AD Connect: Pokud používáte [předávací ověřování](active-directory-aadconnect-pass-through-authentication.md) jako způsob přihlášení, není nutná žádná další akce.</span><span class="sxs-lookup"><span data-stu-id="18e18-112">Set up your Azure AD Connect server: If you use [Pass-through Authentication](active-directory-aadconnect-pass-through-authentication.md) as your sign-in method, no further action is required.</span></span> <span data-ttu-id="18e18-113">Pokud používáte [synchronizaci hodnoty Hash hesla](active-directory-aadconnectsync-implement-password-synchronization.md) jako způsob přihlášení a pokud je brána firewall mezi Azure AD Connect a službou Azure AD, ověřte, že:</span><span class="sxs-lookup"><span data-stu-id="18e18-113">If you use [Password Hash Synchronization](active-directory-aadconnectsync-implement-password-synchronization.md) as your sign-in method, and if there is a firewall between Azure AD Connect and Azure AD, ensure that:</span></span>
- <span data-ttu-id="18e18-114">Používáte-li verze 1.1.484.0 nebo novější služby Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="18e18-114">You are using versions 1.1.484.0 or later of Azure AD Connect.</span></span>
- <span data-ttu-id="18e18-115">Azure AD Connect může komunikovat s `*.msappproxy.net` adresy URL a více port 443.</span><span class="sxs-lookup"><span data-stu-id="18e18-115">Azure AD Connect can communicate with `*.msappproxy.net` URLs and over port 443.</span></span> <span data-ttu-id="18e18-116">Tento požadavek se vztahuje pouze v případě, že povolíte funkci hello, ne pro skutečné uživatelská přihlášení.</span><span class="sxs-lookup"><span data-stu-id="18e18-116">This prerequisite is applicable only when you enable hello feature, not for actual user sign-ins.</span></span>
- <span data-ttu-id="18e18-117">Azure AD Connect provádět přímé připojení toohello IP [Azure datového centra rozsahy IP adres](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="18e18-117">Azure AD Connect can make direct IP connections toohello [Azure data center IP ranges](https://www.microsoft.com/download/details.aspx?id=41653).</span></span> <span data-ttu-id="18e18-118">Tento požadavek je opět použít, pouze v případě, že povolíte funkci hello.</span><span class="sxs-lookup"><span data-stu-id="18e18-118">Again, this prerequisite is applicable only when you enable hello feature.</span></span>
2. <span data-ttu-id="18e18-119">Potřebujete přihlašovací údaje správce domény pro každou doménovou strukturu AD synchronizovat tooAzure AD (přes Azure AD Connect) a pro uživatele, jehož chcete tooenable bezproblémové jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="18e18-119">You need Domain Administrator credentials for each AD forest that you synchronize tooAzure AD (using Azure AD Connect) and for whose users you want tooenable Seamless SSO.</span></span>

## <a name="step-2-enable-hello-feature"></a><span data-ttu-id="18e18-120">Krok 2: Povolení funkce hello</span><span class="sxs-lookup"><span data-stu-id="18e18-120">Step 2: Enable hello feature</span></span>

<span data-ttu-id="18e18-121">Bezproblémové jednotné přihlašování se dá nastavit pomocí [Azure AD Connect](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="18e18-121">Seamless SSO can be enabled using [Azure AD Connect](active-directory-aadconnect.md).</span></span>

<span data-ttu-id="18e18-122">Pokud se jedná o novou instalaci Azure AD Connect, zvolte hello [vlastní cesta](active-directory-aadconnect-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="18e18-122">If you are doing a fresh installation of Azure AD Connect, choose hello [custom installation path](active-directory-aadconnect-get-started-custom.md).</span></span> <span data-ttu-id="18e18-123">V hello "Přihlášení uživatele" stránka zkontrolujte možnost "Povolit jednotné přihlašování" hello.</span><span class="sxs-lookup"><span data-stu-id="18e18-123">At hello "User sign-in" page, check hello "Enable single sign on" option.</span></span>

![Azure AD Connect – přihlášení uživatele](./media/active-directory-aadconnect-sso/sso8.png)

<span data-ttu-id="18e18-125">Pokud již máte instalace služby Azure AD Connect, zvolte "Změna uživatele přihlašovací stránka" na Azure AD Connect a klikněte na tlačítko Další..</span><span class="sxs-lookup"><span data-stu-id="18e18-125">If you already have an installation of Azure AD Connect, choose "Change user sign-in page" on Azure AD Connect and click "Next".</span></span> <span data-ttu-id="18e18-126">Zkontrolujte možnost "Povolit jednotné přihlašování" hello.</span><span class="sxs-lookup"><span data-stu-id="18e18-126">Then check hello "Enable single sign on" option.</span></span>

![Azure AD Connect – přihlášení uživatele změn](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

<span data-ttu-id="18e18-128">Pomocí Průvodce hello nadále pokračujte, dokud nezískáte toohello "Povolit jednotné přihlašování" stránky.</span><span class="sxs-lookup"><span data-stu-id="18e18-128">Continue through hello wizard until you get toohello "Enable single sign on" page.</span></span> <span data-ttu-id="18e18-129">Zadejte přihlašovací údaje správce domény pro každou doménovou strukturu AD synchronizovat tooAzure AD (přes Azure AD Connect) a pro uživatele, jehož chcete tooenable bezproblémové jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="18e18-129">Provide Domain Administrator credentials for each AD forest that you synchronize tooAzure AD (using Azure AD Connect) and for whose users you want tooenable Seamless SSO.</span></span> 

<span data-ttu-id="18e18-130">Po dokončení Průvodce hello je povoleno bezproblémové jednotného přihlašování na klientovi.</span><span class="sxs-lookup"><span data-stu-id="18e18-130">After completion of hello wizard, Seamless SSO is enabled on your tenant.</span></span>

>[!NOTE]
> <span data-ttu-id="18e18-131">pověření správce domény Hello nejsou uložené ve službě Azure AD Connect nebo ve službě Azure AD, ale jsou pouze funkce použité tooenable hello.</span><span class="sxs-lookup"><span data-stu-id="18e18-131">hello Domain Administrator credentials are not stored in Azure AD Connect or in Azure AD, but are only used tooenable hello feature.</span></span>

<span data-ttu-id="18e18-132">Postupujte podle těchto pokynů tooverify, že je povoleno jednotné přihlašování bezproblémové správně:</span><span class="sxs-lookup"><span data-stu-id="18e18-132">Follow these instructions tooverify that you have enabled Seamless SSO correctly:</span></span>

1. <span data-ttu-id="18e18-133">Přihlaste se toohello [centra pro správu Azure Active Directory](https://aad.portal.azure.com) s přihlašovacími údaji hello globální správce pro vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="18e18-133">Sign in toohello [Azure Active Directory admin center](https://aad.portal.azure.com) with hello Global Administrator credentials for your tenant.</span></span>
2. <span data-ttu-id="18e18-134">Vyberte **Azure Active Directory** na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="18e18-134">Select **Azure Active Directory** on hello left-hand navigation.</span></span>
3. <span data-ttu-id="18e18-135">Vyberte **Azure AD Connect**.</span><span class="sxs-lookup"><span data-stu-id="18e18-135">Select **Azure AD Connect**.</span></span>
4. <span data-ttu-id="18e18-136">Ověřte, že hello **bezproblémové jednotné přihlašování** zobrazí jako **povoleno**.</span><span class="sxs-lookup"><span data-stu-id="18e18-136">Verify that hello **Seamless Single Sign-On** feature shows as **Enabled**.</span></span>

![Portál Azure – okno Azure AD Connect](./media/active-directory-aadconnect-sso/sso10.png)

## <a name="step-3-roll-out-hello-feature"></a><span data-ttu-id="18e18-138">Krok 3: Zavádět hello funkce</span><span class="sxs-lookup"><span data-stu-id="18e18-138">Step 3: Roll out hello feature</span></span>

<span data-ttu-id="18e18-139">tooroll hello funkce tooyour uživatelů, je nutné nastavení zóny intranetu tooadd dva Azure adresy URL AD (https://autologon.microsoftazuread-sso.com a https://aadg.windows.net.nsatc.net) toousers se prostřednictvím zásad skupiny ve službě Active Directory.</span><span class="sxs-lookup"><span data-stu-id="18e18-139">tooroll out hello feature tooyour users, you need tooadd two Azure AD URLs (https://autologon.microsoftazuread-sso.com and https://aadg.windows.net.nsatc.net) toousers' Intranet zone settings via Group Policy in Active Directory.</span></span>

>[!NOTE]
> <span data-ttu-id="18e18-140">Hello následující pokyny funguje jenom pro Internet Explorer a Google Chrome v systému Windows (pokud ji sdílí sadu adres URL důvěryhodných serverů s aplikací Internet Explorer).</span><span class="sxs-lookup"><span data-stu-id="18e18-140">hello following instructions only work for Internet Explorer and Google Chrome on Windows  (if it shares set of trusted site URLs with Internet Explorer).</span></span> <span data-ttu-id="18e18-141">Přečtěte si další části hello pokyny tooset Mozilla Firefox a Google Chrome na macu.</span><span class="sxs-lookup"><span data-stu-id="18e18-141">Read hello next section for instructions tooset up Mozilla Firefox and Google Chrome on Mac.</span></span>

### <a name="why-do-you-need-toomodify-users-intranet-zone-settings"></a><span data-ttu-id="18e18-142">Proč potřebujete nastavení zóny intranetu toomodify uživatelů?</span><span class="sxs-lookup"><span data-stu-id="18e18-142">Why do you need toomodify users' Intranet zone settings?</span></span>

<span data-ttu-id="18e18-143">Ve výchozím nastavení prohlížeč hello automaticky vypočítá hello správné zóny (Internetu nebo intranetu) z adresy URL.</span><span class="sxs-lookup"><span data-stu-id="18e18-143">By default, hello browser automatically calculates hello right zone (Internet or Intranet) from a URL.</span></span> <span data-ttu-id="18e18-144">Například http://contoso/ je namapované toohello zóny intranetu, zatímco http://intranet.contoso.com/ je namapované toohello internetové zóny (protože hello adresa URL obsahuje tečku).</span><span class="sxs-lookup"><span data-stu-id="18e18-144">For example, http://contoso/ is mapped toohello Intranet zone, whereas http://intranet.contoso.com/ is mapped toohello Internet zone (because hello URL contains a period).</span></span> <span data-ttu-id="18e18-145">Prohlížeče Neodesílat Kerberos lístky tooa koncového bodu cloudu - jako hello dvou Azure AD adres URL -, pokud jeho adresa URL je explicitně přidat zóny intranetu prohlížeče toohello.</span><span class="sxs-lookup"><span data-stu-id="18e18-145">Browsers don't send Kerberos tickets tooa cloud endpoint - like hello two Azure AD URLs - unless its URL is explicitly added toohello browser's Intranet zone.</span></span>

### <a name="detailed-steps"></a><span data-ttu-id="18e18-146">Podrobné kroky</span><span class="sxs-lookup"><span data-stu-id="18e18-146">Detailed steps</span></span>

1. <span data-ttu-id="18e18-147">Spusťte nástroj pro správu zásad skupiny hello.</span><span class="sxs-lookup"><span data-stu-id="18e18-147">Open hello Group Policy Management tool.</span></span>
2. <span data-ttu-id="18e18-148">Upravte hello zásad skupiny, která je použitá toosome nebo všech uživatelů.</span><span class="sxs-lookup"><span data-stu-id="18e18-148">Edit hello Group Policy that is applied toosome or all your users.</span></span> <span data-ttu-id="18e18-149">V tomto příkladu používáme hello **výchozí zásady domény**.</span><span class="sxs-lookup"><span data-stu-id="18e18-149">In this example, we use hello **Default Domain Policy**.</span></span>
3. <span data-ttu-id="18e18-150">Přejděte příliš**uživatele Konfigurace počítače\Šablony pro správu\Součásti systému Windows\Internet Internetu řízení Panel\Security stránky** a vyberte **lokality tooZone přiřazení seznamu**.</span><span class="sxs-lookup"><span data-stu-id="18e18-150">Navigate too**User Configuration\Administrative Templates\Windows Components\Internet Explorer\Internet Control Panel\Security Page** and select **Site tooZone Assignment List**.</span></span>
<span data-ttu-id="18e18-151">![Jednotné přihlašování](./media/active-directory-aadconnect-sso/sso6.png)</span><span class="sxs-lookup"><span data-stu-id="18e18-151">![Single sign-on](./media/active-directory-aadconnect-sso/sso6.png)</span></span>  
4. <span data-ttu-id="18e18-152">Povolit zásady hello a zadejte následující hodnoty (Azure AD adres URL které jsou předávány lístky protokolu Kerberos) hello a data (*1* označuje zóny intranetu) v dialogovém okně hello.</span><span class="sxs-lookup"><span data-stu-id="18e18-152">Enable hello policy, and enter hello following values (Azure AD URLs where Kerberos tickets are forwarded) and data (*1* indicates Intranet zone) in hello dialog box.</span></span>

        Value: https://autologon.microsoftazuread-sso.com
        Data: 1
        Value: https://aadg.windows.net.nsatc.net
        Data: 1
>[!NOTE]
> <span data-ttu-id="18e18-153">Pokud chcete toodisallow někteří uživatelé pomocí bezproblémové jednotné přihlašování – například pokud tito uživatelé se přihlašujete na sdílené veřejné terminály – nastavte hello předcházející hodnoty příliš*4*.</span><span class="sxs-lookup"><span data-stu-id="18e18-153">If you want toodisallow some users from using Seamless SSO - for instance, if these users are signing in on shared kiosks - set hello preceding values too*4*.</span></span> <span data-ttu-id="18e18-154">Tato akce přidá hello adresy URL Azure AD toohello zóny s omezeným přístupem a selže bezproblémové SSO celou dobu hello.</span><span class="sxs-lookup"><span data-stu-id="18e18-154">This action adds hello Azure AD URLs toohello Restricted Zone, and fails Seamless SSO all hello time.</span></span>

5. <span data-ttu-id="18e18-155">Klikněte na tlačítko **OK** a **OK** znovu.</span><span class="sxs-lookup"><span data-stu-id="18e18-155">Click **OK** and **OK** again.</span></span>

![Jednotné přihlašování](./media/active-directory-aadconnect-sso/sso7.png)

### <a name="browser-considerations"></a><span data-ttu-id="18e18-157">Důležité informace o prohlížeči</span><span class="sxs-lookup"><span data-stu-id="18e18-157">Browser considerations</span></span>

#### <a name="mozilla-firefox"></a><span data-ttu-id="18e18-158">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="18e18-158">Mozilla Firefox</span></span>

<span data-ttu-id="18e18-159">Mozilla Firefox automaticky neprovádí ověřování protokolu Kerberos.</span><span class="sxs-lookup"><span data-stu-id="18e18-159">Mozilla Firefox doesn't automatically do Kerberos Authentication.</span></span> <span data-ttu-id="18e18-160">Každý uživatel má toomanually přidat hello Azure AD adresy URL tootheir Firefox nastavení pomocí hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="18e18-160">Each user has toomanually add hello Azure AD URLs tootheir Firefox settings using hello following steps:</span></span>
1. <span data-ttu-id="18e18-161">Spustit Firefox a zadejte `about:config` v panelu Adresa hello.</span><span class="sxs-lookup"><span data-stu-id="18e18-161">Run Firefox and enter `about:config` in hello address bar.</span></span> <span data-ttu-id="18e18-162">Zavřete všechny oznámení, které vidíte.</span><span class="sxs-lookup"><span data-stu-id="18e18-162">Dismiss any notifications that you see.</span></span>
2. <span data-ttu-id="18e18-163">Vyhledejte hello **identifikátory URI network.negotiate auth.trusted** předvoleb.</span><span class="sxs-lookup"><span data-stu-id="18e18-163">Search for hello **network.negotiate-auth.trusted-uris** preference.</span></span> <span data-ttu-id="18e18-164">Tato předvolba obsahuje seznam důvěryhodných serverů na Firefox ověřování protokolem Kerberos.</span><span class="sxs-lookup"><span data-stu-id="18e18-164">This preference lists Firefox's trusted sites for Kerberos authentication.</span></span>
3. <span data-ttu-id="18e18-165">Klikněte pravým tlačítkem a vyberte "Upravit".</span><span class="sxs-lookup"><span data-stu-id="18e18-165">Right-click and select "Modify".</span></span>
4. <span data-ttu-id="18e18-166">Zadejte "https://autologon.microsoftazuread-sso.com, https://aadg.windows.net.nsatc.net" v poli hello.</span><span class="sxs-lookup"><span data-stu-id="18e18-166">Enter "https://autologon.microsoftazuread-sso.com, https://aadg.windows.net.nsatc.net" in hello field.</span></span>
5. <span data-ttu-id="18e18-167">Klikněte na tlačítko "OK" a znovu otevřete prohlížeč hello.</span><span class="sxs-lookup"><span data-stu-id="18e18-167">Click "OK" and reopen hello browser.</span></span>

#### <a name="safari-on-mac-os"></a><span data-ttu-id="18e18-168">Safari v systému Mac OS</span><span class="sxs-lookup"><span data-stu-id="18e18-168">Safari on Mac OS</span></span>

<span data-ttu-id="18e18-169">Zkontrolujte, že hello počítač se systémem Mac OS je připojený k tooAD.</span><span class="sxs-lookup"><span data-stu-id="18e18-169">Ensure that hello machine running Mac OS is joined tooAD.</span></span> <span data-ttu-id="18e18-170">Zobrazí pokyny o tom, toodo, [zde](http://training.apple.com/pdf/Best_Practices_for_Integrating_OS_X_with_Active_Directory.pdf).</span><span class="sxs-lookup"><span data-stu-id="18e18-170">See instructions on how toodo that [here](http://training.apple.com/pdf/Best_Practices_for_Integrating_OS_X_with_Active_Directory.pdf).</span></span>

#### <a name="google-chrome-on-mac-os"></a><span data-ttu-id="18e18-171">Google Chrome systému Mac OS</span><span class="sxs-lookup"><span data-stu-id="18e18-171">Google Chrome on Mac OS</span></span>

<span data-ttu-id="18e18-172">Google Chrome na ostatní platformy jiný systém než Windows a Mac OS, najdete v části příliš[v tomto článku](https://dev.chromium.org/administrators/policy-list-3#AuthServerWhitelist) informace o tom, jak toowhitelist hello adresy URL služby Azure AD pro integrované ověřování.</span><span class="sxs-lookup"><span data-stu-id="18e18-172">For Google Chrome on Mac OS and other non-Windows platforms, refer too[this article](https://dev.chromium.org/administrators/policy-list-3#AuthServerWhitelist) for information on how toowhitelist hello Azure AD URLs for integrated authentication.</span></span>

<span data-ttu-id="18e18-173">Pomocí zásad skupiny služby Active Directory třetích stran rozšíření tooroll tooFirefox hello adresy URL Azure AD a Google Chrome na uživatele počítačů Mac je mimo rozsah tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="18e18-173">Using third-party Active Directory Group Policy extensions tooroll out hello Azure AD URLs tooFirefox and Google Chrome on Mac users is outside of this article's scope.</span></span>

#### <a name="known-limitations"></a><span data-ttu-id="18e18-174">Známá omezení</span><span class="sxs-lookup"><span data-stu-id="18e18-174">Known limitations</span></span>

<span data-ttu-id="18e18-175">Bezproblémové SSO nefunguje v privátním režimu procházení na prohlížeče Firefox a okraj.</span><span class="sxs-lookup"><span data-stu-id="18e18-175">Seamless SSO doesn't work in private browsing mode on Firefox and Edge browsers.</span></span> <span data-ttu-id="18e18-176">Také nefunguje v Internet Exploreru Pokud hello prohlížeče běží v režimu rozšířené ochrany.</span><span class="sxs-lookup"><span data-stu-id="18e18-176">It also doesn't work on Internet Explorer if hello browser is running in Enhanced Protection mode.</span></span>

>[!IMPORTANT]
><span data-ttu-id="18e18-177">Jsme nedávno vrácena podporu pro hraniční tooinvestigate zákazníka hlášené problémy.</span><span class="sxs-lookup"><span data-stu-id="18e18-177">We recently rolled back support for Edge tooinvestigate customer-reported issues.</span></span>

## <a name="step-4-test-hello-feature"></a><span data-ttu-id="18e18-178">Krok 4: Test hello funkce</span><span class="sxs-lookup"><span data-stu-id="18e18-178">Step 4: Test hello feature</span></span>

<span data-ttu-id="18e18-179">Funkce hello tootest pro konkrétního uživatele, ujistěte se, že _všechny_ hello následující podmínky jsou splněné:</span><span class="sxs-lookup"><span data-stu-id="18e18-179">tootest hello feature for a specific user, ensure that _all_ hello following conditions are in place:</span></span>
  - <span data-ttu-id="18e18-180">Hello uživatel přihlašuje na firemní zařízení.</span><span class="sxs-lookup"><span data-stu-id="18e18-180">hello user is signing in on a corporate device.</span></span>
  - <span data-ttu-id="18e18-181">Hello zařízení bylo tooyour dříve připojené k doméně Active Directory (AD).</span><span class="sxs-lookup"><span data-stu-id="18e18-181">hello device has been previously joined tooyour Active Directory (AD) domain.</span></span>
  - <span data-ttu-id="18e18-182">Hello zařízení má tooyour přímé připojení k řadiči domény (DC), buď v hello podnikové pevné nebo bezdrátové síti nebo prostřednictvím připojení vzdáleného přístupu, jako je například připojení k síti VPN.</span><span class="sxs-lookup"><span data-stu-id="18e18-182">hello device has a direct connection tooyour Domain Controller (DC), either on hello corporate wired or wireless network or via a remote access connection, such as a VPN connection.</span></span>
  - <span data-ttu-id="18e18-183">Máte [nasazuje hello funkce](##step-3-roll-out-the-feature) toothis uživatele pomocí zásad skupiny.</span><span class="sxs-lookup"><span data-stu-id="18e18-183">You have [rolled out hello feature](##step-3-roll-out-the-feature) toothis user using Group Policy.</span></span>

<span data-ttu-id="18e18-184">scénář hello tootest kde hello uživatel zadá pouze hello uživatelské jméno, ale není hello heslo:</span><span class="sxs-lookup"><span data-stu-id="18e18-184">tootest hello scenario where hello user enters only hello username, but not hello password:</span></span>
- <span data-ttu-id="18e18-185">Přihlaste se k *https://myapps.microsoft.com/* v nové relaci privátní prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="18e18-185">Sign into *https://myapps.microsoft.com/* in a new private browser session.</span></span>

<span data-ttu-id="18e18-186">tootest hello scénář, kde uživatel hello nemá tooenter hello uživatelské jméno nebo hello heslo:</span><span class="sxs-lookup"><span data-stu-id="18e18-186">tootest hello scenario where hello user doesn't have tooenter hello username or hello password:</span></span> 
- <span data-ttu-id="18e18-187">Přihlaste se k *https://myapps.microsoft.com/contoso.onmicrosoft.com* v nové relaci privátní prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="18e18-187">Sign into *https://myapps.microsoft.com/contoso.onmicrosoft.com* in a new private browser session.</span></span> <span data-ttu-id="18e18-188">Nahraďte "*contoso*" s název vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="18e18-188">Replace "*contoso*" with your tenant's name.</span></span>
- <span data-ttu-id="18e18-189">Nebo se přihlaste *https://myapps.microsoft.com/contoso.com* v nové relaci privátní prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="18e18-189">Or sign into *https://myapps.microsoft.com/contoso.com* in a new private browser session.</span></span> <span data-ttu-id="18e18-190">Nahraďte "*contoso.com*" s ověřenou doménu (nikoli federované domény) ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="18e18-190">Replace "*contoso.com*" with a verified domain (not a federated domain) in your tenant.</span></span>

## <a name="step-5-roll-over-keys"></a><span data-ttu-id="18e18-191">Krok 5: Vrácení nad klíči</span><span class="sxs-lookup"><span data-stu-id="18e18-191">Step 5: Roll over keys</span></span>

<span data-ttu-id="18e18-192">V kroku 2 vytvoří Azure AD Connect účty počítačů (představující Azure AD) ve všech hello AD v doménových strukturách, na kterých jste povolili bezproblémové jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="18e18-192">In Step 2, Azure AD Connect creates computer accounts (representing Azure AD) in all hello AD forests on which you have enabled Seamless SSO.</span></span> <span data-ttu-id="18e18-193">Další informace v podrobněji [zde](active-directory-aadconnect-sso-how-it-works.md).</span><span class="sxs-lookup"><span data-stu-id="18e18-193">Learn more in detail [here](active-directory-aadconnect-sso-how-it-works.md).</span></span> <span data-ttu-id="18e18-194">Pro lepší zabezpečení se doporučuje, že můžete často mění hello protokolu Kerberos dešifrovací klíče tyto účty počítačů.</span><span class="sxs-lookup"><span data-stu-id="18e18-194">For improved security, it is recommended that  you frequently roll over hello Kerberos decryption keys of these computer accounts.</span></span>

>[!IMPORTANT]
><span data-ttu-id="18e18-195">Nepotřebujete toodo tento krok _okamžitě_ po povolení funkce hello.</span><span class="sxs-lookup"><span data-stu-id="18e18-195">You don't need toodo this step _immediately_ after you have enabled hello feature.</span></span> <span data-ttu-id="18e18-196">Mění hello protokolu Kerberos dešifrovací klíče minimálně každých 30 dnů.</span><span class="sxs-lookup"><span data-stu-id="18e18-196">Roll over hello Kerberos decryption keys at least every 30 days.</span></span>

## <a name="next-steps"></a><span data-ttu-id="18e18-197">Další kroky</span><span class="sxs-lookup"><span data-stu-id="18e18-197">Next steps</span></span>

- <span data-ttu-id="18e18-198">[**Podrobné technické informace** ](active-directory-aadconnect-sso-how-it-works.md) -pochopit, jak tato funkce funguje.</span><span class="sxs-lookup"><span data-stu-id="18e18-198">[**Technical Deep Dive**](active-directory-aadconnect-sso-how-it-works.md) - Understand how this feature works.</span></span>
- <span data-ttu-id="18e18-199">[**Nejčastější dotazy** ](active-directory-aadconnect-sso-faq.md) -odpovědi toofrequently dotazy.</span><span class="sxs-lookup"><span data-stu-id="18e18-199">[**Frequently Asked Questions**](active-directory-aadconnect-sso-faq.md) - Answers toofrequently asked questions.</span></span>
- <span data-ttu-id="18e18-200">[**Řešení potíží s** ](active-directory-aadconnect-troubleshoot-sso.md) – zjistěte, jak tooresolve běžné problémy s funkcí hello.</span><span class="sxs-lookup"><span data-stu-id="18e18-200">[**Troubleshoot**](active-directory-aadconnect-troubleshoot-sso.md) - Learn how tooresolve common issues with hello feature.</span></span>
- <span data-ttu-id="18e18-201">[**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – pro vyplnění žádosti o nové funkce.</span><span class="sxs-lookup"><span data-stu-id="18e18-201">[**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - For filing new feature requests.</span></span>
