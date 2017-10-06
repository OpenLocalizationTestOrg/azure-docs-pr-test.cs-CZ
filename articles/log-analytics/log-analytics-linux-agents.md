---
redirect_url: /azure/log-analytics/log-analytics-agent-linux
redirect_document_id: True
ROBOTS: NOINDEX
ms.openlocfilehash: 8b526144cd565f6750368e12970f008e66cc2023
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-linux-computers-toolog-analytics"></a>Připojení počítače tooLog vaše Linux Analytics
Pomocí analýzy protokolů, můžete shromažďovat a fungovat na informace shromážděné z počítače se systémem Linux. Přidání data shromážděná z Linux tooOMS umožňuje toomanage systémy Linux a kontejner řešení jako Docker, bez ohledu na to, kde jsou počítače umístěny – prakticky odkudkoli. Zdroje dat může nacházet ve vašem datovém centru místní jako fyzické servery, virtuální počítače ve službě hostovaných v cloudu jako Amazon Web Services (AWS) nebo Microsoft Azure, nebo i hello přenosných počítačů na vaše oddělení podpory. Kromě toho OMS taky shromažďuje data z počítače se systémem Windows podobně, takže podporuje hybridním skutečně IT prostředí.

Můžete zobrazit a spravovat data ze všech těchto zdrojů s analýzy protokolů v OMS pomocí portálu správy. To snižuje nutnost hello toomonitor pomocí jinými systémy, umožňuje snadno tooconsume, a můžete exportovat žádná data, jako řešení toowhatever obchodní analýzy nebo systém, který už máte.

