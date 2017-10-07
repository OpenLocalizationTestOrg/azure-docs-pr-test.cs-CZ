---
title: "aaaDeploy zařízení StorSimple (Update 1) | Microsoft Docs"
description: "Popisuje hello kroky a osvědčené postupy nasazení zařízení StorSimple Update 1 hello a služby."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: ac631d3c-3c53-4c9e-9e4a-5c61c0cd8167
ms.service: storsimple
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 339b68f29a73bb77670e76e454cf271c7de4a6e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-on-premises-storsimple-device-update-1"></a>Nasazení místního zařízení StorSimple (Update 1)
> [!div class="op_single_selector"]
> * [Update 2](storsimple-deployment-walkthrough-u2.md)
> * [Update 1](storsimple-deployment-walkthrough-u1.md)
> * [Verze GA](storsimple-deployment-walkthrough.md)
> 
> 

## <a name="overview"></a>Přehled
Vítejte tooMicrosoft nasazení zařízení StorSimple v Azure. Tyto kurzy nasazení se vztahují tooStorSimple 8000 Update 1.0 řady. Tato série kurzů popisuje, jak tooconfigure zařízení StorSimple a zahrnuje kontrolní seznam konfigurace, požadavky konfigurace a podrobnou konfiguraci kroky.

Hello informace v těchto kurzech předpokládá, že kontrole hello bezpečnostní opatření a vybaleno, namontovali do racku a zapojena jeho zařízení StorSimple. Pokud stále potřebujete tooperform ty úlohy, začněte prostudováním hello [bezpečnostní opatření](storsimple-safety.md). V závislosti na modelu zařízení, můžete poté můžete zařízení vybalit, namontovat do racku a zapojit jeho kabeláž podle pokynů hello v:

* [Zařízení 8100 – Vybalení, namontování do racku a zapojení kabeláže](storsimple-8100-hardware-installation.md)
* [Zařízení 8600 – Vybalení, namontování do racku a zapojení kabeláže](storsimple-8600-hardware-installation.md)

Budete potřebovat správce oprávnění toocomplete hello procesu instalace a konfigurace. Doporučujeme, abyste si prošli kontrolní seznam konfigurace hello před zahájením. proces nasazení a konfigurace Hello může trvat některé toocomplete čas.

