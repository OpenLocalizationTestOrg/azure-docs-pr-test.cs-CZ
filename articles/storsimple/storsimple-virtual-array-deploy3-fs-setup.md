---
title: "aaaSet až pole virtuální zařízení StorSimple jako souborový server | Microsoft Docs"
description: "V tomto kurzu třetí v nasazení pole virtuální zařízení StorSimple pokyn tooset až virtuálního zařízení jako souborový server."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: f609f6ff-0927-48bb-a68a-6d8985d2fe34
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/17/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 89cade37980f342695c0adee42c4ade0e1d53d2e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-storsimple-virtual-array---set-up-as-file-server-via-azure-portal"></a>Nasazení pole virtuálního zařízení StorSimple - Set až jako souborový server prostřednictvím portálu Azure
![](./media/storsimple-virtual-array-deploy3-fs-setup/fileserver4.png)

## <a name="introduction"></a>Úvod
Tento článek popisuje, jak tooperform počáteční nastavení, zaregistrovat zařízení StorSimple souborový server, dokončení hello nastavení zařízení a vytvořit a připojit tooSMB sdílené složky. Toto je poslední článek hello v řadě hello kurzy nasazení vyžaduje toocompletely nasadit virtuální pole jako souborový server nebo server se službou iSCSI.

proces instalace a konfigurace Hello může trvat přibližně 10 minut toocomplete. Hello informace v tomto článku se vztahují pouze toohello nasazení hello pole virtuální zařízení StorSimple. Nasazení hello řadu zařízení StorSimple 8000, přejděte na: [nasazení zařízení řady StorSimple 8000 softwarem Update 2](storsimple-deployment-walkthrough-u2.md).

## <a name="setup-prerequisites"></a>Požadavky instalačního programu
Než začnete konfigurovat a nastavit pole virtuální zařízení StorSimple, ujistěte se, že:

* Když máte zřízenou virtuální pole a připojené tooit jako podrobné v hello [zřídit virtuální pole Hyper-v StorSimple](storsimple-virtual-array-deploy2-provision-hyperv.md) nebo [zřídit o virtuální zařízení StorSimple pole v prostředí VMware](storsimple-virtual-array-deploy2-provision-vmware.md).
* Máte hello registrační klíč služby ze služby StorSimple Manager zařízení hello vytvořený virtuální pole toomanage StorSimple. Další informace najdete v tématu [krok 2: registrační klíč služby hello Get](storsimple-virtual-array-deploy1-portal-prep.md#step-2-get-the-service-registration-key) pro pole virtuální zařízení StorSimple.
* Pokud je to hello druhém a dalším virtuální pole, které jsou registraci s existující službu Správce zařízení StorSimple, měli byste mít šifrovacího klíče dat služby hello. Tento klíč byl vygenerován při hello první zařízení byl úspěšně registrován s touto službou. Pokud jste ztratili tento klíč, přečtěte si téma [šifrovacího klíče dat služby Get hello](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) pro pole virtuální zařízení StorSimple.

## <a name="step-by-step-setup"></a>Krok za krokem instalace
Použijte hello následující podrobné pokyny tooset a konfiguraci pole virtuální zařízení StorSimple.

## <a name="step-1-complete-hello-local-web-ui-setup-and-register-your-device"></a>Krok 1: Dokončení hello místní webový instalační program uživatelského rozhraní a zaregistrovat zařízení
#### <a name="toocomplete-hello-setup-and-register-hello-device"></a>toocomplete hello instalace a registrace zařízení hello
1. Otevřete okno prohlížeče a připojte toohello místního webového uživatelského rozhraní. Zadejte:
   
   `https://<ip-address of network interface>`
   
   Použijte adresu URL připojení hello si poznamenali v předchozím kroku hello. Zobrazí chyba oznamující, že došlo k potížím s certifikátem zabezpečení webu hello. Klikněte na tlačítko **pokračovat webová stránka toothis**.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image2.png)
