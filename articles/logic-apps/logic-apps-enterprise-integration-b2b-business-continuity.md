---
title: "obnovení aaaDisaster pro účet Integrace B2B – Azure Logic Apps | Microsoft Docs"
description: "Zotavení po havárii B2B aplikace logiky"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: e86564a3c5a2607d22514936c606e2843cba0416
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="logic-apps-b2b-cross-region-disaster-recovery"></a>Zotavení po havárii mezi oblastmi B2B aplikace logiky

Úlohy B2B zahrnovat peníze transakce jako objednávky a faktury. Během události po havárii je velmi důležitá pro hello toomeet obnovit tooquickly firmy, které firemní úrovni SLA dohodnutých s jejich partnery. Tento článek ukazuje, jak toobuild kontinuity podnikových procesů plánu pro úlohy B2B. 

* Připravenost obnovení po havárii 
* Převzetí služeb při selhání toosecondary oblast během události po havárii 
* Vrátit zpět tooprimary oblast po události po havárii

## <a name="disaster-recovery-readiness"></a>Připravenost obnovení po havárii  

1. Určete sekundární oblasti a vytvořte [integrace účet](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) v sekundární oblasti hello.

2. Přidání partnery, schémat a smluv pro toky zpráva hello požadované kde hello spustit stav potřebuje toobe replikovat toosecondary oblasti integrace účtu.

   > [!TIP]
   > Ujistěte se, že konzistence v hello integrace účet artefaktů na zásady vytváření názvů v oblastech. 

3. hello toopull spustit stav z hello primární oblasti, vytvoření aplikace logiky v sekundární oblasti hello. 

   Musí mít tuto aplikaci logiky *aktivační událost* a *akce*. 
   aktivační událost Hello by se měly připojit tooprimary oblasti integrace účet a hello akce by se měly připojit toosecondary oblasti integrace účtu. 
   Podle hello časový interval, aktivační událost hello dotazování hello primární oblasti spustit stav tabulce a vrátí nové záznamy hello, pokud existuje. Akce Hello je aktualizuje toosecondary oblasti integrace účtu. 
   To pomáhá tooget přírůstkové běhový stav z oblasti toosecondary primární oblasti.

4. Kontinuita podnikových procesů v Logic Apps je integrace účet určený toosupport založené na protokolech B2B - X12 AS2, EDIFACT a. toofind podrobné kroky, vyberte hello příslušné odkazy.

5. Hello doporučení je toodeploy všechny prostředky primární oblasti v sekundární oblasti příliš. 

   Primární oblasti prostředky zahrnují Azure SQL Database nebo Azure Cosmos DB, Azure Service Bus a Azure Event Hubs použít pro zasílání zpráv Azure API Management a funkce hello Azure Logic Apps v Azure App Service.   

6. Připojení z primární oblasti tooa sekundární oblasti. hello toopull spustit stav od primární oblasti, vytvoření aplikace logiky v sekundární oblasti. 

   aplikace logiky Hello by měl mít aktivační události a akce. 
   aktivační událost Hello by měl připojit tooa primární oblasti integrace účet. 
   Hello akce by měl připojit tooa sekundární oblasti integrace účet. 
   Podle hello časový interval, aktivační událost hello dotazování hello primární oblasti spustit stav tabulce a vrátí nové záznamy hello, pokud existuje. 
   Akce Hello je aktualizuje tooa sekundární oblasti integrace účtu. 
   Tento proces pomáhá tooget přírůstkové běhový stav ze sekundární oblasti toohello hello primární oblasti.

