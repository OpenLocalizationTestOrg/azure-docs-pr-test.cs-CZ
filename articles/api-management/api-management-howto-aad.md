---
title: "aaaAuthorize vývojářským účtům pomocí Azure Active Directory – Azure API Management | Microsoft Docs"
description: "Zjistěte, jak tooauthorize uživatelů pomocí služby Azure Active Directory ve službě API Management."
services: api-management
documentationcenter: API Management
author: steved0x
manager: erikre
editor: 
ms.assetid: 33a69a83-94f2-4e4e-9cef-f2a5af3c9732
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: ebf5447a509a47df35e4262138bfcf423cb1dd5c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooauthorize-developer-accounts-using-azure-active-directory-in-azure-api-management"></a>Jak tooauthorize vývojáře účtů pomocí služby Azure Active Directory v Azure API Management
## <a name="overview"></a>Přehled
Tento průvodce vám ukáže, jak tooenable přístup k portálu pro vývojáře toohello pro uživatele ze služby Azure Active Directory. Tento průvodce také ukazuje, jak toomanage skupiny uživatelů Azure Active Directory tak, že přidáte externí skupiny, které obsahují hello uživatelů Azure Active Directory.

> toocomplete hello kroky v této příručce je nejdřív potřeba Azure Active Directory které toocreate aplikace.
> 
> 

## <a name="how-tooauthorize-developer-accounts-using-azure-active-directory"></a>Jak tooauthorize vývojáře účtů pomocí služby Azure Active Directory
tooget začít, klikněte na tlačítko **portál vydavatele** v hello portál Azure pro služby API Management. Tím přejdete portál vydavatele toohello API Management.

![Portál vydavatele][api-management-management-console]

> Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management] [ Create an API Management service instance] v hello [Začínáme s Azure API Management] [ Get started with Azure API Management] kurzu.
> 
> 

Klikněte na tlačítko **zabezpečení** z hello **API Management** nabídky na levé straně hello a klikněte na **externí identity**.

![Externí identity][api-management-security-external-identities]

Klikněte na tlačítko **Azure Active Directory**. Poznamenejte si hello **adresy URL pro přesměrování** a přepnout tooyour Azure Active Directory v hello portálu Azure Classic.

![Externí identity][api-management-security-aad-new]

Klikněte na tlačítko hello **přidat** tlačítko toocreate novou aplikaci Azure Active Directory a vyberte **přidat aplikaci, kterou vyvíjí Moje organizace**.

![Přidejte novou aplikaci Azure Active Directory][api-management-new-aad-application-menu]

Zadejte název aplikace hello, vyberte možnost **webové aplikace nebo webové rozhraní API**a klikněte na tlačítko Další hello.

![Novou aplikaci Azure Active Directory][api-management-new-aad-application-1]

Pro **přihlašovací adresa URL**, zadejte hello přihlašování URL portálu pro vývojáře. V tomto příkladu hello **přihlašovací adresa URL** je `https://aad03.portal.current.int-azure-api.net/signin`. 

Pro hello **URL ID aplikace**, zadejte hello výchozí doménu nebo vlastní doménu pro hello Azure Active Directory a připojení tooit jedinečného řetězce. V tomto příkladu hello výchozí doménu **https://contoso5api.onmicrosoft.com** se používá s příponou hello **/api** zadaný.

![Nové vlastnosti aplikace Azure Active Directory][api-management-new-aad-application-2]

Klikněte na tlačítko hello zaškrtněte tlačítko toosave a vytvoření aplikace hello a přepínače toohello **konfigurace** kartě tooconfigure hello novou aplikaci.

![Vytvořit novou aplikaci Azure Active Directory][api-management-new-aad-app-created]

Pokud se více Azure Active Directory se používá pro tuto aplikaci toobe, klikněte na tlačítko **Ano** pro **aplikace je víceklientské**. Výchozí hodnota Hello je **ne**.

![Aplikace je více klientů][api-management-aad-app-multi-tenant]

Kopírování hello **adresy URL pro přesměrování** z hello **Azure Active Directory** části hello **externí identity** v portálu vydavatele hello a vložte jej do hello **Adresa URL odpovědi** textové pole. 

![Adresa URL odpovědi][api-management-aad-reply-url]

Karta, vyberte hello konfigurace Scroll toohello dolní části hello **oprávnění aplikací** rozevíracího seznamu a zkontrolujte **čtení dat adresáře**.

![Oprávnění aplikací][api-management-aad-app-permissions]

Vyberte hello **delegování oprávnění** rozevíracího seznamu a zkontrolujte **povolit přihlášení a čtení uživatelských profilů**.

![Přidělená oprávnění][api-management-aad-delegated-permissions]

> Další informace o aplikaci a přidělená oprávnění najdete v tématu [hello přístup k rozhraní Graph API][Accessing hello Graph API].
> 
> 

