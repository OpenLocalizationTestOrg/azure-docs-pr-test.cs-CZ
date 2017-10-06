---
title: "aaaConfigure zabezpečeného LDAP (LDAPS) v Azure AD Domain Services | Microsoft Docs"
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
ms.openlocfilehash: a0d6e2faf474b1f0cbe157fb4ae2754b1d521ef9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Konfigurace zabezpečeného LDAP (LDAPS) pro spravované doméně služby Azure AD Domain Services

## <a name="before-you-begin"></a>Než začnete
Ujistěte se, když jste dokončili [úloha 2 – export hello zabezpečený LDAP certifikát tooa. Soubor PFX](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md).

Vyberte, zda toouse hello práci s portálem Azure preview nebo hello Azure classic portálu toocomplete této úlohy.
> [!div class="op_single_selector"]
> * **Portál Azure (Preview)**: [povolit zabezpečený LDAP pomocí hello portálu Azure](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)
> * **Portál Azure classic**: [povolit zabezpečený LDAP pomocí portálu Azure classic hello](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps-classic.md)
>
>


## <a name="task-3---enable-secure-ldap-for-hello-managed-domain-using-hello-classic-azure-portal"></a>Úloha 3 – povolení zabezpečeného LDAP pro hello spravované doméně pomocí portálu Azure classic hello
tooenable zabezpečený LDAP, proveďte následující kroky konfigurace hello:

