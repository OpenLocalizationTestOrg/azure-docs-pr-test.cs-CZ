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
# <a name="monitoring-azure-functions"></a>Monitorování služby Azure Functions

## <a name="overview"></a>Přehled 


**Monitorování** kartě pro jednotlivé funkce umožňuje zkontrolovat každé spuštění funkce.

![Karta sledovat Azure funkce](./media/functions-monitoring/monitor-tab.png) 

Kliknutím na tlačítko spuštění umožňuje zkontrolovat doba trvání, vstupních dat, chyby a přidružené soubory protokolu. To je užitečné, ladění a funkce optimalizace výkonu.


> [!IMPORTANT]
> Při použití [spotřeba hostování plán](functions-overview.md#pricing) pro Azure Functions **monitorování** dlaždice v okně Přehled funkce aplikace nezobrazí se žádná data. Je to proto platformou dynamicky Škáluje a spravuje výpočetní instance za vás, tak tyto metriky nejsou smysluplný na plánu spotřeby. Sledování využití funkce aplikací, by měl místo toho postupujte podle pokynů v tomto článku.
> 
> Následující snímek obrazovky ukazuje příklad:
> 
> ![Monitorování v okně Hlavní prostředků](./media/functions-monitoring/app-service-overview-monitoring.png)



## <a name="real-time-monitoring"></a>Sledování v reálném čase

Sledování v reálném čase je k dispozici kliknutím **živé události datového proudu** jak je uvedeno níže. 

![Možnost živé události datového proudu pro kartě monitorování](./media/functions-monitoring/monitor-tab-live-event-stream.png)

Datový proud živé události bude být proto vytvořen na nové záložce prohlížeče, jak je uvedeno níže. 

![Příklad živé události datového proudu](./media/functions-monitoring/live-event-stream.png)


> [!NOTE]
> Je známý problém, který může způsobit, že dat, aby se nepodařilo načíst. Pokud narazíte na to, budete muset zavřete kartu prohlížeče obsahující datový proud živé události a pak klikněte na tlačítko **živé události datového proudu** znovu tak, aby ji správně naplnění dat událostí datového proudu. 

Datový proud živé události bude graf statistiku následující funkce:

* Spuštěních spuštěných za sekundu
* Spuštěních dokončit za jednu sekundu
* Spuštění se nezdařilo za sekundu
* Průměrná doba provádění v milisekundách.

Tyto statistické údaje jsou v reálném čase, ale skutečný vytváření grafů data provádění pravděpodobně přibližně 10 sekund latence.






## <a name="monitoring-log-files-from-a-command-line"></a>Monitorování soubory protokolů z příkazového řádku


Soubory protokolu na relaci příkazového řádku na místní pracovní stanici, pomocí rozhraní příkazového řádku Azure (CLI) nebo prostředí PowerShell můžete datového proudu.

### <a name="monitoring-function-app-log-files-with-the-azure-cli"></a>Monitorování souborů protokolů pomocí Azure CLI funkce aplikace

Abyste mohli začít, [nainstalovat rozhraní příkazového řádku Azure](../cli-install-nodejs.md)

Přihlaste se k účtu Azure, pomocí následujícího příkazu nebo libovolné jiné možnosti nezabývá, [přihlášení k Azure z rozhraní příkazového řádku Azure](../xplat-cli-connect.md).

    azure login

Pokud chcete povolit režim Azure CLI Service Management (ASM) použijte následující příkaz:.

    azure config mode asm

Pokud máte více předplatných, použijte následující příkazy seznam předplatných a nastavit aktuální předplatné na odběr, který obsahuje funkce aplikace.

    azure account list
    azure account set <subscriptionNameOrId>

Následující příkaz bude stream souborů protokolu v aplikaci funkce příkazového řádku:

    azure site log tail -v <function app name>

### <a name="monitoring-function-app-log-files-with-powershell"></a>Monitorování souborů protokolů funkce aplikace pomocí prostředí PowerShell

Abyste mohli začít, [instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

Přidání účtu Azure tak, že spustíte následující příkaz:

    PS C:\> Add-AzureAccount

Pokud máte více předplatných, můžete je seznam podle názvu pomocí následujícího příkazu zobrazíte, pokud správné předplatné není aktuálně vybrané na základě `IsCurrent` vlastnost:

    PS C:\> Get-AzureSubscription

Pokud potřebujete nastavit aktivní předplatné na jeden obsahující funkce aplikace, použijte následující příkaz:

    PS C:\> Get-AzureSubscription -SubscriptionName "MyFunctionAppSubscription" | Select-AzureSubscription

Stream protokoly do relace prostředí PowerShell pomocí následujícího příkazu:

    PS C:\> Get-AzureWebSiteLog -Name MyFunctionApp -Tail

Další informace najdete v části [postup: Stream protokoly pro webové aplikace](../app-service-web/web-sites-enable-diagnostic-log.md#streamlogs). 

## <a name="next-steps"></a>Další kroky
Další informace najdete v následujících materiálech:

* [Testování funkce](functions-test-a-function.md)
* [Škálování funkce](functions-scale.md)

