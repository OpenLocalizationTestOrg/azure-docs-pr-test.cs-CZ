---
title: "Spravovat virtuální počítače pomocí Průzkumníku Azure pro IntelliJ | Microsoft Docs"
description: "Zjistěte, jak spravovat virtuální počítače Azure pomocí Průzkumníka Azure pro IntelliJ."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 9197580407b3509fbf9a842e1fee1e6348478c34
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-virtual-machines-by-using-the-azure-explorer-for-intellij"></a><span data-ttu-id="a7628-103">Spravovat virtuální počítače pomocí Průzkumníku Azure pro IntelliJ</span><span class="sxs-lookup"><span data-stu-id="a7628-103">Manage virtual machines by using the Azure Explorer for IntelliJ</span></span>

<span data-ttu-id="a7628-104">Průzkumník Azure, který je součástí sady nástrojů Azure pro IntelliJ, poskytuje Java vývojářům snadno použitelné řešení pro správu virtuálních počítačů v jejich účtu Azure z uvnitř IntelliJ integrované vývojové prostředí (IDE).</span><span class="sxs-lookup"><span data-stu-id="a7628-104">The Azure Explorer, which is part of the Azure Toolkit for IntelliJ, provides Java developers with an easy-to-use solution for managing virtual machines in their Azure account from inside the IntelliJ integrated development environment (IDE).</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-virtual-machine-in-intellij"></a><span data-ttu-id="a7628-105">Vytvoření virtuálního počítače v IntelliJ</span><span class="sxs-lookup"><span data-stu-id="a7628-105">Create a virtual machine in IntelliJ</span></span>

<span data-ttu-id="a7628-106">K vytvoření virtuálního počítače pomocí Průzkumníku Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="a7628-106">To create a virtual machine by using the Azure Explorer, do the following:</span></span> 

1. <span data-ttu-id="a7628-107">Přihlaste se k účtu Azure pomocí kroků v [přihlášení pokyny pro Azure Toolkit IntelliJ] článku.</span><span class="sxs-lookup"><span data-stu-id="a7628-107">Sign in to your Azure account by using the steps in the [Sign-in instructions for the Azure Toolkit for IntelliJ] article.</span></span>

2. <span data-ttu-id="a7628-108">V **Azure Explorer** , rozbalte **Azure** uzel, klikněte pravým tlačítkem na **virtuální počítače**a potom klikněte na **vytvoření virtuálního počítače**.</span><span class="sxs-lookup"><span data-stu-id="a7628-108">In the **Azure Explorer** view, expand the **Azure** node, right-click **Virtual Machines**, and then click **Create VM**.</span></span> 

   <span data-ttu-id="a7628-109">![Příkaz vytvoření virtuálního počítače][CR01]</span><span class="sxs-lookup"><span data-stu-id="a7628-109">![The Create VM command][CR01]</span></span>  
    <span data-ttu-id="a7628-110">**Vytvořit nový virtuální počítač** otevře se průvodce.</span><span class="sxs-lookup"><span data-stu-id="a7628-110">The **Create new Virtual Machine** wizard opens.</span></span>

3. <span data-ttu-id="a7628-111">V **zvolte předplatné** oken, vyberte své předplatné a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="a7628-111">In the **Choose a Subscription** window, select your subscription, and then click **Next**.</span></span> 

   ![Zvolte předplatné okna][CR02]

4. <span data-ttu-id="a7628-113">V **vyberte bitovou kopii virtuálního počítače** okno, zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="a7628-113">In the **Select a Virtual Machine Image** window, enter the following information:</span></span>

   * <span data-ttu-id="a7628-114">**Umístění**: Určuje, kde bude vytvořen virtuální počítač (například *západní USA*).</span><span class="sxs-lookup"><span data-stu-id="a7628-114">**Location**: Specifies where your virtual machine will be created (for example, *West US*).</span></span> 

   * <span data-ttu-id="a7628-115">**Doporučená image**: Určuje, že zvolíte bitovou kopii z zkrácený seznam běžně používané bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="a7628-115">**Recommended image**: Specifies that you will choose an image from an abbreviated list of commonly used images.</span></span>

   * <span data-ttu-id="a7628-116">**Vlastní image**: Určuje, že zvolíte vlastní image tím, že poskytuje následující informace:</span><span class="sxs-lookup"><span data-stu-id="a7628-116">**Custom image**: Specifies that you will choose a custom image by providing the following information:</span></span>

      * <span data-ttu-id="a7628-117">**Vydavatel**: Určuje publisher, který vytvořil bitovou kopii, kterou budete používat pro virtuální počítač (například *Microsoft*).</span><span class="sxs-lookup"><span data-stu-id="a7628-117">**Publisher**: Specifies the publisher that created the image that you will use for your virtual machine (for example, *Microsoft*).</span></span>

      * <span data-ttu-id="a7628-118">**Nabízejí**: Určuje nabídky používat od vydavatele, vybraný virtuální počítač (například *JDK*).</span><span class="sxs-lookup"><span data-stu-id="a7628-118">**Offer**: Specifies the virtual machine offering to use from the selected publisher (for example, *JDK*).</span></span>

      * <span data-ttu-id="a7628-119">**Skladová položka**: Určuje skladová jednotka (SKU) z vybrané nabídky (například *JDK_8*).</span><span class="sxs-lookup"><span data-stu-id="a7628-119">**Sku**: Specifies the stockkeeping unit (SKU) to use from the selected offering (for example, *JDK_8*).</span></span>

      * <span data-ttu-id="a7628-120">**Verze #**: Určuje, kterou verzi vybrané SKU používat.</span><span class="sxs-lookup"><span data-stu-id="a7628-120">**Version #**: Specifies which version of the selected SKU to use.</span></span>

   ![Vyberte okno bitovou kopii virtuálního počítače][CR03]

