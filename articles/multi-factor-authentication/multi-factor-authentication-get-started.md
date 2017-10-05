---
title: "Výběr mezi cloudovým a serverovým Azure MFA | Dokumentace Microsoftu"
description: "Zvolte pro vás ideální řešení zabezpečení Multi-Factor Authentication položením otázky, co se pokoušíte zabezpečit a kde se nachází vaši uživatelé.  Pak vyberte cloud, server MFA nebo AD FS."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: ec2270ea-13d7-4ebc-8a00-fa75ce6c746d
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/23/2017
ms.author: kgremban
ms.openlocfilehash: 6f8ee3449244b12d2c8b5714e6ad893e2f0b10ee
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="choose-the-azure-multi-factor-authentication-solution-for-you"></a><span data-ttu-id="39876-104">Výběr vhodného řešení Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="39876-104">Choose the Azure Multi-Factor Authentication solution for you</span></span>
<span data-ttu-id="39876-105">Vzhledem k tomu, že existuje několik typů Azure Multi-Factor Authentication (MFA), musíme odpovědět na několik otázek, aby bylo možné zjistit, která verze je pro vás nejlepší.</span><span class="sxs-lookup"><span data-stu-id="39876-105">Because there are several flavors of Azure Multi-Factor Authentication (MFA), we must answer a few questions to figure out which version is the proper one to use.</span></span>  <span data-ttu-id="39876-106">Jsou to tyto otázky:</span><span class="sxs-lookup"><span data-stu-id="39876-106">Those questions are:</span></span>

