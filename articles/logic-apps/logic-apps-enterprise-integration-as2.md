---
title: "aaaAS2 zpráv B2B enterprise integrace - Azure Logic Apps | Microsoft Docs"
description: "Výměna AS2 zpráv B2B enterprise integraci s Azure Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: c9b7e1a9-4791-474c-855f-988bd7bf4b7f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: LADocs; mandia
ms.openlocfilehash: 23f9d74dd0ad807e9cdaedb320d60496cfef9bc3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="exchange-as2-messages-for-enterprise-integration-with-logic-apps"></a>Výměna zpráv AS2 pro podnikové integrace s logic apps

Před výměnou zpráv AS2 pro Azure Logic Apps, musíte vytvořit smlouvy AS2 a uložení této smlouvy ve vašem účtu integrace. Tady jsou hello postup, jak toocreate AS2 smlouvy.

## <a name="before-you-start"></a>Než začnete

Tady je hello položky, které budete potřebovat:

* [Integrace účet](../logic-apps/logic-apps-enterprise-integration-accounts.md) který již má definovaný a přidružené k předplatnému Azure
* Alespoň dva [partnery](logic-apps-enterprise-integration-partners.md) které již jsou definované ve vašem účtu integrace a nakonfigurované hello AS2 kvalifikátor pod **obchodní identit**

> [!NOTE]
> Když vytvoříte smlouvu, hello obsah v souboru smlouvy hello musí odpovídat typ smlouvy hello.    

Po jste [vytvoření účtu integrace](../logic-apps/logic-apps-enterprise-integration-accounts.md) a [přidat partnery](logic-apps-enterprise-integration-partners.md), AS2 smlouvu můžete vytvořit pomocí následujících kroků.

## <a name="create-an-as2-agreement"></a>Vytvoření smlouvy AS2

