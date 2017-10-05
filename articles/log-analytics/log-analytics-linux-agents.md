---
redirect_url: /azure/log-analytics/log-analytics-agent-linux
redirect_document_id: TRUE
ROBOTS: NOINDEX
ms.openlocfilehash: 8332bdd39effab8c2ac9a75ca9a1e2510c940719
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="connect-your-linux-computers-to-log-analytics"></a>Připojení počítačů Linux k analýzy protokolů
Pomocí analýzy protokolů, můžete shromažďovat a fungovat na informace shromážděné z počítače se systémem Linux. Přidání data shromážděná z Linux k OMS umožňuje spravovat systémy Linux a kontejner řešení jako Docker, bez ohledu na to, kde jsou počítače umístěny – prakticky odkudkoli. Zdroje dat mohou být umístěné ve vašem datovém centru místní jako fyzické servery, virtuální počítače ve službě hostovaných v cloudu jako Amazon Web Services (AWS) nebo Microsoft Azure nebo i přenosných počítačů na vaše oddělení podpory. Kromě toho OMS taky shromažďuje data z počítače se systémem Windows podobně, takže podporuje hybridním skutečně IT prostředí.

Můžete zobrazit a spravovat data ze všech těchto zdrojů s analýzy protokolů v OMS pomocí portálu správy. Tím se snižuje potřebu pomocí mnoha různými systémy, díky usnadňují využívat a vy můžete exportovat do ať řešení obchodní analýzy nebo systém, který už máte data, která chcete sledovat.

