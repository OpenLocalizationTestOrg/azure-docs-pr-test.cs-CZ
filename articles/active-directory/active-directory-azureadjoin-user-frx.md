---
title: "aaaSet si nové zařízení s Azure AD během instalace | Microsoft Docs"
description: "Téma, které vysvětluje, jak uživatelé mohou vytvořit připojení ke službě Azure AD během jejich prostředí prvního spuštění."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 6afce4be7f084f1956a6f9dbddaa8def0605956d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-new-device-with-azure-ad-during-setup"></a>Nastavit nové zařízení s Azure AD během instalace
V systému Windows 10 uživatelé mohou připojit své zařízení tooAzure služby Active Directory (Azure AD) v hello při prvním spuštění (FRX). To umožňuje organizacím toodistribute pečlivě zabaleny zařízení tootheir zaměstnanci a studenti, kteří, nebo nechat je zvolte svá vlastní zařízení (CYOD).
Pokud edice Windows 10 Professional nebo Windows 10 Enterprise je nainstalovaná na zařízení, hello prostředí procesu instalace toohello výchozí nastavení pro zařízení vlastněná společností.

## <a name="toojoin-a-device-tooazure-ad"></a>toojoin zařízení tooAzure AD
1. Při zapnutí nové zařízení a spustit proces instalace hello, měli byste vidět hello **získávání připravené** zprávy. Postupujte podle výzvy tooset hello daného zařízení.
2. Spuštění zadáním oblast a jazyk. Přijměte licenční podmínky softwaru společnosti Microsoft hello.
   ![Přizpůsobit pro vaši oblast](./media/active-directory-azureadjoin/active-directory-azureadjoin-customize-region.png)
3. Vyberte síť hello toouse chcete pro připojení toohello Internetu.
4. Vyberte, zda používáte osobní zařízení nebo zařízení ve vlastnictví společnosti. Pokud je ve vlastnictví společnosti, klikněte na tlačítko **toto zařízení patří organizaci toomy**. Spustí se prostředí Azure AD Join hello. Toto je na obrazovce, že se zobrazí, pokud používáte Windows 10 Professional.
   <center>
   ![Kdo je vlastníkem této obrazovce počítače](./media/active-directory-azureadjoin/active-directory-azureadjoin-who-owns-pc.png)
5. Zadejte přihlašovací údaje hello, které byly poskytnuty tooyou vaší organizací.
   <center>
   ![Přihlašovací obrazovky](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)
6. Po zadání uživatelského jména, odpovídající klienta se nachází ve službě Azure AD. Pokud jste v federovanou doménu, bude přesměrované tooyour na místním serveru zabezpečení tokenu služby (STS) – například Active Directory Federation Services (AD FS).
7. Pokud jste uživatele v doméně nefederovaných, zadejte přihlašovací údaje přímo na hello stránky Azure AD hostované. Pokud bylo nakonfigurované firemní branding, uvidíte také logo vaší organizace a podporují text.
8. Se zobrazí výzva k výzvy ověřování Multi-Factor authentication. Tento problém je možné konfigurovat správcem IT.
9. Azure AD ověří, jestli tento uživatel nebo zařízení vyžaduje registraci správy mobilních zařízení.
10. Windows hello zařízení registruje v adresáři organizace hello ve službě Azure AD a zaregistruje ve správě mobilních zařízení v případě potřeby.
11. Pokud jste spravované uživatele, trvá Windows desktop toohello procesem hello automatické přihlášení.
12. Pokud jste federované uživatele, jsou směrované toohello přihlášení Windows obrazovky tooenter přihlašovacích údajů.

> [!NOTE]
> Připojení k místní doméně systému Windows Server Active Directory v systému Windows hello out-of-box prostředí nepodporuje. Proto pokud máte v plánu toojoin tooa domény počítače, měli byste vybrat hello odkaz **Windows nastavte s místním účtem** místo. Ve vašem počítači může potom připojí hello domény z hello nastavení, které provedete krok před.
> 
> 

## <a name="additional-information"></a>Další informace
* [Windows 10 pro podnik hello: způsoby toouse zařízení pro práci](active-directory-azureadjoin-windows10-devices-overview.md)
* [Rozšíření cloudových funkcí tooWindows 10 zařízení prostřednictvím Azure Active Directory Join](active-directory-azureadjoin-user-upgrade.md)
* [Ověřování identit bez hesel pomocí Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Další informace o scénářích použití pro službu Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Připojení zařízení připojených k doméně tooAzure AD pro prostředí Windows 10](active-directory-azureadjoin-devices-group-policy.md)
* [Nastavení služby Azure AD Join](active-directory-azureadjoin-setup.md)

