---
title: "aaaGet spuštěné s ověřováním Azure AD pomocí portálu Azure hello | Microsoft Docs"
description: "Zjistěte, jak toouse hello Azure portálu tooaccess tooconsume ověřování Azure Active Directory (Azure AD) hello rozhraní API služby Azure Media Services."
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
ms.openlocfilehash: 497ad1806b3fd1262802adefb6e12b65ee9f2d61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-ad-authentication-by-using-hello-azure-portal"></a>Začínáme s ověřováním Azure AD pomocí portálu Azure hello

Zjistěte, jak toouse hello Azure tooaccess portálu služby Azure Active Directory (Azure AD) ověřování tooaccess hello rozhraní API služby Azure Media Services.

## <a name="prerequisites"></a>Požadavky

- Účet Azure. Pokud nemáte účet, začínat [bezplatné zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/). 
- Účet Media Services. Další informace najdete v tématu [vytvoření účtu Azure Media Services pomocí portálu Azure hello](media-services-portal-create-account.md).
- Ujistěte se, zkontrolujte hello [přístup k Azure Media Services API s Azure AD authentication přehled](media-services-use-aad-auth-to-access-ams-api.md). 

Při použití ověřování Azure AD pomocí služby Azure Media Services, máte dvě možnosti ověřování:

- **Ověření uživatele**. Ověření osoba, která používá toointeract aplikace hello s prostředky Media Services. interaktivní aplikace Hello měli nejdřív hello uživatele vyzve k přihlašovací údaje. Příklad je používán oprávněným uživatelům toomonitor kódování úlohy správy konzoly aplikace nebo živé streamování. 
- **Objekt zabezpečení ověřování služby**. Ověření služby. Aplikace, které běžně používají tuto metodu ověřování, jsou aplikace, které běží služby démon, střední vrstvy služby nebo naplánované úlohy: webové aplikace, funkce aplikace, aplikace logiky, rozhraní API nebo mikroslužby.

> [!IMPORTANT]
> V současné době Media Services podporuje model ověřování služby Řízení přístupu Azure hello. Řízení přístupu autorizace však bude na 1. června 2018 zastaralá. Doporučujeme, abyste co nejdříve migraci model ověřování toohello Azure AD.

## <a name="select-hello-authentication-method"></a>Vyberte metodu ověřování hello

