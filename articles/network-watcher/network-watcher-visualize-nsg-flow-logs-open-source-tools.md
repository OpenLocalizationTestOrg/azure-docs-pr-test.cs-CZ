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
# <a name="visualize-azure-network-watcher-nsg-flow-logs-using-open-source-tools"></a><span data-ttu-id="0e944-103">Vizualizace protokolů toku NSG sledovací proces sítě Azure pomocí nástroje s otevřeným zdrojem</span><span class="sxs-lookup"><span data-stu-id="0e944-103">Visualize Azure Network Watcher NSG flow logs using open source tools</span></span>

<span data-ttu-id="0e944-104">Skupina zabezpečení sítě toku protokoly poskytují informace, které můžete použít pochopit příchozí a odchozí přenosy IP na skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="0e944-104">Network Security Group flow logs provide information that can be used understand ingress and egress IP traffic on Network Security Groups.</span></span> <span data-ttu-id="0e944-105">Tyto protokoly toku zobrazit odchozí a příchozí tok na základě za pravidlo hello seskupování hello toku platí pro, 5 řazené kolekce členů informace o toku hello (zdrojové nebo cílové IP adresy, zdrojový nebo cílový Port, protokol), a pokud bylo povolené nebo zakázané hello přenosy.</span><span class="sxs-lookup"><span data-stu-id="0e944-105">These flow logs show outbound and inbound flows on a per rule basis, hello NIC hello flow applies to, 5 tuple information about hello flow (Source/Destination IP, Source/Destination Port, Protocol), and if hello traffic was allowed or denied.</span></span>

<span data-ttu-id="0e944-106">Tyto protokoly toku může být obtížné toomanually analýzy a získat přehledy z.</span><span class="sxs-lookup"><span data-stu-id="0e944-106">These flow logs can be difficult toomanually parse and gain insights from.</span></span> <span data-ttu-id="0e944-107">Existuje však několik nástrojů s otevřeným zdrojem, které vám mohou pomoci vizualizovat tato data.</span><span class="sxs-lookup"><span data-stu-id="0e944-107">However, there are several open source tools that can help visualize this data.</span></span> <span data-ttu-id="0e944-108">Tento článek vám poskytne tyto protokoly hello elastické zásobníku, pomocí kterých bude umožňují tooquickly index a vizualizovat svoje protokoly toku na řídicím panelu Kibana toovisualize řešení.</span><span class="sxs-lookup"><span data-stu-id="0e944-108">This article will provide a solution toovisualize these logs using hello Elastic Stack, which will allow you tooquickly index and visualize your flow logs on a Kibana dashboard.</span></span>

## <a name="scenario"></a><span data-ttu-id="0e944-109">Scénář</span><span class="sxs-lookup"><span data-stu-id="0e944-109">Scenario</span></span>

<span data-ttu-id="0e944-110">V tomto článku nastavíme řešení, které vám umožní toovisualize skupinu zabezpečení sítě toku protokolů pomocí hello elastické zásobníku.</span><span class="sxs-lookup"><span data-stu-id="0e944-110">In this article, we will set up a solution that will allow you toovisualize Network Security Group flow logs using hello Elastic Stack.</span></span>  <span data-ttu-id="0e944-111">O Logstash vstupní modul plug-in obdrží hello toku protokoly přímo z objektu blob úložiště hello nakonfigurovaný pro obsahující protokoly toku hello.</span><span class="sxs-lookup"><span data-stu-id="0e944-111">A Logstash input plugin will obtain hello flow logs directly from hello storage blob configured for containing hello flow logs.</span></span> <span data-ttu-id="0e944-112">Potom pomocí hello elastické zásobníku, protokoly toku hello bude indexované a použít toocreate informací o Kibana toovisualize řídicí panel hello.</span><span class="sxs-lookup"><span data-stu-id="0e944-112">Then, using hello Elastic Stack, hello flow logs will be indexed and used toocreate a Kibana dashboard toovisualize hello information.</span></span>

![scénář][scenario]

## <a name="steps"></a><span data-ttu-id="0e944-114">Kroky</span><span class="sxs-lookup"><span data-stu-id="0e944-114">Steps</span></span>

