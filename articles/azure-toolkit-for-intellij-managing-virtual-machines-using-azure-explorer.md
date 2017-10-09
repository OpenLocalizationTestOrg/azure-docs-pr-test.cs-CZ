---
title: "hello aaaManage virtuální počítače pomocí Průzkumníku Azure pro IntelliJ | Microsoft Docs"
description: "Zjistěte, jak toomanage virtuální počítače Azure pomocí hello Průzkumníka Azure pro IntelliJ."
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
ms.openlocfilehash: a73dd4f73b311dd3413f6712e3b76c36ee464de1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-virtual-machines-by-using-hello-azure-explorer-for-intellij"></a><span data-ttu-id="171ef-103">Správa virtuálních počítačů pomocí hello Průzkumník Azure pro IntelliJ</span><span class="sxs-lookup"><span data-stu-id="171ef-103">Manage virtual machines by using hello Azure Explorer for IntelliJ</span></span>

<span data-ttu-id="171ef-104">Hello Průzkumník Azure, který je součástí hello nástrojů Azure pro IntelliJ, poskytuje vývojáře v jazyce Java s řešením snadno použitelné pro správu virtuálních počítačů v jejich účtu Azure z uvnitř hello IntelliJ integrované vývojové prostředí (IDE).</span><span class="sxs-lookup"><span data-stu-id="171ef-104">hello Azure Explorer, which is part of hello Azure Toolkit for IntelliJ, provides Java developers with an easy-to-use solution for managing virtual machines in their Azure account from inside hello IntelliJ integrated development environment (IDE).</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-virtual-machine-in-intellij"></a><span data-ttu-id="171ef-105">Vytvoření virtuálního počítače v IntelliJ</span><span class="sxs-lookup"><span data-stu-id="171ef-105">Create a virtual machine in IntelliJ</span></span>

<span data-ttu-id="171ef-106">toocreate virtuálního počítače pomocí hello Azure Explorer hello následující:</span><span class="sxs-lookup"><span data-stu-id="171ef-106">toocreate a virtual machine by using hello Azure Explorer, do hello following:</span></span> 

1. <span data-ttu-id="171ef-107">Přihlaste se pomocí kroků hello v hello tooyour účet Azure [přihlášení pokyny pro hello Azure Toolkit IntelliJ] článku.</span><span class="sxs-lookup"><span data-stu-id="171ef-107">Sign in tooyour Azure account by using hello steps in hello [Sign-in instructions for hello Azure Toolkit for IntelliJ] article.</span></span>

2. <span data-ttu-id="171ef-108">V hello **Azure Explorer** , rozbalte hello **Azure** uzel, klikněte pravým tlačítkem na **virtuální počítače**a potom klikněte na **vytvoření virtuálního počítače**.</span><span class="sxs-lookup"><span data-stu-id="171ef-108">In hello **Azure Explorer** view, expand hello **Azure** node, right-click **Virtual Machines**, and then click **Create VM**.</span></span> 

   <span data-ttu-id="171ef-109">![Hello příkaz vytvoření virtuálního počítače][CR01]</span><span class="sxs-lookup"><span data-stu-id="171ef-109">![hello Create VM command][CR01]</span></span>  
    <span data-ttu-id="171ef-110">Hello **vytvořit nový virtuální počítač** otevře se průvodce.</span><span class="sxs-lookup"><span data-stu-id="171ef-110">hello **Create new Virtual Machine** wizard opens.</span></span>

3. <span data-ttu-id="171ef-111">V hello **zvolte předplatné** oken, vyberte své předplatné a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="171ef-111">In hello **Choose a Subscription** window, select your subscription, and then click **Next**.</span></span> 

   ![Hello zvolte předplatné okna][CR02]

