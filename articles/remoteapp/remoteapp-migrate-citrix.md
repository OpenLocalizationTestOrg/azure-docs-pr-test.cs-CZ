---
title: aaaMigrate z Azure Remoteappu tooCitrix XenApp Essentials | Microsoft Docs
description: Jak toomigrate z Azure Remoteappu tooCitrix XenApp Essentials
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 695a8165-3454-4855-8e21-f2eb2c61201b
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: aa3ce28bc5a86d5b1dd3408196d51935395f55c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-from-azure-remoteapp-toocitrix-xenapp-essentials"></a>Migrace z Azure Remoteappu tooCitrix XenApp Essentials

Pokud používáte Azure Remoteappu a chcete toomigrate tooCitrix XenApp Essentials, jsou na paměti několik tookeep požadavky. Nejdřív přečíst společnosti Citrix [průvodce krok za krokem technického nasazení pro Citrix XenApp Essentials](https://docs.citrix.com/content/dam/docs/en-us/citrix-cloud/downloads/xenapp-essentials-deployment-guide.pdf) a jeho [online technické knihovně](http://docs.citrix.com/en-us/citrix-cloud/xenapp-and-xendesktop-service/xenapp-essentials.html). 

## <a name="prerequisite-steps-for-migration"></a>Požadované kroky pro migraci

1. Vytvořit novou virtuální síť, nebo zjistit, které virtuální síť Azure v správce Azure Resource Manager do kterého budete nasazovat Citrix XenApp Essentials. Azure RemoteApp používá hello portál Azure classic; Citrix XenApp Essentials podporuje pouze správce Azure Resource Manager.  
2. Zajistěte, aby byl hello virtuální síti, které jste vybrali sítě řadič domény tooyour přístup, protože Citrix podporuje pouze hybridní nasazení. Pokud používáte cloudové nasazení služby Azure RemoteApp, zajistěte, aby virtuální síť sítě řadič domény služby Active Directory tooan přístup. Můžete také použít Azure Active Directory Domain Services (Azure AD DS). 
3. Ujistěte se, že hello DNS správně nakonfigurován pro hello virtuální sítě, že připojení k doméně je úspěšné na první pokus. Můžete vytvořit virtuální počítač (VM) v hello virtuální sítě, které jste vybrali a provádět tooverify ruční domény připojení k této hello DNS a připojení k doméně funguje podle očekávání. Tím se zajistí jste úspěšně hello prvním nasazení Citrix XenApp Essentials. 
4. V případě potřeby vytvořte virtuální síť partnerský vztah mezi klasického portálu virtuální síť Azure, které používáte s Azure RemoteApp a virtuální sítí Azure Resource Manager. Tento proces partnerského vztahu funguje v případě hello dvě sítě jsou umístěny v hello stejné oblasti. Pokud ne, používají pro síť site-to-site VPN tooconnect hello virtuální sítě. 
5. V případě potřeby číst [jak toomigrate dat do a z Azure Remoteappu](remoteapp-migrate.md). 
6. Aktualizovat vaše stávající Azure RemoteApp image tooinclude hello Citrix VDA součást (pokyny najdete v tématu hello Citrix dokumentaci). 
7. Přejděte toohello Azure Marketplace a zahájit nasazení Citrix XenApp Essentials.

## <a name="other-considerations"></a>Další důležité informace

Uvědomte si, hello následující další aspekty při migraci:
- Citrix XenApp Essentials podporuje pouze hybridní nasazení. Jinými slovy vyžaduje přístup tooa řadiče domény v připojení k doméně tooperform pořadí. Pokud používáte cloudové nasazení služby Azure RemoteApp, buď pomocí služby Azure AD DS, nebo zajistěte, aby virtuální síti přístup tooActive adresář pro připojení k doméně. 
- jak zjistit, toomove uživatele data tooCitrix XenApp Essentials toolearn [jak toomigrate dat do a z Azure Remoteappu](remoteapp-migrate.md). 
- Citrix XenApp Essentials podporuje pouze účty služby Active Directory. Účty Microsoft (jako jsou outlook.com, msn.com nebo hotmail.com) nejsou podporovány. 

## <a name="citrix-xenapp-essentials-billing"></a>Citrix XenApp Essentials fakturace

Úplné podrobnosti o cenách najdete v tématu hello [– nejčastější dotazy](https://www.citrix.com/global-partners/microsoft/resources/xenapp-essentials-faq.html#tab-30699) a [článek s přehledem Citrix](https://www.citrix.com/global-partners/microsoft/remote-app.html). Existují tři fakturace součásti tooCitrix XenApp Essentials:

- Hello Citrix poplatek, což je 12 na uživatele za měsíc. Podobně jako všechny nákupy Azure Marketplace to je fakturovaná toohello platby spojené s předplatným Azure. Pro zákazníky, Enterprise Agreement (EA) nelze použít peněžní kredity Azure. 
- Vzdálená Data Services (RDS) licencí pro klientský přístup (CAL). V současné době můžete zakoupit hello vzdáleného přístupu poplatek zahrnutý v s hello Citrix XenApp Essentials platebních pro 6,25 $. Pokud jste zákazník EA, můžete pro tuto toopay peněžní kredity Azure. Pokud chcete toouse vaše stávající licence VP CAL vázané, kontaktujte nás na adrese [ arainfo@microsoft.com ](mailto:arainfo@microsoft.com), takže jsme můžete použít tento tooyour faktury. 
- Azure výpočetního prostředí a úložiště. Toto je hello náklady na úložiště Azure a využívat výpočetní energie pro virtuální počítače hello. Pamatujte na ceny při výběru vaší velikosti a uživatel hustotu virtuálních počítačů. Pokud jste zákazník EA, můžete pro tuto toopay peněžní kredity Azure.

Pokud máte dotazy, můžete:
- E-mailu nás na adrese [ arainfo@microsoft.com ](mailto:arainfo@microsoft.com).
- [Požádejte podporu Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Začněte tím, že [otevírání případu podpory Azure pro](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) toohelp s předpokladem kroky 1 až 5. Kroky 6 až 7 kontaktujte Citrix otevřením lístku podpory v rámci portálu pro správu hello Citrix. 
