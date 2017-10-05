---
title: "Konfigurace projektu Azure cloud service pomocí sady Visual Studio | Microsoft Docs"
description: "Informace o konfiguraci projektu Azure cloud service v sadě Visual Studio, v závislosti na požadavcích pro tento projekt."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 609d6965-05cc-47b1-82dc-c76a92d4f295
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/06/2017
ms.author: kraigb
ms.openlocfilehash: 799715093bd1a8c8e77e6cdb7168670dc263dfa5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="configure-an-azure-cloud-service-project-with-visual-studio"></a><span data-ttu-id="8967e-103">Konfigurace projektu Azure cloud service pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8967e-103">Configure an Azure cloud service project with Visual Studio</span></span>
<span data-ttu-id="8967e-104">Projekt Azure cloud service můžete nakonfigurovat v závislosti na požadavcích pro tento projekt.</span><span class="sxs-lookup"><span data-stu-id="8967e-104">You can configure an Azure cloud service project, depending on your requirements for that project.</span></span> <span data-ttu-id="8967e-105">Můžete nastavit vlastnosti projektu v těchto kategoriích:</span><span class="sxs-lookup"><span data-stu-id="8967e-105">You can set properties for the project for the following categories:</span></span>

- <span data-ttu-id="8967e-106">**Publikování Cloudová služba Azure** – můžete nastavit vlastnost a ujistěte se, že není omylem odstranit stávající cloudovou službu nasadit do Azure.</span><span class="sxs-lookup"><span data-stu-id="8967e-106">**Publish a cloud service to Azure** - You can set a property to make sure that an existing cloud service deployed to Azure is not accidentally deleted.</span></span>
- <span data-ttu-id="8967e-107">**Spustit nebo ladění cloudové služby na místním počítači** – můžete vybrat konfiguraci služby, kterou chcete použít a uvést, zda chcete spustit emulátoru úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="8967e-107">**Run or debug a cloud service on the local computer** - You can select a service configuration to use and indicate whether you want to start the Azure storage emulator.</span></span>
- <span data-ttu-id="8967e-108">**Ověřit balíček cloudové služby, když je vytvořeno** -rozhodnete zacházet s všechna upozornění jako chyby, tak, aby bylo možné zajistit, že balíček služby cloud nasadí bez problémů.</span><span class="sxs-lookup"><span data-stu-id="8967e-108">**Validate a cloud service package when it is created** - You can decide to treat any warnings as errors so that you can ensure that the cloud service package deploys without any issues.</span></span> 

## <a name="steps-to-configure-an-azure-cloud-service-project"></a><span data-ttu-id="8967e-109">Postup konfigurace projektu Azure cloud service</span><span class="sxs-lookup"><span data-stu-id="8967e-109">Steps to configure an Azure cloud service project</span></span>
1. <span data-ttu-id="8967e-110">Otevřít nebo vytvořit projekt cloudové služby v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8967e-110">Open or create a cloud service project in Visual Studio</span></span>

1. <span data-ttu-id="8967e-111">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a v místní nabídce vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="8967e-111">In **Solution Explorer**, right-click the project, and, from the context menu, select **Properties**.</span></span>
   
1. <span data-ttu-id="8967e-112">Na stránce vlastností projektu, vyberte **vývoj** kartě.</span><span class="sxs-lookup"><span data-stu-id="8967e-112">In the project's properties page, select the **Development** tab.</span></span>

    ![Nabídka Vlastnosti projektu](./media/vs-azure-tools-configuring-an-azure-project/solution-explorer-project-properties-menu.png)

1. <span data-ttu-id="8967e-114">Nastavit **výzva před odstraněním stávajícího nasazení** k **True**.</span><span class="sxs-lookup"><span data-stu-id="8967e-114">Set **Prompt before deleting an existing deployment** to **True**.</span></span> <span data-ttu-id="8967e-115">Toto nastavení zásady pomáhají zajistit, že nemáte omylem odstranit stávajícího nasazení v Azure</span><span class="sxs-lookup"><span data-stu-id="8967e-115">This setting helps to ensure you don't accidentally delete an existing deployment in Azure</span></span>

1. <span data-ttu-id="8967e-116">Vyberte požadovanou **konfigurace služby** udávajících konfiguraci služby, kterou chcete použít při spuštění nebo ladění cloudové služby místně.</span><span class="sxs-lookup"><span data-stu-id="8967e-116">Select the desired **Service configuration** to indicate which service configuration you want to use when you run or debug your cloud service locally.</span></span> <span data-ttu-id="8967e-117">Další informace o tom, jak změnit konfiguraci služby pro role, naleznete v části [postup konfigurace role pro cloudové služby Azure pomocí sady Visual Studio](./vs-azure-tools-configure-roles-for-cloud-service.md).</span><span class="sxs-lookup"><span data-stu-id="8967e-117">For more information on how to modify a service configuration for a role, see [How to configure the roles for an Azure cloud service with Visual Studio](./vs-azure-tools-configure-roles-for-cloud-service.md).</span></span>

1. <span data-ttu-id="8967e-118">Nastavit **emulátoru úložiště Azure spustit** k **True** při spouštění nebo ladění cloudové služby místně spusťte emulátor úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="8967e-118">Set **Start Azure storage emulator** to **True** to start the Azure storage emulator when you run or debug your cloud service locally.</span></span>

1. <span data-ttu-id="8967e-119">Nastavit **považovat upozornění jako chyby** k **True** a ujistěte se, pokud nejsou chyby ověření balíček nelze publikovat.</span><span class="sxs-lookup"><span data-stu-id="8967e-119">Set **Treat warnings as errors** to **True** to make sure you cannot publish if there are package validation errors.</span></span>

1. <span data-ttu-id="8967e-120">Nastavit **použít webový projekt porty** k **True** a ujistěte se, že vaši webovou roli používá stejný port pokaždé, když ji spustí místně v IIS Express.</span><span class="sxs-lookup"><span data-stu-id="8967e-120">Set **Use web project ports** to **True** to make sure that your web role uses the same port each time it starts locally in IIS Express.</span></span>

1. <span data-ttu-id="8967e-121">Na panelu nástrojů Visual Studio vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="8967e-121">From the Visual Studio toolbar, select **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8967e-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8967e-122">Next steps</span></span>
- [<span data-ttu-id="8967e-123">Konfigurace projektu Azure pomocí více konfigurace služby</span><span class="sxs-lookup"><span data-stu-id="8967e-123">Configure an Azure project using multiple service configurations</span></span>](vs-azure-tools-multiple-services-project-configurations.md)

