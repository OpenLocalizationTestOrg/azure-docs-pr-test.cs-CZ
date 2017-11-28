---
title: "Připojit do systému SAP místně v Azure Logic Apps | Microsoft Docs"
description: "Připojit do systému SAP místní z pracovní postup aplikace logiky přes bránu místní data"
services: logic-apps
author: padmavc
manager: anneta
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/01/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 3fea93f558d5a4ef62550fd1f6486903cb812930
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="connect-to-an-on-premises-sap-system-from-logic-apps-with-the-sap-connector"></a><span data-ttu-id="22030-103">Připojit do systému SAP místní z aplikace logiky s konektorem SAP</span><span class="sxs-lookup"><span data-stu-id="22030-103">Connect to an on-premises SAP system from logic apps with the SAP connector</span></span> 

<span data-ttu-id="22030-104">Místní brána dat umožňuje spravovat data a bezpečný přístup k prostředkům, které jsou na místě.</span><span class="sxs-lookup"><span data-stu-id="22030-104">The on-premises data gateway enables you to manage data, and securely access resources that are on-premises.</span></span> <span data-ttu-id="22030-105">Toto téma ukazuje, jak připojit aplikace logiky do místního systému SAP.</span><span class="sxs-lookup"><span data-stu-id="22030-105">This topic shows how you can connect logic apps to an on-premises SAP system.</span></span> <span data-ttu-id="22030-106">V tomto příkladu aplikace logiky požadavky IDOC přes protokol HTTP a odešle odpověď zpět.</span><span class="sxs-lookup"><span data-stu-id="22030-106">In this example, your logic app requests an IDOC over HTTP and sends the response back.</span></span>    

> [!NOTE]
> <span data-ttu-id="22030-107">Aktuální omezení:</span><span class="sxs-lookup"><span data-stu-id="22030-107">Current limitations:</span></span> 
> - <span data-ttu-id="22030-108">Aplikace logiky časového limitu, pokud nemáte dokončit všechny kroky potřebné pro odpověď v rámci [časový limit požadavku](./logic-apps-limits-and-config.md).</span><span class="sxs-lookup"><span data-stu-id="22030-108">Your logic app times out if all steps required for the response don't finish within the [request timeout limit](./logic-apps-limits-and-config.md).</span></span> <span data-ttu-id="22030-109">V tomto scénáři může získat požadavky blokovány.</span><span class="sxs-lookup"><span data-stu-id="22030-109">In this scenario, requests might get blocked.</span></span> 
> - <span data-ttu-id="22030-110">Nástroje pro výběr souborů nezobrazuje všechna dostupná pole.</span><span class="sxs-lookup"><span data-stu-id="22030-110">The file picker does not display all the available fields.</span></span> <span data-ttu-id="22030-111">V tomto scénáři můžete ručně přidat cesty.</span><span class="sxs-lookup"><span data-stu-id="22030-111">In this scenario, you can manually add paths.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="22030-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="22030-112">Prerequisites</span></span>

