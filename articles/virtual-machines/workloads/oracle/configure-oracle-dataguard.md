---
title: "aaaImplement Oracle Data Guard na virtuálním počítači Azure Linux | Microsoft Docs"
description: "Rychle získáte Oracle Data Guard nahoru a spouštění v prostředí Azure."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: v-shiuma
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: rclaus
ms.openlocfilehash: 6bb530098737e3ca7dd8bab3f4306ecbb620f3f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="implement-oracle-data-guard-on-an-azure-linux-virtual-machine"></a>Implementace Oracle Data Guard na virtuálním počítači Azure Linux 

Rozhraní příkazového řádku Azure je použité toocreate a spravovat prostředky Azure z hello příkazového řádku nebo ve skriptech. Tento článek popisuje, jak toodeploy toouse rozhraní příkazového řádku Azure k databázi Oracle 12c databáze z Azure Marketplace hello bitové kopie. Tento článek pak ukazuje, krok za krokem, jak tooinstall a nakonfigurovat ochranu dat na virtuální počítač Azure (VM).

Než začnete, ujistěte se, že je nainstalované rozhraní příkazového řádku Azure. Další informace najdete v tématu hello [Průvodce instalací Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="prepare-hello-environment"></a>Příprava prostředí hello
### <a name="assumptions"></a>Předpoklady

tooinstall Oracle Data Guard, budete potřebovat toocreate dva virtuální počítače Azure na hello stejné skupině dostupnosti:

- Hello primárního virtuálního počítače (myVM1) má spuštěnou instanci Oracle.
- Hello pohotovostní virtuálních počítačů (Můjvp2) je nainstalován pouze software Oracle hello.

Hello Marketplace image použít toocreate hello virtuálních počítačů je Oracle: Oracle – databáze-Ee:12.1.0.2:latest.

### <a name="sign-in-tooazure"></a>Přihlaste se tooAzure 

Přihlaste se pomocí hello tooyour předplatného Azure [az přihlášení](/cli/azure/#login) příkazů a postupujte podle hello na obrazovce pokynů.

```azurecli
az login
```

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků s použitím hello [vytvořit skupinu az](/cli/azure/group#create) příkaz. Skupinu prostředků Azure je logický kontejner, ve které jsou nasazené a spravované prostředky. 

Hello následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v hello `westus` umístění:

```azurecli
az group create --name myResourceGroup --location westus
```

### <a name="create-an-availability-set"></a>Vytvoření skupiny dostupnosti

Vytvoření skupiny dostupnosti je nepovinný, ale doporučujeme ho. Další informace najdete v tématu [sady Azure dostupnosti pokyny](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines).

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

### <a name="create-a-virtual-machine"></a>Vytvoření virtuálního počítače

Vytvoření virtuálního počítače pomocí hello [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz. 

Hello následující příklad vytvoří dva virtuální počítače s názvem `myVM1` a `myVM2`. Pokud už neexistují v umístění klíče výchozí klíče SSH, vytvoří se také. toouse konkrétní nastavení klíčů, použijte hello `--ssh-key-value` možnost.

Vytvořte myVM1 (primární):
```azurecli
az vm create \
     --resource-group myResourceGroup \
     --name myVM1 \
     --availability-set myAvailabilitySet \
     --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
     --size Standard_DS1_v2  \
     --admin-username azureuser \
     --generate-ssh-keys \
```

Po vytvoření hello virtuálních počítačů, rozhraní příkazového řádku Azure znázorňuje následující ukázka podobné toohello informace. Poznamenejte si hodnotu hello `publicIpAddress`. Můžete použít tuto adresu tooaccess hello virtuálních počítačů.

```azurecli
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "westus",
  "macAddress": "00-0D-3A-36-2F-56",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.64.104.241",
  "resourceGroup": "myResourceGroup"
}
```

Vytvoření Můjvp2 (pohotovostní):
```azurecli
az vm create \
     --resource-group myResourceGroup \
     --name myVM2 \
     --availability-set myAvailabilitySet \
     --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
     --size Standard_DS1_v2  \
     --admin-username azureuser \
     --generate-ssh-keys \
```

Poznamenejte si hodnotu hello `publicIpAddress` po vytvoření Můjvp2.

### <a name="open-hello-tcp-port-for-connectivity"></a>Otevřete port TCP hello pro připojení k síti

Tento krok nakonfiguruje externí koncové body, které umožňují databáze Oracle toohello vzdáleného přístupu.

Otevřete port hello pro myVM1:

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVM1NSG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

výsledek Hello by měl vypadat podobně jako toohello následující odpověď:

```bash
{
  "access": "Allow",
  "description": null,
  "destinationAddressPrefix": "*",
  "destinationPortRange": "1521",
  "direction": "Inbound",
  "etag": "W/\"bd77dcae-e5fd-4bd6-a632-26045b646414\"",
  "id": "/subscriptions/<subscription-id>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myVmNSG/securityRules/allow-oracle",
  "name": "allow-oracle",
  "priority": 999,
  "protocol": "Tcp",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sourceAddressPrefix": "*",
  "sourcePortRange": "*"
}
```

Otevřete port hello pro Můjvp2:

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVM2NSG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

### <a name="connect-toohello-virtual-machine"></a>Připojit toohello virtuálního počítače

Použití hello následující příkaz toocreate na relace SSH s hello virtuálního počítače. Nahraďte IP adresu hello hello `publicIpAddress` hodnotu pro virtuální počítač.

```bash 
$ ssh azureuser@<publicIpAddress>
```

### <a name="create-hello-database-on-myvm1-primary"></a>Vytvořit databázi hello na myVM1 (primární)

Hello Oracle softwaru je již nainstalován na bitovou kopii Marketplace hello, takže hello dalším krokem je tooinstall hello databáze. 

Superuživatel Oracle toohello přepínače:

```bash
$ sudo su - oracle
```

Vytvořte databázi hello:

```bash
$ dbca -silent \
   -createDatabase \
   -templateName General_Purpose.dbc \
   -gdbname cdb1 \
   -sid cdb1 \
   -responseFile NO_VALUE \
   -characterSet AL32UTF8 \
   -sysPassword OraPasswd1 \
   -systemPassword OraPasswd1 \
   -createAsContainerDatabase true \
   -numberOfPDBs 1 \
   -pdbName pdb1 \
   -pdbAdminPassword OraPasswd1 \
   -databaseType MULTIPURPOSE \
   -automaticMemoryManagement false \
   -storageType FS \
   -ignorePreReqs
```
Výstupy by měl vypadat podobně jako toohello následující odpověď:

```bash
Copying database files
1% complete
2% complete
8% complete
13% complete
19% complete
27% complete
Creating and starting Oracle instance
29% complete
32% complete
33% complete
34% complete
38% complete
42% complete
43% complete
45% complete
Completing Database Creation
48% complete
51% complete
53% complete
62% complete
70% complete
72% complete
Creating Pluggable Databases
78% complete
100% complete
Look at hello log file "/u01/app/oracle/cfgtoollogs/dbca/cdb1/cdb1.log" for further details.
```

Nastavení proměnných ORACLE_SID a ORACLE_HOME hello:

```bash
$ ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
$ ORACLE_SID=cdb1; export ORACLE_SID
```

Volitelně můžete přidat ORACLE_HOME a ORACLE_SID toohello /home/oracle/.bashrc souboru, tak, aby tato nastavení se uloží pro budoucí přihlášení:

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=cdb1
```

## <a name="configure-data-guard"></a>Nakonfigurujte ochranu dat

### <a name="enable-archive-log-mode-on-myvm1-primary"></a>Povolit režim protokolu archiv na myVM1 (primární)

```bash
$ sqlplus / as sysdba
SQL> SELECT log_mode FROM v$database;

LOG_MODE
------------
NOARCHIVELOG

SQL> SHUTDOWN IMMEDIATE;
SQL> STARTUP MOUNT;
SQL> ALTER DATABASE ARCHIVELOG;
SQL> ALTER DATABASE OPEN;
```
Povolit protokolování platnost a ujistěte se, zda je přítomen alespoň jeden soubor protokolu:

```bash
SQL> ALTER DATABASE FORCE LOGGING;
SQL> ALTER SYSTEM SWITCH LOGFILE;
```

Vytvořte znovu pohotovostní protokoly:

```bash
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo01.log') SIZE 50M;
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo02.log') SIZE 50M;
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo03.log') SIZE 50M;
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo04.log') SIZE 50M;
```

Zapnout Flashback (díky čemuž obnovení bylo mnohem snazší) a nastavte pohotovostní\_soubor\_tooauto správy. Ukončete SQL * Plus potom.

```bash
SQL> ALTER DATABASE FLASHBACK ON;
SQL> ALTER SYSTEM SET STANDBY_FILE_MANAGEMENT=AUTO;
SQL> EXIT;
```

### <a name="set-up-service-on-myvm1-primary"></a>Nastavení služby v myVM1 (primární)

Upravovat nebo vytvářet hello tnsnames.ora souboru, který je ve složce ORACLE_HOME\network\admin $ hello.

Přidejte hello následující položky:

```bash
cdb1 =
  (DESCRIPTION =
    (ADDRESS_LIST =
      (ADDRESS = (PROTOCOL = TCP)(HOST = myVM1)(PORT = 1521))
    )
    (CONNECT_DATA =
      (SID = cdb1)
    )
  )

cdb1_stby =
  (DESCRIPTION =
    (ADDRESS_LIST =
      (ADDRESS = (PROTOCOL = TCP)(HOST = myVM2)(PORT = 1521))
    )
    (CONNECT_DATA =
      (SID = cdb1)
    )
  )
```

Upravovat nebo vytvářet hello listener.ora souboru, který je ve složce ORACLE_HOME\network\admin $ hello.

Přidejte hello následující položky:

```bash
LISTENER =
  (DESCRIPTION_LIST =
    (DESCRIPTION =
      (ADDRESS = (PROTOCOL = TCP)(HOST = myVM1)(PORT = 1521))
      (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
    )
  )

SID_LIST_LISTENER =
  (SID_LIST =
    (SID_DESC =
      (GLOBAL_DBNAME = cdb1_DGMGRL)
      (ORACLE_HOME = /u01/app/oracle/product/12.1.0/dbhome_1)
      (SID_NAME = cdb1)
    )
  )

ADR_BASE_LISTENER = /u01/app/oracle
```

Povolte zprostředkovatele ochrana dat:
```bash
$ sqlplus / as sysdba
SQL> ALTER SYSTEM SET dg_broker_start=true;
SQL> EXIT;
```
Spuštění nástroje pro sledování hello:

```bash
$ lsnrctl stop
$ lsnrctl start
```

### <a name="set-up-service-on-myvm2-standby"></a>Nastavení služby v Můjvp2 (pohotovostní)

SSH toomyVM2:

```bash 
$ ssh azureuser@<publicIpAddress>
```

Přihlaste se jako Oracle:

```bash
$ sudo su - oracle
```

Upravovat nebo vytvářet hello tnsnames.ora souboru, který je ve složce ORACLE_HOME\network\admin $ hello.

Přidejte hello následující položky:

```bash
cdb1 =
  (DESCRIPTION =
    (ADDRESS_LIST =
      (ADDRESS = (PROTOCOL = TCP)(HOST = myVM1)(PORT = 1521))
    )
    (CONNECT_DATA =
      (SID = cdb1)
    )
  )

cdb1_stby =
  (DESCRIPTION =
    (ADDRESS_LIST =
      (ADDRESS = (PROTOCOL = TCP)(HOST = myVM2)(PORT = 1521))
    )
    (CONNECT_DATA =
      (SID = cdb1)
    )
  )
```

Upravovat nebo vytvářet hello listener.ora souboru, který je ve složce ORACLE_HOME\network\admin $ hello.

Přidejte hello následující položky:

```bash
LISTENER =
  (DESCRIPTION_LIST =
    (DESCRIPTION =
      (ADDRESS = (PROTOCOL = TCP)(HOST = myVM2)(PORT = 1521))
      (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
    )
  )

SID_LIST_LISTENER =
  (SID_LIST =
    (SID_DESC =
      (GLOBAL_DBNAME = cdb1_DGMGRL)
      (ORACLE_HOME = /u01/app/oracle/product/12.1.0/dbhome_1)
      (SID_NAME = cdb1)
    )
  )

ADR_BASE_LISTENER = /u01/app/oracle
```

Spuštění nástroje pro sledování hello:

```bash
$ lsnrctl stop
$ lsnrctl start
```


### <a name="restore-hello-database-toomyvm2-standby"></a>Obnovení databáze toomyVM2 hello (pohotovostní)

Vytvořte hello parametr souboru /tmp/initcdb1_stby.ora s hello následující obsah:
```bash
*.db_name='cdb1'
```

Vytváření složek:

```bash
mkdir -p /u01/app/oracle/oradata/cdb1/pdbseed
mkdir -p /u01/app/oracle/oradata/cdb1/pdb1
mkdir -p /u01/app/oracle/fast_recovery_area/cdb1
mkdir -p /u01/app/oracle/admin/cdb1/adump
```

Vytvořte soubor heslo:

```bash
$ orapwd file=/u01/app/oracle/product/12.1.0/dbhome_1/dbs/orapwcdb1 password=OraPasswd1 entries=10
```
Spuštění databáze hello na Můjvp2:

```bash
$ export ORACLE_SID=cdb1
$ sqlplus / as sysdba

SQL> STARTUP NOMOUNT PFILE='/tmp/initcdb1_stby.ora';
SQL> EXIT;
```

Obnovení databáze hello pomocí nástroje RMAN hello:

```bash
$ rman TARGET sys/OraPasswd1@cdb1 AUXILIARY sys/OraPasswd1@cdb1_stby
```

Spusťte následující příkazy v RMAN hello:
```bash
DUPLICATE TARGET DATABASE
  FOR STANDBY
  FROM ACTIVE DATABASE
  DORECOVER
  SPFILE
    SET db_unique_name='CDB1_STBY' COMMENT 'Is standby'
  NOFILENAMECHECK;
```

Po dokončení příkazu hello byste měli vidět následující toohello podobné zprávy. Ukončete RMAN.
```bash
media recovery complete, elapsed time: 00:00:00
Finished recover at 29-JUN-17
Finished Duplicate Db at 29-JUN-17

RMAN> EXIT;
```

Volitelně můžete přidat ORACLE_HOME a ORACLE_SID toohello /home/oracle/.bashrc souboru, tak, aby tato nastavení se uloží pro budoucí přihlášení:

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=cdb1
```

Povolte zprostředkovatele ochrana dat:
```bash
$ sqlplus / as sysdba
SQL> ALTER SYSTEM SET dg_broker_start=true;
SQL> EXIT;
```

### <a name="configure-data-guard-broker-on-myvm1-primary"></a>Konfigurace zprostředkovatele ochrana dat na myVM1 (primární)

Spusťte správce ochrana dat a přihlaste se pomocí SYS a heslo. (Nepoužívejte ověřování operačního systému.) Proveďte následující hello:

```bash
$ dgmgrl sys/OraPasswd1@cdb1
DGMGRL for Linux: Version 12.1.0.2.0 - 64bit Production

Copyright (c) 2000, 2013, Oracle. All rights reserved.

Welcome tooDGMGRL, type "help" for information.
Connected as SYSDBA.
DGMGRL> CREATE CONFIGURATION my_dg_config AS PRIMARY DATABASE IS cdb1 CONNECT IDENTIFIER IS cdb1;
Configuration "my_dg_config" created with primary database "cdb1"
DGMGRL> ADD DATABASE cdb1_stby AS CONNECT IDENTIFIER IS cdb1_stby MAINTAINED AS PHYSICAL;
Database "cdb1_stby" added
DGMGRL> ENABLE CONFIGURATION;
Enabled.
```

Kontrola konfigurace hello:
```bash
DGMGRL> SHOW CONFIGURATION;

Configuration - my_dg_config

  Protection Mode: MaxPerformance
  Members:
  cdb1      - Primary database
    cdb1_stby - Physical standby database

Fast-Start Failover: DISABLED

Configuration Status:
SUCCESS   (status updated 26 seconds ago)
```

Po dokončení instalace Oracle Data Guard hello. Hello další části se dozvíte, jak tootest hello připojení a přepínačů.

### <a name="connect-hello-database-from-hello-client-machine"></a>Připojit databáze hello z hello klientský počítač

Aktualizujte nebo vytvoření souboru tnsnames.ora hello v klientském počítači. Tento soubor je obvykle v $ORACLE_HOME\network\admin.

Nahraďte hello IP adresy s vaší `publicIpAddress` hodnoty myVM1 a Můjvp2:

```bash
cdb1=
  (DESCRIPTION=
    (ADDRESS=
      (PROTOCOL=TCP)
      (HOST=<myVM1 IP address>)
      (PORT=1521)
    )
    (CONNECT_DATA=
      (SERVER=dedicated)
      (SERVICE_NAME=cdb1)
    )
  )

cdb1_stby=
  (DESCRIPTION=
    (ADDRESS=
      (PROTOCOL=TCP)
      (HOST=<myVM2 IP address>)
      (PORT=1521)
    )
    (CONNECT_DATA=
      (SERVER=dedicated)
      (SERVICE_NAME=cdb1_stby)
    )
  )
```

Spusťte SQL * Plus:

```bash
$ sqlplus sys/OraPasswd1@cdb1
SQL*Plus: Release 12.2.0.1.0 Production on Wed May 10 14:18:31 2017

Copyright (c) 1982, 2016, Oracle.  All rights reserved.

Connected to:
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With hello Partitioning, OLAP, Advanced Analytics and Real Application Testing options

SQL>
```
## <a name="test-hello-data-guard-configuration"></a>Konfigurace testovací hello Data Guard

### <a name="switch-over-hello-database-on-myvm1-primary"></a>Přepínač přes hello databáze na myVM1 (primární)

tooswitch z primární toostandby (cdb1 toocdb1_stby):

```bash
$ dgmgrl sys/OraPasswd1@cdb1
DGMGRL for Linux: Version 12.1.0.2.0 - 64bit Production

Copyright (c) 2000, 2013, Oracle. All rights reserved.

Welcome tooDGMGRL, type "help" for information.
Connected as SYSDBA.
DGMGRL> SWITCHOVER toocdb1_stby;
Performing switchover NOW, please wait...
Operation requires a connection tooinstance "cdb1" on database "cdb1_stby"
Connecting tooinstance "cdb1"...
Connected as SYSDBA.
New primary database "cdb1_stby" is opening...
Operation requires start up of instance "cdb1" on database "cdb1"
Starting instance "cdb1"...
ORACLE instance started.
Database mounted.
Switchover succeeded, new primary is "cdb1_stby"
DGMGRL>
```

Teď se můžete připojit toohello pohotovostní databáze.

Spusťte SQL * Plus:

```bash

$ sqlplus sys/OraPasswd1@cdb1_stby
SQL*Plus: Release 12.2.0.1.0 Production on Wed May 10 14:18:31 2017

Copyright (c) 1982, 2016, Oracle.  All rights reserved.

Connected to:
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With hello Partitioning, OLAP, Advanced Analytics and Real Application Testing options

SQL>
```

### <a name="switch-over-hello-database-on-myvm2-standby"></a>Přepínač přes hello databáze na Můjvp2 (pohotovostní)

tooswitch přes, spusťte na Můjvp2 hello následující:
```bash
$ dgmgrl sys/OraPasswd1@cdb1_stby
DGMGRL for Linux: Version 12.1.0.2.0 - 64bit Production

Copyright (c) 2000, 2013, Oracle. All rights reserved.

Welcome tooDGMGRL, type "help" for information.
Connected as SYSDBA.
DGMGRL> SWITCHOVER toocdb1;
Performing switchover NOW, please wait...
Operation requires a connection tooinstance "cdb1" on database "cdb1"
Connecting tooinstance "cdb1"...
Connected as SYSDBA.
New primary database "cdb1" is opening...
Operation requires start up of instance "cdb1" on database "cdb1_stby"
Starting instance "cdb1"...
ORACLE instance started.
Database mounted.
Switchover succeeded, new primary is "cdb1"
```

Znovu nyní měli být schopný tooconnect toohello primární databáze.

Spusťte SQL * Plus:

```bash

$ sqlplus sys/OraPasswd1@cdb1
SQL*Plus: Release 12.2.0.1.0 Production on Wed May 10 14:18:31 2017

Copyright (c) 1982, 2016, Oracle.  All rights reserved.

Connected to:
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With hello Partitioning, OLAP, Advanced Analytics and Real Application Testing options

SQL>
```

Dokončili jste hello instalaci a konfiguraci Data Guard na Oracle Linux.


## <a name="delete-hello-virtual-machine"></a>Odstranění hello virtuálního počítače

Když už nebude potřebovat hello virtuálních počítačů, můžete použít následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello:

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Další kroky

[Kurz: Vytvoření vysoce dostupné virtuální počítače](../../linux/create-cli-complete.md)

[Prozkoumejte ukázky rozhraní příkazového řádku Azure nasazení virtuálních počítačů](../../linux/cli-samples.md)
