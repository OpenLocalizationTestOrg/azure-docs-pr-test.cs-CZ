---
title: "Azure Active Directory B2C: Dostupnost & data sídle oblast | Microsoft Docs"
description: "Téma na typech hello klientům Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: gsacavdm
manager: krassk
editor: bryanla
ms.assetid: 8a0644da-b825-4edc-8ce9-541c3c976afb
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2017
ms.author: gsacavdm
ms.openlocfilehash: d382921fe9d62183b6d52c47d62f952dabd116c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-region-availability--data-residency"></a>Azure Active Directory B2C: Dostupnost & data sídle oblast
Dostupnost v oblastech a sídla dat jsou dvě příliš neliší koncepty, které jinak použít tooAzure AD B2C z hello rest Azure. Tento článek se popisují hello rozdíly mezi těmito dvěma konceptů a porovnání, jak se vztahují tooAzure a Azure AD B2C.

## <a name="summary"></a>Souhrn
Azure AD B2C je **všeobecně dostupná po celém světě** s hello možností pro **sídle data ve Spojených státech amerických nebo Evropa**.

## <a name="concepts"></a>Koncepty
* **Dostupnost v oblastech** odkazuje toowhere je k dispozici pro použití služby.
* **Data sídle** odkazuje toowhere uživatele jsou uložena data.

## <a name="region-availability"></a>Dostupnost v oblastech
Azure AD B2C je k dispozici po celém světě prostřednictvím hello veřejného cloudu Azure. 

To se liší od hello modelu většině ostatních služeb Azure postupujte podle kroků, které spojte dostupnosti s sídle data. Vidíte příklady tohoto v obou Azure [produkty dostupné podle oblasti](https://azure.microsoft.com/regions/services/) stránku a hello [Active Directory B2C cenové kalkulačky](https://azure.microsoft.com/pricing/details/active-directory-b2c/).

## <a name="data-residency"></a>Rezidence dat
Azure AD B2C data uživatelů uloží ve Spojených státech amerických nebo Evropa.

Sídlo dat je určen podle zemi nebo region je vybraná při [vytvoření klienta Azure AD B2C](active-directory-b2c-get-started.md).

![Snímek obrazovky preview klienta](./media/active-directory-b2c-reference-tenant-type/data-residency-b2c-tenant.png)

Data se nachází v hello Spojených států pro následující zemích hello:

> Spojené státy, Kanada, Kostarika, Dominikánská republika, El Salvador, Kostarika, Mexico, Panama, Portoriko a Trinidad a Tobago

Data se nachází v Evropě pro hello následující zemích nebo oblastech:

> Alžírsko, Rakousko, Ázerbájdžán, Bahrajn, Běloruska, Belgie, Bulharsko, Chorvatsko, Kypru, Česká republika, Dánsko, Egypta, Estonska, Finsko, Francie, Německo, Řecko, Maďarska, Island, Irská, Izrael, Itálie, Jordánsko, Kazachstán, Keni, Kuvajt, Lotyšska, Libanon, Lichtenštejnsko, Lituania, Lucembursko, Makedonie FYRO, Malty, Černá Hora, Maroko, Nizozemsko, Nigérie, Norsko, Omán, Pákistánu, Polsko, Portugalsko, Katar, Rumunska, Ruska, Saúdská Arábie, Srbsko, Slovenska, Slovinsko, Jihoafrická republika, Španělsko, Švédsko, Švýcarska, Tunisko, Turecko, Ukrajina, Spojené arabské emiráty a Spojené království.

Hello zbývající zemích jsou v procesu hello přidávané toohello seznamu.  Teď stále můžete Azure AD B2C výběrem žádné hello zemích výše.

> Afghánistán, Argentiny, Austrálie, Brazílie, základě, Kolumbie, Ekvádor, Hongkong – zvláštní správní oblast ČLR, Indie, Indonésie, Irák, Japonsko, Korejská, Malajsie, Nový Zéland, Paraguay, Peru, Filipíny, Singapur, Srí Lanka, Tchaj-wan, Thajska, Uruguayského a Venezuela.

## <a name="preview-tenant"></a>Náhled klienta
Pokud jste vytvořili klienta B2C během období preview Azure AD B2C, je pravděpodobné, který vaše **klienta typu** uvádí **Preview klienta**. Pokud se jedná o případ hello, používejte pouze pro účely vývoje a testování a ne pro produkční aplikace vašeho klienta.

> [!IMPORTANT]
> Neexistuje cesta migrace z náhled klienta B2C produkční škálování tooa klienta B2C. Všimněte si, že existují známé problémy při odstranění preview B2C klienta a znovu vytvořit klienta B2C produkční škálování s hello se stejným názvem domény. Máte toocreate klienta B2C produkční škálování s jiným názvem domény.


![Snímek obrazovky preview klienta](./media/active-directory-b2c-reference-tenant-type/preview-b2c-tenant.png)
