---
title: "Vytvoření balíčku pro podporu StorSimple | Microsoft Docs"
description: "Naučte se vytvářet, dešifrování a upravovat balíčku pro podporu pro zařízení StorSimple."
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
ms.openlocfilehash: 32d20e7a8adcfc646c592213fe7395b87a93c985
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-a-storsimple-support-package"></a><span data-ttu-id="ffe9f-103">Vytvoření a Správa balíčku pro podporu zařízení StorSimple</span><span class="sxs-lookup"><span data-stu-id="ffe9f-103">Create and manage a StorSimple support package</span></span>
## <a name="overview"></a><span data-ttu-id="ffe9f-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="ffe9f-104">Overview</span></span>
<span data-ttu-id="ffe9f-105">Balíček pro podporu zařízení StorSimple je mechanismus snadné použití, který shromažďuje všechny relevantní protokoly Microsoft Support pomáhající při řešení potíží všechny problémy zařízení StorSimple.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-105">A StorSimple support package is an easy-to-use mechanism that collects all relevant logs to assist Microsoft Support with troubleshooting any StorSimple device issues.</span></span> <span data-ttu-id="ffe9f-106">Shromážděné protokoly jsou zašifrované a komprimované.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-106">The collected logs are encrypted and compressed.</span></span>

<span data-ttu-id="ffe9f-107">Tento kurz obsahuje podrobné pokyny k vytváření a správě balíček pro podporu.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-107">This tutorial includes step-by-step instructions to create and manage the support package.</span></span>

## <a name="create-and-upload-a-support-package-in-the-azure-classic-portal"></a><span data-ttu-id="ffe9f-108">Vytvoření a odeslání balíčku pro podporu na portálu Azure classic</span><span class="sxs-lookup"><span data-stu-id="ffe9f-108">Create and upload a support package in the Azure classic portal</span></span>
<span data-ttu-id="ffe9f-109">Můžete vytvořit a odeslat na web Microsoft Support prostřednictvím balíčku pro podporu **údržby** stránka služby v portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-109">You can create and upload a support package to the Microsoft Support site through the **Maintenance** page of the service in the Azure classic portal.</span></span>

> [!NOTE]
> <span data-ttu-id="ffe9f-110">Nahrávání vyžaduje podporu klíč.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-110">The upload requires a support passkey.</span></span> <span data-ttu-id="ffe9f-111">Pracovníka podpory by měl poskytovat to pro vás e-mailem.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-111">Your support engineer should provide this to you in an email.</span></span>
> 
> 

<span data-ttu-id="ffe9f-112">Podpoře šifrované a komprimované balíčku (.cab) je vytvořen a odesláno na webu podpory.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-112">An encrypted and compressed support package (.cab file) is created and uploaded to the Support site.</span></span> <span data-ttu-id="ffe9f-113">Pracovník podpory pak můžete načíst tento balíček z webu podpory k řešení problému.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-113">The support engineer can then retrieve this package from the Support site for troubleshooting the issue.</span></span>

<span data-ttu-id="ffe9f-114">Proveďte následující kroky na portálu classic k vytvoření balíčku pro podporu.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-114">Perform the following steps in the classic portal to create a support package.</span></span>

#### <a name="to-create-a-support-package-in-the-azure-classic-portal"></a><span data-ttu-id="ffe9f-115">K vytvoření balíčku pro podporu na portálu Azure classic</span><span class="sxs-lookup"><span data-stu-id="ffe9f-115">To create a support package in the Azure classic portal</span></span>
1. <span data-ttu-id="ffe9f-116">Vyberte **zařízení** > **údržby**.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-116">Select **Devices** > **Maintenance**.</span></span>
2. <span data-ttu-id="ffe9f-117">V **balíček pro podporu** vyberte **vytvoření a nahrání balíčku pro podporu**.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-117">In the **Support package** section, select **Create and upload support package**.</span></span>
3. <span data-ttu-id="ffe9f-118">V **vytvoření a nahrání balíčku pro podporu** dialogové okno pole, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="ffe9f-118">In the **Create and upload support package** dialog box, do the following:</span></span>
   
    ![Vytvoření balíčku pro podporu](./media/storsimple-create-manage-support-package/IC740923.png)
   
   * <span data-ttu-id="ffe9f-120">V **podporu přístupový klíč** textové pole, zadejte klíč.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-120">In the **Support Passkey** text box, enter the passkey.</span></span> <span data-ttu-id="ffe9f-121">Pracovníka podpory společnosti Microsoft, by měli tento klíč poslat vám v e-mailu.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-121">Your Microsoft support engineer should send this passkey to you in email.</span></span>
   * <span data-ttu-id="ffe9f-122">Zaškrtněte políčko souhlasu zajistit automaticky odeslat balíček pro podporu na webu Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-122">Select the check box to provide consent to automatically upload the support package to the Microsoft Support site.</span></span>
   * <span data-ttu-id="ffe9f-123">Klikněte na ikonu zaškrtnutí</span><span class="sxs-lookup"><span data-stu-id="ffe9f-123">Click the check icon</span></span> ![Ikona zaškrtnutí](./media/storsimple-create-manage-support-package/IC740895.png)<span data-ttu-id="ffe9f-125">.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-125">.</span></span>

