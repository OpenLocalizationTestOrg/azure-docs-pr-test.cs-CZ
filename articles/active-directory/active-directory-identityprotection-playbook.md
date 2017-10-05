---
title: Azure Active Directory Identity Protection playbook | Microsoft Docs
description: "Zjistěte, jak Azure AD Identity Protection umožňuje omezit možnost útočník zneužít ohroženými identity nebo zařízení a zabezpečit identity nebo zařízení, která byla dříve by mohly vzbuzovat podezření nebo známé došlo k narušení."
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
ms.openlocfilehash: 2ecd07faed785fa6aa179ac1cca35a70d965e1dc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-identity-protection-playbook"></a><span data-ttu-id="ccc0f-104">Azure seznam strategií ochrany identit Active Directory</span><span class="sxs-lookup"><span data-stu-id="ccc0f-104">Azure Active Directory Identity Protection playbook</span></span>
<span data-ttu-id="ccc0f-105">Tato playbook vám pomůže:</span><span class="sxs-lookup"><span data-stu-id="ccc0f-105">This playbook helps you to:</span></span>

* <span data-ttu-id="ccc0f-106">Naplnění dat v prostředí Identity Protection simulace rizikových událostech a ohrožení zabezpečení</span><span class="sxs-lookup"><span data-stu-id="ccc0f-106">Populate data in the Identity Protection environment by simulating risk events and vulnerabilities</span></span>
* <span data-ttu-id="ccc0f-107">Nastavit zásady podmíněného přístupu na základě riziko a otestovat vliv těchto zásad</span><span class="sxs-lookup"><span data-stu-id="ccc0f-107">Set up risk-based conditional access policies and test the impact of these policies</span></span>

## <a name="simulating-risk-events"></a><span data-ttu-id="ccc0f-108">Simulaci rizikových událostí</span><span class="sxs-lookup"><span data-stu-id="ccc0f-108">Simulating Risk Events</span></span>
<span data-ttu-id="ccc0f-109">Tato část vám poskytne kroky pro simulaci následující typy událostí rizika:</span><span class="sxs-lookup"><span data-stu-id="ccc0f-109">This section provides you with steps for simulating the following risk event types:</span></span>

* <span data-ttu-id="ccc0f-110">Přihlášení z anonymních IP adres (snadno)</span><span class="sxs-lookup"><span data-stu-id="ccc0f-110">Sign-ins from anonymous IP addresses (easy)</span></span>
* <span data-ttu-id="ccc0f-111">Přihlášení z neznámých míst (střední)</span><span class="sxs-lookup"><span data-stu-id="ccc0f-111">Sign-ins from unfamiliar locations (moderate)</span></span>
* <span data-ttu-id="ccc0f-112">Nemožná cesta do netypických míst (obtížné)</span><span class="sxs-lookup"><span data-stu-id="ccc0f-112">Impossible travel to atypical locations (difficult)</span></span>

<span data-ttu-id="ccc0f-113">Další události riziko nelze simulated zabezpečeným způsobem.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-113">Other risk events cannot be simulated in a secure manner.</span></span>

### <a name="sign-ins-from-anonymous-ip-addresses"></a><span data-ttu-id="ccc0f-114">Přihlášení z anonymních IP adres</span><span class="sxs-lookup"><span data-stu-id="ccc0f-114">Sign-ins from anonymous IP addresses</span></span>
<span data-ttu-id="ccc0f-115">Tento typ události riziko identifikuje uživatele, kteří mají úspěšném přihlášení z IP adresy, která byla zjištěna jako IP adresa anonymního proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-115">This risk event type identifies users who have successfully signed in from an IP address that has been identified as an anonymous proxy IP address.</span></span> <span data-ttu-id="ccc0f-116">Tyto servery proxy jsou používány osob, které chcete skrýt IP adresu své zařízení a mohou být použity pro zlými úmysly.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-116">These proxies are used by people who want to hide their device’s IP address and may be used for malicious intent.</span></span>

