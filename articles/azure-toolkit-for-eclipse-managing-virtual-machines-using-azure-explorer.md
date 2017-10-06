---
title: "hello aaaManage virtuální počítače pomocí Průzkumníku Azure pro Eclipse | Microsoft Docs"
description: "Zjistěte, jak toomanage virtuální počítače Azure pomocí hello Průzkumníka Azure pro prostředí Eclipse."
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
ms.openlocfilehash: aa83ec7546a0a8514842723b51ce8a5af81f98f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-virtual-machines-by-using-hello-azure-explorer-for-eclipse"></a><span data-ttu-id="28bc5-103">Správa virtuálních počítačů pomocí hello Průzkumník Azure pro Eclipse</span><span class="sxs-lookup"><span data-stu-id="28bc5-103">Manage virtual machines by using hello Azure Explorer for Eclipse</span></span>

<span data-ttu-id="28bc5-104">Hello Průzkumník Azure, který je součástí hello nástrojů Azure pro prostředí Eclipse, poskytuje Java vývojářům snadno použitelné řešení pro správu virtuálních počítačů v jejich účtu Azure z uvnitř hello Eclipse integrované vývojové prostředí (IDE).</span><span class="sxs-lookup"><span data-stu-id="28bc5-104">hello Azure Explorer, which is part of hello Azure Toolkit for Eclipse, provides Java developers with an easy-to-use solution for managing virtual machines in their Azure account from inside hello Eclipse integrated development environment (IDE).</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-eclipse-show-azure-explorer](../includes/azure-toolkit-for-eclipse-show-azure-explorer.md)]

## <a name="create-a-virtual-machine-in-eclipse"></a><span data-ttu-id="28bc5-105">Vytvoření virtuálního počítače v prostředí Eclipse</span><span class="sxs-lookup"><span data-stu-id="28bc5-105">Create a virtual machine in Eclipse</span></span>

<span data-ttu-id="28bc5-106">toocreate virtuálního počítače pomocí hello Azure Explorer hello následující:</span><span class="sxs-lookup"><span data-stu-id="28bc5-106">toocreate a virtual machine by using hello Azure Explorer, do hello following:</span></span>

1. <span data-ttu-id="28bc5-107">Přihlaste se pomocí hello tooyour účet Azure [přihlášení pokyny pro hello nástrojů Azure pro Eclipse].</span><span class="sxs-lookup"><span data-stu-id="28bc5-107">Sign in tooyour Azure account by using hello [Sign-in instructions for hello Azure Toolkit for Eclipse].</span></span>

2. <span data-ttu-id="28bc5-108">V hello **Azure Explorer** , rozbalte hello **Azure** uzel, klikněte pravým tlačítkem na **virtuální počítače**a potom klikněte na **vytvoření virtuálního počítače**.</span><span class="sxs-lookup"><span data-stu-id="28bc5-108">In hello **Azure Explorer** view, expand hello **Azure** node, right-click **Virtual Machines**, and then click **Create VM**.</span></span>

   <span data-ttu-id="28bc5-109">![Hello příkaz vytvoření virtuálního počítače][CR01]</span><span class="sxs-lookup"><span data-stu-id="28bc5-109">![hello Create VM command][CR01]</span></span>  
   <span data-ttu-id="28bc5-110">Hello **vytvořit nový virtuální počítač** otevře se průvodce.</span><span class="sxs-lookup"><span data-stu-id="28bc5-110">hello **Create new Virtual Machine** wizard opens.</span></span>

3. <span data-ttu-id="28bc5-111">V hello **zvolte předplatné** oken, vyberte své předplatné a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="28bc5-111">In hello **Choose a Subscription** window, select your subscription, and then click **Next**.</span></span>

   ![Hello zvolte předplatné okna][CR02]

