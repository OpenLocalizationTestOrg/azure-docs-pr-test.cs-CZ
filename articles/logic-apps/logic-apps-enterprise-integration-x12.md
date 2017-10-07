---
title: "aaaX12 zpráv B2B enterprise integrace - Azure Logic Apps | Microsoft Docs"
description: "Exchange X12 zprávy ve formátu EDI B2B enterprise integraci s Azure Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 7422d2d5-b1c7-4a11-8c9b-0d8cfa463164
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 20a077b299875a16ada66a500d5f1c8f9972d309
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="exchange-x12-messages-for-enterprise-integration-with-logic-apps"></a>Exchange X12 zprávy pro podnikové integrace s logic apps

Před výměnou X12 zprávy pro Azure Logic Apps, musíte vytvořit X12 smlouvy a uložit v účtu integrace této smlouvy. Tady jsou hello postup, jak toocreate na X12 smlouvy.

> [!NOTE]
> Tato stránka popisuje hello X12 funkce pro Azure Logic Apps. Další informace najdete v tématu [EDIFACT](logic-apps-enterprise-integration-edifact.md).

## <a name="before-you-start"></a>Než začnete

Tady je hello položky, které budete potřebovat:

* [Integrace účet](../logic-apps/logic-apps-enterprise-integration-accounts.md) který již má definovaný a přidružené k předplatnému Azure
* Alespoň dva [partnery](../logic-apps/logic-apps-enterprise-integration-partners.md) které jsou definované ve vašem účtu integrace a nakonfigurovaný s identifikátorem hello X12 pod **obchodní identit**    
* Požadovanou [schématu](../logic-apps/logic-apps-enterprise-integration-schemas.md) pro nahrávání tooyour [integrace účtu](../logic-apps/logic-apps-enterprise-integration-accounts.md)

Po jste [vytvoření účtu integrace](../logic-apps/logic-apps-enterprise-integration-accounts.md), [přidat partnery](logic-apps-enterprise-integration-partners.md)a mít [schématu](../logic-apps/logic-apps-enterprise-integration-schemas.md) , při němž toouse, můžete vytvořit X12 smlouvy pomocí následujících kroků.

## <a name="create-an-x12-agreement"></a>Vytvoření X12 smlouvy

