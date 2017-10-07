---
title: "virtuální zařízení aaaStorSimple Update 2 | Microsoft Docs"
description: "Zjistěte, jak toocreate, nasazovat a spravovat virtuální zařízení StorSimple ve službě Microsoft Azure virtual network. (Platí tooStorSimple Update 2)."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: f37752a5-cd0c-479b-bef2-ac2c724bcc37
ms.service: storsimple
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/07/2017
ms.author: alkohli
ms.openlocfilehash: 8d2a0520f1ed30ebec929c2bdabb4838691b8ad6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-a-storsimple-virtual-device-in-azure"></a>Nasazení a správa virtuálního zařízení StorSimple v Azure
## <a name="overview"></a>Přehled
virtuální zařízení řady StorSimple 8000 Hello je další schopností, která se dodává s vaším řešením Microsoft Azure StorSimple. virtuální zařízení StorSimple Hello běží ve virtuálním počítači ve virtuální síti Microsoft Azure a můžete ho tooback nahoru a klonování dat z hostitelů. Tento kurz popisuje, jak toodeploy a spravovat virtuální zařízení v Azure a použít tooall hello virtuální zařízení používající verzi softwaru Update 2 a nižší.

#### <a name="virtual-device-model-comparison"></a>Porovnání modelů virtuálního zařízení
Hello StorSimple virtuální zařízení je k dispozici ve dvou modelech standardní 8010 (dříve označované jako hello 1100) a prémiový 8020 (zaveden v Update 2). Porovnání dvou modelů hello v následující tabulce.