1.  Přihlaste se toohello [portál Azure](http://portal.azure.com "portál Azure").  

2.  Hello levé nabídce vyberte **další služby**. Hello vyhledávacího pole zadejte **integrace** jako filtr. V seznamu výsledků hello vyberte **účty pro integraci**.

    > [!TIP]
    > Pokud nevidíte **další služby**, může mít tooexpand hello nabídky nejdřív. V horní části hello hello sbalené nabídky, vyberte **nabídky Zobrazit**.

    ![Vyberte další služby, filtr na "integraci", "Účty pro integraci"](./media/logic-apps-enterprise-integration-agreements/overview-1.png)

3. V hello **účty pro integraci** okno, které se otevře, vyberte hello integrace účet místo toocreate hello smlouvy.
Pokud nevidíte žádné účty pro integraci, [vytvořit první](../logic-apps/logic-apps-enterprise-integration-accounts.md "všechny informace o účtech integrace").  

    ![Vyberte účet integrace hello kde toocreate hello smlouvy](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. Zvolte hello **smlouvy** dlaždici. Pokud nemáte dlaždici smlouvy, přidejte nejprve hello dlaždice.

    ![Vyberte že dlaždici "Smlouvy"](./media/logic-apps-enterprise-integration-agreements/agreement-1.png)

5. V okně hello smlouvy, které se otevře, zvolte **přidat**.

    ![Zvolte "Přidat"](./media/logic-apps-enterprise-integration-agreements/agreement-2.png)

6. V části **přidat**, zadejte **název** pro vaše smlouvy. Pro **typ smlouvy**, vyberte **AS2**. Vyberte hello **hostitele partnera**, **identitu hostitele**, **hosta partnera**, a **hosta Identity** pro vaše smlouvy.

    ![Zadejte podrobnosti o smlouvě](./media/logic-apps-enterprise-integration-agreements/agreement-3.png)  

    | Vlastnost | Popis |
    | --- | --- |
    | Name (Název) |Název smlouvy hello |
    | Typ smlouvy | Musí být AS2 |
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

    ![Konfigurace "Obdrží nastavení"](./media/logic-apps-enterprise-integration-agreements/agreement-4.png)

2. Volitelně můžete přepsat vlastnosti hello příchozích zpráv výběrem **přepsat vlastnosti zprávy**.

3. toorequire všechny příchozí zprávy toobe podepsané, vyberte **zpráva by měla být podepsána**. Z hello **certifikát** vyberte existující [hosta partnera veřejný certifikát](../logic-apps/logic-apps-enterprise-integration-certificates.md) pro ověření podpisu hello hello zprávy. Nebo vytvořte certifikát hello, pokud nemáte.

4.  Vyberte všechny příchozí zprávy toobe zašifrovaná, toorequire **zpráva by měla šifrovat**. Z hello **certifikát** vyberte existující [privátní certifikát hostitele partnera](../logic-apps/logic-apps-enterprise-integration-certificates.md) pro dešifrování příchozí zprávy. Nebo vytvořte certifikát hello, pokud nemáte.

5. Vyberte toobe toorequire zprávy v komprimovaném tvaru **zpráva by měla být komprimované**.

6. Vyberte toosend oznámení dispozice synchronní zprávy (MDN) pro přijatých zpráv **odeslat MDN**.

7. toosend zaregistrovali MDNs přijatých zpráv, vyberte **odeslat podepsaný MDN**.

8. Vyberte asynchronní MDNs pro přijatých zpráv toosend **odesílat asynchronní MDN**.

9. Jakmile jste hotovi, ujistěte se, že toosave nastavení tak, že zvolíte **OK**.

Nyní je vaše smlouvy připravené toohandle příchozí zprávy, které odpovídají tooyour vybraná nastavení.

| Vlastnost | Popis |
| --- | --- |
| Přepsání vlastností zpráv |Označuje, že je možné přepsat vlastnosti v přijatých zpráv. |
| Zprávu je nutné podepsat. |Vyžaduje toobe zprávy, které jsou digitálně podepsané. Nakonfigurujte hello hosta partnera veřejný certifikát pro ověření podpisu.  |
| Zprávu je nutné zašifrovat. |Vyžaduje toobe zprávy zašifrované. Bez šifrované zprávy budou odmítnuty. Nakonfigurujte privátní certifikát hello hostitele partnera pro dešifrování zprávy hello.  |
| Zprávu je nutné zkomprimovat. |Vyžaduje komprimované toobe zprávy. Non komprimovanou zprávy budou odmítnuty. |
| MDN Text |Hello výchozí dispozice oznámení (MDN) toobe toohello odeslaných zpráv odesílatele zprávy. |
| Odeslat MDN |Vyžaduje MDNs toobe odeslána. |
| Odeslat podepsaný MDN |Vyžaduje MDNs toobe podepsané. |
| Algoritmus povinná kontrola úrovně Důvěryhodnosti |Vyberte algoritmus toouse hello k podepisování zpráv. |
| Odesílat asynchronní MDN | Vyžaduje toobe zprávy odeslané asynchronně. |
| ADRESA URL | Zadejte adresu URL hello kde toosend hello MDNs. |

## <a name="configure-how-your-agreement-sends-messages"></a>Nakonfigurujte, jak vaše smlouvy odešle zprávy

Můžete nakonfigurovat, jak tato smlouva identifikuje a zpracovává odchozích zpráv, které odesílají tooyour partnery prostřednictvím této smlouvy.

1.  V části **přidat**, vyberte **odeslat nastavení**.
Konfigurujte tyto vlastnosti závislosti na vaší smlouvě se hello partnera, který výměny zpráv s vámi. Vlastnost popis najdete v tématu hello tabulky v této části.

    ![Nastavit vlastnosti "Odeslat nastavení" hello](./media/logic-apps-enterprise-integration-agreements/agreement-51.png)

2. toosend podepsané zprávy tooyour partnera, vyberte **povolit podepisování zpráv**. K podepisování zpráv hello v hello **povinná kontrola úrovně Důvěryhodnosti algoritmus** seznamu, vyberte hello *privátní certifikát hostitele partnera povinná kontrola úrovně Důvěryhodnosti algoritmus*. A v hello **certifikát** vyberte existující [privátní certifikát hostitele partnera](../logic-apps/logic-apps-enterprise-integration-certificates.md).

3. toosend šifrované zprávy toohello partnera, vyberte **povolit šifrování zpráv**. Pro šifrování zpráv hello v hello **šifrovací algoritmus** seznamu, vyberte hello *hosta partnera veřejný certifikát algoritmus*.
A v hello **certifikát** vyberte existující [hosta partnera veřejný certifikát](../logic-apps/logic-apps-enterprise-integration-certificates.md).

4. toocompress uvítací zprávu, vyberte **povolit kompresi zpráva**.

5. Hlavička content-type toounfold hello HTTP do jeden řádek, vyberte **hlavičky Unfold HTTP**.

6. tooreceive synchronní MDNs pro hello odeslané zprávy, vyberte **požadavku MDN**.

7. tooreceive zaregistrovali MDNs hello odeslané zprávy, vyberte **požadavek podepsané MDN**.

8. tooreceive asynchronní MDNs pro hello odeslané zprávy, vyberte **žádosti o asynchronní MDN**. Pokud vyberete tuto možnost, zadejte adresu URL hello kde toosend hello MDNs.

9. zrušení toorequire odmítnutí přijetí žádosti, vyberte **povolit NRR**.  

10. toospecify algoritmus formátu toouse v hello povinná kontrola úrovně Důvěryhodnosti nebo přihlášení hello odchozí záhlaví zprávy hello AS2 nebo MDN, vyberte **algoritmus SHA2 formátu**.  

11. Jakmile jste hotovi, ujistěte se, že toosave nastavení tak, že zvolíte **OK**.

Vaše smlouvy je nyní připraven toohandle odchozí zprávy, které odpovídají tooyour vybraná nastavení.

| Vlastnost | Popis |
| --- | --- |
| Povolit podepisování zpráv |Vyžaduje všechny zprávy odeslané z toobe smlouvy hello podepsané. |
| Algoritmus povinná kontrola úrovně Důvěryhodnosti |algoritmus toouse Hello k podepisování zpráv. Konfiguruje hello hostitele partnera privátní certifikát algoritmus povinná kontrola úrovně Důvěryhodnosti pro podepisování zpráv hello. |
| Certifikát |Vyberte certifikát toouse hello k podepisování zpráv. Nakonfiguruje privátní certifikát hello hostitele partnera k podepisování zpráv hello. |
| Povolit šifrování zpráv |Vyžaduje šifrování všechny zprávy odeslané z této smlouvy. Nakonfiguruje hello hosta partnera veřejný certifikát algoritmus pro šifrování zprávy hello. |
| Šifrovací algoritmus |Hello toouse algoritmus šifrování pro šifrování zpráv. Nakonfiguruje hello hosta partnera veřejný certifikát pro šifrování zpráv hello. |
| Certifikát |Hello certifikát toouse tooencrypt zprávy. Nakonfiguruje hello hosta partnera privátní certifikát pro šifrování zpráv hello. |
| Povolit kompresi zpráv |Vyžaduje komprese všechny zprávy odeslané z této smlouvy. |
| Unfold hlavičky protokolu HTTP |Umístí hello HTTP hlavičku content-type na jeden řádek. |
| Žádost o MDN |Vyžaduje MDN pro všechny zprávy odeslané z této smlouvy. |
| Žádost o podepsané MDN |Vyžaduje všechny MDNs odesílaných toothis smlouvy toobe podepsané. |
| Asynchronní MDN požadavku |Vyžaduje asynchronní MDNs toobe odeslané toothis smlouvy. |
| ADRESA URL |Zadejte adresu URL hello kde toosend hello MDNs. |
| Povolit NRR |Vyžaduje neodvolatelnost příjmu (NRR), komunikace atribut, který poskytuje důkazy hello data byla přijata, jak je řešit. |
| Algoritmus SHA2 formátu |Vyberte algoritmus formátu toouse v hello povinná kontrola úrovně Důvěryhodnosti nebo přihlašování hello odchozí záhlaví zprávy AS2 hello nebo MDN |

## <a name="find-your-created-agreement"></a>Najít vaší vytvořené smlouvy

1.  Po dokončení nastavení všechny vaše smlouvy vlastnosti na hello **přidat** okně zvolte **OK** toofinish vytváření smlouvy a okně návratový tooyour integrace účtu.

    Nově přidané smlouvy nyní se zobrazí v vaše **smlouvy** seznamu.

2.  Můžete také zobrazit vaše smlouvy v váš účet Přehled integrace. V okně účtu vaší integrace, zvolte **přehled**, pak vyberte hello **smlouvy** dlaždici. 

    ![Vyberte dlaždici tooview "Smlouvy" všechny smlouvy](./media/logic-apps-enterprise-integration-agreements/agreement-6.png)

## <a name="view-hello-swagger"></a>Zobrazení hello swagger
V tématu hello [swagger podrobnosti](/connectors/as2/). 

## <a name="next-steps"></a>Další kroky
* [Další informace o hello Enterprise integračního balíčku](logic-apps-enterprise-integration-overview.md "Další informace o Enterprise integračního balíčku")  