Tento článek je rychlý úvodní příručce, která vám pomůže shromažďovat a spravovat data pro počítače Linux pomocí agenta OMS pro Linux. Další technické podrobnosti jako konfiguraci proxy serveru, informace o CollectD metriky a vlastní zdroje dat JSON najdete tyto informace v [OMS agenta pro Linux přehled](https://github.com/Microsoft/OMS-Agent-for-Linux) a [OMS agenta pro Linux úplné dokumentace](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md) na Githubu.

Z počítače se systémem Linux v současné době můžete shromažďovat následující typy dat:

* Metriky výkonu
* Události procesu Syslog
* Výstrahy od Nagios a Zabbix
* Metriky výkonu kontejner docker, inventář a protokoly

## <a name="supported-linux-versions"></a>Podporované verze systému Linux
Verze x86 a x64 jsou oficiálně podporované pro řadu distribucí systému Linux. OMS agenta pro Linux mohou však spustit také na dalších distribuce, které nejsou uvedené.

* Linux Amazon 2012.09 prostřednictvím 2015.09
* CentOS Linux 5, 6 a 7
* Oracle Linux 5, 6 a 7
* Red Hat Enterprise Linux Server 5, 6 a 7
* Debian GNU/Linux 6, 7 a 8
* Ubuntu 12.04 LTS, 14.04 LTS, 15.04, 15.10
* SUSE Linux Enterprise Server 11 a 12

## <a name="oms-agent-for-linux"></a>OMS agenta pro Linux
Agent nástroje Operations Management Suite pro Linux obsahuje více balíčků. Verze souboru obsahuje následující balíčky k dispozici spuštěním sady prostředí s `--extract`.

| **Balíček** | **Verze** | **Popis** |
| --- | --- | --- |
| omsagent |1.1.0 |Agent nástroje Operations Management Suite pro Linux |
| omsconfig |1.1.1 |Konfigurace agenta pro agenta OMS |
| OMI |1.0.8.3 |Infrastruktury Open Management Infrastructure (OMI) – prosté Server CIM |
| scx. |1.6.2 |Zprostředkovatelé CIM OMI pro metriku výkonu operačního systému |
| Apache cimprov |1.0.0 |Zprostředkovatel pro OMI monitorování výkonu serveru Apache HTTP Server. Instalovat pouze v případě zjištění serveru Apache HTTP Server. |
| MySQL cimprov |1.0.0 |Výkon serveru MySQL monitorování zprostředkovatele pro OMI. Instalovat pouze v případě zjištění MySQL nebo MariaDB serveru. |
| docker cimprov |0.1.0 |Zprostředkovatel docker pro OMI. Instalovat pouze v případě zjištění Docker. |

### <a name="additional-installation-artifacts"></a>Artefakty další instalace
Po instalaci agenta OMS pro balíčky Linux, se použijí následující další konfiguraci systémové změny. Tyto artefakty se odeberou, když omsagent balíček odinstalován.

* Bez oprávnění uživatele s názvem: `omsagent` je vytvořena. Toto je účet, který spouští démon omsagent jako
* Vytvoření souboru sudoers "zahrnout" v /etc/sudoers.d/omsagent to autorizuje omsagent restartovat démoni syslog a omsagent. Pokud direktivy "zahrnout" sudo nejsou podporovány v nainstalované verzi sudo, tyto položky budou zapsány do /etc/sudoers.
* Konfigurace syslog je upravit tak, aby předávat podmnožinu události agenta. Další informace najdete v tématu **konfigurace shromažďování dat** části

### <a name="linux-data-collection-details"></a>Podrobnosti kolekce dat Linux
Následující tabulka uvádí metody shromažďování dat a další podrobnosti o tom, jak se data shromažďují.

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
> Rsyslog nebo syslog ng jsou požadované ke shromáždění zprávy syslog. Démon procesu syslog výchozí na verze 5 Red Hat Enterprise Linux a CentOS, Oracle Linux verze (sysklog) není podporována pro shromažďování událostí syslog. Ke shromažďování dat syslog z této verze těchto distribuce, musí být nainstalovaná a nakonfigurovaná nahradit sysklog démon rsyslog.
>
>

## <a name="quick-install"></a>Rychlé instalace
Spusťte následující příkazy ke stažení omsagent, ověření kontrolního součtu a pak nainstalovat i integrovaného agenta. Příkazy jsou pro 64bitovou verzi. ID pracovního prostoru a primární klíč se nacházejí na portálu OMS pod **nastavení** na **připojené zdroje** kartě.

![Podrobnosti o pracovním prostoru](./media/log-analytics-linux-agents/oms-direct-agent-primary-key.png)

```
wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR OMS WORKSPACE ID> -s <YOUR OMS WORKSPACE PRIMARY KEY>
```

Existuje mnoho různých dalších metod k instalaci agenta a upgradujte ho. Další informace o nich v [kroky pro instalaci agenta OMS pro Linux](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#steps-to-install-the-oms-agent-for-linux).

Můžete také zobrazit [Azure video s návodem](https://www.youtube.com/watch?v=mF1wtHPEzT0).

## <a name="choose-your-linux-data-collection-method"></a>Zvolte metodu kolekce dat Linux
Tom, jak provedete typy dat, které chcete shromažďovat závisí na, zda chcete použít na portálu OMS nebo pokud chcete upravit různé konfigurační soubory přímo na klienty Linux. Pokud se rozhodnete používat portál, je konfigurace odeslány na všechny klienty Linux automaticky. Pokud potřebujete různé konfigurace pro jiné klienty Linux, musíte upravit soubory klienta jednotlivě – nebo použijte alternativu jako PowerShell DSC, Chef nebo Puppet.

Můžete zadat události procesu syslog a čítače výkonu, které chcete shromáždit pomocí konfigurační soubory do počítače se systémem Linux. *Pokud jste se rozhodli nakonfigurovat shromažďování dat úpravou konfigurační soubory agenta, měli byste zakázat Centralizovaná konfigurace.*  Pokyny najdete níže ke konfiguraci shromažďování dat v konfiguračních souborech na agenta a také zakázat centrální konfigurace pro všechny agenty OMS pro Linux nebo jednotlivé počítače.

### <a name="disable-oms-management-for-an-individual-linux-computer"></a>Zakázat správu OMS pro jednotlivý počítač Linux
Centralizované shromažďování dat pro konfigurační data je zakázána pro jednotlivý počítač Linux pomocí skriptu OMS_MetaConfigHelper.py. To může být užitečné, pokud podmnožině počítačů by měl mít speciální konfigurace.

Postup při zakázání Centralizovaná konfigurace:

```
sudo /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py --disable
```

Chcete-li znovu povolit Centralizovaná konfigurace:

```
sudo /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py –enable
```

## <a name="linux-performance-counters"></a>Čítače výkonu systému Linux
Čítače výkonu systému Linux jsou podobná čítačů výkonu systému Windows, jak fungují podobně. Přidání a konfigurace je můžete použít následující postupy. Po přidání do OMS, data jsou shromažďována pro ně každých 30 sekund.

### <a name="to-add-a-linux-performance-counter-in-oms"></a>Chcete-li přidat čítače výkonu Linux v OMS
1. Chcete-li nakonfigurovat OMS agentů pro Linux pomocí portálu OMS, můžete přidat čítače výkonu Linux na stránce nastavení, klikněte na **Data**.  
2. Na **nastavení** v části **Data** , klikněte na tlačítko **čítače výkonu Linux** a pak vyberte nebo zadejte název čítače, které chcete přidat.  
    ![data](./media/log-analytics-linux-agents/oms-settings-data01.png)
3. Pokud si nejste jisti úplným názvem čítače, pište částečný název a zobrazí se seznam dostupných čítačů. Pokud zjistíte, čítač, který chcete přidat, klikněte na název v seznamu a pak klikněte na ikonu plus přidat čítač.
4. Po přidání čítač se zobrazí v seznamu čítačů zvýrazněná s barevnou panelu.
5. Ve výchozím nastavení **použít dole uvedenou konfiguraci u mých počítačů** je vybraná možnost. Pokud chcete zakázat odesílání konfiguračních dat, zrušte zaškrtnutí políčka.
6. Po dokončení změny čítače výkonu, v dolní části stránky klikněte na tlačítko **Uložit** na provedené změny se dokončí. Změny konfigurace, které jste udělali jsou poté odeslány všechny agenty OMS pro Linux, které jsou registrovány OMS, obvykle během pěti minut.

### <a name="configure-linux-performance-counters-in-oms"></a>Konfigurovat čítačů výkonu systému Linux v OMS
Pro čítače výkonu systému Windows můžete konkrétní instance jednotlivých čítačů výkonu. Pro čítače výkonu systému Linux, ale jakoukoli instanci čítače, který zvolíte, platí pro všechny podřízené čítače nadřazené čítače. V následující tabulce jsou uvedeny běžné instancí, které jsou k dispozici pro Linux a Windows čítače výkonu.

| **Název instance** | **Význam** |
| --- | --- |
| \_Celkový počet |Celkový počet všech instancí |
| \* |Všechny instance |
| (/ &#124; / var) |Odpovídá instancí s názvem: / nebo /var |

Podobně intervalu vzorkování, který zvolíte, čítače nadřazené platí pro všechny jeho podřízené čítače. Jinými slovy jsou všechny podřízené intervalů vzorkování čítače a instance svázané společně.

### <a name="add-and-configure-performance-metrics-with-linux"></a>Přidání a konfigurace metriky výkonu operačního systému Linux
Metriky výkonu ke shromažďování jsou řízeny konfigurace v/etc/opt/microsoft/omsagent/&lt;id pracovního prostoru&gt;/conf/omsagent.conf. V tématu [metriky výkonu k dispozici](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#appendix-available-performance-metrics) dostupných tříd a metriky pro agenta OMS pro Linux.

Každý objekt nebo kategorie metrik výkonu ke shromažďování by měl být definována v konfiguračním souboru jako jeden `<source>` elementu. Syntaxe následující níže.

```
<source>
  type oms_omi  
  object_name "Processor"
  instance_regex ".*"
  counter_name_regex ".*"
  interval 30s
</source>

```

Konfigurovat parametry tohoto elementu jsou:

* **Objekt\_název**: název objektu pro kolekci.
* **Instance\_regex**: *regulární výraz* definování instance, které ke shromažďování. Hodnota: `.*` Určuje všechny instance. Ke shromažďování metrik procesoru pro pouze \_celkový počet instancí, můžete zadat `_Total`. Ke shromažďování metrik proces pro pouze crond nebo sshd instancí, můžete zadat: `(crond|sshd)`.
* **Čítač\_název\_regex**: *regulární výraz* definice, které ke shromažďování čítače (pro objekt). Chcete-li shromažďovat všechny čítače pro objekt, zadejte: `.*`. Chcete-li shromažďovat pouze čítače místa odkládacího souboru objektu paměti, můžete zadat:`.+Swap.+`
* **Interval:**: frekvence, na které se shromažďují objektu čítače.

Výchozí konfiguraci pro metriku výkonu je:

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
Pokud Server databáze MySQL nebo MariaDB Server je v počítači nalezen při instalaci sady omsagent, automaticky se nainstaluje zprostředkovatele pro Server databáze MySQL monitorování výkonu. Tento zprostředkovatel připojí k místnímu serveru MySQL nebo MariaDB vystavit statistiku výkonu. Budete muset nakonfigurovat přihlašovací údaje uživatele MySQL tak, aby zprostředkovatel můžete přístup k serveru databáze MySQL.

Chcete-li definovat výchozí uživatelského účtu pro server databáze MySQL na místního hostitele, použijte v následujícím příkladu příkaz.

> [!NOTE]
> Soubor s přihlašovacími údaji musí být účet omsagent. Doporučuje se spuštěním příkazu mycimprovauth jako omsgent.
>
>

```
sudo su omsagent -c '/opt/microsoft/mysql-cimprov/bin/mycimprovauth default 127.0.0.1 <username> <password>

sudo /opt/omi/bin/service_control restart
```


Alternativně můžete zadat požadovaná pověření MySQL v souboru, vytváření souboru: /var/opt/microsoft/mysql-cimprov/auth/omsagent/mysql-auth. Další informace o správě MySQL přihlašovací údaje pro monitorování prostřednictvím soubor mysql ověřování najdete v části [spravovat MySQL monitorování přihlašovací údaje v souboru authentication](#manage-mysql-monitoring-credentials-in-the-authentication-file).

V tématu [databáze oprávnění vyžadovaná pro čítače výkonu databáze MySQL](#database-permissions-required-for-mysql-performance-counters) podrobnosti o oprávnění objektu MySQL uživatel požaduje ke shromažďování dat výkonu serveru MySQL.

### <a name="enable-apache-http-server-performance-counters-using-linux-commands"></a>Povolit čítače výkonu serveru Apache HTTP Server pomocí příkazů Linux
Pokud serveru Apache HTTP Server je v počítači nalezen, když je nainstalovaná sada omsagent, automaticky se nainstaluje zprostředkovatele pro serveru Apache HTTP Server monitorování výkonu. Tento zprostředkovatel spoléhá na platformě Apache "modul", který je nutné načíst do serveru Apache HTTP Server, aby měli přístup na údaje o výkonu.

Můžete načíst modul pomocí následujícího příkazu:

```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -c
```

Unload modulu Sledování Apache, spusťte následující příkaz:

```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -u
```
### <a name="to-view-performance-data-with-log-analytics"></a>Chcete-li zobrazit údaje o výkonu s analýzy protokolů
1. Na portálu služby Operations Management Suite klikněte na dlaždici hledání protokolů.
2. V panelu vyhledávání, zadejte `* (Type=Perf)` k zobrazení všech čítačů výkonu.

Protože OMS taky shromažďuje data čítače výkonu systému Windows, jste měli oboru rozbalovací výsledky vyhledávání na data specifická pro Linux. Ano v následujícím příkladu by zobrazit údaje o výkonu konkrétní na server Linux příklad s názvem Chorizo21.

```
Type=Perf Computer=chorizo*
```

![Příklad server zobrazen ve výsledcích vyhledávání](./media/log-analytics-linux-agents/oms-perfsearch01.png)

Ve výsledcích, můžete kliknout na **metriky** počítadla, která nebyla shromážděna data pro zobrazení. Data v reálném čase je zobrazen jako grafy pro každý čítač.

![Průzkumníku metrik](./media/log-analytics-linux-agents/oms-perfmetrics01.png)

## <a name="syslog"></a>Syslog
Syslog je protokol protokolování událostí, podobně jako protokol událostí systému Windows – obě fungují podobně jako při zobrazení v OMS.

### <a name="to-add-a-new-linux-syslog-facility-in-oms"></a>Chcete-li přidat nového zařízení syslog Linux v OMS
1. Na **nastavení** v části **Data** , klikněte na **Syslog** a vlevo na ikonu plus, zadejte název mechanismus syslog, který chcete přidat.
    ![Linux syslog](./media/log-analytics-linux-agents/oms-linuxsyslog01.png)
2. Pokud si nejste jisti úplný název zařízení, pište částečný název a zobrazí se seznam dostupných syslog zařízení. Pokud zjistíte, mechanismus syslog, který chcete přidat, klikněte na název v seznamu a pak klikněte na ikonu plus přidat protokolovací mechanismus syslog.
3. Po přidání zařízení, zobrazí se v seznamu se zvýrazněnou s barevnou panelu. V dalším kroku vyberte závažnosti (kategorie syslog zařízení informací), které chcete shromažďovat.
4. V dolní části stránky klikněte na tlačítko **Uložit** na provedené změny se dokončí. Změny konfigurace, které jste udělali jsou poté odeslány všechny agenty OMS pro Linux, které jsou registrovány OMS, obvykle během pěti minut.

### <a name="configure-linux-syslog-facilities-in-linux"></a>Nakonfigurujte zařízení syslog Linux v systému Linux
Události procesu Syslog jsou odesílány z démon procesu syslog, například rsyslog nebo syslog ng, místní port, který agent naslouchá na. Ve výchozím nastavení je to port 25224. Pokud je agent nainstalován, je použita výchozí konfigurace syslog. To se nachází zde:

Rsyslog: /etc/rsyslog.d/rsyslog-oms.conf

Syslog ng: /etc/syslog-ng/syslog-ng.conf

Výchozí konfigurace syslog agenta OMS odesílá události procesu syslog od všech zařízení, se závažností upozornění nebo vyšší.

> [!NOTE]
> Pokud chcete upravit konfiguraci syslog, je nutné restartovat démon procesu syslog změny se projeví.
>
>

Výchozí syslog konfigurace pro agenta OMS pro Linux pro OMS je:

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

### <a name="to-view-all-syslog-events-with-log-analytics"></a>Chcete-li zobrazit všechny události procesu Syslog s analýzy protokolů
1. Na portálu služby Operations Management Suite, klikněte **hledání protokolů** dlaždici.
2. V **Správa protokolu** seskupování, vyberte předdefinované syslog vyhledávání a pak vyberte jedno ji spustit.

Tento příklad ukazuje všechny události procesu Syslog.

![Ukazuje vyhledávání protokolu události procesu Syslog](./media/log-analytics-linux-agents/oms-linux-syslog.png)

Nyní můžete rozbalit výsledky hledání.

## <a name="linux-alerts"></a>Linux výstrahy
Pokud používáte Nagios nebo Zabbix ke správě vašeho počítače se systémem Linux, může přijímat OMS výstrahy generované z těchto nástrojů. Však není aktuálně žádná metoda konfigurace příchozích dat výstrah pomocí portálu OMS. Místo toho bude muset upravit konfigurační soubor zahájit odesílání výstrahy k OMS.

### <a name="collect-alerts-from-nagios"></a>Shromažďovat výstrahy z Nagios
Shromažďovat výstrahy ze serveru Nagios, budete muset provést následující změny konfigurace.

1. Udělit **omsagent** přístup pro čtení k souboru protokolu Nagios (tj. /var/log/nagios/nagios.log). Za předpokladu, že soubor nagios.log je vlastníkem skupiny **nagios** , můžete přidat uživatele **omsagent** k **nagios** skupiny.

    ```
    sudo usermod –a -G nagios omsagent
    ```
2. Upravte soubor omsagent.confconfiguration (/ etc/opt/microsoft/omsagent/&lt;id pracovního prostoru&gt;/conf/omsagent.conf). Ujistěte se, že následující položky jsou existuje a není komentáři se:

    ```
    <source>
    type tail
    #Update path to point to your nagios.log
    path /var/log/nagios/nagios.log
    format none
    tag oms.nagios
    </source>

    <filter oms.nagios>
    type filter_nagios_log
    </filter>
    ```
3. Restartujte démon omsagent:

    ```
    sudo /opt/microsoft/omsagent/bin/service_control restart
    ```

### <a name="collect-alerts-from-zabbix"></a>Shromažďovat výstrahy z Zabbix
Ke shromažďování výstrah ze serveru Zabbix, budete provádět podobným způsobem jako pro Nagios vyšší, s výjimkou toho, budete muset zadat uživatele a heslo v *text vymažte, pokud*. Toto není ideální, ale pravděpodobně bude brzy změnit. Chcete-li vyřešit tento problém, doporučujeme vytvořit uživatele a udělit mu oprávnění k monitorování pouze.

Oddílu příklad konfiguračního souboru omsagent.conf (/ etc/opt/microsoft/omsagent/&lt;id pracovního prostoru&gt;/conf/omsagent.conf) pro Zabbix by měl vypadat takto:

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
Po dokončení konfigurace počítače Linux k odesílání upozornění na OMS, můžete zobrazit výstrahy několik jednoduchých protokolu vyhledávací dotazy. Následující příklad dotazu vyhledávání vrací všechny zaznamenané výstrahy, které byly vytvořeny. Například v případě nějaká problém v infrastruktuře IT pak výsledky pro následující ukázkový dotaz může určují, kde může být problém pocházejí. A jste můžete snadno přejít k podrobnostem na výstrahy ve zdrojovém systému pomohou úzké šetření. Výhodou je, že nemusíte nutně přejít na různé systémy správy od začátku – za předpokladu, že vaše výstrahy budou zasílány OMS, můžete spustit existuje.

```
Type=Alert
```

#### <a name="to-view-all-nagios-alerts-with-log-analytics"></a>Chcete-li zobrazit všechny výstrahy Nagios s analýzy protokolů
1. Na portálu služby Operations Management Suite, klikněte **hledání protokolů** dlaždici.
2. Na panelu dotazů zadejte následující vyhledávací dotaz

    ```
    Type=Alert SourceSystem=Nagios
    ```
   ![Nagios výstrah zobrazených ve vyhledávání protokolu](./media/log-analytics-linux-agents/oms-linux-nagios-alerts.png)

Až se zobrazí výsledky hledání, můžete přejít do další podrobnosti, jako *AlertState*.

### <a name="to-view-all-zabbix-alerts-with-log-analytics"></a>Chcete-li zobrazit všechny výstrahy Zabbix s analýzy protokolů
1. Na portálu služby Operations Management Suite, klikněte **hledání protokolů** dlaždici.
2. Na panelu dotazů zadejte následující vyhledávací dotaz

    ```
    Type=Alert SourceSystem=Zabbix
    ```
   ![Zabbix výstrah zobrazených ve vyhledávání protokolu](./media/log-analytics-linux-agents/oms-linux-zabbix-alerts.png)

Až se zobrazí výsledky hledání, můžete přejít do další podrobnosti, jako *AlertName*.

## <a name="compatibility-with-system-center-operations-manager"></a>Kompatibilita s nástrojem System Center Operations Manager
OMS agenta pro Linux sdílí binárních souborů agenta s agenta System Center Operations Manager. Instalace agenta OMS pro Linux v systému aktuálně spravován nástrojem Operations Manager upgraduje OMI a SCX balíčky v počítači na novější verzi. Agent OMS pro systémy Linux a System Center 2012 R2 jsou kompatibilní. Ale **System Center 2012 SP1 a starší verze nejsou aktuálně kompatibilní nebo není podporován s agentem OMS pro Linux.**

> [!NOTE]
> Pokud je Agent OMS pro Linux nainstalován do počítače, který není aktuálně spravována nástrojem Operations Manager, a budete později chtít spravovat počítač s nástrojem Operations Manager, je třeba změnit konfiguraci OMI před zjištění počítače. **Tento krok není nutný, pokud je nainstalován agent nástroje Operations Manager před OMS agenta pro Linux.**
>
>

### <a name="to-enable-the-oms-agent-for-linux-to-communicate-with-operations-manager"></a>Chcete-li povolit agenta OMS pro Linux ke komunikaci s nástrojem Operations Manager
1. Upravit soubor /etc/opt/omi/conf/omiserver.conf
2. Ujistěte se, že na začátek řádku s **httpsport =** definuje port 1270. Například`httpsport=1270`
3. Restartujte OMI server:

    ```
    sudo /opt/omi/bin/service_control restart
    ```

## <a name="database-permissions-required-for-mysql-performance-counters"></a>Databáze oprávnění vyžadovaná pro čítače výkonu databáze MySQL
Pokud chcete udělit oprávnění pro uživatele monitorování MySQL, poskytující uživatel musí mít oprávnění k 'GRANT option, a také udělení oprávnění.

Aby mohl uživatel MySQL vrátit data výkonu uživatele bude potřebovat přístup k následující dotazy:

```
SHOW GLOBAL STATUS;
SHOW GLOBAL VARIABLES:
```

Kromě těchto dotazů MySQL uživatel vyžaduje vyberte přístup k následující výchozí tabulky:

* INFORMATION_SCHEMA
* MySQL

Tato oprávnění lze udělit spuštěním následujících příkazů grant.

```
GRANT SELECT ON information_schema.* TO ‘monuser’@’localhost’;
GRANT SELECT ON mysql.* TO ‘monuser’@’localhost’;
```

## <a name="manage-mysql-monitoring-credentials-in-the-authentication-file"></a>Spravovat MySQL monitorování přihlašovací údaje v souboru ověřování
V následujících částech vám pomohou spravovat MySQL přihlašovací údaje.

### <a name="configure-the-mysql-omi-provider"></a>Konfigurace zprostředkovatele MySQL OMI
Zprostředkovatel MySQL OMI vyžaduje předkonfigurované uživatele MySQL a nainstalovat MySQL klientské knihovny chcete-li se dotazovat informací stavu nebo výkonu z instance databáze MySQL.

### <a name="mysql-omi-authentication-file"></a>Soubor pro ověření MySQL OMI
Určit, jaké vazby adresy a portu MySQL instance naslouchá na a jaké přihlašovací údaje se má použít ke shromažďování metrik používá zprostředkovatel MySQL OMI soubor ověřování. Během instalace MySQL OMI zprostředkovatele vyhledá MySQL my.cnf konfigurační soubory (výchozí umístění) pro vazbu adresy a portu a částečně nastavit MySQL OMI soubor pro ověření.

K dokončení monitorování instance serveru MySQL, přidá soubor předem vygenerovaná ověřování MySQL OMI ve správném adresáři.

### <a name="authentication-file-format"></a>Formát souboru ověřování
MySQL OMI ověřování soubor je textový soubor, který obsahuje informace o:

* Port
* Vazby – adresy
* Uživatelské jméno MySQL
* Heslo s kódováním base64

Soubor databáze MySQL OMI ověřování jenom uděluje oprávnění pro čtení a zápis Linux uživateli, který ji vygenerovala.

```
[Port]=[Bind-Address], [username], [Base64 encoded Password]
(Port)=(Bind-Address), (username), (Base64 encoded Password)
(Port)=(Bind-Address), (username), (Base64 encoded Password)
AutoUpdate=[true|false]
```

Výchozí soubor pro ověření MySQL OMI obsahuje výchozí instance a číslo portu v závislosti na tom, jaké informace je k dispozici a Analyzovaná z konfiguračního souboru nalezen MySQL.

Výchozí instance je prostředkem, který chcete-li více instancí databáze MySQL na jednom hostiteli systému Linux jednodušší správu a je označený jako instance s portu 0. Všechny přidané instance zdědí vlastnosti z instance výchozí nastavení. Například pokud je přidána instance databáze MySQL naslouchání na portu '3308', vazby adresu výchozí instance, uživatelské jméno a heslo kódováním Base64 použije sledovat instance naslouchá na 3308 a zkuste to. Pokud instanci na 3308 je vázaný na jinou adresu a používá stejný MySQL dvojici uživatelské jméno a heslo je potřeba jenom respecification adresy vazby a zdědí vlastnosti.

Příklady souboru ověřování vypadat takto.

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
| Port |Port představuje aktuální port, který naslouchá instance databáze MySQL na.  Port 0 znamená, že následující vlastnosti se používají pro výchozí instanci. |
| Vazby – adresy |Adresa vazby je aktuální vazby MySQL-adresa |
| uživatelské jméno |Toto uživatelské jméno uživatele databáze MySQL, které chcete použít k monitorování instance serveru MySQL. |
| Kódováním base64, pomocí hesla |Jedná se o heslo uživatele monitorování MySQL kódovaný jako Base64. |
| Pomocí funkce Automatické aktualizace |Když upgraduje se zprostředkovatel OMI MySQL bude zprostředkovatele Prohledat znovu na změny v souboru my.cnf a přepsat soubor MySQL OMI ověřování. Tento příznak nastavte na hodnotu PRAVDA nebo NEPRAVDA v závislosti na požadované aktualizace do souboru MySQL OMI ověřování. |

#### <a name="authentication-file-location"></a>Umístění souboru ověřování
Soubor databáze MySQL OMI ověřování by měl nachází v následujícím umístění a s názvem "mysql ověření":

/var/OPT/Microsoft/MySQL-cimprov/auth/omsagent/MySQL-auth

Soubor (a ověřování nebo omsagent directory) by měl být vlastněných uživatelem omsagent.

## <a name="agent-logs"></a>Protokoly agenta
Protokoly pro OMS agenta pro Linux je na:

/ var/opt/microsoft/omsagent/&lt;id pracovního prostoru&gt;/log/

Protokoly pro OMS agenta pro Linux programu omsconfig (Konfigurace agenta) je na:

/ var/opt/microsoft/omsconfig nebo protokolu nebo

Protokoly pro součásti infrastruktury OMI a SCX (které poskytují data metriky výkonu) je na:

/ var/opt/omi/log/a /var/opt/microsoft/scx/log

## <a name="troubleshooting-the-oms-agent-for-linux"></a>Řešení potíží s agentem OMS pro Linux
Použijte následující informace pro diagnostiku a řešení běžných problémů.

Pokud žádné informace o odstraňování potíží v této části vám pomůže, můžete taky v následujících zdrojích informací vyřešit váš problém.

* Zákazníci s Premier support může přihlásit případu podpory prostřednictvím [úrovně Premier](https://premier.microsoft.com/)
* Zákazníci s smlouvy podporu systému Azure se můžou přihlásit případů podpory [portálu Azure](https://manage.windowsazure.com/?getsupport=true)
* Soubor [potíže Githubu](https://github.com/Microsoft/OMS-Agent-for-Linux/issues)
* Fóru pro zpětnou vazbu pro návrhy a pro vytvoření sestavy chyb [http://aka.ms/opinsightsfeedback](http://aka.ms/opinsightsfeedback)

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
> Úpravy konfigurační soubory pro čítače výkonu a syslog jsou přepsány, pokud je povoleno nastavení portálu OMS. Můžete zakázat konfigurací na portálu OMS (pro všechny uzly) nebo pro jeden uzly spuštěním následující:
>
>

```
sudo su omsagent -c /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py --disable
```


### <a name="enable-debug-logging"></a>Povolit protokolování ladění
Pokud chcete povolit protokolování ladění, můžete použít modul plug-in výstup OMS a podrobný výstup.

#### <a name="oms-output-plugin"></a>Modul plug-in výstup OMS
FluentD umožňuje modulu plug-in k určení úrovně protokolování pro úrovně jiný protokol pro vstupy a výstupy. Zadejte úroveň jiný protokol pro výstup OMS, upravte konfiguraci obecné agenta v `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` souboru.

V dolní části konfiguračního souboru, změnit `log_level` vlastnost z `info` k `debug`.

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

Protokolování ladění vám umožní zobrazit dávkové nahrávání ke službě OMS oddělených typ, počet datových položek a čas potřebný k odeslání.

*Příklad povoleno ladění protokolu:*

```
Success sending oms.nagios x 1 in 0.14s
Success sending oms.omi x 4 in 0.52s
Success sending oms.syslog.authpriv.info x 1 in 0.91s
```

#### <a name="verbose-output"></a>Podrobný výstup
Místo použití modulu plug-in výstup OMS, také výstup můžete položky dat přímo na `stdout`, který se zobrazí v OMS agenta pro Linux souboru protokolu.

V konfiguračním souboru obecné agenta OMS na `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`, okomentujte OMS výstup přidáním modulů plug-in `#` před každý řádek.

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

Níže tento modul plug-in výstup odebrat komentář v následující části odebráním `#` symbol na začátku každého řádku.

```
<match **>
  type stdout
</match>
```

### <a name="forwarded-syslog-messages-do-not-appear-in-the-log"></a>Přesměrovaná zprávy Syslog se nezobrazí v protokolu
#### <a name="probable-causes"></a>Možných příčin
* Použitá na Linux server konfigurace neumožňuje kolekce odeslaných zařízení a/nebo protokolu úrovně
* Syslog není předávaná správně na Linux server
* Počet zpráv předávaná za sekundu, je příliš velké pro základní konfiguraci agenta OMS pro Linux pro zpracování

#### <a name="resolutions"></a>Řešení
* Ověřte, že konfigurace na portálu OMS Syslog všech zařízení a úrovně správné protokolu
  * **Portál OMS > Nastavení > Data > Syslog**
* Ověřte, že nativní syslog zasílání zpráv démoni (`rsyslog`, `syslog-ng`) mohou přijímat přesměrované zprávy
* Zkontrolujte nastavení brány firewall na serveru Syslog zajistit, že zprávy nejsou blokovány.
* Simulovat zprávu Syslog pomocí OMS `logger` příkaz – například:
  * `logger -p local0.err "This is my test message"`

### <a name="problems-connecting-to-oms-when-using-a-proxy"></a>Potíže s připojením k OMS při použití serveru proxy
#### <a name="probable-causes"></a>Možných příčin
* Proxy server zadaný při instalaci a konfiguraci agenta je nesprávný
* Koncové body služby OMS nejsou whitelistested ve vašem datovém centru

#### <a name="resolutions"></a>Řešení
* Znovu nainstalujte agenta OMS pro Linux pomocí následujícího příkazu s parametrem `-v` povolena. To umožňuje podrobný výstup agenta připojení prostřednictvím proxy serveru pro službu OMS.
  * `/opt/microsoft/omsagent/bin/omsadmin.sh -w <OMS Workspace ID> -s <OMS Workspace Key> -p <Proxy Conf> -v`
  * Přečtěte si dokumentaci k proxy serveru OMS na [konfigurace agenta pro použití s HTTP proxy server](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#configuring-the-agent-for-use-with-an-http-proxy-server)
* Ověřte, zda jsou následující koncové body služby OMS seznam povolených adres

| Prostředek agenta | Porty |
| --- | --- |
| &#42;. Ods.opinsights.Azure.com |Port 443 |
| &#42;. OMS.opinsights.Azure.com |Port 443 |
| ods.systemcenteradvisor.com |Port 443 |
| &#42;.blob.core.windows.net/ |Port 443 |

### <a name="a-403-error-is-displayed-when-onboarding"></a>Zobrazí se Chyba 403 při připojování
#### <a name="probable-causes"></a>Možných příčin
* Datum a čas, jsou nesprávná na Linux Server
* ID pracovního prostoru a používá klíč pracovního prostoru jsou nesprávné

#### <a name="resolution"></a>Řešení
* Ověřte časový server Linux pomocí `date` příkaz. Pokud data jsou větší nebo menší než 15 minut od aktuálního času, registrace se nezdaří. Chcete-li vyřešit tento problém, aktualizujte datum a časové pásmo serveru Linux.
* Nejnovější verzi agenta OMS pro Linux vás upozorní, pokud je časový rozdíl je příčinou selhání registrace
* Opětovná zařadit pomocí správné ID pracovního prostoru a klíč pracovního prostoru. V tématu [registrace pomocí příkazového řádku](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) Další informace.

### <a name="a-500-error-or-404-error-appears-in-the-log-file-after-onboarding"></a>500 Chyba nebo chybu 404 se zobrazí v souboru protokolu za registrace
Jde o známý problém, který spadá první nahrávání dat Linux do pracovního prostoru OMS. To nemá vliv odesílat data nebo jiné problémy. Můžete ignorovat chyby, když se od začátku registrace.

### <a name="nagios-data-does-not-appear-in-the-oms-portal"></a>Nagios data nezobrazí na portálu OMS
#### <a name="probable-causes"></a>Možných příčin
* Omsagent uživatel nemá oprávnění ke čtení ze souboru protokolu Nagios
* Nagios zdroje a filtru oddílů jsou stále vloženy do komentáře v souboru omsagent.conf

#### <a name="resolutions"></a>Řešení
* Aby bylo možné číst ze souboru Nagios přidejte omsagent uživatele. V tématu [Nagios výstrahy](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#nagios-alerts) Další informace.
* V OMS agenta pro Linux obecné konfiguračního souboru v `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`, ujistěte se, že **i** Nagios zdroje a filtru oddílů mít komentáře odebrána, podobně jako v následujícím příkladu.

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


### <a name="linux-data-doesnt-appear-in-the-oms-portal"></a>Linux data se nezobrazí na portálu OMS
#### <a name="probable-causes"></a>Možných příčin
* Připojování ke službě OMS se nezdařilo
* Připojení ke službě OMS blokováno.
* Agent OMS pro Linux data je zálohovaná

#### <a name="resolutions"></a>Řešení
* Ověřte, že registrace ke službě OMS byla úspěšná, jestli `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf` existuje.
* Opětovná zařadit omsadmin.sh příkazového řádku. V tématu [registrace pomocí příkazového řádku](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) Další informace.
* Pokud používáte proxy server, používejte proxy server výše uvedené kroky řešení potíží
* V některých případech při OMS agenta pro Linux nemůže komunikovat se službou OMS data na agentovi je zálohovaná na velikost vyrovnávací paměti úplné 50 MB. Restartujte agenta OMS pro Linux spuštěním `/opt/microsoft/omsagent/bin/service_control restart` příkaz.
  >[AZURE.NOTE] Tento problém vyřešen v 1.1.0-28 verze agenta nebo novější.

### <a name="syslog-linux-performance-counter-configuration-is-not-applied-in-the-oms-portal"></a>Konfigurace čítače výkonu Syslog Linux není použita na portálu OMS
#### <a name="probable-causes"></a>Možných příčin
* Konfigurace agenta v OMS agenta pro Linux nebyl načíst nejnovější konfigurace z portálu OMS.
* Upravené nastavení na portálu nebyly použity.

#### <a name="resolutions"></a>Řešení
`omsconfig`je agent konfigurace v OMS agenta pro Linux, který načte změny konfigurace portálu OMS každých 5 minut. Tato konfigurace se potom použije OMS agenta pro Linux konfigurační soubory umístěné na `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`.

* V některých případech nemusí být schopni komunikovat se službou konfigurace portálu, což vede k nejnovější konfigurace nebudou použity OMS agenta pro Linux konfigurace agenta.
* Ověřte, zda `omsconfig` je agent nainstalovaný následujícím kódem:

  * `dpkg --list omsconfig` nebo `rpm -qi omsconfig`
  * Pokud nainstalovaná není, znovu nainstalujte nejnovější verzi agenta OMS pro Linux
* Ověřte, zda `omsconfig` agent může komunikovat se službou OMS

  * Spustit `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'` příkaz
    * Výše uvedeného příkazu vrátí konfiguraci tohoto agenta načte z portálu, včetně nastavení Syslog, čítače výkonu systému Linux a vlastní protokoly
    * Pokud se výše uvedeného příkazu nezdaří, spusťte `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py` příkaz. Tento příkaz zajistí omsconfig agenta pro komunikaci se službou OMS načíst nejnovější konfigurací.

### <a name="custom-linux-log-data-does-not-appear-in-the-oms-portal"></a>Vlastní data protokolu Linux nezobrazí na portálu OMS
#### <a name="probable-causes"></a>Možných příčin
* Registrace k službě OMS se nezdařilo
* **Platí následující konfigurace pro servery se systémem Linux** nebylo vybráno nastavení
* omsconfig nebyl zachyceny nejnovější vlastní protokol z portálu.
* `omsagent` Použití nemá přístup k vlastní protokol z důvodu problému oprávnění nebo `omsagent` nebyl nalezen. V takovém případě se zobrazí následující výstup:
  * `[DATETIME] [warn]: file not found. Continuing without tailing it.`
  * `[DATETIME] [error]: file not accessible by omsagent.`
* Jedná se o známý problém s časování, která byla opravena v OMS agenta pro Linux verze 1.1.0-217

#### <a name="resolutions"></a>Řešení
* Ověřte, že jste úspěšně zařazený, nemá tak, že určíte jestli `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf` soubor existuje.
  * Pokud potřebné, zařadit znovu pomocí příkazového řádku omsadmin.sh. V tématu [registrace pomocí příkazového řádku](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) Další informace.
* Na portálu OMS pod **nastavení** na **Data** ověřte, že **platí následující konfigurace pro servery se systémem Linux** je vybráno nastavení  
  ![použít konfiguraci](./media/log-analytics-linux-agents/customloglinuxenabled.png)
* Ověřte, zda `omsconfig` agent může komunikovat se službou OMS

  * Spustit `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'` příkaz
  * Výše uvedeného příkazu vrátí konfiguraci tohoto agenta načte z portálu, včetně nastavení Syslog, čítače výkonu systému Linux a vlastní protokoly
  * Pokud se výše uvedeného příkazu nezdaří, spusťte `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py` příkaz. Tento příkaz zajistí omsconfig agenta pro komunikaci se službou OMS a načíst nejnovější konfigurací.

Místo OMS agenta pro Linux uživatel, který spouští jako privilegovaných uživatelů `root`, Agent OMS pro Linux běží jako `omsagent` uživatele. Ve většině případů musí mít udělen výslovná oprávnění uživateli, aby bylo možné číst určité soubory.

Udělení oprávnění ke `omsagent` uživatele, spusťte následující příkazy:

1. Přidat `omsagent` uživatele na konkrétní skupinu s`sudo usermod -a -G <GROUPNAME> <USERNAME>`
2. Udělte universal oprávnění ke čtení pro požadovaný soubor s`sudo chmod -R ugo+rw <FILE DIRECTORY>`

Je známý problém s časování, která byla opravena v OMS agenta pro Linux verze 1.1.0-217. Po aktualizaci na nejnovější verzi agenta, spusťte následující příkaz, abyste získali nejnovější verzi modulu plug-in výstup:

```
sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.conf /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf
```

## <a name="known-limitations"></a>Známá omezení
Zkontrolujte v následujících částech Další informace o aktuální omezení OMS agenta pro Linux.

### <a name="azure-diagnostics"></a>Diagnostika Azure
Pro virtuální počítače Linux běží v Azure mohou být vyžadovány další kroky umožňuje shromažďování dat pomocí diagnostiky Azure a Operations Management Suite. **Verze 2.2** rozšíření diagnostiky pro Linux je vyžadována pro kompatibilitu s agentem OMS pro Linux.

Další informace o instalaci a konfiguraci rozšíření diagnostiky pro Linux najdete v tématu [pomocí příkazu příkazového řádku Azure CLI můžete povolit rozšíření diagnostiky Linux](../virtual-machines/linux/classic/diagnostic-extension-v2.md#use-the-azure-cli-command-to-enable-the-linux-diagnostic-extension).

**Upgrade rozšíření diagnostiky Linux 2.0 na 2.2 rozhraní příkazového řádku Azure ASM:**

```
azure vm extension set -u <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions 2.0
azure vm extension set <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions 2.2 --private-config-path PrivateConfig.json
```

**ARM**

```
azure vm extension set -u <resource-group> <vm-name> Microsoft.Insights.VMDiagnosticsSettings Microsoft.OSTCExtensions 2.0
azure vm extension set <resource-group> <vm-name> LinuxDiagnostic Microsoft.OSTCExtensions 2.2 --private-config-path PrivateConfig.json
```

Tyto příklady příkaz odkazovat na soubor s názvem PrivateConfig.json. Formát souboru by měl vypadat podobně jako následující příklad.

```
    {
    "storageAccountName":"the storage account to receive data",
    "storageAccountKey":"the key of the account"
    }
```

### <a name="sysklog-is-not-supported"></a>Sysklog není podporován.
Rsyslog nebo syslog ng jsou požadované ke shromáždění zprávy syslog. Démon procesu syslog výchozí na verze 5 Red Hat Enterprise Linux a CentOS, Oracle Linux verze (sysklog) není podporována pro shromažďování událostí syslog. Ke shromažďování dat syslog z této verze těchto distribuce, musí být nainstalovaná a nakonfigurovaná nahradit sysklog démon rsyslog. Další informace o nahrazení sysklog rsyslog najdete v tématu [nainstalovat nově vytvořený rsyslog RPM](http://wiki.rsyslog.com/index.php/Rsyslog_on_CentOS_success_story#Install_the_newly_built_rsyslog_RPM).

## <a name="next-steps"></a>Další kroky
* Článek [Přidání řešení Log Analytics z galerie řešení](log-analytics-add-solutions.md) popisuje přidání funkcí a shromažďování dat.
* Chcete-li zobrazit podrobné informace shromážděné řešeními, seznamte se s [vyhledáváním protokolů](log-analytics-log-searches.md).
* Pomocí [řídicích panelů](log-analytics-dashboards.md) můžete ukládat a zobrazovat vlastní vyhledávání.
