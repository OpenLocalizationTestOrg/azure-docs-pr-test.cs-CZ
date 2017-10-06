---
title: "aaaLog lístku podpory pro řady StorSimple 8000 | Microsoft Docs"
description: "Zjistěte, jak požádat o toocreate podporu a spustit relaci podpory zařízení StorSimple."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 2ebc20fe-f490-4749-8e43-c9fac86f1676
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/27/2017
ms.author: alkohli;anbacker
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e1a3aa3c56e036c782c4fb502c477dc0feaa0ccd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="contact-microsoft-support-for-your-storsimple"></a><span data-ttu-id="3dc55-103">Obraťte se na podporu společnosti Microsoft pro vaše zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="3dc55-103">Contact Microsoft Support for your StorSimple</span></span>
<span data-ttu-id="3dc55-104">Pokud narazíte na potíže s vaším řešením Microsoft Azure StorSimple, můžete vytvořit žádost o službu pro technickou podporu.</span><span class="sxs-lookup"><span data-stu-id="3dc55-104">If you encounter any issues with your Microsoft Azure StorSimple solution, you can create a service request for technical support.</span></span> <span data-ttu-id="3dc55-105">V online relaci s svého specialistu technické podpory můžete také potřebovat toostart relaci podpory zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="3dc55-105">In an online session with your support engineer, you may also need toostart a support session on your StorSimple device.</span></span> <span data-ttu-id="3dc55-106">Tento článek vás provede procesem:</span><span class="sxs-lookup"><span data-stu-id="3dc55-106">This article walks you through:</span></span>

* <span data-ttu-id="3dc55-107">Jak požádat o toocreate podporu.</span><span class="sxs-lookup"><span data-stu-id="3dc55-107">How toocreate a support request.</span></span>
* <span data-ttu-id="3dc55-108">Jak hello toostart relaci podpora v rozhraní Windows PowerShell zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="3dc55-108">How toostart a support session in hello Windows PowerShell interface of your StorSimple device.</span></span>

