---
title: "aaaVisualize NSG sledovací proces sítě Azure toku protokolů pomocí nástroje s otevřeným zdrojem | Microsoft Docs"
description: "Tato stránka popisuje, jak otevřít toouse zdroje nástroje toovisualize NSG toku protokoly."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: e9b2dcad-4da4-4d6b-aee2-6d0afade0cb8
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 47cb529d4a1e00e8c4c0fa6885cbf72aed3e74c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-azure-network-watcher-nsg-flow-logs-using-open-source-tools"></a>Vizualizace protokolů toku NSG sledovací proces sítě Azure pomocí nástroje s otevřeným zdrojem

Skupina zabezpečení sítě toku protokoly poskytují informace, které můžete použít pochopit příchozí a odchozí přenosy IP na skupiny zabezpečení sítě. Tyto protokoly toku zobrazit odchozí a příchozí tok na základě za pravidlo hello seskupování hello toku platí pro, 5 řazené kolekce členů informace o toku hello (zdrojové nebo cílové IP adresy, zdrojový nebo cílový Port, protokol), a pokud bylo povolené nebo zakázané hello přenosy.

Tyto protokoly toku může být obtížné toomanually analýzy a získat přehledy z. Existuje však několik nástrojů s otevřeným zdrojem, které vám mohou pomoci vizualizovat tato data. Tento článek vám poskytne tyto protokoly hello elastické zásobníku, pomocí kterých bude umožňují tooquickly index a vizualizovat svoje protokoly toku na řídicím panelu Kibana toovisualize řešení.

## <a name="scenario"></a>Scénář

V tomto článku nastavíme řešení, které vám umožní toovisualize skupinu zabezpečení sítě toku protokolů pomocí hello elastické zásobníku.  O Logstash vstupní modul plug-in obdrží hello toku protokoly přímo z objektu blob úložiště hello nakonfigurovaný pro obsahující protokoly toku hello. Potom pomocí hello elastické zásobníku, protokoly toku hello bude indexované a použít toocreate informací o Kibana toovisualize řídicí panel hello.

![scénář][scenario]

## <a name="steps"></a>Kroky

### <a name="enable-network-security-group-flow-logging"></a>Protokolování toku povolit skupinu zabezpečení sítě
V tomto scénáři musí mít síťové zabezpečení skupiny toku protokolování zapnuta alespoň jednu skupinu zabezpečení sítě ve vašem účtu. Pokyny k povolení protokolů toku zabezpečení sítě, najdete v části toohello následujícího článku [Úvod tooflow protokolování pro skupiny zabezpečení sítě](network-watcher-nsg-flow-logging-overview.md).


### <a name="set-up-hello-elastic-stack"></a>Nastavit hello elastické zásobníku
Propojením NSG toku protokoly s hello elastické zásobníku, můžeme vytvořit řídicí panel Kibana co nám umožňují toosearch, graf, analyzovat a odvozena statistiky z našich protokolů.

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
1. V dalším potřebovat tooconfigure Logstash tooaccess a analyzovat protokoly toku hello. Vytvoření souboru logstash.conf pomocí:

    ```
    sudo touch /etc/logstash/conf.d/logstash.conf
    ```

1. Přidejte následující soubor obsahu toohello hello:

  ```
    input {
      azureblob
        {
            storage_account_name => "mystorageaccount"
            storage_access_key => "storageaccesskey"
            container => "nsgflowlogContainerName"
            codec => "json"
        }
      }

      filter {
        split { field => "[records]" }
        split { field => "[records][properties][flows]"}
        split { field => "[records][properties][flows][flows]"}
        split { field => "[records][properties][flows][flows][flowTuples]"}

     mutate{
      split => { "[records][resourceId]" => "/"}
      add_field => {"Subscription" => "%{[records][resourceId][2]}"
                    "ResourceGroup" => "%{[records][resourceId][4]}"
                    "NetworkSecurityGroup" => "%{[records][resourceId][8]}"}
      convert => {"Subscription" => "string"}
      convert => {"ResourceGroup" => "string"}
      convert => {"NetworkSecurityGroup" => "string"}
      split => { "[records][properties][flows][flows][flowTuples]" => ","}
      add_field => {
                  "unixtimestamp" => "%{[records][properties][flows][flows][flowTuples][0]}"
                  "srcIp" => "%{[records][properties][flows][flows][flowTuples][1]}"
                  "destIp" => "%{[records][properties][flows][flows][flowTuples][2]}"
                  "srcPort" => "%{[records][properties][flows][flows][flowTuples][3]}"
                  "destPort" => "%{[records][properties][flows][flows][flowTuples][4]}"
                  "protocol" => "%{[records][properties][flows][flows][flowTuples][5]}"
                  "trafficflow" => "%{[records][properties][flows][flows][flowTuples][6]}"
                  "traffic" => "%{[records][properties][flows][flows][flowTuples][7]}"
                   }
      convert => {"unixtimestamp" => "integer"}
      convert => {"srcPort" => "integer"}
      convert => {"destPort" => "integer"}        
     }

     date{
       match => ["unixtimestamp" , "UNIX"]
     }
    }

    output {
      stdout { codec => rubydebug }
      elasticsearch {
        hosts => "localhost"
        index => "nsg-flow-logs"
      }
    }  

  ```

Další pokyny k instalaci Logstash, najdete v části toohello [oficiální dokumentaci](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)

### <a name="install-hello-logstash-input-plugin-for-azure-blob-storage"></a>Nainstalujte hello Logstash vstupní modul plug-in pro úložiště objektů blob v Azure

