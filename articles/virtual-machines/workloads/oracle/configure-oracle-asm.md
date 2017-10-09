---
title: "aaaSet až Oracle ASM na virtuálním počítači Azure Linux | Microsoft Docs"
description: "Rychle získáte Oracle ASM nahoru a spouštění v prostředí Azure."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: RicksterCDN
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/19/2017
ms.author: rclaus
ms.openlocfilehash: d6a7046638e919876477d46943faabcb1872acac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-oracle-asm-on-an-azure-linux-virtual-machine"></a>Nastavit Oracle ASM na virtuálním počítači Azure Linux  

Virtuální počítače Azure, zadejte plně konfigurovatelné a flexibilní výpočetního prostředí. Tento kurz se zaměřuje na základní virtuální počítač Azure nasazení v kombinaci s hello instalace a konfigurace systému Oracle automatizované úložiště ASM (Správa).  Získáte informace o těchto tématech:

> [!div class="checklist"]
> * Vytvořit a připojit tooan databáze Oracle virtuálních počítačů
> * Instalace a konfigurace Oracle automatizovaná správa úložiště
> * Instalace a konfigurace infrastruktury Oracle mřížky
> * Inicializace instalace Oracle ASM
> * Vytvoření Oracle DB spravuje ASM


[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Pokud zvolíte tooinstall a místně pomocí hello rozhraní příkazového řádku, tento kurz vyžaduje, že používáte verzi rozhraní příkazového řádku Azure hello verze 2.0.4 nebo novější. Spustit `az --version` toofind hello verze. Pokud potřebujete tooinstall nebo aktualizace, přečtěte si [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="prepare-hello-environment"></a>Příprava prostředí hello

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

toocreate skupinu prostředků použijte hello [vytvořit skupinu az](/cli/azure/group#create) příkaz. Skupinu prostředků Azure je logický kontejner, ve které jsou nasazené a spravované prostředky. V tomto příkladu skupinu prostředků s názvem *myResourceGroup* v hello *eastus* oblast.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

### <a name="create-a-vm"></a>Vytvoření virtuálního počítače

toocreate virtuálního počítače na základě bitové kopie databáze Oracle hello a nakonfigurovat ji toouse Oracle ASM, použijte hello [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz. 

Hello následující příklad vytvoří virtuální počítač s názvem můjvp přesměrovat, který je velikost Standard_DS2_v2 s čtyři připojené datových disků 50 GB. Pokud už neexistují v hello umístění klíče výchozí, vytvoří také klíče SSH.  toouse konkrétní nastavení klíčů, použijte hello `--ssh-key-value` možnost.  

   ```azurecli-interactive
   az vm create --resource-group myResourceGroup \
    --name myVM \
    --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
    --size Standard_DS2_v2 \
    --generate-ssh-keys \
    --data-disk-sizes-gb 50 50 50 50
   ```

Po vytvoření hello virtuálních počítačů Azure CLI zobrazí informace podobné toohello následující ukázka. Poznamenejte si hodnotu hello pro `publicIpAddress`. Můžete použít tuto adresu tooaccess hello virtuálních počítačů.

   ```azurecli
   {
     "fqdns": "",
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
     "location": "eastus",
     "macAddress": "00-0D-3A-36-2F-56",
     "powerState": "VM running",
     "privateIpAddress": "10.0.0.4",
     "publicIpAddress": "13.64.104.241",
     "resourceGroup": "myResourceGroup"
   }
   ```

### <a name="connect-toohello-vm"></a>Připojit toohello virtuálních počítačů

toocreate na relace SSH s hello virtuálních počítačů a konfigurovat další nastavení, použijte následující příkaz hello. Nahraďte IP adresu hello hello `publicIpAddress` hodnotu pro virtuální počítač.

```bash 
ssh <publicIpAddress>
```

## <a name="install-oracle-asm"></a>Nainstalujte Oracle ASM

tooinstall ASM Oracle, dokončení hello následující kroky. 

Další informace o instalaci Oracle ASM najdete v tématu [Oracle ASMLib soubory ke stažení pro Oracle Linux 6](http://www.oracle.com/technetwork/server-storage/linux/asmlib/ol6-1709075.html).  

1. Je třeba toologin jako kořenového adresáře v pořadí toocontinue s instalací ASM:

   ```bash
   sudo su -
   ```
   
2. Spusťte tyto další příkazy tooinstall Oracle ASM součásti:

   ```bash
    yum list | grep oracleasm 
    yum -y install kmod-oracleasm.x86_64 
    yum -y install oracleasm-support.x86_64 
    wget http://download.oracle.com/otn_software/asmlib/oracleasmlib-2.0.12-1.el6.x86_64.rpm 
    yum -y install oracleasmlib-2.0.12-1.el6.x86_64.rpm 
    rm -f oracleasmlib-2.0.12-1.el6.x86_64.rpm
   ```

3. Ověřte, zda je nainstalován Oracle ASM:

   ```bash
   rpm -qa |grep oracleasm
   ```

    Hello výstup tohoto příkazu by měl obsahovat hello následující součásti:

    ```bash
   oracleasm-support-2.1.10-4.el6.x86_64
   kmod-oracleasm-2.0.8-15.el6_9.x86_64
   oracleasmlib-2.0.12-1.el6.x86_64
    ```

4. ASM vyžaduje konkrétní uživatele a role v pořadí toofunction správně. Následující příkazy Hello vytvořit hello předběžné uživatelské účty a skupiny: 

   ```bash
    groupadd -g 54345 asmadmin 
    groupadd -g 54346 asmdba 
    groupadd -g 54347 asmoper 
    useradd -u 3000 -g oinstall -G dba,asmadmin,asmdba,asmoper grid 
    usermod -g oinstall -G dba,asmdba,asmadmin oracle
   ```

5. Ověření uživatelů a skupin byly vytvořeny správně:

   ```bash
   id grid
   ```

    Hello výstup tohoto příkazu by měl obsahovat následující hello uživatelů a skupin:

    ```bash
    uid=3000(grid) gid=54321(oinstall) groups=54321(oinstall),54322(dba),54345(asmadmin),54346(asmdba),54347(asmoper)
    ```
 
6. Vytvořte složku pro uživatele *mřížky* a změnit vlastníka hello:

   ```bash
   mkdir /u01/app/grid 
   chown grid:oinstall /u01/app/grid
   ```

## <a name="set-up-oracle-asm"></a>Nastavit Oracle ASM

V tomto kurzu hello výchozího uživatele je *mřížky* a hello výchozí skupina je *asmadmin*. Ujistěte se, že hello *oracle* uživatel je součástí skupiny asmadmin hello. tooset Oracle ASM Installation, dokončení hello následující kroky:

1. Nastavení hello Oracle ASM knihovny ovladačů zahrnuje definování hello výchozího uživatele (mřížky) a výchozí skupiny (asmadmin) a také konfigurace hello jednotky toostart na spouštěcí (zvolte y) a tooscan pro disky na spouštěcí (zvolte y). Je třeba tooanswer hello výzvy z hello následující příkaz:

   ```bash
   /usr/sbin/oracleasm configure -i
   ```

   Hello výstup tohoto příkazu by měla vypadat podobně jako toohello následující, zastavuje s toobe výzvy odpovědi.

    ```bash
   Configuring hello Oracle ASM library driver.

   This will configure hello on-boot properties of hello Oracle ASM library
   driver. hello following questions will determine whether hello driver is
   loaded on boot and what permissions it will have. hello current values
   will be shown in brackets ('[]'). Hitting <ENTER> without typing an
   answer will keep that current value. Ctrl-C will abort.

   Default user tooown hello driver interface []: grid
   Default group tooown hello driver interface []: asmadmin
   Start Oracle ASM library driver on boot (y/n) [n]: y
   Scan for Oracle ASM disks on boot (y/n) [y]: y
   Writing Oracle ASM library driver configuration: done
   ```

2. Konfigurace disku hello zobrazení:
   ```bash
   cat /proc/partitions
   ```

   Hello výstup tohoto příkazu by měl vypadat podobně jako toohello následující seznam dostupných disků

   ```bash
   8       16   14680064 sdb
   8       17   14678976 sdb1
   8        0   52428800 sda
   8        1     512000 sda1
   8        2   51915776 sda2
   8       48   52428800 sdd
   8       64   52428800 sde
   8       80   52428800 sdf
   8       32   52428800 sdc
   11       0       1152 sr0
   ```

3. Formát disku */dev/sdc* spuštěním hello následující příkaz a volaného hello výzvy k:
   - *n*pro nový oddíl
   - *p* pro primární oddíl
   - *1* tooselect hello první oddíl
   - stiskněte klávesu `enter` pro první cylindr výchozí hello
   - stiskněte klávesu `enter` pro hello výchozí poslední cylindr
   - stiskněte klávesu *w* toowrite hello změny toohello oddílu tabulky  

   ```bash
   fdisk /dev/sdc
   ```
   
   Pomocí výše uvedeného odpovědi hello, hello výstup hello fdisk příkazu by měl vypadat podobně jako následující hello:

   ```bash
   Device contains not a valid DOS partition table, or Sun, SGI or OSF disklabel
   Building a new DOS disklabel with disk identifier 0xf865c6ca.
   Changes will remain in memory only, until you decide toowrite them.
   After that, of course, hello previous content won't be recoverable.

   Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

   hello device presents a logical sector size that is smaller than
   hello physical sector size. Aligning tooa physical sector (or optimal
   I/O) size boundary is recommended, or performance may be impacted.

   WARNING: DOS-compatible mode is deprecated. It's strongly recommended to
           switch off hello mode (command 'c') and change display units to
           sectors (command 'u').

   Command (m for help): n
   Command action
     e   extended
     p   primary partition (1-4)
   p
   Partition number (1-4): 1
   First cylinder (1-6527, default 1):
   Using default value 1
   Last cylinder, +cylinders or +size{K,M,G} (1-6527, default 6527):
   Using default value 6527

   Command (m for help): w
   hello partition table has been altered!

   Calling ioctl() toore-read partition table.
   Syncing disks.
   ```

4. Opakování hello předcházející příkaz fdisk pro `/dev/sdd`, `/dev/sde`, a `/dev/sdf`.

5. Kontrola konfigurace disku hello:

   ```bash
   cat /proc/partitions
   ```

   výstup Hello hello příkazu by měl vypadat podobně jako následující hello:

   ```bash
   major minor  #blocks  name

     8       16   14680064 sdb
     8       17   14678976 sdb1
     8       32   52428800 sdc
     8       33   52428096 sdc1
     8       48   52428800 sdd
     8       49   52428096 sdd1
     8       64   52428800 sde
     8       65   52428096 sde1
     8       80   52428800 sdf
     8       81   52428096 sdf1
     8        0   52428800 sda
     8        1     512000 sda1
     8        2   51915776 sda2
     11       0    1048575 sr0
   ```

6. Zkontrolujte stav služby hello Oracle ASM a spuštění služby hello Oracle ASM:

   ```bash
   service oracleasm status 
   service oracleasm start
   ```

   výstup Hello hello příkazu by měl vypadat podobně jako následující hello:
   
   ```bash
   Checking if ASM is loaded: no
   Checking if /dev/oracleasm is mounted: no
   Initializing hello Oracle ASMLib driver:                     [  OK  ]
   Scanning hello system for Oracle ASMLib disks:               [  OK  ]
   ```

7. Vytvořte disky Oracle ASM:

   ```bash
   service oracleasm createdisk ASMSP /dev/sdc1 
   service oracleasm createdisk DATA /dev/sdd1 
   service oracleasm createdisk DATA1 /dev/sde1 
   service oracleasm createdisk FRA /dev/sdf1
   ```    

   výstup Hello hello příkazu by měl vypadat podobně jako následující hello:

   ```bash
   Marking disk "ASMSP" as an ASM disk:                       [  OK  ]
   Marking disk "DATA" as an ASM disk:                        [  OK  ]
   Marking disk "DATA1" as an ASM disk:                       [  OK  ]
   Marking disk "FRA" as an ASM disk:                         [  OK  ]
   ```

8. Seznam disků Oracle ASM:

   ```bash
   service oracleasm listdisks
   ```   

   výstup Hello hello příkazu by měl seznamu vypnout hello následující disky Oracle ASM:

   ```bash
    ASMSP
    DATA
    DATA1
    FRA
   ```

9. Změnit hello hesla pro uživatele root, oracle a mřížky hello. **Poznamenejte si tyto nová hesla** jako jejich použijete později v průběhu instalace hello.

   ```bash
   passwd oracle 
   passwd grid 
   passwd root
   ```

10. Změna oprávnění složky hello:

   ```bash
   chmod -R 775 /opt 
   chown grid:oinstall /opt 
   chown oracle:oinstall /dev/sdc1 
   chown oracle:oinstall /dev/sdd1 
   chown oracle:oinstall /dev/sde1 
   chown oracle:oinstall /dev/sdf1 
   chmod 600 /dev/sdc1 
   chmod 600 /dev/sdd1 
   chmod 600 /dev/sde1 
   chmod 600 /dev/sdf1
   ```

## <a name="download-and-prepare-oracle-grid-infrastructure"></a>Stáhněte si a příprava infrastruktury mřížky Oracle

toodownload a příprava softwaru Oracle mřížky infrastruktury hello, dokončení hello následující kroky:

1. Stáhnout Oracle mřížky infrastruktury z hello [stránky pro stažení Oracle ASM](http://www.oracle.com/technetwork/database/enterprise-edition/downloads/database12c-linux-download-2240591.html). 

   V části s názvem stažení hello **databáze Oracle 12c verze 1 mřížky infrastruktury (12.1.0.2.0) pro Linux x86-64**, stáhnout hello dva soubory .zip.

2. Po stažení hello .zip soubory tooyour klientský počítač, můžete použít protokol Secure kopírování (SCP) toocopy hello soubory tooyour virtuálních počítačů:

   ```bash
   scp *.zip <publicIpAddress>:.
   ```

3. SSH zpět do Oracle virtuálního počítače v Azure v pořadí toomove hello .zip soubory do hello / opt složky. Potom změňte vlastníka hello hello souborů:

   ```bash
   ssh <publicIPAddress>
   sudo mv ./*.zip /opt
   cd /opt
   sudo chown grid:oinstall linuxamd64_12102_grid_1of2.zip
   sudo chown grid:oinstall linuxamd64_12102_grid_2of2.zip
   ```

4. Rozbalte soubory hello. (Instalace hello Linux nástroj rozbalte, pokud je ještě není nainstalován.)
   
   ```bash
   sudo yum install unzip
   sudo unzip linuxamd64_12102_grid_1of2.zip
   sudo unzip linuxamd64_12102_grid_2of2.zip
   ```

5. Změna oprávnění:
   
   ```bash
   sudo chown -R grid:oinstall /opt/grid
   ```

6. Aktualizace nakonfigurované velikosti odkládacího souboru. Oracle mřížky součásti potřebovat aspoň 6.8 GB tooinstall místa odkládacího souboru mřížky. Hello velikosti odkládacího souboru výchozí pro Oracle Linux bitové kopie v Azure je 2 048 MB paměti. Je třeba tooincrease `ResourceDisk.SwapSizeMB` v hello `/etc/waagent.conf` soubor a po restartování služby WALinuxAgent hello hello aktualizovat nastavení tootake vliv. Protože je jen pro čtení souboru, potřebujete přístup pro zápis tooenable toochange souboru oprávnění.

   ```bash
   sudo chmod 777 /etc/waagent.conf  
   vi /etc/waagent.conf
   ```
   
   Vyhledejte `ResourceDisk.SwapSizeMB` a změňte hodnotu hello příliš**8192**. Budete potřebovat toopress `insert` tooenter vložení režimu, zadejte hodnotu hello **8192** a potom stiskněte klávesu `esc` tooreturn toocommand režimu. toowrite hello změny a ukončete hello soubor, zadejte `:wq` a stiskněte klávesu `enter`.
   
   > [!NOTE]
   > Důrazně doporučujeme vždy použít `WALinuxAgent` tooconfigure záměna prostoru tak, aby se vždy vytvoří na hello místní dočasné disk (disk dočasné) pro nejlepší výkon. Další informace o najdete v tématu [jak tooadd prohození souborů ve virtuálních počítačích Linux Azure](https://support.microsoft.com/en-us/help/4010058/how-to-add-a-swap-file-in-linux-azure-virtual-machines).

## <a name="prepare-your-local-client-and-vm-toorun-x11"></a>Příprava místního klienta a virtuálních počítačů toorun x11
Konfigurace Oracle ASM vyžaduje grafické rozhraní toocomplete hello instalace a konfigurace. Používáme protokol toofacilitate hello x11 této instalace. Pokud používáte systém klienta (Mac nebo Linux), který už má X11 funkce povolena a konfigurován – můžete přeskočit tuto konfiguraci a nastavení výhradní tooWindows počítače. 

1. [Stáhněte si PuTTY](http://www.putty.org/) a [stáhnout Xming](https://xming.en.softonic.com/) tooyour počítač se systémem Windows. Budete potřebovat instalaci hello toocomplete obě tyto aplikace s výchozími hodnotami hello než budete pokračovat.

2. Po instalaci PuTTY, otevřete příkazový řádek, změňte do hello PuTTY složky (například C:\Program Files\PuTTY) a spusťte `puttygen.exe` v pořadí toogenerate klíč.

3. V generátoru PuTTY klíče:
   
   1. Vygenerovat klíč výběrem hello `Generate` tlačítko.
   2. Kopírovat obsah hello hello klíče (Ctrl + C).
   3. Vyberte hello `Save private key` tlačítko.
   4. Ignorovat upozornění hello o zabezpečení hello klíč s přístupové heslo a potom vyberte `OK`.

   ![Snímek obrazovky PuTTY klíče generátor](./media/oracle-asm/puttykeygen.png)

4. Ve vašem virtuálním počítači spusťte tyto příkazy:

   ```bash
   sudo su - grid
   mkdir .ssh 
   cd .ssh
   ```

5. Vytvořte soubor s názvem `authorized_keys`. Vložte obsah hello hello klíče v tomto souboru a potom uložte soubor hello.

   > [!NOTE]
   > Hello klíč musí obsahovat řetězec hello `ssh-rsa`. Obsah hello hello klíče musí být také, jeden řádek textu.
   >  

6. V systému klienta spusťte PuTTY. V hello **kategorie** podokně přejděte příliš**připojení** > **SSH** > **Auth**. V hello **soubor privátního klíče pro ověřování** pole, procházet toohello klíč, který jste vygenerovali dříve.

   ![Snímek obrazovky možností ověřování SSH hello](./media/oracle-asm/setprivatekey.png)

7. V hello **kategorie** podokně přejděte příliš**připojení** > **SSH** > **X11**. Vyberte hello **povolit X11 předávání** zaškrtávací políčko.

   ![Snímek obrazovky hello SSH X11 předávání možnosti](./media/oracle-asm/enablex11.png)

8. V hello **kategorie** podokně přejděte příliš**relace**. Zadejte virtuální počítač ASM Oracle `<publicIPaddress>` ve hello hostitele název dialogové okno, zadejte nový `Saved Session` název a potom klikněte na `Save`.  Po uložení, klikněte na `open` tooconnect tooyour Oracle ASM virtuálního počítače.  Hello poprvé připojíte, budete upozorněni, že hello vzdáleného systému není uložena v mezipaměti v registru. Klikněte na `yes` tooadd ho a pokračovat.

   ![Snímek obrazovky možnosti relace PuTTY hello](./media/oracle-asm/puttysession.png)

## <a name="install-oracle-grid-infrastructure"></a>Instalace infrastruktury mřížky Oracle

tooinstall Oracle mřížky infrastruktury, dokončení hello následující kroky:

1. Přihlaste se jako **mřížky**. (Musí být schopný toosign v aniž byste byli vyzváni k zadání hesla.) 

   > [!NOTE]
   > Pokud používáte systém Windows, ujistěte se, že jste spustili Xming před zahájením instalace hello.

   ```bash
   cd /opt/grid
   ./runInstaller
   ```

   Otevře se Oracle mřížky infrastruktury 12c instalační program verze 1. (To může trvat několik minut, než instalační program toostart hello.)

2. Na hello **vyberte možnost instalace** vyberte **instalace a konfigurace infrastruktury mřížky Oracle pro samostatný Server**.

   ![Snímek obrazovky stránky vyberte možnost instalace instalační hello](./media/oracle-asm/install01.png)

3. Na hello **vyberte jazyky produktu** zkontrolujte **Angličtina** nebo je hello jazyk, který chcete vybrat.  Klikněte na `next` (Další).

4. Na hello **vytvořit skupinu disku ASM** stránky:
   - Zadejte název pro skupinu disku hello.
   - V části **redundance**, vyberte **externí**.
   - V části **velikost alokační jednotky**, vyberte **4**.
   - V části **přidat disky**, vyberte **ORCLASMSP**.
   - Klikněte na `next` (Další).

5. Na hello **zadejte heslo ASM** stránky, vyberte hello **používat stejné hesla pro tyto účty** možnost a zadejte heslo.

   ![Snímek obrazovky stránky zadejte heslo ASM instalační hello](./media/oracle-asm/install04.png)

6. Na hello **zadejte možnosti správy** stránky, máte možnost tooconfigure hello EM cloudu řízení. Tato možnost jsme přeskočili – klikněte na tlačítko `next` toocontinue. 

7. Na hello **privilegovaných skupin operačního systému** stránky, použijte výchozí nastavení hello. Klikněte na tlačítko `next` toocontinue.

8. Na hello **zadejte umístění instalace** stránky, použijte výchozí nastavení hello. Klikněte na tlačítko `next` toocontinue.

9. Na hello **vytvoření inventáře** změňte hello inventáře Directory příliš`/u01/app/grid/oraInventory`. Klikněte na tlačítko `next` toocontinue.

   ![Snímek obrazovky stránky vytvořit inventáře instalační hello](./media/oracle-asm/install08.png)

10. Na hello **konfiguraci spuštění skriptu kořenové** stránky, vyberte hello **automaticky spouštět skripty konfigurace** zaškrtávací políčko. Pak vyberte hello **použít přihlašovací údaje uživatele "root"** možnost a zadejte heslo uživatele root hello.

    ![Snímek obrazovky instalačního programu hello kořenové skriptu provádění konfiguraci stránky](./media/oracle-asm/install09.png)

11. Na hello **provedení požadovaných součástí kontroluje** stránce hello aktuální instalace se nezdaří s chybami. Toto je očekávané chování. Vyberte `Fix & Check Again`.

12. V hello **oprava skriptu** dialogové okno, klikněte na tlačítko `OK`.

13. Na hello **Souhrn** stránka, zkontrolujte vybrané nastavení a pak klikněte na tlačítko `Install`.

    ![Snímek obrazovky stránky Souhrn instalační hello](./media/oracle-asm/install12.png)

14. Zobrazí se upozornění dialogové okno oznamující konfigurační skripty, stačí spustit toobe jako privilegovaných uživatelů. Klikněte na tlačítko `Yes` toocontinue.

15. Na hello **Dokončit** klikněte na tlačítko `Close` toofinish hello instalace.

## <a name="set-up-your-oracle-asm-installation"></a>Nastavení instalace Oracle ASM

tooset Oracle ASM Installation, dokončení hello následující kroky:

1. Ujistěte se ještě nejste přihlášení jako **mřížky**, z vaší X11 relace. Může být nutné toohit `enter` toorevive hello terminálu. Spusťte hello Oracle automatizované úložiště správy konfigurace pomocníka:

   ```bash
   cd /u01/app/grid/product/12.1.0/grid/bin
   ./asmca
   ```

   Otevře se Oracle ASM konfigurace pomocníka.

2. V hello **konfigurace ASM: disku skupiny** dialogové okno pole, klikněte na tlačítko hello `Create` tlačítko a pak klikněte na tlačítko `Show Advanced Options`.

3. V hello **vytvořit skupinu disku** dialogové okno:

   - Zadejte název skupiny disku hello **DATA**.
   - V části **vyberte disky člen**, vyberte **ORCL_DATA** a **ORCL_DATA1**.
   - V části **velikost alokační jednotky**, vyberte **4**.
   - Klikněte na tlačítko `ok` toocreate hello disku skupiny.
   - Klikněte na tlačítko `ok` tooclose hello potvrzovacím okně.

   ![Snímek obrazovky dialogového okna vytvořit skupinu disku hello](./media/oracle-asm/asm02.png)

4. V hello **konfigurace ASM: disku skupiny** dialogové okno pole, klikněte na tlačítko hello `Create` tlačítko a pak klikněte na tlačítko `Show Advanced Options`.

5. V hello **vytvořit skupinu disku** dialogové okno:

   - Zadejte název skupiny disku hello **FRA**.
   - V části **redundance**, vyberte **externí (none)**.
   - V části **vyberte disky člen**, vyberte **ORCL_FRA**.
   - V části **velikost alokační jednotky**, vyberte **4**.
   - Klikněte na tlačítko `ok` toocreate hello disku skupiny.
   - Klikněte na tlačítko `ok` tooclose hello potvrzovacím okně.

   ![Snímek obrazovky dialogového okna vytvořit skupinu disku hello](./media/oracle-asm/asm04.png)

6. Vyberte **ukončení** tooclose ASM konfigurace pomocníka.

   ![Snímek obrazovky hello nakonfigurovat ASM: disku skupiny dialogové okno s tlačítkem ukončení](./media/oracle-asm/asm05.png)

## <a name="create-hello-database"></a>Vytvoření databáze hello

Hello softwaru databáze Oracle je již nainstalován na bitovou kopii hello Azure Marketplace. toocreate databáze, dokončení hello následující kroky:

1. Přepnout uživatele toohello Oracle superuživatel a potom inicializujte hello naslouchací proces pro protokolování:

   ```bash
   su - oracle
   cd /u01/app/oracle/product/12.1.0/dbhome_1/bin
   ./dbca
   ```
   Otevře se Pomocník pro konfiguraci databáze.

2. Na hello **operace databáze** klikněte na tlačítko `Create Database`.

3. Na hello **režimu vytváření** stránky:

   - Zadejte název databáze hello.
   - Pro **typ úložiště**, ujistěte se, **automatické úložiště Management (ASM)** je vybrána.
   - Pro **umístění souborů databáze**, použít výchozí hello ASM navrhované umístění.
   - Pro **rychlého obnovení oblasti**, použít výchozí hello ASM navrhované umístění.
   - Zadejte **hesla pro správu** a **potvrzení hesla**.
   - Ujistěte se, `create as container database` je vybrána.
   - Zadejte `pluggable database name` hodnotu.

4. Na hello **Souhrn** stránka, zkontrolujte vybrané nastavení a pak klikněte na tlačítko `Finish` toocreate hello databáze.

   ![Snímek obrazovky stránky Souhrn hello](./media/oracle-asm/createdb03.png)

5. Hello databáze byla vytvořena. Na hello **Dokončit** stránky, máte možnost toounlock další účty toouse tuto databázi a změnit hesla hello hello. Pokud toodo chcete udělat, vyberte **Správa hesel** -jinak klikněte na `close`.

## <a name="delete-hello-vm"></a>Odstranit hello virtuálních počítačů

Úspěšně jste nakonfigurovali Oracle automatizovat správu úložiště na bitovou kopii databáze Oracle hello z hello Azure Marketplace.  Pokud již nepotřebujete tento virtuální počítač, můžete použít následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello:

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Další kroky

[Kurz: Konfigurace Oracle DataGuard](configure-oracle-dataguard.md)

[Kurz: Konfigurace Oracle GoldenGate](Configure-oracle-golden-gate.md)

Zkontrolujte [architektury Oracle DB](oracle-design.md)
