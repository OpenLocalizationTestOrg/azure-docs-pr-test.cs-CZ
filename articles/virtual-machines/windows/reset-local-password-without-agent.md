---
title: "aaaReset místní heslo systému Windows bez agenta Azure | Microsoft Docs"
description: "Jak tooreset hello hesla místního uživatelského účtu systému Windows, když hello Azure hostovaný agent není nainstalován nebo je funkční na virtuálním počítači"
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: cf353dd3-89c9-47f6-a449-f874f0957013
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/07/2017
ms.author: iainfou
ms.openlocfilehash: c559c31ea142f9cf50d2c5b6182c5355fec9bac5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooreset-local-windows-password-for-azure-vm"></a><span data-ttu-id="d168a-103">Jak tooreset místní heslo systému Windows pro virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="d168a-103">How tooreset local Windows password for Azure VM</span></span>
<span data-ttu-id="d168a-104">Můžete obnovit heslo místního Windows hello virtuálního počítače v Azure pomocí hello [portál Azure nebo Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) zadaný je nainstalován agent hosta Azure hello.</span><span class="sxs-lookup"><span data-stu-id="d168a-104">You can reset hello local Windows password of a VM in Azure using hello [Azure portal or Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) provided hello Azure guest agent is installed.</span></span> <span data-ttu-id="d168a-105">Tato metoda je hello primární způsob tooreset heslo pro virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="d168a-105">This method is hello primary way tooreset a password for an Azure VM.</span></span> <span data-ttu-id="d168a-106">Pokud narazíte na potíže s hello Azure hostovaný agent neodpovídá, nebo selhání tooinstall po nahrání vlastní image, můžete ručně obnovit heslo systému Windows.</span><span class="sxs-lookup"><span data-stu-id="d168a-106">If you encounter issues with hello Azure guest agent not responding, or failing tooinstall after uploading a custom image, you can manually reset a Windows password.</span></span> <span data-ttu-id="d168a-107">Tento článek podrobně popisuje, jak tooreset heslo místního účtu připojením hello tooanother zdrojový OS virtuálního disku virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d168a-107">This article details how tooreset a local account password by attaching hello source OS virtual disk tooanother VM.</span></span> 

> [!WARNING]
> <span data-ttu-id="d168a-108">Tento proces lze používejte pouze jako poslední možnost.</span><span class="sxs-lookup"><span data-stu-id="d168a-108">Only use this process as a last resort.</span></span> <span data-ttu-id="d168a-109">Vždy zkuste tooreset hesla pomocí hello [portál Azure nebo Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) první.</span><span class="sxs-lookup"><span data-stu-id="d168a-109">Always try tooreset a password using hello [Azure portal or Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) first.</span></span>
> 
> 

## <a name="overview-of-hello-process"></a><span data-ttu-id="d168a-110">Přehled procesu hello</span><span class="sxs-lookup"><span data-stu-id="d168a-110">Overview of hello process</span></span>
<span data-ttu-id="d168a-111">Hello základní kroky pro místní heslo resetovat pro virtuální počítač s Windows v Azure, když není žádný agent hosta Azure toohello přístup je následující:</span><span class="sxs-lookup"><span data-stu-id="d168a-111">hello core steps for performing a local password reset for a Windows VM in Azure when there is no access toohello Azure guest agent is as follows:</span></span>

