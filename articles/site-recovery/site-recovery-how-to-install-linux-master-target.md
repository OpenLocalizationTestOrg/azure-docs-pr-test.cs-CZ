---
title: "aaaHow tooinstall převzetí služeb při selhání z Azure tooon místní hlavní cílový server Linux | Microsoft Docs"
description: "Před opětovnou ochranu virtuální počítač s Linuxem, potřebujete hlavní cílový server Linux. Zjistěte, jak tooinstall jeden."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 08/11/2017
ms.author: ruturajd
ms.openlocfilehash: d7c55d115712b9862414979f89efb1f177c5f0dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-a-linux-master-target-server"></a>Instalovat hlavní cílový server Linux
Po selhání virtuálních počítačů, může selhat back hello virtuální počítače toohello místního webu. toofail zpět, je nutné tooreprotect hello virtuální počítač z Azure toohello místního webu. Pro tento proces budete potřebovat místní hlavní cílový server tooreceive hello provoz. 

Pokud chráněný virtuální počítač je virtuálního počítače s Windows, musíte Windows hlavní cíl. Pro virtuální počítač s Linuxem budete potřebovat hlavního cíle Linuxu. Čtení hello následující kroky toolearn jak toocreate a nainstalujte systémem Linux hlavní cíl.

> [!IMPORTANT]
> Od verze hello 9.10.0 hlavní cílový server, můžete pouze nainstalovány hello nejnovější hlavní cílový server na serveru Ubuntu 16.04. Nové instalace nejsou povoleny u CentOS6.6 servery. Však můžete dál tooupgrade vaše starého hlavního cílové servery pomocí hello 9.10.0 verze.

## <a name="overview"></a>Přehled
Tento článek obsahuje pokyny, jak tooinstall systémem Linux hlavní cíl.

POST dotazy nebo připomínky můžete na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="prerequisites"></a>Požadavky

* určení toochoose hello hostitele na hello hlavní cíl které toodeploy, pokud hello navrácení služeb po obnovení budete toobe tooan existující místní virtuální počítač nebo tooa nového virtuálního počítače. 
    * Pro existující virtuální počítač hostitele hello hello hlavní cíl musí mít přístup k úložišti dat toohello hello virtuálního počítače.
    * Pokud hello na místním virtuálním počítači neexistuje, je na stejné hostitele jako hlavní cíl hello hello vytvořit hello navrácení služeb po obnovení virtuálního počítače. Můžete vybrat libovolného hostitele ESXi tooinstall hello hlavní cíl.
* Hello hlavní cíl musí být v síti, který může komunikovat s hello procesový server a hello konfigurační server.
* Hello verzi hello hlavní cíl musí být rovna tooor starší než verze hello hello procesový server a hello konfigurační server. Například pokud hello verzi hello konfigurační server je 9.4, hello verzi hello hlavního cíle může být 9.4 nebo 9.3, ale není 9.5.
* hlavní cíl Hello lze pouze virtuální počítač VMware, nikoli na fyzický server.

## <a name="create-hello-master-target-according-toohello-sizing-guidelines"></a>Vytvořit hlavní cíl hello toohello podle pokynů pro změnu velikosti

Vytvořte hlavní cíl hello v souladu s hello následující pokyny k nastavení velikosti:
- **Paměť RAM**: 6 GB nebo více
- **Velikost disku operačního systému**: 100 GB nebo více (tooinstall CentOS6.6)
- **Velikost disku Další jednotka pro uchování**: 1 TB
- **Jader procesoru**: 4 jádra nebo více

Následující Hello podporována Ubuntu jádra jsou podporovány.


|Řada jádra  |Podporovat až příliš |
|---------|---------|
|4.4      |4.4.0-81-Generic         |
|4.8      |4.8.0-56-Generic         |
|4.10     |4.10.0-24-Generic        |


## <a name="deploy-hello-master-target-server"></a>Nasazení hello hlavní cílový server

### <a name="install-ubuntu-16042-minimal"></a>Nainstalujte Ubuntu 16.04.2 minimální

Trvat hello následující hello kroky tooinstall hello Ubuntu 16.04.2 64bitový operační systém.

