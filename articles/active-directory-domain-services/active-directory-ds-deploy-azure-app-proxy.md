---
title: "Azure Active Directory Domain Services: Nasazení Proxy aplikace služby Azure Active Directory | Microsoft Docs"
description: "Použít proxy aplikace služby Azure AD na spravované domény Azure Active Directory Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 4142111231d0256960d0c02d686d51533ba2171c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-azure-ad-application-proxy-on-an-azure-ad-domain-services-managed-domain"></a>Nasazení Azure AD Application Proxy na spravované doméně služby Azure AD Domain Services
Proxy aplikace služby Azure Active Directory (AD) umožňuje podporu zaměstnanci na vzdálených pracovištích tím, že publikujete místní aplikace toobe přístupné přes hello Internetu. S Azure AD Domain Services můžete nyní navýšení a shift starší verze aplikací běžících místní tooAzure infrastruktury služby. Potom můžete publikovat tyto aplikace pomocí hello Azure AD Application Proxy, tooprovide toousers zabezpečený vzdálený přístup ve vaší organizaci.

Pokud jste nový toohello proxy aplikace služby Azure AD, další informace o této funkci s hello následujícího článku: [jak tooprovide zabezpečený vzdálený přístup tooon místní aplikace](../active-directory/active-directory-application-proxy-get-started.md).


## <a name="before-you-begin"></a>Než začnete
úlohy hello tooperform uvedené v tomto článku, budete potřebovat:

1. Platná **předplatné**.
2. **Adresář Azure AD** – buď synchronizovány s místní adresář nebo výhradně cloudový adresář.
3. **Licence Azure AD Basic nebo Premium** je požadovaná toouse hello proxy aplikace služby Azure AD.
4. **Azure AD Domain Services** musí být povolen pro adresář hello Azure AD. Pokud jste tak dosud neučinili, postupujte podle kroků uvedených v hello všechny hello úlohy [příručce Začínáme](active-directory-ds-getting-started.md).

<br>

## <a name="task-1---enable-azure-ad-application-proxy-for-your-azure-ad-directory"></a>Úloha 1 – povolení služby Azure AD Application Proxy pro váš adresář Azure AD
Proveďte následující kroky tooenable hello Azure AD Application Proxy pro váš adresář Azure AD hello.

