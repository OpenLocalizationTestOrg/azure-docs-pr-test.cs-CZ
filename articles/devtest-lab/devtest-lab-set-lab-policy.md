---
title: "zásady aaaManage testovacího prostředí v Azure DevTest Labs | Microsoft Docs"
description: "Další informace o velikosti zásad testovacího toodefine například virtuálních počítačů, maximální virtuálních počítačů na uživatele a vypnutí automatizace."
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
ms.openlocfilehash: 351b3645a1fd729455884e5d177877c2986bd853
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-all-policies-for-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="fa5fb-103">Správa všech zásad pro testovací prostředí v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="fa5fb-103">Manage all policies for a lab in Azure DevTest Labs</span></span>

<span data-ttu-id="fa5fb-104">Azure DevTest Labs umožňuje řídit náklady a minimalizovat odpady ve vaší laboratoře podle Správa zásad (nastavení) pro každý testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-104">Azure DevTest Labs lets you control cost and minimize waste in your labs by managing policies (settings) for each lab.</span></span> <span data-ttu-id="fa5fb-105">Tento článek krok za krokem podrobně vysvětluje, jak tooset každou zásadu.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-105">This article explains in step-by-step detail how tooset each policy.</span></span>  

## <a name="set-allowed-virtual-machine-sizes"></a><span data-ttu-id="fa5fb-106">Sada povolené velikosti virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="fa5fb-106">Set allowed virtual machine sizes</span></span>
<span data-ttu-id="fa5fb-107">Hello zásady pro nastavení hello povolené velikosti virtuálních počítačů pomáhá toominimize testovacím nakládání s tím, že vám toospecify jsou povoleny které velikosti virtuálních počítačů v testovacím prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-107">hello policy for setting hello allowed VM sizes helps toominimize lab waste by enabling you toospecify which VM sizes are allowed in hello lab.</span></span> <span data-ttu-id="fa5fb-108">Pokud tato zásada je aktivována, může být pouze velikosti virtuálních počítačů z tohoto seznamu použité toocreate virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-108">If this policy is activated, only VM sizes from this list can be used toocreate VMs.</span></span>

1. <span data-ttu-id="fa5fb-109">V testovacím hello **konfiguraci a zásady** vyberte **povolené velikosti virtuálních počítačů**.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-109">On hello lab's **Configuration and policies** blade, select **Allowed virtual machines sizes**.</span></span>
   
    ![Velikosti povolené virtuální počítače](./media/devtest-lab-set-lab-policy/allowed-vm-sizes.png)

1. <span data-ttu-id="fa5fb-111">Vyberte **na** tooenable tuto zásadu a **vypnout** toodisable ho.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-111">Select **On** tooenable this policy, and **Off** toodisable it.</span></span>

1. <span data-ttu-id="fa5fb-112">Pokud povolíte tuto zásadu, vyberte jeden nebo více velikosti virtuálních počítačů, které lze vytvořit ve svém testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-112">If you enable this policy, select one or more VM sizes that can be created in your lab.</span></span>

1. <span data-ttu-id="fa5fb-113">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-113">Select **Save**.</span></span>

## <a name="set-virtual-machines-per-user"></a><span data-ttu-id="fa5fb-114">Sada virtuálních počítačů na uživatele</span><span class="sxs-lookup"><span data-stu-id="fa5fb-114">Set virtual machines per user</span></span>
<span data-ttu-id="fa5fb-115">Hello zásady pro **virtuálních počítačů na uživatele** vám umožní toospecify hello maximální počet virtuálních počítačů, které je možné vytvářet podle jednotlivých uživatelů.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-115">hello policy for **Virtual machines per user** allows you toospecify hello maximum number of VMs that can be created by an individual user.</span></span> <span data-ttu-id="fa5fb-116">Pokud se uživatel pokusí toocreate nebo deklarace identity virtuálního počítače při byla splněna hello limit počtu uživatelů, chybová zpráva označuje tento virtuální počítač nelze vytvořit nebo žádat hello.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-116">If a user attempts toocreate or claim a VM when hello user limit has been met, an error message indicates that hello VM cannot be created/claimed.</span></span> 

