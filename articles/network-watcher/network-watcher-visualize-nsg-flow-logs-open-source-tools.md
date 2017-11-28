---
title: "Vizualizace protokolů toku NSG sledovací proces sítě Azure pomocí nástroje s otevřeným zdrojem | Microsoft Docs"
description: "Tato stránka popisuje, jak použít open source nástroje k vizualizaci toku protokolů NSG."
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
ms.openlocfilehash: 20f60ccd9108a7473705c2368f28d3152d0dd614
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="visualize-azure-network-watcher-nsg-flow-logs-using-open-source-tools"></a><span data-ttu-id="264e0-103">Vizualizace protokolů toku NSG sledovací proces sítě Azure pomocí nástroje s otevřeným zdrojem</span><span class="sxs-lookup"><span data-stu-id="264e0-103">Visualize Azure Network Watcher NSG flow logs using open source tools</span></span>

<span data-ttu-id="264e0-104">Skupina zabezpečení sítě toku protokoly poskytují informace, které můžete použít pochopit příchozí a odchozí přenosy IP na skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="264e0-104">Network Security Group flow logs provide information that can be used understand ingress and egress IP traffic on Network Security Groups.</span></span> <span data-ttu-id="264e0-105">Tyto protokoly toku zobrazit příchozí a odchozí toky na základě na pravidlo, tok se vztahuje na síťový adaptér, 5 řazené kolekce členů informace o toku (zdrojové nebo cílové IP adresy, zdrojový nebo cílový Port, protokol), a pokud se povoluje nebo odepírá provoz.</span><span class="sxs-lookup"><span data-stu-id="264e0-105">These flow logs show outbound and inbound flows on a per rule basis, the NIC the flow applies to, 5 tuple information about the flow (Source/Destination IP, Source/Destination Port, Protocol), and if the traffic was allowed or denied.</span></span>

<span data-ttu-id="264e0-106">Tyto protokoly toku může být obtížné ručně analyzovat a získáte přehled o z.</span><span class="sxs-lookup"><span data-stu-id="264e0-106">These flow logs can be difficult to manually parse and gain insights from.</span></span> <span data-ttu-id="264e0-107">Existuje však několik nástrojů s otevřeným zdrojem, které vám mohou pomoci vizualizovat tato data.</span><span class="sxs-lookup"><span data-stu-id="264e0-107">However, there are several open source tools that can help visualize this data.</span></span> <span data-ttu-id="264e0-108">Tento článek se poskytují řešení vizualizovat tyto protokoly pomocí elastické zásobníku, která vám umožní rychle indexu a vizualizovat vaše toku přihlášení Kibana řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="264e0-108">This article will provide a solution to visualize these logs using the Elastic Stack, which will allow you to quickly index and visualize your flow logs on a Kibana dashboard.</span></span>

## <a name="scenario"></a><span data-ttu-id="264e0-109">Scénář</span><span class="sxs-lookup"><span data-stu-id="264e0-109">Scenario</span></span>

<span data-ttu-id="264e0-110">V tomto článku nastavíme řešení, které vám umožňuje vizualizovat protokolů toku skupinu zabezpečení sítě pomocí elastické zásobníku.</span><span class="sxs-lookup"><span data-stu-id="264e0-110">In this article, we will set up a solution that will allow you to visualize Network Security Group flow logs using the Elastic Stack.</span></span>  <span data-ttu-id="264e0-111">O Logstash vstupní modul plug-in obdrží protokoly toku přímo z objektu blob storage, který je nakonfigurován pro obsahující protokoly toku.</span><span class="sxs-lookup"><span data-stu-id="264e0-111">A Logstash input plugin will obtain the flow logs directly from the storage blob configured for containing the flow logs.</span></span> <span data-ttu-id="264e0-112">Potom pomocí elastické zásobníku, protokoly toku indexované se použije vytvoření řídicího panelu Kibana k vizualizaci informací.</span><span class="sxs-lookup"><span data-stu-id="264e0-112">Then, using the Elastic Stack, the flow logs will be indexed and used to create a Kibana dashboard to visualize the information.</span></span>

![scénář][scenario]