> [!NOTE]
> informace o nasazení Hello StorSimple publikované na webu Microsoft Azure hello platí tooStorSimple 8000 řady jenom zařízení. Úplné informace o řadě zařízení hello 5000 a 7000 najdete v tématu: [http://onlinehelp.storsimple.com/](http://onlinehelp.storsimple.com). 5000 a 7000 řady informace o nasazení naleznete v tématu hello [StorSimple úvodní příručce k systému](http://onlinehelp.storsimple.com/111_Appliance/).
> 
> 

## <a name="deployment-steps"></a>Kroky nasazení
Proveďte tyto kroky požadované tooconfigure zařízení StorSimple a připojte ho tooyour služby StorSimple Manager. Kromě toho toohello potřebné kroky, jsou volitelné kroky a postupy, které může být vhodné při nasazení hello. Hello podrobný postup nasazení pokyny o tom, kdy byste měli dělat každý z těchto kroků volitelné.

| Krok | Popis |
| --- | --- |
| **POŽADAVKY** |Tyto potřebovat toobe splnit v rámci přípravy nasazení hello. |
| Kontrolní seznam konfigurace nasazení |Tento kontrolní seznam toogather a zaznamenávat informace předchozí tooand použijte během nasazení hello. |
| Požadavky nasazení |Toto ověření hello prostředí je připravený pro nasazení. |
|  | |
| **PODROBNÝ POSTUP NASAZENÍ** |Tyto kroky jsou požadované toodeploy zařízení StorSimple v produkčním prostředí. |
| Krok 1: Vytvoření nové služby |Nastavte správu cloudu a úložiště pro zařízení StorSimple. Pokud máte existující službu pro jiná zařízení StorSimple, tento krok přeskočte. |
| Krok 2: Získání registračního klíče služby hello. |Pomocí tohoto klíče tooregister & připojení zařízení StorSimple se službou správy hello. |
| Krok 3: Konfigurace a registrace zařízení hello pomocí Windows Powershellu pro StorSimple. |Připojit síť tooyour hello zařízení a zaregistrovat ji pomocí Azure toocomplete hello instalaci pomocí služby správy hello. |
| Krok 4: Dokončení minimální instalace zařízení</br>Volitelné: Aktualizace zařízení StorSimple |Použijte hello management service toocomplete hello nastavení zařízení a povolit tooprovide úložiště. |
| Krok 5: Vytvoření kontejneru svazků |Vytvoření kontejneru svazků tooprovision. Kontejner svazků obsahuje účet úložiště, šířku pásma a nastavení šifrování pro všechny svazky hello v ní obsažené. |
| Krok 6: Vytvoření svazku |Zřiďte svazky úložiště v zařízení StorSimple hello vašich serverů. |
| Krok 7: Připojení, inicializace a formátování svazků</br>Volitelné: Konfigurace funkce MPIO |Připojte vaše servery úložiště iSCSI toohello poskytované hello zařízení. Volitelně konfigurujte funkci MPIO tooensure, že servery budou tolerovat připojení, sítě a rozhraní selhání. |
| Krok 8: Provedení zálohy |Nastavit tooprotect vaše zásady zálohování dat |
|  | |
| **DALŠÍ POSTUPY** |Při nasazení řešení může být nutné toorefer toothese postupy. |
| Konfigurace nového účtu úložiště pro službu hello. | |
| Pomocí konzoly sériového portu zařízení PuTTY tooconnect toohello. | |
| Získání názvu IQN hostitele serveru Windows hello. | |
| Vytvoření ruční zálohy | |

## <a name="deployment-configuration-checklist"></a>Kontrolní seznam konfigurace nasazení
Hello následující kontrolní seznam konfigurace nasazení popisuje informace hello, je nutné toocollect, než a při konfiguraci hello softwaru zařízení StorSimple. Předběžná příprava některých z těchto informací předem, bude vám pomůžou zjednodušit hello proces nasazení zařízení StorSimple hello ve vašem prostředí. Nasazení zařízení, použijte tento kontrolní seznam tooalso Poznámka dolů hello podrobnosti konfigurace.

| Krok | Parametr | Podrobnosti | Hodnoty |
| --- | --- | --- | --- |
| **Zapojení kabeláže zařízení** |Sériový přístup |Počátečního konfigurace zařízení |Ano/Ne |
|  | | | |
| **Konfigurace a registrace zařízení** |Nastavení síťového rozhraní DATA 0 |IP adresa síťového rozhraní DATA 0:</br>Maska podsítě:</br>Brána:</br>Primární server DNS:</br>Primární server NTP:</br>Webový proxy server protokolu IP nebo plně kvalifikovaný název domény (volitelné):</br>Port webového proxy serveru: | |
| &nbsp; |Heslo správce zařízení |Heslo musí být tvořeno 8 až 15 znaky a musí obsahovat malá písmena, velká písmena, číslice a speciální znaky. | |
| &nbsp; |Heslo správce Snapshot Manageru zařízení StorSimple |Heslo musí být tvořeno 14 až 15 znaky a musí obsahovat malá písmena, velká písmena, číslice a speciální znaky. | |
| &nbsp; |Registrační klíč služby |Tento klíč se generují z hello portál Azure classic. | |
| &nbsp; |Datový šifrovací klíč služby |Tento klíč je vytvořen po registraci zařízení hello službou management hello prostřednictvím hello prostředí Windows PowerShell pro StorSimple. Klíč zkopírujte a uložte na bezpečném místě. | |
|  | | | |
| **Provedení minimálního nastavení zařízení** |Popisný název zařízení |Toto je popisný název zařízení hello. | |
| &nbsp; |Časové pásmo |Toto časové pásmo bude zařízení používat pro všechny naplánované operace. | |
| &nbsp; |Sekundární server DNS |Toto je požadovaná konfigurace. | |
| &nbsp; |Síťové rozhraní: Pevné IP adresy řadiče síťového rozhraní DATA 0 |Tyto IP adresy musí být směrovatelné toohello Internetu.</br>Pevná IP adresa řadiče síťového rozhraní DATA 0:</br>Pevná IP adresa řadiče síťového rozhraní DATA 1: | |
|  | | | |
| **Další nastavení síťového rozhraní** |Síťové rozhraní: DATA 1</br>Pokud je povoleno iSCSI, neprovádějte konfiguraci hello brány. |Účel: Cloud / iSCSI / nepoužito</br>IP adresa:</br>Maska podsítě:</br>Brána: | |
| &nbsp; |Síťové rozhraní: DATA 2</br>Pokud je povoleno iSCSI, neprovádějte konfiguraci hello brány. |Účel: Cloud / iSCSI / nepoužito</br>IP adresa:</br>Maska podsítě:</br>Brána: | |
| &nbsp; |Síťové rozhraní: DATA 3</br>Pokud je povoleno iSCSI, neprovádějte konfiguraci hello brány. |Účel: Cloud / iSCSI / nepoužito</br>IP adresa:</br>Maska podsítě:</br>Brána: | |
| &nbsp; |Síťové rozhraní: DATA 4</br>Pokud je povoleno iSCSI, neprovádějte konfiguraci hello brány. |Účel: Cloud / iSCSI / nepoužito</br>IP adresa:</br>Maska podsítě:</br>Brána: | |
| &nbsp; |Síťové rozhraní: DATA 5</br>Pokud je povoleno iSCSI, neprovádějte konfiguraci hello brány. |Účel: Cloud / iSCSI / nepoužito</br>IP adresa:</br>Maska podsítě:</br>Brána: | |
|  | | | |
| **Vytvoření kontejneru svazků** |Název kontejneru svazků: |Název kontejneru hello | |
| &nbsp; |Účet úložiště Azure: |Úložiště účet název & přístup klíče tooassociate s tímto kontejnerem svazků | |
| &nbsp; |Šifrovací klíč cloudového úložiště: |Šifrovací klíč pro úložiště v jednotlivých kontejnerech | |
|  | | | |
| **Vytvoření svazku** |Podrobnosti jednotlivých svazků |Název svazku: | |
| &nbsp; |&nbsp; |Velikost: | |
| &nbsp; |&nbsp; |Typ použití: | |
| &nbsp; |&nbsp; |Název ACR: | |
| &nbsp; |&nbsp; |Výchozí zásady zálohování: | |
|  | | | |
| **Připojení, inicializace a formátování svazku** |Podrobnosti o každém hostitelském serveru připojení toohello úložiště |Název systému Windows Server: | |
| &nbsp; |&nbsp; |Název IQN systému Windows Server: | |
| &nbsp; |&nbsp; |Název svazku systému Windows Server: | |
| &nbsp; |&nbsp; |Přípojný bod systému souborů NTFS nebo písmeno jednotky: | |

## <a name="deployment-prerequisites"></a>Požadavky nasazení
Hello následující části popisují požadavky konfigurace hello služby StorSimple Manager a zařízení StorSimple.

### <a name="for-hello-storsimple-manager-service"></a>Pro hello služby StorSimple Manager
Než začnete, ujistěte se, že:

* Máte účet Microsoft a přihlašovací údaje účtu.
* Máte účet služby Microsoft Azure Storage a přihlašovací údaje účtu.
* Vaše předplatné Microsoft Azure je povoleno pro hello služby StorSimple Manager. Vaše předplatné by mělo být zakoupeno prostřednictvím hello [smlouva Enterprise Agreement](https://azure.microsoft.com/pricing/enterprise-agreement/).
* Máte přístup k softwaru pro emulaci tooterminal jako je například PuTTY.

### <a name="for-hello-device-in-hello-datacenter"></a>Pro zařízení hello v datovém centru hello
Před konfigurací zařízení hello, ujistěte se, že:

* Zařízení je zcela vybaleno, namontováno v racku a je kompletně zapojena jeho kabeláž napájení, sítě a sériového přístupu, jak je popsáno v následujících článcích:
  
  * [Zařízení 8100 – Vybalení, namontování do racku a zapojení kabeláže](storsimple-8100-hardware-installation.md)
  * [Zařízení 8600 – Vybalení, namontování do racku a zapojení kabeláže](storsimple-8600-hardware-installation.md)

### <a name="for-hello-network-in-hello-datacenter"></a>Pro síť hello v datovém centru hello
Než začnete, ujistěte se, že:

* Hello porty v bráně firewall datového centra jsou otevřené tooallow pro přenosy iSCSI a cloudu jak je popsáno v [požadavcích sítě pro zařízení StorSimple](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).

## <a name="step-by-step-deployment"></a>Podrobný postup nasazení
Použijte následující podrobné pokyny toodeploy hello zařízení StorSimple v datovém centru hello.

## <a name="step-1-create-a-new-service"></a>Krok 1: Vytvoření nové služby
Služba StorSimple Manager může spravovat více zařízení StorSimple. Proveďte následující kroky toocreate novou instanci třídy služby StorSimple Manager hello hello.

[!INCLUDE [storsimple-create-new-service](../../includes/storsimple-create-new-service.md)]

> [!IMPORTANT]
> Pokud jste nepovolili automatické vytvoření účtu úložiště hello s službou, budete potřebovat toocreate alespoň jeden účet úložiště po úspěšném vytvoření služby. Tento účet úložiště se použije při vytváření kontejneru svazku.
> 
> * Pokud nebyl vytvořen účet úložiště automaticky, přejděte příliš[konfigurace nového účtu úložiště pro službu hello](#configure-a-new-storage-account-for-the-service) podrobné pokyny.
> * Pokud jste povolili hello automatické vytvoření účtu úložiště, pokračujte příliš[krok 2: registrační klíč služby hello Get](#step-2-get-the-service-registration-key).
> 
> 

## <a name="step-2-get-hello-service-registration-key"></a>Krok 2: Získání registračního klíče služby hello
Po hello služby StorSimple Manager je spuštěný a funkční, budete potřebovat registrační klíč služby hello tooget. Tento klíč je použité tooregister a připojení zařízení StorSimple službou hello.

Proveďte následující kroky v hello portál Azure classic hello.

[!INCLUDE [storsimple-get-service-registration-key](../../includes/storsimple-get-service-registration-key.md)]

## <a name="step-3-configure-and-register-hello-device-through-windows-powershell-for-storsimple"></a>Krok 3: Konfigurace a registrace zařízení hello pomocí Windows Powershellu pro StorSimple
Pomocí prostředí Windows PowerShell pro StorSimple toocomplete hello počátečním nastavení zařízení StorSimple, jak je popsáno v hello následující postup. Toocomplete software pro emulaci terminálu toouse bude nutné tento krok. Další informace najdete v tématu [konzoly sériového portu toohello zařízení tooconnect pro použití klienta PuTTY](#use-putty-to-connect-to-the-device-serial-console).

[!INCLUDE [storsimple-configure-and-register-device-u1](../../includes/storsimple-configure-and-register-device-u1.md)]

## <a name="step-4-complete-minimum-device-setup"></a>Krok 4: Dokončení minimální instalace zařízení
Pro hello minimální konfigurace zařízení zařízení StorSimple je potřeba:

* Nastavte sekundární server DNS hello.
* Nejméně v jednom síťovém rozhraní povolit iSCSI.
* Přiřadíte pevné IP adresy řadiče tooboth hello.

Proveďte následující kroky v hello Azure classic portálu toocomplete hello minimální instalace zařízení hello.

[!INCLUDE [storsimple-complete-minimum-device-setup](../../includes/storsimple-complete-minimum-device-setup-u1.md)]

## <a name="step-5-create-a-volume-container"></a>Krok 5: Vytvoření kontejneru svazků
Kontejner svazků obsahuje účet úložiště, šířku pásma a nastavení šifrování pro všechny svazky hello v ní obsažené. Před zahájením zřizování svazků v zařízení StorSimple budete potřebovat toocreate kontejner svazků.

Proveďte následující kroky v hello Azure classic portálu toocreate kontejner svazků hello.

[!INCLUDE [storsimple-create-volume-container](../../includes/storsimple-create-volume-container.md)]

## <a name="step-6-create-a-volume"></a>Krok 6: Vytvoření svazku
Po vytvoření kontejneru svazků můžete zřídit svazek úložiště na zařízení StorSimple hello pro své servery. Proveďte hello proveďte kroky v hello Azure classic portálu toocreate svazku.

> [!IMPORTANT]
> Služba StorSimple Manager umožňuje vytvářet pouze dynamicky zřizované svazky. Nelze vytvářet zcela či částečně zřizované svazky.
> 
> 

[!INCLUDE [storsimple-create-volume](../../includes/storsimple-create-volume.md)]

## <a name="step-7-mount-initialize-and-format-a-volume"></a>Krok 7: Připojení, inicializace a formátování svazků
Hello následující kroky se provádějí v hostiteli systému Windows Server.

> [!IMPORTANT]
> * Hello vysoké dostupnosti vašeho řešení StorSimple doporučujeme konfigurace funkce MPIO na vaše iSCSI předchozí tooconfiguring servery (volitelné) hostitele. Konfigurace funkce MPIO na hostitelských serverech zajistí, že hello servery budou tolerovat odkaz, sítě nebo selhání rozhraní.
> * Funkce MPIO a standardu iSCSI instalace a konfigurace pokyny na hostitelském Windows serveru najdete příliš[konfigurace funkce MPIO pro zařízení StorSimple](storsimple-configure-mpio-windows-server.md). Tyto také bude zahrnovat hello kroky toomount, inicializace a formátování svazků zařízení StorSimple.
> * Funkce MPIO a standardu iSCSI instalace a konfigurace pokyny na hostitele platformy Linux najdete příliš[konfigurace funkce MPIO pro hostitele se systémem StorSimple Linux](storsimple-configure-mpio-on-linux.md)
> 
> 

Pokud se rozhodnete tooconfigure funkce MPIO, provést následující kroky toomount hello, inicializace a formátujte svazky zařízení StorSimple na hostitelském Windows serveru.

[!INCLUDE [storsimple-mount-initialize-format-volume](../../includes/storsimple-mount-initialize-format-volume.md)]

## <a name="step-8-take-a-backup"></a>Krok 8: Provedení zálohy
Zálohy vytvořené v určitých časových bodech poskytují ochranu a zvyšují možnost jejich obnovení při současném zkrácení doby potřebné k obnovení. Zařízení StorSimple lze zálohovat dvěma různými způsoby: pomocí místních snímků nebo pomocí cloudových snímků. Každý z těchto typů zálohování může být **naplánovaný** nebo **ruční**.

Proveďte následující kroky v hello portálu toocreate Azure classic naplánované zálohování hello.

[!INCLUDE [storsimple-take-backup](../../includes/storsimple-take-backup.md)]

Ruční zálohování lze provést kdykoliv. Postupy, přejděte příliš[vytvoření ruční zálohy](#create-a-manual-backup).

## <a name="configure-a-new-storage-account-for-hello-service"></a>Konfigurace nového účtu úložiště pro službu hello
Toto je volitelný krok, je nutné tooperform, pouze pokud jste nepovolili automatické vytvoření účtu úložiště hello s službou. Účet úložiště Microsoft Azure je požadovaná toocreate kontejneru svazků zařízení StorSimple.

Pokud potřebujete toocreate účet úložiště Azure v jiné oblasti, přečtěte si [o účtech úložiště Azure](../storage/common/storage-create-storage-account.md) podrobné pokyny.

Proveďte následující kroky v hello portál Azure classic, na hello hello **služby StorSimple Manager** stránky.

[!INCLUDE [storsimple-configure-new-storage-account-u1](../../includes/storsimple-configure-new-storage-account-u1.md)]

## <a name="use-putty-tooconnect-toohello-device-serial-console"></a>Použít PuTTY tooconnect toohello konzole sériového portu zařízení
tooconnect tooWindows Powershellu pro StorSimple, budete potřebovat software pro emulaci terminálu toouse jako je například PuTTY. PuTTY můžete použít při přístupu hello zařízení přímo pomocí konzoly sériového portu hello nebo otevřením relace Telnetu ze vzdáleného počítače.

[!INCLUDE [Use PuTTY tooconnect toohello device serial console](../../includes/storsimple-use-putty.md)]

## <a name="scan-for-and-apply-updates"></a>Vyhledání a instalace aktualizací
Aktualizace zařízení může trvat několik hodin. Proveďte následující kroky tooscan pro hello a použití aktualizací na zařízení.
<!--can take 1-4 hours-->

<!--If you have a gateway configured on a network interface other than Data 0, you will need toodisable Data 2 and Data 3 network interfaces before installing hello update. Go too**Devices > Configure** and disable Data 2 and Data 3 interfaces. You should re-enable these interfaces after hello device is updated.-->

#### <a name="tooupdate-your-device"></a>tooupdate zařízení
1. Na zařízení hello **rychlý Start** klikněte na tlačítko **zařízení**. Vyberte fyzické zařízení hello, klikněte na **údržby** a pak klikněte na **kontrolovat aktualizace**.  
2. Vytvoří se úloha tooscan dostupné aktualizace. Pokud jsou k dispozici aktualizace, hello **kontrolovat aktualizace** změní příliš**instalovat aktualizace**. Klikněte na **Install Updates** (Instalovat aktualizace).
3. Vytvoří se úloha aktualizace. Monitorování stavu hello aktualizace přechodem příliš**úlohy**.
   
   > [!NOTE]
   > Když se úloha aktualizace hello spustí, okamžitě se zobrazuje stav hello jako 50 procent. Hello stav se změní procento too100 pouze po dokončení úlohy aktualizace hello. Neexistuje žádný stav v reálném čase pro proces aktualizace hello.
   > 
   > 
4. Po úspěšné aktualizaci hello zařízení povolte síťová rozhraní Data 2 a Data 3, pokud tyto byly zakázány.

<!-- In step 2, you may be requested toodisable Data 2 and Data 3 prior tooinstalling hello updates. You must disable these network interfaces or hello updates may fail.-->

## <a name="get-hello-iqn-of-a-windows-server-host"></a>Získání názvu IQN hostitele Windows Server hello
Proveďte následující kroky tooget hello iSCSI hello kvalifikovaný název (IQN) hostitele Windows se systémem Windows Server® 2012.

[!INCLUDE [Create a manual backup](../../includes/storsimple-get-iqn.md)]

## <a name="create-a-manual-backup"></a>Vytvoření ruční zálohy
Proveďte následující kroky v hello Azure classic portálu toocreate na vyžádání ruční zálohu jednoho svazku zařízení StorSimple hello.

[!INCLUDE [Create a manual backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="configure-mpio"></a>Konfigurace funkce MPIO
Funkce MPIO (Multipath I/O) je volitelná funkce a v systému Windows Server ve výchozím nastavení není nainstalována. Je nutné ji nainstalovat jako funkci pomocí Správce serveru. Pokyny k instalaci funkce MPIO najdete příliš[konfigurace funkce MPIO pro zařízení StorSimple](storsimple-configure-mpio-windows-server.md).

Pokyny k instalaci funkce MPIO pro zařízení StorSimple připojené tooa hostitele Linux najdete příliš[konfigurace funkce MPIO pro hostitele se systémem Linux](storsimple-configure-mpio-on-linux.md).

> [!NOTE]
> Funkce MPIO není podporována ve virtuálních zařízeních StorSimple.
> 
> 

## <a name="next-steps"></a>Další kroky
* Konfigurace [virtuálního zařízení](storsimple-virtual-device-u2.md)
* Použití hello [služby StorSimple Manager](storsimple-manager-service-administration.md) toomanage zařízení StorSimple.
