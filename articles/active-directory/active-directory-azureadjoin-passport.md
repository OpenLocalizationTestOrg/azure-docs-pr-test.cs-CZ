---
title: "aaaAuthenticating identit bez hesel pomocí Windows Hello pro firmy a Azure AD | Microsoft Docs"
description: "Obsahuje přehled Windows Hello pro firmy a další informace o nasazení Windows Hello pro firmy."
services: active-directory
documentationcenter: 
author: femila
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: f907bb90-8776-46ca-9e12-279949af66ff
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 7c1c52e10b7ab7a89ec3226ffa7cf01896267871
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="authenticating-identities-without-passwords-through-windows-hello-for-business"></a>Ověřování identit bez hesel prostřednictvím Windows Hello pro firmy
Hello aktuální metody ověřování pomocí hesla samostatně nejsou dostatečná tookeep uživateli bezpečné. Uživatelé opakovaně používat a zapomněli hesla. Hesla jsou breachable, phishable, náchylné k chybám toocracks a uhodnutelných. Získají obtížné tooremember a náchylné k chybám tooattacks jako "[předat hello hash](https://technet.microsoft.com/dn785092.aspx)".

## <a name="about-windows-hello-for-business"></a>O Windows Hello pro firmy
Windows Hello pro firmy je soukromého a veřejného klíče nebo ověřování pomocí certifikátů přístup pro organizace a spotřebitelé, která přesahuje hesla. Tato forma ověřování spoléhá na pár klíčů přihlašovací údaje, které může nahradit hesla a jsou odolné toobreaches, krádeže phishing.

 Windows Hello pro firmy umožňuje uživateli ověření účtu Microsoft tooa, účet systému Windows Server Active Directory, účet Microsoft Azure Active Directory (Azure AD) nebo službu jiných společností než Microsoft, která podporuje ověřování rychlého IDentity Online (FIDO). Po počáteční dvoustupňové ověřování během Windows Hello pro firmy registraci Windows Hello pro firmy je nastavený na zařízení uživatele hello a hello uživatel nastaví gesto, což může být Windows Hello nebo PIN kód. uživatel Hello poskytuje hello gesto tooverify svou identitu. Systém Windows pak používá Windows Hello pro firmy tooauthenticate hello uživatele a snadněji služby a prostředky tooaccess chráněný.

Hello privátní klíč je k dispozici výhradně pomocí "gesto uživatele" jako PIN kód, biometrické nebo vzdáleném zařízení, jako jsou čipové karty, která hello uživatel používá toosign toohello zařízení. Tyto informace jsou propojené tooa certifikát nebo asymetrický dvojici klíčů. privátní klíč Hello je ověřeno, pokud má zařízení hello čipu Trusted Platform Module (TPM) hardwaru. privátní klíč Hello nikdy neopustí hello zařízení.

veřejný klíč Hello není registrována s Azure Active Directory a Windows Server Active Directory (pro místní). Zprostředkovatelů identity (IDPs) ověřte hello uživatele tak, že mapování hello veřejný klíč hello uživatele toohello soukromý klíč a zadejte přihlašovací údaje prostřednictvím jednoho čas heslem (OTP), PhoneFactor nebo mechanismus různých oznámení.

## <a name="why-enterprises-should-adopt-windows-hello-for-business"></a>Proč podniky přijmout Windows Hello pro firmy
Když zapnete Windows Hello pro firmy, podniky můžete zabezpečit jejich prostředky i podle:

* Nastavení Windows Hello pro firmy s možností preferované hardwaru. To znamená, že se budou generovat klíče na čip TPM 1.2 nebo TPM 2.0, pokud je k dispozici. Když není TPM k dispozici, bude software vygenerování klíče hello.
* Definující hello složitost a délka hello kód PIN a určuje, jestli je povolená Hello využití ve vaší organizaci.
* Konfigurace Windows Hello pro firmy toosupport čipové karty jako scénáře pomocí vztahu důvěryhodnosti na základě certifikátů.

## <a name="how-windows-hello-for-business-works"></a>Jak Windows Hello pro firmy funguje
1. Klíče generované na hardwaru hello TPM nebo softwaru. Mnoho zařízení mají předdefinovaný čip TPM, který zabezpečuje hello hardwaru integrací kryptografické klíče do zařízení. Čip TPM 1.2 nebo TPM 2.0 generuje klíče ani certifikáty, které jsou vytvořené pomocí klíče generované hello.
2. Hello TPM osvědčuje tyto klíče vázané na hardwaru.
3. Gesto jeden odemčení odemkne hello zařízení. Tato gesto umožňuje přístup k prostředkům toomultiple Pokud hello zařízení připojených k doméně nebo Azure AD připojený.

## <a name="how-hello-windows-hello-for-business-lifecycle-works"></a>Jak funguje hello Windows Hello pro firmy životního cyklu
![Windows Hello pro firmy životního cyklu](./media/active-directory-azureadjoin/active-directory-azureadjoin-microsoft-passport.png)

Hello předchozí diagram znázorňuje hello soukromého a veřejného klíče dvojice a ověření hello hello poskytovatele identit. Každý z těchto kroků je podrobně popsány zde:

1. Hello uživatel prokáží jejich identitu prostřednictvím několik předdefinovaných kontroly pravopisu metod (gesta, fyzických čipových karet, služba Multi-Factor authentication) a odešle tato informace tooan zprostředkovatele Identity (IDP) jako Azure Active Directory nebo místní služby Active Directory.
2. zařízení Hello vytvoří hello klíč, osvědčuje hello klíč, trvá hello veřejnou část tohoto klíče, připojí s příkazy stanice, přihlášení a odešle ho toohello IDP tooregister hello klíč.
3. Jakmile hello IDP zaregistruje hello veřejné části hello klíče, hello IDP výzvy hello toosign zařízení s hello část privátního klíče hello.
4. Hello IDP pak ověří a problémy hello ověřovací token, která umožňuje hello uživatele a hello zařízení přístup k chráněné hello prostředkům. IDPs můžete napsat aplikací pro různé platformy nebo použít prohlížeč podporu (přes rozhraní API jazyka JavaScript nebo Webcrypto) toocreate a používat Windows Hello pro firmy přihlašovací údaje pro své uživatele.

## <a name="hello-deployment-requirements-for-windows-hello-for-business"></a>požadavky na nasazení Hello pro Windows Hello pro firmy
### <a name="at-hello-enterprise-level"></a>Na úrovni podniku hello
* Hello enterprise má předplatné Azure.

### <a name="at-hello-user-level"></a>Na úrovni uživatele hello
* Hello počítač běží Windows 10 Professional nebo Enterprise.

Podrobné pokyny pro nasazení, najdete v části [povolit Windows Hello pro firmy v organizaci hello](active-directory-azureadjoin-passport-deployment.md).

## <a name="additional-information"></a>Další informace
* [Windows 10 pro podnik hello: způsoby toouse zařízení pro práci](active-directory-azureadjoin-windows10-devices-overview.md)
* [Rozšíření cloudových funkcí tooWindows 10 zařízení prostřednictvím Azure Active Directory Join](active-directory-azureadjoin-user-upgrade.md)
* [Další informace o scénářích použití pro službu Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Připojení zařízení připojených k doméně tooAzure AD pro prostředí Windows 10](active-directory-azureadjoin-devices-group-policy.md)
* [Nastavení služby Azure AD Join](active-directory-azureadjoin-setup.md)

