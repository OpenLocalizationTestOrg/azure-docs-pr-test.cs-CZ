---
title: "Azure Active Directory B2C: Resetování hesla pomocí samoobslužné služby | Microsoft Docs"
description: "Téma ukázka, jak resetování tooset si hesla pomocí samoobslužné služby pro uživatele v Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: curtand
ms.assetid: c87ed86e-1520-42b1-8c31-46cd44ed5310
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: ff87eea42259b610702da73af35d0a38eb7dd33d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-set-up-self-service-password-reset-for-your-consumers"></a>Azure Active Directory B2C: Nastavení samoobslužného resetování hesla pro uživatele
S hello samoobslužného resetování hesel funkce, uživatele (kteří zaregistrovali pro místní účty) mohou resetovat svá hesla na své vlastní. To významně snižuje zatížení hello vašim zaměstnancům technické podpory, zvlášť pokud vaše aplikace nemá milionům příjemcům na základě v pravidelných intervalech. V současné době podporujeme jenom pomocí ověřenou e-mailovou adresu jako metodu obnovení. V budoucnu hello přidáme obnovení dodatečné metody (ověřené telefonní číslo, bezpečnostní otázky atd.).

> [!NOTE]
> Tento článek se týká tooself služby heslo resetovat použít v kontextu hello zásady přihlášení. Pokud potřebujete zásady resetování plně přizpůsobitelná hesel volána z vaší aplikace, najdete v části [v tomto článku](active-directory-b2c-reference-policies.md#create-a-password-reset-policy).
> 
> 

Ve výchozím nastavení, nebudou mít adresáře hesla pomocí samoobslužné služby resetování zapnutý. Použití hello následující kroky tooturn ho na:

1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com/) jako hello správce předplatného. Toto je stejný pracovní nebo školní účet nebo hello stejný účet Microsoft, kterou jste použili toocreate adresáře hello.
2. Přejděte toohello rozšíření Active Directory na hello navigačním panelu na levé straně hello.
3. Najít adresáře v rámci hello **Directory** kartě a klikněte na něj.
4. Klikněte na tlačítko hello **konfigurace** kartě.
5. Projděte dolů toohello **zásady resetování hesel uživatelů** části a přepínání hello **uživatele povolen pro resetování hesla** možnost příliš**Ano**. Všimněte si, že hello **alternativní e-mailovou adresu** zaškrtnutá možnost; necháte, protože se jedná.
   
    ![Samoobslužné resetování hesla](./media/active-directory-b2c-reference-sspr/sspr.png)
6. Klikněte na tlačítko **Uložit** v hello dolní části stránky hello. Hotovo!

tootest, použít funkci "Spustit nyní" hello na všechny zásady přihlášení, který má místní účty jako zprostředkovatele identity. Na přihlášení místní účet hello stránka (kde zadáte e-mailovou adresu a heslo, nebo uživatelské jméno a heslo), klikněte na tlačítko **nelze získat přístup k účtu?** tooverify hello prostředí pro uživatele.

> [!NOTE]
> Hello stránky resetování hesla pomocí samoobslužné služby lze přizpůsobit pomocí hello [firemního brandingu funkce](../active-directory/active-directory-add-company-branding.md).
> 
> 

