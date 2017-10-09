---
title: "aaaProvision pole virtuální zařízení StorSimple ve službě VMware | Microsoft Docs"
description: "V tomto kurzu druhé pole virtuální zařízení StorSimple řady nasazení zahrnuje zřizování virtuálního zařízení ve službě VMware."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 0425b2a9-d36f-433d-8131-ee0cacef95f8
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/15/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0912c1c315a04ea46b6373a8fcd5554ecae14e61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-storsimple-virtual-array---provision-in-vmware"></a>Nasazení zařízení StorSimple virtuální pole - zřídit ve službě VMware
![](./media/storsimple-virtual-array-deploy2-provision-vmware/vmware4.png)

## <a name="overview"></a>Přehled
Tento kurz popisuje, jak tooprovision a připojte tooa pole virtuální zařízení StorSimple v hostitelském systému, systémem VMware ESXi 5.5 a vyšší. Tento článek se týká nasazení toohello polí virtuální zařízení StorSimple v portálu Azure a hello cloudu Microsoft Azure Government.

Potřebujete tooprovision oprávnění správce a připojit tooa virtuální zařízení. Hello zřizování a počáteční instalace může trvat přibližně 10 minut toocomplete.

## <a name="provisioning-prerequisites"></a>Zřizování požadavky
Hello požadavky tooprovision virtuálního zařízení v hostitelském systému, systémem VMware ESXi 5.5 a vyšší, jsou následující.

### <a name="for-hello-storsimple-device-manager-service"></a>Pro hello služby StorSimple Manager zařízení
Než začnete, ujistěte se, že:

* Jste dokončili všechny kroky hello v [Příprava hello portál pro pole virtuální zařízení StorSimple](storsimple-virtual-array-deploy1-portal-prep.md).
* Image virtuálního zařízení hello pro VMware jste si stáhli z portálu Azure hello. Další informace najdete v tématu **krok 3: stažení hello virtuální zařízení image** z [portál hello Příprava pro Průvodce pole virtuální zařízení StorSimple](storsimple-virtual-array-deploy1-portal-prep.md).

### <a name="for-hello-storsimple-virtual-device"></a>Pro virtuální zařízení StorSimple hello
Před nasazením virtuálního zařízení, ujistěte se, že:

* Máte přístup tooa hostitelský systém s technologií Hyper-V (2008 R2 nebo novější), můžou být použité tooa zřídit zařízení.
* Hello systém hostitele je možné toodedicate hello následující prostředky tooprovision virtuální zařízení:

  * Minimálně 4 jádra.
  * Alespoň 8 GB paměti RAM. Pokud máte v plánu tooconfigure hello virtuální pole jako souborový server, 8 GB podporuje menší než 2 miliony souborů. Je nutné 16 GB paměti RAM toosupport 2 – 4 miliony souborů.
  * Jedno síťové rozhraní.
  * 500 GB virtuální disk pro data systému.

### <a name="for-hello-network-in-datacenter"></a>Hello sítě datového centra.
Než začnete, ujistěte se, že:

* Zkontrolování hello síťové požadavky toodeploy virtuálního zařízení StorSimple a síti datového centra nakonfigurované hello podle požadavků hello. 

## <a name="step-by-step-provisioning"></a>Krok za krokem zřizování
tooprovision a připojit tooa virtuální zařízení, musíte tooperform hello následující kroky:

1. Zajistěte, aby systém hostitele hello dostatečná prostředky toomeet hello virtuální zařízení minimální požadavky.
2. Zřídit virtuální zařízení ve vaší hypervisoru.
3. Spusťte hello virtuální zařízení a získat hello IP adresu.

## <a name="step-1-ensure-host-system-meets-minimum-virtual-device-requirements"></a>Krok 1: Ujistěte se, že hostitelský systém splňuje požadavky na minimální virtuálního zařízení
toocreate virtuálního zařízení, budete potřebovat:

* Přístup k tooa hostitele se systémem VMware ESXi Server 5.5 a vyšší.
* Klienta VMware vSphere na hostiteli ESXi hello toomanage systému.

  * Minimálně 4 jádra.
  * Alespoň 8 GB paměti RAM. Pokud máte v plánu tooconfigure hello virtuální pole jako souborový server, 8 GB podporuje menší než 2 miliony souborů. Je nutné 16 GB paměti RAM toosupport 2 – 4 miliony souborů.
  * Jedno síťové rozhraní propojená síť toohello schopný tooInternet směrování provozu. Hello minimální šířky pásma Internetu by měl být tooallow 5 MB/s pro optimální fungování hello zařízení.
  * 500 GB virtuální disk pro data.