1. <span data-ttu-id="fa5fb-117">V testovacím hello **konfiguraci a zásady** nabídce vyberte možnost **virtuálních počítačů na uživatele**.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-117">On hello lab's **Configuration and policies** menu, select **Virtual machines per user**.</span></span>
   
    ![Virtuální počítače na uživatele](./media/devtest-lab-set-lab-policy/max-vms-per-user.png)

1. <span data-ttu-id="fa5fb-119">Vyberte **Ano** toolimit hello počet virtuálních počítačů na uživatele.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-119">Select **Yes** toolimit hello number of VMs per user.</span></span> <span data-ttu-id="fa5fb-120">Pokud nechcete, aby toolimit hello počet virtuálních počítačů na uživatele, vyberte **ne**.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-120">If you do not want toolimit hello number of VMs per user, select **No**.</span></span> <span data-ttu-id="fa5fb-121">Pokud vyberete **Ano**, zadejte číselnou hodnotu, která určuje maximální počet virtuálních počítačů, které můžou vytvořit nebo jeden uživatel hello.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-121">If you select **Yes**, enter a numeric value indicating hello maximum number of VMs that can be created or claimed by a user.</span></span> 

1. <span data-ttu-id="fa5fb-122">Vyberte **Ano** toolimit hello počet virtuálních počítačů, které můžete použít SSD (solid-state disk).</span><span class="sxs-lookup"><span data-stu-id="fa5fb-122">Select **Yes** toolimit hello number of VMs that can use SSD (solid-state disk).</span></span> <span data-ttu-id="fa5fb-123">Pokud nechcete, aby toolimit hello počet virtuálních počítačů, které můžete použít SSD, vyberte **ne**.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-123">If you do not want toolimit hello number of VMs that can use SSD, select **No**.</span></span> <span data-ttu-id="fa5fb-124">Pokud vyberete **Ano**, zadejte hodnotu, která určuje maximální počet virtuálních počítačů, které lze vytvořit pomocí SSD hello.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-124">If you select **Yes**, enter a value indicating hello maximum number of VMs that can be created using SSD.</span></span> 

1. <span data-ttu-id="fa5fb-125">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-125">Select **Save**.</span></span>

## <a name="set-virtual-machines-per-lab"></a><span data-ttu-id="fa5fb-126">Sada virtuálních počítačů na testovacího prostředí</span><span class="sxs-lookup"><span data-stu-id="fa5fb-126">Set virtual machines per lab</span></span>
<span data-ttu-id="fa5fb-127">Hello zásady pro **virtuálních počítačů na testovacím** vám umožní toospecify hello maximální počet virtuálních počítačů, které lze vytvořit pro aktuální prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-127">hello policy for **Virtual machines per lab** allows you toospecify hello maximum number of VMs that can be created for hello current lab.</span></span> <span data-ttu-id="fa5fb-128">Pokud se uživatel pokusí toocreate virtuálního počítače při byla splněna limit hello testovacího prostředí, chybovou zprávu označuje, že hello virtuální počítač nelze vytvořit.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-128">If a user attempts toocreate a VM when hello lab limit has been met, an error message indicates that hello VM cannot be created.</span></span> 

1. <span data-ttu-id="fa5fb-129">V testovacím hello **konfiguraci a zásady** nabídce vyberte možnost **virtuálních počítačů na testovacím**.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-129">On hello lab's **Configuration and policies** menu, select **Virtual machines per lab**.</span></span>
   
    ![Virtuální počítače na testovacího prostředí](./media/devtest-lab-set-lab-policy/max-vms-per-lab.png)

