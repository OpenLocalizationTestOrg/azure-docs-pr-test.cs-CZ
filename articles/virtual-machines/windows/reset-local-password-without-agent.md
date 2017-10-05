---
title: "Resetovat heslo místního systému Windows bez agenta Azure | Microsoft Docs"
description: "Obnovení hesla místního uživatelského účtu systému Windows, když Azure hostovaného agenta, není nainstalován nebo je funkční na virtuálním počítači"
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
ms.openlocfilehash: 880f5e5967298401fc2522124af3746d9906ffa8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-reset-local-windows-password-for-azure-vm"></a><span data-ttu-id="3a9bd-103">Jak obnovit heslo místního systému Windows pro virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="3a9bd-103">How to reset local Windows password for Azure VM</span></span>
<span data-ttu-id="3a9bd-104">Můžete resetovat hesla místního systému Windows virtuálního počítače v Azure pomocí [portál Azure nebo Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) zadaný je nainstalován agent hosta Azure.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-104">You can reset the local Windows password of a VM in Azure using the [Azure portal or Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) provided the Azure guest agent is installed.</span></span> <span data-ttu-id="3a9bd-105">Tato metoda je primární způsob, jak resetovat heslo pro virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-105">This method is the primary way to reset a password for an Azure VM.</span></span> <span data-ttu-id="3a9bd-106">Pokud narazíte na potíže s agentem Azure hosta neodpovídá nebo je možné nainstalovat, nahráním vlastní image, můžete ručně obnovit heslo systému Windows.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-106">If you encounter issues with the Azure guest agent not responding, or failing to install after uploading a custom image, you can manually reset a Windows password.</span></span> <span data-ttu-id="3a9bd-107">Tento článek popisuje, jak resetovat heslo místního účtu pomocí zdrojový OS virtuální disk se připojuje k jiným virtuálním Počítačem.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-107">This article details how to reset a local account password by attaching the source OS virtual disk to another VM.</span></span> 

> [!WARNING]
> <span data-ttu-id="3a9bd-108">Tento proces lze používejte pouze jako poslední možnost.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-108">Only use this process as a last resort.</span></span> <span data-ttu-id="3a9bd-109">Vždy se pokusí resetovat heslo pomocí [portál Azure nebo Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) první.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-109">Always try to reset a password using the [Azure portal or Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) first.</span></span>
> 
> 

## <a name="overview-of-the-process"></a><span data-ttu-id="3a9bd-110">Přehled procesu</span><span class="sxs-lookup"><span data-stu-id="3a9bd-110">Overview of the process</span></span>
<span data-ttu-id="3a9bd-111">Základní kroky pro místní heslo resetovat pro virtuální počítač s Windows v Azure, když není k dispozici přístup k Azure hostovaného agenta je následující:</span><span class="sxs-lookup"><span data-stu-id="3a9bd-111">The core steps for performing a local password reset for a Windows VM in Azure when there is no access to the Azure guest agent is as follows:</span></span>

* <span data-ttu-id="3a9bd-112">Odstraňte zdrojový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-112">Delete the source VM.</span></span> <span data-ttu-id="3a9bd-113">Virtuální disky se zachovají.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-113">The virtual disks are retained.</span></span>
* <span data-ttu-id="3a9bd-114">Disk s operačním systémem zdrojového Virtuálního počítače připojte k jiným virtuálním Počítačem na stejné umístění v rámci vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-114">Attach the source VM's OS disk to another VM on the same location within your Azure subscription.</span></span> <span data-ttu-id="3a9bd-115">Tento virtuální počítač se označuje jako řešení potíží virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-115">This VM is referred to as the troubleshooting VM.</span></span>
* <span data-ttu-id="3a9bd-116">Pomocí řešení potíží virtuální počítač, vytvořte některé konfigurační soubory na disku pro operační systém zdrojového Virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-116">Using the troubleshooting VM, create some config files on the source VM's OS disk.</span></span>
* <span data-ttu-id="3a9bd-117">Odpojte disk s operačním systémem Virtuálního počítače z virtuálního počítače, řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-117">Detach the VM's OS disk from the troubleshooting VM.</span></span>
* <span data-ttu-id="3a9bd-118">Pomocí šablony Resource Manageru k vytvoření virtuálního počítače, pomocí původní virtuální disk.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-118">Use a Resource Manager template to create a VM, using the original virtual disk.</span></span>
* <span data-ttu-id="3a9bd-119">Když se nový virtuální počítač spustí, konfigurační soubory, které vytvoříte aktualizovat heslo požadovaného uživatele.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-119">When the new VM boots, the config files you create update the password of the required user.</span></span>

