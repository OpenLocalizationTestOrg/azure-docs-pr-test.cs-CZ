---
title: "aaaReplicate virtuální počítače VMware nebo fyzických serverů tooanother lokality (klasický portál Azure) | Microsoft Docs"
description: "Tento článek tooreplicate virtuální počítače VMware nebo Windows nebo Linuxem fyzických serverů tooa sekundární webový server pomocí Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: nsoneji
manager: jwhit
editor: 
ms.assetid: b2cba944-d3b4-473c-8d97-9945c7eabf63
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: nisoneji
ms.openlocfilehash: 5789ca07f0aa15cf194615fd33103dac930d7b7f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-on-premises-vmware-virtual-machines-or-physical-servers-tooa-secondary-site-in-hello-classic-azure-portal"></a>Replikace lokálních virtuálních počítačů VMware nebo fyzických serverů tooa sekundární lokality na portálu Azure classic hello

## <a name="overview"></a>Přehled
InMage Scout v Azure Site Recovery poskytuje v reálném čase replikaci mezi místními servery VMware. InMage Scout je součástí předplatného služby Azure Site Recovery. 

## <a name="prerequisites"></a>Požadavky
**Účet Azure**: budete potřebovat [Microsoft Azure](https://azure.microsoft.com/) účtu. Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/). [Další informace](https://azure.microsoft.com/pricing/details/site-recovery/) o cenách za Site Recovery

## <a name="step-1-create-a-vault"></a>Krok 1: Vytvoření trezoru
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Klikněte na tlačítko Nový > Správa > Zálohování a obnovení lokality (OMS). Alternativně můžete klikněte na tlačítko Procházet > trezoru služeb zotavení > Přidat.
3. V **název** zadejte popisný název tooidentify hello trezoru. Máte-li více předplatných, vyberte jedno z nich.
4. V **skupiny prostředků** vytvořte novou skupinu prostředků nebo vyberte nějaký existující. Zadejte oblasti Azure toocomplete požadované pole.
5. V **umístění**, hello vyberte zeměpisnou oblast trezoru hello. toocheck podporované oblasti, najdete v části [ceník služby Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Pokud chcete tooquickly přístup hello trezoru z hello řídicího panelu klikněte na toodashboard PIN kód a potom klikněte na tlačítko vytvořit.
7. Hello nový trezor se zobrazí na hello řídicí panel > všechny prostředky, a na hello trezory hlavní služeb zotavení okno.

## <a name="step-2-configure-hello-vault-and-download-inmage-scout-components"></a>Krok 2: Konfigurace hello trezoru a InMage Scout součásti stáhnout
1. V okně trezory služeb zotavení hello vyberte svůj trezor a klikněte na tlačítko nastavení.
2. V **nastavení** > **Začínáme** klikněte na tlačítko **Site Recovery** > Krok 1: **Příprava infrastruktury**  >  **Cíl ochrany**.
3. V **cíl ochrany** vyberte toorecovery lokalitu a vyberte možnost Ano, s VMware vSphere hypervisoru. Pak klikněte na OK.
4. V **Scout instalace**, klikněte na možnost stažení toodownload InMage Scout 8.0.1 GA softwaru a registrační klíč. Hello instalační soubory pro všechny hello požadované součásti jsou v souboru ZIP staženého hello.

## <a name="step-3-install-component-updates"></a>Krok 3: Instalace aktualizace součástí
Přečtěte si informace o hello nejnovější [aktualizace](#updates). Nainstalujete hello soubory aktualizací na serverech v hello následující pořadí:

1. RX server, pokud existuje
2. Konfigurace serverů
3. Proces servery
4. Hlavních cílových serverů
5. vContinuum servery
6. Zdrojový server (Windows a Linux Server)

Instalovat aktualizace hello následujícím způsobem:

1. Stáhnout hello [aktualizace](https://aka.ms/asr-scout-update5) soubor .zip. Tento soubor ZIP obsahuje hello následující soubory:

   * RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.gz
   * CX_Windows_8.0.4.0_GA_Update_4_8725865_14Sep16.exe
   * UA_Windows_8.0.5.0_GA_Update_5_11525802_20Apr17.exe
   * UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz
   * vCon_Windows_8.0.5.0_GA_Update_5_11525767_20Apr17.exe
   * Uživatelský Agent update4 bits pro RHEL5, OL5, OL6, SUSE 10, SUSE 11: UA_<Linux OS>_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz
2. Extrahujte soubory .zip hello.<br>
3. **Pro hello RX server**: kopírování **RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.gz** toohello RX serveru a rozbalte ho. V hello extrahovat spusťte **/Install**.<br>
4. **Pro server proces serveru nebo konfigurace hello**: kopírování **CX_Windows_8.0.4.0_GA_Update_4_8725865_14Sep16.exe** toohello konfigurační server a procesový server. Klikněte dvakrát na toorun ho.<br>
5. **Pro hlavní cílový server Windows hello**: tooupdate hello unified agent, kopie **UA_Windows_8.0.5.0_GA_Update_5_11525802_20Apr17.exe** toohello hlavní cílový server. Klikněte dvakrát na jeho toorun ho. Všimněte si, že hello unified agent je také použít toohello zdrojového serveru, pokud nedojde k aktualizaci zdroje do Update4. Měli byste jej nainstalovat na zdrojovém serveru hello i, jak je uvedeno dále v tomto seznamu.<br>
6. **Pro hello vContinuum server**: kopírování **vCon_Windows_8.0.5.0_GA_Update_5_11525767_20Apr17.exe** toohello vContinuum server.  Ujistěte se, že jste zavřeli hello vContinuum průvodce. Poklikejte na soubor toorun hello ho.<br>
7. **Pro hlavní cílový server hello Linux**: tooupdate hello unified agent, kopie **UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** toohello hlavního cílového serveru a rozbalte ho. V hello extrahovat spusťte **/Install**.<br>
8. **Pro zdrojový server Windows hello**: Pokud zdroje pro update4 už je nepotřebujete tooinstall aktualizací 5 agenta na zdroji. Pokud je menší než update4, použít hello aktualizace 5 agenta.
tooupdate hello unified agent, kopie **UA_Windows_8.0.5.0_GA_Update_5_11525802_20Apr17.exe** toohello zdrojového serveru. Klikněte dvakrát na jeho toorun ho. <br>
9. **Pro zdrojový server hello Linux**: tooupdate hello jednotná agenta, zkopírujte odpovídající verzi uživatelský Agent souboru toohello Linux serveru a rozbalte ho. V hello extrahovat spusťte **/Install**.  Příklad: Pro RHEL 6,7 64bitový server, zkopírujte **UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** toohello serveru a rozbalte ho. V hello extrahovat spusťte **/Install**.

## <a name="step-4-set-up-replication"></a>Krok 4: Nastavení replikace
1. Nastavení replikace mezi hello zdrojem a cílem VMware lokalit.
2. Pokyny použijte s produktem hello hello dokumentaci InMage Scout, který byl stažen. Alternativně můžete dostat hello dokumentace následujícím způsobem:

   * [Poznámky k verzi](https://aka.ms/asr-scout-release-notes)
   * [Matice kompatibility](https://aka.ms/asr-scout-cm)
   * [Uživatelská příručka](https://aka.ms/asr-scout-user-guide)
   * [RX uživatelská příručka](https://aka.ms/asr-scout-rx-user-guide)
   * [Příručka pro rychlou instalaci](https://aka.ms/asr-scout-quick-install-guide)

## <a name="updates"></a>Aktualizace
### <a name="azure-site-recovery-scout-801-update-5"></a>Azure Site Recovery Scout 8.0.1 aktualizace 5
Kumulativní aktualizace je Scout aktualizace 5. Obsahuje všechny opravy hello služby aktualizaci1 do update4 a následující nové oprav chyb a vylepšení.
Opravy, které se přidají tooupdate5 update4 Scout automatické obnovení systému jsou konkrétní tooMaster cíle a vContinuum součásti. Pokud všechny zdroje jsou vaše servery, hlavního cíle, konfigurační Server, Server proces a RX již na automatické obnovení systému Scout update4 pak budete muset tooapply aktualizovat 5 jenom na hlavním cílovém serveru. 

**Nová podpora platformy**
* SUSE Linux Enterprise Server 11, aktualizace Service Pack 4(SP4)

> [!NOTE]
> 64bitový SLES 11 SP4 **InMage_UA_8.0.1.0_SLES11-SP4-64_GA_13Apr2017_release.tar.gz** je součástí balíčku základní balíček Scout GA **InMage_Scout_Standard_8.0.1 GA.zip**. Stáhněte si balíček Scout GA z portálu, jak je uvedeno v [krok 1](#step-1-create-a-vault).
>

**Opravy chyb a vylepšení**

* Zvýšit spolehlivost podpora clusteru se systémem Windows
    * Fixed – Přetrvával některé hello P2V MSCS diskům clusteru stane RAW po obnovení
    * Obnovení clusteru fixed-P2V MSCS nezdaří z důvodu neshody pořadí toodisk
    * Přidat disky, které operace se nezdaří s neshoda velikost disku clusteru fixed-MSCS
    * V velikost ověření selže clusteru MSCS zdroj fixed-s kontroly připravenosti mapování RDM logických jednotek
    * Fixed-jeden uzel clusteru ochrana se nezdaří z důvodu neshody problém tooSCSI 
    * Fixed-znovu nastavit ochranu serveru clusteru P2V Windows hello se nezdaří, pokud nejsou cílové disky clusteru. 
    
* Během navrácení služeb po obnovení ochrany pokud vybrané MT se nenachází na hello stejný server ESXi jako u hello chráněný zdrojového počítače (během dopředného ochrany), pak vContinuum převezme nesprávný MT hello během obnovení navrácení služeb po obnovení a následně operaci obnovení selže.

> [!NOTE]
> 
> * Výše opravy clusteru P2V jsou příslušné tooonly tyto fyzické clusteru MSCS, která jsou vyžadována okamžitá chráněná s update5 Scout automatické obnovení systému. tooavail hello clusteru opravy na hello už chráněný clusteru P2V MSCS s aktualizací starší, musíte hello toofollow upgradu se kroky, které jsou uvedené v části hello 12, Upgrade chráněné tooScout clusteru P2V MSCS Update5 z [verze Scout automatické obnovení systému Poznámky k](https://aka.ms/asr-scout-release-notes).
> 
> * Znovu nastavit ochranu fyzického clusteru MSCS můžete znovu použít stávající cílové disky pouze pokud v hello době znovu zapnout ochrana, hello stejnou sadu disky jsou aktivní na každém hello cluster, ve kterém uzly jako po původně chráněný. Pokud ne, pak existují ruční kroky, jak je uvedeno v části 12 [poznámky k verzi Scout automatické obnovení systému](https://aka.ms/asr-scout-release-notes) příliš přesunout hello cíl straně disky toohello správné datastore cesta toore používají je během znovu zapnout ochrana. Pokud znovu nastavte ochranu clusteru MSCS hello v režimu P2V bez následující kroky upgradu poté vytvoří nový disk na cílovém serveru ESXi pro hello. Je nutné staré disky hello toomanually odstranit z úložiště dat hello.
> 
> * Vždy, když zdroje SLES11 nebo SLES11 s jakýkoli server service pack je řádně restartovat a pak jednu by měl ručně označit hello **kořenové** jako nebudete nijak upozorněni v uživatelském rozhraní CX disku páry replikace pro znovu synchronizovat. Pokud to neuděláte, označit hello kořenové disku pro synchronizaci, mohou se zobrazit problémy s integritou (DI) data.
> 

### <a name="azure-site-recovery-scout-801-update-4"></a>Azure Site Recovery Scout 8.0.1 aktualizací 4
Scout Update 4 je kumulativní aktualizace. Obsahuje všechny opravy hello služby aktualizaci1 do update3 a následující nové oprav chyb a vylepšení.

**Nová podpora platformy**

* Byla přidána podpora pro vCenter/vSphere 6.0, 6.1 a 6.2
* Byla přidána podpora pro následující operační systémy Linux
  * Red Hat Enterprise Linux (RHEL) 7.0, 7.1 a 7.2
  * CentOS 7.0, 7.1 a 7.2
  * Red Hat Enterprise Linux (RHEL) 6.8
  * CentOS 6.8

> [!NOTE]
> RHEL nebo CentOS 7 64bitové **InMage_UA_8.0.1.0_RHEL7-64_GA_06Oct2016_release.tar.gz** je součástí balíčku základní balíček Scout GA **InMage_Scout_Standard_8.0.1 GA.zip**. Stáhněte si balíček Scout GA z portálu, jak je uvedeno v [krok 1](#step-1-create-a-vault).
>
>

**Opravy chyb a vylepšení**

* Vylepšené vypnutí zpracování pro následující operační systémy Linux a klony tooprevent nežádoucí znovu synchronizovat problémy.
  * Red Hat Enterprise Linux (RHEL) 6.x
  * Oracle Linux (OL) 6.x
* Pro systémy Linux omezený přístup dokončení složku, oprávnění v instalační adresář jednotná agenta jsou nyní pouze toohello místního uživatele.
* V systému Windows načíst vypršení časového limitu problém při vydání běžné označit kniha distribuované konzistence na výraznou distribuované aplikace jako clustery SQL a bod sdílené složky.
* Přidání protokolu související oprava v CX základní instalační služby.
* Odkaz ke stažení VMware vCLI 6.0 se přidá tooWindows základní instalační program hlavního cílového serveru.
* Během převzetí služeb při selhání a zotavení po Havárii cvičení přidat další kontroly a protokoly pro změny konfigurace sítě.
* Informace o zachování přetrvával není hlášené toohello CX.  
* Pro fyzický cluster svazek znovu velikost operace prostřednictvím Průvodce vContinuum selhává při zmenšení svazku zdroje došlo.
* Cluster ochrany selhalo s chybou "Podpis disku hello toofind se nezdařilo" Pokud disk clusteru je PRDM disk.
* cxps přenosu zhroucení serveru z důvodu výjimky out-of-range.
* Název serveru a sloupce IP je teď s možností změny velikosti nabízené instalace stránce průvodce vContinuum.
* Vylepšení RX rozhraní API
  * Poskytuje pět nejnovější dostupné společné body konzistence (pouze zaručena značek).
  * Poskytuje podrobnosti kapacity a volné místo pro všechny hello chráněné zařízení.
  * Poskytuje stav Scout ovladačů na zdrojovém serveru.

> [!NOTE]
> * **InMage_Scout_Standard_8.0.1_GA.zip** základní balíček nyní aktualizoval základní instalační program CX **InMage_CX_8.0.1.0_Windows_GA_26Feb2015_release.exe** a základní instalační služby systému Windows hlavního cíle **InMage_ Scout_vContinuum_MT_8.0.1.0_Windows_GA_26Feb2015_release.exe**. Pro všechny nové instalace použijí nové bits GA CX a Windows hlavního cíle.
> * Aktualizace 4 můžete použít přímo na 8.0.1 Všeobecné
> * Hello konfigurační server a aktualizace RX nedá se vrátit zpět, jakmile se uplatňují na hello systému.
>
>

### <a name="azure-site-recovery-scout-801-update-3"></a>Azure Site Recovery Scout 8.0.1 aktualizací 3
Aktualizace 3 zahrnuje následující hello oprav chyb a vylepšení:

* konfigurační server Hello a RX úspěšná trezoru Site Recovery toohello tooregister, když jsou za hello proxy.
* Hello počet hodin, které hello plánovaného bodu obnovení (RPO) není splněná nejsou aktualizována sestavy health hello.
* Hello konfigurační server není synchronizuje s RX, pokud hello ESX hardwaru údaje nebo podrobnosti o síti obsahují znaky znakové sady UTF-8.
* Řadiče domény systému Windows Server 2008 R2 nezdaří tooboot po obnovení.
* Offline synchronizace nefungují podle očekávání.
* Po převzetí služeb při selhání virtuálního počítače (VM) odstranění replikace páru vázne v hello uživatelského rozhraní CX po dlouhou dobu a uživatelé nebo nelze dokončit navrácení služeb po obnovení hello operace obnovení.
* Celkové byly optimalizovány s snímku operace, které provádějí úlohu konzistence hello toohelp snížit aplikace odpojí jako klienti SQL.
* vylepšili jsme výkon Hello nástroje konzistence hello (VACP.exe) snížením hello využití paměti, která je požadována pro vytváření snímků v systému Windows.
* Pokud heslo hello je větší než 16 znaků, instalace Hello push dojde k chybě služby.
* vContinuum není kontrolu a dotaz na nová pověření vCenter při změně hello pověření.
* V systému Linux není správce mezipaměti hlavní cíl hello (cachemgr) stahování souborů z hello procesní server, což vede k omezení pár replikace.
* Když hello fyzické převzetí služeb při selhání clusteru (MSCS) disku objednávka není stejná ve všech uzlech hello text hello, replikace není nastaven pro některé svazky clusteru hello.
  <br/>Všimněte si, že je tento cluster hello toobe znovu tootake výhod tato oprava.  
* Funkce SMTP nefungují podle očekávání po upgradu RX z Scout 7.1 tooScout 8.0.1.
* Další statistiky byly přidány do protokolu hello hello vrácení operace tootrack hello dobu trvá toocomplete ho.
* Byla přidána podpora pro operační systémy Linux na zdrojovém serveru hello:
  * Red Hat Enterprise Linux (RHEL) 6 aktualizace 7
  * CentOS 6 aktualizace 7
* Hello CX a RX uživatelského rozhraní můžete nyní zobrazit hello oznámení pro dvojici hello, která přejde do režimu rastrového obrázku.
* Hello následující opravy zabezpečení byly přidány v RX:

| **Popis problému** | **Implementace postupů** |
| --- | --- |
| Autorizace vynechat prostřednictvím parametru manipulaci |Použít toonon uživatelé s omezeným přístupem. |
| Padělání požadavku posílaného mezi weby |Stránka token koncept implementovaná hello, který generuje náhodně na každé stránce. <br/>S tímto uvidíte: <li> Existuje pouze jedna přihlášení instance pro hello stejného uživatele.</li><li>Aktualizace stránky nefunguje – přesměruje toohello řídicího panelu.</li> |
| Nahrávání škodlivý souborů |Rozšíření toocertain souborům s omezeným přístupem. Povolené jsou rozšíření: 7z, aiff, amp, avi, bmp, csv, dokumentů, docx, fla, flv, gif, gz, gzip, jpeg, jpg, protokolu, mid mov, mp3, mp4, mpc, mpeg, mpg, ods, odt, pdf, png, ppt, pptx, pxd, RT, paměti ram, rar, rm, rmi, rmvb, rtf, sdc, sitd, swf, sxc, sxw, vkládání , tgz, tif, tiff, txt, vsd, wav, wma, wmv, xls, xlsx, xml a zip. |
| Trvalé skriptování mezi servery |Přidat vstupní ověření. |

> [!NOTE]
> * Všechny Site Recovery aktualizace jsou kumulativní. Update 3 obsahuje všechny opravy hello Update 1 a Update 2. Update 3 můžete použít přímo na 8.0.1 Všeobecné
> * Hello konfigurační server a aktualizace RX nedá se vrátit zpět, jakmile se uplatňují na hello systému.
>
>

### <a name="azure-site-recovery-scout-801-update-2-update-03dec15"></a>Azure Site Recovery Scout 8.0.1 Update 2 (03 aktualizace DEC – 15)
Opravy v aktualizaci Update 2 patří:

* **Konfigurační server**: oprava problému, která zabránila hello 31 dnů volné měření funkce fungovat podle očekávání při hello konfigurační server se zaregistroval v Site Recovery.
* **Jednotná agenta**: opravy problému v Update 1, jejichž výsledkem není nainstalována na hlavní cílový server hello při upgradu z verze 8.0 too8.0.1 aktualizace hello.

### <a name="azure-site-recovery-scout-801-update-1"></a>Azure Site Recovery Scout 8.0.1 Update 1
Aktualizace 1 zahrnuje následující hello oprav chyb a nové funkce:

* 31 dní volné ochrany na instanci serveru. To vám umožní tootest funkce nebo nastavení testování konceptu.
  * Všechny operace na serveru hello, včetně převzetí služeb při selhání a navrácení služeb po obnovení, jsou zdarma hello první 31 dní od hello dobu, po kterou server nejprve chráněné pomocí Scout obnovení lokality.
  * Z hello 32nd dne dále, každý chráněný server se bude účtovat poplatek rychlostí hello standardní instanci Azure Site Recovery ochrany tooa vlastněných zákazníků lokality.
  * V každém okamžiku je k dispozici na stránce řídicího panelu hello trezoru Azure Site Recovery hello hello počet chráněných serverů, které jsou aktuálně vám byly účtovány poplatky.
* Přidána podpora pro rozhraní příkazového řádku (vCLI) vSphere 5.5 Update 2.
* Byla přidána podpora pro operační systémy Linux na zdrojovém serveru hello:
  * RHEL 6 aktualizaci 6
  * RHEL 5 aktualizovat 11
  * CentOS 6 aktualizací 6
  * Aktualizace centOS 5 11
* Opravy chyb tooaddress hello následující problémy:
  * Registrace trezoru nezdaří hello konfigurační server nebo RX server.
  * Svazky clusteru nezobrazí podle očekávání, pokud virtuálních počítačů v clusteru se znovu po jejich obnovení.
  * Navrácení služeb po obnovení selže, když hello hlavní cílový server je hostovaná na jiném serveru ESXi z hello místní produkční virtuálních počítačů.
  * Při upgradu too8.0.1, které ovlivňují ochranu a operace se změní konfiguraci souboru oprávnění.
  * Prahová hodnota Hello Opakovaná synchronizace se nevynucuje podle očekávání, což vede tooinconsistent replikace chování.
  * nastavení plánovaného bodu obnovení Hello se nezobrazují správně ve rozhraní hello konfigurace serveru. Hodnota dat Hello nekomprimovaným nesprávně zobrazuje hodnota hello komprimované.
  * operace odebrání Hello nedojde k odstranění podle očekávání v Průvodci vContinuum hello a replikace není odstraněný z rozhraní hello konfigurace serveru.
  * V Průvodci vContinuum hello hello disku je automaticky při kliknutí na tlačítko **podrobnosti** v zobrazení disku hello během ochrany MSCS virtuálních počítačů.
  * Během hello fyzického na virtuální (P2V) scénáři nejsou požadované HP služby, jako je například CIMnotify a CqMgHost, přesunutý toomanual v obnovení virtuálního počítače. Výsledkem další spuštění.
  * Ochranu virtuálních počítačů Linux selže, pokud jsou více než 26 disků na hlavním cílovém serveru hello.

## <a name="next-steps"></a>Další kroky
POST nějaké dotazy, které mají na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).
