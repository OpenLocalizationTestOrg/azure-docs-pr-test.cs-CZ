---
title: "aaaConnect tooon místní systém souborů z Azure Logic Apps | Microsoft Docs"
description: "Připojit místní tooon systémy souborů z pracovní postup aplikace logiky prostřednictvím brány dat hello místní a konektor systému souborů"
keywords: "systémy souborů"
services: logic-apps
author: derek1ee
manager: anneta
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/27/2017
ms.author: LADocs; deli
ms.openlocfilehash: beb5565293def4aba81f63f19e77d7498aac38c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooon-premises-file-systems-from-logic-apps-with-hello-file-system-connector"></a>Připojit místní tooon systémy souborů z aplikace logiky s konektorem hello systému souborů

Hybridní cloud připojení je centrální toologic aplikace, takže toomanage data a bezpečný přístup k místním prostředkům, aplikace logiky můžete použít hello místní data gateway. V tomto článku ukážeme, jak tooconnect tooan místní systém souborů pomocí základní scénáře: kopírování souboru to je tooa nahrané tooDropbox sdílení souborů a potom odeslat e-mail.

## <a name="prerequisites"></a>Požadavky

- Instalace a konfigurace hello nejnovější [místní brána dat](https://www.microsoft.com/download/details.aspx?id=53127).
- Nainstalujte hello nejnovější místní bránu data gateway verze 1.15.6150.1 nebo vyšší. [Připojení brány dat místní toohello](http://aka.ms/logicapps-gateway) seznamy hello kroky. Hello brány musí být nainstalován v místním počítači, než budete pokračovat s hello zbytek hello kroky.

## <a name="add-trigger-and-actions-for-connecting-tooyour-file-system"></a>Přidání aktivační události a akcí pro připojení tooyour systému souborů

1. Vytvoření aplikace logiky a přidejte této aktivační události Dropbox: **vytvoření souboru** 
2. V části hello aktivační událost, zvolte **další krok** > **přidat akci**. 
3. Hello vyhledávacího pole zadejte `file system` , můžete zobrazit všechna podporovaná akce pro konektor hello systému souborů.

   ![Vyhledejte soubor konektoru](media/logic-apps-using-file-connector/search-file-connector.png)

2. Zvolte hello **vytvořit soubor** akce a vytvořit systém souborů tooyour připojení.

   Pokud nemáte existující připojení, jste výzvami toocreate jeden.

   1. Zvolte **připojit prostřednictvím místní brána dat**. Zobrazí se další vlastnosti.
   2. Vyberte kořenové složce systému souborů.
      
       > [!NOTE]
       > Hello kořenové složky je hello hlavní nadřazené složky, která se používá k relativní cesty pro všechny akce související s souboru. Můžete zadat do místní složky v počítači hello kde hello místní data brána je nainstalovaná, nebo složku hello může být, že sdílená síťová složka hello počítače přístup.

   3. Zadejte hello uživatelské jméno a heslo pro bránu hello.
   4. Vyberte hello brány, kterou jste dříve nainstalovali.

       ![Konfigurace připojení](media/logic-apps-using-file-connector/create-file.png)

3. Po zadání všech podrobností o hello zvolte **vytvořit**. 

   Služba Logic Apps nakonfiguruje a otestuje připojení, a ujistěte se, zda text hello připojení funguje správně. 
   Pokud připojení hello je správně nastavena, zobrazí možnosti pro hello akci, kterou jste dříve vybrali. 
   konektor systému souboru Hello je nyní připravena k použití.

4. Určete, má toocopy soubory z Dropbox toohello kořenové složky pro vaše místní sdílené složky.

   ![Vytvoření souboru akce](media/logic-apps-using-file-connector/create-file-filled.png)

5. Po souboru logiku aplikace kopie hello přidáte akci Outlook, která odešle e-mail, tak příslušné uživatele vědět o hello nový soubor. Zadejte hello příjemce, název a text e-mailů hello. 

   V hello dynamického obsahu pro výběr můžete data výstupy z hello souboru konektoru, můžete přidat e-mailu toohello další podrobnosti.

   ![Odesílání e-mailu akce](media/logic-apps-using-file-connector/send-email.png)

6. Uložte svou aplikaci logiky. Testování aplikace s tím, že nahrajete soubor tooDropbox. Hello soubor by měl získat zkopírovaný toohello místní sdílené složky a měli byste obdržet e-mailu o operaci hello.

   > [!TIP] 
   > Zjistěte, jak příliš[sledování funkce logic apps](../logic-apps/logic-apps-monitor-your-logic-apps.md).

Blahopřejeme, nyní máte aplikaci logiky práci, kterou můžete připojit tooyour v místním systému souborů. Zkuste zkoumat další funkce, které hello konektor nabízí, třeba:

- Vytvoření souboru
- Zobrazit seznam souborů ve složce
- Připojení souboru
- Odstranit soubor
- Získat obsah souboru
- Získat obsah souboru pomocí cesty
- Získat metadata souboru
- Získat metadata souboru pomocí cesty
- Zobrazit seznam souborů v kořenové složce
- Soubor aktualizace

## <a name="view-hello-swagger"></a>Zobrazení hello swagger
V tématu hello [swagger podrobnosti](/connectors/fileconnector/). 

## <a name="get-help"></a>Podpora

tooask otázky, odpovědi na otázky a zjistěte, jaké další Azure Logic Apps uživatelé dělají, navštivte hello [fórum Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

toohelp zlepšení Azure Logic Apps a konektory, hlasovat o nebo odeslání nápadů v hello [web pro zasílání názorů uživatele Azure Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Další kroky

- [Připojení místní tooon dat](../logic-apps/logic-apps-gateway-connection.md) z aplikace logiky
- Další informace o [integrace enterprise](../logic-apps/logic-apps-enterprise-integration-overview.md)