## <a name="step-2-provision-a-virtual-device-in-hypervisor"></a>Krok 2: Zřídit virtuální zařízení v hypervisoru
Proveďte následující kroky tooprovision virtuálního zařízení ve vaší hypervisoru hello.

1. Zkopírujte bitovou kopii virtuálního zařízení hello ve vašem systému. Jste si stáhli tuto bitovou kopii virtuálního prostřednictvím hello portálu Azure.

   1. Ujistěte se, že jste si stáhli hello nejnovější soubor bitové kopie. Pokud jste předtím stáhli hello image, stáhněte ji znovu tooensure máte nejnovější bitovou kopii hello. Hello nejnovější image má dva soubory (místo toho).
   2. Poznamenejte si hello umístění, kam jste zkopírovali hello image jako později v postupu hello používáte tuto bitovou kopii.

2. Přihlaste se toohello ESXi serveru pomocí klienta vSphere hello. Je nutné toohave správce oprávnění toocreate virtuálního počítače.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image1.png)
3. V klientovi vSphere hello, v části hello inventáře v levém podokně hello vyberte hello ESXi serveru.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image2.png)
4. Nahrajte hello VMDK toohello ESXi serveru. Přejděte toohello **konfigurace** kartě v pravém podokně hello. V části **hardwaru**, vyberte **úložiště**.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image3.png)
5. V hello pravým podokně v části **Datastores**, vyberte hello úložiště dat, kam chcete tooupload hello VMDK. úložiště dat Hello musí mít dostatek volného místa pro hello operačního systému a datové disky.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image4.png)
6. Klikněte pravým tlačítkem a vyberte **procházet úložiště**.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image5.png)
7. A **úložiště prohlížeče** se zobrazí v okně.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image6.png)
8. V panelu nástrojů hello, klikněte na ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image7.png) ikonu toocreate novou složku. Zadejte název složky hello a poznamenejte si ho. Tento název složky bude používat později, při vytváření virtuálního počítače (doporučeno osvědčený postup). Klikněte na **OK**.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image8.png)
9. Zobrazí se nová složka Hello v levém podokně hello hello **úložiště prohlížeče**.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image9.png)
10. Klikněte na ikonu nahrávání hello ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image10.png) a vyberte **nahrát soubor**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image11.png)
11. Procházet a soubory VMDK toohello, které jste stáhli. Existují dva soubory. Vyberte soubor tooupload.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image12m.png)
12. Klikněte na tlačítko **otevřete**. odesílání Hello toohello soubor VMDK hello zadané úložiště dat spustí. Ho může trvat několik minut, než tooupload souboru hello.
13. Po dokončení nahrávání hello zobrazí hello souboru v úložišti dat hello v hello složku, kterou jste vytvořili.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image14.png)

    Teď nahrát hello druhý VMDK souboru toohello stejné úložiště.
14. Vrátí toohello vSphere klienta okno. S ESXi server vybraný, klikněte pravým tlačítkem a vyberte **nový virtuální počítač**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image15.png)
15. A **vytvořit nový virtuální počítač** se okno. Na hello **konfigurace** stránky, vyberte hello **vlastní** možnost. Klikněte na **Další**.
    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image16.png)
