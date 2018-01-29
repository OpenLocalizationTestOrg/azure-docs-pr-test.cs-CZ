---
title: "Stream monitorování data do centra událostí Azure | Microsoft Docs"
description: "Naučte se všechna vaše Azure monitorování data do centra událostí k získání dat do partnerský server SIEM nebo nástroj pro analýzu datového proudu."
author: johnkemnetz
manager: robb
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/11/2018
ms.author: johnkem
ms.openlocfilehash: b2813035b4665a36b475e791965d395b84ddb3f1
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/12/2018
---
# <a name="stream-azure-monitoring-data-to-an-event-hub-for-consumption-by-an-external-tool"></a>Monitorování data do centra událostí pro používání pomocí externího nástroje Azure datového proudu

Monitorování Azure poskytuje jeden kanál pro získání přístupu ke všem dat monitorování od prostředí Azure, umožňuje snadno nastavit partnera SIEM a nástroje k spotřebě těchto dat monitorování. Tento článek vás provede nastavení různých vrstev dat z prostředí Azure k odeslání do jednoho Event Hubs oboru názvů nebo událost rozbočovač, kde ji můžete shromáždit pomocí externího nástroje.

## <a name="what-data-can-i-send-into-an-event-hub"></a>Jaká data může odesílat do centra událostí? 

V prostředí Azure existuje několik "vrstvy" monitorování dat a způsob přístupu k datům z jednotlivých úrovní se mírně liší. Tyto úrovně obvykle, může být označen jako:

- **Monitorování dat aplikací:** Data o výkonu a funkce kód napsali a jsou spuštěné v Azure. Příklady monitorování dat aplikací: trasování výkonu, protokoly aplikací a telemetrie uživatele. Monitorování data aplikace se obvykle shromažďují v jednom z následujících způsobů:
  - Podle instrumentace kódu pomocí sady SDK, jako [Application Insights SDK](../application-insights/app-insights-overview.md).
  - Spuštěním agenta monitorování, která naslouchá pro novou aplikaci protokoly na počítači spuštění aplikace, například [agenta pro diagnostiku systému Windows Azure](./azure-diagnostics.md) nebo [agenta pro diagnostiku Azure Linux](../virtual-machines/linux/diagnostic-extension.md).
- **Hostovaného operačního systému, dat monitorování:** Data o operačním systému, na kterém je aplikace spuštěna. Příklady data monitorování hostovaného operačního systému by Linux syslog nebo události systému Windows. Chcete-li shromažďovat data tohoto typu, je potřeba nainstalovat agenta, jako [agenta pro diagnostiku systému Windows Azure](./azure-diagnostics.md) nebo [agenta pro diagnostiku Azure Linux](../virtual-machines/linux/diagnostic-extension.md).
- **Data monitorování prostředků Azure:** Data o operaci prostředek služby Azure. Pro některé typy prostředků Azure, jako jsou virtuální počítače je hostovaný operační systém a aplikace pro monitorování v rámci služby Azure. Pro další prostředky Azure, například skupin zabezpečení sítě zdroj dat monitorování je nejvyšší úroveň dat, které jsou k dispozici (protože není žádný hostovaný operační systém nebo aplikace běžící v těchto zdrojích informací). Tato data se můžou shromažďovat pomocí [nastavení diagnostiky pro prostředek](./monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings).
- **Data monitorování platformy Azure:** Data o operace a Správa předplatného Azure nebo klienta, jakož i data týkající se stavu a operaci Azure sám sebe. [Protokol aktivit](./monitoring-overview-activity-logs.md), včetně data o stavu služby a služby Active Directory audity jsou příklady platformy dat monitorování. Tato data se můžou shromažďovat pomocí také nastavení pro diagnostiku.

Data z libovolné úrovně můžete odeslat do centra událostí, kde mohou být vyžádány do nástroje a partnera. Další části popisují, jak můžete nakonfigurovat dat z jednotlivých úrovní tok dat do centra událostí. Postup předpokládá, že už máte prostředky v této vrstvě ke sledování.

