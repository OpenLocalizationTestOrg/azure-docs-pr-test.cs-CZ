---
title: "Správa zásad testovacího prostředí v Azure DevTest Labs | Microsoft Docs"
description: "Naučte se definovat zásady testovacího prostředí, například velikosti virtuálních počítačů, maximální virtuálních počítačů na uživatele a vypnutí automatizace."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 7756aa64-49ca-45a0-9f90-0fd101c7be85
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.author: tarcher
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 328a4d893637d7150807855e118b485a2c3bbfc5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-all-policies-for-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="b0045-103">Správa všech zásad pro testovací prostředí v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="b0045-103">Manage all policies for a lab in Azure DevTest Labs</span></span>

<span data-ttu-id="b0045-104">Azure DevTest Labs umožňuje řídit náklady a minimalizovat odpady ve vaší laboratoře podle Správa zásad (nastavení) pro každý testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="b0045-104">Azure DevTest Labs lets you control cost and minimize waste in your labs by managing policies (settings) for each lab.</span></span> <span data-ttu-id="b0045-105">Tento článek krok za krokem podrobně vysvětluje postup nastavení jednotlivých zásad.</span><span class="sxs-lookup"><span data-stu-id="b0045-105">This article explains in step-by-step detail how to set each policy.</span></span>  

## <a name="set-allowed-virtual-machine-sizes"></a><span data-ttu-id="b0045-106">Sada povolené velikosti virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="b0045-106">Set allowed virtual machine sizes</span></span>
<span data-ttu-id="b0045-107">Zásady pro nastavení povolené velikosti virtuálních počítačů pomáhá minimalizovat odpady testovacího prostředí tím, že vám nastavit, které velikosti virtuálních počítačů jsou povoleny v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b0045-107">The policy for setting the allowed VM sizes helps to minimize lab waste by enabling you to specify which VM sizes are allowed in the lab.</span></span> <span data-ttu-id="b0045-108">Tato zásada je aktivována, lze nastavit pouze velikosti virtuálních počítačů z tohoto seznamu k vytvoření virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="b0045-108">If this policy is activated, only VM sizes from this list can be used to create VMs.</span></span>

1. <span data-ttu-id="b0045-109">V tomto prostředí **konfiguraci a zásady** vyberte **povolené velikosti virtuálních počítačů**.</span><span class="sxs-lookup"><span data-stu-id="b0045-109">On the lab's **Configuration and policies** blade, select **Allowed virtual machines sizes**.</span></span>
   
    ![Velikosti povolené virtuální počítače](./media/devtest-lab-set-lab-policy/allowed-vm-sizes.png)

1. <span data-ttu-id="b0045-111">Vyberte **na** Chcete-li povolit tuto zásadu a **vypnout** ji zakázat.</span><span class="sxs-lookup"><span data-stu-id="b0045-111">Select **On** to enable this policy, and **Off** to disable it.</span></span>

1. <span data-ttu-id="b0045-112">Pokud povolíte tuto zásadu, vyberte jeden nebo více velikosti virtuálních počítačů, které lze vytvořit ve svém testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b0045-112">If you enable this policy, select one or more VM sizes that can be created in your lab.</span></span>

1. <span data-ttu-id="b0045-113">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b0045-113">Select **Save**.</span></span>

## <a name="set-virtual-machines-per-user"></a><span data-ttu-id="b0045-114">Sada virtuálních počítačů na uživatele</span><span class="sxs-lookup"><span data-stu-id="b0045-114">Set virtual machines per user</span></span>
<span data-ttu-id="b0045-115">Zásady pro **virtuálních počítačů na uživatele** umožňuje určit maximální počet virtuálních počítačů, které je možné vytvářet podle jednotlivých uživatelů.</span><span class="sxs-lookup"><span data-stu-id="b0045-115">The policy for **Virtual machines per user** allows you to specify the maximum number of VMs that can be created by an individual user.</span></span> <span data-ttu-id="b0045-116">Pokud se uživatel pokusí o vytvoření nebo deklarací identity virtuálního počítače, když byla splněna limit počtu uživatelů, chybová zpráva označuje, že virtuální počítač nelze vytvořit vyžádaná.</span><span class="sxs-lookup"><span data-stu-id="b0045-116">If a user attempts to create or claim a VM when the user limit has been met, an error message indicates that the VM cannot be created/claimed.</span></span> 