## <a name="detailed-steps"></a><span data-ttu-id="3a9bd-120">Podrobné kroky</span><span class="sxs-lookup"><span data-stu-id="3a9bd-120">Detailed steps</span></span>
<span data-ttu-id="3a9bd-121">Vždy se pokusí resetovat heslo pomocí [portál Azure nebo Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) než to zkusíte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-121">Always try to reset a password using the [Azure portal or Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) before trying the following steps.</span></span> <span data-ttu-id="3a9bd-122">Ujistěte se, že máte zálohu virtuálního počítače, před zahájením.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-122">Make sure you have a backup of your VM before you start.</span></span> 

1. <span data-ttu-id="3a9bd-123">Odstraňte ovlivněné virtuální počítač na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-123">Delete the affected VM in Azure portal.</span></span> <span data-ttu-id="3a9bd-124">Odstraňuje se virtuální počítač odstraní pouze metadata, odkaz na virtuální počítač v rámci Azure.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-124">Deleting the VM only deletes the metadata, the reference of the VM within Azure.</span></span> <span data-ttu-id="3a9bd-125">Virtuální disky jsou uchovány při odstranění virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="3a9bd-125">The virtual disks are retained when the VM is deleted:</span></span>
   
   * <span data-ttu-id="3a9bd-126">Vyberte virtuální počítač na portálu Azure, klikněte na *odstranit*:</span><span class="sxs-lookup"><span data-stu-id="3a9bd-126">Select the VM in the Azure portal, click *Delete*:</span></span>
     
     ![Odstraňte existující virtuální počítač](./media/reset-local-password-without-agent/delete_vm.png)
2. <span data-ttu-id="3a9bd-128">Disk s operačním systémem zdrojového Virtuálního počítače připojte k řešení potíží virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-128">Attach the source VM’s OS disk to the troubleshooting VM.</span></span> <span data-ttu-id="3a9bd-129">Řešení potíží virtuální počítač musí být ve stejné oblasti jako disk s operačním systémem zdrojového Virtuálního počítače (například `West US`):</span><span class="sxs-lookup"><span data-stu-id="3a9bd-129">The troubleshooting VM must be in the same region as the source VM's OS disk (such as `West US`):</span></span>
   
   * <span data-ttu-id="3a9bd-130">Vyberte řešení potíží virtuální počítač na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-130">Select the troubleshooting VM in the Azure portal.</span></span> <span data-ttu-id="3a9bd-131">Klikněte na tlačítko *disky* | *připojit existující*:</span><span class="sxs-lookup"><span data-stu-id="3a9bd-131">Click *Disks* | *Attach existing*:</span></span>
     
     ![Připojit stávající disk](./media/reset-local-password-without-agent/disks_attach_existing.png)
     
     <span data-ttu-id="3a9bd-133">Vyberte *souboru virtuálního pevného disku* a potom vyberte účet úložiště, který obsahuje vaše zdrojového virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="3a9bd-133">Select *VHD File* and then select the storage account that contains your source VM:</span></span>
     
     ![Výběr účtu úložiště](./media/reset-local-password-without-agent/disks_select_storageaccount.PNG)
     
     <span data-ttu-id="3a9bd-135">Vyberte kontejner zdroje.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-135">Select the source container.</span></span> <span data-ttu-id="3a9bd-136">Kontejner zdroj je obvykle *virtuální pevné disky*:</span><span class="sxs-lookup"><span data-stu-id="3a9bd-136">The source container is typically *vhds*:</span></span>
     
     ![Vyberte kontejner úložiště](./media/reset-local-password-without-agent/disks_select_container.png)
     
     <span data-ttu-id="3a9bd-138">Vyberte virtuální pevný disk operačního systému připojit.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-138">Select the OS vhd to attach.</span></span> <span data-ttu-id="3a9bd-139">Klikněte na tlačítko *vyberte* dokončete proces:</span><span class="sxs-lookup"><span data-stu-id="3a9bd-139">Click *Select* to complete the process:</span></span>
     
     ![Vybrat zdroj virtuálního disku](./media/reset-local-password-without-agent/disks_select_source_vhd.png)
