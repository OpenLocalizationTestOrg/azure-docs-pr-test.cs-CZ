---
title: "Microsoft Threat modelování nástroj – Azure | Microsoft Docs"
description: "Další informace o všech funkcí, které jsou k dispozici v nástroji pro modelování hrozeb"
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
ms.openlocfilehash: 621ff305d7e782f85eeaae6c3fb02031673549c6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="threat-modeling-tool-feature-overview"></a>Přehled funkcí nástroje modelování hrozeb

Jsme rádi, že jste se rozhodli použít nástroj modelování hrozeb pro vaše threat modelování potřebám! Pokud jste tak dosud neučinili, navštivte  **[Začínáme s nástrojem modelování hrozeb](./azure-security-threat-modeling-tool-getting-started.md)**  základní informace.

> Naše nástroj se často aktualizuje, proto tento často najdete v příručce pro naše nejnovější funkce a vylepšení.

Kliknutím na tlačítko "Vytvořit nový Model" otevře prázdné úvodní stránka, podobně jako na následujícím obrázku:

![Prázdné úvodní stránky](./media/azure-security-threat-modeling-tool/tmtstart.png)

Pomocí model hrozeb vytvořené náš tým v  **[Začínáme](./azure-security-threat-modeling-tool-getting-started.md)**  příkladu budeme najdete všechny funkce, které jsou k dispozici v nástroji ještě dnes.

![Model základní hrozeb](./media/azure-security-threat-modeling-tool/basictmt.png)

## <a name="navigation"></a>Navigace

Než začnete integrované funkce, přejděte přes hlavními součástmi najít v nástroji

### <a name="menu-items"></a>Položky nabídky

Prostředí by měl vypadat přibližně další produkty společnosti Microsoft. Začněme prostřednictvím položky nabídky nejvyšší úrovně:

![Položky nabídky](./media/azure-security-threat-modeling-tool/menuitems.png)

| Štítek                               | Podrobnosti      |
| --------------------------------------- | ------------ |
| **File** | <ul><li>Otevřít, uložte a zavřete soubory</li><li>Účty přihlášení volitelném OneDrive</li><li>Sdílet odkazy (zobrazení a úpravy)</li><li>Zobrazení informací o souboru</li><li>Použít novou šablonu na existující modely</li></ul> |
| **Upravit** | Zpět/opakování akce, jako dobře kopii, vkládání a odstraňování |
| **Zobrazení** | <ul><li>Přepínání mezi **Analysis** a **návrhu** zobrazení</li><li>Otevřete uzavřené windows (e.g.stencils, vlastností elementů a zprávy)</li><li>Obnovit výchozí nastavení rozložení</li></ul> |
| **Diagram** | Přidání nebo odstranění diagramy a procházení "karty" diagramů |
| **Sestavy** | Vytváření sestav HTML sdílet s ostatními uživateli |
| **Pomoc** | Provede můžete použít nástroj |

Ikony jsou zkratky pro nejvyšší úrovně nabídky:

| Ikona                               | Podrobnosti      |
| --------------------------------------- | ------------ |
| **Otevřete** | Otevře se nový soubor |
| **Uložit** | Uloží aktuální soubor |
| **Návrh** | Klient se přepne do zobrazení návrhu, kde můžete vytvořit modely |
| **Analýza** | Ukazuje generované hrozby a jejich vlastnosti |
| **Přidat Diagram** | Přidá nový diagram (podobně jako nové karty v aplikaci Excel) |
| **Odstranit Diagram** | Odstraní aktuální diagram |
| **Vyjmutí/kopírování/vkládání** | Kopie nebo kusy/vloží elementy |
| **Vrátit/opakovat** | Vrátí zpět nebo znovu provede akce |
| **Přiblížení / oddálení** | Zvětší směřující diagram pro lepší zobrazení |
| **Váš názor** | Otevře na fóru MSDN |

### <a name="canvas"></a>Plátno

Místa, kde můžete přetáhnout myší elementy do. Přetažení je nejrychlejší a nejúčinnější způsob, jak vytvářet modely. Může také klikněte pravým tlačítkem a vyberte v nabídce, která přidává obecné verzích prvky, které používáte, jak je uvedeno níže.

#### <a name="dropping-the-stencil-on-the-canvas"></a>Vyřazení vzorníku na plátno

![Vyřaďte plátno](./media/azure-security-threat-modeling-tool/canvasdrop1.png)

#### <a name="clicking-on-the-stencil"></a>Kliknutím na vzorníku

![Vlastnosti elementů](./media/azure-security-threat-modeling-tool/canvasdrop2.png)

### <a name="stencils"></a>Vzorníky

Kde můžete najít všechny vzorníky k dispozici pro použití podle vybraná šablona. Pokud nemůžete najít správné elementy, zkuste použít jinou šablonu nebo měnit podle vlastních potřeb. Obecně byste měli nemůže najít kombinaci kategorie, jako jsou následující:

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
| **Poznámky k** | Ruční poznámky přidaných do souboru vývojové týmy během celého procesu návrhu a revize |

### <a name="element-properties"></a>Vlastnosti elementů

To se liší podle vybrané elementy. Kromě hranice vztahu důvěryhodnosti všechny ostatní elementy obsahovat 3 obecné možnosti:

| Vlastnost element                               | Podrobnosti      |
| --------------------------------------- | ------------ |
| **Název** | Užitečné pro pojmenování vaší procesů, úložiště, interactors a toky snadno rozpoznat |
| **Mimo rozsah** | Pokud vyberete, element se dostala mimo matici hrozbu generace (nedoporučuje se) |
| **Důvod mimo rozsah** | Při zarovnání do bloku pole umožníte uživatelům vědět, proč mimo obor nebyla vybrána. |

Vlastnosti jsou změnit v rámci každé kategorie elementu. Klikněte na každý prvek zkontrolovat dostupné možnosti, nebo otevřete šablonu, kterou chcete získat další informace. Pojďme do funkcí.

## <a name="welcome-screen"></a>Úvodní obrazovka

Na úvodní obrazovce je první věcí, kterou najdete v části při otevření aplikace.

### <a name="open-a-model"></a>Otevřete model

Ukazatele myši na tlačítko "Otevřete modelu" ukazuje možnosti 2 Skrytá: "Otevřete z tento počítač" a "Otevřete z Onedrivu." První otevře soubor otevřete obrazovku, zatímco druhý vás provede v procesu přihlašování pro OneDrive, umožní vám vybrat složky a soubory. Po úspěšném ověření.

![Otevřete modelu](./media/azure-security-threat-modeling-tool/openmodel.png)

![Otevřete v počítači nebo OneDrive](./media/azure-security-threat-modeling-tool/openmodel2.png)

### <a name="feedback-suggestions-and-issues"></a>Zpětná vazba, návrhy a problémy

Výběrem této možnosti se dostanete na fórech MSDN nástroje SDL. Je skvělým způsobem, jak podívejte se na ostatní uživatelé názory týkající se nástroje, včetně řešení a nových nápadů.

![Váš názor](./media/azure-security-threat-modeling-tool/feedback.png)

## <a name="design-view"></a>Návrhové zobrazení

Při každém otevření nebo vytvořit nový model, budete přesměrováni na zobrazení návrhu.

### <a name="adding-elements"></a>Přidávání elementů

Chcete-li přidat elementů v mřížce 2 způsoby:

- **Přetáhnout myší** – přetáhněte požadované elementu k mřížce, potom použijte vlastností elementů můžete poskytnout dodatečné informace.
- **Klikněte pravým tlačítkem na** – klikněte pravým tlačítkem kamkoli na mřížky a vyberte z rozevírací nabídky. Obecná reprezentace tohoto prvku se zobrazí na obrazovce.

### <a name="connecting-elements"></a>Připojení elementy

Prvky v nástroji propojit 2 způsoby:

- **Přetáhnout myší** – přetáhněte požadovaného toku dat do mřížky a připojte se k příslušné elementy oba elementy end.
- **Klikněte na tlačítko + Shift** – klikněte na první prvek (odesílání dat), stiskněte a podržte klávesu Shift a potom vyberte druhý prvkem (přijetí dat). Klikněte pravým tlačítkem a vyberte možnost "Připojit". Pokud používáte toku dat obousměrný, není pořadí jako důležité.

### <a name="properties"></a>Vlastnosti

Zobrazuje všechny vlastnosti, které je možné upravit v vzorníky umístěny v diagramu. Pokud chcete zobrazit vlastnosti, právě klikněte na vzorníku a informace vyplní odpovídajícím způsobem. Následující příklad ukazuje před a po "Databáze" vzorníku je přetáhli diagramu:

#### <a name="before"></a>Před

![Před](./media/azure-security-threat-modeling-tool/properties1.png)

#### <a name="after"></a>Po

![Po](./media/azure-security-threat-modeling-tool/properties2.png)

### <a name="messages"></a>Zprávy

Pokud chcete vytvořit model hrozeb a nezapomněli připojení k prvkům toky dat, v okně zprávy upozorní vás tak, aby fungoval. Můžete ho ignorovat nebo postupujte podle pokynů a vyřešte problém. 

![Zprávy](./media/azure-security-threat-modeling-tool/messages.png)

### <a name="notes"></a>Poznámky

Přepínání karty ze zprávy k poznámkám umožňuje přidání poznámky do diagramu zaznamenat všechny své myšlenky

## <a name="analysis-view"></a>Zobrazení analýzy

Jakmile jste hotovi sestavování diagramu, přejít do zobrazení analysis přejdete na možnosti horní nabídce a zvolením lupy vedle palety Malování.

![Zobrazení analýzy](./media/azure-security-threat-modeling-tool/analysisview.png)

### <a name="generated-threat-selection"></a>Výběr generovaného hrozeb

Když kliknete na hrozbu, můžete využít tři jedinečné funkce:

| Funkce                               | Informace      |
| --------------------------------------- | ------------ |
| **Indikátor pro čtení** | <p>Hrozby je nyní označena jako pro čtení, který lze snadno pomoct tak sledovat položky, které už se prostřednictvím</p><p>![Pro čtení nebo nepřečtená indikátoru](./media/azure-security-threat-modeling-tool/readmode.png)</p> |
| **Interakce fokusu** | <p>Zvýrazní interakci v diagramu patřící do této hrozby</p><p>![Interakce fokusu](./media/azure-security-threat-modeling-tool/interactionfocus.png)</p> |
| **Vlastnosti hrozeb** | <p>Další informace o riziko, že se importují v okně vlastností hrozeb</p><p>![Vlastnosti hrozeb](./media/azure-security-threat-modeling-tool/threatproperties.png)</p> |

### <a name="priority-change"></a>Priorita změny

Změna úrovně priority jednotlivé generovaného hrozby také změní jejich barvy snadno identifikovat vysoká, střední a nízké priority hrozeb.

![Priorita změny](./media/azure-security-threat-modeling-tool/prioritychange.png)

### <a name="threat-properties-editable-fields"></a>Upravitelné pole vlastnosti hrozeb

Jak je vidět na předchozím obrázku, uživatelé mohou změnit informace generovaný nástrojem také přidat informace do určitá pole, jako je například zarovnání do bloku. Tato pole jsou generovány šablonou, takže pokud potřebujete další informace na jednotlivé hrozby, můžete se doporučuje, aby změny.

![Vlastnosti hrozeb](./media/azure-security-threat-modeling-tool/threatproperties.png)

## <a name="reports"></a>Reports

Po dokončení změny priority a aktualizaci stavu jednotlivé generovaného hrozby, můžete soubor uložit nebo vytisknout sestavu tak, že přejdete na "Zpráva" a potom "Vytvořit úplná sestava." Zobrazí se výzva k název sestavy a až to uděláte, měli byste vidět něco podobného jako na následujícím obrázku:

![Sestava](./media/azure-security-threat-modeling-tool/report.png)

## <a name="next-steps"></a>Další kroky

Přispívat šablonu pro komunity, přejděte prosím do naší  **[Githubu](https://github.com/Microsoft/threat-modeling-templates)**  stránky. **[Stáhněte si](https://aka.ms/tmtpreview)**  nástroje a začněte ještě dnes.
