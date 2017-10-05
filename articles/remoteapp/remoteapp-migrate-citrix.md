---
title: Migrace z Azure Remoteappu k Citrix XenApp Essentials | Microsoft Docs
description: Migrace z Azure Remoteappu k Citrix XenApp Essentials
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
ms.openlocfilehash: fcd96a466d1c0dad17d7012308281ef868463b19
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="migrate-from-azure-remoteapp-to-citrix-xenapp-essentials"></a>Migrace z Azure Remoteappu k Citrix XenApp Essentials

Pokud používáte Azure Remoteappu a chcete provést migraci na Citrix XenApp Essentials, existuje několik požadavků na mějte na paměti. Nejdřív přečíst společnosti Citrix [průvodce krok za krokem technického nasazení pro Citrix XenApp Essentials](https://docs.citrix.com/content/dam/docs/en-us/citrix-cloud/downloads/xenapp-essentials-deployment-guide.pdf) a jeho [online technické knihovně](http://docs.citrix.com/en-us/citrix-cloud/xenapp-and-xendesktop-service/xenapp-essentials.html). 

## <a name="prerequisite-steps-for-migration"></a>Požadované kroky pro migraci

1. Vytvořit novou virtuální síť, nebo zjistit, které virtuální síť Azure v správce Azure Resource Manager do kterého budete nasazovat Citrix XenApp Essentials. Azure RemoteApp používá portál Azure classic; Citrix XenApp Essentials podporuje pouze správce Azure Resource Manager.  
2. Zajistěte, aby virtuální síť, kterou jste vybrali síťového přístupu k řadiči domény, protože Citrix podporuje pouze hybridní nasazení. Pokud používáte cloudové nasazení služby Azure RemoteApp, zajistěte, aby virtuální síť síťového přístupu k řadiči domény služby Active Directory. Můžete také použít Azure Active Directory Domain Services (Azure AD DS). 
3. Ujistěte se, že DNS je správně nakonfigurované pro virtuální síť, že připojení k doméně je úspěšné na první pokus. Můžete vytvořit virtuální počítač (VM) ve virtuální síti, které jste vybrali a proveďte ruční domény připojit k ověření, že DNS a domény připojení funguje podle očekávání. Tím se zajistí můžete úspěšné při prvním nasazení Citrix XenApp Essentials. 
4. V případě potřeby vytvořte virtuální síť partnerský vztah mezi klasického portálu virtuální síť Azure, které používáte s Azure RemoteApp a virtuální sítí Azure Resource Manager. Tento proces partnerského vztahu funguje v případě, že dvě sítě jsou umístěny ve stejné oblasti. Pokud ne, použijte pro připojení virtuální sítě pro sítě site-to-site VPN. 
5. V případě potřeby číst [migrace dat do a z Azure Remoteappu](remoteapp-migrate.md). 
6. Aktualizovat stávající image Azure Remoteappu zahrnout komponentu Citrix VDA (pokynů, najdete v dokumentaci systému Citrix). 
7. Přejděte do Azure Marketplace a zahájit nasazení Citrix XenApp Essentials.

## <a name="other-considerations"></a>Další důležité informace

Mějte na paměti následující další aspekty při migraci:
- Citrix XenApp Essentials podporuje pouze hybridní nasazení. Jinými slovy vyžaduje přístup k síti do řadiče domény, aby bylo možné provést připojení k doméně. Pokud používáte cloudové nasazení služby Azure RemoteApp, buď pomocí služby Azure AD DS, nebo zajistěte, aby virtuální síti přístup ke službě Active Directory pro připojení k doméně. 
- Další postupy pro přesun dat uživatele do Citrix XenApp Essentials najdete v tématu [migrace dat do a z Azure Remoteappu](remoteapp-migrate.md). 
- Citrix XenApp Essentials podporuje pouze účty služby Active Directory. Účty Microsoft (jako jsou outlook.com, msn.com nebo hotmail.com) nejsou podporovány. 

## <a name="citrix-xenapp-essentials-billing"></a>Citrix XenApp Essentials fakturace

Úplné podrobnosti o cenách najdete v tématu [– nejčastější dotazy](https://www.citrix.com/global-partners/microsoft/resources/xenapp-essentials-faq.html#tab-30699) a [článek s přehledem Citrix](https://www.citrix.com/global-partners/microsoft/remote-app.html). Existují tři fakturace součástí Citrix XenApp Essentials:

- Služba zdarma Citrix, což je 12 na uživatele za měsíc. Podobně jako všechny nákupy Azure Marketplace se fakturuje způsobu platby spojené s předplatným Azure. Pro zákazníky, Enterprise Agreement (EA) nelze použít peněžní kredity Azure. 
- Vzdálená Data Services (RDS) licencí pro klientský přístup (CAL). V současné době můžete zakoupit vzdáleného přístupu poplatek, který je instalován s platebních Citrix XenApp Essentials pro 6,25 $. Pokud jste zákazník EA, můžete k platbě za to peněžní kredity Azure. Pokud chcete používat vaše stávající licence VP CAL vázané, obraťte se na nás na adrese [ arainfo@microsoft.com ](mailto:arainfo@microsoft.com), takže jsme můžete použít na vašem vyúčtování. 
- Azure výpočetního prostředí a úložiště. Toto je náklady na úložiště Azure a využívat výpočetní spotřeby pro virtuální počítače. Pamatujte na ceny při výběru vaší velikosti a uživatel hustotu virtuálních počítačů. Pokud jste zákazník EA, můžete k platbě za to peněžní kredity Azure.

Pokud máte dotazy, můžete:
- E-mailu nás na adrese [ arainfo@microsoft.com ](mailto:arainfo@microsoft.com).
- [Požádejte podporu Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Začněte tím, že [otevírání případu podpory Azure pro](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) usnadní požadovaných součástí kroky 1 až 5. Kroky 6 až 7 kontaktujte Citrix otevřením lístku podpory v rámci portálu pro správu systému Citrix. 
