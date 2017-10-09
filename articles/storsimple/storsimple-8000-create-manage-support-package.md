---
title: "balíček pro podporu řady StorSimple 8000 aaaCreate | Microsoft Docs"
description: "Zjistěte, jak toocreate, dešifrování a upravit balíčku pro podporu pro vaše zařízení řady StorSimple 8000."
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
ms.date: 07/05/2017
ms.author: alkohli
ms.openlocfilehash: 857555b6ba31b1527f8f00d19818ebbec6005d0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-a-support-package-for-storsimple-8000-series"></a><span data-ttu-id="b6966-103">Vytvoření a Správa balíčku pro podporu pro řady StorSimple 8000</span><span class="sxs-lookup"><span data-stu-id="b6966-103">Create and manage a support package for StorSimple 8000 series</span></span>

## <a name="overview"></a><span data-ttu-id="b6966-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="b6966-104">Overview</span></span>

<span data-ttu-id="b6966-105">Balíček pro podporu zařízení StorSimple je mechanismus snadné použití, který shromažďuje všechny relevantní protokoly tooassist Microsoft Support vyřešit všechny problémy zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="b6966-105">A StorSimple support package is an easy-to-use mechanism that collects all relevant logs tooassist Microsoft Support with troubleshooting any StorSimple device issues.</span></span> <span data-ttu-id="b6966-106">Hello shromažďovat protokoly jsou zašifrované a komprimované.</span><span class="sxs-lookup"><span data-stu-id="b6966-106">hello collected logs are encrypted and compressed.</span></span>

