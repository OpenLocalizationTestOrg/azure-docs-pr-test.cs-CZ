---
title: Active Directory Identity Protection playbook aaaAzure | Microsoft Docs
description: "Zjistěte, jak Azure AD Identity Protection vám umožní toolimit hello schopnost útočník tooexploit ohroženými identity nebo zařízení a toosecure identity nebo zařízení, která byla dříve toobe podezření nebo známých ohrožení zabezpečení."
services: active-directory
keywords: "ochrany identit Azure active directory, cloud app discovery,. Správa aplikací, zabezpečení, rizik, úroveň rizika, ohrožení zabezpečení, zásady zabezpečení"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 60836abf-f0e9-459d-b344-8e06b8341d25
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 6252bc133fc0c0f84800ee245a04bbf62d4cd25b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-identity-protection-playbook"></a>Azure seznam strategií ochrany identit Active Directory
Tato playbook vám pomůže:

* Naplnění dat v prostředí Identity Protection hello simulace rizikových událostech a ohrožení zabezpečení
* Nastavit zásady podmíněného přístupu na základě riziko a testování hello vliv těchto zásad

## <a name="simulating-risk-events"></a>Simulaci rizikových událostí
Tato část vám poskytne kroky pro simulaci hello následující typy událostí rizika:

* Přihlášení z anonymních IP adres (snadno)
* Přihlášení z neznámých míst (střední)
* Neuskutečnitelná cesta tooatypical umístění (obtížné)

Další události riziko nelze simulated zabezpečeným způsobem.

### <a name="sign-ins-from-anonymous-ip-addresses"></a>Přihlášení z anonymních IP adres
Tento typ události riziko identifikuje uživatele, kteří mají úspěšném přihlášení z IP adresy, která byla zjištěna jako IP adresa anonymního proxy serveru. Tyto servery proxy jsou uživatelé, kteří mají toohide používá IP adresu své zařízení a mohou být použity pro zlými úmysly.

**toosimulate přihlášení z anonymních IP adresy, proveďte následující kroky hello**:

1. Stáhnout hello [Tor prohlížeče](https://www.torproject.org/projects/torbrowser.html.en).
2. Pomocí hello Tor prohlížeče, přejděte příliš[https://myapps.microsoft.com](https://myapps.microsoft.com).   
3. Zadejte přihlašovací údaje hello hello účtu má tooappear v hello **přihlášení z anonymních IP adres** sestavy.

přihlášení Hello se zobrazí na řídicím panelu Identity Protection hello během pěti minut. 

### <a name="sign-ins-from-unfamiliar-locations"></a>Přihlášení z neznámých míst
Hello neznámých míst riziko je v reálném čase vyhodnocení přihlášení mechanismus, který zvažuje po přihlášení umístění (IP, zeměpisné šířky nebo zeměpisnou délku a ASN) toodetermine nové / neznámého umístění. systém Hello ukládá předchozí IP adresy, zeměpisné šířky nebo zeměpisnou délku a čísla ASN uživatele a zvažuje těchto toobe známé umístění. Umístění přihlášení je považován za obeznámeni, pokud hello přihlášení umístění neodpovídá žádnému z existující známé umístění hello.

Ochrany identit Azure Active Directory:  

* má po počáteční learning dobu 14 dnů, za které není příznak jiných umístění jako neznámých míst.
* ignoruje přihlášení ze známé zařízení a umístění, které jsou geograficky zavřít tooan existující známé umístění.

toosimulate neznámých míst, máte v umístění a zařízení, která hello účet nebyl přihlášení z před toosign. 

**toosimulate přihlášení z neznámého umístění, proveďte následující kroky hello**:

1. Vyberte účet, který má aspoň 14 dnů přihlášení historie. 
2. Proveďte:
   
   a. Při použití sítě VPN, přejděte příliš[https://myapps.microsoft.com](https://myapps.microsoft.com) a zadejte přihlašovací údaje hello hello účtu má toosimulate hello riziko událost pro.
   
   b. Požádejte přidružení v jiné umístění toosign pomocí pověření účtu hello (nedoporučuje se).

přihlášení Hello se zobrazí na řídicím panelu Identity Protection hello během pěti minut.

### <a name="impossible-travel-tooatypical-location"></a>Neuskutečnitelná cesta tooatypical umístění
Simulaci hello neuskutečnitelná cesta podmínka je složité, protože algoritmus hello používá machine learning tooweed na hodnotu false pozitivních třeba neuskutečnitelná cesta ze známé zařízení, nebo přihlášení z sítě VPN, které jsou používány jiných uživatelů v adresáři hello. Kromě toho hello algoritmus vyžaduje 3 dny too14 historii přihlášení pro uživatele hello před zahájením generování rizikových událostech.

**toosimulate tooatypical na neuskutečnitelná cesta umístění provést následující kroky hello**:

1. Pomocí standardní prohlížeči přejděte příliš[https://myapps.microsoft.com](https://myapps.microsoft.com).  
2. Zadejte přihlašovací údaje hello hello účet, který chcete toogenerate událost riziko neuskutečnitelná cesta pro.
3. Změna uživatelského agenta. Můžete změnit uživatelský agent v aplikaci Internet Explorer z nástrojů pro vývojáře, nebo změnit váš uživatelský agent v Firefox nebo Chrome pomocí doplňku přepínači uživatelského agenta.
4. Změníte IP adresu. Můžete změnit IP adresu pomocí sítě VPN, rozšíření Tor nebo otáčí nový počítač v Azure v různých datových center.
5. Přihlaste se příliš[https://myapps.microsoft.com](https://myapps.microsoft.com) pomocí stejných přihlašovacích údajů jako před a během několika minut po hello předchozí přihlášení hello.

přihlášení Hello se zobrazí v řídicím panelu hello ochrany identit v rámci 2 – 4 hodiny.<br>
Z důvodu hello komplexní modelů strojového učení související se situací je možné, že se ho nebude získat převzata.<br> Můžete chtít tooreplicate tyto kroky pro více účtů Azure AD.

## <a name="simulating-vulnerabilities"></a>Simulaci ohrožení zabezpečení
Ohrožení zabezpečení jsou slabá místa v prostředí Azure AD, které může zneužít objektu actor chybný. Aktuálně jsou prezentované 3 typy ohrožení zabezpečení v Azure AD Identity Protection, které využít další funkce služby Azure AD. Tyto chyby zabezpečení se zobrazí na řídicím panelu Identity Protection hello automaticky po tyto funkce jsou nastavené.

* Azure AD [služby Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)
* Azure AD [Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).
* Azure AD [Privileged Identity Management](active-directory-privileged-identity-management-configure.md). 

## <a name="user-compromise-risk"></a>Riziko ohrožení zabezpečení pro uživatele
**tootest riziko ohrožení zabezpečení pro uživatele, proveďte následující kroky hello**:

1. Přihlaste se příliš[https://portal.azure.com](https://portal.azure.com) pomocí přihlašovacích údajů globálního správce pro vašeho klienta.
2. Přejděte příliš**Identity Protection**. 
3. Na hlavní hello **Azure AD Identity Protection** okně klikněte na tlačítko **nastavení**. 
4. Na hello **nastavení portálu** okno, v části **pravidla zabezpečení**, klikněte na tlačítko **riziko ohrožení zabezpečení uživatele**. 
5. Na hello **přihlášení riziko** okně zapnout **Povolit pravidlo** vypnout a potom klikněte na **Uložit** nastavení.
6. Pro daný uživatelský účet, simulovat neznámého umístění nebo anonymní IP riziko událost. To bude zvýšit úroveň rizika hello uživatele pro daného uživatele příliš**střední**.
7. Počkejte několik minut a ověřte, že úrovni uživatele pro vaše uživatele je **střední**.
8. Přejděte toohello **nastavení portálu** okno.
9. Na hello **riziko ohrožení zabezpečení uživatele** okno, v části **Povolit pravidlo**, vyberte **na** . 
10. Vyberte jednu z hello následující možnosti:
    
    a. tooblock, vyberte **střední** pod **bloku přihlásit**.
    
    b. Změna tooenforce zabezpečeného hesla, vyberte **střední** pod **vyžadovat vícefaktorové ověřování**.
11. Klikněte na **Uložit**.
12. Nyní můžete otestovat podmíněného přístupu na základě riziko podle přihlášení pomocí uživatel s oprávněním vyšší úrovně rizika úrovní. Pokud uživatel riziko hello střední, v závislosti na konfiguraci hello zásad, vaše přihlášení je buď zablokuje nebo je vynucen toochange heslo. 
    <br><br>
    ![Playbook](./media/active-directory-identityprotection-playbook/201.png "Playbook")
    <br>

## <a name="sign-in-risk"></a>Riziko přihlášení
**tootest přihlášení riziko, proveďte následující kroky hello:**

1. Přihlaste se příliš[https://portal.azure.com ](https://portal.azure.com) pomocí přihlašovacích údajů globálního správce pro vašeho klienta.
2. Přejděte příliš**Identity Protection**.
3. Na hlavní hello **Azure AD Identity Protection** okně klikněte na tlačítko **nastavení**. 
4. Na hello **nastavení portálu** okno, v části **pravidla zabezpečení**, klikněte na tlačítko **přihlášení riziko**.
5. Na hello ** přihlásit riziko ** vyberte **na** pod **Povolit pravidlo**. 
6. Vyberte jednu z hello následující možnosti:
   
   a. tooblock, vyberte **střední** pod **bloku přihlášení**
   
   b. Změna tooenforce zabezpečeného hesla, vyberte **střední** pod **vyžadovat vícefaktorové ověřování**.
7. tooblock, vyberte střední v rámci bloku přihlášení.
8. tooenforce služby Multi-Factor authentication, vyberte **střední** pod **vyžadovat vícefaktorové ověřování**.
9. Klikněte na **Uložit**.
10. Nyní můžete otestovat podmíněného přístupu na základě riziko tak, že simuluje neznámých míst hello nebo anonymní IP riziko události, protože jsou oba **střední** rizik události.


![Playbook](./media/active-directory-identityprotection-playbook/200.png "Playbook")


## <a name="see-also"></a>Viz také
* [Ochrany identit Azure Active Directory](active-directory-identityprotection.md)

