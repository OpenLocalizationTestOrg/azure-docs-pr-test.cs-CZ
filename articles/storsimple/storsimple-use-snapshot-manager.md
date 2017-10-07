---
title: "uživatelské rozhraní aaaStorSimple Snapshot Manager | Microsoft Docs"
description: "Popisuje hello StorSimple Snapshot Manager uživatelské rozhraní a vysvětluje, jak toouse ho toomanage úlohy zálohování a hello zálohování katalogu."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: c7d91892-2881-41a2-a7a2-908dc3646493
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.custom: 
ms.openlocfilehash: 865520fdaf1b559714b52b428ad136b084d65c99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-snapshot-manager-user-interface-toomanage-backup-jobs-and-backup-catalog"></a>Úlohy zálohování rozhraní toomanage uživatele pro použití StorSimple Snapshot Manager a katalog zálohování

## <a name="overview"></a>Přehled
Hello StorSimple Snapshot Manager má intuitivní uživatelské rozhraní, můžete použít tootake a spravovat zálohy. V tomto kurzu poskytuje úvod toohello uživatelské rozhraní a pak vysvětluje, jak toouse součástí hello. Podrobný popis hello Snapshot Manager zařízení StorSimple, najdete v části [co je StorSimple Snapshot Manager?](storsimple-what-is-snapshot-manager.md)

### <a name="console-description"></a>Popis konzoly
tooview hello uživatelské rozhraní, klikněte na ikonu StorSimple Snapshot Manager hello na ploše. Zobrazí se okno konzoly Hello, jak ukazuje následující obrázek hello.

![Podokna Snapshot Manager zařízení StorSimple](./media/storsimple-use-snapshot-manager/HCS_SSM_gui_panes.png)

okno konzoly Hello má pět hlavních prvků. Klikněte na příslušný odkaz hello úplný popis jednotlivých prvků.

* [Panel nabídek](#menu-bar) 
* [Panel nástrojů](#tool-bar) 
* [Podokno oboru](#scope-pane) 
* [Podokno výsledků](#results-pane) 
* [Podokna akcí](#actions-pane) 

Kromě toho hello StorSimple Snapshot Manager podporuje [klávesové navigaci a počet zkratky](#keyboard-navigation-and-shortcuts).

### <a name="console-accessibility"></a>Usnadnění přístupu konzoly
uživatelské rozhraní Hello StorSimple Snapshot Manager podporuje hello usnadnění funkce poskytované službou operační systém Windows hello a hello Microsoft Management Console (MMC) a také některé klávesové zkratky specifické StorSimple Snapshot Manager. 

* Popis funkce usnadnění systému Windows hello, přejděte příliš[klávesové zkratky pro systém Windows](https://support.microsoft.com/kb/126449). 
* Popis funkce usnadnění hello konzoly MMC, přejděte příliš[usnadnění pro konzolu MMC 3.0](https://technet.microsoft.com/library/cc766075.aspx)
* Popis funkce usnadnění hello Snapshot Manager zařízení StorSimple, přejděte příliš[klávesové navigaci a zkratky](#keyboard-navigation-and-shortcuts).

## <a name="menu-bar"></a>Panel nabídek
panel nabídek Hello hello horní části okna konzoly hello obsahuje [soubor](#file-menu), [akce](#action-menu), [zobrazení](#view-menu), [Oblíbené](#favorites-menu), [okna ](#window-menu), a [pomoci](#help-menu) nabídky.

Kliknutím na libovolnou položku v hello nabídky panelu toosee seznam příkazů, k dispozici na této nabídky. Hello následující příklad ukazuje hello **zobrazení** nabídce vybrali v řádku nabídek hello.

![Vybraná nabídka Zobrazit](./media/storsimple-use-snapshot-manager/HCS_SSM_View_menu.png)

### <a name="file-menu"></a>Nabídka Soubor
Hello **souboru** nabídky obsahuje standardní příkazy Microsoft Management Console (MMC).

#### <a name="menu-access"></a>Přístup do nabídky
tooview hello **soubor** nabídky, klikněte na tlačítko **souboru** v řádku nabídek hello. Zobrazí se následující nabídky Hello.

![Nabídka Soubor Snapshot Manager zařízení StorSimple](./media/storsimple-use-snapshot-manager/HCS_SSM_FileMenu.png) 

#### <a name="menu-description"></a>Popis nabídky
Hello následující tabulka popisuje položky, které se zobrazují na hello **souboru** nabídky.

| Položky nabídky | Popis |
|:--- |:--- |
| Nový |Klikněte na tlačítko **nový** toocreate novou konzolu podle hello StorSimple Snapshot Manager. |
| Otevřenost |Klikněte na tlačítko **otevřete** tooopen stávající konzoly. |
| Uložit |Klikněte na tlačítko **Uložit** toosave hello aktuální konzolu. |
| Uložit jako |Klikněte na tlačítko **uložit jako** toocreate nové, přejmenovat instanci hello aktuální konzolu. Použití hello **uložit jako** možnost toocustomize zobrazení a uložit pro pozdější načtení. Například můžete vytvořit StorSimple Snapshot Manager moduly snap in tento bod toospecific servery. |
| Přidat nebo odebrat modul Snap-in |Klikněte na tlačítko **přidat nebo odebrat modul Snap-in** tooadd nebo odebrat moduly snap in a tooorganize uzly v hello **oboru** podokně. Další informace, přejděte příliš[přidat, odebrat a uspořádat moduly Snap in a rozšíření v MMC 3.0](https://technet.microsoft.com/library/cc722035.aspx). |
| Možnosti |Klikněte na tlačítko **možnosti** toochange hello konzoly ikonu, zadejte režimy přístupu uživatele a oprávnění nebo odstranit konzoly soubory tooincrease volného místa na disku. |
| Seznam cest k souborům |Klikněte na tlačítko k cestě v seznamu tooreopen hello číslované soubor, který jste právě otevřeli. |
| Ukončení |Klikněte na tlačítko **ukončení** tooclose hello **souboru** nabídky. |

### <a name="action-menu"></a>Nabídka Akce
Použití hello **akce** tooselect nabídky z dostupné akce. k dispozici tooyou Hello položky závisí na výběr hello provedete v hello **oboru** podokně nebo **výsledky** podokně.

#### <a name="menu-access"></a>Přístup do nabídky
tooview hello **akce** nabídky, proveďte jednu z následujících hello:

* Klikněte pravým tlačítkem na položku v hello **oboru** podokně nebo **výsledky** podokně.
* Vyberte položku v hello **oboru** podokně nebo **výsledky** podokně a pak klikněte na tlačítko **akce** v řádku nabídek hello. 

Například, pokud vyberete hello nejvyšší uzel v hello **oboru** podokně a pak klikněte pravým tlačítkem nebo klikněte na **akce** v řádku nabídek hello hello následující nabídky se zobrazí.

![Nabídka Akce Snapshot Manager zařízení StorSimple](./media/storsimple-use-snapshot-manager/HCS_SSM_Action_menu.png)

Hello **akce** podokně (na hello napravo od konzoly hello) obsahuje hello stejný seznam akcí, jako hello **akce** nabídky. Kromě toho hello **akce** podokně obsahuje hello **zobrazení** možnosti nabídky, které umožňují toocreate vlastní zobrazení hello **výsledky** podokně.

![Otevření podokna akce s nabídka Zobrazit](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane_Results.png)

#### <a name="menu-description"></a>Popis nabídky
Hello následující tabulka obsahuje abecední seznam akcí StorSimple Snapshot Manager. 

* Hello **akce** sloupci jsou uvedeny akce, které můžete provádět na uzlech a výsledky. 
* Hello **navigační** sloupec vysvětluje, jak hello odpovídající toodisplay **akce** nabídky, ve kterém můžete vybrat hello akce. Některé akce se zobrazí v několika **akce** nabídky. Pro tyto akce, vyberte jednu **navigační** možnost ze seznamu s odrážkami hello. 
* Hello **popis** sloupec popisuje jak toouse každou akci v hello **akce** nabídky nebo podokna akce a vysvětluje, jak funguje.

> [!NOTE]
> Hello **akce** podokně a **akce** nabídky obsahovat další možnosti, jako například **zobrazení**, **nové okno**,  **Aktualizujte**, **exportovat seznam**, a **pomoci**. Tyto možnosti jsou k dispozici jako součást hello konzoly MMC a nejsou konkrétní tooStorSimple Snapshot Manager. Hello tabulka obsahuje popisy těchto možností.
> 
> 

| Akce | Navigace | Popis |
|:--- |:--- |:--- |
| Ověření |Klikněte na tlačítko hello **zařízení** uzel a klikněte pravým tlačítkem na zařízení v hello **výsledky** podokně. |Klikněte na tlačítko **ověřit** tooenter hello heslo, které jste nakonfigurovali pro hello zařízení. |
| Klon |Rozbalte položku **zálohování katalogu**, rozbalte položku **cloudových snímků**, klikněte na tlačítko ze zálohy a pak vyberte svazek v hello **výsledky** podokně. |Klikněte na tlačítko **klon** toocreate kopii cloudu snímku a uložte ho do umístění, který určíte. |
| Konfigurace zařízení |Klikněte pravým tlačítkem na hello **zařízení** uzlu. |Klikněte na tlačítko **nakonfigurovat zařízení** tooconfigure jednoho zařízení nebo více zařízení tooconnect toohello Windows hostitele. |
| Vytvořit zásady zálohování |Proveďte jednu z následujících hello:<ul><li>Klikněte pravým tlačítkem na **zásady zálohování**.</li><li>Klikněte na tlačítko nebo rozbalte **svazku skupiny**a potom klikněte pravým tlačítkem na skupinu svazku.</li><li>Klikněte na tlačítko nebo rozbalte **katalog zálohování**a potom klikněte pravým tlačítkem na skupinu svazku.</li></ul> |Klikněte na tlačítko **vytvořit zásady zálohování** tooconfigure plánované zálohování pro skupinu svazku. |
| Vytvoření svazku skupiny |Proveďte jednu z následujících hello:<ul><li>Klikněte na tlačítko hello **svazky** uzel a potom klikněte pravým tlačítkem na svazek v hello **výsledky** podokně.</li><li>Klikněte pravým tlačítkem na hello **svazku skupiny** uzlu.</li></ul> |Klikněte na tlačítko **vytvořit skupinu svazku** tooassign svazky tooa svazku skupiny. |
| Odstranění |Klikněte na uzel nebo výsledek (Tato položka je zobrazena na mnoho **akce** nabídky a **akce** podokna.) |Klikněte na tlačítko **odstranit** toodelete hello uzel nebo výsledek, který jste vybrali. Jakmile se zobrazí dialogové okno potvrzení hello, potvrďte, nebo zrušte odstraňování hello. |
| Podrobnosti |Klikněte na tlačítko hello **zařízení** uzel a potom klikněte pravým tlačítkem na zařízení v hello **výsledky** podokně. |Klikněte na tlačítko **podrobnosti** toosee hello podrobnosti konfigurace pro zařízení. |
| Upravit |Klikněte na tlačítko **zásady zálohování**a potom klikněte pravým tlačítkem na zásadu v hello **výsledky** podokně. |Klikněte na tlačítko **upravit** toochange hello plán zálohování pro skupinu svazku. |
| Export seznamu |Klikněte na libovolný uzel nebo výsledek (Tato položka je zobrazena na všech **akce** nabídky a **akce** podokna.) |Klikněte na tlačítko **exportovat seznam** toosave seznamu v souboru hodnot oddělených čárkami (CSV). Tento soubor pak můžete importovat do aplikace tabulkového procesoru k analýze. |
| Nápověda |Klikněte na libovolný uzel nebo výsledek. (Tato položka je zobrazena na všech **akce** nabídky a **akce** podokna.) |Klikněte na tlačítko **pomoci** tooopen online nápověda v samostatném okně prohlížeče. |
| Nové okno |Klikněte na libovolný uzel nebo výsledek (Tato položka je zobrazena na všech **akce** nabídky a **akce** podokna.) |Klikněte na tlačítko **nové okno** tooopen nové okno StorSimple Snapshot Manager. |
| Aktualizace |Klikněte na libovolný uzel nebo výsledek (Tato položka je zobrazena na všech **akce** nabídky a **akce** podokna.) |Klikněte na tlačítko **aktualizovat** tooupdate hello aktuálně zobrazený StorSimple Snapshot Manager okno. |
| Aktualizace zařízení |Klikněte na tlačítko hello **zařízení** uzel a klikněte pravým tlačítkem na zařízení v hello **výsledky** podokně. |Klikněte na tlačítko **aktualizace zařízení** toosynchronize konkrétní připojené zařízení s StorSimple Snapshot Manager. |
| Aktualizace zařízení |Klikněte pravým tlačítkem na hello **zařízení** uzlu. |Klikněte na tlačítko **aktualizace zařízení** toosynchronize seznam připojených zařízení s StorSimple Snapshot Manager. |
| Prohledat znovu svazky |Klikněte pravým tlačítkem na hello **svazky** uzlu. |Klikněte na tlačítko **Prohledat znovu svazky** tooupdate hello seznamu svazků, které se zobrazí v hello **výsledky** podokně. |
| Obnovení |Rozbalte položku **zálohování katalogu**, rozbalte skupinu svazku, rozbalte položku **místní snímky** nebo **cloudových snímků**a potom klikněte pravým tlačítkem na zálohu. |Klikněte na tlačítko **obnovení** tooreplace hello aktuální svazek seskupovat data s hello data ze zálohy vybrané hello. |
| Proveďte zálohování |Proveďte jednu z následujících hello:<ul><li>Rozbalte položku **svazku skupiny**a potom klikněte pravým tlačítkem na skupinu svazku.</li><li>Rozbalte položku **katalog zálohování**a potom klikněte pravým tlačítkem na skupinu svazku.</li></ul> |Klikněte na tlačítko **provést zálohování** toostart okamžitě úlohu zálohování. |
| Přepnout importů zobrazení |Klikněte pravým tlačítkem na nejvyšší uzel hello v hello **oboru** podokně (hello **StorSimple Snapshot Manager** uzlu v příkladech hello). |Klikněte na tlačítko **přepnout importů zobrazení** tooshow nebo skrýt hello svazku skupin a přidružených záloh, které byly naimportovány z hello řídicího panelu služby StorSimple Manager zařízení. |

### <a name="view-menu"></a>Nabídka Zobrazit
Použití hello **zobrazení** toocreate nabídky vlastní zobrazení hello **výsledky** podokně obsah. Hello **zobrazení** nabídky obsahuje **přidat nebo odebrat sloupce** a **přizpůsobit** možnosti.

#### <a name="menu-access"></a>Přístup do nabídky
Dostanete hello **zobrazení** nabídky v řádku nabídek hello nebo v hello **akce** podokně.

![Nabídka Zobrazit Snapshot Manager zařízení StorSimple](./media/storsimple-use-snapshot-manager/HCS_SSM_View_menu.png) 

#### <a name="menu-description"></a>Popis nabídky
Hello následující tabulka popisuje položky, které se zobrazují na hello **zobrazení** nabídky.

| Položky nabídky | Popis |
|:--- |:--- |
| Přidat nebo odebrat sloupce |Klikněte na tlačítko **přidat nebo odebrat sloupce** tooadd nebo odebrat sloupce v hello **výsledky** podokně. |
| Přizpůsobení |Klikněte na tlačítko **přizpůsobit** tooshow nebo skrytí položek v okně konzoly hello StorSimple Snapshot Manager. |

### <a name="favorites-menu"></a>Oblíbené položky nabídky
Použít hello **Oblíbené** tooadd nabídky, odstraňování a uspořádání zobrazení stránky a úlohy, které používáte. 

#### <a name="menu-access"></a>Přístup do nabídky
Dostanete hello **Oblíbené** nabídky v řádku nabídek hello.

![StorSimple Snapshot Manager oblíbených položek nabídky](./media/storsimple-use-snapshot-manager/HCS_SSM_FavoritesMenu.png)

#### <a name="menu-description"></a>Popis nabídky
Hello následující tabulka popisuje položky, které se zobrazují na hello **Oblíbené** nabídky.

| Položky nabídky | Popis |
|:--- |:--- |
| Přidat tooFavorites |Klikněte na tlačítko **přidat tooFavorites** tooadd hello aktuální zobrazení tooyour seznamu oblíbených položek. |
| Uspořádání oblíbených položek |Klikněte na tlačítko **uspořádat oblíbené položky** tooorganize hello obsah složky Oblíbené položky. |

### <a name="window-menu"></a>Nabídky okna
Použití hello **okno** nabídky tooadd a uspořádání StorSimple Snapshot Manager konzoly windows.

#### <a name="menu-access"></a>Přístup do nabídky
Dostanete hello **okno** nabídky v řádku nabídek hello.

![Nabídka okno Snapshot Manager zařízení StorSimple](./media/storsimple-use-snapshot-manager/HCS_SSM_WindowMenu.png)

Hello číslovaný seznam v dolní části hello hello nabídky ukazuje, že windows hello, které jsou aktuálně otevřít. Klikněte na libovolné okno v okně Seznam toobring hello do popředí hello. 

#### <a name="menu-description"></a>Popis nabídky
Hello následující tabulka popisuje hello položky, které se zobrazují v nabídce okno hello.

| Položky nabídky | Popis |
|:--- |:--- |
| Nové okno |Klikněte na tlačítko **nové okno** tooopen nové okno konzoly (v přidání toohello existující okně). |
| CASCADE |Klikněte na tlačítko **Cascade** toodisplay hello otevřít konzolu systému windows ve stylu CSS. |
| Vedle sebe |Klikněte na tlačítko **dlaždici vodorovně** toodisplay hello otevřít konzolu systému windows ve formátu dlaždice (nebo mřížky). |
| Seřadí ikony |Pokud máte více konzoly windows otevřete a rozmístěny v ploše je minimalizovat a pak klikněte na **. Seřadit ikony** tooarrange je na vodorovném řádku hello dolní části obrazovky. |

### <a name="help-menu"></a>Nabídka Nápověda
Použití hello **pomoci** nabídky tooview k dispozici online nápovědy pro StorSimple Snapshot Manager a hello konzoly MMC. Můžete také zobrazit informace o hello konzoly MMC a StorSimple Snapshot Manager software verze, které jsou aktuálně nainstalovány ve vašem systému. 

Dostanete hello **pomoci** nabídky v řádku nabídek hello. Témata nápovědy StorSimple Snapshot Manager můžete taky přejít z hello **akce** podokně.

![Nabídka Nápověda Snapshot Manager zařízení StorSimple](./media/storsimple-use-snapshot-manager/HCS_SSM_HelpMenu.png)

#### <a name="menu-description"></a>Popis nabídky
Hello následující tabulka popisuje položky, které se zobrazují v nabídce Nápověda hello.

| Položky nabídky | Popis |
|:--- |:--- |
| Nápovědy na StorSimple Snapshot Manager |Klikněte na tlačítko **pomoci na StorSimple Snapshot Manager** tooopen nápovědy Snapshot Manager zařízení StorSimple v samostatném okně. |
| Témata nápovědy |Klikněte na tlačítko **témata nápovědy** tooopen online nápovědy konzoly MMC v samostatném okně. |
| Webu TechCenter |Klikněte na tlačítko **webu TechCenter** tooopen hello Microsoft TechNet technická Center domovskou stránku v samostatném okně. |
| Konzola Microsoft Management Console |Klikněte na tlačítko **o konzoly Microsoft Management Console** toosee, kterou verzi hello konzoly Microsoft Management Console je nainstalován ve vašem systému. |
| O Snapshot Manager zařízení StorSimple |Klikněte na tlačítko **o StorSimple Snapshot Manager** toosee, která verze modulu snap-in hello je nainstalovaná ve vašem systému. |

## <a name="tool-bar"></a>Panel nástrojů
panel nástrojů Hello, nacházel pod řádku nabídek hello obsahuje navigační a ikony úloh. Každá ikona je konkrétní úkol tooa zástupce.

### <a name="icon-descriptions"></a>Ikona popisy
Hello následující tabulka popisuje hello ikony, které se zobrazují na panelu nástrojů hello. 

| Ikona | Popis |
|:--- |:--- |
| ![Šipka doleva](./media/storsimple-use-snapshot-manager/HCS_SSM_LeftArrow.png) |Klikněte na tlačítko hello šipku vlevo ikonu tooreturn toohello předchozí stránku. |
| ![Šipka doprava](./media/storsimple-use-snapshot-manager/HCS_SSM_RightArrow.png) |Klikněte na další stránku hello šipku vpravo toogo toohello (Pokud hello šipku není šedá, hello akce k dispozici). |
| ![Šipka dolů](./media/storsimple-use-snapshot-manager/HCS_SSM_Up.png) |Klikněte na tlačítko hello až ikonu toogo jednu úroveň ve stromu konzoly hello (hello **oboru** podokně). |
| ![Zobrazit/skrýt strom konzoly](./media/storsimple-use-snapshot-manager/HCS_SSM_ShowConsoleTree.png) |Klikněte na tlačítko hello zobrazit či skrýt konzole stromu ikonu tooshow nebo skrytí hello **oboru** podokně. |
| ![Export seznamu](./media/storsimple-use-snapshot-manager/HCS_SSM_ExportListIcon.png) |Klikněte na tlačítko hello export seznamu ikona tooexport soubor seznamu tooa CSV, který určíte. |
| ![Ikonu nápovědy](./media/storsimple-use-snapshot-manager/HCS_SSM_HelpIcon.png) |Klikněte na tlačítko tooopen ikonu nápovědy hello online téma nápovědy konzoly MMC. |
| ![Zobrazit či skrýt podokno akcí](./media/storsimple-use-snapshot-manager/HCS_SSM_ShowAction.png) |Klikněte na tlačítko Zobrazit či skrýt hello **akce** hello ikonu tooshow nebo skrýt podokno **akce** podokně. |

## <a name="scope-pane"></a>Podokno oboru
Hello **oboru** podokno je krajní levé podokno hello v hello StorSimple Snapshot Manager uživatelského rozhraní. Obsahuje hello stromu konzoly (nebo uzel) a hello primární navigační mechanismus pro StorSimple Snapshot Manager. 

### <a name="scope-pane-structure"></a>Struktura podokně oboru
Hello **oboru** podokně obsahuje řadu prokliknutelný objekty (uzlů) uspořádány do stromové struktury. 

![Podokno oboru](./media/storsimple-use-snapshot-manager/HCS_SSM_Scope_pane.png) 

* tooexpand nebo Sbalit uzel, klikněte na tlačítko hello šipku ikonu další toohello název uzlu.
* Stav hello tooview nebo obsah uzlu, klikněte na název uzlu hello. Hello informace se zobrazí v hello **výsledky** podokně. 

Hello **oboru** podokně obsahuje hello následující uzly: 

* [Uzlu zařízení](#devices-node) 
* [Svazky uzlu](#volumes-node) 
* [Uzel skupiny svazku](#volume-groups-node) 
* [Zálohování uzlu zásady](#backup-policies-node) 
* [Zálohování uzel katalogu](#backup-catalog-node) 
* [Uzel úlohy](#jobs-node) 

### <a name="scope-pane-tasks"></a>Obor podokna úloh
Můžete použít hello **oboru** toocomplete podokna akce na konkrétním uzlu. tooselect úlohy, proveďte jednu z následujících hello:

* Klikněte pravým tlačítkem na uzel hello a pak vyberte úlohu hello hello nabídce, která se zobrazí.
* Klikněte na uzel hello a pak klikněte na **akce** v řádku nabídek hello. Vyberte úkol hello hello nabídce, která se zobrazí.
* Klikněte na uzel hello a pak vyberte hello akce v hello **akce** podokně.

Když vyberete uzel a použít některou z těchto metod toosee seznam úkolů, se zobrazí pouze akce, které lze provést v tomto uzlu.

### <a name="devices-node"></a>Uzlu zařízení
Hello **zařízení** představuje uzel zařízení StorSimple hello a virtuální zařízení StorSimple, které jsou připojené tooStorSimple Snapshot Manager. Vyberte tento uzel tooconnect a konfigurace zařízení a importovat jeho přidružené svazky, svazky skupiny a existující záložní kopie. Více zařízení může být připojené tooa jednoho hostitele.

* tooexpand hello uzel, klikněte na ikonu šipky hello další příliš**zařízení**.
* toosee nabídky Dostupné akce, klikněte pravým tlačítkem na hello **zařízení** nebo klikněte pravým tlačítkem na uzel hello uzly, které se zobrazují v hello rozšířené zobrazení.
* Klikněte na tlačítko toosee seznam nakonfigurované zařízení **zařízení** v hello **oboru** podokně. Hello seznam zařízení, spolu s informacemi o každé zařízení se zobrazí v hello **výsledky** podokně.

### <a name="volumes-node"></a>Svazky uzlu
Hello **svazky** představuje uzel hello jednotky, které odpovídají toohello svazky připojené hello hostitele, včetně těch, které zjištěný prostřednictvím iSCSI a ty zjištěný prostřednictvím zařízení. Pomocí tohoto uzlu tooview hello seznamu dostupných svazků a přiřadit skupiny toovolume jednotlivé svazky.

* tooexpand hello uzel, klikněte na ikonu šipky hello další příliš**svazky**.
* toosee nabídky Dostupné akce, klikněte pravým tlačítkem na hello **svazky** nebo klikněte pravým tlačítkem na uzel hello uzly, které se zobrazují v hello rozšířené zobrazení.
* Klikněte na tlačítko toosee seznam svazků, **svazky** v hello **oboru** podokně. Hello seznam svazků, spolu s informacemi o každém svazku se zobrazí v hello **výsledky** podokně.

### <a name="volume-groups-node"></a>Uzel skupiny svazku
Svazek skupin se také označují jako konzistence skupin. Každá skupina svazek je fond svazků týkající se aplikací, který pomáhá konzistence aplikací tooensure během operace zálohování. Použití hello **svazku skupiny** uzlu tooconfigure tyto skupiny a interaktivní zálohy tootake nebo vytvořte plánů zálohování. 

* tooexpand hello uzel, klikněte na ikonu šipky hello další příliš**svazku skupiny**.
* toosee nabídky Dostupné akce, klikněte pravým tlačítkem na hello **svazku skupiny** nebo klikněte pravým tlačítkem na uzel hello uzly, které se zobrazují v hello rozšířené zobrazení.
* Klikněte na tlačítko toosee seznamu skupin svazku, **svazku skupiny** v hello **oboru** podokně. Hello seznam skupin svazku, spolu s informacemi o každé skupiny svazku se zobrazí v hello **výsledky** podokně.

### <a name="backup-policies-node"></a>Zálohování uzlu zásady
Zásady zálohování jsou plány úloh pro místní a cloudových snímků. Použití hello **zásady zálohování** toospecify uzlu, jak často se vytvoří zálohu a jak dlouho by měl být zálohu zachovány. 

* tooexpand hello uzel, klikněte na ikonu šipky hello další příliš**zásady zálohování**.
* toosee nabídky Dostupné akce, klikněte pravým tlačítkem na hello **zásady zálohování** nebo klikněte pravým tlačítkem na uzel hello uzly, které se zobrazují v hello rozšířené zobrazení.
* toosee seznam zásady zálohování, klikněte na tlačítko **zásady zálohování** v hello **oboru** podokně. Hello seznam zásady zálohování, spolu s informacemi o Každá zásada se zobrazí v hello **výsledky** podokně.

> [!NOTE]
> Můžete uchovávat maximálně 64 zálohy.


### <a name="backup-catalog-node"></a>Zálohování uzel katalogu
Hello **katalog zálohování** uzel obsahuje seznamy na místě a odlehlého zálohování svazků Azure StorSimple. Tento uzel je seřazená podle skupiny svazku a kontejneru skupiny pro každý svazek obsahuje samostatné struktury pro místní snímky (hello **místní snímek**uzlu s) a cloudových snímků (hello **cloudových snímků** uzlu ). Po rozbalení kontejneru skupiny pro každý svazek obsahuje seznam všech úspěšné zálohy hello, které byly provedeny interaktivně nebo nakonfigurované zásady.

* tooexpand hello uzel, klikněte na ikonu šipky hello další příliš**katalog zálohování**.
* toosee nabídky Dostupné akce, klikněte pravým tlačítkem na hello **katalog zálohování** nebo klikněte pravým tlačítkem na uzel hello uzly, které se zobrazují v hello rozšířené zobrazení.
* Klikněte na tlačítko toosee seznam snímků zálohy, **zálohování katalogu** v hello **oboru** podokně. Hello seznam snímky, spolu s informacemi o jednotlivých snímků, se zobrazí v hello **výsledky** podokně.

### <a name="local-snapshots-node"></a>Místní uzel snímky
Hello **místní snímky** uzel obsahuje seznam místních snímků pro skupinu konkrétní svazku. uzel Hello se nachází v hello **katalog zálohování** uzel v hello **oboru** podokně. Místní snímky jsou v daném okamžiku kopie svazku data, která jsou uložená na zařízení Azure StorSimple hello. Tento typ zálohy obvykle, můžete vytvořit a rychle obnovit. Stejně jako místní záložní kopii můžete použít místní snímek.

* tooexpand hello uzel, klikněte na ikonu šipky hello další příliš**místní snímky**.
* toosee nabídky Dostupné akce, klikněte pravým tlačítkem na hello **místní snímky** nebo klikněte pravým tlačítkem na uzel hello uzly, které se zobrazují v hello rozšířené zobrazení.
* Klikněte na tlačítko toosee seznam místních snímků **místní snímky** v hello **oboru** podokně. Hello seznam snímky, spolu s informacemi o jednotlivých snímků, se zobrazí v hello **výsledky** podokně.

### <a name="cloud-snapshots-node"></a>Cloudové snímky uzlu
Hello **cloudových snímků** uzlu uvádí cloudových snímků pro skupinu konkrétní svazku. uzel Hello se nachází v hello **katalog zálohování** uzel v hello **oboru** podokně. Cloudové snímky jsou v daném okamžiku kopie svazku data, která jsou uložená v cloudu hello. Cloudový snímek je ekvivalentní tooa snímku replikují na jiný, odlehlého úložiště systému. Cloudové snímky jsou obzvláště užitečná ve scénářích zotavení po havárii.

* tooexpand hello uzel, klikněte na ikonu šipky hello další příliš**cloudových snímků**.
* toosee nabídky Dostupné akce, klikněte pravým tlačítkem na hello **cloudových snímků** nebo klikněte pravým tlačítkem na uzel hello uzly, které se zobrazují v hello rozšířené zobrazení.
* Klikněte na tlačítko toosee seznam cloudových snímků **cloudových snímků** v hello **oboru** podokně. Hello seznam snímky, spolu s informacemi o jednotlivých snímků, se zobrazí v hello **výsledky** podokně.

### <a name="jobs-node"></a>Uzel úlohy
Hello **úlohy** uzel obsahuje informace o naplánované, spuštěné a nedávno dokončené úlohy zálohování. 

* tooexpand hello uzel, klikněte na ikonu šipky hello další příliš**úlohy**.
* toosee nabídky Dostupné akce, klikněte pravým tlačítkem na hello **úlohy** nebo klikněte pravým tlačítkem na uzel hello uzly, které se zobrazují v hello rozšířené zobrazení.
* toosee seznam naplánované úlohy, rozbalte položku hello **úlohy** uzel a pak klikněte na tlačítko **naplánovaná**. Hello seznam dříve nakonfigurované úlohy a informace o každé úloze se zobrazí v hello **výsledky** podokně. 
* toosee seznam nedávno dokončené úlohy, rozbalte položku hello **úlohy** uzel a pak klikněte na tlačítko **posledních 24 hodin**. Seznam úloh, které byly dokončeny v hello posledních 24 hodin se zobrazí v hello **výsledky** podokně. Hello **výsledky** podokně také obsahuje informace o každé dokončené úlohy.
* toosee seznam úloh, které jsou aktuálně spuštěné, rozbalte položku hello **úlohy** uzel a pak klikněte na tlačítko **systémem**. Hello seznam aktuálně spuštěných úloh a informace o každé úloze se zobrazí v hello **výsledky** podokně.

## <a name="results-pane"></a>Podokno výsledků
Hello **výsledky** podokno je hello prostředním podokně v hello StorSimple Snapshot Manager uživatelského rozhraní. Obsahuje seznamy a podrobné informace o stavu pro uzel hello jste vybrali v hello **oboru** podokně.

### <a name="example"></a>Příklad
hello toosee následující ukázka, klikněte na tlačítko hello **svazku skupiny** uzel v hello **oboru** podokně. Hello **výsledky** podokně se zobrazí seznam skupin svazek s podrobnostmi o každé skupiny.

![Podokno výsledků](./media/storsimple-use-snapshot-manager/HCS_SSM_Results_pane.png) 

Můžete nakonfigurovat hello podrobnosti uvedené v hello **výsledky** podokně: klikněte pravým tlačítkem na uzel v hello **oboru** podokně klikněte na tlačítko **zobrazení**a pak klikněte na tlačítko **přidat nebo odebrat Sloupce**.

## <a name="actions-pane"></a>Podokna akcí
Hello **akce** podokno je hello pravém podokně v hello StorSimple Snapshot Manager uživatelského rozhraní. Obsahuje nabídky operací, které můžete provádět na uzlu hello, zobrazení nebo data, která jste vybrali v hello **oboru** podokně nebo **výsledky** podokně. Hello **akce** podokně obsahuje hello stejné příkazy jako hello **akce** nabídky, které jsou dostupné pro položky v hello **oboru** podokně a **výsledky**podokně. Popis každé akce naleznete v tématu hello tabulky v hello **akce** části nabídky.

### <a name="examples"></a>Příklady
Následující ukázka v hello hello toosee **oboru** podokně rozbalte hello **úlohy** uzel a klikněte na tlačítko **naplánovaná**. Hello **akce** podokně zobrazí hello dostupné akce pro hello **naplánovaná** uzlu.

![Příklad naplánované úlohy podokna akce](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane.png) 

více možností toosee, v hello **oboru** podokně rozbalte hello **úlohy** uzel, klikněte na tlačítko **naplánovaná**a pak klikněte na naplánovanou úlohu v hello **výsledky**podokně. Hello **akce** podokně zobrazí hello dostupné akce pro hello naplánovanou úlohu, jak ukazuje následující příklad hello.

![Příklad použití akce úlohy v podokně Akce](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane_Results.png)

## <a name="keyboard-navigation-and-shortcuts"></a>Navigace klávesnice a zkratky
StorSimple Snapshot Manager umožňuje hello funkce usnadnění programu operačního systému Windows hello a hello Microsoft Management Console (MMC). Zahrnuje také některé vlastnosti navigace klávesnice a zkratky, které jsou specifické toohello Snapshot Manager zařízení StorSimple, jak je popsáno v následující části hello.

* [Navigační klávesy klávesnice](#keyboard-navigation-keys) 
* [Řádek nabídek klávesové zkratky](#menu-bar-shortcut-keys) 
* [Obor podokně klávesové zkratky](#scope-pane-shortcut-keys) 

### <a name="keyboard-navigation-keys"></a>Navigační klávesy klávesnice
Hello následující tabulka popisuje hello klíče, které můžete použít toonavigate hello StorSimple Snapshot Manager uživatelské rozhraní. 

| Navigační klávesy | Akce |
|:--- |:--- |
| Šipka dolů |Použít hello dolů klíče toomove šipku svisle toohello další položky v nabídce nebo podokně. |
| Zadejte |Stiskněte klávesu hello Enter klíče toocomplete akce a poté pokračujte dalším krokem toohello. Například stisknutím klávesy Enter tooselect **Další**, **OK**, nebo **vytvořit**, a potom přejděte toohello další krok průvodce. |
| ESC |Stiskněte klávesu hello Esc klíče tooclose nabídky nebo toocancel a zavřete stránku. |
| F1 |Stiskněte hello F1 klíče tooview tématu nápovědy pro aktuálně aktivní okno hello. |
| F5 |Stisknutím klávesy hello F5 klíče toorefresh uzlu. |
| F6 |Stiskněte klávesu hello F6 klíče toomove z hello **oboru** podokně toohello **výsledky** podokně. |
| F10 |Stisknutím klávesy hello F10 klíče toogo toohello nabídce. |
| Klávesy šipka vlevo |Použijte hello zbývající klíče toomove šipku vodorovně z z panelu možnost toohello předchozí nabídky. Při přesunutí toohello předchozí se zobrazí v hello nabídce panelu, akce hello (nebo kontextu) nabídky hello předchozí položky. |
| Klávesy šipka vpravo |Použijte hello šipku vpravo klíče toomove vodorovně z jedné nabídky panelu toohello možnost Další. Při přesunutí se zobrazí toohello další nabídky v hello nabídce panelu, akce hello (nebo kontextu) pro novou položku hello. |
| Klávesy TAB |Použijte hello karta klíče toomove toohello další podokno na hello konzole nebo toohello další výběr nebo textového pole na stránce. |
| Klávesa Šipka nahoru |Použít hello až klíče toomove šipku svisle toohello předchozí položky v nabídce nebo podokně. |

### <a name="menu-bar-shortcut-keys"></a>Řádek nabídek klávesové zkratky
Hello následující tabulka popisuje hello kombinace klávesových zkratek pro hello řádku nabídek. Po stisknutí klávesy hello klávesové zkratky a otevře se nabídka hello, můžete použít nabídku klávesové zkratky (hello podtržené klíče v nabídce hello). Další informace o řádku nabídek hello přejděte příliš[řádku nabídek](#menu-bar).

| Zástupce | výsledek | Klávesová zkratka nabídky | výsledek |
|:--- |:--- |:--- |:--- |
| ALT + F |Otevře se okno hello **souboru** nabídky. |N |Otevře se nové instance konzoly. |
|  |O |Otevře se okno hello **nástroje pro správu** stránky. | |
|  |S |Uloží hello StorSimple Snapshot Manager konzoly. | |
|  |A |Otevře se okno hello **uložit jako** stránky. | |
|  |M |Otevře se okno hello **přidat nebo odebrat modul Snap-in** stránky. | |
|  |P |Otevře se okno hello **možnosti** stránky. | |
|  |H |Otevře se online nápovědy. | |
| ALT + A |Otevře se okno hello **akce** nabídky. |I |Možnost zobrazení import hello vypíná a zapíná. |
|  |W |Otevře se nové konzoly StorSimple Snapshot Manager. | |
|  |F |Aktualizace konzoly hello StorSimple Snapshot Manager. | |
|  |L |Otevře se okno hello **exportovat seznam** stránky. | |
|  |H |Otevře se online nápovědy. | |
| ALT + V |Otevře se okno hello **zobrazení** nabídky. |A |Otevře se okno hello **přidat nebo odebrat sloupce** stránky. |
|  |U |Otevře se okno hello **přizpůsobit zobrazení** stránky. | |
| ALT + O |Otevře se okno hello **Oblíbené** nabídky. |A |Otevře se okno hello **přidat tooFavorites** stránky. |
|  |O |Otevře se okno hello **uspořádat oblíbené položky** stránky. | |
| ALT + W |Otevře se okno hello **okno** nabídky. |N |Otevře další okno StorSimple Snapshot Manager. |
|  |C |Zobrazí všechny otevřené konzoly windows ve stylu CSS. | |
|  |T |Zobrazí všechny otevřené konzoly windows ve tvaru mřížky. | |
|  |I |Seřadí ikony na vodorovném řádku v hello dolní části obrazovky. | |
| ALT + H |Otevře se okno hello **pomoci** nabídky. |H |Otevře se online nápovědy. |
|  |T |Otevře stránku hello Center technologie Microsoft TechNet. | |
|  |A |Otevře se okno hello **o konzoly Microsoft Management Console** stránky. | |

### <a name="scope-pane-shortcut-keys"></a>Obor podokně klávesové zkratky
Hello následující tabulky popisují hello zástupce kombinace kláves pro každý uzel v hello **oboru** podokně. 

* [Klávesové zkratky uzlu zařízení](#devices-node-shortcut-keys)
* [Svazky uzlu klávesové zkratky](#volumes-node-shortcut-keys)
* [Svazek skupiny uzlu klávesové zkratky](#volume-groups-node-shortcut-keys)
* [Zálohování zásady uzlu klávesové zkratky](#backup-policies-node-shortcut-keys)
* [Zálohování katalogu uzlu klávesové zkratky](#backup-catalog-node-shortcut-keys)
* [Úlohy uzlu klávesové zkratky](#jobs-node-shortcut-keys)

#### <a name="devices-node-shortcut-keys"></a>Klávesové zkratky uzlu zařízení
| Místní nabídky | výsledek |
|:--- |:--- |
| C |Otevře se okno hello **nakonfigurovat zařízení** stránky. |
| D |Aktualizuje seznam hello zařízení a podrobnosti o zařízení. |
| V |Otevře se okno hello **zobrazení** nabídky. |
| W |Otevře se nové konzoly StorSimple Snapshot Manager zaměřené na hello **podrobnosti** uzlu. |
| F |Aktualizace konzoly hello StorSimple Snapshot Manager. |
| L |Otevře se okno hello **exportovat seznam** stránky. |
| H |Otevře se online nápovědy. |

#### <a name="volumes-node-shortcut-keys"></a>Svazky uzlu klávesové zkratky
| Místní nabídky | výsledek |
|:--- |:--- |
| V |Aktualizace hello seznam svazků. |
| V (dvakrát stiskněte) |Otevře se okno hello **zobrazení** nabídky. |
| W |Otevře se nové konzoly StorSimple Snapshot Manager zaměřené na hello **svazky** uzlu. |
| F |Aktualizace konzoly hello StorSimple Snapshot Manager. |
| L |Otevře se okno hello **exportovat seznam** stránky. |
| H |Otevře se online nápovědy. |

#### <a name="volume-groups-node-shortcut-keys"></a>Svazek skupiny uzlu klávesové zkratky
| Místní nabídky | výsledek |
|:--- |:--- |
| G |Otevře se okno hello **vytvořte skupinu svazku** stránky. |
| V |Otevře se okno hello **zobrazení** nabídky. |
| W |Otevře se nové konzoly StorSimple Snapshot Manager zaměřené na hello **svazku skupiny** uzlu. |
| F |Aktualizace konzoly hello StorSimple Snapshot Manager. |
| L |Otevře se okno hello **exportovat seznam** stránky. |
| H |Otevře se online nápovědy. |

#### <a name="backup-policies-node-shortcut-keys"></a>Zálohování zásady uzlu klávesové zkratky
| Místní nabídky | výsledek |
|:--- |:--- |
| B |Otevře se okno hello **vytvořit zásadu** stránky. |
| V |Otevře se okno hello **zobrazení** nabídky. |
| W |Otevře se nové konzoly StorSimple Snapshot Manager zaměřené na hello **svazku skupiny** uzlu. |
| F |Aktualizace konzoly hello StorSimple Snapshot Manager. |
| L |Otevře se okno hello ** exportovat seznam ** stránky. |
| H |Otevře se online nápovědy. |

#### <a name="backup-catalog-node-shortcut-keys"></a>Zálohování katalogu uzlu klávesové zkratky
| Místní nabídky | výsledek |
|:--- |:--- |
| W |Otevře se nové konzoly StorSimple Snapshot Manager zaměřené na hello **svazku skupiny** uzlu. |
| F |Aktualizace konzoly hello StorSimple Snapshot Manager. |
| H |Otevře se online nápovědy. |

#### <a name="jobs-node-shortcut-keys"></a>Úlohy uzlu klávesové zkratky
| Místní nabídky | výsledek |
|:--- |:--- |
| V |Otevře se okno hello **zobrazení** nabídky. |
| W |Otevře se nové konzoly StorSimple Snapshot Manager zaměřené na hello **úlohy** uzlu. |
| F |Aktualizace konzoly hello StorSimple Snapshot Manager. |
| L |Otevře se okno hello **exportovat seznam** stránky. |
| H |Otevře online nápovědy |

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[pomocí vašeho řešení StorSimple Snapshot Manager zařízení StorSimple tooadminister](storsimple-snapshot-manager-admin.md).
* Zjistěte, jak příliš[použít tooconnect Snapshot Manager zařízení StorSimple a spravovat zařízení](storsimple-snapshot-manager-manage-devices.md).

