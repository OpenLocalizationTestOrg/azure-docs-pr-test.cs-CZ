---
title: "aaaConfigure zabezpečeného LDAP (LDAPS) v Azure AD Domain Services | Microsoft Docs"
description: "Konfigurace zabezpečení protokolu LDAP (LDAPS) pro spravované doméně služby Azure AD Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: c6da94b6-4328-4230-801a-4b646055d4d7
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: maheshu
ms.openlocfilehash: 99e44d917b115d7f7a67ff0a5e22c5c9165287e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Konfigurace zabezpečeného LDAP (LDAPS) pro spravované doméně služby Azure AD Domain Services
Tento článek ukazuje, jak můžete povolit zabezpečené Lightweight Directory přístup protokolu (LDAPS) vaší spravované domény služby Azure AD Domain Services. Zabezpečený LDAP se také označuje jako "přístup protokolu LDAP (Lightweight Directory) přes vrstvy SSL (Secure Sockets) nebo zabezpečení TLS (Transport Layer)'.

## <a name="before-you-begin"></a>Než začnete
úlohy hello tooperform uvedené v tomto článku, budete potřebovat:

1. Platná **předplatné**.
2. **Adresář Azure AD** – buď synchronizovány s místní adresář nebo výhradně cloudový adresář.
3. **Azure AD Domain Services** musí být povolen pro adresář hello Azure AD. Pokud jste tak dosud neučinili, postupujte podle kroků uvedených v hello všechny hello úlohy [příručce Začínáme](active-directory-ds-getting-started.md).
4. A **tooenable toobe používá certifikát zabezpečený LDAP**.

   * **Doporučená** – Získejte certifikát od důvěryhodné veřejné certifikační autority. Tato možnost konfigurace je bezpečnější.
   * Alternativně můžete také příliš[vytvořit certifikát podepsaný svým držitelem](#task-1---obtain-a-certificate-for-secure-ldap) jak uvidíte později v tomto článku.

<br>

### <a name="requirements-for-hello-secure-ldap-certificate"></a>Požadavky pro hello zabezpečený LDAP certifikát
Získejte platný certifikát na hello následující pokyny, dříve než povolíte zabezpečený LDAP. Pokud se pokusíte tooenable dojde k selhání zabezpečený LDAP vaší spravované domény s neplatný nebo nesprávný certifikát.

1. **Důvěryhodného vystavitele** -hello certifikát musí být vydaný autoritu důvěřují počítače připojující se toohello spravované doméně pomocí zabezpečený LDAP. Tato autorita může být veřejné certifikační autority tyto počítače důvěřují.
2. **Doba platnosti** -hello certifikát musí být platný pro alespoň hello další 3 až 6 měsíců. Zabezpečený LDAP tooyour spravované domény přístupu dojde k narušení když vyprší platnost certifikátu hello.
3. **Název subjektu** -hello název předmětu na certifikátu hello musí být zástupný znak vaší spravované domény. Například pokud název domény "contoso100.com", název subjektu certifikátu hello musí být: *. contoso100.com ". Nastavit hello DNS (alternativní název subjektu) toothis zástupný název.
4. **Použití klíče** - hello certifikát musí být nakonfigurovaný pro následující hello používá – digitální podpisy i šifrování klíče.
5. **Účel certifikátu** -hello certifikát musí být platný pro ověření serveru SSL.

> [!NOTE]
> **Podnikové certifikační autority:** Azure AD Domain Services nepodporuje použití zabezpečeného LDAP certifikátů vystavených certifikační autoritou rozlehlé sítě vaší organizace. Toto omezení se totiž hello služby nedůvěřuje certifikační Autority jako certifikát kořenové certifikační autority rozlehlé sítě. 
>
>

<br>

## <a name="task-1---obtain-a-certificate-for-secure-ldap"></a>Úloha 1 – získání certifikátu pro zabezpečený LDAP
první úlohou Hello zahrnuje získání certifikátu pro zabezpečený LDAP přístup toohello spravované domény. Máte dvě možnosti:

* Získejte certifikát od certifikační autority. autorita Hello může být veřejné certifikační autority.
* Vytvořte certifikát podepsaný svým držitelem.

### <a name="option-a-recommended---obtain-a-secure-ldap-certificate-from-a-certification-authority"></a>Možnost (doporučeno) - získat zabezpečený LDAP certifikát od certifikační autority
Pokud vaše organizace obdrží svoje certifikáty od veřejné certifikační autority, budete potřebovat tooobtain hello zabezpečený LDAP certifikát od této veřejné certifikační autority.

Při žádosti o certifikát, ujistěte se, že jste splňují všechny požadavky hello uvedených v [požadavky hello zabezpečený LDAP certifikát](#requirements-for-the-secure-ldap-certificate).

> [!NOTE]
> Klientské počítače, které je třeba tooconnect toohello spravované doméně pomocí zabezpečený LDAP musí důvěřovat hello vystavitele certifikátu zabezpečeného LDAP hello.
>
>

### <a name="option-b---create-a-self-signed-certificate-for-secure-ldap"></a>Možnost B - vytvořit certifikát podepsaný svým držitelem pro zabezpečený LDAP
Pokud neočekáváte toouse certifikát od veřejné certifikační autority, můžete se rozhodnout toocreate certifikát podepsaný svým držitelem pro zabezpečený LDAP.

**Vytvořit certifikát podepsaný svým držitelem pomocí prostředí PowerShell**

Na počítači s Windows, otevřete nové okno prostředí PowerShell jako **správce** a typ hello následující příkazy, toocreate nový certifikát podepsaný svým držitelem.

    $lifetime=Get-Date

    New-SelfSignedCertificate -Subject *.contoso100.com -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment -Type SSLServerAuthentication -DnsName *.contoso100.com

V předchozích ukázkových hello, nahraďte '*. contoso100.com "hello název domény DNS s vaší spravované domény. For example, pokud jste vytvořili spravované doméně nazvané 'contoso100.onmicrosoft.com', nahraďte '*. contoso100.com "v hello výše skriptu s ' *. contoso100.onmicrosoft.com').

![Vyberte adresář služby Azure AD](./media/active-directory-domain-services-admin-guide/secure-ldap-powershell-create-self-signed-cert.png)

Hello nově vytvořený certifikát podepsaný svým držitelem se umístí do úložiště certifikátů hello místního počítače.


## <a name="next-step"></a>Další krok
[Úloha 2 – export hello tooa certifikát zabezpečený LDAP. Soubor PFX](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md)
