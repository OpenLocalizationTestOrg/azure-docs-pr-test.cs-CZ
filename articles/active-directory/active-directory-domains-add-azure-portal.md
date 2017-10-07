---
title: "aaaAdd vaši vlastní doménu, název tooAzure služby Active Directory | Microsoft Docs"
description: "Jak tooadd doménu vaší společnosti názvy tooAzure služby Active Directory a jak tooverify hello název domény."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: d97e57c6-578a-4929-8fb8-42e858a711c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: curtand
ms.openlocfilehash: 88d5f443cd10b098a9a9ffb3137f5e1ca33b6aad
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

Pomocí služby Azure Active Directory (Azure AD), můžete přidat vaše podnikové doméně název tooAzure AD také. Názvy domén můžete mít, že vám organizace používá toodo firmy a uživatele, kteří se přihlašují pomocí firemního názvu domény. Přidání tooAzure název domény hello AD umožňuje tooassign uživatelská jména v adresáři hello, které jsou známé tooyour uživatelů, jako například 'alice@contoso.com. " Hello proces je jednoduchý:

1. Přidejte adresář tooyour název vlastní domény hello
2. Přidání položky DNS pro název domény hello u registrátora názvu domény hello
3. Ověřte hello vlastní název domény ve službě Azure AD

## <a name="how-do-i-add-a-domain-name"></a>Jak přidat název domény?
1. Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je globálním správcem adresáře hello.
2. Vyberte **další služby**, zadejte **Azure Active Directory** v hello textového pole a pak vyberte **Enter**.
   
   ![Správa uživatelů otevírání](./media/active-directory-domains-add-azure-portal/user-management.png)
3. Na hello ***název adresáře*** vyberte **názvy domén**.
4. Na hello  ***název adresáře* -názvy domén** okně, vyberte hello **přidat** příkaz.
   
   ![Výběrem příkazu přidat hello](./media/active-directory-domains-add-azure-portal/add-command.png)
5. Na hello **název domény** okno, zadejte název vlastní domény hello hello pole, jako je například "contoso.com" a potom vyberte **přidat doménu**. Být, že tooinclude hello .com, .net nebo jinou příponu nejvyšší úrovně.
6. Na hello ***domainname*** okno (to znamená, hello okno, které se otevře s nový název domény v hlavě hello), získat informace záznam DNS hello, Azure AD použijete tooverify, že vaše organizace vlastní hello vlastní název domény.
   
   ![získat informace o záznam DNS.](./media/active-directory-domains-add-azure-portal/get-dns-info.png)

Nyní, když jste přidali hello název domény, musí Azure AD ověřit, že vaše organizace vlastní název domény hello. Před Azure AD mohl toto ověření provést, musíte přidat položku DNS hello souboru zóny DNS pro název domény hello. Tato úloha se provádí na webu hello registrátorem názvu domény pro název domény hello.

## <a name="add-hello-dns-entry-at-hello-domain-name-registrar-for-hello-domain"></a>Přidejte položku DNS hello v hello registrátorem názvu domény pro doménu hello
Hello další krok toouse název vlastní domény s Azure AD je souboru zóny DNS hello tooupdate pro doménu hello. To umožňuje tooverify Azure AD, že vaše organizace vlastní hello vlastní název domény.

1. Přihlaste se toohello registrátorem názvu domény pro doménu hello. Pokud nemáte přístup tooupdate hello záznam DNS, požádejte hello osobě nebo týmu, který má tento přístup toocomplete krok 2 a toolet, které znáte, až se dokončí.
2. Soubor zóny DNS hello aktualizace pro hello domény tak, že přidáte položku DNS hello tooyou poskytovaný Azure AD. Tato položka DNS umožňuje službě Azure AD tooverify vlastnictví domény hello. Hello položka DNS neovlivní žádné chování, například směrování pošty nebo webhosting.

Nápovědu k této přidání položky DNS hello číst [pokyny pro přidání položky DNS u oblíbených registrátorů DNS](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## <a name="verify-hello-domain-name-with-azure-ad"></a>Ověřte název domény hello s Azure AD
Po přidání položky DNS hello jste název domény hello připravené tooverify s Azure AD.

Název domény můžete ověřit, až poté, co mají rozšíří hello záznamy DNS. Šíření často trvá jen několik sekund, ale občas může zabrat i hodinu nebo déle. Pokud ověření není možné hello poprvé, zkuste to znovu později.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je globálním správcem adresáře hello.
2. Vyberte **Procházet**, zadejte do textového pole hello správu uživatelů a potom vyberte **Enter**.
   
   ![Správa uživatelů otevírání](./media/active-directory-domains-add-azure-portal/user-management.png)
3. Na hello **Správa uživatelů - názvy domén** okně, vyberte hello název neověřené domény, které chcete tooverify.
4. Na hello ***domainname*** okno (to znamená, hello okno, které se otevře s nový název domény v hlavě hello), vyberte **ověřte** toocomplete hello ověření.

Teď můžete [přiřazovat uživatelská jména, která obsahují vlastní název domény](active-directory-users-create-azure-portal.md).

## <a name="troubleshooting"></a>Řešení potíží
Pokud nemůžete ověřit vlastní název domény, zkuste následující hello. Začneme s hello nejčastějších a pracovní dolů toohello nejméně častým.

1. **Počkejte hodinu**. Záznamy DNS potřebovat toopropagate před Azure AD můžete ověřit hello domény. Může to trvat hodinu i déle.
2. **Ujistěte se, hello nebyl zadán záznam DNS a zda je správný**. Dokončení tohoto kroku na hello webu pro hello registrátorem názvu domény pro doménu hello. Azure AD nelze ověřit název domény hello, pokud hello položky DNS není k dispozici v hello souboru zóny DNS, nebo pokud není přesnou shodu s položky DNS hello této Azure AD k dispozici můžete. Pokud nemáte přístup tooupdate záznamy DNS pro doménu hello u registrátora názvu domény hello, sdílet hello položku DNS s hello osobě nebo týmu ve vaší organizaci, kteří tento přístup a požádejte je záznam DNS tooadd hello.
3. **Odstranit název domény hello z jiného adresáře v Azure AD**. Název domény můžete ověřit jenom v jediném adresáři. Pokud jste název domény dříve ověřili v jiném adresáři, musíte ho odstranit a teprve potom ho můžete ověřit v novém adresáři. čtení toolearn o odstranění názvy domén, [Správa vlastních názvů domén](active-directory-domains-manage-azure-portal.md).    

## <a name="add-more-custom-domain-names"></a>Přidání dalších vlastních názvů domén
Pokud vaše organizace používá více vlastních názvů domén, například "contoso.com" a "contosobank.com", můžete je přidat až tooa maximálně 900 názvů domén. Použijte stejné kroky tohoto článku tooadd jednotlivých názvů domén hello.

## <a name="next-steps"></a>Další kroky
[Správa vlastních názvů domén](active-directory-domains-manage-azure-portal.md)

