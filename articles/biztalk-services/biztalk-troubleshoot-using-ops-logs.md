---
title: "Řešení potíží s služby BizTalk Services pomocí protokoly operací | Microsoft Docs"
description: "Řešení potíží s služby BizTalk Services pomocí protokoly operací. MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 1081a9c6-58cc-4a76-8ac8-11e5e7a6ea27
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: c0c83361f94ffd9c30d7fcc551ff4b85ad7d6fa5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="biztalk-services-troubleshoot-using-operation-logs"></a><span data-ttu-id="0868f-104">BizTalk Services: Řešení problémů pomocí protokolů operací</span><span class="sxs-lookup"><span data-stu-id="0868f-104">BizTalk Services: Troubleshoot using operation logs</span></span>

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

## <a name="what-are-the-operation-logs"></a><span data-ttu-id="0868f-105">Jaké jsou protokoly operací</span><span class="sxs-lookup"><span data-stu-id="0868f-105">What are the Operation Logs</span></span>
<span data-ttu-id="0868f-106">Protokoly operací je funkce služby pro správu, k dispozici na portálu Azure classic, který vám umožní zobrazit historické protokoly operací týkajících se služeb Azure, včetně služby BizTalk Services.</span><span class="sxs-lookup"><span data-stu-id="0868f-106">Operation Logs is a Management Services feature available in the Azure classic portal that allows you to view historical logs of operations performed on your Azure services, including BizTalk Services.</span></span> <span data-ttu-id="0868f-107">To umožňuje zobrazit historická data týkající se správy operací na své předplatné služby BizTalk až 180 dní.</span><span class="sxs-lookup"><span data-stu-id="0868f-107">This enables you to view historical data related to management operations on your BizTalk Service subscription as far back as 180 days.</span></span>

