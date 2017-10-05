---
title: "Řešení potíží s dvoustupňové ověřování | Microsoft Docs"
description: "Tento dokument se poskytují uživatelům informace o co dělat, pokud se problém se službou Azure Multi-Factor Authentication."
services: multi-factor-authentication
keywords: "vícefaktorové ověřování klienta, problém s ověřováním, ID korelace"
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 8f3aef42-7f66-4656-a7cd-d25a971cb9eb
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: end-user
ms.openlocfilehash: e4b59efd12a64f58634b66590cf6999cc9e68eb6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="get-help-with-two-step-verification"></a><span data-ttu-id="ceeff-104">Získat pomoc s dvoustupňové ověření</span><span class="sxs-lookup"><span data-stu-id="ceeff-104">Get help with two-step verification</span></span>
<span data-ttu-id="ceeff-105">Tento článek obsahuje odpovědi na nejčastější otázky, které uživatelé požádat o dvoustupňovém ověřování.</span><span class="sxs-lookup"><span data-stu-id="ceeff-105">This article answers the most common questions that people ask about two-step verification.</span></span> 

## <a name="why-do-i-have-to-perform-two-step-verification-can-i-turn-it-off"></a><span data-ttu-id="ceeff-106">Proč musím provádět dvoustupňové ověřování?</span><span class="sxs-lookup"><span data-stu-id="ceeff-106">Why do I have to perform two-step verification?</span></span> <span data-ttu-id="ceeff-107">Můžete vypnout jeho nastavení?</span><span class="sxs-lookup"><span data-stu-id="ceeff-107">Can I turn it off?</span></span>

<span data-ttu-id="ceeff-108">Dvoustupňové ověření je funkce zabezpečení, která organizaci rozhodli používat k ochraně svých účtů.</span><span class="sxs-lookup"><span data-stu-id="ceeff-108">Two-step verification is a security feature that your organization chose to use to protect your accounts.</span></span> <span data-ttu-id="ceeff-109">Je bezpečnější než pouhým heslem, protože závisí na dvě formy ověřování: něco znáte a něco s vámi máte.</span><span class="sxs-lookup"><span data-stu-id="ceeff-109">It's more secure than just a password, because it relies on two forms of authentication: something you know, and something you have with you.</span></span> <span data-ttu-id="ceeff-110">Něco, co víte, je jím vaše heslo.</span><span class="sxs-lookup"><span data-stu-id="ceeff-110">The something you know is your password.</span></span> <span data-ttu-id="ceeff-111">Je něco, co máte s vámi telefonu nebo zařízení, které obvykle mají s vámi.</span><span class="sxs-lookup"><span data-stu-id="ceeff-111">The something you have with you is a phone or device that you commonly have with you.</span></span> <span data-ttu-id="ceeff-112">Pokud váš účet je chráněný pomocí dvoustupňové ověřování, to znamená, že kyberzločinci nemůžete se přihlásit jako jste pokud získají heslo nějakým způsobem protože nemají přístup k telefonu příliš.</span><span class="sxs-lookup"><span data-stu-id="ceeff-112">When your account is protected with two-step verification, that means that a malicious hacker can't sign in as you if they get your password somehow because they don't have access to your phone, too.</span></span> 

<span data-ttu-id="ceeff-113">Společnost Microsoft nabízí dvoustupňové ověření, ale vaše organizace rozhodne použít funkci.</span><span class="sxs-lookup"><span data-stu-id="ceeff-113">Microsoft offers two-step verification, but your organization chooses to use the feature.</span></span> <span data-ttu-id="ceeff-114">Nelze chodit Pokud vaše IT oddělení vyžaduje, aby se z vás, stejně jako nemůže vyjádření výslovného nesouhlasu s heslem abyste ochránili svůj účet.</span><span class="sxs-lookup"><span data-stu-id="ceeff-114">You can't opt out if your IT department requires it of you, just like you can't opt out of using a password to protect your account.</span></span> 

