---
title: "aaaDeploy Azure přístup k panelu rozšíření pro aplikaci Internet Explorer pomocí objektu zásad skupiny | Microsoft Docs"
description: "Jak toouse skupiny zásad toodeploy hello Internet Explorer rozšíření pro portál Moje aplikace hello."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 7c2d49c8-5be0-4e7e-abac-332f9dfda736
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/02/2017
ms.author: markvi
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 926f15950bbe81d2fbfe1b0b856470da40880d7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodeploy-hello-access-panel-extension-for-internet-explorer-using-group-policy"></a>Jak tooDeploy hello rozšíření přístup k panelu pro Internet Explorer pomocí zásad skupiny
Tento kurz ukazuje, jak tooremotely zásad skupiny toouse instalovat hello přístupový Panel rozšíření pro Internet Explorer na počítačích uživatelů. Toto rozšíření je pro Internet Explorer uživatelů, kteří potřebují toosign do aplikace, které jsou nakonfigurované pomocí [založené na heslech jednotné přihlašování](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).

Doporučuje se, že správci automatizovat nasazení hello tohoto rozšíření. Jinak uživatelé mají toodownload a nainstalovat hello rozšíření, která je chyba náchylné k chybám toouser a musí mít oprávnění správce. Tento kurz se zaměřuje na jednu z metod automatizaci nasazení softwaru pomocí zásad skupiny. [Další informace o zásadách skupiny.](https://technet.microsoft.com/windowsserver/bb310732.aspx)

Hello rozšíření přístupový Panel je také k dispozici pro [Chrome](https://go.microsoft.com/fwLink/?LinkID=311859) a [Firefox](https://go.microsoft.com/fwLink/?LinkID=626998), ani jedno, které vyžadují tooinstall oprávnění správce.

## <a name="prerequisites"></a>Požadavky
* Jste nastavili [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), a připojení uživatelů počítačů tooyour domény.
* Musíte mít hello "Upravit nastavení" oprávnění tooedit hello objekt zásad skupiny (GPO). Ve výchozím nastavení, toto oprávnění mají členové hello následující skupiny zabezpečení: Domain Administrators, Enterprise Administrators a Group Policy Creator Owners. [Další informace](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx)

## <a name="step-1-create-hello-distribution-point"></a>Krok 1: Vytvoření hello distribučního bodu
Nejprve musíte umístit hello instalační balíček na síťové umístění, které jsou přístupné hello počítače, u nějž chcete tooremotely instalace hello rozšíření na. toodo, postupujte takto:

1. Přihlaste se jako správce serveru toohello
2. V hello **správce serveru** okně přejděte příliš**soubory a služby úložiště**.
   
    ![Otevřené soubory a služby úložiště](./media/active-directory-saas-ie-group-policy/files-services.png)
3. Přejděte toohello **sdílené složky** kartě. Pak klikněte na tlačítko **úlohy** > **Nová sdílená složka...**
   
    ![Otevřené soubory a služby úložiště](./media/active-directory-saas-ie-group-policy/shares.png)
4. Dokončení hello **Průvodce vytvořením nové sdílené složky** a tooensure sadu oprávnění, aby byla přístupná z vašich uživatelů počítačů. [Další informace o sdílených složek.](https://technet.microsoft.com/library/cc753175.aspx)
5. Stáhněte si následující balíček Instalační služby systému Windows (soubor .msi) hello: [Extension.msi Panel přístupu](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access Panel Extension.msi)
6. Zkopírujte hello instalační program balíčku tooa požadovaného umístění na sdílené složky hello.
   
    ![Zkopírujte sdílenou toohello hello .msi.](./media/active-directory-saas-ie-group-policy/copy-package.png)
7. Ověřte, zda klientské počítače jsou možné tooaccess hello instalační balíček ze sdílené složky hello. 

## <a name="step-2-create-hello-group-policy-object"></a>Krok 2: Vytvoření hello objektu zásad skupiny
1. Přihlaste se na toohello serveru, který je hostitelem vaší instalace služby Active Directory Domain Services (AD DS).
2. Hello správce serveru, přejděte v příliš**nástroje** > **Správa zásad skupiny**.
   
    ![Přejděte tooTools > Managment zásad skupiny](./media/active-directory-saas-ie-group-policy/tools-gpm.png)
3. V levém podokně hello hello **Správa zásad skupiny** okně zobrazení hierarchie organizační jednotky (OU) a určit, ve které oboru chcete zásady skupiny tooapply hello. Například se můžete rozhodnout toopick malé tooa toodeploy organizační jednotky několik uživatelů pro testování, nebo vyberete nejvyšší úrovni organizační jednotky toodeploy tooyour celé organizace.
   
   > [!NOTE]
   > Pokud by jako toocreate nebo upravit organizačních jednotkách (OU), přepněte zpět toohello správce serveru a přejděte příliš**nástroje** > **Active Directory Users and Computers**.
   > 
   > 
4. Jakmile vyberete organizační jednotku, pravým tlačítkem myši a vyberte **vytvořit objekt zásad skupiny v této doméně a propojit jej sem...**
   
    ![Vytvořit nový objekt zásad skupiny](./media/active-directory-saas-ie-group-policy/create-gpo.png)
5. V hello **nový objekt zásad skupiny** řádku, zadejte název pro hello nového objektu zásad skupiny.
   
    ![Název hello nový objekt zásad skupiny](./media/active-directory-saas-ie-group-policy/name-gpo.png)
6. Hello klikněte pravým tlačítkem na objekt zásad skupiny, který jste vytvořili a vyberte **upravit**.
   
    ![Upravit hello nový objekt zásad skupiny](./media/active-directory-saas-ie-group-policy/edit-gpo.png)

## <a name="step-3-assign-hello-installation-package"></a>Krok 3: Přiřaďte hello instalační balíček
1. Určete, jestli chcete rozšíření hello toodeploy na základě **konfigurace počítače** nebo **konfigurace uživatele**. Při použití [konfigurace počítače](https://technet.microsoft.com/library/cc736413%28v=ws.10%29.aspx), rozšíření hello je nainstalovaná na počítači hello bez ohledu na to, které uživatelé přihlásit tooit. S [konfigurace uživatele](https://technet.microsoft.com/library/cc781953%28v=ws.10%29.aspx), uživatelé mají hello rozšíření nainstalovat pro ně bez ohledu na počítače, které se přihlašují.
2. V levém podokně hello hello **Editor správy zásad skupiny** okně přejděte tooeither hello následující cesty ke složce zadat, v závislosti na tom, jaký typ konfigurace jste zvolili:
   
   * `Computer Configuration/Policies/Software Settings/`
   * `User Configuration/Policies/Software Settings/`
3. Klikněte pravým tlačítkem na **instalace softwaru**, pak vyberte **nový** > **balíčku...**
   
    ![Vytvořit nový balíček pro instalaci softwaru](./media/active-directory-saas-ie-group-policy/new-package.png)
4. Přejděte toohello sdílená složka, která obsahuje balíček instalačního programu hello z [krok 1: Vytvoření distribučního bodu hello](#step-1-create-the-distribution-point), vyberte soubor MSI hello a klikněte na tlačítko **otevřete**.
   
   > [!IMPORTANT]
   > Pokud sdílená složka hello je umístěná na tento stejný server, ověřte, zda přistupujete hello .msi prostřednictvím cesta k souboru hello sítě, nikoli hello místní cesta.
   > 
   > 
   
    ![Vyberte hello instalační balíček ze sdílené složky hello.](./media/active-directory-saas-ie-group-policy/select-package.png)
5. V hello **nasazení softwaru** výzvy, vyberte **přiřazeno** pro metodu nasazení. Pak klikněte na **OK**.
   
    ![Vyberte přiřazeno a potom klikněte na tlačítko OK.](./media/active-directory-saas-ie-group-policy/deployment-method.png)

Hello rozšíření je nyní nasazené toohello organizační jednotku, kterou jste vybrali. [Další informace o instalace softwaru zásad skupiny.](https://technet.microsoft.com/library/cc738858%28v=ws.10%29.aspx)

## <a name="step-4-auto-enable-hello-extension-for-internet-explorer"></a>Krok 4: Povolit automatické hello rozšíření pro Internet Explorer
Kromě toho instalačního programu hello toorunning, každou příponu pro Internet Explorer musí být explicitně povoleno před použitím. Hello postupujte podle kroků tooenable hello rozšíření panely přístup pomocí zásad skupiny:

1. V hello **Editor správy zásad skupiny** přejděte tooeither hello následující cesty, v závislosti na tom, jaký typ konfigurace, který jste zvolili v okně [krok 3: přiřaďte hello instalační balíček](#step-3-assign-the-installation-package):
   
   * `Computer Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
   * `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
2. Klikněte pravým tlačítkem na **seznam doplňků**a vyberte **upravit**.
    ![Seznam rozšíření upravte.](./media/active-directory-saas-ie-group-policy/edit-add-on-list.png)
3. V hello **seznam doplňků** vyberte **povoleno**. Potom v části hello **možnosti** klikněte na tlačítko **zobrazit...** .
   
    ![Klikněte na položku Povolit a pak klikněte na zobrazit...](./media/active-directory-saas-ie-group-policy/edit-add-on-list-window.png)
4. V hello **zobrazit obsah** okně provést hello následující kroky:
   
   1. Pro první sloupec hello (hello **název hodnoty** pole), zkopírujte a vložte hello následující ID třídy:`{030E9A3F-7B18-4122-9A60-B87235E4F59E}`
   2. Pro druhý sloupec hello (hello **hodnotu** pole), zadejte hello následující hodnotu:`1`
   3. Klikněte na tlačítko **OK** tooclose hello **zobrazit obsah** okna.
      
      ![Vyplňte hodnoty hello jak je uvedeno výše.](./media/active-directory-saas-ie-group-policy/show-contents.png)
5. Klikněte na tlačítko **OK** tooapply změny a zavřít hello **seznam doplňků** okno.

Hello rozšíření by měl nyní povoleno pro hello počítačů v hello vybrané organizační jednotce. [Další informace o používání tooenable zásad skupiny nebo zakázat rozšíření prohlížeče Internet Explorer.](https://technet.microsoft.com/library/dn454941.aspx)

## <a name="step-5-optional-disable-remember-password-prompt"></a>Krok 5 (volitelné): zakázat "Zapamatovat heslo" řádku
Při mohou uživatelé přihlášení toowebsites pomocí hello rozšíření přístup k panelu, Internet Explorer následující hello zobrazit výzva s dotazem "by vám jako toostore heslo?"

![](./media/active-directory-saas-ie-group-policy/remember-password-prompt.png)

Pokud chcete uživatelům z výzva zobrazila tooprevent, potom postupujte podle kroků hello tooprevent automatické dokončování z zapamatování hesla:

1. V hello **Editor správy zásad skupiny** okno, přejděte toohello cestu uvedené níže. Toto nastavení konfigurace je k dispozici v části pouze **konfigurace uživatele**.
   
   * `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/`
2. Najít nastavení hello s názvem **zapnout hello funkce automatického dokončování pro uživatelská jména a hesla ve formulářích**.
   
   > [!NOTE]
   > Předchozí verze služby Active Directory může zobrazit seznam toto nastavení s názvem hello **zakázat automatické dokončování toosave hesla**. Hello konfigurace tohoto nastavení se liší od hello nastavení popsané v tomto kurzu.
   > 
   > 
   
    ![Nezapomeňte toolook pro to, v části Nastavení uživatele.](./media/active-directory-saas-ie-group-policy/disable-auto-complete.png)
3. Klikněte pravým tlačítkem na hello výše nastavení a vyberte **upravit**.
4. V okně hello s názvem **zapnout hello funkce automatického dokončování pro uživatelská jména a hesla ve formulářích**, vyberte **zakázané**.
   
    ![Vyberte zakázat](./media/active-directory-saas-ie-group-policy/disable-passwords.png)
5. Klikněte na tlačítko **OK** tooapply tyto změny a zavřít hello okno.

Uživatelé budou už možné toostore přihlašovacích údajů nebo použít automatické dokončování tooaccess dříve uložené přihlašovací údaje. Však tato zásada Povolit uživatelům toocontinue toouse automatické dokončování pro jiné typy polí formuláře, například pole hledání.

> [!WARNING]
> Pokud po uživatele vybrali toostore některé přihlašovací údaje, je tato zásada povolena, bude tato zásada *není* vymazat hello přihlašovací údaje, které již byly uloženy.
> 
> 

## <a name="step-6-testing-hello-deployment"></a>Krok 6: Testování hello nasazení
Pokud hello rozšíření nasazení proběhlo úspěšně, postupujte podle kroků hello níže tooverify:

1. Pokud jste nasadili pomocí **konfigurace počítače**, přihlaste se klientský počítač, který patří toohello organizační jednotku, kterou jste vybrali v [krok 2: vytvoření objektu zásad skupiny hello](#step-2-create-the-group-policy-object). Pokud jste nasadili pomocí **konfigurace uživatele**, ujistěte se, že toosign v jako uživatel, který patří toothat organizační jednotky.
2. Může trvat několik sign in pro zásady skupiny hello změny toofully aktualizace se tento počítač. tooforce hello aktualizace, otevřete **příkazového řádku** okno a spuštění hello následující příkaz:`gpupdate /force`
3. Je nutné restartovat počítač hello pro místní tootake instalace hello. Spuštění může trvat výrazně déle než obvykle při rozšíření hello nainstaluje.
4. Po restartování počítače, otevřete **Internet Explorer**. V pravém horním rohu hello hello okna, klikněte na tlačítko **nástroje** (ozubené kolečko ikona hello) a potom vyberte **spravovat doplňky**.
   
    ![Přejděte tooTools > Spravovat doplňky](./media/active-directory-saas-ie-group-policy/manage-add-ons.png)
5. V hello **spravovat doplňky** okno, ověřte, že hello **rozšíření přístup k panelu** byla nainstalována a že jeho **stav** byl nastaven příliš**povoleno**.
   
    ![Ověřte, že hello rozšíření přístup k panelu je nainstalovaný a povolený.](./media/active-directory-saas-ie-group-policy/verify-install.png)

## <a name="related-articles"></a>Související články
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
* [Přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md)
* [Řešení potíží s hello rozšíření panely přístup pro prohlížeč Internet Explorer](active-directory-saas-ie-troubleshooting.md)

