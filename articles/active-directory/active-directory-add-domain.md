---
title: "aaaAdd vaši vlastní doménu, název tooAzure služby Active Directory | Microsoft Docs"
description: "Jak tooadd doménu vaší společnosti názvy tooAzure služby Active Directory a jak tooverify hello název domény."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 35a6e20a-9907-432b-9d36-16b916a5c249
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: eb208138f2633aaecc54f68dc947caf80d856d23
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-custom-domain-name-tooazure-active-directory"></a>Přidat tooAzure název vlastní domény služby Active Directory
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-domains-add-azure-portal.md)
> * [Portál Azure Classic](active-directory-add-domain.md)
> 
> 

Máte k dispozici jeden nebo více názvů domén, že vaše organizace používá toodo obchodní, a vaši uživatelé přihlásí tooyour podnikové síti pomocí firemního názvu domény. Teď, když používáte Azure Active Directory (Azure AD), můžete přidat vaše podnikové doméně název tooAzure AD také. To vám umožní tooassign uživatelská jména v adresáři hello, které jsou známé tooyour uživatelů, jako například 'alice@contoso.com. " Hello proces je jednoduchý:

1. Přidejte adresář tooyour název vlastní domény hello
2. Přidání položky DNS pro název domény hello u registrátora názvu domény hello
3. Ověřte hello vlastní název domény ve službě Azure AD

> [!IMPORTANT]
> Společnost Microsoft doporučuje, která můžete spravovat Azure AD pomocí hello [centra pro správu Azure AD](https://aad.portal.azure.com) v hello hello portál Azure místo použití portálu Azure classic, kterou se odkazuje v tomto článku. Pro jak tooadd název domény vaší společnosti v Centru pro správu hello Azure AD, najdete v části [přiřazení rolí správce v Azure Active Directory](active-directory-domains-add-azure-portal.md).

Pokud máte v plánu tooconfigure vaše vlastní doména název toobe používat s Active Directory Federation Services (AD FS) nebo jiné služby tokenů zabezpečení (STS) ve vaší podnikové síti, postupujte podle pokynů hello v [přidat a nakonfigurovat pro doménu federace se službou Azure Active Directory](active-directory-add-domain-federated.md). To je užitečné, pokud máte v plánu toosynchronize uživatelé z vaší podnikové adresáře tooAzure AD, a [synchronizace hodnot hash hesel](active-directory-aadconnectsync-implement-password-synchronization.md) nesplňuje vaše požadavky.

## <a name="add-a-custom-domain-name-tooyour-directory"></a>Přidejte adresář tooyour název vlastní domény
1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com/) pomocí uživatelského účtu, který je globálním správcem adresáře Azure AD.
2. V **služby Active Directory**, otevřete svůj adresář a vyberte hello **domén** kartě.
3. Na panelu příkazů hello, vyberte **přidat**. Zadejte název vlastní domény, jako je například "contoso.com" hello. Zkontrolujte, že tooinclude hello .com, .net nebo jinou příponu nejvyšší úrovně a nechte hello políčko "single sign-on" Vymazat (federační).
4. Vyberte **Přidat**.
5. Na druhé stránce hello Průvodce přidáním domény hello získáte hello záznam DNS, Azure AD použijete tooverify, že vaše organizace vlastní hello vlastní název domény.

Nyní, když jste přidali hello název domény, musí Azure AD ověřit, že vaše organizace vlastní název domény hello. Před Azure AD mohl toto ověření provést, musíte přidat položku DNS hello souboru zóny DNS pro název domény hello. Tato úloha se provádí na webu hello registrátorem názvu domény pro název domény hello.

## <a name="add-hello-dns-entry-at-hello-domain-name-registrar-for-hello-domain"></a>Přidejte položku DNS hello v hello registrátorem názvu domény pro doménu hello
Hello další krok toouse název vlastní domény s Azure AD je souboru zóny DNS hello tooupdate pro doménu hello. To umožňuje tooverify Azure AD, že vaše organizace vlastní hello vlastní název domény.

