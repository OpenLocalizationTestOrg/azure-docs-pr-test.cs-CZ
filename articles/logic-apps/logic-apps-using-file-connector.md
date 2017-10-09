---
title: "aaaConnect tooon místní systém souborů z Azure Logic Apps | Microsoft Docs"
description: "Připojit místní tooon systémy souborů z pracovní postup aplikace logiky prostřednictvím brány dat hello místní a konektor systému souborů"
keywords: "systémy souborů"
services: logic-apps
author: derek1ee
manager: anneta
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/27/2017
ms.author: LADocs; deli
ms.openlocfilehash: beb5565293def4aba81f63f19e77d7498aac38c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooon-premises-file-systems-from-logic-apps-with-hello-file-system-connector"></a><span data-ttu-id="9ed95-104">Připojit místní tooon systémy souborů z aplikace logiky s konektorem hello systému souborů</span><span class="sxs-lookup"><span data-stu-id="9ed95-104">Connect tooon-premises file systems from logic apps with hello File System connector</span></span>

<span data-ttu-id="9ed95-105">Hybridní cloud připojení je centrální toologic aplikace, takže toomanage data a bezpečný přístup k místním prostředkům, aplikace logiky můžete použít hello místní data gateway.</span><span class="sxs-lookup"><span data-stu-id="9ed95-105">Hybrid cloud connectivity is central toologic apps, so toomanage data and securely access on-premises resources, your logic apps can use hello on-premises data gateway.</span></span> <span data-ttu-id="9ed95-106">V tomto článku ukážeme, jak tooconnect tooan místní systém souborů pomocí základní scénáře: kopírování souboru to je tooa nahrané tooDropbox sdílení souborů a potom odeslat e-mail.</span><span class="sxs-lookup"><span data-stu-id="9ed95-106">In this article, we show how tooconnect tooan on-premises file system with a basic scenario: copy a file that's uploaded tooDropbox tooa file share, then send an email.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9ed95-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9ed95-107">Prerequisites</span></span>

