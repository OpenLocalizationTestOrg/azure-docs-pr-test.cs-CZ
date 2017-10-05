---
title: "Použití sdílené přístupové podpisy (SAS) ve službě Azure Storage | Microsoft Docs"
description: "Naučte se používat sdílené přístupové podpisy (SAS) pro delegování přístupu k prostředkům Azure Storage, včetně objektů BLOB, fronty, tabulky a soubory."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 46fd99d7-36b3-4283-81e3-f214b29f1152
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/18/2017
ms.author: marsma
ms.openlocfilehash: ae944eae4e62132e47bf9069d83817a1a6779ff7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-shared-access-signatures-sas"></a><span data-ttu-id="9eadc-103">Použití sdílených přístupových podpisů (SAS)</span><span class="sxs-lookup"><span data-stu-id="9eadc-103">Using shared access signatures (SAS)</span></span>

<span data-ttu-id="9eadc-104">Sdílený přístupový podpis (SAS) poskytuje způsob, jak udělit omezený přístup k objektům v účtu úložiště pro ostatní klienty bez vystavení klíč účtu.</span><span class="sxs-lookup"><span data-stu-id="9eadc-104">A shared access signature (SAS) provides you with a way to grant limited access to objects in your storage account to other clients, without exposing your account key.</span></span> <span data-ttu-id="9eadc-105">V tomto článku jsme poskytovat přehled o modelu SAS, SAS osvědčené postupy a podívejte se na některé příklady.</span><span class="sxs-lookup"><span data-stu-id="9eadc-105">In this article, we provide an overview of the SAS model, review SAS best practices, and look at some examples.</span></span>

