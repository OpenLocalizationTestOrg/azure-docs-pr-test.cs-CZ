---
title: aaaOverview metriky v Microsoft Azure | Microsoft Docs
description: "Přehled metriky a jejich použití v Microsoft Azure"
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 405ec51c-0946-4ec9-b535-60f65c4a5bd1
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/02/2017
ms.author: johnkem
ms.openlocfilehash: 2b97f51e0554dae95a929241ae1f0e25e5c215ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-metrics-in-microsoft-azure"></a>Přehled metriky v Microsoft Azure
Tento článek popisuje, co metriky jsou ve službě Microsoft Azure jejich výhody a jak toostart jejich používání.  

## <a name="what-are-metrics"></a>Jaké jsou metriky?
Azure monitorování umožňuje tooconsume telemetrie toogain přehled hello výkonu a stavu úlohy v Azure. Hello nejdůležitější typ Azure telemetrická data je hello metriky (také nazývané čítače výkonu) vysílaných prostředků nejvíce Azure. Monitorování Azure poskytuje několik způsobů tooconfigure a využívat tyto metriky pro monitorování a řešení potíží.

## <a name="what-can-you-do-with-metrics"></a>Co se děje s metriky?
Metriky jsou cenné zdroj telemetrie a umožňují toodo hello následující úlohy:

* **Sledování výkonu hello** vaše prostředku (například počítač, web nebo logiku aplikace) vykreslení jeho metriky na portálu graf a Připnutí tento řídicí panel tooa grafu.
* **Upozorňování problému** ovlivňuje když metriky překračuje prahovou hodnotu určité text hello výkonu prostředku.
* **Konfigurovat automatické akce**, jako je automatické škálování prostředku nebo když metriky překračuje určité prahovou hodnotu, která iniciovala sady runbook.
* **Provádět pokročilé analýzy** nebo generování sestav na trendy výkonu a využití vaší prostředku.
* **Archiv** hello výkon nebo stav historii prostředku **pro dodržování předpisů nebo auditování** účely.

## <a name="what-are-hello-characteristics-of-metrics"></a>Jaké jsou vlastnosti hello metrik?
Metriky mít hello následující vlastnosti:

* Mají všechny metriky **jedné minuty frekvence**. Zobrazí hodnota metriky každou minutu z prostředku, která poskytuje téměř v reálném čase přehled hello stav a stav prostředku.
* Metriky **k dispozici okamžitě**. Není nutné tooopt v nebo nastavit další diagnostiky.
* Dostanete **30 dní od historie** pro jednotlivé metriky. Můžete rychle zobrazit hello poslední a měsíční trendy v hello výkon nebo stav prostředku.

Rovněž můžete:

* Konfigurovat metriku **výstraha pravidla, které odešle oznámení nebo trvá automatizované akce** když metrika hello protne hello prahovou hodnotu, která jste nastavili. Při automatickém škálování je zvláštní automatizované akce, které povolí tooscale na příchozí požadavky vaší prostředků toomeet nebo načte na vašem webu nebo výpočetních prostředků. Můžete nakonfigurovat zadané nastavení škálování tooscale pravidlo příchozí nebo odchozí podle metriky při překročení prahové hodnoty.

* **Trasy** všechny metriky Application Insights nebo analýzy protokolů (OMS) tooenable rychlé analýzy, vyhledávání a vlastní výstrahy na metriky data z vašich prostředků. Můžete také datového proudu metriky tooan centra událostí, umožňuje toothen směrovat je tooAzure Stream Analytics nebo toocustom aplikací pro analýzu skoro v reálném čase. Nastavíte centra událostí streamování pomocí nastavení pro diagnostiku.

* **Archivovat metriky toostorage** pro uchování delší nebo je používat pro offline generování sestav. Při konfiguraci nastavení diagnostiky pro prostředek, můžete směrovat vaše metriky tooAzure úložiště objektů Blob.

* Snadno zjistit, přístup, a **zobrazit všechny metriky** prostřednictvím portálu Azure při vykreslení hello metriky na graf a vyberte prostředek hello.

* **Využívat** hello metriky prostřednictvím hello nové Azure monitorování REST API.

* **Dotaz** metriky pomocí rutin prostředí PowerShell text hello nebo hello REST API napříč platformami.

  ![Směrování metriky v Azure monitorování](./media/monitoring-overview-metrics/Metrics_Overview_v4.png)

## <a name="access-metrics-via-hello-portal"></a>Metriky přístup přes portál hello
Toto je rychlý návod, jak hello toocreate metriky grafu pomocí portálu Azure.

### <a name="tooview-metrics-after-creating-a-resource"></a>metriky tooview po vytvoření prostředku
1. Otevřete hello portálu Azure.
2. Vytvoření webu k službě Azure App Service.
3. Po vytvoření webu, přejděte toohello **přehled** okno hello webu.
4. Můžete zobrazit nové metriky jako **monitorování** dlaždici. Potom můžete upravit hello dlaždice a vybrat další metriky.

   ![Metriky pro prostředek v Azure monitorování](./media/monitoring-overview-metrics/MetricsOverview1.png)

### <a name="tooaccess-all-metrics-in-a-single-place"></a>tooaccess všechny metriky na jednom místě
1. Otevřete hello portálu Azure.
2. Přejděte toohello nové **monitorování** kartu a pak a vyberte možnost hello **metriky** možnost pod ním.
3. Vyberte z rozevíracího seznamu hello vaše předplatné, skupinu prostředků a hello název prostředku hello.
4. Zobrazit seznam dostupných metriky hello. Pak vyberte metriku hello zájem a jeho vykreslení.
5. Řídicí panel toohello ho můžete připojit kliknutím hello pin v pravém horním rohu hello.

   ![Přístup k všechny metriky na jednom místě v Azure monitorování](./media/monitoring-overview-metrics/MetricsOverview2.png)

