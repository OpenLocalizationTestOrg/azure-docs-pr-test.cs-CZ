---
title: "Připojit k místní systémy souborů z Azure Logic Apps | Microsoft Docs"
description: "Připojení k místní systémy souborů z vaší aplikace logiky pracovního postupu prostřednictvím místní brána dat a konektor systému souborů"
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
ms.openlocfilehash: f33e7c58103c57e17e4e273caba1ab9b83f0cd2b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="connect-to-on-premises-file-systems-from-logic-apps-with-the-file-system-connector"></a>Připojit k místní systémy souborů z aplikace logiky s konektorem systému souborů

Hybridní cloud připojení je důležitá pro aplikace logiky, takže spravovat data a bezpečný přístup k místním prostředkům, aplikace logiky můžete použít místní brána data. V tomto článku jsme ukazují, jak se připojit k systému souborů místní pomocí základní scénáře: Zkopírujte soubor, který nahrání dat do sdílené složky a potom odeslat e-mail.

## <a name="prerequisites"></a>Požadavky

- Instalace a konfigurace nejnovější [místní brána dat](https://www.microsoft.com/download/details.aspx?id=53127).
- Nainstalujte nejnovější bránu místní data, verzi 1.15.6150.1 nebo vyšší. [Připojení k bráně místní data](http://aka.ms/logicapps-gateway) jsou uvedené kroky. Brány musí být nainstalován v místním počítači, než budete pokračovat s dalšími kroky.

## <a name="add-trigger-and-actions-for-connecting-to-your-file-system"></a>Přidání aktivační události a akcí pro připojení k systému souborů

1. Vytvoření aplikace logiky a přidejte této aktivační události Dropbox: **vytvoření souboru** 
2. V části aktivační událost, zvolte **další krok** > **přidat akci**. 
3. Do vyhledávacího pole zadejte `file system` , můžete zobrazit všechna podporovaná akce pro konektor systému souborů.

   ![Vyhledejte soubor konektoru](media/logic-apps-using-file-connector/search-file-connector.png)

2. Vyberte **vytvořit soubor** akce a vytvořit připojení k systému souborů.

   Pokud nemáte existující připojení, zobrazí se výzva k jeho vytvoření.

   1. Zvolte **připojit prostřednictvím místní brána dat**. Zobrazí se další vlastnosti.
   2. Vyberte kořenové složce systému souborů.
      
       > [!NOTE]
       > Kořenová složka je hlavní nadřazené složky, která se používá k relativní cesty pro všechny akce související s souboru. Můžete zadat do místní složky v počítači, kde je nainstalován bránu dat na místě, nebo složce může být sdílená síťová složka má počítač přístup.

   3. Zadejte uživatelské jméno a heslo pro bránu.
   4. Vyberte bránu, kterou jste dříve nainstalovali.

       ![Konfigurace připojení](media/logic-apps-using-file-connector/create-file.png)

3. Po zadání všechny podrobnosti, vyberte **vytvořit**. 

   Služba Logic Apps nakonfiguruje a otestuje připojení, a ujistěte se, že připojení funguje správně. 
   Pokud je připojení správně nastavena, zobrazí možnosti pro akci, kterou jste dříve vybrali. 
   Konektor systému souborů je nyní připravena k použití.

4. Zadejte, že chcete kopírovat soubory z Dropbox do kořenové složky pro vaše místní sdílená.

   ![Vytvoření souboru akce](media/logic-apps-using-file-connector/create-file-filled.png)

5. Po aplikaci logiky zkopíruje soubor, přidejte Outlook akci, která odešle e-mail, tak příslušné uživatele vědět o nový soubor. Zadejte příjemce, název a text e-mailu. 

   V dynamického obsahu výběr můžete data výstupy z konektoru nástroje souboru, můžete přidat další podrobnosti k e-mailu.

   ![Odesílání e-mailu akce](media/logic-apps-using-file-connector/send-email.png)

6. Uložte svou aplikaci logiky. Testování aplikace s tím, že nahrajete soubor na Dropbox. Soubor by měl získat zkopírován do místní sdílené složky a měli byste obdržet e-mailu o operaci.

   > [!TIP] 
   > Zjistěte, jak [sledování funkce logic apps](../logic-apps/logic-apps-monitor-your-logic-apps.md).

Blahopřejeme, nyní máte pracovní aplikace logiky, která může připojit k systému souborů na místě. Zkuste zkoumat další funkce, které nabízí konektor, například:

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

## <a name="view-the-swagger"></a>Zobrazení swagger
Najdete v článku [swagger podrobnosti](/connectors/fileconnector/). 

## <a name="get-help"></a>Podpora

Klást otázky, odpovídat na ně a poučit se ze zkušeností jiných uživatelů Azure Logic Apps můžete ve [fóru Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

Pokud chcete pomoci při vylepšování Azure Logic Apps a konektorů, hlasujte nebo zanechte své nápady na [webu zpětné vazby uživatelů Azure Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Další kroky

- [Připojení k místním datům](../logic-apps/logic-apps-gateway-connection.md) z aplikace logiky
- Další informace o [integrace enterprise](../logic-apps/logic-apps-enterprise-integration-overview.md)
