---
title: "Jak používat hesla aplikací v Azure MFA? | Dokumentace Microsoftu"
description: "Tato stránka vám pomůže uživatelům pomoct pochopit, jaké jsou hesla aplikací a jaké se používají s ohledem na Azure MFA."
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
ms.openlocfilehash: 1ecc2bdef5ff7ef8ed8dded7dc12428ce9657821
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="what-are-app-passwords-in-azure-multi-factor-authentication"></a>Co jsou hesla aplikací v Azure Multi-Factor Authentication?
Některé neprohlížečové aplikace, jako je například klienta Apple nativní e-mailu, který používá protokolu Exchange Active Sync, aktuálně nepodporují službu Multi-Factor authentication. Služba Multi-Factor authentication je povoleno podle uživatelů.  To znamená, že uživatel nemůže používání služby Multi-Factor authentication, pokud:

- Uživatel povolen pro službu Multi-Factor authentication
- Uživatel se pokouší použít neprohlížečové aplikace.

Heslo aplikace umožňuje uživatelům používat aplikace.

Až budete mít heslo aplikace, použijte místo původní heslo těchto neprohlížečových aplikací. Při registraci pro dvoustupňové ověření, že informuje Microsoft nechcete libovolný uživatel přihlásit pomocí hesla, pokud nemohou také provádět druhé ověření. Klient pro nativní e-mailové Apple na váš telefon nemůžete se přihlásit jako je vzhledem k tomu, že nemůžete žádat pro dvoustupňové ověření. Řešení tohoto problému je vytvoření každodenní bezpečnější heslo aplikace, které nepoužíváte. Hesla aplikací jsou pouze pro tyto aplikace, které nemohou podporovat dvoustupňové ověřování. Používejte heslo aplikace, takže aplikace se můžou obejít ověřování Multi-Factor authentication a pokračovat v práci.


> [!NOTE]
> Klienti Office 2013 (včetně Outlook) podporují nové protokoly pro ověřování a lze použít s dvoustupňové ověřování. Hesla aplikací nejsou potřeba použít s klienty Office 2013.  Další informace najdete v tématu [Office 2013 veřejné Preview verze moderního ověřování oznámeno](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).


## <a name="how-to-use-app-passwords"></a>Postupy pro používání hesel aplikací
Zde jsou některé věci vědět o heslech aplikací:

* Nevytvářejte vlastní hesla aplikací. Jsou automaticky generovány.
* Aktuálně je omezení 40 hesel na uživatele. 
* Pokud se pokusíte po dosažení limitu vytvořit heslo aplikace, budete muset před vytvořením nové odstraňte jednu z vašich dosavadních hesel aplikací.
* Používejte jedno heslo aplikace na zařízení, nikoliv podle aplikací. Můžete například vytvořit jedno heslo aplikace pro přenosný počítač a používat takové heslo aplikace pro všechny aplikace v tomto přenosném počítači. Pak vytvořte druhý heslo aplikace se používat pro všechny aplikace na ploše. 
* Jedno heslo aplikace jsou uvedeny poprvé, můžete zaregistrovat pro dvoustupňové ověření.  Pokud potřebujete další těch, můžete je vytvořit.



## <a name="creating-and-deleting-app-passwords"></a>Vytvoření nebo odstranění hesla aplikace
Během vaše počáteční přihlášení budete mít heslo aplikace, kterou můžete použít.  Můžete také vytvořit a později odstranit hesla aplikací. Jak odstranit hesla aplikací závisí na tom, jak používat ověřování Multi-Factor authentication. Odpovězte si na tyto otázky k určení, kde by měli přejít ke správě hesel aplikací: 