4. <span data-ttu-id="171ef-113">V hello **vyberte bitovou kopii virtuálního počítače** okno, zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="171ef-113">In hello **Select a Virtual Machine Image** window, enter hello following information:</span></span>

   * <span data-ttu-id="171ef-114">**Umístění**: Určuje, kde bude vytvořen virtuální počítač (například *západní USA*).</span><span class="sxs-lookup"><span data-stu-id="171ef-114">**Location**: Specifies where your virtual machine will be created (for example, *West US*).</span></span> 

   * <span data-ttu-id="171ef-115">**Doporučená image**: Určuje, že zvolíte bitovou kopii z zkrácený seznam běžně používané bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="171ef-115">**Recommended image**: Specifies that you will choose an image from an abbreviated list of commonly used images.</span></span>

   * <span data-ttu-id="171ef-116">**Vlastní image**: Určuje, že zvolíte vlastní image tím, že poskytuje hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="171ef-116">**Custom image**: Specifies that you will choose a custom image by providing hello following information:</span></span>

      * <span data-ttu-id="171ef-117">**Vydavatel**: Určuje hello vydavatele, který vytvořili hello obrázek, který budete používat pro virtuální počítač (například *Microsoft*).</span><span class="sxs-lookup"><span data-stu-id="171ef-117">**Publisher**: Specifies hello publisher that created hello image that you will use for your virtual machine (for example, *Microsoft*).</span></span>

      * <span data-ttu-id="171ef-118">**Nabízejí**: Určuje nabídky toouse od vydavatele vybrané hello hello virtuálního počítače (například *JDK*).</span><span class="sxs-lookup"><span data-stu-id="171ef-118">**Offer**: Specifies hello virtual machine offering toouse from hello selected publisher (for example, *JDK*).</span></span>

      * <span data-ttu-id="171ef-119">**Skladová položka**: Určuje hello skladové jednotky (SKU) toouse z vybrané nabídky hello (například *JDK_8*).</span><span class="sxs-lookup"><span data-stu-id="171ef-119">**Sku**: Specifies hello stockkeeping unit (SKU) toouse from hello selected offering (for example, *JDK_8*).</span></span>

      * <span data-ttu-id="171ef-120">**Verze #**: Určuje, která verze hello vybrané SKU toouse.</span><span class="sxs-lookup"><span data-stu-id="171ef-120">**Version #**: Specifies which version of hello selected SKU toouse.</span></span>

   ![Hello vyberte časové období bitovou kopii virtuálního počítače][CR03]

5. <span data-ttu-id="171ef-122">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="171ef-122">Click **Next**.</span></span> 

6. <span data-ttu-id="171ef-123">V hello **základní nastavení virtuálního počítače** okno, zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="171ef-123">In hello **Virtual Machine Basic Settings** window, enter hello following information:</span></span>

   * <span data-ttu-id="171ef-124">**Název virtuálního počítače**: Určuje hello název pro nový virtuální počítač, který musí začínat písmenem a obsahovat pouze písmena, číslice a pomlčky.</span><span class="sxs-lookup"><span data-stu-id="171ef-124">**Virtual machine name**: Specifies hello name for your new virtual machine, which must start with a letter and contain only letters, numbers, and hyphens.</span></span>

   * <span data-ttu-id="171ef-125">**Velikost**: Určuje hello počet jader a paměti tooallocate pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="171ef-125">**Size**: Specifies hello number of cores and memory tooallocate for your virtual machine.</span></span>

   * <span data-ttu-id="171ef-126">**Uživatelské jméno**: Určuje hello správce účtu toocreate pro virtuální počítač pod správou.</span><span class="sxs-lookup"><span data-stu-id="171ef-126">**User name**: Specifies hello administrator account toocreate for managing your virtual machine.</span></span>

   * <span data-ttu-id="171ef-127">**Heslo** a **potvrdit**: Určuje hello heslo pro účet správce.</span><span class="sxs-lookup"><span data-stu-id="171ef-127">**Password** and **Confirm**: Specifies hello password for your administrator account.</span></span>

   ![okno Hello základní nastavení virtuálního počítače][CR04]

7. <span data-ttu-id="171ef-129">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="171ef-129">Click **Next**.</span></span> 

