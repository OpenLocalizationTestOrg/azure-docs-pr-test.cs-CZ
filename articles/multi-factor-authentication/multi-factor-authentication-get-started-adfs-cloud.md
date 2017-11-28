---
title: "aaaSecure cloudových prostředků s Azure MFA a AD FS | Microsoft Docs"
description: "Toto je stránka Azure Multi-Factor authentication hello, který popisuje, jak tooget pracovat s Azure MFA a AD FS v cloudu hello."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 0927fc67-8090-4fdd-913a-b3cfed3fbe77
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/29/2017
ms.author: kgremban
ms.openlocfilehash: 8d38d6a4af63ddcaf0fefded0b73d82d5178aa36
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="securing-cloud-resources-with-azure-multi-factor-authentication-and-ad-fs"></a><span data-ttu-id="4032c-103">Zabezpečení cloudových prostředků s Azure Multi-Factor Authentication a AD FS</span><span class="sxs-lookup"><span data-stu-id="4032c-103">Securing cloud resources with Azure Multi-Factor Authentication and AD FS</span></span>
<span data-ttu-id="4032c-104">Pokud je vaše organizace Federovaná pomocí Azure Active Directory, použijte ověřování Azure Multi-Factor Authentication nebo Active Directory Federation Services (AD FS) toosecure prostředky, které jsou dostupné přes Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4032c-104">If your organization is federated with Azure Active Directory, use Azure Multi-Factor Authentication or Active Directory Federation Services (AD FS) toosecure resources that are accessed by Azure AD.</span></span> <span data-ttu-id="4032c-105">Použijte následující postupy toosecure prostředků Azure Active Directory s Azure Multi-Factor Authentication nebo Active Directory Federation Services hello.</span><span class="sxs-lookup"><span data-stu-id="4032c-105">Use hello following procedures toosecure Azure Active Directory resources with either Azure Multi-Factor Authentication or Active Directory Federation Services.</span></span>

## <a name="secure-azure-ad-resources-using-ad-fs"></a><span data-ttu-id="4032c-106">Zabezpečení prostředků Azure AD pomocí služby AD FS</span><span class="sxs-lookup"><span data-stu-id="4032c-106">Secure Azure AD resources using AD FS</span></span>
<span data-ttu-id="4032c-107">toosecure vaše prostředek v cloudu, nastavit pravidlo deklarací identity, aby Active Directory Federation Services vysílá hello multipleauthn deklarací, když uživatel provede dvoustupňové ověření úspěšně.</span><span class="sxs-lookup"><span data-stu-id="4032c-107">toosecure your cloud resource, set up a claims rule so that Active Directory Federation Services emits hello multipleauthn claim when a user performs two-step verification successfully.</span></span> <span data-ttu-id="4032c-108">Tento požadavek je předán na tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="4032c-108">This claim is passed on tooAzure AD.</span></span> <span data-ttu-id="4032c-109">Použijte tento postup toowalk prostřednictvím hello kroky:</span><span class="sxs-lookup"><span data-stu-id="4032c-109">Follow this procedure toowalk through hello steps:</span></span>


1. <span data-ttu-id="4032c-110">Otevřete správu služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="4032c-110">Open AD FS Management.</span></span>
2. <span data-ttu-id="4032c-111">Na levé straně hello vyberte **vztahy důvěryhodnosti předávající strany**.</span><span class="sxs-lookup"><span data-stu-id="4032c-111">On hello left, select **Relying Party Trusts**.</span></span>
3. <span data-ttu-id="4032c-112">Klikněte pravým tlačítkem na **Platforma identit Microsoft Office 365** a vyberte **Upravit pravidla deklarací identity**.</span><span class="sxs-lookup"><span data-stu-id="4032c-112">Right-click on **Microsoft Office 365 Identity Platform** and select **Edit Claim Rules**.</span></span>

   ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)

4. <span data-ttu-id="4032c-114">V pravidlech transformace vystavení klikněte na **Přidat pravidlo**.</span><span class="sxs-lookup"><span data-stu-id="4032c-114">On Issuance Transform Rules, click **Add Rule**.</span></span>

   ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)

