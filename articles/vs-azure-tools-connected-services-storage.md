---
title: "Přidat Azure Storage pomocí připojené služby v sadě Visual Studio | Microsoft Docs"
description: "Přidejte úložiště Azure do vaší aplikace pomocí dialogu Visual Studio přidat připojení služby"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 521ec044-ad4b-4828-8864-01decde2e758
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2017
ms.author: kraigb
ms.openlocfilehash: 35638083cd75e1b751d00a9c8163a3bc7480f0cd
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="adding-azure-storage-by-using-visual-studio-connected-services"></a><span data-ttu-id="926da-103">Přidání úložiště Azure pomocí Visual Studio připojené Services</span><span class="sxs-lookup"><span data-stu-id="926da-103">Adding Azure storage by using Visual Studio Connected Services</span></span>
<span data-ttu-id="926da-104">Pomocí sady Visual Studio, připojením některý z těchto do služby Azure Storage pomocí **přidat připojení služby** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="926da-104">With Visual Studio, you can connect any of the following to Azure Storage by using the **Add Connected Services** dialog:</span></span>

- <span data-ttu-id="926da-105">Cloudová služba jazyka C#</span><span class="sxs-lookup"><span data-stu-id="926da-105">C# cloud service</span></span>
- <span data-ttu-id="926da-106">Rozhraní .NET back-end mobilní služby</span><span class="sxs-lookup"><span data-stu-id="926da-106">.NET backend mobile service</span></span>
- <span data-ttu-id="926da-107">Web ASP.NET nebo služby</span><span class="sxs-lookup"><span data-stu-id="926da-107">ASP.NET website or service</span></span>
- <span data-ttu-id="926da-108">ASP.NET základní služby</span><span class="sxs-lookup"><span data-stu-id="926da-108">ASP.NET Core service</span></span>
- <span data-ttu-id="926da-109">Služba Azure webové úlohy</span><span class="sxs-lookup"><span data-stu-id="926da-109">Azure WebJob service</span></span> 

<span data-ttu-id="926da-110">Funkci připojené služby přidá všechny potřebné odkazy a kód připojení do projektu a odpovídajícím způsobem upravit konfigurační soubory.</span><span class="sxs-lookup"><span data-stu-id="926da-110">The connected service functionality adds all the needed references and connection code to your project, and modifies your configuration files appropriately.</span></span> 

<span data-ttu-id="926da-111">Po dokončení **přidat připojení služby** dialogové okno automaticky zobrazí dokumentaci s podrobnostmi o kroky potřebné k začít pracovat s úložištěm blob, fronty a tabulky.</span><span class="sxs-lookup"><span data-stu-id="926da-111">After completion, the **Add Connected Services** dialog automatically displays documentation detailing the steps required to start working with blob storage, queues, and tables.</span></span>

## <a name="connect-to-azure-storage-using-the-connected-services-dialog"></a><span data-ttu-id="926da-112">Připojit k úložišti Azure pomocí dialogového okna připojení služby</span><span class="sxs-lookup"><span data-stu-id="926da-112">Connect to Azure Storage using the Connected Services dialog</span></span>
1. <span data-ttu-id="926da-113">Otevřete projekt v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="926da-113">Open your project in Visual Studio</span></span>

1. <span data-ttu-id="926da-114">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **připojené služby** uzel a z kontextovou nabídku a vyberte **přidat připojení službě**.</span><span class="sxs-lookup"><span data-stu-id="926da-114">In **Solution Explorer**, right-click the **Connected Services** node, and, from the context menu, and select **Add Connected Service**.</span></span>
   
    ![Přidat Azure připojení služby](./media/vs-azure-tools-connected-services-storage/IC796702.png)

1. <span data-ttu-id="926da-116">V **připojené služby** vyberte **úložiště v cloudu s Azure Storage**.</span><span class="sxs-lookup"><span data-stu-id="926da-116">In the **Connected Services** page, select **Cloud Storage with Azure Storage**.</span></span>
   
    ![Přidejte úložiště Azure](./media/vs-azure-tools-connected-services-storage/add-azure-storage.png)

1. <span data-ttu-id="926da-118">V **Azure Storage** dialogovém okně vyberte stávající účet úložiště a vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="926da-118">In the **Azure Storage** dialog, select an existing storage account, and select **Add**.</span></span>
   
    <span data-ttu-id="926da-119">Pokud potřebujete vytvořit účet úložiště, přejděte k dalšímu kroku.</span><span class="sxs-lookup"><span data-stu-id="926da-119">If you need to create a storage account, go to the next step.</span></span> <span data-ttu-id="926da-120">Jinak přejděte ke kroku 6.</span><span class="sxs-lookup"><span data-stu-id="926da-120">Otherwise, skip to step 6.</span></span>
    
    ![Přidat existující účet úložiště do projektu](./media/vs-azure-tools-connected-services-storage/select-azure-storage-account.png)

