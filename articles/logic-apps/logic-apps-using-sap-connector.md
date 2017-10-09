---
title: "aaaConnect tooan místní systém SAP v Azure Logic Apps | Microsoft Docs"
description: "Připojení systému SAP místní tooan z pracovní postup aplikace logiky prostřednictvím brány data místně hello"
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
ms.openlocfilehash: 594ec5fed337398bf931d396684630ee9f907d2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooan-on-premises-sap-system-from-logic-apps-with-hello-sap-connector"></a><span data-ttu-id="a5067-103">Připojení systému SAP místní tooan z aplikace logiky s konektorem SAP hello</span><span class="sxs-lookup"><span data-stu-id="a5067-103">Connect tooan on-premises SAP system from logic apps with hello SAP connector</span></span> 

<span data-ttu-id="a5067-104">Brána dat místní Hello vám umožňuje toomanage dat a bezpečný přístup k prostředkům, které jsou na místě.</span><span class="sxs-lookup"><span data-stu-id="a5067-104">hello on-premises data gateway enables you toomanage data, and securely access resources that are on-premises.</span></span> <span data-ttu-id="a5067-105">Toto téma ukazuje, jak je možné připojit logiku aplikace tooan v místním SAP systému.</span><span class="sxs-lookup"><span data-stu-id="a5067-105">This topic shows how you can connect logic apps tooan on-premises SAP system.</span></span> <span data-ttu-id="a5067-106">V tomto příkladu aplikace logiky požadavky IDOC přes protokol HTTP a odešle odpověď hello zpět.</span><span class="sxs-lookup"><span data-stu-id="a5067-106">In this example, your logic app requests an IDOC over HTTP and sends hello response back.</span></span>    

> [!NOTE]
> <span data-ttu-id="a5067-107">Aktuální omezení:</span><span class="sxs-lookup"><span data-stu-id="a5067-107">Current limitations:</span></span> 
> - <span data-ttu-id="a5067-108">Aplikace logiky časového limitu, pokud nemáte dokončit všechny kroky potřebné pro hello odpověď v rámci hello [časový limit požadavku](./logic-apps-limits-and-config.md).</span><span class="sxs-lookup"><span data-stu-id="a5067-108">Your logic app times out if all steps required for hello response don't finish within hello [request timeout limit](./logic-apps-limits-and-config.md).</span></span> <span data-ttu-id="a5067-109">V tomto scénáři může získat požadavky blokovány.</span><span class="sxs-lookup"><span data-stu-id="a5067-109">In this scenario, requests might get blocked.</span></span> 
> - <span data-ttu-id="a5067-110">Výběr souborů Hello nezobrazuje všechna dostupná pole hello.</span><span class="sxs-lookup"><span data-stu-id="a5067-110">hello file picker does not display all hello available fields.</span></span> <span data-ttu-id="a5067-111">V tomto scénáři můžete ručně přidat cesty.</span><span class="sxs-lookup"><span data-stu-id="a5067-111">In this scenario, you can manually add paths.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a5067-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a5067-112">Prerequisites</span></span>

