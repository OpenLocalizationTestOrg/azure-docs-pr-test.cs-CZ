---
title: "aaaAccess rozhraní API služby Azure Media Services pomocí ověřování Azure Active Directory | Microsoft Docs"
description: "Další informace o konceptech a kroky tootake toouse Azure Active Directory (Azure AD) tooauthenticate přístup toohello rozhraní API služby Azure Media Services."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: juliako
ms.openlocfilehash: bb8f75f39100dc37098260c24ab4a199ef689d46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="access-hello-azure-media-services-api-with-azure-ad-authentication"></a>Hello přístup k rozhraní API služby Azure Media Services pomocí ověřování Azure AD
 
Hello rozhraní API služby Azure Media Services je rozhraní RESTful API. Můžete ho tooperform operací s prostředky média pomocí rozhraní REST API nebo pomocí sady SDK k dispozici klienta. Azure Media Services nabízí klienta Media Services SDK pro rozhraní Microsoft .NET. toobe autorizovaný prostředky tooaccess Media Services a hello Media Services API, musíte nejprve být ověřeni. 

Služba Media Services podporuje [Azure Active Directory (Azure AD)-ověřování na základě](../active-directory/active-directory-whatis.md). Hello Azure Media REST service vyžaduje, aby hello uživatele nebo aplikace, která vytváří požadavky REST API hello měl buď hello **Přispěvatel** nebo **vlastníka** role tooaccess hello prostředky. Další informace najdete v tématu [Začínáme s řízením přístupu na základě rolí v hello portál Azure](../active-directory/role-based-access-control-what-is.md).  

> [!IMPORTANT]
> V současné době Media Services podporuje model ověřování služby Řízení přístupu Azure hello. Řízení přístupu autorizace však bude na 1. června 2018 zastaralá. Doporučujeme, abyste co nejdříve migraci model ověřování toohello Azure AD.

Tento dokument poskytuje přehled o tom, jak tooaccess hello Media Services API pomocí rozhraní API .NET nebo REST.

## <a name="access-control"></a>Řízení přístupu

Pro hello REST Azure média požadavku toosucceed hello volání uživatel musí mít roli Přispěvatel nebo vlastníka pro hello účtu Media Services, že se pokouší tooaccess.  
Pouze uživatel s hello vlastníka role můžete udělit uživatelů toonew přístup média prostředků (účet) nebo aplikace. role Přispěvatel Hello přístup pouze hello mediální zdroj.
Neoprávněné žádosti nezdaří s stavový kód 401. Pokud se tento kód chyby, zkontrolujte, zda váš uživatel má hello Přispěvatel nebo roli vlastníka přiřazená k účtu Media Services hello uživatele. Zkontrolovat to můžete v hello portálu Azure. Vyhledejte svůj účet mediálních a pak klikněte na hello **řízení přístupu** kartě. 

![Karta řízení přístupu](./media/media-services-use-aad-auth-to-access-ams-api/media-services-access-control.png)

## <a name="types-of-authentication"></a>Typy ověřování 
 
Při použití ověřování Azure AD pomocí služby Azure Media Services, máte dvě možnosti ověřování:

- **Ověření uživatele**. Ověření osoba, která používá toointeract aplikace hello s prostředky Media Services. interaktivní aplikace Hello měli nejdřív hello uživatele vyzve k přihlašovacích údajů uživatele hello. Příklad je používán oprávněným uživatelům toomonitor kódování úlohy správy konzoly aplikace nebo živé streamování. 
- **Objekt zabezpečení ověřování služby**. Ověření služby. Aplikace, které běžně používají tuto metodu ověřování jsou aplikace, které běží služby démon, střední vrstvy služby nebo naplánované úlohy. Příklady jsou webové aplikace, funkce aplikací, aplikace logiky, rozhraní API a mikroslužeb.

### <a name="user-authentication"></a>Ověřování uživatelů 

Aplikace, které by měl použít metodu ověřování uživatele hello jsou pro správu nebo monitorování nativní aplikace: mobilní aplikace, aplikace pro Windows a konzolové aplikace. Tohoto typu řešení je užitečné, když chcete zásahem ze strany službou hello v jednom z hello následující scénáře:

- Řídicí panel pro kódování úlohy monitorování.
- Řídicí panel pro vaše živé datové proudy monitorování.
- Správa aplikací pro uživatele desktop či mobile tooadminister prostředky v účtu Media Services.

> [!NOTE]
> Tato metoda ověřování není používat u určených aplikací. 

Nativní aplikace musíte nejprve získat přístupový token ze služby Azure AD a pak ho používat, pokud provedete HTTP požadavků toohello Media Services REST API. Přidáte hlavičku požadavku tokenu toohello hello přístup. 

Hello následující diagram znázorňuje tok ověřování Typická interaktivní aplikace: 

![Diagram nativní aplikace](./media/media-services-use-aad-auth-to-access-ams-api/media-services-native-aad-app1.png)

V předchozím diagramu hello představují hello čísla hello toku hello požadavků v chronologickém pořadí.

> [!NOTE]
> Když používáte metodu ověřování uživatele hello, všechny aplikace sdílet hello stejné ID klienta nativní aplikace (výchozí) a nativní aplikace identifikátor URI pro přesměrování. 

1. Vyzvat uživatele k zadání přihlašovacích údajů.
2. Žádost o přístupový token služby Azure AD s hello následující parametry:  

    * Koncový bod pro klienta Azure AD.

        informace o Hello klienta lze načíst z hello portálu Azure. V pravém horním rohu hello umístěte ukazatel myši hello název hello přihlášeného uživatele.
    * Identifikátor URI prostředku Media Services. 

        Tento identifikátor URI je hello stejné pro účty služby Media Services, které jsou v hello stejné prostředí Azure (například https://rest.media.azure.net).

    * ID klienta aplikace Media Services (nativní).
    * Identifikátor URI pro přesměrování služby Media Services (nativní) aplikace.
    * Identifikátor URI pro REST Media Services.
        
        Hello URI představuje koncový bod REST API hello (například https://test03.restv2.westus.media.azure.net/api/).

    tooget hodnoty těchto parametrů, najdete v části [použít nastavení ověřování Azure tooaccess portálu Azure AD hello](media-services-portal-get-started-with-aad.md) pomocí možnosti ověřování uživatele hello.

3. přístupový token Hello Azure AD se odesílají toohello klienta.
4. Hello klient odešle požadavek toohello REST API služby Azure Media s tokenem přístupu hello Azure AD.
5. Hello klient získá zpět hello data ze služby Media Services.

Informace o tom, jak požaduje toocommunicate ověřování toouse Azure AD se zbytkem pomocí klienta hello Media Services .NET SDK najdete v tématu [používání Azure AD authentication tooaccess hello Media Services API pomocí rozhraní .NET](media-services-dotnet-get-started-with-aad.md). 

Pokud nepoužíváte klienta hello Media Services .NET SDK, musíte ručně vytvořit požadavek tokenu přístupu Azure AD pomocí parametrů hello popsané v kroku 2. Další informace najdete v tématu [jak toouse hello Azure AD Authentication Library tooget hello tokenu Azure AD](../active-directory/develop/active-directory-authentication-libraries.md).

### <a name="service-principal-authentication"></a>Ověřování instančních objektů

Aplikace, které běžně používají tuto metodu ověřování, jsou aplikace, které běží služby střední vrstvy a naplánované úlohy: webové aplikace, funkce aplikací, aplikace logiky, rozhraní API a mikroslužeb. Tato metoda ověřování je také vhodná pro interaktivní aplikace, ve kterých můžete chtít toouse účtu služby toomanage prostředkům.

Při použití hello služby ověření objektu metoda toobuild spotřebitelské scénáře ověřování obvykle se využívá v hello střední vrstvy (prostřednictvím některé rozhraní API) a ne přímo v mobilních nebo desktopových aplikací. 

toouse tato metoda vytvořit aplikaci Azure AD a služby objekt v jeho vlastní klienta. Po vytvoření aplikace hello poskytněte hello aplikace Přispěvatel nebo vlastníka role přístup toohello účtu Media Services. Můžete to uděláte v hello portál Azure, pomocí rozhraní příkazového řádku Azure nebo pomocí skriptu prostředí PowerShell. Můžete taky použít existující aplikaci Azure AD. Můžete zaregistrovat a spravovat aplikaci Azure AD a instanční objekt [v hello portál Azure](media-services-portal-get-started-with-aad.md). Také to provedete pomocí [Azure CLI 2.0](media-services-use-aad-auth-to-access-ams-api.md) nebo [prostředí PowerShell](media-services-powershell-create-and-configure-aad-app.md). 

![Vícevrstvé aplikace](./media/media-services-use-aad-auth-to-access-ams-api/media-services-principal-service-aad-app1.png)

Po vytvoření aplikace Azure AD, získat hodnoty pro hello následující nastavení. Je třeba tyto hodnoty pro ověřování:

- ID klienta 
- Tajný klíč klienta 

V předchozím obrázku hello představují hello čísla hello toku hello požadavků v chronologickém pořadí:
    
1. Vícevrstvé aplikace (webového rozhraní API nebo webové aplikace) požaduje přístupový token služby Azure AD, který má hello následující parametry:  

    * Koncový bod pro klienta Azure AD.

        informace o Hello klienta lze načíst z hello portálu Azure. V pravém horním rohu hello umístěte ukazatel myši hello název hello přihlášeného uživatele.
    * Identifikátor URI prostředku Media Services. 

        Tento identifikátor URI je hello stejné pro účty služby Media Services, které se nacházejí v hello stejné prostředí Azure (například https://rest.media.azure.net).

    * Identifikátor URI pro REST Media Services.

        Hello URI představuje koncový bod REST API hello (například https://test03.restv2.westus.media.azure.net/api/).

    * Hodnoty aplikace Azure AD: hello ID klienta a tajný klíč klienta.
    
    tooget hodnoty těchto parametrů, najdete v části [použít nastavení ověřování Azure tooaccess portálu Azure AD hello](media-services-portal-get-started-with-aad.md) pomocí možnosti ověření objektu služby hello.

2. přístupový token Hello Azure AD se odesílají toohello střední vrstvy.
4. střední vrstvy Hello odešle žádost toohello REST API služby Azure Media s tokenem hello Azure AD.
5. střední vrstvy Hello získá zpět hello data ze služby Media Services.

Další informace o tom, jak požaduje toocommunicate ověřování toouse Azure AD se zbytkem pomocí klienta hello Media Services .NET SDK najdete v tématu [používání Azure AD authentication tooaccess rozhraní API služby Azure Media Services s .NET](media-services-dotnet-get-started-with-aad.md). 

Pokud nepoužíváte klienta hello Media Services .NET SDK, musíte ručně vytvořit požadavek tokenu Azure AD pomocí parametrů, které jsou popsané v kroku 1. Další informace najdete v tématu [jak toouse hello Azure AD Authentication Library tooget hello tokenu Azure AD](../active-directory/develop/active-directory-authentication-libraries.md).

## <a name="troubleshooting"></a>Řešení potíží

Výjimka: "hello na vzdálený server vrátil chybu: (401) Neautorizováno."

Řešení: Hello toosucceed požadavku Media Services REST, hello volání musí být uživatel roli Přispěvatel nebo vlastníka v účtu Media Services, že se pokouší tooaccess hello. Další informace najdete v tématu hello [řízení přístupu](media-services-use-aad-auth-to-access-ams-api.md#access-control) části.

## <a name="resources"></a>Zdroje

Hello následující články jsou Přehled konceptů ověřování Azure AD: 

- [Scénáře ověřování řešené pomocí Azure AD](../active-directory/develop/active-directory-authentication-scenarios.md#basics-of-authentication-in-azure-ad)
- [Přidání, aktualizace nebo odebrání aplikace ve službě Azure AD](../active-directory/develop/active-directory-integrating-applications.md)
- [Konfigurace a správa řízení přístupu na základě Role pomocí prostředí PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md)

## <a name="next-steps"></a>Další kroky

* Pomocí portálu Azure hello příliš[přístup k Azure AD authentication tooconsume rozhraní API služby Azure Media Services](media-services-portal-get-started-with-aad.md).
* Pomocí ověřování Azure AD příliš[přístup k rozhraní API služby Azure Media Services s .NET](media-services-dotnet-get-started-with-aad.md).