### <a name="enable-network-security-group-flow-logging"></a><span data-ttu-id="0e944-115">Protokolování toku povolit skupinu zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="0e944-115">Enable Network Security Group flow logging</span></span>
<span data-ttu-id="0e944-116">V tomto scénáři musí mít síťové zabezpečení skupiny toku protokolování zapnuta alespoň jednu skupinu zabezpečení sítě ve vašem účtu.</span><span class="sxs-lookup"><span data-stu-id="0e944-116">For this scenario, you must have Network Security Group Flow Logging enabled on at least one Network Security Group in your account.</span></span> <span data-ttu-id="0e944-117">Pokyny k povolení protokolů toku zabezpečení sítě, najdete v části toohello následujícího článku [Úvod tooflow protokolování pro skupiny zabezpečení sítě](network-watcher-nsg-flow-logging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0e944-117">For instructions on enabling Network Security Flow Logs, refer toohello following article [Introduction tooflow logging for Network Security Groups](network-watcher-nsg-flow-logging-overview.md).</span></span>


### <a name="set-up-hello-elastic-stack"></a><span data-ttu-id="0e944-118">Nastavit hello elastické zásobníku</span><span class="sxs-lookup"><span data-stu-id="0e944-118">Set up hello Elastic Stack</span></span>
<span data-ttu-id="0e944-119">Propojením NSG toku protokoly s hello elastické zásobníku, můžeme vytvořit řídicí panel Kibana co nám umožňují toosearch, graf, analyzovat a odvozena statistiky z našich protokolů.</span><span class="sxs-lookup"><span data-stu-id="0e944-119">By connecting NSG flow logs with hello Elastic Stack, we can create a Kibana dashboard what allows us toosearch, graph, analyze, and derive insights from our logs.</span></span>

#### <a name="install-elasticsearch"></a><span data-ttu-id="0e944-120">Nainstalujte Elasticsearch</span><span class="sxs-lookup"><span data-stu-id="0e944-120">Install Elasticsearch</span></span>

1. <span data-ttu-id="0e944-121">Hello elastické zásobníku z verze 5.0 a vyšší vyžaduje Java 8.</span><span class="sxs-lookup"><span data-stu-id="0e944-121">hello Elastic Stack from version 5.0 and above requires Java 8.</span></span> <span data-ttu-id="0e944-122">Spusťte příkaz hello `java -version` toocheck vaší verzí.</span><span class="sxs-lookup"><span data-stu-id="0e944-122">Run hello command `java -version` toocheck your version.</span></span> <span data-ttu-id="0e944-123">Pokud nemáte java nainstalovat, naleznete v toodocumentation [Oracle na webu](http://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html)</span><span class="sxs-lookup"><span data-stu-id="0e944-123">If you do not have java install, refer toodocumentation on [Oracle's website](http://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html)</span></span>
1. <span data-ttu-id="0e944-124">Stáhněte si balíček správné binární hello systému:</span><span class="sxs-lookup"><span data-stu-id="0e944-124">Download hello correct binary package for your system:</span></span>

    ```
    curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.2.0.deb
    sudo dpkg -i elasticsearch-5.2.0.deb
    sudo /etc/init.d/elasticsearch start
    ```

    <span data-ttu-id="0e944-125">Ostatní metody instalace najdete na [Elasticsearch instalace](https://www.elastic.co/guide/en/beats/libbeat/5.2/elasticsearch-installation.html)</span><span class="sxs-lookup"><span data-stu-id="0e944-125">Other installation methods can be found at [Elasticsearch Installation](https://www.elastic.co/guide/en/beats/libbeat/5.2/elasticsearch-installation.html)</span></span>

1. <span data-ttu-id="0e944-126">Ověřte, zda je spuštěna Elasticsearch příkazem hello:</span><span class="sxs-lookup"><span data-stu-id="0e944-126">Verify that Elasticsearch is running with hello command:</span></span>

    ```
    curl http://127.0.0.1:9200
    ```

    <span data-ttu-id="0e944-127">Měli byste vidět podobné toothis odpovědi:</span><span class="sxs-lookup"><span data-stu-id="0e944-127">You should see a response similar toothis:</span></span>

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

<span data-ttu-id="0e944-128">Další pokyny pro instalaci elastické vyhledávání, najdete v části stránky toohello [instalace](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/_installation.html)</span><span class="sxs-lookup"><span data-stu-id="0e944-128">For further instructions on installing Elastic search, refer toohello page [Installation](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/_installation.html)</span></span>

### <a name="install-logstash"></a><span data-ttu-id="0e944-129">Nainstalujte Logstash</span><span class="sxs-lookup"><span data-stu-id="0e944-129">Install Logstash</span></span>

1. <span data-ttu-id="0e944-130">tooinstall Logstash spusťte hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="0e944-130">tooinstall Logstash run hello following commands:</span></span>

    ```
    curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.deb
    sudo dpkg -i logstash-5.2.0.deb
    ```
1. <span data-ttu-id="0e944-131">V dalším potřebovat tooconfigure Logstash tooaccess a analyzovat protokoly toku hello.</span><span class="sxs-lookup"><span data-stu-id="0e944-131">Next we need tooconfigure Logstash tooaccess and parse hello flow logs.</span></span> <span data-ttu-id="0e944-132">Vytvoření souboru logstash.conf pomocí:</span><span class="sxs-lookup"><span data-stu-id="0e944-132">Create a logstash.conf file using:</span></span>

    ```
    sudo touch /etc/logstash/conf.d/logstash.conf
    ```

1. <span data-ttu-id="0e944-133">Přidejte následující soubor obsahu toohello hello:</span><span class="sxs-lookup"><span data-stu-id="0e944-133">Add hello following content toohello file:</span></span>

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

<span data-ttu-id="0e944-134">Další pokyny k instalaci Logstash, najdete v části toohello [oficiální dokumentaci](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)</span><span class="sxs-lookup"><span data-stu-id="0e944-134">For further instructions on installing Logstash, refer toohello [official documentation](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)</span></span>

### <a name="install-hello-logstash-input-plugin-for-azure-blob-storage"></a><span data-ttu-id="0e944-135">Nainstalujte hello Logstash vstupní modul plug-in pro úložiště objektů blob v Azure</span><span class="sxs-lookup"><span data-stu-id="0e944-135">Install hello Logstash input plugin for Azure blob storage</span></span>

<span data-ttu-id="0e944-136">Tento modul plug-in Logstash vám umožní toodirectly přístup hello toku protokoly ze svého účtu úložiště určený.</span><span class="sxs-lookup"><span data-stu-id="0e944-136">This Logstash plugin will allow you toodirectly access hello flow logs from their designated storage account.</span></span> <span data-ttu-id="0e944-137">tooinstall tento modul plug-in z hello výchozí Logstash instalační adresář (v této případu /usr/share/logstash/bin) spusťte příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="0e944-137">tooinstall this plugin, from hello default Logstash installation directory (in this case /usr/share/logstash/bin) run hello command:</span></span>

```
logstash-plugin install logstash-input-azureblob
```

<span data-ttu-id="0e944-138">toostart Logstash spusťte příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="0e944-138">toostart Logstash run hello command:</span></span>

```
sudo /etc/init.d/logstash start
```

<span data-ttu-id="0e944-139">Další informace o tento modul plug-in, najdete v části toodocumentation [sem](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob)</span><span class="sxs-lookup"><span data-stu-id="0e944-139">For more information about this plugin, refer toodocumentation [here](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob)</span></span>

### <a name="install-kibana"></a><span data-ttu-id="0e944-140">Nainstalujte Kibana</span><span class="sxs-lookup"><span data-stu-id="0e944-140">Install Kibana</span></span>

1. <span data-ttu-id="0e944-141">Spusťte následující příkazy tooinstall Kibana hello:</span><span class="sxs-lookup"><span data-stu-id="0e944-141">Run hello following commands tooinstall Kibana:</span></span>

  ```
  curl -L -O https://artifacts.elastic.co/downloads/kibana/kibana-5.2.0-linux-x86_64.tar.gz
  tar xzvf kibana-5.2.0-linux-x86_64.tar.gz
  ```

1. <span data-ttu-id="0e944-142">toorun Kibana použijte příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="0e944-142">toorun Kibana use hello commands:</span></span>

  ```
  cd kibana-5.2.0-linux-x86_64/
  ./bin/kibana
  ```

1. <span data-ttu-id="0e944-143">tooview Kibana webového rozhraní, přejděte příliš`http://localhost:5601`</span><span class="sxs-lookup"><span data-stu-id="0e944-143">tooview your Kibana web interface, navigate too`http://localhost:5601`</span></span>
1. <span data-ttu-id="0e944-144">Pro tento scénář je hello index způsobem používaným pro protokoly toku hello "protokolů nsg toku".</span><span class="sxs-lookup"><span data-stu-id="0e944-144">For this scenario, hello index pattern used for hello flow logs is "nsg-flow-logs".</span></span> <span data-ttu-id="0e944-145">Můžete změnit vzor hello indexu v části "výstupní" hello logstash.conf souboru.</span><span class="sxs-lookup"><span data-stu-id="0e944-145">You may change hello index pattern in hello "output" section of your logstash.conf file.</span></span>

1. <span data-ttu-id="0e944-146">Pokud chcete vzdáleně tooview hello Kibana řídicí panel, vytvoření příchozího pravidla NSG povolením přístupu příliš**portu 5601**.</span><span class="sxs-lookup"><span data-stu-id="0e944-146">If you want tooview hello Kibana dashboard remotely, create an inbound NSG rule allowing access too**port 5601**.</span></span>

### <a name="create-a-kibana-dashboard"></a><span data-ttu-id="0e944-147">Vytvořit řídicí panel Kibana</span><span class="sxs-lookup"><span data-stu-id="0e944-147">Create a Kibana dashboard</span></span>

<span data-ttu-id="0e944-148">V tomto článku uvádíme ukázkový řídicí panel vám tooview trendy a podrobnosti v upozornění.</span><span class="sxs-lookup"><span data-stu-id="0e944-148">For this article, we have provided a sample dashboard for you tooview trends and details in your alerts.</span></span>

![Obrázek 1][1]

1. <span data-ttu-id="0e944-150">Stažení souboru řídicí panel hello [sem](https://aka.ms/networkwatchernsgflowlogdashboard), hello vizualizace souboru [sem](https://aka.ms/networkwatchernsgflowlogvisualizations)a soubor hello uložené hledání [zde](https://aka.ms/networkwatchernsgflowlogsearch).</span><span class="sxs-lookup"><span data-stu-id="0e944-150">Download hello dashboard file [here](https://aka.ms/networkwatchernsgflowlogdashboard), hello visualization file [here](https://aka.ms/networkwatchernsgflowlogvisualizations), and hello saved search file [here](https://aka.ms/networkwatchernsgflowlogsearch).</span></span>

1. <span data-ttu-id="0e944-151">V části hello **správy** kartě z Kibana, přejděte příliš**uložit objekty** a importovat všechny tři soubory.</span><span class="sxs-lookup"><span data-stu-id="0e944-151">Under hello **Management** tab of Kibana, navigate too**Saved Objects** and import all three files.</span></span> <span data-ttu-id="0e944-152">Potom z hello **řídicí panel** karta můžete otevřít a zatížení hello ukázkový řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="0e944-152">Then from hello **Dashboard** tab you can open and load hello sample dashboard.</span></span>

<span data-ttu-id="0e944-153">Můžete také vytvořit vlastní vizualizace a přizpůsobit směrem metriky týkající se vlastní řídicí panely.</span><span class="sxs-lookup"><span data-stu-id="0e944-153">You can also create your own visualizations and dashboards tailored towards metrics of your own interest.</span></span> <span data-ttu-id="0e944-154">Další informace o vytváření vizualizací Kibana z na Kibana [oficiální dokumentaci](https://www.elastic.co/guide/en/kibana/current/visualize.html).</span><span class="sxs-lookup"><span data-stu-id="0e944-154">Read more about creating Kibana visualizations from Kibana's [official documentation](https://www.elastic.co/guide/en/kibana/current/visualize.html).</span></span>

### <a name="visualize-nsg-flow-logs"></a><span data-ttu-id="0e944-155">Vizualizace protokolů NSG toku</span><span class="sxs-lookup"><span data-stu-id="0e944-155">Visualize NSG flow logs</span></span>

<span data-ttu-id="0e944-156">Hello ukázkový řídicí panel poskytuje několik vizualizace hello toku protokolů:</span><span class="sxs-lookup"><span data-stu-id="0e944-156">hello sample dashboard provides several visualizations of hello flow logs:</span></span>

1. <span data-ttu-id="0e944-157">Toky podle rozhodnutí/směr v čase - čas řady grafy znázorňující počet hello toky přes hello časové období.</span><span class="sxs-lookup"><span data-stu-id="0e944-157">Flows by Decision/Direction Over Time - time series graphs showing hello number of flows over hello time period.</span></span> <span data-ttu-id="0e944-158">Můžete upravit hello jednotka času a rozpětí obě tyto vizualizace.</span><span class="sxs-lookup"><span data-stu-id="0e944-158">You can edit hello unit of time and span of both these visualizations.</span></span> <span data-ttu-id="0e944-159">Toky podle poměr hello ukazuje rozhodnutí povolit nebo odepřít rozhodnutí, při toky ve směru ukazuje hello poměr příchozí a odchozí přenosy.</span><span class="sxs-lookup"><span data-stu-id="0e944-159">Flows by Decision shows hello proportion of allow or deny decisions made, while Flows by Direction shows hello proportion of inbound and outbound traffic.</span></span> <span data-ttu-id="0e944-160">S tyto vizuální prvky můžete prozkoumat provoz trendů v čase a vyhledejte všechny špičky nebo neobvyklou vzory.</span><span class="sxs-lookup"><span data-stu-id="0e944-160">With these visuals you can examine traffic trends over time and look for any spikes or unusual patterns.</span></span>

  ![Obrázek 2][2]

1. <span data-ttu-id="0e944-162">Toky podle cílové/zdrojový Port – výsečové grafy znázorňující hello rozpis toků tootheir příslušné porty.</span><span class="sxs-lookup"><span data-stu-id="0e944-162">Flows by Destination/Source Port – pie charts showing hello breakdown of flows tootheir respective ports.</span></span> <span data-ttu-id="0e944-163">K tomuto zobrazení se zobrazí vaše nejčastěji používané porty.</span><span class="sxs-lookup"><span data-stu-id="0e944-163">With this view you can see your most commonly used ports.</span></span> <span data-ttu-id="0e944-164">Pokud kliknete na určitém portu v rámci hello výsečového grafu, bude filtrovat hello zbytek řídicí panel hello dolů tooflows tento port.</span><span class="sxs-lookup"><span data-stu-id="0e944-164">If you click on a specific port within hello pie chart, hello rest of hello dashboard will filter down tooflows of that port.</span></span>

  ![figure3][3]

1. <span data-ttu-id="0e944-166">Počet toky a Nejdřívější čas protokolu – metriky ukazuje zaznamenána hello počet toky a zachytit hello datum hello nejdřívější protokolu.</span><span class="sxs-lookup"><span data-stu-id="0e944-166">Number of Flows and Earliest Log Time – metrics showing you hello number of flows recorded and hello date of hello earliest log captured.</span></span>

  ![figure4][4]

1. <span data-ttu-id="0e944-168">Toky NSG a pravidlo – pruhový graf ukazuje hello distribuční toků v jednotlivých skupinách NSG, jakož i hello distribuce pravidel v jednotlivých skupinách NSG.</span><span class="sxs-lookup"><span data-stu-id="0e944-168">Flows by NSG and Rule – a bar graph showing you hello distribution of flows within each NSG, as well as hello distribution of rules within each NSG.</span></span> <span data-ttu-id="0e944-169">Zde vidíte které skupina NSG a pravidla generované hello většina provozu.</span><span class="sxs-lookup"><span data-stu-id="0e944-169">From here you can see which NSG and rules generated hello most traffic.</span></span>

  ![figure5][5]

1. <span data-ttu-id="0e944-171">Prvních 10 zdrojové nebo cílové IP adresy – pruhové grafy znázorňující hello prvních 10 zdrojové a cílové IP adresy.</span><span class="sxs-lookup"><span data-stu-id="0e944-171">Top 10 Source/Destination IPs – bar charts showing hello top 10 source and destination IPs.</span></span> <span data-ttu-id="0e944-172">Tyto grafy tooshow můžete upravit více nebo méně nejvyšší IP adresy.</span><span class="sxs-lookup"><span data-stu-id="0e944-172">You can adjust these charts tooshow more or less top IPs.</span></span> <span data-ttu-id="0e944-173">Odsud můžete může najdete v části hello nejčastěji výskytu IP adresy, stejně jako hello rozhodnutí provoz (povolit nebo zakázat) prováděné směrem každý IP.</span><span class="sxs-lookup"><span data-stu-id="0e944-173">From here you can see hello most commonly occurring IPs as well as hello traffic decision (allow or deny) being made towards each IP.</span></span>

  ![figure6][6]

1. <span data-ttu-id="0e944-175">Tok řazené kolekce členů – Tato tabulka ukazuje hello informace obsažené v rámci každé toku řazené kolekce členů a také jeho odpovídající hodnot a pravidla.</span><span class="sxs-lookup"><span data-stu-id="0e944-175">Flow Tuples – this table shows you hello information contained within each flow tuple, as well as its corresponding NGS and rule.</span></span>

  ![figure7][7]

<span data-ttu-id="0e944-177">Použití hello dotazu pruhu v horní části hello hello řídicího panelu, můžete filtrovat dolů hello řídicí panel podle libovolný parametr hello toky, jako je například ID předplatného, skupiny prostředků, pravidla nebo jakoukoli jinou proměnnou, které vás zajímají.</span><span class="sxs-lookup"><span data-stu-id="0e944-177">Using hello query bar at hello top of hello dashboard, you can filter down hello dashboard based on any parameter of hello flows, such as subscription ID, resource groups, rule, or any other variable of interest.</span></span> <span data-ttu-id="0e944-178">Další informace o Kibana na dotazy a filtry, najdete v části toohello [oficiální dokumentaci](https://www.elastic.co/guide/en/beats/packetbeat/current/kibana-queries-filters.html)</span><span class="sxs-lookup"><span data-stu-id="0e944-178">For more about Kibana's queries and filters, refer toohello [official documentation](https://www.elastic.co/guide/en/beats/packetbeat/current/kibana-queries-filters.html)</span></span>

## <a name="conclusion"></a><span data-ttu-id="0e944-179">Závěr</span><span class="sxs-lookup"><span data-stu-id="0e944-179">Conclusion</span></span>

<span data-ttu-id="0e944-180">Kombinací hello skupinu zabezpečení sítě toku protokoly s hello elastické zásobníku jsme mít spolu výkonný a přizpůsobit způsob toovisualize naše síťový provoz.</span><span class="sxs-lookup"><span data-stu-id="0e944-180">By combining hello Network Security Group flow logs with hello Elastic Stack, we have come up with powerful and customizable way toovisualize our network traffic.</span></span> <span data-ttu-id="0e944-181">Tyto řídicí panely umožňují získat tooquickly a sdílet uplatnitelné informace o vaší síti a také filtru dolů a prozkoumat na všechny potenciální anomálií.</span><span class="sxs-lookup"><span data-stu-id="0e944-181">These dashboards allow you tooquickly gain and share insights about your network traffic, as well as filter down and investigate on any potential anomalies.</span></span> <span data-ttu-id="0e944-182">Pomocí Kibana, můžete přizpůsobit tyto řídicí panely a vytvořit konkrétní vizualizace toomeet všechny potřeby zabezpečení, auditování a dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="0e944-182">Using Kibana, you can tailor these dashboards and create specific visualizations toomeet any security, audit, and compliance needs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0e944-183">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0e944-183">Next steps</span></span>

<span data-ttu-id="0e944-184">Zjistěte, jak toovisualize vaše skupina NSG toku protokoly s Power BI navštivte stránky [vizualizovat NSG toků protokoly s Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="0e944-184">Learn how toovisualize your NSG flow logs with Power BI by visiting [Visualize NSG flows logs with Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>


<!--Image references-->

[scenario]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/scenario.png
[1]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure1.png
[2]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure2.png
[3]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure3.png
[4]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure4.png
[5]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure5.png
[6]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure6.png
[7]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure7.png