1. Přihlaste se toohello registrátorem názvu domény pro doménu hello. Pokud nemáte přístup tooupdate hello záznam DNS, požádejte hello osobě nebo týmu, který má tento přístup toocomplete krok 2 a toolet, které znáte, až se dokončí.
2. Soubor zóny DNS hello aktualizace pro hello domény tak, že přidáte položku DNS hello tooyou poskytovaný Azure AD. Tato položka DNS umožňuje službě Azure AD tooverify vlastnictví domény hello. Hello položka DNS neovlivní žádné chování, například směrování pošty nebo webhosting.

Nápovědu k této přidání položky DNS hello číst [pokyny pro přidání položky DNS u oblíbených registrátorů DNS](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## <a name="verify-hello-domain-name-with-azure-ad"></a>Ověřte název domény hello s Azure AD
Po přidání položky DNS hello jste název domény hello připravené tooverify s Azure AD.

Pokud máte ještě hello **přidáním domény** Průvodce otevřený, vyberte možnost **ověřte** na třetí stránce průvodce hello hello. Když vyberete **ověřte**, Azure AD bude hledat položky DNS hello v hello souboru zóny DNS pro doménu hello. Azure AD můžete ověřit název domény hello až poté, co mají rozšíří hello záznamy DNS. Šíření často trvá jen několik sekund, ale občas může zabrat i hodinu nebo déle. Pokud ověření není možné hello poprvé, zkuste to znovu později.

Pokud hello **přidáním domény** Průvodce zavřený, můžete ověřit doménu hello v hello [portál Azure classic](https://manage.windowsazure.com/):

1. Přihlaste se pomocí uživatelského účtu, který je globálním správcem adresáře Azure AD.
2. Otevřete váš adresář a zaškrtněte možnost hello **domén** kartě.
3. Název domény vyberte hello má tooverify a vyberte **ověřte** na panelu příkazů hello.
4. Vyberte **ověřte** v hello dialogové okno pole toocomplete hello ověření.

Teď můžete [přiřazovat uživatelská jména, která obsahují vlastní název domény](active-directory-add-domain-add-users.md).

## <a name="troubleshooting"></a>Řešení potíží
Pokud nemůžete ověřit vlastní název domény, zkuste následující hello. Začneme s hello nejčastějších a pracovní dolů toohello nejméně častým.

1. **Počkejte hodinu**. Záznamy DNS potřebovat toopropagate před Azure AD můžete ověřit hello domény. Může to trvat hodinu i déle.
2. **Ujistěte se, hello nebyl zadán záznam DNS a zda je správný**. Dokončení tohoto kroku na hello webu pro hello registrátorem názvu domény pro doménu hello. Azure AD nelze ověřit název domény hello, pokud hello položky DNS není k dispozici v hello souboru zóny DNS, nebo pokud není přesnou shodu s položky DNS hello této Azure AD k dispozici můžete. Pokud nemáte přístup tooupdate záznamy DNS pro doménu hello u registrátora názvu domény hello, sdílet hello položku DNS s hello osobě nebo týmu ve vaší organizaci, kteří tento přístup a požádejte je záznam DNS tooadd hello.
3. **Odstranit název domény hello z jiného adresáře v Azure AD**. Název domény můžete ověřit jenom v jediném adresáři. Pokud jste název domény dříve ověřili v jiném adresáři, musíte ho odstranit a teprve potom ho můžete ověřit v novém adresáři. čtení toolearn o odstranění názvy domén, [Správa vlastních názvů domén](active-directory-add-manage-domain-names.md).

## <a name="add-more-custom-domain-names"></a>Přidání dalších vlastních názvů domén
Pokud vaše organizace používá více vlastních názvů domén, například "contoso.com" a "contosobank.com", můžete je přidat až tooa maximálně 900 názvů domén. Použijte stejné kroky tohoto článku tooadd jednotlivých názvů domén hello.

## <a name="next-steps"></a>Další kroky
* [Přiřazení uživatelských jmen, která obsahují vlastní název domény](active-directory-add-domain-add-users.md)
* [Správa vlastních názvů domén](active-directory-add-manage-domain-names.md)
* [Další informace o konceptech správy domén ve službě Azure AD](active-directory-add-domain-concepts.md)
* [Zobrazení firemního brandingu během přihlašování uživatelů](active-directory-add-company-branding.md)
* [Pomocí názvů domén toomanage prostředí PowerShell ve službě Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)

