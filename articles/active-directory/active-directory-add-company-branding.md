---
title: "aaaAdd firemního brandingu tooyour přihlásit a přístupového panelu"
description: "Zjistěte, jak tooadd na stránce firemního brandingu toohello Azure přihlášení a hello přistupovat k panelu stránky"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: f74621b4-4ef0-4899-8c0e-0c20347a8c31
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/23/2017
ms.author: curtand
ms.openlocfilehash: b1c6442028393552bff3d380312c8cd1bfda5d31
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-company-branding-tooyour-sign-in-and-access-panel-pages"></a>Přidání firemního brandingu tooyour přihlásit a přístupového panelu
tooavoid nedorozuměním mnoho společností má tooapply konzistentní vzhled a chování ve všech hello webů a služeb, které spravují. Tato funkce poskytuje Azure Active Directory tak, že umožní toocustomize hello vzhled hello následující webové stránky s svoje firemní logo a vlastní barevná schémata:

* **Přihlašovací stránka** -Toto je stránka hello, který se zobrazí při přihlášení tooOffice 365 nebo jiné webové aplikace, které používají Azure AD jako zprostředkovatele identity. Budete používat tuto stránku při vyhledávání domovské sféry nebo tooenter přihlašovacích údajů. Hello zjišťování domovské sféry umožňuje hello systému tooredirect federovaných uživatelů tootheir místní služby tokenů zabezpečení (například služby AD FS).
* **Stránka přístupového panelu** – hello přístupový Panel je webový portál, který vám umožní tooview a spuštění hello cloudové aplikace vám správce Azure AD udělil přístup. tooaccess hello přístupový Panel hello použijte následující adresu URL: [https://myapps.microsoft.com](https://myapps.microsoft.com).

Toto téma vysvětluje, jak můžete přizpůsobit přihlašovací stránku hello a stránka přístupového panelu hello.

> [!NOTE]
> * Firemní branding je funkce, která je k dispozici pouze v případě, že jste upgradovali toohello Premium nebo Basic edice služby Azure Active Directory nebo Office 365 uživatelem. Další informace najdete v článku [Edice služby Azure Active Directory](active-directory-editions.md).
> * Edice Azure Active Directory Premium a Basic jsou k dispozici zákazníkům v Číně pomocí hello celosvětové instance služby Azure Active Directory. Edice Azure Active Directory Premium a Basic nejsou aktuálně podporované ve službě Microsoft Azure hello provozované v Číně společností 21Vianet. Další informace, kontaktujte nás na hello [fóru služby Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/).
>
>

## <a name="customizing-hello-sign-in-page"></a>Přizpůsobení přihlašovací stránku hello
Pokud potřebujete přístup založené na prohlížeči tooyour cloudových aplikací a služeb, které vaše organizace předplatila, obvykle použijete přihlašovací stránku hello.

Pokud jste použili změny tooyour přihlašovací stránky, může trvat až hodinu tooan tooappear změny hello.

Přihlašovací stránka ve vaší firemní úpravě se zobrazí jenom tehdy, když službu navštívíte pomocí adresy URL konkrétního klienta, například https://outlook.com/**contoso**.com nebo https://mail.**contoso**.com.

Když službu navštívíte pomocí adresy URL, která se neváže ke konkrétnímu klientu (např: https://mail.office365.com), zobrazí se přihlašovací stránka bez firemní úpravy. V tomto případě se branding zobrazí až potom, co zadáte ID uživatele nebo vyberete dlaždici uživatele.

> [!NOTE]
> * Název domény musí zobrazovat jako "Aktivní" v hello **služby Active Directory** > **Directory** > **domén** části hello portál Azure classic kde jste branding nakonfigurovali.
> * Branding přihlašovací stránky se nepřenáší toohello spotřebitelskou přihlašovací stránku společnosti Microsoft. Pokud se přihlásíte pomocí osobního účtu Microsoft, mohou se zobrazit seznam uživatelských dlaždic vykreslí Azure AD partnerské, ale hello branding vaší organizace se nevztahuje toohello stránky účtu Microsoft přihlásit.
>
>

Pokud chcete tooshow vaší společnosti značku, barvy a další přizpůsobitelné prvky na této stránce, najdete v části hello následující obrázky toounderstand hello rozdíl mezi oběma prostředími hello.

Následující snímek obrazovky ukazuje příklad stránky přihlašovací hello Office 365 na stolním počítači a Hello **před** přizpůsobení:

![Přihlašovací stránka Office 365 před přizpůsobením][1]

Následující snímek obrazovky ukazuje příklad stránky přihlašovací hello Office 365 na stolním počítači a Hello **po** přizpůsobení:

![Přihlašovací stránka Office 365 po přizpůsobení][2]

Hello následující snímek obrazovky ukazuje příklad přihlašovací stránky hello Office 365 na mobilním zařízení **před** přizpůsobení:

![Přihlašovací stránka Office 365 před přizpůsobením][3]

Hello následující snímek obrazovky ukazuje příklad přihlašovací stránky hello Office 365 na mobilním zařízení **po** přizpůsobení:

![Přihlašovací stránka Office 365 po přizpůsobení][4]

Když změníte velikost okna prohlížeče, hello velký obrázek, jako je hello jeden uvedený výše, se často ořezává tooaccommodate poměrům stran různých obrazovek. Myslete na to že byste měli zkusit tookeep hello klíčové vizuální prvky obrázku hello tak, že budou vždy zobrazovat v levém horním rohu hello (pravém jazyků zprava doleva). To je důležité, protože změna velikosti obvykle dochází z pravého dolního rohu probíhající hello směrem hello horní a doleva nebo dolní hello směrem k horním hello.

Hello následující obrázek ukazuje, jak je obrázek hello oříznout změněnou toohello zbývající po hello prohlížeče:

![][6]

Zde je zobrazení po změně velikosti prohlížeče hello směrem k horním hello:

![][7]

## <a name="what-elements-on-hello-page-can-i-customize"></a>Přizpůsobitelné prvky na stránku hello můžete přizpůsobit?
Můžete přizpůsobit hello na přihlašovací stránku hello následující prvky:

![][5]

| Prvek stránky | Umístění na stránku hello |
|:--- | --- |
| Banner s logem |Zobrazuje se v pravém horním hello hello stránky. Nahradí hello logo hello cílové lokality, které se přihlašujete toodisplays (např. Office 365 nebo Azure). |
| Velký obrázek / barva pozadí |Zobrazuje se v levé hello hello stránky. Nahradí hello image hello cílové lokality, které se přihlašujete toodisplays. Hello barva pozadí se může zobrazit místo velkého obrázku hello u připojení s malou šířkou pásma nebo na úzkých obrazovkách. |
| Zůstat přihlášeni |V části textového pole hesla hello. |
| Text na přihlašovací stránce |Pokud budete potřebovat tooconvey užitečné informace před Přihlaste se pomocí pracovního nebo školního účtu, odstavcích hello zápatí stránky. Můžete například tooinclude hello telefonní číslo tooyour technické podpory nebo právní prohlášení. |

> [!NOTE]
> Všechny prvky jsou volitelné. Například pokud určíte Banner s logem, ale žádný velký obrázek, přihlašovací stránku hello zobrazí vaše logo a hello obrázku pro hello cílové lokality (tedy hello Office 365 obrázek kalifornské dálnice).
>
>

Na stránku přihlášení hello **zůstat přihlášeni** políčko umožňuje uživatele tooremain, při jejich zavřete a znovu ho otevřete svého prohlížeče přihlášený. Na životnost relace to vliv nemá. Můžete skrýt hello zaškrtávací políčko je na hello Azure Active Directory přihlašovací stránky.

Jestli se zobrazí zaškrtávací políčko hello závisí na nastavení hello **skrýt KMSI**.

![][9]

toohide hello zaškrtávací políčko, nakonfigurujte toto nastavení příliš**Hidden**.

> [!NOTE]
> Některé funkce služby SharePoint Online a Office 2010, závisí na uživatele, je možné toocheck toto políčko. Pokud nakonfigurujete toto nastavení toohidden, může se uživatelům zobrazí další a neočekávané výzvy toosign v.
>
>

Všechny prvky na této stránce můžete lokalizovat. Po konfiguraci „výchozí“ sady prvků přizpůsobení můžete nakonfigurovat další verze pro různá národní prostředí. Různé prvky mezi sebou můžete kombinovat. Můžete například provést následující věci:

* Vytvořte „výchozí“ velký obrázek, který je použitelný pro všechny kultury, a potom vytvořte specifické verze pro angličtinu a francouzštinu. Při nastavení vašeho prohlížeče tooone z těchto dvou jazyků hello konkrétní image se zobrazí, když u všech ostatních jazyků se zobrazí výchozí obrázek hello.
* Nakonfigurujte různá loga vaší organizace (například verze pro japonštinu nebo hebrejštinu).

## <a name="access-panel-page-customization"></a>Přizpůsobení stránky přístupového panelu
stránka přístupového panelu Hello je v podstatě stránkou portálu, která pro rychlý přístup toohello cloudové aplikace, které máte přístup k tooby správce. Na této stránce se vaše aplikace zobrazují jako dlaždice aplikací, na které můžete kliknout.

Hello následující snímek obrazovky ukazuje příklad stránky přístupového panelu po přizpůsobení.

![][8]

## <a name="configure-your-directory-with-company-branding"></a>Nastavení konfigurace adresáře pomocí firemního brandingu
Můžete nakonfigurovat jednu výchozí sadu přizpůsobitelných prvků každému adresáři v hello portál Azure classic. Po uložení hello výchozí nastavení, může správce přidat lokalizované verze jednotlivých prvků pro různé jazyky nebo národní prostředí. Všechny přizpůsobitelné prvky jsou volitelné.

Například pokud nakonfigurujete výchozí Banner s logem, ale žádný velký obrázek, hello přihlašovací stránka zobrazí vaše logo v pravém horním rohu hello. Zobrazí se ale hello výchozí obrázek webu hello.

Představte si hello následující konfigurace:

* Výchozí banner s logem a text přihlašovací stránky v angličtině
* Text přihlašovací stránky přizpůsobený pro němčinu

Pokud je vaším preferovaným jazykem němčina, získáte hello výchozí Banner s logem, ale hello německým textem.

Když technicky je možné nakonfigurovat sadu pro každý jazyk podporovaný službou Azure AD, doporučujeme, abyste hello počet variant – z nízké, údržby a lepšího výkonu.

> [!IMPORTANT]
> Yammer nemá zobrazit hello Azure AD rámci přihlašovací stránky až po přihlášení uživatele hello. Hello uživateli se zobrazí přihlašovací stránky Office 365 je obecný hello nejprve, a pak hello značky stránky, po který.   
 
 
**tooadd firemního brandingu tooyour adresáře, proveďte následující kroky hello:**

1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com) jako správce adresáře hello chcete toocustomize.
2. Vyberte adresář hello chcete toocustomize.
3. V panelu nástrojů hello hello nahoře, klikněte na **konfigurace**.
4. Klikněte na **Přizpůsobit branding**.
5. Upravte prvky hello chcete toocustomize. Všechna pole jsou volitelná.
6. Klikněte na **Uložit**.

Může to trvat až hodinu tooan pro nové změny můžete provedeny toohello přihlašovací stránky branding tooappear.

**tooadd konkrétní jazyk firemní branding, proveďte následující kroky hello:**

1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com) jako správce adresáře hello chcete toocustomize.
2. Vyberte adresář hello chcete toocustomize.
fs3. V panelu nástrojů hello hello nahoře, klikněte na **konfigurace**.
4. Klikněte na **Přizpůsobit branding**.
5. Klikněte na **Přidat branding pro konkrétní jazyk**.
6. Vyberte jazyk hello toocustomize hello logo a pak klikněte na **Další**.
7. Upravte jenom hello prvky, pro které chcete přepsání tooconfigure konkrétní jazyk. Všechna pole jsou volitelná. Pokud pole necháte prázdné, pak hello vlastní výchozí hodnota se zobrazí místo (nebo Microsoft výchozí text hello, pokud není nakonfigurováno vlastní výchozí).
8. Klikněte na **Uložit**.

