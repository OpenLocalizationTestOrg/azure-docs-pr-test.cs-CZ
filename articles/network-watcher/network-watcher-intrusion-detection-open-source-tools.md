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
# <a name="perform-network-intrusion-detection-with-network-watcher-and-open-source-tools"></a><span data-ttu-id="9eb0b-103">Provedení zjišťování neoprávněných vniknutí sítě s sledovací proces sítě a open source nástroje</span><span class="sxs-lookup"><span data-stu-id="9eb0b-103">Perform network intrusion detection with Network Watcher and open source tools</span></span>

<span data-ttu-id="9eb0b-104">Zachycení paketu jsou klíčovou komponentou pro implementace systémy zjišťování neoprávněných vniknutí sítě (ID) a provádění monitorování zabezpečení sítě (NSM).</span><span class="sxs-lookup"><span data-stu-id="9eb0b-104">Packet captures are a key component for implementing network intrusion detection systems (IDS) and performing Network Security Monitoring (NSM).</span></span> <span data-ttu-id="9eb0b-105">Existuje několik nástrojů ID s otevřeným zdrojem, které zpracování paketů zachycení a vyhledejte podpis vniknutí možné sítě a škodlivé aktivity.</span><span class="sxs-lookup"><span data-stu-id="9eb0b-105">There are several open source IDS tools that process packet captures and look for signatures of possible network intrusions and malicious activity.</span></span> <span data-ttu-id="9eb0b-106">Pomocí zachycení paketu hello poskytuje sledovací proces sítě, můžete analyzovat vaše síť pro všechny škodlivé vniknutí nebo ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="9eb0b-106">Using hello packet captures provided by Network Watcher, you can analyze your network for any harmful intrusions or vulnerabilities.</span></span>

<span data-ttu-id="9eb0b-107">Jeden takový nástroj s otevřeným zdrojem je Suricata, modul ID, která používá sady pravidel toomonitor síťový provoz a aktivuje upozornění pokaždé, když dojde k podezřelé události.</span><span class="sxs-lookup"><span data-stu-id="9eb0b-107">One such open source tool is Suricata, an IDS engine that uses rulesets toomonitor network traffic and triggers alerts whenever suspicious events occur.</span></span> <span data-ttu-id="9eb0b-108">Suricata nabízí modul Vícevláknová, což znamená, že ho můžete provádět analýzy zatížení sítě s vyšší rychlostí a efektivitu.</span><span class="sxs-lookup"><span data-stu-id="9eb0b-108">Suricata offers a multi-threaded engine, meaning it can perform network traffic analysis with increased speed and efficiency.</span></span> <span data-ttu-id="9eb0b-109">Další podrobnosti o Suricata a jeho funkce navštivte jejich na https://suricata-ids.org/.</span><span class="sxs-lookup"><span data-stu-id="9eb0b-109">For more details about Suricata and its capabilities, visit their website at https://suricata-ids.org/.</span></span>

## <a name="scenario"></a><span data-ttu-id="9eb0b-110">Scénář</span><span class="sxs-lookup"><span data-stu-id="9eb0b-110">Scenario</span></span>

