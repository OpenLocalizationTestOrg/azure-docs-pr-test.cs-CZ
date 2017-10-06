---
title: "aaaMicrosoft ověřovací aplikací pro mobilní telefony | Microsoft Docs"
description: "Zjistěte, jak tooupgrade toohello nejnovější verzi Azure Authenticator."
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
ms.openlocfilehash: d895d92d89613cbafd9fc09d4ff4810cbf25652e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-microsoft-authenticator-app"></a><span data-ttu-id="33183-103">Začínáme s aplikací Microsoft Authenticator hello</span><span class="sxs-lookup"><span data-stu-id="33183-103">Get started with hello Microsoft Authenticator app</span></span>
<span data-ttu-id="33183-104">Hello aplikaci Microsoft Authenticator poskytuje další úroveň zabezpečení v pracovní nebo školní účet (například bsimon@contoso.com) nebo účtu Microsoft (například bsimon@outlook.com).</span><span class="sxs-lookup"><span data-stu-id="33183-104">hello Microsoft Authenticator app provides an additional level of security in your work or school account (for example, bsimon@contoso.com) or your Microsoft account (for example, bsimon@outlook.com).</span></span>

<span data-ttu-id="33183-105">aplikace Hello funguje v jednom ze dvou způsobů:</span><span class="sxs-lookup"><span data-stu-id="33183-105">hello app works in one of two ways:</span></span>

* <span data-ttu-id="33183-106">**Oznámení**.</span><span class="sxs-lookup"><span data-stu-id="33183-106">**Notification**.</span></span> <span data-ttu-id="33183-107">aplikace Hello můžou pomoct zabránit neoprávněnému přístupu tooaccounts a zastavit podvodné transakce vynucením tablet nebo smartphone tooyour oznámení.</span><span class="sxs-lookup"><span data-stu-id="33183-107">hello app can help prevent unauthorized access tooaccounts and stop fraudulent transactions by pushing a notification tooyour smartphone or tablet.</span></span> <span data-ttu-id="33183-108">Jednoduše zobrazení hello oznámení a pokud je oprávněné, vyberte **ověřte**.</span><span class="sxs-lookup"><span data-stu-id="33183-108">Simply view hello notification, and if it's legitimate, select **Verify**.</span></span> <span data-ttu-id="33183-109">Jinak, můžete vybrat **Odepřít**.</span><span class="sxs-lookup"><span data-stu-id="33183-109">Otherwise, you can select **Deny**.</span></span> 
* <span data-ttu-id="33183-110">**Ověřovací kód**.</span><span class="sxs-lookup"><span data-stu-id="33183-110">**Verification code**.</span></span> <span data-ttu-id="33183-111">Hello aplikace můžete použít jako softwaru token toogenerate OAuth ověřovací kód.</span><span class="sxs-lookup"><span data-stu-id="33183-111">hello app can be used as a software token toogenerate an OAuth verification code.</span></span> <span data-ttu-id="33183-112">Po zadání uživatelského jména a hesla, jste zadali kód hello poskytované aplikace hello do přihlašovací obrazovky hello.</span><span class="sxs-lookup"><span data-stu-id="33183-112">After you enter your username and password, you enter hello code provided by hello app into hello sign-in screen.</span></span> <span data-ttu-id="33183-113">ověřovací kód Hello poskytuje druhou podobu ověřování.</span><span class="sxs-lookup"><span data-stu-id="33183-113">hello verification code provides a second form of authentication.</span></span>

<span data-ttu-id="33183-114">aplikace Microsoft Authenticator Hello nahrazuje aplikaci Azure Authenticator hello.</span><span class="sxs-lookup"><span data-stu-id="33183-114">hello Microsoft Authenticator app replaces hello Azure Authenticator app.</span></span> <span data-ttu-id="33183-115">aplikaci Azure Authenticator Hello pořád funguje, ale pokud se rozhodnete nové aplikace Microsoft Authenticator toomove toohello, tento článek vám může pomoci.</span><span class="sxs-lookup"><span data-stu-id="33183-115">hello Azure Authenticator app still works, but if you decide toomove toohello new Microsoft Authenticator app, this article can assist you.</span></span>  