<span data-ttu-id="ceeff-115">Pokud máte dvoustupňové ověření zapnuté pro svůj osobní účet Microsoft a chcete-li změnit nastavení, přečtěte si [o dvoustupňovém ověřování](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) místo.</span><span class="sxs-lookup"><span data-stu-id="ceeff-115">If you have two-step verification turned on for your personal Microsoft account and want to change your settings, read [About two-step verification](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) instead.</span></span> 

## <a name="i-dont-have-my-phone-with-me-today"></a><span data-ttu-id="ceeff-116">Není k dispozici telefon se mi ještě dnes</span><span class="sxs-lookup"><span data-stu-id="ceeff-116">I don't have my phone with me today</span></span>

<span data-ttu-id="ceeff-117">Některé dnů, po který necháte telefonu v domácnostech, ale pořád je potřeba se přihlásit v práci.</span><span class="sxs-lookup"><span data-stu-id="ceeff-117">Some days you leave your phone at home, but still need to sign in at work.</span></span> <span data-ttu-id="ceeff-118">Je první věcí, kterou byste měli přihlášení pomocí různých ověření metoda.</span><span class="sxs-lookup"><span data-stu-id="ceeff-118">The first thing you should try is signing in with a different verification method.</span></span> <span data-ttu-id="ceeff-119">Při registraci pro dvoustupňové ověření, se můžete nastavit více než jeden telefonní číslo?</span><span class="sxs-lookup"><span data-stu-id="ceeff-119">When you registered for two-step verification, did you set up more than one phone number?</span></span> <span data-ttu-id="ceeff-120">Pokud chcete vyzkoušet, přihlášení pomocí jinou metodu, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="ceeff-120">To try signing in with a different method, follow these steps:</span></span>

1. <span data-ttu-id="ceeff-121">Přihlaste se jako obvykle.</span><span class="sxs-lookup"><span data-stu-id="ceeff-121">Sign in as you normally would.</span></span>
2. <span data-ttu-id="ceeff-122">Po otevření stránky dvoustupňové ověření, vyberte **použít jinou možností ověření**.</span><span class="sxs-lookup"><span data-stu-id="ceeff-122">When the two-step verification page opens, choose **Use a different verification option**.</span></span>

   ![Různé ověření](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)

3. <span data-ttu-id="ceeff-124">Vyberte možnost ověření, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="ceeff-124">Select the verification option you want to use.</span></span>
4. <span data-ttu-id="ceeff-125">Pokračujte dvoustupňové ověřování.</span><span class="sxs-lookup"><span data-stu-id="ceeff-125">Continue with two-step verification.</span></span>

<span data-ttu-id="ceeff-126">Pokud nevidíte **použít jinou možností ověření** odkaz, pak to znamená, že jste nenastavili alternativní metody při první registraci pro dvoustupňové ověření.</span><span class="sxs-lookup"><span data-stu-id="ceeff-126">If you don't see the **Use a different verification option** link, then that means you didn't set up alternative methods when you first registered for two-step verification.</span></span> <span data-ttu-id="ceeff-127">Kontaktujte oddělení IT získat pomoc s přihlášením k vašemu účtu.</span><span class="sxs-lookup"><span data-stu-id="ceeff-127">Contact your IT department to get help signing in to your account.</span></span> <span data-ttu-id="ceeff-128">Jakmile jste přihlášení, nezapomeňte [spravovat nastavení](multi-factor-authentication-end-user-manage-settings.md) přidat další ověřovací metody pro další použití.</span><span class="sxs-lookup"><span data-stu-id="ceeff-128">Once you're signed in, make sure to [manage your settings](multi-factor-authentication-end-user-manage-settings.md) to add additional verification methods for next time.</span></span> 

<span data-ttu-id="ceeff-129">Pokud se zobrazí **použít jinou možností ověření** odkaz, ale nebudete mít přístup k vaší alternativní metody buď, obraťte se na oddělení IT získat pomoc s přihlášením k vašemu účtu.</span><span class="sxs-lookup"><span data-stu-id="ceeff-129">If you do see the **Use a different verification option** link, but you don't have access to your alternative methods either, contact your IT department to get help signing in to your account.</span></span> 