<span data-ttu-id="9eb0b-111">Tento článek vysvětluje, jak tooset do vašeho prostředí tooperform sítě pomocí sledovací proces sítě, Suricata, zjišťování neoprávněných vniknutí a hello elastické zásobníku.</span><span class="sxs-lookup"><span data-stu-id="9eb0b-111">This article explains how tooset up your environment tooperform network intrusion detection using Network Watcher, Suricata, and hello Elastic Stack.</span></span> <span data-ttu-id="9eb0b-112">Sledovací proces sítě poskytuje že vám hello paketu zaznamená zjišťování neoprávněných vniknutí použité tooperform sítě.</span><span class="sxs-lookup"><span data-stu-id="9eb0b-112">Network Watcher provides you with hello packet captures used tooperform network intrusion detection.</span></span> <span data-ttu-id="9eb0b-113">Zaznamená Suricata procesy hello paketů a aktivační události výstrahy založené na pakety, které odpovídají jeho dané ruleset hrozeb.</span><span class="sxs-lookup"><span data-stu-id="9eb0b-113">Suricata processes hello packet captures and trigger alerts based on packets that match its given ruleset of threats.</span></span> <span data-ttu-id="9eb0b-114">Tyto výstrahy jsou uloženy v souboru protokolu na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="9eb0b-114">These alerts are stored in a log file on your local machine.</span></span> <span data-ttu-id="9eb0b-115">Hello protokoly Suricata pomocí hello elastické zásobníku, lze indexovat a použít toocreate Kibana řídicího panelu, poskytování vizuální reprezentace hello protokoly a znamená tooquickly získat přehledy toopotential sítě ohrožení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="9eb0b-115">Using hello Elastic Stack, hello logs generated by Suricata can be indexed and used toocreate a Kibana dashboard, providing you with a visual representation of hello logs and a means tooquickly gain insights toopotential network vulnerabilities.</span></span>  

![scénář jednoduché webové aplikace][1]

<span data-ttu-id="9eb0b-117">Obě open source nástroje lze nastavit na virtuální počítač Azure, umožní vám tooperform tuto analýzu v prostředí sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="9eb0b-117">Both open source tools can be set up on an Azure VM, allowing you tooperform this analysis within your own Azure network environment.</span></span>

## <a name="steps"></a><span data-ttu-id="9eb0b-118">Kroky</span><span class="sxs-lookup"><span data-stu-id="9eb0b-118">Steps</span></span>

### <a name="install-suricata"></a><span data-ttu-id="9eb0b-119">Nainstalujte Suricata</span><span class="sxs-lookup"><span data-stu-id="9eb0b-119">Install Suricata</span></span>

<span data-ttu-id="9eb0b-120">Všechny ostatní metody instalace najdete v článku http://suricata.readthedocs.io/en/latest/install.html</span><span class="sxs-lookup"><span data-stu-id="9eb0b-120">For all other methods of installation, visit http://suricata.readthedocs.io/en/latest/install.html</span></span>

1. <span data-ttu-id="9eb0b-121">V hello terminál příkazového řádku virtuálního počítače spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="9eb0b-121">In hello command-line terminal of your VM run hello following commands:</span></span>

    ```
    sudo add-apt-repository ppa:oisf/suricata-stable
    sudo apt-get update
    sudo sudo apt-get install suricata
    ```

1. <span data-ttu-id="9eb0b-122">tooverify instalace, spusťte příkaz hello `suricata -h` toosee hello úplný seznam příkazů.</span><span class="sxs-lookup"><span data-stu-id="9eb0b-122">tooverify your installation, run hello command `suricata -h` toosee hello full list of commands.</span></span>

### <a name="download-hello-emerging-threats-ruleset"></a><span data-ttu-id="9eb0b-123">Stáhnout ruleset vznikající hrozby hello</span><span class="sxs-lookup"><span data-stu-id="9eb0b-123">Download hello Emerging Threats ruleset</span></span>

<span data-ttu-id="9eb0b-124">V této fázi nemáme žádná pravidla pro Suricata toorun.</span><span class="sxs-lookup"><span data-stu-id="9eb0b-124">At this stage, we do not have any rules for Suricata toorun.</span></span> <span data-ttu-id="9eb0b-125">Pokud neexistují specifické hrozby tooyour sítě chcete toodetect, nebo můžete také vyvinuté pomocí sady pravidel z mnoha různých poskytovatelé, například vznikající hrozby nebo VRT pravidla z Snort, můžete vytvořit vlastní pravidla.</span><span class="sxs-lookup"><span data-stu-id="9eb0b-125">You can create your own rules if there are specific threats tooyour network you would like toodetect, or you can also use developed rule sets from a number of providers, such as Emerging Threats, or VRT rules from Snort.</span></span> <span data-ttu-id="9eb0b-126">Tady používáme volně přístupné ruleset vznikající hrozby hello:</span><span class="sxs-lookup"><span data-stu-id="9eb0b-126">We use hello freely accessible Emerging Threats ruleset here:</span></span>

