---
title: "Vytvoření projektu Azure cloud service pomocí sady Visual Studio | Microsoft Docs"
description: "Naučte se vytvářet projekt Azure cloud service pomocí sady Visual Studio"
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
ms.openlocfilehash: 1f6ded87b551f660853ea4eb0548f3d942e28fa8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="creating-an-azure-cloud-service-project-with-visual-studio"></a><span data-ttu-id="07139-103">Vytvoření projektu Azure cloud service pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="07139-103">Creating an Azure cloud service project with Visual Studio</span></span>
<span data-ttu-id="07139-104">Nástroje Azure pro sadu Visual Studio poskytuje šablony projektu, který umožňuje vytvářet cloudové služby Azure.</span><span class="sxs-lookup"><span data-stu-id="07139-104">The Azure Tools for Visual Studio provides a project template that lets you create an Azure cloud service.</span></span> <span data-ttu-id="07139-105">Po vytvoření projektu sady Visual Studio umožňuje nakonfigurovat, ladění a nasazení cloudové služby pro Azure.</span><span class="sxs-lookup"><span data-stu-id="07139-105">Once the project has been created, Visual Studio enables you to configure, debug, and deploy the cloud service to Azure.</span></span>

## <a name="steps-to-create-an-azure-cloud-service-project-in-visual-studio"></a><span data-ttu-id="07139-106">Postup vytvoření projektu Azure cloud service v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="07139-106">Steps to create an Azure cloud service project in Visual Studio</span></span>
<span data-ttu-id="07139-107">Tato část vás provede procesem vytvoření projektu Azure cloud service v sadě Visual Studio pomocí jedné nebo více webových rolí.</span><span class="sxs-lookup"><span data-stu-id="07139-107">This section walks you through creating an Azure cloud service project in Visual Studio with one or more web roles.</span></span>  

1. <span data-ttu-id="07139-108">Spuštění sady Visual Studio jako správce.</span><span class="sxs-lookup"><span data-stu-id="07139-108">Start Visual Studio as an administrator.</span></span>

1. <span data-ttu-id="07139-109">V hlavní nabídce vyberte **soubor** > **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="07139-109">On the main menu, select **File** > **New** > **Project**.</span></span>

1. <span data-ttu-id="07139-110">Vyberte **cloudu** z Visual C# nebo Visual Basic projektu šablony uzly a vyberte **Azure Cloud Service** ze seznamu šablon.</span><span class="sxs-lookup"><span data-stu-id="07139-110">Select **Cloud** from the Visual C# or Visual Basic project template nodes, and select **Azure Cloud Service** from the list of templates.</span></span>

    ![Nové cloudové služby Azure](./media/vs-azure-tools-azure-project-create/new-project-wizard-for-cloud-service.png)

1. <span data-ttu-id="07139-112">Určete, která verze rozhraní .NET Framework, kterou chcete použít k vývoji projektu.</span><span class="sxs-lookup"><span data-stu-id="07139-112">Specify which version of the .NET Framework you want to use to develop your project.</span></span>

1. <span data-ttu-id="07139-113">Zadejte název a umístění projektu a název řešení.</span><span class="sxs-lookup"><span data-stu-id="07139-113">Enter a name and location for your project and a name for the solution.</span></span> 

1. <span data-ttu-id="07139-114">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="07139-114">Select **OK**.</span></span>

1. <span data-ttu-id="07139-115">V **nová Cloudová služba Microsoft Azure** dialogovém okně, vyberte role, které chcete přidat a zvolte tlačítko se šipkou doprava je přidáte do vašeho řešení.</span><span class="sxs-lookup"><span data-stu-id="07139-115">In the **New Microsoft Azure Cloud Service** dialog, select the roles that you want to add, and choose the right arrow button to add them to your solution.</span></span>

    ![Vyberte nové role Azure cloudové služby](./media/vs-azure-tools-azure-project-create/new-cloud-service.png)

1. <span data-ttu-id="07139-117">Pokud chcete přejmenovat role, kterou jste přidali, najeďte na roli v **nová Cloudová služba Microsoft Azure** dialogové okno a v místní nabídce vyberte **přejmenovat**.</span><span class="sxs-lookup"><span data-stu-id="07139-117">To rename a role that you've added, hover on the role in the **New Microsoft Azure Cloud Service** dialog, and, from the context menu, select **Rename**.</span></span> <span data-ttu-id="07139-118">Můžete také přejmenovat roli v rámci vašeho řešení (v **Průzkumníku řešení**) po byla přidána.</span><span class="sxs-lookup"><span data-stu-id="07139-118">You can also rename a role within your solution (in the **Solution Explorer**) after it has been added.</span></span>

    ![Přejmenujte roli služby Azure cloud](./media/vs-azure-tools-azure-project-create/new-cloud-service-rename.png)

<span data-ttu-id="07139-120">Projekt Visual Studio Azure má přidružení k roli projekty v řešení.</span><span class="sxs-lookup"><span data-stu-id="07139-120">The Visual Studio Azure project has associations to the role projects in the solution.</span></span> <span data-ttu-id="07139-121">Tento projekt taky zahrnuje *souboru definice služby* a *konfigurační soubor služby*:</span><span class="sxs-lookup"><span data-stu-id="07139-121">The project also includes the *service definition file* and *service configuration file*:</span></span>

- <span data-ttu-id="07139-122">**Soubor definice služby** -definuje nastavení běhového prostředí pro aplikaci, včetně toho, jaké role jsou požadovány, koncových bodů a velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="07139-122">**Service definition file** - Defines the runtime settings for your application, including what roles are required, endpoints, and virtual machine size.</span></span> 
- <span data-ttu-id="07139-123">**Konfigurační soubor služby** – konfiguruje, jak velký počet instancí role jsou spustit a hodnoty k nastavení určenému pro roli.</span><span class="sxs-lookup"><span data-stu-id="07139-123">**Service configuration file** - Configures how many instances of a role are run and the values of the settings defined for a role.</span></span> 

<span data-ttu-id="07139-124">Další informace o těchto souborech najdete v tématu [konfiguraci rolí pro cloudové služby Azure pomocí sady Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).</span><span class="sxs-lookup"><span data-stu-id="07139-124">For more information about these files, see [Configure the Roles for an Azure cloud service with Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="07139-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="07139-125">Next steps</span></span>
- [<span data-ttu-id="07139-126">Správa rolí v projektech Azure cloud service pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="07139-126">Managing roles in Azure cloud service projects with Visual Studio</span></span>](./vs-azure-tools-cloud-service-project-managing-roles.md)
