---
title: "aaaConfigure funkce MPIO pro zařízení StorSimple | Microsoft Docs"
description: "Popisuje, jak tooconfigure Multipath I/O (MPIO) pro zařízení StorSimple připojené tooa hostitele se systémem Windows Server 2012 R2."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/05/2017
ms.author: alkohli
ms.openlocfilehash: 7796524edc739826ba1e977161fc9988c6d316e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-multipath-io-for-your-storsimple-device"></a>Konfigurace funkce Multipath I/O pro zařízení StorSimple

Tento kurz popisuje kroky hello by měl postupujte podle tooinstall a použijte funkci Multipath I/O (MPIO) hello na hostitele se systémem Windows Server 2012 R2 a připojené tooa fyzického zařízení StorSimple. Hello pokyny v tomto článku se vztahuje tooStorSimple 8000 řady fyzické jenom zařízení. Funkce MPIO není aktuálně podporováno v zařízení s StorSimple cloudu.

Vysoce dostupné a odolné proti chybám sítě SAN konfigurace sestavení vytvořené podpora funkce Multipath I/O (MPIO) hello v systému Windows Server toohelp společnosti Microsoft. Funkce MPIO používá redundantní komponenty fyzických cesty – adaptéry, kabely a přepínače – toocreate logických cest mezi serverem hello a hello úložné zařízení. Pokud dojde k selhání součásti, způsobí toofail logickou cestu, logika více cest použije alternativní cesty pro vstupy/výstupy tak, aby aplikace můžete stále přístup k datům. Kromě toho v závislosti na vaší konfiguraci funkce MPIO lze také vylepšit výkon vyrovnává zatížení hello mezi tyto cesty. Další informace najdete v tématu [Přehled funkce MPIO](https://technet.microsoft.com/library/cc725907.aspx "MPIO přehled a funkce").

Funkce MPIO na zařízení StorSimple by měl být nakonfigurovaný pro hello vysoké dostupnosti vašeho řešení StorSimple. Při instalaci funkce MPIO na hostitelských serverech s Windows serverem 2012 R2 hello servery budou tolerovat klikněte na odkaz, sítě nebo selhání rozhraní.

## <a name="mpio-configuration-summary"></a>Souhrn konfigurace funkce MPIO

Funkce MPIO je volitelná funkce v systému Windows Server a není nainstalována ve výchozím nastavení. Je nutné ji nainstalovat jako funkci pomocí Správce serveru.

Postupujte podle těchto kroků tooconfigure funkci MPIO na zařízení StorSimple:

* Krok 1: Instalace funkce MPIO na hostitelském serveru Windows hello
* Krok 2: Konfigurace funkce MPIO pro svazky zařízení StorSimple
* Krok 3: Připojení StorSimple svazky na hostiteli hello
* Krok 4: Konfigurace funkce MPIO pro zajištění vysoké dostupnosti a vyrovnávání zatížení

Každý z předchozích kroků hello je popsané v následující části hello.

## <a name="step-1-install-mpio-on-hello-windows-server-host"></a>Krok 1: Instalace funkce MPIO na hostitelském serveru Windows hello

Dokončete tuto funkci na hostiteli služby Windows Server tooinstall hello následující postup.

#### <a name="tooinstall-mpio-on-hello-host"></a>tooinstall funkci MPIO na hostiteli hello

1. Otevřete správce serveru na hostiteli s Windows Server. Ve výchozím nastavení správce serveru spustí, když se člen skupiny Administrators hello přihlásí tooa počítače se systémem Windows Server 2012 R2 nebo Windows Server 2012. Pokud hello správce serveru už není otevřený, klikněte na tlačítko **Start > Správce serveru**.
   
   ![Správce serveru](./media/storsimple-configure-mpio-windows-server/IC740997.png)

2. Klikněte na tlačítko **správce serveru > řídicí panel > Přidat role a funkce**. Tím se spustí hello **přidat role a funkce** průvodce.
   
   ![Přidat role a funkce Průvodce 1](./media/storsimple-configure-mpio-windows-server/IC740998.png)
3. V hello **přidat role a funkce** průvodce proveďte následující kroky hello:
   
   1. Na hello **před zahájením** klikněte na tlačítko **Další**.
   2. Na hello **vybrat typ instalace** přijměte výchozí nastavení hello **na základě rolí nebo na základě funkcí** instalace. Klikněte na **Další**.
   
       ![Přidat role a funkce Průvodce 2](./media/storsimple-configure-mpio-windows-server/IC740999.png)
   3. Na hello **vybrat cílový server** vyberte **vyberte server z fondu serverů hello**. Hostitelský server by měly být zjištěny automaticky. Klikněte na **Další**.
   4. Na hello **vybrat role serveru** klikněte na tlačítko **Další**.
   5. Na hello **vyberte funkce** vyberte **funkce Multipath I/O**a klikněte na tlačítko **Další**.
   
       ![Přidat role a funkce Průvodce 5](./media/storsimple-configure-mpio-windows-server/IC741000.png)
   6. Na hello **potvrdit vybrané možnosti instalace** potvrďte výběr hello a potom vyberte **restartování hello cílový server automaticky podle potřeby**, jak je uvedeno níže. Klikněte na **Nainstalovat**.
   
       ![Přidat role a funkce Průvodce 8](./media/storsimple-configure-mpio-windows-server/IC741001.png)
   7. Upozornění se zobrazí po dokončení instalace hello. Klikněte na tlačítko **Zavřít** tooclose hello průvodce.
   
       ![Přidat role a funkce Průvodce 9](./media/storsimple-configure-mpio-windows-server/IC741002.png)

## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>Krok 2: Konfigurace funkce MPIO pro svazky zařízení StorSimple

Funkce MPIO musí být nakonfigurované tooidentify svazky zařízení StorSimple. svazky zařízení StorSimple toorecognize tooconfigure funkce MPIO, proveďte následující kroky hello.

#### <a name="tooconfigure-mpio-for-storsimple-volumes"></a>tooconfigure funkce MPIO pro svazky zařízení StorSimple

1. Otevřete hello **konfigurace funkce MPIO**. Klikněte na tlačítko **správce serveru > řídicí panel > Nástroje > funkce MPIO**.
2. V hello **MPIO vlastnosti** dialogové okno, vyberte hello **zjistit více cest** kartě.
3. Vyberte **přidat podporu pro zařízení iSCSI**a potom klikněte na **přidat**.  
   ![Funkce MPIO vlastnosti zjistit více cest](./media/storsimple-configure-mpio-windows-server/IC741003.png)
4. Restartujte server hello po zobrazení výzvy.
5. V hello **MPIO vlastnosti** dialogové okno pole, klikněte na tlačítko hello **MPIO zařízení** kartě. Klikněte na tlačítko **Přidat**.
    </br>![Funkce MPIO vlastnosti MPIO zařízení](./media/storsimple-configure-mpio-windows-server/IC741004.png)
6. V hello **přidat podporu funkce MPIO** dialogovém **ID hardwaru zařízení**, zadejte sériové číslo zařízení. tooget hello sériové číslo, přístup k zařízení služby StorSimple Manager zařízení. Přejděte příliš**zařízení > řídicí panel**. sériové číslo zařízení Hello se zobrazí v pravém hello **rychlý přehled** podokně řídicí panel hello zařízení.
    </br>
    ![Přidání podpory funkce MPIO](./media/storsimple-configure-mpio-windows-server/IC741005.png)
7. Restartujte server hello po zobrazení výzvy.

## <a name="step-3-mount-storsimple-volumes-on-hello-host"></a>Krok 3: Připojení StorSimple svazky na hostiteli hello

Po dokončení konfigurace funkce MPIO v systému Windows Server, na zařízení StorSimple hello vytvořit svazky může být připojen a pak můžete využít výhod funkce MPIO pro redundanci. toomount svazek, proveďte následující kroky hello.

#### <a name="toomount-volumes-on-hello-host"></a>toomount svazky na hostiteli hello

1. Otevřete hello **vlastnosti iniciátoru iSCSI** okno na hostitelském serveru Windows hello. Klikněte na tlačítko **správce serveru > řídicí panel > Nástroje > iniciátor iSCSI**.
2. V hello **vlastnosti iniciátoru iSCSI** dialogové okno, klikněte na kartu hello zjišťování a pak klikněte na tlačítko **zjistit cílový portál**.
3. V hello **zjistit cílový portál** dialogové okno pole, proveďte následující kroky hello:
   
   1. Zadejte IP adresu hello hello datový port zařízení StorSimple (například zadejte DATA 0).
   2. Klikněte na tlačítko **OK** tooreturn toohello **vlastnosti iniciátoru iSCSI** dialogové okno.
     
     > [!IMPORTANT]
     > **Pokud používáte privátní sítě pro připojení iSCSI, zadejte IP adresu hello hello datový port, který je připojený toohello privátní sítě.**
    
4. Opakujte kroky 2 až 3 pro druhý rozhraní sítě (například DATA 1) na zařízení. Uvědomte si, že tato rozhraní mají být povoleny pro iSCSI. Další informace najdete v tématu [upravit síťových rozhraní](storsimple-8000-modify-device-config.md#modify-network-interfaces).
5. Vyberte hello **cíle** ve hello **vlastnosti iniciátoru iSCSI** dialogové okno. Měli byste vidět zařízení StorSimple hello cíle IQN pod **zjištěné cíle**.

   ![Karta cíle vlastnosti iniciátoru iSCSI](./media/storsimple-configure-mpio-windows-server/IC741007.png)
   
6. Klikněte na tlačítko **připojit** tooestablish na relace iSCSI s vaším zařízením StorSimple. A **připojit tooTarget** zobrazí se dialogové okno.
7. V hello **připojit tooTarget** dialogové okno, vyberte hello **povolit více cestami** zaškrtávací políčko. Klikněte na tlačítko **rozšířené**.
8. V hello **Upřesnit nastavení** dialogové okno pole, proveďte následující kroky hello:
   
   1. Na hello **místní adaptér** rozevíracího seznamu vyberte **iniciátor iSCSI společnosti Microsoft**.
   2. Na hello **iniciátor IP** rozevíracího seznamu, vyberte hello IP adresu hostitele hello.
   3. Na hello **cílový portál** IP rozevíracího seznamu vyberte hello IP rozhraní zařízení.
   4. Klikněte na tlačítko **OK** tooreturn toohello **vlastnosti iniciátoru iSCSI** dialogové okno.
9. Klikněte na **Vlastnosti**. V hello **vlastnosti** dialogové okno, klikněte na tlačítko **Přidat relaci**.
10. V hello **připojit tooTarget** dialogové okno, vyberte hello **povolit více cestami** zaškrtávací políčko. Klikněte na tlačítko **rozšířené**.
11. V hello **Upřesnit nastavení** dialogové okno:

    1. Na hello **místní adaptér** rozevíracího seznamu vyberte iniciátor iSCSI společnosti Microsoft.
    2. Na hello **iniciátor IP** rozevíracího seznamu, vyberte hello IP adresu odpovídající toohello hostitele. V takovém případě se připojujete dvě síťová rozhraní hello zařízení tooa jedno síťové rozhraní na hostiteli hello. Toto rozhraní je hello proto stejný jako příkaz, který poskytuje pro hello první relaci.
    3. Na hello **portál cílová IP adresa** rozevíracího seznamu, vyberte hello IP adresu pro druhý rozhraní dat hello zapnuta hello zařízení.
    4. Klikněte na tlačítko **OK** dialogové okno iSCSI Initiator vlastnosti tooreturn toohello. Jste přidali na druhý toohello cíl relace.
12. Otevřete **Správa počítače** přechodem příliš**správce serveru > řídicí panel > Správa počítače**. V levém podokně hello, klikněte na **úložiště > Správa disků**. na zařízení StorSimple hello, které jsou viditelné toothis hostitele vytvořit svazek Hello se zobrazí pod **Správa disků** jako nové disky.
13. Inicializovat hello disk a vytvořte nový svazek. Během procesu hello formátu vyberte velikost bloku 64 KB.
    
    ![Správa disků](./media/storsimple-configure-mpio-windows-server/IC741008.png)
14. V části **Správa disků**, klikněte pravým tlačítkem na hello **disku** a vyberte **vlastnosti**.
15. V hello StorSimple modelu ### **vlastnosti zařízení s více cestami disku** dialogové okno pole, klikněte na tlačítko hello **MPIO** kartě.
    
    ![Disk s více cestami DeviceProp StorSimple 8100.](./media/storsimple-configure-mpio-windows-server/IC741009.png)
16. V hello **DSM název** klikněte na tlačítko **podrobnosti** a ověřte, zda text hello parametry jsou nastavena toohello výchozí parametry. Hello výchozí parametry jsou:
    
    * Cestu ověřte období = 30
    * Počet opakování = 3
    * PDO odebrat období = 20
    * Interval opakování = 1
    * Ověřte cestu povolené = není zaškrtnuto.

> [!NOTE]
> **Neměňte výchozí parametry hello.**


## <a name="step-4-configure-mpio-for-high-availability-and-load-balancing"></a>Krok 4: Konfigurace funkce MPIO pro zajištění vysoké dostupnosti a vyrovnávání zatížení

S více cestami na základě vysokou dostupnost a vyrovnávání zatížení a více relací musí ručně přidat toodeclare hello různé cesty k dispozici. Například pokud má hostitel hello dvě rozhraní, které jsou připojené tooSAN a hello zařízení má dvě rozhraní připojených tooSAN, pak je nutné nakonfigurovat pomocí správné cesty permutací (pouze dvě relace se bude vyžadovat Pokud je každé rozhraní dat a hostitele rozhraní čtyři relací v jiné podsíti protokolu IP a není směrovatelné).

**Doporučujeme, abyste měli aspoň 8 paralelních aktivních relací mezi hello zařízení a hostitele vaší aplikace.** Toho lze dosáhnout povolením 4 rozhraní sítě systému Windows Server. Použití fyzická síťová rozhraní nebo virtuální rozhraní pomocí technologie virtualizace sítě na úrovni hello hardwaru nebo operačního systému na hostiteli s Windows Server. S hello dvě síťová rozhraní na hello zařízení tato konfigurace by způsobilo 8 aktivních relací. Tato konfigurace pomáhá optimalizovat propustnost hello zařízení a cloudu.

> [!IMPORTANT]
> **Doporučujeme vám, že nemíchat 1 GbE a 10 GbE síťových rozhraní. Pokud používáte dvě síťová rozhraní, obě rozhraní musí být identické typu hello.**

Hello následující postup popisuje, jak tooadd relace, pokud je zařízení StorSimple se dvěma síťovými rozhraními připojené tooa hostitele se dvěma síťovými rozhraními. To vám dává jenom 4 relací. Použijte stejný postup s zařízení StorSimple pomocí dvou síťových rozhraní připojených tooa hostitele s čtyři síťových rozhraní. Je nutné tooconfigure 8 namísto hello 4 relací, které jsou zde popsané.

### <a name="tooconfigure-mpio-for-high-availability-and-load-balancing"></a>tooconfigure funkce MPIO pro vysokou dostupnost a vyrovnávání zatížení

1. Provedení zjišťování cíle hello: v hello **vlastnosti iniciátoru iSCSI** dialogové okno, v hello **zjišťování** , klikněte na **vyhledat portál**.
2. V hello **připojit tooTarget** dialogové okno pole, zadejte IP adresu hello jednoho hello zařízení síťových rozhraní.
3. Klikněte na tlačítko **OK** tooreturn toohello **vlastnosti iniciátoru iSCSI** dialogové okno.
4. V hello **vlastnosti iniciátoru iSCSI** dialogové okno, vyberte hello **cíle** , zvýrazněte hello zjištěné cíl a pak klikněte **Connect**. Hello **připojit tooTarget** zobrazí se dialogové okno.
5. V hello **připojit tooTarget** dialogové okno:
   
   1. Ponechejte výchozí hello vybrané nastavení cíl pro **přidat toto připojení** toohello seznam oblíbených cílů. Díky tomu hello zařízení automaticky pokusí toorestart hello připojení pokaždé, když se tento počítač restartuje.
   2. Vyberte hello **povolit více cestami** zaškrtávací políčko.
   3. Klikněte na tlačítko **rozšířené**.
6. V hello **Upřesnit nastavení** dialogové okno:
   
   1. Na hello **místní adaptér** rozevíracího seznamu vyberte **iniciátor iSCSI společnosti Microsoft**.
   2. Na hello **iniciátor IP** rozevíracího seznamu, vyberte hello IP adresu hostitele hello.
   3. Na hello **portál cílová IP adresa** rozevíracího seznamu, vyberte hello IP adresu rozhraní data hello zapnuta hello zařízení.
   4. Klikněte na tlačítko **OK** dialogové okno iSCSI Initiator vlastnosti tooreturn toohello.
7. Klikněte na tlačítko **vlastnosti**a v hello **vlastnosti** dialogové okno, klikněte na tlačítko **Přidat relaci**.
8. V hello **připojit tooTarget** dialogové okno, vyberte hello **povolit více cestami** zaškrtněte políčko a potom klikněte na **Upřesnit**.
9. V hello **Upřesnit nastavení** dialogové okno:
   
   1. Na hello **místní adaptér** rozevíracího seznamu vyberte **iniciátor iSCSI společnosti Microsoft**.
   2. Na hello **iniciátor IP** rozevíracího seznamu, vyberte hello IP adresu odpovídající toohello druhý rozhraní na hostiteli hello.
   3. Na hello **portál cílová IP adresa** rozevíracího seznamu, vyberte hello IP adresu pro druhý rozhraní dat hello zapnuta hello zařízení.
   4. Klikněte na tlačítko **OK** tooreturn toohello **vlastnosti iniciátoru iSCSI** dialogové okno. Nyní jste přidali na druhý toohello cíl relace.
10. Opakujte kroky 8-10 tooadd další relace (cest) toohello cíl. Dvě rozhraní na hostiteli hello a dva na hello zařízení můžete přidat celkem čtyři relací.
11. Po přidání požadovaných hello relací (cest), v hello **vlastnosti iniciátoru iSCSI** dialogové okno, vyberte hello cíle a klikněte na tlačítko **vlastnosti**. Na kartě relací hello hello **vlastnosti** dialogové okno, Poznámka hello čtyři relace identifikátory, které odpovídají toohello možné cesta permutací. toocancel relaci, vyberte identifikátor další tooa relace hello zaškrtávací políčko a potom klikněte na **odpojení**.
12. tooview zařízení uvedené v rámci relace, vyberte hello **zařízení** klikněte na kartu tooconfigure hello zásady funkce MPIO pro vybrané zařízení **MPIO**. Hello **podrobnosti o zařízení** zobrazí se dialogové okno. Na hello **MPIO** kartě můžete vybrat odpovídající hello **zásad vyrovnávání zatížení** nastavení. Můžete také zobrazit hello **Active** nebo **pohotovostní** typ cesty.

## <a name="next-steps"></a>Další kroky

Další informace o [pomocí hello toomodify služby StorSimple Manager zařízení StorSimple konfiguraci zařízení](storsimple-8000-modify-device-config.md).

