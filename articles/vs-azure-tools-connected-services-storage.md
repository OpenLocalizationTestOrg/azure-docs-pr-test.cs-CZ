---
title: "aaaAdd Azure Storage pomocí připojené služby v sadě Visual Studio | Microsoft Docs"
description: "Přidat aplikaci Azure Storage tooyour hello Visual Studio přidat připojení služby dialogovém okně"
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
ms.openlocfilehash: 56b42063d86510b330e405108e28d50e6ba4da05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="adding-azure-storage-by-using-visual-studio-connected-services"></a><span data-ttu-id="bcb88-103">Přidání úložiště Azure pomocí Visual Studio připojené Services</span><span class="sxs-lookup"><span data-stu-id="bcb88-103">Adding Azure storage by using Visual Studio Connected Services</span></span>
<span data-ttu-id="bcb88-104">Pomocí sady Visual Studio, můžete připojit žádné z následujících tooAzure úložiště pomocí hello hello **přidat připojení služby** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="bcb88-104">With Visual Studio, you can connect any of hello following tooAzure Storage by using hello **Add Connected Services** dialog:</span></span>

- <span data-ttu-id="bcb88-105">Cloudová služba jazyka C#</span><span class="sxs-lookup"><span data-stu-id="bcb88-105">C# cloud service</span></span>
- <span data-ttu-id="bcb88-106">Rozhraní .NET back-end mobilní služby</span><span class="sxs-lookup"><span data-stu-id="bcb88-106">.NET backend mobile service</span></span>
- <span data-ttu-id="bcb88-107">Web ASP.NET nebo služby</span><span class="sxs-lookup"><span data-stu-id="bcb88-107">ASP.NET website or service</span></span>
- <span data-ttu-id="bcb88-108">ASP.NET základní služby</span><span class="sxs-lookup"><span data-stu-id="bcb88-108">ASP.NET Core service</span></span>
- <span data-ttu-id="bcb88-109">Služba Azure webové úlohy</span><span class="sxs-lookup"><span data-stu-id="bcb88-109">Azure WebJob service</span></span> 

<span data-ttu-id="bcb88-110">Hello funkci připojené služby přidá všechny odkazy hello potřeby a připojení kód tooyour projektu a odpovídajícím způsobem upravit konfigurační soubory.</span><span class="sxs-lookup"><span data-stu-id="bcb88-110">hello connected service functionality adds all hello needed references and connection code tooyour project, and modifies your configuration files appropriately.</span></span> 

<span data-ttu-id="bcb88-111">Po dokončení hello **přidat připojení služby** dokumentace s podrobnostmi o hello kroky požadované toostart práce úložiště objektů blob, fronty a tabulky se automaticky zobrazí dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bcb88-111">After completion, hello **Add Connected Services** dialog automatically displays documentation detailing hello steps required toostart working with blob storage, queues, and tables.</span></span>

## <a name="connect-tooazure-storage-using-hello-connected-services-dialog"></a><span data-ttu-id="bcb88-112">Připojit tooAzure úložiště pomocí služby připojené hello dialogové okno</span><span class="sxs-lookup"><span data-stu-id="bcb88-112">Connect tooAzure Storage using hello Connected Services dialog</span></span>
1. <span data-ttu-id="bcb88-113">Otevřete projekt v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bcb88-113">Open your project in Visual Studio</span></span>

1. <span data-ttu-id="bcb88-114">V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **připojené služby** uzlu a z hello kontextovou nabídku a vyberte **přidat připojení službě**.</span><span class="sxs-lookup"><span data-stu-id="bcb88-114">In **Solution Explorer**, right-click hello **Connected Services** node, and, from hello context menu, and select **Add Connected Service**.</span></span>
   
    ![Přidat Azure připojení služby](./media/vs-azure-tools-connected-services-storage/IC796702.png)

1. <span data-ttu-id="bcb88-116">V hello **připojené služby** vyberte **úložiště v cloudu s Azure Storage**.</span><span class="sxs-lookup"><span data-stu-id="bcb88-116">In hello **Connected Services** page, select **Cloud Storage with Azure Storage**.</span></span>
   
    ![Přidejte úložiště Azure](./media/vs-azure-tools-connected-services-storage/add-azure-storage.png)

1. <span data-ttu-id="bcb88-118">V hello **Azure Storage** dialogovém okně vyberte stávající účet úložiště a vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="bcb88-118">In hello **Azure Storage** dialog, select an existing storage account, and select **Add**.</span></span>
   
    <span data-ttu-id="bcb88-119">Pokud potřebujete toocreate účet úložiště, přejděte toohello další krok.</span><span class="sxs-lookup"><span data-stu-id="bcb88-119">If you need toocreate a storage account, go toohello next step.</span></span> <span data-ttu-id="bcb88-120">Jinak přejděte toostep 6.</span><span class="sxs-lookup"><span data-stu-id="bcb88-120">Otherwise, skip toostep 6.</span></span>
    
    ![Přidat existující tooproject účtu úložiště](./media/vs-azure-tools-connected-services-storage/select-azure-storage-account.png)

