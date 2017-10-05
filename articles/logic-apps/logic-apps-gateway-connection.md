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
# <a name="access-data-sources-on-premises-from-logic-apps-with-the-on-premises-data-gateway"></a><span data-ttu-id="d31cd-104">Přístup ke zdrojům dat místně z aplikací logiky s místní brány dat</span><span class="sxs-lookup"><span data-stu-id="d31cd-104">Access data sources on premises from logic apps with the on-premises data gateway</span></span>

<span data-ttu-id="d31cd-105">Chcete-li z vašich logic apps přistupovat ke zdrojům dat místně, nastavte bránu místní data, která aplikace logiky můžete použít se podporované konektory.</span><span class="sxs-lookup"><span data-stu-id="d31cd-105">To access data sources on premises from your logic apps, set up an on-premises data gateway that logic apps can use with supported connectors.</span></span> <span data-ttu-id="d31cd-106">Brána funguje jako mostu, který poskytuje přenos rychlé dat a šifrování mezi datové zdroje na místní a aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="d31cd-106">The gateway acts as a bridge that provides quick data transfer and encryption between data sources on premises and your logic apps.</span></span> <span data-ttu-id="d31cd-107">Brána předává data z místního zdroje na šifrované kanály přes Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="d31cd-107">The gateway relays data from on-premises sources on encrypted channels through the Azure Service Bus.</span></span> <span data-ttu-id="d31cd-108">Veškerý provoz pochází jako zabezpečené odchozí provoz z agenta brány.</span><span class="sxs-lookup"><span data-stu-id="d31cd-108">All traffic originates as secure outbound traffic from the gateway agent.</span></span> <span data-ttu-id="d31cd-109">Další informace o [fungování bránu dat](logic-apps-gateway-install.md#gateway-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="d31cd-109">Learn more about [how the data gateway works](logic-apps-gateway-install.md#gateway-cloud-service).</span></span> 

<span data-ttu-id="d31cd-110">Brána podporuje připojení k těmto zdrojům dat místně:</span><span class="sxs-lookup"><span data-stu-id="d31cd-110">The gateway supports connections to these data sources on premises:</span></span>

*   <span data-ttu-id="d31cd-111">BizTalk Server 2016</span><span class="sxs-lookup"><span data-stu-id="d31cd-111">BizTalk Server 2016</span></span>
*   <span data-ttu-id="d31cd-112">DB2</span><span class="sxs-lookup"><span data-stu-id="d31cd-112">DB2</span></span>  
*   <span data-ttu-id="d31cd-113">Systém souborů</span><span class="sxs-lookup"><span data-stu-id="d31cd-113">File System</span></span>
*   <span data-ttu-id="d31cd-114">Informix</span><span class="sxs-lookup"><span data-stu-id="d31cd-114">Informix</span></span>
*   <span data-ttu-id="d31cd-115">MQ</span><span class="sxs-lookup"><span data-stu-id="d31cd-115">MQ</span></span>
*   <span data-ttu-id="d31cd-116">MySQL</span><span class="sxs-lookup"><span data-stu-id="d31cd-116">MySQL</span></span>
*   <span data-ttu-id="d31cd-117">Oracle Database</span><span class="sxs-lookup"><span data-stu-id="d31cd-117">Oracle Database</span></span>
*   <span data-ttu-id="d31cd-118">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="d31cd-118">PostgreSQL</span></span>
*   <span data-ttu-id="d31cd-119">Aplikace serveru SAP</span><span class="sxs-lookup"><span data-stu-id="d31cd-119">SAP Application Server</span></span> 
*   <span data-ttu-id="d31cd-120">Zpráva serveru SAP</span><span class="sxs-lookup"><span data-stu-id="d31cd-120">SAP Message Server</span></span>
*   <span data-ttu-id="d31cd-121">SharePoint</span><span class="sxs-lookup"><span data-stu-id="d31cd-121">SharePoint</span></span>
*   <span data-ttu-id="d31cd-122">SQL Server</span><span class="sxs-lookup"><span data-stu-id="d31cd-122">SQL Server</span></span>
*   <span data-ttu-id="d31cd-123">Teradata</span><span class="sxs-lookup"><span data-stu-id="d31cd-123">Teradata</span></span>

<span data-ttu-id="d31cd-124">Tyto kroky ukazují, jak nastavit místní brána dat pro práci s logic apps.</span><span class="sxs-lookup"><span data-stu-id="d31cd-124">These steps show how to set up the on-premises data gateway to work with your logic apps.</span></span> <span data-ttu-id="d31cd-125">Další informace o podporované konektory najdete v tématu [konektory pro Azure Logic Apps](../connectors/apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="d31cd-125">For more information about supported connectors, see [Connectors for Azure Logic Apps](../connectors/apis-list.md).</span></span> 

<span data-ttu-id="d31cd-126">Informace o tom, jak používat bránu s jinými službami, najdete v těchto článcích:</span><span class="sxs-lookup"><span data-stu-id="d31cd-126">For information about how to use the gateway with other services, see these articles:</span></span>

*   [<span data-ttu-id="d31cd-127">Microsoft Power BI místní brány dat</span><span class="sxs-lookup"><span data-stu-id="d31cd-127">Microsoft Power BI on-premises data gateway</span></span>](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
*   [<span data-ttu-id="d31cd-128">Služba Azure gateway místní dat služby Analysis Services</span><span class="sxs-lookup"><span data-stu-id="d31cd-128">Azure Analysis Services on-premises data gateway</span></span>](../analysis-services/analysis-services-gateway.md)
*   [<span data-ttu-id="d31cd-129">Microsoft Flow místní brány dat</span><span class="sxs-lookup"><span data-stu-id="d31cd-129">Microsoft Flow on-premises data gateway</span></span>](https://flow.microsoft.com/documentation/gateway-manage/)
*   [<span data-ttu-id="d31cd-130">Microsoft PowerApps místní brány dat</span><span class="sxs-lookup"><span data-stu-id="d31cd-130">Microsoft PowerApps on-premises data gateway</span></span>](https://powerapps.microsoft.com/tutorials/gateway-management/)

## <a name="requirements"></a><span data-ttu-id="d31cd-131">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d31cd-131">Requirements</span></span>

* <span data-ttu-id="d31cd-132">Musíte mít již [nainstalovat bránu dat na místním počítači](logic-apps-gateway-install.md).</span><span class="sxs-lookup"><span data-stu-id="d31cd-132">You must have already [installed the data gateway on a local computer](logic-apps-gateway-install.md).</span></span>

* <span data-ttu-id="d31cd-133">Když se přihlásíte na portál Azure, budete muset použít stejný pracovní nebo školní účet, který se používá ke [nainstalovat bránu dat místní](logic-apps-gateway-install.md#requirements).</span><span class="sxs-lookup"><span data-stu-id="d31cd-133">When you sign in to the Azure portal, you have to use the same work or school account that was used to [install the on-premises data gateway](logic-apps-gateway-install.md#requirements).</span></span> <span data-ttu-id="d31cd-134">Předplatné Azure k použití při vytváření prostředku brány na portálu Azure pro instalaci brány musí mít také váš přihlašovací účet.</span><span class="sxs-lookup"><span data-stu-id="d31cd-134">Your sign-in account must also have an Azure subscription to use when you create a gateway resource in the Azure portal for your gateway installation.</span></span>

* <span data-ttu-id="d31cd-135">Instalace brány nelze již požadoval podle prostředek Azure brány.</span><span class="sxs-lookup"><span data-stu-id="d31cd-135">Your gateway installation can't already be claimed by an Azure gateway resource.</span></span> <span data-ttu-id="d31cd-136">Instalace brány pouze jeden prostředek, služba Azure gateway můžete přidružit.</span><span class="sxs-lookup"><span data-stu-id="d31cd-136">You can associate your gateway installation to only one Azure gateway resource.</span></span> <span data-ttu-id="d31cd-137">Deklarace identity se stane, když vytvoříte prostředek brány tak, aby instalace není k dispozici pro další prostředky.</span><span class="sxs-lookup"><span data-stu-id="d31cd-137">Claim happens when you create the gateway resource so that the installation is unavailable for other resources.</span></span>

## <a name="set-up-the-data-gateway-connection"></a><span data-ttu-id="d31cd-138">Nastavení připojení brány dat</span><span class="sxs-lookup"><span data-stu-id="d31cd-138">Set up the data gateway connection</span></span>

### <a name="1-install-the-on-premises-data-gateway"></a><span data-ttu-id="d31cd-139">1. Instalace na místní datovou bránu</span><span class="sxs-lookup"><span data-stu-id="d31cd-139">1. Install the on-premises data gateway</span></span>

<span data-ttu-id="d31cd-140">Pokud jste to ještě neudělali, postupujte podle kroků [postup nainstalovat bránu dat místní](logic-apps-gateway-install.md).</span><span class="sxs-lookup"><span data-stu-id="d31cd-140">If you haven't already, follow the [steps to install the on-premises data gateway](logic-apps-gateway-install.md).</span></span> <span data-ttu-id="d31cd-141">Než budete pokračovat v dalších krocích, ujistěte se, že jste nainstalovali bránu dat na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="d31cd-141">Before you continue with the other steps, make sure that you installed the data gateway on a local computer.</span></span>

<a name="create-gateway-resource"></a>
### <a name="2-create-an-azure-resource-for-the-on-premises-data-gateway"></a><span data-ttu-id="d31cd-142">2. Vytvořit prostředek služby Azure pro bránu místní data</span><span class="sxs-lookup"><span data-stu-id="d31cd-142">2. Create an Azure resource for the on-premises data gateway</span></span>

<span data-ttu-id="d31cd-143">Po instalaci brány na místním počítači, musíte vytvořit vaše brána data gateway jako prostředek v Azure.</span><span class="sxs-lookup"><span data-stu-id="d31cd-143">After you install the gateway on a local computer, you must create your data gateway as a resource in Azure.</span></span> <span data-ttu-id="d31cd-144">Tento krok také přidruží vaší brány prostředků ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="d31cd-144">This step also associates your gateway resource with your Azure subscription.</span></span>

1. <span data-ttu-id="d31cd-145">Přihlaste se na web [Azure Portal](https://portal.azure.com "Azure Portal").</span><span class="sxs-lookup"><span data-stu-id="d31cd-145">Sign in to the [Azure portal](https://portal.azure.com "Azure portal").</span></span> <span data-ttu-id="d31cd-146">Nezapomeňte použít stejný Azure pracovní nebo školní e-mailovou adresu použitý k instalaci brány.</span><span class="sxs-lookup"><span data-stu-id="d31cd-146">Make sure to use the same Azure work or school email address used to install the gateway.</span></span>

2. <span data-ttu-id="d31cd-147">V levé nabídce v Azure, zvolte **nový** > **Enterprise integrace** > **místní brána dat** jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="d31cd-147">On the left menu in Azure, choose **New** > **Enterprise Integration** > **On-premises data gateway** as shown here:</span></span>

   ![Najít "místní brána dat"](./media/logic-apps-gateway-connection/find-on-premises-data-gateway.png)

3. <span data-ttu-id="d31cd-149">Na **vytvořit připojení bránu** okno, zadejte tyto údaje pro vytvoření prostředku brány dat:</span><span class="sxs-lookup"><span data-stu-id="d31cd-149">On the **Create connection gateway** blade, provide these details to create your data gateway resource:</span></span>

    * <span data-ttu-id="d31cd-150">**Název**: Zadejte název pro prostředek brány.</span><span class="sxs-lookup"><span data-stu-id="d31cd-150">**Name**: Enter a name for your gateway resource.</span></span> 

    * <span data-ttu-id="d31cd-151">**Předplatné**: Vyberte předplatné Azure, které chcete přidružit k prostředku brány.</span><span class="sxs-lookup"><span data-stu-id="d31cd-151">**Subscription**: Select the Azure subscription to associate with your gateway resource.</span></span> 
    <span data-ttu-id="d31cd-152">Tento odběr musí být ve stejném předplatném jako svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="d31cd-152">This subscription should be the same subscription as your logic app.</span></span>
   
      <span data-ttu-id="d31cd-153">Výchozí předplatné je založena na účet Azure, který jste použili k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="d31cd-153">The default subscription is based on the Azure account that you used to sign in.</span></span>

    * <span data-ttu-id="d31cd-154">**Skupina prostředků**: Vytvořte skupinu prostředků nebo vyberte existující skupinu prostředků pro nasazení brány prostředku.</span><span class="sxs-lookup"><span data-stu-id="d31cd-154">**Resource group**: Create a resource group or select an existing resource group for deploying your gateway resource.</span></span> 
    <span data-ttu-id="d31cd-155">Skupiny prostředků usnadňují správu souvisejících prostředků Azure jako kolekce.</span><span class="sxs-lookup"><span data-stu-id="d31cd-155">Resource groups help you manage related Azure assets as a collection.</span></span>

    * <span data-ttu-id="d31cd-156">**Umístění**: Azure omezuje toto umístění do stejné oblasti, která byla vybrána pro cloudové službě brány během [instalace brány](logic-apps-gateway-install.md).</span><span class="sxs-lookup"><span data-stu-id="d31cd-156">**Location**: Azure restricts this location to the same region that was selected for the gateway cloud service during [gateway installation](logic-apps-gateway-install.md).</span></span> 

      > [!NOTE]
      > <span data-ttu-id="d31cd-157">Ujistěte se, že umístění prostředků brány odpovídá umístění brány cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="d31cd-157">Make sure that the gateway resource location matches the gateway cloud service location.</span></span> <span data-ttu-id="d31cd-158">Instalace brány, jinak hodnota nemusí zobrazit v seznamu nainstalovaných brány můžete vybrat v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="d31cd-158">Otherwise, your gateway installation might not appear in the installed gateways list for you to select in the next step.</span></span>
      > 
      > <span data-ttu-id="d31cd-159">Pro prostředek brány a pro svou aplikaci logiky můžete různých oblastech.</span><span class="sxs-lookup"><span data-stu-id="d31cd-159">You can use different regions for your gateway resource and for your logic app.</span></span>

    * <span data-ttu-id="d31cd-160">**Název instalace**: Pokud vaše instalace brány již není vybrána, vyberte brány, kterou jste dříve nainstalovali.</span><span class="sxs-lookup"><span data-stu-id="d31cd-160">**Installation Name**: If your gateway installation isn't already selected, select the gateway that you previously installed.</span></span> 

    <span data-ttu-id="d31cd-161">Chcete-li prostředek brány přidat do řídicího panelu Azure, zvolte **připnout na řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="d31cd-161">To add the gateway resource to your Azure dashboard, choose **Pin to dashboard**.</span></span> 
    <span data-ttu-id="d31cd-162">Až budete hotoví, zvolte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d31cd-162">When you're done, choose **Create**.</span></span>

    <span data-ttu-id="d31cd-163">Například:</span><span class="sxs-lookup"><span data-stu-id="d31cd-163">For example:</span></span>

    ![Zadejte podrobnosti vytvořit bránu, místní data](./media/logic-apps-gateway-connection/createblade.png)

    <span data-ttu-id="d31cd-165">K vyhledání nebo zobrazit vaše brána data gateway kdykoli hlavní Azure levé nabídce přejděte na **více služeb** > **Enterprise integrace** > **místní brány Data Gateways**.</span><span class="sxs-lookup"><span data-stu-id="d31cd-165">To find or view your data gateway at any time,  from the main Azure left menu, go to  **More Services** > **Enterprise Integration** > **On-premises Data Gateways**.</span></span>

    ![Přejděte na "Další služby", "Enterprise integraci", "místní Data Gateway"](./media/logic-apps-gateway-connection/find-on-premises-data-gateway-enterprise-integration.png)

<a name="connect-logic-app-gateway"></a>
### <a name="3-connect-your-logic-app-to-the-on-premises-data-gateway"></a><span data-ttu-id="d31cd-167">3. Připojení aplikace logiky k bráně místní data</span><span class="sxs-lookup"><span data-stu-id="d31cd-167">3. Connect your logic app to the on-premises data gateway</span></span>

<span data-ttu-id="d31cd-168">Teď, když jste vytvořili prostředku bránu dat a vašeho předplatného Azure přidružené k prostředku, vytvořte připojení mezi svou aplikaci logiky a brána data gateway.</span><span class="sxs-lookup"><span data-stu-id="d31cd-168">Now that you've created your data gateway resource and associated your Azure subscription with that resource, create a connection between your logic app and the data gateway.</span></span>

> [!NOTE]
> <span data-ttu-id="d31cd-169">Vaše umístění připojení brány musí být ve stejné oblasti jako svou aplikaci logiky, ale můžete použít bránu data gateway, která existuje v jiné oblasti.</span><span class="sxs-lookup"><span data-stu-id="d31cd-169">Your gateway connection location must exist in the same region as your logic app, but you can use a data gateway that exists in a different region.</span></span>

1. <span data-ttu-id="d31cd-170">Na portálu Azure vytvořit nebo otevřít v návrháři aplikace logiky aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="d31cd-170">In the Azure portal, create or open your logic app in Logic App Designer.</span></span>

2. <span data-ttu-id="d31cd-171">Přidejte konektor, který podporuje místní připojení, jako je SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d31cd-171">Add a connector that supports on-premises connections, like SQL Server.</span></span>

3. <span data-ttu-id="d31cd-172">Následující pořadí uvedeném, vyberte **připojit prostřednictvím místní brána dat**, zadejte jedinečný název a požadované informace a vyberte prostředek brány dat, který chcete použít.</span><span class="sxs-lookup"><span data-stu-id="d31cd-172">Following the order shown, select **Connect via on-premises data gateway**, provide a unique connection name and the required information, and select the data gateway resource that you want to use.</span></span> <span data-ttu-id="d31cd-173">Až budete hotoví, zvolte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d31cd-173">When you're done, choose **Create**.</span></span>

   > [!TIP]
   > <span data-ttu-id="d31cd-174">Jedinečný název umožňuje snadno identifikovat toto připojení později, zejména v případě, že vytvoříte více připojení.</span><span class="sxs-lookup"><span data-stu-id="d31cd-174">A unique connection name helps you easily identify that connection later, especially when you create multiple connections.</span></span> <span data-ttu-id="d31cd-175">Pokud je k dispozici, zahrnují také kvalifikované domény pro vaše uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="d31cd-175">If applicable, also include the qualified domain for your username.</span></span> 

   ![Vytvoření připojení mezi brána logiku aplikace a data](./media/logic-apps-gateway-connection/blankconnection.png)

<span data-ttu-id="d31cd-177">Blahopřejeme, připojení brány je nyní připraven pro svou aplikaci logiky používat.</span><span class="sxs-lookup"><span data-stu-id="d31cd-177">Congratulations, your gateway connection is now ready for your logic app to use.</span></span>

## <a name="edit-your-gateway-connection-settings"></a><span data-ttu-id="d31cd-178">Upravit nastavení připojení brány</span><span class="sxs-lookup"><span data-stu-id="d31cd-178">Edit your gateway connection settings</span></span>

<span data-ttu-id="d31cd-179">Po vytvoření připojení brány pro svou aplikaci logiky, můžete chtít později aktualizovat nastavení pro dané připojení.</span><span class="sxs-lookup"><span data-stu-id="d31cd-179">After you create a gateway connection for your logic app, you might want to later update the settings for that specific connection.</span></span>

1. <span data-ttu-id="d31cd-180">Vyhledání připojení brány:</span><span class="sxs-lookup"><span data-stu-id="d31cd-180">To find the gateway connection:</span></span>

   * <span data-ttu-id="d31cd-181">V okně aplikace logiky v rámci **nástroje pro vývoj**, vyberte **rozhraní API připojení**.</span><span class="sxs-lookup"><span data-stu-id="d31cd-181">On the logic app blade, under **Development Tools**, select **API Connections**.</span></span> 
   
     <span data-ttu-id="d31cd-182">**Rozhraní API připojení** podokně se zobrazují všechna připojení rozhraní API, které jsou přidružené k aplikaci logiky, včetně připojení brány.</span><span class="sxs-lookup"><span data-stu-id="d31cd-182">The **API Connections** pane shows all API connections associated with your logic app, including gateway connections.</span></span>

     ![Přejděte do aplikace logiky, vyberte položku "API připojení"](./media/logic-apps-gateway-connection/logic-app-find-api-connections.png)

   * <span data-ttu-id="d31cd-184">Nebo v hlavní Azure levé nabídce, přejděte na **více služeb** > **Web a mobilní služby** > **připojení rozhraní API** pro všechna rozhraní API připojení, včetně připojení brány, které jsou spojeny s předplatným Azure.</span><span class="sxs-lookup"><span data-stu-id="d31cd-184">Or, from the main Azure left menu, go to **More Services** > **Web & Mobile Services** > **API Connections** for all API connections, including gateway connections, that are associated with your Azure subscription.</span></span> 

   * <span data-ttu-id="d31cd-185">Nebo na hlavní Azure nabídce vlevo, přejděte na **všechny prostředky** pro všechna rozhraní API připojení, včetně připojení brány, které jsou spojeny s předplatným Azure.</span><span class="sxs-lookup"><span data-stu-id="d31cd-185">Or, on the main Azure left menu, go to **All resources** for all API connections, including gateway connections, that are associated with your Azure subscription.</span></span>

2. <span data-ttu-id="d31cd-186">Vyberte připojení brány, který chcete zobrazit nebo upravit a zvolit **připojení k rozhraní API upravit**.</span><span class="sxs-lookup"><span data-stu-id="d31cd-186">Select the gateway connection that you want to view or edit, and choose **Edit API connection**.</span></span>

   > [!TIP]
   > <span data-ttu-id="d31cd-187">Pokud vaše aktualizace se projeví, zkuste [zastavením a restartováním brány služby systému Windows](./logic-apps-gateway-install.md#restart-gateway).</span><span class="sxs-lookup"><span data-stu-id="d31cd-187">If your updates don't take effect, try [stopping and restarting the gateway Windows service](./logic-apps-gateway-install.md#restart-gateway).</span></span>

<a name="change-delete-gateway-resource"></a>
## <a name="switch-or-delete-your-on-premises-data-gateway-resource"></a><span data-ttu-id="d31cd-188">Přepínač nebo odstranit prostředek brány vaše místní data</span><span class="sxs-lookup"><span data-stu-id="d31cd-188">Switch or delete your on-premises data gateway resource</span></span>

<span data-ttu-id="d31cd-189">Vytvořte prostředek jiné brány, bránu přidružit jiný zdroj nebo odebrat prostředek brány, můžete odstranit prostředek brány bez ovlivnění instalace brány.</span><span class="sxs-lookup"><span data-stu-id="d31cd-189">To create a different gateway resource, associate your gateway with a different resource, or remove the gateway resource, you can delete the gateway resource without affecting the gateway installation.</span></span> 

1. <span data-ttu-id="d31cd-190">Hlavní Azure levé nabídce přejděte na **všechny prostředky**.</span><span class="sxs-lookup"><span data-stu-id="d31cd-190">From the main Azure left menu, go to **All resources**.</span></span> 
2. <span data-ttu-id="d31cd-191">Najděte a vyberte prostředek brány vaše data.</span><span class="sxs-lookup"><span data-stu-id="d31cd-191">Find and select your data gateway resource.</span></span>
3. <span data-ttu-id="d31cd-192">Zvolte **místní brána dat**a na panelu nástrojů prostředků, zvolte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="d31cd-192">Choose **On-premises Data Gateway**, and on the resource toolbar, choose **Delete**.</span></span>

<a name="faq"></a>
## <a name="frequently-asked-questions"></a><span data-ttu-id="d31cd-193">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="d31cd-193">Frequently asked questions</span></span>

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

## <a name="next-steps"></a><span data-ttu-id="d31cd-194">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d31cd-194">Next steps</span></span>

* [<span data-ttu-id="d31cd-195">Zabezpečení aplikací logiky</span><span class="sxs-lookup"><span data-stu-id="d31cd-195">Secure your logic apps</span></span>](./logic-apps-securing-a-logic-app.md)
* [<span data-ttu-id="d31cd-196">Běžných příkladů a scénářů pro logic apps</span><span class="sxs-lookup"><span data-stu-id="d31cd-196">Common examples and scenarios for logic apps</span></span>](./logic-apps-examples-and-scenarios.md)
