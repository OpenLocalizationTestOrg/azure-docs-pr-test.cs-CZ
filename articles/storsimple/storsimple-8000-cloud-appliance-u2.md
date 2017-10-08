---
title: "aaaStorSimple cloudu zařízení Update 3 | Microsoft Docs"
description: "Zjistěte, jak toocreate, nasazovat a spravovat o cloudu zařízení StorSimple ve službě Microsoft Azure virtual network. (Platí tooStorSimple Update 3 a novější)."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/10/2017
ms.author: alkohli
ms.openlocfilehash: ba60a629f1f4b8f0d4566eeb45bae8696f50d0af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-a-storsimple-cloud-appliance-in-azure-update-3-and-later"></a>Nasazení a správa řešení StorSimple Cloud Appliance v Azure (s aktualizací Update 3 a novější)

## <a name="overview"></a>Přehled

Hello StorSimple 8000 řady cloudu zařízení je další schopností, která se dodává s vaším řešením Microsoft Azure StorSimple. Hello cloudu zařízení StorSimple běží na virtuálním počítači ve virtuální síti Microsoft Azure a můžete ho tooback nahoru a klonování dat z hostitelů.

Tento článek popisuje podrobný postup toodeploy hello a spravovat o cloudu zařízení StorSimple v Azure. Po přečtení tohoto článku:

* Pochopte, jak se liší od fyzického zařízení hello hello cloudu zařízení.
* Být schopný toocreate a nakonfigurovat zařízení hello cloudu.
* Připojte zařízení toohello cloudu.
* Zjistěte, jak toowork s hello cloudové zařízení.

V tomto kurzu platí tooall hello zařízení StorSimple cloudu systémem Update 3 nebo novější.

#### <a name="cloud-appliance-model-comparison"></a>Porovnání modelů cloudových zařízení

Hello cloudu zařízení StorSimple je k dispozici v dva modely standardní 8010 (dříve označované jako hello 1100) a prémiový 8020 (zaveden v Update 2). Následující tabulka Hello uvede porovnání dvou modelů hello.

