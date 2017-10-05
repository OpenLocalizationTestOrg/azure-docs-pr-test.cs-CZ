---
title: "Spravovat virtuální počítače pomocí Průzkumníku Azure pro Eclipse | Microsoft Docs"
description: "Zjistěte, jak spravovat virtuální počítače Azure pomocí Průzkumníka Azure pro prostředí Eclipse."
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
ms.openlocfilehash: 9784e8af9c530078afee06f08a23403a44b0762f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-virtual-machines-by-using-the-azure-explorer-for-eclipse"></a><span data-ttu-id="2c41a-103">Spravovat virtuální počítače pomocí Průzkumníku Azure pro Eclipse</span><span class="sxs-lookup"><span data-stu-id="2c41a-103">Manage virtual machines by using the Azure Explorer for Eclipse</span></span>

<span data-ttu-id="2c41a-104">Průzkumník Azure, který je součástí sady nástrojů Azure pro prostředí Eclipse, poskytuje Java vývojářům snadno použitelné řešení pro správu virtuálních počítačů v jejich účtu Azure z uvnitř Eclipse integrované vývojové prostředí (IDE).</span><span class="sxs-lookup"><span data-stu-id="2c41a-104">The Azure Explorer, which is part of the Azure Toolkit for Eclipse, provides Java developers with an easy-to-use solution for managing virtual machines in their Azure account from inside the Eclipse integrated development environment (IDE).</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-eclipse-show-azure-explorer](../includes/azure-toolkit-for-eclipse-show-azure-explorer.md)]

## <a name="create-a-virtual-machine-in-eclipse"></a><span data-ttu-id="2c41a-105">Vytvoření virtuálního počítače v prostředí Eclipse</span><span class="sxs-lookup"><span data-stu-id="2c41a-105">Create a virtual machine in Eclipse</span></span>

<span data-ttu-id="2c41a-106">K vytvoření virtuálního počítače pomocí Průzkumníku Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="2c41a-106">To create a virtual machine by using the Azure Explorer, do the following:</span></span>

1. <span data-ttu-id="2c41a-107">Přihlaste se k účtu Azure pomocí [pokyny přihlášení k Azure nástrojů pro Eclipse].</span><span class="sxs-lookup"><span data-stu-id="2c41a-107">Sign in to your Azure account by using the [Sign-in instructions for the Azure Toolkit for Eclipse].</span></span>

2. <span data-ttu-id="2c41a-108">V **Azure Explorer** , rozbalte **Azure** uzel, klikněte pravým tlačítkem na **virtuální počítače**a potom klikněte na **vytvoření virtuálního počítače**.</span><span class="sxs-lookup"><span data-stu-id="2c41a-108">In the **Azure Explorer** view, expand the **Azure** node, right-click **Virtual Machines**, and then click **Create VM**.</span></span>

   <span data-ttu-id="2c41a-109">![Příkaz vytvoření virtuálního počítače][CR01]</span><span class="sxs-lookup"><span data-stu-id="2c41a-109">![The Create VM command][CR01]</span></span>  
   <span data-ttu-id="2c41a-110">**Vytvořit nový virtuální počítač** otevře se průvodce.</span><span class="sxs-lookup"><span data-stu-id="2c41a-110">The **Create new Virtual Machine** wizard opens.</span></span>

3. <span data-ttu-id="2c41a-111">V **zvolte předplatné** oken, vyberte své předplatné a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="2c41a-111">In the **Choose a Subscription** window, select your subscription, and then click **Next**.</span></span>

   ![Zvolte předplatné okna][CR02]

4. <span data-ttu-id="2c41a-113">V **vyberte bitovou kopii virtuálního počítače** okno, zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="2c41a-113">In the **Select a Virtual Machine Image** window, enter the following information:</span></span>

   * <span data-ttu-id="2c41a-114">**Umístění**: Určuje, kde bude vytvořen virtuální počítač (například *západní USA*).</span><span class="sxs-lookup"><span data-stu-id="2c41a-114">**Location**: Specifies where your virtual machine will be created (for example, *West US*).</span></span>

   * <span data-ttu-id="2c41a-115">**Vydavatel**: Určuje publisher, který vytvořil bitovou kopii, které použijete k vytvoření virtuálního počítače (například *Microsoft*).</span><span class="sxs-lookup"><span data-stu-id="2c41a-115">**Publisher**: Specifies the publisher that created the image you'll use to create your virtual machine (for example, *Microsoft*).</span></span>

   * <span data-ttu-id="2c41a-116">**Nabízejí**: Určuje nabídky používat od vydavatele, vybraný virtuální počítač (například *JDK*).</span><span class="sxs-lookup"><span data-stu-id="2c41a-116">**Offer**: Specifies the virtual machine offering to use from the selected publisher (for example, *JDK*).</span></span>

   * <span data-ttu-id="2c41a-117">**Skladová položka**: Určuje skladová jednotka (SKU) z vybrané nabídky (například *JDK_8*).</span><span class="sxs-lookup"><span data-stu-id="2c41a-117">**Sku**: Specifies the stockkeeping unit (SKU) to use from the selected offering (for example, *JDK_8*).</span></span>

   * <span data-ttu-id="2c41a-118">**Verze #**: Určuje, kterou verzi vybrané SKU používat.</span><span class="sxs-lookup"><span data-stu-id="2c41a-118">**Version #**: Specifies which version of the selected SKU to use.</span></span>

    ![Vyberte okno bitovou kopii virtuálního počítače][CR03]

