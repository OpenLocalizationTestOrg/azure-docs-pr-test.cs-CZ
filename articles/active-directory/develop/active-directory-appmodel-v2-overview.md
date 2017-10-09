---
title: "koncový bod v2.0 aaaAzure služby Active Directory | Microsoft Docs"
description: "Aplikace Úvod toobuilding Account Microsoft a Azure Active Directory přihlášení."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 2dee579f-fdf6-474b-bc2c-016c931eaa27
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: ae5946b02c50ae5a6137dc1decae66c96647e875
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sign-in-microsoft-account--azure-ad-users-in-a-single-app"></a>Přihlášení uživatele Azure AD v jediné aplikaci & Account Microsoft
V hello po vývojář aplikace, kteří chtěli toosupport i osobní účty Microsoft a pracovních účtů ze služby Azure Active Directory byla požadovaná toointegrate dva samostatné systémy.  Hello **koncového bodu Azure AD v2.0** zavádí novou verzi ověřování rozhraní API, která vám umožní toosign v obou typech účty pomocí jednoho Jednoduchá integrace.  Aplikace, které používají koncového bodu v2.0 hello můžete také používat rozhraní REST API z hello [Microsoft Graph](https://graph.microsoft.io) pomocí buď typu účtu.

## <a name="getting-started"></a>Začínáme
Vyberte oblíbenou platformu z hello následující seznamu toobuild aplikace pomocí našich opensourcové knihovny & architektury.  Alternativně můžete použít toosend naše OAuth 2.0 & OpenID Connect dokumentaci protokolu a přijímat zprávy protokolu přímo bez použití knihovny ověřování.

<br />

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]

## <a name="whats-new"></a>Novinky
Zde Hello informace budou užitečné porozumět, co je & co není možné s koncovým bodem v2.0 hello.

* Další informace o hello [typy aplikací můžete vytvořit s koncovým bodem v2.0 hello](active-directory-v2-flows.md).
* Pochopení hello [omezení, omezení a omezení](active-directory-v2-limitations.md) s koncovým bodem v2.0 hello.
* Podívejte se na tento přehled – video pro koncový bod v2.0 hello:

>[!VIDEO https://channel9.msdn.com/Events/Build/2017/P4031/player]

## <a name="reference"></a>Referenční informace
Následující odkazy budou užitečné při prozkoumávání hello platformy podrobněji:

* [referenční informace o protokolu v2.0](active-directory-v2-protocols.md)
* [Odkaz tokenu v2.0](active-directory-v2-tokens.md)
* [Referenční příručka knihovny v2.0](active-directory-v2-libraries.md)
* [Obory a souhlasu v koncového bodu v2.0 hello](active-directory-v2-scopes.md)
* [Hello Microsoft Graph](https://graph.microsoft.io)

## <a name="help--support"></a>Nápověda a podpora
Jedná se o hello nejlepší místech tooget pomoc s vývojem v Azure Active Directory.

* [Značky `azure-active-directory` a `adal` na Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory+or+adal)
* [Zpětná vazba k Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/category/164757-developer-experiences)


> [!NOTE]
> Pokud potřebujete jenom toosign v pracovním a školním účtům z Azure Active Directory, měli byste začít s naše [Příručka pro vývojáře Azure AD](active-directory-developers-guide.md).  koncový bod v2.0 Hello je určená pro vývojáře, kteří potřebují explicitně toosign v osobní účty Microsoft.

