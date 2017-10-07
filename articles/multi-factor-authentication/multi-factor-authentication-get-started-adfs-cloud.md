---
title: "aaaSecure cloudových prostředků s Azure MFA a AD FS | Microsoft Docs"
description: "Toto je stránka Azure Multi-Factor authentication hello, který popisuje, jak tooget pracovat s Azure MFA a AD FS v cloudu hello."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 0927fc67-8090-4fdd-913a-b3cfed3fbe77
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/29/2017
ms.author: kgremban
ms.openlocfilehash: 8d38d6a4af63ddcaf0fefded0b73d82d5178aa36
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="securing-cloud-resources-with-azure-multi-factor-authentication-and-ad-fs"></a>Zabezpečení cloudových prostředků s Azure Multi-Factor Authentication a AD FS
Pokud je vaše organizace Federovaná pomocí Azure Active Directory, použijte ověřování Azure Multi-Factor Authentication nebo Active Directory Federation Services (AD FS) toosecure prostředky, které jsou dostupné přes Azure AD. Použijte následující postupy toosecure prostředků Azure Active Directory s Azure Multi-Factor Authentication nebo Active Directory Federation Services hello.

## <a name="secure-azure-ad-resources-using-ad-fs"></a>Zabezpečení prostředků Azure AD pomocí služby AD FS
toosecure vaše prostředek v cloudu, nastavit pravidlo deklarací identity, aby Active Directory Federation Services vysílá hello multipleauthn deklarací, když uživatel provede dvoustupňové ověření úspěšně. Tento požadavek je předán na tooAzure AD. Použijte tento postup toowalk prostřednictvím hello kroky:


1. Otevřete správu služby AD FS.
2. Na levé straně hello vyberte **vztahy důvěryhodnosti předávající strany**.
3. Klikněte pravým tlačítkem na **Platforma identit Microsoft Office 365** a vyberte **Upravit pravidla deklarací identity**.

   ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)

4. V pravidlech transformace vystavení klikněte na **Přidat pravidlo**.

   ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)

5. V průvodci Přidat transformované deklarace identity pravidlo text hello, vyberte **předávat nebo filtrovat příchozí deklarace identity** z hello rozevíracího seznamu a klikněte na tlačítko **Další**.

   ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)

6. Pojmenujte pravidlo. 
7. Vyberte **odkazy na metody ověřování** jako hello příchozí typ deklarace identity.
8. Vyberte **Předávat všechny hodnoty deklarací identity**.
    ![	Průvodce přidáním pravidla – deklarace identity transformace](./media/multi-factor-authentication-get-started-adfs-cloud/configurewizard.png)
9. Klikněte na **Dokončit**. Zavřete konzolu hello AD FS.

## <a name="trusted-ips-for-federated-users"></a>Důvěryhodné IP adresy pro federované uživatele
Důvěryhodné IP adresy umožňují správcům tooby průchodu dvoustupňové ověřování pro konkrétní IP adresy, nebo pro federované uživatele, kteří mají požadavky pocházejících z vlastního intranetu. Hello následující části popisují, jak tooconfigure Azure Multi-Factor Authentication důvěryhodné IP adresy s federovanými uživateli a obejít dvoustupňové ověření, když požadavek pochází z uvnitř intranetu federovaného uživatele. Toho dosáhnete pomocí konfigurace služby AD FS toouse průchozí nebo filtru příchozí šablony deklarace identity s hello typ deklarace identity uvnitř podnikové sítě.

Tento příklad používá Office 365 pro naše trusty přijímající strany.

### <a name="configure-hello-ad-fs-claims-rules"></a>Konfigurace pravidel deklarací identity hello služby AD FS
První věc Hello potřebujeme toodo je tooconfigure hello služby AD FS deklarace identity. Vytvoříte dvě pravidla deklarace identity, jeden pro hello uvnitř podnikové sítě deklarace identity, typ a další pro zachování přihlášení uživatelů.

1. Otevřete správu služby AD FS.
2. Na levé straně hello vyberte **vztahy důvěryhodnosti předávající strany**.
3. Klikněte pravým tlačítkem na **Platforma identit Microsoft Office 365** a vyberte **Upravit pravidla deklarací identity…**
   ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)
4. V pravidlech transformace vystavení klikněte na **Přidat pravidlo.**
   ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)
5. V průvodci Přidat transformované deklarace identity pravidlo text hello, vyberte **předávat nebo filtrovat příchozí deklarace identity** z hello rozevíracího seznamu a klikněte na tlačítko **Další**.
   ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)
6. V hello pole Další tooClaim název pravidla pojmenujte pravidla. Příklad: InsideCorpNet.
7. Z rozevíracího seznamu hello, další tooIncoming typ deklarace identity, vyberte **uvnitř podnikové sítě**.
   ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip4.png)
8. Klikněte na **Finish** (Dokončit).
9. V pravidlech transformace vystavení klikněte na **Přidat pravidlo**.
10. V průvodci Přidat transformované deklarace identity pravidlo text hello, vyberte **odesílat deklarace pomocí vlastního pravidla** z hello rozevíracího seznamu a klikněte na tlačítko **Další**.
11. V poli hello pod položkou Název pravidla deklarace identity: Zadejte *zachovat přihlášené uživatele*.
12. V hello vlastní pravidlo zadejte:

        c:[Type == "http://schemas.microsoft.com/2014/03/psso"]
            => issue(claim = c);
    ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip5.png)
13. Klikněte na **Dokončit**.
14. Klikněte na tlačítko **Použít**.
15. Klikněte na tlačítko **OK**.
16. Zavřete správu služby AD FS.

### <a name="configure-azure-multi-factor-authentication-trusted-ips-with-federated-users"></a>Konfigurovat důvěryhodné IP adresy ověřování Azure Multi-Factor Authentication s federovanými uživateli
Teď, když hello deklarace identity jsou na místě, můžeme konfigurovat důvěryhodné IP adresy.

1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).
2. Na levé straně hello, klikněte na tlačítko **služby Active Directory**.
3. V adresáři vyberte hello adresář, kam chcete tooset až důvěryhodné IP adresy.
4. Na hello adresáře, které jste vybrali, klikněte na tlačítko **konfigurace**.
5. V části hello vícefaktorového ověřování, klikněte na tlačítko **spravovat nastavení služby**.
6. Na stránce nastavení služby hello v rámci důvěryhodných adres IP, vyberte **přeskočit více-factor-ověřování pro žádosti od federovaných uživatelů v mém intranetu**.  

   ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip6.png)
   
7. Klikněte na **Uložit**.
8. Jakmile byly použity hello aktualizace, klikněte na možnost **zavřete**.

A to je vše! V tomto okamžiku federovaní uživatelé služeb Office 365 by měl mít pouze toouse MFA při deklarace identity pochází z mimo hello podnikovém intranetu.
