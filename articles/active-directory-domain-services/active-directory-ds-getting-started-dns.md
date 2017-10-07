---
title: "Azure Active Directory Domain Services: Aktualizace nastavení DNS pro hello virtuální síť Azure | Microsoft Docs"
description: "Začínáme se službou Azure Active Directory Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: d4f3e82c-6807-4690-b298-4eabad2b7927
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/27/2017
ms.author: maheshu
ms.openlocfilehash: e6eaff555cb9b7bb89ab7581d8de0b8cfc844529
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-azure-active-directory-domain-services-preview"></a>Povolení služby Azure Active Directory Domain Services (Preview)

## <a name="task-4-update-dns-settings-for-hello-azure-virtual-network"></a>Úloha 4: aktualizace nastavení DNS pro hello virtuální síť Azure
V předchozích úlohy konfigurace hello povolíte úspěšně Azure Active Directory Domain Services pro svůj adresář. Další úlohou Hello je tooensure, můžete počítače v rámci virtuální sítě hello připojit a využívat tyto služby. V tomto článku aktualizace hello nastavení serveru DNS pro vaši virtuální síť toopoint toohello dvě IP adresy kde je k dispozici ve virtuální síti hello Azure Active Directory Domain Services.

tooupdate hello serveru DNS pro hello virtuální síť, ve kterém jste povolili Azure Active Directory Domain Services dokončení hello následující kroky:

1. Hello **přehled** karta obsahuje sadu **požadované kroky konfigurace** toobe provést po vaší spravované domény plně zřízený. prvním krokem konfigurace Hello je **nastavení serveru DNS aktualizace pro vaši virtuální síť**.

    ![Domain Services – Karta Přehled po úplném zřízení](./media/getting-started/domain-services-provisioned-overview.png)

2. Když je doména úplně zřízená, zobrazují se na této dlaždici dvě IP adresy. Každý z těchto IP adres představuje řadič vaší spravované domény.

3. toocopy hello první IP adresa tooclipboard, klikněte na další tooit tlačítko hello kopírování. Pak klikněte na tlačítko hello **servery DNS nakonfigurujte** tlačítko.

4. Vložte hello první IP adresu do hello **server DNS přidat** textového pole v hello **servery DNS** okno. Posuňte vodorovně toohello zbývajících toocopy hello druhou IP adresu a vložte jej do hello **server DNS přidat** textové pole.

    ![Domain Services – Aktualizace DNS](./media/getting-started/domain-services-update-dns.png)

5. Klikněte na tlačítko **Uložit** po dokončení tooupdate hello servery DNS pro virtuální síť hello.

> [!NOTE]
> Virtuální počítače v síti hello získat nové nastavení DNS hello pouze po restartu. Chcete-li nastavení DNS tooget hello aktualizovat hned, aktivovat restartování hello portál, prostředí PowerShell nebo hello rozhraní příkazového řádku.
>
>

## <a name="next-step"></a>Další krok
[Úloha 5: povolení tooAzure synchronizace hesla Active Directory Domain Services](active-directory-ds-getting-started-password-sync.md)
