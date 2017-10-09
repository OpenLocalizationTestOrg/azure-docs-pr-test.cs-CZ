---
title: "aaaExtend prostředí HDInsight pomocí virtuální sítě - Azure | Microsoft Docs"
description: "Zjistěte, jak toouse Azure Virtual Network tooconnect HDInsight tooother cloudové prostředky nebo prostředky ve vašem datovém centru"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 37b9b600-d7f8-4cb1-a04a-0b3a827c6dcc
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/23/2017
ms.author: larryfr
ms.openlocfilehash: ba80be4d9f280c6c62fa8acc996ef5f921acdbbd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="extend-azure-hdinsight-using-an-azure-virtual-network"></a>Rozšíření Azure HDInsight pomocí virtuální síť Azure

Zjistěte, jak toouse HDInsight s [Azure Virtual Network](../virtual-network/virtual-networks-overview.md). Použití virtuální sítě Azure umožňuje hello následující scénáře:

* Připojení tooHDInsight přímo z místní sítě.

* Připojení HDInsight toodata ukládá v Azure virtuální sítě.

* Přímo hello přístupem služby Hadoop, které nejsou k dispozici veřejně přes internet. Například Kafka rozhraní API nebo hello HBase Java API.

> [!WARNING]
> Hello informace v tomto dokumentu vyžaduje znalosti o protokolu TCP/IP v síti. Pokud nejste obeznámeni s prací v síti TCP/IP, by měla spolupracovat s uživatelem, který je před provedením změny tooproduction sítě.

## <a name="planning"></a>Plánování

Hello následují hello otázky, které je nutné zodpovědět při plánování tooinstall HDInsight ve virtuální síti:

* Potřebujete tooinstall HDInsight do existující virtuální síť? Nebo můžete vytvořit novou síť?

    Pokud používáte stávající virtuální síť, musíte konfigurace sítě hello toomodify před instalací HDInsight. Další informace najdete v tématu hello [přidat HDInsight tooan existující virtuální síť](#existingvnet) části.

* Chcete tooconnect hello virtuální sítě obsahující HDInsight tooanother virtuální sítě nebo v místní síti?

    tooeasily práci s prostředky v sítích, můžete potřebovat toocreate vlastní DNS a nakonfigurujte předávání DNS. Další informace najdete v tématu hello [připojení více sítí](#multinet) části.

* Chcete, aby toorestrict či přesměrování příchozích a odchozích přenosů tooHDInsight?

    HDInsight musí mít neomezený komunikace s konkrétní IP adresy v hello datového centra Azure. Existují také několik portů, které musí být povoleno přes brány firewall pro komunikaci klienta. Další informace najdete v tématu hello [řízení síťového provozu](#networktraffic) části.

## <a id="existingvnet"></a>Přidat HDInsight tooan existující virtuální síť

Jak používat hello kroky v této části toodiscover tooadd nové tooan HDInsight existující virtuální síť Azure.

> [!NOTE]
> Nelze přidat stávající cluster HDInsight do virtuální sítě.

1. Používáte pro virtuální síť hello klasický nebo modelu nasazení Resource Manager?

    HDInsight 3.4 a větší vyžaduje virtuální sítě Resource Manager. Dřívějších verzích HDInsight vyžaduje klasickou virtuální síť.

    Pokud vaší stávající sítě klasickou virtuální síť, musíte vytvořit virtuální sítě Resource Manager a potom se připojte hello dva. [Připojení klasické virtuální sítě toonew virtuálních sítí](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

    Jakmile připojený, HDInsight v síti Resource Manager hello nainstalovaný mohou komunikovat s prostředky v síti classic hello.

2. Používáte vynucené tunelování? Vynucené tunelování je nastavení podsítě, které vynutí odchozí internetové přenosy tooa zařízení pro kontroly a protokolování. HDInsight nepodporuje vynucené tunelování. Buď odeberte vynucené tunelování před instalací HDInsight do podsítě, nebo vytvořit novou podsíť pro HDInsight.

3. Používáte skupiny zabezpečení sítě, trasy definované uživatelem nebo virtuální síťové zařízení toorestrict provoz do nebo z hello virtuální sítě?

    Jako spravovanou službu vyžaduje HDInsight neomezený přístup tooseveral IP adresy v hello datového centra Azure. tooallow komunikace se tyto IP adresy, aktualizovat všechny existující skupiny zabezpečení sítě nebo trasy definované uživatelem.

    HDInsight je hostitelem více služeb, které používají různé porty. Provoz toothese porty nejsou blokovány. Seznam portů tooallow přes virtuální zařízení brány firewall, naleznete v části hello [zabezpečení](#security) části.

    toofind existující konfiguraci zabezpečení, hello použijte následující příkazy prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure:

    * Skupiny zabezpečení sítě

        ```powershell
        $resourceGroupName = Read-Input -Prompt "Enter hello resource group that contains hello virtual network used with HDInsight"
        get-azurermnetworksecuritygroup -resourcegroupname $resourceGroupName
        ```

        ```azurecli-interactive
        read -p "Enter hello name of hello resource group that contains hello virtual network: " RESOURCEGROUP
        az network nsg list --resource-group $RESOURCEGROUP
        ```

        Další informace najdete v tématu hello [odstraňování skupin zabezpečení sítě](../virtual-network/virtual-network-nsg-troubleshoot-portal.md) dokumentu.

        > [!IMPORTANT]
        > Pravidla skupiny zabezpečení sítě jsou použity v pořadí podle priority pravidel. Hello první pravidlo, které odpovídá vzoru provoz hello platí a žádné jiné se použijí pro tento přenos. Pravidla pořadí od nejvíce projektovou tooleast projektovou. Další informace najdete v tématu hello [filtrování provozu sítě přenosů se skupinami zabezpečení sítě](../virtual-network/virtual-networks-nsg.md) dokumentu.

    * Trasy definované uživatelem

        ```powershell
        $resourceGroupName = Read-Input -Prompt "Enter hello resource group that contains hello virtual network used with HDInsight"
        get-azurermroutetable -resourcegroupname $resourceGroupName
        ```

        ```azurecli-interactive
        read -p "Enter hello name of hello resource group that contains hello virtual network: " RESOURCEGROUP
        az network route-table list --resource-group $RESOURCEGROUP
        ```

        Další informace najdete v tématu hello [řešení potíží s trasy](../virtual-network/virtual-network-routes-troubleshoot-portal.md) dokumentu.

4. Vytvoření clusteru HDInsight a vyberte hello Azure Virtual Network během konfigurace. Použijte hello kroky v následujícím procesu vytváření clusteru hello toounderstand dokumenty hello:

    * [Vytvoření HDInsight pomocí hello portálu Azure](hdinsight-hadoop-create-linux-clusters-portal.md)
    * [Vytvoření HDInsight pomocí Azure PowerShellu](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
    * [Vytvoření HDInsight pomocí Azure CLI 1.0](hdinsight-hadoop-create-linux-clusters-azure-cli.md)
    * [Vytvoření HDInsight pomocí šablony Azure Resource Manager](hdinsight-hadoop-create-linux-clusters-arm-templates.md)

  > [!IMPORTANT]
  > Přidání HDInsight tooa virtuální síť se na krok volitelné konfiguraci. Zda tooselect hello virtuální sítě se při konfiguraci clusteru hello.

## <a id="multinet"></a>Připojení více sítí

Hello největších problém s konfigurací s více sítě je překlad mezi sítěmi hello.

Azure poskytuje překlad názvů pro služby Azure, které jsou nainstalovány ve virtuální síti. Toto řešení předdefinovaným názvem umožňuje toohello tooconnect HDInsight následující prostředky pomocí platný plně kvalifikovaný název domény (FQDN):

* Hello jakémukoli prostředku, který je dostupný na Internetu. Například microsoft.com, google.com.

* Jakémukoli prostředku, který je v hello stejnou virtuální síť Azure, pomocí hello __interní název DNS__ hello prostředku. Například při použití hello výchozí název řešení, jsou hello následující příklad interní DNS názvy přiřazené tooHDInsight pracovní uzly:

    * wn0 hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net
    * wn2 hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net

    Obě tyto uzly může komunikovat přímo s a jiné uzly v HDInsight pomocí interní názvy DNS.

rozlišení názvů výchozí Hello nemá __není__ povolit HDInsight tooresolve hello názvy prostředků v sítích, které jsou připojené k toohello virtuální sítě. Například je běžné toojoin místní sítě toohello virtuální sítě. S pouze hello výchozí překlad IP adres HDInsight nemají přístup k prostředkům v místní síti hello podle názvu. Hello opačné je také nastavena hodnota true, prostředky ve vaší místní síti nemají přístup k prostředkům ve virtuální síti hello podle názvu.

> [!WARNING]
> Musíte vytvořit hello vlastního serveru DNS a konfigurovat virtuální sítě toouse hello jej před vytvořením hello clusteru HDInsight.

tooenable překlad mezi hello virtuální sítě a prostředky v připojené k sítím, je třeba provést hello následující akce:

1. Vytvoření vlastního serveru DNS v hello virtuální sítě Azure, kam budete tooinstall HDInsight.

2. Nakonfigurujte hello virtuální sítě toouse hello vlastního serveru DNS.

3. Najde hello přiřazené přípona DNS pro vaši virtuální síť Azure. Tato hodnota je příliš podobné`0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net`. Informace o zjištění hello příponu DNS, najdete v části hello [příklad: vlastní DNS](#example-dns) části.

4. Konfigurace předávání mezi servery DNS hello. Konfigurace Hello závisí na typu hello vzdálené sítě.

    * Pokud hello vzdálené sítě do místní sítě, nakonfigurujte DNS takto:
        
        * __Vlastní DNS__ (ve virtuální síti hello):

            * Předat dál požadavků pro přípony DNS hello hello virtuální sítě toohello Azure rekurzivní překladač (168.63.129.16). Azure zpracovává požadavky na prostředky ve virtuální síti hello

            * Předat dál všechny ostatní požadavky toohello místní server DNS. Hello místního DNS zpracovává všechny ostatní požadavky na rozlišení názvů, i požadavky na internetové prostředky, jako je například Microsoft.com.

        * __Místní DNS__: předávat požadavky pro hello virtuální sítě DNS přípona toohello vlastního serveru DNS. Hello vlastního serveru DNS potom předává toohello Azure rekurzivní překladač.

        Tato požadavky na konfiguraci tras pro plně kvalifikované názvy domény, které obsahují příponu DNS hello hello virtuální sítě toohello vlastního serveru DNS. Server DNS místní hello zpracovává všechny požadavky (i pro veřejné internetové adresy).

    * Pokud vzdálené sítě hello jinou virtuální sítí Azure, nakonfigurujte DNS následujícím způsobem:

        * __Vlastní DNS__ (v každé virtuální sítě):

            * Požadavky pro příponu DNS hello hello virtuální sítě jsou předávány toohello vlastní servery DNS. Hello DNS v každé virtuální sítě je zodpovědná za řešení prostředky ve své síti.

            * Předávání všech dalších překladač Azure rekurzivní toohello požadavky. rekurzivní překladač Hello je zodpovědná za řešení místní a prostředků z Internetu.

        server DNS Hello pro každou síť, předává žádosti o toohello jiných, podle přípony DNS. Další požadavky se přeloží pomocí překladače Azure rekurzivní hello.

    Příklad každé konfiguraci, naleznete v části hello [příklad: vlastní DNS](#example-dns) části.

Další informace najdete v tématu hello [překlad názvů pro virtuální počítače a instance rolí](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) dokumentu.

## <a name="directly-connect-toohadoop-services"></a>Připojovat přímo tooHadoop služby

Většina dokumentace v HDInsight předpokládá, že máte přístup toohello clusteru přes hello Internetu. Například, že se můžete připojit toohello clusteru https://CLUSTERNAME.azurehdinsight.net. Tuto adresu používá hello veřejné bránu, která není k dispozici, pokud jste použili skupiny Nsg nebo hello udr toorestrict přístupu z Internetu.

tooconnect tooAmbari a jiné webové stránky prostřednictvím hello virtuální sítě, použijte hello následující kroky:

1. toodiscover hello interní plně kvalifikované názvy domény (FQDN) hello uzly clusteru HDInsight, použijte jednu z následujících metod hello:

    ```powershell
    $resourceGroupName = "hello resource group that contains hello virtual network used with HDInsight"

    $clusterNICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName | where-object {$_.Name -like "*node*"}

    $nodes = @()
    foreach($nic in $clusterNICs) {
        $node = new-object System.Object
        $node | add-member -MemberType NoteProperty -name "Type" -value $nic.Name.Split('-')[1]
        $node | add-member -MemberType NoteProperty -name "InternalIP" -value $nic.IpConfigurations.PrivateIpAddress
        $node | add-member -MemberType NoteProperty -name "InternalFQDN" -value $nic.DnsSettings.InternalFqdn
        $nodes += $node
    }
    $nodes | sort-object Type
    ```

    ```azurecli
    az network nic list --resource-group <resourcegroupname> --output table --query "[?contains(name,'node')].{NICname:name,InternalIP:ipConfigurations[0].privateIpAddress,InternalFQDN:dnsSettings.internalFqdn}"
    ```

    V seznamu hello uzlů vrátil najít hello plně kvalifikovaný název domény pro hello hlavních uzlech a použitím tooAmbari tooconnect hello plně kvalifikované názvy domény a dalších webových služeb. Například použít `http://<headnode-fqdn>:8080` tooaccess Ambari.

    > [!IMPORTANT]
    > Některé služby hostované v uzlech head hello aktivní pouze na jednom uzlu současně. Pokud se pokusíte přístup k službě na jeden hlavní uzel a vrátí chybu 404, přepínač toohello jiných hlavního uzlu.

2. uzel hello toodetermine a port, který služba je k dispozici, najdete v části hello [porty používané služby Hadoop v HDInsight](./hdinsight-hadoop-port-settings-for-services.md) dokumentu.

## <a id="networktraffic"></a>Řízení síťového provozu

Síťový provoz v Azure Virtual Network se dá řídit pomocí hello následující metody:

* **Skupin zabezpečení sítě** (NSG) umožňují toofilter příchozí a odchozí přenosy toohello sítě. Další informace najdete v tématu hello [filtrování provozu sítě přenosů se skupinami zabezpečení sítě](../virtual-network/virtual-networks-nsg.md) dokumentu.

    > [!WARNING]
    > HDInsight nepodporuje omezení odchozí provoz.

* **Trasy definované uživatelem** (UDR) definovat tok přenosů mezi prostředky v síti hello. Další informace najdete v tématu hello [trasy definované uživatelem a předávání IP](../virtual-network/virtual-networks-udr-overview.md) dokumentu.

* **Síťových virtuálních zařízení** replikovat hello funkce zařízení, jako jsou brány firewall a směrovače. Další informace najdete v tématu hello [síťových zařízení](https://azure.microsoft.com/solutions/network-appliances) dokumentu.

Jako spravované služby HDInsight vyžaduje služby stavu a správu tooAzure neomezený přístup v hello cloudu Azure. Pokud používáte skupiny Nsg a udr, je nutné zajistit, že HDInsight tyto služby můžete stále komunikovat s HDInsight.

HDInsight poskytuje služby na několika portech. Pokud používáte virtuální zařízení brány firewall, musíte povolit přenosy na hello porty používané pro tyto služby. Další informace najdete v části hello [požadované porty].

### <a id="hdinsight-ip"></a>Prostředí HDInsight pomocí skupin zabezpečení sítě a trasy definované uživatelem

Pokud máte v úmyslu používat **skupin zabezpečení sítě** nebo **trasy definované uživatelem** toocontrol přenos v síti, proveďte následující akce před instalací HDInsight hello:

1. Identifikujte hello oblast Azure, abyste naplánovali toouse pro HDInsight.

2. Identifikujte hello IP adresy potřeba v prostředí HDInsight. Další informace najdete v tématu hello [IP adresy, které vyžadují HDInsight](#hdinsight-ip) části.

3. Vytvoření nebo úprava skupiny zabezpečení sítě hello nebo trasy definované uživatelem hello podsítě, abyste naplánovali tooinstall HDInsight do.

    * __Skupin zabezpečení sítě__: Povolit __příchozí__ přenosy na portu __443__ z hello IP adres.
    * __Trasy definované uživatelem__: Vytvoření trasy tooeach IP adresy a nastavte hello __typ dalšího směrování__ too__Internet__.

Další informace o skupinách zabezpečení sítě nebo trasy definované uživatelem najdete v části hello následující dokumentaci:

* [Skupina zabezpečení sítě](../virtual-network/virtual-networks-nsg.md)

* [Trasy definované uživatelem](../virtual-network/virtual-networks-udr-overview.md)

#### <a name="forced-tunneling"></a>Vynucené tunelování

Vynucené tunelování je konfigurace směrování definovaný uživatelem, kde veškerý provoz z podsítě je vynucené tooa určitého síťového umístění nebo umístění, jako například do místní sítě. HDInsight nemá __není__ podporu vynuceného tunelování.

## <a id="hdinsight-ip"></a>Požadované IP adresy

> [!IMPORTANT]
> Hello Azure stavu a služby pro správu musí být schopný toocommunicate s HDInsight. Pokud používáte skupiny zabezpečení sítě nebo trasy definované uživatelem, povolí provoz z hello IP adres pro tyto tooreach služby HDInsight.
>
> Pokud nepoužijete skupin zabezpečení sítě nebo trasy definované uživatelem toocontrol přenosů, můžete ignorovat v této části.

Pokud používáte skupiny zabezpečení sítě nebo trasy definované uživatelem, musíte povolit přenosy z hello Azure stavu a správu služeb tooreach HDInsight. Použijte následující postup toofind hello IP adresy, které musí být povoleno hello:

1. Vždy musí povolit provoz z hello následující IP adresy:

    | IP adresa | Povolené portu | Směr |
    | ---- | ----- | ----- |
    | 168.61.49.99 | 443 | Příchozí |
    | 23.99.5.239 | 443 | Příchozí |
    | 168.61.48.131 | 443 | Příchozí |
    | 138.91.141.162 | 443 | Příchozí |

2. Pokud váš cluster HDInsight v jednom z následujících oblastí hello, musíte povolit přenosy z hello IP adresy uvedené pro oblast hello:

    > [!IMPORTANT]
    > Pokud není uvedené hello oblast Azure, který používáte, pak použijte jenom hello čtyři IP adresy z kroku 1.

    | Země | Oblast | Povolené IP adresy | Povolené portu | Směr |
    | ---- | ---- | ---- | ---- | ----- |
    | Asie | Východní Asie | 23.102.235.122</br>52.175.38.134 | 443 | Příchozí |
    | &nbsp; | Jihovýchodní Asie | 13.76.245.160</br>13.76.136.249 | 443 | Příchozí |
    | Austrálie | Austrálie – východ | 104.210.84.115</br>13.75.152.195 | 443 | Příchozí |
    | &nbsp; | Austrálie – jihovýchod | 13.77.2.56</br>13.77.2.94 | 443 | Příchozí |
    | Brazílie | Brazílie – jih | 191.235.84.104</br>191.235.87.113 | 443 | Příchozí |
    | Kanada | Východní Kanada | 52.229.127.96</br>52.229.123.172 | 443 | Příchozí |
    | &nbsp; | Střední Kanada | 52.228.37.66</br>52.228.45.222 | 443 | Příchozí |
    | Čína | Čína – sever | 42.159.96.170</br>139.217.2.219 | 443 | Příchozí |
    | &nbsp; | Čína – východ | 42.159.198.178</br>42.159.234.157 | 443 | Příchozí |
    | Evropa | Severní Evropa | 52.164.210.96</br>13.74.153.132 | 443 | Příchozí |
    | &nbsp; | Západní Evropa| 52.166.243.90</br>52.174.36.244 | 443 | Příchozí |
    | Německo | Německo – střed | 51.4.146.68</br>51.4.146.80 | 443 | Příchozí |
    | &nbsp; | Německo – severovýchod | 51.5.150.132</br>51.5.144.101 | 443 | Příchozí |
    | Indie | Střed Indie | 52.172.153.209</br>52.172.152.49 | 443 | Příchozí |
    | Japonsko | Japonsko – východ | 13.78.125.90</br>13.78.89.60 | 443 | Příchozí |
    | &nbsp; | Japonsko – západ | 40.74.125.69</br>138.91.29.150 | 443 | Příchozí |
    | Korea | Korea – střed | 52.231.39.142</br>52.231.36.209 | 433 | Příchozí |
    | &nbsp; | Korea – jih | 52.231.203.16</br>52.231.205.214 | 443 | Příchozí
    | Spojené království | Spojené království – západ | 51.141.13.110</br>51.141.7.20 | 443 | Příchozí |
    | &nbsp; | Spojené království – jih | 51.140.47.39</br>51.140.52.16 | 443 | Příchozí |
    | Spojené státy | Střed USA | 13.67.223.215</br>40.86.83.253 | 443 | Příchozí |
    | &nbsp; | Střed USA – sever | 157.56.8.38</br>157.55.213.99 | 443 | Příchozí |
    | &nbsp; | Západní střed USA | 52.161.23.15</br>52.161.10.167 | 443 | Příchozí |
    | &nbsp; | Západní USA 2 | 52.175.211.210</br>52.175.222.222 | 443 | Příchozí |

    Informace o hello IP adresy toouse pro Azure Government, najdete v části hello [Azure Government Intelligence + analýzy](https://docs.microsoft.com/azure/azure-government/documentation-government-services-intelligenceandanalytics) dokumentu.

3. Pokud používáte vlastního serveru DNS s vaší virtuální síti, musíte také povolit přístup z __168.63.129.16__. Tato adresa se Azure rekurzivní překladač. Další informace najdete v tématu hello [překlad názvů pro virtuální počítače a Role instance](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) dokumentu.

Další informace najdete v tématu hello [řízení síťového provozu](#networktraffic) části.

## <a id="hdinsight-ports"></a>Požadované porty

Pokud máte v úmyslu používat síť **virtuální zařízení brány firewall** toosecure hello virtuální síť, musíte povolit odchozí přenosy na hello následující porty:

* 53
* 443
* 1433
* 11000-11999
* 14000-14999

Seznam portů pro určité služby, najdete v části hello [porty používané služby Hadoop v HDInsight](hdinsight-hadoop-port-settings-for-services.md) dokumentu.

Další informace o pravidlech brány firewall pro virtuální zařízení, najdete v části hello [virtuální zařízení scénář](../virtual-network/virtual-network-scenario-udr-gw-nva.md) dokumentu.

## <a id="hdinsight-nsg"></a>Příklad: skupin zabezpečení sítě s HDInsight

Příklady Hello v této části ukazují, jak skupinu zabezpečení sítě toocreate pravidel, které umožňují služby Azure pro HDInsight toocommunicate s hello. Před použitím hello příklady, upravte hello IP adresy toomatch hello ty, které jsou pro hello oblast Azure, který používáte. Tyto informace můžete najít v hello [prostředí HDInsight pomocí skupin zabezpečení sítě a trasy definované uživatelem](#hdinsight-ip) části.

### <a name="azure-resource-management-template"></a>Šablony Azure Správa prostředků

Hello následující Správa prostředků šablona vytvoří virtuální síť, která omezuje příchozí provoz, ale umožňuje přenos z hello IP adres vyžadují HDInsight. Tato šablona vytvoří cluster služby HDInsight také ve virtuální síti hello.

* [Nasazení zabezpečených virtuální síť Azure a cluster systému HDInsight Hadoop](https://azure.microsoft.com/resources/templates/101-hdinsight-secure-vnet/)

> [!IMPORTANT]
> Změnit hello IP adresy použité v této příklad toomatch hello oblast Azure, který používáte. Tyto informace můžete najít v hello [prostředí HDInsight pomocí skupin zabezpečení sítě a trasy definované uživatelem](#hdinsight-ip) části.

### <a name="azure-powershell"></a>Azure PowerShell

Použijte následující toocreate skript prostředí PowerShell virtuální síť, která omezuje příchozí provoz a umožňuje provoz z hello IP adres pro oblast Severní Evropa hello hello.

> [!IMPORTANT]
> Změnit hello IP adresy použité v této příklad toomatch hello oblast Azure, který používáte. Tyto informace můžete najít v hello [prostředí HDInsight pomocí skupin zabezpečení sítě a trasy definované uživatelem](#hdinsight-ip) části.

```powershell
$vnetName = "Replace with your virtual network name"
$resourceGroupName = "Replace with hello resource group hello virtual network is in"
$subnetName = "Replace with hello name of hello subnet that you plan toouse for HDInsight"
# Get hello Virtual Network object
$vnet = Get-AzureRmVirtualNetwork `
    -Name $vnetName `
    -ResourceGroupName $resourceGroupName
# Get hello region hello Virtual network is in.
$location = $vnet.Location
# Get hello subnet object
$subnet = $vnet.Subnets | Where-Object Name -eq $subnetName
# Create a Network Security Group.
# And add exemptions for hello HDInsight health and management services.
$nsg = New-AzureRmNetworkSecurityGroup `
    -Name "hdisecure" `
    -ResourceGroupName $resourceGroupName `
    -Location $location `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -name "hdirule1" `
        -Description "HDI health and management address 52.164.210.96" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "52.164.210.96" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 300 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 13.74.153.132" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "13.74.153.132" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 301 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 168.61.49.99" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "168.61.49.99" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 302 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 23.99.5.239" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "23.99.5.239" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 303 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 168.61.48.131" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "168.61.48.131" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 304 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 138.91.141.162" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "138.91.141.162" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 305 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "blockeverything" `
        -Description "Block everything else" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "*" `
        -SourceAddressPrefix "Internet" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Deny `
        -Priority 500 `
        -Direction Inbound
# Set hello changes toohello security group
Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
# Apply hello NSG toohello subnet
Set-AzureRmVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name $subnetName `
    -AddressPrefix $subnet.AddressPrefix `
    -NetworkSecurityGroup $nsg
```

> [!IMPORTANT]
> Tento příklad ukazuje, jak tooadd pravidla tooallow příchozí přenosy na hello požadované IP adresy. Neobsahuje toorestrict pravidlo příchozí přístup z jiných zdrojů.
>
> Hello následující příklad ukazuje, jak přistupovat k tooenable SSH ze hello Internetu:
>
> ```powershell
> Add-AzureRmNetworkSecurityRuleConfig -Name "SSH" -Description "SSH" -Protocol "*" -SourcePortRange "*" -DestinationPortRange "22" -SourceAddressPrefix "*" -DestinationAddressPrefix "VirtualNetwork" -Access Allow -Priority 306 -Direction Inbound
> ```

### <a name="azure-cli"></a>Azure CLI

Pomocí následujících kroků toocreate virtuální síť, která omezuje příchozí provoz, ale umožňuje přenos z hello IP adres vyžadují HDInsight hello.

1. Hello použijte následující příkaz toocreate novou skupinu zabezpečení sítě s názvem `hdisecure`. Nahraďte **RESOURCEGROUPNAME** s hello skupinu prostředků, který obsahuje hello Azure Virtual Network. Nahraďte **umístění** s umístěním (oblastí), hello této skupiny hello byl vytvořen v.

    ```azurecli
    az network nsg create -g RESOURCEGROUPNAME -n hdisecure -l LOCATION
    ```

    Po vytvoření skupiny hello zobrazí informace o nové skupiny hello.

2. Použijte následující tooadd pravidla toohello novou skupinu zabezpečení sítě, povolit příchozí komunikaci na portu 443 z hello stavu a správu služby Azure HDInsight hello. Nahraďte **RESOURCEGROUPNAME** s názvem hello hello skupiny prostředků, která obsahuje hello Azure Virtual Network.

    > [!IMPORTANT]
    > Změnit hello IP adresy použité v této příklad toomatch hello oblast Azure, který používáte. Tyto informace můžete najít v hello [prostředí HDInsight pomocí skupin zabezpečení sítě a trasy definované uživatelem](#hdinsight-ip) části.

    ```azurecli
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule1 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "52.164.210.96" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 300 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "13.74.153.132" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 301 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "168.61.49.99" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 302 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "23.99.5.239" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 303 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "168.61.48.131" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 304 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "138.91.141.162" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 305 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n block --protocol "*" --source-port-range "*" --destination-port-range "*" --source-address-prefix "Internet" --destination-address-prefix "VirtualNetwork" --access "Deny" --priority 500 --direction "Inbound"
    ```

3. tooretrieve hello jedinečný identifikátor pro tuto skupinu zabezpečení sítě, použijte následující příkaz hello:

    ```azurecli
    az network nsg show -g RESOURCEGROUPNAME -n hdisecure --query 'id'
    ```

    Tento příkaz vrátí hodnotu podobné toohello následující text:

        "/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUPNAME/providers/Microsoft.Network/networkSecurityGroups/hdisecure"

    Pokud neobdržíte hello očekávané výsledky, použijte dvojité uvozovky kolem id v příkazu hello.

4. Použijte následující příkaz tooapply hello zabezpečení skupiny tooa podsíti hello. Nahraďte hello __GUID__ a __RESOURCEGROUPNAME__ hodnoty s hello ty, které jsou vráceny z předchozího kroku hello. Nahraďte __VNETNAME__ a __SUBNETNAME__ s hello název virtuální sítě a název podsítě, které chcete toocreate.

    ```azurecli
    az network vnet subnet update -g RESOURCEGROUPNAME --vnet-name VNETNAME --name SUBNETNAME --set networkSecurityGroup.id="/subscriptions/GUID/resourceGroups/RESOURCEGROUPNAME/providers/Microsoft.Network/networkSecurityGroups/hdisecure"
    ```

    Po dokončení tohoto příkazu můžete nainstalovat HDInsight do hello virtuální sítě.

> [!IMPORTANT]
> Tyto kroky pouze otevřete přístup toohello stavu a správu Služba HDInsight na hello cloudu Azure. Všechny ostatní přístup toohello clusteru HDInsight z hello mimo virtuální síť je blokována. tooenable přístup mimo hello virtuální sítě, musíte přidat další pravidla skupiny zabezpečení sítě.
>
> Hello následující příklad ukazuje, jak přistupovat k tooenable SSH ze hello Internetu:
>
> ```azurecli
> az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule5 --protocol "*" --source-port-range "*" --destination-port-range "22" --source-address-prefix "*" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 306 --direction "Inbound"
> ```

## <a id="example-dns"></a>Příklad: Konfigurace DNS

### <a name="name-resolution-between-a-virtual-network-and-a-connected-on-premises-network"></a>Překlad mezi virtuální sítí a připojené místní sítě

Tento příklad vytvoří hello následující předpoklady:

* Máte virtuální síť Azure, který je připojený tooan místní síti pomocí brány VPN.

* Hello vlastního serveru DNS ve virtuální síti hello běží jako hello operačního systému Linux nebo Unix.

* [Vytvoření vazby](https://www.isc.org/downloads/bind/) je nainstalován na serveru DNS pro vlastní hello.

Na hello vlastního serveru DNS ve virtuální síti hello:

1. Použití Azure PowerShell nebo rozhraní příkazového řádku Azure toofind hello příponu DNS virtuální sítě hello:

    ```powershell
    $resourceGroupName = Read-Input -Prompt "Enter hello resource group that contains hello virtual network used with HDInsight"
    $NICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName
    $NICs[0].DnsSettings.InternalDomainNameSuffix
    ```

    ```azurecli-interactive
    read -p "Enter hello name of hello resource group that contains hello virtual network: " RESOURCEGROUP
    az network nic list --resource-group $RESOURCEGROUP --query "[0].dnsSettings.internalDomainNameSuffix"
    ```

2. Na hello vlastního serveru DNS pro virtuální síť hello, použijte následující text jako hello obsah hello hello `/etc/bind/named.conf.local` souboru:

    ```
    // Forward requests for hello virtual network suffix tooAzure recursive resolver
    zone "0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net" {
        type forward;
        forwarders {168.63.129.16;}; # Azure recursive resolver
    };
    ```

    Nahraďte hello `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` hodnotu s hello příponu DNS virtuální sítě.

    Tato konfigurace směrovat všechny požadavky na DNS pro příponu DNS hello hello virtuální sítě toohello Azure rekurzivní překladač.

2. Na hello vlastního serveru DNS pro virtuální síť hello, použijte následující text jako hello obsah hello hello `/etc/bind/named.conf.options` souboru:

    ```
    // Clients tooaccept requests from
    // TODO: Add hello IP range of hello joined network toothis list
    acl goodclients {
        10.0.0.0/16; # IP address range of hello virtual network
        localhost;
        localnets;
    };

    options {
            directory "/var/cache/bind";

            recursion yes;

            allow-query { goodclients; };

            # All other requests are sent toohello following
            forwarders {
                192.168.0.1; # Replace with hello IP address of your on-premises DNS server
            };

            dnssec-validation auto;

            auth-nxdomain no;    # conform tooRFC1035
            listen-on { any; };
    };
    ```
    
    * Nahraďte hello `10.0.0.0/16` hodnotu s hello rozsah IP adres vaší virtuální sítě. Tato položka umožňuje název řešení požadavků adresy v tomto rozsahu.

    * Přidat rozsah IP adres hello místní sítě toohello hello `acl goodclients { ... }` části.  položka umožňuje požadavky na rozlišení názvů z prostředků v hello do místní sítě.
    
    * Nahradí hodnotu hello `192.168.0.1` hello IP adresu serveru DNS na místě. Tato položka směrovat všechny požadavky toohello místní DNS servery DNS.

3. Konfigurace hello toouse, restartujte vazby. Například, `sudo service bind9 restart`.

4. Přidání serveru DNS pro podmíněné předávání toohello místně. Nakonfigurujte požadavky toosend pro podmíněné předávání hello hello přípony DNS z kroku 1 toohello vlastního serveru DNS.

    > [!NOTE]
    > Dokumentaci hello softwaru DNS pro konkrétní na postupy pro tooadd pro podmíněné předávání.

Po dokončení těchto kroků, se můžete připojit tooresources v síti, buď pomocí plně kvalifikovaných názvů domény (FQDN). HDInsight můžete nainstalovat do virtuální sítě hello.

### <a name="name-resolution-between-two-connected-virtual-networks"></a>Překlad mezi dvěma připojené virtuální sítě

Tento příklad vytvoří hello následující předpoklady:

* Máte dvě virtuální sítě Azure, které jsou připojené pomocí brány VPN nebo partnerský vztah.

* Hello vlastního serveru DNS v obě sítě běží jako hello operačního systému Linux nebo Unix.

* [Vytvoření vazby](https://www.isc.org/downloads/bind/) je nainstalován na hello vlastní servery DNS.

1. Použití Azure PowerShell nebo rozhraní příkazového řádku Azure toofind hello příponu DNS obě virtuální sítě:

    ```powershell
    $resourceGroupName = Read-Input -Prompt "Enter hello resource group that contains hello virtual network used with HDInsight"
    $NICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName
    $NICs[0].DnsSettings.InternalDomainNameSuffix
    ```

    ```azurecli-interactive
    read -p "Enter hello name of hello resource group that contains hello virtual network: " RESOURCEGROUP
    az network nic list --resource-group $RESOURCEGROUP --query "[0].dnsSettings.internalDomainNameSuffix"
    ```

2. Použití hello následující text jako hello obsah hello `/etc/bind/named.config.local` souboru na hello vlastního serveru DNS. Tuto změnu proveďte na hello vlastního serveru DNS v obě virtuální sítě.

    ```
    // Forward requests for hello virtual network suffix tooAzure recursive resolver
    zone "0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net" {
        type forward;
        forwarders {10.0.0.4;}; # hello IP address of hello DNS server in hello other virtual network
    };
    ```

    Nahraďte hello `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` hodnotu s příponu DNS hello hello __jiných__ virtuální sítě. Tato položka směruje požadavky pro příponu DNS hello hello vzdálených síťových toohello vlastní DNS v síti.

3. Na hello vlastní servery DNS v obě virtuální sítě, použijte následující text jako hello obsah hello hello `/etc/bind/named.conf.options` souboru:

    ```
    // Clients tooaccept requests from
    acl goodclients {
        10.1.0.0/16; # hello IP address range of one virtual network
        10.0.0.0/16; # hello IP address range of hello other virtual network
        localhost;
        localnets;
    };

    options {
            directory "/var/cache/bind";

            recursion yes;

            allow-query { goodclients; };

            forwarders {
            168.63.129.16;   # Azure recursive resolver         
            };

            dnssec-validation auto;

            auth-nxdomain no;    # conform tooRFC1035
            listen-on { any; };
    };
    ```
    
    * Nahraďte hello `10.0.0.0/16` a `10.1.0.0/16` hodnoty s hello IP adres, rozsahy virtuálních sítí. Tato položka umožňuje prostředky v každé sítě toomake požadavky hello serverů DNS.

    Všechny žádosti, které nejsou pro přípony DNS hello hello virtuálních sítí (například microsoft.com) se zpracovává souborem hello Azure rekurzivní překladač.

4. Konfigurace hello toouse, restartujte vazby. Například `sudo service bind9 restart` na obou serverech DNS.

Po dokončení těchto kroků, se můžete připojit tooresources ve virtuální síti hello pomocí plně kvalifikovaných názvů domény (FQDN). HDInsight můžete nainstalovat do virtuální sítě hello.

## <a name="next-steps"></a>Další kroky

* Příklad začátku do konce konfigurace HDInsight tooconnect tooan do místní sítě, naleznete v části [připojit HDInsight tooan do místní sítě](./connect-on-premises-network.md).

* Další informace o virtuálních sítí Azure, najdete v části hello [Přehled virtuálních sítí Azure](../virtual-network/virtual-networks-overview.md).

* Další informace o skupinách zabezpečení sítě najdete v tématu [skupin zabezpečení sítě](../virtual-network/virtual-networks-nsg.md).

* Další informace o trasy definované uživatelem, najdete v části [trasy definované uživatelem a předávání IP](../virtual-network/virtual-networks-udr-overview.md).