| Model zařízení | 8010<sup>1</sup> | 8020 |
| --- | --- | --- |
| **Maximální kapacita** |30 TB |64 TB |
| **Virtuální počítač Azure** |Standard_A3 (4 jádra, 7 GB paměti) |Standard_DS3 (4 jádra, 14 GB paměti) |
| **Kompatibilita verzí** |Verze používající software před Update 2 nebo novější |Verze používající software Update 2 nebo novější |
| **Dostupnost v oblastech** |Všechny oblasti Azure |Všechny oblasti Azure, které podporují službu Storage úrovně Premium a virtuální počítače Azure DS3<br></br> Použití [tento seznam](https://azure.microsoft.com/en-us/regions/services) toosee, pokud obě *virtuální počítače > DS-series* a *úložiště > Disk úložiště* jsou k dispozici ve vašem regionu. |
| **Typ úložiště** |Pro místní disky používá službu Azure Standard Storage<br></br> Zjistěte, jak příliš[vytvořit standardní účet úložiště](../storage/common/storage-create-storage-account.md) |Pro místní disky používá Azure Premium Storage.<sup>2</sup> <br></br>Zjistěte, jak příliš[vytvořit účet úložiště úrovně Premium](../storage/common/storage-premium-storage.md) |
| **Pokyny týkající se úloh** |Načítání souborů ze zálohy na úrovni položek |Scénáře vývoje a testování v cloudu, nízká latence, náročnější úlohy <br></br>Sekundární zařízení pro zotavení po havárii |

<sup>1</sup> *dříve označované jako hello 1100*.

<sup>2</sup> *obě hello 8010 a 8020 použít standardní úložiště Azure pro hello cloudu vrstvy. hello rozdíl existuje pouze v hello místní vrstvy v rámci zařízení hello*.

Tento článek popisuje hello podrobný postup nasazení virtuálního zařízení StorSimple v Azure. Po přečtení tohoto článku:

* Pochopte, jak se liší od fyzického zařízení hello hello virtuální zařízení.
* Být schopný toocreate a nakonfigurovat virtuální zařízení hello.
* Připojte toohello virtuální zařízení.
* Zjistěte, jak toowork s virtuálním zařízením hello.

V tomto kurzu platí tooall hello virtuální zařízení StorSimple verzi Update 2 a vyšší.

## <a name="how-hello-virtual-device-differs-from-hello-physical-device"></a>Jak se liší od fyzického zařízení hello hello virtuálního zařízení
virtuální zařízení StorSimple Hello je jen pro software verze zařízení storsimple, která běží na jednom uzlu v virtuální počítač Microsoft Azure. Hello virtuální zařízení podporuje scénáře zotavení po havárii ve kterých fyzické zařízení není k dispozici a je vhodné pro použití v načítání na úrovni položek ze zálohy, místní zotavení po havárii a cloudu vývojářů a testovací scénáře.

#### <a name="differences-from-hello-physical-device"></a>Rozdíl oproti hello fyzického zařízení
Hello následující tabulka uvádí některé hlavní rozdíly mezi hello virtuálního zařízení StorSimple a fyzickým zařízením StorSimple hello.

|  | Fyzické zařízení | Virtuální zařízení |
| --- | --- | --- |
| **Umístění** |Se nachází v datovém centru hello. |Běží v Azure. |
| **Síťová rozhraní** |Má šest síťových rozhraní: DATA 0 až DATA 5. |Má pouze jedno síťové rozhraní: DATA 0. |
| **Registrace** |Během kroku konfigurace hello zaregistrovány. |Registrace je samostatná úloha. |
| **Šifrovací klíč dat služby** |Obnovte na fyzickém zařízení hello a aktualizujte virtuální zařízení hello s hello nový klíč. |Nelze znovu vygenerovat z hello virtuální zařízení. |

## <a name="prerequisites-for-hello-virtual-device"></a>Předpoklady pro virtuální zařízení hello
Hello následující části popisují požadavky konfigurace hello virtuální zařízení StorSimple. Předchozí toodeploying virtuálního zařízení, přečtěte si hello [bezpečnostních aspektech použití virtuálního zařízení](storsimple-security.md#storsimple-virtual-device-security).

#### <a name="azure-requirements"></a>Požadavky na Azure
Než zřídíte virtuální zařízení hello, je třeba toomake hello následující přípravy v prostředí Azure:

* Pro virtuální zařízení hello [konfigurace virtuální sítě v Azure](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Používáte-li službu Premium Storage, musíte vytvořit virtuální síť v oblasti Azure, která podporuje službu Premium Storage. oblasti úložiště Premium Hello jsou oblasti, které odpovídají toohello řádek pro *Disk úložiště* v seznamu hello [služby Azure podle oblasti](https://azure.microsoft.com/en-us/regions/services).
* Je vhodné toouse hello výchozí server DNS poskytovaný platformou Azure místo zadávání vlastního názvu serveru DNS. Pokud není platný název serveru DNS nebo serveru DNS hello není možné tooresolve IP adresy správně, vytvoření hello hello virtuální zařízení se nezdaří.
* Připojení point-to-site a site-to-site jsou volitelná, ale nejsou vyžadována. Pokud chcete, můžete nastavit tyto možnosti pro pokročilejší scénáře.
* Můžete vytvořit [virtuální počítače Azure](../virtual-machines/virtual-machines-linux-about.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (hostitelské servery) ve virtuální síti hello, který můžete použít hello svazky vystavené virtuálním zařízením hello. Tyto servery musí splňovat následující požadavky hello:                             

  * Být virtuální počítače Windows nebo Linux s nainstalovaným softwarem iniciátoru iSCSI.
  * Běžet ve hello stejné virtuální síti jako virtuální zařízení hello.
  * Být schopný tooconnect toohello cíli iSCSI virtuálního zařízení hello prostřednictvím hello interní IP adresu hello virtuální zařízení.
* Zajistěte, aby jste nakonfigurovali podporu pro iSCSI a cloudové přenosy na hello stejné virtuální síti.

#### <a name="storsimple-requirements"></a>Požadavky na StorSimple
Ujistěte se, hello následující službu Azure StorSimple tooyour aktualizace před vytvořením virtuálního zařízení:

* Přidat [záznamy řízení přístupu](storsimple-manage-acrs.md) pro hello virtuálních počítačů, které budou servery toobe hostitele pro virtuální zařízení.
* Použití [účet úložiště](storsimple-manage-storage-accounts.md#add-a-storage-account) v hello stejné oblasti jako virtuální zařízení hello. Účty úložiště v jiných oblastech mohou vést k nižšímu výkonu. Účet Standard nebo Premium Storage můžete použít s virtuálním zařízením hello. Další informace o toocreate [standardní účet úložiště](../storage/common/storage-create-storage-account.md) nebo [účtu Storage úrovně Premium](../storage/common/storage-premium-storage.md)
* Použijte jiný účet úložiště pro vytvoření virtuálního zařízení z hello, jedna pro vaše data. Pomocí hello stejný účet úložiště může vést k nižšímu výkonu.

Ujistěte se, že máte hello následující informace, než můžete začít:

* Účet portálu Azure Classic s přihlašovacími údaji.
* Kopie šifrovacího klíče dat hello služby z fyzického zařízení.

## <a name="create-and-configure-hello-virtual-device"></a>Vytvořit a nakonfigurovat virtuální zařízení hello
Před provedením těchto postupů se ujistěte, že jste splnili hello [požadavky pro virtuální zařízení hello](#prerequisites-for-the-virtual-device).

Po vytvoření virtuální sítě, nakonfigurované služby StorSimple Manager a registraci fyzického zařízení StorSimple službou hello, můžete použít následující kroky toocreate hello a nakonfigurovat virtuální zařízení StorSimple.

### <a name="step-1-create-a-virtual-device"></a>Krok 1: Vytvoření virtuálního zařízení
Proveďte následující kroky toocreate hello StorSimple virtuální zařízení hello.

[!INCLUDE [Create a virtual device](../../includes/storsimple-create-virtual-device-u2.md)]

Pokud hello vytvoření virtuálního zařízení hello selže v tomto kroku, nemusí mít toohello připojení k Internetu. Další informace, přejděte příliš[vyřešit chyby připojení k Internetu](#troubleshoot-internet-connectivity-errors) při vytváření virtuálního zařízení.

### <a name="step-2-configure-and-register-hello-virtual-device"></a>Krok 2: Konfigurace a registrace virtuálního zařízení hello
Před zahájením tohoto postupu se ujistěte, že máte kopii šifrovacího klíče dat služby hello. Hello šifrovacího klíče dat služby byl vytvořen při konfiguraci prvního zařízení StorSimple a byly pokyn toosave jej na bezpečném místě. Pokud nemáte kopii šifrovacího klíče dat služby hello, musí Microsoft Support požádejte o pomoc.

Proveďte následující kroky tooconfigure hello a registraci virtuálního zařízení StorSimple.

[!INCLUDE [Configure and register a virtual device](../../includes/storsimple-configure-register-virtual-device.md)]

### <a name="step-3-optional-modify-hello-device-configuration-settings"></a>Krok 3: (Volitelné) upravit hello nastavení konfigurace pro zařízení
Hello následující část popisuje nastavení konfigurace zařízení hello potřebné pro virtuální zařízení StorSimple hello, pokud chcete toouse CHAP, Snapshot Manager zařízení StorSimple nebo změnit heslo správce zařízení hello.

#### <a name="configure-hello-chap-initiator"></a>Konfigurace iniciátoru CHAP hello
Tento parametr obsahuje přihlašovací údaje hello, které virtuální zařízení (cíl) očekává z iniciátorů hello (serverů), které se pokoušíte tooaccess hello svazky. iniciátory Hello poskytne CHAP uživatelské jméno a heslo tooidentify CHAP sami tooyour zařízení během tohoto ověřování. Podrobný postup je uveden příliš[konfigurace CHAP pro vaše zařízení](storsimple-configure-chap.md#unidirectional-or-one-way-authentication).

#### <a name="configure-hello-chap-target"></a>Konfigurace cíle CHAP hello
Tento parametr obsahuje přihlašovací údaje hello, které virtuální zařízení používá, když iniciátor podporou CHAP žádá vzájemné nebo obousměrné ověřování. Virtuální zařízení bude používat uživatelské jméno Reverse CHAP a Reverse CHAP heslo tooidentify samotné toohello iniciátor během tohoto procesu ověřování. Upozorňujeme, že nastavení cíle CHAP jsou globální nastavení. Při jejich použití, budou všechny hello svazky připojené toohello virtuální zařízení úložiště používat ověřování CHAP. Podrobný postup je uveden příliš[konfigurace CHAP pro vaše zařízení](storsimple-configure-chap.md#bidirectional-or-mutual-authentication).

#### <a name="configure-hello-storsimple-snapshot-manager-password"></a>Konfigurace hesla Snapshot Manageru zařízení StorSimple hello
Software Snapshot Manager zařízení StorSimple se nachází na hostiteli s Windows a umožňuje správci toomanage zálohy zařízení StorSimple ve formě hello místních a cloudových snímků.

> [!NOTE]
> Hostiteli s Windows hello virtuální zařízení, je virtuální počítač Azure.
>
>

Při konfiguraci zařízení v hello Snapshot Manager zařízení StorSimple, zobrazí se výzva tooprovide hello tooauthenticate StorSimple zařízení IP adresu a heslo zařízení úložiště. Podrobný postup je uveden příliš[konfigurace zařízení StorSimple Snapshot Manager heslo](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password).

#### <a name="change-hello-device-administrator-password"></a>Heslo správce zařízení hello změn
Při použití hello prostředí Windows PowerShell rozhraní tooaccess hello virtuální zařízení, budou požadované tooenter hesla správce zařízení. Hello zabezpečení vašich dat, se vyžaduje toochange toto heslo před hello virtuální zařízení se může používat. Podrobný postup je uveden příliš[konfigurace hesla správce zařízení](storsimple-change-passwords.md#change-the-device-administrator-password).

## <a name="connect-remotely-toohello-virtual-device"></a>Vzdálené připojení toohello virtuálního zařízení
Ve výchozím nastavení není povoleno vzdáleného přístupu tooyour virtuálního zařízení prostřednictvím rozhraní Windows PowerShell hello. Nejprve nutné tooenable vzdálené správy na hello virtuální zařízení a pak ji povolit na hello klienta, které budou použité tooaccess virtuální zařízení.

Hello dvoustupňový proces tooconnect vzdáleně je podrobně popsán níže.

### <a name="step-1-configure-remote-management"></a>Krok 1: Konfigurace vzdálené správy
Proveďte následující kroky tooconfigure vzdálené správy pro virtuální zařízení StorSimple hello.

[!INCLUDE [Configure remote management via HTTP for virtual device](../../includes/storsimple-configure-remote-management-http-device.md)]

### <a name="step-2-remotely-access-hello-virtual-device"></a>Krok 2: Vzdálený přístup hello virtuálního zařízení
Po povolení vzdálené správy na stránce konfigurace zařízení StorSimple hello, můžete použít prostředí Windows PowerShell vzdálenou komunikaci tooconnect toohello virtuální zařízení z jiného virtuálního počítače ve hello stejné virtuální síti; Například můžete připojit z hello hostitele virtuálních počítačů, že jste nakonfigurovali a používali tooconnect iSCSI. Ve většině nasazení bude máte už otevřený veřejný koncový bod tooaccess váš hostitel virtuálního počítače, který můžete použít pro přístup k virtuálnímu zařízení hello.

> [!WARNING]
> **Pro zvýšení zabezpečení důrazně doporučujeme používat protokol HTTPS, pokud se připojujete toohello koncových bodů a pak odstraňte koncové body hello po dokončení vzdálené relace prostředí PowerShell.**
>
>

Postupujte podle pokynů hello v [vzdáleném připojení zařízení StorSimple tooyour](storsimple-remote-connect.md) tooset až vzdálené komunikace pro vaše virtuální zařízení.

## <a name="connect-directly-toohello-virtual-device"></a>Připojte se přímo toohello virtuálního zařízení
Můžete také připojit přímo toohello virtuální zařízení. Pokud chcete tooconnect přímo toohello virtuální zařízení z jiného počítače mimo hello virtuální sítě nebo mimo prostředí Microsoft Azure hello, budete potřebovat další koncové body toocreate jak je popsáno v hello následující postup.

Proveďte následující kroky toocreate veřejný koncový bod na virtuální zařízení hello hello.

[!INCLUDE [Create public endpoints on a virtual device](../../includes/storsimple-create-public-endpoints-virtual-device.md)]

Doporučujeme vám, zda je připojit z jiného virtuálního počítače ve hello stejné virtuální sítě, protože tento postup minimalizuje hello počet veřejných koncových bodů ve virtuální síti. Pokud použijete tuto metodu, jednoduše připojíte toohello virtuálního počítače prostřednictvím relace vzdálené plochy a pak konfiguraci tohoto virtuálního počítače pro použití stejně jako u jiných klientů systému Windows v místní síti. Protože hello port již bude znám nepotřebujete tooappend hello veřejný port číslo.

## <a name="work-with-hello-storsimple-virtual-device"></a>Práce s virtuální zařízení StorSimple hello
Teď, když je vytvořen a nakonfigurován hello virtuálního zařízení StorSimple, jste připravené toostart práci s ním. Můžete pracovat kontejnery svazků, svazky a zásadami zálohování na virtuální zařízení stejně jako na fyzickém zařízení StorSimple; Hello jediným rozdílem je, že je potřeba toomake se, že jste vybrali hello virtuální zařízení ze seznamu zařízení. Odkazovat příliš[použít toomanage služby StorSimple Manager hello virtuálního zařízení](storsimple-manager-service-administration.md) podrobné postupy různých správy úloh pro virtuální zařízení hello hello.

Hello následující části popisují některé rozdíly hello, které můžete narazit při práci s virtuálním zařízením hello.

### <a name="maintain-a-storsimple-virtual-device"></a>Údržba virtuálního zařízení StorSimple
Protože jde o softwarové zařízení, údržby pro virtuální zařízení hello je minimální při porovnání toomaintenance pro hello fyzického zařízení. Máte hello následující možnosti:

* **Aktualizace softwaru** – můžete zobrazit hello datum poslední aktualizace softwaru hello společně s žádné aktualizace stavové zprávy. Můžete použít hello **kontrolovat aktualizace** tlačítko dole hello tooperform stránku hello ruční kontrolu, pokud chcete toocheck nové aktualizace. Pokud jsou k dispozici aktualizace, klikněte na tlačítko **instalovat aktualizace** tooinstall. Protože se nachází na virtuálním zařízení hello pouze jedno rozhraní, to znamená, že bude k mírnému přerušení služby při použití aktualizací. virtuální zařízení Hello bude vypnutí a restartování (v případě potřeby) tooapply všechny aktualizace, které byly vydány. Podrobný postup najdete příliš[aktualizace zařízení](storsimple-update-device.md#install-regular-updates-via-the-azure-classic-portal).
* **Balíček pro podporu** – můžete vytvořit a nahrávání toohelp balíčku na podporu společnosti Microsoft Support řešení problémů s virtuálním zařízením. Podrobný postup najdete příliš[vytvoření a Správa balíčku pro podporu](storsimple-create-manage-support-package.md).

### <a name="storage-accounts-for-a-virtual-device"></a>Účty služby Storage pro virtuální zařízení
Účty služby Storage jsou vytvořeny pro použití službou StorSimple Manager hello, hello virtuální zařízení a hello fyzického zařízení. Při vytváření účtů úložiště, doporučujeme použít oblast identifikátor v hello popisný název toohelp zkontrolujte danou oblast hello je konzistentní v rámci všech součástí systému hello. Pro virtuální zařízení je důležité, že všechny hello součásti se v hello stejné problémy s výkonem tooprevent oblast.

Podrobný postup najdete příliš[přidání účtu úložiště](storsimple-manage-storage-accounts.md#add-a-storage-account).

### <a name="deactivate-a-storsimple-virtual-device"></a>Deaktivace virtuálního zařízení StorSimple
Deaktivace virtuálního zařízení odstraní hello virtuálních počítačů a hello prostředky vytvořené při jeho zřizování. Po deaktivaci virtuálního zařízení hello je nelze obnovit tooits předchozího stavu. Před deaktivací hello virtuální zařízení, ujistěte se, že toostop nebo odstranit klienty a hostitele, které na ní závisí.

Deaktivace virtuálního zařízení má za následek hello následující akce:

* Hello virtuální zařízení je odebráno.
* disk Hello operačního systému a datové disky, které jsou vytvořené pro virtuální zařízení hello se odeberou.
* Hello hostované služby a virtuální sítě vytvořené při zřizování zůstanou zachovány. Pokud je nepoužíváte, je nutné je odstranit ručně.
* Cloudové snímky vytvořené pro hello virtuální zařízení zůstanou zachovány.

Podrobný postup najdete příliš[deaktivace a odstranění zařízení StorSimple](storsimple-deactivate-and-delete-device.md).

Jakmile hello virtuální zařízení zobrazeno jako deaktivované na stránce služby StorSimple Manager hello, hello virtuálního zařízení můžete odstranit ze seznamu zařízení na hello **zařízení** stránky.

### <a name="start-stop-and-restart-a-virtual-device"></a>Spuštění, zastavení a restartování virtuálního zařízení
Na rozdíl od fyzického zařízení StorSimple hello neexistuje žádné napájení nebo vypnutí tlačítko toopush na virtuální zařízení StorSimple. Mohou však nastat situace, kde potřebujete toostop a restartování virtuálního zařízení hello. Některé aktualizace může například vyžadovat, že tento hello virtuálních počítačů se restartování procesu aktualizace toofinish hello. Hello nejjednodušší způsob, jak můžete toostart, zastavení a restartování virtuálního zařízení je toouse hello konzoly pro správu virtuálních počítačů.

Když se podíváte na hello konzoly pro správu, hello virtuálního zařízení uveden stav **systémem** vzhledem k tomu, že je spuštěna ve výchozím nastavení po jeho vytvoření. Virtuální počítač můžete kdykoli spustit, zastavit nebo restartovat.

[!INCLUDE [Stop and restart virtual device](../../includes/storsimple-stop-restart-virtual-device.md)]

### <a name="reset-toofactory-defaults"></a>Obnovit výchozí hodnoty toofactory
Pokud se rozhodnete, že chcete toostart přes s virtuálním zařízením, jednoduše deaktivovat a odstranit a pak vytvořte novou. Stejně jako při obnovení fyzického zařízení nebude mít nové virtuální zařízení nainstalovány; žádné aktualizace proto zkontrolujte, zda toocheck aktualizací před jeho použitím.

## <a name="fail-over-toohello-virtual-device"></a>Selhání toohello virtuálního zařízení
Zotavení po havárii (DR) je jedním z hello klíčových scénářů, které hello virtuální zařízení je navržené pro zařízení StorSimple. V tomto scénáři hello fyzického zařízení StorSimple nebo celého datového centra nemusí být k dispozici. Naštěstí můžete použít virtuální zařízení toorestore operací v alternativním umístění. Hello kontejnery svazků ze hello zdrojového zařízení během zotavení po Havárii, změnit vlastnictví a jsou přenášená toohello virtuální zařízení. Hello požadavky pro zotavení po Havárii je, že hello virtuální zařízení bylo vytvořeno a nakonfigurováno, všechny svazky hello v rámci kontejneru svazků hello být offline, a hello kontejner svazků má přiřazený cloudový snímek.

> [!NOTE]
> * Při použití virtuálního zařízení jako hello sekundární zařízení pro zotavení po Havárii, mějte na paměti, že hello 8010 má úložiště Standard Storage 30 TB a zařízení 8020 úložiště Premium Storage 64 TB. Hello vyšší kapacity 8020 virtuální zařízení může být vhodnější pro scénář zotavení po havárii.
> * Nelze převzetí služeb při selhání nebo klonování z zařízení se systémem aktualizovat 2 zařízení tooa 1 softwarem před aktualizací. Můžete však převzít zařízení se systémem Update 2 tooa zařízení se softwarem Update 1 (1.1 nebo 1.2)
>
>

Podrobný postup najdete příliš[převzetí služeb při selhání tooa virtuální zařízení](storsimple-device-failover-disaster-recovery.md#fail-over-to-a-storsimple-virtual-device).

## <a name="shut-down-or-delete-hello-virtual-device"></a>Vypnutí nebo odstranění virtuálního zařízení hello
Pokud jste dříve nakonfigurovali a používali StorSimple virtuální zařízení, ale nyní chcete toostop nabíhání poplatků za jeho použití, můžete vypnout hello virtuální zařízení. Vypínání hello virtuálního zařízení neodstraní jeho operační systém nebo datové disky v úložišti. Zastaví nabíhání poplatků za vaše předplatné, ale poplatky za úložiště pro hello operačního systému a datové disky bude pokračovat.

Pokud odstraníte nebo vypnout hello virtuální zařízení, se zobrazí jako **Offline** na stránce zařízení hello hello služby StorSimple Manager. Můžete vybrat toodeactivate nebo odstranit hello zařízení, pokud také chcete toodelete hello zálohy vytvořené virtuálním zařízením hello. Další informace naleznete v tématu [Deaktivace a odstranění zařízení StorSimple](storsimple-deactivate-and-delete-device.md).

[!INCLUDE [Shut down a virtual device](../../includes/storsimple-shutdown-virtual-device.md)]

[!INCLUDE [Delete a virtual device](../../includes/storsimple-delete-virtual-device.md)]

## <a name="troubleshoot-internet-connectivity-errors"></a>Řešení potíží s připojením k internetu
Během vytváření hello virtuálního zařízení Pokud neexistuje žádné toohello připojení k Internetu, hello vytvoření selže. tootroubleshoot, pokud je hello selhání kvůli připojení k Internetu, proveďte hello proveďte kroky v hello portál Azure classic:

1. Vytvořte v Azure virtuální počítač s Windows Serverem 2012. Tento virtuální počítač by měl použít hello stejný účet úložiště, virtuální síť a podsíť, protože používá virtuální zařízení. Pokud už máte existující hostitele Windows serveru v Azure pomocí hello stejný účet úložiště, virtuální síť a podsíť, můžete ji použít i připojení k Internetu tootroubleshoot hello.
2. Vzdálené přihlášení do hello virtuální počítač vytvořený v předchozím kroku hello.
3. Otevřete okno příkazového řádku uvnitř hello virtuálního počítače (Win + R a pak zadejte `cmd`).
4. Spusťte následující cmd příkazového řádku hello hello.

    `nslookup windows.net`
5. Pokud `nslookup` nezdaří, pak selhání připojení k Internetu brání virtuálního zařízení hello registraci toohello služby StorSimple Manager.
6. Proveďte změny hello požadované tooensure tooyour virtuální sítě, která hello virtuální zařízení je možné tooaccess Azure webů, jako je "windows.net".

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[použít toomanage služby StorSimple Manager hello virtuálního zařízení](storsimple-manager-service-administration.md).
* Pochopit, jak příliš[obnovit svazek StorSimple ze zálohovacího skladu](storsimple-restore-from-backup-set.md).
