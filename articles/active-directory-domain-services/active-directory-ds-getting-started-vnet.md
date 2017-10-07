---
title: "Služba Azure Active Directory Domain Services: Vytvoření nebo výběr virtuální sítě | Dokumentace Microsoftu"
description: "Povolit Azure Active Directory Domain Services pomocí hello portál Azure classic"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 13ab1608-e3d8-40de-9f7b-9b5b42199af4
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/28/2017
ms.author: maheshu
ms.openlocfilehash: 212c741b20e846742d94f70342c4263ce8984153
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-or-select-a-virtual-network-for-azure-active-directory-domain-services"></a>Vytvoření nebo výběr virtuální sítě pro Azure Active Directory Domain Services
## <a name="before-you-begin"></a>Než začnete
Odkazovat příliš[požadavky sítě pro Azure Active Directory Domain Services](active-directory-ds-networking.md).

## <a name="task-2-create-an-azure-virtual-network"></a>Úloha 2: Vytvořte virtuální síť Azure
Další úlohou konfigurace Hello je toocreate virtuální sítě Azure a podsítě v něm. V této podsíti v rámci své virtuální sítě povolíte službu Azure Active Directory Domain Services. Pokud máte existující virtuální síť, která si přejete toouse, můžete tento krok přeskočit.

> [!NOTE]
> Ujistěte se, že hello virtuální síť Azure, vytvořte nebo zvolte, toouse s Azure Active Directory Domain Services patří tooan oblast Azure, která je podporována službou Azure Active Directory Domain Services. tooascertain hello oblastí Azure, ve kterých je k dispozici Azure Active Directory Domain Services najdete v tématu [služby Azure podle oblasti](https://azure.microsoft.com/regions/#services/).
>
>Poznamenejte si název hello tooensure hello virtuální sítě vyberte správnou virtuální síť hello, pokud povolíte Azure Active Directory Domain Services v dalším kroku konfigurace.


toocreate Azure virtuální sítě, ve kterém chcete tooenable Azure Active Directory Domain Services, postupujte podle těchto pokynů konfigurace:

1. Přejděte toohello [portál Azure classic](https://manage.windowsazure.com).
2. V levém podokně hello vyberte **sítě**.

    ![Uzel sítí](./media/active-directory-domain-services-getting-started/networks-node.png)  
    Hello **virtuální sítě** otevře se okno.
3. V podokně úloh v dolní části hello okna hello hello, klikněte na tlačítko **nový**.

    ![Okno Virtuální sítě](./media/active-directory-domain-services-getting-started/virtual-networks.png)
4. Klikněte na **Síťové služby** a vyberte možnost **Virtuální síť**.

    ![Virtuální síť – rychle vytvořit](./media/active-directory-domain-services-getting-started/virtual-network-quickcreate.png)
5. toocreate virtuální síť, klikněte na tlačítko **rychle vytvořit**.

6. Zadejte **název** pro svoji virtuální sítě a zvažte provedení hello následující:
    * Můžete zvolit tooconfigure **adresní prostor** nebo **maximální počet virtuálních počítačů** pro tuto síť.
    * Můžete ponechat hello **DNS server** nastavení jako **žádné** teď. Po povolení Azure Active Directory Domain Services, můžete aktualizovat nastavení hello.
7. V hello **umístění** rozevíracího seznamu vyberte podporovanou oblast Azure.  
    tooascertain hello oblastí Azure, ve kterých je k dispozici Azure Active Directory Domain Services najdete v tématu [služby Azure podle oblasti](https://azure.microsoft.com/regions/#services/).
8. toocreate vaší virtuální síti, klikněte na tlačítko **vytvořit virtuální síť**.

    ![Vytvoření virtuální sítě pro Azure Active Directory Domain Services](./media/active-directory-domain-services-getting-started/create-vnet.png)
9. Po vytvoření virtuální sítě, vyberte název hello hello virtuální sítě a pak klikněte na tlačítko hello **konfigurace** kartě.

    ![Vytvoření podsítě](./media/active-directory-domain-services-getting-started/create-vnet-properties.png)
10. V části **adresní prostory virtuální sítě**, klikněte na tlačítko **přidat podsíť**a pak zadejte podsíť s názvem hello **AaddsSubnet**.

    ![Vytvoření podsítě pro Azure Active Directory Domain Services](./media/active-directory-domain-services-getting-started/create-vnet-add-subnet.png)

11. toocreate hello podsítě, klikněte na tlačítko **Uložit**.


## <a name="next-step"></a>Další krok
[Úloha 3: Povolení služby Azure Active Directory Domain Services](active-directory-ds-getting-started-enableaadds.md)