5. <span data-ttu-id="a7628-122">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="a7628-122">Click **Next**.</span></span> 

6. <span data-ttu-id="a7628-123">V **základní nastavení virtuálního počítače** okno, zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="a7628-123">In the **Virtual Machine Basic Settings** window, enter the following information:</span></span>

   * <span data-ttu-id="a7628-124">**Název virtuálního počítače**: Určuje název pro nový virtuální počítač, který musí začínat písmenem a obsahovat pouze písmena, číslice a pomlčky.</span><span class="sxs-lookup"><span data-stu-id="a7628-124">**Virtual machine name**: Specifies the name for your new virtual machine, which must start with a letter and contain only letters, numbers, and hyphens.</span></span>

   * <span data-ttu-id="a7628-125">**Velikost**: Určuje počet jader a paměti k přidělení pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="a7628-125">**Size**: Specifies the number of cores and memory to allocate for your virtual machine.</span></span>

   * <span data-ttu-id="a7628-126">**Uživatelské jméno**: Určuje účet správce, který chcete vytvořit pro virtuální počítač pod správou.</span><span class="sxs-lookup"><span data-stu-id="a7628-126">**User name**: Specifies the administrator account to create for managing your virtual machine.</span></span>

   * <span data-ttu-id="a7628-127">**Heslo** a **potvrdit**: Určuje heslo pro účet správce.</span><span class="sxs-lookup"><span data-stu-id="a7628-127">**Password** and **Confirm**: Specifies the password for your administrator account.</span></span>

   ![Okno základní nastavení virtuálního počítače][CR04]

7. <span data-ttu-id="a7628-129">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="a7628-129">Click **Next**.</span></span> 

