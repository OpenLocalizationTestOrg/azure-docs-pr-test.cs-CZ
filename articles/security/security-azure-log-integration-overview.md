---
title: "protokoly aaaIntegrate z Azure prostředků do vašeho systému SIEM systémů | Microsoft Docs"
description: "Další informace o integraci Azure protokolu, jejích klíčových funkcích a jak to funguje."
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TerryLanfear
ms.assetid: 9c1346e1-baf8-4975-b2f2-42ae05b2dc0a
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/10/2017
ms.author: TomSh
ms.custom: azlog
ms.openlocfilehash: 4a59ce625702e5266a7c8eb020473cfeaf6b1964
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toomicrosoft-azure-log-integration"></a>Úvod tooMicrosoft integrace protokolů Azure
Další informace o integraci Azure protokolu, jejích klíčových funkcích a jak to funguje.

## <a name="overview"></a>Přehled

Integrace protokolů Azure je volné řešení, které vám umožní toointegrate nezpracovaná protokoly z vašich prostředků Azure v tooyour místní informace o zabezpečení a událostí správy (SIEM) systémech.

Integrace Azure protokolu událostí systému Windows shromažďuje z protokoly Prohlížeče událostí systému Windows, [protokoly aktivity Azure](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), [výstrahy Azure Security Center](../security-center/security-center-intro.md), a [Azure diagnostické protokoly](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) z Prostředky Azure. Tato integrace pomáhá řešení SIEM poskytnout jednotný řídicí panel pro všechny prostředky, místně nebo v cloudu hello, takže můžete agregovat, korelovat, analyzovat a výstrahy pro události zabezpečení.

>[!NOTE]
V tomto okamžiku jsou hello podporovány pouze cloudy komerční Azure a Azure Government. Ostatních cloudů nejsou podporovány.

![Integrace protokolů Azure][1]

## <a name="what-logs-can-i-integrate"></a>Jaké protokoly můžete integrovat?
Azure vytvoří rozsáhlé protokolování pro všechny služby Azure. Tyto protokoly představují tři typy protokolů:

* **Řízení nebo protokoly** poskytují přehled o hello [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) operace vytvoření, aktualizace a odstranění. Azure protokoly aktivity je příklad tohoto typu protokolu.
* **Data roviny protokoly** poskytují přehled o hello událostí vyvolaných jako součást hello využití prostředek služby Azure. Příkladem tohoto typu protokolu je hello Prohlížeč událostí systému Windows na **systému**, **zabezpečení**, a **aplikace** kanálů v virtuálního počítače s Windows. Dalším příkladem je protokolování diagnostiky nakonfigurovaný pomocí Azure monitorování
* **Zpracování události** zadejte analyzovaných událostí a informace o výstrahách zpracovat vaším jménem. Příkladem tohoto typu události je Azure Center výstrah zabezpečení, pokud je zpracovat a analyzovat předplatné tooprovide výstrahy relevantní tooyour aktuální lepšímu zabezpečení Azure Security Center.

Integrace se službou Azure protokolu podporuje ArcSight, QRadar a Splunk. Za všech okolností Zkontrolujte prosím se vašeho systému SIEM dodavatele tooassess jestli mají nativní konektor. V některých případech nebudete potřebovat toouse protokolu integrace se službou Azure, když nativní konektory jsou k dispozici. Další informace o podporovaných protokolů, že typy navštivte hello – nejčastější dotazy.

>[!NOTE]
Sice se integrace protokolu Azure bezplatné řešení, existují vyplývající z úložiště informace souboru protokolu hello náklady na úložiště Azure.

Komunita pomoc je k dispozici prostřednictvím hello [fórum MSDN integrace protokolu Azure](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration). fórum Hello poskytuje hello AzLog komunity hello možnost toosupport vzájemně otázky, odpovědi, tipy a triky na tom, jak tooget hello nejvíce mimo protokolu integrace se službou Azure. Kromě toho hello protokolu integrace se službou Azure tým monitoruje Toto fórum a bude vždy, když můžeme.

Můžete také otevřít [žádost o podporu](../azure-supportability/how-to-create-azure-support-request.md). toodo se vyberte **integrace protokolu** jako hello služby, pro kterou jsou žádosti o podporu.

## <a name="next-steps"></a>Další kroky
V tomto dokumentu jste přináší tooAzure integrace protokolu. toolearn informace o Azure protokolu integrace a hello typy protokolů podporovaná, najdete v části hello následující:

* [Integrace se službou protokolu Microsoft Azure](https://www.microsoft.com/download/details.aspx?id=53324) – Download Center podrobnosti, požadavky na systém a instalovat pokyny týkající se integrace protokolů Azure.
* [Začínáme s Azure protokolu integrace](security-azure-log-integration-get-started.md) – tento kurz vás provede procesem instalace integrace protokolů Azure a integrace protokoly z úložiště Azure WAD, protokoly aktivity Azure, Azure Security Center výstrahy a Azure Active Directory protokoly auditu.
* [Partner kroky konfigurace](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – tento příspěvek blogu ukazuje, jak tooconfigure Azure protokolu toowork integrace s partnerských řešení Splunk, HP ArcSight a IBM QRadar. Tomto blogu představuje naše aktuální pozici na konfiguraci hello partnerských řešení. Ve všech případech naleznete v dokumentaci řešení toopartner nejdřív.
* [Aktivita a ASC výstrahy nad syslog tooQRadar](https://blogs.msdn.microsoft.com/azuresecurity/2016/09/24/integrate-azure-logs-to-qradar/) – tento příspěvek blogu popisuje kroky, hello toosend aktivity a Azure Security Center výstrahy přes syslog tooQRadar
* [Protokolů Azure integrace nejčastější dotazy (FAQ)](security-azure-log-integration-faq.md) – nejčastější dotazy týkající se tento odpovědi dotazy týkající se integrace protokolů Azure.
* [Integrace Security Center výstrahy s Azure protokolu integrace](../security-center/security-center-integrating-alerts-with-log-integration.md) – tento dokument ukazuje, jak toosync Azure Security Center výstrahy protokolu integrace se službou Azure.

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png
