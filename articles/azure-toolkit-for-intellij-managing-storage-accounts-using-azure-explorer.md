---
title: "Spravovat účty úložiště pomocí Průzkumníka Azure pro IntelliJ | Microsoft Docs"
description: "Naučte se spravovat účtům Azure storage pomocí Průzkumníka Azure pro IntelliJ."
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
ms.openlocfilehash: a1b56cc2751fc43a1ad6917eca77eec460f26694
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-storage-accounts-by-using-the-azure-explorer-for-intellij"></a><span data-ttu-id="d0b9e-103">Spravovat účty úložiště pomocí Průzkumníka Azure pro IntelliJ</span><span class="sxs-lookup"><span data-stu-id="d0b9e-103">Manage storage accounts by using the Azure Explorer for IntelliJ</span></span>

<span data-ttu-id="d0b9e-104">Průzkumník Azure, který je součástí sady nástrojů Azure pro IntelliJ, poskytuje vývojáře v jazyce Java s řešením snadno použitelné pro správu účtů úložiště v Azure účtu z uvnitř IntelliJ integrované vývojové prostředí (IDE).</span><span class="sxs-lookup"><span data-stu-id="d0b9e-104">The Azure Explorer, which is part of the Azure Toolkit for IntelliJ, provides Java developers with an easy-to-use solution for managing storage accounts in their Azure account from inside the IntelliJ integrated development environment (IDE).</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-storage-account-in-intellij"></a><span data-ttu-id="d0b9e-105">Vytvořit účet úložiště v IntelliJ</span><span class="sxs-lookup"><span data-stu-id="d0b9e-105">Create a storage account in IntelliJ</span></span>

<span data-ttu-id="d0b9e-106">Pokud chcete vytvořit účet úložiště pomocí Průzkumníka Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="d0b9e-106">To create a storage account by using the Azure Explorer, do the following:</span></span>

1. <span data-ttu-id="d0b9e-107">Přihlaste se k účtu Azure pomocí [pokyny přihlášení k Azure nástrojů pro Eclipse].</span><span class="sxs-lookup"><span data-stu-id="d0b9e-107">Sign in to your Azure account by using the [Sign-in instructions for the Azure Toolkit for Eclipse].</span></span> 

2. <span data-ttu-id="d0b9e-108">V **Azure Explorer** , rozbalte **Azure** uzel, klikněte pravým tlačítkem na **účty úložiště**a potom klikněte na **vytvořit účet úložiště**.</span><span class="sxs-lookup"><span data-stu-id="d0b9e-108">In the **Azure Explorer** view, expand the **Azure** node, right-click **Storage Accounts**, and then click **Create Storage Account**.</span></span>

   ![Vytvořit účet úložiště příkaz][CS01]