* [<span data-ttu-id="39876-107">Co se pokouším zabezpečit?</span><span class="sxs-lookup"><span data-stu-id="39876-107">What am I trying to secure</span></span>](#what-am-i-trying-to-secure)
* [<span data-ttu-id="39876-108">Kde se nacházejí uživatelé?</span><span class="sxs-lookup"><span data-stu-id="39876-108">Where are the users located</span></span>](#where-are-the-users-located)
* [<span data-ttu-id="39876-109">Jaké funkce budu potřebovat?</span><span class="sxs-lookup"><span data-stu-id="39876-109">What features do I need?</span></span>](#what-featured-do-i-need)

<span data-ttu-id="39876-110">V následujících částech najdete tipy k zodpovězení každé z těchto otázek.</span><span class="sxs-lookup"><span data-stu-id="39876-110">The following sections provide guidance on determining each of these answers.</span></span>

## <a name="what-am-i-trying-to-secure"></a><span data-ttu-id="39876-111">Co se pokouším zabezpečit?</span><span class="sxs-lookup"><span data-stu-id="39876-111">What am I trying to secure?</span></span>
<span data-ttu-id="39876-112">Pro určení správného řešení dvoustupňového ověření si nejdříve musíme odpovědět na otázku, co se pokoušíte zabezpečit pomocí druhé metody ověřování.</span><span class="sxs-lookup"><span data-stu-id="39876-112">To determine the correct two-step verification solution, first we must answer the question of what are you trying to secure with a second method of authentication.</span></span>  <span data-ttu-id="39876-113">Jedná se o aplikaci, která je v Azure?</span><span class="sxs-lookup"><span data-stu-id="39876-113">Is it an application that is in Azure?</span></span>  <span data-ttu-id="39876-114">Nebo systém vzdáleného přístupu?</span><span class="sxs-lookup"><span data-stu-id="39876-114">Or a remote access system?</span></span>  <span data-ttu-id="39876-115">Tím, že určíme, co se pokoušíme zabezpečit, si můžeme odpovědět na otázku, kde musí být povoleno ověřování Multi-Factor Authentication.</span><span class="sxs-lookup"><span data-stu-id="39876-115">By determining what we are trying to secure, we can answer the question of where Multi-Factor Authentication needs to be enabled.</span></span>  

| <span data-ttu-id="39876-116">Co se pokoušíte zabezpečit</span><span class="sxs-lookup"><span data-stu-id="39876-116">What are you trying to secure</span></span> | <span data-ttu-id="39876-117">MFA v cloudu</span><span class="sxs-lookup"><span data-stu-id="39876-117">MFA in the cloud</span></span> | <span data-ttu-id="39876-118">Server MFA</span><span class="sxs-lookup"><span data-stu-id="39876-118">MFA Server</span></span> |
| --- |:---:|:---:|
| <span data-ttu-id="39876-119">Vlastní aplikace Microsoftu</span><span class="sxs-lookup"><span data-stu-id="39876-119">First-party Microsoft apps</span></span> |<span data-ttu-id="39876-120">●</span><span class="sxs-lookup"><span data-stu-id="39876-120">●</span></span> |<span data-ttu-id="39876-121">●</span><span class="sxs-lookup"><span data-stu-id="39876-121">●</span></span> |
| <span data-ttu-id="39876-122">Aplikace Saas v galerii aplikací</span><span class="sxs-lookup"><span data-stu-id="39876-122">SaaS apps in the app gallery</span></span> |<span data-ttu-id="39876-123">●</span><span class="sxs-lookup"><span data-stu-id="39876-123">●</span></span> |  |
| <span data-ttu-id="39876-124">Webové aplikace publikované prostřednictvím proxy aplikace Azure AD</span><span class="sxs-lookup"><span data-stu-id="39876-124">Web applications published through Azure AD App Proxy</span></span> |<span data-ttu-id="39876-125">●</span><span class="sxs-lookup"><span data-stu-id="39876-125">●</span></span> |  |
| <span data-ttu-id="39876-126">Aplikace služby IIS nepublikované prostřednictvím proxy aplikace Azure AD</span><span class="sxs-lookup"><span data-stu-id="39876-126">IIS applications not published through Azure AD App Proxy</span></span> | |<span data-ttu-id="39876-127">●</span><span class="sxs-lookup"><span data-stu-id="39876-127">●</span></span> |
| <span data-ttu-id="39876-128">Vzdálený přístup, jako je například síť VPN, RDG</span><span class="sxs-lookup"><span data-stu-id="39876-128">Remote access such as VPN, RDG</span></span> | <span data-ttu-id="39876-129">●</span><span class="sxs-lookup"><span data-stu-id="39876-129">●</span></span> | <span data-ttu-id="39876-130">●</span><span class="sxs-lookup"><span data-stu-id="39876-130">●</span></span> |

## <a name="where-are-the-users-located"></a><span data-ttu-id="39876-131">Kde se nachází uživatelé</span><span class="sxs-lookup"><span data-stu-id="39876-131">Where are the users located</span></span>
<span data-ttu-id="39876-132">Když se dále podíváme, kde se naši uživatelé nacházejí, pomůže nám to určit správné řešení – jestli je to v cloudu nebo místně pomocí serveru MFA.</span><span class="sxs-lookup"><span data-stu-id="39876-132">Next, looking at where our users are located helps to determine the correct solution to use, whether in the cloud or on-premises using the MFA Server.</span></span>

| <span data-ttu-id="39876-133">Umístění uživatele</span><span class="sxs-lookup"><span data-stu-id="39876-133">User Location</span></span> | <span data-ttu-id="39876-134">MFA v cloudu</span><span class="sxs-lookup"><span data-stu-id="39876-134">MFA in the cloud</span></span> | <span data-ttu-id="39876-135">Server MFA</span><span class="sxs-lookup"><span data-stu-id="39876-135">MFA Server</span></span> |
| --- |:---:|:---:|
| <span data-ttu-id="39876-136">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="39876-136">Azure Active Directory</span></span> |<span data-ttu-id="39876-137">●</span><span class="sxs-lookup"><span data-stu-id="39876-137">●</span></span> | |
| <span data-ttu-id="39876-138">Azure AD a místní AD využívající federaci se službou AD FS</span><span class="sxs-lookup"><span data-stu-id="39876-138">Azure AD and on-premises AD using federation with AD FS</span></span> |<span data-ttu-id="39876-139">●</span><span class="sxs-lookup"><span data-stu-id="39876-139">●</span></span> |<span data-ttu-id="39876-140">●</span><span class="sxs-lookup"><span data-stu-id="39876-140">●</span></span> |
| <span data-ttu-id="39876-141">Azure AD a místní AD pomocí DirSync Azure AD Sync Azure AD Connect – bez synchronizace hesla</span><span class="sxs-lookup"><span data-stu-id="39876-141">Azure AD and on-premises AD using DirSync, Azure AD Sync, Azure AD Connect - no password sync</span></span> |<span data-ttu-id="39876-142">●</span><span class="sxs-lookup"><span data-stu-id="39876-142">●</span></span> |<span data-ttu-id="39876-143">●</span><span class="sxs-lookup"><span data-stu-id="39876-143">●</span></span> |
| <span data-ttu-id="39876-144">Azure AD a místní AD pomocí DirSync Azure AD Sync Azure AD Connect – se synchronizací hesla</span><span class="sxs-lookup"><span data-stu-id="39876-144">Azure AD and on-premises AD using DirSync, Azure AD Sync, Azure AD Connect - with password sync</span></span> |<span data-ttu-id="39876-145">●</span><span class="sxs-lookup"><span data-stu-id="39876-145">●</span></span> | |
| <span data-ttu-id="39876-146">Místní služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="39876-146">On-premises Active Directory</span></span> | |<span data-ttu-id="39876-147">●</span><span class="sxs-lookup"><span data-stu-id="39876-147">●</span></span> |

## <a name="what-features-do-i-need"></a><span data-ttu-id="39876-148">Jaké funkce budu potřebovat?</span><span class="sxs-lookup"><span data-stu-id="39876-148">What features do I need?</span></span>
<span data-ttu-id="39876-149">Následující tabulka porovnává funkce, které jsou dostupné se službou Multi-Factor Authentication v cloudu a s Multi-Factor Authentication Serverem.</span><span class="sxs-lookup"><span data-stu-id="39876-149">The following table compares the features that are available with Multi-Factor Authentication in the cloud and with the Multi-Factor Authentication Server.</span></span>

| <span data-ttu-id="39876-150">Funkce</span><span class="sxs-lookup"><span data-stu-id="39876-150">Feature</span></span> | <span data-ttu-id="39876-151">MFA v cloudu</span><span class="sxs-lookup"><span data-stu-id="39876-151">MFA in the cloud</span></span> | <span data-ttu-id="39876-152">Server MFA</span><span class="sxs-lookup"><span data-stu-id="39876-152">MFA Server</span></span> |
| --- |:---:|:---:|
| <span data-ttu-id="39876-153">Oznámení mobilní aplikace jako druhý faktor</span><span class="sxs-lookup"><span data-stu-id="39876-153">Mobile app notification as a second factor</span></span> | <span data-ttu-id="39876-154">●</span><span class="sxs-lookup"><span data-stu-id="39876-154">●</span></span> | <span data-ttu-id="39876-155">●</span><span class="sxs-lookup"><span data-stu-id="39876-155">●</span></span> |
| <span data-ttu-id="39876-156">Kód ověření mobilní aplikace jako druhý faktor</span><span class="sxs-lookup"><span data-stu-id="39876-156">Mobile app verification code as a second factor</span></span> | <span data-ttu-id="39876-157">●</span><span class="sxs-lookup"><span data-stu-id="39876-157">●</span></span> | <span data-ttu-id="39876-158">●</span><span class="sxs-lookup"><span data-stu-id="39876-158">●</span></span> |
| <span data-ttu-id="39876-159">Telefonní hovor jako druhý faktor</span><span class="sxs-lookup"><span data-stu-id="39876-159">Phone call as second factor</span></span> | <span data-ttu-id="39876-160">●</span><span class="sxs-lookup"><span data-stu-id="39876-160">●</span></span> | <span data-ttu-id="39876-161">●</span><span class="sxs-lookup"><span data-stu-id="39876-161">●</span></span> |
| <span data-ttu-id="39876-162">Jednosměrné služby SMS jako druhý faktor</span><span class="sxs-lookup"><span data-stu-id="39876-162">One-way SMS as second factor</span></span> | <span data-ttu-id="39876-163">●</span><span class="sxs-lookup"><span data-stu-id="39876-163">●</span></span> | <span data-ttu-id="39876-164">●</span><span class="sxs-lookup"><span data-stu-id="39876-164">●</span></span> |
| <span data-ttu-id="39876-165">Obousměrné služby SMS jako druhý faktor</span><span class="sxs-lookup"><span data-stu-id="39876-165">Two-way SMS as second factor</span></span> | | <span data-ttu-id="39876-166">●</span><span class="sxs-lookup"><span data-stu-id="39876-166">●</span></span> |
| <span data-ttu-id="39876-167">Tokeny hardwaru jako druhý faktor</span><span class="sxs-lookup"><span data-stu-id="39876-167">Hardware Tokens as second factor</span></span> | | <span data-ttu-id="39876-168">●</span><span class="sxs-lookup"><span data-stu-id="39876-168">●</span></span> |
| <span data-ttu-id="39876-169">Hesla aplikací pro klienty Office 365, kteří nepodporují MFA</span><span class="sxs-lookup"><span data-stu-id="39876-169">App passwords for Office 365 clients that don’t support MFA</span></span> | <span data-ttu-id="39876-170">●</span><span class="sxs-lookup"><span data-stu-id="39876-170">●</span></span> | |
| <span data-ttu-id="39876-171">Kontrola správce nad metodami ověřování</span><span class="sxs-lookup"><span data-stu-id="39876-171">Admin control over authentication methods</span></span> | <span data-ttu-id="39876-172">●</span><span class="sxs-lookup"><span data-stu-id="39876-172">●</span></span> | <span data-ttu-id="39876-173">●</span><span class="sxs-lookup"><span data-stu-id="39876-173">●</span></span> |
| <span data-ttu-id="39876-174">Režim kódu PIN</span><span class="sxs-lookup"><span data-stu-id="39876-174">PIN mode</span></span> | | <span data-ttu-id="39876-175">●</span><span class="sxs-lookup"><span data-stu-id="39876-175">●</span></span> |
| <span data-ttu-id="39876-176">Výstraha podvodů</span><span class="sxs-lookup"><span data-stu-id="39876-176">Fraud alert</span></span> |<span data-ttu-id="39876-177">●</span><span class="sxs-lookup"><span data-stu-id="39876-177">●</span></span> | <span data-ttu-id="39876-178">●</span><span class="sxs-lookup"><span data-stu-id="39876-178">●</span></span> |
| <span data-ttu-id="39876-179">Sestavy MFA</span><span class="sxs-lookup"><span data-stu-id="39876-179">MFA Reports</span></span> |<span data-ttu-id="39876-180">●</span><span class="sxs-lookup"><span data-stu-id="39876-180">●</span></span> | <span data-ttu-id="39876-181">●</span><span class="sxs-lookup"><span data-stu-id="39876-181">●</span></span> |
| <span data-ttu-id="39876-182">Jednorázové potlačení</span><span class="sxs-lookup"><span data-stu-id="39876-182">One-Time Bypass</span></span> | | <span data-ttu-id="39876-183">●</span><span class="sxs-lookup"><span data-stu-id="39876-183">●</span></span> |
| <span data-ttu-id="39876-184">Vlastní přivítání pro telefonní hovory</span><span class="sxs-lookup"><span data-stu-id="39876-184">Custom greetings for phone calls</span></span> | <span data-ttu-id="39876-185">●</span><span class="sxs-lookup"><span data-stu-id="39876-185">●</span></span> | <span data-ttu-id="39876-186">●</span><span class="sxs-lookup"><span data-stu-id="39876-186">●</span></span> |
| <span data-ttu-id="39876-187">Přizpůsobitelné ID volajícího pro telefonní hovory</span><span class="sxs-lookup"><span data-stu-id="39876-187">Customizable caller ID for phone calls</span></span> | <span data-ttu-id="39876-188">●</span><span class="sxs-lookup"><span data-stu-id="39876-188">●</span></span> | <span data-ttu-id="39876-189">●</span><span class="sxs-lookup"><span data-stu-id="39876-189">●</span></span> |
| <span data-ttu-id="39876-190">Důvěryhodné IP adresy</span><span class="sxs-lookup"><span data-stu-id="39876-190">Trusted IPs</span></span> | <span data-ttu-id="39876-191">●</span><span class="sxs-lookup"><span data-stu-id="39876-191">●</span></span> | <span data-ttu-id="39876-192">●</span><span class="sxs-lookup"><span data-stu-id="39876-192">●</span></span> |
| <span data-ttu-id="39876-193">Zapamatovat MFA pro důvěryhodná zařízení</span><span class="sxs-lookup"><span data-stu-id="39876-193">Remember MFA for trusted devices</span></span> | <span data-ttu-id="39876-194">●</span><span class="sxs-lookup"><span data-stu-id="39876-194">●</span></span> | |
| <span data-ttu-id="39876-195">Podmíněný přístup</span><span class="sxs-lookup"><span data-stu-id="39876-195">Conditional access</span></span> | <span data-ttu-id="39876-196">●</span><span class="sxs-lookup"><span data-stu-id="39876-196">●</span></span> | <span data-ttu-id="39876-197">●</span><span class="sxs-lookup"><span data-stu-id="39876-197">●</span></span> |
| <span data-ttu-id="39876-198">Mezipaměť</span><span class="sxs-lookup"><span data-stu-id="39876-198">Cache</span></span> |  | <span data-ttu-id="39876-199">●</span><span class="sxs-lookup"><span data-stu-id="39876-199">●</span></span> |

## <a name="next-steps"></a><span data-ttu-id="39876-200">Další kroky</span><span class="sxs-lookup"><span data-stu-id="39876-200">Next steps</span></span>

<span data-ttu-id="39876-201">Teď, když jsme určili, zda chcete použít cloudové  multi-factor authentication nebo server multi-factor authentication na místě, můžete začít s nastavováním a používáním Azure Multi-Factor Authentication.</span><span class="sxs-lookup"><span data-stu-id="39876-201">Now that we have determined whether to use cloud multi-factor authentication or the MFA Server on-premises, we can get started setting up and using Azure Multi-Factor Authentication.</span></span> <span data-ttu-id="39876-202">**Vyberte ikonu, která představuje váš scénář.**</span><span class="sxs-lookup"><span data-stu-id="39876-202">**Select the icon that represents your scenario**</span></span>

<center>




<span data-ttu-id="39876-203">[![Cloud](./media/multi-factor-authentication-get-started/cloud2.png)](multi-factor-authentication-get-started-cloud.md)  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[![Server](./media/multi-factor-authentication-get-started/server2.png)](multi-factor-authentication-get-started-server.md) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </center></span><span class="sxs-lookup"><span data-stu-id="39876-203">[![Cloud](./media/multi-factor-authentication-get-started/cloud2.png)](multi-factor-authentication-get-started-cloud.md)  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[![Server](./media/multi-factor-authentication-get-started/server2.png)](multi-factor-authentication-get-started-server.md) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </center></span></span>