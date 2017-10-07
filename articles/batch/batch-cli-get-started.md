---
title: "aaaGet začít s Azure CLI pro dávku | Microsoft Docs"
description: "Rychlý úvod toohello Batch příkazy Get v rozhraní příkazového řádku Azure pro správu prostředků služby Azure Batch"
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: fcd76587-1827-4bc8-a84d-bba1cd980d85
ms.service: batch
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: big-compute
ms.date: 07/20/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 14f28311ecb16c8097d0d304a4ad89de282a2e9b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-batch-resources-with-azure-cli"></a>Správa prostředků služby Batch pomocí Azure CLI

Hello Azure CLI 2.0 je Azure nové prostředí příkazového řádku pro správu prostředků Azure. Je možné používat ho v systémech macOS, Linux a Windows. Azure CLI 2.0 je optimalizovaná pro správu a Správa prostředků Azure z příkazového řádku hello. Toomanage hello rozhraní příkazového řádku Azure můžete použít účty Azure Batch a toomanage prostředkům, například fondy, úlohy a úkoly. S hello rozhraní příkazového řádku Azure, můžete skript řadu hello hello stejné úlohy, které se provádějí pomocí rozhraní API služby Batch, portálu Azure a rutiny prostředí PowerShell Batch.

Tento článek obsahuje přehled používání rozhraní [Azure CLI verze 2.0](https://docs.microsoft.com/cli/azure/overview) se službou Batch. V tématu [Začínáme s Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) přehled pomocí hello rozhraní příkazového řádku Azure.

Společnost Microsoft doporučuje používat nejnovější verzi hello hello rozhraní příkazového řádku Azure, verze 2.0. Další informace o verzi 2.0 najdete v článku [Příkazový řádek Azure 2.0 je nyní veřejně k dispozici](https://azure.microsoft.com/blog/announcing-general-availability-of-vm-storage-and-network-azure-cli-2-0/).

## <a name="set-up-hello-azure-cli"></a>Nastavit hello rozhraní příkazového řádku Azure

tooinstall hello rozhraní příkazového řádku Azure, postupujte podle kroků hello uvedených v [hello instalace rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/install-azure-cli.md).

> [!TIP]
> Doporučujeme aktualizovat vaše instalace rozhraní příkazového řádku Azure často tootake výhod aktualizace a vylepšení služby.
> 
> 

## <a name="command-help"></a>Nápověda k příkazům

Můžete zobrazit text nápovědy pro každý příkaz v hello rozhraní příkazového řádku Azure připojením `-h` toohello příkaz. Jiné parametry vynechejte. Například:

* tooget nápovědu pro hello `az` příkazu, zadejte:`az -h`
* tooget seznam všech příkazů Batch v hello rozhraní příkazového řádku, použijte:`az batch -h`
* tooget nápovědu k vytvoření účtu Batch, zadejte:`az batch account create -h`

Pokud máte pochybnosti, použijte hello `-h` možnost příkazového řádku tooget nápovědu k libovolnému příkazu příkazového řádku Azure CLI.

> [!NOTE]
> Starší verze hello používá rozhraní příkazového řádku Azure `azure` toopreface příkaz rozhraní příkazového řádku. Ve verzi 2.0 začínají všechny příkazy příznakem `az`. Být jisti tooupdate skripty toouse hello novou syntaxi s verze 2.0.
>
>  

Kromě toho naleznete v dokumentaci referenční dokumentace rozhraní příkazového řádku Azure toohello podrobnosti o [rozhraní příkazového řádku Azure pro dávku](https://docs.microsoft.com/cli/azure/batch). 

## <a name="log-in-and-authenticate"></a>Přihlášení a ověření

toouse hello rozhraní příkazového řádku Azure pomocí služby Batch, potřebujete toolog v a ověření. Existují dvě toofollow jednoduchých kroků:

1. **Přihlaste se k Azure.** Protokolování do Azure poskytuje přístup příkazů tooAzure Resource Manager, včetně [služba Batch Management](batch-management-dotnet.md) příkazy.  
2. **Přihlaste se ke svému účtu Batch.** Protokolování do vašeho dávkového účtu poskytuje přístup k tooBatch služby příkazy.   

### <a name="log-in-tooazure"></a>Přihlaste se tooAzure

Existuje několik různých způsobů toolog do Azure, které jsou podrobně popsány v [Přihlaste se pomocí Azure CLI 2.0](https://docs.microsoft.com/cli/azure/authenticate-azure-cli):

1. [Interaktivní přihlášení](https://docs.microsoft.com/cli/azure/authenticate-azure-cli#interactive-log-in): Přihlaste interaktivně Pokud používáte rozhraní příkazového řádku Azure sami z příkazového řádku hello.
2. [Přihlášení pomocí instančního objektu](https://docs.microsoft.com/cli/azure/authenticate-azure-cli#logging-in-with-a-service-principal): Pokud spouštíte příkazy rozhraní příkazového řádku Azure CLI ze skriptu nebo aplikace, přihlaste se pomocí instančního objektu.

Pro účely hello tohoto článku, ukážeme, jak toolog do Azure interaktivně. Typ [az přihlášení](https://docs.microsoft.com/cli/azure/#login) na příkazovém řádku hello:

```azurecli
# Log in tooAzure and authenticate interactively.
az login
```

Hello `az login` příkaz vrátí token tooauthenticate, můžete použít, jak je vidět tady. Postupujte podle pokynů tooopen hello webové stránky a odeslat hello token tooAzure:

![Přihlaste se tooAzure](./media/batch-cli-get-started/az-login.png)

Příklady Hello uvedené v hello [ukázkové skripty prostředí](#sample-shell-scripts) tématu zobrazit také jak toostart relaci příkazového řádku Azure CLI interaktivně protokolováním do Azure. Jakmile jste přihlášení, můžete volat příkazy toowork s Batch správy prostředků, včetně účty Batch, klíče, balíčky aplikací a kvóty.  

### <a name="log-in-tooyour-batch-account"></a>Přihlaste se tooyour účtu Batch

toouse hello rozhraní příkazového řádku Azure toomanage prostředky Batch, například fondy, úlohy a úkoly, je nutné toolog do vašeho účtu Batch a ověření. toolog v toohello služby Batch, použijte hello [připojení k účtu batch az](https://docs.microsoft.com/cli/azure/batch/account#login) příkaz. 

Máte dvě možnosti ověření proti účtu Batch:

- **Ověření pomocí služby Azure Active Directory (Azure AD)** 

    Ověřování s Azure AD při použití hello rozhraní příkazového řádku Azure pomocí služby Batch je hello výchozí a doporučuje pro většinu scénářů. 
    
    Při přihlášení tooAzure interaktivně, jak je popsáno v předchozí části hello, vaše přihlašovací údaje jsou ukládány do mezipaměti, tedy hello rozhraní příkazového řádku Azure můžete přihlásit tooyour pomocí těchto stejné přihlašovací údaje účtu Batch. Pokud se přihlásit pomocí objektu služby tooAzure, tyto přihlašovací údaje jsou také použít toolog v tooyour účtu Batch.

    Výhoda služby Azure AD je, že nabízí řízení přístupu na základě role (RBAC). S RBAC závisí přístup uživatele na jejich přiřazené role místo, jestli budou mít klíče účtu hello. Místo správy klíčů účtu můžete spravovat role RBAC a nechat řízení přístupu a ověřování na službě Azure AD.  

    Ověřování s Azure AD je povinný, pokud jste vytvořili nastavení vašeho účtu Azure Batch pomocí jeho režim přidělení fondu too'User předplatné '. 

    toolog v tooyour dávkového účtu pomocí Azure AD, volejte hello [připojení k účtu batch az](https://docs.microsoft.com/cli/azure/batch/account#login) příkaz: 

    ```azurecli
    az batch account login -g myresource group -n mybatchaccount
    ```

- **Ověření pomocí sdíleného klíče**

    [Ověření pomocí sdíleného klíče](https://docs.microsoft.com/rest/api/batchservice/authenticate-requests-to-the-azure-batch-service#authentication-via-shared-key) služba Batch používá příkazy příkazového řádku Azure CLI tooauthenticate pro hello klíčů pro váš účet přístup.

    Pokud vytváříte skripty rozhraní příkazového řádku Azure Batch příkazy volání tooautomate, můžete použít ověření sdíleným klíčem nebo hlavní služby Azure AD. V některých scénářích může být ověření pomocí sdíleného klíče jednodušší než vytvoření instančního objektu.  

    toolog pomocí ověření sdíleným klíčem, zahrnují hello `--shared-key-auth` možnost na příkazovém řádku hello:

    ```azurecli
    az batch account login -g myresourcegroup -n mybatchaccount --shared-key-auth
    ```

Příklady Hello uvedené v hello [ukázkové skripty prostředí](#sample-shell-scripts) části ukazují, jak toolog do vašeho účtu Batch s hello rozhraní příkazového řádku Azure pomocí Azure AD a sdílený klíč.

## <a name="use-azure-batch-cli-templates-and-file-transfer-preview"></a>Použití šablon rozhraní příkazového řádku služby Batch a přenos souborů (Preview)

Můžete vytvořit hello rozhraní příkazového řádku Azure toorun dávkové úlohy na kompletní bez nutnosti psaní kódu. Dávkové soubory šablony podporují vytváření fondů, úloh a úloh s hello rozhraní příkazového řádku Azure. Můžete taky hello rozhraní příkazového řádku Azure tooupload úlohy vstupní soubory toohello účet úložiště Azure přidružený hello účet Batch a z něj stahují úlohy výstupní soubory. Další informace najdete v tématu [Použití šablon rozhraní příkazového řádku služby Batch a přenos souborů (Preview)](batch-cli-templates.md).

## <a name="sample-shell-scripts"></a>Ukázkové skripty prostředí

Ukázkové skripty Hello uvedené v následující tabulce zobrazit text hello, jak příkazy toouse rozhraní příkazového řádku Azure pomocí služby Batch hello a běžných úlohách tooaccomplish Batch Management service. Tyto ukázkové skripty zahrnují řadu hello příkazy, které jsou k dispozici v hello rozhraní příkazového řádku Azure pro dávku. 

| Skript | Poznámky |
|---|---|
| [Vytvoření účtu služby Batch](./scripts/batch-cli-sample-create-account.md) | Vytvoří účet Batch a přidruží ho k účtu úložiště. |
| [Přidání aplikace](./scripts/batch-cli-sample-add-application.md) | Přidá aplikaci a nahraje zabalené binární soubory.|
| [Správa fondů služby Batch](./scripts/batch-cli-sample-manage-pool.md) | Ukazuje vytváření, změny velikosti a správu fondů. |
| [Spuštění úlohy a úkolů pomocí služby Batch](./scripts/batch-cli-sample-run-job.md) | Ukazuje spuštění úlohy a přidávání úkolů. |

## <a name="json-files-for-resource-creation"></a>Soubory JSON pro vytváření prostředků

Když vytvoříte prostředky Batch jako fondů a úloh, můžete zadat soubor JSON obsahující konfiguraci hello nový prostředek místo předávání jeho parametrů jako možnosti příkazového řádku. Například:

```azurecli
az batch pool create my_batch_pool.json
```

Můžete si sice vytvořit většina prostředky Batch pomocí pouze možnosti příkazového řádku, některé funkce vyžadují, aby souboru formátu JSON, který obsahuje podrobnosti prostředku hello. Pokud chcete, aby toospecify zdrojových souborů pro spouštěcí úkol, například musíte použít soubor JSON.

toosee hello syntaxe JSON požadované toocreate prostředku, získáte toohello [Batch REST API odkaz] [ rest_api] dokumentaci. Každý "Přidat *typ prostředku*" v nápovědě k hello odkazu k REST API obsahuje ukázkové skripty JSON pro vytvoření prostředku. Tyto ukázkové skripty JSON můžete použít jako šablona pro toouse soubory JSON s hello rozhraní příkazového řádku Azure. Například toosee hello syntaxe JSON pro vytvoření fondu odkazovat příliš[přidat účet fondu tooan][rest_add_pool].

Ukázkový skript, který určuje soubor JSON, najdete v článku [Spuštění úlohy a úkolů pomocí služby Batch](./scripts/batch-cli-sample-run-job.md).

> [!NOTE]
> Pokud zadáte soubor JSON, když vytvoříte prostředek, se ignorují další parametry, které jste zadali na hello příkazového řádku pro tento prostředek.
> 
> 

## <a name="efficient-queries-for-batch-resources"></a>Efektivní dotazy na prostředky služby Batch

Každý typ prostředku Batch podporuje příkaz `list`, který zadá dotaz na účet Batch a vypíše seznam prostředků příslušného typu. Například můžete vytvořit seznam hello fondy ve vašem účtu a hello úkoly v úloze:

```azurecli
az batch pool list
az batch task list --job-id job001
```

Při dotazování služby Batch hello pomocí `list` operace, můžete zadat dobu hello toolimit klauzule OData vrácená data. Vzhledem k tomu, že všechny filtrování dojde na straně serveru, pouze hello data, která budete požadovat protne hello přenosu. Využití šířky pásma tyto klauzule toosave (a tedy času) při provádění operace výpisu.

Hello následující tabulka popisuje hello klauzule OData nepodporuje hello služby Batch:

| Klauzule | Popis |
|---|---|
| `--select-clause [select-clause]` | Vrátí podmnožinu vlastností pro každou entitu. |
| `--filter-clause [filter-clause]` | Vrátí pouze entity, které odpovídají hello zadaný výraz OData. |
| `--expand-clause [expand-clause]` | Získá informace hello entity v jediném volání REST základní. Hello rozbalte klauzule aktuálně podporuje pouze hello `stats` vlastnost. |

Pro ukázku skriptu této ukazuje, jak zjistit, toouse klauzuli OData [spouštějí úlohy a úlohy pomocí služby Batch](./scripts/batch-cli-sample-run-job.md).

Další informace o provádění dotazů efektivní seznamu s klauzulemi OData najdete v tématu [efektivní dotazování služby Azure Batch hello](batch-efficient-list-queries.md).

## <a name="troubleshooting-tips"></a>Rady pro řešení potíží

Hello následující tipy mohou pomoci při odstraňování problémů, rozhraní příkazového řádku Azure:

* Použití `-h` tooget **text nápovědy** k libovolnému příkazu rozhraní příkazového řádku
* Použití `-v` a `-vv` toodisplay **podrobné** příkaz výstupu. Když hello `-vv` příznak je součástí, hello rozhraní příkazového řádku Azure zobrazí hello skutečné REST požadavky a odpovědi. Tyto přepínače jsou užitečné pro zobrazení úplného chybového výstupu.
* Můžete zobrazit **příkaz výstup jako JSON** s hello `--json` možnost. Příkaz `az batch pool show pool001 --json` například zobrazí vlastnosti fondu pool001 ve formátu JSON. Můžete zkopírovat a upravit tento výstup toouse v `--json-file` (viz [soubory JSON](#json-files) výše v tomto článku).
<!---Loc Comment: Please, check link [JSON files] since it's not redirecting tooany location.--->
* Hello [fórum Batch] [ batch_forum] je monitorován pomocí členové týmu Batch. Pokud narazíte na problémy nebo hledáte pomoc s konkrétní operací, můžete tu uveřejnit své otázky.

## <a name="next-steps"></a>Další kroky

* Další informace o hello rozhraní příkazového řádku Azure najdete v tématu hello [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).
* Další informace o prostředcích služby Batch najdete v článku [Přehled služby Azure Batch pro vývojáře](batch-api-basics.md).
* Další informace o použití šablony toocreate fondy, úlohy a úlohy Batch bez nutnosti psaní kódu najdete v tématu [použití Azure Batch rozhraní příkazového řádku šablony a přenos souborů (Preview)](batch-cli-templates.md).

[batch_forum]: https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch
[github_readme]: https://github.com/Azure/azure-xplat-cli/blob/dev/README.md
[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
