---
title: "aaaProvision pole virtuální zařízení StorSimple v Hyper-V | Microsoft Docs"
description: "Tento druhý kurz v nasazení pole virtuální zařízení StorSimple zahrnuje zřizování virtuální pole Hyper-v."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 4354963c-e09d-41ac-9c8b-f21abeae9913
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/15/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f47d642f740827ae1440b819e07067c6a183527f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-storsimple-virtual-array---provision-in-hyper-v"></a>Nasazení zařízení StorSimple virtuální pole - zřízení technologie Hyper-v
![](./media/storsimple-virtual-array-deploy2-provision-hyperv/hyperv4.png)

## <a name="overview"></a>Přehled
Tento kurz popisuje, jak tooprovision virtuální zařízení StorSimple pole v systému hostitele s technologií Hyper-V v systému Windows Server 2012 R2, Windows Server 2012 nebo Windows Server 2008 R2. Tento článek se týká nasazení toohello polí virtuální zařízení StorSimple v portálu Azure a cloudu Microsoft Azure Government.

Je třeba tooprovision oprávnění správce a nakonfigurovat virtuální pole. Hello zřizování a počáteční instalace může trvat přibližně 10 minut toocomplete.

## <a name="provisioning-prerequisites"></a>Zřizování požadavky
Zde najdete hello požadavky tooprovision virtuální pole v systému hostitele s technologií Hyper-V v systému Windows Server 2012 R2, Windows Server 2012 nebo Windows Server 2008 R2.

### <a name="for-hello-storsimple-device-manager-service"></a>Pro hello služby StorSimple Manager zařízení
Než začnete, ujistěte se, že:

* Jste dokončili všechny kroky hello v [Příprava hello portál pro pole virtuální zařízení StorSimple](storsimple-virtual-array-deploy1-portal-prep.md).
* Jste si stáhli hello virtuální pole image pro Hyper-V z hello portálu Azure. Další informace najdete v tématu **krok 3: stažení hello virtuální pole image** z [portál hello Příprava pro Průvodce pole virtuální zařízení StorSimple](storsimple-virtual-array-deploy1-portal-prep.md).

  > [!IMPORTANT]
  > Hello software spuštěný na hello pole virtuální zařízení StorSimple lze použít pouze s hello služby StorSimple Manager zařízení.
  >
  >

### <a name="for-hello-storsimple-virtual-array"></a>Pro hello pole virtuální zařízení StorSimple
Před nasazením virtuální pole, ujistěte se, že:

* Máte přístup tooa hostitelský systém s Hyper-V v systému Windows Server 2008 R2 nebo novější, můžou být použité tooa zřídit zařízení.
* Hello systém hostitele je možné toodedicate hello následující prostředky tooprovision virtuální pole:

  * Minimálně 4 jádra.
  * Alespoň 8 GB paměti RAM. Pokud máte v plánu tooconfigure hello virtuální pole jako souborový server, 8 GB podporuje menší než 2 miliony souborů. Je nutné 16 GB paměti RAM toosupport 2 – 4 miliony souborů.
  * Jedno síťové rozhraní.
  * 500 GB virtuální disk pro data.