3. <span data-ttu-id="3a9bd-141">Připojení k řešení potíží virtuálního počítače pomocí vzdálené plochy a ujistěte se, že disk s operačním systémem zdrojového Virtuálního počítače je viditelná:</span><span class="sxs-lookup"><span data-stu-id="3a9bd-141">Connect to the troubleshooting VM using Remote Desktop and ensure the source VM's OS disk is visible:</span></span>
   
   * <span data-ttu-id="3a9bd-142">Vyberte řešení potíží virtuální počítač na portálu Azure a klikněte na tlačítko *Connect*.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-142">Select the troubleshooting VM in the Azure portal and click *Connect*.</span></span>
   * <span data-ttu-id="3a9bd-143">Otevřete soubor RDP, který stahuje.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-143">Open the RDP file that downloads.</span></span> <span data-ttu-id="3a9bd-144">Zadejte uživatelské jméno a heslo řešení potíží virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-144">Enter the username and password of the troubleshooting VM.</span></span>
   * <span data-ttu-id="3a9bd-145">V Průzkumníku souborů vyhledejte datový disk, který jste připojili.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-145">In File Explorer, look for the data disk you attached.</span></span> <span data-ttu-id="3a9bd-146">Když zdroj virtuálního pevného disku Virtuálního počítače je pouze datový disk připojen k řešení potíží virtuální počítač, měla by být F: jednotky:</span><span class="sxs-lookup"><span data-stu-id="3a9bd-146">If the source VM’s VHD is the only data disk attached to the troubleshooting VM, it should be the F: drive:</span></span>
     
     ![Zobrazit připojené datový disk](./media/reset-local-password-without-agent/troubleshooting_vm_fileexplorer.png)
4. <span data-ttu-id="3a9bd-148">Vytvoření `gpt.ini` v `\Windows\System32\GroupPolicy` na jednotce zdrojového Virtuálního počítače (pokud existuje gpt.ini, přejmenujte gpt.ini.bak):</span><span class="sxs-lookup"><span data-stu-id="3a9bd-148">Create `gpt.ini` in `\Windows\System32\GroupPolicy` on the source VM’s drive (if gpt.ini exists, rename to gpt.ini.bak):</span></span>
   
   > [!WARNING]
   > <span data-ttu-id="3a9bd-149">Ujistěte se, že nevytvoříte omylem následující soubory v C:\Windows, disk operačního systému pro řešení potíží virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-149">Make sure that you do not accidentally create the following files in C:\Windows, the OS drive for the troubleshooting VM.</span></span> <span data-ttu-id="3a9bd-150">Vytvořte tyto soubory na jednotce operačního systému pro vaše zdrojového virtuálního počítače, který je připojen jako datový disk.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-150">Create the following files in the OS drive for your source VM that is attached as a data disk.</span></span>
   > 
   > 
   
   * <span data-ttu-id="3a9bd-151">Přidejte následující řádky do `gpt.ini` souborů, které jste vytvořili:</span><span class="sxs-lookup"><span data-stu-id="3a9bd-151">Add the following lines into the `gpt.ini` file you created:</span></span>
     
     ```
     [General]
     gPCFunctionalityVersion=2
     gPCMachineExtensionNames=[{42B5FAAE-6536-11D2-AE5A-0000F87571E3}{40B6664F-4972-11D1-A7CA-0000F87571E3}]
     Version=1
     ```
     
     ![Vytvoření gpt.ini](./media/reset-local-password-without-agent/create_gpt_ini.png)
5. <span data-ttu-id="3a9bd-153">Vytvoření `scripts.ini` v `\Windows\System32\GroupPolicy\Machine\Scripts`.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-153">Create `scripts.ini` in `\Windows\System32\GroupPolicy\Machine\Scripts`.</span></span> <span data-ttu-id="3a9bd-154">Ujistěte se, že se zobrazují skrytých složek.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-154">Make sure hidden folders are shown.</span></span> <span data-ttu-id="3a9bd-155">V případě potřeby vytvořte `Machine` nebo `Scripts` složek.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-155">If needed, create the `Machine` or `Scripts` folders.</span></span>
   
   * <span data-ttu-id="3a9bd-156">Přidejte následující řádky `scripts.ini` souborů, které jste vytvořili:</span><span class="sxs-lookup"><span data-stu-id="3a9bd-156">Add the following lines the `scripts.ini` file you created:</span></span>
     
     ```
     [Startup]
     0CmdLine=C:\Windows\System32\FixAzureVM.cmd
     0Parameters=
     ```
     
     ![Vytvoření scripts.ini](./media/reset-local-password-without-agent/create_scripts_ini.png)