## <a name="steps"></a><span data-ttu-id="264e0-114">Kroky</span><span class="sxs-lookup"><span data-stu-id="264e0-114">Steps</span></span>

### <a name="enable-network-security-group-flow-logging"></a><span data-ttu-id="264e0-115">Protokolování toku povolit skupinu zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="264e0-115">Enable Network Security Group flow logging</span></span>
<span data-ttu-id="264e0-116">V tomto scénáři musí mít síťové zabezpečení skupiny toku protokolování zapnuta alespoň jednu skupinu zabezpečení sítě ve vašem účtu.</span><span class="sxs-lookup"><span data-stu-id="264e0-116">For this scenario, you must have Network Security Group Flow Logging enabled on at least one Network Security Group in your account.</span></span> <span data-ttu-id="264e0-117">Pokyny k povolení protokolů toku zabezpečení sítě, naleznete v následujícím článku [Úvod do toku protokolování pro skupiny zabezpečení sítě](network-watcher-nsg-flow-logging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="264e0-117">For instructions on enabling Network Security Flow Logs, refer to the following article [Introduction to flow logging for Network Security Groups](network-watcher-nsg-flow-logging-overview.md).</span></span>


### <a name="set-up-the-elastic-stack"></a><span data-ttu-id="264e0-118">Nastavit elastické zásobníku</span><span class="sxs-lookup"><span data-stu-id="264e0-118">Set up the Elastic Stack</span></span>
<span data-ttu-id="264e0-119">Propojením protokolů NSG toku s elastické zásobníku můžeme vytvořit řídicí panel Kibana a co umožňuje vyhledávat, graf, analyzovat a statistiky odvozena z našich protokolů.</span><span class="sxs-lookup"><span data-stu-id="264e0-119">By connecting NSG flow logs with the Elastic Stack, we can create a Kibana dashboard what allows us to search, graph, analyze, and derive insights from our logs.</span></span>

#### <a name="install-elasticsearch"></a><span data-ttu-id="264e0-120">Nainstalujte Elasticsearch</span><span class="sxs-lookup"><span data-stu-id="264e0-120">Install Elasticsearch</span></span>

1. <span data-ttu-id="264e0-121">Elastické zásobníku z verze 5.0 a vyšší vyžaduje Java 8.</span><span class="sxs-lookup"><span data-stu-id="264e0-121">The Elastic Stack from version 5.0 and above requires Java 8.</span></span> <span data-ttu-id="264e0-122">Spusťte příkaz `java -version` zkontrolujte vaši verzi.</span><span class="sxs-lookup"><span data-stu-id="264e0-122">Run the command `java -version` to check your version.</span></span> <span data-ttu-id="264e0-123">Pokud nemáte java nainstalovat, najdete v dokumentaci k na [Oracle na webu](http://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html)</span><span class="sxs-lookup"><span data-stu-id="264e0-123">If you do not have java install, refer to documentation on [Oracle's website](http://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html)</span></span>
1. <span data-ttu-id="264e0-124">Stáhněte si správné binární balíček pro váš systém:</span><span class="sxs-lookup"><span data-stu-id="264e0-124">Download the correct binary package for your system:</span></span>

    ```
    curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.2.0.deb
    sudo dpkg -i elasticsearch-5.2.0.deb
    sudo /etc/init.d/elasticsearch start
    ```

    <span data-ttu-id="264e0-125">Ostatní metody instalace najdete na [Elasticsearch instalace](https://www.elastic.co/guide/en/beats/libbeat/5.2/elasticsearch-installation.html)</span><span class="sxs-lookup"><span data-stu-id="264e0-125">Other installation methods can be found at [Elasticsearch Installation](https://www.elastic.co/guide/en/beats/libbeat/5.2/elasticsearch-installation.html)</span></span>

1. <span data-ttu-id="264e0-126">Ověřte, zda je spuštěna Elasticsearch pomocí příkazu:</span><span class="sxs-lookup"><span data-stu-id="264e0-126">Verify that Elasticsearch is running with the command:</span></span>

    ```
    curl http://127.0.0.1:9200
    ```

    <span data-ttu-id="264e0-127">Byste měli vidět odpověď podobná této:</span><span class="sxs-lookup"><span data-stu-id="264e0-127">You should see a response similar to this:</span></span>

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

<span data-ttu-id="264e0-128">Další pokyny k instalaci elastické vyhledávání, naleznete na stránce [instalace](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/_installation.html)</span><span class="sxs-lookup"><span data-stu-id="264e0-128">For further instructions on installing Elastic search, refer to the page [Installation](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/_installation.html)</span></span>

### <a name="install-logstash"></a><span data-ttu-id="264e0-129">Nainstalujte Logstash</span><span class="sxs-lookup"><span data-stu-id="264e0-129">Install Logstash</span></span>

1. <span data-ttu-id="264e0-130">Chcete-li nainstalovat Logstash spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="264e0-130">To install Logstash run the following commands:</span></span>

    ```
    curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.deb
    sudo dpkg -i logstash-5.2.0.deb
    ```
1. <span data-ttu-id="264e0-131">Další budeme muset nakonfigurovat Logstash přístup a analyzovat protokoly toku.</span><span class="sxs-lookup"><span data-stu-id="264e0-131">Next we need to configure Logstash to access and parse the flow logs.</span></span> <span data-ttu-id="264e0-132">Vytvoření souboru logstash.conf pomocí:</span><span class="sxs-lookup"><span data-stu-id="264e0-132">Create a logstash.conf file using:</span></span>

    ```
    sudo touch /etc/logstash/conf.d/logstash.conf
    ```

1. <span data-ttu-id="264e0-133">Do souboru přidejte následující obsah:</span><span class="sxs-lookup"><span data-stu-id="264e0-133">Add the following content to the file:</span></span>

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

<span data-ttu-id="264e0-134">Další pokyny k instalaci Logstash naleznete [oficiální dokumentaci](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)</span><span class="sxs-lookup"><span data-stu-id="264e0-134">For further instructions on installing Logstash, refer to the [official documentation](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)</span></span>

### <a name="install-the-logstash-input-plugin-for-azure-blob-storage"></a><span data-ttu-id="264e0-135">Nainstalujte modul plug-in vstupní Logstash pro úložiště objektů blob v Azure</span><span class="sxs-lookup"><span data-stu-id="264e0-135">Install the Logstash input plugin for Azure blob storage</span></span>

<span data-ttu-id="264e0-136">Tento modul plug-in Logstash vám umožní přímý přístup k toku protokoly ze svého účtu úložiště určený.</span><span class="sxs-lookup"><span data-stu-id="264e0-136">This Logstash plugin will allow you to directly access the flow logs from their designated storage account.</span></span> <span data-ttu-id="264e0-137">Chcete-li nainstalovat tento modul plug-in, spusťte příkaz z výchozí Logstash instalační adresář (v této případu /usr/share/logstash/bin):</span><span class="sxs-lookup"><span data-stu-id="264e0-137">To install this plugin, from the default Logstash installation directory (in this case /usr/share/logstash/bin) run the command:</span></span>

```
logstash-plugin install logstash-input-azureblob
```

<span data-ttu-id="264e0-138">Chcete-li spustit Logstash spusťte příkaz:</span><span class="sxs-lookup"><span data-stu-id="264e0-138">To start Logstash run the command:</span></span>

```
sudo /etc/init.d/logstash start
```

<span data-ttu-id="264e0-139">Další informace o tento modul plug-in, najdete v dokumentaci k [sem](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob)</span><span class="sxs-lookup"><span data-stu-id="264e0-139">For more information about this plugin, refer to documentation [here](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob)</span></span>

### <a name="install-kibana"></a><span data-ttu-id="264e0-140">Nainstalujte Kibana</span><span class="sxs-lookup"><span data-stu-id="264e0-140">Install Kibana</span></span>

1. <span data-ttu-id="264e0-141">Spusťte následující příkazy pro instalaci Kibana:</span><span class="sxs-lookup"><span data-stu-id="264e0-141">Run the following commands to install Kibana:</span></span>

  ```
  curl -L -O https://artifacts.elastic.co/downloads/kibana/kibana-5.2.0-linux-x86_64.tar.gz
  tar xzvf kibana-5.2.0-linux-x86_64.tar.gz
  ```

1. <span data-ttu-id="264e0-142">Chcete-li spustit Kibana použijte příkazy:</span><span class="sxs-lookup"><span data-stu-id="264e0-142">To run Kibana use the commands:</span></span>

  ```
  cd kibana-5.2.0-linux-x86_64/
  ./bin/kibana
  ```

1. <span data-ttu-id="264e0-143">Chcete-li zobrazit vaše Kibana webové rozhraní, přejděte na`http://localhost:5601`</span><span class="sxs-lookup"><span data-stu-id="264e0-143">To view your Kibana web interface, navigate to `http://localhost:5601`</span></span>
1. <span data-ttu-id="264e0-144">V tomto scénáři je vzor indexu používá pro protokoly toku "protokolů nsg toku".</span><span class="sxs-lookup"><span data-stu-id="264e0-144">For this scenario, the index pattern used for the flow logs is "nsg-flow-logs".</span></span> <span data-ttu-id="264e0-145">Můžete změnit vzor indexu v části "výstupní" logstash.conf souboru.</span><span class="sxs-lookup"><span data-stu-id="264e0-145">You may change the index pattern in the "output" section of your logstash.conf file.</span></span>

1. <span data-ttu-id="264e0-146">Pokud chcete zobrazit řídicí panel Kibana vzdáleně, vytvoření příchozího pravidla NSG povolení přístupu k **portu 5601**.</span><span class="sxs-lookup"><span data-stu-id="264e0-146">If you want to view the Kibana dashboard remotely, create an inbound NSG rule allowing access to **port 5601**.</span></span>

### <a name="create-a-kibana-dashboard"></a><span data-ttu-id="264e0-147">Vytvořit řídicí panel Kibana</span><span class="sxs-lookup"><span data-stu-id="264e0-147">Create a Kibana dashboard</span></span>

<span data-ttu-id="264e0-148">V tomto článku uvádíme na ukázkový řídicí panel můžete prohlédnout podrobnosti a trendy v upozornění.</span><span class="sxs-lookup"><span data-stu-id="264e0-148">For this article, we have provided a sample dashboard for you to view trends and details in your alerts.</span></span>

![Obrázek 1][1]

1. <span data-ttu-id="264e0-150">Stáhněte si soubor řídicí panel [sem](https://aka.ms/networkwatchernsgflowlogdashboard), soubor vizualizace [sem](https://aka.ms/networkwatchernsgflowlogvisualizations)a soubor uloženého hledání [zde](https://aka.ms/networkwatchernsgflowlogsearch).</span><span class="sxs-lookup"><span data-stu-id="264e0-150">Download the dashboard file [here](https://aka.ms/networkwatchernsgflowlogdashboard), the visualization file [here](https://aka.ms/networkwatchernsgflowlogvisualizations), and the saved search file [here](https://aka.ms/networkwatchernsgflowlogsearch).</span></span>

1. <span data-ttu-id="264e0-151">V části **správy** kartě z Kibana, přejděte na **uložit objekty** a importovat všechny tři soubory.</span><span class="sxs-lookup"><span data-stu-id="264e0-151">Under the **Management** tab of Kibana, navigate to **Saved Objects** and import all three files.</span></span> <span data-ttu-id="264e0-152">Potom z **řídicí panel** karta můžete otevřít a načíst ukázkový řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="264e0-152">Then from the **Dashboard** tab you can open and load the sample dashboard.</span></span>

<span data-ttu-id="264e0-153">Můžete také vytvořit vlastní vizualizace a přizpůsobit směrem metriky týkající se vlastní řídicí panely.</span><span class="sxs-lookup"><span data-stu-id="264e0-153">You can also create your own visualizations and dashboards tailored towards metrics of your own interest.</span></span> <span data-ttu-id="264e0-154">Další informace o vytváření vizualizací Kibana z na Kibana [oficiální dokumentaci](https://www.elastic.co/guide/en/kibana/current/visualize.html).</span><span class="sxs-lookup"><span data-stu-id="264e0-154">Read more about creating Kibana visualizations from Kibana's [official documentation](https://www.elastic.co/guide/en/kibana/current/visualize.html).</span></span>

### <a name="visualize-nsg-flow-logs"></a><span data-ttu-id="264e0-155">Vizualizace protokolů NSG toku</span><span class="sxs-lookup"><span data-stu-id="264e0-155">Visualize NSG flow logs</span></span>

<span data-ttu-id="264e0-156">Ukázkový řídicí panel poskytuje několik vizualizace toku protokolů:</span><span class="sxs-lookup"><span data-stu-id="264e0-156">The sample dashboard provides several visualizations of the flow logs:</span></span>

1. <span data-ttu-id="264e0-157">Toky podle rozhodnutí/směr v čase - čas řady grafy znázorňující počet toků za časové období.</span><span class="sxs-lookup"><span data-stu-id="264e0-157">Flows by Decision/Direction Over Time - time series graphs showing the number of flows over the time period.</span></span> <span data-ttu-id="264e0-158">Můžete upravit jednotku času a rozpětí obě tyto vizualizace.</span><span class="sxs-lookup"><span data-stu-id="264e0-158">You can edit the unit of time and span of both these visualizations.</span></span> <span data-ttu-id="264e0-159">Toky rozhodnutím zobrazuje podíl povolit nebo odepřít rozhodnutí, zatímco toky ve směru zobrazuje podíl příchozí a odchozí přenosy.</span><span class="sxs-lookup"><span data-stu-id="264e0-159">Flows by Decision shows the proportion of allow or deny decisions made, while Flows by Direction shows the proportion of inbound and outbound traffic.</span></span> <span data-ttu-id="264e0-160">S tyto vizuální prvky můžete prozkoumat provoz trendů v čase a vyhledejte všechny špičky nebo neobvyklou vzory.</span><span class="sxs-lookup"><span data-stu-id="264e0-160">With these visuals you can examine traffic trends over time and look for any spikes or unusual patterns.</span></span>

  ![Obrázek 2][2]

1. <span data-ttu-id="264e0-162">Toky podle cílové/zdrojový Port – výsečové grafy znázorňující rozpis toků k příslušným portům.</span><span class="sxs-lookup"><span data-stu-id="264e0-162">Flows by Destination/Source Port – pie charts showing the breakdown of flows to their respective ports.</span></span> <span data-ttu-id="264e0-163">K tomuto zobrazení se zobrazí vaše nejčastěji používané porty.</span><span class="sxs-lookup"><span data-stu-id="264e0-163">With this view you can see your most commonly used ports.</span></span> <span data-ttu-id="264e0-164">Pokud kliknete na určitém portu v rámci výsečového grafu, zbývající část řídicího panelu vyfiltruje dolů toky tento port.</span><span class="sxs-lookup"><span data-stu-id="264e0-164">If you click on a specific port within the pie chart, the rest of the dashboard will filter down to flows of that port.</span></span>

  ![figure3][3]

1. <span data-ttu-id="264e0-166">Počet toky a Nejdřívější čas protokolu – metriky ukazuje toků zaznamenaných a datum nejdřívější protokolu zaznamenat.</span><span class="sxs-lookup"><span data-stu-id="264e0-166">Number of Flows and Earliest Log Time – metrics showing you the number of flows recorded and the date of the earliest log captured.</span></span>

  ![figure4][4]

1. <span data-ttu-id="264e0-168">Toky NSG a pravidlo – pruhový graf ukazuje rozdělení toků v jednotlivých skupinách NSG a také distribuce pravidel v jednotlivých skupinách NSG.</span><span class="sxs-lookup"><span data-stu-id="264e0-168">Flows by NSG and Rule – a bar graph showing you the distribution of flows within each NSG, as well as the distribution of rules within each NSG.</span></span> <span data-ttu-id="264e0-169">Zde se zobrazí, které skupina NSG a pravidla generované nejvíce provoz.</span><span class="sxs-lookup"><span data-stu-id="264e0-169">From here you can see which NSG and rules generated the most traffic.</span></span>

  ![figure5][5]

1. <span data-ttu-id="264e0-171">TOP 10 zdrojové nebo cílové IP adresy – pruhové grafy znázorňující prvních 10 zdrojové a cílové IP adresy.</span><span class="sxs-lookup"><span data-stu-id="264e0-171">Top 10 Source/Destination IPs – bar charts showing the top 10 source and destination IPs.</span></span> <span data-ttu-id="264e0-172">Můžete upravit tyto grafy zobrazíte více nebo méně nejvyšší IP adresy.</span><span class="sxs-lookup"><span data-stu-id="264e0-172">You can adjust these charts to show more or less top IPs.</span></span> <span data-ttu-id="264e0-173">Zde vidíte nejčastěji výskytu IP adresy, jakož i provoz rozhodnutí (povolit nebo zakázat) prováděné směrem každý IP.</span><span class="sxs-lookup"><span data-stu-id="264e0-173">From here you can see the most commonly occurring IPs as well as the traffic decision (allow or deny) being made towards each IP.</span></span>

  ![figure6][6]

1. <span data-ttu-id="264e0-175">Tok řazené kolekce členů – Tato tabulka ukazuje, informace obsažené v rámci každé toku řazené kolekce členů a také odpovídající hodnot a pravidla.</span><span class="sxs-lookup"><span data-stu-id="264e0-175">Flow Tuples – this table shows you the information contained within each flow tuple, as well as its corresponding NGS and rule.</span></span>

  ![figure7][7]

<span data-ttu-id="264e0-177">Na panelu dotazů v horní části řídicího panelu, můžete filtrovat pomocí dolů řídicím panelu založené na libovolný parametr toky, jako je například ID předplatného, skupiny prostředků, pravidla nebo jakoukoli jinou proměnnou, které vás zajímají.</span><span class="sxs-lookup"><span data-stu-id="264e0-177">Using the query bar at the top of the dashboard, you can filter down the dashboard based on any parameter of the flows, such as subscription ID, resource groups, rule, or any other variable of interest.</span></span> <span data-ttu-id="264e0-178">Další informace o Kibana na dotazy a filtry, naleznete [oficiální dokumentaci](https://www.elastic.co/guide/en/beats/packetbeat/current/kibana-queries-filters.html)</span><span class="sxs-lookup"><span data-stu-id="264e0-178">For more about Kibana's queries and filters, refer to the [official documentation](https://www.elastic.co/guide/en/beats/packetbeat/current/kibana-queries-filters.html)</span></span>

## <a name="conclusion"></a><span data-ttu-id="264e0-179">Závěr</span><span class="sxs-lookup"><span data-stu-id="264e0-179">Conclusion</span></span>

<span data-ttu-id="264e0-180">Kombinací protokolů toku skupinu zabezpečení sítě se elastické zásobníkem budeme mít spolu výkonný a přizpůsobit způsob vizualizace naše síťový provoz.</span><span class="sxs-lookup"><span data-stu-id="264e0-180">By combining the Network Security Group flow logs with the Elastic Stack, we have come up with powerful and customizable way to visualize our network traffic.</span></span> <span data-ttu-id="264e0-181">Tyto řídicí panely vám umožňují rychle získat a sdílet uplatnitelné informace o vaší síti a také filtru dolů a prozkoumat na všechny potenciální anomálií.</span><span class="sxs-lookup"><span data-stu-id="264e0-181">These dashboards allow you to quickly gain and share insights about your network traffic, as well as filter down and investigate on any potential anomalies.</span></span> <span data-ttu-id="264e0-182">Kibana můžete přizpůsobit tyto řídicí panely a vytváření konkrétní vizualizací, které splňují všechny potřeby zabezpečení, auditování a dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="264e0-182">Using Kibana, you can tailor these dashboards and create specific visualizations to meet any security, audit, and compliance needs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="264e0-183">Další kroky</span><span class="sxs-lookup"><span data-stu-id="264e0-183">Next steps</span></span>

<span data-ttu-id="264e0-184">Zjistěte, jak toku protokolů NSG s Power BI vizualizovat navštivte stránky [vizualizovat NSG toků protokoly s Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="264e0-184">Learn how to visualize your NSG flow logs with Power BI by visiting [Visualize NSG flows logs with Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>


<!--Image references-->

[scenario]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/scenario.png
[1]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure1.png
[2]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure2.png
[3]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure3.png
[4]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure4.png
[5]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure5.png
[6]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure6.png
[7]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure7.png
