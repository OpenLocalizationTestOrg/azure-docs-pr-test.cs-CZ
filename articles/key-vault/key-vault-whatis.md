---
title: aaaWhat je Azure Key Vault? | Dokumentace Microsoftu
description: "Azure Key Vault pomáhá chránit kryptografické klíče a tajné klíče používané cloudovými aplikacemi a službami. Pomocí Azure Key Vault můžou zákazníci šifrovat klíče a tajné klíče (např. ověřovací klíče, klíče účtu úložiště, šifrovací klíče dat, soubory PFX a hesla) pomocí klíčů chráněných moduly hardwarového zabezpečení (HSM)."
services: key-vault
documentationcenter: 
author: cabailey
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: e759df6f-0638-43b1-98ed-30b3913f9b82
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/19/2017
ms.author: cabailey
ms.openlocfilehash: 296fcce03658b96b84afab299b73681bbe8ac9fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-key-vault"></a>Co je Azure Key Vault?
Azure Key Vault je dostupný ve většině oblastí. Další informace najdete v tématu hello [stránce s cenami Key Vault](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Úvod
Azure Key Vault pomáhá chránit kryptografické klíče a tajné klíče používané cloudovými aplikacemi a službami. Pomocí Key Vault můžete šifrovat klíče a tajné klíče (např. ověřovací klíče, klíče účtu úložiště, šifrovací klíče dat, soubory PFX a hesla) pomocí klíčů chráněných moduly hardwarového zabezpečení (HSM). Pro zvýšené bezpečí můžete klíče importovat nebo generovat v modulech HSM. Pokud se rozhodnete toodo se procesy Microsoft vaše klíče v FIPS 140-2 úrovně 2 ověřené HSM (hardware a firmware).  

Key Vault zjednodušuje proces správy klíčů hello a umožní vám toomaintain kontrolu nad klíči, které k přístupu a šifrování dat. Vývojáři můžou vytvořit klíče pro vývoj a testování v minutách a potom je bez problémů migrovat tooproduction klíče. Správce zabezpečení můžete udělit (a odvolávat) oprávnění tookeys, podle potřeby.

Hello použijte následující tabulku toobetter pochopit, jak může Key Vault pomoci toomeet hello potřebám vývojářů a správců zabezpečení.

| Role | Popis problému | Vyřešeno Azure Key Vault |
| --- | --- | --- |
| Vývojář aplikace Azure |"Chci, aby toowrite aplikace pro Azure, která používá klíče pro podepisování a šifrování, které má tyto klíče toobe externí z mé aplikace tak, aby je vhodný pro aplikaci, která je geograficky distribuovanou hello řešení. <br/><br/>Chci také toobe tyto klíče a tajné klíče chráněné, bez nutnosti toowrite hello kód sám. Taky chci tyto klíče a tajné klíče toobe snadno pro mě nejlepší toouse v aplikacích a poskytovaly optimální výkon." |√ Klíče jsou uložené v trezoru, a když je potřeba, volají se identifikátorem URI.<br/><br/> √ Klíče jsou chráněné systémem Azure pomocí standardních algoritmů, délek klíčů a modulů hardwarového zabezpečení (HSM).<br/><br/> √ Klíče se zpracovávají v modulech HSM, které jsou umístěny v hello stejné datových centrech Azure jako aplikace hello. To poskytuje vyšší spolehlivost a menší latenci než pokud hello klíče jsou umístěny v samostatných umístění, jako je například místní. |
| Vývojář softwaru jako služby (SaaS) |"Nechci hello odpovědnost nebo potenciální odpovědnosti pro moje zákazníků klientské klíče a tajné klíče. <br/><br/>I mají zákazníci tooown hello a spravovali svoje klíče, aby mi umožní soustředit se na to, co umím nejlépe, který poskytuje funkce softwaru hello jádra." |√ Zákazníci můžou svoje klíče importovat do systému Azure a spravovat je. Pokud některá aplikace SaaS potřebuje tooperform kryptografické operace pomocí klíčů zákazníků, Key Vault nepodporuje tyto operace jménem aplikace hello. aplikace Hello klíče hello zákazníků nezná. |
| Ředitel pro bezpečnost |"Chci, aby tooknow, že naše aplikace v souladu s moduly hardwarového zabezpečení FIPS 140-2 úroveň 2 pro bezpečnou správu klíčů. <br/><br/>I má toomake jistotu, že Moje organizace má kontrolu nad hello životního cyklu klíče a můžete sledovat použití klíče. <br/><br/>A přestože používáme více služeb Azure a prostředky, chcete toomanage hello klíče z jednoho umístění v Azure." |√ Moduly HMS jsou ověřené podle standardu FIPS 140-2 Level 2.<br/><br/>√ Key Vault je navržený tak, aby společnost Microsoft vaše klíče neznala ani neextrahovala.<br/><br/>√ Používání klíčů se protokoluje téměř v reálném čase.<br/><br/>√ hello trezor poskytuje jednotné rozhraní – bez ohledu na to, kolik trezorů v Azure máte, které oblasti jejich podporu a které aplikace je používají. |

Trezory klíčů může vytvářet a používat každý, kdo má předplatné Azure. Přestože je Key Vault přínosný pro vývojáře a správce zabezpečení, může ho implementovat a spravovat libovolný správce organizace, který v organizaci spravuje ostatní služby Azure. Tento správce by například přihlásit pomocí předplatného Azure, vytvořit trezor pro organizaci hello v které toostore klíče a potom mít na starost provozní úlohy, jako například:

* Vytvoření nebo import klíče nebo tajného klíče
* Odvolání nebo odstranění klíče nebo tajného klíče
* Ověření uživatelů a aplikací tooaccess hello trezoru klíčů, aby se potom mohli spravovat nebo používat jeho klíče a tajné klíče
* Konfigurace používání klíčů (například podepisování nebo šifrování)
* Sledování používání klíčů

Tento správce by potom poskytne vývojářům identifikátory URI toocall ze svých aplikací a jejich zabezpečení správce poskytnout informace o protokolování používání klíčů. 

   ![Přehled Azure Key Vault][1]

Vývojáři taky mohou spravovat klíče hello přímo, pomocí rozhraní API. Další informace najdete v tématu [hello Příručka pro vývojáře Key Vault](key-vault-developers-guide.md).

## <a name="next-steps"></a>Další kroky
Úvodní kurz pro správce najdete v tématu [Začínáme s Azure Key Vault](key-vault-get-started.md).

Další informace o protokolování využití Key Vault najdete v tématu[Protokolování Azure Key Vault](key-vault-logging.md).

Další informace o používání klíčů a tajných klíčů se službou Azure Key Vault najdete v tématu [Informace o klíčích, tajných klíčích a certifikátech](https://msdn.microsoft.com/library/azure/dn903623\(v=azure.1\).aspx).

<!--Image references-->
[1]: ./media/key-vault-whatis/AzureKeyVault_overview.png