4. <span data-ttu-id="28bc5-113">V hello **vyberte bitovou kopii virtuálního počítače** okno, zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="28bc5-113">In hello **Select a Virtual Machine Image** window, enter hello following information:</span></span>

   * <span data-ttu-id="28bc5-114">**Umístění**: Určuje, kde bude vytvořen virtuální počítač (například *západní USA*).</span><span class="sxs-lookup"><span data-stu-id="28bc5-114">**Location**: Specifies where your virtual machine will be created (for example, *West US*).</span></span>

   * <span data-ttu-id="28bc5-115">**Vydavatel**: Určuje hello vydavatele, který vytvořil bitovou kopii hello použijete toocreate virtuálního počítače (například *Microsoft*).</span><span class="sxs-lookup"><span data-stu-id="28bc5-115">**Publisher**: Specifies hello publisher that created hello image you'll use toocreate your virtual machine (for example, *Microsoft*).</span></span>

   * <span data-ttu-id="28bc5-116">**Nabízejí**: Určuje nabídky toouse od vydavatele vybrané hello hello virtuálního počítače (například *JDK*).</span><span class="sxs-lookup"><span data-stu-id="28bc5-116">**Offer**: Specifies hello virtual machine offering toouse from hello selected publisher (for example, *JDK*).</span></span>

   * <span data-ttu-id="28bc5-117">**Skladová položka**: Určuje hello skladové jednotky (SKU) toouse z vybrané nabídky hello (například *JDK_8*).</span><span class="sxs-lookup"><span data-stu-id="28bc5-117">**Sku**: Specifies hello stockkeeping unit (SKU) toouse from hello selected offering (for example, *JDK_8*).</span></span>

   * <span data-ttu-id="28bc5-118">**Verze #**: Určuje, která verze hello vybrané SKU toouse.</span><span class="sxs-lookup"><span data-stu-id="28bc5-118">**Version #**: Specifies which version of hello selected SKU toouse.</span></span>

    ![Hello vyberte časové období bitovou kopii virtuálního počítače][CR03]

5. <span data-ttu-id="28bc5-120">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="28bc5-120">Click **Next**.</span></span>

6. <span data-ttu-id="28bc5-121">V hello **základní nastavení virtuálního počítače** okno, zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="28bc5-121">In hello **Virtual Machine Basic Settings** window, enter hello following information:</span></span>

   * <span data-ttu-id="28bc5-122">**Název virtuálního počítače**: Určuje hello název pro nový virtuální počítač, který musí začínat písmenem a obsahovat pouze písmena, číslice a pomlčky.</span><span class="sxs-lookup"><span data-stu-id="28bc5-122">**Virtual Machine Name**: Specifies hello name for your new virtual machine, which must start with a letter and contain only letters, numbers, and hyphens.</span></span>

   * <span data-ttu-id="28bc5-123">**Velikost**: Určuje hello počet jader a paměti tooallocate pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="28bc5-123">**Size**: Specifies hello number of cores and memory tooallocate for your virtual machine.</span></span>

   * <span data-ttu-id="28bc5-124">**Uživatelské jméno**: Určuje hello správce účtu toocreate pro virtuální počítač pod správou.</span><span class="sxs-lookup"><span data-stu-id="28bc5-124">**User name**: Specifies hello administrator account toocreate for managing your virtual machine.</span></span>

   * <span data-ttu-id="28bc5-125">**Heslo** a **potvrdit**: Určuje hello heslo pro účet správce.</span><span class="sxs-lookup"><span data-stu-id="28bc5-125">**Password** and **Confirm**: Specifies hello password for your administrator account.</span></span>

    ![okno Hello základní nastavení virtuálního počítače][CR04]

7. <span data-ttu-id="28bc5-127">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="28bc5-127">Click **Next**.</span></span>