Kontinuita podnikových procesů v účtu integrace Logic Apps poskytuje podporu na základě hello B2B protokoly X12, AS2 a EDIFACT. Podrobné pokyny týkající se použití X12 a AS2 najdete v tématu [X12](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#x12) a [AS2](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#as2) v tomto článku.

## <a name="fail-over-tooa-secondary-region-during-a-disaster-event"></a>Převzetí služeb při selhání tooa sekundární oblasti během události po havárii

Během události po havárii, když primární oblasti hello není k dispozici pro kontinuitu podnikových procesů, přímé přenosy toohello sekundární oblast. Obchodní toorecover funkce rychle toomeet hello RPO/RTO dohodnutých jejich partnery pomáhá sekundární oblast. Minimalizuje také úsilí toofail přes z jedné oblasti tooanother oblasti. 

Při kopírování řízení čísla od primární oblasti sekundární oblasti tooa očekávané latence neexistuje. tooavoid odesílání duplicitní generovaného řízení čísla toopartners během události po havárii, hello doporučení je tooincrement hello řízení čísla ve smlouvách sekundární oblasti hello pomocí [rutiny prostředí PowerShell](https://blogs.msdn.microsoft.com/david_burgs_blog/2017/03/09/fresh-of-the-press-new-azure-powershell-cmdlets-for-upcoming-x12-connector-disaster-recovery).

## <a name="fall-back-tooa-primary-region-post-disaster-event"></a>Vrátit zpět tooa primární oblasti po havárii událostí

toofall back tooa primární oblasti případě, že je k dispozici, postupujte takto:

1. Zastavte příjem zpráv od partnerů v sekundární oblasti hello.  

2. Zvýšit číslo řízení hello generované pro všechny smlouvy primární oblasti hello pomocí [rutiny prostředí PowerShell](https://blogs.msdn.microsoft.com/david_burgs_blog/2017/03/09/fresh-of-the-press-new-azure-powershell-cmdlets-for-upcoming-x12-connector-disaster-recovery).  

3. Přímý přenos dat z primární oblasti toohello hello sekundární oblast.

4. Zkontrolujte, zda je povoleno aplikaci logiky hello vytvořen v sekundární oblasti hello pro stahování, spusťte stavu od primární oblasti hello.

## <a name="x12"></a>X12 

Kontinuita podnikových procesů pro EDI X 12 dokumentů je založena na ovládací prvek čísla:

> [!TIP]
> Můžete taky hello [X12 rychlý start šablony](https://azure.microsoft.com/documentation/templates/201-logic-app-x12-disaster-recovery-replication/) toocreate logiku aplikace. Vytváření primární a sekundární integrace účty jsou požadavky toouse hello šablony. Hello šablony pomáhá toocreate dvě aplikace logiky, jednu pro čísla přijaté řízení a druhou pro generovaný řízení čísla. Příslušné triggery a akce se vytvoří v hello aplikace logiky, připojení hello aktivační událost toohello primární integrace účet a hello akce toohello sekundární integrace účet.

**Požadavky**

tooenable zotavení po havárii pro příchozí zprávy, vyberte v nastavení přijímat hello X12 smlouvy hello duplicitní zkontrolujte nastavení.

![Vyberte nastavení duplicitní kontroly](./media/logic-apps-enterprise-integration-b2b-business-continuity/dupcheck.png)  

1. Vytvoření [aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md) v sekundární oblasti.    

2. Hledání **X12**a vyberte **X12-při změně číslo řízení**.   

   ![Vyhledejte X12](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn1.png)

   aktivační událost Hello vyzve k zadání tooestablish účet připojení tooan integrace. 
   aktivační událost Hello by měl být připojený tooa primární oblasti integrace účtu.

3. Zadejte název připojení, vyberte vaše *primární oblasti integrace účet* z hello seznam a vyberte **vytvořit**.   

   ![Název účtu primární oblasti integrace](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn2.png)

4. Hello **data a času toostart řízení číslo synchronizace** nastavení je volitelné. Hello **frekvence** lze nastavit příliš**den**, **hodinu**, **minutu**, nebo **druhý** se v intervalu.   

   ![Data a času a frekvence](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn3.png)

5. Vyberte **nový krok** > **přidat akci**.

   ![Nový krok, přidat na akce](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn4.png)

6. Hledání **X12**a vyberte **X12-přidáte nebo aktualizujete řízení čísla**.   

   ![Přidat nebo aktualizovat čísla ovládací prvek](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn5.png)

7. Vyberte účet akce tooa sekundární oblasti integrace tooconnect **změnit připojení** > **přidat nové připojení** pro seznam účtů, k dispozici integrace hello. Zadejte název připojení, vyberte vaše *sekundární oblasti integrace účet* z hello seznam a vyberte **vytvořit**. 

   ![Název účtu služby sekundární oblast integrace](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn6.png)

8. Kliknutím na ikonu hello v pravém horním rohu přepínače tooraw vstupy.

   ![Vstupy tooraw přepínače](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12rawinputs.png)

9. Vyberte textu v hello dynamického obsahu pro výběr a uložte aplikaci logiky hello.

   ![Dynamická pole obsahu](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn7.png)

   Podle hello časový interval, aktivační události hello dotazování hello primární oblasti přijatých řízení číslo tabulky a vrátí nové záznamy hello. 
   Akce Hello aktualizuje hello záznamy v hello sekundární oblasti integrace účtu. 
   Pokud nejsou dostupné žádné aktualizace, stav hello aktivační události zobrazuje jako **vynecháno**.   

   ![Číslo tabulky ovládací prvek](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12recevicedcn8.png)

Podle hello časový interval, hello přírůstkové běhový stav replikuje z sekundární oblasti tooa primární oblasti. Během události po havárii, když primární oblasti hello není k dispozici, přímé přenosy toohello sekundární oblast pro kontinuitu podnikových procesů. 

## <a name="edifact"></a>EDIFACT 

Kontinuita podnikových procesů pro dokumenty EDI EDIFACT je založena na ovládací prvek čísla.

**Požadavky**

tooenable zotavení po havárii pro příchozí zprávy, vyberte v nastavení přijímat smlouvy EDIFACT hello duplicitní zkontrolujte nastavení.

![Vyberte nastavení duplicitní kontroly](./media/logic-apps-enterprise-integration-b2b-business-continuity/edifactdupcheck.png)  

1. Vytvoření [aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md) v sekundární oblasti.    

2. Hledání **EDIFACT**a vyberte **EDIFACT - při změně číslo řízení**.

   ![Vyhledejte EDIFACT](./media/logic-apps-enterprise-integration-b2b-business-continuity/edifactcn1.png)

   aktivační událost Hello vyzve k zadání tooestablish účet připojení tooan integrace. 
   aktivační událost Hello by měl být připojený tooa primární oblasti integrace účtu. 

3. Zadejte název připojení, vyberte vaše *primární oblasti integrace účet* z hello seznam a vyberte **vytvořit**.    

   ![Název účtu primární oblasti integrace](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12CN2.png)

4. Hello **data a času toostart řízení číslo synchronizace** nastavení je volitelné. Hello **frekvence** lze nastavit příliš**den**, **hodinu**, **minutu**, nebo **druhý** se v intervalu.    

   ![Data a času a frekvence](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn3.png)

6. Vyberte **nový krok** > **přidat akci**.    

   ![Nový krok, přidat na akce](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn4.png)

7. Hledání **EDIFACT**a vyberte **EDIFACT - přidáte nebo aktualizujete řízení čísla**.   

   ![Přidat nebo aktualizovat čísla ovládací prvek](./media/logic-apps-enterprise-integration-b2b-business-continuity/EdifactChooseAction.png)

8. Vyberte účet akce tooa sekundární oblasti integrace tooconnect **změnit připojení** > **přidat nové připojení** pro seznam účtů, k dispozici integrace hello. Zadejte název připojení, vyberte vaše *sekundární oblasti integrace účet* z hello seznam a vyberte **vytvořit**.

   ![Název účtu služby sekundární oblast integrace](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn6.png)

9. Kliknutím na ikonu hello v pravém horním rohu přepínače tooraw vstupy.

   ![Vstupy tooraw přepínače](./media/logic-apps-enterprise-integration-b2b-business-continuity/Edifactrawinputs.png)

10. Vyberte textu v hello dynamického obsahu pro výběr a uložte aplikaci logiky hello.   

   ![Dynamická pole obsahu](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12CN7.png)

   Podle hello časový interval, aktivační události hello dotazování hello primární oblasti přijatých řízení číslo tabulky a vrátí nové záznamy hello.
   Akce Hello aktualizuje hello záznamy toohello sekundární oblasti integrace účtu. 
   Pokud nejsou dostupné žádné aktualizace, stav hello aktivační události zobrazuje jako **vynecháno**.

   ![Číslo tabulky ovládací prvek](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12recevicedcn8.png)

Podle hello časový interval, hello přírůstkové běhový stav replikuje z sekundární oblasti tooa primární oblasti. Během události po havárii, když primární oblasti hello není k dispozici, přímé přenosy toohello sekundární oblast pro kontinuitu podnikových procesů. 

## <a name="as2"></a>AS2 

Kontinuita podnikových procesů pro dokumenty, které používají protokol AS2 hello je na základě ID zprávy hello a hello povinná kontrola úrovně Důvěryhodnosti hodnoty.

> [!TIP]
> Můžete taky hello [šablony rychlý start AS2](https://github.com/Azure/azure-quickstart-templates/pull/3302) toocreate logiku aplikace. Vytváření primární a sekundární integrace účty jsou požadavky toouse hello šablony. Šablona Hello pomáhá vytvářet aplikace logiky, který má aktivační události a akce. aplikace logiky Hello vytvoří připojení z účet primární integrace tooa aktivace a účet akce tooa sekundární integrace.

1. Vytvoření [aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md) v sekundární oblasti hello.  

2. Hledání **AS2**a vyberte **AS2 - hodnota při povinná kontrola úrovně Důvěryhodnosti je vytvořena**.   

   ![Vyhledejte AS2](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid1.png)

   Aktivační událost vyzve tooestablish účet připojení tooan integrace. 
   aktivační událost Hello by měl být připojený tooa primární oblasti integrace účtu. 
   
3. Zadejte název připojení, vyberte vaše *primární oblasti integrace účet* z hello seznam a vyberte **vytvořit**.

   ![Název účtu primární oblasti integrace](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid2.png)

4. Hello **synchronizace hodnotu data a času toostart povinná kontrola úrovně Důvěryhodnosti** nastavení je volitelné. Hello **frekvence** lze nastavit příliš**den**, **hodinu**, **minutu**, nebo **druhý** se v intervalu.   

   ![Data a času a frekvence](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid3.png)

5. Vyberte **nový krok** > **přidat akci**.  

   ![Nový krok, přidat na akce](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid4.png)

6. Hledání **AS2**a vyberte **AS2 - přidáte nebo aktualizujete obsah povinná kontrola úrovně Důvěryhodnosti**.  

   ![Povinná kontrola úrovně Důvěryhodnosti přidání nebo aktualizace](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid5.png)

7. Vyberte účet akce tooa sekundární integrace tooconnect **změnit připojení** > **přidat nové připojení** pro seznam účtů, k dispozici integrace hello. Zadejte název připojení, vyberte vaše *sekundární oblasti integrace účet* z hello seznam a vyberte **vytvořit**.

   ![Název účtu služby sekundární oblast integrace](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid6.png)

8. Kliknutím na ikonu hello v pravém horním rohu přepínače tooraw vstupy.

   ![Vstupy tooraw přepínače](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2rawinputs.png)

9. Vyberte textu v hello dynamického obsahu pro výběr a uložte aplikaci logiky hello.   

   ![Dynamický obsah](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid7.png)

   Podle hello časový interval, aktivační události hello dotazování hello primární oblasti tabulku a vrátí nové záznamy hello. Akce Hello je aktualizuje toohello sekundární oblasti integrace účtu. 
   Pokud nejsou dostupné žádné aktualizace, stav hello aktivační události zobrazuje jako **vynecháno**.  

   ![Primární oblasti tabulku](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid8.png)

Podle hello časový interval, hello přírůstkové běhový stav replikuje z hello primární oblasti toohello sekundární oblast. Během události po havárii, když primární oblasti hello není k dispozici, přímé přenosy toohello sekundární oblast pro kontinuitu podnikových procesů. 

## <a name="next-steps"></a>Další kroky

[Monitorování zpráv B2B](logic-apps-monitor-b2b-message.md)

