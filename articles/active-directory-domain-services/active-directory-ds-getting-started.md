---
title: "Azure Active Directory Domain Services: Začínáme | Microsoft Docs"
description: "Povolit Azure Active Directory Domain Services pomocí portálu Azure"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: ace1ed4a-bf7f-43c1-a64a-6b51a2202473
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/26/2017
ms.author: maheshu
ms.openlocfilehash: 16b206b97f71eeffbb4fddb9e447649034998672
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal"></a>Povolit Azure Active Directory Domain Services pomocí portálu Azure
Tento článek ukazuje, jak povolit Azure Active Directory Domain Services (Azure AD DS) pomocí portálu Azure.

Ke spuštění **povolit Azure AD Domain Services** průvodce proveďte následující kroky:

1. Přejděte na [portál Azure](https://portal.azure.com).
2. V levém podokně klikněte na **nový**.
3. V **nový** zadejte **Domain Services** do panelu vyhledávání.

    ![Vyhledání služeb domény](./media/getting-started/search-domain-services.png)

4. Kliknutím vyberte **Azure AD Domain Services** ze seznamu návrhy vyhledávání. Na **Azure AD Domain Services** klikněte na tlačítko **vytvořit** tlačítko.

    ![Zobrazení služeb domény](./media/getting-started/domain-services-blade.png)

5. **Povolit Azure AD Domain Services** se spustí průvodce.


## <a name="task-1-configure-basic-settings"></a>Úloha 1: nakonfigurovali základní nastavení
V **Základy** stránku průvodce, můžete zadat název domény DNS pro spravovanou doménu. Můžete také zvolit skupinu prostředků a umístění Azure, ke které by měly být nasazeny spravované doméně.

![Základní informace o konfiguraci](./media/getting-started/domain-services-blade-basics.png)

1. Vyberte **název domény DNS** vaší spravované domény.

   > [!NOTE]
   > **Pokyny pro výběr název domény DNS**
   > * **Název předdefinované domény:** ve výchozím nastavení, průvodce určuje název výchozí nebo vestavěné domény adresáře (s **. onmicrosoft.com** příponu) za vás. Pokud chcete umožnit zabezpečený přístup protokolu LDAP k spravované doméně přes internet, očekávejte problémy vytváření veřejný záznam DNS nebo získání certifikátu zabezpečeného LDAP od veřejné certifikační Autority pro tento název domény. Vlastní Microsoft *. onmicrosoft.com* domény a certifikačních autorit nevydá certifikáty zaručování pro tuto doménu.
   * **Názvy vlastních domén:** můžete také zadat v názvu vlastní domény. V tomto příkladu je použit vlastní název domény *contoso100.com*.
   * **Přípony domény bez směrovatelné:** obecně doporučujeme zabraňující přípon názvů směrovat domén. Pro instanci je lepší Vyhněte se vytváření doméně s názvem domény DNS, contoso.local'. Přípona DNS, .local' není směrovatelný a může způsobit problémy s překladem DNS.
   * **Omezení domény předpony:** předponu vaší zadaného názvu domény (například *contoso100* v *contoso100.com* název domény) musí obsahovat nejvýše 15 znaků. Nelze vytvořit spravované doméně s předponou delší než 15 znaků.
   * **Síťový název je v konfliktu:** zkontrolujte, zda název domény DNS, který jste zvolili pro spravovanou doménu ve virtuální síti již neexistuje. Konkrétně, zkontrolujte, zda:
       * Už máte doménu služby Active Directory se stejným názvem domény DNS ve virtuální síti.
       * Virtuální síť, kde budete chtít povolit spravované domény nemá připojení k síti VPN s vaší místní síti. V tomto scénáři Ujistěte se, že nemáte doménu se stejným názvem domény DNS ve vaší místní síti.
       * Ve virtuální síti existuje cloudová služba s tímto názvem.
    >

2. Vyberte Azure **předplatné** ve kterém chcete vytvořit spravované domény.

3. Vyberte **skupiny prostředků** k spravované doméně by měly patřit. Můžete buď **vytvořit nový** nebo **použít existující** možnosti vybrat skupinu prostředků.

4. Zvolte Azure **umístění** ve které má být vytvořena spravované domény. Na **sítě** stránky v průvodci zobrazí pouze virtuální sítě, které patří do umístění, které jste vybrali.

5. Až budete hotovi, klikněte na tlačítko **OK** přesunout na **sítě** stránce průvodce.


## <a name="next-step"></a>Další krok
[Úloha 2: Konfigurace nastavení sítě](active-directory-ds-getting-started-network.md)
