---
title: "aaaConnect tooan místní systém SAP v Azure Logic Apps | Microsoft Docs"
description: "Připojení systému SAP místní tooan z pracovní postup aplikace logiky prostřednictvím brány data místně hello"
services: logic-apps
author: padmavc
manager: anneta
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/01/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 594ec5fed337398bf931d396684630ee9f907d2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooan-on-premises-sap-system-from-logic-apps-with-hello-sap-connector"></a>Připojení systému SAP místní tooan z aplikace logiky s konektorem SAP hello 

Brána dat místní Hello vám umožňuje toomanage dat a bezpečný přístup k prostředkům, které jsou na místě. Toto téma ukazuje, jak je možné připojit logiku aplikace tooan v místním SAP systému. V tomto příkladu aplikace logiky požadavky IDOC přes protokol HTTP a odešle odpověď hello zpět.    

> [!NOTE]
> Aktuální omezení: 
> - Aplikace logiky časového limitu, pokud nemáte dokončit všechny kroky potřebné pro hello odpověď v rámci hello [časový limit požadavku](./logic-apps-limits-and-config.md). V tomto scénáři může získat požadavky blokovány. 
> - Výběr souborů Hello nezobrazuje všechna dostupná pole hello. V tomto scénáři můžete ručně přidat cesty.

## <a name="prerequisites"></a>Požadavky

- Instalace a konfigurace hello nejnovější [místní brána dat](https://www.microsoft.com/download/details.aspx?id=53127) verze 1.15.6150.1 nebo novější. [Jak tooconnect toohello místní brána dat v aplikaci logiky](http://aka.ms/logicapps-gateway) seznamy hello kroky. Hello brány musí být nainstalován v místním počítači, abyste mohli pokračovat.

- Stažení a instalace hello nejnovější SAP knihovny klienta na hello stejný počítač, kam jste nainstalovali bránu dat hello. Použijte některou z následujících verze SAP hello: 
    - Serveru SAP
        - Jakýkoli Server SAP tuto podporu hello konektor .NET (NCo) 3.0
 
    - SAP klienta
        - Konektor .NET SAP (NCo) 3.0

## <a name="add-triggers-and-actions-for-connecting-tooyour-sap-system"></a>Přidat triggery a akce pro připojení tooyour systému SAP

konektor SAP Hello má akce, ale není aktivační události. Ano máme toouse jiné aktivační událost při spuštění hello hello pracovního postupu. 

1. Přidat aktivační událost hello požadavků a odpovědí a potom vyberte **nový krok**.

2. Vyberte **přidat akci**a potom vyberte hello SAP konektoru zadáním `SAP` v poli vyhledávání hello:    

     ![Vyberte Server aplikace SAP nebo Server zpráv SAP](media/logic-apps-using-sap-connector/sap-action.png)

3. Vyberte [ **SAP aplikační Server** ](https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server) nebo [ **Server zpráv SAP**](http://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm), podle vašeho nastavení SAP. Pokud nemáte existující připojení, jste výzvami toocreate jeden.

   1. Vyberte **připojit prostřednictvím místní brána dat**a zadejte podrobnosti hello systému SAP:   

       ![Přidat připojovací řetězec tooSAP](media/logic-apps-using-sap-connector/picture2.png)  

   2. V části **brány**, vyberte existující bránu, nebo tooinstall novou bránu, vyberte **instalaci brány**.

        ![Instalace nové brány](media/logic-apps-using-sap-connector/install-gateway.png)
  
   3. Po zadání všech podrobností o hello vyberte **vytvořit**. 
   Služba Logic Apps nakonfiguruje a otestuje připojení hello, a ujistěte se, zda text hello připojení funguje správně.

4. Zadejte název pro připojení k prostředí SAP.

5. Nyní jsou k dispozici různé možnosti SAP Hello. toofind vaše IDOC kategorie, vyberte ze seznamu hello. Nebo ručně zadejte v hello cestu a vyberte hello HTTP odpovědi v hello **textu** pole:

     ![Akce SAP](media/logic-apps-using-sap-connector/picture3.png)

6. Hello akce pro vytváření pro přidání **odpověď HTTP**. zpráva odpovědi Hello by měla být z výstupu SAP hello.

7. Uložte svou aplikaci logiky. Otestujte zasláním IDOC prostřednictvím adresy URL hello HTTP aktivační události. Po hello IDOC je odeslána Čekejte na odpověď hello z aplikace logiky hello:   

     > [!TIP]
     > Podívat, jak příliš[sledování funkce Logic Apps](../logic-apps/logic-apps-monitor-your-logic-apps.md).

Nyní, konektor SAP hello je přidali tooyour aplikace logiky, prozkoumejte další funkce:

- BAPI
- DOKUMENT RFC

## <a name="get-help"></a>Podpora

tooask otázky, odpovědi na otázky a zjistěte, jaké další Azure Logic Apps uživatelé dělají, navštivte hello [fórum Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

toohelp zlepšení Azure Logic Apps a konektory, hlasovat o nebo odeslání nápadů v hello [web pro zasílání názorů uživatele Azure Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Další kroky

- Zjistěte, jak toovalidate, transformace a dalších funkcí jako BizTalk v hello [Enterprise integračního balíčku](../logic-apps/logic-apps-enterprise-integration-overview.md). 
- [Připojení místní tooon dat](../logic-apps/logic-apps-gateway-connection.md) z aplikace logiky