8. <span data-ttu-id="a7628-130">V **související prostředky** okno, zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="a7628-130">In the **Associated Resources** window, enter the following information:</span></span>

   * <span data-ttu-id="a7628-131">**Skupina prostředků**: Určuje skupinu prostředků pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="a7628-131">**Resource group**: Specifies the resource group for your virtual machine.</span></span> <span data-ttu-id="a7628-132">Vyberte jednu z následujících možností:</span><span class="sxs-lookup"><span data-stu-id="a7628-132">Select one of the following options:</span></span>
      * <span data-ttu-id="a7628-133">**Vytvořit nový**: Určuje, že chcete vytvořit novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="a7628-133">**Create new**: Specifies that you want to create a new resource group.</span></span>
      * <span data-ttu-id="a7628-134">**Použít existující**: Určuje, zda chcete vybrat ze seznamu skupin prostředků, které jsou spojeny s vaším účtem Azure.</span><span class="sxs-lookup"><span data-stu-id="a7628-134">**Use existing**: Specifies that you want to select from a list of resource groups that are associated with your Azure account.</span></span>

       ![Okna přidružené prostředky][CR07]

   * <span data-ttu-id="a7628-136">**Účet úložiště**: Určuje účet úložiště, který chcete použít pro virtuální počítač uložen.</span><span class="sxs-lookup"><span data-stu-id="a7628-136">**Storage account**: Specifies the storage account to use for storing your virtual machine.</span></span> <span data-ttu-id="a7628-137">Můžete vybrat existující účet úložiště nebo vytvořit nový účet.</span><span class="sxs-lookup"><span data-stu-id="a7628-137">You can choose an existing storage account or create a new account.</span></span> <span data-ttu-id="a7628-138">Pokud se rozhodnete **vytvořit nový**, zobrazí se dialogové okno následující:</span><span class="sxs-lookup"><span data-stu-id="a7628-138">If you choose **Create New**, the following dialog box appears:</span></span>

      ![Dialogové okno Vytvořit účet úložiště][CR05]

   * <span data-ttu-id="a7628-140">**Virtuální síť** a **podsíť**: Určuje virtuální síť a podsíť, kterému se bude připojovat virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a7628-140">**Virtual Network** and **Subnet**: Specifies the virtual network and subnet that your virtual machine will connect to.</span></span> <span data-ttu-id="a7628-141">Můžete použít existující síť a podsíť, nebo můžete vytvořit nové sítě a podsítě.</span><span class="sxs-lookup"><span data-stu-id="a7628-141">You can use an existing network and subnet, or you can create a new network and subnet.</span></span> <span data-ttu-id="a7628-142">Pokud vyberete **vytvořit nový**, zobrazí se dialogové okno následující:</span><span class="sxs-lookup"><span data-stu-id="a7628-142">If you select **Create new**, the following dialog box appears:</span></span>

      ![Dialogové okno vytvořit virtuální síť][CR06]

   * <span data-ttu-id="a7628-144">**Veřejná IP adresa**: Určuje externí IP adresu pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="a7628-144">**Public IP address**: Specifies an external-facing IP address for your virtual machine.</span></span> <span data-ttu-id="a7628-145">Můžete vytvořit novou IP adresu nebo, pokud virtuální počítač nebude mít veřejnou IP adresu, můžete si vybrat **(None)**.</span><span class="sxs-lookup"><span data-stu-id="a7628-145">You can choose to create a new IP address or, if your virtual machine will not have a public IP address, you can select **(None)**.</span></span> 

   * <span data-ttu-id="a7628-146">**Skupina zabezpečení sítě**: Určuje volitelné síťové brány firewall pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="a7628-146">**Network security group**: Specifies an optional networking firewall for your virtual machine.</span></span> <span data-ttu-id="a7628-147">Můžete vybrat existující bránu firewall, nebo pokud virtuální počítač nebude používat síťovou bránu firewall, můžete vybrat **(None)**.</span><span class="sxs-lookup"><span data-stu-id="a7628-147">You can select an existing firewall or, if your virtual machine will not use a network firewall, you can select **(None)**.</span></span> 

   * <span data-ttu-id="a7628-148">**Skupina dostupnosti**: Určuje, že virtuální počítač může patřit do sadu volitelné dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="a7628-148">**Availability set**: Specifies an optional availability set that your virtual machine can belong to.</span></span> <span data-ttu-id="a7628-149">Můžete vybrat existující sadu dostupnosti, vytvořit novou skupinu dostupnosti nebo, pokud nebude virtuální počítač patří do skupiny dostupnosti, vyberte **(None)**.</span><span class="sxs-lookup"><span data-stu-id="a7628-149">You can select an existing availability set, create a new availability set or, if your virtual machine will not belong to an availability set, select **(None)**.</span></span>

9. <span data-ttu-id="a7628-150">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="a7628-150">Click **Finish**.</span></span>  
    <span data-ttu-id="a7628-151">Nový virtuální počítač se zobrazí v okně nástroje Průzkumník Azure.</span><span class="sxs-lookup"><span data-stu-id="a7628-151">Your new virtual machine appears in the Azure Explorer tool window.</span></span> 

   ![Nový virtuální počítač v zobrazení Průzkumník Azure][CR08]

## <a name="restart-a-virtual-machine-in-intellij"></a><span data-ttu-id="a7628-153">Restartování virtuálního počítače v IntelliJ</span><span class="sxs-lookup"><span data-stu-id="a7628-153">Restart a virtual machine in IntelliJ</span></span>

<span data-ttu-id="a7628-154">K restartování virtuálního počítače pomocí Průzkumníku Azure v IntelliJ, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="a7628-154">To restart a virtual machine by using the Azure Explorer in IntelliJ, do the following:</span></span>

1. <span data-ttu-id="a7628-155">V **Azure Explorer** zobrazení, klikněte pravým tlačítkem na virtuální počítač a pak vyberte **restartujte**.</span><span class="sxs-lookup"><span data-stu-id="a7628-155">In the **Azure Explorer** view, right-click the virtual machine, and then select **Restart**.</span></span>

   ![Příkaz restartovat virtuální počítač][RE01]

