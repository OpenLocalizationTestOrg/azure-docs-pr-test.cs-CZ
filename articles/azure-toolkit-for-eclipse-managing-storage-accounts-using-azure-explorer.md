---
title: "účty úložiště aaaManage pomocí hello Průzkumník Azure pro Eclipse | Microsoft Docs"
description: "Zjistěte, jak toomanage úložiště Azure účtů pomocí hello Průzkumník Azure pro Eclipse."
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
ms.openlocfilehash: b7ec53e77e3c96d87754b9a658d31e6182121b22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-storage-accounts-by-using-hello-azure-explorer-for-eclipse"></a><span data-ttu-id="74483-103">Správa účtů úložiště pomocí hello Průzkumník Azure pro Eclipse</span><span class="sxs-lookup"><span data-stu-id="74483-103">Manage storage accounts by using hello Azure Explorer for Eclipse</span></span>

<span data-ttu-id="74483-104">Hello Průzkumník Azure, který je součástí hello nástrojů Azure pro prostředí Eclipse, poskytuje vývojáře v jazyce Java s řešením pro snadné použití ke správě účtů úložiště v účtu Azure z uvnitř hello Eclipse integrované vývojové prostředí (IDE).</span><span class="sxs-lookup"><span data-stu-id="74483-104">hello Azure Explorer, which is part of hello Azure Toolkit for Eclipse, provides Java developers with an easy-to-use solution for managing storage accounts in their Azure account from inside hello Eclipse integrated development environment (IDE).</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-eclipse-show-azure-explorer](../includes/azure-toolkit-for-eclipse-show-azure-explorer.md)]

## <a name="create-a-storage-account-in-eclipse"></a><span data-ttu-id="74483-105">Vytvořit účet úložiště v prostředí Eclipse</span><span class="sxs-lookup"><span data-stu-id="74483-105">Create a storage account in Eclipse</span></span>

<span data-ttu-id="74483-106">toocreate účet úložiště pomocí hello Azure Explorer hello následující:</span><span class="sxs-lookup"><span data-stu-id="74483-106">toocreate a storage account by using hello Azure Explorer, do hello following:</span></span>

1. <span data-ttu-id="74483-107">Přihlaste se pomocí hello tooyour účet Azure [přihlášení pokyny pro hello nástrojů Azure pro Eclipse].</span><span class="sxs-lookup"><span data-stu-id="74483-107">Sign in tooyour Azure account by using hello [Sign-in instructions for hello Azure Toolkit for Eclipse].</span></span>

2. <span data-ttu-id="74483-108">V hello **Azure Explorer** , rozbalte hello **Azure** uzel, klikněte pravým tlačítkem na **účty úložiště**a pak klikněte na tlačítko **vytvořit účet úložiště**.</span><span class="sxs-lookup"><span data-stu-id="74483-108">In hello **Azure Explorer** view, expand hello **Azure** node, right-click **Storage Accounts**, and then click **Create Storage Account**.</span></span>

   ![Vytvořit účet úložiště příkaz][CS01]

3. <span data-ttu-id="74483-110">V hello **vytvořit účet úložiště** dialogovém okně zadejte hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="74483-110">In hello **Create Storage Account** dialog box, specify hello following options:</span></span>

   ![Vytvořit nový účet úložiště, dialogové okno][CS02]

   * <span data-ttu-id="74483-112">**Název**: Určuje název hello hello nový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="74483-112">**Name**: Specifies hello name for hello new storage account.</span></span>

   * <span data-ttu-id="74483-113">**Předplatné**: Určuje hello předplatné Azure, který má toouse pro nový účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="74483-113">**Subscription**: Specifies hello Azure subscription that you want toouse for hello new storage account.</span></span>

   * <span data-ttu-id="74483-114">**Skupina prostředků**: Určuje hello skupinu prostředků pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="74483-114">**Resource Group**: Specifies hello resource group for your virtual machine.</span></span> <span data-ttu-id="74483-115">Vyberte jednu z hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="74483-115">Select one of hello following options:</span></span>
      * <span data-ttu-id="74483-116">**Vytvořit nový**: Určuje, že chcete toocreate novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="74483-116">**Create New**: Specifies that you want toocreate a new resource group.</span></span>
      * <span data-ttu-id="74483-117">**Použít existující**: Určuje, že vyberete ze seznamu skupin prostředků, které jsou spojeny s vaším účtem Azure.</span><span class="sxs-lookup"><span data-stu-id="74483-117">**Use Existing**: Specifies that you will select from a list of resource groups that are associated with your Azure account.</span></span>

   * <span data-ttu-id="74483-118">**Oblast**: Určuje hello umístění, které se vytvoří účet úložiště (například "Západní USA").</span><span class="sxs-lookup"><span data-stu-id="74483-118">**Region**: Specifies hello location where your storage account will be created (for example, "West US").</span></span>

   * <span data-ttu-id="74483-119">**Účet druh**: Určuje typ hello toocreate účtu úložiště (například "úložiště objektů Blob").</span><span class="sxs-lookup"><span data-stu-id="74483-119">**Account kind**: Specifies hello type of storage account toocreate (for example, "Blob storage").</span></span> <span data-ttu-id="74483-120">Další informace najdete v tématu [účty Azure storage].</span><span class="sxs-lookup"><span data-stu-id="74483-120">For more information, see [About Azure storage accounts].</span></span>

   * <span data-ttu-id="74483-121">**Výkon**: Určuje, který účet úložiště nabídky toouse od vydavatele vybrané hello (například "Premium").</span><span class="sxs-lookup"><span data-stu-id="74483-121">**Performance**: Specifies which storage account offering toouse from hello selected publisher (for example, "Premium").</span></span> <span data-ttu-id="74483-122">Další informace najdete v tématu [úložiště Azure škálovatelnosti a cílech výkonnosti].</span><span class="sxs-lookup"><span data-stu-id="74483-122">For more information, see [Azure storage scalability and performance targets].</span></span>

   * <span data-ttu-id="74483-123">**Replikace**: Určuje hello replikace pro účet úložiště hello (například "Zónově redundantní").</span><span class="sxs-lookup"><span data-stu-id="74483-123">**Replication**: Specifies hello replication for hello storage account (for example, "Zone-Redundant").</span></span> <span data-ttu-id="74483-124">Další informace najdete v tématu [replikace Azure storage].</span><span class="sxs-lookup"><span data-stu-id="74483-124">For more information, see [Azure storage replication].</span></span>