6. <span data-ttu-id="3a9bd-158">Vytvoření `FixAzureVM.cmd` v `\Windows\System32` s následující obsah, nahraďte `<username>` a `<newpassword>` vlastními hodnotami:</span><span class="sxs-lookup"><span data-stu-id="3a9bd-158">Create `FixAzureVM.cmd` in `\Windows\System32` with the following contents, replacing `<username>` and `<newpassword>` with your own values:</span></span>
   
    ```
    net user <username> <newpassword> /add
    net localgroup administrators <username> /add
    net localgroup "remote desktop users" <username> /add

    ```

    ![Vytvoření FixAzureVM.cmd](./media/reset-local-password-without-agent/create_fixazure_cmd.png)
   
    <span data-ttu-id="3a9bd-160">Při definování nové heslo, musí splňovat požadavky na složitost nakonfigurovaným heslem pro váš virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-160">You must meet the configured password complexity requirements for your VM when defining the new password.</span></span>
7. <span data-ttu-id="3a9bd-161">Na portálu Azure odpojte disk z virtuálního počítače, řešení potíží:</span><span class="sxs-lookup"><span data-stu-id="3a9bd-161">In Azure portal, detach the disk from the troubleshooting VM:</span></span>
   
   * <span data-ttu-id="3a9bd-162">Vyberte řešení potíží virtuální počítač na portálu Azure, klikněte na *disky*.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-162">Select the troubleshooting VM in the Azure portal, click *Disks*.</span></span>
   * <span data-ttu-id="3a9bd-163">Klikněte na tlačítko připojit datový disk v kroku 2, vyberte *odpojení*:</span><span class="sxs-lookup"><span data-stu-id="3a9bd-163">Select the data disk attached in step 2, click *Detach*:</span></span>
     
     ![Odpojte disk](./media/reset-local-password-without-agent/detach_disk.png)
8. <span data-ttu-id="3a9bd-165">Než vytvoříte virtuální počítač, získejte identifikátor URI pro váš zdrojový disk operačního systému:</span><span class="sxs-lookup"><span data-stu-id="3a9bd-165">Before you create a VM, obtain the URI to your source OS disk:</span></span>
   
   * <span data-ttu-id="3a9bd-166">Vyberte účet úložiště na portálu Azure, klikněte na tlačítko *objekty BLOB*.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-166">Select the storage account in the Azure portal, click *Blobs*.</span></span>
   * <span data-ttu-id="3a9bd-167">Vyberte kontejner.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-167">Select the container.</span></span> <span data-ttu-id="3a9bd-168">Kontejner zdroj je obvykle *virtuální pevné disky*:</span><span class="sxs-lookup"><span data-stu-id="3a9bd-168">The source container is typically *vhds*:</span></span>
     
     ![Vyberte účet úložiště objektů blob](./media/reset-local-password-without-agent/select_storage_details.png)
     
     <span data-ttu-id="3a9bd-170">Vyberte zdroj virtuální pevný disk operačního systému virtuálního počítače a klikněte na tlačítko *kopie* vedle položky *URL* název:</span><span class="sxs-lookup"><span data-stu-id="3a9bd-170">Select your source VM OS VHD and click the *Copy* button next to the *URL* name:</span></span>
     
     ![Zkopírujte disk identifikátor URI](./media/reset-local-password-without-agent/copy_source_vhd_uri.png)