5. <span data-ttu-id="4032c-116">V průvodci Přidat transformované deklarace identity pravidlo text hello, vyberte **předávat nebo filtrovat příchozí deklarace identity** z hello rozevíracího seznamu a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="4032c-116">On hello Add Transform Claim Rule Wizard, select **Pass Through or Filter an Incoming Claim** from hello drop-down and click **Next**.</span></span>

   ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)

6. <span data-ttu-id="4032c-118">Pojmenujte pravidlo.</span><span class="sxs-lookup"><span data-stu-id="4032c-118">Give your rule a name.</span></span> 
7. <span data-ttu-id="4032c-119">Vyberte **odkazy na metody ověřování** jako hello příchozí typ deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="4032c-119">Select **Authentication Methods References** as hello Incoming claim type.</span></span>
8. <span data-ttu-id="4032c-120">Vyberte **Předávat všechny hodnoty deklarací identity**.</span><span class="sxs-lookup"><span data-stu-id="4032c-120">Select **Pass through all claim values**.</span></span>
    <span data-ttu-id="4032c-121">![	Průvodce přidáním pravidla – deklarace identity transformace](./media/multi-factor-authentication-get-started-adfs-cloud/configurewizard.png)</span><span class="sxs-lookup"><span data-stu-id="4032c-121">![Add Transform Claim Rule Wizard](./media/multi-factor-authentication-get-started-adfs-cloud/configurewizard.png)</span></span>
9. <span data-ttu-id="4032c-122">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="4032c-122">Click **Finish**.</span></span> <span data-ttu-id="4032c-123">Zavřete konzolu hello AD FS.</span><span class="sxs-lookup"><span data-stu-id="4032c-123">Close hello AD FS Management console.</span></span>

## <a name="trusted-ips-for-federated-users"></a><span data-ttu-id="4032c-124">Důvěryhodné IP adresy pro federované uživatele</span><span class="sxs-lookup"><span data-stu-id="4032c-124">Trusted IPs for federated users</span></span>
<span data-ttu-id="4032c-125">Důvěryhodné IP adresy umožňují správcům tooby průchodu dvoustupňové ověřování pro konkrétní IP adresy, nebo pro federované uživatele, kteří mají požadavky pocházejících z vlastního intranetu.</span><span class="sxs-lookup"><span data-stu-id="4032c-125">Trusted IPs allow administrators tooby-pass two-step verification for specific IP addresses, or for federated users that have requests originating from within their own intranet.</span></span> <span data-ttu-id="4032c-126">Hello následující části popisují, jak tooconfigure Azure Multi-Factor Authentication důvěryhodné IP adresy s federovanými uživateli a obejít dvoustupňové ověření, když požadavek pochází z uvnitř intranetu federovaného uživatele.</span><span class="sxs-lookup"><span data-stu-id="4032c-126">hello following sections describe how tooconfigure Azure Multi-Factor Authentication Trusted IPs with federated users and by-pass two-step verification when a request originates from within a federated users intranet.</span></span> <span data-ttu-id="4032c-127">Toho dosáhnete pomocí konfigurace služby AD FS toouse průchozí nebo filtru příchozí šablony deklarace identity s hello typ deklarace identity uvnitř podnikové sítě.</span><span class="sxs-lookup"><span data-stu-id="4032c-127">This is achieved by configuring AD FS toouse a pass-through or filter an incoming claim template with hello Inside Corporate Network claim type.</span></span>

<span data-ttu-id="4032c-128">Tento příklad používá Office 365 pro naše trusty přijímající strany.</span><span class="sxs-lookup"><span data-stu-id="4032c-128">This example uses Office 365 for our Relying Party Trusts.</span></span>