### <a name="for-hello-network-in-hello-datacenter"></a>Pro síť hello v datovém centru hello
Než začnete, zkontrolujte hello sítě toodeploy požadavky pole virtuální zařízení StorSimple a nakonfigurujte síť datového centra hello správně. Další informace najdete v tématu [pole virtuální zařízení StorSimple sítě požadavky](storsimple-ova-system-requirements.md#networking-requirements).

## <a name="step-by-step-provisioning"></a>Krok za krokem zřizování
tooprovision a připojit virtuální pole tooa, musíte tooperform hello následující kroky:

1. Zajistěte, aby systém hostitele hello dostatek prostředků toomeet hello virtuální pole minimální požadavky.
2. Zřídit virtuální pole ve vašem hypervisoru.
3. Spusťte virtuální pole hello a získat hello IP adresu.

Každý z těchto kroků je vysvětleno v následující části hello.

## <a name="step-1-ensure-that-hello-host-system-meets-minimum-virtual-array-requirements"></a>Krok 1: Zkontrolujte, zda hello hostitelský systém splňuje požadavky na minimální virtuální pole
toocreate virtuální pole, je třeba:

* role Hello technologie Hyper-V nainstalovaná v systému Windows Server 2012 R2, Windows Server 2012 nebo Windows Server 2008 R2 SP1.
* Microsoft Hyper-V Manager na klienta se systémem Microsoft Windows připojené toohello hostitele.

Zajistěte, aby byl tento hello základní hardware (hostitelský systém), na kterém vytváříte virtuální pole hello možné toodedicate hello následující prostředky tooyour virtuální pole:

* Minimálně 4 jádra.
* Alespoň 8 GB paměti RAM. Pokud máte v plánu tooconfigure hello virtuální pole jako souborový server, 8 GB podporuje menší než 2 miliony souborů. Je nutné 16 GB paměti RAM toosupport 2 – 4 miliony souborů.
* Jedno síťové rozhraní.
* 500 GB virtuální disk pro data systému.

## <a name="step-2-provision-a-virtual-array-in-hypervisor"></a>Krok 2: Zřídit virtuální pole v hypervisoru
Proveďte následující kroky tooprovision zařízení ve vaší hypervisoru hello.

#### <a name="tooprovision-a-virtual-array"></a>tooprovision virtuální pole
1. Na hostiteli s Windows Server zkopírujte hello virtuální pole image tooa místní disk. Jste si stáhli tuto bitovou kopii (VHD nebo VHDX) prostřednictvím hello portálu Azure. Poznamenejte si hello umístění, kam jste zkopírovali hello image jako později v postupu hello používáte tuto bitovou kopii.
2. Otevřete **správce serveru**. V hello pravém horním rohu klikněte na **nástroje** a vyberte **Správce technologie Hyper-V**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image1.png)  

   Pokud používáte Windows Server 2008 R2, otevřete hello Správce technologie Hyper-V. Ve Správci serveru klikněte na tlačítko **role > Hyper-V > Správce technologie Hyper-V**.
3. V **Správce technologie Hyper-V**, v podokně hello oboru, klikněte pravým tlačítkem na váš systém uzlu tooopen hello kontextové nabídky a pak klikněte na tlačítko **nový** > **virtuální počítač**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image2.png)
4. Na hello **před zahájením** stránku hello Průvodce novým virtuálním počítačem, klikněte na tlačítko **Další**.
5. Na hello **zadejte název a umístění** zadejte **název** pro vaše virtuální pole. Klikněte na **Další**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image4.png)
6. Na hello **určit generaci** stránky, vyberte typ image hello zařízení a pak klikněte na tlačítko **Další**. Tato stránka se nezobrazí, pokud používáte Windows Server 2008 R2.

   * Zvolte **generace 2** Pokud jste stáhli bitovou kopii .vhdx pro Windows Server 2012 nebo novější.
   * Zvolte **generace 1** Pokud jste stáhli VHD image pro Windows Server 2008 R2 nebo novější.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image5.png)
7. Na hello **přiřazení paměti** stránky, zadejte **spouštěcí paměť** z alespoň **8192 MB**, nemáte povolte dynamickou paměť a pak klikněte na tlačítko **Další**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image6.png)  
8. Na hello **konfigurace sítě** zadejte hello virtuální přepínač, který je připojený toohello Internetu a pak klikněte na tlačítko **Další**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image7.png)
9. Na hello **připojit virtuální pevný disk** vyberte **použít existující virtuální pevný disk**, zadejte umístění hello hello virtuální pole obrázku (vhdx nebo VHD) a pak klikněte na tlačítko **Další**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image8m.png)
10. Zkontrolujte hello **Souhrn** a pak klikněte na **Dokončit** toocreate hello virtuálního počítače.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image9.png)
11. toomeet hello minimální požadavky, je nutné 4 jádra. tooadd 4 virtuální procesory, vyberte hostitelský systém v hello **Správce technologie Hyper-V** okno. V pravém podokně hello pod hello seznam **virtuální počítače**, vyhledejte hello virtuálního počítače, kterou jste právě vytvořili. Vyberte a klikněte pravým tlačítkem na název počítače hello a vyberte **nastavení**.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image10.png)
12. Na hello **nastavení** klepněte ve hello levém podokně, **procesoru**. V pravém hello podokně nastavte **počet virtuálních procesorů** too4 (nebo více). Klikněte na tlačítko **Použít**.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image11.png)
13. toomeet hello minimální požadavky, musíte taky tooadd 500 GB virtuální datový disk. V hello **nastavení** stránky:

    1. V levém podokně hello vyberte **řadič SCSI**.
    2. V pravém podokně hello vyberte **pevný disk,** a klikněte na tlačítko **přidat**.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image12.png)
