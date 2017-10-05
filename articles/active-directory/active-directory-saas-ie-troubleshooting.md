---
title: "Řešení potíží s příponou Panel přístupu Azure pro aplikaci Internet Explorer | Microsoft Docs"
description: "Postup nasazení aplikace Internet Explorer rozšíření pro portál Moje aplikace pomocí zásad skupiny."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: f56b3230-26fd-42ec-9e3d-2c12daf15479
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/02/2017
ms.author: markvi
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 938d0b4046afa8c80eabe542f4541d0554948f5d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="troubleshooting-the-access-panel-extension-for-internet-explorer"></a><span data-ttu-id="6f70b-103">Řešení potíží s příponou Panel přístupu pro Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="6f70b-103">Troubleshooting the Access Panel Extension for Internet Explorer</span></span>
<span data-ttu-id="6f70b-104">Tento článek vám pomůže vyřešit tyto problémy:</span><span class="sxs-lookup"><span data-stu-id="6f70b-104">This article helps you troubleshoot the following problems:</span></span>

* <span data-ttu-id="6f70b-105">Nelze získat přístup k vaší aplikace prostřednictvím portálu Moje aplikace při používání aplikace Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="6f70b-105">You're unable to access your apps through the My Apps portal while using Internet Explorer.</span></span>
* <span data-ttu-id="6f70b-106">Zobrazí se zpráva "Instalace softwaru", i když jste již nainstalovali software.</span><span class="sxs-lookup"><span data-stu-id="6f70b-106">You see the "Install Software" message even though you've already installed the software.</span></span>

<span data-ttu-id="6f70b-107">Pokud jste správce, viz také: [nasazení rozšíření Panel přístupu pro Internet Explorer pomocí zásad skupiny](active-directory-saas-ie-group-policy.md)</span><span class="sxs-lookup"><span data-stu-id="6f70b-107">If you are an admin, see also: [How to Deploy the Access Panel Extension for Internet Explorer using Group Policy](active-directory-saas-ie-group-policy.md)</span></span>

## <a name="run-the-diagnostic-tool"></a><span data-ttu-id="6f70b-108">Spustit diagnostické nástroje</span><span class="sxs-lookup"><span data-stu-id="6f70b-108">Run the Diagnostic Tool</span></span>
<span data-ttu-id="6f70b-109">Instalace problémy s příponou Panel přístupu lze diagnostikovat pomocí stažení a spuštění diagnostické nástroje přístupového panelu:</span><span class="sxs-lookup"><span data-stu-id="6f70b-109">You can diagnose installation problems with the Access Panel Extension by downloading and running the Access Panel diagnostic tool:</span></span>

1. [<span data-ttu-id="6f70b-110">Chcete-li stáhnout nástroj pro diagnostiku, klikněte sem.</span><span class="sxs-lookup"><span data-stu-id="6f70b-110">Click here to download the diagnostic tool.</span></span>](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)
2. <span data-ttu-id="6f70b-111">Otevřete soubor a stiskněte klávesu **extrahujte všechny** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6f70b-111">Open the file, and press **Extract all** button.</span></span>
   
    ![Stiskněte klávesu extrahujte všechny](./media/active-directory-saas-ie-troubleshooting/extract1.png)
3. <span data-ttu-id="6f70b-113">Stiskněte **extrahovat** tlačítko Pokračovat.</span><span class="sxs-lookup"><span data-stu-id="6f70b-113">Then press the **Extract** button to continue.</span></span>
   
    ![Stiskněte klávesu extrakce](./media/active-directory-saas-ie-troubleshooting/extract2.png)
4. <span data-ttu-id="6f70b-115">Chcete-li spustit nástroj, klikněte pravým tlačítkem na soubor s názvem **AccessPanelExtensionDiagnosticTool**, pak vyberte **Otevřít protokolem > Microsoft Windows na základě Script Host**.</span><span class="sxs-lookup"><span data-stu-id="6f70b-115">To run the tool, right-click the file named **AccessPanelExtensionDiagnosticTool**, then select **Open with > Microsoft Windows Based Script Host**.</span></span>
   
    ![Otevřít v programu > základě Microsoft Windows Script Host](./media/active-directory-saas-ie-troubleshooting/open_tool.png)
5. <span data-ttu-id="6f70b-117">Zobrazí se následující okno diagnostiky, které popisuje, co mohou být další potíže s instalací.</span><span class="sxs-lookup"><span data-stu-id="6f70b-117">You will then see the following diagnostic window, which describes what might be wrong with your installation.</span></span>
   
    ![Ukázka okno diagnostiky](./media/active-directory-saas-ie-troubleshooting/tool_preview.png)
