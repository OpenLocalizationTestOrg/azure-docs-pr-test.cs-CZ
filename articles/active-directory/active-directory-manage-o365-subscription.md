---
title: "aaaManage hello adresáře pro předplatné služeb Office 365 ve službě Azure | Microsoft Docs"
description: "Správa adresáře pro předplatné služeb Office 365 pomocí Azure Active Directory a hello portál Azure classic"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 746987b7-2dfd-4b35-819d-37c1b65c5c81
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/25/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 4faf9936d7e94b85356a18adfcf3d2a48e5b025f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hello-directory-for-your-office-365-subscription-in-azure"></a>Spravovat hello adresáře pro předplatné služeb Office 365 ve službě Azure
Tento článek popisuje, jak toomanage adresáře, který byl vytvořen pro předplatné služeb Office 365 pomocí hello portál Azure classic. Musí být buď hello služby správce nebo spolusprávce předplatného Azure toosign v toohello portál Azure classic. Pokud ještě nemáte předplatné Azure, můžete si pomocí tohoto odkazu zaregistrovat [bezplatnou 30denní zkušební verzi](https://azure.microsoft.com/trial/get-started-active-directory/) ještě dnes a nasadit první cloudové řešení za méně než 5 minut. Být jisti toouse hello pracovní nebo školní účet, který používáte toosign v tooOffice 365.

> [!IMPORTANT]
> Společnost Microsoft doporučuje, která můžete spravovat Azure AD pomocí hello [centra pro správu Azure AD](https://aad.portal.azure.com) v hello hello portál Azure místo použití portálu Azure classic, kterou se odkazuje v tomto článku.

Po dokončení hello předplatného Azure se můžete přihlásit toohello portál Azure classic a přístupu ke službám Azure. Klikněte na rozšíření Active Directory hello toomanage hello stejný adresář, který ověřuje vaši uživatelé Office 365.

Pokud již máte předplatné Azure, je také snadný hello proces toomanage další adresář. Michael Smith má například předplatné služeb Office 365 pro doménu Contoso.com. Má také předplatné služby Azure, které si zaregistroval pomocí účtu Microsoft, msmith@hotmail.com. V tomto případě spravuje dva adresáře.

| Předplatné | Office 365 | Azure |
| --- | --- | --- |
|   Zobrazované jméno |Contoso |Výchozí adresář Azure Active Directory (Azure AD) |
|   Název domény |contoso.com |msmithhotmail.onmicrosoft.com |

Chce toomanage hello uživatelských identit v adresáři společnosti Contoso hello při tooAzure pomocí svého účtu Microsoft, je přihlášený tak, aby mohl povolit funkce Azure AD, jako je vícefaktorové ověřování. Následující diagram Hello mohou pomoci tooillustrate hello procesu.

![Diagram toomanage dvou nezávislých adresářů](./media/active-directory-manage-o365-subscription/AAD_O365_03.png)

V tomto případě jsou hello dva adresáře navzájem nezávislé.

## <a name="toomanage-two-independent-directories"></a>toomanage dvou nezávislých adresářů
V pořadí pro Michael Smith toomanage oba adresáře při je přihlášený tooAzure jako msmith@hotmail.com, musí dokončit hello následující kroky:

> [!NOTE]
> Kroky je možné dokončit, jen když je uživatel přihlášený pomocí účtu Microsoft. Pokud hello uživatel přihlášený pomocí pracovního nebo školního účtu, hello možnost příliš**použít existující adresář** není k dispozici. Pracovní nebo školní účet může být ověřen pouze pomocí jeho domovského adresáře (tedy hello adresáře, kde hello pracovní nebo školní účet uložen, a že hello firmy nebo ve škole vlastní).
>
>

1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com) jako msmith@hotmail.com.
2. Klikněte na tlačítko **Nový** > **App Services** > **Active Directory** > **Adresář** > **Vytvořit vlastní**.
3. Klikněte na možnost použít existující adresář a zaškrtněte možnost hello **jsem připravené toobe Odhlásit** zaškrtávací políčko.
4. Přihlaste se toohello portál Azure classic jako globální správce Contoso.onmicrosoft.com (například msmith@contoso.com).
5. Po zobrazení výzvy příliš**adresáře společnosti Contoso hello použití s Azure?**, klikněte na tlačítko **pokračovat**.
6. Klikněte na tlačítko **Odhlásit**.
7. Přihlaste se toohello portál Azure classic jako msmith@hotmail.com. hello adresáře společnosti Contoso a výchozí adresář hello se zobrazí v hello rozšíření Active Directory.

Po dokončení těchto kroků msmith@hotmail.com je globální správce adresáře společnosti Contoso hello.

## <a name="tooadminister-resources-as-hello-global-admin"></a>tooadminister prostředky jako hello globální správce.
Nyní předpokládejme, že se Jane Doe potřebuje spravovat weby a databáze prostředků, které jsou přidružené hello předplatné Azure pro msmith@hotmail.com. Předtím, než bude moct provést, musí Michael Smith toocomplete těchto dodatečných kroků:

1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com) pomocí účtu správce služby hello pro hello předplatné Azure (v tomto příkladu msmith@hotmail.com).
2. Přenos adresáře společnosti Contoso toohello hello předplatného: klikněte na tlačítko **nastavení** > **odběry** > Vybrat předplatné hello > **upravit adresář** > vyberte **Contoso (Contoso.com)**. Jako součást hello přenos žádný pracovní nebo školní účty, které jsou spolusprávci předplatného hello se odeberou.
3. Přidat Jane Doe jako spolusprávce předplatného hello: klikněte na tlačítko **nastavení** > **správci** > Vybrat předplatné hello > **přidat** > typ **JohnDoe@Contoso.com**.

## <a name="next-steps"></a>Další kroky
Další informace o hello vztah mezi předplatnými a adresáři najdete v tématu [jak předplatné je spojeno s adresářem](active-directory-how-subscriptions-associated-directory.md).