1. <span data-ttu-id="b0045-117">V tomto prostředí **konfiguraci a zásady** nabídce vyberte možnost **virtuálních počítačů na uživatele**.</span><span class="sxs-lookup"><span data-stu-id="b0045-117">On the lab's **Configuration and policies** menu, select **Virtual machines per user**.</span></span>
   
    ![Virtuální počítače na uživatele](./media/devtest-lab-set-lab-policy/max-vms-per-user.png)

1. <span data-ttu-id="b0045-119">Vyberte **Ano** omezit počet virtuálních počítačů na uživatele.</span><span class="sxs-lookup"><span data-stu-id="b0045-119">Select **Yes** to limit the number of VMs per user.</span></span> <span data-ttu-id="b0045-120">Pokud nechcete omezit počet virtuálních počítačů na uživatele, vyberte **ne**.</span><span class="sxs-lookup"><span data-stu-id="b0045-120">If you do not want to limit the number of VMs per user, select **No**.</span></span> <span data-ttu-id="b0045-121">Pokud vyberete **Ano**, zadejte číselnou hodnotu, která určuje maximální počet virtuálních počítačů, které můžou vytvořit nebo jeden uživatel.</span><span class="sxs-lookup"><span data-stu-id="b0045-121">If you select **Yes**, enter a numeric value indicating the maximum number of VMs that can be created or claimed by a user.</span></span> 

1. <span data-ttu-id="b0045-122">Vyberte **Ano** omezit počet virtuálních počítačů, které můžete použít SSD (solid-state disk).</span><span class="sxs-lookup"><span data-stu-id="b0045-122">Select **Yes** to limit the number of VMs that can use SSD (solid-state disk).</span></span> <span data-ttu-id="b0045-123">Pokud nechcete omezit počet virtuálních počítačů, které můžete použít SSD, vyberte **ne**.</span><span class="sxs-lookup"><span data-stu-id="b0045-123">If you do not want to limit the number of VMs that can use SSD, select **No**.</span></span> <span data-ttu-id="b0045-124">Pokud vyberete **Ano**, zadejte hodnotu, která určuje maximální počet virtuálních počítačů, které lze vytvořit pomocí SSD.</span><span class="sxs-lookup"><span data-stu-id="b0045-124">If you select **Yes**, enter a value indicating the maximum number of VMs that can be created using SSD.</span></span> 

1. <span data-ttu-id="b0045-125">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b0045-125">Select **Save**.</span></span>

## <a name="set-virtual-machines-per-lab"></a><span data-ttu-id="b0045-126">Sada virtuálních počítačů na testovacího prostředí</span><span class="sxs-lookup"><span data-stu-id="b0045-126">Set virtual machines per lab</span></span>
<span data-ttu-id="b0045-127">Zásady pro **virtuálních počítačů na testovacím** umožňuje určit maximální počet virtuálních počítačů, které lze vytvořit pro aktuální testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="b0045-127">The policy for **Virtual machines per lab** allows you to specify the maximum number of VMs that can be created for the current lab.</span></span> <span data-ttu-id="b0045-128">Pokud se uživatel pokusí o vytvoření virtuálního počítače, když byla splněna limit testovacího prostředí, chybová zpráva označuje, že virtuální počítač nelze vytvořit.</span><span class="sxs-lookup"><span data-stu-id="b0045-128">If a user attempts to create a VM when the lab limit has been met, an error message indicates that the VM cannot be created.</span></span> 

1. <span data-ttu-id="b0045-129">V tomto prostředí **konfiguraci a zásady** nabídce vyberte možnost **virtuálních počítačů na testovacím**.</span><span class="sxs-lookup"><span data-stu-id="b0045-129">On the lab's **Configuration and policies** menu, select **Virtual machines per lab**.</span></span>
   
    ![Virtuální počítače na testovacího prostředí](./media/devtest-lab-set-lab-policy/max-vms-per-lab.png)

