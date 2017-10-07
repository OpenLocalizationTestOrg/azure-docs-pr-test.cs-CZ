---
title: "aaaHow toouse hello Azure Active Directory Power BI balíček obsahu | Microsoft Docs"
description: "Zjistěte, jak toouse hello Azure Active Directory Power BI balíček obsahu"
services: active-directory
author: MarkusVi
manager: femila
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: d07d678aedbe3089c4ea5f981f72311bdb389a17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-azure-active-directory-power-bi-content-pack"></a>Jak toouse hello Azure Active Directory Power BI balíček obsahu

Pro vás jako správce IT je naprosto nezbytné pochopit, jakým způsobem uživatelé přijímají a používají funkce služby Azure Active Directory. Umožňuje vám tooplan vaší IT infrastruktury a komunikace tooincrease využití a tooget hello nejvíce mimo funkce AAD. Pack obsah Power BI pro Azure Active Directory poskytuje hello toofurther možnost analyzovat vaše data toounderstand použití tato data toogather bohatší přehledy co se děje s Azure Active Directory pro hello různé možnosti můžete výraznou spoléhají na.  S hello integrace Azure Active Directory, rozhraní API do Power BI můžete snadno stáhnout hello balíčky předem připraveného obsahu a získáte přehled o tooall hello aktivity v rámci služby Azure Active Directory pomocí Power BI nabízí bohaté vizualizace prostředí. Můžete si vytvořit vlastní řídicí panel a snadno ho sdílet se všemi uživateli ve vaší organizaci. 

Toto téma poskytuje vám podrobný návod, jak tooinstall a používání hello obsah pack ve vašem prostředí.

## <a name="installation"></a>Instalace  

**tooinstall hello balíček obsahu Power BI:**