6. <span data-ttu-id="6f70b-119">Klikněte na tlačítko "**Ano**" umožníte programu opravit problémy, které byly nalezeny.</span><span class="sxs-lookup"><span data-stu-id="6f70b-119">Click "**YES**" to let the program fix the issues that have been found.</span></span>
7. <span data-ttu-id="6f70b-120">Pokud chcete tyto změny uložit, zavřít okno každé aplikace Internet Explorer a pak znovu otevřete Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="6f70b-120">To save these changes, close every Internet Explorer window, and then open Internet Explorer again.</span></span><br /><span data-ttu-id="6f70b-121">Pokud pořád nemáte přístup k vaší aplikace, zkuste následující postup.</span><span class="sxs-lookup"><span data-stu-id="6f70b-121">If you still can't access your apps, try the steps below.</span></span>

## <a name="check-that-the-access-panel-extension-is-enabled"></a><span data-ttu-id="6f70b-122">Zkontrolujte, že je povolené rozšíření přístup k panelu</span><span class="sxs-lookup"><span data-stu-id="6f70b-122">Check that the Access Panel Extension is enabled</span></span>
<span data-ttu-id="6f70b-123">Chcete-li ověřit, že je v Internet Exploreru povolená rozšíření přístup k panelu:</span><span class="sxs-lookup"><span data-stu-id="6f70b-123">To verify that the Access Panel Extension is enabled in Internet Explorer:</span></span>