Kopírování hello **Id klienta** toohello schránky.

![Id klienta][api-management-aad-app-client-id]

Přepněte zpět toohello portál vydavatele a vložte hello **Id klienta** zkopírovaných z konfigurace aplikace hello Azure Active Directory.

![Id klienta][api-management-client-id]

Přepnout zpět toohello konfigurace Azure Active Directory a klikněte na tlačítko hello **vyberte dobu trvání** rozevírací seznam v hello **klíče** části a zadat interval. V tomto příkladu **1 rok** se používá.

![Klíč][api-management-aad-key-before-save]

Klikněte na tlačítko **Uložit** toosave hello konfiguraci a zobrazení hello klíč. Kopírování hello klíče toohello schránky.

> Tento klíč si poznamenejte. Po zavření okna konfigurace Azure Active Directory hello hello klíč nelze zobrazit znovu.
> 
> 

![Klíč][api-management-aad-key-after-save]

Přepínač back toohello vydavatele portál a vložit hello klíč do hello **tajný klíč klienta** textové pole.

![Tajný klíč klienta][api-management-client-secret]

**Povolené klienty** Určuje adresáře, které mají přístup toohello rozhraní API pro instanci služby API Management hello. Zadejte domény hello hello toowhich instancí Azure Active Directory má toogrant přístup. Více domén můžete oddělit vložení znaků newline, mezery nebo čárkami.

![Povolené klientů][api-management-client-allowed-tenants]


Jakmile hello potřeby zadána konfigurace, klikněte na možnost **Uložit**.

![Uložit][api-management-client-allowed-tenants-save]

Po uložení změn hello hello uživatele v hello zadaný Azure Active Directory můžete pomocí následujících kroků hello v přihlásit portál pro vývojáře toohello [přihlásit pomocí účtu Azure Active Directory portálu pro vývojáře toohello] [Log in toohello Developer portal using an Azure Active Directory account].