> [!NOTE]
> Metriky na úrovni hostitele mají přístup z virtuálních počítačů (založené na Azure Resource Manager) a sadách škálování virtuálních počítačů bez jakékoli další diagnostiky. Tyto nové metriky na úrovni hostitele jsou k dispozici pro Windows a Linux instance. Tyto metriky nejsou toobe zaměňovat s hello úrovni operačního systému hosta metriky, které máte přístup toowhen, který zapnete Azure Diagnostics virtuálních počítačů nebo sady škálování virtuálního počítače. toolearn Další informace o konfiguraci diagnostiky, najdete v části [co je Microsoft Azure Diagnostics](../azure-diagnostics.md).
>
>

## <a name="access-metrics-via-hello-rest-api"></a>Metriky přístup prostřednictvím hello REST API
Azure metriky je přístupný prostřednictvím rozhraní API Správce Azure monitorování hello. Existují dvě rozhraní API, které vám pomohou zjistit a přístup k metriky:

* Použití hello [Azure monitorování metrika definice rozhraní API REST](https://msdn.microsoft.com/library/mt743621.aspx) tooaccess hello seznam metriky, které jsou k dispozici pro službu.
* Použití hello [REST API služby Azure monitorování metriky](https://msdn.microsoft.com/library/mt743622.aspx) tooaccess hello metriky skutečná data.

> [!NOTE]
> Tento článek se zabývá hello metriky prostřednictvím hello [nové rozhraní API pro metriky](https://msdn.microsoft.com/library/dn931930.aspx) pro prostředky Azure. verze rozhraní API Hello hello nové metriky definice rozhraní API je 2016-03-01 a hello verze pro metriky rozhraní API je 2016-09-01. starší verze definice metrik Hello a metriky lze přistupovat pomocí rozhraní API verze 2014-04-01 hello.
>
>

Podrobnější návod pomocí hello Azure monitorování REST API najdete v tématu [REST API služby Azure monitorování návod](monitoring-rest-api-walkthrough.md).

## <a name="export-metrics"></a>Export metriky
Můžete přejít toohello **nastavení diagnostiky** okno pod hello **monitorování** kartě a zobrazit možnosti exportu hello metrik. Můžete vybrat metriky (a diagnostických protokolů) toobe směrovány tooBlob úložiště, tooAzure Event Hubs nebo tooOMS pro použití případů, které byly uvedený dříve v tomto článku.

 ![Možnosti exportu metrik, které v Azure monitorování](./media/monitoring-overview-metrics/MetricsOverview3.png)

To můžete nakonfigurovat pomocí šablony Resource Manageru, [prostředí PowerShell](insights-powershell-samples.md), [rozhraní příkazového řádku Azure](insights-cli-samples.md), nebo [rozhraní REST API](https://msdn.microsoft.com/library/dn931943.aspx).

## <a name="take-action-on-metrics"></a>Provést akci pro metriky
oznámení tooreceive nebo proveďte automatizované akce na data metriky, můžete nakonfigurovat nastavení automatického škálování nebo pravidla výstrah.

### <a name="configure-alert-rules"></a>Konfigurace pravidla výstrah
Pravidla výstrah můžete nakonfigurovat na metriky. Tato pravidla výstrahy můžete zkontrolovat, pokud metriky překročila určitou mez. Potom můžete upozornění e-mailem nebo fire webhooku, kterou lze použít toorun všech vlastních skriptů. Můžete také použít integrace produktu třetí strany tooconfigure webhooku hello.

 ![Metriky a pravidla výstrah v monitorování Azure](./media/monitoring-overview-metrics/MetricsOverview4.png)

### <a name="autoscale-your-azure-resources"></a>Škálování vašeho Azure prostředky
Některé prostředky Azure podporují hello škálování se nebo ve více instancí toohandle úlohy. Škálování platí tooApp služby (webové aplikace), sady škálování virtuálního počítače a classic Azure Cloud Services. Škálování pravidla tooscale nebo v můžete nakonfigurovat, když metrika, který má dopad na vaše úloha překračuje prahovou hodnotu, která zadáte. Další informace najdete v tématu [přehled automatické škálování](monitoring-overview-autoscale.md).

 ![Metriky a škálování v Azure monitorování](./media/monitoring-overview-metrics/MetricsOverview5.png)

## <a name="learn-about-supported-services-and-metrics"></a>Další informace o podporovaných služeb a metriky
Monitorování Azure je nová infrastruktura metriky. Podporuje následující služby Azure v hello Azure portal a hello novou verzi hello rozhraní API služby Azure monitorování hello:

* Virtuální počítače (založené na správci prostředků Azure)
* Škálovací sady virtuálních počítačů
* Batch
* Obor názvů centra událostí
* Oboru názvů Service Bus (pouze premium SKU)
* Databáze SQL (verze 12)
* Fond elastické SQL
* Websites
* Webové serverové farmy
* Logic Apps
* Centra IoT
* Redis Cache
* Sítě: Application Gateway
* Search

Můžete zobrazit podrobný seznam všech služeb hello podporována a jejich metriky [Azure monitorování metriky – podporované metriky na typ prostředku](monitoring-supported-metrics.md).

## <a name="next-steps"></a>Další kroky
Najdete v odkazech toohello v rámci celého tohoto článku. Kromě toho další informace o:  

* [Běžné metriky pro automatické škálování](insights-autoscale-common-metrics.md)
* [Jak toocreate pravidla výstrah](insights-alerts-portal.md)
* [Analýza protokolů z úložiště Azure s analýzy protokolů](../log-analytics/log-analytics-azure-storage.md)