8. <span data-ttu-id="28bc5-128">V hello **vytvořit nový účet úložiště** okno, zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="28bc5-128">In hello **Create New Storage Account** window, enter hello following information:</span></span>

   * <span data-ttu-id="28bc5-129">**Skupina prostředků**: Určuje hello skupinu prostředků pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="28bc5-129">**Resource Group**: Specifies hello resource group for your virtual machine.</span></span> <span data-ttu-id="28bc5-130">Vyberte jednu z hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="28bc5-130">Select one of hello following options:</span></span>
      * <span data-ttu-id="28bc5-131">**Vytvořit nový**: Určuje, že chcete toocreate novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="28bc5-131">**Create new**: Specifies that you want toocreate a new resource group.</span></span>
      * <span data-ttu-id="28bc5-132">**Použít existující**: Určuje, že chcete tooselect skupinu prostředků, který je už přidružená ke svému účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="28bc5-132">**Use existing**: Specifies that you want tooselect a resource group that is already associated with your Azure account.</span></span>

      ![Dialogové okno Vytvořit nový účet úložiště Hello][CR05]

   * <span data-ttu-id="28bc5-134">**Účet úložiště**: Určuje toouse účet hello úložiště pro uložení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="28bc5-134">**Storage account**: Specifies hello storage account toouse for storing your virtual machine.</span></span> <span data-ttu-id="28bc5-135">Můžete použít existující účet úložiště nebo vytvořit nový účet.</span><span class="sxs-lookup"><span data-stu-id="28bc5-135">You can use an existing storage account or create a new account.</span></span>

   * <span data-ttu-id="28bc5-136">**Virtuální síť** a **podsíť**: Určuje hello virtuální síť a podsíť, kterému se bude připojovat virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="28bc5-136">**Virtual Network** and **Subnet**: Specifies hello virtual network and subnet that your virtual machine will connect to.</span></span> <span data-ttu-id="28bc5-137">Můžete použít existující síť a podsíť, nebo můžete vytvořit nové sítě a podsítě.</span><span class="sxs-lookup"><span data-stu-id="28bc5-137">You can use an existing network and subnet, or you can create a new network and subnet.</span></span> <span data-ttu-id="28bc5-138">Pokud vyberete **vytvořit nový**, zobrazí se následující dialogové okno hello:</span><span class="sxs-lookup"><span data-stu-id="28bc5-138">If you select **Create new**, hello following dialog box is displayed:</span></span>

      ![Dialogové okno Vytvořit novou virtuální síť Hello][CR06]

9. <span data-ttu-id="28bc5-140">V hello **související prostředky** okno, zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="28bc5-140">In hello **Associated Resources** window, enter hello following information:</span></span>

   * <span data-ttu-id="28bc5-141">**Veřejná IP adresa**: Určuje externí IP adresu pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="28bc5-141">**Public IP address**: Specifies an external-facing IP address for your virtual machine.</span></span> <span data-ttu-id="28bc5-142">Můžete toocreate novou IP adresu nebo, pokud virtuální počítač nebude mít veřejnou IP adresu, můžete si vybrat **(None)**.</span><span class="sxs-lookup"><span data-stu-id="28bc5-142">You can choose toocreate a new IP address or, if your virtual machine will not have a public IP address, you can select **(None)**.</span></span>

   * <span data-ttu-id="28bc5-143">**Skupina zabezpečení sítě**: Určuje volitelné síťové brány firewall pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="28bc5-143">**Network security group**: Specifies an optional networking firewall for your virtual machine.</span></span> <span data-ttu-id="28bc5-144">Můžete vybrat existující bránu firewall, nebo pokud virtuální počítač nebude používat síťovou bránu firewall, můžete vybrat **(None)**.</span><span class="sxs-lookup"><span data-stu-id="28bc5-144">You can select an existing firewall or, if your virtual machine will not use a network firewall, you can select **(None)**.</span></span>

   * <span data-ttu-id="28bc5-145">**Skupina dostupnosti**: Určuje, že virtuální počítač může patřit do sadu volitelné dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="28bc5-145">**Availability set**: Specifies an optional availability set that your virtual machine can belong to.</span></span> <span data-ttu-id="28bc5-146">Můžete vybrat existující sadu dostupnosti nebo vytvořit novou skupinu dostupnosti nebo, pokud virtuální počítač nebude patří tooan skupinu dostupnosti, můžete vybrat **(None)**.</span><span class="sxs-lookup"><span data-stu-id="28bc5-146">You can select an existing availability set or create a new availability set or, if your virtual machine will not belong tooan availability set, you can select **(None)**.</span></span>

   ![okna přidružené prostředky Hello][CR07]

