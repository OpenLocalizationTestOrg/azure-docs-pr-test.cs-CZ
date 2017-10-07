---
title: "vaše přihlášení stránku hello Azure Active Directory aaaCustomize | Microsoft Docs"
description: "Zjistěte, jak tooadd na stránce firemního brandingu toohello Azure přihlášení"
services: active-directory
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: f8b932bc-8b4f-42b5-a2d3-f2c076234a78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: jeffgilb
custom: it-pro
ms.openlocfilehash: 7a7ccdeef0764f6cf9e9e224acd4350983031fe0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="quickstart-add-company-branding-tooyour-sign-in-page-in-azure-ad"></a>Rychlé spuštění: Přidání firemního brandingu přihlašovací stránky tooyour ve službě Azure AD
tooavoid nedorozuměním mnoho společností má tooapply konzistentní vzhled a chování ve všech hello webů a služeb, které spravují. Azure Active Directory (Azure AD) umožňuje používat tuto funkci tak, že umožní toocustomize hello vzhled hello přihlašovací stránka s svoje firemní logo a vlastní barevná schémata. přihlašovací stránku Hello je hello stránka, která se zobrazí při přihlášení tooOffice 365 nebo jiné webové aplikace, které používají Azure AD jako zprostředkovatele identity. Budete používat tuto stránku tooenter přihlašovacích údajů.

> [!NOTE]
> * Firemní branding je k dispozici pouze v případě, že jste upgradovali toohello Premium nebo Basic edice Azure AD, nebo mít licenci pro Office 365. toolearn-li funkci podporován typ vaší licence zkontrolujte hello [stránce s cenami. informace o službě Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).
> 
> * Edice Azure Active Directory Premium a Basic jsou k dispozici zákazníkům v Číně pomocí hello celosvětové instance služby Azure Active Directory. Edice Azure Active Directory Premium a Basic nejsou aktuálně podporované ve službě Microsoft Azure hello provozované v Číně společností 21Vianet. Další informace, kontaktujte nás na hello [fóru služby Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/).

## <a name="customizing-hello-sign-in-page"></a>Přizpůsobení přihlašovací stránku hello

<!--You can customize hello following elements on hello sign-in page: <attach image>-->