16. Na hello **název a umístění** zadejte hello název virtuálního počítače. Tento název by měl odpovídat název složky hello (osvědčeného postupu) jste zadali v kroku 8.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image17.png)
17. Na hello **úložiště** vyberte úložiště dat chcete toouse tooprovision virtuálního počítače.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image18.png)
18. Na hello **verze virtuálního počítače** vyberte **verze virtuálního počítače: 8**. Verze 8 too11 všechny podporované.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image19.png)
19. Na hello **hostovaný operační systém** stránky, vyberte hello **hostovaný operační systém** jako **Windows**. Pro **verze**, hello rozevíracího seznamu vyberte **Microsoft Windows Server 2012 (64 bitů)**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image20.png)
20. Na hello **procesory** stránky, upravte hello **virtuální soketů** a **počet jader na virtuální soketu** , který hello **celkový počet jader**je 4 (nebo více). Klikněte na **Další**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image21.png)
21. Na hello **paměti** zadejte 8 GB (nebo více) paměti RAM. Klikněte na **Další**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image22.png)
22. Na hello **sítě** stránky, zadejte číslo hello hello síťových rozhraní. Hello minimální požadavek je jedno síťové rozhraní.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image23.png)
23. Na hello **řadič SCSI** přijměte výchozí hello **řadič LSI Logic SAS**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image24.png)
24. Na hello **vyberte Disk** vyberte **použijte existující virtuální disk**. Klikněte na **Další**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image25.png)
25. Na hello **vyberte stávající Disk** v části **cesta k souboru disku**, klikněte na tlačítko **Procházet**. Tím se otevře **procházet Datastores** dialogové okno. Přejděte toohello umístění, kde jste nahráli hello VMDK. Nyní uvidíte pouze jeden soubor v úložišti dat hello jako sloučeným hello dva soubory, které jste původně nahráli. Vyberte soubor hello a klikněte na tlačítko **OK**. Klikněte na **Další**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image26.png)
26. Na hello **pokročilé možnosti** , přijměte výchozí hello a klikněte na tlačítko **Další**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image27.png)
27. Na hello **připraven tooComplete** zkontrolujte všechna nastavení hello spojená s hello nového virtuálního počítače. Zkontrolujte **upravit nastavení virtuálního počítače hello před dokončením**. Klikněte na tlačítko **pokračovat**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image28.png)
28. Na hello **virtuální počítače vlastnosti** stránku hello **hardwaru** najděte hardwaru zařízení hello. Vyberte **nový pevný Disk**. Klikněte na tlačítko **Přidat**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image29.png)
29. Zobrazí **přidat Hardware** okno. Na hello **typ zařízení** v části **zvolte hello typu zařízení chcete tooadd**, vyberte **pevný Disk**a klikněte na tlačítko **Další**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image30.png)
30. Na hello **vyberte Disk** vyberte **vytvořit nový virtuální disk**. Klikněte na **Další**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image31.png)
31. Na hello **vytvoření disku** změňte hello **velikost disku** too500 GB (nebo více). Zatímco 500 GB je minimální požadavek hello, můžete zřídit vždy větší disku. Všimněte si, že nelze rozbalit nebo zmenšení disku hello jednou zřízený. Další informace o hello velikost disku tooprovision, projděte si část nastavení velikosti hello v hello [osvědčené postupy dokumentu](storsimple-ova-best-practices.md). V části **disku zřizování**, vyberte **dynamickým zajišťováním**. Klikněte na **Další**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image32.png)
32. Na hello **pokročilé možnosti** přijměte výchozí hello.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image33.png)
33. Na hello **připraven tooComplete** zkontrolujte možnosti disku hello. Klikněte na **Dokončit**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image34.png)
34. Vrátí toohello stránku vlastností virtuálního počítače. Nový pevný disk je přidána tooyour virtuálního počítače. Klikněte na **Dokončit**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image35.png)
35. S virtuální počítač vybrali v pravém podokně hello přejděte toohello **Souhrn** kartě. Zkontrolujte nastavení hello u virtuálního počítače.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image36.png)

Virtuální počítač je nyní zajištěna. dalším krokem Hello je toopower na tomto počítači a získat hello IP adresu.

## <a name="step-3-start-hello-virtual-device-and-get-hello-ip"></a>Krok 3: Spuštění hello virtuální zařízení a získat hello IP
Proveďte následující kroky toostart hello virtuální zařízení a připojit tooit.