1. <span data-ttu-id="fa5fb-131">Vyberte **Ano** toolimit hello počet virtuálních počítačů za testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-131">Select **Yes** toolimit hello number of VMs per lab.</span></span> <span data-ttu-id="fa5fb-132">Pokud nechcete, aby toolimit hello počet virtuálních počítačů za testovacího prostředí, vyberte **ne**.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-132">If you do not want toolimit hello number of VMs per lab, select **No**.</span></span> <span data-ttu-id="fa5fb-133">Pokud vyberete **Ano**, zadejte číselnou hodnotu, která určuje maximální počet virtuálních počítačů, které můžou vytvořit nebo jeden uživatel hello.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-133">If you select **Yes**, enter a numeric value indicating hello maximum number of VMs that can be created or claimed by a user.</span></span> 

1. <span data-ttu-id="fa5fb-134">Vyberte **Ano** toolimit hello počet virtuálních počítačů, které můžete použít SSD (solid-state disk).</span><span class="sxs-lookup"><span data-stu-id="fa5fb-134">Select **Yes** toolimit hello number of VMs that can use SSD (solid-state disk).</span></span> <span data-ttu-id="fa5fb-135">Pokud nechcete, aby toolimit hello počet virtuálních počítačů, které můžete použít SSD, vyberte **ne**.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-135">If you do not want toolimit hello number of VMs that can use SSD, select **No**.</span></span> <span data-ttu-id="fa5fb-136">Pokud vyberete **Ano**, zadejte hodnotu, která určuje maximální počet virtuálních počítačů, které lze vytvořit pomocí SSD hello.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-136">If you select **Yes**, enter a value indicating hello maximum number of VMs that can be created using SSD.</span></span> 

1. <span data-ttu-id="fa5fb-137">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-137">Select **Save**.</span></span>

## <a name="set-auto-shutdown"></a><span data-ttu-id="fa5fb-138">Sada auto vypnutí</span><span class="sxs-lookup"><span data-stu-id="fa5fb-138">Set auto-shutdown</span></span>
<span data-ttu-id="fa5fb-139">vypnutí automatického zásada Hello pomáhá toominimize testovacím nakládání s tím, že jste toospecify hello dobu, po kterou vypnout virtuální počítače v tomto testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-139">hello auto-shutdown policy helps toominimize lab waste by allowing you toospecify hello time that this lab's VMs shut down.</span></span>

1. <span data-ttu-id="fa5fb-140">V testovacím hello **konfiguraci a zásady** vyberte **Auto-shutdown**.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-140">On hello lab's **Configuration and policies** blade, select **Auto-shutdown**.</span></span>
   
    ![Vypnutí automatického](./media/devtest-lab-set-lab-policy/auto-shutdown.png)

1. <span data-ttu-id="fa5fb-142">Vyberte **na** tooenable tuto zásadu a **vypnout** toodisable ho.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-142">Select **On** tooenable this policy, and **Off** toodisable it.</span></span>

1. <span data-ttu-id="fa5fb-143">Pokud povolíte tuto zásadu, zadejte hello (a časové pásmo) tooshut dolů všech virtuálních počítačů v testovacím prostředí aktuální hello.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-143">If you enable this policy, specify hello time (and time zone) tooshut down all VMs in hello current lab.</span></span>

1. <span data-ttu-id="fa5fb-144">Zadejte **Ano** nebo **ne** pro toosend možnost hello předchozí toohello oznámení 15 minut zadaný čas ukončení automaticky.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-144">Specify **Yes** or **No** for hello option toosend a notification 15 minutes prior toohello specified auto-shutdown time.</span></span> <span data-ttu-id="fa5fb-145">Pokud zadáte **Ano**, zadejte adresu URL webhooku koncový bod tooreceive hello oznámení.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-145">If you specify **Yes**, enter a webhook URL endpoint tooreceive hello notification.</span></span> <span data-ttu-id="fa5fb-146">Další informace o webhooky najdete v tématu [vytvoření webhooku nebo funkce rozhraní API Azure](../azure-functions/functions-create-a-web-hook-or-api-function.md).</span><span class="sxs-lookup"><span data-stu-id="fa5fb-146">For more information about webhooks, see [Create a webhook or API Azure Function](../azure-functions/functions-create-a-web-hook-or-api-function.md).</span></span> 

