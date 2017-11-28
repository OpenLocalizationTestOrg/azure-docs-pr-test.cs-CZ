---
title: Active Directory Identity Protection playbook aaaAzure | Microsoft Docs
description: "Zjistěte, jak Azure AD Identity Protection vám umožní toolimit hello schopnost útočník tooexploit ohroženými identity nebo zařízení a toosecure identity nebo zařízení, která byla dříve toobe podezření nebo známých ohrožení zabezpečení."
services: active-directory
keywords: "ochrany identit Azure active directory, cloud app discovery,. Správa aplikací, zabezpečení, rizik, úroveň rizika, ohrožení zabezpečení, zásady zabezpečení"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 60836abf-f0e9-459d-b344-8e06b8341d25
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 6252bc133fc0c0f84800ee245a04bbf62d4cd25b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-identity-protection-playbook"></a><span data-ttu-id="d6298-104">Azure seznam strategií ochrany identit Active Directory</span><span class="sxs-lookup"><span data-stu-id="d6298-104">Azure Active Directory Identity Protection playbook</span></span>
<span data-ttu-id="d6298-105">Tato playbook vám pomůže:</span><span class="sxs-lookup"><span data-stu-id="d6298-105">This playbook helps you to:</span></span>

* <span data-ttu-id="d6298-106">Naplnění dat v prostředí Identity Protection hello simulace rizikových událostech a ohrožení zabezpečení</span><span class="sxs-lookup"><span data-stu-id="d6298-106">Populate data in hello Identity Protection environment by simulating risk events and vulnerabilities</span></span>
* <span data-ttu-id="d6298-107">Nastavit zásady podmíněného přístupu na základě riziko a testování hello vliv těchto zásad</span><span class="sxs-lookup"><span data-stu-id="d6298-107">Set up risk-based conditional access policies and test hello impact of these policies</span></span>

## <a name="simulating-risk-events"></a><span data-ttu-id="d6298-108">Simulaci rizikových událostí</span><span class="sxs-lookup"><span data-stu-id="d6298-108">Simulating Risk Events</span></span>
<span data-ttu-id="d6298-109">Tato část vám poskytne kroky pro simulaci hello následující typy událostí rizika:</span><span class="sxs-lookup"><span data-stu-id="d6298-109">This section provides you with steps for simulating hello following risk event types:</span></span>

* <span data-ttu-id="d6298-110">Přihlášení z anonymních IP adres (snadno)</span><span class="sxs-lookup"><span data-stu-id="d6298-110">Sign-ins from anonymous IP addresses (easy)</span></span>
* <span data-ttu-id="d6298-111">Přihlášení z neznámých míst (střední)</span><span class="sxs-lookup"><span data-stu-id="d6298-111">Sign-ins from unfamiliar locations (moderate)</span></span>
* <span data-ttu-id="d6298-112">Neuskutečnitelná cesta tooatypical umístění (obtížné)</span><span class="sxs-lookup"><span data-stu-id="d6298-112">Impossible travel tooatypical locations (difficult)</span></span>

<span data-ttu-id="d6298-113">Další události riziko nelze simulated zabezpečeným způsobem.</span><span class="sxs-lookup"><span data-stu-id="d6298-113">Other risk events cannot be simulated in a secure manner.</span></span>

### <a name="sign-ins-from-anonymous-ip-addresses"></a><span data-ttu-id="d6298-114">Přihlášení z anonymních IP adres</span><span class="sxs-lookup"><span data-stu-id="d6298-114">Sign-ins from anonymous IP addresses</span></span>
<span data-ttu-id="d6298-115">Tento typ události riziko identifikuje uživatele, kteří mají úspěšném přihlášení z IP adresy, která byla zjištěna jako IP adresa anonymního proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="d6298-115">This risk event type identifies users who have successfully signed in from an IP address that has been identified as an anonymous proxy IP address.</span></span> <span data-ttu-id="d6298-116">Tyto servery proxy jsou uživatelé, kteří mají toohide používá IP adresu své zařízení a mohou být použity pro zlými úmysly.</span><span class="sxs-lookup"><span data-stu-id="d6298-116">These proxies are used by people who want toohide their device’s IP address and may be used for malicious intent.</span></span>