> [!NOTE]
> <span data-ttu-id="0868f-108">Tato funkce zaznamená pouze protokoly pro operace správy v BizTalk Services, například při spuštění služby, založenou na aktivní, a tak dále.</span><span class="sxs-lookup"><span data-stu-id="0868f-108">This feature only captures logs for management operations on BizTalk Services, such as when the service was started, backed up, and so on.</span></span> <span data-ttu-id="0868f-109">Tyto operace jsou sledovány bez ohledu na to, jestli se provedlo z portálu Azure classic nebo pomocí [rozhraní API REST služby BizTalk](http://msdn.microsoft.com/library/azure/dn232347.aspx).</span><span class="sxs-lookup"><span data-stu-id="0868f-109">Such operations are tracked irrespective of whether they are performed from the Azure classic portal or by using the [BizTalk Service REST APIs](http://msdn.microsoft.com/library/azure/dn232347.aspx).</span></span> <span data-ttu-id="0868f-110">Úplný seznam operací, které jsou sledovány pomocí služby správy najdete v tématu [Operations sledovaných pomocí služeb Azure Management](#bizops).</span><span class="sxs-lookup"><span data-stu-id="0868f-110">For a complete list of operations that are tracked using Management Services, see [Operations Tracked Using Azure Management Services](#bizops).</span></span><br/><br/>
> <span data-ttu-id="0868f-111">Není to zachytit v protokolech činnosti týkající se služby BizTalk runtime (například zprávy zpracovává mostů atd.).</span><span class="sxs-lookup"><span data-stu-id="0868f-111">This does not capture the logs for activities related to BizTalk Service runtime (such as message processed by bridges, and so on.).</span></span> <span data-ttu-id="0868f-112">Pokud chcete zobrazit tyto protokoly, použijte zobrazení sledování z portálu služby BizTalk Services.</span><span class="sxs-lookup"><span data-stu-id="0868f-112">To view these logs, use the Tracking view from the BizTalk Services portal.</span></span> <span data-ttu-id="0868f-113">Další informace najdete v tématu [sledování zpráv](http://msdn.microsoft.com/library/azure/hh949805.aspx).</span><span class="sxs-lookup"><span data-stu-id="0868f-113">For more information, see [Tracking Messages](http://msdn.microsoft.com/library/azure/hh949805.aspx).</span></span>
> 
> 

## <a name="view-biztalk-services-operation-logs"></a><span data-ttu-id="0868f-114">Zobrazení, protokoly operací služby BizTalk Services</span><span class="sxs-lookup"><span data-stu-id="0868f-114">View BizTalk Services Operation Logs</span></span>
1. <span data-ttu-id="0868f-115">Na portálu Azure classic, vyberte **Management Services**a pak vyberte **protokoly operací** kartě.</span><span class="sxs-lookup"><span data-stu-id="0868f-115">In the Azure classic portal, select **Management Services**, and then select the **Operation Logs** tab.</span></span>
2. <span data-ttu-id="0868f-116">Můžete filtrovat na základě různých parametrů jako předplatné, rozsah kalendářních dat, typu služby (např. služby BizTalk), název služby nebo stav operace (úspěšné, neúspěšné) protokolů.</span><span class="sxs-lookup"><span data-stu-id="0868f-116">You can filter the logs based on different parameters like subscription, date range, service type (e.g. BizTalk Services), service name, or status of the operation (Succeeded, Failed).</span></span>
3. <span data-ttu-id="0868f-117">Vyberte na značku zaškrtnutí zobrazíte filtrované seznamu.</span><span class="sxs-lookup"><span data-stu-id="0868f-117">Select the checkmark to view the filtered list.</span></span> <span data-ttu-id="0868f-118">Následující obrázek znázorňuje aktivity související s testbiztalkservice: ![zobrazit protokoly operací][ViewLogs]</span><span class="sxs-lookup"><span data-stu-id="0868f-118">The following image shows activities related to testbiztalkservice: ![View operation logs][ViewLogs]</span></span> 
4. <span data-ttu-id="0868f-119">Chcete-li zobrazit více o konkrétní operaci, vyberte řádek a klikněte na **podrobnosti** na hlavním panelu v dolní části.</span><span class="sxs-lookup"><span data-stu-id="0868f-119">To view more about a specific operation, select the row, and click **Details** in the task bar at the bottom.</span></span>

## <span data-ttu-id="0868f-120"><a name="bizops"></a>Operace sledovat pomocí služby Azure pro</span><span class="sxs-lookup"><span data-stu-id="0868f-120"><a name="bizops"></a>Operations Tracked Using Azure Management Services</span></span>
<span data-ttu-id="0868f-121">Následující tabulka uvádí operace, které jsou sledovány pomocí služby pro Azure:</span><span class="sxs-lookup"><span data-stu-id="0868f-121">The following table lists the operations that are tracked using the Azure Management Services:</span></span>

| <span data-ttu-id="0868f-122">Název operace</span><span class="sxs-lookup"><span data-stu-id="0868f-122">Operation Name</span></span> | <span data-ttu-id="0868f-123">Úkol</span><span class="sxs-lookup"><span data-stu-id="0868f-123">Task</span></span> |
| --- | --- |
| <span data-ttu-id="0868f-124">CreateBizTalkService</span><span class="sxs-lookup"><span data-stu-id="0868f-124">CreateBizTalkService</span></span> |<span data-ttu-id="0868f-125">Operace vytvoření nové služby BizTalk</span><span class="sxs-lookup"><span data-stu-id="0868f-125">Operation to create a new BizTalk Service</span></span> |
| <span data-ttu-id="0868f-126">DeleteBizTalkService</span><span class="sxs-lookup"><span data-stu-id="0868f-126">DeleteBizTalkService</span></span> |<span data-ttu-id="0868f-127">Operace odstranění služby BizTalk</span><span class="sxs-lookup"><span data-stu-id="0868f-127">Operation to delete a BizTalk Service</span></span> |
| <span data-ttu-id="0868f-128">RestartBizTalkService</span><span class="sxs-lookup"><span data-stu-id="0868f-128">RestartBizTalkService</span></span> |<span data-ttu-id="0868f-129">Operace restartování služby BizTalk</span><span class="sxs-lookup"><span data-stu-id="0868f-129">Operation to restart a BizTalk Service</span></span> |
| <span data-ttu-id="0868f-130">StartBizTalkService</span><span class="sxs-lookup"><span data-stu-id="0868f-130">StartBizTalkService</span></span> |<span data-ttu-id="0868f-131">Operaci pro spuštění služby BizTalk</span><span class="sxs-lookup"><span data-stu-id="0868f-131">Operation to start a BizTalk Service</span></span> |
| <span data-ttu-id="0868f-132">StopBizTalkService</span><span class="sxs-lookup"><span data-stu-id="0868f-132">StopBizTalkService</span></span> |<span data-ttu-id="0868f-133">Operace zastavení služby BizTalk</span><span class="sxs-lookup"><span data-stu-id="0868f-133">Operation to stop a BizTalk Service</span></span> |
| <span data-ttu-id="0868f-134">DisableBizTalkService</span><span class="sxs-lookup"><span data-stu-id="0868f-134">DisableBizTalkService</span></span> |<span data-ttu-id="0868f-135">Operace zakázání služby BizTalk</span><span class="sxs-lookup"><span data-stu-id="0868f-135">Operation to disable a BizTalk Service</span></span> |
| <span data-ttu-id="0868f-136">EnableBizTalkService</span><span class="sxs-lookup"><span data-stu-id="0868f-136">EnableBizTalkService</span></span> |<span data-ttu-id="0868f-137">Operaci povolení služby BizTalk</span><span class="sxs-lookup"><span data-stu-id="0868f-137">Operation to enable a BizTalk Service</span></span> |
| <span data-ttu-id="0868f-138">BackupBizTalkService</span><span class="sxs-lookup"><span data-stu-id="0868f-138">BackupBizTalkService</span></span> |<span data-ttu-id="0868f-139">Operace zálohování služby BizTalk</span><span class="sxs-lookup"><span data-stu-id="0868f-139">Operation to back up a BizTalk Service</span></span> |
| <span data-ttu-id="0868f-140">RestoreBizTalkService</span><span class="sxs-lookup"><span data-stu-id="0868f-140">RestoreBizTalkService</span></span> |<span data-ttu-id="0868f-141">Operace obnovení služby BizTalk ze zadané zálohy</span><span class="sxs-lookup"><span data-stu-id="0868f-141">Operation to restore a BizTalk Service from specified backup</span></span> |
| <span data-ttu-id="0868f-142">SuspendBizTalkService</span><span class="sxs-lookup"><span data-stu-id="0868f-142">SuspendBizTalkService</span></span> |<span data-ttu-id="0868f-143">Operace pozastavení služby BizTalk</span><span class="sxs-lookup"><span data-stu-id="0868f-143">Operation to suspend a BizTalk Service</span></span> |
| <span data-ttu-id="0868f-144">ResumeBizTalkService</span><span class="sxs-lookup"><span data-stu-id="0868f-144">ResumeBizTalkService</span></span> |<span data-ttu-id="0868f-145">Operace obnovení služby BizTalk</span><span class="sxs-lookup"><span data-stu-id="0868f-145">Operation to resume a BizTalk Service</span></span> |
| <span data-ttu-id="0868f-146">ScaleBizTalkService</span><span class="sxs-lookup"><span data-stu-id="0868f-146">ScaleBizTalkService</span></span> |<span data-ttu-id="0868f-147">Operace škálování služby BizTalk nahoru nebo dolů</span><span class="sxs-lookup"><span data-stu-id="0868f-147">Operation to scale a BizTalk Service up or down</span></span> |
| <span data-ttu-id="0868f-148">ConfigUpdateBizTalkService</span><span class="sxs-lookup"><span data-stu-id="0868f-148">ConfigUpdateBizTalkService</span></span> |<span data-ttu-id="0868f-149">Operace se aktualizovat konfiguraci služby BizTalk</span><span class="sxs-lookup"><span data-stu-id="0868f-149">Operation to update the configuration of a BizTalk Service</span></span> |
| <span data-ttu-id="0868f-150">ServiceUpdateBizTalkService</span><span class="sxs-lookup"><span data-stu-id="0868f-150">ServiceUpdateBizTalkService</span></span> |<span data-ttu-id="0868f-151">Operace k aktualizaci nebo starší verzi služby BizTalk na jinou verzi</span><span class="sxs-lookup"><span data-stu-id="0868f-151">Operation to upgrade or downgrade a BizTalk Service to a different version</span></span> |
| <span data-ttu-id="0868f-152">PurgeBackupBizTalkService</span><span class="sxs-lookup"><span data-stu-id="0868f-152">PurgeBackupBizTalkService</span></span> |<span data-ttu-id="0868f-153">Operace k vyprázdnění zálohování služby BizTalk mimo dobu uchování</span><span class="sxs-lookup"><span data-stu-id="0868f-153">Operation to purge backups of the BizTalk Service outside the retention period</span></span> |

## <a name="see-also"></a><span data-ttu-id="0868f-154">Viz také</span><span class="sxs-lookup"><span data-stu-id="0868f-154">See Also</span></span>
* [<span data-ttu-id="0868f-155">Zálohování služby BizTalk</span><span class="sxs-lookup"><span data-stu-id="0868f-155">Backup BizTalk Service</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=325584)
* [<span data-ttu-id="0868f-156">Služba BizTalk obnovit ze zálohy</span><span class="sxs-lookup"><span data-stu-id="0868f-156">Restore BizTalk Service from Backup</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=325582)
* [<span data-ttu-id="0868f-157">BizTalk Services: Developer, Basic, Standard a Premium tabulka edic</span><span class="sxs-lookup"><span data-stu-id="0868f-157">BizTalk Services: Developer, Basic, Standard and Premium Editions Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302279)
* [<span data-ttu-id="0868f-158">BizTalk Services: Zřízení pomocí Azure portál classic</span><span class="sxs-lookup"><span data-stu-id="0868f-158">BizTalk Services: Provisioning Using Azure classic portal</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302280)
* [<span data-ttu-id="0868f-159">BizTalk Services: Tabulka stavů zřízení</span><span class="sxs-lookup"><span data-stu-id="0868f-159">BizTalk Services: Provisioning Status Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=329870)
* [<span data-ttu-id="0868f-160">BizTalk Services: Karty Řídicí panel, Sledování a Škálování</span><span class="sxs-lookup"><span data-stu-id="0868f-160">BizTalk Services: Dashboard, Monitor and Scale tabs</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302281)
* [<span data-ttu-id="0868f-161">BizTalk Services: Omezování</span><span class="sxs-lookup"><span data-stu-id="0868f-161">BizTalk Services: Throttling</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302282)
* [<span data-ttu-id="0868f-162">BizTalk Services: Název a klíč vystavitele</span><span class="sxs-lookup"><span data-stu-id="0868f-162">BizTalk Services: Issuer Name and Issuer Key</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=303941)
* [<span data-ttu-id="0868f-163">Jak začít používat sadu SDK Azure BizTalk Services</span><span class="sxs-lookup"><span data-stu-id="0868f-163">How do I Start Using the Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[ViewLogs]: ./media/biztalk-troubleshoot-using-ops-logs/Operation-Logs.png

