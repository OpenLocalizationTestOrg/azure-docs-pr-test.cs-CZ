---
title: "Azure MFA přihlášení s dvoustupňové ověřování | Microsoft Docs"
description: "Tato stránka poskytuje pokyny na koho se obracet, pokud chcete zobrazit různé přihlášení dostupné metody s Azure MFA."
keywords: "ověřování uživatelů, přihlášení, přihlaste se pomocí mobilního telefonu, přihlaste se pomocí telefonní číslo do kanceláře"
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: b310b762-471b-4b26-887a-a321c9e81d46
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2017
ms.author: kgremban
ms.reviewer: librown
ms.custom: end-user
ms.openlocfilehash: d12115be61ca00dfb86dd822ccae9f9096fa796a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="the-sign-in-experience-with-azure-multi-factor-authentication"></a><span data-ttu-id="08032-104">Přihlašování uživatelů s Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="08032-104">The sign-in experience with Azure Multi-Factor Authentication</span></span>
> [!NOTE]
> <span data-ttu-id="08032-105">Účelem tohoto článku je provede typických možností přihlašování.</span><span class="sxs-lookup"><span data-stu-id="08032-105">The purpose of this article is to walk through a typical sign-in experience.</span></span> <span data-ttu-id="08032-106">Pomoc s přihlášením nebo k řešení problémů najdete v tématu [potíže s Azure Multi-Factor Authentication s](multi-factor-authentication-end-user-troubleshoot.md).</span><span class="sxs-lookup"><span data-stu-id="08032-106">For help with signing in, or to troubleshoot problems, see [Having trouble with Azure Multi-Factor Authentication](multi-factor-authentication-end-user-troubleshoot.md).</span></span>

## <a name="what-will-your-sign-in-experience-be"></a><span data-ttu-id="08032-107">Co bude vaše přihlášení?</span><span class="sxs-lookup"><span data-stu-id="08032-107">What will your sign-in experience be?</span></span>
<span data-ttu-id="08032-108">Vaše přihlášení se liší v závislosti na tom, co chcete použít jako druhý faktor: telefonní hovor, ověřování aplikace nebo texty.</span><span class="sxs-lookup"><span data-stu-id="08032-108">Your sign-in experience differs depending on what you choose to use as your second factor: a phone call, an authentication app, or texts.</span></span> <span data-ttu-id="08032-109">Zvolte možnost, která nejlépe popisuje, co dělají:</span><span class="sxs-lookup"><span data-stu-id="08032-109">Choose the option that best describes what you are doing:</span></span>