#### <a name="toostart-hello-virtual-device"></a>toostart hello virtuálního zařízení
1. Spuštění virtuálního zařízení hello. V hello vSphere nástroje Configuration Manager, v levém podokně hello vyberte zařízení a klikněte pravým tlačítkem na toobring až hello kontextové nabídky. Vyberte **Power** a pak vyberte **zapnout**. To by měl power ve virtuálním počítači. Hello stav se zobrazí ve spodní hello **poslední úkoly** podokně hello vSphere klienta.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image37.png)
2. Hello instalační úlohy, které bude trvat několik minut toocomplete. Jakmile zařízení hello pracuje, přejděte toohello **konzoly** kartě. Odeslat Ctrl + Alt + Delete toolog toohello zařízení. Alternativně můžete bodu kurzoru hello v okně konzoly hello a stiskněte klávesu Ctrl + Alt + Insert. Výchozí uživatel Hello *StorSimpleAdmin* a hello výchozí heslo je *Heslo1*.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image38.png)
3. Z bezpečnostních důvodů vyprší platnost hesla správce zařízení hello při prvním přihlášení hello. Jste výzvami toochange hello heslo.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image39.png)
4. Zadejte heslo obsahující alespoň 8 znaků. Hello heslo musí obsahovat 3 z 4 z těchto požadavků: velká písmena, malá písmena, číselné a speciální znaky. Zadejte znovu heslo tooconfirm hello ho. Budete informováni, že došlo ke změně toto heslo hello.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image40.png)
5. Po hello heslo je úspěšně změněno, může restartování virtuálního zařízení hello. Čekejte na restartování toocomplete hello. konzole Windows PowerShell Hello hello zařízení nemusí být zobrazeny společně s indikátor průběhu.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image41.png)
6. Kroky 6 až 8 se projeví pouze při dalším spuštění v prostředí bez služby DHCP. Pokud jste v prostředí s DHCP, pak Přeskočit tyto kroky a přejít toostep 9. Pokud je spouštěn nahoru zařízení v prostředí bez služby DHCP, zobrazí se následující obrazovka hello.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image42m.png)

   V dalším kroku nakonfigurujte hello sítě.
7. Použití hello `Get-HcsIpAddress` příkaz toolist hello síťových rozhraní povolené na virtuálním zařízení. Pokud vaše zařízení má jedno síťové rozhraní povolena, je hello výchozí název přiřazený toothis rozhraní `Ethernet`.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image43m.png)
8. Použití hello `Set-HcsIpAddress` rutiny tooconfigure hello sítě. Níže je uveden příklad:

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image44.png)
9. Po dokončení počáteční nastavení hello a hello zařízení má spuštění nahoru, zobrazí se text hlavičky hello zařízení. Poznamenejte si hello IP adresu a adresu URL hello zobrazené v hello banner text toomanage hello zařízení. Použijete tuto IP adresu tooconnect toohello webového uživatelského rozhraní virtuální zařízení a dokončení hello místní instalaci a registraci.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image45.png)
10. (Volitelné) Tento krok proveďte jenom v případě, že nasazujete zařízení v hello Cloud vlády. Nyní povolíte hello Spojených států informace o zpracování Standard FIPS (Federal) režim na zařízení. standard Hello FIPS 140 definuje kryptografické algoritmy pro použití schváleno nám Federal government počítačových systémů hello ochranu citlivá data.

    1. tooenable hello režimu FIPS, spusťte následující rutinu hello:

        `Enable-HcsFIPSMode`
    2. Restartování zařízení po povolení režimu FIPS hello tak, aby ověření kryptografických hello vstoupí v platnost.

       > [!NOTE]
       > Můžete povolit nebo zakázat režim FIPS ve vašem zařízení. Střídání hello zařízení mezi režim FIPS a standardu FIPS není podporováno.
       >
       >

Pokud zařízení nesplňuje hello minimální požadavky na konfiguraci, zobrazí se chyba v text hlavičky hello (zobrazené dole). Konfigurace zařízení hello toomodify budete potřebovat tak, aby měl minimální požadavky na odpovídající zdroje toomeet hello. Potom můžete restartovat a toohello zařízení připojit. Odkazovat toohello minimální požadavky na konfiguraci v [krok 1: Zkontrolujte, zda hello hostitelský systém splňuje požadavky na minimální virtuální zařízení](#step-1-ensure-host-system-meets-minimum-virtual-device-requirements).

![](./media/storsimple-virtual-array-deploy2-provision-vmware/image46.png)

Pokud potýkáte chybě během počáteční konfigurace hello pomocí hello místního webového uživatelského rozhraní, podívejte se na toohello následující pracovních postupů:

* Spustit diagnostické testy příliš[řešení potíží s instalací webového uživatelského rozhraní](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).
* [Generovat balíček protokolu a prohlížení soubory protokolů](storsimple-ova-web-ui-admin.md#generate-a-log-package).

## <a name="next-steps"></a>Další kroky
* [Nastavení pole virtuální zařízení StorSimple jako souborový server](storsimple-virtual-array-deploy3-fs-setup.md)
* [Nastavení pole virtuální zařízení StorSimple jako serveru iSCSI](storsimple-virtual-array-deploy3-iscsi-setup.md)
