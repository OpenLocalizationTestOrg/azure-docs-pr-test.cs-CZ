---
title: "aaaAdd tooAzure vlastní domény AD | Microsoft Docs"
description: "Vysvětluje, jak tooadd vlastní doménu v Azure Active Directory."
services: active-directory
author: jeffgilb
manager: femila
ms.assetid: 0a90c3c5-4e0e-43bd-a606-6ee00f163038
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: jeffgilb
ms.reviewer: jsnow
ms.custom: it-pro
ms.openlocfilehash: 878cecad364ec47f1c6755d742aaccbce627dc5e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="quickstart-add-a-custom-domain-name-tooazure-active-directory"></a>Rychlé spuštění: Přidání tooAzure název vlastní domény služby Active Directory

Každý adresář Azure AD se dodává s počáteční název domény ve formě hello *domainname*. onmicrosoft.com. hello počáteční název domény nelze změnit ani odstranit, ale můžete přidat vaše podnikové doméně název tooAzure AD také. Vaše organizace například pravděpodobně nemá jiné domény názvů používaných toodo firmy a uživatele, kteří se přihlašují pomocí firemního názvu domény. Přidání vlastní doménu názvy tooAzure AD umožňuje tooassign uživatelská jména v adresáři hello, které jsou známé tooyour uživatelů, jako například 'alice@contoso.com. " místo ' alice @*<domain name>*. onmicrosoft.com ". Hello proces je jednoduchý:

1. Přidejte adresář tooyour název vlastní domény hello
2. Přidání položky DNS pro název domény hello u registrátora názvu domény hello
3. Ověřte hello vlastní název domény ve službě Azure AD

## <a name="add-your-custom-domain"></a>Přidat vaši vlastní doménu.
1. Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je globálním správcem adresáře hello.
2. Vyberte **další služby**, zadejte **Azure Active Directory** v hello textového pole a pak vyberte **Enter**.
   
   ![Správa uživatelů otevírání](./media/active-directory-domains-add-azure-portal/user-management.png)
3. Na hello ***název adresáře*** vyberte **názvy domén**.
4. Na hello  ***název adresáře* -názvy domén** okně, vyberte hello **přidat** příkaz.
   
   ![Výběrem příkazu přidat hello](./media/active-directory-domains-add-azure-portal/add-command.png)
5. Na hello **název domény** okno, zadejte název vlastní domény hello hello pole, jako je například "contoso.com" a potom vyberte **přidat doménu**. Být, že tooinclude hello .com, .net nebo jinou příponu nejvyšší úrovně.
6. Na hello ***název domény*** okno (s vlastní název domény v hlavě hello), získat hello DNS záznam informace toouse tooverify, že vaše organizace vlastní hello vlastní název domény.
   
   ![získat informace o záznam DNS.](./media/active-directory-domains-add-azure-portal/get-dns-info.png)

> [!TIP]
> Pokud máte v plánu toofederate místní Windows Server AD s Azure AD, pak musíte tooselect hello **I tooconfigure plánování tuto doménu pro jednotné přihlašování s Moje místní služby Active Directory** políčko při spuštění nástroje hello Azure AD Connect toosynchronize adresářů. Musíte taky tooregister hello stejným názvem domény, vyberte pro federaci s místní adresáře hello **Azure AD Domain** krok v Průvodci hello. Uvidíte, jaké tohoto kroku v Průvodci hello vypadá jako [v těchto pokynech](./connect/active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation). Pokud jste nástroj hello Azure AD Connect, můžete [ho stáhnout zde](http://go.microsoft.com/fwlink/?LinkId=615771).

Nyní, když jste přidali hello název domény, musí Azure AD ověřit, že vaše organizace vlastní název domény hello. Před Azure AD mohl toto ověření provést, musíte přidat položku DNS hello souboru zóny DNS pro název domény hello. Tato úloha se provádí na webu hello registrátorem názvu domény pro název domény hello.

## <a name="add-hello-dns-entry-at-hello-domain-name-registrar-for-hello-domain"></a>Přidejte položku DNS hello v hello registrátorem názvu domény pro doménu hello
Hello další krok toouse název vlastní domény s Azure AD je souboru zóny DNS hello tooupdate pro doménu hello. Azure AD můžete pak ověřte, že vaše organizace vlastní hello vlastní název domény.

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
4. Na hello ***název domény*** okno (to znamená, hello okno, které se otevře s nový název domény v hlavě hello), vyberte **ověřte** toocomplete hello ověření.

> [!TIP]
> Můžete přidat až too900 vlastních názvů domén, ale může být pouze jeden [nastavit jako název hello primární domény pro váš adresář Azure AD](active-directory-domains-manage-azure-portal.md#set-the-primary-domain-name-for-your-azure-ad-directory) používá ve výchozím nastavení se při vytváření nové účty.

Nyní můžete vytvořit cloudový uživatelské účty nebo aktualizace dříve synchronizovaných položek místní informace o uživatelském účtu, pomocí vlastního názvu domény. Můžete také upravit informace o dříve synchronizovaných uživatelského účtu domény příponu pomocí [Microsoft PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) nebo hello [rozhraní Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations).

## <a name="troubleshooting"></a>Řešení potíží
Pokud nemůžete ověřit vlastní název domény, zkuste následující kroky řešení potíží hello:

1. **Počkejte hodinu**. Záznamy DNS potřebovat toopropagate před Azure AD můžete ověřit hello domény. Může to trvat hodinu i déle.
2. **Ujistěte se, hello nebyl zadán záznam DNS a zda je správný**. Dokončení tohoto kroku na hello webu pro hello registrátorem názvu domény pro doménu hello. Azure AD nelze ověřit název domény hello, pokud hello položky DNS není k dispozici v hello souboru zóny DNS, nebo pokud není přesnou shodu s položky DNS hello této Azure AD k dispozici můžete. Pokud nemáte přístup tooupdate záznamy DNS pro doménu hello u registrátora názvu domény hello, sdílet hello položku DNS s hello osobě nebo týmu ve vaší organizaci, kteří tento přístup a požádejte je záznam DNS tooadd hello.
3. **Odstranit název domény hello z jiného adresáře v Azure AD**. Název domény můžete ověřit jenom v jediném adresáři. Pokud jste název domény dříve ověřili v jiném adresáři, musíte ho odstranit a teprve potom ho můžete ověřit v novém adresáři. čtení toolearn o odstranění názvy domén, [Správa vlastních názvů domén](active-directory-domains-manage-azure-portal.md).    

## <a name="add-more-custom-domain-names"></a>Přidání dalších vlastních názvů domén
Pokud vaše organizace používá více než jeden vlastní název domény, jako je například "contoso.com" a "contosobank.com", můžete přidat až too900 více pro každou opakováním hello kroků v tomto článku.

### <a name="learn-more"></a>Další informace
[Koncepční přehled vlastních názvů domén ve službě Azure AD](active-directory-add-domain-concepts.md)

[Správa vlastních názvů domén](active-directory-domains-manage-azure-portal.md)


## <a name="next-steps"></a>Další kroky
V tento rychlý start, když jste se naučili jak tooadd tooAzure vlastní domény AD. 

Můžete použít hello následujících odkaz tooadd nové vlastní domény ve službě Azure AD z hello portálu Azure.

> [!div class="nextstepaction"]
> [Přidat vlastní doménu](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/QuickStart) 