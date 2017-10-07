---
title: "aaaCreate balíčku pro podporu StorSimple | Microsoft Docs"
description: "Zjistěte, jak toocreate, dešifrování a upravit balíčku pro podporu pro zařízení StorSimple."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: eac76f5f-5db1-4c92-af8c-54053b91e66c
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 209aeee50e823fd2ca96ababd1d0cf3ea9cdad53
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-a-storsimple-support-package"></a><span data-ttu-id="7a129-103">Vytvoření a Správa balíčku pro podporu zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="7a129-103">Create and manage a StorSimple support package</span></span>
## <a name="overview"></a><span data-ttu-id="7a129-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="7a129-104">Overview</span></span>
<span data-ttu-id="7a129-105">Balíček pro podporu zařízení StorSimple je mechanismus snadné použití, který shromažďuje všechny relevantní protokoly tooassist Microsoft Support vyřešit všechny problémy zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="7a129-105">A StorSimple support package is an easy-to-use mechanism that collects all relevant logs tooassist Microsoft Support with troubleshooting any StorSimple device issues.</span></span> <span data-ttu-id="7a129-106">Hello shromažďovat protokoly jsou zašifrované a komprimované.</span><span class="sxs-lookup"><span data-stu-id="7a129-106">hello collected logs are encrypted and compressed.</span></span>

<span data-ttu-id="7a129-107">Tento kurz zahrnuje toocreate podrobné pokyny a spravovat balíček pro podporu hello.</span><span class="sxs-lookup"><span data-stu-id="7a129-107">This tutorial includes step-by-step instructions toocreate and manage hello support package.</span></span>

## <a name="create-and-upload-a-support-package-in-hello-azure-classic-portal"></a><span data-ttu-id="7a129-108">Vytvoření a odeslání balíčku pro podporu v hello portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="7a129-108">Create and upload a support package in hello Azure classic portal</span></span>
<span data-ttu-id="7a129-109">Můžete vytvořit a nahrát lokalitě podporu balíček toohello Microsoft Support prostřednictvím hello **údržby** stránku hello služby v hello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="7a129-109">You can create and upload a support package toohello Microsoft Support site through hello **Maintenance** page of hello service in hello Azure classic portal.</span></span>

> [!NOTE]
> <span data-ttu-id="7a129-110">nahrávání Hello vyžaduje podporu klíč.</span><span class="sxs-lookup"><span data-stu-id="7a129-110">hello upload requires a support passkey.</span></span> <span data-ttu-id="7a129-111">Pracovníka podpory by měl poskytovat tento tooyou e-mailem.</span><span class="sxs-lookup"><span data-stu-id="7a129-111">Your support engineer should provide this tooyou in an email.</span></span>
> 
> 

<span data-ttu-id="7a129-112">Podpoře šifrované a komprimované balíčku (.cab) je vytvořený a nahrané toohello podporu lokality.</span><span class="sxs-lookup"><span data-stu-id="7a129-112">An encrypted and compressed support package (.cab file) is created and uploaded toohello Support site.</span></span> <span data-ttu-id="7a129-113">pracovník podpory Hello pak můžete načíst tento balíček z lokality hello podporu pro řešení potíží s hello.</span><span class="sxs-lookup"><span data-stu-id="7a129-113">hello support engineer can then retrieve this package from hello Support site for troubleshooting hello issue.</span></span>

<span data-ttu-id="7a129-114">Proveďte následující kroky v hello klasického portálu toocreate balíčku pro podporu hello.</span><span class="sxs-lookup"><span data-stu-id="7a129-114">Perform hello following steps in hello classic portal toocreate a support package.</span></span>