1. Přihlaste se jako správce v hello [portál Azure](http://portal.azure.com).

2. Klikněte na tlačítko **Azure Active Directory** toobring si přehled directory hello. Klikněte na tlačítko **podnikové aplikace, které**.

    ![Vyberte adresář Azure AD](./media/app-proxy/app-proxy-enable-start.png)
3. Klikněte na tlačítko **proxy aplikace**. Pokud nemáte předplatné Azure AD Basic nebo Azure AD Premium, uvidíte možnost tooenable zkušební verzi. Přepnutí **povolení Proxy aplikace?** příliš**povolit** a klikněte na tlačítko **Uložit**.

    ![Povolit Proxy aplikace](./media/app-proxy/app-proxy-enable-proxy-blade.png)
4. toodownload hello konektor, klikněte na tlačítko hello **konektor** tlačítko.

    ![Stažení konektoru](./media/app-proxy/app-proxy-enabled-download-connector.png)
5. Na stránce pro stažení hello, přijměte licenční podmínky pro hello a smlouvy o ochraně osobních údajů a klikněte na hello **Stáhnout** tlačítko.

    ![Potvrďte stahování](./media/app-proxy/app-proxy-enabled-confirm-download.png)


## <a name="task-2---provision-domain-joined-windows-servers-toodeploy-hello-azure-ad-application-proxy-connector"></a>Úloha 2 – zřídit připojené k doméně Windows servery toodeploy hello Azure AD aplikace konektor Proxy
Je nutné připojený k doméně systému Windows Server virtuálních počítačů na které můžete nainstalovat konektor proxy aplikace služby Azure AD hello. V závislosti na aplikace hello publikovanému můžete se rozhodnout tooprovision více serverů, na kterých je nainstalován konektor hello. Tuto volbu nasazení vám dává větší dostupnosti a pomáhá zpracování větší zátěže pak ověřování.

Přidělení servery konektoru hello na hello stejné virtuální síti (nebo připojení/peered virtuální sítě), ve kterém jste povolili vaší spravované domény služby Azure AD Domain Services. Podobně, musí servery hello hostování aplikací hello publikujete pomocí Proxy aplikace hello toobe nainstalovaná na hello stejné virtuální síti Azure.

servery konektoru tooprovision, postupujte podle uvedených v článku hello s názvem úlohy hello [připojit k spravované doméně systému Windows virtuálního počítače tooa](active-directory-ds-admin-guide-join-windows-vm.md).


## <a name="task-3---install-and-register-hello-azure-ad-application-proxy-connector"></a>Úloha 3 – nainstalujte a zaregistrujte hello konektoru Proxy aplikace služby Azure AD
Dříve zřízení virtuálního počítače s Windows serverem a k ní připojili toohello spravované domény. V této úloze nainstalujete konektor proxy aplikace služby Azure AD hello na tomto virtuálním počítači.

1. Zkopírujte hello konektor instalace balíčku toohello virtuálních počítačů na který nainstalujete konektor Proxy aplikace Azure AD webových hello.

2. Spustit **AADApplicationProxyConnectorInstaller.exe** hello virtuálního počítače. Přijměte licenční podmínky pro software hello.

    ![Přijměte podmínky pro instalaci](./media/app-proxy/app-proxy-install-connector-terms.png)
3. Během instalace jsou výzvami tooregister hello konektor s hello Proxy aplikací z adresáře služby Azure AD.
    * Zadejte vaše **přihlašovací údaje Azure AD globálního správce**. Klient globálního správce se může lišit od vašich přihlašovacích údajů ke službě Microsoft Azure.
    * Hello správce účtu používaného tooregister hello konektor musí patřit toohello stejný adresář, kde jste povolili službu Proxy aplikace hello. Například pokud hello klienta domény contoso.com, dobrý den, Správce musí být admin@contoso.com nebo jiný platný alias v této doméně.
    * Pokud je konfigurace rozšířeného zabezpečení aplikace Internet Explorer je vypnuté pro hello server hello konektor instalujete, mohou být blokovány úvodní obrazovka registrace. tooallow přístup, postupujte podle pokynů hello v hello chybová zpráva. Ujistěte se, že je rozšířené zabezpečení aplikace Internet Explorer vypnuto.
    * Pokud registrace konektoru selže, podívejte se do článku [Poradce při potížích s proxy aplikace](../active-directory/active-directory-application-proxy-troubleshoot.md).

    ![Nainstalovaný konektor](./media/app-proxy/app-proxy-connector-installed.png)
4. konektor hello tooensure funguje správně, spusťte hello Azure AD Application Proxy Connector Poradce při potížích. Měli byste vidět úspěšné sestavy po spuštěné hello Poradce při potížích.

    ![Poradce při potížích s úspěch](./media/app-proxy/app-proxy-connector-troubleshooter.png)
5. Měli byste vidět hello nově nainstalovaný konektor uvedené na stránce proxy aplikace hello v adresáři služby Azure AD.

    ![](./media/app-proxy/app-proxy-connector-page.png)

> [!NOTE]
> Můžete se rozhodnout tooinstall konektory na několika serverech tooguarantee vysoká dostupnost pro ověřování aplikacích publikovaných prostřednictvím hello proxy aplikace služby Azure AD. Proveďte hello stejné kroky uvedené výše tooinstall hello konektoru na jiné servery připojené k tooyour spravované domény.
>
>

## <a name="next-steps"></a>Další kroky
Máte nastavit hello proxy aplikace služby Azure AD a integraci s vaší spravované domény služby Azure AD Domain Services.

* **Migraci virtuálních počítačů aplikace tooAzure:** můžete navýšení a shift aplikace z místní servery tooAzure virtuální počítače připojené k tooyour spravované domény. Díky tomu pomáhá vám zbavit náklady na infrastrukturu hello spuštěných servery místně.

* **Publikování aplikací pomocí proxy aplikace služby Azure AD:** publikování aplikací, které běží v Azure virtuální počítače pomocí hello proxy aplikace služby Azure AD. Další informace najdete v tématu [publikování aplikací pomocí proxy aplikace služby Azure AD](../active-directory/application-proxy-publish-azure-portal.md)


## <a name="deployment-note---publish-iwa-integrated-windows-authentication-applications-using-azure-ad-application-proxy"></a>Poznámka: nasazení - publikování integrované ověřování systému Windows (integrované ověřování systému Windows) aplikací pomocí proxy aplikace služby Azure AD
Umožňují aplikacím tooyour přihlašování pomocí integrovaného ověřování systému Windows (IWA) udělení oprávnění konektory Proxy aplikace tooimpersonate uživatelů a odesílat a přijímat tokeny jejich jménem. Nakonfigurujte omezené delegování protokolu kerberos (použitím KCD) hello konektor toogrant hello požadované oprávnění tooaccess prostředky v hello spravované domény. Pro zvýšení zabezpečení pomocí hello založené na prostředcích použitím KCD mechanismus spravované domény.


### <a name="enable-resource-based-kerberos-constrained-delegation-for-hello-azure-ad-application-proxy-connector"></a>Povolit založené na prostředcích omezené delegování protokolu kerberos pro konektor Proxy aplikace hello Azure AD
konektor Proxy aplikace služby Azure Hello musí být nakonfigurovaný omezeného delegování protokolu kerberos použitím (KCD), takže je možné zosobňovat uživatele na spravované doméně hello. Na spravované doméně služby Azure AD Domain Services nemáte oprávnění správce domény. Proto **tradiční použitím KCD úrovni účtu nelze konfigurovat ve spravované doméně**.

Použít použitím KCD založené na prostředcích, jak je popsáno v tomto [článku](active-directory-ds-enable-kcd.md).

> [!NOTE]
> Je třeba toobe členem skupiny "Administrators AAD řadiče domény" hello, tooadminister hello spravované doméně pomocí rutin prostředí AD PowerShell.
>
>

Použijte hello prostředí PowerShell Get-ADComputer rutiny tooretrieve hello nastavení pro hello počítač, na které hello Azure AD Application Proxy connector nainstalován.
```
$ConnectorComputerAccount = Get-ADComputer -Identity contoso100-proxy.contoso100.com
```

Následně pomocí tooset rutiny Set-ADComputer hello až založené na prostředcích použitím KCD server hello prostředku.
```
Set-ADComputer contoso100-resource.contoso100.com -PrincipalsAllowedToDelegateToAccount $ConnectorComputerAccount
```

Pokud jste nasadili více konektorů Proxy aplikace na vaší spravované domény, je třeba tooconfigure založené na prostředcích použitím KCD pro každou konektor instanci.


## <a name="related-content"></a>Související obsah
* [Azure AD Domain Services – Příručka Začínáme](active-directory-ds-getting-started.md)
* [Nakonfigurovat omezené delegování protokolu Kerberos na spravované doméně](active-directory-ds-enable-kcd.md)
* [Omezené delegování přehled protokolu Kerberos](https://technet.microsoft.com/library/jj553400.aspx)
