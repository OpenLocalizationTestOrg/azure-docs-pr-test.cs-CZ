---
title: "aaaSecuring PaaS webové a mobilní aplikace pomocí Azure App Services | Microsoft Docs"
description: " Další informace o Azure App Services osvědčené postupy zabezpečení pro zabezpečení vašich PaaS webové a mobilní aplikace. "
services: security
documentationcenter: na
author: techlake
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: terrylan
ms.openlocfilehash: 68a741c668efe2157cff114b649e0e287ef196f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="securing-paas-web-and-mobile-applications-using-azure-app-services"></a>Zabezpečení PaaS webové a mobilní aplikace pomocí Azure App Services

V tomto článku probereme kolekce [Azure App Services](https://azure.microsoft.com/services/app-service/) osvědčené postupy zabezpečení pro zabezpečení vašich PaaS webové a mobilní aplikace. Tyto doporučené postupy jsou odvozeny od našich zkušeností s Azure a hello prostředí zákazníků, jako sami.

## <a name="azure-app-services"></a>Azure App Services
[Azure App Services](../app-service/app-service-value-prop-what-is.md) je nabídka PaaS, který vám umožní vytvořit webové a mobilní aplikace pro všechny platformy a zařízení a připojit toodata kdekoliv a v hello cloudu nebo místně. Aplikační služby zahrnuje hello webové a mobilní funkce, které byly dříve nabízeli samostatně jako weby Azure a Azure Mobile Services. Obsahuje také nové možnosti pro automatizaci obchodních procesů a hostování cloudových rozhraní API. Jako jediná integrovaná služba App Services přináší bohatou sadu funkcí tooweb, mobilní a integrační scénáře.

toolearn více, najdete v přehledech [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) a [webové aplikace](../app-service-web/app-service-web-overview.md).

## <a name="best-practices"></a>Osvědčené postupy

Při používání aplikační služby, dodržujte tyto doporučené postupy pro:

- [Ověřování pomocí služby Azure Active Directory (AD)](../app-service-web/web-sites-authentication-authorization.md#authenticate-through-azure-active-directory). App Services poskytuje službu OAuth 2.0 pro zprostředkovatele identity. OAuth 2.0 se zaměřuje na jednoduchost vývojáře klienta při současném poskytování toky konkrétní autorizace pro webové aplikace, aplikací klasické pracovní plochy a mobilních telefonů. Azure AD používá tooenable OAuth 2.0 je tooauthorize přístup toomobile a webové aplikace.
- Omezení přístupu na základě hello nutné tooknow a principy zabezpečení nejnižší oprávnění. Omezení přístupu je nutné pro organizace, které mají tooenforce zásady zabezpečení pro přístup k datům. Řízení přístupu na základě rolí (RBAC) může být použité tooassign oprávnění toousers, skupinám a aplikacím v určitých oboru. toolearn Další informace o udělení uživatelé přistupovat k tooapplications, najdete v části [Začínáme se správou přístupu](../active-directory/role-based-access-control-what-is.md).
- Chrání vaše klíče. Nezávisle na tom, jak dobrý zabezpečení je při ztrátě klíče pro předplatné. Azure Key Vault pomáhá chránit kryptografické klíče a tajné klíče používané cloudovými aplikacemi a službami. Pomocí Key Vault můžete šifrovat klíče a tajné klíče (např. ověřovací klíče, klíče účtu úložiště, šifrovací klíče dat, soubory PFX a hesla) pomocí klíčů chráněných moduly hardwarového zabezpečení (HSM). Pro zvýšené bezpečí můžete klíče importovat nebo generovat v modulech HSM. V tématu [Azure Key Vault](../key-vault/key-vault-whatis.md) toolearn Další. Můžete taky Key Vault toomanage certifikáty protokolu TLS s automatického obnovení.
- Omezte příchozí zdrojové IP adresy. [Aplikace služby prostředí](../app-service-web/app-service-app-service-environment-intro.md) má funkce integrace virtuální sítě, která pomáhá omezit příchozí zdrojové IP adresy pomocí skupin zabezpečení sítě (Nsg). Pokud jste obeznámeni s virtuální sítí Azure (virtuální sítě), je to funkci, která vám umožní tooplace, které mnoho vašich prostředků Azure v Internetu jiných výrobců, směrovatelné síti, která můžete řídit přístup k. Další, najdete v části toolearn [aplikace integrovat Azure Virtual Network](../app-service-web/web-sites-integrate-with-vnet.md).

## <a name="next-steps"></a>Další kroky
Tento článek se zavedl tooa kolekce App Services osvědčené postupy zabezpečení pro zabezpečení vašich PaaS webové a mobilní aplikace. Další informace o zabezpečení vašeho nasazení PaaS toolearn najdete v části:

- [Zabezpečení nasazení PaaS](security-paas-deployments.md)
- [Zabezpečení PaaS webové a mobilní aplikace pomocí Azure SQL Database a SQL Data Warehouse](security-paas-applications-using-sql.md)
