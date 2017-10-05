---
title: "Nasazení služby Správce zařízení StorSimple v Azure | Microsoft Docs"
description: "Vysvětluje, jak vytvářet a odstraňovat služby StorSimple Manager zařízení na portálu Azure a popisuje, jak spravovat registrační klíč služby."
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
ms.openlocfilehash: 22bb4a32f006d7e49356743c2a87eb622a61d18e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="deploy-the-storsimple-device-manager-service-for-storsimple-8000-series-devices"></a>Nasazení služby Správce zařízení StorSimple pro řadu zařízení StorSimple 8000

## <a name="overview"></a>Přehled

Služba Správce zařízení StorSimple běží v Microsoft Azure a připojí k více zařízení StorSimple. Po vytvoření služby, můžete ke správě všech zařízení, které jsou připojené ke službě StorSimple Manager zařízení z jedné centrální umístění, a současně minimalizujete její související administrativní zátěže.

Tento kurz popisuje kroky potřebné k vytváření, odstraňování, migrace služby a správu registrační klíč služby. Informace obsažené v tomto článku se vztahuje pouze na řadu zařízení StorSimple 8000. Další informace o pole virtuální zařízení StorSimple, přejděte na [nasazení služby StorSimple Manager zařízení pro vaše pole virtuální zařízení StorSimple](storsimple-virtual-array-manage-service.md).

## <a name="create-a-service"></a>Vytvoření služby
Postup vytvoření služby StorSimple Manager zařízení, musíte mít:

* Předplatné s smlouvu Enterprise Agreement
* Aktivní účet úložiště Microsoft Azure
* Fakturační informace, které se používá pro správu přístupu

Jsou povoleny pouze odběry ke smlouvě Enterprise. Microsoft Sponsorship odběry, které byly povoleny v portálu Azure classic nejsou podporovány na portálu Azure. Zobrazí se následující zpráva a při použití nepodporované předplatného:

![Předplatné není platný](./media/storsimple-8000-manage-service/subscription-not-valid.jpg)

Můžete také vygenerovat výchozí účet úložiště při vytváření služby.

Jeden služby můžete spravovat více zařízení. Zařízení, ale nemůžou zahrnovat víc služeb. Velký podnik může mít víc instancí služby pro práci s různých předplatných, organizace nebo i umístění nasazení. 

> [!NOTE]
> Je třeba samostatné instance služby StorSimple Manager zařízení ke správě zařízení řady StorSimple 8000 a pole virtuální zařízení StorSimple.

Proveďte následující kroky k vytvoření služby.

[!INCLUDE [storsimple-create-new-service](../../includes/storsimple-8000-create-new-service.md)]


Pro každou službu StorSimple Manager zařízení existují následující atributy:

* **Název** – název, který byl přiřazen při vytváření služby StorSimple Manager zařízení. **Název služby nelze změnit po vytvoření služby. To platí také pro ostatní entity, jako jsou zařízení, svazky, kontejnery svazků a zásady zálohování, které nelze přejmenovat na portálu Azure.**
* **Stav** – stav služby, která může být **Active**, **vytváření**, nebo **Online**.
* **Umístění** – zeměpisné umístění, ve kterém se nasadí zařízení StorSimple.
* **Předplatné** – fakturace předplatného, které je přidružené k vaší službě.

## <a name="move-a-service-to-azure-portal"></a>Přesuňte službu na portálu Azure
Řady StorSimple 8000 je nyní spravovat na portálu Azure. Pokud máte existující službu ke správě zařízení StorSimple, doporučujeme přesunout služby do portálu Azure. Klasický portál Azure pro službu StorSimple Manager není k dispozici po 30 září 2017.

