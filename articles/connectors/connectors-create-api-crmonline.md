---
title: aaaConnect tooDynamics 365 (online) z Azure Logic Apps | Microsoft Docs
description: "Vytvořit logiku aplikace pracovní postupy, které spravovat Dynamics 365 entity (online) prostřednictvím hello rozhraní API poskytované hello Dynamics 365 konektoru"
services: logic-apps
cloud: Azure Stack
author: Mattp123
manager: anneta
documentationcenter: 
tags: connectors
ms.assetid: 0dc2abef-7d2c-4a2d-87ca-fad21367d135
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: matp; LADocs
ms.openlocfilehash: 183d7a6b8e5d2c0eecc70da0da3806e06c382df4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-toodynamics-365-from-logic-app-workflows"></a>Připojit tooDynamics 365 z pracovních aplikace logiky

S Logic Apps můžete připojit tooDynamics 365 (online) a vytvořit toky užitečné firmy, které vytvořit záznamy, aktualizovat položky nebo vrátí seznam záznamů. Konektor hello Dynamics 365 můžete:

* Sestavení vaší firmy toku na základě hello dat, které můžete získat z Dynamics 365 (online).
* Pomocí akcí, které se odpověď a pak proveďte výstup hello k dispozici pro další akce. Při aktualizaci položky v Dynamics 365 (online), můžete odeslat e-mailu pomocí Office 365.

Toto téma ukazuje, jak toocreate aplikace logiky, vytvoří úlohu v Dynamics 365 vždy, když se vytvoří nové zájemce v Dynamics 365.

## <a name="prerequisites"></a>Požadavky
* Účet Azure.
* Účet Dynamics 365 (online).

## <a name="create-a-task-when-a-new-lead-is-created-in-dynamics-365"></a>Vytvoření úlohy při vytváření nového zájemce v Dynamics 365