14. Na hello **pevný disk** stránky, vyberte hello **virtuální pevný disk** možnost a klikněte na tlačítko **nový**. Hello **Průvodce vytvořením virtuálního pevného disku** spustí.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image13.png)
15. Na hello **před zahájením** stránku hello Průvodce vytvořením virtuálního pevného disku, klikněte na tlačítko **Další**.
16. Na hello **stránky výběr formátu disku**, přijměte výchozí možnost hello **VHDX** formátu. Klikněte na **Další**. Tato obrazovka není uveden, pokud systém Windows Server 2008 R2.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image15.png)
17. Na hello **zvolit typ disku stránky**, nastavte typ virtuálního pevného disku jako **dynamicky se zvětšující** (doporučeno). **Pevná velikost** disk by pracovat, ale může být nutné toowait dlouhou dobu. Doporučujeme, abyste nepoužívejte hello **Differencing** možnost. Klikněte na **Další**. V systému Windows Server 2012 R2 a Windows Server 2012 **dynamicky se zvětšující** je hello výchozí možnost, zatímco ve Windows serveru 2008 R2, je výchozí hello **pevnou velikost**.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image16.png)
18. Na hello **zadat název a umístění** zadejte **název** a také **umístění** (můžete procházet tooone) pro hello datový disk. Klikněte na **Další**.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image17.png)
19. Na hello **konfigurovat Disk** stránky, vyberte hello možnost **vytvořit nový prázdný virtuální pevný disk** a zadejte velikost hello jako **500 GB** (nebo více). Zatímco 500 GB je minimální požadavek hello, můžete zřídit vždy větší disku. Všimněte si, že nelze rozbalit nebo zmenšení disku hello jednou zřízený. Další informace o hello velikost disku tooprovision, projděte si část nastavení velikosti hello v hello [osvědčené postupy dokumentu](storsimple-ova-best-practices.md). Klikněte na **Další**.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image18.png)
20. Na hello **Souhrn** , zkontrolujte podrobnosti hello data virtuálního disku a pokud splnit, klikněte na tlačítko **Dokončit** toocreate hello disku. Hello Průvodce zavře, virtuální pevný disk je přidat tooyour počítače.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image19.png)
21. Vrátí toohello **nastavení** stránky. Klikněte na tlačítko **OK** tooclose hello **nastavení** stránku a vrátit okno Správce tooHyper-V.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image20.png)

## <a name="step-3-start-hello-virtual-array-and-get-hello-ip"></a>Krok 3: Spuštění virtuální pole hello a získat hello IP
Proveďte následující kroky toostart hello virtuální pole a připojit tooit.

#### <a name="toostart-hello-virtual-array"></a>toostart hello virtuální pole
1. Spusťte virtuální pole hello.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image21.png)
2. Po hello zařízení běží, vyberte hello zařízení, klikněte pravým tlačítkem a vyberte **Connect**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image22.png)
3. Můžete mít toowait 5 až 10 minut pro hello zařízení toobe připraven. Stavová zpráva se zobrazí na hello konzoly tooindicate hello průběh. Poté, co je připraven hello zařízení, se příliš**akce**. Stiskněte klávesu `Ctrl + Alt + Delete` toolog v toohello virtuální pole. Výchozí uživatel Hello *StorSimpleAdmin* a hello výchozí heslo je *Heslo1*.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image23.png)
4. Z bezpečnostních důvodů vyprší platnost hesla správce zařízení hello při prvním přihlášení hello. Jste výzvami toochange hello heslo.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image24.png)

   Zadejte heslo obsahující alespoň 8 znaků. Hello heslo musí splňovat minimálně 3 z následujících 4 požadavky hello: velká písmena, malá písmena, číselné a speciální znaky. Zadejte znovu heslo tooconfirm hello ho. Zobrazí se upozornění, že došlo ke změně toto heslo hello.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image25.png)
