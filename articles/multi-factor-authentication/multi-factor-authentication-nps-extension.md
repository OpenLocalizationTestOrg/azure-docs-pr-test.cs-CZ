---
title: "Použít existující servery NPS k poskytování funkcí Azure MFA | Microsoft Docs"
description: "Server NPS rozšíření pro Azure Multi-Factor Authentication není jednoduchým řešením pro přidání možností cloudového dvoustupňové vericiation do vaší stávající infrastruktury pro ověřování."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: H1Hack27Feb2017; it-pro
ms.openlocfilehash: fa125292ee85bd9b5329cffeff7f076d3002cbf3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="integrate-your-existing-nps-infrastructure-with-azure-multi-factor-authentication"></a><span data-ttu-id="ef250-103">Stávající infrastruktury serveru NPS integrovat Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="ef250-103">Integrate your existing NPS infrastructure with Azure Multi-Factor Authentication</span></span>

<span data-ttu-id="ef250-104">Rozšíření serveru NPS (Network Policy Server) pro Azure MFA přidá možnosti vícefaktorového ověřování založená na cloudu k infrastruktuře ověřování pomocí existujících serverů.</span><span class="sxs-lookup"><span data-stu-id="ef250-104">The Network Policy Server (NPS) extension for Azure MFA adds cloud-based MFA capabilities to your authentication infrastructure using your existing servers.</span></span> <span data-ttu-id="ef250-105">Rozšíření serveru NPS můžete přidat telefonní hovor, textová zpráva nebo ověření telefonu v aplikaci k vaší stávající tok ověřování bez nutnosti instalovat, konfigurovat a spravovat nové servery.</span><span class="sxs-lookup"><span data-stu-id="ef250-105">With the NPS extension, you can add phone call, text message, or phone app verification to your existing authentication flow without having to install, configure, and maintain new servers.</span></span> 

<span data-ttu-id="ef250-106">Toto rozšíření byla vytvořena pro organizace, které chcete chránit připojení k síti VPN bez nasazení Azure MFA serveru.</span><span class="sxs-lookup"><span data-stu-id="ef250-106">This extension was created for organizations that want to protect VPN connections without deploying the Azure MFA Server.</span></span> <span data-ttu-id="ef250-107">Rozšíření serveru NPS slouží jako adaptér mezi RADIUS a cloudu Azure MFA pro poskytování druhý faktor ověřování pro federované nebo synchronizovaných uživatelů.</span><span class="sxs-lookup"><span data-stu-id="ef250-107">The NPS extension acts as an adapter between RADIUS and cloud-based Azure MFA to provide a second factor of authentication for federated or synced users.</span></span>

<span data-ttu-id="ef250-108">Pokud používáte rozšíření NPS pro Azure MFA, tok ověřování zahrnuje následující součásti:</span><span class="sxs-lookup"><span data-stu-id="ef250-108">When using the NPS extension for Azure MFA, the authentication flow includes the following components:</span></span> 

1. <span data-ttu-id="ef250-109">**Server NAS nebo virtuální privátní sítě** přijímá požadavky od klientů VPN a převede je na požadavky protokolu RADIUS na serverech NPS.</span><span class="sxs-lookup"><span data-stu-id="ef250-109">**NAS/VPN Server** receives requests from VPN clients and converts them into RADIUS requests to NPS servers.</span></span> 
2. <span data-ttu-id="ef250-110">**NPS Server** připojuje ke službě Active Directory a provést primární ověřování pro požadavky protokolu RADIUS a na úspěch, předává žádosti žádné nainstalovaná rozšíření.</span><span class="sxs-lookup"><span data-stu-id="ef250-110">**NPS Server** connects to Active Directory to perform the primary authentication for the RADIUS requests and, upon success, passes the request to any installed extensions.</span></span>  
3. <span data-ttu-id="ef250-111">**Rozšíření serveru NPS** aktivuje požadavek na Azure MFA pro sekundární ověřování.</span><span class="sxs-lookup"><span data-stu-id="ef250-111">**NPS Extension** triggers a request to Azure MFA for the secondary authentication.</span></span> <span data-ttu-id="ef250-112">Jakmile rozšíření obdrží odpověď, a pokud ověřovací test MFA úspěšné, dokončení žádosti o ověření a poskytovat tokeny zabezpečení, které obsahují deklaraci identity MFA NPS server, vydaný služby tokenů zabezpečení Azure.</span><span class="sxs-lookup"><span data-stu-id="ef250-112">Once the extension receives the response, and if the MFA challenge succeeds, it completes the authentication request by providing the NPS server with security tokens that include an MFA claim, issued by Azure STS.</span></span>  
4. <span data-ttu-id="ef250-113">**Azure MFA** komunikuje s Azure Active Directory se načíst podrobnosti o uživatele a provádí pomocí metody ověřování nakonfigurovat tak, aby uživatel sekundární ověřování.</span><span class="sxs-lookup"><span data-stu-id="ef250-113">**Azure MFA** communicates with Azure Active Directory to retrieve the user’s details and performs the secondary authentication using a verification method configured to the user.</span></span>

<span data-ttu-id="ef250-114">Následující diagram znázorňuje tento tok požadavku vysoké úrovně ověřování:</span><span class="sxs-lookup"><span data-stu-id="ef250-114">The following diagram illustrates this high-level authentication request flow:</span></span> 

![Vývojový diagram ověřování](./media/multi-factor-authentication-nps-extension/auth-flow.png)

## <a name="plan-your-deployment"></a><span data-ttu-id="ef250-116">Plánování nasazení</span><span class="sxs-lookup"><span data-stu-id="ef250-116">Plan your deployment</span></span>

<span data-ttu-id="ef250-117">Rozšíření serveru NPS automaticky zpracovává redundance, takže není nutné žádnou zvláštní konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="ef250-117">The NPS extension automatically handles redundancy, so you don't need a special configuration.</span></span>

