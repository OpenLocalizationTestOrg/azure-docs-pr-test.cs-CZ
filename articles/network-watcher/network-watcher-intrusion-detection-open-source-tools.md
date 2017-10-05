---
title: "Provedení zjišťování neoprávněných vniknutí sítě s otevřeným zdrojem nástroje a sledovací proces sítě Azure | Microsoft Docs"
description: "Tento článek popisuje, jak použít sledovací proces sítě Azure a otevřete source nástroje k provedení zjišťování neoprávněných vniknutí sítě"
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
ms.openlocfilehash: 82d5e525859ebe03b152c63e4debbae469049c12
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="perform-network-intrusion-detection-with-network-watcher-and-open-source-tools"></a><span data-ttu-id="618c9-103">Provedení zjišťování neoprávněných vniknutí sítě s sledovací proces sítě a open source nástroje</span><span class="sxs-lookup"><span data-stu-id="618c9-103">Perform network intrusion detection with Network Watcher and open source tools</span></span>

<span data-ttu-id="618c9-104">Zachycení paketu jsou klíčovou komponentou pro implementace systémy zjišťování neoprávněných vniknutí sítě (ID) a provádění monitorování zabezpečení sítě (NSM).</span><span class="sxs-lookup"><span data-stu-id="618c9-104">Packet captures are a key component for implementing network intrusion detection systems (IDS) and performing Network Security Monitoring (NSM).</span></span> <span data-ttu-id="618c9-105">Existuje několik nástrojů ID s otevřeným zdrojem, které zpracování paketů zachycení a vyhledejte podpis vniknutí možné sítě a škodlivé aktivity.</span><span class="sxs-lookup"><span data-stu-id="618c9-105">There are several open source IDS tools that process packet captures and look for signatures of possible network intrusions and malicious activity.</span></span> <span data-ttu-id="618c9-106">Pomocí paketu zaznamená zadaný ve sledovací proces sítě, můžete analyzovat vaše síť pro všechny škodlivé vniknutí nebo ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="618c9-106">Using the packet captures provided by Network Watcher, you can analyze your network for any harmful intrusions or vulnerabilities.</span></span>

<span data-ttu-id="618c9-107">Jeden takový nástroj s otevřeným zdrojem je Suricata, modul ID, která používá sady pravidel pro monitorování síťového provozu a aktivuje upozornění pokaždé, když dojde k podezřelé události.</span><span class="sxs-lookup"><span data-stu-id="618c9-107">One such open source tool is Suricata, an IDS engine that uses rulesets to monitor network traffic and triggers alerts whenever suspicious events occur.</span></span> <span data-ttu-id="618c9-108">Suricata nabízí modul Vícevláknová, což znamená, že ho můžete provádět analýzy zatížení sítě s vyšší rychlostí a efektivitu.</span><span class="sxs-lookup"><span data-stu-id="618c9-108">Suricata offers a multi-threaded engine, meaning it can perform network traffic analysis with increased speed and efficiency.</span></span> <span data-ttu-id="618c9-109">Další podrobnosti o Suricata a jeho funkce navštivte jejich na https://suricata-ids.org/.</span><span class="sxs-lookup"><span data-stu-id="618c9-109">For more details about Suricata and its capabilities, visit their website at https://suricata-ids.org/.</span></span>

## <a name="scenario"></a><span data-ttu-id="618c9-110">Scénář</span><span class="sxs-lookup"><span data-stu-id="618c9-110">Scenario</span></span>

<span data-ttu-id="618c9-111">Tento článek vysvětluje, jak nastavit svoje prostředí k provedení zjišťování neoprávněných vniknutí sítě pomocí sledovací proces sítě, Suricata a elastické zásobníku.</span><span class="sxs-lookup"><span data-stu-id="618c9-111">This article explains how to set up your environment to perform network intrusion detection using Network Watcher, Suricata, and the Elastic Stack.</span></span> <span data-ttu-id="618c9-112">Sledovací proces sítě vám poskytne zachytávání paketů použít k provedení zjišťování neoprávněných vniknutí sítě.</span><span class="sxs-lookup"><span data-stu-id="618c9-112">Network Watcher provides you with the packet captures used to perform network intrusion detection.</span></span> <span data-ttu-id="618c9-113">Suricata zpracovává paketu zachycení a aktivační události výstrahy podle paketů, které odpovídají jeho dané ruleset hrozeb.</span><span class="sxs-lookup"><span data-stu-id="618c9-113">Suricata processes the packet captures and trigger alerts based on packets that match its given ruleset of threats.</span></span> <span data-ttu-id="618c9-114">Tyto výstrahy jsou uloženy v souboru protokolu na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="618c9-114">These alerts are stored in a log file on your local machine.</span></span> <span data-ttu-id="618c9-115">Používání elastické zásobníku, protokoly Suricata může být indexované a použít k vytvoření řídicího panelu Kibana, vám poskytnou vizuální reprezentace protokoly a znamená a rychle získáte přehled o na potenciální ohrožení zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="618c9-115">Using the Elastic Stack, the logs generated by Suricata can be indexed and used to create a Kibana dashboard, providing you with a visual representation of the logs and a means to quickly gain insights to potential network vulnerabilities.</span></span>  