5. Po hello heslo je úspěšně změněno, může restartovat virtuální pole hello. Počkejte toostart hello zařízení.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image26.png)

    konzole Windows PowerShell Hello hello zařízení se zobrazí spolu s indikátor průběhu.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image27.png)
6. Kroky 6 až 8 se projeví pouze při dalším spuštění v prostředí bez služby DHCP. Pokud jste v prostředí s DHCP, pak Přeskočit tyto kroky a přejít toostep 9. Pokud je spouštěn nahoru zařízení v prostředí bez služby DHCP, zobrazí se následující obrazovka hello.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image28m.png)

    V dalším kroku nakonfigurujte hello sítě.
7. Použití hello `Get-HcsIpAddress` příkaz toolist hello síťových rozhraní na vaše virtuální pole povolené. Pokud vaše zařízení má jedno síťové rozhraní povolena, je hello výchozí název přiřazený toothis rozhraní `Ethernet`.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image29m.png)
8. Použití hello `Set-HcsIpAddress` rutiny tooconfigure hello sítě. Viz následující ukázka hello:

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image30.png)
9. Po dokončení počáteční nastavení hello a hello zařízení má spuštění nahoru, zobrazí se text hlavičky hello zařízení. Poznamenejte si hello IP adresu a adresu URL hello zobrazené v hello banner text toomanage hello zařízení. Použijte tuto IP adresu tooconnect toohello webového uživatelského rozhraní virtuální pole a dokončení hello místní instalaci a registraci.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image31m.png)
10. (Volitelné) Tento krok proveďte jenom v případě, že nasazujete zařízení v hello Cloud vlády. Nyní povolíte hello Spojených států informace o zpracování Standard FIPS (Federal) režim na zařízení. standard Hello FIPS 140 definuje kryptografické algoritmy pro použití schváleno nám Federal government počítačových systémů hello ochranu citlivá data.

    1. tooenable hello režimu FIPS, spusťte následující rutinu hello:

        `Enable-HcsFIPSMode`
    2. Restartování zařízení po povolení režimu FIPS hello tak, aby ověření kryptografických hello vstoupí v platnost.

       > [!NOTE]
       > Můžete povolit nebo zakázat režim FIPS ve vašem zařízení. Střídání hello zařízení mezi režim FIPS a standardu FIPS není podporováno.
       >
       >

Pokud zařízení nesplňuje hello minimální požadavky na konfiguraci, zobrazí hello následující došlo k chybě v text hlavičky hello (zobrazené dole). Upravte konfiguraci zařízení hello tak, aby hello počítač má odpovídající zdroje toomeet hello minimální požadavky. Potom můžete restartovat a toohello zařízení připojit. Odkazovat toohello minimální požadavky na konfiguraci v [krok 1: Zkontrolujte, zda hello hostitelský systém splňuje požadavky na minimální virtuální pole](#step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements).

![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image32.png)

Pokud potýkáte chybě během počáteční konfigurace hello pomocí hello místního webového uživatelského rozhraní, podívejte se na toohello následující pracovních postupů:

* Spustit diagnostické testy příliš[řešení potíží s instalací webového uživatelského rozhraní](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).
* [Generovat balíček protokolu a prohlížení soubory protokolů](storsimple-ova-web-ui-admin.md#generate-a-log-package).

## <a name="next-steps"></a>Další kroky
* [Nastavení pole virtuální zařízení StorSimple jako souborový server](storsimple-virtual-array-deploy3-fs-setup.md)
* [Nastavení pole virtuální zařízení StorSimple jako serveru iSCSI](storsimple-virtual-array-deploy3-iscsi-setup.md)
