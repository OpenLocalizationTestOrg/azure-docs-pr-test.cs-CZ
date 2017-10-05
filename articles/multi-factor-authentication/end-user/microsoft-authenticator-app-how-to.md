---
title: "Aplikace Microsoft Authenticator pro mobilních telefonů | Microsoft Docs"
description: "Naučte se upgradovat na nejnovější verzi Azure Authenticator."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 3065a1ee-f253-41f0-a68d-2bd84af5ffba
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: kgremban
ms.reviewer: librown
ms.custom: H1Hack27Feb2017, end-user
ms.openlocfilehash: 6bcb6d9f7a1e9b241fa70690016b03d6eb5887ab
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-the-microsoft-authenticator-app"></a><span data-ttu-id="89a37-103">Začínáme s aplikací Microsoft Authenticator</span><span class="sxs-lookup"><span data-stu-id="89a37-103">Get started with the Microsoft Authenticator app</span></span>
<span data-ttu-id="89a37-104">Aplikace Microsoft Authenticator poskytuje další úroveň zabezpečení v pracovní nebo školní účet (například bsimon@contoso.com) nebo účtu Microsoft (například bsimon@outlook.com).</span><span class="sxs-lookup"><span data-stu-id="89a37-104">The Microsoft Authenticator app provides an additional level of security in your work or school account (for example, bsimon@contoso.com) or your Microsoft account (for example, bsimon@outlook.com).</span></span>

<span data-ttu-id="89a37-105">Aplikace funguje v jednom ze dvou způsobů:</span><span class="sxs-lookup"><span data-stu-id="89a37-105">The app works in one of two ways:</span></span>

* <span data-ttu-id="89a37-106">**Oznámení**.</span><span class="sxs-lookup"><span data-stu-id="89a37-106">**Notification**.</span></span> <span data-ttu-id="89a37-107">Aplikace může pomoci zabránit neoprávněnému přístupu k účtům a zastavit podvodné transakce vynucením oznámení tablet nebo smartphone.</span><span class="sxs-lookup"><span data-stu-id="89a37-107">The app can help prevent unauthorized access to accounts and stop fraudulent transactions by pushing a notification to your smartphone or tablet.</span></span> <span data-ttu-id="89a37-108">Jednoduše oznámení zobrazte a pokud je oprávněné, vyberte **ověřte**.</span><span class="sxs-lookup"><span data-stu-id="89a37-108">Simply view the notification, and if it's legitimate, select **Verify**.</span></span> <span data-ttu-id="89a37-109">Jinak, můžete vybrat **Odepřít**.</span><span class="sxs-lookup"><span data-stu-id="89a37-109">Otherwise, you can select **Deny**.</span></span> 
* <span data-ttu-id="89a37-110">**Ověřovací kód**.</span><span class="sxs-lookup"><span data-stu-id="89a37-110">**Verification code**.</span></span> <span data-ttu-id="89a37-111">Aplikace můžete používat jako softwarový token pro generování k ověření kódu OAuth.</span><span class="sxs-lookup"><span data-stu-id="89a37-111">The app can be used as a software token to generate an OAuth verification code.</span></span> <span data-ttu-id="89a37-112">Po zadání uživatelského jména a hesla, je třeba zadat kód poskytovaném aplikací na obrazovce přihlášení.</span><span class="sxs-lookup"><span data-stu-id="89a37-112">After you enter your username and password, you enter the code provided by the app into the sign-in screen.</span></span> <span data-ttu-id="89a37-113">Ověřovací kód poskytuje druhou podobu ověřování.</span><span class="sxs-lookup"><span data-stu-id="89a37-113">The verification code provides a second form of authentication.</span></span>

<span data-ttu-id="89a37-114">Aplikace Microsoft Authenticator nahrazuje aplikaci Azure Authenticator.</span><span class="sxs-lookup"><span data-stu-id="89a37-114">The Microsoft Authenticator app replaces the Azure Authenticator app.</span></span> <span data-ttu-id="89a37-115">Aplikace Azure Authenticator pořád funguje, ale pokud se rozhodnete přesunout do nové aplikace Microsoft Authenticator, tento článek vám může pomoci.</span><span class="sxs-lookup"><span data-stu-id="89a37-115">The Azure Authenticator app still works, but if you decide to move to the new Microsoft Authenticator app, this article can assist you.</span></span>  

