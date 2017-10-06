---
title: "aaaExtending cloudu možnosti tooWindows 10 zařízení prostřednictvím Azure Active Directory Join | Microsoft Docs"
description: "Poskytuje podrobný přehled o tom, jak zařízení s Windows 10 můžete využít Azure AD Join tooget registrované v Azure Active Directory."
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
ms.openlocfilehash: db9ae9caeb3951d1fdd1d2477827012fd10ace60
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="extending-cloud-capabilities-toowindows-10-devices-through-azure-active-directory-join"></a>Rozšíření cloudových funkcí tooWindows 10 zařízení prostřednictvím Azure Active Directory Join
## <a name="what-is-azure-active-directory-join"></a>Co je Azure Active Directory Join?
Azure Active Directory Join (Azure AD Join) je hello funkce, která registruje zařízení ve vlastnictví společnosti v Azure Active Directory tooenable centralizované správy zařízení hello. Umožní uživatelům, jako je například zaměstnanci a studenti tooconnect toohello enterprise cloudu prostřednictvím Azure Active Directory. To umožňuje zjednodušenou nasazení systému Windows a přístup tooorganizational aplikacím a prostředkům z libovolného zařízení systému Windows, firemních i patřících uživatelům (BYOD).

Připojení k Azure AD je určený pro podniky, které jsou cloudu první nebo jenom pro cloud – obvykle malé a střední firmy, které nemají místní infrastrukturu Windows Server Active Directory. Můžete uvedené, Azure AD Join a taky použít ve velkých organizacích na zařízení, které jsou nepodporující to tradiční domény spojení (mobilní zařízení, např.) nebo pro uživatele, kteří potřebují především tooaccess Office 365 nebo jiné aplikace SaaS integrované s Azure AD.

I když se připojení k tradiční doméně hello stále nabízí hello nejlepší místní prostředí na zařízeních, které podporují připojení domény, připojení k Azure AD je vhodný pro zařízení, která nelze připojení k doméně. Připojení k Azure AD je také vhodné pro správu uživatelů v cloudu hello. Dělá to tak pomocí možnosti správy mobilních zařízení místo použití nástroje pro správu tradičních domény jako zásad skupiny a System Center Configuration Manager (SCCM).

![Přehled o firemní zařízení a osobní zařízení pomocí místní služby Active Directory a Azure AD](./media/active-directory-azureadjoin/active-directory-azureadjoin-overview.png)

## <a name="why-should-enterprises-adopt-azure-ad-join"></a>Proč by měl podniky přijmout připojení ke službě Azure AD?
* **Podnikům, které jsou do značné míry v hello cloudu**: Pokud byl přesunut nebo přesouváte tooa modelu, ve kterém jsou snížení nároků vaše místní a chcete toooperate další v cloudu hello, Azure AD Join může hodit. Možná jste vytvořili účty Azure AD ručně nebo přes synchronizaci služby Active Directory v místě. V obou případech budete mít účet ve službě Azure AD, a můžete ji použít toosign v tooWindows 10. Uživatelé se můžete zapojit do jejich počítačů tooAzure AD buď hello out-of-box rozhraní (při prvním zapnutí) nebo pomocí nabídky nastavení hello. Po připojení, budou uživatelé mohli využívat jeden přihlašování (SSO) přístup k toocloud prostředkům třeba Office 365 ve svém prohlížeči zakázanou nebo v aplikacích Office.
* **Vzdělávací instituce**: jednom ze scénářů hello častých o je, že vzdělávací instituce má dva typy uživatelů: zaměstnance školy a studenti. Zaměstnance školy členové jsou považovány za dlouhodobější členy hello organizace, tak, aby pro ně vytvoříte účty místní žádoucí. Ale studenti, kteří jsou členy shorter-term hello organizace a proto je možné spravovat ve službě Azure AD. To znamená, že adresář škálování mohou poslat toohello cloudu místo uloženého místně. Také znamená, že studenti, kteří si v tooWindows s účty služby Azure AD a získat přístup k tooOffice 365 prostředky, ve svém prohlížeči zakázanou nebo v aplikacích Office.
* **V maloobchodě**: jiný scénář jsme se dozvěděli od zákazníků je jejich desire toomanage sezónní pracovníci snadněji.  Účty pro zaměstnance na plný úvazek, dlouhodobější znovu, jsou vytvořeny obvykle, protože místní účty na počítačích připojených k doméně. Ale sezónní pracovníci jsou shorter-term členy hello org, takže je žádoucí toomanage je kde uživatelských licencí můžete snadno přesouvat. Vytváření tyto uživatelské účty v cloudu hello s licencemi Office 365 umožňuje hello uživatelé tooget hello výhody podepisování tooWindows a v aplikacích Office pomocí účtu Azure AD. Mezitím větší flexibilitu s jejich licencí udržovat po opustí.
* **Konkurence**: I když můžete udržovat uživatelé ve vaší místní službě Active Directory, můžete mohou využívat výhod museli uživatelé být připojený k Azure AD. Je to způsobeno nabízí zjednodušené spojující prostředí, Správa efektivní zařízení, registrace správu automatické mobilních zařízení a funkce přihlašování pro Azure AD a místní prostředky Azure AD.  

