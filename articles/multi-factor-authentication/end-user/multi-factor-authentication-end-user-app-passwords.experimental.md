---
title: "aaaHow toouse hesla aplikací v Azure MFA? | Dokumentace Microsoftu"
description: "Tato stránka vám pomůže pochopit, jaké jsou hesla aplikací a jaké jsou uživatelé použít pro s ohledem tooAzure vícefaktorového ověřování."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 345b757b-5a2b-48eb-953f-d363313be9e5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: kgremban
ms.custom: end-user
ms.openlocfilehash: 3afa2003d8e87576f035bf9440a1dba67bd85f5b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-are-app-passwords-in-azure-multi-factor-authentication"></a>Co jsou hesla aplikací v Azure Multi-Factor Authentication?
Některé neprohlížečové aplikace, jako je například hello Apple nativní e-mailové klienta, který používá protokolu Exchange Active Sync, aktuálně nepodporují službu Multi-Factor authentication. Služba Multi-Factor authentication je povoleno podle uživatelů.  To znamená, že uživatel nemůže používání služby Multi-Factor authentication, pokud:

- Hello uživatel povolen pro službu Multi-Factor authentication
- Hello uživatel se pokusil toouse neprohlížečovou aplikaci.

Heslo aplikace umožňuje hello uživatele toouse hello aplikace.

Až budete mít heslo aplikace, použijte místo původní heslo těchto neprohlížečových aplikací. Při registraci pro dvoustupňové ověření, jste informuje Microsoft není toolet každý, kdo přihlásit pomocí hesla Pokud nemůžou také provést hello druhé ověření. Hello Apple nativní e-mailového klienta na váš telefon nemůžete se přihlásit jako je vzhledem k tomu, že nemůžete žádat pro dvoustupňové ověření. Hello řešení toothis problém je toocreate bezpečnější heslo aplikace, které nepoužíváte každodenní. Hesla aplikací jsou pouze pro tyto aplikace, které nemohou podporovat dvoustupňové ověřování. Používejte heslo aplikace hello, takže můžete přeskočit službu Multi-Factor authentication a pokračovat toowork aplikace.


> [!NOTE]
> Klienti Office 2013 (včetně Outlook) podporují nové protokoly pro ověřování a lze použít s dvoustupňové ověřování. Hesla aplikací nejsou potřeba použít s klienty Office 2013.  Další informace najdete v tématu [Office 2013 veřejné Preview verze moderního ověřování oznámeno](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).


## <a name="how-toouse-app-passwords"></a>Jak toouse hesel aplikací
Zde jsou některé věci tooknow o heslech aplikací:

* Nevytvářejte vlastní hesla aplikací. Jsou automaticky generovány.
* Aktuálně je omezení 40 hesel na uživatele. 
* Pokud po dosažení limitu hello toocreate heslo aplikace, budete mít toodelete jeden z vašich dosavadních hesel aplikací předtím, než vytvoříte nový.
* Používejte jedno heslo aplikace na zařízení, nikoliv podle aplikací. Můžete například vytvořit jedno heslo aplikace pro přenosný počítač a používat takové heslo aplikace pro všechny aplikace v tomto přenosném počítači. Pak vytvořte druhý toouse heslo aplikace pro všechny aplikace na ploše. 
* Máte jeden hello heslo aplikace při prvním zaregistrujete pro dvoustupňové ověření.  Pokud potřebujete další těch, můžete je vytvořit.



## <a name="creating-and-deleting-app-passwords"></a>Vytvoření nebo odstranění hesla aplikace
Během vaše počáteční přihlášení budete mít heslo aplikace, kterou můžete použít.  Můžete také vytvořit a později odstranit hesla aplikací. Jak odstranit hesla aplikací závisí na tom, jak používat ověřování Multi-Factor authentication. Odpověď hello následující otázky toodetermine, kde by měli přejít toomanage hesla aplikací: 

