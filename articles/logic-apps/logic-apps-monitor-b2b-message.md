---
title: "Monitorování transakcí B2B a nastavení protokolování - Azure Logic Apps | Microsoft Docs"
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
ms.openlocfilehash: f717dae9a70a96944b623f22b90cf8c5a943f382
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="monitor-and-set-up-diagnostics-logging-for-b2b-communication-in-integration-accounts"></a><span data-ttu-id="c7b2e-103">Sledování a protokolování diagnostiky pro komunikace B2B ve účty pro integraci</span><span class="sxs-lookup"><span data-stu-id="c7b2e-103">Monitor and set up diagnostics logging for B2B communication in integration accounts</span></span>

<span data-ttu-id="c7b2e-104">Po nastavení komunikace B2B mezi dvěma systémem obchodních procesů nebo aplikací prostřednictvím účtu integrace tyto entity můžou vyměňovat zprávy mezi sebou.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-104">After you set up B2B communication between two running business processes or applications through your integration account, those entities can exchange messages with each other.</span></span> <span data-ttu-id="c7b2e-105">Potvrďte tuto komunikaci funguje podle očekávání, můžete nastavit pro AS2, X12, monitorování a EDIFACT zprávy, spolu s protokolování diagnostiky pro váš účet integrace prostřednictvím [Azure Log Analytics](../log-analytics/log-analytics-overview.md) služby.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-105">To confirm this communication works as expected, you can set up monitoring for AS2, X12, and EDIFACT messages, along with diagnostics logging for your integration account through the [Azure Log Analytics](../log-analytics/log-analytics-overview.md) service.</span></span> <span data-ttu-id="c7b2e-106">Tato služba v [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md) monitoruje své cloudové a místní prostředí, což pomáhá udržovat jejich dostupnost a výkon a taky shromažďuje podrobnosti runtime a událostí pro širší ladění.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-106">This service in [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md) monitors your cloud and on-premises environments, helping you maintain their availability and performance, and also collects runtime details and events for richer debugging.</span></span> <span data-ttu-id="c7b2e-107">Můžete také [pomocí diagnostických dat s jinými službami](#extend-diagnostic-data), jako jsou služby Azure Storage a Azure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-107">You can also [use your diagnostic data with other services](#extend-diagnostic-data), like Azure Storage and Azure Event Hubs.</span></span>

## <a name="requirements"></a><span data-ttu-id="c7b2e-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c7b2e-108">Requirements</span></span>

* <span data-ttu-id="c7b2e-109">Aplikace logiky, který je nastavený s protokolování diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-109">A logic app that's set up with diagnostics logging.</span></span> <span data-ttu-id="c7b2e-110">Další informace [postup nastavení protokolování pro tuto aplikaci logiky](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).</span><span class="sxs-lookup"><span data-stu-id="c7b2e-110">Learn [how to set up logging for that logic app](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).</span></span>

  > [!NOTE]
  > <span data-ttu-id="c7b2e-111">Po splnění tohoto požadavku, měli byste pracovního prostoru [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c7b2e-111">After you've met this requirement, you should have a workspace in the [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md).</span></span> <span data-ttu-id="c7b2e-112">Při nastavení protokolování pro váš účet integrace, měli byste použít stejné pracovním prostorem OMS.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-112">You should use the same OMS workspace when you set up logging for your integration account.</span></span> <span data-ttu-id="c7b2e-113">Pokud nemáte pracovním prostorem OMS, přečtěte si [jak vytvořit pracovní prostor služby OMS](../log-analytics/log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="c7b2e-113">If you don't have an OMS workspace, learn [how to create an OMS workspace](../log-analytics/log-analytics-get-started.md).</span></span>

* <span data-ttu-id="c7b2e-114">Integrace účet, který je propojený s svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-114">An integration account that's linked to your logic app.</span></span> <span data-ttu-id="c7b2e-115">Další informace [postup vytvoření účtu integrace se zobrazí odkaz na svou aplikaci logiky](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md).</span><span class="sxs-lookup"><span data-stu-id="c7b2e-115">Learn [how to create an integration account with a link to your logic app](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md).</span></span>

## <a name="turn-on-diagnostics-logging-for-your-integration-account"></a><span data-ttu-id="c7b2e-116">Zapněte diagnostiku protokolování pro váš účet integrace</span><span class="sxs-lookup"><span data-stu-id="c7b2e-116">Turn on diagnostics logging for your integration account</span></span>

<span data-ttu-id="c7b2e-117">Můžete zapnout protokolování buď přímo z vašeho účtu integrace nebo [pomocí služby Azure monitorování](#azure-monitor-service).</span><span class="sxs-lookup"><span data-stu-id="c7b2e-117">You can turn on logging either directly from your integration account or [through the Azure Monitor service](#azure-monitor-service).</span></span> <span data-ttu-id="c7b2e-118">Monitorování Azure poskytuje základní monitorování s daty úrovni infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-118">Azure Monitor provides basic monitoring with infrastructure-level data.</span></span> <span data-ttu-id="c7b2e-119">Další informace o [Azure monitorování](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md).</span><span class="sxs-lookup"><span data-stu-id="c7b2e-119">Learn more about [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md).</span></span>

### <a name="turn-on-diagnostics-logging-directly-from-your-integration-account"></a><span data-ttu-id="c7b2e-120">Zapněte diagnostiku protokolování přímo z vašeho účtu integrace</span><span class="sxs-lookup"><span data-stu-id="c7b2e-120">Turn on diagnostics logging directly from your integration account</span></span>

1. <span data-ttu-id="c7b2e-121">V [portál Azure](https://portal.azure.com), najděte a vyberte svůj účet integrace.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-121">In the [Azure portal](https://portal.azure.com), find and select your integration account.</span></span> <span data-ttu-id="c7b2e-122">V části **monitorování**, zvolte **protokolů diagnostiky** jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="c7b2e-122">Under **Monitoring**, choose **Diagnostics logs** as shown here:</span></span>

   ![Najít a vyberte svůj účet integrace, zvolte "Diagnostické protokoly"](media/logic-apps-monitor-b2b-message/integration-account-diagnostics.png)

2. <span data-ttu-id="c7b2e-124">Po výběru účtu integrace, automaticky se vyberou následující hodnoty.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-124">After you select your integration account, the following values are automatically selected.</span></span> <span data-ttu-id="c7b2e-125">Pokud tyto hodnoty jsou správné, zvolte **zapněte diagnostiku**.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-125">If these values are correct, choose **Turn on diagnostics**.</span></span> <span data-ttu-id="c7b2e-126">Jinak vyberte možnost hodnoty, které chcete:</span><span class="sxs-lookup"><span data-stu-id="c7b2e-126">Otherwise, select the values that you want:</span></span>

   1. <span data-ttu-id="c7b2e-127">V části **předplatné**, vyberte předplatné Azure, který používáte s vaším účtem integrace.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-127">Under **Subscription**, select the Azure subscription that you use with your integration account.</span></span>
   2. <span data-ttu-id="c7b2e-128">V části **skupiny prostředků**, vyberte skupinu prostředků, který používáte s vaším účtem integrace.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-128">Under **Resource group**, select the resource group that you use with your integration account.</span></span>
   3. <span data-ttu-id="c7b2e-129">V části **typ prostředku**, vyberte **účty pro integraci**.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-129">Under **Resource type**, select **Integration accounts**.</span></span> 
   4. <span data-ttu-id="c7b2e-130">V části **prostředků**, vyberte svůj účet integrace.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-130">Under **Resource**, select your integration account.</span></span> 
   5. <span data-ttu-id="c7b2e-131">Zvolte **zapněte diagnostiku**.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-131">Choose **Turn on diagnostics**.</span></span>

   ![Nastavení diagnostiky pro váš účet integrace](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account.png)

3. <span data-ttu-id="c7b2e-133">V části **nastavení diagnostiky**a potom **stav**, zvolte **na**.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-133">Under **Diagnostics settings**, and then **Status**, choose **On**.</span></span>

   ![Zapněte diagnostiku Azure](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account-2.png)

4. <span data-ttu-id="c7b2e-135">Nyní vyberte pracovní prostor OMS a data se mají použít pro protokolování, jak je znázorněno:</span><span class="sxs-lookup"><span data-stu-id="c7b2e-135">Now select the OMS workspace and data to use for logging as shown:</span></span>

   1. <span data-ttu-id="c7b2e-136">Vyberte **odeslat k analýze protokolů**.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-136">Select **Send to Log Analytics**.</span></span> 
   2. <span data-ttu-id="c7b2e-137">V části **analýzy protokolů**, zvolte **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-137">Under **Log Analytics**, choose **Configure**.</span></span> 
   3. <span data-ttu-id="c7b2e-138">V části **pracovních prostorů OMS**, vyberte pracovní prostor OMS pro protokolování.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-138">Under **OMS Workspaces**, select the OMS workspace to use for logging.</span></span>
   4. <span data-ttu-id="c7b2e-139">V části **protokolu**, vyberte **IntegrationAccountTrackingEvents** kategorie.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-139">Under **Log**, select the **IntegrationAccountTrackingEvents** category.</span></span>
   5. <span data-ttu-id="c7b2e-140">Zvolte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-140">Choose **Save**.</span></span>

   ![Nastavení analýzy protokolů tak může odesílat data diagnostiky protokolu](media/logic-apps-monitor-b2b-message/send-diagnostics-data-log-analytics-workspace.png)

5. <span data-ttu-id="c7b2e-142">Nyní [nastavení sledování zpráv B2B v OMS](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).</span><span class="sxs-lookup"><span data-stu-id="c7b2e-142">Now [set up tracking for your B2B messages in OMS](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).</span></span>

<a name="azure-monitor-service"></a>

### <a name="turn-on-diagnostics-logging-through-azure-monitor"></a><span data-ttu-id="c7b2e-143">Zapnutí protokolování diagnostiky prostřednictvím Azure monitorování</span><span class="sxs-lookup"><span data-stu-id="c7b2e-143">Turn on diagnostics logging through Azure Monitor</span></span>

1. <span data-ttu-id="c7b2e-144">V [portál Azure](https://portal.azure.com), v hlavní nabídce Azure, zvolte **monitorování**, **protokolů diagnostiky**.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-144">In the [Azure portal](https://portal.azure.com), on the main Azure menu, choose **Monitor**, **Diagnostics logs**.</span></span> <span data-ttu-id="c7b2e-145">Potom vyberte svůj účet integrace, jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="c7b2e-145">Then select your integration account as shown here:</span></span>

   ![Zvolte "Sledovat", "Diagnostické protokoly", vyberte svůj účet integrace](media/logic-apps-monitor-b2b-message/monitor-service-diagnostics-logs.png)

2. <span data-ttu-id="c7b2e-147">Po výběru účtu integrace, automaticky se vyberou následující hodnoty.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-147">After you select your integration account, the following values are automatically selected.</span></span> <span data-ttu-id="c7b2e-148">Pokud tyto hodnoty jsou správné, zvolte **zapněte diagnostiku**.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-148">If these values are correct, choose **Turn on diagnostics**.</span></span> <span data-ttu-id="c7b2e-149">Jinak vyberte možnost hodnoty, které chcete:</span><span class="sxs-lookup"><span data-stu-id="c7b2e-149">Otherwise, select the values that you want:</span></span>

   1. <span data-ttu-id="c7b2e-150">V části **předplatné**, vyberte předplatné Azure, který používáte s vaším účtem integrace.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-150">Under **Subscription**, select the Azure subscription that you use with your integration account.</span></span>
   2. <span data-ttu-id="c7b2e-151">V části **skupiny prostředků**, vyberte skupinu prostředků, který používáte s vaším účtem integrace.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-151">Under **Resource group**, select the resource group that you use with your integration account.</span></span>
   3. <span data-ttu-id="c7b2e-152">V části **typ prostředku**, vyberte **účty pro integraci**.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-152">Under **Resource type**, select **Integration accounts**.</span></span>
   4. <span data-ttu-id="c7b2e-153">V části **prostředků**, vyberte svůj účet integrace.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-153">Under **Resource**, select your integration account.</span></span>
   5. <span data-ttu-id="c7b2e-154">Zvolte **zapněte diagnostiku**.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-154">Choose **Turn on diagnostics**.</span></span>

   ![Nastavení diagnostiky pro váš účet integrace](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account.png)

3. <span data-ttu-id="c7b2e-156">V části **nastavení diagnostiky**, zvolte **na**.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-156">Under **Diagnostics settings**, choose **On**.</span></span>

   ![Zapněte diagnostiku Azure](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account-2.png)

4. <span data-ttu-id="c7b2e-158">Nyní vyberte OMS pracovní prostor a událostí kategorie pro protokolování, jak je znázorněno:</span><span class="sxs-lookup"><span data-stu-id="c7b2e-158">Now select the OMS workspace and event category for logging as shown:</span></span>

   1. <span data-ttu-id="c7b2e-159">Vyberte **odeslat k analýze protokolů**.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-159">Select **Send to Log Analytics**.</span></span> 
   2. <span data-ttu-id="c7b2e-160">V části **analýzy protokolů**, zvolte **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-160">Under **Log Analytics**, choose **Configure**.</span></span> 
   3. <span data-ttu-id="c7b2e-161">V části **pracovních prostorů OMS**, vyberte pracovní prostor OMS pro protokolování.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-161">Under **OMS Workspaces**, select the OMS workspace to use for logging.</span></span>
   4. <span data-ttu-id="c7b2e-162">V části **protokolu**, vyberte **IntegrationAccountTrackingEvents** kategorie.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-162">Under **Log**, select the **IntegrationAccountTrackingEvents** category.</span></span>
   5. <span data-ttu-id="c7b2e-163">Až budete hotoví, zvolte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-163">When you're done, choose **Save**.</span></span>

   ![Nastavení analýzy protokolů tak může odesílat data diagnostiky protokolu](media/logic-apps-monitor-b2b-message/send-diagnostics-data-log-analytics-workspace.png)

5. <span data-ttu-id="c7b2e-165">Nyní [nastavení sledování zpráv B2B v OMS](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).</span><span class="sxs-lookup"><span data-stu-id="c7b2e-165">Now [set up tracking for your B2B messages in OMS](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).</span></span>

## <a name="extend-how-and-where-you-use-diagnostic-data-with-other-services"></a><span data-ttu-id="c7b2e-166">Rozšíření jak a kde používáte diagnostických dat s jinými službami</span><span class="sxs-lookup"><span data-stu-id="c7b2e-166">Extend how and where you use diagnostic data with other services</span></span>

<span data-ttu-id="c7b2e-167">Společně s Azure Log Analytics můžete rozšířit použití aplikace logiky diagnostických dat s jinými službami Azure, například:</span><span class="sxs-lookup"><span data-stu-id="c7b2e-167">Along with Azure Log Analytics, you can extend how you use your logic app's diagnostic data with other Azure services, for example:</span></span> 

* [<span data-ttu-id="c7b2e-168">Archiv, který Azure Diagnostics protokolů v úložišti Azure</span><span class="sxs-lookup"><span data-stu-id="c7b2e-168">Archive Azure Diagnostics Logs in Azure Storage</span></span>](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md)
* [<span data-ttu-id="c7b2e-169">Datový proud protokolů Azure Diagnostics do služby Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="c7b2e-169">Stream Azure Diagnostics Logs to Azure Event Hubs</span></span>](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md) 

<span data-ttu-id="c7b2e-170">Můžete pak monitorování pomocí telemetrie a analýzy z jiných služeb, jako například get v reálném čase [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) a [Power BI](../log-analytics/log-analytics-powerbi.md).</span><span class="sxs-lookup"><span data-stu-id="c7b2e-170">You can then get real-time monitoring by using telemetry and analytics from other services, like [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) and [Power BI](../log-analytics/log-analytics-powerbi.md).</span></span> <span data-ttu-id="c7b2e-171">Například:</span><span class="sxs-lookup"><span data-stu-id="c7b2e-171">For example:</span></span>

* [<span data-ttu-id="c7b2e-172">Datový proud dat ze služby Event Hubs do služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="c7b2e-172">Stream data from Event Hubs to Stream Analytics</span></span>](../stream-analytics/stream-analytics-define-inputs.md)
* [<span data-ttu-id="c7b2e-173">Analýza dat pomocí služby Stream Analytics a vytvořit řídicí panel analýzu v reálném čase v Power BI</span><span class="sxs-lookup"><span data-stu-id="c7b2e-173">Analyze streaming data with Stream Analytics and create a real-time analytics dashboard in Power BI</span></span>](../stream-analytics/stream-analytics-power-bi-dashboard.md)

<span data-ttu-id="c7b2e-174">V závislosti na možnosti, které chcete nastavit, ujistěte se, že jste první [vytvořit účet úložiště Azure](../storage/common/storage-create-storage-account.md) nebo [vytváření centra událostí Azure](../event-hubs/event-hubs-create.md).</span><span class="sxs-lookup"><span data-stu-id="c7b2e-174">Based on the options that you want set up, make sure that you first [create an Azure storage account](../storage/common/storage-create-storage-account.md) or [create an Azure event hub](../event-hubs/event-hubs-create.md).</span></span> <span data-ttu-id="c7b2e-175">Vyberte požadované možnosti pro které chcete poslat diagnostická data:</span><span class="sxs-lookup"><span data-stu-id="c7b2e-175">Then select the options for where you want to send diagnostic data:</span></span>

![Odesílání dat do úložiště Azure účet nebo události rozbočovače](./media/logic-apps-monitor-b2b-message/storage-account-event-hubs.png)

> [!NOTE]
> <span data-ttu-id="c7b2e-177">Období uchovávání se projeví pouze v případě, že ji rozhodnete použít účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-177">Retention periods apply only when you choose to use a storage account.</span></span>

## <a name="supported-tracking-schemas"></a><span data-ttu-id="c7b2e-178">Podporované sledování schémata</span><span class="sxs-lookup"><span data-stu-id="c7b2e-178">Supported tracking schemas</span></span>

<span data-ttu-id="c7b2e-179">Azure podporuje tyto typy schémat, které stanoveny schémata kromě vlastní typ sledování.</span><span class="sxs-lookup"><span data-stu-id="c7b2e-179">Azure supports these tracking schema types, which all have fixed schemas except the Custom type.</span></span>

* [<span data-ttu-id="c7b2e-180">Schéma sledování AS2</span><span class="sxs-lookup"><span data-stu-id="c7b2e-180">AS2 tracking schema</span></span>](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [<span data-ttu-id="c7b2e-181">Schéma sledování X12</span><span class="sxs-lookup"><span data-stu-id="c7b2e-181">X12 tracking schema</span></span>](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [<span data-ttu-id="c7b2e-182">Vlastní schémata sledování</span><span class="sxs-lookup"><span data-stu-id="c7b2e-182">Custom tracking schema</span></span>](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)

## <a name="next-steps"></a><span data-ttu-id="c7b2e-183">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c7b2e-183">Next steps</span></span>

* [<span data-ttu-id="c7b2e-184">Sledování zpráv B2B v OMS</span><span class="sxs-lookup"><span data-stu-id="c7b2e-184">Track B2B messages in OMS</span></span>](../logic-apps/logic-apps-track-b2b-messages-omsportal.md "zpráv B2B sledování v OMS")
* [<span data-ttu-id="c7b2e-185">Další informace o integračního balíčku Enterprise</span><span class="sxs-lookup"><span data-stu-id="c7b2e-185">Learn more about the Enterprise Integration Pack</span></span>](../logic-apps/logic-apps-enterprise-integration-overview.md "Další informace o Enterprise integračního balíčku")

