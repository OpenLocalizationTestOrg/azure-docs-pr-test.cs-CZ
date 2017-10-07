---
title: "transakce aaaMonitor B2B a nastavení protokolování - Azure Logic Apps | Microsoft Docs"
description: "Monitorování pro AS2, X 12 a EDIFACT zprávy, spusťte protokolování diagnostiky pro váš účet integrace"
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 07/21/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: a6745ebf41aab331020bfec072f5806711d125bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-set-up-diagnostics-logging-for-b2b-communication-in-integration-accounts"></a>Sledování a protokolování diagnostiky pro komunikace B2B ve účty pro integraci

Po nastavení komunikace B2B mezi dvěma systémem obchodních procesů nebo aplikací prostřednictvím účtu integrace tyto entity můžou vyměňovat zprávy mezi sebou. tooconfirm tato komunikace funguje podle očekávání, můžete nastavit pro AS2, X12, monitorování a EDIFACT zprávy, spolu s protokolování diagnostiky pro váš účet integrace prostřednictvím hello [Azure Log Analytics](../log-analytics/log-analytics-overview.md) služby. Tato služba v [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md) monitoruje své cloudové a místní prostředí, což pomáhá udržovat jejich dostupnost a výkon a taky shromažďuje podrobnosti runtime a událostí pro širší ladění. Můžete také [pomocí diagnostických dat s jinými službami](#extend-diagnostic-data), jako jsou služby Azure Storage a Azure Event Hubs.

## <a name="requirements"></a>Požadavky

* Aplikace logiky, který je nastavený s protokolování diagnostiky. Další informace [jak tooset protokolování pro tuto aplikaci logiky](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

  > [!NOTE]
  > Po splnění tohoto požadavku, měli byste mít pracovního prostoru v hello [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md). Měli byste použít při nastavování protokolování pro váš účet integrace hello stejný pracovní prostor OMS. Pokud nemáte pracovním prostorem OMS, přečtěte si [jak toocreate pracovním prostorem OMS](../log-analytics/log-analytics-get-started.md).

* Integrace účet, který má propojené tooyour aplikace logiky. Další informace [účet, jak toocreate integrační aplikace logiky tooyour odkaz](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md).

## <a name="turn-on-diagnostics-logging-for-your-integration-account"></a>Zapněte diagnostiku protokolování pro váš účet integrace

Můžete zapnout protokolování buď přímo z vašeho účtu integrace nebo [prostřednictvím hello služba Azure sledování](#azure-monitor-service). Monitorování Azure poskytuje základní monitorování s daty úrovni infrastruktury. Další informace o [Azure monitorování](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md).

### <a name="turn-on-diagnostics-logging-directly-from-your-integration-account"></a>Zapněte diagnostiku protokolování přímo z vašeho účtu integrace

1. V hello [portál Azure](https://portal.azure.com), najděte a vyberte svůj účet integrace. V části **monitorování**, zvolte **protokolů diagnostiky** jak je vidět tady:

   ![Najít a vyberte svůj účet integrace, zvolte "Diagnostické protokoly"](media/logic-apps-monitor-b2b-message/integration-account-diagnostics.png)

2. Po výběru účtu integrace, automaticky se vyberou hello následující hodnoty. Pokud tyto hodnoty jsou správné, zvolte **zapněte diagnostiku**. Jinak vyberte možnost hello hodnoty, které chcete:

   1. V části **předplatné**, vyberte hello předplatné Azure, který používáte s vaším účtem integrace.
   2. V části **skupiny prostředků**, vyberte skupinu prostředků hello, který používáte s vaším účtem integrace.
   3. V části **typ prostředku**, vyberte **účty pro integraci**. 
   4. V části **prostředků**, vyberte svůj účet integrace. 
   5. Zvolte **zapněte diagnostiku**.

   ![Nastavení diagnostiky pro váš účet integrace](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account.png)

3. V části **nastavení diagnostiky**a potom **stav**, zvolte **na**.

   ![Zapněte diagnostiku Azure](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account-2.png)

4. Nyní vyberte toouse prostoru a data OMS hello protokolování, jak je znázorněno:

   1. Vyberte **odeslat tooLog Analytics**. 
   2. V části **analýzy protokolů**, zvolte **konfigurace**. 
   3. V části **pracovních prostorů OMS**, vyberte hello OMS prostoru toouse pro protokolování.
   4. V části **protokolu**, vyberte hello **IntegrationAccountTrackingEvents** kategorie.
   5. Zvolte **Uložit**.

   ![Nastavení analýzy protokolů, můžete odeslat protokolu tooa dat diagnostiky](media/logic-apps-monitor-b2b-message/send-diagnostics-data-log-analytics-workspace.png)

5. Nyní [nastavení sledování zpráv B2B v OMS](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

<a name="azure-monitor-service"></a>

### <a name="turn-on-diagnostics-logging-through-azure-monitor"></a>Zapnutí protokolování diagnostiky prostřednictvím Azure monitorování

1. V hello [portál Azure](https://portal.azure.com), na hello hlavní nabídky Azure, zvolte **monitorování**, **protokolů diagnostiky**. Potom vyberte svůj účet integrace, jak je vidět tady:

   ![Zvolte "Sledovat", "Diagnostické protokoly", vyberte svůj účet integrace](media/logic-apps-monitor-b2b-message/monitor-service-diagnostics-logs.png)

2. Po výběru účtu integrace, automaticky se vyberou hello následující hodnoty. Pokud tyto hodnoty jsou správné, zvolte **zapněte diagnostiku**. Jinak vyberte možnost hello hodnoty, které chcete:

   1. V části **předplatné**, vyberte hello předplatné Azure, který používáte s vaším účtem integrace.
   2. V části **skupiny prostředků**, vyberte skupinu prostředků hello, který používáte s vaším účtem integrace.
   3. V části **typ prostředku**, vyberte **účty pro integraci**.
   4. V části **prostředků**, vyberte svůj účet integrace.
   5. Zvolte **zapněte diagnostiku**.

   ![Nastavení diagnostiky pro váš účet integrace](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account.png)

3. V části **nastavení diagnostiky**, zvolte **na**.

   ![Zapněte diagnostiku Azure](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account-2.png)

4. Nyní vyberte kategorii pracovní prostor a událostí OMS hello pro protokolování, jak je znázorněno:

   1. Vyberte **odeslat tooLog Analytics**. 
   2. V části **analýzy protokolů**, zvolte **konfigurace**. 
   3. V části **pracovních prostorů OMS**, vyberte hello OMS prostoru toouse pro protokolování.
   4. V části **protokolu**, vyberte hello **IntegrationAccountTrackingEvents** kategorie.
   5. Až budete hotoví, zvolte **Uložit**.

   ![Nastavení analýzy protokolů, můžete odeslat protokolu tooa dat diagnostiky](media/logic-apps-monitor-b2b-message/send-diagnostics-data-log-analytics-workspace.png)

5. Nyní [nastavení sledování zpráv B2B v OMS](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

## <a name="extend-how-and-where-you-use-diagnostic-data-with-other-services"></a>Rozšíření jak a kde používáte diagnostických dat s jinými službami

Společně s Azure Log Analytics můžete rozšířit použití aplikace logiky diagnostických dat s jinými službami Azure, například: 

* [Archiv, který Azure Diagnostics protokolů v úložišti Azure](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md)
* [TooAzure datový proud protokolů diagnostiky Azure Event Hubs](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md) 

Můžete pak monitorování pomocí telemetrie a analýzy z jiných služeb, jako například get v reálném čase [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) a [Power BI](../log-analytics/log-analytics-powerbi.md). Například:

* [Datový proud dat ze služby Event Hubs tooStream Analytics](../stream-analytics/stream-analytics-define-inputs.md)
* [Analýza dat pomocí služby Stream Analytics a vytvořit řídicí panel analýzu v reálném čase v Power BI](../stream-analytics/stream-analytics-power-bi-dashboard.md)

Podle hello možnosti, které chcete nastavit, ujistěte se, že jste první [vytvořit účet úložiště Azure](../storage/common/storage-create-storage-account.md) nebo [vytváření centra událostí Azure](../event-hubs/event-hubs-create.md). Potom vyberte možnosti hello místo toosend diagnostických dat:

![Odeslání dat tooAzure úložiště účet nebo události rozbočovače](./media/logic-apps-monitor-b2b-message/storage-account-event-hubs.png)

> [!NOTE]
> Období uchovávání použít pouze v případě, že zvolíte toouse účet úložiště.

## <a name="supported-tracking-schemas"></a>Podporované sledování schémata

Azure podporuje tyto typy schémat, které stanoveny schémata s výjimkou hello vlastní typ sledování.

* [Schéma sledování AS2](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [Schéma sledování X12](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Vlastní schémata sledování](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)

## <a name="next-steps"></a>Další kroky

* [Sledování zpráv B2B v OMS](../logic-apps/logic-apps-track-b2b-messages-omsportal.md "zpráv B2B sledování v OMS")
* [Další informace o hello Enterprise integračního balíčku](../logic-apps/logic-apps-enterprise-integration-overview.md "Další informace o Enterprise integračního balíčku")