1. Používáte dvoustupňové ověřování pro svůj osobní účet Microsoft? Pokud ano, by měla odkazovat toohello [hesla aplikací a dvoustupňové ověření](https://support.microsoft.com/help/12409/microsoft-account-app-passwords-two-step-verification) článek nápovědy. Pokud ne, pokračujte tooquestion dva.

2. OK, abyste používat dvoustupňové ověřování pro svůj pracovní nebo školní účet. Používáte ji toosign v aplikacích tooOffice 365? Pokud ano, se seznamte s příliš[vytvořit heslo aplikace pro Office 365](https://support.office.com/article/Create-an-app-password-for-Office-365-3e7c860f-bda4-4441-a618-b53953ee1183) nápovědu. Pokud ne, pokračujte tooquestion tři. 

3. Používáte dvoustupňové ověření pomocí Microsoft Azure? Pokud ano, pokračovat toohello [spravovat hesla aplikace v portálu Azure hello](#manage-app-passwords-in-the-Azure-portal) tohoto článku. Pokud ne, pokračujte tooquestion čtyři.

4. Nejste si jisti, kde používáte dvoustupňové ověřování? Pokračovat toohello [spravovat hesla aplikací s hello MyApps portál](#manage-app-passwords-with-the-myapps-portal) tohoto článku. 


## <a name="manage-app-passwords-in-hello-azure-portal"></a>Správa hesel aplikací v hello portálu Azure
Pokud používáte dvoustupňové ověřování s Azure, budete chtít toocreate hesla aplikací prostřednictvím hello portálu Azure.

### <a name="toocreate-app-passwords-in-hello-azure-portal"></a>hesla aplikací toocreate v hello portálu Azure
1. Přihlaste se toohello portál Azure classic.
2. V horní části hello klikněte pravým tlačítkem na své uživatelské jméno a vyberte další ověření zabezpečení.
3. Na stránce ověření hello hello horní vyberte hesel aplikací
4. Klikněte na možnost **Vytvořit**.
5. Zadejte název hesla aplikace hello a klikněte na tlačítko **další**
6. Hello aplikace heslo toohello schránky zkopírujte a vložte jej do vaší aplikace.
   
   ![Cloud](./media/multi-factor-authentication-end-user-app-passwords/app2.png)


### <a name="toodelete-app-passwords-in-hello-azure-portal"></a>hesla aplikací toodelete v hello portálu Azure
1. Přihlaste se toohello portál Azure classic.
2. V horní části hello klikněte pravým tlačítkem na své uživatelské jméno a vyberte další ověření zabezpečení.
3. V horní části hello, další ověření zabezpečení tooadditional, vyberte **hesla aplikací.**
4. Chcete toodelete, vyberte další heslo aplikace toohello **odstranit**.
5. Potvrdit odstranění hello kliknutím **Ano**.
6. Po odstranění hesla aplikace hello můžete kliknout na **zavřete**.


## <a name="manage-app-passwords-with-hello-myapps-portal"></a>Spravujte hesla aplikací s hello MyApps portálu.
Pokud si nejste jisti, jak používat ověřování Multi-Factor authentication, pak můžete vždy vytvořit a odstranit hesla aplikací prostřednictvím portálu myapps hello.

### <a name="toocreate-an-app-password-using-hello-myapps-portal"></a>toocreate heslo aplikace pomocí hello Myapps portálu
1. Přihlaste se příliš[https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Klikněte na název v pravé horní části hello a zvolte **profil**.
3. Vyberte **dalšího ověření zabezpečení**.
   ![Vyberte další bezpečnostní ověření – snímek obrazovky](./media/multi-factor-authentication-end-user-manage/myapps1.png)

4. Vyberte **hesla aplikací**.
   ![Vyberte hesel aplikací – snímek obrazovky](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)

5. Klikněte na možnost **Vytvořit**.
6. Zadejte název hesla aplikace hello a klikněte na tlačítko **Další**.
7. Hello aplikace heslo toohello schránky zkopírujte a vložte jej do vaší aplikace.
   ![Vytvořit heslo aplikace](./media/multi-factor-authentication-end-user-app-passwords/create2.png)

### <a name="toodelete-an-app-password-using-hello-myapps-portal"></a>toodelete heslo aplikace pomocí hello Myapps portálu
1. Přihlaste se příliš[https://myapps.microsoft.com](https://myapps.microsoft.com)
2. V horní části hello vyberte profil.
3. Vyberte **dalšího ověření zabezpečení**.

   ![Vyberte další bezpečnostní ověření – snímek obrazovky](./media/multi-factor-authentication-end-user-manage/myapps1.png)

4. Vyberte **hesla aplikací**.

   ![Vyberte hesel aplikací – snímek obrazovky](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)

5. Klikněte na tlačítko Další heslo aplikace toohello chcete toodelete, **odstranit**.

   ![Odstranit heslo aplikace](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)

6. Potvrďte, že chcete toodelete toto heslo kliknutím **Ano**.
7. Po odstranění hesla aplikace hello můžete kliknout na **zavřete**.

## <a name="next-steps"></a>Další kroky

- [Spravovat nastavení dvoustupňového ověřování](multi-factor-authentication-end-user-manage-settings.md)

- Vyzkoušet hello [aplikaci Microsoft Authenticator](microsoft-authenticator-app-how-to.md) tooverify vaše přihlášení s oznámeními aplikace místo přijetí texty nebo volání. 