<span data-ttu-id="b6966-107">Tento kurz zahrnuje toocreate podrobné pokyny a spravovat hello balíček pro podporu pro vaše zařízení řady StorSimple 8000.</span><span class="sxs-lookup"><span data-stu-id="b6966-107">This tutorial includes step-by-step instructions toocreate and manage hello support package for your StorSimple 8000 series device.</span></span> <span data-ttu-id="b6966-108">Pokud pracujete s polem virtuální zařízení StorSimple, přejděte příliš[generovat balíček protokolu](storsimple-ova-web-ui-admin.md#generate-a-log-package).</span><span class="sxs-lookup"><span data-stu-id="b6966-108">If you are working with a StorSimple Virtual Array, go too[generate a log package](storsimple-ova-web-ui-admin.md#generate-a-log-package).</span></span>

## <a name="create-a-support-package"></a><span data-ttu-id="b6966-109">Vytvoření balíčku pro podporu</span><span class="sxs-lookup"><span data-stu-id="b6966-109">Create a support package</span></span>

<span data-ttu-id="b6966-110">V některých případech budete potřebovat toomanually vytvořit balíček podporu hello prostřednictvím Windows Powershellu pro StorSimple.</span><span class="sxs-lookup"><span data-stu-id="b6966-110">In some cases, you'll need toomanually create hello support package through Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="b6966-111">Například:</span><span class="sxs-lookup"><span data-stu-id="b6966-111">For example:</span></span>

* <span data-ttu-id="b6966-112">Pokud potřebujete tooremove citlivé informace z protokolu souborů předchozí toosharing s Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="b6966-112">If you need tooremove sensitive information from your log files prior toosharing with Microsoft Support.</span></span>
* <span data-ttu-id="b6966-113">Pokud máte potíže s odeslání balíčku hello kvůli tooconnectivity problémy.</span><span class="sxs-lookup"><span data-stu-id="b6966-113">If you are having difficulty uploading hello package due tooconnectivity issues.</span></span>

<span data-ttu-id="b6966-114">Váš balíček ručně generovaného podpory můžete sdílet s Microsoft Support prostřednictvím e-mailu.</span><span class="sxs-lookup"><span data-stu-id="b6966-114">You can share your manually generated support package with Microsoft Support over email.</span></span> <span data-ttu-id="b6966-115">Proveďte následující kroky toocreate balíčku pro podporu ve Windows Powershellu pro StorSimple hello.</span><span class="sxs-lookup"><span data-stu-id="b6966-115">Perform hello following steps toocreate a support package in Windows PowerShell for StorSimple.</span></span>

#### <a name="toocreate-a-support-package-in-windows-powershell-for-storsimple"></a><span data-ttu-id="b6966-116">toocreate balíčku pro podporu ve Windows Powershellu pro StorSimple</span><span class="sxs-lookup"><span data-stu-id="b6966-116">toocreate a support package in Windows PowerShell for StorSimple</span></span>

1. <span data-ttu-id="b6966-117">toostart relaci prostředí Windows PowerShell s oprávněními správce na hello vzdálený počítač, který byl použit zařízení StorSimple tooyour tooconnect, zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="b6966-117">toostart a Windows PowerShell session as an administrator on hello remote computer that's used tooconnect tooyour StorSimple device, enter hello following command:</span></span>
   
    `Start PowerShell`
2. <span data-ttu-id="b6966-118">V relaci prostředí Windows PowerShell text hello připojte toohello SSAdmin konzoly vašeho zařízení:</span><span class="sxs-lookup"><span data-stu-id="b6966-118">In hello Windows PowerShell session, connect toohello SSAdmin Console of your device:</span></span>
   
   1. <span data-ttu-id="b6966-119">Hello příkazového řádku zadejte:</span><span class="sxs-lookup"><span data-stu-id="b6966-119">At hello command prompt, enter:</span></span>
     
       `$MS = New-PSSession -ComputerName <IP address for DATA 0> -Credential SSAdmin -ConfigurationName "SSAdminConsole"`
   2. <span data-ttu-id="b6966-120">V poli hello dialogové okno, které se otevře zadejte heslo správce zařízení.</span><span class="sxs-lookup"><span data-stu-id="b6966-120">In hello dialog box that opens, enter your device administrator password.</span></span> <span data-ttu-id="b6966-121">výchozí heslo Hello je _Heslo1_.</span><span class="sxs-lookup"><span data-stu-id="b6966-121">hello default password is _Password1_.</span></span>
     
      ![Dialogové okno přihlašovacích údajů prostředí PowerShell](./media/storsimple-8000-create-manage-support-package/IC740962.png)
   3. <span data-ttu-id="b6966-123">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="b6966-123">Select **OK**.</span></span>
   4. <span data-ttu-id="b6966-124">Hello příkazového řádku zadejte:</span><span class="sxs-lookup"><span data-stu-id="b6966-124">At hello command prompt, enter:</span></span>
     
      `Enter-PSSession $MS`
3. <span data-ttu-id="b6966-125">V relaci hello, které se otevře zadejte příslušný příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="b6966-125">In hello session that opens, enter hello appropriate command.</span></span>
   
   * <span data-ttu-id="b6966-126">Pro sdílené síťové složky, které jsou chráněné heslem zadejte:</span><span class="sxs-lookup"><span data-stu-id="b6966-126">For network shares that are password protected, enter:</span></span>
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" –Credential "Username" -Force`
     
       <span data-ttu-id="b6966-127">(Protože balíček pro podporu hello je zašifrován) budete vyzváni k zadání hesla, cesta toohello sdílené síťové složky a šifrovací přístupové heslo.</span><span class="sxs-lookup"><span data-stu-id="b6966-127">You'll be prompted for a password, a path toohello network shared folder, and an encryption passphrase (because hello support package is encrypted).</span></span> <span data-ttu-id="b6966-128">Balíček pro podporu je poté jste vytvořili v zadané složce hello.</span><span class="sxs-lookup"><span data-stu-id="b6966-128">A support package is then created in hello specified folder.</span></span>
   * <span data-ttu-id="b6966-129">Pro sdílené složky, které nejsou chráněné heslem, není nutné hello `-Credential` parametr.</span><span class="sxs-lookup"><span data-stu-id="b6966-129">For shares that are not password protected, you do not need hello `-Credential` parameter.</span></span> <span data-ttu-id="b6966-130">Zadejte hello následující:</span><span class="sxs-lookup"><span data-stu-id="b6966-130">Enter hello following:</span></span>
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" -Force`
     
       <span data-ttu-id="b6966-131">balíček pro podporu Hello se vytvoří pro oba řadiče v hello zadané síťové sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="b6966-131">hello support package is created for both controllers in hello specified network shared folder.</span></span> <span data-ttu-id="b6966-132">Je soubor zašifrované, komprimované, který může odeslat tooMicrosoft podporu pro řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="b6966-132">It's an encrypted, compressed file that can be sent tooMicrosoft Support for troubleshooting.</span></span> <span data-ttu-id="b6966-133">Další informace najdete v tématu [obraťte se na podporu společnosti Microsoft](storsimple-8000-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="b6966-133">For more information, see [Contact Microsoft Support](storsimple-8000-contact-microsoft-support.md).</span></span>

### <a name="hello-export-hcssupportpackage-cmdlet-parameters"></a><span data-ttu-id="b6966-134">Parametry rutiny Export-HcsSupportPackage Hello</span><span class="sxs-lookup"><span data-stu-id="b6966-134">hello Export-HcsSupportPackage cmdlet parameters</span></span>

<span data-ttu-id="b6966-135">Můžete použít následující parametry pomocí rutiny Export-HcsSupportPackage hello hello.</span><span class="sxs-lookup"><span data-stu-id="b6966-135">You can use hello following parameters with hello Export-HcsSupportPackage cmdlet.</span></span>

| <span data-ttu-id="b6966-136">Parametr</span><span class="sxs-lookup"><span data-stu-id="b6966-136">Parameter</span></span> | <span data-ttu-id="b6966-137">Požadované a volitelné</span><span class="sxs-lookup"><span data-stu-id="b6966-137">Required/Optional</span></span> | <span data-ttu-id="b6966-138">Popis</span><span class="sxs-lookup"><span data-stu-id="b6966-138">Description</span></span> |
| --- | --- | --- |
| `-Path` |<span data-ttu-id="b6966-139">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b6966-139">Required</span></span> |<span data-ttu-id="b6966-140">Použijte umístění hello tooprovide hello síťové sdílené složky, ve které hello je umístěn balíček pro podporu.</span><span class="sxs-lookup"><span data-stu-id="b6966-140">Use tooprovide hello location of hello network shared folder in which hello support package is placed.</span></span> |
| `-EncryptionPassphrase` |<span data-ttu-id="b6966-141">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="b6966-141">Required</span></span> |<span data-ttu-id="b6966-142">Použití tooprovide toohelp přístupové heslo šifrování balíček pro podporu hello.</span><span class="sxs-lookup"><span data-stu-id="b6966-142">Use tooprovide a passphrase toohelp encrypt hello support package.</span></span> |
| `-Credential` |<span data-ttu-id="b6966-143">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="b6966-143">Optional</span></span> |<span data-ttu-id="b6966-144">Použijte přihlašovací údaje toosupply přístupu pro sdílenou síťovou složku hello.</span><span class="sxs-lookup"><span data-stu-id="b6966-144">Use toosupply access credentials for hello network shared folder.</span></span> |
| `-Force` |<span data-ttu-id="b6966-145">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="b6966-145">Optional</span></span> |<span data-ttu-id="b6966-146">Pomocí tooskip hello šifrovací přístupové heslo potvrzení kroku.</span><span class="sxs-lookup"><span data-stu-id="b6966-146">Use tooskip hello encryption passphrase confirmation step.</span></span> |
| `-PackageTag` |<span data-ttu-id="b6966-147">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="b6966-147">Optional</span></span> |<span data-ttu-id="b6966-148">Použít toospecify adresář v *cesta* ve které podporu hello je umístěn balíčku.</span><span class="sxs-lookup"><span data-stu-id="b6966-148">Use toospecify a directory under *Path* in which hello support package is placed.</span></span> <span data-ttu-id="b6966-149">Hello výchozí hodnota je [název]-[aktuální datum a time:yyyy-MM-dd-HH-mm-ss].</span><span class="sxs-lookup"><span data-stu-id="b6966-149">hello default is [device name]-[current date and time:yyyy-MM-dd-HH-mm-ss].</span></span> |
| `-Scope` |<span data-ttu-id="b6966-150">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="b6966-150">Optional</span></span> |<span data-ttu-id="b6966-151">Zadejte jako **clusteru** toocreate (výchozí) balíčku pro podporu pro oba řadiče.</span><span class="sxs-lookup"><span data-stu-id="b6966-151">Specify as **Cluster** (default) toocreate a support package for both controllers.</span></span> <span data-ttu-id="b6966-152">Pokud chcete pouze pro aktuální řadič hello toocreate balíčku, zadejte **řadič**.</span><span class="sxs-lookup"><span data-stu-id="b6966-152">If you want toocreate a package only for hello current controller, specify **Controller**.</span></span> |

## <a name="edit-a-support-package"></a><span data-ttu-id="b6966-153">Upravte balíček podpory</span><span class="sxs-lookup"><span data-stu-id="b6966-153">Edit a support package</span></span>

<span data-ttu-id="b6966-154">Po vygenerování balíčku pro podporu, bude pravděpodobně nutné tooedit hello balíček tooremove citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="b6966-154">After you have generated a support package, you might need tooedit hello package tooremove sensitive information.</span></span> <span data-ttu-id="b6966-155">To může zahrnovat názvů svazků, zařízení IP adresy a názvy zálohování ze souborů protokolů hello.</span><span class="sxs-lookup"><span data-stu-id="b6966-155">This can include volume names, device IP addresses, and backup names from hello log files.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b6966-156">Můžete upravit pouze podporu balíček, který byl vytvořen pomocí Windows Powershellu pro StorSimple.</span><span class="sxs-lookup"><span data-stu-id="b6966-156">You can only edit a support package that was generated through Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="b6966-157">Nelze upravit balíček vytvořen v hello portálu Azure pomocí služby StorSimple Manager zařízení.</span><span class="sxs-lookup"><span data-stu-id="b6966-157">You can't edit a package created in hello Azure portal with StorSimple Device Manager service.</span></span>

<span data-ttu-id="b6966-158">tooedit balíčku pro podporu před nahráním na webu Microsoft Support hello, nejprve dešifrovat balíček pro podporu hello, upravit hello soubory a pak ho znovu zašifrovat.</span><span class="sxs-lookup"><span data-stu-id="b6966-158">tooedit a support package before uploading it on hello Microsoft Support site, first decrypt hello support package, edit hello files, and then re-encrypt it.</span></span> <span data-ttu-id="b6966-159">Proveďte následující kroky hello.</span><span class="sxs-lookup"><span data-stu-id="b6966-159">Perform hello following steps.</span></span>

#### <a name="tooedit-a-support-package-in-windows-powershell-for-storsimple"></a><span data-ttu-id="b6966-160">tooedit balíčku pro podporu ve Windows Powershellu pro StorSimple</span><span class="sxs-lookup"><span data-stu-id="b6966-160">tooedit a support package in Windows PowerShell for StorSimple</span></span>

1. <span data-ttu-id="b6966-161">Generovat balíček podporu jak je popsáno dříve, v [toocreate balíčku pro podporu ve Windows Powershellu pro StorSimple](#to-create-a-support-package-in-windows-powershell-for-storsimple).</span><span class="sxs-lookup"><span data-stu-id="b6966-161">Generate a support package as described earlier, in [toocreate a support package in Windows PowerShell for StorSimple](#to-create-a-support-package-in-windows-powershell-for-storsimple).</span></span>
2. <span data-ttu-id="b6966-162">[Stáhnout skript hello](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) místně na vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="b6966-162">[Download hello script](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) locally on your client.</span></span>
3. <span data-ttu-id="b6966-163">Importujte modul prostředí Windows PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="b6966-163">Import hello Windows PowerShell module.</span></span> <span data-ttu-id="b6966-164">Zadejte hello cesta toohello místní složku, ve které jste stáhli hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="b6966-164">Specify hello path toohello local folder in which you downloaded hello script.</span></span> <span data-ttu-id="b6966-165">modul hello tooimport, zadejte:</span><span class="sxs-lookup"><span data-stu-id="b6966-165">tooimport hello module, enter:</span></span>
   
    `Import-module <Path toohello folder that contains hello Windows PowerShell script>`
4. <span data-ttu-id="b6966-166">Všechny soubory hello *.aes* soubory, které jsou komprimovaná a šifrovaná.</span><span class="sxs-lookup"><span data-stu-id="b6966-166">All hello files are *.aes* files that are compressed and encrypted.</span></span> <span data-ttu-id="b6966-167">toodecompress a dešifrování souborů, zadejte:</span><span class="sxs-lookup"><span data-stu-id="b6966-167">toodecompress and decrypt files, enter:</span></span>
   
    `Open-HcsSupportPackage <Path toohello folder that contains support package files>`
   
    <span data-ttu-id="b6966-168">Všimněte si, že hello skutečný soubor rozšíření se nyní zobrazují pro všechny soubory hello.</span><span class="sxs-lookup"><span data-stu-id="b6966-168">Note that hello actual file extensions are now displayed for all hello files.</span></span>
   
    ![Upravte balíček podpory](./media/storsimple-8000-create-manage-support-package/IC750706.png)
5. <span data-ttu-id="b6966-170">Když se zobrazí výzva k hello šifrovací přístupové heslo, zadejte přístupové heslo hello, který jste použili při vytvoření balíčku pro podporu hello.</span><span class="sxs-lookup"><span data-stu-id="b6966-170">When you're prompted for hello encryption passphrase, enter hello passphrase that you used when hello support package was created.</span></span>
   
        cmdlet Open-HcsSupportPackage at command pipeline position 1
   
        Supply values for hello following parameters:EncryptionPassphrase: ****
6. <span data-ttu-id="b6966-171">Vyhledejte složku toohello, který obsahuje soubory protokolu hello.</span><span class="sxs-lookup"><span data-stu-id="b6966-171">Browse toohello folder that contains hello log files.</span></span> <span data-ttu-id="b6966-172">Protože soubory protokolu hello jsou nyní dekomprimovat a dešifrovat, to bude mít původní přípony souborů.</span><span class="sxs-lookup"><span data-stu-id="b6966-172">Because hello log files are now decompressed and decrypted, these will have original file extensions.</span></span> <span data-ttu-id="b6966-173">Upravte tyto soubory tooremove žádné informace o zákazníkovi například názvy svazku a IP adresy zařízení a ukládat soubory hello.</span><span class="sxs-lookup"><span data-stu-id="b6966-173">Modify these files tooremove any customer-specific information, such as volume names and device IP addresses, and save hello files.</span></span>
7. <span data-ttu-id="b6966-174">Zavřít hello soubory toocompress je s gzip šifrovat pomocí standardu AES 256.</span><span class="sxs-lookup"><span data-stu-id="b6966-174">Close hello files toocompress them with gzip and encrypt them with AES-256.</span></span> <span data-ttu-id="b6966-175">Toto je pro rychlost a zabezpečení v balíčku pro podporu hello přenosu přes síť.</span><span class="sxs-lookup"><span data-stu-id="b6966-175">This is for speed and security in transferring hello support package over a network.</span></span> <span data-ttu-id="b6966-176">toocompress a šifrování souborů, zadejte následující hello:</span><span class="sxs-lookup"><span data-stu-id="b6966-176">toocompress and encrypt files, enter hello following:</span></span>
   
    `Close-HcsSupportPackage <Path toohello folder that contains support package files>`
   
    ![Upravte balíček podpory](./media/storsimple-8000-create-manage-support-package/IC750707.png)
8. <span data-ttu-id="b6966-178">Po zobrazení výzvy zadejte šifrovací přístupové heslo hello upravené podporu balíčku.</span><span class="sxs-lookup"><span data-stu-id="b6966-178">When prompted, provide an encryption passphrase for hello modified support package.</span></span>
   
        cmdlet Close-HcsSupportPackage at command pipeline position 1
        Supply values for hello following parameters:EncryptionPassphrase: ****
9. <span data-ttu-id="b6966-179">Poznamenejte si nové heslo hello, takže ho můžete sdílet s Microsoft Support vyžádání.</span><span class="sxs-lookup"><span data-stu-id="b6966-179">Write down hello new passphrase, so that you can share it with Microsoft Support when requested.</span></span>

### <a name="example-editing-files-in-a-support-package-on-a-password-protected-share"></a><span data-ttu-id="b6966-180">Příklad: Úprava souborů v balíčku pro podporu ve sdílené složce chráněné heslem</span><span class="sxs-lookup"><span data-stu-id="b6966-180">Example: Editing files in a support package on a password-protected share</span></span>

<span data-ttu-id="b6966-181">Hello následující příklad ukazuje, jak toodecrypt, upravit a znovu zašifrovat balíčku pro podporu.</span><span class="sxs-lookup"><span data-stu-id="b6966-181">hello following example shows how toodecrypt, edit, and re-encrypt a support package.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="b6966-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b6966-182">Next steps</span></span>

* <span data-ttu-id="b6966-183">Další informace o hello [informace shromážděny v balíčku pro podporu hello](https://support.microsoft.com/help/3193606/storsimple-support-packages-and-device-logs)</span><span class="sxs-lookup"><span data-stu-id="b6966-183">Learn about hello [information collected in hello Support package](https://support.microsoft.com/help/3193606/storsimple-support-packages-and-device-logs)</span></span>
* <span data-ttu-id="b6966-184">Zjistěte, jak příliš[použití podpůrných balíčků a zařízení v protokolech tootroubleshoot nasazením zařízení](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="b6966-184">Learn how too[use support packages and device logs tootroubleshoot your device deployment](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting).</span></span>
* <span data-ttu-id="b6966-185">Zjistěte, jak příliš[použití hello tooadminister service Manager zařízení StorSimple zařízení StorSimple](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="b6966-185">Learn how too[use hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