#### <a name="toocreate-a-support-package-in-hello-azure-classic-portal"></a><span data-ttu-id="7a129-115">toocreate balíčku pro podporu v hello portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="7a129-115">toocreate a support package in hello Azure classic portal</span></span>
1. <span data-ttu-id="7a129-116">Vyberte **zařízení** > **údržby**.</span><span class="sxs-lookup"><span data-stu-id="7a129-116">Select **Devices** > **Maintenance**.</span></span>
2. <span data-ttu-id="7a129-117">V hello **balíček pro podporu** vyberte **vytvoření a nahrání balíčku pro podporu**.</span><span class="sxs-lookup"><span data-stu-id="7a129-117">In hello **Support package** section, select **Create and upload support package**.</span></span>
3. <span data-ttu-id="7a129-118">V hello **vytvoření a nahrání balíčku pro podporu** dialogové okno pole, hello následující:</span><span class="sxs-lookup"><span data-stu-id="7a129-118">In hello **Create and upload support package** dialog box, do hello following:</span></span>
   
    ![Vytvoření balíčku pro podporu](./media/storsimple-create-manage-support-package/IC740923.png)
   
   * <span data-ttu-id="7a129-120">V hello **přístupový klíč podporu** text zadejte hello přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="7a129-120">In hello **Support Passkey** text box, enter hello passkey.</span></span> <span data-ttu-id="7a129-121">Pracovníka podpory společnosti Microsoft, by měli poslat tooyou tento klíč v e-mailu.</span><span class="sxs-lookup"><span data-stu-id="7a129-121">Your Microsoft support engineer should send this passkey tooyou in email.</span></span>
   * <span data-ttu-id="7a129-122">Vyberte hello políčko tooprovide souhlasu tooautomatically nahrávání hello podporu balíček toohello Microsoft Support lokalitu.</span><span class="sxs-lookup"><span data-stu-id="7a129-122">Select hello check box tooprovide consent tooautomatically upload hello support package toohello Microsoft Support site.</span></span>
   * <span data-ttu-id="7a129-123">Klikněte na ikonu zaškrtnutí hello</span><span class="sxs-lookup"><span data-stu-id="7a129-123">Click hello check icon</span></span> ![Ikona zaškrtnutí](./media/storsimple-create-manage-support-package/IC740895.png)<span data-ttu-id="7a129-125">.</span><span class="sxs-lookup"><span data-stu-id="7a129-125">.</span></span>

## <a name="manually-create-a-support-package"></a><span data-ttu-id="7a129-126">Ruční vytvoření balíčku pro podporu</span><span class="sxs-lookup"><span data-stu-id="7a129-126">Manually create a support package</span></span>
<span data-ttu-id="7a129-127">V některých případech budete potřebovat toomanually vytvořit balíček podporu hello prostřednictvím Windows Powershellu pro StorSimple.</span><span class="sxs-lookup"><span data-stu-id="7a129-127">In some cases, you'll need toomanually create hello support package through Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="7a129-128">Například:</span><span class="sxs-lookup"><span data-stu-id="7a129-128">For example:</span></span>

* <span data-ttu-id="7a129-129">Pokud potřebujete tooremove citlivé informace z protokolu souborů předchozí toosharing s Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="7a129-129">If you need tooremove sensitive information from your log files prior toosharing with Microsoft Support.</span></span>
* <span data-ttu-id="7a129-130">Pokud máte potíže s odeslání balíčku hello kvůli tooconnectivity problémy.</span><span class="sxs-lookup"><span data-stu-id="7a129-130">If you are having difficulty uploading hello package due tooconnectivity issues.</span></span>

<span data-ttu-id="7a129-131">Váš balíček ručně generovaného podpory můžete sdílet s Microsoft Support prostřednictvím e-mailu.</span><span class="sxs-lookup"><span data-stu-id="7a129-131">You can share your manually generated support package with Microsoft Support over email.</span></span> <span data-ttu-id="7a129-132">Proveďte následující kroky toocreate balíčku pro podporu ve Windows Powershellu pro StorSimple hello.</span><span class="sxs-lookup"><span data-stu-id="7a129-132">Perform hello following steps toocreate a support package in Windows PowerShell for StorSimple.</span></span>

#### <a name="toocreate-a-support-package-in-windows-powershell-for-storsimple"></a><span data-ttu-id="7a129-133">toocreate balíčku pro podporu ve Windows Powershellu pro StorSimple</span><span class="sxs-lookup"><span data-stu-id="7a129-133">toocreate a support package in Windows PowerShell for StorSimple</span></span>
1. <span data-ttu-id="7a129-134">toostart relaci prostředí Windows PowerShell s oprávněními správce na hello vzdálený počítač, který byl použit zařízení StorSimple tooyour tooconnect, zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="7a129-134">toostart a Windows PowerShell session as an administrator on hello remote computer that's used tooconnect tooyour StorSimple device, enter hello following command:</span></span>
   
    `Start PowerShell`