![scénář jednoduché webové aplikace][1]

<span data-ttu-id="618c9-117">Oba nástroje s otevřeným zdrojem můžete být nastavit ve virtuálním počítači Azure, umožňuje provádět analýzy v rámci Azure prostředí sítě.</span><span class="sxs-lookup"><span data-stu-id="618c9-117">Both open source tools can be set up on an Azure VM, allowing you to perform this analysis within your own Azure network environment.</span></span>

## <a name="steps"></a><span data-ttu-id="618c9-118">Kroky</span><span class="sxs-lookup"><span data-stu-id="618c9-118">Steps</span></span>

### <a name="install-suricata"></a><span data-ttu-id="618c9-119">Nainstalujte Suricata</span><span class="sxs-lookup"><span data-stu-id="618c9-119">Install Suricata</span></span>

<span data-ttu-id="618c9-120">Všechny ostatní metody instalace najdete v článku http://suricata.readthedocs.io/en/latest/install.html</span><span class="sxs-lookup"><span data-stu-id="618c9-120">For all other methods of installation, visit http://suricata.readthedocs.io/en/latest/install.html</span></span>

1. <span data-ttu-id="618c9-121">V terminál příkazového řádku virtuálního počítače spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="618c9-121">In the command-line terminal of your VM run the following commands:</span></span>

    ```
    sudo add-apt-repository ppa:oisf/suricata-stable
    sudo apt-get update
    sudo sudo apt-get install suricata
    ```

1. <span data-ttu-id="618c9-122">Pokud chcete ověřit instalaci, spusťte příkaz `suricata -h` chcete zobrazit úplný seznam příkazů.</span><span class="sxs-lookup"><span data-stu-id="618c9-122">To verify your installation, run the command `suricata -h` to see the full list of commands.</span></span>

### <a name="download-the-emerging-threats-ruleset"></a><span data-ttu-id="618c9-123">Stáhnout ruleset vznikající hrozby</span><span class="sxs-lookup"><span data-stu-id="618c9-123">Download the Emerging Threats ruleset</span></span>

<span data-ttu-id="618c9-124">V této fázi nemáme žádná pravidla pro Suricata ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="618c9-124">At this stage, we do not have any rules for Suricata to run.</span></span> <span data-ttu-id="618c9-125">Můžete vytvořit vlastní pravidla, pokud existují specifické hrozby k síti, že chcete zjistit, nebo můžete také vyvinuté pomocí sady pravidel z mnoha různých poskytovatelé, například vznikající hrozby nebo VRT pravidla z Snort.</span><span class="sxs-lookup"><span data-stu-id="618c9-125">You can create your own rules if there are specific threats to your network you would like to detect, or you can also use developed rule sets from a number of providers, such as Emerging Threats, or VRT rules from Snort.</span></span> <span data-ttu-id="618c9-126">Tady používáme volně přístupné ruleset vznikající hrozby:</span><span class="sxs-lookup"><span data-stu-id="618c9-126">We use the freely accessible Emerging Threats ruleset here:</span></span>

<span data-ttu-id="618c9-127">Stáhněte sadu pravidel a zkopírujte je do adresáře:</span><span class="sxs-lookup"><span data-stu-id="618c9-127">Download the rule set and copy them into the directory:</span></span>

```
wget http://rules.emergingthreats.net/open/suricata/emerging.rules.tar.gz
tar zxf emerging.rules.tar.gz
sudo cp -r rules /etc/suricata/
```

### <a name="process-packet-captures-with-suricata"></a><span data-ttu-id="618c9-128">Proces paketu zachytávali Suricata</span><span class="sxs-lookup"><span data-stu-id="618c9-128">Process packet captures with Suricata</span></span>