* <span data-ttu-id="d168a-112">Odstraňte hello zdrojového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d168a-112">Delete hello source VM.</span></span> <span data-ttu-id="d168a-113">virtuální disky Hello zůstanou zachovány.</span><span class="sxs-lookup"><span data-stu-id="d168a-113">hello virtual disks are retained.</span></span>
* <span data-ttu-id="d168a-114">Připojte tooanother disku operačního systému virtuálního počítače hello zdrojového Virtuálního počítače na hello stejné umístění v rámci vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="d168a-114">Attach hello source VM's OS disk tooanother VM on hello same location within your Azure subscription.</span></span> <span data-ttu-id="d168a-115">Tento virtuální počítač je hello odkazované tooas řešení potíží s virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d168a-115">This VM is referred tooas hello troubleshooting VM.</span></span>
* <span data-ttu-id="d168a-116">Pomocí hello řešení potíží s virtuálního počítače, vytvořte některé konfigurační soubory na disk s operačním systémem hello zdrojového Virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d168a-116">Using hello troubleshooting VM, create some config files on hello source VM's OS disk.</span></span>
* <span data-ttu-id="d168a-117">Odpojte disk s operačním systémem hello Virtuálního počítače z hello řešení potíží s virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d168a-117">Detach hello VM's OS disk from hello troubleshooting VM.</span></span>
* <span data-ttu-id="d168a-118">Použijte toocreate správce prostředků šablony virtuálního počítače, pomocí hello původní virtuální disk.</span><span class="sxs-lookup"><span data-stu-id="d168a-118">Use a Resource Manager template toocreate a VM, using hello original virtual disk.</span></span>
* <span data-ttu-id="d168a-119">Když hello nový virtuální počítač spustí, hello konfigurační soubory můžete vytvořit heslo hello aktualizace hello požadované uživatele.</span><span class="sxs-lookup"><span data-stu-id="d168a-119">When hello new VM boots, hello config files you create update hello password of hello required user.</span></span>

## <a name="detailed-steps"></a><span data-ttu-id="d168a-120">Podrobné kroky</span><span class="sxs-lookup"><span data-stu-id="d168a-120">Detailed steps</span></span>
<span data-ttu-id="d168a-121">Vždy zkuste tooreset hesla pomocí hello [portál Azure nebo Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) než to zkusíte hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="d168a-121">Always try tooreset a password using hello [Azure portal or Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) before trying hello following steps.</span></span> <span data-ttu-id="d168a-122">Ujistěte se, že máte zálohu virtuálního počítače, před zahájením.</span><span class="sxs-lookup"><span data-stu-id="d168a-122">Make sure you have a backup of your VM before you start.</span></span> 

1. <span data-ttu-id="d168a-123">Odstranit hello vliv virtuálního počítače na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d168a-123">Delete hello affected VM in Azure portal.</span></span> <span data-ttu-id="d168a-124">Odstraňování hello virtuálních počítačů odstraní pouze metadata hello, hello odkaz hello virtuálních počítačů v rámci Azure.</span><span class="sxs-lookup"><span data-stu-id="d168a-124">Deleting hello VM only deletes hello metadata, hello reference of hello VM within Azure.</span></span> <span data-ttu-id="d168a-125">Při odstranění hello virtuálních počítačů, zůstanou zachovány Hello virtuální disky:</span><span class="sxs-lookup"><span data-stu-id="d168a-125">hello virtual disks are retained when hello VM is deleted:</span></span>
   
   * <span data-ttu-id="d168a-126">Vyberte hello virtuálních počítačů v hello portál Azure, klikněte na tlačítko *odstranit*:</span><span class="sxs-lookup"><span data-stu-id="d168a-126">Select hello VM in hello Azure portal, click *Delete*:</span></span>
     
     ![Odstraňte existující virtuální počítač](./media/reset-local-password-without-agent/delete_vm.png)
