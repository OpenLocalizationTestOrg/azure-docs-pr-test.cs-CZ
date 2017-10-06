---
title: "aaaFail zálohování virtuálních počítačů VMware z Azure na portálu classic starší verze hello | Microsoft Docs"
description: "Tento článek popisuje, jak replikovat zpět toofail virtuálního počítače VMware, který byl tooAzure s Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: mkjain
editor: 
ms.assetid: a63524bf-990c-42ee-8554-e017e6e67e68
ms.service: site-recovery
ms.devlang: na
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: ruturajd@microsoft.com
ms.openlocfilehash: 5ef66b366dcdc43f3bc171e0ed1532216cc2ab89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="fail-back-vmware-virtual-machines-and-physical-servers-from-azure-toovmware-with-azure-site-recovery-legacy"></a>Selhání back virtuální počítače VMware a fyzické servery z Azure tooVMware s Azure Site Recovery (zastaralé)
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-failback-azure-to-vmware.md)
> * [Portál Azure Classic](site-recovery-failback-azure-to-vmware-classic.md)
> * [Portál Azure Classic (zastaralé)](site-recovery-failback-azure-to-vmware-classic-legacy.md)
>
>

Tento článek popisuje, jak se toofail back virtuální počítače VMware a fyzické servery Windows nebo Linuxem z Azure tooyour místní lokality poté, co jste nastavili replikaci z místní lokality, pomocí tooAzure [Azure Site Recovery?](site-recovery-overview.md).

Tento článek popisuje původní konfiguraci a by nemělo být použito pro nové trezory.

## <a name="architecture"></a>Architektura
Tento diagram představuje scénář hello převzetí služeb při selhání a navrácení služeb po obnovení. Hello blue řádky představují hello připojení používaná během převzetí služeb při selhání. Hello red řádky představují hello připojení používaná během navrácení služeb po obnovení. Hello čar pomocí šipek projít hello Internetu.

![](./media/site-recovery-failback-azure-to-vmware/vconports.png)

## <a name="before-you-start"></a>Než začnete
* Můžete by měl mít převzetí služeb při selhání virtuálních počítačů VMware nebo fyzických serverů a by měly být spuštěny v Azure.
* Všimněte si, že můžete pouze nezdaří back virtuální počítače VMware a fyzické servery Windows nebo Linuxem z Azure tooVMware virtuální počítače v primární lokalitě místní hello.  Pokud po obnovení zpět fyzického počítače, tooAzure převzetí služeb při selhání bude převedena tooan virtuální počítač Azure a tooVMware navrácení služeb po obnovení bude převedena tooa virtuálního počítače VMware.

Zde je, jak nastavit navrácení služeb po obnovení:

1. **Nastavit komponenty navrácení služeb po obnovení**: budete potřebovat tooset server vContinuum na místních počítačích a nasměrujete ho toohello konfigurační server virtuálního počítače v Azure. Budete také nastavíte procesový server jako virtuální počítač Azure toosend data zpět toohello místní hlavní cílový server. Procesový server hello zaregistrujete u hello konfigurační server, který zpracovává hello převzetí služeb při selhání. Můžete nainstalovat místní hlavní cílový server. Pokud potřebujete hlavní cílový server systému Windows je nastavení automaticky při instalaci vContinuum. Pokud potřebujete Linux budete potřebovat tooset ho nahoru na samostatný server ručně.
2. **Povolení ochrany a navrácení služeb po obnovení**: po jste nastavili hello součásti, v vContinuum budete potřebovat tooenable ochrany pro přes virtuální počítače Azure se nezdařilo. Můžete spustit kontrolu připravenosti na hello virtuální počítače a spustit převzetí služeb při selhání z Azure tooyour místní lokality. Po dokončení navrácení služeb po obnovení je znovu nastavte ochranu místní počítače tak, aby jejich spuštění replikace tooAzure.

## <a name="step-1-install-vcontinuum-on-premises"></a>Krok 1: Instalace vContinuum na místě
Budete potřebovat tooinstall server vContinuum místně a nasměrovat ho toohello konfigurační server.

