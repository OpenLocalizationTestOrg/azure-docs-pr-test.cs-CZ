---
title: "aaaCreate prvního pracovního postupu mezi cloudové aplikace a cloudové služby - Azure Logic Apps | Microsoft Docs"
description: "Přečtěte si, jak automatizovat firemní procesy pro systémovou integraci a integraci podnikových aplikací (EAI) pomocí vytváření a spouštění pracovních postupů v Azure Logic Apps."
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
keywords: "pracovní postup, cloudové aplikace, cloudové služby, firemní procesy, systémová integrace, integrace podnikových aplikací, enterprise application integration, EAI"
documentationcenter: 
ms.assetid: ce3582b5-9c58-4637-9379-75ff99878dcd
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/31/2017
ms.author: LADocs; jehollan; estfan
ms.openlocfilehash: 17ec589b1c8923b5ad3e6479fc856b6ac81754ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-logic-app-workflow-tooautomate-processes-between-cloud-apps-and-cloud-services"></a>Vytvoření první aplikace logiky pracovního postupu tooautomate procesy mezi cloudových aplikací a cloudové služby

Bez psaní jakéhokoli kódu můžete snáze a rychleji automatizovat firemní procesy díky vytváření a spouštění pracovních postupů v [Azure Logic Apps](logic-apps-what-are-logic-apps.md). Tento první příklad ukazuje, jak toocreate aplikace základní logika pracovního postupu, který kontroluje informačního kanálu RSS kanálu pro nový obsah na webu. Jakmile se zobrazí nových položek v informačním kanálu hello webu, aplikace logiky hello odešle e-mailu z účtu aplikaci Outlook nebo z Gmailu.

toocreate a spusťte aplikaci logiky, budete potřebovat tyto položky:

* Předplatné Azure. Pokud předplatné nemáte, můžete [začít s bezplatným účtem Azure](https://azure.microsoft.com/free/). Jinak si můžete [zaregistrovat předplatné s průběžnými platbami](https://azure.microsoft.com/pricing/purchase-options/).

  Vaše předplatné se používá pro účtování využívání aplikace logiky. Přečtěte si, jak funguje [měření využití](../logic-apps/logic-apps-pricing.md) a jaké jsou [ceny](https://azure.microsoft.com/pricing/details/logic-apps) Azure Logic Apps.

Tento příklad také vyžaduje následující položky:

* Účet Outlook.com, Office 365 Outlook nebo Gmail

    > [!TIP]
    > Pokud máte osobní [účet Microsoft](https://account.microsoft.com/account), máte účet Outlook.com. Pokud máte pracovní nebo školní účet Azure, máte účet **Office 365 Outlook**.

* Tooa odkaz informačního kanálu RSS webu. Tento příklad používá hello [RSS pro horní scénářů z webu CNN.com hello](http://rss.cnn.com/rss/cnn_topstories.rss):`http://rss.cnn.com/rss/cnn_topstories.rss`

## <a name="add-a-trigger-that-starts-your-workflow"></a>Přidání triggeru, který spustí váš pracovní postup

A [ *aktivační událost* ](./logic-apps-what-are-logic-apps.md#logic-app-concepts) událost, která spustí pracovní postup aplikace logiky a je hello první položku, která potřebuje aplikace logiky.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com "portál Azure").

2. V levé nabídce hello zvolte **nový** > **Enterprise integrace** > **aplikace logiky** jak je vidět tady:

     ![Azure Portal, Nový, Podniková integrace, Aplikace logiky](media/logic-apps-create-a-logic-app/azure-portal-create-logic-app.png)

   > [!TIP]
   > Můžete také **nový**, zadejte do vyhledávacího pole hello `logic app`, a stiskněte klávesu Enter. Pak zvolte **Aplikace logiky** > **Vytvořit**.

3. Pojmenujte svoji novou aplikaci logiky a vyberte své předplatné Azure. Nyní vytvořte a vyberte skupinu prostředků Azure, která vám pomůže organizovat a spravovat související prostředky Azure. Nakonec vyberte hello datacenter umístění pro hostování aplikace logiky. Až budete připraveni, zvolte **Pin toodashboard** a potom **vytvořit**.

     ![Podrobnosti aplikace logiky](media/logic-apps-create-a-logic-app/logic-app-settings.png)

   > [!NOTE]
   > Když vyberete **Pin toodashboard**, se zobrazí na hello řídicí panel Azure po nasazení aplikace logiky a automaticky otevře. Pokud svou aplikaci logiky nezobrazí na řídicím panelu hello na hello **všechny prostředky** dlaždici, zvolte **najdete v části více**a vyberte svou aplikaci logiky. Nebo vyberte v levé nabídce hello **další služby**. V části **Podniková integrace** zvolte **Aplikace logiky** a vyberte svoji aplikaci logiky.

4. Při prvním otevření aplikace logiky pro hello ukazuje hello logiku aplikace Návrhář šablony, které můžete použít tooget spuštěna. Tentokrát klikněte na **Prázdná aplikace logiky**, abyste mohli aplikaci vytvořit od začátku.

    Hello logiku aplikace Designer otevře a zobrazuje dostupné služby a *aktivační události* , můžete použít v aplikaci logiky.

5. Hello vyhledávacího pole zadejte `RSS`a vyberte této aktivační události: **RSS – Pokud je publikovaná v položku informačního kanálu** 

    ![Trigger RSS](media/logic-apps-create-a-logic-app/rss-trigger.png)

6. Zadejte hello odkaz pro informační kanál RSS hello webu, které chcete tootrack. 

     Můžete také změnit **frekvenci** a **interval**. 
     Tato nastavení určují, jak často má aplikace logiky kontrolovat nové položky a vracet všechny položky nalezené za určité období.

     V tomto příkladu Podíváme se každý den pro horní scénářů odeslány toohello CNN webu.

     ![Nastavení triggeru s informačním kanálem RSS, frekvencí a intervalem](media/logic-apps-create-a-logic-app/rss-trigger-setup.png)

7. Teď svoji práci uložte. (Na hello návrháře řádku nabídek zvolte **Uložit**.)

   ![Uložení aplikace logiky](media/logic-apps-create-a-logic-app/save-logic-app.png)

   Pokud uložíte, aplikace logiky přejde za provozu, ale v současné době svou aplikaci logiky pouze vyhledává nové položky v hello zadaná informační kanál RSS. 
   toomake aktivuje se v tomto příkladu užitečnější, že přidáme akci, která provede aplikace logiky po aktivační událost.

## <a name="add-an-action-that-responds-tooyour-trigger"></a>Přidat akci, která odpovídá tooyour aktivační události

[*Akce*](./logic-apps-what-are-logic-apps.md#logic-app-concepts) je úloha, kterou provádí pracovní postup vaší aplikace logiky. Po přidání aplikace logiky tooyour aktivační událost, můžete přidat akci tooperform operace s daty generovanými této aktivační události. Pro náš příklad teď nemůžeme přidat akci, která se pošle e-mailu, když se objeví nové položky v informačního kanálu RSS hello webu.

1. V Návrháři hello pod aktivační událost, zvolte **nový krok** > **přidat akci** jak je vidět tady:

   ![Přidání akce](media/logic-apps-create-a-logic-app/add-new-action.png)

   Hello návrháře ukazuje [dostupných konektorů](../connectors/apis-list.md) tak, aby tooperform akce můžete vybrat, pokud aktivuje aktivační událost.

2. Podle e-mailový účet, postupujte podle kroků hello pro aplikaci Outlook nebo z Gmailu.

   * e-mailu toosend z účtu Outlook hello vyhledávacího pole zadejte `outlook`. V části **Služby** zvolte **Outlook.com** pro osobní účty Microsoft nebo **Office 365 Outlook** pro pracovní nebo školní účty Azure. 
   V části **Akce** zvolte **Odeslat e-mail**.

       ![Výběr akce Outlooku „Odeslat e-mail“](media/logic-apps-create-a-logic-app/actions.png)

   * e-mailu toosend z účtu z Gmailu hello vyhledávacího pole zadejte `gmail`. 
   V části **Akce** zvolte **Odeslat e-mail**.

       ![Výběr akce Gmailu „Odeslat e-mail“](media/logic-apps-create-a-logic-app/actions-gmail.png)

3. Když se zobrazí výzva k zadání pověření, přihlaste se pomocí hello uživatelské jméno a heslo pro e-mailový účet. 

4. Zadejte hello podrobnosti pro tuto akci, jako je hello cílové e-mailovou adresu a zvolte hello parametry pro hello data tooinclude hello e-mailem, například:

   ![Vyberte data tooinclude v e-mailu](media/logic-apps-create-a-logic-app/rss-action-setup.png)

    Pokud jste zvolili Outlook, vaše aplikace logiky může vypadat podobně jako v tomto příkladu:

    ![Hotová aplikace logiky](media/logic-apps-create-a-logic-app/save-run-complete-logic-app.png)

5.  Uložte provedené změny. (Na hello návrháře řádku nabídek zvolte **Uložit**.)

6. Nyní můžete aplikaci logiky ručně spustit a otestovat ji. Na hello návrháře řádku nabídek zvolte **spustit**. Jinak můžete je nechat aplikaci logiky, zkontrolujte, zda text hello zadal informační kanál RSS podle plánu hello, které jste nastavili.

   Pokud svou aplikaci logiky najde nové položky, hello logiku aplikace odešle e-mailu, který zahrnuje vybraná data. 
   Pokud nebudou nalezeny žádné nové položky, přeskočí aplikace logiky hello akci, která odešle e-mail.

7. toomonitor a zkontrolujte svou aplikaci logiky je spuštění a aktivaci historie, v nabídce aplikace logiky, vyberte **přehled**.

   ![Sledování a kontrola činnosti a historie spouštění aplikaci logiky](media/logic-apps-create-a-logic-app/logic-app-run-trigger-history.png)

   > [!TIP]
   > Pokud nenajdete hello data, která očekáváte, na panelu příkazů hello, opakujte výběr **aktualizovat**.

   Další informace o stavu aplikace logiky toolearn nebo spuštění a aktivaci historie nebo toodiagnose svou aplikaci logiky najdete v tématu [řešení aplikace logiky](logic-apps-diagnosing-failures.md).

      > [!NOTE]
      > Aplikace logiky se bude spouštět dál, dokud ji nevypnete. Zvolte tooturn vypnout vaší aplikace pro tuto chvíli se v nabídce aplikace logiky, **přehled**. Na panelu příkazů hello, zvolte **zakázat**.

Gratulujeme, právě jste vytvořili a spustili svoji první základní aplikaci logiky. Také jste se naučili, jak snadno vytvořit pracovní postupy pro automatizaci procesů, jak a integrovat cloudové aplikace a cloudové služby – a všechno bez programování.

## <a name="manage-your-logic-app"></a>Správa aplikací logiky

toomanage aplikace, můžete provádět úlohy, jako zkontrolujte stav hello, upravit, zobrazit historii, vypnout nebo odstranit aplikaci logiky.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com "portál Azure").

2. V levé nabídce hello zvolte **další služby**. V části **Podniková integrace** zvolte **Logic Apps**. Vyberte svou aplikaci logiky. 

   V nabídce aplikace logiky hello můžete najít tyto úlohy správy logiku aplikace:

   |Úkol|Kroky| 
   |:---|:---| 
   | Zobrazení stavu aplikace, historie spouštění a obecných informací| Zvolte **Přehled**.| 
   | Úprava aplikace | Zvolte **Návrhář aplikace logiky**. | 
   | Zobrazení definice JSON pracovního postupu vaší aplikace | Zvolte **Zobrazení kódu aplikace logiky**. | 
   | Zobrazení operací, které aplikace logiky provedla | Zvolte **Protokol aktivit**. | 
   | Zobrazení předchozích verzí aplikace logiky | Zvolte **Verze**. | 
   | Dočasné vypnutí aplikace | Zvolte **přehled**, potom na panelu příkazů hello, zvolte **zakázat**. | 
   | Odstranění aplikace | Zvolte **přehled**, potom na panelu příkazů hello, zvolte **odstranit**. Zadejte název aplikace logiky a zvolte **Odstranit**. | 

## <a name="get-help"></a>Podpora

tooask otázky, odpovědi na otázky a zjistěte, jaké další Azure Logic Apps uživatelé dělají, navštivte hello [fórum Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

toohelp zlepšení Azure Logic Apps a konektory, hlasovat o nebo odeslání nápadů v hello [web pro zasílání názorů uživatele Azure Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Další kroky

*  [Přidání podmínek a spuštění pracovních postupů](../logic-apps/logic-apps-use-logic-app-features.md)
*    [Šablony pro aplikace logiky](../logic-apps/logic-apps-use-logic-app-templates.md)
*  [Vytváření aplikací logiky ze šablon Azure Resource Manageru](../logic-apps/logic-apps-arm-provision.md)
