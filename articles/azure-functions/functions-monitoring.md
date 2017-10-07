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
# <a name="monitoring-azure-functions"></a>Monitorování služby Azure Functions

## <a name="overview"></a>Přehled 


Hello **monitorování** kartě pro jednotlivé funkce vám umožní tooreview každé spuštění funkce.

![Karta sledovat Azure funkce](./media/functions-monitoring/monitor-tab.png) 

Kliknutím na tlačítko spuštění umožňuje vám tooreview hello doba trvání, vstupních dat, chyb a přidružené soubory protokolu. To je užitečné, ladění a funkce optimalizace výkonu.


> [!IMPORTANT]
> Při použití hello [spotřeba hostování plán](functions-overview.md#pricing) Azure Functions hello **monitorování** dlaždice v okně Přehled funkce aplikace hello nezobrazí se žádná data. Je to proto hello platformy dynamicky Škáluje a spravuje výpočetní instance za vás, tak tyto metriky nejsou smysluplný na plánu spotřeby. toomonitor hello využití funkce aplikací, měli byste místo toho použít hello pokyny v tomto článku.
> 
> Hello následující snímek obrazovky ukazuje příklad:
> 
> ![Monitorování v okně Hlavní prostředků hello](./media/functions-monitoring/app-service-overview-monitoring.png)



## <a name="real-time-monitoring"></a>Sledování v reálném čase

Sledování v reálném čase je k dispozici kliknutím **živé události datového proudu** jak je uvedeno níže. 

![Možnost datový proud události hello monitorování karta za provozu](./media/functions-monitoring/monitor-tab-live-event-stream.png)

datový proud Hello živé události bude být proto vytvořen na nové záložce prohlížeče, jak je uvedeno níže. 

![Příklad živé události datového proudu](./media/functions-monitoring/live-event-stream.png)


> [!NOTE]
> Je známý problém, může způsobit, že vaše data toofail toobe naplněno. Pokud to setkáte, může být nutné tooclose hello prohlížeče karta obsahující hello živé události datového proudu a pak klikněte na tlačítko **živé události datového proudu** znovu tooallow ho tooproperly naplnění dat událostí datového proudu. 

datový proud Hello živé události bude graf hello následující statistiky pro funkce:

* Spuštěních spuštěných za sekundu
* Spuštěních dokončit za jednu sekundu
* Spuštění se nezdařilo za sekundu
* Průměrná doba provádění v milisekundách.

Tyto statistické údaje jsou v reálném čase, ale hello skutečné vytváření grafů hello provádění dat pravděpodobně přibližně 10 sekund latence.






## <a name="monitoring-log-files-from-a-command-line"></a>Monitorování soubory protokolů z příkazového řádku


Proudy relaci příkazového řádku tooa soubory protokolu na místní pracovní stanici pomocí hello rozhraní příkazového řádku Azure (CLI) nebo prostředí PowerShell.

### <a name="monitoring-function-app-log-files-with-hello-azure-cli"></a>Monitorování souborů protokolů aplikace funkce s hello rozhraní příkazového řádku Azure

spuštění, tooget [nainstalovat hello rozhraní příkazového řádku Azure](../cli-install-nodejs.md)

Přihlaste se k účtu Azure pomocí hello následující příkaz, nebo libovolná z hello jiné možnosti popsané v, [přihlásit tooAzure z příkazového řádku Azure CLI hello](../xplat-cli-connect.md).

    azure login

Použití hello následující příkaz tooenable Azure CLI Service Management (ASM) režim:.

    azure config mode asm

Pokud máte více předplatných, použijte následující příkazy toolist hello předplatných a sadu hello aktuální předplatné toohello předplatné, které obsahuje funkce aplikace.

    azure account list
    azure account set <subscriptionNameOrId>

Hello následující příkaz bude stream hello souborů protokolu do funkce aplikace toohello příkazového řádku:

    azure site log tail -v <function app name>

### <a name="monitoring-function-app-log-files-with-powershell"></a>Monitorování souborů protokolů funkce aplikace pomocí prostředí PowerShell

spuštění, tooget [instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

Přidání účtu Azure tak, že spustíte následující příkaz hello:

    PS C:\> Add-AzureAccount

Pokud máte více předplatných, můžete vytvořit seznam je podle názvu s hello následující příkaz toosee, pokud na základě hello správné předplatné je hello aktuálně vybrané `IsCurrent` vlastnost:

    PS C:\> Get-AzureSubscription

Pokud potřebujete tooset hello aktivní předplatné toohello jeden obsahující funkce aplikace, použijte následující příkaz hello:

    PS C:\> Get-AzureSubscription -SubscriptionName "MyFunctionAppSubscription" | Select-AzureSubscription

Datový proud hello protokolů tooyour relaci prostředí PowerShell s hello následující příkaz:

    PS C:\> Get-AzureWebSiteLog -Name MyFunctionApp -Tail

Další informace najdete v části příliš[postup: Stream protokoly pro webové aplikace](../app-service-web/web-sites-enable-diagnostic-log.md#streamlogs). 

## <a name="next-steps"></a>Další kroky
Další informace najdete v tématu hello následující prostředky:

* [Testování funkce](functions-test-a-function.md)
* [Škálování funkce](functions-scale.md)

