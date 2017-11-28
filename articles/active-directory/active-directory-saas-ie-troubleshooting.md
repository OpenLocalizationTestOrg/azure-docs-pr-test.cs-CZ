---
title: "aaaTroubleshooting hello Azure přístup k panelu rozšíření pro aplikaci Internet Explorer | Microsoft Docs"
description: "Jak toouse skupiny zásad toodeploy hello Internet Explorer rozšíření pro portál Moje aplikace hello."
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
ms.openlocfilehash: 23cbb6117f34759330206de3a26f1397486acedb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-hello-access-panel-extension-for-internet-explorer"></a><span data-ttu-id="c6a55-103">Řešení potíží s hello rozšíření panely přístup pro prohlížeč Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="c6a55-103">Troubleshooting hello Access Panel Extension for Internet Explorer</span></span>
<span data-ttu-id="c6a55-104">Tento článek vám pomůže vyřešit hello následující problémy:</span><span class="sxs-lookup"><span data-stu-id="c6a55-104">This article helps you troubleshoot hello following problems:</span></span>

* <span data-ttu-id="c6a55-105">Můžete se vaše aplikace prostřednictvím portálu Moje aplikace hello nelze tooaccess při používání aplikace Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="c6a55-105">You're unable tooaccess your apps through hello My Apps portal while using Internet Explorer.</span></span>
* <span data-ttu-id="c6a55-106">Zobrazí zpráva "Instalace softwaru" i když jste již nainstalovali hello software hello.</span><span class="sxs-lookup"><span data-stu-id="c6a55-106">You see hello "Install Software" message even though you've already installed hello software.</span></span>

<span data-ttu-id="c6a55-107">Pokud jste správce, viz také: [jak tooDeploy hello rozšíření přístup k panelu pro Internet Explorer pomocí zásad skupiny](active-directory-saas-ie-group-policy.md)</span><span class="sxs-lookup"><span data-stu-id="c6a55-107">If you are an admin, see also: [How tooDeploy hello Access Panel Extension for Internet Explorer using Group Policy](active-directory-saas-ie-group-policy.md)</span></span>

## <a name="run-hello-diagnostic-tool"></a><span data-ttu-id="c6a55-108">Spusťte nástroj pro diagnostiku hello</span><span class="sxs-lookup"><span data-stu-id="c6a55-108">Run hello Diagnostic Tool</span></span>
<span data-ttu-id="c6a55-109">Instalace problémy s hello rozšíření přístup k panelu lze diagnostikovat pomocí stažení a spuštění hello přístupový Panel diagnostické nástroje:</span><span class="sxs-lookup"><span data-stu-id="c6a55-109">You can diagnose installation problems with hello Access Panel Extension by downloading and running hello Access Panel diagnostic tool:</span></span>

1. [<span data-ttu-id="c6a55-110">Toodownload hello diagnostický nástroj, klikněte sem.</span><span class="sxs-lookup"><span data-stu-id="c6a55-110">Click here toodownload hello diagnostic tool.</span></span>](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)
2. <span data-ttu-id="c6a55-111">Soubor otevřete hello a stiskněte klávesu **extrahujte všechny** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c6a55-111">Open hello file, and press **Extract all** button.</span></span>
   
    ![Stiskněte klávesu extrahujte všechny](./media/active-directory-saas-ie-troubleshooting/extract1.png)
3. <span data-ttu-id="c6a55-113">Stiskněte klávesu hello **extrahovat** toocontinue tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c6a55-113">Then press hello **Extract** button toocontinue.</span></span>
   
    ![Stiskněte klávesu extrakce](./media/active-directory-saas-ie-troubleshooting/extract2.png)
4. <span data-ttu-id="c6a55-115">Nástroj hello toorun, klikněte pravým tlačítkem na hello soubor s názvem **AccessPanelExtensionDiagnosticTool**, pak vyberte **Otevřít protokolem > Microsoft Windows na základě Script Host**.</span><span class="sxs-lookup"><span data-stu-id="c6a55-115">toorun hello tool, right-click hello file named **AccessPanelExtensionDiagnosticTool**, then select **Open with > Microsoft Windows Based Script Host**.</span></span>
   
    ![Otevřít v programu > základě Microsoft Windows Script Host](./media/active-directory-saas-ie-troubleshooting/open_tool.png)
5. <span data-ttu-id="c6a55-117">Zobrazí se následující okno diagnostiky, která popisuje, co mohou být další potíže s instalací hello.</span><span class="sxs-lookup"><span data-stu-id="c6a55-117">You will then see hello following diagnostic window, which describes what might be wrong with your installation.</span></span>
   
    ![Ukázka diagnostiky okna hello](./media/active-directory-saas-ie-troubleshooting/tool_preview.png)
