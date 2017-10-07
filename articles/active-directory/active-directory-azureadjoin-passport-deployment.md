---
title: "aaaEnable Microsoft Windows Hello pro firmy ve vaší organizaci | Microsoft Docs"
description: "Nasazení pokyny tooenable Microsoft Passport ve vaší organizaci."
services: active-directory
documentationcenter: 
keywords: "Konfigurace Microsoft Passport, Microsoft Windows Hello pro firmy nasazení"
author: MarkusVi
manager: femila
tags: azure-classic-portal
ms.assetid: 7dbbe3c6-1cd7-429c-a9b2-115fcbc02416
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.openlocfilehash: 6041f5916f606752bc55844b1b2d0a423b913cd3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-microsoft-windows-hello-for-business-in-your-organization"></a>Povolit Microsoft Windows Hello pro firmy ve vaší organizaci
Po [připojení zařízení s Windows 10 připojených k doméně se službou Azure Active Directory](active-directory-azureadjoin-devices-group-policy.md), hello následující tooenable Microsoft Windows Hello pro firmy ve vaší organizaci:

1. Nasazení nástroje System Center Configuration Manager  
2. Konfigurace nastavení zásad
3. Konfigurace profilu certifikátu hello  

## <a name="deploy-system-center-configuration-manager"></a>Nasazení nástroje System Center Configuration Manager
toodeploy uživatelské certifikáty založené na Windows Hello pro firmy klíče, je třeba hello následující:

* **System Center Configuration Manager aktuální větve** -potřebovat tooinstall verze 1606 nebo vyšší. Další informace najdete v tématu hello [dokumentace pro System Center Configuration Manager](https://technet.microsoft.com/library/mt346023.aspx) a [Blog týmu nástroje System Center Configuration Manager](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx).
* **Infrastruktury veřejných klíčů (PKI)** -tooenable Microsoft Windows Hello pro firmy pomocí uživatelských certifikátů, musíte mít infrastrukturu veřejných KLÍČŮ na místě. Pokud nemáte, nebo pokud nechcete, aby toouse ji u uživatelských certifikátů můžete nasadit nový řadič domény, který má sestavení 10551 (nebo lepší) nainstalován systém Windows Server 2016. Postupujte podle kroků hello příliš[instalace repliky řadiče domény v existující doméně](https://technet.microsoft.com/library/jj574134.aspx) nebo příliš[instalaci nové doménové struktury služby Active Directory, pokud vytváříte nové prostředí](https://technet.microsoft.com/library/jj574166). (hello soubory ISO jsou k dispozici ke stažení na [Signiant média Exchange](https://datatransfer.microsoft.com/signiant_media_exchange/spring/main?sdkAccessible=true).)

## <a name="configure-policy-settings"></a>Konfigurace nastavení zásad
hello tooconfigure Microsoft Windows Hello pro firmy nastavení zásad, máte dvě možnosti:

* Zásady skupiny ve službě Active Directory 
* Hello System Center Configuration Manager 

Pomocí zásad skupiny ve službě Active Directory je doporučená metoda tooconfigure Microsoft Windows Hello pro firmy nastavení zásad hello. 

Pomocí nástroje System Center Configuration Manager je metoda hello preferované, pokud jste ji použít i toodeploy certifikáty. Tento scénář:

* Zajišťuje kompatibilitu s hello novější scénáře nasazení
* Vyžaduje na straně klienta hello Windows 10 verze 1607 nebo lepší.

### <a name="configure-microsoft-windows-hello-for-business-via-group-policy-in-active-directory"></a>Konfigurace Microsoft Windows Hello pro firmy prostřednictvím zásad skupiny ve službě Active Directory
**Kroky**:

1. Otevřete správce serveru a přejděte příliš**nástroje** > **Správa zásad skupiny**.
2. Správa zásad skupiny přejděte toohello domény uzlu, který odpovídá toohello domény, ve kterém chcete tooenable připojení ke službě Azure AD.
3. Klikněte pravým tlačítkem na **objekty zásad skupiny**a vyberte **nový**. Pojmenujte objektu zásad skupiny, například povolení Windows Hello pro firmy. Klikněte na **OK**.
4. Klikněte pravým tlačítkem na nový objekt zásad skupiny a potom vyberte **upravit**.
5. Přejděte příliš**konfigurace počítače** > **zásady** > **šablony pro správu** > **Windows Součásti** > **Windows Hello pro firmy**.
6. Klikněte pravým tlačítkem na **povolit Windows Hello pro firmy**a potom vyberte **upravit**.
7. Vyberte hello **povoleno** a pak klikněte na tlačítko **použít**. Klikněte na **OK**.
8. Nyní můžete propojit hello objektu zásad skupiny tooa umístění podle vaší volby. tooenable tato zásada u všech hello připojený k doméně zařízení s Windows 10 ve vaší organizaci, odkaz hello zásad skupiny toohello domény. Například:
   * Konkrétní organizační jednotku (OU) ve službě Active Directory, kde budou umístěné počítače s Windows 10 připojených k doméně
   * Skupinu zabezpečení, která obsahuje připojený k doméně počítače Windows 10, které se budou automaticky registrovat s Azure AD

### <a name="configure-windows-hello-for-business-using-system-center-configuration-manager"></a>Konfigurace Windows Hello pro firmy pomocí System Center Configuration Manager
**Kroky**:

1. Otevřete hello **System Center Configuration Manager**a potom přejděte příliš**prostředky a kompatibilita > Nastavení dodržování předpisů > přístup k prostředkům společnosti > Windows Hello pro firmy profily**.
   
    ![Konfigurace Windows Hello pro firmy](./media/active-directory-azureadjoin-passport-deployment/01.png)
2. V panelu nástrojů hello hello nahoře, klikněte na **vytvořit Windows Hello pro firmy profil**.
   
    ![Konfigurace Windows Hello pro firmy](./media/active-directory-azureadjoin-passport-deployment/02.png)
3. Na hello **Obecné** dialogové okno, proveďte následující kroky hello:
   
    ![Konfigurace Windows Hello pro firmy](./media/active-directory-azureadjoin-passport-deployment/03.png)
   
    a. V hello **název** textovému poli, zadejte název pro svůj profil, například **Můj profil WHfB**.
   
    b. Klikněte na **Další**.
4. Na hello **podporované platformy** dialogové okno, vyberte hello platformy, které budou zřizovány s Tento Windows Hello pro firmy profil a pak klikněte na tlačítko **Další**.
   
    ![Konfigurace Windows Hello pro firmy](./media/active-directory-azureadjoin-passport-deployment/04.png)
5. Na hello **nastavení** dialogové okno, proveďte následující kroky hello:
   
    ![Konfigurace Windows Hello pro firmy](./media/active-directory-azureadjoin-passport-deployment/05.png)
   
    a. Jako **konfigurace Windows Hello pro firmy**, vyberte **povoleno**.
   
    b. Jako **používat (Trusted Platform Module)**, vyberte **požadované**. 
   
    c. Jako **metodu ověřování**, vyberte **založené na certifikátech**.
   
    d. Klikněte na **Další**.
6. Na hello **Souhrn** dialogové okno, klikněte na tlačítko **Další**.
7. Na hello **dokončení** dialogové okno, klikněte na tlačítko **Zavřít**.
8. V panelu nástrojů hello hello nahoře, klikněte na **nasadit**.
   
    ![Konfigurace Windows Hello pro firmy](./media/active-directory-azureadjoin-passport-deployment/06.png)

## <a name="configure-hello-certificate-profile"></a>Konfigurace profilu certifikátu hello
Pokud používáte ověřování pomocí certifikátů pro místní ověřování, budete potřebovat tooconfigure a nasazení profilu certifikátu. Tato úloha vyžaduje tooset nastavujete NDES server a role lokality bodu registrace certifikátu v hello System Center Configuration Manager. Další podrobnosti najdete v tématu hello [požadavky na profily certifikátů v nástroji Configuration Manager](https://technet.microsoft.com/library/dn261205.aspx).

1. Otevřete hello **System Center Configuration Manager**a potom přejděte příliš**prostředky a kompatibilita > Nastavení dodržování předpisů > přístup k prostředkům společnosti > profily certifikátů**.
2. Vyberte šablonu, která obsahuje čipové karty přihlášení rozšířené použití klíče (EKU).

Na hello **zápis SCEP** stránku hello profilu certifikátu, je nutné toochoose **tooPassport pro pracovní jinak dojde k selhání instalace** jako hello **zprostředkovatele úložiště klíčů**.

## <a name="next-steps"></a>Další kroky
* [Windows 10 pro podnik hello: způsoby toouse zařízení pro práci](active-directory-azureadjoin-windows10-devices-overview.md)
* [Rozšíření cloudových funkcí tooWindows 10 zařízení prostřednictvím Azure Active Directory Join](active-directory-azureadjoin-user-upgrade.md)
* [Ověřování identit bez hesel pomocí Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Další informace o scénářích použití pro službu Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Připojení zařízení připojených k doméně tooAzure AD pro prostředí Windows 10](active-directory-azureadjoin-devices-group-policy.md)
* [Nastavení služby Azure AD Join](active-directory-azureadjoin-setup.md)