9. <span data-ttu-id="28bc5-148">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="28bc5-148">Click **Finish**.</span></span>  
  <span data-ttu-id="28bc5-149">Nový virtuální počítač se zobrazí v okně nástroje Azure Exploreru hello.</span><span class="sxs-lookup"><span data-stu-id="28bc5-149">Your new virtual machine is displayed in hello Azure Explorer tool window.</span></span>

   ![Nový virtuální počítač][CR08]

## <a name="restart-a-virtual-machine-in-eclipse"></a><span data-ttu-id="28bc5-151">Restartování virtuálního počítače v prostředí Eclipse</span><span class="sxs-lookup"><span data-stu-id="28bc5-151">Restart a virtual machine in Eclipse</span></span>

<span data-ttu-id="28bc5-152">virtuální počítač pomocí hello Průzkumník Azure v prostředí Eclipse toorestart hello následující:</span><span class="sxs-lookup"><span data-stu-id="28bc5-152">toorestart a virtual machine by using hello Azure Explorer in Eclipse, do hello following:</span></span>

1. <span data-ttu-id="28bc5-153">V hello **Azure Explorer** zobrazení, klikněte pravým tlačítkem na hello virtuálního počítače a pak vyberte **restartujte**.</span><span class="sxs-lookup"><span data-stu-id="28bc5-153">In hello **Azure Explorer** view, right-click hello virtual machine, and then select **Restart**.</span></span>

   ![příkaz restartovat virtuální počítač Hello][RE01]

2. <span data-ttu-id="28bc5-155">V okně potvrzení hello klikněte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="28bc5-155">In hello confirmation window, click **Yes**.</span></span>

   ![okno pro potvrzení restartování Hello][RE02]

## <a name="shut-down-a-virtual-machine-in-eclipse"></a><span data-ttu-id="28bc5-157">Vypnout virtuální počítač v prostředí Eclipse</span><span class="sxs-lookup"><span data-stu-id="28bc5-157">Shut down a virtual machine in Eclipse</span></span>

<span data-ttu-id="28bc5-158">tooshut dolů spuštěného virtuálního počítače pomocí hello Průzkumník Azure v prostředí Eclipse hello následující:</span><span class="sxs-lookup"><span data-stu-id="28bc5-158">tooshut down a running virtual machine by using hello Azure Explorer in Eclipse, do hello following:</span></span>

1. <span data-ttu-id="28bc5-159">V hello **Azure Explorer** zobrazení, klikněte pravým tlačítkem na hello virtuálního počítače a pak vyberte **vypnutí**.</span><span class="sxs-lookup"><span data-stu-id="28bc5-159">In hello **Azure Explorer** view, right-click hello virtual machine, and then select **Shutdown**.</span></span>

   ![příkaz pro vypnutí virtuálního počítače Hello][SH01]

2. <span data-ttu-id="28bc5-161">V okně potvrzení hello klikněte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="28bc5-161">In hello confirmation window, click **Yes**.</span></span>

   ![okno pro potvrzení Hello vypnutí virtuálního počítače][SH02]

## <a name="delete-a-virtual-machine-in-eclipse"></a><span data-ttu-id="28bc5-163">Odstranění virtuálního počítače v prostředí Eclipse</span><span class="sxs-lookup"><span data-stu-id="28bc5-163">Delete a virtual machine in Eclipse</span></span>

<span data-ttu-id="28bc5-164">virtuální počítač pomocí hello Průzkumník Azure v prostředí Eclipse toodelete hello následující:</span><span class="sxs-lookup"><span data-stu-id="28bc5-164">toodelete a virtual machine by using hello Azure Explorer in Eclipse, do hello following:</span></span>

1. <span data-ttu-id="28bc5-165">V hello **Azure Explorer** zobrazení, klikněte pravým tlačítkem na hello virtuálního počítače a pak vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="28bc5-165">In hello **Azure Explorer** view, right-click hello virtual machine, and then select **Delete**.</span></span>

   ![příkaz Delete Hello virtuálního počítače][DE01]

2. <span data-ttu-id="28bc5-167">V okně potvrzení hello klikněte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="28bc5-167">In hello confirmation window, click **Yes**.</span></span>

   ![okno pro potvrzení odstranění virtuálního počítače Hello][DE02]

