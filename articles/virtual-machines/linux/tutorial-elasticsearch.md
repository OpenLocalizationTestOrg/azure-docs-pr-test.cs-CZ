---
title: "Nasazení ElasticSearch na vývoj pro virtuální počítač v Azure"
description: "Kurz – instalace elastické zásobníku na vývojovém virtuálního počítače s Linuxem v Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: rloutlaw
manager: justhe
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 10/11/2017
ms.author: routlaw
ms.openlocfilehash: 5b0b51504478cc0d501a89760ccd60808a69ccbd
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="install-the-elastic-stack-on-an-azure-vm"></a><span data-ttu-id="b7e09-103">Azure virtuálního počítače nainstalujte elastické zásobníku</span><span class="sxs-lookup"><span data-stu-id="b7e09-103">Install the Elastic Stack on an Azure VM</span></span>

<span data-ttu-id="b7e09-104">Tento článek vás provede procesem nasazení [Elasticsearch](https://www.elastic.co/products/elasticsearch), [Logstash](https://www.elastic.co/products/logstash), a [Kibana](https://www.elastic.co/products/kibana), na virtuálního počítače s Ubuntu v Azure.</span><span class="sxs-lookup"><span data-stu-id="b7e09-104">This article walks you through how to deploy [Elasticsearch](https://www.elastic.co/products/elasticsearch), [Logstash](https://www.elastic.co/products/logstash), and [Kibana](https://www.elastic.co/products/kibana), on an Ubuntu VM in Azure.</span></span> <span data-ttu-id="b7e09-105">Pokud chcete zobrazit elastické zásobníku v akci, můžete volitelně připojit k Kibana a pracovat s ukázkami protokolování dat.</span><span class="sxs-lookup"><span data-stu-id="b7e09-105">To see the Elastic Stack in action, you can optionally connect to Kibana  and work with some sample logging data.</span></span> 

<span data-ttu-id="b7e09-106">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="b7e09-106">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b7e09-107">Vytvoření virtuálního počítače s Ubuntu ve skupině prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="b7e09-107">Create an Ubuntu VM in an Azure resource group</span></span>
> * <span data-ttu-id="b7e09-108">Do virtuálního počítače nainstalujte Elasticsearch, Logstash a Kibana</span><span class="sxs-lookup"><span data-stu-id="b7e09-108">Install Elasticsearch, Logstash, and Kibana on the VM</span></span>
> * <span data-ttu-id="b7e09-109">Poslat Elasticsearch s Logstash ukázková data</span><span class="sxs-lookup"><span data-stu-id="b7e09-109">Send sample data to Elasticsearch with Logstash</span></span> 
> * <span data-ttu-id="b7e09-110">Otevřete porty a pracovat s daty v konzole Kibana</span><span class="sxs-lookup"><span data-stu-id="b7e09-110">Open ports and work with data in the Kibana console</span></span>


 <span data-ttu-id="b7e09-111">Toto nasazení je vhodná pro základní vývoj s elastické zásobníku.</span><span class="sxs-lookup"><span data-stu-id="b7e09-111">This deployment is suitable for basic development with the Elastic Stack.</span></span> <span data-ttu-id="b7e09-112">Další informace o elastické zásobníku, včetně doporučení pro produkční prostředí, najdete v článku [elastické dokumentaci](https://www.elastic.co/guide/index.html) a [Azure architektura Center](/azure/architecture/elasticsearch/).</span><span class="sxs-lookup"><span data-stu-id="b7e09-112">For more on the Elastic Stack, including recommendations for a production environment, see the [Elastic documentation](https://www.elastic.co/guide/index.html) and the [Azure Architecture Center](/azure/architecture/elasticsearch/).</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="b7e09-113">Pokud se rozhodnete nainstalovat a místně používat rozhraní příkazového řádku, musíte mít Azure CLI verze 2.0.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="b7e09-113">If you choose to install and use the CLI locally, this tutorial requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="b7e09-114">Verzi zjistíte spuštěním příkazu `az --version`.</span><span class="sxs-lookup"><span data-stu-id="b7e09-114">Run `az --version` to find the version.</span></span> <span data-ttu-id="b7e09-115">Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="b7e09-115">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-a-resource-group"></a><span data-ttu-id="b7e09-116">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="b7e09-116">Create a resource group</span></span>

<span data-ttu-id="b7e09-117">Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="b7e09-117">Create a resource group with the [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="b7e09-118">Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="b7e09-118">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="b7e09-119">Následující příklad vytvoří skupinu prostředků *myResourceGroup* v umístění *eastus*.</span><span class="sxs-lookup"><span data-stu-id="b7e09-119">The following example creates a resource group named *myResourceGroup* in the *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a><span data-ttu-id="b7e09-120">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="b7e09-120">Create a virtual machine</span></span>

<span data-ttu-id="b7e09-121">Vytvořte virtuální počítač pomocí příkazu [az vm create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="b7e09-121">Create a VM with the [az vm create](/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="b7e09-122">Následující příklad vytvoří virtuální počítač *myVM*, a pokud ve výchozím umístění klíčů ještě neexistují klíče SSH, vytvoří je.</span><span class="sxs-lookup"><span data-stu-id="b7e09-122">The following example creates a VM named *myVM* and creates SSH keys if they do not already exist in a default key location.</span></span> <span data-ttu-id="b7e09-123">Chcete-li použít konkrétní sadu klíčů, použijte možnost `--ssh-key-value`.</span><span class="sxs-lookup"><span data-stu-id="b7e09-123">To use a specific set of keys, use the `--ssh-key-value` option.</span></span>  

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="b7e09-124">Po vytvoření virtuálního počítače se v Azure CLI zobrazí podobné informace jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="b7e09-124">When the VM has been created, the Azure CLI shows information similar to the following example.</span></span> <span data-ttu-id="b7e09-125">Poznamenejte si `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="b7e09-125">Take note of the `publicIpAddress`.</span></span> <span data-ttu-id="b7e09-126">Tato adresa se používá pro přístup k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="b7e09-126">This address is used to access the VM.</span></span>

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="ssh-into-your-vm"></a><span data-ttu-id="b7e09-127">Připojení SSH k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="b7e09-127">SSH into your VM</span></span>

<span data-ttu-id="b7e09-128">Pokud si nejste jisti již veřejnou IP adresu vašeho virtuálního počítače, spusťte [seznam veřejné ip sítě az](/cli/azure/network/public-ip#list) příkaz:</span><span class="sxs-lookup"><span data-stu-id="b7e09-128">If you don't already know the public IP address of your VM, run the [az network public-ip list](/cli/azure/network/public-ip#list) command:</span></span>

```azurecli-interactive
az network public-ip list --resource-group myResourceGroup --query [].ipAddress
```

<span data-ttu-id="b7e09-129">Pomocí následujícího příkazu vytvořte s virtuálním počítačem relaci SSH.</span><span class="sxs-lookup"><span data-stu-id="b7e09-129">Use the following command to create an SSH session with the virtual machine.</span></span> <span data-ttu-id="b7e09-130">Nahraďte správné veřejnou IP adresu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b7e09-130">Substitute the correct public IP address of your virtual machine.</span></span> <span data-ttu-id="b7e09-131">V tomto příkladu IP adresa je *40.68.254.142*.</span><span class="sxs-lookup"><span data-stu-id="b7e09-131">In this example, the IP address is *40.68.254.142*.</span></span>

```bash
ssh azureuser@40.68.254.142
```

## <a name="install-the-elastic-stack"></a><span data-ttu-id="b7e09-132">Nainstalujte elastické zásobníku</span><span class="sxs-lookup"><span data-stu-id="b7e09-132">Install the Elastic Stack</span></span>

<span data-ttu-id="b7e09-133">Import podpisový klíč Elasticsearch a aktualizace vašeho seznamu VÝSTIŽNÝ zdroje zahrnout úložiště elastické balíčků:</span><span class="sxs-lookup"><span data-stu-id="b7e09-133">Import the Elasticsearch signing key and update your APT sources list to include the Elastic package repository:</span></span>

```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
echo "deb https://artifacts.elastic.co/packages/5.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-5.x.list
```

<span data-ttu-id="b7e09-134">Nainstalujte na virtuální počítač virtuální Java a nakonfigurujte JAVA_HOME Tato proměnná je nezbytné pro elastické zásobníku součásti, které chcete spustit.</span><span class="sxs-lookup"><span data-stu-id="b7e09-134">Install the Java Virtual on the VM and configure the JAVA_HOME variable-this is necessary for the Elastic Stack components to run.</span></span>

```bash
sudo apt update && sudo apt install openjdk-8-jre-headless
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
```

<span data-ttu-id="b7e09-135">Spusťte následující příkazy k aktualizaci zdroje balíčků Ubuntu a nainstalujte Elasticsearch, Kibana a Logstash.</span><span class="sxs-lookup"><span data-stu-id="b7e09-135">Run the following commands to update Ubuntu package sources and install Elasticsearch, Kibana, and Logstash.</span></span>

```bash
sudo apt update && sudo apt install elasticsearch kibana logstash   
```

> [!NOTE]
> <span data-ttu-id="b7e09-136">Podrobné pokyny k instalaci, včetně rozložení adresáře a počáteční konfigurace, jsou zachována ve [Elastická na dokumentaci](https://www.elastic.co/guide/en/elastic-stack/current/installing-elastic-stack.html)</span><span class="sxs-lookup"><span data-stu-id="b7e09-136">Detailed installation instructions, including directory layouts and initial configuration, are maintained in [Elastic's documentation](https://www.elastic.co/guide/en/elastic-stack/current/installing-elastic-stack.html)</span></span>

## <a name="start-elasticsearch"></a><span data-ttu-id="b7e09-137">Spustit Elasticsearch</span><span class="sxs-lookup"><span data-stu-id="b7e09-137">Start Elasticsearch</span></span> 

<span data-ttu-id="b7e09-138">Spusťte Elasticsearch na vašem virtuálním počítači pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="b7e09-138">Start Elasticsearch on your VM with the following command:</span></span>

```bash
sudo systemctl start elasticsearch.service
```

<span data-ttu-id="b7e09-139">Tento příkaz neprodukuje žádný výstup, tak ověřte, zda je na virtuálním počítači s tímto spuštěna Elasticsearch `curl` příkaz:</span><span class="sxs-lookup"><span data-stu-id="b7e09-139">This command produces no output, so verify that Elasticsearch is running on the VM with this `curl` command:</span></span>

```bash
curl -XGET 'localhost:9200/'
```

<span data-ttu-id="b7e09-140">Pokud Elasticsearch běží, zobrazí se výstup takto:</span><span class="sxs-lookup"><span data-stu-id="b7e09-140">If Elasticsearch is running, you see output like the following:</span></span>

```json
{
  "name" : "w6Z4NwR",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "SDzCajBoSK2EkXmHvJVaDQ",
  "version" : {
    "number" : "5.6.3",
    "build_hash" : "1a2f265",
    "build_date" : "2017-10-06T20:33:39.012Z",
    "build_snapshot" : false,
    "lucene_version" : "6.6.1"
  },
  "tagline" : "You Know, for Search"
}
```

## <a name="start-logstash-and-add-data-to-elasticsearch"></a><span data-ttu-id="b7e09-141">Spustit Logstash a přidejte data do Elasticsearch</span><span class="sxs-lookup"><span data-stu-id="b7e09-141">Start Logstash and add data to Elasticsearch</span></span>

<span data-ttu-id="b7e09-142">Spusťte Logstash pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="b7e09-142">Start Logstash with the following command:</span></span>

```bash 
sudo systemctl start logstash.service
```

<span data-ttu-id="b7e09-143">Testovací Logstash v interaktivním režimu a ujistěte se, že funguje správně:</span><span class="sxs-lookup"><span data-stu-id="b7e09-143">Test Logstash in interactive mode to make sure it's working correctly:</span></span>

```bash
sudo /usr/share/logstash/bin/logstash -e 'input { stdin { } } output { stdout {} }'
```

<span data-ttu-id="b7e09-144">Toto je základní logstash [kanálu](https://www.elastic.co/guide/en/logstash/5.6/pipeline.html) , vrátí standardní vstup standardním výstupu.</span><span class="sxs-lookup"><span data-stu-id="b7e09-144">This is a basic logstash [pipeline](https://www.elastic.co/guide/en/logstash/5.6/pipeline.html) that echoes standard input to standard output.</span></span> 

```output
The stdin plugin is now waiting for input:
hello azure
2017-10-11T20:01:08.904Z myVM hello azure
```

<span data-ttu-id="b7e09-145">Nastavte Logstash předávání zprávy jádra z tohoto virtuálního počítače do Elasticsearch.</span><span class="sxs-lookup"><span data-stu-id="b7e09-145">Set up Logstash to forward the kernel messages from this VM to Elasticsearch.</span></span> <span data-ttu-id="b7e09-146">Vytvořte nový soubor v prázdný adresář s názvem `vm-syslog-logstash.conf` a vložte následující konfigurace Logstash:</span><span class="sxs-lookup"><span data-stu-id="b7e09-146">Create a new file in an empty directory called `vm-syslog-logstash.conf` and paste in the following Logstash configuration:</span></span>

```Logstash
input {
    stdin {
        type => "stdin-type"
    }

    file {
        type => "syslog"
        path => [ "/var/log/*.log", "/var/log/*/*.log", "/var/log/messages", "/var/log/syslog" ]
        start_position => "beginning"
    }
}

output {

    stdout {
        codec => rubydebug
    }
    elasticsearch {
        hosts  => "localhost:9200"
    }
}
```

<span data-ttu-id="b7e09-147">Testování této konfigurace a jak odesílat syslog data Elasticsearch:</span><span class="sxs-lookup"><span data-stu-id="b7e09-147">Test this configuration and send the syslog data to Elasticsearch:</span></span>

```bash
sudo /usr/share/logstash/bin/logstash -f vm-syslog-logstash.conf
```

<span data-ttu-id="b7e09-148">Zobrazí položky syslogu v terminálu opakována při odesílání do Elasticsearch.</span><span class="sxs-lookup"><span data-stu-id="b7e09-148">You see the syslog entries in your terminal echoed as they are sent to Elasticsearch.</span></span> <span data-ttu-id="b7e09-149">Použití `CTRL+C` ukončíte mimo Logstash po odeslali některá data.</span><span class="sxs-lookup"><span data-stu-id="b7e09-149">Use `CTRL+C` to exit out of Logstash once you've sent some data.</span></span>

## <a name="start-kibana-and-visualize-the-data-in-elasticsearch"></a><span data-ttu-id="b7e09-150">Spusťte Kibana a znázorňovat data ve Elasticsearch</span><span class="sxs-lookup"><span data-stu-id="b7e09-150">Start Kibana and visualize the data in Elasticsearch</span></span>

<span data-ttu-id="b7e09-151">Upravit `/etc/kibana/kibana.yml` a změňte IP adresu Kibana naslouchá na, takže je přístupný z webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="b7e09-151">Edit `/etc/kibana/kibana.yml` and change the IP address Kibana listens on so you can access it from your web browser.</span></span>

```bash
server.host:"0.0.0.0"
```

<span data-ttu-id="b7e09-152">Spusťte Kibana pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="b7e09-152">Start Kibana with the following command:</span></span>

```bash
sudo systemctl start kibana.service
```

<span data-ttu-id="b7e09-153">Otevřete port 5601 z příkazového řádku Azure a povolil vzdálený přístup ke konzole Kibana:</span><span class="sxs-lookup"><span data-stu-id="b7e09-153">Open port 5601 from the Azure CLI to allow remote access to the Kibana console:</span></span>

```azurecli-interactive
az vm open-port --port 5601 --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="b7e09-154">Otevře konzolu Kibana a vyberte **vytvořit** vygenerujte výchozí index na základě dat syslog jste poslali do Elasticsearch dříve.</span><span class="sxs-lookup"><span data-stu-id="b7e09-154">Open up the Kibana console and select **Create** to generate a default index based on the syslog data you sent to Elasticsearch earlier.</span></span> 

![Procházet události procesu Syslog v Kibana](media/elasticsearch-install/kibana-index.png)

<span data-ttu-id="b7e09-156">Vyberte **Discover** v konzole Kibana pro vyhledávání, procházení a filtrovat prostřednictvím události procesu syslog.</span><span class="sxs-lookup"><span data-stu-id="b7e09-156">Select **Discover** on the Kibana console to search, browse, and filter through the syslog events.</span></span>

![Procházet události procesu Syslog v Kibana](media/elasticsearch-install/kibana-search-filter.png)

## <a name="next-steps"></a><span data-ttu-id="b7e09-158">Další postup</span><span class="sxs-lookup"><span data-stu-id="b7e09-158">Next steps</span></span>

<span data-ttu-id="b7e09-159">V tomto kurzu jste nasadili elastické zásobníku do vývoj virtuálního počítače v Azure.</span><span class="sxs-lookup"><span data-stu-id="b7e09-159">In this tutorial, you deployed the Elastic Stack into a development VM in Azure.</span></span> <span data-ttu-id="b7e09-160">Naučili jste se tyto postupy:</span><span class="sxs-lookup"><span data-stu-id="b7e09-160">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b7e09-161">Vytvoření virtuálního počítače s Ubuntu ve skupině prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="b7e09-161">Create an Ubuntu VM in an Azure resource group</span></span>
> * <span data-ttu-id="b7e09-162">Do virtuálního počítače nainstalujte Elasticsearch, Logstash a Kibana</span><span class="sxs-lookup"><span data-stu-id="b7e09-162">Install Elasticsearch, Logstash, and Kibana on the VM</span></span>
> * <span data-ttu-id="b7e09-163">Ukázková data poslat Elasticsearch z Logstash</span><span class="sxs-lookup"><span data-stu-id="b7e09-163">Send sample data to Elasticsearch from Logstash</span></span> 
> * <span data-ttu-id="b7e09-164">Otevřete porty a pracovat s daty v konzole Kibana</span><span class="sxs-lookup"><span data-stu-id="b7e09-164">Open ports and work with data in the Kibana console</span></span>