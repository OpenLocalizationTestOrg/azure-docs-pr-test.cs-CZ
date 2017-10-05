---
title: "Synchronizace obsahu ze složky cloudu do služby Azure App Service"
description: "Informace o nasazení aplikace do služby Azure App Service prostřednictvím synchronizace obsahu ze složky cloudu."
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
ms.openlocfilehash: 010e7dc492abefaa3afe814c0322af9f6fe5acd2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="sync-content-from-a-cloud-folder-to-azure-app-service"></a><span data-ttu-id="75c3d-103">Synchronizace obsahu ze složky cloudu do služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="75c3d-103">Sync content from a cloud folder to Azure App Service</span></span>
<span data-ttu-id="75c3d-104">V tomto kurzu se dozvíte, jak nasadit do [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) podle synchronizuje svůj obsah z oblíbených cloudových úložiště služby, jako je Dropbox a OneDrive.</span><span class="sxs-lookup"><span data-stu-id="75c3d-104">This tutorial shows you how to deploy to [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) by syncing your content from popular cloud storage services like Dropbox and OneDrive.</span></span> 

## <span data-ttu-id="75c3d-105"><a name="overview"></a>Přehled nasazení obsahu synchronizace</span><span class="sxs-lookup"><span data-stu-id="75c3d-105"><a name="overview"></a>Overview of content sync deployment</span></span>
<span data-ttu-id="75c3d-106">Používá technologii nasazení synchronizace obsahu na vyžádání [modul nasazení Kudu](https://github.com/projectkudu/kudu/wiki) integrované se službou App Service.</span><span class="sxs-lookup"><span data-stu-id="75c3d-106">The on-demand content sync deployment is powered by the [Kudu deployment engine](https://github.com/projectkudu/kudu/wiki) integrated with App Service.</span></span> <span data-ttu-id="75c3d-107">V [portálu Azure](https://portal.azure.com), můžete určit složku v cloudovém úložišti, pracovat s kódu aplikace a obsah v této složce a kliknutím tlačítko Synchronizovat do služby App Service.</span><span class="sxs-lookup"><span data-stu-id="75c3d-107">In the [Azure Portal](https://portal.azure.com), you can designate a folder in your cloud storage, work with your app code and content in that folder, and sync to App Service with the click of a button.</span></span> <span data-ttu-id="75c3d-108">Synchronizace obsahu využívá pro proces Kudu pro sestavení a nasazení.</span><span class="sxs-lookup"><span data-stu-id="75c3d-108">Content sync utilizes the Kudu process for build and deployment.</span></span> 

## <span data-ttu-id="75c3d-109"><a name="contentsync"></a>Postup povolení nasazení obsahu synchronizace</span><span class="sxs-lookup"><span data-stu-id="75c3d-109"><a name="contentsync"></a>How to enable content sync deployment</span></span>
<span data-ttu-id="75c3d-110">Povolit synchronizaci obsahu z [portálu Azure](https://portal.azure.com), postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="75c3d-110">To enable content sync from the [Azure Portal](https://portal.azure.com), follow these steps:</span></span>

1. <span data-ttu-id="75c3d-111">V okně vaší aplikace na portálu Azure, klikněte na tlačítko **nastavení** > **zdroj nasazení**.</span><span class="sxs-lookup"><span data-stu-id="75c3d-111">In your app's blade in the Azure Portal, click **Settings** > **Deployment Source**.</span></span> <span data-ttu-id="75c3d-112">Klikněte na tlačítko **zvolit zdroj**, pak vyberte **OneDrive** nebo **Dropbox** jako zdroj pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="75c3d-112">Click **Choose Source**, then select **OneDrive** or **Dropbox** as the source for deployment.</span></span> 
   
    ![Synchronizace obsahu](./media/app-service-deploy-content-sync/deployment_source.png)
   
   > [!NOTE]
   > <span data-ttu-id="75c3d-114">Kvůli základním rozdílům v rozhraní API **Onedrivu pro firmy** v tuto chvíli není podporován.</span><span class="sxs-lookup"><span data-stu-id="75c3d-114">Because of underlying differences in the APIs, **OneDrive for Business** is not supported at this time.</span></span> 
   > 
   > 
2. <span data-ttu-id="75c3d-115">Dokončení autorizace pracovního postupu povolení aplikaci služby pro přístup k cestě určené pro konkrétní předem definovaná pro OneDrive nebo Dropbox, kde veškerý obsah vaší služby App Service bude uložena.</span><span class="sxs-lookup"><span data-stu-id="75c3d-115">Complete the authorization workflow to enable App Service to access a specific pre-defined designated path for OneDrive or Dropbox where all of your App Service content will be stored.</span></span>  
    <span data-ttu-id="75c3d-116">Po autorizace služby App Service platformy získáte možnost vytvořit složku obsahu v určené cestě obsahu, nebo vybrat existující složku obsahu v této cestě určené obsahu.</span><span class="sxs-lookup"><span data-stu-id="75c3d-116">After authorization the App Service platform will give you the option to create a content folder under the designated content path, or to choose an existing content folder under this designated content path.</span></span> <span data-ttu-id="75c3d-117">Určené cesty obsahu v rámci vaší cloudové úložiště účty používané pro synchronizaci služby App Service jsou následující:</span><span class="sxs-lookup"><span data-stu-id="75c3d-117">The designated content paths under your cloud storage accounts used for App Service sync are the following:</span></span>  
   
   * <span data-ttu-id="75c3d-118">**OneDrive**:`Apps\Azure Web Apps`</span><span class="sxs-lookup"><span data-stu-id="75c3d-118">**OneDrive**: `Apps\Azure Web Apps`</span></span> 
   * <span data-ttu-id="75c3d-119">**Dropbox**:`Dropbox\Apps\Azure`</span><span class="sxs-lookup"><span data-stu-id="75c3d-119">**Dropbox**: `Dropbox\Apps\Azure`</span></span>
3. <span data-ttu-id="75c3d-120">Po počáteční synchronizaci obsahu lze inicializovat synchronizace obsahu na vyžádání z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="75c3d-120">After the initial content sync the content sync can be initiated on demand from the Azure portal.</span></span> <span data-ttu-id="75c3d-121">Historie nasazení je k dispozici **nasazení** okno.</span><span class="sxs-lookup"><span data-stu-id="75c3d-121">Deployment history is available with the **Deployments** blade.</span></span>
   
    ![Historie nasazení](./media/app-service-deploy-content-sync/onedrive_sync.png)

<span data-ttu-id="75c3d-123">Další informace o nasazení Dropbox je k dispozici v části [nasazení ze Dropbox](http://blogs.msdn.com/b/windowsazure/archive/2013/03/19/new-deploy-to-windows-azure-web-sites-from-dropbox.aspx).</span><span class="sxs-lookup"><span data-stu-id="75c3d-123">More information for Dropbox deployment is available under [Deploy from Dropbox](http://blogs.msdn.com/b/windowsazure/archive/2013/03/19/new-deploy-to-windows-azure-web-sites-from-dropbox.aspx).</span></span> 

