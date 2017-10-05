---
title: "Azure AD Connect: Bezproblémové jednotné přihlašování – rychlý Start | Microsoft Docs"
description: "Tento článek popisuje, jak začít pracovat s Azure Active Directory bezproblémové jednotné přihlašování."
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
ms.openlocfilehash: 977108687734a5eb7f7a30419de2a6bdef184d0e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-seamless-single-sign-on-quick-start"></a><span data-ttu-id="1a1f6-104">Azure Active Directory bezproblémové jednotné přihlašování: rychlý start</span><span class="sxs-lookup"><span data-stu-id="1a1f6-104">Azure Active Directory Seamless Single Sign-On: Quick start</span></span>

## <a name="how-to-deploy-seamless-sso"></a><span data-ttu-id="1a1f6-105">Postup nasazení bezproblémové jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="1a1f6-105">How to deploy Seamless SSO</span></span>

<span data-ttu-id="1a1f6-106">Azure Active Directory bezproblémové jednotné přihlašování (Azure AD bezproblémové jednotné přihlašování) automaticky přihlásí uživatele v když jsou v jejich podnikových počítačů připojených k podnikové síti.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-106">Azure Active Directory Seamless Single Sign-On (Azure AD Seamless SSO) automatically signs users in when they are on their corporate desktops connected to your corporate network.</span></span> <span data-ttu-id="1a1f6-107">Poskytuje uživatelům snadný přístup k vaší cloudové aplikace bez nutnosti jakékoli další místní komponenty.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-107">It provides your users easy access to your cloud-based applications without needing any additional on-premises components.</span></span>

>[!IMPORTANT]
><span data-ttu-id="1a1f6-108">Funkci bezproblémové jednotného přihlašování je aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-108">The Seamless SSO feature is currently in preview.</span></span>

<span data-ttu-id="1a1f6-109">Chcete-li nasadit bezproblémové jednotné přihlašování, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="1a1f6-109">To deploy Seamless SSO, you need to follow these steps:</span></span>

## <a name="step-1-check-prerequisites"></a><span data-ttu-id="1a1f6-110">Krok 1: Kontrola předpokladů</span><span class="sxs-lookup"><span data-stu-id="1a1f6-110">Step 1: Check prerequisites</span></span>

<span data-ttu-id="1a1f6-111">Ujistěte se, že jsou splněné následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="1a1f6-111">Ensure that the following prerequisites are in place:</span></span>

