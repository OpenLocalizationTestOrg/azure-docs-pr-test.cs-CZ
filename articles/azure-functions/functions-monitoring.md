---
title: aaaMonitoring Azure Functions | Microsoft Docs
description: "Zjistěte, jak toomonitor Azure Functions."
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
ms.openlocfilehash: 254348d1cefce925654bd9532715b6def571e0ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-azure-functions"></a><span data-ttu-id="866f4-104">Monitorování služby Azure Functions</span><span class="sxs-lookup"><span data-stu-id="866f4-104">Monitoring Azure Functions</span></span>

## <a name="overview"></a><span data-ttu-id="866f4-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="866f4-105">Overview</span></span> 


<span data-ttu-id="866f4-106">Hello **monitorování** kartě pro jednotlivé funkce vám umožní tooreview každé spuštění funkce.</span><span class="sxs-lookup"><span data-stu-id="866f4-106">hello **Monitor** tab for each function allows you tooreview each execution of a function.</span></span>

![Karta sledovat Azure funkce](./media/functions-monitoring/monitor-tab.png) 

<span data-ttu-id="866f4-108">Kliknutím na tlačítko spuštění umožňuje vám tooreview hello doba trvání, vstupních dat, chyb a přidružené soubory protokolu.</span><span class="sxs-lookup"><span data-stu-id="866f4-108">Clicking an execution allows you tooreview hello duration, input data, errors, and associated log files.</span></span> <span data-ttu-id="866f4-109">To je užitečné, ladění a funkce optimalizace výkonu.</span><span class="sxs-lookup"><span data-stu-id="866f4-109">This is useful debugging and performance tuning your functions.</span></span>