<span data-ttu-id="9eb0b-127">Stáhněte sadu pravidel hello a zkopírujte je do adresáře hello:</span><span class="sxs-lookup"><span data-stu-id="9eb0b-127">Download hello rule set and copy them into hello directory:</span></span>

```
wget http://rules.emergingthreats.net/open/suricata/emerging.rules.tar.gz
tar zxf emerging.rules.tar.gz
sudo cp -r rules /etc/suricata/
```

### <a name="process-packet-captures-with-suricata"></a><span data-ttu-id="9eb0b-128">Proces paketu zachytávali Suricata</span><span class="sxs-lookup"><span data-stu-id="9eb0b-128">Process packet captures with Suricata</span></span>

<span data-ttu-id="9eb0b-129">paket tooprocess zaznamená pomocí Suricata, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="9eb0b-129">tooprocess packet captures using Suricata, run hello following command:</span></span>

```
sudo suricata -c /etc/suricata/suricata.yaml -r <location_of_pcapfile>
```
<span data-ttu-id="9eb0b-130">toocheck hello výsledné výstrahy, přečtěte si soubor fast.log hello:</span><span class="sxs-lookup"><span data-stu-id="9eb0b-130">toocheck hello resulting alerts, read hello fast.log file:</span></span>
```
tail -f /var/log/suricata/fast.log
```

### <a name="set-up-hello-elastic-stack"></a><span data-ttu-id="9eb0b-131">Nastavit hello elastické zásobníku</span><span class="sxs-lookup"><span data-stu-id="9eb0b-131">Set up hello Elastic Stack</span></span>

<span data-ttu-id="9eb0b-132">Zatímco hello protokoly, které vytváří Suricata obsahovat cenné informace o dění na naše síť, tyto soubory protokolu nejsou nejjednodušší tooread hello a pochopit.</span><span class="sxs-lookup"><span data-stu-id="9eb0b-132">While hello logs that Suricata produces contain valuable information about what’s happening on our network, these log files aren’t hello easiest tooread and understand.</span></span> <span data-ttu-id="9eb0b-133">Propojením Suricata s hello elastické zásobníku jsme vytvořit řídicí panel Kibana co nám umožňují toosearch, graf, analyzovat a statistiky odvozena z našich protokolů.</span><span class="sxs-lookup"><span data-stu-id="9eb0b-133">By connecting Suricata with hello Elastic Stack, we can create a Kibana dashboard what allows us toosearch, graph, analyze, and derive insights from our logs.</span></span>

#### <a name="install-elasticsearch"></a><span data-ttu-id="9eb0b-134">Nainstalujte Elasticsearch</span><span class="sxs-lookup"><span data-stu-id="9eb0b-134">Install Elasticsearch</span></span>

