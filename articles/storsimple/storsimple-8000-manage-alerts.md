---
title: "aaaView a spravovat výstrahy pro zařízení řady StorSimple 8000 | Microsoft Docs"
description: "Popisuje výstrahy podmínek StorSimple a závažnosti, jak tooconfigure výstrahy oznámení a jak toouse hello Správce zařízení StorSimple služby toomanage výstrahy."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/22/2017
ms.author: alkohli
ms.openlocfilehash: fdd1a911ceff542bc6bdeae877e1f0fc381bf27c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-tooview-and-manage-storsimple-alerts"></a>Použijte tooview služby StorSimple Manager zařízení hello a Správa výstrah StorSimple

## <a name="overview"></a>Přehled

Hello **výstrahy** okno v hello služby StorSimple Manager zařízení poskytuje způsob pro vás tooreview a zrušte zaškrtnutí StorSimple výstrahy související s zařízení na základě v reálném čase. V tomto okně můžete centrálně monitorovat problémy v oblasti stavu hello zařízení StorSimple a hello celkového řešení Microsoft Azure StorSimple.

Tento kurz popisuje běžné výstrahy podmínek, úrovně závažnosti výstrah a jak tooconfigure oznamováním výstrah. Kromě toho zahrnuje výstrahy stručnou referenční tabulky, které umožňují tooquickly můžete vyhledat konkrétní výstrahu a reagují odpovídajícím způsobem.

![stránka výstrah](./media/storsimple-8000-manage-alerts/configure-alerts-email11.png)

## <a name="common-alert-conditions"></a>Obecné podmínky výstrah

Zařízení StorSimple generuje výstrahy v odpovědi tooa řadu podmínek. Hello následují hello nejčastějších typů výstrah podmínek:

* **Problémy s hardwarem** – tyto výstrahy vás informovat o hello stav hardwaru. Umožňují vám vědět, pokud jsou potřeba upgrady firmwaru, pokud rozhraní sítě má problémy nebo pokud došlo k potížím s jedním z datových jednotkách.
* **Problémy s připojením k** – tyto výstrahy dojít, když se potíže při přenosu dat. Problémy s komunikací může dojít během přenosu dat tooand z účtu úložiště Azure hello nebo splatnosti toolack připojení mezi hello zařízení a služby StorSimple Manager zařízení hello. Problémy s komunikací jsou některé hello nejtěžší toofix, protože nejsou k dispozici mnoho body selhání. Měli byste vždy nejprve ověřit, že připojení k síti a přístup k Internetu jsou k dispozici před pokračováním na toomore potíží pro pokročilé uživatele. Nápovědu k řešení potíží, přejděte příliš[Poradce při potížích s rutiny Test-Connection hello](storsimple-troubleshoot-deployment.md).
* **Problémy s výkonem** – tyto výstrahy jsou nastat, když systém nepracuje optimálně, například když je v případě velkého zatížení.

Kromě toho může zobrazit výstrahy související toosecurity, aktualizací nebo selhání úlohy.

## <a name="alert-severity-levels"></a>Úrovně závažnosti výstrah

Výstrahy mají různé úrovně závažnosti, v závislosti na hello vlivu, který hello výstrahy situaci bude mít a hello potřebu toohello upozornění na odpověď. jsou Hello úrovně závažnosti:

* **Kritické** – Tato výstraha je v odpovědi tooa podmínku, která ovlivňuje hello úspěšné výkon systému. Akce je požadovaná tooensure že hello StorSimple služba není přerušena.
* **Upozornění** – tento stav může být důležité, pokud není vyřešené. Doporučujeme prozkoumat hello situaci a trvat jakýkoli problém hello tooclear je vyžadována akce.
* **Informace o** – Tato výstraha obsahuje informace, které mohou být užitečné při sledování a správa systému.

## <a name="configure-alert-settings"></a>Konfigurace nastavení výstrah

Můžete zvolit, jestli chcete toobe oznámení e-mailové výstrahy podmínek pro každý z vašich zařízení StorSimple. Kromě toho můžete identifikovat ostatní příjemci oznámení výstrah tak, že zadáte jejich e-mailové adresy hello **ostatní příjemci e-mailu** pole, oddělené středníky.

> [!NOTE]
> Můžete zadat maximálně 20 e-mailové adresy na jedno zařízení.

