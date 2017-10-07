---
title: "místní síť aaaConnect HDInsight tooyour – Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak toocreate HDInsight cluster v Azure Virtual Network a připojte ho tooyour do místní sítě. Zjistěte, jak tooconfigure překlad mezi HDInsight a místní sítí pomocí vlastního serveru DNS."
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/21/2017
ms.author: larryfr
ms.openlocfilehash: 8a3adf0e3df7726d8e6566d723700506baaf89a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-hdinsight-tooyour-on-premise-network"></a>Připojení HDInsight tooyour místní sítě

Zjistěte, jak tooconnect HDInsight tooyour místní sítě pomocí virtuální sítě Azure a brány VPN. Tento dokument obsahuje informace o plánování na:

* Pomocí HDInsight ve virtuální síti Azure, která se připojuje tooyour místní sítě.

* Konfigurace překladu názvů DNS mezi hello virtuální sítě a v místní síti.

* Konfigurace sítě zabezpečení skupiny toorestrict internet přístup tooHDInsight.

* Porty poskytovaných v HDInsight ve virtuální síti hello.

## <a name="create-hello-virtual-network-configuration"></a>Vytvoření konfigurace virtuální sítě hello

> [!IMPORTANT]
> Pokud hledáte pokyny krok za krokem k připojování HDInsight tooyour místní sítě pomocí virtuální síť Azure, naleznete v tématu hello [připojit HDInsight tooyour místní sítě](connect-on-premises-network.md) dokumentu.

Použití hello následující dokumenty toolearn jak toocreate virtuální síť Azure, který je připojený tooyour místní sítě:
    
* [Pomocí hello portálu Azure](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)