<span data-ttu-id="9eadc-106">Další příklady kódu pomocí SAS nad rámec těch, které jsou tu popsané, najdete v části [Začínáme s Azure Blob Storage v rozhraní .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/) a ostatních vzorků, které jsou k dispozici v [ukázky kódu Azure](https://azure.microsoft.com/documentation/samples/?service=storage) knihovny.</span><span class="sxs-lookup"><span data-stu-id="9eadc-106">For additional code examples using SAS beyond those presented here, see [Getting Started with Azure Blob Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/) and other samples available in the [Azure Code Samples](https://azure.microsoft.com/documentation/samples/?service=storage) library.</span></span> <span data-ttu-id="9eadc-107">Stažení ukázkových aplikací a jejich spuštění nebo procházet kód na Githubu.</span><span class="sxs-lookup"><span data-stu-id="9eadc-107">You can download the sample applications and run them, or browse the code on GitHub.</span></span>

## <a name="what-is-a-shared-access-signature"></a><span data-ttu-id="9eadc-108">Co je sdílený přístupový podpis?</span><span class="sxs-lookup"><span data-stu-id="9eadc-108">What is a shared access signature?</span></span>
<span data-ttu-id="9eadc-109">Sdílený přístupový podpis poskytuje Delegovaný přístup k prostředkům ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="9eadc-109">A shared access signature provides delegated access to resources in your storage account.</span></span> <span data-ttu-id="9eadc-110">Pomocí SAS můžete udělit klientům přístup k prostředkům ve vašem účtu úložiště bez sdílení klíče účtu.</span><span class="sxs-lookup"><span data-stu-id="9eadc-110">With a SAS, you can grant clients access to resources in your storage account, without sharing your account keys.</span></span> <span data-ttu-id="9eadc-111">Toto je klíče bod použití sdílených přístupových podpisů ve svých aplikacích – SAS je zabezpečení způsob, jak sdílet svým prostředkům úložiště bez kompromisů klíče účtu.</span><span class="sxs-lookup"><span data-stu-id="9eadc-111">This is the key point of using shared access signatures in your applications--a SAS is a secure way to share your storage resources without compromising your account keys.</span></span>

[!INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

<span data-ttu-id="9eadc-112">SAS vám poskytuje podrobnou kontrolu nad typ přístupu, kterou byste udělit klientům, kteří mají SAS, včetně:</span><span class="sxs-lookup"><span data-stu-id="9eadc-112">A SAS gives you granular control over the type of access you grant to clients who have the SAS, including:</span></span>

* <span data-ttu-id="9eadc-113">Interval, za které je platný, včetně počáteční čas a čas vypršení platnosti SAS.</span><span class="sxs-lookup"><span data-stu-id="9eadc-113">The interval over which the SAS is valid, including the start time and the expiry time.</span></span>
* <span data-ttu-id="9eadc-114">Oprávnění udělená pomocí SAS.</span><span class="sxs-lookup"><span data-stu-id="9eadc-114">The permissions granted by the SAS.</span></span> <span data-ttu-id="9eadc-115">SAS pro objekt blob může například udělit pro čtení a zápisu oprávnění k tomuto objektu blob ale oprávnění k odstranění.</span><span class="sxs-lookup"><span data-stu-id="9eadc-115">For example, a SAS for a blob might grant read and write permissions to that blob, but not delete permissions.</span></span>
* <span data-ttu-id="9eadc-116">Volitelné IP adresu nebo rozsah IP adres, ze kterých Azure Storage bude přijímat SAS.</span><span class="sxs-lookup"><span data-stu-id="9eadc-116">An optional IP address or range of IP addresses from which Azure Storage will accept the SAS.</span></span> <span data-ttu-id="9eadc-117">Například můžete určit rozsah IP adres, které patří do vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="9eadc-117">For example, you might specify a range of IP addresses belonging to your organization.</span></span>
* <span data-ttu-id="9eadc-118">Protokol, přes který bude přijímat Azure Storage SAS.</span><span class="sxs-lookup"><span data-stu-id="9eadc-118">The protocol over which Azure Storage will accept the SAS.</span></span> <span data-ttu-id="9eadc-119">Tento volitelný parametr můžete omezit přístup ke klientům pomocí protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9eadc-119">You can use this optional parameter to restrict access to clients using HTTPS.</span></span>

## <a name="when-should-you-use-a-shared-access-signature"></a><span data-ttu-id="9eadc-120">Když budete používat sdílený přístupový podpis?</span><span class="sxs-lookup"><span data-stu-id="9eadc-120">When should you use a shared access signature?</span></span>
<span data-ttu-id="9eadc-121">Pokud chcete poskytnout přístup k prostředkům ve vašem účtu úložiště libovolnému klientovi, které nevykazují přístupových klíčů k účtu úložiště, můžete použít SAS.</span><span class="sxs-lookup"><span data-stu-id="9eadc-121">You can use a SAS when you want to provide access to resources in your storage account to any client not possessing your storage account's access keys.</span></span> <span data-ttu-id="9eadc-122">Váš účet úložiště zahrnuje jak primární a sekundární přístupový klíč, které obě udělit přístup správce ke svému účtu a všechny prostředky v něm.</span><span class="sxs-lookup"><span data-stu-id="9eadc-122">Your storage account includes both a primary and secondary access key, both of which grant administrative access to your account, and all resources within it.</span></span> <span data-ttu-id="9eadc-123">Vystavení některý z nich otevře účet tak, aby možnost škodlivý nebo nedbalosti použití.</span><span class="sxs-lookup"><span data-stu-id="9eadc-123">Exposing either of these keys opens your account to the possibility of malicious or negligent use.</span></span> <span data-ttu-id="9eadc-124">Sdílené přístupové podpisy zadejte bezpečné alternativu, která umožňuje klientům čtení, zápisu a odstranění dat ve vašem účtu úložiště podle oprávnění, která jste explicitně udělí oprávnění a bez nutnosti klíč účtu.</span><span class="sxs-lookup"><span data-stu-id="9eadc-124">Shared access signatures provide a safe alternative that allows clients to read, write, and delete data in your storage account according to the permissions you've explicitly granted, and without need for an account key.</span></span>

<span data-ttu-id="9eadc-125">Běžný scénář, kde je užitečné SAS je služba, kde uživatelé číst a zapisovat svá vlastní data do účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="9eadc-125">A common scenario where a SAS is useful is a service where users read and write their own data to your storage account.</span></span> <span data-ttu-id="9eadc-126">Ve scénáři, kde účet úložiště ukládá data o uživatele existují dva vzory typické návrhu:</span><span class="sxs-lookup"><span data-stu-id="9eadc-126">In a scenario where a storage account stores user data, there are two typical design patterns:</span></span>

1. <span data-ttu-id="9eadc-127">Klienti nahrávání a stahování dat přes službu proxy serveru front-end, který provádí ověřování.</span><span class="sxs-lookup"><span data-stu-id="9eadc-127">Clients upload and download data via a front-end proxy service, which performs authentication.</span></span> <span data-ttu-id="9eadc-128">Tuto službu front-endu proxy nabízí výhodu v podobě povolení ověření obchodní pravidla, ale pro velké objemy dat nebo vysoký počet transakcí, vytváření služby, který můžete přizpůsobit tak, aby odpovídaly vyžádání může být nákladné nebo obtížná.</span><span class="sxs-lookup"><span data-stu-id="9eadc-128">This front-end proxy service has the advantage of allowing validation of business rules, but for large amounts of data or high-volume transactions, creating a service that can scale to match demand may be expensive or difficult.</span></span>

  ![Diagram scénáře: Front-end proxy služby][sas-storage-fe-proxy-service]

1. <span data-ttu-id="9eadc-130">Jednoduché služby ověřuje klienta podle potřeby a pak vygeneruje SAS.</span><span class="sxs-lookup"><span data-stu-id="9eadc-130">A lightweight service authenticates the client as needed and then generates a SAS.</span></span> <span data-ttu-id="9eadc-131">Jakmile klient obdrží SAS, můžete přímo s definována SAS a pro interval povolený SAS oprávnění přístupu k prostředkům účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="9eadc-131">Once the client receives the SAS, they can access storage account resources directly with the permissions defined by the SAS and for the interval allowed by the SAS.</span></span> <span data-ttu-id="9eadc-132">SAS snižuje potřebu směrování všechna data prostřednictvím služby proxy serveru front-end.</span><span class="sxs-lookup"><span data-stu-id="9eadc-132">The SAS mitigates the need for routing all data through the front-end proxy service.</span></span>

  ![Diagram scénáře: SAS zprostředkovatel služby][sas-storage-provider-service]

<span data-ttu-id="9eadc-134">Mnoho reálných služby mohou používat hybridní tyto dva přístupy.</span><span class="sxs-lookup"><span data-stu-id="9eadc-134">Many real-world services may use a hybrid of these two approaches.</span></span> <span data-ttu-id="9eadc-135">Například některá data mohou zpracovat a ověřit prostřednictvím proxy serveru front-end, zatímco jiné dat je uložit nebo číst přímo pomocí SAS.</span><span class="sxs-lookup"><span data-stu-id="9eadc-135">For example, some data might be processed and validated via the front-end proxy, while other data is saved and/or read directly using SAS.</span></span>

<span data-ttu-id="9eadc-136">Kromě toho budete muset použít SAS pro ověření zdroje objektu v operaci kopírování v některých scénářích:</span><span class="sxs-lookup"><span data-stu-id="9eadc-136">Additionally, you will need to use a SAS to authenticate the source object in a copy operation in certain scenarios:</span></span>

* <span data-ttu-id="9eadc-137">Pokud kopírujete objekt blob do jiného objektu blob, který se nachází v jiný účet úložiště, musíte použít SAS k ověření zdroje objektu blob.</span><span class="sxs-lookup"><span data-stu-id="9eadc-137">When you copy a blob to another blob that resides in a different storage account, you must use a SAS to authenticate the source blob.</span></span> <span data-ttu-id="9eadc-138">Volitelně můžete SAS k ověření cílový objekt blob také.</span><span class="sxs-lookup"><span data-stu-id="9eadc-138">You can optionally use a SAS to authenticate the destination blob as well.</span></span>
* <span data-ttu-id="9eadc-139">Při kopírování souboru do jiného souboru, který se nachází v jiný účet úložiště, musíte použít SAS k ověření zdrojový soubor.</span><span class="sxs-lookup"><span data-stu-id="9eadc-139">When you copy a file to another file that resides in a different storage account, you must use a SAS to authenticate the source file.</span></span> <span data-ttu-id="9eadc-140">Volitelně můžete SAS k ověření cílového souboru.</span><span class="sxs-lookup"><span data-stu-id="9eadc-140">You can optionally use a SAS to authenticate the destination file as well.</span></span>
* <span data-ttu-id="9eadc-141">Při kopírování objektu blob do souboru nebo soubor do objektu blob, musíte použít SAS k ověření zdroje objektu, i v případě, že zdrojové a cílové objekty jsou umístěny ve stejném účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="9eadc-141">When you copy a blob to a file, or a file to a blob, you must use a SAS to authenticate the source object, even if the source and destination objects reside within the same storage account.</span></span>

## <a name="types-of-shared-access-signatures"></a><span data-ttu-id="9eadc-142">Druhy sdílených přístupových podpisů</span><span class="sxs-lookup"><span data-stu-id="9eadc-142">Types of shared access signatures</span></span>
<span data-ttu-id="9eadc-143">Můžete vytvořit dva druhy sdílených přístupových podpisů:</span><span class="sxs-lookup"><span data-stu-id="9eadc-143">You can create two types of shared access signatures:</span></span>

* <span data-ttu-id="9eadc-144">**SAS služby.**</span><span class="sxs-lookup"><span data-stu-id="9eadc-144">**Service SAS.**</span></span> <span data-ttu-id="9eadc-145">SAS služby deleguje přístup k prostředku jen v jedné službě úložiště: službě Blob, Queue, Table nebo File.</span><span class="sxs-lookup"><span data-stu-id="9eadc-145">The service SAS delegates access to a resource in just one of the storage services: the Blob, Queue, Table, or File service.</span></span> <span data-ttu-id="9eadc-146">V tématu [vytváření SAS služby](https://msdn.microsoft.com/library/dn140255.aspx) a [příklady SAS služby](https://msdn.microsoft.com/library/dn140256.aspx) podrobné informace o vytváření token SAS služby.</span><span class="sxs-lookup"><span data-stu-id="9eadc-146">See [Constructing a Service SAS](https://msdn.microsoft.com/library/dn140255.aspx) and [Service SAS Examples](https://msdn.microsoft.com/library/dn140256.aspx) for in-depth information about constructing the service SAS token.</span></span>
* <span data-ttu-id="9eadc-147">**SAS účtu.**</span><span class="sxs-lookup"><span data-stu-id="9eadc-147">**Account SAS.**</span></span> <span data-ttu-id="9eadc-148">Účet SAS delegáti přístup k prostředkům v jedné nebo více službách úložiště.</span><span class="sxs-lookup"><span data-stu-id="9eadc-148">The account SAS delegates access to resources in one or more of the storage services.</span></span> <span data-ttu-id="9eadc-149">Všechny operace, které jsou dostupné přes SAS služby jsou dostupné prostřednictvím SAS účtu.</span><span class="sxs-lookup"><span data-stu-id="9eadc-149">All of the operations available via a service SAS are also available via an account SAS.</span></span> <span data-ttu-id="9eadc-150">Kromě toho s účtem SAS, můžete delegovat přístup k operace, které platí pro danou službu, jako například **vlastnosti služby Get/Set** a **získat statistiky služby**.</span><span class="sxs-lookup"><span data-stu-id="9eadc-150">Additionally, with the account SAS, you can delegate access to operations that apply to a given service, such as **Get/Set Service Properties** and **Get Service Stats**.</span></span> <span data-ttu-id="9eadc-151">Můžete taky delegovat přístup k operacím čtení, zápis a odstranění pro kontejnery objektů blob, tabulky a sdílené složky, který se nedá vymezit přes SAS služby.</span><span class="sxs-lookup"><span data-stu-id="9eadc-151">You can also delegate access to read, write, and delete operations on blob containers, tables, queues, and file shares that are not permitted with a service SAS.</span></span> <span data-ttu-id="9eadc-152">V tématu [vytváření SAS účtu](https://msdn.microsoft.com/library/mt584140.aspx) podrobné informace o vytváření tokenu SAS účtu.</span><span class="sxs-lookup"><span data-stu-id="9eadc-152">See [Constructing an Account SAS](https://msdn.microsoft.com/library/mt584140.aspx) for in-depth information about constructing the account SAS token.</span></span>

## <a name="how-a-shared-access-signature-works"></a><span data-ttu-id="9eadc-153">Jak funguje sdílený přístupový podpis</span><span class="sxs-lookup"><span data-stu-id="9eadc-153">How a shared access signature works</span></span>
<span data-ttu-id="9eadc-154">Sdílený přístupový podpis je podepsaný identifikátor URI, který odkazuje na jeden nebo více prostředků úložiště a obsahuje token, který obsahuje speciální sadu parametry dotazu.</span><span class="sxs-lookup"><span data-stu-id="9eadc-154">A shared access signature is a signed URI that points to one or more storage resources and includes a token that contains a special set of query parameters.</span></span> <span data-ttu-id="9eadc-155">Token Určuje, jak můžete získat přístup k prostředkům na klientem.</span><span class="sxs-lookup"><span data-stu-id="9eadc-155">The token indicates how the resources may be accessed by the client.</span></span> <span data-ttu-id="9eadc-156">Jeden z parametrů dotazu, podpisu, se vytvářejí na základě parametrů SAS a podepsaný pomocí klíč účtu.</span><span class="sxs-lookup"><span data-stu-id="9eadc-156">One of the query parameters, the signature, is constructed from the SAS parameters and signed with the account key.</span></span> <span data-ttu-id="9eadc-157">Tento podpis úložiště Azure slouží k ověřování SAS.</span><span class="sxs-lookup"><span data-stu-id="9eadc-157">This signature is used by Azure Storage to authenticate the SAS.</span></span>

<span data-ttu-id="9eadc-158">Tady je příklad identifikátoru URI SAS, zobrazuje identifikátor URI a tokenu SAS:</span><span class="sxs-lookup"><span data-stu-id="9eadc-158">Here's an example of a SAS URI, showing the resource URI and the SAS token:</span></span>

![Komponenty identifikátoru URI SAS][sas-storage-uri]

<span data-ttu-id="9eadc-160">SAS token je řetězec vygenerujete na *klienta* straně (najdete v článku [příklady SAS](#sas-examples) části Příklady kódu).</span><span class="sxs-lookup"><span data-stu-id="9eadc-160">The SAS token is a string you generate on the *client* side (see the [SAS examples](#sas-examples) section for code examples).</span></span> <span data-ttu-id="9eadc-161">Token SAS, který generovat s Klientská knihovna pro úložiště, například není sledována úložiště Azure žádným způsobem.</span><span class="sxs-lookup"><span data-stu-id="9eadc-161">A SAS token you generate with the storage client library, for example, is not tracked by Azure Storage in any way.</span></span> <span data-ttu-id="9eadc-162">Můžete vytvořit neomezený počet tokeny SAS na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="9eadc-162">You can create an unlimited number of SAS tokens on the client side.</span></span>

<span data-ttu-id="9eadc-163">Když klient poskytuje identifikátor URI SAS pro úložiště Azure jako součást požadavku, služba zkontroluje parametry SAS a definice virů, aby ověřte, zda je platný pro ověření žádosti.</span><span class="sxs-lookup"><span data-stu-id="9eadc-163">When a client provides a SAS URI to Azure Storage as part of a request, the service checks the SAS parameters and signature to verify that it is valid for authenticating the request.</span></span> <span data-ttu-id="9eadc-164">Pokud službu ověřuje, že podpis je platný, pak je požadavek ověřen.</span><span class="sxs-lookup"><span data-stu-id="9eadc-164">If the service verifies that the signature is valid, then the request is authenticated.</span></span> <span data-ttu-id="9eadc-165">Jinak je žádost odmítnuta s kódem chyby 403 (zakázáno).</span><span class="sxs-lookup"><span data-stu-id="9eadc-165">Otherwise, the request is declined with error code 403 (Forbidden).</span></span>

## <a name="shared-access-signature-parameters"></a><span data-ttu-id="9eadc-166">Sdílený přístupový podpis parametry</span><span class="sxs-lookup"><span data-stu-id="9eadc-166">Shared access signature parameters</span></span>
<span data-ttu-id="9eadc-167">SAS účtu a tokeny SAS služby zahrnují některé společné parametry a také provést několik parametrů, aby se liší.</span><span class="sxs-lookup"><span data-stu-id="9eadc-167">The account SAS and service SAS tokens include some common parameters, and also take a few parameters that that are different.</span></span>

### <a name="parameters-common-to-account-sas-and-service-sas-tokens"></a><span data-ttu-id="9eadc-168">Parametry, které jsou společné pro SAS účtu a tokeny SAS služby</span><span class="sxs-lookup"><span data-stu-id="9eadc-168">Parameters common to account SAS and service SAS tokens</span></span>
* <span data-ttu-id="9eadc-169">**Verze rozhraní API** volitelný parametr, který určuje verzi služby úložiště používat k provedení požadavku.</span><span class="sxs-lookup"><span data-stu-id="9eadc-169">**Api version** An optional parameter that specifies the storage service version to use to execute the request.</span></span>
* <span data-ttu-id="9eadc-170">**Verze služby** povinný parametr, který určuje verzi služby úložiště používat k ověření žádosti.</span><span class="sxs-lookup"><span data-stu-id="9eadc-170">**Service version** A required parameter that specifies the storage service version to use to authenticate the request.</span></span>
* <span data-ttu-id="9eadc-171">**Počáteční čas.**</span><span class="sxs-lookup"><span data-stu-id="9eadc-171">**Start time.**</span></span> <span data-ttu-id="9eadc-172">Toto je čas, kdy začne platit SAS.</span><span class="sxs-lookup"><span data-stu-id="9eadc-172">This is the time at which the SAS becomes valid.</span></span> <span data-ttu-id="9eadc-173">Čas zahájení pro sdílený přístupový podpis je volitelný.</span><span class="sxs-lookup"><span data-stu-id="9eadc-173">The start time for a shared access signature is optional.</span></span> <span data-ttu-id="9eadc-174">Pokud čas spuštění je vynechán, SAS je hned platná.</span><span class="sxs-lookup"><span data-stu-id="9eadc-174">If a start time is omitted, the SAS is effective immediately.</span></span> <span data-ttu-id="9eadc-175">Čas spuštění musí být vyjádřena v UTC (Coordinated Universal Time), s speciální označení UTC ("Z"), například `1994-11-05T13:15:30Z`.</span><span class="sxs-lookup"><span data-stu-id="9eadc-175">The start time must be expressed in UTC (Coordinated Universal Time), with a special UTC designator ("Z"), for example `1994-11-05T13:15:30Z`.</span></span>
* <span data-ttu-id="9eadc-176">**Čas vypršení platnosti.**</span><span class="sxs-lookup"><span data-stu-id="9eadc-176">**Expiry time.**</span></span> <span data-ttu-id="9eadc-177">Toto je čas, po jejímž uplynutí SAS již není platný.</span><span class="sxs-lookup"><span data-stu-id="9eadc-177">This is the time after which the SAS is no longer valid.</span></span> <span data-ttu-id="9eadc-178">Osvědčené postupy doporučujeme, abyste zadejte čas vypršení platnosti pro SAS, nebo přidružit k zásadám uložené přístup.</span><span class="sxs-lookup"><span data-stu-id="9eadc-178">Best practices recommend that you either specify an expiry time for a SAS, or associate it with a stored access policy.</span></span> <span data-ttu-id="9eadc-179">Čas vypršení platnosti musí být vyjádřena v UTC (Coordinated Universal Time), s speciální označení UTC ("Z"), například `1994-11-05T13:15:30Z` (Další informace níže).</span><span class="sxs-lookup"><span data-stu-id="9eadc-179">The expiry time must be expressed in UTC (Coordinated Universal Time), with a special UTC designator ("Z"), for example `1994-11-05T13:15:30Z` (see more below).</span></span>
* <span data-ttu-id="9eadc-180">**Oprávnění.**</span><span class="sxs-lookup"><span data-stu-id="9eadc-180">**Permissions.**</span></span> <span data-ttu-id="9eadc-181">Oprávnění určená v SAS znamenat jakým operacím klienta můžete provést u prostředků úložiště pomocí SAS.</span><span class="sxs-lookup"><span data-stu-id="9eadc-181">The permissions specified on the SAS indicate what operations the client can perform against the storage resource using the SAS.</span></span> <span data-ttu-id="9eadc-182">K dispozici oprávnění se liší pro SAS účtu a SAS služby.</span><span class="sxs-lookup"><span data-stu-id="9eadc-182">Available permissions differ for an account SAS and a service SAS.</span></span>
* <span data-ttu-id="9eadc-183">**IP.**</span><span class="sxs-lookup"><span data-stu-id="9eadc-183">**IP.**</span></span> <span data-ttu-id="9eadc-184">Volitelný parametr, který určuje IP adresu nebo rozsah IP adres mimo Azure (naleznete v části [stav konfigurace relace směrování](../expressroute/expressroute-workflows.md#routing-session-configuration-state) pro Express Route) ze kterého tak, aby přijímal požadavky.</span><span class="sxs-lookup"><span data-stu-id="9eadc-184">An optional parameter that specifies an IP address or a range of IP addresses outside of Azure (see the section [Routing session configuration state](../expressroute/expressroute-workflows.md#routing-session-configuration-state) for Express Route) from which to accept requests.</span></span>
* <span data-ttu-id="9eadc-185">**Protokol.**</span><span class="sxs-lookup"><span data-stu-id="9eadc-185">**Protocol.**</span></span> <span data-ttu-id="9eadc-186">Volitelný parametr, který určuje protokol, povolené pro žádost.</span><span class="sxs-lookup"><span data-stu-id="9eadc-186">An optional parameter that specifies the protocol permitted for a request.</span></span> <span data-ttu-id="9eadc-187">Možné hodnoty jsou protokolů HTTPS a HTTP (`https,http`), který je pouze výchozí hodnotu, nebo HTTPS (`https`).</span><span class="sxs-lookup"><span data-stu-id="9eadc-187">Possible values are both HTTPS and HTTP (`https,http`), which is the default value, or HTTPS only (`https`).</span></span> <span data-ttu-id="9eadc-188">Všimněte si, že HTTP jenom není povolenou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9eadc-188">Note that HTTP only is not a permitted value.</span></span>
* <span data-ttu-id="9eadc-189">**Podpis.**</span><span class="sxs-lookup"><span data-stu-id="9eadc-189">**Signature.**</span></span> <span data-ttu-id="9eadc-190">Podpis se vytvářejí na základě ostatní parametry zadaný jako součást tokenu a pak se zašifrují.</span><span class="sxs-lookup"><span data-stu-id="9eadc-190">The signature is constructed from the other parameters specified as part token and then encrypted.</span></span> <span data-ttu-id="9eadc-191">Slouží k ověření SAS.</span><span class="sxs-lookup"><span data-stu-id="9eadc-191">It's used to authenticate the SAS.</span></span>

### <a name="parameters-for-a-service-sas-token"></a><span data-ttu-id="9eadc-192">Parametry pro token SAS služby</span><span class="sxs-lookup"><span data-stu-id="9eadc-192">Parameters for a service SAS token</span></span>
* <span data-ttu-id="9eadc-193">**Úložiště prostředků.**</span><span class="sxs-lookup"><span data-stu-id="9eadc-193">**Storage resource.**</span></span> <span data-ttu-id="9eadc-194">Úložiště prostředků, pro které můžete delegovat přístup se službou SAS patří:</span><span class="sxs-lookup"><span data-stu-id="9eadc-194">Storage resources for which you can delegate access with a service SAS include:</span></span>
  * <span data-ttu-id="9eadc-195">Kontejnery a objekty BLOB</span><span class="sxs-lookup"><span data-stu-id="9eadc-195">Containers and blobs</span></span>
  * <span data-ttu-id="9eadc-196">Sdílených složek a souborů</span><span class="sxs-lookup"><span data-stu-id="9eadc-196">File shares and files</span></span>
  * <span data-ttu-id="9eadc-197">Fronty</span><span class="sxs-lookup"><span data-stu-id="9eadc-197">Queues</span></span>
  * <span data-ttu-id="9eadc-198">Tabulky a rozsahy adres entity tabulky.</span><span class="sxs-lookup"><span data-stu-id="9eadc-198">Tables and ranges of table entities.</span></span>

### <a name="parameters-for-an-account-sas-token"></a><span data-ttu-id="9eadc-199">Parametry pro token SAS účtu</span><span class="sxs-lookup"><span data-stu-id="9eadc-199">Parameters for an account SAS token</span></span>
* <span data-ttu-id="9eadc-200">**Služba nebo služby.**</span><span class="sxs-lookup"><span data-stu-id="9eadc-200">**Service or services.**</span></span> <span data-ttu-id="9eadc-201">SAS účtu můžete delegovat přístup k jednomu nebo více službách úložiště.</span><span class="sxs-lookup"><span data-stu-id="9eadc-201">An account SAS can delegate access to one or more of the storage services.</span></span> <span data-ttu-id="9eadc-202">Například můžete vytvořit účet SAS, delegáti přístup ke službě Blob a souboru.</span><span class="sxs-lookup"><span data-stu-id="9eadc-202">For example, you can create an account SAS that delegates access to the Blob and File service.</span></span> <span data-ttu-id="9eadc-203">Nebo můžete vytvořit SAS, delegáti přístup k všechny čtyři služeb (objekt Blob, fronty, tabulky a soubor).</span><span class="sxs-lookup"><span data-stu-id="9eadc-203">Or you can create a SAS that delegates access to all four services (Blob, Queue, Table, and File).</span></span>
* <span data-ttu-id="9eadc-204">**Typy prostředků úložiště.**</span><span class="sxs-lookup"><span data-stu-id="9eadc-204">**Storage resource types.**</span></span> <span data-ttu-id="9eadc-205">Účet SAS se vztahuje na jeden nebo více tříd prostředky úložiště, nikoli konkrétní prostředek.</span><span class="sxs-lookup"><span data-stu-id="9eadc-205">An account SAS applies to one or more classes of storage resources, rather than a specific resource.</span></span> <span data-ttu-id="9eadc-206">Můžete vytvořit SAS pro delegování přístupu k účtu:</span><span class="sxs-lookup"><span data-stu-id="9eadc-206">You can create an account SAS to delegate access to:</span></span>
  * <span data-ttu-id="9eadc-207">API úrovně služeb, které se nazývají proti prostředků účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="9eadc-207">Service-level APIs, which are called against the storage account resource.</span></span> <span data-ttu-id="9eadc-208">Mezi příklady patří **vlastnosti služby Get/Set**, **získat statistiky služby**, a **seznamu kontejnery nebo fronty nebo tabulky nebo sdílené složky**.</span><span class="sxs-lookup"><span data-stu-id="9eadc-208">Examples include **Get/Set Service Properties**, **Get Service Stats**, and **List Containers/Queues/Tables/Shares**.</span></span>
  * <span data-ttu-id="9eadc-209">Kontejner úrovni rozhraní API, které se nazývají u těchto objektů kontejneru pro každou službu: blob kontejnery, front, tabulky a sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="9eadc-209">Container-level APIs, which are called against the container objects for each service: blob containers, queues, tables, and file shares.</span></span> <span data-ttu-id="9eadc-210">Mezi příklady patří **vytvoření nebo odstranění kontejneru**, **vytvoření nebo odstranění fronty**, **vytvoření nebo odstranění tabulky**, **vytvoření nebo odstranění sdílené složky**a  **Seznam objektů BLOB nebo souborů a adresářů**.</span><span class="sxs-lookup"><span data-stu-id="9eadc-210">Examples include **Create/Delete Container**, **Create/Delete Queue**, **Create/Delete Table**, **Create/Delete Share**, and **List Blobs/Files and Directories**.</span></span>
  * <span data-ttu-id="9eadc-211">API úrovni objektů, které se nazývají proti objekty BLOB, fronty zpráv, entity tabulky a soubory.</span><span class="sxs-lookup"><span data-stu-id="9eadc-211">Object-level APIs, which are called against blobs, queue messages, table entities, and files.</span></span> <span data-ttu-id="9eadc-212">Například **Put Blob**, **dotazu Entity**, **získat zprávy**, a **vytvořit soubor**.</span><span class="sxs-lookup"><span data-stu-id="9eadc-212">For example, **Put Blob**, **Query Entity**, **Get Messages**, and **Create File**.</span></span>

## <a name="examples-of-sas-uris"></a><span data-ttu-id="9eadc-213">Příklady identifikátorů URI SAS</span><span class="sxs-lookup"><span data-stu-id="9eadc-213">Examples of SAS URIs</span></span>

### <a name="service-sas-uri-example"></a><span data-ttu-id="9eadc-214">Příklad identifikátor URI SAS služby</span><span class="sxs-lookup"><span data-stu-id="9eadc-214">Service SAS URI example</span></span>

<span data-ttu-id="9eadc-215">Tady je příklad služby identifikátor URI SAS, který poskytuje oprávnění čtení a zápisu do objektu blob.</span><span class="sxs-lookup"><span data-stu-id="9eadc-215">Here is an example of a service SAS URI that provides read and write permissions to a blob.</span></span> <span data-ttu-id="9eadc-216">Dojde-li jednotlivých součástí identifikátoru URI pochopit, jak přispívá ke SAS v tabulce:</span><span class="sxs-lookup"><span data-stu-id="9eadc-216">The table breaks down each part of the URI to understand how it contributes to the SAS:</span></span>

```
https://myaccount.blob.core.windows.net/sascontainer/sasblob.txt?sv=2015-04-05&st=2015-04-29T22%3A18%3A26Z&se=2015-04-30T02%3A23%3A26Z&sr=b&sp=rw&sip=168.1.5.60-168.1.5.70&spr=https&sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D
```

| <span data-ttu-id="9eadc-217">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="9eadc-217">Name</span></span> | <span data-ttu-id="9eadc-218">Část SAS</span><span class="sxs-lookup"><span data-stu-id="9eadc-218">SAS portion</span></span> | <span data-ttu-id="9eadc-219">Popis</span><span class="sxs-lookup"><span data-stu-id="9eadc-219">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9eadc-220">Identifikátor URI objektu BLOB</span><span class="sxs-lookup"><span data-stu-id="9eadc-220">Blob URI</span></span> |`https://myaccount.blob.core.windows.net/sascontainer/sasblob.txt` |<span data-ttu-id="9eadc-221">Adresa objektu blob.</span><span class="sxs-lookup"><span data-stu-id="9eadc-221">The address of the blob.</span></span> <span data-ttu-id="9eadc-222">Všimněte si, že pomocí protokolu HTTPS se důrazně doporučuje.</span><span class="sxs-lookup"><span data-stu-id="9eadc-222">Note that using HTTPS is highly recommended.</span></span> |
| <span data-ttu-id="9eadc-223">Verze služby úložiště</span><span class="sxs-lookup"><span data-stu-id="9eadc-223">Storage services version</span></span> |`sv=2015-04-05` |<span data-ttu-id="9eadc-224">Pro úložiště služby verze 2012-02-12 a novější, tento parametr určuje verze se má použít.</span><span class="sxs-lookup"><span data-stu-id="9eadc-224">For storage services version 2012-02-12 and later, this parameter indicates the version to use.</span></span> |
| <span data-ttu-id="9eadc-225">Čas spuštění</span><span class="sxs-lookup"><span data-stu-id="9eadc-225">Start time</span></span> |`st=2015-04-29T22%3A18%3A26Z` |<span data-ttu-id="9eadc-226">Zadat ve formátu času UTC.</span><span class="sxs-lookup"><span data-stu-id="9eadc-226">Specified in UTC time.</span></span> <span data-ttu-id="9eadc-227">Pokud chcete SAS platit okamžitě, vynechejte čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="9eadc-227">If you want the SAS to be valid immediately, omit the start time.</span></span> |
| <span data-ttu-id="9eadc-228">Čas vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="9eadc-228">Expiry time</span></span> |`se=2015-04-30T02%3A23%3A26Z` |<span data-ttu-id="9eadc-229">Zadat ve formátu času UTC.</span><span class="sxs-lookup"><span data-stu-id="9eadc-229">Specified in UTC time.</span></span> |
| <span data-ttu-id="9eadc-230">Prostředek</span><span class="sxs-lookup"><span data-stu-id="9eadc-230">Resource</span></span> |`sr=b` |<span data-ttu-id="9eadc-231">Daný prostředek k objektu blob.</span><span class="sxs-lookup"><span data-stu-id="9eadc-231">The resource is a blob.</span></span> |
| <span data-ttu-id="9eadc-232">Oprávnění</span><span class="sxs-lookup"><span data-stu-id="9eadc-232">Permissions</span></span> |`sp=rw` |<span data-ttu-id="9eadc-233">Oprávnění udělená pomocí sdíleného přístupového podpisu zahrnout Read (r) a zápis (w).</span><span class="sxs-lookup"><span data-stu-id="9eadc-233">The permissions granted by the SAS include Read (r) and Write (w).</span></span> |
| <span data-ttu-id="9eadc-234">Rozsah IP adres</span><span class="sxs-lookup"><span data-stu-id="9eadc-234">IP range</span></span> |`sip=168.1.5.60-168.1.5.70` |<span data-ttu-id="9eadc-235">Rozsah IP adres, ze kterých budou přijímány žádost.</span><span class="sxs-lookup"><span data-stu-id="9eadc-235">The range of IP addresses from which a request will be accepted.</span></span> |
| <span data-ttu-id="9eadc-236">Protocol (Protokol)</span><span class="sxs-lookup"><span data-stu-id="9eadc-236">Protocol</span></span> |`spr=https` |<span data-ttu-id="9eadc-237">Jsou povoleny pouze požadavky pomocí protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9eadc-237">Only requests using HTTPS are permitted.</span></span> |
| <span data-ttu-id="9eadc-238">Podpis</span><span class="sxs-lookup"><span data-stu-id="9eadc-238">Signature</span></span> |`sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D` |<span data-ttu-id="9eadc-239">Používá k ověření přístupu k objektu blob.</span><span class="sxs-lookup"><span data-stu-id="9eadc-239">Used to authenticate access to the blob.</span></span> <span data-ttu-id="9eadc-240">Podpis je klíčem HMAC, vypočítán na řetězec k podepsání a klíč pomocí algoritmus SHA256 a pak zakódován pomocí kódování Base64.</span><span class="sxs-lookup"><span data-stu-id="9eadc-240">The signature is an HMAC computed over a string-to-sign and key using the SHA256 algorithm, and then encoded using Base64 encoding.</span></span> |

### <a name="account-sas-uri-example"></a><span data-ttu-id="9eadc-241">Příklad identifikátor URI pro SAS účtu</span><span class="sxs-lookup"><span data-stu-id="9eadc-241">Account SAS URI example</span></span>

<span data-ttu-id="9eadc-242">Tady je příklad účtu SAS, který používá stejné společné parametry na tokenu.</span><span class="sxs-lookup"><span data-stu-id="9eadc-242">Here is an example of an account SAS that uses the same common parameters on the token.</span></span> <span data-ttu-id="9eadc-243">Vzhledem k tomu, že tyto parametry jsou popsané výše, nejsou popsané v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="9eadc-243">Since these parameters are described above, they are not described here.</span></span> <span data-ttu-id="9eadc-244">Jenom parametry, které jsou specifické pro účet, který SAS, které jsou popsané v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="9eadc-244">Only the parameters that are specific to account SAS are described in the table below.</span></span>

```
https://myaccount.blob.core.windows.net/?restype=service&comp=properties&sv=2015-04-05&ss=bf&srt=s&st=2015-04-29T22%3A18%3A26Z&se=2015-04-30T02%3A23%3A26Z&sr=b&sp=rw&sip=168.1.5.60-168.1.5.70&spr=https&sig=F%6GRVAZ5Cdj2Pw4tgU7IlSTkWgn7bUkkAg8P6HESXwmf%4B
```

| <span data-ttu-id="9eadc-245">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="9eadc-245">Name</span></span> | <span data-ttu-id="9eadc-246">Část SAS</span><span class="sxs-lookup"><span data-stu-id="9eadc-246">SAS portion</span></span> | <span data-ttu-id="9eadc-247">Popis</span><span class="sxs-lookup"><span data-stu-id="9eadc-247">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9eadc-248">Identifikátor URI</span><span class="sxs-lookup"><span data-stu-id="9eadc-248">Resource URI</span></span> |`https://myaccount.blob.core.windows.net/?restype=service&comp=properties` |<span data-ttu-id="9eadc-249">Objekt Blob koncový bod služby, s parametry pro získávání vlastnosti služby (Pokud je volána s GET) nebo nastavení vlastností služby (při volání sadou).</span><span class="sxs-lookup"><span data-stu-id="9eadc-249">The Blob service endpoint, with parameters for getting service properties (when called with GET) or setting service properties (when called with SET).</span></span> |
| <span data-ttu-id="9eadc-250">Služby</span><span class="sxs-lookup"><span data-stu-id="9eadc-250">Services</span></span> |`ss=bf` |<span data-ttu-id="9eadc-251">SAS se vztahuje na služby objektů Blob a souborů</span><span class="sxs-lookup"><span data-stu-id="9eadc-251">The SAS applies to the Blob and File services</span></span> |
| <span data-ttu-id="9eadc-252">Typy prostředků</span><span class="sxs-lookup"><span data-stu-id="9eadc-252">Resource types</span></span> |`srt=s` |<span data-ttu-id="9eadc-253">SAS se vztahuje k operacím na úrovni služby.</span><span class="sxs-lookup"><span data-stu-id="9eadc-253">The SAS applies to service-level operations.</span></span> |
| <span data-ttu-id="9eadc-254">Oprávnění</span><span class="sxs-lookup"><span data-stu-id="9eadc-254">Permissions</span></span> |`sp=rw` |<span data-ttu-id="9eadc-255">Oprávnění udělit přístup k operacích čtení a zápisu.</span><span class="sxs-lookup"><span data-stu-id="9eadc-255">The permissions grant access to read and write operations.</span></span> |

<span data-ttu-id="9eadc-256">Vzhledem k tomu, že oprávnění jsou omezeny na úrovni služby, jsou přístupné operací s Tento SAS **získat vlastnosti objektu Blob služby** (čtení) a **nastavit vlastnosti služby objektů Blob** (zápisu).</span><span class="sxs-lookup"><span data-stu-id="9eadc-256">Given that permissions are restricted to the service level, accessible operations with this SAS are **Get Blob Service Properties** (read) and **Set Blob Service Properties** (write).</span></span> <span data-ttu-id="9eadc-257">Ale a jiný zdroj v URI stejný token SAS také může delegovat přístup k **získat statistiky služby objektů Blob** (načíst).</span><span class="sxs-lookup"><span data-stu-id="9eadc-257">However, with a different resource URI, the same SAS token could also be used to delegate access to **Get Blob Service Stats** (read).</span></span>

## <a name="controlling-a-sas-with-a-stored-access-policy"></a><span data-ttu-id="9eadc-258">Řízení SAS se zásadami uložené přístupu</span><span class="sxs-lookup"><span data-stu-id="9eadc-258">Controlling a SAS with a stored access policy</span></span>
<span data-ttu-id="9eadc-259">Sdílený přístupový podpis může trvat dvě formy:</span><span class="sxs-lookup"><span data-stu-id="9eadc-259">A shared access signature can take one of two forms:</span></span>

* <span data-ttu-id="9eadc-260">**Ad hoc SAS:** když vytvoříte ad hoc SAS, čas zahájení, čas vypršení platnosti a oprávnění pro SAS se všechny zadaný identifikátor URI SAS (nebo v případě, kdy je čas spuštění vynechán odvozených).</span><span class="sxs-lookup"><span data-stu-id="9eadc-260">**Ad hoc SAS:** When you create an ad hoc SAS, the start time, expiry time, and permissions for the SAS are all specified in the SAS URI (or implied, in the case where start time is omitted).</span></span> <span data-ttu-id="9eadc-261">Tento typ SAS se dá vytvořit jako SAS účtu nebo SAS služby.</span><span class="sxs-lookup"><span data-stu-id="9eadc-261">This type of SAS can be created as an account SAS or a service SAS.</span></span>
* <span data-ttu-id="9eadc-262">**SAS se zásadami přístupu uložené:** zásadu uložené přístupu je definovaný na kontejner prostředku--kontejner objektů blob, tabulky, fronty, nebo sdílení – souborů a slouží ke správě omezení pro jeden nebo více sdílených přístupových podpisů.</span><span class="sxs-lookup"><span data-stu-id="9eadc-262">**SAS with stored access policy:** A stored access policy is defined on a resource container--a blob container, table, queue, or file share--and can be used to manage constraints for one or more shared access signatures.</span></span> <span data-ttu-id="9eadc-263">Pokud přidružíte SAS se zásadami přístupu uložené, zdědí SAS omezení – čas spuštění, čas vypršení platnosti a--definována pro zásady uložené přístupu oprávnění.</span><span class="sxs-lookup"><span data-stu-id="9eadc-263">When you associate a SAS with a stored access policy, the SAS inherits the constraints--the start time, expiry time, and permissions--defined for the stored access policy.</span></span>

> [!NOTE]
> <span data-ttu-id="9eadc-264">SAS účtu musí být v současné době ad hoc SAS.</span><span class="sxs-lookup"><span data-stu-id="9eadc-264">Currently, an account SAS must be an ad hoc SAS.</span></span> <span data-ttu-id="9eadc-265">Uložených přístupu zásady se ještě nepodporují pro SAS účtu.</span><span class="sxs-lookup"><span data-stu-id="9eadc-265">Stored access policies are not yet supported for account SAS.</span></span>

<span data-ttu-id="9eadc-266">Rozdíl mezi dvěma formuláři je důležité pro jeden klíč scénář: odvolaných certifikátů.</span><span class="sxs-lookup"><span data-stu-id="9eadc-266">The difference between the two forms is important for one key scenario: revocation.</span></span> <span data-ttu-id="9eadc-267">Protože identifikátor URI SAS je adresa URL, každý uživatel, který získá SAS, můžete použít bez ohledu na to, který byl původně vytvořen.</span><span class="sxs-lookup"><span data-stu-id="9eadc-267">Because a SAS URI is a URL, anyone that obtains the SAS can use it, regardless of who originally created it.</span></span> <span data-ttu-id="9eadc-268">Pokud veřejně publikována SAS, můžete použít kdokoli na světě.</span><span class="sxs-lookup"><span data-stu-id="9eadc-268">If a SAS is published publicly, it can be used by anyone in the world.</span></span> <span data-ttu-id="9eadc-269">SAS udělí přístup k prostředkům všem uživatelům, kteří mají dokud jednu ze čtyř akcí se stane:</span><span class="sxs-lookup"><span data-stu-id="9eadc-269">A SAS grants access to resources to anyone possessing it until one of four things happens:</span></span>

1. <span data-ttu-id="9eadc-270">Je dosaženo času vypršení platnosti, zadaný na SAS.</span><span class="sxs-lookup"><span data-stu-id="9eadc-270">The expiry time specified on the SAS is reached.</span></span>
2. <span data-ttu-id="9eadc-271">Čas vypršení platnosti zadané v zásadách přístupu uložené odkazuje SAS je dosaženo (Pokud je odkazována zásadu uložené přístupu a určuje, že čas vypršení platnosti).</span><span class="sxs-lookup"><span data-stu-id="9eadc-271">The expiry time specified on the stored access policy referenced by the SAS is reached (if a stored access policy is referenced, and if it specifies an expiry time).</span></span> <span data-ttu-id="9eadc-272">Tato situace může nastat, protože uplyne, nebo úpravě zásad přístupu uložené, doba vypršení platnosti v minulosti, což je jeden způsob odvolání SAS.</span><span class="sxs-lookup"><span data-stu-id="9eadc-272">This can occur either because the interval elapses, or because you've modified the stored access policy with an expiry time in the past, which is one way to revoke the SAS.</span></span>
3. <span data-ttu-id="9eadc-273">Zásady přístupu uložené odkazuje SAS je odstranit, což je další způsob odvolání SAS.</span><span class="sxs-lookup"><span data-stu-id="9eadc-273">The stored access policy referenced by the SAS is deleted, which is another way to revoke the SAS.</span></span> <span data-ttu-id="9eadc-274">Upozorňujeme, že pokud je znovu vytvořit zásady uložené přístupu se stejným názvem, všechny existující tokeny SAS bude znovu být platný podle oprávnění spojená s uložené přístup zásad (za předpokladu, že který neuplynul čas vypršení platnosti na sdíleného přístupového podpisu).</span><span class="sxs-lookup"><span data-stu-id="9eadc-274">Note that if you recreate the stored access policy with exactly the same name, all existing SAS tokens will again be valid according to the permissions associated with that stored access policy (assuming that the expiry time on the SAS has not passed).</span></span> <span data-ttu-id="9eadc-275">Pokud se hodláte odvolání SAS, je nutné použít jiný název, pokud je znovu vytvořit zásady přístupu, doba vypršení platnosti v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="9eadc-275">If you are intending to revoke the SAS, be sure to use a different name if you recreate the access policy with an expiry time in the future.</span></span>
4. <span data-ttu-id="9eadc-276">Znovu vygeneruje klíč účtu, který byl použit k vytvoření přidružení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="9eadc-276">The account key that was used to create the SAS is regenerated.</span></span> <span data-ttu-id="9eadc-277">Znovu vygenerovat klíč účtu způsobí, že všechny součásti aplikace pomocí tohoto klíče selhání k ověření, dokud se aktualizovány používat další platný účet klíč nebo klíč nově se znova vygeneroval účtu.</span><span class="sxs-lookup"><span data-stu-id="9eadc-277">Regenerating an account key will cause all application components using that key to fail to authenticate until they're updated to use either the other valid account key or the newly regenerated account key.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9eadc-278">Sdílený přístupový podpis identifikátor URI je spojena s účet klíč používaný k vytvoření podpisu a přidruženého uložené zásady přístupu (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="9eadc-278">A shared access signature URI is associated with the account key used to create the signature, and the associated stored access policy (if any).</span></span> <span data-ttu-id="9eadc-279">Pokud je zadána žádná zásada uložené přístup, jediný způsob, jak odvolat sdílený přístupový podpis je změnit klíč účtu.</span><span class="sxs-lookup"><span data-stu-id="9eadc-279">If no stored access policy is specified, the only way to revoke a shared access signature is to change the account key.</span></span>

## <a name="authenticating-from-a-client-application-with-a-sas"></a><span data-ttu-id="9eadc-280">Ověřování z klientské aplikace s SAS</span><span class="sxs-lookup"><span data-stu-id="9eadc-280">Authenticating from a client application with a SAS</span></span>
<span data-ttu-id="9eadc-281">Klienta, která je vlastníkem SAS slouží k ověření požadavku vůči účet úložiště, pro kterou, které nemají klíče účtu SAS.</span><span class="sxs-lookup"><span data-stu-id="9eadc-281">A client who is in possession of a SAS can use the SAS to authenticate a request against a storage account for which they do not possess the account keys.</span></span> <span data-ttu-id="9eadc-282">SAS můžete zahrnout do připojovací řetězec nebo použití přímo z metodu, nebo odpovídající konstruktor.</span><span class="sxs-lookup"><span data-stu-id="9eadc-282">A SAS can be included in a connection string, or used directly from the appropriate constructor or method.</span></span>

### <a name="using-a-sas-in-a-connection-string"></a><span data-ttu-id="9eadc-283">Pomocí SAS v připojovacím řetězci</span><span class="sxs-lookup"><span data-stu-id="9eadc-283">Using a SAS in a connection string</span></span>
[!INCLUDE [storage-use-sas-in-connection-string-include](../../includes/storage-use-sas-in-connection-string-include.md)]

### <a name="using-a-sas-in-a-constructor-or-method"></a><span data-ttu-id="9eadc-284">Pomocí SAS v konstruktoru nebo – metoda</span><span class="sxs-lookup"><span data-stu-id="9eadc-284">Using a SAS in a constructor or method</span></span>
<span data-ttu-id="9eadc-285">Několik konstruktorů knihovny klienta Azure Storage a přetížení metody nabízejí parametr SAS, aby se můžete ověřit žádost o služby pomocí SAS.</span><span class="sxs-lookup"><span data-stu-id="9eadc-285">Several Azure Storage client library constructors and method overloads offer a SAS parameter, so that you can authenticate a request to the service with a SAS.</span></span>

<span data-ttu-id="9eadc-286">Zde je například identifikátor URI SAS použít k vytvoření odkaz na objekt blob bloku.</span><span class="sxs-lookup"><span data-stu-id="9eadc-286">For example, here a SAS URI is used to create a reference to a block blob.</span></span> <span data-ttu-id="9eadc-287">SAS poskytuje jenom přihlašovacích údajů nezbytných pro požadavek.</span><span class="sxs-lookup"><span data-stu-id="9eadc-287">The SAS provides the only credentials needed for the request.</span></span> <span data-ttu-id="9eadc-288">Odkaz na objekt blob bloku se pak použije pro operaci zápisu:</span><span class="sxs-lookup"><span data-stu-id="9eadc-288">The block blob reference is then used for a write operation:</span></span>

```csharp
string sasUri = "https://storagesample.blob.core.windows.net/sample-container/" +
    "sampleBlob.txt?sv=2015-07-08&sr=b&sig=39Up9JzHkxhUIhFEjEH9594DJxe7w6cIRCg0V6lCGSo%3D" +
    "&se=2016-10-18T21%3A51%3A37Z&sp=rcw";

CloudBlockBlob blob = new CloudBlockBlob(new Uri(sasUri));

// Create operation: Upload a blob with the specified name to the container.
// If the blob does not exist, it will be created. If it does exist, it will be overwritten.
try
{
    MemoryStream msWrite = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
    msWrite.Position = 0;
    using (msWrite)
    {
        await blob.UploadFromStreamAsync(msWrite);
    }

    Console.WriteLine("Create operation succeeded for SAS {0}", sasUri);
    Console.WriteLine();
}
catch (StorageException e)
{
    if (e.RequestInformation.HttpStatusCode == 403)
    {
        Console.WriteLine("Create operation failed for SAS {0}", sasUri);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }
    else
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}

```

## <a name="best-practices-when-using-sas"></a><span data-ttu-id="9eadc-289">Osvědčené postupy při použití SAS</span><span class="sxs-lookup"><span data-stu-id="9eadc-289">Best practices when using SAS</span></span>
<span data-ttu-id="9eadc-290">Při použití sdílených přístupových podpisů v aplikacích, budete muset mít na paměti dva potenciální rizika:</span><span class="sxs-lookup"><span data-stu-id="9eadc-290">When you use shared access signatures in your applications, you need to be aware of two potential risks:</span></span>

* <span data-ttu-id="9eadc-291">Pokud SAS došlo k úniku, můžete použít libovolný uživatel, který získává, což může potenciálně ohrozit váš účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="9eadc-291">If a SAS is leaked, it can be used by anyone who obtains it, which can potentially compromise your storage account.</span></span>
* <span data-ttu-id="9eadc-292">Pokud SAS poskytované vyprší platnost klientskou aplikaci a aplikace se nepodařilo načíst novou SAS ze služby, pak může být narušen funkcí aplikace.</span><span class="sxs-lookup"><span data-stu-id="9eadc-292">If a SAS provided to a client application expires and the application is unable to retrieve a new SAS from your service, then the application's functionality may be hindered.</span></span>

<span data-ttu-id="9eadc-293">Následující doporučení pro použití sdílených přístupových podpisů můžete zmírnit tato rizika:</span><span class="sxs-lookup"><span data-stu-id="9eadc-293">The following recommendations for using shared access signatures can help mitigate these risks:</span></span>

1. <span data-ttu-id="9eadc-294">**Vždycky používají protokol HTTPS** k vytvoření nebo distribuovat SAS.</span><span class="sxs-lookup"><span data-stu-id="9eadc-294">**Always use HTTPS** to create or distribute a SAS.</span></span> <span data-ttu-id="9eadc-295">Pokud SAS je předán přes protokol HTTP a zachycení, je možné číst SAS a použít jej jako určený uživatel může mít potenciálně ohrožení citlivých dat nebo aby vám umožnil poškození dat podle uživateli se zlými úmysly útočník provádění útok man-in-the-middle.</span><span class="sxs-lookup"><span data-stu-id="9eadc-295">If a SAS is passed over HTTP and intercepted, an attacker performing a man-in-the-middle attack is able to read the SAS and then use it just as the intended user could have, potentially compromising sensitive data or allowing for data corruption by the malicious user.</span></span>
2. <span data-ttu-id="9eadc-296">**Odkaz na uložený zásady přístupu, kde je to možné.**</span><span class="sxs-lookup"><span data-stu-id="9eadc-296">**Reference stored access policies where possible.**</span></span> <span data-ttu-id="9eadc-297">Zásady přístupu uložené získáte možnost odvolat oprávnění bez nutnosti obnovit klíče účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="9eadc-297">Stored access policies give you the option to revoke permissions without having to regenerate the storage account keys.</span></span> <span data-ttu-id="9eadc-298">Nastavit dobu platnosti na tyto velmi daleké budoucnosti (nebo nekonečné) a ujistěte se, že se pravidelně aktualizují přesune dále do budoucna.</span><span class="sxs-lookup"><span data-stu-id="9eadc-298">Set the expiration on these very far in the future (or infinite) and make sure it's regularly updated to move it farther into the future.</span></span>
3. <span data-ttu-id="9eadc-299">**Použijte ucelené časy vypršení platnosti na ad hoc SAS.**</span><span class="sxs-lookup"><span data-stu-id="9eadc-299">**Use near-term expiration times on an ad hoc SAS.**</span></span> <span data-ttu-id="9eadc-300">Tímto způsobem i když je ohrožení SAS, je platná pouze pro po krátkou dobu.</span><span class="sxs-lookup"><span data-stu-id="9eadc-300">In this way, even if a SAS is compromised, it's valid only for a short time.</span></span> <span data-ttu-id="9eadc-301">Tento postup je zvlášť důležité, pokud nemůže odkazovat zásadu uložené přístupu.</span><span class="sxs-lookup"><span data-stu-id="9eadc-301">This practice is especially important if you cannot reference a stored access policy.</span></span> <span data-ttu-id="9eadc-302">Ucelené časy vypršení platnosti také omezit množství dat, který lze zapisovat do objektu blob omezením čas nahrát do něj k dispozici.</span><span class="sxs-lookup"><span data-stu-id="9eadc-302">Near-term expiration times also limit the amount of data that can be written to a blob by limiting the time available to upload to it.</span></span>
4. <span data-ttu-id="9eadc-303">**Mají klienti automaticky obnovte SAS v případě potřeby.**</span><span class="sxs-lookup"><span data-stu-id="9eadc-303">**Have clients automatically renew the SAS if necessary.**</span></span> <span data-ttu-id="9eadc-304">Klienti musí obnovit SAS dobře před vypršením platnosti, aby bylo možné dobu opakovaných pokusů, pokud není k dispozici služba poskytující SAS.</span><span class="sxs-lookup"><span data-stu-id="9eadc-304">Clients should renew the SAS well before the expiration, in order to allow time for retries if the service providing the SAS is unavailable.</span></span> <span data-ttu-id="9eadc-305">Pokud vaše SAS je určen pro použití pro malý počet okamžitou, krátkodobou operace, které se očekává, že být dokončeny v rámci dobu platnosti, pak to může být zbytečné, není-li třeba obnovit očekávaným SAS.</span><span class="sxs-lookup"><span data-stu-id="9eadc-305">If your SAS is meant to be used for a small number of immediate, short-lived operations that are expected to be completed within the expiration period, then this may be unnecessary as the SAS is not expected to be renewed.</span></span> <span data-ttu-id="9eadc-306">Ale pokud máte klienta, který je pravidelně zajistit požadavky přes SAS, pak možnost vypršení platnosti dodává do play.</span><span class="sxs-lookup"><span data-stu-id="9eadc-306">However, if you have client that is routinely making requests via SAS, then the possibility of expiration comes into play.</span></span> <span data-ttu-id="9eadc-307">Klíče význam je vyvážit potřebu SAS být krátkodobou (jako výše uvedená) s potřeba zajistit, že klient požaduje obnovení již v rané fázi dostatek (aby se zabránilo přerušení z důvodu vypršení platnosti před úspěšné obnovení sdíleného přístupového podpisu).</span><span class="sxs-lookup"><span data-stu-id="9eadc-307">The key consideration is to balance the need for the SAS to be short-lived (as previously stated) with the need to ensure that the client is requesting renewal early enough (to avoid disruption due to the SAS expiring prior to successful renewal).</span></span>
5. <span data-ttu-id="9eadc-308">**Pečlivě se časem spuštění SAS.**</span><span class="sxs-lookup"><span data-stu-id="9eadc-308">**Be careful with SAS start time.**</span></span> <span data-ttu-id="9eadc-309">Pokud nastavíte čas zahájení pro SAS k **nyní**, z důvodu hodin zkreslit (rozdíly v aktuální čas podle různých počítačů), selhání může být dodržen občas u prvních několika minut.</span><span class="sxs-lookup"><span data-stu-id="9eadc-309">If you set the start time for a SAS to **now**, then due to clock skew (differences in current time according to different machines), failures may be observed intermittently for the first few minutes.</span></span> <span data-ttu-id="9eadc-310">Obecně platí nastavte výchozí čas být alespoň 15 minut v minulosti.</span><span class="sxs-lookup"><span data-stu-id="9eadc-310">In general, set the start time to be at least 15 minutes in the past.</span></span> <span data-ttu-id="9eadc-311">Nebo si ho nastavit vůbec, což znamená, že platný okamžitě ve všech případech.</span><span class="sxs-lookup"><span data-stu-id="9eadc-311">Or, don't set it at all, which will make it valid immediately in all cases.</span></span> <span data-ttu-id="9eadc-312">Obecně platí i pro také – čas vypršení platnosti mějte na paměti, můžete pozorovat až 15 minut od času zkosení v obou směrech na žádnou žádostí.</span><span class="sxs-lookup"><span data-stu-id="9eadc-312">The same generally applies to expiry time as well--remember that you may observe up to 15 minutes of clock skew in either direction on any request.</span></span> <span data-ttu-id="9eadc-313">Pro klienty pomocí verze REST před 2012-02-12 je maximální doba trvání SAS, která neodkazuje na zásadu uložené přístupu 1 hodinu a všechny zásady zadání dlouhodobější než, se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="9eadc-313">For clients using a REST version prior to 2012-02-12, the maximum duration for a SAS that does not reference a stored access policy is 1 hour, and any policies specifying longer term than that will fail.</span></span>
6. <span data-ttu-id="9eadc-314">**Buďte konkrétní prostředek nelze přistupovat.**</span><span class="sxs-lookup"><span data-stu-id="9eadc-314">**Be specific with the resource to be accessed.**</span></span> <span data-ttu-id="9eadc-315">Nejlepším postupem zabezpečení je poskytnout minimální požadovaná oprávnění uživatele.</span><span class="sxs-lookup"><span data-stu-id="9eadc-315">A security best practice is to provide a user with the minimum required privileges.</span></span> <span data-ttu-id="9eadc-316">Když uživatel potřebuje pouze přístup pro čtení k jedné entity, pak jim udělte přístup pro čtení k této jedné entity a ne pro čtení, zápisu a odstraňování přístup k všechny entity.</span><span class="sxs-lookup"><span data-stu-id="9eadc-316">If a user only needs read access to a single entity, then grant them read access to that single entity, and not read/write/delete access to all entities.</span></span> <span data-ttu-id="9eadc-317">Navíc pomáhají zmenšit prostor škody, pokud SAS dojde k ohrožení bezpečnosti vzhledem k tomu SAS má méně do nesprávných rukou útočníka.</span><span class="sxs-lookup"><span data-stu-id="9eadc-317">This also helps lessen the damage if a SAS is compromised because the SAS has less power in the hands of an attacker.</span></span>
7. <span data-ttu-id="9eadc-318">**Pochopte, že váš účet bude účtován na jakékoli využití, včetně udělat pomocí SAS.**</span><span class="sxs-lookup"><span data-stu-id="9eadc-318">**Understand that your account will be billed for any usage, including that done with SAS.**</span></span> <span data-ttu-id="9eadc-319">Pokud zadáte oprávnění k zápisu do objektu blob, uživatel může zvolit nahrát objekt blob 200GB.</span><span class="sxs-lookup"><span data-stu-id="9eadc-319">If you provide write access to a blob, a user may choose to upload a 200GB blob.</span></span> <span data-ttu-id="9eadc-320">Pokud jste dali je také přístup pro čtení, se rozhodnout ho stáhnout 10krát, by docházelo 2 TB v náklady na celkový výstup za vás.</span><span class="sxs-lookup"><span data-stu-id="9eadc-320">If you've given them read access as well, they may choose to download it 10 times, incurring 2 TB in egress costs for you.</span></span> <span data-ttu-id="9eadc-321">Znovu zadejte omezenými oprávněními pro zmírnění potenciální akce uživatelé se zlými úmysly.</span><span class="sxs-lookup"><span data-stu-id="9eadc-321">Again, provide limited permissions to help mitigate the potential actions of malicious users.</span></span> <span data-ttu-id="9eadc-322">Pomocí krátkodobou SAS snížit této hrozby (ale být s vědomím zkosení podél koncový čas hodiny).</span><span class="sxs-lookup"><span data-stu-id="9eadc-322">Use short-lived SAS to reduce this threat (but be mindful of clock skew on the end time).</span></span>
8. <span data-ttu-id="9eadc-323">**Ověření dat zapsaných pomocí SAS.**</span><span class="sxs-lookup"><span data-stu-id="9eadc-323">**Validate data written using SAS.**</span></span> <span data-ttu-id="9eadc-324">Když klientské aplikace zapisuje data do účtu úložiště, mějte na paměti, že mohou být problémy s daty.</span><span class="sxs-lookup"><span data-stu-id="9eadc-324">When a client application writes data to your storage account, keep in mind that there can be problems with that data.</span></span> <span data-ttu-id="9eadc-325">Pokud vaše aplikace vyžaduje, aby se tato data ověřit nebo oprávnění předtím, než je připravený k použití, byste měli provést toto ověření po zapsání dat, a než bude použit v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9eadc-325">If your application requires that that data be validated or authorized before it is ready to use, you should perform this validation after the data is written and before it is used by your application.</span></span> <span data-ttu-id="9eadc-326">Tento postup také chrání před poškozený nebo škodlivý dat, zapisuje do vašeho účtu, uživatel, který správně získali SAS nebo uživatelem zneužitím uniklé SAS.</span><span class="sxs-lookup"><span data-stu-id="9eadc-326">This practice also protects against corrupt or malicious data being written to your account, either by a user who properly acquired the SAS, or by a user exploiting a leaked SAS.</span></span>
9. <span data-ttu-id="9eadc-327">**Nepoužívejte vždy SAS.**</span><span class="sxs-lookup"><span data-stu-id="9eadc-327">**Don't always use SAS.**</span></span> <span data-ttu-id="9eadc-328">Někdy rizika spojená s konkrétní operaci u vašeho účtu úložiště převažují nad přínosy SAS.</span><span class="sxs-lookup"><span data-stu-id="9eadc-328">Sometimes the risks associated with a particular operation against your storage account outweigh the benefits of SAS.</span></span> <span data-ttu-id="9eadc-329">Pro tyto operace vytvoření služby střední vrstvy, který zapisuje do svého účtu úložiště po provedení obchodní pravidla ověřování, ověřování a auditování.</span><span class="sxs-lookup"><span data-stu-id="9eadc-329">For such operations, create a middle-tier service that writes to your storage account after performing business rule validation, authentication, and auditing.</span></span> <span data-ttu-id="9eadc-330">Někdy je také jednodušší ke správě přístupu k jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="9eadc-330">Also, sometimes it's simpler to manage access in other ways.</span></span> <span data-ttu-id="9eadc-331">Například, pokud chcete, aby všechny objekty BLOB v kontejneru veřejně čitelné, když vytváříte kontejneru Public, místo pro přístup k poskytování SAS každého klienta.</span><span class="sxs-lookup"><span data-stu-id="9eadc-331">For example, if you want to make all blobs in a container publically readable, you can make the container Public, rather than providing a SAS to every client for access.</span></span>
10. <span data-ttu-id="9eadc-332">**Použijte analytika úložiště do monitorování vaší aplikace.**</span><span class="sxs-lookup"><span data-stu-id="9eadc-332">**Use Storage Analytics to monitor your application.**</span></span> <span data-ttu-id="9eadc-333">Pomocí protokolování a metriky můžete sledovat všechny Špička při selhání ověřování kvůli výpadku ve službě zprostředkovatele SAS nebo nechtěnému odstranění zásady uložené přístupu.</span><span class="sxs-lookup"><span data-stu-id="9eadc-333">You can use logging and metrics to observe any spike in authentication failures due to an outage in your SAS provider service or to the inadvertent removal of a stored access policy.</span></span> <span data-ttu-id="9eadc-334">Najdete v článku [Blog týmu Azure Storage](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/08/03/windows-azure-storage-logging-using-logs-to-track-storage-requests.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="9eadc-334">See the [Azure Storage Team Blog](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/08/03/windows-azure-storage-logging-using-logs-to-track-storage-requests.aspx) for additional information.</span></span>

## <a name="sas-examples"></a><span data-ttu-id="9eadc-335">Příklady SAS</span><span class="sxs-lookup"><span data-stu-id="9eadc-335">SAS examples</span></span>
<span data-ttu-id="9eadc-336">Zde jsou některé příklady oba typy sdílené přístupové podpisy SAS účtu a SAS služby.</span><span class="sxs-lookup"><span data-stu-id="9eadc-336">Below are some examples of both types of shared access signatures, account SAS and service SAS.</span></span>

<span data-ttu-id="9eadc-337">Pokud chcete spustit tyto příklady C#, budete muset odkazují následující balíčky NuGet ve vašem projektu:</span><span class="sxs-lookup"><span data-stu-id="9eadc-337">To run these C# examples, you need to reference the following NuGet packages in your project:</span></span>

* <span data-ttu-id="9eadc-338">[Azure Storage Client Library for .NET](http://www.nuget.org/packages/WindowsAzure.Storage), verze 6.x nebo novější (pro použití účtu SAS).</span><span class="sxs-lookup"><span data-stu-id="9eadc-338">[Azure Storage Client Library for .NET](http://www.nuget.org/packages/WindowsAzure.Storage), version 6.x or later (to use account SAS).</span></span>
* [<span data-ttu-id="9eadc-339">Azure Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="9eadc-339">Azure Configuration Manager</span></span>](http://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager)

<span data-ttu-id="9eadc-340">Další příklady, které ukazují, jak vytvořit a otestovat SAS naleznete v tématu [ukázky kódu Azure pro úložiště](https://azure.microsoft.com/documentation/samples/?service=storage).</span><span class="sxs-lookup"><span data-stu-id="9eadc-340">For additional examples that show how to create and test a SAS, see [Azure Code Samples for Storage](https://azure.microsoft.com/documentation/samples/?service=storage).</span></span>

### <a name="example-create-and-use-an-account-sas"></a><span data-ttu-id="9eadc-341">Příklad: Vytvoření a použití SAS účtu</span><span class="sxs-lookup"><span data-stu-id="9eadc-341">Example: Create and use an account SAS</span></span>
<span data-ttu-id="9eadc-342">Následující příklad kódu vytvoří účet SAS, který je platný pro služby objektů Blob a soubor a klient získá oprávnění pro čtení, zápisu a seznam oprávnění k přístupu k rozhraním API úrovně služeb.</span><span class="sxs-lookup"><span data-stu-id="9eadc-342">The following code example creates an account SAS that is valid for the Blob and File services, and gives the client permissions read, write, and list permissions to access service-level APIs.</span></span> <span data-ttu-id="9eadc-343">SAS účtu omezuje protokol HTTPS, musí žádosti s protokolem HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9eadc-343">The account SAS restricts the protocol to HTTPS, so the request must be made with HTTPS.</span></span>

```csharp
static string GetAccountSASToken()
{
    // To create the account SAS, you need to use your shared key credentials. Modify for your account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

    // Create a new access policy for the account.
    SharedAccessAccountPolicy policy = new SharedAccessAccountPolicy()
        {
            Permissions = SharedAccessAccountPermissions.Read | SharedAccessAccountPermissions.Write | SharedAccessAccountPermissions.List,
            Services = SharedAccessAccountServices.Blob | SharedAccessAccountServices.File,
            ResourceTypes = SharedAccessAccountResourceTypes.Service,
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Protocols = SharedAccessProtocol.HttpsOnly
        };

    // Return the SAS token.
    return storageAccount.GetSharedAccessSignature(policy);
}
```

<span data-ttu-id="9eadc-344">Použití SAS účtu pro přístup k rozhraní API pro úroveň služby pro službu Blob, vytvořte objekt Blob klienta pomocí SAS a koncový bod úložiště objektů Blob pro váš účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="9eadc-344">To use the account SAS to access service-level APIs for the Blob service, construct a Blob client object using the SAS and the Blob storage endpoint for your storage account.</span></span>

```csharp
static void UseAccountSAS(string sasToken)
{
    // Create new storage credentials using the SAS token.
    StorageCredentials accountSAS = new StorageCredentials(sasToken);
    // Use these credentials and the account name to create a Blob service client.
    CloudStorageAccount accountWithSAS = new CloudStorageAccount(accountSAS, "account-name", endpointSuffix: null, useHttps: true);
    CloudBlobClient blobClientWithSAS = accountWithSAS.CreateCloudBlobClient();

    // Now set the service properties for the Blob client created with the SAS.
    blobClientWithSAS.SetServiceProperties(new ServiceProperties()
    {
        HourMetrics = new MetricsProperties()
        {
            MetricsLevel = MetricsLevel.ServiceAndApi,
            RetentionDays = 7,
            Version = "1.0"
        },
        MinuteMetrics = new MetricsProperties()
        {
            MetricsLevel = MetricsLevel.ServiceAndApi,
            RetentionDays = 7,
            Version = "1.0"
        },
        Logging = new LoggingProperties()
        {
            LoggingOperations = LoggingOperations.All,
            RetentionDays = 14,
            Version = "1.0"
        }
    });

    // The permissions granted by the account SAS also permit you to retrieve service properties.
    ServiceProperties serviceProperties = blobClientWithSAS.GetServiceProperties();
    Console.WriteLine(serviceProperties.HourMetrics.MetricsLevel);
    Console.WriteLine(serviceProperties.HourMetrics.RetentionDays);
    Console.WriteLine(serviceProperties.HourMetrics.Version);
}
```

### <a name="example-create-a-stored-access-policy"></a><span data-ttu-id="9eadc-345">Příklad: Vytvoření zásady uložené přístupu</span><span class="sxs-lookup"><span data-stu-id="9eadc-345">Example: Create a stored access policy</span></span>
<span data-ttu-id="9eadc-346">Následující kód vytvoří zásadu uložené přístup do kontejneru.</span><span class="sxs-lookup"><span data-stu-id="9eadc-346">The following code creates a stored access policy on a container.</span></span> <span data-ttu-id="9eadc-347">Zásady přístupu můžete použít k určení omezení pro SAS služby na kontejneru nebo jeho objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="9eadc-347">You can use the access policy to specify constraints for a service SAS on the container or its blobs.</span></span>

```csharp
private static async Task CreateSharedAccessPolicyAsync(CloudBlobContainer container, string policyName)
{
    // Create a new shared access policy and define its constraints.
    // The access policy provides create, write, read, list, and delete permissions.
    SharedAccessBlobPolicy sharedPolicy = new SharedAccessBlobPolicy()
    {
        // When the start time for the SAS is omitted, the start time is assumed to be the time when the storage service receives the request.
        // Omitting the start time for a SAS that is effective immediately helps to avoid clock skew.
        SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
        Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.List |
            SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Create | SharedAccessBlobPermissions.Delete
    };

    // Get the container's existing permissions.
    BlobContainerPermissions permissions = await container.GetPermissionsAsync();

    // Add the new policy to the container's permissions, and set the container's permissions.
    permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
    await container.SetPermissionsAsync(permissions);
}
```

### <a name="example-create-a-service-sas-on-a-container"></a><span data-ttu-id="9eadc-348">Příklad: Vytvoření SAS služby na kontejneru</span><span class="sxs-lookup"><span data-stu-id="9eadc-348">Example: Create a service SAS on a container</span></span>
<span data-ttu-id="9eadc-349">Následující kód vytvoří SAS do kontejneru.</span><span class="sxs-lookup"><span data-stu-id="9eadc-349">The following code creates a SAS on a container.</span></span> <span data-ttu-id="9eadc-350">Pokud je zadaný název existující zásady uložené přístup, je přidružen SAS tuto zásadu.</span><span class="sxs-lookup"><span data-stu-id="9eadc-350">If the name of an existing stored access policy is provided, that policy is associated with the SAS.</span></span> <span data-ttu-id="9eadc-351">Pokud je k dispozici žádné uložené přístup zásady, kód vytvoří SAS ad-hoc v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="9eadc-351">If no stored access policy is provided, then the code creates an ad-hoc SAS on the container.</span></span>

```csharp
private static string GetContainerSasUri(CloudBlobContainer container, string storedPolicyName = null)
{
    string sasContainerToken;

    // If no stored policy is specified, create a new access policy and define its constraints.
    if (storedPolicyName == null)
    {
        // Note that the SharedAccessBlobPolicy class is used both to define the parameters of an ad-hoc SAS, and
        // to construct a shared access policy that is saved to the container's shared access policies.
        SharedAccessBlobPolicy adHocPolicy = new SharedAccessBlobPolicy()
        {
            // When the start time for the SAS is omitted, the start time is assumed to be the time when the storage service receives the request.
            // Omitting the start time for a SAS that is effective immediately helps to avoid clock skew.
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List
        };

        // Generate the shared access signature on the container, setting the constraints directly on the signature.
        sasContainerToken = container.GetSharedAccessSignature(adHocPolicy, null);

        Console.WriteLine("SAS for blob container (ad hoc): {0}", sasContainerToken);
        Console.WriteLine();
    }
    else
    {
        // Generate the shared access signature on the container. In this case, all of the constraints for the
        // shared access signature are specified on the stored access policy, which is provided by name.
        // It is also possible to specify some constraints on an ad-hoc SAS and others on the stored access policy.
        sasContainerToken = container.GetSharedAccessSignature(null, storedPolicyName);

        Console.WriteLine("SAS for blob container (stored access policy): {0}", sasContainerToken);
        Console.WriteLine();
    }

    // Return the URI string for the container, including the SAS token.
    return container.Uri + sasContainerToken;
}
```

### <a name="example-create-a-service-sas-on-a-blob"></a><span data-ttu-id="9eadc-352">Příklad: Vytvoření SAS služby na objekt blob</span><span class="sxs-lookup"><span data-stu-id="9eadc-352">Example: Create a service SAS on a blob</span></span>
<span data-ttu-id="9eadc-353">Následující kód vytvoří SAS na objekt blob.</span><span class="sxs-lookup"><span data-stu-id="9eadc-353">The following code creates a SAS on a blob.</span></span> <span data-ttu-id="9eadc-354">Pokud je zadaný název existující zásady uložené přístup, je přidružen SAS tuto zásadu.</span><span class="sxs-lookup"><span data-stu-id="9eadc-354">If the name of an existing stored access policy is provided, that policy is associated with the SAS.</span></span> <span data-ttu-id="9eadc-355">Pokud je k dispozici žádné uložené přístup zásady, kód vytvoří SAS ad-hoc u objektu blob.</span><span class="sxs-lookup"><span data-stu-id="9eadc-355">If no stored access policy is provided, then the code creates an ad-hoc SAS on the blob.</span></span>

```csharp
private static string GetBlobSasUri(CloudBlobContainer container, string blobName, string policyName = null)
{
    string sasBlobToken;

    // Get a reference to a blob within the container.
    // Note that the blob may not exist yet, but a SAS can still be created for it.
    CloudBlockBlob blob = container.GetBlockBlobReference(blobName);

    if (policyName == null)
    {
        // Create a new access policy and define its constraints.
        // Note that the SharedAccessBlobPolicy class is used both to define the parameters of an ad-hoc SAS, and
        // to construct a shared access policy that is saved to the container's shared access policies.
        SharedAccessBlobPolicy adHocSAS = new SharedAccessBlobPolicy()
        {
            // When the start time for the SAS is omitted, the start time is assumed to be the time when the storage service receives the request.
            // Omitting the start time for a SAS that is effective immediately helps to avoid clock skew.
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Create
        };

        // Generate the shared access signature on the blob, setting the constraints directly on the signature.
        sasBlobToken = blob.GetSharedAccessSignature(adHocSAS);

        Console.WriteLine("SAS for blob (ad hoc): {0}", sasBlobToken);
        Console.WriteLine();
    }
    else
    {
        // Generate the shared access signature on the blob. In this case, all of the constraints for the
        // shared access signature are specified on the container's stored access policy.
        sasBlobToken = blob.GetSharedAccessSignature(null, policyName);

        Console.WriteLine("SAS for blob (stored access policy): {0}", sasBlobToken);
        Console.WriteLine();
    }

    // Return the URI string for the container, including the SAS token.
    return blob.Uri + sasBlobToken;
}
```

## <a name="conclusion"></a><span data-ttu-id="9eadc-356">Závěr</span><span class="sxs-lookup"><span data-stu-id="9eadc-356">Conclusion</span></span>
<span data-ttu-id="9eadc-357">Sdílené přístupové podpisy jsou užitečné pro zajištění omezené oprávnění k účtu úložiště na klienty, kteří by neměl mít klíč účtu.</span><span class="sxs-lookup"><span data-stu-id="9eadc-357">Shared access signatures are useful for providing limited permissions to your storage account to clients that should not have the account key.</span></span> <span data-ttu-id="9eadc-358">Jako takový jsou sice podstatná součást model zabezpečení pro všechny aplikace pomocí Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="9eadc-358">As such, they are a vital part of the security model for any application using Azure Storage.</span></span> <span data-ttu-id="9eadc-359">Pokud budete postupovat podle osvědčených postupů, které jsou zde uvedeny, můžete zajistit větší flexibilita při přístupu k prostředkům ve vašem účtu úložiště, aniž by to ohrozilo zabezpečení aplikace SAS.</span><span class="sxs-lookup"><span data-stu-id="9eadc-359">If you follow the best practices listed here, you can use SAS to provide greater flexibility of access to resources in your storage account, without compromising the security of your application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9eadc-360">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9eadc-360">Next Steps</span></span>
* [<span data-ttu-id="9eadc-361">Správa anonymního přístupu pro čtení ke kontejnerům a objektům BLOB</span><span class="sxs-lookup"><span data-stu-id="9eadc-361">Manage anonymous read access to containers and blobs</span></span>](storage-manage-access-to-resources.md)
* [<span data-ttu-id="9eadc-362">Delegování přístupu se sdíleným přístupovým podpisem</span><span class="sxs-lookup"><span data-stu-id="9eadc-362">Delegating Access with a Shared Access Signature</span></span>](http://msdn.microsoft.com/library/azure/ee395415.aspx)
* [<span data-ttu-id="9eadc-363">Představení tabulky a fronty SAS</span><span class="sxs-lookup"><span data-stu-id="9eadc-363">Introducing Table and Queue SAS</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx)

[sas-storage-fe-proxy-service]: ./media/storage-dotnet-shared-access-signature-part-1/sas-storage-fe-proxy-service.png
[sas-storage-provider-service]: ./media/storage-dotnet-shared-access-signature-part-1/sas-storage-provider-service.png
[sas-storage-uri]: ./media/storage-dotnet-shared-access-signature-part-1/sas-storage-uri.png
