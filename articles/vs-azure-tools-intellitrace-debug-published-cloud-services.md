---
title: "Ladění publikovaný Azure cloud service pomocí sady Visual Studio a IntelliTrace | Microsoft Docs"
description: "Zjistěte, jak ladit Cloudová služba se Visual Studio a IntelliTrace"
services: visual-studio-online
documentationcenter: n/a
author: kraigb
manager: ghogen
editor: 
ms.assetid: 5e6662fc-b917-43ea-bf2b-4f2fc3d213dc
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: 7905dfb97cbd7578a8422567fe674839d00c21ef
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="debugging-a-published-azure-cloud-service-with-visual-studio-and-intellitrace"></a><span data-ttu-id="75238-103">Ladění publikovaný Azure cloud service sadou Visual Studio a IntelliTrace</span><span class="sxs-lookup"><span data-stu-id="75238-103">Debugging a published Azure cloud service with Visual Studio and IntelliTrace</span></span>
<span data-ttu-id="75238-104">S použitím technologie IntelliTrace můžete protokolovat rozsáhlé ladicí informace pro instanci role při spuštění v Azure.</span><span class="sxs-lookup"><span data-stu-id="75238-104">With IntelliTrace, you can log extensive debugging information for a role instance when it runs in Azure.</span></span> <span data-ttu-id="75238-105">Pokud potřebujete najít příčinu problému, můžete protokoly IntelliTrace krokovat kód ze sady Visual Studio, jako kdyby byly spuštěny v Azure.</span><span class="sxs-lookup"><span data-stu-id="75238-105">If you need to find the cause of a problem, you can use the IntelliTrace logs to step through your code from Visual Studio as if it were running in Azure.</span></span> <span data-ttu-id="75238-106">IntelliTrace záznamy ve skutečnosti klíčů provádění kódu a dat prostředí, když vaše aplikace Azure běží jako cloudová služba v Azure a umožňuje přehrát zaznamenaná data ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="75238-106">In effect, IntelliTrace records key code execution and environment data when your Azure application is running as a cloud service in Azure, and lets you replay the recorded data from Visual Studio.</span></span> 

<span data-ttu-id="75238-107">Můžete použít IntelliTrace, pokud máte Visual Studio Enterprise nainstalován a cíle vaší aplikace Azure rozhraní .NET Framework 4 nebo novější verze.</span><span class="sxs-lookup"><span data-stu-id="75238-107">You can use IntelliTrace if you have Visual Studio Enterprise installed and your Azure application targets .NET Framework 4 or a later version.</span></span> <span data-ttu-id="75238-108">IntelliTrace shromažďuje informace o Azure role.</span><span class="sxs-lookup"><span data-stu-id="75238-108">IntelliTrace collects information for your Azure roles.</span></span> <span data-ttu-id="75238-109">Virtuální počítače pro tyto role vždy spouštět 64bitové operační systémy.</span><span class="sxs-lookup"><span data-stu-id="75238-109">The virtual machines for these roles always run 64-bit operating systems.</span></span>