2. <span data-ttu-id="d168a-128">Připojte toohello disku operačního systému hello zdrojového Virtuálního počítače, řešení potíží s virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d168a-128">Attach hello source VM’s OS disk toohello troubleshooting VM.</span></span> <span data-ttu-id="d168a-129">Hello řešení potíží s virtuálního počítače musí být v hello stejné oblasti jako disk s operačním systémem hello zdrojového Virtuálního počítače (například `West US`):</span><span class="sxs-lookup"><span data-stu-id="d168a-129">hello troubleshooting VM must be in hello same region as hello source VM's OS disk (such as `West US`):</span></span>
   
   * <span data-ttu-id="d168a-130">Vyberte hello řešení potíží s virtuálních počítačů v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d168a-130">Select hello troubleshooting VM in hello Azure portal.</span></span> <span data-ttu-id="d168a-131">Klikněte na tlačítko *disky* | *připojit existující*:</span><span class="sxs-lookup"><span data-stu-id="d168a-131">Click *Disks* | *Attach existing*:</span></span>
     
     ![Připojit stávající disk](./media/reset-local-password-without-agent/disks_attach_existing.png)
     
     <span data-ttu-id="d168a-133">Vyberte *souboru virtuálního pevného disku* a potom vyberte účet úložiště hello, která obsahuje vaše zdrojového virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="d168a-133">Select *VHD File* and then select hello storage account that contains your source VM:</span></span>
     
     ![Výběr účtu úložiště](./media/reset-local-password-without-agent/disks_select_storageaccount.PNG)
     
     <span data-ttu-id="d168a-135">Vyberte kontejner zdroj hello.</span><span class="sxs-lookup"><span data-stu-id="d168a-135">Select hello source container.</span></span> <span data-ttu-id="d168a-136">kontejner Hello zdroj je obvykle *virtuální pevné disky*:</span><span class="sxs-lookup"><span data-stu-id="d168a-136">hello source container is typically *vhds*:</span></span>
     
     ![Vyberte kontejner úložiště](./media/reset-local-password-without-agent/disks_select_container.png)
     
     <span data-ttu-id="d168a-138">Vyberte hello tooattach virtuální pevný disk operačního systému.</span><span class="sxs-lookup"><span data-stu-id="d168a-138">Select hello OS vhd tooattach.</span></span> <span data-ttu-id="d168a-139">Klikněte na tlačítko *vyberte* toocomplete hello proces:</span><span class="sxs-lookup"><span data-stu-id="d168a-139">Click *Select* toocomplete hello process:</span></span>
     
     ![Vybrat zdroj virtuálního disku](./media/reset-local-password-without-agent/disks_select_source_vhd.png)
3. <span data-ttu-id="d168a-141">Připojit toohello řešení potíží s virtuálního počítače pomocí vzdálené plochy a ujistěte se, že disk s operačním systémem hello zdrojového Virtuálního počítače je viditelná:</span><span class="sxs-lookup"><span data-stu-id="d168a-141">Connect toohello troubleshooting VM using Remote Desktop and ensure hello source VM's OS disk is visible:</span></span>
   
   * <span data-ttu-id="d168a-142">Vyberte hello řešení potíží s virtuálních počítačů v hello portál Azure a klikněte na *Connect*.</span><span class="sxs-lookup"><span data-stu-id="d168a-142">Select hello troubleshooting VM in hello Azure portal and click *Connect*.</span></span>
   * <span data-ttu-id="d168a-143">Otevřete soubor RDP hello, který stahuje.</span><span class="sxs-lookup"><span data-stu-id="d168a-143">Open hello RDP file that downloads.</span></span> <span data-ttu-id="d168a-144">Zadejte hello uživatelské jméno a heslo hello řešení potíží s virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d168a-144">Enter hello username and password of hello troubleshooting VM.</span></span>
   * <span data-ttu-id="d168a-145">V Průzkumníku souborů vyhledejte hello datový disk, který jste připojili.</span><span class="sxs-lookup"><span data-stu-id="d168a-145">In File Explorer, look for hello data disk you attached.</span></span> <span data-ttu-id="d168a-146">Když hello zdroj, který virtuální pevný disk Virtuálního počítače hello pouze datový disk připojený toohello řešení potíží s virtuálních počítačů, měla by být hello F: jednotky:</span><span class="sxs-lookup"><span data-stu-id="d168a-146">If hello source VM’s VHD is hello only data disk attached toohello troubleshooting VM, it should be hello F: drive:</span></span>
     
     ![Zobrazit připojené datový disk](./media/reset-local-password-without-agent/troubleshooting_vm_fileexplorer.png)
