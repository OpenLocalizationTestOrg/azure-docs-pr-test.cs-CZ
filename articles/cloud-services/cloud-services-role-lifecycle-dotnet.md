---
title: "události životního cyklu Cloudová služba aaaHandle | Microsoft Docs"
description: "Zjistěte, jak lze použít metody životního cyklu hello Cloudová služba role v rozhraní .NET"
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 39b30acd-57b9-48b7-a7c4-40ea3430e451
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: cc0ccc5f055b965202b6e081a6ab72ad5d39b034
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="customize-hello-lifecycle-of-a-web-or-worker-role-in-net"></a>Přizpůsobení hello životního cyklu Web nebo Worker role v rozhraní .NET
Když vytvoříte roli pracovního procesu, můžete rozšířit hello [RoleEntryPoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx) třída, která poskytuje metody pro vás toooverride, který vám umožní reagovat toolifecycle události. Pro webové role Tato třída je volitelné, takže je nutné ji toorespond toolifecycle události.

## <a name="extend-hello-roleentrypoint-class"></a>Rozšíření třídy RoleEntryPoint hello
Hello [RoleEntryPoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx) třída obsahuje metody, které se nazývají Azure při jeho **spustí**, **spouští**, nebo **zastaví** roli web nebo worker. Volitelně můžete přepsat tyto metody toomanage role inicializace, role vypnutí pořadí nebo hello prováděcí vlákno hello role. 

Při rozšiřování **RoleEntryPoint**, byste měli být vědomi následujících chování metod hello hello:

* Hello [OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) a [OnStop](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) metody vrátit logickou hodnotu, takže je možné tooreturn **false** z těchto metod.
  
   Pokud váš kód vrátí **false**neočekávaného ukončení procesu hello role, bez spuštění jakékoli pořadí vypnutí můžete mít na místě. Obecně platí, neměli byste vrácení **false** z hello **OnStart** metoda.
* Žádné nezachycená v rámci přetížení **RoleEntryPoint** metoda je považován za k neošetřené výjimce.
  
   Pokud dojde k výjimce v rámci jedné z metod hello životního cyklu, Azure vyvolá hello [UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx) události, a pak hello proces bude ukončen. Po vaše role je v režimu offline, se restartuje v Azure. Když dojde k neošetřené výjimce, hello [zastavení](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.stopping.aspx) událostí není vyvolána a hello **OnStop** metoda není volána.

Pokud vaše role se nespustí, nebo je recyklace mezi hello inicializace zaneprázdněn a ukončení stav, může váš kód vyvolání k neošetřené výjimce v rámci jedné události životního cyklu hello, každou roli hello čas se restartuje. V takovém případě použijte hello [UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx) událostí toodetermine hello příčinou hello výjimky a správně zpracovat. Vaše role může být také vrácení z hello [spustit](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metoda, což způsobí, že hello role toorestart. Další informace o stavy nasazení najdete v tématu [běžné problémy které příčina role tooRecycle](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md).

> [!NOTE]
> Pokud používáte hello **nástroje Azure pro sadu Microsoft Visual Studio** toodevelop vaší aplikace, šablony projektů hello role automaticky rozšířit hello **RoleEntryPoint** třídy, v hello *WebRole.cs* a *WorkerRole.cs* soubory.
> 
> 

## <a name="onstart-method"></a>OnStart – metoda
Hello **OnStart** metoda je volána, když vaše instance role do online režimu Azure. Při spouštění hello OnStart kód hello role instance je označena jako **zaneprázdněn** a žádné externí přenosy budou směrovanou tooit nástroj pro vyrovnávání zatížení hello. Tato metoda tooperform inicializace práce, například implementace obslužné rutiny událostí a spuštění můžete přepsat [Azure Diagnostics](cloud-services-how-to-monitor.md).

Pokud **OnStart** vrátí **true**, hello instance je úspěšně inicializován a Azure volá hello **RoleEntryPoint.Run** metoda. Pokud **OnStart** vrátí **false**, hello role ukončí okamžitě, bez spuštění jakýchkoli pořadí plánované vypnutí.

Následující příklad ukazuje kód jak Hello toooverride hello **OnStart** metoda. Tato metoda nakonfiguruje a spustí monitorování diagnostiky, když hello role instance spustí a nastaví přenos protokolování účet úložiště tooa dat:

```csharp
public override bool OnStart()
{
    var config = DiagnosticMonitor.GetDefaultInitialConfiguration();

    config.DiagnosticInfrastructureLogs.ScheduledTransferLogLevelFilter = LogLevel.Error;
    config.DiagnosticInfrastructureLogs.ScheduledTransferPeriod = TimeSpan.FromMinutes(5);

    DiagnosticMonitor.Start("DiagnosticsConnectionString", config);

    return true;
}
```

## <a name="onstop-method"></a>Onstop – metoda
Hello **OnStop** metoda je volána po instanci role je v režimu offline v Azure a před k ukončení procesu hello. Tato metoda toocall kódu vyžadovaného pro vaše toocleanly instance role vypnout můžete přepsat.

> [!IMPORTANT]
> Kód spuštěný v hello **OnStop** metoda má toofinish omezenou dobu, když je volána z jiných důvodů než vypnutí spuštěné uživatelem. Po uplynutí této doby, hello ukončení procesu, takže je nutné nejprve tento kód v hello **OnStop** metoda lze rychle spustit nebo toleruje toocompletion není spuštěna. Hello **OnStop** metoda je volána po hello **zastavení** událost se vyvolá.
> 
> 

## <a name="run-method"></a>Run – metoda
Můžete přepsat hello **spustit** metoda tooimplement dlouho spuštěných vláken pro instanci role.

Přepsání hello **spustit** metoda není vyžadováno; hello výchozí implementace spustí vlákno, které uspí navždy. Pokud přepíšete hello **spustit** metoda, váš kód by měl blokovat po neomezenou dobu. Pokud hello **spustit** metoda vrací výsledek, hello role je automaticky řádně recykluje; jinými slovy, vyvolá Azure hello **zastavení** událostí a volání hello **OnStop** metoda tak aby vaše vypnutí pořadí může být spuštěna před hello role do režimu offline.

### <a name="implementing-hello-aspnet-lifecycle-methods-for-a-web-role"></a>Implementace metody životního cyklu hello ASP.NET pro webové role
Můžete použít metody životního cyklu ASP.NET hello, kromě toothose poskytované hello **RoleEntryPoint** třídy toomanage inicializace a vypnutí pořadí pro webové role. To může být užitečná pro účely kompatibility, pokud jsou portování existující aplikaci tooAzure ASP.NET. metody životního cyklu ASP.NET Hello se volat v rámci hello **RoleEntryPoint** metody. Hello **aplikace\_spustit** metoda je volána po hello **RoleEntryPoint.OnStart** Metoda dokončení. Hello **aplikace\_End** metoda je volána před provedením hello **RoleEntryPoint.OnStop** metoda je volána.

## <a name="next-steps"></a>Další kroky
Zjistěte, jak příliš[vytvořit balíček služby cloud](cloud-services-model-and-package.md).

