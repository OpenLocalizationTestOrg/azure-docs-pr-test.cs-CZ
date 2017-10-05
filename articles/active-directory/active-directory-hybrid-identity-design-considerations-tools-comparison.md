---
title: "Hybridní identita: Porovnání nástrojů pro integraci adresáře | Dokumentace Microsoftu"
description: "Tato stránka nabízí komplexní tabulku porovnávající různé nástroje integrace adresáře, které lze použít pro integraci adresáře."
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
ms.openlocfilehash: f87b5837c7a786c8b309a6a52e69b53c0ad4c9d8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="hybrid-identity-directory-integration-tools-comparison"></a>Hybridní identita: Porovnání nástrojů pro integraci adresáře
V průběhu let se nástroje pro integraci adresáře rozšířily a vyvinuly.  Tento dokument vám poskytne ucelený přehled těchto nástrojů a porovnání funkcí, které jsou v nich dostupné.

<!-- The hardcoded link is a workaround for campaign ids not working in acom links-->

> [!NOTE]
> Azure AD Connect zahrnuje komponenty a funkce dříve vydávané jako Dirsync a AAD Sync. Tyto nástroje se již nebudou vydávat jednotlivě a všechna budoucí vylepšení budou zahrnuta do aktualizací služby Azure AD Connect, abyste vždycky věděli, kde sehnat ty nejnovější funkce.
> 
> DirSync a Azure AD Sync jsou nyní zastaralé. Další informace najdete [tady](active-directory-aadconnect-dirsync-deprecated.md).
> 
> 

U jednotlivých tabulek použijte následující klíč.

● = Nyní dostupné  
FR = budoucí verze  
PP = Public Preview  

## <a name="on-premises-to-cloud-synchronization"></a>Synchronizace z místního prostředí do cloudu
| Funkce | Azure Active Directory Connect | Služby synchronizace Azure Active Directory (AAD Sync) | Synchronizační nástroj služby Azure Active Directory (DirSync) | Forefront Identity Manager 2010 R2 (FIM) | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|:---:|
| Připojení k jedné místní doménové struktuře AD |● |● |● |● |● |
| Připojení k několika místním doménovým strukturám AD |● |● | |● |● |
| Připojení k několika místním Exchange Orgs |● | | | | |
| Připojení k jednomu místnímu adresáři LDAP |FR | | |● |● |
| Připojení k několika místním adresářům LDAP |FR | | |● |● |
| Připojení k místním adresářům AD a LDAP |FR | | |● |● |
| Připojení k vlastním systémům (tj. SQL, Oracle, MySQL atd.) |FR | | |● |● |
| Synchronizace atributů definovaných zákazníkem (rozšíření adresáře) |● | | | | |
| Připojení k místním HR (tj, SAP, Oracle eBusiness, PeopleSoft) |FR | | |● |● |
| Podporuje pravidla synchronizace FIM a konektory pro zřizování do místních systémů. | | | |● |● |

## <a name="cloud-to-on-premises-synchronization"></a>Synchronizace z cloudu do místního prostředí
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
| Lokalizace správce UX pro jazyky Windows Serveru |● |● |● | |
| Lokalizace koncového uživatele UX pro jazyky Windows Serveru | | | |● |
| Podpora pro Windows Server 2008 a Windows Server 2008 R2 |● pro synchronizaci, ne pro federaci |● |● |● |
| Podpora pro Windows Server 2012 a Windows Server 2012 R2 |● |● |● |● |

## <a name="filtering-and-configuration"></a>Filtrování a konfigurace
| Funkce | Azure Active Directory Connect | Služby synchronizace Azure Active Directory | Synchronizační nástroj služby Azure Active Directory (DirSync) | Forefront Identity Manager 2010 R2 (FIM) | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|:---:|
| Filtrování v rámci domén a organizačních jednotek |● |● |● |● |● |
| Filtrování v rámci hodnot atributů objektů |● |● |● |● |● |
| Povolení minimální sady atributů, které mají být synchronizovány (MinSync) |● |● | | | |
| Povolení použití jiných šablon služeb pro toky atributů |● |● | | | |
| Povolení odebrání atributů z toku ze služby AD do služby Azure AD |● |● | | | |
| Povolení upřesňujících úprav pro toky atributů |● |● | |● |● |

## <a name="next-steps"></a>Další kroky
Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).

