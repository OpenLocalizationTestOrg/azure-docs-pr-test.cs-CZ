---
title: "Vytvoření lístku podpory nebo případ řady StorSimple 8000 | Microsoft Docs"
description: "Informace o protokolu žádosti o podporu a spustit relaci podporu ve vašem zařízení řady StorSimple 8000."
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
ms.openlocfilehash: 4b5a14237ce79100f980b2186b2c3c887abaa296
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="contact-microsoft-support"></a><span data-ttu-id="8e999-103">Obraťte se na podporu společnosti Microsoft</span><span class="sxs-lookup"><span data-stu-id="8e999-103">Contact Microsoft Support</span></span>

<span data-ttu-id="8e999-104">Správce zařízení StorSimple poskytuje schopnost **protokolu nové žádosti o podporu** v okně Souhrn služby.</span><span class="sxs-lookup"><span data-stu-id="8e999-104">The StorSimple Device Manager provides the capability to **log a new support request** within the service summary blade.</span></span> <span data-ttu-id="8e999-105">Pokud narazíte na potíže s vašeho řešení StorSimple, můžete vytvořit žádost o službu pro technickou podporu.</span><span class="sxs-lookup"><span data-stu-id="8e999-105">If you encounter any issues with your StorSimple solution, you can create a service request for technical support.</span></span> <span data-ttu-id="8e999-106">V relaci online s svého specialistu technické podpory může také muset spustit relaci podpory zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="8e999-106">In an online session with your support engineer, you may also need to start a support session on your StorSimple device.</span></span> <span data-ttu-id="8e999-107">Tento článek vás provede procesem:</span><span class="sxs-lookup"><span data-stu-id="8e999-107">This article walks you through:</span></span>

* <span data-ttu-id="8e999-108">Postup vytvoření žádosti o podporu.</span><span class="sxs-lookup"><span data-stu-id="8e999-108">How to create a support request.</span></span>
* <span data-ttu-id="8e999-109">Jak spravovat cyklus podpory žádost z portálu.</span><span class="sxs-lookup"><span data-stu-id="8e999-109">How to manage a support request lifecycle from within the portal.</span></span>
* <span data-ttu-id="8e999-110">Jak spustit relaci podpora v rozhraní Windows PowerShell zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="8e999-110">How to start a support session in the Windows PowerShell interface of your StorSimple device.</span></span>

