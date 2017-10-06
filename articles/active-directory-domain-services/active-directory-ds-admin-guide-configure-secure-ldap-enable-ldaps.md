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
ms.openlocfilehash: 8781285cd02d690788056b985b017efd7e4d6b3f
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


## <a name="task-3---enable-secure-ldap-for-hello-managed-domain-using-hello-azure-portal-preview"></a>Úloha 3 – povolení zabezpečeného LDAP pro hello spravované doméně pomocí hello portálu Azure (Preview)
tooenable zabezpečený LDAP, proveďte následující kroky konfigurace hello:

1. Přejděte toohello  **[portál Azure](https://portal.azure.com)**.

2. Vyhledejte domain services v hello **vyhledávání prostředků** vyhledávacího pole. Vyberte **Azure AD Domain Services** z výsledek hledání hello. Hello **Azure AD Domain Services** okno uvádí vaší spravované domény.

    ![Najít spravované doméně, se zřídí](./media/getting-started/domain-services-provisioning-state-find-resource.png)

2. Další podrobnosti o hello domény klikněte na tlačítko hello název toosee hello spravované domény (například "contoso100.com").

    ![Doménových služeb – Stav zřizování](./media/getting-started/domain-services-provisioning-state.png)

3. Klikněte na tlačítko **zabezpečení LDAP** v navigačním podokně hello.

    ![Doménových služeb – okno zabezpečený LDAP](./media/active-directory-domain-services-admin-guide/secure-ldap-blade.png)

4. Zabezpečený LDAP přístup tooyour spravované domény je ve výchozím nastavení zakázaná. Přepnutí **zabezpečení LDAP** příliš**povolit**.

    ![Povolit zabezpečený LDAP](./media/active-directory-domain-services-admin-guide/secure-ldap-blade-configure.png)
5. Ve výchozím nastavení zabezpečeného přístupu tooyour LDAP spravované domény přes hello Internetu je zakázáno. Přepnutí **hello povolit zabezpečený LDAP přístup přes internet** příliš**povolit**, v případě potřeby. 

6. Klikněte na následující ikona složky hello **. Soubor PFX s certifikátem zabezpečení LDAP**. Zadejte soubor PFX toohello cesta hello s certifikátem hello zabezpečený LDAP přístup toohello spravované domény.

7. Zadejte hello **toodecrypt heslo. Soubor PFX**. Zadejte hello stejné heslo, které jste použili při exportu souboru PFX toohello hello certifikátu.

8. Až skončíte, klikněte na tlačítko hello **Uložit** tlačítko.

9. Zobrazí oznámení, že uvidíte, že zabezpečený LDAP je konfigurován pro hello spravované domény. Až do dokončení této operace, nelze změnit další nastavení pro doménu hello.

    ![Konfigurace zabezpečeného LDAP pro spravované doméně hello](./media/active-directory-domain-services-admin-guide/secure-ldap-blade-configuring.png)

> [!NOTE]
> Jak dlouho trvá přibližně 10 minut too15 tooenable zabezpečený LDAP vaší spravované domény. Pokud hello za předpokladu, že zabezpečený LDAP certifikát neodpovídá hello požadované kritéria, zabezpečený LDAP není povolena pro svůj adresář a zobrazit informace o selhání. Například hello název domény není správný, hello certifikátu již vypršela nebo brzo vyprší. V takovém případě zkuste to znovu s platným certifikátem.
>
>

<br>

## <a name="task-4---configure-dns-tooaccess-hello-managed-domain-from-hello-internet"></a>Úloha 4: Konfigurace DNS tooaccess hello spravované doméně z hello Internetu
> [!NOTE]
> **Nepovinná úloha** – Pokud neplánujete tooaccess hello spravované doméně pomocí LDAPS přes hello internet, přeskočte tento úkol konfigurace.
>
>

Než začnete tuto úlohu, zkontrolujte jste dokončili hello kroků uvedených v [úloha 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview).

Jakmile povolíte zabezpečený LDAP přístup přes internet hello vaší spravované domény, je nutné tooupdate DNS, aby klientské počítače najít této spravované domény. Na konci hello úlohy 3, externí IP adresa se zobrazí na hello **vlastnosti** okno v **externí IP adresu pro LDAPS přístup**.

Nakonfigurujte poskytovatele externí DNS tak, aby tento název DNS hello hello spravované domény (například "ldaps.contoso100.com') body toothis externí IP adresu. V našem příkladu potřebujeme toocreate hello následující položky DNS:

    ldaps.contoso100.com  -> 52.165.38.113

Je to – jsou nyní připraven tooconnect toohello spravované doméně pomocí zabezpečený LDAP přes hello Internetu.

> [!WARNING]
> Mějte na paměti, že klientské počítače musí důvěřovat hello vystavitele hello LDAPS certifikát toobe možné tooconnect úspěšně toohello spravované doméně pomocí LDAPS. Pokud používáte veřejně důvěryhodné certifikační autority, není nutné toodo nic vzhledem k tomu, že klientské počítače důvěřovat tyto vystavitelů certifikátů. Pokud používáte certifikát podepsaný svým držitelem, nainstalujte hello veřejnou část hello certifikát podepsaný svým držitelem do úložiště důvěryhodných certifikátů hello na klientském počítači hello.
>
>


## <a name="task-5---lock-down-ldaps-access-tooyour-managed-domain-over-hello-internet"></a>Úloha 5: zamykání LDAPS přístup tooyour spravované hello domény přes internet
> [!NOTE]
> **Nepovinná úloha** – Pokud jste nepovolili LDAPS spravované doméně toohello přístup přes hello internet, Přeskočit tuto úlohu konfigurace.
>
>

Než začnete tuto úlohu, zkontrolujte jste dokončili hello kroků uvedených v [úloha 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview).

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