1. <span data-ttu-id="bcb88-122">toocreate účet úložiště:</span><span class="sxs-lookup"><span data-stu-id="bcb88-122">toocreate a storage account:</span></span> 
   
   1. <span data-ttu-id="bcb88-123">Vyberte **vytvořit nový účet úložiště** dolnímu hello hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="bcb88-123">Select **Create a New Storage Account** at hello bottom of hello dialog.</span></span>

   1. <span data-ttu-id="bcb88-124">Vyplňte hello **vytvořit účet úložiště** dialogové okno a vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="bcb88-124">Fill out hello **Create Storage Account** dialog, and select **Create**.</span></span>
      
       ![Nový účet úložiště Azure](./media/vs-azure-tools-connected-services-storage/create-storage-account.png)
      
   1. <span data-ttu-id="bcb88-126">Když hello **Azure Storage** zobrazí dialog, hello nový účet úložiště se zobrazí v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="bcb88-126">When hello **Azure Storage** dialog is displayed, hello new storage account appears in hello list.</span></span> <span data-ttu-id="bcb88-127">Vyberte v seznamu hello hello nový účet úložiště a vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="bcb88-127">Select hello new storage account in hello list, and select **Add**.</span></span>

1. <span data-ttu-id="bcb88-128">Hello úložiště připojené služby se zobrazí pod hello **odkazy na službu** uzlu projektu.</span><span class="sxs-lookup"><span data-stu-id="bcb88-128">hello storage connected service appears under hello **Service References** node of your project.</span></span>
   
## <a name="how-your-project-is-modified"></a><span data-ttu-id="bcb88-129">Jak se mění projektu</span><span class="sxs-lookup"><span data-stu-id="bcb88-129">How your project is modified</span></span>
<span data-ttu-id="bcb88-130">Po dokončení hello dialogové okno přidá odkazy na Visual Studio a upraví určité konfigurační soubory.</span><span class="sxs-lookup"><span data-stu-id="bcb88-130">When you finish hello dialog, Visual Studio adds references and modifies certain configuration files.</span></span> <span data-ttu-id="bcb88-131">určité změny Hello závisí na typu projektu hello:</span><span class="sxs-lookup"><span data-stu-id="bcb88-131">hello specific changes depend on hello project type:</span></span> 

- <span data-ttu-id="bcb88-132">Projekt ASP.NET - [co se stalo – projekty ASP.NET](http://go.microsoft.com/fwlink/p/?LinkId=513126)</span><span class="sxs-lookup"><span data-stu-id="bcb88-132">ASP.NET project - [What happened – ASP.NET Projects](http://go.microsoft.com/fwlink/p/?LinkId=513126)</span></span>
- <span data-ttu-id="bcb88-133">Projekt ASP.NET Core - [co se stalo – projekty ASP.NET 5](http://go.microsoft.com/fwlink/p/?LinkId=513124)</span><span class="sxs-lookup"><span data-stu-id="bcb88-133">ASP.NET Core project - [What happened – ASP.NET 5 Projects](http://go.microsoft.com/fwlink/p/?LinkId=513124)</span></span> 
- <span data-ttu-id="bcb88-134">Projekt cloudové služby (webových rolí a rolí pracovního procesu) - [co se stalo – projekty cloudových služeb](http://go.microsoft.com/fwlink/p/?LinkId=516965)</span><span class="sxs-lookup"><span data-stu-id="bcb88-134">Cloud service project (web roles and worker roles) - [What happened – Cloud Service projects](http://go.microsoft.com/fwlink/p/?LinkId=516965)</span></span>
- <span data-ttu-id="bcb88-135">Projekt webové úlohy – [co se stalo – projekty webové úlohy](visual-studio/vs-storage-webjobs-what-happened.md)</span><span class="sxs-lookup"><span data-stu-id="bcb88-135">WebJob project - [What happened - WebJob projects](visual-studio/vs-storage-webjobs-what-happened.md)</span></span>

## <a name="next-steps"></a><span data-ttu-id="bcb88-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bcb88-136">Next steps</span></span>
- [<span data-ttu-id="bcb88-137">Fóru MSDN: Úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="bcb88-137">MSDN Forum: Azure Storage</span></span>](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata)
- [<span data-ttu-id="bcb88-138">Blog týmu Microsoft Azure Storage</span><span class="sxs-lookup"><span data-stu-id="bcb88-138">Microsoft Azure Storage Team Blog</span></span>](http://blogs.msdn.com/b/windowsazurestorage/)
- [<span data-ttu-id="bcb88-139">Dokumentace k Azure Storage</span><span class="sxs-lookup"><span data-stu-id="bcb88-139">Azure Storage documentation</span></span>](https://docs.microsoft.com/azure/storage/)