| Model zařízení | 8010<sup>1</sup> | 8020 |
| --- | --- | --- |
| **Maximální kapacita** |30 TB |64 TB |
| **Virtuální počítač Azure** |Standard_A3 (4 jádra, 7 GB paměti)| Standard_DS3 (4 jádra, 14 GB paměti)|
| **Dostupnost v oblastech** |Všechny oblasti Azure |Oblasti Azure, které podporují službu Storage úrovně Premium a virtuální počítače Azure DS3<br></br>Použití [tento seznam](https://azure.microsoft.com/regions/services/) toosee, pokud obě **virtuální počítače > DS-series** a **úložiště > Disk úložiště** jsou k dispozici ve vašem regionu. |
| **Typ úložiště** |Pro místní disky používá službu Azure Standard Storage<br></br> Zjistěte, jak příliš[vytvořit standardní účet úložiště](../storage/common/storage-create-storage-account.md) |Pro místní disky používá Azure Premium Storage.<sup>2</sup> <br></br>Zjistěte, jak příliš[vytvořit účet úložiště úrovně Premium](../storage/common/storage-premium-storage.md) |
| **Pokyny týkající se úloh** |Načítání souborů ze zálohy na úrovni položek |Scénáře vývoje a testování v cloudu <br></br>Úlohy s vyšším výkonem a nízkou latencí<br></br>Sekundární zařízení pro zotavení po havárii |

<sup>1</sup> *dříve označované jako hello 1100*.

<sup>2</sup> *obě hello 8010 a 8020 použít standardní úložiště Azure pro hello cloudu vrstvy. hello rozdíl existuje pouze v hello místní vrstvy v rámci zařízení hello*.

## <a name="how-hello-cloud-appliance-differs-from-hello-physical-device"></a>Jak se liší od fyzického zařízení hello hello cloudu zařízení

Hello cloudu zařízení StorSimple je jen pro software verze zařízení storsimple, která běží na jednom uzlu v virtuální počítač Microsoft Azure. Hello cloudu zařízení podporuje scénáře zotavení po havárii, ve kterých fyzické zařízení není k dispozici. Hello cloudu zařízení je vhodné pro použití v načítání na úrovni položek ze zálohy, místní zotavení po havárii a scénáře vývoje a testování v cloudu.

#### <a name="differences-from-hello-physical-device"></a>Rozdíl oproti hello fyzického zařízení

Hello následující tabulka uvádí některé hlavní rozdíly mezi hello cloudu zařízení StorSimple a fyzickým zařízením StorSimple hello.

|  | Fyzické zařízení | Cloudové zařízení |
| --- | --- | --- |
| **Umístění** |Se nachází v datovém centru hello. |Běží v Azure. |
| **Síťová rozhraní** |Má šest síťových rozhraní: DATA 0 až DATA 5. |Má pouze jedno síťové rozhraní: DATA 0. |
| **Registrace** |Během počáteční konfigurace krok hello zaregistrovány. |Registrace je samostatná úloha. |
| **Šifrovací klíč dat služby** |Obnovte na fyzickém zařízení hello a aktualizujte hello cloudu zařízení s hello nový klíč. |Nelze znovu vygenerovat z hello cloudu zařízení. |
| **Podporované typy svazků** |Podporuje místně připnuté a vrstvené svazky. |Podporuje pouze vrstvené svazky. |

## <a name="prerequisites-for-hello-cloud-appliance"></a>Předpoklady pro hello cloudu zařízení

Hello následující části popisují požadavky konfigurace hello pro vaše zařízení StorSimple cloudu. Před nasazením zařízení cloudu, zkontrolujte hello bezpečnostních aspektech použití zařízení cloudu.

[!INCLUDE [StorSimple Cloud Appliance security](../../includes/storsimple-8000-cloud-appliance-security.md)]

#### <a name="azure-requirements"></a>Požadavky na Azure

Než zřídíte hello cloudu zařízení, je třeba toomake hello následující přípravy v prostředí Azure:

* Ujistěte se, že ve svém datovém centru máte nasazené a spuštěné fyzické zařízení StorSimple řady 8000 (model 8100 nebo 8600). Zaregistrovat toto zařízení s hello stejné služby StorSimple Manager zařízení, která chcete toocreate a cloudu zařízení StorSimple pro.
* Pro zařízení hello cloudu [konfigurace virtuální sítě v Azure](../virtual-network/virtual-networks-create-vnet-arm-pportal.md). Používáte-li službu Premium Storage, musíte vytvořit virtuální síť v oblasti Azure, která podporuje službu Premium Storage. oblasti Hello Storage úrovně Premium jsou oblasti, které odpovídají toohello řádek pro ukládání na Disk v hello [seznam služeb Azure podle oblasti](https://azure.microsoft.com/regions/services/).
* Doporučujeme vám, že používáte hello výchozí server DNS poskytovaný platformou Azure místo zadávání vlastního názvu serveru DNS. Pokud není platný název serveru DNS nebo serveru DNS hello není možné tooresolve IP adresy správně, vytvoření hello hello cloudu zařízení se nezdaří.
* Připojení point-to-site a site-to-site jsou volitelná, ale nejsou vyžadována. Pokud chcete, můžete nastavit tyto možnosti pro pokročilejší scénáře.
* Můžete vytvořit [virtuální počítače Azure](../virtual-machines/virtual-machines-windows-quick-create-portal.md) (hostitelské servery) ve virtuální síti hello, který můžete použít hello svazky vystavené hello cloudu zařízení. Tyto servery musí splňovat následující požadavky hello:

  * Být virtuální počítače Windows nebo Linux s nainstalovaným softwarem iniciátoru iSCSI.
  * Běžet ve hello stejné virtuální síti jako hello cloudu zařízení.
  * Být schopný tooconnect cíle iSCSI toohello hello cloudu zařízení prostřednictvím interní IP adresu hello hello cloudu zařízení.
  * Zajistěte, aby jste nakonfigurovali podporu pro iSCSI a cloudové přenosy na hello stejné virtuální síti.

#### <a name="storsimple-requirements"></a>Požadavky na StorSimple

Ujistěte se, hello následující služby StorSimple Manager zařízení tooyour aktualizace před vytvořením cloudu zařízení:

* Přidat [záznamy řízení přístupu](storsimple-8000-manage-acrs.md) hello virtuálních počítačů, které jsou probíhající toobe hello hostitelské servery pro vaše zařízení cloudu.
* Použití [účet úložiště](storsimple-8000-manage-storage-accounts.md#add-a-storage-account) v hello stejné oblasti jako zařízení hello cloudu. Účty úložiště v jiných oblastech mohou vést k nižšímu výkonu. Účet Standard nebo Premium Storage můžete použít s hello cloudu zařízení. Další informace o toocreate [standardní účet úložiště](../storage/common/storage-create-storage-account.md) nebo [účtu Storage úrovně Premium](../storage/common/storage-premium-storage.md)
* Použijte jiný účet úložiště pro vytvoření cloudu zařízení z hello, jedna pro vaše data. Pomocí hello stejný účet úložiště může vést k nižšímu výkonu.

Ujistěte se, že máte hello následující informace, než můžete začít:

* Účet na webu Azure Portal s přihlašovacími údaji pro přístup.
* Kopie šifrovacího klíče dat hello služby z fyzického zařízení registrované služby StorSimple Manager zařízení toohello.

## <a name="create-and-configure-hello-cloud-appliance"></a>Vytvoření a konfigurace zařízení cloudu hello

Před provedením těchto postupů se ujistěte, že jste splnili hello [požadavky pro zařízení cloudu hello](#prerequisites-for-the-cloud-appliance).

Proveďte následující kroky toocreate o zařízení StorSimple cloudu hello.

### <a name="step-1-create-a-cloud-appliance"></a>Krok 1: Vytvoření cloudového zařízení

Proveďte následující kroky toocreate hello zařízení StorSimple cloudu hello.

[!INCLUDE [Create a cloud appliance](../../includes/storsimple-8000-create-cloud-appliance-u2.md)]

Pokud se nezdaří vytvoření hello hello cloudu zařízení v tomto kroku, nemusí mít toohello připojení k Internetu. Další informace, přejděte příliš[vyřešit chyby připojení k Internetu](#troubleshoot-internet-connectivity-errors) při vytváření cloudu zařízení.

### <a name="step-2-configure-and-register-hello-cloud-appliance"></a>Krok 2: Konfigurace a registrace zařízení cloudu hello

Než začnete tento postup, ujistěte se, že máte kopii šifrovacího klíče dat služby hello. šifrovací klíč dat služby Hello se vytvoří při registraci první fyzického zařízení StorSimple s hello služby StorSimple Manager zařízení. Byly pokyn toosave jej na bezpečném místě. Pokud nemáte kopii šifrovacího klíče dat služby hello, musí Microsoft Support požádejte o pomoc.

Proveďte následující kroky tooconfigure hello a zaregistrujte zařízení StorSimple cloudu.

[!INCLUDE [Configure and register a cloud appliance](../../includes/storsimple-8000-configure-register-cloud-appliance.md)]

### <a name="step-3-optional-modify-hello-device-configuration-settings"></a>Krok 3: (Volitelné) upravit hello nastavení konfigurace pro zařízení

Hello následující část popisuje nastavení konfigurace zařízení hello potřebné pro hello cloudu zařízení StorSimple, chcete-li toouse CHAP, Snapshot Manager zařízení StorSimple nebo změnit heslo správce zařízení hello.

#### <a name="configure-hello-chap-initiator"></a>Konfigurace iniciátoru CHAP hello

Tento parametr obsahuje přihlašovací údaje hello, které vaší cloudové zařízení (cíl) očekává z iniciátorů hello (serverů), které se pokoušíte tooaccess hello svazky. iniciátory Hello zadejte CHAP uživatelské jméno a heslo tooidentify CHAP sami tooyour zařízení během tohoto ověřování. Podrobný postup je uveden příliš[konfigurace CHAP pro vaše zařízení](storsimple-8000-configure-chap.md#unidirectional-or-one-way-authentication).

#### <a name="configure-hello-chap-target"></a>Konfigurace cíle CHAP hello

Tento parametr obsahuje přihlašovací údaje hello, které vaše cloudové zařízení používá, když iniciátor podporou CHAP požádá o vzájemné nebo obousměrné ověření. Vaše zařízení cloudu používá uživatelské jméno Reverse CHAP a Reverse CHAP heslo tooidentify samotné toohello iniciátor během tohoto procesu ověřování.

> [!NOTE]
> Nastavení cíle protokolu CHAP jsou globální nastavení. Když tato nastavení se aplikují, připojené všechny svazky hello toohello cloudu zařízení ověřování CHAP.

Podrobný postup je uveden příliš[konfigurace CHAP pro vaše zařízení](storsimple-8000-configure-chap.md#bidirectional-or-mutual-authentication).

#### <a name="configure-hello-storsimple-snapshot-manager-password"></a>Konfigurace hesla Snapshot Manageru zařízení StorSimple hello

Software Snapshot Manager zařízení StorSimple se nachází na hostiteli s Windows a umožňuje správci toomanage zálohy zařízení StorSimple ve formě hello místních a cloudových snímků.

> [!NOTE]
> Hostiteli s Windows hello cloudu zařízení, je virtuální počítač Azure.

Při konfiguraci zařízení v hello Snapshot Manager zařízení StorSimple, zobrazí se výzva tooprovide hello tooauthenticate StorSimple zařízení IP adresu a heslo zařízení úložiště. Podrobný postup je uveden příliš[konfigurace zařízení StorSimple Snapshot Manager heslo](storsimple-8000-change-passwords.md#set-the-storsimple-snapshot-manager-password).

#### <a name="change-hello-device-administrator-password"></a>Heslo správce zařízení hello změn

Při použití hello prostředí Windows PowerShell rozhraní tooaccess hello cloudu zařízení, jsou požadované tooenter hesla správce zařízení. Hello zabezpečení vašich dat musíte změnit toto heslo před použitím hello cloudu zařízení. Podrobný postup je uveden příliš[konfigurace hesla správce zařízení](../storsimple/storsimple-8000-change-passwords.md#change-the-device-administrator-password).

## <a name="connect-remotely-toohello-cloud-appliance"></a>Vzdálené připojení zařízení toohello cloudu

Ve výchozím nastavení není povoleno zařízení cloudu tooyour vzdáleného přístupu prostřednictvím rozhraní Windows PowerShell hello. Nejprve je nutné povolit vzdálené správy na zařízení cloudu hello a na hello klient použil tooaccess hello cloudu zařízení.

Hello dvoustupňové postupem popisuje, jak tooconnect vzdáleně tooyour cloudové zařízení.

### <a name="step-1-configure-remote-management"></a>Krok 1: Konfigurace vzdálené správy

Proveďte následující kroky tooconfigure vzdálenou správu pro vaše zařízení StorSimple cloudu hello.

[!INCLUDE [Configure remote management via HTTP for cloud appliance](../../includes/storsimple-8000-configure-remote-management-http-device.md)]

### <a name="step-2-remotely-access-hello-cloud-appliance"></a>Krok 2: Vzdálený přístup k zařízení cloudu hello

Po povolení vzdálené správy na zařízení hello cloudu pomocí prostředí Windows PowerShell vzdálenou komunikaci tooconnect toohello zařízení z jiného virtuálního počítače ve hello stejné virtuální síti. Například můžete připojit z hello hostitele virtuálních počítačů, že jste nakonfigurovali a používali tooconnect iSCSI. Ve většině nasazení se otevře veřejný koncový bod tooaccess váš hostitel virtuálního počítače, který můžete použít pro přístup k zařízení hello cloudu.

> [!WARNING]
> **Pro zvýšení zabezpečení důrazně doporučujeme používat protokol HTTPS, pokud se připojujete toohello koncových bodů a pak odstraňte koncové body hello po dokončení vzdálené relace prostředí PowerShell.**

Je třeba postupovat podle pokynů hello v [vzdáleném připojení zařízení StorSimple tooyour](storsimple-8000-remote-connect.md) tooset až vzdálené komunikace pro vaše zařízení cloudu.

## <a name="connect-directly-toohello-cloud-appliance"></a>Připojte se přímo toohello cloudu zařízení

Můžete také připojit přímo toohello cloudu zařízení. tooconnect přímo toohello cloudu zařízení z jiného počítače mimo hello virtuální sítě nebo mimo hello prostředí Microsoft Azure, je třeba vytvořit další koncové body.

Proveďte následující kroky toocreate veřejný koncový bod na zařízení cloudu hello hello.

[!INCLUDE [Create public endpoints on a cloud appliance](../../includes/storsimple-8000-create-public-endpoints-cloud-appliance.md)]

Doporučujeme vám, zda je připojit z jiného virtuálního počítače ve hello stejné virtuální sítě, protože tento postup minimalizuje hello počet veřejných koncových bodů ve virtuální síti. V takovém případě připojit toohello virtuálního počítače prostřednictvím relace vzdálené plochy a pak konfiguraci tohoto virtuálního počítače pro použití stejně jako u jiných klientů systému Windows v místní síti. Není nutné číslem veřejného portu hello tooappend protože hello port je již znám.

## <a name="work-with-hello-storsimple-cloud-appliance"></a>Práce s hello cloudu zařízení StorSimple

Teď, když je vytvořen a nakonfigurován hello cloudu zařízení StorSimple, jste připravené toostart práci s ním. Na cloudovém zařízení můžete pracovat s kontejnery svazků, svazky a zásadami zálohování stejně jako na fyzickém zařízení StorSimple. Hello jediným rozdílem je, že to musíte toomake vyberte hello cloudu zařízení ze seznamu zařízení. Odkazovat příliš[použít toomanage služby StorSimple Manager zařízení hello cloudu zařízení](storsimple-8000-manager-service-administration.md) podrobné postupy různých správy úloh pro zařízení cloudu hello hello.

Hello následující části popisují některé rozdíly hello, které zaznamenáte při práci s hello cloudu zařízení.

### <a name="maintain-a-storsimple-cloud-appliance"></a>Údržba řešení StorSimple Cloud Appliance

Protože jde o softwarové zařízení, údržba pro zařízení cloudu hello je minimální při porovnání toomaintenance pro hello fyzického zařízení.

Cloudové zařízení nelze aktualizovat. Použijte nejnovější verzi softwaru toocreate nové zařízení cloudu hello.


### <a name="storage-accounts-for-a-cloud-appliance"></a>Účty úložiště pro cloudové zařízení

Účty služby Storage jsou vytvořeny pro použití hello služby StorSimple Manager zařízení, hello cloudu zařízení a hello fyzického zařízení. Při vytváření účtů úložiště, doporučujeme použít identifikátor oblasti v hello popisný název. To pomáhá zajistit, že danou oblast hello je konzistentní v rámci všech součástí systému hello. Pro zařízení cloudu, je důležité, zda jsou všechny součásti hello v hello stejné problémy s výkonem tooprevent oblast.

Podrobný postup najdete příliš[přidání účtu úložiště](storsimple-8000-manage-storage-accounts.md#add-a-storage-account).

### <a name="deactivate-a-storsimple-cloud-appliance"></a>Deaktivace řešení StorSimple Cloud Appliance

Pokud deaktivujete cloudu zařízení, akce hello odstraní hello virtuálních počítačů a hello prostředky vytvořené při jeho zřizování. Po deaktivaci hello cloudu zařízení nejde obnovit tooits předchozího stavu. Před deaktivací hello cloudu zařízení, ujistěte se, že toostop nebo odstranit klienty a hostitele, které na ní závisí.

Deaktivace zařízení cloudu výsledkem hello následující akce:

* Hello cloudu zařízení se odebere.
* disk Hello operačního systému a datové disky, které jsou vytvořené pro hello cloudu zařízení se odeberou.
* Hello hostované služby a virtuální sítě vytvořené při zřizování zůstanou zachovány. Pokud je nepoužíváte, je nutné je odstranit ručně.
* Cloudové snímky vytvořené pro hello cloudu zařízení zůstanou zachovány.

Podrobný postup najdete příliš[deaktivace a odstranění zařízení StorSimple](storsimple-8000-deactivate-and-delete-device.md).

Jakmile hello cloudu zařízení se zobrazí jako deaktivované v okně service Manager zařízení StorSimple hello, můžete odstranit hello cloudu zařízení ze seznamu zařízení na hello **zařízení** okno.

### <a name="start-stop-and-restart-a-cloud-appliance"></a>Spuštění, zastavení a restartování cloudového zařízení
Na rozdíl od fyzického zařízení StorSimple hello neexistuje žádné napájení nebo vypnutí tlačítko toopush v zařízení s StorSimple cloudu. Však mohou nastat situace, kdy potřebujete toostop a restartujte hello cloudu zařízení.

Hello nejjednodušší způsob, jak můžete toostart, ukončete a restartujte cloudu zařízení je prostřednictvím hello okno služby virtuálních počítačů. Služba Virtual machine hello přejděte. Hello seznamu virtuálních počítačů identifikovat hello virtuálních počítačů odpovídající tooyour cloudu zařízení (stejný název) a klikněte na název virtuálního počítače hello. Když se podíváte v okně vaší virtuální počítač, je stav zařízení cloudu hello **systémem** vzhledem k tomu, že je spuštěna ve výchozím nastavení po jeho vytvoření. Virtuální počítač můžete kdykoli spustit, zastavit nebo restartovat.

[!INCLUDE [Stop and restart cloud appliance](../../includes/storsimple-8000-stop-restart-cloud-appliance.md)]

### <a name="reset-toofactory-defaults"></a>Obnovit výchozí hodnoty toofactory
Pokud se rozhodnete, které mají toostart přes se vaše zařízení cloudu, deaktivovat a odstranit a pak vytvořte novou.

## <a name="fail-over-toohello-cloud-appliance"></a>Selhání toohello cloudu zařízení
Zotavení po havárii (DR) je jedním z hello klíčových scénářů, které hello zařízení StorSimple cloudu byl navržený pro. V tomto scénáři hello fyzického zařízení StorSimple nebo celého datového centra nemusí být k dispozici. Naštěstí můžete použít operace toorestore zařízení cloudu v alternativním umístění. Během zotavení po Havárii hello kontejnery svazků ze zdrojového zařízení hello změnit vlastnictví a jsou přenášená toohello cloudu zařízení.

Hello požadavky pro zotavení po Havárii jsou:

* zařízení Hello cloud je vytvořen a nakonfigurován.
* Všechny svazky hello v rámci kontejneru svazků hello jsou offline.
* Hello kontejneru svazků, které jste převzetí služeb při selhání, má přiřazený cloudový snímek.

> [!NOTE]
> * Pokud používáte zařízení cloudu jako hello sekundární zařízení pro zotavení po Havárii, mějte na paměti, že hello 8010 má úložiště Standard Storage 30 TB a zařízení 8020 úložiště Premium Storage 64 TB. Hello vyšší kapacity 8020 cloudu zařízení může být vhodnější pro scénář zotavení po havárii.

Podrobný postup najdete příliš[převzít tooa cloudu zařízení](storsimple-8000-device-failover-cloud-appliance.md).

## <a name="delete-hello-cloud-appliance"></a>Odstranit hello cloudu zařízení
Pokud jste dříve nakonfigurovali a používali o zařízení StorSimple cloudu ale nyní chcete toostop nabíhání poplatků za jeho použití, je nutné zastavit hello cloudu zařízení. Ukončení hello cloudu zařízení zruší přidělení hello virtuálních počítačů. Tato akce zastaví nabíhání poplatků za předplatné. Nicméně bude pokračovat Hello poplatky za úložiště pro hello operačního systému a datové disky.

toostop, které všechny hello poplatky, je třeba odstranit zařízení hello cloudu. zálohování hello toodelete vytvořené hello cloudu zařízení, můžete deaktivovat nebo odstranit hello zařízení. Další informace naleznete v tématu [Deaktivace a odstranění zařízení StorSimple](storsimple-8000-deactivate-and-delete-device.md).

[!INCLUDE [Delete a cloud appliance](../../includes/storsimple-8000-delete-cloud-appliance.md)]

## <a name="troubleshoot-internet-connectivity-errors"></a>Řešení potíží s připojením k internetu
Během vytváření hello cloudu zařízení, pokud neexistuje žádné toohello připojení k Internetu, hello vytvoření kroku dojde k chybě. chyby připojení k Internetu tootroubleshoot, proveďte hello proveďte kroky v hello portálu Azure:

1. [Vytvořte v Azure virtuální počítač s Windows Serverem 2012](/articles/virtual-machines/windows/quick-create-portal.md). Tento virtuální počítač by měl použít hello stejný účet úložiště, virtuální síť a podsíť, jak používat vaše zařízení cloudu. Pokud je existující hello hostitele Windows serveru v Azure pomocí stejného účtu úložiště, virtuální síť a podsíť, můžete ji použít i připojení k Internetu tootroubleshoot hello.
2. Vzdálené přihlášení do hello virtuální počítač vytvořený v předchozím kroku hello.
3. Otevřete okno příkazového řádku uvnitř hello virtuálního počítače (Win + R a pak zadejte `cmd`).
4. Spusťte následující cmd příkazového řádku hello hello.

    `nslookup windows.net`
5. Pokud `nslookup` nezdaří, pak zařízení cloudu hello registraci služby StorSimple Manager zařízení toohello brání selhání připojení k Internetu.
6. Proveďte změny hello požadované tooensure tooyour virtuální sítě, která hello cloudu zařízení je možné tooaccess Azure lokalit, jako _windows.net_.

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[použít toomanage služby StorSimple Manager zařízení hello cloudu zařízení](storsimple-8000-manager-service-administration.md).
* Pochopit, jak příliš[obnovit svazek StorSimple ze zálohovacího skladu](storsimple-8000-restore-from-backup-set-u2.md).