**tooremove firemního brandingu z adresáře, proveďte následující kroky hello:**

1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com) jako správce adresáře hello chcete toocustomize.
2. Vyberte adresář hello chcete toocustomize.
3. V panelu nástrojů hello hello nahoře, klikněte na **konfigurace**.
4. Klikněte na **Přizpůsobit branding**.
5. Na stránce Přizpůsobit Branding hello vyberte **upravit nastavení existujícího brandingu** a potom přejděte na další stránku toohello.
6. V závislosti na tom, jaký prvek chcete tooremove, proveďte jednu nebo více hello následující:

    a. V části **Banner s logem** vyberte možnost **Odebrat nahrané logo**.

    b. V části **Dlaždice s logem** vyberte možnost **Odebrat nahrané logo**.

    c. Odeberte hello text ze všech textových polí.

    d. Klikněte na **Další**.

    e. Odeberte hello text ze všech textových polí.
7. Klikněte na tlačítko **Uložit** tooremove hello elementy.
8. V případě potřeby klikněte na tlačítko **přizpůsobit Branding** znovu a opakujte tyto kroky pro všechny branding pro konkrétní jazyk vyžadující toobe odebrat.
    Všechna nastavení brandingu se odebraly po kliknutí na tlačítko **přizpůsobit Branding** a zobrazit hello **přizpůsobit výchozí Branding** formuláře s žádnou konfiguraci nastavení.

