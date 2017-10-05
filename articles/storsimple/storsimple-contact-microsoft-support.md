---
title: "Přihlaste se lístek podpory pro řady StorSimple 8000 | Microsoft Docs"
description: "Naučte se vytvořit žádost o podporu a spustit relaci podpory zařízení StorSimple."
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
ms.openlocfilehash: cecc2566b432e897b5eff0c12e66598f0518ed80
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="contact-microsoft-support-for-your-storsimple"></a><span data-ttu-id="509b9-103">Obraťte se na podporu společnosti Microsoft pro vaše zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="509b9-103">Contact Microsoft Support for your StorSimple</span></span>
<span data-ttu-id="509b9-104">Pokud narazíte na potíže s vaším řešením Microsoft Azure StorSimple, můžete vytvořit žádost o službu pro technickou podporu.</span><span class="sxs-lookup"><span data-stu-id="509b9-104">If you encounter any issues with your Microsoft Azure StorSimple solution, you can create a service request for technical support.</span></span> <span data-ttu-id="509b9-105">V relaci online s svého specialistu technické podpory může také muset spustit relaci podpory zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="509b9-105">In an online session with your support engineer, you may also need to start a support session on your StorSimple device.</span></span> <span data-ttu-id="509b9-106">Tento článek vás provede procesem:</span><span class="sxs-lookup"><span data-stu-id="509b9-106">This article walks you through:</span></span>

* <span data-ttu-id="509b9-107">Postup vytvoření žádosti o podporu.</span><span class="sxs-lookup"><span data-stu-id="509b9-107">How to create a support request.</span></span>
* <span data-ttu-id="509b9-108">Jak spustit relaci podpora v rozhraní Windows PowerShell zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="509b9-108">How to start a support session in the Windows PowerShell interface of your StorSimple device.</span></span>

