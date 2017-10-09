---
title: "aaaConnect vašeho počítače se systémem Linux tooOperations Management Suite (OMS) | Microsoft Docs"
description: "Tento článek popisuje, jak tooconnect počítače se systémem Linux hostované v Azure, jiné cloudové nebo místní tooOMS pomocí hello OMS agenta pro Linux."
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/21/2017
ms.author: magoedte
ms.openlocfilehash: cb4fc671d0678f9fadc689c6ba7d719213aa61b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-linux-computers-toooperations-management-suite-oms"></a>Připojit vaše počítače se systémem Linux tooOperations Management Suite (OMS) 

S Microsoft Operations Management Suite (OMS) můžete shromažďovat a provádění akcí s data generovaná z počítače se systémem Linux a kontejner řešení jako Docker, umístěný ve vašem datovém centru místní jako fyzických serverech nebo virtuálních počítačů, virtuálních počítačů v Služba hostovaných v cloudu jako Amazon Web Services (AWS) nebo Microsoft Azure. Můžete použít také k dispozici v OMS řešení pro správu třeba sledování změn, změny konfigurace tooidentify, a Správa aktualizací toomanage softwaru aktualizace tooproactively spravovat životní cyklus hello virtuální počítače Linux. 

Hello OMS agenta pro Linux komunikuje přes port 443 protokolu TCP odchozí s hello OMS služby, a pokud počítač hello připojí tooa brány firewall nebo proxy server toocommunicate přes hello Internetu, zkontrolujte [konfigurace hello agenta pro použití s proxy serveru HTTP Server nebo brána OMS](#configuring-the-agent-for-use-with-an-http-proxy-server-or-oms-gateway) toounderstand změny konfigurace, které bude nutné toobe použít.  Pokud sledujete hello počítači pomocí System Center 2016 - Operations Manager nebo Operations Manager 2012 R2, může být vícedomé s hello OMS služby toocollect daty a předat dál toohello služby a bude i nadále monitorovat pomocí nástroje Operations Manager.  Počítače se systémem Linux monitorovány podle skupiny pro správu nástroje Operations Manager, která je integrovaná s OMS neobdrží konfigurace zdroje dat a předávat shromážděné data prostřednictvím skupiny pro správu hello.  Hello OMS agent nemůže být nakonfigurované tooreport toomore než jednoho pracovního prostoru.  

Pokud vaše zásady zabezpečení IT neumožňují počítače ve vaší síti tooconnect toohello Internetu, můžete být nakonfigurované tooconnect toohello OMS brány tooreceive informace o konfiguraci a odeslat shromážděná data v závislosti na hello řešení, které máte hello agenta Povolit. Další informace a na tom, jak tooconfigure vaše agenta OMS Linux toocommunicate prostřednictvím brány OMS toohello OMS služby, najdete v části kroky [připojit počítače tooOMS pomocí hello OMS brány](log-analytics-oms-gateway.md).  

Hello následující diagram znázorňuje hello připojení mezi počítačů spravovaných agentem Linux hello a OMS, včetně hello směr a porty.

![agent přímé komunikaci s OMS diagram](./media/log-analytics-agent-linux/log-analytics-agent-linux-communication.png)

## <a name="system-requirements"></a>Požadavky na systém
Než začnete, zkontrolujte následující podrobnosti tooverify splňujete požadavky hello hello.

### <a name="supported-linux-operating-systems"></a>Podporované operační systémy Linux
Hello následující Linuxových distribucích jsou oficiálně podporované.  Hello OMS agenta pro Linux mohou však spustit také na dalších distribuce, které nejsou uvedené.

* Amazon Linux 2012.09 too2015.09 (x86/x64)
* CentOS Linux 5, 6 a 7 (x86/x64)
* Oracle Linux 5, 6 a 7 (x86/x64)
* Red Hat Enterprise Linux Server 5, 6 a 7 (x86/x64)
* Debian GNU/Linux 6, 7 a 8 (x86/x64)
* Ubuntu 12.04 LTS, 14.04 LTS, 15.04, 15.10, 16.04 LTS (x86/x64)
* SUSE Linux Enterprise Server 11 a 12 (x86/x64)

### <a name="network"></a>Síť
Následující seznam hello proxy Hello informace a informace o konfiguraci brány firewall vyžaduje pro hello toocommunicate agenta Linux s OMS. Přenosy jsou odchozí z vaše síťová služba toohello OMS. 

|Prostředek agenta| Porty |  
|------|---------|  
|*.ods.opinsights.azure.com | Port 443|   
|*.oms.opinsights.azure.com | Port 443|   
|*.BLOB.Core.Windows.NET/ | Port 443|   
|*.azure-automation.net | Port 443|  

### <a name="package-requirements"></a>Požadavky na balíček

 **Požadovaný balíček**   | **Popis**   | **Minimální verze**
--------------------- | --------------------- | -------------------
Glibc | Knihovny jazyka C GNU   | 2.5-12 
OpenSSL | Knihovny OpenSSL | 0.9.8E nebo 1.0
Curl | cURL webového klienta | 7.15.5
Python ctypes | | 
PAM | PAM moduly | 

> [!NOTE]
>  Rsyslog nebo syslog ng jsou požadované toocollect zprávy syslog. Démon procesu syslog výchozí Hello na verze 5 Red Hat Enterprise Linux a CentOS, Oracle Linux verze (sysklog) není podporována pro shromažďování událostí syslog. toocollect syslog data z této verze těchto distribuce hello rsyslog démon by měl být nainstalován a nakonfigurován tooreplace sysklog 

Hello agent obsahuje více balíčků. Hello verze souboru obsahuje následující balíčky, které jsou k dispozici ve spuštěné sadě hello prostředí s hello `--extract`:

**Balíček** | **Verze** | **Popis**
----------- | ----------- | --------------
omsagent | 1.4.0 | Hello Operations Management Suite agenta pro Linux
omsconfig | 1.1.1 | Konfigurace agenta pro hello agenta OMS
OMI | 1.2.0 | Open Management Infrastructure (OMI) - lightweight CIM Server
scx. | 1.6.3 | Zprostředkovatelé CIM OMI pro metriku výkonu operačního systému
Apache cimprov | 1.0.1 | Zprostředkovatel pro OMI monitorování výkonu serveru Apache HTTP Server. Nainstalovat, pokud je zjištěn serveru Apache HTTP Server.
MySQL cimprov | 1.0.1 | Výkon serveru MySQL monitorování zprostředkovatele pro OMI. Nainstalovat, pokud je zjištěn MySQL nebo MariaDB server.
docker cimprov | 1.0.0 | Zprostředkovatel docker pro OMI. Nainstalovat, pokud je zjištěn Docker.

### <a name="compatibility-with-system-center-operations-manager"></a>Kompatibilita s nástrojem System Center Operations Manager
Hello OMS agenta pro Linux sdílí binárních souborů agenta s hello agenta System Center Operations Manager. Pokud instalujete hello OMS agenta pro Linux v systému aktuálně spravován nástrojem Operations Manager, hello OMI a SCX balíčků na novější verzi tooa hello počítače. V této verzi, hello OMS a System Center 2016 - Operations Manager, Operations Manager 2012 R2 agenti pro Linux jsou kompatibilní. 

> [!NOTE]
> System Center 2012 SP1 a starší verze nejsou aktuálně kompatibilní nebo není podporován s hello OMS agenta pro Linux.<br>
> Pokud hello OMS agenta pro Linux je nainstalovaná tooa počítač, který není aktuálně sledovaných nástrojem Operations Manager, a potom chcete toomonitor hello počítač s nástrojem Operations Manager, musíte upravit hello [OMI konfigurace](#enable-the-oms-agent-for-linux-to-report-to-system-center-operations-manager) předchozí počítač toodiscovering hello. **Tento krok je *není* nutný v případě, že je nainstalován agent Operations Manager hello před hello OMS agenta pro Linux.**

### <a name="system-configuration-changes"></a>Změny konfigurace systému
Po instalaci hello OMS agenta pro Linux balíčky, hello následující další systémové změny konfigurace se použijí. Tyto artefakty se odeberou, když hello omsagent balíček odinstalován.

* Bez oprávnění uživatele s názvem: `omsagent` je vytvořena. Toto je hello účet hello omsagent démon běží jako.
* Soubor "zahrnout" sudoers je vytvořena při /etc/sudoers.d/omsagent. To autorizuje omsagent toorestart hello syslog a omsagent démoni. Pokud direktivy "zahrnout" sudo nejsou podporovány v hello nainstalované verzi sudo, tyto položky jsou zapsány příliš/etc/sudoers.
* Konfigurace syslog Hello je upravený tooforward podmnožinu události toohello agenta. Další informace najdete v tématu hello **konfigurace shromažďování dat** části

### <a name="upgrade-from-a-previous-release"></a>Upgrade z předchozí verze
Upgrade z verze dříve, než je 1.0.0-47 v této verzi podporovány. Provedení instalace hello s hello `--upgrade` příkaz upgraduje všechny součásti hello agenta toohello nejnovější verzi.

## <a name="installing-hello-agent"></a>Instalace agenta hello

Tato část popisuje, jak tooinstall hello OMS agenta pro Linux pomocí bunndle, která obsahuje Debian a RPM balíčky pro všechny komponenty agenta hello.  Ho lze nainstalovat přímo nebo extrahují tooretrieve hello jednotlivých balíčků.  

Nejprve je třeba vaše OMS ID a klíč, který můžete najít a přepínání toohello [klasický portál OMS](https://mms.microsoft.com).  Na hello **přehled** stránky z hlavní nabídky vyberte možnost hello **nastavení**a potom přejděte příliš**připojené servery Sources\Linux**.  Zobrazí hello hodnota toohello napravo od **ID pracovního prostoru** a **primární klíč**.  Zkopírujte a vložte do vašeho oblíbeného editoru.    

1. Stažení hello nejnovější [OMS agenta pro Linux (x64)](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/OMSAgent_GA_v1.4.0-45/omsagent-1.4.0-45.universal.x64.sh) nebo [OMS agenta pro Linux x86](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/OMSAgent_GA_v1.4.0-45/omsagent-1.4.0-45.universal.x86.sh) z Githubu.  
2. Přeneste hello příslušné sady (x86 nebo x64) tooyour počítač Linux pomocí spojovací bod služby/sftp.
3. Instalace sady hello pomocí hello `--install` nebo `--upgrade` argument. 

    > [!NOTE]
    > Pokud například když je již nainstalován agent hello System Center Operations Manager pro Linux jsou nainstalovány všechny existující balíčky, použijte hello `--upgrade` argument. tooconnect tooOperations Management Suite během instalace, zadejte hello `-w <WorkspaceID>` a `-s <Shared Key>` parametry.


#### <a name="tooinstall-and-onboard-directly"></a>tooinstall a připojit přímo
```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade -w <workspace id> -s <shared key>
```

#### <a name="tooupgrade-hello-agent-package"></a>balíček agenta tooupgrade hello
```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade
```

#### <a name="tooinstall-and-onboard-tooa-workspace-in-us-government-cloud"></a>tooinstall a zařadit tooa pracovního prostoru v Cloud vlády USA
```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade -w <workspace id> -s <shared key> -d opinsights.azure.us
```

## <a name="configuring-hello-agent-for-use-with-an-http-proxy-server-or-oms-gateway"></a>Konfigurace hello agenta pro použití s OMS brány nebo HTTP proxy server
Hello OMS agenta pro Linux podporuje komunikaci buď prostřednictvím protokolu HTTP nebo HTTPS proxy serveru nebo brány OMS toohello OMS služby.  Anonymní i základní ověřování (uživatelské jméno a heslo) je podporováno.  

### <a name="proxy-configuration"></a>Konfigurace proxy serveru
Hodnota konfigurace proxy Hello má hello následující syntaxi:

`[protocol://][user:password@]proxyhost[:port]`

Vlastnost|Popis
-|-
Protocol (Protokol)|protokol HTTP nebo https
Uživatel|Volitelné uživatelské jméno pro ověření proxy serverem
heslo|Volitelné heslo pro ověření proxy serverem
proxyhost|Adresa nebo plně kvalifikovaný název domény hello proxy serveru nebo OMS brány
port|Číslo portu volitelné hello proxy serveru nebo OMS brány

Příklad: `http://user01:password@proxy01.contoso.com:8080`

Hello proxy server lze zadat během instalace nebo změnou hello proxy.conf konfigurační soubor po instalaci.   

### <a name="specify-proxy-configuration-during-installation"></a>Zadejte konfiguraci proxy serveru během instalace
Hello `-p` nebo `--proxy` argument hello omsagent instalace sadě určuje toouse konfigurace proxy hello. 

```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade -p http://<proxy user>:<proxy password>@<proxy address>:<proxy port> -w <workspace id> -s <shared key>
```

### <a name="define-hello-proxy-configuration-in-a-file"></a>Zadejte konfiguraci proxy serveru hello v souboru
konfigurace proxy serveru Hello lze nastavit v souborech hello `/etc/opt/microsoft/omsagent/proxy.conf` a `/etc/opt/microsoft/omsagent/conf/proxy.conf `. Hello soubory mohou být přímo vytvořená nebo upravená, ale jejich oprávnění musí být aktualizované toogrant hello omiuser uživatel oprávnění ke čtení pro soubory hello. Například:
```
proxyconf="https://proxyuser:proxypassword@proxyserver01:8080"
sudo echo $proxyconf >>/etc/opt/microsoft/omsagent/proxy.conf
sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf
sudo chmod 600 /etc/opt/microsoft/omsagent/proxy.conf /etc/opt/microsoft/omsagent/conf/proxy.conf  
sudo /opt/microsoft/omsagent/bin/service_control restart [<workspace id>]
```

### <a name="removing-hello-proxy-configuration"></a>Odebrání konfigurace proxy hello
tooremove konfiguraci dříve definovaném proxy a vrátit toodirect připojení, odstraňte soubor proxy.conf hello:
```
sudo rm /etc/opt/microsoft/omsagent/proxy.conf /etc/opt/microsoft/omsagent/conf/proxy.conf
sudo /opt/microsoft/omsagent/bin/service_control restart 
```

## <a name="onboarding-with-operations-management-suite"></a>Registrace pomocí služby Operations Management Suite
Pokud během instalace sady hello nebyla zadána ID a klíč, musí hello agenta následně zaregistrován u služby Operations Management Suite.

### <a name="onboarding-using-hello-command-line"></a>Registrace hello příkazového řádku
Spusťte příkaz omsadmin.sh hello se zadaným id pracovního prostoru hello a klíč vašeho pracovního prostoru. Tento příkaz musí spustit jako kořenového adresáře (s zvýšení oprávnění sudo):
```
cd /opt/microsoft/omsagent/bin
sudo ./omsadmin.sh -w <WorkspaceID> -s <Shared Key>
```

### <a name="onboarding-using-a-file"></a>Registrace pomocí souboru.
1.  Vytvoření souboru hello `/etc/omsagent-onboard.conf`. Hello soubor musí být přístupné pro čtení a zápis pro kořenový adresář.
`sudo vi /etc/omsagent-onboard.conf`
2.  Vložte následující řádky v souboru hello se vaše ID a sdílený klíč hello:

        WORKSPACE_ID=<WorkspaceID>  
        SHARED_KEY=<Shared Key>  
   
3.  Spusťte následující příkaz tooOnboard tooOMS hello:`sudo /opt/microsoft/omsagent/bin/omsadmin.sh`
4.  Odstraní soubor Hello na úspěšnou registraci.

## <a name="enable-hello-oms-agent-for-linux-tooreport-toosystem-center-operations-manager"></a>Povolit hello OMS agenta pro Linux tooreport tooSystem Center Operations Manager
Proveďte následující kroky tooconfigure hello agenta OMS Linux tooreport tooa System Center Operations Manager pro skupinu pro správu hello.  

1. Upravte soubor hello`/etc/opt/omi/conf/omiserver.conf`
2. Ujistěte se, že hello řádek začínající **httpsport =** definuje hello port 1270. Například:`httpsport=1270`
3. Restartujte hello OMI server:`sudo /opt/omi/bin/service_control restart`

## <a name="agent-logs"></a>Protokoly agenta
Hello protokoly pro hello OMS agenta pro Linux najdete tady: `/var/opt/microsoft/omsagent/<workspace id>/log/` hello protokoly pro programu hello omsconfig (Konfigurace agenta) najdete na: `/var/opt/microsoft/omsconfig/log/` protokoly pro součásti pro OMI a SCX hello (které poskytují data metriky výkonu) najdete na:`/var/opt/omi/log/ and /var/opt/microsoft/scx/log`

### <a name="log-rotation-configuration"></a>Protokol otočení konfigurace ##
Hello protokolu otáčení konfigurace pro omsagent najdete tady:`/etc/logrotate.d/omsagent-<workspace id>`

Hello výchozí nastavení je: 
```
/var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log {
    rotate 5
    missingok
    notifempty
    compress
    size 50k
    copytruncate
}
```

## <a name="uninstalling-hello-oms-agent-for-linux"></a>Odinstalace hello OMS agenta pro Linux
Hello balíčky agenta odinstalovat, když spuštěné hello sady .sh soubor s hello `--purge` argument, který kompletně odebere hello agent a jeho konfigurace z počítače hello.   

```
> sudo rpm -e omsconfig
> sudo rpm -e omsagent
> sudo /opt/microsoft/scx/bin/uninstall
```

## <a name="troubleshooting"></a>Řešení potíží

### <a name="issue-unable-tooconnect-through-proxy-toooms"></a>Problém: Nelze tooconnect prostřednictvím proxy tooOMS

#### <a name="probable-causes"></a>Možných příčin
* byl nesprávný Hello proxy zadaný během registrace
* Koncové body služby OMS Hello nejsou whitelistested ve vašem datovém centru 

#### <a name="resolutions"></a>Řešení
1. Reonboard toohello OMS služby s hello OMS agenta pro Linux pomocí hello následující příkaz s možností hello `-v` povolena. To umožňuje podrobný výstup hello agenta připojení prostřednictvím hello proxy toohello služby OMS. 
`/opt/microsoft/omsagent/bin/omsadmin.sh -w <OMS Workspace ID> -s <OMS Workspace Key> -p <Proxy Conf> -v`

2. Projděte si část hello [Konfigurace hello agenta pro použití s proxy serveru HTTP server(#configuring the-agent-for-use-with-a-http-proxy-server) tooverify jste správně nakonfigurovali hello toocommunicate agenta přes proxy server.    
* Dvojitý zkontrolujte, zda text hello následující koncové body služby OMS jsou seznam povolených adres:

    |Prostředek agenta| Porty |  
    |------|---------|  
    |*.ods.opinsights.azure.com | Port 443|   
    |*.oms.opinsights.azure.com | Port 443|   
    |ods.systemcenteradvisor.com | Port 443|   
    |*.BLOB.Core.Windows.NET/ | Port 443|   

### <a name="issue-you-receive-a-403-error-when-trying-tooonboard"></a>Problém: Obdržíte 403 chybu při pokusu o tooonboard

#### <a name="probable-causes"></a>Možných příčin
* Datum a čas je nesprávný na Linux Server 
* ID pracovního prostoru a klíč pracovního prostoru používá nejsou správné

#### <a name="resolution"></a>Řešení

1. Zkontrolujte hello čas na serveru Linux s datem příkaz hello. Pokud je doba hello +/-15 minut od aktuálního času, registrace se nezdaří. toocorrect této aktualizace hello datum a časové pásmo serveru Linux. 
2. Ověřte, zda že jste nainstalovali nejnovější verzi hello hello OMS agenta pro Linux.  nejnovější verze Hello nyní vás upozorní, pokud čas zkosení způsobuje selhání registrace hello.
3. Reonboard pomocí správné ID pracovního prostoru a klíč pracovního prostoru následující pokyny k instalaci hello dříve v tomto tématu.

### <a name="issue-you-see-a-500-and-404-error-in-hello-log-file-right-after-onboarding"></a>Problém: Zobrazí 500 a 404 Chyba v souboru protokolu hello hned po registraci
Jde o známý problém, ke kterému dochází na první nahrávání dat Linux do pracovního prostoru OMS. To nemá vliv dat probíhá odeslané nebo služby.

### <a name="issue--you-are-not-seeing-any-data-in-hello-oms-portal"></a>Problém: Nevidíte všechna data na portálu OMS hello

#### <a name="probable-causes"></a>Možných příčin

- Registrace toohello OMS služby se nezdařilo
- Připojení toohello OMS služby je blokovaný.
- Vytvoření zálohy OMS agenta pro Linux data

#### <a name="resolutions"></a>Řešení
1. Zkontrolujte, pokud registrace hello OMS služby bylo úspěšné kontrolou, pokud hello následující soubor existuje:`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`
2. Reonboard pomocí hello `omsadmin.sh` příkazového řádku pokyny
3. Pokud používáte proxy server, podívejte se na toohello proxy řešení kroků uvedených výše.
4. V některých případech při hello OMS agenta pro Linux nemůže komunikovat s hello OMS služby data na hello agenta je velikost vyrovnávací paměti úplné toohello zařazených do fronty, což je 50 MB. Hello OMS agenta pro Linux by měla být restartována tak, že spustíte následující příkaz hello: `/opt/microsoft/omsagent/bin/service_control restart [<workspace id>]`. 

    >[!NOTE]
    >Tento problém vyřešen v 1.1.0-28 verze agenta nebo novější.
> 