5. <span data-ttu-id="2c41a-120">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="2c41a-120">Click **Next**.</span></span>

6. <span data-ttu-id="2c41a-121">V **základní nastavení virtuálního počítače** okno, zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="2c41a-121">In the **Virtual Machine Basic Settings** window, enter the following information:</span></span>

   * <span data-ttu-id="2c41a-122">**Název virtuálního počítače**: Určuje název pro nový virtuální počítač, který musí začínat písmenem a obsahovat pouze písmena, číslice a pomlčky.</span><span class="sxs-lookup"><span data-stu-id="2c41a-122">**Virtual Machine Name**: Specifies the name for your new virtual machine, which must start with a letter and contain only letters, numbers, and hyphens.</span></span>

   * <span data-ttu-id="2c41a-123">**Velikost**: Určuje počet jader a paměti k přidělení pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="2c41a-123">**Size**: Specifies the number of cores and memory to allocate for your virtual machine.</span></span>

   * <span data-ttu-id="2c41a-124">**Uživatelské jméno**: Určuje účet správce, který chcete vytvořit pro virtuální počítač pod správou.</span><span class="sxs-lookup"><span data-stu-id="2c41a-124">**User name**: Specifies the administrator account to create for managing your virtual machine.</span></span>

   * <span data-ttu-id="2c41a-125">**Heslo** a **potvrdit**: Určuje heslo pro účet správce.</span><span class="sxs-lookup"><span data-stu-id="2c41a-125">**Password** and **Confirm**: Specifies the password for your administrator account.</span></span>

    ![Okno základní nastavení virtuálního počítače][CR04]

7. <span data-ttu-id="2c41a-127">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="2c41a-127">Click **Next**.</span></span>

8. <span data-ttu-id="2c41a-128">V **vytvořit nový účet úložiště** okno, zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="2c41a-128">In the **Create New Storage Account** window, enter the following information:</span></span>

   * <span data-ttu-id="2c41a-129">**Skupina prostředků**: Určuje skupinu prostředků pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="2c41a-129">**Resource Group**: Specifies the resource group for your virtual machine.</span></span> <span data-ttu-id="2c41a-130">Vyberte jednu z následujících možností:</span><span class="sxs-lookup"><span data-stu-id="2c41a-130">Select one of the following options:</span></span>
      * <span data-ttu-id="2c41a-131">**Vytvořit nový**: Určuje, že chcete vytvořit novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="2c41a-131">**Create new**: Specifies that you want to create a new resource group.</span></span>
      * <span data-ttu-id="2c41a-132">**Použít existující**: Určuje, zda chcete vybrat skupinu prostředků, který je už přidružená ke svému účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="2c41a-132">**Use existing**: Specifies that you want to select a resource group that is already associated with your Azure account.</span></span>

      ![Dialogové okno Vytvořit nový účet úložiště][CR05]

   * <span data-ttu-id="2c41a-134">**Účet úložiště**: Určuje účet úložiště, který chcete použít pro virtuální počítač uložen.</span><span class="sxs-lookup"><span data-stu-id="2c41a-134">**Storage account**: Specifies the storage account to use for storing your virtual machine.</span></span> <span data-ttu-id="2c41a-135">Můžete použít existující účet úložiště nebo vytvořit nový účet.</span><span class="sxs-lookup"><span data-stu-id="2c41a-135">You can use an existing storage account or create a new account.</span></span>

   * <span data-ttu-id="2c41a-136">**Virtuální síť** a **podsíť**: Určuje virtuální síť a podsíť, kterému se bude připojovat virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="2c41a-136">**Virtual Network** and **Subnet**: Specifies the virtual network and subnet that your virtual machine will connect to.</span></span> <span data-ttu-id="2c41a-137">Můžete použít existující síť a podsíť, nebo můžete vytvořit nové sítě a podsítě.</span><span class="sxs-lookup"><span data-stu-id="2c41a-137">You can use an existing network and subnet, or you can create a new network and subnet.</span></span> <span data-ttu-id="2c41a-138">Pokud vyberete **vytvořit nový**, zobrazí se následující dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="2c41a-138">If you select **Create new**, the following dialog box is displayed:</span></span>

      ![Dialogové okno Vytvořit novou virtuální síť][CR06]

