---
title: "Nahrát na server certifikát rozhraní API správy Azure | Microsoft Docs"
description: "Naučte se nahrát athe rozhraní API pro správu certifikátů pro portál Azure Classic."
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 1b813833-39c8-46be-8666-fd0960cfbf04
ms.service: na
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: adegeo
ms.openlocfilehash: 9dc438e927acd9aef38f06807fabf3dda9b021c9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="upload-an-azure-management-api-management-certificate"></a>Nahrajte certifikát pro správu Azure rozhraní API pro správu
Certifikáty pro správu umožňují ověření pomocí modelu nasazení classic poskytovaný platformou Azure. Mnoho programy a nástroje (například Visual Studio nebo sadu Azure SDK) použít tyto certifikáty k automatizaci konfigurace a nasazení různých služeb Azure. 

> [!WARNING]
> Dej si pozor! Tyto typy certifikátů, povolí všem uživatelům, kteří se mají spravovat předplatné, které jsou přidruženy ověřuje.
>
>

Pokud vás zajímají další informace o Azure certifikáty (včetně vytváření certifikát podepsaný svým držitelem), najdete v části [Přehled certifikátů pro Azure Cloud Services](cloud-services/cloud-services-certs-create.md#what-are-management-certificates).

Můžete také použít [Azure Active Directory](https://azure.microsoft.com/en-us/services/active-directory/) k ověření kódu klienta pro účely automatizace.

## <a name="upload-a-management-certificate"></a>Nahrání certifikátu pro správu
Jakmile je certifikát správy vytvořený, (soubor .cer s pouze veřejný klíč) nahrajte ho do portálu. Je-li certifikát k dispozici na portálu, každý, kdo má odpovídající certifikátu (privátní klíč) můžete připojit přes rozhraní API pro správu a přístup k prostředkům pro přidružené předplatné.

1. Přihlaste se k portálu [Azure Portal](http://portal.azure.com).
2. Klikněte na tlačítko **další služby** v seznamu dolní služby Azure, pak vyberte **odběry** v _Obecné_ skupinu služeb.

    ![Předplatné nabídky](./media/azure-api-management-certs/subscriptions_menu.png)

3. Ujistěte se, že vyberte správné předplatné, který chcete přidružit k certifikátu.     
4. Po výběru správné předplatné, stiskněte klávesu **certifikáty pro správu** v _nastavení_ skupiny.

    ![Nastavení](./media/azure-api-management-certs/mgmtcerts_menu.png)

5. Stiskněte **nahrát** tlačítko.

    ![Nahrát na stránce certifikáty](./media/azure-api-management-certs/certificates_page.png)
6. Vyplňte informace dialogové okno a stiskněte klávesu **nahrát**.

    ![Nastavení](./media/azure-api-management-certs/certificate_details.png)

## <a name="next-steps"></a>Další kroky
Teď, když máte certifikát pro správu přidružené předplatné, můžete (po instalaci odpovídající certifikátu místně) prostřednictvím kódu programu připojit k [modelu nasazení classic REST API](https://msdn.microsoft.com/library/azure/mt420159.aspx) a automatizovat různé prostředky Azure, které jsou také přidružené tomuto předplatnému.