2. <span data-ttu-id="7a129-135">V relaci prostředí Windows PowerShell text hello připojte toohello SSAdmin konzoly vašeho zařízení:</span><span class="sxs-lookup"><span data-stu-id="7a129-135">In hello Windows PowerShell session, connect toohello SSAdmin Console of your device:</span></span>
   
   * <span data-ttu-id="7a129-136">Hello příkazového řádku zadejte:</span><span class="sxs-lookup"><span data-stu-id="7a129-136">At hello command prompt, enter:</span></span>
     
       `$MS = New-PSSession -ComputerName <IP address for DATA 0> -Credential SSAdmin -ConfigurationName "SSAdminConsole"`
   * <span data-ttu-id="7a129-137">V poli hello dialogové okno, které se otevře zadejte heslo správce zařízení.</span><span class="sxs-lookup"><span data-stu-id="7a129-137">In hello dialog box that opens, enter your device administrator password.</span></span> <span data-ttu-id="7a129-138">výchozí heslo Hello je:</span><span class="sxs-lookup"><span data-stu-id="7a129-138">hello default password is:</span></span>
     
      `Password1`
     
      ![Dialogové okno přihlašovacích údajů prostředí PowerShell](./media/storsimple-create-manage-support-package/IC740962.png)
   * <span data-ttu-id="7a129-140">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="7a129-140">Select **OK**.</span></span>
   * <span data-ttu-id="7a129-141">Hello příkazového řádku zadejte:</span><span class="sxs-lookup"><span data-stu-id="7a129-141">At hello command prompt, enter:</span></span>
     
      `Enter-PSSession $MS`
3. <span data-ttu-id="7a129-142">V relaci hello, které se otevře zadejte příslušný příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="7a129-142">In hello session that opens, enter hello appropriate command.</span></span>
   
   * <span data-ttu-id="7a129-143">Pro sdílené síťové složky, které jsou chráněné heslem zadejte:</span><span class="sxs-lookup"><span data-stu-id="7a129-143">For network shares that are password protected, enter:</span></span>
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" –Credential "Username" -Force`
     
       <span data-ttu-id="7a129-144">(Protože balíček pro podporu hello je zašifrován) budete vyzváni k zadání hesla, cesta toohello sdílené síťové složky a šifrovací přístupové heslo.</span><span class="sxs-lookup"><span data-stu-id="7a129-144">You'll be prompted for a password, a path toohello network shared folder, and an encryption passphrase (because hello support package is encrypted).</span></span> <span data-ttu-id="7a129-145">Balíček pro podporu je poté jste vytvořili v zadané složce hello.</span><span class="sxs-lookup"><span data-stu-id="7a129-145">A support package is then created in hello specified folder.</span></span>
   * <span data-ttu-id="7a129-146">Pro sdílené složky, které nejsou chráněné heslem, není nutné hello `-Credential` parametr.</span><span class="sxs-lookup"><span data-stu-id="7a129-146">For shares that are not password protected, you do not need hello `-Credential` parameter.</span></span> <span data-ttu-id="7a129-147">Zadejte hello následující:</span><span class="sxs-lookup"><span data-stu-id="7a129-147">Enter hello following:</span></span>
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" -Force`
     
       <span data-ttu-id="7a129-148">balíček pro podporu Hello se vytvoří pro oba řadiče v hello zadané síťové sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="7a129-148">hello support package is created for both controllers in hello specified network shared folder.</span></span> <span data-ttu-id="7a129-149">Je soubor zašifrované, komprimované, který může odeslat tooMicrosoft podporu pro řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="7a129-149">It's an encrypted, compressed file that can be sent tooMicrosoft Support for troubleshooting.</span></span> <span data-ttu-id="7a129-150">Další informace najdete v tématu [obraťte se na podporu společnosti Microsoft](storsimple-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="7a129-150">For more information, see [Contact Microsoft Support](storsimple-contact-microsoft-support.md).</span></span>

### <a name="hello-export-hcssupportpackage-cmdlet-parameters"></a><span data-ttu-id="7a129-151">Parametry rutiny Export-HcsSupportPackage Hello</span><span class="sxs-lookup"><span data-stu-id="7a129-151">hello Export-HcsSupportPackage cmdlet parameters</span></span>
<span data-ttu-id="7a129-152">Můžete použít následující parametry pomocí rutiny Export-HcsSupportPackage hello hello.</span><span class="sxs-lookup"><span data-stu-id="7a129-152">You can use hello following parameters with hello Export-HcsSupportPackage cmdlet.</span></span>