4. <span data-ttu-id="d168a-148">Vytvoření `gpt.ini` v `\Windows\System32\GroupPolicy` na jednotce hello zdrojového Virtuálního počítače (pokud existuje gpt.ini, přejmenujte toogpt.ini.bak):</span><span class="sxs-lookup"><span data-stu-id="d168a-148">Create `gpt.ini` in `\Windows\System32\GroupPolicy` on hello source VM’s drive (if gpt.ini exists, rename toogpt.ini.bak):</span></span>
   
   > [!WARNING]
   > <span data-ttu-id="d168a-149">Zajistěte, aby náhodně vytvořit hello následující soubory v C:\Windows, hello jednotky operačního systému pro hello řešení potíží s virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d168a-149">Make sure that you do not accidentally create hello following files in C:\Windows, hello OS drive for hello troubleshooting VM.</span></span> <span data-ttu-id="d168a-150">Vytvořte hello následující soubory v jednotce hello operačního systému pro vaše zdrojového virtuálního počítače, který je připojen jako datový disk.</span><span class="sxs-lookup"><span data-stu-id="d168a-150">Create hello following files in hello OS drive for your source VM that is attached as a data disk.</span></span>
   > 
   > 
   
   * <span data-ttu-id="d168a-151">Přidejte následující řádky do hello hello `gpt.ini` souborů, které jste vytvořili:</span><span class="sxs-lookup"><span data-stu-id="d168a-151">Add hello following lines into hello `gpt.ini` file you created:</span></span>
     
     ```
     [General]
     gPCFunctionalityVersion=2
     gPCMachineExtensionNames=[{42B5FAAE-6536-11D2-AE5A-0000F87571E3}{40B6664F-4972-11D1-A7CA-0000F87571E3}]
     Version=1
     ```
     
     ![Vytvoření gpt.ini](./media/reset-local-password-without-agent/create_gpt_ini.png)
5. <span data-ttu-id="d168a-153">Vytvoření `scripts.ini` v `\Windows\System32\GroupPolicy\Machine\Scripts`.</span><span class="sxs-lookup"><span data-stu-id="d168a-153">Create `scripts.ini` in `\Windows\System32\GroupPolicy\Machine\Scripts`.</span></span> <span data-ttu-id="d168a-154">Ujistěte se, že se zobrazují skrytých složek.</span><span class="sxs-lookup"><span data-stu-id="d168a-154">Make sure hidden folders are shown.</span></span> <span data-ttu-id="d168a-155">V případě potřeby vytvořte hello `Machine` nebo `Scripts` složek.</span><span class="sxs-lookup"><span data-stu-id="d168a-155">If needed, create hello `Machine` or `Scripts` folders.</span></span>
   
   * <span data-ttu-id="d168a-156">Přidejte následující řádky hello hello `scripts.ini` souborů, které jste vytvořili:</span><span class="sxs-lookup"><span data-stu-id="d168a-156">Add hello following lines hello `scripts.ini` file you created:</span></span>
     
     ```
     [Startup]
     0CmdLine=C:\Windows\System32\FixAzureVM.cmd
     0Parameters=
     ```
     
     ![Vytvoření scripts.ini](./media/reset-local-password-without-agent/create_scripts_ini.png)
6. <span data-ttu-id="d168a-158">Vytvoření `FixAzureVM.cmd` v `\Windows\System32` s hello následující obsah, nahraďte `<username>` a `<newpassword>` vlastními hodnotami:</span><span class="sxs-lookup"><span data-stu-id="d168a-158">Create `FixAzureVM.cmd` in `\Windows\System32` with hello following contents, replacing `<username>` and `<newpassword>` with your own values:</span></span>
   
    ```
    net user <username> <newpassword> /add
    net localgroup administrators <username> /add
    net localgroup "remote desktop users" <username> /add

    ```

    ![Vytvoření FixAzureVM.cmd](./media/reset-local-password-without-agent/create_fixazure_cmd.png)
   
    <span data-ttu-id="d168a-160">Při definování hello nové heslo, musí splňovat požadavky na složitost hesla hello nakonfigurované pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="d168a-160">You must meet hello configured password complexity requirements for your VM when defining hello new password.</span></span>