## <a name="opt-in-for-two-step-verification"></a><span data-ttu-id="33183-116">Vyjádřit výslovný souhlas pro dvoustupňové ověření</span><span class="sxs-lookup"><span data-stu-id="33183-116">Opt in for two-step verification</span></span>

<span data-ttu-id="33183-117">aplikace Microsoft Authenticator Hello nefunguje samostatně.</span><span class="sxs-lookup"><span data-stu-id="33183-117">hello Microsoft Authenticator app doesn't work by itself.</span></span> <span data-ttu-id="33183-118">Konfigurovat tooprompt vaše účty pro druhý metoda ověření po můžete přihlásit pomocí uživatelského jména a hesla.</span><span class="sxs-lookup"><span data-stu-id="33183-118">Configure each of your accounts tooprompt you for a second verification method after you sign in with your username and password.</span></span> 

<span data-ttu-id="33183-119">Pro pracovní nebo školní účet neobdržíte obvykle toochoose této funkce si sami.</span><span class="sxs-lookup"><span data-stu-id="33183-119">For a work or school account, you don't usually get toochoose this feature for yourself.</span></span> <span data-ttu-id="33183-120">Místo toho správce zabezpečení vyjádřit výslovný souhlas vaším jménem a upozorní vás tooregister metody ověření pro váš účet.</span><span class="sxs-lookup"><span data-stu-id="33183-120">Instead, a security administrator opts in on your behalf and then notifies you tooregister verification methods for your account.</span></span> <span data-ttu-id="33183-121">Pokud tento scénář se vztahuje tooyou, přečtěte si informace v [co Azure Multi-Factor Authentication znamená pro mě nejlepší](multi-factor-authentication-end-user.md).</span><span class="sxs-lookup"><span data-stu-id="33183-121">If this scenario applies tooyou, learn more in [What does Azure Multi-Factor Authentication mean for me](multi-factor-authentication-end-user.md).</span></span>

<span data-ttu-id="33183-122">Pro osobní účet budete potřebovat tooset si dvoustupňové ověřování pro sami.</span><span class="sxs-lookup"><span data-stu-id="33183-122">For a personal account, you need tooset up two-step verification for yourself.</span></span> <span data-ttu-id="33183-123">Pokud máte účet Microsoft, tyto kroky jsou k dispozici v [o dvoustupňovém ověřování](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).</span><span class="sxs-lookup"><span data-stu-id="33183-123">If you have a Microsoft account, those steps are available in [About two-step verification](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).</span></span> 

<span data-ttu-id="33183-124">Můžete taky hello Microsoft Authenticator s účty jiných společností než Microsoft.</span><span class="sxs-lookup"><span data-stu-id="33183-124">You can also use hello Microsoft Authenticator with non-Microsoft accounts.</span></span> <span data-ttu-id="33183-125">Funkce hello se volání něco jiného než dvoustupňové ověření, ale musí být schopný toofind ji v části Nastavení zabezpečení nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="33183-125">They may call hello feature something other than two-step verification, but you should be able toofind it under security or sign-in settings.</span></span> 