Než začnete, budete muset [vytvoření služby Event Hubs obor názvů a event hub](../event-hubs/event-hubs-create.md). Toto centrum obor názvů a událostí je cílem pro všechna vaše data monitorování.

## <a name="how-do-i-set-up-azure-platform-monitoring-data-to-be-streamed-to-an-event-hub"></a>Jak nastavit platformy Azure dat monitorování tok dat do centra událostí?

Data monitorování platformy Azure pochází z dva hlavní zdroje:
1. [Protokol činnosti Azure](./monitoring-overview-activity-logs.md), který obsahuje vytvořením, aktualizovat a odstranit ze Správce prostředků, změny v nástroji operations [stavu služby Azure](../service-health/service-health-overview.md) , může mít vliv na prostředky v rámci vašeho předplatného, [stav prostředku](../service-health/resource-health-overview.md) přechodů mezi stavy a několik dalších typů událostí na úrovni předplatného. [Tento článek podrobně všechny kategorie události, které se zobrazují v protokolu aktivit Azure](./monitoring-activity-log-schema.md).
2. [Generování sestav Azure Active Directory](../active-directory/active-directory-reporting-azure-portal.md), který obsahuje historii přihlašovací aktivitu a audit záznam změn provedených v rámci konkrétní klienta. Ještě není možné k datům datového proudu Azure Active Directory do centra událostí.

### <a name="stream-azure-activity-log-data-into-an-event-hub"></a>Data protokolu aktivita Azure datový proud do centra událostí

K odesílání dat z Azure aktivity protokolu do oboru názvů Event Hubs, můžete nastavit profil protokolu na vaše předplatné. [Postupujte podle této příručky](./monitoring-stream-activity-logs-event-hubs.md) nastavení profilu protokolu na vaše předplatné. K tomu po každé předplatné, které chcete sledovat.

> [!TIP]
> Profil protokolu aktuálně jenom vám umožní vybrat na obor názvů služby Event Hubs, ve kterém se vytvoří centra událostí s název "insights provozní protokoly." Ještě není možné zadat vlastní název centra událostí v profilu protokolu.

## <a name="how-do-i-set-up-azure-resource-monitoring-data-to-be-streamed-to-an-event-hub"></a>Jak nastavit prostředků Azure dat monitorování tok dat do centra událostí?

Prostředky Azure emitování dva druhy dat monitorování:
1. [Diagnostické protokoly prostředků](./monitoring-overview-of-diagnostic-logs.md)
2. [Metriky](monitoring-overview-metrics.md)

Oba typy dat se posílají do centra událostí pomocí diagnostiky nastavení prostředků. [Postupujte podle této příručky](./monitoring-stream-diagnostic-logs-to-event-hubs.md) nastavit diagnostické nastavení prostředků na určitém prostředku. Nastavení prostředků diagnostiky na každý prostředek, ze kterého chcete shromažďovat protokoly.

