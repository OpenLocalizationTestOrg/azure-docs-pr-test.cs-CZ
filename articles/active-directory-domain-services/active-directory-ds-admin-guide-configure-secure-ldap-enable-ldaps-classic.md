---
title: "Konfigurace zabezpečeného LDAP (LDAPS) ve službě Azure AD Domain Services | Microsoft Docs"
description: "Konfigurace zabezpečení protokolu LDAP (LDAPS) pro spravované doméně služby Azure AD Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 9d563c45-9578-410d-96c8-355af64feae8
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: maheshu
ms.openlocfilehash: 3aafe209aad7383cd0610d147b5fdba673023c93
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Konfigurace zabezpečeného LDAP (LDAPS) pro spravované doméně služby Azure AD Domain Services

## <a name="before-you-begin"></a>Než začnete
Ujistěte se, když jste dokončili [úloha 2 - zabezpečený LDAP certifikát, který chcete exportovat. Soubor PFX](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md).

Vyberte, zda chcete tuto úlohu dokončit pomocí práci s portálem Azure preview nebo portálu Azure classic.
> [!div class="op_single_selector"]
> * **Portál Azure (Preview)**: [povolit zabezpečený LDAP pomocí portálu Azure](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)
> * **Portál Azure classic**: [povolit zabezpečený LDAP pomocí portálu Azure classic](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps-classic.md)
>
>


## <a name="task-3---enable-secure-ldap-for-the-managed-domain-using-the-classic-azure-portal"></a>Úloha 3 – povolení zabezpečeného LDAP pro spravované doméně pomocí portálu Azure classic
Pokud chcete povolit zabezpečený LDAP, proveďte následující kroky konfigurace:

