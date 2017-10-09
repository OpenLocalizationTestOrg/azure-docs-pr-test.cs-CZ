---
title: "aaaCollect Linux výkonu aplikace v OMS Log Analytics | Microsoft Docs"
description: "Tento článek obsahuje podrobné informace pro konfiguraci hello agenta OMS pro čítače výkonu toocollect Linux MySQL a serveru Apache HTTP Server."
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
ms.date: 05/04/2017
ms.author: magoedte
ms.openlocfilehash: 51105c6add5c7941a004570a76a4d94c02fc8a71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="collect-performance-counters-for-linux-applications-in-log-analytics"></a>Shromáždit čítače výkonu pro Linux aplikace v analýzy protokolů 
Tento článek obsahuje podrobné informace pro konfiguraci hello [OMS agenta pro Linux](https://github.com/Microsoft/OMS-Agent-for-Linux) toocollect čítače výkonu pro konkrétní aplikace.  aplikace Hello zahrnuté v tomto článku jsou:  

- [MySQL](#MySQL)
- [Serveru Apache HTTP Server](#apache-http-server)

## <a name="mysql"></a>MySQL
Pokud Server databáze MySQL nebo MariaDB Server v počítači se zjistí hello při instalaci agenta OMS hello, se automaticky nainstaluje zprostředkovatele pro Server databáze MySQL monitorování výkonu. Tento zprostředkovatel připojí toohello místní databáze MySQL nebo MariaDB tooexpose Statistika výkonu serveru. Přihlašovací údaje uživatele MySQL musí nakonfigurovat tak, aby hello zprostředkovatele přístup hello MySQL serveru.

### <a name="configure-mysql-credentials"></a>Konfigurovat přihlašovací údaje databáze MySQL
Hello MySQL OMI zprostředkovatele vyžaduje předkonfigurované uživatele MySQL a nainstalovat MySQL klientské knihovny v pořadí tooquery hello výkonu a informace o stavu z instance databáze MySQL hello.  Tyto přihlašovací údaje jsou uloženy v souboru ověřování, který je uložený na hello Linux agent.  soubor pro ověření Hello určuje jaké vazby adresu a port hello MySQL instance naslouchá na a co pověření toouse toogather metriky.  

Během instalace hello OMS agenta pro Linux hello MySQL OMI zprostředkovatele vyhledá MySQL my.cnf konfigurační soubory (výchozí umístění) vazby adresy a portu a částečně sadu hello soubor pro ověření MySQL OMI.

Hello MySQL ověřování soubor je uložen v `/var/opt/microsoft/mysql-cimprov/auth/omsagent/mysql-auth`.


### <a name="authentication-file-format"></a>Formát souboru ověřování
Následuje hello formát pro hello soubor pro ověření MySQL OMI

    [Port]=[Bind-Address], [username], [Base64 encoded Password]
    (Port)=(Bind-Address), (username), (Base64 encoded Password)
    (Port)=(Bind-Address), (username), (Base64 encoded Password)
    AutoUpdate=[true|false]

Hello položky v souboru authentication hello jsou popsané v následující tabulka hello.

| Vlastnost | Popis |
|:--|:--|
| Port | Představuje hello aktuální port hello instance naslouchá na MySQL. Port 0 určuje, že jsou následující vlastnosti hello používá výchozí instanci. |
| Vazby – adresy| Adresa-aktuální MySQL vazby. |
| uživatelské jméno| Instance serveru toouse toomonitor hello MySQL použít MySQL uživatele. |
| Kódováním base64, pomocí hesla| Heslo uživatele monitorování hello MySQL kódovaný jako Base64. |
| Pomocí funkce Automatické aktualizace| Určuje, zda toorescan pro změny v souboru my.cnf hello a přepsat soubor MySQL OMI ověřování hello hello MySQL OMI zprostředkovatele upgradován. |

### <a name="default-instance"></a>Výchozí instance
Hello MySQL OMI ověřování souboru můžete definovat výchozí instance a port číslo toomake správu více instancí databáze MySQL na jednom hostiteli systému Linux jednodušší.  instance s portem 0 je označený jako Hello výchozí instanci. Všechny další instance zdědí vlastnosti nastavit z hello výchozí instanci, pokud určí různé hodnoty. Například pokud je přidána instance databáze MySQL naslouchání na portu '3308', vazby adresu hello výchozí instance, uživatelské jméno a heslo kódováním Base64 bude použité tootry a monitorovat naslouchání na 3308 instance hello. Pokud je adresa vázané tooanother hello instanci na 3308 a používá hello stejné uživatelské jméno MySQL a je potřeba heslo pár pouze hello vazby adresa a hello další vlastnosti, zdědí.

Hello následující tabulka obsahuje příklad nastavení instance 

| Popis | File |
|:--|:--|
| Výchozí instance a instanci s portem 3308. | `0=127.0.0.1, myuser, cnBwdA==`<br>`3308=, ,`<br>`AutoUpdate=true` |
| Výchozí instance a instanci s portem 3308 a jiné uživatelské jméno a heslo. | `0=127.0.0.1, myuser, cnBwdA==`<br>`3308=127.0.1.1, myuser2,cGluaGVhZA==`<br>`AutoUpdate=true` |


### <a name="mysql-omi-authentication-file-program"></a>MySQL OMI ověřování souboru programu
Je součástí instalace hello hello MySQL OMI zprostředkovatele MySQL OMI soubor programu ověřování, který lze použít tooedit hello MySQL OMI ověřování souboru. programu Hello ověřování souboru naleznete na následující umístění hello.

    /opt/microsoft/mysql-cimprov/bin/mycimprovauth

> [!NOTE]
> soubor s přihlašovacími údaji Hello musí být přečíst hello omsagent účtu. Spuštění příkazu mycimprovauth hello jako omsgent se doporučuje.

Hello následující tabulka obsahuje podrobnosti o na hello syntaxe pro používání mycimprovauth.

| Operace | Příklad | Popis
|:--|:--|:--|
| pomocí funkce Automatické aktualizace * false\|Hodnota TRUE * | false mycimprovauth automatických aktualizací | Nastaví, jestli se automaticky aktualizuje soubor pro ověření hello na restartovat nebo aktualizovat. |
| výchozí *vazby adresu uživatelské jméno, heslo* | mycimprovauth výchozí 127.0.0.1 kořenové pwd | Nastaví hello výchozí instance v hello soubor pro ověření MySQL OMI.<br>pole hesla Hello by měly být zadány ve formátu prostého textu – hello heslo v hello souboru MySQL OMI ověřování bude kódováním Base 64. |
| Odstranit * default\|port_num * | mycimprovauth 3308 | Odstraní zadanou instanci hello buď výchozí nebo číslo portu. |
| Pomoc | mycimprov nápovědy | Vytiskne seznam toouse příkazy. |
| Tisk | mycimprov tisku | Vytiskne snadno tooread soubor pro ověření MySQL OMI. |
| Aktualizovat port_num *vazby adresu uživatelské jméno, heslo* | mycimprov aktualizace 3307 127.0.0.1 kořenové pwd | Aktualizuje zadaný instanci hello nebo přidá instanci hello, pokud neexistuje. |

Hello následujících příkladech příkazů definovat výchozí uživatelského účtu pro hello MySQL server na localhost.  pole hesla Hello by měly být zadány ve formátu prostého textu – hello heslo v hello souboru MySQL OMI ověřování bude kódováním Base 64

    sudo su omsagent -c '/opt/microsoft/mysql-cimprov/bin/mycimprovauth default 127.0.0.1 <username> <password>'
    sudo /opt/omi/bin/service_control restart

### <a name="database-permissions-required-for-mysql-performance-counters"></a>Databáze oprávnění vyžadovaná pro čítače výkonu databáze MySQL
Hello MySQL uživatel vyžaduje přístup toohello následující dotazy toocollect data o výkonu serveru MySQL. 

    SHOW GLOBAL STATUS;
    SHOW GLOBAL VARIABLES:


uživatel MySQL Hello také vyžaduje následující výchozí tabulky toohello vyberte přístup.

- INFORMATION_SCHEMA
- databáze MySQL. 

Tato oprávnění lze udělit tak, že spustíte následující příkazy grant hello.

    GRANT SELECT ON information_schema.* too‘monuser’@’localhost’;
    GRANT SELECT ON mysql.* too‘monuser’@’localhost’;


> [!NOTE]
> toogrant oprávnění tooa MySQL monitorování uživatele hello udělení uživatele musí mít oprávnění "Udělit možnost" hello, jakož i udělení oprávnění hello.

### <a name="define-performance-counters"></a>Definování čítače výkonu

Jakmile nakonfigurujete hello OMS agenta pro Linux toosend data tooLog analýzy, je nutné nakonfigurovat toocollect čítače výkonu hello.  Pomocí postupu hello v [Windows a Linux zdroje dat výkonu v analýzy protokolů](log-analytics-data-sources-windows-events.md) s hello čítače v hello následující tabulka.

| Název objektu | Název čítače |
|:--|:--|
| Databáze MySQL | Místo na disku v bajtech |
| Databáze MySQL | Tabulky |
| Server databáze MySQL | Přerušené připojení Pct |
| Server databáze MySQL | Připojení pomocí protokolu Pct |
| Server databáze MySQL | Využití místa na disku v bajtech |
| Server databáze MySQL | Tabulka úplné prohledávání Pct |
| Server databáze MySQL | Fondu vyrovnávací paměti InnoDB dosáhl Pct |
| Server databáze MySQL | Vyrovnávací paměť InnoDB fondu použití protokolu Pct |
| Server databáze MySQL | Vyrovnávací paměť InnoDB fondu použití protokolu Pct |
| Server databáze MySQL | Protokol Pct požadavků klíče mezipaměti |
| Server databáze MySQL | Klíče mezipaměti použití protokolu Pct |
| Server databáze MySQL | Protokol Pct zápisu klíče mezipaměti |
| Server databáze MySQL | Pct přístupů do mezipaměti dotazu |
| Server databáze MySQL | Dotaz mezipaměti vyřadí Pct |
| Server databáze MySQL | Dotaz mezipaměti použití protokolu Pct |
| Server databáze MySQL | Protokol Pct vstupů do mezipaměti tabulky |
| Server databáze MySQL | Tabulka mezipaměti použití protokolu Pct |
| Server databáze MySQL | Tabulka zámku kolizí Pct |

## <a name="apache-http-server"></a>Serveru Apache HTTP Server 
Pokud v počítači hello se zjistí serveru Apache HTTP Server, pokud je nainstalovaná sada omsagent hello, se automaticky nainstaluje zprostředkovatele pro serveru Apache HTTP Server monitorování výkonu. Tento zprostředkovatel spoléhá na modul Apache, který je nutné načíst do hello serveru Apache HTTP Server v datech o výkonu tooaccess pořadí. modul Hello mohou být načteny s hello následující příkaz:
```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -c
```

toounload hello Apache modulu sledování, spusťte následující příkaz hello:
```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -u
```

### <a name="define-performance-counters"></a>Definování čítače výkonu

Jakmile nakonfigurujete hello OMS agenta pro Linux toosend data tooLog analýzy, je nutné nakonfigurovat toocollect čítače výkonu hello.  Pomocí postupu hello v [Windows a Linux zdroje dat výkonu v analýzy protokolů](log-analytics-data-sources-windows-events.md) s hello čítače v hello následující tabulka.

| Název objektu | Název čítače |
|:--|:--|
| Serveru Apache HTTP Server | Vytížených pracovních procesů |
| Serveru Apache HTTP Server | Nečinných pracovních procesů |
| Serveru Apache HTTP Server | Protokol PCT vytížených pracovních procesů |
| Serveru Apache HTTP Server | Celkový počet Pct procesoru |
| Apache virtuální hostitel | Chyby za minutu - klienta |
| Apache virtuální hostitel | Chyby za minutu - Server |
| Apache virtuální hostitel | KB každý požadavek |
| Apache virtuální hostitel | Žádosti o KB za sekundu |
| Apache virtuální hostitel | Počet požadavků za sekundu |



## <a name="next-steps"></a>Další kroky
* [Shromažďování čítačů výkonu](log-analytics-data-sources-performance-counters.md) od agentů systému Linux.
* Další informace o [protokolu hledání](log-analytics-log-searches.md) tooanalyze hello data budou shromažďovat ze zdroje dat a řešení. 