<span data-ttu-id="618c9-129">Při zpracování paketů zaznamená pomocí Suricata, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="618c9-129">To process packet captures using Suricata, run the following command:</span></span>

```
sudo suricata -c /etc/suricata/suricata.yaml -r <location_of_pcapfile>
```
<span data-ttu-id="618c9-130">Kontrola výsledné výstrah, přečtěte si soubor fast.log:</span><span class="sxs-lookup"><span data-stu-id="618c9-130">To check the resulting alerts, read the fast.log file:</span></span>
```
tail -f /var/log/suricata/fast.log
```

### <a name="set-up-the-elastic-stack"></a><span data-ttu-id="618c9-131">Nastavit elastické zásobníku</span><span class="sxs-lookup"><span data-stu-id="618c9-131">Set up the Elastic Stack</span></span>

<span data-ttu-id="618c9-132">Když protokoly, které vytváří Suricata obsahují cenné informace o dění na naše síť, tyto soubory protokolu nejsou nejjednodušší ke čtení a pochopení.</span><span class="sxs-lookup"><span data-stu-id="618c9-132">While the logs that Suricata produces contain valuable information about what’s happening on our network, these log files aren’t the easiest to read and understand.</span></span> <span data-ttu-id="618c9-133">Připojením Suricata elastické zásobníkem, můžeme vytvořit řídicí panel Kibana a co umožňuje vyhledávat, graf, analyzovat a statistiky odvozena z našich protokolů.</span><span class="sxs-lookup"><span data-stu-id="618c9-133">By connecting Suricata with the Elastic Stack, we can create a Kibana dashboard what allows us to search, graph, analyze, and derive insights from our logs.</span></span>

#### <a name="install-elasticsearch"></a><span data-ttu-id="618c9-134">Nainstalujte Elasticsearch</span><span class="sxs-lookup"><span data-stu-id="618c9-134">Install Elasticsearch</span></span>

