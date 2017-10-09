---
title: "zdroje dat aaaAccess místně pro Azure Logic Apps | Microsoft Docs"
description: "Nastaví bránu dat místní hello, tak můžete přistupovat ke zdrojům dat místně z aplikace logiky"
keywords: "přístup k datům na místní, přenos dat, šifrování, zdroje dat"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 6cb4449d-e6b8-4c35-9862-15110ae73e6a
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/13/2017
ms.author: LADocs; dimazaid; estfan
ms.openlocfilehash: 1d3deaac5a095316ce78e224dab0c08559bc2ff2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="access-data-sources-on-premises-from-logic-apps-with-hello-on-premises-data-gateway"></a>Přístup ke zdrojům dat místně z aplikací logiky s bránou data místně hello

zdroje dat tooaccess místně z vašich aplikací logiky nastavit bránu místní data, která aplikace logiky můžete použít se podporované konektory. Hello brány funguje jako mostu, který poskytuje přenos rychlé dat a šifrování mezi datové zdroje na místní a aplikace logiky. Brána Hello předává data z místního zdroje na šifrované kanály prostřednictvím hello Azure Service Bus. Veškerý provoz pochází jako zabezpečené odchozí provoz z agenta brány hello. Další informace o [fungování brány dat hello](logic-apps-gateway-install.md#gateway-cloud-service). 

Hello brána podporuje místní zdroje dat toothese připojení:

*   BizTalk Server 2016
*   DB2  
*   Systém souborů
*   Informix
*   MQ
*   MySQL
*   Oracle Database
*   PostgreSQL
*   Aplikace serveru SAP 
*   Zpráva serveru SAP
*   SharePoint
*   SQL Server
*   Teradata

Tyto kroky ukazují, jak tooset až hello místní brány toowork data s logic apps. Další informace o podporované konektory najdete v tématu [konektory pro Azure Logic Apps](../connectors/apis-list.md). 

Informace o tom, jak toouse hello brány s jinými službami najdete v těchto článcích:

*   [Microsoft Power BI místní brány dat](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
*   [Služba Azure gateway místní dat služby Analysis Services](../analysis-services/analysis-services-gateway.md)
*   [Microsoft Flow místní brány dat](https://flow.microsoft.com/documentation/gateway-manage/)
*   [Microsoft PowerApps místní brány dat](https://powerapps.microsoft.com/tutorials/gateway-management/)

## <a name="requirements"></a>Požadavky

* Musíte mít již [nainstalovali hello bránu dat na místním počítači](logic-apps-gateway-install.md).

* Při přihlášení toohello portál Azure máte toouse hello stejný pracovní nebo školní účet, který byl použit příliš[nainstalovat bránu dat místní hello](logic-apps-gateway-install.md#requirements). Přihlášení musí mít váš účet také toouse předplatného Azure při vytváření prostředku brány v hello portál Azure pro instalaci brány.

* Instalace brány nelze již požadoval podle prostředek Azure brány. Můžete přidružit brány instalace tooonly jedna služba Azure gateway prostředku. Deklarace identity se stane, když vytvoříte prostředek brány hello tak, aby instalace hello není k dispozici pro další prostředky.

## <a name="set-up-hello-data-gateway-connection"></a>Nastavit připojení k bráně data hello

### <a name="1-install-hello-on-premises-data-gateway"></a>1. Nainstalovat bránu dat místní hello

Pokud jste to ještě neudělali, postupujte podle hello [kroky tooinstall hello místní data brána](logic-apps-gateway-install.md). Než budete pokračovat s hello další kroky, ujistěte se, že jste nainstalovali bránu dat hello v místním počítači.

<a name="create-gateway-resource"></a>
### <a name="2-create-an-azure-resource-for-hello-on-premises-data-gateway"></a>2. Vytvořit prostředek služby Azure pro bránu dat místní hello

Po instalaci brány hello v místním počítači, musíte vytvořit vaše brána data gateway jako prostředek v Azure. Tento krok také přidruží vaší brány prostředků ve vašem předplatném Azure.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com "portál Azure"). Zajistěte, aby toouse hello Azure pracovní nebo školní e-mailovou adresu používat tooinstall hello brány.

2. V levé nabídce hello v Azure, zvolte **nový** > **Enterprise integrace** > **místní brána dat** jak je vidět tady:

   ![Najít "místní brána dat"](./media/logic-apps-gateway-connection/find-on-premises-data-gateway.png)

3. Na hello **vytvořit připojení bránu** okno, zadejte tyto údaje toocreate prostředku brány dat:

    * **Název**: Zadejte název pro prostředek brány. 

    * **Předplatné**: Vyberte hello tooassociate předplatného Azure s vaší brány prostředků. 
    Tento odběr musí být hello stejnému předplatnému jako svou aplikaci logiky.
   
      výchozí předplatné Hello je založena na hello účet Azure, který jste použili toosign v.

    * **Skupina prostředků**: Vytvořte skupinu prostředků nebo vyberte existující skupinu prostředků pro nasazení brány prostředku. 
    Skupiny prostředků usnadňují správu souvisejících prostředků Azure jako kolekce.

    * **Umístění**: Azure omezuje toto umístění toohello stejné oblasti, která byla vybrána pro cloudové službě Brána hello během [instalace brány](logic-apps-gateway-install.md). 

      > [!NOTE]
      > Ujistěte se, že umístění prostředku hello brány odpovídá umístění hello brány cloudové služby. Jinak instalace brány nemusí zobrazit v seznamu brány hello nainstalován pro tooselect můžete v dalším kroku hello.
      > 
      > Pro prostředek brány a pro svou aplikaci logiky můžete různých oblastech.

    * **Název instalace**: Pokud vaše instalace brány již není vybrána, vyberte hello brány, kterou jste dříve nainstalovali. 

    Zvolte tooadd hello brány prostředků tooyour řídicí panel Azure **Pin toodashboard**. 
    Až budete hotoví, zvolte **vytvořit**.

    Například:

    ![Zadejte podrobnosti toocreate vaše místní brána data gateway](./media/logic-apps-gateway-connection/createblade.png)

    toofind nebo zobrazení, vaše brána data gateway v každém okamžiku hello hlavní Azure levé nabídce, přejděte příliš **více služeb** > **Enterprise integrace** > **místní Data Brány**.

    ![Přejděte příliš "Další služby", "Enterprise integraci", "Místní Data Gateway"](./media/logic-apps-gateway-connection/find-on-premises-data-gateway-enterprise-integration.png)

<a name="connect-logic-app-gateway"></a>
### <a name="3-connect-your-logic-app-toohello-on-premises-data-gateway"></a>3. Připojte bránu logiku aplikace toohello místní data

Teď, když jste vytvořili prostředku bránu dat a vašeho předplatného Azure přidružené k prostředku, vytvořte připojení mezi bránu dat a hello aplikace logiky.

> [!NOTE]
> Vaše brána umístění připojení musí existovat v hello stejné oblasti jako svou aplikaci logiky, ale můžete použít bránu data gateway, která existuje v jiné oblasti.

1. V hello portálu Azure vytvořte nebo otevřete aplikaci logiky v návrháři aplikace logiky.

2. Přidejte konektor, který podporuje místní připojení, jako je SQL Server.

3. Následující hello pořadí uvedeném, vyberte **připojit prostřednictvím místní brána dat**, zadejte jedinečný název a text hello požadované informace a vyberte hello data gateway prostředků, které chcete toouse. Až budete hotoví, zvolte **vytvořit**.

   > [!TIP]
   > Jedinečný název umožňuje snadno identifikovat toto připojení později, zejména v případě, že vytvoříte více připojení. Pokud je k dispozici, zahrnují také hello domény pro vaše uživatelské jméno. 

   ![Vytvoření připojení mezi brána logiku aplikace a data](./media/logic-apps-gateway-connection/blankconnection.png)

Blahopřejeme, připojení brány je nyní připraven pro vaše toouse aplikace logiky.

## <a name="edit-your-gateway-connection-settings"></a>Upravit nastavení připojení brány

Po vytvoření připojení brány pro svou aplikaci logiky, můžete nastavení hello toolater aktualizací pro dané připojení.

1. připojení brány toofind hello:

   * V okně aplikace logiky hello v části **nástroje pro vývoj**, vyberte **rozhraní API připojení**. 
   
     Hello **rozhraní API připojení** podokně se zobrazují všechna připojení rozhraní API, které jsou přidružené k aplikaci logiky, včetně připojení brány.

     ![Přejděte tooyour aplikace logiky, vyberte položku "API připojení"](./media/logic-apps-gateway-connection/logic-app-find-api-connections.png)

   * Nebo hello hlavní Azure levé nabídce, přejděte příliš **více služeb** > **Web a mobilní služby** > **připojení rozhraní API** pro všechna rozhraní API připojení včetně brány připojení, které jsou spojeny s předplatným Azure. 

   * Nebo hello hlavní Azure levém nabídky, přejděte příliš**všechny prostředky** pro všechna rozhraní API připojení, včetně připojení brány, které jsou spojeny s předplatným Azure.

2. Vyberte připojení brány hello chcete tooview nebo upravit a zvolte **připojení k rozhraní API upravit**.

   > [!TIP]
   > Pokud vaše aktualizace se projeví, zkuste [zastavením a restartováním služby systému Windows hello brány](./logic-apps-gateway-install.md#restart-gateway).

<a name="change-delete-gateway-resource"></a>
## <a name="switch-or-delete-your-on-premises-data-gateway-resource"></a>Přepínač nebo odstranit prostředek brány vaše místní data

toocreate prostředek jinou bránu bránu přidružit jiný prostředek nebo odebrat prostředek hello brány, můžete odstranit prostředek brány hello bez ovlivnění hello instalace brány. 

1. Hello hlavní Azure levé nabídce, přejděte příliš**všechny prostředky**. 
2. Najděte a vyberte prostředek brány vaše data.
3. Zvolte **místní brána dat**a na panelu nástrojů hello prostředků, zvolte **odstranit**.

<a name="faq"></a>
## <a name="frequently-asked-questions"></a>Nejčastější dotazy

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

## <a name="next-steps"></a>Další kroky

* [Zabezpečení aplikací logiky](./logic-apps-securing-a-logic-app.md)
* [Běžných příkladů a scénářů pro logic apps](./logic-apps-examples-and-scenarios.md)