| <span data-ttu-id="7a129-153">Parametr</span><span class="sxs-lookup"><span data-stu-id="7a129-153">Parameter</span></span> | <span data-ttu-id="7a129-154">Požadované a volitelné</span><span class="sxs-lookup"><span data-stu-id="7a129-154">Required/Optional</span></span> | <span data-ttu-id="7a129-155">Popis</span><span class="sxs-lookup"><span data-stu-id="7a129-155">Description</span></span> |
| --- | --- | --- |
| `-Path` |<span data-ttu-id="7a129-156">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="7a129-156">Required</span></span> |<span data-ttu-id="7a129-157">Použijte umístění hello tooprovide hello síťové sdílené složky, ve které hello je umístěn balíček pro podporu.</span><span class="sxs-lookup"><span data-stu-id="7a129-157">Use tooprovide hello location of hello network shared folder in which hello support package is placed.</span></span> |
| `-EncryptionPassphrase` |<span data-ttu-id="7a129-158">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="7a129-158">Required</span></span> |<span data-ttu-id="7a129-159">Použití tooprovide toohelp přístupové heslo šifrování balíček pro podporu hello.</span><span class="sxs-lookup"><span data-stu-id="7a129-159">Use tooprovide a passphrase toohelp encrypt hello support package.</span></span> |
| `-Credential` |<span data-ttu-id="7a129-160">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="7a129-160">Optional</span></span> |<span data-ttu-id="7a129-161">Použijte přihlašovací údaje toosupply přístupu pro sdílenou síťovou složku hello.</span><span class="sxs-lookup"><span data-stu-id="7a129-161">Use toosupply access credentials for hello network shared folder.</span></span> |
| `-Force` |<span data-ttu-id="7a129-162">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="7a129-162">Optional</span></span> |<span data-ttu-id="7a129-163">Pomocí tooskip hello šifrovací přístupové heslo potvrzení kroku.</span><span class="sxs-lookup"><span data-stu-id="7a129-163">Use tooskip hello encryption passphrase confirmation step.</span></span> |
| `-PackageTag` |<span data-ttu-id="7a129-164">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="7a129-164">Optional</span></span> |<span data-ttu-id="7a129-165">Použít toospecify adresář v *cesta* ve které podporu hello je umístěn balíčku.</span><span class="sxs-lookup"><span data-stu-id="7a129-165">Use toospecify a directory under *Path* in which hello support package is placed.</span></span> <span data-ttu-id="7a129-166">Hello výchozí hodnota je [název]-[aktuální datum a time:yyyy-MM-dd-HH-mm-ss].</span><span class="sxs-lookup"><span data-stu-id="7a129-166">hello default is [device name]-[current date and time:yyyy-MM-dd-HH-mm-ss].</span></span> |
| `-Scope` |<span data-ttu-id="7a129-167">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="7a129-167">Optional</span></span> |<span data-ttu-id="7a129-168">Zadejte jako **clusteru** toocreate (výchozí) balíčku pro podporu pro oba řadiče.</span><span class="sxs-lookup"><span data-stu-id="7a129-168">Specify as **Cluster** (default) toocreate a support package for both controllers.</span></span> <span data-ttu-id="7a129-169">Pokud chcete pouze pro aktuální řadič hello toocreate balíčku, zadejte **řadič**.</span><span class="sxs-lookup"><span data-stu-id="7a129-169">If you want toocreate a package only for hello current controller, specify **Controller**.</span></span> |

## <a name="edit-a-support-package"></a><span data-ttu-id="7a129-170">Upravte balíček podpory</span><span class="sxs-lookup"><span data-stu-id="7a129-170">Edit a support package</span></span>
<span data-ttu-id="7a129-171">Po vygenerování balíčku pro podporu, bude pravděpodobně nutné tooedit hello balíček tooremove citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="7a129-171">After you have generated a support package, you might need tooedit hello package tooremove sensitive information.</span></span> <span data-ttu-id="7a129-172">To může zahrnovat názvů svazků, zařízení IP adresy a názvy zálohování ze souborů protokolů hello.</span><span class="sxs-lookup"><span data-stu-id="7a129-172">This can include volume names, device IP addresses, and backup names from hello log files.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7a129-173">Můžete upravit pouze podporu balíček, který byl vytvořen pomocí Windows Powershellu pro StorSimple.</span><span class="sxs-lookup"><span data-stu-id="7a129-173">You can only edit a support package that was generated through Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="7a129-174">Nelze upravit balíček vytvořen v hello portál Azure classic pomocí služby StorSimple Manager.</span><span class="sxs-lookup"><span data-stu-id="7a129-174">You can't edit a package created in hello Azure classic portal with StorSimple Manager service.</span></span>
> 
> 