1. <span data-ttu-id="6f70b-124">V aplikaci Internet Explorer, klikněte **ozubené kolečko ikonu** v pravém horním rohu okna.</span><span class="sxs-lookup"><span data-stu-id="6f70b-124">In Internet Explorer, click the **Gear icon** on the top right corner of the window.</span></span> <span data-ttu-id="6f70b-125">Potom vyberte **Možnosti Internetu**.</span><span class="sxs-lookup"><span data-stu-id="6f70b-125">Then select **Internet options**.</span></span><br /><span data-ttu-id="6f70b-126">(V starší verze aplikace Internet Explorer tento nástroj naleznete v části **nástroje > Možnosti Internetu**.</span><span class="sxs-lookup"><span data-stu-id="6f70b-126">(In older versions of Internet Explorer you can find this under **Tools > Internet options**.</span></span>
   
    ![Přejděte na Nástroje > Možnosti Internetu](./media/active-directory-saas-ie-troubleshooting/internetoptions.png)
2. <span data-ttu-id="6f70b-128">Klikněte na tlačítko **programy** a potom klikněte **spravovat doplňky** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6f70b-128">Click the **Programs** tab, then click the **Manage add-ons** button.</span></span>
   
    ![Kliknutím na Spravovat doplňky](./media/active-directory-saas-ie-troubleshooting/internetoptions_programs.png)
3. <span data-ttu-id="6f70b-130">V tomto dialogovém okně vyberte **rozšíření přístup k panelu** a klikněte **povolit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6f70b-130">In this dialog, select **Access Panel Extension** and then click the **Enable** button.</span></span>
   
    ![Klikněte na položku Povolit](./media/active-directory-saas-ie-troubleshooting/enableaddon.png)
4. <span data-ttu-id="6f70b-132">Pokud chcete tyto změny uložit, zavřít okno každé aplikace Internet Explorer a pak znovu otevřete Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="6f70b-132">To save these changes, close every Internet Explorer window and then open Internet Explorer again.</span></span>

## <a name="enable-extensions-for-inprivate-browsing"></a><span data-ttu-id="6f70b-133">Povolit rozšíření pro procházení InPrivate</span><span class="sxs-lookup"><span data-stu-id="6f70b-133">Enable Extensions for InPrivate Browsing</span></span>
<span data-ttu-id="6f70b-134">Pokud používáte v režimu procházení InPrivate:</span><span class="sxs-lookup"><span data-stu-id="6f70b-134">If you are using the InPrivate Browsing mode:</span></span>

1. <span data-ttu-id="6f70b-135">V aplikaci Internet Explorer, klikněte **ozubené kolečko ikonu** v pravém horním rohu okna.</span><span class="sxs-lookup"><span data-stu-id="6f70b-135">In Internet Explorer, click the **Gear icon** on the top right corner of the window.</span></span> <span data-ttu-id="6f70b-136">Potom vyberte **Možnosti Internetu**.</span><span class="sxs-lookup"><span data-stu-id="6f70b-136">Then select **Internet options**.</span></span><br /><span data-ttu-id="6f70b-137">(V starší verze aplikace Internet Explorer tento nástroj naleznete v části **nástroje > Možnosti Internetu**.</span><span class="sxs-lookup"><span data-stu-id="6f70b-137">(In older versions of Internet Explorer you can find this under **Tools > Internet options**.</span></span>
   
    ![Ukázka okno diagnostiky](./media/active-directory-saas-ie-troubleshooting/inprivateoptions.png)
2. <span data-ttu-id="6f70b-139">Přejděte na **o ochraně osobních údajů** kartě pak **zrušte zaškrtnutí políčka** zaškrtávací políčko s názvem bez přípony **zakažte panely nástrojů a rozšíření při procházení se službou InPrivate spuštění**</span><span class="sxs-lookup"><span data-stu-id="6f70b-139">Go to the **Privacy** tab, then **uncheck** the checkbox labeled **Disable toolbars and extensions when InPrivate Browsing starts**</span></span></p>
   
    ![Zrušte zaškrtnutí zakažte panely nástrojů a rozšíření při spuštění procházení InPrivate](./media/active-directory-saas-ie-troubleshooting/enabletoolbars.png)
3. <span data-ttu-id="6f70b-141">Pokud chcete tyto změny uložit, zavřít okno každé aplikace Internet Explorer a pak znovu otevřete Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="6f70b-141">To save these changes, close every Internet Explorer window and then open Internet Explorer again.</span></span>

## <a name="uninstall-the-access-panel-extension"></a><span data-ttu-id="6f70b-142">Odinstalovat rozšíření Panel přístupu</span><span class="sxs-lookup"><span data-stu-id="6f70b-142">Uninstall the Access Panel Extension</span></span>
<span data-ttu-id="6f70b-143">Chcete-li odinstalovat rozšíření přístupový Panel z vašeho počítače:</span><span class="sxs-lookup"><span data-stu-id="6f70b-143">To uninstall the Access Panel extension from your computer:</span></span>

1. <span data-ttu-id="6f70b-144">Na klávesnici, stiskněte **Windows klíč** k otevření nabídky Start.</span><span class="sxs-lookup"><span data-stu-id="6f70b-144">On your keyboard, press the **Windows key** to open the Start menu.</span></span> <span data-ttu-id="6f70b-145">V otevřeném v nabídce, můžete zadat nic, aby hledání.</span><span class="sxs-lookup"><span data-stu-id="6f70b-145">When the menu is open, you can type anything to do a search.</span></span> <span data-ttu-id="6f70b-146">Zadejte "Ovládací panely" a pak otevřete **ovládací panely** při zobrazí ve výsledcích hledání.</span><span class="sxs-lookup"><span data-stu-id="6f70b-146">Type "Control Panel" and then open the **Control Panel** when it appears in the search results.</span></span>
   
    ![Vyhledejte ovládací panely](./media/active-directory-saas-ie-troubleshooting/search_sm.png)
2. <span data-ttu-id="6f70b-148">V pravém horním rohu ovládacího panelu změnit **zobrazit** možnost k **ikony. velké ikony**.</span><span class="sxs-lookup"><span data-stu-id="6f70b-148">In the top right corner of the Control Panel, change the **View by** option to **Large icons**.</span></span> <span data-ttu-id="6f70b-149">Poté vyhledejte a vyberte **programy a funkce** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6f70b-149">Then find and click the **Programs and Features** button.</span></span>
   
    ![Zobrazení změn zobrazíte ikony. velké ikony](./media/active-directory-saas-ie-troubleshooting/control_panel.png)
3. <span data-ttu-id="6f70b-151">V seznamu vyberte **rozšíření přístup k panelu**a potom **odinstalovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6f70b-151">From the list, select **Access Panel Extension**, and the click the **Uninstall** button.</span></span>
   
    ![Klikněte na tlačítko Odinstalovat](./media/active-directory-saas-ie-troubleshooting/uninstall.png)
4. <span data-ttu-id="6f70b-153">Potom můžete zkusit nainstalovat rozšíření zjistíte, pokud byl problém vyřešen.</span><span class="sxs-lookup"><span data-stu-id="6f70b-153">You can then try to install the extension again to see if the problem has been resolved.</span></span>

<span data-ttu-id="6f70b-154">Pokud narazíte na problémy odinstalace rozšíření, můžete také odebrat pomocí [Microsoft opravte ji](https://go.microsoft.com/?linkid=9779673) nástroj.</span><span class="sxs-lookup"><span data-stu-id="6f70b-154">If you encounter issues uninstalling the extension, you can also remove it using the [Microsoft Fix It](https://go.microsoft.com/?linkid=9779673) tool.</span></span>

## <a name="related-articles"></a><span data-ttu-id="6f70b-155">Související články</span><span class="sxs-lookup"><span data-stu-id="6f70b-155">Related Articles</span></span>
* [<span data-ttu-id="6f70b-156">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6f70b-156">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="6f70b-157">Přístup k aplikaci a jednotné přihlašování s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6f70b-157">Application access and single sign-on with Azure Active Directory</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="6f70b-158">Postup nasazení rozšíření Panel přístupu pro Internet Explorer pomocí zásad skupiny</span><span class="sxs-lookup"><span data-stu-id="6f70b-158">How to Deploy the Access Panel Extension for Internet Explorer using Group Policy</span></span>](active-directory-saas-ie-group-policy.md)

