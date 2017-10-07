---
title: aaaOverview Azure Diagnostics | Microsoft Docs
description: "Použití Azure diagnostics pro ladění, měření výkonu, monitorování, analýza provozu v cloudových služeb, virtuální počítače a služby infrastruktury"
services: multiple
documentationcenter: .net
author: rboucher
manager: 
editor: 
ms.assetid: baad40d8-c915-4f93-b486-8b160bf33463
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/18/2017
ms.author: robb
ms.openlocfilehash: 2a03a96a37091894d7ab16120c125116e4bf462a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-diagnostics"></a>Co je Azure Diagnostics
Azure Diagnostics je hello funkce v rámci Azure, která umožňuje hello shromažďování diagnostických dat na nasazené aplikace. Můžete použít rozšíření diagnostiky hello z mnoha různých zdrojů. Aktuálně podporované jsou webové služby Azure Cloud a rolí pracovního procesu, virtuální počítače Azure systémem Microsoft Windows a Service Fabric. Jinými službami Azure mají své vlastní samostatné diagnostiky.

## <a name="data-you-can-collect"></a>Data, která můžete shromáždit
Azure Diagnostics můžete shromáždit hello následující typy dat:

| Zdroj dat | Popis |
| --- | --- |
| Čítače výkonu |Operační systém a vlastní čítače výkonu |
| Protokoly aplikací |Trasování zpráv zapsaných správcem vaší aplikace |
| Protokoly událostí systému Windows |Informace odesílané systému toohello protokolování událostí systému Windows |
| Zdroj události rozhraní .NET |Kód zápisu událostí pomocí rozhraní .NET hello [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) – třída |
| Protokoly služby IIS |Informace o webů služby IIS |
| Manifest na základě trasování událostí pro Windows |Události trasování pro Windows události vygenerované modulem jakýkoli proces |
| Výpisy stavu systému |Informace o stavu hello hello procesu v hello události při selhání aplikace |
| Vlastní chybové protokoly |Protokoly vytvořené aplikace nebo služby |
| Infrastrukturu Azure diagnostické protokoly |Informace o diagnostiky sám sebe |

Hello rozšíření diagnostiky Azure může přenést tato data tooan účtu úložiště Azure nebo odeslat tooservices jako [Application Insights](../application-insights/app-insights-cloudservices.md). Hello dat můžete použít pro ladění a řešení potíží s měření výkonu, sledování využití prostředků, analýza provozu a plánování kapacity a auditování.

## <a name="versioning"></a>Správa verzí
V tématu [historie Správa verzí Azure Diagnostics](azure-diagnostics-versioning-history.md).

## <a name="next-steps"></a>Další kroky
Zvolte služby, která se pokoušíte toocollect diagnostiky a použít hello následující články tooget spuštěna. Pomocí odkazů hello obecné Azure diagnostics pro referenční informace pro konkrétní úlohy.

## <a name="web-apps"></a>Web Apps
Všimněte si, že Web Apps nepoužívají Azure Diagnostics. Najít hello ekvivalentní informace na [webové aplikace](../app-service-web/web-sites-enable-diagnostic-log.md)

## <a name="cloud-services-using-azure-diagnostics"></a>Cloudové služby pomocí Azure Diagnostics
* Pokud používáte Visual Studio, najdete v části [pomocí aplikace Visual Studio tootrace aplikace cloudové služby](../vs-azure-tools-debug-cloud-services-virtual-machines.md) tooget spuštěna. Jinak naleznete v tématu
* [Jak toomonitor Cloud services pomocí Azure Diagnostics](../cloud-services/cloud-services-how-to-monitor.md)
* [Nastavení Azure Diagnostics v aplikaci cloudové služby](../cloud-services/cloud-services-dotnet-diagnostics.md)

Pokročilejší témata naleznete v tématu

* [Pomocí Azure Diagnostics Application Insights pro cloudové služby](../application-insights/app-insights-cloudservices.md)
* [Sledování toku hello aplikace cloudových služeb s Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md)
* [Použití prostředí PowerShell tooset až diagnostiky na cloudové služby](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="virtual-machines-using-azure-diagnostics"></a>Virtuální počítače pomocí Azure Diagnostics
* Pokud používáte Visual Studio, najdete v části [pomocí aplikace Visual Studio tootrace virtuální počítače Azure](../vs-azure-tools-debug-cloud-services-virtual-machines.md) tooget spuštěna. Jinak naleznete v tématu
* [Nastavení Azure Diagnostics na virtuální počítač Azure](../virtual-machines-dotnet-diagnostics.md)

Pokročilejší témata naleznete v tématu

* [Použití prostředí PowerShell tooset až diagnostiky ve virtuálních počítačích Azure](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Vytvoření Windows virtuálního počítače s monitorování a Diagnostika pomocí šablony Azure Resource Manageru](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="service-fabric-using-azure-diagnostics"></a>Service Fabric pomocí diagnostiky Azure
Začínáme v [monitorování aplikace Service Fabric](../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). Mnoho dalších článků diagnostiky Service Fabric jsou k dispozici v navigačním stromu hello na hello zbývající po získání toothis článku.

## <a name="general-azure-diagnostics-articles"></a>Obecné Azure Diagnostics články
* [Schéma konfigurace Azure Diagnostics](https://msdn.microsoft.com/library/azure/mt634524.aspx) – zjistěte, jak toochange hello toocollect souboru schématu a trasy diagnostická data. Všimněte si, že můžete taky soubor schématu hello toochange Visual Studio.
* [Jak je Azure Diagnostics data uložená ve službě Azure Storage](../cloud-services/cloud-services-dotnet-diagnostics-storage.md) -znát názvy hello hello tabulky a umístění, kam je zapisován hello diagnostických dat objektů BLOB.
* Další informace příliš[pomocí čítače výkonu v Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics-performance-counters.md).
* Další informace příliš[trasy Azure diagnostics informace tooApplication statistiky](azure-diagnostics-configure-application-insights.md)
* Pokud máte potíže se spuštěním diagnostiky nebo hledání vaše data v tabulkách Azure Storage, najdete v části [řešení potíží s Azure Diagnostics](azure-diagnostics-troubleshooting.md)
