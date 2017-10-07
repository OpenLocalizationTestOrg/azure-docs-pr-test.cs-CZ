---
title: "aaaCollect a analyzovat zprávy Syslog v OMS Log Analytics | Microsoft Docs"
description: "Syslog je protokol protokolování událostí, který je běžné tooLinux. Tento článek popisuje, jak tooconfigure kolekce zprávy Syslog v analýzy protokolů a podrobnosti záznamů hello vytvoří v úložišti OMS hello."
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: f1d5bde4-6b86-4b8e-b5c1-3ecbaba76198
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/12/2017
ms.author: magoedte;bwren
ms.openlocfilehash: 8bfa0bca3f2f18287d1352c98bbaa2a70e41e276
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="syslog-data-sources-in-log-analytics"></a>Syslog zdroje dat v analýzy protokolů
Syslog je protokol protokolování událostí, který je běžné tooLinux.  Aplikace bude odesílat zprávy, které může být uložené na místním počítači hello nebo doručit tooa Syslog kolekce.  Pokud je nainstalovaná hello OMS agenta pro Linux, nakonfiguruje hello místní Syslog démon tooforward zprávy toohello agenta.  Hello agent pak odešle zpráva tooLog hello Analytics kde se vytvoří odpovídající záznam v úložišti OMS hello.  

> [!NOTE]
> Analýzy protokolů podporuje kolekce zprávy odeslané rsyslog nebo syslog ng, kde je rsyslog démon výchozí hello. Démon procesu syslog výchozí Hello na verze 5 Red Hat Enterprise Linux a CentOS, Oracle Linux verze (sysklog) není podporována pro shromažďování událostí syslog. hello toocollect syslog data z této verze těchto distribuce [rsyslog démon](http://rsyslog.com) by měl být nainstalován a nakonfigurován tooreplace sysklog.
>
>

![Kolekce Syslog](media/log-analytics-data-sources-syslog/overview.png)

## <a name="configuring-syslog"></a>Konfigurace procesu Syslog
Hello OMS agenta pro Linux bude shromažďovat jenom události s hello zařízení a závažnosti, které jsou určené v konfiguraci.  Syslog můžete nakonfigurovat přes portál OMS hello nebo Správa konfiguračních souborů na agenty systému Linux.

### <a name="configure-syslog-in-hello-oms-portal"></a>Konfigurace Syslog na portálu OMS hello
Nakonfigurovat Syslog z hello [nabídce Data v nastavení analýzy protokolů](log-analytics-data-sources.md#configuring-data-sources).  Tato konfigurace se doručí toohello konfigurační soubor na každého agenta systému Linux.

Můžete přidat nové zařízení zadáním v jeho název a kliknutím na tlačítko  **+** .  Pro každé zařízení budou shromažďovány pouze zprávy s hello vybrané závažnosti.  Zkontrolujte hello závažnosti pro hello konkrétní zařízení, které chcete toocollect.  Žádné další kritéria nemůžete zadat toofilter zprávy.

![Konfigurace procesu Syslog](media/log-analytics-data-sources-syslog/configure.png)

Standardně jsou všechny změny konfigurace automaticky instaluje tooall agenty.  Pokud chcete tooconfigure Syslog ručně na každý agenta systému Linux, poté zrušte zaškrtnutí políčka hello *použít následující konfigurace počítačů se systémem Linux toomy*.

### <a name="configure-syslog-on-linux-agent"></a>Konfigurace Syslog na agenta systému Linux
Když hello [Linux klienta je nainstalován OMS agent](log-analytics-linux-agents.md), nainstaluje výchozí syslog konfiguračního souboru, který definuje hello zařízení a závažnost hello zprávy, které se shromažďují.  Tato konfigurace hello toochange soubor můžete upravit.  Hello konfigurační soubor se liší v závislosti na hello Syslog nainstaloval démon procesu, který hello klienta.

> [!NOTE]
> Pokud chcete upravit konfiguraci hello syslog, je nutné restartovat hello démon procesu syslog pro hello změny tootake vliv.
>
>

#### <a name="rsyslog"></a>rsyslog
Hello konfiguračního souboru pro rsyslog se nachází v **/etc/rsyslog.d/95-omsagent.conf**.  Níže jsou uvedeny výchozí obsah.  To shromažďuje syslog zpráv odeslaných z místního agenta hello pro všechna zařízení s úrovní varování nebo vyšší.

    kern.warning       @127.0.0.1:25224
    user.warning       @127.0.0.1:25224
    daemon.warning     @127.0.0.1:25224
    auth.warning       @127.0.0.1:25224
    syslog.warning     @127.0.0.1:25224
    uucp.warning       @127.0.0.1:25224
    authpriv.warning   @127.0.0.1:25224
    ftp.warning        @127.0.0.1:25224
    cron.warning       @127.0.0.1:25224
    local0.warning     @127.0.0.1:25224
    local1.warning     @127.0.0.1:25224
    local2.warning     @127.0.0.1:25224
    local3.warning     @127.0.0.1:25224
    local4.warning     @127.0.0.1:25224
    local5.warning     @127.0.0.1:25224
    local6.warning     @127.0.0.1:25224
    local7.warning     @127.0.0.1:25224

Budovy můžete odebrat odstraněním jeho části hello konfigurační soubor.  Můžete omezit hello závažnosti, které se shromažďují pro konkrétní zařízení úpravou položky tohoto zařízení.  Například toolimit hello uživatele zařízení toomessages s závažnosti chyby nebo vyšší byste upravili daného řádku hello konfigurační soubor toohello následující:

    user.error    @127.0.0.1:25224


#### <a name="syslog-ng"></a>Syslog ng
Hello konfigurační soubor pro syslog ng je umístění v **/etc/syslog-ng/syslog-ng.conf**.  Níže jsou uvedeny výchozí obsah.  To shromažďuje zpráv odeslaných z místního agenta hello všech zařízení a všechny závažnosti.   

    #
    # Warnings (except iptables) in one file:
    #
    destination warn { file("/var/log/warn" fsync(yes)); };
    log { source(src); filter(f_warn); destination(warn); };

    #OMS_Destination
    destination d_oms { udp("127.0.0.1" port(25224)); };

    #OMS_facility = auth
    filter f_auth_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(auth); };
    log { source(src); filter(f_auth_oms); destination(d_oms); };

    #OMS_facility = authpriv
    filter f_authpriv_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(authpriv); };
    log { source(src); filter(f_authpriv_oms); destination(d_oms); };

    #OMS_facility = cron
    filter f_cron_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(cron); };
    log { source(src); filter(f_cron_oms); destination(d_oms); };

    #OMS_facility = daemon
    filter f_daemon_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(daemon); };
    log { source(src); filter(f_daemon_oms); destination(d_oms); };

    #OMS_facility = kern
    filter f_kern_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(kern); };
    log { source(src); filter(f_kern_oms); destination(d_oms); };

    #OMS_facility = local0
    filter f_local0_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local0); };
    log { source(src); filter(f_local0_oms); destination(d_oms); };

    #OMS_facility = local1
    filter f_local1_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local1); };
    log { source(src); filter(f_local1_oms); destination(d_oms); };

    #OMS_facility = mail
    filter f_mail_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(mail); };
    log { source(src); filter(f_mail_oms); destination(d_oms); };

    #OMS_facility = syslog
    filter f_syslog_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(syslog); };
    log { source(src); filter(f_syslog_oms); destination(d_oms); };

    #OMS_facility = user
    filter f_user_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };

Budovy můžete odebrat odstraněním jeho části hello konfigurační soubor.  Můžete omezit hello závažnosti, které se shromažďují pro konkrétní zařízení jejich vyjmutím ze seznamu.  Například toolimit hello uživatele zařízení toojust zprávy upozornění a kritickou byste upravili této části hello konfigurační soubor toohello následující:

    #OMS_facility = user
    filter f_user_oms { level(alert,crit) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };


### <a name="collecting-data-from-additional-syslog-ports"></a>Shromažďování dat z další porty Syslog
Hello OMS agent přijímají zprávy Syslog v místním klientovi hello na portu 25224.  Pokud je nainstalován hello agent, použít výchozí konfigurace syslog a najít v hello následující umístění:

* Rsyslog:`/etc/rsyslog.d/95-omsagent.conf`
* Syslog-ng:`/etc/syslog-ng/syslog-ng.conf`

Číslo portu hello můžete změnit tak, že vytvoříte dvě konfigurační soubory: soubor konfigurace FluentD a soubor rsyslog nebo syslog ng v závislosti na instalaci hello démon procesu Syslog.  

* Hello FluentD konfigurační soubor by měl být nový soubor umístěný ve: `/etc/opt/microsoft/omsagent/conf/omsagent.d` a nahraďte hodnotu hello v hello **port** položka se vaše vlastní číslo portu.

        <source>
          type syslog
          port %SYSLOG_PORT%
          bind 127.0.0.1
          protocol_type udp
          tag oms.syslog
        </source>
        <filter oms.syslog.**>
          type filter_syslog
        </filter>

