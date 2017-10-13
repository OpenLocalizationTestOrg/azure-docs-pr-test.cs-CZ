---
title: "Začínáme se službou Azure Batch pomocí příkazového řádku Azure CLI | Dokumentace Microsoftu"
description: "Rychlý úvod k příkazům Batch v rozhraní příkazového řádku Azure CLI pro správu prostředků služby Azure Batch"
services: batch
documentationcenter: 
author: v-dotren
manager: timlt
editor: 
ms.assetid: fcd76587-1827-4bc8-a84d-bba1cd980d85
ms.service: batch
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: big-compute
ms.date: 09/28/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 68a5493282fa4a0b54ba551c48ae963a42b94dca
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="manage-batch-resources-with-azure-cli"></a>Správa prostředků služby Batch pomocí Azure CLI

Rozhraní příkazového řádku Azure CLI 2.0 představuje nové prostředí příkazového řádku Azure pro správu prostředků Azure. Je možné používat ho v systémech macOS, Linux a Windows. Rozhraní příkazového řádku Azure CLI 2.0 je optimalizováno pro správu prostředků Azure z příkazového řádku. Rozhraní příkazového řádku Azure CLI můžete používat ke správě účtů služby Azure Batch a ke správě prostředků, jako jsou fondy, úlohy a úkoly. V rozhraní příkazového řádku Azure CLI můžete používat skripty pro mnoho stejných úkolů, které se provádějí prostřednictvím rozhraní API služby Batch, webu Azure Portal a rutin PowerShellu služby Batch.

