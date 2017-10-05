---
title: "Zabezpečení PaaS webové a mobilní aplikace pomocí Azure App Services | Microsoft Docs"
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
ms.openlocfilehash: 0738522250f1863c9584936a2d2b2c7a0a823c8c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="securing-paas-web-and-mobile-applications-using-azure-app-services"></a>Zabezpečení PaaS webové a mobilní aplikace pomocí Azure App Services

V tomto článku probereme kolekce [Azure App Services](https://azure.microsoft.com/services/app-service/) osvědčené postupy zabezpečení pro zabezpečení vašich PaaS webové a mobilní aplikace. Tyto doporučené postupy jsou odvozeny od našich zkušeností s Azure a prostředí zákazníků jako sami.

## <a name="azure-app-services"></a>Azure App Services
[Azure App Services](../app-service/app-service-value-prop-what-is.md) je PaaS, nabídka, která umožňuje vytvářet webových a mobilních aplikací pro všechny platformy a zařízení a připojte se k datům kdekoliv a v cloudu nebo místně. Aplikační služby zahrnuje webové a mobilní funkce, které byly dříve nabízeli samostatně jako weby Azure a Azure Mobile Services. Obsahuje také nové možnosti pro automatizaci obchodních procesů a hostování cloudových rozhraní API. Jako jediná integrovaná služba App Services přináší bohatou sadu funkcí pro webové, mobilní a integrační scénáře.

Další informace najdete v tématu Přehled na [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) a [webové aplikace](../app-service-web/app-service-web-overview.md).

## <a name="best-practices"></a>Osvědčené postupy

Při používání aplikační služby, dodržujte tyto doporučené postupy pro:

- [Ověřování pomocí služby Azure Active Directory (AD)](../app-service-web/web-sites-authentication-authorization.md#authenticate-through-azure-active-directory). App Services poskytuje službu OAuth 2.0 pro zprostředkovatele identity. OAuth 2.0 se zaměřuje na jednoduchost vývojáře klienta při současném poskytování toky konkrétní autorizace pro webové aplikace, aplikací klasické pracovní plochy a mobilních telefonů. Azure AD umožňují autorizovat přístup k mobilní a webových aplikací pomocí OAuth 2.0.
- Omezení přístupu na základě potřeba znát a principy zabezpečení nejnižší oprávnění. Omezení přístupu je nutné pro organizace, které chcete vynutit zásady zabezpečení pro přístup k datům. Řízení přístupu na základě rolí (RBAC) slouží k přiřazení oprávnění pro uživatele, skupiny a aplikace v určité oboru. Další informace o uživatelům udělen přístup k aplikacím, najdete v části [Začínáme se správou přístupu](../active-directory/role-based-access-control-what-is.md).
- Chrání vaše klíče. Nezávisle na tom, jak dobrý zabezpečení je při ztrátě klíče pro předplatné. Azure Key Vault pomáhá chránit kryptografické klíče a tajné klíče používané cloudovými aplikacemi a službami. Pomocí Key Vault můžete šifrovat klíče a tajné klíče (např. ověřovací klíče, klíče účtu úložiště, šifrovací klíče dat, soubory PFX a hesla) pomocí klíčů chráněných moduly hardwarového zabezpečení (HSM). Pro zvýšené bezpečí můžete klíče importovat nebo generovat v modulech HSM. V tématu [Azure Key Vault](../key-vault/key-vault-whatis.md) Další informace. Key Vault můžete také spravovat certifikáty protokolu TLS s automatického obnovení.
- Omezte příchozí zdrojové IP adresy. [Aplikace služby prostředí](../app-service-web/app-service-app-service-environment-intro.md) má funkce integrace virtuální sítě, která pomáhá omezit příchozí zdrojové IP adresy pomocí skupin zabezpečení sítě (Nsg). Pokud jste obeznámeni s virtuální sítí Azure (virtuální sítě), toto je funkce, která umožňuje umístit mnoho vašich prostředků Azure v Internetu jiných výrobců, směrovatelné síti, která můžete řídit přístup ke. Další informace najdete v tématu [aplikace integrovat Azure Virtual Network](../app-service-web/web-sites-integrate-with-vnet.md).

## <a name="next-steps"></a>Další kroky
Tento článek seznámili kolekce App Services osvědčené postupy zabezpečení pro zabezpečení vašich PaaS webové a mobilní aplikace. Další informace o zabezpečení vašich PaaS nasazení najdete v tématu:

- [Zabezpečení nasazení PaaS](security-paas-deployments.md)
- [Zabezpečení PaaS webové a mobilní aplikace pomocí Azure SQL Database a SQL Data Warehouse](security-paas-applications-using-sql.md)