* Pro rsyslog, měli vytvořit nový soubor konfigurace umístěný v: `/etc/rsyslog.d/` a hello hodnota % SYSLOG_PORT % nahraďte váš vlastní číslo portu.  

    > [!NOTE]
    > Pokud změníte tuto hodnotu v konfiguračním souboru hello `95-omsagent.conf`, kdy hello agent platí výchozí konfiguraci bude přepsán.
    >

        # OMS Syslog collection for workspace %WORKSPACE_ID%
        kern.warning              @127.0.0.1:%SYSLOG_PORT%
        user.warning              @127.0.0.1:%SYSLOG_PORT%
        daemon.warning            @127.0.0.1:%SYSLOG_PORT%
        auth.warning              @127.0.0.1:%SYSLOG_PORT%

* Hello syslog ng konfigurace by měl být upraven tak, že zkopírujete následující hello příklad konfigurace a přidání hello vlastní upravené nastavení toohello konec hello syslog ng.conf konfigurační soubor umístěný ve `/etc/syslog-ng/`.  Proveďte **není** použít výchozí štítek hello **% WORKSPACE_ID % _oms** nebo **% WORKSPACE_ID_OMS**, definovat vlastní popisek toohelp rozlišit změny.  

    > [!NOTE]
    > Pokud změníte výchozí hodnoty hello v konfiguračním souboru hello, budou při hello agent použije výchozí konfigurace přepsány.
    >

        filter f_custom_filter { level(warning) and facility(auth; };
        destination d_custom_dest { udp("127.0.0.1" port(%SYSLOG_PORT%)); };
        log { source(s_src); filter(f_custom_filter); destination(d_custom_dest); };

Po dokončení změny hello, hello Syslog a hello služba agent OMS musí toobe restartovat tooensure hello konfigurace změny vstoupí v platnost.   

## <a name="syslog-record-properties"></a>Vlastnosti záznamu Syslog
Zaznamenává Syslog mít typ **Syslog** a mít hello vlastnosti v hello následující tabulka.

| Vlastnost | Popis |
|:--- |:--- |
| Počítač |Počítač, který hello událost nebyla shromážděna z. |
| Zařízení |Definuje hello součást hello systému, který vygeneroval zprávu hello. |
| HostIP |IP adresa systému hello odesílání zprávy hello. |
| Název hostitele |Název systému hello odesílání zprávy hello. |
| Úroveň závažnosti |Úroveň závažnosti události hello. |
| SyslogMessage |Text zprávy hello. |
| ID procesu |ID procesu hello generovaný uvítací zprávu. |
| eventTime |Datum a čas, který hello událostí byl vygenerován. |

## <a name="log-queries-with-syslog-records"></a>Dotazy protokolu se záznamy Syslog
Hello následující tabulka obsahuje různé příklady dotazů protokolu, která načítají záznamy Syslog.

| Dotaz | Popis |
|:--- |:--- |
| Typ = Syslog |Všechny systémové protokoly. |
| Typ = Syslog úroveň závažnosti chyby = |Všechny záznamy Syslog s závažnosti chyby. |
| Typ = Syslog &#124; míra count() počítačem. |Počet Syslog záznamy počítačem. |
| Typ = Syslog &#124; míra count() podle zařízení |Počet Syslog záznamy podle budovy. |

>[!NOTE]
> Pokud pracovní prostor byl upgradovaný toohello [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak změní následující toohello hello výše dotazy.

> | Dotaz | Popis |
|:--- |:--- |
| Syslog |Všechny systémové protokoly. |
| Syslog &#124; kde úroveň závažnosti == "error. |Všechny záznamy Syslog s závažnosti chyby. |
| Syslog &#124; shrnout AggregatedValue = count() počítačem. |Počet Syslog záznamy počítačem. |
| Syslog &#124; shrnout AggregatedValue = count() podle zařízení |Počet Syslog záznamy podle budovy. |

## <a name="next-steps"></a>Další kroky
* Další informace o [protokolu hledání](log-analytics-log-searches.md) tooanalyze hello data budou shromažďovat ze zdroje dat a řešení.
* Použití [vlastní pole](log-analytics-custom-fields.md) tooparse data z syslog záznamů do jednotlivých polí.
* [Konfigurace agentů Linux](log-analytics-linux-agents.md) toocollect jiné datové typy.
