---
title: "přihlášení aaaAzure MFA s dvoustupňové ověřování | Microsoft Docs"
description: "Tato stránka poskytuje pokyny na kde toogo toosee hello různé metody přihlašování k dispozici s Azure MFA."
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
ms.openlocfilehash: fcd5eb5e8426eda537db9e099bf247bde29c195b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="hello-sign-in-experience-with-azure-multi-factor-authentication"></a><span data-ttu-id="debb0-104">Hello přihlašování uživatelů s Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="debb0-104">hello sign-in experience with Azure Multi-Factor Authentication</span></span>
> [!NOTE]
> <span data-ttu-id="debb0-105">účelem Hello tohoto článku je toowalk prostřednictvím typických možností přihlašování.</span><span class="sxs-lookup"><span data-stu-id="debb0-105">hello purpose of this article is toowalk through a typical sign-in experience.</span></span> <span data-ttu-id="debb0-106">Pomoc s přihlášením nebo tootroubleshoot problémy, najdete v tématu [potíže s Azure Multi-Factor Authentication s](multi-factor-authentication-end-user-troubleshoot.md).</span><span class="sxs-lookup"><span data-stu-id="debb0-106">For help with signing in, or tootroubleshoot problems, see [Having trouble with Azure Multi-Factor Authentication](multi-factor-authentication-end-user-troubleshoot.md).</span></span>

## <a name="what-will-your-sign-in-experience-be"></a><span data-ttu-id="debb0-107">Co bude vaše přihlášení?</span><span class="sxs-lookup"><span data-stu-id="debb0-107">What will your sign-in experience be?</span></span>
<span data-ttu-id="debb0-108">Vaše přihlášení se liší v závislosti na volbách toouse jako druhý faktor: telefonní hovor, ověřování aplikace nebo texty.</span><span class="sxs-lookup"><span data-stu-id="debb0-108">Your sign-in experience differs depending on what you choose toouse as your second factor: a phone call, an authentication app, or texts.</span></span> <span data-ttu-id="debb0-109">Vyberte možnost hello, která nejlépe vystihuje co dělají:</span><span class="sxs-lookup"><span data-stu-id="debb0-109">Choose hello option that best describes what you are doing:</span></span>