## <a name="manually-create-a-support-package"></a><span data-ttu-id="ffe9f-126">Ruční vytvoření balíčku pro podporu</span><span class="sxs-lookup"><span data-stu-id="ffe9f-126">Manually create a support package</span></span>
<span data-ttu-id="ffe9f-127">V některých případech musíte ručně vytvořit balíček pro podporu prostřednictvím Windows Powershellu pro StorSimple.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-127">In some cases, you'll need to manually create the support package through Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="ffe9f-128">Například:</span><span class="sxs-lookup"><span data-stu-id="ffe9f-128">For example:</span></span>

* <span data-ttu-id="ffe9f-129">Pokud je třeba odebrat citlivé informace ze souborů protokolu před sdílení s Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-129">If you need to remove sensitive information from your log files prior to sharing with Microsoft Support.</span></span>
* <span data-ttu-id="ffe9f-130">Pokud máte potíže s odeslání balíčku kvůli problémům s připojením.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-130">If you are having difficulty uploading the package due to connectivity issues.</span></span>

<span data-ttu-id="ffe9f-131">Váš balíček ručně generovaného podpory můžete sdílet s Microsoft Support prostřednictvím e-mailu.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-131">You can share your manually generated support package with Microsoft Support over email.</span></span> <span data-ttu-id="ffe9f-132">Proveďte následující kroky k vytvoření balíčku pro podporu ve Windows Powershellu pro StorSimple.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-132">Perform the following steps to create a support package in Windows PowerShell for StorSimple.</span></span>

#### <a name="to-create-a-support-package-in-windows-powershell-for-storsimple"></a><span data-ttu-id="ffe9f-133">Vytvoření balíčku pro podporu ve Windows Powershellu pro StorSimple</span><span class="sxs-lookup"><span data-stu-id="ffe9f-133">To create a support package in Windows PowerShell for StorSimple</span></span>
1. <span data-ttu-id="ffe9f-134">Chcete-li spustit relaci prostředí Windows PowerShell s oprávněními správce na vzdáleném počítači, který se používá k připojení k zařízení StorSimple, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ffe9f-134">To start a Windows PowerShell session as an administrator on the remote computer that's used to connect to your StorSimple device, enter the following command:</span></span>
   
    `Start PowerShell`
2. <span data-ttu-id="ffe9f-135">V relaci prostředí Windows PowerShell se připojte ke konzole SSAdmin vašeho zařízení:</span><span class="sxs-lookup"><span data-stu-id="ffe9f-135">In the Windows PowerShell session, connect to the SSAdmin Console of your device:</span></span>
   
   * <span data-ttu-id="ffe9f-136">Na příkazovém řádku zadejte:</span><span class="sxs-lookup"><span data-stu-id="ffe9f-136">At the command prompt, enter:</span></span>
     
       `$MS = New-PSSession -ComputerName <IP address for DATA 0> -Credential SSAdmin -ConfigurationName "SSAdminConsole"`
   * <span data-ttu-id="ffe9f-137">V dialogovém okně, které se otevře zadejte heslo správce zařízení.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-137">In the dialog box that opens, enter your device administrator password.</span></span> <span data-ttu-id="ffe9f-138">Výchozí heslo je:</span><span class="sxs-lookup"><span data-stu-id="ffe9f-138">The default password is:</span></span>
     
      `Password1`
     
      ![Dialogové okno přihlašovacích údajů prostředí PowerShell](./media/storsimple-create-manage-support-package/IC740962.png)
   * <span data-ttu-id="ffe9f-140">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-140">Select **OK**.</span></span>
   * <span data-ttu-id="ffe9f-141">Na příkazovém řádku zadejte:</span><span class="sxs-lookup"><span data-stu-id="ffe9f-141">At the command prompt, enter:</span></span>
     
      `Enter-PSSession $MS`
