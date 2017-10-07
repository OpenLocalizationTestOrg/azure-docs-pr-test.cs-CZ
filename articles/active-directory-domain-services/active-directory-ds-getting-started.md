---
title: "Azure Active Directory Domain Services: Začínáme | Microsoft Docs"
description: "Povolit Azure Active Directory Domain Services pomocí hello portálu Azure (Preview)"
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
ms.openlocfilehash: 79cbb21c4a50194f5ad8ca1a4a8493ee4a260a9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-azure-active-directory-domain-services-using-hello-azure-portal-preview"></a>Povolit Azure Active Directory Domain Services pomocí hello portálu Azure (Preview)
Tento článek ukazuje, jak hello tooenable Azure Active Directory Domain Services (Azure AD DS) pomocí portálu Azure.


toolaunch hello **povolit Azure AD Domain Services** průvodce, dokončení hello následující kroky:

1. Přejděte toohello [portál Azure](https://portal.azure.com).
2. V levém podokně hello, klikněte na **nový**.
3. V hello **nový** okno, zadejte **Domain Services** do panelu Hledat hello.

    ![Vyhledání služeb domény](./media/getting-started/search-domain-services.png)

4. Klikněte na tlačítko tooselect **Azure AD Domain Services** ze seznamu hello návrhy vyhledávání. Na hello **Azure AD Domain Services** okně klikněte na tlačítko hello **vytvořit** tlačítko.

    ![Okno služby domény](./media/getting-started/domain-services-blade.png)

5. Hello **povolit Azure AD Domain Services** se spustí průvodce.


## <a name="task-1-configure-basic-settings"></a>Úloha 1: nakonfigurovali základní nastavení
V hello **Základy** stránku hello průvodce, můžete zadat název domény DNS hello hello spravované domény. Můžete také vybrat skupinu prostředků hello a umístění Azure toowhich hello spravované domény by měl být nasazena.

![Základní informace o konfiguraci](./media/getting-started/domain-services-blade-basics.png)

1. Zvolte hello **název domény DNS** vaší spravované domény.

   * Hello výchozí název domény hello adresáře (s **. onmicrosoft.com** příponu) je zadána ve výchozím nastavení.

   * Můžete také zadat v názvu vlastní domény. V tomto příkladu je název vlastní domény hello *contoso100.com*.

     > [!WARNING]
     > Předpona Hello vaší zadaného názvu domény (například *contoso100* v hello *contoso100.com* název domény) musí obsahovat nejvýše 15 znaků. Nelze vytvořit spravované doméně s předponou delší než 15 znaků.
     >
     >

2. Zkontrolujte název domény DNS hello, které jste vybrali pro hello spravované domény ve virtuální síti hello již neexistuje. Konkrétně, zkontrolujte, zda:

   * Už máte doménu hello stejným názvem domény DNS ve virtuální síti hello.

   * Hello virtuální síť, kde plánujete tooenable hello spravované domény nemá připojení k síti VPN s vaší místní síti. V tomto scénáři, ujistěte se, nemáte doménu hello stejným názvem domény DNS ve vaší místní síti.

   * Máte stávající cloudovou službu s tímto názvem ve virtuální síti hello.

3. Zvolte hello **typ virtuální sítě**. Ve výchozím nastavení, hello **Resource Manager** je vybrán virtuální sítě. Doporučujeme použít tento typ virtuální sítě pro všechny nově vytvořený spravované domény.

4. Vyberte hello Azure **předplatné** ve kterém chcete toocreate hello spravované domény.

5. Vyberte hello **skupiny prostředků** by měly patřit toowhich hello spravované domény. Můžete buď hello **vytvořit nový** nebo **použít existující** skupiny prostředků hello tooselect možnosti.

6. Zvolte hello Azure **umístění** v které hello je potřeba vytvořit spravované domény. Na hello **sítě** stránku hello Průvodce zobrazí pouze virtuální sítě, které patří toohello umístění, které jste vybrali.

7. Až budete hotovi, klikněte na tlačítko **OK** toomove na toohello **sítě** hello průvodce.


## <a name="next-step"></a>Další krok
[Úloha 2: Konfigurace nastavení sítě](active-directory-ds-getting-started-network.md)