9. <span data-ttu-id="2c41a-140">V **související prostředky** okno, zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="2c41a-140">In the **Associated Resources** window, enter the following information:</span></span>

   * <span data-ttu-id="2c41a-141">**Veřejná IP adresa**: Určuje externí IP adresu pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="2c41a-141">**Public IP address**: Specifies an external-facing IP address for your virtual machine.</span></span> <span data-ttu-id="2c41a-142">Můžete vytvořit novou IP adresu nebo, pokud virtuální počítač nebude mít veřejnou IP adresu, můžete si vybrat **(None)**.</span><span class="sxs-lookup"><span data-stu-id="2c41a-142">You can choose to create a new IP address or, if your virtual machine will not have a public IP address, you can select **(None)**.</span></span>

   * <span data-ttu-id="2c41a-143">**Skupina zabezpečení sítě**: Určuje volitelné síťové brány firewall pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="2c41a-143">**Network security group**: Specifies an optional networking firewall for your virtual machine.</span></span> <span data-ttu-id="2c41a-144">Můžete vybrat existující bránu firewall, nebo pokud virtuální počítač nebude používat síťovou bránu firewall, můžete vybrat **(None)**.</span><span class="sxs-lookup"><span data-stu-id="2c41a-144">You can select an existing firewall or, if your virtual machine will not use a network firewall, you can select **(None)**.</span></span>

   * <span data-ttu-id="2c41a-145">**Skupina dostupnosti**: Určuje, že virtuální počítač může patřit do sadu volitelné dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="2c41a-145">**Availability set**: Specifies an optional availability set that your virtual machine can belong to.</span></span> <span data-ttu-id="2c41a-146">Můžete vybrat existující sadu dostupnosti nebo vytvořit novou skupinu dostupnosti nebo, pokud nebude virtuální počítač patří do skupiny dostupnosti, můžete si vybrat **(None)**.</span><span class="sxs-lookup"><span data-stu-id="2c41a-146">You can select an existing availability set or create a new availability set or, if your virtual machine will not belong to an availability set, you can select **(None)**.</span></span>

   ![Okna přidružené prostředky][CR07]

9. <span data-ttu-id="2c41a-148">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="2c41a-148">Click **Finish**.</span></span>  
  <span data-ttu-id="2c41a-149">Nový virtuální počítač se zobrazí v okně nástroje Průzkumník Azure.</span><span class="sxs-lookup"><span data-stu-id="2c41a-149">Your new virtual machine is displayed in the Azure Explorer tool window.</span></span>

   ![Nový virtuální počítač][CR08]

## <a name="restart-a-virtual-machine-in-eclipse"></a><span data-ttu-id="2c41a-151">Restartování virtuálního počítače v prostředí Eclipse</span><span class="sxs-lookup"><span data-stu-id="2c41a-151">Restart a virtual machine in Eclipse</span></span>

<span data-ttu-id="2c41a-152">K restartování virtuálního počítače pomocí Průzkumníku Azure v prostředí Eclipse, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="2c41a-152">To restart a virtual machine by using the Azure Explorer in Eclipse, do the following:</span></span>

1. <span data-ttu-id="2c41a-153">V **Azure Explorer** zobrazení, klikněte pravým tlačítkem na virtuální počítač a pak vyberte **restartujte**.</span><span class="sxs-lookup"><span data-stu-id="2c41a-153">In the **Azure Explorer** view, right-click the virtual machine, and then select **Restart**.</span></span>

   ![Příkaz restartovat virtuální počítač][RE01]

2. <span data-ttu-id="2c41a-155">V okně potvrzení klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="2c41a-155">In the confirmation window, click **Yes**.</span></span>

   ![Okno pro potvrzení restartování][RE02]

## <a name="shut-down-a-virtual-machine-in-eclipse"></a><span data-ttu-id="2c41a-157">Vypnout virtuální počítač v prostředí Eclipse</span><span class="sxs-lookup"><span data-stu-id="2c41a-157">Shut down a virtual machine in Eclipse</span></span>

