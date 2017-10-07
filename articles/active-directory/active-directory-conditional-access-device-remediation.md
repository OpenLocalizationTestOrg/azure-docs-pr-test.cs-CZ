---
title: "aaaYou nelze získat existuje zde na hello portálu Azure ze zařízení se systémem Windows | Microsoft Docs"
description: "Další informace, které nelze get existuje zde pochází z a co můžete zkontrolovat tooavoid běžící do toto dialogové okno."
services: active-directory
keywords: "přístup podmíněný zařízením, registrace zařízení, povolení registrace zařízení, registrace zařízení a MDM"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 8ad0156c-0812-4855-8563-6fbff6194174
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: eda2aa10fbff5b3e515723219f61c1cb6f634e29
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="you-cant-get-there-from-here-on-a-windows-device"></a>Odsud se tam nelze dostat pro zařízení s Windows

Při pokusu například o přístup k intranetu Sharepointu Online ve vaší organizaci můžete narazit na stránku, která uvádí, že *odsud se tam nelze dostat*. Tato stránka se zobrazuje, protože správce nakonfiguroval zásady podmíněného přístupu, která zabraňuje prostředkům organizace tooyour přístup za určitých podmínek. Může být nutné toocontact helpdesk nebo váš správce tooget tento problém vyřešit, existuje několik věcí, které můžete vyzkoušet sami, nejprve.

Pokud používáte **Windows** zařízení, měli byste zkontrolovat hello následující:

- Používáte podporovaný prohlížeč?

- Používáte ve vašem zařízení podporovanou verzi systému Windows?

- Odpovídá vaše zařízení požadavkům?






## <a name="supported-browser"></a>Podporovaný prohlížeč

Pokud váš správce nakonfiguroval zásady podmíněného přístupu, můžete pro přístup k prostředkům vaší organizace používat jenom podporovaný prohlížeč. V zařízeních se systémem Windows se podporují jenom prohlížeče **Internet Explorer** a **Edge**.

Můžete snadno identifikovat, jestli nelze přistupovat k prostředku z důvodu tooan nepodporovaný prohlížeč tak, že vyhledá v oddílu Podrobnosti hello chybovou stránku hello:

![Zpráva „Odsud se tam nelze dostat“ pro nepodporované prohlížeče](./media/active-directory-conditional-access-device-remediation/02.png "Scénář")

