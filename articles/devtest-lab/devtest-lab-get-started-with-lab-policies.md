---
title: "zásady aaaManage základní testovacího prostředí v Azure DevTest Labs | Microsoft Docs"
description: "Zjistěte, jak tooset některé základní zásady hello (nastavení) pro testovací prostředí v DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: tarcher
ms.openlocfilehash: 792c0d19cfe73964be9c03d3de7751a868fd86e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-basic-policies-for-a-lab-in-azure-devtest-labs"></a><span data-ttu-id="0c30d-103">Spravovat základní zásady pro testovací prostředí v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="0c30d-103">Manage basic policies for a lab in Azure DevTest Labs</span></span>

<span data-ttu-id="0c30d-104">Azure DevTest Labs vám umožní toocontrol náklady a minimalizovat odpady ve vaší laboratoře podle Správa zásad (nastavení) pro každý testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="0c30d-104">Azure DevTest Labs enables you toocontrol cost and minimize waste in your labs by managing policies (settings) for each lab.</span></span> <span data-ttu-id="0c30d-105">V tomto článku jste Začínáme se zásadami podle učení jak tooset dva nejdůležitější zásady hello – omezení hello počet virtuálních počítačů (VM), které můžou vytvořit nebo nárokován jednoho uživatele a konfiguraci automatického vypnutí.</span><span class="sxs-lookup"><span data-stu-id="0c30d-105">In this article, you get started with policies by learning how tooset two of hello most critical policies - limiting hello number of virtual machines (VM) that can be created or claimed by a single user, and configuring auto-shutdown.</span></span> <span data-ttu-id="0c30d-106">tooview jak tooset každé zásadě testovacího prostředí, najdete v článku hello [definovat zásady testovacího prostředí v Azure DevTest Labs](devtest-lab-set-lab-policy.md).</span><span class="sxs-lookup"><span data-stu-id="0c30d-106">tooview how tooset every lab policy, see hello article, [Define lab policies in Azure DevTest Labs](devtest-lab-set-lab-policy.md).</span></span>  

## <a name="accessing-a-labs-policies-in-azure-devtest-labs"></a><span data-ttu-id="0c30d-107">Přístup k zásady testovacího prostředí v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="0c30d-107">Accessing a lab's policies in Azure DevTest Labs</span></span>
<span data-ttu-id="0c30d-108">Hello následující postup vás provede nastavením zásad pro testovací prostředí v Azure DevTest Labs:</span><span class="sxs-lookup"><span data-stu-id="0c30d-108">hello following steps guide you through setting up policies for a lab in Azure DevTest Labs:</span></span>

<span data-ttu-id="0c30d-109">tooview (a změňte) hello zásady pro testovací prostředí, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="0c30d-109">tooview (and change) hello policies for a lab, follow these steps:</span></span>

1. <span data-ttu-id="0c30d-110">Přihlaste se toohello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="0c30d-110">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

1. <span data-ttu-id="0c30d-111">Vyberte **další služby**a potom vyberte **DevTest Labs** hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="0c30d-111">Select **More services**, and then select **DevTest Labs** from hello list.</span></span>

1. <span data-ttu-id="0c30d-112">Ze seznamu hello labs vyberte požadované prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="0c30d-112">From hello list of labs, select hello desired lab.</span></span>   

1. <span data-ttu-id="0c30d-113">Vyberte **konfiguraci a zásady**.</span><span class="sxs-lookup"><span data-stu-id="0c30d-113">Select **Configuration and policies**.</span></span>

    ![Okna nastavení zásad](./media/devtest-lab-set-lab-policy/policies-menu.png)

1. <span data-ttu-id="0c30d-115">Hello **konfiguraci a zásady** okno obsahuje nabídky nastavení, které můžete zadat.</span><span class="sxs-lookup"><span data-stu-id="0c30d-115">hello **Configuration and policies** blade contains a menu of settings that you can specify.</span></span> <span data-ttu-id="0c30d-116">Tento článek se týká pouze hello nastavení pro **virtuálních počítačů na uživatele** a **Auto-shutdown**.</span><span class="sxs-lookup"><span data-stu-id="0c30d-116">This article covers only hello settings for **Virtual machines per user** and **Auto-shutdown**.</span></span> <span data-ttu-id="0c30d-117">toolearn o hello zbývající nastavení, najdete v části [Správa všech zásad pro testovací prostředí v Azure DevTest Labs](./devtest-lab-set-lab-policy.md).</span><span class="sxs-lookup"><span data-stu-id="0c30d-117">toolearn about hello remaining settings, see [Manage all policies for a lab in Azure DevTest Labs](./devtest-lab-set-lab-policy.md).</span></span> 
   
