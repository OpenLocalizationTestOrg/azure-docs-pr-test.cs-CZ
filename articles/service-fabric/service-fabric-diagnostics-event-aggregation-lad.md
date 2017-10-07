---
title: "aaaAzure agregace událostí služby Fabric pomocí diagnostiky Azure Linux | Microsoft Docs"
description: "Další informace o agregaci a shromažďování událostí pomocí LAD pro monitorování a Diagnostika Azure Service Fabric clusterů."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/17/2017
ms.author: dekapur
ms.openlocfilehash: aefa869219a0dd219e01e6574816fe3ce47fe472
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="event-aggregation-and-collection-using-linux-azure-diagnostics"></a>Seskupení událostí a kolekce pomocí diagnostiky Azure Linux
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-event-aggregation-wad.md)
> * [Linux](service-fabric-diagnostics-event-aggregation-lad.md)
>
>

Když používáte cluster služby Azure Service Fabric, je vhodné toocollect hello protokoly ze všech uzlů hello v centrálním umístění. S protokoly hello v centrálním umístění, vám pomáhají analyzovat a vyřešit problémy v clusteru nebo problémy v hello aplikací a služeb spuštěných v daném clusteru.

Jedním ze způsobů tooupload a shromáždit protokoly je toouse hello Linux Azure Diagnostics (LAD) přípony, který odešle tooAzure protokoly úložiště a také obsahuje hello možnost toosend protokoly tooAzure Application Insights nebo Event Hubs. Můžete také použít externího procesu tooread hello události z úložiště a umístit je o analýzy platformy produkt, například [analýzy protokolů OMS](../log-analytics/log-analytics-service-fabric.md) nebo jiné řešení pro analýzu protokolu.

## <a name="log-and-event-sources"></a>Protokol a událostí zdroje

### <a name="service-fabric-platform-events"></a>Události platformy Service Fabric
Service Fabric vysílá několik protokolů se na pole prostřednictvím [LTTng](http://lttng.org), včetně provozní události nebo modul runtime události. Tyto protokoly jsou uloženy v umístění hello hello určuje šablony správce prostředků clusteru. tooget nebo nastavit hello podrobnosti o účtu úložiště, vyhledá hello značku **AzureTableWinFabETWQueryable** a vyhledejte **StoreConnectionString**.

### <a name="application-events"></a>Události aplikace
 Události vygenerované z vašich aplikací a služeb kódu podle specifikace můžete při instrumentaci váš software. Můžete použít řešení protokolování, který zapíše soubory založený na textu protokolu – například LTTng. Další informace naleznete v dokumentaci k LTTng hello na trasování vaší aplikace.

[Monitorování a Diagnostika služby v instalačním programu místním počítači vývoj](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

## <a name="deploy-hello-diagnostics-extension"></a>Nasazení rozšíření diagnostiky hello
Hello prvním krokem při shromažďování protokolů je rozšíření diagnostiky hello toodeploy na každém hello virtuálních počítačů v clusteru Service Fabric hello. Hello rozšíření diagnostiky shromažďuje protokoly na každý virtuální počítač a odesílá je toohello účet úložiště, který určíte. kroky Hello lišit v závislosti na tom, zda používáte hello portál Azure nebo Azure Resource Manager.

nastavit toodeploy hello diagnostiky rozšíření toohello virtuálních počítačů v clusteru hello jako součást vytváření clusteru **diagnostiky** příliš**na**. Po vytvoření clusteru hello, nelze toto nastavení změnit pomocí portálu hello.

Potom nakonfigurujte soubory hello toocollect Linux Azure Diagnostics (LAD) a umístěte je do vašeho účtu úložiště. Tento postup je podrobně jako scénář 3 ("odeslání souborů protokolu") v článku hello [toomonitor pomocí LAD a diagnostikovat virtuální počítače s Linuxem](../virtual-machines/linux/classic/diagnostic-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json). Následující tento proces získá přístup toohello trasování. Můžete nahrát hello trasování tooa vizualizér podle svého výběru.

Můžete také nasadit hello rozšíření diagnostiky pomocí Azure Resource Manager. Hello proces je podobný pro systém Windows a Linux a je popsané u clusterů systému Windows v [jak toocollect protokoly s Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md).

Můžete také použít Operations Management Suite, jak je popsáno v [Operations Management Suite Log Analytics s Linuxem](https://blogs.technet.microsoft.com/hybridcloud/2016/01/28/operations-management-suite-log-analytics-with-linux/).

Po dokončení této konfigurace hello LAD agenta monitorování hello zadané soubory protokolu. Vždy, když je nový řádek souboru připojením toohello, vytvoří syslog položku, která je odeslané toohello úložiště, který jste zadali.

## <a name="next-steps"></a>Další kroky

toounderstand podrobněji, jaké události byste měli zkontrolovat při řešení potíží, najdete v části [LTTng dokumentace](http://lttng.org/docs) a [pomocí LAD](../virtual-machines/linux/classic/diagnostic-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).