3. <span data-ttu-id="d0b9e-110">V **vytvořit účet úložiště** dialogové okno pole, určete následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="d0b9e-110">In the **Create Storage Account** dialog box, specify the following options:</span></span>

   ![Vytvořit nový účet úložiště, dialogové okno][CS02]

   * <span data-ttu-id="d0b9e-112">**Název**: Určuje název pro nový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="d0b9e-112">**Name**: Specifies the name for the new storage account.</span></span>

   * <span data-ttu-id="d0b9e-113">**Účet druh**: Určuje typ účtu úložiště k vytvoření (například "Úložiště objektů Blob").</span><span class="sxs-lookup"><span data-stu-id="d0b9e-113">**Account kind**: Specifies the type of storage account to create (for example, "Blob storage").</span></span> <span data-ttu-id="d0b9e-114">Další informace najdete v tématu [účty Azure storage].</span><span class="sxs-lookup"><span data-stu-id="d0b9e-114">For more information, see [About Azure storage accounts].</span></span> 

   * <span data-ttu-id="d0b9e-115">**Výkon**: Určuje, který účet úložiště nabídky používat od vydavatele, vybrané (například "Premium").</span><span class="sxs-lookup"><span data-stu-id="d0b9e-115">**Performance**: Specifies which storage account offering to use from the selected publisher (for example, "Premium").</span></span> <span data-ttu-id="d0b9e-116">Další informace najdete v tématu [úložiště Azure škálovatelnosti a cílech výkonnosti].</span><span class="sxs-lookup"><span data-stu-id="d0b9e-116">For more information, see [Azure storage scalability and performance targets].</span></span> 

   * <span data-ttu-id="d0b9e-117">**Replikace**: Určuje replikace pro účet úložiště (například "Zónově redundantní").</span><span class="sxs-lookup"><span data-stu-id="d0b9e-117">**Replication**: Specifies the replication for the storage account (for example, "Zone-Redundant").</span></span> <span data-ttu-id="d0b9e-118">Další informace najdete v tématu [replikace Azure storage].</span><span class="sxs-lookup"><span data-stu-id="d0b9e-118">For more information, see [Azure storage replication].</span></span> 

   * <span data-ttu-id="d0b9e-119">**Předplatné**: Určuje předplatné Azure, kterou chcete použít pro nový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="d0b9e-119">**Subscription**: Specifies the Azure subscription that you want to use for the new storage account.</span></span>

   * <span data-ttu-id="d0b9e-120">**Umístění**: Určuje umístění, které se vytvoří účet úložiště (například "Západní USA").</span><span class="sxs-lookup"><span data-stu-id="d0b9e-120">**Location**: Specifies the location where your storage account will be created (for example, "West US").</span></span>

   * <span data-ttu-id="d0b9e-121">**Skupina prostředků**: Určuje skupinu prostředků pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="d0b9e-121">**Resource Group**: Specifies the resource group for your virtual machine.</span></span> <span data-ttu-id="d0b9e-122">Vyberte jednu z následujících možností:</span><span class="sxs-lookup"><span data-stu-id="d0b9e-122">Select one of the following options:</span></span>
      * <span data-ttu-id="d0b9e-123">**Vytvořit nový**: Určuje, že chcete vytvořit novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="d0b9e-123">**Create new**: Specifies that you want to create a new resource group.</span></span>
      * <span data-ttu-id="d0b9e-124">**Použít existující**: Určuje, že vyberete ze seznamu skupin prostředků, které jsou spojeny s vaším účtem Azure.</span><span class="sxs-lookup"><span data-stu-id="d0b9e-124">**Use existing**: Specifies that you will select from a list of resource groups that are associated with your Azure account.</span></span>

4. <span data-ttu-id="d0b9e-125">Když zadáte všechny z předchozích možností klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="d0b9e-125">When you have specified all of the preceding options, click **OK**.</span></span>

## <a name="create-a-storage-container-in-intellij"></a><span data-ttu-id="d0b9e-126">Vytvoření kontejneru úložiště v IntelliJ</span><span class="sxs-lookup"><span data-stu-id="d0b9e-126">Create a storage container in IntelliJ</span></span>

<span data-ttu-id="d0b9e-127">Pokud chcete vytvořit kontejner úložiště pomocí Průzkumníka Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="d0b9e-127">To create a storage container by using the Azure Explorer, do the following:</span></span>

1. <span data-ttu-id="d0b9e-128">V zobrazení Průzkumník Azure, klikněte pravým tlačítkem na účet úložiště, kde chcete vytvořit kontejner a potom klikněte na **vytvořit kontejner objektů blob**.</span><span class="sxs-lookup"><span data-stu-id="d0b9e-128">In the Azure Explorer view, right-click the storage account where you want to create a container, and then click **Create blob container**.</span></span>

   ![Vytvoření příkazu kontejner objektů blob][CC01]