> [!IMPORTANT]
> <span data-ttu-id="866f4-110">Při použití hello [spotřeba hostování plán](functions-overview.md#pricing) Azure Functions hello **monitorování** dlaždice v okně Přehled funkce aplikace hello nezobrazí se žádná data.</span><span class="sxs-lookup"><span data-stu-id="866f4-110">When using hello [Consumption hosting plan](functions-overview.md#pricing) for Azure Functions, hello **Monitoring** tile in hello Function App overview blade will not show any data.</span></span> <span data-ttu-id="866f4-111">Je to proto hello platformy dynamicky Škáluje a spravuje výpočetní instance za vás, tak tyto metriky nejsou smysluplný na plánu spotřeby.</span><span class="sxs-lookup"><span data-stu-id="866f4-111">This is because hello platform dynamically scales and manages compute instances for you, so these metrics are not meaningful on a Consumption plan.</span></span> <span data-ttu-id="866f4-112">toomonitor hello využití funkce aplikací, měli byste místo toho použít hello pokyny v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="866f4-112">toomonitor hello usage of your Function Apps, you should instead use hello guidance in this article.</span></span>
> 
> <span data-ttu-id="866f4-113">Hello následující snímek obrazovky ukazuje příklad:</span><span class="sxs-lookup"><span data-stu-id="866f4-113">hello following screen-shot shows an example:</span></span>
> 
> ![Monitorování v okně Hlavní prostředků hello](./media/functions-monitoring/app-service-overview-monitoring.png)



## <a name="real-time-monitoring"></a><span data-ttu-id="866f4-115">Sledování v reálném čase</span><span class="sxs-lookup"><span data-stu-id="866f4-115">Real-time monitoring</span></span>

<span data-ttu-id="866f4-116">Sledování v reálném čase je k dispozici kliknutím **živé události datového proudu** jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="866f4-116">Real-time monitoring is available by clicking **live event stream** as shown below.</span></span> 

![Možnost datový proud události hello monitorování karta za provozu](./media/functions-monitoring/monitor-tab-live-event-stream.png)

<span data-ttu-id="866f4-118">datový proud Hello živé události bude být proto vytvořen na nové záložce prohlížeče, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="866f4-118">hello live event stream will be graphed in a new browser tab as shown below.</span></span> 

![Příklad živé události datového proudu](./media/functions-monitoring/live-event-stream.png)


> [!NOTE]
> <span data-ttu-id="866f4-120">Je známý problém, může způsobit, že vaše data toofail toobe naplněno.</span><span class="sxs-lookup"><span data-stu-id="866f4-120">There is a known issue that may cause your data toofail toobe populated.</span></span> <span data-ttu-id="866f4-121">Pokud to setkáte, může být nutné tooclose hello prohlížeče karta obsahující hello živé události datového proudu a pak klikněte na tlačítko **živé události datového proudu** znovu tooallow ho tooproperly naplnění dat událostí datového proudu.</span><span class="sxs-lookup"><span data-stu-id="866f4-121">If you experience this, you may need tooclose hello browser tab containing hello live event stream and then click **live event stream** again tooallow it tooproperly populate your event stream data.</span></span> 

<span data-ttu-id="866f4-122">datový proud Hello živé události bude graf hello následující statistiky pro funkce:</span><span class="sxs-lookup"><span data-stu-id="866f4-122">hello live event stream will graph hello following statistics for your function:</span></span>

* <span data-ttu-id="866f4-123">Spuštěních spuštěných za sekundu</span><span class="sxs-lookup"><span data-stu-id="866f4-123">Executions started per second</span></span>
* <span data-ttu-id="866f4-124">Spuštěních dokončit za jednu sekundu</span><span class="sxs-lookup"><span data-stu-id="866f4-124">Executions completed per second</span></span>
* <span data-ttu-id="866f4-125">Spuštění se nezdařilo za sekundu</span><span class="sxs-lookup"><span data-stu-id="866f4-125">Executions failed per second</span></span>
* <span data-ttu-id="866f4-126">Průměrná doba provádění v milisekundách.</span><span class="sxs-lookup"><span data-stu-id="866f4-126">Average execution time in milliseconds.</span></span>

<span data-ttu-id="866f4-127">Tyto statistické údaje jsou v reálném čase, ale hello skutečné vytváření grafů hello provádění dat pravděpodobně přibližně 10 sekund latence.</span><span class="sxs-lookup"><span data-stu-id="866f4-127">These statistics are real-time but hello actual graphing of hello execution data may have around 10 seconds of latency.</span></span>






## <a name="monitoring-log-files-from-a-command-line"></a><span data-ttu-id="866f4-128">Monitorování soubory protokolů z příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="866f4-128">Monitoring log files from a command line</span></span>


<span data-ttu-id="866f4-129">Proudy relaci příkazového řádku tooa soubory protokolu na místní pracovní stanici pomocí hello rozhraní příkazového řádku Azure (CLI) nebo prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="866f4-129">You can stream log files tooa command line session on a local workstation using hello Azure Command Line Interface (CLI) or PowerShell.</span></span>

### <a name="monitoring-function-app-log-files-with-hello-azure-cli"></a><span data-ttu-id="866f4-130">Monitorování souborů protokolů aplikace funkce s hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="866f4-130">Monitoring function app log files with hello Azure CLI</span></span>

<span data-ttu-id="866f4-131">spuštění, tooget [nainstalovat hello rozhraní příkazového řádku Azure](../cli-install-nodejs.md)</span><span class="sxs-lookup"><span data-stu-id="866f4-131">tooget started, [install hello Azure CLI](../cli-install-nodejs.md)</span></span>

<span data-ttu-id="866f4-132">Přihlaste se k účtu Azure pomocí hello následující příkaz, nebo libovolná z hello jiné možnosti popsané v, [přihlásit tooAzure z příkazového řádku Azure CLI hello](../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="866f4-132">Log into your Azure account using hello following command, or any of hello other options covered in, [Log in tooAzure from hello Azure CLI](../xplat-cli-connect.md).</span></span>

    azure login

<span data-ttu-id="866f4-133">Použití hello následující příkaz tooenable Azure CLI Service Management (ASM) režim:.</span><span class="sxs-lookup"><span data-stu-id="866f4-133">Use hello following command tooenable Azure CLI Service Management (ASM) mode:.</span></span>

    azure config mode asm

<span data-ttu-id="866f4-134">Pokud máte více předplatných, použijte následující příkazy toolist hello předplatných a sadu hello aktuální předplatné toohello předplatné, které obsahuje funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="866f4-134">If you have multiple subscriptions, use hello following commands toolist your subscriptions and set hello current subscription toohello subscription that contains your function app.</span></span>

    azure account list
    azure account set <subscriptionNameOrId>

<span data-ttu-id="866f4-135">Hello následující příkaz bude stream hello souborů protokolu do funkce aplikace toohello příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="866f4-135">hello following command will stream hello log files of your function app toohello command line:</span></span>

    azure site log tail -v <function app name>

### <a name="monitoring-function-app-log-files-with-powershell"></a><span data-ttu-id="866f4-136">Monitorování souborů protokolů funkce aplikace pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="866f4-136">Monitoring function app log files with PowerShell</span></span>

<span data-ttu-id="866f4-137">spuštění, tooget [instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="866f4-137">tooget started, [install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="866f4-138">Přidání účtu Azure tak, že spustíte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="866f4-138">Add your Azure account by running hello following command:</span></span>

    PS C:\> Add-AzureAccount

<span data-ttu-id="866f4-139">Pokud máte více předplatných, můžete vytvořit seznam je podle názvu s hello následující příkaz toosee, pokud na základě hello správné předplatné je hello aktuálně vybrané `IsCurrent` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="866f4-139">If you have multiple subscriptions, you can list them by name with hello following command toosee if hello correct subscription is hello currently selected based on `IsCurrent` property:</span></span>

    PS C:\> Get-AzureSubscription

<span data-ttu-id="866f4-140">Pokud potřebujete tooset hello aktivní předplatné toohello jeden obsahující funkce aplikace, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="866f4-140">If you need tooset hello active subscription toohello one containing your function app, use hello following command:</span></span>

    PS C:\> Get-AzureSubscription -SubscriptionName "MyFunctionAppSubscription" | Select-AzureSubscription

<span data-ttu-id="866f4-141">Datový proud hello protokolů tooyour relaci prostředí PowerShell s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="866f4-141">Stream hello logs tooyour PowerShell session with hello following command:</span></span>

    PS C:\> Get-AzureWebSiteLog -Name MyFunctionApp -Tail

<span data-ttu-id="866f4-142">Další informace najdete v části příliš[postup: Stream protokoly pro webové aplikace](../app-service-web/web-sites-enable-diagnostic-log.md#streamlogs).</span><span class="sxs-lookup"><span data-stu-id="866f4-142">For more information refer too[How to: Stream logs for web apps](../app-service-web/web-sites-enable-diagnostic-log.md#streamlogs).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="866f4-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="866f4-143">Next steps</span></span>
<span data-ttu-id="866f4-144">Další informace najdete v tématu hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="866f4-144">For more information, see hello following resources:</span></span>

* [<span data-ttu-id="866f4-145">Testování funkce</span><span class="sxs-lookup"><span data-stu-id="866f4-145">Testing a function</span></span>](functions-test-a-function.md)
* [<span data-ttu-id="866f4-146">Škálování funkce</span><span class="sxs-lookup"><span data-stu-id="866f4-146">Scale a function</span></span>](functions-scale.md)

