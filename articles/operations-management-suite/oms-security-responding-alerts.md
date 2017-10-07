---
title: "aaaMonitoring a odpovídá tooSecurity výstrahy v Operations Management Suite zabezpečení a Audit řešení | Microsoft Docs"
description: "Tento dokument pomáhá vám toouse hello threat intelligence možnost k dispozici v OMS zabezpečení a Audit toomonitor a reagovat toosecurity výstrahy."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 7d45a32b-1341-4bb5-a436-1f42a8a2590a
ms.service: operations-management-suite
ms.custom: oms-security
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/13/2017
ms.author: yurid
ms.openlocfilehash: 3d92b6809b7bd934c889afc119e5e34ff2b85f1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-and-responding-toosecurity-alerts-in-operations-management-suite-security-and-audit-solution"></a>Monitorování a zpracování výstrah toosecurity v Operations Management Suite zabezpečení a Audit řešení
Tento dokument vám pomůže používat hello threat intelligence možnost k dispozici v OMS zabezpečení a Audit toomonitor a reagovat toosecurity výstrahy.

## <a name="what-is-oms"></a>Co je OMS?
Microsoft Operations Management Suite (OMS) je řešení pro správu IT, která pomáhá spravovat a chránit místní a cloudové infrastruktury založené na službě cloud společnosti Microsoft. Další informace o OMS, přečtěte si článek hello [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="threat-intelligence"></a>Analýza hrozeb
V prostředí podniku, kde uživatelé mají sítě toohello rozsáhlý přístup a použití různých zařízení tooconnect toocorporate dat je nutné, můžete aktivně sledovat vaše prostředky a rychle reagovat toosecurity incidenty. To je zvláště důležité z perspektivy životního cyklu hello zabezpečení, protože některé počítačové bezpečnosti, které hrozeb nemusí vyvolat výstrahy nebo podezřelé aktivity, které lze identifikovat podle tradiční technické ovládací prvky zabezpečení. 

Pomocí hello **analýzou hrozeb** možnost k dispozici v OMS zabezpečení a Audit, správci IT může identifikovat bezpečnostní hrozby proti hello prostředí, například, zjistit, zda je součástí určitého počítače [ botnet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection). Uzly v botnet počítačů se může stát, když útočníci nedovoleným způsobem nainstaluje malware, který tajně připojí tento příkaz toohello počítače a řízení. Můžete také poznat potenciální hrozby pocházejících z podzemní komunikačních kanálů, jako například [darknet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection_honeypots_darkents). 

V pořadí toobuild použít tento analýzou hrozeb, OMS zabezpečení a Audit dat pocházejících z více zdrojů v rámci Microsoftu. OMS zabezpečení a Audit využije tato data tooidentify potenciálních hrozeb pro vaše prostředí.

Hello analýzou hrozeb podokně tvoří podle tři hlavní možnosti:

* Servery s odchozím škodlivým provozem
* Typy zjištěnými hrozbami
* Mapa analýzy hrozeb

> [!NOTE]
> Přehled všechny tyto možnosti najdete v tématu [Začínáme s Operations Management Suite zabezpečení a Audit řešení](oms-security-getting-started.md).
> 
> 

### <a name="responding-toosecurity-alerts"></a>Odpovídá toosecurity výstrahy
Jeden z hello kroky [reakcí na incidenty zabezpečení](https://technet.microsoft.com/library/cc512623.aspx) proces je tooidentify hello závažnost ohrožení systémy hello. V této fázi proveďte hello následující úlohy:

* Určení hello povahy útoku hello
* Určení místo pro útok hello původu
* Určení hello záměr hello útoku. Byl hello útoků, konkrétně zaměřen na konkrétní informace tooacquire organizace, nebo byl náhodných?
* Určit hello systémy, které ohrožený
* Identifikovat hello soubory, které mají přístup k a určení hello citlivosti těchto souborů

Můžete využít **analýzou hrozeb** informace v OMS zabezpečení a Audit toohelp řešení těchto úloh. Postupujte podle kroků hello tooaccess, to **analýzou hrozeb** možnosti:

1. V hello **Microsoft Operations Management Suite** klikněte na hlavním řídicím **zabezpečení a Audit** dlaždici.
   
    ![Zabezpečení a Audit](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig1.png)
2. V hello **zabezpečení a Audit** řídicího panelu, zobrazí se hello **analýzou hrozeb** možnosti v pravé hello, jak je uvedeno níže:
   
    ![Intel hrozeb](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig2-ga.png)

Tyto tři dlaždice získáte přehled o aktuální ohrožení hello. V hello **serveru s odchozím škodlivým provozem** Pokud je jakýkoli počítač, který monitorujete (uvnitř nebo mimo síť) bude možné tooidentify tedy odesílání toohello škodlivý přenos Internetu. 

Hello **zjistila hrozba typy** dlaždice zobrazuje souhrnné údaje o hello hrozeb, které jsou aktuální "v divoký hello", pokud kliknete na tuto dlaždici se zobrazí další podrobnosti o těchto hrozbách jako zobrazit níže:

![Zjištěné typy hrozeb](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig3.png)

Další informace o jednotlivé hrozby můžete rozbalit kliknutím na. Následující příklad Hello ukazuje další podrobnosti o Botnet:

![Další informace o ohrožení](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig4.png)

Jak je popsáno v hello začátku této části, tato informace může být velmi užitečná při s případem reakcí na incidenty. Mohou také být důležité během forenzního vyšetřování, kde je nutné toofind hello zdroj hello útoku, který systém došlo k ohrožení a hello časové osy. V této sestavě mohli snadno identifikovat některé klíčové podrobnosti o hello útok, jako například: hello zdroj hello útoku, hello místní IP, došlo k ohrožení a hello aktuální relace stav připojení hello. 

Hello **threat intelligence mapy** vám pomůže tooidentify hello aktuální umístění kolem hello zeměkouli mající škodlivý přenos. Existují oranžová (příchozí) a červený (odchozí) šipky v této mapě identifikují hello směr provozu, pokud v jednom z těchto šipky kliknete, zobrazí hello typ hrozbě a hello směr provozu jak je uvedeno níže:

![mapa analýzy hrozeb](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig5.png)

> [!NOTE]
> Ukázka uvidíte o tom, jak toouse tato funkce během odpověď incidentu procesu Výukové video prezentace hello [zmírnit hrozby zabezpečení datového centra s asistencí šetření pomocí služby Operations Management Suite](https://myignite.microsoft.com/videos/5000) doručit na Microsoft Ignite.
> 

### <a name="responding-toodistinct-malicious-ip-accessed"></a>Odpovídá toodistinct škodlivé IP získat přístup
V některých scénářích si můžete všimnout potenciálně škodlivé IP adresy, ke které přistupoval jeden monitorovaný počítač:

![mapa analýzy hrozeb](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig6.png)

Tato výstraha a ostatní uživatele v hello stejnou kategorií, jsou generovány prostřednictvím zabezpečení OMS s využitím [analýzou hrozeb Microsoftu](https://youtu.be/O4WtxgUrDc8). Hello analýzou hrozeb dat je shromažďovaná společností Microsoft a také zakoupených od začátku poskytovatelů intelligence hrozeb. Tato data se často aktualizuje a přizpůsobit přesunutí toofast hrozeb. Z důvodu tooits povahy, měly být spojeny s další zdroje informací o zabezpečení při [příčin](https://blogs.technet.microsoft.com/msoms/2016/12/08/investigating-suspicious-activity-in-a-hybrid-cloud-with-oms-security/) výstrahy zabezpečení. 

## <a name="customize-alerts-received-via-e-mail"></a>Přizpůsobení obdržel prostřednictvím e-mailových upozornění

Můžete přizpůsobit, kteří uživatelé ve vaší organizaci bude upozorněn výstrahy zabezpečení jsou aktivovány OMS zabezpečení. Tato možnost je k dispozici v části Přehled / řídicí panel OMS hello nastavení na:

![E-mail](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig7.png)

## <a name="see-also"></a>Viz také
V tomto dokumentu jste se naučili jak toouse hello **analýzou hrozeb** možnost v OMS zabezpečení a Audit řešení toorespond toosecurity výstrahy. Další informace o zabezpečení OMS toolearn najdete hello následující články:

* [Přehled Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Začínáme se službou Operations Management Suite zabezpečení a Audit řešení](oms-security-getting-started.md)
* [Monitorování prostředků v řešení Zabezpečení a audit v Operations Management Suite](oms-security-monitoring-resources.md)