Firemního brandingu přizpůsobení zobrazí na přihlašovací stránku hello Azure AD, když uživatelé přistupují k adresy URL konkrétního klienta, jako [ *https://outlook.com/contoso.com*](https://outlook.com/contoso.com).

Když uživatelé navštíví obecné adresy URL, například www.office.com, neobsahuje přihlašovací stránku hello firemního brandingu přizpůsobení, protože hello systému nebude vědět, kdo je hello uživatele. Firemní branding zobrazí po uživatele zadejte své ID uživatele nebo vyberte dlaždici uživatele.

> [!NOTE]
> * Název domény musí zobrazovat jako "Aktivní" v hello **domén** část hello portálu Azure, ve kterém jste branding nakonfigurovali. Další informace najdete v tématu [přidání vlastního názvu domény](add-custom-domain.md).
> * Branding přihlašovací stránky se nepřenáší toohello přihlašovací stránku pro osobní účty Microsoft. Pokud zaměstnancům nebo obchodní hosté, přihlaste se pomocí osobního účtu Microsoft, neodráží jejich přihlašovací stránku hello branding vaší organizace.


### <a name="banner-logo"></a>Banner s logem 

Popis | Omezení | Doporučení
------- | ------- | ----------
Hello banner s logem se zobrazí na hello přihlásit a na stránkách panely hello.<br>Na hello přihlašovací stránka zobrazí po hello uživatele organizaci je určen, obvykle po hello uživatelské jméno.  | Transparentní JPG nebo PNG<br>Maximální výška: 36 px<br>Maximální šířka: 245 px | Tady použijte logo vaší organizace.<br>Použijte průhledný obrázek. Nepředpokládejte, že bude bílé pozadí hello.<br>Nepřidávejte výplně kolem vaše logo v bitové kopii hello nebo vaše logo bude vypadat nepřiměřeně malé.

### <a name="username-hint"></a>Pomocný parametr uživatelského jména   
Popis | Omezení | Doporučení
------- | ------- | ----------
To umožní vlastní nastavení hello pomocný parametr text v poli hello uživatelské jméno. | Text v kódu Unicode až too64 znaků<br>Jenom prostý text | Doporučujeme vám, že není nastaveno to pokud očekáváte mimo vaši organizaci toosign v aplikaci tooyour uživatele typu Host.
            
### <a name="sign-in-page-text"></a>Text na přihlašovací stránce   
Popis | Omezení | Doporučení
------- | ------- | ----------
Zobrazuje se v dolní části hello hello přihlášení formuláře a může být použité toocommunicate Další informace, jako je hello telefonní číslo tooyour technické podpory nebo právní prohlášení. | Text v kódu Unicode až too256 znaků<br>Jenom prostý text (žádné odkazy nebo značky jazyka HTML). 

### <a name="sign-in-page-image"></a>Obrázek přihlašovací stránky  
Popis | Omezení | Doporučení
------- | ------- | ----------
To se zobrazí v pozadí hello hello přihlašovací stránky, je ukotvené toohello center hello zobrazitelné místa a bude škálování a oříznout toofill hello okno prohlížeče.  <br>Na úzkých obrazovkách například mobilních telefonů se nezobrazí tuto bitovou kopii.<br>Černé maska s 0.55 krytí uplatní přes tuto bitovou kopii kódem našeho při načtení stránky hello. | JPG nebo PNG<br>Obrázek dimenze: 1920 × 1080 pixelů<br>Velikost souboru: &gt; 300 KB | <br>Používat Image místě, kde není silné subjektu fokus. Hello neprůhledného přihlašovací formulář se zobrazí nad hello středu tuto bitovou kopii a může zahrnovat libovolnou část hello bitové kopie v závislosti na velikosti hello hello okna prohlížeče.<br>Zachovejte velikost souboru hello jako možné tooensure rychlé zatížení časy. 

### <a name="background-color"></a>Barva pozadí
Popis | Omezení | Doporučení
------- | ------- | ----------
Tato barva použijí se místo hello obrázek pozadí na připojení s malou šířkou pásma. |   Barva RGB v šestnáctkové soustavě (Příklad: #FFFFFF | Doporučujeme pomocí hello primární barvu hello banner s logem nebo barva vaší organizace.

### <a name="show-option-tooremain-signed-in"></a>Zobrazit tooremain možnost přihlášení
Popis | Omezení | Doporučení
------- | ------- | ----------
Azure AD přihlášení poskytuje hello uživatele hello možnost tooremain podepsané případě, že zavřete a znovu ho otevřete svého prohlížeče. Pomocí této toohide tuto možnost.<br>Tuto možnost nastavíte příliš "žádný" toohide tuto možnost od uživatelů. | &nbsp; | Toto nastavení neovlivní dobu platnosti relace.<br>Některé funkce služby SharePoint Online a Office 2010, závisí na uživatel moct toochoose tooremain přihlášení. Pokud nastavíte tento toobe skrytá, může se uživatelům zobrazí další a neočekávané výzvy toosign v.

> [!NOTE]
> Všechny prvky jsou volitelné. Například pokud určíte banner s logem se žádný obrázek na pozadí, hello přihlašovací stránka zobrazí vaše obrázek pozadí logo a hello hello cílové lokality (například Office 365).

## <a name="add-company-branding-tooyour-directory"></a>Přidání firemního brandingu tooyour adresáře

1. Přihlaste se příliš[hello portál Azure](https://portal.azure.com) pomocí účtu, který je globálním správcem adresáře hello.
2. Vyberte **další služby**, zadejte **uživatelů a skupin** v hello textového pole a pak vyberte **Enter**.

   ![Správa uživatelů otevírání](./media/active-directory-branding-custom-signon-azure-portal/user-management.png)
3. Na hello **uživatelů a skupin** vyberte **firemní branding**.
4. Na hello **uživatelé a skupiny - firemní branding** okně, vyberte hello **upravit** příkaz.

    ![Upravit vlastní branding](./media/active-directory-branding-custom-signon-azure-portal/edit-branding.png)
5. Upravte prvky hello chcete toocustomize. Všechny prvky jsou volitelné.
6. Klikněte na **Uložit**.

Může to trvat až hodinu tooan pro všechny změny provedené toohello přihlašovací stránka brandingu tooappear.

## <a name="add-language-specific-company-branding-tooyour-directory"></a>Přidání konkrétní jazyk firemního brandingu tooyour adresáře

1. Přihlaste se toohello [centra pro správu Azure AD](https://aad.portal.azure.com) pomocí účtu, který je globálním správcem adresáře hello.
2. Vyberte **uživatelů a skupin** v hello textového pole a pak vyberte **Enter**.

   ![Správa uživatelů otevírání](./media/active-directory-branding-localize-azure-portal/user-management.png)
3. Na hello **uživatelů a skupin** vyberte **firemní branding**.
4. Na hello **uživatelé a skupiny - firemní branding** okně, vyberte hello **přidat jazyk** příkaz.

    ![Přidání brandingu elementy jazyka](./media/active-directory-branding-localize-azure-portal/add-language.png)
5. Upravte prvky hello chcete toocustomize. Všechny prvky jsou volitelné.
6. Klikněte na **Uložit**.

Může to trvat až hodinu tooan pro všechny změny provedené toohello přihlašovací stránka brandingu tooappear.

## <a name="next-steps"></a>Další kroky
V tento rychlý start když jste se naučili jak tooadd firemního brandingu directory tooyour Azure AD. 

Můžete použít hello následující odkaz tooconfigure vašeho firemního brandingu ve službě Azure AD z hello portálu Azure.

> [!div class="nextstepaction"]
> [Konfigurace značky společnosti](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/LoginTenantBrandingBlade) 