- <span data-ttu-id="a5067-113">Instalace a konfigurace hello nejnovější [místní brána dat](https://www.microsoft.com/download/details.aspx?id=53127) verze 1.15.6150.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="a5067-113">Install and configure hello latest [on-premises data gateway](https://www.microsoft.com/download/details.aspx?id=53127) version 1.15.6150.1 or newer.</span></span> <span data-ttu-id="a5067-114">[Jak tooconnect toohello místní brána dat v aplikaci logiky](http://aka.ms/logicapps-gateway) seznamy hello kroky.</span><span class="sxs-lookup"><span data-stu-id="a5067-114">[How tooconnect toohello on-premises data gateway in a logic app](http://aka.ms/logicapps-gateway) lists hello steps.</span></span> <span data-ttu-id="a5067-115">Hello brány musí být nainstalován v místním počítači, abyste mohli pokračovat.</span><span class="sxs-lookup"><span data-stu-id="a5067-115">hello gateway must be installed on an on-premises machine before you can proceed.</span></span>

- <span data-ttu-id="a5067-116">Stažení a instalace hello nejnovější SAP knihovny klienta na hello stejný počítač, kam jste nainstalovali bránu dat hello.</span><span class="sxs-lookup"><span data-stu-id="a5067-116">Download and install hello latest SAP client library on hello same machine where you installed hello data gateway.</span></span> <span data-ttu-id="a5067-117">Použijte některou z následujících verze SAP hello:</span><span class="sxs-lookup"><span data-stu-id="a5067-117">Use any of hello following SAP versions:</span></span> 
    - <span data-ttu-id="a5067-118">Serveru SAP</span><span class="sxs-lookup"><span data-stu-id="a5067-118">SAP Server</span></span>
        - <span data-ttu-id="a5067-119">Jakýkoli Server SAP tuto podporu hello konektor .NET (NCo) 3.0</span><span class="sxs-lookup"><span data-stu-id="a5067-119">Any SAP Server that support hello .NET Connector (NCo) 3.0</span></span>
 
    - <span data-ttu-id="a5067-120">SAP klienta</span><span class="sxs-lookup"><span data-stu-id="a5067-120">SAP Client</span></span>
        - <span data-ttu-id="a5067-121">Konektor .NET SAP (NCo) 3.0</span><span class="sxs-lookup"><span data-stu-id="a5067-121">SAP .NET Connector (NCo) 3.0</span></span>

## <a name="add-triggers-and-actions-for-connecting-tooyour-sap-system"></a><span data-ttu-id="a5067-122">Přidat triggery a akce pro připojení tooyour systému SAP</span><span class="sxs-lookup"><span data-stu-id="a5067-122">Add triggers and actions for connecting tooyour SAP system</span></span>

<span data-ttu-id="a5067-123">konektor SAP Hello má akce, ale není aktivační události.</span><span class="sxs-lookup"><span data-stu-id="a5067-123">hello SAP connector has actions, but not triggers.</span></span> <span data-ttu-id="a5067-124">Ano máme toouse jiné aktivační událost při spuštění hello hello pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="a5067-124">So, we have toouse another trigger at hello start of hello workflow.</span></span> 

1. <span data-ttu-id="a5067-125">Přidat aktivační událost hello požadavků a odpovědí a potom vyberte **nový krok**.</span><span class="sxs-lookup"><span data-stu-id="a5067-125">Add hello Request/Response trigger, and then select **New step**.</span></span>

2. <span data-ttu-id="a5067-126">Vyberte **přidat akci**a potom vyberte hello SAP konektoru zadáním `SAP` v poli vyhledávání hello:</span><span class="sxs-lookup"><span data-stu-id="a5067-126">Select **Add an action**, and then select hello SAP connector by typing `SAP` in hello search field:</span></span>    

     ![Vyberte Server aplikace SAP nebo Server zpráv SAP](media/logic-apps-using-sap-connector/sap-action.png)

3. <span data-ttu-id="a5067-128">Vyberte [ **SAP aplikační Server** ](https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server) nebo [ **Server zpráv SAP**](http://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm), podle vašeho nastavení SAP.</span><span class="sxs-lookup"><span data-stu-id="a5067-128">Select [**SAP Application Server**](https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server) or [**SAP Message Server**](http://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm), based on your SAP setup.</span></span> <span data-ttu-id="a5067-129">Pokud nemáte existující připojení, jste výzvami toocreate jeden.</span><span class="sxs-lookup"><span data-stu-id="a5067-129">If you don't have an existing connection, you are prompted toocreate one.</span></span>

   1. <span data-ttu-id="a5067-130">Vyberte **připojit prostřednictvím místní brána dat**a zadejte podrobnosti hello systému SAP:</span><span class="sxs-lookup"><span data-stu-id="a5067-130">Select **Connect via on-premises data gateway**, and enter hello details for your SAP system:</span></span>   

       ![Přidat připojovací řetězec tooSAP](media/logic-apps-using-sap-connector/picture2.png)  

   2. <span data-ttu-id="a5067-132">V části **brány**, vyberte existující bránu, nebo tooinstall novou bránu, vyberte **instalaci brány**.</span><span class="sxs-lookup"><span data-stu-id="a5067-132">Under **Gateway**, select an existing gateway, or tooinstall a new gateway, select **Install Gateway**.</span></span>

        ![Instalace nové brány](media/logic-apps-using-sap-connector/install-gateway.png)
  
   3. <span data-ttu-id="a5067-134">Po zadání všech podrobností o hello vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a5067-134">After you enter all hello details, select **Create**.</span></span> 
   <span data-ttu-id="a5067-135">Služba Logic Apps nakonfiguruje a otestuje připojení hello, a ujistěte se, zda text hello připojení funguje správně.</span><span class="sxs-lookup"><span data-stu-id="a5067-135">Logic Apps configures and tests hello connection, making sure that hello connection works properly.</span></span>

4. <span data-ttu-id="a5067-136">Zadejte název pro připojení k prostředí SAP.</span><span class="sxs-lookup"><span data-stu-id="a5067-136">Enter a name for your SAP connection.</span></span>

5. <span data-ttu-id="a5067-137">Nyní jsou k dispozici různé možnosti SAP Hello.</span><span class="sxs-lookup"><span data-stu-id="a5067-137">hello different SAP options are now available.</span></span> <span data-ttu-id="a5067-138">toofind vaše IDOC kategorie, vyberte ze seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="a5067-138">toofind your IDOC category, select from hello list.</span></span> <span data-ttu-id="a5067-139">Nebo ručně zadejte v hello cestu a vyberte hello HTTP odpovědi v hello **textu** pole:</span><span class="sxs-lookup"><span data-stu-id="a5067-139">Or manually type in hello path, and select hello HTTP response in hello **body** field:</span></span>

     ![Akce SAP](media/logic-apps-using-sap-connector/picture3.png)

6. <span data-ttu-id="a5067-141">Hello akce pro vytváření pro přidání **odpověď HTTP**.</span><span class="sxs-lookup"><span data-stu-id="a5067-141">Add hello action for creating an **HTTP Response**.</span></span> <span data-ttu-id="a5067-142">zpráva odpovědi Hello by měla být z výstupu SAP hello.</span><span class="sxs-lookup"><span data-stu-id="a5067-142">hello response message should be from hello SAP output.</span></span>

7. <span data-ttu-id="a5067-143">Uložte svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="a5067-143">Save your logic app.</span></span> <span data-ttu-id="a5067-144">Otestujte zasláním IDOC prostřednictvím adresy URL hello HTTP aktivační události.</span><span class="sxs-lookup"><span data-stu-id="a5067-144">Test it by sending an IDOC through hello HTTP trigger URL.</span></span> <span data-ttu-id="a5067-145">Po hello IDOC je odeslána Čekejte na odpověď hello z aplikace logiky hello:</span><span class="sxs-lookup"><span data-stu-id="a5067-145">After hello IDOC is sent, wait for hello response from hello logic app:</span></span>   

     > [!TIP]
     > <span data-ttu-id="a5067-146">Podívat, jak příliš[sledování funkce Logic Apps](../logic-apps/logic-apps-monitor-your-logic-apps.md).</span><span class="sxs-lookup"><span data-stu-id="a5067-146">Check out how too[monitor your Logic Apps](../logic-apps/logic-apps-monitor-your-logic-apps.md).</span></span>

<span data-ttu-id="a5067-147">Nyní, konektor SAP hello je přidali tooyour aplikace logiky, prozkoumejte další funkce:</span><span class="sxs-lookup"><span data-stu-id="a5067-147">Now that hello SAP connector is added tooyour logic app, start exploring other functionalities:</span></span>

- <span data-ttu-id="a5067-148">BAPI</span><span class="sxs-lookup"><span data-stu-id="a5067-148">BAPI</span></span>
- <span data-ttu-id="a5067-149">DOKUMENT RFC</span><span class="sxs-lookup"><span data-stu-id="a5067-149">RFC</span></span>

## <a name="get-help"></a><span data-ttu-id="a5067-150">Podpora</span><span class="sxs-lookup"><span data-stu-id="a5067-150">Get help</span></span>

<span data-ttu-id="a5067-151">tooask otázky, odpovědi na otázky a zjistěte, jaké další Azure Logic Apps uživatelé dělají, navštivte hello [fórum Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span><span class="sxs-lookup"><span data-stu-id="a5067-151">tooask questions, answer questions, and learn what other Azure Logic Apps users are doing, visit hello [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span></span>

<span data-ttu-id="a5067-152">toohelp zlepšení Azure Logic Apps a konektory, hlasovat o nebo odeslání nápadů v hello [web pro zasílání názorů uživatele Azure Logic Apps](http://aka.ms/logicapps-wish).</span><span class="sxs-lookup"><span data-stu-id="a5067-152">toohelp improve Azure Logic Apps and connectors, vote on or submit ideas at hello [Azure Logic Apps user feedback site](http://aka.ms/logicapps-wish).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a5067-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a5067-153">Next steps</span></span>

- <span data-ttu-id="a5067-154">Zjistěte, jak toovalidate, transformace a dalších funkcí jako BizTalk v hello [Enterprise integračního balíčku](../logic-apps/logic-apps-enterprise-integration-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a5067-154">Learn how toovalidate, transform, and other BizTalk-like functions in hello [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md).</span></span> 
- <span data-ttu-id="a5067-155">[Připojení místní tooon dat](../logic-apps/logic-apps-gateway-connection.md) z aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="a5067-155">[Connect tooon-premises data](../logic-apps/logic-apps-gateway-connection.md) from logic apps</span></span>