| <span data-ttu-id="08032-110">Jak se můžete přihlásit?</span><span class="sxs-lookup"><span data-stu-id="08032-110">How do you sign in?</span></span> | 
| --- |
| [<span data-ttu-id="08032-111">Pomocí telefonního hovoru na telefon mobilní telefon nebo office</span><span class="sxs-lookup"><span data-stu-id="08032-111">With a phone call to my mobile or office phone</span></span>](#signing-in-with-a-phone-call) |
| [<span data-ttu-id="08032-112">S textem na můj mobilní telefon</span><span class="sxs-lookup"><span data-stu-id="08032-112">With a text to my mobile phone</span></span>](#signing-in-with-a-text-message)
| [<span data-ttu-id="08032-113">S oznámení z aplikace Microsoft Authenticator</span><span class="sxs-lookup"><span data-stu-id="08032-113">With notifications from the Microsoft Authenticator app</span></span>](#signing-in-with-the-microsoft-authenticator-app-using-notification) |
| [<span data-ttu-id="08032-114">S ověřovací kódy z aplikace Microsoft Authenticator</span><span class="sxs-lookup"><span data-stu-id="08032-114">With verification codes from the Microsoft Authenticator app</span></span>](#signing-in-with-the-microsoft-authenticator-app-using-verification-code) |
| [<span data-ttu-id="08032-115">S alternativní metodu protože Moje preferovanou metodu nelze použít nyní</span><span class="sxs-lookup"><span data-stu-id="08032-115">With an alternate method, because I can't use my preferred method right now</span></span>](#signing-in-with-an-alternate-method) |

## <a name="signing-in-with-a-phone-call"></a><span data-ttu-id="08032-116">Přihlášení pomocí telefonního hovoru</span><span class="sxs-lookup"><span data-stu-id="08032-116">Signing in with a phone call</span></span>
<span data-ttu-id="08032-117">Následující informace popisuje činnost dvoustupňové ověření pomocí volání do telefonu mobilní telefon nebo office.</span><span class="sxs-lookup"><span data-stu-id="08032-117">The following information describes the two-step verification experience with a call to your mobile or office phone.</span></span>

1. <span data-ttu-id="08032-118">Přihlaste se k aplikaci nebo službu, jako je například Office 365 pomocí uživatelského jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="08032-118">Sign in to an application or service such as Office 365 using your username and password.</span></span>  
2. <span data-ttu-id="08032-119">Microsoft zavolá.</span><span class="sxs-lookup"><span data-stu-id="08032-119">Microsoft calls you.</span></span>  
3. <span data-ttu-id="08032-120">Přijměte hovor a stiskem tlačítka #.</span><span class="sxs-lookup"><span data-stu-id="08032-120">Answer the phone and hit the # key.</span></span>  

## <a name="signing-in-with-a-text-message"></a><span data-ttu-id="08032-121">Přihlášení pomocí textové zprávy</span><span class="sxs-lookup"><span data-stu-id="08032-121">Signing in with a text message</span></span>
<span data-ttu-id="08032-122">Následující informace popisuje činnost dvoustupňové ověření pomocí textové zprávy na váš mobilní telefon:</span><span class="sxs-lookup"><span data-stu-id="08032-122">The following information describes the two-step verification experience with a text message to your mobile phone:</span></span>

1. <span data-ttu-id="08032-123">Přihlaste se k aplikaci nebo službu, jako je například Office 365 pomocí uživatelského jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="08032-123">Sign in to an application or service such as Office 365 using your username and password.</span></span> 
2. <span data-ttu-id="08032-124">Microsoft vám pošle textovou zprávu obsahující číslo kód.</span><span class="sxs-lookup"><span data-stu-id="08032-124">Microsoft sends you a text message that contains a number code.</span></span> 
3. <span data-ttu-id="08032-125">V poli na stránce přihlášení zadejte kód.</span><span class="sxs-lookup"><span data-stu-id="08032-125">Enter the code in the box provided on the sign-in page.</span></span> 

## <a name="signing-in-with-the-microsoft-authenticator-app"></a><span data-ttu-id="08032-126">Přihlášení pomocí aplikace Microsoft Authenticator</span><span class="sxs-lookup"><span data-stu-id="08032-126">Signing in with the Microsoft Authenticator app</span></span> 
<span data-ttu-id="08032-127">Následující informace popisují možností použití aplikace Microsoft Authenticator pro dvoustupňové ověření.</span><span class="sxs-lookup"><span data-stu-id="08032-127">The following information describes the experience of using the Microsoft Authenticator app for two-step verifications.</span></span> <span data-ttu-id="08032-128">Existují dva různé způsoby použití aplikace.</span><span class="sxs-lookup"><span data-stu-id="08032-128">There are two different ways to use the app.</span></span> <span data-ttu-id="08032-129">Může přijímat nabízená oznámení na vašem zařízení, nebo můžete otevřít aplikaci získat ověřovací kód.</span><span class="sxs-lookup"><span data-stu-id="08032-129">You can receive push notifications on your device, or you can open the app to get a verification code.</span></span>

### <a name="to-sign-in-with-a-notification-from-the-microsoft-authenticator-app"></a><span data-ttu-id="08032-130">Přihlásit se přes oznámení z aplikace Microsoft Authenticator</span><span class="sxs-lookup"><span data-stu-id="08032-130">To sign in with a notification from the Microsoft Authenticator app</span></span>
1. <span data-ttu-id="08032-131">Přihlaste se k aplikaci nebo službu, jako je například Office 365 pomocí uživatelského jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="08032-131">Sign in to an application or service such as Office 365 using your username and password.</span></span>
2. <span data-ttu-id="08032-132">Microsoft odešle oznámení do aplikace Microsoft Authenticator na vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="08032-132">Microsoft sends a notification to the Microsoft Authenticator app on your device.</span></span>

  ![Microsoft odesílá oznámení](./media/multi-factor-authentication-end-user-signin/notify.png)

3. <span data-ttu-id="08032-134">Otevřete oznámení na vaše telefonní a vyberte **ověřte** klíč.</span><span class="sxs-lookup"><span data-stu-id="08032-134">Open the notification on your phone and select the **Verify** key.</span></span> <span data-ttu-id="08032-135">Pokud vaše společnost vyžaduje PIN, zadejte ho sem.</span><span class="sxs-lookup"><span data-stu-id="08032-135">If your company requires a PIN, enter it here.</span></span>
4. <span data-ttu-id="08032-136">Můžete by měl nyní přihlášeni.</span><span class="sxs-lookup"><span data-stu-id="08032-136">You should now be signed in.</span></span>

### <a name="to-sign-in-using-a-verification-code-with-the-microsoft-authenticator-app"></a><span data-ttu-id="08032-137">K přihlášení pomocí ověřovací kód v aplikaci Microsoft Authenticator</span><span class="sxs-lookup"><span data-stu-id="08032-137">To sign in using a verification code with the Microsoft Authenticator app</span></span>

<span data-ttu-id="08032-138">Pokud používáte aplikaci Microsoft Authenticator získat ověřovací kódy, pak při otevření aplikace uvidíte číslo pod názvem svého účtu.</span><span class="sxs-lookup"><span data-stu-id="08032-138">If you use the Microsoft Authenticator app to get verification codes, then when you open the app you see a number under your account name.</span></span> <span data-ttu-id="08032-139">Toto číslo změní každých 30 sekund, tak, aby nepoužívejte stejné číslo dvakrát.</span><span class="sxs-lookup"><span data-stu-id="08032-139">This number changes every 30 seconds so that you don't use the same number twice.</span></span> <span data-ttu-id="08032-140">Pokud jste žádali ověřovací kód, otevřete aplikaci a používat je omezený na aktuálně zobrazený.</span><span class="sxs-lookup"><span data-stu-id="08032-140">When you're asked for a verification code, open the app and use whatever number is currently displayed.</span></span> 

1. <span data-ttu-id="08032-141">Přihlaste se k aplikaci nebo službu, jako je například Office 365 pomocí uživatelského jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="08032-141">Sign in to an application or service such as Office 365 using your username and password.</span></span>
2. <span data-ttu-id="08032-142">Microsoft vás vyzve k zadání ověřovací kód.</span><span class="sxs-lookup"><span data-stu-id="08032-142">Microsoft prompts you for a verification code.</span></span>

  ![Zadejte ověřovací kód](./media/multi-factor-authentication-end-user-signin/verify3.png)

3. <span data-ttu-id="08032-144">Otevřete aplikaci Microsoft Authenticator na váš telefon a zadejte kód v poli, kde se přihlašujete.</span><span class="sxs-lookup"><span data-stu-id="08032-144">Open the Microsoft Authenticator app on your phone and enter the code in the box where you are signing in.</span></span>

## <a name="signing-in-with-an-alternate-method"></a><span data-ttu-id="08032-145">Přihlášení pomocí alternativní metoda</span><span class="sxs-lookup"><span data-stu-id="08032-145">Signing in with an alternate method</span></span>
<span data-ttu-id="08032-146">Někdy nemáte telefonu nebo zařízení, které jste nastavili jako způsob upřednostňované ověření.</span><span class="sxs-lookup"><span data-stu-id="08032-146">Sometimes you don't have the phone or device that you set up as your preferred verification method.</span></span> <span data-ttu-id="08032-147">Tato situace je proto doporučujeme, abyste nastavili metody zálohování pro váš účet.</span><span class="sxs-lookup"><span data-stu-id="08032-147">This situation is why we recommend that you set up backup methods for your account.</span></span> <span data-ttu-id="08032-148">V následující části se dozvíte, jak se přihlásit pomocí alternativní metodu, když primární metodu nemusí být k dispozici.</span><span class="sxs-lookup"><span data-stu-id="08032-148">The following section shows you how to sign in with an alternate method when your primary method may not be available.</span></span>

1. <span data-ttu-id="08032-149">Přihlaste se k aplikaci nebo službu, jako je například Office 365 pomocí uživatelského jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="08032-149">Sign in to an application or service such as Office 365 using your username and password.</span></span>
2. <span data-ttu-id="08032-150">Vyberte **použít jinou možností ověření**.</span><span class="sxs-lookup"><span data-stu-id="08032-150">Select **Use a different verification option**.</span></span> <span data-ttu-id="08032-151">Zobrazí možnosti různých ověření založené na tom, kolik má instalační program.</span><span class="sxs-lookup"><span data-stu-id="08032-151">You see different verification options based on how many you have setup.</span></span>
3. <span data-ttu-id="08032-152">Zvolte alternativní metodu a přihlaste se.</span><span class="sxs-lookup"><span data-stu-id="08032-152">Choose an alternate method and sign in.</span></span>

  ![Použití alternativní metody](./media/multi-factor-authentication-end-user-signin/alt.png)

## <a name="next-steps"></a><span data-ttu-id="08032-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="08032-154">Next steps</span></span>

<span data-ttu-id="08032-155">Pokud máte potíže s přihlášením k dvoustupňové ověření, získat další informace v [potíže s Azure Multi-Factor Authentication s](multi-factor-authentication-end-user-troubleshoot.md).</span><span class="sxs-lookup"><span data-stu-id="08032-155">If you have problems signing in with two-step verification, get more information at [Having trouble with Azure Multi-Factor Authentication](multi-factor-authentication-end-user-troubleshoot.md).</span></span>

<span data-ttu-id="08032-156">Zjistěte, jak [spravovat nastavení dvoustupňového ověřování](multi-factor-authentication-end-user-manage-settings.md).</span><span class="sxs-lookup"><span data-stu-id="08032-156">Learn how to [Manage your two-step verification settings](multi-factor-authentication-end-user-manage-settings.md).</span></span>

<span data-ttu-id="08032-157">Zjistěte, jak [začít pracovat s aplikací Microsoft Authenticator](microsoft-authenticator-app-how-to.md) tak, aby oznámení můžete použít k přihlášení, namísto texty a telefonních hovorů.</span><span class="sxs-lookup"><span data-stu-id="08032-157">Find out how to [Get started with the Microsoft Authenticator app](microsoft-authenticator-app-how-to.md) so that you can use notifications to sign in, instead of texts and phone calls.</span></span> 