## <a name="opt-in-for-two-step-verification"></a><span data-ttu-id="89a37-116">Vyjádřit výslovný souhlas pro dvoustupňové ověření</span><span class="sxs-lookup"><span data-stu-id="89a37-116">Opt in for two-step verification</span></span>

<span data-ttu-id="89a37-117">Aplikace Microsoft Authenticator nefunguje samostatně.</span><span class="sxs-lookup"><span data-stu-id="89a37-117">The Microsoft Authenticator app doesn't work by itself.</span></span> <span data-ttu-id="89a37-118">Konfigurovat účty a vyzvat vás k druhé metody ověřování, jakmile se přihlásíte pomocí uživatelského jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="89a37-118">Configure each of your accounts to prompt you for a second verification method after you sign in with your username and password.</span></span> 

<span data-ttu-id="89a37-119">Pro pracovní nebo školní účet neobdržíte obvykle vybrat tuto funkci pro sami.</span><span class="sxs-lookup"><span data-stu-id="89a37-119">For a work or school account, you don't usually get to choose this feature for yourself.</span></span> <span data-ttu-id="89a37-120">Místo toho správce zabezpečení vyjádřit výslovný souhlas vaším jménem a potom upozorní vás, abyste zaregistrovat metody ověření pro váš účet.</span><span class="sxs-lookup"><span data-stu-id="89a37-120">Instead, a security administrator opts in on your behalf and then notifies you to register verification methods for your account.</span></span> <span data-ttu-id="89a37-121">Pokud pro vás platí tento scénář, další informace v [co Azure Multi-Factor Authentication znamená pro mě nejlepší](multi-factor-authentication-end-user.md).</span><span class="sxs-lookup"><span data-stu-id="89a37-121">If this scenario applies to you, learn more in [What does Azure Multi-Factor Authentication mean for me](multi-factor-authentication-end-user.md).</span></span>

<span data-ttu-id="89a37-122">Pro osobní účet budete muset nastavit dvoustupňové ověřování pro sebe.</span><span class="sxs-lookup"><span data-stu-id="89a37-122">For a personal account, you need to set up two-step verification for yourself.</span></span> <span data-ttu-id="89a37-123">Pokud máte účet Microsoft, tyto kroky jsou k dispozici v [o dvoustupňovém ověřování](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).</span><span class="sxs-lookup"><span data-stu-id="89a37-123">If you have a Microsoft account, those steps are available in [About two-step verification](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).</span></span> 

<span data-ttu-id="89a37-124">Můžete taky Microsoft Authenticator s účty jiných společností než Microsoft.</span><span class="sxs-lookup"><span data-stu-id="89a37-124">You can also use the Microsoft Authenticator with non-Microsoft accounts.</span></span> <span data-ttu-id="89a37-125">Tato funkce se volání něco jiného než dvoustupňové ověření, ale musí být schopen najít v části zabezpečení nebo nastavení přihlášení.</span><span class="sxs-lookup"><span data-stu-id="89a37-125">They may call the feature something other than two-step verification, but you should be able to find it under security or sign-in settings.</span></span> 