- <span data-ttu-id="22030-113">Instalace a konfigurace nejnovější [místní brána dat](https://www.microsoft.com/download/details.aspx?id=53127) verze 1.15.6150.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="22030-113">Install and configure the latest [on-premises data gateway](https://www.microsoft.com/download/details.aspx?id=53127) version 1.15.6150.1 or newer.</span></span> <span data-ttu-id="22030-114">[Jak se připojit k bráně místní data v aplikaci logiky](http://aka.ms/logicapps-gateway) jsou uvedené kroky.</span><span class="sxs-lookup"><span data-stu-id="22030-114">[How to connect to the on-premises data gateway in a logic app](http://aka.ms/logicapps-gateway) lists the steps.</span></span> <span data-ttu-id="22030-115">Brány musí být nainstalován v místním počítači, abyste mohli pokračovat.</span><span class="sxs-lookup"><span data-stu-id="22030-115">The gateway must be installed on an on-premises machine before you can proceed.</span></span>

- <span data-ttu-id="22030-116">Stáhněte a nainstalujte nejnovější klientské knihovny SAP ve stejném počítači, kam jste nainstalovali bránu data gateway.</span><span class="sxs-lookup"><span data-stu-id="22030-116">Download and install the latest SAP client library on the same machine where you installed the data gateway.</span></span> <span data-ttu-id="22030-117">Použijte některou z následujících verzí SAP:</span><span class="sxs-lookup"><span data-stu-id="22030-117">Use any of the following SAP versions:</span></span> 
    - <span data-ttu-id="22030-118">Serveru SAP</span><span class="sxs-lookup"><span data-stu-id="22030-118">SAP Server</span></span>
        - <span data-ttu-id="22030-119">Libovolný Server SAP, který podporují konektor .NET (NCo) 3.0</span><span class="sxs-lookup"><span data-stu-id="22030-119">Any SAP Server that support the .NET Connector (NCo) 3.0</span></span>
 
    - <span data-ttu-id="22030-120">SAP klienta</span><span class="sxs-lookup"><span data-stu-id="22030-120">SAP Client</span></span>
        - <span data-ttu-id="22030-121">Konektor .NET SAP (NCo) 3.0</span><span class="sxs-lookup"><span data-stu-id="22030-121">SAP .NET Connector (NCo) 3.0</span></span>

## <a name="add-triggers-and-actions-for-connecting-to-your-sap-system"></a><span data-ttu-id="22030-122">Přidat triggery a akce pro připojení k systému SAP</span><span class="sxs-lookup"><span data-stu-id="22030-122">Add triggers and actions for connecting to your SAP system</span></span>

<span data-ttu-id="22030-123">Konektor SAP má akce, ale není aktivační události.</span><span class="sxs-lookup"><span data-stu-id="22030-123">The SAP connector has actions, but not triggers.</span></span> <span data-ttu-id="22030-124">Ano jsme muset použít jiné aktivační událost při spuštění pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="22030-124">So, we have to use another trigger at the start of the workflow.</span></span> 

1. <span data-ttu-id="22030-125">Přidat aktivační událost požadavků a odpovědí a potom vyberte **nový krok**.</span><span class="sxs-lookup"><span data-stu-id="22030-125">Add the Request/Response trigger, and then select **New step**.</span></span>

2. <span data-ttu-id="22030-126">Vyberte **přidat akci**a potom vyberte konektor SAP zadáním `SAP` do pole hledání:</span><span class="sxs-lookup"><span data-stu-id="22030-126">Select **Add an action**, and then select the SAP connector by typing `SAP` in the search field:</span></span>    

     ![Vyberte Server aplikace SAP nebo Server zpráv SAP](media/logic-apps-using-sap-connector/sap-action.png)

3. <span data-ttu-id="22030-128">Vyberte [ **SAP aplikační Server** ](https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server) nebo [ **Server zpráv SAP**](http://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm), podle vašeho nastavení SAP.</span><span class="sxs-lookup"><span data-stu-id="22030-128">Select [**SAP Application Server**](https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server) or [**SAP Message Server**](http://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm), based on your SAP setup.</span></span> <span data-ttu-id="22030-129">Pokud nemáte existující připojení, zobrazí se výzva k jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="22030-129">If you don't have an existing connection, you are prompted to create one.</span></span>

   1. <span data-ttu-id="22030-130">Vyberte **připojit prostřednictvím místní brána dat**a zadejte podrobnosti pro váš systém SAP:</span><span class="sxs-lookup"><span data-stu-id="22030-130">Select **Connect via on-premises data gateway**, and enter the details for your SAP system:</span></span>   

       ![Přidat připojovací řetězec k SAP](media/logic-apps-using-sap-connector/picture2.png)  

   2. <span data-ttu-id="22030-132">V části **brány**vyberte existující bránu, nebo pokud chcete nainstalovat novou bránu, **instalaci brány**.</span><span class="sxs-lookup"><span data-stu-id="22030-132">Under **Gateway**, select an existing gateway, or to install a new gateway, select **Install Gateway**.</span></span>

        ![Instalace nové brány](media/logic-apps-using-sap-connector/install-gateway.png)
  
   3. <span data-ttu-id="22030-134">Když zadáte všechny podrobnosti, vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="22030-134">After you enter all the details, select **Create**.</span></span> 
   <span data-ttu-id="22030-135">Služba Logic Apps nakonfiguruje a otestuje připojení, a ujistěte se, že připojení funguje správně.</span><span class="sxs-lookup"><span data-stu-id="22030-135">Logic Apps configures and tests the connection, making sure that the connection works properly.</span></span>

4. <span data-ttu-id="22030-136">Zadejte název pro připojení k prostředí SAP.</span><span class="sxs-lookup"><span data-stu-id="22030-136">Enter a name for your SAP connection.</span></span>

5. <span data-ttu-id="22030-137">Různé možnosti SAP jsou nyní k dispozici.</span><span class="sxs-lookup"><span data-stu-id="22030-137">The different SAP options are now available.</span></span> <span data-ttu-id="22030-138">Najít váš IDOC kategorie, vyberte ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="22030-138">To find your IDOC category, select from the list.</span></span> <span data-ttu-id="22030-139">Ručně zadejte cestu a vyberte odpověď HTTP v **textu** pole:</span><span class="sxs-lookup"><span data-stu-id="22030-139">Or manually type in the path, and select the HTTP response in the **body** field:</span></span>

     ![Akce SAP](media/logic-apps-using-sap-connector/picture3.png)

6. <span data-ttu-id="22030-141">Akce pro vytváření pro přidání **odpověď HTTP**.</span><span class="sxs-lookup"><span data-stu-id="22030-141">Add the action for creating an **HTTP Response**.</span></span> <span data-ttu-id="22030-142">Zpráva odpovědi musí být z výstupu SAP.</span><span class="sxs-lookup"><span data-stu-id="22030-142">The response message should be from the SAP output.</span></span>

7. <span data-ttu-id="22030-143">Uložte svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="22030-143">Save your logic app.</span></span> <span data-ttu-id="22030-144">Otestujte zasláním IDOC prostřednictvím adresy URL protokolu HTTP aktivační události.</span><span class="sxs-lookup"><span data-stu-id="22030-144">Test it by sending an IDOC through the HTTP trigger URL.</span></span> <span data-ttu-id="22030-145">Po odeslání IDOC Čekejte na odpověď z aplikace logiky:</span><span class="sxs-lookup"><span data-stu-id="22030-145">After the IDOC is sent, wait for the response from the logic app:</span></span>   

     > [!TIP]
     > <span data-ttu-id="22030-146">Podívejte se na postup [sledování funkce Logic Apps](../logic-apps/logic-apps-monitor-your-logic-apps.md).</span><span class="sxs-lookup"><span data-stu-id="22030-146">Check out how to [monitor your Logic Apps](../logic-apps/logic-apps-monitor-your-logic-apps.md).</span></span>

<span data-ttu-id="22030-147">Teď, když je konektor SAP přidal do aplikace logiky, prozkoumejte další funkce:</span><span class="sxs-lookup"><span data-stu-id="22030-147">Now that the SAP connector is added to your logic app, start exploring other functionalities:</span></span>

- <span data-ttu-id="22030-148">BAPI</span><span class="sxs-lookup"><span data-stu-id="22030-148">BAPI</span></span>
- <span data-ttu-id="22030-149">DOKUMENT RFC</span><span class="sxs-lookup"><span data-stu-id="22030-149">RFC</span></span>

## <a name="get-help"></a><span data-ttu-id="22030-150">Podpora</span><span class="sxs-lookup"><span data-stu-id="22030-150">Get help</span></span>

<span data-ttu-id="22030-151">Klást otázky, odpovídat na ně a poučit se ze zkušeností jiných uživatelů Azure Logic Apps můžete ve [fóru Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span><span class="sxs-lookup"><span data-stu-id="22030-151">To ask questions, answer questions, and learn what other Azure Logic Apps users are doing, visit the [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span></span>

<span data-ttu-id="22030-152">Pokud chcete pomoci při vylepšování Azure Logic Apps a konektorů, hlasujte nebo zanechte své nápady na [webu zpětné vazby uživatelů Azure Logic Apps](http://aka.ms/logicapps-wish).</span><span class="sxs-lookup"><span data-stu-id="22030-152">To help improve Azure Logic Apps and connectors, vote on or submit ideas at the [Azure Logic Apps user feedback site](http://aka.ms/logicapps-wish).</span></span>

## <a name="next-steps"></a><span data-ttu-id="22030-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="22030-153">Next steps</span></span>

- <span data-ttu-id="22030-154">Zjistěte, jak chcete ověřit, transformace a dalších funkcí jako BizTalk v [Enterprise integračního balíčku](../logic-apps/logic-apps-enterprise-integration-overview.md).</span><span class="sxs-lookup"><span data-stu-id="22030-154">Learn how to validate, transform, and other BizTalk-like functions in the [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md).</span></span> 
- <span data-ttu-id="22030-155">[Připojení k místním datům](../logic-apps/logic-apps-gateway-connection.md) z aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="22030-155">[Connect to on-premises data](../logic-apps/logic-apps-gateway-connection.md) from logic apps</span></span>
