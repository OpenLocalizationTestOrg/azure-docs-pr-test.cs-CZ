---
title: "hello aaaDeploy služby StorSimple Manager zařízení v Azure | Microsoft Docs"
description: "Popisuje, jak toocreate a odstranění hello služby StorSimple Manager zařízení v hello portálu Azure a popisuje, jak toomanage hello registrační klíč služby."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/13/2017
ms.author: alkohli
ms.openlocfilehash: b84a907d6b735c8fee7bdc51f9c0074857297d2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-hello-storsimple-device-manager-service-for-storsimple-8000-series-devices"></a>Nasazení služby StorSimple Manager zařízení hello pro řadu zařízení StorSimple 8000

## <a name="overview"></a>Přehled

Hello služby StorSimple Manager zařízení běží ve službě Microsoft Azure a připojí zařízení StorSimple toomultiple. Po vytvoření hello služby, můžete ho toomanage všechna hello zařízení, které jsou připojené toohello Správce zařízení StorSimple služby z jedné centrální umístění, a současně minimalizujete její související administrativní zátěže.

Tento kurz popisuje hello kroky potřebné k hello vytváření, odstraňování, migrace hello služby a správu hello registrační klíč služby hello. Hello informace obsažené v tomto článku je použít jenom zařízení řady tooStorSimple 8000. Další informace o pole virtuální zařízení StorSimple, přejděte příliš[nasazení služby StorSimple Manager zařízení pro vaše pole virtuální zařízení StorSimple](storsimple-virtual-array-manage-service.md).

## <a name="create-a-service"></a>Vytvoření služby
toocreate služby StorSimple Manager zařízení, je třeba toohave:

* Předplatné s smlouvu Enterprise Agreement
* Aktivní účet úložiště Microsoft Azure
* fakturační informace, které se používá pro správu přístupu Hello

Jsou povoleny pouze hello odběry ke smlouvě Enterprise. Microsoft Sponsorship odběry, které byly povoleny v hello portál Azure classic nepodporuje hello portálu Azure. Zobrazí se následující zpráva při použití předplatné nepodporované hello:

![Předplatné není platný](./media/storsimple-8000-manage-service/subscription-not-valid.jpg)

Můžete také toogenerate výchozí účet úložiště při vytváření služby hello.

Jeden služby můžete spravovat více zařízení. Zařízení, ale nemůžou zahrnovat víc služeb. Velký podnik může mít více instancí toowork služby pomocí různých předplatných, organizace nebo i umístění nasazení. 

> [!NOTE]
> Je třeba samostatné instance řadu zařízení StorSimple Manager zařízení služby toomanage StorSimple 8000 a pole virtuální zařízení StorSimple.

Proveďte následující kroky toocreate služby hello.

[!INCLUDE [storsimple-create-new-service](../../includes/storsimple-8000-create-new-service.md)]


Pro každou službu StorSimple Manager zařízení existují hello následující atributy:

* **Název** – hello název, který byl přiřazen služby StorSimple Manager zařízení tooyour, pokud byla vytvořena. **název služby Hello nelze změnit po vytvoření služby hello. To platí také pro ostatní entity, jako jsou zařízení, svazky, kontejnery svazků a zásady zálohování, které nelze přejmenovat v hello portálu Azure.**
* **Stav** – hello stav hello služby, který může být **Active**, **vytváření**, nebo **Online**.
* **Umístění** – hello zeměpisné umístění, ve které hello StorSimple zařízení nasadí.
* **Předplatné** – hello fakturace předplatného, které souvisí s vaší služby.

## <a name="move-a-service-tooazure-portal"></a>Přesunout portál tooAzure služby
Řady StorSimple 8000 je nyní spravovat v hello portálu Azure. Pokud máte stávající služby toomanage hello zařízení StorSimple, doporučujeme přesunout vaší služby toohello portálu Azure. Hello klasický portál Azure pro hello služby StorSimple Manager není k dispozici po 30 září 2017.