<span data-ttu-id="2c41a-158">Vypnout spuštěného virtuálního počítače pomocí Průzkumníku Azure v prostředí Eclipse, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="2c41a-158">To shut down a running virtual machine by using the Azure Explorer in Eclipse, do the following:</span></span>

1. <span data-ttu-id="2c41a-159">V **Azure Explorer** zobrazení, klikněte pravým tlačítkem na virtuální počítač a pak vyberte **vypnutí**.</span><span class="sxs-lookup"><span data-stu-id="2c41a-159">In the **Azure Explorer** view, right-click the virtual machine, and then select **Shutdown**.</span></span>

   ![Příkaz pro vypnutí virtuálního počítače][SH01]

2. <span data-ttu-id="2c41a-161">V okně potvrzení klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="2c41a-161">In the confirmation window, click **Yes**.</span></span>

   ![Okno pro potvrzení vypnutí virtuálního počítače][SH02]

## <a name="delete-a-virtual-machine-in-eclipse"></a><span data-ttu-id="2c41a-163">Odstranění virtuálního počítače v prostředí Eclipse</span><span class="sxs-lookup"><span data-stu-id="2c41a-163">Delete a virtual machine in Eclipse</span></span>

<span data-ttu-id="2c41a-164">Odstranění virtuálního počítače pomocí Průzkumníku Azure v prostředí Eclipse, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="2c41a-164">To delete a virtual machine by using the Azure Explorer in Eclipse, do the following:</span></span>

1. <span data-ttu-id="2c41a-165">V **Azure Explorer** zobrazení, klikněte pravým tlačítkem na virtuální počítač a pak vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="2c41a-165">In the **Azure Explorer** view, right-click the virtual machine, and then select **Delete**.</span></span>

   ![Příkaz Delete virtuálního počítače][DE01]

2. <span data-ttu-id="2c41a-167">V okně potvrzení klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="2c41a-167">In the confirmation window, click **Yes**.</span></span>

   ![Okno pro potvrzení odstranění virtuálního počítače][DE02]

## <a name="next-steps"></a><span data-ttu-id="2c41a-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2c41a-169">Next steps</span></span>
<span data-ttu-id="2c41a-170">Další informace o Azure velikostí virtuálních počítačů a cenách najdete v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="2c41a-170">For more information about Azure virtual-machine sizes and pricing, see the following resources:</span></span>

* <span data-ttu-id="2c41a-171">Azure velikostí virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="2c41a-171">Azure virtual-machine sizes</span></span>
  * <span data-ttu-id="2c41a-172">[Velikosti pro virtuální počítače s Windows v Azure]</span><span class="sxs-lookup"><span data-stu-id="2c41a-172">[Sizes for Windows virtual machines in Azure]</span></span>
  * <span data-ttu-id="2c41a-173">[Velikosti pro virtuální počítače s Linuxem v Azure]</span><span class="sxs-lookup"><span data-stu-id="2c41a-173">[Sizes for Linux virtual machines in Azure]</span></span>
* <span data-ttu-id="2c41a-174">Azure virtual Machines – ceny</span><span class="sxs-lookup"><span data-stu-id="2c41a-174">Azure virtual-machine pricing</span></span>
  * <span data-ttu-id="2c41a-175">[Ceny virtuálního počítače Windows]</span><span class="sxs-lookup"><span data-stu-id="2c41a-175">[Windows virtual-machine pricing]</span></span>
  * <span data-ttu-id="2c41a-176">[Virtuální počítače Linux – ceny]</span><span class="sxs-lookup"><span data-stu-id="2c41a-176">[Linux virtual-machine pricing]</span></span>

<span data-ttu-id="2c41a-177">Další informace o sadách Azure pro integrovaného vývojového prostředí Java najdete v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="2c41a-177">For more information about the Azure Toolkits for Java IDEs, see the following resources:</span></span>

