---
title: "aaaMicrosoft nástroj pro modelování hrozeb – Azure | Microsoft Docs"
description: "Další informace o všech hello funkce dostupné v hello nástroj modelování hrozeb"
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: f9ad5e623e7758063084cb7fc723c5735161a846
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="threat-modeling-tool-feature-overview"></a>Přehled funkcí nástroje modelování hrozeb

Jsme rádi, že jste zvolili toouse hello nástroj modelování hrozeb pro vaše threat modelování potřebám! Pokud jste tak dosud neučinili, navštivte  **[Začínáme s hello nástroj modelování hrozeb](./azure-security-threat-modeling-tool-getting-started.md)**  toolearn hello základy.

> Naše nástroj se často aktualizuje, takže zaškrtněte toto políčko Průvodce často toosee naše nejnovější funkce a vylepšení.

Kliknutím na tlačítko "Vytvořit nový Model" hello otevře prázdné úvodní stránky, podobně jako toohello obrázek níže:

![Prázdné úvodní stránky](./media/azure-security-threat-modeling-tool/tmtstart.png)

Pomocí model hrozeb hello vytvořené náš tým v hello  **[Začínáme](./azure-security-threat-modeling-tool-getting-started.md)**  příkladu budeme podívejte se na všechny dostupné v nástroji hello hello funkce ještě dnes.

![Model základní hrozeb](./media/azure-security-threat-modeling-tool/basictmt.png)

## <a name="navigation"></a>Navigace

Než začnete hello integrované funkce, přejděte přes nebyly nalezeny v nástroji hello hlavní komponenty hello

### <a name="menu-items"></a>Položky nabídky

Hello prostředí by měl být podobné tooother produkty společnosti Microsoft. Začněme prostřednictvím položky nabídky nejvyšší úrovně hello:

![Položky nabídky](./media/azure-security-threat-modeling-tool/menuitems.png)

| Štítek                               | Podrobnosti      |
| --------------------------------------- | ------------ |
| **File** | <ul><li>Otevřít, uložte a zavřete soubory</li><li>Účty přihlášení volitelném OneDrive</li><li>Sdílet odkazy (zobrazení a úpravy)</li><li>Zobrazení informací o souboru</li><li>Použít novou šablonu tooExisting modely</li></ul> |
| **Upravit** | Zpět/opakování akce, jako dobře kopii, vkládání a odstraňování |
| **Zobrazení** | <ul><li>Přepínání mezi **Analysis** a **návrhu** zobrazení</li><li>Otevřete uzavřené windows (e.g.stencils, vlastností elementů a zprávy)</li><li>Resetovat nastavení toodefault rozložení</li></ul> |
| **Diagram** | Přidání nebo odstranění diagramy a procházení "karty" diagramů |
| **Sestavy** | Vytvoření sestavy tooshare HTML s ostatními uživateli |
| **Pomoc** | Provede toohelp použijete nástroj hello |

zástupce pro hello nejvyšší úrovně nabídky jsou ikony Hello:

| Ikona                               | Podrobnosti      |
| --------------------------------------- | ------------ |
| **Otevřete** | Otevře se nový soubor |
| **Uložit** | Uloží aktuální soubor |
| **Návrh** | Klient se přepne do zobrazení návrhu, kde můžete vytvořit modely |
| **Analýza** | Ukazuje generované hrozby a jejich vlastnosti |
| **Přidat Diagram** | Přidá nový diagram (podobně jako toonew karty v aplikaci Excel) |
| **Odstranit Diagram** | Odstraní aktuální diagram |
| **Vyjmutí/kopírování/vkládání** | Kopie nebo kusy/vloží elementy |
| **Vrátit/opakovat** | Vrátí zpět nebo znovu provede akce |
| **Přiblížení / oddálení** | Zvětší směřující hello diagram pro lepší zobrazení |
| **Váš názor** | Otevře hello fórum MSDN |

### <a name="canvas"></a>Plátno

Hello místa, kde můžete přetáhnout myší elementy do. Přetažení je hello nejrychlejší a nejúčinnější způsob, jak toobuild modelů. Může také klikněte pravým tlačítkem a vyberte z nabídky hello, které se přidá obecné verze hello prvků, které používáte, jak je uvedeno níže.

#### <a name="dropping-hello-stencil-on-hello-canvas"></a>Vyřazení hello vzorníku na plátně hello

![Vyřaďte plátno](./media/azure-security-threat-modeling-tool/canvasdrop1.png)

#### <a name="clicking-on-hello-stencil"></a>Kliknutím na vzorníku hello

![Vlastnosti elementů](./media/azure-security-threat-modeling-tool/canvasdrop2.png)

### <a name="stencils"></a>Vzorníky

Tam, kde můžete najít všechny předlohy, k dispozici toouse podle vybraná šablona hello. Pokud nemůžete najít správné elementy hello, zkuste použít jinou šablonu, nebo upravit jeden toofit vašim potřebám. Obecně platí musí být schopný toofind kombinací kategorií jako hello ty, které jsou níže:

| Název vzorníku                               | Podrobnosti      |
| --------------------------------------- | ------------ |
| **Proces** | Aplikace, moduly plug-in prohlížeče, vláken, virtuální počítače |
| **Externí interakce** | Zprostředkovatelé ověřování, prohlížečů, uživatelé, webové aplikace |
| **Úložiště dat** | Mezipaměť, úložiště, konfigurační soubory, databáze, registru |
| **Tok dat** | Binární, ALPC, HTTP, HTTPS, protokol TLS/SSL, IOCTL, IPSec, s názvem kanálu, RPC/DCOM, protokol SMB, UDP |
| **Vztah důvěryhodnosti hranic čáry či ohraničení** | Podnikové sítě, Internet, počítače, izolovaného prostoru, režimu uživatele nebo jádra |

### <a name="notesmessages"></a>Poznámky a zprávy

| Komponenta                               | Podrobnosti      |
| --------------------------------------- | ------------ |
| **Zprávy** | Interní nástroj pro logiku, která upozorní uživatele, vždy, když dojde k chybě, jako je například žádná data proudí mezi elementy |
| **Poznámky k** | Ruční poznámky přidané toohello soubor engineering týmy celková hello návrhu a proces zkontrolovat |

### <a name="element-properties"></a>Vlastnosti elementů

To se liší podle vybrané elementy hello. Kromě hranice vztahu důvěryhodnosti všechny ostatní elementy obsahovat 3 obecné možnosti:

| Vlastnost element                               | Podrobnosti      |
| --------------------------------------- | ------------ |
| **Název** | Užitečné pro pojmenování vaší procesů, úložiště, interactors a toky toobe snadno rozpoznat. |
| **Mimo rozsah** | Pokud vyberete, hello element se dostala mimo hello threat generování matice (nedoporučuje se) |
| **Důvod mimo rozsah** | Při zarovnání do bloku pole toolet uživatelé věděli, proč je mimo rozsah byla vybrána |

Vlastnosti jsou změnit v rámci každé kategorie elementu. Klikněte na dostupné možnosti každý element tooinspect hello nebo otevřít další toolearn šablony hello. Pojďme do funkce hello.

## <a name="welcome-screen"></a>Úvodní obrazovka

úvodní obrazovka Hello je první krok text hello, který se zobrazí při otevření aplikace hello.

### <a name="open-a-model"></a>Otevřete model

Ukazatele myši na tlačítko "Otevřete modelu" ukazuje možnosti 2 Skrytá: "Otevřete z tento počítač" a "Otevřete z Onedrivu." Hello nejprve otevře úvodní obrazovka otevřít soubor, zatímco hello druhý vás provede hello v procesu přihlašování pro OneDrive, což vám toopick složek a souborů po úspěšném ověření.

![Otevřete modelu](./media/azure-security-threat-modeling-tool/openmodel.png)

![Otevřete v počítači nebo OneDrive](./media/azure-security-threat-modeling-tool/openmodel2.png)

### <a name="feedback-suggestions-and-issues"></a>Zpětná vazba, návrhy a problémy

Výběrem této možnosti se dostanete fórech MSDN toohello nástroje SDL. Je toocheck skvělý způsob, jak jiní lidé produkt hello nástroj, včetně řešení a nových nápadů.

![Váš názor](./media/azure-security-threat-modeling-tool/feedback.png)

## <a name="design-view"></a>Návrhové zobrazení

Při každém otevření nebo vytvořit nový model, budete přesměrováni toohello zobrazení návrhu.

### <a name="adding-elements"></a>Přidávání elementů

Způsoby 2 tooadd elementů v mřížce hello:

- **Přetáhnout myší** – přetáhněte hello požadovaný element toohello mřížky, potom použijte hello vlastnosti tooprovide Další informace o elementu.
- **Klikněte pravým tlačítkem na** – klikněte pravým tlačítkem kamkoli na hello mřížky a vyberte z rozevírací nabídky hello. Obecná reprezentace tohoto prvku se zobrazí na úvodní obrazovka.

### <a name="connecting-elements"></a>Připojení elementy

Způsoby 2 tooconnect prvky v nástroji hello:

- **Přetáhnout myší** – přetáhněte hello požadovaného toku dat toohello mřížky a připojte oba elementy end toohello příslušných prvků.
- **Klikněte na tlačítko + Shift** – klikněte na první prvek hello (odesílání dat), stiskněte a podržte klávesu Shift hello a pak vyberte hello druhý prvkem (přijetí dat). Klikněte pravým tlačítkem a vyberte možnost "Připojit". Pokud používáte toku dat obousměrný, hello pořadí není jako důležité.

### <a name="properties"></a>Vlastnosti

Zobrazuje všechny hello vlastnosti, které je možné upravit v hello vzorníky umístěny v diagramu hello. Vlastnosti hello toosee, právě klikněte na hello vzorníku a hello informace vyplní odpovídajícím způsobem. Následující příklad Hello ukazuje před a po "Databáze" vzorníku je přetáhli hello diagram:

#### <a name="before"></a>Před

![Před](./media/azure-security-threat-modeling-tool/properties1.png)

#### <a name="after"></a>Po

![Po](./media/azure-security-threat-modeling-tool/properties2.png)

### <a name="messages"></a>Zprávy

Pokud chcete vytvořit model hrozeb a zapomněli, že tooconnect data proudí tooelements, upozorní vás okno zprávy hello tooact. Můžete si vybrat tooignore ji nebo postupujte podle pokynů toofix hello problém hello. 

![Zprávy](./media/azure-security-threat-modeling-tool/messages.png)

### <a name="notes"></a>Poznámky

Přepínání karty ze zprávy tooNotes umožňuje vám tooadd poznámky tooyour diagram toocapture své myšlenky

## <a name="analysis-view"></a>Zobrazení analýzy

Jakmile jste hotovi sestavování diagramu, zobrazení tooanalysis přejít budete vybrané možnosti toohello horní nabídce a zvolením hello lupy další toohello Malování palety.

![Zobrazení analýzy](./media/azure-security-threat-modeling-tool/analysisview.png)

### <a name="generated-threat-selection"></a>Výběr generovaného hrozeb

Když kliknete na hrozbu, můžete využít tři jedinečné funkce:

| Funkce                               | Informace      |
| --------------------------------------- | ------------ |
| **Indikátor pro čtení** | <p>Hrozby je nyní označena jako pro čtení, který lze snadno v aplikaci sledování hello položky, které už se prostřednictvím</p><p>![Pro čtení nebo nepřečtená indikátoru](./media/azure-security-threat-modeling-tool/readmode.png)</p> |
| **Interakce fokusu** | <p>Zvýrazní interakci v diagramu hello patřící toothat hrozeb</p><p>![Interakce fokusu](./media/azure-security-threat-modeling-tool/interactionfocus.png)</p> |
| **Vlastnosti hrozeb** | <p>Další informace o ohrožení hello je vložené do hello threat vlastnosti – okno</p><p>![Vlastnosti hrozeb](./media/azure-security-threat-modeling-tool/threatproperties.png)</p> |

### <a name="priority-change"></a>Priorita změny

Změna úrovně priority hello jednotlivé generovaného hrozby také změní jejich barvy toomake ho snadno tooidentify vysoká, střední a nízké priority hrozeb.

![Priorita změny](./media/azure-security-threat-modeling-tool/prioritychange.png)

### <a name="threat-properties-editable-fields"></a>Upravitelné pole vlastnosti hrozeb

Jak je vidět v hello obrázku výše, uživatelé mohou změnit hello informace generované nástrojem hello také přidat pole toocertain informace, jako je například zarovnání do bloku. Tato pole jsou generovány šablonou hello, takže pokud potřebujete další informace na jednotlivé hrozby, jste podporovali toomake úpravy.

![Vlastnosti hrozeb](./media/azure-security-threat-modeling-tool/threatproperties.png)

## <a name="reports"></a>Reports

Po dokončení změny priority a aktualizuje hello stav každého z nich generuje hrozeb, můžete soubor hello uložit nebo vytisknout sestavu tak, že budete příliš "Oznamovat" a potom "vytvořit úplná sestava." Zobrazí se výzva tooname hello sestavy a až to uděláte, měli byste vidět něco podobného toohello obrázku níže:

![Sestava](./media/azure-security-threat-modeling-tool/report.png)

## <a name="next-steps"></a>Další kroky

toocontribute šablonu pro hello komunity, přejděte prosím tooour  **[Githubu](https://github.com/Microsoft/threat-modeling-templates)**  stránky. **[Stáhněte si](https://aka.ms/tmtpreview)**  tooget hello nástroj spustit ještě dnes.
