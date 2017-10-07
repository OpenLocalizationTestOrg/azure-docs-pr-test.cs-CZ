---
title: "řešení aaaUse nástroje a rozhraní API služby Azure Batch toodevelop rozsáhlé paralelní zpracování | Microsoft Docs"
description: "Další informace o hello rozhraní API a nástroje, které jsou k dispozici pro vývoj řešení se službou Azure Batch hello."
services: batch
author: tamram
manager: timlt
ms.service: batch
ms.topic: get-started-article
ms.date: 03/08/2017
ms.author: tamram
ms.openlocfilehash: ca75a1a63b3e7e6b0805e79a63685bc49aaaca8f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-batch-apis-and-tools"></a>Přehled rozhraní API a nástrojů služby Batch

Zpracování paralelní úlohy pomocí služby Azure Batch se obvykle provádí programově pomocí jedné z hello [rozhraní API služby Batch](#batch-development-apis). Klientská aplikace nebo služby můžete použít rozhraní API služby Batch toocommunicate hello s hello služby Batch. S hello rozhraní API služby Batch, můžete vytvořit a spravovat fondy počítačových uzlů, virtuální počítače nebo cloudové služby. Pak můžete naplánovat úlohy a úkoly toorun na těchto uzlech. 

Můžete efektivně zpracovávat rozsáhlé úlohy pro vaši organizaci nebo poskytovat front-end služby zákazníkům tooyour tak, aby můžete spouštět úlohy a úkoly – na vyžádání nebo naplánované – na jednom, stovkách nebo dokonce tisíce uzly. Službu Azure Batch můžete také používat jako součást rozsáhlejšího pracovního postupu spravovaného nástroji, například [Azure Data Factory](../data-factory/data-factory-data-processing-using-batch.md?toc=%2fazure%2fbatch%2ftoc.json).

> [!TIP]
> Až budete připraveni toodig v toohello Batch API hlubšího porozumění hello funkce poskytuje, podívejte se na hello [přehled funkcí Batch pro vývojáře](batch-api-basics.md).
> 
> 

## <a name="azure-accounts-for-batch-development"></a>Účty Azure pro vývoj pro službu Batch
Při vývoji řešení Batch budete používat hello následující účty ve službě Microsoft Azure.

* **Účet a předplatné Azure** – Pokud zatím předplatné Azure nemáte, můžete si aktivovat [výhodu předplatitele MSDN][msdn_benefits] nebo si zaregistrovat [bezplatný účet Azure][free_account]. Při vytvoření účtu pro vás bude vytvořeno výchozí předplatné.
* **Účet Batch** – Prostředky služby Azure Batch, včetně fondů, výpočetních uzlů, úloh a úkolů, jsou přidružené k účtu Azure Batch. Pokud vaše aplikace provede požadavek hello služby Batch, ověřuje žádosti o hello pomocí hello název účtu Azure Batch, hello URL hello účtu a přístupový klíč. Můžete [vytvořit účet Batch](batch-account-create-portal.md) v hello portálu Azure.
* **Účet Storage** – Služba Batch zahrnuje integrovanou podporu pro práci se soubory ve službě [Azure Storage][azure_storage]. Téměř každý scénář Batch používá úložiště objektů Blob v Azure pro přípravu hello programy, které vaše úkoly spouštět a hello data, která budou zpracovávat a pro ukládání výstupních dat, která generují hello. toocreate účet úložiště, najdete v části [účty Azure storage](../storage/common/storage-create-storage-account.md).

## <a name="batch-service-apis"></a>Rozhraní API služby Batch

Vaše aplikace a služby můžete vydávat přímá volání rozhraní REST API nebo použít jeden nebo více následujících klientských knihoven toorun hello a spravovat úlohy Azure Batch.

| Rozhraní API | API – referenční informace | Stáhnout | Kurz | Ukázky kódů | Další informace |
| --- | --- | --- | --- | --- | --- |
| **Batch REST** |[MSDN][batch_rest] |Není k dispozici |- |- | [Podporované verze](https://docs.microsoft.com/rest/api/batchservice/batch-service-rest-api-versioning) |
| **Batch .NET** |[docs.microsoft.com][api_net] |[NuGet ][api_net_nuget] |[Kurz](batch-dotnet-get-started.md) |[GitHub][api_sample_net] | [Poznámky k verzi](http://aka.ms/batch-net-dataplane-changelog) |
| **Batch Python** |[readthedocs.io][api_python] |[PyPI][api_python_pypi] |[Kurz](batch-python-tutorial.md)|[GitHub][api_sample_python] | [Soubor Readme](https://github.com/Azure/azure-sdk-for-python/blob/master/doc/batch.rst) |
| **Batch Node.js** |[github.io][api_nodejs] |[npm][api_nodejs_npm] |- |- | [Soubor Readme](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/batch) |
| **Batch Java** |[github.io][api_java] |[Maven][api_java_jar] |- |[Soubor Readme][api_sample_java] | [Soubor Readme](https://github.com/Azure/azure-batch-sdk-for-java)|

## <a name="batch-management-apis"></a>Rozhraní API služby Batch Management

Hello rozhraní API Správce Azure Resource Manageru pro dávku poskytují programový přístup tooBatch účty. Pomocí těchto rozhraní API můžete programově spravovat účty Batch, kvóty a balíčky aplikací.  

| Rozhraní API | API – referenční informace | Stáhnout | Kurz | Ukázky kódů |
| --- | --- | --- | --- | --- |
| **REST Batch Resource Manageru** |[docs.microsoft.com][api_rest_mgmt] |– |- |[GitHub](https://github.com/Azure-Samples/batch-dotnet-manage-batch-accounts) |
| **.NET Batch Resource Manageru** |[docs.microsoft.com][api_net_mgmt] |[NuGet ][api_net_mgmt_nuget] | [Kurz](batch-management-dotnet.md) |[GitHub][api_sample_net] |

## <a name="batch-command-line-tools"></a>Nástroje příkazového řádku služby Batch

Tyto nástroje příkazového řádku zadejte hello stejné funkce jako hello služby Batch a rozhraní API služby Batch Management: 

* [Rutiny prostředí PowerShell služby batch][batch_ps]: hello rutiny služby Azure Batch v hello [prostředí Azure PowerShell](/powershell/azure/overview) modulu umožňují toomanage prostředky Batch v prostředí PowerShell.
* [Rozhraní příkazového řádku Azure](/cli/azure/overview): hello rozhraní příkazového řádku Azure (Azure CLI) je sada nástrojů a platformy, která poskytuje příkazy prostředí pro interakci s řadou služeb Azure, včetně služby Batch hello a služba Batch Management. V tématu [prostředky spravovat Batch pomocí rozhraní příkazového řádku Azure](batch-cli-get-started.md) pro další informace o používání hello rozhraní příkazového řádku Azure pomocí služby Batch.

## <a name="other-tools-for-application-development"></a>Další nástroje pro vývoj aplikací

Tady je několik dalších nástrojů, které můžou být užitečné při sestavování a ladění aplikací a služeb Batch:

* [Portál Azure][portal]: můžete vytvořit, monitorování a odstranit fondy, úlohy a úkoly v hello Azure Batch okna Batch na portálu. Při spuštění úloh a i stahování souborů z hello výpočetních uzlů ve fondech, můžete zobrazit informace o těchto a dalších prostředků hello stavu. Například při řešení potíží si můžete stáhnout soubor `stderr.txt` neúspěšné úlohy. Můžete také stáhnout soubory Remote Desktop (RDP) používané toolog toocompute uzly.
* [Průzkumník Azure Batch][batch_explorer]: Batch Explorer poskytuje podobné funkce správy prostředků Batch jako hello portálu Azure, ale v samostatné aplikace klienta Windows Presentation Foundation (WPF). Jeden z hello ukázkových aplikací Batch .NET k dispozici na [Githubu][github_samples], můžete ji vytvořit s Visual Studio 2015 nebo novější a použít ho toobrowse a spravovat hello prostředky v účtu Batch při vývoji a ladění řešení Batch. Zobrazit úlohy, fondu a podrobnosti úlohy stáhnout soubory z výpočetních uzlů a vzdáleně připojit toonodes pomocí souborů Remote Desktop (RDP), které si můžete stáhnout pomocí Průzkumníka služby Batch.
* [Microsoft Azure Storage Explorer][storage_explorer]: při nejedná striktně nástroj služby Azure Batch, hello Storage Explorer je jiný toohave cenným nástrojem při vývoji a ladění řešení Batch.

## <a name="additional-resources"></a>Další zdroje

- v tématu toolearn o protokolování událostí z vaší aplikace Batch [protokolovat události diagnostických hodnocení a sledování řešení Batch](batch-diagnostics.md). Odkaz na události vyvolané službou hello služby Batch, najdete v části [Batch Analytics](batch-analytics.md).
- Informace o proměnných prostředí pro výpočetní uzly najdete v tématu [Proměnné prostředí výpočetních uzlů služby Azure Batch](batch-compute-node-environment-variables.md).

## <a name="next-steps"></a>Další kroky

* Čtení hello [přehled funkcí Batch pro vývojáře](batch-api-basics.md), základní informace pro každý, kdo Příprava toouse dávky. Hello článek obsahuje podrobnější informace o prostředky služby Batch, například fondy, uzlů, úlohy a úkoly a hello mnoho funkcí rozhraní API, které můžete použít při vytváření aplikace Batch.
* [Začínáme s knihovnou Azure Batch hello pro .NET](batch-dotnet-get-started.md) toolearn jak toouse C# a hello tooexecute knihovny Batch .NET jednoduché úlohy pomocí běžné pracovní postup služby Batch. Tento článek by měl být jeden z vašich prvních zastávek při učení jak toouse hello služby Batch. K dispozici je také [verze Python](batch-python-tutorial.md) hello kurzu.
* Stáhnout hello [ukázky kódů v Githubu] [ github_samples] toosee, jak můžete pomocí ukázkové úlohy Batch tooschedule a proces rozhraní jak C# a Python.
* Podívejte se na hello [Batch studijní] [ learning_path] tooget představu o hello prostředky k dispozici tooyou jako jste další toowork pomocí služby Batch.


[azure_storage]: https://azure.microsoft.com/services/storage/
[api_java]: http://azure.github.io/azure-sdk-for-java/
[api_java_jar]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-batch%22
[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_nuget]: https://www.nuget.org/packages/Azure.Batch/
[api_rest_mgmt]: https://docs.microsoft.com/\rest/api/batchmanagement/
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_net_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[api_nodejs]: http://azure.github.io/azure-sdk-for-node/azure-batch/latest/
[api_nodejs_npm]: https://www.npmjs.com/package/azure-batch
[api_python]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html
[api_python_pypi]: https://pypi.python.org/pypi/azure-batch
[api_sample_net]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp
[api_sample_python]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[api_sample_java]: https://github.com/Azure/azure-batch-samples/tree/master/Java/
[batch_ps]: /powershell/resourcemanager/azurerm.batch/v2.7.0/azurerm.batch
[batch_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[free_account]: https://azure.microsoft.com/free/
[github_samples]: https://github.com/Azure/azure-batch-samples
[learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[msdn_benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[storage_explorer]: http://storageexplorer.com/
[portal]: https://portal.azure.com