1. <span data-ttu-id="b0045-131">Vyberte **Ano** omezit počet virtuálních počítačů za testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="b0045-131">Select **Yes** to limit the number of VMs per lab.</span></span> <span data-ttu-id="b0045-132">Pokud nechcete omezit počet virtuálních počítačů za testovacího prostředí, vyberte **ne**.</span><span class="sxs-lookup"><span data-stu-id="b0045-132">If you do not want to limit the number of VMs per lab, select **No**.</span></span> <span data-ttu-id="b0045-133">Pokud vyberete **Ano**, zadejte číselnou hodnotu, která určuje maximální počet virtuálních počítačů, které můžou vytvořit nebo jeden uživatel.</span><span class="sxs-lookup"><span data-stu-id="b0045-133">If you select **Yes**, enter a numeric value indicating the maximum number of VMs that can be created or claimed by a user.</span></span> 

1. <span data-ttu-id="b0045-134">Vyberte **Ano** omezit počet virtuálních počítačů, které můžete použít SSD (solid-state disk).</span><span class="sxs-lookup"><span data-stu-id="b0045-134">Select **Yes** to limit the number of VMs that can use SSD (solid-state disk).</span></span> <span data-ttu-id="b0045-135">Pokud nechcete omezit počet virtuálních počítačů, které můžete použít SSD, vyberte **ne**.</span><span class="sxs-lookup"><span data-stu-id="b0045-135">If you do not want to limit the number of VMs that can use SSD, select **No**.</span></span> <span data-ttu-id="b0045-136">Pokud vyberete **Ano**, zadejte hodnotu, která určuje maximální počet virtuálních počítačů, které lze vytvořit pomocí SSD.</span><span class="sxs-lookup"><span data-stu-id="b0045-136">If you select **Yes**, enter a value indicating the maximum number of VMs that can be created using SSD.</span></span> 

1. <span data-ttu-id="b0045-137">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b0045-137">Select **Save**.</span></span>

## <a name="set-auto-shutdown"></a><span data-ttu-id="b0045-138">Sada auto vypnutí</span><span class="sxs-lookup"><span data-stu-id="b0045-138">Set auto-shutdown</span></span>
<span data-ttu-id="b0045-139">Vypnutí automatického zásada pomáhá minimalizovat odpady testovacího prostředí tím, že se vám zadejte dobu, kterou vypnout virtuální počítače v tomto testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b0045-139">The auto-shutdown policy helps to minimize lab waste by allowing you to specify the time that this lab's VMs shut down.</span></span>

1. <span data-ttu-id="b0045-140">V tomto prostředí **konfiguraci a zásady** vyberte **Auto-shutdown**.</span><span class="sxs-lookup"><span data-stu-id="b0045-140">On the lab's **Configuration and policies** blade, select **Auto-shutdown**.</span></span>
   
    ![Vypnutí automatického](./media/devtest-lab-set-lab-policy/auto-shutdown.png)

1. <span data-ttu-id="b0045-142">Vyberte **na** Chcete-li povolit tuto zásadu a **vypnout** ji zakázat.</span><span class="sxs-lookup"><span data-stu-id="b0045-142">Select **On** to enable this policy, and **Off** to disable it.</span></span>

1. <span data-ttu-id="b0045-143">Pokud povolíte tuto zásadu, zadejte dobu (a časového pásma) a ukončí se všechny virtuální počítače v aktuálním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b0045-143">If you enable this policy, specify the time (and time zone) to shut down all VMs in the current lab.</span></span>

1. <span data-ttu-id="b0045-144">Zadejte **Ano** nebo **ne** pro možnost Odeslat oznámení 15 minut před časem zadaný auto vypnutí.</span><span class="sxs-lookup"><span data-stu-id="b0045-144">Specify **Yes** or **No** for the option to send a notification 15 minutes prior to the specified auto-shutdown time.</span></span> <span data-ttu-id="b0045-145">Pokud zadáte **Ano**, zadejte koncový bod adresy URL webhooku pro příjem oznámení.</span><span class="sxs-lookup"><span data-stu-id="b0045-145">If you specify **Yes**, enter a webhook URL endpoint to receive the notification.</span></span> <span data-ttu-id="b0045-146">Další informace o webhooky najdete v tématu [vytvoření webhooku nebo funkce rozhraní API Azure](../azure-functions/functions-create-a-web-hook-or-api-function.md).</span><span class="sxs-lookup"><span data-stu-id="b0045-146">For more information about webhooks, see [Create a webhook or API Azure Function](../azure-functions/functions-create-a-web-hook-or-api-function.md).</span></span> 

