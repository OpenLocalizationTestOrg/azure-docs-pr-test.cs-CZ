---
title: "Rozšíření cloudových funkcí na zařízeních s Windows 10 prostřednictvím Azure Active Directory Join | Microsoft Docs"
description: "Poskytuje podrobný přehled o tom, jak zařízení s Windows 10 můžete využít Azure AD Join získat zaregistrovat v Azure Active Directory."
services: active-directory
documentationcenter: 
author: femila
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: 0cd4942f-7d54-474e-bd12-8e6764b0d42a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: d3a3d3efe1c43caff3b8d2956c14e8c90d05d22b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="extending-cloud-capabilities-to-windows-10-devices-through-azure-active-directory-join"></a>Rozšíření možností cloudu u zařízení s Windows 10 prostřednictvím služby Azure Active Directory Join
## <a name="what-is-azure-active-directory-join"></a>Co je Azure Active Directory Join?
Azure Active Directory Join (Azure AD Join) je funkce, která registruje zařízení ve vlastnictví společnosti v Azure Active Directory povolit centralizovanou správu zařízení. Umožní uživatelům, jako je například zaměstnanci a studenti, kteří se připojili ke cloudu enterprise prostřednictvím Azure Active Directory. To umožňuje zjednodušenou nasazení systému Windows a přístup k organizační aplikacím a prostředkům z libovolného zařízení systému Windows, firemních i patřících uživatelům (BYOD).

Připojení k Azure AD je určený pro podniky, které jsou cloudu první nebo jenom pro cloud – obvykle malé a střední firmy, které nemají místní infrastrukturu Windows Server Active Directory. Ale nutné dodat, Azure AD Join lze a bude také použít ve velkých organizacích na zařízeních, která jsou nepodporující to tradiční domény spojení (mobilní zařízení, např.), nebo pro uživatele, kteří především potřebují přístup k Office 365 nebo jiné SaaS aplikace integrované s Azure AD.

I když se připojení k doméně tradiční stále nabízí že nejlepší místní prostředí na zařízeních, které podporují připojení domény, Azure AD Join je vhodná pro zařízení, která nelze připojení k doméně. Připojení k Azure AD je také vhodné pro správu uživatelů v cloudu. Dělá to tak pomocí možnosti správy mobilních zařízení místo použití nástroje pro správu tradičních domény jako zásad skupiny a System Center Configuration Manager (SCCM).

![Přehled o firemní zařízení a osobní zařízení pomocí místní služby Active Directory a Azure AD](./media/active-directory-azureadjoin/active-directory-azureadjoin-overview.png)

