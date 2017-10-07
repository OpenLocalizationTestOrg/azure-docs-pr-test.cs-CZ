---
title: "zjišťování neoprávněných vniknutí aaaPerform sítě s otevřeným zdrojem nástroje a sledovací proces sítě Azure | Microsoft Docs"
description: "Tento článek popisuje, jak toouse sledovací proces sítě Azure a s otevřeným zdrojem nástroje tooperform sítě zjišťování neoprávněných vniknutí"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 0f043f08-19e1-4125-98b0-3e335ba69681
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: b5a909b827ab32ad6b2fd8e2911a944fd940249e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="perform-network-intrusion-detection-with-network-watcher-and-open-source-tools"></a>Provedení zjišťování neoprávněných vniknutí sítě s sledovací proces sítě a open source nástroje

Zachycení paketu jsou klíčovou komponentou pro implementace systémy zjišťování neoprávněných vniknutí sítě (ID) a provádění monitorování zabezpečení sítě (NSM). Existuje několik nástrojů ID s otevřeným zdrojem, které zpracování paketů zachycení a vyhledejte podpis vniknutí možné sítě a škodlivé aktivity. Pomocí zachycení paketu hello poskytuje sledovací proces sítě, můžete analyzovat vaše síť pro všechny škodlivé vniknutí nebo ohrožení zabezpečení.

Jeden takový nástroj s otevřeným zdrojem je Suricata, modul ID, která používá sady pravidel toomonitor síťový provoz a aktivuje upozornění pokaždé, když dojde k podezřelé události. Suricata nabízí modul Vícevláknová, což znamená, že ho můžete provádět analýzy zatížení sítě s vyšší rychlostí a efektivitu. Další podrobnosti o Suricata a jeho funkce navštivte jejich na https://suricata-ids.org/.

## <a name="scenario"></a>Scénář

Tento článek vysvětluje, jak tooset do vašeho prostředí tooperform sítě pomocí sledovací proces sítě, Suricata, zjišťování neoprávněných vniknutí a hello elastické zásobníku. Sledovací proces sítě poskytuje že vám hello paketu zaznamená zjišťování neoprávněných vniknutí použité tooperform sítě. Zaznamená Suricata procesy hello paketů a aktivační události výstrahy založené na pakety, které odpovídají jeho dané ruleset hrozeb. Tyto výstrahy jsou uloženy v souboru protokolu na místním počítači. Hello protokoly Suricata pomocí hello elastické zásobníku, lze indexovat a použít toocreate Kibana řídicího panelu, poskytování vizuální reprezentace hello protokoly a znamená tooquickly získat přehledy toopotential sítě ohrožení zabezpečení.  

![scénář jednoduché webové aplikace][1]

Obě open source nástroje lze nastavit na virtuální počítač Azure, umožní vám tooperform tuto analýzu v prostředí sítě Azure.

## <a name="steps"></a>Kroky

### <a name="install-suricata"></a>Nainstalujte Suricata

Všechny ostatní metody instalace najdete v článku http://suricata.readthedocs.io/en/latest/install.html

1. V hello terminál příkazového řádku virtuálního počítače spusťte následující příkazy hello:

    ```
    sudo add-apt-repository ppa:oisf/suricata-stable
    sudo apt-get update
    sudo sudo apt-get install suricata
    ```

1. tooverify instalace, spusťte příkaz hello `suricata -h` toosee hello úplný seznam příkazů.

### <a name="download-hello-emerging-threats-ruleset"></a>Stáhnout ruleset vznikající hrozby hello

V této fázi nemáme žádná pravidla pro Suricata toorun. Pokud neexistují specifické hrozby tooyour sítě chcete toodetect, nebo můžete také vyvinuté pomocí sady pravidel z mnoha různých poskytovatelé, například vznikající hrozby nebo VRT pravidla z Snort, můžete vytvořit vlastní pravidla. Tady používáme volně přístupné ruleset vznikající hrozby hello:

Stáhněte sadu pravidel hello a zkopírujte je do adresáře hello:

```
wget http://rules.emergingthreats.net/open/suricata/emerging.rules.tar.gz
tar zxf emerging.rules.tar.gz
sudo cp -r rules /etc/suricata/
```

### <a name="process-packet-captures-with-suricata"></a>Proces paketu zachytávali Suricata

paket tooprocess zaznamená pomocí Suricata, spusťte následující příkaz hello:

```
sudo suricata -c /etc/suricata/suricata.yaml -r <location_of_pcapfile>
```
toocheck hello výsledné výstrahy, přečtěte si soubor fast.log hello:
```
tail -f /var/log/suricata/fast.log
```

### <a name="set-up-hello-elastic-stack"></a>Nastavit hello elastické zásobníku

Zatímco hello protokoly, které vytváří Suricata obsahovat cenné informace o dění na naše síť, tyto soubory protokolu nejsou nejjednodušší tooread hello a pochopit. Propojením Suricata s hello elastické zásobníku jsme vytvořit řídicí panel Kibana co nám umožňují toosearch, graf, analyzovat a statistiky odvozena z našich protokolů.

