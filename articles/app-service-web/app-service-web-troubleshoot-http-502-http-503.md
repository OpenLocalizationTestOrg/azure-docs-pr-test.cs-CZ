---
title: "aaaFix 502 Chybná brána, 503 Služba nedostupná chyby | Microsoft Docs"
description: "Vyřešte potíže Chybná brána 502 a 503 Služba není k dispozici chyby ve vaší webové aplikace hostované v Azure App Service."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: top-support-issue
keywords: "502 Chybná brána 503 Služba nedostupná, chyby 503, chyby 502"
ms.assetid: 51cd331a-a3fa-438f-90ef-385e755e50d5
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2016
ms.author: cephalin
ms.openlocfilehash: d9d8dcddaac930967a2e8d2bfd8cad09e6824c17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-http-errors-of-502-bad-gateway-and-503-service-unavailable-in-your-azure-web-apps"></a>Řešení chyb HTTP "502 Chybná brána" a "503 Služba není k dispozici" ve službě Azure web apps
"502 Chybná brána" a "503 Služba nedostupná" jsou běžné chyby ve vaší webové aplikace hostované v [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714). Tento článek vám pomůže vyřešit tyto chyby.

Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na hello Azure odborníky na [hello MSDN Azure a hello Stack Overflow fóra](https://azure.microsoft.com/support/forums/). Alternativně můžete také soubor incidentu podpory Azure. Přejděte toohello [podporu Azure lokality](https://azure.microsoft.com/support/options/) a klikněte na **získat podporu**.

## <a name="symptom"></a>Příznaky
Při procházení toohello webové aplikace, vrátí HTTP "502 Chybná brána" Chyba nebo HTTP Chyba "503 Služba není k dispozici".

## <a name="cause"></a>Příčina
Tento problém je často způsoben problémy na úrovni aplikací, například:

* požadavky trvá příliš dlouho
* aplikace pomocí vysoké paměti nebo procesoru
* selhání kvůli výjimce tooan aplikace.

## <a name="troubleshooting-steps-toosolve-502-bad-gateway-and-503-service-unavailable-errors"></a>Řešení potíží s kroky toosolve "502 Chybná brána" a "503 Služba nedostupná" chyb
Řešení potíží s jde rozdělit na tři samostatné úkoly v sekvenčním pořadí:

1. [Sledovat a monitorovat chování aplikace](#observe)
2. [Shromažďování dat](#collect)
3. [Zmírnění hello problém](#mitigate)

[App Service Web Apps](/services/app-service/web/) nabízí různé možnosti v každém kroku.

<a name="observe" />

### <a name="1-observe-and-monitor-application-behavior"></a>1. Sledovat a monitorovat chování aplikace
#### <a name="track-service-health"></a>Sledování stavu služby
Microsoft Azure publicizes pokaždé, když je služba došlo k přerušení nebo výkonu snížení. Stav hello hello služby můžete sledovat na hello [portálu Azure](https://portal.azure.com/). Další informace najdete v tématu [sledovat stav služeb](../monitoring-and-diagnostics/insights-service-health.md).

#### <a name="monitor-your-web-app"></a>Monitorování webové aplikace
Tato možnost umožňuje toofind out, pokud vaše aplikace je žádné problémy s. V okně vaší webové aplikace, klikněte na tlačítko hello **požadavky a chyby** dlaždici. Hello **metrika** okno se zobrazí všechny metriky hello můžete přidat.

Některé hello metriky, můžete toomonitor pro vaši webovou aplikaci

* Průměrná paměti pracovní sady
* Průměrná doba odezvy
* Čas procesoru
* Paměť pracovní sady
* Požadavky

![monitorování webové aplikace k řešení chyb HTTP 502 Chybná brána a 503 Služba nedostupná](./media/app-service-web-troubleshoot-HTTP-502-503/1-monitor-metrics.png)

Další informace naleznete v tématu:

* [Monitorování webové aplikace v Azure App Service](web-sites-monitor.md)
* [Zobrazování oznámení o výstrahách](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

<a name="collect" />

### <a name="2-collect-data"></a>2. Shromažďování dat
#### <a name="use-hello-azure-app-service-support-portal"></a>Hello použití portálu Azure App Service podpory
Web Apps poskytuje hello možnost tootroubleshoot problémy související tooyour webové aplikace prohlížením HTTP protokoly, protokoly událostí, výpisy procesů a další. Dostanete tyto informace pomocí náš portál podpory v **http://&lt;název aplikace >.scm.azurewebsites.net/Support**

portál Hello podpora služby Azure App Service nabízí tři samostatné karty toosupport hello tři kroky běžné scénáře řešení potíží:

1. Sledovat aktuální chování
2. Analýza shromažďování diagnostické informace a spuštěním hello předdefinované analyzátory
3. Zmírnění

Pokud problém hello se děje nyní, klikněte na **analyzovat** > **diagnostiky** > **diagnostikovat teď** toocreate diagnostické relaci, která bude shromažďovat HTTP zaznamená, protokoly Prohlížeče událostí, paměti, že výpisy, protokoly chyb PHP a PHP, zpracování sestavy.

Jakmile hello data jsou shromažďována, bude také spustit analýzu na hello dat a poskytnout sestavu ve formátu HTML.

V případě, že chcete toodownload hello data, ve výchozím nastavení, by je uložená ve složce D:\home\data\DaaS hello.

Další informace o portálu hello podpora služby Azure App Service naleznete v části [tooSupport nové aktualizace rozšíření lokality pro weby sady Azure](https://azure.microsoft.com/blog/new-updates-to-support-site-extension-for-azure-websites).

#### <a name="use-hello-kudu-debug-console"></a>Použití hello konzoly ladění modulu Kudu
Webové aplikace se dodává s konzolou pro ladění, který můžete použít pro ladění, prohlížení, nahrávání souborů a také koncové body JSON pro získání informací o vašem prostředí. Tento postup se nazývá hello *Kudu konzoly* nebo hello *řídicí panel SCM* pro vaši webovou aplikaci.

Tento řídicí panel můžete přejít pomocí odkazu přejdete toohello **https://&lt;název aplikace >.scm.azurewebsites.net/**.

Jsou některé hello věcí, které Kudu poskytuje:

* nastavení prostředí pro vaši aplikaci
* datový proud protokolu
* diagnostické výpis
* ladění konzoly, ve kterém můžete spouštět rutiny prostředí Powershell a základních příkazů DOS.

Další užitečné funkce Kudu je, že v případě, že aplikace je vyvolání first chance výjimek, můžete použít Kudu a výpisů hello SysInternals nástroj Procdump toocreate paměti. Tyto výpisy paměti jsou snímky hello procesu a často může pomoci při odstraňování složitějších problémů s vaší webové aplikace.

Další informace o funkcích, které jsou k dispozici v Kudu, najdete v části [online nástroje weby Azure, měli byste vědět o](https://azure.microsoft.com/blog/windows-azure-websites-online-tools-you-should-know-about/).

<a name="mitigate" />

### <a name="3-mitigate-hello-issue"></a>3. Zmírnění hello problém
#### <a name="scale-hello-web-app"></a>Škálování hello webové aplikace
Ve službě Azure App Service pro vyšší výkon a propustnost, můžete upravit hello škálování, ve kterém je spuštěná vaše aplikace. Škálování webovou aplikaci zahrnuje dvě souvisejících akcích: Změna vašeho plánu služby App Service tooa vyšší cenové úrovně a konfigurace určitá nastavení po Přepnuli jste toohello vyšší cenová úroveň.

Další informace o škálování najdete v tématu [škálování webové aplikace v Azure App Service](web-sites-scale.md).

Kromě toho můžete zvolit toorun aplikaci na více než jednu instanci. Pouze to poskytuje další možnost zpracování, ale také vám dává některé množství odolnost proti chybám. Pokud hello proces přestane fungovat na jednu instanci, hello ostatní instance stále bude obsluhovat požadavky.

Můžete nastavit hello toobe ruční nebo automatické škálování.

#### <a name="use-autoheal"></a>Pomocí funkce AutoHeal
Funkce AutoHeal recykluje pracovní proces hello pro vaši aplikaci na základě nastavení, které zvolíte (jako jsou změny konfigurace, požadavky, omezení na základě paměti nebo hello čas potřeby tooexecute požadavek). Většinu času hello recyklaci hello proces je hello nejrychlejší způsob, jak toorecover po chybě. I když můžete vždy restartovat hello webové aplikace z přímo v rámci hello portálu Azure, funkce AutoHeal bude provádět automaticky za vás. Toodo stačí je přidat některé aktivační události v hello kořenovém souboru web.config pro vaši webovou aplikaci. Všimněte si, že toto nastavení by fungovat v hello stejný způsobem, i když se vaše aplikace není jeden .net.

Další informace najdete v tématu [automatické opravy weby Azure](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites/).

#### <a name="restart-hello-web-app"></a>Restartujte hello webové aplikace
Tento problém je často hello nejjednodušší způsob, jak toorecover z jednorázových problémů. Na hello [portálu Azure](https://portal.azure.com/), v okně vaší webové aplikace, máte hello možnosti toostop nebo restartujte aplikaci.

 ![Restartujte aplikace toosolve chyby protokolu HTTP 502 Chybná brána a 503 Služba nedostupná](./media/app-service-web-troubleshoot-HTTP-502-503/2-restart.png)

Můžete také spravovat webové aplikace pomocí Azure Powershell. Další informace najdete v tématu [Použití Azure PowerShellu s Azure Resource Managerem](../powershell-azure-resource-manager.md).

