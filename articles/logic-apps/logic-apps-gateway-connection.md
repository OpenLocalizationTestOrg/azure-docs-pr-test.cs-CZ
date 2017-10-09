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
# <a name="access-data-sources-on-premises-from-logic-apps-with-hello-on-premises-data-gateway"></a><span data-ttu-id="34755-104">Přístup ke zdrojům dat místně z aplikací logiky s bránou data místně hello</span><span class="sxs-lookup"><span data-stu-id="34755-104">Access data sources on premises from logic apps with hello on-premises data gateway</span></span>

<span data-ttu-id="34755-105">zdroje dat tooaccess místně z vašich aplikací logiky nastavit bránu místní data, která aplikace logiky můžete použít se podporované konektory.</span><span class="sxs-lookup"><span data-stu-id="34755-105">tooaccess data sources on premises from your logic apps, set up an on-premises data gateway that logic apps can use with supported connectors.</span></span> <span data-ttu-id="34755-106">Hello brány funguje jako mostu, který poskytuje přenos rychlé dat a šifrování mezi datové zdroje na místní a aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="34755-106">hello gateway acts as a bridge that provides quick data transfer and encryption between data sources on premises and your logic apps.</span></span> <span data-ttu-id="34755-107">Brána Hello předává data z místního zdroje na šifrované kanály prostřednictvím hello Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="34755-107">hello gateway relays data from on-premises sources on encrypted channels through hello Azure Service Bus.</span></span> <span data-ttu-id="34755-108">Veškerý provoz pochází jako zabezpečené odchozí provoz z agenta brány hello.</span><span class="sxs-lookup"><span data-stu-id="34755-108">All traffic originates as secure outbound traffic from hello gateway agent.</span></span> <span data-ttu-id="34755-109">Další informace o [fungování brány dat hello](logic-apps-gateway-install.md#gateway-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="34755-109">Learn more about [how hello data gateway works](logic-apps-gateway-install.md#gateway-cloud-service).</span></span> 

<span data-ttu-id="34755-110">Hello brána podporuje místní zdroje dat toothese připojení:</span><span class="sxs-lookup"><span data-stu-id="34755-110">hello gateway supports connections toothese data sources on premises:</span></span>

*   <span data-ttu-id="34755-111">BizTalk Server 2016</span><span class="sxs-lookup"><span data-stu-id="34755-111">BizTalk Server 2016</span></span>
*   <span data-ttu-id="34755-112">DB2</span><span class="sxs-lookup"><span data-stu-id="34755-112">DB2</span></span>  
*   <span data-ttu-id="34755-113">Systém souborů</span><span class="sxs-lookup"><span data-stu-id="34755-113">File System</span></span>
*   <span data-ttu-id="34755-114">Informix</span><span class="sxs-lookup"><span data-stu-id="34755-114">Informix</span></span>
*   <span data-ttu-id="34755-115">MQ</span><span class="sxs-lookup"><span data-stu-id="34755-115">MQ</span></span>
*   <span data-ttu-id="34755-116">MySQL</span><span class="sxs-lookup"><span data-stu-id="34755-116">MySQL</span></span>
*   <span data-ttu-id="34755-117">Oracle Database</span><span class="sxs-lookup"><span data-stu-id="34755-117">Oracle Database</span></span>
*   <span data-ttu-id="34755-118">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="34755-118">PostgreSQL</span></span>
*   <span data-ttu-id="34755-119">Aplikace serveru SAP</span><span class="sxs-lookup"><span data-stu-id="34755-119">SAP Application Server</span></span> 
*   <span data-ttu-id="34755-120">Zpráva serveru SAP</span><span class="sxs-lookup"><span data-stu-id="34755-120">SAP Message Server</span></span>
*   <span data-ttu-id="34755-121">SharePoint</span><span class="sxs-lookup"><span data-stu-id="34755-121">SharePoint</span></span>
*   <span data-ttu-id="34755-122">SQL Server</span><span class="sxs-lookup"><span data-stu-id="34755-122">SQL Server</span></span>
*   <span data-ttu-id="34755-123">Teradata</span><span class="sxs-lookup"><span data-stu-id="34755-123">Teradata</span></span>

<span data-ttu-id="34755-124">Tyto kroky ukazují, jak tooset až hello místní brány toowork data s logic apps.</span><span class="sxs-lookup"><span data-stu-id="34755-124">These steps show how tooset up hello on-premises data gateway toowork with your logic apps.</span></span> <span data-ttu-id="34755-125">Další informace o podporované konektory najdete v tématu [konektory pro Azure Logic Apps](../connectors/apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="34755-125">For more information about supported connectors, see [Connectors for Azure Logic Apps](../connectors/apis-list.md).</span></span> 

<span data-ttu-id="34755-126">Informace o tom, jak toouse hello brány s jinými službami najdete v těchto článcích:</span><span class="sxs-lookup"><span data-stu-id="34755-126">For information about how toouse hello gateway with other services, see these articles:</span></span>

*   [<span data-ttu-id="34755-127">Microsoft Power BI místní brány dat</span><span class="sxs-lookup"><span data-stu-id="34755-127">Microsoft Power BI on-premises data gateway</span></span>](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
*   [<span data-ttu-id="34755-128">Služba Azure gateway místní dat služby Analysis Services</span><span class="sxs-lookup"><span data-stu-id="34755-128">Azure Analysis Services on-premises data gateway</span></span>](../analysis-services/analysis-services-gateway.md)
*   [<span data-ttu-id="34755-129">Microsoft Flow místní brány dat</span><span class="sxs-lookup"><span data-stu-id="34755-129">Microsoft Flow on-premises data gateway</span></span>](https://flow.microsoft.com/documentation/gateway-manage/)
*   [<span data-ttu-id="34755-130">Microsoft PowerApps místní brány dat</span><span class="sxs-lookup"><span data-stu-id="34755-130">Microsoft PowerApps on-premises data gateway</span></span>](https://powerapps.microsoft.com/tutorials/gateway-management/)

## <a name="requirements"></a><span data-ttu-id="34755-131">Požadavky</span><span class="sxs-lookup"><span data-stu-id="34755-131">Requirements</span></span>

* <span data-ttu-id="34755-132">Musíte mít již [nainstalovali hello bránu dat na místním počítači](logic-apps-gateway-install.md).</span><span class="sxs-lookup"><span data-stu-id="34755-132">You must have already [installed hello data gateway on a local computer](logic-apps-gateway-install.md).</span></span>

* <span data-ttu-id="34755-133">Při přihlášení toohello portál Azure máte toouse hello stejný pracovní nebo školní účet, který byl použit příliš[nainstalovat bránu dat místní hello](logic-apps-gateway-install.md#requirements).</span><span class="sxs-lookup"><span data-stu-id="34755-133">When you sign in toohello Azure portal, you have toouse hello same work or school account that was used too[install hello on-premises data gateway](logic-apps-gateway-install.md#requirements).</span></span> <span data-ttu-id="34755-134">Přihlášení musí mít váš účet také toouse předplatného Azure při vytváření prostředku brány v hello portál Azure pro instalaci brány.</span><span class="sxs-lookup"><span data-stu-id="34755-134">Your sign-in account must also have an Azure subscription toouse when you create a gateway resource in hello Azure portal for your gateway installation.</span></span>

* <span data-ttu-id="34755-135">Instalace brány nelze již požadoval podle prostředek Azure brány.</span><span class="sxs-lookup"><span data-stu-id="34755-135">Your gateway installation can't already be claimed by an Azure gateway resource.</span></span> <span data-ttu-id="34755-136">Můžete přidružit brány instalace tooonly jedna služba Azure gateway prostředku.</span><span class="sxs-lookup"><span data-stu-id="34755-136">You can associate your gateway installation tooonly one Azure gateway resource.</span></span> <span data-ttu-id="34755-137">Deklarace identity se stane, když vytvoříte prostředek brány hello tak, aby instalace hello není k dispozici pro další prostředky.</span><span class="sxs-lookup"><span data-stu-id="34755-137">Claim happens when you create hello gateway resource so that hello installation is unavailable for other resources.</span></span>

## <a name="set-up-hello-data-gateway-connection"></a><span data-ttu-id="34755-138">Nastavit připojení k bráně data hello</span><span class="sxs-lookup"><span data-stu-id="34755-138">Set up hello data gateway connection</span></span>

### <a name="1-install-hello-on-premises-data-gateway"></a><span data-ttu-id="34755-139">1. Nainstalovat bránu dat místní hello</span><span class="sxs-lookup"><span data-stu-id="34755-139">1. Install hello on-premises data gateway</span></span>

<span data-ttu-id="34755-140">Pokud jste to ještě neudělali, postupujte podle hello [kroky tooinstall hello místní data brána](logic-apps-gateway-install.md).</span><span class="sxs-lookup"><span data-stu-id="34755-140">If you haven't already, follow hello [steps tooinstall hello on-premises data gateway](logic-apps-gateway-install.md).</span></span> <span data-ttu-id="34755-141">Než budete pokračovat s hello další kroky, ujistěte se, že jste nainstalovali bránu dat hello v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="34755-141">Before you continue with hello other steps, make sure that you installed hello data gateway on a local computer.</span></span>

<a name="create-gateway-resource"></a>
### <a name="2-create-an-azure-resource-for-hello-on-premises-data-gateway"></a><span data-ttu-id="34755-142">2. Vytvořit prostředek služby Azure pro bránu dat místní hello</span><span class="sxs-lookup"><span data-stu-id="34755-142">2. Create an Azure resource for hello on-premises data gateway</span></span>

<span data-ttu-id="34755-143">Po instalaci brány hello v místním počítači, musíte vytvořit vaše brána data gateway jako prostředek v Azure.</span><span class="sxs-lookup"><span data-stu-id="34755-143">After you install hello gateway on a local computer, you must create your data gateway as a resource in Azure.</span></span> <span data-ttu-id="34755-144">Tento krok také přidruží vaší brány prostředků ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="34755-144">This step also associates your gateway resource with your Azure subscription.</span></span>

1. <span data-ttu-id="34755-145">Přihlaste se toohello [portál Azure](https://portal.azure.com "portál Azure").</span><span class="sxs-lookup"><span data-stu-id="34755-145">Sign in toohello [Azure portal](https://portal.azure.com "Azure portal").</span></span> <span data-ttu-id="34755-146">Zajistěte, aby toouse hello Azure pracovní nebo školní e-mailovou adresu používat tooinstall hello brány.</span><span class="sxs-lookup"><span data-stu-id="34755-146">Make sure toouse hello same Azure work or school email address used tooinstall hello gateway.</span></span>

2. <span data-ttu-id="34755-147">V levé nabídce hello v Azure, zvolte **nový** > **Enterprise integrace** > **místní brána dat** jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="34755-147">On hello left menu in Azure, choose **New** > **Enterprise Integration** > **On-premises data gateway** as shown here:</span></span>

   ![Najít "místní brána dat"](./media/logic-apps-gateway-connection/find-on-premises-data-gateway.png)

3. <span data-ttu-id="34755-149">Na hello **vytvořit připojení bránu** okno, zadejte tyto údaje toocreate prostředku brány dat:</span><span class="sxs-lookup"><span data-stu-id="34755-149">On hello **Create connection gateway** blade, provide these details toocreate your data gateway resource:</span></span>

    * <span data-ttu-id="34755-150">**Název**: Zadejte název pro prostředek brány.</span><span class="sxs-lookup"><span data-stu-id="34755-150">**Name**: Enter a name for your gateway resource.</span></span> 

    * <span data-ttu-id="34755-151">**Předplatné**: Vyberte hello tooassociate předplatného Azure s vaší brány prostředků.</span><span class="sxs-lookup"><span data-stu-id="34755-151">**Subscription**: Select hello Azure subscription tooassociate with your gateway resource.</span></span> 
    <span data-ttu-id="34755-152">Tento odběr musí být hello stejnému předplatnému jako svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="34755-152">This subscription should be hello same subscription as your logic app.</span></span>
   
      <span data-ttu-id="34755-153">výchozí předplatné Hello je založena na hello účet Azure, který jste použili toosign v.</span><span class="sxs-lookup"><span data-stu-id="34755-153">hello default subscription is based on hello Azure account that you used toosign in.</span></span>

    * <span data-ttu-id="34755-154">**Skupina prostředků**: Vytvořte skupinu prostředků nebo vyberte existující skupinu prostředků pro nasazení brány prostředku.</span><span class="sxs-lookup"><span data-stu-id="34755-154">**Resource group**: Create a resource group or select an existing resource group for deploying your gateway resource.</span></span> 
    <span data-ttu-id="34755-155">Skupiny prostředků usnadňují správu souvisejících prostředků Azure jako kolekce.</span><span class="sxs-lookup"><span data-stu-id="34755-155">Resource groups help you manage related Azure assets as a collection.</span></span>

    * <span data-ttu-id="34755-156">**Umístění**: Azure omezuje toto umístění toohello stejné oblasti, která byla vybrána pro cloudové službě Brána hello během [instalace brány](logic-apps-gateway-install.md).</span><span class="sxs-lookup"><span data-stu-id="34755-156">**Location**: Azure restricts this location toohello same region that was selected for hello gateway cloud service during [gateway installation](logic-apps-gateway-install.md).</span></span> 

      > [!NOTE]
      > <span data-ttu-id="34755-157">Ujistěte se, že umístění prostředku hello brány odpovídá umístění hello brány cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="34755-157">Make sure that hello gateway resource location matches hello gateway cloud service location.</span></span> <span data-ttu-id="34755-158">Jinak instalace brány nemusí zobrazit v seznamu brány hello nainstalován pro tooselect můžete v dalším kroku hello.</span><span class="sxs-lookup"><span data-stu-id="34755-158">Otherwise, your gateway installation might not appear in hello installed gateways list for you tooselect in hello next step.</span></span>
      > 
      > <span data-ttu-id="34755-159">Pro prostředek brány a pro svou aplikaci logiky můžete různých oblastech.</span><span class="sxs-lookup"><span data-stu-id="34755-159">You can use different regions for your gateway resource and for your logic app.</span></span>

    * <span data-ttu-id="34755-160">**Název instalace**: Pokud vaše instalace brány již není vybrána, vyberte hello brány, kterou jste dříve nainstalovali.</span><span class="sxs-lookup"><span data-stu-id="34755-160">**Installation Name**: If your gateway installation isn't already selected, select hello gateway that you previously installed.</span></span> 

    <span data-ttu-id="34755-161">Zvolte tooadd hello brány prostředků tooyour řídicí panel Azure **Pin toodashboard**.</span><span class="sxs-lookup"><span data-stu-id="34755-161">tooadd hello gateway resource tooyour Azure dashboard, choose **Pin toodashboard**.</span></span> 
    <span data-ttu-id="34755-162">Až budete hotoví, zvolte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="34755-162">When you're done, choose **Create**.</span></span>

    <span data-ttu-id="34755-163">Například:</span><span class="sxs-lookup"><span data-stu-id="34755-163">For example:</span></span>

    ![Zadejte podrobnosti toocreate vaše místní brána data gateway](./media/logic-apps-gateway-connection/createblade.png)

    <span data-ttu-id="34755-165">toofind nebo zobrazení, vaše brána data gateway v každém okamžiku hello hlavní Azure levé nabídce, přejděte příliš **více služeb** > **Enterprise integrace** > **místní Data Brány**.</span><span class="sxs-lookup"><span data-stu-id="34755-165">toofind or view your data gateway at any time,  from hello main Azure left menu, go too  **More Services** > **Enterprise Integration** > **On-premises Data Gateways**.</span></span>

    ![Přejděte příliš "Další služby", "Enterprise integraci", "Místní Data Gateway"](./media/logic-apps-gateway-connection/find-on-premises-data-gateway-enterprise-integration.png)

<a name="connect-logic-app-gateway"></a>
### <a name="3-connect-your-logic-app-toohello-on-premises-data-gateway"></a><span data-ttu-id="34755-167">3. Připojte bránu logiku aplikace toohello místní data</span><span class="sxs-lookup"><span data-stu-id="34755-167">3. Connect your logic app toohello on-premises data gateway</span></span>

<span data-ttu-id="34755-168">Teď, když jste vytvořili prostředku bránu dat a vašeho předplatného Azure přidružené k prostředku, vytvořte připojení mezi bránu dat a hello aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="34755-168">Now that you've created your data gateway resource and associated your Azure subscription with that resource, create a connection between your logic app and hello data gateway.</span></span>

> [!NOTE]
> <span data-ttu-id="34755-169">Vaše brána umístění připojení musí existovat v hello stejné oblasti jako svou aplikaci logiky, ale můžete použít bránu data gateway, která existuje v jiné oblasti.</span><span class="sxs-lookup"><span data-stu-id="34755-169">Your gateway connection location must exist in hello same region as your logic app, but you can use a data gateway that exists in a different region.</span></span>

1. <span data-ttu-id="34755-170">V hello portálu Azure vytvořte nebo otevřete aplikaci logiky v návrháři aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="34755-170">In hello Azure portal, create or open your logic app in Logic App Designer.</span></span>

2. <span data-ttu-id="34755-171">Přidejte konektor, který podporuje místní připojení, jako je SQL Server.</span><span class="sxs-lookup"><span data-stu-id="34755-171">Add a connector that supports on-premises connections, like SQL Server.</span></span>

3. <span data-ttu-id="34755-172">Následující hello pořadí uvedeném, vyberte **připojit prostřednictvím místní brána dat**, zadejte jedinečný název a text hello požadované informace a vyberte hello data gateway prostředků, které chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="34755-172">Following hello order shown, select **Connect via on-premises data gateway**, provide a unique connection name and hello required information, and select hello data gateway resource that you want toouse.</span></span> <span data-ttu-id="34755-173">Až budete hotoví, zvolte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="34755-173">When you're done, choose **Create**.</span></span>

   > [!TIP]
   > <span data-ttu-id="34755-174">Jedinečný název umožňuje snadno identifikovat toto připojení později, zejména v případě, že vytvoříte více připojení.</span><span class="sxs-lookup"><span data-stu-id="34755-174">A unique connection name helps you easily identify that connection later, especially when you create multiple connections.</span></span> <span data-ttu-id="34755-175">Pokud je k dispozici, zahrnují také hello domény pro vaše uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="34755-175">If applicable, also include hello qualified domain for your username.</span></span> 

   ![Vytvoření připojení mezi brána logiku aplikace a data](./media/logic-apps-gateway-connection/blankconnection.png)

<span data-ttu-id="34755-177">Blahopřejeme, připojení brány je nyní připraven pro vaše toouse aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="34755-177">Congratulations, your gateway connection is now ready for your logic app toouse.</span></span>

## <a name="edit-your-gateway-connection-settings"></a><span data-ttu-id="34755-178">Upravit nastavení připojení brány</span><span class="sxs-lookup"><span data-stu-id="34755-178">Edit your gateway connection settings</span></span>

<span data-ttu-id="34755-179">Po vytvoření připojení brány pro svou aplikaci logiky, můžete nastavení hello toolater aktualizací pro dané připojení.</span><span class="sxs-lookup"><span data-stu-id="34755-179">After you create a gateway connection for your logic app, you might want toolater update hello settings for that specific connection.</span></span>

1. <span data-ttu-id="34755-180">připojení brány toofind hello:</span><span class="sxs-lookup"><span data-stu-id="34755-180">toofind hello gateway connection:</span></span>

   * <span data-ttu-id="34755-181">V okně aplikace logiky hello v části **nástroje pro vývoj**, vyberte **rozhraní API připojení**.</span><span class="sxs-lookup"><span data-stu-id="34755-181">On hello logic app blade, under **Development Tools**, select **API Connections**.</span></span> 
   
     <span data-ttu-id="34755-182">Hello **rozhraní API připojení** podokně se zobrazují všechna připojení rozhraní API, které jsou přidružené k aplikaci logiky, včetně připojení brány.</span><span class="sxs-lookup"><span data-stu-id="34755-182">hello **API Connections** pane shows all API connections associated with your logic app, including gateway connections.</span></span>

     ![Přejděte tooyour aplikace logiky, vyberte položku "API připojení"](./media/logic-apps-gateway-connection/logic-app-find-api-connections.png)

   * <span data-ttu-id="34755-184">Nebo hello hlavní Azure levé nabídce, přejděte příliš **více služeb** > **Web a mobilní služby** > **připojení rozhraní API** pro všechna rozhraní API připojení včetně brány připojení, které jsou spojeny s předplatným Azure.</span><span class="sxs-lookup"><span data-stu-id="34755-184">Or, from hello main Azure left menu, go too **More Services** > **Web & Mobile Services** > **API Connections** for all API connections, including gateway connections, that are associated with your Azure subscription.</span></span> 

   * <span data-ttu-id="34755-185">Nebo hello hlavní Azure levém nabídky, přejděte příliš**všechny prostředky** pro všechna rozhraní API připojení, včetně připojení brány, které jsou spojeny s předplatným Azure.</span><span class="sxs-lookup"><span data-stu-id="34755-185">Or, on hello main Azure left menu, go too**All resources** for all API connections, including gateway connections, that are associated with your Azure subscription.</span></span>

2. <span data-ttu-id="34755-186">Vyberte připojení brány hello chcete tooview nebo upravit a zvolte **připojení k rozhraní API upravit**.</span><span class="sxs-lookup"><span data-stu-id="34755-186">Select hello gateway connection that you want tooview or edit, and choose **Edit API connection**.</span></span>

   > [!TIP]
   > <span data-ttu-id="34755-187">Pokud vaše aktualizace se projeví, zkuste [zastavením a restartováním služby systému Windows hello brány](./logic-apps-gateway-install.md#restart-gateway).</span><span class="sxs-lookup"><span data-stu-id="34755-187">If your updates don't take effect, try [stopping and restarting hello gateway Windows service](./logic-apps-gateway-install.md#restart-gateway).</span></span>

<a name="change-delete-gateway-resource"></a>
## <a name="switch-or-delete-your-on-premises-data-gateway-resource"></a><span data-ttu-id="34755-188">Přepínač nebo odstranit prostředek brány vaše místní data</span><span class="sxs-lookup"><span data-stu-id="34755-188">Switch or delete your on-premises data gateway resource</span></span>

<span data-ttu-id="34755-189">toocreate prostředek jinou bránu bránu přidružit jiný prostředek nebo odebrat prostředek hello brány, můžete odstranit prostředek brány hello bez ovlivnění hello instalace brány.</span><span class="sxs-lookup"><span data-stu-id="34755-189">toocreate a different gateway resource, associate your gateway with a different resource, or remove hello gateway resource, you can delete hello gateway resource without affecting hello gateway installation.</span></span> 

1. <span data-ttu-id="34755-190">Hello hlavní Azure levé nabídce, přejděte příliš**všechny prostředky**.</span><span class="sxs-lookup"><span data-stu-id="34755-190">From hello main Azure left menu, go too**All resources**.</span></span> 
2. <span data-ttu-id="34755-191">Najděte a vyberte prostředek brány vaše data.</span><span class="sxs-lookup"><span data-stu-id="34755-191">Find and select your data gateway resource.</span></span>
3. <span data-ttu-id="34755-192">Zvolte **místní brána dat**a na panelu nástrojů hello prostředků, zvolte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="34755-192">Choose **On-premises Data Gateway**, and on hello resource toolbar, choose **Delete**.</span></span>

<a name="faq"></a>
## <a name="frequently-asked-questions"></a><span data-ttu-id="34755-193">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="34755-193">Frequently asked questions</span></span>

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

## <a name="next-steps"></a><span data-ttu-id="34755-194">Další kroky</span><span class="sxs-lookup"><span data-stu-id="34755-194">Next steps</span></span>

* [<span data-ttu-id="34755-195">Zabezpečení aplikací logiky</span><span class="sxs-lookup"><span data-stu-id="34755-195">Secure your logic apps</span></span>](./logic-apps-securing-a-logic-app.md)
* [<span data-ttu-id="34755-196">Běžných příkladů a scénářů pro logic apps</span><span class="sxs-lookup"><span data-stu-id="34755-196">Common examples and scenarios for logic apps</span></span>](./logic-apps-examples-and-scenarios.md)