<span data-ttu-id="75238-110">Jako alternativu, můžete použít [vzdálené ladění](http://go.microsoft.com/fwlink/p/?LinkId=623041) připojit přímo k Cloudová služba, která běží v Azure.</span><span class="sxs-lookup"><span data-stu-id="75238-110">As an alternative, you can use [remote debugging](http://go.microsoft.com/fwlink/p/?LinkId=623041) to attach directly to a cloud service that's running in Azure.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="75238-111">IntelliTrace je určený jenom pro účely ladění a by nemělo být použito pro produkční nasazení.</span><span class="sxs-lookup"><span data-stu-id="75238-111">IntelliTrace is intended for debug scenarios only, and should not be used for a production deployment.</span></span>
> 

## <a name="configure-an-azure-application-for-intellitrace"></a><span data-ttu-id="75238-112">Konfigurace aplikací Azure pro IntelliTrace</span><span class="sxs-lookup"><span data-stu-id="75238-112">Configure an Azure application for IntelliTrace</span></span>
<span data-ttu-id="75238-113">Pokud chcete povolit IntelliTrace pro aplikaci Azure, musíte vytvořit a publikování aplikace z projektu Visual Studio Azure.</span><span class="sxs-lookup"><span data-stu-id="75238-113">To enable IntelliTrace for an Azure application, you must create and publish the application from a Visual Studio Azure project.</span></span> <span data-ttu-id="75238-114">Před publikováním do Azure, je nutné nakonfigurovat IntelliTrace pro vaše aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="75238-114">You must configure IntelliTrace for your Azure application before you publish it to Azure.</span></span> <span data-ttu-id="75238-115">Pokud publikujete aplikaci bez konfigurace IntelliTrace, budete muset znovu publikovat projektu.</span><span class="sxs-lookup"><span data-stu-id="75238-115">If you publish your application without configuring IntelliTrace, you need to republish the project.</span></span> <span data-ttu-id="75238-116">Další informace najdete v tématu [publikování Azure cloud services projektů pomocí sady Visual Studio](http://go.microsoft.com/fwlink/p/?LinkId=623012).</span><span class="sxs-lookup"><span data-stu-id="75238-116">For more information, see [Publishing an Azure cloud services projects using Visual Studio](http://go.microsoft.com/fwlink/p/?LinkId=623012).</span></span>

1. <span data-ttu-id="75238-117">Až budete připravení nasadit aplikaci Azure, ověřte, že jsou vaše cíle sestavení projektu nastavena na **ladění**.</span><span class="sxs-lookup"><span data-stu-id="75238-117">When you are ready to deploy your Azure application, verify that your project build targets are set to **Debug**.</span></span>

1. <span data-ttu-id="75238-118">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a v místní nabídce vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="75238-118">In **Solution Explorer**, right-click project, and, from the context menu, select **Publish**.</span></span>
   
1. <span data-ttu-id="75238-119">V **publikování aplikaci Azure** dialogové okno, vyberte předplatné Azure a vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="75238-119">In the **Publish Azure Application** dialog, select the Azure subscription, and select **Next**.</span></span>

1. <span data-ttu-id="75238-120">V **nastavení** vyberte **Upřesnit nastavení** kartě.</span><span class="sxs-lookup"><span data-stu-id="75238-120">In the **Settings** page, select the **Advanced Settings** tab.</span></span>

1. <span data-ttu-id="75238-121">Zapnout **povolit IntelliTrace** možnost shromáždit protokoly IntelliTrace pro vaši aplikaci, když je publikován v cloudu.</span><span class="sxs-lookup"><span data-stu-id="75238-121">Turn on the **Enable IntelliTrace** option to collect IntelliTrace logs for your application when it is published in the cloud.</span></span>
   
1. <span data-ttu-id="75238-122">Chcete-li přizpůsobit základní konfiguraci IntelliTrace, vyberte **nastavení** vedle **povolit IntelliTrace**.</span><span class="sxs-lookup"><span data-stu-id="75238-122">To customize the basic IntelliTrace configuration, select **Settings** next to **Enable IntelliTrace**.</span></span>

    ![Odkaz nastavení IntelliTrace](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/intellitrace-settings-link.png)
   
1. <span data-ttu-id="75238-124">V **IntelliTrace nastavení** dialogové okno, můžete zadat které události do protokolu, zda se mají shromažďovat informace o volání, které moduly a procesy shromažďování protokolů a kolik místa k přidělení k záznamu.</span><span class="sxs-lookup"><span data-stu-id="75238-124">In the **IntelliTrace Settings** dialog, you can specify which events to log, whether to collect call information, which modules and processes to collect logs for, and how much space to allocate to the recording.</span></span> <span data-ttu-id="75238-125">Další informace o IntelliTrace najdete v tématu [ladění pomocí technologie IntelliTrace](http://go.microsoft.com/fwlink/?LinkId=214468).</span><span class="sxs-lookup"><span data-stu-id="75238-125">For more information about IntelliTrace, see [Debugging with IntelliTrace](http://go.microsoft.com/fwlink/?LinkId=214468).</span></span>
   
    ![Nastavení IntelliTrace](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC519063.png)

<span data-ttu-id="75238-127">V protokolu IntelliTrace je cyklický soubor protokolu maximální velikost zadaná v nastavení IntelliTrace (výchozí velikost je 250 MB).</span><span class="sxs-lookup"><span data-stu-id="75238-127">The IntelliTrace log is a circular log file of the maximum size specified in the IntelliTrace settings (the default size is 250 MB).</span></span> <span data-ttu-id="75238-128">Do souboru v systému souborů virtuálního počítače se shromáždí protokoly IntelliTrace.</span><span class="sxs-lookup"><span data-stu-id="75238-128">IntelliTrace logs are collected to a file in the file system of the virtual machine.</span></span> <span data-ttu-id="75238-129">Pokud budete požadovat protokoly, je snímek prováděné v tomto bodě v čase a stahovat do místního počítače.</span><span class="sxs-lookup"><span data-stu-id="75238-129">When you request the logs, a snapshot is taken at that point in time and downloaded to your local computer.</span></span>

<span data-ttu-id="75238-130">Po cloudovou službu systému Azure se publikoval do Azure, můžete určit, pokud bylo povoleno IntelliTrace z uzlu Azure v **Průzkumníka serveru**, jak je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="75238-130">After the Azure cloud service has been published to Azure, you can determine if IntelliTrace has been enabled from the Azure node in **Server Explorer**, as shown in the following image:</span></span>

![Průzkumník serveru - IntelliTrace povoleno](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC744134.png)

## <a name="download-intellitrace-logs-for-a-role-instance"></a><span data-ttu-id="75238-132">Stažení protokolů IntelliTrace pro instanci role</span><span class="sxs-lookup"><span data-stu-id="75238-132">Download IntelliTrace logs for a role instance</span></span>
<span data-ttu-id="75238-133">Pomocí sady Visual Studio, můžete stáhnout protokoly IntelliTrace pro instanci role pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="75238-133">Using Visual Studio, you can download IntelliTrace logs for a role instance by following these steps:</span></span>

1. <span data-ttu-id="75238-134">V **Průzkumníka serveru**, rozbalte **cloudové služby** uzel a vyhledejte instanci role, jejichž protokoly, které chcete stáhnout.</span><span class="sxs-lookup"><span data-stu-id="75238-134">In **Server Explorer**, expand the **Cloud Services** node, and locate role instance whose logs you wish to download.</span></span> 

1. <span data-ttu-id="75238-135">Klikněte pravým tlačítkem na instanci role a v místní nabídce s vyberte **zobrazit protokoly IntelliTrace**.</span><span class="sxs-lookup"><span data-stu-id="75238-135">Right-click the role instance, and from the s context menu, select **View IntelliTrace Logs**.</span></span> 

    ![Zobrazit možnosti nabídky protokoly IntelliTrace](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/view-intellitrace-logs.png)

1. <span data-ttu-id="75238-137">Protokoly IntelliTrace se stáhnou do souboru v adresáři v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="75238-137">The IntelliTrace logs are downloaded to a file in a directory on your local computer.</span></span> <span data-ttu-id="75238-138">Pokaždé, když, abyste si vyžádali nástroje IntelliTrace v protokolech, se vytvoří nový snímek.</span><span class="sxs-lookup"><span data-stu-id="75238-138">Each time that you request the IntelliTrace logs, a new snapshot is created.</span></span> <span data-ttu-id="75238-139">Během stahování protokolů, Visual Studio zobrazí průběh operace v **protokol činnosti Azure** okno.</span><span class="sxs-lookup"><span data-stu-id="75238-139">While the logs are being downloaded, Visual Studio displays the progress of the operation in the **Azure Activity Log** window.</span></span> <span data-ttu-id="75238-140">Jak je znázorněno na následujícím obrázku, můžete rozbalit položky řádku pro operaci pro zobrazení dalších podrobností.</span><span class="sxs-lookup"><span data-stu-id="75238-140">As shown in the following figure, you can expand the line item for the operation to see more detail.</span></span>

![VST_IntelliTraceDownloadProgress](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC745551.png)

<span data-ttu-id="75238-142">Můžete pokračovat v práci v sadě Visual Studio, když jsou stahování protokolů IntelliTrace.</span><span class="sxs-lookup"><span data-stu-id="75238-142">You can continue to work in Visual Studio while the IntelliTrace logs are downloading.</span></span> <span data-ttu-id="75238-143">Po dokončení stahování protokol otevře v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="75238-143">When the log has finished downloading, it opens in Visual Studio.</span></span>

> [!NOTE]
> <span data-ttu-id="75238-144">Protokoly IntelliTrace může obsahovat výjimky, které rozhraní generuje a zpracovává později.</span><span class="sxs-lookup"><span data-stu-id="75238-144">The IntelliTrace logs might contain exceptions that the framework generates and handles afterwards.</span></span> <span data-ttu-id="75238-145">Kód vnitřní framework generuje tyto výjimky jako součást normální spuštění role, takže může bezpečně ignorovat.</span><span class="sxs-lookup"><span data-stu-id="75238-145">Internal framework code generates these exceptions as a normal part of starting up a role, so you may safely ignore them.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="75238-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="75238-146">Next steps</span></span>
- [<span data-ttu-id="75238-147">Možnosti pro ladění cloudové služby Azure</span><span class="sxs-lookup"><span data-stu-id="75238-147">Options for debugging Azure cloud services</span></span>](vs-azure-tools-debugging-cloud-services-overview.md)
- [<span data-ttu-id="75238-148">Publikování Azure cloud service pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="75238-148">Publishing an Azure cloud service using Visual Studio</span></span>](vs-azure-tools-publishing-a-cloud-service.md)