## <a name="set-virtual-machines-per-user"></a><span data-ttu-id="0c30d-118">Sada virtuálních počítačů na uživatele</span><span class="sxs-lookup"><span data-stu-id="0c30d-118">Set virtual machines per user</span></span>
<span data-ttu-id="0c30d-119">Hello zásady pro **virtuálních počítačů na uživatele** vám umožní toospecify hello maximální počet virtuálních počítačů, které je možné vytvářet podle jednotlivých uživatelů.</span><span class="sxs-lookup"><span data-stu-id="0c30d-119">hello policy for **Virtual machines per user** allows you toospecify hello maximum number of VMs that can be created by an individual user.</span></span> <span data-ttu-id="0c30d-120">Pokud se uživatel pokusí toocreate nebo deklarace identity virtuálního počítače při byla splněna hello limit počtu uživatelů, chybová zpráva označuje tento virtuální počítač nelze vytvořit nebo žádat hello.</span><span class="sxs-lookup"><span data-stu-id="0c30d-120">If a user attempts toocreate or claim a VM when hello user limit has been met, an error message indicates that hello VM cannot be created/claimed.</span></span> 

1. <span data-ttu-id="0c30d-121">V testovacím hello **konfiguraci a zásady** nabídce vyberte možnost **virtuálních počítačů na uživatele**.</span><span class="sxs-lookup"><span data-stu-id="0c30d-121">On hello lab's **Configuration and policies** menu, select **Virtual machines per user**.</span></span>
   
    ![Virtuální počítače na uživatele](./media/devtest-lab-set-lab-policy/max-vms-per-user.png)

1. <span data-ttu-id="0c30d-123">Vyberte **Ano** toolimit hello počet virtuálních počítačů na uživatele.</span><span class="sxs-lookup"><span data-stu-id="0c30d-123">Select **Yes** toolimit hello number of VMs per user.</span></span> <span data-ttu-id="0c30d-124">Pokud nechcete, aby toolimit hello počet virtuálních počítačů na uživatele, vyberte **ne**.</span><span class="sxs-lookup"><span data-stu-id="0c30d-124">If you do not want toolimit hello number of VMs per user, select **No**.</span></span> <span data-ttu-id="0c30d-125">Pokud vyberete **Ano**, zadejte číselnou hodnotu, která určuje maximální počet virtuálních počítačů, které můžou vytvořit nebo jeden uživatel hello.</span><span class="sxs-lookup"><span data-stu-id="0c30d-125">If you select **Yes**, enter a numeric value indicating hello maximum number of VMs that can be created or claimed by a user.</span></span> 

1. <span data-ttu-id="0c30d-126">Vyberte **Ano** toolimit hello počet virtuálních počítačů, které můžete použít SSD (solid-state disk).</span><span class="sxs-lookup"><span data-stu-id="0c30d-126">Select **Yes** toolimit hello number of VMs that can use SSD (solid-state disk).</span></span> <span data-ttu-id="0c30d-127">Pokud nechcete, aby toolimit hello počet virtuálních počítačů, které můžete použít SSD, vyberte **ne**.</span><span class="sxs-lookup"><span data-stu-id="0c30d-127">If you do not want toolimit hello number of VMs that can use SSD, select **No**.</span></span> <span data-ttu-id="0c30d-128">Pokud vyberete **Ano**, zadejte hodnotu, která určuje maximální počet virtuálních počítačů, které lze vytvořit pomocí SSD hello.</span><span class="sxs-lookup"><span data-stu-id="0c30d-128">If you select **Yes**, enter a value indicating hello maximum number of VMs that can be created using SSD.</span></span> 

1. <span data-ttu-id="0c30d-129">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="0c30d-129">Select **Save**.</span></span>

## <a name="set-auto-shutdown"></a><span data-ttu-id="0c30d-130">Sada auto vypnutí</span><span class="sxs-lookup"><span data-stu-id="0c30d-130">Set auto-shutdown</span></span>
<span data-ttu-id="0c30d-131">vypnutí automatického zásada Hello pomáhá toominimize testovacím nakládání s tím, že jste toospecify hello dobu, po kterou vypnout virtuální počítače v tomto testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="0c30d-131">hello auto-shutdown policy helps toominimize lab waste by allowing you toospecify hello time that this lab's VMs shut down.</span></span>

1. <span data-ttu-id="0c30d-132">V testovacím hello **konfiguraci a zásady** vyberte **Auto-shutdown**.</span><span class="sxs-lookup"><span data-stu-id="0c30d-132">On hello lab's **Configuration and policies** blade, select **Auto-shutdown**.</span></span>
   
    ![Vypnutí automatického](./media/devtest-lab-set-lab-policy/auto-shutdown.png)

1. <span data-ttu-id="0c30d-134">Vyberte **na** tooenable tuto zásadu a **vypnout** toodisable ho.</span><span class="sxs-lookup"><span data-stu-id="0c30d-134">Select **On** tooenable this policy, and **Off** toodisable it.</span></span>

1. <span data-ttu-id="0c30d-135">Pokud povolíte tuto zásadu, zadejte hello (a časové pásmo) tooshut dolů všech virtuálních počítačů v testovacím prostředí aktuální hello.</span><span class="sxs-lookup"><span data-stu-id="0c30d-135">If you enable this policy, specify hello time (and time zone) tooshut down all VMs in hello current lab.</span></span>