1. Přihlaste se k [Power BI](https://app.powerbi.com/groups/me/getdata/services) pomocí účtu Power BI (Toto je hello stejný účet jako O365 nebo účtu služby Azure AD).

2. V dolní části hello hello levém navigačním podokně, vyberte **načíst Data**.

    ![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/01.png)
 
3. V hello **služby** pole, klikněte na tlačítko **získat**.
   
    ![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/02.png)

4.  Vyhledejte **Azure Active Directory**.

    ![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/03.png)
 
5.  Po zobrazení výzvy zadejte ID klienta služby Azure AD a pak klikněte na **Další**.

    > [!TIP] 
    > Hello tooget rychlý způsob Id klienta pro vašeho tenanta Office 365 / Azure AD je toologin toohello portálu Azure AD, Procházet adresář toohello a zkopírujte hello ID z hello následující adresu URL: https://manage.windowsazure.com/woodgroveonline.com#Workspaces/ ActiveDirectoryExtension/Directory/<tenantid>/directoryQuickStart

    ![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/04.png) 

6.  Klikněte na **Přihlášení**. 
 
    ![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/05.png) 



7.  Zadejte uživatelské jméno a heslo a pak klikněte na **Přihlášení**.
 
    ![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/06.png) 

8.  V dialogovém okně souhlasu hello aplikace, klikněte na tlačítko **přijmout**.
 
9.  Jakmile řídicí panel protokolů aktivity služby Azure Active Directory vytvoříte, klikněte na něj.
 
    ![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/08.png) 

## <a name="what-can-i-do-with-this-content-pack"></a>Co můžu s tímto balíčkem obsahu dělat?

Před jsme přejít do co můžete dělat s Tento balíček obsahu, zde uvádíme rychlý náhled hello různé sestavy v obsahu hello aktualizací Service pack. Sestava dat přejde zpět toohello **posledních 30 dní**.

### <a name="reports-included-in-this-version-of-azure-active-directory-logs-content-pack"></a>Sestavy zahrnuté v této verzi balíčku obsahu protokolů služby Azure Active Directory

**Sestavy využití aplikací a Trend**: získat přehled o hello aplikace použitý v organizaci a ty, které jsou používány hello nejvíce a kdy. Můžete použít tuto sestavu toogather přehledy využití aplikace, které se nedávno nasazen ve vaší organizaci nebo zjistit, které aplikace jsou oblíbených. Díky tomu může zvýšit využití, pokud se zobrazí, pokud aplikace hello nepoužívá.

**Přihlášení podle umístění a uživatelé**: získat přehled o všech přihlášení hello provádí pomocí Azure Identity a poskytuje přehled o hello identity uživatelů hello. Můžete podrobněji analyzovat individuální přihlášení a odpovědět si na otázky jako:

- Odkud se tento uživatel přihlásil?
- Které má uživatel hello většina přihlášení a kde budou přihlášení z? 
- Úspěšné přihlášení hello?  
 
Podrobnosti můžete rozbalit kliknutím na konkrétní datum nebo umístění.

**Jedineční uživatelé na aplikaci**: Získejte přehled všech jedinečných uživatelů, kteří danou aplikaci využívají. Patří sem pouze uživatelé, kteří se k aplikaci *úspěšně* přihlásili.

**Přihlášení zařízení**: získat zobrazení hello typ operačního systému a prohlížeče jsou používány uživatelé ve vaší organizaci s podrobné informace o hello uživatelů, včetně:

- Uživatelské jméno
- IP adresa
- Umístění 
- Stav přihlášení 

U této sestavy můžete porozumět hello různé profily zařízení používá v rámci vaší organizace a určete, podle toho, co je použije zásady zařízení

**Trychtýř SSPR**: Získejte představu o tom, jakým způsobem se ve vaší organizaci resetují hesla. Získání funkce Náhled na tom, kolik heslo resetovat pokusů o pomocí nástroje SSPR hello a kolik z nich byly úspěšné. Chyby resetování hesla hello pomocí hello SSPR trychtýřového podrobněji a pochopit, proč došlo k určitým chybám. Tato sestava umožňuje lépe pochopili, jak nástroj SSPR hello se používá v rámci vaší organizace, abyste měli hello správné rozhodnutí.

## <a name="customizing-azure-ad-activity-content-pack"></a>Přizpůsobení balíčku obsahu aktivit služby Azure AD

**Změnit vizualizace**: vizualizace sestavy můžete změnit kliknutím **upravit sestavu** a vyberte hello vizualizace chcete.
 
![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/09.png) 
 
![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/10.png) 

**Zahrnout další pole**: můžete přidat sestavu toohello pole nebo ho odebrat výběrem hello visual toowhich chcete pole hello tooadd nebo odebrat. V příkladu hello níže přidám zobrazení tabulky toohello pole "stav přihlášení". 
 
![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/11.png) 

**Řídicí panel tooyour vizualizace PIN**: přizpůsobit svůj řídicí panel, patří vlastní sestavu toohello vizualizace a připnout toohello řídicího panelu. V příkladu hello níže I přidán nový filtr, který se nazývá "stav přihlášení" a zahrnuty v sestavě hello. I taky hello vizualizace se změnil z pruhový graf tooa spojnicový graf a můžete Připnout tento nový visual toohello řídicí panel.

![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/12.png) 

![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/13.png) 
 

 


**Sdílení řídicího panelu**: Po vytvoření hello obsah, který chcete, můžete sdílet hello řídicí panel s hello uživateli ve vaší organizaci. Pamatovat si prosím, že jakmile hello sestavy sdílíte, uvidí vybraných v sestavě hello polí hello.
 
![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/14.png) 



## <a name="scheduling-a-daily-refresh-of-your-power-bi-report"></a>Plánování denní aktualizace sestavy Power BI

tooschedule denní aktualizaci sestavy Power BI, přejděte příliš**datové sady > Nastavení > naplánovat aktualizaci** a nastavte ji, jak je uvedeno níže.
 
![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/15.png) 

## <a name="updating-toonewer-version-of-content-pack"></a>Aktualizace toonewer verze obsahu sady

Pokud chcete, aby tooupdate obsah pack tooget novější verze:

- Stáhněte si nový balíček obsahu hello a nastavení podle pokynů uvedených v tomto článku.

- Jakmile jste nastavili tak, přejděte příliš**zdroj dat > Nastavení > přihlašovací údaje ke zdroji dat** a znova zadejte přihlašovací údaje, jak je uvedeno níže

    ![Balíček obsahu Azure Active Directory Power BI Content Pack](./media/active-directory-reporting-power-bi-content-pack-how-to/16.png) 

Jakmile hello novou verzi balíčku obsahu hello funguje, můžete odebrat předchozí verzi aplikace hello v případě potřeby odstraněním hello základní sestavy a datové sady přidružené k tento balíček obsahu.

## <a name="still-having-issues"></a>Pořád máte problémy? 

Podívejte se na [průvodce odstraňováním potíží](active-directory-reporting-troubleshoot-content-pack.md). Obecnou nápovědu k Power BI najdete v těchto [článcích nápovědy](https://powerbi.microsoft.com/en-us/documentation/powerbi-service-get-started/).
 

## <a name="next-steps"></a>Další kroky

Přehled vytváření sestav najdete v tématu hello [generování sestav Azure Active Directory](active-directory-reporting-azure-portal.md).