## <a name="i-lost-my-phone-or-got-a-new-number"></a><span data-ttu-id="ceeff-130">I ztratíte telefon nebo získali nové číslo</span><span class="sxs-lookup"><span data-stu-id="ceeff-130">I lost my phone or got a new number</span></span>
<span data-ttu-id="ceeff-131">Existují dva způsoby dostat se zpátky do vašeho účtu.</span><span class="sxs-lookup"><span data-stu-id="ceeff-131">There are two ways to get back in to your account.</span></span> <span data-ttu-id="ceeff-132">První je se přihlásit pomocí vašeho alternativní číslo telefonu pro ověření, pokud jste nastavili jednu.</span><span class="sxs-lookup"><span data-stu-id="ceeff-132">The first is to sign in using your alternate authentication phone number, if you have set one up.</span></span> <span data-ttu-id="ceeff-133">Druhá je požádejte oddělení IT vymazat nastavení.</span><span class="sxs-lookup"><span data-stu-id="ceeff-133">The second is to ask your IT department to clear your settings.</span></span>

<span data-ttu-id="ceeff-134">Pokud váš telefon byl ztráty nebo odcizení, doporučujeme také zjistit vaše IT oddělení, takže mohou resetovat hesla aplikace a zrušte všechny zapamatovaných zařízení.</span><span class="sxs-lookup"><span data-stu-id="ceeff-134">If your phone was lost or stolen, we also recommend that you tell your IT department so they can reset your app passwords and clear any remembered devices.</span></span> 

### <a name="use-an-alternate-phone-number"></a><span data-ttu-id="ceeff-135">Použít alternativní telefonní číslo</span><span class="sxs-lookup"><span data-stu-id="ceeff-135">Use an alternate phone number</span></span>
<span data-ttu-id="ceeff-136">Pokud jste nastavili více možností ověření, včetně záložní telefonní číslo nebo ověřovací aplikace na jiném zařízení, můžete jeden z nich k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="ceeff-136">If you have set up multiple verification options, including a secondary phone number or an authenticator app on a different device, you can use one of them to sign in.</span></span>

<span data-ttu-id="ceeff-137">K přihlášení pomocí alternativní telefonní číslo, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="ceeff-137">To sign in using the alternate phone number, follow these steps:</span></span>

1. <span data-ttu-id="ceeff-138">Přihlaste se jako obvykle.</span><span class="sxs-lookup"><span data-stu-id="ceeff-138">Sign in as you normally would.</span></span>
2. <span data-ttu-id="ceeff-139">Po zobrazení výzvy provést ještě další ověření vašeho účtu, vyberte **použít jinou možností ověření**.</span><span class="sxs-lookup"><span data-stu-id="ceeff-139">When prompted to further verify your account, choose **Use a different verification option**.</span></span>
   
   ![Různé ověření](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)

3. <span data-ttu-id="ceeff-141">Vyberte telefonní číslo nebo zařízení, které máte přístup.</span><span class="sxs-lookup"><span data-stu-id="ceeff-141">Select the phone number or device that you have access to.</span></span>
4. <span data-ttu-id="ceeff-142">Poté, co si zpět ve vašem účtu [spravovat nastavení](multi-factor-authentication-end-user-manage-settings.md) Chcete-li změnit číslo telefonu pro ověření.</span><span class="sxs-lookup"><span data-stu-id="ceeff-142">After you're back in your account, [manage your settings](multi-factor-authentication-end-user-manage-settings.md) to change your authentication phone number.</span></span>