## <a name="install-the-app"></a><span data-ttu-id="89a37-126">Nainstalujte aplikaci</span><span class="sxs-lookup"><span data-stu-id="89a37-126">Install the app</span></span>
<span data-ttu-id="89a37-127">Je k dispozici pro aplikaci Microsoft Authenticator [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), a [iOS](http://go.microsoft.com/fwlink/?Linkid=825073).</span><span class="sxs-lookup"><span data-stu-id="89a37-127">The Microsoft Authenticator app is available for [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), and [iOS](http://go.microsoft.com/fwlink/?Linkid=825073).</span></span>

## <a name="add-accounts-to-the-app"></a><span data-ttu-id="89a37-128">Přidejte účty do aplikace</span><span class="sxs-lookup"><span data-stu-id="89a37-128">Add accounts to the app</span></span>
<span data-ttu-id="89a37-129">U každého účtu, který chcete přidat do aplikace Microsoft Authenticator použijte jednu z následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="89a37-129">For each account that you want to add to the Microsoft Authenticator app, use one of the following procedures:</span></span>

### <a name="add-a-personal-microsoft-account-to-the-app"></a><span data-ttu-id="89a37-130">Přidat do aplikace osobní účet Microsoft</span><span class="sxs-lookup"><span data-stu-id="89a37-130">Add a personal Microsoft account to the app</span></span>

<span data-ttu-id="89a37-131">Osobní účet Microsoft (jeden, který používáte pro přihlášení k Outlook.com, Xbox, Skype, atd.) všechny, které musíte udělat je, přihlaste se ke svému účtu v aplikaci Microsoft Authenticator.</span><span class="sxs-lookup"><span data-stu-id="89a37-131">For a personal Microsoft account (one that you use to sign in to Outlook.com, Xbox, Skype, etc.), all you have to do is sign in to your account in the Microsoft Authenticator app.</span></span>

### <a name="add-a-work-or-school-account-to-the-app-using-the-qr-code-scanner"></a><span data-ttu-id="89a37-132">Přidat pracovní nebo školní účet do aplikace pomocí skeneru kód QR</span><span class="sxs-lookup"><span data-stu-id="89a37-132">Add a work or school account to the app using the QR code scanner</span></span>
1. <span data-ttu-id="89a37-133">Přejděte na obrazovku nastavení ověření zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="89a37-133">Go to the security verification settings screen.</span></span>  <span data-ttu-id="89a37-134">Informace o tom, jak získat na tuto obrazovku najdete v tématu [Změna nastavení zabezpečení](multi-factor-authentication-end-user-manage-settings.md#where-to-find-the-settings-page).</span><span class="sxs-lookup"><span data-stu-id="89a37-134">For information on how to get to this screen, see [Changing your security settings](multi-factor-authentication-end-user-manage-settings.md#where-to-find-the-settings-page).</span></span>
2. <span data-ttu-id="89a37-135">Zaškrtněte políčko vedle **ověřovací aplikaci** vyberte **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="89a37-135">Check the box next to **Authenticator app** then select **Configure**.</span></span>

    ![Tlačítka konfigurovat na obrazovce nastavení ověření zabezpečení](./media/authenticator-app-how-to/azureauthe.png)

    <span data-ttu-id="89a37-137">Otevře obrazovky s kód QR na něm.</span><span class="sxs-lookup"><span data-stu-id="89a37-137">This brings up a screen with a QR code on it.</span></span>

    ![Obrazovka, která poskytuje kód QR](./media/authenticator-app-how-to/barcode2.png)
3. <span data-ttu-id="89a37-139">Otevřete aplikaci Microsoft Authenticator.</span><span class="sxs-lookup"><span data-stu-id="89a37-139">Open the Microsoft Authenticator app.</span></span> <span data-ttu-id="89a37-140">Na **účty** obrazovku, vyberte  **+** a potom zadejte, zda chcete přidat pracovní nebo školní účet.</span><span class="sxs-lookup"><span data-stu-id="89a37-140">On the **accounts** screen, select **+**, and then specify that you want to add a work or school account.</span></span>
4. <span data-ttu-id="89a37-141">Použít fotoaparát naskenujte kód QR, a potom vyberte **provádí** zavřete obrazovce kód QR.</span><span class="sxs-lookup"><span data-stu-id="89a37-141">Use the camera to scan the QR code, and then select **Done** to close the QR code screen.</span></span>

    <span data-ttu-id="89a37-142">Pokud svůj fotoaparát nepracuje správně, můžete [ručně zadejte kód QR a adresa URL](#add-an-account-to-the-app-manually).</span><span class="sxs-lookup"><span data-stu-id="89a37-142">If your camera is not working properly, you can [enter the QR code and URL manually](#add-an-account-to-the-app-manually).</span></span>

5. <span data-ttu-id="89a37-143">Pokud aplikace zobrazuje název účtu s šestimístný kód pod ním, jste hotovi.</span><span class="sxs-lookup"><span data-stu-id="89a37-143">When the app shows your account name with a six-digit code underneath it, you're done.</span></span> 

    ![Účty obrazovky](./media/authenticator-app-how-to/accounts.png)

### <a name="add-an-account-to-the-app-manually"></a><span data-ttu-id="89a37-145">Ručně přidat účet do aplikace</span><span class="sxs-lookup"><span data-stu-id="89a37-145">Add an account to the app manually</span></span>
1. <span data-ttu-id="89a37-146">Přejděte na obrazovku nastavení ověření zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="89a37-146">Go to the security verification settings screen.</span></span>  <span data-ttu-id="89a37-147">Informace o tom, jak získat na tuto obrazovku najdete v tématu [Změna nastavení zabezpečení](multi-factor-authentication-end-user-manage-settings.md).</span><span class="sxs-lookup"><span data-stu-id="89a37-147">For information on how to get to this screen, see [Changing your security settings](multi-factor-authentication-end-user-manage-settings.md).</span></span>
2. <span data-ttu-id="89a37-148">Vyberte **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="89a37-148">Select **Configure**.</span></span>

    ![Tlačítka konfigurovat na obrazovce nastavení ověření zabezpečení](./media/authenticator-app-how-to/azureauthe.png)

    <span data-ttu-id="89a37-150">Otevře obrazovky s kód QR na něm.</span><span class="sxs-lookup"><span data-stu-id="89a37-150">This brings up a screen with a QR code on it.</span></span>  <span data-ttu-id="89a37-151">Všimněte si kód a adresu URL.</span><span class="sxs-lookup"><span data-stu-id="89a37-151">Note the code and URL.</span></span>

    ![Obrazovky, který poskytuje kód QR a adresy URL](./media/authenticator-app-how-to/barcode2.png)
3. <span data-ttu-id="89a37-153">Otevřete aplikaci Microsoft Authenticator.</span><span class="sxs-lookup"><span data-stu-id="89a37-153">Open the Microsoft Authenticator app.</span></span> <span data-ttu-id="89a37-154">Na **účty** obrazovku, vyberte  **+** a potom zadejte, zda chcete přidat pracovní nebo školní účet.</span><span class="sxs-lookup"><span data-stu-id="89a37-154">On the **accounts** screen, select **+**, and then specify that you want to add a work or school account.</span></span>

4. <span data-ttu-id="89a37-155">V skener, vyberte **ručně zadejte kód**.</span><span class="sxs-lookup"><span data-stu-id="89a37-155">In the scanner, select **enter code manually**.</span></span>

    ![Obrazovka pro skenování kód QR](./media/multi-factor-authentication-end-user-first-time/scan2.png)
5. <span data-ttu-id="89a37-157">Zadejte kód a adresu URL do příslušných polí v aplikaci a potom vyberte **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="89a37-157">Enter the code and the URL in the appropriate boxes in the app, then select **Finish**.</span></span>

    ![Obrazovka pro zadání kód a adresu URL](./media/authenticator-app-how-to/manual.png)

6. <span data-ttu-id="89a37-159">Pokud aplikace zobrazuje název účtu s šestimístný kód pod ním, jste hotovi.</span><span class="sxs-lookup"><span data-stu-id="89a37-159">When the app shows your account name with a six-digit code underneath it, you're done.</span></span>

    ![Účty obrazovky](./media/authenticator-app-how-to/accounts.png)

### <a name="add-an-account-to-the-app-using-touch-id"></a><span data-ttu-id="89a37-161">Přidat účet k aplikaci pomocí Touch ID</span><span class="sxs-lookup"><span data-stu-id="89a37-161">Add an account to the app using Touch ID</span></span>
<span data-ttu-id="89a37-162">Aplikace Microsoft Authenticator v systému iOS podporuje Touch ID.</span><span class="sxs-lookup"><span data-stu-id="89a37-162">The Microsoft Authenticator app on iOS supports Touch ID.</span></span>  <span data-ttu-id="89a37-163">Azure Multi-Factor Authentication umožňuje organizacím vyžadoval kód PIN pro zařízení.</span><span class="sxs-lookup"><span data-stu-id="89a37-163">Azure Multi-Factor Authentication allows organizations to require a PIN for devices.</span></span> <span data-ttu-id="89a37-164">S Touch ID uživatelé systému iOS nemusíte zadání kódu PIN.</span><span class="sxs-lookup"><span data-stu-id="89a37-164">With Touch ID, iOS users don’t need to enter a PIN.</span></span> <span data-ttu-id="89a37-165">Místo toho můžete kontrolovat jejich otisk prstu a vybrat **schválit**.</span><span class="sxs-lookup"><span data-stu-id="89a37-165">Instead, they can scan their fingerprint and select **Approve**.</span></span>

<span data-ttu-id="89a37-166">Nastavení Touch ID s Microsoft Authenticator je jednoduché.</span><span class="sxs-lookup"><span data-stu-id="89a37-166">Setting up Touch ID with Microsoft Authenticator is simple.</span></span> <span data-ttu-id="89a37-167">Dokončení výzvy normální ověření pomocí kódu PIN.</span><span class="sxs-lookup"><span data-stu-id="89a37-167">You complete a normal verification challenge with a PIN.</span></span> <span data-ttu-id="89a37-168">Pokud vaše zařízení podporuje Touch ID, Microsoft Authenticator nastaví ho automaticky pro tento účet.</span><span class="sxs-lookup"><span data-stu-id="89a37-168">If your device supports Touch ID, Microsoft Authenticator sets it up automatically for that account.</span></span>

![Ověření instalace Touch ID](./media/authenticator-app-how-to/touchid1.png)

<span data-ttu-id="89a37-170">Z tohoto bodu dál, když budete vyzváni k ověření vaše přihlášení, vyberte přijaté nabízených oznámení a kontrolovat otisk prstu místo zadávání svůj PIN kód.</span><span class="sxs-lookup"><span data-stu-id="89a37-170">From that point forward, when you're required to verify your sign-in, you select the received push notification and scan your fingerprint instead of entering your PIN.</span></span>

![Nabízená oznámení](./media/authenticator-app-how-to/touchid2.png)

## <a name="use-the-app-when-you-sign-in"></a><span data-ttu-id="89a37-172">Použití aplikace při přihlášení</span><span class="sxs-lookup"><span data-stu-id="89a37-172">Use the app when you sign in</span></span>

<span data-ttu-id="89a37-173">Jakmile váš účet je přidán do aplikace, zobrazí se výzva provedete test ověření a ujistěte se, že všechno, co byla nakonfigurována správně.</span><span class="sxs-lookup"><span data-stu-id="89a37-173">Once your account is added to the app, you may be prompted to do a test verification to make sure everything was configured correctly.</span></span> <span data-ttu-id="89a37-174">Potom je vše.</span><span class="sxs-lookup"><span data-stu-id="89a37-174">After that, you're done!</span></span> <span data-ttu-id="89a37-175">Nemusíte dělat žádné další kroky až po příštím přihlášení.</span><span class="sxs-lookup"><span data-stu-id="89a37-175">You don't need to do anything else until the next time you sign in.</span></span>

<span data-ttu-id="89a37-176">Pokud jste se rozhodli použít ověřovací kódy v aplikaci, můžete začít neuvidíte na domovské stránce.</span><span class="sxs-lookup"><span data-stu-id="89a37-176">If you chose to use verification codes in the app, you start to see them on the home page.</span></span> <span data-ttu-id="89a37-177">Se změní každých 30 sekund, takže máte vždy nový kód, když budete potřebovat.</span><span class="sxs-lookup"><span data-stu-id="89a37-177">They change every 30 seconds so that you always have a new code when you need one.</span></span> <span data-ttu-id="89a37-178">Ale nemusíte dělat nic s nimi, dokud se přihlásit a zobrazí se výzva k zadání ověřovací kód.</span><span class="sxs-lookup"><span data-stu-id="89a37-178">But you don't need to do anything with them until you sign in and are prompted to enter a verification code.</span></span>  