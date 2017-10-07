---
title: "Instalační program serveru aaaMicrosoft Azure StorSimple virtuální pole iSCSI | Microsoft Docs"
description: "Popisuje, jak tooperform počátečním nastavení registrace zařízení StorSimple serveru iSCSI a dokončení instalace zařízení."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 4db116d1-978b-48e8-b572-a719a8425dbc
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.openlocfilehash: b4ff6391cb2af69d4e83dcdac5e027f8498005b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-storsimple-virtual-array--set-up-as-an-iscsi-server-via-azure-portal"></a>Nasadit pole virtuálního zařízení StorSimple – sadu až jako server se službou iSCSI prostřednictvím portálu Azure

![tok procesu instalace iSCSI](./media/storsimple-virtual-array-deploy3-iscsi-setup/iscsi4.png)

## <a name="overview"></a>Přehled

V tomto kurzu nasazení použije toohello Microsoft Azure StorSimple virtuální pole. Tento kurz popisuje, jak tooperform hello počáteční nastavení, registrace zařízení StorSimple iSCSI serveru, nastavení zařízení dokončení hello a pak vytvořit, připojení, inicializace a formátování svazků na pole virtuální zařízení StorSimple nakonfigurovaný jako server se službou iSCSI. 

Hello postupů popsaných v tomto poli trvat přibližně 30 minut too1 hodinu toocomplete. Hello informace publikované v tomto článku se vztahují pouze tooStorSimple virtuální pole.

## <a name="setup-prerequisites"></a>Požadavky instalačního programu

Než začnete konfigurovat a nastavit pole virtuální zařízení StorSimple, ujistěte se, že:

* Zřízení virtuální pole a připojené tooit, jak je popsáno v [nasazení zařízení StorSimple virtuální pole - zřídit virtuální pole Hyper-v](storsimple-ova-deploy2-provision-hyperv.md) nebo [nasazení zařízení StorSimple virtuální pole - zřídit virtuální pole v prostředí VMware ](storsimple-virtual-array-deploy2-provision-vmware.md).
* Máte registrační klíč služby hello z hello Správce zařízení StorSimple služby, které jste vytvořili toomanage vaše pole virtuální zařízení StorSimple. Další informace najdete v tématu **krok 2: registrační klíč služby hello Get** v [nasazení zařízení StorSimple virtuální pole - Příprava portálu hello](storsimple-virtual-array-deploy1-portal-prep.md#step-2-get-the-service-registration-key).
* Pokud je to hello druhém a dalším virtuální pole, které jsou registraci s existující službu Správce zařízení StorSimple, měli byste mít šifrovacího klíče dat služby hello. Tento klíč byl vygenerován při hello první zařízení byl úspěšně registrován s touto službou. Pokud jste ztratili tento klíč, přečtěte si téma **šifrovacího klíče dat služby Get hello** v [použití hello webového uživatelského rozhraní tooadminister pole virtuální zařízení StorSimple](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key).

## <a name="step-by-step-setup"></a>Krok za krokem instalace

Použijte hello následující podrobné pokyny tooset a nakonfigurovat virtuální zařízení StorSimple pole:

* [Krok 1: Dokončení hello místní webový instalační program uživatelského rozhraní a zaregistrovat zařízení](#step-1-complete-the-local-web-ui-setup-and-register-your-device)
* [Krok 2: Dokončení hello požadované nastavení zařízení](#step-2-complete-the-required-device-setup)
* [Krok 3: Přidání svazku](#step-3-add-a-volume)
* [Krok 4: Připojení, inicializace a formátování svazků](#step-4-mount-initialize-and-format-a-volume)

## <a name="step-1-complete-hello-local-web-ui-setup-and-register-your-device"></a>Krok 1: Dokončení hello místní webový instalační program uživatelského rozhraní a zaregistrovat zařízení

#### <a name="toocomplete-hello-setup-and-register-hello-device"></a>toocomplete hello instalace a registrace zařízení hello

1. Otevřete okno prohlížeče. tooconnect toohello webového uživatelského rozhraní typu:
   
    `https://<ip-address of network interface>`
   
    Použijte adresu URL připojení hello si poznamenali v předchozím kroku hello. Zobrazí se chyba oznamující, že došlo k potížím s certifikátem zabezpečení webu hello. Klikněte na tlačítko **pokračovat toothis webové stránky**.
   
    ![Chyba certifikátu zabezpečení](./media/storsimple-virtual-array-deploy3-iscsi-setup/image3.png)
2. Přihlášení toohello webové uživatelské rozhraní virtuálního zařízení jako **StorSimpleAdmin**. Zadejte heslo správce zařízení hello, který jste změnili v kroku 3: spuštění hello virtuálního zařízení v [nasazení zařízení StorSimple virtuální pole - zřídit virtuální zařízení technologie Hyper-V](storsimple-virtual-array-deploy2-provision-hyperv.md) nebo [nasazení zařízení StorSimple virtuální pole - Zřídit virtuální zařízení v prostředí VMware](storsimple-virtual-array-deploy2-provision-vmware.md).
   
    ![Přihlašovací stránka](./media/storsimple-virtual-array-deploy3-iscsi-setup/image4.png)
3. Budete přesměrováni toohello **Domů** stránky. Tato stránka popisuje hello různá nastavení požadované tooconfigure a registrace virtuálního zařízení StorSimple Manager zařízení službě hello hello. Všimněte si, že hello **nastavení síťového**, **webové nastavení proxy serveru**, a **nastavení času** jsou volitelné. Hello pouze požadované nastavení **nastavení zařízení** a **nastavení cloudu**.
   
    ![Domovská stránka](./media/storsimple-virtual-array-deploy3-iscsi-setup/image5.png)
4. Na hello **nastavení síťového** v části **síťových rozhraní**, bude pro vás automaticky nakonfigurován DATA 0. Každé síťové rozhraní je nastavena podle tooget výchozí IP adresu automaticky (DHCP). Proto adresu IP adresy, podsítě a bránu bude automaticky přiřazen (pro IPv4 i IPv6).
   
    Při plánování toodeploy zařízení jako server iSCSI (tooprovision blokové úložiště), doporučujeme zakázat hello **získat IP adresu automaticky** řádku a nakonfigurovat statické IP adresy.
   
    ![Stránka nastavení sítě](./media/storsimple-virtual-array-deploy3-iscsi-setup/image6.png)
   
    Pokud jste přidali více než jedno síťové rozhraní během zřizování hello hello zařízení, můžete je konfigurovat tady. Všimněte si, že síťové rozhraní můžete nakonfigurovat jako IPv4 pouze nebo jako oba protokoly IPv4 a IPv6. Nejsou podporovány pouze konfigurace IPv6.
5. Servery DNS jsou požadované, protože se používají při zařízení pokusí toocommunicate s poskytovatele cloudových služeb úložiště nebo tooresolve zařízení podle názvu Pokud je nakonfigurovaný jako souborový server. Na hello **nastavení síťového** stránky v rámci hello **servery DNS**:
   
   1. Primární a sekundární server DNS automaticky nakonfigurují. Pokud si zvolíte tooconfigure statické IP adresy, můžete zadat servery DNS. Pro vysokou dostupnost doporučujeme nakonfigurovat primární a sekundární server DNS.
   2. Klikněte na tlačítko **Použít**. Použije a ověřte nastavení sítě hello.
6. Na hello **nastavení zařízení** stránky:
   
   1. Přiřadit jedinečný **název** tooyour zařízení. Tento název může obsahovat 1 až 15 znaků a může obsahovat písmena, číslice a pomlčky.
   2. Klikněte na tlačítko hello **serveru iSCSI** ikonu ![ikona serveru iSCSI](./media/storsimple-virtual-array-deploy3-iscsi-setup/image7.png) pro hello **typu** zařízení, které vytváříte. Server se službou iSCSI vám umožní tooprovision blokové úložiště.
   3. Zadejte, pokud chcete tento toobe zařízení připojených k doméně. Pokud je vaše zařízení serveru iSCSI, pak připojení doméně hello je volitelné. Pokud se rozhodnete toonot připojení k vaší doméně tooa serveru iSCSI, klikněte na tlačítko **použít**, počkejte toobe hello nastavení použít a potom toohello další krok přeskočit.
      
       Pokud chcete, aby toojoin hello zařízení tooa domény. Zadejte **název domény**a potom klikněte na **použít**.
      
      > [!NOTE]
      > Pokud připojení vaší doméně tooa serveru iSCSI, zajistěte, aby byl virtuální pole ve vlastní organizační jednotku (OU) pro Microsoft Azure Active Directory a žádné objekty zásad skupiny (GPO) jsou použité tooit.
      > 
      > 
   4. Zobrazí se dialogové okno. Zadejte přihlašovací údaje domény v zadaném formátu hello. Klikněte na ikonu zaškrtnutí hello ![ikona zaškrtnutí](./media/storsimple-virtual-array-deploy3-iscsi-setup/image15.png). přihlašovací údaje do domény Hello se ověří. Zobrazí se chybová zpráva a pokud hello přihlašovací údaje jsou nesprávné.
      
       ![přihlašovací údaje](./media/storsimple-virtual-array-deploy3-iscsi-setup/image8.png)
   5. Klikněte na tlačítko **Použít**. Použije a ověřit nastavení zařízení hello.
7. (Volitelně) nakonfigurujte váš webový proxy server. Přestože konfigurace webového proxy serveru je volitelný, mějte na paměti, že pokud používáte webový proxy server, můžete pouze nakonfigurovat ji sem.
   
    ![nakonfigurovat webový proxy server](./media/storsimple-virtual-array-deploy3-iscsi-setup/image9.png)
   
    Na hello **webový proxy server** stránky:
   
   1. Zadejte hello **webová adresa URL proxy serveru** v tomto formátu: *http://host-IP adresu* nebo *FDQN:Port číslo*. Všimněte si, že nejsou podporovány adresy URL typu HTTPS.
   2. Zadejte **ověřování** jako **základní** nebo **žádné**.
   3. Pokud používáte ověřování, budete také potřebovat tooprovide **uživatelské jméno** a **heslo**.
   4. Klikněte na tlačítko **Použít**. To bude ověřit a použít hello nakonfigurované nastavení webového proxy serveru.
8. (Volitelně) nakonfigurujte pro zařízení, jako je například časové pásmo hello nastavení času a hello primární a sekundární servery NTP. Protože vaše zařízení musí synchronizovat čas, takže můžete ověřit pomocí vašich poskytovatelů cloudových služeb, jsou vyžadovány NTP servery.
   
    ![Nastavení času](./media/storsimple-virtual-array-deploy3-iscsi-setup/image10.png)
   
    Na hello **nastavení času** stránky:
   
   1. Hello rozevíracím seznamu vyberte hello **časové pásmo** podle hello zeměpisného umístění, ve které hello zařízení nasazuje. Hello výchozí časové pásmo pro vaše zařízení je PST. Toto časové pásmo bude zařízení používat pro všechny naplánované operace.
   2. Zadejte **primární server NTP** pro vaše zařízení nebo přijměte výchozí hodnotu hello time.windows.com. Ujistěte se, že vaše síť umožňuje toopass provoz NTP z vašeho datového centra toohello Internetu.
   3. Volitelně zadejte **serveru NTP sekundární** pro vaše zařízení.
   4. Klikněte na tlačítko **Použít**. To bude ověřit a použít nastavení času hello nakonfigurované.
9. Nakonfigurujte nastavení hello cloudu pro vaše zařízení. V tomto kroku bude dokončení konfigurace hello místní zařízení a služby StorSimple Manager zařízení zaregistrujte zařízení hello.
   
   1. Zadejte hello **registrační klíč služby** které jste získali **krok 2: registrační klíč služby hello Get** v [nasazení zařízení StorSimple virtuální pole - Příprava hello portál](storsimple-virtual-array-deploy1-portal-prep.md#step-2-get-the-service-registration-key).
   2. Pokud to není hello první zařízení, které jsou s touto službou registraci, budete potřebovat tooprovide hello **šifrovacího klíče dat služby**. Tento klíč je požadován s hello služby registrace klíče tooregister další zařízení s hello služby StorSimple Manager zařízení. Další informace najdete v části příliš[šifrovacího klíče dat služby Get hello](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) ve vašem místním webové uživatelské rozhraní.
   3. Klikněte na tlačítko **zaregistrovat**. To se restartuje hello zařízení. Toowait může být nutné 2 – 3 minut před hello zařízení úspěšně zaregistrované. Po restartování zařízení hello budete přesměrováni toohello přihlašovací stránce.
      
      ![Registrace zařízení](./media/storsimple-virtual-array-deploy3-iscsi-setup/image11.png)
10. Vrátí toohello portálu Azure.
11. Přejděte toohello **zařízení** okna vaší služby. Pokud máte velké množství prostředků, klikněte na tlačítko **všechny prostředky**, klikněte na název služby (vyhledejte ho v případě potřeby) a pak klikněte na tlačítko **zařízení**.
12. Na hello **zařízení** okno, ověřte, že hello zařízení úspěšně připojilo toohello služby vyhledáním hello stavu. musí být ve stavu zařízení Hello **připraveni tooset až**.
    
    ![Registrace zařízení](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis1m.png)

## <a name="step-2-configure-hello-device-as-iscsi-server"></a>Krok 2: Konfigurace zařízení hello jako iSCSI server

Proveďte hello proveďte kroky v hello nastavení Azure portálu toocomplete hello požadované zařízení.

#### <a name="tooconfigure-hello-device-as-iscsi-server"></a>zařízení hello tooconfigure jako iSCSI server

1. Přejděte služby StorSimple Manager zařízení tooyour a potom přejděte příliš**správy > zařízení**. V hello **zařízení** okně, vyberte hello zařízení, kterou jste právě vytvořili. Zobrazí se toto zařízení jako **připraveni tooset až**.
   
    ![Nakonfigurujte zařízení jako iSCSI server](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis1m.png) 
2. Klikněte na tlačítko hello zařízení a vy se zobrazí nápis zprávu s upozorněním, toto zařízení hello je připraven toosetup.
   
    ![Nakonfigurujte zařízení jako iSCSI server](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis2m.png)  
3. Klikněte na tlačítko **konfigurace** na panelu příkazů hello zařízení. Otevře se hello **konfigurace** okno. V hello **konfigurace** okně hello následující:
   
   * název serveru iSCSI Hello se automaticky vyplní.
   * Zajistěte, aby šifrování úložiště cloudu hello je nastaven příliš**povoleno**. To zajišťuje, aby hello data odeslaná z cloudu toohello hello zařízení šifrovala.
   * Zadejte 32 znaků šifrovací klíč a záznam v aplikaci Správa klíčů pro budoucí použití.
   * Vyberte toobe účtu úložiště používat s vaším zařízením. V tomto předplatném, můžete vybrat existující účet úložiště, nebo můžete kliknout na **přidat** toochoose účet z jiného předplatného.
     
     ![Nakonfigurujte zařízení jako iSCSI server](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis4m.png)
4. Klikněte na tlačítko **konfigurace** toocomplete nastavení serveru iSCSI hello.
   
    ![Nakonfigurujte zařízení jako iSCSI server](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis5m.png) 
5. Budete informováni, zda text hello iSCSI serveru probíhá vytváření. Po úspěšném vytvoření serveru se službou iSCSI hello hello **zařízení** okno se aktualizuje a hello odpovídající zařízení ve stavu **Online**.
   
    ![Nakonfigurujte zařízení jako iSCSI server](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis9m.png)

## <a name="step-3-add-a-volume"></a>Krok 3: Přidání svazku

1. V hello **zařízení** okně, vyberte hello zařízení právě nakonfigurovali jako serveru iSCSI. Klikněte na tlačítko **...**  (případně klikněte pravým tlačítkem na tento řádek) a z hello kontextové nabídky, vyberte **Přidání svazku**. Můžete také kliknout na **+ přidat svazek** z příkazového řádku hello. Otevře se hello **Přidání svazku** okno.
   
    ![Přidání svazku](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis10m.png)
2. V hello **Přidání svazku** okně hello následující:
   
   * V hello **název svazku** pole, zadejte jedinečný název pro svazek. Název Hello musí být řetězec, který obsahuje 3 too127 znaky.
   * V hello **typ** rozevíracího seznamu, zadejte zda toocreate **nastavování** nebo **místně vázaný** svazku. Pro úlohy, které vyžadují místní záruky, nízkou latenci a vyšší výkon, vyberte **místně vázaný** **svazku**. Všechna ostatní data, vyberte **nastavování** **svazku**.
   * V hello **kapacity** pole, zadejte hello velikost svazku hello. Vrstvený svazek musí být mezi 500 GB a 5 TB a místně vázaný svazek musí být mezi 50 GB a 500 GB.
     
     Místně vázaný svazek je tlustě zřízený a zajišťuje, že primární data hello ve svazku hello zůstává v zařízení hello a nebudou distribuována toohello cloudu.
     
     Vrstvený svazek na hello je tence zřízený druhé straně. Když vytvoříte vrstvený svazek, na místní úrovni hello je zřízený přibližně 10 % hello místa a 90 % prostoru hello se zřizuje v cloudu hello. Například pokud jste zřídili svazku 1 TB, 100 GB by nacházet v místním prostoru hello a 900 GB se použije v cloudu hello při hello datové vrstvy. To znamená naopak je, že pokud spustíte mimo všechny hello volné místo na hello zařízení nemůže zřídit vrstvené sdílené složky (protože hello 10 % nebudete mít k dispozici).
     
     ![Přidání svazku](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis12.png)
   * Klikněte na tlačítko **připojení hostitele**, vyberte přístup k řízení záznamu (ACR) odpovídající toohello iniciátor iSCSI má tooconnect toothis svazek a pak klikněte na tlačítko **vyberte**. <br><br> 
3. Klikněte na tlačítko tooadd na nového hostitele připojené **přidat nový**, zadejte název hostitele hello a jeho iSCSI kvalifikovaný název IQN () a pak klikněte na tlačítko **přidat**. Pokud hello IQN nemáte, přejděte příliš[hello příloha A: získání názvu IQN hostitele Windows serveru](#appendix-a-get-the-iqn-of-a-windows-server-host).
   
      ![Přidání svazku](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis15m.png)
4. Po dokončení konfigurace svazku, klikněte na tlačítko **OK**. Vytvoří se svazek s hello zadaná nastavení a můžete se zobrazí oznámení. Ve výchozím nastavení monitorování a zálohování budou povolené pro svazek hello.
   
     ![Přidání svazku](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis18m.png)
5. tooconfirm, který hello svazek byl úspěšně vytvořil, přejděte toohello **svazky** okno. Měli byste vidět hello svazku uvedené.
   
   ![Přidání svazku](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis20m.png)

## <a name="step-4-mount-initialize-and-format-a-volume"></a>Krok 4: Připojení, inicializace a formátování svazků

Hello provést následující kroky toomount, inicializujte a formátujte svazky zařízení StorSimple na hostitelském Windows serveru.

#### <a name="toomount-initialize-and-format-a-volume"></a>toomount, inicializace a formátování svazků

1. Otevřete hello **iniciátor iSCSI** aplikace na příslušný server hello.
2. V hello **vlastnosti iniciátoru iSCSI** okně na hello **zjišťování** , klikněte na **vyhledat portál**.
   
    ![zjistit portálu](./media/storsimple-virtual-array-deploy3-iscsi-setup/image22.png)
3. V hello **zjistit cílový portál** dialogové okno, zadejte hello IP adresu vašeho iSCSI povolený síťového rozhraní a pak klikněte na tlačítko **OK**.
   
    ![IP adresa](./media/storsimple-virtual-array-deploy3-iscsi-setup/image23.png)
4. V hello **vlastnosti iniciátoru iSCSI** okně na hello **cíle** najděte hello **zjištěné cíle**. (Každý svazek bude zjištěné cíle.) Stav zařízení Hello by měla zobrazovat jako **neaktivní**.
   
    ![zjištěné cíle](./media/storsimple-virtual-array-deploy3-iscsi-setup/image24.png)
5. Vyberte cílové zařízení a pak klikněte na tlačítko **Connect**. Po připojení zařízení hello, stav hello by se měl změnit příliš**připojeno**. (Další informace o použití iniciátoru iSCSI společnosti Microsoft hello najdete v tématu [instalace a konfigurace Microsoft iSCSI Initiator][1].
   
    ![Vyberte cílové zařízení](./media/storsimple-virtual-array-deploy3-iscsi-setup/image25.png)
6. Na hostiteli s Windows, stiskněte klávesu s logem Windows hello + X a potom klikněte na **spustit**.
7. V hello **spustit** dialogové okno, typ **Diskmgmt.msc**. Klikněte na tlačítko **OK**a hello **Správa disků** se zobrazí dialogové okno. Hello pravém podokně se zobrazí hello svazky na hostiteli.
8. V hello **Správa disků** okně hello připojené svazky zobrazí jak ukazuje následující obrázek hello. Klikněte pravým tlačítkem na hello zjištěné svazek (klikněte na název disku hello) a potom klikněte na **Online**.
   
    ![Správa disků](./media/storsimple-virtual-array-deploy3-iscsi-setup/image26.png)
9. Klikněte pravým tlačítkem a vyberte **inicializovat Disk**.
   
    ![Inicializovat disk 1](./media/storsimple-virtual-array-deploy3-iscsi-setup/image27.png)
10. V dialogovém okně hello, vyberte tooinitialize hello disky a pak klikněte na tlačítko **OK**.
    
    ![Inicializovat disk 2](./media/storsimple-virtual-array-deploy3-iscsi-setup/image28.png)
11. Spustí se průvodce Nový jednoduchý svazek Hello. Vyberte velikost disku a pak klikněte na tlačítko **Další**.
    
    ![Průvodce vytvořením svazku 1](./media/storsimple-virtual-array-deploy3-iscsi-setup/image29.png)
12. Přiřadit toohello svazek písmeno jednotky a pak klikněte na tlačítko **Další**.
    
    ![Průvodce vytvořením svazku 2](./media/storsimple-virtual-array-deploy3-iscsi-setup/image30.png)
13. Zadejte hello parametry tooformat hello svazku. **V systému Windows Server je podporována pouze v systému souborů NTFS.** Nastavit too64K velikost jednotky přidělení hello. Zadejte popisek svazku. Je součástí osvědčeného postupu pro tento název toobe identické toohello název svazku, které jste zadali na pole virtuální zařízení StorSimple. Klikněte na **Další**.
    
    ![Průvodce vytvořením svazku 3](./media/storsimple-virtual-array-deploy3-iscsi-setup/image31.png)
14. Zkontrolujte hodnoty hello svazku a pak klikněte na tlačítko **Dokončit**.
    
    ![Průvodce vytvořením svazku 4](./media/storsimple-virtual-array-deploy3-iscsi-setup/image32.png)
    
    Hello svazky se zobrazí jako **Online** na hello **Správa disků** stránky.
    
    ![svazky online](./media/storsimple-virtual-array-deploy3-iscsi-setup/image33.png)

## <a name="next-steps"></a>Další kroky

Zjistěte, jak toouse hello místního webového uživatelského rozhraní příliš[spravovat vaše pole virtuální zařízení StorSimple](storsimple-ova-web-ui-admin.md).

## <a name="appendix-a-get-hello-iqn-of-a-windows-server-host"></a>Příloha A: Get hello názvu IQN hostitele Windows serveru

Proveďte následující kroky tooget hello iSCSI hello kvalifikovaný název (IQN) hostitele Windows se systémem Windows Server 2012.

#### <a name="tooget-hello-iqn-of-a-windows-host"></a>tooget hello IQN hostitele Windows

1. Na hostiteli s Windows spusťte iniciátor iSCSI společnosti Microsoft hello.
2. V hello **vlastnosti iniciátoru iSCSI** okně na hello **konfigurace** , vyberte a zkopírujte řetězec hello z hello **název iniciátoru** pole.
   
    ![Vlastnosti iniciátoru iSCSI](./media/storsimple-virtual-array-deploy3-iscsi-setup/image34.png)
3. Uložte tento řetězec.

<!--Reference link-->
[1]: https://technet.microsoft.com/library/ee338480(WS.10).aspx