## <a name="next-steps"></a><span data-ttu-id="28bc5-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="28bc5-169">Next steps</span></span>
<span data-ttu-id="28bc5-170">Další informace o Azure velikosti virtuálního počítače a cenách najdete v tématu hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="28bc5-170">For more information about Azure virtual-machine sizes and pricing, see hello following resources:</span></span>

* <span data-ttu-id="28bc5-171">Azure velikostí virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="28bc5-171">Azure virtual-machine sizes</span></span>
  * <span data-ttu-id="28bc5-172">[Velikosti pro virtuální počítače s Windows v Azure]</span><span class="sxs-lookup"><span data-stu-id="28bc5-172">[Sizes for Windows virtual machines in Azure]</span></span>
  * <span data-ttu-id="28bc5-173">[Velikosti pro virtuální počítače s Linuxem v Azure]</span><span class="sxs-lookup"><span data-stu-id="28bc5-173">[Sizes for Linux virtual machines in Azure]</span></span>
* <span data-ttu-id="28bc5-174">Azure virtual Machines – ceny</span><span class="sxs-lookup"><span data-stu-id="28bc5-174">Azure virtual-machine pricing</span></span>
  * <span data-ttu-id="28bc5-175">[Ceny virtuálního počítače Windows]</span><span class="sxs-lookup"><span data-stu-id="28bc5-175">[Windows virtual-machine pricing]</span></span>
  * <span data-ttu-id="28bc5-176">[Virtuální počítače Linux – ceny]</span><span class="sxs-lookup"><span data-stu-id="28bc5-176">[Linux virtual-machine pricing]</span></span>

<span data-ttu-id="28bc5-177">Další informace o hello sadách Azure pro integrovaného vývojového prostředí Java najdete v tématu hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="28bc5-177">For more information about hello Azure Toolkits for Java IDEs, see hello following resources:</span></span>

* <span data-ttu-id="28bc5-178">[Azure nástrojů pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="28bc5-178">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="28bc5-179">[Co je nového v hello nástrojů Azure pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="28bc5-179">[What's new in hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="28bc5-180">[Instalace hello nástrojů Azure pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="28bc5-180">[Installing hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="28bc5-181">[přihlášení pokyny pro hello nástrojů Azure pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="28bc5-181">[Sign-in instructions for hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="28bc5-182">[Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse]</span><span class="sxs-lookup"><span data-stu-id="28bc5-182">[Create a Hello World web app for Azure in Eclipse]</span></span>
* <span data-ttu-id="28bc5-183">[Sada Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="28bc5-183">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="28bc5-184">[Co je nového v hello nástrojů Azure pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="28bc5-184">[What's new in hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="28bc5-185">[Instalace hello Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="28bc5-185">[Installing hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="28bc5-186">[Přihlášení pokyny pro hello Azure Toolkit IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="28bc5-186">[Sign-in instructions for hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="28bc5-187">[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="28bc5-187">[Create a Hello World web app for Azure in IntelliJ]</span></span>

<span data-ttu-id="28bc5-188">Další informace o používání Azure s Java najdete v tématu [Azure střediska pro vývojáře Java] a [Java nástrojů pro Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="28bc5-188">For more information about using Azure with Java, see [Azure Java Developer Center] and [Java Tools for Visual Studio Team Services].</span></span>

<!-- URL List -->

[Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse.md
[Sada Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij.md
[Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Instalace hello nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Instalace hello Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[přihlášení pokyny pro hello nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Přihlášení pokyny pro hello Azure Toolkit IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Co je nového v hello nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Co je nového v hello nástrojů Azure pro IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure střediska pro vývojáře Java]: https://azure.microsoft.com/develop/java/
[Java nástrojů pro Visual Studio Team Services]: https://java.visualstudio.com/

[Velikosti pro virtuální počítače s Windows v Azure]: /azure/virtual-machines/virtual-machines-windows-sizes
[Velikosti pro virtuální počítače s Linuxem v Azure]: /azure/virtual-machines/virtual-machines-linux-sizes
[Ceny virtuálního počítače Windows]: /pricing/details/virtual-machines/windows/
[Virtuální počítače Linux – ceny]: /pricing/details/virtual-machines/linux/

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