### <a name="configure-hello-ad-fs-claims-rules"></a><span data-ttu-id="4032c-129">Konfigurace pravidel deklarací identity hello služby AD FS</span><span class="sxs-lookup"><span data-stu-id="4032c-129">Configure hello AD FS claims rules</span></span>
<span data-ttu-id="4032c-130">První věc Hello potřebujeme toodo je tooconfigure hello služby AD FS deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="4032c-130">hello first thing we need toodo is tooconfigure hello AD FS claims.</span></span> <span data-ttu-id="4032c-131">Vytvoříte dvě pravidla deklarace identity, jeden pro hello uvnitř podnikové sítě deklarace identity, typ a další pro zachování přihlášení uživatelů.</span><span class="sxs-lookup"><span data-stu-id="4032c-131">Create two claims rules, one for hello Inside Corporate Network claim type and an additional one for keeping our users signed in.</span></span>

1. <span data-ttu-id="4032c-132">Otevřete správu služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="4032c-132">Open AD FS Management.</span></span>
2. <span data-ttu-id="4032c-133">Na levé straně hello vyberte **vztahy důvěryhodnosti předávající strany**.</span><span class="sxs-lookup"><span data-stu-id="4032c-133">On hello left, select **Relying Party Trusts**.</span></span>
3. <span data-ttu-id="4032c-134">Klikněte pravým tlačítkem na **Platforma identit Microsoft Office 365** a vyberte **Upravit pravidla deklarací identity…**
   ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)</span><span class="sxs-lookup"><span data-stu-id="4032c-134">Right-click on **Microsoft Office 365 Identity Platform** and select **Edit Claim Rules…**
![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)</span></span>
4. <span data-ttu-id="4032c-135">V pravidlech transformace vystavení klikněte na **Přidat pravidlo.**
   ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)</span><span class="sxs-lookup"><span data-stu-id="4032c-135">On Issuance Transform Rules, click **Add Rule.**
![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)</span></span>
5. <span data-ttu-id="4032c-136">V průvodci Přidat transformované deklarace identity pravidlo text hello, vyberte **předávat nebo filtrovat příchozí deklarace identity** z hello rozevíracího seznamu a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="4032c-136">On hello Add Transform Claim Rule Wizard, select **Pass Through or Filter an Incoming Claim** from hello drop-down and click **Next**.</span></span>
   <span data-ttu-id="4032c-137">![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)</span><span class="sxs-lookup"><span data-stu-id="4032c-137">![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)</span></span>
6. <span data-ttu-id="4032c-138">V hello pole Další tooClaim název pravidla pojmenujte pravidla.</span><span class="sxs-lookup"><span data-stu-id="4032c-138">In hello box next tooClaim rule name, give your rule a name.</span></span> <span data-ttu-id="4032c-139">Příklad: InsideCorpNet.</span><span class="sxs-lookup"><span data-stu-id="4032c-139">For example: InsideCorpNet.</span></span>
7. <span data-ttu-id="4032c-140">Z rozevíracího seznamu hello, další tooIncoming typ deklarace identity, vyberte **uvnitř podnikové sítě**.</span><span class="sxs-lookup"><span data-stu-id="4032c-140">From hello drop-down, next tooIncoming claim type, select **Inside Corporate Network**.</span></span>
   <span data-ttu-id="4032c-141">![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip4.png)</span><span class="sxs-lookup"><span data-stu-id="4032c-141">![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip4.png)</span></span>
8. <span data-ttu-id="4032c-142">Klikněte na **Finish** (Dokončit).</span><span class="sxs-lookup"><span data-stu-id="4032c-142">Click **Finish**.</span></span>
9. <span data-ttu-id="4032c-143">V pravidlech transformace vystavení klikněte na **Přidat pravidlo**.</span><span class="sxs-lookup"><span data-stu-id="4032c-143">On Issuance Transform Rules, click **Add Rule**.</span></span>
10. <span data-ttu-id="4032c-144">V průvodci Přidat transformované deklarace identity pravidlo text hello, vyberte **odesílat deklarace pomocí vlastního pravidla** z hello rozevíracího seznamu a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="4032c-144">On hello Add Transform Claim Rule Wizard, select **Send Claims Using a Custom Rule** from hello drop-down and click **Next**.</span></span>
11. <span data-ttu-id="4032c-145">V poli hello pod položkou Název pravidla deklarace identity: Zadejte *zachovat přihlášené uživatele*.</span><span class="sxs-lookup"><span data-stu-id="4032c-145">In hello box under Claim rule name: enter *Keep Users Signed In*.</span></span>
12. <span data-ttu-id="4032c-146">V hello vlastní pravidlo zadejte:</span><span class="sxs-lookup"><span data-stu-id="4032c-146">In hello Custom rule box, enter:</span></span>

        c:[Type == "http://schemas.microsoft.com/2014/03/psso"]
            => issue(claim = c);
    ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip5.png)
