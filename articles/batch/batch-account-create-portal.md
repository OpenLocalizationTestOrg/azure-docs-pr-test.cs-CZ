---
title: "aaaCreate účtu Batch na portálu Azure hello | Microsoft Docs"
description: "Zjistěte, jak toocreate Azure Batch účet v hello Azure portálu toorun rozsáhlé paralelní úlohy v cloudu hello"
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
ms.openlocfilehash: 2176f88ba0a1a3298023de8f520d46ef28a664b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-batch-account-with-hello-azure-portal"></a>Vytvoření účtu Batch se hello portálu Azure

> [!div class="op_single_selector"]
> * [Azure Portal](batch-account-create-portal.md)
> * [Knihovna Batch Management .NET](batch-management-dotnet.md)
>
>

Zjistěte, jak toocreate Azure Batch účet v hello [portál Azure][azure_portal]a vyberte vlastnosti účtu hello, splňující výpočetního scénáře. Zjistěte, kde jako vlastnosti důležité účtu toofind přístupových klíčů a adresy URL účtu.

Pro informace o účty Batch a scénáře viz hello [funkci přehled](batch-api-basics.md).



## <a name="create-a-batch-account"></a>Vytvoření účtu Batch

Pomocí portálu toocreate hello účtu Batch v jednom z hello dva *fond přidělení režimy*: **služba Batch** režimu nebo hello novější **uživatele předplatné** režimu, který vyžaduje více konfigurace. Informace o těchto dvou režimů najdete v tématu hello [funkci přehled](batch-api-basics.md#account). Funkce hello uživatelského předplatné režimu naleznete také hello [příspěvku na blogu](https://blogs.technet.microsoft.com/windowshpc/2017/03/17/azure-batch-vnet-and-custom-image-support-for-virtual-machine-pools/).

## <a name="batch-service-mode"></a>Režim služby Batch



1. Přihlaste se toohello [portál Azure][azure_portal].
2. Klikněte na **Nový** > **Compute** > **Batch Service**.

    ![Batch v hello Marketplace.][marketplace_portal]
3. Hello **nový účet Batch** zobrazí se okno. V tématu Popis hello níže jednotlivých prvků okna.

    ![Vytvoření účtu Batch][account_portal]

    a. **Název účtu**: název účtu Batch hello zvolíte, musí být jedinečný v rámci hello oblast Azure, kde se má vytvořit účet hello (viz **umístění** níže). název účtu Hello může obsahovat jenom malá písmena nebo číslice a musí být 3 až 24 znaků.

    b. **Předplatné**: hello předplatné, ve které toocreate hello účtu Batch. Pokud máte jenom jedno předplatné, bude ve výchozím nastavení vybrané.

    c. **Režim přidělování fondů:** Vyberte možnost **Služba Batch**.

    c. **Skupina prostředků**: Vyberte existující skupinu prostředků vašeho nového účtu Batch, popřípadě si vytvořte novou.

    d. **Umístění**: hello oblast Azure, ve které toocreate hello účtu Batch. Jako možnosti se zobrazí pouze hello oblasti podporuje vaše předplatné a skupina prostředků.

    e. **Účet úložiště** (volitelné): Účet úložiště Azure pro obecné účely, který přidružíte k účtu Batch. Toto nastavení se doporučuje pro většinu účtů Batch. Další informace najdete v části [Propojený účet Azure Storage](#linked-azure-storage-account) níže v tomto článku.

4. Klikněte na tlačítko **vytvořit** toocreate hello účtu.

   portál Hello označuje, že nasazení právě probíhá. Po dokončení se v části **Oznámení** zobrazí zpráva **Nasazení se podařila**.

## <a name="user-subscription-mode"></a>Režim předplatného uživatele

### <a name="allow-azure-batch-tooaccess-hello-subscription-one-time-operation"></a>Povolit Azure Batch tooaccess hello předplatné (jednorázová operace)
Při vytváření účtu Batch první v uživatelském režimu předplatné, proveďte následující kroky tooregister hello předplatného pomocí služby Batch. (Pokud jste dříve to, přeskočte toohello další části.)

1. Přihlaste se toohello [portál Azure][azure_portal].

2. Klikněte na tlačítko **více služeb** > **odběry**a klikněte na předplatné hello chcete toouse pro hello účtu Batch.

3. V hello **předplatné** okně klikněte na tlačítko **přístup k ovládacímu prvku (IAM)** > **přidat**.

    ![Řízení přístupu pro předplatné][subscription_access]

4. Na hello **přidat oprávnění** okně, vyberte hello **Přispěvatel** role, vyhledejte hello Batch API. Vyhledávání pro každý z těchto řetězců vyhledejte hello rozhraní API:
    1. **MicrosoftAzureBatch**.
    2. **Microsoft Azure Batch**. Novější tenanti služby Azure AD mohou používat tento název.
    3. **ddbf3205-c6bd-46ae-8127-60eb93363864** se hello ID hello Batch API. 

5. Po nalezení hello Batch API, vyberte ho a klikněte na tlačítko **Uložit**.

    ![Přidání oprávnění pro službu Batch][add_permission]

### <a name="create-a-key-vault"></a>Vytvořte trezor klíčů
V uživatelském režimu předplatné, se vyžaduje klíče trezoru služby Azure, patří toothe stejné skupině prostředků jako toobe účtu Batch hello vytvořili. Zkontrolujte, zda je skupina prostředků hello v oblasti, kde je Batch [k dispozici](https://azure.microsoft.com/regions/services/) a které podporuje vaše předplatné.

1. V hello [portál Azure][azure_portal], klikněte na tlačítko **nový** > **zabezpečení a identita** > **Key Vault** .

2. V hello **vytvořit Key Vault** okno, zadejte název trezoru klíčů hello a vytvořte skupinu prostředků v hello oblasti, které chcete použít pro účet Batch. Nechte hello zbývající nastavení na výchozí hodnoty a pak klikněte na **vytvořit**.

### <a name="create-a-batch-account"></a>Vytvoření účtu Batch

1. V hello [portál Azure][azure_portal], klikněte na tlačítko **nový** > **výpočetní** > **služba Batch**.

    ![Batch v hello Marketplace.][marketplace_portal]
3. Hello **nový účet Batch** zobrazí se okno. V tématu Popis hello níže jednotlivých prvků okna.

    ![Vytvoření účtu Batch][account_portal_byos]

    a. **Název účtu**: název účtu Batch hello zvolíte, musí být jedinečný v rámci hello oblast Azure, kde se má vytvořit účet hello (viz **umístění** níže). název účtu Hello může obsahovat jenom malá písmena nebo číslice a musí být 3 až 24 znaků.

    b. **Předplatné**: Pokud máte více než jedno předplatné, vyberte předplatné hello, který jste zaregistrovali hello služby Batch.

    c. **Režim přidělování fondů:** Vyberte **Předplatné uživatele**.

    d. **Trezor klíčů**: Vyberte hello trezoru klíčů jste vytvořili pro účet Batch v předchozí části hello. Volitelně můžete vytvořit nový trezor klíčů. Po výběru hello trezoru, vyberte hello políčko toogrant Azure Batch přístup toohello klíče trezoru.

    c. **Skupina prostředků**: Skupina prostředků vyberte hello, ve které jste vytvořili trezor klíčů hello.

    d. **Umístění**: hello oblast Azure, ve které jste vytvořili trezor klíčů hello pro účet Batch hello.

    e. **Účet úložiště** (volitelné): Účet úložiště Azure pro obecné účely, který přidružíte k účtu Batch. Toto nastavení se doporučuje pro většinu účtů Batch. Další informace najdete v části [Propojený účet Azure Storage](#linked-azure-storage-account) dole.

4. Klikněte na tlačítko **vytvořit** toocreate hello účtu.

   portál Hello označuje, že nasazení právě probíhá. Po dokončení se v části **Oznámení** zobrazí zpráva **Nasazení se podařila**.



## <a name="view-batch-account-properties"></a>Zobrazení vlastností účtu Batch
Po vytvoření účtu hello můžete otevřít hello **okno účtu Batch** tooaccess jeho nastavení a vlastností. Všechna nastavení účtu a vlastnosti mají přístup pomocí hello levé nabídce okna účtu Batch hello.

![Okno účtu Batch na webu Azure Portal][account_blade]

* **Adresa URL účtu batch**: když budete vyvíjet aplikace s hello [rozhraní API služby Batch](batch-apis-tools.md#azure-accounts-for-batch-development), budete potřebovat tooaccess adresa URL účtu prostředky služby Batch. Adresa URL účtu Batch má hello následující formát:

    `https://<account_name>.<region>.batch.azure.com`

![Adresa URL účtu Batch na portálu][account_url]

* **Přístupové klíče** (režim služby Batch): tooyour tooauthenticate přístup k účtu Batch z vaší aplikace, budete potřebovat přístupový klíč účtu. (Toto nastavení není k dispozici v režimu předplatného uživatele, pokud používáte ověřování pomocí Azure Active Directory.)

    tooview nebo znovu vygenerovat přístupové klíče účtu Batch, zadejte `keys` v levé nabídce hello **vyhledávání** pole v okně účtu Batch hello a pak vyberte **klíče**.

    ![Klíče účtu Batch na webu Azure Portal][account_keys]

[!INCLUDE [batch-pricing-include](../../includes/batch-pricing-include.md)]

## <a name="linked-azure-storage-account"></a>Propojený účet Azure Storage

Volitelně můžete propojit pro obecné účely tooyour účtu Azure Storage účtu Batch. Hello [balíčky aplikací](batch-application-packages.md) služby Batch používá úložiště objektů Blob v Azure, stejně jako hello [Batch .NET konvence souboru](batch-task-output.md) knihovny. Tyto volitelné funkce vám pomohou při nasazení aplikace hello spuštěné úkoly služby Batch a zachování hello dat, které vytvářejí.

Doporučujeme vytvořit nový účet úložiště pro výhradní použití vaším účtem Batch.

![Vytvoření účtu úložiště pro obecné účely][storage_account]

> [!NOTE]
> Azure Batch aktuálně podporuje pouze hello obecný typ účtu úložiště. Tento typ účtu je popsaný v kroku 5, [Vytvoření účtu úložiště] (../storage/common/storage-create-storage-account.md#create-a-storage-account), v tématu [O účtech úložiště Azure](../storage/common/storage-create-storage-account.md).
>
>

> [!WARNING]
> Dávejte pozor při opakovaném generování přístupových klíčů hello propojeného účtu úložiště. Obnovit pouze jeden klíč účtu úložiště a klikněte na tlačítko **synchronizace klíčů** na hello propojené okně účtu úložiště. Hello počkejte pět minut tooallow hello klíče toopropagate toohello výpočetních uzlů ve fondech, pak znovu vygenerujte a synchronizujte Další klíč v případě potřeby. Pokud byste znovu generovali oba klíče v hello stejný čas, výpočetní uzly nebudou moct toosynchronize ani jeden klíč a dojde ke ztrátě přístupu účtu úložiště toohello.
>
>

![Obnovování klíčů účtu úložiště][4]

## <a name="batch-service-quotas-and-limits"></a>Kvóty a omezení služby Batch
Potřeba si uvědomit, že jako s předplatným Azure a jiné Azure services, určité [kvóty a omezení](batch-quota-limit.md) použít tooBatch účty. Aktuální kvóty pro účet Batch se zobrazují hello portálu v účtu hello **vlastnosti**.

![Kvóty účtu Batch na webu Azure Portal][quotas]



Kromě toho řada těchto kvót může být zvýšena jednoduše se žádost o podporu volné produktu ve hello portálu Azure. V tématu [kvóty a omezení pro hello služby Azure Batch](batch-quota-limit.md) podrobnosti o žádají o zvýšení kvóty.

## <a name="other-batch-account-management-options"></a>Další možnosti správy účtu Batch
Kromě toho toousing hello portálu Azure, můžete také vytvořit a spravovat účty Batch s hello následující:

* [Rutiny PowerShellu pro účty Batch](batch-powershell-cmdlets-get-started.md)
* [Azure CLI](batch-cli-get-started.md)
* [Knihovna Batch Management .NET](batch-management-dotnet.md)

## <a name="next-steps"></a>Další kroky
* V tématu hello [přehled funkcí Batch](batch-api-basics.md) toolearn Další informace o konceptech služby Batch a funkce. Hello článek popisuje hello primární prostředky služby Batch například fondy, výpočetní uzly, úlohy a úkoly a nabízí přehled hello funkcí služby, které umožňují spouštění velkých výpočetních úloh.
* Další informace hello základy vývoje aplikací s povolenými Batch pomocí hello [klientské knihovny Batch .NET](batch-dotnet-get-started.md) nebo [Python](batch-python-tutorial.md). Tyto články úvodní vás provede funkční aplikaci, která používá tooexecute služby Batch hello zatížení v několika výpočetních uzlech a zahrnuje použití služby Azure Storage pro zatížení přípravě a načítání souborů.

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