2. <span data-ttu-id="a7628-157">V okně potvrzení klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="a7628-157">In the confirmation window, click **Yes**.</span></span> 

   ![Okno pro potvrzení restartování virtuálního počítače][RE02]

## <a name="shut-down-a-virtual-machine-in-intellij"></a><span data-ttu-id="a7628-159">Vypnout virtuální počítač v IntelliJ</span><span class="sxs-lookup"><span data-stu-id="a7628-159">Shut down a virtual machine in IntelliJ</span></span>

<span data-ttu-id="a7628-160">Vypnout spuštěného virtuálního počítače pomocí Průzkumníku Azure v IntelliJ, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="a7628-160">To shut down a running virtual machine by using the Azure Explorer in IntelliJ, do the following:</span></span>

1. <span data-ttu-id="a7628-161">V **Azure Explorer** zobrazení, klikněte pravým tlačítkem na virtuální počítač a pak vyberte **vypnutí**.</span><span class="sxs-lookup"><span data-stu-id="a7628-161">In the **Azure Explorer** view, right-click the virtual machine, and then select **Shutdown**.</span></span>

   ![Příkaz pro vypnutí virtuálního počítače][SH01]

2. <span data-ttu-id="a7628-163">V okně potvrzení klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="a7628-163">In the confirmation window, click **Yes**.</span></span> 

   ![Vypnutí virtuálního počítače potvrzovacím okně][SH02]

## <a name="delete-a-virtual-machine-in-intellij"></a><span data-ttu-id="a7628-165">Odstranění virtuálního počítače v IntelliJ</span><span class="sxs-lookup"><span data-stu-id="a7628-165">Delete a virtual machine in IntelliJ</span></span>

<span data-ttu-id="a7628-166">Odstranění virtuálního počítače pomocí Průzkumníku Azure v IntelliJ, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="a7628-166">To delete a virtual machine by using the Azure Explorer in IntelliJ, do the following:</span></span>

1. <span data-ttu-id="a7628-167">V **Azure Explorer** zobrazení, klikněte pravým tlačítkem na virtuální počítač a pak vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="a7628-167">In the **Azure Explorer** view, right-click the virtual machine, and then select **Delete**.</span></span>

   ![Příkaz Delete virtuálního počítače][DE01]

2. <span data-ttu-id="a7628-169">V okně potvrzení klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="a7628-169">In the confirmation window, click **Yes**.</span></span> 

   ![Okno pro potvrzení odstranění virtuálního počítače][DE02]

## <a name="next-steps"></a><span data-ttu-id="a7628-171">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a7628-171">Next steps</span></span>
<span data-ttu-id="a7628-172">Další informace o Azure velikostí virtuálních počítačů a cenách najdete v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="a7628-172">For more information about Azure virtual-machine sizes and pricing, see the following resources:</span></span>

* <span data-ttu-id="a7628-173">Azure velikostí virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="a7628-173">Azure virtual-machine sizes</span></span>
  * <span data-ttu-id="a7628-174">[Velikosti pro virtuální počítače s Windows v Azure]</span><span class="sxs-lookup"><span data-stu-id="a7628-174">[Sizes for Windows virtual machines in Azure]</span></span>
  * <span data-ttu-id="a7628-175">[Velikosti pro virtuální počítače s Linuxem v Azure]</span><span class="sxs-lookup"><span data-stu-id="a7628-175">[Sizes for Linux virtual machines in Azure]</span></span>
* <span data-ttu-id="a7628-176">Azure virtual Machines – ceny</span><span class="sxs-lookup"><span data-stu-id="a7628-176">Azure virtual-machine pricing</span></span>
  * <span data-ttu-id="a7628-177">[Ceny virtuálního počítače Windows]</span><span class="sxs-lookup"><span data-stu-id="a7628-177">[Windows virtual-machine pricing]</span></span>
  * <span data-ttu-id="a7628-178">[Virtuální počítače Linux – ceny]</span><span class="sxs-lookup"><span data-stu-id="a7628-178">[Linux virtual-machine pricing]</span></span>

<span data-ttu-id="a7628-179">Další informace o sadách Azure pro integrovaného vývojového prostředí Java najdete v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="a7628-179">For more information about the Azure Toolkits for Java IDEs, see the following resources:</span></span>

