---
title: "aaaImplement Oracle Golden brány na virtuální počítač Azure Linux | Microsoft Docs"
description: "Rychle získáte bránou Golden Oracle nahoru a spouštění v prostředí Azure."
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
ms.date: 05/19/2017
ms.author: rclaus
ms.openlocfilehash: 320cafd5d23ee472f0af9f92577bc6f432f65778
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="implement-oracle-golden-gate-on-an-azure-linux-vm"></a>Implementace Oracle Golden brány ve virtuálním počítači Azure Linux 

Hello rozhraní příkazového řádku Azure je použité toocreate a spravovat prostředky Azure z hello příkazového řádku nebo ve skriptech. Tento průvodce podrobnosti, jak toouse hello rozhraní příkazového řádku Azure toodeploy Oracle 12c databáze z bitové kopie Galerie hello Azure Marketplace. 

Tento dokument vám názorně ukáže, jak toocreate, instalaci a konfiguraci brány Golden Oracle na virtuální počítač Azure.

Než začnete, ujistěte se, že byla nainstalována rozhraní příkazového řádku Azure hello. Další informace najdete v tématu [Průvodce instalací Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="prepare-hello-environment"></a>Příprava prostředí hello

instalace brány Golden Oracle hello tooperform, je nutné toocreate dva virtuální počítače Azure na hello stejné skupině dostupnosti. Hello Marketplace image použít toocreate hello virtuálních počítačů je **Oracle: Oracle – databáze-Ee:12.1.0.2:latest**.

Také potřebujete toobe obeznámeni s Unix editor vi a mají základní znalosti o x11 (X Windows).

Hello Následuje souhrn hello prostředí konfigurace:
> 
> |  | **Primární lokalita** | **Replikace webu** |
> | --- | --- | --- |
> | **Databázi Oracle** |Oracle 12c verze 2 – (12.1.0.2) |Oracle 12c verze 2 – (12.1.0.2)|
> | **Název počítače** |myVM1 |Můjvp2 |
> | **Operační systém** |Oracle Linux 6.x |Oracle Linux 6.x |
> | **Oracle SID** |CDB1 |CDB1 |
> | **Schéma replikace** |TEST|TEST |
> | **Vlastník nebo replikovat Golden brány** |C ##GGADMIN |REPUSER |
> | **Proces Golden brány** |EXTORA |REPORA|


### <a name="sign-in-tooazure"></a>Přihlaste se tooAzure 

Přihlaste se tooyour předplatné s hello [az přihlášení](/cli/azure/#login) příkaz. Potom postupujte podle hello na obrazovce pokynů.

```azurecli
az login
```

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz. Skupinu prostředků Azure je logický kontejner, do které prostředky Azure jsou nasazeny a z které mohly být spravovány. 

Hello následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v hello `westus` umístění.

```azurecli
az group create --name myResourceGroup --location westus
```

### <a name="create-an-availability-set"></a>Vytvoření skupiny dostupnosti

Hello následující krok je volitelný, ale doporučené. Další informace najdete v tématu [Azure dostupnosti nastaví průvodce](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines).

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

### <a name="create-a-virtual-machine"></a>Vytvoření virtuálního počítače

Vytvoření virtuálního počítače s hello [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz. 

Hello následující příklad vytvoří dva virtuální počítače s názvem `myVM1` a `myVM2`. Pokud už neexistují ve výchozím umístění klíče, vytvoření klíčů SSH. toouse konkrétní nastavení klíčů, použijte hello `--ssh-key-value` možnost.

#### <a name="create-myvm1-primary"></a>Vytvořte myVM1 (primární):
```azurecli
az vm create \
     --resource-group myResourceGroup \
     --name myVM1 \
     --availability-set myAvailabilitySet \
     --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
     --size Standard_DS1_v2  \
     --generate-ssh-keys \
```

Po hello, kterou virtuální počítač byl vytvořen hello rozhraní příkazového řádku Azure znázorňuje následující ukázka podobné toohello informace. (Poznamenejte hello `publicIpAddress`. Tato adresa je použité tooaccess hello virtuálních počítačů).

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

#### <a name="create-myvm2-replicate"></a>Vytvoření Můjvp2 (Replikovat):
```azurecli
az vm create \
     --resource-group myResourceGroup \
     --name myVM2 \
     --availability-set myAvailabilitySet \
     --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
     --size Standard_DS1_v2  \
     --generate-ssh-keys \
```

Poznamenejte si hello `publicIpAddress` i po jeho vytvoření.

### <a name="open-hello-tcp-port-for-connectivity"></a>Otevřete port TCP hello pro připojení k síti

dalším krokem Hello je tooconfigure externí koncové body, které umožňují databáze Oracle hello tooaccess vzdáleně. tooconfigure hello externí koncové body, spusťte následující příkazy hello.

#### <a name="open-hello-port-for-myvm1"></a>Otevřete port hello pro myVM1:

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVm1NSG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

výsledky Hello by měl vypadat podobně jako toohello následující odpověď:

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

#### <a name="open-hello-port-for-myvm2"></a>Otevřete port hello pro Můjvp2:

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVm2NSG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

### <a name="connect-toohello-virtual-machine"></a>Připojit toohello virtuálního počítače

Použití hello následující příkaz toocreate na relace SSH s hello virtuálního počítače. Nahraďte IP adresu hello hello `publicIpAddress` virtuálního počítače.

```bash 
ssh <publicIpAddress>
```

### <a name="create-hello-database-on-myvm1-primary"></a>Vytvořit databázi hello na myVM1 (primární)

Hello Oracle softwaru je již nainstalován na bitovou kopii Marketplace hello, takže hello dalším krokem je tooinstall hello databáze. 

Spusťte hello softwaru jako superuživatele, oracle, hello:

```bash
sudo su - oracle
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
Look at hello log file "/u01/app/oracle/cfgtoollogs/dbca/cdb1/cdb1.log" for more details.
```

Nastavení proměnných ORACLE_SID a ORACLE_HOME hello.

```bash
$ ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
$ ORACLE_SID=gg1; export ORACLE_SID
$ LD_LIBRARY_PATH=ORACLE_HOME/lib; export LD_LIBRARY_PATH
```

Volitelně můžete přidat ORACLE_HOME a ORACLE_SID toohello .bashrc souboru, tak, aby tato nastavení se uloží pro budoucí přihlášení:

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=gg1
# add Oracle library path
export LD_LIBRARY_PATH=$ORACLE_HOME/lib
```

### <a name="start-oracle-listener"></a>Spuštění nástroje pro sledování Oracle
```bash
$ sudo su - oracle
$ lsnrctl start
```

### <a name="create-hello-database-on-myvm2-replicate"></a>Vytvořit databázi hello na Můjvp2 (Replikovat)

```bash
sudo su - oracle
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
Nastavení proměnných ORACLE_SID a ORACLE_HOME hello.

```bash
$ ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
$ ORACLE_SID=cdb1; export ORACLE_SID
$ LD_LIBRARY_PATH=ORACLE_HOME/lib; export LD_LIBRARY_PATH
```

Volitelně můžete přidaný ORACLE_HOME a ORACLE_SID toohello .bashrc soubor, tak, aby tato nastavení se uloží pro budoucí přihlášení.

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=cdb1
# add Oracle library path
export LD_LIBRARY_PATH=$ORACLE_HOME/lib
```

### <a name="start-oracle-listener"></a>Spuštění nástroje pro sledování Oracle
```bash
$ sudo su - oracle
$ lsnrctl start
```

## <a name="configure-golden-gate"></a>Konfigurace brány Golden 
tooconfigure Golden brány, proveďte kroky hello v této části.

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
Povolit protokolování platnost a ujistěte se, zda je přítomen alespoň jeden soubor protokolu.

```bash
SQL> ALTER DATABASE FORCE LOGGING;
SQL> ALTER SYSTEM SWITCH LOGFILE;
SQL> ALTER SYSTEM set enable_goldengate_replication=true;
SQL> ALTER PLUGGABLE DATABASE PDB1 OPEN;
SQL> ALTER SESSION SET CONTAINER=PDB1;
SQL> ALTER DATABASE ADD SUPPLEMENTAL LOG DATA;
SQL> EXIT;
```

### <a name="download-golden-gate-software"></a>Stáhnout software Golden brány
toodownload a příprava softwaru brány Golden Oracle hello, dokončení hello následující kroky:

1. Stáhnout hello **fbo_ggs_Linux_x64_shiphome.zip** soubor z hello [stránky pro stažení Oracle Golden brány](http://www.oracle.com/technetwork/middleware/goldengate/downloads/index.html). V části hello stáhnout název **12.x.x.x Oracle GoldenGate pro Oracle Linux x86-64**, měla by existovat sadu toodownload soubory .zip.

2. Po stažení hello .zip soubory tooyour klientský počítač pomocí protokolu Secure kopírování (SCP) toocopy hello soubory tooyour virtuálních počítačů:

  ```bash
  $ scp fbo_ggs_Linux_x64_shiphome.zip <publicIpAddress>:<folder>
  ```

3. Přesunout toohello soubory .zip hello **/ opt** složky. Potom změňte vlastníka hello hello souborů následujícím způsobem:

  ```bash
  $ sudo su -
  # mv <folder>/*.zip /opt
  ```

4. Rozbalte soubory hello (instalace hello Linux rozbalte nástroj, pokud ještě nejsou nainstalované):

  ```bash
  # yum install unzip
  # cd /opt
  # unzip fbo_ggs_Linux_x64_shiphome.zip
  ```

5. Změna oprávnění:

  ```bash
  # chown -R oracle:oinstall /opt/fbo_ggs_Linux_x64_shiphome
  ```

### <a name="prepare-hello-client-and-vm-toorun-x11-for-windows-clients-only"></a>Příprava klienta hello a virtuálních počítačů toorun x11 (pro pouze klienty systému Windows)
Toto je volitelný krok. Pokud používáte klienta Linux nebo už máte x11, můžete přeskočit tento krok instalace.

1. Stáhněte si PuTTY a Xming tooyour počítač se systémem Windows:

  * [Stáhněte si PuTTY](http://www.putty.org/)
  * [Stáhnout Xming](https://xming.en.softonic.com/)

2.  Po instalaci PuTTY v hello PuTTY složky (například C:\Program Files\PuTTY), spusťte puttygen.exe (generátor PuTTY klíč).

3.  V generátoru PuTTY klíče:

  - toogenerate klíč, vyberte hello **generování** tlačítko.
  - Kopírovat obsah hello hello klíče (**Ctrl + C**).
  - Vyberte hello **uložit privátní klíč** tlačítko.
  - Ignorovat upozornění hello, který se zobrazí a potom vyberte **OK**.

    ![Snímek obrazovky stránky PuTTY klíče generátor hello](./media/oracle-golden-gate/puttykeygen.png)

4.  Ve vašem virtuálním počítači spusťte tyto příkazy:

  ```bash
  # sudo su - oracle
  $ mkdir .ssh (if not already created)
  $ cd .ssh
  ```

5. Vytvořte soubor s názvem **authorized_keys**. Vložte obsah hello hello klíče v tomto souboru a potom uložte soubor hello.

  > [!NOTE]
  > Hello klíč musí obsahovat řetězec hello `ssh-rsa`. Obsah hello hello klíče musí být také, jeden řádek textu.
  >  

6. Spusťte PuTTY. V hello **kategorie** podokně, vyberte **připojení** > **SSH** > **Auth**. V hello **soubor privátního klíče pro ověřování** pole, procházet toohello klíč, který jste vygenerovali dříve.

  ![Snímek obrazovky stránky hello nastavit privátní klíč](./media/oracle-golden-gate/setprivatekey.png)

7. V hello **kategorie** podokně, vyberte **připojení** > **SSH** > **X11**. Potom vyberte hello **povolit X11 předávání** pole.

  ![Snímek obrazovky stránky povolit X11 hello](./media/oracle-golden-gate/enablex11.png)

8. V hello **kategorie** podokně přejděte příliš**relace**. Zadejte informace o hostiteli hello a potom vyberte **otevřete**.

  ![Snímek obrazovky stránky relace hello](./media/oracle-golden-gate/puttysession.png)

### <a name="install-golden-gate-software"></a>Instalovat software Golden brány

tooinstall Oracle Golden brány, dokončení hello následující kroky:

1. Přihlaste se jako oracle. (Musí být schopný toosign v aniž byste byli vyzváni k zadání hesla.) Ujistěte se, že Xming běží před zahájením instalace hello.
 
  ```bash
  $ cd /opt/fbo_ggs_Linux_x64_shiphome/Disk1
  $ ./runInstaller
  ```
2. Vyberte, Oracle GoldenGate pro databázi Oracle 12c'. Potom vyberte **Další** toocontinue.

  ![Snímek obrazovky stránky instalace vyberte Instalační program hello](./media/oracle-golden-gate/golden_gate_install_01.png)

3. Změnit umístění softwaru hello. Potom vyberte hello **spustit správce** pole a zadejte umístění databáze hello. Vyberte **Další** toocontinue.

  ![Snímek obrazovky stránky instalace vyberte hello](./media/oracle-golden-gate/golden_gate_install_02.png)

4. Změňte adresář hello inventáře a potom vyberte **Další** toocontinue.

  ![Snímek obrazovky stránky instalace vyberte hello](./media/oracle-golden-gate/golden_gate_install_03.png)

5. Na hello **Souhrn** obrazovku, vyberte **nainstalovat** toocontinue.

  ![Snímek obrazovky stránky instalace vyberte Instalační program hello](./media/oracle-golden-gate/golden_gate_install_04.png)

6. Může být výzvami toorun skript jako "kořenový". Pokud ano, otevřete relaci samostatné ssh toohello virtuálních počítačů, sudo tooroot a spusťte skript hello. Vyberte **OK** pokračovat.

  ![Snímek obrazovky stránky instalace vyberte hello](./media/oracle-golden-gate/golden_gate_install_05.png)

7. Po dokončení instalace hello vyberte **Zavřít** toocomplete hello procesu.

  ![Snímek obrazovky stránky instalace vyberte hello](./media/oracle-golden-gate/golden_gate_install_06.png)

### <a name="set-up-service-on-myvm1-primary"></a>Nastavení služby v myVM1 (primární)

1. Vytvořit nebo aktualizovat souboru tnsnames.ora hello:

  ```bash
  $ cd $ORACLE_HOME/network/admin
  $ vi tnsnames.ora

  cdb1=
    (DESCRIPTION=
      (ADDRESS=
        (PROTOCOL=TCP)
        (HOST=localhost)
        (PORT=1521)
      )
      (CONNECT_DATA=
        (SERVER=dedicated)
        (SERVICE_NAME=cdb1)
      )
    )

  pdb1=
    (DESCRIPTION=
      (ADDRESS=
        (PROTOCOL=TCP)
        (HOST=localhost)
        (PORT=1521)
      )
      (CONNECT_DATA=
        (SERVER=dedicated)
        (SERVICE_NAME=pdb1)
      )
    )
  ```

2. Vytvořte hello Golden brány vlastníka a uživatelské účty.

  > [!NOTE]
  > Hello vlastníka účet musí mít předponu C ##.
  >

    ```bash
    $ sqlplus / as sysdba
    SQL> CREATE USER C##GGADMIN identified by ggadmin;
    SQL> EXEC dbms_goldengate_auth.grant_admin_privilege('C##GGADMIN',container=>'ALL');
    SQL> GRANT DBA tooC##GGADMIN container=all;
    SQL> connect C##GGADMIN/ggadmin
    SQL> ALTER SESSION SET CONTAINER=PDB1;
    SQL> EXIT;
    ```

3. Vytvořte hello Golden brány testovací uživatelský účet:

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ sqlplus system/OraPasswd1@pdb1
  SQL> CREATE USER test identified by test DEFAULT TABLESPACE USERS TEMPORARY TABLESPACE TEMP;
  SQL> GRANT connect, resource, dba tootest;
  SQL> ALTER USER test QUOTA 100M on USERS;
  SQL> connect test/test@pdb1
  SQL> @demo_ora_create
  SQL> @demo_ora_insert
  SQL> EXIT;
  ```

4. Nakonfigurujte soubor parametrů extrakce hello.

 Spuštění rozhraní příkazového řádku zlaté brány hello (ggsci):

  ```bash
  $ sudo su - oracle
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  GGSCI> DBLOGIN USERID test@pdb1, PASSWORD test
  Successfully logged into database  pdb1
  GGSCI>  ADD SCHEMATRANDATA pdb1.test
  2017-05-23 15:44:25  INFO    OGG-01788  SCHEMATRANDATA has been added on schema test.
  2017-05-23 15:44:25  INFO    OGG-01976  SCHEMATRANDATA for scheduling columns has been added on schema test.

  GGSCI> EDIT PARAMS EXTORA
  ```
5. Přidejte hello následující toohello EXTRAKCE parametr soubor (pomocí příkazů vi). Stisknutím klávesy Esc, ': QW!. toosave soubor. 

  ```bash
  EXTRACT EXTORA
  USERID C##GGADMIN, PASSWORD ggadmin
  RMTHOST 10.0.0.5, MGRPORT 7809
  RMTTRAIL ./dirdat/rt  
  DDL INCLUDE MAPPED
  DDLOPTIONS REPORT 
  LOGALLSUPCOLS
  UPDATERECORDFORMAT COMPACT
  TABLE pdb1.test.TCUSTMER;
  TABLE pdb1.test.TCUSTORD;
  ```
6. Extrahujte registrace--integrované extrakce:

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci

  GGSCI> dblogin userid C##GGADMIN, password ggadmin
  Successfully logged into database CDB$ROOT.

  GGSCI> REGISTER EXTRACT EXTORA DATABASE CONTAINER(pdb1)

  2017-05-23 15:58:34  INFO    OGG-02003  Extract EXTORA successfully registered with database at SCN 1821260.

  GGSCI> exit
  ```
7. Nastavit extrakce kontrolní body a spustit v reálném čase extrakce:

  ```bash
  $ ./ggsci
  GGSCI>  ADD EXTRACT EXTORA, INTEGRATED TRANLOG, BEGIN NOW
  EXTRACT (Integrated) added.

  GGSCI>  ADD RMTTRAIL ./dirdat/rt, EXTRACT EXTORA, MEGABYTES 10
  RMTTRAIL added.

  GGSCI>  START EXTRACT EXTORA

  Sending START request tooMANAGER ...
  EXTRACT EXTORA starting

  GGSCI > info all

  Program     Status      Group       Lag at Chkpt  Time Since Chkpt

  MANAGER     RUNNING
  EXTRACT     RUNNING     EXTORA      00:00:11      00:00:04
  ```
V tomto kroku najít hello od oznámení změny stavu, který se použije později v jiné části:

  ```bash
  $ sqlplus / as sysdba
  SQL> alter session set container = pdb1;
  SQL> SELECT current_scn from v$database;
  CURRENT_SCN
  -----------
      1857887
  SQL> EXIT;
  ```

  ```bash
  $ ./ggsci
  GGSCI> EDIT PARAMS INITEXT
  ```

  ```bash
  EXTRACT INITEXT
  USERID C##GGADMIN, PASSWORD ggadmin
  RMTHOST 10.0.0.5, MGRPORT 7809
  RMTTASK REPLICAT, GROUP INITREP
  TABLE pdb1.test.*, SQLPREDICATE 'AS OF SCN 1857887'; 
  ```

  ```bash
  GGSCI> ADD EXTRACT INITEXT, SOURCEISTABLE
  ```

### <a name="set-up-service-on-myvm2-replicate"></a>Nastavení služby v Můjvp2 (Replikovat)


1. Vytvořit nebo aktualizovat souboru tnsnames.ora hello:

  ```bash
  $ cd $ORACLE_HOME/network/admin
  $ vi tnsnames.ora

  cdb1=
    (DESCRIPTION=
      (ADDRESS=
        (PROTOCOL=TCP)
        (HOST=localhost)
        (PORT=1521)
      )
      (CONNECT_DATA=
        (SERVER=dedicated)
        (SERVICE_NAME=cdb1)
      )
    )

  pdb1=
    (DESCRIPTION=
      (ADDRESS=
        (PROTOCOL=TCP)
        (HOST=localhost)
        (PORT=1521)
      )
      (CONNECT_DATA=
        (SERVER=dedicated)
        (SERVICE_NAME=pdb1)
      )
    )
  ```

2. Vytvořte účet replikace:

  ```bash
  $ sqlplus / as sysdba
  SQL> alter session set container = pdb1;
  SQL> create user repuser identified by rep_pass container=current;
  SQL> grant dba toorepuser;
  SQL> exec dbms_goldengate_auth.grant_admin_privilege('REPUSER',container=>'PDB1');
  SQL> connect repuser/rep_pass@pdb1 
  SQL> EXIT;
  ```

3. Vytvoření uživatelského účtu testovací Golden brány:

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ sqlplus system/OraPasswd1@pdb1
  SQL> CREATE USER test identified by test DEFAULT TABLESPACE USERS TEMPORARY TABLESPACE TEMP;
  SQL> GRANT connect, resource, dba tootest;
  SQL> ALTER USER test QUOTA 100M on USERS;
  SQL> connect test/test@pdb1
  SQL> @demo_ora_create
  SQL> EXIT;
  ```

4. REPLICAT parametr souboru tooreplicate změny: 

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  GGSCI> EDIT PARAMS REPORA  
  ```
  Obsah souboru REPORA parametr:

  ```bash
  REPLICAT REPORA
  ASSUMETARGETDEFS
  DISCARDFILE ./dirrpt/repora.dsc, PURGE, MEGABYTES 100
  DDL INCLUDE MAPPED
  DDLOPTIONS REPORT
  DBOPTIONS INTEGRATEDPARAMS(parallelism 6)
  USERID repuser@pdb1, PASSWORD rep_pass
  MAP pdb1.test.*, TARGET pdb1.test.*;
  ```

5. Nastavte kontrolní bod replicat:

  ```bash
  GGSCI> ADD REPLICAT REPORA, INTEGRATED, EXTTRAIL ./dirdat/rt
  GGSCI> EDIT PARAMS INITREP

  ```

  ```bash
  REPLICAT INITREP
  ASSUMETARGETDEFS
  DISCARDFILE ./dirrpt/tcustmer.dsc, APPEND
  USERID repuser@pdb1, PASSWORD rep_pass
  MAP pdb1.test.*, TARGET pdb1.test.*;   
  ```

  ```bash
  GGSCI> ADD REPLICAT INITREP, SPECIALRUN
  ```

### <a name="set-up-hello-replication-myvm1-and-myvm2"></a>Nastavení replikace hello (myVM1 a Můjvp2)

#### <a name="1-set-up-hello-replication-on-myvm2-replicate"></a>1. Nastavení replikace hello na Můjvp2 (Replikovat)

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  GGSCI> EDIT PARAMS MGR
  ```
Aktualizujte soubor hello hello následující:

  ```bash
  PORT 7809
  ACCESSRULE, PROG *, IPADDR *, ALLOW
  ```
Potom restartujte službu Manager hello:

  ```bash
  GGSCI> STOP MGR
  GGSCI> START MGR
  GGSCI> EXIT
  ```

#### <a name="2-set-up-hello-replication-on-myvm1-primary"></a>2. Nastavení replikace hello na myVM1 (primární)

Spusťte počáteční zatížení hello a zkontrolujte chyby:

```bash
$ cd /u01/app/oracle/product/12.1.0/oggcore_1
$ ./ggsci
GGSCI> START EXTRACT INITEXT
GGSCI> VIEW REPORT INITEXT
```
#### <a name="3-set-up-hello-replication-on-myvm2-replicate"></a>3. Nastavení replikace hello na Můjvp2 (Replikovat)

Změna hello oznámení změny stavu číslo s číslem hello jste předtím získali:

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  START REPLICAT REPORA, AFTERCSN 1857887
  ```
zahájení replikace Hello a jej můžete otestovat pomocí vkládání nových záznamů tooTEST tabulek.


### <a name="view-job-status-and-troubleshooting"></a>Zobrazení stavu úlohy a řešení potíží

#### <a name="view-reports"></a>Zobrazení sestav
tooview hlásí myVM1, spusťte následující příkazy hello:

  ```bash
  GGSCI> VIEW REPORT EXTORA 
  ```
 
tooview hlásí Můjvp2, spusťte následující příkazy hello:

  ```bash
  GGSCI> VIEW REPORT REPORA
  ```

#### <a name="view-status-and-history"></a>Zobrazit stav a historie
tooview stav a historie na myVM1, spusťte následující příkazy hello:

  ```bash
  GGSCI> dblogin userid c##ggadmin, password ggadmin 
  GGSCI> INFO EXTRACT EXTORA, DETAIL
  ```

tooview stav a historie na Můjvp2, spusťte následující příkazy hello:

  ```bash
  GGSCI> dblogin userid repuser@pdb1 password rep_pass 
  GGSCI> INFO REP REPORA, DETAIL
  ```
Tím se dokončí hello instalaci a konfiguraci brány Golden na Oracle linux.


## <a name="delete-hello-virtual-machine"></a>Odstranění hello virtuálního počítače

Když už je potřeba, může být hello následující příkaz skupiny prostředků použít tooremove hello, virtuálních počítačů a všechny související prostředky.

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Další kroky

[Kurz vytvoření virtuálního počítače s vysokou dostupností](../../linux/create-cli-complete.md)

[Prozkoumejte ukázky nasazení virtuálního počítače pomocí rozhraní příkazového řádku](../../linux/cli-samples.md)