1. <span data-ttu-id="618c9-135">Elastické zásobníku z verze 5.0 a vyšší vyžaduje Java 8.</span><span class="sxs-lookup"><span data-stu-id="618c9-135">The Elastic Stack from version 5.0 and above requires Java 8.</span></span> <span data-ttu-id="618c9-136">Spusťte příkaz `java -version` zkontrolujte vaši verzi.</span><span class="sxs-lookup"><span data-stu-id="618c9-136">Run the command `java -version` to check your version.</span></span> <span data-ttu-id="618c9-137">Pokud nemáte java nainstalovat, najdete v dokumentaci k na [Oracle na webu](http://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html)</span><span class="sxs-lookup"><span data-stu-id="618c9-137">If you do not have java install, refer to documentation on [Oracle's website](http://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html)</span></span>
1. <span data-ttu-id="618c9-138">Stáhněte si správné binární balíček pro váš systém:</span><span class="sxs-lookup"><span data-stu-id="618c9-138">Download the correct binary package for your system:</span></span>

    ```
    curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.2.0.deb
    sudo dpkg -i elasticsearch-5.2.0.deb
    sudo /etc/init.d/elasticsearch start
    ```

    <span data-ttu-id="618c9-139">Ostatní metody instalace najdete na [Elasticsearch instalace](https://www.elastic.co/guide/en/beats/libbeat/5.2/elasticsearch-installation.html)</span><span class="sxs-lookup"><span data-stu-id="618c9-139">Other installation methods can be found at [Elasticsearch Installation](https://www.elastic.co/guide/en/beats/libbeat/5.2/elasticsearch-installation.html)</span></span>

1. <span data-ttu-id="618c9-140">Ověřte, zda je spuštěna Elasticsearch pomocí příkazu:</span><span class="sxs-lookup"><span data-stu-id="618c9-140">Verify that Elasticsearch is running with the command:</span></span>

    ```
    curl http://127.0.0.1:9200
    ```

    <span data-ttu-id="618c9-141">Byste měli vidět odpověď podobná této:</span><span class="sxs-lookup"><span data-stu-id="618c9-141">You should see a response similar to this:</span></span>

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

<span data-ttu-id="618c9-142">Další pokyny k instalaci elastické vyhledávání, naleznete na stránce [instalace](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/_installation.html)</span><span class="sxs-lookup"><span data-stu-id="618c9-142">For further instructions on installing Elastic search, refer to the page [Installation](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/_installation.html)</span></span>

### <a name="install-logstash"></a><span data-ttu-id="618c9-143">Nainstalujte Logstash</span><span class="sxs-lookup"><span data-stu-id="618c9-143">Install Logstash</span></span>

1. <span data-ttu-id="618c9-144">Chcete-li nainstalovat Logstash spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="618c9-144">To install Logstash run the following commands:</span></span>

    ```
    curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.deb
    sudo dpkg -i logstash-5.2.0.deb
    ```
1. <span data-ttu-id="618c9-145">Další, je potřeba nakonfigurovat Logstash číst z výstupu eve.json souboru.</span><span class="sxs-lookup"><span data-stu-id="618c9-145">Next we need to configure Logstash to read from the output of eve.json file.</span></span> <span data-ttu-id="618c9-146">Vytvoření souboru logstash.conf pomocí:</span><span class="sxs-lookup"><span data-stu-id="618c9-146">Create a logstash.conf file using:</span></span>

    ```
    sudo touch /etc/logstash/conf.d/logstash.conf
    ```

1. <span data-ttu-id="618c9-147">Přidejte následující obsah do souboru (ujistěte se, zda je cesta k souboru eve.json správný):</span><span class="sxs-lookup"><span data-stu-id="618c9-147">Add the following content to the file (make sure that the path to the eve.json file is correct):</span></span>

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

1. <span data-ttu-id="618c9-148">Zajistěte, aby k správná oprávnění k souboru eve.json aby Logstash můžete ingestování soubor.</span><span class="sxs-lookup"><span data-stu-id="618c9-148">Make sure to give the correct permissions to the eve.json file so that Logstash can ingest the file.</span></span>
    
    ```
    sudo chmod 775 /var/log/suricata/eve.json
    ```

1. <span data-ttu-id="618c9-149">Chcete-li spustit Logstash spusťte příkaz:</span><span class="sxs-lookup"><span data-stu-id="618c9-149">To start Logstash run the command:</span></span>

    ```
    sudo /etc/init.d/logstash start
    ```

<span data-ttu-id="618c9-150">Další pokyny k instalaci Logstash naleznete [oficiální dokumentaci](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)</span><span class="sxs-lookup"><span data-stu-id="618c9-150">For further instructions on installing Logstash, refer to the [official documentation](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)</span></span>

### <a name="install-kibana"></a><span data-ttu-id="618c9-151">Nainstalujte Kibana</span><span class="sxs-lookup"><span data-stu-id="618c9-151">Install Kibana</span></span>

1. <span data-ttu-id="618c9-152">Spusťte následující příkazy pro instalaci Kibana:</span><span class="sxs-lookup"><span data-stu-id="618c9-152">Run the following commands to install Kibana:</span></span>

    ```
    curl -L -O https://artifacts.elastic.co/downloads/kibana/kibana-5.2.0-linux-x86_64.tar.gz
    tar xzvf kibana-5.2.0-linux-x86_64.tar.gz

    ```
1. <span data-ttu-id="618c9-153">Chcete-li spustit Kibana použijte příkazy:</span><span class="sxs-lookup"><span data-stu-id="618c9-153">To run Kibana use the commands:</span></span>

    ```
    cd kibana-5.2.0-linux-x86_64/
    ./bin/kibana
    ```

1. <span data-ttu-id="618c9-154">Chcete-li zobrazit vaše Kibana webové rozhraní, přejděte na`http://localhost:5601`</span><span class="sxs-lookup"><span data-stu-id="618c9-154">To view your Kibana web interface, navigate to `http://localhost:5601`</span></span>
1. <span data-ttu-id="618c9-155">V tomto scénáři je vzor indexu používá pro protokoly Suricata "logstash-*"</span><span class="sxs-lookup"><span data-stu-id="618c9-155">For this scenario, the index pattern used for the Suricata logs is "logstash-*"</span></span>

1. <span data-ttu-id="618c9-156">Pokud chcete zobrazit řídicí panel Kibana vzdáleně, vytvoření příchozího pravidla NSG povolení přístupu k **portu 5601**.</span><span class="sxs-lookup"><span data-stu-id="618c9-156">If you want to view the Kibana dashboard remotely, create an inbound NSG rule allowing access to **port 5601**.</span></span>

### <a name="create-a-kibana-dashboard"></a><span data-ttu-id="618c9-157">Vytvořit řídicí panel Kibana</span><span class="sxs-lookup"><span data-stu-id="618c9-157">Create a Kibana dashboard</span></span>

<span data-ttu-id="618c9-158">V tomto článku uvádíme na ukázkový řídicí panel můžete prohlédnout podrobnosti a trendy v upozornění.</span><span class="sxs-lookup"><span data-stu-id="618c9-158">For this article, we have provided a sample dashboard for you to view trends and details in your alerts.</span></span>

1. <span data-ttu-id="618c9-159">Stáhněte si soubor řídicí panel [sem](https://aka.ms/networkwatchersuricatadashboard), soubor vizualizace [sem](https://aka.ms/networkwatchersuricatavisualization)a soubor uloženého hledání [zde](https://aka.ms/networkwatchersuricatasavedsearch).</span><span class="sxs-lookup"><span data-stu-id="618c9-159">Download the dashboard file [here](https://aka.ms/networkwatchersuricatadashboard), the visualization file [here](https://aka.ms/networkwatchersuricatavisualization), and the saved search file [here](https://aka.ms/networkwatchersuricatasavedsearch).</span></span>

1. <span data-ttu-id="618c9-160">V části **správy** kartě z Kibana, přejděte na **uložit objekty** a importovat všechny tři soubory.</span><span class="sxs-lookup"><span data-stu-id="618c9-160">Under the **Management** tab of Kibana, navigate to **Saved Objects** and import all three files.</span></span> <span data-ttu-id="618c9-161">Potom z **řídicí panel** karta můžete otevřít a načíst ukázkový řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="618c9-161">Then from the **Dashboard** tab you can open and load the sample dashboard.</span></span>

<span data-ttu-id="618c9-162">Můžete také vytvořit vlastní vizualizace a přizpůsobit směrem metriky týkající se vlastní řídicí panely.</span><span class="sxs-lookup"><span data-stu-id="618c9-162">You can also create your own visualizations and dashboards tailored towards metrics of your own interest.</span></span> <span data-ttu-id="618c9-163">Další informace o vytváření vizualizací Kibana z na Kibana [oficiální dokumentaci](https://www.elastic.co/guide/en/kibana/current/visualize.html).</span><span class="sxs-lookup"><span data-stu-id="618c9-163">Read more about creating Kibana visualizations from Kibana's [official documentation](https://www.elastic.co/guide/en/kibana/current/visualize.html).</span></span>

![řídicí panel kibana][2]

### <a name="visualize-ids-alert-logs"></a><span data-ttu-id="618c9-165">Vizualizace ID výstrahy protokoly</span><span class="sxs-lookup"><span data-stu-id="618c9-165">Visualize IDS alert logs</span></span>

<span data-ttu-id="618c9-166">Ukázkový řídicí panel poskytuje několik vizualizace Suricata výstrahy protokolů:</span><span class="sxs-lookup"><span data-stu-id="618c9-166">The sample dashboard provides several visualizations of the Suricata alert logs:</span></span>

1. <span data-ttu-id="618c9-167">Výstrahy podle GeoIP – mapu zobrazující distribuci výstrahy podle země původu podle zeměpisného umístění (určené IP)</span><span class="sxs-lookup"><span data-stu-id="618c9-167">Alerts by GeoIP – a map showing the distribution of alerts by their country of origin based on geographic location (determined by IP)</span></span>

    ![geograficky ip][3]

1. <span data-ttu-id="618c9-169">TOP 10 výstrahy – souhrn nejčastěji se vyskytující 10 aktivovány výstrahy a jejich popis.</span><span class="sxs-lookup"><span data-stu-id="618c9-169">Top 10 Alerts – a summary of the 10 most frequent triggered alerts and their description.</span></span> <span data-ttu-id="618c9-170">Kliknutím na jednotlivé výstrahy filtruje dolů řídicím panelu na informace týkající se této konkrétní výstrahu.</span><span class="sxs-lookup"><span data-stu-id="618c9-170">Clicking an individual alert filters down the dashboard to the information pertaining to that specific alert.</span></span>

    ![Obrázek 4][4]

1. <span data-ttu-id="618c9-172">Počet výstrah – celkový počet výstrahy aktivovány sada pravidel pro</span><span class="sxs-lookup"><span data-stu-id="618c9-172">Number of Alerts – the total count of alerts triggered by the ruleset</span></span>

    ![Obrázek 5][5]

1. <span data-ttu-id="618c9-174">Byla aktivována nejvyšší 20 zdrojové nebo cílové IP adresy nebo porty - výsečové grafy znázorňující nejvyšší 20 IP adresy a porty této výstrahy.</span><span class="sxs-lookup"><span data-stu-id="618c9-174">Top 20 Source/Destination IPs/Ports - pie charts showing the top 20 IPs and ports that alerts were triggered on.</span></span> <span data-ttu-id="618c9-175">Můžete filtrovat na konkrétní IP adresy nebo porty a kolik a jaký druh výstrahy dolů, se aktivují.</span><span class="sxs-lookup"><span data-stu-id="618c9-175">You can filter down on specific IPs/ports to see how many and what kind of alerts are being triggered.</span></span>

    ![Obrázek 6][6]

1. <span data-ttu-id="618c9-177">Souhrn výstrah – tabulky Shrnutí konkrétní podrobnosti o každém individuální výstrahu.</span><span class="sxs-lookup"><span data-stu-id="618c9-177">Alert Summary – a table summarizing specific details of each individual alert.</span></span> <span data-ttu-id="618c9-178">Tato tabulka zobrazíte další parametry, které vás zajímají pro každou výstrahu můžete přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="618c9-178">You can customize this table to show other parameters of interest for each alert.</span></span>

    ![Obrázek 7][7]

<span data-ttu-id="618c9-180">Další dokumentaci týkající se vytvoření vlastního vizualizace a řídicích panelů najdete v tématu [oficiální dokumentaci na Kibana](https://www.elastic.co/guide/en/kibana/current/introduction.html).</span><span class="sxs-lookup"><span data-stu-id="618c9-180">For more documentation on creating custom visualizations and dashboards, see [Kibana’s official documentation](https://www.elastic.co/guide/en/kibana/current/introduction.html).</span></span>

## <a name="conclusion"></a><span data-ttu-id="618c9-181">Závěr</span><span class="sxs-lookup"><span data-stu-id="618c9-181">Conclusion</span></span>

<span data-ttu-id="618c9-182">Kombinací paketu zaznamená zadaný sledovací proces sítě a ID nástroje s otevřeným zdrojem, jako je Suricata, je možné provést zjišťování neoprávněných vniknutí sítě pro širokou škálu hrozeb.</span><span class="sxs-lookup"><span data-stu-id="618c9-182">By combining packet captures provided by Network Watcher and open source IDS tools such as Suricata, you can perform network intrusion detection for a wide range of threats.</span></span> <span data-ttu-id="618c9-183">Tyto řídicí panely vám umožňují rychle rozpoznat trendy a anomálie v rámci vaší sítě, jako dobře dig do data, která mají-li zjistit, že hlavní příčiny výstrahy, jako je například uživatel se zlými úmysly agentů nebo citlivé porty.</span><span class="sxs-lookup"><span data-stu-id="618c9-183">These dashboards allow you to quickly spot trends and anomalies within your network, as well dig into the data to discover root causes of alerts such as malicious user agents or vulnerable ports.</span></span> <span data-ttu-id="618c9-184">S Tento extrahovaná data můžete provést informované rozhodnutí o tom, jak reagovat na ochránit síť před všechny pokusy o škodlivé vniknutí a vytvářet pravidla pro budoucí před proniknutím k vaší síti.</span><span class="sxs-lookup"><span data-stu-id="618c9-184">With this extracted data, you can make informed decisions on how to react to and protect your network from any harmful intrusion attempts, and create rules to prevent future intrusions to your network.</span></span>

## <a name="next-steps"></a><span data-ttu-id="618c9-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="618c9-185">Next steps</span></span>

<span data-ttu-id="618c9-186">Zjistěte, jak aktivovat zachycení paketů, které jsou založeny na výstrahách navštivte stránky [pomocí zachytáváním paketů provádět monitorování proaktivní sítě s Azure Functions](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="618c9-186">Learn how to trigger packet captures based on alerts by visiting [Use packet capture to do proactive network monitoring with Azure Functions](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="618c9-187">Zjistěte, jak toku protokolů NSG s Power BI vizualizovat navštivte stránky [vizualizovat NSG toků protokoly s Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="618c9-187">Learn how to visualize your NSG flow logs with Power BI by visiting [Visualize NSG flows logs with Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>



<!-- images -->
[1]: ./media/network-watcher-intrusion-detection-open-source-tools/figure1.png
[2]: ./media/network-watcher-intrusion-detection-open-source-tools/figure2.png
[3]: ./media/network-watcher-intrusion-detection-open-source-tools/figure3.png
[4]: ./media/network-watcher-intrusion-detection-open-source-tools/figure4.png
[5]: ./media/network-watcher-intrusion-detection-open-source-tools/figure5.png
[6]: ./media/network-watcher-intrusion-detection-open-source-tools/figure6.png
[7]: ./media/network-watcher-intrusion-detection-open-source-tools/figure7.png