> [!TIP]
> Zásad Azure vám pomůže zajistit, že každý prostředek v rámci určitého oboru vždy nastavená se nastavení diagnostiky [pomocí DeployIfNotExists účinek v pravidlo zásad](../azure-policy/policy-definition.md#policy-rule). Dnes DeployIfNotExists je podporována pouze na integrovaných zásad.

## <a name="how-do-i-set-up-guest-os-monitoring-data-to-be-streamed-to-an-event-hub"></a>Jak nastavit data monitorování operačního systému hosta tok dat do centra událostí?

Musíte nainstalovat agenta hosta data monitorování operačního systému odeslat do centra událostí. Pro Windows nebo Linux zadejte data, která chcete k odeslání do centra událostí, jakož i centra událostí, ke kterému data by měly být odeslány v konfiguračním souboru a že konfigurační soubor předat agenta spuštěného virtuálního počítače.

### <a name="stream-linux-data-to-an-event-hub"></a>Datový proud Linux data do centra událostí

[Diagnostiky Azure Linux agent](../virtual-machines/linux/diagnostic-extension.md) slouží k odesílání dat monitorování od počítač s Linuxem do centra událostí. K tomu přidáním centra událostí jako jímku ve vašem LAD nastavení konfigurace chráněný soubor JSON. [Najdete v tomto článku Další informace o přidání jímek centra událostí do vaší diagnostiky Azure Linux agent](../virtual-machines/linux/diagnostic-extension.md#protected-settings).

> [!NOTE]
> Nelze nastavit vysílání datového proudu data monitorování hostovaného operačního systému do centra událostí na portálu. Místo toho je nutné ručně upravit konfigurační soubor.

### <a name="stream-windows-data-to-an-event-hub"></a>Datový proud dat systému Windows do centra událostí

[Agenta pro Windows Azure diagnostiku](./azure-diagnostics.md) slouží k odesílání dat monitorování od počítače s Windows do centra událostí. To lze proveďte tak, že přidáte centra událostí jako jímku v části vaší privateConfig WAD konfiguračního souboru. [Najdete v článku na další informace o přidávání rozbočovače jímek událostí pro Windows Azure Diagnostic agenta](./azure-diagnostics-streaming-event-hubs.md).

> [!NOTE]
> Nelze nastavit vysílání datového proudu data monitorování hostovaného operačního systému do centra událostí na portálu. Místo toho je nutné ručně upravit konfigurační soubor.

## <a name="how-do-i-set-up-application-monitoring-data-to-be-streamed-to-event-hub"></a>Jak nastavím monitorování aplikací datový tok dat do centra událostí?

Aplikace data monitorování vyžaduje, že kódu je instrumentovány pomocí sady SDK, není k dispozici pro obecné účely řešení pro monitorování data do centra událostí v Azure směrování aplikací. Ale [Azure Application Insights](../application-insights/app-insights-overview.md) jedna služba, která lze použít ke shromažďování dat Azure úrovni aplikace. Pokud používáte Application Insights, proudy dat monitorování do centra událostí následujícím způsobem:

1. [Nastavit průběžné export](../application-insights/app-insights-export-telemetry.md) dat Application Insights na účet úložiště.

2. Nastavení aplikace logiky aktivovaného časovačem, [získává data z úložiště objektů blob](../connectors/connectors-create-api-azureblobstorage.md#use-an-action) a [sami jako zprávy do centra událostí](../connectors/connectors-create-api-azure-event-hubs.md#send-events-to-your-event-hub-from-your-logic-app).

## <a name="what-can-i-do-with-the-monitoring-data-being-sent-to-my-event-hub"></a>Jak se data sledování odesílána Moje centra událostí?

Směrování monitorování data do centra událostí s monitorováním Azure umožňuje snadnou integraci s partnera SIEM a nástrojů pro monitorování. Většina nástrojů vyžadují určitá oprávnění k předplatnému Azure číst data z centra událostí a připojovací řetězec centra událostí. Tady je seznam nástrojů se službou Azure monitorování integrace doplňovat:

* **IBM QRadar** -Microsoft Azure DSM a Microsoft Azure Event Hub protokolu jsou k dispozici ke stažení [webu podpory IBM](http://www.ibm.com/support). Můžete [Další informace o integraci s Azure zde](https://www.ibm.com/support/knowledgecenter/SS42VS_DSM/c_dsm_guide_microsoft_azure_overview.html?cp=SS42VS_7.3.0).
* **Splunk** - [rozšíření monitorování Azure pro Splunk](https://splunkbase.splunk.com/app/3534/) je k dispozici v Splunkbase a open source projektu. [Dokumentace je zde](https://github.com/Microsoft/AzureMonitorAddonForSplunk/wiki/Azure-Monitor-Addon-For-Splunk).
* **SumoLogic** – pokyny pro nastavení SumoLogic ke zpracování dat z centra událostí jsou [zde k dispozici](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure-Audit/02Collect-Logs-for-Azure-Audit-from-Event-Hub)

## <a name="next-steps"></a>Další kroky
* [Archiv protokol aktivit na účet úložiště](monitoring-archive-activity-log.md)
* [Přečtěte si přehled protokol činnosti Azure](monitoring-overview-activity-logs.md)
* [Nastavit výstrahy na základě aktivity protokolu události](insights-auditlog-to-webhook-email.md)