3. <span data-ttu-id="ffe9f-142">V této relaci, které se otevře zadejte příslušný příkaz.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-142">In the session that opens, enter the appropriate command.</span></span>
   
   * <span data-ttu-id="ffe9f-143">Pro sdílené síťové složky, které jsou chráněné heslem zadejte:</span><span class="sxs-lookup"><span data-stu-id="ffe9f-143">For network shares that are password protected, enter:</span></span>
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" –Credential "Username" -Force`
     
       <span data-ttu-id="ffe9f-144">(Protože balíček pro podporu je zašifrován) budete vyzváni k zadání hesla, cestu ke sdílené síťové složce a šifrovací přístupové heslo.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-144">You'll be prompted for a password, a path to the network shared folder, and an encryption passphrase (because the support package is encrypted).</span></span> <span data-ttu-id="ffe9f-145">Balíček pro podporu se pak vytvoří v zadané složce.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-145">A support package is then created in the specified folder.</span></span>
   * <span data-ttu-id="ffe9f-146">Pro sdílené složky, které nejsou chráněné heslem, není nutné `-Credential` parametr.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-146">For shares that are not password protected, you do not need the `-Credential` parameter.</span></span> <span data-ttu-id="ffe9f-147">Zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ffe9f-147">Enter the following:</span></span>
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" -Force`
     
       <span data-ttu-id="ffe9f-148">Podpora balíček je vytvořen pro oba řadiče v zadané síťové sdílené složce.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-148">The support package is created for both controllers in the specified network shared folder.</span></span> <span data-ttu-id="ffe9f-149">Je soubor zašifrované, komprimované, který lze odeslat společnosti Microsoft Support pro řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-149">It's an encrypted, compressed file that can be sent to Microsoft Support for troubleshooting.</span></span> <span data-ttu-id="ffe9f-150">Další informace najdete v tématu [obraťte se na podporu společnosti Microsoft](storsimple-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="ffe9f-150">For more information, see [Contact Microsoft Support](storsimple-contact-microsoft-support.md).</span></span>

### <a name="the-export-hcssupportpackage-cmdlet-parameters"></a><span data-ttu-id="ffe9f-151">Parametry rutiny Export-HcsSupportPackage</span><span class="sxs-lookup"><span data-stu-id="ffe9f-151">The Export-HcsSupportPackage cmdlet parameters</span></span>
<span data-ttu-id="ffe9f-152">Pomocí rutiny Export-HcsSupportPackage můžete použít následující parametry.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-152">You can use the following parameters with the Export-HcsSupportPackage cmdlet.</span></span>

| <span data-ttu-id="ffe9f-153">Parametr</span><span class="sxs-lookup"><span data-stu-id="ffe9f-153">Parameter</span></span> | <span data-ttu-id="ffe9f-154">Požadované a volitelné</span><span class="sxs-lookup"><span data-stu-id="ffe9f-154">Required/Optional</span></span> | <span data-ttu-id="ffe9f-155">Popis</span><span class="sxs-lookup"><span data-stu-id="ffe9f-155">Description</span></span> |
| --- | --- | --- |
| `-Path` |<span data-ttu-id="ffe9f-156">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="ffe9f-156">Required</span></span> |<span data-ttu-id="ffe9f-157">Pomocí zadejte umístění síťové sdílené složky, ve kterém je umístěn balíček pro podporu.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-157">Use to provide the location of the network shared folder in which the support package is placed.</span></span> |
| `-EncryptionPassphrase` |<span data-ttu-id="ffe9f-158">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="ffe9f-158">Required</span></span> |<span data-ttu-id="ffe9f-159">Pomocí lze zadat přístupové heslo k pomoci podporu balíček šifrovat.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-159">Use to provide a passphrase to help encrypt the support package.</span></span> |
| `-Credential` |<span data-ttu-id="ffe9f-160">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="ffe9f-160">Optional</span></span> |<span data-ttu-id="ffe9f-161">Slouží k poskytování přihlašovací údaje pro sdílené síťové složce přístup.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-161">Use to supply access credentials for the network shared folder.</span></span> |
| `-Force` |<span data-ttu-id="ffe9f-162">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="ffe9f-162">Optional</span></span> |<span data-ttu-id="ffe9f-163">Použijte šifrovací přístupové heslo potvrzení krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-163">Use to skip the encryption passphrase confirmation step.</span></span> |
| `-PackageTag` |<span data-ttu-id="ffe9f-164">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="ffe9f-164">Optional</span></span> |<span data-ttu-id="ffe9f-165">Použijte k určení adresáře v rámci *cesta* v balíčku pro podporu je umístěn.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-165">Use to specify a directory under *Path* in which the support package is placed.</span></span> <span data-ttu-id="ffe9f-166">Výchozí hodnota je [název]-[aktuální datum a time:yyyy-MM-dd-HH-mm-ss].</span><span class="sxs-lookup"><span data-stu-id="ffe9f-166">The default is [device name]-[current date and time:yyyy-MM-dd-HH-mm-ss].</span></span> |
| `-Scope` |<span data-ttu-id="ffe9f-167">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="ffe9f-167">Optional</span></span> |<span data-ttu-id="ffe9f-168">Zadejte jako **clusteru** (výchozí) k vytvoření balíčku pro podporu pro oba řadiče.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-168">Specify as **Cluster** (default) to create a support package for both controllers.</span></span> <span data-ttu-id="ffe9f-169">Pokud chcete vytvořit balíček pouze pro aktuální kontroler, zadejte **řadič**.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-169">If you want to create a package only for the current controller, specify **Controller**.</span></span> |