<span data-ttu-id="ef250-118">Můžete vytvořit tolik serverů povolena Azure MFA serveru NPS podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="ef250-118">You can create as many Azure MFA-enabled NPS servers as you need.</span></span> <span data-ttu-id="ef250-119">Pokud instalujete několik serverů, používejte rozdíl klientský certifikát pro každou z nich.</span><span class="sxs-lookup"><span data-stu-id="ef250-119">If you do install multiple servers, you should use a difference client certificate for each one of them.</span></span> <span data-ttu-id="ef250-120">Vytvoření certifikátu pro každý server znamená, že můžete aktualizovat jednotlivě každý certifikát a nestarat se o výpadek napříč všemi servery.</span><span class="sxs-lookup"><span data-stu-id="ef250-120">Creating a cert for each server means that you can update each cert individually, and not worry about downtime across all your servers.</span></span>

<span data-ttu-id="ef250-121">Servery VPN směrovat žádosti o ověření, takže je třeba vědět nové servery NPS povolená Azure MFA.</span><span class="sxs-lookup"><span data-stu-id="ef250-121">VPN servers route authentication requests, so they need to be aware of the new Azure MFA-enabled NPS servers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ef250-122">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ef250-122">Prerequisites</span></span>

<span data-ttu-id="ef250-123">Rozšíření serveru NPS je určená pro práci s vaší stávající infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="ef250-123">The NPS extension is meant to work with your existing infrastructure.</span></span> <span data-ttu-id="ef250-124">Ujistěte se, že máte následující předpoklady, než začnete.</span><span class="sxs-lookup"><span data-stu-id="ef250-124">Make sure you have the following prerequisites before you begin.</span></span>

### <a name="licenses"></a><span data-ttu-id="ef250-125">Licence</span><span class="sxs-lookup"><span data-stu-id="ef250-125">Licenses</span></span>

<span data-ttu-id="ef250-126">Server NPS rozšíření pro Azure MFA není k dispozici pro zákazníky s [licencí pro ověřování Azure Multi-Factor Authentication](multi-factor-authentication.md) (zahrnutá v Azure AD Premium, EMS nebo předplatné vícefaktorového ověřování).</span><span class="sxs-lookup"><span data-stu-id="ef250-126">The NPS Extension for Azure MFA is available to customers with [licenses for Azure Multi-Factor Authentication](multi-factor-authentication.md) (included with Azure AD Premium, EMS, or an MFA subscription).</span></span>

### <a name="software"></a><span data-ttu-id="ef250-127">Software</span><span class="sxs-lookup"><span data-stu-id="ef250-127">Software</span></span>

<span data-ttu-id="ef250-128">Windows Server 2008 R2 SP1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ef250-128">Windows Server 2008 R2 SP1 or above.</span></span>

### <a name="libraries"></a><span data-ttu-id="ef250-129">Knihovny</span><span class="sxs-lookup"><span data-stu-id="ef250-129">Libraries</span></span>

