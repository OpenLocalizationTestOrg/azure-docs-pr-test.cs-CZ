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
# <a name="collect-logs-by-using-azure-diagnostics"></a>Shromažďování protokolů pomocí Azure Diagnostics
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
> * [Linux](service-fabric-diagnostics-how-to-setup-lad.md)
> 
> 

Když používáte cluster služby Azure Service Fabric, je vhodné shromažďovat protokoly ze všech uzlů v centrálním umístění. S protokoly v centrálním umístění snadno k analýze a řešení problémů, zda jsou ve vaší služby, aplikace nebo samotného clusteru. Jeden způsob, jak nahrát a shromažďování protokolů je použití rozšíření diagnostiky Azure, který odešle protokoly do služby Azure Storage, Azure Application Insights nebo Azure Event Hubs. Můžete také číst události z úložiště nebo Event Hubs a umístěte je v produktu, jako [analýzy protokolů](../log-analytics/log-analytics-service-fabric.md) nebo jiné řešení pro analýzu protokolu. [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) se dodává s komplexní protokolu vyhledávání a analýzy služby předdefinované.

## <a name="log-sources-that-you-might-want-to-collect"></a>Protokol zdroje, které můžete chtít shromažďovat
* **Protokoly Service Fabric**: vygenerované z platformy prostřednictvím [LTTng](http://lttng.org) a nahrané do účtu úložiště. Protokoly mohou být provozní události nebo modul runtime události, které vysílá platformu. Tyto protokoly se ukládají do umístění, které určuje manifestu clusteru. (Chcete-li získat podrobnosti o účtu úložiště, vyhledejte značky **AzureTableWinFabETWQueryable** a vyhledejte **StoreConnectionString**.)
* **Události aplikace**: vygenerované z kódu vaší služby. Můžete použít řešení protokolování, který zapíše soubory založený na textu protokolu – například LTTng. Další informace najdete v dokumentaci LTTng na trasování vaší aplikace.  

## <a name="deploy-the-diagnostics-extension"></a>Nasazení rozšíření diagnostiky
Prvním krokem při shromažďování protokolů je k nasazení rozšíření diagnostiky na všech virtuálních počítačích v clusteru Service Fabric. Rozšíření diagnostiky shromažďuje protokoly na každý virtuální počítač a odesílá je do účtu úložiště, který určíte. Kroky lišit v závislosti na tom, zda používáte portál Azure nebo Azure Resource Manager.

Chcete-li nasadit rozšíření diagnostiky pro virtuální počítače v clusteru jako součást vytváření clusteru, nastavte **diagnostiky** k **na**. Po vytvoření clusteru, nelze toto nastavení změnit pomocí portálu.

Potom nakonfigurujte Linux Azure Diagnostics (LAD) pro shromažďování souborů a umístěte je do vašeho účtu úložiště. Tento postup je podrobně jako scénář 3 ("odeslání souborů protokolu") v článku [LAD pomocí monitorování a Diagnostika virtuální počítače s Linuxem](../virtual-machines/linux/classic/diagnostic-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json). Provedení tohoto postupu získá přístup k trasování. Trasování můžete uložit do vizualizéru podle svého výběru.

Můžete také nasadit rozšíření diagnostiky pomocí Azure Resource Manager. Proces je podobný pro systém Windows a Linux a je popsané u clusterů systému Windows v [postup shromažďování protokolů pomocí Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md).

Můžete také použít Operations Management Suite, jak je popsáno v [Operations Management Suite Log Analytics s Linuxem](https://blogs.technet.microsoft.com/hybridcloud/2016/01/28/operations-management-suite-log-analytics-with-linux/).

Po dokončení této konfigurace agenta LAD monitoruje protokol příslušné soubory. Vždy, když nový řádek je připojen k souboru, vytvoří záznam syslog, která je odeslána do úložiště, které jste zadali.

## <a name="next-steps"></a>Další kroky
Chcete-li podrobněji pochopit, jaké události byste měli zkontrolovat při odstraňování problémů, přečtěte si téma [LTTng dokumentace](http://lttng.org/docs) a [pomocí LAD](../virtual-machines/linux/classic/diagnostic-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

