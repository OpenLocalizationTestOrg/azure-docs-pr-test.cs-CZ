---
title: "Azure Active Directory B2C: Resetování hesla pomocí samoobslužné služby | Microsoft Docs"
description: "Téma, který ukazuje, jak nastavit samoobslužné resetování hesla pro uživatele v Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mtillman
editor: curtand
ms.assetid: c87ed86e-1520-42b1-8c31-46cd44ed5310
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: c0a10869477647820828b22b6291522be255844d
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/09/2018
---
# <a name="azure-active-directory-b2c-set-up-self-service-password-reset-for-your-consumers"></a>Azure Active Directory B2C: Nastavení samoobslužného resetování hesla pro uživatele
Funkce resetování hesla pomocí samoobslužné služby s uživatele (kteří zaregistrovali pro místní účty) mohou resetovat svá hesla na své vlastní. To významně snižuje zátěž vašim zaměstnancům technické podpory, zvlášť pokud vaše aplikace nemá milionům příjemcům na základě v pravidelných intervalech. V současné době podporujeme jenom pomocí ověřenou e-mailovou adresu jako metodu obnovení. Přidáme obnovení dodatečné metody (ověřené telefonní číslo, bezpečnostní otázky atd.) v budoucnu.

> [!NOTE]
> Tento článek se týká hesla pomocí samoobslužné služby resetování použít v kontextu zásady přihlášení. Pokud potřebujete zásady resetování plně přizpůsobitelná hesel volána z vaší aplikace, najdete v části [v tomto článku](active-directory-b2c-reference-policies.md#create-a-password-reset-policy).
> 
> 

Ve výchozím nastavení, nebudou mít adresáře hesla pomocí samoobslužné služby resetování zapnutý. Chcete-li pomocí následujících kroků:

1. Přihlaste se k [portál Azure](https://portal.azure.com/) jako správce předplatného. Toto je stejný pracovní nebo školní účet nebo stejný účet Microsoft, který jste použili k vytvoření adresáře.
2. Spustit službu Active Directory (v navigačním panelu na levé straně).
3. Vyberte **vlastnosti**.
4. Přejděte dolů k položce **povolit samoobslužné resetování hesla** části a jeho přepínání **všechny**. 
5. Klikněte na tlačítko **Uložit** v horní části stránky. Hotovo!

K testování, pomocí funkce "Spustit nyní" na všechny zásady přihlášení, který má místní účty jako zprostředkovatele identity. Na přihlášení místní účet stránka (kde zadáte e-mailovou adresu a heslo, nebo uživatelské jméno a heslo), klikněte na tlačítko **nelze získat přístup k účtu?** ověření prostředí pro uživatele.

> [!NOTE]
> Na stránkách resetování hesla pomocí samoobslužné služby lze přizpůsobit pomocí [firemního brandingu funkce](../active-directory/customize-branding.md).
> 
> 

