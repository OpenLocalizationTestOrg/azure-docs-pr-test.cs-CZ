---
title: "účty úložiště aaaManage pomocí hello Průzkumník Azure pro IntelliJ | Microsoft Docs"
description: "Zjistěte, jak toomanage úložiště Azure účty pomocí hello Průzkumník Azure pro IntelliJ."
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
ms.openlocfilehash: b094bdcd2fa2782cb12b67c96ac406fbe4c1aa3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-storage-accounts-by-using-hello-azure-explorer-for-intellij"></a><span data-ttu-id="2800e-103">Spravovat účty úložiště pomocí hello Průzkumník Azure pro IntelliJ</span><span class="sxs-lookup"><span data-stu-id="2800e-103">Manage storage accounts by using hello Azure Explorer for IntelliJ</span></span>

<span data-ttu-id="2800e-104">Hello Průzkumník Azure, který je součástí hello nástrojů Azure pro IntelliJ, poskytuje vývojáře v jazyce Java s řešením pro snadné použití ke správě účtů úložiště v účtu Azure z uvnitř hello IntelliJ integrované vývojové prostředí (IDE).</span><span class="sxs-lookup"><span data-stu-id="2800e-104">hello Azure Explorer, which is part of hello Azure Toolkit for IntelliJ, provides Java developers with an easy-to-use solution for managing storage accounts in their Azure account from inside hello IntelliJ integrated development environment (IDE).</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-storage-account-in-intellij"></a><span data-ttu-id="2800e-105">Vytvořit účet úložiště v IntelliJ</span><span class="sxs-lookup"><span data-stu-id="2800e-105">Create a storage account in IntelliJ</span></span>

<span data-ttu-id="2800e-106">toocreate účet úložiště pomocí hello Azure Explorer hello následující:</span><span class="sxs-lookup"><span data-stu-id="2800e-106">toocreate a storage account by using hello Azure Explorer, do hello following:</span></span>

1. <span data-ttu-id="2800e-107">Přihlaste se pomocí hello tooyour účet Azure [přihlášení pokyny pro hello nástrojů Azure pro Eclipse].</span><span class="sxs-lookup"><span data-stu-id="2800e-107">Sign in tooyour Azure account by using hello [Sign-in instructions for hello Azure Toolkit for Eclipse].</span></span> 

2. <span data-ttu-id="2800e-108">V hello **Azure Explorer** , rozbalte hello **Azure** uzel, klikněte pravým tlačítkem na **účty úložiště**a pak klikněte na tlačítko **vytvořit účet úložiště**.</span><span class="sxs-lookup"><span data-stu-id="2800e-108">In hello **Azure Explorer** view, expand hello **Azure** node, right-click **Storage Accounts**, and then click **Create Storage Account**.</span></span>

   ![Vytvořit účet úložiště příkaz][CS01]

3. <span data-ttu-id="2800e-110">V hello **vytvořit účet úložiště** dialogovém okně zadejte hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="2800e-110">In hello **Create Storage Account** dialog box, specify hello following options:</span></span>

   ![Vytvořit nový účet úložiště, dialogové okno][CS02]

   * <span data-ttu-id="2800e-112">**Název**: Určuje název hello hello nový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="2800e-112">**Name**: Specifies hello name for hello new storage account.</span></span>

   * <span data-ttu-id="2800e-113">**Účet druh**: Určuje typ hello toocreate účtu úložiště (například "úložiště objektů Blob").</span><span class="sxs-lookup"><span data-stu-id="2800e-113">**Account kind**: Specifies hello type of storage account toocreate (for example, "Blob storage").</span></span> <span data-ttu-id="2800e-114">Další informace najdete v tématu [účty Azure storage].</span><span class="sxs-lookup"><span data-stu-id="2800e-114">For more information, see [About Azure storage accounts].</span></span> 

   * <span data-ttu-id="2800e-115">**Výkon**: Určuje, který účet úložiště nabídky toouse od vydavatele vybrané hello (například "Premium").</span><span class="sxs-lookup"><span data-stu-id="2800e-115">**Performance**: Specifies which storage account offering toouse from hello selected publisher (for example, "Premium").</span></span> <span data-ttu-id="2800e-116">Další informace najdete v tématu [úložiště Azure škálovatelnosti a cílech výkonnosti].</span><span class="sxs-lookup"><span data-stu-id="2800e-116">For more information, see [Azure storage scalability and performance targets].</span></span> 

   * <span data-ttu-id="2800e-117">**Replikace**: Určuje hello replikace pro účet úložiště hello (například "Zónově redundantní").</span><span class="sxs-lookup"><span data-stu-id="2800e-117">**Replication**: Specifies hello replication for hello storage account (for example, "Zone-Redundant").</span></span> <span data-ttu-id="2800e-118">Další informace najdete v tématu [replikace Azure storage].</span><span class="sxs-lookup"><span data-stu-id="2800e-118">For more information, see [Azure storage replication].</span></span> 

   * <span data-ttu-id="2800e-119">**Předplatné**: Určuje hello předplatné Azure, který má toouse pro nový účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="2800e-119">**Subscription**: Specifies hello Azure subscription that you want toouse for hello new storage account.</span></span>

   * <span data-ttu-id="2800e-120">**Umístění**: Určuje hello umístění, které se vytvoří účet úložiště (například "Západní USA").</span><span class="sxs-lookup"><span data-stu-id="2800e-120">**Location**: Specifies hello location where your storage account will be created (for example, "West US").</span></span>

   * <span data-ttu-id="2800e-121">**Skupina prostředků**: Určuje hello skupinu prostředků pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="2800e-121">**Resource Group**: Specifies hello resource group for your virtual machine.</span></span> <span data-ttu-id="2800e-122">Vyberte jednu z hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="2800e-122">Select one of hello following options:</span></span>
      * <span data-ttu-id="2800e-123">**Vytvořit nový**: Určuje, že chcete toocreate novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="2800e-123">**Create new**: Specifies that you want toocreate a new resource group.</span></span>
      * <span data-ttu-id="2800e-124">**Použít existující**: Určuje, že vyberete ze seznamu skupin prostředků, které jsou spojeny s vaším účtem Azure.</span><span class="sxs-lookup"><span data-stu-id="2800e-124">**Use existing**: Specifies that you will select from a list of resource groups that are associated with your Azure account.</span></span>

