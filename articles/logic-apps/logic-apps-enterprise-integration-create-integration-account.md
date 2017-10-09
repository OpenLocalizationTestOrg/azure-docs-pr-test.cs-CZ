---
title: "aaaCreate, odkaz, odstranit nebo přesunout integrace účtu v Azure logic apps | Microsoft Docs"
description: "Jak toocreate integrační účet a propojit jej tooyour logic apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: d3ad9e99-a9ee-477b-81bf-0881e11e632f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/23/2017
ms.author: LADocs; mandia
ms.openlocfilehash: fda6c91723b3e3624ee176df112ba8b6c9800273
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-an-integration-account"></a>Co je integrace účet?

Účet integrace umožňuje aplikacím integrace enterprise toomanage artefaktů, včetně schémat, map, certifikáty, partneři a smlouvy. Všechny integrace aplikace, které vytvoříte používá integraci účet tooaccess tyto schémat, map, certifikáty a tak dále.

## <a name="create-an-integration-account"></a>Vytvoření účtu integrace

1.  Přihlaste se toohello [portál Azure](http://portal.azure.com "portál Azure"). Hello levé nabídce vyberte **další služby**.

    ![Vyberte možnost "Další služby"](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. Hello vyhledávacího pole zadejte "integrace" filtru. V seznamu výsledků hello vyberte **účty pro integraci**.

    ![Filtrovat podle "integraci", vyberte "Účty pro integraci"](./media/logic-apps-enterprise-integration-accounts/account-2.png)  

3. V horní části hello hello stránky, zvolte **přidat**.

    ![Vyberte Přidat](./media/logic-apps-enterprise-integration-accounts/account-3.png)

4. Název vašeho účtu integrace a vyberte hello předplatné Azure, které chcete toouse. Můžete buď vytvořit novou **skupiny prostředků** nebo vyberte existující skupinu prostředků. Vyberte **umístění** pro hostování účtu integrace a **cenová úroveň**. 

    Až budete připraveni, zvolte **vytvořit**.

    ![Zadejte podrobnosti pro váš účet integrace](./media/logic-apps-enterprise-integration-accounts/account-4.png)

    Azure zřídí účtu integrace v hello vybrané umístění, které by se měla dokončit během 1 minuty.

5. Obnovte stránku hello. Zobrazí váš nový účet integrace, které jsou uvedené.

    ![Zobrazí se váš nový účet integrace](./media/logic-apps-enterprise-integration-accounts/account-5.png) 

V dalším kroku propojte hello integrace účet je vytvořený tooyour logiku aplikaci. 

## <a name="link-an-integration-account-tooa-logic-app"></a>Odkaz integrace účet tooa logiku aplikace

toogive aplikace logiky přístup toomaps, schémata, smlouvy a artefaktů ve vašem účtu integrace odkaz hello integrace účet tooyour logiku aplikace.

### <a name="prerequisites"></a>Požadavky

* Účet integrace
* Aplikace logiky

> [!NOTE] 
> Zkontrolujte, zda jsou vaše integrace účet a logiku aplikace v hello *stejného umístění Azure* před zahájením.


1. V hello portálu Azure vyberte svou aplikaci logiky a zkontrolujte umístění svou aplikaci logiky.

    ![Vyberte svou aplikaci logiky, zkontrolujte umístění](./media/logic-apps-enterprise-integration-accounts/linkaccount-1.png)

2. V části **nastavení**, vyberte **integrace účet**.

    ![Vyberte "Účet integrace"](./media/logic-apps-enterprise-integration-accounts/linkaccount-2.png)

3. Z hello **vyberte účet, integrace** seznamu, vyberte hello integrace účtu chcete toolink tooyour logiku aplikace. Vyberte propojení, toofinish **Uložit**.

    ![Vyberte svůj účet integrace](./media/logic-apps-enterprise-integration-accounts/linkaccount-3.png)

    Zobrazí se oznámení, že zobrazuje svoji integraci aplikace logiky propojené tooyour je účet a všechny artefakty ve vašem účtu integrace jsou nyní k dispozici tooyour aplikace logiky.

    ![Aplikace logiky je propojený účet tooyour integrace](./media/logic-apps-enterprise-integration-accounts/linkaccount-5.png)

Teď, když je váš účet integrace aplikace logiky propojené tooyour, můžete použít konektory hello B2B ve vašich logic apps. Některé běžné konektory B2B zahrnují ověření XML a plochý soubor kódování nebo dekódování.  

## <a name="delete-your-integration-account"></a>Odstranění účtu integrace

1. Vyberte **další služby**.

    ![Vyberte možnost "Další služby"](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. Hello vyhledávacího pole zadejte "integrace" filtru. V seznamu výsledků hello vyberte **účty pro integraci**.

    ![Filtrovat podle "integraci", vyberte "Účty pro integraci"](./media/logic-apps-enterprise-integration-accounts/account-2.png)  

3. Vyberte účet hello integrace, které chcete toodelete.

    ![Vyberte účet toodelete integrace](./media/logic-apps-enterprise-integration-accounts/account-5.png)

4. V nabídce hello zvolte **odstranit**.

    ![Zvolte "Odstranit"](./media/logic-apps-enterprise-integration-accounts/delete.png)

5. Potvrzení účtu choice toodelete hello integrace.

## <a name="move-your-integration-account"></a>Přesunutí účtu integrace

toomove tooanother účet integraci Azure předplatné nebo skupinu prostředků, postupujte podle těchto kroků.

> [!IMPORTANT]
> Po přesunutí účet integrace, je nutné aktualizovat všechny skripty toouse hello nové ID prostředku.

1. Vyberte **další služby**.

    ![Vyberte možnost "Další služby"](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. Hello vyhledávacího pole zadejte "integrace" filtru. V seznamu výsledků hello vyberte **účty pro integraci**.

    ![Filtrovat podle "integraci", vyberte "Účty pro integraci"](./media/logic-apps-enterprise-integration-accounts/account-2.png)

3. Vyberte účet hello integrace, které chcete toomove. V části **nastavení**, zvolte **vlastnosti**.

    ![Vyberte účet toomove integrace. V části Nastavení vyberte možnost Vlastnosti](./media/logic-apps-enterprise-integration-accounts/move.png)

5. Změnit skupinu prostředků hello nebo předplatné Azure, který je spojen s vaším účtem integrace.

    ![Vyberte skupiny prostředků změnu nebo změnu předplatného](./media/logic-apps-enterprise-integration-accounts/move-2.png)

## <a name="next-steps"></a>Další kroky
* [Další informace o smlouvy](../logic-apps/logic-apps-enterprise-integration-agreements.md "Další informace o integraci smlouvy enterprise")  

