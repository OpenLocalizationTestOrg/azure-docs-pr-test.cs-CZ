---
title: "zařízení řady StorSimple 8000 aaaDeploy Government portálu | Microsoft Docs"
description: "Popisuje hello kroky a osvědčené postupy pro nasazení hello zařízení řady StorSimple 8000 systémem Update 3 a později a hello služby v hello portál Azure Government."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/22/2017
ms.author: alkohli
ms.openlocfilehash: ea643cd59dcdf17482268d14c1348a3b5fb098b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-on-premises-storsimple-device-in-hello-government-portal"></a>Nasazení místního zařízení StorSimple hello Government portálu

## <a name="overview"></a>Přehled
Vítejte tooMicrosoft nasazení zařízení StorSimple v Azure. Tyto kurzy nasazení se vztahují toohello StorSimple řady 8000 spouštění 3 aktualizace softwaru nebo později v hello portál Azure Government. Tato série kurzů zahrnuje kontrolní seznam konfigurace, seznam požadavky konfigurace a podrobné konfigurační kroky pro zařízení StorSimple.

Hello informace v těchto kurzech předpokládá, že kontrole hello bezpečnostní opatření a vybaleno, namontovali do racku a zapojena jeho zařízení StorSimple. Pokud stále potřebujete tooperform ty úlohy, začněte prostudováním hello [bezpečnostní opatření](storsimple-safety.md). Postupujte podle pokynů pro konkrétní zařízení hello toounpack, namontujte do racku a zapojení kabeláže zařízení.

* [Zařízení 8100 – Vybalení, namontování do racku a zapojení kabeláže](storsimple-8100-hardware-installation.md)
* [Zařízení 8600 – Vybalení, namontování do racku a zapojení kabeláže](storsimple-8600-hardware-installation.md)

Budete potřebovat správce oprávnění toocomplete hello procesu instalace a konfigurace. Doporučujeme, abyste si prošli kontrolní seznam konfigurace hello před zahájením. proces nasazení a konfigurace Hello může trvat některé toocomplete čas.