1. Přejděte na  **[portál Azure classic](https://manage.windowsazure.com)**.
2. V levém podokně vyberte uzel **Active Directory**.
3. Vyberte adresář Azure AD (také označovány jako "klienta"), pro kterou jste povolili službu Azure AD Domain Services.

    ![Vyberte adresář služby Azure AD](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. Klikněte na kartu **KONFIGUROVAT**.

    ![Karta konfigurace adresáře](./media/active-directory-domain-services-getting-started/configure-tab.png)
5. Přejděte dolů k části s názvem **služby domény**. Měli byste vidět možnost s názvem **zabezpečení protokolu LDAP (LDAPS)** jak je znázorněno na následujícím snímku obrazovky:

    ![Oddíl konfigurace doménových služeb](./media/active-directory-domain-services-admin-guide/secure-ldap-start.png)
6. Klikněte **konfigurovat certifikát...**  tlačítko zprovoznit **konfigurace certifikátu pro zabezpečené LDAP** dialogové okno.

    ![Konfigurace certifikátu pro zabezpečený LDAP](./media/active-directory-domain-services-admin-guide/secure-ldap-configure-cert-page.png)
7. Klikněte na ikonu následující složky **certifikát pomocí souboru PFX** k určení souboru PFX, který obsahuje certifikát, které chcete použít pro zabezpečený přístup protokolu LDAP k spravované doméně. Také zadejte heslo, které jste zadali při exportu certifikátu do souboru PFX. Potom klikněte na tlačítko Hotovo v dolní části.

    ![Zadejte soubor zabezpečený LDAP PFX a heslo](./media/active-directory-domain-services-admin-guide/secure-ldap-specify-pfx.png)
8. **Domény služby** části **konfigurace** by měl získat šedě a je v **čekající na vyřízení...**  stavu pro několik minut. Během této doby je LDAPS certifikát ověřit přesnost a zabezpečený LDAP je nakonfigurována pro vaší spravované domény.

    ![Zabezpečený LDAP - čekající na vyřízení stavu](./media/active-directory-domain-services-admin-guide/secure-ldap-pending-state.png)

   > [!NOTE]
   > Chcete-li povolit zabezpečený LDAP vaší spravované domény trvá asi 10 až 15 minut. Pokud k zadanému zabezpečený LDAP certifikátu neodpovídá požadované kritéria, zabezpečený LDAP není povoleno pro svůj adresář a zobrazit informace o selhání. Například je nesprávný název domény, certifikátu již vypršela nebo brzo vyprší.
   >
   >

9. Když vaší spravované domény úspěšně povolen zabezpečený LDAP **čekající na vyřízení...**  by zpráva zmizí. Měli byste vidět kryptografický otisk certifikátu, zobrazí.

    ![Zabezpečený LDAP povoleno](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled.png)

<br>

## <a name="task-4---enable-secure-ldap-access-over-the-internet"></a>Úloha 4 – povolení zabezpečeného přístupu LDAP přes internet
**Nepovinná úloha** – Pokud neplánujete přístup ke spravované doméně pomocí LDAPS přes internet, přeskočte tento úkol konfigurace.

Než začnete tuto úlohu, zkontrolujte jste dokončili podle kroků uvedených v [úloha 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-classic-azure-portal).

1. Měli byste vidět možnost **povolit ZABEZPEČENÉ LDAP přístup přes INTERNET** v **domény služby** části **konfigurace** stránky. Tato možnost nastavená na **ne** ve výchozím nastavení, protože je ve výchozím nastavení zakázán přístup k Internetu k spravované doméně přes zabezpečený LDAP.

    ![Zabezpečený LDAP povoleno](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled2.png)
2. Přepnutí **UMOŽNIT ZABEZPEČENÝ LDAP přístup přes INTERNET** k **Ano**. Klikněte **Uložit** tlačítko na dolním panelu.
    ![Zabezpečený LDAP povoleno](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access.png)
3. **Domény služby** části **konfigurace** by měl získat šedě a je v **čekající na vyřízení...**  stavu pro několik minut. Po určité době je povolen přístup k Internetu k spravované doméně přes zabezpečený LDAP.

    ![Zabezpečený LDAP - čekající na vyřízení stavu](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access-pending-state.png)

   > [!NOTE]
   > Povolit přístup k Internetu přes zabezpečený LDAP vaší spravované domény trvá přibližně 10 minut.
   >
   >
4. Při zapnutém úspěšně zabezpečený LDAP přístup k vaší spravované domény přes internet, **čekající na vyřízení...**  by zpráva zmizí. Měli byste vidět externí IP adresu, kterou lze použít pro přístup k adresáři přes LDAPS v poli **externí IP adresu pro LDAPS přístup**.

    ![Zabezpečený LDAP povoleno](./media/active-directory-domain-services-admin-guide/secure-ldap-internet-access-enabled.png)

<br>

## <a name="task-5---configure-dns-to-access-the-managed-domain-from-the-internet"></a>Úloha 5: Konfigurace DNS pro přístup k spravované doméně z Internetu
**Nepovinná úloha** – Pokud neplánujete přístup ke spravované doméně pomocí LDAPS přes internet, přeskočte tento úkol konfigurace.

Než začnete tuto úlohu, zkontrolujte jste dokončili podle kroků uvedených v [úloha 4](#task-4---enable-secure-ldap-access-over-the-internet).

Jakmile povolíte zabezpečený LDAP přístup přes internet vaší spravované domény, je potřeba aktualizovat DNS, aby klientské počítače najít této spravované domény. Na konci úloha 4 externí IP adresa se zobrazí na **konfigurace** kartě v **externí IP adresu pro LDAPS přístup**.

Nakonfigurujte externího poskytovatele DNS, aby název DNS spravované doméně (například "ldaps.contoso100.com') odkazuje na tato externí IP adresu. V našem příkladu je potřeba vytvořit následující položku DNS:

    ldaps.contoso100.com  -> 52.165.38.113

Je to – nyní jste připraveni k připojení k spravované doméně pomocí zabezpečený LDAP přes internet.

> [!WARNING]
> Mějte na paměti, že klientské počítače musí důvěřovat vystavitele certifikátu LDAPS moct úspěšně připojit k spravované doméně pomocí LDAPS. Pokud používáte certifikační autoritu organizace nebo veřejně důvěryhodné certifikační autority, není potřeba dělat nic, protože klientské počítače důvěřovat tyto vystavitelů certifikátů. Pokud používáte certifikát podepsaný svým držitelem, budete muset nainstalovat část veřejný certifikát podepsaný svým držitelem do úložiště důvěryhodných certifikátů v klientském počítači.
>
>


## <a name="lock-down-ldaps-access-to-your-managed-domain-over-the-internet"></a>Uzamčení LDAPS přístup k vaší spravované domény přes internet
> [!NOTE]
> **Nepovinná úloha** – Pokud jste nepovolili LDAPS přístup k spravované doméně přes internet, přeskočte tento úkol konfigurace.
>
>

Než začnete tuto úlohu, zkontrolujte jste dokončili podle kroků uvedených v [úloha 4](#task-4---enable-secure-ldap-access-over-the-internet).

Vystavení vaší spravované domény pro LDAPS přístup přes internet představuje bezpečnostní riziko. Spravované doméně je dosažitelný z Internetu na port používaný pro zabezpečený LDAP (to znamená, port 636). Proto můžete omezit přístup k spravované doméně na konkrétní známé IP adresy. Pro lepší zabezpečení vytvořte skupinu zabezpečení sítě (NSG) a přidružte ji k podsíti, kde jste povolili službu Azure AD Domain Services.

Následující tabulka znázorňuje ukázku NSG můžete nakonfigurovat, zamknout zabezpečený LDAP přístup přes internet. NSG obsahuje sadu pravidel, která povolí příchozí LDAPS přístup přes port TCP 636 pouze ze zadané sady IP adres. Výchozí pravidlo, DenyAll, platí pro všechny ostatní příchozí přenosy z Internetu. Pravidla NSG pro povolení LDAPS přístupu přes internet ze zadaných IP adres má vyšší prioritu než skupina NSG DenyAll pravidlo.

![Ukázka skupiny NSG k zabezpečení přístupu k LDAPS přes internet](./media/active-directory-domain-services-admin-guide/secure-ldap-sample-nsg.png)

**Další informace** - [skupin zabezpečení sítě](../virtual-network/virtual-networks-nsg.md).

<br>

## <a name="related-content"></a>Související obsah
* [Azure AD Domain Services – Příručka Začínáme](active-directory-ds-getting-started.md)
* [Správa spravované domény služby Azure AD Domain Services](active-directory-ds-admin-guide-administer-domain.md)
* [Správa zásad skupiny na spravované doméně služby Azure AD Domain Services](active-directory-ds-admin-guide-administer-group-policy.md)
* [Skupiny zabezpečení sítě](../virtual-network/virtual-networks-nsg.md)
* [Vytvořit skupinu zabezpečení sítě](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)
