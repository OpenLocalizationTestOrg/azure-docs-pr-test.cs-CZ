---
title: "aaaConfigure funkci MPIO na hostiteli připojen tooStorSimple virtuální pole | Microsoft Docs"
description: "Popisuje, jak tooconfigure Multipath I/O (MPIO) pro pole virtuální zařízení StorSimple připojené tooa hostitele se systémem Windows Server 2012 R2."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 5b7a7f99-ee5b-4b7d-ab32-483a5a1fa504
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/01/2017
ms.author: alkohli
ms.openlocfilehash: 0e6df23bba29395329685cbf2c968675abb04cfc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-multipath-io-on-windows-server-host-for-hello-storsimple-virtual-array"></a>Konfigurace funkce Multipath I/O v hostiteli systému Windows Server pro hello pole virtuální zařízení StorSimple
## <a name="overview"></a>Přehled
Tento článek popisuje, jak použít specifické nastavení pro svazky jen StorSimple tooinstall funkci Multipath I/O (MPIO) na hostiteli služby Windows Server a potom ověřte funkce MPIO pro svazky zařízení StorSimple. Hello postupu se předpokládá, že vaše virtuální zařízení StorSimple 1200 pole se dvěma síťovými rozhraními je připojený tooa systému Windows Server hostitele se dvěma síťovými rozhraními. Hello informace obsažené v tomto článku se vztahuje pouze toohello virtuální pole. Informace o řadu zařízení StorSimple 8000 přejděte příliš[konfigurace funkce MPIO pro hostitele zařízení StorSimple](storsimple-configure-mpio-windows-server.md). 

