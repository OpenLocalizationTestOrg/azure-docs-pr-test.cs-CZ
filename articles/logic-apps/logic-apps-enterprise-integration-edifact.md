---
title: "aaaEDIFACT zpráv B2B enterprise integrace - Azure Logic Apps | Microsoft Docs"
description: "Exchange EDIFACT zprávy ve formátu EDI B2B enterprise integraci s Azure Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 2257d2c8-1929-4390-b22c-f96ca8b291bc
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 07/26/2016
ms.author: LADocs; jonfan
ms.openlocfilehash: 496fadcda58de2d36459202b839b0a63c9e5857c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="exchange-edifact-messages-for-enterprise-integration-with-logic-apps"></a>Zprávy Exchange EDIFACT pro podnikové integrace s logic apps

Před výměnou zpráv EDIFACT pro Azure Logic Apps, musíte vytvořit smlouvy EDIFACT a uložení této smlouvy ve vašem účtu integrace. Tady jsou hello postup, jak toocreate EDIFACT smlouvu.

> [!NOTE]
> Tato stránka popisuje hello EDIFACT funkce pro Azure Logic Apps. Další informace najdete v tématu [X12](logic-apps-enterprise-integration-x12.md).

## <a name="before-you-start"></a>Než začnete

Tady je hello položky, které budete potřebovat:

* [Integrace účet](../logic-apps/logic-apps-enterprise-integration-accounts.md) který již má definovaný a přidružené k předplatnému Azure  
* Alespoň dva [partnery](logic-apps-enterprise-integration-partners.md) , jsou již definováni ve vašem účtu integrace

> [!NOTE]
> Když vytvoříte smlouvy, hello obsah v hello zprávy, které můžete přijímat nebo odesílat tooand z hello partnera musí odpovídat typ smlouvy hello.

Po jste [vytvoření účtu integrace](../logic-apps/logic-apps-enterprise-integration-accounts.md) a [přidat partnery](logic-apps-enterprise-integration-partners.md), EDIFACT smlouvu můžete vytvořit pomocí následujících kroků.

## <a name="create-an-edifact-agreement"></a>Vytvoření smlouvy EDIFACT 

