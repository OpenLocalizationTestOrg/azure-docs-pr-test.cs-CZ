---
title: "Monitorování Azure Functions | Microsoft Docs"
description: "Zjistěte, jak monitorovat Azure Functions."
services: functions
documentationcenter: na
author: wesmc7777
manager: erikre
editor: 
tags: 
keywords: "funkce azure, funkce, zpracování událostí, webhook, dynamické výpočty, architektura bez serverů"
ms.assetid: 501722c3-f2f7-4224-a220-6d59da08a320
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/03/2016
ms.author: wesmc
ms.openlocfilehash: b70214593b1417265387f42306a633bb0df2920e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="monitoring-azure-functions"></a><span data-ttu-id="a061b-104">Monitorování služby Azure Functions</span><span class="sxs-lookup"><span data-stu-id="a061b-104">Monitoring Azure Functions</span></span>

## <a name="overview"></a><span data-ttu-id="a061b-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="a061b-105">Overview</span></span> 


<span data-ttu-id="a061b-106">**Monitorování** kartě pro jednotlivé funkce umožňuje zkontrolovat každé spuštění funkce.</span><span class="sxs-lookup"><span data-stu-id="a061b-106">The **Monitor** tab for each function allows you to review each execution of a function.</span></span>

![Karta sledovat Azure funkce](./media/functions-monitoring/monitor-tab.png) 

<span data-ttu-id="a061b-108">Kliknutím na tlačítko spuštění umožňuje zkontrolovat doba trvání, vstupních dat, chyby a přidružené soubory protokolu.</span><span class="sxs-lookup"><span data-stu-id="a061b-108">Clicking an execution allows you to review the duration, input data, errors, and associated log files.</span></span> <span data-ttu-id="a061b-109">To je užitečné, ladění a funkce optimalizace výkonu.</span><span class="sxs-lookup"><span data-stu-id="a061b-109">This is useful debugging and performance tuning your functions.</span></span>


