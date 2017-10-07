---
title: "aaaAdd vlastní název domény a nastavení federované přihlašování tooAzure služby Active Directory | Microsoft Docs"
description: "Jak tooadd vaší společnosti názvů domén služby Active Directory tooset tooAzure až federované přihlašování mezi Azure Active Directory a vaše místní řešení federační"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 27126c7e-e6d6-4ef3-a4fb-f5f0706e749d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: curtand
ms.openlocfilehash: 77f8cc87c27871ac96581360762aaa8d24c0eb8d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-your-custom-domain-name-tooazure-active-directory"></a>Přidejte název tooAzure vaši vlastní doménu služby Active Directory
Můžete si nakonfigurovat vlastní název domény, například contoso.com, aby uživatelé webu contoso.com mohli využívat jednotné federované přihlašování z podnikové sítě. Pokud již máte služby Active Directory Federation Services (AD FS) nebo jiné federační server provozovaný ve vaší podnikové síti, můžete konfigurovat Azure AD toouse vlastního názvu domény pomocí nástroje Azure AD Connect hello. Můžete také použít Azure AD Connect toodeploy nové služby AD FS prostředí a nakonfigurujte pro federované jeden přihlašování tooAzure AD.

Pokud nemáte k dispozici a nemáte v plánu toodeploy služby AD FS nebo jiné federační server, postupujte podle těchto pokynů: [přidat tooAzure název vlastní domény služby Active Directory](active-directory-add-domain.md).

## <a name="add-a-custom-domain-name-tooyour-directory"></a>Přidejte adresář tooyour název vlastní domény
1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com/) pomocí uživatelského účtu, který je globálním správcem adresáře Azure AD.
2. V **služby Active Directory**, otevřete svůj adresář a vyberte hello **domén** kartě.
3. Na panelu příkazů hello, vyberte **přidat**a pak zadejte název vlastní domény, jako je například "contoso.com" hello. Být, že tooinclude hello .com, .net nebo jinou příponu nejvyšší úrovně.
4. Vyberte hello **I tooconfigure plánování tuto doménu pro jednotné přihlašování s Moje místní služby Active Directory** zaškrtávací políčko.
5. Vyberte **Přidat**.

Spustit hello Azure AD Connect nástroj tooget hello položky DNS použije tento Azure AD tooverify hello domény. Zobrazí se položka DNS hello v hello **Azure AD Domain** krok v Průvodci hello. Uvidíte, jaké tohoto kroku v Průvodci hello vypadá jako [v těchto pokynech](connect/active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation). Pokud jste nástroj hello Azure AD Connect, můžete [ho stáhnout zde](http://go.microsoft.com/fwlink/?LinkId=615771).

## <a name="add-hello-dns-entry-at-hello-domain-name-registrar-for-hello-domain"></a>Přidejte položku DNS hello v hello registrátorem názvu domény pro doménu hello
Hello další krok toouse název vlastní domény s Azure AD je souboru zóny DNS hello tooupdate pro doménu hello. To umožňuje tooverify Azure AD, že vaše organizace vlastní hello vlastní název domény.

1. Přihlaste se na web toohello pro registrátorem názvu domény pro název domény. Pokud nemáte přístup toodo to, požádejte hello osobě nebo týmu ve vaší organizaci, kteří tento přístup toocomplete krok 2 a toolet, které znáte, až se dokončí.
2. Soubor zóny DNS hello aktualizace pro hello domény tak, že přidáte položku DNS hello tooyou poskytovaný Azure AD. Tato položka DNS umožňuje službě Azure AD tooverify vlastnictví domény hello. Hello položka DNS neovlivní žádné chování, například směrování pošty nebo webhosting.

Nápovědu k tomuto kroku najdete v článku [Pokyny k přidání položky DNS u oblíbených registrátorů DNS](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## <a name="verify-hello-domain-name-with-azure-ad"></a>Ověřte název domény hello s Azure AD
Po přidání položky DNS hello jste název domény hello připravené tooverify s Azure AD.

tooverify hello domény, vyberte **Další** na hello **Azure AD Domain** kroku průvodce hello Azure AD Connect. Azure AD bude hledat položky DNS hello v hello souboru zóny DNS pro doménu hello. Azure AD ověřit název domény hello jenom, jakmile mají rozšíří hello záznamy DNS. Šíření často trvá jen několik sekund, ale občas může zabrat hodinu nebo déle. Pokud ověření není možné hello poprvé, zkuste to znovu později.

Potom pokračujte hello zbývajících kroků v Průvodci hello Azure AD Connect. Tím se synchronizují uživatelů z vašeho systému Windows Server AD tooAzure AD. Synchronizovaní uživatelé v doméně hello, který jste nakonfigurovali pro federaci bude možné tooget federované jednotné přihlašování v prostředí z vaší podnikové síti tooAzure AD.

## <a name="troubleshooting"></a>Řešení potíží
Pokud nemůžete ověřit vlastní název domény, zkuste následující hello. Začneme s hello nejčastějších a pracovní dolů toohello nejméně častým.

1. **Počkejte hodinu**. Záznamy DNS potřebovat toopropagate před Azure AD můžete ověřit hello domény. Může to trvat hodinu i déle.
2. **Ujistěte se, hello nebyl zadán záznam DNS a zda je správný**. Dokončení tohoto kroku na hello webu pro hello registrátorem názvu domény pro doménu hello. Azure AD nelze ověřit název domény hello, pokud hello položky DNS není k dispozici v hello souboru zóny DNS, nebo pokud není přesnou shodu s položky DNS hello této Azure AD k dispozici můžete. Pokud nemáte přístup tooupdate záznamy DNS pro doménu hello u registrátora názvu domény hello, sdílet hello položku DNS s hello osobě nebo týmu ve vaší organizaci, kteří tento přístup a požádejte je záznam DNS tooadd hello.
3. **Odstranit název domény hello z jiného adresáře v Azure AD**. Název domény můžete ověřit jenom v jediném adresáři. Pokud jste název domény dříve ověřili v jiném adresáři, musíte ho odstranit a teprve potom ho můžete ověřit v novém adresáři. čtení toolearn o odstranění názvy domén, [Správa vlastních názvů domén](active-directory-add-manage-domain-names.md).

## <a name="add-more-custom-domain-names"></a>Přidání dalších vlastních názvů domén
Pokud vaše organizace používá více vlastních názvů domén, například "contoso.com" a "contosobank.com", můžete je přidat až tooa maximálně 900 názvů domén. Použijte stejné kroky tohoto článku tooadd jednotlivých názvů domén hello.

## <a name="next-steps"></a>Další kroky
* [Správa vlastních názvů domén](active-directory-add-manage-domain-names.md)
* [Další informace o konceptech správy domén ve službě Azure AD](active-directory-add-domain-concepts.md)
* [Zobrazení firemního brandingu během přihlašování uživatelů](active-directory-add-company-branding.md)
* [Pomocí názvů domén toomanage prostředí PowerShell ve službě Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)