6. <span data-ttu-id="c6a55-119">Klikněte na tlačítko "**Ano**" toolet hello program oprava hello problémy, které byly nalezeny.</span><span class="sxs-lookup"><span data-stu-id="c6a55-119">Click "**YES**" toolet hello program fix hello issues that have been found.</span></span>
7. <span data-ttu-id="c6a55-120">toosave tyto změny, každý okno Internet Exploreru zavřít a znovu otevřete Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="c6a55-120">toosave these changes, close every Internet Explorer window, and then open Internet Explorer again.</span></span><br /><span data-ttu-id="c6a55-121">Pokud pořád nemáte přístup k vaší aplikace, zkuste následující postup hello.</span><span class="sxs-lookup"><span data-stu-id="c6a55-121">If you still can't access your apps, try hello steps below.</span></span>

## <a name="check-that-hello-access-panel-extension-is-enabled"></a><span data-ttu-id="c6a55-122">Zkontrolujte, že hello rozšíření přístup k panelu je povoleno</span><span class="sxs-lookup"><span data-stu-id="c6a55-122">Check that hello Access Panel Extension is enabled</span></span>
<span data-ttu-id="c6a55-123">v Internet Exploreru je povolená tooverify, který hello rozšíření přístup k panelu:</span><span class="sxs-lookup"><span data-stu-id="c6a55-123">tooverify that hello Access Panel Extension is enabled in Internet Explorer:</span></span>