8. <span data-ttu-id="171ef-130">V hello **související prostředky** okno, zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="171ef-130">In hello **Associated Resources** window, enter hello following information:</span></span>

   * <span data-ttu-id="171ef-131">**Skupina prostředků**: Určuje hello skupinu prostředků pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="171ef-131">**Resource group**: Specifies hello resource group for your virtual machine.</span></span> <span data-ttu-id="171ef-132">Vyberte jednu z hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="171ef-132">Select one of hello following options:</span></span>
      * <span data-ttu-id="171ef-133">**Vytvořit nový**: Určuje, že chcete toocreate novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="171ef-133">**Create new**: Specifies that you want toocreate a new resource group.</span></span>
      * <span data-ttu-id="171ef-134">**Použít existující**: Určuje, že chcete tooselect ze seznamu skupin prostředků, které jsou spojeny s vaším účtem Azure.</span><span class="sxs-lookup"><span data-stu-id="171ef-134">**Use existing**: Specifies that you want tooselect from a list of resource groups that are associated with your Azure account.</span></span>

       ![okna přidružené prostředky Hello][CR07]

   * <span data-ttu-id="171ef-136">**Účet úložiště**: Určuje toouse účet hello úložiště pro uložení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="171ef-136">**Storage account**: Specifies hello storage account toouse for storing your virtual machine.</span></span> <span data-ttu-id="171ef-137">Můžete vybrat existující účet úložiště nebo vytvořit nový účet.</span><span class="sxs-lookup"><span data-stu-id="171ef-137">You can choose an existing storage account or create a new account.</span></span> <span data-ttu-id="171ef-138">Pokud se rozhodnete **vytvořit nový**, zobrazí se následující dialogové okno hello:</span><span class="sxs-lookup"><span data-stu-id="171ef-138">If you choose **Create New**, hello following dialog box appears:</span></span>

      ![Dialogové okno Vytvořit účet úložiště Hello][CR05]

   * <span data-ttu-id="171ef-140">**Virtuální síť** a **podsíť**: Určuje hello virtuální síť a podsíť, kterému se bude připojovat virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="171ef-140">**Virtual Network** and **Subnet**: Specifies hello virtual network and subnet that your virtual machine will connect to.</span></span> <span data-ttu-id="171ef-141">Můžete použít existující síť a podsíť, nebo můžete vytvořit nové sítě a podsítě.</span><span class="sxs-lookup"><span data-stu-id="171ef-141">You can use an existing network and subnet, or you can create a new network and subnet.</span></span> <span data-ttu-id="171ef-142">Pokud vyberete **vytvořit nový**, zobrazí se následující dialogové okno hello:</span><span class="sxs-lookup"><span data-stu-id="171ef-142">If you select **Create new**, hello following dialog box appears:</span></span>

      ![Dialogové okno vytvořit virtuální síť Hello][CR06]

   * <span data-ttu-id="171ef-144">**Veřejná IP adresa**: Určuje externí IP adresu pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="171ef-144">**Public IP address**: Specifies an external-facing IP address for your virtual machine.</span></span> <span data-ttu-id="171ef-145">Můžete toocreate novou IP adresu nebo, pokud virtuální počítač nebude mít veřejnou IP adresu, můžete si vybrat **(None)**.</span><span class="sxs-lookup"><span data-stu-id="171ef-145">You can choose toocreate a new IP address or, if your virtual machine will not have a public IP address, you can select **(None)**.</span></span> 

   * <span data-ttu-id="171ef-146">**Skupina zabezpečení sítě**: Určuje volitelné síťové brány firewall pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="171ef-146">**Network security group**: Specifies an optional networking firewall for your virtual machine.</span></span> <span data-ttu-id="171ef-147">Můžete vybrat existující bránu firewall, nebo pokud virtuální počítač nebude používat síťovou bránu firewall, můžete vybrat **(None)**.</span><span class="sxs-lookup"><span data-stu-id="171ef-147">You can select an existing firewall or, if your virtual machine will not use a network firewall, you can select **(None)**.</span></span> 

   * <span data-ttu-id="171ef-148">**Skupina dostupnosti**: Určuje, že virtuální počítač může patřit do sadu volitelné dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="171ef-148">**Availability set**: Specifies an optional availability set that your virtual machine can belong to.</span></span> <span data-ttu-id="171ef-149">Můžete vybrat existující sadu dostupnosti, vytvořte novou skupinu dostupnosti nebo, pokud virtuální počítač nebude patří tooan nastavení dostupnosti, vyberte **(None)**.</span><span class="sxs-lookup"><span data-stu-id="171ef-149">You can select an existing availability set, create a new availability set or, if your virtual machine will not belong tooan availability set, select **(None)**.</span></span>