<span data-ttu-id="8e999-111">Zkontrolujte [StorSimple 8000 řady podporu SLA a informace o](https://msdn.microsoft.com/library/mt433077.aspx) předtím, než vytvoříte žádost o podporu.</span><span class="sxs-lookup"><span data-stu-id="8e999-111">Review the [StorSimple 8000 Series Support SLAs and information](https://msdn.microsoft.com/library/mt433077.aspx) before you create a Support request.</span></span>

## <a name="create-a-support-request"></a><span data-ttu-id="8e999-112">Vytvořit žádost o podporu</span><span class="sxs-lookup"><span data-stu-id="8e999-112">Create a support request</span></span>

<span data-ttu-id="8e999-113">V závislosti na vaší [plán podpory](https://azure.microsoft.com/support/plans/), můžete vytvořit lístky žádostí o podporu pro problém na zařízení StorSimple přímo z okna souhrnu service Manager zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="8e999-113">Depending upon your [support plan](https://azure.microsoft.com/support/plans/), you can create support tickets for an issue on your StorSimple device directly from the StorSimple Device Manager service summary blade.</span></span> <span data-ttu-id="8e999-114">Proveďte následující kroky k vytvoření žádosti o podporu:</span><span class="sxs-lookup"><span data-stu-id="8e999-114">Perform the following steps to create a support request:</span></span>

#### <a name="to-create-a-support-request"></a><span data-ttu-id="8e999-115">Chcete-li vytvořit žádost o podporu</span><span class="sxs-lookup"><span data-stu-id="8e999-115">To create a support request</span></span>

1. <span data-ttu-id="8e999-116">Přejděte do služby Správce zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="8e999-116">Go to your StorSimple Device Manager service.</span></span> <span data-ttu-id="8e999-117">V okně Souhrn nastavení služby, přejděte na **podporu + Poradce při potížích s** části a pak klikněte na **nová žádost o podporu**.</span><span class="sxs-lookup"><span data-stu-id="8e999-117">In the service summary blade settings, go to **SUPPORT + TROUBLESHOOTING** section and then click **New support request**.</span></span>
     
    ![MS obraťte se na podporu prostřednictvím nového portálu](./media/storsimple-8000-contact-microsoft-support/contactsupport1.png)
   
2. <span data-ttu-id="8e999-119">V **nová žádost o podporu** vyberte **Základy**.</span><span class="sxs-lookup"><span data-stu-id="8e999-119">In the **New support request** blade, select **Basics**.</span></span> <span data-ttu-id="8e999-120">V **Základy** okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8e999-120">In the **Basics** blade, do the following steps:</span></span>
   1. <span data-ttu-id="8e999-121">Z **vydat typ** rozevíracího seznamu vyberte **technické**.</span><span class="sxs-lookup"><span data-stu-id="8e999-121">From the **Issue type** drop-down list , select **Technical**.</span></span>
   2. <span data-ttu-id="8e999-122">Aktuální **předplatné**, **služby** typ a **prostředků** (zařízení služby StorSimple Manager) se automaticky vybraná.</span><span class="sxs-lookup"><span data-stu-id="8e999-122">The current **Subscription**, **Service** type, and the **Resource** (StorSimple Device Manager service) are automatically chosen.</span></span> 
   3. <span data-ttu-id="8e999-123">Vyberte **plán podpory** z rozevíracího seznamu, pokud máte více schématy spojené s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="8e999-123">Select a **Support plan** from the drop-down list if you have multiple plans associated with your subscription.</span></span> <span data-ttu-id="8e999-124">Je nutné placený plán podpory povolit technická podpora.</span><span class="sxs-lookup"><span data-stu-id="8e999-124">You need a paid support plan to enable Technical Support.</span></span>
   4. <span data-ttu-id="8e999-125">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="8e999-125">Click **Next**.</span></span>

       ![MS obraťte se na podporu prostřednictvím nového portálu](./media/storsimple-8000-contact-microsoft-support/contactsupport2.png)

3. <span data-ttu-id="8e999-127">V **nová žádost o podporu** vyberte **krok 2 problém**.</span><span class="sxs-lookup"><span data-stu-id="8e999-127">In the **New support request** blade, select **Step 2 Problem**.</span></span> <span data-ttu-id="8e999-128">V **problém** okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8e999-128">In the **Problem** blade, do the following steps:</span></span>
    
    1. <span data-ttu-id="8e999-129">Vyberte **závažnost**.</span><span class="sxs-lookup"><span data-stu-id="8e999-129">Choose the **Severity**.</span></span>
    2. <span data-ttu-id="8e999-130">Zadejte, jestli tento problém se týká zařízení nebo službu StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="8e999-130">Specify if the issue is related to the appliance or the StorSimple Device Manager service.</span></span>
    3. <span data-ttu-id="8e999-131">Vyberte **kategorie** pro toto vydání a zadejte více **podrobnosti** o problému.</span><span class="sxs-lookup"><span data-stu-id="8e999-131">Choose a **Category** for this issue and provide more **Details** about the issue.</span></span>
    4. <span data-ttu-id="8e999-132">Zadejte počáteční datum a čas pro problém.</span><span class="sxs-lookup"><span data-stu-id="8e999-132">Provide the start date and time for the problem.</span></span>
    5. <span data-ttu-id="8e999-133">V **nahrávání souborů**, klikněte na ikonu složky a přejděte do balíčku pro podporu.</span><span class="sxs-lookup"><span data-stu-id="8e999-133">In the **File upload**, click the folder icon to browse to your support package.</span></span>
    6. <span data-ttu-id="8e999-134">Zkontrolujte **sdílet diagnostické informace**.</span><span class="sxs-lookup"><span data-stu-id="8e999-134">Check **Share diagnostic information**.</span></span>
    7. <span data-ttu-id="8e999-135">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="8e999-135">Click **Next**.</span></span>

       ![MS obraťte se na podporu prostřednictvím nového portálu](./media/storsimple-8000-contact-microsoft-support/contactsupport3.png) 

4. <span data-ttu-id="8e999-137">V **nová žádost o podporu** okně klikněte na tlačítko **krok 3 kontaktní údaje**.</span><span class="sxs-lookup"><span data-stu-id="8e999-137">In the **New support request** blade, click **Step 3 Contact information**.</span></span> <span data-ttu-id="8e999-138">V **kontaktní informace** okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8e999-138">In the **Contact information** blade, do the following steps:</span></span>

    1. <span data-ttu-id="8e999-139">V **obraťte se na možnosti**, poskytovat upřednostňovaný způsob kontaktování (telefon nebo e-mail) a jazyka.</span><span class="sxs-lookup"><span data-stu-id="8e999-139">In the **Contact options**, provide your preferred contact method (phone or email) and the language.</span></span> <span data-ttu-id="8e999-140">Doba odezvy se vybere automaticky podle plánu předplatného.</span><span class="sxs-lookup"><span data-stu-id="8e999-140">The response time is automatically selected based on your subscription plan.</span></span>
    2. <span data-ttu-id="8e999-141">V kontaktní údaje zadejte název, e-mailu, obraťte se na volitelné, země.</span><span class="sxs-lookup"><span data-stu-id="8e999-141">In the Contact information, provide your name, email, optional contact, country.</span></span> <span data-ttu-id="8e999-142">Vyberte **uložit změny kontaktů pro budoucí požadavky na podporu** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="8e999-142">Select the **Save contact changes for future support requests** check box.</span></span>
    3. <span data-ttu-id="8e999-143">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8e999-143">Click **Create**.</span></span>
   
        ![MS obraťte se na podporu prostřednictvím nového portálu](./media/storsimple-8000-contact-microsoft-support/contactsupport5.png)   

    <span data-ttu-id="8e999-145">Microsoft Support bude tyto informace používat k oslovení vám další informace, diagnostiku a řešení.</span><span class="sxs-lookup"><span data-stu-id="8e999-145">Microsoft Support will use this information to reach out to you for further information, diagnosis, and resolution.</span></span>
<span data-ttu-id="8e999-146">Po odeslání vaši žádost o vás bude kontaktovat pracovníka podpory vám co nejdříve pokračovat k žádosti.</span><span class="sxs-lookup"><span data-stu-id="8e999-146">After you have submitted your request, a Support engineer will contact you as soon as possible to proceed with your request.</span></span>

## <a name="manage-a-support-request"></a><span data-ttu-id="8e999-147">Spravovat žádosti o podporu</span><span class="sxs-lookup"><span data-stu-id="8e999-147">Manage a support request</span></span>

<span data-ttu-id="8e999-148">Po vytvoření lístku můžete spravovat lístek v celém jeho životním cyklu přímo z portálu.</span><span class="sxs-lookup"><span data-stu-id="8e999-148">After creating a support ticket, you can manage the lifecycle of the ticket from within the portal.</span></span>

#### <a name="to-manage-your-support-requests"></a><span data-ttu-id="8e999-149">Ke správě žádostí o podporu</span><span class="sxs-lookup"><span data-stu-id="8e999-149">To manage your support requests</span></span>

1. <span data-ttu-id="8e999-150">Chcete-li se na stránku nápovědy a podpory, přejděte na **procházet > Nápověda a podpora**.</span><span class="sxs-lookup"><span data-stu-id="8e999-150">To get to the help and support page, navigate to **Browse > Help + support**.</span></span>

    ![Spravovat žádosti o podporu](./media/storsimple-8000-contact-microsoft-support/managesupport1.png)

2. <span data-ttu-id="8e999-152">Tabulkový seznam všechny žádosti o podporu se zobrazí v **Nápověda a podpora** okno.</span><span class="sxs-lookup"><span data-stu-id="8e999-152">A tabular listing of All the support requests is displayed in the **Help + support** blade.</span></span>

    ![Spravovat žádosti o podporu](./media/storsimple-8000-contact-microsoft-support/managesupport2.png)

3. <span data-ttu-id="8e999-154">Vyberte a klikněte na žádost o podporu.</span><span class="sxs-lookup"><span data-stu-id="8e999-154">Select and click a support request.</span></span> <span data-ttu-id="8e999-155">Můžete zobrazit stav a podrobnosti o této žádosti.</span><span class="sxs-lookup"><span data-stu-id="8e999-155">You can view the status and the details for this request.</span></span> <span data-ttu-id="8e999-156">Klikněte na tlačítko **+ novou zprávu** Pokud chcete ke zpracování v této žádosti.</span><span class="sxs-lookup"><span data-stu-id="8e999-156">Click **+ New message** if you want to follow up on this request.</span></span>

    ![Spravovat žádosti o podporu](./media/storsimple-8000-contact-microsoft-support/managesupport3.png)

## <a name="start-a-support-session-in-windows-powershell-for-storsimple"></a><span data-ttu-id="8e999-158">Spusťte relaci podpory v prostředí Windows PowerShell pro StorSimple</span><span class="sxs-lookup"><span data-stu-id="8e999-158">Start a support session in Windows PowerShell for StorSimple</span></span>

<span data-ttu-id="8e999-159">Chcete-li vyřešit potíže, které se můžete setkat s zařízení StorSimple, musíte použít s týmem Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="8e999-159">To troubleshoot any issues that you might experience with the StorSimple device, you will need to engage with the Microsoft Support team.</span></span> <span data-ttu-id="8e999-160">Microsoft Support možná muset použít relaci podporu pro přihlášení k zařízení.</span><span class="sxs-lookup"><span data-stu-id="8e999-160">Microsoft Support may need to use a support session to log on to your device.</span></span>

<span data-ttu-id="8e999-161">Proveďte následující kroky pro spuštění relace podpory:</span><span class="sxs-lookup"><span data-stu-id="8e999-161">Perform the following steps to start a support session:</span></span>

#### <a name="to-start-a-support-session"></a><span data-ttu-id="8e999-162">Chcete-li spustit relaci podpory</span><span class="sxs-lookup"><span data-stu-id="8e999-162">To start a support session</span></span>

1. <span data-ttu-id="8e999-163">Přístup k zařízení přímo pomocí konzoly sériového portu nebo prostřednictvím relace Telnetu ze vzdáleného počítače.</span><span class="sxs-lookup"><span data-stu-id="8e999-163">Access the device directly by using the serial console or through a telnet session from a remote computer.</span></span> <span data-ttu-id="8e999-164">Chcete-li to provést, postupujte podle kroků v [použití klienta PuTTY k připojení ke konzole sériového portu zařízení](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="8e999-164">To do this, follow the steps in [Use PuTTY to connect to the device serial console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span></span>
2. <span data-ttu-id="8e999-165">V této relaci, které se otevře, stiskněte **Enter** klíč k získání příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="8e999-165">In the session that opens, press the **Enter** key to get a command prompt.</span></span>
3. <span data-ttu-id="8e999-166">V nabídce konzoly sériového portu, vyberte možnost 1, **přihlásit úplný přístup**.</span><span class="sxs-lookup"><span data-stu-id="8e999-166">In the serial console menu, select option 1, **Log in with full access**.</span></span>
4. <span data-ttu-id="8e999-167">Do příkazového řádku zadejte následující heslo:</span><span class="sxs-lookup"><span data-stu-id="8e999-167">At the prompt, type the following password:</span></span>
   
    `Password1`
5. <span data-ttu-id="8e999-168">Do příkazového řádku zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8e999-168">At the prompt, type the following command:</span></span>
   
    `Enable-HcsSupportAccess`
6. <span data-ttu-id="8e999-169">Zobrazí se vám zašifrovaný řetězec.</span><span class="sxs-lookup"><span data-stu-id="8e999-169">An encrypted string will be presented to you.</span></span> <span data-ttu-id="8e999-170">Zkopírujte tento řetězec do textového editoru, například Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="8e999-170">Copy this string into a text editor such as Notepad.</span></span>
7. <span data-ttu-id="8e999-171">Uložte tento řetězec a odeslání v e-mailovou zprávu na Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="8e999-171">Save this string and send it in an email message to Microsoft Support.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8e999-172">Podpora přístupu můžete vypnout spuštěním `Disable-HcsSupportAccess`.</span><span class="sxs-lookup"><span data-stu-id="8e999-172">You can disable support access by running `Disable-HcsSupportAccess`.</span></span> <span data-ttu-id="8e999-173">Zařízení StorSimple se také pokusí zakázat přístup podporu 8 hodin po relace byl inicializován.</span><span class="sxs-lookup"><span data-stu-id="8e999-173">The StorSimple device will also attempt to disable support access 8 hours after the session was initiated.</span></span> <span data-ttu-id="8e999-174">Je osvědčeným postupem změnit svoje přihlašovací údaje zařízení StorSimple po inicializaci relaci podpory.</span><span class="sxs-lookup"><span data-stu-id="8e999-174">It is a best practice to change your StorSimple device credentials after initiating a support session.</span></span>


## <a name="next-steps"></a><span data-ttu-id="8e999-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8e999-175">Next steps</span></span>

<span data-ttu-id="8e999-176">Zjistěte, jak [diagnostikovat a vyřešit problémy související s zařízení řady StorSimple 8000](storsimple-troubleshoot-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="8e999-176">Learn how to [diagnose and solve problems related to your StorSimple 8000 series device](storsimple-troubleshoot-deployment.md)</span></span>
