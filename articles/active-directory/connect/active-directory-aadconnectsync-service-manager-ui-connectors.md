---
title: "aaaConnectors v hello uživatelského rozhraní Azure AD Synchronization Service Manager | Microsoft Docs"
description: "Porozumět hello konektory karta v hello Synchronization Service Manager pro Azure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 60f1d979-8e6d-4460-aaab-747fffedfc1e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c0969630313178b1e299385b1289360c8f787cb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-connectors-with-hello-azure-ad-connect-sync-service-manager"></a>Pomocí konektorů hello Správce synchronizace služby Azure AD Connect

![Správce synchronizace služby](./media/active-directory-aadconnectsync-service-manager-ui/connectors.png)

Karta konektory Hello je použité toomanage všechny systémy hello synchronizační modul je připojena k.

## <a name="connector-actions"></a>Konektor akce
| Akce | Komentář |
| --- | --- |
| Vytvořit |Nepoužívejte. Pro připojení tooadditional AD doménových struktur, použijte Průvodce instalací hello. |
| Vlastnosti |Použít pro filtrování organizační jednotky a domény. |
| [Odstranění](#delete) |Použít tooeither odstranit data hello hello konektoru místo nebo toodelete připojení tooa doménovou strukturu. |
| [Konfigurace profilů spuštění](#configure-run-profiles) |S výjimkou domény filtrování, nic tooconfigure sem. Tuto akci lze použít profily spuštění toosee již nakonfigurována. |
| Spusťte |Použít toostart výjimečný spuštění profilu. |
| Zastavit |Zastaví konektor aktuálně spuštěných profil. |
| Export konektoru |Nepoužívejte. |
| Konektor pro import |Nepoužívejte. |
| Aktualizace konektoru |Nepoužívejte. |
| Aktualizovat schéma |Aktualizuje hello uložené v mezipaměti schématu. Jedná se upřednostňované toouse hello možnost v Průvodci instalací hello místo, od které také aktualizace synchronizovat pravidla. |
| [Hledání prostoru konektoru](#search-connector-space) |Použít toofind objekty a příliš[podle objektu a jeho data prostřednictvím systému hello](#follow-an-object-and-its-data-through-the-system). |

### <a name="delete"></a>Odstranění
akci odstranění Hello se používá pro dvě různé věci.  
![Správce synchronizace služby](./media/active-directory-aadconnectsync-service-manager-ui/connectordelete.png)

Hello možnost **odstranit pouze prostor konektoru** odeberete všechna data, ale zachovat hello konfigurace.

Hello možnost **odstranit konektor a konektor místo** odebere hello dat a konfigurace hello. Tato možnost se používá, když nechcete, aby tooconnect tooa doménové struktury už.

Obě možnosti synchronizovat všechny objekty a aktualizovat objekty metaverse hello. Tato akce je dlouhotrvající operace.

### <a name="configure-run-profiles"></a>Konfigurace profilů spuštění
Tato možnost umožňuje toosee hello profily nakonfigurované pro konektor spuštění.

![Správce synchronizace služby](./media/active-directory-aadconnectsync-service-manager-ui/configurerunprofiles.png)

### <a name="search-connector-space"></a>Hledání prostoru konektoru
Akce místa konektoru vyhledávání Hello je užitečné toofind objekty a data potíže.

![Správce synchronizace služby](./media/active-directory-aadconnectsync-service-manager-ui/cssearch.png)

Začněte výběrem **oboru**. Můžete hledat na základě dat (relativního Rozlišujícího, rozlišující název, ukotvení podstromu), nebo stavu objektu hello (všechny ostatní možnosti).  
![Správce synchronizace služby](./media/active-directory-aadconnectsync-service-manager-ui/cssearchscope.png)  
Pokud například provedete hledání podstromu, získáte všechny objekty v jedné organizační jednotce.  
![Správce synchronizace služby](./media/active-directory-aadconnectsync-service-manager-ui/cssearchsubtree.png)  
Z této tabulky můžete vybrat objekt, vyberte **vlastnosti**, a [na něho kliknout,](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md) z prostoru konektoru zdroj hello, prostřednictvím hello metaverse a prostoru konektoru toohello cíl.

### <a name="changing-hello-ad-ds-account-password"></a>Změna hesla účtu hello AD DS
Pokud změníte heslo účtu hello, hello synchronizační služba již nebude možné tooimport a exportu změny tooon místní AD.   Mohou se zobrazit následující hello:

- Hello importu a exportu krok pro hello AD konektoru selže s chybou "bez pověření start".
- V prohlížeči událostí systému Windows hello protokolu událostí aplikací obsahuje chybu s 6000 ID události a zprávu "hello toorun správy agenta"contoso.com"se nezdařilo, protože pověření hello nebyly platné."

tooresolve hello vydání aktualizace hello služby AD DS uživatelskému účtu pomocí hello následující:


1. Spusťte hello Synchronization Service Manager (START → synchronizace Service).
</br>![Správce synchronizace služby](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)
2. Přejděte toohello **konektory** kartě.
3. Vyberte hello AD konektor, který je nakonfigurovaný toouse hello účet služby AD DS.
4. V části Akce, vyberte **vlastnosti**.
5. V místním dialogovém okně hello vyberte připojení tooActive Directory doménové struktury:
6. Název doménové struktury Hello označuje hello odpovídající místní AD.
7. Uživatelské jméno Hello označuje hello AD DS účet používaný pro synchronizaci.
8. Zadejte nové heslo hello hello AD DS účtu v textovém poli heslo hello ![Azure AD Connect Sync šifrování klíče nástroj](media/active-directory-aadconnectsync-encryption-key/key6.png)
9. Klikněte na tlačítko OK toosave hello nové heslo a restartujte hello synchronizační služba tooremove hello staré heslo z mezipaměti.



## <a name="next-steps"></a>Další kroky
Další informace o hello [synchronizace Azure AD Connect](active-directory-aadconnectsync-whatis.md) konfigurace.

Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).
