---
title: "aaaView a spravovat Microsoft Azure StorSimple virtuální pole výstrahy | Microsoft Docs"
description: "Popisuje výstrahy podmínky pole virtuální zařízení StorSimple a závažnost a jak toouse hello StorSimple Manager služby toomanage výstrahy."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 97ee25a1-0ec3-4883-9a0a-54b722598462
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b0fb5b1b9064f33df1d8fa7ace45f0d72b0a1622
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-device-manager-toomanage-alerts-for-hello-storsimple-virtual-array"></a>Pomocí Správce zařízení StorSimple toomanage výstrahy pro hello pole virtuální zařízení StorSimple

## <a name="overview"></a>Přehled

Funkce oznámení Hello v hello služby StorSimple Manager zařízení poskytuje způsob, můžete tooreview a zrušte výstrahy související tooStorSimple virtuální pole na základě v reálném čase. Můžete použít hello výstrahy na hello **souhrn služby** okno toocentrally monitorování stavu problémy hello vaše pole virtuální zařízení StorSimple a hello celkového řešení Microsoft Azure StorSimple.

Tento kurz popisuje, jak úrovně závažnosti výstrah tooconfigure oznámení o výstrahách, obecné výstrahy podmínky a jak výstrahy tooview a sledovat. Kromě toho zahrnuje výstrahy stručnou referenční tabulky, které umožňují tooquickly můžete vyhledat konkrétní výstrahu a reagují odpovídajícím způsobem.

![stránka výstrah](./media/storsimple-virtual-array-manage-alerts/alerts1.png)

## <a name="configure-alert-settings"></a>Konfigurace nastavení výstrah

Můžete zvolit, jestli chcete toobe oznámení e-mailem hello výstrahy podmínky pro všechny vaše pole virtuální zařízení StorSimple. Kromě toho můžete identifikovat ostatní příjemci oznámení výstrah tak, že zadáte jejich e-mailové adresy hello **příjemců další e-mailu** pole, oddělené středníky.

> [!NOTE]
> Můžete zadat maximálně 20 e-mailové adresy na virtuální pole.


Po povolení e-mailové oznámení pro virtuální pole členy hello seznam oznámení e-mailovou zprávu, dojde k kritickou výstrahu pokaždé, když obdrží. odešle zprávy Hello z  *storsimple-alerts-noreply@mail.windowsazure.com*  a popíše hello podmínka pro upozornění. Můžete kliknout na příjemci **Unsubscribe** tooremove sami hello e-mailové oznámení seznamu.

#### <a name="tooenable-email-notification-for-alerts"></a>tooenable e-mailové oznámení pro výstrahy

1. Přejděte tooyour Správce zařízení StorSimple, služby a hello **správy** , vyberte a klikněte na tlačítko **zařízení**. Hello seznam zařízení, na které se zobrazí vyberte a klikněte na zařízení.
   
    ![nastavení výstrah](./media/storsimple-virtual-array-manage-alerts/alerts2.png)
2. Otevře se hello **nastavení** okno. V hello **nastavení zařízení** vyberte **Obecné**. Otevře se hello **obecné nastavení** okno.
   
    ![Konfigurace oznámení výstrah](./media/storsimple-virtual-array-manage-alerts/alerts4.png)
3. V hello **obecné nastavení** okně přejděte příliš**výstrahy nastavení** tématu a nastavte následující hello:
   
   1. V hello **povolit e-mailové oznámení** pole, vyberte **Ano**.
   2. V hello **e-mailem správci služeb** pole, vyberte **Ano** Pokud nechcete, aby správce služeb hello toohave a všechny spolusprávci dostávat oznámení o výstrahách hello.
   3. V hello **příjemců další e-mailu** zadejte hello e-mailové adresy všech příjemců, kterým mají být doručena oznámení o výstrahách hello. Zadejte názvy ve formátu hello  *someone@somewhere.com* . Použijte středníky tooseparate hello e-mailové adresy. Můžete nakonfigurovat maximálně 20 e-mailové adresy na virtuální zařízení.
      
       ![Konfigurace oznámení výstrah](./media/storsimple-virtual-array-manage-alerts/alerts6.png)
   4. toosend testovací e-mailová oznámení, klikněte na tlačítko **odeslat zkušební e-mail**. Hello služby StorSimple Manager zařízení se zobrazí stavové zprávy, jak předává hello testovací oznámení.
      
       ![Výstrahy testování odeslaných e-mailové oznámení](./media/storsimple-virtual-array-manage-alerts/alerts7.png)
      
      > [!NOTE]
      > Pokud hello testovací zpráva oznámení nelze odeslat, služby StorSimple Manager zařízení hello se zobrazí příslušná zpráva. Klikněte na tlačítko **OK**, počkejte několik minut a potom opakujte toosend vaše testovací zpráva oznámení.
      > 
      > 
   5. V dolní části hello hello stránky, klikněte na tlačítko **Uložit** toosave konfiguraci. Po zobrazení výzvy k potvrzení klikněte na **Ano**.
      
      ![Výstrahy testování odeslaných e-mailové oznámení](./media/storsimple-virtual-array-manage-alerts/alerts10.png)

## <a name="common-alert-conditions"></a>Obecné podmínky výstrah

Pole virtuální zařízení StorSimple generuje výstrahy v odpovědi tooa řadu podmínek. Hello následují hello nejčastějších typů výstrah podmínek:

* **Problémy s připojením k** – tyto výstrahy dojít, když se potíže při přenosu dat. Problémy s komunikací může dojít během přenosu dat tooand z účtu úložiště Azure hello nebo splatnosti toolack připojení mezi hello virtuální zařízení a služby StorSimple Manager zařízení hello. Problémy s komunikací jsou některé hello nejtěžší toofix, protože nejsou k dispozici mnoho body selhání. Měli byste vždy nejprve ověřit, že připojení k síti a přístup k Internetu jsou k dispozici před pokračováním na toomore potíží pro pokročilé uživatele. Informace o portech a nastavení brány firewall, přejděte příliš[požadavky na systém pole virtuální zařízení StorSimple](storsimple-ova-system-requirements.md). Nápovědu k řešení potíží, přejděte příliš[Poradce při potížích s rutiny Test-Connection hello](storsimple-troubleshoot-deployment.md).
* **Problémy s výkonem** – tyto výstrahy jsou nastat, když systém nepracuje optimálně, například když je v případě velkého zatížení.

Kromě toho může zobrazit výstrahy související toosecurity, aktualizací nebo selhání úlohy.

## <a name="alert-severity-levels"></a>Úrovně závažnosti výstrah

Výstrahy mají různé úrovně závažnosti, v závislosti na hello vlivu, který hello výstrahy situaci bude mít a hello potřebu toohello upozornění na odpověď. jsou Hello úrovně závažnosti:

* **Kritické** – Tato výstraha je v odpovědi tooa podmínku, která ovlivňuje hello úspěšné výkon systému. Akce je požadovaná tooensure že hello StorSimple služba není přerušena.
* **Upozornění** – tento stav může být důležité, pokud není vyřešené. Doporučujeme prozkoumat hello situaci a trvat jakýkoli problém hello tooclear je vyžadována akce.
* **Informace o** – Tato výstraha obsahuje informace, které mohou být užitečné při sledování a správa systému.

## <a name="view-and-track-alerts"></a>Zobrazení a sledovat výstrahy

Souhrn okno služby Hello Správce zařízení StorSimple poskytuje stručný přehled hello počet výstrah na virtuální zařízení uspořádané podle úrovně závažnosti.

![Řídicí panel výstrahy](./media/storsimple-virtual-array-manage-alerts/alerts14.png)

Klikněte na tlačítko hello tooopen úrovně závažnosti hello **výstrahy** okno. Hello výsledky obsahovat pouze hello výstrahy, které odpovídají této úrovně závažnosti.

![Sestava výstrahy s rozsahem tooalert typu](./media/storsimple-virtual-array-manage-alerts/alerts15.png)

Klikněte na výstrahu v hello seznamu tooget další podrobnosti hello výstrahy, včetně Dobrý den, kdy naposledy byla nahlášena hello výstraha, hello počtu výskytů hello upozornění na hello zařízení a hello doporučenou akci tooresolve hello výstrahu.

![Seznam výstrah a podrobnosti](./media/storsimple-virtual-array-manage-alerts/alerts16.png)

Pokud potřebujete toosend hello informace tooMicrosoft podpory můžete zkopírovat hello podrobnosti výstrahy tooa textového souboru. Po a potom hello doporučení a vyřešit hello podmínka pro upozornění na místě, byste měli vymazat hello výstrahy ze seznamu hello. Vyberte výstrahu, hello hello seznamu a pak klikněte na tlačítko **zrušte**. tooclear více výstrah, vyberte všechny výstrahy, klikněte na možnost žádný sloupec s výjimkou hello **výstraha** sloupec a pak klikněte na tlačítko **zrušte** po výběru všechny hello výstrahy toobe vymazán.

Když kliknete na tlačítko **zrušte**, budete mít hello možnost tooprovide komentáře o výstraze hello a kroky hello následovala tooresolve hello problém. 

![výstrahy komentáře](./media/storsimple-virtual-array-manage-alerts/alerts17.png)

Některé události budou vymazána systémem hello, pokud jiná událost se aktivuje se novými informacemi. 

## <a name="sort-and-review-alerts"></a>Řazení a zkontrolujte výstrahy

Hello **výstrahy** až too250 výstrahy můžete zobrazit okno. Pokud byl překročen počet výstrah, zobrazí se všechny výstrahy v hello výchozí zobrazení. Zkombinováním hello toocustomize pole, které výstrahy se zobrazují následující:

* **Stav** – můžete zobrazit buď **Active** nebo **nezaškrtnuto** výstrahy. Aktivní výstrahy se stále aktivují v systému, zatímco nezaškrtnuté výstrahy byly buď ručně vymazat správcem nebo prostřednictvím kódu programu vymazat, protože podmínka pro upozornění hello aktualizovat systém hello se novými informacemi.
* **Závažnost** – můžete zobrazit výstrahy všechny úrovně závažnosti (kritická, upozornění, informace o) nebo pouze určité závažnosti, například pouze kritické výstrahy.
* **Zdroj** – můžete zobrazit výstrahy ze všech zdrojů, nebo omezit toothose hello výstrahy, které pocházejí z hello služby nebo jeden nebo všechny hello virtuální zařízení.
* **Čas rozsah** – zadáním hello **z** a **k** kalendářních dat a časových razítek, můžete se podívat na výstrahy během hello časové období, které vás zajímají.

## <a name="alerts-quick-reference"></a>Stručná referenční příručka výstrahy

Následující tabulky Hello seznam některých hello StorSimple výstrahy, které se mohou vyskytnout, a také další informace a doporučení tam, kde je k dispozici. Pole virtuální zařízení StorSimple výstrahy spadat do jednoho z následujících kategorií hello:

* [Výstrahy připojení cloudu](#cloud-connectivity-alerts)
* [Konfigurace výstrahy](#configuration-alerts)
* [Upozornění na selhání úlohy](#job-failure-alerts)
* [Výstrahy výkonu](#performance-alerts)
* [Výstrahy zabezpečení](#security-alerts)
* [Aktualizace výstrah](#update-alerts)

### <a name="cloud-connectivity-alerts"></a>Výstrahy připojení cloudu

| Textu výstrahy | Událost | Další informace / doporučené akce |
|:--- |:--- |:--- |
| Zařízení  *<device name>*  je Nepřipojeno toohello cloudu. |Hello s názvem zařízení se nemůže připojit toohello cloudu. |Nelze se připojit toohello cloudu. To může být kvůli tooone hello následující:<ul><li>Je možné, problémy s hello nastavení sítě na vašem zařízení.</li><li>Je možné, problémy s hello přihlašovacích údajů účtu úložiště.</li></ul>Další informace o řešení potíží s připojením, přejděte toohello [místního webového uživatelského rozhraní](storsimple-ova-web-ui-admin.md) hello zařízení. |

### <a name="configuration-alerts"></a>Konfigurace výstrahy

| Textu výstrahy | Událost | Další informace / doporučené akce |
|:--- |:--- |:--- |
| Nepodporovaná konfigurace virtuálního zařízení na místě. |Nízký výkon. |aktuální konfigurace Hello může způsobit snížení výkonu. Zkontrolujte, zda server splňuje požadavky na minimální konfiguraci hello. Další informace, přejděte příliš[pole požadavky virtuální zařízení StorSimple](storsimple-ova-system-requirements.md). |
| Spustíte nedostatek místa na disku zřízené v <*název zařízení*>. |Upozornění místa na disku. |Jsou dostatek místa na disku zřízené. toofree místo, zvažte přesunutí úloh tooanother svazek nebo sdílenou složku nebo odstraňovat data. |

### <a name="job-failure-alerts"></a>Upozornění na selhání úlohy

| Textu výstrahy | Událost | Další informace / doporučené akce |
|:--- |:--- |:--- |
| Zálohování <*název zařízení*> nebylo možné dokončit. |Selhání úlohy zálohování. |Nelze vytvořit zálohu. Jedna z následujících hello vezměte v úvahu:<ul><li>Problémy s připojením k může bránit operace zálohování hello z úspěšné dokončení. Ujistěte se, že neexistují žádné problémy s připojením. Další informace o řešení potíží s připojením, přejděte toohello [místního webového uživatelského rozhraní](storsimple-ova-web-ui-admin.md) pro virtuální zařízení.</li><li>Dosáhli jste limitu úložiště k dispozici hello. toofree místo, zvažte odstranění všechny zálohy, které už nejsou potřeba.</li></ul> Vyřešte problémy hello, zrušte hello výstrahy a opakujte operaci hello. |
| Klonovat z <*název zařízení*> nebylo možné dokončit. |Klonování úloha se nezdařila. |Nelze vytvořit klon počítače. Jedna z následujících hello vezměte v úvahu:<ul><li>Zálohování seznamu nemusí být platný. Aktualizujte tooverify seznamu hello je stále platný.</li><li>Problémy s připojením k může bránit úspěšné dokončení operace klonování hello. Ujistěte se, že neexistují žádné problémy s připojením.</li><li>Dosáhli jste limitu úložiště k dispozici hello. toofree místo, zvažte odstranění všechny zálohy, které už nejsou potřeba.</li></ul>Vyřešte problémy hello, zrušte hello výstrahy a opakujte operaci hello. |

### <a name="performance-alerts"></a>Výstrahy výkonu

| Textu výstrahy | Událost | Další informace / doporučené akce |
|:--- |:--- |:--- |
| Neočekávané zpoždění při přenosu dat se setkáváte s. |Přenos dat pomalé. |Dochází k omezení chybám, když překročíte hello cíle škálovatelnosti služby úložiště. Služba úložiště Hello nemá tato tooensure, který žádná jednoho klienta nebo klienta můžete použít službu hello na náklady hello jiných. Další informace o řešení potíží s vaším účtem úložiště Azure, přejděte příliš[monitorování, Diagnostika a řešení Microsoft Azure Storage](../storage/common/storage-monitoring-diagnosing-troubleshooting.md). |
| Máte málo místní rezervace místa na disku na <*název zařízení*>. |Doba pomalé odezvy. |10 % hello celková velikost zřízeného <*název zařízení*> je vyhrazen na hello místní zařízení a vy se teď nedostatek hello vyhrazené místo. zatížení Hello <*název zařízení*> generuje a vyšší míra změn, nebo může mít nedávno migrace velké množství dat.. Výsledkem může být snížený výkon. Vezměte v úvahu mezi hello následující akce tooresolve toto:<ul><li>Zvýšit hello cloud šířky pásma toothis zařízení.</li><li>Snižte nebo přesunutí úloh tooanother svazek nebo sdílenou složku.</li></ul> |

### <a name="security-alerts"></a>Výstrahy zabezpečení

| Textu výstrahy | Událost | Další informace / doporučené akce |
|:--- |:--- |:--- |
| Heslo pro <*název zařízení*> do vypršení platnosti <*číslo*> dnů. |Heslo upozornění. |Heslo vyprší za < číslo < dnů. Zvažte změnu hesla. Další informace, přejděte příliš[hesla správce zařízení StorSimple virtuální pole hello změnu](storsimple-virtual-array-change-device-admin-password.md). |

### <a name="update-alerts"></a>Aktualizace výstrah

| Textu výstrahy | Událost | Další informace / doporučené akce |
|:--- |:--- |:--- |
| Jsou nové aktualizace dostupné pro vaše zařízení. |Aktualizace toohello pole virtuální zařízení StorSimple jsou k dispozici. |Můžete nainstalovat nové aktualizace z hello **údržby** stránky. |
| Nelze vyhledat nové aktualizace na <*název zařízení*>. |Aktualizujte selhání. |Došlo k chybě při instalaci nové aktualizace. Můžete ručně nainstalovat aktualizace hello. Pokud hello potíže potrvají, obraťte se na [Microsoft Support](storsimple-contact-microsoft-support.md). |

## <a name="next-steps"></a>Další kroky

* [Další informace o hello pole virtuální zařízení StorSimple](storsimple-ova-overview.md).

