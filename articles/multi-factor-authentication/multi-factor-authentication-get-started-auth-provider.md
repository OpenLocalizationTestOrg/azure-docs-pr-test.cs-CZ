---
title: "aaaGet spuštění zprostředkovatel vícefaktorového ověřování Azure | Microsoft Docs"
description: "Zjistěte, jak toocreate poskytovatele Azure Multi-Factor Auth."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: a7dd5030-7d40-4654-8fbd-88e53ddc1ef5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/28/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: 00ea967a80b43baff38c1de586c54d95c9abac2c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-an-azure-multi-factor-auth-provider"></a>Začínáme s poskytovatelem ověřování Azure Multi-Factor Auth
Dvoustupňové ověřování je k dispozici ve výchozím nastavení pro globální správce, kteří mají uživatele služeb Azure Active Directory a Office 365. Ale pokud chcete využít tootake [pokročilé funkce](multi-factor-authentication-whats-next.md) pak by měl nákupu hello plnou verzi Azure Multi-Factor Authentication (MFA).

Se používá poskytovatele Azure Multi-Factor Auth tootake výhod funkcí poskytovaných plnou verzi Azure MFA hello. Je určený pro uživatele, kteří **nemají licence prostřednictvím Azure MFA, Azure AD Premium nebo Enterprise Mobility + Security (EMS)**.  Azure MFA, Azure AD Premium a EMS zahrnují hello plnou verzi Azure MFA ve výchozím nastavení. Pokud máte licence, nepotřebujete poskytovatele Azure Multi-Factor Auth.

Zprostředkovatele služby Azure Multi-Factor Auth je požadovaná toodownload hello SDK.

> [!IMPORTANT]
> toodownload hello SDK, je nutné toocreate poskytovatele Azure Multi-Factor Auth i v případě, že máte licence Azure MFA, AAD Premium nebo EMS.  Pokud je pro tento účel vytvořit poskytovatele Azure Multi-Factor Auth a už máte licence, se toocreate hello zda zprostředkovatele s hello **za povoleného uživatele** modelu. Pak propojte hello zprostředkovatele toohello adresář, který obsahuje hello licence Azure MFA, Azure AD Premium nebo EMS. Tato konfigurace zajistí, že se pouze účtují Pokud máte více jedinečných uživatelů, kteří provádění dvoustupňové ověření, než hello počet licencí, které vlastníte.

## <a name="what-is-an-azure-multi-factor-auth-provider"></a>Co je poskytovatele ověřování Azure Multi-Factor Auth?

Pokud nemáte licence pro ověřování Azure Multi-Factor Authentication, můžete vytvořit ověřování zprostředkovatele toorequire dvoustupňové ověřování pro vaše uživatele. Pokud jsou vývoj vlastní aplikace a chcete tooenable Azure MFA, vytvářet poskytovatele ověřování a [stáhnout hello SDK](multi-factor-authentication-sdk.md).

Existují dva typy zprostředkovatelé ověřování a hello rozdíl je kolem jak účtovat vašeho předplatného Azure. možnost za ověření Hello vypočítá hello počet ověření adresovat vašeho klienta za měsíc. Tato možnost je vhodná, pokud máte řadu uživatelů, kteří se ověřují jenom občas, například pokud vícefaktorové ověřování vyžadujete pro vlastní aplikaci. možnost uživatelská Hello vypočítá počet hello jednotlivce ve vašem klientovi, kteří provádějí dvoustupňové ověřování za měsíc. Tato možnost je vhodné, pokud mají někteří uživatelé s licencí ale potřebovat tooextend MFA toomore uživatelů nad rámec licenční omezení.

## <a name="create-a-multi-factor-auth-provider"></a>Vytvoření poskytovatele Multi-Factor Auth
Pomocí následujících kroků toocreate poskytovatele Azure Multi-Factor Auth hello. Azure zprostředkovatelé vícefaktorového ověřování mohou být vytvořeny pouze v hello portál Azure classic. Pokud nemůžete se přihlásit toohello portál Azure classic, zkontrolujte toomake se, že je váš klient Azure AD [přidružené předplatné Azure](../active-directory/active-directory-how-subscriptions-associated-directory.md). 