2. <span data-ttu-id="d0b9e-130">V **vytvořit kontejner objektů blob** dialogové okno, zadejte název pro váš kontejner a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="d0b9e-130">In the **Create blob container** dialog box, specify the name for your container, and then click **OK**.</span></span> <span data-ttu-id="d0b9e-131">Další informace o pojmenovávání kontejnerů úložiště najdete v tématu [pojmenování a odkazování na kontejnerů, objektů BLOB a metadat].</span><span class="sxs-lookup"><span data-stu-id="d0b9e-131">For more information about naming storage containers, see [Naming and referencing containers, blobs, and metadata].</span></span>

   ![Vytvoření dialogového okna kontejneru úložiště][CC02]

## <a name="delete-a-storage-container-in-intellij"></a><span data-ttu-id="d0b9e-133">Odstranit kontejner úložiště v IntelliJ</span><span class="sxs-lookup"><span data-stu-id="d0b9e-133">Delete a storage container in IntelliJ</span></span>

<span data-ttu-id="d0b9e-134">Pokud chcete odstranit kontejner úložiště pomocí Průzkumníka Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="d0b9e-134">To delete a storage container by using the Azure Explorer, do the following:</span></span>

1. <span data-ttu-id="d0b9e-135">V zobrazení Průzkumník Azure, klikněte pravým tlačítkem na kontejner úložiště a pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="d0b9e-135">In the Azure Explorer view, right-click the storage container, and then click **Delete**.</span></span>

   ![Odstranit příkaz kontejneru úložiště][DC01]

2. <span data-ttu-id="d0b9e-137">V okně potvrzení klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="d0b9e-137">In the confirmation window, click **Yes**.</span></span>

   ![Odstranit okno pro potvrzení kontejneru úložiště][DC02]

## <a name="delete-a-storage-account-in-intellij"></a><span data-ttu-id="d0b9e-139">Odstranit účet úložiště v IntelliJ</span><span class="sxs-lookup"><span data-stu-id="d0b9e-139">Delete a storage account in IntelliJ</span></span>

<span data-ttu-id="d0b9e-140">Pokud chcete odstranit účet úložiště pomocí Průzkumníka Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="d0b9e-140">To delete a storage account by using the Azure Explorer, do the following:</span></span>

1. <span data-ttu-id="d0b9e-141">V **Azure Explorer** zobrazení, klikněte pravým tlačítkem na účet úložiště a pak vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="d0b9e-141">In the **Azure Explorer** view, right-click the storage account, and then select **Delete**.</span></span>

   ![Odstraňte nabídce účtu úložiště][DS01]

2. <span data-ttu-id="d0b9e-143">V okně potvrzení klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="d0b9e-143">In the confirmation window, click **Yes**.</span></span>

   ![Odstranit okno pro potvrzení účtu úložiště][DS02]

## <a name="next-steps"></a><span data-ttu-id="d0b9e-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d0b9e-145">Next steps</span></span>
<span data-ttu-id="d0b9e-146">Další informace o účtech úložiště Azure velikosti a ceny, najdete v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="d0b9e-146">For more information about Azure storage accounts, sizes, and pricing, see the following resources:</span></span>

* <span data-ttu-id="d0b9e-147">[Úvod do Microsoft Azure Storage]</span><span class="sxs-lookup"><span data-stu-id="d0b9e-147">[Introduction to Microsoft Azure Storage]</span></span>
* <span data-ttu-id="d0b9e-148">[účty Azure storage]</span><span class="sxs-lookup"><span data-stu-id="d0b9e-148">[About Azure storage accounts]</span></span>
* <span data-ttu-id="d0b9e-149">Účet služby Azure storage velikosti</span><span class="sxs-lookup"><span data-stu-id="d0b9e-149">Azure storage-account sizes</span></span>
  * <span data-ttu-id="d0b9e-150">[Velikosti pro účty úložiště v systému Windows v Azure]</span><span class="sxs-lookup"><span data-stu-id="d0b9e-150">[Sizes for Windows storage accounts in Azure]</span></span>
  * <span data-ttu-id="d0b9e-151">[Velikosti pro Linux účty úložiště v Azure]</span><span class="sxs-lookup"><span data-stu-id="d0b9e-151">[Sizes for Linux storage accounts in Azure]</span></span>
