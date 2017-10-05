---
title: "Přistupovat ke zdrojům dat místně pro Azure Logic Apps | Microsoft Docs"
description: "Nastavit místní brána dat tak můžete přistupovat ke zdrojům dat místně z aplikace logiky"
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
ms.openlocfilehash: 24793b83ca284fe9510fe21bc2d13b0589209d36
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="access-data-sources-on-premises-from-logic-apps-with-the-on-premises-data-gateway"></a>Přístup ke zdrojům dat místně z aplikací logiky s místní brány dat

Chcete-li z vašich logic apps přistupovat ke zdrojům dat místně, nastavte bránu místní data, která aplikace logiky můžete použít se podporované konektory. Brána funguje jako mostu, který poskytuje přenos rychlé dat a šifrování mezi datové zdroje na místní a aplikace logiky. Brána předává data z místního zdroje na šifrované kanály přes Azure Service Bus. Veškerý provoz pochází jako zabezpečené odchozí provoz z agenta brány. Další informace o [fungování bránu dat](logic-apps-gateway-install.md#gateway-cloud-service). 

Brána podporuje připojení k těmto zdrojům dat místně:

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

Tyto kroky ukazují, jak nastavit místní brána dat pro práci s logic apps. Další informace o podporované konektory najdete v tématu [konektory pro Azure Logic Apps](../connectors/apis-list.md). 

Informace o tom, jak používat bránu s jinými službami, najdete v těchto článcích:

*   [Microsoft Power BI místní brány dat](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
*   [Služba Azure gateway místní dat služby Analysis Services](../analysis-services/analysis-services-gateway.md)
*   [Microsoft Flow místní brány dat](https://flow.microsoft.com/documentation/gateway-manage/)
*   [Microsoft PowerApps místní brány dat](https://powerapps.microsoft.com/tutorials/gateway-management/)

## <a name="requirements"></a>Požadavky

* Musíte mít již [nainstalovat bránu dat na místním počítači](logic-apps-gateway-install.md).

* Když se přihlásíte na portál Azure, budete muset použít stejný pracovní nebo školní účet, který se používá ke [nainstalovat bránu dat místní](logic-apps-gateway-install.md#requirements). Předplatné Azure k použití při vytváření prostředku brány na portálu Azure pro instalaci brány musí mít také váš přihlašovací účet.

* Instalace brány nelze již požadoval podle prostředek Azure brány. Instalace brány pouze jeden prostředek, služba Azure gateway můžete přidružit. Deklarace identity se stane, když vytvoříte prostředek brány tak, aby instalace není k dispozici pro další prostředky.

## <a name="set-up-the-data-gateway-connection"></a>Nastavení připojení brány dat

### <a name="1-install-the-on-premises-data-gateway"></a>1. Instalace na místní datovou bránu

Pokud jste to ještě neudělali, postupujte podle kroků [postup nainstalovat bránu dat místní](logic-apps-gateway-install.md). Než budete pokračovat v dalších krocích, ujistěte se, že jste nainstalovali bránu dat na místním počítači.

<a name="create-gateway-resource"></a>
### <a name="2-create-an-azure-resource-for-the-on-premises-data-gateway"></a>2. Vytvořit prostředek služby Azure pro bránu místní data

Po instalaci brány na místním počítači, musíte vytvořit vaše brána data gateway jako prostředek v Azure. Tento krok také přidruží vaší brány prostředků ve vašem předplatném Azure.

1. Přihlaste se na web [Azure Portal](https://portal.azure.com "Azure Portal"). Nezapomeňte použít stejný Azure pracovní nebo školní e-mailovou adresu použitý k instalaci brány.

2. V levé nabídce v Azure, zvolte **nový** > **Enterprise integrace** > **místní brána dat** jak je vidět tady:

   ![Najít "místní brána dat"](./media/logic-apps-gateway-connection/find-on-premises-data-gateway.png)

3. Na **vytvořit připojení bránu** okno, zadejte tyto údaje pro vytvoření prostředku brány dat:

    * **Název**: Zadejte název pro prostředek brány. 

    * **Předplatné**: Vyberte předplatné Azure, které chcete přidružit k prostředku brány. 
    Tento odběr musí být ve stejném předplatném jako svou aplikaci logiky.
   
      Výchozí předplatné je založena na účet Azure, který jste použili k přihlášení.

    * **Skupina prostředků**: Vytvořte skupinu prostředků nebo vyberte existující skupinu prostředků pro nasazení brány prostředku. 
    Skupiny prostředků usnadňují správu souvisejících prostředků Azure jako kolekce.

    * **Umístění**: Azure omezuje toto umístění do stejné oblasti, která byla vybrána pro cloudové službě brány během [instalace brány](logic-apps-gateway-install.md). 

      > [!NOTE]
      > Ujistěte se, že umístění prostředků brány odpovídá umístění brány cloudové služby. Instalace brány, jinak hodnota nemusí zobrazit v seznamu nainstalovaných brány můžete vybrat v dalším kroku.
      > 
      > Pro prostředek brány a pro svou aplikaci logiky můžete různých oblastech.

    * **Název instalace**: Pokud vaše instalace brány již není vybrána, vyberte brány, kterou jste dříve nainstalovali. 

    Chcete-li prostředek brány přidat do řídicího panelu Azure, zvolte **připnout na řídicí panel**. 
    Až budete hotoví, zvolte **vytvořit**.

    Například:

    ![Zadejte podrobnosti vytvořit bránu, místní data](./media/logic-apps-gateway-connection/createblade.png)

    K vyhledání nebo zobrazit vaše brána data gateway kdykoli hlavní Azure levé nabídce přejděte na **více služeb** > **Enterprise integrace** > **místní brány Data Gateways**.

    ![Přejděte na "Další služby", "Enterprise integraci", "místní Data Gateway"](./media/logic-apps-gateway-connection/find-on-premises-data-gateway-enterprise-integration.png)

<a name="connect-logic-app-gateway"></a>
### <a name="3-connect-your-logic-app-to-the-on-premises-data-gateway"></a>3. Připojení aplikace logiky k bráně místní data

Teď, když jste vytvořili prostředku bránu dat a vašeho předplatného Azure přidružené k prostředku, vytvořte připojení mezi svou aplikaci logiky a brána data gateway.

> [!NOTE]
> Vaše umístění připojení brány musí být ve stejné oblasti jako svou aplikaci logiky, ale můžete použít bránu data gateway, která existuje v jiné oblasti.

1. Na portálu Azure vytvořit nebo otevřít v návrháři aplikace logiky aplikace logiky.

2. Přidejte konektor, který podporuje místní připojení, jako je SQL Server.

3. Následující pořadí uvedeném, vyberte **připojit prostřednictvím místní brána dat**, zadejte jedinečný název a požadované informace a vyberte prostředek brány dat, který chcete použít. Až budete hotoví, zvolte **vytvořit**.

   > [!TIP]
   > Jedinečný název umožňuje snadno identifikovat toto připojení později, zejména v případě, že vytvoříte více připojení. Pokud je k dispozici, zahrnují také kvalifikované domény pro vaše uživatelské jméno. 

   ![Vytvoření připojení mezi brána logiku aplikace a data](./media/logic-apps-gateway-connection/blankconnection.png)

Blahopřejeme, připojení brány je nyní připraven pro svou aplikaci logiky používat.

## <a name="edit-your-gateway-connection-settings"></a>Upravit nastavení připojení brány

Po vytvoření připojení brány pro svou aplikaci logiky, můžete chtít později aktualizovat nastavení pro dané připojení.

1. Vyhledání připojení brány:

   * V okně aplikace logiky v rámci **nástroje pro vývoj**, vyberte **rozhraní API připojení**. 
   
     **Rozhraní API připojení** podokně se zobrazují všechna připojení rozhraní API, které jsou přidružené k aplikaci logiky, včetně připojení brány.

     ![Přejděte do aplikace logiky, vyberte položku "API připojení"](./media/logic-apps-gateway-connection/logic-app-find-api-connections.png)

   * Nebo v hlavní Azure levé nabídce, přejděte na **více služeb** > **Web a mobilní služby** > **připojení rozhraní API** pro všechna rozhraní API připojení, včetně připojení brány, které jsou spojeny s předplatným Azure. 

   * Nebo na hlavní Azure nabídce vlevo, přejděte na **všechny prostředky** pro všechna rozhraní API připojení, včetně připojení brány, které jsou spojeny s předplatným Azure.

2. Vyberte připojení brány, který chcete zobrazit nebo upravit a zvolit **připojení k rozhraní API upravit**.

   > [!TIP]
   > Pokud vaše aktualizace se projeví, zkuste [zastavením a restartováním brány služby systému Windows](./logic-apps-gateway-install.md#restart-gateway).

<a name="change-delete-gateway-resource"></a>
## <a name="switch-or-delete-your-on-premises-data-gateway-resource"></a>Přepínač nebo odstranit prostředek brány vaše místní data

Vytvořte prostředek jiné brány, bránu přidružit jiný zdroj nebo odebrat prostředek brány, můžete odstranit prostředek brány bez ovlivnění instalace brány. 

1. Hlavní Azure levé nabídce přejděte na **všechny prostředky**. 
2. Najděte a vyberte prostředek brány vaše data.
3. Zvolte **místní brána dat**a na panelu nástrojů prostředků, zvolte **odstranit**.

<a name="faq"></a>
## <a name="frequently-asked-questions"></a>Nejčastější dotazy

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

## <a name="next-steps"></a>Další kroky

* [Zabezpečení aplikací logiky](./logic-apps-securing-a-logic-app.md)
* [Běžných příkladů a scénářů pro logic apps](./logic-apps-examples-and-scenarios.md)
