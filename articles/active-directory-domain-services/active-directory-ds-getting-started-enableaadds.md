---
title: "Azure Active Directory Domain Services: Povolení služby Azure Active Directory Domain Services | Dokumentace Microsoftu"
description: "Povolit Azure Active Directory Domain Services pomocí hello portál Azure classic"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: c659da59-f4b5-4edd-b702-1727a8ccb36f
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/28/2017
ms.author: maheshu
ms.openlocfilehash: 6263eb1849808a7c85e572e1046bc9039362dd9f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-azure-active-directory-domain-services-using-hello-azure-classic-portal"></a>Povolit Azure Active Directory Domain Services pomocí hello portál Azure classic

## <a name="task-3-enable-azure-active-directory-domain-services"></a>Úloha 3: Povolení služby Azure Active Directory Domain Services
V této úloze povolíte Azure Active Directory Domain Services (Azure AD DS) pro váš adresář díky hello následující kroky:

1. Přejděte toohello [portál Azure classic](https://manage.windowsazure.com).
2. V levém podokně hello vyberte hello **služby Active Directory** tlačítko.
3. Vyberte klienta Azure Active Directory (Azure AD) hello (adresář) pro které chcete tooenable služby Azure AD DS.

    ![Vyberte adresář služby Azure AD](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. Na hello **preview directory** klikněte na tlačítko hello **konfigurace** kartě.

    ![Karta konfigurace adresáře](./media/active-directory-domain-services-getting-started/configure-tab.png)
5. V části **služby domény**, změňte hello **povolit doménové služby pro tento adresář** možnost příliš**Ano**.  
    Na stránce hello se zobrazí další možnosti konfigurace Azure Active Directory Domain Services.

    ![Povolení doménových služeb](./media/active-directory-domain-services-getting-started/enable-domain-services.png)

   > [!NOTE]
   > Když povolíte Azure Active Directory Domain Services pro vašeho klienta, Azure AD generuje a uloží hello protokolu Kerberos a NTLM hodnoty hash přihlašovacích údajů vyžadované pro ověřování uživatelů.
   >
   >
6. Zadejte hello **název domény DNS služby domain services**.

   * Hello výchozí název domény hello adresáře (s **. onmicrosoft.com** příponu) je standardně vybraná.

   * Hello seznam obsahuje všechny domény, které byly nakonfigurovány pro váš adresář Azure AD, včetně obě ověřit a neověřených domén, které nakonfigurujete na hello **domén** kartě.

   * Můžete zadat i vlastní název domény. V tomto příkladu je název vlastní domény hello *contoso100.com*.

     > [!WARNING]
     > Předpona Hello vaší zadaného názvu domény (například *contoso100* v hello *contoso100.com* název domény) musí obsahovat nejvýše 15 znaků. Nelze vytvořit domény služby Azure Active Directory Domain Services s předponou obsahující více než 15 znaků.
     >
     >
7. Zkontrolujte název domény DNS hello, které jste vybrali pro hello spravované domény ve virtuální síti hello již neexistuje. Konkrétně zkontrolujte zda toosee:

   * Už máte doménu hello stejným názvem domény DNS ve virtuální síti hello.

   * Hello virtuální sítě, které jste vybrali má připojení VPN s vaší místní síti a máte doménu s hello stejným názvem domény DNS ve vaší místní síti.

   * Máte stávající cloudovou službu s tímto názvem ve virtuální síti hello.
8. Vyberte virtuální síť, na kterém chcete Azure Active Directory Domain Services toobe k dispozici. Vyberte virtuální síť hello a vyhrazené podsíť, které jste vytvořili v hello **služeb domény připojit virtuální síť toothis** rozevíracího seznamu. Zvažte také následující hello:

   * Zkontrolujte, zda text hello virtuální síti, kterou jste určili patří tooan oblast Azure, která je podporována službou Azure Active Directory Domain Services. tooascertain hello oblastí Azure, kde je k dispozici Azure Active Directory Domain Services najdete v tématu [služby Azure podle oblasti](https://azure.microsoft.com/regions/#services/).

   * V rozevíracím seznamu hello se nezobrazí virtuální sítě, které patří tooa oblasti, kde se nepodporuje Azure Active Directory Domain Services.

   * Použijte vyhrazenou podsítí v rámci hello virtuální sítě pro Azure Active Directory Domain Services. Proveďte *není* vyberte hello podsíť brány. Viz téma o [aspektech sítí](active-directory-ds-networking.md).

   * Podobně virtuální sítě, které byly vytvořeny pomocí Azure Resource Manager nejsou uvedena v rozevíracím seznamu hello. Virtuální sítě na bázi Resource Manageru nejsou v současné době službou Azure Active Directory Domain Services podporované.
9. Klikněte na tlačítko tooenable Azure Active Directory Domain Services, v podokně úloh hello v hello dolní části stránky hello **Uložit**.
    * Zatímco Azure Active Directory Domain Services je povolený pro váš adresář, stránku hello zobrazuje stav *čekající*.

        ![Povolení okna Domain Services](./media/active-directory-domain-services-getting-started/enable-domain-services-pendingstate.png)

        > [!NOTE]
        > Služba Azure Active Directory Domain Services poskytuje vysokou dostupnost vaší spravované domény. Jakmile povolíte Azure Active Directory Domain Services, jsou hello IP adresy, které služby jsou dostupné ve virtuální síti hello zobrazených současně. Hello druhou IP adresu zobrazí krátce po hello nejprve jako brzy hello služba povolí vysokou dostupnost vaší domény. Při vysoké dostupnosti je nakonfigurovaná a aktivní pro vaši doménu, jste měli vidět dvě IP adresy v hello **služby domény** části hello **konfigurace** kartě.
        >
        >
    * Po přibližně 20 minut too30, hello první IP adresy ve které doméně služby jsou dostupné ve vaší virtuální síti v hello **IP adresu** na hello **konfigurace** stránky.

        ![Okno Domain Services se zobrazenou první zřízenou IP adresou](./media/active-directory-domain-services-getting-started/domain-services-enabled-firstdc-available.png)
    * Když je vysoká dostupnost provozní vaší domény, zobrazí se na stránce hello dvě IP adresy. Na těchto dvou IP adresách je k dispozici vaše spravovaná doména ve vybrané virtuální síti.

10. Poznamenejte si hello dvě IP adresy, abyste aktualizujete hello nastavení DNS pro vaši virtuální síť. Díky tomu virtuální počítače na hello virtuální sítě tooconnect toohello domény pro operace, jako je například připojení k doméně.

    ![Okno služby Domain Service se zobrazením obou zřízených IP adres](./media/active-directory-domain-services-getting-started/domain-services-enabled-bothdcs-available.png)

> [!NOTE]
> Spravované domény tooyour synchronizace v závislosti na velikosti hello klienta služby Azure AD (například hello počet uživatele nebo skupiny), trvá několik minut. Tento proces synchronizace se odehrává na pozadí hello. U velkých klientů s desítkami tisíc objektů může trvat i několik dní pro všechny uživatele, členství ve skupinách a přihlašovacích údajů toobe synchronizované.
>
>

## <a name="next-step"></a>Další krok
[Úloha 4: aktualizace nastavení DNS hello hello virtuální síť Azure](active-directory-ds-getting-started-update-dns.md)