<span data-ttu-id="ccc0f-117">**Pro simulaci přihlášení z anonymních IP adresy, proveďte následující kroky**:</span><span class="sxs-lookup"><span data-stu-id="ccc0f-117">**To simulate a sign-in from an anonymous IP, perform the following steps**:</span></span>

1. <span data-ttu-id="ccc0f-118">Stažení [Tor prohlížeče](https://www.torproject.org/projects/torbrowser.html.en).</span><span class="sxs-lookup"><span data-stu-id="ccc0f-118">Download the [Tor Browser](https://www.torproject.org/projects/torbrowser.html.en).</span></span>
2. <span data-ttu-id="ccc0f-119">Pomocí prohlížeče Tor, přejděte na [https://myapps.microsoft.com](https://myapps.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="ccc0f-119">Using the Tor Browser, navigate to [https://myapps.microsoft.com](https://myapps.microsoft.com).</span></span>   
3. <span data-ttu-id="ccc0f-120">Zadejte přihlašovací údaje účtu, který se má zobrazit v **přihlášení z anonymních IP adres** sestavy.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-120">Enter the credentials of the account you want to appear in the **Sign-ins from anonymous IP addresses** report.</span></span>

<span data-ttu-id="ccc0f-121">Přihlášení se zobrazí na řídicím panelu ochrany identit během pěti minut.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-121">The sign-in will show up on the Identity Protection dashboard within 5 minutes.</span></span> 

### <a name="sign-ins-from-unfamiliar-locations"></a><span data-ttu-id="ccc0f-122">Přihlášení z neznámých míst</span><span class="sxs-lookup"><span data-stu-id="ccc0f-122">Sign-ins from unfamiliar locations</span></span>
<span data-ttu-id="ccc0f-123">Riziko neznámých míst je v reálném čase vyhodnocení přihlášení mechanismus, který zvažuje po přihlášení umístění (IP, zeměpisné šířky nebo zeměpisnou délku a ASN) k určení nového / neznámého umístění.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-123">The unfamiliar locations risk is a real-time sign-in evaluation mechanism that considers past sign-in locations (IP, Latitude / Longitude and ASN) to determine new / unfamiliar locations.</span></span> <span data-ttu-id="ccc0f-124">Systém ukládá předchozí IP adresy, zeměpisné šířky nebo zeměpisnou délku a čísla ASN uživatele a tyto být známé umístění zvažuje.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-124">The system stores previous IPs, Latitude / Longitude, and ASNs of a user and considers these to be familiar locations.</span></span> <span data-ttu-id="ccc0f-125">Umístění přihlášení je považován za obeznámeni, pokud umístění přihlášení neodpovídá žádnému z existující známé umístění.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-125">A sign-in location is considered unfamiliar if the sign-in location does not match any of the existing familiar locations.</span></span>

<span data-ttu-id="ccc0f-126">Ochrany identit Azure Active Directory:</span><span class="sxs-lookup"><span data-stu-id="ccc0f-126">Azure Active Directory Identity Protection:</span></span>  

* <span data-ttu-id="ccc0f-127">má po počáteční learning dobu 14 dnů, za které není příznak jiných umístění jako neznámých míst.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-127">has an initial learning period of 14 days during which it does not flag any new locations as unfamiliar locations.</span></span>
* <span data-ttu-id="ccc0f-128">ignoruje přihlášení ze známé zařízení a umístění, které jsou geograficky blízko existující známé umístění.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-128">ignores sign-ins from familiar devices and locations that are geographically close to an existing familiar location.</span></span>

<span data-ttu-id="ccc0f-129">Pro simulaci neznámých míst, budete muset přihlásit z umístění a zařízení, které účet nebyl přihlášení z před.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-129">To simulate unfamiliar locations, you have to sign in from a location and device that the account has not signed in from before.</span></span> 

<span data-ttu-id="ccc0f-130">**Pro simulaci přihlášení z neznámého umístění, proveďte následující kroky**:</span><span class="sxs-lookup"><span data-stu-id="ccc0f-130">**To simulate a sign-in from an unfamiliar location, perform the following steps**:</span></span>

1. <span data-ttu-id="ccc0f-131">Vyberte účet, který má aspoň 14 dnů přihlášení historie.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-131">Choose an account that has at least a 14-day sign-in history.</span></span> 
2. <span data-ttu-id="ccc0f-132">Proveďte:</span><span class="sxs-lookup"><span data-stu-id="ccc0f-132">Do either:</span></span>
   
   <span data-ttu-id="ccc0f-133">a.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-133">a.</span></span> <span data-ttu-id="ccc0f-134">Při použití sítě VPN, přejděte na [https://myapps.microsoft.com](https://myapps.microsoft.com) a zadejte přihlašovací údaje účtu, kterou chcete simulovat riziko událost.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-134">While using a VPN, navigate to [https://myapps.microsoft.com](https://myapps.microsoft.com) and enter the credentials of the account you want to simulate the risk event for.</span></span>
   
   <span data-ttu-id="ccc0f-135">b.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-135">b.</span></span> <span data-ttu-id="ccc0f-136">Požádejte přidružení v jiném umístění se přihlásit pomocí přihlašovacích údajů účtu (nedoporučuje se).</span><span class="sxs-lookup"><span data-stu-id="ccc0f-136">Ask an associate in a different location to sign in using the account’s credentials (not recommended).</span></span>

<span data-ttu-id="ccc0f-137">Přihlášení se zobrazí na řídicím panelu ochrany identit během pěti minut.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-137">The sign-in will show up on the Identity Protection dashboard within 5 minutes.</span></span>

### <a name="impossible-travel-to-atypical-location"></a><span data-ttu-id="ccc0f-138">Nemožná cesta do netypických umístění</span><span class="sxs-lookup"><span data-stu-id="ccc0f-138">Impossible travel to atypical location</span></span>
<span data-ttu-id="ccc0f-139">Simulaci podmínku neuskutečnitelná cesta je složité, protože algoritmus využívá strojové učení k odstraňování plevele na hodnotu false pozitivních třeba neuskutečnitelná cesta ze známé zařízení, nebo přihlášení z sítě VPN, které se používají jinými uživateli v adresáři.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-139">Simulating the impossible travel condition is difficult because the algorithm uses machine learning to weed out false-positives such as impossible travel from familiar devices, or sign-ins from VPNs that are used by other users in the directory.</span></span> <span data-ttu-id="ccc0f-140">Kromě toho algoritmus vyžaduje 3 až 14 dní historii přihlášení pro uživatele před zahájením generování rizikových událostí.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-140">Additionally, the algorithm requires a sign-in history of 3 to 14 days for the user before it begins generating risk events.</span></span>

<span data-ttu-id="ccc0f-141">**Pro simulaci nemožná cesta do netypických umístění, proveďte následující kroky**:</span><span class="sxs-lookup"><span data-stu-id="ccc0f-141">**To simulate an impossible travel to atypical location, perform the following steps**:</span></span>

1. <span data-ttu-id="ccc0f-142">Pomocí standardní prohlížeč, přejděte do [https://myapps.microsoft.com](https://myapps.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="ccc0f-142">Using your standard browser, navigate to [https://myapps.microsoft.com](https://myapps.microsoft.com).</span></span>  
2. <span data-ttu-id="ccc0f-143">Zadejte přihlašovací údaje účtu, pomocí kterého chcete generovat událost riziko neuskutečnitelná cesta pro.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-143">Enter the credentials of the account you want to generate an impossible travel risk event for.</span></span>
3. <span data-ttu-id="ccc0f-144">Změna uživatelského agenta.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-144">Change your user agent.</span></span> <span data-ttu-id="ccc0f-145">Můžete změnit uživatelský agent v aplikaci Internet Explorer z nástrojů pro vývojáře, nebo změnit váš uživatelský agent v Firefox nebo Chrome pomocí doplňku přepínači uživatelského agenta.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-145">You can change user agent in Internet Explorer from Developer Tools, or change your user agent in Firefox or Chrome using a user-agent switcher add-on.</span></span>
4. <span data-ttu-id="ccc0f-146">Změníte IP adresu.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-146">Change your IP address.</span></span> <span data-ttu-id="ccc0f-147">Můžete změnit IP adresu pomocí sítě VPN, rozšíření Tor nebo otáčí nový počítač v Azure v různých datových center.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-147">You can change your IP address by using a VPN, a Tor add-on, or spinning up a new machine in Azure in a different data center.</span></span>
5. <span data-ttu-id="ccc0f-148">Přihlaste se do [https://myapps.microsoft.com](https://myapps.microsoft.com) pomocí stejných přihlašovacích údajů jako před a během několika minut po předchozí přihlášení.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-148">Sign-in to [https://myapps.microsoft.com](https://myapps.microsoft.com) using the same credentials as before and within a few minutes after the previous sign-in.</span></span>

<span data-ttu-id="ccc0f-149">Přihlášení se zobrazí v řídicím panelu ochrany identit v rámci 2 – 4 hodiny.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-149">The sign-in will show up in the Identity Protection dashboard within 2-4 hours.</span></span><br>
<span data-ttu-id="ccc0f-150">Z důvodu složitých strojového učení modely související se situací je možné, že se ho nebude získat převzata.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-150">Because of the complex machine learning models involved, there is a chance it will not get picked up.</span></span><br> <span data-ttu-id="ccc0f-151">Můžete chtít replikovat tyto kroky pro více účtů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-151">You might want to replicate these steps for multiple Azure AD accounts.</span></span>

## <a name="simulating-vulnerabilities"></a><span data-ttu-id="ccc0f-152">Simulaci ohrožení zabezpečení</span><span class="sxs-lookup"><span data-stu-id="ccc0f-152">Simulating vulnerabilities</span></span>
<span data-ttu-id="ccc0f-153">Ohrožení zabezpečení jsou slabá místa v prostředí Azure AD, které může zneužít objektu actor chybný.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-153">Vulnerabilities are weaknesses in an Azure AD environment that can be exploited by a bad actor.</span></span> <span data-ttu-id="ccc0f-154">Aktuálně jsou prezentované 3 typy ohrožení zabezpečení v Azure AD Identity Protection, které využít další funkce služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-154">Currently 3 types of vulnerabilities are surfaced in Azure AD Identity Protection that leverage other features of Azure AD.</span></span> <span data-ttu-id="ccc0f-155">Tyto chyby zabezpečení se zobrazí na řídicím panelu ochrany identit automaticky po tyto funkce jsou nastavené.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-155">These Vulnerabilities will be displayed on the Identity Protection dashboard automatically once these features are set up.</span></span>

* <span data-ttu-id="ccc0f-156">Azure AD [služby Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)</span><span class="sxs-lookup"><span data-stu-id="ccc0f-156">Azure AD [Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)</span></span>
* <span data-ttu-id="ccc0f-157">Azure AD [Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ccc0f-157">Azure AD [Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).</span></span>
* <span data-ttu-id="ccc0f-158">Azure AD [Privileged Identity Management](active-directory-privileged-identity-management-configure.md).</span><span class="sxs-lookup"><span data-stu-id="ccc0f-158">Azure AD [Privileged Identity Management](active-directory-privileged-identity-management-configure.md).</span></span> 

## <a name="user-compromise-risk"></a><span data-ttu-id="ccc0f-159">Riziko ohrožení zabezpečení pro uživatele</span><span class="sxs-lookup"><span data-stu-id="ccc0f-159">User compromise risk</span></span>
<span data-ttu-id="ccc0f-160">**K testování riziko ohrožení zabezpečení pro uživatele, proveďte následující kroky**:</span><span class="sxs-lookup"><span data-stu-id="ccc0f-160">**To test User compromise risk, perform the following steps**:</span></span>

1. <span data-ttu-id="ccc0f-161">Přihlaste se do [https://portal.azure.com](https://portal.azure.com) pomocí přihlašovacích údajů globálního správce pro vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-161">Sign-in to [https://portal.azure.com](https://portal.azure.com) with global administrator credentials for your tenant.</span></span>
2. <span data-ttu-id="ccc0f-162">Přejděte na **ochrany identit**.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-162">Navigate to **Identity Protection**.</span></span> 
3. <span data-ttu-id="ccc0f-163">V hlavním **Azure AD Identity Protection** okně klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-163">On the main **Azure AD Identity Protection** blade, click **Settings**.</span></span> 
4. <span data-ttu-id="ccc0f-164">Na **nastavení portálu** okno, v části **pravidla zabezpečení**, klikněte na tlačítko **riziko ohrožení zabezpečení uživatele**.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-164">On the **Portal Settings** blade, under **Security rules**, click **User compromise risk**.</span></span> 
5. <span data-ttu-id="ccc0f-165">Na **přihlášení riziko** okně zapnout **Povolit pravidlo** vypnout a potom klikněte na **Uložit** nastavení.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-165">On the **Sign in Risk** blade, turn **Enable rule** off, and then click **Save** settings.</span></span>
6. <span data-ttu-id="ccc0f-166">Pro daný uživatelský účet, simulovat neznámého umístění nebo anonymní IP riziko událost.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-166">For a given user account, simulate an unfamiliar locations or anonymous IP risk event.</span></span> <span data-ttu-id="ccc0f-167">To bude zvýšit úroveň rizika uživatele pro daného uživatele k **střední**.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-167">This will elevate the user risk level for that user to **Medium**.</span></span>
7. <span data-ttu-id="ccc0f-168">Počkejte několik minut a ověřte, že úrovni uživatele pro vaše uživatele je **střední**.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-168">Wait a few minutes, and then verify that user level for your user is **Medium**.</span></span>
8. <span data-ttu-id="ccc0f-169">Přejděte na **nastavení portálu** okno.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-169">Go to the **Portal Settings** blade.</span></span>
9. <span data-ttu-id="ccc0f-170">Na **riziko ohrožení zabezpečení uživatele** okno, v části **Povolit pravidlo**, vyberte **na** .</span><span class="sxs-lookup"><span data-stu-id="ccc0f-170">On the **User Compromise Risk** blade, under **Enable rule**, select **On** .</span></span> 
10. <span data-ttu-id="ccc0f-171">Vyberte jednu z následujících možností:</span><span class="sxs-lookup"><span data-stu-id="ccc0f-171">Select one of the following options:</span></span>
    
    <span data-ttu-id="ccc0f-172">a.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-172">a.</span></span> <span data-ttu-id="ccc0f-173">Blok, vyberte **střední** pod **bloku přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-173">To block, select **Medium** under **Block sign in**.</span></span>
    
    <span data-ttu-id="ccc0f-174">b.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-174">b.</span></span> <span data-ttu-id="ccc0f-175">Pokud chcete vynutit zabezpečené heslo změnit, vyberte **střední** pod **vyžadovat vícefaktorové ověřování**.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-175">To enforce secure password change, select **Medium** under **Require multi-factor authentication**.</span></span>
11. <span data-ttu-id="ccc0f-176">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-176">Click **Save**.</span></span>
12. <span data-ttu-id="ccc0f-177">Nyní můžete otestovat podmíněného přístupu na základě riziko podle přihlášení pomocí uživatel s oprávněním vyšší úrovně rizika úrovní.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-177">You can now test risk-based conditional access by signing in using a user with an elevated risk level.</span></span> <span data-ttu-id="ccc0f-178">Pokud je uživatel riziko střední, v závislosti na konfiguraci zásad, vaše přihlášení je buď zablokuje nebo je vynucen ke změně hesla.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-178">If the user risk is Medium, depending on the configuration of your policy, your sign-in is be either blocked or you are forced to change your password.</span></span> 
    <br><br><span data-ttu-id="ccc0f-179">
    ![Playbook](./media/active-directory-identityprotection-playbook/201.png "Playbook")
    </span><span class="sxs-lookup"><span data-stu-id="ccc0f-179">
![Playbook](./media/active-directory-identityprotection-playbook/201.png "Playbook")
</span></span><br>

## <a name="sign-in-risk"></a><span data-ttu-id="ccc0f-180">Riziko přihlášení</span><span class="sxs-lookup"><span data-stu-id="ccc0f-180">Sign-in risk</span></span>
<span data-ttu-id="ccc0f-181">**K testování znaménkem v riziko, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ccc0f-181">**To test a sign in risk, perform the following steps:**</span></span>

1. <span data-ttu-id="ccc0f-182">Přihlaste se do [https://portal.azure.com ](https://portal.azure.com) pomocí přihlašovacích údajů globálního správce pro vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-182">Sign-in to [https://portal.azure.com ](https://portal.azure.com) with global administrator credentials for your tenant.</span></span>
2. <span data-ttu-id="ccc0f-183">Přejděte na **ochrany identit**.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-183">Navigate to **Identity Protection**.</span></span>
3. <span data-ttu-id="ccc0f-184">V hlavním **Azure AD Identity Protection** okně klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-184">On the main **Azure AD Identity Protection** blade, click **Settings**.</span></span> 
4. <span data-ttu-id="ccc0f-185">Na **nastavení portálu** okno, v části **pravidla zabezpečení**, klikněte na tlačítko **přihlášení riziko**.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-185">On the **Portal Settings** blade, under **Security rules**, click **Sign in risk**.</span></span>
5. <span data-ttu-id="ccc0f-186">Na ** přihlásit riziko ** vyberte **na** pod **Povolit pravidlo**.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-186">On the **Sign in Risk **blade, select **On** under **Enable rule**.</span></span> 
6. <span data-ttu-id="ccc0f-187">Vyberte jednu z následujících možností:</span><span class="sxs-lookup"><span data-stu-id="ccc0f-187">Select one of the following options:</span></span>
   
   <span data-ttu-id="ccc0f-188">a.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-188">a.</span></span> <span data-ttu-id="ccc0f-189">Blokování, vyberte **střední** pod **bloku přihlášení**</span><span class="sxs-lookup"><span data-stu-id="ccc0f-189">To block, select **Medium** under **Block sign in**</span></span>
   
   <span data-ttu-id="ccc0f-190">b.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-190">b.</span></span> <span data-ttu-id="ccc0f-191">Pokud chcete vynutit zabezpečené heslo změnit, vyberte **střední** pod **vyžadovat vícefaktorové ověřování**.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-191">To enforce secure password change, select **Medium** under **Require multi-factor authentication**.</span></span>
7. <span data-ttu-id="ccc0f-192">Blokování, vyberte v části přihlášení bloku střední.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-192">To block, select Medium under Block sign in.</span></span>
8. <span data-ttu-id="ccc0f-193">Chcete-li vynutit ověřování Multi-Factor authentication, vyberte **střední** pod **vyžadovat vícefaktorové ověřování**.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-193">To enforce multi-factor authentication, select **Medium** under **Require multi-factor authentication**.</span></span>
9. <span data-ttu-id="ccc0f-194">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-194">Click on **Save**.</span></span>
10. <span data-ttu-id="ccc0f-195">Nyní můžete otestovat podmíněného přístupu na základě riziko tak, že simuluje neznámých míst nebo anonymní IP riziko události, protože jsou oba **střední** rizik události.</span><span class="sxs-lookup"><span data-stu-id="ccc0f-195">You can now test risk-based conditional access by simulating the unfamiliar locations or anonymous IP risk events because they are both **Medium** risk events.</span></span>


<span data-ttu-id="ccc0f-196">![Playbook](./media/active-directory-identityprotection-playbook/200.png "Playbook")</span><span class="sxs-lookup"><span data-stu-id="ccc0f-196">![Playbook](./media/active-directory-identityprotection-playbook/200.png "Playbook")</span></span>


## <a name="see-also"></a><span data-ttu-id="ccc0f-197">Viz také</span><span class="sxs-lookup"><span data-stu-id="ccc0f-197">See also</span></span>
* [<span data-ttu-id="ccc0f-198">Ochrany identit Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ccc0f-198">Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection.md)