#### <a name="install-elasticsearch"></a>Nainstalujte Elasticsearch

1. Hello elastické zásobníku z verze 5.0 a vyšší vyžaduje Java 8. Spusťte příkaz hello `java -version` toocheck vaší verzí. Pokud nemáte java nainstalovat, naleznete v toodocumentation [Oracle na webu](http://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html)
1. Stáhněte si balíček správné binární hello systému:

    ```
    curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.2.0.deb
    sudo dpkg -i elasticsearch-5.2.0.deb
    sudo /etc/init.d/elasticsearch start
    ```

    Ostatní metody instalace najdete na [Elasticsearch instalace](https://www.elastic.co/guide/en/beats/libbeat/5.2/elasticsearch-installation.html)

1. Ověřte, zda je spuštěna Elasticsearch příkazem hello:

    ```
    curl http://127.0.0.1:9200
    ```

    Měli byste vidět podobné toothis odpovědi:

    ```
    {
    "name" : "Angela Del Toro",
    "cluster_name" : "elasticsearch",
    "version" : {
        "number" : "5.2.0",
        "build_hash" : "8ff36d139e16f8720f2947ef62c8167a888992fe",
        "build_timestamp" : "2016-01-27T13:32:39Z",
        "build_snapshot" : false,
        "lucene_version" : "6.1.0"
    },
    "tagline" : "You Know, for Search"
    }
    ```

Další pokyny pro instalaci elastické vyhledávání, najdete v části stránky toohello [instalace](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/_installation.html)

### <a name="install-logstash"></a>Nainstalujte Logstash

1. tooinstall Logstash spusťte hello následující příkazy:

    ```
    curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.deb
    sudo dpkg -i logstash-5.2.0.deb
    ```
1. Další potřebujeme tooconfigure Logstash tooread z výstupu hello eve.json souboru. Vytvoření souboru logstash.conf pomocí:

    ```
    sudo touch /etc/logstash/conf.d/logstash.conf
    ```

1. Přidejte následující soubor obsahu toohello hello (ujistěte se, že hello cesta toohello eve.json soubor je správný):

    ```ruby
    input {
    file {
        path => ["/var/log/suricata/eve.json"]
        codec =>  "json"
        type => "SuricataIDPS"
    }

    }

    filter {
    if [type] == "SuricataIDPS" {
        date {
        match => [ "timestamp", "ISO8601" ]
        }
        ruby {
        code => "
            if event.get('[event_type]') == 'fileinfo'
            event.set('[fileinfo][type]', event.get('[fileinfo][magic]').to_s.split(',')[0])
            end
        "
        }

        ruby{
        code => "
            if event.get('[event_type]') == 'alert'
            sp = event.get('[alert][signature]').to_s.split(' group ')
            if (sp.length == 2) and /\A\d+\z/.match(sp[1])
                event.set('[alert][signature]', sp[0])
            end
            end
            "
        }
    }

    if [src_ip]  {
        geoip {
        source => "src_ip"
        target => "geoip"
        #database => "/opt/logstash/vendor/geoip/GeoLiteCity.dat"
        add_field => [ "[geoip][coordinates]", "%{[geoip][longitude]}" ]
        add_field => [ "[geoip][coordinates]", "%{[geoip][latitude]}"  ]
        }
        mutate {
        convert => [ "[geoip][coordinates]", "float" ]
        }
        if ![geoip.ip] {
        if [dest_ip]  {
            geoip {
            source => "dest_ip"
            target => "geoip"
            #database => "/opt/logstash/vendor/geoip/GeoLiteCity.dat"
            add_field => [ "[geoip][coordinates]", "%{[geoip][longitude]}" ]
            add_field => [ "[geoip][coordinates]", "%{[geoip][latitude]}"  ]
            }
            mutate {
            convert => [ "[geoip][coordinates]", "float" ]
            }
        }
        }
    }
    }

    output {
    elasticsearch {
        hosts => "localhost"
    }
    }
    ```

1. Zajistěte, aby toogive hello správná oprávnění toohello eve.json soubor tak, aby Logstash dokáže přijímat hello souboru.
    
    ```
    sudo chmod 775 /var/log/suricata/eve.json
    ```

1. toostart Logstash spusťte příkaz hello:

    ```
    sudo /etc/init.d/logstash start
    ```

Další pokyny k instalaci Logstash, najdete v části toohello [oficiální dokumentaci](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)

### <a name="install-kibana"></a>Nainstalujte Kibana

1. Spusťte následující příkazy tooinstall Kibana hello:

    ```
    curl -L -O https://artifacts.elastic.co/downloads/kibana/kibana-5.2.0-linux-x86_64.tar.gz
    tar xzvf kibana-5.2.0-linux-x86_64.tar.gz

    ```
1. toorun Kibana použijte příkazy hello:

    ```
    cd kibana-5.2.0-linux-x86_64/
    ./bin/kibana
    ```

1. tooview Kibana webového rozhraní, přejděte příliš`http://localhost:5601`
1. V tomto scénáři hello index způsobem používaným pro hello Suricata protokolů je "logstash-*"

1. Pokud chcete vzdáleně tooview hello Kibana řídicí panel, vytvoření příchozího pravidla NSG povolením přístupu příliš**portu 5601**.

### <a name="create-a-kibana-dashboard"></a>Vytvořit řídicí panel Kibana

V tomto článku uvádíme ukázkový řídicí panel vám tooview trendy a podrobnosti v upozornění.

1. Stažení souboru řídicí panel hello [sem](https://aka.ms/networkwatchersuricatadashboard), hello vizualizace souboru [sem](https://aka.ms/networkwatchersuricatavisualization)a soubor hello uložené hledání [zde](https://aka.ms/networkwatchersuricatasavedsearch).

1. V části hello **správy** kartě z Kibana, přejděte příliš**uložit objekty** a importovat všechny tři soubory. Potom z hello **řídicí panel** karta můžete otevřít a zatížení hello ukázkový řídicí panel.

Můžete také vytvořit vlastní vizualizace a přizpůsobit směrem metriky týkající se vlastní řídicí panely. Další informace o vytváření vizualizací Kibana z na Kibana [oficiální dokumentaci](https://www.elastic.co/guide/en/kibana/current/visualize.html).

![řídicí panel kibana][2]

### <a name="visualize-ids-alert-logs"></a>Vizualizace ID výstrahy protokoly

Hello ukázkový řídicí panel poskytuje několik vizualizace hello Suricata výstrahy protokolů:

1. Výstrahy podle GeoIP – mapu zobrazující hello distribuce výstrahy podle země původu podle zeměpisného umístění (určené IP)

    ![geograficky ip][3]

1. TOP 10 výstrahy – souhrn hello 10 nejčastěji se vyskytující aktivaci výstrah a jejich popis. Kliknutím na jednotlivé výstrahy filtruje dolů hello řídicí panel toohello informace týkající se toothat konkrétní výstrahu.

    ![Obrázek 4][4]

1. Počet výstrah – celkový počet výstrahy aktivovány hello ruleset hello

    ![Obrázek 5][5]

1. Prvních 20 počítačů zdrojové nebo cílové IP adresy nebo porty - výsečové grafy znázorňující hello nejvyšší 20 IP adresy a porty, které výstrahy byla aktivována. Můžete filtrovat dolů na konkrétní IP adresy nebo porty toosee kolik a jaký druh výstrahy, se aktivují.

    ![Obrázek 6][6]

1. Souhrn výstrah – tabulky Shrnutí konkrétní podrobnosti o každém individuální výstrahu. Tato tabulka tooshow můžete přizpůsobit další parametry, které vás zajímají pro každou výstrahu.

    ![Obrázek 7][7]

Další dokumentaci týkající se vytvoření vlastního vizualizace a řídicích panelů najdete v tématu [oficiální dokumentaci na Kibana](https://www.elastic.co/guide/en/kibana/current/introduction.html).

## <a name="conclusion"></a>Závěr

Kombinací paketu zaznamená zadaný sledovací proces sítě a ID nástroje s otevřeným zdrojem, jako je Suricata, je možné provést zjišťování neoprávněných vniknutí sítě pro širokou škálu hrozeb. Tyto řídicí panely povolit, můžete tooquickly trendy a anomálie v rámci vaší sítě i proniknout do hello data toodiscover hlavní příčiny výstrahy, jako je například uživatel se zlými úmysly agentů nebo citlivé porty. Tato extrahovaná data můžete informovaně rozhodnout o jak tooreact tooand chránit vaši síť před všechny pokusy o škodlivé vniknutí a vytvoření pravidla tooprevent budoucí vniknutí tooyour sítě.

## <a name="next-steps"></a>Další kroky

Zjistěte, jak zaznamená tootrigger paketů na základě výstrahy navštivte stránky [pomocí paketu zachycení toodo Proaktivní monitorování sítě s Azure Functions](network-watcher-alert-triggered-packet-capture.md)

Zjistěte, jak toovisualize vaše skupina NSG toku protokoly s Power BI navštivte stránky [vizualizovat NSG toků protokoly s Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)



<!-- images -->
[1]: ./media/network-watcher-intrusion-detection-open-source-tools/figure1.png
[2]: ./media/network-watcher-intrusion-detection-open-source-tools/figure2.png
[3]: ./media/network-watcher-intrusion-detection-open-source-tools/figure3.png
[4]: ./media/network-watcher-intrusion-detection-open-source-tools/figure4.png
[5]: ./media/network-watcher-intrusion-detection-open-source-tools/figure5.png
[6]: ./media/network-watcher-intrusion-detection-open-source-tools/figure6.png
[7]: ./media/network-watcher-intrusion-detection-open-source-tools/figure7.png
