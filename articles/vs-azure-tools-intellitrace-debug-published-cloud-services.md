---
title: "aaaDebugging publikované na Azure cloud service pomocí sady Visual Studio a IntelliTrace | Microsoft Docs"
description: "Zjistěte, jak služba toodebug cloudu s Visual Studio a IntelliTrace"
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
ms.openlocfilehash: 60734a71d5499de72e1ca81a3ab0ab9f34bc303a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="debugging-a-published-azure-cloud-service-with-visual-studio-and-intellitrace"></a><span data-ttu-id="1bc86-103">Ladění publikovaný Azure cloud service sadou Visual Studio a IntelliTrace</span><span class="sxs-lookup"><span data-stu-id="1bc86-103">Debugging a published Azure cloud service with Visual Studio and IntelliTrace</span></span>
<span data-ttu-id="1bc86-104">S použitím technologie IntelliTrace můžete protokolovat rozsáhlé ladicí informace pro instanci role při spuštění v Azure.</span><span class="sxs-lookup"><span data-stu-id="1bc86-104">With IntelliTrace, you can log extensive debugging information for a role instance when it runs in Azure.</span></span> <span data-ttu-id="1bc86-105">Pokud potřebujete toofind hello příčinu problému, můžete použít toostep protokoly IntelliTrace hello prostřednictvím kódu ze sady Visual Studio jako kdyby byly spuštěny v Azure.</span><span class="sxs-lookup"><span data-stu-id="1bc86-105">If you need toofind hello cause of a problem, you can use hello IntelliTrace logs toostep through your code from Visual Studio as if it were running in Azure.</span></span> <span data-ttu-id="1bc86-106">Ve skutečnosti dat technologie IntelliTrace záznamy klíče kódu provádění a prostředí při aplikaci Azure běží jako cloudová služba v Azure a umožňuje vám přehráním hello zaznamenaná data ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1bc86-106">In effect, IntelliTrace records key code execution and environment data when your Azure application is running as a cloud service in Azure, and lets you replay hello recorded data from Visual Studio.</span></span> 

<span data-ttu-id="1bc86-107">Můžete použít IntelliTrace, pokud máte Visual Studio Enterprise nainstalován a cíle vaší aplikace Azure rozhraní .NET Framework 4 nebo novější verze.</span><span class="sxs-lookup"><span data-stu-id="1bc86-107">You can use IntelliTrace if you have Visual Studio Enterprise installed and your Azure application targets .NET Framework 4 or a later version.</span></span> <span data-ttu-id="1bc86-108">IntelliTrace shromažďuje informace o Azure role.</span><span class="sxs-lookup"><span data-stu-id="1bc86-108">IntelliTrace collects information for your Azure roles.</span></span> <span data-ttu-id="1bc86-109">Hello virtuálních počítačů pro tyto role vždy spouštět 64bitové operační systémy.</span><span class="sxs-lookup"><span data-stu-id="1bc86-109">hello virtual machines for these roles always run 64-bit operating systems.</span></span>