- <span data-ttu-id="9ed95-108">Instalace a konfigurace hello nejnovější [místní brána dat](https://www.microsoft.com/download/details.aspx?id=53127).</span><span class="sxs-lookup"><span data-stu-id="9ed95-108">Install and configure hello latest [on-premises data gateway](https://www.microsoft.com/download/details.aspx?id=53127).</span></span>
- <span data-ttu-id="9ed95-109">Nainstalujte hello nejnovější místní bránu data gateway verze 1.15.6150.1 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="9ed95-109">Install hello latest on-premises data gateway, version 1.15.6150.1 or above.</span></span> <span data-ttu-id="9ed95-110">[Připojení brány dat místní toohello](http://aka.ms/logicapps-gateway) seznamy hello kroky.</span><span class="sxs-lookup"><span data-stu-id="9ed95-110">[Connect toohello on-premises data gateway](http://aka.ms/logicapps-gateway) lists hello steps.</span></span> <span data-ttu-id="9ed95-111">Hello brány musí být nainstalován v místním počítači, než budete pokračovat s hello zbytek hello kroky.</span><span class="sxs-lookup"><span data-stu-id="9ed95-111">hello gateway must be installed on an on-premises machine before you can continue with hello rest of hello steps.</span></span>

## <a name="add-trigger-and-actions-for-connecting-tooyour-file-system"></a><span data-ttu-id="9ed95-112">Přidání aktivační události a akcí pro připojení tooyour systému souborů</span><span class="sxs-lookup"><span data-stu-id="9ed95-112">Add trigger and actions for connecting tooyour file system</span></span>

1. <span data-ttu-id="9ed95-113">Vytvoření aplikace logiky a přidejte této aktivační události Dropbox: **vytvoření souboru**</span><span class="sxs-lookup"><span data-stu-id="9ed95-113">Create a logic app, and add this Dropbox trigger: **When a file is created**</span></span> 
2. <span data-ttu-id="9ed95-114">V části hello aktivační událost, zvolte **další krok** > **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="9ed95-114">Under hello trigger, choose **Next Step** > **Add an action**.</span></span> 
3. <span data-ttu-id="9ed95-115">Hello vyhledávacího pole zadejte `file system` , můžete zobrazit všechna podporovaná akce pro konektor hello systému souborů.</span><span class="sxs-lookup"><span data-stu-id="9ed95-115">In hello search box, enter `file system` so you can view all supported actions for hello File System connector.</span></span>

   ![Vyhledejte soubor konektoru](media/logic-apps-using-file-connector/search-file-connector.png)

2. <span data-ttu-id="9ed95-117">Zvolte hello **vytvořit soubor** akce a vytvořit systém souborů tooyour připojení.</span><span class="sxs-lookup"><span data-stu-id="9ed95-117">Choose hello **Create file** action, and create a connection tooyour file system.</span></span>

   <span data-ttu-id="9ed95-118">Pokud nemáte existující připojení, jste výzvami toocreate jeden.</span><span class="sxs-lookup"><span data-stu-id="9ed95-118">If you don't have an existing connection, you are prompted toocreate one.</span></span>

   1. <span data-ttu-id="9ed95-119">Zvolte **připojit prostřednictvím místní brána dat**.</span><span class="sxs-lookup"><span data-stu-id="9ed95-119">Choose **Connect via on-premises data gateway**.</span></span> <span data-ttu-id="9ed95-120">Zobrazí se další vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="9ed95-120">More properties appear.</span></span>
   2. <span data-ttu-id="9ed95-121">Vyberte kořenové složce systému souborů.</span><span class="sxs-lookup"><span data-stu-id="9ed95-121">Select your root folder for your file system.</span></span>
      
       > [!NOTE]
       > <span data-ttu-id="9ed95-122">Hello kořenové složky je hello hlavní nadřazené složky, která se používá k relativní cesty pro všechny akce související s souboru.</span><span class="sxs-lookup"><span data-stu-id="9ed95-122">hello root folder is hello main parent folder, which is used for relative paths for all file-related actions.</span></span> <span data-ttu-id="9ed95-123">Můžete zadat do místní složky v počítači hello kde hello místní data brána je nainstalovaná, nebo složku hello může být, že sdílená síťová složka hello počítače přístup.</span><span class="sxs-lookup"><span data-stu-id="9ed95-123">You can specify a local folder on hello machine where hello on-premises data gateway is installed, or hello folder can be a network share that hello machine can access.</span></span>

   3. <span data-ttu-id="9ed95-124">Zadejte hello uživatelské jméno a heslo pro bránu hello.</span><span class="sxs-lookup"><span data-stu-id="9ed95-124">Enter hello username and password for hello gateway.</span></span>
   4. <span data-ttu-id="9ed95-125">Vyberte hello brány, kterou jste dříve nainstalovali.</span><span class="sxs-lookup"><span data-stu-id="9ed95-125">Select hello gateway that you previously installed.</span></span>

       ![Konfigurace připojení](media/logic-apps-using-file-connector/create-file.png)

3. <span data-ttu-id="9ed95-127">Po zadání všech podrobností o hello zvolte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9ed95-127">After you provide all hello details, choose **Create**.</span></span> 

   <span data-ttu-id="9ed95-128">Služba Logic Apps nakonfiguruje a otestuje připojení, a ujistěte se, zda text hello připojení funguje správně.</span><span class="sxs-lookup"><span data-stu-id="9ed95-128">Logic Apps configures and tests your connection, making sure that hello connection works properly.</span></span> 
   <span data-ttu-id="9ed95-129">Pokud připojení hello je správně nastavena, zobrazí možnosti pro hello akci, kterou jste dříve vybrali.</span><span class="sxs-lookup"><span data-stu-id="9ed95-129">If hello connection is set up correctly, you see options for hello action that you previously selected.</span></span> 
   <span data-ttu-id="9ed95-130">konektor systému souboru Hello je nyní připravena k použití.</span><span class="sxs-lookup"><span data-stu-id="9ed95-130">hello file system connector is now ready for use.</span></span>

4. <span data-ttu-id="9ed95-131">Určete, má toocopy soubory z Dropbox toohello kořenové složky pro vaše místní sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="9ed95-131">Specify that you want toocopy files from Dropbox toohello root folder for your on-premises file share.</span></span>

   ![Vytvoření souboru akce](media/logic-apps-using-file-connector/create-file-filled.png)

5. <span data-ttu-id="9ed95-133">Po souboru logiku aplikace kopie hello přidáte akci Outlook, která odešle e-mail, tak příslušné uživatele vědět o hello nový soubor.</span><span class="sxs-lookup"><span data-stu-id="9ed95-133">After your logic app copies hello file, add an Outlook action that sends an email so relevant users know about hello new file.</span></span> <span data-ttu-id="9ed95-134">Zadejte hello příjemce, název a text e-mailů hello.</span><span class="sxs-lookup"><span data-stu-id="9ed95-134">Enter hello recipients, title, and body of hello email.</span></span> 

   <span data-ttu-id="9ed95-135">V hello dynamického obsahu pro výběr můžete data výstupy z hello souboru konektoru, můžete přidat e-mailu toohello další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="9ed95-135">In hello dynamic content selector, you can choose data outputs from hello file connector so you can add more details toohello email.</span></span>

   ![Odesílání e-mailu akce](media/logic-apps-using-file-connector/send-email.png)

6. <span data-ttu-id="9ed95-137">Uložte svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="9ed95-137">Save your logic app.</span></span> <span data-ttu-id="9ed95-138">Testování aplikace s tím, že nahrajete soubor tooDropbox.</span><span class="sxs-lookup"><span data-stu-id="9ed95-138">Test your app by uploading a file tooDropbox.</span></span> <span data-ttu-id="9ed95-139">Hello soubor by měl získat zkopírovaný toohello místní sdílené složky a měli byste obdržet e-mailu o operaci hello.</span><span class="sxs-lookup"><span data-stu-id="9ed95-139">hello file should get copied toohello on-premises file share, and you should receive an email about hello operation.</span></span>

   > [!TIP] 
   > <span data-ttu-id="9ed95-140">Zjistěte, jak příliš[sledování funkce logic apps](../logic-apps/logic-apps-monitor-your-logic-apps.md).</span><span class="sxs-lookup"><span data-stu-id="9ed95-140">Learn how too[monitor your logic apps](../logic-apps/logic-apps-monitor-your-logic-apps.md).</span></span>

<span data-ttu-id="9ed95-141">Blahopřejeme, nyní máte aplikaci logiky práci, kterou můžete připojit tooyour v místním systému souborů.</span><span class="sxs-lookup"><span data-stu-id="9ed95-141">Congratulations, you now have a working logic app that can connect tooyour on-premises file system.</span></span> <span data-ttu-id="9ed95-142">Zkuste zkoumat další funkce, které hello konektor nabízí, třeba:</span><span class="sxs-lookup"><span data-stu-id="9ed95-142">Try exploring other functionalities that hello connector offers, for example:</span></span>

- <span data-ttu-id="9ed95-143">Vytvoření souboru</span><span class="sxs-lookup"><span data-stu-id="9ed95-143">Create file</span></span>
- <span data-ttu-id="9ed95-144">Zobrazit seznam souborů ve složce</span><span class="sxs-lookup"><span data-stu-id="9ed95-144">List files in folder</span></span>
- <span data-ttu-id="9ed95-145">Připojení souboru</span><span class="sxs-lookup"><span data-stu-id="9ed95-145">Append file</span></span>
- <span data-ttu-id="9ed95-146">Odstranit soubor</span><span class="sxs-lookup"><span data-stu-id="9ed95-146">Delete file</span></span>
- <span data-ttu-id="9ed95-147">Získat obsah souboru</span><span class="sxs-lookup"><span data-stu-id="9ed95-147">Get file content</span></span>
- <span data-ttu-id="9ed95-148">Získat obsah souboru pomocí cesty</span><span class="sxs-lookup"><span data-stu-id="9ed95-148">Get file content using path</span></span>
- <span data-ttu-id="9ed95-149">Získat metadata souboru</span><span class="sxs-lookup"><span data-stu-id="9ed95-149">Get file metadata</span></span>
- <span data-ttu-id="9ed95-150">Získat metadata souboru pomocí cesty</span><span class="sxs-lookup"><span data-stu-id="9ed95-150">Get file metadata using path</span></span>
- <span data-ttu-id="9ed95-151">Zobrazit seznam souborů v kořenové složce</span><span class="sxs-lookup"><span data-stu-id="9ed95-151">List files in root folder</span></span>
- <span data-ttu-id="9ed95-152">Soubor aktualizace</span><span class="sxs-lookup"><span data-stu-id="9ed95-152">Update file</span></span>

## <a name="view-hello-swagger"></a><span data-ttu-id="9ed95-153">Zobrazení hello swagger</span><span class="sxs-lookup"><span data-stu-id="9ed95-153">View hello swagger</span></span>
<span data-ttu-id="9ed95-154">V tématu hello [swagger podrobnosti](/connectors/fileconnector/).</span><span class="sxs-lookup"><span data-stu-id="9ed95-154">See hello [swagger details](/connectors/fileconnector/).</span></span> 

## <a name="get-help"></a><span data-ttu-id="9ed95-155">Podpora</span><span class="sxs-lookup"><span data-stu-id="9ed95-155">Get help</span></span>

<span data-ttu-id="9ed95-156">tooask otázky, odpovědi na otázky a zjistěte, jaké další Azure Logic Apps uživatelé dělají, navštivte hello [fórum Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span><span class="sxs-lookup"><span data-stu-id="9ed95-156">tooask questions, answer questions, and learn what other Azure Logic Apps users are doing, visit hello [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span></span>

<span data-ttu-id="9ed95-157">toohelp zlepšení Azure Logic Apps a konektory, hlasovat o nebo odeslání nápadů v hello [web pro zasílání názorů uživatele Azure Logic Apps](http://aka.ms/logicapps-wish).</span><span class="sxs-lookup"><span data-stu-id="9ed95-157">toohelp improve Azure Logic Apps and connectors, vote on or submit ideas at hello [Azure Logic Apps user feedback site](http://aka.ms/logicapps-wish).</span></span>

## <a name="next-steps"></a><span data-ttu-id="9ed95-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9ed95-158">Next steps</span></span>

- <span data-ttu-id="9ed95-159">[Připojení místní tooon dat](../logic-apps/logic-apps-gateway-connection.md) z aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="9ed95-159">[Connect tooon-premises data](../logic-apps/logic-apps-gateway-connection.md) from logic apps</span></span>
- <span data-ttu-id="9ed95-160">Další informace o [integrace enterprise](../logic-apps/logic-apps-enterprise-integration-overview.md)</span><span class="sxs-lookup"><span data-stu-id="9ed95-160">Learn about [enterprise integration](../logic-apps/logic-apps-enterprise-integration-overview.md)</span></span>