<span data-ttu-id="3dc55-109">Zkontrolujte hello [StorSimple 8000 řady podporu SLA a informace o](https://msdn.microsoft.com/library/mt433077.aspx) předtím, než vytvoříte žádost o podporu.</span><span class="sxs-lookup"><span data-stu-id="3dc55-109">Review hello [StorSimple 8000 Series Support SLAs and information](https://msdn.microsoft.com/library/mt433077.aspx) before you create a Support request.</span></span>

## <a name="create-a-support-request"></a><span data-ttu-id="3dc55-110">Vytvořit žádost o podporu</span><span class="sxs-lookup"><span data-stu-id="3dc55-110">Create a support request</span></span>
<span data-ttu-id="3dc55-111">Proveďte následující kroky toocreate žádosti o podporu hello:</span><span class="sxs-lookup"><span data-stu-id="3dc55-111">Perform hello following steps toocreate a support request:</span></span>

#### <a name="toocreate-a-support-request"></a><span data-ttu-id="3dc55-112">toocreate žádosti o podporu</span><span class="sxs-lookup"><span data-stu-id="3dc55-112">toocreate a support request</span></span>
1. <span data-ttu-id="3dc55-113">V hello [portál Azure classic](https://manage.windowsazure.com/), v horním pravém rohu hello, klikněte na název účtu a pak klikněte na tlačítko **obraťte se na podporu společnosti Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="3dc55-113">In hello [Azure classic portal](https://manage.windowsazure.com/), in hello upper right corner, click your account name and then click **Contact Microsoft Support**.</span></span>
   
    ![MS obraťte se na podporu přes ManagementPortal](./media/storsimple-contact-microsoft-support/Ibiza1.png)
2. <span data-ttu-id="3dc55-115">Bude přesměrované toohello nový portál Azure (portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3dc55-115">You will be redirected toohello new Azure portal (portal.azure.com).</span></span> <span data-ttu-id="3dc55-116">Klikněte na tlačítko hello **nová žádost o podporu** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="3dc55-116">Click hello **New support request** tile.</span></span>
   
    ![MS obraťte se na podporu prostřednictvím nového portálu](./media/storsimple-contact-microsoft-support/Ibiza2.png)
   
    <span data-ttu-id="3dc55-118">Na pravé straně hello úvodní obrazovka, hello **nová žádost o podporu** podokně se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="3dc55-118">On hello right side of hello screen, hello **New support request** pane appears.</span></span> 
   
    ![Nové podokno žádosti o podporu](./media/storsimple-contact-microsoft-support/Ibiza3a.png)
3. <span data-ttu-id="3dc55-120">V hello **Základy** dialogové okno, dokončení hello následující:</span><span class="sxs-lookup"><span data-stu-id="3dc55-120">In hello **Basics** dialog box, complete hello following:</span></span>                                
   
   1. <span data-ttu-id="3dc55-121">Z hello **vydat typ** rozevíracího seznamu vyberte **technické**.</span><span class="sxs-lookup"><span data-stu-id="3dc55-121">From hello **Issue type** drop-down list , select **Technical**.</span></span>
   2. <span data-ttu-id="3dc55-122">Vyberte **předplatné** z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="3dc55-122">Select a **Subscription** from hello drop-down list.</span></span>
   3. <span data-ttu-id="3dc55-123">Z hello **služby** rozevíracího seznamu vyberte **StorSimple**.</span><span class="sxs-lookup"><span data-stu-id="3dc55-123">From hello **Service** drop-down list, select **StorSimple**.</span></span> 
   4. <span data-ttu-id="3dc55-124">Vyberte **plán podpory** z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="3dc55-124">Select a **Support plan** from hello drop-down list.</span></span> <span data-ttu-id="3dc55-125">Je nutné placené podporu plán tooenable technická podpora.</span><span class="sxs-lookup"><span data-stu-id="3dc55-125">You need a paid support plan tooenable Technical Support.</span></span>
4. <span data-ttu-id="3dc55-126">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="3dc55-126">Click **Next**.</span></span> <span data-ttu-id="3dc55-127">Hello **problém** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3dc55-127">hello **Problem** dialog box appears.</span></span>
   
    ![Nové podokno žádosti o podporu](./media/storsimple-contact-microsoft-support/Ibiza5a.png) 
5. <span data-ttu-id="3dc55-129">V hello **problém** dialogové okno, dokončení hello následující:</span><span class="sxs-lookup"><span data-stu-id="3dc55-129">In hello **Problem** dialog box, complete hello following:</span></span>
   
   1. <span data-ttu-id="3dc55-130">Vyberte **závažnost** úrovně z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="3dc55-130">Select a **Severity** level from hello drop-down list.</span></span>
   2. <span data-ttu-id="3dc55-131">Vyberte **typ problému** z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="3dc55-131">Select a **Problem type** from hello drop-down list.</span></span>
   3. <span data-ttu-id="3dc55-132">Vyberte **kategorie** z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="3dc55-132">Select a **Category** from hello drop-down list.</span></span> 
   4. <span data-ttu-id="3dc55-133">V hello **podrobnosti** pole Krátce popište svůj problém.</span><span class="sxs-lookup"><span data-stu-id="3dc55-133">In hello **Details** box, briefly describe your issue.</span></span>
   5. <span data-ttu-id="3dc55-134">V hello **časového rámce** pole, zda text hello datum, čas a časové pásmo, které odpovídá toohello nejaktuálnějšího výskytu vašeho problému.</span><span class="sxs-lookup"><span data-stu-id="3dc55-134">In hello **Time frame** box, indicate hello date, time, and time zone that corresponds toohello most recent occurrence of your issue.</span></span>
   6. <span data-ttu-id="3dc55-135">V části **nahrávání souborů**, klikněte na tlačítko hello složky ikonu toobrowse tooyour podporu balíčku.</span><span class="sxs-lookup"><span data-stu-id="3dc55-135">Under **File upload**, click hello folder icon toobrowse tooyour support package.</span></span>
   7. <span data-ttu-id="3dc55-136">Vyberte hello **sdílet diagnostické informace** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="3dc55-136">Select hello **Share diagnostic information** check box.</span></span>
6. <span data-ttu-id="3dc55-137">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="3dc55-137">Click **Next**.</span></span> <span data-ttu-id="3dc55-138">Hello **kontaktní informace** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3dc55-138">hello **Contact information** dialog box appears.</span></span>
   
    ![Nové podokno žádosti o podporu](./media/storsimple-contact-microsoft-support/Ibiza6a.png) 
7. <span data-ttu-id="3dc55-140">Zadejte své kontaktní informace a vyberte způsob kontaktu (telefon nebo e-mail).</span><span class="sxs-lookup"><span data-stu-id="3dc55-140">Enter your contact information and select a contact method (phone or email).</span></span> 
8. <span data-ttu-id="3dc55-141">Vyberte hello **uložit změny kontaktů pro budoucí požadavky na podporu** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="3dc55-141">Select hello **Save contact changes for future support requests** check box.</span></span>
9. <span data-ttu-id="3dc55-142">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3dc55-142">Click **Create**.</span></span>

<span data-ttu-id="3dc55-143">Po odeslání vaši žádost o pracovníka podpory vás bude kontaktovat co nejdříve tooproceed k žádosti.</span><span class="sxs-lookup"><span data-stu-id="3dc55-143">After you have submitted your request, a Support engineer will contact you as soon as possible tooproceed with your request.</span></span>

## <a name="start-a-support-session-in-windows-powershell-for-storsimple"></a><span data-ttu-id="3dc55-144">Spusťte relaci podpory v prostředí Windows PowerShell pro StorSimple</span><span class="sxs-lookup"><span data-stu-id="3dc55-144">Start a support session in Windows PowerShell for StorSimple</span></span>
<span data-ttu-id="3dc55-145">tootroubleshoot všechny problémy, které setkat s hello zařízení StorSimple, budete potřebovat tooengage s týmem Microsoft Support hello.</span><span class="sxs-lookup"><span data-stu-id="3dc55-145">tootroubleshoot any issues that you might experience with hello StorSimple device, you will need tooengage with hello Microsoft Support team.</span></span> <span data-ttu-id="3dc55-146">Microsoft Support může být nutné toouse toolog relace podporu na tooyour zařízení.</span><span class="sxs-lookup"><span data-stu-id="3dc55-146">Microsoft Support may need toouse a support session toolog on tooyour device.</span></span> 

<span data-ttu-id="3dc55-147">Proveďte následující hello kroky toostart relaci podpory:</span><span class="sxs-lookup"><span data-stu-id="3dc55-147">Perform hello following steps toostart a support session:</span></span>

#### <a name="toostart-a-support-session"></a><span data-ttu-id="3dc55-148">toostart relaci podpory</span><span class="sxs-lookup"><span data-stu-id="3dc55-148">toostart a support session</span></span>
1. <span data-ttu-id="3dc55-149">Přístup hello zařízení přímo pomocí konzoly sériového portu hello nebo prostřednictvím relace Telnetu ze vzdáleného počítače.</span><span class="sxs-lookup"><span data-stu-id="3dc55-149">Access hello device directly by using hello serial console or through a telnet session from a remote computer.</span></span> <span data-ttu-id="3dc55-150">toodo tento, postupujte podle kroků hello v [konzoly sériového portu toohello zařízení tooconnect pro použití klienta PuTTY](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="3dc55-150">toodo this, follow hello steps in [Use PuTTY tooconnect toohello device serial console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span></span>
2. <span data-ttu-id="3dc55-151">V relaci hello, které se otevře, stiskněte klávesu hello **Enter** klíče tooget příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="3dc55-151">In hello session that opens, press hello **Enter** key tooget a command prompt.</span></span>
3. <span data-ttu-id="3dc55-152">V nabídce konzoly sériového portu hello, vyberte možnost 1, **přihlásit úplný přístup**.</span><span class="sxs-lookup"><span data-stu-id="3dc55-152">In hello serial console menu, select option 1, **Log in with full access**.</span></span>
4. <span data-ttu-id="3dc55-153">Hello řádku zadejte následující heslo hello:</span><span class="sxs-lookup"><span data-stu-id="3dc55-153">At hello prompt, type hello following password:</span></span> 
   
    `Password1`
5. <span data-ttu-id="3dc55-154">Hello řádku zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="3dc55-154">At hello prompt, type hello following command:</span></span>
   
    `Enable-HcsSupportAccess`
6. <span data-ttu-id="3dc55-155">Jako zašifrovaný řetězec zobrazí tooyou.</span><span class="sxs-lookup"><span data-stu-id="3dc55-155">An encrypted string will be presented tooyou.</span></span> <span data-ttu-id="3dc55-156">Zkopírujte tento řetězec do textového editoru, například Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="3dc55-156">Copy this string into a text editor such as Notepad.</span></span>
7. <span data-ttu-id="3dc55-157">Uložte tento řetězec a poslat ho e-mailové zprávy tooMicrosoft podpory.</span><span class="sxs-lookup"><span data-stu-id="3dc55-157">Save this string and send it in an email message tooMicrosoft Support.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="3dc55-158">Podpora přístupu můžete vypnout spuštěním `Disable-HcsSupportAccess`.</span><span class="sxs-lookup"><span data-stu-id="3dc55-158">You can disable support access by running `Disable-HcsSupportAccess`.</span></span> <span data-ttu-id="3dc55-159">zařízení StorSimple Hello pokusí toodisable podporu přístup také 8 hodin po hello relace byl inicializován.</span><span class="sxs-lookup"><span data-stu-id="3dc55-159">hello StorSimple device will also attempt toodisable support access 8 hours after hello session was initiated.</span></span> <span data-ttu-id="3dc55-160">Je nejlepší postup toochange zařízení StorSimple pověření po inicializaci relaci podpory.</span><span class="sxs-lookup"><span data-stu-id="3dc55-160">It is a best practice toochange your StorSimple device credentials after initiating a support session.</span></span>
> 
> 

