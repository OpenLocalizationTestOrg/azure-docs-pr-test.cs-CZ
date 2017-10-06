---
title: aaaTransform XML s XSLT mapy - Azure Logic Apps | Microsoft Docs
description: "Přidat, že XSLT mapuje tootransform data XML s Azure Logic Apps a hello Enterprise integračního balíčku"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: 90f5cfc4-46b2-4ef7-8ac4-486bb0e3f289
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 720e6f988b8542136dfcc402c3c463fcfb2f23cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-maps-for-xml-data-transform"></a>Přidat mapy pro transformaci dat XML

Používá integrace Enterprise mapuje data XML tootransform mezi formáty. Mapu je dokument XML, který definuje hello data v dokumentu, který by měl být převedeny do jiného formátu. 

## <a name="why-use-maps"></a>Proč používat mapy?

Předpokládejme, že pravidelně zobrazit B2B objednávky nebo faktury z zákazník, který používá formát YYYMMDD hello mezi daty. Ale ve vaší organizaci, ukládat data ve formátu MMDDYYY hello. Můžete použít mapování příliš*transformace* hello YYYMMDD formát data do hello MMDDYYY před uložením hello objednávky nebo faktury podrobnosti v databázi zákazníků aktivity.

## <a name="how-do-i-create-a-map"></a>Vytvoření mapy

Můžete vytvořit projekty BizTalk integrace s hello [Enterprise integračního balíčku](logic-apps-enterprise-integration-overview.md "Další informace o hello enterprise integračního balíčku") pro Visual Studio 2015. Potom můžete vytvořit soubor mapa integrace pro, který umožňuje vizuálně položky mezi dva soubory schématu XML mapy. Po sestavení tohoto projektu, budete mít k dokumentu XSLT.

## <a name="how-do-i-add-a-map"></a>Jak přidat mapu?

1. V hello portálu Azure, vyberte **Procházet**.

    ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)

2. Hello filtru vyhledávacího pole zadejte **integrace**, pak vyberte **účty pro integraci** ze seznamu výsledků hello.

    ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)

3. Vyberte účet integrace hello místo tooadd hello mapy.

    ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. Vyberte hello **mapy** dlaždici.

    ![](./media/logic-apps-enterprise-integration-maps/map-1.png)

5. Po otevření okna mapy hello zvolte **přidat**.

    ![](./media/logic-apps-enterprise-integration-maps/map-2.png)  

6. Zadejte **název** pro mapu. Mapa hello tooupload souboru, vyberte ikonu složky hello na pravé straně hello hello **mapy** textové pole. Po dokončení procesu nahrávání hello zvolte **OK**.

    ![](./media/logic-apps-enterprise-integration-maps/map-3.png)

7. Po Azure přidá hello mapy tooyour integrace účet, obdržíte zprávu na obrazovce, která uvádí, zda byl přidán souboru mapy, nebo ne. Poté, co se vám tato zpráva, zvolte hello **mapy** dlaždice, aby se dala zobrazit hello nově přidány mapy.

    ![](./media/logic-apps-enterprise-integration-maps/map-4.png)

## <a name="how-do-i-edit-a-map"></a>Jak upravit mapu?

Musíte nahrát nový soubor s hello změny, které chcete mapy. Nejprve si můžete stáhnout hello mapy pro úpravy.

tooupload nové mapu, která nahradí existující mapu hello, postupujte podle těchto kroků.

1. Zvolte hello **mapy** dlaždici.

2. Po otevření okna mapy hello vyberte hello mapy, které chcete tooedit.

3. Na hello **mapy** okně zvolte **aktualizace**.

    ![](./media/logic-apps-enterprise-integration-maps/edit-1.png)

4. V hello pro výběr souborů, vyberte soubor s mapováním hello má tooupload a pak vyberte **otevřete**.

    ![](./media/logic-apps-enterprise-integration-maps/edit-2.png)

## <a name="how-toodelete-a-map"></a>Jak toodelete mapu?

1. Zvolte hello **mapy** dlaždici.

2. Po otevření okna mapy hello vyberte mapu hello chcete toodelete.

3. Zvolte **odstranit**.

    ![](./media/logic-apps-enterprise-integration-maps/delete.png)

4. Potvrďte, že chcete toodelete hello mapy.

    ![](./media/logic-apps-enterprise-integration-maps/delete-confirmation-1.png)

## <a name="next-steps"></a>Další kroky
* [Další informace o hello Enterprise integračního balíčku](logic-apps-enterprise-integration-overview.md "Další informace o Enterprise integračního balíčku")  
* [Další informace o smlouvy](../logic-apps/logic-apps-enterprise-integration-agreements.md "Další informace o integraci smlouvy enterprise")  
* [Další informace o transformací](logic-apps-enterprise-integration-transform.md "Další informace o integraci transformací enterprise")  