1. <span data-ttu-id="9eb0b-135">Hello elastické zásobníku z verze 5.0 a vyšší vyžaduje Java 8.</span><span class="sxs-lookup"><span data-stu-id="9eb0b-135">hello Elastic Stack from version 5.0 and above requires Java 8.</span></span> <span data-ttu-id="9eb0b-136">Spusťte příkaz hello `java -version` toocheck vaší verzí.</span><span class="sxs-lookup"><span data-stu-id="9eb0b-136">Run hello command `java -version` toocheck your version.</span></span> <span data-ttu-id="9eb0b-137">Pokud nemáte java nainstalovat, naleznete v toodocumentation [Oracle na webu](http://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html)</span><span class="sxs-lookup"><span data-stu-id="9eb0b-137">If you do not have java install, refer toodocumentation on [Oracle's website](http://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html)</span></span>
1. <span data-ttu-id="9eb0b-138">Stáhněte si balíček správné binární hello systému:</span><span class="sxs-lookup"><span data-stu-id="9eb0b-138">Download hello correct binary package for your system:</span></span>

    ```
    curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.2.0.deb
    sudo dpkg -i elasticsearch-5.2.0.deb
    sudo /etc/init.d/elasticsearch start
    ```

    <span data-ttu-id="9eb0b-139">Ostatní metody instalace najdete na [Elasticsearch instalace](https://www.elastic.co/guide/en/beats/libbeat/5.2/elasticsearch-installation.html)</span><span class="sxs-lookup"><span data-stu-id="9eb0b-139">Other installation methods can be found at [Elasticsearch Installation](https://www.elastic.co/guide/en/beats/libbeat/5.2/elasticsearch-installation.html)</span></span>

1. <span data-ttu-id="9eb0b-140">Ověřte, zda je spuštěna Elasticsearch příkazem hello:</span><span class="sxs-lookup"><span data-stu-id="9eb0b-140">Verify that Elasticsearch is running with hello command:</span></span>

    ```
    curl http://127.0.0.1:9200
    ```

    <span data-ttu-id="9eb0b-141">Měli byste vidět podobné toothis odpovědi:</span><span class="sxs-lookup"><span data-stu-id="9eb0b-141">You should see a response similar toothis:</span></span>

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

<span data-ttu-id="9eb0b-142">Další pokyny pro instalaci elastické vyhledávání, najdete v části stránky toohello [instalace](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/_installation.html)</span><span class="sxs-lookup"><span data-stu-id="9eb0b-142">For further instructions on installing Elastic search, refer toohello page [Installation](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/_installation.html)</span></span>

### <a name="install-logstash"></a><span data-ttu-id="9eb0b-143">Nainstalujte Logstash</span><span class="sxs-lookup"><span data-stu-id="9eb0b-143">Install Logstash</span></span>

1. <span data-ttu-id="9eb0b-144">tooinstall Logstash spusťte hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="9eb0b-144">tooinstall Logstash run hello following commands:</span></span>

    ```
    curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.deb
    sudo dpkg -i logstash-5.2.0.deb
    ```
1. <span data-ttu-id="9eb0b-145">Další potřebujeme tooconfigure Logstash tooread z výstupu hello eve.json souboru.</span><span class="sxs-lookup"><span data-stu-id="9eb0b-145">Next we need tooconfigure Logstash tooread from hello output of eve.json file.</span></span> <span data-ttu-id="9eb0b-146">Vytvoření souboru logstash.conf pomocí:</span><span class="sxs-lookup"><span data-stu-id="9eb0b-146">Create a logstash.conf file using:</span></span>

    ```
    sudo touch /etc/logstash/conf.d/logstash.conf
    ```

1. <span data-ttu-id="9eb0b-147">Přidejte následující soubor obsahu toohello hello (ujistěte se, že hello cesta toohello eve.json soubor je správný):</span><span class="sxs-lookup"><span data-stu-id="9eb0b-147">Add hello following content toohello file (make sure that hello path toohello eve.json file is correct):</span></span>

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

1. <span data-ttu-id="9eb0b-148">Zajistěte, aby toogive hello správná oprávnění toohello eve.json soubor tak, aby Logstash dokáže přijímat hello souboru.</span><span class="sxs-lookup"><span data-stu-id="9eb0b-148">Make sure toogive hello correct permissions toohello eve.json file so that Logstash can ingest hello file.</span></span>
    
    ```
    sudo chmod 775 /var/log/suricata/eve.json
    ```

1. <span data-ttu-id="9eb0b-149">toostart Logstash spusťte příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="9eb0b-149">toostart Logstash run hello command:</span></span>

    ```
    sudo /etc/init.d/logstash start
    ```

<span data-ttu-id="9eb0b-150">Další pokyny k instalaci Logstash, najdete v části toohello [oficiální dokumentaci](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)</span><span class="sxs-lookup"><span data-stu-id="9eb0b-150">For further instructions on installing Logstash, refer toohello [official documentation](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)</span></span>

### <a name="install-kibana"></a><span data-ttu-id="9eb0b-151">Nainstalujte Kibana</span><span class="sxs-lookup"><span data-stu-id="9eb0b-151">Install Kibana</span></span>

1. <span data-ttu-id="9eb0b-152">Spusťte následující příkazy tooinstall Kibana hello:</span><span class="sxs-lookup"><span data-stu-id="9eb0b-152">Run hello following commands tooinstall Kibana:</span></span>

    ```
    curl -L -O https://artifacts.elastic.co/downloads/kibana/kibana-5.2.0-linux-x86_64.tar.gz
    tar xzvf kibana-5.2.0-linux-x86_64.tar.gz

    ```
1. <span data-ttu-id="9eb0b-153">toorun Kibana použijte příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="9eb0b-153">toorun Kibana use hello commands:</span></span>

    ```
    cd kibana-5.2.0-linux-x86_64/
    ./bin/kibana
    ```

1. <span data-ttu-id="9eb0b-154">tooview Kibana webového rozhraní, přejděte příliš`http://localhost:5601`</span><span class="sxs-lookup"><span data-stu-id="9eb0b-154">tooview your Kibana web interface, navigate too`http://localhost:5601`</span></span>
1. <span data-ttu-id="9eb0b-155">V tomto scénáři hello index způsobem používaným pro hello Suricata protokolů je "logstash-*"</span><span class="sxs-lookup"><span data-stu-id="9eb0b-155">For this scenario, hello index pattern used for hello Suricata logs is "logstash-*"</span></span>

1. <span data-ttu-id="9eb0b-156">Pokud chcete vzdáleně tooview hello Kibana řídicí panel, vytvoření příchozího pravidla NSG povolením přístupu příliš**portu 5601**.</span><span class="sxs-lookup"><span data-stu-id="9eb0b-156">If you want tooview hello Kibana dashboard remotely, create an inbound NSG rule allowing access too**port 5601**.</span></span>

### <a name="create-a-kibana-dashboard"></a><span data-ttu-id="9eb0b-157">Vytvořit řídicí panel Kibana</span><span class="sxs-lookup"><span data-stu-id="9eb0b-157">Create a Kibana dashboard</span></span>

<span data-ttu-id="9eb0b-158">V tomto článku uvádíme ukázkový řídicí panel vám tooview trendy a podrobnosti v upozornění.</span><span class="sxs-lookup"><span data-stu-id="9eb0b-158">For this article, we have provided a sample dashboard for you tooview trends and details in your alerts.</span></span>

1. <span data-ttu-id="9eb0b-159">Stažení souboru řídicí panel hello [sem](https://aka.ms/networkwatchersuricatadashboard), hello vizualizace souboru [sem](https://aka.ms/networkwatchersuricatavisualization)a soubor hello uložené hledání [zde](https://aka.ms/networkwatchersuricatasavedsearch).</span><span class="sxs-lookup"><span data-stu-id="9eb0b-159">Download hello dashboard file [here](https://aka.ms/networkwatchersuricatadashboard), hello visualization file [here](https://aka.ms/networkwatchersuricatavisualization), and hello saved search file [here](https://aka.ms/networkwatchersuricatasavedsearch).</span></span>

1. <span data-ttu-id="9eb0b-160">V části hello **správy** kartě z Kibana, přejděte příliš**uložit objekty** a importovat všechny tři soubory.</span><span class="sxs-lookup"><span data-stu-id="9eb0b-160">Under hello **Management** tab of Kibana, navigate too**Saved Objects** and import all three files.</span></span> <span data-ttu-id="9eb0b-161">Potom z hello **řídicí panel** karta můžete otevřít a zatížení hello ukázkový řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="9eb0b-161">Then from hello **Dashboard** tab you can open and load hello sample dashboard.</span></span>

<span data-ttu-id="9eb0b-162">Můžete také vytvořit vlastní vizualizace a přizpůsobit směrem metriky týkající se vlastní řídicí panely.</span><span class="sxs-lookup"><span data-stu-id="9eb0b-162">You can also create your own visualizations and dashboards tailored towards metrics of your own interest.</span></span> <span data-ttu-id="9eb0b-163">Další informace o vytváření vizualizací Kibana z na Kibana [oficiální dokumentaci](https://www.elastic.co/guide/en/kibana/current/visualize.html).</span><span class="sxs-lookup"><span data-stu-id="9eb0b-163">Read more about creating Kibana visualizations from Kibana's [official documentation](https://www.elastic.co/guide/en/kibana/current/visualize.html).</span></span>

![řídicí panel kibana][2]

### <a name="visualize-ids-alert-logs"></a><span data-ttu-id="9eb0b-165">Vizualizace ID výstrahy protokoly</span><span class="sxs-lookup"><span data-stu-id="9eb0b-165">Visualize IDS alert logs</span></span>

<span data-ttu-id="9eb0b-166">Hello ukázkový řídicí panel poskytuje několik vizualizace hello Suricata výstrahy protokolů:</span><span class="sxs-lookup"><span data-stu-id="9eb0b-166">hello sample dashboard provides several visualizations of hello Suricata alert logs:</span></span>

1. <span data-ttu-id="9eb0b-167">Výstrahy podle GeoIP – mapu zobrazující hello distribuce výstrahy podle země původu podle zeměpisného umístění (určené IP)</span><span class="sxs-lookup"><span data-stu-id="9eb0b-167">Alerts by GeoIP – a map showing hello distribution of alerts by their country of origin based on geographic location (determined by IP)</span></span>

    ![geograficky ip][3]

1. <span data-ttu-id="9eb0b-169">TOP 10 výstrahy – souhrn hello 10 nejčastěji se vyskytující aktivaci výstrah a jejich popis.</span><span class="sxs-lookup"><span data-stu-id="9eb0b-169">Top 10 Alerts – a summary of hello 10 most frequent triggered alerts and their description.</span></span> <span data-ttu-id="9eb0b-170">Kliknutím na jednotlivé výstrahy filtruje dolů hello řídicí panel toohello informace týkající se toothat konkrétní výstrahu.</span><span class="sxs-lookup"><span data-stu-id="9eb0b-170">Clicking an individual alert filters down hello dashboard toohello information pertaining toothat specific alert.</span></span>

    ![Obrázek 4][4]

1. <span data-ttu-id="9eb0b-172">Počet výstrah – celkový počet výstrahy aktivovány hello ruleset hello</span><span class="sxs-lookup"><span data-stu-id="9eb0b-172">Number of Alerts – hello total count of alerts triggered by hello ruleset</span></span>

    ![Obrázek 5][5]

1. <span data-ttu-id="9eb0b-174">Prvních 20 počítačů zdrojové nebo cílové IP adresy nebo porty - výsečové grafy znázorňující hello nejvyšší 20 IP adresy a porty, které výstrahy byla aktivována.</span><span class="sxs-lookup"><span data-stu-id="9eb0b-174">Top 20 Source/Destination IPs/Ports - pie charts showing hello top 20 IPs and ports that alerts were triggered on.</span></span> <span data-ttu-id="9eb0b-175">Můžete filtrovat dolů na konkrétní IP adresy nebo porty toosee kolik a jaký druh výstrahy, se aktivují.</span><span class="sxs-lookup"><span data-stu-id="9eb0b-175">You can filter down on specific IPs/ports toosee how many and what kind of alerts are being triggered.</span></span>

    ![Obrázek 6][6]

1. <span data-ttu-id="9eb0b-177">Souhrn výstrah – tabulky Shrnutí konkrétní podrobnosti o každém individuální výstrahu.</span><span class="sxs-lookup"><span data-stu-id="9eb0b-177">Alert Summary – a table summarizing specific details of each individual alert.</span></span> <span data-ttu-id="9eb0b-178">Tato tabulka tooshow můžete přizpůsobit další parametry, které vás zajímají pro každou výstrahu.</span><span class="sxs-lookup"><span data-stu-id="9eb0b-178">You can customize this table tooshow other parameters of interest for each alert.</span></span>

    ![Obrázek 7][7]

<span data-ttu-id="9eb0b-180">Další dokumentaci týkající se vytvoření vlastního vizualizace a řídicích panelů najdete v tématu [oficiální dokumentaci na Kibana](https://www.elastic.co/guide/en/kibana/current/introduction.html).</span><span class="sxs-lookup"><span data-stu-id="9eb0b-180">For more documentation on creating custom visualizations and dashboards, see [Kibana’s official documentation](https://www.elastic.co/guide/en/kibana/current/introduction.html).</span></span>

## <a name="conclusion"></a><span data-ttu-id="9eb0b-181">Závěr</span><span class="sxs-lookup"><span data-stu-id="9eb0b-181">Conclusion</span></span>

<span data-ttu-id="9eb0b-182">Kombinací paketu zaznamená zadaný sledovací proces sítě a ID nástroje s otevřeným zdrojem, jako je Suricata, je možné provést zjišťování neoprávněných vniknutí sítě pro širokou škálu hrozeb.</span><span class="sxs-lookup"><span data-stu-id="9eb0b-182">By combining packet captures provided by Network Watcher and open source IDS tools such as Suricata, you can perform network intrusion detection for a wide range of threats.</span></span> <span data-ttu-id="9eb0b-183">Tyto řídicí panely povolit, můžete tooquickly trendy a anomálie v rámci vaší sítě i proniknout do hello data toodiscover hlavní příčiny výstrahy, jako je například uživatel se zlými úmysly agentů nebo citlivé porty.</span><span class="sxs-lookup"><span data-stu-id="9eb0b-183">These dashboards allow you tooquickly spot trends and anomalies within your network, as well dig into hello data toodiscover root causes of alerts such as malicious user agents or vulnerable ports.</span></span> <span data-ttu-id="9eb0b-184">Tato extrahovaná data můžete informovaně rozhodnout o jak tooreact tooand chránit vaši síť před všechny pokusy o škodlivé vniknutí a vytvoření pravidla tooprevent budoucí vniknutí tooyour sítě.</span><span class="sxs-lookup"><span data-stu-id="9eb0b-184">With this extracted data, you can make informed decisions on how tooreact tooand protect your network from any harmful intrusion attempts, and create rules tooprevent future intrusions tooyour network.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9eb0b-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9eb0b-185">Next steps</span></span>

<span data-ttu-id="9eb0b-186">Zjistěte, jak zaznamená tootrigger paketů na základě výstrahy navštivte stránky [pomocí paketu zachycení toodo Proaktivní monitorování sítě s Azure Functions](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="9eb0b-186">Learn how tootrigger packet captures based on alerts by visiting [Use packet capture toodo proactive network monitoring with Azure Functions](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="9eb0b-187">Zjistěte, jak toovisualize vaše skupina NSG toku protokoly s Power BI navštivte stránky [vizualizovat NSG toků protokoly s Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="9eb0b-187">Learn how toovisualize your NSG flow logs with Power BI by visiting [Visualize NSG flows logs with Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>



<!-- images -->
[1]: ./media/network-watcher-intrusion-detection-open-source-tools/figure1.png
[2]: ./media/network-watcher-intrusion-detection-open-source-tools/figure2.png
[3]: ./media/network-watcher-intrusion-detection-open-source-tools/figure3.png
[4]: ./media/network-watcher-intrusion-detection-open-source-tools/figure4.png
[5]: ./media/network-watcher-intrusion-detection-open-source-tools/figure5.png
[6]: ./media/network-watcher-intrusion-detection-open-source-tools/figure6.png
[7]: ./media/network-watcher-intrusion-detection-open-source-tools/figure7.png