1. Používáte dvoustupňové ověřování pro svůj osobní účet Microsoft? Pokud ano, se seznamte s [hesla aplikací a dvoustupňové ověření](https://support.microsoft.com/help/12409/microsoft-account-app-passwords-two-step-verification) článek nápovědy. Pokud ne, přejděte k otázka, na dvě.

2. OK, abyste používat dvoustupňové ověřování pro svůj pracovní nebo školní účet. Používáte ji k přihlášení do aplikací Office 365? Pokud ano, se seznamte s [vytvořit heslo aplikace pro Office 365](https://support.office.com/article/Create-an-app-password-for-Office-365-3e7c860f-bda4-4441-a618-b53953ee1183) nápovědu. Pokud ne, přejděte k otázku tři. 

3. Používáte dvoustupňové ověření pomocí Microsoft Azure? Pokud ano, pokračovat [spravovat hesla aplikací na portálu Azure](#manage-app-passwords-in-the-Azure-portal) tohoto článku. Pokud ne, přejděte k otázka čtyři.

4. Nejste si jisti, kde používáte dvoustupňové ověřování? Pokračujte [spravovat hesla aplikací s portálem MyApps](#manage-app-passwords-with-the-myapps-portal) tohoto článku. 


## <a name="manage-app-passwords-in-the-azure-portal"></a>Správa hesel aplikací na portálu Azure
Pokud používáte dvoustupňové ověřování s Azure, budete chtít vytvořit hesla aplikací prostřednictvím portálu Azure.

### <a name="to-create-app-passwords-in-the-azure-portal"></a>Chcete-li vytvořit hesla aplikací na portálu Azure
1. Přihlaste se k portálu Azure Classic.
2. V horní části klikněte pravým tlačítkem na své uživatelské jméno a vyberte další ověření zabezpečení.
3. Na stránce ověření nahoře vyberte možnost hesla aplikací
4. Klikněte na možnost **Vytvořit**.
5. Zadejte název hesla aplikace a klikněte na tlačítko **další**
6. Kopírovat heslo aplikace do schránky a vložte jej do vaší aplikace.
   
   ![Cloud](./media/multi-factor-authentication-end-user-app-passwords/app2.png)


### <a name="to-delete-app-passwords-in-the-azure-portal"></a>Chcete-li odstranit hesla aplikací na portálu Azure
1. Přihlaste se k portálu Azure Classic.
2. V horní části klikněte pravým tlačítkem na své uživatelské jméno a vyberte další ověření zabezpečení.
3. V horní části vedle dalšího ověření zabezpečení, vyberte **hesla aplikací.**
4. Vedle heslo aplikace, které chcete odstranit, vyberte **odstranit**.
5. Potvrďte odstranění kliknutím **Ano**.
6. Po odstranění hesla aplikace, můžete kliknout na **zavřete**.


## <a name="manage-app-passwords-with-the-myapps-portal"></a>Spravujte hesla aplikací s portálem MyApps.
Pokud si nejste jisti, jak používat ověřování Multi-Factor authentication, pak můžete vždy vytvořit a odstranit hesla aplikací prostřednictvím portálu myapps.

### <a name="to-create-an-app-password-using-the-myapps-portal"></a>Chcete-li vytvořit heslo aplikace pomocí portálu Myapps
1. Přihlaste se k [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Klikněte na název v pravé horní a zvolte **profil**.
3. Vyberte **dalšího ověření zabezpečení**.
   ![Vyberte další bezpečnostní ověření – snímek obrazovky](./media/multi-factor-authentication-end-user-manage/myapps1.png)

4. Vyberte **hesla aplikací**.
   ![Vyberte hesel aplikací – snímek obrazovky](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)

5. Klikněte na možnost **Vytvořit**.
6. Zadejte název hesla aplikace a klikněte na tlačítko **Další**.
7. Kopírovat heslo aplikace do schránky a vložte jej do vaší aplikace.
   ![Vytvořit heslo aplikace](./media/multi-factor-authentication-end-user-app-passwords/create2.png)

### <a name="to-delete-an-app-password-using-the-myapps-portal"></a>Chcete-li odstranit heslo aplikace pomocí portálu Myapps
1. Přihlaste se k [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. V horní části vyberte profil.
3. Vyberte **dalšího ověření zabezpečení**.

   ![Vyberte další bezpečnostní ověření – snímek obrazovky](./media/multi-factor-authentication-end-user-manage/myapps1.png)

4. Vyberte **hesla aplikací**.

   ![Vyberte hesel aplikací – snímek obrazovky](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)

5. Vedle heslo aplikace, které chcete odstranit, klikněte na tlačítko **odstranit**.

   ![Odstranit heslo aplikace](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)

6. Potvrďte, že chcete odstranit toto heslo kliknutím **Ano**.
7. Po odstranění hesla aplikace, můžete kliknout na **zavřete**.

## <a name="next-steps"></a>Další kroky

- [Spravovat nastavení dvoustupňového ověřování](multi-factor-authentication-end-user-manage-settings.md)

- Vyzkoušení [aplikaci Microsoft Authenticator](microsoft-authenticator-app-how-to.md) ověření vašeho přihlášení s oznámeními aplikace místo přijetí texty nebo volání. 
