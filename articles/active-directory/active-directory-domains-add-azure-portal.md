---
title: "Přidání vlastního názvu domény do Azure Active Directory | Dokumentace Microsoftu"
description: "Postup přidání firemních názvů domén do Azure Active Directory a způsob ověření názvu domény."
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
ms.openlocfilehash: ad72f768add7edc1d34a85c27dc2aa1b4e4b3a50
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="add-a-custom-domain-name-to-azure-active-directory"></a>Přidání vlastního názvu domény do Azure Active Directory
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-domains-add-azure-portal.md)
> * [Portál Azure Classic](active-directory-add-domain.md)
> 

Pomocí služby Azure Active Directory (Azure AD), můžete přidat název vaší firemní domény do Azure AD i. Můžete mít názvy domén, které vám organizace používá pro firmy a uživatele, kteří se přihlašují pomocí firemního názvu domény. Přidání názvu domény do Azure AD umožňuje přiřazovat uživatelská jména v adresáři, které jsou pro uživatele, jako například 'alice@contoso.com. " Proces je jednoduchý:

1. Přidání vlastního názvu domény do adresáře
2. Přidání položky DNS pro název domény u registrátora názvu domény
3. Ověření vlastního názvu domény v Azure AD

## <a name="how-do-i-add-a-domain-name"></a>Jak přidat název domény?
1. Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který je globální správce adresáře.
2. Vyberte **další služby**, zadejte **Azure Active Directory** v textovém poli a potom vyberte **Enter**.
   
   ![Správa uživatelů otevírání](./media/active-directory-domains-add-azure-portal/user-management.png)
3. Na ***název adresáře*** vyberte **názvy domén**.
4. Na  ***název adresáře* -názvy domén** okně, vyberte **přidat** příkaz.
   
   ![Použití příkazu Přidat](./media/active-directory-domains-add-azure-portal/add-command.png)
5. Na **název domény** okno, zadejte název vlastní domény do pole, jako je například "contoso.com" a potom vyberte **přidat doménu**. Nezapomeňte napsat i příponu .com, .net nebo jinou příponu nejvyšší úrovně.
6. Na ***domainname*** okno (to znamená, v okně otevřeném obsahující v názvu nový název domény), získat informace o záznam DNS, využívající Azure AD ověřit, že vaše organizace vlastní vlastní název domény.
   
   ![získat informace o záznam DNS.](./media/active-directory-domains-add-azure-portal/get-dns-info.png)

Teď, když jste název domény přidali, musí Azure AD ověřit, jestli ho vaše organizace vlastní. Aby Azure AD mohl toto ověření provést, musíte přidat položku DNS do souboru zóny DNS pro název domény. To se provádí na webu registrátora názvu domény.

## <a name="add-the-dns-entry-at-the-domain-name-registrar-for-the-domain"></a>Přidání položky DNS pro doménu u registrátora názvu domény
Pokud chcete používat vlastní název domény ve službě Azure AD, dalším krokem je aktualizace souboru zóny DNS pro takovou doménu. Tím povolíte, aby služba Azure AD ověřila, že vaše organizace je vlastníkem vlastního názvu domény.

1. Přihlaste se k registrátorovi názvu domény. Pokud k aktualizaci položky DNS nemáte přístup, požádejte osobu nebo tým, kteří přístup mají, aby dokončili krok 2 a dali vám vědět, až bude hotový.
2. Aktualizujte soubor zóny DNS pro doménu tím, že přidáte položku DNS, kterou jste získali od Azure AD. Tato položka DNS umožňuje službě Azure AD ověřit vlastnictví domény. Položka DNS neovlivní žádné chování, například směrování pošty nebo webhosting.

Nápovědu k přidání položky DNS najdete v článku [Pokyny k přidání položky DNS u oblíbených registrátorů DNS](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## <a name="verify-the-domain-name-with-azure-ad"></a>Ověření názvu domény pomocí služby Azure AD
Po přidání položky DNS jste připraveni na ověření názvu domény pomocí Azure AD.

Název domény můžete ověřit, až poté, co jste rozšíří záznamy DNS. Šíření často trvá jen několik sekund, ale občas může zabrat i hodinu nebo déle. Pokud ověření na první pokus nefunguje, zkuste to později.

1. Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který je globální správce adresáře.
2. Vyberte **Procházet**, zadejte do textového pole správy uživatelů a potom vyberte **Enter**.
   
   ![Správa uživatelů otevírání](./media/active-directory-domains-add-azure-portal/user-management.png)
3. Na **Správa uživatelů - názvy domén** okně, vyberte název neověřené domény, který chcete ověřit.
4. Na ***domainname*** okno (to znamená, v okně otevřeném obsahující v názvu nový název domény), vyberte **ověřte, zda** a dokončete ověření.

Teď můžete [přiřazovat uživatelská jména, která obsahují vlastní název domény](active-directory-users-create-azure-portal.md).

## <a name="troubleshooting"></a>Řešení potíží
Pokud nemůžete vlastní název domény ověřit, zkuste následující postup. Začneme těmi nejběžnějšími a budeme postupovat až k těm nejméně častým.

1. **Počkejte hodinu**. Záznamy DNS se musí nejprve rozšířit a teprve potom může služba Azure AD doménu ověřit. Může to trvat hodinu i déle.
2. **Zkontrolujte, jestli je zadán záznam DNS a jestli je správně**. Tento krok proveďte na webu registrátora názvu domény. Azure AD nemůže ověřit název domény, pokud není záznam DNS k dispozici v souboru zóny DNS, nebo pokud se položka DNS přesně neshoduje s položkou, kterou vám poskytla služba Azure AD. Pokud nemáte přístup k aktualizaci záznamů DNS domény u registrátora názvu domény, poskytněte položku DNS osobě nebo týmu z vaší organizace, kteří tento přístup mají, a požádejte je, aby položku DNS přidali.
3. **Odstraňte název domény z jiného adresáře ve službě Azure AD**. Název domény můžete ověřit jenom v jediném adresáři. Pokud jste název domény dříve ověřili v jiném adresáři, musíte ho odstranit a teprve potom ho můžete ověřit v novém adresáři. Další informace o odstraňování názvů domén najdete v článku [Správa vlastních názvů domén](active-directory-domains-manage-azure-portal.md).    

## <a name="add-more-custom-domain-names"></a>Přidání dalších vlastních názvů domén
Pokud vaše organizace používá několik vlastních názvů domén, například contoso.com a contosobank.com, můžete je přidat všechny (až do maximálního počtu 900 názvů domén). Všechny názvy domén můžete přidat opakováním postupu popsaného v tomto článku.

## <a name="next-steps"></a>Další kroky
[Správa vlastních názvů domén](active-directory-domains-manage-azure-portal.md)