1. Přejděte toohello  **[portál Azure classic](https://manage.windowsazure.com)**.
2. Vyberte hello **služby Active Directory** uzlu v levém podokně hello.
3. Vyberte adresář hello Azure AD (také odkazované tooas "klienta"), pro kterou jste povolili službu Azure AD Domain Services.

    ![Vyberte adresář služby Azure AD](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. Klikněte na tlačítko hello **konfigurace** kartě.

    ![Karta konfigurace adresáře](./media/active-directory-domain-services-getting-started/configure-tab.png)
5. Projděte dolů toohello části s názvem **služby domény**. Měli byste vidět možnost s názvem **zabezpečení protokolu LDAP (LDAPS)** jak ukazuje následující snímek obrazovky hello:

    ![Oddíl konfigurace doménových služeb](./media/active-directory-domain-services-admin-guide/secure-ldap-start.png)
6. Klikněte na tlačítko hello **konfigurovat certifikát...**  toobring tlačítko nahoru hello **konfigurace certifikátu pro zabezpečené LDAP** dialogové okno.

    ![Konfigurace certifikátu pro zabezpečený LDAP](./media/active-directory-domain-services-admin-guide/secure-ldap-configure-cert-page.png)
7. Klikněte na následující ikona složky hello **certifikát pomocí souboru PFX** toospecify hello PFX souboru, který obsahuje certifikát hello chcete toouse pro zabezpečený LDAP přístup toohello spravované domény. Zadejte také hello heslo, které jste zadali při exportu souboru PFX toohello hello certifikátu. Potom klikněte na provést na dolní hello tlačítko hello.

    ![Zadejte soubor zabezpečený LDAP PFX a heslo](./media/active-directory-domain-services-admin-guide/secure-ldap-specify-pfx.png)
8. Hello **domény služby** části hello **konfigurace** by měl získat šedě a je v hello **čekající na vyřízení...**  stavu pro několik minut. Během této doby je hello LDAPS certifikát ověřit přesnost a zabezpečený LDAP je nakonfigurována pro vaší spravované domény.

    ![Zabezpečený LDAP - čekající na vyřízení stavu](./media/active-directory-domain-services-admin-guide/secure-ldap-pending-state.png)

   > [!NOTE]
   > Jak dlouho trvá přibližně 10 minut too15 tooenable zabezpečený LDAP vaší spravované domény. Pokud hello za předpokladu, že zabezpečený LDAP certifikát neodpovídá hello požadované kritéria, zabezpečený LDAP není povolena pro svůj adresář a zobrazit informace o selhání. Například hello název domény není správný, hello certifikátu již vypršela nebo brzo vyprší.
   >
   >

9. Když zabezpečený LDAP úspěšně povolen vaší spravované domény, hello **čekající na vyřízení...**  by zpráva zmizí. Měli byste vidět hello kryptografický otisk certifikátu hello zobrazí.

    ![Zabezpečený LDAP povoleno](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled.png)

<br>

## <a name="task-4---enable-secure-ldap-access-over-hello-internet"></a>Úloha 4 – povolení zabezpečeného přístupu LDAP přes hello Internetu
**Nepovinná úloha** – Pokud neplánujete tooaccess hello spravované doméně pomocí LDAPS přes hello internet, přeskočte tento úkol konfigurace.

Než začnete tuto úlohu, zkontrolujte jste dokončili hello kroků uvedených v [úloha 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-classic-azure-portal).

1. Měli byste vidět možnost příliš**hello povolit ZABEZPEČENÉ LDAP přístup přes INTERNET** v hello **domény služby** části hello **konfigurace** stránky. Tato možnost nastavená příliš**ne** ve výchozím nastavení vzhledem k tomu, že toohello přístup k Internetu spravované domény přes zabezpečený LDAP ve výchozím nastavení vypnutá.

    ![Zabezpečený LDAP povoleno](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled2.png)
2. Přepnutí **hello povolit ZABEZPEČENÉ LDAP přístup přes INTERNET** příliš**Ano**. Klikněte na tlačítko hello **Uložit** tlačítko na dolním panelu hello.
    ![Zabezpečený LDAP povoleno](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access.png)
3. Hello **domény služby** části hello **konfigurace** by měl získat šedě a je v hello **čekající na vyřízení...**  stavu pro několik minut. Po určité době je povoleno spravované domény tooyour internet přístup přes zabezpečený LDAP.

    ![Zabezpečený LDAP - čekající na vyřízení stavu](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access-pending-state.png)

   > [!NOTE]
   > Jak dlouho trvá přibližně 10 minut tooenable přístup k Internetu přes zabezpečený LDAP vaší spravované domény.
   >
   >
4. Pokud zabezpečeného přístupu tooyour LDAP spravované domény přes hello Internetu byl úspěšně povolen, hello **čekající na vyřízení...**  by zpráva zmizí. Měli byste vidět hello externí IP adresu, kterou se dá použít tooaccess adresáře prostřednictvím LDAPS v poli hello **externí IP adresu pro LDAPS přístup**.

    ![Zabezpečený LDAP povoleno](./media/active-directory-domain-services-admin-guide/secure-ldap-internet-access-enabled.png)

<br>

## <a name="task-5---configure-dns-tooaccess-hello-managed-domain-from-hello-internet"></a>Úloha 5: Konfigurace DNS tooaccess hello spravované doméně z hello Internetu
**Nepovinná úloha** – Pokud neplánujete tooaccess hello spravované doméně pomocí LDAPS přes hello internet, přeskočte tento úkol konfigurace.

Než začnete tuto úlohu, zkontrolujte jste dokončili hello kroků uvedených v [úloha 4](#task-4---enable-secure-ldap-access-over-the-internet).

Jakmile povolíte zabezpečený LDAP přístup přes internet hello vaší spravované domény, je nutné tooupdate DNS, aby klientské počítače najít této spravované domény. Na konci hello úlohy 4, externí IP adresa se zobrazí na hello **konfigurace** kartě v **externí IP adresu pro LDAPS přístup**.

Nakonfigurujte poskytovatele externí DNS tak, aby tento název DNS hello hello spravované domény (například "ldaps.contoso100.com') body toothis externí IP adresu. V našem příkladu potřebujeme toocreate hello následující položky DNS:

    ldaps.contoso100.com  -> 52.165.38.113

Je to – jsou nyní připraven tooconnect toohello spravované doméně pomocí zabezpečený LDAP přes hello Internetu.

> [!WARNING]
> Mějte na paměti, že klientské počítače musí důvěřovat hello vystavitele hello LDAPS certifikát toobe možné tooconnect úspěšně toohello spravované doméně pomocí LDAPS. Pokud používáte certifikační autoritu organizace nebo veřejně důvěryhodné certifikační autority, není nutné toodo nic vzhledem k tomu, že klientské počítače důvěřovat tyto vystavitelů certifikátů. Pokud používáte certifikát podepsaný svým držitelem, je třeba tooinstall hello veřejnou část hello certifikát podepsaný svým držitelem do úložiště důvěryhodných certifikátů hello na klientském počítači hello.
>
>


## <a name="lock-down-ldaps-access-tooyour-managed-domain-over-hello-internet"></a>Uzamčení LDAPS přístup ke spravované doméně tooyour přes hello Internetu
> [!NOTE]
> **Nepovinná úloha** – Pokud jste nepovolili LDAPS spravované doméně toohello přístup přes hello internet, Přeskočit tuto úlohu konfigurace.
>
>

Než začnete tuto úlohu, zkontrolujte jste dokončili hello kroků uvedených v [úloha 4](#task-4---enable-secure-ldap-access-over-the-internet).

Vystavení vaší spravované domény pro LDAPS přístup přes hello internet představuje bezpečnostní riziko. Hello spravované domény je dostupný z hello Internetu v hello port používaný pro zabezpečený LDAP (to znamená, port 636). Proto můžete toorestrict přístup toohello spravované domény toospecific známé IP adresy. Pro lepší zabezpečení vytvořte skupinu zabezpečení sítě (NSG) a přidružte ji k hello podsíť, kde jste povolili službu Azure AD Domain Services.

Hello následující tabulka znázorňuje ukázku NSG můžete nakonfigurovat, toolock dolů zabezpečený LDAP přístup přes hello Internetu. Hello NSG obsahuje sadu pravidel, která povolí příchozí LDAPS přístup přes port TCP 636 pouze ze zadané sady IP adres. Hello výchozí 'DenyAll' pravidlo tooall další příchozí provoz z hello Internetu. Hello NSG pravidlo tooallow LDAPS přístup prostřednictvím hello má vyšší prioritu než hello pravidla DenyAll NSG internet ze zadaných IP adres.

![Hello ukázka NSG toosecure LDAPS přístup prostřednictvím Internetu](./media/active-directory-domain-services-admin-guide/secure-ldap-sample-nsg.png)

**Další informace** - [skupin zabezpečení sítě](../virtual-network/virtual-networks-nsg.md).

<br>

## <a name="related-content"></a>Související obsah
* [Azure AD Domain Services – Příručka Začínáme](active-directory-ds-getting-started.md)
* [Správa spravované domény služby Azure AD Domain Services](active-directory-ds-admin-guide-administer-domain.md)
* [Správa zásad skupiny na spravované doméně služby Azure AD Domain Services](active-directory-ds-admin-guide-administer-group-policy.md)
* [Skupiny zabezpečení sítě](../virtual-network/virtual-networks-nsg.md)
* [Vytvořit skupinu zabezpečení sítě](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)
