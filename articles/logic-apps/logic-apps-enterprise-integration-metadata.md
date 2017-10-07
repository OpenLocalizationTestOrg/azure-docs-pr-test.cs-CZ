---
title: "integrace aaaManage účet artefaktu metadat - Azure Logic Apps | Microsoft Docs"
description: "Přidat nebo načíst metadata artefaktů z účtů integrace pro Azure Logic Apps"
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 11/21/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 8de71bffa9f9975d5409716b2208fa6c3a9545d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-artifact-metadata-in-integration-accounts-for-logic-apps"></a>Spravovat artefaktu metadat v účtech integrace pro logic apps

Můžete definovat vlastní metadata pro artefakty v účty pro integraci a načtení tohoto metadat při běhu pro svou aplikaci logiky. Například můžete zadat metadata pro artefakty jako partnery, smlouvy, schémat a mapy – všechny ukládání metadat pomocí páry klíč hodnota. V současné době artefakty nelze vytvořit metadat prostřednictvím uživatelského rozhraní, ale můžete použít rozhraní REST API toocreate metadat. metadata tooadd při vytváření nebo vyberte partnera, smlouvy nebo schématu v hello portál Azure, zvolte **upravit jako JSON**. tooretrieve artefaktu metadat v aplikace logiky, můžete použít funkce integrace vyhledávání artefaktů účtu hello.

## <a name="add-metadata-tooartifacts-in-integration-accounts"></a>Přidání metadat tooartifacts v účty pro integraci

1. Vytvoření [integrace účet](logic-apps-enterprise-integration-create-integration-account.md).

2. Přidat účet integrace tooyour artefaktů, například, [partnera](logic-apps-enterprise-integration-partners.md#how-to-create-a-partner), [smlouvy](logic-apps-enterprise-integration-agreements.md#how-to-create-agreements), nebo [schématu](logic-apps-enterprise-integration-schemas.md).

3.  Vyberte hello artefaktů, zvolte **upravit jako JSON**a zadejte podrobnosti o metadata.

    ![Zadejte metadat](media/logic-apps-enterprise-integration-metadata/image1.png)

## <a name="retrieve-metadata-from-artifacts-for-logic-apps"></a>Načtení metadat z artefaktů pro logic apps

1. Vytvoření [aplikace logiky](logic-apps-create-a-logic-app.md).

2. Vytvoření [odkaz z vašeho účtu logiku aplikace tooyour integrace](logic-apps-enterprise-integration-create-integration-account.md#link-an-integration-account-to-a-logic-app). 

3. Návrhář aplikace na základě logiky, přidejte aktivační událost jako *požadavku* nebo *HTTP* tooyour aplikace logiky.

4.  Zvolte **další krok** > **přidat akci**. Vyhledejte *integrace* , můžete najít a pak vyberte **integrace účet – integrace vyhledávání artefaktů účtu**.

    ![Vyberte integrační účet artefaktů vyhledávání](media/logic-apps-enterprise-integration-metadata/image2.png)

5. Vyberte hello **typu artefaktu**a zadejte hello **artefaktů název**.

    ![Vyberte typ artefaktů a zadejte název artefaktů](media/logic-apps-enterprise-integration-metadata/image3.png)

## <a name="example-retrieve-partner-metadata"></a>Příklad: Načtení partnera metadat

Metadata partnera má tyto `routingUrl` podrobnosti:

![Vyhledejte "routingURL" metadata partnera.](media/logic-apps-enterprise-integration-metadata/image6.png)

1. V aplikaci logiky přidat aktivační událost, **integrace účet – integrace vyhledávání artefaktů účtu** akce pro váš partner a **HTTP**.

    ![Přidání aktivační událost, artefaktů vyhledávání a aplikace logiky tooyour "HTTP"](media/logic-apps-enterprise-integration-metadata/image4.png)

2. tooretrieve hello identifikátor URI, přejděte tooCode zobrazení pro svou aplikaci logiky. Vaše definici aplikace logiky by měl vypadat jako tento ukázkový:

    ![Hledání vyhledávání](media/logic-apps-enterprise-integration-metadata/image5.png)


## <a name="next-steps"></a>Další kroky
* [Další informace o smlouvy](logic-apps-enterprise-integration-agreements.md "Další informace o integraci smlouvy enterprise")  