1. <span data-ttu-id="b0045-147">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b0045-147">Select **Save**.</span></span>

    <span data-ttu-id="b0045-148">Ve výchozím nastavení, jakmile bude povoleno, tato zásada platí pro všechny virtuální počítače v aktuálním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b0045-148">By default, once enabled, this policy applies to all VMs in the current lab.</span></span> <span data-ttu-id="b0045-149">Chcete-li toto nastavení odebrat z konkrétní virtuální počítač, otevřete okno Virtuálního počítače a změňte jeho **Auto-shutdown** nastavení</span><span class="sxs-lookup"><span data-stu-id="b0045-149">To remove this setting from a specific VM, open the VM's blade and change its **Auto-shutdown** setting</span></span> 

## <a name="set-auto-start"></a><span data-ttu-id="b0045-150">Sada automatického – spuštění</span><span class="sxs-lookup"><span data-stu-id="b0045-150">Set auto-start</span></span>
<span data-ttu-id="b0045-151">Automaticky spouštěná zásady umožňuje zadat při virtuálních počítačů v aktuálním prostředí by měl být spuštěn.</span><span class="sxs-lookup"><span data-stu-id="b0045-151">The auto-start policy allows you to specify when the VMs in the current lab should be started.</span></span>  

1. <span data-ttu-id="b0045-152">V tomto prostředí **konfiguraci a zásady** vyberte **automatického spuštění**.</span><span class="sxs-lookup"><span data-stu-id="b0045-152">On the lab's **Configuration and policies** blade, select **Auto-start**.</span></span>
   
    ![Automatické spuštění](./media/devtest-lab-set-lab-policy/auto-start.png)

2. <span data-ttu-id="b0045-154">Vyberte **na** Chcete-li povolit tuto zásadu a **vypnout** ji zakázat.</span><span class="sxs-lookup"><span data-stu-id="b0045-154">Select **On** to enable this policy, and **Off** to disable it.</span></span>

3. <span data-ttu-id="b0045-155">Pokud povolíte tuto zásadu, zadejte naplánovaný čas zahájení, časové pásmo a dny v týdnu, pro kterou platí čas.</span><span class="sxs-lookup"><span data-stu-id="b0045-155">If you enable this policy, specify the scheduled start time, time zone, and the days of the week for which the time applies.</span></span> 

4. <span data-ttu-id="b0045-156">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b0045-156">Select **Save**.</span></span>

    <span data-ttu-id="b0045-157">Jakmile bude povoleno, není tato zásada použitá automaticky všechny virtuální počítače v aktuálním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b0045-157">Once enabled, this policy is not automatically applied to any VMs in the current lab.</span></span> <span data-ttu-id="b0045-158">Chcete-li použít tato nastavení pro konkrétní virtuální počítač, otevřete okno Virtuálního počítače a změňte jeho **automatického spuštění** nastavení</span><span class="sxs-lookup"><span data-stu-id="b0045-158">To apply this setting to a specific VM, open the VM's blade and change its **Auto-start** setting</span></span> 