Tento článek obsahuje přehled používání rozhraní [Azure CLI verze 2.0](https://docs.microsoft.com/cli/azure/overview) se službou Batch. V článku [Začínáme s Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) najdete přehled používání rozhraní příkazového řádku CLI s Azure.

Společnost Microsoft doporučuje používat nejnovější verzi rozhraní příkazového řádku Azure CLI – verzi 2.0. Další informace o verzi 2.0 najdete v článku [Příkazový řádek Azure 2.0 je nyní veřejně k dispozici](https://azure.microsoft.com/blog/announcing-general-availability-of-vm-storage-and-network-azure-cli-2-0/).

## <a name="set-up-the-azure-cli"></a>Instalace rozhraní příkazového řádku Azure CLI

Pokud chcete nainstalovat rozhraní příkazového řádku Azure CLI, postupujte podle kroků uvedených v článku [Instalace rozhraní příkazového řádku Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).

> [!TIP]
> Instalaci rozhraní příkazového řádku Azure CLI doporučujeme často aktualizovat, abyste mohli využívat výhody, které vám přinášejí aktualizace a vylepšení služby.
> 
> 

## <a name="command-help"></a>Nápověda k příkazům

Pro každý příkaz v rámci rozhraní příkazového řádku Azure CLI můžete zobrazit nápovědu, pokud za název příkazu přidáte parametr `-h`. Jiné parametry vynechejte. Například:

* Pokud chcete zobrazit nápovědu pro příkaz `az`, zadejte: `az -h`
* Pokud chcete vypsat seznam všech příkazů Batch v rámci rozhraní příkazového řádku, zadejte: `az batch -h`
* Pokud chcete získat nápovědu k vytvoření účtu Batch, zadejte: `az batch account create -h`

Pokud si nejste jisti, pomocí parametru příkazového řádku `-h` můžete zobrazit nápovědu pro kterýkoli příkaz rozhraní příkazového řádku Azure CLI.

> [!NOTE]
> Starší verze rozhraní příkazového řádku Azure CLI používaly před příkazy CLI příznak `azure`. Ve verzi 2.0 začínají všechny příkazy příznakem `az`. Nezapomeňte aktualizovat skripty, aby u verze 2.0 používaly novou syntaxi.
>
>  

Kromě toho si prohlédněte referenční dokumentaci rozhraní příkazového řádku Azure CLI, kde najdete podrobnosti o [příkazech rozhraní příkazového řádku Azure CLI pro službu Batch](https://docs.microsoft.com/cli/azure/batch). 

## <a name="log-in-and-authenticate"></a>Přihlášení a ověření

Pokud chcete používat rozhraní příkazového řádku Azure CLI se službou Batch, potřebujete se přihlásit a provést ověření. Je třeba provést dva jednoduché kroky:

1. **Přihlaste se k Azure.** Přihlášení k Azure vám umožní přístup k příkazům správce Azure Resource Manager včetně příkazů [služby Batch Management](batch-management-dotnet.md).  
2. **Přihlaste se ke svému účtu Batch.** Přihlášení k účtu Batch vám umožní přístup k příkazům služby Batch.   

### <a name="log-in-to-azure"></a>Přihlášení k Azure

Existuje několik různých způsobů přihlášení k Azure, které jsou podrobně popsány v článku [Přihlášení pomocí Azure CLI 2.0](https://docs.microsoft.com/cli/azure/authenticate-azure-cli):

1. [Interaktivní přihlášení](https://docs.microsoft.com/cli/azure/authenticate-azure-cli#az_authenticate_azure_cli_interactive_log_in): Přihlaste se interaktivně, pokud spouštíte příkazy rozhraní příkazového řádku Azure CLI sami z příkazového řádku.
2. [Přihlášení pomocí instančního objektu](https://docs.microsoft.com/cli/azure/authenticate-azure-cli#az_authenticate_azure_cli_logging_in_with_a_service_principal): Pokud spouštíte příkazy rozhraní příkazového řádku Azure CLI ze skriptu nebo aplikace, přihlaste se pomocí instančního objektu.

Pro účely tohoto článku vám ukážeme, jak se k Azure přihlásit interaktivně. V příkazovém řádku napište [az login](https://docs.microsoft.com/cli/azure/#login):

```azurecli
# Log in to Azure and authenticate interactively.
az login
```

Příkaz `az login` vrátí token, který můžete použít k ověření, jak je vidět zde. Postupem podle zobrazených pokynů otevřete webovou stránku a odešlete token do služby Azure:

![Přihlášení k Azure](./media/batch-cli-get-started/az-login.png)

Příklady uvedené v části [Ukázkové skripty prostředí](#sample-shell-scripts) také ukazují, jak spustit relaci rozhraní příkazového řádku Azure CLI pomocí interaktivního přihlášení k Azure. Jakmile se přihlásíte, můžete volat příkazy pro práci s prostředky služby Batch Management včetně účtů Batch, klíčů, balíčků aplikací a kvót.  

### <a name="log-in-to-your-batch-account"></a>Přihlášení k účtu Batch

Pokud chcete rozhraní příkazového řádku Azure CLI používat ke správě prostředků služby Batch, jako jsou fondy, úlohy a úkoly, musíte se přihlásit k účtu Batch a provést ověření. Ke službě Batch se přihlásíte pomocí příkazu [az batch account login](https://docs.microsoft.com/cli/azure/batch/account#az_batch_account_login). 

Máte dvě možnosti ověření proti účtu Batch:

- **Ověření pomocí služby Azure Active Directory (Azure AD)** 

    Ověření pomocí služby Azure AD je výchozí možností, pokud používáte rozhraní příkazového řádku Azure CLI se službou Batch. Tuto možnost doporučujeme pro většinu scénářů. 
    
    Při interaktivním přihlášení k Azure, které je popsáno v předchozí části, jsou vaše přihlašovací údaje uloženy v mezipaměti, takže rozhraní příkazového řádku Azure CLI vás může přihlásit k účtu Batch pomocí stejných přihlašovacích údajů. Pokud se k Azure přihlásíte pomocí instančního objektu, použijí se tyto přihlašovací údaje také k přihlášení k účtu Batch.

    Výhoda služby Azure AD je, že nabízí řízení přístupu na základě role (RBAC). Při řízení přístupu na základě role (RBAC) závisí přístup uživatelů na jejich přiřazené roli, a ne na tom, jestli mají nebo nemají klíče účtu. Místo správy klíčů účtu můžete spravovat role RBAC a nechat řízení přístupu a ověřování na službě Azure AD.  

        To log in to your Batch account using Azure AD, call the [az batch account login](https://docs.microsoft.com/cli/azure/batch/account#az_batch_account_login) command: 

    ```azurecli
    az batch account login -g myresource group -n mybatchaccount
    ```

- **Ověření pomocí sdíleného klíče**

    [Ověření pomocí sdíleného klíče](https://docs.microsoft.com/rest/api/batchservice/authenticate-requests-to-the-azure-batch-service#authentication-via-shared-key) používá k ověření příkazů rozhraní příkazového řádku Azure CLI pro službu Batch klíče pro přístup k účtu.

    Pokud vytváříte skripty rozhraní příkazového řádku Azure CLI k automatizaci volání příkazů služby Batch, můžete použít buď ověření pomocí sdíleného klíče, anebo instanční objekt služby Azure AD. V některých scénářích může být ověření pomocí sdíleného klíče jednodušší než vytvoření instančního objektu.  

    Pokud se chcete přihlásit s ověřením pomocí sdíleného klíče, zahrňte v příkazovém řádku možnost `--shared-key-auth`:

    ```azurecli
    az batch account login -g myresourcegroup -n mybatchaccount --shared-key-auth
    ```

Příklady uvedené v části [Ukázkové skripty prostředí](#sample-shell-scripts) ukazují, jak se k účtu Batch přihlásit v rozhraní příkazového řádku Azure CLI jak pomocí služby Azure AD, tak i pomocí sdíleného klíče.

## <a name="use-azure-batch-cli-templates-and-file-transfer-preview"></a>Použití šablon rozhraní příkazového řádku služby Batch a přenos souborů (Preview)

Pomocí Azure CLI můžete spouštět kompletní úlohy Batch bez psaní kódu. Soubory šablon služby Batch podporují vytváření fondů, úloh a úkolů pomocí Azure CLI. Pomocí Azure CLI můžete také nahrávat vstupní soubory úloh do účtu Azure Storage přidruženého k účtu Batch a stahovat z něj výstupní soubory úloh. Další informace najdete v tématu [Použití šablon rozhraní příkazového řádku služby Batch a přenos souborů (Preview)](batch-cli-templates.md).

## <a name="sample-shell-scripts"></a>Ukázkové skripty prostředí

Ukázkové skripty uvedené v následující tabulce ukazují, jak provádět běžné úkoly pomocí rozhraní příkazového řádku Azure CLI se službou Batch a službou Batch Management. Tyto ukázkové skripty zahrnují řadu příkazů, které jsou k dispozici v rozhraní příkazového řádku Azure CLI pro službu Batch. 

| Skript | Poznámky |
|---|---|
| [Vytvoření účtu služby Batch](./scripts/batch-cli-sample-create-account.md) | Vytvoří účet Batch a přidruží ho k účtu úložiště. |
| [Přidání aplikace](./scripts/batch-cli-sample-add-application.md) | Přidá aplikaci a nahraje zabalené binární soubory.|
| [Správa fondů služby Batch](./scripts/batch-cli-sample-manage-pool.md) | Ukazuje vytváření, změny velikosti a správu fondů. |
| [Spuštění úlohy a úkolů pomocí služby Batch](./scripts/batch-cli-sample-run-job.md) | Ukazuje spuštění úlohy a přidávání úkolů. |

## <a name="json-files-for-resource-creation"></a>Soubory JSON pro vytváření prostředků

Při vytváření prostředků Batch, jako jsou fondy a úlohy, můžete určit soubor JSON obsahující konfiguraci nového prostředku namísto předávání jejích parametrů v podobě parametrů příkazového řádku. Například:

```azurecli
az batch pool create my_batch_pool.json
```

Ačkoli mnoho prostředků služby Batch můžete vytvářet pouze prostřednictvím parametrů příkazového řádku, některé funkce vyžadují, abyste určili soubor ve formátu JSON obsahující podrobnosti o prostředku. Soubor JSON je například třeba použít, pokud chcete určit soubory prostředků pro úkol při spuštění.

Pokud si chcete prohlédnout syntaxi souboru JSON vyžadovanou k vytvoření prostředku, prostudujte si dokumentaci [Reference k rozhraní REST API služby Batch][rest_api]. Každé z témat Přidání *typ prostředku* v dokumentaci Reference k rozhraní REST API obsahuje ukázkové skripty JSON pro vytvoření příslušného prostředku. Tyto ukázkové skripty JSON můžete použít jako šablony pro soubory JSON a používat je v rozhraní příkazového řádku Azure CLI. Pokud si například chcete prohlédnout syntaxi skriptu JSON pro vytvoření fondu, podívejte se na téma [Přidání fondu k účtu][rest_add_pool].

Ukázkový skript, který určuje soubor JSON, najdete v článku [Spuštění úlohy a úkolů pomocí služby Batch](./scripts/batch-cli-sample-run-job.md).

> [!NOTE]
> Pokud při vytváření prostředku určíte soubor JSON, budou všechny ostatní parametry zadané v příkazovém řádku pro příslušný prostředek ignorovány.
> 
> 

## <a name="efficient-queries-for-batch-resources"></a>Efektivní dotazy na prostředky služby Batch

Každý typ prostředku Batch podporuje příkaz `list`, který zadá dotaz na účet Batch a vypíše seznam prostředků příslušného typu. Můžete například vypsat seznam fondů v rámci vašeho účet a seznam úkolů v rámci úloh:

```azurecli
az batch pool list
az batch task list --job-id job001
```

Při dotazování služby Batch pomocí operace `list` můžete určit klauzuli OData a omezit tak množství vrácených dat. Protože veškeré filtrování probíhá na straně serveru, přenášejí se pouze data, která požadujete. Při provádění operací vypsání seznamu můžete pomocí těchto klauzulí snížit využití šířky pásma (a tedy čas).

Následující tabulka popisuje klauzule OData podporované službou Batch:

| Klauzule | Popis |
|---|---|
| `--select-clause [select-clause]` | Vrátí podmnožinu vlastností pro každou entitu. |
| `--filter-clause [filter-clause]` | Vrátí pouze ty entity, které odpovídají zadanému výrazu OData. |
| `--expand-clause [expand-clause]` | Získá informace o entitách v rámci jediného základního volání REST. Klauzule expand v současné době podporuje pouze vlastnost `stats`. |

Ukázkový skript, který ukazuje, jak používat klauzuli OData, najdete v článku [Spuštění úlohy a úkolů pomocí služby Batch](./scripts/batch-cli-sample-run-job.md).

Další informace o provádění efektivních dotazů pomocí příkazu list s klauzulemi OData najdete v článku [Efektivní dotazování na službu Azure Batch](batch-efficient-list-queries.md).

## <a name="troubleshooting-tips"></a>Rady pro řešení potíží

Následující tipy mohou pomoci při řešení potíží s rozhraním příkazového řádku Azure CLI:

* Použijte parametr `-h` k získání **textu nápovědy** pro kterýkoli příkaz rozhraní příkazového řádku CLI.
* Pomocí parametrů `-v` a `-vv` zobrazíte **podrobný** výstup příkazu. Pokud zahrnete příznak `-vv`, zobrazí rozhraní příkazového řádku Azure CLI příslušné požadavky a odpovědi služby REST. Tyto přepínače jsou užitečné pro zobrazení úplného chybového výstupu.
* Pomocí parametru `--json` můžete zobrazit **výstup příkazu ve formátu JSON**. Příkaz `az batch pool show pool001 --json` například zobrazí vlastnosti fondu pool001 ve formátu JSON. Pak můžete tento výstup zkopírovat a upravit pro použití v příkazu `--json-file` (viz [soubory JSON](#json-files) dříve v tomto článku).
<!---Loc Comment: Please, check link [JSON files] since it's not redirecting to any location.--->
* [Fórum služby Batch][batch_forum] je sledováno členy týmu služby Batch. Pokud narazíte na problémy nebo hledáte pomoc s konkrétní operací, můžete tu uveřejnit své otázky.

## <a name="next-steps"></a>Další kroky

* Další informace o rozhraní příkazového řádku Azure CLI najdete v [dokumentaci k rozhraní příkazového řádku Azure CLI](https://docs.microsoft.com/cli/azure/overview).
* Další informace o prostředcích služby Batch najdete v článku [Přehled služby Azure Batch pro vývojáře](batch-api-basics.md).
* Další informace o použití šablon služby Batch k vytvoření fondů, úloh a úkolů bez psaní kódu najdete v tématu [Použití šablon rozhraní příkazového řádku služby Batch a přenos souborů (Preview)](batch-cli-templates.md).

[batch_forum]: https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch
[github_readme]: https://github.com/Azure/azure-xplat-cli/blob/dev/README.md
[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