> [!IMPORTANT]
> <span data-ttu-id="a061b-110">Při použití [spotřeba hostování plán](functions-overview.md#pricing) pro Azure Functions **monitorování** dlaždice v okně Přehled funkce aplikace nezobrazí se žádná data.</span><span class="sxs-lookup"><span data-stu-id="a061b-110">When using the [Consumption hosting plan](functions-overview.md#pricing) for Azure Functions, the **Monitoring** tile in the Function App overview blade will not show any data.</span></span> <span data-ttu-id="a061b-111">Je to proto platformou dynamicky Škáluje a spravuje výpočetní instance za vás, tak tyto metriky nejsou smysluplný na plánu spotřeby.</span><span class="sxs-lookup"><span data-stu-id="a061b-111">This is because the platform dynamically scales and manages compute instances for you, so these metrics are not meaningful on a Consumption plan.</span></span> <span data-ttu-id="a061b-112">Sledování využití funkce aplikací, by měl místo toho postupujte podle pokynů v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="a061b-112">To monitor the usage of your Function Apps, you should instead use the guidance in this article.</span></span>
> 
> <span data-ttu-id="a061b-113">Následující snímek obrazovky ukazuje příklad:</span><span class="sxs-lookup"><span data-stu-id="a061b-113">The following screen-shot shows an example:</span></span>
> 
> ![Monitorování v okně Hlavní prostředků](./media/functions-monitoring/app-service-overview-monitoring.png)



## <a name="real-time-monitoring"></a><span data-ttu-id="a061b-115">Sledování v reálném čase</span><span class="sxs-lookup"><span data-stu-id="a061b-115">Real-time monitoring</span></span>

<span data-ttu-id="a061b-116">Sledování v reálném čase je k dispozici kliknutím **živé události datového proudu** jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="a061b-116">Real-time monitoring is available by clicking **live event stream** as shown below.</span></span> 

![Možnost živé události datového proudu pro kartě monitorování](./media/functions-monitoring/monitor-tab-live-event-stream.png)

<span data-ttu-id="a061b-118">Datový proud živé události bude být proto vytvořen na nové záložce prohlížeče, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="a061b-118">The live event stream will be graphed in a new browser tab as shown below.</span></span> 

![Příklad živé události datového proudu](./media/functions-monitoring/live-event-stream.png)


> [!NOTE]
> <span data-ttu-id="a061b-120">Je známý problém, který může způsobit, že dat, aby se nepodařilo načíst.</span><span class="sxs-lookup"><span data-stu-id="a061b-120">There is a known issue that may cause your data to fail to be populated.</span></span> <span data-ttu-id="a061b-121">Pokud narazíte na to, budete muset zavřete kartu prohlížeče obsahující datový proud živé události a pak klikněte na tlačítko **živé události datového proudu** znovu tak, aby ji správně naplnění dat událostí datového proudu.</span><span class="sxs-lookup"><span data-stu-id="a061b-121">If you experience this, you may need to close the browser tab containing the live event stream and then click **live event stream** again to allow it to properly populate your event stream data.</span></span> 

<span data-ttu-id="a061b-122">Datový proud živé události bude graf statistiku následující funkce:</span><span class="sxs-lookup"><span data-stu-id="a061b-122">The live event stream will graph the following statistics for your function:</span></span>

* <span data-ttu-id="a061b-123">Spuštěních spuštěných za sekundu</span><span class="sxs-lookup"><span data-stu-id="a061b-123">Executions started per second</span></span>
* <span data-ttu-id="a061b-124">Spuštěních dokončit za jednu sekundu</span><span class="sxs-lookup"><span data-stu-id="a061b-124">Executions completed per second</span></span>
* <span data-ttu-id="a061b-125">Spuštění se nezdařilo za sekundu</span><span class="sxs-lookup"><span data-stu-id="a061b-125">Executions failed per second</span></span>
* <span data-ttu-id="a061b-126">Průměrná doba provádění v milisekundách.</span><span class="sxs-lookup"><span data-stu-id="a061b-126">Average execution time in milliseconds.</span></span>

<span data-ttu-id="a061b-127">Tyto statistické údaje jsou v reálném čase, ale skutečný vytváření grafů data provádění pravděpodobně přibližně 10 sekund latence.</span><span class="sxs-lookup"><span data-stu-id="a061b-127">These statistics are real-time but the actual graphing of the execution data may have around 10 seconds of latency.</span></span>






## <a name="monitoring-log-files-from-a-command-line"></a><span data-ttu-id="a061b-128">Monitorování soubory protokolů z příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="a061b-128">Monitoring log files from a command line</span></span>


<span data-ttu-id="a061b-129">Soubory protokolu na relaci příkazového řádku na místní pracovní stanici, pomocí rozhraní příkazového řádku Azure (CLI) nebo prostředí PowerShell můžete datového proudu.</span><span class="sxs-lookup"><span data-stu-id="a061b-129">You can stream log files to a command line session on a local workstation using the Azure Command Line Interface (CLI) or PowerShell.</span></span>

### <a name="monitoring-function-app-log-files-with-the-azure-cli"></a><span data-ttu-id="a061b-130">Monitorování souborů protokolů pomocí Azure CLI funkce aplikace</span><span class="sxs-lookup"><span data-stu-id="a061b-130">Monitoring function app log files with the Azure CLI</span></span>

<span data-ttu-id="a061b-131">Abyste mohli začít, [nainstalovat rozhraní příkazového řádku Azure](../cli-install-nodejs.md)</span><span class="sxs-lookup"><span data-stu-id="a061b-131">To get started, [install the Azure CLI](../cli-install-nodejs.md)</span></span>

<span data-ttu-id="a061b-132">Přihlaste se k účtu Azure, pomocí následujícího příkazu nebo libovolné jiné možnosti nezabývá, [přihlášení k Azure z rozhraní příkazového řádku Azure](../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="a061b-132">Log into your Azure account using the following command, or any of the other options covered in, [Log in to Azure from the Azure CLI](../xplat-cli-connect.md).</span></span>

    azure login

<span data-ttu-id="a061b-133">Pokud chcete povolit režim Azure CLI Service Management (ASM) použijte následující příkaz:.</span><span class="sxs-lookup"><span data-stu-id="a061b-133">Use the following command to enable Azure CLI Service Management (ASM) mode:.</span></span>

    azure config mode asm

<span data-ttu-id="a061b-134">Pokud máte více předplatných, použijte následující příkazy seznam předplatných a nastavit aktuální předplatné na odběr, který obsahuje funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="a061b-134">If you have multiple subscriptions, use the following commands to list your subscriptions and set the current subscription to the subscription that contains your function app.</span></span>

    azure account list
    azure account set <subscriptionNameOrId>

<span data-ttu-id="a061b-135">Následující příkaz bude stream souborů protokolu v aplikaci funkce příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="a061b-135">The following command will stream the log files of your function app to the command line:</span></span>

    azure site log tail -v <function app name>

### <a name="monitoring-function-app-log-files-with-powershell"></a><span data-ttu-id="a061b-136">Monitorování souborů protokolů funkce aplikace pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="a061b-136">Monitoring function app log files with PowerShell</span></span>

<span data-ttu-id="a061b-137">Abyste mohli začít, [instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a061b-137">To get started, [install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="a061b-138">Přidání účtu Azure tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a061b-138">Add your Azure account by running the following command:</span></span>

    PS C:\> Add-AzureAccount

<span data-ttu-id="a061b-139">Pokud máte více předplatných, můžete je seznam podle názvu pomocí následujícího příkazu zobrazíte, pokud správné předplatné není aktuálně vybrané na základě `IsCurrent` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="a061b-139">If you have multiple subscriptions, you can list them by name with the following command to see if the correct subscription is the currently selected based on `IsCurrent` property:</span></span>

    PS C:\> Get-AzureSubscription

<span data-ttu-id="a061b-140">Pokud potřebujete nastavit aktivní předplatné na jeden obsahující funkce aplikace, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a061b-140">If you need to set the active subscription to the one containing your function app, use the following command:</span></span>

    PS C:\> Get-AzureSubscription -SubscriptionName "MyFunctionAppSubscription" | Select-AzureSubscription

<span data-ttu-id="a061b-141">Stream protokoly do relace prostředí PowerShell pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="a061b-141">Stream the logs to your PowerShell session with the following command:</span></span>

    PS C:\> Get-AzureWebSiteLog -Name MyFunctionApp -Tail

<span data-ttu-id="a061b-142">Další informace najdete v části [postup: Stream protokoly pro webové aplikace](../app-service-web/web-sites-enable-diagnostic-log.md#streamlogs).</span><span class="sxs-lookup"><span data-stu-id="a061b-142">For more information refer to [How to: Stream logs for web apps](../app-service-web/web-sites-enable-diagnostic-log.md#streamlogs).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="a061b-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a061b-143">Next steps</span></span>
<span data-ttu-id="a061b-144">Další informace najdete v následujících materiálech:</span><span class="sxs-lookup"><span data-stu-id="a061b-144">For more information, see the following resources:</span></span>

* [<span data-ttu-id="a061b-145">Testování funkce</span><span class="sxs-lookup"><span data-stu-id="a061b-145">Testing a function</span></span>](functions-test-a-function.md)
* [<span data-ttu-id="a061b-146">Škálování funkce</span><span class="sxs-lookup"><span data-stu-id="a061b-146">Scale a function</span></span>](functions-scale.md)