* <span data-ttu-id="a7628-180">[Azure nástrojů pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="a7628-180">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="a7628-181">[Co je nového v sadě nástrojů Azure pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="a7628-181">[What's new in the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="a7628-182">[Instalace sady Azure Toolkit pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="a7628-182">[Installing the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="a7628-183">[Pokyny přihlášení k Azure nástrojů pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="a7628-183">[Sign-in instructions for the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="a7628-184">[Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse]</span><span class="sxs-lookup"><span data-stu-id="a7628-184">[Create a Hello World web app for Azure in Eclipse]</span></span>
* <span data-ttu-id="a7628-185">[Sada Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="a7628-185">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="a7628-186">[Co je nového v sadě nástrojů Azure pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="a7628-186">[What's new in the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="a7628-187">[Instalace sady Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="a7628-187">[Installing the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="a7628-188">[přihlášení pokyny pro Azure Toolkit IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="a7628-188">[Sign-in instructions for the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="a7628-189">[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="a7628-189">[Create a Hello World web app for Azure in IntelliJ]</span></span>

<span data-ttu-id="a7628-190">Další informace o používání Azure s Java najdete v tématu [Azure střediska pro vývojáře Java] a [Java nástrojů pro Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="a7628-190">For more information about using Azure with Java, see [Azure Java Developer Center] and [Java Tools for Visual Studio Team Services].</span></span>

<!-- URL List -->

<span data-ttu-id="a7628-191">[Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse.md</span><span class="sxs-lookup"><span data-stu-id="a7628-191">[Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse.md</span></span>
<span data-ttu-id="a7628-192">[Sada Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij.md</span><span class="sxs-lookup"><span data-stu-id="a7628-192">[Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij.md</span></span>
<span data-ttu-id="a7628-193">[Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="a7628-193">[Create a Hello World web app for Azure in Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span></span>
<span data-ttu-id="a7628-194">[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="a7628-194">[Create a Hello World web app for Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span></span>
<span data-ttu-id="a7628-195">[Instalace sady Azure Toolkit pro Eclipse]: ./azure-toolkit-for-eclipse-installation.md</span><span class="sxs-lookup"><span data-stu-id="a7628-195">[Installing the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md</span></span>
<span data-ttu-id="a7628-196">[Instalace sady Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-installation.md</span><span class="sxs-lookup"><span data-stu-id="a7628-196">[Installing the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-installation.md</span></span>
<span data-ttu-id="a7628-197">[Pokyny přihlášení k Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="a7628-197">[Sign-in instructions for the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span></span>
<span data-ttu-id="a7628-198">[přihlášení pokyny pro Azure Toolkit IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="a7628-198">[Sign-in instructions for the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span></span>
<span data-ttu-id="a7628-199">[Co je nového v sadě nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="a7628-199">[What's new in the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md</span></span>
<span data-ttu-id="a7628-200">[Co je nového v sadě nástrojů Azure pro IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="a7628-200">[What's new in the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md</span></span>

<span data-ttu-id="a7628-201">[Azure střediska pro vývojáře Java]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="a7628-201">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>
<span data-ttu-id="a7628-202">[Java nástrojů pro Visual Studio Team Services]: https://java.visualstudio.com/</span><span class="sxs-lookup"><span data-stu-id="a7628-202">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span></span>

<span data-ttu-id="a7628-203">[Velikosti pro virtuální počítače s Windows v Azure]: /azure/virtual-machines/virtual-machines-windows-sizes</span><span class="sxs-lookup"><span data-stu-id="a7628-203">[Sizes for Windows virtual machines in Azure]: /azure/virtual-machines/virtual-machines-windows-sizes</span></span>
<span data-ttu-id="a7628-204">[Velikosti pro virtuální počítače s Linuxem v Azure]: /azure/virtual-machines/virtual-machines-linux-sizes</span><span class="sxs-lookup"><span data-stu-id="a7628-204">[Sizes for Linux virtual machines in Azure]: /azure/virtual-machines/virtual-machines-linux-sizes</span></span>
<span data-ttu-id="a7628-205">[Ceny virtuálního počítače Windows]: /pricing/details/virtual-machines/windows/</span><span class="sxs-lookup"><span data-stu-id="a7628-205">[Windows virtual-machine pricing]: /pricing/details/virtual-machines/windows/</span></span>
<span data-ttu-id="a7628-206">[Virtuální počítače Linux – ceny]: /pricing/details/virtual-machines/linux/</span><span class="sxs-lookup"><span data-stu-id="a7628-206">[Linux virtual-machine pricing]: /pricing/details/virtual-machines/linux/</span></span>


<!-- IMG List -->

[RE01]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/RE01.png
[RE02]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/RE02.png

[SH01]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/SH01.png
[SH02]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/SH02.png

[DE01]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/DE01.png
[DE02]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/DE02.png

[CR01]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR01.png
[CR02]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR02.png
[CR03]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR03.png
[CR04]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR04.png
[CR05]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR05.png
[CR06]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR06.png
[CR07]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR07.png
[CR08]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR08.png