13. <span data-ttu-id="4032c-148">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="4032c-148">Click **Finish**.</span></span>
14. <span data-ttu-id="4032c-149">Klikněte na tlačítko **Použít**.</span><span class="sxs-lookup"><span data-stu-id="4032c-149">Click **Apply**.</span></span>
15. <span data-ttu-id="4032c-150">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4032c-150">Click **Ok**.</span></span>
16. <span data-ttu-id="4032c-151">Zavřete správu služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="4032c-151">Close AD FS Management.</span></span>

### <a name="configure-azure-multi-factor-authentication-trusted-ips-with-federated-users"></a><span data-ttu-id="4032c-152">Konfigurovat důvěryhodné IP adresy ověřování Azure Multi-Factor Authentication s federovanými uživateli</span><span class="sxs-lookup"><span data-stu-id="4032c-152">Configure Azure Multi-Factor Authentication Trusted IPs with Federated Users</span></span>
<span data-ttu-id="4032c-153">Teď, když hello deklarace identity jsou na místě, můžeme konfigurovat důvěryhodné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="4032c-153">Now that hello claims are in place, we can configure trusted IPs.</span></span>

1. <span data-ttu-id="4032c-154">Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="4032c-154">Sign in toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="4032c-155">Na levé straně hello, klikněte na tlačítko **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="4032c-155">On hello left, click **Active Directory**.</span></span>
3. <span data-ttu-id="4032c-156">V adresáři vyberte hello adresář, kam chcete tooset až důvěryhodné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="4032c-156">Under Directory, select hello directory where you want tooset up trusted IPs.</span></span>
4. <span data-ttu-id="4032c-157">Na hello adresáře, které jste vybrali, klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="4032c-157">On hello Directory you have selected, click **Configure**.</span></span>
5. <span data-ttu-id="4032c-158">V části hello vícefaktorového ověřování, klikněte na tlačítko **spravovat nastavení služby**.</span><span class="sxs-lookup"><span data-stu-id="4032c-158">In hello multi-factor authentication section, click **Manage service settings**.</span></span>
6. <span data-ttu-id="4032c-159">Na stránce nastavení služby hello v rámci důvěryhodných adres IP, vyberte **přeskočit více-factor-ověřování pro žádosti od federovaných uživatelů v mém intranetu**.</span><span class="sxs-lookup"><span data-stu-id="4032c-159">On hello Service Settings page, under trusted IPs, select **Skip multi-factor-authentication for requests from federated users on my intranet**.</span></span>  

   ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip6.png)
   
7. <span data-ttu-id="4032c-161">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="4032c-161">Click **save**.</span></span>
8. <span data-ttu-id="4032c-162">Jakmile byly použity hello aktualizace, klikněte na možnost **zavřete**.</span><span class="sxs-lookup"><span data-stu-id="4032c-162">Once hello updates have been applied, click **close**.</span></span>

<span data-ttu-id="4032c-163">A to je vše!</span><span class="sxs-lookup"><span data-stu-id="4032c-163">That’s it!</span></span> <span data-ttu-id="4032c-164">V tomto okamžiku federovaní uživatelé služeb Office 365 by měl mít pouze toouse MFA při deklarace identity pochází z mimo hello podnikovém intranetu.</span><span class="sxs-lookup"><span data-stu-id="4032c-164">At this point, federated Office 365 users should only have toouse MFA when a claim originates from outside hello corporate intranet.</span></span>