<span data-ttu-id="7a129-175">tooedit balíčku pro podporu před nahráním na webu Microsoft Support hello, nejprve dešifrovat balíček pro podporu hello, upravit hello soubory a pak ho znovu zašifrovat.</span><span class="sxs-lookup"><span data-stu-id="7a129-175">tooedit a support package before uploading it on hello Microsoft Support site, first decrypt hello support package, edit hello files, and then re-encrypt it.</span></span> <span data-ttu-id="7a129-176">Proveďte následující kroky hello.</span><span class="sxs-lookup"><span data-stu-id="7a129-176">Perform hello following steps.</span></span>

#### <a name="tooedit-a-support-package-in-windows-powershell-for-storsimple"></a><span data-ttu-id="7a129-177">tooedit balíčku pro podporu ve Windows Powershellu pro StorSimple</span><span class="sxs-lookup"><span data-stu-id="7a129-177">tooedit a support package in Windows PowerShell for StorSimple</span></span>
1. <span data-ttu-id="7a129-178">Generovat balíček podporu jak je popsáno dříve, v [toocreate balíčku pro podporu ve Windows Powershellu pro StorSimple](#to-create-a-support-package-in-windows-powershell-for-storsimple).</span><span class="sxs-lookup"><span data-stu-id="7a129-178">Generate a support package as described earlier, in [toocreate a support package in Windows PowerShell for StorSimple](#to-create-a-support-package-in-windows-powershell-for-storsimple).</span></span>
2. <span data-ttu-id="7a129-179">[Stáhnout skript hello](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) místně na vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="7a129-179">[Download hello script](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) locally on your client.</span></span>
3. <span data-ttu-id="7a129-180">Importujte modul prostředí Windows PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="7a129-180">Import hello Windows PowerShell module.</span></span> <span data-ttu-id="7a129-181">Zadejte hello cesta toohello místní složku, ve které jste stáhli hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="7a129-181">Specify hello path toohello local folder in which you downloaded hello script.</span></span> <span data-ttu-id="7a129-182">modul hello tooimport, zadejte:</span><span class="sxs-lookup"><span data-stu-id="7a129-182">tooimport hello module, enter:</span></span>
   
    `Import-module <Path toohello folder that contains hello Windows PowerShell script>`
4. <span data-ttu-id="7a129-183">Všechny soubory hello *.aes* soubory, které jsou komprimovaná a šifrovaná.</span><span class="sxs-lookup"><span data-stu-id="7a129-183">All hello files are *.aes* files that are compressed and encrypted.</span></span> <span data-ttu-id="7a129-184">toodecompress a dešifrování souborů, zadejte:</span><span class="sxs-lookup"><span data-stu-id="7a129-184">toodecompress and decrypt files, enter:</span></span>
   
    `Open-HcsSupportPackage <Path toohello folder that contains support package files>`
   
    <span data-ttu-id="7a129-185">Všimněte si, že hello skutečný soubor rozšíření se nyní zobrazují pro všechny soubory hello.</span><span class="sxs-lookup"><span data-stu-id="7a129-185">Note that hello actual file extensions are now displayed for all hello files.</span></span>
   
    ![Upravte balíček podpory](./media/storsimple-create-manage-support-package/IC750706.png)
5. <span data-ttu-id="7a129-187">Když se zobrazí výzva k hello šifrovací přístupové heslo, zadejte přístupové heslo hello, který jste použili při vytvoření balíčku pro podporu hello.</span><span class="sxs-lookup"><span data-stu-id="7a129-187">When you're prompted for hello encryption passphrase, enter hello passphrase that you used when hello support package was created.</span></span>
   
        cmdlet Open-HcsSupportPackage at command pipeline position 1
   
        Supply values for hello following parameters:EncryptionPassphrase: ****
6. <span data-ttu-id="7a129-188">Vyhledejte složku toohello, který obsahuje soubory protokolu hello.</span><span class="sxs-lookup"><span data-stu-id="7a129-188">Browse toohello folder that contains hello log files.</span></span> <span data-ttu-id="7a129-189">Protože soubory protokolu hello jsou nyní dekomprimovat a dešifrovat, to bude mít původní přípony souborů.</span><span class="sxs-lookup"><span data-stu-id="7a129-189">Because hello log files are now decompressed and decrypted, these will have original file extensions.</span></span> <span data-ttu-id="7a129-190">Upravte tyto soubory tooremove žádné informace o zákazníkovi například názvy svazku a IP adresy zařízení a ukládat soubory hello.</span><span class="sxs-lookup"><span data-stu-id="7a129-190">Modify these files tooremove any customer-specific information, such as volume names and device IP addresses, and save hello files.</span></span>
7. <span data-ttu-id="7a129-191">Zavřít hello soubory toocompress je s gzip šifrovat pomocí standardu AES 256.</span><span class="sxs-lookup"><span data-stu-id="7a129-191">Close hello files toocompress them with gzip and encrypt them with AES-256.</span></span> <span data-ttu-id="7a129-192">Toto je pro rychlost a zabezpečení v balíčku pro podporu hello přenosu přes síť.</span><span class="sxs-lookup"><span data-stu-id="7a129-192">This is for speed and security in transferring hello support package over a network.</span></span> <span data-ttu-id="7a129-193">toocompress a šifrování souborů, zadejte následující hello:</span><span class="sxs-lookup"><span data-stu-id="7a129-193">toocompress and encrypt files, enter hello following:</span></span>
   
    `Close-HcsSupportPackage <Path toohello folder that contains support package files>`
   
    ![Upravte balíček podpory](./media/storsimple-create-manage-support-package/IC750707.png)
8. <span data-ttu-id="7a129-195">Po zobrazení výzvy zadejte šifrovací přístupové heslo hello upravené podporu balíčku.</span><span class="sxs-lookup"><span data-stu-id="7a129-195">When prompted, provide an encryption passphrase for hello modified support package.</span></span>
   
        cmdlet Close-HcsSupportPackage at command pipeline position 1
        Supply values for hello following parameters:EncryptionPassphrase: ****
9. <span data-ttu-id="7a129-196">Poznamenejte si nové heslo hello, takže ho můžete sdílet s Microsoft Support vyžádání.</span><span class="sxs-lookup"><span data-stu-id="7a129-196">Write down hello new passphrase, so that you can share it with Microsoft Support when requested.</span></span>

### <a name="example-editing-files-in-a-support-package-on-a-password-protected-share"></a><span data-ttu-id="7a129-197">Příklad: Úprava souborů v balíčku pro podporu ve sdílené složce chráněné heslem</span><span class="sxs-lookup"><span data-stu-id="7a129-197">Example: Editing files in a support package on a password-protected share</span></span>
<span data-ttu-id="7a129-198">Hello následující příklad ukazuje, jak toodecrypt, upravit a znovu zašifrovat balíčku pro podporu.</span><span class="sxs-lookup"><span data-stu-id="7a129-198">hello following example shows how toodecrypt, edit, and re-encrypt a support package.</span></span>

        PS C:\WINDOWS\system32> Import-module C:\Users\Default\StorSimple\SupportPackage\HCSSupportPackageTools.psm1

        PS C:\WINDOWS\system32> Open-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Open-HcsSupportPackage at command pipeline position 1

        Supply values for hello following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32> Close-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Close-HcsSupportPackage at command pipeline position 1

        Supply values for hello following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32>

## <a name="next-steps"></a><span data-ttu-id="7a129-199">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7a129-199">Next steps</span></span>
* <span data-ttu-id="7a129-200">Zjistěte, jak příliš[použití podpůrných balíčků a zařízení v protokolech tootroubleshoot nasazením zařízení](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="7a129-200">Learn how too[use support packages and device logs tootroubleshoot your device deployment](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting).</span></span>
* <span data-ttu-id="7a129-201">Zjistěte, jak příliš[použití hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="7a129-201">Learn how too[use hello StorSimple Manager service tooadminister your StorSimple device](storsimple-manager-service-administration.md).</span></span>