Hello možnost toomigrate toohello portál Azure je k dispozici ve fázích. Pokud nevidíte ve možnost toomigrate tooAzure portál, ale chcete toomove a projít hello dopad migrace, jak je uvedeno v hello [důležité informace týkající se přechodu](#considerations-for-transition), můžete [odeslat žádost o](https://aka.ms/ss8000-cx-signup).

### <a name="considerations-for-transition"></a>Důležité informace týkající se přechodu

Zkontrolujte hello dopad migrace toohello neaktivních nový portál Azure, před přesunutím hello služby.

#### <a name="before-you-transition"></a>Před přechod

* Vaše zařízení používá aktualizace 3.0 nebo novější. Pokud vaše zařízení běží starší verze, nainstalujte nejnovější aktualizace hello. Další informace, přejděte příliš[nainstalovat aktualizace 4](storsimple-8000-install-update-4.md). Pokud používáte zařízení s StorSimple cloudu (8010/8020), vytvořte nové zařízení cloudu s Update 4.0. 

* Až se kvůli toohello nový portál Azure, nemůžete použít hello Azure classic portálu toomanage zařízení StorSimple.

* neexistuje žádné výpadky v případě zařízení hello přechod Hello je omezovaly.

* Všichni správci zařízení StorSimple hello pod hello zadat kvůli předplatného.

#### <a name="during-hello-transition"></a>Během přechodu hello

* Nelze spravovat vaše zařízení z portálu hello.
* Operace, například vrstvení a naplánované zálohování pokračovat toooccur.
* Neodstraňujte hello staré Správci zařízení StorSimple, když probíhá přechod hello.

#### <a name="after-hello-transition"></a>Po přechodu hello

* Již nebude možné spravovat vaše zařízení z portálu classic hello.

* Hello existující rutiny prostředí PowerShell Azure Service Management (ASM) nejsou podporovány. Aktualizujte hello skripty toomanage zařízení prostřednictvím hello Azure Resource Manager.

* Konfiguraci služby a zařízení zůstanou zachovány. Všechny svazky a zálohy jsou také kvůli toohello portálu Azure.

### <a name="begin-transition"></a>Začněte přechodu

Proveďte následující kroky tootransition hello vaší služby toohello portálu Azure.

1. Přejděte tooyour službu StorSimple Manager portálu classic hello.

2. Zobrazí oznámení, že informuje, že služby StorSimple Manager zařízení hello je nyní k dispozici v hello portálu Azure. Upozorňujeme, že v hello portálu Azure, služby hello se označují tooas služby StorSimple Manager zařízení.

    ![Migrace oznámení](./media/storsimple-8000-manage-service/service-transition1.jpg)

    1. Ujistěte se, že jste si přečetli hello úplné dopad migrace.
    2. Projděte si seznam hello Správce zařízení StorSimple, která budou přesunuty z portálu classic hello.

3. Klikněte na tlačítko **migrovat**. Přechod Hello spustí a trvá několik minut toocomplete.

Po dokončení přechodu hello můžete spravovat zařízení prostřednictvím hello služby StorSimple Manager zařízení v hello portálu Azure.

V hello portálu Azure pouze hello zařízení StorSimple se systémem aktualizace 3.0 a vyšší jsou podporovány. Hello zařízení, které se používají starší verze mít omezenou podporu. Následující tabulka summrizes operací, které jsou podporovány v hello zařízení se systémem versios předchozí tooUpdate 3.0, když jste migrovali ze hello classic toohello portál Azure Hello.

| Operace                                                                                                                       | Podporuje se      |
|---------------------------------------------------------------------------------------------------------------------------------|----------------|
| Registrovat zařízení                                                                                                               | Ano            |
| Konfigurace nastavení zařízení, jako je například obecné, sítě a zabezpečení                                                                | Ano            |
| Kontrola, stáhnout a nainstalovat aktualizace                                                                                             | Ano            |
| Deaktivovat zařízení                                                                                                               | Ano            |
| Odstranění zařízení                                                                                                                   | Ano            |
| Vytvářet, upravovat a odstraňovat kontejneru svazků                                                                                   | Ne             |
| Vytvářet, upravovat a odstraňovat svazku                                                                                             | Ne             |
| Vytvářet, upravovat a odstraňovat zásady zálohování                                                                                      | Ne             |
| Proveďte ruční zálohy                                                                                                            | Ne             |
| Proveďte zálohu naplánované                                                                                                         | Neuvedeno |
| Obnovení z záloh                                                                                                        | Ne             |
| Klonování tooa zařízení se systémem aktualizace 3.0 a novějších <br> Hello zdrojového zařízení se systémem tooUpdate předchozí verze 3.0.                                | Ano            |
| Klonování tooa zařízení se systémem tooUpdate předchozí verze 3.0                                                                          | Ne             |
| Převzetí služeb při selhání jako zdrojového zařízení <br> (ze zařízení se systémem verze předchozí tooUpdate 3.0 tooa zařízení se systémem aktualizace 3.0 a novější)                                                               | Ano            |
| Převzetí služeb při selhání jako cílové zařízení <br> (tooa zařízení se systémem předchozí tooUpdate software verze 3.0)                                                                                   | Ne             |
| Vymazat výstrahu                                                                                                                  | Ano            |
| Zobrazit zásady zálohování, zálohování katalog, svazky, kontejnery svazků, monitorování grafy, úlohy a výstrahy vytvořené v portálu classic | Ano            |
| Zapnutí a vypnutí řadiče zařízení                                                                                              | Ano            |


## <a name="delete-a-service"></a>Odstranění služby

Před odstraněním služby, ujistěte se, že žádné připojená zařízení ji používají. Pokud služba hello se používá, deaktivujte hello připojené zařízení. Hello deaktivovat operaci severu hello připojení mezi hello zařízení a hello služby, ale zachovat data zařízení hello v cloudu hello.

> [!IMPORTANT]
> Po odstranění služby hello operace se nedá vrátit. Jakékoli zařízení, které se pomocí služby hello musí toobe resetování toofactory výchozí předtím, než se dá použít s jinou službu. V tomto scénáři hello místní data na zařízení hello, jakož i konfigurace hello bude ztracena.

Proveďte následující kroky toodelete služby hello.

### <a name="toodelete-a-service"></a>toodelete služby

1. Vyhledávání pro službu hello chcete toodelete. Klikněte na tlačítko **prostředky** ikonu a pak vstup hello toosearch příslušné podmínky. Ve výsledcích hledání hello klikněte na tlačítko hello služby chcete toodelete.

    ![Toodelete služby vyhledávání](./media/storsimple-8000-manage-service/deletessdevman1.png)

2. Tím přejdete okno služby StorSimple Manager zařízení toohello. Klikněte na **Odstranit**.

    ![Odstranění služby](./media/storsimple-8000-manage-service/deletessdevman2.png)

3. Klikněte na tlačítko **Ano** v hello potvrzení oznámení. To může trvat několik minut, než toobe služby hello odstranit.

    ![Potvrzení odstranění](./media/storsimple-8000-manage-service/deletessdevman3.png)

## <a name="get-hello-service-registration-key"></a>Získat registrační klíč služby hello

Po úspěšném vytvoření služby budete potřebovat tooregister zařízení StorSimple službou hello. tooregister prvního zařízení StorSimple, bude nutné hello registrační klíč služby. tooregister další zařízení s existující službu StorSimple, potřebujete hello registrační klíč a hello šifrovacího klíče dat služby (který je generován během registrace na první zařízení hello). Další informace o hello šifrovacího klíče dat služby najdete v tématu [zabezpečení zařízení StorSimple](storsimple-8000-security.md). Hello registrační klíč lze získat přístup k **klíče** v okně vaší Správce zařízení StorSimple.

Proveďte následující kroky tooget hello služby registrační klíč hello.

[!INCLUDE [storsimple-8000-get-service-registration-key](../../includes/storsimple-8000-get-service-registration-key.md)]

Registrační klíč služby hello mějte do bezpečného umístění. Budete potřebovat tento klíč, a také šifrovacího klíče dat služby hello, tooregister další zařízení s touto službou. Po získání registračního klíče služby hello, je nutné nakonfigurovat zařízení prostřednictvím hello Windows Powershellu pro StorSimple rozhraní.

Podrobnosti o tom toouse klíče, najdete v tématu registrace [krok 3: Konfigurace a registrace zařízení hello pomocí Windows Powershellu pro StorSimple](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

## <a name="regenerate-hello-service-registration-key"></a>Znovu vygenerovat registrační klíč služby hello
Je nutné tooregenerate registrační klíč služby, pokud jsou požadované tooperform střídání klíče nebo pokud došlo ke změně hello seznamu správců služeb. Pokud jste znovu vygenerovat klíč hello, hello nový klíč slouží pouze k registraci dalších zařízení. Hello zařízení, které již byly zaregistrovány nejsou ovlivněny tímto procesem.

Proveďte následující kroky tooregenerate registrační klíč služby hello.

### <a name="tooregenerate-hello-service-registration-key"></a>registrační klíč služby hello tooregenerate
1. V hello **Manager zařízení StorSimple** okně přejděte příliš**správy &gt;**  **klíče**.
    
    ![Okno Klíče](./media/storsimple-8000-manage-service/regenregkey2.png)

2. V hello **klíče** okně klikněte na tlačítko **znovu vygenerovat**.

    ![Klikněte na tlačítko znovu vygenerovat](./media/storsimple-8000-manage-service/regenregkey3.png)
3. V hello **registrační klíč znovu generovat služby** okně zkontrolujte hello akce při vyžaduje hello klíče se generují. Všechny následné zařízení hello, které jsou registrované s touto službou používat hello nový registrační klíč. Klikněte na tlačítko **znovu vygenerovat** tooconfirm. Upozornění se zobrazí po dokončení opětovné generování hello.

    ![Zkontrolujte znovu vygenerovat](./media/storsimple-8000-manage-service/regenregkey4.png)

4. Zobrazí se nový registrační klíč služby.

5. Zkopírujte tento klíč a uložit ho pro registraci nové zařízení s touto službou.



## <a name="change-hello-service-data-encryption-key"></a>Změna šifrovacího klíče dat služby hello
Šifrovací klíče dat služby jsou použité tooencrypt důvěrné zákaznická data, jako je například pověření účtu, úložiště, které jsou odesílány z zařízení StorSimple toohello služby StorSimple Manager. Budete potřebovat toochange tyto klíče pravidelně Pokud má vaše organizace IT zásad střídání klíče na zařízení úložiště hello. Hello změny klíče proces může být mírně lišit podle toho, jestli je jeden nebo více zařízení spravuje hello služby StorSimple Manager. Další informace, přejděte příliš[StorSimple zabezpečení a ochranu dat](storsimple-8000-security.md).

Změna šifrovacího klíče dat hello služby je proces, krok 3:

1. Pomocí skriptů prostředí Windows PowerShell pro Azure Resource Manager, autorizaci zařízení toochange hello šifrovacího klíče dat služby.
2. Pomocí Windows Powershellu pro StorSimple, zahajte hello služby dat šifrování klíče změnu.
3. Pokud máte více než jedno zařízení StorSimple, aktualizujte hello služby datového šifrovacího klíče na hello jiná zařízení.

### <a name="step-1-use-windows-powershell-script-tooauthorize-a-device-toochange-hello-service-data-encryption-key"></a>Krok 1: Použití prostředí Windows PowerShell skriptu tooAuthorize zařízení toochange hello šifrovacího klíče dat služby
Správce zařízení hello obvykle bude požadovat tohoto správce služeb hello autorizovat šifrovací klíče zařízení toochange služby data. Správce služeb Hello bude potom budete autorizovat hello zařízení toochange hello klíč.

Tento krok se provádí pomocí skriptu Azure Resource Manager na základě hello. Správce služeb Hello můžete vybrat zařízení, které je vhodné toobe oprávnění. Hello zařízení je pak šifrovacího klíče dat služby autorizovaný toostart hello změňte procesu. 

Další informace o používání skriptu hello přejděte příliš[autorizovat ServiceEncryptionRollover.ps1](https://github.com/anoobbacker/storsimpledevicemgmttools/blob/master/Authorize-ServiceEncryptionRollover.ps1)

#### <a name="which-devices-can-be-authorized-toochange-service-data-encryption-keys"></a>Zařízení, která může být oprávnění šifrovací klíče dat služby toochange?
Zařízení musí splňovat následující kritéria, aby mohla být autorizovaným tooinitiate služby dat šifrování klíče změny hello:

* Hello zařízení musí být online toobe vhodné pro službu data šifrování změny klíče autorizace.
* Můžete povolit hello stejného zařízení znovu po 30 minutách Pokud klíč hello změnit nebyla inicializována.
* Jiné zařízení, můžete povolit za předpokladu, že změny klíče hello nebyla inicializována hello dříve oprávněným zařízením. Po hello nové zařízení byla potvrzena, nelze staré zařízení hello zahajte změnu hello.
* Zařízení nejde autorizovat, když probíhá výměna šifrovacího klíče dat služby hello hello.
* Zařízení můžete povolit, když některé z hello zařízení registrovaná službou hello mít převracet hello šifrování, jiné mají není. 

### <a name="step-2-use-windows-powershell-for-storsimple-tooinitiate-hello-service-data-encryption-key-change"></a>Krok 2: Použití Windows Powershellu pro StorSimple tooinitiate hello služby šifrování klíče změny dat
Tento krok se provádí v hello Windows Powershellu pro StorSimple rozhraní na hello oprávnění zařízení StorSimple.

> [!NOTE]
> Žádné operace lze provést v hello portál Azure služby StorSimple Manager, dokud se nedokončí výměna klíče hello.
> 
> 

Pokud používáte hello zařízení konzoly sériového portu tooconnect toohello rozhraní Windows PowerShell, proveďte následující kroky hello.

#### <a name="tooinitiate-hello-service-data-encryption-key-change"></a>změna šifrovacího klíče dat služby tooinitiate hello
1. Vyberte možnost 1 toolog na s úplným přístupem.
2. Hello příkazového řádku zadejte:
   
     `Invoke-HcsmServiceDataEncryptionKeyChange`
3. Po úspěšném dokončení hello rutiny, zobrazí se nové šifrovacího klíče dat služby. Zkopírujte a uložte tento klíč pro použití v kroku 3 tohoto procesu. Tento klíč bude použit tooupdate všechny hello zbývající zařízení registrovaná službou StorSimple Manager hello.
   
   > [!NOTE]
   > Tento proces musí zahájit v rámci čtyři hodiny autorizace zařízení StorSimple.
   > 
   > 
   
   Tento nový klíč se pak odešlou toohello služby toobe nabídnutých tooall hello zařízení, které jsou registrovány hello služby. Na řídicím panelu služby hello se potom zobrazí výstrahu. Služba Hello zakáže všechny operace hello na hello zaregistrovat zařízení a Správce zařízení hello bude nutné tooupdate šifrovacího klíče dat služby hello na hello jiná zařízení. Ale hello vstupně-výstupních operací (odesílání dat v cloudu toohello hostitele) nesmí být přerušeny.
   
   Pokud máte jedno zařízení zaregistrován tooyour služby, proces výměny hello je nyní dokončen a hello další krok můžete vynechat. Pokud máte více zařízení registrovaná tooyour službu, pokračujte toostep 3.

### <a name="step-3-update-hello-service-data-encryption-key-on-other-storsimple-devices"></a>Krok 3: Aktualizace hello šifrovacího klíče dat služby na jiná zařízení StorSimple
Tyto kroky je potřeba provést v prostředí Windows PowerShell rozhraní hello zařízení StorSimple, pokud máte více služby StorSimple Manager tooyour registrovaných zařízení. Hello klíč, který jste získali v kroku 2 je třeba použít tooupdate, které všechny hello zbývající zaregistrována hello služby StorSimple Manager zařízení StorSimple.

Proveďte následující kroky tooupdate hello šifrování dat pro služby ve vašem zařízení hello.

#### <a name="tooupdate-hello-service-data-encryption-key"></a>šifrovací klíč dat služby tooupdate hello
1. Pomocí Windows Powershellu pro StorSimple tooconnect toohello konzolu. Vyberte možnost 1 toolog na s úplným přístupem.
2. Hello příkazového řádku zadejte:
   
    `Invoke-HcsmServiceDataEncryptionKeyChange – ServiceDataEncryptionKey`
3. Zadejte hello služby datový šifrovací klíč, který jste získali v [krok 2: použití Windows Powershellu pro StorSimple tooinitiate hello dat šifrování klíče změny služby](#to-initiate-the-service-data-encryption-key-change).


## <a name="next-steps"></a>Další kroky
* Další informace o hello [proces nasazení zařízení StorSimple](storsimple-8000-deployment-walkthrough-u2.md).
* Další informace o [Správa účtu úložiště StorSimple](storsimple-8000-manage-storage-accounts.md).
* Další informace o příliš[použití hello tooadminister service Manager zařízení StorSimple zařízení StorSimple](storsimple-8000-manager-service-administration.md).