4. <span data-ttu-id="2800e-125">Pokud jste zadali všechny hello předcházející možnosti, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="2800e-125">When you have specified all of hello preceding options, click **OK**.</span></span>

## <a name="create-a-storage-container-in-intellij"></a><span data-ttu-id="2800e-126">Vytvoření kontejneru úložiště v IntelliJ</span><span class="sxs-lookup"><span data-stu-id="2800e-126">Create a storage container in IntelliJ</span></span>

<span data-ttu-id="2800e-127">toocreate kontejner úložiště pomocí hello Azure Explorer hello následující:</span><span class="sxs-lookup"><span data-stu-id="2800e-127">toocreate a storage container by using hello Azure Explorer, do hello following:</span></span>

1. <span data-ttu-id="2800e-128">V zobrazení Průzkumník Azure hello, klikněte pravým tlačítkem na účet úložiště hello, kam chcete toocreate kontejner a pak klikněte na tlačítko **vytvořit kontejner objektů blob**.</span><span class="sxs-lookup"><span data-stu-id="2800e-128">In hello Azure Explorer view, right-click hello storage account where you want toocreate a container, and then click **Create blob container**.</span></span>

   ![Vytvoření příkazu kontejner objektů blob][CC01]

2. <span data-ttu-id="2800e-130">V hello **vytvořit kontejner objektů blob** dialogové okno, zadejte název hello pro váš kontejner a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="2800e-130">In hello **Create blob container** dialog box, specify hello name for your container, and then click **OK**.</span></span> <span data-ttu-id="2800e-131">Další informace o pojmenovávání kontejnerů úložiště najdete v tématu [pojmenování a odkazování na kontejnerů, objektů BLOB a metadat].</span><span class="sxs-lookup"><span data-stu-id="2800e-131">For more information about naming storage containers, see [Naming and referencing containers, blobs, and metadata].</span></span>

   ![Vytvoření dialogového okna kontejneru úložiště][CC02]

## <a name="delete-a-storage-container-in-intellij"></a><span data-ttu-id="2800e-133">Odstranit kontejner úložiště v IntelliJ</span><span class="sxs-lookup"><span data-stu-id="2800e-133">Delete a storage container in IntelliJ</span></span>

<span data-ttu-id="2800e-134">toodelete kontejner úložiště pomocí hello Azure Explorer hello následující:</span><span class="sxs-lookup"><span data-stu-id="2800e-134">toodelete a storage container by using hello Azure Explorer, do hello following:</span></span>

1. <span data-ttu-id="2800e-135">Hello Průzkumník Azure, klikněte pravým tlačítkem na kontejner hello úložiště a pak klikněte na **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="2800e-135">In hello Azure Explorer view, right-click hello storage container, and then click **Delete**.</span></span>

   ![Odstranit příkaz kontejneru úložiště][DC01]

2. <span data-ttu-id="2800e-137">V okně potvrzení hello klikněte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="2800e-137">In hello confirmation window, click **Yes**.</span></span>

   ![Odstranit okno pro potvrzení kontejneru úložiště][DC02]

## <a name="delete-a-storage-account-in-intellij"></a><span data-ttu-id="2800e-139">Odstranit účet úložiště v IntelliJ</span><span class="sxs-lookup"><span data-stu-id="2800e-139">Delete a storage account in IntelliJ</span></span>

<span data-ttu-id="2800e-140">toodelete účet úložiště pomocí hello Azure Explorer hello následující:</span><span class="sxs-lookup"><span data-stu-id="2800e-140">toodelete a storage account by using hello Azure Explorer, do hello following:</span></span>

1. <span data-ttu-id="2800e-141">V hello **Azure Explorer** zobrazení, klikněte pravým tlačítkem na účet úložiště hello a pak vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="2800e-141">In hello **Azure Explorer** view, right-click hello storage account, and then select **Delete**.</span></span>

   ![Odstraňte nabídce účtu úložiště][DS01]

