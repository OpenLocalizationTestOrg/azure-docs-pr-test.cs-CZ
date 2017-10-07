---
title: "aaaCreate podporu lístků nebo případ pro řady StorSimple 8000 | Microsoft Docs"
description: "Zjistěte, jak toolog podporují požadavek a spustit relaci podporu ve vašem zařízení řady StorSimple 8000."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: alkohli;
ms.openlocfilehash: 832e3ac739dafa6dd8c24aaea38aef9c6f1b27b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="contact-microsoft-support"></a><span data-ttu-id="76bd8-103">Obraťte se na podporu společnosti Microsoft</span><span class="sxs-lookup"><span data-stu-id="76bd8-103">Contact Microsoft Support</span></span>

<span data-ttu-id="76bd8-104">Hello Správce zařízení StorSimple poskytuje možnost hello příliš**protokolu nové žádosti o podporu** v rámci hello služby souhrnu okna.</span><span class="sxs-lookup"><span data-stu-id="76bd8-104">hello StorSimple Device Manager provides hello capability too**log a new support request** within hello service summary blade.</span></span> <span data-ttu-id="76bd8-105">Pokud narazíte na potíže s vašeho řešení StorSimple, můžete vytvořit žádost o službu pro technickou podporu.</span><span class="sxs-lookup"><span data-stu-id="76bd8-105">If you encounter any issues with your StorSimple solution, you can create a service request for technical support.</span></span> <span data-ttu-id="76bd8-106">V online relaci s svého specialistu technické podpory můžete také potřebovat toostart relaci podpory zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="76bd8-106">In an online session with your support engineer, you may also need toostart a support session on your StorSimple device.</span></span> <span data-ttu-id="76bd8-107">Tento článek vás provede procesem:</span><span class="sxs-lookup"><span data-stu-id="76bd8-107">This article walks you through:</span></span>

* <span data-ttu-id="76bd8-108">Jak požádat o toocreate podporu.</span><span class="sxs-lookup"><span data-stu-id="76bd8-108">How toocreate a support request.</span></span>
* <span data-ttu-id="76bd8-109">Jak toomanage podporu žádost životního cyklu z portálu hello.</span><span class="sxs-lookup"><span data-stu-id="76bd8-109">How toomanage a support request lifecycle from within hello portal.</span></span>
* <span data-ttu-id="76bd8-110">Jak hello toostart relaci podpora v rozhraní Windows PowerShell zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="76bd8-110">How toostart a support session in hello Windows PowerShell interface of your StorSimple device.</span></span>

