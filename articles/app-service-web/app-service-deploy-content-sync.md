---
title: "aaaSync obsah ze složky tooAzure cloudové služby App Service"
description: "Zjistěte, jak toodeploy aplikace tooAzure služby App Service přes obsah synchronizovat ze složky cloudu."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: 88d3a670-303a-4fa2-9de9-715cc904acec
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2016
ms.author: dariagrigoriu
ms.openlocfilehash: e1c6d53a427c36126d9cdb33cc21b4126b9d9c2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sync-content-from-a-cloud-folder-tooazure-app-service"></a><span data-ttu-id="14e4f-103">Synchronizace obsahu ze složky tooAzure cloudové služby App Service</span><span class="sxs-lookup"><span data-stu-id="14e4f-103">Sync content from a cloud folder tooAzure App Service</span></span>
<span data-ttu-id="14e4f-104">Tento kurz ukazuje, jak toodeploy příliš[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) podle synchronizuje svůj obsah z oblíbených cloudových úložiště služby, jako je Dropbox a OneDrive.</span><span class="sxs-lookup"><span data-stu-id="14e4f-104">This tutorial shows you how toodeploy too[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) by syncing your content from popular cloud storage services like Dropbox and OneDrive.</span></span> 

## <span data-ttu-id="14e4f-105"><a name="overview"></a>Přehled nasazení obsahu synchronizace</span><span class="sxs-lookup"><span data-stu-id="14e4f-105"><a name="overview"></a>Overview of content sync deployment</span></span>
<span data-ttu-id="14e4f-106">nasazení synchronizace obsahu na vyžádání Hello používá technologii hello [modul nasazení Kudu](https://github.com/projectkudu/kudu/wiki) integrované se službou App Service.</span><span class="sxs-lookup"><span data-stu-id="14e4f-106">hello on-demand content sync deployment is powered by hello [Kudu deployment engine](https://github.com/projectkudu/kudu/wiki) integrated with App Service.</span></span> <span data-ttu-id="14e4f-107">V hello [portálu Azure](https://portal.azure.com), lze určit složku v cloudovém úložišti, pracovat s kódu aplikace a obsah v této složce a tooApp synchronizační službu s hello klikněte tlačítko.</span><span class="sxs-lookup"><span data-stu-id="14e4f-107">In hello [Azure Portal](https://portal.azure.com), you can designate a folder in your cloud storage, work with your app code and content in that folder, and sync tooApp Service with hello click of a button.</span></span> <span data-ttu-id="14e4f-108">Synchronizace obsahu využívá pro proces hello Kudu pro sestavení a nasazení.</span><span class="sxs-lookup"><span data-stu-id="14e4f-108">Content sync utilizes hello Kudu process for build and deployment.</span></span> 

## <span data-ttu-id="14e4f-109"><a name="contentsync"></a>Jak obsah tooenable synchronizovat nasazení</span><span class="sxs-lookup"><span data-stu-id="14e4f-109"><a name="contentsync"></a>How tooenable content sync deployment</span></span>
<span data-ttu-id="14e4f-110">tooenable obsahu sync z hello [portálu Azure](https://portal.azure.com), postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="14e4f-110">tooenable content sync from hello [Azure Portal](https://portal.azure.com), follow these steps:</span></span>

1. <span data-ttu-id="14e4f-111">V okně vaší aplikace v hello portálu Azure, klikněte na tlačítko **nastavení** > **zdroj nasazení**.</span><span class="sxs-lookup"><span data-stu-id="14e4f-111">In your app's blade in hello Azure Portal, click **Settings** > **Deployment Source**.</span></span> <span data-ttu-id="14e4f-112">Klikněte na tlačítko **zvolit zdroj**, pak vyberte **OneDrive** nebo **Dropbox** jako hello zdroj pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="14e4f-112">Click **Choose Source**, then select **OneDrive** or **Dropbox** as hello source for deployment.</span></span> 
   
    ![Synchronizace obsahu](./media/app-service-deploy-content-sync/deployment_source.png)
   
   > [!NOTE]
   > <span data-ttu-id="14e4f-114">Kvůli základním rozdílům v hello rozhraní API **Onedrivu pro firmy** v tuto chvíli není podporován.</span><span class="sxs-lookup"><span data-stu-id="14e4f-114">Because of underlying differences in hello APIs, **OneDrive for Business** is not supported at this time.</span></span> 
   > 
   > 
2. <span data-ttu-id="14e4f-115">Dokončení hello autorizace pracovního postupu tooenable služby App Service tooaccess definovány konkrétní předem určenou cestu pro OneDrive nebo Dropbox, kde veškerý obsah vaší služby App Service bude uložena.</span><span class="sxs-lookup"><span data-stu-id="14e4f-115">Complete hello authorization workflow tooenable App Service tooaccess a specific pre-defined designated path for OneDrive or Dropbox where all of your App Service content will be stored.</span></span>  
    <span data-ttu-id="14e4f-116">Po hello autorizace služby App Service platformy získáte možnost toocreate hello složku obsahu v rámci hello určené cestu obsahu nebo toochoose existující složku obsahu v této cestě určené obsahu.</span><span class="sxs-lookup"><span data-stu-id="14e4f-116">After authorization hello App Service platform will give you hello option toocreate a content folder under hello designated content path, or toochoose an existing content folder under this designated content path.</span></span> <span data-ttu-id="14e4f-117">cesty obsahu Hello uvedených v části vaší cloudové úložiště účty používané pro synchronizaci služby App Service jsou hello následující:</span><span class="sxs-lookup"><span data-stu-id="14e4f-117">hello designated content paths under your cloud storage accounts used for App Service sync are hello following:</span></span>  
   
   * <span data-ttu-id="14e4f-118">**OneDrive**:`Apps\Azure Web Apps`</span><span class="sxs-lookup"><span data-stu-id="14e4f-118">**OneDrive**: `Apps\Azure Web Apps`</span></span> 
   * <span data-ttu-id="14e4f-119">**Dropbox**:`Dropbox\Apps\Azure`</span><span class="sxs-lookup"><span data-stu-id="14e4f-119">**Dropbox**: `Dropbox\Apps\Azure`</span></span>
3. <span data-ttu-id="14e4f-120">Po hello lze inicializovat obsahu synchronizace hello počáteční synchronizace obsahu na vyžádání z portálu Azure hello.</span><span class="sxs-lookup"><span data-stu-id="14e4f-120">After hello initial content sync hello content sync can be initiated on demand from hello Azure portal.</span></span> <span data-ttu-id="14e4f-121">Historie nasazení je k dispozici s hello **nasazení** okno.</span><span class="sxs-lookup"><span data-stu-id="14e4f-121">Deployment history is available with hello **Deployments** blade.</span></span>
   
    ![Historie nasazení](./media/app-service-deploy-content-sync/onedrive_sync.png)

<span data-ttu-id="14e4f-123">Další informace o nasazení Dropbox je k dispozici v části [nasazení ze Dropbox](http://blogs.msdn.com/b/windowsazure/archive/2013/03/19/new-deploy-to-windows-azure-web-sites-from-dropbox.aspx).</span><span class="sxs-lookup"><span data-stu-id="14e4f-123">More information for Dropbox deployment is available under [Deploy from Dropbox](http://blogs.msdn.com/b/windowsazure/archive/2013/03/19/new-deploy-to-windows-azure-web-sites-from-dropbox.aspx).</span></span> 