## <a name="set-expiration-date"></a><span data-ttu-id="b0045-159">Nastavit datum vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="b0045-159">Set expiration date</span></span>
<span data-ttu-id="b0045-160">Můžete nastavit vypršení platnosti datum, kdy jste [vytvořit virtuální počítač](devtest-lab-add-vm.md).</span><span class="sxs-lookup"><span data-stu-id="b0045-160">You can set an expiration date when you [create the VM](devtest-lab-add-vm.md).</span></span> <span data-ttu-id="b0045-161">V **upřesňující nastavení**, zvolte ikonu Kalendář zadat datum, na kterém virtuální počítač se automaticky odstraní.</span><span class="sxs-lookup"><span data-stu-id="b0045-161">In **Advanced settings**, choose the calendar icon to specify a date on which the VM will be automatically deleted.</span></span>  <span data-ttu-id="b0045-162">Ve výchozím nastavení virtuální počítač nikdy nevyprší.</span><span class="sxs-lookup"><span data-stu-id="b0045-162">By default, the VM will never expire.</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a><span data-ttu-id="b0045-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b0045-163">Next steps</span></span>
<span data-ttu-id="b0045-164">Jakmile jste definovaný a použít různá nastavení zásad virtuálních počítačů pro testovací prostředí, zde jsou další vyzkoušejte:</span><span class="sxs-lookup"><span data-stu-id="b0045-164">Once you've defined and applied the various VM policy settings for your lab, here are some things to try next:</span></span>

* <span data-ttu-id="b0045-165">[Pochopení sdílené IP adresy](devtest-lab-shared-ip.md) -vysvětluje, jak sdílené IP adresy se používají v DevTest Labs, chcete-li minimalizovat počet veřejných IP adres, které jsou požadované pro připojení k testovacího prostředí virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="b0045-165">[Understand shared IP addresses](devtest-lab-shared-ip.md) - Explains how shared IP addresses are used in DevTest Labs to minimize the number of public IP addresses required to connect to your lab VMs.</span></span>
* <span data-ttu-id="b0045-166">[Konfigurovat náklady na správu](devtest-lab-configure-cost-management.md) -ukazuje, jak používat **měsíční Trend odhadované náklady na** grafu</span><span class="sxs-lookup"><span data-stu-id="b0045-166">[Configure cost management](devtest-lab-configure-cost-management.md) - Illustrates how to use the **Monthly Estimated Cost Trend** chart</span></span>  
  <span data-ttu-id="b0045-167">Chcete-li zobrazit aktuální měsíc je odhadované náklady na začátku a předpokládané náklady na koncový měsíc.</span><span class="sxs-lookup"><span data-stu-id="b0045-167">to view the current month's estimated cost-to-date and the projected end-of-month cost.</span></span>
* <span data-ttu-id="b0045-168">[Vytvořit vlastní image](devtest-lab-create-template.md) – při vytváření virtuálního počítače, zadejte základ, který může být buď vlastní image nebo image pořízenou prostřednictvím Marketplace.</span><span class="sxs-lookup"><span data-stu-id="b0045-168">[Create custom image](devtest-lab-create-template.md) - When you create a VM, you specify a base, which can be either a custom image or a Marketplace image.</span></span> <span data-ttu-id="b0045-169">Tento článek ukazuje, jak vytvořit vlastní obrázek ze souboru virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="b0045-169">This article illustrates how to create a custom image from a VHD file.</span></span>
* <span data-ttu-id="b0045-170">[Konfigurace Marketplace image](devtest-lab-configure-marketplace-images.md) – Azure DevTest Labs podporuje vytváření virtuálních počítačů založené na imagích Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="b0045-170">[Configure Marketplace images](devtest-lab-configure-marketplace-images.md) - Azure DevTest Labs supports creating VMs based on Azure Marketplace images.</span></span> <span data-ttu-id="b0045-171">Tento článek ukazuje, jak určit, které, pokud existuje, může být Azure Marketplace Image používá při vytváření virtuálních počítačů v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b0045-171">This article illustrates how to specify which, if any, Azure Marketplace images can be used when creating VMs in a lab.</span></span>
* <span data-ttu-id="b0045-172">[Vytvoření virtuálního počítače v testovacím prostředí](devtest-lab-add-vm-with-artifacts.md) -znázorňuje, jak vytvořit virtuální počítač z bitové kopie základní (buď vlastní nebo Marketplace) a postup práce s artefakty ve vašem virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="b0045-172">[Create a VM in a lab](devtest-lab-add-vm-with-artifacts.md) - Illustrates how to create a VM from a base image (either custom or Marketplace), and how to work with artifacts in your VM.</span></span>