Tento modul plug-in Logstash vám umožní toodirectly přístup hello toku protokoly ze svého účtu úložiště určený. tooinstall tento modul plug-in z hello výchozí Logstash instalační adresář (v této případu /usr/share/logstash/bin) spusťte příkaz hello:

```
logstash-plugin install logstash-input-azureblob
```

toostart Logstash spusťte příkaz hello:

```
sudo /etc/init.d/logstash start
```

Další informace o tento modul plug-in, najdete v části toodocumentation [sem](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob)

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
1. Pro tento scénář je hello index způsobem používaným pro protokoly toku hello "protokolů nsg toku". Můžete změnit vzor hello indexu v části "výstupní" hello logstash.conf souboru.

1. Pokud chcete vzdáleně tooview hello Kibana řídicí panel, vytvoření příchozího pravidla NSG povolením přístupu příliš**portu 5601**.

### <a name="create-a-kibana-dashboard"></a>Vytvořit řídicí panel Kibana

V tomto článku uvádíme ukázkový řídicí panel vám tooview trendy a podrobnosti v upozornění.

![Obrázek 1][1]

1. Stažení souboru řídicí panel hello [sem](https://aka.ms/networkwatchernsgflowlogdashboard), hello vizualizace souboru [sem](https://aka.ms/networkwatchernsgflowlogvisualizations)a soubor hello uložené hledání [zde](https://aka.ms/networkwatchernsgflowlogsearch).

1. V části hello **správy** kartě z Kibana, přejděte příliš**uložit objekty** a importovat všechny tři soubory. Potom z hello **řídicí panel** karta můžete otevřít a zatížení hello ukázkový řídicí panel.

Můžete také vytvořit vlastní vizualizace a přizpůsobit směrem metriky týkající se vlastní řídicí panely. Další informace o vytváření vizualizací Kibana z na Kibana [oficiální dokumentaci](https://www.elastic.co/guide/en/kibana/current/visualize.html).

### <a name="visualize-nsg-flow-logs"></a>Vizualizace protokolů NSG toku

Hello ukázkový řídicí panel poskytuje několik vizualizace hello toku protokolů:

1. Toky podle rozhodnutí/směr v čase - čas řady grafy znázorňující počet hello toky přes hello časové období. Můžete upravit hello jednotka času a rozpětí obě tyto vizualizace. Toky podle poměr hello ukazuje rozhodnutí povolit nebo odepřít rozhodnutí, při toky ve směru ukazuje hello poměr příchozí a odchozí přenosy. S tyto vizuální prvky můžete prozkoumat provoz trendů v čase a vyhledejte všechny špičky nebo neobvyklou vzory.

  ![Obrázek 2][2]

1. Toky podle cílové/zdrojový Port – výsečové grafy znázorňující hello rozpis toků tootheir příslušné porty. K tomuto zobrazení se zobrazí vaše nejčastěji používané porty. Pokud kliknete na určitém portu v rámci hello výsečového grafu, bude filtrovat hello zbytek řídicí panel hello dolů tooflows tento port.

  ![figure3][3]

1. Počet toky a Nejdřívější čas protokolu – metriky ukazuje zaznamenána hello počet toky a zachytit hello datum hello nejdřívější protokolu.

  ![figure4][4]

1. Toky NSG a pravidlo – pruhový graf ukazuje hello distribuční toků v jednotlivých skupinách NSG, jakož i hello distribuce pravidel v jednotlivých skupinách NSG. Zde vidíte které skupina NSG a pravidla generované hello většina provozu.

  ![figure5][5]

1. Prvních 10 zdrojové nebo cílové IP adresy – pruhové grafy znázorňující hello prvních 10 zdrojové a cílové IP adresy. Tyto grafy tooshow můžete upravit více nebo méně nejvyšší IP adresy. Odsud můžete může najdete v části hello nejčastěji výskytu IP adresy, stejně jako hello rozhodnutí provoz (povolit nebo zakázat) prováděné směrem každý IP.

  ![figure6][6]

1. Tok řazené kolekce členů – Tato tabulka ukazuje hello informace obsažené v rámci každé toku řazené kolekce členů a také jeho odpovídající hodnot a pravidla.

  ![figure7][7]

Použití hello dotazu pruhu v horní části hello hello řídicího panelu, můžete filtrovat dolů hello řídicí panel podle libovolný parametr hello toky, jako je například ID předplatného, skupiny prostředků, pravidla nebo jakoukoli jinou proměnnou, které vás zajímají. Další informace o Kibana na dotazy a filtry, najdete v části toohello [oficiální dokumentaci](https://www.elastic.co/guide/en/beats/packetbeat/current/kibana-queries-filters.html)

## <a name="conclusion"></a>Závěr

Kombinací hello skupinu zabezpečení sítě toku protokoly s hello elastické zásobníku jsme mít spolu výkonný a přizpůsobit způsob toovisualize naše síťový provoz. Tyto řídicí panely umožňují získat tooquickly a sdílet uplatnitelné informace o vaší síti a také filtru dolů a prozkoumat na všechny potenciální anomálií. Pomocí Kibana, můžete přizpůsobit tyto řídicí panely a vytvořit konkrétní vizualizace toomeet všechny potřeby zabezpečení, auditování a dodržování předpisů.

## <a name="next-steps"></a>Další kroky

Zjistěte, jak toovisualize vaše skupina NSG toku protokoly s Power BI navštivte stránky [vizualizovat NSG toků protokoly s Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)


<!--Image references-->

[scenario]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/scenario.png
[1]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure1.png
[2]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure2.png
[3]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure3.png
[4]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure4.png
[5]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure5.png
[6]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure6.png
[7]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure7.png