pouze nápravy Hello je toouse prohlížeč, podporující hello aplikace pro platformu zařízení. Úplný seznam podporovaných prohlížečů najdete v části [Podporované prohlížeče](active-directory-conditional-access-supported-apps.md#supported-browsers-for-device-based-policies).  


## <a name="supported-versions-of-windows"></a>Podporované verze Windows

musí být true o operační systém Windows hello na vašem zařízení Hello následující: 

- Pokud používáte operační systém pro stolní Windows na vašem zařízení, musí toobe Windows 7 nebo novější.
- Pokud používáte operační systém Windows server na vašem zařízení, je nutné toobe Windows Server 2008 R2 nebo novější. 


## <a name="compliant-device"></a>Odpovídající zařízení

Správce může nakonfigurovat zásady podmíněného přístupu, která umožňuje prostředkům organizace tooyour přístup jenom z kompatibilních zařízení. toobe předpisy, že zařízení musí být buď připojený k tooyour místní služby Active Directory nebo připojený k tooyour Azure Active Directory.

Můžete snadno identifikovat, jestli nelze přistupovat k prostředku z důvodu tooa zařízení, který není kompatibilní s prohlížením hello v části Podrobnosti o chybě stránky hello:
 
![Zprávy „Odsud se tam nelze dostat“ pro neregistrovaná zařízení](./media/active-directory-conditional-access-device-remediation/01.png "Scénář")


### <a name="is-your-device-joined-tooan-on-premises-active-directory"></a>Je vaše zařízení připojených k tooan místní služby Active Directory?

**Pokud se vaše zařízení připojí tooan místní služby Active Directory ve vaší organizaci:**

1. Ujistěte se, přihlásit tooWindows pomocí svého pracovního účtu (účtu služby Active Directory).
2. Připojte tooyour podnikové síti přes virtuální privátní sítě (VPN) nebo technologie DirectAccess.
3. Až se připojíte, stiskněte klávesu s logem Windows hello + klíče toolock hello L relace systému Windows.
4. Odemkněte relaci Windows zadáním přihlašovacích údajů ke svému pracovnímu účtu.
5. Počkejte několik minut a akci opakujte tooaccess hello aplikaci nebo službě.
6. Pokud se zobrazí hello stejné klikněte na tlačítko hello **podrobnosti** propojit a obraťte se na správce s podrobnostmi hello.


### <a name="is-your-device-not-joined-tooan-on-premises-active-directory"></a>Je vaše zařízení není připojené k tooan místní služby Active Directory?

Pokud zařízení není připojený tooan místní služby Active Directory a systémem Windows 10, máte dvě možnosti:

* Spusťte službu Azure AD Join
* Přidat váš pracovní nebo školní účet tooWindows

Informace o rozdílech mezi těmito dvěma možnostmi najdete v článku [Používání zařízení s Windows 10 na pracovišti](active-directory-azureadjoin-windows10-devices.md).  
Pokud zařízení:

- Patří tooyour organizace, byste měli spustit připojení ke službě Azure AD.
- Osobní zařízení nebo Windows phone, přidávejte váš pracovní nebo školní účet tooWindows 



#### <a name="azure-ad-join-on-windows-10"></a>Azure AD Join v systému Windows 10

Hello toojoin kroky jsou vaše zařízení tooAzure AD svázané hello verzi Windows 10 běží na něm. toodetermine hello verzi operačního systému Windows 10, spusťte hello **winver** příkaz: 

![Verze systému Windows](./media/active-directory-conditional-access-device-remediation/03.png )


**Windows 10 Anniversary Update (verze 1607):**

1. Otevřete hello **nastavení** aplikace.
2. Klikněte na **Účty**  >  **Přístup do práce nebo do školy**.
3. Klikněte na **Připojit**.
4. Klikněte na tlačítko **připojit toto zařízení tooAzure AD**.
5. Ověřování organizace tooyour, poskytovat služby Multi-Factor authentication, pokud se zobrazí výzva a potom postupujte podle kroků hello, které jsou zobrazeny.
6. Odhlaste se a znovu se přihlaste pomocí svého pracovního účtu.
7. Zkuste to znovu tooaccess hello aplikace.

**Windows 10 November 2015 Update (verze 1511):**

1. Otevřete hello **nastavení** aplikace.
2. Klikněte na **Systém**  >  **O systému**.
3. Klikněte na **Připojit se k Azure AD**.
4. Ověřování organizace tooyour, poskytovat služby Multi-Factor authentication, pokud se zobrazí výzva a potom postupujte podle kroků hello, které jsou zobrazeny.
5. Odhlaste se a znovu se přihlaste pomocí svého pracovního účtu (účtu Azure AD).
6. Zkuste to znovu tooaccess hello aplikace.


#### <a name="workplace-join-on-windows-81"></a>Připojení k pracovišti ve Windows 8.1

Pokud zařízení není připojené k doméně a běží Windows 8.1, toodo k síti na pracovišti a zaregistrovat v Microsoft Intune, hello následující kroky:

1. Otevřete **Nastavení počítače**.
2. Klikněte na **Síť**  >  **Pracoviště**.
3. Klikněte na **Připojit**.
4. Ověřování organizace tooyour, poskytovat služby Multi-Factor authentication, pokud se zobrazí výzva a potom postupujte podle kroků hello, které jsou zobrazeny.
5. Klikněte na **Zapnout**.
6. Zkuste to znovu tooaccess hello aplikace.



#### <a name="add-your-work-or-school-account-toowindows"></a>Přidat váš pracovní nebo školní účet tooWindows 


**Windows 10 Anniversary Update (verze 1607):**

1. Otevřete hello **nastavení** aplikace.
2. Klikněte na **Účty**  >  **Přístup do práce nebo do školy**.
3. Klikněte na **Připojit**.
4. Ověřování organizace tooyour, poskytovat služby Multi-Factor authentication, pokud se zobrazí výzva a potom postupujte podle kroků hello, které jsou zobrazeny.
5. Zkuste to znovu tooaccess hello aplikace.


**Windows 10 November 2015 Update (verze 1511):**

1. Otevřete hello **nastavení** aplikace.
2. Klikněte na **Účty**  >  **Vaše účty**.
3. Klikněte na **Přidat pracovní nebo školní účet**.
4. Ověřování organizace tooyour, poskytovat služby Multi-Factor authentication, pokud se zobrazí výzva a potom postupujte podle kroků hello, které jsou zobrazeny.
5. Zkuste to znovu tooaccess hello aplikace.





## <a name="next-steps"></a>Další kroky
[Podmíněný přístup ke službě Azure Active Directory](active-directory-conditional-access.md)

