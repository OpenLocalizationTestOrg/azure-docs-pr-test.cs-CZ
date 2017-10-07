---
title: "Hybridní identita: Porovnání nástrojů pro integraci adresáře | Dokumentace Microsoftu"
description: "Toto je stránka nabízí komplexní tabulku porovnávající hello různé nástroje integrace adresáře, které můžete použít pro integraci adresáře."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: 1e62a4bd-4d55-4609-895e-70131dedbf52
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 18ac0a0f58726eceb85510df516e8db71429b313
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="hybrid-identity-directory-integration-tools-comparison"></a>Hybridní identita: Porovnání nástrojů pro integraci adresáře
V průběhu let hello nástroje integrace adresáře hello zvětšil a vyvinuly.  Tento dokument je toohelp poskytují ucelený přehled těchto nástrojů a porovnání hello funkcí, které jsou v nich dostupné.

<!-- hello hardcoded link is a workaround for campaign ids not working in acom links-->

> [!NOTE]
> Azure AD Connect zahrnuje hello komponenty a funkce dřív vydané jako Dirsync a AAD Sync. Tyto nástroje jsou už vydán jednotlivě a všechna budoucí vylepšení budou zahrnuty do aktualizace tooAzure AD Connect, abyste vždycky věděli, kde tooget hello nejnovější funkce.
> 
> DirSync a Azure AD Sync jsou nyní zastaralé. Další informace najdete [tady](active-directory-aadconnect-dirsync-deprecated.md).
> 
> 

Použijte následující klíč pro každou z tabulek hello hello.

● = Nyní dostupné  
FR = budoucí verze  
PP = Public Preview  

## <a name="on-premises-toocloud-synchronization"></a>Místní tooCloud synchronizace
| Funkce | Azure Active Directory Connect | Služby synchronizace Azure Active Directory (AAD Sync) | Synchronizační nástroj služby Azure Active Directory (DirSync) | Forefront Identity Manager 2010 R2 (FIM) | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|:---:|
| Připojit toosingle místní doménové struktuře Active Directory |● |● |● |● |● |
| Připojit toomultiple místní doménových struktur AD |● |● | |● |● |
| Připojit toomultiple místní Exchange Orgs |● | | | | |
| Připojení adresáře LDAP místní toosingle |FR | | |● |● |
| Připojení adresáře LDAP místní toomultiple |FR | | |● |● |
| Připojit tooon místní AD a místní adresáře LDAP |FR | | |● |● |
| Připojit toocustom systémy (tj. SQL, Oracle, MySQL atd.) |FR | | |● |● |
| Synchronizace atributů definovaných zákazníkem (rozšíření adresáře) |● | | | | |
| Připojit tooon místním HR (tj, SAP, Oracle eBusiness, PeopleSoft) |FR | | |● |● |
| Podporuje pravidla synchronizace FIM a konektory pro zřizování tooon místní systémy. | | | |● |● |

## <a name="cloud-tooon-premises-synchronization"></a>Synchronizace místních tooOn cloudu
| Funkce | Azure Active Directory Connect | Služby synchronizace Azure Active Directory | Synchronizační nástroj služby Azure Active Directory (DirSync) | Forefront Identity Manager 2010 R2 (FIM) | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|:---:|
| Zpětný zápis zařízení |● | |● | | |
| Zpětný zápis atributů (pro hybridního nasazení Exchange) |● |● |● |● |● |
| Zpětný zápis skupin objektů |● | | | | |
| Zpětný zápis hesel (ze samoobslužného resetování hesla (SSPR) a změny hesla) |● |● | | | |

## <a name="authentication-feature-support"></a>Podpora funkce ověřování
| Funkce | Azure Active Directory Connect | Služby synchronizace Azure Active Directory | Synchronizační nástroj služby Azure Active Directory (DirSync) | Forefront Identity Manager 2010 R2 (FIM) | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|:---:|
| Synchronizace hesla pro jednu místní doménovou strukturu AD |● |● |● | | |
| Synchronizace hesla pro několik místních doménových struktur AD |● |● | | | |
| Jednotné přihlašování s federací |● |● |● |● |● |
| Zpětný zápis hesel (ze SSPR a změny hesla) |● |● | | | |

## <a name="set-up-and-installation"></a>Nastavení a instalace
| Funkce | Azure Active Directory Connect | Služby synchronizace Azure Active Directory | Synchronizační nástroj služby Azure Active Directory (DirSync) | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|
| Podporuje instalaci na řadič domény |● |● |● | |
| Podporuje instalaci pomocí SQL Express |● |● |● | |
| Snadný upgrade z nástroje DirSync |● | | | |
| Lokalizace správce UX tooWindows jazyků serveru |● |● |● | |
| Lokalizace koncového uživatele UX tooWindows jazyků serveru | | | |● |
| Podpora pro Windows Server 2008 a Windows Server 2008 R2 |● pro synchronizaci, ne pro federaci |● |● |● |
| Podpora pro Windows Server 2012 a Windows Server 2012 R2 |● |● |● |● |

## <a name="filtering-and-configuration"></a>Filtrování a konfigurace
| Funkce | Azure Active Directory Connect | Služby synchronizace Azure Active Directory | Synchronizační nástroj služby Azure Active Directory (DirSync) | Forefront Identity Manager 2010 R2 (FIM) | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|:---:|
| Filtrování v rámci domén a organizačních jednotek |● |● |● |● |● |
| Filtrování v rámci hodnot atributů objektů |● |● |● |● |● |
| Povolit minimální sadu atributů toobe synchronizovány (MinSync) |● |● | | | |
| Povolit toobe šablony jiný služeb pro toky atributů |● |● | | | |
| Povolení odebrání atributů z toku ze AD tooAzure AD |● |● | | | |
| Povolení upřesňujících úprav pro toky atributů |● |● | |● |● |

## <a name="next-steps"></a>Další kroky
Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).

