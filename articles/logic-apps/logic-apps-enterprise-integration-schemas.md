---
title: "aaaSchemas pro ověření XML - Azure Logic Apps | Microsoft Docs"
description: "Ověření XML – dokumenty s schémata pro Azure Logic Apps a Enterprise integračního balíčku"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: 56c5846c-5d8c-4ad4-9652-60b07aa8fc3b
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 87cf92741e10ff7cccd260f27442909e34928903
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="validate-xml-with-schemas-for-azure-logic-apps-and-hello-enterprise-integration-pack"></a>Ověření XML s schémata pro Azure Logic Apps a hello Enterprise integračního balíčku

Schémata potvrdit, že jsou platné hello XML – dokumenty, které vám a obsahují hello očekávané data v předdefinovaném formátu. Schémata také pomůže ověřit zprávy, které se vyměňují ve scénáři B2B.

## <a name="add-a-schema"></a>Přidat schéma.

1. V hello portálu Azure, vyberte **další služby**.

    ![Portál Azure, "Další služby"](media/logic-apps-enterprise-integration-schemas/overview-11.png)

2. Hello filtru vyhledávacího pole zadejte **integrace**a vyberte **účty pro integraci** ze seznamu výsledků hello.

    ![Pole filtru hledání](media/logic-apps-enterprise-integration-schemas/overview-21.png)

3. Vyberte hello **integrace účet** místo tooadd hello schématu.

    ![Seznam účtů, integrace](media/logic-apps-enterprise-integration-schemas/overview-31.png)

4. Zvolte hello **schémata** dlaždici.

    ![Příklad integrace účet "Schémata"](media/logic-apps-enterprise-integration-schemas/schema-11.png)

### <a name="add-a-schema-file-smaller-than-2-mb"></a>Přidejte soubor schématu, která je menší než 2 MB

1. V hello **schémata** okno, které se otevře (z hello předchozích kroků), zvolte **přidat**.

    ![Schémata okně "Přidat"](media/logic-apps-enterprise-integration-schemas/schema-21.png)

2. Zadejte název pro schéma. Nahrát soubor schématu hello výběrem hello složky ikonu další toohello **schématu** pole. Po dokončení procesu nahrávání hello vyberte **OK**.

    ![Snímek obrazovky "Přidat schéma", se zvýrazněnou "malý soubor"](media/logic-apps-enterprise-integration-schemas/schema-31.png)

### <a name="add-a-schema-file-larger-than-2-mb-up-too8-mb-maximum"></a>Přidejte soubor schématu, která je větší než 2 MB (až too8 MB maximum)

Tyto kroky se liší v závislosti na úroveň přístupu kontejneru objektů blob hello: **veřejné** nebo **žádné anonymní přístup**.

**toodetermine tato úroveň přístupu**

1.  Otevřete **Azure Storage Explorer**. 

2.  V části **kontejnery objektů Blob**, vyberte možnost kontejner objektů blob hello chcete. 

3.  Vyberte **zabezpečení**, **úroveň přístupu**.

Pokud je úroveň přístupu objektu blob hello **veřejné**, postupujte podle těchto kroků.

![Azure Storage Explorer "Kontejnery objektů Blob", "Zabezpečení" a "Veřejná" zvýrazněná](media/logic-apps-enterprise-integration-schemas/blob-public.png)

1. Nahrát účet úložiště tooyour hello schématu a zkopírujte hello identifikátor URI.

    ![Účet úložiště se zvýrazněnou identifikátor URI](media/logic-apps-enterprise-integration-schemas/schema-blob.png)

2. V **přidat schéma**, vyberte **velkých souborů**a zadejte hello URI v hello **obsahu URI** textové pole.

    ![Schémata s tlačítko "Přidat" a "Velkých souborů" zvýrazněná](media/logic-apps-enterprise-integration-schemas/schema-largefile.png)

Pokud je úroveň přístupu objektu blob hello **žádné anonymní přístup**, postupujte podle těchto kroků.

![Azure Storage Explorer "Kontejnery objektů Blob", "Zabezpečení" a "Žádná anonymní přístup" zvýrazněná](media/logic-apps-enterprise-integration-schemas/blob-1.png)

1. Nahrajte účet úložiště tooyour hello schématu.

    ![Účet úložiště](media/logic-apps-enterprise-integration-schemas/blob-3.png)

2. Vygenerujte sdílený přístupový podpis pro hello schématu.

    ![Účet úložiště se zvýrazněnou kartu podpisy sdíleného přístupu](media/logic-apps-enterprise-integration-schemas/blob-2.png)

3. V **přidat schéma**, vyberte **velkých souborů**a zadejte hello sdílený přístupový podpis URI v hello **obsahu URI** textové pole.

    ![Schémata s tlačítko "Přidat" a "Velkých souborů" zvýrazněná](media/logic-apps-enterprise-integration-schemas/schema-largefile.png)

4. V hello **schémata** okno účtu integrace, by se měla zobrazit vaše nově přidané schéma.

    ![Účtu integrace s "Schémat" a zvýrazní nové schéma hello](media/logic-apps-enterprise-integration-schemas/schema-41.png)

## <a name="edit-schemas"></a>Upravit schémat.

1. Zvolte hello **schémata** dlaždici.

2. Po hello **schémata** otevře se okno, vyberte hello schéma, které chcete tooedit.

3. Na hello **schémata** okně zvolte **upravit**.

    ![Okno schémat.](media/logic-apps-enterprise-integration-schemas/edit-12.png)

4. Vyberte hello souboru schématu, které tooedit, a potom vyberte **otevřete**.

    ![Tooedit soubor otevřete schématu](media/logic-apps-enterprise-integration-schemas/edit-31.png)

Azure ukazuje a zprávy, které hello schéma se úspěšně nahrál.

## <a name="delete-schemas"></a>Odstranit schémat.

1. Zvolte hello **schémata** dlaždici.

2. Po hello **schémata** otevře se okno, vyberte hello schématu chcete toodelete.

3. Na hello **schémata** okně zvolte **odstranit**.

    ![Okno schémat.](media/logic-apps-enterprise-integration-schemas/delete-12.png)

4. tooconfirm, které chcete toodelete hello vybraná schématu, zvolte **Ano**.

    ![Potvrzovací zpráva "Odstranit schématu"](media/logic-apps-enterprise-integration-schemas/delete-21.png)

    V hello **schémata** okně hello schématu seznam aktualizuje a už obsahuje hello schéma, které jste odstranili.

    ![Svoji integraci účet s "Schémata" zvýrazněná](media/logic-apps-enterprise-integration-schemas/delete-31.png)

## <a name="next-steps"></a>Další kroky
* [Další informace o hello Enterprise integračního balíčku](logic-apps-enterprise-integration-overview.md "Další informace o hello enterprise integračního balíčku").  