1. <span data-ttu-id="fa5fb-147">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-147">Select **Save**.</span></span>

    <span data-ttu-id="fa5fb-148">Ve výchozím nastavení, až bude povolená tato zásada platí tooall virtuálních počítačů v testovacím prostředí aktuální hello.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-148">By default, once enabled, this policy applies tooall VMs in hello current lab.</span></span> <span data-ttu-id="fa5fb-149">tooremove toto nastavení z konkrétní virtuální počítač, otevřete okno hello Virtuálního počítače a změňte jeho **Auto-shutdown** nastavení</span><span class="sxs-lookup"><span data-stu-id="fa5fb-149">tooremove this setting from a specific VM, open hello VM's blade and change its **Auto-shutdown** setting</span></span> 

## <a name="set-auto-start"></a><span data-ttu-id="fa5fb-150">Sada automatického – spuštění</span><span class="sxs-lookup"><span data-stu-id="fa5fb-150">Set auto-start</span></span>
<span data-ttu-id="fa5fb-151">Zásady automatické spuštění Hello povoluje toospecify při hello virtuálních počítačů v testovacím prostředí aktuální hello by měl být spuštěn.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-151">hello auto-start policy allows you toospecify when hello VMs in hello current lab should be started.</span></span>  

1. <span data-ttu-id="fa5fb-152">V testovacím hello **konfiguraci a zásady** vyberte **automatického spuštění**.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-152">On hello lab's **Configuration and policies** blade, select **Auto-start**.</span></span>
   
    ![Automatické spuštění](./media/devtest-lab-set-lab-policy/auto-start.png)

2. <span data-ttu-id="fa5fb-154">Vyberte **na** tooenable tuto zásadu a **vypnout** toodisable ho.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-154">Select **On** tooenable this policy, and **Off** toodisable it.</span></span>

3. <span data-ttu-id="fa5fb-155">Pokud povolíte tuto zásadu, zadejte hello naplánovaný čas zahájení, časové pásmo a hello dny v týdnu hello, pro které hello doba platí.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-155">If you enable this policy, specify hello scheduled start time, time zone, and hello days of hello week for which hello time applies.</span></span> 

4. <span data-ttu-id="fa5fb-156">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-156">Select **Save**.</span></span>

    <span data-ttu-id="fa5fb-157">Jakmile bude povoleno, tato zásada není automaticky použité tooany virtuálních počítačů v testovacím prostředí aktuální hello.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-157">Once enabled, this policy is not automatically applied tooany VMs in hello current lab.</span></span> <span data-ttu-id="fa5fb-158">tooapply tato nastavení tooa konkrétní virtuální počítač, okno otevřít hello Virtuálního počítače a změňte jeho **automatického spuštění** nastavení</span><span class="sxs-lookup"><span data-stu-id="fa5fb-158">tooapply this setting tooa specific VM, open hello VM's blade and change its **Auto-start** setting</span></span> 