* <span data-ttu-id="2c41a-178">[Azure nástrojů pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="2c41a-178">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="2c41a-179">[Co je nového v sadě nástrojů Azure pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="2c41a-179">[What's new in the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="2c41a-180">[Instalace sady Azure Toolkit pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="2c41a-180">[Installing the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="2c41a-181">[pokyny přihlášení k Azure nástrojů pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="2c41a-181">[Sign-in instructions for the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="2c41a-182">[Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse]</span><span class="sxs-lookup"><span data-stu-id="2c41a-182">[Create a Hello World web app for Azure in Eclipse]</span></span>
* <span data-ttu-id="2c41a-183">[Sada Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="2c41a-183">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="2c41a-184">[Co je nového v sadě nástrojů Azure pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="2c41a-184">[What's new in the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="2c41a-185">[Instalace sady Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="2c41a-185">[Installing the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="2c41a-186">[Přihlášení pokyny pro Azure Toolkit IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="2c41a-186">[Sign-in instructions for the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="2c41a-187">[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="2c41a-187">[Create a Hello World web app for Azure in IntelliJ]</span></span>

<span data-ttu-id="2c41a-188">Další informace o používání Azure s Java najdete v tématu [Azure střediska pro vývojáře Java] a [Java nástrojů pro Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="2c41a-188">For more information about using Azure with Java, see [Azure Java Developer Center] and [Java Tools for Visual Studio Team Services].</span></span>

<!-- URL List -->

<span data-ttu-id="2c41a-189">[Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse.md</span><span class="sxs-lookup"><span data-stu-id="2c41a-189">[Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse.md</span></span>
<span data-ttu-id="2c41a-190">[Sada Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij.md</span><span class="sxs-lookup"><span data-stu-id="2c41a-190">[Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij.md</span></span>
<span data-ttu-id="2c41a-191">[Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="2c41a-191">[Create a Hello World web app for Azure in Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span></span>
<span data-ttu-id="2c41a-192">[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="2c41a-192">[Create a Hello World web app for Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span></span>
<span data-ttu-id="2c41a-193">[Instalace sady Azure Toolkit pro Eclipse]: ./azure-toolkit-for-eclipse-installation.md</span><span class="sxs-lookup"><span data-stu-id="2c41a-193">[Installing the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md</span></span>
<span data-ttu-id="2c41a-194">[Instalace sady Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-installation.md</span><span class="sxs-lookup"><span data-stu-id="2c41a-194">[Installing the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-installation.md</span></span>
<span data-ttu-id="2c41a-195">[pokyny přihlášení k Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="2c41a-195">[Sign-in instructions for the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span></span>
<span data-ttu-id="2c41a-196">[Přihlášení pokyny pro Azure Toolkit IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="2c41a-196">[Sign-in instructions for the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span></span>
<span data-ttu-id="2c41a-197">[Co je nového v sadě nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="2c41a-197">[What's new in the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md</span></span>
<span data-ttu-id="2c41a-198">[Co je nového v sadě nástrojů Azure pro IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="2c41a-198">[What's new in the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md</span></span>

<span data-ttu-id="2c41a-199">[Azure střediska pro vývojáře Java]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="2c41a-199">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>
<span data-ttu-id="2c41a-200">[Java nástrojů pro Visual Studio Team Services]: https://java.visualstudio.com/</span><span class="sxs-lookup"><span data-stu-id="2c41a-200">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span></span>

<span data-ttu-id="2c41a-201">[Velikosti pro virtuální počítače s Windows v Azure]: /azure/virtual-machines/virtual-machines-windows-sizes</span><span class="sxs-lookup"><span data-stu-id="2c41a-201">[Sizes for Windows virtual machines in Azure]: /azure/virtual-machines/virtual-machines-windows-sizes</span></span>
<span data-ttu-id="2c41a-202">[Velikosti pro virtuální počítače s Linuxem v Azure]: /azure/virtual-machines/virtual-machines-linux-sizes</span><span class="sxs-lookup"><span data-stu-id="2c41a-202">[Sizes for Linux virtual machines in Azure]: /azure/virtual-machines/virtual-machines-linux-sizes</span></span>
<span data-ttu-id="2c41a-203">[Ceny virtuálního počítače Windows]: /pricing/details/virtual-machines/windows/</span><span class="sxs-lookup"><span data-stu-id="2c41a-203">[Windows virtual-machine pricing]: /pricing/details/virtual-machines/windows/</span></span>
<span data-ttu-id="2c41a-204">[Virtuální počítače Linux – ceny]: /pricing/details/virtual-machines/linux/</span><span class="sxs-lookup"><span data-stu-id="2c41a-204">[Linux virtual-machine pricing]: /pricing/details/virtual-machines/linux/</span></span>

<!-- IMG List -->

[RE01]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/RE01.png
[RE02]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/RE02.png

[SH01]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/SH01.png
[SH02]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/SH02.png

[DE01]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/DE01.png
[DE02]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/DE02.png

[CR01]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR01.png
[CR02]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR02.png
[CR03]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR03.png
[CR04]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR04.png
[CR05]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR05.png
[CR06]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR06.png
[CR07]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR07.png
[CR08]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR08.png