1.  Přihlaste se toohello [portál Azure](http://portal.azure.com "portál Azure"). Hello levé nabídce vyberte **další služby**. 

    > [!TIP]
    > Pokud nevidíte **další služby**, může mít tooexpand hello nabídky nejdřív. V horní části hello hello sbalené nabídky, vyberte **nabídky Zobrazit**.

    ![V levé nabídce vyberte "Další služby"](./media/logic-apps-enterprise-integration-x12/account-1.png)

2.  Hello vyhledávacího pole zadejte "integrace" jako filtr. V seznamu výsledků hello vyberte **účty pro integraci**.  

    ![Filtrovat podle "integraci", vyberte "Účty pro integraci"](./media/logic-apps-enterprise-integration-x12/account-2.png)

3. V hello **účty pro integraci** okno, které se otevře, vyberte hello integrace účet místo tooadd hello smlouvy.
Pokud nevidíte žádné účty pro integraci, [vytvořit první](../logic-apps/logic-apps-enterprise-integration-accounts.md "všechny informace o účtech integrace").

    ![Vyberte účet integrace kde toocreate hello smlouvy](./media/logic-apps-enterprise-integration-x12/account-3.png)

4. Vyberte **přehled**, pak vyberte hello **smlouvy** dlaždici. Pokud nemáte dlaždici smlouvy, přidejte nejprve hello dlaždice. 

    ![Vyberte že dlaždici "Smlouvy"](./media/logic-apps-enterprise-integration-agreements/agreement-1.png)

5. V okně hello smlouvy, které se otevře, zvolte **přidat**.

    ![Zvolte "Přidat"](./media/logic-apps-enterprise-integration-agreements/agreement-2.png)     

6. V části **přidat**, zadejte **název** pro vaše smlouvy. Hello typ smlouvy, vyberte **X12**. Vyberte hello **hostitele partnera**, **identitu hostitele**, **hosta partnera**, a **hosta Identity** pro vaše smlouvy. Vlastnost podrobnosti naleznete v tématu hello tabulky v tomto kroku.

    ![Zadejte podrobnosti o smlouvě](./media/logic-apps-enterprise-integration-x12/x12-1.png)  

    | Vlastnost | Popis |
    | --- | --- |
    | Name (Název) |Název smlouvy hello |
    | Typ smlouvy | Musí být X12 |
    | Hostitele partnera |Smlouvu musí hostitelské i hostované partnera. Hello hostitele partnera představuje hello organizace, který konfiguruje hello smlouvy. |
    | Identitu hostitele |Identifikátor pro hostitele partnera hello |
    | Partner hosta |Smlouvu musí hostitelské i hostované partnera. Hello hosta partnera představuje hello organizace, která je spolupráci s hello hostitele partnera. |
    | Identity hosta |Identifikátor pro partnera hosta hello |
    | Získat nastavení |Tyto vlastnosti použít tooall zprávy přijaté službou smlouvu. |
    | Odeslat nastavení |Tyto vlastnosti použít tooall zprávy odeslané smlouvu. |  

  > [!NOTE]
  > Rozlišení X12 smlouvy závisí na odpovídající kvalifikátor hello odesílatele a identifikátor a hello příjemce kvalifikátor a identifikátor definovaný v hello partnera a příchozí zprávy. Pokud tyto hodnoty změnit pro svého partnera, aktualizujte příliš hello smlouvy.

## <a name="configure-how-your-agreement-handles-received-messages"></a>Nakonfigurujte, jak vaše smlouvy popisovače přijatých zpráv

Teď, když jste nastavili hello smlouvy vlastnosti, můžete nakonfigurovat, jak tato smlouva identifikuje a zpracovává příchozí zprávy přijaté od svého partnera prostřednictvím této smlouvy.

1.  V části **přidat**, vyberte **přijímat nastavení**.
Konfigurujte tyto vlastnosti závislosti na vaší smlouvě se hello partnera, který výměny zpráv s vámi. Vlastnost popis najdete v tématu hello tabulky v této části.

    **Získat nastavení** jsou uspořádány do těchto oddílů: identifikátory, potvrzení, schémata, obálky, řízení čísla, ověření a interní nastavení.

2. Jakmile jste hotovi, ujistěte se, že toosave nastavení tak, že zvolíte **OK**.

Nyní je vaše smlouvy připravené toohandle příchozí zprávy, které odpovídají tooyour vybraná nastavení.

### <a name="identifiers"></a>Identifikátory

![Nastavit identifikátor vlastnosti](./media/logic-apps-enterprise-integration-x12/x12-2.png)  

| Vlastnost | Popis |
| --- | --- |
| ISA1 (autorizace kvalifikátor) |Hello rozevíracím seznamu vyberte hodnoty kvalifikátor hello autorizace. |
| ISA2 |Volitelné. Zadejte hodnotu informace autorizace. Pokud je hodnota hello, které jste zadali pro ISA1 než 00, zadejte minimálně jeden alfanumerický znak a maximálně 10. |
| ISA3 (kvalifikátor zabezpečení) |Hello rozevíracím seznamu vyberte hodnotu kvalifikátor hello zabezpečení. |
| ISA4 |Volitelné. Zadejte hodnotu informace zabezpečení hello. Pokud je hodnota hello, které jste zadali pro ISA3 než 00, zadejte minimálně jeden alfanumerický znak a maximálně 10. |

### <a name="acknowledgment"></a>Potvrzení

![Nastavit vlastnosti potvrzení](./media/logic-apps-enterprise-integration-x12/x12-3.png) 

| Vlastnost | Popis |
| --- | --- |
| TA1 očekávání |Vrátí odesílatele výměnu toohello technické potvrzení |
| FA očekávání |Vrátí funkční potvrzení toohello výměnu odesílatele. Potom vyberte, zda chcete hello 997 nebo 999 potvrzování, podle verze schématu hello |
| Zahrnout AK2/IK2 smyčky |Umožňuje generování AK2 smyčky v funkční potvrzování pro sady přijatý transakce |

### <a name="schemas"></a>Schémata

Vyberte schéma pro každý typ transakce (ST1) a aplikace Sender (GS2). přijímat Hello kanálu provede zpětný překlad hello příchozí zprávy lze porovnávat hello ST1 a GS2 hello příchozí zprávy s hello hodnoty sady sem a schématu hello hello příchozí zprávy s hello schématu tady nastavíte.

![Vyberte možnost schématu](./media/logic-apps-enterprise-integration-x12/x12-33.png) 

| Vlastnost | Popis |
| --- | --- |
| Verze |Vyberte verzi hello X12 |
| Typ transakce (ST01) |Vyberte typ transakce hello |
| Aplikace Sender (GS02) |Vyberte aplikace sender hello |
| Schéma |Vyberte soubor schématu hello chcete toouse. Schémata jsou přidány tooyour integrace účtu. |

> [!NOTE]
> Nakonfigurujte požadované hello [schématu](../logic-apps/logic-apps-enterprise-integration-schemas.md) který je nahraný tooyour [integrace účet](../logic-apps/logic-apps-enterprise-integration-accounts.md).

### <a name="envelopes"></a>Obálky

![Zadejte hello oddělovače v sadě transakce: Zvolte Standardní identifikátor nebo opakování oddělovače](./media/logic-apps-enterprise-integration-x12/x12-34.png)

| Vlastnost | Popis |
| --- | --- |
| ISA11 využití |Určuje toouse hello oddělovače v sadě transakce: <p>Vyberte **standardní identifikátor** toouse a tečka (.) pro desítkovém zápisu, nikoli hello zápisem hello příchozí dokumentu v hello EDI přijímat kanálu. <p>Vyberte **opakování oddělovače** toospecify hello oddělovač pro opakované výskyty jednoduché datového elementu nebo opakovaných datová struktura. Například obvykle hello karátová (^) se používá jako oddělovač opakování hello. U HIPAA schémat můžete použít pouze karátová hello. |

### <a name="control-numbers"></a>Ovládací prvek čísla

![Vyberte, jak řídit toohandle číslo duplicitní položky](./media/logic-apps-enterprise-integration-x12/x12-35.png) 

| Vlastnost | Popis |
| --- | --- |
| Zakáže duplikáty Interchange číslo ovládacího prvku |Blokovat duplicitní mimoúrovňové křižovatky. Zkontroluje hello výměnu řízení číslo (ISA13) pro hello přijatých výměnu řízení číslo. Pokud je zjištěna shoda, zobrazí hello kanálu není zpracovat výměnu hello. Můžete zadat hello počet dní pro provedení kontroly hello tím, že hodnota *kontrolovat duplicitní ISA13 každých (dny)*. |
| Zakázat duplicity kontrolních čísel skupiny |Blok interchanges s duplicitní skupině řízení čísla. |
| Zakázat duplicity kontrolních čísel sad transakcí |Blok interchanges s čísly verzí sady se duplicitní transakce. |

### <a name="validations"></a>Ověření

![Nastavit vlastnosti ověření pro přijatých zpráv](./media/logic-apps-enterprise-integration-x12/x12-36.png) 

Po dokončení každý řádek ověření jiné automaticky přidá. Pokud nezadáte všechna pravidla, ověření používá řádek "Výchozí" hello.

| Vlastnost | Popis |
| --- | --- |
| Typ zprávy |Vyberte typ zprávy EDI hello. |
| Ověření EDI |Proveďte ověření EDI na typy dat definované schéma hello EDI vlastnosti, omezení délky, prázdný datové prvky a koncové oddělovače. |
| Rozšířené ověření |Pokud není hello datový typ EDI, ověření je na hello data element požadavku a povoleny opakování, výčty a data element délka ověření (min/max). |
| Povolit úvodní nebo koncové nuly |Zachování všechny další počáteční nebo koncové nula a místo znaků. Nevysunujte tyto znaky. |
| Trim – úvodní nebo koncové nuly |Odeberte úvodní a koncové nuly a místo znaků. |
| Koncové oddělovače zásad |Generovat koncové oddělovače. <p>Vyberte **není povoleno** tooprohibit koncové oddělovače a oddělovače v hello přijatých výměnu. Pokud hello výměnu koncové oddělovače a oddělovačů, výměnu hello je deklarovaná není platný. <p>Vyberte **volitelné** tooaccept interchanges s nebo bez koncové oddělovače a oddělovačů. <p>Vyberte **povinné** při hello výměnu musí mít koncové oddělovače a oddělovačů. |

### <a name="internal-settings"></a>Vnitřní nastavení

![Vyberte interní nastavení](./media/logic-apps-enterprise-integration-x12/x12-37.png) 

| Vlastnost | Popis |
| --- | --- |
| Převést předpokládané formátu desetinného čísla "Nn" tooa základní 10 číselná hodnota |Převede představuje počet EDI, která je zadána ve formátu hello "Nn" do základu 10 číselná hodnota |
| Pokud jsou povolené koncové oddělovače, vytvořit prázdné značky XML |Vyberte toto políčko toohave hello výměnu odesílatele zahrnout prázdný značky XML pro koncové oddělovače. |
| Rozdělit výměnu jako sady transakcí – pozastavit sady transakcí při chybě|Analyzuje každou transakci, nastavte v výměnu do samostatného dokumentu XML s použitím hello obálek toohello transakce sady. Pozastaví pouze hello transakce, kde hello se ověřování nezdaří. |
| Rozdělit výměnu jako sady transakcí – pozastavit výměnu při chybě|Analyzuje každou transakci, nastavte v výměnu do samostatného dokumentu XML s použitím příslušné obálky hello. Celý výměnu pozastaví, pokud selže ověření se jednu nebo více sad transakce v hello výměnu. | 
| Zachovat výměnu – pozastavení sady transakce při chybě |Výměnu hello zůstanou zachovány, vytvoří dokument XML pro celý dávkové výměnu hello. Pozastaví pouze transakce sady hello, které nesplní ověření přitom dál tooprocess všechny ostatní sady transakce. |
| Zachovat výměnu – pozastavit výměnu při chybě |Výměnu hello zůstanou zachovány, vytvoří dokument XML pro celý dávkové výměnu hello. Pozastaví výměnu celý text hello, pokud selže ověření se jednu nebo více sad transakce v hello výměnu. |

## <a name="configure-how-your-agreement-sends-messages"></a>Nakonfigurujte, jak vaše smlouvy odešle zprávy

Můžete nakonfigurovat, jak tato smlouva identifikuje a zpracovává odchozích zpráv, které odešlete tooyour partnera prostřednictvím této smlouvy.

1.  V části **přidat**, vyberte **odeslat nastavení**.
Konfigurujte tyto vlastnosti závislosti na vaší smlouvě se svého partnera, který výměny zpráv s vámi. Vlastnost popis najdete v tématu hello tabulky v této části.

    **Odeslat nastavení** jsou uspořádány do těchto oddílů: identifikátory, potvrzení, schémata, obálky, znakové sady a oddělovačů, řízení čísla a ověření.

2. Jakmile jste hotovi, ujistěte se, že toosave nastavení tak, že zvolíte **OK**.

Vaše smlouvy je nyní připraven toohandle odchozí zprávy, které odpovídají tooyour vybraná nastavení.

### <a name="identifiers"></a>Identifikátory

![Nastavit identifikátor vlastnosti](./media/logic-apps-enterprise-integration-x12/x12-4.png)  

| Vlastnost | Popis |
| --- | --- |
| Kvalifikátor autorizace (ISA1) |Hello rozevíracím seznamu vyberte hodnoty kvalifikátor hello autorizace. |
| ISA2 |Zadejte hodnotu informace autorizace. Pokud tato hodnota je než 00, zadejte minimálně jeden alfanumerický znak a maximálně 10. |
| Kvalifikátor zabezpečení (ISA3) |Hello rozevíracím seznamu vyberte hodnotu kvalifikátor hello zabezpečení. |
| ISA4 |Zadejte hodnotu informace zabezpečení hello. Pokud je tato hodnota než 00 pro hello hodnotu (ISA4) textového pole zadejte minimálně jednu hodnotu alfanumerické znaky a maximálně 10. |

### <a name="acknowledgment"></a>Potvrzení

![Nastavit vlastnosti potvrzení](./media/logic-apps-enterprise-integration-x12/x12-5.png)  

| Vlastnost | Popis |
| --- | --- |
| TA1 očekávání |Vrátí odesílatele výměnu toohello technické potvrzení (TA1). Toto nastavení určuje, že partnera hello hostitele, který je odesílání hello zpráv požadavků na potvrzení od partnera hosta hello hello smlouvy. Tato potvrzení by se měl partner hello hostitele na základě nastavení přijímat hello hello smlouvy. |
| FA očekávání |Vrátí odesílatele výměnu toohello funkční potvrzení (IM). Vyberte, zda chcete hello 997 nebo 999 potvrzení, podle verze schématu hello, které pracujete. Tato potvrzení by se měl partner hello hostitele na základě nastavení přijímat hello hello smlouvy. |
| Verze DM |Vyberte verzi hello DM |

### <a name="schemas"></a>Schémata

![Vyberte toouse schématu](./media/logic-apps-enterprise-integration-x12/x12-5.png)  

| Vlastnost | Popis |
| --- | --- |
| Verze |Vyberte verzi hello X12 |
| Typ transakce (ST01) |Vyberte typ transakce hello |
| SCHÉMA |Vyberte toouse schématu hello. Schémata jsou umístěny ve vašem účtu integrace. Pokud vyberete schématu nejprve, automaticky konfiguruje verze a transakce typu  |

> [!NOTE]
> Nakonfigurujte požadované hello [schématu](../logic-apps/logic-apps-enterprise-integration-schemas.md) který je nahraný tooyour [integrace účet](../logic-apps/logic-apps-enterprise-integration-accounts.md).

### <a name="envelopes"></a>Obálky

![Zadejte hello oddělovače v sadě transakce: Zvolte Standardní identifikátor nebo opakování oddělovače](./media/logic-apps-enterprise-integration-x12/x12-6.png) 

| Vlastnost | Popis |
| --- | --- |
| ISA11 využití |Určuje toouse hello oddělovače v sadě transakce: <p>Vyberte **standardní identifikátor** toouse a tečka (.) pro desítkovém zápisu, nikoli hello zápisem hello příchozí dokumentu v hello EDI přijímat kanálu. <p>Vyberte **opakování oddělovače** toospecify hello oddělovač pro opakované výskyty jednoduché datového elementu nebo opakovaných datová struktura. Například obvykle hello karátová (^) se používá jako oddělovač opakování hello. U HIPAA schémat můžete použít pouze karátová hello. |

### <a name="control-numbers"></a>Ovládací prvek čísla

![Zadejte číslo řízení vlastnosti](./media/logic-apps-enterprise-integration-x12/x12-8.png) 

| Vlastnost | Popis |
| --- | --- |
| Číslo verze ovládacího prvku (ISA12) |Vyberte verzi hello hello X12 standardní |
| Použití ukazatele (ISA15) |Vyberte hello kontextu výměnu.  Hello informace, provozními daty, nebo hodnoty testovacích dat |
| Schéma |Generuje hello GS a ST segmenty pro výměnu kódováním X12 odešle toohello odeslat kanálu |
| SPRAVUJE ORGANIZACE GS1 |Volitelné, vyberte hodnotu pro hello funkční kód z rozevíracího seznamu hello |
| GS2 |Volitelné, odesílatel aplikace |
| GS3 |Volitelné, aplikace příjemce |
| GS4 |Volitelné, vyberte CCYYMMDD nebo RRMMDD |
| GS5 |Volitelné, vyberte hh: mm, HHMMSS nebo HHMMSSdd |
| GS7 |Volitelné, vyberte hodnotu pro příslušné agentury hello z rozevíracího seznamu hello |
| GS8 |Volitelné, verzi dokumentu hello |
| Interchange číslo ovládací prvek (ISA13) |Vyžaduje, zadejte rozsah hodnot pro hello výměnu řízení číslo. Zadejte číselnou hodnotu minimálně 1 a maximálně 999999999 |
| Číslo skupiny ovládací prvek (GS06) |Vyžaduje, zadejte rozsah čísel pro číslo řízení skupiny hello. Zadejte číselnou hodnotu minimálně 1 a maximálně 999999999 |
| Transakce nastavit počet ovládací prvek (ST02) |Vyžaduje, zadejte rozsah čísel pro hello transakce nastavit řízení číslo. Zadejte rozsah číselné hodnoty minimálně 1 a maximálně 999999999 |
| Předvolba |Volitelné, určené pro hello rozsah čísel transakce sadu ovládací prvek použít v potvrzení. Zadejte číselnou hodnotu pro střední dvě pole hello a alfanumerické hodnoty (v případě potřeby) pro pole předponu a příponu hello. Střední pole Hello jsou povinné a obsahovat hello minimální a maximální hodnoty pro číslo hello ovládací prvek |
| Přípona |Volitelné, určené pro hello rozsah čísel transakce sadu řízení používány potvrzení. Zadejte číselnou hodnotu pro střední dvě pole hello a alfanumerické hodnotu (v případě potřeby) pro pole předponu a příponu hello. Střední pole Hello jsou povinné a obsahovat hello minimální a maximální hodnoty pro číslo hello ovládací prvek |

### <a name="character-sets-and-separators"></a>Znakové sady a oddělovače

Kromě hello znaková sada, můžete zadat jinou sadu oddělovače u každého typu zprávy. Pokud znakovou sadu není určena pro danou zprávou schéma, se používá hello výchozí znakovou sadu.

![Zadejte oddělovače pro typ zprávy](./media/logic-apps-enterprise-integration-x12/x12-9.png) 

| Vlastnost | Popis |
| --- | --- |
| Znakové sady toobe použít |toovalidate hello vlastnosti, vyberte hello X12 znakovou sadu. Možnosti Hello jsou Basic, rozšířené a UTF8. |
| Schéma |Vyberte z rozevíracího seznamu hello schéma. Po dokončení každý řádek je automaticky přidán nový řádek. Pro vybrané schéma hello vyberte hello oddělovačů nastavit, které chcete toouse, podle hello následující popisy oddělovače. |
| Typ vstupu |Vyberte z rozevíracího seznamu hello typem vstupu. |
| Součást oddělovače |složené datové prvky tooseparate, zadejte jeden znak. |
| Oddělovač elementu dat |tooseparate jednoduché datové elementů v rámci složené datové prvky, zadejte jeden znak. |
| Nahrazení používá znak |Zadejte znak, kterým používá k nahrazení všech znaků oddělujících v datové části hello při generování zpráv hello odchozí X12. |
| Segment ukončovací znak |tooindicate hello konec EDI segment zadejte jeden znak. |
| Přípona |Vyberte hello znak, který se používá s identifikátorem hello segmentu. Pokud jste určit příponu pak hello segment ukončovací datový prvek nesmí být prázdné. Pokud segment ukončovací hello je prázdné, je třeba určit příponu. |

> [!TIP]
> tooprovide speciální znak hodnoty, upravit smlouvu hello jako JSON a zadejte hodnotu ASCII hello hello speciální znak.

### <a name="validation"></a>Ověření

![Nastavit vlastnosti ověření pro zasílání zpráv](./media/logic-apps-enterprise-integration-x12/x12-10.png) 

Po dokončení každý řádek ověření jiné automaticky přidá. Pokud nezadáte všechna pravidla, ověření používá řádek "Výchozí" hello.

| Vlastnost | Popis |
| --- | --- |
| Typ zprávy |Vyberte typ zprávy EDI hello. |
| Ověření EDI |Proveďte ověření EDI na typy dat definované schéma hello EDI vlastnosti, omezení délky, prázdný datové prvky a koncové oddělovače. |
| Rozšířené ověření |Pokud není hello datový typ EDI, ověření je na hello data element požadavku a povoleny opakování, výčty a data element délka ověření (min/max). |
| Povolit úvodní nebo koncové nuly |Zachování všechny další počáteční nebo koncové nula a místo znaků. Nevysunujte tyto znaky. |
| Trim – úvodní nebo koncové nuly |Odeberte počáteční nebo koncové nulový počet znaků. |
| Koncové oddělovače zásad |Generovat koncové oddělovače. <p>Vyberte **není povoleno** výměnu odeslat koncové oddělovače tooprohibit a oddělovače v hello. Pokud hello výměnu koncové oddělovače a oddělovačů, výměnu hello je deklarovaná není platný. <p>Vyberte **volitelné** toosend interchanges s nebo bez koncové oddělovače a oddělovačů. <p>Vyberte **povinné** Pokud hello odeslané výměnu musí mít koncové oddělovače a oddělovačů. |

## <a name="find-your-created-agreement"></a>Najít vaší vytvořené smlouvy

1.  Po dokončení nastavení všechny vaše smlouvy vlastnosti na hello **přidat** okně zvolte **OK** toofinish vytváření smlouvy a okně návratový tooyour integrace účtu.

    Nově přidané smlouvy nyní se zobrazí v vaše **smlouvy** seznamu.

2.  Můžete také zobrazit vaše smlouvy v váš účet Přehled integrace. V okně účtu vaší integrace, zvolte **přehled**, pak vyberte hello **smlouvy** dlaždici.

    ![Vyberte dlaždici tooview "Smlouvy" všechny smlouvy](./media/logic-apps-enterprise-integration-x12/x12-1-5.png)   

## <a name="view-hello-swagger"></a>Zobrazení hello swagger
V tématu hello [swagger podrobnosti](/connectors/x12/). 

## <a name="learn-more"></a>Další informace
* [Další informace o hello Enterprise integračního balíčku](../logic-apps/logic-apps-enterprise-integration-overview.md "Další informace o Enterprise integračního balíčku")  

