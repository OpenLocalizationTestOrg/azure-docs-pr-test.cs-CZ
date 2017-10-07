---
title: "Azure Active Directory B2C: Vlastní atributy | Microsoft Docs"
description: "Jak toouse vlastních atributů v Azure Active Directory B2C toocollect informací o uživatelích"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 055ffb0a-197b-4716-8dad-1fd8a01e174f
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: fb1bff77ad54c246c6d2f69f39c03eafb76fe6bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-use-custom-attributes-toocollect-information-about-your-consumers"></a>Azure Active Directory B2C: Použijte vlastní atributy toocollect informací o uživatelích
Adresáře Azure Active Directory (Azure AD) B2C se dodává s integrovanou sadu informace (atributy): křestní jméno, příjmení, Město, PSČ a další atributy. Každá aplikace určených však má jedinečné požadavky na jaké atributy toogather z příjemci. S Azure AD B2C můžete rozšířit hello sadu atributů, které jsou uložené na každý uživatelský účet. Můžete vytvořit vlastní atributy pro hello [portál Azure](https://portal.azure.com/) a použít ho v registraci zásady, jak je uvedeno níže. Můžete také číst a zapsat tyto atributy pomocí hello [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).

> [!NOTE]
> Vlastní atributy použití [Azure AD Graph API rozšíření schématu služby Directory](https://msdn.microsoft.com/library/azure/dn720459.aspx).
> 
> 

## <a name="create-a-custom-attribute"></a>Vytvořit vlastní atribut
1. [Postupujte podle těchto kroků toonavigate toohello B2C okno s funkcemi na hello portál Azure](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Klikněte na tlačítko **uživatelské atributy**.
3. Klikněte na tlačítko **+ přidat** hello horní části okna hello.
4. Zadejte **název** pro vlastní atribut hello (například "ShoeSize") a volitelně **popis**. Klikněte na možnost **Vytvořit**.
   
   > [!NOTE]
   > Pouze hello "String" **datový typ** aktuálně nejsou k dispozici.
   > 
   > 

vlastní atribut Hello je nyní k dispozici v seznamu hello **uživatelské atributy**a pro použití ve vaší registrační zásady.

## <a name="use-a-custom-attribute-in-your-sign-up-policy"></a>Použít vlastní atribut ve svojí registrační zásadě
1. [Postupujte podle těchto kroků toonavigate toohello B2C okno s funkcemi na hello portál Azure](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Klikněte na **Zásady registrace**.
3. Klikněte na vaší registrační zásadě (například "B2C_1_SiUp") tooopen ho. Klikněte na tlačítko **upravit** hello horní části okna hello.
4. Klikněte na tlačítko **atributy registrace** a vyberte hello vlastních atributů (například "ShoeSize"). Klikněte na **OK**.
5. Klikněte na tlačítko **deklarace identity aplikace** a vyberte hello vlastních atributů. Klikněte na **OK**.
6. Klikněte na tlačítko **Uložit** hello horní části okna hello.

Můžete vytvořit funkci "Spustit nyní" hello hello zásad tooverify hello prostředí pro uživatele. Teď by měl vidět "ShoeSize" hello seznam atributů shromážděná během registrace příjemce a najdete v části v tokenu odeslaných zpět tooyour aplikace hello.

## <a name="notes"></a>Poznámky
* Společně s registraci zásady vlastní atributy lze také v zásadách registrace nebo přihlášení a zásady pro úpravy profilu.
* Není o známé omezení vlastních atributů. Je pouze vytvořené hello poprvé se používá v žádné zásady, a ne v případě, že ho přidáte toohello seznam **uživatelské atributy**.