### <a name="clear-your-settings"></a><span data-ttu-id="ceeff-143">Clear – nastavení</span><span class="sxs-lookup"><span data-stu-id="ceeff-143">Clear your settings</span></span>
<span data-ttu-id="ceeff-144">Pokud jste nenakonfigurovali telefonní číslo sekundárního ověřování, budete muset požádejte o pomoc vaše oddělení IT.</span><span class="sxs-lookup"><span data-stu-id="ceeff-144">If you have not configured a secondary authentication phone number, you have to contact your IT department for help.</span></span> <span data-ttu-id="ceeff-145">Vymazat je mají nastavení, při příštím přihlášení, zobrazí se výzva k [zaregistrovat pro dvoustupňové ověření](multi-factor-authentication-end-user-first-time.md) znovu.</span><span class="sxs-lookup"><span data-stu-id="ceeff-145">Have them clear your settings so the next time you sign in, you will be prompted to [register for two-step verification](multi-factor-authentication-end-user-first-time.md) again.</span></span>

## <a name="i-am-not-receiving-a-text-or-call-on-my-phone"></a><span data-ttu-id="ceeff-146">I doručována text nebo volání na telefon</span><span class="sxs-lookup"><span data-stu-id="ceeff-146">I am not receiving a text or call on my phone</span></span>
<span data-ttu-id="ceeff-147">Tady je několik důvodů, proč můžete se pokusit přihlásit, ale není zobrazí text nebo telefonní hovor.</span><span class="sxs-lookup"><span data-stu-id="ceeff-147">There are several reasons why you may try to sign in, but not receive the text or phone call.</span></span> <span data-ttu-id="ceeff-148">Pokud jste úspěšně texty nebo obdrželi telefonní hovory na váš telefon v minulosti, pak příčinou je pravděpodobně problém s poskytovateli telefonní není váš účet.</span><span class="sxs-lookup"><span data-stu-id="ceeff-148">If you've successfully received texts or phone calls to your phone in the past, then this is probably an issue with the phone provider, not your account.</span></span> <span data-ttu-id="ceeff-149">Ujistěte se, že máte funkční buňky signál, a pokud chcete přijímat textové zprávy Ujistěte se, že budete moci přijímat textové zprávy.</span><span class="sxs-lookup"><span data-stu-id="ceeff-149">Make sure that you have good cell signal, and if you are trying to receive a text message make sure that you are able to recieve text messages.</span></span> <span data-ttu-id="ceeff-150">Požádejte friend volat jste nebo text jste jako testu.</span><span class="sxs-lookup"><span data-stu-id="ceeff-150">Ask a friend to call you or text you as a test.</span></span> 

<span data-ttu-id="ceeff-151">Pokud jste čekali několik minut, než text nebo volání, nejrychlejší způsob, jak dostat do vašeho účtu je zkuste jinou možnost.</span><span class="sxs-lookup"><span data-stu-id="ceeff-151">If you've waited several minutes for a text or call, the fastest way to get into your account is to try a different option.</span></span>

1. <span data-ttu-id="ceeff-152">Vyberte **použít jinou možností ověření** na stránku, která čeká na ověření.</span><span class="sxs-lookup"><span data-stu-id="ceeff-152">Select **Use a different verification option** on the page that's waiting for your verification.</span></span>
   
    ![Různé ověření](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)
2. <span data-ttu-id="ceeff-154">Vyberte telefonní číslo nebo doručení metodu, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="ceeff-154">Select the phone number or delivery method you want to use.</span></span>
   
    <span data-ttu-id="ceeff-155">Pokud jste dostali více ověřovací kódy, použijte nejnovější.</span><span class="sxs-lookup"><span data-stu-id="ceeff-155">If you received multiple verification codes, use the newest one.</span></span>

<span data-ttu-id="ceeff-156">Pokud nemáte nakonfigurován jinou metodu, obraťte se na oddělení IT a požádejte je o smazat nastavení.</span><span class="sxs-lookup"><span data-stu-id="ceeff-156">If you don’t have another method configured, contact your IT department and ask them to clear your settings.</span></span> <span data-ttu-id="ceeff-157">Při příštím přihlášení, zobrazí se výzva k [nastavení služby Multi-Factor authentication](multi-factor-authentication-end-user-first-time.md) znovu.</span><span class="sxs-lookup"><span data-stu-id="ceeff-157">The next time you sign in, you will be prompted to [set up multi-factor authentication](multi-factor-authentication-end-user-first-time.md) again.</span></span>