<span data-ttu-id="509b9-109">Zkontrolujte [StorSimple 8000 řady podporu SLA a informace o](https://msdn.microsoft.com/library/mt433077.aspx) předtím, než vytvoříte žádost o podporu.</span><span class="sxs-lookup"><span data-stu-id="509b9-109">Review the [StorSimple 8000 Series Support SLAs and information](https://msdn.microsoft.com/library/mt433077.aspx) before you create a Support request.</span></span>

## <a name="create-a-support-request"></a><span data-ttu-id="509b9-110">Vytvořit žádost o podporu</span><span class="sxs-lookup"><span data-stu-id="509b9-110">Create a support request</span></span>
<span data-ttu-id="509b9-111">Proveďte následující kroky k vytvoření žádosti o podporu:</span><span class="sxs-lookup"><span data-stu-id="509b9-111">Perform the following steps to create a support request:</span></span>

#### <a name="to-create-a-support-request"></a><span data-ttu-id="509b9-112">Chcete-li vytvořit žádost o podporu</span><span class="sxs-lookup"><span data-stu-id="509b9-112">To create a support request</span></span>
1. <span data-ttu-id="509b9-113">V [portál Azure classic](https://manage.windowsazure.com/), v pravém horním rohu klikněte na název účtu a pak klikněte na tlačítko **obraťte se na podporu společnosti Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="509b9-113">In the [Azure classic portal](https://manage.windowsazure.com/), in the upper right corner, click your account name and then click **Contact Microsoft Support**.</span></span>
   
    ![MS obraťte se na podporu přes ManagementPortal](./media/storsimple-contact-microsoft-support/Ibiza1.png)
2. <span data-ttu-id="509b9-115">Budete přesměrováni na nový portál Azure (portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="509b9-115">You will be redirected to the new Azure portal (portal.azure.com).</span></span> <span data-ttu-id="509b9-116">Klikněte **nová žádost o podporu** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="509b9-116">Click the **New support request** tile.</span></span>
   
    ![MS obraťte se na podporu prostřednictvím nového portálu](./media/storsimple-contact-microsoft-support/Ibiza2.png)
   
    <span data-ttu-id="509b9-118">Na pravé straně obrazovky **nová žádost o podporu** podokně se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="509b9-118">On the right side of the screen, the **New support request** pane appears.</span></span> 
   
    ![Nové podokno žádosti o podporu](./media/storsimple-contact-microsoft-support/Ibiza3a.png)
3. <span data-ttu-id="509b9-120">V **Základy** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="509b9-120">In the **Basics** dialog box, complete the following:</span></span>                                
   
   1. <span data-ttu-id="509b9-121">Z **vydat typ** rozevíracího seznamu vyberte **technické**.</span><span class="sxs-lookup"><span data-stu-id="509b9-121">From the **Issue type** drop-down list , select **Technical**.</span></span>
   2. <span data-ttu-id="509b9-122">Vyberte **předplatné** z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="509b9-122">Select a **Subscription** from the drop-down list.</span></span>
   3. <span data-ttu-id="509b9-123">Z **služby** rozevíracího seznamu vyberte **StorSimple**.</span><span class="sxs-lookup"><span data-stu-id="509b9-123">From the **Service** drop-down list, select **StorSimple**.</span></span> 
   4. <span data-ttu-id="509b9-124">Vyberte **plán podpory** z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="509b9-124">Select a **Support plan** from the drop-down list.</span></span> <span data-ttu-id="509b9-125">Je nutné placený plán podpory povolit technická podpora.</span><span class="sxs-lookup"><span data-stu-id="509b9-125">You need a paid support plan to enable Technical Support.</span></span>
4. <span data-ttu-id="509b9-126">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="509b9-126">Click **Next**.</span></span> <span data-ttu-id="509b9-127">**Problém** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="509b9-127">The **Problem** dialog box appears.</span></span>
   
    ![Nové podokno žádosti o podporu](./media/storsimple-contact-microsoft-support/Ibiza5a.png) 
5. <span data-ttu-id="509b9-129">V **problém** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="509b9-129">In the **Problem** dialog box, complete the following:</span></span>
   
   1. <span data-ttu-id="509b9-130">Vyberte **závažnost** úrovně z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="509b9-130">Select a **Severity** level from the drop-down list.</span></span>
   2. <span data-ttu-id="509b9-131">Vyberte **typ problému** z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="509b9-131">Select a **Problem type** from the drop-down list.</span></span>
   3. <span data-ttu-id="509b9-132">Vyberte **kategorie** z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="509b9-132">Select a **Category** from the drop-down list.</span></span> 
   4. <span data-ttu-id="509b9-133">V **podrobnosti** pole Krátce popište svůj problém.</span><span class="sxs-lookup"><span data-stu-id="509b9-133">In the **Details** box, briefly describe your issue.</span></span>
   5. <span data-ttu-id="509b9-134">V **časového rámce** pole, zda datum, čas a časové pásmo, které odpovídá nejaktuálnějšího výskytu vašeho problému.</span><span class="sxs-lookup"><span data-stu-id="509b9-134">In the **Time frame** box, indicate the date, time, and time zone that corresponds to the most recent occurrence of your issue.</span></span>
   6. <span data-ttu-id="509b9-135">V části **nahrávání souborů**, klikněte na ikonu složky a přejděte do balíčku pro podporu.</span><span class="sxs-lookup"><span data-stu-id="509b9-135">Under **File upload**, click the folder icon to browse to your support package.</span></span>
   7. <span data-ttu-id="509b9-136">Vyberte **sdílet diagnostické informace** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="509b9-136">Select the **Share diagnostic information** check box.</span></span>
6. <span data-ttu-id="509b9-137">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="509b9-137">Click **Next**.</span></span> <span data-ttu-id="509b9-138">**Kontaktní informace** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="509b9-138">The **Contact information** dialog box appears.</span></span>
   
    ![Nové podokno žádosti o podporu](./media/storsimple-contact-microsoft-support/Ibiza6a.png) 
7. <span data-ttu-id="509b9-140">Zadejte své kontaktní informace a vyberte způsob kontaktu (telefon nebo e-mail).</span><span class="sxs-lookup"><span data-stu-id="509b9-140">Enter your contact information and select a contact method (phone or email).</span></span> 
8. <span data-ttu-id="509b9-141">Vyberte **uložit změny kontaktů pro budoucí požadavky na podporu** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="509b9-141">Select the **Save contact changes for future support requests** check box.</span></span>
9. <span data-ttu-id="509b9-142">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="509b9-142">Click **Create**.</span></span>

<span data-ttu-id="509b9-143">Po odeslání vaši žádost o vás bude kontaktovat pracovníka podpory vám co nejdříve pokračovat k žádosti.</span><span class="sxs-lookup"><span data-stu-id="509b9-143">After you have submitted your request, a Support engineer will contact you as soon as possible to proceed with your request.</span></span>

## <a name="start-a-support-session-in-windows-powershell-for-storsimple"></a><span data-ttu-id="509b9-144">Spusťte relaci podpory v prostředí Windows PowerShell pro StorSimple</span><span class="sxs-lookup"><span data-stu-id="509b9-144">Start a support session in Windows PowerShell for StorSimple</span></span>
<span data-ttu-id="509b9-145">Chcete-li vyřešit potíže, které se můžete setkat s zařízení StorSimple, musíte použít s týmem Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="509b9-145">To troubleshoot any issues that you might experience with the StorSimple device, you will need to engage with the Microsoft Support team.</span></span> <span data-ttu-id="509b9-146">Microsoft Support možná muset použít relaci podporu pro přihlášení k zařízení.</span><span class="sxs-lookup"><span data-stu-id="509b9-146">Microsoft Support may need to use a support session to log on to your device.</span></span> 

<span data-ttu-id="509b9-147">Proveďte následující kroky pro spuštění relace podpory:</span><span class="sxs-lookup"><span data-stu-id="509b9-147">Perform the following steps to start a support session:</span></span>

#### <a name="to-start-a-support-session"></a><span data-ttu-id="509b9-148">Chcete-li spustit relaci podpory</span><span class="sxs-lookup"><span data-stu-id="509b9-148">To start a support session</span></span>
1. <span data-ttu-id="509b9-149">Přístup k zařízení přímo pomocí konzoly sériového portu nebo prostřednictvím relace Telnetu ze vzdáleného počítače.</span><span class="sxs-lookup"><span data-stu-id="509b9-149">Access the device directly by using the serial console or through a telnet session from a remote computer.</span></span> <span data-ttu-id="509b9-150">Chcete-li to provést, postupujte podle kroků v [použití klienta PuTTY k připojení ke konzole sériového portu zařízení](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="509b9-150">To do this, follow the steps in [Use PuTTY to connect to the device serial console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span></span>
2. <span data-ttu-id="509b9-151">V této relaci, které se otevře, stiskněte **Enter** klíč k získání příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="509b9-151">In the session that opens, press the **Enter** key to get a command prompt.</span></span>
3. <span data-ttu-id="509b9-152">V nabídce konzoly sériového portu, vyberte možnost 1, **přihlásit úplný přístup**.</span><span class="sxs-lookup"><span data-stu-id="509b9-152">In the serial console menu, select option 1, **Log in with full access**.</span></span>
4. <span data-ttu-id="509b9-153">Do příkazového řádku zadejte následující heslo:</span><span class="sxs-lookup"><span data-stu-id="509b9-153">At the prompt, type the following password:</span></span> 
   
    `Password1`
5. <span data-ttu-id="509b9-154">Do příkazového řádku zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="509b9-154">At the prompt, type the following command:</span></span>
   
    `Enable-HcsSupportAccess`
6. <span data-ttu-id="509b9-155">Zobrazí se vám zašifrovaný řetězec.</span><span class="sxs-lookup"><span data-stu-id="509b9-155">An encrypted string will be presented to you.</span></span> <span data-ttu-id="509b9-156">Zkopírujte tento řetězec do textového editoru, například Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="509b9-156">Copy this string into a text editor such as Notepad.</span></span>
7. <span data-ttu-id="509b9-157">Uložte tento řetězec a odeslání v e-mailovou zprávu na Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="509b9-157">Save this string and send it in an email message to Microsoft Support.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="509b9-158">Podpora přístupu můžete vypnout spuštěním `Disable-HcsSupportAccess`.</span><span class="sxs-lookup"><span data-stu-id="509b9-158">You can disable support access by running `Disable-HcsSupportAccess`.</span></span> <span data-ttu-id="509b9-159">Zařízení StorSimple se také pokusí zakázat přístup podporu 8 hodin po relace byl inicializován.</span><span class="sxs-lookup"><span data-stu-id="509b9-159">The StorSimple device will also attempt to disable support access 8 hours after the session was initiated.</span></span> <span data-ttu-id="509b9-160">Je osvědčeným postupem změnit svoje přihlašovací údaje zařízení StorSimple po inicializaci relaci podpory.</span><span class="sxs-lookup"><span data-stu-id="509b9-160">It is a best practice to change your StorSimple device credentials after initiating a support session.</span></span>
> 
> 

