---
title: "aaaAzure protokoly pro ověřování Active Directory | Microsoft Docs"
description: "Přehled hello ověřovací protokoly podporované pomocí Azure Active Directory (AD)"
documentationcenter: dev-center-name
author: priyamohanram
services: active-directory
manager: mbaldwin
editor: 
ms.assetid: 7a838ae2-c24c-4304-b6c0-e77fb888e6c0
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: priyamo
ms.custom: aaddev
ms.openlocfilehash: 1584efa83d30746075e970b8523c3abdccd34859
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# Protokoly pro ověřování Azure Active Directory
Azure Active Directory (Azure AD) podporuje několik protokolů hello nejčastěji používá ověřování a autorizace. Hello témata v této části popisují hello podporované protokoly a jejich provádění ve službě Azure AD. témata Hello zahrnuté kontrolu podporované typy deklarací identity, použití toohello Úvod federačních metadat, podrobného OAuth 2.0. a referenční dokumentace protokolu SAML 2.0 a část s řešením potíží.

## Protokoly ověřování články a referenční informace
* [Důležité informace o podpisový klíč výměny ve službě Azure AD](active-directory-signing-key-rollover.md) – Další informace o Azure AD podepisování cadence výměny klíčů, změny, které provedete tooupdate hello klíč automaticky a diskusi k jak tooupdate hello nejběžnějších scénářů aplikace.
* [Podporované typy deklarací identity a Token](active-directory-token-and-claims.md) -Další informace o deklaracích identity hello v hello tokeny, které Azure AD vydá.
* [Federační Metadata](active-directory-federation-metadata.md) – zjistěte, jak toofind a interpretovat hello dokumenty metadat, které generuje Azure AD.
* [OAuth 2.0 ve službě Azure AD](active-directory-protocols-oauth-code.md) -Další informace o implementaci hello OAuth 2.0 ve službě Azure AD.
* [OpenID Connect 1.0](active-directory-protocols-openid-connect-code.md) – zjistěte, jak toouse OAuth 2.0, ověřovací protokol, pro ověřování.
* [Služby volání tooService s pověření klienta](active-directory-protocols-oauth-service-to-service.md) – zjistěte, jak tok pro volání služby tooservice poskytování pověření klienta toouse OAuth 2.0.
* [Služby volání tooService s tokem On-Behalf-Of](active-directory-protocols-oauth-on-behalf-of.md) – zjistěte, jakým způsobem volá toouse toku OAuth 2.0 On-Behalf-Of pro tooservice služby.
* [Referenční informace o protokolu SAML](active-directory-saml-protocol-reference.md) -Další informace o profily jednotného přihlašování a jeden Sign-out SAML hello Azure AD.

## Viz také
[Příručka pro vývojáře Azure Active Directory](active-directory-developers-guide.md)

[Pro ověřování pomocí služby Azure AD](../../app-service-web/web-sites-authentication-authorization.md)

[Ukázky kódu služby Active Directory](active-directory-code-samples.md)
