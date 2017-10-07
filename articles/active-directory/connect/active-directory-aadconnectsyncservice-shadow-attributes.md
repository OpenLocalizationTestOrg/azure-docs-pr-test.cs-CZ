---
title: "aaaAzure AD Connect sync služby stínové atributy | Microsoft Docs"
description: "Popisuje, jak fungují stínové atributy ve službě Azure AD Connect sync service."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 1b8665e7488c6078b655f8a3e35519145bacd898
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-service-shadow-attributes"></a>Azure AD Connect sync služby stínové atributy
Většina atributy jsou reprezentované hello stejný způsobem v Azure AD jsou ve vaší místní službě Active Directory. Ale některé atributy mají některé zvláštní zpracování a hodnota atributu hello ve službě Azure AD může být jiná než co Azure AD Connect synchronizuje.

## <a name="introducing-shadow-attributes"></a>Představení stínové atributy
Některé atributy mají dva reprezentace ve službě Azure AD. Hodnota místní hello a vypočítaná hodnota, která jsou uloženy. Tyto další atributy, se nazývají stínové atributy. Nejběžnější atributy Hello dva, kde uvidíte toto chování jsou **userPrincipalName** a **proxyAddress**. Hello změnu hodnoty atributu se stane, když existují hodnoty v těchto atributech představující-ověření domény. Ale hello synchronizační modul v připojení čte hello hodnotu v atributu stínové hello tak z jeho perspektivy, hello atribut bylo potvrzeno službou Azure AD.

Nejde zobrazit hello stínové atributů s použitím hello Azure portálu nebo pomocí prostředí PowerShell. Ale Principy hello koncept pomáhá vám tootroubleshoot určité scénáře kde hello atribut má jiné hodnoty na místě a v cloudu hello.

toobetter pochopit hello chování, podívejte se na tomto příkladu ze společnosti Fabrikam:  
![Domény](./media/active-directory-aadconnectsyncservice-shadow-attributes/domains.png)  
Mají více přípon UPN ve svojí místní službě Active Directory, ale jeden pouze ověřit.

### <a name="userprincipalname"></a>UserPrincipalName
Uživatel, který má následující hodnoty atributu v doméně neověřený hello:

| Atribut | Hodnota |
| --- | --- |
| userPrincipalName na místě | lee.sperry@fabrikam.com |
| Azure AD shadowUserPrincipalName | lee.sperry@fabrikam.com |
| Azure AD userPrincipalName | lee.sperry@fabrikam.onmicrosoft.com |

atribut userPrincipalName Hello je hello hodnotu, která se zobrazí po pomocí prostředí PowerShell.

Vzhledem k tomu hodnota atributu skutečné místní hello je uložená ve službě Azure AD, když ověříte hello fabrikam.com domény, Azure AD aktualizuje atribut userPrincipalName hello s hodnotou hello z hello shadowUserPrincipalName. Nemáte toosynchronize změny z Azure AD Connect pro toobe tyto hodnoty aktualizovat.

### <a name="proxyaddresses"></a>proxyAddresses
Dobrý den, stejný proces pro pouze včetně ověřených domén rovněž nastane proxyAddresses, ale s některé další logiku. Zkontrolujte Hello ověřených domén se stane pouze pro poštovní schránky uživatele. Poštovní uživatele nebo kontaktujte představují uživateli v jiné organizaci Exchange a můžete přidat všechny hodnoty v proxyAddresses toothese objekty.

Uživatel poštovní schránky, buď místní nebo v systému Exchange Online se zobrazí pouze hodnoty ověřených domén. Ho může vypadat například takto:

| Atribut | Hodnota |
| --- | --- |
| místní proxyAddresses | SMTP:abbie.spencer@fabrikamonline.com</br>smtp:abbie.spencer@fabrikam.com</br>smtp:abbie@fabrikamonline.com |
| Exchange Online proxyAddresses | SMTP:abbie.spencer@fabrikamonline.com</br>smtp:abbie@fabrikamonline.com</br>SIP:abbie.spencer@fabrikamonline.com |

V takovém případě  **smtp:abbie.spencer@fabrikam.com**  byla odebrána, vzhledem k tomu, že doména nebyla ověřena. Ale Exchange také přidat  **SIP:abbie.spencer@fabrikamonline.com** . Společnost Fabrikam nepoužíval Lync nebo Skype na místě, ale Azure AD a Exchange Online připravte se na to.

Tato logika pro proxyAddresses je odkazované tooas **ProxyCalc**. Vyvolání ProxyCalc každé změně na uživatele při:

- Hello uživatel má přiřazeno plánu služby, které zahrnuje Exchange Online i v případě, že uživatel hello nebyl licenci pro Exchange. Například pokud uživatel hello je přiřazen hello Office E3 SKU, ale byl přiřazen pouze SharePoint Online. To platí i v případě, že poštovní schránky je stále místně.
- msExchRecipientTypeDetails Hello atributu obsahuje hodnotu.
- Musíte provést změnu tooproxyAddresses nebo userPrincipalName.

ProxyCalc může trvat některé čas tooprocess změnu na uživatele a není synchronní s procesem exportu hello Azure AD Connect.

> [!NOTE]
> Hello ProxyCalc logic obsahuje některé další chování pro pokročilé scénáře, které nejsou popsané v tomto tématu. Toto téma je pro vás toounderstand hello chování a není dokumentu všechny interní logiku.

### <a name="quarantined-attribute-values"></a>Hodnoty atributů v karanténě
Stínové atributy se také používají po duplicitními hodnotami atributů. Další informace najdete v tématu [odolnosti duplicitní atribut](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).

## <a name="see-also"></a>Viz také
* [Synchronizace služby Azure AD Connect](active-directory-aadconnectsync-whatis.md)
* [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).