7. <span data-ttu-id="d168a-161">Na portálu Azure odpojte disk hello od hello řešení potíží s virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="d168a-161">In Azure portal, detach hello disk from hello troubleshooting VM:</span></span>
   
   * <span data-ttu-id="d168a-162">Vyberte hello řešení potíží s virtuálních počítačů v hello portálu Azure, klikněte na *disky*.</span><span class="sxs-lookup"><span data-stu-id="d168a-162">Select hello troubleshooting VM in hello Azure portal, click *Disks*.</span></span>
   * <span data-ttu-id="d168a-163">Vyberte hello datový disk připojený v kroku 2, klikněte na tlačítko *odpojení*:</span><span class="sxs-lookup"><span data-stu-id="d168a-163">Select hello data disk attached in step 2, click *Detach*:</span></span>
     
     ![Odpojte disk](./media/reset-local-password-without-agent/detach_disk.png)
8. <span data-ttu-id="d168a-165">Než vytvoříte virtuální počítač, získejte hello URI tooyour zdrojový OS disk:</span><span class="sxs-lookup"><span data-stu-id="d168a-165">Before you create a VM, obtain hello URI tooyour source OS disk:</span></span>
   
   * <span data-ttu-id="d168a-166">Klikněte na účet úložiště vyberte hello hello portál Azure, *objekty BLOB*.</span><span class="sxs-lookup"><span data-stu-id="d168a-166">Select hello storage account in hello Azure portal, click *Blobs*.</span></span>
   * <span data-ttu-id="d168a-167">Vyberte kontejner hello.</span><span class="sxs-lookup"><span data-stu-id="d168a-167">Select hello container.</span></span> <span data-ttu-id="d168a-168">kontejner Hello zdroj je obvykle *virtuální pevné disky*:</span><span class="sxs-lookup"><span data-stu-id="d168a-168">hello source container is typically *vhds*:</span></span>
     
     ![Vyberte účet úložiště objektů blob](./media/reset-local-password-without-agent/select_storage_details.png)
     
     <span data-ttu-id="d168a-170">Vyberte zdroj virtuální pevný disk operačního systému virtuálního počítače a klikněte na tlačítko hello *kopie* tlačítko Další toohello *URL* název:</span><span class="sxs-lookup"><span data-stu-id="d168a-170">Select your source VM OS VHD and click hello *Copy* button next toohello *URL* name:</span></span>
     
     ![Zkopírujte disk identifikátor URI](./media/reset-local-password-without-agent/copy_source_vhd_uri.png)
9. <span data-ttu-id="d168a-172">Vytvoření virtuálního počítače z disku operačního systému hello zdrojového Virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="d168a-172">Create a VM from hello source VM’s OS disk:</span></span>
   
   * <span data-ttu-id="d168a-173">Použití [této šablony Azure Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) toocreate virtuálního počítače z specializované virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="d168a-173">Use [this Azure Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) toocreate a VM from a specialized VHD.</span></span> <span data-ttu-id="d168a-174">Klikněte na tlačítko hello `Deploy tooAzure` tlačítko tooopen hello portál Azure s použitím šablon podrobnostmi hello vyplní za vás.</span><span class="sxs-lookup"><span data-stu-id="d168a-174">Click hello `Deploy tooAzure` button tooopen hello Azure portal with hello templated details populated for you.</span></span>
   * <span data-ttu-id="d168a-175">Pokud chcete tooretain všech hello předchozí nastavení pro hello virtuálního počítače, vyberte *úpravy šablony* tooprovide existující virtuální síť, podsíť, síťový adaptér nebo veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="d168a-175">If you want tooretain all hello previous settings for hello VM, select *Edit template* tooprovide your existing VNet, subnet, network adapter, or public IP.</span></span>
   * <span data-ttu-id="d168a-176">V hello `OSDISKVHDURI` parametr textového pole, vložte hello získat identifikátor URI zdrojového virtuálního pevného disku v předchozím kroku hello:</span><span class="sxs-lookup"><span data-stu-id="d168a-176">In hello `OSDISKVHDURI` parameter text box, paste hello URI of your source VHD obtain in hello preceding step:</span></span>
     
     ![Vytvoření virtuálního počítače ze šablony](./media/reset-local-password-without-agent/create_new_vm_from_template.png)
