---
title: "aaaGet začít s Microsoft Power BI Embedded"
description: "Power BI Embedded – přidejte do své aplikace business intelligence interaktivní sestavy Power BI."
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 4787cf44-5d1c-4bc3-b3fd-bf396e5c1176
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 03/11/2017
ms.author: asaxton
ms.openlocfilehash: ebb550cb4eba761dde3c23e4dd0314fc885817e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-microsoft-power-bi-embedded"></a>Začínáme s Microsoft Power BI Embedded

**Power BI Embedded** je služba Azure, že umožňuje tooadd vývojáři aplikace interaktivní Power BI sestavy do svých vlastních aplikacích. **Power BI Embedded** funguje s stávající aplikace bez nutnosti změna nebo změna hello způsobem uživatelé přihlásit.

Prostředky pro **Microsoft Power BI Embedded** jsou zřízené prostřednictvím hello [rozhraní API Azure ARM](https://msdn.microsoft.com/library/mt712306.aspx). V takovém případě je prostředek hello zřídíte **kolekce pracovních prostorů Power BI**.

![](media/power-bi-embedded-get-started/introduction.png)

## <a name="create-a-workspace-collection"></a>Vytvoření kolekce pracovních prostorů

A **kolekce pracovních prostorů** je prostředek Azure nejvyšší úrovně hello a kontejner pro hello obsah, který se vloží do vaší aplikace. **Kolekci pracovních prostorů** lze vytvořit dvěma způsoby:

* Ručně pomocí hello portálu Azure
* Programově pomocí rozhraní API Správce Azure Resource arm hello

Projděme hello kroky toobuild **kolekce pracovních prostorů** pomocí hello portálu Azure.

1. Otevřete a přihlaste se k **webu Azure Portal**: [http://portal.azure.com](http://portal.azure.com).
2. Klikněte na tlačítko **+ nový** na horním panelu hello.
   
   ![](media/power-bi-embedded-get-started/create-workspace-1.png)
3. V části **Data + analýzy** klikněte na **Power BI Embedded**.
4. Na hello **okno kolekce pracovních prostorů**, zadejte hello požadované informace. Pokud jde o **cenu**, nahlédněte do [cen za Power BI Embedded](http://go.microsoft.com/fwlink/?LinkID=760527).
   
   ![](media/power-bi-embedded-get-started/create-workspace-2.png)
5. Klikněte na možnost **Vytvořit**.

Hello **kolekce pracovních prostorů** bude trvat několik tooprovision situacích. Po dokončení ověření budete přesměrováni toohello **okno kolekce pracovních prostorů**.

   ![](media/power-bi-embedded-get-started/create-workspace-3.png)

Hello **okno pro vytvoření** obsahuje hello informace, které potřebujete toocall hello rozhraní API, která slouží k vytváření pracovních prostorů a nasazování obsahu toothem.

<a name="view-access-keys"/>

## <a name="view-power-bi-api-access-keys"></a>Zobrazení přístupových klíčů k rozhraním API pro Power BI

Jedním z nejdůležitějších kusy informace potřebné toocall hello Power BI REST API hello jsou hello **přístupové klíče**. Jedná se o hello použité toogenerate **tokeny aplikací** , které jsou používané tooauthenticate vaší žádostí o rozhraní API. tooview vaše **přístupové klíče**, klikněte na tlačítko **přístupové klíče** na hello **okno nastavení**. Další informace o **tokenech aplikace** naleznete v části [Ověřování a autorizace pomocí Power BI Embedded](power-bi-embedded-app-token-flow.md).

   ![](media/power-bi-embedded-get-started/access-keys.png)

Jak si všimnete, máte dva klíče.

   ![](media/power-bi-embedded-get-started/access-keys-2.png)

Zkopírujte tyto klíče a zabezpečeně je uložte do své aplikace. To je velmi důležité, že jste s těmito klíči zacházeli jako heslo, protože budete poskytují přístup k obsahu hello tooall v vaše **kolekce pracovních prostorů**.

Ačkoli jsou uvedeny dva klíče, najednou je zapotřebí pouze jeden z nich. druhý klíč Hello je k dispozici, mohli klíče pravidelně obnovovat bez přerušení služby toohello přístup.

Nyní když máte instanci Power BI pro vaši aplikaci a **přístupové klíče**, můžete do vlastní aplikace naimportovat sestavu. Před zjistíte, jak tooimport a sestavy, hello další část popisuje vytváření tooembed datové sady a sestavy Power BI do aplikace.

## <a name="working-with-workspaces"></a>Práce s pracovními prostory

Po vytvoření vaší kolekce pracovních prostorů, budete potřebovat toocreate pracovní prostor, který bude obsahovat sestavy a datové sady. toocreate pracovního prostoru, budete potřebovat toouse hello [Post Worksapce REST API](https://msdn.microsoft.com/library/azure/mt711503.aspx).

## <a name="create-power-bi-datasets-and-reports-tooembed-into-an-app-using-power-bi-desktop"></a>Vytvoření tooembed datové sady a sestavy Power BI do aplikace pomocí Power BI Desktop

Teď, když jste vytvořili instanci Power BI pro vaši aplikaci a mít **přístupové klíče**, budete potřebovat toocreate hello Power BI datové sady a sestavy, které chcete tooembed. Datové sady a sestavy lze vytvořit pomocí **Power BI Desktopu**. [Power BI Desktop si můžete stáhnout zdarma](https://go.microsoft.com/fwlink/?LinkId=521662). Nebo, začněte tooquickly, si můžete stáhnout hello [prodejní analýzy ukázkový soubor PBIX](http://go.microsoft.com/fwlink/?LinkID=780547).

> [!NOTE]
> Další informace o tom toolearn toouse **Power BI Desktop**, najdete v části [Začínáme s Power BI Desktop](https://powerbi.microsoft.com/guided-learning/powerbi-learning-0-2-get-started-power-bi-desktop).

S **Power BI Desktop**, tooyour zdroj dat připojíte importem kopie dat hello do **Power BI Desktop** nebo připojení přímo toohello datového zdroje pomocí **DirectQuery**.

Tady jsou hello rozdíly mezi použitím **Import** a **DirectQuery**.

| Import | DirectQuery |
| --- | --- |
| Tabulky, sloupce *a data* se naimportují, tedy zkopírují do **Power BI Desktop**. Při práci s vizualizacemi si **Power BI Desktop** dotazem vyžádá kopii dat hello. toosee žádné změny k chybě toohello základní data, musíte aktualizovat, nebo importovat, celou aktuální datovou sadu znovu. |Do **Power BI Desktop** se naimportují, tedy zkopírují pouze *tabulky a sloupce*. Při práci s vizualizacemi si **Power BI Desktop** dotazy hello základní zdroj dat, což znamená, vám vždy zobrazují aktuální data. |

Další informace o připojování zdroje dat tooa najdete v tématu [zdroj dat připojit tooa](power-bi-embedded-connect-datasource.md).

Po uložení práce v **Power BI Desktop** se vytvoří soubor PBIX. Tento soubor obsahuje vaši sestavu. Kromě toho hello při importu dat hello soubor PBIX obsahovat úplnou datovou sadu, nebo pokud používáte **DirectQuery**, hello soubor PBIX obsahuje jen schéma datové sady. Hello soubor PBIX programově nasadíte do pracovního prostoru pomocí hello [rozhraní API pro Power BI Import](https://msdn.microsoft.com/library/mt711504.aspx).

> [!NOTE]
> **Power BI Embedded** má další rozhraní API toochange hello server a databáze, datová sada odkazuje tooand sadu pověření účtu služby, který hello datová sada bude používat tooconnect tooyour databáze. Viz [Post SetAllConnections](https://msdn.microsoft.com/library/mt711505.aspx) a [Patch Gateway Datasource](https://msdn.microsoft.com/library/mt711498.aspx).

## <a name="create-power-bi-datasets-and-reports-using-apis"></a>Vytváření datových sad a sestav Power BI pomocí rozhraní API

### <a name="datsets"></a>Datové sady

Můžete vytvořit datové sady v rámci Power BI Embedded pomocí hello REST API. Potom můžete datovou sadu naplnit daty. To vám umožní toowork s daty bez nutnosti hello z Power BI Desktop. Další informace najdete v tématu [Post Datasets](https://msdn.microsoft.com/library/azure/mt778875.aspx).

### <a name="reports"></a>Reports

Můžete vytvořit sestavy z datové sady přímo do vaší aplikace pomocí hello rozhraní API jazyka JavaScript. Další informace najdete v tématu s popisem [vytvoření nové sestavy z datové sady v Power BI Embedded](power-bi-embedded-create-report-from-dataset.md).

## <a name="see-also"></a>Viz také

[Začínáme s ukázkou](power-bi-embedded-get-started-sample.md)  
[Ověřování a autorizace v Power BI Embedded](power-bi-embedded-app-token-flow.md)  
[Vložení sestavy](power-bi-embedded-embed-report.md)  
[Vytvoření nové sestavy z datové sady v Power BI Embedded](power-bi-embedded-create-report-from-dataset.md)
[Ukládání sestav](power-bi-embedded-save-reports.md)  
[Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[Vložená ukázka JavaScriptu](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
Chcete se ještě na něco zeptat? [Zkuste hello komunitě Power BI](http://community.powerbi.com/)