1.  Přihlaste se toohello [portál Azure](http://portal.azure.com "portál Azure"). Hello levé nabídce vyberte **další služby**.

    > [!TIP]
    > Pokud nevidíte **další služby**, může mít tooexpand hello nabídky nejdřív. V horní části hello hello sbalené nabídky, vyberte **nabídky Zobrazit**.

    ![V levé nabídce vyberte "Další služby"](./media/logic-apps-enterprise-integration-edifact/edifact-0.png)

2. Hello vyhledávacího pole zadejte "integrace" filtru. V seznamu výsledků hello vyberte **účty pro integraci**.

    ![Filtrovat podle "integraci", vyberte "Účty pro integraci"](./media/logic-apps-enterprise-integration-edifact/edifact-1-3.png)

3. V hello **účty pro integraci** okno, které se otevře, vyberte hello integrace účet místo toocreate hello smlouvy.
Pokud nevidíte žádné účty pro integraci, [vytvořit první](../logic-apps/logic-apps-enterprise-integration-accounts.md "všechny informace o účtech integrace").  

    ![Vyberte účet integrace kde toocreate hello smlouvy](./media/logic-apps-enterprise-integration-edifact/edifact-1-4.png)

4. Zvolte hello **smlouvy** dlaždici. Pokud nemáte dlaždici smlouvy, přidejte nejprve hello dlaždice.   

    ![Vyberte že dlaždici "Smlouvy"](./media/logic-apps-enterprise-integration-edifact/edifact-1-5.png)

5. V okně hello smlouvy, které se otevře, zvolte **přidat**.

    ![Zvolte "Přidat"](./media/logic-apps-enterprise-integration-edifact/edifact-agreement-2.png)

6. V části **přidat**, zadejte **název** pro vaše smlouvy. Pro **typ smlouvy**, vyberte **EDIFACT**. Vyberte hello **hostitele partnera**, **identitu hostitele**, **hosta partnera**, a **hosta Identity** pro vaše smlouvy.

    ![Zadejte podrobnosti o smlouvě](./media/logic-apps-enterprise-integration-edifact/edifact-1.png)

    | Vlastnost | Popis |
    | --- | --- |
    | Name (Název) |Název smlouvy hello |
    | Typ smlouvy | Musí být EDIFACT |
    | Hostitele partnera |Smlouvu musí hostitelské i hostované partnera. Hello hostitele partnera představuje hello organizace, který konfiguruje hello smlouvy. |
    | Identitu hostitele |Identifikátor pro hostitele partnera hello |
    | Partner hosta |Smlouvu musí hostitelské i hostované partnera. Hello hosta partnera představuje hello organizace, která je spolupráci s hello hostitele partnera. |
    | Identity hosta |Identifikátor pro partnera hosta hello |
    | Získat nastavení |Tyto vlastnosti použít tooall zprávy přijaté službou smlouvu. |
    | Odeslat nastavení |Tyto vlastnosti použít tooall zprávy odeslané smlouvu. |

## <a name="configure-how-your-agreement-handles-received-messages"></a>Nakonfigurujte, jak vaše smlouvy popisovače přijatých zpráv

Teď, když jste nastavili hello smlouvy vlastnosti, můžete nakonfigurovat, jak tato smlouva identifikuje a zpracovává příchozí zprávy přijaté od svého partnera prostřednictvím této smlouvy.

1.  V části **přidat**, vyberte **přijímat nastavení**.
Konfigurujte tyto vlastnosti závislosti na vaší smlouvě se hello partnera, který výměny zpráv s vámi. Vlastnost popis najdete v tématu hello tabulky v této části.

    **Získat nastavení** jsou uspořádány do těchto oddílů: identifikátory, potvrzení, schémata, řízení čísla, ověřování a interní nastavení.

    ![Konfigurace "Obdrží nastavení"](./media/logic-apps-enterprise-integration-edifact/edifact-2.png)  

2. Jakmile jste hotovi, ujistěte se, že toosave nastavení tak, že zvolíte **OK**.

Nyní je vaše smlouvy připravené toohandle příchozí zprávy, které odpovídají tooyour vybraná nastavení.

### <a name="identifiers"></a>Identifikátory

| Vlastnost | Popis |
| --- | --- |
| UNB6.1 (heslo příjemce odkaz) |Zadejte hodnotu mezi 1 a 14 znaky alfanumerické hodnotu. |
| UNB6.2 (kvalifikátor příjemce odkaz) |Zadejte hodnotu alfanumerické znaky s nejméně jeden znak a maximálně dva znaky. |

### <a name="acknowledgments"></a>Potvrzování

| Vlastnost | Popis |
| --- | --- |
| Přijetí zprávy (CONTRL) |Zaškrtněte toto políčko tooreturn technické toohello výměnu odesílatele potvrzení (CONTRL). potvrzení Hello se odesílají odesílatele výměnu toohello podle hello nastavení pro odesílání pro smlouvu hello. |
| Potvrzení (CONTRL) |Zaškrtněte toto políčko tooreturn funkční (CONTRL) potvrzení toohello výměnu odesílatele hello potvrzení je odeslána odesílatele výměnu toohello podle hello nastavení pro odesílání pro smlouvu hello. |

### <a name="schemas"></a>Schémata

| Vlastnost | Popis |
| --- | --- |
| UNH2.1 (TYP) |Vyberte typ sadu transakce. |
| UNH2.2 (VERZE) |Zadejte číslo verze zprávy hello. (Minimální, jeden znak, maximum, tři znaky). |
| UNH2.3 (VERZE) |Zadejte číslo verze zprávy hello. (Minimální, jeden znak, maximum, tři znaky). |
| UNH2.5 (PŘIDRUŽENÉ PŘIŘAZENÉ KÓD) |Zadejte kód hello přiřazen. (Maximální šest znaků. Musí být alfanumerické znaky). |
| UNG2.1 (ID ODESÍLATELE APLIKACE) |Zadejte hodnotu alfanumerické znaky s nejméně jeden znak a maximálně 35 znaků. |
| UNG2.2 (KVALIFIKÁTOR APLIKACE ODESÍLATELE KÓDU) |Zadejte hodnotu alfanumerické delší než 4 znaky. |
| SCHÉMA |Vyberte hello předtím nahrála schématu chcete toouse z vašeho účtu přidružené integrace. |

### <a name="control-numbers"></a>Ovládací prvek čísla
| Vlastnost | Popis |
| --- | --- |
| Zakáže duplikáty Interchange číslo ovládacího prvku |tooblock duplicitní mimoúrovňové křižovatky, vyberte tuto vlastnost. Pokud vybraná, hello EDIFACT dekódovat akce ověří, že hello výměnu ovládací prvek číslo (UNB5) pro výměnu hello přijatých neodpovídá číslo řízení dříve zpracované výměnu. Pokud je zjištěna shoda, nebude zpracován hello výměnu. |
| Kontrolovat duplicitní UNB5 každých (dny) |Pokud jste zvolili toodisallow duplicitní výměnu řízení čísla, můžete zadat hello počet dní, když tooperform hello zkontrolujte tím, že hello odpovídající hodnotu pro toto nastavení. |
| Zakázat duplicity kontrolních čísel skupiny |tooblock interchanges s duplicitní skupině řízení čísla (UNG5), vyberte tuto vlastnost. |
| Zakázat duplicity kontrolních čísel sad transakcí |tooblock interchanges se duplicitní transakce sadu řízení čísla (UNH1), vyberte tuto vlastnost. |
| Číslo EDIFACT potvrzení Ovládací prvek |transakce hello toodesignate nastaven čísla referenční dokumentace pro použití v potvrzení, zadejte hodnotu pro předponu hello, rozsah čísel odkaz a příponu. |

### <a name="validations"></a>Ověření

Po dokončení každý řádek ověření jiné automaticky přidá. Pokud nezadáte všechna pravidla, ověření používá řádek "Výchozí" hello.

| Vlastnost | Popis |
| --- | --- |
| Typ zprávy |Vyberte typ zprávy EDI hello. |
| Ověření EDI |Proveďte ověření EDI na typy dat definované schéma hello EDI vlastnosti, omezení délky, prázdný datové prvky a koncové oddělovače. |
| Rozšířené ověření |Pokud není hello datový typ EDI, ověření je na hello data element požadavku a povoleny opakování, výčty a data element délka ověření (min/max). |
| Povolit úvodní nebo koncové nuly |Zachování všechny další počáteční nebo koncové nula a místo znaků. Nevysunujte tyto znaky. |
| Trim – úvodní nebo koncové nuly |Odeberte úvodní a koncové nuly a místo znaků. |
| Koncové oddělovače zásad |Generovat koncové oddělovače. <p>Vyberte **není povoleno** tooprohibit koncové oddělovače a oddělovače v hello přijatých výměnu. Pokud hello výměnu koncové oddělovače a oddělovačů, výměnu hello je deklarovaná není platný. <p>Vyberte **volitelné** tooaccept interchanges s nebo bez koncové oddělovače a oddělovačů. <p>Vyberte **povinné** při hello přijatých výměnu musí mít koncové oddělovače a oddělovačů. |

### <a name="internal-settings"></a>Vnitřní nastavení

| Vlastnost | Popis |
| --- | --- |
| Pokud jsou povolené koncové oddělovače, vytvořit prázdné značky XML |Vyberte toto políčko toohave hello výměnu odesílatele zahrnout prázdný značky XML pro koncové oddělovače. |
| Rozdělit výměnu jako sady transakcí – pozastavit sady transakcí při chybě|Analyzuje každou transakci, nastavte v výměnu do samostatného dokumentu XML s použitím hello obálek toohello transakce sady. Pozastavte pouze transakce sady hello, které nesplní ověření. |
| Rozdělit výměnu jako sady transakcí – pozastavit výměnu při chybě|Analyzuje každou transakci, nastavte v výměnu do samostatného dokumentu XML s použitím příslušné obálky hello. Pozastavte celý výměnu hello, pokud selže ověření se jednu nebo více sad transakce v hello výměnu. | 
| Zachovat výměnu – pozastavení sady transakce při chybě |Výměnu hello zůstanou zachovány, vytvoří dokument XML pro celý dávkové výměnu hello. Pozastavit pouze transakce sady hello, které nesplní ověření, přitom dál tooprocess nastaví všechny ostatní transakce. |
| Zachovat výměnu – pozastavit výměnu při chybě |Výměnu hello zůstanou zachovány, vytvoří dokument XML pro celý dávkové výměnu hello. Pozastavte celý výměnu hello, pokud selže ověření se jednu nebo více sad transakce v hello výměnu. |

## <a name="configure-how-your-agreement-sends-messages"></a>Nakonfigurujte, jak vaše smlouvy odešle zprávy

Můžete nakonfigurovat, jak tato smlouva identifikuje a zpracovává odchozích zpráv, které odesílají tooyour partnery prostřednictvím této smlouvy.

1.  V části **přidat**, vyberte **odeslat nastavení**.
Konfigurujte tyto vlastnosti závislosti na vaší smlouvě se svého partnera, který výměny zpráv s vámi. Vlastnost popis najdete v tématu hello tabulky v této části.

    **Odeslat nastavení** jsou uspořádány do těchto oddílů: identifikátory, potvrzení, schémata, obálky, znakové sady a oddělovačů, řízení čísla a ověření.

    ![Konfigurace "Odeslat nastavení"](./media/logic-apps-enterprise-integration-edifact/edifact-3.png)    

2. Jakmile jste hotovi, ujistěte se, že toosave nastavení tak, že zvolíte **OK**.

Vaše smlouvy je nyní připraven toohandle odchozí zprávy, které odpovídají tooyour vybraná nastavení.

### <a name="identifiers"></a>Identifikátory

| Vlastnost | Popis |
| --- | --- |
| UNB1.2 (syntaxe verze) |Vyberte hodnotu mezi **1** a **4**. |
| UNB2.3 (Adresa zpětného směrování odesílatele) |Zadejte hodnotu alfanumerické znaky s nejméně jeden znak a maximálně 14 znaků. |
| UNB3.3 (Adresa zpětného směrování příjemce) |Zadejte hodnotu alfanumerické znaky s nejméně jeden znak a maximálně 14 znaků. |
| UNB6.1 (heslo příjemce odkaz) |Zadejte hodnotu alfanumerické s minimálně jeden a maximálně 14 znaků. |
| UNB6.2 (kvalifikátor příjemce odkaz) |Zadejte hodnotu alfanumerické znaky s nejméně jeden znak a maximálně dva znaky. |
| UNB7 (referenční dokumentace ID aplikace) |Zadejte hodnotu alfanumerické znaky s nejméně jeden znak a maximálně 14 znaků |

### <a name="acknowledgment"></a>Potvrzení
| Vlastnost | Popis |
| --- | --- |
| Přijetí zprávy (CONTRL) |Toto políčko zaškrtněte, pokud hello hostované partnera očekává tooreceive technické potvrzení (CONTRL). Toto nastavení určuje hello hostované partnera, který odesílá uvítací zprávu, požádá o potvrzení z hello hosta partnera. |
| Potvrzení (CONTRL) |Toto políčko zaškrtněte, pokud hello hostované partnera očekává tooreceive funkční potvrzení (CONTRL). Toto nastavení určuje hello hostované partnera, který odesílá uvítací zprávu, požádá o potvrzení z hello hosta partnera. |
| Vygenerovat smyčku SG1/SG4 pro přijaté sady transakcí |Pokud jste zvolili toorequest funkční potvrzení, vyberte toto zaškrtávací políčko tooforce generování sz1/BS4 smyčky v funkční potvrzování CONTRL pro sady přijatý transakce. |

### <a name="schemas"></a>Schémata
| Vlastnost | Popis |
| --- | --- |
| UNH2.1 (TYP) |Vyberte typ sadu transakce. |
| UNH2.2 (VERZE) |Zadejte číslo verze zprávy hello. |
| UNH2.3 (VERZE) |Zadejte číslo verze zprávy hello. |
| SCHÉMA |Vyberte toouse schématu hello. Schémata jsou umístěny ve vašem účtu integrace. tooaccess schémat, nejprve odkaz svou aplikaci logiky tooyour integrace účtu. |

### <a name="envelopes"></a>Obálky
| Vlastnost | Popis |
| --- | --- |
| UNB8 (zpracování Priority kódu) |Zadejte abecední hodnotu, která není více než jeden znak. |
| UNB10 (smlouva komunikaci) |Zadejte hodnotu alfanumerické znaky s nejméně jeden znak a maximálně 40 znaků. |
| UNB11 (Test ukazatele) |Vyberte, které je toto políčko tooindicate, který hello výměnu generované testovacích dat |
| Použít segment UNA (Nápověda k řetězci služby) |Zaškrtněte toto políčko toogenerate UNA segmentu pro toobe výměnu hello odeslána. |
| Použít segmenty UNG (Hlavička skupiny funkce) |Zaškrtněte toto políčko toocreate seskupení segmentů v záhlaví skupiny funkčnosti hello partnera hosta toohello zprávy odeslané hello. Hello následující hodnoty jsou použité toocreate hello UNG segmenty: <p>Pro **UNG1**, zadejte hodnotu alfanumerické znaky s nejméně jeden znak a maximálně šest znaků. <p>Pro **UNG2.1**, zadejte hodnotu alfanumerické znaky s nejméně jeden znak a maximálně 35 znaků. <p>Pro **UNG2.2**, zadejte hodnotu alfanumerické delší než 4 znaky. <p>Pro **UNG3.1**, zadejte hodnotu alfanumerické znaky s nejméně jeden znak a maximálně 35 znaků. <p>Pro **UNG3.2**, zadejte hodnotu alfanumerické delší než 4 znaky. <p>Pro **UNG6**, zadejte hodnotu alfanumerické s minimálně jeden a maximálně tři znaky. <p>Pro **UNG7.1**, zadejte hodnotu alfanumerické znaky s nejméně jeden znak a maximálně tři znaky. <p>Pro **UNG7.2**, zadejte hodnotu alfanumerické znaky s nejméně jeden znak a maximálně tři znaky. <p>Pro **UNG7.3**, zadejte hodnotu alfanumerické minimálně 1 znak a maximálně 6 znaků. <p>Pro **UNG8**, zadejte hodnotu alfanumerické znaky s nejméně jeden znak a maximálně 14 znaků. |

### <a name="character-sets-and-separators"></a>Znakové sady a oddělovače

Kromě hello znaková sada, můžete zadat jinou sadu oddělovače toobe použít u každého typu zprávy. Pokud znakovou sadu není určena pro danou zprávou schéma, se používá hello výchozí znakovou sadu.

| Vlastnost | Popis |
| --- | --- |
| UNB1.1 (System Identifier) |Vyberte hello EDIFACT znak nastavit toobe na hello odchozí výměnu použita. |
| Schéma |Vyberte z rozevíracího seznamu hello schéma. Po dokončení každý řádek je automaticky přidán nový řádek. Pro vybrané schéma hello vyberte hello oddělovačů nastavit, které chcete toouse, podle hello následující popisy oddělovače. |
| Typ vstupu |Vyberte z rozevíracího seznamu hello typem vstupu. |
| Součást oddělovače |složené datové prvky tooseparate, zadejte jeden znak. |
| Oddělovač elementu dat |tooseparate jednoduché datové elementů v rámci složené datové prvky, zadejte jeden znak. |
| Segment ukončovací znak |tooindicate hello konec EDI segment zadejte jeden znak. |
| Přípona |Vyberte hello znak, který se používá s identifikátorem hello segmentu. Pokud jste určit příponu pak hello segment ukončovací datový prvek nesmí být prázdné. Pokud segment ukončovací hello je prázdné, je třeba určit příponu. |

### <a name="control-numbers"></a>Ovládací prvek čísla
| Vlastnost | Popis |
| --- | --- |
| UNB5 (Interchange řízení číslo) |Zadejte předponu, rozsah hodnot pro hello výměnu řízení číslo a příponu. Tyto hodnoty jsou použité toogenerate odchozí výměnu. Předpona Hello a příponu jsou volitelné, když je třeba zadat číslo řízení hello. ovládací prvek Hello je zvýšena pro každou novou zprávu; Předpona Hello a příponu zůstanou hello stejné. |
| UNG5 (skupina řízení číslo) |Zadejte předponu, rozsah hodnot pro hello výměnu řízení číslo a příponu. Tyto hodnoty jsou použity číslo toogenerate hello skupiny ovládacího prvku. Předpona Hello a příponu jsou volitelné, když je třeba zadat číslo řízení hello. Hello ovládací prvek je zvýšena pro každé nové zprávy, dokud nebude dosaženo maximální hodnoty hello; Předpona Hello a příponu zůstanou hello stejné. |
| UNH1 (zpráva hlavičky referenční číslo) |Zadejte předponu, rozsah hodnot pro hello výměnu řízení číslo a příponu. Tyto hodnoty jsou použity toogenerate hello zpráva hlavičky referenční číslo. Hello předponu a příponu jsou volitelné, zatímco hello referenční číslo se vyžaduje. Hello referenční číslo se zvýší, pro každou novou zprávu; Předpona Hello a příponu zůstanou hello stejné. |

### <a name="validations"></a>Ověření

Po dokončení každý řádek ověření jiné automaticky přidá. Pokud nezadáte všechna pravidla, ověření používá řádek "Výchozí" hello.

| Vlastnost | Popis |
| --- | --- |
| Typ zprávy |Vyberte typ zprávy EDI hello. |
| Ověření EDI |Proveďte ověření EDI na datových typů, jak definované vlastnosti EDI hello hello schématu, omezení délky, prázdný datové prvky a koncové oddělovače. |
| Rozšířené ověření |Pokud není hello datový typ EDI, ověření je na hello data element požadavku a povoleny opakování, výčty a data element délka ověření (min/max). |
| Povolit úvodní nebo koncové nuly |Zachování všechny další počáteční nebo koncové nula a místo znaků. Nevysunujte tyto znaky. |
| Trim – úvodní nebo koncové nuly |Odeberte počáteční nebo koncové nulový počet znaků. |
| Koncové oddělovače zásad |Generovat koncové oddělovače. <p>Vyberte **není povoleno** výměnu odeslat koncové oddělovače tooprohibit a oddělovače v hello. Pokud hello výměnu koncové oddělovače a oddělovačů, výměnu hello je deklarovaná není platný. <p>Vyberte **volitelné** toosend interchanges s nebo bez koncové oddělovače a oddělovačů. <p>Vyberte **povinné** Pokud hello odeslané výměnu musí mít koncové oddělovače a oddělovačů. |

## <a name="find-your-created-agreement"></a>Najít vaší vytvořené smlouvy

1.  Po dokončení nastavení všechny vaše smlouvy vlastnosti na hello **přidat** okně zvolte **OK** toofinish vytváření smlouvy a okně návratový tooyour integrace účtu.

    Nově přidané smlouvy nyní se zobrazí v vaše **smlouvy** seznamu.

2.  Můžete také zobrazit vaše smlouvy v váš účet Přehled integrace. V okně účtu vaší integrace, zvolte **přehled**, pak vyberte hello **smlouvy** dlaždici. 

    ![Vyberte dlaždici tooview "Smlouvy" všechny smlouvy](./media/logic-apps-enterprise-integration-edifact/edifact-4.png)   

## <a name="view-swagger-file"></a>Soubor Swagger zobrazení
Podrobnosti Swagger hello tooview hello EDIFACT konektor, najdete v tématu [EDIFACT](/connectors/edifact/).

## <a name="learn-more"></a>Další informace
* [Další informace o hello Enterprise integračního balíčku](logic-apps-enterprise-integration-overview.md "Další informace o Enterprise integračního balíčku")  

