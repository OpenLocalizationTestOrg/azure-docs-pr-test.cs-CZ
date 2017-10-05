---
title: "Shromažďování protokolů pomocí Linux Azure Diagnostics | Microsoft Docs"
description: "Tento článek popisuje, jak nastavení Azure Diagnostics pro shromažďování protokolů z clusteru Service Fabric Linux běží v Azure."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: a160d469-8b7d-4560-82dd-8500db34a44a
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/28/2017
ms.author: subramar
ms.openlocfilehash: 3e41caaeb38c55d1c6c3bfdce2f81c86b4aff4d0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="collect-logs-by-using-azure-diagnostics"></a><span data-ttu-id="81458-103">Shromažďování protokolů pomocí Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="81458-103">Collect logs by using Azure Diagnostics</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="81458-104">Windows</span><span class="sxs-lookup"><span data-stu-id="81458-104">Windows</span></span>](service-fabric-diagnostics-how-to-setup-wad.md)
> * [<span data-ttu-id="81458-105">Linux</span><span class="sxs-lookup"><span data-stu-id="81458-105">Linux</span></span>](service-fabric-diagnostics-how-to-setup-lad.md)
> 
> 

<span data-ttu-id="81458-106">Když používáte cluster služby Azure Service Fabric, je vhodné shromažďovat protokoly ze všech uzlů v centrálním umístění.</span><span class="sxs-lookup"><span data-stu-id="81458-106">When you're running an Azure Service Fabric cluster, it's a good idea to collect the logs from all the nodes in a central location.</span></span> <span data-ttu-id="81458-107">S protokoly v centrálním umístění snadno k analýze a řešení problémů, zda jsou ve vaší služby, aplikace nebo samotného clusteru.</span><span class="sxs-lookup"><span data-stu-id="81458-107">Having the logs in a central location makes it easy to analyze and troubleshoot issues, whether they are in your services, your application, or the cluster itself.</span></span> <span data-ttu-id="81458-108">Jeden způsob, jak nahrát a shromažďování protokolů je použití rozšíření diagnostiky Azure, který odešle protokoly do služby Azure Storage, Azure Application Insights nebo Azure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="81458-108">One way to upload and collect logs is to use the Azure Diagnostics extension, which uploads logs to Azure Storage, Azure Application Insights or Azure Event Hubs.</span></span> <span data-ttu-id="81458-109">Můžete také číst události z úložiště nebo Event Hubs a umístěte je v produktu, jako [analýzy protokolů](../log-analytics/log-analytics-service-fabric.md) nebo jiné řešení pro analýzu protokolu.</span><span class="sxs-lookup"><span data-stu-id="81458-109">You can also read the events from storage or Event Hubs and place them in a product such as [Log Analytics](../log-analytics/log-analytics-service-fabric.md) or another log-parsing solution.</span></span> <span data-ttu-id="81458-110">[Azure Application Insights](https://azure.microsoft.com/services/application-insights/) se dodává s komplexní protokolu vyhledávání a analýzy služby předdefinované.</span><span class="sxs-lookup"><span data-stu-id="81458-110">[Azure Application Insights](https://azure.microsoft.com/services/application-insights/) comes with a comprehensive log search and analytics service built-in.</span></span>

## <a name="log-sources-that-you-might-want-to-collect"></a><span data-ttu-id="81458-111">Protokol zdroje, které můžete chtít shromažďovat</span><span class="sxs-lookup"><span data-stu-id="81458-111">Log sources that you might want to collect</span></span>
* <span data-ttu-id="81458-112">**Protokoly Service Fabric**: vygenerované z platformy prostřednictvím [LTTng](http://lttng.org) a nahrané do účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="81458-112">**Service Fabric logs**: Emitted from the platform via [LTTng](http://lttng.org) and uploaded to your storage account.</span></span> <span data-ttu-id="81458-113">Protokoly mohou být provozní události nebo modul runtime události, které vysílá platformu.</span><span class="sxs-lookup"><span data-stu-id="81458-113">Logs can be operational events or runtime events that the platform emits.</span></span> <span data-ttu-id="81458-114">Tyto protokoly se ukládají do umístění, které určuje manifestu clusteru.</span><span class="sxs-lookup"><span data-stu-id="81458-114">These logs are stored in the location that the cluster manifest specifies.</span></span> <span data-ttu-id="81458-115">(Chcete-li získat podrobnosti o účtu úložiště, vyhledejte značky **AzureTableWinFabETWQueryable** a vyhledejte **StoreConnectionString**.)</span><span class="sxs-lookup"><span data-stu-id="81458-115">(To get the storage account details, search for the tag **AzureTableWinFabETWQueryable** and look for **StoreConnectionString**.)</span></span>
* <span data-ttu-id="81458-116">**Události aplikace**: vygenerované z kódu vaší služby.</span><span class="sxs-lookup"><span data-stu-id="81458-116">**Application events**: Emitted from your service's code.</span></span> <span data-ttu-id="81458-117">Můžete použít řešení protokolování, který zapíše soubory založený na textu protokolu – například LTTng.</span><span class="sxs-lookup"><span data-stu-id="81458-117">You can use any logging solution that writes text-based log files--for example, LTTng.</span></span> <span data-ttu-id="81458-118">Další informace najdete v dokumentaci LTTng na trasování vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="81458-118">For more information, see the LTTng documentation on tracing your application.</span></span>  

## <a name="deploy-the-diagnostics-extension"></a><span data-ttu-id="81458-119">Nasazení rozšíření diagnostiky</span><span class="sxs-lookup"><span data-stu-id="81458-119">Deploy the Diagnostics extension</span></span>
<span data-ttu-id="81458-120">Prvním krokem při shromažďování protokolů je k nasazení rozšíření diagnostiky na všech virtuálních počítačích v clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="81458-120">The first step in collecting logs is to deploy the Diagnostics extension on each of the VMs in the Service Fabric cluster.</span></span> <span data-ttu-id="81458-121">Rozšíření diagnostiky shromažďuje protokoly na každý virtuální počítač a odesílá je do účtu úložiště, který určíte.</span><span class="sxs-lookup"><span data-stu-id="81458-121">The Diagnostics extension collects logs on each VM and uploads them to the storage account that you specify.</span></span> <span data-ttu-id="81458-122">Kroky lišit v závislosti na tom, zda používáte portál Azure nebo Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="81458-122">The steps vary based on whether you use the Azure portal or Azure Resource Manager.</span></span>

<span data-ttu-id="81458-123">Chcete-li nasadit rozšíření diagnostiky pro virtuální počítače v clusteru jako součást vytváření clusteru, nastavte **diagnostiky** k **na**.</span><span class="sxs-lookup"><span data-stu-id="81458-123">To deploy the Diagnostics extension to the VMs in the cluster as part of cluster creation, set **Diagnostics** to **On**.</span></span> <span data-ttu-id="81458-124">Po vytvoření clusteru, nelze toto nastavení změnit pomocí portálu.</span><span class="sxs-lookup"><span data-stu-id="81458-124">After you create the cluster, you can't change this setting by using the portal.</span></span>

<span data-ttu-id="81458-125">Potom nakonfigurujte Linux Azure Diagnostics (LAD) pro shromažďování souborů a umístěte je do vašeho účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="81458-125">Then, configure Linux Azure Diagnostics (LAD) to collect the files and place them into your storage account.</span></span> <span data-ttu-id="81458-126">Tento postup je podrobně jako scénář 3 ("odeslání souborů protokolu") v článku [LAD pomocí monitorování a Diagnostika virtuální počítače s Linuxem](../virtual-machines/linux/classic/diagnostic-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="81458-126">This process is explained as scenario 3 ("Upload your own log files") in the article [Using LAD to monitor and diagnose Linux VMs](../virtual-machines/linux/classic/diagnostic-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span> <span data-ttu-id="81458-127">Provedení tohoto postupu získá přístup k trasování.</span><span class="sxs-lookup"><span data-stu-id="81458-127">Following this process gets you access to the traces.</span></span> <span data-ttu-id="81458-128">Trasování můžete uložit do vizualizéru podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="81458-128">You can upload the traces to a visualizer of your choice.</span></span>

<span data-ttu-id="81458-129">Můžete také nasadit rozšíření diagnostiky pomocí Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="81458-129">You can also deploy the Diagnostics extension by using Azure Resource Manager.</span></span> <span data-ttu-id="81458-130">Proces je podobný pro systém Windows a Linux a je popsané u clusterů systému Windows v [postup shromažďování protokolů pomocí Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md).</span><span class="sxs-lookup"><span data-stu-id="81458-130">The process is similar for Windows and Linux and is documented for Windows clusters in [How to collect logs with Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md).</span></span>

<span data-ttu-id="81458-131">Můžete také použít Operations Management Suite, jak je popsáno v [Operations Management Suite Log Analytics s Linuxem](https://blogs.technet.microsoft.com/hybridcloud/2016/01/28/operations-management-suite-log-analytics-with-linux/).</span><span class="sxs-lookup"><span data-stu-id="81458-131">You can also use Operations Management Suite, as described in [Operations Management Suite Log Analytics with Linux](https://blogs.technet.microsoft.com/hybridcloud/2016/01/28/operations-management-suite-log-analytics-with-linux/).</span></span>

<span data-ttu-id="81458-132">Po dokončení této konfigurace agenta LAD monitoruje protokol příslušné soubory.</span><span class="sxs-lookup"><span data-stu-id="81458-132">After you finish this configuration, the LAD agent monitors the specified log files.</span></span> <span data-ttu-id="81458-133">Vždy, když nový řádek je připojen k souboru, vytvoří záznam syslog, která je odeslána do úložiště, které jste zadali.</span><span class="sxs-lookup"><span data-stu-id="81458-133">Whenever a new line is appended to the file, it creates a syslog entry that is sent to the storage that you specified.</span></span>

## <a name="next-steps"></a><span data-ttu-id="81458-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="81458-134">Next steps</span></span>
<span data-ttu-id="81458-135">Chcete-li podrobněji pochopit, jaké události byste měli zkontrolovat při odstraňování problémů, přečtěte si téma [LTTng dokumentace](http://lttng.org/docs) a [pomocí LAD](../virtual-machines/linux/classic/diagnostic-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="81458-135">To understand in more detail what events you should examine while troubleshooting issues, see [LTTng documentation](http://lttng.org/docs) and [Using LAD](../virtual-machines/linux/classic/diagnostic-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>
