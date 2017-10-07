---
title: "Služby Azure Active Directory B2C: Vícefaktorové ověřování | Microsoft Docs"
description: "Jak tooenable služby Multi-Factor Authentication v aplikacích určených zabezpečené pomocí Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 53ef86c4-1586-45dc-9952-dbbd62f68afc
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 29beb1e6ab5d8ab5a65f9c5c068d9e71c60418dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-enable-multi-factor-authentication-in-your-consumer-facing-applications"></a>Azure Active Directory B2C: Povolení služby Multi-Factor Authentication v aplikacích určených uživatelům
Azure Active Directory (Azure AD) B2C se integruje přímo s [Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md) , mohli přidat druhou vrstvu zabezpečení prostředí toosign-up a přihlašování v aplikacích určených uživatelům. A můžete to provést bez nutnosti napsat jediný řádek kódu. Momentálně podporujeme telefonního hovoru a textové zprávy ověření. Pokud jste již vytvořili zásady registrace a přihlášení, stále můžete povolit službu Multi-Factor Authentication.

> [!NOTE]
> Služba Multi-Factor Authentication lze povolit taky při vytváření zásad registrace a přihlášení, nikoli pouze úpravou existující zásady.
> 
> 

Tato funkce vám pomůže aplikace zpracovávat scénáře, jako je hello následující:

* Nevyžadují službu Multi-Factor Authentication tooaccess jednu aplikaci, ale potřebujete ho tooaccess jiný. Například hello příjemce může přihlásit k aplikaci automaticky pojištění přes sociální nebo místní účet, ale před přístup k domovskému pojištění aplikaci hello zaregistrovaný v hello stejné musí ověřit hello telefonní číslo adresáře.
* Nevyžadují službu Multi-Factor Authentication tooaccess aplikace obecně, ale potřebujete ho tooaccess hello citlivých částí v něm. Například hello příjemce může přihlásit tooa bankovnictví aplikace s sociálních nebo místní účet, zkontrolujte zůstatek účtu, ale musí ověřit hello telefonní číslo, před pokusem o přenos přenosu.

## <a name="modify-your-sign-up-policy-tooenable-multi-factor-authentication"></a>Upravit tooenable vaší registrační zásady vícefaktorového ověřování
1. [Postupujte podle těchto kroků toonavigate toohello B2C okno s funkcemi na hello portál Azure](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Klikněte na **Zásady registrace**.
3. Klikněte na vaší registrační zásadě (například "B2C_1_SiUp") tooopen ho.
4. Klikněte na tlačítko **služby Multi-Factor authentication** a vypnout hello **stavu** příliš**ON**. Klikněte na **OK**.
5. Klikněte na tlačítko **Uložit** hello horní části okna hello.

Můžete vytvořit funkci "Spustit nyní" hello hello zásad tooverify hello prostředí pro uživatele. Potvrďte hello následující:

Uživatelský účet se vytvoří ve vašem adresáři předtím, než dojde k hello kroku Multi-Factor Authentication. Během kroku hello hello příjemce požaduje tooprovide jejího telefonní číslo a ověřte ji. Pokud je ověření úspěšné, je připojen hello telefonní číslo toohello uživatelský účet pro pozdější použití. I když příjemce hello zruší nebo potlačení, potvrdí můžete být vyzváni tooverify telefonní číslo znovu během hello příštím přihlášení (s povolenou službou Multi-Factor Authentication).

## <a name="modify-your-sign-in-policy-tooenable-multi-factor-authentication"></a>Upravit tooenable vaší registrační zásady vícefaktorového ověřování
1. [Postupujte podle těchto kroků toonavigate toohello B2C okno s funkcemi na hello portál Azure](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Klikněte na tlačítko **přihlášení zásady**.
3. Klikněte na vaší přihlašovací zásad (například "B2C_1_SiIn") tooopen ho. Klikněte na tlačítko **upravit** hello horní části okna hello.
4. Klikněte na tlačítko **služby Multi-Factor authentication** a vypnout hello **stavu** příliš**ON**. Klikněte na **OK**.
5. Klikněte na tlačítko **Uložit** hello horní části okna hello.

Můžete vytvořit funkci "Spustit nyní" hello hello zásad tooverify hello prostředí pro uživatele. Potvrďte hello následující:

Když příjemce hello přihlásí (s použitím sociálních nebo místní účet), pokud ověřený telefonní číslo je připojen toohello uživatelský účet se se zobrazí výzva, tooverify ho. Pokud je připojen bez telefonního čísla, hello příjemce se zobrazí výzva, tooprovide jeden a ověřte ji. Na úspěšné ověřování, je připojen hello telefonní číslo toohello uživatelský účet pro pozdější použití.

## <a name="multi-factor-authentication-on-other-policies"></a>Služba Multi-Factor Authentication na další zásady
Jak je popsáno pro registrace a přihlášení zásady, výše, je také možné tooenable služby Multi-Factor authentication registrace nebo přihlášení zásady a heslo resetovat zásady. Bude k dispozici co nejdříve na zásady pro úpravy profilu.

