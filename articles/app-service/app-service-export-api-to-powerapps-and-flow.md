---
title: "aaaExporting služby hostované v Azure API tooPowerApps a Microsoft Flow | Microsoft Docs"
description: "Přehled o tom, jak tooexpose rozhraní API hostovaná v tooPowerApps služby App Service a Flow Microsoft"
services: app-service
documentationcenter: 
author: mattchenderson
manager: erikre
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 06/20/2017
ms.author: mahender
ms.openlocfilehash: 285b6efa3af5b0feac1ee2f617c0dc56dc3fd198
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="exporting-an-azure-hosted-api-toopowerapps-and-microsoft-flow"></a>Export služby hostované v Azure API tooPowerApps a Flow Microsoft

## <a name="creating-custom-connectors-for-powerapps-and-microsoft-flow"></a>Vytvoření vlastních konektorů pro PowerApps a Flow Microsoft

[PowerApps](https://powerapps.com) je služba pro vytváření a používání vlastní obchodní aplikace, které se připojují tooyour dat a fungovat na všech platformách. [Microsoft Flow](https://flow.microsoft.com) umožňuje snadno tooautomate pracovní postupy a obchodní procesy mezi vaše oblíbené aplikace a služby. PowerApps a Microsoft Flow obsahují celou řadu předdefinovaných konektory toodata zdrojů například Office 365, Dynamics 365, Salesforce a další. Uživatelé však také potřebovat zdroje dat nemůže tooleverage toobe a rozhraní API sestavuje organizace.

Podobně vývojáři, kteří mají tooexpose další široce v rámci hello organizace chtít toomake jejich dostupné tooPowerApps rozhraní API a uživatelé Microsoft Flow příslušných rozhraní API. Toto téma vám ukáže, jak tooexpose rozhraní API vytvořených pomocí služby Azure App Service nebo Azure Functions tooPowerApps a Flow společnosti Microsoft. [Aplikační služba Azure](https://azure.microsoft.com/services/app-service/) je nabídku platforma jako služba, která umožňuje vývojářům tooquickly a snadno sestavení podnikové úrovni webové, mobilní a aplikacích API. [Azure Functions](https://azure.microsoft.com/services/functions/) je založený na událostech bez serveru výpočetních řešení, které vám umožní tooquickly Autor kód, který může reagovat tooother části systému a škálování na základě poptávky.

toolearn více o těchto službách najdete v části:
- [PowerApps na základě učení](https://powerapps.microsoft.com/guided-learning/learning-introducing-powerapps/) 
- [Microsoft toku na základě učení](https://flow.microsoft.com/guided-learning/learning-introducing-flow/)
- [Co je App Service?](https://docs.microsoft.com/azure/app-service/app-service-value-prop-what-is)
- [Co je Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-overview)

## <a name="sharing-an-api-definition"></a>Sdílení definice rozhraní API

Rozhraní API jsou často popsané pomocí [OpenAPI dokumentu](https://www.openapis.org/) (někdy označovaná tooas dokumentu "Swagger"). Tato položka obsahuje všechny informace hello o jaké operace jsou k dispozici a jak by měla být hello data strukturu. PowerApps a Flow Microsoft můžete vytvořit vlastní konektory pro každý dokument, OpenAPI 2.0. Po vytvoření vlastní konektor, lze použít v přesně hello stejným způsobem jako jeden z předdefinovaných konektory hello a lze snadno integrovat do aplikace.

Azure App Service a Azure Functions mají [integrovanou podporu](https://docs.microsoft.com/azure/app-service-api/app-service-api-metadata) pro vytváření, hostování a správu dokument OpenAPI. V pořadí toocreate vlastní konektor pro webové, mobilní, rozhraní API nebo funkce aplikace PowerApps a toku nutné toobe zadané kopie hello definice.

> [!NOTE]
> Protože je použita kopii hello definice rozhraní API, PowerApps a Flow Microsoft nebude vědět okamžitě o aktualizace nebo aplikace toohello narušující změny. Pokud je k dispozici nová verze rozhraní API hello, tyto kroky je potřeba zopakovat pro novou verzi hello. 

tooprovide PowerApps a Flow Microsoft s hello hostované definice rozhraní API pro aplikace, postupujte takto:

1. Otevřete hello [portálu Azure](https://portal.azure.com) a přejděte tooyour aplikace služby App Service nebo Azure Functions.

    Pokud používáte Azure App Service, vyberte **definice rozhraní API** hello nastavení seznamu. 
    
    Pokud používáte Azure Functions, vyberte svou aplikaci funkce a potom zvolte **funkce**a potom **definice rozhraní API**. Může také zvolit tooopen hello **definice rozhraní API (preview)** kartě místo.

2. Pokud bylo zadáno definice rozhraní API, zobrazí se **exportovat tooPowerApps + Microsoft Flow** tlačítko. Klikněte na toto tlačítko toobegin hello export proces.

3. Vyberte hello **Export režimu**. Tato hodnota určuje hello kroků budete potřebovat toofollow toocreate konektor. Služby App Service nabízí dvě možnosti pro poskytování vaší definice rozhraní API PowerApps a Flow Microsoft:

    **Express** hello se dá vytvořit vlastní konektor hello z v rámci portálu Azure. To vyžaduje, že tento hello asi přihlášený uživatel má oprávnění toocreate konektory v hello cílovém prostředí. Toto je hello doporučenému přístupu, pokud tento požadavek můžete splnit. Pokud při použití tohoto režimu, postupujte podle hello [Express export](#express) podle následujících pokynů.

    **Ruční** umožňuje exportovat kopii linkové hello rozhraní API, který lze importovat pomocí hello PowerApps nebo Microsoft Flow portálů. Toto je hello doporučenému přístupu, pokud hello Azure uživatele a hello uživatele s konektory toocreate oprávnění jsou jiné osoby nebo pokud hello konektor musí toobe vytvořené v jiného klienta. Pokud při použití tohoto režimu, postupujte podle hello [ruční export a import](#manual) podle následujících pokynů.

<a name="express"></a>
## <a name="express-export"></a>Express exportu

V této části vytvoříte novou vlastní konektor z v rámci hello portálu Azure. Musíte být přihlášeni do hello klienta toowhich chcete tooexport a musíte mít oprávnění toocreate vlastní konektor v cílovém prostředí hello.

1. Vyberte hello prostředí, ve kterém chcete toocreate hello konektor. Potom zadejte název pro vlastní konektor.

2. Pokud vaše definice rozhraní API obsahuje všechny definice zabezpečení, tyto volaná v kroku #2. V případě potřeby zadejte hello zabezpečení podrobnosti konfigurace potřeby toogrant uživatelé přístup k rozhraní API tooyour. Další informace najdete v tématu [ověřování](#auth) níže. 

3. Klikněte na tlačítko **OK** toocreate vlastní konektor.


<a name="manual"></a>
## <a name="manual-export-and-import"></a>Ruční export a import

V pořadí toocreate vlastní konektor pro webové, mobilní, rozhraní API nebo funkce aplikace bude potřeba dva kroky:

1. [Načítání definice rozhraní API hello ze služby App Service nebo Azure Functions](#export)
2. [Import definice rozhraní API hello do PowerApps a Flow Microsoft](#import)

Je možné, že tyto dva kroky potřebovat toobe prováděné samostatné jednotlivce v organizaci, protože daného uživatele nemusí mít oprávnění tooperform obě akce. V takovém případě vývojář, který má přístup toohello Přispěvatel aplikace služby App Service nebo Azure Functions potřebovat tooobtain hello rozhraní API definice (jeden soubor JSON) nebo tooit odkaz. Tooprovide se pak musí vlastník této definici tooa PowerApps nebo Flow Microsoft. Daného vlastníka můžete použít hello metadata toocreate hello vlastní konektor.

<a name="export"></a>
### <a name="retrieving-hello-api-definition-from-app-service-or-azure-functions"></a>Načítání definice rozhraní API hello ze služby App Service nebo Azure Functions

V této části budete exportovat hello definice rozhraní API pro App Service API, toobe použít později v hello PowerApps nebo Microsoft Flow na portálu.

1. Můžete zvolit tooeither **stáhnout definice hello rozhraní API** nebo **získejte odkaz**. Podle toho, co vyberete, bude k dispozici hello výsledek v další části hello. Vyberte jednu z těchto možností a postupujte podle pokynů hello.
 
2. Pokud vaše definice rozhraní API obsahuje všechny definice zabezpečení, tyto volaná v kroku #2. Během importu PowerApps a Flow Microsoft tyto zjistí a zobrazí výzvu pro informace o zabezpečení. Shromážděte hello přihlašovací údaje související tooeach definice pro použití v další části hello. Další informace najdete v tématu [ověřování](#auth) níže. 

<a name="import"></a>
### <a name="importing-hello-api-definition-into-powerapps-and-microsoft-flow"></a>Import definice rozhraní API hello do PowerApps a Flow Microsoft

V této části vytvoříte vlastní konektor v PowerApps a Microsoft Flow pomocí dříve získali definice hello rozhraní API. Vlastní konektory jsou sdílené mezi hello dvě služby, proto musíte pouze jednou tooimport hello definice. Další informace o vlastních konektorů najdete v tématu [zaregistrovat a použít vlastní konektory v PowerApps] a [zaregistrovat a použít vlastní konektory v Microsoft Flow].

1. Otevřete hello [Powerapps webový portál](https://web.powerapps.com) nebo hello [webový portál Microsoft Flow](https://flow.microsoft.com/)a přihlaste se. 

2. Klikněte na tlačítko hello **nastavení** tlačítko (ikona hello ozubené kolečko) v pravé horní hello hello stránku a vyberte **vlastní konektory**. 

3. Klikněte na tlačítko **vytvořte vlastní konektor**.

4. Na hello **Obecné** kartě, zadejte název pro rozhraní API a pak nahrajte hello OpenAPI definice nebo vložte adresu URL metadat hello. Klikněte na tlačítko **pokračovat**.

4. Na hello **zabezpečení** kartě, pokud jsou podrobnosti výzvami tooprovide ověřování, zadejte hodnoty hello získanou v předchozí části hello. Pokud tomu tak není, pokračujte dalším krokem toohello.

5. Na hello **definice** , všechny operace hello definované v souboru OpenAPI jsou automaticky vyplněna. Pokud jsou definovány všechny požadované operace, můžete přejít toohello další krok. Pokud ne, můžete přidat a upravit operations sem.

6. Klikněte na tlačítko **vytvořit konektor**. Pokud chcete, aby tootest volání rozhraní API, přejděte toohello další krok.

7. Na hello **Test** kartě, vytvořit připojení, vyberte tootest operaci a zadejte všechna data hello operace.

8. Klikněte na tlačítko **testovací operace**.


<a name="auth"></a>
## <a name="authentication"></a>Authentication

PowerApps a Microsoft Flow nativně podporují kolekci zprostředkovatelů identity, které můžou být použité toolog v uživatelé vlastní konektor. Pokud vaše rozhraní API vyžaduje ověření, ujistěte se, že se zaznamená jako _zabezpečení definice_ v OpenAPI dokumentu. Během exportu budete potřebovat tooprovide konfigurační hodnoty, které umožňují PowerApps Microsoft Flow tooperform přihlášení akce.

Tato část popisuje hello typy ověřování, které jsou podporovány produktem express toku hello: klíč rozhraní API, Azure Active Directory a obecné OAuth 2.0. Úplný seznam poskytovatelů a přihlašovací údaje hello každé vyžaduje, najdete v části [zaregistrovat a použít vlastní konektory v PowerApps] a [zaregistrovat a použít vlastní konektory v Microsoft Flow].

### <a name="api-key"></a>Klíč rozhraní API
Pokud se používá toto schéma zabezpečení, budou uživatelé hello konektoru výzvami tooprovide hello klíč při vytváření připojení. Můžete zadat toohelp název klíče rozhraní API je vědět, které je zapotřebí. Pro Azure Functions obvykle bude jeden z klíčů hostitele hello, pokrývajících několik funkcí v rámci aplikace hello funkce.

### <a name="azure-active-directory"></a>Azure Active Directory
Při konfiguraci vlastní konektor, který vyžaduje přihlášení AAD, jsou požadovány dva registrace AAD aplikace: jedno toomodel hello back-end rozhraní API a jeden konektor hello toomodel v PowerApps a toku.

Rozhraní API by měl být nakonfigurovaný toowork s registrací první hello, a to bude již postarat Pokud jste použili hello [ověřování/autorizace služby App Service](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication) funkce.

Budete mít toomanually vytvořte druhý registrace hello hello konektor, pomocí hello kroky popsané v [přidání aplikace AAD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#adding-an-application). Hello registrace musí mít toohave Delegovaný přístup tooyour rozhraní API a adresa URL odpovědi z `https://msmanaged-na.consent.azure-apim.net/redirect`. Najdete v tématu [v tomto příkladu](
https://powerapps.microsoft.com/tutorials/customapi-azure-resource-manager-tutorial/) další podrobnosti, nahraďte vaše rozhraní API pro hello Azure Resource Manager.

vyžadují se Hello následující hodnoty konfigurace:
- **ID klienta** -hello ID klienta konektoru registrace AAD
- **Tajný klíč klienta** -tajný klíč klienta hello konektoru registrace AAD
- **Adresa URL pro přihlášení** – hello základní adresu URL pro AAD. Ve veřejné Azure, bude obvykle jednat `https://login.windows.net`.
- **ID klienta** -hello ID toobe hello klienta použít pro přihlášení hello. To by měla být "běžné" nebo hello ID hello klienta, ve které hello konektor vytvořen.
- **Adresa URL prostředku** -hello URL prostředku registrace AAD back-end vašeho rozhraní API

> [!IMPORTANT]
> Pokud jiné fyzické budete importovat definici hello rozhraní API do PowerApps a Flow Microsoft jako součást ruční toku hello, budete potřebovat tooprovide je s hello ID a klienta tajný klíč klienta **registrace konektoru hello**, také jako adresa URL prostředku hello rozhraní API. Ujistěte se, že těchto tajných klíčů spravuje bezpečně. **Nesdílí hello bezpečnostních pověření hello rozhraní API, sám sebe.**

### <a name="generic-oauth-20"></a>Obecné OAuth 2.0
Hello obecné podpory OAuth 2.0 umožňuje toointegrate s kteréhokoli poskytovatele služeb, OAuth 2.0. To vám umožní toobring v žádné vlastní poskytovatele, který není nativně podporované.

vyžadují se Hello následující hodnoty konfigurace:
- **ID klienta** -hello ID klienta OAuth 2.0
- **Tajný klíč klienta** -tajný klíč klienta hello OAuth 2.0
- **URL pro autorizaci** -hello URL pro autorizaci OAuth 2.0
- **Token URL** -hello adresy URL tokenu OAuth 2.0
- **Aktualizujte adresu URL** -hello URL aktualizace OAuth 2.0



[zaregistrovat a použít vlastní konektory v PowerApps]: https://powerapps.microsoft.com/tutorials/register-custom-api/
[zaregistrovat a použít vlastní konektory v Microsoft Flow]: https://flow.microsoft.com/documentation/register-custom-api/
