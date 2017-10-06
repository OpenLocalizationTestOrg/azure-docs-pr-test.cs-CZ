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
# <a name="troubleshooting-hello-access-panel-extension-for-internet-explorer"></a>Řešení potíží s hello rozšíření panely přístup pro prohlížeč Internet Explorer
Tento článek vám pomůže vyřešit hello následující problémy:

* Můžete se vaše aplikace prostřednictvím portálu Moje aplikace hello nelze tooaccess při používání aplikace Internet Explorer.
* Zobrazí zpráva "Instalace softwaru" i když jste již nainstalovali hello software hello.

Pokud jste správce, viz také: [jak tooDeploy hello rozšíření přístup k panelu pro Internet Explorer pomocí zásad skupiny](active-directory-saas-ie-group-policy.md)

## <a name="run-hello-diagnostic-tool"></a>Spusťte nástroj pro diagnostiku hello
Instalace problémy s hello rozšíření přístup k panelu lze diagnostikovat pomocí stažení a spuštění hello přístupový Panel diagnostické nástroje:

1. [Toodownload hello diagnostický nástroj, klikněte sem.](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)
2. Soubor otevřete hello a stiskněte klávesu **extrahujte všechny** tlačítko.
   
    ![Stiskněte klávesu extrahujte všechny](./media/active-directory-saas-ie-troubleshooting/extract1.png)
3. Stiskněte klávesu hello **extrahovat** toocontinue tlačítko.
   
    ![Stiskněte klávesu extrakce](./media/active-directory-saas-ie-troubleshooting/extract2.png)
4. Nástroj hello toorun, klikněte pravým tlačítkem na hello soubor s názvem **AccessPanelExtensionDiagnosticTool**, pak vyberte **Otevřít protokolem > Microsoft Windows na základě Script Host**.
   
    ![Otevřít v programu > základě Microsoft Windows Script Host](./media/active-directory-saas-ie-troubleshooting/open_tool.png)
5. Zobrazí se následující okno diagnostiky, která popisuje, co mohou být další potíže s instalací hello.
   
    ![Ukázka diagnostiky okna hello](./media/active-directory-saas-ie-troubleshooting/tool_preview.png)
6. Klikněte na tlačítko "**Ano**" toolet hello program oprava hello problémy, které byly nalezeny.
7. toosave tyto změny, každý okno Internet Exploreru zavřít a znovu otevřete Internet Explorer.<br />Pokud pořád nemáte přístup k vaší aplikace, zkuste následující postup hello.

## <a name="check-that-hello-access-panel-extension-is-enabled"></a>Zkontrolujte, že hello rozšíření přístup k panelu je povoleno
v Internet Exploreru je povolená tooverify, který hello rozšíření přístup k panelu:

1. V aplikaci Internet Explorer, klikněte na tlačítko hello **ozubené kolečko ikonu** na hello pravém horním rohu okna hello. Potom vyberte **Možnosti Internetu**.<br />(V starší verze aplikace Internet Explorer tento nástroj naleznete v části **nástroje > Možnosti Internetu**.
   
    ![Přejděte tooTools > Možnosti Internetu](./media/active-directory-saas-ie-troubleshooting/internetoptions.png)
2. Klikněte na tlačítko hello **programy** a potom klikněte na tlačítko hello **spravovat doplňky** tlačítko.
   
    ![Kliknutím na Spravovat doplňky](./media/active-directory-saas-ie-troubleshooting/internetoptions_programs.png)
3. V tomto dialogovém okně vyberte **rozšíření přístup k panelu** a pak klikněte na tlačítko hello **povolit** tlačítko.
   
    ![Klikněte na položku Povolit](./media/active-directory-saas-ie-troubleshooting/enableaddon.png)
4. toosave tyto změny, zavřete všechny okně Internet Exploreru a pak znovu otevřete Internet Explorer.

## <a name="enable-extensions-for-inprivate-browsing"></a>Povolit rozšíření pro procházení InPrivate
Pokud používáte režim procházení se službou InPrivate hello:

1. V aplikaci Internet Explorer, klikněte na tlačítko hello **ozubené kolečko ikonu** na hello pravém horním rohu okna hello. Potom vyberte **Možnosti Internetu**.<br />(V starší verze aplikace Internet Explorer tento nástroj naleznete v části **nástroje > Možnosti Internetu**.
   
    ![Ukázka diagnostiky okna hello](./media/active-directory-saas-ie-troubleshooting/inprivateoptions.png)
2. Přejděte toohello **o ochraně osobních údajů** kartě pak **zrušte zaškrtnutí políčka** hello zaškrtávací políčko s názvem bez přípony **zakažte panely nástrojů a rozšíření při procházení se službou InPrivate spuštění**</p>
   
    ![Zrušte zaškrtnutí zakažte panely nástrojů a rozšíření při spuštění procházení InPrivate](./media/active-directory-saas-ie-troubleshooting/enabletoolbars.png)
3. toosave tyto změny, zavřete všechny okně Internet Exploreru a pak znovu otevřete Internet Explorer.

## <a name="uninstall-hello-access-panel-extension"></a>Odinstalovat rozšíření přístup k panelu hello
toouninstall hello přístupový Panel rozšíření z vašeho počítače:

1. Na klávesnici stiskněte hello **Windows klíč** nabídky Start tooopen hello. V otevřeném hello nabídky, můžete zadat nic toodo vyhledávání. Zadejte "Ovládací panely" a pak otevřete hello **ovládací panely** při zobrazí ve výsledcích hledání hello.
   
    ![Vyhledejte ovládací panely](./media/active-directory-saas-ie-troubleshooting/search_sm.png)
2. Hello pravém horním rohu hello ovládací panely, změňte hello **zobrazit** možnost příliš**ikony. velké ikony**. Poté vyhledejte a vyberte hello **programy a funkce** tlačítko.
   
    ![Změn hello zobrazení tooshow ikony. velké ikony](./media/active-directory-saas-ie-troubleshooting/control_panel.png)
3. Hello seznamu, vyberte **rozšíření přístup k panelu**a klikněte na tlačítko hello hello **odinstalovat** tlačítko.
   
    ![Klikněte na tlačítko Odinstalovat](./media/active-directory-saas-ie-troubleshooting/uninstall.png)
4. Potom můžete zkusit tooinstall hello rozšíření znovu toosee Pokud hello problém byl vyřešen.

Pokud narazíte na problémy odinstalace hello rozšíření, můžete také odebrat pomocí hello [Microsoft opravte ji](https://go.microsoft.com/?linkid=9779673) nástroj.

## <a name="related-articles"></a>Související články
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
* [Přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md)
* [Jak tooDeploy hello rozšíření přístup k panelu pro Internet Explorer pomocí zásad skupiny](active-directory-saas-ie-group-policy.md)