Tento článek je úvodní příručce, která vám pomůže shromažďovat a spravovat data pro počítače Linux pomocí hello OMS agenta pro Linux. Další technické podrobnosti jako konfiguraci proxy serveru, informace o CollectD metriky a vlastní zdroje dat JSON najdete tyto informace v [OMS agenta pro Linux přehled](https://github.com/Microsoft/OMS-Agent-for-Linux) a [OMS agenta pro Linux úplné dokumentace](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md) na Githubu.

V současné době můžete shromáždit hello následujících typů dat z počítače se systémem Linux:

* Metriky výkonu
* Události procesu Syslog
* Výstrahy od Nagios a Zabbix
* Metriky výkonu kontejner docker, inventář a protokoly

## <a name="supported-linux-versions"></a>Podporované verze systému Linux
Verze x86 a x64 jsou oficiálně podporované pro řadu distribucí systému Linux. Hello OMS agenta pro Linux mohou však spustit také na dalších distribuce, které nejsou uvedené.

* Linux Amazon 2012.09 prostřednictvím 2015.09
* CentOS Linux 5, 6 a 7
* Oracle Linux 5, 6 a 7
* Red Hat Enterprise Linux Server 5, 6 a 7
* Debian GNU/Linux 6, 7 a 8
* Ubuntu 12.04 LTS, 14.04 LTS, 15.04, 15.10
* SUSE Linux Enterprise Server 11 a 12

## <a name="oms-agent-for-linux"></a>OMS agenta pro Linux
Hello Operations Management Suite agenta pro Linux obsahuje více balíčků. Hello verze souboru obsahuje následující balíčky, které jsou k dispozici ve spuštěné sadě hello prostředí s hello `--extract`.

| **Balíček** | **Verze** | **Popis** |
| --- | --- | --- |
| omsagent |1.1.0 |Hello Operations Management Suite agenta pro Linux |
| omsconfig |1.1.1 |Konfigurace agenta pro hello agenta OMS |
| OMI |1.0.8.3 |Infrastruktury Open Management Infrastructure (OMI) – prosté Server CIM |
| scx. |1.6.2 |Zprostředkovatelé CIM OMI pro metriku výkonu operačního systému |
| Apache cimprov |1.0.0 |Zprostředkovatel pro OMI monitorování výkonu serveru Apache HTTP Server. Instalovat pouze v případě zjištění serveru Apache HTTP Server. |
| MySQL cimprov |1.0.0 |Výkon serveru MySQL monitorování zprostředkovatele pro OMI. Instalovat pouze v případě zjištění MySQL nebo MariaDB serveru. |
| docker cimprov |0.1.0 |Zprostředkovatel docker pro OMI. Instalovat pouze v případě zjištění Docker. |

### <a name="additional-installation-artifacts"></a>Artefakty další instalace
Po instalaci agenta OMS hello pro balíčky Linux, hello následující další systémové změny konfigurace se použijí. Tyto artefakty se odeberou, když hello omsagent balíček odinstalován.

* Bez oprávnění uživatele s názvem: `omsagent` je vytvořena. Toto je hello účet hello omsagent démon běží jako
* Vytvoření souboru sudoers "zahrnout" v /etc/sudoers.d/omsagent to autorizuje omsagent toorestart hello syslog a omsagent démoni. Pokud direktivy "zahrnout" sudo nejsou podporovány v hello nainstalované verzi sudo, tyto položky budou zapsány příliš/etc/sudoers.
* Konfigurace syslog Hello je upravený tooforward podmnožinu události toohello agenta. Další informace najdete v tématu hello **konfigurace shromažďování dat** části

### <a name="linux-data-collection-details"></a>Podrobnosti kolekce dat Linux
Hello následující tabulka uvádí metody shromažďování dat a další podrobnosti o tom, jak se data shromažďují.

| Zdroj | Přímé agenta | Agenta nástroje SCOM | Azure Storage | SCOM vyžaduje? | Data agenta SCOM odeslána prostřednictvím skupiny pro správu | Frekvence kolekce |
| --- | --- | --- | --- | --- | --- | --- |
| Zabbix |![Ano](./media/log-analytics-linux-agents/oms-bullet-green.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |1 minuta |
| Nagios |![Ano](./media/log-analytics-linux-agents/oms-bullet-green.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |v případě přijetí |
| syslog |![Ano](./media/log-analytics-linux-agents/oms-bullet-green.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |ze služby Azure storage: 10 minut; z agenta: na přijetí |
| Čítače výkonu systému Linux |![Ano](./media/log-analytics-linux-agents/oms-bullet-green.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |podle plánu, minimálně 10 sekund. |
| sledování změn |![Ano](./media/log-analytics-linux-agents/oms-bullet-green.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |![Ne](./media/log-analytics-linux-agents/oms-bullet-red.png) |každou hodinu |

### <a name="package-requirements"></a>Požadavky na balíček
| **Požadovaný balíček** | **Popis** | **Minimální verze** |
| --- | --- | --- |
| Glibc |Knihovna GNU C |2.5-12 |
| OpenSSL |Knihovny OpenSSL |0.9.8E nebo 1.0 |
| Curl |cURL webového klienta |7.15.5 |
| Python ctypes |Funkce knihovny |neuvedeno |
| PAM |PAM moduly |neuvedeno |

> [!NOTE]
> Rsyslog nebo syslog ng jsou požadované toocollect zprávy syslog. Démon procesu syslog výchozí Hello na verze 5 Red Hat Enterprise Linux a CentOS, Oracle Linux verze (sysklog) není podporována pro shromažďování událostí syslog. toocollect syslog data z této verze těchto distribuce hello rsyslog démon by měl být nainstalován a nakonfigurován tooreplace sysklog.
>
>

## <a name="quick-install"></a>Rychlé instalace
Spusťte následující příkazy toodownload hello omsagent hello, ověření kontrolního součtu hello, pak instalace a zařadit hello agenta. Příkazy jsou pro 64bitovou verzi. Hello ID pracovního prostoru a primární klíč se nacházejí na portálu OMS hello pod **nastavení** na hello **připojené zdroje** kartě.

![Podrobnosti o pracovním prostoru](./media/log-analytics-linux-agents/oms-direct-agent-primary-key.png)

```
wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR OMS WORKSPACE ID> -s <YOUR OMS WORKSPACE PRIMARY KEY>
```

Existují různé další metody tooinstall hello agenta a upgradujte ho. Další informace o nich v [kroky tooinstall hello OMS agenta pro Linux](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#steps-to-install-the-oms-agent-for-linux).

Můžete také zobrazit hello [Azure video s návodem](https://www.youtube.com/watch?v=mF1wtHPEzT0).

## <a name="choose-your-linux-data-collection-method"></a>Zvolte metodu kolekce dat Linux
Jak vybrat hello datové typy, které byste jako toocollect závisí na tom, jestli se má portál OMS hello toouse, nebo pokud chcete upravit různé konfigurační soubory přímo na klienty Linux. Pokud si zvolíte toouse hello portál, konfigurace hello je odeslána tooall vaši klienti Linux. Pokud potřebujete různé konfigurace pro jiné klienty Linux, budete potřebovat soubory klienta tooedit jednotlivě – nebo použijete alternativu jako PowerShell DSC, Chef nebo Puppet.

Můžete zadat hello syslog události a čítače výkonu, které chcete toocollect pomocí konfigurační soubory do počítače se systémem Linux hello. *Pokud jste zvolili tooconfigure shromažďování dat úpravou konfigurační soubory agenta, měli byste zakázat hello Centralizovaná konfigurace.*  Pokyny najdete níže shromažďování dat tooconfigure hello agenta konfigurační soubory a také toodisable centrální konfigurace pro všechny agenty OMS pro Linux nebo jednotlivé počítače.

### <a name="disable-oms-management-for-an-individual-linux-computer"></a>Zakázat správu OMS pro jednotlivý počítač Linux
Centralizované shromažďování dat pro konfigurační data je zakázána pro jednotlivý počítač Linux s hello OMS_MetaConfigHelper.py skriptu. To může být užitečné, pokud podmnožině počítačů by měl mít speciální konfigurace.

Centralizovaná konfigurace toodisable:

```
sudo /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py --disable
```

Povolit toore Centralizovaná konfigurace:

```
sudo /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py –enable
```

## <a name="linux-performance-counters"></a>Čítače výkonu systému Linux
Čítače výkonu systému Linux jsou podobné čítače výkonu tooWindows – jak fungují podobně. Můžete použít následující postupy tooadd hello a je nakonfigurovat. Po přidání tooOMS data jsou shromažďována pro ně každých 30 sekund.

### <a name="tooadd-a-linux-performance-counter-in-oms"></a>tooadd Linux čítače výkonu v OMS
1. tooconfigure OMS agentů pro Linux pomocí portálu OMS hello, můžete přidat čítače výkonu Linux na stránce nastavení hello, klikněte na tlačítko **Data**.  
2. Na hello **nastavení** v části **Data** , klikněte na tlačítko **čítače výkonu Linux** a pak vyberte nebo zadejte hello název čítače hello chcete tooadd.  
    ![data](./media/log-analytics-linux-agents/oms-settings-data01.png)
3. Pokud si nejste jisti hello úplný název čítače hello, pište částečný název a zobrazí se seznam dostupných čítačů. Když najít hello čítače můžete tooadd, klikněte na název hello hello seznamu a potom klikněte na hello plus ikonu tooadd hello čítače.
4. Po přidání hello čítač se zobrazí v seznamu hello čítačů zvýrazněná s barevnou panelu.
5. Ve výchozím nastavení, hello **použít níže uvedených počítačích toomy konfigurace** je vybraná možnost. Pokud chcete toodisable odesílání konfiguračních dat, zrušte výběr hello.
6. Po dokončení změny čítače výkonu, v dolní části hello hello stránky klikněte na tlačítko **Uložit** toofinalize změny. Hello změny konfigurace, které jste udělali se pak odešlou tooall hello OMS agentů pro Linux, které jsou registrovány OMS, obvykle během pěti minut.

### <a name="configure-linux-performance-counters-in-oms"></a>Konfigurovat čítačů výkonu systému Linux v OMS
Pro čítače výkonu systému Windows můžete konkrétní instance jednotlivých čítačů výkonu. Pro čítače výkonu systému Linux, platí jakoukoli instanci čítače, který zvolíte, čítače podřízené tooall hello nadřazené čítače. Hello následující tabulka zobrazuje hello běžné instance čítače výkonu k dispozici tooboth Linux a Windows.

| **Název instance** | **Význam** |
| --- | --- |
| \_Celkový počet |Celkový počet všech instancí hello |
| \* |Všechny instance |
| (/ &#124; / var) |Odpovídá instancí s názvem: / nebo /var |

Podobně hello ukázka interval, který vyberete pro nadřazené čítač platí tooall jeho podřízených čítače. Jinými slovy jsou všechny intervalů vzorkování čítač podřízené hello a instance svázané společně.

### <a name="add-and-configure-performance-metrics-with-linux"></a>Přidání a konfigurace metriky výkonu operačního systému Linux
Toocollect metriky výkonu jsou řízeny hello konfigurace v/etc/opt/microsoft/omsagent/&lt;id pracovního prostoru&gt;/conf/omsagent.conf. V tématu [metriky výkonu k dispozici](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#appendix-available-performance-metrics) dostupných tříd a metriky pro hello OMS agenta pro Linux.

Každý objekt nebo kategorii toocollect metriky výkonu by měla být definována v konfiguračním souboru hello jako jeden `<source>` elementu. Syntaxe Hello následující hello níže.

```
<source>
  type oms_omi  
  object_name "Processor"
  instance_regex ".*"
  counter_name_regex ".*"
  interval 30s
</source>

```

Konfigurovat parametry Hello tohoto elementu jsou:

* **Objekt\_název**: název objektu hello hello kolekce.
* **Instance\_regex**: *regulární výraz* definování které toocollect instance. Hello hodnota: `.*` Určuje všechny instance. toocollect procesoru metriky pro pouze hello \_celkový počet instancí, můžete zadat `_Total`. toocollect proces metriky pro pouze hello crond nebo sshd instancí, můžete zadat: `(crond|sshd)`.
* **Čítač\_název\_regex**: *regulární výraz* definování které toocollect čítače (pro objekt hello). Zadejte všechny čítače pro objekt hello toocollect: `.*`. toocollect pouze odkládacího prostoru čítače hello paměti objektu, můžete zadat:`.+Swap.+`
* **Interval:**: hello frekvence, na které hello se shromažďují objektu čítače.

Hello výchozí konfiguraci pro metriku výkonu je:

```
<source>
  type oms_omi
  object_name "Physical Disk"
  instance_regex ".*"
  counter_name_regex ".*"
  interval 5m
</source>

<source>
  type oms_omi
  object_name "Logical Disk"
  instance_regex ".*
  counter_name_regex ".*"
  interval 5m
</source>

<source>
  type oms_omi
  object_name "Processor"
  instance_regex ".*
  counter_name_regex ".*"
  interval 30s
</source>

<source>
  type oms_omi
  object_name "Memory"
  instance_regex ".*"
  counter_name_regex ".*"
  interval 30s
</source>

```

### <a name="enable-mysql-performance-counters-using-linux-commands"></a>Povolit MySQL čítače výkonu pomocí příkazů Linux
Pokud Server databáze MySQL nebo MariaDB Server v počítači se zjistí hello při instalaci sady omsagent hello, automaticky se nainstaluje zprostředkovatele pro Server databáze MySQL monitorování výkonu. Tento zprostředkovatel připojí toohello místní databáze MySQL nebo MariaDB tooexpose Statistika výkonu serveru. Budete potřebovat přihlašovací údaje uživatele tooconfigure MySQL, aby hello zprostředkovatele přístup hello MySQL serveru.

toodefine výchozího uživatele účtu pro hello MySQL server na localhost, hello použijte následující příkaz Ukázka.

> [!NOTE]
> soubor s přihlašovacími údaji Hello musí být přečíst hello omsagent účtu. Spuštění příkazu mycimprovauth hello jako omsgent se doporučuje.
>
>

```
sudo su omsagent -c '/opt/microsoft/mysql-cimprov/bin/mycimprovauth default 127.0.0.1 <username> <password>

sudo /opt/omi/bin/service_control restart
```


Alternativně můžete zadat hello vyžadují přihlašovací údaje databáze MySQL do souboru, vytváření souboru hello: /var/opt/microsoft/mysql-cimprov/auth/omsagent/mysql-auth. Další informace o správě MySQL přihlašovací údaje pro monitorování prostřednictvím hello mysql auth souboru, najdete v části [spravovat MySQL monitorování přihlašovací údaje v souboru authentication hello](#manage-mysql-monitoring-credentials-in-the-authentication-file).

V tématu [databáze oprávnění vyžadovaná pro čítače výkonu databáze MySQL](#database-permissions-required-for-mysql-performance-counters) podrobnosti o objektu oprávněních hello MySQL uživatele toocollect data o výkonu serveru MySQL.

### <a name="enable-apache-http-server-performance-counters-using-linux-commands"></a>Povolit čítače výkonu serveru Apache HTTP Server pomocí příkazů Linux
Pokud v počítači hello se zjistí serveru Apache HTTP Server, pokud je nainstalovaná sada omsagent hello, automaticky se nainstaluje zprostředkovatele pro serveru Apache HTTP Server monitorování výkonu. Tento zprostředkovatel spoléhá na platformě Apache "modul", který je nutné načíst do hello serveru Apache HTTP Server v datech o výkonu tooaccess pořadí.

Modul hello můžete načíst pomocí hello následující příkaz:

```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -c
```

toounload hello Apache modulu sledování, spusťte následující příkaz hello:

```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -u
```
### <a name="tooview-performance-data-with-log-analytics"></a>data výkonu tooview s analýzy protokolů
1. V hello portál Operations Management Suite klikněte na dlaždici hledání protokolů hello.
2. V panelu vyhledávání hello, zadejte `* (Type=Perf)` tooview všech čítačů výkonu.

Protože OMS taky shromažďuje data čítače výkonu systému Windows, jste měli oboru rozbalovací hello tooLinux specifických dat hledání. Ano hello následujícím příkladu by zobrazit výkon dat konkrétní tooan příklad Linux serveru s názvem Chorizo21.

```
Type=Perf Computer=chorizo*
```

![Příklad server zobrazen ve výsledcích vyhledávání](./media/log-analytics-linux-agents/oms-perfsearch01.png)

Ve výsledcích hello, můžete kliknout na **metriky** tooview hello čítače, které pro nebyla shromážděna data. Data v reálném čase je zobrazen jako grafy pro každý čítač.

![Průzkumníku metrik](./media/log-analytics-linux-agents/oms-perfmetrics01.png)

## <a name="syslog"></a>Syslog
Syslog je událost protokolování protokol podobné tooWindows protokoly událostí – obě fungují podobně jako při zobrazení v OMS.

### <a name="tooadd-a-new-linux-syslog-facility-in-oms"></a>tooadd nového zařízení syslog Linux v OMS
1. Na hello **nastavení** v části **Data** , klikněte na tlačítko **Syslog** a toohello vlevo hello a ikonu, zadejte název hello hello mechanismus syslog, které chcete tooadd.
    ![Linux syslog](./media/log-analytics-linux-agents/oms-linuxsyslog01.png)
2. Pokud si nejste jisti hello úplný název hello zařízení, pište částečný název a zobrazí se seznam dostupných syslog zařízení. Zjistíte, když chcete tooadd, klikněte na název hello v seznamu hello a pak klikněte na hello plus ikonu tooadd hello protokolovací mechanismus syslog hello protokolovací mechanismus syslog.
3. Po přidání hello zařízení, zobrazí se v seznamu hello zvýrazněná s barevnou panelu. V dalším kroku vyberte hello závažnosti (kategorie syslog zařízení informací), které chcete toocollect.
4. V dolní části hello hello stránky klikněte na tlačítko **Uložit** toofinalize změny. Hello změny konfigurace, které jste udělali se pak odešlou tooall hello OMS agentů pro Linux, které jsou registrovány OMS, obvykle během pěti minut.

### <a name="configure-linux-syslog-facilities-in-linux"></a>Nakonfigurujte zařízení syslog Linux v systému Linux
Události procesu Syslog jsou odesílány z hello démon procesu syslog, například rsyslog nebo syslog ng, místního portu tooa tohoto agenta hello naslouchá na. Ve výchozím nastavení je to port 25224. Pokud je nainstalován hello agent, je použita výchozí konfigurace syslog. To se nachází zde:

Rsyslog: /etc/rsyslog.d/rsyslog-oms.conf

Syslog ng: /etc/syslog-ng/syslog-ng.conf

Konfigurace syslog agenta OMS výchozí Hello odesílá události procesu syslog od všech zařízení, se závažností upozornění nebo vyšší.

> [!NOTE]
> Pokud chcete upravit konfiguraci hello syslog, je nutné restartovat hello démon procesu syslog pro hello změny tootake vliv.
>
>

Hello výchozí syslog pro hello OMS agenta pro Linux pro OMS je konfigurace:

#### <a name="rsyslog"></a>rsyslog
```
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
```

#### <a name="syslog-ng"></a>Syslog ng
```
#OMS_facility = all
filter f_warning_oms { level(warning); };
destination warning_oms { tcp("127.0.0.1" port(25224)); };
log { source(src); filter(f_warning_oms); destination(warning_oms); };
```

### <a name="tooview-all-syslog-events-with-log-analytics"></a>tooview všechny události procesu Syslog s analýzy protokolů
1. Hello Operations Management Suite portálu, klikněte na tlačítko hello **hledání protokolů** dlaždici.
2. V hello **Správa protokolu** seskupování, vyberte předdefinované syslog vyhledávání a pak vyberte jeden toorun ho.

Tento příklad ukazuje všechny události procesu Syslog.

![Ukazuje vyhledávání protokolu události procesu Syslog](./media/log-analytics-linux-agents/oms-linux-syslog.png)

Nyní můžete rozbalit výsledky hledání.

## <a name="linux-alerts"></a>Linux výstrahy
Pokud používáte Nagios nebo Zabbix toomanage počítače se systémem Linux a pak OMS může přijímat hello výstrahy generované z těchto nástrojů. Je však aktuálně žádná metoda tooconfigure příchozích dat výstrah pomocí portálu OMS hello. Místo toho je nutné tooedit konfigurační soubor toostart odesílání výstrah tooOMS.

### <a name="collect-alerts-from-nagios"></a>Shromažďovat výstrahy z Nagios
toocollect výstrahy ze serveru Nagios, musíte toomake hello následující změny konfigurace.

1. Udělení hello uživatele **omsagent** soubor protokolu Nagios toohello přístup pro čtení (tj. /var/log/nagios/nagios.log). Za předpokladu, že soubor nagios.log hello je vlastníkem skupiny hello **nagios** , můžete přidat uživatele hello **omsagent** toohello **nagios** skupiny.

    ```
    sudo usermod –a -G nagios omsagent
    ```
2. Upravte soubor omsagent.confconfiguration hello (/ etc/opt/microsoft/omsagent/&lt;id pracovního prostoru&gt;/conf/omsagent.conf). Zajistěte, aby hello následující položky jsou existuje a není komentáři se:

    ```
    <source>
    type tail
    #Update path toopoint tooyour nagios.log
    path /var/log/nagios/nagios.log
    format none
    tag oms.nagios
    </source>

    <filter oms.nagios>
    type filter_nagios_log
    </filter>
    ```
3. Restartujte démon omsagent hello:

    ```
    sudo /opt/microsoft/omsagent/bin/service_control restart
    ```

### <a name="collect-alerts-from-zabbix"></a>Shromažďovat výstrahy z Zabbix
toocollect výstrahy ze serveru Zabbix, budete provádět podobné toothose kroky pro Nagios výše, s výjimkou budete potřebovat toospecify uživatele a heslo v *text vymažte, pokud*. Toto není ideální, ale pravděpodobně bude brzy změnit. tooaddress tento problém, doporučujeme vytvořit hello uživatele a udělit mu oprávnění toomonitor pouze.

Na příkladu část hello omsagent.conf konfiguračního souboru (/ etc/opt/microsoft/omsagent/&lt;id pracovního prostoru&gt;/conf/omsagent.conf) pro Zabbix by měla vypadat přibližně hello následující:

```
<source>
  type zabbix_alerts
  run_interval 1m
  tag oms.zabbix
  zabbix_url http://localhost/zabbix/api_jsonrpc.php
  zabbix_username Admin
  zabbix_password zabbix
</source>

```

### <a name="view-alerts-in-log-analytics-search"></a>Zobrazit výstrahy ve vyhledávání analýzy protokolů
Po dokončení konfigurace vaší tooOMS Linux počítače toosend výstrahy, můžete použít několik jednoduchých protokolu vyhledávací dotazy tooview hello výstrahy. Hello následující příklad vyhledávací dotaz vrátí všechny hello zaznamenávají výstrahy, které byly vygenerovány. Například pokud nějaká problému dojde v infrastruktuře IT, pak výsledky pro hello následující ukázka, že dotaz může znamenat, kde může být problém hello pocházejí. A můžete snadno zobrazit další podrobnosti v toohello výstrahy ve zdrojovém systému toohelp úzké šetření. Hello výhodou je, že nemáte nutně systémy správy toovarious toogo od začátku hello – za předpokladu, že vaše výstrahy se posílají tooOMS, můžete spustit existuje.

```
Type=Alert
```

#### <a name="tooview-all-nagios-alerts-with-log-analytics"></a>tooview Nagios všechny výstrahy s analýzy protokolů
1. Hello Operations Management Suite portálu, klikněte na tlačítko hello **hledání protokolů** dlaždici.
2. V panelu hello dotazu zadejte hello následující vyhledávací dotaz.

    ```
    Type=Alert SourceSystem=Nagios
    ```
   ![Nagios výstrah zobrazených ve vyhledávání protokolu](./media/log-analytics-linux-agents/oms-linux-nagios-alerts.png)

Až se zobrazí výsledky hledání hello, můžete přejít do další podrobnosti, jako *AlertState*.

### <a name="tooview-all-zabbix-alerts-with-log-analytics"></a>tooview všechny výstrahy Zabbix s analýzy protokolů
1. Hello Operations Management Suite portálu, klikněte na tlačítko hello **hledání protokolů** dlaždici.
2. V panelu hello dotazu zadejte hello následující vyhledávací dotaz.

    ```
    Type=Alert SourceSystem=Zabbix
    ```
   ![Zabbix výstrah zobrazených ve vyhledávání protokolu](./media/log-analytics-linux-agents/oms-linux-zabbix-alerts.png)

Až se zobrazí výsledky hledání hello, můžete přejít do další podrobnosti, jako *AlertName*.

## <a name="compatibility-with-system-center-operations-manager"></a>Kompatibilita s nástrojem System Center Operations Manager
Hello OMS agenta pro Linux sdílí binárních souborů agenta s hello agenta System Center Operations Manager. Instalace hello OMS agenta pro Linux v systému aktuálně spravován nástrojem Operations Manager upgrady hello OMI a SCX balíčků na novější verzi tooa hello počítače. Hello OMS agenta pro Linux a System Center 2012 R2 jsou kompatibilní. Ale **System Center 2012 SP1 a starší verze nejsou aktuálně kompatibilní nebo není podporován s hello OMS agenta pro Linux.**

> [!NOTE]
> Pokud je nainstalované tooa počítač, který není aktuálně spravována nástrojem Operations Manager hello OMS agenta pro Linux a chcete později toomanage hello počítač s nástrojem Operations Manager, je třeba změnit konfiguraci OMI hello před zjistit počítač se hello. **Tento krok není nutný, pokud je nainstalován agent nástroje Operations Manager hello před hello OMS agenta pro Linux.**
>
>

### <a name="tooenable-hello-oms-agent-for-linux-toocommunicate-with-operations-manager"></a>tooenable hello OMS agenta pro Linux toocommunicate s nástrojem Operations Manager
1. Upravit soubor /etc/opt/omi/conf/omiserver.conf hello
2. Ujistěte se, že hello řádek začínající **httpsport =** definuje hello port 1270. Například`httpsport=1270`
3. Restartujte hello OMI server:

    ```
    sudo /opt/omi/bin/service_control restart
    ```

## <a name="database-permissions-required-for-mysql-performance-counters"></a>Databáze oprávnění vyžadovaná pro čítače výkonu databáze MySQL
toogrant oprávnění tooa MySQL uživatele monitorování, hello poskytující uživatel musí mít oprávnění "Udělit možnost" hello, jakož i udělení oprávnění hello.

Aby hello MySQL uživatele tooreturn výkonu dat hello uživatel bude potřebovat přístup k toohello následující dotazy:

```
SHOW GLOBAL STATUS;
SHOW GLOBAL VARIABLES:
```

Kromě toho toothese dotazy hello MySQL uživatel vyžaduje následující výchozí tabulky toohello vyberte přístup:

* INFORMATION_SCHEMA
* MySQL

Tato oprávnění lze udělit tak, že spustíte následující příkazy grant hello.

```
GRANT SELECT ON information_schema.* too‘monuser’@’localhost’;
GRANT SELECT ON mysql.* too‘monuser’@’localhost’;
```

## <a name="manage-mysql-monitoring-credentials-in-hello-authentication-file"></a>Spravovat MySQL monitorování přihlašovací údaje v souboru authentication hello
Hello následující části snadněji tak můžete spravovat MySQL přihlašovací údaje.

### <a name="configure-hello-mysql-omi-provider"></a>Konfigurace hello MySQL OMI zprostředkovatele
Hello MySQL OMI zprostředkovatele vyžaduje předkonfigurované uživatele MySQL a nainstalovat MySQL klientské knihovny v pořadí tooquery hello stavu nebo výkonu informace z instance databáze MySQL hello.

### <a name="mysql-omi-authentication-file"></a>Soubor pro ověření MySQL OMI
MySQL OMI zprostředkovatele používá k ověření souboru toodetermine instance jaké vazby adresu a port, MySQL hello naslouchá na a jaké přihlašovací údaje toouse toogather metriky. Během instalace hello MySQL OMI zprostředkovatele vyhledá MySQL my.cnf konfigurační soubory (výchozí umístění) vazby adresy a portu a částečně sadu hello soubor pro ověření MySQL OMI.

toocomplete monitorování instance serveru databáze MySQL, přidá soubor předem vygenerovaná ověřování MySQL OMI do správného adresáře hello.

### <a name="authentication-file-format"></a>Formát souboru ověřování
Hello MySQL OMI ověřování soubor je textový soubor, který obsahuje informace o:

* Port
* Vazby – adresy
* Uživatelské jméno MySQL
* Heslo s kódováním base64

Hello souboru MySQL OMI authentication pouze uděluje oprávnění pro čtení/zápisu toohello Linux uživatele, která ji vygenerovala.

```
[Port]=[Bind-Address], [username], [Base64 encoded Password]
(Port)=(Bind-Address), (username), (Base64 encoded Password)
(Port)=(Bind-Address), (username), (Base64 encoded Password)
AutoUpdate=[true|false]
```

Výchozí soubor pro ověření MySQL OMI obsahuje výchozí instance a číslo portu v závislosti na tom, jaké informace je k dispozici a Analyzovaná z hello nalezen konfigurační soubor MySQL.

Hello výchozí instance je znamená toomake, Správa více instancí databáze MySQL na jednom hostiteli systému Linux jednodušší a je označený jako instance hello s portu 0. Všechny instance přidané zdědí vlastnosti nastavit od hello výchozí instanci. Například pokud je přidána instance databáze MySQL naslouchání na portu '3308', vazby adresu hello výchozí instance, uživatelské jméno a heslo kódováním Base64 bude použité tootry a monitorovat naslouchání na 3308 instance hello. Pokud instance hello na 3308 je vázaný tooanother adresu a hello používá stejné uživatelské jméno MySQL a pár heslo pouze hello respecification hello je vyžadována adresa vazby a hello ostatní vlastnosti zdědí.

Příklady hello ověřování souboru vypadat podobně jako následující hello.

Výchozí instance a instanci s portem 3308:

```
0=127.0.0.1, myuser, cnBwdA==3308=, ,AutoUpdate=true
```

Výchozí instance a instance s port 3308 + různých Base 64 kódovaný heslo:

```
0=127.0.0.1, myuser, cnBwdA==3308=127.0.1.1, , AutoUpdate=true
```


| **Vlastnost** | **Popis** |
| --- | --- |
| Port |Port představuje hello aktuální port hello instance naslouchá na MySQL.  Hello port 0 znamená, že následující vlastnosti hello jsou pro výchozí instanci. |
| Vazby – adresy |Hello vazby adresa je hello aktuální MySQL vazby adresa |
| uživatelské jméno |Toto uživatelské jméno hello hello MySQL uživatele chcete instanci serveru toouse toomonitor hello MySQL. |
| Kódováním base64, pomocí hesla |Toto je heslo hello hello MySQL monitorování uživatele kódovaný jako Base64. |
| Pomocí funkce Automatické aktualizace |Upgradován hello MySQL OMI zprostředkovatele hello zprostředkovatele bude znovu zkontrolujte změny v souboru my.cnf hello a přepsat soubor hello MySQL OMI ověřování. Nastavte tento příznak tootrue nebo NEPRAVDA v závislosti na požadované aktualizace toohello MySQL OMI soubor pro ověření. |

#### <a name="authentication-file-location"></a>Umístění souboru ověřování
Hello MySQL OMI ověřování souboru by mělo být umístěný v hello následující umístění a s názvem "mysql ověření":

/var/OPT/Microsoft/MySQL-cimprov/auth/omsagent/MySQL-auth

Hello souboru (a ověřování nebo omsagent directory) by měl být vlastněných uživatelem omsagent hello.

## <a name="agent-logs"></a>Protokoly agenta
Hello protokoly pro hello OMS agenta pro Linux je na:

/ var/opt/microsoft/omsagent/&lt;id pracovního prostoru&gt;/log/

Hello protokoly pro hello OMS agenta pro Linux programu omsconfig (Konfigurace agenta) je na:

/ var/opt/microsoft/omsconfig nebo protokolu nebo

Protokoly pro součásti pro OMI a SCX hello (které poskytují data metriky výkonu) je na:

/ var/opt/omi/log/a /var/opt/microsoft/scx/log

## <a name="troubleshooting-hello-oms-agent-for-linux"></a>Řešení potíží s hello OMS agenta pro Linux
Použijte následující informace toodiagnose hello a řešení běžných problémů.

Pokud žádná z hello řešení potíží s informace v této části vám pomůže, můžete taky hello následující prostředky toohelp vyřešit váš problém.

* Zákazníci s Premier support může přihlásit případu podpory prostřednictvím [úrovně Premier](https://premier.microsoft.com/)
* Zákazníci s smlouvy podporu Azure se můžou přihlásit případů podpory v hello [portálu Azure](https://manage.windowsazure.com/?getsupport=true)
* Soubor [potíže Githubu](https://github.com/Microsoft/OMS-Agent-for-Linux/issues)
* Fóru pro zpětnou vazbu pro návrhy a toocreate sestavy chyb [http://aka.ms/opinsightsfeedback](http://aka.ms/opinsightsfeedback)

### <a name="important-log-locations"></a>Umístění důležité protokolů
| File | Cesta |
| --- | --- |
| Agent OMS pro soubor protokolu systému Linux |`/var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log ` |
| V souboru protokolu konfigurace agenta OMS |`/var/opt/microsoft/omsconfig/omsconfig.log` |

### <a name="important-configuration-files"></a>Důležité konfigurační soubory
| Catergory | Umístění souboru |
| --- | --- |
| Syslog |`/etc/syslog-ng/syslog-ng.conf`nebo `/etc/rsyslog.conf` nebo`/etc/rsyslog.d/95-omsagent.conf` |
| Výkon, Nagios, Zabbix, OMS výstup a obecné agenta |`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` |
| Další konfigurace |`/etc/opt/microsoft/omsagent/<workspace id>/omsagent.d/*.conf` |

> [!NOTE]
> Úpravy konfigurační soubory pro čítače výkonu a syslog jsou přepsány, pokud je povoleno nastavení portálu OMS. Můžete zakázat konfiguraci v hello portálu OMS (pro všechny uzly) nebo pro jeden uzly spuštěním hello následující:
>
>

```
sudo su omsagent -c /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py --disable
```


### <a name="enable-debug-logging"></a>Povolit protokolování ladění
ladění tooenable protokolování, můžete použít modul plug-in výstup hello OMS a podrobný výstup.

#### <a name="oms-output-plugin"></a>Modul plug-in výstup OMS
FluentD umožňuje hello modul plug-in toospecify úrovně protokolování pro úrovně jiný protokol pro vstupy a výstupy. toospecify úrovní jiný protokol pro výstup OMS upravit konfiguraci hello obecné agenta v hello `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` souboru.

Téměř hello dolní části hello konfigurační soubor, změňte hello `log_level` vlastnost z `info` příliš`debug`.

 ```
 <match oms.** docker.**>
  type out_oms
  log_level debug
  num_threads 5
  buffer_chunk_limit 5m
  buffer_type file
  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms*.buffer
  buffer_queue_limit 10
  flush_interval 20s
  retry_limit 10
  retry_wait 30s
</match>
 ```

Protokolování ladění vám umožní toosee zpracovat v dávce nahrávání toohello OMS služby oddělených typ, počet datových položek a doba trvání toosend.

*Příklad povoleno ladění protokolu:*

```
Success sending oms.nagios x 1 in 0.14s
Success sending oms.omi x 4 in 0.52s
Success sending oms.syslog.authpriv.info x 1 in 0.91s
```

#### <a name="verbose-output"></a>Podrobný výstup
Místo použití modulu plug-in výstup hello OMS, také výstup můžete položky dat přímo příliš`stdout`, který se zobrazí v hello agenta OMS pro soubor protokolu systému Linux.

V souboru konfigurace obecné agenta hello OMS na `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`, okomentujte hello OMS výstup přidáním modulů plug-in `#` před každý řádek.

```
#<match oms.** docker.**>
#  type out_oms
#  log_level info
#  num_threads 5
#  buffer_chunk_limit 5m
#  buffer_type file
#  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms*.buffer
#  buffer_queue_limit 10
#  flush_interval 20s
#  retry_limit 10
#  retry_wait 30s
#</match>
```

Níže hello výstup modulu plug-in, odeberte hello komentář v následující části odebráním hello hello `#` symbol na hello začátku každého řádku.

```
<match **>
  type stdout
</match>
```

### <a name="forwarded-syslog-messages-do-not-appear-in-hello-log"></a>Přesměrovaná zprávy Syslog se nezobrazí v protokolu hello
#### <a name="probable-causes"></a>Možných příčin
* Hello použitá konfigurace toohello Linux serveru povolit kolekce zařízení hello odeslat nebo protokolování úrovně
* Syslog není předávaná správně toohello Linux server
* jsou příliš velké vzhledem k základní konfiguraci hello hello OMS agenta pro Linux toohandle Hello počet zpráv předávaná za sekundu

#### <a name="resolutions"></a>Řešení
* Ověřte konfiguraci tohoto hello v hello portálu OMS, pro Syslog má všechny hello zařízení a úrovně správné protokolu hello
  * **Portál OMS > Nastavení > Data > Syslog**
* Ověřte, že nativní syslog zasílání zpráv démoni (`rsyslog`, `syslog-ng`) jsou možné tooreceive hello předávat zprávy
* Zkontrolujte nastavení brány firewall na tooensure serveru Syslog hello nejsou blokovány zprávy
* Simulovat tooOMS zprávy Syslog pomocí hello `logger` příkaz – například:
  * `logger -p local0.err "This is my test message"`

### <a name="problems-connecting-toooms-when-using-a-proxy"></a>Potíže s připojením tooOMS při použití serveru proxy
#### <a name="probable-causes"></a>Možných příčin
* Hello proxy zadaný při instalaci a konfiguraci agenta hello je nesprávný
* Koncové body služby OMS Hello nejsou whitelistested ve vašem datovém centru

#### <a name="resolutions"></a>Řešení
* Přeinstalujte hello OMS agenta pro Linux pomocí hello následující příkaz s možností hello `-v` povolena. To umožňuje podrobný výstup hello agenta připojení prostřednictvím hello proxy toohello služby OMS.
  * `/opt/microsoft/omsagent/bin/omsadmin.sh -w <OMS Workspace ID> -s <OMS Workspace Key> -p <Proxy Conf> -v`
  * Přečtěte si dokumentaci k hello proxy OMS na [konfigurace hello agenta pro použití se HTTP proxy server](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#configuring-the-agent-for-use-with-an-http-proxy-server)
* Ověřte, že hello následující koncové body služby OMS jsou seznam povolených adres

| Prostředek agenta | Porty |
| --- | --- |
| &#42;. Ods.opinsights.Azure.com |Port 443 |
| &#42;. OMS.opinsights.Azure.com |Port 443 |
| ods.systemcenteradvisor.com |Port 443 |
| &#42;.blob.core.windows.net/ |Port 443 |

### <a name="a-403-error-is-displayed-when-onboarding"></a>Zobrazí se Chyba 403 při připojování
#### <a name="probable-causes"></a>Možných příčin
* Hello datum a čas jsou nesprávná na Linux Server
* Hello ID pracovního prostoru a používá klíč pracovního prostoru jsou nesprávné

#### <a name="resolution"></a>Řešení
* Ověřte hello čas na serveru Linux s hello `date` příkaz. Pokud hello dat je větší nebo menší než 15 minut od hello aktuální čas, registrace se nezdaří. toocorrect, aktualizovat hello datum a časové pásmo serveru Linux.
* nejnovější verzi hello OMS agenta pro Linux Hello vás upozorní, pokud je časový rozdíl je příčinou selhání registrace
* Znovu zařadit pomocí hello správné ID pracovního prostoru a klíč pracovního prostoru. V tématu [registrace pomocí příkazového řádku hello](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) Další informace.

### <a name="a-500-error-or-404-error-appears-in-hello-log-file-after-onboarding"></a>500 Chyba nebo chybu 404 se zobrazí v souboru protokolu hello po registraci
Jde o známý problém, který spadá hello první nahrávání dat Linux do pracovního prostoru OMS. To nemá vliv odesílat data nebo jiné problémy. Můžete ignorovat hello chyby, když se od začátku registrace.

### <a name="nagios-data-does-not-appear-in-hello-oms-portal"></a>Nagios data se nezobrazují v hello portálu OMS
#### <a name="probable-causes"></a>Možných příčin
* Hello omsagent uživatel nemá oprávnění tooread ze souboru protokolu Nagios hello
* Hello Nagios zdroje a filtru oddílů jsou stále vloženy do komentáře v souboru omsagent.conf hello

#### <a name="resolutions"></a>Řešení
* Přidáte uživatele omsagent hello v pořadí tooread ze souboru Nagios hello. V tématu [Nagios výstrahy](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#nagios-alerts) Další informace.
* V hello OMS agenta pro Linux obecné konfiguračního souboru v `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`, ujistěte se, že **i** hello Nagios zdroje a filtr sekce mají komentáře k odebrání, podobně jako toohello následující ukázka.

```
<source>
  type tail
  path /var/log/nagios/nagios.log
  format none
  tag oms.nagios
</source>

<filter oms.nagios>
  type filter_nagios_log
</filter>
```


### <a name="linux-data-doesnt-appear-in-hello-oms-portal"></a>Linux data nezobrazí v hello portálu OMS
#### <a name="probable-causes"></a>Možných příčin
* Registrace toohello OMS služby se nezdařilo
* Připojení toohello OMS služby je blokovaný.
* je Hello OMS agenta pro Linux data zálohovaná

#### <a name="resolutions"></a>Řešení
* Ověřte, že registrace toohello OMS služby bylo úspěšné kontrolou tohoto hello `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf` existuje.
* Znovu zařadit pomocí hello omsadmin.sh příkazového řádku. V tématu [registrace pomocí příkazového řádku hello](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) Další informace.
* Pokud používáte proxy server, používejte hello proxy pro řešení potíží výše
* V některých případech při hello OMS agenta pro Linux nemůže komunikovat s hello OMS služby data na hello agenta je zálohovaná toohello úplné vyrovnávací paměti velikost 50 MB. Restartujte hello OMS agenta pro Linux spuštěním hello `/opt/microsoft/omsagent/bin/service_control restart` příkaz.
  >[AZURE.NOTE] Tento problém vyřešen v 1.1.0-28 verze agenta nebo novější.

### <a name="syslog-linux-performance-counter-configuration-is-not-applied-in-hello-oms-portal"></a>Konfigurace čítače výkonu Syslog Linux není použita na portálu OMS hello
#### <a name="probable-causes"></a>Možných příčin
* Hello konfiguraci agenta v hello OMS agenta pro Linux nebyla načtena hello nejnovější konfigurace z portálu OMS hello.
* Hello revidovaný nastavení portálu hello nebyly použity

#### <a name="resolutions"></a>Řešení
`omsconfig`je hello konfiguraci agenta v hello OMS agenta pro Linux, který načte změny konfigurace portálu OMS každých 5 minut. Tato konfigurace je pak použité toohello OMS agenta pro Linux konfigurační soubory umístěné v `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`.

* V některých případech hello OMS agenta pro Linux konfigurace agenta nemusí být možné toocommunicate službou hello konfigurace portálu, což vede k nejnovější konfigurace nebudou použity.
* Ověřte, že hello `omsconfig` je agent nainstalovaný s hello následující:

  * `dpkg --list omsconfig` nebo `rpm -qi omsconfig`
  * Pokud nainstalovaná není, znovu nainstalujte nejnovější verzi hello agenta OMS hello pro Linux
* Ověřte, že hello `omsconfig` agent může komunikovat s hello OMS služby

  * Spustit hello `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'` příkaz
    * Hello nahoře na příkaz vrátí hello konfiguraci tohoto agenta načte z hello portálu, včetně nastavení Syslog, čítače výkonu systému Linux a vlastní protokoly
    * Pokud se výše hello příkazu nezdaří, spusťte hello `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py` příkaz. Tento příkaz zajistí hello omsconfig agenta toocommunicate s hello OMS služby tooretrieve hello nejnovější konfigurací.

### <a name="custom-linux-log-data-does-not-appear-in-hello-oms-portal"></a>Vlastní data protokolu Linux nejsou uvedené v hello portálu OMS
#### <a name="probable-causes"></a>Možných příčin
* Registrace tooOMS služby se nezdařilo
* Hello **hello použít následující konfigurace toomy servery se systémem Linux** nebylo vybráno nastavení
* omsconfig nebyl zachyceny nejnovější vlastní protokol hello z portálu hello
* Hello `omsagent` použití je nelze tooaccess hello vlastního protokolu z důvodu problému oprávnění tooa nebo `omsagent` nebyl nalezen. V takovém případě se zobrazí hello následující výstup:
  * `[DATETIME] [warn]: file not found. Continuing without tailing it.`
  * `[DATETIME] [error]: file not accessible by omsagent.`
* Jedná se o známý problém s hello časování, která byla opravena v hello OMS agenta pro Linux verze 1.1.0-217

#### <a name="resolutions"></a>Řešení
* Ověřte, že jste úspěšně zařazený, nemá tak, že určíte, zda hello `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf` soubor existuje.
  * Pokud potřebné, zařadit znovu s použitím hello omsadmin.sh příkazového řádku. V tématu [registrace pomocí příkazového řádku hello](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) Další informace.
* V hello portálu OMS, v části **nastavení** na hello **Data** kartě, ujistěte se, že hello **hello použít následující konfigurace toomy servery se systémem Linux** je vybráno nastavení  
  ![použít konfiguraci](./media/log-analytics-linux-agents/customloglinuxenabled.png)
* Ověřte, že hello `omsconfig` agent může komunikovat s hello OMS služby

  * Spustit hello `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'` příkaz
  * Hello nahoře na příkaz vrátí hello konfiguraci tohoto agenta načte z portálu, včetně nastavení Syslog, čítače výkonu systému Linux a vlastní protokoly hello
  * Pokud se výše hello příkazu nezdaří, spusťte hello `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py` příkaz. Tento příkaz vynutí hello omsconfig agenta toocommunicate službou OMS a načíst nejnovější konfigurace hello.

Místo hello OMS agenta pro Linux uživatel, který spouští jako privilegovaných uživatelů `root`, hello OMS agenta pro Linux běží jako hello `omsagent` uživatele. Ve většině případů musí být výslovná oprávnění udělená toohello uživateli v pořadí tooread určité soubory.

oprávnění toogrant příliš`omsagent` uživatele, spusťte následující příkazy hello:

1. Přidat hello `omsagent` konkrétní skupiny uživatelů tooa s`sudo usermod -a -G <GROUPNAME> <USERNAME>`
2. Požadovaný soubor toohello grant univerzální přístup pro čtení s`sudo chmod -R ugo+rw <FILE DIRECTORY>`

Je známý problém s hello časování, která byla opravena v hello OMS agenta pro Linux verze 1.1.0-217. Po aktualizaci toohello nejnovější verze agenta, spusťte následující příkaz tooget hello nejnovější verzi modulů plug-in výstup hello hello:

```
sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.conf /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf
```

## <a name="known-limitations"></a>Známá omezení
Zkontrolujte následující oddíly toolearn o aktuálních omezeních hello OMS agenta pro Linux hello.

### <a name="azure-diagnostics"></a>Diagnostika Azure
Pro virtuální počítače Linux běží v Azure mohou být další kroky požadované tooallow shromažďování dat pomocí diagnostiky Azure a Operations Management Suite. **Verze 2.2** hello rozšíření diagnostiky pro Linux je vyžadována pro kompatibilitu s hello OMS agenta pro Linux.

Další informace o instalaci a konfiguraci hello rozšíření diagnostiky pro Linux najdete v tématu [použít hello rozhraní příkazového řádku Azure příkaz tooenable rozšíření diagnostiky Linux](../virtual-machines/linux/classic/diagnostic-extension-v2.md#use-the-azure-cli-command-to-enable-the-linux-diagnostic-extension).

**Upgrade hello rozšíření diagnostiky Linux z 2.0 too2.2 Azure CLI ASM:**

```
azure vm extension set -u <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions 2.0
azure vm extension set <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions 2.2 --private-config-path PrivateConfig.json
```

**ARM**

```
azure vm extension set -u <resource-group> <vm-name> Microsoft.Insights.VMDiagnosticsSettings Microsoft.OSTCExtensions 2.0
azure vm extension set <resource-group> <vm-name> LinuxDiagnostic Microsoft.OSTCExtensions 2.2 --private-config-path PrivateConfig.json
```

Tyto příklady příkaz odkazovat na soubor s názvem PrivateConfig.json. Formát Hello tohoto souboru by měla vypadat přibližně hello následující ukázka.

```
    {
    "storageAccountName":"hello storage account tooreceive data",
    "storageAccountKey":"hello key of hello account"
    }
```

### <a name="sysklog-is-not-supported"></a>Sysklog není podporován.
Rsyslog nebo syslog ng jsou požadované toocollect zprávy syslog. Démon procesu syslog výchozí Hello na verze 5 Red Hat Enterprise Linux a CentOS, Oracle Linux verze (sysklog) není podporována pro shromažďování událostí syslog. toocollect syslog data z této verze těchto distribuce hello rsyslog démon by měl být nainstalován a nakonfigurován tooreplace sysklog. Další informace o nahrazení sysklog rsyslog najdete v tématu [nainstalovat hello nově vytvořené rsyslog RPM](http://wiki.rsyslog.com/index.php/Rsyslog_on_CentOS_success_story#Install_the_newly_built_rsyslog_RPM).

## <a name="next-steps"></a>Další kroky
* [Přidat řešení pro analýzu protokolu z hello řešení Galerie](log-analytics-add-solutions.md) tooadd funkce a shromáždění dat.
* Seznamte se s [protokolu hledání](log-analytics-log-searches.md) tooview podrobné informace shromážděné řešení.
* Použití [řídicí panely](log-analytics-dashboards.md) toosave a zobrazit prohledává vlastní vlastní.