2. Přihlášení toohello webové uživatelské rozhraní vaší virtuální pole jako **StorSimpleAdmin**. Zadejte heslo správce zařízení hello, který jste změnili v kroku 3: spuštění hello virtuální pole v [zřídit virtuální pole Hyper-v StorSimple](storsimple-virtual-array-deploy2-provision-hyperv.md) nebo v [zřídit o virtuální zařízení StorSimple pole v prostředí VMware](storsimple-virtual-array-deploy2-provision-vmware.md).
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image3.png)
3. Budete přesměrováni toohello **Domů** stránky. Tato stránka popisuje hello různá nastavení požadované tooconfigure a zaregistrujte virtuální pole s služby StorSimple Manager zařízení hello hello. Hello **nastavení síťového**, **webové nastavení proxy serveru**, a **nastavení času** jsou volitelné. Hello pouze požadované nastavení **nastavení zařízení** a **nastavení cloudu**.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image4.png)
4. V hello **nastavení síťového** v části **síťových rozhraní**, bude pro vás automaticky nakonfigurován DATA 0. Každé síťové rozhraní je automaticky nastaven pomocí výchozí tooget IP adresy (DHCP). Adresu IP, podsíť a brána se proto automaticky přiřadí (pro IPv4 i IPv6).
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image5.png)
   
   Pokud jste přidali více než jedno síťové rozhraní během zřizování hello hello zařízení, můžete je konfigurovat tady. Všimněte si, že síťové rozhraní můžete nakonfigurovat jako IPv4 pouze nebo jako oba protokoly IPv4 a IPv6. Nejsou podporovány pouze konfigurace IPv6.
5. Servery DNS jsou požadované, protože se používají při zařízení pokusí toocommunicate s poskytovatele cloudových služeb úložiště nebo tooresolve zařízení podle názvu, když nakonfigurovaný jako souborový server. V hello **nastavení síťového** stránky v rámci hello **servery DNS**:
   
   1. Primární a sekundární server DNS budou automaticky nakonfigurovány. Pokud si zvolíte tooconfigure statické IP adresy, můžete zadat servery DNS. Pro vysokou dostupnost doporučujeme nakonfigurovat primární a sekundární server DNS.
   2. Klikněte na tlačítko **použít** tooapply a ověřte nastavení sítě hello.
6. V hello **nastavení zařízení** stránky:
   
   1. Přiřadit jedinečný **název** tooyour zařízení. Tento název může obsahovat 1 až 15 znaků a může obsahovat písmena, číslice a pomlčky.
   2. Klikněte na tlačítko hello **souborový server** ikonu ![](./media/storsimple-virtual-array-deploy3-fs-setup/image6.png) pro hello **typu** zařízení, které vytváříte. Souborový server vám umožní toocreate sdílené složky.
   3. Zařízení je souborový server, budete potřebovat toojoin hello zařízení tooa domény. Zadejte **název domény**.
   4. Klikněte na tlačítko **Použít**.
7. Zobrazí se dialogové okno. Zadejte přihlašovací údaje domény v zadaném formátu hello. Klikněte na ikonu zaškrtnutí hello. ověření pověření domény Hello. Zobrazí chybovou zprávu, pokud hello přihlašovací údaje jsou nesprávné.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image7.png)
8. Klikněte na tlačítko **Použít**. Použije a ověřit nastavení zařízení hello.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image8.png)
   
   > [!NOTE]
   > Zkontrolujte, zda je vaše virtuální pole ve vlastní organizační jednotku (OU) pro službu Active Directory a žádné objekty zásad skupiny (GPO) jsou použité tooit nebo zděděná. Zásady skupiny může instalovat aplikace jako antivirový software hello pole virtuální zařízení StorSimple. Instalace dalšího softwaru není podporována a může vést k poškození toodata. 
   > 
   > 
