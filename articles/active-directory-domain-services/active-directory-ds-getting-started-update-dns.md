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
ms.openlocfilehash: 484ff1a197a651bccb2b416448056acf69b0d8c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="update-dns-settings-for-hello-azure-virtual-network"></a>Aktualizace nastavení DNS pro hello virtuální síť Azure
## <a name="task-4-update-dns-settings-for-hello-azure-virtual-network"></a>Úloha 4: Aktualizace nastavení DNS pro hello virtuální síť Azure
V předchozích úlohy konfigurace hello povolíte úspěšně Azure Active Directory Domain Services pro svůj adresář. Další úlohou Hello je tooensure, můžete počítače v rámci virtuální sítě hello připojit a využívat tyto služby. V tomto článku aktualizace hello nastavení serveru DNS pro vaši virtuální síť toopoint toohello dvě IP adresy kde je k dispozici ve virtuální síti hello Azure Active Directory Domain Services.

> [!NOTE]
> Poté, co jste povolili Azure Active Directory Domain Services pro adresář hello, Všimněte si hello IP adresy pro Azure Active Directory Domain Services, zobrazí se na hello **konfigurace** svůj adresář.
>
>

tooupdate hello serveru DNS pro hello virtuální síť, ve kterém jste povolili Azure Active Directory Domain Services dokončení hello následující kroky:

1. Přejděte toohello [portál Azure classic](https://manage.windowsazure.com).
2. V levém podokně hello vyberte **sítě**.  
    Hello **sítě** otevře se okno.

    ![Okno Virtuální sítě](./media/active-directory-domain-services-getting-started/virtual-network-select.png)
3. Na hello **virtuální sítě** karty, vyberte hello virtuální síť, ve kterém jste povolili službu Azure Active Directory Domain Services tooview jeho vlastnosti.
4. Klikněte na tlačítko hello **konfigurace** kartě.

    ![Okno Virtuální sítě](./media/active-directory-domain-services-getting-started/virtual-network-configure-tab.png)
5. V hello **servery DNS** zadejte obě hello IP adresy, které byly zobrazeny v hello **Domain Services** část hello **konfigurace** svůj adresář.
6. Klikněte na tlačítko toosave hello nastavení serveru DNS této virtuální sítě, v podokně úloh hello v hello dolní části okna hello **Uložit**.

   ![Aktualizovat nastavení serveru DNS hello hello virtuální sítě](./media/active-directory-domain-services-getting-started/update-dns.png)

> [!NOTE]
>  Virtuální počítače v síti hello získat nové nastavení DNS hello pouze po restartu. Chcete-li nastavení DNS tooget hello aktualizovat hned, aktivovat restartování hello portál, prostředí PowerShell nebo hello rozhraní příkazového řádku.
>
>

## <a name="next-steps"></a>Další kroky
Úloha 5: [povolit tooAzure synchronizace hesla Active Directory Domain Services](active-directory-ds-getting-started-password-sync.md)