10. <span data-ttu-id="d168a-178">Po hello nový virtuální počítač spuštěný, připojit toohello virtuálního počítače pomocí vzdálené plochy s hello nové heslo jste zadali v hello `FixAzureVM.cmd` skriptu.</span><span class="sxs-lookup"><span data-stu-id="d168a-178">After hello new VM is running, connect toohello VM using Remote Desktop with hello new password you specified in hello `FixAzureVM.cmd` script.</span></span>
11. <span data-ttu-id="d168a-179">Z vaší relace vzdálené toohello nového virtuálního počítače, odeberte hello následující soubory tooclean hello prostředí:</span><span class="sxs-lookup"><span data-stu-id="d168a-179">From your remote session toohello new VM, remove hello following files tooclean up hello environment:</span></span>
    
    * <span data-ttu-id="d168a-180">Z %windir%\System32</span><span class="sxs-lookup"><span data-stu-id="d168a-180">From %windir%\System32</span></span>
      * <span data-ttu-id="d168a-181">odebrat FixAzureVM.cmd</span><span class="sxs-lookup"><span data-stu-id="d168a-181">remove FixAzureVM.cmd</span></span>
    * <span data-ttu-id="d168a-182">Z %windir%\System32\GroupPolicy\Machine\\</span><span class="sxs-lookup"><span data-stu-id="d168a-182">From %windir%\System32\GroupPolicy\Machine\\</span></span>
      * <span data-ttu-id="d168a-183">odebrat scripts.ini</span><span class="sxs-lookup"><span data-stu-id="d168a-183">remove scripts.ini</span></span>
    * <span data-ttu-id="d168a-184">Z %windir%\System32\GroupPolicy</span><span class="sxs-lookup"><span data-stu-id="d168a-184">From %windir%\System32\GroupPolicy</span></span>
      * <span data-ttu-id="d168a-185">Odeberte gpt.ini (pokud existoval gpt.ini a přejmenovat toogpt.ini.bak, přejmenování hello .bak souboru back toogpt.ini)</span><span class="sxs-lookup"><span data-stu-id="d168a-185">remove gpt.ini (if gpt.ini existed before, and you renamed it toogpt.ini.bak, rename hello .bak file back toogpt.ini)</span></span>

## <a name="next-steps"></a><span data-ttu-id="d168a-186">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d168a-186">Next steps</span></span>
<span data-ttu-id="d168a-187">Pokud se pořád nemůžete připojit pomocí vzdálené plochy, přečtěte si téma hello [Průvodci odstraňováním potíží RDP](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d168a-187">If you still cannot connect using Remote Desktop, see hello [RDP troubleshooting guide](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="d168a-188">Hello [podrobný Průvodce odstraňováním potíží s RDP](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) vypadá na řešení potíží s metody, nikoli konkrétní kroky.</span><span class="sxs-lookup"><span data-stu-id="d168a-188">hello [detailed RDP troubleshooting guide](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) looks at troubleshooting methods rather than specific steps.</span></span> <span data-ttu-id="d168a-189">Můžete také [otevřete žádost o podporu Azure](https://azure.microsoft.com/support/options/) praktických pomoc.</span><span class="sxs-lookup"><span data-stu-id="d168a-189">You can also [open an Azure support request](https://azure.microsoft.com/support/options/) for hands-on assistance.</span></span>