<span data-ttu-id="76bd8-111">Zkontrolujte hello [StorSimple 8000 řady podporu SLA a informace o](https://msdn.microsoft.com/library/mt433077.aspx) předtím, než vytvoříte žádost o podporu.</span><span class="sxs-lookup"><span data-stu-id="76bd8-111">Review hello [StorSimple 8000 Series Support SLAs and information](https://msdn.microsoft.com/library/mt433077.aspx) before you create a Support request.</span></span>

## <a name="create-a-support-request"></a><span data-ttu-id="76bd8-112">Vytvořit žádost o podporu</span><span class="sxs-lookup"><span data-stu-id="76bd8-112">Create a support request</span></span>

<span data-ttu-id="76bd8-113">V závislosti na vaší [plán podpory](https://azure.microsoft.com/support/plans/), můžete vytvořit lístky žádostí o podporu pro problém na zařízení StorSimple přímo z souhrnu okna služby hello Správce zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="76bd8-113">Depending upon your [support plan](https://azure.microsoft.com/support/plans/), you can create support tickets for an issue on your StorSimple device directly from hello StorSimple Device Manager service summary blade.</span></span> <span data-ttu-id="76bd8-114">Proveďte následující kroky toocreate žádosti o podporu hello:</span><span class="sxs-lookup"><span data-stu-id="76bd8-114">Perform hello following steps toocreate a support request:</span></span>

#### <a name="toocreate-a-support-request"></a><span data-ttu-id="76bd8-115">toocreate žádosti o podporu</span><span class="sxs-lookup"><span data-stu-id="76bd8-115">toocreate a support request</span></span>

1. <span data-ttu-id="76bd8-116">Přejděte služby StorSimple Manager zařízení tooyour.</span><span class="sxs-lookup"><span data-stu-id="76bd8-116">Go tooyour StorSimple Device Manager service.</span></span> <span data-ttu-id="76bd8-117">V nastavení souhrnu okna hello služby, přejděte příliš**podporu + Poradce při potížích s** části a pak klikněte na **nová žádost o podporu**.</span><span class="sxs-lookup"><span data-stu-id="76bd8-117">In hello service summary blade settings, go too**SUPPORT + TROUBLESHOOTING** section and then click **New support request**.</span></span>
     
    ![MS obraťte se na podporu prostřednictvím nového portálu](./media/storsimple-8000-contact-microsoft-support/contactsupport1.png)
   
2. <span data-ttu-id="76bd8-119">V hello **nová žádost o podporu** vyberte **Základy**.</span><span class="sxs-lookup"><span data-stu-id="76bd8-119">In hello **New support request** blade, select **Basics**.</span></span> <span data-ttu-id="76bd8-120">V hello **Základy** okně hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="76bd8-120">In hello **Basics** blade, do hello following steps:</span></span>
   1. <span data-ttu-id="76bd8-121">Z hello **vydat typ** rozevíracího seznamu vyberte **technické**.</span><span class="sxs-lookup"><span data-stu-id="76bd8-121">From hello **Issue type** drop-down list , select **Technical**.</span></span>
   2. <span data-ttu-id="76bd8-122">Hello aktuální **předplatné**, **služby** typu a hello **prostředků** (zařízení služby StorSimple Manager) se automaticky vybraná.</span><span class="sxs-lookup"><span data-stu-id="76bd8-122">hello current **Subscription**, **Service** type, and hello **Resource** (StorSimple Device Manager service) are automatically chosen.</span></span> 
   3. <span data-ttu-id="76bd8-123">Vyberte **plán podpory** hello rozevíracího seznamu, pokud máte více schématy spojené s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="76bd8-123">Select a **Support plan** from hello drop-down list if you have multiple plans associated with your subscription.</span></span> <span data-ttu-id="76bd8-124">Je nutné placené podporu plán tooenable technická podpora.</span><span class="sxs-lookup"><span data-stu-id="76bd8-124">You need a paid support plan tooenable Technical Support.</span></span>
   4. <span data-ttu-id="76bd8-125">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="76bd8-125">Click **Next**.</span></span>

       ![MS obraťte se na podporu prostřednictvím nového portálu](./media/storsimple-8000-contact-microsoft-support/contactsupport2.png)

3. <span data-ttu-id="76bd8-127">V hello **nová žádost o podporu** vyberte **krok 2 problém**.</span><span class="sxs-lookup"><span data-stu-id="76bd8-127">In hello **New support request** blade, select **Step 2 Problem**.</span></span> <span data-ttu-id="76bd8-128">V hello **problém** okně hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="76bd8-128">In hello **Problem** blade, do hello following steps:</span></span>
    
    1. <span data-ttu-id="76bd8-129">Zvolte hello **závažnost**.</span><span class="sxs-lookup"><span data-stu-id="76bd8-129">Choose hello **Severity**.</span></span>
    2. <span data-ttu-id="76bd8-130">Zadejte, zda je problém hello související toohello zařízení nebo hello služby StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="76bd8-130">Specify if hello issue is related toohello appliance or hello StorSimple Device Manager service.</span></span>
    3. <span data-ttu-id="76bd8-131">Vyberte **kategorie** pro toto vydání a zadejte více **podrobnosti** o hello problému.</span><span class="sxs-lookup"><span data-stu-id="76bd8-131">Choose a **Category** for this issue and provide more **Details** about hello issue.</span></span>
    4. <span data-ttu-id="76bd8-132">Zadejte hello počáteční datum a čas pro hello problém.</span><span class="sxs-lookup"><span data-stu-id="76bd8-132">Provide hello start date and time for hello problem.</span></span>
    5. <span data-ttu-id="76bd8-133">V hello **nahrávání souborů**, klikněte na tlačítko hello složky ikonu toobrowse tooyour podporu balíčku.</span><span class="sxs-lookup"><span data-stu-id="76bd8-133">In hello **File upload**, click hello folder icon toobrowse tooyour support package.</span></span>
    6. <span data-ttu-id="76bd8-134">Zkontrolujte **sdílet diagnostické informace**.</span><span class="sxs-lookup"><span data-stu-id="76bd8-134">Check **Share diagnostic information**.</span></span>
    7. <span data-ttu-id="76bd8-135">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="76bd8-135">Click **Next**.</span></span>

       ![MS obraťte se na podporu prostřednictvím nového portálu](./media/storsimple-8000-contact-microsoft-support/contactsupport3.png) 

4. <span data-ttu-id="76bd8-137">V hello **nová žádost o podporu** okně klikněte na tlačítko **krok 3 kontaktní údaje**.</span><span class="sxs-lookup"><span data-stu-id="76bd8-137">In hello **New support request** blade, click **Step 3 Contact information**.</span></span> <span data-ttu-id="76bd8-138">V hello **kontaktní informace** okně hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="76bd8-138">In hello **Contact information** blade, do hello following steps:</span></span>

    1. <span data-ttu-id="76bd8-139">V hello **obraťte se na možnosti**, poskytovat upřednostňovaný způsob kontaktování (telefon nebo e-mail) a hello jazyk.</span><span class="sxs-lookup"><span data-stu-id="76bd8-139">In hello **Contact options**, provide your preferred contact method (phone or email) and hello language.</span></span> <span data-ttu-id="76bd8-140">Doba odezvy Hello se vybere automaticky podle plánu předplatného.</span><span class="sxs-lookup"><span data-stu-id="76bd8-140">hello response time is automatically selected based on your subscription plan.</span></span>
    2. <span data-ttu-id="76bd8-141">V hello kontaktní údaje zadejte název, e-mailu, obraťte se na volitelné, země.</span><span class="sxs-lookup"><span data-stu-id="76bd8-141">In hello Contact information, provide your name, email, optional contact, country.</span></span> <span data-ttu-id="76bd8-142">Vyberte hello **uložit změny kontaktů pro budoucí požadavky na podporu** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="76bd8-142">Select hello **Save contact changes for future support requests** check box.</span></span>
    3. <span data-ttu-id="76bd8-143">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="76bd8-143">Click **Create**.</span></span>
   
        ![MS obraťte se na podporu prostřednictvím nového portálu](./media/storsimple-8000-contact-microsoft-support/contactsupport5.png)   

    <span data-ttu-id="76bd8-145">Microsoft Support použije tooreach tato informace se tooyou pro další informace, diagnostiku a řešení.</span><span class="sxs-lookup"><span data-stu-id="76bd8-145">Microsoft Support will use this information tooreach out tooyou for further information, diagnosis, and resolution.</span></span>
<span data-ttu-id="76bd8-146">Po odeslání vaši žádost o pracovníka podpory vás bude kontaktovat co nejdříve tooproceed k žádosti.</span><span class="sxs-lookup"><span data-stu-id="76bd8-146">After you have submitted your request, a Support engineer will contact you as soon as possible tooproceed with your request.</span></span>

## <a name="manage-a-support-request"></a><span data-ttu-id="76bd8-147">Spravovat žádosti o podporu</span><span class="sxs-lookup"><span data-stu-id="76bd8-147">Manage a support request</span></span>

<span data-ttu-id="76bd8-148">Po vytvoření lístku podpory, můžete spravovat životní cyklus hello hello lístku z portálu hello.</span><span class="sxs-lookup"><span data-stu-id="76bd8-148">After creating a support ticket, you can manage hello lifecycle of hello ticket from within hello portal.</span></span>

#### <a name="toomanage-your-support-requests"></a><span data-ttu-id="76bd8-149">toomanage podporu požadavků</span><span class="sxs-lookup"><span data-stu-id="76bd8-149">toomanage your support requests</span></span>

1. <span data-ttu-id="76bd8-150">tooget toohello nápovědu a podporu stránky, přejděte příliš**procházet > Nápověda a podpora**.</span><span class="sxs-lookup"><span data-stu-id="76bd8-150">tooget toohello help and support page, navigate too**Browse > Help + support**.</span></span>

    ![Spravovat žádosti o podporu](./media/storsimple-8000-contact-microsoft-support/managesupport1.png)

2. <span data-ttu-id="76bd8-152">Tabulkový seznam všech hello podporu požadavků se zobrazí v hello **Nápověda a podpora** okno.</span><span class="sxs-lookup"><span data-stu-id="76bd8-152">A tabular listing of All hello support requests is displayed in hello **Help + support** blade.</span></span>

    ![Spravovat žádosti o podporu](./media/storsimple-8000-contact-microsoft-support/managesupport2.png)

3. <span data-ttu-id="76bd8-154">Vyberte a klikněte na žádost o podporu.</span><span class="sxs-lookup"><span data-stu-id="76bd8-154">Select and click a support request.</span></span> <span data-ttu-id="76bd8-155">Můžete zobrazit stav hello a hello podrobnosti pro tento požadavek.</span><span class="sxs-lookup"><span data-stu-id="76bd8-155">You can view hello status and hello details for this request.</span></span> <span data-ttu-id="76bd8-156">Klikněte na tlačítko **+ novou zprávu** Pokud si chcete toofollow na tento požadavek.</span><span class="sxs-lookup"><span data-stu-id="76bd8-156">Click **+ New message** if you want toofollow up on this request.</span></span>

    ![Spravovat žádosti o podporu](./media/storsimple-8000-contact-microsoft-support/managesupport3.png)

## <a name="start-a-support-session-in-windows-powershell-for-storsimple"></a><span data-ttu-id="76bd8-158">Spusťte relaci podpory v prostředí Windows PowerShell pro StorSimple</span><span class="sxs-lookup"><span data-stu-id="76bd8-158">Start a support session in Windows PowerShell for StorSimple</span></span>

<span data-ttu-id="76bd8-159">tootroubleshoot všechny problémy, které setkat s hello zařízení StorSimple, budete potřebovat tooengage s týmem Microsoft Support hello.</span><span class="sxs-lookup"><span data-stu-id="76bd8-159">tootroubleshoot any issues that you might experience with hello StorSimple device, you will need tooengage with hello Microsoft Support team.</span></span> <span data-ttu-id="76bd8-160">Microsoft Support může být nutné toouse toolog relace podporu na tooyour zařízení.</span><span class="sxs-lookup"><span data-stu-id="76bd8-160">Microsoft Support may need toouse a support session toolog on tooyour device.</span></span>

<span data-ttu-id="76bd8-161">Proveďte následující hello kroky toostart relaci podpory:</span><span class="sxs-lookup"><span data-stu-id="76bd8-161">Perform hello following steps toostart a support session:</span></span>

#### <a name="toostart-a-support-session"></a><span data-ttu-id="76bd8-162">toostart relaci podpory</span><span class="sxs-lookup"><span data-stu-id="76bd8-162">toostart a support session</span></span>

1. <span data-ttu-id="76bd8-163">Přístup hello zařízení přímo pomocí konzoly sériového portu hello nebo prostřednictvím relace Telnetu ze vzdáleného počítače.</span><span class="sxs-lookup"><span data-stu-id="76bd8-163">Access hello device directly by using hello serial console or through a telnet session from a remote computer.</span></span> <span data-ttu-id="76bd8-164">toodo tento, postupujte podle kroků hello v [konzoly sériového portu toohello zařízení tooconnect pro použití klienta PuTTY](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="76bd8-164">toodo this, follow hello steps in [Use PuTTY tooconnect toohello device serial console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span></span>
2. <span data-ttu-id="76bd8-165">V relaci hello, které se otevře, stiskněte klávesu hello **Enter** klíče tooget příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="76bd8-165">In hello session that opens, press hello **Enter** key tooget a command prompt.</span></span>
3. <span data-ttu-id="76bd8-166">V nabídce konzoly sériového portu hello, vyberte možnost 1, **přihlásit úplný přístup**.</span><span class="sxs-lookup"><span data-stu-id="76bd8-166">In hello serial console menu, select option 1, **Log in with full access**.</span></span>
4. <span data-ttu-id="76bd8-167">Hello řádku zadejte následující heslo hello:</span><span class="sxs-lookup"><span data-stu-id="76bd8-167">At hello prompt, type hello following password:</span></span>
   
    `Password1`
5. <span data-ttu-id="76bd8-168">Hello řádku zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="76bd8-168">At hello prompt, type hello following command:</span></span>
   
    `Enable-HcsSupportAccess`
6. <span data-ttu-id="76bd8-169">Jako zašifrovaný řetězec zobrazí tooyou.</span><span class="sxs-lookup"><span data-stu-id="76bd8-169">An encrypted string will be presented tooyou.</span></span> <span data-ttu-id="76bd8-170">Zkopírujte tento řetězec do textového editoru, například Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="76bd8-170">Copy this string into a text editor such as Notepad.</span></span>
7. <span data-ttu-id="76bd8-171">Uložte tento řetězec a poslat ho e-mailové zprávy tooMicrosoft podpory.</span><span class="sxs-lookup"><span data-stu-id="76bd8-171">Save this string and send it in an email message tooMicrosoft Support.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="76bd8-172">Podpora přístupu můžete vypnout spuštěním `Disable-HcsSupportAccess`.</span><span class="sxs-lookup"><span data-stu-id="76bd8-172">You can disable support access by running `Disable-HcsSupportAccess`.</span></span> <span data-ttu-id="76bd8-173">zařízení StorSimple Hello pokusí toodisable podporu přístup také 8 hodin po hello relace byl inicializován.</span><span class="sxs-lookup"><span data-stu-id="76bd8-173">hello StorSimple device will also attempt toodisable support access 8 hours after hello session was initiated.</span></span> <span data-ttu-id="76bd8-174">Je nejlepší postup toochange zařízení StorSimple pověření po inicializaci relaci podpory.</span><span class="sxs-lookup"><span data-stu-id="76bd8-174">It is a best practice toochange your StorSimple device credentials after initiating a support session.</span></span>


## <a name="next-steps"></a><span data-ttu-id="76bd8-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="76bd8-175">Next steps</span></span>

<span data-ttu-id="76bd8-176">Zjistěte, jak příliš[diagnostikovat a vyřešit problémy související tooyour StorSimple 8000 řady zařízení](storsimple-troubleshoot-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="76bd8-176">Learn how too[diagnose and solve problems related tooyour StorSimple 8000 series device](storsimple-troubleshoot-deployment.md)</span></span>