1. <span data-ttu-id="926da-122">Chcete-li vytvořit účet úložiště:</span><span class="sxs-lookup"><span data-stu-id="926da-122">To create a storage account:</span></span> 
   
   1. <span data-ttu-id="926da-123">Vyberte **vytvořit nový účet úložiště** v dolní části dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="926da-123">Select **Create a New Storage Account** at the bottom of the dialog.</span></span>

   1. <span data-ttu-id="926da-124">Vyplňte **vytvořit účet úložiště** dialogové okno a vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="926da-124">Fill out the **Create Storage Account** dialog, and select **Create**.</span></span>
      
       ![Nový účet úložiště Azure](./media/vs-azure-tools-connected-services-storage/create-storage-account.png)
      
   1. <span data-ttu-id="926da-126">Když **Azure Storage** se zobrazí dialogové okno, v seznamu se zobrazí nový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="926da-126">When the **Azure Storage** dialog is displayed, the new storage account appears in the list.</span></span> <span data-ttu-id="926da-127">V seznamu vyberte nový účet úložiště a vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="926da-127">Select the new storage account in the list, and select **Add**.</span></span>

1. <span data-ttu-id="926da-128">Úložiště připojené služby se zobrazí pod **odkazy na službu** uzlu projektu.</span><span class="sxs-lookup"><span data-stu-id="926da-128">The storage connected service appears under the **Service References** node of your project.</span></span>
   
## <a name="how-your-project-is-modified"></a><span data-ttu-id="926da-129">Jak se mění projektu</span><span class="sxs-lookup"><span data-stu-id="926da-129">How your project is modified</span></span>
<span data-ttu-id="926da-130">Po dokončení dialogu přidá odkazy na Visual Studio a upraví určité konfigurační soubory.</span><span class="sxs-lookup"><span data-stu-id="926da-130">When you finish the dialog, Visual Studio adds references and modifies certain configuration files.</span></span> <span data-ttu-id="926da-131">Určité změny závisí na typu projektu:</span><span class="sxs-lookup"><span data-stu-id="926da-131">The specific changes depend on the project type:</span></span> 

- <span data-ttu-id="926da-132">Projekt ASP.NET - [co se stalo – projekty ASP.NET](http://go.microsoft.com/fwlink/p/?LinkId=513126)</span><span class="sxs-lookup"><span data-stu-id="926da-132">ASP.NET project - [What happened – ASP.NET Projects](http://go.microsoft.com/fwlink/p/?LinkId=513126)</span></span>
- <span data-ttu-id="926da-133">Projekt ASP.NET Core - [co se stalo – projekty ASP.NET 5](http://go.microsoft.com/fwlink/p/?LinkId=513124)</span><span class="sxs-lookup"><span data-stu-id="926da-133">ASP.NET Core project - [What happened – ASP.NET 5 Projects](http://go.microsoft.com/fwlink/p/?LinkId=513124)</span></span> 
- <span data-ttu-id="926da-134">Projekt cloudové služby (webových rolí a rolí pracovního procesu) - [co se stalo – projekty cloudových služeb](http://go.microsoft.com/fwlink/p/?LinkId=516965)</span><span class="sxs-lookup"><span data-stu-id="926da-134">Cloud service project (web roles and worker roles) - [What happened – Cloud Service projects](http://go.microsoft.com/fwlink/p/?LinkId=516965)</span></span>
- <span data-ttu-id="926da-135">Projekt webové úlohy – [co se stalo – projekty webové úlohy](visual-studio/vs-storage-webjobs-what-happened.md)</span><span class="sxs-lookup"><span data-stu-id="926da-135">WebJob project - [What happened - WebJob projects](visual-studio/vs-storage-webjobs-what-happened.md)</span></span>

## <a name="next-steps"></a><span data-ttu-id="926da-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="926da-136">Next steps</span></span>
- [<span data-ttu-id="926da-137">Fóru MSDN: Úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="926da-137">MSDN Forum: Azure Storage</span></span>](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata)
- [<span data-ttu-id="926da-138">Blog týmu Microsoft Azure Storage</span><span class="sxs-lookup"><span data-stu-id="926da-138">Microsoft Azure Storage Team Blog</span></span>](http://blogs.msdn.com/b/windowsazurestorage/)
- [<span data-ttu-id="926da-139">Dokumentace k Azure Storage</span><span class="sxs-lookup"><span data-stu-id="926da-139">Azure Storage documentation</span></span>](https://docs.microsoft.com/azure/storage/)
