---
title: "aaaHow tooLink tooAzure předplatnému Azure AD B2C | Microsoft Docs"
description: "Podrobný průvodce tooenable fakturace pro klienta Azure AD B2C do předplatného Azure."
services: active-directory-b2c
documentationcenter: dev-center-name
author: rojasja
manager: mbaldwin
ms.service: active-directory-b2c
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/05/2016
ms.author: joroja
ms.openlocfilehash: 07b2ed5f7f5f543c9cbb8e9a35107462448e9440
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="linking-an-azure-subscription-tooan-azure-b2c-tenant-toopay-for-usage-charges"></a>Propojení předplatným Azure tooan Azure B2C klienta toopay pro poplatky za používání

Poplatky za probíhající používání pro Azure Active Directory B2C (nebo Azure AD B2C) jsou fakturovaná tooan předplatné Azure. Je nezbytné pro hello klienta správce tooexplicitly odkaz hello Azure AD B2C klienta tooan předplatného Azure po vytvoření klienta hello B2C, sám sebe.  Tento odkaz je dosaženo vytvořením Azure AD "Klienta B2C" prostředků v hello cílové předplatné Azure. Velký počet klientů B2C může být propojený tooa jedno předplatné Azure společně s dalším prostředkům služby Azure (například virtuální počítače, úložiště dat, LogicApps)


> [!IMPORTANT]
> nejnovější informace o využití fakturaci a cenách pro B2C je na následující stránku hello Hello: [cenová Azure AD B2C](
https://azure.microsoft.com/pricing/details/active-directory-b2c/)

## <a name="step-1---create-an-azure-ad-b2c-tenant"></a>Krok 1 – Vytvoření klienta Azure AD B2C
Vytvoření klienta B2C musíte nejprve dokončit. Tento krok přeskočte, pokud jste již vytvořili cílových klienta B2C. [Začínáme s Azure AD B2C](active-directory-b2c-get-started.md)

## <a name="step-2---open-azure-portal-in-hello-azure-ad-tenant-that-shows-your-azure-subscription"></a>Krok 2 – otevřete portál Azure hello klienta Azure AD, který ukazuje vašeho předplatného Azure
Přejděte toohello [portál Azure](https://portal.azure.com). Přepněte toohello klienta Azure AD, který ukazuje hello chcete toouse předplatného Azure. Tento klient Azure AD se liší od klienta B2C hello. V rámci hello portálu Azure klikněte na název účtu hello na hello pravé horní části hello řídicí panel tooselect hello klienta Azure AD. Předplatné Azure je potřebné tooproceed. [Získání předplatného Azure.](https://account.windowsazure.com/signup?showCatalog=True)

![Přepínání tooyour klienta Azure AD](./media/active-directory-b2c-how-to-enable-billing/SelectAzureADTenant.png)

## <a name="step-3---create-a-b2c-tenant-resource-in-azure-marketplace"></a>Krok 3 – vytvoření klienta B2C prostředků v Azure Marketplace
Otevřete kliknutím na ikonu hello Marketplace, nebo výběrem hello zelená "+" v hello levém horním rohu řídicího panelu hello Marketplace.  Vyhledejte a vyberte Azure Active Directory B2C. Vyberte možnost vytvořit.

![Vyberte Marketplace.](./media/active-directory-b2c-how-to-enable-billing/marketplace.png)

![Hledání AD B2C](./media/active-directory-b2c-how-to-enable-billing/searchb2c.png)

Hello prostředků Azure AD B2C vytvořit dialogové okno hello zahrnuje následující parametry:

1. Klienta Azure AD B2C – vyberte z rozevíracího seznamu hello klienta Azure AD B2C.  Zobrazit pouze oprávněné klienty Azure AD B2C.  Oprávněné klienty B2C splňovat tyto podmínky: hello globální správce klienta hello B2C a hello B2C klienta není aktuálně přidružené tooan předplatného Azure

2. Azure AD B2C prostředků název – je název domény hello Zkontrolujte předem vybrané toomatch hello klienta B2C

3. Předplatné -, ve kterém se správce nebo spolusprávce aktivní předplatné Azure.  Několik klientů Azure AD B2C, mohou být přidány tooone předplatného Azure

4. Umístění skupiny prostředků a skupina prostředků – tento artefaktů umožňuje uspořádat několik prostředků Azure.  Tato volba nemá žádný vliv na umístění klienta B2C, výkon nebo stav fakturace

5. Připnout toodashboard pro nejjednodušší klienta tooyour B2C přístup fakturační informace a hello nastavení klienta B2C ![vytvořit prostředek B2C](./media/active-directory-b2c-how-to-enable-billing/createresourceb2c.png)

## <a name="step-4---manage-your-b2c-tenant-resources-optional"></a>Krok 4 – spravovat prostředky klienta B2C (volitelné)
Po dokončení nasazení nového prostředku "Klienta B2C" je vytvořen v hello cílová skupina prostředků a souvisejících předplatného Azure.  Měli byste vidět nový prostředek typu "Klienta B2C" přidat spolu s vašich prostředků Azure.

![Vytvořte prostředek B2C](./media/active-directory-b2c-how-to-enable-billing/b2cresourcedashboard.png)

Kliknutím na hello B2C klienta prostředků, budete moci
- Klikněte na předplatné název tooreview fakturační informace. Viz fakturaci a využití.
- Klikněte na tlačítko Nastavení Azure AD B2C tooopen novou kartu prohlížeče přímo v klientovi tooyour B2C okno nastavení
- Odeslání žádosti o podporu
- Přesunout vaše tooanother prostředků klienta B2C předplatné Azure nebo tooanother skupinu prostředků.  Tato volba změny, které předplatné obdrží poplatky za používání.

![Nastavení prostředků B2C](./media/active-directory-b2c-how-to-enable-billing/b2cresourcesettings.png)

## <a name="known-issues"></a>Známé problémy
- Odstranění klienta B2C. Pokud klienta B2C je vytvořen, odstranit a znovu vytvořit s hello stejným názvem domény, prosím také odstranit a znovu vytvořit hello "Serveru Linking" prostředek s hello stejným názvem domény.  Budete tuto "Propojování" prostředků v části "Všechny prostředky" v klientovi předplatné hello prostřednictvím hello portálu Azure.
- Samoobslužné uložena omezení umístění místních prostředků.  Ve výjimečných případech může uživatel navázat místní omezení pro vytváření prostředků Azure.  Toto omezení by mohlo zabránit vytvoření hello hello propojení mezi předplatné Azure a klienta B2C. toomitigate, prosím uvolnit toto omezení.

## <a name="next-steps"></a>Další kroky
Po dokončení pro každou z vašich klientů B2C těchto kroků se vašeho předplatného Azure se fakturuje v souladu s podrobností o Azure přímý nebo smlouvu Enterprise.
- Zkontrolujte využití a fakturace ve vašem vybraném předplatném Azure
- Přečtěte si podrobné informace o použití ve dnech sestav pomocí hello [API pro vytváření sestav využití](active-directory-b2c-reference-usage-reporting-api.md)