Možnost migrace na web Azure Portal je k dispozici ve fázích. Pokud nevidíte možnost migrace na web Azure Portal, ale chtěli byste ji využít a už jste prozkoumali dopady migrace popsané v tématu věnovaném [aspektům přechodu](#considerations-for-transition), můžete [poslat žádost](https://aka.ms/ss8000-cx-signup).

### <a name="considerations-for-transition"></a>Důležité informace týkající se přechodu

Zkontrolujte dopad migrace na nový portál Azure, před přesunutím službu.

#### <a name="before-you-transition"></a>Před přechod

* Vaše zařízení používá aktualizace 3.0 nebo novější. Pokud vaše zařízení běží starší verze, nainstalujte nejnovější aktualizace. Další informace, přejděte na [nainstalovat aktualizace 4](storsimple-8000-install-update-4.md). Pokud používáte zařízení s StorSimple cloudu (8010/8020), vytvořte nové zařízení cloudu s Update 4.0. 

* Jakmile kvůli na nový portál Azure nelze použít klasický portál Azure ke správě zařízení StorSimple.

* Přechod je omezovaly a nedojde k žádnému výpadku pro zařízení.

* Kvůli všechny správce zařízení StorSimple v rámci zadaného předplatného.

#### <a name="during-the-transition"></a>Během přechodu

* Nelze spravovat vaše zařízení z portálu.
* Operace, například vrstvení a naplánované zálohování pokračovat dojít.
* Neodstraňujte staré Správci zařízení StorSimple, když probíhá přechod.

#### <a name="after-the-transition"></a>Po přechodu

* Zařízení již nebude možné spravovat na klasickém portálu.

* Stávající rutiny prostředí PowerShell Azure Service Management (ASM) nejsou podporovány. Skripty ke správě svých zařízení prostřednictvím Správce Azure Resource Manager aktualizujte.

* Konfiguraci služby a zařízení zůstanou zachovány. Všechny svazky a zálohy jsou také přešla na portál Azure.

### <a name="begin-transition"></a>Začněte přechodu

Proveďte následující kroky přechodu služby k portálu Azure.

1. Přejděte na stávající služby StorSimple Manager na portálu classic.

2. Zobrazí oznámení, že informuje, že služby StorSimple Manager zařízení je nyní k dispozici na portálu Azure. Všimněte si, že se na portálu Azure, služby se označuje jako služba Manager zařízení StorSimple.

    ![Migrace oznámení](./media/storsimple-8000-manage-service/service-transition1.jpg)

    1. Ujistěte se, že jste si přečetli úplné dopad migrace.
    2. Projděte si seznam správců zařízení StorSimple, který budou přesunuty z klasického portálu.

3. Klikněte na tlačítko **migrovat**. Přechod spustí a trvá několik minut na dokončení.

Po dokončení přechodu můžete spravovat zařízení prostřednictvím služby StorSimple Manager zařízení na portálu Azure.

Na portálu Azure jsou podporovány pouze zařízení StorSimple se systémem aktualizace 3.0 a vyšší. Zařízení, která se používají starší verze mít omezenou podporu. Následující tabulka summrizes operací, které jsou podporovány na zařízení spuštěný versios před aktualizací 3.0 po provedení migrace z klasického na portál Azure.

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
| Klonovat na zařízení se systémem aktualizace 3.0 a novějších <br> Zdrojového zařízení používá verzi před aktualizací 3.0.                                | Ano            |
| Klonovat na zařízení se systémem verze starší než Update 3.0                                                                          | Ne             |
| Převzetí služeb při selhání jako zdrojového zařízení <br> (ze zařízení používá verzi před 3.0 aktualizace do zařízení se systémem aktualizace 3.0 a novější)                                                               | Ano            |
| Převzetí služeb při selhání jako cílové zařízení <br> (pro zařízení používající verzi softwaru před aktualizací 3.0)                                                                                   | Ne             |
| Vymazat výstrahu                                                                                                                  | Ano            |
| Zobrazit zásady zálohování, zálohování katalog, svazky, kontejnery svazků, monitorování grafy, úlohy a výstrahy vytvořené v portálu classic | Ano            |
| Zapnutí a vypnutí řadiče zařízení                                                                                              | Ano            |


## <a name="delete-a-service"></a>Odstranění služby

Před odstraněním služby, ujistěte se, že žádné připojená zařízení ji používají. Pokud služba se používá, deaktivujte připojená zařízení. Operaci deaktivovat severu připojení mezi zařízením a služby, ale zachovat data zařízení v cloudu.

> [!IMPORTANT]
> Po odstranění služby nelze vrátit operaci zpět. Jakékoli zařízení, které se pomocí služby musí být resetovat do výchozího předtím, než se dá použít s jinou službu. V tomto scénáři bude ztracena, místní data na zařízení a také v konfiguraci.

Proveďte následující kroky pro odstranění služby.

### <a name="to-delete-a-service"></a>Chcete-li odstranit služby

1. Vyhledejte službu, kterou chcete odstranit. Klikněte na tlačítko **prostředky** ikonu a potom zadejte příslušné podmínky pro vyhledávání. Ve výsledcích hledání klikněte na službu, kterou chcete odstranit.

    ![Chcete-li odstranit vyhledávací služba](./media/storsimple-8000-manage-service/deletessdevman1.png)

2. Tím přejdete do okna služby StorSimple Manager zařízení. Klikněte na **Odstranit**.

    ![Odstranění služby](./media/storsimple-8000-manage-service/deletessdevman2.png)

3. Klikněte na tlačítko **Ano** v potvrzení oznámení. To může trvat několik minut, než službu, kterou chcete odstranit.

    ![Potvrzení odstranění](./media/storsimple-8000-manage-service/deletessdevman3.png)

## <a name="get-the-service-registration-key"></a>Získání registračního klíče služby

Po úspěšném vytvoření služby musíte registrace zařízení StorSimple pomocí služby. K registraci svého prvního zařízení StorSimple, budete potřebovat registrační klíč služby. Chcete-li zaregistrovat další zařízení s existující službu StorSimple, je třeba registrační klíč a služby datového šifrovacího klíče (což je vygenerovaný na první zařízení během registrace). Další informace o šifrovacího klíče dat služby najdete v tématu [zabezpečení zařízení StorSimple](storsimple-8000-security.md). Abyste mohli získat registrační klíč přístup k **klíče** v okně vaší Správce zařízení StorSimple.

Proveďte následující kroky k získání registračního klíče služby.

[!INCLUDE [storsimple-8000-get-service-registration-key](../../includes/storsimple-8000-get-service-registration-key.md)]

Zachovat registrační klíč služby do bezpečného umístění. Budete potřebovat tento klíč, jakož i služby datový šifrovací klíč k registraci dalších zařízení s touto službou. Po získání registračního klíče služby, je nutné nakonfigurovat zařízení pomocí Windows Powershellu pro StorSimple rozhraní.

Podrobnosti o tom, jak používat tento registrační klíč najdete v tématu [krok 3: Konfigurace a registrace zařízení pomocí Windows Powershellu pro StorSimple](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

## <a name="regenerate-the-service-registration-key"></a>Znovu vygenerovat registrační klíč služby
Budete muset znovu vygenerovat registrační klíč služby, pokud je nutné provést střídání klíče, nebo pokud došlo ke změně seznamu správců služeb. Pokud jste znovu vygenerovat klíč, nový klíč slouží pouze k registraci dalších zařízení. Zařízení, která již byla zaregistrována nejsou ovlivněny tímto procesem.

Proveďte následující kroky k opětovnému vytvoření registrační klíč služby.

### <a name="to-regenerate-the-service-registration-key"></a>Chcete-li znovu vygenerovat registrační klíč služby
1. V **Manager zařízení StorSimple** okno, přejděte na **správy &gt;**  **klíče**.
    
    ![Okno Klíče](./media/storsimple-8000-manage-service/regenregkey2.png)

2. V **klíče** okně klikněte na tlačítko **znovu vygenerovat**.

    ![Klikněte na tlačítko znovu vygenerovat](./media/storsimple-8000-manage-service/regenregkey3.png)
3. V **registrační klíč znovu generovat služby** okno, zkontrolujte akce vyžadována, je-li se znovu vygenerovat klíče. Další zařízení, které jsou registrované s touto službou pomocí nového registračního klíče. Klikněte na tlačítko **znovu vygenerovat** k potvrzení. Upozornění se zobrazí po dokončení opětovné vygenerování.

    ![Zkontrolujte znovu vygenerovat](./media/storsimple-8000-manage-service/regenregkey4.png)

4. Zobrazí se nový registrační klíč služby.

5. Zkopírujte tento klíč a uložit ho pro registraci nové zařízení s touto službou.



## <a name="change-the-service-data-encryption-key"></a>Změna šifrovacího klíče dat služby
Šifrovací klíče dat služby slouží k šifrování dat důvěrné zákazníka, jako je například přihlašovací údaje účtu úložiště, které se odesílají ze služby StorSimple Manager zařízení StorSimple. Je potřeba pravidelně měnit tyto klíče, pokud má vaše organizace IT zásad střídání klíče na zařízení úložiště. Proces změny klíče může být mírně lišit podle toho, jestli je jednoho zařízení nebo více zařízení spravovaná pomocí služby StorSimple Manager. Další informace, přejděte na [StorSimple zabezpečení a ochranu dat](storsimple-8000-security.md).

Změna šifrovacího klíče dat služby je proces, krok 3:

1. Pomocí skriptů prostředí Windows PowerShell pro Azure Resource Manager, autorizace zařízení, které chcete změnit šifrovacího klíče dat služby.
2. Pomocí Windows Powershellu pro StorSimple, zahajte změnu klíče šifrování dat služby.
3. Pokud máte více než jedno zařízení StorSimple, aktualizujte šifrovacího klíče dat služby na jiných zařízení.

### <a name="step-1-use-windows-powershell-script-to-authorize-a-device-to-change-the-service-data-encryption-key"></a>Krok 1: Použití prostředí Windows PowerShell skriptu k autorizaci zařízení, které chcete změnit šifrovacího klíče dat služby
Obvykle Správce zařízení bude požadovat, aby správce služeb autorizace zařízení, které chcete změnit šifrovací klíče dat služby. Správce služeb bude potom budete autorizovat zařízení změňte klíč.

Tento krok se provádí pomocí Azure Resource Manager na základě skriptu. Správce služeb můžete vybrat zařízení, které vás opravňuje k autorizaci. Zařízení je pak oprávnění ke spuštění procesu změny klíče šifrování dat služby. 

Další informace o používání skriptu, přejděte na [autorizovat ServiceEncryptionRollover.ps1](https://github.com/anoobbacker/storsimpledevicemgmttools/blob/master/Authorize-ServiceEncryptionRollover.ps1)

#### <a name="which-devices-can-be-authorized-to-change-service-data-encryption-keys"></a>Zařízení, která lze udělit oprávnění k změnit šifrovací klíče dat služby?
Zařízení musí splňovat následující kritéria, než může být oprávnění k inicializaci služby dat šifrování klíče změny:

* Zařízení musí být online způsobilé k autorizaci změny klíče šifrování dat služby.
* Znovu po 30 minutách může autorizovat do stejného zařízení, pokud změny klíče nebyla inicializována.
* Jiné zařízení, můžete povolit za předpokladu, že změny klíče nebyla inicializována dříve oprávněným zařízením. Po autorizoval nové zařízení stará zařízení nelze zahájit změnu.
* Zařízení nejde autorizovat, když probíhá výměna šifrovacího klíče dat služby.
* Zařízení můžete povolit, pokud některá zařízení zaregistrovaná ve službě service mít převracet šifrování, jiné mají není. 

### <a name="step-2-use-windows-powershell-for-storsimple-to-initiate-the-service-data-encryption-key-change"></a>Krok 2: Použití Windows Powershellu pro StorSimple zahájíte změny klíče šifrování dat služby
Tento krok se provádí v prostředí Windows PowerShell pro rozhraní StorSimple na autorizované zařízení StorSimple.

> [!NOTE]
> Žádné operace lze provést na webu Azure portal služby StorSimple Manager, dokud se nedokončí výměny klíčů.
> 
> 

Pokud používáte konzole sériového portu zařízení pro připojení k rozhraní Windows PowerShell, proveďte následující kroky.

#### <a name="to-initiate-the-service-data-encryption-key-change"></a>Zahajte změnu klíče šifrování dat služby
1. Výběrem možnosti 1 přihlaste s úplným přístupem.
2. Na příkazovém řádku zadejte:
   
     `Invoke-HcsmServiceDataEncryptionKeyChange`
3. Po úspěšném dokončení rutiny, zobrazí se nové šifrovacího klíče dat služby. Zkopírujte a uložte tento klíč pro použití v kroku 3 tohoto procesu. Tento klíč se používá k aktualizaci všech zbývajících zařízení registrovaný ve službě StorSimple Manager.
   
   > [!NOTE]
   > Tento proces musí zahájit v rámci čtyři hodiny autorizace zařízení StorSimple.
   > 
   > 
   
   Tento nový klíč je odeslaný do služby k vloží do všech zařízení, která jsou zaregistrovaná ve službě service. Na řídicím panelu služby se potom zobrazí výstrahu. Službu vypne všechny operace na registrovaná zařízení a pak muset aktualizovat šifrovacího klíče dat služby na jiných zařízení Správce zařízení. Ale nesmí být přerušeny vstupně-výstupních operací (odesílání dat do cloudu hostitelů).
   
   Pokud máte jedno zařízení zaregistrován ke službě, proces výměny je nyní dokončen a můžete přejít na další krok. Pokud máte více zařízení zaregistrován ke službě, pokračujte krokem 3.

### <a name="step-3-update-the-service-data-encryption-key-on-other-storsimple-devices"></a>Krok 3: Aktualizace šifrovacího klíče dat služby na jiná zařízení StorSimple
Tyto kroky je potřeba provést v prostředí Windows PowerShell rozhraní zařízení StorSimple, pokud máte více zařízení zaregistrován ke službě StorSimple Manager. Klíč, který jste získali v kroku 2 musíte použít k aktualizaci všech zbývajících zařízení StorSimple registrovaný ve službě StorSimple Manager.

Proveďte následující kroky k aktualizaci šifrování dat služby ve vašem zařízení.

#### <a name="to-update-the-service-data-encryption-key"></a>Chcete-li aktualizovat šifrovacího klíče dat služby
1. Pomocí Windows Powershellu pro StorSimple pro připojení ke konzole. Výběrem možnosti 1 přihlaste s úplným přístupem.
2. Na příkazovém řádku zadejte:
   
    `Invoke-HcsmServiceDataEncryptionKeyChange – ServiceDataEncryptionKey`
3. Zadejte šifrovací klíč dat služby, který jste získali v [krok 2: použití Windows Powershellu pro StorSimple zahájíte změny klíče šifrování dat služby](#to-initiate-the-service-data-encryption-key-change).


## <a name="next-steps"></a>Další kroky
* Další informace o [proces nasazení zařízení StorSimple](storsimple-8000-deployment-walkthrough-u2.md).
* Další informace o [Správa účtu úložiště StorSimple](storsimple-8000-manage-storage-accounts.md).
* Další informace o tom, jak [použít službu StorSimple Manager zařízení ke správě zařízení StorSimple](storsimple-8000-manager-service-administration.md).