<span data-ttu-id="ef250-130">Tyto knihovny jsou automaticky nainstalovány s příponou.</span><span class="sxs-lookup"><span data-stu-id="ef250-130">These libraries are installed automatically with the extension.</span></span>
-   [<span data-ttu-id="ef250-131">Distribuovatelné balíčky Visual C++ pro Visual Studio 2013 (X64)</span><span class="sxs-lookup"><span data-stu-id="ef250-131">Visual C++ Redistributable Packages for Visual Studio 2013 (X64)</span></span>](https://www.microsoft.com/download/details.aspx?id=40784)
-   [<span data-ttu-id="ef250-132">Microsoft Azure Active Directory modul pro prostředí Windows PowerShell verze 1.1.166.0</span><span class="sxs-lookup"><span data-stu-id="ef250-132">Microsoft Azure Active Directory Module for Windows PowerShell version 1.1.166.0</span></span>](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)

### <a name="azure-active-directory"></a><span data-ttu-id="ef250-133">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ef250-133">Azure Active Directory</span></span>

<span data-ttu-id="ef250-134">Všechny, kteří používají rozšíření NPS musí synchronizovat s Azure Active Directory přes Azure AD Connect a musí být zaregistrovaný pro MFA.</span><span class="sxs-lookup"><span data-stu-id="ef250-134">Everyone using the NPS extension must be synced to Azure Active Directory using Azure AD Connect, and must be registered for MFA.</span></span>

<span data-ttu-id="ef250-135">Při instalaci rozšíření, potřebujete přihlašovací údaje pro ID a správce adresáře pro vašeho tenanta Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ef250-135">When you install the extension, you need the directory ID and admin credentials for your Azure AD tenant.</span></span> <span data-ttu-id="ef250-136">Vaše ID adresáře v můžete najít [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ef250-136">You can find your directory ID in the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="ef250-137">Přihlaste se jako správce, vyberte **Azure Active Directory** ikonu na levé straně vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="ef250-137">Sign in as an administrator, select the **Azure Active Directory** icon on the left, then select **Properties**.</span></span> <span data-ttu-id="ef250-138">Zkopírovat identifikátor GUID v **ID adresáře** pole a uložte ho.</span><span class="sxs-lookup"><span data-stu-id="ef250-138">Copy the GUID in the **Directory ID** box and save it.</span></span> <span data-ttu-id="ef250-139">Můžete použít tento identifikátor GUID jako ID klienta při instalaci rozšíření serveru NPS.</span><span class="sxs-lookup"><span data-stu-id="ef250-139">You use this GUID as the tenant ID when you install the NPS extension.</span></span>

![Najít ID vašeho adresáře v rámci Azure Active Directory vlastnosti](./media/multi-factor-authentication-nps-extension/find-directory-id.png)

## <a name="prepare-your-environment"></a><span data-ttu-id="ef250-141">Příprava prostředí</span><span class="sxs-lookup"><span data-stu-id="ef250-141">Prepare your environment</span></span>

<span data-ttu-id="ef250-142">Před instalací NPS rozšíření, které chcete připravit můžete prostředí pro zpracování provozu ověřování.</span><span class="sxs-lookup"><span data-stu-id="ef250-142">Before you install the NPS extension, you want to prepare you environment to handle the authentication traffic.</span></span>

### <a name="enable-the-nps-role-on-a-domain-joined-server"></a><span data-ttu-id="ef250-143">Povolení role NPS na server připojený k doméně</span><span class="sxs-lookup"><span data-stu-id="ef250-143">Enable the NPS role on a domain-joined server</span></span>

<span data-ttu-id="ef250-144">NPS server se připojuje k Azure Active Directory a ověřuje požadavky vícefaktorového ověřování.</span><span class="sxs-lookup"><span data-stu-id="ef250-144">The NPS server connects to Azure Active Directory and authenticates the MFA requests.</span></span> <span data-ttu-id="ef250-145">Vyberte jeden server pro tuto roli.</span><span class="sxs-lookup"><span data-stu-id="ef250-145">Choose one server for this role.</span></span> <span data-ttu-id="ef250-146">Doporučujeme vybrat server, který nemůže pracovat s požadavky z jiných služeb, protože server NPS rozšíření vyvolá chyby pro všechny žádosti, které nejsou protokolu RADIUS.</span><span class="sxs-lookup"><span data-stu-id="ef250-146">We recommend choosing a server that doesn't handle requests from other services, because the NPS extension throws errors for any requests that aren't RADIUS.</span></span>

1. <span data-ttu-id="ef250-147">Na serveru, otevřete **Průvodce přidáním rolí a funkcí** v nabídce rychlý start správce serveru.</span><span class="sxs-lookup"><span data-stu-id="ef250-147">On your server, open the **Add Roles and Features Wizard** from the Server Manager Quickstart menu.</span></span>
2. <span data-ttu-id="ef250-148">Zvolte **instalace na základě rolí nebo na základě funkcí** pro váš typ instalace.</span><span class="sxs-lookup"><span data-stu-id="ef250-148">Choose **Role-based or feature-based installation** for your installation type.</span></span>
3. <span data-ttu-id="ef250-149">Vyberte **služba síťové zásady a přístup** role serveru.</span><span class="sxs-lookup"><span data-stu-id="ef250-149">Select the **Network Policy and Access Services** server role.</span></span> <span data-ttu-id="ef250-150">O požadované funkce pro tuto roli spouštět může překryvné okno.</span><span class="sxs-lookup"><span data-stu-id="ef250-150">A window may pop up to inform you of required features to run this role.</span></span>
4. <span data-ttu-id="ef250-151">Postupujte podle pokynů Průvodce až potvrzovací stránku.</span><span class="sxs-lookup"><span data-stu-id="ef250-151">Continue through the wizard until the Confirmation page.</span></span> <span data-ttu-id="ef250-152">Vyberte **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="ef250-152">Select **Install**.</span></span>

<span data-ttu-id="ef250-153">Nyní když máte server určený pro server NPS, byste měli nakonfigurovat tento server zpracovávat příchozí požadavky protokolu RADIUS z daného řešení VPN.</span><span class="sxs-lookup"><span data-stu-id="ef250-153">Now that you have a server designated for NPS, you should also configure this server to handle incoming RADIUS requests from the VPN solution.</span></span>

### <a name="configure-your-vpn-solution-to-communicate-with-the-nps-server"></a><span data-ttu-id="ef250-154">Konfigurace řešení sítě VPN pro komunikaci se serverem NPS</span><span class="sxs-lookup"><span data-stu-id="ef250-154">Configure your VPN solution to communicate with the NPS server</span></span>

<span data-ttu-id="ef250-155">V závislosti na tom, které použijete řešení sítě VPN postup konfigurace vaší zásady ověřování RADIUS lišit.</span><span class="sxs-lookup"><span data-stu-id="ef250-155">Depending on which VPN solution you use, the steps to configure your RADIUS authentication policy vary.</span></span> <span data-ttu-id="ef250-156">Nakonfigurujte tuto zásadu tak, aby odkazovaly na váš server RADIUS serveru NPS.</span><span class="sxs-lookup"><span data-stu-id="ef250-156">Configure this policy to point to your RADIUS NPS server.</span></span>

### <a name="sync-domain-users-to-the-cloud"></a><span data-ttu-id="ef250-157">Uživatelé domény synchronizace do cloudu</span><span class="sxs-lookup"><span data-stu-id="ef250-157">Sync domain users to the cloud</span></span>

<span data-ttu-id="ef250-158">Tento krok již mohou být dokončené na klientovi, ale je dobrým znovu zkontrolovat, že Azure AD Connect nedávno synchronizoval vaší databáze.</span><span class="sxs-lookup"><span data-stu-id="ef250-158">This step may already be complete on your tenant, but it's good to double-check that Azure AD Connect has synchronized your databases recently.</span></span>

1. <span data-ttu-id="ef250-159">Přihlaste se na webu [Azure Portal](https://portal.azure.com) jako správce.</span><span class="sxs-lookup"><span data-stu-id="ef250-159">Sign in to the [Azure portal](https://portal.azure.com) as an administrator.</span></span>
2. <span data-ttu-id="ef250-160">Vyberte **Azure Active Directory** > **Azure AD Connect**</span><span class="sxs-lookup"><span data-stu-id="ef250-160">Select **Azure Active Directory** > **Azure AD Connect**</span></span>
3. <span data-ttu-id="ef250-161">Ověřte, zda je vaše stav synchronizace **povoleno** a že poslední synchronizace byla menší než hodinou.</span><span class="sxs-lookup"><span data-stu-id="ef250-161">Verify that your sync status is **Enabled** and that your last sync was less than an hour ago.</span></span>

<span data-ttu-id="ef250-162">Pokud je potřeba ji nové kolo synchronizace, nám podle pokynů v [synchronizace Azure AD Connect: Scheduler](../active-directory/connect/active-directory-aadconnectsync-feature-scheduler.md#start-the-scheduler).</span><span class="sxs-lookup"><span data-stu-id="ef250-162">If you need to kick off a new round of synchronization, us the instructions in [Azure AD Connect sync: Scheduler](../active-directory/connect/active-directory-aadconnectsync-feature-scheduler.md#start-the-scheduler).</span></span>

### <a name="determine-which-authentication-methods-your-users-can-use"></a><span data-ttu-id="ef250-163">Určit, jaké metody ověřování mohou uživatelé používat</span><span class="sxs-lookup"><span data-stu-id="ef250-163">Determine which authentication methods your users can use</span></span>

<span data-ttu-id="ef250-164">Existují dva faktory, které ovlivňují, které metody ověřování jsou k dispozici s nasazením rozšíření serveru NPS:</span><span class="sxs-lookup"><span data-stu-id="ef250-164">There are two factors that affect which authentication methods are available with an NPS extension deployment:</span></span>

1. <span data-ttu-id="ef250-165">Heslo šifrovacího algoritmu používaného mezi klientem RADIUS (sítě VPN, Netscaler server či jiné) a servery NPS.</span><span class="sxs-lookup"><span data-stu-id="ef250-165">The password encryption algorithm used between the RADIUS client (VPN, Netscaler server, or other) and the NPS servers.</span></span>
   - <span data-ttu-id="ef250-166">**PAP** podporuje všechny metody ověřování Azure mfa v cloudu: telefonní hovor, jednosměrné textová zpráva, oznámení mobilní aplikace a kód ověření mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="ef250-166">**PAP** supports all the authentication methods of Azure MFA in the cloud: phone call, one-way text message, mobile app notification, and mobile app verification code.</span></span>
   - <span data-ttu-id="ef250-167">**CHAPV2** a **EAP** podporují telefonní hovory a oznámení mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="ef250-167">**CHAPV2** and **EAP** support phone call and mobile app notification.</span></span>
2. <span data-ttu-id="ef250-168">Metody zadávání znaků, klientská aplikace (VPN, Netscaler server či jiné) může zpracovat.</span><span class="sxs-lookup"><span data-stu-id="ef250-168">The input methods that the client application (VPN, Netscaler server, or other) can handle.</span></span> <span data-ttu-id="ef250-169">Například klienta VPN mít některé prostředky, které chcete umožnit uživatelům zadejte ověřovací kód z mobilní aplikace nebo text?</span><span class="sxs-lookup"><span data-stu-id="ef250-169">For example, does the VPN client have some means to allow the user to type in a verification code from a text or mobile app?</span></span>

<span data-ttu-id="ef250-170">Když nasadíte rozšíření serveru NPS, použijte tyto faktory k vyhodnocení, které metody jsou k dispozici pro vaše uživatele.</span><span class="sxs-lookup"><span data-stu-id="ef250-170">When you deploy the NPS extension, use these factors to evaluate which methods are available for your users.</span></span> <span data-ttu-id="ef250-171">Pokud váš klient RADIUS podporuje protokol PAP, ale klient UX nemá vstupní pole pro ověřovací kód, pak telefonní hovory a oznámení mobilní aplikace jsou dvě podporované možnosti.</span><span class="sxs-lookup"><span data-stu-id="ef250-171">If your RADIUS client supports PAP, but the client UX doesn't have input fields for a verification code, then phone call and mobile app notification are the two supported options.</span></span>

<span data-ttu-id="ef250-172">Můžete [zakázat nepodporované ověřování metody](multi-factor-authentication-whats-next.md#selectable-verification-methods) v Azure.</span><span class="sxs-lookup"><span data-stu-id="ef250-172">You can [disable unsupported authentication methods](multi-factor-authentication-whats-next.md#selectable-verification-methods) in Azure.</span></span>

### <a name="enable-users-for-mfa"></a><span data-ttu-id="ef250-173">Povolit uživatelům pro MFA</span><span class="sxs-lookup"><span data-stu-id="ef250-173">Enable users for MFA</span></span>

<span data-ttu-id="ef250-174">Před nasazením úplné rozšíření serveru NPS, musíte povolit MFA pro uživatele, které chcete provést dvoustupňové ověřování.</span><span class="sxs-lookup"><span data-stu-id="ef250-174">Before you deploy the full NPS extension, you need to enable MFA for the users that you want to perform two-step verification.</span></span> <span data-ttu-id="ef250-175">Více okamžitě k testovací rozšíření, jako je nasadit, potřebujete účet alespoň jeden test, který je plně zaregistrován u služby Multi-Factor Authentication.</span><span class="sxs-lookup"><span data-stu-id="ef250-175">More immediately, to test the extension as you deploy it, you need at least one test account that is fully registered for Multi-Factor Authentication.</span></span>

<span data-ttu-id="ef250-176">Získat účet testu, spuštění pomocí těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="ef250-176">Use these steps to get a test account started:</span></span>
1. <span data-ttu-id="ef250-177">Přihlaste se k [https://aka.ms/mfasetup](https://aka.ms/mfasetup) s testovací účet.</span><span class="sxs-lookup"><span data-stu-id="ef250-177">Sign in to [https://aka.ms/mfasetup](https://aka.ms/mfasetup) with a test account.</span></span> 
2. <span data-ttu-id="ef250-178">Postupujte podle pokynů k nastavení metodu ověřování.</span><span class="sxs-lookup"><span data-stu-id="ef250-178">Follow the prompts to set up a verification method.</span></span>
3. <span data-ttu-id="ef250-179">Buď vytvoření zásady podmíněného přístupu nebo [změnit stav uživatele](multi-factor-authentication-get-started-user-states.md) tak, aby vyžadovala dvoustupňové ověřování pro testovací účet.</span><span class="sxs-lookup"><span data-stu-id="ef250-179">Either create a conditional access policy or [change the user state](multi-factor-authentication-get-started-user-states.md) to require two-step verification for the test account.</span></span> 

<span data-ttu-id="ef250-180">Vaši uživatelé také muset postupovat podle těchto kroků k registraci předtím, než můžete ověřit pomocí rozšíření serveru NPS.</span><span class="sxs-lookup"><span data-stu-id="ef250-180">Your users also need to follow these steps to enroll before they can authenticate with the NPS extension.</span></span>

## <a name="install-the-nps-extension"></a><span data-ttu-id="ef250-181">Nainstalujte rozšíření serveru NPS</span><span class="sxs-lookup"><span data-stu-id="ef250-181">Install the NPS extension</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ef250-182">Nainstalujte rozšíření serveru NPS na jiném serveru než VPN přístupový bod.</span><span class="sxs-lookup"><span data-stu-id="ef250-182">Install the NPS extension on a different server than the VPN access point.</span></span>

### <a name="download-and-install-the-nps-extension-for-azure-mfa"></a><span data-ttu-id="ef250-183">Stáhnout a nainstalovat rozšíření NPS pro Azure MFA</span><span class="sxs-lookup"><span data-stu-id="ef250-183">Download and install the NPS extension for Azure MFA</span></span>

1.  <span data-ttu-id="ef250-184">[Stáhněte si rozšíření NPS](https://aka.ms/npsmfa) z webu Microsoft Download Center.</span><span class="sxs-lookup"><span data-stu-id="ef250-184">[Download the NPS Extension](https://aka.ms/npsmfa) from the Microsoft Download Center.</span></span>
2.  <span data-ttu-id="ef250-185">Zkopírujte binárního souboru na serveru Network Policy Server, kterou chcete konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="ef250-185">Copy the binary to the Network Policy Server you want to configure.</span></span>
3.  <span data-ttu-id="ef250-186">Spustit *setup.exe* a postupujte podle pokynů pro instalaci.</span><span class="sxs-lookup"><span data-stu-id="ef250-186">Run *setup.exe* and follow the installation instructions.</span></span> <span data-ttu-id="ef250-187">Pokud narazíte na chyby, zkontrolujte, že dvě knihovny z části požadovaných součástí byly úspěšně nainstalovány.</span><span class="sxs-lookup"><span data-stu-id="ef250-187">If you encounter errors, double-check that the two libraries from the prerequisite section were successfully installed.</span></span>

### <a name="run-the-powershell-script"></a><span data-ttu-id="ef250-188">Spuštění powershellového skriptu</span><span class="sxs-lookup"><span data-stu-id="ef250-188">Run the PowerShell script</span></span>

<span data-ttu-id="ef250-189">Instalační program vytvoří skript prostředí PowerShell v tomto umístění: `C:\Program Files\Microsoft\AzureMfa\Config` (kde C:\ představuje instalační jednotce).</span><span class="sxs-lookup"><span data-stu-id="ef250-189">The installer creates a PowerShell script in this location: `C:\Program Files\Microsoft\AzureMfa\Config` (where C:\ is your installation drive).</span></span> <span data-ttu-id="ef250-190">Tento skript prostředí PowerShell provede následující akce:</span><span class="sxs-lookup"><span data-stu-id="ef250-190">This PowerShell script performs the following actions:</span></span>

-   <span data-ttu-id="ef250-191">Vytvořte certifikát podepsaný svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="ef250-191">Create a self-signed certificate.</span></span>
-   <span data-ttu-id="ef250-192">Přidružte veřejný klíč certifikátu pro službu objektu zabezpečení v Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ef250-192">Associate the public key of the certificate to the service principal on Azure AD.</span></span>
-   <span data-ttu-id="ef250-193">Uložte certifikát v úložišti certifikátů místního počítače.</span><span class="sxs-lookup"><span data-stu-id="ef250-193">Store the cert in the local machine cert store.</span></span>
-   <span data-ttu-id="ef250-194">Udělit přístup k privátní klíč certifikátu do síťového uživatele.</span><span class="sxs-lookup"><span data-stu-id="ef250-194">Grant access to the certificate’s private key to Network User.</span></span>
-   <span data-ttu-id="ef250-195">Restartujte serveru NPS.</span><span class="sxs-lookup"><span data-stu-id="ef250-195">Restart the NPS.</span></span>

<span data-ttu-id="ef250-196">Pokud chcete používat vlastní certifikáty (namísto certifikáty podepsané svým držitelem, které generuje skript prostředí PowerShell), spusťte skript prostředí PowerShell pro dokončení instalace.</span><span class="sxs-lookup"><span data-stu-id="ef250-196">Unless you want to use your own certificates (instead of the self-signed certificates that the PowerShell script generates), run the PowerShell Script to complete the installation.</span></span> <span data-ttu-id="ef250-197">Pokud instalujete rozšíření na více serverech, každé z nich by měl mít svůj vlastní certifikát.</span><span class="sxs-lookup"><span data-stu-id="ef250-197">If you install the extension on multiple servers, each one should have its own certificate.</span></span>

1. <span data-ttu-id="ef250-198">Spusťte prostředí Windows PowerShell jako správce.</span><span class="sxs-lookup"><span data-stu-id="ef250-198">Run Windows PowerShell as an administrator.</span></span>
2. <span data-ttu-id="ef250-199">Změňte adresáře.</span><span class="sxs-lookup"><span data-stu-id="ef250-199">Change directories.</span></span>

   `cd "C:\Program Files\Microsoft\AzureMfa\Config"`

3. <span data-ttu-id="ef250-200">Spusťte skript prostředí PowerShell vytvořené instalační služby.</span><span class="sxs-lookup"><span data-stu-id="ef250-200">Run the PowerShell script created by the installer.</span></span>

   `.\AzureMfaNpsExtnConfigSetup.ps1`

4. <span data-ttu-id="ef250-201">Prostředí PowerShell vyzve k zadání vašeho ID klienta.</span><span class="sxs-lookup"><span data-stu-id="ef250-201">PowerShell prompts for your tenant ID.</span></span> <span data-ttu-id="ef250-202">Použijte identifikátor GUID ID adresáře, který jste zkopírovali z portálu Azure v části předpoklady.</span><span class="sxs-lookup"><span data-stu-id="ef250-202">Use the Directory ID GUID that you copied from the Azure portal in the prerequisites section.</span></span>
5. <span data-ttu-id="ef250-203">Přihlaste se ke službě Azure AD jako správce.</span><span class="sxs-lookup"><span data-stu-id="ef250-203">Sign in to Azure AD as an administrator.</span></span>
6. <span data-ttu-id="ef250-204">PowerShell zobrazí zpráva o úspěšném provedení až po dokončení skriptu.</span><span class="sxs-lookup"><span data-stu-id="ef250-204">PowerShell shows a success message when the script is finished.</span></span>  

<span data-ttu-id="ef250-205">Opakujte tyto kroky na všechny další servery NPS, které chcete nastavit pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="ef250-205">Repeat these steps on any additional NPS servers that you want to set up for load balancing.</span></span>

>[!NOTE]
><span data-ttu-id="ef250-206">Pokud používáte vlastní certifikáty místo aby generovala certifikáty pomocí skriptu prostředí PowerShell, ujistěte se, že správně zarovnané na serveru NPS zásady vytváření názvů.</span><span class="sxs-lookup"><span data-stu-id="ef250-206">If you use your own certificates instead of generating certificates with the PowerShell script, make sure that they align to the NPS naming convention.</span></span> <span data-ttu-id="ef250-207">Název subjektu musí být **CN =\<TenantID\>, OU = Microsoft NPS rozšíření**.</span><span class="sxs-lookup"><span data-stu-id="ef250-207">The subject name must be **CN=\<TenantID\>,OU=Microsoft NPS Extension**.</span></span> 

## <a name="configure-your-nps-extension"></a><span data-ttu-id="ef250-208">Konfigurace serveru NPS rozšíření</span><span class="sxs-lookup"><span data-stu-id="ef250-208">Configure your NPS extension</span></span>

<span data-ttu-id="ef250-209">Tato část obsahuje důležité informace o návrhu a návrhy pro úspěšné nasazení rozšíření serveru NPS.</span><span class="sxs-lookup"><span data-stu-id="ef250-209">This section includes design considerations and suggestions for successful NPS extension deployments.</span></span>

### <a name="configuration-limitations"></a><span data-ttu-id="ef250-210">Konfigurace omezení</span><span class="sxs-lookup"><span data-stu-id="ef250-210">Configuration limitations</span></span>

- <span data-ttu-id="ef250-211">Server NPS rozšíření pro Azure MFA nezahrnuje nástroje pro migraci uživatelů a nastavení z MFA serveru do cloudu.</span><span class="sxs-lookup"><span data-stu-id="ef250-211">The NPS extension for Azure MFA does not include tools to migrate users and settings from MFA Server to the cloud.</span></span> <span data-ttu-id="ef250-212">Z tohoto důvodu doporučujeme pomocí rozšíření pro nová nasazení, nikoli stávajícího nasazení.</span><span class="sxs-lookup"><span data-stu-id="ef250-212">For this reason, we suggest using the extension for new deployments, rather than existing deployment.</span></span> <span data-ttu-id="ef250-213">Pokud používáte rozšíření na stávajícího nasazení, uživatelé musí provést výš znovu k naplnění jejich podrobnosti MFA v cloudu.</span><span class="sxs-lookup"><span data-stu-id="ef250-213">If you use the extension on an existing deployment, your users have to perform proof-up again to populate their MFA details in the cloud.</span></span>  
- <span data-ttu-id="ef250-214">Rozšíření serveru NPS používá název UPN z místní služby Active directory k identifikaci uživatele v Azure MFA pro provádění sekundární umožňuje Rozšíření lze nakonfigurovat k využívání jiný identifikátor jako alternativního přihlašovacího ID nebo vlastních polí služby Active Directory než UPN.</span><span class="sxs-lookup"><span data-stu-id="ef250-214">The NPS extension uses the UPN from the on-premises Active directory to identify the user on Azure MFA for performing the Secondary Auth. The extension can be configured to use a different identifier like alternate login ID or custom Active Directory field other than UPN.</span></span> <span data-ttu-id="ef250-215">V tématu [rozšířených možnostech konfigurace pro NPS rozšíření pro službu Multi-Factor Authentication](multi-factor-authentication-advanced-vpn-configurations.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="ef250-215">See [Advanced configuration options for the NPS extension for Multi-Factor Authentication](multi-factor-authentication-advanced-vpn-configurations.md) for more information.</span></span>
- <span data-ttu-id="ef250-216">Ne všechny protokoly šifrování podporují všechny metody ověřování.</span><span class="sxs-lookup"><span data-stu-id="ef250-216">Not all encryption protocols support all verification methods.</span></span>
   - <span data-ttu-id="ef250-217">**PAP** podporuje telefonní hovor, jednosměrné textová zpráva, oznámení mobilní aplikace a kód ověření mobilní aplikace</span><span class="sxs-lookup"><span data-stu-id="ef250-217">**PAP** supports phone call, one-way text message, mobile app notification, and mobile app verification code</span></span>
   - <span data-ttu-id="ef250-218">**CHAPV2** a **EAP** podporují telefonní hovory a oznámení mobilní aplikace</span><span class="sxs-lookup"><span data-stu-id="ef250-218">**CHAPV2** and **EAP** support phone call and mobile app notification</span></span>

### <a name="control-radius-clients-that-require-mfa"></a><span data-ttu-id="ef250-219">Ovládací prvek klientů RADIUS, které vyžadují vícefaktorové ověřování</span><span class="sxs-lookup"><span data-stu-id="ef250-219">Control RADIUS clients that require MFA</span></span>

<span data-ttu-id="ef250-220">Jakmile povolíte MFA pro klienta RADIUS pomocí rozšíření serveru NPS, všechny ověřování pro tohoto klienta jsou potřebná k provedení vícefaktorového ověřování.</span><span class="sxs-lookup"><span data-stu-id="ef250-220">Once you enable MFA for a RADIUS client using the NPS Extension, all authentications for this client are required to perform MFA.</span></span> <span data-ttu-id="ef250-221">Pokud chcete povolit MFA pro některé klienty RADIUS a jiné ne, můžete nakonfigurovat dva servery NPS a nainstalovat rozšíření pouze v jedné z nich.</span><span class="sxs-lookup"><span data-stu-id="ef250-221">If you want to enable MFA for some RADIUS clients but not others, you can configure two NPS servers and install the extension on only one of them.</span></span> <span data-ttu-id="ef250-222">Konfigurace klientů RADIUS, které chcete, aby se vyžadovalo k posílání požadavků na server NPS, který je nakonfigurovaný s příponou a další klienti RADIUS na server NPS není nakonfigurované s příponou.</span><span class="sxs-lookup"><span data-stu-id="ef250-222">Configure RADIUS clients that you want to require MFA to send requests to the NPS server configured with the extension, and other RADIUS clients to the NPS server not configured with the extension.</span></span>

### <a name="prepare-for-users-that-arent-enrolled-for-mfa"></a><span data-ttu-id="ef250-223">Příprava pro uživatele, která nejsou zaregistrovaná pro MFA</span><span class="sxs-lookup"><span data-stu-id="ef250-223">Prepare for users that aren't enrolled for MFA</span></span>

<span data-ttu-id="ef250-224">Pokud máte uživatele, která nejsou zaregistrovaná pro MFA, můžete určit, co se stane při pokusu o ověření.</span><span class="sxs-lookup"><span data-stu-id="ef250-224">If you have users that aren't enrolled for MFA, you can determine what happens when they try to authenticate.</span></span> <span data-ttu-id="ef250-225">Použít nastavení registru *REQUIRE_USER_MATCH* v cestě registru *HKLM\Software\Microsoft\AzureMFA* můžete řídit chování funkce.</span><span class="sxs-lookup"><span data-stu-id="ef250-225">Use the registry setting *REQUIRE_USER_MATCH* in the registry path *HKLM\Software\Microsoft\AzureMFA* to control the feature behavior.</span></span> <span data-ttu-id="ef250-226">Toto nastavení má možnost jeden konfigurace:</span><span class="sxs-lookup"><span data-stu-id="ef250-226">This setting has a single configuration option:</span></span>

| <span data-ttu-id="ef250-227">Klíč</span><span class="sxs-lookup"><span data-stu-id="ef250-227">Key</span></span> | <span data-ttu-id="ef250-228">Hodnota</span><span class="sxs-lookup"><span data-stu-id="ef250-228">Value</span></span> | <span data-ttu-id="ef250-229">Výchozí</span><span class="sxs-lookup"><span data-stu-id="ef250-229">Default</span></span> |
| --- | ----- | ------- |
| <span data-ttu-id="ef250-230">REQUIRE_USER_MATCH</span><span class="sxs-lookup"><span data-stu-id="ef250-230">REQUIRE_USER_MATCH</span></span> | <span data-ttu-id="ef250-231">HODNOTU TRUE NEBO FALSE</span><span class="sxs-lookup"><span data-stu-id="ef250-231">TRUE/FALSE</span></span> | <span data-ttu-id="ef250-232">Není nastavena (ekvivalentní na hodnotu TRUE)</span><span class="sxs-lookup"><span data-stu-id="ef250-232">Not set (equivalent to TRUE)</span></span> |

<span data-ttu-id="ef250-233">Účelem tohoto nastavení je určit, co dělat, když uživatel není zaregistrovaný pro MFA.</span><span class="sxs-lookup"><span data-stu-id="ef250-233">The purpose of this setting is to determine what to do when a user is not enrolled for MFA.</span></span> <span data-ttu-id="ef250-234">Pokud klíč neexistuje, není nastavena nebo je nastaven na hodnotu TRUE a uživatel není registrovaný, potom rozšíření selže ověřovací test MFA.</span><span class="sxs-lookup"><span data-stu-id="ef250-234">When the key does not exist, is not set, or is set to TRUE, and the user is not enrolled, then the extension fails the MFA challenge.</span></span> <span data-ttu-id="ef250-235">Když klíč nastaven na hodnotu FALSE a uživatel není registrovaný, ověření pokračuje bez vícefaktorového ověřování.</span><span class="sxs-lookup"><span data-stu-id="ef250-235">When the key is set to FALSE and the user is not enrolled, authentication proceeds without performing MFA.</span></span>

<span data-ttu-id="ef250-236">Můžete vytvořit tento klíč a nastavte ji na hodnotu FALSE, zatímco jsou vaši uživatelé registrace, a ne všechny možné registrovat pro Azure MFA ještě.</span><span class="sxs-lookup"><span data-stu-id="ef250-236">You can choose to create this key and set it to FALSE while your users are onboarding, and may not all be enrolled for Azure MFA yet.</span></span> <span data-ttu-id="ef250-237">Ale vzhledem k tomu, že nastavení klíče umožňuje uživatelům, která nejsou zaregistrovaná pro vícefaktorové ověřování pro přihlášení, byste měli odebrat tento klíč před přechodem do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="ef250-237">However, since setting the key permits users that aren't enrolled for MFA to sign in, you should remove this key before going to production.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="ef250-238">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="ef250-238">Troubleshooting</span></span>

### <a name="how-do-i-verify-that-the-client-cert-is-installed-as-expected"></a><span data-ttu-id="ef250-239">Jak ověřit, zda je nainstalován klientský certifikát, podle očekávání?</span><span class="sxs-lookup"><span data-stu-id="ef250-239">How do I verify that the client cert is installed as expected?</span></span>

<span data-ttu-id="ef250-240">Vyhledejte certifikát podepsaný svým držitelem, které jsou vytvořené instalační program v úložiště certifikátu a zkontrolujte, zda má privátní klíč oprávnění udělená uživateli **síťové služby**.</span><span class="sxs-lookup"><span data-stu-id="ef250-240">Look for the self-signed certificate created by the installer in the cert store, and check that the private key has permissions granted to user **NETWORK SERVICE**.</span></span> <span data-ttu-id="ef250-241">Certifikát obsahuje název subjektu z **CN \<tenantid\>, OU = Microsoft NPS rozšíření**</span><span class="sxs-lookup"><span data-stu-id="ef250-241">The cert has a subject name of **CN \<tenantid\>, OU = Microsoft NPS Extension**</span></span>

-------------------------------------------------------------

### <a name="how-can-i-verify-that-my-client-cert-is-associated-to-my-tenant-in-azure-active-directory"></a><span data-ttu-id="ef250-242">Jak můžete ověřit, že moje klientského certifikátu je přidružena k klienta v Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ef250-242">How can I verify that my client cert is associated to my tenant in Azure Active Directory?</span></span>

<span data-ttu-id="ef250-243">Otevřete příkazový řádek prostředí PowerShell a spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="ef250-243">Open PowerShell command prompt and run the following commands:</span></span>

```
import-module MSOnline
Connect-MsolService
Get-MsolServicePrincipalCredential -AppPrincipalId "981f26a1-7f43-403b-a875-f8b09b8cd720" -ReturnKeyValues 1 
```

<span data-ttu-id="ef250-244">Tyto příkazy vytisknout všechny certifikáty přidružení klientovi s vaší instancí rozšíření serveru NPS v relaci prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ef250-244">These commands print all the certificates associating your tenant with your instance of the NPS extension in your PowerShell session.</span></span> <span data-ttu-id="ef250-245">Vyhledejte certifikát tak, že vyexportujete klientského certifikátu jako soubor "x.509 s kódováním Base-64" bez soukromého klíče a jejímu porovnání s seznamu z prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ef250-245">Look for your certificate by exporting your client cert as a "Base-64 encoded X.509(.cer)" file without the private key, and compare it with the list from PowerShell.</span></span>

<span data-ttu-id="ef250-246">Platný-z a platné – dokud časová razítka, které jsou v podobě čitelný, lze filtrovat zřejmé misfits Pokud příkaz vrátí více než jeden certifikát.</span><span class="sxs-lookup"><span data-stu-id="ef250-246">Valid-From and Valid-Until timestamps, which are in human-readable form, can be used to filter out obvious misfits if the command returns more than one cert.</span></span>

-------------------------------------------------------------

### <a name="why-are-my-requests-failing-with-adal-token-error"></a><span data-ttu-id="ef250-247">Proč se nedaří Moje žádosti s ADAL Chyba tokenu?</span><span class="sxs-lookup"><span data-stu-id="ef250-247">Why are my requests failing with ADAL token error?</span></span>

<span data-ttu-id="ef250-248">Tato chyba může být způsobeno jedním z několika důvodů.</span><span class="sxs-lookup"><span data-stu-id="ef250-248">This error could be due to one of several reasons.</span></span> <span data-ttu-id="ef250-249">Tyto kroky použijte k řešení:</span><span class="sxs-lookup"><span data-stu-id="ef250-249">Use these steps to help troubleshoot:</span></span>

1. <span data-ttu-id="ef250-250">Restartujte NPS server.</span><span class="sxs-lookup"><span data-stu-id="ef250-250">Restart your NPS server.</span></span>
2. <span data-ttu-id="ef250-251">Ověřte, zda je tento certifikát klienta nainstalovány podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="ef250-251">Verify that that client cert is installed as expected.</span></span>
3. <span data-ttu-id="ef250-252">Ověřte, zda je certifikát přidružený vašeho klienta na Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ef250-252">Verify that the certificate is associated with your tenant on Azure AD.</span></span>
4. <span data-ttu-id="ef250-253">Ověřte, zda že je přístupná ze serveru s příponou této https://login.microsoftonline.com/.</span><span class="sxs-lookup"><span data-stu-id="ef250-253">Verify that https://login.microsoftonline.com/ is accessible from the server running the extension.</span></span>

-------------------------------------------------------------

### <a name="why-does-authentication-fail-with-an-error-in-http-logs-stating-that-the-user-is-not-found"></a><span data-ttu-id="ef250-254">Proč ověřování nezdaří s chybou v protokolech HTTP s oznámením, že uživatel není nalezen?</span><span class="sxs-lookup"><span data-stu-id="ef250-254">Why does authentication fail with an error in HTTP logs stating that the user is not found?</span></span>

<span data-ttu-id="ef250-255">Ověřte, zda je spuštěna AD Connect a že uživatel je součástí Windows Active Directory a Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ef250-255">Verify that AD Connect is running, and that the user is present in both Windows Active Directory and Azure Active Directory.</span></span>

------------------------------------------------------------

### <a name="why-do-i-see-http-connect-errors-in-logs-with-all-my-authentications-failing"></a><span data-ttu-id="ef250-256">Proč vidí HTTP připojit chyby v protokolech s Moje ověřování selhání?</span><span class="sxs-lookup"><span data-stu-id="ef250-256">Why do I see HTTP connect errors in logs with all my authentications failing?</span></span>

<span data-ttu-id="ef250-257">Ověřte, že ze serveru se spuštěným rozšíření NPS je přístupný web https://adnotifications.windowsazure.com.</span><span class="sxs-lookup"><span data-stu-id="ef250-257">Verify that https://adnotifications.windowsazure.com is reachable from the server running the NPS extension.</span></span>


## <a name="next-steps"></a><span data-ttu-id="ef250-258">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ef250-258">Next steps</span></span>

- <span data-ttu-id="ef250-259">Konfigurovat alternativní ID pro přihlášení, nebo nastavit seznam výjimek pro IP adresy, který by neměl provádět dvoustupňové ověření v [rozšířených možnostech konfigurace pro NPS rozšíření pro službu Multi-Factor Authentication](nps-extension-advanced-configuration.md)</span><span class="sxs-lookup"><span data-stu-id="ef250-259">Configure alternate IDs for login, or set up an exception list for IPs that shouldn't perform two-step verification in [Advanced configuration options for the NPS extension for Multi-Factor Authentication](nps-extension-advanced-configuration.md)</span></span>

- [<span data-ttu-id="ef250-260">řešení chybových zpráv z rozšíření NPS pro Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="ef250-260">Resolve error messages from the NPS extension for Azure Multi-Factor Authentication</span></span>](multi-factor-authentication-nps-errors.md)