1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com) jako správce.
2. Na levé straně hello vyberte **služby Active Directory**.
3. Na stránce služby Active Directory hello hello horní, vyberte **zprostředkovatelé vícefaktorového ověřování**.
   
   ![Vytvoření poskytovatele MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider1.png)

4. V dolní části hello, klikněte na položku **nový**.
   
   ![Vytvoření poskytovatele MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider2.png)

5. V části App Services vyberte **Poskytovatel Multi-Factor Auth**.
   
   ![Vytvoření poskytovatele MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider3.png)

6. Vyberte možnost **Rychle vytvořit**.
   
   ![Vytvoření poskytovatele MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider4.png)

7. Vyplňte následující pole hello a vyberte **vytvořit**.
   1. **Název** – hello název hello zprostředkovatel vícefaktorového ověřování.
   2. **Model použití** – Zvolte jednu ze dvou možností:
      * Za ověření – nákupní model, který účtuje za ověření. Obvykle se používá pro scénáře, které používají Azure Multi-Factor Authentication v aplikaci zaměřené na spotřebitele.
      * Pro povolené uživatele – nákupní model, který účtuje za povoleného uživatele. Obvykle se používá pro přístup tooapplications zaměstnanec například Office 365. Tuto možnost vyberte, pokud máte uživatele, kteří už mají licence na Azure MFA.
   3. **Adresář** – hello Azure Active Directory klienta tohoto hello je přidružen poskytovatel Multi-Factor Authentication. Uvědomte si, hello následující:
      * Není nutné toocreate adresář služby Azure AD zprostředkovatel vícefaktorového ověřování. Toto pole ponechte prázdné, pokud jsou pouze plánování toodownload hello Server Azure Multi-Factor Authentication nebo SDK.
      * Hello zprostředkovatel vícefaktorového ověřování musí být přidružen výhoda tootake adresář Azure AD hello pokročilé funkce.
      * Ke každému adresáři Azure AD může být přidružený pouze jeden poskytovatel Multi-Factor Auth.  
      ![Vytvoření poskytovatele MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider5.png)

8. Po kliknutí na tlačítko vytvořit, hello se vytvoří zprostředkovatel vícefaktorového ověřování a by měla zobrazit zpráva: **byl úspěšně vytvořen zprostředkovatel vícefaktorového ověřování**. Klikněte na tlačítko **OK**.  
   
   ![Vytvoření poskytovatele MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider6.png)  

## <a name="manage-your-multi-factor-auth-provider"></a>Správa poskytovatele Multi-Factor Auth

Nelze změnit hello použití modelu (za povoleného uživatele nebo za ověření), po vytvoření poskytovatele MFA. Můžete však odstranit hello zprostředkovatele vícefaktorového ověřování a pak vytvořte různé využití modelu.

Pokud aktuální zprostředkovatel vícefaktorového ověřování je hello přidružen adresář služby Azure AD (také označované jako klient služby Azure AD), můžete bezpečně odstranit hello zprostředkovatele vícefaktorového ověřování a vytvoření jednoho klienta propojené toohello stejnou službou Azure AD. Případně pokud jste zakoupili dostatek MFA, Azure AD Premium nebo Enterprise Mobility + Security (EMS) licence toocover všechny uživatele, kteří jsou povolené pro MFA, můžete odstranit zprostředkovatele vícefaktorového ověřování hello úplně.

Pokud váš poskytovatel MFA není klienta tooan propojené služby Azure AD, nebo odkaz hello nového MFA zprostředkovatele tooa jiné službě Azure AD klienta, se nepřenesou uživatelská nastavení a možnosti konfigurace. Navíc existující servery Azure MFA potřebovat toobe znovu aktivovat pomocí přihlašovacích údajů pro aktivaci vygenerované pomocí hello nového poskytovatele MFA. Opětovná aktivace toolink MFA servery hello je toohello nového poskytovatele MFA neovlivní telefonního hovoru a textové zprávy ověřování, ale oznámení zastaví pro všechny uživatele, dokud se znovu aktivovat mobilní aplikaci hello mobilní aplikace.

## <a name="next-steps"></a>Další kroky

[Stáhnout hello SDK služby Multi-Factor Authentication](multi-factor-authentication-sdk.md)

[Konfigurace nastavení služby Multi-Factor Authentication](multi-factor-authentication-whats-next.md)
