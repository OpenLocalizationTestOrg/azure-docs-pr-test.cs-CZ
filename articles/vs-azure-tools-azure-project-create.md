---
title: "aaaCreating projektu Azure cloud service pomocí sady Visual Studio | Microsoft Docs"
description: "Další informace nyní toocreate projektu Azure cloud service pomocí sady Visual Studio"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: ec580df7-3dcc-45a9-a1d9-8c110678dfb5
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: 3c357016aa423688199a7ab3a670115e33a98fe9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="creating-an-azure-cloud-service-project-with-visual-studio"></a><span data-ttu-id="fdc02-103">Vytvoření projektu Azure cloud service pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fdc02-103">Creating an Azure cloud service project with Visual Studio</span></span>
<span data-ttu-id="fdc02-104">Hello nástroje Azure pro sadu Visual Studio poskytuje šablony projektu, který umožňuje vytvářet cloudové služby Azure.</span><span class="sxs-lookup"><span data-stu-id="fdc02-104">hello Azure Tools for Visual Studio provides a project template that lets you create an Azure cloud service.</span></span> <span data-ttu-id="fdc02-105">Po vytvoření hello projektu sady Visual Studio umožňuje tooconfigure, ladění a nasazení hello cloudové služby tooAzure.</span><span class="sxs-lookup"><span data-stu-id="fdc02-105">Once hello project has been created, Visual Studio enables you tooconfigure, debug, and deploy hello cloud service tooAzure.</span></span>

## <a name="steps-toocreate-an-azure-cloud-service-project-in-visual-studio"></a><span data-ttu-id="fdc02-106">Kroky toocreate projektu Azure cloud service v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fdc02-106">Steps toocreate an Azure cloud service project in Visual Studio</span></span>
<span data-ttu-id="fdc02-107">Tato část vás provede procesem vytvoření projektu Azure cloud service v sadě Visual Studio pomocí jedné nebo více webových rolí.</span><span class="sxs-lookup"><span data-stu-id="fdc02-107">This section walks you through creating an Azure cloud service project in Visual Studio with one or more web roles.</span></span>  

1. <span data-ttu-id="fdc02-108">Spuštění sady Visual Studio jako správce.</span><span class="sxs-lookup"><span data-stu-id="fdc02-108">Start Visual Studio as an administrator.</span></span>

1. <span data-ttu-id="fdc02-109">V hlavní nabídce hello vyberte **soubor** > **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="fdc02-109">On hello main menu, select **File** > **New** > **Project**.</span></span>

1. <span data-ttu-id="fdc02-110">Vyberte **cloudu** z hello Visual C# nebo Visual Basic projektu šablony uzly a vyberte **Azure Cloud Service** hello seznamu šablon.</span><span class="sxs-lookup"><span data-stu-id="fdc02-110">Select **Cloud** from hello Visual C# or Visual Basic project template nodes, and select **Azure Cloud Service** from hello list of templates.</span></span>

    ![Nové cloudové služby Azure](./media/vs-azure-tools-azure-project-create/new-project-wizard-for-cloud-service.png)

1. <span data-ttu-id="fdc02-112">Určete, která verze rozhraní .NET Framework, pokud chcete toouse toodevelop hello.</span><span class="sxs-lookup"><span data-stu-id="fdc02-112">Specify which version of hello .NET Framework you want toouse toodevelop your project.</span></span>

1. <span data-ttu-id="fdc02-113">Zadejte název a umístění projektu a název řešení hello.</span><span class="sxs-lookup"><span data-stu-id="fdc02-113">Enter a name and location for your project and a name for hello solution.</span></span> 

1. <span data-ttu-id="fdc02-114">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="fdc02-114">Select **OK**.</span></span>

1. <span data-ttu-id="fdc02-115">V hello **nová Cloudová služba Microsoft Azure** dialogové okno, vyberte hello role má tooadd a zvolte hello šipku vpravo tlačítko tooadd je tooyour řešení.</span><span class="sxs-lookup"><span data-stu-id="fdc02-115">In hello **New Microsoft Azure Cloud Service** dialog, select hello roles that you want tooadd, and choose hello right arrow button tooadd them tooyour solution.</span></span>

    ![Vyberte nové role Azure cloudové služby](./media/vs-azure-tools-azure-project-create/new-cloud-service.png)

1. <span data-ttu-id="fdc02-117">toorename role, kterou jste přidali, při přechodu na hello role v hello **nová Cloudová služba Microsoft Azure** dialogové okno a v místní nabídce hello, vyberte **přejmenovat**.</span><span class="sxs-lookup"><span data-stu-id="fdc02-117">toorename a role that you've added, hover on hello role in hello **New Microsoft Azure Cloud Service** dialog, and, from hello context menu, select **Rename**.</span></span> <span data-ttu-id="fdc02-118">Můžete také přejmenovat roli v rámci vašeho řešení (v hello **Průzkumníku řešení**) po byla přidána.</span><span class="sxs-lookup"><span data-stu-id="fdc02-118">You can also rename a role within your solution (in hello **Solution Explorer**) after it has been added.</span></span>

    ![Přejmenujte roli služby Azure cloud](./media/vs-azure-tools-azure-project-create/new-cloud-service-rename.png)

<span data-ttu-id="fdc02-120">projekt Visual Studio Azure Hello má přidružení toohello role projekty v řešení hello.</span><span class="sxs-lookup"><span data-stu-id="fdc02-120">hello Visual Studio Azure project has associations toohello role projects in hello solution.</span></span> <span data-ttu-id="fdc02-121">projekt Hello také zahrnuje hello *souboru definice služby* a *konfigurační soubor služby*:</span><span class="sxs-lookup"><span data-stu-id="fdc02-121">hello project also includes hello *service definition file* and *service configuration file*:</span></span>

- <span data-ttu-id="fdc02-122">**Soubor definice služby** -definuje hello nastavení běhového prostředí pro aplikaci, včetně toho, jaké role jsou požadovány, koncových bodů a velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fdc02-122">**Service definition file** - Defines hello runtime settings for your application, including what roles are required, endpoints, and virtual machine size.</span></span> 
- <span data-ttu-id="fdc02-123">**Konfigurační soubor služby** -nakonfiguruje jak velký počet instancí role jsou spouštěny a hello hodnoty hello nastavení definované pro roli.</span><span class="sxs-lookup"><span data-stu-id="fdc02-123">**Service configuration file** - Configures how many instances of a role are run and hello values of hello settings defined for a role.</span></span> 

<span data-ttu-id="fdc02-124">Další informace o těchto souborech najdete v tématu [konfigurovat hello role pro cloudové služby Azure pomocí sady Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).</span><span class="sxs-lookup"><span data-stu-id="fdc02-124">For more information about these files, see [Configure hello Roles for an Azure cloud service with Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="fdc02-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fdc02-125">Next steps</span></span>
- [<span data-ttu-id="fdc02-126">Správa rolí v projektech Azure cloud service pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fdc02-126">Managing roles in Azure cloud service projects with Visual Studio</span></span>](./vs-azure-tools-cloud-service-project-managing-roles.md)