## <a name="set-expiration-date"></a><span data-ttu-id="fa5fb-159">Nastavit datum vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="fa5fb-159">Set expiration date</span></span>
<span data-ttu-id="fa5fb-160">Můžete nastavit vypršení platnosti datum, kdy jste [vytvořit hello virtuálních počítačů](devtest-lab-add-vm.md).</span><span class="sxs-lookup"><span data-stu-id="fa5fb-160">You can set an expiration date when you [create hello VM](devtest-lab-add-vm.md).</span></span> <span data-ttu-id="fa5fb-161">V **upřesňující nastavení**, zvolte hello kalendáře ikonu toospecify datum, na kterém hello virtuální počítač se automaticky odstraní.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-161">In **Advanced settings**, choose hello calendar icon toospecify a date on which hello VM will be automatically deleted.</span></span>  <span data-ttu-id="fa5fb-162">Ve výchozím nastavení hello virtuálních počítačů nikdy nevyprší.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-162">By default, hello VM will never expire.</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a><span data-ttu-id="fa5fb-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fa5fb-163">Next steps</span></span>
<span data-ttu-id="fa5fb-164">Jakmile jste definovány a použity hello různá nastavení zásad virtuálních počítačů pro testovací prostředí, tady jsou některé věci tootry dále:</span><span class="sxs-lookup"><span data-stu-id="fa5fb-164">Once you've defined and applied hello various VM policy settings for your lab, here are some things tootry next:</span></span>

* <span data-ttu-id="fa5fb-165">[Pochopení sdílené IP adresy](devtest-lab-shared-ip.md) -vysvětluje, jak sdílené IP adresy se používají v DevTest Labs toominimize hello počet veřejných IP adres vyžaduje tooconnect tooyour prostředí virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-165">[Understand shared IP addresses](devtest-lab-shared-ip.md) - Explains how shared IP addresses are used in DevTest Labs toominimize hello number of public IP addresses required tooconnect tooyour lab VMs.</span></span>
* <span data-ttu-id="fa5fb-166">[Konfigurovat náklady na správu](devtest-lab-configure-cost-management.md) -ukazuje, jak toouse hello **měsíční odhadované náklady Trend** grafu</span><span class="sxs-lookup"><span data-stu-id="fa5fb-166">[Configure cost management](devtest-lab-configure-cost-management.md) - Illustrates how toouse hello **Monthly Estimated Cost Trend** chart</span></span>  
  <span data-ttu-id="fa5fb-167">tooview hello aktuálního měsíce odhadované náklady na začátku a náklady na koncový měsíc hello k projekci.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-167">tooview hello current month's estimated cost-to-date and hello projected end-of-month cost.</span></span>
* <span data-ttu-id="fa5fb-168">[Vytvořit vlastní image](devtest-lab-create-template.md) – při vytváření virtuálního počítače, zadejte základ, který může být buď vlastní image nebo image pořízenou prostřednictvím Marketplace.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-168">[Create custom image](devtest-lab-create-template.md) - When you create a VM, you specify a base, which can be either a custom image or a Marketplace image.</span></span> <span data-ttu-id="fa5fb-169">Tento článek ukazuje, jak toocreate vlastní obrázek z soubor virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-169">This article illustrates how toocreate a custom image from a VHD file.</span></span>
* <span data-ttu-id="fa5fb-170">[Konfigurace Marketplace image](devtest-lab-configure-marketplace-images.md) – Azure DevTest Labs podporuje vytváření virtuálních počítačů založené na imagích Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-170">[Configure Marketplace images](devtest-lab-configure-marketplace-images.md) - Azure DevTest Labs supports creating VMs based on Azure Marketplace images.</span></span> <span data-ttu-id="fa5fb-171">Tento článek ukazuje, jak toospecify, který, pokud existuje, Azure Marketplace bitové kopie lze použít při vytváření virtuálních počítačů v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-171">This article illustrates how toospecify which, if any, Azure Marketplace images can be used when creating VMs in a lab.</span></span>
* <span data-ttu-id="fa5fb-172">[Vytvoření virtuálního počítače v testovacím prostředí](devtest-lab-add-vm-with-artifacts.md) -ukazuje, jak toocreate virtuální počítač z bitové kopie základní (buď vlastní nebo Marketplace) a jak toowork s artefakty ve vašem virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="fa5fb-172">[Create a VM in a lab](devtest-lab-add-vm-with-artifacts.md) - Illustrates how toocreate a VM from a base image (either custom or Marketplace), and how toowork with artifacts in your VM.</span></span>