9. <span data-ttu-id="3a9bd-172">Vytvoření virtuálního počítače z disku operačního systému zdrojového Virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="3a9bd-172">Create a VM from the source VM’s OS disk:</span></span>
   
   * <span data-ttu-id="3a9bd-173">Použití [této šablony Azure Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) vytvoření virtuálního počítače z specializované virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-173">Use [this Azure Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) to create a VM from a specialized VHD.</span></span> <span data-ttu-id="3a9bd-174">Klikněte `Deploy to Azure` tlačítko otevřete portál Azure s použitím šablon podrobnosti vyplní za vás.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-174">Click the `Deploy to Azure` button to open the Azure portal with the templated details populated for you.</span></span>
   * <span data-ttu-id="3a9bd-175">Pokud chcete zachovat předchozí nastavení pro virtuální počítač, vyberte *úpravy šablony* zajistit existující virtuální síť, podsíť, síťový adaptér nebo veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-175">If you want to retain all the previous settings for the VM, select *Edit template* to provide your existing VNet, subnet, network adapter, or public IP.</span></span>
   * <span data-ttu-id="3a9bd-176">V `OSDISKVHDURI` parametr textového pole, vložte identifikátor URI virtuálního pevného disku zdrojového získat v předchozím kroku:</span><span class="sxs-lookup"><span data-stu-id="3a9bd-176">In the `OSDISKVHDURI` parameter text box, paste the URI of your source VHD obtain in the preceding step:</span></span>
     
     ![Vytvoření virtuálního počítače ze šablony](./media/reset-local-password-without-agent/create_new_vm_from_template.png)
10. <span data-ttu-id="3a9bd-178">Po spuštění nového virtuálního počítače připojit k virtuálnímu počítači pomocí vzdálené plochy s nové heslo, které jste zadali v `FixAzureVM.cmd` skriptu.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-178">After the new VM is running, connect to the VM using Remote Desktop with the new password you specified in the `FixAzureVM.cmd` script.</span></span>
11. <span data-ttu-id="3a9bd-179">Ze vzdálené relace do nového virtuálního počítače odeberte tyto soubory do vyčištění prostředí:</span><span class="sxs-lookup"><span data-stu-id="3a9bd-179">From your remote session to the new VM, remove the following files to clean up the environment:</span></span>
    
    * <span data-ttu-id="3a9bd-180">Z %windir%\System32</span><span class="sxs-lookup"><span data-stu-id="3a9bd-180">From %windir%\System32</span></span>
      * <span data-ttu-id="3a9bd-181">odebrat FixAzureVM.cmd</span><span class="sxs-lookup"><span data-stu-id="3a9bd-181">remove FixAzureVM.cmd</span></span>
    * <span data-ttu-id="3a9bd-182">Z %windir%\System32\GroupPolicy\Machine\\</span><span class="sxs-lookup"><span data-stu-id="3a9bd-182">From %windir%\System32\GroupPolicy\Machine\\</span></span>
      * <span data-ttu-id="3a9bd-183">odebrat scripts.ini</span><span class="sxs-lookup"><span data-stu-id="3a9bd-183">remove scripts.ini</span></span>
    * <span data-ttu-id="3a9bd-184">Z %windir%\System32\GroupPolicy</span><span class="sxs-lookup"><span data-stu-id="3a9bd-184">From %windir%\System32\GroupPolicy</span></span>
      * <span data-ttu-id="3a9bd-185">Odeberte gpt.ini (pokud existoval gpt.ini a přejmenoval jej na gpt.ini.bak, přejmenujte soubor .bak zpět do gpt.ini)</span><span class="sxs-lookup"><span data-stu-id="3a9bd-185">remove gpt.ini (if gpt.ini existed before, and you renamed it to gpt.ini.bak, rename the .bak file back to gpt.ini)</span></span>

## <a name="next-steps"></a><span data-ttu-id="3a9bd-186">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3a9bd-186">Next steps</span></span>
<span data-ttu-id="3a9bd-187">Pokud se pořád nemůžete připojit pomocí vzdálené plochy, přečtěte si téma [Průvodci odstraňováním potíží RDP](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3a9bd-187">If you still cannot connect using Remote Desktop, see the [RDP troubleshooting guide](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="3a9bd-188">[Podrobný Průvodce odstraňováním potíží s RDP](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) vypadá na řešení potíží s metody, nikoli konkrétní kroky.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-188">The [detailed RDP troubleshooting guide](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) looks at troubleshooting methods rather than specific steps.</span></span> <span data-ttu-id="3a9bd-189">Můžete také [otevřete žádost o podporu Azure](https://azure.microsoft.com/support/options/) praktických pomoc.</span><span class="sxs-lookup"><span data-stu-id="3a9bd-189">You can also [open an Azure support request](https://azure.microsoft.com/support/options/) for hands-on assistance.</span></span>