funkci MPIO Hello v systému Windows Server pomáhá sestavení vysoce dostupné a odolné proti chybám úložiště konfigurace. Funkce MPIO používá redundantní komponenty fyzických cesty – adaptéry, kabely a přepínače – toocreate logických cest mezi serverem hello a hello úložné zařízení. Pokud dojde k selhání součásti, způsobí toofail logickou cestu, logika více cest použije alternativní cesty pro vstupy/výstupy tak, aby aplikace můžete stále přístup k datům. Kromě toho v závislosti na vaší konfiguraci funkce MPIO lze také vylepšit výkon znovu Vyrovnávání zatížení hello mezi tyto cesty. Další informace najdete v tématu [Přehled funkce MPIO](https://technet.microsoft.com/library/cc725907.aspx "MPIO přehled a funkce").

Pro hello vysoké dostupnosti vašeho řešení StorSimple konfigurujte funkci MPIO na hello připojené tooyour hostitelů Windows Server pole virtuální zařízení StorSimple (model 1200). Hello hostitele servery budou tolerovat klikněte na odkaz, sítě nebo selhání rozhraní. 

Je nutné toofollow tyto kroky tooconfigure MPIO: 

* Předpoklady konfigurace
* Krok 1: Instalace funkce MPIO na hostitelském serveru Windows hello
* Krok 2: Konfigurace funkce MPIO pro svazky zařízení StorSimple
* Krok 3: Připojení StorSimple svazky na hostiteli hello

Každý hello výše kroky je popsané v následující části hello.

## <a name="prerequisites"></a>Požadavky
Tato část podrobně hello požadavky konfigurace hostitelů Windows Server hello a vaše virtuální pole.

### <a name="on-windows-server-host"></a>Na hostitelském Windows serveru
* Ujistěte se, že váš hostitel systému Windows Server má 2 síťových rozhraní, které jsou povolené.

### <a name="on-storsimple-virtual-array"></a>V poli virtuální zařízení StorSimple
* pole virtuálním Hello by měl nakonfigurovaný jako server se službou iSCSI. Další, najdete v části toolearn [nastavit virtuální pole jako server se službou iSCSI](storsimple-virtual-array-deploy3-iscsi-setup.md). Jeden nebo víc síťových rozhraní by měla být povolená na pole hello.   
* Hello síťová rozhraní virtuálních pole by měly být dostupné z hostitele serveru Windows hello.
* Na virtuální zařízení StorSimple pole by se vytvořit jeden nebo více svazků. Další, najdete v části toolearn [přidat svazek](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume) na pole virtuální zařízení StorSimple. V tomto postupu jsme vytvořili svazky 3 (1 místně vázaný a 2 vrstvené svazky, jak je uvedené níže) v poli virtuálním hello.
  
    ![mpio0](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio0.png)

### <a name="hardware-configuration-for-storsimple-virtual-array"></a>Konfigurace hardwaru pro pole virtuální zařízení StorSimple
Hello na následujícím obrázku hello hardwarová konfigurace pro vysokou dostupnost a vyrovnávání zatížení více cest pro hostitele Windows serveru a pole virtuální zařízení StorSimple použít v tomto postupu.

![Konfigurace funkce MPIO hardwaru](./media/storsimple-virtual-array-configure-mpio-windows-server/1200hardwareconfig.png)

Jak je znázorněno v předchozích obrázek hello:

* Pole virtuální zařízení StorSimple zřídit na Hyper-V je zařízením active jeden uzel, který je nakonfigurovaný jako server se službou iSCSI.
* Na pole jsou povolené dvě rozhraní virtuální sítě. V hello místního webového uživatelského rozhraní vaše virtuální pole, ověřte, že jsou povolené dvě rozhraní sítě tak, že přejdete příliš**nastavení sítě** jak je uvedeno níže:
  
    ![Síťová rozhraní zapnuta 1200](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio9.png)
  
    Poznámka: hello IPv4 adresy hello povoleno síťových rozhraní (Ethernet, Ethernet 2 ve výchozím nastavení) a uložit pro pozdější použití na hostiteli hello.
* Dvě síťová rozhraní jsou povolené na hostiteli s Windows Server. Pokud hello připojené rozhraní pro hostitele a pole jsou na hello stejné podsíti, pak bude 4 cesty k dispozici. To se hello případu v tomto postupu. Ale pokud každé síťové rozhraní na hello pole a hostitele rozhraní jsou v jiné podsíti protokolu IP (a ne směrovatelné), pak pouze 2 cesty nebudou k dispozici.

## <a name="step-1-install-mpio-on-hello-windows-server-host"></a>Krok 1: Instalace funkce MPIO na hostitelském serveru Windows hello
Funkce MPIO je volitelná funkce v systému Windows Server a není nainstalována ve výchozím nastavení. Je nutné ji nainstalovat jako funkci pomocí Správce serveru. Dokončete tuto funkci na hostiteli služby Windows Server tooinstall hello následující postup.

[!INCLUDE [storsimple-install-mpio-windows-server-host](../../includes/storsimple-install-mpio-windows-server-host.md)]

## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>Krok 2: Konfigurace funkce MPIO pro svazky zařízení StorSimple
Funkce MPIO musí svazky zařízení StorSimple tooidentify toobe nakonfigurované. svazky zařízení StorSimple toorecognize tooconfigure funkce MPIO, proveďte následující kroky hello.

[!INCLUDE [storsimple-configure-mpio-volumes](../../includes/storsimple-configure-mpio-volumes.md)]

## <a name="step-3-mount-storsimple-volumes-on-hello-host"></a>Krok 3: Připojení StorSimple svazky na hostiteli hello
Po dokončení konfigurace funkce MPIO v systému Windows Server, svazky, které jsou vytvořené v poli StorSimple hello může být připojen a pak můžete využít výhod funkce MPIO pro redundanci. toomount svazek, proveďte následující kroky hello.

#### <a name="toomount-volumes-on-hello-host"></a>toomount svazky na hostiteli hello
1. Otevřete hello **vlastnosti iniciátoru iSCSI** okno na hostitelském serveru Windows hello. Přejděte příliš**správce serveru > řídicí panel > Nástroje > iniciátor iSCSI**.
2. V hello **vlastnosti iniciátoru iSCSI** dialogové okno, klikněte na tlačítko **zjišťování**a potom klikněte na **zjistit cílový portál**.
3. V hello **zjistit cílový portál** dialogové okno pole, hello následující:
   
    1. Zadejte IP adresu hello hello první povolené síťové rozhraní na pole virtuální zařízení StorSimple. Ve výchozím nastavení, bude **Ethernet**. 
    2. Klikněte na tlačítko **OK** tooreturn toohello **vlastnosti iniciátoru iSCSI** dialogové okno.
     
    > [!IMPORTANT]
    > Pokud používáte privátní sítě pro připojení iSCSI, zadejte IP adresu hello hello datový port, který je připojený toohello privátní sítě.
     
4. Opakujte kroky 2 až 3 pro druhý rozhraní sítě (například Ethernet 2) na pole. 
5. Vyberte hello **cíle** ve hello **vlastnosti iniciátoru iSCSI** dialogové okno. Pro vaše virtuální pole, měli byste vidět prostor pro každý svazek jako cíl v části **zjištěné cíle**. V takovém případě by být zjištěny tři cíle (odpovídající toothree svazky).
   
    ![mpio1](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio1.png)
6. Klikněte na tlačítko **připojit** tooestablish na relace iSCSI s pole virtuální zařízení StorSimple. A **připojit tooTarget** se zobrazí dialogové okno. Vyberte hello **povolit více cestami** zaškrtávací políčko. Klikněte na tlačítko **rozšířené**.
   
    ![mpio2](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio2.png)
7. V hello **Upřesnit nastavení** dialogové okno pole, hello následující:                                        
   
    1. Na hello **místní adaptér** rozevíracího seznamu vyberte **iniciátor iSCSI společnosti Microsoft**.
    2. Na hello **iniciátor IP** rozevíracího seznamu, vyberte hello IP adresu hostitele hello.
    3. Na hello **cílový portál** IP rozevíracího seznamu vyberte hello IP rozhraní pole.
    4. Klikněte na tlačítko **OK** tooreturn toohello **vlastnosti iniciátoru iSCSI** dialogové okno.
     
        ![mpio3](./media/storsimple-ova-configure-mpio-windows-server/mpio3.png)

8. Klikněte na **Vlastnosti**. 
   
    ![mpio4](./media/storsimple-ova-configure-mpio-windows-server/mpio4.png)

9. V hello **vlastnosti** dialogové okno, klikněte na tlačítko **Přidat relaci**.
   
   ![mpio5](./media/storsimple-ova-configure-mpio-windows-server/mpio5.png)
10. V hello **připojit tooTarget** dialogové okno, vyberte hello **povolit více cestami** zaškrtávací políčko. Klikněte na tlačítko **rozšířené**.
11. V hello **Upřesnit nastavení** dialogové okno:                                        
    
    1. Na hello **místní adaptér** rozevíracího seznamu vyberte iniciátor iSCSI společnosti Microsoft.

    2. Na hello **iniciátor IP** rozevíracího seznamu, vyberte hello IP adresu odpovídající toohello hostitele. V takovém případě se připojujete dvě síťová rozhraní hello pole tooa jedno síťové rozhraní na hostiteli hello. Toto rozhraní je hello proto stejný jako příkaz, který poskytuje pro hello první relaci.

    3. Na hello **portál cílová IP adresa** rozevíracího seznamu, vyberte hello IP adresu pro druhý rozhraní dat hello zapnuta hello pole.

    4. Klikněte na tlačítko **OK** dialogové okno iSCSI Initiator vlastnosti tooreturn toohello. Jste přidali na druhý toohello cíl relace.
      
       ![mpio11](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio11.png)
    
    5. Po přidání požadovaných hello relací (cest), v hello **vlastnosti iniciátoru iSCSI** dialogové okno, vyberte hello cíle a klikněte na tlačítko **vlastnosti**. Na kartě relací hello hello **vlastnosti** dialogové okno, Poznámka hello čtyři relace identifikátory, které odpovídají toohello možné cesta permutací. toocancel relaci, vyberte identifikátor další tooa relace hello zaškrtávací políčko a potom klikněte na **odpojení**.

    6. tooview zařízení uvedené v rámci relace, vyberte hello **zařízení** klikněte na kartu tooconfigure hello zásady funkce MPIO pro vybrané zařízení **MPIO**.

    7. Hello **podrobnosti** se zobrazí dialogové okno. Na hello **MPIO** kartě můžete vybrat odpovídající hello **zásad vyrovnávání zatížení** nastavení. Můžete také zobrazit hello **Active** nebo **pohotovostní** typ cesty.
12. Opakováním kroků 8 až 11 tooadd další relace (cest) toohello cíl. Dvě rozhraní na hostiteli hello a dva na hello virtuální pole můžete přidat celkem čtyři relace pro každý cíl. 
    
    ![mpio14](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio14.png)
13. Budete potřebovat toorepeat tyto kroky pro každý svazek (povrchy jako cíl).
    
    ![mpio15](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio15.png)
14. Otevřete **Správa počítače** přechodem příliš**správce serveru > řídicí panel > Správa počítače**. V levém podokně hello, klikněte na **úložiště > Správa disků**. Hello na hello pole virtuální zařízení StorSimple vytvořit svazky, které jsou viditelné toothis hostitele se zobrazí v části **Správa disků** jako nové disky.
15. Inicializovat hello disk a vytvořte nový svazek. Během procesu hello formátu vyberte velikost alokační jednotky (Austrálie) 64 kB. Hello postup opakujte u všech dostupných svazků hello.
    
    ![Správa disků](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio20.png)
16. V části **Správa disků**, klikněte pravým tlačítkem na hello **disku** a vyberte **vlastnosti**.
17. V hello **vlastnosti zařízení s více cestami disku** dialogovém okně klikněte na hello **funkce MPIO** kartě.
    
    ![Vlastnosti disku](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio21.png)
18. V hello **DSM název** klikněte na tlačítko **podrobnosti** a ověřte, zda text hello parametry jsou nastavena toohello výchozí parametry. Hello výchozí parametry jsou:
    
    * Cestu ověřte období = 30
    * Počet opakování = 3
    * PDO odebrat období = 20
    * Interval opakování = 1
    * Ověřte cestu povolené = není zaškrtnuto.
    
    > [!NOTE]
    > **Neměňte výchozí parametry hello.**
   
## <a name="next-steps"></a>Další kroky
Další informace o [pomocí hello tooadminister služby StorSimple Manager zařízení StorSimple virtuální pole](storsimple-virtual-array-manager-service-administration.md).