## <a name="edit-a-support-package"></a><span data-ttu-id="ffe9f-170">Upravte balíček podpory</span><span class="sxs-lookup"><span data-stu-id="ffe9f-170">Edit a support package</span></span>
<span data-ttu-id="ffe9f-171">Po vygenerování balíčku pro podporu, možná budete muset upravit balíček, který chcete odebrat citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-171">After you have generated a support package, you might need to edit the package to remove sensitive information.</span></span> <span data-ttu-id="ffe9f-172">To může zahrnovat názvů svazků, zařízení IP adresy a názvy zálohování ze souborů protokolů.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-172">This can include volume names, device IP addresses, and backup names from the log files.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ffe9f-173">Můžete upravit pouze podporu balíček, který byl vytvořen pomocí Windows Powershellu pro StorSimple.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-173">You can only edit a support package that was generated through Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="ffe9f-174">Nelze upravit balíček vytvořený na portálu Azure classic pomocí služby StorSimple Manager.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-174">You can't edit a package created in the Azure classic portal with StorSimple Manager service.</span></span>
> 
> 

<span data-ttu-id="ffe9f-175">Upravit balíčku pro podporu před nahráním na webu Microsoft Support, nejprve dešifrovat balíček pro podporu, upravte soubory a pak ho znovu zašifrovat.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-175">To edit a support package before uploading it on the Microsoft Support site, first decrypt the support package, edit the files, and then re-encrypt it.</span></span> <span data-ttu-id="ffe9f-176">Proveďte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-176">Perform the following steps.</span></span>