## <a name="install-hello-app"></a><span data-ttu-id="33183-126">Instalace aplikace hello</span><span class="sxs-lookup"><span data-stu-id="33183-126">Install hello app</span></span>
<span data-ttu-id="33183-127">je k dispozici pro aplikaci Microsoft Authenticator Hello [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), a [iOS](http://go.microsoft.com/fwlink/?Linkid=825073).</span><span class="sxs-lookup"><span data-stu-id="33183-127">hello Microsoft Authenticator app is available for [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), and [iOS](http://go.microsoft.com/fwlink/?Linkid=825073).</span></span>

## <a name="add-accounts-toohello-app"></a><span data-ttu-id="33183-128">Přidat aplikaci toohello účty</span><span class="sxs-lookup"><span data-stu-id="33183-128">Add accounts toohello app</span></span>
<span data-ttu-id="33183-129">U každého účtu, které chcete aplikaci Microsoft Authenticator toohello tooadd použijte jednu z následujících postupů hello:</span><span class="sxs-lookup"><span data-stu-id="33183-129">For each account that you want tooadd toohello Microsoft Authenticator app, use one of hello following procedures:</span></span>

### <a name="add-a-personal-microsoft-account-toohello-app"></a><span data-ttu-id="33183-130">Přidat aplikaci toohello osobní účet Microsoft</span><span class="sxs-lookup"><span data-stu-id="33183-130">Add a personal Microsoft account toohello app</span></span>

<span data-ttu-id="33183-131">Pro osobní účet Microsoft (jeden použít toosign v tooOutlook.com, Xbox, Skype, apod), všechny máte toodo je přihlášení tooyour účtu v aplikaci Microsoft Authenticator hello.</span><span class="sxs-lookup"><span data-stu-id="33183-131">For a personal Microsoft account (one that you use toosign in tooOutlook.com, Xbox, Skype, etc.), all you have toodo is sign in tooyour account in hello Microsoft Authenticator app.</span></span>

### <a name="add-a-work-or-school-account-toohello-app-using-hello-qr-code-scanner"></a><span data-ttu-id="33183-132">Přidat pracovní nebo školní účet toohello aplikace pomocí skener kód QR hello</span><span class="sxs-lookup"><span data-stu-id="33183-132">Add a work or school account toohello app using hello QR code scanner</span></span>
1. <span data-ttu-id="33183-133">Obrazovka nastavení ověření zabezpečení toohello přejděte.</span><span class="sxs-lookup"><span data-stu-id="33183-133">Go toohello security verification settings screen.</span></span>  <span data-ttu-id="33183-134">Informace o tom tooget toothis obrazovky, najdete v části [Změna nastavení zabezpečení](multi-factor-authentication-end-user-manage-settings.md#where-to-find-the-settings-page).</span><span class="sxs-lookup"><span data-stu-id="33183-134">For information on how tooget toothis screen, see [Changing your security settings](multi-factor-authentication-end-user-manage-settings.md#where-to-find-the-settings-page).</span></span>
2. <span data-ttu-id="33183-135">Zaškrtněte políčko hello vedle příliš**ověřovací aplikaci** vyberte **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="33183-135">Check hello box next too**Authenticator app** then select **Configure**.</span></span>

    ![tlačítko Konfigurovat Hello na obrazovku nastavení ověření zabezpečení hello](./media/authenticator-app-how-to/azureauthe.png)

    <span data-ttu-id="33183-137">Otevře obrazovky s kód QR na něm.</span><span class="sxs-lookup"><span data-stu-id="33183-137">This brings up a screen with a QR code on it.</span></span>

    ![Obrazovka, která poskytuje kód QR hello](./media/authenticator-app-how-to/barcode2.png)
3. <span data-ttu-id="33183-139">Aplikace Microsoft Authenticator otevřete hello.</span><span class="sxs-lookup"><span data-stu-id="33183-139">Open hello Microsoft Authenticator app.</span></span> <span data-ttu-id="33183-140">Na hello **účty** obrazovku, vyberte  **+** a pak určete, zda má tooadd pracovní nebo školní účet.</span><span class="sxs-lookup"><span data-stu-id="33183-140">On hello **accounts** screen, select **+**, and then specify that you want tooadd a work or school account.</span></span>
4. <span data-ttu-id="33183-141">Použít hello fotoaparát tooscan hello QR kód a potom vyberte **provádí** tooclose hello QR kód obrazovky.</span><span class="sxs-lookup"><span data-stu-id="33183-141">Use hello camera tooscan hello QR code, and then select **Done** tooclose hello QR code screen.</span></span>

    <span data-ttu-id="33183-142">Pokud svůj fotoaparát nepracuje správně, můžete [hello QR kód a adresu URL zadat ručně](#add-an-account-to-the-app-manually).</span><span class="sxs-lookup"><span data-stu-id="33183-142">If your camera is not working properly, you can [enter hello QR code and URL manually](#add-an-account-to-the-app-manually).</span></span>

5. <span data-ttu-id="33183-143">Pokud aplikace hello ukazuje název účtu s šestimístný kód pod ním, jste hotovi.</span><span class="sxs-lookup"><span data-stu-id="33183-143">When hello app shows your account name with a six-digit code underneath it, you're done.</span></span> 

    ![Účty obrazovky](./media/authenticator-app-how-to/accounts.png)

### <a name="add-an-account-toohello-app-manually"></a><span data-ttu-id="33183-145">Ručně přidejte účet toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="33183-145">Add an account toohello app manually</span></span>
1. <span data-ttu-id="33183-146">Obrazovka nastavení ověření zabezpečení toohello přejděte.</span><span class="sxs-lookup"><span data-stu-id="33183-146">Go toohello security verification settings screen.</span></span>  <span data-ttu-id="33183-147">Informace o tom tooget toothis obrazovky, najdete v části [Změna nastavení zabezpečení](multi-factor-authentication-end-user-manage-settings.md).</span><span class="sxs-lookup"><span data-stu-id="33183-147">For information on how tooget toothis screen, see [Changing your security settings](multi-factor-authentication-end-user-manage-settings.md).</span></span>
2. <span data-ttu-id="33183-148">Vyberte **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="33183-148">Select **Configure**.</span></span>

    ![tlačítko Konfigurovat Hello na obrazovku nastavení ověření zabezpečení hello](./media/authenticator-app-how-to/azureauthe.png)

    <span data-ttu-id="33183-150">Otevře obrazovky s kód QR na něm.</span><span class="sxs-lookup"><span data-stu-id="33183-150">This brings up a screen with a QR code on it.</span></span>  <span data-ttu-id="33183-151">Všimněte si hello kód a adresu URL.</span><span class="sxs-lookup"><span data-stu-id="33183-151">Note hello code and URL.</span></span>

    ![Obrazovka, která poskytuje hello QR kód a adresu URL](./media/authenticator-app-how-to/barcode2.png)
3. <span data-ttu-id="33183-153">Aplikace Microsoft Authenticator otevřete hello.</span><span class="sxs-lookup"><span data-stu-id="33183-153">Open hello Microsoft Authenticator app.</span></span> <span data-ttu-id="33183-154">Na hello **účty** obrazovku, vyberte  **+** a pak určete, zda má tooadd pracovní nebo školní účet.</span><span class="sxs-lookup"><span data-stu-id="33183-154">On hello **accounts** screen, select **+**, and then specify that you want tooadd a work or school account.</span></span>

4. <span data-ttu-id="33183-155">V hello skener, vyberte **ručně zadejte kód**.</span><span class="sxs-lookup"><span data-stu-id="33183-155">In hello scanner, select **enter code manually**.</span></span>

    ![Obrazovka pro skenování kód QR](./media/multi-factor-authentication-end-user-first-time/scan2.png)
5. <span data-ttu-id="33183-157">Zadejte hello kód a adresu URL hello hello příslušných polí v aplikaci hello a potom vyberte **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="33183-157">Enter hello code and hello URL in hello appropriate boxes in hello app, then select **Finish**.</span></span>

    ![Obrazovka pro zadání kód a adresu URL](./media/authenticator-app-how-to/manual.png)

6. <span data-ttu-id="33183-159">Pokud aplikace hello ukazuje název účtu s šestimístný kód pod ním, jste hotovi.</span><span class="sxs-lookup"><span data-stu-id="33183-159">When hello app shows your account name with a six-digit code underneath it, you're done.</span></span>

    ![Účty obrazovky](./media/authenticator-app-how-to/accounts.png)

### <a name="add-an-account-toohello-app-using-touch-id"></a><span data-ttu-id="33183-161">Přidat účet toohello aplikaci pomocí Touch ID</span><span class="sxs-lookup"><span data-stu-id="33183-161">Add an account toohello app using Touch ID</span></span>
<span data-ttu-id="33183-162">aplikace Microsoft Authenticator Hello v systému iOS podporuje Touch ID.</span><span class="sxs-lookup"><span data-stu-id="33183-162">hello Microsoft Authenticator app on iOS supports Touch ID.</span></span>  <span data-ttu-id="33183-163">Azure Multi-Factor Authentication umožňuje organizacím toorequire PIN kódu pro zařízení.</span><span class="sxs-lookup"><span data-stu-id="33183-163">Azure Multi-Factor Authentication allows organizations toorequire a PIN for devices.</span></span> <span data-ttu-id="33183-164">Uživatelé systému iOS se Touch ID, nepotřebujete tooenter kód PIN.</span><span class="sxs-lookup"><span data-stu-id="33183-164">With Touch ID, iOS users don’t need tooenter a PIN.</span></span> <span data-ttu-id="33183-165">Místo toho můžete kontrolovat jejich otisk prstu a vybrat **schválit**.</span><span class="sxs-lookup"><span data-stu-id="33183-165">Instead, they can scan their fingerprint and select **Approve**.</span></span>

<span data-ttu-id="33183-166">Nastavení Touch ID s Microsoft Authenticator je jednoduché.</span><span class="sxs-lookup"><span data-stu-id="33183-166">Setting up Touch ID with Microsoft Authenticator is simple.</span></span> <span data-ttu-id="33183-167">Dokončení výzvy normální ověření pomocí kódu PIN.</span><span class="sxs-lookup"><span data-stu-id="33183-167">You complete a normal verification challenge with a PIN.</span></span> <span data-ttu-id="33183-168">Pokud vaše zařízení podporuje Touch ID, Microsoft Authenticator nastaví ho automaticky pro tento účet.</span><span class="sxs-lookup"><span data-stu-id="33183-168">If your device supports Touch ID, Microsoft Authenticator sets it up automatically for that account.</span></span>

![Ověření instalace Touch ID](./media/authenticator-app-how-to/touchid1.png)

<span data-ttu-id="33183-170">Z tohoto bodu předání, když jste požadované tooverify vaše přihlášení, vyberte hello přijatých nabízených oznámení a kontrolovat otisk prstu místo zadávání svůj PIN kód.</span><span class="sxs-lookup"><span data-stu-id="33183-170">From that point forward, when you're required tooverify your sign-in, you select hello received push notification and scan your fingerprint instead of entering your PIN.</span></span>

![Nabízená oznámení](./media/authenticator-app-how-to/touchid2.png)

## <a name="use-hello-app-when-you-sign-in"></a><span data-ttu-id="33183-172">Použití aplikace hello při přihlášení</span><span class="sxs-lookup"><span data-stu-id="33183-172">Use hello app when you sign in</span></span>

<span data-ttu-id="33183-173">Jakmile je váš účet je přidán toohello aplikace, může být výzvami toodo toomake ověřovací test, která je opravdu že všechno, co byla nakonfigurována správně.</span><span class="sxs-lookup"><span data-stu-id="33183-173">Once your account is added toohello app, you may be prompted toodo a test verification toomake sure everything was configured correctly.</span></span> <span data-ttu-id="33183-174">Potom je vše.</span><span class="sxs-lookup"><span data-stu-id="33183-174">After that, you're done!</span></span> <span data-ttu-id="33183-175">Nepotřebujete toodo cokoliv jiného dokud hello příštím přihlášení.</span><span class="sxs-lookup"><span data-stu-id="33183-175">You don't need toodo anything else until hello next time you sign in.</span></span>

<span data-ttu-id="33183-176">Pokud jste zvolili toouse ověřovací kódy v hello aplikaci, můžete spustit toosee je na domovskou stránku hello.</span><span class="sxs-lookup"><span data-stu-id="33183-176">If you chose toouse verification codes in hello app, you start toosee them on hello home page.</span></span> <span data-ttu-id="33183-177">Se změní každých 30 sekund, takže máte vždy nový kód, když budete potřebovat.</span><span class="sxs-lookup"><span data-stu-id="33183-177">They change every 30 seconds so that you always have a new code when you need one.</span></span> <span data-ttu-id="33183-178">Ale nepotřebujete toodo něco s nimi dokud přihlásit a jsou výzvami tooenter ověřovací kód.</span><span class="sxs-lookup"><span data-stu-id="33183-178">But you don't need toodo anything with them until you sign in and are prompted tooenter a verification code.</span></span>  
