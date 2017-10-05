---
title: "Export rozhraní API Azure hostovaná PowerApps a Microsoft toku | Microsoft Docs"
description: "Přehled o tom, jak vystavit rozhraní API hostované ve službě App Service PowerApps a Flow Microsoft"
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
ms.openlocfilehash: 0d166a2e5b60c3b9f911f9fc3ad49ad7f252abb4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="exporting-an-azure-hosted-api-to-powerapps-and-microsoft-flow"></a>Export rozhraní API Azure hostovaná PowerApps a Microsoft toku

## <a name="creating-custom-connectors-for-powerapps-and-microsoft-flow"></a>Vytvoření vlastních konektorů pro PowerApps a Flow Microsoft

[PowerApps](https://powerapps.com) je služba pro vytváření a používání vlastní obchodní aplikace, které se připojují ke svým datům a fungovat na všech platformách. [Microsoft Flow](https://flow.microsoft.com) usnadňuje automatizovat pracovní postupy a obchodní procesy mezi vaše oblíbené aplikace a služby. Jak PowerApps a Flow Microsoft obsahují celou řadu předdefinovaných konektory ke zdrojům dat, jako je například Office 365, Dynamics 365, Salesforce a další. Ale uživatelé také musí být schopné využít zdroje dat a rozhraní API sestavuje organizace.

Podobně vývojáře, která chcete vystavit příslušných rozhraní API více široce v rámci organizace chtít zpřístupnit příslušných rozhraní API uživatelům PowerApps a Flow společnosti Microsoft. Toto téma vám ukáže, jak vystavit rozhraní API vytvořené s Azure App Service nebo Azure Functions PowerApps a Flow společnosti Microsoft. [Aplikační služba Azure](https://azure.microsoft.com/services/app-service/) je nabídku platforma jako služba, která umožňuje vývojářům snadno a rychle vytvářet podnikové úrovni webové, mobilní a aplikacích API. [Azure Functions](https://azure.microsoft.com/services/functions/) je založený na událostech bez serveru výpočetních řešení, které umožňuje rychle vytvořit kód, který může reagovat na dalších částí systému a škálování na základě poptávky.

Další informace o těchto službách najdete v tématu:
- [PowerApps na základě učení](https://powerapps.microsoft.com/guided-learning/learning-introducing-powerapps/) 
- [Microsoft toku na základě učení](https://flow.microsoft.com/guided-learning/learning-introducing-flow/)
- [Co je App Service?](https://docs.microsoft.com/azure/app-service/app-service-value-prop-what-is)
- [Co je Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-overview)

## <a name="sharing-an-api-definition"></a>Sdílení definice rozhraní API

Rozhraní API jsou často popsané pomocí [OpenAPI dokumentu](https://www.openapis.org/) (někdy označované jako dokument "Swagger"). Tato položka obsahuje všechny informace o jaké operace jsou k dispozici a jak by měla být strukturovaná data. PowerApps a Flow Microsoft můžete vytvořit vlastní konektory pro každý dokument, OpenAPI 2.0. Po vytvoření vlastní konektor, mohou být používány stejným způsobem jako jeden z předdefinovaných konektory a lze snadno integrovat do aplikace.

Azure App Service a Azure Functions mají [integrovanou podporu](https://docs.microsoft.com/azure/app-service-api/app-service-api-metadata) pro vytváření, hostování a správu dokument OpenAPI. Chcete-li vytvořit vlastní konektor pro webové, mobilní, rozhraní API nebo funkce aplikace, PowerApps a toku musí mít kopii definici.

> [!NOTE]
> Protože je použita kopii definice rozhraní API, PowerApps a Flow Microsoft nebude vědět okamžitě o aktualizace nebo nejnovější změny do aplikace. Pokud je k dispozici nová verze rozhraní API, tyto kroky je potřeba zopakovat pro novou verzi. 

PowerApps a Microsoft Flow poskytnout hostované definice rozhraní API pro vaši aplikaci, postupujte takto:

1. Otevřete [portálu Azure](https://portal.azure.com) a přejděte do aplikace služby App Service nebo Azure Functions.

    Pokud používáte Azure App Service, vyberte **definice rozhraní API** ze seznamu nastavení. 
    
    Pokud používáte Azure Functions, vyberte svou aplikaci funkce a potom zvolte **funkce**a potom **definice rozhraní API**. Může se také rozhodnout otevřít **definice rozhraní API (preview)** kartě místo.

2. Pokud bylo zadáno definice rozhraní API, zobrazí se **exportovat do PowerApps + Microsoft Flow** tlačítko. Kliknutím na toto tlačítko k zahájení procesu exportu.

3. Vyberte **Export režimu**. Určuje, kroky, které budete muset provést, pokud chcete vytvořit konektor. Služby App Service nabízí dvě možnosti pro poskytování vaší definice rozhraní API PowerApps a Flow Microsoft:

    **Express** se dá vytvořit vlastní konektor z portálu Azure. To vyžaduje, aby si přihlášený uživatel má oprávnění k vytváření konektory v cílovém prostředí. Toto je doporučený postup, pokud tento požadavek můžete splnit. Pokud při použití tohoto režimu, postupujte podle kroků [Express export](#express) podle následujících pokynů.

    **Ruční** umožňuje exportovat kopii linkové rozhraní API, který lze importovat používání portálů PowerApps nebo Flow společnosti Microsoft. Toto je doporučený postup, pokud uživatel Azure a uživatel s oprávněním k vytvoření konektory jsou jiné osoby nebo pokud konektor musí být vytvořen v jiného klienta. Pokud při použití tohoto režimu, postupujte podle kroků [ruční export a import](#manual) podle následujících pokynů.

<a name="express"></a>
## <a name="express-export"></a>Express exportu

V této části vytvoříte novou vlastní konektor z portálu Azure. Musíte být přihlášeni do klienta, ke které chcete exportovat, a musí mít oprávnění k vytvoření vlastní konektor v cílovém prostředí.

1. Vyberte prostředí, ve kterém chcete vytvořit konektor. Potom zadejte název pro vlastní konektor.

2. Pokud vaše definice rozhraní API obsahuje všechny definice zabezpečení, tyto volaná v kroku #2. V případě potřeby zadejte podrobnosti konfigurace zabezpečení potřebné pro uživatelům uděluje přístup k vašemu rozhraní API. Další informace najdete v tématu [ověřování](#auth) níže. 

3. Klikněte na tlačítko **OK** vytvořit vaše vlastní konektor.


<a name="manual"></a>
## <a name="manual-export-and-import"></a>Ruční export a import

Chcete-li vytvořit vlastní konektor pro webové, mobilní, rozhraní API nebo funkce aplikace, bude potřeba dva kroky:

1. [Načítání definice rozhraní API z aplikace služby nebo funkce Azure](#export)
2. [Import definice rozhraní API do PowerApps a Flow Microsoft](#import)

Je možné, že tyto dva kroky muset prováděné samostatné jednotlivce v organizaci, jak se daného uživatele nemusí mít oprávnění k provádění obou akcí. V takovém případě vývojáři, který má Přispěvatel přístup k aplikaci služby App Service nebo Azure Functions muset získat definice rozhraní API (jeden soubor JSON) nebo odkaz na něj. Pak bude muset poskytují této definici PowerApps nebo Microsoft Flow vlastníka. Daného vlastníka můžete použít k vytvoření vlastní konektor metadata.

<a name="export"></a>
### <a name="retrieving-the-api-definition-from-app-service-or-azure-functions"></a>Načítání definice rozhraní API z aplikace služby nebo funkce Azure

V této části budete exportovat definice rozhraní API pro vaše rozhraní API App Service, který se má použít později na portálu PowerApps nebo Flow Microsoft.

1. Buď můžete **stáhnout definice rozhraní API** nebo **získejte odkaz**. Podle toho, co si zvolíte, výsledkem bude k dispozici v další části. Vyberte jednu z těchto možností a postupujte podle pokynů.
 
2. Pokud vaše definice rozhraní API obsahuje všechny definice zabezpečení, tyto volaná v kroku #2. Během importu PowerApps a Flow Microsoft tyto zjistí a zobrazí výzvu pro informace o zabezpečení. Získat přihlašovací údaje související s každou definici pro použití v další části. Další informace najdete v tématu [ověřování](#auth) níže. 

<a name="import"></a>
### <a name="importing-the-api-definition-into-powerapps-and-microsoft-flow"></a>Import definice rozhraní API do PowerApps a Flow Microsoft

V této části vytvoříte vlastní konektor v PowerApps a Microsoft Flow pomocí definice rozhraní API dříve získali. Vlastní konektory jsou sdílené mezi dvě služby, takže potřebujete importovat definici jednou. Další informace o vlastních konektorů najdete v tématu [zaregistrovat a použít vlastní konektory v PowerApps] a [zaregistrovat a použít vlastní konektory v Microsoft Flow].

1. Otevřete [Powerapps webový portál](https://web.powerapps.com) nebo [webový portál Microsoft Flow](https://flow.microsoft.com/)a přihlaste se. 

2. Klikněte **nastavení** tlačítko (ikona ozubené kolečko) v pravém horním rohu stránky na stránku a vyberte **vlastní konektory**. 

3. Klikněte na tlačítko **vytvořte vlastní konektor**.

4. Na **Obecné** kartě, zadejte název pro rozhraní API a potom odeslat definici OpenAPI nebo vložte adresu URL metadat. Klikněte na tlačítko **pokračovat**.

4. Na **zabezpečení** kartě, pokud se zobrazí výzva k zadejte podrobnosti o ověřování, zadejte hodnoty získané v předchozím oddílu. V opačném případě přejděte k dalšímu kroku.

5. Na **definice** , všechny operace, které jsou definované v souboru OpenAPI jsou automaticky vyplněna. Pokud jsou definovány všechny požadované operace, můžete přejít k dalšímu kroku. Pokud ne, můžete přidat a upravit operations sem.

6. Klikněte na tlačítko **vytvořit konektor**. Pokud chcete testovat volání rozhraní API, přejděte k dalšímu kroku.

7. Na **testování** vytvořit připojení, vyberte na testovací operace a zadejte všechna data vyžadovanou operaci.

8. Klikněte na tlačítko **testovací operace**.


<a name="auth"></a>
## <a name="authentication"></a>Authentication

PowerApps a Microsoft Flow nativně podporují kolekci zprostředkovatelů identity, které se dají použít k přihlášení uživatelé vlastní konektor. Pokud vaše rozhraní API vyžaduje ověření, ujistěte se, že se zaznamená jako _zabezpečení definice_ v OpenAPI dokumentu. Během exportu musíte zadat hodnoty konfigurace, které umožňují PowerApps Flow Microsoft k provádění akcí přihlášení.

Tato část popisuje typy ověřování, které podporuje rychlé toku: klíč rozhraní API, Azure Active Directory a obecné OAuth 2.0. Úplný seznam poskytovatelů a každý vyžaduje přihlašovací údaje, najdete v části [zaregistrovat a použít vlastní konektory v PowerApps] a [zaregistrovat a použít vlastní konektory v Microsoft Flow].

### <a name="api-key"></a>Klíč rozhraní API
Pokud se používá toto schéma zabezpečení, uživatelé konektoru budou vyzváni k zadání klíč při vytváření připojení. Můžete zadat název klíče pomáhá jim vědět, který klíč je potřeba rozhraní API. Pro Azure Functions obvykle bude jeden z klíčů hostitele, pokrývajících několik funkcí v rámci funkce aplikace.

### <a name="azure-active-directory"></a>Azure Active Directory
Při konfiguraci vlastní konektor, který vyžaduje přihlášení AAD, jsou požadovány dva registrace AAD aplikace: jeden pro modelování back-end rozhraní API a jeden pro modelování konektor v PowerApps a toku.

Rozhraní API by měl být nakonfigurovaný pro práci s první registraci, a to bude již postarat Pokud jste použili [ověřování/autorizace služby App Service](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication) funkce.

Budete muset ručně vytvořit druhý registrace pro tento konektor, pomocí kroků v [přidání aplikace AAD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#adding-an-application). Registrace je potřeba mít Delegovaný přístup k rozhraní API a adresa URL odpovědi z `https://msmanaged-na.consent.azure-apim.net/redirect`. Najdete v tématu [v tomto příkladu](
https://powerapps.microsoft.com/tutorials/customapi-azure-resource-manager-tutorial/) podrobněji, nahraďte vaše rozhraní API pro Azure Resource Manager.

Jsou tyto hodnoty konfigurace vyžaduje:
- **ID klienta** -ID klienta konektoru registrace AAD
- **Tajný klíč klienta** -tajný klíč klienta konektoru registrace AAD
- **Adresa URL pro přihlášení** – základní adresu URL pro AAD. Ve veřejné Azure, bude obvykle jednat `https://login.windows.net`.
- **ID klienta** -ID klienta, který se má použít pro přihlášení. Měl by být "běžné" nebo ID klienta, ve kterém se vytváří konektor.
- **Adresa URL prostředku** – prostředek URL registrace AAD rozhraní API back-end

> [!IMPORTANT]
> Pokud jiné fyzické budete importovat definici rozhraní API do PowerApps a Flow Microsoft jako součást Ruční postup, musíte jim poskytnout ID klienta a tajný klíč klienta **registrace konektoru**, a také na adresu URL zdroje rozhraní API. Ujistěte se, že těchto tajných klíčů spravuje bezpečně. **Nesdílí pověření zabezpečení rozhraní API, sám sebe.**

### <a name="generic-oauth-20"></a>Obecné OAuth 2.0
Obecné podpory OAuth 2.0 umožňuje integraci se službou kteréhokoli poskytovatele služeb, OAuth 2.0. To umožňuje přinést si všechny vlastní poskytovatele, který není nativně podporované.

Jsou tyto hodnoty konfigurace vyžaduje:
- **ID klienta** -ID klienta OAuth 2.0
- **Tajný klíč klienta** -tajný klíč klienta OAuth 2.0
- **URL pro autorizaci** -URL pro autorizaci OAuth 2.0
- **Token URL** -adresy URL tokenu OAuth 2.0
- **Aktualizujte adresu URL** – adresa URL aktualizace OAuth 2.0



[zaregistrovat a použít vlastní konektory v PowerApps]: https://powerapps.microsoft.com/tutorials/register-custom-api/
[zaregistrovat a použít vlastní konektory v Microsoft Flow]: https://flow.microsoft.com/documentation/register-custom-api/