> [!NOTE]
> informace o nasazení Hello StorSimple publikované na webu Microsoft Azure hello platí tooStorSimple 8000 řady jenom zařízení. Úplné informace o řadě zařízení hello 7000, přejděte na: [http://onlinehelp.storsimple.com/](http://onlinehelp.storsimple.com). Informace o nasazení řady 7000 najdete v části hello [StorSimple úvodní příručce k systému](http://onlinehelp.storsimple.com/111_Appliance/).


## <a name="deployment-steps"></a>Kroky nasazení
Proveďte tyto kroky požadované tooconfigure zařízení StorSimple a připojte ho služba tooyour Manager zařízení StorSimple. Kromě toho toohello potřebné kroky, jsou volitelné kroky a postupy, může být vhodné toocomplete při nasazení hello. Hello podrobný postup nasazení pokyny o tom, kdy byste měli dělat každý z těchto kroků volitelné.

| Krok | Popis |
| --- | --- |
| **POŽADAVKY** |Tyto potřebovat toobe splnit v rámci přípravy nasazení hello. |
| [Kontrolní seznam konfigurace nasazení](#deployment-configuration-checklist) |Tento kontrolní seznam toogather a zaznamenávat informace předchozí tooand použijte během nasazení hello. |
| [Požadavky nasazení](#deployment-prerequisites) |Tyto ověřte, že hello prostředí je připravený pro nasazení. |
|  | |
| **PODROBNÝ POSTUP NASAZENÍ** |Tyto kroky jsou požadované toodeploy zařízení StorSimple v produkčním prostředí. |
| [Krok 1: Vytvoření nové služby](#step-1-create-a-new-service) |Nastavte správu cloudu a úložiště pro zařízení StorSimple. *Pokud máte existující službu pro jiná zařízení StorSimple, tento krok přeskočte*. |
| [Krok 2: Získání registračního klíče služby hello](#step-2-get-the-service-registration-key) |Pomocí tohoto klíče tooregister a připojení zařízení StorSimple se službou správy hello. |
| [Krok 3: Konfigurace a registrace zařízení hello pomocí Windows Powershellu pro StorSimple](#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple) |Připojit síť tooyour hello zařízení a zaregistrovat ji pomocí Azure toocomplete hello instalaci pomocí služby správy hello. |
| [Krok 4: Dokončení hello minimální instalace zařízení](#step-4-complete-minimum-device-setup) </br>Volitelné: Aktualizace zařízení StorSimple |Použijte hello management service toocomplete hello nastavení zařízení a povolit tooprovide úložiště. |
| [Krok 5: Vytvoření kontejneru svazků](#step-5-create-a-volume-container) |Vytvoření kontejneru svazků tooprovision. Kontejner svazků obsahuje účet úložiště, šířku pásma a nastavení šifrování pro všechny svazky hello v ní obsažené. |
| [Krok 6: Vytvoření svazku](#step-6-create-a-volume) |Zřiďte svazky úložiště v zařízení StorSimple hello vašich serverů. |
| [Krok 7: Připojení, inicializace a formátování svazků](#step-7-mount-initialize-and-format-a-volume) </br>Volitelné: Konfigurace funkce MPIO |Připojte vaše servery úložiště iSCSI toohello poskytované hello zařízení. Volitelně nakonfigurujte tooensure funkce MPIO, že servery budou tolerovat připojení, sítě a rozhraní selhání. |
| [Krok 8: Provedení zálohy](#step-8-take-a-backup) |Nastavit tooprotect vaše zásady zálohování dat |
|  | |
| **DALŠÍ POSTUPY** |Při nasazení řešení může být nutné toorefer toothese postupy. |
| [Konfigurace nového účtu úložiště pro službu hello](#configure-a-new-storage-account-for-the-service) | |
| [Použít PuTTY tooconnect toohello konzole sériového portu zařízení](#use-putty-to-connect-to-the-device-serial-console) | |
| [Vyhledání a instalace aktualizací](#scan-for-and-apply-updates) | |
| [Získání názvu IQN hostitele Windows Server hello](#get-the-iqn-of-a-windows-server-host) | |
| [Vytvoření ruční zálohy](#create-a-manual-backup) | |


## <a name="deployment-configuration-checklist"></a>Kontrolní seznam konfigurace nasazení
Před nasazením zařízení StorSimple, budete potřebovat toocollect informace tooconfigure hello softwaru na vašem zařízení. Předběžná příprava některých z těchto informací předem, bude vám pomůžou zjednodušit hello proces nasazení zařízení StorSimple hello ve vašem prostředí. Stáhnout a použít tento kontrolní seznam toonote hello konfigurace podrobnosti během nasazování zařízení.

[Stáhnout kontrolní seznam konfigurace nasazení zařízení StorSimple](http://www.microsoft.com/download/details.aspx?id=49159)

## <a name="deployment-prerequisites"></a>Požadavky nasazení
Hello následující části popisují požadavky konfigurace hello služby StorSimple Manager zařízení a zařízení StorSimple.

### <a name="for-hello-storsimple-device-manager-service"></a>Pro hello služby StorSimple Manager zařízení
Než začnete, ujistěte se, že:

* Máte účet Microsoft a přihlašovací údaje účtu.
* Máte účet služby Microsoft Azure Storage a přihlašovací údaje účtu.
* Vaše předplatné Microsoft Azure je povoleno pro hello služby StorSimple Manager zařízení. Vaše předplatné by mělo být zakoupeno prostřednictvím hello [smlouva Enterprise Agreement](https://azure.microsoft.com/pricing/enterprise-agreement/).
* Máte přístup k softwaru pro emulaci tooterminal jako je například PuTTY.

### <a name="for-hello-device-in-hello-datacenter"></a>Pro zařízení hello v datovém centru hello
Před konfigurací zařízení hello, ujistěte se, že:

* Zařízení je zcela vybaleno, namontováno v racku a je kompletně zapojena jeho kabeláž napájení, sítě a sériového přístupu, jak je popsáno v následujících článcích:
  
  * [Zařízení 8100 – Vybalení, namontování do racku a zapojení kabeláže](storsimple-8100-hardware-installation.md)
  * [Zařízení 8600 – Vybalení, namontování do racku a zapojení kabeláže](storsimple-8600-hardware-installation.md)

### <a name="for-hello-network-in-hello-datacenter"></a>Pro síť hello v datovém centru hello
Než začnete, ujistěte se, že:

* Hello porty v bráně firewall datového centra jsou otevřené tooallow pro přenosy iSCSI a cloudu jak je popsáno v [požadavcích sítě pro zařízení StorSimple](storsimple-8000-system-requirements.md#networking-requirements-for-your-storsimple-device).

## <a name="step-by-step-deployment"></a>Podrobný postup nasazení
Použijte následující podrobné pokyny toodeploy hello zařízení StorSimple v datovém centru hello.

## <a name="step-1-create-a-new-service"></a>Krok 1: Vytvoření nové služby
Služba Správce zařízení StorSimple může spravovat více zařízení StorSimple. Proveďte následující kroky toocreate novou instanci třídy hello služby StorSimple Manager zařízení hello.

[!INCLUDE [storsimple-8000-create-new-service-gov](../../includes/storsimple-8000-create-new-service-gov.md)]

> [!IMPORTANT]
> Pokud jste nepovolili automatické vytvoření účtu úložiště hello s službou, budete potřebovat toocreate alespoň jeden účet úložiště po úspěšném vytvoření služby. Tento účet úložiště se použije při vytváření kontejneru svazku.
> 
> * Pokud nebyl vytvořen účet úložiště automaticky, přejděte příliš[konfigurace nového účtu úložiště pro službu hello](#configure-a-new-storage-account-for-the-service) podrobné pokyny.
> * Pokud jste povolili hello automatické vytvoření účtu úložiště, pokračujte příliš[krok 2: registrační klíč služby hello Get](#step-2-get-the-service-registration-key).


## <a name="step-2-get-hello-service-registration-key"></a>Krok 2: Získání registračního klíče služby hello
Po hello služby StorSimple Manager zařízení je spuštěný a funkční, budete potřebovat registrační klíč služby hello tooget. Tento klíč je použité tooregister a připojení služby toohello zařízení StorSimple.

Provedení následujících kroků na portálu Government hello hello.

[!INCLUDE [storsimple-8000-get-service-registration-key](../../includes/storsimple-8000-get-service-registration-key.md)]

## <a name="step-3-configure-and-register-hello-device-through-windows-powershell-for-storsimple"></a>Krok 3: Konfigurace a registrace zařízení hello pomocí Windows Powershellu pro StorSimple
Pomocí prostředí Windows PowerShell pro StorSimple toocomplete hello počátečním nastavení zařízení StorSimple, jak je popsáno v hello následující postup. Toocomplete software pro emulaci terminálu toouse bude nutné tento krok. Další informace najdete v tématu [konzoly sériového portu toohello zařízení tooconnect pro použití klienta PuTTY](#use-putty-to-connect-to-the-device-serial-console).

[!INCLUDE [storsimple-8000-configure-and-register-device-gov](../../includes/storsimple-8000-configure-and-register-device-gov-u2.md)]

## <a name="step-4-complete-minimum-device-setup"></a>Krok 4: Dokončení minimální instalace zařízení
Pro hello minimální konfigurace zařízení zařízení StorSimple je potřeba:

* Zadejte popisný název zařízení.
* Nastavit časové pásmo hello zařízení.
* Přiřadíte pevné IP adresy řadiče tooboth hello.

Proveďte následující kroky v hello Azure Government portálu toocomplete hello minimální instalace zařízení hello.

[!INCLUDE [storsimple-8000-complete-minimum-device-setup-u2](../../includes/storsimple-8000-complete-minimum-device-setup-u2.md)]

## <a name="step-5-create-a-volume-container"></a>Krok 5: Vytvoření kontejneru svazků
Kontejner svazků obsahuje účet úložiště, šířku pásma a nastavení šifrování pro všechny svazky hello v ní obsažené. Před zahájením zřizování svazků v zařízení StorSimple budete potřebovat toocreate kontejner svazků.

Proveďte následující kroky v hello Government portálu toocreate kontejner svazků hello.

[!INCLUDE [storsimple-8000-create-volume-container](../../includes/storsimple-8000-create-volume-container.md)]

## <a name="step-6-create-a-volume"></a>Krok 6: Vytvoření svazku
Po vytvoření kontejneru svazků můžete zřídit svazek úložiště na zařízení StorSimple hello pro své servery. Proveďte hello proveďte kroky v hello Government portálu toocreate svazku.

> [!IMPORTANT]
> Služba StorSimple Manager zařízení může vytvářet pouze dynamicky zřizované svazky.  Nelze však vytvářet částečně zřizované svazky.

[!INCLUDE [storsimple-8000-create-volume](../../includes/storsimple-8000-create-volume-u2.md)]

## <a name="step-7-mount-initialize-and-format-a-volume"></a>Krok 7: Připojení, inicializace a formátování svazků
Proveďte tyto kroky na hostiteli s Windows Server.

> [!IMPORTANT]
> * Hello vysoké dostupnosti vašeho řešení StorSimple doporučujeme konfigurace funkce MPIO na vaše iSCSI předchozí tooconfiguring servery (volitelné) hostitele. Konfigurace funkce MPIO na hostitelských serverech zajistí, že hello servery budou tolerovat odkaz, sítě nebo selhání rozhraní.
> * Funkce MPIO a standardu iSCSI instalace a konfigurace pokyny na hostitelském Windows serveru najdete příliš[konfigurace funkce MPIO pro zařízení StorSimple](storsimple-configure-mpio-windows-server.md). Tyto také bude zahrnovat hello kroky toomount, inicializace a formátování svazků zařízení StorSimple.
> * Funkce MPIO a standardu iSCSI instalace a konfigurace pokyny na hostitele platformy Linux najdete příliš[konfigurace funkce MPIO pro hostitele se systémem StorSimple Linux](storsimple-configure-mpio-on-linux.md)

Pokud se rozhodnete tooconfigure funkce MPIO, provést následující kroky toomount hello, inicializace a formátujte svazky zařízení StorSimple na hostitelském Windows serveru.

[!INCLUDE [storsimple-mount-initialize-format-volume](../../includes/storsimple-mount-initialize-format-volume.md)]

## <a name="step-8-take-a-backup"></a>Krok 8: Provedení zálohy
Zálohy vytvořené v určitých časových bodech poskytují ochranu a zvyšují možnost jejich obnovení při současném zkrácení doby potřebné k obnovení. Zařízení StorSimple lze zálohovat dvěma různými způsoby: pomocí místních snímků nebo pomocí cloudových snímků. Každý z těchto typů zálohování může být **naplánovaný** nebo **ruční**.

Proveďte hello proveďte kroky v hello Government portálu toocreate plánované zálohování.

[!INCLUDE [storsimple-8000-take-backup](../../includes/storsimple-8000-take-backup.md)]

Ruční zálohování lze provést kdykoliv. Postupy, přejděte příliš[vytvoření ruční zálohy](#create-a-manual-backup).

## <a name="configure-a-new-storage-account-for-hello-service"></a>Konfigurace nového účtu úložiště pro službu hello
Toto je volitelný krok, je nutné tooperform, pouze pokud jste nepovolili automatické vytvoření účtu úložiště hello s službou. Účet úložiště Microsoft Azure je požadovaná toocreate kontejneru svazků zařízení StorSimple.

Pokud potřebujete toocreate účet úložiště Azure v jiné oblasti, přečtěte si [o účtech úložiště Azure](../storage/common/storage-create-storage-account.md) podrobné pokyny.

Proveďte následující kroky Government portálu hello na hello hello **služby StorSimple Manager zařízení** stránky.

[!INCLUDE [storsimple-configure-new-storage-account-u1](../../includes/storsimple-8000-configure-new-storage-account-u2.md)]

## <a name="use-putty-tooconnect-toohello-device-serial-console"></a>Použít PuTTY tooconnect toohello konzole sériového portu zařízení
tooconnect tooWindows Powershellu pro StorSimple, budete potřebovat software pro emulaci terminálu toouse jako je například PuTTY. PuTTY můžete použít při přístupu hello zařízení přímo pomocí konzoly sériového portu hello nebo otevřením relace Telnetu ze vzdáleného počítače.

[!INCLUDE [Use PuTTY tooconnect toohello device serial console](../../includes/storsimple-use-putty.md)]

## <a name="scan-for-and-apply-updates"></a>Vyhledání a instalace aktualizací
Aktualizace zařízení může trvat několik hodin. Podrobné kroky na tom, jak tooinstall hello nejnovější aktualizace naleznete příliš[nainstalovat aktualizaci 4](storsimple-8000-install-update-4.md).

## <a name="get-hello-iqn-of-a-windows-server-host"></a>Získání názvu IQN hostitele Windows Server hello
Proveďte následující kroky tooget hello iSCSI hello kvalifikovaný název (IQN) hostitele Windows se systémem Windows Server® 2012.

[!INCLUDE [Get IQN of your Windows Server host](../../includes/storsimple-get-iqn.md)]

## <a name="create-a-manual-backup"></a>Vytvoření ruční zálohy
Proveďte následující kroky v portálu toocreate Government hello na vyžádání ruční zálohu jednoho svazku zařízení StorSimple hello.

[!INCLUDE [Create a manual backup](../../includes/storsimple-8000-create-manual-backup.md)]

## <a name="next-steps"></a>Další kroky
* Konfigurace [virtuálního zařízení](storsimple-8000-cloud-appliance-u2.md)
* Použití hello [služby StorSimple Manager zařízení](storsimple-8000-manager-service-administration.md) toomanage zařízení StorSimple.

