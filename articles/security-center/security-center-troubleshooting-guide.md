---
title: "aaaAzure Průvodce odstraňováním potíží Security Center | Microsoft Docs"
description: "Tento dokument pomůže tootroubleshoot problémy v Azure Security Center."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 44462de6-2cc5-4672-b1d3-dbb4749a28cd
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/11/2017
ms.author: yurid
ms.openlocfilehash: 78b3c49eb66fe3a4f80efbba3a47a87b039c07ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-center-troubleshooting-guide"></a>Průvodce odstraňováním potíží pro službu Azure Security Center
Tato příručka je pro odborníky informačních technologií (IT) informace, analytikům zabezpečení informací a můžou správci cloudové jejichž organizace používá Azure Security Center a potřebovat problémy související s tootroubleshoot Security Center.

>[!NOTE] 
>Počínaje časná 2017 června, Security Center používá hello agenta Microsoft Monitoring Agent toocollect a uložit data. V tématu [Azure Security Center platformy migrace](security-center-platform-migration.md) toolearn Další. Hello informace v tomto článku představuje funkce Security Center po přechodu toohello agenta Microsoft Monitoring Agent.
>

## <a name="troubleshooting-guide"></a>Průvodce odstraňováním potíží
Tato příručka vysvětluje, jak tootroubleshoot Security Center související problémy. Většina hello provést v Centru zabezpečení řešení potíží s probíhá nejprve kontrolou hello [protokol auditování](https://azure.microsoft.com/updates/audit-logs-in-azure-preview-portal/) záznamy pro hello se nezdařilo součásti. Na základě protokolů auditu můžete zjistit:

* Které operace proběhly
* Kdo je inicioval hello operaci
* Pokud došlo k operaci hello
* Hello stav operace hello
* Hello hodnotách jiných vlastností, které vám můžou pomoct zkoumání operaci hello

protokol auditování Hello obsahuje všechny operace zápisu (PUT, POST, DELETE) provést na vašich prostředků, ale nezahrnuje operace čtení (GET).

## <a name="microsoft-monitoring-agent"></a>Microsoft Monitoring Agent
Security Center používá hello agenta Microsoft Monitoring Agent – to je hello používá stejné agenta hello Operations Management Suite a analýzy protokolů služba – toocollect zabezpečení dat z Azure virtuální počítače. Po povolení shromažďování dat a je správně nainstalován hello agent v hello cílový počítač, níže hello procesu musí být ve spuštění:

* HealthService.exe

Pokud otevřete konzolu pro správu služby hello (services.msc), zobrazí se také spuštěna služba Microsoft Monitoring Agent hello jak je uvedeno níže:

![Služby](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig5.png)

Otevřete toosee, kterou verzi hello agenta, budete mít, **Správce úloh**, v hello **procesy** karta najít hello **služba Microsoft Monitoring Agent**, klikněte pravým tlačítkem myši na něm a Klikněte na tlačítko **vlastnosti**. V hello **podrobnosti** kartě, podívejte hello verze souboru, jak je uvedeno níže:

![File](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig6.png)
   

## <a name="microsoft-monitoring-agent-installation-scenarios"></a>Scénáře instalace služby Microsoft Monitoring Agent
Existují dva scénáře instalace, které mohou mít různé výsledky při instalaci hello agenta Microsoft Monitoring Agent do vašeho počítače. Hello Podporované scénáře jsou:

* **Agent nainstalovaný automaticky pomocí služby Security Center**: v tomto scénáři bude možné tooview hello výstrahy v umístění, Security Center a hledání protokolů. Obdržíte e-mailových oznámení toohello e-mailovou adresu, který byl nakonfigurován v zásadách zabezpečení hello pro hello předplatné hello prostředek patří.
.
* **Agent ručně nainstalovat na virtuální počítač nachází v Azure**: v tomto scénáři, pokud používáte agenty stáhli a nainstalovali ručně předchozí tooFebruary 2017, nebudete moct tooview hello výstrahy Security Center portálu hello pouze v případě, že můžete filtrovat hello pracovní prostor hello předplatné patří. V případě filtru na hello předplatné hello prostředku patří pro vás nebudou moct toosee žádné výstrahy. Obdržíte e-mailových oznámení toohello e-mailovou adresu, která byla konfigurována pro hello předplatné hello prostoru patří do zásady zabezpečení hello.

>[!NOTE]
> chování hello tooavoid hello podrobně druhý, ujistěte se, že si stáhnout nejnovější verzi agenta hello hello.
> 

## <a name="troubleshooting-monitoring-agent-network-requirements"></a>Odstraňování potíží se síťovými požadavky na agenta monitorování
Pro agenty tooconnect tooand registr s Centrem zabezpečení musí mít přístup k prostředkům toonetwork, včetně hello čísla portů a adres URL domény.

- Pro proxy servery budete potřebovat tooensure, který hello odpovídající proxy serveru, které prostředky jsou nakonfigurované v nastavení agenta. Přečtěte si další informace v tomto článku [jak toochange hello nastavení proxy serveru](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-windows-agents#configure-proxy-settings).
- Pro brány firewall, které omezují přístup toohello Internet je nutné tooconfigure tooOMS přístup toopermit vaší brány firewall. V nastavení agenta nemusíte nic konfigurovat.

Hello následující tabulka ukazuje prostředky potřebné pro komunikaci.

| Prostředek agenta | Porty | Obejít kontrolu protokolu HTTPS |
|---|---|---|
| *.ods.opinsights.azure.com | 443 | Ano |
| *.oms.opinsights.azure.com | 443 | Ano |
| *.blob.core.windows.net | 443 | Ano |
| *.azure-automation.net | 443 | Ano |

Pokud narazíte na potíže registrace s agentem hello, ujistěte se, že článek hello tooread [jak problémy registrace služby Operations Management Suite tootroubleshoot](https://support.microsoft.com/en-us/help/3126513/how-to-troubleshoot-operations-management-suite-onboarding-issues).


## <a name="troubleshooting-endpoint-protection-not-working-properly"></a>Řešení potíží s tím, že ochrana koncových bodů nefunguje správně

agent hosta Hello je proces nadřazené hello všechno hello [Antimalware od Microsoftu](../security/azure-security-antimalware.md) nemá rozšíření. Při procesu agenta hosta hello selže, může selhat také hello spouštěný jako podřízený proces agenta hosta hello Antimalware od Microsoftu.  Ve scénářích, jako je doporučené tooverify hello následující možnosti:

- Pokud hello cílovém virtuálním počítači je vlastní image a hello Tvůrce hello virtuálních počítačů nikdy nainstalován agent hosta.
- Pokud cílový hello je virtuální počítač s Linuxem místo virtuální počítač s Windows následnou instalaci verze systému Windows hello hello antimalwarových rozšíření na virtuální počítač s Linuxem se nezdaří. agent hosta Linux Hello má specifické požadavky z hlediska verze operačního systému a požadované balíčky a pokud nejsou splněny tyto požadavky nebude hello agenta virtuálního počítače fungovat existuje buď. 
- Pokud hello virtuálního počítače byl vytvořen s stará verze agenta hosta. Pokud byl, byste měli vědět, že některé staré agenty nelze automatickou aktualizaci samotné toohello novější verze a to může vést toothis problém. Vždy používejte nejnovější verzi agenta hosta hello, pokud vytváření vlastních bitových kopií.
- Některý software třetích stran správy může zakázat agenta hosta hello nebo blokovat přístup k umístění souborů toocertain. Pokud máte nainstalovaný na vašem virtuálním počítači třetích stran, zajistěte, aby byl tento agent hello v seznamu vyloučení hello.
- Určitá nastavení brány firewall nebo skupina zabezpečení sítě (NSG) může blokovat tooand provoz sítě z agenta hosta.
- Některé seznamy řízení přístupu (ACL) mohou bránit v přístupu k disku.
- Nedostatek místa na disku můžete blokovat agenta hosta hello nebude fungovat správně. 

Ve výchozím nastavení hello Microsoft Antimalware uživatelské rozhraní je zakázána, číst [povolení Microsoft Antimalware uživatelské rozhraní na Azure Resource Manager virtuální počítače po nasazení](https://blogs.msdn.microsoft.com/azuresecurity/2016/03/09/enabling-microsoft-antimalware-user-interface-post-deployment/) Další informace o tom, tooenable ho potřebujete.

## <a name="troubleshooting-problems-loading-hello-dashboard"></a>Řešení potíží s problémy s načtením hello řídicí panel

Pokud máte problémy načítání řídicího panelu Security Center hello, ujistěte se, že hello uživatele, který registruje hello předplatné tooSecurity Center (tj. hello první uživatel jeden, který otevřel Security Center s předplatným hello) a hello uživatel, který chcete tooturn na shromažďování dat musí být *vlastníka* nebo *Přispěvatel* v předplatném hello. Od této chvíle také uživatelé s *čtečky* na hello předplatné můžete zobrazit hello řídicí panel nebo výstrahy nebo doporučení nebo zásad.

## <a name="contacting-microsoft-support"></a>Kontaktování oddělení podpory společnosti Microsoft
Některé problémy lze identifikovat pomocí hello pokynů v tomto článku, ostatní můžete také vyhledat popsané na stránce hello Security Center veřejné [fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureSecurityCenter). Pokud však potřebujete odstraňovat potíže mimo tento rámec, můžete vytvořit novou žádost o podporu prostřednictvím webu **Azure Portal**, jak je znázorněno níže: 

![Podpora společnosti Microsoft](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig2.png)


## <a name="see-also"></a>Viz také
V tomto dokumentu jste se naučili jak tooconfigure zásady zabezpečení v Azure Security Center. toolearn Další informace o službě Azure Security Center, najdete v části hello následující:

* [Průvodce Azure Security Center plánováním a provozem](security-center-planning-and-operations-guide.md) – Další informace jak tooplan a pochopit hello návrhu aspekty tooadopt Azure Security Center.
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – zjistěte, jak toomonitor hello stav svých prostředků Azure
* [Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – Další informace jak toomanage a reakce toosecurity výstrahy
* [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – zjistěte, jak toomonitor hello stav vašich partnerských řešení.
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello
* [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/) – Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů

