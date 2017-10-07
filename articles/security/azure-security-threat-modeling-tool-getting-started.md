---
title: "aaaGetting Začínáme - Microsoft Threat modelování nástroj – Azure | Microsoft Docs"
description: "Toto je hlubší přehled zvýraznění hello nástroj modelování hrozeb v akci."
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
ms.openlocfilehash: 75ef139071e8abd0e743aa17b443a6e353f29372
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-hello-threat-modeling-tool"></a>Začínáme s hello nástroj modelování hrozeb

cloudové a podnikové zabezpečení nástroje team Hello vydané hello Preview nástroj modelování hrozeb na začátku roku jako bezplatný  **[klikněte na tlačítko Stáhnout](https://aka.ms/tmtpreview)**. Hello změnu v hodnotě mechanismus doručení umožňuje nám toocustomers hello toopush nejnovější vylepšení a opravy chyb při každém otevření hello nástroj, což snadněji toomaintain a použití.
Tento článek vás provede procesem hello Začínáme s threat Microsoft SDL hello modelování přístup a ukazuje, jak toouse hello nástroj toodevelop skvělé threat modely jako páteřní zabezpečení procesu.

Tento článek je založený na stávajících znalostí threat SDL hello modelování přístup. Rychlý přehled najdete v části příliš**[modelování ohrožení webových aplikací](https://msdn.microsoft.com/library/ms978516.aspx)**  a archivované verze  **[odkrýt pomocí nedostatky zabezpečení hello STRIDE přístup](https://docs.google.com/viewer?a=v&pid=sites&srcid=ZGVmYXVsdGRvbWFpbnxzZWN1cmVwcm9ncmFtbWluZ3xneDo0MTY1MmM0ZDI0ZjQ4ZDMy)**  Článku na webu MSDN publikované v 2006.

shrnout tooquickly, hello přístupu zahrnuje vytvoření diagram, identifikace hrozeb, jejich zmírnění a ověření každého zmírnění dopadů. Zde je diagram, který označuje tento proces:

![Proces SDL](./media/azure-security-threat-modeling-tool/sdlapproach.png)

## <a name="starting-hello-threat-modeling-process"></a>Počáteční proces modelování hrozeb hello

Při spuštění hello nástroj modelování hrozeb, můžete si všimnout pár věcí, jak je vidět na obrázku hello:

![Prázdné úvodní stránky](./media/azure-security-threat-modeling-tool/tmtstart.png)

### <a name="threat-model-section"></a>Část model hrozeb

| Komponenta                                   | Podrobnosti                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Zpětná vazba, návrhy a tlačítko problémy** | Trvá můžete hello  **[fórum MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=sdlprocess)**  pro všechny věci SDL. Nabízí možnost tooread prostřednictvím jiné činnosti uživatelů, spolu s alternativní řešení a doporučení. Pokud stále nemůžete najít co hledáte, e-mailu tmtextsupport@microsoft.com pro naše toohelp tým podpory vám                                                                                                                            |
| **Vytvoření modelu**                          | Otevře prázdného plátna pro jste toodraw diagramu. Ujistěte se, že tooselect šablonu, kterou byste chtěli toouse pro váš model                                                                                                                                                                                                                                                                                                                                                                       |
| **Šablonu pro nové modely**                 | Před vytvořením model je třeba vybrat které toouse šablony. Naše hlavní šablona je hello Azure Threat modelu šablony, která obsahuje vzorníky, hrozby a jejich zmírnění specifické pro Azure. Pro obecné modely vyberte z rozevírací nabídky hello hello SDL TM Knowledge Base. Chcete toocreate vlastní šablonu nebo odesílat novou pro všechny uživatele? Podívejte se na naše  **[šablony úložiště](https://github.com/Microsoft/threat-modeling-templates)**  GitHub stránce toolearn další                              |
| **Otevřete Model**                            | <p>Otevře se okno uložili dřív modely hrozeb. Funkce nedávno otevřít modely Hello je skvělé, pokud potřebujete tooopen nejnovější soubory. Po přesunutí ukazatele myši hello výběr, zjistíte 2 způsoby tooopen modely:</p><p><ul><li>Otevřete z tohoto počítače – classic způsob otevření souboru, který používá místní úložiště</li><li>Otevřete z Onedrivu můžete použít složek v OneDrive toosave a sdílet všechny modely jejich hrozeb v jednom místě toohelp zvýšení produktivity a spolupráce týmů</li></ul></p> |
| **Příručka Začínáme**                   | Otevře se okno hello  **[Microsoft Threat modelování nástroj](./azure-security-threat-modeling-tool.md)**  hlavní stránce                                                                                                                                                                                                                                                                                                                                                                                            |

### <a name="template-section"></a>Část šablony

| Komponenta               | Podrobnosti                                                                                                                                                          |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Vytvořte novou šablonu** | Otevře prázdné šablonu pro toobuild můžete na zařízení. Pokud nemáte rozsáhlé znalosti při vytváření šablony od začátku, doporučujeme toobuild z existující |
| **Otevřít šablonu**       | Otevře existující šablony vám toomake změny příliš                                                                                                             |

Hello nástroj modelování hrozeb tým pracuje neustále tooimprove nástroj funkce a možnosti. Několik malých změn může trvat místní průběhu hello hello rok, ale všechny hlavní změny vyžadují přepisů v příručce hello. Odkazovat tooit často tooensure získat nejnovější oznámení hello.

## <a name="building-a-model"></a>Vytvoření modelu

V této části jsme podle:

- Cristina (Vývojář)
- Richard (program manager) a
- Ashish (tester)

Že jsou projít hello procesu vývoje jejich první model hrozeb.

> Richard: Dobrý den Cristina, I v diagramu modelu threat hello práce na incidentu a chtěli toomake zda My hello informace správné. Můžete mi ji projít pomoci?
> Cristina: absolutně. Podívejme se.
> Richard otevře nástroj hello a sdílí s Cristina jeho obrazovka.

![Model základní hrozeb](./media/azure-security-threat-modeling-tool/basictmt.png)

> Cristina: Ok, vypadá jasné, ale můžete je provede mi ji?
> Richard: zda! Zde je výčet hello:
> - Naše lidského uživatele vykreslením jako entitu mimo – čtverce
> - Budou se odesílání příkazů tooour webový server – hello kruhu.
> - Webový server Hello je konzultace ohledně databáze (dva řádky paralelní)

Co Richard právě vám ukázal, Cristina je diagramu toku dat, zkratka pro  **[Diagram toku dat](https://en.wikipedia.org/wiki/Data_flow_diagram)**. Hello nástroj modelování hrozeb umožňuje uživatelům toospecify důvěryhodnosti hranice, která je indikován čárami tečkovaná hello red, kde jsou různých entit v ovládacím prvku tooshow. Například správci IT vyžadovat systému Active Directory pro účely ověření, takže hello služby Active Directory je mimo jejich řízení.

> Cristina: Vypadá správné toome. Co o hrozbách hello?
> Richard: Chci můžete zobrazit.

## <a name="analyzing-threats"></a>Analýza hrozeb

Po zobrazení analysis hello z hello ikona nabídky výběru (soubor s ikonou lupy), byl přijat tooa seznam generovaného hrozeb hello Threat modelování nástroj najít na základě hello výchozí šablony, používaný přístup SDL hello názvem klikne  **[ STRIDE (falšování identity, úmyslné poškozování, zpřístupnění informací, odepření služby a zvýšení úrovně oprávnění)](https://en.wikipedia.org/wiki/STRIDE_(security))**. Rada Hello je, že software pochází pod předvídatelný sadu hrozeb, které lze nalézt pomocí těchto 6 kategorií.

Tento přístup je jako zabezpečení domu tím zajistí každé dveře a okna Blokovací mechanizmus má zavedené před přidáním poplašný systém nebo následné dotazy po zloděj hello.

![Základní hrozeb](./media/azure-security-threat-modeling-tool/basicthreats.png)

Richard začne výběrem hello první položka v seznamu hello. Stane se toto:

Nejprve je vylepšená hello interakce mezi dvěma vzorníky hello

![Interakce](./media/azure-security-threat-modeling-tool/interaction.png)

Druhý, další informace o ohrožení hello se zobrazí v okně Vlastnosti Threat hello

![Informace o interakci](./media/azure-security-threat-modeling-tool/interactioninfo.png)

Hello generované threat pomůže mu pochopit potenciální chyby v návrhu. Hello STRIDE kategorizaci mu dává představu na potenciální vektory útoku, při hello další popis mu říkají, přesně určit, jak má nesprávný, společně s potenciální toomitigate způsoby, jak ho. Si můžete použít upravitelné pole toowrite poznámky v podrobnostech hello zarovnání do bloku nebo změnit prioritu hodnocení v závislosti na jeho organizace chyb panelu.

Šablony Azure mít další podrobnosti toohelp uživatele, ne jenom co je nesprávný, ale i jak toofix ho přidáním dokumentace specifické pro tooAzure popisy, příklady a hypertextové odkazy.

Popis Hello provedené mu mějte na paměti důležitost hello přidávání uživatelů k ověřování mechanismus tooprevent z se maskování, odhalil hello první threat toobe pracovali. Několik minut do hello diskuse s Cristina, se rozumí hello důležitosti implementace řízení přístupu a rolí. Richard vyplněno některé rychlé poznámky toomake se, že tyto byly implementovány.

Jako Richard přešel do hello hrozeb v části zpřístupnění informací, mohl realizován plán řízení přístupu hello vyžaduje některé účty jen pro čtení pro generování auditování a sestavy. Zadá zajímalo, zda by měl proběhnout nové hrozby, ale hello, které byly jejich zmírnění hello stejné, proto mu hello threat uvedeno odpovídajícím způsobem.
Mu taky představit další o zpřístupnění informací a uvědomili si, že záložní pásky hello ručního tooneed šifrování, úlohy pro hello provozní tým.

Hrozby návrhu toohello není k dispozici z důvodu zabezpečení nebo jejich zmírnění tooexisting zaručuje lze změnit příliš "Není k dispozici" z rozevíracího seznamu Stav hello. Existují tři další možnosti: není spuštěna – výchozí výběr, potřebuje šetření – používá toofollow na položky a Mitigated – po plně pracovali.

## <a name="reports--sharing"></a>Sestavy a sdílení

Jakmile Richard prochází seznam hello s Cristina a přidá důležité poznámky, jejich zmírnění nebo důkazy, prioritu a stav změní, he vybere sestavy -> vytvořit uložit sestavu, která vytiskne dobrý sestavy pro mu toogo přes s -> úplná sestava kolegy tooensure hello správné zabezpečení pracovní je implementováno.

![Informace o interakci](./media/azure-security-threat-modeling-tool/report.png)

Pokud Richard chce tooshare hello souboru místo toho, mohl snadno to provedete lze uložit v účtu OneDrive jeho organizace. Jakmile se provede, který, mu zkopírujte odkaz na dokument hello a sdílet s jeho kolegové. 

## <a name="threat-modeling-meetings"></a>Schůzek modelování hrozeb

Když Richard jeho kolegu toohis model hrozeb pomocí OneDrive, Ashish, hello tester, byl underwhelmed. Mailech vypadalo jako Richard a Cristina provedena mnoha případech důležité rohu, které by mohly být snadno ohrozit. Jeho pochybnostmi je toothreat modely doplňku.

V tomto scénáři po Ashish převzal hello model hrozeb, volal pro dvě schůzky modelování hrozeb: toosynchronize schůzek na hello proces a pomocí hello diagramy a pak druhý schůzku ke kontrole hrozeb a podpisu.

V prvním zasedání hello stráví Ashish proti everyone prostřednictvím threat SDL hello modelování proces 10 minut. Potom vyžádat si diagram modelu hello hrozeb a spuštění vysvětlením podrobně. Do pěti minut byla určena důležité chybí komponenta.

Několik minut později Ashish a Richard tu do rozšířené diskuzi o tom, jak byl sestaven hello webový server. Nebyla hello nejvhodnější způsob pro tooproceed schůzky, ale všichni nakonec souhlas, že již v rané fázi zjišťování nesoulad mezi databází hello probíhalo toosave, které je čas v budoucnosti hello.

V hello druhý schůzky, team hello projít hello hrozeb, popsané některé tooaddress způsoby, jak je a podepsaný vypnout na hello model hrozeb. Jejich zaškrtnutí hello dokumentu do správy zdrojového kódu a pokračovat, s vývoj.

## <a name="thinking-about-assets"></a>Přemýšlení o prostředcích

Některé nástroje pro čtení, kteří mají threat modelován všimnout, že už jsme nebyly mluvili o prostředcích ve všech. Zjistili jsme mnoho softwarových vývojářů lépe než porozumí hello koncept prostředků a jaké prostředky útočníka mohly zajímat pochopení jejich softwaru.

Pokud budete toothreat modelu domu, můžete začít tím přemýšlení o vaší rodiny, nenahraditelných fotek a cenné grafiky. Případně můžete začít tím přemýšlení o kdo může proniknout a hello aktuální zabezpečení systému. Nebo můžete začít tím, že vzhledem k tomu hello fyzické funkcí, jako například hello fondu nebo hello front porch. Jedná se podobá toothinking o prostředky, útočníkům nebo návrh softwaru. Některé z těchto tří přístupů fungovat.

Hello přístup toothreat modelování, které jsme si okomentovat je podstatně jednodušší než co Microsoft provedla v posledních hello. Zjistili jsme, že hello softwaru návrhu přístup funguje dobře u mnoha týmy. Věříme, že, které obsahují vaše.

## <a name="next-steps"></a>Další kroky

Odesílat dotazy, komentáře a riziko z hlediska tootmtextsupport@microsoft.com. **[Stáhněte si](https://aka.ms/tmtpreview)**  spuštění tooget hello nástroj modelování hrozeb.
