---
title: "Vytvoření účtu Batch na webu Azure Portal | Dokumentace Microsoftu"
description: "Naučte se vytvořit účet Azure Batch na webu Azure Portal, abyste mohli spouštět velké paralelní úlohy v cloudu."
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: 3fbae545-245f-4c66-aee2-e25d7d5d36db
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/20/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 520d1d42d35b25db1a35d4317e9eb616cf5de565
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-batch-account-with-the-azure-portal"></a>Vytvoření účtu Batch pomocí webu Azure Portal

> [!div class="op_single_selector"]
> * [Azure Portal](batch-account-create-portal.md)
> * [Knihovna Batch Management .NET](batch-management-dotnet.md)
>
>

Naučte se vytvořit účet Azure Batch na webu [Azure Portal][azure_portal] a zvolit vlastnosti účtu odpovídající výpočetnímu scénáři. Zjistěte, kde najít důležité vlastnosti účtu, jako jsou přístupové klíče a adresy URL účtu.

Informace o scénářích a účtech Batch najdete v [přehledu funkcí](batch-api-basics.md).



## <a name="create-a-batch-account"></a>Vytvoření účtu Batch

Prostřednictvím portálu můžete vytvořit účet Batch v jednom ze dvou *režimu přidělování fondů*: v režimu **služby Batch** nebo v novějším režimu **předplatného uživatele**, který vyžaduje další konfiguraci. Informace o těchto dvou režimech najdete v [přehledu funkcí](batch-api-basics.md#account). Funkce režimu předplatného uživatele najdete také v [příspěvku na blogu](https://blogs.technet.microsoft.com/windowshpc/2017/03/17/azure-batch-vnet-and-custom-image-support-for-virtual-machine-pools/).

## <a name="batch-service-mode"></a>Režim služby Batch



1. Přihlaste se na web [Azure Portal][azure_portal].
2. Klikněte na **Nový** > **Compute** > **Batch Service**.

    ![Batch na webu Marketplace][marketplace_portal]
3. Zobrazí se okno **Nový účet Batch**. Níže jsou uvedeny popisy jednotlivých prvků okna.

    ![Vytvoření účtu Batch][account_portal]

    a. **Název účtu:** Název účtu Batch, který zvolíte, musí být jedinečný v oblasti Azure, ve které se nový účet vytvoří (viz **Umístění** níže). Název účtu může obsahovat jenom malá písmena nebo čísla a musí být dlouhý 3 až 24 znaků.

    b. **Předplatné**: Předplatné, ve kterém chcete účet Batch vytvořit. Pokud máte jenom jedno předplatné, bude ve výchozím nastavení vybrané.

    c. **Režim přidělování fondů:** Vyberte možnost **Služba Batch**.

    c. **Skupina prostředků**: Vyberte existující skupinu prostředků vašeho nového účtu Batch, popřípadě si vytvořte novou.

    d. **Umístění**: Oblast Azure, ve které chcete účet Batch vytvořit. Jako možnosti se zobrazí jenom oblasti, které podporuje vaše předplatné a skupina prostředků.

    e. **Účet úložiště** (volitelné): Účet úložiště Azure pro obecné účely, který přidružíte k účtu Batch. Toto nastavení se doporučuje pro většinu účtů Batch. Další informace najdete v části [Propojený účet Azure Storage](#linked-azure-storage-account) níže v tomto článku.

4. Kliknutím na **Vytvořit** vytvořte účet.

   Na portálu se zobrazí zpráva o tom, že nasazování probíhá. Po dokončení se v části **Oznámení** zobrazí zpráva **Nasazení se podařila**.

## <a name="user-subscription-mode"></a>Režim předplatného uživatele

### <a name="allow-azure-batch-to-access-the-subscription-one-time-operation"></a>Povolení přístupu k předplatnému pro Azure Batch (jednorázová operace)
Při vytváření prvního účtu Batch v režimu předplatného uživatele proveďte následující kroky, abyste předplatné zaregistrovali ve službě Batch. (Pokud jste tento postup již provedli, přejděte k další části.)

1. Přihlaste se na web [Azure Portal][azure_portal].

2. Klikněte na **Další služby** > **Předplatné** a klikněte na předplatné, které chcete pro účet Batch použít.

3. V okně **Předplatné** klikněte na **Řízení přístupu (IAM)** > **Přidat**.

    ![Řízení přístupu pro předplatné][subscription_access]

4. V okně **Přidejte oprávnění** vyberte roli **Přispěvatel** a vyhledejte rozhraní API služby Batch. Hledejte každý z těchto řetězců, dokud nenajdete rozhraní API:
    1. **MicrosoftAzureBatch**.
    2. **Microsoft Azure Batch**. Novější tenanti služby Azure AD mohou používat tento název.
    3. **ddbf3205-c6bd-46ae-8127-60eb93363864** je ID rozhraní API služby Batch. 

5. Jakmile najdete rozhraní API služby Batch, vyberte ho a klikněte na **Uložit**.

    ![Přidání oprávnění pro službu Batch][add_permission]

### <a name="create-a-key-vault"></a>Vytvořte trezor klíčů
V režimu předplatného uživatele je vyžadován trezor klíčů Azure, který patří do stejné skupiny prostředků jako účet Batch, který se má vytvořit. Ověřte, že se skupina prostředků nachází v oblasti, kde je služba Batch [k dispozici](https://azure.microsoft.com/regions/services/) a která podporuje vaše předplatné.

1. Na webu [Azure Portal][azure_portal] klikněte na **Nový** > **Zabezpečení + identita** > **Trezor klíčů**.

2. V okně **Vytvořit trezor klíčů** zadejte název pro trezor klíčů a vytvořte skupinu prostředků v oblasti, kterou chcete použít pro účet Batch. Pro zbývající nastavení ponechejte výchozí hodnoty a pak klikněte na **Vytvořit**.

### <a name="create-a-batch-account"></a>Vytvoření účtu Batch

1. Na webu [Azure Portal][azure_portal] klikněte na **Nový** > **Compute** > **Služba Batch**.

    ![Batch na webu Marketplace][marketplace_portal]
3. Zobrazí se okno **Nový účet Batch**. Níže jsou uvedeny popisy jednotlivých prvků okna.

    ![Vytvoření účtu Batch][account_portal_byos]

    a. **Název účtu:** Název účtu Batch, který zvolíte, musí být jedinečný v oblasti Azure, ve které se nový účet vytvoří (viz **Umístění** níže). Název účtu může obsahovat jenom malá písmena nebo čísla a musí být dlouhý 3 až 24 znaků.

    b. **Předplatné:** Máte-li více předplatných, vyberte předplatné, které jste zaregistrovali ve službě Batch.

    c. **Režim přidělování fondů:** Vyberte **Předplatné uživatele**.

    d. **Trezor klíčů:** Vyberte trezor klíčů, který jste vytvořili pro účet Batch v předchozí části. Volitelně můžete vytvořit nový trezor klíčů. Po výběru trezoru zaškrtnutím políčka udělte službě Azure Batch přístup k trezoru klíčů.

    c. **Skupina prostředků:** Vyberte skupinu prostředků, ve které jste vytvořili trezor klíčů.

    d. **Umístění:** Oblast Azure, ve které jste vytvořili trezor klíčů pro účet Batch.

    e. **Účet úložiště** (volitelné): Účet úložiště Azure pro obecné účely, který přidružíte k účtu Batch. Toto nastavení se doporučuje pro většinu účtů Batch. Další informace najdete v části [Propojený účet Azure Storage](#linked-azure-storage-account) dole.

4. Kliknutím na **Vytvořit** vytvořte účet.

   Na portálu se zobrazí zpráva o tom, že nasazování probíhá. Po dokončení se v části **Oznámení** zobrazí zpráva **Nasazení se podařila**.



## <a name="view-batch-account-properties"></a>Zobrazení vlastností účtu Batch
Po vytvoření účtu můžete otevřít **okno účtu Batch** pro přístup k jeho nastavením a vlastnostem. Všechna nastavení a vlastnosti účtu jsou přístupná pomocí levé nabídky okna účtu Batch.

![Okno účtu Batch na webu Azure Portal][account_blade]

* **Adresa URL účtu Batch**: Při vývoji aplikace pomocí [rozhraní API služby Batch](batch-apis-tools.md#azure-accounts-for-batch-development) budete pro přístup k prostředkům Batch potřebovat adresu URL účtu. Adresa URL účtu Batch má následující formát:

    `https://<account_name>.<region>.batch.azure.com`

![Adresa URL účtu Batch na portálu][account_url]

* **Přístupové klíče** (režim služby Batch): Pro ověření přístupu k účtu Batch z vaší aplikace budete potřebovat přístupový klíč účtu. (Toto nastavení není k dispozici v režimu předplatného uživatele, pokud používáte ověřování pomocí Azure Active Directory.)

    Chcete-li zobrazit nebo obnovit přístupový klíč účtu Batch, zadejte `keys` do pole **Hledat** v levé nabídce v okně účtu Batch a vyberte položku **Klíče**.

    ![Klíče účtu Batch na webu Azure Portal][account_keys]

[!INCLUDE [batch-pricing-include](../../includes/batch-pricing-include.md)]

## <a name="linked-azure-storage-account"></a>Propojený účet Azure Storage

Volitelně můžete s účtem Batch propojit účet úložiště Azure pro obecné účely. Funkce [balíčků aplikací](batch-application-packages.md) služby Batch využívá úložiště objektů blob v Azure, stejně jako knihovna [.NET Batch File Conventions](batch-task-output.md). Tyto volitelné funkce vám pomohou při nasazení aplikací spouštěných vašimi úkoly Batch a při zachování jimi vytvářených dat.

Doporučujeme vytvořit nový účet úložiště pro výhradní použití vaším účtem Batch.

![Vytvoření účtu úložiště pro obecné účely][storage_account]

> [!NOTE]
> Azure Batch momentálně podporuje pouze typ účtu úložiště pro obecné účely. Tento typ účtu je popsaný v kroku 5, [Vytvoření účtu úložiště] (../storage/common/storage-create-storage-account.md#create-a-storage-account), v tématu [O účtech úložiště Azure](../storage/common/storage-create-storage-account.md).
>
>

> [!WARNING]
> Při obnovování přístupových klíčů v propojeném účtu úložiště buďte velmi opatrní. Obnovte vždy jen jeden klíč účtu úložiště a v okně propojeného účtu úložiště klikněte na možnost **Synchronizovat klíče**. Počkejte 5 minut, aby se klíče rozšířily do výpočetních uzlů ve fondech, a potom v případě potřeby znovu vygenerujte a synchronizujte další klíč. Pokud byste obnovili (znovu vygenerovali) oba klíče najednou, výpočetní uzly by nedokázaly synchronizovat ani jeden klíč a vy byste ztratili přístup k účtu úložiště.
>
>

![Obnovování klíčů účtu úložiště][4]

## <a name="batch-service-quotas-and-limits"></a>Kvóty a omezení služby Batch
Pamatujte, že podobně jako pro předplatné a další služby Azure, i pro účty Batch platí určité [kvóty a omezení](batch-quota-limit.md). Aktuální kvóty účtu Batch se zobrazují na portálu ve **vlastnostech** účtu.

![Kvóty účtu Batch na webu Azure Portal][quotas]



Mnohé z těchto kvót je také možné zvýšit jednoduchým požadavkem na bezplatnou podporu, odeslaným pomocí webu Azure Portal. Podrobnosti o vyžádání zvýšení kvóty viz [Kvóty a omezení pro službu Azure Batch](batch-quota-limit.md).

## <a name="other-batch-account-management-options"></a>Další možnosti správy účtu Batch
Kromě webu Azure Portal můžete účty Batch vytvářet a spravovat následujícími způsoby:

* [Rutiny PowerShellu pro účty Batch](batch-powershell-cmdlets-get-started.md)
* [Azure CLI](batch-cli-get-started.md)
* [Knihovna Batch Management .NET](batch-management-dotnet.md)

## <a name="next-steps"></a>Další kroky
* Další informace o koncepcích a funkcích služby Batch najdete v článku [Přehled funkcí Batch](batch-api-basics.md). Článek popisuje primární prostředky služby Batch, například fondy, výpočetní uzly a úlohy a nabízí přehled funkcí služby, které umožňují spouštění velkých výpočetních úloh.
* Seznamte se se základy vývoje aplikací s podporou služby Batch pomocí [klientské knihovny Batch .NET](batch-dotnet-get-started.md) nebo [Pythonu](batch-python-tutorial.md). Tyto úvodní články vás provedou funkční aplikací, která používá službu Batch ke spouštění úlohy na několika výpočetních uzlech, a představí vám použití služby Azure Storage k přípravě a načítání souborů úloh.

[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx

[azure_portal]: https://portal.azure.com
[batch_pricing]: https://azure.microsoft.com/pricing/details/batch/

[4]: ./media/batch-account-create-portal/batch_acct_04.png "Obnovování klíčů účtu úložiště"
[marketplace_portal]: ./media/batch-account-create-portal/marketplace_batch.PNG
[account_blade]: ./media/batch-account-create-portal/batch_blade.png
[account_portal]: ./media/batch-account-create-portal/batch_acct_portal.png
[account_keys]: ./media/batch-account-create-portal/account_keys.PNG
[account_url]: ./media/batch-account-create-portal/account_url.png
[storage_account]: ./media/batch-account-create-portal/storage_account.png
[quotas]: ./media/batch-account-create-portal/quotas.png
[subscription_access]: ./media/batch-account-create-portal/subscription_iam.png
[add_permission]: ./media/batch-account-create-portal/add_permission.png
[account_portal_byos]: ./media/batch-account-create-portal/batch_acct_portal_byos.png