<span data-ttu-id="d6298-117">**toosimulate přihlášení z anonymních IP adresy, proveďte následující kroky hello**:</span><span class="sxs-lookup"><span data-stu-id="d6298-117">**toosimulate a sign-in from an anonymous IP, perform hello following steps**:</span></span>

1. <span data-ttu-id="d6298-118">Stáhnout hello [Tor prohlížeče](https://www.torproject.org/projects/torbrowser.html.en).</span><span class="sxs-lookup"><span data-stu-id="d6298-118">Download hello [Tor Browser](https://www.torproject.org/projects/torbrowser.html.en).</span></span>
2. <span data-ttu-id="d6298-119">Pomocí hello Tor prohlížeče, přejděte příliš[https://myapps.microsoft.com](https://myapps.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="d6298-119">Using hello Tor Browser, navigate too[https://myapps.microsoft.com](https://myapps.microsoft.com).</span></span>   
3. <span data-ttu-id="d6298-120">Zadejte přihlašovací údaje hello hello účtu má tooappear v hello **přihlášení z anonymních IP adres** sestavy.</span><span class="sxs-lookup"><span data-stu-id="d6298-120">Enter hello credentials of hello account you want tooappear in hello **Sign-ins from anonymous IP addresses** report.</span></span>

<span data-ttu-id="d6298-121">přihlášení Hello se zobrazí na řídicím panelu Identity Protection hello během pěti minut.</span><span class="sxs-lookup"><span data-stu-id="d6298-121">hello sign-in will show up on hello Identity Protection dashboard within 5 minutes.</span></span> 

### <a name="sign-ins-from-unfamiliar-locations"></a><span data-ttu-id="d6298-122">Přihlášení z neznámých míst</span><span class="sxs-lookup"><span data-stu-id="d6298-122">Sign-ins from unfamiliar locations</span></span>
<span data-ttu-id="d6298-123">Hello neznámých míst riziko je v reálném čase vyhodnocení přihlášení mechanismus, který zvažuje po přihlášení umístění (IP, zeměpisné šířky nebo zeměpisnou délku a ASN) toodetermine nové / neznámého umístění.</span><span class="sxs-lookup"><span data-stu-id="d6298-123">hello unfamiliar locations risk is a real-time sign-in evaluation mechanism that considers past sign-in locations (IP, Latitude / Longitude and ASN) toodetermine new / unfamiliar locations.</span></span> <span data-ttu-id="d6298-124">systém Hello ukládá předchozí IP adresy, zeměpisné šířky nebo zeměpisnou délku a čísla ASN uživatele a zvažuje těchto toobe známé umístění.</span><span class="sxs-lookup"><span data-stu-id="d6298-124">hello system stores previous IPs, Latitude / Longitude, and ASNs of a user and considers these toobe familiar locations.</span></span> <span data-ttu-id="d6298-125">Umístění přihlášení je považován za obeznámeni, pokud hello přihlášení umístění neodpovídá žádnému z existující známé umístění hello.</span><span class="sxs-lookup"><span data-stu-id="d6298-125">A sign-in location is considered unfamiliar if hello sign-in location does not match any of hello existing familiar locations.</span></span>

<span data-ttu-id="d6298-126">Ochrany identit Azure Active Directory:</span><span class="sxs-lookup"><span data-stu-id="d6298-126">Azure Active Directory Identity Protection:</span></span>  

* <span data-ttu-id="d6298-127">má po počáteční learning dobu 14 dnů, za které není příznak jiných umístění jako neznámých míst.</span><span class="sxs-lookup"><span data-stu-id="d6298-127">has an initial learning period of 14 days during which it does not flag any new locations as unfamiliar locations.</span></span>
* <span data-ttu-id="d6298-128">ignoruje přihlášení ze známé zařízení a umístění, které jsou geograficky zavřít tooan existující známé umístění.</span><span class="sxs-lookup"><span data-stu-id="d6298-128">ignores sign-ins from familiar devices and locations that are geographically close tooan existing familiar location.</span></span>

<span data-ttu-id="d6298-129">toosimulate neznámých míst, máte v umístění a zařízení, která hello účet nebyl přihlášení z před toosign.</span><span class="sxs-lookup"><span data-stu-id="d6298-129">toosimulate unfamiliar locations, you have toosign in from a location and device that hello account has not signed in from before.</span></span> 

<span data-ttu-id="d6298-130">**toosimulate přihlášení z neznámého umístění, proveďte následující kroky hello**:</span><span class="sxs-lookup"><span data-stu-id="d6298-130">**toosimulate a sign-in from an unfamiliar location, perform hello following steps**:</span></span>

1. <span data-ttu-id="d6298-131">Vyberte účet, který má aspoň 14 dnů přihlášení historie.</span><span class="sxs-lookup"><span data-stu-id="d6298-131">Choose an account that has at least a 14-day sign-in history.</span></span> 
2. <span data-ttu-id="d6298-132">Proveďte:</span><span class="sxs-lookup"><span data-stu-id="d6298-132">Do either:</span></span>
   
   <span data-ttu-id="d6298-133">a.</span><span class="sxs-lookup"><span data-stu-id="d6298-133">a.</span></span> <span data-ttu-id="d6298-134">Při použití sítě VPN, přejděte příliš[https://myapps.microsoft.com](https://myapps.microsoft.com) a zadejte přihlašovací údaje hello hello účtu má toosimulate hello riziko událost pro.</span><span class="sxs-lookup"><span data-stu-id="d6298-134">While using a VPN, navigate too[https://myapps.microsoft.com](https://myapps.microsoft.com) and enter hello credentials of hello account you want toosimulate hello risk event for.</span></span>
   
   <span data-ttu-id="d6298-135">b.</span><span class="sxs-lookup"><span data-stu-id="d6298-135">b.</span></span> <span data-ttu-id="d6298-136">Požádejte přidružení v jiné umístění toosign pomocí pověření účtu hello (nedoporučuje se).</span><span class="sxs-lookup"><span data-stu-id="d6298-136">Ask an associate in a different location toosign in using hello account’s credentials (not recommended).</span></span>

<span data-ttu-id="d6298-137">přihlášení Hello se zobrazí na řídicím panelu Identity Protection hello během pěti minut.</span><span class="sxs-lookup"><span data-stu-id="d6298-137">hello sign-in will show up on hello Identity Protection dashboard within 5 minutes.</span></span>

### <a name="impossible-travel-tooatypical-location"></a><span data-ttu-id="d6298-138">Neuskutečnitelná cesta tooatypical umístění</span><span class="sxs-lookup"><span data-stu-id="d6298-138">Impossible travel tooatypical location</span></span>
<span data-ttu-id="d6298-139">Simulaci hello neuskutečnitelná cesta podmínka je složité, protože algoritmus hello používá machine learning tooweed na hodnotu false pozitivních třeba neuskutečnitelná cesta ze známé zařízení, nebo přihlášení z sítě VPN, které jsou používány jiných uživatelů v adresáři hello.</span><span class="sxs-lookup"><span data-stu-id="d6298-139">Simulating hello impossible travel condition is difficult because hello algorithm uses machine learning tooweed out false-positives such as impossible travel from familiar devices, or sign-ins from VPNs that are used by other users in hello directory.</span></span> <span data-ttu-id="d6298-140">Kromě toho hello algoritmus vyžaduje 3 dny too14 historii přihlášení pro uživatele hello před zahájením generování rizikových událostech.</span><span class="sxs-lookup"><span data-stu-id="d6298-140">Additionally, hello algorithm requires a sign-in history of 3 too14 days for hello user before it begins generating risk events.</span></span>

<span data-ttu-id="d6298-141">**toosimulate tooatypical na neuskutečnitelná cesta umístění provést následující kroky hello**:</span><span class="sxs-lookup"><span data-stu-id="d6298-141">**toosimulate an impossible travel tooatypical location, perform hello following steps**:</span></span>

1. <span data-ttu-id="d6298-142">Pomocí standardní prohlížeči přejděte příliš[https://myapps.microsoft.com](https://myapps.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="d6298-142">Using your standard browser, navigate too[https://myapps.microsoft.com](https://myapps.microsoft.com).</span></span>  
2. <span data-ttu-id="d6298-143">Zadejte přihlašovací údaje hello hello účet, který chcete toogenerate událost riziko neuskutečnitelná cesta pro.</span><span class="sxs-lookup"><span data-stu-id="d6298-143">Enter hello credentials of hello account you want toogenerate an impossible travel risk event for.</span></span>
3. <span data-ttu-id="d6298-144">Změna uživatelského agenta.</span><span class="sxs-lookup"><span data-stu-id="d6298-144">Change your user agent.</span></span> <span data-ttu-id="d6298-145">Můžete změnit uživatelský agent v aplikaci Internet Explorer z nástrojů pro vývojáře, nebo změnit váš uživatelský agent v Firefox nebo Chrome pomocí doplňku přepínači uživatelského agenta.</span><span class="sxs-lookup"><span data-stu-id="d6298-145">You can change user agent in Internet Explorer from Developer Tools, or change your user agent in Firefox or Chrome using a user-agent switcher add-on.</span></span>
4. <span data-ttu-id="d6298-146">Změníte IP adresu.</span><span class="sxs-lookup"><span data-stu-id="d6298-146">Change your IP address.</span></span> <span data-ttu-id="d6298-147">Můžete změnit IP adresu pomocí sítě VPN, rozšíření Tor nebo otáčí nový počítač v Azure v různých datových center.</span><span class="sxs-lookup"><span data-stu-id="d6298-147">You can change your IP address by using a VPN, a Tor add-on, or spinning up a new machine in Azure in a different data center.</span></span>
5. <span data-ttu-id="d6298-148">Přihlaste se příliš[https://myapps.microsoft.com](https://myapps.microsoft.com) pomocí stejných přihlašovacích údajů jako před a během několika minut po hello předchozí přihlášení hello.</span><span class="sxs-lookup"><span data-stu-id="d6298-148">Sign-in too[https://myapps.microsoft.com](https://myapps.microsoft.com) using hello same credentials as before and within a few minutes after hello previous sign-in.</span></span>

<span data-ttu-id="d6298-149">přihlášení Hello se zobrazí v řídicím panelu hello ochrany identit v rámci 2 – 4 hodiny.</span><span class="sxs-lookup"><span data-stu-id="d6298-149">hello sign-in will show up in hello Identity Protection dashboard within 2-4 hours.</span></span><br>
<span data-ttu-id="d6298-150">Z důvodu hello komplexní modelů strojového učení související se situací je možné, že se ho nebude získat převzata.</span><span class="sxs-lookup"><span data-stu-id="d6298-150">Because of hello complex machine learning models involved, there is a chance it will not get picked up.</span></span><br> <span data-ttu-id="d6298-151">Můžete chtít tooreplicate tyto kroky pro více účtů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d6298-151">You might want tooreplicate these steps for multiple Azure AD accounts.</span></span>

## <a name="simulating-vulnerabilities"></a><span data-ttu-id="d6298-152">Simulaci ohrožení zabezpečení</span><span class="sxs-lookup"><span data-stu-id="d6298-152">Simulating vulnerabilities</span></span>
<span data-ttu-id="d6298-153">Ohrožení zabezpečení jsou slabá místa v prostředí Azure AD, které může zneužít objektu actor chybný.</span><span class="sxs-lookup"><span data-stu-id="d6298-153">Vulnerabilities are weaknesses in an Azure AD environment that can be exploited by a bad actor.</span></span> <span data-ttu-id="d6298-154">Aktuálně jsou prezentované 3 typy ohrožení zabezpečení v Azure AD Identity Protection, které využít další funkce služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d6298-154">Currently 3 types of vulnerabilities are surfaced in Azure AD Identity Protection that leverage other features of Azure AD.</span></span> <span data-ttu-id="d6298-155">Tyto chyby zabezpečení se zobrazí na řídicím panelu Identity Protection hello automaticky po tyto funkce jsou nastavené.</span><span class="sxs-lookup"><span data-stu-id="d6298-155">These Vulnerabilities will be displayed on hello Identity Protection dashboard automatically once these features are set up.</span></span>

* <span data-ttu-id="d6298-156">Azure AD [služby Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)</span><span class="sxs-lookup"><span data-stu-id="d6298-156">Azure AD [Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)</span></span>
* <span data-ttu-id="d6298-157">Azure AD [Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d6298-157">Azure AD [Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).</span></span>
* <span data-ttu-id="d6298-158">Azure AD [Privileged Identity Management](active-directory-privileged-identity-management-configure.md).</span><span class="sxs-lookup"><span data-stu-id="d6298-158">Azure AD [Privileged Identity Management](active-directory-privileged-identity-management-configure.md).</span></span> 

## <a name="user-compromise-risk"></a><span data-ttu-id="d6298-159">Riziko ohrožení zabezpečení pro uživatele</span><span class="sxs-lookup"><span data-stu-id="d6298-159">User compromise risk</span></span>
<span data-ttu-id="d6298-160">**tootest riziko ohrožení zabezpečení pro uživatele, proveďte následující kroky hello**:</span><span class="sxs-lookup"><span data-stu-id="d6298-160">**tootest User compromise risk, perform hello following steps**:</span></span>

1. <span data-ttu-id="d6298-161">Přihlaste se příliš[https://portal.azure.com](https://portal.azure.com) pomocí přihlašovacích údajů globálního správce pro vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="d6298-161">Sign-in too[https://portal.azure.com](https://portal.azure.com) with global administrator credentials for your tenant.</span></span>
2. <span data-ttu-id="d6298-162">Přejděte příliš**Identity Protection**.</span><span class="sxs-lookup"><span data-stu-id="d6298-162">Navigate too**Identity Protection**.</span></span> 
3. <span data-ttu-id="d6298-163">Na hlavní hello **Azure AD Identity Protection** okně klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="d6298-163">On hello main **Azure AD Identity Protection** blade, click **Settings**.</span></span> 
4. <span data-ttu-id="d6298-164">Na hello **nastavení portálu** okno, v části **pravidla zabezpečení**, klikněte na tlačítko **riziko ohrožení zabezpečení uživatele**.</span><span class="sxs-lookup"><span data-stu-id="d6298-164">On hello **Portal Settings** blade, under **Security rules**, click **User compromise risk**.</span></span> 
5. <span data-ttu-id="d6298-165">Na hello **přihlášení riziko** okně zapnout **Povolit pravidlo** vypnout a potom klikněte na **Uložit** nastavení.</span><span class="sxs-lookup"><span data-stu-id="d6298-165">On hello **Sign in Risk** blade, turn **Enable rule** off, and then click **Save** settings.</span></span>
6. <span data-ttu-id="d6298-166">Pro daný uživatelský účet, simulovat neznámého umístění nebo anonymní IP riziko událost.</span><span class="sxs-lookup"><span data-stu-id="d6298-166">For a given user account, simulate an unfamiliar locations or anonymous IP risk event.</span></span> <span data-ttu-id="d6298-167">To bude zvýšit úroveň rizika hello uživatele pro daného uživatele příliš**střední**.</span><span class="sxs-lookup"><span data-stu-id="d6298-167">This will elevate hello user risk level for that user too**Medium**.</span></span>
7. <span data-ttu-id="d6298-168">Počkejte několik minut a ověřte, že úrovni uživatele pro vaše uživatele je **střední**.</span><span class="sxs-lookup"><span data-stu-id="d6298-168">Wait a few minutes, and then verify that user level for your user is **Medium**.</span></span>
8. <span data-ttu-id="d6298-169">Přejděte toohello **nastavení portálu** okno.</span><span class="sxs-lookup"><span data-stu-id="d6298-169">Go toohello **Portal Settings** blade.</span></span>
9. <span data-ttu-id="d6298-170">Na hello **riziko ohrožení zabezpečení uživatele** okno, v části **Povolit pravidlo**, vyberte **na** .</span><span class="sxs-lookup"><span data-stu-id="d6298-170">On hello **User Compromise Risk** blade, under **Enable rule**, select **On** .</span></span> 
10. <span data-ttu-id="d6298-171">Vyberte jednu z hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="d6298-171">Select one of hello following options:</span></span>
    
    <span data-ttu-id="d6298-172">a.</span><span class="sxs-lookup"><span data-stu-id="d6298-172">a.</span></span> <span data-ttu-id="d6298-173">tooblock, vyberte **střední** pod **bloku přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="d6298-173">tooblock, select **Medium** under **Block sign in**.</span></span>
    
    <span data-ttu-id="d6298-174">b.</span><span class="sxs-lookup"><span data-stu-id="d6298-174">b.</span></span> <span data-ttu-id="d6298-175">Změna tooenforce zabezpečeného hesla, vyberte **střední** pod **vyžadovat vícefaktorové ověřování**.</span><span class="sxs-lookup"><span data-stu-id="d6298-175">tooenforce secure password change, select **Medium** under **Require multi-factor authentication**.</span></span>
11. <span data-ttu-id="d6298-176">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d6298-176">Click **Save**.</span></span>
12. <span data-ttu-id="d6298-177">Nyní můžete otestovat podmíněného přístupu na základě riziko podle přihlášení pomocí uživatel s oprávněním vyšší úrovně rizika úrovní.</span><span class="sxs-lookup"><span data-stu-id="d6298-177">You can now test risk-based conditional access by signing in using a user with an elevated risk level.</span></span> <span data-ttu-id="d6298-178">Pokud uživatel riziko hello střední, v závislosti na konfiguraci hello zásad, vaše přihlášení je buď zablokuje nebo je vynucen toochange heslo.</span><span class="sxs-lookup"><span data-stu-id="d6298-178">If hello user risk is Medium, depending on hello configuration of your policy, your sign-in is be either blocked or you are forced toochange your password.</span></span> 
    <br><br><span data-ttu-id="d6298-179">
    ![Playbook](./media/active-directory-identityprotection-playbook/201.png "Playbook")
    </span><span class="sxs-lookup"><span data-stu-id="d6298-179">
![Playbook](./media/active-directory-identityprotection-playbook/201.png "Playbook")
</span></span><br>

## <a name="sign-in-risk"></a><span data-ttu-id="d6298-180">Riziko přihlášení</span><span class="sxs-lookup"><span data-stu-id="d6298-180">Sign-in risk</span></span>
<span data-ttu-id="d6298-181">**tootest přihlášení riziko, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="d6298-181">**tootest a sign in risk, perform hello following steps:**</span></span>

1. <span data-ttu-id="d6298-182">Přihlaste se příliš[https://portal.azure.com ](https://portal.azure.com) pomocí přihlašovacích údajů globálního správce pro vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="d6298-182">Sign-in too[https://portal.azure.com ](https://portal.azure.com) with global administrator credentials for your tenant.</span></span>
2. <span data-ttu-id="d6298-183">Přejděte příliš**Identity Protection**.</span><span class="sxs-lookup"><span data-stu-id="d6298-183">Navigate too**Identity Protection**.</span></span>
3. <span data-ttu-id="d6298-184">Na hlavní hello **Azure AD Identity Protection** okně klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="d6298-184">On hello main **Azure AD Identity Protection** blade, click **Settings**.</span></span> 
4. <span data-ttu-id="d6298-185">Na hello **nastavení portálu** okno, v části **pravidla zabezpečení**, klikněte na tlačítko **přihlášení riziko**.</span><span class="sxs-lookup"><span data-stu-id="d6298-185">On hello **Portal Settings** blade, under **Security rules**, click **Sign in risk**.</span></span>
5. <span data-ttu-id="d6298-186">Na hello ** přihlásit riziko ** vyberte **na** pod **Povolit pravidlo**.</span><span class="sxs-lookup"><span data-stu-id="d6298-186">On hello **Sign in Risk **blade, select **On** under **Enable rule**.</span></span> 
6. <span data-ttu-id="d6298-187">Vyberte jednu z hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="d6298-187">Select one of hello following options:</span></span>
   
   <span data-ttu-id="d6298-188">a.</span><span class="sxs-lookup"><span data-stu-id="d6298-188">a.</span></span> <span data-ttu-id="d6298-189">tooblock, vyberte **střední** pod **bloku přihlášení**</span><span class="sxs-lookup"><span data-stu-id="d6298-189">tooblock, select **Medium** under **Block sign in**</span></span>
   
   <span data-ttu-id="d6298-190">b.</span><span class="sxs-lookup"><span data-stu-id="d6298-190">b.</span></span> <span data-ttu-id="d6298-191">Změna tooenforce zabezpečeného hesla, vyberte **střední** pod **vyžadovat vícefaktorové ověřování**.</span><span class="sxs-lookup"><span data-stu-id="d6298-191">tooenforce secure password change, select **Medium** under **Require multi-factor authentication**.</span></span>
7. <span data-ttu-id="d6298-192">tooblock, vyberte střední v rámci bloku přihlášení.</span><span class="sxs-lookup"><span data-stu-id="d6298-192">tooblock, select Medium under Block sign in.</span></span>
8. <span data-ttu-id="d6298-193">tooenforce služby Multi-Factor authentication, vyberte **střední** pod **vyžadovat vícefaktorové ověřování**.</span><span class="sxs-lookup"><span data-stu-id="d6298-193">tooenforce multi-factor authentication, select **Medium** under **Require multi-factor authentication**.</span></span>
9. <span data-ttu-id="d6298-194">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d6298-194">Click on **Save**.</span></span>
10. <span data-ttu-id="d6298-195">Nyní můžete otestovat podmíněného přístupu na základě riziko tak, že simuluje neznámých míst hello nebo anonymní IP riziko události, protože jsou oba **střední** rizik události.</span><span class="sxs-lookup"><span data-stu-id="d6298-195">You can now test risk-based conditional access by simulating hello unfamiliar locations or anonymous IP risk events because they are both **Medium** risk events.</span></span>


<span data-ttu-id="d6298-196">![Playbook](./media/active-directory-identityprotection-playbook/200.png "Playbook")</span><span class="sxs-lookup"><span data-stu-id="d6298-196">![Playbook](./media/active-directory-identityprotection-playbook/200.png "Playbook")</span></span>


## <a name="see-also"></a><span data-ttu-id="d6298-197">Viz také</span><span class="sxs-lookup"><span data-stu-id="d6298-197">See also</span></span>
* [<span data-ttu-id="d6298-198">Ochrany identit Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d6298-198">Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection.md)