<span data-ttu-id="1bc86-110">Jako alternativu, můžete použít [vzdálené ladění](http://go.microsoft.com/fwlink/p/?LinkId=623041) tooattach přímo Cloudová služba tooa, který běží v Azure.</span><span class="sxs-lookup"><span data-stu-id="1bc86-110">As an alternative, you can use [remote debugging](http://go.microsoft.com/fwlink/p/?LinkId=623041) tooattach directly tooa cloud service that's running in Azure.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1bc86-111">IntelliTrace je určený jenom pro účely ladění a by nemělo být použito pro produkční nasazení.</span><span class="sxs-lookup"><span data-stu-id="1bc86-111">IntelliTrace is intended for debug scenarios only, and should not be used for a production deployment.</span></span>
> 

## <a name="configure-an-azure-application-for-intellitrace"></a><span data-ttu-id="1bc86-112">Konfigurace aplikací Azure pro IntelliTrace</span><span class="sxs-lookup"><span data-stu-id="1bc86-112">Configure an Azure application for IntelliTrace</span></span>
<span data-ttu-id="1bc86-113">tooenable IntelliTrace pro aplikaci Azure, musíte vytvořit a publikování aplikace hello z projektu Visual Studio Azure.</span><span class="sxs-lookup"><span data-stu-id="1bc86-113">tooenable IntelliTrace for an Azure application, you must create and publish hello application from a Visual Studio Azure project.</span></span> <span data-ttu-id="1bc86-114">Před publikováním tooAzure, je nutné nakonfigurovat IntelliTrace pro vaše aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="1bc86-114">You must configure IntelliTrace for your Azure application before you publish it tooAzure.</span></span> <span data-ttu-id="1bc86-115">Pokud publikujete aplikaci bez konfigurace IntelliTrace, je třeba toorepublish hello projektu.</span><span class="sxs-lookup"><span data-stu-id="1bc86-115">If you publish your application without configuring IntelliTrace, you need toorepublish hello project.</span></span> <span data-ttu-id="1bc86-116">Další informace najdete v tématu [publikování Azure cloud services projektů pomocí sady Visual Studio](http://go.microsoft.com/fwlink/p/?LinkId=623012).</span><span class="sxs-lookup"><span data-stu-id="1bc86-116">For more information, see [Publishing an Azure cloud services projects using Visual Studio](http://go.microsoft.com/fwlink/p/?LinkId=623012).</span></span>

1. <span data-ttu-id="1bc86-117">Až budete připraveni toodeploy vaše aplikace Azure, ověřte, zda jsou vaše cíle sestavení projektu nastavena příliš**ladění**.</span><span class="sxs-lookup"><span data-stu-id="1bc86-117">When you are ready toodeploy your Azure application, verify that your project build targets are set too**Debug**.</span></span>

1. <span data-ttu-id="1bc86-118">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a hello místní nabídce vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="1bc86-118">In **Solution Explorer**, right-click project, and, from hello context menu, select **Publish**.</span></span>
   
1. <span data-ttu-id="1bc86-119">V hello **publikování aplikaci Azure** dialogové okno, vyberte hello předplatného Azure a vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="1bc86-119">In hello **Publish Azure Application** dialog, select hello Azure subscription, and select **Next**.</span></span>

1. <span data-ttu-id="1bc86-120">V hello **nastavení** stránky, vyberte hello **Upřesnit nastavení** kartě.</span><span class="sxs-lookup"><span data-stu-id="1bc86-120">In hello **Settings** page, select hello **Advanced Settings** tab.</span></span>

1. <span data-ttu-id="1bc86-121">Zapnout hello **povolit IntelliTrace** možnost toocollect protokoly IntelliTrace pro vaši aplikaci, když je publikován v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="1bc86-121">Turn on hello **Enable IntelliTrace** option toocollect IntelliTrace logs for your application when it is published in hello cloud.</span></span>
   
1. <span data-ttu-id="1bc86-122">toocustomize hello základní IntelliTrace konfiguraci, vyberte **nastavení** další příliš**povolit IntelliTrace**.</span><span class="sxs-lookup"><span data-stu-id="1bc86-122">toocustomize hello basic IntelliTrace configuration, select **Settings** next too**Enable IntelliTrace**.</span></span>

    ![Odkaz nastavení IntelliTrace](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/intellitrace-settings-link.png)
   
1. <span data-ttu-id="1bc86-124">V hello **IntelliTrace nastavení** dialogové okno, můžete zadat, které události toolog, jestli toocollect volání informace, které procesy a moduly toocollect v protokolech a kolik místa tooallocate toohello záznam.</span><span class="sxs-lookup"><span data-stu-id="1bc86-124">In hello **IntelliTrace Settings** dialog, you can specify which events toolog, whether toocollect call information, which modules and processes toocollect logs for, and how much space tooallocate toohello recording.</span></span> <span data-ttu-id="1bc86-125">Další informace o IntelliTrace najdete v tématu [ladění pomocí technologie IntelliTrace](http://go.microsoft.com/fwlink/?LinkId=214468).</span><span class="sxs-lookup"><span data-stu-id="1bc86-125">For more information about IntelliTrace, see [Debugging with IntelliTrace](http://go.microsoft.com/fwlink/?LinkId=214468).</span></span>
   
    ![Nastavení IntelliTrace](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC519063.png)

<span data-ttu-id="1bc86-127">protokolu IntelliTrace Hello je cyklický soubor protokolu hello maximální velikost zadaná v nastavení IntelliTrace hello (hello výchozí velikost je 250 MB).</span><span class="sxs-lookup"><span data-stu-id="1bc86-127">hello IntelliTrace log is a circular log file of hello maximum size specified in hello IntelliTrace settings (hello default size is 250 MB).</span></span> <span data-ttu-id="1bc86-128">Protokoly IntelliTrace jsou shromážděná tooa souboru v systému souborů hello hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1bc86-128">IntelliTrace logs are collected tooa file in hello file system of hello virtual machine.</span></span> <span data-ttu-id="1bc86-129">Pokud budete požadovat hello protokoly, snímek je prováděné v tomto bodě v čase a stahovat tooyour místního počítače.</span><span class="sxs-lookup"><span data-stu-id="1bc86-129">When you request hello logs, a snapshot is taken at that point in time and downloaded tooyour local computer.</span></span>

<span data-ttu-id="1bc86-130">Po hello cloudové služby Azure publikované tooAzure, můžete určit, pokud bylo povoleno IntelliTrace z hello Azure uzlu v **Průzkumníka serveru**, jak ukazuje následující obrázek hello:</span><span class="sxs-lookup"><span data-stu-id="1bc86-130">After hello Azure cloud service has been published tooAzure, you can determine if IntelliTrace has been enabled from hello Azure node in **Server Explorer**, as shown in hello following image:</span></span>

![Průzkumník serveru - IntelliTrace povoleno](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC744134.png)

## <a name="download-intellitrace-logs-for-a-role-instance"></a><span data-ttu-id="1bc86-132">Stažení protokolů IntelliTrace pro instanci role</span><span class="sxs-lookup"><span data-stu-id="1bc86-132">Download IntelliTrace logs for a role instance</span></span>
<span data-ttu-id="1bc86-133">Pomocí sady Visual Studio, můžete stáhnout protokoly IntelliTrace pro instanci role pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="1bc86-133">Using Visual Studio, you can download IntelliTrace logs for a role instance by following these steps:</span></span>

1. <span data-ttu-id="1bc86-134">V **Průzkumníka serveru**, rozbalte položku hello **cloudové služby** uzel a vyhledejte instanci role, jejichž protokoly, které chcete toodownload.</span><span class="sxs-lookup"><span data-stu-id="1bc86-134">In **Server Explorer**, expand hello **Cloud Services** node, and locate role instance whose logs you wish toodownload.</span></span> 

1. <span data-ttu-id="1bc86-135">Klikněte pravým tlačítkem na instanci role hello a hello s místní nabídce vyberte **zobrazit protokoly IntelliTrace**.</span><span class="sxs-lookup"><span data-stu-id="1bc86-135">Right-click hello role instance, and from hello s context menu, select **View IntelliTrace Logs**.</span></span> 

    ![Zobrazit možnosti nabídky protokoly IntelliTrace](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/view-intellitrace-logs.png)

1. <span data-ttu-id="1bc86-137">protokoly IntelliTrace Hello jsou tooa stažený soubor v adresáři v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="1bc86-137">hello IntelliTrace logs are downloaded tooa file in a directory on your local computer.</span></span> <span data-ttu-id="1bc86-138">Pokaždé, když, abyste si vyžádali hello protokoly IntelliTrace, se vytvoří nový snímek.</span><span class="sxs-lookup"><span data-stu-id="1bc86-138">Each time that you request hello IntelliTrace logs, a new snapshot is created.</span></span> <span data-ttu-id="1bc86-139">Během stahování hello protokoly, Visual Studio zobrazí průběh hello hello operace v hello **protokol činnosti Azure** okno.</span><span class="sxs-lookup"><span data-stu-id="1bc86-139">While hello logs are being downloaded, Visual Studio displays hello progress of hello operation in hello **Azure Activity Log** window.</span></span> <span data-ttu-id="1bc86-140">Jak ukazuje následující obrázek hello, můžete rozbalit položky na řádku hello pro operaci toosee hello podrobněji.</span><span class="sxs-lookup"><span data-stu-id="1bc86-140">As shown in hello following figure, you can expand hello line item for hello operation toosee more detail.</span></span>

![VST_IntelliTraceDownloadProgress](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC745551.png)

<span data-ttu-id="1bc86-142">V sadě Visual Studio můžete pokračovat toowork v době, kdy se stahuje protokoly IntelliTrace hello.</span><span class="sxs-lookup"><span data-stu-id="1bc86-142">You can continue toowork in Visual Studio while hello IntelliTrace logs are downloading.</span></span> <span data-ttu-id="1bc86-143">Po dokončení stahování hello protokolu otevře v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1bc86-143">When hello log has finished downloading, it opens in Visual Studio.</span></span>

> [!NOTE]
> <span data-ttu-id="1bc86-144">Hello protokoly nástroje IntelliTrace může obsahovat výjimky dané platformy hello generuje a zpracovává později.</span><span class="sxs-lookup"><span data-stu-id="1bc86-144">hello IntelliTrace logs might contain exceptions that hello framework generates and handles afterwards.</span></span> <span data-ttu-id="1bc86-145">Kód vnitřní framework generuje tyto výjimky jako součást normální spuštění role, takže může bezpečně ignorovat.</span><span class="sxs-lookup"><span data-stu-id="1bc86-145">Internal framework code generates these exceptions as a normal part of starting up a role, so you may safely ignore them.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="1bc86-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1bc86-146">Next steps</span></span>
- [<span data-ttu-id="1bc86-147">Možnosti pro ladění cloudové služby Azure</span><span class="sxs-lookup"><span data-stu-id="1bc86-147">Options for debugging Azure cloud services</span></span>](vs-azure-tools-debugging-cloud-services-overview.md)
- [<span data-ttu-id="1bc86-148">Publikování Azure cloud service pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1bc86-148">Publishing an Azure cloud service using Visual Studio</span></span>](vs-azure-tools-publishing-a-cloud-service.md)