---
title: aaaRun MariaDB (MySQL) clusteru v Azure | Microsoft Docs
description: "Vytvoření MariaDB + Galera MySQL clusteru na virtuálních počítačích Azure"
services: virtual-machines-linux
documentationcenter: 
author: sabbour
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: d0d21937-7aac-4222-8255-2fdc4f2ea65b
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/15/2015
ms.author: asabbour
ms.openlocfilehash: f9a4d6c45d76478a8a3526b407c7bbe6aeb40423
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="mariadb-mysql-cluster-azure-tutorial"></a>MariaDB (MySQL) clusteru: kurz pro Azure
> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Azure Resource Manager](../../../resource-manager-deployment-model.md) a classic. Tento článek se týká modelu nasazení classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model hello Azure Resource Manager.

> [!NOTE]
> MariaDB Enterprise clusteru je teď dostupná v hello Azure Marketplace. Nová nabídka Hello automaticky nasadí cluster MariaDB Galera na Azure Resource Manager. Měli byste použít novou nabídku hello z [Azure Marketplace](https://azure.microsoft.com/en-us/marketplace/partners/mariadb/cluster-maxscale/).
>
>

Tento článek ukazuje, jak toocreate více hlavní [Galera](http://galeracluster.com/products/) cluster [MariaDBs](https://mariadb.org/en/about/) (robustní, škálovatelnou a spolehlivé drop-in nahrazení pro databázi MySQL) toowork v prostředí s vysokou dostupností v Azure virtuální počítače.

## <a name="architecture-overview"></a>Přehled architektury
Tento článek popisuje, jak toocomplete hello následující kroky:

- Vytvořte tři uzly clusteru.
- Samostatné hello datových disků z hello disk operačního systému.
- Vytvoření hello datové disky v tooincrease RAID-0 nebo rozdělená nastavení IOPS.
- Funkci Vyrovnávání zatížení Azure toobalance hello zatížení pro hello tři uzly.
- toominimize opakovaných fungovat, vytvořte image virtuálního počítače, který obsahuje MariaDB + Galera a použít ho toocreate hello jiných clusteru virtuálních počítačů.

![Architektura systému](./media/mariadb-mysql-cluster/Setup.png)

> [!NOTE]
> Toto téma používá hello [rozhraní příkazového řádku Azure](../../../cli-install-nodejs.md) nástroje, tak zkontrolujte, zda toodownload je a jejich připojení pokyny podle toohello tooyour předplatného Azure. Pokud potřebujete příkazy toohello referenční dokumentace, která je k dispozici v hello příkazového řádku Azure CLI, přečtěte si hello [reference k příkazům rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2). Budete také potřebovat příliš[vytvoření klíče SSH pro ověřování] a poznamenejte si umístění souboru .pem hello.
>
>

## <a name="create-hello-template"></a>Vytvoření šablony hello
### <a name="infrastructure"></a>Infrastruktura
1. Vytvořte na skupinu vztahů toohold hello prostředky společně.

        azure account affinity-group create mariadbcluster --location "North Europe" --label "MariaDB Cluster"
2. Vytvoření virtuální sítě.

        azure network vnet create --address-space 10.0.0.0 --cidr 8 --subnet-name mariadb --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --affinity-group mariadbcluster mariadbvnet
3. Vytvořte všechny naše disky toohost účet úložiště. Více než 40 vytíženou disky by neměly umístit na hello stejné tooavoid účet úložiště nedosáhli limitu účet úložiště IOPS hello 20 000. V takovém případě jste dobře nižší než toto omezení, takže všechno, co budete uložit na stejný účet pro jednoduchost hello.

        azure storage account create mariadbstorage --label mariadbstorage --affinity-group mariadbcluster
4. Najít název hello hello CentOS 7 bitovou kopii virtuálního počítače.

        azure vm image list | findstr CentOS
   výstup Hello se něco podobného jako `5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926`.

   Použijte tento název v hello následující krok.
5. Vytvořit šablonu virtuálního počítače hello a nahraďte /path/to/key.pem hello cestu, kde je uložený hello vygenerovat klíč SSH .pem.

        azure vm create --virtual-network-name mariadbvnet --subnet-names mariadb --blob-url "http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-os.vhd"  --vm-size Medium --ssh 22 --ssh-cert "/path/to/key.pem" --no-ssh-password mariadbtemplate 5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926 azureuser
6. Připojte čtyři 500 GB dat toohello disky virtuálních počítačů pro použití v konfiguraci RAID hello.

        FOR /L %d IN (1,1,4) DO azure vm disk attach-new mariadbhatemplate 512 http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-data-%d.vhd
7. Použití SSH toosign v toohello šablony virtuálního počítače, který jste vytvořili na mariadbhatemplate.cloudapp.net:22 a připojte se pomocí soukromého klíče.

### <a name="software"></a>Software
1. Získejte kořenový hello.

        sudo su

2. Instalace podpory RAID:

    a. Nainstalujte mdadm.

              yum install mdadm

    b. Vytvořte konfiguraci 0/stripe hello EXT4 systémem souborů.

              mdadm --create --verbose /dev/md0 --level=stripe --raid-devices=4 /dev/sdc /dev/sdd /dev/sde /dev/sdf
              mdadm --detail --scan >> /etc/mdadm.conf
              mkfs -t ext4 /dev/md0
    c. Vytvoření adresáře hello přípojného bodu.

              mkdir /mnt/data
    d. Načtěte hello UUID hello nově vytvořený RAID zařízení.

              blkid | grep /dev/md0
    e. Upravte /etc/fstab.

              vi /etc/fstab
    f. Přidat hello zařízení tooenable automatické připojení na restartování, nahraďte hodnotou hello hello UUID získat z předchozích hello **blkid** příkaz.

              UUID=<UUID FROM PREVIOUS>   /mnt/data ext4   defaults,noatime   1 2
    g. Připojte nový oddíl hello.

              mount /mnt/data

3. Nainstalujte MariaDB.

    a. Vytvořte soubor MariaDB.repo hello.

                vi /etc/yum.repos.d/MariaDB.repo

    b. Zadejte soubor úložišti hello s hello následující obsah:

              [mariadb]
              name = MariaDB
              baseurl = http://yum.mariadb.org/10.0/centos7-amd64
              gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
              gpgcheck=1
    c. tooavoid konfliktů, odeberte existující operátory a mariadb knihovny.

           yum remove postfix mariadb-libs-*
    d. Nainstalujte MariaDB s Galera.

           yum install MariaDB-Galera-server MariaDB-client galera

4. Přesuňte hello MySQL dat adresáře toohello RAID bloku zařízení.

    a. Zkopírujte aktuální adresář MySQL hello do nového umístění a odebrat adresář staré hello.

           cp -avr /var/lib/mysql /mnt/data  
           rm -rf /var/lib/mysql
    b. Nastavte oprávnění pro nový adresář hello odpovídajícím způsobem.

           chown -R mysql:mysql /mnt/data && chmod -R 755 /mnt/data/

    c. Vytvořte symlink, který odkazuje hello staré toohello nové umístění adresáře na hello RAID oddílu.

           ln -s /mnt/data/mysql /var/lib/mysql

5. Protože [SELinux naruší operace clusteru hello](http://galeracluster.com/documentation-webpages/configuration.html#selinux), je nutné toodisable pro hello aktuální relaci. Upravit `/etc/selinux/config` toodisable pro následné restartování.

            setenforce 0

            then editing `/etc/selinux/config` tooset `SELINUX=permissive`
6. Ověření spuštění MySQL.

   a. Spusťte MySQL.

           service mysql start
   b. Zabezpečení instalace hello MySQL, nastavit hello kořenové heslo, odeberte anonymní uživatelé toodisable kořenové vzdálené přihlášení a odebrání hello testovací databáze.

           mysql_secure_installation
   c. Vytvořte uživatele na hello databáze pro operace clusteru a volitelně pro vaše aplikace.

           mysql -u root -p
           GRANT ALL PRIVILEGES ON *.* too'cluster'@'%' IDENTIFIED BY 'p@ssw0rd' WITH GRANT OPTION; FLUSH PRIVILEGES;
           exit

   d. Zastavte MySQL.

            service mysql stop
7. Vytvoření konfigurace zástupný symbol.

   a. Upravte hello MySQL konfigurace toocreate zástupný symbol pro nastavení clusteru hello. Nepřepisovat existující hello  **`<Variables>`**  nebo zrušte komentář u teď. Který se stane po vytvoření virtuálního počítače z této šablony.

            vi /etc/my.cnf.d/server.cnf
   b. Upravit hello  **[galera]**  části a vyčistit ho.

   c. Upravit hello **[mariadb]** části.

           wsrep_provider=/usr/lib64/galera/libgalera_smm.so
           binlog_format=ROW
           wsrep_sst_method=rsync
           bind-address=0.0.0.0 # When set too0.0.0.0, hello server listens tooremote connections
           default_storage_engine=InnoDB
           innodb_autoinc_lock_mode=2

           wsrep_sst_auth=cluster:p@ssw0rd # CHANGE: Username and password you created for hello SST cluster MySQL user
           #wsrep_cluster_name='mariadbcluster' # CHANGE: Uncomment and set your desired cluster name
           #wsrep_cluster_address="gcomm://mariadb1,mariadb2,mariadb3" # CHANGE: Uncomment and Add all your servers
           #wsrep_node_address='<ServerIP>' # CHANGE: Uncomment and set IP address of this server
           #wsrep_node_name='<NodeName>' # CHANGE: Uncomment and set hello node name of this server
8. Otevřené požadované porty v bráně firewall hello pomocí FirewallD na CentOS 7.

   * MySQL:`firewall-cmd --zone=public --add-port=3306/tcp --permanent`
   * GALERA:`firewall-cmd --zone=public --add-port=4567/tcp --permanent`
   * GALERA IST:`firewall-cmd --zone=public --add-port=4568/tcp --permanent`
   * RSYNC:`firewall-cmd --zone=public --add-port=4444/tcp --permanent`
   * Znovu načtete hello brány firewall:`firewall-cmd --reload`

9. Optimalizujte hello systému pro výkon. Další informace najdete v tématu [strategie ladění výkonu](optimize-mysql.md).

   a. Upravte konfigurační soubor MySQL hello znovu.

            vi /etc/my.cnf.d/server.cnf
   b. Upravit hello **[mariadb]** části a připojte hello následující obsah:

   > [!NOTE]
   > Doporučujeme, abyste tento innodb\_vyrovnávací paměti\_pool_size je 70 procent paměti Virtuálního počítače. V tomto příkladu ho je nastavená na 2.45 GB pro střední hello virtuální počítač Azure s 3.5 GB paměti RAM.
   >
   >

           innodb_buffer_pool_size = 2508M # hello buffer pool contains buffered data and hello index. This is usually set too70 percent of physical memory.
           innodb_log_file_size = 512M #  Redo logs ensure that write operations are fast, reliable, and recoverable after a crash
           max_connections = 5000 # A larger value will give hello server more time toorecycle idled connections
           innodb_file_per_table = 1 # Speed up hello table space transmission and optimize hello debris management performance
           innodb_log_buffer_size = 128M # hello log buffer allows transactions toorun without having tooflush hello log toodisk before hello transactions commit
           innodb_flush_log_at_trx_commit = 2 # hello setting of 2 enables hello most data integrity and is suitable for Master in MySQL cluster
           query_cache_size = 0
10. Zastavit MySQL, zakázat službu MySQL na spuštění tooavoid přerušení hello clusteru při přidávání uzlu spustit a zrušit jejich zřízení počítače hello.

        service mysql stop
        chkconfig mysql off
        waagent -deprovision
11. Zaznamenejte hello virtuálního počítače přes portál hello. (V současné době [vydání #1268 v nástrojích příkazového řádku Azure CLI hello](https://github.com/Azure/azure-xplat-cli/issues/1268) popisuje hello fakt, že obrazů z nástrojů příkazového řádku Azure hello není zachycen hello připojené datových disků.)

    a. Vypněte počítač hello prostřednictvím portálu hello.

    b. Klikněte na tlačítko **zaznamenat** a zadejte název bitové kopie hello jako **mariadb-galera image**. Zadejte popis a zkontrolujte "I jste spustili příkaz waagent."
      
      ![Zachytit virtuální počítač hello](./media/mariadb-mysql-cluster/Capture2.PNG)

## <a name="create-hello-cluster"></a>Vytvoření clusteru hello
Vytvořte tři virtuální počítače pomocí šablony hello vytvořili a potom nakonfigurovat a spustit hello clusteru.

1. Vytvořte hello první virtuální počítač CentOS 7 z hello mariadb-galera-image bitovou kopii, kterou jste vytvořili, poskytuje hello následující informace:

 - Název virtuální sítě: mariadbvnet
 - Podsítě: mariadb
 - Počítač velikost: střední
 - Název cloudové služby: mariadbha (nebo jakýkoli název chcete přistupovat prostřednictvím mariadbha.cloudapp.net toobe)
 - Název počítače: mariadb1
 - Uživatelské jméno: azureuser
 - Přístup SSH: povoleno
 - Předávání soubor .pem certifikátu hello SSH a nahraďte /path/to/key.pem hello cestu, kam jste uložili hello vygenerovat klíč SSH .pem.

   > [!NOTE]
   > Hello následující příkazy jsou rozděleny na více řádků pro přehlednost, ale měli zadejte každou na jeden řádek.
   >
   >
        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 22
        --vm-name mariadb1
        mariadbha mariadb-galera-image azureuser
2. Vytvořte dva další virtuální počítače propojením toohello mariadbha cloudové služby. Změnit název virtuálního počítače hello a hello port tooa jedinečný portu SSH není v konfliktu se ostatní virtuální počítače v hello stejné cloudové služby.

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 23
        --vm-name mariadb2
        --connect mariadbha mariadb-galera-image azureuser
  Pro MariaDB3:

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 24
        --vm-name mariadb3
        --connect mariadbha mariadb-galera-image azureuser
3. Budete potřebovat tooget hello interní IP adresu jednotlivých virtuálních počítačů hello tři hello další krok:

    ![Získání IP adresy](./media/mariadb-mysql-cluster/IP.png)
4. Použití SSH toosign v toohello tři virtuální počítače a upravovat soubor konfigurace hello na každý z nich.

        sudo vi /etc/my.cnf.d/server.cnf

    Zrušením komentáře u  **`wsrep_cluster_name`**  a  **`wsrep_cluster_address`**  odebráním hello  **#**  od začátku hello hello řádku.
    Kromě toho nahradit  **`<ServerIP>`**  v  **`wsrep_node_address`**  a  **`<NodeName>`**  v  **`wsrep_node_name`**  s hello Virtuálního počítače IP adres a name, a zrušte komentář u také tyto řádky.
5. Spusťte hello clusteru na MariaDB1 a nechat ji spustit při spuštění.

        sudo service mysql bootstrap
        chkconfig mysql on
6. Spusťte MySQL na MariaDB2 a MariaDB3 a nechat ji spustit při spuštění.

        sudo service mysql start
        chkconfig mysql on

## <a name="load-balance-hello-cluster"></a>Cluster hello Vyrovnávání zatížení
Při vytváření virtuálních počítačů hello v clusteru byly přidány do skupiny dostupnosti názvem clusteravset tooensure, jejich umístění v různých doménách selhání a aktualizace, a že Azure nikdy nemá údržby na všech počítačích najednou. Tato konfigurace splňuje požadavky hello toobe nepodporuje hello smlouvu o úrovni Azure služeb (SLA).

Teď použijte nástroj pro vyrovnávání zatížení Azure toobalance požadavky mezi hello tři uzly.

Spusťte následující příkazy v počítači pomocí rozhraní příkazového řádku Azure hello hello.

Struktura parametry příkazu Hello je:`azure vm endpoint create-multiple <MachineName> <PublicPort>:<VMPort>:<Protocol>:<EnableDirectServerReturn>:<Load Balanced Set Name>:<ProbeProtocol>:<ProbePort>`

    azure vm endpoint create-multiple mariadb1 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb2 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb3 3306:3306:tcp:false:MySQL:tcp:3306

Hello rozhraní příkazového řádku nastaví hello zatížení vyrovnávání testu interval too15 sekund, což může být trochu příliš dlouhý. Změnit hello portálu v části **koncové body** pro všechny virtuální počítače hello.

![Upravit koncový bod](./media/mariadb-mysql-cluster/Endpoint.PNG)

Vyberte **Reconfigure hello architektuře s vyrovnáváním zatížení nastavit**.

![Překonfigurujte hello-s vyrovnáváním zatížení](./media/mariadb-mysql-cluster/Endpoint2.PNG)

Změna **Interval sběru dat** too5 sekund a uložte změny.

![Interval kontroly změn](./media/mariadb-mysql-cluster/Endpoint3.PNG)

## <a name="validate-hello-cluster"></a>Ověření clusteru hello
provádí náročné práce Hello. Hello clusteru musí být nyní přístupná na `mariadbha.cloudapp.net:3306`, který dotkne hello nástroj pro vyrovnávání zatížení a požadavky na směrování mezi hello tři virtuální počítače efektivně a bez obtíží.

Použijte váš oblíbený klienta tooconnect MySQL, nebo se připojte z jednoho z tooverify hello virtuální počítače, který pracuje v tomto clusteru.

     mysql -u cluster -h mariadbha.cloudapp.net -p

Pak vytvořte databázi a naplnit určitými daty.

    CREATE DATABASE TestDB;
    USE TestDB;
    CREATE TABLE TestTable (id INT NOT NULL AUTO_INCREMENT PRIMARY KEY, value VARCHAR(255));
    INSERT INTO TestTable (value)  VALUES ('Value1');
    INSERT INTO TestTable (value)  VALUES ('Value2');
    SELECT * FROM TestTable;

Hello databáze, kterou jste vytvořili vrátí hello následující tabulka:

    +----+--------+
    | id | value  |
    +----+--------+
    |  1 | Value1 |
    |  4 | Value2 |
    +----+--------+
    2 rows in set (0.00 sec)

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Další kroky
V tomto článku jste vytvořili tři uzly MariaDB + Galera vysoce dostupný cluster v Azure virtuální počítače spuštěné CentOS 7. virtuální počítače, Hello jsou zatížení vyrovnávaném pomocí vyrovnávání zatížení Azure.

Můžete chtít toolook v [jiný způsob toocluster MySQL v systému Linux](mysql-cluster.md) a způsoby příliš[optimalizace a testování výkonu databáze MySQL na virtuálních počítačích Azure Linux](optimize-mysql.md).

<!--Anchors-->
[Architecture overview]:#architecture-overview
[Creating hello template]:#creating-the-template
[Creating hello cluster]:#creating-the-cluster
[Load balancing hello cluster]:#load-balancing-the-cluster
[Validating hello cluster]:#validating-the-cluster
[Next steps]:#next-steps

<!--Image references-->

<!--Link references-->
[Galera]:http://galeracluster.com/products/
[MariaDBs]:https://mariadb.org/en/about/
[vytvoření klíče SSH pro ověřování]:http://www.jeff.wilcox.name/2013/06/secure-linux-vms-with-ssh-certificates/
[issue #1268 in hello Azure CLI]:https://github.com/Azure/azure-xplat-cli/issues/1268