V hello lze zadat více domén **klientům povoleno** části. Před každý uživatel může přihlásit z jiné doméně než hello původní domény, kde byl zaregistrován hello aplikace, musí se globální správce jinou doménu hello udělit oprávnění tooaccess aplikace hello dat adresáře. toogrant oprávnění globálního správce hello by měli přejít příliš`https://<URL of your developer portal>/aadadminconsent` (například https://contoso.portal.azure-api.net/aadadminconsent), zadejte název domény hello hello chtějí toogive přístup tooand klienta služby Active Directory, klikněte na tlačítko Odeslat. V následující hello příklad, globální správce `miaoaad.onmicrosoft.com` se pokouší toogive oprávnění toothis konkrétní developer portal. 

![Oprávnění][api-management-aad-consent]

Na další obrazovce hello bude globální správce hello výzvami tooconfirm udělíte oprávnění hello. 

![Oprávnění][api-management-permissions-form]

> Pokud není globální správce pokusí toolog v před oprávnění jsou udělena globální správce, hello pokus o přihlášení selže, se zobrazí chybové obrazovce.
> 
> 

## <a name="how-tooadd-an-external-azure-active-directory-group"></a>Jak tooadd externí Azure Active Directory skupiny
Když povolíte přístup pro uživatele v Azure Active Directory, můžete přidat, skupiny Azure Active Directory do rozhraní API správy toomore snadno spravovat hello přidružení hello vývojářů ve skupině hello hello požadovaných produktů.

> tooconfigure externí skupiny Azure Active Directory, hello Azure Active Directory musí nejdřív nakonfigurovat na kartě hello identit pomocí následujícího postupu hello v předchozí části hello. 
> 
> 

Externí skupiny Azure Active Directory se přidají hello **viditelnost** kartě hello produktů, pro kterou chcete toogrant přístup toohello skupiny. Klikněte na tlačítko **produkty**a potom klikněte na název hello požadované produktu hello.

![Konfigurace produktu][api-management-configure-product]

Přepínač toohello **viditelnost** a klikněte na **přidat skupiny z Azure Active Directory**.

![Přidání skupin][api-management-add-groups]

Vyberte hello **klienta Azure Active Directory** z hello rozevíracího seznamu a potom název typu hello hello požadované skupiny v hello **skupiny** toobe přidat textové pole.

![Vyberte skupinu][api-management-select-group]

Tento název skupiny lze nalézt v hello **skupiny** seznam pro Azure Active Directory, jak ukazuje následující příklad hello.

![Seznam skupin Azure Active Directory][api-management-aad-groups-list]

Klikněte na tlačítko **přidat** toovalidate hello název skupiny a přidejte skupinu hello. V tomto příkladu hello **Contoso 5 vývojáři** je přidána externí skupina. 

![Přidat skupinu][api-management-aad-group-added]

Klikněte na tlačítko **Uložit** toosave hello výběr nové skupiny.

Jakmile skupinu služby Azure Active Directory byla nakonfigurována jedním z produktů, je k dispozici toobe na hello zaškrtnuta možnost **viditelnost** hello kartu pro ostatní produkty v instanci služby API Management hello.

tooreview a konfigurovat hello vlastnosti pro externí skupiny po byly přidány, klikněte na název hello skupiny hello z hello **skupiny** kartě.

![Správa skupin][api-management-groups]

Zde můžete upravit hello **název** a hello **popis** hello skupiny.

![Úprava skupiny][api-management-edit-group]

Uživatelé z hello nakonfigurované Azure Active Directory můžete přihlásit na portál pro vývojáře toohello a zobrazení a přihlášení k odběru tooany skupin, ke kterým mají viditelnost podle následujících pokynů hello v následující části hello.

## <a name="how-toolog-in-toohello-developer-portal-using-an-azure-active-directory-account"></a>Jak toolog v portálu pro vývojáře toohello pomocí účtu Azure Active Directory
toolog do portálu pro vývojáře hello pomocí účtu Azure Active Directory nakonfigurované v předchozích částech hello, otevřete nové okno prohlížeče pomocí hello **přihlašovací adresa URL** z konfigurace aplikace hello služby Active Directory a klikněte na **Azure Active Directory**.

![Portál pro vývojáře][api-management-dev-portal-signin]

Zadejte přihlašovací údaje hello jednoho hello uživatelů ve vašem Azure Active Directory a klikněte na tlačítko **přihlášení**.

![Přihlášení][api-management-aad-signin]

S registračním formuláři můžete být vyzváni, pokud je vyžadováno žádné další údaje. Dokončení hello registračním formuláři a klikněte na tlačítko **zaregistrovat**.

![Registrace][api-management-complete-registration]

Uživatel je nyní přihlášeni hello portál pro vývojáře pro instanci služby API Management.

![Registrace je dokončena.][api-management-registration-complete]

[api-management-management-console]: ./media/api-management-howto-aad/api-management-management-console.png
[api-management-security-external-identities]: ./media/api-management-howto-aad/api-management-security-external-identities.png
[api-management-security-aad-new]: ./media/api-management-howto-aad/api-management-security-aad-new.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-aad/api-management-new-aad-application-menu.png
[api-management-new-aad-application-1]: ./media/api-management-howto-aad/api-management-new-aad-application-1.png
[api-management-new-aad-application-2]: ./media/api-management-howto-aad/api-management-new-aad-application-2.png
[api-management-new-aad-app-created]: ./media/api-management-howto-aad/api-management-new-aad-app-created.png
[api-management-aad-app-permissions]: ./media/api-management-howto-aad/api-management-aad-app-permissions.png
[api-management-aad-app-client-id]: ./media/api-management-howto-aad/api-management-aad-app-client-id.png
[api-management-client-id]: ./media/api-management-howto-aad/api-management-client-id.png
[api-management-aad-key-before-save]: ./media/api-management-howto-aad/api-management-aad-key-before-save.png
[api-management-aad-key-after-save]: ./media/api-management-howto-aad/api-management-aad-key-after-save.png
[api-management-client-secret]: ./media/api-management-howto-aad/api-management-client-secret.png
[api-management-client-allowed-tenants]: ./media/api-management-howto-aad/api-management-client-allowed-tenants.png
[api-management-client-allowed-tenants-save]: ./media/api-management-howto-aad/api-management-client-allowed-tenants-save.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-aad/api-management-aad-delegated-permissions.png
[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.png
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png
[api-management-aad-app-multi-tenant]: ./media/api-management-howto-aad/api-management-aad-app-multi-tenant.png
[api-management-aad-reply-url]: ./media/api-management-howto-aad/api-management-aad-reply-url.png
[api-management-aad-consent]: ./media/api-management-howto-aad/api-management-aad-consent.png
[api-management-permissions-form]: ./media/api-management-howto-aad/api-management-permissions-form.png
[api-management-configure-product]: ./media/api-management-howto-aad/api-management-configure-product.png
[api-management-add-groups]: ./media/api-management-howto-aad/api-management-add-groups.png
[api-management-select-group]: ./media/api-management-howto-aad/api-management-select-group.png
[api-management-aad-groups-list]: ./media/api-management-howto-aad/api-management-aad-groups-list.png
[api-management-aad-group-added]: ./media/api-management-howto-aad/api-management-aad-group-added.png
[api-management-groups]: ./media/api-management-howto-aad/api-management-groups.png
[api-management-edit-group]: ./media/api-management-howto-aad/api-management-edit-group.png

[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How tooadd and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs tooa product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Accessing hello Graph API]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API toouse OAuth 2.0 user authorization]: #step2
[Test hello OAuth 2.0 user authorization in hello Developer Portal]: #step3
[Next steps]: #next-steps

[Log in toohello Developer portal using an Azure Active Directory account]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account