**Krok 1:** přejděte toohello [stáhnout odkaz](https://www.ubuntu.com/download/server/thank-you?version=16.04.2&architecture=amd64) a zvolte nejbližší zrcadlení hello z které stáhnout soubor ISO Ubuntu 16.04.2 minimální 64-bit.

Zachovat Ubuntu 16.04.2 minimální 64-bit ISO v jednotce hello DVD a spusťte hello systému.

**Krok 2:** vyberte **Angličtina** jako upřednostňovaný jazyk a potom vyberte **Enter**.

![Výběr jazyka](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image1.png)

**Krok 3:** vyberte **nainstalovat Ubuntu Server**a potom vyberte **Enter**.

![Vyberte možnost instalace Ubuntu Server](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image2.png)

**Krok 4:** vyberte **Angličtina** jako upřednostňovaný jazyk a potom vyberte **Enter**.

![Vyberte angličtina jako upřednostňovaný jazyk](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image3.png)

**Krok 5:** hello vyberte příslušnou možnost z hello **časové pásmo** seznam možností a potom vyberte **Enter**.

![Vyberte hello správné časové pásmo](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image4.png)

**Krok 6:** vyberte **ne** (hello výchozí možnost) a potom vyberte **Enter**.


![Konfigurace hello klávesnice](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image5.png)

**Krok 7:** vyberte **angličtinu (US)** jako hello země původu hello klávesnice a pak vyberte **Enter**.

![Vyberte USA jako hello země původu](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image6.png)

**Krok 8:** vyberte **angličtinu (US)** jako hello rozložení klávesnice a potom vyberte **Enter**.

![Vyberte angličtinu jako hello rozložení klávesnice](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image7.png)

**Krok 9:** zadejte hello název hostitele pro server v hello **Hostname** a pak vyberte **pokračovat**.

![Zadejte název hostitele hello pro váš server](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image8.png)

**Krok 10:** toocreate uživatelský účet, zadejte hello uživatelské jméno a potom vyberte **pokračovat**.

![Vytvoření uživatelského účtu](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image9.png)

**Krok 11:** zadejte heslo hello hello nový uživatelský účet a potom vyberte **pokračovat**.

![Zadejte heslo hello](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image10.png)

**Krok 12:** potvrďte hello heslo pro nového uživatele hello a pak vyberte **pokračovat**.

![Potvrzení hesla hello](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image11.png)

**Krok 13:** vyberte **ne** (hello výchozí možnost) a potom vyberte **Enter**.

![Nastavit uživatele a hesla](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image12.png)

**Krok 14:** Pokud hello časové pásmo, které se zobrazí správný, vyberte **Ano** (hello výchozí možnost) a potom vyberte **Enter**.

tooreconfigure svým časovým pásmem, vyberte **ne**.

![Nakonfigurujte hodiny hello](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image13.png)

**Krok 15:** hello dělení možnosti metody, vyberte **na základě - použít celý disk**a potom vyberte **Enter**.

![Vyberte hello dělení možnost – Metoda](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image14.png)

**Krok 16:** vyberte hello příslušný disk z hello **vyberte disk toopartition** možnosti a pak vyberte **Enter**.


![Vyberte hello disk](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image15.png)

**Krok 17:** vyberte **Ano** toowrite hello toodisk změny a pak vyberte **Enter**.

![Zápis změn toodisk hello](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image16.png)

**Krok 18:** vyberte hello výchozí možnost, vyberte **pokračovat**a potom vyberte **Enter**.

![Vyberte možnost Výchozí hello](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image17.png)

**Krok 19:** vyberte příslušnou možnost hello pro správu upgrady systému a pak vyberte **Enter**.

![Vyberte, jak toomanage upgradu](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image18.png)

> [!WARNING]
> Protože hello Azure Site Recovery hlavní cílový server vyžaduje velmi konkrétní verzi hello Ubuntu, je třeba tooensure této hello jádra, které jsou zakázány upgrady pro virtuální počítač hello. Pokud se povolí, nějaké regulární upgrady způsobit hello hlavní cílový server toomalfunction. Zkontrolujte, zda jste vybrali hello **žádné automatické aktualizace** možnost.


**Krok 20:** vyberte výchozí možnosti. Pokud chcete openSSH pro připojení SSH, vyberte hello **OpenSSH server** a pak vyberte možnost **pokračovat**.

![Vyberte software](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image19.png)

**Krok 21:** vyberte **Ano**a potom vyberte **Enter**.

![Isntall hello GRUB spouštěcí zavaděč](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image20.png)

**Krok 22:** hello vyberte příslušné zařízení pro hello spouštěcí zavaděč instalace (pokud možno **/dev/sda**) a potom vyberte **Enter**.

![Vyberte zařízení, které spouštěcí zavaděč instalace](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image21.png)

**Krok 23:** vyberte **pokračovat**a potom vyberte **Enter** toofinish hello instalace.

![Dokončení instalace hello](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image22.png)

Po dokončení instalace hello přihlaste toohello virtuálních počítačů s novými pověřeními uživatele hello. (Odkazovat příliš**krok 10** Další informace.)

Trvat hello kroky, které jsou popsané v následující snímek obrazovky tooset hello KOŘENOVÉ hello heslo uživatele. Přihlaste se jako KOŘENOVÉ uživatele.

![Heslo uživatele ROOT hello sady](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image23.png)


### <a name="prepare-hello-machine-for-configuration-as-a-master-target-server"></a>Příprava hello počítače pro konfiguraci jako hlavní cílový server
V dalším kroku Příprava hello počítače pro konfiguraci jako hlavní cílový server.

tooget hello ID pro každý pevného disku SCSI na virtuálním počítači Linux povolit hello **disku. EnableUUID = TRUE** parametr.

tooenable, které tento parametr hello proveďte následující kroky:

1. Vypněte virtuální počítač.

2. Klikněte pravým tlačítkem na položku hello pro hello virtuální počítač v levém podokně hello a potom vyberte **upravit nastavení**.

3. Vyberte hello **možnosti** kartě.

4. V levém podokně hello vyberte **Upřesnit** > **Obecné**a potom vyberte hello **parametry konfigurace** tlačítko na hello pravé dolní části obrazovky hello.

    ![Karta Možnosti](./media/site-recovery-how-to-install-linux-master-target/media/image20.png)

    Hello **parametry konfigurace** možnost není k dispozici, když hello počítač běží. toomake na této kartě aktivní, vypněte virtuální počítač hello.

5. Zda řádek s **disku. EnableUUID** již existuje.

    - Pokud hodnota hello existuje a je nastaven příliš**False**, změňte hodnotu hello příliš**True**. (hodnoty hello nejsou malá a velká písmena.)

    - Pokud hodnota hello existuje a je nastaven příliš**True**, vyberte **zrušit**.

    - Pokud hodnota hello neexistuje, vyberte **přidat řádek**.

    - Ve sloupci Název hello přidat **disku. EnableUUID**a pak nastavte hodnotu hello příliš**TRUE**.

    ![Kontroluje, zda disk. EnableUUID již existuje.](./media/site-recovery-how-to-install-linux-master-target/media/image21.png)

#### <a name="disable-kernel-upgrades"></a>Zakázat upgrady jádra

Azure Site Recovery hlavní cílový server vyžaduje velmi konkrétní verzi hello Ubuntu, zkontrolujte, zda jsou pro virtuální počítač hello vypnutá upgrady hello jádra.

Pokud upgrady jádra jsou povolené, jakékoli regulární upgrady způsobit hello hlavní cílový server toomalfunction.

#### <a name="download-and-install-additional-packages"></a>Stáhněte a nainstalujte další balíčky

> [!NOTE]
> Ujistěte se, že máte toodownload připojení k Internetu a nainstalovat další balíčky. Pokud nemáte připojení k Internetu, je třeba toomanually tyto balíčky RPM najít a nainstalovat je.

```
apt-get install -y multipath-tools lsscsi python-pyasn1 lvm2 kpartx
```

### <a name="get-hello-installer-for-setup"></a>Získat hello instalační program pro instalaci

Pokud hlavní cíl se připojení k Internetu, můžete použít následující kroky toodownload hello instalačního programu hello. Můžete, jinak zkopírujte instalační službu hello z hello procesový server a potom ji nainstalovat.

#### <a name="download-hello-master-target-installation-packages"></a>Stáhněte si instalační balíčky hello hlavní cíl

[Stáhnout hello nejnovější Linux hlavní cíl instalace bits](https://aka.ms/latestlinuxmobsvc).

toodownload ho pomocí Linux, zadejte:

```
wget https://aka.ms/latestlinuxmobsvc -O latestlinuxmobsvc.tar.gz
```

Ujistěte se, stáhněte a rozbalte instalační program hello v domovském adresáři. Pokud jste příliš rozbalte**/usr/místní**, hello instalace se nezdaří.


#### <a name="access-hello-installer-from-hello-process-server"></a>Instalační program hello přístup ze serveru proces hello

1. Na serveru proces hello přejděte příliš**C:\Program Files (x86) \Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**.

2. Hello instalační program vyžaduje soubor zkopírovat z hello procesový server a uložte ho jako **latestlinuxmobsvc.tar.gz** v domovském adresáři.


### <a name="apply-custom-configuration-changes"></a>Změny vlastní konfigurace

změny tooapply vlastní konfigurace, použijte hello následující kroky:


1. Spusťte následující příkaz toountar hello binární hello.
    ```
    tar -zxvf latestlinuxmobsvc.tar.gz
    ```
    ![Snímek obrazovky toorun příkaz hello](./media/site-recovery-how-to-install-linux-master-target/image16.png)

2. Spusťte následující příkaz toogive oprávnění hello.
    ```
    chmod 755 ./ApplyCustomChanges.sh
    ```

3. Spusťte následující příkaz toorun hello skriptu hello.
    ```
    ./ApplyCustomChanges.sh
    ```
> [!NOTE]
> Spusťte skript hello jenom jednou na serveru hello. Vypněte hello server. Potom restartujte hello server po přidat disk, jak je popsáno v další části hello.

### <a name="add-a-retention-disk-toohello-linux-master-target-virtual-machine"></a>Přidat uchování disku toohello hlavní cílový virtuální počítač s Linuxem

Použijte následující postup toocreate disku pro uchování hello:

1. Připojte nový hlavní cílový virtuální počítač Linux toohello disk 1 TB a potom hello počítač spustit.

2. Použití hello **vícenásobný -udou** příkaz toolearn hello vícenásobný ID disku pro uchování hello.

    ```
    multipath -ll
    ```
    ![Hello vícenásobný ID disku pro uchování hello](./media/site-recovery-how-to-install-linux-master-target/media/image22.png)

3. Formátování hello disku a pak vytvořit systém souborů na nový disk hello.

    ```
    mkfs.ext4 /dev/mapper/<Retention disk's multipath id>
    ```
    ![Vytvoření systému souborů na jednotce hello](./media/site-recovery-how-to-install-linux-master-target/media/image23.png)

4. Po vytvoření hello systému souborů, připojte disk uchování hello.
    ```
    mkdir /mnt/retention
    mount /dev/mapper/<Retention disk's multipath id> /mnt/retention
    ```
    ![Disku pro uchování hello připojení](./media/site-recovery-how-to-install-linux-master-target/media/image24.png)

5. Vytvoření hello **fstab** položka toomount hello jednotka pro uchování pokaždé, když hello systém při spuštění.
    ```
    vi /etc/fstab
    ```
    Vyberte **vložit** toobegin úprav souboru hello. Vytvořte nový řádek a potom vložte následující text hello. Upravte ID vícenásobný hello disku podle hello zvýrazněná vícenásobný ID z předchozí příkaz hello.

     **/dev/mapper/ <Retention disks multipath id> /mnt/uchování ext4 rw 0 0**

    Vyberte **Esc**a pak zadejte **: QW** (zápisu a ukončení) okno editor tooclose hello.

### <a name="install-hello-master-target"></a>Instalovat hlavní cíl hello

> [!IMPORTANT]
> Hello verzi hello hlavní cílový server musí být rovna tooor starší než verze hello hello procesový server a hello konfigurační server. Pokud není tato podmínka splněná, opětovné ochrany úspěšná, ale replikace selže.


> [!NOTE]
> Než nainstalujete hello hlavní cílový server, zkontrolujte, že hello **/etc/hosts** soubor hello virtuálního počítače obsahuje položky, které mapují hello názvem místního hostitele toohello IP adresy, které jsou přidruženy všechny síťové adaptéry.

1. Kopírovat heslo hello z **C:\ProgramData\Microsoft Azure lokality Recovery\private\connection.passphrase** na hello konfiguračním serveru. Potom uložte ho jako **passphrase.txt** v hello hello stejného adresáře tak, že spustíte následující příkaz:

    ```
    echo <passphrase> >passphrase.txt
    ```
    Příklad: 
    
    ```
    echo itUx70I47uxDuUVY >passphrase.txt
    ```

2. Všimněte si, IP adresa serveru konfigurace hello. Budete ho potřebovat v dalším kroku hello.

3. Spusťte následující příkaz tooinstall hello hlavní cílový server hello a hello server zaregistrovat hello konfigurační server.

    ```
    ./install -q -d /usr/local/ASR -r MT -v VmWare
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <ConfigurationServer IP Address> -P passphrase.txt
    ```

    Příklad: 
    
    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i 104.40.75.37 -P passphrase.txt
    ```

    Počkejte na dokončení skriptu hello. Pokud hlavní cíl hello zaregistruje úspěšně, hlavní cíl hello je uvedený na hello **infrastruktura Site Recovery** hello portálu.


#### <a name="install-hello-master-target-by-using-interactive-installation"></a>Instalovat hlavní cíl hello pomocí interaktivní instalace

1. Spusťte následující příkaz tooinstall hello hlavní cíl hello. Pro roli hello agenta, vyberte **hlavního cíle**.

    ```
    ./install
    ```

2. Zvolte hello výchozí umístění pro instalaci a potom vyberte **Enter** toocontinue.

    ![Volba výchozí umístění pro instalaci hlavní cíl](./media/site-recovery-how-to-install-linux-master-target/image17.png)

Po dokončení instalace hello zaregistrujte konfigurační server hello pomocí příkazového řádku hello.

1. Všimněte si hello IP adresu hello konfigurační server. Budete ho potřebovat v dalším kroku hello.

2. Spusťte následující příkaz tooinstall hello hlavní cílový server hello a hello server zaregistrovat hello konfigurační server.

    ```
    ./install -q -d /usr/local/ASR -r MT -v VmWare
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <ConfigurationServer IP Address> -P passphrase.txt
    ```
    Příklad: 

    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i 104.40.75.37 -P passphrase.txt
    ```

   Počkejte na dokončení skriptu hello. Pokud hlavní cíl hello je úspěšně registrovaná, hlavní cíl hello je uvedený na hello **infrastruktura Site Recovery** hello portálu.


### <a name="upgrade-hello-master-target"></a>Hlavní cíl upgradu hello

Spusťte instalační program hello. Automaticky rozpozná, že je tento hello agent nainstalovaný na hlavním cíli hello. tooupgrade, vyberte **Y**.  Po dokončení instalace hello, zkontrolujte verzi hello hello hlavního cíle nainstalován pomocí hello následující příkaz.

    ```
    cat /usr/local/.vx_version
    ```

Uvidíte, že hello **verze** pole obsahuje číslo verze hello hello hlavní cíl.

### <a name="install-vmware-tools-on-hello-master-target-server"></a>Nainstalujte nástroje VMware na hlavní cílový server hello

Nástroje VMware tooinstall na hlavním cíli hello je nutné, aby ho můžete zjistit úložiště dat hello. Pokud nejsou nainstalovány nástroje hello, úvodní obrazovka opětovné ochrany se neuvádějí v úložišti dat hello. Po instalaci nástroje VMware hello je třeba toorestart.

## <a name="next-steps"></a>Další kroky
Po hello instalace a registrace hlavního cíle hello má finsihed, uvidíte hello hlavní cíl se zobrazují v hello **hlavního cíle** kapitoly **infrastruktura Site Recovery**, v části hello Přehled konfigurace serveru.

Teď můžete pokračovat s [vytvoření](site-recovery-how-to-reprotect.md), za nímž následují navrácení služeb po obnovení.

## <a name="common-issues"></a>Běžné problémy

* Ujistěte se, že jste nezapínejte řešení Storage vMotion na žádné součásti správy, jako je hlavní cíl. Pokud se hlavní cíl hello přesune po úspěšné opětovné ochrany, nelze odpojit hello disky virtuálního počítače (VMDKs). V takovém případě navrácení služeb po obnovení selže.

* hlavní cíl Hello by neměl mít všechny snímky hello virtuálního počítače. Pokud existují snímky, navrácení služeb po obnovení se nezdaří.

* Z důvodu toosome vlastní konfigurace síťový adaptér v někteří zákazníci hello síťové rozhraní je zakázané během spouštění a hello hlavní cíl agenta nelze inicializovat. Ujistěte se, že hello následující vlastnosti jsou správně nastaveny. Zkontrolujte tyto vlastnosti v hello Ethernet karty souboru /etc/sysconfig/network-scripts/ifcfg-eth *.
    * BOOTPROTO = dhcp
    * ONBOOT = Ano