#### <a name="to-edit-a-support-package-in-windows-powershell-for-storsimple"></a><span data-ttu-id="ffe9f-177">Chcete-li upravit balíčku pro podporu ve Windows Powershellu pro StorSimple</span><span class="sxs-lookup"><span data-stu-id="ffe9f-177">To edit a support package in Windows PowerShell for StorSimple</span></span>
1. <span data-ttu-id="ffe9f-178">Generovat balíček podporu jak je popsáno dříve, v [k vytvoření balíčku pro podporu ve Windows Powershellu pro StorSimple](#to-create-a-support-package-in-windows-powershell-for-storsimple).</span><span class="sxs-lookup"><span data-stu-id="ffe9f-178">Generate a support package as described earlier, in [To create a support package in Windows PowerShell for StorSimple](#to-create-a-support-package-in-windows-powershell-for-storsimple).</span></span>
2. <span data-ttu-id="ffe9f-179">[Stáhnout skript](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) místně na vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-179">[Download the script](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) locally on your client.</span></span>
3. <span data-ttu-id="ffe9f-180">Naimportujte modul prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-180">Import the Windows PowerShell module.</span></span> <span data-ttu-id="ffe9f-181">Zadejte cestu k místní složce, ve které jste stáhli skript.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-181">Specify the path to the local folder in which you downloaded the script.</span></span> <span data-ttu-id="ffe9f-182">Chcete-li importovat modul, zadejte:</span><span class="sxs-lookup"><span data-stu-id="ffe9f-182">To import the module, enter:</span></span>
   
    `Import-module <Path to the folder that contains the Windows PowerShell script>`
4. <span data-ttu-id="ffe9f-183">Všechny soubory jsou *.aes* soubory, které jsou komprimovaná a šifrovaná.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-183">All the files are *.aes* files that are compressed and encrypted.</span></span> <span data-ttu-id="ffe9f-184">Chcete-li dekomprimovat a dešifrování souborů, zadejte:</span><span class="sxs-lookup"><span data-stu-id="ffe9f-184">To decompress and decrypt files, enter:</span></span>
   
    `Open-HcsSupportPackage <Path to the folder that contains support package files>`
   
    <span data-ttu-id="ffe9f-185">Všimněte si, že skutečný soubor rozšíření se nyní zobrazují všechny soubory.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-185">Note that the actual file extensions are now displayed for all the files.</span></span>
   
    ![Upravte balíček podpory](./media/storsimple-create-manage-support-package/IC750706.png)
5. <span data-ttu-id="ffe9f-187">Když se zobrazí výzva k šifrovací přístupové heslo, zadejte heslo, které jste použili při vytvoření balíčku podpory.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-187">When you're prompted for the encryption passphrase, enter the passphrase that you used when the support package was created.</span></span>
   
        cmdlet Open-HcsSupportPackage at command pipeline position 1
   
        Supply values for the following parameters:EncryptionPassphrase: ****
6. <span data-ttu-id="ffe9f-188">Přejděte do složky, která obsahuje soubory protokolu.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-188">Browse to the folder that contains the log files.</span></span> <span data-ttu-id="ffe9f-189">Protože soubory protokolu jsou nyní dekomprimovat a dešifrovat, to bude mít původní přípony souborů.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-189">Because the log files are now decompressed and decrypted, these will have original file extensions.</span></span> <span data-ttu-id="ffe9f-190">Tyto soubory odebrat jakékoli informace o zákazníkovi například názvů svazků a zařízení IP adresy, upravovat a ukládat soubory.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-190">Modify these files to remove any customer-specific information, such as volume names and device IP addresses, and save the files.</span></span>
7. <span data-ttu-id="ffe9f-191">Zavřete soubory a komprese je s gzip, je šifrování pomocí standardu AES 256.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-191">Close the files to compress them with gzip and encrypt them with AES-256.</span></span> <span data-ttu-id="ffe9f-192">Toto je pro rychlost a zabezpečení v balíčku pro podporu přenosu přes síť.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-192">This is for speed and security in transferring the support package over a network.</span></span> <span data-ttu-id="ffe9f-193">Možnost komprimace a šifrování souborů, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ffe9f-193">To compress and encrypt files, enter the following:</span></span>
   
    `Close-HcsSupportPackage <Path to the folder that contains support package files>`
   
    ![Upravte balíček podpory](./media/storsimple-create-manage-support-package/IC750707.png)
8. <span data-ttu-id="ffe9f-195">Po zobrazení výzvy zadejte šifrovací přístupové heslo pro balíček upravené podpory.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-195">When prompted, provide an encryption passphrase for the modified support package.</span></span>
   
        cmdlet Close-HcsSupportPackage at command pipeline position 1
        Supply values for the following parameters:EncryptionPassphrase: ****
9. <span data-ttu-id="ffe9f-196">Zapište nové přístupové heslo, takže ho můžete sdílet s Microsoft Support na požádání.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-196">Write down the new passphrase, so that you can share it with Microsoft Support when requested.</span></span>

### <a name="example-editing-files-in-a-support-package-on-a-password-protected-share"></a><span data-ttu-id="ffe9f-197">Příklad: Úprava souborů v balíčku pro podporu ve sdílené složce chráněné heslem</span><span class="sxs-lookup"><span data-stu-id="ffe9f-197">Example: Editing files in a support package on a password-protected share</span></span>
<span data-ttu-id="ffe9f-198">Následující příklad ukazuje, jak k dešifrování, upravit a znovu zašifrovat balíčku pro podporu.</span><span class="sxs-lookup"><span data-stu-id="ffe9f-198">The following example shows how to decrypt, edit, and re-encrypt a support package.</span></span>

        PS C:\WINDOWS\system32> Import-module C:\Users\Default\StorSimple\SupportPackage\HCSSupportPackageTools.psm1

        PS C:\WINDOWS\system32> Open-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Open-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32> Close-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Close-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32>

## <a name="next-steps"></a><span data-ttu-id="ffe9f-199">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ffe9f-199">Next steps</span></span>
* <span data-ttu-id="ffe9f-200">Zjistěte, jak [k řešení potíží s nasazením zařízení použít podporu balíčky a protokolů zařízení](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="ffe9f-200">Learn how to [use support packages and device logs to troubleshoot your device deployment](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting).</span></span>
* <span data-ttu-id="ffe9f-201">Zjistěte, jak [použít službu StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="ffe9f-201">Learn how to [use the StorSimple Manager service to administer your StorSimple device](storsimple-manager-service-administration.md).</span></span>