* <span data-ttu-id="d0b9e-152">Účet služby Azure storage – ceny</span><span class="sxs-lookup"><span data-stu-id="d0b9e-152">Azure storage-account pricing</span></span>
  * <span data-ttu-id="d0b9e-153">[Ceny účtu úložiště systému Windows]</span><span class="sxs-lookup"><span data-stu-id="d0b9e-153">[Windows storage-account pricing]</span></span>
  * <span data-ttu-id="d0b9e-154">[Ceny Linux účet úložiště]</span><span class="sxs-lookup"><span data-stu-id="d0b9e-154">[Linux storage-account pricing]</span></span>

<span data-ttu-id="d0b9e-155">Další informace o sadách Azure pro integrovaného vývojového prostředí Java najdete v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="d0b9e-155">For more information about Azure Toolkits for Java IDEs, see the following resources:</span></span>

* <span data-ttu-id="d0b9e-156">[Azure nástrojů pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="d0b9e-156">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="d0b9e-157">[Co je nového v sadě nástrojů Azure pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="d0b9e-157">[What's new in the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="d0b9e-158">[Instalace sady Azure Toolkit pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="d0b9e-158">[Installing the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="d0b9e-159">[pokyny přihlášení k Azure nástrojů pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="d0b9e-159">[Sign-in instructions for the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="d0b9e-160">[Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse]</span><span class="sxs-lookup"><span data-stu-id="d0b9e-160">[Create a Hello World web app for Azure in Eclipse]</span></span>
* <span data-ttu-id="d0b9e-161">[Sada Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="d0b9e-161">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="d0b9e-162">[Co je nového v sadě nástrojů Azure pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="d0b9e-162">[What's new in the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="d0b9e-163">[Instalace sady Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="d0b9e-163">[Installing the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="d0b9e-164">[Přihlášení pokyny pro Azure Toolkit IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="d0b9e-164">[Sign-in instructions for the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="d0b9e-165">[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="d0b9e-165">[Create a Hello World web app for Azure in IntelliJ]</span></span>

<span data-ttu-id="d0b9e-166">Další informace o používání Azure s Java najdete v tématu [Azure střediska pro vývojáře Java] a [Java nástrojů pro Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="d0b9e-166">For more information about using Azure with Java, see [Azure Java Developer Center] and [Java Tools for Visual Studio Team Services].</span></span>

<!-- URL List -->

<span data-ttu-id="d0b9e-167">[Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse.md</span><span class="sxs-lookup"><span data-stu-id="d0b9e-167">[Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse.md</span></span>
<span data-ttu-id="d0b9e-168">[Sada Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij.md</span><span class="sxs-lookup"><span data-stu-id="d0b9e-168">[Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij.md</span></span>
<span data-ttu-id="d0b9e-169">[Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="d0b9e-169">[Create a Hello World web app for Azure in Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span></span>
<span data-ttu-id="d0b9e-170">[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="d0b9e-170">[Create a Hello World web app for Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span></span>
<span data-ttu-id="d0b9e-171">[Instalace sady Azure Toolkit pro Eclipse]: ./azure-toolkit-for-eclipse-installation.md</span><span class="sxs-lookup"><span data-stu-id="d0b9e-171">[Installing the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md</span></span>
<span data-ttu-id="d0b9e-172">[Instalace sady Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-installation.md</span><span class="sxs-lookup"><span data-stu-id="d0b9e-172">[Installing the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-installation.md</span></span>
<span data-ttu-id="d0b9e-173">[pokyny přihlášení k Azure nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="d0b9e-173">[Sign-in instructions for the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span></span>
<span data-ttu-id="d0b9e-174">[Přihlášení pokyny pro Azure Toolkit IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="d0b9e-174">[Sign-in instructions for the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span></span>
<span data-ttu-id="d0b9e-175">[Co je nového v sadě nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="d0b9e-175">[What's new in the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md</span></span>
<span data-ttu-id="d0b9e-176">[Co je nového v sadě nástrojů Azure pro IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="d0b9e-176">[What's new in the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md</span></span>

<span data-ttu-id="d0b9e-177">[Azure střediska pro vývojáře Java]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="d0b9e-177">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>
<span data-ttu-id="d0b9e-178">[Java nástrojů pro Visual Studio Team Services]: https://java.visualstudio.com/</span><span class="sxs-lookup"><span data-stu-id="d0b9e-178">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span></span>

<span data-ttu-id="d0b9e-179">[Úvod do Microsoft Azure Storage]: /azure/storage/storage-introduction</span><span class="sxs-lookup"><span data-stu-id="d0b9e-179">[Introduction to Microsoft Azure Storage]: /azure/storage/storage-introduction</span></span>
<span data-ttu-id="d0b9e-180">[účty Azure storage]: /azure/storage/storage-create-storage-account</span><span class="sxs-lookup"><span data-stu-id="d0b9e-180">[About Azure storage accounts]: /azure/storage/storage-create-storage-account</span></span>
<span data-ttu-id="d0b9e-181">[replikace Azure storage]: /azure/storage/storage-redundancy</span><span class="sxs-lookup"><span data-stu-id="d0b9e-181">[Azure storage replication]: /azure/storage/storage-redundancy</span></span>
<span data-ttu-id="d0b9e-182">[Škálovatelnost a cíle výkonnosti Azure storage]: /azure/storage/storage-scalability-targets</span><span class="sxs-lookup"><span data-stu-id="d0b9e-182">[Azure storage scalability and Performance Targets]: /azure/storage/storage-scalability-targets</span></span>
<span data-ttu-id="d0b9e-183">[pojmenování a odkazování na kontejnerů, objektů BLOB a metadat]: http://go.microsoft.com/fwlink/?LinkId=255555</span><span class="sxs-lookup"><span data-stu-id="d0b9e-183">[Naming and referencing containers, blobs, and metadata]: http://go.microsoft.com/fwlink/?LinkId=255555</span></span>

<span data-ttu-id="d0b9e-184">[Velikosti pro účty úložiště v systému Windows v Azure]: /azure/virtual-machines/virtual-machines-windows-sizes</span><span class="sxs-lookup"><span data-stu-id="d0b9e-184">[Sizes for Windows storage accounts in Azure]: /azure/virtual-machines/virtual-machines-windows-sizes</span></span>
<span data-ttu-id="d0b9e-185">[Velikosti pro Linux účty úložiště v Azure]: /azure/virtual-machines/virtual-machines-linux-sizes</span><span class="sxs-lookup"><span data-stu-id="d0b9e-185">[Sizes for Linux storage accounts in Azure]: /azure/virtual-machines/virtual-machines-linux-sizes</span></span>
<span data-ttu-id="d0b9e-186">[Ceny účtu úložiště systému Windows]: /pricing/details/virtual-machines/windows/</span><span class="sxs-lookup"><span data-stu-id="d0b9e-186">[Windows storage-account pricing]: /pricing/details/virtual-machines/windows/</span></span>
<span data-ttu-id="d0b9e-187">[Ceny Linux účet úložiště]: /pricing/details/virtual-machines/linux/</span><span class="sxs-lookup"><span data-stu-id="d0b9e-187">[Linux storage-account pricing]: /pricing/details/virtual-machines/linux/</span></span>

<!-- IMG List -->

[CS01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CS01.png
[CS02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CS02.png
[CC01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CC01.png
[CC02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CC02.png

[DS01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DS01.png
[DS02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DS02.png
[DC01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DC01.png
[DC02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DC02.png
