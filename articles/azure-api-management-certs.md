---
title: "aaaUpload certifikát správy rozhraní API služby Azure | Microsoft Docs"
description: "Zjistěte, jak tooupload athe rozhraní API pro správu certifikátů pro hello portálu Azure Classic."
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
ms.openlocfilehash: 8294d7131cfb01dba664bd4fd04b6fc22c1e93ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upload-an-azure-management-api-management-certificate"></a>Nahrajte certifikát pro správu Azure rozhraní API pro správu
Certifikáty pro správu povolit tooauthenticate s modelem nasazení classic hello poskytovaný platformou Azure. Mnoho programy a nástroje (například Visual Studio nebo hello Azure SDK) používat tyto certifikáty tooautomate konfigurace a nasazení různých služeb Azure. 

> [!WARNING]
> Dej si pozor! Tyto typy certifikátů povolit všem uživatelům, kteří s nimi ověřuje toomanage hello předplatného jsou přidruženy.
>
>

Pokud vás zajímají další informace o Azure certifikáty (včetně vytváření certifikát podepsaný svým držitelem), najdete v části [Přehled certifikátů pro Azure Cloud Services](cloud-services/cloud-services-certs-create.md#what-are-management-certificates).

Můžete také použít [Azure Active Directory](https://azure.microsoft.com/en-us/services/active-directory/) kód klienta tooauthenticate za účelem automatizace.

## <a name="upload-a-management-certificate"></a>Nahrání certifikátu pro správu
Jakmile je certifikát správy vytvořený, (soubor .cer s pouze veřejný klíč hello) nahrajte ho do portálu hello. Pokud je k dispozici v portálu hello hello certifikát, každý, kdo má odpovídající certifikátu (privátní klíč) připojovat prostřednictvím hello rozhraní API pro správu a přístup k prostředkům hello hello přidruženého odběru.

1. Přihlaste se toohello [portál Azure](http://portal.azure.com).
2. Klikněte na tlačítko **další služby** v seznamu služeb Azure dolní hello, pak vyberte **odběry** v hello _Obecné_ skupinu služeb.

    ![Předplatné nabídky](./media/azure-api-management-certs/subscriptions_menu.png)

3. Zajistěte, aby tooselect hello správné předplatné, které chcete tooassociate certifikátem hello.     
4. Po výběru správné předplatné hello stiskněte **certifikáty pro správu** v hello _nastavení_ skupiny.

    ![Nastavení](./media/azure-api-management-certs/mgmtcerts_menu.png)

5. Stiskněte klávesu hello **nahrát** tlačítko.

    ![Nahrát na stránce certifikáty](./media/azure-api-management-certs/certificates_page.png)
6. Vyplňte informace hello dialogové okno a stiskněte klávesu **nahrát**.

    ![Nastavení](./media/azure-api-management-certs/certificate_details.png)

## <a name="next-steps"></a>Další kroky
Teď, když máte certifikát pro správu přidružené předplatné, můžete (po instalaci hello certifikát, se místně) prostřednictvím kódu programu připojit toohello [modelu nasazení classic REST API](https://msdn.microsoft.com/library/azure/mt420159.aspx) a automatizovat Hello různé prostředky Azure, které jsou také přidružené tomuto předplatnému.
