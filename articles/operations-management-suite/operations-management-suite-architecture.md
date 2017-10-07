---
title: aaaOperations architektura Management Suite (OMS) | Microsoft Docs
description: "Microsoft Operations Management Suite (OMS) je cloudové řešení společnosti Microsoft pro správu IT, které pomáhá se správou a ochranou místních a cloudových infrastruktur.  Tento článek identifikuje hello různé služby, zahrnuté v OMS a poskytuje odkazy tootheir podrobné obsah."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 40e41686-7e35-4d85-bbe8-edbcb295a534
ms.service: operations-management-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/11/2017
ms.author: bwren
ms.openlocfilehash: fa3227aa9c19219009ebe363b7fd2d6565cec59c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="oms-architecture"></a>Architektura OMS
[Operations Management Suite (OMS)](https://azure.microsoft.com/documentation/services/operations-management-suite/) je kolekce cloudových služeb pro správu vašich místních a cloudových prostředí.  Tento článek popisuje hello jinou místní a cloudové komponent OMS a jejich výpočetní Architektura vysoké úrovni cloudu.  Pro každou službu Další podrobnosti vám může naleznete v dokumentaci toohello.

## <a name="log-analytics"></a>Log Analytics
Všechna data shromážděná pomocí [analýzy protokolů](https://azure.microsoft.com/documentation/services/log-analytics/) je uložený v úložišti hello OMS, který je hostován v Azure.  Připojené zdroje vygenerovat data shromážděná do úložiště OMS hello.  V současné době jsou podporované tři typy připojených zdrojů.

* Agent nainstalovaný na [Windows](../log-analytics/log-analytics-windows-agents.md) nebo [Linux](../log-analytics/log-analytics-linux-agents.md) počítače přímo připojené tooOMS.
* Skupina pro správu System Center Operations Manager (SCOM) [připojené tooLog Analytics](../log-analytics/log-analytics-om-agents.md) .  Agenti SCOM pokračovat toocommunicate se servery pro správu, které předávání událostí a tooLog data výkonu Analytics.
* [Účet služby Azure Storage](../log-analytics/log-analytics-azure-storage.md), který shromažďuje data [Diagnostiky Azure](../cloud-services/cloud-services-dotnet-diagnostics.md) z role pracovního procesu, webové role nebo virtuálního počítače v Azure.

Zdroje dat definovat hello data, která shromažďuje analýzy protokolů z připojených zdrojů, včetně protokolů událostí a čítače výkonu.  Řešení přidat funkce tooOMS a lze snadno přidat tooyour prostoru z hello [OMS řešení Galerie](../log-analytics/log-analytics-add-solutions.md).  Některá řešení můžou vyžadovat přímé připojení tooLog Analytics od agentů SCOM, zatímco jiné může vyžadují toobe další agent nainstalován.

Analýzy protokolů má webový portál, můžete použít toomanage OMS prostředků, přidat a nakonfigurovat OMS řešení a zobrazit a analyzovat data v úložišti OMS hello.

![Základní architektura služby Log Analytics](media/operations-management-suite-architecture/log-analytics.png)

## <a name="azure-automation"></a>Azure Automation
[Runbooky služby automatizace Azure](http://azure.microsoft.com/documentation/services/automation) jsou spouštěny v hello cloudu Azure a mají přístup k prostředkům, které jsou v Azure, v jiných cloudových služeb nebo přístupné z veřejného Internetu hello.  Můžete také pomocí procesu [Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md) určit místní počítače ve svém místním datovém centru a umožnit tak runbookům přístup k místním prostředkům.

[Konfigurace DSC](../automation/automation-dsc-overview.md) uložené ve službě Azure Automation může být přímo použité tooAzure virtuálních počítačů.  Další fyzické a virtuální počítače může požádat o konfigurace z načítacího serveru Azure Automation DSC hello.

Automatizace Azure má OMS řešení, které zobrazuje statistiky a propojuje toolaunch hello portál Azure pro žádné operace.

![Základní architektura služby Azure Automation](media/operations-management-suite-architecture/automation.png)

## <a name="azure-backup"></a>Azure Backup
Chráněná data ve službě [Azure Backup](http://azure.microsoft.com/documentation/services/backup) se ukládají do trezoru záloh umístěného v konkrétní geografické oblasti.  Hello data se replikují uvnitř hello stejné oblasti a v závislosti na typu hello trezoru, může být také replikované tooanother oblast pro další redundanci.

Azure Backup obsahuje tři základní scénáře.

* Počítač s Windows a agentem služby Azure Backup.  To vám umožní toobackup soubory a složky v jakémkoli systému Windows server nebo klienta přímo tooyour trezor služby Azure backup.  
* Server System Center Data Protection Manageru (DPM) nebo server Microsoft Azure Backup. Díky tomu můžete tooleverage DPM nebo Microsoft Azure Backup Server toobackup soubory a složky kromě tooapplication úlohy, jako například toolocal úložiště SQL a službu SharePoint a poté replikují tooyour trezor služby Azure backup.
* Rozšíření virtuálního počítače Azure.  To vám umožní toobackup virtuální počítače Azure tooyour trezor služby Azure backup.

Zálohování Azure má OMS řešení, které zobrazuje statistiky a propojuje toolaunch hello portál Azure pro žádné operace.

![Základní architektura služby Azure Backup](media/operations-management-suite-architecture/backup.png)

## <a name="azure-site-recovery"></a>Azure Site Recovery
[Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) orchestruje replikaci, převzetí služeb při selhání a navrácení služeb po obnovení u virtuálních počítačů a fyzických serverů. Replikační data se vyměňují mezi hostitele Hyper-V, hypervisory VMware a fyzické servery v datových centrech primární a sekundární nebo mezi datovým centrem hello a úložiště Azure.  Site Recovery ukládá metadata do trezorů umístěných v konkrétní oblasti Azure. Žádná replikovaná data jsou uložena ve hello služba Site Recovery.

Azure Site Recovery obsahuje tři základní scénáře replikace.

**Replikace virtuálních počítačů Hyper-V**

* Pokud jsou virtuální počítače Hyper-V spravované v cloudech VMM, můžete replikovat tooa sekundárního datového centra nebo tooAzure úložiště.  TooAzure replikace je přes zabezpečené připojení k Internetu.  Replikace tooa sekundárního datového centra je nad hello LAN.
* Pokud virtuální počítače Hyper-V nejsou spravované přes VMM, můžete replikovat jenom tooAzure úložiště.  TooAzure replikace je přes zabezpečené připojení k Internetu.

**Replikace virtuálních počítačů VMware**

* Můžete provádět replikaci VMware virtuální počítače tooa sekundárního datacentra systémem VMware nebo tooAzure úložiště.  Replikace tooAzure situace může nastat, přes síť site-to-site VPN nebo Azure ExpressRoute nebo přes zabezpečené internetové připojení. Sekundární datacentrum tooa replikace probíhá přes hello InMage Scout datový kanál.

**Replikace fyzických serverů s Windows nebo Linuxem** 

* Můžete replikovat fyzické servery tooa sekundárního datového centra nebo tooAzure úložiště. Replikace tooAzure situace může nastat, přes síť site-to-site VPN nebo Azure ExpressRoute nebo přes zabezpečené internetové připojení. Sekundární datacentrum tooa replikace probíhá přes hello InMage Scout datový kanál.  Azure Site Recovery má OMS řešení, která zobrazuje statistikami, ale hello portálu Azure, musíte použít pro žádné operace.

![Základní architektura služby Azure Site Recovery](media/operations-management-suite-architecture/site-recovery.png)

## <a name="next-steps"></a>Další kroky
* Další informace o [Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics).
* Další informace o [Azure Automation](https://azure.microsoft.com/documentation/services/automation).
* Další informace o [Azure Backup](http://azure.microsoft.com/documentation/services/backup).
* Další informace o [Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery).

