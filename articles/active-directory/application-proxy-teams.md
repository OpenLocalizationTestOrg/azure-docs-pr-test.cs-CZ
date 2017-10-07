---
title: "aaaAccess aplikací Proxy aplikace Azure AD v týmy | Microsoft Docs"
description: "Pomocí proxy aplikace služby Azure AD tooaccess místní aplikaci prostřednictvím Teams společnosti Microsoft."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 13c36e43ae6349df09272e308ad4f40451cbbeb9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="access-your-on-premises-applications-through-microsoft-teams"></a>Přístup k místním aplikacím přes Teams společnosti Microsoft

Azure Proxy aplikace Active Directory nabízí jeden přihlašování tooon místní aplikace bez ohledu na to, kde se a Microsoft Teams zjednodušuje vaše spolupráce úsilí na jednom místě. Integrace hello dva společně znamená, že vaši uživatelé mohli být produktivní s jejich členy týmu v jakékoliv jiné situaci. 

Uživatele můžete přidat cloudové aplikace tootheir týmy kanály [pomocí karty](https://support.office.com/article/Video-Using-Tabs-7350a03e-017a-4a00-a6ae-1c9fe8c497b3?ui=en-US&rs=en-US&ad=US), ale co se stane, když je tento web služby SharePoint nebo všechny používají nástroj plánování hostovaný místně? Proxy aplikací je hello řešení. Aplikace můžou přidat publikované prostřednictvím Proxy aplikace tootheir hello kanály pomocí stejné adresy URL externí vždy používají tooaccess svoje aplikace vzdáleně. A protože Proxy aplikace ověřuje prostřednictvím Azure Active Directory, hello dodávané stejné jednoho přihlášení prostřednictvím.


## <a name="install-hello-application-proxy-connector-and-publish-your-app"></a>Instalace konektoru Proxy aplikace hello a publikování aplikace

Pokud jste to ještě neudělali, [konfiguraci Proxy aplikace vašeho klienta a instalaci konektoru hello](active-directory-application-proxy-enable.md). Potom [publikovat místní aplikace](application-proxy-publish-azure-portal.md) pro vzdálený přístup. Při publikování aplikace hello, poznamenejte si externí adresu URL hello vzhledem k tomu, že koncoví uživatelé musí tyto informace při přidávání tooTeams aplikace hello.

Pokud již máte aplikace publikována, ale nepamatujete jejich externí adresy URL, vyhledejte je v hello [portál Azure](https://portal.azure.com). Přihlaste se a potom přejděte příliš**Azure Active Directory** > **podnikové aplikace, které** > **všechny aplikace** > vyberte svou aplikaci > **Proxy aplikace**.

## <a name="add-your-app-tooteams"></a>Přidat tooTeams vaší aplikace

Jakmile publikujete aplikaci hello prostřednictvím Proxy aplikace, dát uživatelům vědět, že se můžete přidat jako karty přímo v jejich týmy kanály. Kliknul proveďte tyto kroky:

1. Přejděte toohello týmy kanálu místo tooadd tuto aplikaci a vyberte  **+**  tooadd na kartě.

   ![Vyberte Přidat na kartě](./media/application-proxy-teams/add-tab.png)

2. Vyberte **webu** z hello kartě Možnosti.

   ![Přidání webu](./media/application-proxy-teams/website.png)

3. Pojmenujte hello kartě a nastavte hello URL toohello Proxy aplikace externí adresu URL. 

   ![Konfigurovat adresu URL a název karty](./media/application-proxy-teams/tab-name-url.png)

Jakmile jeden člen týmu přidá kartu hello, se zobrazí u všech uživatelů v kanálu hello. Uživatelé, kteří mají přístup k aplikaci toohello získat přístup jednoho přihlášení s přihlašovacími údaji hello, které používají pro Teams společnosti Microsoft. Uživatelé, kteří nemají přístup k aplikaci toohello se zobrazí karta hello v týmy, ale je blokovaný, dokud jim poskytnout oprávnění toohello místní aplikace a hello Azure portálu publikovanou verzi aplikace hello. 

## <a name="next-steps"></a>Další kroky

- Zjistěte, jak příliš[publikování webů služby SharePoint místní](application-proxy-enable-remote-access-sharepoint.md) pomocí Proxy aplikace.
- Konfigurace vaší aplikace toouse [vlastní domény](active-directory-application-proxy-custom-domains.md) pro jejich externí adresu URL. 