<span data-ttu-id="ceeff-158">Pokud máte často zpoždění z důvodu chybné buňky signál, doporučujeme použít [aplikaci Microsoft Authenticator](microsoft-authenticator-app-how-to.md) na vašem smartphonu.</span><span class="sxs-lookup"><span data-stu-id="ceeff-158">If you often have delays due to bad cell signal, we recommend you use the [Microsoft Authenticator app](microsoft-authenticator-app-how-to.md) on your smartphone.</span></span> <span data-ttu-id="ceeff-159">Aplikace může generovat náhodné zabezpečovací kódy, které používáte k přihlášení a tyto kódy nevyžadují žádné buňky signál nebo připojení k Internetu.</span><span class="sxs-lookup"><span data-stu-id="ceeff-159">The app can generate random security codes that you use to sign in, and these codes don't require any cell signal or internet connection.</span></span>

## <a name="app-passwords-are-not-working"></a><span data-ttu-id="ceeff-160">Hesla aplikací nejsou práce</span><span class="sxs-lookup"><span data-stu-id="ceeff-160">App passwords are not working</span></span>
<span data-ttu-id="ceeff-161">Zkontrolujte, zda jste správně zadali heslo aplikace.</span><span class="sxs-lookup"><span data-stu-id="ceeff-161">First, make sure that you have entered the app password correctly.</span></span> <span data-ttu-id="ceeff-162">Heslo generovaného aplikace nahrazuje normální heslo, ale jenom pro starší aplikací klasické pracovní plochy, které nepodporují dvoustupňové ověřování.</span><span class="sxs-lookup"><span data-stu-id="ceeff-162">The generated app password replaces your normal password, but only for older desktop applications that don't support two-step verification.</span></span> <span data-ttu-id="ceeff-163">Pokud stále nefunguje, zkuste přihlášení a [vytvořit nové heslo aplikace](multi-factor-authentication-end-user-app-passwords.md).</span><span class="sxs-lookup"><span data-stu-id="ceeff-163">If it still isn't working, try signing-in and [create a new app password](multi-factor-authentication-end-user-app-passwords.md).</span></span>  <span data-ttu-id="ceeff-164">Pokud stále nepomůže, obraťte se na oddělení IT a potom kliknul [odstranit vašich dosavadních hesel aplikací](../multi-factor-authentication-manage-users-and-devices.md) a vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="ceeff-164">If it still doesn't work, contact your IT department and have them [delete your existing app passwords](../multi-factor-authentication-manage-users-and-devices.md) and then you can create a new one.</span></span>

## <a name="i-didnt-find-an-answer-to-my-problem"></a><span data-ttu-id="ceeff-165">Odpověď na Můj problém I nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="ceeff-165">I didn't find an answer to my problem.</span></span>
<span data-ttu-id="ceeff-166">Pokud jste se pokusili postup řešení, ale jsou stále spuštěná na problémy, obraťte se na oddělení IT.</span><span class="sxs-lookup"><span data-stu-id="ceeff-166">If you've tried these troubleshooting steps but are still running into problems, contact your IT department.</span></span> <span data-ttu-id="ceeff-167">Jejich by mohli pomoct.</span><span class="sxs-lookup"><span data-stu-id="ceeff-167">They should be able to assist you.</span></span>

## <a name="related-topics"></a><span data-ttu-id="ceeff-168">Související témata</span><span class="sxs-lookup"><span data-stu-id="ceeff-168">Related topics</span></span>
* [<span data-ttu-id="ceeff-169">Spravovat nastavení pro dvoustupňové ověření</span><span class="sxs-lookup"><span data-stu-id="ceeff-169">Manage your settings for two-step verification</span></span>](multi-factor-authentication-end-user-manage-settings.md)  
* [<span data-ttu-id="ceeff-170">Nejčastější dotazy k aplikaci Microsoft Authenticator</span><span class="sxs-lookup"><span data-stu-id="ceeff-170">Microsoft Authenticator application FAQ</span></span>](microsoft-authenticator-app-faq.md)