## <a name="why-should-enterprises-adopt-azure-ad-join"></a>Proč by měl podniky přijmout připojení ke službě Azure AD?
* **Podnikům, které jsou do značné míry v cloudu**: Pokud byl přesunut nebo přesouváte k modelu, ve kterém jsou snížení nároků vaše místní a chcete pracovat, informace v cloudu, Azure AD Join může hodit. Možná jste vytvořili účty Azure AD ručně nebo přes synchronizaci služby Active Directory v místě. V obou případech budete mít účet ve službě Azure AD, a můžete ho použít pro přihlášení k Windows 10. Vaši uživatelé můžete připojení jejich počítačů do Azure AD prostřednictvím buď out-of-box zkušenosti (při prvním zapnutí) nebo v nabídce nastavení. Po připojení, budou uživatelé mohli využívat (jeden přihlašování SSO) přístup k prostředkům cloudu třeba Office 365 ve svém prohlížeči zakázanou nebo v aplikacích Office.
* **Vzdělávací instituce**: jednom ze scénářů častých o je, že vzdělávací instituce má dva typy uživatelů: zaměstnance školy a studenti. Zaměstnance školy členové jsou považovány za dlouhodobější členy organizace, tak, aby pro ně vytvoříte účty místní žádoucí. Ale studenti, kteří jsou členy shorter-term organizace a proto je možné spravovat ve službě Azure AD. To znamená, že můžete directory škálování vloží do cloudu místo uloženého místně. Také znamená, že studenty můžete přihlásit do systému Windows pomocí svých účtů Azure AD a získat přístup k prostředkům Office 365, buď ve svém prohlížeči zakázanou, nebo v aplikacích Office.
* **V maloobchodě**: jiný scénář jsme se dozvěděli od zákazníků je jejich přání snadněji spravovat sezónních pracovníků.  Účty pro zaměstnance na plný úvazek, dlouhodobější znovu, jsou vytvořeny obvykle, protože místní účty na počítačích připojených k doméně. Ale sezónní pracovníci jsou shorter-term členy org, takže je třeba spravovat kde uživatelských licencí můžete snadno přesouvat. Vytváření tyto uživatelské účty v cloudu s licencemi Office 365 umožňuje uživatelům získat výhody přihlášení k systému Windows a Office aplikace pomocí účtu Azure AD. Mezitím větší flexibilitu s jejich licencí udržovat po opustí.
* **Konkurence**: I když můžete udržovat uživatelé ve vaší místní službě Active Directory, můžete mohou využívat výhod museli uživatelé být připojený k Azure AD. Je to způsobeno nabízí zjednodušené spojující prostředí, Správa efektivní zařízení, registrace správu automatické mobilních zařízení a funkce přihlašování pro Azure AD a místní prostředky Azure AD.  

## <a name="what-capabilities-does-azure-ad-join-offer"></a>Jaké funkce nabízí připojení ke službě Azure AD?
S Azure AD Join získáte následující:

* **Automatické zřizování zařízení ve vlastnictví firmy**: uživatelé s Windows 10, můžete nakonfigurovat zařízením nikdy nepracovali, pečlivě zabaleny přihlašování out-of-box bez zapojení IT.
* **Podpora pro moderní velikostem**: na zařízení, které nemají tradiční domény připojit funkce funguje připojení k Azure AD.  
* **Podpora pro existující účty organizace**: uživatelé už nepotřebují pro vytváření a údržbu osobní účet Microsoft získat dosažení co nejlepších výsledků na vystavený společnosti zařízení, stejně jako s Windows 8. Svoje existující pracovních účtů ve službě Azure AD použitím místo. Pro mnoho společností to v podstatě znamená, že uživatelé můžete nastavit a přihlaste se k systému Windows se stejnými pověřeními, které používají pro přístup k Office 365.
* **Registrace mobilních zařízení s automatickou správu**: zařízení je automaticky registrovat správy mobilních zařízení při připojení k Azure AD. Tento proces funguje s Microsoft Intune a partnerské řešení správy mobilních zařízení. Při správě zařízení se provádí pomocí Intune, správci IT mohou monitorování nebo spravovat Azure zařízení připojená k AD vedle zařízení připojených k doméně v konzole pro správu SCCM.
* **Jednotné přihlašování k prostředkům společnosti**: uživatelé mohli využívat jednotného přihlašování z plochy Windows k aplikacím a prostředkům v cloudu, jako je například Office 365 a tisíce obchodních aplikací, které jsou závislé na službě Azure AD pro ověřování prostřednictvím [ Azure AD Connect](active-directory-azureadjoin-deployment-aadjoindirect.md). Zařízení vlastněná společností, které jsou připojené ke službě Azure AD také získejte jednotného přihlašování k prostředkům místně, když je zařízení v podnikové síti a z libovolného místa, když tyto prostředky jsou zveřejňovány prostřednictvím [Azure AD Application Proxy](https://msdn.microsoft.com/library/azure/Dn768219.aspx).
* **OS State Roaming**: nastavení usnadnění, weby, sítě Wi-Fi hesla a další nastavení jsou synchronizovány mezi firemních zařízení bez nutnosti osobní účet Microsoft.
* **Windows Store připravené pro podniky**: Windows Store podporuje aplikace pořízení a licencování s účty Azure AD. Organizace můžou volume license aplikace a zpřístupnit uživatelům ve své organizaci.

## <a name="how-do-different-devices-work-with-azure-ad-join"></a>Jak fungují různých zařízení s Azure AD Join?
| Firemní zařízení (připojené k místní doméně) | Firemní zařízení (připojený do cloudu) | Osobní zařízení |
| --- | --- | --- |
| Uživatelé mohou přihlásit do systému Windows s pracovní přihlašovací údaje (stejně jako dnes). |Uživatelé se můžete přihlásit do systému Windows pomocí pracovní přihlašovací údaje, které jsou spravovány v Azure AD. Toto je relevantní pro firemní zařízení ve třech případech: <ol><li>Organizace nemá místní (například o malou firmu) služby Active Directory.</li><li>Organizace nemá vytvořit všechny uživatelské účty ve službě Active Directory (například účty pro studenty, konzultanty nebo sezónní pracovníci nejsou vytvořeny ve službě Active Directory).</li><li>Organizace má firemní zařízení, které se nedá připojit k doméně služby (místní), jako jsou telefony nebo tablety s Mobile SKU (například sekundární zařízení potřebný k objektu pro vytváření nebo prodejní podlaží).</li></ol> Připojení k Azure AD podporuje připojení podnikových zařízení pro federované i spravované organizací. |Přihlášení k systému Windows s jejich osobní přihlašovacích údajů účtu Microsoft (žádná změna). |
| Uživatelé mají přístup k nastavení roamingu a enterprise, Windows Store. Tyto služby pracovat s pracovních účtů a nevyžadují osobní účet Microsoft. To vyžaduje organizacím připojení svojí místní službě Active Directory k Azure AD. |Mohou uživatelé samoobslužné služby Instalační program. Může použít rozhraní při prvním spuštění (FRX) pomocí svého pracovního účtu jako alternativu k s IT zřizování zařízení, i když obě metody jsou podporovány. |Uživatelé mohou snadno přidávat pracovní účet, který je spravován v Active Directory nebo Azure AD. |
| Uživatelé mají možnost jednotného přihlašování z plochy fungování aplikace, weby a prostředky – včetně místních prostředků a cloudové aplikace, které používají Azure AD pro ověřování. |Zařízení se automaticky registruje v adresáři enterprise (Azure AD) a automaticky zaregistrovaná pomocí správy mobilních zařízení. (Funkce azure AD Premium). |Uživatelé mají možnost jednotného přihlašování napříč aplikace a weby nebo prostředkům s tímto pracovním účtem. |
| Uživatele můžete přidat svoje osobní Microsoft účtů pro přístup k jejich osobní obrázky a soubory bez dopadu na podniková data. (Roaming nastavení pokračovat v práci s jejich pracovních účtů.) Účet Microsoft umožňuje jednotné přihlašování a už jednotky cestovní nastavení. |Uživatelé mohou provádět samoobslužné resetování hesla (SSPR) na přihlášení do systému Windows, což znamená, že se můžete obnovit zapomenuté heslo. (Funkce azure AD Premium). |Uživatelé mají přístup do firemní sítě Windows Store tak, aby můžete získat a použít-obchodní aplikace na svých osobních zařízeních. |

## <a name="additional-information"></a>Další informace
* [Windows 10 pro firmy: Možnosti, jak používat zařízení pro práci](active-directory-azureadjoin-windows10-devices-overview.md)
* [Rozšíření možností cloudu u zařízení s Windows 10 prostřednictvím služby Azure Active Directory Join](active-directory-azureadjoin-user-upgrade.md)
* [Ověřování identit bez hesel pomocí Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Další informace o scénářích použití pro službu Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Připojení zařízení k doméně služby Azure AD ve Windows 10 – ukázky z praxe](active-directory-azureadjoin-devices-group-policy.md)
* [Nastavení služby Azure AD Join](active-directory-azureadjoin-setup.md)