1.  [Přihlaste se tooAzure](https://portal.azure.com).

2.  Hello Azure vyhledávacího pole zadejte `Logic apps`, a stiskněte klávesu ENTER.

      ![Najít Logic Apps](./media/connectors-create-api-crmonline/find-logic-apps.png)

3.  V části **Logic apps**, klikněte na tlačítko **přidat**.

      ![Přidat LogicApp](./media/connectors-create-api-crmonline/add-logic-app.png)

4.  aplikace logiky toocreate hello, dokončení hello **název**, **předplatné**, **skupiny prostředků**, a **umístění** pole a pak klikněte na tlačítko  **Vytvoření**.

5.  Vyberte nové aplikace logiky hello. Po přijetí hello **nasazení bylo úspěšné** oznámení, klikněte na tlačítko **aktualizovat**.

6.  V části **nástroje pro vývoj**, klikněte na tlačítko **návrhář aplikace na základě logiky**. V seznamu hello šablony, klikněte na tlačítko **prázdné aplikace logiky**.

7.  Hello vyhledávacího pole zadejte `Dynamics 365`. Z hello Dynamics 365 aktivuje seznamu, vyberte **Dynamics 365 – když se vytvoří záznam**.

8.  Pokud jste výzvami toosign v tooDynamics 365, udělejte to teď.

9.  V podrobnostech aktivační událost hello zadejte hello následující informace:

  * **Název organizace**. Vyberte hello Dynamics 365 instance, která má toolisten aplikace logiky hello k.

  * **Název entity**. Vyberte hello entita, která chcete toolisten k. Tato událost funguje jako aplikace logiky hello toostart aktivační události. 
  V tomto návodu **vede** je vybrána.

  * **Jak často chcete toocheck pro položky?** Tyto hodnoty nastaveny, jak často hello logiku aplikace zkontroluje aktualizace související toohello aktivační události. Hello výchozí nastavení je toocheck aktualizací každé tři minuty.

    * **Frekvence**. Vyberte sekund, minut, hodin nebo dnů.

    * **Interval**. Zadejte hello počet sekund, minut, hodin nebo dnů, kdy chcete toopass před hello další kontroly.

      ![Podrobnosti o logiku aktivační událostí aplikace](./media/connectors-create-api-crmonline/trigger-details.png)

10. Klikněte na tlačítko **nový krok**a potom klikněte na **přidat akci**.

11. Hello vyhledávacího pole zadejte `Dynamics 365`. Hello akce seznamu, vyberte **Dynamics 365 – vytvořit nový záznam**.

12. Zadejte hello následující informace:

    * **Název organizace**. Vyberte instanci hello Dynamics 365 místo hello toku toocreate hello záznamu. 
    Všimněte si, že tato instance nemá toobe hello stejné instance, kde je aktivována událost hello z.

    * **Název entity**. Vyberte, chcete toocreate záznam, když je aktivována událost hello entity hello. 
    V tomto návodu **úlohy** je vybrána.

13. Klikněte na tlačítko v hello **subjektu** pole, které se zobrazí. Z hello dynamického obsahu seznamu, který se zobrazí můžete si vybrat jednu z těchto polí:

    * **Příjmení**. Hello příjmení pro hello realizace výběru v tomto poli vloží do pole Subject hello hello úlohy, při záznamu úloh hello.
    * **Téma**. Pokud vyberete toto pole vloží hello tématu pole pro hello realizace do pole Předmět hello hello úkolu, hello úloh záznam vytvoření. 
    Klikněte na tlačítko **tématu** tooadd této toohello **subjektu** pole.

      ![Logiku aplikace vytvoří nové podrobnosti záznamu](./media/connectors-create-api-crmonline/create-record-details.png)

14. Na panelu nástrojů hello návrhář aplikace logiky, klikněte na tlačítko **Uložit**.

    ![Návrhář aplikace na základě nástrojů Logika uložit](./media/connectors-create-api-crmonline/designer-toolbar-save.png)

15. Klikněte na tlačítko toostart hello aplikace logiky **spustit**.

    ![Návrhář aplikace na základě nástrojů Logika uložit](./media/connectors-create-api-crmonline/designer-toolbar-run.png)

16. Teď vytvořte záznam realizace v 365 Dynamics pro prodej a v tématu vaše toku v akci!

## <a name="set-advanced-options-for-a-logic-app-step"></a>Upřesnit možnosti pro krok aplikace logiky

jak toofilter dat v kroku aplikaci logiky, klikněte na tlačítko toospecify **zobrazit rozšířené možnosti** v tomto kroku přidáte filtru nebo pořadí dotazem.

Můžete například použít pouze aktivní účty filtru dotazu tooget a pořadí podle názvu účtu hello. tooperform to úloh, zadejte dotaz filtru OData hello `statuscode eq 1`a vyberte **název účtu** hello dynamického obsahu seznamu. Další informace: [MSDN: $filter](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_1) a [$orderby](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_2).

![Aplikace logiky rozšířené možnosti](./media/connectors-create-api-crmonline/advanced-options.png)

### <a name="best-practices-when-using-advanced-options"></a>Osvědčené postupy při použití rozšířené možnosti

Při přidání pole tooa hodnotu, zda zadejte hodnotu, nebo vyberte hodnotu ze seznamu obsahu dynamických hello musí odpovídat typ pole hello.

Typ pole  |Jak toouse  |Kde toofind  |Name (Název)  |Datový typ  
---------|---------|---------|---------|---------
Textová pole|Textová pole vyžadují jeden řádek textu nebo dynamický obsah, který je pole typu text. Mezi příklady patří pole kategorie a dílčí kategorie hello.|Nastavení > Přizpůsobení > Přizpůsobit hello System > entity > úlohy > pole |category |Jeden řádek textu        
Pole celé číslo | Některá pole vyžadují celé číslo nebo dynamický obsah, který je pole typu integer. Mezi příklady patří dokončeno % a doby trvání. |Nastavení > Přizpůsobení > Přizpůsobit hello System > entity > úlohy > pole |Procento dokončení |Celé číslo         
Datum pole | Některá pole vyžadují datum ve formátu mm/dd/rrrr nebo dynamický obsah, který je pole typu datum. Příklady jsou vytvořené na počáteční datum, skutečné zahájení poslední na dobu uchování, skutečný konec a datum splatnosti. | Nastavení > Přizpůsobení > Přizpůsobit hello System > entity > úlohy > pole |createdon |Datum a čas
Zadejte pole, které vyžadují záznamu ID i vyhledávání |Některá pole, které odkazují na jiný záznam entity vyžadují ID záznamu hello a typ vyhledávání hello. |Nastavení > Přizpůsobení > Přizpůsobit hello System > entity > účet > pole  | ID účtu  | Primární klíč

### <a name="more-examples-of-fields-that-require-both-a-record-id-and-lookup-type"></a>Zadejte další příklady pole, které vyžadují ID záznamu a vyhledávání
Rozšíření v předchozí tabulce hello, zde jsou další příklady pole, která není pracovat s hodnotami vybraný ze seznamu obsahu dynamických hello. Místo toho tato pole vyžadují obě ID a vyhledávací typ záznamu zadali do pole hello v PowerApps.  
* Vlastník a typ vlastníka. v poli Vlastník Hello musí být platné ID záznamu uživateli nebo týmu Hello vlastníka typ musí být buď **systemusers** nebo **týmy**.
* Zákazníka a typ odběratele. Hello zákazníka pole musí být platný účet nebo se obraťte na ID záznamu. Hello vlastníka typ musí být buď **účty** nebo **kontakty**.
* A typu. Hello týkající se pole musí být platný záznam ID, jako je například účet nebo se obraťte na ID záznamu. Hello ohledně typu musí být hello vyhledávání typu hello záznam, jako například **účty** nebo **kontakty**.

Hello následující příklad akce vytvoření úloh přidá záznam klienta, která odpovídá ID záznamu toohello přidáním toohello týkající se pole hello úlohy.

![Tok recordId a typ účtu](./media/connectors-create-api-crmonline/recordid-type-account.png)

Tento příklad také přiřadí hello úloh tooa konkrétního uživatele na základě ID uživatele hello záznamu.

![Tok recordId a typ účtu](./media/connectors-create-api-crmonline/recordid-type-user.png)

toofind záznam je ID, najdete v následující části hello: *najít ID záznamu hello*

## <a name="find-hello-record-id"></a>Najít ID záznamu hello

1. Otevřete záznam, jako je například záznam klienta.

2. Na panelu nástrojů hello akce, klikněte na tlačítko **Pop Out** ![záznam zobrazovat](./media/connectors-create-api-crmonline/popout-record.png).
Případně na panelu nástrojů hello akce toocopy hello úplnou adresu URL do výchozí program e-mailu, klikněte na tlačítko **odkazu A e-MAILU**.

   ID záznamu Hello se zobrazí mezi hello % 7b a %7 d kódování znaků z adresy URL hello.

   ![Tok recordId a typ účtu](./media/connectors-create-api-crmonline/recordid.png)

## <a name="troubleshooting"></a>Řešení potíží
tootroubleshoot vadný krok v aplikaci logiky, zobrazení Podrobnosti o stavu hello hello události.

1. V části **Logic Apps**, vyberte svou aplikaci logiky a pak klikněte na tlačítko **přehled**. 

   Hello v oblasti souhrnu se zobrazí a poskytuje stav hello spustit pro aplikaci logiky hello. 

   ![Stav spuštění aplikace logiky](./media/connectors-create-api-crmonline/tshoot1.png)

2. tooview Další informace o všechny neúspěšné spuštění, klikněte na tlačítko hello se nezdařilo událostí. Klikněte na tlačítko tooexpand vadný krok tohoto kroku.

   ![Rozbalte vadný krok](./media/connectors-create-api-crmonline/tshoot2.png)

   Podrobnosti krok Hello zobrazí a mohou pomoci při odstraňování hello příčinu selhání hello.

   ![Krok podrobnosti neúspěšné](./media/connectors-create-api-crmonline/tshoot3.png)

Další informace o odstraňování potíží s aplikací logiky najdete v tématu [diagnostikování selhání aplikace logiky](../logic-apps/logic-apps-diagnosing-failures.md).

## <a name="connector-specific-details"></a>Podrobnosti o konkrétní konektor

Zobrazit všechny aktivační události a akce definované v hello swagger a také zobrazit žádné limity v hello [connector – podrobnosti](/connectors/crm/). 

## <a name="next-steps"></a>Další kroky
Prozkoumejte hello dalších dostupných konektorů v Logic Apps v našem [rozhraní API seznamu](apis-list.md).
