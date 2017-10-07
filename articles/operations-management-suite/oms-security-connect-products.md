---
title: "aaaConnecting toohello zabezpečení produkty Operations Management Suite (OMS) zabezpečení a Audit řešení | Microsoft Docs"
description: "Tento dokument pomůže vám tooconnect vaše tooOperations produkty zabezpečení Management Suite zabezpečení a Audit řešení pomocí běžný formát událostí."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 46eee484-e078-4bad-8c89-c88a3508f6aa
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2017
ms.author: yurid
ms.openlocfilehash: 0f4b372d0379987c4e249628a3c8d52733be65c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connecting-your-security-products-toohello-operations-management-suite-oms-security-and-audit-solution"></a>Připojení zabezpečení produkty toohello Operations Management Suite (OMS) zabezpečení a Audit řešení 
Tento dokument vám připojit zabezpečovací produkty do hello OMS zabezpečení a Audit řešení pomůže. jsou podporovány Hello následující zdroje:

- Události CEF (Common Event Format)
- Události Cisco ASA


## <a name="what-is-cef"></a>Co je CEF?
Běžný formát událostí (CEF) je standardní formát nad zprávy Syslog, používá mnoho zabezpečení dodavatelé tooallow událostí vzájemná funkční spolupráce mezi různými platformami. OMS zabezpečení a Audit řešení podporovat přijímání dat pomocí CEF, což vám umožní tooconnect zabezpečovací produkty OMS zabezpečení. 

Připojením vašeho tooOMS zdroje dat, jsou možné tootake výhod hello následující možnosti, které jsou součástí této platformě:

- Hledání a korelace
- Auditování
- Výstrahy
- Analýza hrozeb
- Významné problémy

## <a name="collection-of-security-solution-logs"></a>Shromažďování protokolů řešení zabezpečení

Zabezpečení OMS podporuje shromažďování protokolů pomocí formátu CEF přes Syslogy a protokoly [Cisco ASA](https://blogs.technet.microsoft.com/msoms/2016/08/25/add-your-cisco-asa-logs-to-oms-security/). V tomto příkladu je počítač se systémem Linux spuštěn démon procesu syslog ng hello zdroj (počítač, který generuje protokoly hello) a cíl hello je OMS zabezpečení. počítač se systémem Linux tooprepare hello budete potřebovat tooperform hello následující úkoly:

- Stáhněte si hello OMS agenta pro Linux, verze 1.2.0-25 nebo vyšší.
- Postupujte podle tématu hello **Stručná příručka nainstalovat** z [v tomto článku](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#steps-to-install-the-oms-agent-for-linux) tooinstall a zařadit hello agenta tooyour prostoru.

Hello agent je obvykle nainstalován na jiný počítač než hello jsou generovány, na které hello protokoly jeden. Počítač agenta předávání hello protokoly toohello bude obvykle vyžadují hello následující kroky:

- Nakonfigurujte hello protokolování produktu nebo počítač tooforward hello požadované události toohello démon procesu syslog (rsyslog nebo syslog ng) v počítači agenta hello.
- Povolte hello démon procesu syslog na hello agenta počítač tooreceive zpráv ze vzdáleného systému.

V počítači agenta hello třeba hello události toobe odeslaný port UDP toolocal démon procesu syslog hello 25226. Příchozí události na tomto portu naslouchá Hello agenta. Hello tady je příklad konfigurace pro odesílání všechny události z hello místní systém toohello agenta (nastavení můžete upravit toofit hello konfiguraci vaší místní):

1. Hello otevřete okno terminálu a přejděte toohello directory */etc/syslog-ng /* 
2. Vytvořte nový soubor *zabezpečení config-omsagent.conf* a přidejte hello následující obsah: OMS_facility = local4
    
    filter f_local4_oms { facility(local4); };

    destination security_oms { tcp("127.0.0.1" port(25226)); };

    log { source(src); filter(f_local4_oms); destination(security_oms); };
    
3. Stáhněte si soubor hello *security_events.conf* a umístěte */etc/opt/microsoft/omsagent/conf/omsagent.d/* v počítači agenta OMS hello.
4. Zadejte příkaz hello níže démon procesu syslog hello toorestart: *pro syslog-ng spustit:*
    
    ```
    sudo service rsyslog restart
    ```

    *Pro rsyslog spusťte:*
    
    ```
    /etc/init.d/syslog-ng restart
    ```
5. Zadejte příkaz hello níže toorestart hello agenta OMS:

    *Pro syslog-ng spusťte:*
    
    ```
    sudo service omsagent restart
    ```

    *Pro rsyslog spusťte:*
    
    ```
    systemctl restart omsagent
    ```
6. Zadejte následující příkaz hello a zkontrolujte tooconfirm hello výsledek, že nejsou žádné chyby v protokolu agenta OMS hello:

    ``` 
    tail /var/opt/microsoft/omsagent/log/omsagent.log
    ```

## <a name="reviewing-collected-security-events"></a>Kontrola shromážděných událostí zabezpečení

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

Po hello konfigurace, začne událostí zabezpečení hello toobe požita OMS zabezpečení. toovisualize těchto události, otevřete hello protokolu vyhledávání, zadejte příkaz hello *typ = CommonSecurityLog* v hello pole hledání a stiskněte klávesu ENTER. Hello následující příklad ukazuje hello výsledek tohoto příkazu, Všimněte si, že v tomto případě OMS zabezpečení již požity protokolů zabezpečení od více dodavatelů:
   
![Vyhodnocování standardních hodnot v řešení Zabezpečení a audit v OMS](./media/oms-security-connect-products/oms-security-connect-products-fig1.png)

Můžete upřesnit tohoto hledání pro jeden jednoho dodavatele, například toovisualize online Cisco protokoly, typ: *typ = CommonSecurityLog DeviceVendor = Cisco*. Hello "CommonSecurityLog" má předdefinovaná pole pro všechny hlavičky CEF včetně základní extensios hello při jakékoli jiné rozšíření, zda je "Rozšíření vlastních", nebo Ne, budou vložena do pole "Přepište". Můžete použít hello vlastní pole funkce tooget vyhrazené pole z něj. 

### <a name="accessing-computers-missing-baseline-assessment"></a>Přístup k počítačům, u nichž nedošlo k vyhodnocení standardních hodnot
OMS podporuje profil základní člen hello domény na Windows Server 2008 R2 až tooWindows Server 2012 R2. Standardní hodnoty systému Windows Server 2016 ještě nejsou dokončené, ale budou přidány okamžitě po publikování. Všechny ostatní operační systémy, zkontrolovat pomocí OMS zabezpečení a Audit hodnocení směrného plánu se zobrazí pod hello **počítače s chybějícími směrného plánu vyhodnocení** části.

## <a name="see-also"></a>Viz také
V tomto dokumentu jste se naučili jak tooconnect vaše řešení tooOMS CEF. Další informace o zabezpečení OMS toolearn najdete hello následující články:

* [Přehled Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Monitorování a výstrahy tooSecurity odpovídá v Operations Management Suite zabezpečení a Audit řešení](oms-security-responding-alerts.md)
* [Monitorování prostředků v řešení Zabezpečení a audit v Operations Management Suite](oms-security-monitoring-resources.md)

