---
title: "protokoly aaaCollect pomocí diagnostiky Azure Linux | Microsoft Docs"
description: "Tento článek popisuje, jak tooset si Azure Diagnostics toocollect protokolů z clusteru Service Fabric Linux běží v Azure."
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
ms.openlocfilehash: f61172876e744ea3e361f9ae513254239d6ba27f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="collect-logs-by-using-azure-diagnostics"></a>Shromažďování protokolů pomocí Azure Diagnostics
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
> * [Linux](service-fabric-diagnostics-how-to-setup-lad.md)
> 
> 

Když používáte cluster služby Azure Service Fabric, je vhodné toocollect hello protokoly ze všech uzlů hello v centrálním umístění. S protokoly hello v centrálním umístění je snadno tooanalyze a řešení problémů, zda jsou ve vaší služby, aplikace nebo samotného clusteru hello. Jedním ze způsobů tooupload a shromáždit protokoly je toouse hello Azure Diagnostics rozšíření, které nahrávání protokolů tooAzure úložiště, Azure Application Insights nebo Azure Event Hubs. Můžete také číst hello události z úložiště nebo Event Hubs a umístěte je v produktu, jako [analýzy protokolů](../log-analytics/log-analytics-service-fabric.md) nebo jiné řešení pro analýzu protokolu. [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) se dodává s komplexní protokolu vyhledávání a analýzy služby předdefinované.

## <a name="log-sources-that-you-might-want-toocollect"></a>Můžete chtít toocollect zdroje protokolu
* **Protokoly Service Fabric**: vygenerované z platformy hello prostřednictvím [LTTng](http://lttng.org) a odesláno tooyour účet úložiště. Protokoly mohou být provozní události nebo vysílá události modulu runtime, které hello platformy. Tyto protokoly se ukládají v umístění hello Určuje, že hello manifestu clusteru. (tooget hello podrobnosti o účtu úložiště, vyhledá hello značku **AzureTableWinFabETWQueryable** a vyhledejte **StoreConnectionString**.)
* **Události aplikace**: vygenerované z kódu vaší služby. Můžete použít řešení protokolování, který zapíše soubory založený na textu protokolu – například LTTng. Další informace naleznete v dokumentaci k LTTng hello na trasování vaší aplikace.  

## <a name="deploy-hello-diagnostics-extension"></a>Nasazení rozšíření diagnostiky hello
Hello prvním krokem při shromažďování protokolů je rozšíření diagnostiky hello toodeploy na každém hello virtuálních počítačů v clusteru Service Fabric hello. Hello rozšíření diagnostiky shromažďuje protokoly na každý virtuální počítač a odesílá je toohello účet úložiště, který určíte. kroky Hello lišit v závislosti na tom, zda používáte hello portál Azure nebo Azure Resource Manager.

nastavit toodeploy hello diagnostiky rozšíření toohello virtuálních počítačů v clusteru hello jako součást vytváření clusteru **diagnostiky** příliš**na**. Po vytvoření clusteru hello, nelze toto nastavení změnit pomocí portálu hello.

Potom nakonfigurujte soubory hello toocollect Linux Azure Diagnostics (LAD) a umístěte je do vašeho účtu úložiště. Tento postup je podrobně jako scénář 3 ("odeslání souborů protokolu") v článku hello [toomonitor pomocí LAD a diagnostikovat virtuální počítače s Linuxem](../virtual-machines/linux/classic/diagnostic-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json). Následující tento proces získá přístup toohello trasování. Můžete nahrát hello trasování tooa vizualizér podle svého výběru.

Můžete také nasadit hello rozšíření diagnostiky pomocí Azure Resource Manager. Hello proces je podobný pro systém Windows a Linux a je popsané u clusterů systému Windows v [jak toocollect protokoly s Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md).

Můžete také použít Operations Management Suite, jak je popsáno v [Operations Management Suite Log Analytics s Linuxem](https://blogs.technet.microsoft.com/hybridcloud/2016/01/28/operations-management-suite-log-analytics-with-linux/).

Po dokončení této konfigurace hello LAD agenta monitorování hello zadané soubory protokolu. Vždy, když je nový řádek souboru připojením toohello, vytvoří syslog položku, která je odeslané toohello úložiště, který jste zadali.

## <a name="next-steps"></a>Další kroky
toounderstand podrobněji, jaké události byste měli zkontrolovat při řešení potíží, najdete v části [LTTng dokumentace](http://lttng.org/docs) a [pomocí LAD](../virtual-machines/linux/classic/diagnostic-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