9. <span data-ttu-id="171ef-150">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="171ef-150">Click **Finish**.</span></span>  
    <span data-ttu-id="171ef-151">Nový virtuální počítač se zobrazí v okně nástroje Azure Exploreru hello.</span><span class="sxs-lookup"><span data-stu-id="171ef-151">Your new virtual machine appears in hello Azure Explorer tool window.</span></span> 

   ![Nový virtuální počítač v hello Průzkumník Azure][CR08]

## <a name="restart-a-virtual-machine-in-intellij"></a><span data-ttu-id="171ef-153">Restartování virtuálního počítače v IntelliJ</span><span class="sxs-lookup"><span data-stu-id="171ef-153">Restart a virtual machine in IntelliJ</span></span>

<span data-ttu-id="171ef-154">virtuální počítač pomocí hello Průzkumník Azure v IntelliJ, toorestart hello následující:</span><span class="sxs-lookup"><span data-stu-id="171ef-154">toorestart a virtual machine by using hello Azure Explorer in IntelliJ, do hello following:</span></span>

1. <span data-ttu-id="171ef-155">V hello **Azure Explorer** zobrazení, klikněte pravým tlačítkem na hello virtuálního počítače a pak vyberte **restartujte**.</span><span class="sxs-lookup"><span data-stu-id="171ef-155">In hello **Azure Explorer** view, right-click hello virtual machine, and then select **Restart**.</span></span>

   ![příkaz restartovat virtuální počítač Hello][RE01]

2. <span data-ttu-id="171ef-157">V okně potvrzení hello klikněte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="171ef-157">In hello confirmation window, click **Yes**.</span></span> 

   ![Hello restartovat virtuální počítač potvrzovacím okně][RE02]

## <a name="shut-down-a-virtual-machine-in-intellij"></a><span data-ttu-id="171ef-159">Vypnout virtuální počítač v IntelliJ</span><span class="sxs-lookup"><span data-stu-id="171ef-159">Shut down a virtual machine in IntelliJ</span></span>

<span data-ttu-id="171ef-160">tooshut dolů spuštěného virtuálního počítače pomocí hello Průzkumník Azure v IntelliJ, hello následující:</span><span class="sxs-lookup"><span data-stu-id="171ef-160">tooshut down a running virtual machine by using hello Azure Explorer in IntelliJ, do hello following:</span></span>

1. <span data-ttu-id="171ef-161">V hello **Azure Explorer** zobrazení, klikněte pravým tlačítkem na hello virtuálního počítače a pak vyberte **vypnutí**.</span><span class="sxs-lookup"><span data-stu-id="171ef-161">In hello **Azure Explorer** view, right-click hello virtual machine, and then select **Shutdown**.</span></span>

   ![příkaz pro vypnutí virtuálního počítače Hello][SH01]

2. <span data-ttu-id="171ef-163">V okně potvrzení hello klikněte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="171ef-163">In hello confirmation window, click **Yes**.</span></span> 

   ![Vypněte virtuální počítač potvrzovacím okně Hello][SH02]

## <a name="delete-a-virtual-machine-in-intellij"></a><span data-ttu-id="171ef-165">Odstranění virtuálního počítače v IntelliJ</span><span class="sxs-lookup"><span data-stu-id="171ef-165">Delete a virtual machine in IntelliJ</span></span>

<span data-ttu-id="171ef-166">virtuální počítač pomocí hello Průzkumník Azure v IntelliJ, toodelete hello následující:</span><span class="sxs-lookup"><span data-stu-id="171ef-166">toodelete a virtual machine by using hello Azure Explorer in IntelliJ, do hello following:</span></span>

1. <span data-ttu-id="171ef-167">V hello **Azure Explorer** zobrazení, klikněte pravým tlačítkem na hello virtuálního počítače a pak vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="171ef-167">In hello **Azure Explorer** view, right-click hello virtual machine, and then select **Delete**.</span></span>

   ![příkaz Delete Hello virtuálního počítače][DE01]

2. <span data-ttu-id="171ef-169">V okně potvrzení hello klikněte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="171ef-169">In hello confirmation window, click **Yes**.</span></span> 

   ![Hello odstranit okno pro potvrzení virtuálního počítače][DE02]