1. <span data-ttu-id="0c30d-136">Zadejte **Ano** nebo **ne** pro toosend možnost hello předchozí toohello oznámení 15 minut zadaný čas ukončení automaticky.</span><span class="sxs-lookup"><span data-stu-id="0c30d-136">Specify **Yes** or **No** for hello option toosend a notification 15 minutes prior toohello specified auto-shutdown time.</span></span> <span data-ttu-id="0c30d-137">Pokud zadáte **Ano**, zadejte adresu URL webhooku koncový bod tooreceive hello oznámení.</span><span class="sxs-lookup"><span data-stu-id="0c30d-137">If you specify **Yes**, enter a webhook URL endpoint tooreceive hello notification.</span></span> <span data-ttu-id="0c30d-138">Další informace o webhooky najdete v tématu [vytvoření webhooku nebo funkce rozhraní API Azure](../azure-functions/functions-create-a-web-hook-or-api-function.md).</span><span class="sxs-lookup"><span data-stu-id="0c30d-138">For more information about webhooks, see [Create a webhook or API Azure Function](../azure-functions/functions-create-a-web-hook-or-api-function.md).</span></span> 

1. <span data-ttu-id="0c30d-139">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="0c30d-139">Select **Save**.</span></span>

    <span data-ttu-id="0c30d-140">Ve výchozím nastavení, až bude povolená tato zásada platí tooall virtuálních počítačů v testovacím prostředí aktuální hello.</span><span class="sxs-lookup"><span data-stu-id="0c30d-140">By default, once enabled, this policy applies tooall VMs in hello current lab.</span></span> <span data-ttu-id="0c30d-141">tooremove toto nastavení z konkrétní virtuální počítač, otevřete okno hello Virtuálního počítače a změňte jeho **Auto-shutdown** nastavení</span><span class="sxs-lookup"><span data-stu-id="0c30d-141">tooremove this setting from a specific VM, open hello VM's blade and change its **Auto-shutdown** setting</span></span> 

## <a name="set-auto-start"></a><span data-ttu-id="0c30d-142">Sada automatického – spuštění</span><span class="sxs-lookup"><span data-stu-id="0c30d-142">Set auto-start</span></span>
<span data-ttu-id="0c30d-143">Zásady automatické spuštění Hello povoluje toospecify při hello virtuálních počítačů v testovacím prostředí aktuální hello by měl být spuštěn.</span><span class="sxs-lookup"><span data-stu-id="0c30d-143">hello auto-start policy allows you toospecify when hello VMs in hello current lab should be started.</span></span>  

1. <span data-ttu-id="0c30d-144">V testovacím hello **konfiguraci a zásady** vyberte **automatického spuštění**.</span><span class="sxs-lookup"><span data-stu-id="0c30d-144">On hello lab's **Configuration and policies** blade, select **Auto-start**.</span></span>
   
    ![Automatické spuštění](./media/devtest-lab-set-lab-policy/auto-start.png)

2. <span data-ttu-id="0c30d-146">Vyberte **na** tooenable tuto zásadu a **vypnout** toodisable ho.</span><span class="sxs-lookup"><span data-stu-id="0c30d-146">Select **On** tooenable this policy, and **Off** toodisable it.</span></span>

3. <span data-ttu-id="0c30d-147">Pokud povolíte tuto zásadu, zadejte hello naplánovaný čas zahájení, časové pásmo a hello dny v týdnu hello, pro které hello doba platí.</span><span class="sxs-lookup"><span data-stu-id="0c30d-147">If you enable this policy, specify hello scheduled start time, time zone, and hello days of hello week for which hello time applies.</span></span> 

4. <span data-ttu-id="0c30d-148">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="0c30d-148">Select **Save**.</span></span>

    <span data-ttu-id="0c30d-149">Jakmile bude povoleno, tato zásada není automaticky použité tooany virtuálních počítačů v testovacím prostředí aktuální hello.</span><span class="sxs-lookup"><span data-stu-id="0c30d-149">Once enabled, this policy is not automatically applied tooany VMs in hello current lab.</span></span> <span data-ttu-id="0c30d-150">tooapply tato nastavení tooa konkrétní virtuální počítač, okno otevřít hello Virtuálního počítače a změňte jeho **automatického spuštění** nastavení</span><span class="sxs-lookup"><span data-stu-id="0c30d-150">tooapply this setting tooa specific VM, open hello VM's blade and change its **Auto-start** setting</span></span> 

## <a name="next-steps"></a><span data-ttu-id="0c30d-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0c30d-151">Next steps</span></span>

- <span data-ttu-id="0c30d-152">[Definovat zásady testovacího prostředí v Azure DevTest Labs](devtest-lab-set-lab-policy.md) – zjistěte, jak toomodify jiných zásad testovacího prostředí</span><span class="sxs-lookup"><span data-stu-id="0c30d-152">[Define lab policies in Azure DevTest Labs](devtest-lab-set-lab-policy.md) - Learn how toomodify other lab policies</span></span> 