4. <span data-ttu-id="74483-125">Pokud jste zadali všechny hello předcházející možnosti, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="74483-125">When you have specified all of hello preceding options, click **Create**.</span></span>

## <a name="create-a-storage-container-in-eclipse"></a><span data-ttu-id="74483-126">Vytvoření kontejneru úložiště v prostředí Eclipse</span><span class="sxs-lookup"><span data-stu-id="74483-126">Create a storage container in Eclipse</span></span>

<span data-ttu-id="74483-127">toocreate kontejner úložiště pomocí hello Azure Explorer hello následující:</span><span class="sxs-lookup"><span data-stu-id="74483-127">toocreate a storage container by using hello Azure Explorer, do hello following:</span></span>

1. <span data-ttu-id="74483-128">V hello **Azure Explorer** zobrazit, klikněte pravým tlačítkem na účet úložiště hello, kam chcete toocreate kontejner a pak klikněte na tlačítko **vytvořit kontejner objektů blob**.</span><span class="sxs-lookup"><span data-stu-id="74483-128">In hello **Azure Explorer** view, right-click hello storage account where you want toocreate a container, and then click **Create blob container**.</span></span>

   ![Vytvoření příkazu kontejner objektů blob][CC01]

2. <span data-ttu-id="74483-130">V hello **vytvořit kontejner objektů blob** dialogové okno, zadejte název hello pro váš kontejner a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="74483-130">In hello **Create blob container** dialog box, specify hello name for your container, and then click **OK**.</span></span> <span data-ttu-id="74483-131">Další informace o pojmenovávání kontejnerů úložiště najdete v tématu [pojmenování a odkazování na kontejnerů, objektů BLOB a metadat].</span><span class="sxs-lookup"><span data-stu-id="74483-131">For more information about naming storage containers, see [Naming and referencing containers, blobs, and metadata].</span></span>

   ![Vytvoření dialogového okna kontejner objektů blob][CC02]

## <a name="delete-a-storage-container-in-eclipse"></a><span data-ttu-id="74483-133">Odstranit kontejner úložiště v prostředí Eclipse</span><span class="sxs-lookup"><span data-stu-id="74483-133">Delete a storage container in Eclipse</span></span>

<span data-ttu-id="74483-134">toodelete kontejner úložiště pomocí hello Azure Explorer hello následující:</span><span class="sxs-lookup"><span data-stu-id="74483-134">toodelete a storage container by using hello Azure Explorer, do hello following:</span></span>

1. <span data-ttu-id="74483-135">V hello **Azure Explorer** zobrazení, klikněte pravým tlačítkem na kontejner úložiště hello a pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="74483-135">In hello **Azure Explorer** view, right-click hello storage container, and then click **Delete**.</span></span>

   ![Odstranit příkaz kontejneru úložiště][DC01]

2. <span data-ttu-id="74483-137">V okně potvrzení hello klikněte **OK**.</span><span class="sxs-lookup"><span data-stu-id="74483-137">In hello confirmation window, click **OK**.</span></span>

   ![Odstranit okno pro potvrzení kontejneru úložiště][DC02]

## <a name="delete-a-storage-account-in-eclipse"></a><span data-ttu-id="74483-139">Odstranit účet úložiště v prostředí Eclipse</span><span class="sxs-lookup"><span data-stu-id="74483-139">Delete a storage account in Eclipse</span></span>