* [Použití Azure PowerShellu](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

* [Použití Azure CLI](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md)

## <a name="configure-name-resolution"></a>Konfigurování překladu

tooallow HDInsight a prostředky v toocommunicate hello připojený k síti podle názvu, je třeba provést hello následující akce:

* Vytvoření vlastního serveru DNS v hello Azure Virtual Network.

* Nakonfigurujte hello virtuální sítě toouse hello vlastního serveru DNS místo výchozí hello Azure rekurzivní překladač.

* Konfigurace předávání mezi hello vlastního serveru DNS a serveru DNS na místě.

Tato konfigurace umožňuje hello následující chování:

* Požadavky pro plně kvalifikované názvy domény které mají příponu DNS hello __pro virtuální síť hello__ se předávají toohello vlastního serveru DNS. Hello vlastního serveru DNS potom předává tyto požadavky toohello rekurzivní překladač Azure, která vrátí hodnotu hello IP adresu.

* Všechny ostatní požadavky jsou předávány toohello místní DNS server. I požadavky na veřejné internetové prostředky, jako je například microsoft.com jsou předávány toohello na místním serveru DNS pro rozlišení názvu.

Zelená řádky v následujícím diagramu hello, jsou požadavky na prostředky, které končí příponu DNS hello hello virtuální sítě. Modré řádky jsou požadavky na prostředky v místní síti hello nebo na hello veřejného Internetu.

![Diagram způsob řešení požadavky na DNS v hello konfigurace použitá v tomto dokumentu](./media/connect-on-premises-network/on-premises-to-cloud-dns.png)

### <a name="create-a-custom-dns-server"></a>Vytvoření vlastního serveru DNS

> [!IMPORTANT]
> Musíte vytvořit a nakonfigurovat hello DNS server před instalací HDInsight do virtuální sítě hello.

toocreate Linux virtuálního počítače, který používá hello [vazby](https://www.isc.org/downloads/bind/) DNS softwaru, hello použijte následující kroky:

> [!NOTE]
> Hello následující postup použijte hello [portál Azure](https://portal.azure.com) toocreate virtuální počítač Azure. Jiné způsoby toocreate virtuálního počítače najdete v části hello [vytvoření virtuálního počítače – rozhraní příkazového řádku Azure](../virtual-machines/linux/quick-create-cli.md) a [vytvoření virtuálního počítače - prostředí Azure PowerShell](../virtual-machines/linux/quick-create-portal.md) dokumenty.

1. Z hello [portál Azure](https://portal.azure.com), vyberte  __+__ , __výpočetní__, a __Ubuntu Server 16.04 LTS__.

    ![Vytvoření virtuálního počítače Ubuntu](./media/connect-on-premises-network/create-ubuntu-vm.png)

2. Z hello __Základy__ zadejte hello následující informace:

    * __Název__: popisný název, který identifikuje tento virtuální počítač. Například __DNSProxy__.
    * __Uživatelské jméno__: název hello hello účtu SSH.
    * __Veřejný klíč SSH__ nebo __heslo__: hello metody ověřování pro hello účtu SSH. Doporučujeme používat veřejných klíčů, protože se jedná o bezpečnější. Další informace najdete v tématu hello [vytvoření a použití klíče SSH pro virtuální počítače s Linuxem](../virtual-machines/linux/mac-create-ssh-keys.md) dokumentu.
    * __Skupina prostředků__: vyberte __použít existující__a pak vyberte skupinu prostředků hello, který obsahuje hello virtuální sítě vytvořené dříve.
    * __Umístění__: Vyberte hello stejné umístění jako virtuální síť hello.

    ![Základní konfigurace virtuálního počítače](./media/connect-on-premises-network/vm-basics.png)

    Jiné položky v hello nechte výchozí hodnoty a potom vyberte __OK__.

3. Z hello __zvolte velikost__ část, vyberte hello velikost virtuálního počítače. V tomto kurzu vyberte hello nejmenší a nejnižší náklady možnost. toocontinue, použijte hello __vyberte__ tlačítko.

4. Z hello __nastavení__ zadejte hello následující informace:

    * __Virtuální síť__: Vyberte hello virtuální síť, kterou jste vytvořili dříve.

    * __Podsíť__: Vyberte výchozí hello podsíť pro virtuální síť hello. Proveďte __není__ vyberte hello podsítě používané hello VPN Gateway.

    * __Účet úložiště diagnostiky__: Vyberte existující účet úložiště, nebo vytvořte novou.

    ![Nastavení virtuální sítě](./media/connect-on-premises-network/virtual-network-settings.png)

    Nechat hello jiné položky v hello výchozí hodnotu a pak vyberte __OK__ toocontinue.

5. Z hello __nákupu__ části, vyberte hello __nákupu__ tlačítko toocreate hello virtuálního počítače.

6. Po vytvoření virtuálního počítače hello jeho __přehled__ části se zobrazí. Hello seznamu na levé straně hello vyberte __vlastnosti__. Uložit hello __veřejnou IP adresu__ a __privátní IP adresa__ hodnoty. Použije se v další části hello.

    ![Veřejné a privátní IP adresy](./media/connect-on-premises-network/vm-ip-addresses.png)

### <a name="install-and-configure-bind-dns-software"></a>Instalace a konfigurace vazby (DNS software)

1. Použití SSH tooconnect toohello __veřejnou IP adresu__ hello virtuálního počítače. Následující ukázka Hello připojí tooa virtuálního počítače v 40.68.254.142:

    ```bash
    ssh sshuser@40.68.254.142
    ```

    Nahraďte `sshuser` s hello SSH uživatelský účet, jste zadali při vytváření clusteru hello.

    > [!NOTE]
    > Existují různé způsoby tooobtain hello `ssh` nástroj. Na systému Linux, Unix a systému macOS je poskytována jako součást hello operačního systému. Pokud používáte systém Windows, zvažte jednu hello následující možnosti:
    >
    > * [Prostředí cloudu Azure](../cloud-shell/quickstart.md)
    > * [Bash na Ubuntu na Windows 10](https://msdn.microsoft.com/commandline/wsl/about)
    > * [Git (https://git-scm.com/)](https://git-scm.com/)
    > * [OpenSSH (https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)

2. tooinstall vazby, použijte následující příkazy z relace SSH hello hello:

    ```bash
    sudo apt-get update -y
    sudo apt-get install bind9 -y
    ```

3. tooconfigure vazby tooforward název řešení požadavky tooyour místní server DNS, použijte následující text jako hello obsah hello hello `/etc/bind/named.conf.options` souboru:

        acl goodclients {
            10.0.0.0/16; # Replace with hello IP address range of hello virtual network
            10.1.0.0/16; # Replace with hello IP address range of hello on-premises network
            localhost;
            localnets;
        };

        options {
                directory "/var/cache/bind";

                recursion yes;

                allow-query { goodclients; };

                forwarders {
                192.168.0.1; # Replace with hello IP address of hello on-premises DNS server
                };

                dnssec-validation auto;

                auth-nxdomain no;    # conform tooRFC1035
                listen-on { any; };
        };

    > [!IMPORTANT]
    > Nahraďte hodnoty hello v hello `goodclients` oddíl s rozsah IP adres hello hello virtuální sítě a místní sítě. Tento oddíl definuje hello adresy, které tento server DNS přijímá požadavky od.
    >
    > Nahraďte hello `192.168.0.1` položku v hello `forwarders` oddíl s hello IP adresu serveru DNS na místě. Tato položka trasy DNS požadavky tooyour místní server DNS pro rozlišení.

    tooedit tento soubor hello použijte následující příkaz:

    ```bash
    sudo nano /etc/bind/named.conf.options
    ```

    toosave hello soubor, použijte __Ctrl + X__, __Y__a potom __Enter__.

4. Z relace SSH hello použijte následující příkaz hello:

    ```bash
    hostname -f
    ```

    Tento příkaz vrátí hodnotu podobné toohello následující text:

        dnsproxy.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net

    Hello `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` text je hello __příponu DNS__ pro tuto virtuální síť. Tato hodnota, uložte, protože se později používá.

5. tooconfigure vazby tooresolve názvy DNS pro prostředky v rámci hello virtuální sítě, použijte následující text jako hello obsah hello hello `/etc/bind/named.conf.local` souboru:

        // Replace hello following with hello DNS suffix for your virtual network
        zone "icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net" {
            type forward;
            forwarders {168.63.129.16;}; # hello Azure recursive resolver
        };

    > [!IMPORTANT]
    > Je třeba nahradit hello `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` s příponou hello DNS, který jste získali dříve.

    tooedit tento soubor hello použijte následující příkaz:

    ```bash
    sudo nano /etc/bind/named.conf.local
    ```

    toosave hello soubor, použijte __Ctrl + X__, __Y__a potom __Enter__.

6. toostart vazby, hello použijte následující příkaz:

    ```bash
    sudo service bind9 restart
    ```

7. tooverify, který vazbu můžete vyřešit hello názvy zdrojů v síti na pracovišti, hello použijte následující příkazy:

    ```bash
    sudo apt install dnsutils
    nslookup dns.mynetwork.net 10.0.0.4
    ```

    > [!IMPORTANT]
    > Nahraďte `dns.mynetwork.net` s hello plně kvalifikovaný název domény (FQDN) prostředku v síti na pracovišti.
    >
    > Nahraďte `10.0.0.4` s hello __interní IP adresu__ vašeho vlastního serveru DNS ve virtuální síti hello.

    Hello odpovědi, zobrazí se podobné toohello následující text:

        Server:         10.0.0.4
        Address:        10.0.0.4#53

        Non-authoritative answer:
        Name:   dns.mynetwork.net
        Address: 192.168.0.4

### <a name="configure-hello-virtual-network-toouse-hello-custom-dns-server"></a>Konfigurace hello virtuální sítě toouse hello vlastního serveru DNS

tooconfigure hello virtuální sítě toouse hello vlastního serveru DNS místo hello Azure rekurzivní překladač použijte hello následující kroky:

1. V hello [portál Azure](https://portal.azure.com), vyberte hello virtuální sítě a pak vyberte __servery DNS__.

2. Vyberte __vlastní__a zadejte hello __interní IP adresu__ hello vlastního serveru DNS. Nakonec vyberte __Uložit__.

    ![Nastavit hello vlastního serveru DNS pro síť hello](./media/connect-on-premises-network/configure-custom-dns.png)

### <a name="configure-hello-on-premises-dns-server"></a>Nakonfigurujte server DNS místní hello

V předchozí části hello že jste nakonfigurovali hello vlastní DNS server tooforward požadavky toohello místní server DNS. Dále je nutné nakonfigurovat hello místní DNS server tooforward požadavky toohello vlastního serveru DNS.

Postup pro konkrétní tooconfigure vašeho serveru DNS, najdete v dokumentaci hello software serveru DNS. Vyhledejte hello postup tooconfigure __pro podmíněné předávání__.

Podmíněné dopředného pouze předá požadavky pro konkrétní příponu DNS. V takovém případě musíte nakonfigurovat server pro předávání pro příponu DNS hello hello virtuální sítě. Požadavky pro tuto příponu mají předávat toohello IP adresu hello vlastního serveru DNS. 

Hello následující text je příklad konfigurace pro podmíněné předávání pro hello **vazby** DNS softwaru:

    zone "icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net" {
        type forward;
        forwarders {10.0.0.4;}; # hello custom DNS server's internal IP address
    };

Informace o použití DNS na **systému Windows Server 2016**, najdete v části hello [přidat DnsServerConditionalForwarderZone](https://technet.microsoft.com/itpro/powershell/windows/dnsserver/add-dnsserverconditionalforwarderzone) dokumentace...

Jakmile jste nakonfigurovali server DNS místní hello, můžete použít `nslookup` z hello místní sítě tooverify, abyste mohli vyřešit názvy ve virtuální síti hello. Následující ukázka Hello 

```bash
nslookup dnsproxy.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net 196.168.0.4
```

Tento příklad používá hello na místním serveru DNS na 196.168.0.4 tooresolve hello název hello vlastního serveru DNS. Nahraďte text hello, jeden pro server DNS místní hello hello IP adresu. Nahraďte hello `dnsproxy` adresu s hello plně kvalifikovaný název domény hello vlastního serveru DNS.

## <a name="optional-control-network-traffic"></a>Volitelné: Řízení síťového provozu

Můžete použít skupiny zabezpečení sítě (NSG) nebo trasy definované uživatelem (UDR) toocontrol síťový provoz. Skupiny Nsg umožňují toofilter příchozí a odchozí přenos dat a povolí nebo zakážou provoz hello. Udr umožňují toocontrol tok přenosů mezi prostředky ve virtuální síti hello hello internet a hello do místní sítě.

> [!WARNING]
> HDInsight vyžaduje příchozí přístup z konkrétní IP adresy v hello cloudu Azure a neomezený přístup pro odchozí připojení. Pokud používáte skupiny Nsg nebo udr toocontrol provoz, je třeba provést hello následující kroky:
>
> 1. Najde hello IP adresy pro hello umístění, která obsahuje virtuální síť. Seznam požadované IP adresy podle umístění najdete v tématu [požadované IP adresy](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip).
>
> 2. Povolí příchozí provoz z hello IP adres.
>
>    * __Skupina NSG__: Povolit __příchozí__ přenosy na portu __443__ z hello __Internet__.
>    * __UDR__: Sada hello __další směrování__ typ too__Internet__ hello trasy.

Příklad použití Azure PowerShell nebo rozhraní příkazového řádku Azure toocreate hello skupin Nsg, naleznete v části hello [rozšířit HDInsight s virtuálními sítěmi Azure](./hdinsight-extend-hadoop-virtual-network.md#hdinsight-nsg) dokumentu.

## <a name="create-hello-hdinsight-cluster"></a>Vytvoření clusteru HDInsight se hello

> [!WARNING]
> Před instalací HDInsight ve virtuální síti hello je nutné nakonfigurovat hello vlastního serveru DNS.

Hello použijte kroky v hello [vytvoření clusteru HDInsight pomocí portálu Azure hello](./hdinsight-hadoop-create-linux-clusters-portal.md) dokumentu toocreate clusteru služby HDInsight.

> [!WARNING]
> * Při vytváření clusteru musíte zvolit hello umístění, která obsahuje virtuální síť.
>
> * V hello __upřesňující nastavení__ část konfigurace, je nutné vybrat hello virtuální síť a podsíť, kterou jste vytvořili dříve.

## <a name="connecting-toohdinsight"></a>Připojení tooHDInsight

Většina dokumentace v HDInsight předpokládá, že máte přístup toohello clusteru přes hello Internetu. Například, že se můžete připojit toohello clusteru https://CLUSTERNAME.azurehdinsight.net. Tuto adresu používá hello veřejné bránu, která není k dispozici, pokud jste použili skupiny Nsg nebo hello udr toorestrict přístupu z Internetu.

toodirectly připojení tooHDInsight přes hello virtuální síť, použijte hello následující kroky:

1. toodiscover hello interní plně kvalifikované názvy domény hello uzly clusteru HDInsight, použijte jednu z následujících metod hello:

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

2. toodetermine hello port, který služba je k dispozici, najdete v části hello [porty používané služby Hadoop v HDInsight](./hdinsight-hadoop-port-settings-for-services.md) dokumentu.

    > [!IMPORTANT]
    > Některé služby hostované v uzlech head hello aktivní pouze na jednom uzlu současně. Pokud se pokusíte přístup k službě jeden hlavního uzlu a ona selže, přepínače toohello jiných hlavního uzlu.
    >
    > Například Ambari je aktivní pouze jeden hlavního uzlu současně. Pokud se pokusíte přístup k Ambari na jeden hlavní uzel a vrátí chybu 404, pak je spuštěn na hello jiných hlavního uzlu.

## <a name="next-steps"></a>Další kroky

* Další informace o používání HDInsight ve virtuální síti, najdete v části [rozšířit HDInsight pomocí virtuálních sítí Azure](./hdinsight-extend-hadoop-virtual-network.md).

* Další informace o virtuálních sítí Azure, najdete v části hello [Přehled virtuálních sítí Azure](../virtual-network/virtual-networks-overview.md).

* Další informace o skupinách zabezpečení sítě najdete v tématu [skupin zabezpečení sítě](../virtual-network/virtual-networks-nsg.md).

* Další informace o trasy definované uživatelem, najdete v části [trasy definované uživatelem a předávání IP](../virtual-network/virtual-networks-udr-overview.md).
