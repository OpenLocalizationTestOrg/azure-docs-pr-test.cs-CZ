---
title: "Azure Active Directory Domain Services: Začínáme | Microsoft Docs"
description: "Povolit Azure Active Directory Domain Services pomocí portálu Azure (Preview)"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: ace1ed4a-bf7f-43c1-a64a-6b51a2202473
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: maheshu
ms.openlocfilehash: 47507096a6245d4f1ba57a652ddf5167b3776ae9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal-preview"></a>Povolit Azure Active Directory Domain Services pomocí portálu Azure (Preview)
Tento článek ukazuje, jak povolit Azure Active Directory Domain Services (Azure AD DS) pomocí portálu Azure.


Ke spuštění **povolit Azure AD Domain Services** průvodce proveďte následující kroky:

1. Přejděte na [portál Azure](https://portal.azure.com).
2. V levém podokně klikněte na **nový**.
3. V **nový** okno, zadejte **Domain Services** do panelu vyhledávání.

    ![Vyhledání služeb domény](./media/getting-started/search-domain-services.png)

4. Kliknutím vyberte **Azure AD Domain Services** ze seznamu návrhy vyhledávání. Na **Azure AD Domain Services** okně klikněte **vytvořit** tlačítko.

    ![Okno služby domény](./media/getting-started/domain-services-blade.png)

5. **Povolit Azure AD Domain Services** se spustí průvodce.


## <a name="task-1-configure-basic-settings"></a>Úloha 1: nakonfigurovali základní nastavení
V **Základy** stránku průvodce, můžete zadat název domény DNS pro spravovanou doménu. Můžete také zvolit skupinu prostředků a umístění Azure, ke které by měly být nasazeny spravované doméně.

![Základní informace o konfiguraci](./media/getting-started/domain-services-blade-basics.png)

1. Vyberte **název domény DNS** vaší spravované domény.

   * Výchozí název domény adresáře (s **. onmicrosoft.com** příponu) je zadána ve výchozím nastavení.

   * Můžete také zadat v názvu vlastní domény. V tomto příkladu je použit vlastní název domény *contoso100.com*.

     > [!WARNING]
     > Předpona zadaného názvu domény (například *contoso100* v případě názvu domény *contoso100.com*) musí obsahovat nejvýše 15 znaků. Nelze vytvořit spravované doméně s předponou delší než 15 znaků.
     >
     >

2. Ujistěte se, že vámi zvolený název domény DNS pro spravovanou doménu ještě ve virtuální síti neexistuje. Konkrétně, zkontrolujte, zda:

   * Ve virtuální síti již existuje doména se stejným názvem domény DNS.

   * Virtuální síť, kde budete chtít povolit spravované domény nemá připojení k síti VPN s vaší místní síti. V tomto scénáři Ujistěte se, že nemáte doménu se stejným názvem domény DNS ve vaší místní síti.

   * Ve virtuální síti existuje cloudová služba s tímto názvem.

3. Vyberte **typ virtuální sítě**. Ve výchozím nastavení **Resource Manager** je vybrán virtuální sítě. Doporučujeme použít tento typ virtuální sítě pro všechny nově vytvořený spravované domény.

4. Vyberte Azure **předplatné** ve kterém chcete vytvořit spravované domény.

5. Vyberte **skupiny prostředků** k spravované doméně by měly patřit. Můžete buď **vytvořit nový** nebo **použít existující** možnosti vybrat skupinu prostředků.

6. Zvolte Azure **umístění** ve které má být vytvořena spravované domény. Na **sítě** stránky v průvodci zobrazí pouze virtuální sítě, které patří do umístění, které jste vybrali.

7. Až budete hotovi, klikněte na tlačítko **OK** přesunout na **sítě** stránce průvodce.


## <a name="next-step"></a>Další krok
[Úloha 2: Konfigurace nastavení sítě](active-directory-ds-getting-started-network.md)