1. <span data-ttu-id="c6a55-124">V aplikaci Internet Explorer, klikněte na tlačítko hello **ozubené kolečko ikonu** na hello pravém horním rohu okna hello.</span><span class="sxs-lookup"><span data-stu-id="c6a55-124">In Internet Explorer, click hello **Gear icon** on hello top right corner of hello window.</span></span> <span data-ttu-id="c6a55-125">Potom vyberte **Možnosti Internetu**.</span><span class="sxs-lookup"><span data-stu-id="c6a55-125">Then select **Internet options**.</span></span><br /><span data-ttu-id="c6a55-126">(V starší verze aplikace Internet Explorer tento nástroj naleznete v části **nástroje > Možnosti Internetu**.</span><span class="sxs-lookup"><span data-stu-id="c6a55-126">(In older versions of Internet Explorer you can find this under **Tools > Internet options**.</span></span>
   
    ![Přejděte tooTools > Možnosti Internetu](./media/active-directory-saas-ie-troubleshooting/internetoptions.png)
2. <span data-ttu-id="c6a55-128">Klikněte na tlačítko hello **programy** a potom klikněte na tlačítko hello **spravovat doplňky** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c6a55-128">Click hello **Programs** tab, then click hello **Manage add-ons** button.</span></span>
   
    ![Kliknutím na Spravovat doplňky](./media/active-directory-saas-ie-troubleshooting/internetoptions_programs.png)
3. <span data-ttu-id="c6a55-130">V tomto dialogovém okně vyberte **rozšíření přístup k panelu** a pak klikněte na tlačítko hello **povolit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c6a55-130">In this dialog, select **Access Panel Extension** and then click hello **Enable** button.</span></span>
   
    ![Klikněte na položku Povolit](./media/active-directory-saas-ie-troubleshooting/enableaddon.png)
4. <span data-ttu-id="c6a55-132">toosave tyto změny, zavřete všechny okně Internet Exploreru a pak znovu otevřete Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="c6a55-132">toosave these changes, close every Internet Explorer window and then open Internet Explorer again.</span></span>

## <a name="enable-extensions-for-inprivate-browsing"></a><span data-ttu-id="c6a55-133">Povolit rozšíření pro procházení InPrivate</span><span class="sxs-lookup"><span data-stu-id="c6a55-133">Enable Extensions for InPrivate Browsing</span></span>
<span data-ttu-id="c6a55-134">Pokud používáte režim procházení se službou InPrivate hello:</span><span class="sxs-lookup"><span data-stu-id="c6a55-134">If you are using hello InPrivate Browsing mode:</span></span>

1. <span data-ttu-id="c6a55-135">V aplikaci Internet Explorer, klikněte na tlačítko hello **ozubené kolečko ikonu** na hello pravém horním rohu okna hello.</span><span class="sxs-lookup"><span data-stu-id="c6a55-135">In Internet Explorer, click hello **Gear icon** on hello top right corner of hello window.</span></span> <span data-ttu-id="c6a55-136">Potom vyberte **Možnosti Internetu**.</span><span class="sxs-lookup"><span data-stu-id="c6a55-136">Then select **Internet options**.</span></span><br /><span data-ttu-id="c6a55-137">(V starší verze aplikace Internet Explorer tento nástroj naleznete v části **nástroje > Možnosti Internetu**.</span><span class="sxs-lookup"><span data-stu-id="c6a55-137">(In older versions of Internet Explorer you can find this under **Tools > Internet options**.</span></span>
   
    ![Ukázka diagnostiky okna hello](./media/active-directory-saas-ie-troubleshooting/inprivateoptions.png)
2. <span data-ttu-id="c6a55-139">Přejděte toohello **o ochraně osobních údajů** kartě pak **zrušte zaškrtnutí políčka** hello zaškrtávací políčko s názvem bez přípony **zakažte panely nástrojů a rozšíření při procházení se službou InPrivate spuštění**</span><span class="sxs-lookup"><span data-stu-id="c6a55-139">Go toohello **Privacy** tab, then **uncheck** hello checkbox labeled **Disable toolbars and extensions when InPrivate Browsing starts**</span></span></p>
   
    ![Zrušte zaškrtnutí zakažte panely nástrojů a rozšíření při spuštění procházení InPrivate](./media/active-directory-saas-ie-troubleshooting/enabletoolbars.png)
3. <span data-ttu-id="c6a55-141">toosave tyto změny, zavřete všechny okně Internet Exploreru a pak znovu otevřete Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="c6a55-141">toosave these changes, close every Internet Explorer window and then open Internet Explorer again.</span></span>

## <a name="uninstall-hello-access-panel-extension"></a><span data-ttu-id="c6a55-142">Odinstalovat rozšíření přístup k panelu hello</span><span class="sxs-lookup"><span data-stu-id="c6a55-142">Uninstall hello Access Panel Extension</span></span>
<span data-ttu-id="c6a55-143">toouninstall hello přístupový Panel rozšíření z vašeho počítače:</span><span class="sxs-lookup"><span data-stu-id="c6a55-143">toouninstall hello Access Panel extension from your computer:</span></span>

1. <span data-ttu-id="c6a55-144">Na klávesnici stiskněte hello **Windows klíč** nabídky Start tooopen hello.</span><span class="sxs-lookup"><span data-stu-id="c6a55-144">On your keyboard, press hello **Windows key** tooopen hello Start menu.</span></span> <span data-ttu-id="c6a55-145">V otevřeném hello nabídky, můžete zadat nic toodo vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="c6a55-145">When hello menu is open, you can type anything toodo a search.</span></span> <span data-ttu-id="c6a55-146">Zadejte "Ovládací panely" a pak otevřete hello **ovládací panely** při zobrazí ve výsledcích hledání hello.</span><span class="sxs-lookup"><span data-stu-id="c6a55-146">Type "Control Panel" and then open hello **Control Panel** when it appears in hello search results.</span></span>
   
    ![Vyhledejte ovládací panely](./media/active-directory-saas-ie-troubleshooting/search_sm.png)
2. <span data-ttu-id="c6a55-148">Hello pravém horním rohu hello ovládací panely, změňte hello **zobrazit** možnost příliš**ikony. velké ikony**.</span><span class="sxs-lookup"><span data-stu-id="c6a55-148">In hello top right corner of hello Control Panel, change hello **View by** option too**Large icons**.</span></span> <span data-ttu-id="c6a55-149">Poté vyhledejte a vyberte hello **programy a funkce** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c6a55-149">Then find and click hello **Programs and Features** button.</span></span>
   
    ![Změn hello zobrazení tooshow ikony. velké ikony](./media/active-directory-saas-ie-troubleshooting/control_panel.png)
3. <span data-ttu-id="c6a55-151">Hello seznamu, vyberte **rozšíření přístup k panelu**a klikněte na tlačítko hello hello **odinstalovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c6a55-151">From hello list, select **Access Panel Extension**, and hello click hello **Uninstall** button.</span></span>
   
    ![Klikněte na tlačítko Odinstalovat](./media/active-directory-saas-ie-troubleshooting/uninstall.png)
4. <span data-ttu-id="c6a55-153">Potom můžete zkusit tooinstall hello rozšíření znovu toosee Pokud hello problém byl vyřešen.</span><span class="sxs-lookup"><span data-stu-id="c6a55-153">You can then try tooinstall hello extension again toosee if hello problem has been resolved.</span></span>

<span data-ttu-id="c6a55-154">Pokud narazíte na problémy odinstalace hello rozšíření, můžete také odebrat pomocí hello [Microsoft opravte ji](https://go.microsoft.com/?linkid=9779673) nástroj.</span><span class="sxs-lookup"><span data-stu-id="c6a55-154">If you encounter issues uninstalling hello extension, you can also remove it using hello [Microsoft Fix It](https://go.microsoft.com/?linkid=9779673) tool.</span></span>

## <a name="related-articles"></a><span data-ttu-id="c6a55-155">Související články</span><span class="sxs-lookup"><span data-stu-id="c6a55-155">Related Articles</span></span>
* [<span data-ttu-id="c6a55-156">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c6a55-156">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="c6a55-157">Přístup k aplikaci a jednotné přihlašování s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c6a55-157">Application access and single sign-on with Azure Active Directory</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="c6a55-158">Jak tooDeploy hello rozšíření přístup k panelu pro Internet Explorer pomocí zásad skupiny</span><span class="sxs-lookup"><span data-stu-id="c6a55-158">How tooDeploy hello Access Panel Extension for Internet Explorer using Group Policy</span></span>](active-directory-saas-ie-group-policy.md)