## <a name="what-capabilities-does-azure-ad-join-offer"></a>Jaké funkce nabízí připojení ke službě Azure AD?
S Azure AD Join získáte následující hello:

* **Automatické zřizování zařízení ve vlastnictví firmy**: uživatelé s Windows 10, můžete nakonfigurovat zařízením nikdy nepracovali, pečlivě zabaleny out-of-box přihlašování hello bez zapojení IT.
* **Podpora pro moderní velikostem**: na zařízení, které nemají hello tradiční domény připojit funkce funguje připojení k Azure AD.  
* **Podpora pro existující účty organizace**: uživatelé už nebude potřebovat toocreate a údržbu osobní tooget účet Microsoft hello nejlepších na vystavený společnosti zařízení, stejně jako s Windows 8. Svoje existující pracovních účtů ve službě Azure AD použitím místo. Pro mnoho společností to v podstatě znamená, že uživatelé můžete nastavit a přihlášení tooWindows s hello stejné přihlašovací údaje, používají tooaccess Office 365.
* **Registrace mobilních zařízení s automatickou správu**: zařízení při správě mobilních zařízení při připojení automaticky zaregistroval tooAzure AD. Tento proces funguje s Microsoft Intune a partnerské řešení správy mobilních zařízení. Při správě zařízení se provádí pomocí Intune, správci IT mohou monitorování nebo spravovat Azure zařízení připojená k AD vedle zařízení připojených k doméně v konzole pro správu SCCM hello.
* **Jednotné přihlašování toocompany prostředky**: uživatelé mohli využívat jednotného přihlašování z plochy tooapps Windows hello a prostředky v cloudu hello, jako je například Office 365 a tisíce obchodních aplikací, které jsou závislé na službě Azure AD pro ověřování prostřednictvím [Azure AD Connect](active-directory-azureadjoin-deployment-aadjoindirect.md). Zařízení vlastněná společností, které jsou připojené k tooAzure AD také získejte tooon místní prostředky jednotné přihlašování, pokud zařízení hello v podnikové síti a z libovolného místa, pokud tyto prostředky jsou zveřejňovány prostřednictvím hello [Azure AD Application Proxy](https://msdn.microsoft.com/library/azure/Dn768219.aspx).
* **OS State Roaming**: nastavení usnadnění, weby, sítě Wi-Fi hesla a další nastavení jsou synchronizovány mezi firemních zařízení bez nutnosti osobní účet Microsoft.
* **Windows Store připravené pro podniky**: hello Windows Store podporuje aplikace pořízení a licencování s účty Azure AD. Organizace můžou volume license aplikací a ujistěte se, je k dispozici toohello uživatelé v organizaci.

## <a name="how-do-different-devices-work-with-azure-ad-join"></a>Jak fungují různých zařízení s Azure AD Join?
| Firemní zařízení (místní tooon připojené k doméně) | Firemní zařízení (připojený k toohello cloud) | Osobní zařízení |
| --- | --- | --- |
| Uživatelé mohou přihlásit do systému Windows s pracovní přihlašovací údaje (stejně jako dnes). |Uživatelé můžou přihlásit tooWindows s pracovní přihlašovací údaje, které jsou spravovány v Azure AD. Toto je relevantní pro firemní zařízení ve třech případech: <ol><li>Hello organizace nemá místní (například o malou firmu) služby Active Directory.</li><li>Hello organizace nemá vytvořit všechny uživatelské účty ve službě Active Directory (například účty pro studenty, konzultanty nebo sezónní pracovníci nejsou vytvořeny ve službě Active Directory).</li><li>Hello organizace nemá podniková zařízení, která nemůže být připojený k tooan (místní) domény, jako jsou telefony nebo tablety s Mobile SKU (například sekundární zařízení prováděné tooa objekt pro vytváření nebo prodejní podlaží).</li></ol> Připojení k Azure AD podporuje připojení podnikových zařízení pro federované i spravované organizací. |Uživatelé přihlásit tooWindows s jejich osobní přihlašovacích údajů účtu Microsoft (žádná změna). |
| Uživatelé mají přístup tooroaming nastavení a enterprise hello Windows Store. Tyto služby pracovat s pracovních účtů a nevyžadují osobní účet Microsoft. To vyžaduje organizace tooconnect jejich místní služby Active Directory tooAzure AD. |Mohou uživatelé samoobslužné služby Instalační program. Mohou přejít prostřednictvím hello při prvním spuštění (FRX) pomocí svého pracovního účtu jako alternativní toohaving IT hello zřizování zařízení, i když obě metody jsou podporovány. |Uživatelé mohou snadno přidávat pracovní účet, který je spravován v Active Directory nebo Azure AD. |
| Uživatelé mají možnost jednotného přihlašování z hello plochy toowork aplikace, weby a prostředky – včetně místních prostředků a cloudové aplikace, které používají Azure AD pro ověřování. |Zařízení se automaticky registruje v hello enterprise directory (Azure AD) a automaticky zaregistrovaná pomocí správy mobilních zařízení. (Funkce azure AD Premium). |Uživatelé mají možnost jednotného přihlašování k aplikacím a toowebsites nebo prostředkům s tímto pracovním účtem. |
| Uživatele můžete přidat svoje osobní účty tooaccess Microsoft jejich osobní obrázky a soubory bez dopadu na podniková data. (Roaming nastavení pokračovat toowork pomocí pracovního účtu.) Hello účtu Microsoft umožňuje jednotné přihlašování a už jednotky hello cestovní nastavení. |Uživatelé mohou provádět samoobslužné resetování hesla (SSPR) na přihlášení do systému Windows, což znamená, že se můžete obnovit zapomenuté heslo. (Funkce azure AD Premium). |Uživatelé mají přístup k toohello enterprise Windows Store tak, aby můžete získat a použít-obchodní aplikace na svých osobních zařízeních. |

## <a name="additional-information"></a>Další informace
* [Windows 10 pro podnik hello: způsoby toouse zařízení pro práci](active-directory-azureadjoin-windows10-devices-overview.md)
* [Rozšíření cloudových funkcí tooWindows 10 zařízení prostřednictvím Azure Active Directory Join](active-directory-azureadjoin-user-upgrade.md)
* [Ověřování identit bez hesel pomocí Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Další informace o scénářích použití pro službu Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Připojení zařízení připojených k doméně tooAzure AD pro prostředí Windows 10](active-directory-azureadjoin-devices-group-policy.md)
* [Nastavení služby Azure AD Join](active-directory-azureadjoin-setup.md)