Jakmile povolíte e-mailové oznámení pro zařízení, členem seznam oznámení hello obdrží e-mailovou zprávu, dojde k kritickou výstrahu pokaždé, když. odešle zprávy Hello z  *storsimple-alerts-noreply@mail.windowsazure.com*  a popíše hello podmínka pro upozornění. Můžete kliknout na příjemci **Unsubscribe** tooremove sami hello e-mailové oznámení seznamu.

#### <a name="tooenable-email-notification-of-alerts-for-a-device"></a>tooenable e-mailové oznámení výstrah pro zařízení.
1. Přejděte služby StorSimple Manager zařízení tooyour. Hello seznam zařízení vyberte a klikněte na zařízení hello chcete tooconfigure.
2. Přejděte příliš**nastavení** > **Obecné** hello zařízení.

   ![Okno výstrahy](./media/storsimple-8000-manage-alerts/configure-alerts-email2.png)
   
2. V hello **obecné nastavení** okně přejděte příliš**výstrahy nastavení** a nastavte následující hello:
   
   1. V hello **odesílání e-mailové oznámení** pole, vyberte **Ano**.
   2. V hello **e-mailem správci služeb** pole, vyberte **Ano** Správce služeb hello toohave a všechny spolusprávci dostávat oznámení o výstrahách hello.
   3. V hello **ostatní příjemci e-mailu** zadejte hello e-mailové adresy všech příjemců, kterým mají být doručena oznámení o výstrahách hello. Zadejte názvy ve formátu hello  *someone@somewhere.com* . Použijte středníky tooseparate hello e-mailové adresy. Můžete nakonfigurovat maximálně 20 e-mailové adresy na jedno zařízení. 
      
3. toosend testovací e-mailová oznámení, klikněte na tlačítko **odeslat zkušební e-mail**. Hello služby StorSimple Manager zařízení se zobrazí stavové zprávy, jak předává hello testovací oznámení.

    ![nastavení výstrah](./media/storsimple-8000-manage-alerts/configure-alerts-email3.png)

4. Zobrazí upozornění, když hello testovací e-mail je odeslán. 
   
    ![Výstrahy testování odeslaných e-mailové oznámení](./media/storsimple-8000-manage-alerts/configure-alerts-email4.png)
   
   > [!NOTE]
   > Pokud hello testovací zpráva oznámení nelze odeslat, služby StorSimple Manager zařízení hello se zobrazí příslušná chybová zpráva. Počkejte několik minut a akci opakujte toosend vaše testovací zpráva oznámení. 

5. Po dokončení konfigurace hello, klikněte na tlačítko **Uložit**. Po zobrazení výzvy k potvrzení klikněte na **Ano**.

     ![Výstrahy testování odeslaných e-mailové oznámení](./media/storsimple-8000-manage-alerts/configure-alerts-email5.png)

## <a name="view-and-track-alerts"></a>Zobrazení a sledovat výstrahy

Souhrn okno služby Hello Správce zařízení StorSimple poskytuje stručný přehled hello počet výstrah v zařízeních, uspořádané podle úrovně závažnosti.

![Řídicí panel výstrahy](./media/storsimple-8000-manage-alerts/device-summary4.png)

Kliknutím na úroveň závažnosti hello otevře hello **výstrahy** okno. Hello výsledky obsahovat pouze hello výstrahy, které odpovídají této úrovně závažnosti.

Kliknutím na výstrahu v seznamu hello vám poskytne další podrobnosti pro hello výstrahu, včetně hello poslední čas hello upozornění ohlásil, hello počet výskytů hello výstrahy na hello zařízení a hello Doporučená akce tooresolve hello výstrahy. Pokud je výstraha hardwaru, bude rovněž určit hello hardwarových součástí.

![Příklad oznámení hardwaru](./media/storsimple-8000-manage-alerts/configure-alerts-email14.png)

Pokud potřebujete toosend hello informace tooMicrosoft podpory můžete zkopírovat hello podrobnosti výstrahy tooa textového souboru. Po a potom hello doporučení a vyřešit hello podmínka pro upozornění na místě, byste měli vymazat hello výstrahy ze zařízení hello výběrem hello upozornění v hello **výstrahy** okno a kliknutím na **vymazat**. tooclear více výstrah, vyberte všechny výstrahy, klikněte na možnost žádný sloupec s výjimkou hello **výstraha** sloupec a pak klikněte na tlačítko **zrušte** po výběru všechny hello výstrahy toobe vymazán. Mějte na paměti některé výstrahy jsou po vyřešení problému hello nebo když systém hello se novými informacemi aktualizují hello výstrahy automaticky vymazány.

