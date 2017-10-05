---
title: "Připojit k místní systémy souborů z Azure Logic Apps | Microsoft Docs"
description: "Připojení k místní systémy souborů z vaší aplikace logiky pracovního postupu prostřednictvím místní brána dat a konektor systému souborů"
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
ms.openlocfilehash: f33e7c58103c57e17e4e273caba1ab9b83f0cd2b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="connect-to-on-premises-file-systems-from-logic-apps-with-the-file-system-connector"></a><span data-ttu-id="101b9-104">Připojit k místní systémy souborů z aplikace logiky s konektorem systému souborů</span><span class="sxs-lookup"><span data-stu-id="101b9-104">Connect to on-premises file systems from logic apps with the File System connector</span></span>

<span data-ttu-id="101b9-105">Hybridní cloud připojení je důležitá pro aplikace logiky, takže spravovat data a bezpečný přístup k místním prostředkům, aplikace logiky můžete použít místní brána data.</span><span class="sxs-lookup"><span data-stu-id="101b9-105">Hybrid cloud connectivity is central to logic apps, so to manage data and securely access on-premises resources, your logic apps can use the on-premises data gateway.</span></span> <span data-ttu-id="101b9-106">V tomto článku jsme ukazují, jak se připojit k systému souborů místní pomocí základní scénáře: Zkopírujte soubor, který nahrání dat do sdílené složky a potom odeslat e-mail.</span><span class="sxs-lookup"><span data-stu-id="101b9-106">In this article, we show how to connect to an on-premises file system with a basic scenario: copy a file that's uploaded to Dropbox to a file share, then send an email.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="101b9-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="101b9-107">Prerequisites</span></span>