<span data-ttu-id="74483-140">toodelete účet úložiště pomocí hello Azure Explorer hello následující:</span><span class="sxs-lookup"><span data-stu-id="74483-140">toodelete a storage account by using hello Azure Explorer, do hello following:</span></span>

1. <span data-ttu-id="74483-141">V hello **Azure Explorer** zobrazení, klikněte pravým tlačítkem na účet úložiště hello a pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="74483-141">In hello **Azure Explorer** view, right-click hello storage account, and then click **Delete**.</span></span>

   ![Příkaz účet úložiště odstranit][DS01]

2. <span data-ttu-id="74483-143">V okně potvrzení hello klikněte **OK**.</span><span class="sxs-lookup"><span data-stu-id="74483-143">In hello confirmation window, click **OK**.</span></span>

   ![Odstranit okno pro potvrzení účtu úložiště][DS02]

## <a name="next-steps"></a><span data-ttu-id="74483-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="74483-145">Next steps</span></span>
<span data-ttu-id="74483-146">Další informace o účtech Azure storage, velikosti a cenách najdete v části hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="74483-146">For more information about Azure storage accounts, sizes, and pricing, see hello following resources:</span></span>

* <span data-ttu-id="74483-147">[Úvod tooMicrosoft Azure Storage]</span><span class="sxs-lookup"><span data-stu-id="74483-147">[Introduction tooMicrosoft Azure Storage]</span></span>
* <span data-ttu-id="74483-148">[účty Azure storage]</span><span class="sxs-lookup"><span data-stu-id="74483-148">[About Azure storage accounts]</span></span>
* <span data-ttu-id="74483-149">Účet služby Azure storage velikosti</span><span class="sxs-lookup"><span data-stu-id="74483-149">Azure storage-account sizes</span></span>
  * <span data-ttu-id="74483-150">[Velikosti pro účty úložiště v systému Windows v Azure]</span><span class="sxs-lookup"><span data-stu-id="74483-150">[Sizes for Windows storage accounts in Azure]</span></span>
  * <span data-ttu-id="74483-151">[Velikosti pro Linux účty úložiště v Azure]</span><span class="sxs-lookup"><span data-stu-id="74483-151">[Sizes for Linux storage accounts in Azure]</span></span>
* <span data-ttu-id="74483-152">Účet služby Azure storage – ceny</span><span class="sxs-lookup"><span data-stu-id="74483-152">Azure storage-account pricing</span></span>
  * <span data-ttu-id="74483-153">[Ceny účtu úložiště systému Windows]</span><span class="sxs-lookup"><span data-stu-id="74483-153">[Windows storage-account pricing]</span></span>
  * <span data-ttu-id="74483-154">[Ceny Linux účet úložiště]</span><span class="sxs-lookup"><span data-stu-id="74483-154">[Linux storage-account pricing]</span></span>

<span data-ttu-id="74483-155">Další informace o sadách Azure pro integrovaného vývojového prostředí Java najdete v tématu hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="74483-155">For more information about Azure Toolkits for Java IDEs, see hello following resources:</span></span>

* <span data-ttu-id="74483-156">[Azure nástrojů pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="74483-156">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="74483-157">[Co je nového v hello nástrojů Azure pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="74483-157">[What's new in hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="74483-158">[Instalace hello nástrojů Azure pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="74483-158">[Installing hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="74483-159">[přihlášení pokyny pro hello nástrojů Azure pro Eclipse]</span><span class="sxs-lookup"><span data-stu-id="74483-159">[Sign-in instructions for hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="74483-160">[Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse]</span><span class="sxs-lookup"><span data-stu-id="74483-160">[Create a Hello World web app for Azure in Eclipse]</span></span>
* <span data-ttu-id="74483-161">[Sada Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="74483-161">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="74483-162">[Co je nového v hello nástrojů Azure pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="74483-162">[What's new in hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="74483-163">[Instalace hello Azure Toolkit pro IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="74483-163">[Installing hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="74483-164">[Přihlášení pokyny pro hello Azure Toolkit IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="74483-164">[Sign-in instructions for hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="74483-165">[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="74483-165">[Create a Hello World web app for Azure in IntelliJ]</span></span>

<span data-ttu-id="74483-166">Další informace o používání Azure s Java najdete v tématu [Azure střediska pro vývojáře Java] a [Java nástrojů pro Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="74483-166">For more information about using Azure with Java, see [Azure Java Developer Center] and [Java Tools for Visual Studio Team Services].</span></span>

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

[CS01]: ./media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/CS01.png
[CS02]: ./media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/CS02.png
[CC01]: ./media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/CC01.png
[CC02]: ./media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/CC02.png

[DS01]: ./media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/DS01.png
[DS02]: ./media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/DS02.png
[DC01]: ./media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/DC01.png
[DC02]: ./media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/DC02.png
