---
title: "tok hello aaaTrace v cloudových služeb aplikací s Azure Diagnostics | Microsoft Docs"
description: "Přidejte trasování zprávy tooan aplikaci Azure toohelp ladění, měření výkonu, monitorování, analýza provozu a další."
services: cloud-services
documentationcenter: .net
author: rboucher
manager: jwhit
editor: 
ms.assetid: 09934772-cc07-4fd2-ba88-b224ca192f8e
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/20/2016
ms.author: robb
ms.openlocfilehash: d2ed7b5997ae1d298115b4ce593bb5051a9a0c75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="trace-hello-flow-of-a-cloud-services-application-with-azure-diagnostics"></a>Sledování toku hello aplikace cloudových služeb s Azure Diagnostics
Trasování je způsob, jak můžete toomonitor hello provádění aplikace, když je spuštěná. Můžete použít hello [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx), [System.Diagnostics.Debug](https://msdn.microsoft.com/library/system.diagnostics.debug.aspx), a [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) třídy toorecord informace o chybách a spuštění aplikace v protokolech, textové soubory nebo jiné zařízení pro pozdější analýzu. Další informace o trasování najdete v tématu [trasování a instrumentace aplikací](https://msdn.microsoft.com/library/zs6s4h68.aspx).

## <a name="use-trace-statements-and-trace-switches"></a>Pomocí příkazů trasování a trasování – přepínače
Implementace trasování v aplikaci cloudové služby přidáním hello [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) toohello konfiguraci aplikací a provádění volá tooSystem.Diagnostics.Trace nebo System.Diagnostics.Debug ve vaší kód aplikace. Použití hello konfigurační soubor *app.config* rolí pracovního procesu a hello *web.config* pro webové role. Když vytvoříte novou hostovanou službu pomocí šablony sady Visual Studio, Azure Diagnostics se automaticky přidá toohello projektu a hello DiagnosticMonitorTraceListener je přidána toohello odpovídající konfigurační soubor pro hello role, které přidáte.

Informace o umístění trasovacích příkazů najdete v tématu [postupy: Přidání příkazů trasování tooApplication kód](https://msdn.microsoft.com/library/zd83saa2.aspx).

Tím, že umístíte [trasování – přepínače](https://msdn.microsoft.com/library/3at424ac.aspx) ve vašem kódu můžete ovládat, zda dojde k trasování a jak rozsáhlé je. To vám umožňuje monitorovat stav hello vaší aplikace v provozním prostředí. To je obzvláště důležité v obchodní aplikace, která používá několik součástí běžící na více počítačích. Další informace najdete v tématu [postupy: Konfigurace trasování – přepínače](https://msdn.microsoft.com/library/t06xyy08.aspx).

## <a name="configure-hello-trace-listener-in-an-azure-application"></a>Konfigurace naslouchací proces trasování hello v aplikaci Azure
Trasování ladění a TraceSource, musíte nastavit toocollect "naslouchací procesy" a záznamů hello zpráv, které jsou odeslány. Naslouchací procesy shromažďování, ukládání a směrovat trasovací zprávy. Budou směrovat hello trasování výstup tooan příslušná cílová, jako je například protokol, okno nebo textového souboru. Azure Diagnostics používá hello [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) třídy.

Ještě před dokončením hello postupem je nutné inicializovat hello Azure monitorování diagnostiky. toodo, najdete v tématu [povolení diagnostiky v Microsoft Azure](cloud-services-dotnet-diagnostics.md).

Všimněte si, že pokud používáte hello šablony, které jsou k dispozici Visual Studio, konfigurace hello hello naslouchacího procesu se automaticky přidá za vás.

### <a name="add-a-trace-listener"></a>Přidejte naslouchací proces trasování
1. Otevřete soubor web.config nebo app.config hello pro vaši roli.
2. Přidejte následující kód toohello soubor hello. Změňte hello atribut toouse hello verze číslo verze sestavení hello, na které je odkazováno na. verze sestavení Hello nezmění nutně při každém vydání sady Azure SDK Pokud tooit aktualizace.
   
    ```
    <system.diagnostics>
        <trace>
            <listeners>
                <add type="Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener,
                  Microsoft.WindowsAzure.Diagnostics,
                  Version=2.8.0.0,
                  Culture=neutral,
                  PublicKeyToken=31bf3856ad364e35"
                  name="AzureDiagnostics">
                    <filter type="" />
                </add>
            </listeners>
        </trace>
    </system.diagnostics>
    ```
   > [!IMPORTANT]
   > Ujistěte se, že máte projektu odkaz toohello Microsoft.WindowsAzure.Diagnostics sestavení. Číslo verze aktualizace hello v xml hello výše toomatch hello verzi hello odkazovat Microsoft.WindowsAzure.Diagnostics sestavení.
   > 
   > 
3. Uložte soubor konfigurace hello.

Další informace o naslouchací procesy najdete v tématu [trasování – moduly naslouchání](https://msdn.microsoft.com/library/4y5y10s7.aspx).

Po dokončení hello kroky tooadd hello naslouchací proces, můžete přidat kód tooyour příkazy trasování.

### <a name="tooadd-trace-statement-tooyour-code"></a>kód pro příkaz tooyour tooadd trasování
1. Otevřete zdrojový soubor pro vaši aplikaci. Například hello <RoleName>soubor cs pro roli pracovního procesu hello nebo webové role.
2. Přidejte následující hello pomocí příkazu, pokud již nebyly přidané:
    ```
        using System.Diagnostics;
    ```
3. Přidání příkazů trasování, kam chcete toocapture informace o stavu hello vaší aplikace. Můžete použít různé metody tooformat hello výstup hello příkaz trasování. Další informace najdete v tématu [postupy: Přidání příkazů trasování tooApplication kód](https://msdn.microsoft.com/library/zd83saa2.aspx).
4. Uložte hello zdrojový soubor.