## <a name="testing-and-examples"></a>Testování a příklady
Před provedením změn v produkčním prostředí doporučujeme nejprve experimenty s testovacím klientem.

**tooverify jestli váš branding byl použit:**

1. Otevřete relaci prohlížeče InPrivate nebo Incognito.
2. Navštivte https://outlook.com/contoso.com, nahraďte contoso.com hello domény, kterou jste přizpůsobili.

Tento postup funguje i s doménami, které mají tvar contoso.onmicrosoft.com.

toohelp vytvoříte nastaví efektivní přizpůsobení, přizpůsobili jsme hello následující dvě fiktivní přihlašovací stránky:

* [http://aka.ms/aaddemo001](http://aka.ms/aaddemo001)
* [http://aka.ms/aaddemo002](http://aka.ms/aaddemo002)

nastavení pro konkrétní jazyk hello tootest, musíte toomodify hello výchozí jazykové předvolby v jazyce tooa webového prohlížeče, které jste nastavili ve vašem vlastním přizpůsobení. V Internet Exploreru můžete konfiguraci provést v hello **Možnosti Internetu** nabídky.

## <a name="customizable-elements"></a>Přizpůsobitelné prvky
Některé přizpůsobitelné prvky v Azure AD mají více možností použití. Můžete nakonfigurovat loga společnosti jednou za adresáře a se používá na přihlášení hello i přístupového panelu. Některé přizpůsobitelné prvky jsou konkrétní pouze toohello přihlašovací stránky. Hello následující tabulka poskytuje podrobnosti pro hello různých přizpůsobitelných prvcích.

| Name (Název) | Popis | Omezení | Doporučení |
| --- | --- | --- | --- |
| Banner s logem |Hello Banner s logem se zobrazuje na přihlašovací stránku hello a hello přístupového panelu. |<p>JPG nebo PNG</p><p>60x280 pixelů</p><p>10 kB</p> |<p>Použijte celé logo vaší organizace (včetně piktogramu a logotypu).</p><p>Dodržte maximální výšku 30 pixelů vysoké tooavoid nezobrazovaly posuvníky na mobilních zařízeních</p><p>Dodržte maximální velikost 4 kB.</p><p>Použijte průhledný obrázek PNG (Nepředpokládejte Tento přihlašovací stránku hello má vždy bílé pozadí)</p> |
| Dlaždice s logem |(aktuálně nepoužívá v přihlašovací stránku hello) V budoucích hello tento text může být použité tooreplace hello obecný "pracovní nebo školní účet" piktogramu na různých místech prostředí hello. |<p>JPG nebo PNG</p><p>120x120 pixelů</p><p>10 kB</p> |<p>Udržte to jednoduché (žádný drobný text), jak tuto bitovou kopii, může být změněnou too50 % |
| </p> | | | |
| Popisek uživatelského jména na přihlašovací stránce |(aktuálně nepoužívá v přihlašovací stránku hello) V budoucích hello tento text může být použité tooreplace hello obecný "pracovní nebo školní účet" řetězec na různých místech prostředí hello. Je možné ji nastavit toosomething jako "Účet Contoso" nebo "Contoso ID". |<p>Text v kódu Unicode, až too50 znaků</p><p>Jenom prostý text (žádné odkazy nebo značky jazyka HTML).</p> |<p>Pište krátce a jednoduše.</p><p>Požádejte uživatele, jak se obvykle odkazovat toohello pracovní nebo školní účet, který jim poskytujete.</p> |
| Text na přihlašovací stránce |Tento text "standardní" se zobrazí níže uvedeného formuláře přihlašovací stránku hello a může být použité toocommunicate dalších pokynů nebo kde tooget Nápověda a podpora. |<p>Text v kódu Unicode, až too256 znaků</p><p>Jenom prostý text (žádné odkazy nebo značky jazyka HTML).</p> |Dodržte maximální délku textu 250 znaků (přibližně tři řádky textu). |
| Obrázek na přihlašovací stránce |Obrázek Hello je velký obrázek, který se zobrazí na levé straně toohello přihlašovací stránku hello formuláře přihlašovací stránku hello. |<p>JPG nebo PNG</p><p>1420×1200</p><p>500 kB</p> |<p>1420×1200 pixelů</p><p>Důležité: Pokuste se udržet co nejmenší, ideálně do 200 kB. Pokud je obrázek příliš velký, je ovlivněn výkon hello hello přihlašovací stránky, když hello obrázek není načtený v mezipaměti</p><p>Tento obrázek se často ořezává, tooaccommodate poměrům stran různých obrazovek. Vizuální prvky hello mějte hello levém horním rohu (pro jazyky psané zprava doleva vpravo nahoře), protože změna velikosti obvykle v pravém dolním rohu hello, postupuje směrem hello horní a doleva jako zmenší okno prohlížeče hello.</p> |
| Barva pozadí na přihlašovací stránce |Barva pozadí přihlašovací stránku Hello se používá v hello oblasti toohello nalevo od formuláře přihlašovací stránku hello. |Musí to být barva RGB v šestnáctkovém formátu (příklad: #FFFFFF). |<p>Barva pozadí Hello se může zobrazit místo hello velkého obrázku v případě připojení s malou šířkou pásma</p><p>Doporučujeme vybrat primární barvu hello hello Banner s logem</p> |

## <a name="next-steps"></a>Další kroky
* [Začínáme se službou Azure Active Directory Premium](active-directory-get-started-premium.md)
* [Zobrazení sestav přístupů a používání](active-directory-view-access-usage-reports.md)

<!--Image references-->
[1]: ./media/active-directory-add-company-branding/SignInPage_beforecustomization.png
[2]: ./media/active-directory-add-company-branding/SignInPage_aftercustomization.png
[3]: ./media/active-directory-add-company-branding/SignInPage_mobile_beforecustomization.png
[4]: ./media/active-directory-add-company-branding/SignInPage_mobile_aftercustomization.png
[5]: ./media/active-directory-add-company-branding/SignInPage_aftercustomization_elements.png
[6]: ./media/active-directory-add-company-branding/SignInPage_aftercustomization_croppedleft.png
[7]: ./media/active-directory-add-company-branding/SignInPage_aftercustomization_croppedtop.png
[8]: ./media/active-directory-add-company-branding/APBranding.png
[9]: ./media/active-directory-add-company-branding/hidekmsi.png
