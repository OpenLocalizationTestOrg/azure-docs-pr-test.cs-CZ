---
title: "Azure AD Connect: Začínáme s použitím expresního nastavení | Dokumentace Microsoftu"
description: "Zjistěte, jak toodownload, nainstalujte a spusťte Průvodce instalací hello Azure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: curtand
ms.assetid: b6ce45fd-554d-4f4d-95d1-47996d561c9f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 79f796fa7738b85e9236e856bddb529379f60390
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-ad-connect-using-express-settings"></a>Začínáme se službou Azure AD Connect s použitím expresního nastavení
**Expresní nastavení** Azure AD Connect se používá, pokud máte jednoduchou doménovou strukturu a [synchronizaci hesel](active-directory-aadconnectsync-implement-password-synchronization.md) pro ověřování. **Expresní nastavení** je hello výchozí možnost a používá se u scénáře hello nejčastěji nasazuje. Můžete se jenom několik kliknutí a tokeny tooextend vašeho místního adresáře toohello cloudu.

Před zahájením instalace Azure AD Connect, ujistěte se, příliš[stáhnout Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771) a dokončení hello nezbytné kroky [Azure AD Connect: Hardware a nezbytné předpoklady](active-directory-aadconnect-prerequisites.md).

Pokud expresní nastavení neodpovídá vaší topologii, přečtěte si [související dokumentaci](#related-documentation) s dalšími scénáři.

## <a name="express-installation-of-azure-ad-connect"></a>Expresní instalace služby Azure AD Connect
Zobrazí se tyto kroky v akci v hello [videa](#videos) části.

1. Přihlaste se jako, které chcete tooinstall Azure AD Connect na server toohello místního správce. Měli byste to provést na serveru hello chcete toobe hello synchronizační server.
2. Přejděte poklikejte na soubor tooand **AzureADConnect.msi**.
3. Na úvodní obrazovce hello vyberte hello pole schválení toohello licenční smlouvy a klikněte na **pokračovat**.  
4. Na úvodní obrazovka Expresní nastavení, klikněte na tlačítko **použít Expresní nastavení**.  
   ![Vítejte tooAzure AD Connect](./media/active-directory-aadconnect-get-started-express/express.png)
5. Na úvodní obrazovka tooAzure AD Connect zadejte hello uživatelské jméno a heslo pro globálního správce pro vaši službu Azure AD. Klikněte na **Další**.  
   ![Připojit tooAzure AD](./media/active-directory-aadconnect-get-started-express/connectaad.png) Pokud se zobrazí chybová zpráva a máte problémy s připojením, pak v tématu [řešení problémů s připojením](active-directory-aadconnect-troubleshoot-connectivity.md).
6. Na obrazovce DS tooAD hello připojení zadejte hello uživatelské jméno a heslo pro účet správce podnikové sítě. Můžete zadat část hello domény ve formátu NetBios nebo plně kvalifikovaný název domény, tj. FABRIKAM\administrator nebo fabrikam.com\administrator. Klikněte na **Další**.  
   ![Připojit tooAD DS](./media/active-directory-aadconnect-get-started-express/connectad.png)
7. Hello [ **konfigurace přihlášení k Azure AD** ](active-directory-aadconnect-user-signin.md#azure-ad-sign-in-configuration) stránka zobrazuje jenom Pokud jste neprovedli [ověření svých domén](../active-directory-add-domain.md) v hello [požadavky](active-directory-aadconnect-prerequisites.md).
   ![Neověřené domény](./media/active-directory-aadconnect-get-started-express/unverifieddomain.png)  
   Pokud se vám zobrazí tato stránka, zkontrolujte všechny domény označené jako **Nepřidáno** a **Neověřeno**. Ujistěte se, že domény, které používáte, byly ověřeny v Azure AD. Po ověření domény, klikněte na symbol obnovení hello.
8. Na obrazovce připraveno tooconfigure hello, klikněte na tlačítko **nainstalovat**.
   * Volitelně můžete na stránce Připraveno tooconfigure hello, můžete zrušit zaškrtnutí hello **zahájit proces synchronizace hello ihned po dokončení konfigurace** zaškrtávací políčko. Jste měli zaškrtnutí tohoto políčka zrušte Pokud chcete toodo další konfiguraci, například [filtrování](active-directory-aadconnectsync-configure-filtering.md). Pokud zrušíte tuto možnost, hello Průvodce provede konfiguraci synchronizace, ale ponechá hello scheduler zakázána. Nejde spustit, dokud jej ručně neaktivujete tím [opětným spuštěním Průvodce instalací hello](active-directory-aadconnectsync-installation-wizard.md).
   * Pokud máte Exchange ve vaší místní službě Active Directory, pak máte také možnost tooenable [ **hybridní nasazení systému Exchange**](https://technet.microsoft.com/library/jj200581.aspx). Tuto možnost povolte, pokud jste v plánu toohave poštovní schránky Exchange existovaly jak v cloudu hello a místně v hello stejný čas.
     ![Připraveno tooconfigure Azure AD Connect](./media/active-directory-aadconnect-get-started-express/readytoconfigure.png)
9. Po dokončení instalace hello, klikněte na tlačítko **ukončení**.
10. Po dokončení instalace hello se odhlaste a znovu přihlaste teprve pak použijte Synchronization Service Manager nebo Synchronization Rule Editor.

## <a name="videos"></a>Videa
Video o používání hello Expresní instalace najdete v tématu:

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-Active-Directory-Connect-Express-Settings/player]
> 
> 

## <a name="next-steps"></a>Další kroky
Teď, když máte nainstalovanou službu Azure AD Connect můžete [ověřit hello instalaci a přiřadit licence](active-directory-aadconnect-whats-next.md).

Další informace o těchto funkcích, které byly povoleny v rámci instalace hello: [automatický upgrade](active-directory-aadconnect-feature-automatic-upgrade.md), [prevence náhodného odstranění](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md), a [Azure AD Connect Health](../connect-health/active-directory-aadconnect-health-sync.md).

Další informace o těchto běžných tématech: [Plánovač a jak synchronizaci tootrigger](active-directory-aadconnectsync-feature-scheduler.md).

Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).

## <a name="related-documentation"></a>Související dokumentace
| Téma |
| --- | --- |
| Přehled služby Azure AD Connect |
| Instalace s vlastním nastavením |
| Upgrade z nástroje DirSync |
| Účty použité k instalaci |