1. [Stáhněte si vContinuum](http://go.microsoft.com/fwlink/?linkid=526305).
2. Pak stáhnout hello [vContinuum aktualizace](http://go.microsoft.com/fwlink/?LinkID=533813) verze.
3. Nainstalujte nejnovější verzi vContinuum hello. Na hello **úvodní** klikněte na stránce **Další**.
    ![](./media/site-recovery-failback-azure-to-vmware/image2.png)
4. Na první stránce průvodce hello hello zadejte IP adresu hello CX serveru a port serveru CX hello. Vyberte **používat protokol HTTPS**.

   ![](./media/site-recovery-failback-azure-to-vmware/image3.png)
5. Najít konfigurační server hello IP adresu na hello **řídicí panel** kartě hello konfigurační server virtuálního počítače v Azure.
   ![](./media/site-recovery-failback-azure-to-vmware/image4.png)
6. Najít hello konfigurační server HTTPS veřejný port na hello **koncové body** kartě hello konfigurační server virtuálního počítače v Azure.

   ![](./media/site-recovery-failback-azure-to-vmware/image5.png)
7. Na hello **CS přístupové heslo podrobnosti** stránky zadejte přístupové heslo hello, kterou jste si poznamenali dolů při registraci hello konfigurační server. Pokud si nepamatujete ho zkontrolujte v **C:\\Program Files (x86)\\InMage systémy\\privátní\\connection.passphrase** na konfiguračním serveru hello virtuálních počítačů.

    ![](./media/site-recovery-failback-azure-to-vmware/image6.png)
8. V hello **vyberte cílové umístění** stránky zadejte, kam chcete tooinstall hello vContinuum server a klikněte na tlačítko **nainstalovat**.

   ![](./media/site-recovery-failback-azure-to-vmware/image7.png)
9. Po dokončení instalace můžete spustit vContinuum.
    ![](./media/site-recovery-failback-azure-to-vmware/image8.png)

## <a name="step-2-install-a-process-server-in-azure"></a>Krok 2: Instalace procesový server v Azure
Budete potřebovat tooinstall procesní server v Azure, aby hello virtuálních počítačů v Azure můžete odeslat hello data zpět tooan místní hlavní cílový server.

1. Na hello **konfigurační servery** stránky v Azure, vyberte tooadd nový procesový server.

   ![](./media/site-recovery-failback-azure-to-vmware/image9.png)
2. Zadejte název serveru, který proces a jméno a heslo virtuální počítač toohello tooconnect jako správce. Vyberte hello konfigurace serveru toowhich chcete tooregister hello procesový server. Měl by být hello stejný server používáte tooprotect a převzetí služeb virtuálních počítačů.
3. Zadejte hello Azure sítě, ve které hello procesový server nasadit. Měla by být hello stejné síti jako hello konfigurační server. Zadejte jedinečnou IP adresu vybranou podsíť a zahájit nasazení.

   ![](./media/site-recovery-failback-azure-to-vmware/image10.png)
4. Úloha je spouštěná toodeploy hello procesový server.

   ![](./media/site-recovery-failback-azure-to-vmware/image11.png)
5. Po nasazení hello procesový server v Azure může přihlásit k serveru hello pomocí hello přihlašovacích údajů, které jste zadali. Zaregistrujte procesový server hello v hello stejným způsobem, jakým jste zaregistrovali hello místně procesový server.

   ![](./media/site-recovery-failback-azure-to-vmware/image12.png)

> [!NOTE]
> Hello zaregistrovaných během navrácení služeb po obnovení není k dispozici v části Vlastnosti virtuálního počítače ve službě Site Recovery. Pouze jsou zobrazeny v hello **servery** kartě hello konfigurace serveru toowhich úspěšné registrace. Může trvat asi 10 až 15 minut, dokud nebude jejich hello proces serveru se zobrazí na kartě hello.
>
>

## <a name="step-3-install-a-master-target-server-on-premises"></a>Krok 3: Instalace místní hlavní cílový server
V závislosti na operačním systému zdrojového virtuálního počítače je nutné tooinstall systémem Linux nebo hlavní cílový server Windows místně.

### <a name="deploy-a-windows-master-target-server"></a>Nasazení systému Windows hlavní cílový server
Hlavní cíl Windows je již součástí vContinuum instalační program. Při instalaci hello vContinuum hlavní server je také nasazen na hello stejný počítač a registrovaný toohello konfigurační server.

1. nasazení toobegin, vytvořte prázdnou počítač místně na hostiteli ESX hello, na kterém chcete toorecover hello virtuálních počítačů z Azure.
2. Zkontrolujte, zda existují aspoň dva disky připojené toohello virtuálního počítače – jedna se používá pro hello operační systém a hello jiné se používá pro jednotka pro uchování hello.
3. Instalace operačního systému hello.
4. Nainstalujte na hello server hello vContinuum. Tím se také dokončí instalaci hello hlavní cílový server.

### <a name="deploy-a-linux-master-target-server"></a>Nasazení hlavní cílový server Linux
1. nasazení toobegin, vytvořte prázdnou počítač místně na hostiteli ESX hello, na kterém chcete toorecover hello virtuálních počítačů z Azure.
2. Zkontrolujte, zda existují aspoň dva disky připojené toohello virtuálního počítače – jedna se používá pro hello operační systém a hello jiné se používá pro jednotka pro uchování hello.
3. Nainstalujte operační systém Linux hello. Hello hlavní cílový systém Linux neměli používat LVM pro kořenovou nebo uchování prostory úložiště. Linux, které hlavní cílový server je ve výchozím nastavení nakonfigurované tooavoid LVM oddíly nebo disky zjišťování.
4. Oddíly, které můžete vytvořit:

   ![](./media/site-recovery-failback-azure-to-vmware/image13.png)
5. Před zahájením instalace hlavní cíl hello proveďte hello následujících kroků instalace post.

#### <a name="post-os-installation-steps"></a>POST kroky instalace operačního systému
tooget hello ID SCSI pro každou z pevného disku SCSI na virtuálním počítači Linux povolit hello parametr "disk. EnableUUID = TRUE "následujícím způsobem:

1. Vypněte virtuální počítač.
2. Klikněte pravým tlačítkem na položku hello virtuálních počítačů v levém panelu hello > **upravit nastavení**.
3. Klikněte na tlačítko hello **možnosti** kartě. Vyberte **Upřesnit\>obecné položky** > **parametry konfigurace**. Hello **parametry konfigurace** možnost je dostupná, jenom když hello počítač je vypnutý.

    ![](./media/site-recovery-failback-azure-to-vmware/image14.png)
4. Zkontroluje, jestli řádek s **disku. EnableUUID** existuje. Pokud nemá a je nastaven příliš**False** nastavit také**True** (ne velká a malá písmena). Pokud existuje a je nastaven tootrue, klikněte na tlačítko **zrušit** a po je při spuštění si vyzkoušet hello SCSI příkaz uvnitř hello hostovaného operačního systému. Pokud neexistuje, klikněte na tlačítko **přidat řádek**.
5. Přidáte disk. EnableUUID v hello **název** sloupce. Jeho hodnotu nastavte jako TRUE. Nepřidáte hello nad hodnotu společně s uvozovky.

    ![](./media/site-recovery-failback-azure-to-vmware/image15.png)

#### <a name="download-and-install-hello-additional-packages"></a>Stáhněte a nainstalujte hello další balíčky
Poznámka: Ujistěte se, zda text hello systému připojení k Internetu před stahování a instalace dalších balíčků hello.

\#YUM nainstalovat -y xfsprogs perl lsscsi rsync wget kexec nástroje

Tento příkaz stáhne tyto 15 balíčky z úložiště CentOS 6.6 a nainstaluje je:

BC. 1.06.95 1.el6.x86\_64. ot. / min

busybox. 1.15.1 20.el6.x86\_64. ot. / min

elfutils knihovny 0.158 3.2.el6.x86\_64. ot. / min

kexec nástroje 2.0.0 280.el6.x86\_64. ot. / min

lsscsi. 0.23 2.el6.x86\_64. ot. / min

lzo. 2.03 3.1.el6\_5.1.x86\_64. ot. / min

perlu. 5.10.1 136.el6\_6.1.x86\_64. ot. / min

Perl-Module-modulární-3.90-136.el6\_6.1.x86\_64. ot. / min

Perl-Pod – řídicí sekvence-1.04-136.el6\_6.1.x86\_64. ot. / min

Perl-Pod-jednoduché-3.13-136.el6\_6.1.x86\_64. ot. / min

Perl knihovny 5.10.1 136.el6\_6.1.x86\_64. ot. / min

Perl verze 0,77 136.el6\_6.1.x86\_64. ot. / min

RSync. 3.0.6 12.el6.x86\_64. ot. / min

Tenhle 1.1.0 1.el6.x86\_64. ot. / min

wget. 1.12 5.el6\_6.1.x86\_64. ot. / min

Pokud zdrojový počítač hello používá Reiser nebo XFS systému souborů pro hello kořenovou nebo spouštěcí zařízení, pak následující balíčky musí být stažen a nainstalován na předchozí tooprotection hlavního cíle Linuxu.

\#/usr/local disku CD

\#wget <http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm>

\#wget <http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm>

\#ot. / min - ivh kmod-reiserfs 0,0 1.el6.elrepo.x86\_64. rpm reiserfs-utils-3.6.21-1.el6.elrepo.x86\_64. ot. / min

\#wget <http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_64.rpm>

\#ot. / min - ivh xfsprogs-3.1.1-16.el6.x86\_64. ot. / min

#### <a name="apply-custom-configuration-changes"></a>Změny vlastní konfigurace
Před použitím těchto změn Ujistěte se, můžete po dokončení hello předchozí části, a potom postupujte podle těchto kroků:

1. Zkopírujte hello RHEL 6-64 Unified Agent binární toohello nově vytvořený operačního systému.
2. Spusťte tento příkaz hello toountar binární: **funkce vkládání - zxvf \<název souboru\>**
3. Spusťte tento příkaz toogive oprávnění: \# **./ApplyCustomChanges.sh chmod 755**
4. Spusťte skript hello:  **\# ./ApplyCustomChanges.sh**. Spusťte skript hello jenom jednou na serveru hello. Restartujte hello server po spuštění skriptu hello.

### <a name="install-hello-linux-server"></a>Nainstalovat server pro Linux hello
1. [Stáhněte si](http://go.microsoft.com/fwlink/?LinkID=529757) hello instalační soubor.
2. Zkopírujte soubor toohello hello virtuálnímu počítači hlavního cíle Linuxu pomocí nástroj pro klienta pomocí protokolu sftp, podle vašeho výběru. Případně můžete přihlašuje hello Linux hlavního cílového virtuálního počítače a použít wget toodownload hello instalační balíček na zadaný odkaz
3. Přihlášení do virtuálního počítače hello Linux hlavní cílový server pomocí ssh klienta podle vašeho výběru
4. Pokud jste připojení toohello Azure sítě, na kterém je nasazen Linux hlavní cílový server přes připojení VPN a používat interní IP adresu pro hello serveru, které můžete najít ve virtuálním počítači **řídicí panel** kartě a portu 22 tooconnect toohello Linux hlavní cíl serveru pomocí Secure Shell.
5. Pokud se připojujete toohello Linux hlavní cílový server přes veřejný připojení k Internetu pomocí hello Linux hlavního cílového serveru veřejná virtuální IP adresy (z virtuálních počítačů hello **řídicí panel** kartu) a hello veřejný koncový bod vytvoří pro ssh toologin toohello Linux servder.
6. Extrahujte soubory hello z vkládání archivu instalační program hlavního cílového serveru hello algoritmem gzip Linux spuštěním: *"funkce vkládání – xvzf Microsoft automatické obnovení systému\_uživatelský Agent\_8.2.0.0\_RHEL6 64\"* z adresáře hello, obsahuje soubor Instalační služby systému hello.

    ![](./media/site-recovery-failback-azure-to-vmware/image16.png)
7. Pokud jste extrahované hello instalační soubory tooa jiný adresář změnit byly extrahovat toohello directory toowhich hello obsah hello vkládání archivu. Z této cesty adresáře spustit "sudo. / install.sh".

    ![](./media/site-recovery-failback-azure-to-vmware/image17.png)
8. Když vyberte výzvami toochoose primární roli **2 (hlavní cílový)**. Nechte hello jiné možnosti interaktivní instalace na jejich výchozí hodnoty.
9. Počkejte tooappear instalace toocontinue a hello rozhraní konfigurace hostitele. Hello nástroj Konfigurace hostitele hello Linux hlavnímu cílovému serveru je nástroj příkazového řádku. Nemění velikost hello ssh okno nástroj klienta. Použití hello šipku klíče tooselect hello **globální** možnost a stiskněte klávesu ENTER na klávesnici.

    ![](./media/site-recovery-failback-azure-to-vmware/image18.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image19.png)
10. V poli hello **IP** zadejte hello interní IP adresu serveru konfigurace hello (najdete ji v hello **řídicí panel** kartě hello konfigurační server virtuálního počítače) a stiskněte klávesu ENTER. V **Port** zadejte **22** a stiskněte klávesu ENTER.
11. Nechte **použití HTTPS** jako **Ano**, a stiskněte klávesu ENTER.
12. Zadejte přístupové heslo, který byl vygenerován při hello konfigurační server hello. Pokud používáte PUTTY klienta z Windows počítač toossh toohello virtuální počítač s Linuxem můžete Shift + Insert toopaste hello obsah schránky hello. Zkopírujte hello přístupové heslo toohello místní schránky pomocí kombinace kláves Ctrl-C a použijte Shift + Insert toopaste ho. Stiskněte klávesu ENTER.
13. Pomocí tooquit klíče toonavigate hello šipku vpravo a stiskněte klávesu ENTER. Počkejte toocomplete instalace.

    ![](./media/site-recovery-failback-azure-to-vmware/image20.png)

Pokud z nějakého důvodu selhala tooregister Linux hlavní cílový server toohello konfigurační server můžete provést tak znovu tak, že spustíte nástroj Konfigurace hostitele /usr/local/ASR/Vx/bin/hostconfigcli (je nutné nejdříve toothis oprávnění přístupu tooset adresář spuštěním chmod jako superuživatele).

#### <a name="validate-master-target-registration"></a>Ověření registrace hlavního cíle
Můžete ověřit, že hello hlavní cílový server byl úspěšně zaregistrován toohello konfigurační server v trezoru Azure Site Recovery > **konfigurační Server** > **podrobnosti o serveru**.

> [!NOTE]
> Po registraci hello hlavní cílový server, pokud se zobrazí chyby konfigurace, které hello virtuální počítač byl pravděpodobně odstraněn z Azure nebo nejsou správně nakonfigurované koncové body, je to proto, že i když hello je zjištěna hlavní cíl konfigurace podle hello Azure dndpoints při hello hlavní cíl nasazení do Azure, tato akce není pro místní hlavní cílový server místní na hodnotu true. To nebude mít vliv na navrácení služeb po obnovení a tyto chyby můžete ignorovat.
>
>

## <a name="step-4-protect-hello-virtual-machines-toohello-on-premises-site"></a>Krok 4: Ochrana hello virtuální počítače toohello místního serveru
Než budete navrácení služeb po obnovení, musíte tooprotect virtuální počítače toohello místního webu.

### <a name="before-you-begin"></a>Než začnete
Když virtuální počítač při selhání tooAzure, přidá jednotce velmi dočasného souboru stránky. To je další jednotky, která není obvykle nutné ve vaší převzal virtuálních počítačů, protože je již můžete mít na jednotku vyhrazené pro stránkovací soubor. Než začnete zpětné ochrany hello virtuálních počítačů, je potřeba zajistit, že této jednotky do režimu offline, aby nejsou chráněny. Proveďte to následujícím způsobem:

1. Otevřete správu počítače a vyberte položku Správa úložiště tak, aby vypíše hello disky online a připojené toohello počítače.
2. Vyberte počítač připojený toohello dočasným diskovým hello a zvolte toobring ho do režimu offline.

### <a name="protect-hello-vms"></a>Ochrana virtuálních počítačů hello
1. V hello portálu Azure zkontrolujte hello stavy tooensure hello virtuální počítač, který jste při selhání.

    ![](./media/site-recovery-failback-azure-to-vmware/image21.png)
2. Spusťte vContinuum na váš počítač.

    ![](./media/site-recovery-failback-azure-to-vmware/image8.png)
3. Klikněte na tlačítko **novou ochranu** a vyberte typ systému hello operace,
4. V novém okně hello, který otevřete vyberte hello **typ operačního systému** > **získat podrobnosti o** pro hello virtuální počítače chcete toofail zpět. V **primární server podrobnosti**, zjišťovat a vyberte hello virtuálních počítačů, které chcete tooprotect. Virtuální počítače jsou uvedené pod názvem hello vCenter hostitele, které byly na před převzetí služeb při selhání.
5. Když vyberete tooprotect virtuálního počítače (a má již převzal tooAzure) automaticky otevírané okno poskytuje dvě položky pro virtuální počítač hello. Je to proto, že konfigurační server hello zjistí dvě instance virtuálních počítačů, které jsou zaregistrované tooit hello. Je třeba položku hello tooremove pro hello místní virtuální počítač tak, aby můžete chránit hello správné virtuálních počítačů. tooidentify hello správnou virtuální počítač Azure položku zde, může se přihlásit k hello virtuální počítač Azure a přejděte tooC:\Program soubory (x86) \Microsoft Azure Site Recovery\Application Data\etc. V souboru drscout.conf hello Identifikujte ID hello hostitele. V dialogovém okně vContinuum hello zachovat hello položku, u kterého jste našli ID hostitele hello na hello virtuálních počítačů. Odstraňte všechny ostatní položky. tooselect hello opravte virtuální počítač může odkazovat tooits IP adresu. Hello IP adresa rozsahu místní bude hello místní počítač.

   ![](./media/site-recovery-failback-azure-to-vmware/image22.png)

   ![](./media/site-recovery-failback-azure-to-vmware/image23.png)
6. Na serveru vCenter hello zastavte hello virtuální počítač. Můžete také odstranit hello virtuálních počítačích na místě.
7. Potom zadejte hello místní MT server toowhich chcete tooprotect hello virtuálních počítačů. toodo tohoto připojení toohello vCenter server toowhich chcete toofail zpět.

    ![](./media/site-recovery-failback-azure-to-vmware/image24.png)
8. Vyberte hello hlavní cílový server založený na hello hostitele toowhich chcete toorecover hello virtuálních počítačů.

    ![](./media/site-recovery-failback-azure-to-vmware/image24.png)
9. Zadejte hello možnost replikace pro každou hello virtuálních počítačů. toodo to budete potřebovat tooselect hello obnovení straně úložiště toowhich hello virtuální počítače budou obnoveny. Hello tabulka shrnuje různé možnosti hello tooprovide musíte pro každý virtuální počítač.

    ![](./media/site-recovery-failback-azure-to-vmware/image25.png)

   | **Možnost** | **Možnost doporučená hodnota** |
   | --- | --- |
   |  IP adresa serveru procesů |Vyberte procesový server hello nasazené v Azure |
   |  Uchování velikost v MB | |
   |  Hodnotu uchování |1 |
   |  Počet dnů nebo hodin |Dnů |
   |  Interval konzistence |1 |
   |  Vyberte cílové úložiště |Hello úložiště k dispozici v lokalitě obnovení hello. úložiště dat Hello by měl dostatek místa a být hostitele ESX k dispozici toohello, na kterém chcete toorestore hello virtuálního počítače. |
10. Nakonfigurujte hello po převzetí služeb při selhání tooon místní lokalitu se získat vlastnosti, které hello virtuálního počítače. Hello vlastnosti jsou shrnuty v následující tabulce hello.

    ![](./media/site-recovery-failback-azure-to-vmware/image26.png)

    | **Vlastnost** | **Podrobnosti** |
    | --- | --- |
    | Konfigurace sítě |Pro každý síťový adaptér zjištěn, vyberte ho a klikněte na tlačítko **změnu** tooconfigure hello navrácení služeb po obnovení IP adresu pro virtuální počítač hello. |
    | Konfigurace hardwaru |Zadejte hello procesoru a paměti hello pro hello virtuálních počítačů. Nastavení může být použité tooall hello virtuálních počítačů, které se pokoušíte tooprotect. tooidentify Dobrý den správné hodnoty pro hello procesoru a paměti, můžete odkazovat toohello velikost role virtuálních počítačů IAAS a zobrazit hello počet jader a paměti. |
    | Zobrazované jméno |Po navrácení služeb po obnovení tooon místní můžete přejmenovat hello virtuálních počítačů, jak se objeví v hello vCenter inventáře. Hello výchozí název je název hostitele počítače hello virtuálního počítače. název virtuálního počítače hello tooidentify, mohou odkazovat toohello seznamu virtuálních počítačů ve skupině ochrany hello. |
    | Konfigurace NAT |Podrobněji níže |

    ![](./media/site-recovery-failback-azure-to-vmware/image27.png)

#### <a name="configure-nat-settings"></a>Konfigurace nastavení NAT.
1. tooenable ochranu hello virtuální počítače, dva komunikační kanály potřebovat toobe navázat. první kanál Hello je mezi hello virtuálního počítače a hello procesového serveru. Tento kanál shromažďuje hello data z hello virtuálních počítačů a odešle ji toohello procesový server, které pak odešle hello data toohello hlavní cílový server. Pokud hello procesový server a toobe hello virtuální počítač chráněný, jsou na hello stejnou virtuální síť Azure, pak je nepotřebujete toouse NAT nastavení. V opačném případě zadejte nastavení překladu síťových adres. Zobrazení hello veřejnou IP adresu hello procesní server v Azure.

    ![](./media/site-recovery-failback-azure-to-vmware/image28.png)
2. druhý kanál Hello je mezi hello procesový server a hlavní cílový server hello. Hello možnost toouse NAT nebo není závisí na tom, jestli se pomocí připojení VPN na základě nebo komunikaci přes hello Internetu. Nevybírejte NAt, pokud používáte sítě VPN, ale jenom v případě, že používáte připojení k Internetu.

    ![](./media/site-recovery-failback-azure-to-vmware/image29.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image30.png)
3. Pokud nebyly odstraněny hello místní virtuální počítače jako zadaný a pokud jste hello úložiště selhání back toostill obsahuje hello staré VMDK, bude nutné tooensure této hello navrácení virtuální počítač získá vytvořený na nové místo. toodo tento vyberte hello **Upřesnit** nastavení a zadejte alternativní toorestore tooin složky **nastavení název složky**. Nechte hello další možnosti ve výchozím nastavení. Použijte hello složky název nastavení tooall hello servery.

    ![](./media/site-recovery-failback-azure-to-vmware/image31.png)
4. Spustit připravenosti kontrolu tooensure že hello virtuální počítače jsou připraveny toobe chráněný back tooon místní.

    ![](./media/site-recovery-failback-azure-to-vmware/image32.png)
5. Počkejte na její toocomplete. Pokud je úspěšně na všech virtuálních počítačích můžete zadat název plánu ochrany hello. Pak klikněte na tlačítko **chránit** toobegin.

    ![](./media/site-recovery-failback-azure-to-vmware/image33.png)
6. Můžete sledovat průběh v vContinuum.

    ![](./media/site-recovery-failback-azure-to-vmware/image34.png)
7. Po dokončení kroku hello **aktivace plán ochranu** dokončení můžete monitorovat ochranu virtuálního počítače na portálu Site Recovery hello.

    ![](./media/site-recovery-failback-azure-to-vmware/image35.png)
8. Přesný stav hello kliknete na hello virtuálních počítačů a monitorování jeho průběh můžete zobrazit.

    ![](./media/site-recovery-failback-azure-to-vmware/image36.png)

## <a name="prepare-hello-failback-plan"></a>Příprava plánu hello navrácení služeb po obnovení
Můžete připravit plán navrácení služeb po obnovení pomocí vContinuum tak, aby v každém okamžiku může selhání toohello zpět na místní lokalitu se hello aplikace. Tyto plány obnovení jsou velmi podobné plány obnovení toohello ve službě Site Recovery.

1. Spusťte vContinuum a vyberte **Spravovat plány** > **obnovit.** Najdete v seznamu všechny hello plány, které byly použité toofail přes virtuální počítače. Můžete použít hello stejné plány toorecover.

   ![](./media/site-recovery-failback-azure-to-vmware/image37.png)
2. Vyberte plán ochranu hello a všechny hello chcete toorecover v ní virtuální počítače. Když vyberete každý virtuální počítač se zobrazí další podrobnosti, včetně hello cílový ESX server a hello zdrojového virtuálního počítače disku. Klikněte na tlačítko **Další** toobegin hello obnovit průvodce a vyberte virtuální počítače hello chcete toorecover.

    ![](./media/site-recovery-failback-azure-to-vmware/image38.png)
3. Můžete obnovit podle několik možností, ale doporučujeme použít **nejnovější značky** a vyberte **použít pro všechny virtuální počítače** tooensure, který hello nejnovější data z hello virtuálního počítače se použije.
4. Spustit hello **Kontrola připravenosti.** To zkontroluje, jestli jsou správné parametry hello nakonfigurované tooenable obnovení virtuálního počítače. Klikněte na tlačítko **Další** Pokud všechny kontroly hello jsou úspěšné. Není-li protokol hello a vyřešte chyby hello.

    ![](./media/site-recovery-failback-azure-to-vmware/image39.png)
5. V **konfigurace virtuálního počítače** ověřte, zda jsou správně nastaveny nastavení obnovení hello. Pokud potřebujete, můžete změnit nastavení virtuálního počítače hello.

   ![](./media/site-recovery-failback-azure-to-vmware/image40.png)
6. Projděte si seznam hello virtuálních počítačů, které budou obnoveny a určete pořadí obnovení. Všimněte si, že virtuální počítače jsou uvedeny pomocí hello název hostitelského počítače. Může být obtížné toomap hello počítače hostitele název toohello virtuálního počítače.
   názvy hello toomap, přejděte toohello virtuální počítače **řídicí panel** v Azure a zkontrolujte název hostitele virtuálního počítače hello.

    ![](./media/site-recovery-failback-azure-to-vmware/image41.png)
7. Zadejte název plánu a vyberte **později obnovit**. Doporučujeme, abyste toorecover později vzhledem k tomu, že počáteční ochraně hello nemusí být úplná.
8. Klikněte na **obnovit** toosave hello plán nebo aktivační událost hello zotavení, pokud jste vybrali toorecover teď a ne později. Hello obnovení stavu toosee můžete zkontrolovat, pokud byla uloženého hello plánu.

   ![](./media/site-recovery-failback-azure-to-vmware/image42.png)

   ![](./media/site-recovery-failback-azure-to-vmware/image43.png)

## <a name="recover-virtual-machines"></a>Obnovení virtuálních počítačů
Po vytvoření plánu hello, můžete obnovit hello virtuálních počítačů. Před zkontrolujte, že hello virtuálních počítačů dokončili synchronizace. Pokud replikace ve stavu OK dokončení ochrany hello a prahovou hodnotu RPO hello byla splněna. Můžete ověřit stav ve vlastnostech virtuálního počítače hello.

![](./media/site-recovery-failback-azure-to-vmware/image44.png)

Než zahájíte obnovení hello vypněte na hello virtuální počítače Azure. To zajišťuje neexistuje žádné schizofrenní a že uživatelé budou přistupovat pouze jedna kopie aplikace hello.

1. Spusťte hello uložit plán. V vContinuum vyberte **monitorování** hello plány. Rutina Vypíše seznam všech hello plány, které byly spuštěny.

   ![](./media/site-recovery-failback-azure-to-vmware/image45.png)
2. Vyberte hello plán v **obnovení** a klikněte na tlačítko **spustit**. Obnovení můžete sledovat. Po zapnuli virtuálních počítačů na se můžete připojit toothem v vCenter.

   ![](./media/site-recovery-failback-azure-to-vmware/image46.png)

## <a name="protect-tooazure-again-after-failback"></a>Chránit tooAzure znovu po navrácení služeb po obnovení
Po dokončení navrácení služeb po obnovení bude pravděpodobně potřeba tooprotect hello virtuální počítače znovu. Proveďte to následujícím způsobem:

1. Zkontrolujte funkčnost hello virtuálních počítačích na místě a jestli jsou dostupné aplikace.
2. Na portálu Azure Site Recovery hello vyberte hello virtuální počítače a odstraňte je. Možnost ochrany hello toodisable hello virtuálních počítačů. Díky žádné další ochrany virtuálních počítačů hello.
3. Odstraňte hello převzal virtuální počítače Azure v Azure
4. Odstranění hello původního virtuálního počítače na vSpehere. Jedná se o hello virtuálních počítačů, které jste dříve převzetí služeb při selhání tooAzure.
5. Na portálu Site Recovery hello Chraňte hello virtuálních počítačů, které nedávno převzít služby při selhání. Po, že jsou chráněné přidáním tooa plánu obnovení.

## <a name="next-steps"></a>Další kroky
* [Přečtěte si informace o](site-recovery-vmware-to-azure-classic.md) replikovat virtuální počítače VMware a fyzické servery tooAzure pomocí hello rozšířeného nasazení.