- <span data-ttu-id="101b9-108">Instalace a konfigurace nejnovější [místní brána dat](https://www.microsoft.com/download/details.aspx?id=53127).</span><span class="sxs-lookup"><span data-stu-id="101b9-108">Install and configure the latest [on-premises data gateway](https://www.microsoft.com/download/details.aspx?id=53127).</span></span>
- <span data-ttu-id="101b9-109">Nainstalujte nejnovější bránu místní data, verzi 1.15.6150.1 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="101b9-109">Install the latest on-premises data gateway, version 1.15.6150.1 or above.</span></span> <span data-ttu-id="101b9-110">[Připojení k bráně místní data](http://aka.ms/logicapps-gateway) jsou uvedené kroky.</span><span class="sxs-lookup"><span data-stu-id="101b9-110">[Connect to the on-premises data gateway](http://aka.ms/logicapps-gateway) lists the steps.</span></span> <span data-ttu-id="101b9-111">Brány musí být nainstalován v místním počítači, než budete pokračovat s dalšími kroky.</span><span class="sxs-lookup"><span data-stu-id="101b9-111">The gateway must be installed on an on-premises machine before you can continue with the rest of the steps.</span></span>

## <a name="add-trigger-and-actions-for-connecting-to-your-file-system"></a><span data-ttu-id="101b9-112">Přidání aktivační události a akcí pro připojení k systému souborů</span><span class="sxs-lookup"><span data-stu-id="101b9-112">Add trigger and actions for connecting to your file system</span></span>

1. <span data-ttu-id="101b9-113">Vytvoření aplikace logiky a přidejte této aktivační události Dropbox: **vytvoření souboru**</span><span class="sxs-lookup"><span data-stu-id="101b9-113">Create a logic app, and add this Dropbox trigger: **When a file is created**</span></span> 
2. <span data-ttu-id="101b9-114">V části aktivační událost, zvolte **další krok** > **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="101b9-114">Under the trigger, choose **Next Step** > **Add an action**.</span></span> 
3. <span data-ttu-id="101b9-115">Do vyhledávacího pole zadejte `file system` , můžete zobrazit všechna podporovaná akce pro konektor systému souborů.</span><span class="sxs-lookup"><span data-stu-id="101b9-115">In the search box, enter `file system` so you can view all supported actions for the File System connector.</span></span>

   ![Vyhledejte soubor konektoru](media/logic-apps-using-file-connector/search-file-connector.png)

2. <span data-ttu-id="101b9-117">Vyberte **vytvořit soubor** akce a vytvořit připojení k systému souborů.</span><span class="sxs-lookup"><span data-stu-id="101b9-117">Choose the **Create file** action, and create a connection to your file system.</span></span>

   <span data-ttu-id="101b9-118">Pokud nemáte existující připojení, zobrazí se výzva k jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="101b9-118">If you don't have an existing connection, you are prompted to create one.</span></span>

   1. <span data-ttu-id="101b9-119">Zvolte **připojit prostřednictvím místní brána dat**.</span><span class="sxs-lookup"><span data-stu-id="101b9-119">Choose **Connect via on-premises data gateway**.</span></span> <span data-ttu-id="101b9-120">Zobrazí se další vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="101b9-120">More properties appear.</span></span>
   2. <span data-ttu-id="101b9-121">Vyberte kořenové složce systému souborů.</span><span class="sxs-lookup"><span data-stu-id="101b9-121">Select your root folder for your file system.</span></span>
      
       > [!NOTE]
       > <span data-ttu-id="101b9-122">Kořenová složka je hlavní nadřazené složky, která se používá k relativní cesty pro všechny akce související s souboru.</span><span class="sxs-lookup"><span data-stu-id="101b9-122">The root folder is the main parent folder, which is used for relative paths for all file-related actions.</span></span> <span data-ttu-id="101b9-123">Můžete zadat do místní složky v počítači, kde je nainstalován bránu dat na místě, nebo složce může být sdílená síťová složka má počítač přístup.</span><span class="sxs-lookup"><span data-stu-id="101b9-123">You can specify a local folder on the machine where the on-premises data gateway is installed, or the folder can be a network share that the machine can access.</span></span>

   3. <span data-ttu-id="101b9-124">Zadejte uživatelské jméno a heslo pro bránu.</span><span class="sxs-lookup"><span data-stu-id="101b9-124">Enter the username and password for the gateway.</span></span>
   4. <span data-ttu-id="101b9-125">Vyberte bránu, kterou jste dříve nainstalovali.</span><span class="sxs-lookup"><span data-stu-id="101b9-125">Select the gateway that you previously installed.</span></span>

       ![Konfigurace připojení](media/logic-apps-using-file-connector/create-file.png)

3. <span data-ttu-id="101b9-127">Po zadání všechny podrobnosti, vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="101b9-127">After you provide all the details, choose **Create**.</span></span> 

   <span data-ttu-id="101b9-128">Služba Logic Apps nakonfiguruje a otestuje připojení, a ujistěte se, že připojení funguje správně.</span><span class="sxs-lookup"><span data-stu-id="101b9-128">Logic Apps configures and tests your connection, making sure that the connection works properly.</span></span> 
   <span data-ttu-id="101b9-129">Pokud je připojení správně nastavena, zobrazí možnosti pro akci, kterou jste dříve vybrali.</span><span class="sxs-lookup"><span data-stu-id="101b9-129">If the connection is set up correctly, you see options for the action that you previously selected.</span></span> 
   <span data-ttu-id="101b9-130">Konektor systému souborů je nyní připravena k použití.</span><span class="sxs-lookup"><span data-stu-id="101b9-130">The file system connector is now ready for use.</span></span>

4. <span data-ttu-id="101b9-131">Zadejte, že chcete kopírovat soubory z Dropbox do kořenové složky pro vaše místní sdílená.</span><span class="sxs-lookup"><span data-stu-id="101b9-131">Specify that you want to copy files from Dropbox to the root folder for your on-premises file share.</span></span>

   ![Vytvoření souboru akce](media/logic-apps-using-file-connector/create-file-filled.png)

5. <span data-ttu-id="101b9-133">Po aplikaci logiky zkopíruje soubor, přidejte Outlook akci, která odešle e-mail, tak příslušné uživatele vědět o nový soubor.</span><span class="sxs-lookup"><span data-stu-id="101b9-133">After your logic app copies the file, add an Outlook action that sends an email so relevant users know about the new file.</span></span> <span data-ttu-id="101b9-134">Zadejte příjemce, název a text e-mailu.</span><span class="sxs-lookup"><span data-stu-id="101b9-134">Enter the recipients, title, and body of the email.</span></span> 

   <span data-ttu-id="101b9-135">V dynamického obsahu výběr můžete data výstupy z konektoru nástroje souboru, můžete přidat další podrobnosti k e-mailu.</span><span class="sxs-lookup"><span data-stu-id="101b9-135">In the dynamic content selector, you can choose data outputs from the file connector so you can add more details to the email.</span></span>

   ![Odesílání e-mailu akce](media/logic-apps-using-file-connector/send-email.png)

6. <span data-ttu-id="101b9-137">Uložte svou aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="101b9-137">Save your logic app.</span></span> <span data-ttu-id="101b9-138">Testování aplikace s tím, že nahrajete soubor na Dropbox.</span><span class="sxs-lookup"><span data-stu-id="101b9-138">Test your app by uploading a file to Dropbox.</span></span> <span data-ttu-id="101b9-139">Soubor by měl získat zkopírován do místní sdílené složky a měli byste obdržet e-mailu o operaci.</span><span class="sxs-lookup"><span data-stu-id="101b9-139">The file should get copied to the on-premises file share, and you should receive an email about the operation.</span></span>

   > [!TIP] 
   > <span data-ttu-id="101b9-140">Zjistěte, jak [sledování funkce logic apps](../logic-apps/logic-apps-monitor-your-logic-apps.md).</span><span class="sxs-lookup"><span data-stu-id="101b9-140">Learn how to [monitor your logic apps](../logic-apps/logic-apps-monitor-your-logic-apps.md).</span></span>

<span data-ttu-id="101b9-141">Blahopřejeme, nyní máte pracovní aplikace logiky, která může připojit k systému souborů na místě.</span><span class="sxs-lookup"><span data-stu-id="101b9-141">Congratulations, you now have a working logic app that can connect to your on-premises file system.</span></span> <span data-ttu-id="101b9-142">Zkuste zkoumat další funkce, které nabízí konektor, například:</span><span class="sxs-lookup"><span data-stu-id="101b9-142">Try exploring other functionalities that the connector offers, for example:</span></span>

- <span data-ttu-id="101b9-143">Vytvoření souboru</span><span class="sxs-lookup"><span data-stu-id="101b9-143">Create file</span></span>
- <span data-ttu-id="101b9-144">Zobrazit seznam souborů ve složce</span><span class="sxs-lookup"><span data-stu-id="101b9-144">List files in folder</span></span>
- <span data-ttu-id="101b9-145">Připojení souboru</span><span class="sxs-lookup"><span data-stu-id="101b9-145">Append file</span></span>
- <span data-ttu-id="101b9-146">Odstranit soubor</span><span class="sxs-lookup"><span data-stu-id="101b9-146">Delete file</span></span>
- <span data-ttu-id="101b9-147">Získat obsah souboru</span><span class="sxs-lookup"><span data-stu-id="101b9-147">Get file content</span></span>
- <span data-ttu-id="101b9-148">Získat obsah souboru pomocí cesty</span><span class="sxs-lookup"><span data-stu-id="101b9-148">Get file content using path</span></span>
- <span data-ttu-id="101b9-149">Získat metadata souboru</span><span class="sxs-lookup"><span data-stu-id="101b9-149">Get file metadata</span></span>
- <span data-ttu-id="101b9-150">Získat metadata souboru pomocí cesty</span><span class="sxs-lookup"><span data-stu-id="101b9-150">Get file metadata using path</span></span>
- <span data-ttu-id="101b9-151">Zobrazit seznam souborů v kořenové složce</span><span class="sxs-lookup"><span data-stu-id="101b9-151">List files in root folder</span></span>
- <span data-ttu-id="101b9-152">Soubor aktualizace</span><span class="sxs-lookup"><span data-stu-id="101b9-152">Update file</span></span>

## <a name="view-the-swagger"></a><span data-ttu-id="101b9-153">Zobrazení swagger</span><span class="sxs-lookup"><span data-stu-id="101b9-153">View the swagger</span></span>
<span data-ttu-id="101b9-154">Najdete v článku [swagger podrobnosti](/connectors/fileconnector/).</span><span class="sxs-lookup"><span data-stu-id="101b9-154">See the [swagger details](/connectors/fileconnector/).</span></span> 

## <a name="get-help"></a><span data-ttu-id="101b9-155">Podpora</span><span class="sxs-lookup"><span data-stu-id="101b9-155">Get help</span></span>

<span data-ttu-id="101b9-156">Klást otázky, odpovídat na ně a poučit se ze zkušeností jiných uživatelů Azure Logic Apps můžete ve [fóru Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span><span class="sxs-lookup"><span data-stu-id="101b9-156">To ask questions, answer questions, and learn what other Azure Logic Apps users are doing, visit the [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span></span>

<span data-ttu-id="101b9-157">Pokud chcete pomoci při vylepšování Azure Logic Apps a konektorů, hlasujte nebo zanechte své nápady na [webu zpětné vazby uživatelů Azure Logic Apps](http://aka.ms/logicapps-wish).</span><span class="sxs-lookup"><span data-stu-id="101b9-157">To help improve Azure Logic Apps and connectors, vote on or submit ideas at the [Azure Logic Apps user feedback site](http://aka.ms/logicapps-wish).</span></span>

## <a name="next-steps"></a><span data-ttu-id="101b9-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="101b9-158">Next steps</span></span>

- <span data-ttu-id="101b9-159">[Připojení k místním datům](../logic-apps/logic-apps-gateway-connection.md) z aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="101b9-159">[Connect to on-premises data](../logic-apps/logic-apps-gateway-connection.md) from logic apps</span></span>
- <span data-ttu-id="101b9-160">Další informace o [integrace enterprise](../logic-apps/logic-apps-enterprise-integration-overview.md)</span><span class="sxs-lookup"><span data-stu-id="101b9-160">Learn about [enterprise integration](../logic-apps/logic-apps-enterprise-integration-overview.md)</span></span>