1. <span data-ttu-id="1a1f6-112">Nastavit server Azure AD Connect: Pokud používáte [předávací ověřování](active-directory-aadconnect-pass-through-authentication.md) jako způsob přihlášení, není nutná žádná další akce.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-112">Set up your Azure AD Connect server: If you use [Pass-through Authentication](active-directory-aadconnect-pass-through-authentication.md) as your sign-in method, no further action is required.</span></span> <span data-ttu-id="1a1f6-113">Pokud používáte [synchronizaci hodnoty Hash hesla](active-directory-aadconnectsync-implement-password-synchronization.md) jako způsob přihlášení a pokud je brána firewall mezi Azure AD Connect a službou Azure AD, ověřte, že:</span><span class="sxs-lookup"><span data-stu-id="1a1f6-113">If you use [Password Hash Synchronization](active-directory-aadconnectsync-implement-password-synchronization.md) as your sign-in method, and if there is a firewall between Azure AD Connect and Azure AD, ensure that:</span></span>
- <span data-ttu-id="1a1f6-114">Používáte-li verze 1.1.484.0 nebo novější služby Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-114">You are using versions 1.1.484.0 or later of Azure AD Connect.</span></span>
- <span data-ttu-id="1a1f6-115">Azure AD Connect může komunikovat s `*.msappproxy.net` adresy URL a více port 443.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-115">Azure AD Connect can communicate with `*.msappproxy.net` URLs and over port 443.</span></span> <span data-ttu-id="1a1f6-116">Tento požadavek se vztahuje pouze v případě, že povolíte funkci, ne pro skutečné uživatelská přihlášení.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-116">This prerequisite is applicable only when you enable the feature, not for actual user sign-ins.</span></span>
- <span data-ttu-id="1a1f6-117">Azure AD Connect připojit přímo IP k [Azure datového centra rozsahy IP adres](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="1a1f6-117">Azure AD Connect can make direct IP connections to the [Azure data center IP ranges](https://www.microsoft.com/download/details.aspx?id=41653).</span></span> <span data-ttu-id="1a1f6-118">Tento požadavek je opět použít, pouze v případě, že povolíte funkci.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-118">Again, this prerequisite is applicable only when you enable the feature.</span></span>
2. <span data-ttu-id="1a1f6-119">Potřebujete přihlašovací údaje správce domény pro každou doménovou strukturu AD, která synchronizovat do Azure AD (přes Azure AD Connect) a pro uživatele, jehož chcete povolit bezproblémové jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-119">You need Domain Administrator credentials for each AD forest that you synchronize to Azure AD (using Azure AD Connect) and for whose users you want to enable Seamless SSO.</span></span>

## <a name="step-2-enable-the-feature"></a><span data-ttu-id="1a1f6-120">Krok 2: Povolení funkce</span><span class="sxs-lookup"><span data-stu-id="1a1f6-120">Step 2: Enable the feature</span></span>

<span data-ttu-id="1a1f6-121">Bezproblémové jednotné přihlašování se dá nastavit pomocí [Azure AD Connect](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="1a1f6-121">Seamless SSO can be enabled using [Azure AD Connect](active-directory-aadconnect.md).</span></span>

<span data-ttu-id="1a1f6-122">Pokud se jedná o novou instalaci Azure AD Connect, vyberte [vlastní cesta](active-directory-aadconnect-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="1a1f6-122">If you are doing a fresh installation of Azure AD Connect, choose the [custom installation path](active-directory-aadconnect-get-started-custom.md).</span></span> <span data-ttu-id="1a1f6-123">Na stránce "Přihlášení uživatele" a zaškrtněte možnost "Povolit jednotné přihlašování na".</span><span class="sxs-lookup"><span data-stu-id="1a1f6-123">At the "User sign-in" page, check the "Enable single sign on" option.</span></span>

![Azure AD Connect – přihlášení uživatele](./media/active-directory-aadconnect-sso/sso8.png)

<span data-ttu-id="1a1f6-125">Pokud již máte instalace služby Azure AD Connect, zvolte "Změna uživatele přihlašovací stránka" na Azure AD Connect a klikněte na tlačítko Další..</span><span class="sxs-lookup"><span data-stu-id="1a1f6-125">If you already have an installation of Azure AD Connect, choose "Change user sign-in page" on Azure AD Connect and click "Next".</span></span> <span data-ttu-id="1a1f6-126">Zkontrolujte možnost "Povolit jednotné přihlašování na".</span><span class="sxs-lookup"><span data-stu-id="1a1f6-126">Then check the "Enable single sign on" option.</span></span>

![Azure AD Connect – přihlášení uživatele změn](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

<span data-ttu-id="1a1f6-128">Postupujte podle pokynů Průvodce až na stránce "Povolit jednotné přihlašování na".</span><span class="sxs-lookup"><span data-stu-id="1a1f6-128">Continue through the wizard until you get to the "Enable single sign on" page.</span></span> <span data-ttu-id="1a1f6-129">Zadejte pověření správce domény pro každou doménovou strukturu AD, která synchronizovat do Azure AD (přes Azure AD Connect) a pro uživatele, jehož chcete povolit bezproblémové jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-129">Provide Domain Administrator credentials for each AD forest that you synchronize to Azure AD (using Azure AD Connect) and for whose users you want to enable Seamless SSO.</span></span> 

<span data-ttu-id="1a1f6-130">Po dokončení průvodce je povoleno bezproblémové jednotného přihlašování na klientovi.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-130">After completion of the wizard, Seamless SSO is enabled on your tenant.</span></span>

>[!NOTE]
> <span data-ttu-id="1a1f6-131">Přihlašovací údaje správce domény nejsou uložené ve službě Azure AD Connect nebo ve službě Azure AD, ale používají jenom k povolení této funkce.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-131">The Domain Administrator credentials are not stored in Azure AD Connect or in Azure AD, but are only used to enable the feature.</span></span>

<span data-ttu-id="1a1f6-132">Postupujte podle těchto pokynů k ověření, že je povoleno jednotné přihlašování bezproblémové správně:</span><span class="sxs-lookup"><span data-stu-id="1a1f6-132">Follow these instructions to verify that you have enabled Seamless SSO correctly:</span></span>

1. <span data-ttu-id="1a1f6-133">Přihlaste se k [centra pro správu Azure Active Directory](https://aad.portal.azure.com) pomocí přihlašovacích údajů globálního správce pro vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-133">Sign in to the [Azure Active Directory admin center](https://aad.portal.azure.com) with the Global Administrator credentials for your tenant.</span></span>
2. <span data-ttu-id="1a1f6-134">Vyberte **Azure Active Directory** na levé straně.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-134">Select **Azure Active Directory** on the left-hand navigation.</span></span>
3. <span data-ttu-id="1a1f6-135">Vyberte **Azure AD Connect**.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-135">Select **Azure AD Connect**.</span></span>
4. <span data-ttu-id="1a1f6-136">Ověřte, zda **bezproblémové jednotné přihlašování** zobrazí jako **povoleno**.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-136">Verify that the **Seamless Single Sign-On** feature shows as **Enabled**.</span></span>

![Portál Azure – okno Azure AD Connect](./media/active-directory-aadconnect-sso/sso10.png)

## <a name="step-3-roll-out-the-feature"></a><span data-ttu-id="1a1f6-138">Krok 3: Přejít na funkci</span><span class="sxs-lookup"><span data-stu-id="1a1f6-138">Step 3: Roll out the feature</span></span>

<span data-ttu-id="1a1f6-139">K zavedení funkci pro vaše uživatele, musíte přidat dva Azure AD adresy URL (https://autologon.microsoftazuread-sso.com a https://aadg.windows.net.nsatc.net) nastavení zóny intranetu uživatelů prostřednictvím zásad skupiny ve službě Active Directory.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-139">To roll out the feature to your users, you need to add two Azure AD URLs (https://autologon.microsoftazuread-sso.com and https://aadg.windows.net.nsatc.net) to users' Intranet zone settings via Group Policy in Active Directory.</span></span>

>[!NOTE]
> <span data-ttu-id="1a1f6-140">Podle následujících pokynů fungovat pouze pro Internet Explorer a Google Chrome v systému Windows (pokud ji sdílí sadu adres URL důvěryhodných serverů s aplikací Internet Explorer).</span><span class="sxs-lookup"><span data-stu-id="1a1f6-140">The following instructions only work for Internet Explorer and Google Chrome on Windows  (if it shares set of trusted site URLs with Internet Explorer).</span></span> <span data-ttu-id="1a1f6-141">Přečtěte si další části Pokyny k nastavení Mozilla Firefox a Google Chrome na macu.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-141">Read the next section for instructions to set up Mozilla Firefox and Google Chrome on Mac.</span></span>

### <a name="why-do-you-need-to-modify-users-intranet-zone-settings"></a><span data-ttu-id="1a1f6-142">Proč je třeba změnit nastavení zóny intranetu uživatelů?</span><span class="sxs-lookup"><span data-stu-id="1a1f6-142">Why do you need to modify users' Intranet zone settings?</span></span>

<span data-ttu-id="1a1f6-143">Ve výchozím prohlížeči automaticky vypočítá správné zóny (Internetu nebo intranetu) z adresy URL.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-143">By default, the browser automatically calculates the right zone (Internet or Intranet) from a URL.</span></span> <span data-ttu-id="1a1f6-144">Například http://contoso/ je namapována na zóny intranetu, zatímco http://intranet.contoso.com/ je namapovaný na zóně Internet, (protože adresa URL obsahuje tečku).</span><span class="sxs-lookup"><span data-stu-id="1a1f6-144">For example, http://contoso/ is mapped to the Intranet zone, whereas http://intranet.contoso.com/ is mapped to the Internet zone (because the URL contains a period).</span></span> <span data-ttu-id="1a1f6-145">Prohlížeče Neodesílat lístky protokolu Kerberos pro koncový bod cloudu – jako dvou Azure AD adres URL - pokud jeho adresa URL je přidané do zóny intranetu prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-145">Browsers don't send Kerberos tickets to a cloud endpoint - like the two Azure AD URLs - unless its URL is explicitly added to the browser's Intranet zone.</span></span>

### <a name="detailed-steps"></a><span data-ttu-id="1a1f6-146">Podrobné kroky</span><span class="sxs-lookup"><span data-stu-id="1a1f6-146">Detailed steps</span></span>

1. <span data-ttu-id="1a1f6-147">Spusťte nástroj pro správu zásad skupiny.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-147">Open the Group Policy Management tool.</span></span>
2. <span data-ttu-id="1a1f6-148">Upravit zásady skupiny, které se použije k některým nebo všem uživatelům.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-148">Edit the Group Policy that is applied to some or all your users.</span></span> <span data-ttu-id="1a1f6-149">V tomto příkladu používáme **výchozí zásady domény**.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-149">In this example, we use the **Default Domain Policy**.</span></span>
3. <span data-ttu-id="1a1f6-150">Přejděte na **uživatele Konfigurace počítače\Šablony pro správu\Součásti systému Windows\Internet Internetu řízení Panel\Security stránky** a vyberte **webu do zóny přiřazení seznamu**.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-150">Navigate to **User Configuration\Administrative Templates\Windows Components\Internet Explorer\Internet Control Panel\Security Page** and select **Site to Zone Assignment List**.</span></span>
<span data-ttu-id="1a1f6-151">![Jednotné přihlašování](./media/active-directory-aadconnect-sso/sso6.png)</span><span class="sxs-lookup"><span data-stu-id="1a1f6-151">![Single sign-on](./media/active-directory-aadconnect-sso/sso6.png)</span></span>  
4. <span data-ttu-id="1a1f6-152">Povolení zásad a zadejte následující hodnoty (které jsou předávány lístky protokolu Kerberos URL Azure AD) a data (*1* označuje zóny intranetu) v dialogovém okně.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-152">Enable the policy, and enter the following values (Azure AD URLs where Kerberos tickets are forwarded) and data (*1* indicates Intranet zone) in the dialog box.</span></span>

        Value: https://autologon.microsoftazuread-sso.com
        Data: 1
        Value: https://aadg.windows.net.nsatc.net
        Data: 1
>[!NOTE]
> <span data-ttu-id="1a1f6-153">Pokud chcete zakázat některé uživatele pomocí bezproblémové jednotné přihlašování – například pokud tito uživatelé se přihlašujete na sdílené veřejné terminály – nastavte předchozí hodnoty na *4*.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-153">If you want to disallow some users from using Seamless SSO - for instance, if these users are signing in on shared kiosks - set the preceding values to *4*.</span></span> <span data-ttu-id="1a1f6-154">Tato akce přidá do zóny s omezeným přístupem adresy URL Azure AD a selže vždy bezproblémové jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-154">This action adds the Azure AD URLs to the Restricted Zone, and fails Seamless SSO all the time.</span></span>

5. <span data-ttu-id="1a1f6-155">Klikněte na tlačítko **OK** a **OK** znovu.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-155">Click **OK** and **OK** again.</span></span>

![Jednotné přihlašování](./media/active-directory-aadconnect-sso/sso7.png)

### <a name="browser-considerations"></a><span data-ttu-id="1a1f6-157">Důležité informace o prohlížeči</span><span class="sxs-lookup"><span data-stu-id="1a1f6-157">Browser considerations</span></span>

#### <a name="mozilla-firefox"></a><span data-ttu-id="1a1f6-158">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="1a1f6-158">Mozilla Firefox</span></span>

<span data-ttu-id="1a1f6-159">Mozilla Firefox automaticky neprovádí ověřování protokolu Kerberos.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-159">Mozilla Firefox doesn't automatically do Kerberos Authentication.</span></span> <span data-ttu-id="1a1f6-160">Každý uživatel má Azure AD adresy URL. ručně přidat do jejich nastavení Firefox pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="1a1f6-160">Each user has to manually add the Azure AD URLs to their Firefox settings using the following steps:</span></span>
1. <span data-ttu-id="1a1f6-161">Spustit Firefox a zadejte `about:config` na panelu Adresa.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-161">Run Firefox and enter `about:config` in the address bar.</span></span> <span data-ttu-id="1a1f6-162">Zavřete všechny oznámení, které vidíte.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-162">Dismiss any notifications that you see.</span></span>
2. <span data-ttu-id="1a1f6-163">Vyhledejte **identifikátory URI network.negotiate auth.trusted** předvoleb.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-163">Search for the **network.negotiate-auth.trusted-uris** preference.</span></span> <span data-ttu-id="1a1f6-164">Tato předvolba obsahuje seznam důvěryhodných serverů na Firefox ověřování protokolem Kerberos.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-164">This preference lists Firefox's trusted sites for Kerberos authentication.</span></span>
3. <span data-ttu-id="1a1f6-165">Klikněte pravým tlačítkem a vyberte "Upravit".</span><span class="sxs-lookup"><span data-stu-id="1a1f6-165">Right-click and select "Modify".</span></span>
4. <span data-ttu-id="1a1f6-166">Zadejte "https://autologon.microsoftazuread-sso.com, https://aadg.windows.net.nsatc.net" v poli.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-166">Enter "https://autologon.microsoftazuread-sso.com, https://aadg.windows.net.nsatc.net" in the field.</span></span>
5. <span data-ttu-id="1a1f6-167">Klikněte na tlačítko "OK" a znovu otevřete v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-167">Click "OK" and reopen the browser.</span></span>

#### <a name="safari-on-mac-os"></a><span data-ttu-id="1a1f6-168">Safari v systému Mac OS</span><span class="sxs-lookup"><span data-stu-id="1a1f6-168">Safari on Mac OS</span></span>

<span data-ttu-id="1a1f6-169">Ujistěte se, že počítač se systémem Mac OS je připojený k AD.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-169">Ensure that the machine running Mac OS is joined to AD.</span></span> <span data-ttu-id="1a1f6-170">Zobrazí pokyny o tom, jak to udělat [zde](http://training.apple.com/pdf/Best_Practices_for_Integrating_OS_X_with_Active_Directory.pdf).</span><span class="sxs-lookup"><span data-stu-id="1a1f6-170">See instructions on how to do that [here](http://training.apple.com/pdf/Best_Practices_for_Integrating_OS_X_with_Active_Directory.pdf).</span></span>

#### <a name="google-chrome-on-mac-os"></a><span data-ttu-id="1a1f6-171">Google Chrome systému Mac OS</span><span class="sxs-lookup"><span data-stu-id="1a1f6-171">Google Chrome on Mac OS</span></span>

<span data-ttu-id="1a1f6-172">Google Chrome na ostatní platformy jiný systém než Windows a Mac OS, najdete v části [v tomto článku](https://dev.chromium.org/administrators/policy-list-3#AuthServerWhitelist) informace o tom, jak vytvořit bílou Azure AD adresy URL pro integrované ověřování.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-172">For Google Chrome on Mac OS and other non-Windows platforms, refer to [this article](https://dev.chromium.org/administrators/policy-list-3#AuthServerWhitelist) for information on how to whitelist the Azure AD URLs for integrated authentication.</span></span>

<span data-ttu-id="1a1f6-173">Pomocí rozšíření zásad skupiny služby Active Directory třetích stran k zavedení adresy URL Azure AD a Firefox, Google Chrome na uživatele počítačů Mac je mimo rozsah tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-173">Using third-party Active Directory Group Policy extensions to roll out the Azure AD URLs to Firefox and Google Chrome on Mac users is outside of this article's scope.</span></span>

#### <a name="known-limitations"></a><span data-ttu-id="1a1f6-174">Známá omezení</span><span class="sxs-lookup"><span data-stu-id="1a1f6-174">Known limitations</span></span>

<span data-ttu-id="1a1f6-175">Bezproblémové SSO nefunguje v privátním režimu procházení na prohlížeče Firefox a okraj.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-175">Seamless SSO doesn't work in private browsing mode on Firefox and Edge browsers.</span></span> <span data-ttu-id="1a1f6-176">Také nefunguje v Internet Exploreru Pokud prohlížeče běží v režimu rozšířené ochrany.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-176">It also doesn't work on Internet Explorer if the browser is running in Enhanced Protection mode.</span></span>

>[!IMPORTANT]
><span data-ttu-id="1a1f6-177">Jsme nedávno vrácena podporu pro hraniční síti, aby prozkoumat problémy hlášené zákazníka.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-177">We recently rolled back support for Edge to investigate customer-reported issues.</span></span>

## <a name="step-4-test-the-feature"></a><span data-ttu-id="1a1f6-178">Krok 4: Testování funkce</span><span class="sxs-lookup"><span data-stu-id="1a1f6-178">Step 4: Test the feature</span></span>

<span data-ttu-id="1a1f6-179">Chcete-li otestovat funkci pro konkrétního uživatele, ověřte, zda _všechny_ jsou splněné následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="1a1f6-179">To test the feature for a specific user, ensure that _all_ the following conditions are in place:</span></span>
  - <span data-ttu-id="1a1f6-180">Uživatel je přihlášení na firemní zařízení.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-180">The user is signing in on a corporate device.</span></span>
  - <span data-ttu-id="1a1f6-181">Zařízení byl dříve připojen k doméně služby Active Directory (AD).</span><span class="sxs-lookup"><span data-stu-id="1a1f6-181">The device has been previously joined to your Active Directory (AD) domain.</span></span>
  - <span data-ttu-id="1a1f6-182">Zařízení má přímé připojení k vaší řadiči domény (DC), buď v podnikové síti pevné nebo bezdrátové sítě nebo prostřednictvím připojení vzdáleného přístupu, jako je například připojení k síti VPN.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-182">The device has a direct connection to your Domain Controller (DC), either on the corporate wired or wireless network or via a remote access connection, such as a VPN connection.</span></span>
  - <span data-ttu-id="1a1f6-183">Máte [nasazuje funkci](##step-3-roll-out-the-feature) na tohoto uživatele pomocí zásad skupiny.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-183">You have [rolled out the feature](##step-3-roll-out-the-feature) to this user using Group Policy.</span></span>

<span data-ttu-id="1a1f6-184">K otestování tohoto scénáře, kde uživatel zadá pouze uživatelské jméno, ale ne heslo:</span><span class="sxs-lookup"><span data-stu-id="1a1f6-184">To test the scenario where the user enters only the username, but not the password:</span></span>
- <span data-ttu-id="1a1f6-185">Přihlaste se k *https://myapps.microsoft.com/* v nové relaci privátní prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-185">Sign into *https://myapps.microsoft.com/* in a new private browser session.</span></span>

<span data-ttu-id="1a1f6-186">K otestování tohoto scénáře, kde uživatel nemusí zadávat uživatelské jméno nebo heslo:</span><span class="sxs-lookup"><span data-stu-id="1a1f6-186">To test the scenario where the user doesn't have to enter the username or the password:</span></span> 
- <span data-ttu-id="1a1f6-187">Přihlaste se k *https://myapps.microsoft.com/contoso.onmicrosoft.com* v nové relaci privátní prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-187">Sign into *https://myapps.microsoft.com/contoso.onmicrosoft.com* in a new private browser session.</span></span> <span data-ttu-id="1a1f6-188">Nahraďte "*contoso*" s název vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-188">Replace "*contoso*" with your tenant's name.</span></span>
- <span data-ttu-id="1a1f6-189">Nebo se přihlaste *https://myapps.microsoft.com/contoso.com* v nové relaci privátní prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-189">Or sign into *https://myapps.microsoft.com/contoso.com* in a new private browser session.</span></span> <span data-ttu-id="1a1f6-190">Nahraďte "*contoso.com*" s ověřenou doménu (nikoli federované domény) ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-190">Replace "*contoso.com*" with a verified domain (not a federated domain) in your tenant.</span></span>

## <a name="step-5-roll-over-keys"></a><span data-ttu-id="1a1f6-191">Krok 5: Vrácení nad klíči</span><span class="sxs-lookup"><span data-stu-id="1a1f6-191">Step 5: Roll over keys</span></span>

<span data-ttu-id="1a1f6-192">V kroku 2 vytvoří Azure AD Connect účty počítačů (představující Azure AD) ve všech AD doménových strukturách, na kterých jste povolili bezproblémové jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-192">In Step 2, Azure AD Connect creates computer accounts (representing Azure AD) in all the AD forests on which you have enabled Seamless SSO.</span></span> <span data-ttu-id="1a1f6-193">Další informace v podrobněji [zde](active-directory-aadconnect-sso-how-it-works.md).</span><span class="sxs-lookup"><span data-stu-id="1a1f6-193">Learn more in detail [here](active-directory-aadconnect-sso-how-it-works.md).</span></span> <span data-ttu-id="1a1f6-194">Pro vyšší úroveň zabezpečení se doporučuje, aby často nezaregistrujete prostřednictvím protokolu Kerberos dešifrovací klíče tyto účty počítačů.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-194">For improved security, it is recommended that  you frequently roll over the Kerberos decryption keys of these computer accounts.</span></span>

>[!IMPORTANT]
><span data-ttu-id="1a1f6-195">Nemusíte proveďte tento krok _okamžitě_ poté, co povolíte funkci.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-195">You don't need to do this step _immediately_ after you have enabled the feature.</span></span> <span data-ttu-id="1a1f6-196">Mění dešifrovací klíče protokolu Kerberos minimálně každých 30 dnů.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-196">Roll over the Kerberos decryption keys at least every 30 days.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1a1f6-197">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1a1f6-197">Next steps</span></span>

- <span data-ttu-id="1a1f6-198">[**Podrobné technické informace** ](active-directory-aadconnect-sso-how-it-works.md) -pochopit, jak tato funkce funguje.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-198">[**Technical Deep Dive**](active-directory-aadconnect-sso-how-it-works.md) - Understand how this feature works.</span></span>
- <span data-ttu-id="1a1f6-199">[**Nejčastější dotazy** ](active-directory-aadconnect-sso-faq.md) -odpovědi na nejčastější dotazy.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-199">[**Frequently Asked Questions**](active-directory-aadconnect-sso-faq.md) - Answers to frequently asked questions.</span></span>
- <span data-ttu-id="1a1f6-200">[**Řešení potíží s** ](active-directory-aadconnect-troubleshoot-sso.md) – zjistěte, jak řešit obvyklé problémy s funkcí.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-200">[**Troubleshoot**](active-directory-aadconnect-troubleshoot-sso.md) - Learn how to resolve common issues with the feature.</span></span>
- <span data-ttu-id="1a1f6-201">[**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – pro vyplnění žádosti o nové funkce.</span><span class="sxs-lookup"><span data-stu-id="1a1f6-201">[**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - For filing new feature requests.</span></span>