1. V hello [portál Azure](https://portal.azure.com/), vyberte svůj účet Media Services.
2. Vyberte, jak tooconnect toohello Media Services API.

    ![Vyberte stránku metoda připojení](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started01.png)

## <a name="user-authentication"></a>Ověřování uživatelů

tooconnect toohello Media Services API pomocí hello možnost ověřování uživatelů, klientská aplikace hello musí toorequest tokenu Azure AD, který má hello následující parametry:  

* Koncový bod tenant Azure AD
* Identifikátor URI prostředku Media Services
* ID klienta aplikace Media Services (nativní) 
* Identifikátor URI pro přesměrování aplikace Media Services (nativní) 
* Identifikátor URI pro REST Media Services

Můžete získat hello hodnoty pro tyto parametry hello **Media Services API s ověřováním uživatele** stránky. 

![Připojení k stránce ověřování uživatele](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started02.png)

Pokud připojíte toohello Media Services API pomocí hello Media Services Microsoft .NET SDK, hello požadované hodnoty jsou k dispozici tooyou jako součást hello SDK. Další informace najdete v tématu [používání Azure AD authentication tooaccess hello rozhraní API služby Azure Media Services s .NET](media-services-dotnet-get-started-with-aad.md).

Pokud nepoužíváte klienta hello Media Services .NET SDK, musíte ručně vytvořit požadavek tokenu Azure AD pomocí parametrů hello uvedenými výše. Další informace najdete v tématu [jak toouse hello Azure AD Authentication Library tooget hello tokenu Azure AD](../active-directory/develop/active-directory-authentication-libraries.md).

## <a name="service-principal-authentication"></a>Ověřování instančních objektů

tooconnect toohello Media Services API pomocí hello hlavní možnosti služby, vaše aplikace střední vrstvy (webového rozhraní API nebo webové aplikace) musí toorequest tokenu Azure AD, který má hello následující parametry:  

* Koncový bod tenant Azure AD
* Identifikátor URI prostředku Media Services 
* Identifikátor URI pro REST Media Services
* Hodnoty aplikace Azure AD: hello **ID klienta** a **tajný klíč klienta**

Můžete získat hello hodnoty pro tyto parametry hello **tooMedia rozhraní API služby připojit pomocí objektu služby** stránky. Pomocí této stránky toocreate nové Azure aplikaci AD nebo tooselect některého ze stávajících. Po výběru hello aplikaci Azure AD, můžete získat hello ID klienta (ID aplikace) a generovat hello klienta tajný klíč (klíč) hodnoty. 

![Připojení k hlavní stránce služby](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started04.png)

Když hello **instanční objekt** otevře se okno, je vybrána hello první aplikaci Azure AD splňující hello následující kritéria:

- Je registrovaný aplikaci Azure AD.
- Na hello účet má oprávnění přispěvatele nebo Owner Role-Based řízení přístupu.

Po vytvoření nebo vyberte aplikaci Azure AD, můžete vytvořit a zkopírovat tajný klíč klienta (klíč) a hello ID klienta (ID aplikace). ID klienta tajný klíč a klienta Hello jsou požadované tooget hello přístupový token v tomto scénáři.

Pokud nemáte oprávnění aplikace Azure AD toocreate ve vaší doméně, nejsou zobrazeny ovládací prvky aplikace Azure AD hello hello okně a zobrazí zpráva s upozorněním.

Pokud připojíte toohello Media Services API pomocí hello sady Media Services .NET SDK, přečtěte si téma [používání Azure AD authentication tooaccess hello rozhraní API služby Azure Media Services s .NET](media-services-dotnet-get-started-with-aad.md).

Pokud nepoužíváte klienta hello Media Services .NET SDK, musíte ručně vytvořit požadavek tokenu Azure AD pomocí parametrů hello uvedenými výše. Další informace najdete v tématu [jak toouse hello Azure AD Authentication Library tooget hello tokenu Azure AD](../active-directory/develop/active-directory-authentication-libraries.md).

### <a name="get-hello-client-id-and-client-secret"></a>Získat ID a klienta sdílený tajný klíč klienta hello

Po výběru existující aplikace Azure AD nebo toocreate hello vyberte možnost Nový, objeví se hello následující tlačítka:

![Správa oprávnění tlačítka a Správa aplikací](./media/media-services-portal-get-started-with-aad/media-services-portal-manage.png)

Klikněte na tlačítko tooopen hello okna aplikací Azure AD, **správě aplikace**. Na hello **správě aplikace** okně můžete získat ID klienta aplikace hello (ID aplikace). toogenerate tajný klíč klienta (klíč), vyberte **klíče**.

![Spravovat aplikace okna klíče možnost](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started06.png) 

### <a name="manage-permissions-and-hello-application"></a>Správa oprávnění a aplikace hello

Jakmile vyberete aplikace hello Azure AD, můžete spravovat aplikace hello a oprávnění. tooset si vaše tooaccess aplikace Azure AD jiné aplikace, klikněte na **spravovat oprávnění**. Pro úlohy správy, jako je například změna klíčů a adresy URL odpovědí nebo manifest aplikace hello tooedit, klikněte na tlačítko **správě aplikace**.

### <a name="edit-hello-apps-settings-or-manifest"></a>Upravit nastavení aplikace hello nebo manifest

nastavení aplikace hello tooedit nebo manifest, klikněte na tlačítko **správě aplikace**.

![Spravovat stránky aplikace](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started05.png)

## <a name="next-steps"></a>Další kroky

Začínáme s [nahrávání souborů tooyour účet](media-services-portal-upload-files.md).