| <span data-ttu-id="debb0-110">Jak se můžete přihlásit?</span><span class="sxs-lookup"><span data-stu-id="debb0-110">How do you sign in?</span></span> | 
| --- |
| [<span data-ttu-id="debb0-111">Po telefonu volání toomy mobilní nebo telefonní číslo do kanceláře</span><span class="sxs-lookup"><span data-stu-id="debb0-111">With a phone call toomy mobile or office phone</span></span>](#signing-in-with-a-phone-call) |
| [<span data-ttu-id="debb0-112">S mobilním telefonem toomy textu</span><span class="sxs-lookup"><span data-stu-id="debb0-112">With a text toomy mobile phone</span></span>](#signing-in-with-a-text-message)
| [<span data-ttu-id="debb0-113">S oznámení z aplikace Microsoft Authenticator hello</span><span class="sxs-lookup"><span data-stu-id="debb0-113">With notifications from hello Microsoft Authenticator app</span></span>](#signing-in-with-the-microsoft-authenticator-app-using-notification) |
| [<span data-ttu-id="debb0-114">S ověřovací kódy z aplikace Microsoft Authenticator hello</span><span class="sxs-lookup"><span data-stu-id="debb0-114">With verification codes from hello Microsoft Authenticator app</span></span>](#signing-in-with-the-microsoft-authenticator-app-using-verification-code) |
| [<span data-ttu-id="debb0-115">S alternativní metodu protože Moje preferovanou metodu nelze použít nyní</span><span class="sxs-lookup"><span data-stu-id="debb0-115">With an alternate method, because I can't use my preferred method right now</span></span>](#signing-in-with-an-alternate-method) |

## <a name="signing-in-with-a-phone-call"></a><span data-ttu-id="debb0-116">Přihlášení pomocí telefonního hovoru</span><span class="sxs-lookup"><span data-stu-id="debb0-116">Signing in with a phone call</span></span>
<span data-ttu-id="debb0-117">Následující informace Hello popisuje hello dvoustupňové ověření zkušenosti s mobilním tooyour volání nebo telefonní číslo do kanceláře.</span><span class="sxs-lookup"><span data-stu-id="debb0-117">hello following information describes hello two-step verification experience with a call tooyour mobile or office phone.</span></span>

1. <span data-ttu-id="debb0-118">Přihlaste se tooan aplikace nebo služby, například Office 365 pomocí uživatelského jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="debb0-118">Sign in tooan application or service such as Office 365 using your username and password.</span></span>  
2. <span data-ttu-id="debb0-119">Microsoft zavolá.</span><span class="sxs-lookup"><span data-stu-id="debb0-119">Microsoft calls you.</span></span>  
3. <span data-ttu-id="debb0-120">Odpovězte hello phone a stiskněte klávesu # hello.</span><span class="sxs-lookup"><span data-stu-id="debb0-120">Answer hello phone and hit hello # key.</span></span>  

## <a name="signing-in-with-a-text-message"></a><span data-ttu-id="debb0-121">Přihlášení pomocí textové zprávy</span><span class="sxs-lookup"><span data-stu-id="debb0-121">Signing in with a text message</span></span>
<span data-ttu-id="debb0-122">Následující informace Hello popisuje hello dvoustupňové ověření zkušenosti s text zprávy tooyour mobilního telefonu:</span><span class="sxs-lookup"><span data-stu-id="debb0-122">hello following information describes hello two-step verification experience with a text message tooyour mobile phone:</span></span>

1. <span data-ttu-id="debb0-123">Přihlaste se tooan aplikace nebo služby, například Office 365 pomocí uživatelského jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="debb0-123">Sign in tooan application or service such as Office 365 using your username and password.</span></span> 
2. <span data-ttu-id="debb0-124">Microsoft vám pošle textovou zprávu obsahující číslo kód.</span><span class="sxs-lookup"><span data-stu-id="debb0-124">Microsoft sends you a text message that contains a number code.</span></span> 
3. <span data-ttu-id="debb0-125">Zadejte kód hello v poli hello na přihlašovací stránku hello.</span><span class="sxs-lookup"><span data-stu-id="debb0-125">Enter hello code in hello box provided on hello sign-in page.</span></span> 

## <a name="signing-in-with-hello-microsoft-authenticator-app"></a><span data-ttu-id="debb0-126">Přihlášení pomocí aplikace Microsoft Authenticator hello</span><span class="sxs-lookup"><span data-stu-id="debb0-126">Signing in with hello Microsoft Authenticator app</span></span> 
<span data-ttu-id="debb0-127">Hello následující informace popisují činnost hello pomocí aplikace Microsoft Authenticator hello pro dvoustupňové ověření.</span><span class="sxs-lookup"><span data-stu-id="debb0-127">hello following information describes hello experience of using hello Microsoft Authenticator app for two-step verifications.</span></span> <span data-ttu-id="debb0-128">Existují dva různé způsoby toouse hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="debb0-128">There are two different ways toouse hello app.</span></span> <span data-ttu-id="debb0-129">Může přijímat nabízená oznámení na vašem zařízení, nebo můžete otevřít tooget aplikace hello ověřovací kód.</span><span class="sxs-lookup"><span data-stu-id="debb0-129">You can receive push notifications on your device, or you can open hello app tooget a verification code.</span></span>

### <a name="toosign-in-with-a-notification-from-hello-microsoft-authenticator-app"></a><span data-ttu-id="debb0-130">toosign pomocí oznámení z aplikace Microsoft Authenticator hello</span><span class="sxs-lookup"><span data-stu-id="debb0-130">toosign in with a notification from hello Microsoft Authenticator app</span></span>
1. <span data-ttu-id="debb0-131">Přihlaste se tooan aplikace nebo služby, například Office 365 pomocí uživatelského jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="debb0-131">Sign in tooan application or service such as Office 365 using your username and password.</span></span>
2. <span data-ttu-id="debb0-132">Microsoft odesílá aplikaci Microsoft Authenticator toohello oznámení na svém zařízení.</span><span class="sxs-lookup"><span data-stu-id="debb0-132">Microsoft sends a notification toohello Microsoft Authenticator app on your device.</span></span>

  ![Microsoft odesílá oznámení](./media/multi-factor-authentication-end-user-signin/notify.png)

3. <span data-ttu-id="debb0-134">Otevřete hello oznámení na vaše telefonní a vyberte hello **ověřte** klíč.</span><span class="sxs-lookup"><span data-stu-id="debb0-134">Open hello notification on your phone and select hello **Verify** key.</span></span> <span data-ttu-id="debb0-135">Pokud vaše společnost vyžaduje PIN, zadejte ho sem.</span><span class="sxs-lookup"><span data-stu-id="debb0-135">If your company requires a PIN, enter it here.</span></span>
4. <span data-ttu-id="debb0-136">Můžete by měl nyní přihlášeni.</span><span class="sxs-lookup"><span data-stu-id="debb0-136">You should now be signed in.</span></span>

### <a name="toosign-in-using-a-verification-code-with-hello-microsoft-authenticator-app"></a><span data-ttu-id="debb0-137">toosign v aplikaci Microsoft Authenticator hello pomocí ověřovací kód</span><span class="sxs-lookup"><span data-stu-id="debb0-137">toosign in using a verification code with hello Microsoft Authenticator app</span></span>

<span data-ttu-id="debb0-138">Pokud používáte hello Microsoft Authenticator aplikace tooget ověřovací kódy, pak při otevření aplikace hello zobrazí číslo pod názvem svého účtu.</span><span class="sxs-lookup"><span data-stu-id="debb0-138">If you use hello Microsoft Authenticator app tooget verification codes, then when you open hello app you see a number under your account name.</span></span> <span data-ttu-id="debb0-139">Toto číslo změní každých 30 sekund, tak, že nepoužíváte hello stejné číslo dvakrát.</span><span class="sxs-lookup"><span data-stu-id="debb0-139">This number changes every 30 seconds so that you don't use hello same number twice.</span></span> <span data-ttu-id="debb0-140">Po zobrazení dotazu pro ověřovací kód, otevřete aplikaci hello a použít je omezený na aktuálně zobrazený.</span><span class="sxs-lookup"><span data-stu-id="debb0-140">When you're asked for a verification code, open hello app and use whatever number is currently displayed.</span></span> 

1. <span data-ttu-id="debb0-141">Přihlaste se tooan aplikace nebo služby, například Office 365 pomocí uživatelského jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="debb0-141">Sign in tooan application or service such as Office 365 using your username and password.</span></span>
2. <span data-ttu-id="debb0-142">Microsoft vás vyzve k zadání ověřovací kód.</span><span class="sxs-lookup"><span data-stu-id="debb0-142">Microsoft prompts you for a verification code.</span></span>

  ![Zadejte ověřovací kód](./media/multi-factor-authentication-end-user-signin/verify3.png)

3. <span data-ttu-id="debb0-144">Otevřete aplikaci Microsoft Authenticator hello na váš telefon a zadejte kód hello hello pole, kde se přihlašujete.</span><span class="sxs-lookup"><span data-stu-id="debb0-144">Open hello Microsoft Authenticator app on your phone and enter hello code in hello box where you are signing in.</span></span>

## <a name="signing-in-with-an-alternate-method"></a><span data-ttu-id="debb0-145">Přihlášení pomocí alternativní metoda</span><span class="sxs-lookup"><span data-stu-id="debb0-145">Signing in with an alternate method</span></span>
<span data-ttu-id="debb0-146">Někdy nemáte hello telefonu nebo zařízení, které jste nastavili jako způsob upřednostňované ověření.</span><span class="sxs-lookup"><span data-stu-id="debb0-146">Sometimes you don't have hello phone or device that you set up as your preferred verification method.</span></span> <span data-ttu-id="debb0-147">Tato situace je proto doporučujeme, abyste nastavili metody zálohování pro váš účet.</span><span class="sxs-lookup"><span data-stu-id="debb0-147">This situation is why we recommend that you set up backup methods for your account.</span></span> <span data-ttu-id="debb0-148">Hello následující části se dozvíte, jak toosign pomocí alternativní metodu, když primární metodu nemusí být k dispozici.</span><span class="sxs-lookup"><span data-stu-id="debb0-148">hello following section shows you how toosign in with an alternate method when your primary method may not be available.</span></span>

1. <span data-ttu-id="debb0-149">Přihlaste se tooan aplikace nebo služby, například Office 365 pomocí uživatelského jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="debb0-149">Sign in tooan application or service such as Office 365 using your username and password.</span></span>
2. <span data-ttu-id="debb0-150">Vyberte **použít jinou možností ověření**.</span><span class="sxs-lookup"><span data-stu-id="debb0-150">Select **Use a different verification option**.</span></span> <span data-ttu-id="debb0-151">Zobrazí možnosti různých ověření založené na tom, kolik má instalační program.</span><span class="sxs-lookup"><span data-stu-id="debb0-151">You see different verification options based on how many you have setup.</span></span>
3. <span data-ttu-id="debb0-152">Zvolte alternativní metodu a přihlaste se.</span><span class="sxs-lookup"><span data-stu-id="debb0-152">Choose an alternate method and sign in.</span></span>

  ![Použití alternativní metody](./media/multi-factor-authentication-end-user-signin/alt.png)

## <a name="next-steps"></a><span data-ttu-id="debb0-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="debb0-154">Next steps</span></span>

<span data-ttu-id="debb0-155">Pokud máte potíže s přihlášením k dvoustupňové ověření, získat další informace v [potíže s Azure Multi-Factor Authentication s](multi-factor-authentication-end-user-troubleshoot.md).</span><span class="sxs-lookup"><span data-stu-id="debb0-155">If you have problems signing in with two-step verification, get more information at [Having trouble with Azure Multi-Factor Authentication](multi-factor-authentication-end-user-troubleshoot.md).</span></span>

<span data-ttu-id="debb0-156">Zjistěte, jak příliš[spravovat nastavení dvoustupňového ověřování](multi-factor-authentication-end-user-manage-settings.md).</span><span class="sxs-lookup"><span data-stu-id="debb0-156">Learn how too[Manage your two-step verification settings](multi-factor-authentication-end-user-manage-settings.md).</span></span>

<span data-ttu-id="debb0-157">Zjistěte, jak příliš[začít pracovat s aplikací Microsoft Authenticator hello](microsoft-authenticator-app-how-to.md) tak, aby můžete toosign oznámení, namísto texty a telefonních hovorů.</span><span class="sxs-lookup"><span data-stu-id="debb0-157">Find out how too[Get started with hello Microsoft Authenticator app](microsoft-authenticator-app-how-to.md) so that you can use notifications toosign in, instead of texts and phone calls.</span></span> 