Když kliknete na tlačítko **zrušte**, budete mít hello možnost tooprovide komentáře o výstraze hello a kroky hello následovala tooresolve hello problém. Některé události budou vymazána systémem hello, pokud jiná událost se aktivuje se novými informacemi. V takovém případě se zobrazí následující zprávu hello.

![Vymazat výstrahu](./media/storsimple-manage-alerts/admin_alerts_system_clear.png)

## <a name="sort-and-review-alerts"></a>Řazení a zkontrolujte výstrahy

Možná bude efektivnější toorun sestavy výstrah tak, aby můžete zkontrolovat a poté je smažte ve skupinách. Kromě toho hello **výstrahy** až too250 výstrahy můžete zobrazit okno. Pokud byl překročen počet výstrah, zobrazí se všechny výstrahy v hello výchozí zobrazení. Zkombinováním hello toocustomize pole, které výstrahy se zobrazují následující:

* **Stav** – můžete zobrazit buď **Active** nebo **nezaškrtnuto** výstrahy. Aktivní výstrahy se stále aktivují v systému, zatímco nezaškrtnuté výstrahy byly buď ručně vymazat správcem nebo prostřednictvím kódu programu vymazat, protože podmínka pro upozornění hello aktualizovat systém hello se novými informacemi.
* **Závažnost** – můžete zobrazit výstrahy všechny úrovně závažnosti (kritická, upozornění, informace o) nebo pouze určité závažnosti, například pouze kritické výstrahy.
* **Zdroj** – můžete zobrazit výstrahy ze všech zdrojů nebo omezit toothose hello výstrahy, které pocházejí z buď hello služby nebo jednoho nebo všech zařízení hello.
* **Čas rozsah** – zadáním hello **z** a **k** kalendářních dat a časových razítek, můžete se podívat na výstrahy během hello časové období, které vás zajímají.

![Seznam výstrah](./media/storsimple-8000-manage-alerts/configure-alerts-email11.png)

## <a name="alerts-quick-reference"></a>Stručná referenční příručka výstrahy

Následující tabulky Hello seznam některých hello Microsoft Azure StorSimple výstrahy, které se mohou vyskytnout, a také další informace a doporučení tam, kde je k dispozici. Výstrahy zařízení StorSimple spadat do jednoho z následujících kategorií hello:

* [Výstrahy připojení cloudu](#cloud-connectivity-alerts)
* [Výstrahy clusteru](#cluster-alerts)
* [Výstrahy pro zotavení po havárii](#disaster-recovery-alerts)
* [Výstrahy hardwaru](#hardware-alerts)
* [Upozornění na selhání úlohy](#job-failure-alerts)
* [Místně vázaný svazek výstrahy](#locally-pinned-volume-alerts)
* [Výstrahy sítě](#networking-alerts)
* [Výstrahy výkonu](#performance-alerts)
* [Výstrahy zabezpečení](#security-alerts)
* [Podpora balíček výstrah](#support-package-alerts)

### <a name="cloud-connectivity-alerts"></a>Výstrahy připojení cloudu

| Textu výstrahy | Událost | Další informace / doporučené akce |
|:--- |:--- |:--- |
| Připojení k příliš <*název přihlašovacího údaje cloudu*> nelze navázat. |Účet úložiště toohello se nemůže připojit. |Zdá se, může být problém s připojením s vaším zařízením. Spusťte hello `Test-HcsmConnection` rutiny z hello rozhraní Windows Powershellu pro StorSimple na vašem zařízení tooidentify a hello problém opravte. Pokud jsou hello nastavení správná, může být hello problém s přihlašovacími údaji hello účtu úložiště hello, pro který byla výstraha hello vyvolána. V takovém případě použijte hello `Test-HcsStorageAccountCredential` rutiny toodetermine Pokud dochází k potížím, které lze vyřešit.<ul><li>Zkontrolujte nastavení sítě.</li><li>Zkontrolujte své přihlašovací údaje účtu úložiště.</li></ul> |
| Jsme neobdrželi prezenční signál z vašeho zařízení z hlediska hello poslední <*číslo*> minut. |Nelze se připojit toodevice. |Zdá se, nastane problém s připojením s vaším zařízením. Použijte prosím hello `Test-HcsmConnection` rutiny z hello rozhraní Windows Powershellu pro StorSimple na vašem zařízení tooidentify a vyřešte problém hello nebo se obraťte na správce sítě. |

### <a name="storsimple-behavior-when-cloud-connectivity-fails"></a>StorSimple chování při selhání připojení cloudu

Co se stane, pokud se nezdaří cloudu připojení zařízení StorSimple spuštění v produkčním prostředí?

Pokud cloudové připojení selže na produkční zařízení StorSimple, pak v závislosti na stavu hello vašeho zařízení hello následující se může objevit:

* **Pro hello místní data na zařízení**: po určitou dobu, bude bez přerušení a čtení bude pokračovat toobe zpracovat. Jako hello počet nezpracovaných vstupně-výstupních zvyšuje, překračuje limit, může spustit hello čtení však toofail.

    V závislosti na hello množství dat na zařízení, hello zápis bude také pokračovat toooccur pro hello první několik hodin po přerušení hello hello cloudu připojení. zápisy Hello se pak zpomalit a nakonec spusťte toofail, pokud připojení cloudu hello dojde k narušení po několik hodin. (Na hello zařízení pro data, která je toobe nabídnutých toohello cloudu není dočasné úložiště. Tato oblast vyprazdňuje při odesílání dat hello. V případě selhání připojení dat v této oblasti úložiště nebude nabídnutých toohello cloudu, a vstupně-výstupní operace se nezdaří.)
* **Pro data hello v cloudu hello**: pro většinu cloudu k chybám připojení, je vrácena chyba. Po obnovení připojení hello hello IOs nedojde k přerušení bez nutnosti toobring hello svazek online uživatele hello. Ve výjimečných případech může být zásahu uživatele požadované toobring back hello svazek online z hello portálu Azure.
* **Pro cloudové snímky v průběhu**: hello zopakován několikrát v rámci 4 až 5 hodin a hello připojení není obnovena, se nezdaří hello cloudových snímků.

### <a name="cluster-alerts"></a>Výstrahy clusteru

| Textu výstrahy | Událost | Další informace / doporučené akce |
|:--- |:--- |:--- |
| Zařízení převzít služby při selhání příliš <*název zařízení*>. |Zařízení je v režimu údržby. |Zařízení se nezdařila v důsledku tooentering nebo existujícímu režimu údržby. To je normální a není vyžadována žádná akce. Po této výstrahy mít potvrzené, zrušte jeho zaškrtnutí ze stránky hello výstrahy. |
| Zařízení převzít služby při selhání příliš <*název zařízení*>. |Právě bylo aktualizováno firmwaru zařízení nebo softwaru. |Došlo clusteru převzetí služeb při selhání kvůli tooan aktualizace. To je normální a není vyžadována žádná akce. Po této výstrahy mít potvrzené, zrušte jeho zaškrtnutí ze stránky hello výstrahy. |
| Zařízení převzít služby při selhání příliš <*název zařízení*>. |Řadič vypnout nebo restartovat. |Zařízení při selhání, protože řadič active hello byl vypnutý nebo restartovaný správcem. Není vyžadována žádná akce. Po této výstrahy mít potvrzené, zrušte jeho zaškrtnutí ze stránky hello výstrahy. |
| Zařízení převzít služby při selhání příliš <*název zařízení*>. |Plánované převzetí služeb při selhání. |Ověřte, že bylo plánované převzetí služeb při selhání. Po přijetí příslušné akce, zrušte tuto výstrahu ze stránky hello výstrahy. |
| Zařízení převzít služby při selhání příliš <*název zařízení*>. |Neplánované převzetí služeb při selhání. |StorSimple vychází tooautomatically obnovit z neplánované převzetí služeb při selhání. Pokud se zobrazí velký počet tyto výstrahy, obraťte se na Microsoft Support. |
| Zařízení převzít služby při selhání příliš <*název zařízení*>. |Jiné nebo neznámé příčina. |Pokud se zobrazí velký počet tyto výstrahy, obraťte se na Microsoft Support. Po hello problém nebude vyřešen, zrušte tuto výstrahu ze stránky hello výstrahy. |
| Služba kritické zařízení hlásí stav jako neúspěšná. |DataPath selhání služby. |Požádejte o pomoc Microsoft Support. |
| Virtuální IP adresy pro síťové rozhraní <*DATA #*> oznámení stavu jako neúspěšná. |Jiné nebo neznámé příčina. |Někdy dočasné stavy může způsobit, že tyto výstrahy. Pokud se jedná o případ hello, pak tato výstraha bude automaticky vymazán po určité době. Pokud hello potíže potrvají, obraťte se na Microsoft Support. |
| Virtuální IP adresy pro síťové rozhraní <*DATA #*> oznámení stavu jako neúspěšná. |Název rozhraní: <*DATA #*> IP adresu <IP address> nelze do online režimu, protože byla zjištěna duplicitní IP adresu v síti hello. |Ujistěte, zda hello duplicitní IP adresa je odebrán ze sítě hello nebo překonfigurujte hello rozhraní s jinou IP adresu. |

### <a name="disaster-recovery-alerts"></a>Výstrahy pro zotavení po havárii

| Textu výstrahy | Událost | Další informace / doporučené akce |
|:--- |:--- |:--- |
| Operace obnovení nelze obnovit všechna hello nastavení pro tuto službu. Data konfigurace zařízení je v nekonzistentním stavu pro některá zařízení. |Data byla zjištěna nekonzistence po zotavení po havárii. |Šifrovaná data ve službě hello není synchronizován s, na zařízení hello. Autorizace zařízení hello <*název zařízení*> z procesu synchronizace hello toostart Správce zařízení StorSimple. Hello pomocí rozhraní Windows Powershellu pro StorSimple toorun hello `Restore-HcsmEncryptedServiceData` na zařízení <*název zařízení*> rutiny poskytování hello staré heslo jako vstupní toothis profil zabezpečení hello toorestore rutiny. Spusťte hello `Invoke-HcsmServiceDataEncryptionKeyChange` rutiny tooupdate hello služby datový šifrovací klíč. Po přijetí příslušné akce, zrušte tuto výstrahu ze stránky hello výstrahy. |

### <a name="hardware-alerts"></a>Výstrahy hardwaru

| Textu výstrahy | Událost | Další informace / doporučené akce |
|:--- |:--- |:--- |
| Hardwarová součást <*ID součásti*> oznámení stavu jako <*stav*>. | |Někdy dočasné stavy může způsobit, že tyto výstrahy. Pokud ano, tato výstraha bude automaticky vymazán po určité době. Pokud hello potíže potrvají, obraťte se na Microsoft Support. |
| Chybně fungující pasivní řadiče. |Hello pasivní (sekundární) řadič nefunguje. |Zařízení je funkční, ale jeden z vašich řadičů nepracuje správně. Zkuste restartovat kontroleru. Pokud hello potíže nevyřeší, obraťte se na Microsoft Support. |

### <a name="job-failure-alerts"></a>Upozornění na selhání úlohy

| Textu výstrahy | Událost | Další informace / doporučené akce |
|:--- |:--- |:--- |
| Zálohování <*ID skupiny svazku zdroje*> se nezdařilo. |Úloha zálohování se nezdařila. |Problémy s připojením k může bránit operace zálohování hello z úspěšné dokončení. Pokud nejsou žádné problémy s připojením, pravděpodobně bylo dosaženo maximální počet záloh hello. Odstraňte všechny zálohy, které již nejsou potřebné a opakujte operaci hello. Po přijetí příslušné akce, zrušte tuto výstrahu ze stránky hello výstrahy. |
| Klonovat z <*zdroje ID zálohování prvků*> příliš <*cílový svazek sériová čísla*> se nezdařilo. |Úloha klonování se nezdařila. |Zálohování seznamu tooverify hello aktualizace, která hello zálohování je stále platný. Pokud je záloha hello platná, je možné, že problémy s připojením k cloudu brání úspěšně dokončení operace klonování hello. Pokud nejsou žádné problémy s připojením, pravděpodobně bylo dosaženo limitu úložiště hello. Odstraňte všechny zálohy, které již nejsou potřebné a opakujte operaci hello. Po přijetí příslušné akce tooresolve hello problém, zrušte tuto výstrahu ze stránky hello výstrahy. |
| Obnovení z <*zdroje ID zálohování prvků*> se nezdařilo. |Obnovení úlohy se nezdařilo. |Zálohování seznamu tooverify hello aktualizace, která hello zálohování je stále platný. Pokud hello zálohování je platný, je možné, že potíže s připojením k cloudu brání úspěšně dokončení operace obnovení hello. Pokud nejsou žádné problémy s připojením, pravděpodobně bylo dosaženo limitu úložiště hello. Odstraňte všechny zálohy, které již nejsou potřebné a opakujte operaci hello. Po přijetí příslušné akce tooresolve hello problém, zrušte tuto výstrahu ze stránky hello výstrahy. |

### <a name="locally-pinned-volume-alerts"></a>Místně vázaný svazek výstrahy

| Textu výstrahy | Událost | Další informace / doporučené akce |
|:--- |:--- |:--- |
| Vytvoření místního svazku <*název svazku*> se nezdařilo. |Úloha vytvoření svazku Hello se nezdařilo. <*Odpovídající toohello chybová zpráva se nepovedlo, kód chyby*>. |Problémy s připojením k může bránit úspěšně dokončení hello místo vytvoření operace. Místně vázaných svazků jsou tlustě zřízený a hello proces vytváření místo zahrnuje přesahu vrstvené svazky toohello cloudu. Pokud nejsou žádné problémy s připojením, můžete vyčerpal hello volné místo v zařízení hello. Určení, zda místo existuje na hello zařízení před opakovaným pokusem o tuto operaci. |
| Rozšíření místní svazek <*název svazku*> se nezdařilo. |Hello svazku úpravy úloha se nezdařila z důvodu příliš <*odpovídající toohello chybová zpráva se nepovedlo, kód chyby*>. |Problémy s připojením k může bránit úspěšné dokončení hello svazku rozšíření operace. Místně vázaných svazků jsou tlustě zřízený a hello proces rozšíření stávající prostor hello zahrnuje přesahu vrstvené svazky toohello cloudu. Pokud nejsou žádné problémy s připojením, můžete vyčerpal hello volné místo v zařízení hello. Určení, zda místo existuje na hello zařízení před opakovaným pokusem o tuto operaci. |
| Převod svazku <*název svazku*> se nezdařilo. |Hello svazku převod tooconvert hello svazku typ úlohy z místně vázaný tootiered se nezdařilo. |Převod svazku hello z typu místně vázaný tootiered nebylo možné dokončit. Ujistěte se, že neexistují žádné problémy s připojením k zabránění úspěšně dokončení operace hello. Řešení potíží s připojením problémy najdete příliš[Poradce při potížích s rutiny Test-HcsmConnection hello](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-test-hcsmconnection-cmdlet).<br>Hello původní místně vázaný svazek nyní byl označen jako vrstvený svazek vzhledem k tomu, že některé hello dat z hello místně vázaný svazek má uniknout toohello cloudu při převodu hello. Hello výsledné vrstvený svazek je stále zabírá volné místo na hello zařízení, které nelze znovu použít pro budoucí místní svazky.<br>Vyřešte problémy s připojením, vymažte hello výstrahy a převést tento svazek back toohello původní místně vázaný svazek typu tooensure všechna data hello je k dispozici místně znovu. |
| Převod svazku <*název svazku*> se nezdařilo. |Hello svazku převod tooconvert hello svazku typ úlohy z vrstvené toolocally připnutý se nezdařilo. |Převod svazku hello z typu, který připnutý vrstvené toolocally nebylo možné dokončit. Ujistěte se, že neexistují žádné problémy s připojením k zabránění úspěšně dokončení operace hello. Řešení potíží s připojením problémy najdete příliš[Poradce při potížích s rutiny Test-HcsmConnection hello](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-test-hcsmconnection-cmdlet).<br>se už nedá uvolnit Hello původní vrstvený svazek nyní označena jako místně vázaný svazek jako součást procesu převodu hello pokračuje toohave dat umístěných v cloudu hello, zatímco hello tlustě zřízený místa na hello zařízení pro tento svazek pro budoucí místní svazky.<br>Vyřešte všechny problémy s připojením, výstrahu zrušte hello a převést tento svazek back toohello původní vrstvený svazek typu tooensure místní místo tlustě zřízený v zařízení hello můžete znovu použít. |
| Blíží energie volné místo pro místní snímky <*název skupiny svazku*> |Místní snímky pro zásady zálohování hello může brzy k dispozici dostatek místa a být zneplatněné tooavoid chyby zápisu hostitele. |Časté místní snímky spolu s vysokou data změny ve svazcích hello spojených s touto skupinou zásady zálohování způsobují volné místo na toobe zařízení hello rychle využívat. Odstraňte všechny místní snímky, které už nejsou potřeba. Také aktualizovat vaše místní snímek plány pro tuto zásadu zálohování tootake méně časté místní snímky a zajistit, aby se pravidelně přijata cloudových snímků. Pokud nejsou provést tyto akce, volné místo pro tyto snímky brzy byl vyčerpán a hello systému budou automaticky odstraňte tooensure, který hostitel zápis pokračovat toobe byly úspěšně zpracovány. |
| Místní snímky pro <*název skupiny svazku*> se zrušila platnost. |Hello místních snímků pro <*název skupiny svazku*> byla zrušena a poté odstranit, protože jejich byly překročení hello volné místo v zařízení hello. |tooensure tento není opakovat v hello budoucí, zkontrolujte hello místní snímek plány pro tyto zásady zálohování a odstraňte všechny místní snímky, které už nejsou potřeba. Časté místní snímky spolu s vysokou data změny ve svazcích hello spojených s touto skupinou zásady zálohování může způsobit volné místo na toobe zařízení hello rychle využívat. |
| Obnovení z <*zdroje ID zálohování prvků*> se nezdařilo. |Úloha obnovení Hello se nezdařila. |Pokud máte místně vázaný nebo kombinaci místně vázaný a vrstvené svazky v zásady zálohování, zálohování seznamu tooverify hello aktualizace, která hello zálohování je stále platný. Pokud hello zálohování je platný, je možné, že potíže s připojením k cloudu brání úspěšně dokončení operace obnovení hello. Hello místně připojené svazky, které byly součástí této skupiny snímku všechna jejich zařízení stažené toohello data nemají, a pokud máte směs vrstvené a místně vázaných svazků v této skupině snímku, nebudou synchronizovány mezi sebou obnovena. toosuccessfully dokončit operaci obnovení hello, přijměte hello svazky v této skupině do offline režimu na hostiteli hello a opakujte operaci obnovení hello. Všimněte si, že žádná data svazku toohello úpravy, které byly provedeny během hello procesu obnovení budou ztraceny. |

### <a name="networking-alerts"></a>Výstrahy sítě

| Textu výstrahy | Událost | Další informace / doporučené akce |
|:--- |:--- |:--- |
| Nepovedlo se spustit služby StorSimple. |Chyba DataPath |Pokud hello potíže potrvají, obraťte se na Microsoft Support. |
| Pro 'Data0' zjištěna duplicitní IP adresu. | |Hello systém zjistil konflikt pro IP adresu, 10.0.0.1'. Hello síťovému prostředku 'Data0' na zařízení hello  *<device1>*  je offline. Ujistěte se, že tato IP adresa není používán ostatní entity v této síti. problémy se síťovým tootroubleshoot, přejděte příliš[řešení pomocí rutiny Get-NetAdapter hello](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-get-netadapter-cmdlet). Obraťte se na správce sítě pro pomoc při řešení tohoto problému. Pokud hello potíže potrvají, obraťte se na Microsoft Support. |
| Adresa IPv4 (nebo IPv6) pro 'Data0' je offline. | |Hello síťovému prostředku "Data0" s IP adresou, 10.0.0.1." a délka '22' předpony na zařízení hello  *<device1>*  je offline. Zkontrolujte, že toowhich porty přepínače hello, které toto rozhraní je připojen provozní. problémy se síťovým tootroubleshoot, přejděte příliš[řešení pomocí rutiny Get-NetAdapter hello](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-get-netadapter-cmdlet). |
| Nelze se připojit toohello ověřovací služby. |Chyba DataPath |Hello URLthat slouží tooauthenticate není dostupný. Ujistěte se, že vašich pravidlech brány firewall zahrnují hello vzorů adresy URL zadaná pro zařízení StorSimple hello. Další informace o vzorů adresy URL na portálu Azure přejděte toohttps://aka.ms/ss-8000-network-reqs. Pokud používáte Azure Cloud vlády, přejděte toohello vzorů adresy URL v https://aka.ms/ss8000-gov-network-reqs.|

### <a name="performance-alerts"></a>Výstrahy výkonu

| Textu výstrahy | Událost | Další informace / doporučené akce |
|:--- |:--- |:--- |
| Hello zařízení zatížení překročil <*prahová hodnota*>. |Nižší než očekávané doby odezvy. |Vaše zařízení sestavy využití v případě velkého zatížení vstupu a výstupu. Může dojít k práci toonot zařízení a také by měl. Zkontrolujte zatížení hello připojenými toohello zařízení a zjistěte, jestli všechny, které by mohly být přesunuty tooanother zařízení nebo které již nejsou potřebné.| Nepovedlo se spustit služby StorSimple. |Chyba DataPath |Pokud hello potíže potrvají, obraťte se na Microsoft Support. |a hello aktuální stav, přejděte příliš[použití hello toomonitor service Manager zařízení StorSimple zařízení](storsimple-monitor-device.md) |

### <a name="security-alerts"></a>Výstrahy zabezpečení

| Textu výstrahy | Událost | Další informace / doporučené akce |
|:--- |:--- |:--- |
| Zahájení relace Microsoft Support. |Relace používaná podporu jiných výrobců. |Potvrďte, že je tento přístup oprávnění. Po přijetí příslušné akce, zrušte tuto výstrahu ze stránky hello výstrahy. |
| Heslo pro <*element*> do vypršení platnosti <*dobu*>. |Se blíží vypršení platnosti hesla. |Změňte si heslo dřív, než vyprší. |
| Informace o konfiguraci zabezpečení pro <*ID elementu*>. | |Hello svazky, které jsou přidružené k tomuto kontejneru svazků nelze použít tooreplicate konfiguraci zařízení StorSimple. tooensure bezpečně uložená data, doporučujeme odstranění kontejneru svazků hello a všechny svazky přidružené kontejneru svazků hello. Po přijetí příslušné akce, zrušte tuto výstrahu ze stránky hello výstrahy. |
| <*číslo*> pokusů o přihlášení se nezdařilo pro <*ID elementu*>. |Více neúspěšné pokusy o přihlášení. |Vaše zařízení pravděpodobně probíhá útok nebo oprávněný uživatel se pokouší tooconnect pomocí nesprávného hesla.<ul><li>Obraťte se na oprávněným uživatelům a ověřte, že tyto pokusy byly ze skutečného zdroje. Pokud budete pokračovat toosee velkého počtu neúspěšných pokusů o přihlášení, zvažte zakázání vzdálené správy a kontaktovat správce sítě. Po přijetí příslušné akce, zrušte tuto výstrahu ze stránky hello výstrahy.</li><li>Zkontrolujte, zda vaše instance Snapshot Manager jsou nakonfigurovány s hello správné heslo. Po přijetí příslušné akce, zrušte tuto výstrahu ze stránky hello výstrahy.</li></ul>Další informace, přejděte příliš[změnit heslo k zařízení s vypršenou platností](storsimple-snapshot-manager-manage-devices.md#change-an-expired-device-password). |
| Jedno nebo více selhání došlo k chybě při změně šifrovacího klíče dat služby hello. | |Došlo k při změně hello šifrovacího klíče dat služby došlo k chybám. Po mít řešit hello chybové stavy, spusťte hello `Invoke-HcsmServiceDataEncryptionKeyChange` rutiny z rozhraní Windows Powershellu pro StorSimple hello vaší služby hello tooupdate zařízení. Pokud potíže potrvají, kontaktujte podporu Microsoftu. Po vyřešení problému hello, zrušte tuto výstrahu ze stránky hello výstrahy. |

### <a name="support-package-alerts"></a>Podpora balíček výstrah

| Textu výstrahy | Událost | Další informace / doporučené akce |
|:--- |:--- |:--- |
| Vytvoření balíčku pro podporu se nezdařilo. |StorSimple nelze generovat balíček hello. |Tuto operaci opakujte. Pokud hello potíže potrvají, obraťte se na Microsoft Support. Po vyřešení problému hello, zrušte tuto výstrahu ze stránky hello výstrahy. |

## <a name="next-steps"></a>Další kroky

Další informace o [StorSimple chyby a řešení potíží s provozní zařízení](storsimple-troubleshoot-operational-device.md).

