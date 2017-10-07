---
title: "aaaHow toofill na konkrétní pole vyvinul vlastní aplikace | Microsoft Docs"
description: "Pokyny v jak toofill na konkrétních polí při registraci vlastní aplikace vyvinuté s Azure AD"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 7e07bc45c58542edb3863db5aad7c845f1a1772e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toofill-out-specific-fields-for-a-custom-developed-application"></a>Jak toofill na konkrétní pole vyvinul vlastní aplikace

V tomto článku poskytují stručný popis všech hello dostupná pole ve formuláři pro registraci aplikace hello v hello [portál Azure](https://portal.azure.com).

## <a name="register-a-new-application"></a>Zaregistrujte novou aplikaci

-   tooregister novou aplikaci, přejděte toohello [portál Azure](https://portal.azure.com).

-   V levém navigačním podokně hello, klikněte na **Azure Active Directory.**

-   Zvolte **registrace aplikace** a klikněte na tlačítko **přidat**.

-   Tuto, otevřete si hello aplikace registračním formuláři.

## <a name="fields-in-hello-application-registration-form"></a>Pole ve formuláři pro registraci aplikace hello


| Pole            | Popis                                                                              |
|------------------|------------------------------------------------------------------------------------------|
| Name (Název)             | Hello název aplikace hello. Musí mít minimálně 4 znaky.                |
| Typ aplikace | **Webovou aplikaci nebo webové rozhraní API**: aplikace, která představuje webovou aplikaci, webového rozhraní API nebo obojí 
| |**Nativní**: aplikace, která může být nainstalována na počítači nebo zařízení uživatele           |
| Adresa URL přihlašování      | Hello adresu URL, kde uživatelé můžou přihlásit toouse vaší aplikace                                  |

Jakmile jste vyplnili hello nad poli, aplikace hello být zaregistrována v hello portál Azure a budete přesměrováni toohello stránky aplikace. Hello **nastavení** tlačítko v podokně aplikace hello otevře stránku hello nastavení, která má více polí pro vás toocustomize vaší aplikace. Následující tabulka Hello popisuje všechny hello polí na stránce nastavení hello. Všimněte si, že by vidět jenom podmnožinu těchto polí, v závislosti na tom, jestli jste vytvořili webovou aplikaci nebo nativní aplikaci.

| Pole           | Popis                                                                                                                                                                                                                                                                                                     |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ID aplikace  | Při registraci aplikace Azure AD přiřadí vaší aplikace ID aplikace. ID aplikace Hello se dá použít toouniquely identifikovat aplikaci v tooAzure požadavků ověřování AD, jakož i tooaccess prostředky jako hello rozhraní Graph API.                                                          |
| Identifikátor ID URI aplikace      | Měl by být jedinečný identifikátor URI, obvykle ve formátu hello **https://&lt;klienta\_název&gt;/&lt;aplikace\_název&gt;.** Během hello tok udělení autorizace, slouží jako jedinečný identifikátor toospecify hello prostředek by měl být které hello token vydán pro. Změní také hello oblast deklarace hello vydaný přístupový token. |
| Nahrát nové logo | Tato tooupload logo, které můžete použít pro vaši aplikaci. Hello loga musí být ve formátu BMP, JPG nebo PNG a velikost souboru hello by měla být menší než 100KB. Hello dimenze pro bitovou kopii hello by měly být 215 x 215 pixelů, s dimenzemi v centrální image 94 x 94 pixelů.                                                       |
| Adresa URL domovské stránky   | Toto je hello přihlašování v adrese URL zadané při registraci aplikace.                                                                                                                                                                                                                                              |
| Adresa URL odhlašovací      | Tato adresa URL odhlásit jediné odhlašování hello. Azure AD odešle požadavek toothis URL odhlašovací při hello uživatele vymaže jejich relace s Azure AD pomocí jiných zaregistrovanou aplikaci.                                                                                                                                       |
| Více-nevyužívá dělené tabulky  | Tento přepínač určuje, zda lze aplikace hello víc klientů. Obvykle to znamená, že externími organizacemi moci používat vaši aplikaci tak, že registrace v jejich klienta a udělení přístupu tootheir organizace data.                                                                   |
| Adresy URL odpovědí      | adresy URL odpovědí Hello jsou hello koncových bodů, kde Azure AD vrátí všechny tokeny, které vaše aplikace požaduje.                                                                                                                                                                                                          |
| Identifikátory URI přesměrování   | U nativních aplikací je to kde hello uživatel být odeslané toofollowing úspěšném ověření. Identifikátor URI požadavku zdrojů vaší aplikace, které v hello OAuth 2.0 odpovídá jednomu z hodnoty hello zaregistrován portálu hello přesměrování Azure AD zkontrolujte, zda text hello.                                                            |
| Klíče            | Můžete vytvořit klíče tooprogrammatically přístup webové rozhraní API pro zabezpečené službou Azure AD bez nutnosti zásahu uživatele. Z hello \* \*klíče\* \* stránky, zadejte popis a hello klíče datum vypršení platnosti a uložte klíč toogenerate hello. Ujistěte se, že toosave někde zabezpečení, jak nebudete moct tooaccess později.             |

## <a name="next-steps"></a>Další kroky
[Správa aplikací pomocí služby Azure Active Directory](active-directory-enable-sso-scenario.md)