2. <span data-ttu-id="2800e-143">V okně potvrzení hello klikněte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="2800e-143">In hello confirmation window, click **Yes**.</span></span>

   ![Odstranit okno pro potvrzení účtu úložiště][DS02]

## <a name="next-steps"></a><span data-ttu-id="2800e-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2800e-145">Next steps</span></span>
<span data-ttu-id="2800e-146">Další informace o účtech Azure storage, velikosti a cenách najdete v části hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="2800e-146">For more information about Azure storage accounts, sizes, and pricing, see hello following resources:</span></span>

* <span data-ttu-id="2800e-147">[Úvod tooMicrosoft Azure Storage]</span><span class="sxs-lookup"><span data-stu-id="2800e-147">[Introduction tooMicrosoft Azure Storage]</span></span>
* <span data-ttu-id="2800e-148">[účty Azure storage]</span><span class="sxs-lookup"><span data-stu-id="2800e-148">[About Azure storage accounts]</span></span>
* <span data-ttu-id="2800e-149">Účet služby Azure storage velikosti</span><span class="sxs-lookup"><span data-stu-id="2800e-149">Azure storage-account sizes</span></span>
  * <span data-ttu-id="2800e-150">[Velikosti pro účty úložiště v systému Windows v Azure]</span><span class="sxs-lookup"><span data-stu-id="2800e-150">[Sizes for Windows storage accounts in Azure]</span></span>
  * <span data-ttu-id="2800e-151">[Velikosti pro Linux účty úložiště v Azure]</span><span class="sxs-lookup"><span data-stu-id="2800e-151">[Sizes for Linux storage accounts in Azure]</span></span>
* <span data-ttu-id="2800e-152">Účet služby Azure storage – ceny</span><span class="sxs-lookup"><span data-stu-id="2800e-152">Azure storage-account pricing</span></span>
  * <span data-ttu-id="2800e-153">[Ceny účtu úložiště systému Windows]</span><span class="sxs-lookup"><span data-stu-id="2800e-153">[Windows storage-account pricing]</span></span>
  * <span data-ttu-id="2800e-154">[Ceny Linux účet úložiště]</span><span class="sxs-lookup"><span data-stu-id="2800e-154">[Linux storage-account pricing]</span></span>

<span data-ttu-id="2800e-155">Další informace o sadách Azure pro integrovaného vývojového prostředí Java najdete v tématu hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="2800e-155">For more information about Azure Toolkits for Java IDEs, see hello following resources:</span></span>

* <span data-ttu-id="2800e-156">[Azure nástrojů pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="2800e-156">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="2800e-157">[Co je nového v hello nástrojů Azure pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="2800e-157">[What's new in hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="2800e-158">[Instalace hello nástrojů Azure pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="2800e-158">[Installing hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="2800e-159">[přihlášení pokyny pro hello nástrojů Azure pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="2800e-159">[Sign-in instructions for hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="2800e-160">[Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse]</span><span class="sxs-lookup"><span data-stu-id="2800e-160">[Create a Hello World web app for Azure in Eclipse]</span></span>
* <span data-ttu-id="2800e-161">[Sada Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="2800e-161">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="2800e-162">[Co je nového v hello nástrojů Azure pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="2800e-162">[What's new in hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="2800e-163">[Instalace hello Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="2800e-163">[Installing hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="2800e-164">[Přihlášení pokyny pro hello Azure Toolkit IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="2800e-164">[Sign-in instructions for hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="2800e-165">[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="2800e-165">[Create a Hello World web app for Azure in IntelliJ]</span></span>

<span data-ttu-id="2800e-166">Další informace o používání Azure s Java najdete v tématu [Azure střediska pro vývojáře Java] a [Java nástrojů pro Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="2800e-166">For more information about using Azure with Java, see [Azure Java Developer Center] and [Java Tools for Visual Studio Team Services].</span></span>

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

[Úvod tooMicrosoft Azure Storage]: /azure/storage/storage-introduction
[účty Azure storage]: /azure/storage/storage-create-storage-account
[replikace Azure storage]: /azure/storage/storage-redundancy
[Škálovatelnost a cíle výkonnosti Azure storage]: /azure/storage/storage-scalability-targets
[pojmenování a odkazování na kontejnerů, objektů BLOB a metadat]: http://go.microsoft.com/fwlink/?LinkId=255555

[Velikosti pro účty úložiště v systému Windows v Azure]: /azure/virtual-machines/virtual-machines-windows-sizes
[Velikosti pro Linux účty úložiště v Azure]: /azure/virtual-machines/virtual-machines-linux-sizes
[Ceny účtu úložiště systému Windows]: /pricing/details/virtual-machines/windows/
[Ceny Linux účet úložiště]: /pricing/details/virtual-machines/linux/

<!-- IMG List -->

[CS01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CS01.png
[CS02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CS02.png
[CC01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CC01.png
[CC02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CC02.png

[DS01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DS01.png
[DS02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DS02.png
[DC01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DC01.png
[DC02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DC02.png