9. (Volitelně) nakonfigurujte váš webový proxy server. Přestože konfigurace webového proxy serveru je volitelný, mějte na paměti, že pokud používáte webový proxy server, můžete pouze nakonfigurovat ji sem.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image9.png)
   
   V hello **webový proxy server** stránky:
   
   1. Zadejte hello **webová adresa URL proxy serveru** v tomto formátu: *http://&lt;hostitele IP adresu nebo plně kvalifikovaného názvu domény&gt;: číslo portu*. Všimněte si, že nejsou podporovány adresy URL typu HTTPS.
   2. Zadejte **ověřování** jako **základní** nebo **žádné**.
   3. Pokud používáte ověřování, budete také potřebovat tooprovide **uživatelské jméno** a **heslo**.
   4. Klikněte na tlačítko **Použít**. To bude ověřit a použít hello nakonfigurované nastavení webového proxy serveru.
10. (Volitelně) nakonfigurujte pro zařízení, jako je například časové pásmo hello nastavení času a hello primární a sekundární servery NTP. Protože vaše zařízení musí synchronizovat čas, takže můžete ověřit pomocí vašich poskytovatelů cloudových služeb, jsou vyžadovány NTP servery.
    
    ![](./media/storsimple-virtual-array-deploy3-fs-setup/image10.png)
    
    V hello **nastavení času** stránky:
    
    1. Hello rozevíracího seznamu vyberte hello **časové pásmo** podle hello zeměpisného umístění, ve které hello zařízení nasazuje. Hello výchozí časové pásmo pro vaše zařízení je PST. Toto časové pásmo bude zařízení používat pro všechny naplánované operace.
    2. Zadejte **primární server NTP** pro vaše zařízení nebo přijměte výchozí hodnotu hello time.windows.com. Ujistěte se, že vaše síť umožňuje toopass provoz NTP z vašeho datového centra toohello Internetu.
    3. Volitelně zadejte **serveru NTP sekundární** pro vaše zařízení.
    4. Klikněte na tlačítko **Použít**. To bude ověřit a použít nastavení času hello nakonfigurované.