## <a name="next-steps"></a><span data-ttu-id="171ef-171">Další kroky</span><span class="sxs-lookup"><span data-stu-id="171ef-171">Next steps</span></span>
<span data-ttu-id="171ef-172">Další informace o Azure velikosti virtuálního počítače a cenách najdete v tématu hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="171ef-172">For more information about Azure virtual-machine sizes and pricing, see hello following resources:</span></span>

* <span data-ttu-id="171ef-173">Azure velikostí virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="171ef-173">Azure virtual-machine sizes</span></span>
  * <span data-ttu-id="171ef-174">[Velikosti pro virtuální počítače s Windows v Azure]</span><span class="sxs-lookup"><span data-stu-id="171ef-174">[Sizes for Windows virtual machines in Azure]</span></span>
  * <span data-ttu-id="171ef-175">[Velikosti pro virtuální počítače s Linuxem v Azure]</span><span class="sxs-lookup"><span data-stu-id="171ef-175">[Sizes for Linux virtual machines in Azure]</span></span>
* <span data-ttu-id="171ef-176">Azure virtual Machines – ceny</span><span class="sxs-lookup"><span data-stu-id="171ef-176">Azure virtual-machine pricing</span></span>
  * <span data-ttu-id="171ef-177">[Ceny virtuálního počítače Windows]</span><span class="sxs-lookup"><span data-stu-id="171ef-177">[Windows virtual-machine pricing]</span></span>
  * <span data-ttu-id="171ef-178">[Virtuální počítače Linux – ceny]</span><span class="sxs-lookup"><span data-stu-id="171ef-178">[Linux virtual-machine pricing]</span></span>

<span data-ttu-id="171ef-179">Další informace o hello sadách Azure pro integrovaného vývojového prostředí Java najdete v tématu hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="171ef-179">For more information about hello Azure Toolkits for Java IDEs, see hello following resources:</span></span>

* <span data-ttu-id="171ef-180">[Azure nástrojů pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="171ef-180">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="171ef-181">[Co je nového v hello nástrojů Azure pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="171ef-181">[What's new in hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="171ef-182">[Instalace hello nástrojů Azure pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="171ef-182">[Installing hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="171ef-183">[Přihlášení pokyny pro hello nástrojů Azure pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="171ef-183">[Sign-in instructions for hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="171ef-184">[Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse]</span><span class="sxs-lookup"><span data-stu-id="171ef-184">[Create a Hello World web app for Azure in Eclipse]</span></span>
* <span data-ttu-id="171ef-185">[Sada Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="171ef-185">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="171ef-186">[Co je nového v hello nástrojů Azure pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="171ef-186">[What's new in hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="171ef-187">[Instalace hello Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="171ef-187">[Installing hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="171ef-188">[přihlášení pokyny pro hello Azure Toolkit IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="171ef-188">[Sign-in instructions for hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="171ef-189">[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="171ef-189">[Create a Hello World web app for Azure in IntelliJ]</span></span>

<span data-ttu-id="171ef-190">Další informace o používání Azure s Java najdete v tématu [Azure střediska pro vývojáře Java] a [Java nástrojů pro Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="171ef-190">For more information about using Azure with Java, see [Azure Java Developer Center] and [Java Tools for Visual Studio Team Services].</span></span>

<!-- URL List -->

[Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse.md
[Sada Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij.md
[Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Instalace hello nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Instalace hello Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Přihlášení pokyny pro hello nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[přihlášení pokyny pro hello Azure Toolkit IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Co je nového v hello nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Co je nového v hello nástrojů Azure pro IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure střediska pro vývojáře Java]: https://azure.microsoft.com/develop/java/
[Java nástrojů pro Visual Studio Team Services]: https://java.visualstudio.com/

[Velikosti pro virtuální počítače s Windows v Azure]: /azure/virtual-machines/virtual-machines-windows-sizes
[Velikosti pro virtuální počítače s Linuxem v Azure]: /azure/virtual-machines/virtual-machines-linux-sizes
[Ceny virtuálního počítače Windows]: /pricing/details/virtual-machines/windows/
[Virtuální počítače Linux – ceny]: /pricing/details/virtual-machines/linux/


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