11. Nakonfigurujte nastavení hello cloudu pro vaše zařízení. V tomto kroku bude dokončení konfigurace hello místní zařízení a služby StorSimple Manager zařízení zaregistrujte zařízení hello.
    
    1. Zadejte hello **registrační klíč služby** které jste získali [krok 2: registrační klíč služby hello Get](storsimple-virtual-array-deploy1-portal-prep.md#step-2-get-the-service-registration-key) pro pole virtuální zařízení StorSimple.
    2. Pokud je to první zařízení registrace s touto službou, zobrazí se hello **šifrovacího klíče dat služby**. Klíč zkopírujte a uložte na bezpečném místě. Tento klíč je požadován s hello služby registrace klíče tooregister další zařízení s hello služby StorSimple Manager zařízení. 
       
       Pokud to není hello první zařízení, které jsou s touto službou registraci, budete potřebovat šifrovacího klíče dat služby tooprovide hello. Další informace najdete v části tooget hello [šifrovacího klíče dat služby](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) ve vašem místním webové uživatelské rozhraní.
    3. Klikněte na tlačítko **zaregistrovat**. To se restartuje hello zařízení. Toowait může být nutné 2 – 3 minut před hello zařízení úspěšně zaregistrované. Po restartování zařízení hello budete přesměrováni toohello přihlašovací stránce.
       
       ![](./media/storsimple-virtual-array-deploy3-fs-setup/image13.png)
12. Vrátí toohello portálu Azure. Přejděte příliš**všechny prostředky**, vyhledávání služby StorSimple Manager zařízení.
    
    ![](./media/storsimple-virtual-array-deploy3-fs-setup/searchdevicemanagerservice1.png) 
13. V hello filtrovat seznam, vyberte svoji službu StorSimple Manager zařízení a pak přejděte příliš**správy > zařízení**. V hello **zařízení** okně ověřte hello zařízení úspěšně připojilo toohello služby a má stav hello **připraveni tooset až**.
    
    ![Konfigurace souborového serveru](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs2m.png)

## <a name="step-2-configure-hello-device-as-file-server"></a>Krok 2: Konfigurace zařízení hello jako souborový server
Proveďte následující kroky v hello hello [portál Azure](https://portal.azure.com/) toocomplete hello požadované nastavení zařízení.

#### <a name="tooconfigure-hello-device-as-file-server"></a>zařízení hello tooconfigure jako souborový server
1. Přejděte služby StorSimple Manager zařízení tooyour a potom přejděte příliš **správy > zařízení**. V hello **zařízení** okně, vyberte hello zařízení, kterou jste právě vytvořili. Zobrazí se toto zařízení jako **připraveni tooset až**.
   
   ![Konfigurace souborového serveru](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs2m.png) 
2. Klikněte na tlačítko hello zařízení a vy se zobrazí nápis zprávu s upozorněním, toto zařízení hello je připraven toosetup.
   
    ![Konfigurace souborového serveru](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs3m.png)
3. Klikněte na tlačítko **konfigurace** na panelu příkazů hello. Otevře se hello **konfigurace** okno. V hello **konfigurace** okně hello následující:
   
    1. název souborového serveru Hello se automaticky vyplní.
    
    2. Zajistěte, aby šifrování úložiště cloudu hello je nastaven příliš**povoleno**. To bude šifrování všech dat hello, které se odesílají toohello cloudu. 
    
    3. Klíče AES 256 bitů se používá s hello uživatelské klíče pro šifrování. Zadejte klíč 32 znaků a potom znovu zadat klíče tooconfirm hello ho. Záznam hello klíč v aplikaci pro správu klíčů pro budoucí použití.
    
    4. Klikněte na tlačítko **konfigurovat požadované nastavení** účet úložiště toospecify pověření toobe používat s vaším zařízením. Klikněte na tlačítko **přidat nový** Pokud nejsou konfigurována žádná pověření účtu úložiště. **Zkontrolujte, jestli hello účet úložiště, které používáte, podporuje objekty BLOB bloku. Objekty BLOB stránky nejsou podporovány.** Další informace o [blokuje objekty BLOB a objekty BLOB stránky](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs).
   
    ![Konfigurace souborového serveru](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs6m.png) 
4. V hello **přidat přihlašovacích údajů účtu úložiště** okně hello následující: 

    1. Vyberte aktuální předplatné, pokud je účet úložiště hello v hello stejném předplatném jako služba hello. Zadejte jiné je úložiště hello účet je mimo předplatné služby hello. 
    
    2. Z rozevíracího seznamu hello zvolte existující účet úložiště. 
    
    3. Hello umístění se automaticky vyplní podle hello zadaný účet úložiště. 
    
    4. Povolte SSL tooensure zabezpečené sítě komunikační kanál mezi hello zařízení a hello cloudu.
    
    5. Klikněte na tlačítko **přidat** tooadd účet toto úložiště přihlašovacích údajů. 
   
        ![Konfigurace souborového serveru](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs8m.png)

5. Po úspěšném vytvoření pověření účtu úložiště hello hello **konfigurace** okno bude aktualizovat toodisplay hello zadané přihlašovací údaje účtu úložiště. Klikněte na **Konfigurovat**.
   
   ![Konfigurace souborového serveru](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs11m.png)
   
   Zobrazí se soubor serveru se vytváří. Po úspěšném vytvoření hello souborový server, budete upozorněni.
   
   ![Konfigurace souborového serveru](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs13m.png)
   
   Stav zařízení Hello změní také příliš**Online**.
   
   ![Konfigurace souborového serveru](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs14m.png)
   
   Můžete pokračovat tooadd sdílenou složku.

## <a name="step-3-add-a-share"></a>Krok 3: Přidání sdílené složky
Proveďte následující kroky v hello hello [portál Azure](https://portal.azure.com/) toocreate sdílenou složku.

#### <a name="toocreate-a-share"></a>toocreate sdílené složky
1. Vyberte zařízení server souboru hello jste nakonfigurovali v předchozím kroku hello a klikněte na tlačítko **...**  (nebo klikněte pravým tlačítkem myši). V místní nabídce hello vyberte **přidat sdílenou složku**. Případně můžete kliknout na **+ přidat sdílenou složku** na panelu příkazů hello zařízení.
   
   ![Přidání sdílené složky](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs15m.png)
2. Zadejte následující nastavení sdílené složky hello:

    1. Jedinečný název sdílené položky. Název Hello musí být řetězec, který obsahuje 3 too127 znaky.
    
    2. Volitelný **popis** hello sdílené složky. Popis Hello pomůže identifikovat hello vlastníkům sdílené složky.
    
    3. A **typ** pro sdílenou složku hello. může být typ Hello **nastavování** nebo **místně vázaný**, s vrstvami se hello výchozí. Pro úlohy, které vyžadují místní záruky, nízkou latenci a vyšší výkon, vyberte **místně vázaný** sdílet. Všechna ostatní data, vyberte **nastavování** sdílet.
    Sdílenou složku místně vázaný je tlustě zřízený a zajišťuje, že primární data hello ve složce hello zůstává místní toohello zařízení a nebudou distribuována toohello cloudu. Vrstvený sdílenou složku na hello je tence zřízený druhé straně. Když vytvoříte sdílenou složku vrstvené, na místní úrovni hello je zřízený 10 % hello místa a 90 % prostoru hello se zřizuje v cloudu hello. Například pokud jste zřídili svazku 1 TB, 100 GB by nacházet v místním prostoru hello a 900 GB se použije v cloudu hello při hello datové vrstvy. Naopak z toho vyplývá, že pokud spustíte mimo všechny hello volné místo na hello zařízení, nejde zřídit vrstvené sdílené složky.
   
    4. V hello **nastavit výchozí úplná oprávnění** pole, přiřadit hello oprávnění toohello uživatele nebo skupinu hello, který přistupuje k této sdílené složce. Zadejte název hello hello uživatele nebo skupiny uživatelů hello v  *john@contoso.com*  formátu. Doporučujeme používat tooaccess oprávnění správce uživatele skupiny (ne jenom jednoho konkrétního uživatele) tooallow tyto sdílené složky. Po přiřazení oprávnění hello sem, pak můžete Průzkumníka souborů toomodify tato oprávnění.
   
    5. Klikněte na tlačítko **přidat** toocreate hello sdílené složky. 
    
        ![Přidání sdílené složky](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs18m.png)
   
        Zobrazí upozornění, že vytvoření sdílené složky hello probíhá.
   
        ![Přidání sdílené složky](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs19m.png)
   
    Po vytvoření sdílené složky hello s hello zadat nastavení, hello **sdílené složky** okno zaktualizuje tooreflect hello novou sdílenou složku. Ve výchozím nastavení monitorování a zálohování jsou povolené pro sdílenou složku hello.
   
    ![Přidání sdílené složky](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs22m.png)

## <a name="step-4-connect-toohello-share"></a>Krok 4: Připojení toohello sdílené složky
Nyní je nutné tooconnect tooone nebo více sdílených složek, které jste vytvořili v předchozím kroku hello. Tyto kroky provádíte u vašeho systému Windows Server hostování připojené tooyour pole virtuální zařízení StorSimple.

#### <a name="tooconnect-toohello-share"></a>sdílené složky toohello tooconnect
1. Stiskněte klávesu ![](./media/storsimple-virtual-array-deploy3-fs-setup/image22.png) + R. V okně Spustit hello zadejte hello *&#92; &#92;&lt; název souborového serveru&gt;*  jako cestu hello nahrazení *název souborového serveru* s hello zařízení název této vám přiřazené tooyour souborového serveru. Klikněte na **OK**.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image23.png)
2. Otevře se Průzkumník souborů. Teď byste měli být schopný toosee hello složky, které jste vytvořili jako složky. Vyberte a dvakrát klikněte na sdílenou složku (složka) tooview hello obsahu.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image24.png)
3. Teď můžete přidávat sdílené složky souborů toothese a provedení zálohy.

## <a name="next-steps"></a>Další kroky
Zjistěte, jak toouse hello místního webového uživatelského rozhraní příliš[spravovat vaše pole virtuální zařízení StorSimple](storsimple-ova-web-ui-admin.md).

